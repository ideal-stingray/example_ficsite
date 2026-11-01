+++
title = "example work 1"
date = 2026-11-01
lastmod = 2026-11-01
draft = false
fandoms = ["my_awesome_fandom"]
series = ["series_1"]
series_weight = 1
tags = ["weesnaw-pebbit", "college au", "complete"]
+++

> This is the summary of my example work.  It's in a quote block.
>
>> I can also do nested quote blocks.
>>
>>> I can nest them as deep as I want.

<details>
<summary>author's note</summary>
Here is my author's note.  It's in a HTML details block.
</details>

Fic goes here!  Make sure to set `draft = false` in the frontmatter, or it won't show up.

This fic is in a series, `"series_1"`.  The `series_weight` setting in the frontmatter tells Hugo what position to put it in on the series page.  If you don't set `series_weight`, Hugo will sort by date.

Here's a Soundcloud embed:
{{< soundcloud-track 253508261 >}}

Here's a Spotify embed:
{{< spotify-track 4PTG3Z6ehGkBFwjybzWkR8 >}}

Here's a Tidal embed:
{{<tidal-track 491206012 >}}

<details>
<summary>author's note</summary>
I'll also put an author's note at the end.
</details>
