+++
author = "Jonathan Van Eenwyk"
date = "2020-12-13"
title = "MacPass and Have I Been Pwned"
description = "How I checked my passwords stored in MacPass against the Have I Been Pwned database"
type = "post"
+++

[Have I Been Pwned](https://haveibeenpwned.com/) hosts a database of passwords
that have been compromised by website breaches. With this data, it's possible to
make sure that we're not continuing to use compromised passwords, putting
ourselves at unnecessary risk. Chrome, for example, does this automatically
presumably using this or a similar database. [KeePass](https://keepass.info/)
has a [plugin](https://github.com/mihaifm/HIBPOfflineCheck) for this.
Unfortunately, no such plugin is available for
[MacPass](https://macpassapp.org/). This post documents how I managed to check
my passwords without resorting to KeePass.

## Prepare the HIBP database
1. Download the Have I Been Pwned (HIBP) password
   [database](https://haveibeenpwned.com/Passwords). I picked version 7 of the
   SHA-1 file ordered by hash (dba43bd82997d5cef156219cb0d295e1ab948727).

2. Clone the [pwnedpass](https://github.com/tylerchr/pwnedpass) project.

3. From the `pwnedpass` directory, convert the password database to a searchable
   format:

```
7z e -so pwned-passwords-sha1-ordered-by-hash-v7.7z pwned-passwords-sha1-ordered-by-hash-v7.txt | go run cmd/pwngen/main.go pwned-passwords.bin
```

4. Launch `pwnd` to enable a local HTTP server mimicking the [HIBP api](https://haveibeenpwned.com/API/v2):

```
go run cmd/pwnd/read.go
```

## Export passwords
1. Export passwords from MacPass in xml format to a file named `all.xml`.

2. Use [xmlstarlet](https://formulae.brew.sh/formula/xmlstarlet) to trim out
   historical passwords which no longer matter:

```
xmlstarlet ed -d '//Entry/History' all.xml > current.xml
```

3. Use xmlstarlet to extract out the passwords into a flat text file:

```
xmlstarlet sel -T -t -m '//Entry[String[Key/text() = "Password"]/Value != ""]' -v 'String[Key/text() = "Password"]/Value' -n current.xml > passwords.txt
```

## Check passwords
1. Clone the [haveibeenpwned](https://github.com/MrBlaise/haveibeenpwned/)
   project.

2. Edit `havei.sh` to point to our local server:

```
sed -i '' 's/https:\/\/api.pwnedpasswords.com/http:\/\/localhost:8889/' havei.sh
```

3. Check all passwords against the HIBP database:

```
./havei.sh --file=passwords.txt -d=0 --plain
```

