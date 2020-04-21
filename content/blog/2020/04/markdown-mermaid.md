+++
author = "Jonathan Van Eenwyk"
date = "2020-04-21"
title = "Writing a business plan with markdown and mermaid"
description = "I love writing when I can focus on the task and not get bogged down in formatting."
type = "post"

+++

I love writing when I can focus on the task and not get bogged down in formatting.  That's why, over the last few years, I've gradually converted important old documents and written most new documents in
[Markdown](https://www.markdownguide.org/basic-syntax/) format.  I primarily use [Typora](https://typora.io/) which is a fantastic editor that emphasizes keeping focus on the writing experience.

Lately, I've been busily writing a business plan to forecast the next few years. Turns out, I can do far more than I imagined by leveraging Typora's support for [Mermaid](https://mermaidjs.github.io/), a human-readable format for describing flowcharts and diagrams.  Building a forecast gantt chart was a breeze:

![Gantt Chart with Mermaid](/img/2020/04/mermaid-gantt.png)

Turns out, Mermaid supports [milestones](https://github.com/mermaid-js/mermaid/pull/788) too, but it's not currently documented.  After digging around a bit, I figured it out and it's actually remarkably easy:

![image-20200421094501358](/img/2020/04/mermaid-milestones.png)

```
gantt
  dateFormat YYYY-MM
  axisFormat %b %Y
  
  Networking : 2021-01, 2021-11
  Join business group : 2021-06, 2022-06
  First hiring target :milestone, 2021-06, 2021-06
  Second hiring target :milestone, 2022-06, 2022-06
  Reach first revenue target :milestone, 2022-01, 2022-01
  Reach second revenue target :milestone, 2023-01, 2023-01
```

Just adding `:milestone` was all that's needed.  The milestone appears halfway between the start and end dates, so you need to specific both dates unless you want to calculate the dates based on other markers. Pretty slick!

It was a little harder to build a normal-looking organizational chart, but I was still able to build something that's good enough:

![image-20200421093905193](/img/2020/04/mermaid-orgchart.png)

Yeah, it's not oriented top-to-bottom, but that's primarily because mermaid doesn't appear to have any advanced layout options and I have a lot of depth in my org chart.