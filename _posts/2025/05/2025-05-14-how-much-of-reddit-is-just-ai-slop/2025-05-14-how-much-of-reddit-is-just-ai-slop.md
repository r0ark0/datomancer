---
layout: post

title: "How Much of Reddit Is Just AI Slop?"
image: /assets/images/posts/2025/05/2025-05-14-how-much-of-reddit-is-just-ai-slop/featured.png
date: 2025-05-14
category: Technology
tags: [internet, ai, reddit]
---

It turns out 23% of posts on [r/AmItheAsshole](https://www.reddit.com/r/AmItheAsshole/) are (potentially) written by AI. But before we freak out and lose our minds, let's break this down a bit.

<div id="aita-ai-vs-human" style="width:100%; height:auto; margin:auto;" class="mb-3"></div>

{% raw %}
<style>
  #aita-ai-vs-human svg { width:100%; height:100%; }
  .slice-path   { stroke:#fff; stroke-width:1px; }
  .slice-image  { opacity:0.15; }
  .ai-label     { fill:#fff; font-size:18px; font-weight:bold; text-anchor:middle; }
  .human-label  { fill:#c1c1c1; font-size:10px; text-anchor:middle; }
</style>

<script src="https://d3js.org/d3.v7.min.js"></script>
<script>
document.addEventListener("DOMContentLoaded", () => {
  // ---- raw AITA counts ----
  const raw = [
    { label: "Very Likely AI",    count:   1 },
    { label: "Likely AI",          count:  45 },
    { label: "Needs Review",       count: 188 },
    { label: "Probably Human",     count: 187 },
    { label: "Very Likely Human",  count: 575 }
  ];

  // ---- treat “Needs Review” as suspected AI ----
  const aiCount    = raw
    .filter(d => d.label === "Needs Review" || d.label.includes("AI"))
    .reduce((sum,d) => sum + d.count, 0);
  const totalCount = raw.reduce((sum,d) => sum + d.count, 0);

  const data = [
    { key: "Suspected AI", value: aiCount,
      img: "https://images.prismic.io/star-trek-untold/ZWM3MjcyNmUtYTE4MC00ZjkyLWJlZjYtOWUwMjBjYmYyMjBl_d8cdc92148d497bba4ac7e4f4fd5b1b5.jpgitoklh-hofva?auto=compress,format&rect=0,0,550,413&w=550&h=413"
    },
    { key: "Human", value: totalCount - aiCount,
      img: "https://i.redd.it/09h00zjesvt61.jpg"
    }
  ];

  // ---- dimensions ----
  const width  = 400,
        height = 400,
        radius = Math.min(width, height) / 2 - 10;

  const svg = d3.select("#aita-ai-vs-human")
    .append("svg")
      .attr("viewBox", `0 0 ${width} ${height}`)
    .append("g")
      .attr("transform", `translate(${width/2},${height/2})`);

  // ---- pie & arc ----
  const pie = d3.pie().sort(null).value(d => d.value);
  const arc = d3.arc().innerRadius(0).outerRadius(radius);

  // ---- fallback colored slices ----
  const color = d3.scaleOrdinal()
    .domain(data.map(d=>d.key))
    .range(["#FF4500","#A6A6A6"]);

  const sliceData = pie(data);

  svg.selectAll("path.slice-path")
    .data(sliceData)
    .join("path")
      .attr("class","slice-path")
      .attr("d", arc)
      .attr("fill", d => color(d.data.key));

  // ---- clipPaths ----
  const defs = svg.append("defs");
  sliceData.forEach(d => {
    defs.append("clipPath")
      .attr("id", `clip-${d.data.key.replace(/\s/g,'')}`)
      .append("path")
        .attr("d", arc(d));
  });

  // ---- images clipped to slice shape ----
  svg.selectAll("image.slice-image")
    .data(sliceData)
    .join("image")
      .attr("class","slice-image")
      .attr("href", d => d.data.img)
      .attr("width", radius*2)
      .attr("height", radius*2)
      .attr("x", -radius)
      // push AI image up a bit so the face is visible
      .attr("y", d => d.data.key==="Suspected AI" ? -radius*1.2 : -radius)
      .attr("preserveAspectRatio","xMidYMid slice")
      .attr("clip-path", d => `url(#clip-${d.data.key.replace(/\s/g,'')})`);

  // ---- labels ----
  // AI inside
  const aiSlice = sliceData.find(d => d.data.key === "Suspected AI");
  const [ax, ay] = arc.centroid(aiSlice);
  svg.append("text")
    .attr("class","ai-label")
    .attr("x", ax).attr("y", ay)
    .text(`${Math.round(aiCount/totalCount*100)}% Suspected AI`);

  // Human just outside
  const humSlice = sliceData.find(d => d.data.key === "Human");
  const [hx, hy]   = arc.centroid(humSlice);
  const offset     = (radius + 20) / radius;
  svg.append("text")
    .attr("class","human-label")
    .attr("x", hx * offset)
    .attr("y", hy * offset)
    .text(`${Math.round((1 - aiCount/totalCount)*100)}% Human`);
});
</script>
{% endraw %}

Bots on Reddit aren’t new. In fact, automated accounts and sockpuppets [have been part of the platform since its earliest days](https://www.vice.com/en/article/how-reddit-got-huge-tons-of-fake-accounts-2/#:~:text=Well%2C%20according%20to%20Reddit%20cofounder,spreading%20a%20blog%20post%20around.). But what is new, at least to many users, is the unsettling realization that they may no longer be interacting with real people at all. The comment that made you laugh, the argument that pulled you in, the post that made you feel heard. 

It might not have been written by a human... Social media is filled with [AI slop](https://en.wikipedia.org/wiki/AI_slop) these days.

Increasingly, Reddit is populated by bots, or at best, users engaging without genuine intent. Using AI to write for them and automate engagement, or putting low effort into writing posts themselves. They're just farming karma or mimicking real conversation. There's a great video from [Neon Sharpe](https://www.youtube.com/@NeonSharpe) on Youtube for how to spot these kinds reddit posts in the wild:

<div class="mb-5">
<iframe width="100%" height="315" src="https://www.youtube.com/embed/Tk3tSsNLBo4?si=uB2IbQyctfCY3fG7" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
</div>

This video inspired me to go a step further: what if we tried to detect these patterns programmatically? Could we quantify just how bot-saturated Reddit has become, and what kind of behavior do these bots (or AI written posts) actually exhibit?

## Quantifying Reddit's AI Slop Problem

There’s a whole taxonomy of bots operating on Reddit, as detailed in one of the more insightful breakdowns I’ve seen: [“How to Identify Bots on Reddit”](https://www.reddit.com/r/LearnUselessTalents/comments/15tzjkb/how_to_identify_bots_on_reddit/). It outlines a sprawling ecosystem, from karma-farming repost bots and bait-and-switch accounts to scam bots pushing knockoff merch in comment threads. These bots often betray themselves with small glitches—like turning an apostrophe (`'`) into the HTML character `&#39;` or using Reddit’s default gibberish usernames. 

Reddit even [encourages developers to build on their platform](https://developers.reddit.com/), to build games or useful, ethical bots. The [Reddit post about identifying bots](https://www.reddit.com/r/LearnUselessTalents/comments/15tzjkb/how_to_identify_bots_on_reddit/) makes a useful distinction between “ethical” bots, like AutoModerator, and those designed to blend in, accumulate karma, and eventually be sold for scams, propaganda, or influence operations.

But while most bots focus on reposting or reactive comments, I became interested in a subtler type: the self-post bot. These accounts write original-looking text posts in subreddits like [r/AmItheAsshole](https://www.reddit.com/r/AmItheAsshole/), mimicking human storytelling, often with surprisingly human results.

But why would anyone do this? 

We can’t say for sure, but there are several plausible motivations. Some bots may be farming karma to resell the account. Others might be part of AI testing pipelines, using Reddit for real-time feedback. In some cases, the goal could be to harvest comment data from human users for model fine-tuning. There’s also the possibility of content laundering or posting fake drama to later recycle into TikTok videos or SEO-friendly articles. It could just be trolling or social experimentation. We can't really say for sure.

Whatever the intent, the effect is the same: a sense that the stories we read and respond to might not be real. Which, as Neon Sharpe pointed out, is a huge bummer and a waste of real human potential.

## Building an AI Detector

I'm very interested in the [Dead Internet Theory](https://en.wikipedia.org/wiki/Dead_Internet_theory) but we'll get to that [later](#section-4-dead-internet-theory). Naturally when I saw there might be a good way to spot AI on Reddit, I thought this might be a fun experiment to try!

What if some of the most upvoted, emotionally resonant posts on Reddit weren’t written by people at all? Inspired by Neon Sharp’s excellent YouTube breakdown I tried my best at building a simple detection system in Python.

```python
# Code sample: Calling our function to evaluate a Reddit post url

def evaluate_url(url):
    jb = fetch_json(url)
    title, body, score = extract_title_body_score(jb)
    raw, cal, breakdown = ai_score(title, body)
    return {
        "url": url,
        "title": title,
        "score": score,  # upvotes
        "raw_score": raw,
        "ai_score": cal,
        "breakdown": breakdown
    }
```

Rather than rely on black-box AI detectors (which are notoriously bad), I built a lightweight, heuristic-based model in plain Python. No AI detection APIs, just some good old-fashioned pattern recognition with Regular expressions and NLTK. 

The detector assigns each Reddit post an "AI-likelihood" score between 0 and 1, based on ten measurable features pulled directly from Neon Sharp’s observations and my own gut checks.

Our measurable features include:

| Feature                            | What it captures                        | Weight |
| ---------------------------------- | --------------------------------------- | -----: |
| **Quote density (q)**              | AI’s over-quoting habit                 |   0.60 |
| **Dash usage (d)**                 | Em- and en-dash over-use                |   0.30 |
| **Odd Unicode (u)**                | Non-ASCII characters (accents, symbols) |   0.02 |
| **Spelling perfection (s)**        | Few or no typos                         |   0.02 |
| **Paragraph regularity (p)**       | Uniform sentence-count per paragraph    |   0.02 |
| **Emotion intensity (e)**          | Absolute VADER sentiment score          |   0.02 |
| **Sentence-length uniformity (l)** | Low variance in sentence lengths        |   0.02 |

By default, and you can see in my example below, ChatGPT tends to add em dashes and quotes to everything. It's a pretty good signal that the content was written by AI if you find an overuse of quotes, and em dashes.

Here's a breakdown for how calibrated AI scores map to each label we're using:

| Label             | Calibrated Score Range | Description                      |
| ----------------- | ---------------------: | -------------------------------- |
| Very Likely Human |            0.00 – 0.20 | Almost certainly human-written   |
| Probably Human    |            0.21 – 0.40 | Likely human, but some doubt     |
| Needs Review      |            0.41 – 0.60 | Borderline — manual check needed |
| Likely AI         |            0.61 – 0.80 | Probably AI-generated            |
| Very Likely AI    |            0.81 – 1.00 | Almost certainly AI-generated    |

To make interpretation easier, I normalized the scores: anything below 0.20 is flagged as "Very Likely Human," anything above 0.80 as “Very Likely AI,” and anything in between as "Likely AI", "Probably Human" or "Needs Review."

This isn’t a perfect system. But as of right now, [reliably distinguishing AI-generated text](https://arstechnica.com/information-technology/2023/07/openai-discontinues-its-ai-writing-detector-due-to-low-rate-of-accuracy/) from human writing [remains largely infeasible](https://www.insidehighered.com/news/tech-innovation/artificial-intelligence/2024/02/09/professors-proceed-caution-using-ai?utm_source=chatgpt.com). In fact these AI detection systems are very easy to fool. To test this, I wrote a fake /r/AmItheAsshole/ story (which I think is pretty good I might add) and ran it though 3 of these AI detection systems. As you can see the results are... bad.

Here's the fake story we came up with (take notice of all quotes and em dashes ChatGPT added in):

> AITA for telling my future MIL to stop "improving" our wedding and banning her from the final planning meetings?
>
> Hi Reddit,  
> I’m (28F) getting married to my amazing fiancé (29M) in three months. Everything was going great until his mom—let’s call her Karen—decided to become the self-appointed creative director of our wedding.
>
> To be clear, we never asked for her help. She offered to “help out a little” early on, and we thought, sure, why not, it’s family. That was our mistake.
>
> It started small. She suggested lavender for the flower arrangements instead of the white and green palette we picked. Then she “accidentally” told the caterer we were doing a full meat menu—we’re vegetarian. She claimed she thought we “changed our minds” after a family BBQ she hosted (??).
>
> The final straw? She reprinted the wedding invites behind our backs.
>
> I wish I were kidding. We had already sent out our invitations—simple, clean, classy. She didn’t like the font or how “bare” they looked. So she had a graphic designer friend remake them with a “more elegant” cursive and a different RSVP number (her number) so she could “help manage replies.” A bunch of my extended family didn’t even know they got the wrong ones until we called to confirm numbers. Some people RSVP’d to her and we had no idea.
>
> When I confronted her, she acted totally innocent—“Oh sweetie, I thought you’d be so busy, I was just trying to take some of the weight off.” I told her flat out that this is our wedding, not hers, and she has no more say in the planning. I even told our wedding planner to not take her calls or emails from now on.
>
> Now my fiancé’s side of the family is upset, saying I’m being too harsh and “shutting out family.” Even my fiancé (who was on my side) is starting to get cold feet about “banning his mom from helping.”
>
> So... AITA for drawing a hard boundary and basically telling my MIL to back off?

Not bad right? Well, I wouldn't want to read this on Reddit knowing my prompt and the lack of effort I put into this. But for testing purposes we're all set. 

Here's the prompt btw:

<img src="/assets/images/posts/2025/05/2025-05-14-how-much-of-reddit-is-just-ai-slop/fake-story-prompt.png" alt="Fake Story Prompt" loading="lazy" width="100%" height="auto" />

All of the AI detectors I tried told me that this was valid text generated by a human...

AI Detector #1:
<img src="/assets/images/posts/2025/05/2025-05-14-how-much-of-reddit-is-just-ai-slop/ai-detector-1.png" alt="AI Detector #1" loading="lazy" width="100%" height="auto" />

AI Detector #2:
<img src="/assets/images/posts/2025/05/2025-05-14-how-much-of-reddit-is-just-ai-slop/ai-detector-2.png" alt="AI Detector #2" loading="lazy" width="100%" height="auto" />

AI Detector #3:
<img src="/assets/images/posts/2025/05/2025-05-14-how-much-of-reddit-is-just-ai-slop/ai-detector-3.png" alt="AI Detector #3" loading="lazy" width="100%" height="auto" />

Do not spend money on AI detection systems. They don't work. And if you're judging any text using a system like this... May the singularity have mercy on your soul. 

But seriously just don't do it, it's very lame and they don't work.

Compare those detection systems against our method, cobbled together in about an hour. And the results are interesting:

<img src="/assets/images/posts/2025/05/2025-05-14-how-much-of-reddit-is-just-ai-slop/custom-text-result-ai.png" alt="Custom Method and AI Text Results" loading="lazy" width="100%" height="auto" />

It properly flagged this story as being generated by AI!

In practice you can see our scoring looks like this:

```json
Raw score:        0.903  
Calibrated score: 0.892  
Breakdown: {
  "q": 1.0,   # quote density
  "d": 0.88,  # dash usage
  "u": 0.60,  # odd/unicode characters
  "s": 0.08,  # spelling perfection
  "p": 0.29,  # paragraph regularity
  "e": 1.0,   # emotion intensity
  "l": 0.00,  # sentence-length uniformity
  "raw": 0.903
}
```

To explain what these mean:

- `q` (quotes) = 1.0, Maximum quotation mark density—over‐quoting is a strong AI signal.
- `d` (dashes) ~ 0.88, Heavy use of em- and en-dashes, another bot‐like punctuation pattern.
- `u` (odd/unicode) = 0.60, Many non-ASCII characters (accents, symbols) sprinkled in.
- `s` (spelling) ~ 0.08, Almost zero typos—near-perfect spelling often reads “too clean.”
- `p` (paragraph regularity) ≈ 0.29, Paragraphs show some uniformity in sentence counts.
- `e` (emotion) = 1.0, VADER sees very strong sentiment—overstated emotion is common in AI text.
- `l` (sentence-length uniformity) = 0.00, Sentence lengths vary wildly (no predictable rhythm).

And finally our scores:

- `Raw score` (0.903), the weighted sum of these signals (quotes and dashes dominate).
- `Calibrated score` (0.892), after subtracting the base (0.10) and stretching to \[0–1], this lands in "Very Likely AI"—a near-certain flag for synthetic content.

Our heuristic still can’t grasp the actual meaning of a post. It won’t flag a crafty AI that pads in Reddit slang or intentionally drops odd characters. None of these individual signals proves anything alone, but when you combine them. The quotes, dashes, non-ASCII characters, perfect spelling, and over-the-top emotion, they form a pretty strong fingerprint. In our test, the custom AI snippet scored a raw 0.903 (calibrated 0.892), landing cleanly in the “Very Likely AI” bucket. That’s exactly where we want it.

Here's an example of a completely human generated bit of text that I wrote myself (I may or may not have had some fun with the story, sorry if you're a course bro):

<img src="/assets/images/posts/2025/05/2025-05-14-how-much-of-reddit-is-just-ai-slop/custom-text-result-human.png" alt="Custom Method and Human Text Results" loading="lazy" width="100%" height="auto" />

With a raw score of 0.035 (calibrated 0.00), this text falls squarely into "Very Likely Human." It knows that this was human generated! Not scientific exactly, but not bad for an hour of work.

In the example from Neon Sharp’s video, "[AITA for refusing to cook...](https://www.reddit.com/r/AITAH/comments/1gh6en0/aita_for_refusing_to_cook_after_my_bf_tried_to/)", the post (38,738 upvotes) scored a raw 0.63 and a calibrated 0.589 using our method, placing it squarely in our "Needs Review" band. It’s not quite "Very Likely AI," but it’s close enough to warrant a second look, and it shows our method flags that case too.

<img src="/assets/images/posts/2025/05/2025-05-14-how-much-of-reddit-is-just-ai-slop/video-text-result.png" alt="Custom Method and Video Text Results" loading="lazy" width="100%" height="auto" />

Given the ubiquity of things like ChatGPT, Gemini, Claude, Copilot and others, it wouldn't surprise me if _most_ users leverage AI tools to gut check content before posting. So our guessing could never be 100% accurate. All we can see so far is that we've built a method that might yield more results than the AI detectors.

I'm going to firmly add another caveat in case it wasn't clear: **THIS ANALYSIS IS ALMOST CERTAINLY WRONG, NO ONE CAN DETECT AI FOR CERTAIN**! And to be fully up front, we took one purely human-written fake AITA post and one AI-generated counterpart and ran them through our detector. The human sample scored a raw 0.035 (calibrated 0.00), landing in "Very Likely Human," while the AI snippet scored a raw 0.90 (calibrated 0.89), clearly "Very Likely AI." This single test shows our heuristics can distinguish extreme cases. Although for statistically robust results, you’d need dozens (or hundreds) of labeled examples.

With that said, Let's see what we found.

## Showing the Results

To evaluate the method, we gathered together 1,000 posts from two subreddits:
- [r/AmItheAsshole](https://www.reddit.com/r/AmItheAsshole/)
- [r/AITAH](https://www.reddit.com/r/AITAH/)

We chose these two subreddits because r/AmItheAsshole is what was covered in the Neon Sharpe video, r/AITAH is largely equivalent in terms of content, but is moderated a bit more loosely on Reddit. 

After running nearly 1,000 of this year’s top r/AmItheAsshole posts (via [https://reddit.com/r/AITAH/top/?sort=top&t=year](https://reddit.com/r/AITAH/top/?sort=top&t=year)) through our detector, the results were both surprising and unsettling.

The AI-likelihood breakdown reveals that only about 5% of highly upvoted posts fall into the Likely/Very Likely AI categories, roughly 19% sit in the ambiguous Needs Review zone, and the remaining 76% are confidently tagged as human.

<div id="label-distribution"></div>

<script src="https://d3js.org/d3.v7.min.js"></script>
<script>
  const labelData = [
    { label: "Very Likely AI",    count:   1 },
    { label: "Likely AI",         count:  45 },
    { label: "Needs Review",      count: 188 },
    { label: "Probably Human",    count: 187 },
    { label: "Very Likely Human", count: 575 }
  ];

  const margin = { top: 40, right: 20, bottom: 60, left: 60 };
  const width  = 600 - margin.left - margin.right;
  const height = 350 - margin.top  - margin.bottom;

  const svg = d3.select("#label-distribution")
    .append("svg")
      .attr("viewBox", `0 0 ${width+margin.left+margin.right} ${height+margin.top+margin.bottom}`)
      .classed("img-fluid", true)
    .append("g")
      .attr("transform", `translate(${margin.left},${margin.top})`);

  const xDomain = [
    "Very Likely Human",
    "Probably Human",
    "Needs Review",
    "Likely AI",
    "Very Likely AI"
  ];
  const x = d3.scaleBand().domain(xDomain).range([0,width]).padding(0.2);
  const y = d3.scaleLinear().domain([0, d3.max(labelData, d=>d.count)]).nice().range([height,0]);

  // Axes
  svg.append("g").call(d3.axisLeft(y));
  svg.append("g")
     .attr("transform", `translate(0,${height})`)
     .call(d3.axisBottom(x))
     .selectAll("text")
       .attr("transform","rotate(-20)")
       .style("text-anchor","end");

  // Bars + native title + count labels
  svg.selectAll("rect.bar")
    .data(labelData)
    .join("rect")
      .attr("class","bar")
      .attr("x", d=> x(d.label))
      .attr("y", d=> y(d.count))
      .attr("width",  x.bandwidth())
      .attr("height", d=> height - y(d.count))
      .attr("fill", "#FF4500")
      .attr("title", d=> `${d.count} posts`);

  svg.selectAll("text.bar-label")
    .data(labelData)
    .join("text")
      .attr("class","bar-label")
      .attr("x", d=> x(d.label) + x.bandwidth()/2)
      .attr("y", d=> y(d.count) - 5)
      .attr("text-anchor","middle")
      .style("font-size","12px")
      .style("fill","#333")
      .text(d=> d.count);

  // Title
  svg.append("text")
    .attr("x", width/2)
    .attr("y", -10)
    .attr("text-anchor","middle")
    .style("font-size","16px")
    .style("font-weight","bold")
    .text("r/AmItheAsshole AI-Likelihood Label Distribution");
</script>

As the chart makes clear, only 46 out of 996 posts, just under 5%, land in our Likely or Very Likely AI bins. Another 188 posts (~19%) fall into the gray-area Needs Review bin, where polished prose could be real or written by AI. The vast majority, 762 posts (~76%), are tagged as human (Probably or Very Likely), and a single outlier (~0.1%) sits in the "Very Likely AI" category.

<div id="ai-vs-upvotes" style="width:100%; max-width:800px; height:450px;"></div>

<style>
  #ai-vs-upvotes svg { width:100%; height:100%; }
  .legend { font-size: 12px; }
  .axis-label {
    font-size: 14px; font-weight: bold; fill: #000;
  }
</style>

{% raw %}
<script src="https://d3js.org/d3.v7.min.js"></script>
<script>
document.addEventListener("DOMContentLoaded", () => {
  // ─── Inline data: replace with your full array ───
  const rawData = [
  { ai_score: 0.000000, score: 41492 },
  { ai_score: 0.386000, score: 29305 },
  { ai_score: 0.109000, score: 27425 },
  { ai_score: 0.007000, score: 27194 },
  { ai_score: 0.052000, score: 26497 },
  { ai_score: 0.401000, score: 25936 },
  { ai_score: 0.437000, score: 25188 },
  { ai_score: 0.000000, score: 25181 },
  { ai_score: 0.603000, score: 25170 },
  { ai_score: 0.000000, score: 25091 },
  { ai_score: 0.037000, score: 24613 },
  { ai_score: 0.035000, score: 24241 },
  { ai_score: 0.000000, score: 24057 },
  { ai_score: 0.590000, score: 23552 },
  { ai_score: 0.129000, score: 23487 },
  { ai_score: 0.083000, score: 23173 },
  { ai_score: 0.243000, score: 23199 },
  { ai_score: 0.471000, score: 23044 },
  { ai_score: 0.000000, score: 23030 },
  { ai_score: 0.637000, score: 22982 },
  { ai_score: 0.000000, score: 22833 },
  { ai_score: 0.038000, score: 22568 },
  { ai_score: 0.071000, score: 22190 },
  { ai_score: 0.000000, score: 22118 },
  { ai_score: 0.000000, score: 22049 },
  { ai_score: 0.000000, score: 21971 },
  { ai_score: 0.000000, score: 21881 },
  { ai_score: 0.485000, score: 21868 },
  { ai_score: 0.374000, score: 21775 },
  { ai_score: 0.000000, score: 21569 },
  { ai_score: 0.629000, score: 21577 },
  { ai_score: 0.000000, score: 20999 },
  { ai_score: 0.152000, score: 20853 },
  { ai_score: 0.253000, score: 20821 },
  { ai_score: 0.000000, score: 20472 },
  { ai_score: 0.000000, score: 20406 },
  { ai_score: 0.000000, score: 20246 },
  { ai_score: 0.588000, score: 20237 },
  { ai_score: 0.000000, score: 20224 },
  { ai_score: 0.067000, score: 19946 },
  { ai_score: 0.055000, score: 19423 },
  { ai_score: 0.002000, score: 19287 },
  { ai_score: 0.259000, score: 19264 },
  { ai_score: 0.092000, score: 19102 },
  { ai_score: 0.014000, score: 19022 },
  { ai_score: 0.106000, score: 18888 },
  { ai_score: 0.000000, score: 18791 },
  { ai_score: 0.590000, score: 18718 },
  { ai_score: 0.593000, score: 18660 },
  { ai_score: 0.000000, score: 18498 },
  { ai_score: 0.000000, score: 18428 },
  { ai_score: 0.000000, score: 18359 },
  { ai_score: 0.000000, score: 18286 },
  { ai_score: 0.022000, score: 18255 },
  { ai_score: 0.000000, score: 18174 },
  { ai_score: 0.721000, score: 18147 },
  { ai_score: 0.000000, score: 18134 },
  { ai_score: 0.260000, score: 18108 },
  { ai_score: 0.590000, score: 17835 },
  { ai_score: 0.000000, score: 17822 },
  { ai_score: 0.028000, score: 17680 },
  { ai_score: 0.000000, score: 17542 },
  { ai_score: 0.101000, score: 17473 },
  { ai_score: 0.000000, score: 17396 },
  { ai_score: 0.030000, score: 17288 },
  { ai_score: 0.427000, score: 17127 },
  { ai_score: 0.441000, score: 17083 },
  { ai_score: 0.000000, score: 17050 },
  { ai_score: 0.361000, score: 16963 },
  { ai_score: 0.605000, score: 16940 },
  { ai_score: 0.593000, score: 16866 },
  { ai_score: 0.000000, score: 16742 },
  { ai_score: 0.204000, score: 16603 },
  { ai_score: 0.197000, score: 16551 },
  { ai_score: 0.016000, score: 16447 },
  { ai_score: 0.590000, score: 16350 },
  { ai_score: 0.007000, score: 16258 },
  { ai_score: 0.000000, score: 16113 },
  { ai_score: 0.301000, score: 16110 },
  { ai_score: 0.233000, score: 15997 },
  { ai_score: 0.471000, score: 15888 },
  { ai_score: 0.002000, score: 15715 },
  { ai_score: 0.000000, score: 15650 },
  { ai_score: 0.205000, score: 15552 },
  { ai_score: 0.768000, score: 15487 },
  { ai_score: 0.594000, score: 15369 },
  { ai_score: 0.278000, score: 15311 },
  { ai_score: 0.365000, score: 15250 },
  { ai_score: 0.045000, score: 15183 },
  { ai_score: 0.060000, score: 15156 },
  { ai_score: 0.403000, score: 15073 },
  { ai_score: 0.090000, score: 15053 },
  { ai_score: 0.261000, score: 15025 },
  { ai_score: 0.625000, score: 14959 },
  { ai_score: 0.000000, score: 14830 },
  { ai_score: 0.291000, score: 14815 },
  { ai_score: 0.374000, score: 14688 },
  { ai_score: 0.593000, score: 14605 },
  { ai_score: 0.579000, score: 14530 },
  { ai_score: 0.082000, score: 14521 },
  { ai_score: 0.565000, score: 14490 },
  { ai_score: 0.004000, score: 14442 },
  { ai_score: 0.259000, score: 14423 },
  { ai_score: 0.592000, score: 14428 },
  { ai_score: 0.000000, score: 14426 },
  { ai_score: 0.026000, score: 14423 },
  { ai_score: 0.464000, score: 14358 },
  { ai_score: 0.000000, score: 14349 },
  { ai_score: 0.344000, score: 14310 },
  { ai_score: 0.583000, score: 14307 },
  { ai_score: 0.203000, score: 14289 },
  { ai_score: 0.392000, score: 14285 },
  { ai_score: 0.349000, score: 14283 },
  { ai_score: 0.215000, score: 14240 },
  { ai_score: 0.000000, score: 14236 },
  { ai_score: 0.000000, score: 14226 },
  { ai_score: 0.602000, score: 14225 },
  { ai_score: 0.028000, score: 14147 },
  { ai_score: 0.000000, score: 14132 },
  { ai_score: 0.000000, score: 14073 },
  { ai_score: 0.000000, score: 14043 },
  { ai_score: 0.645000, score: 14026 },
  { ai_score: 0.000000, score: 14014 },
  { ai_score: 0.000000, score: 14001 },
  { ai_score: 0.218000, score: 13953 },
  { ai_score: 0.000000, score: 13901 },
  { ai_score: 0.225000, score: 13859 },
  { ai_score: 0.587000, score: 13783 },
  { ai_score: 0.000000, score: 13804 },
  { ai_score: 0.581000, score: 13770 },
  { ai_score: 0.552000, score: 13702 },
  { ai_score: 0.000000, score: 13691 },
  { ai_score: 0.000000, score: 13689 },
  { ai_score: 0.071000, score: 13644 },
  { ai_score: 0.460000, score: 13570 },
  { ai_score: 0.000000, score: 13501 },
  { ai_score: 0.000000, score: 13454 },
  { ai_score: 0.048000, score: 13432 },
  { ai_score: 0.067000, score: 13424 },
  { ai_score: 0.191000, score: 13420 },
  { ai_score: 0.189000, score: 13383 },
  { ai_score: 0.197000, score: 13368 },
  { ai_score: 0.209000, score: 13343 },
  { ai_score: 0.000000, score: 13228 },
  { ai_score: 0.082000, score: 13224 },
  { ai_score: 0.000000, score: 13195 },
  { ai_score: 0.628000, score: 13188 },
  { ai_score: 0.000000, score: 13153 },
  { ai_score: 0.459000, score: 13106 },
  { ai_score: 0.000000, score: 13032 },
  { ai_score: 0.384000, score: 13012 },
  { ai_score: 0.000000, score: 12990 },
  { ai_score: 0.374000, score: 12912 },
  { ai_score: 0.592000, score: 12869 },
  { ai_score: 0.586000, score: 12834 },
  { ai_score: 0.000000, score: 12806 },
  { ai_score: 0.700000, score: 12806 },
  { ai_score: 0.000000, score: 12783 },
  { ai_score: 0.152000, score: 12779 },
  { ai_score: 0.000000, score: 12733 },
  { ai_score: 0.109000, score: 12709 },
  { ai_score: 0.587000, score: 12699 },
  { ai_score: 0.125000, score: 12701 },
  { ai_score: 0.032000, score: 12676 },
  { ai_score: 0.000000, score: 12643 },
  { ai_score: 0.164000, score: 12496 },
  { ai_score: 0.234000, score: 12494 },
  { ai_score: 0.294000, score: 12485 },
  { ai_score: 0.000000, score: 12478 },
  { ai_score: 0.409000, score: 12489 },
  { ai_score: 0.184000, score: 12463 },
  { ai_score: 0.429000, score: 12445 },
  { ai_score: 0.431000, score: 12403 },
  { ai_score: 0.000000, score: 12379 },
  { ai_score: 0.000000, score: 12364 },
  { ai_score: 0.000000, score: 12326 },
  { ai_score: 0.259000, score: 12274 },
  { ai_score: 0.342000, score: 12235 },
  { ai_score: 0.000000, score: 12232 },
  { ai_score: 0.000000, score: 12062 },
  { ai_score: 0.009000, score: 12014 },
  { ai_score: 0.002000, score: 12000 },
  { ai_score: 0.113000, score: 11939 },
  { ai_score: 0.547000, score: 11940 },
  { ai_score: 0.000000, score: 11929 },
  { ai_score: 0.000000, score: 11892 },
  { ai_score: 0.057000, score: 11901 },
  { ai_score: 0.000000, score: 11887 },
  { ai_score: 0.000000, score: 11861 },
  { ai_score: 0.000000, score: 11848 },
  { ai_score: 0.588000, score: 11796 },
  { ai_score: 0.199000, score: 11823 },
  { ai_score: 0.329000, score: 11823 },
  { ai_score: 0.437000, score: 11813 },
  { ai_score: 0.592000, score: 11793 },
  { ai_score: 0.000000, score: 11784 },
  { ai_score: 0.000000, score: 11783 },
  { ai_score: 0.000000, score: 11774 },
  { ai_score: 0.132000, score: 11780 },
  { ai_score: 0.216000, score: 11755 },
  { ai_score: 0.000000, score: 11694 },
  { ai_score: 0.000000, score: 11661 },
  { ai_score: 0.594000, score: 11617 },
  { ai_score: 0.000000, score: 11599 },
  { ai_score: 0.097000, score: 11610 },
  { ai_score: 0.747000, score: 11517 },
  { ai_score: 0.000000, score: 11500 },
  { ai_score: 0.582000, score: 11480 },
  { ai_score: 0.380000, score: 11470 },
  { ai_score: 0.209000, score: 11443 },
  { ai_score: 0.000000, score: 11416 },
  { ai_score: 0.000000, score: 11409 },
  { ai_score: 0.609000, score: 11373 },
  { ai_score: 0.225000, score: 11359 },
  { ai_score: 0.679000, score: 11302 },
  { ai_score: 0.624000, score: 11291 },
  { ai_score: 0.390000, score: 11256 },
  { ai_score: 0.595000, score: 11252 },
  { ai_score: 0.094000, score: 11238 },
  { ai_score: 0.000000, score: 11173 },
  { ai_score: 0.639000, score: 11176 },
  { ai_score: 0.003000, score: 11171 },
  { ai_score: 0.000000, score: 11156 },
  { ai_score: 0.000000, score: 11161 },
  { ai_score: 0.635000, score: 11083 },
  { ai_score: 0.000000, score: 11081 },
  { ai_score: 0.593000, score: 11071 },
  { ai_score: 0.020000, score: 11067 },
  { ai_score: 0.260000, score: 11036 },
  { ai_score: 0.000000, score: 10996 },
  { ai_score: 0.000000, score: 10961 },
  { ai_score: 0.000000, score: 10922 },
  { ai_score: 0.000000, score: 10918 },
  { ai_score: 0.099000, score: 10866 },
  { ai_score: 0.000000, score: 10861 },
  { ai_score: 0.261000, score: 10843 },
  { ai_score: 0.000000, score: 10818 },
  { ai_score: 0.000000, score: 10806 },
  { ai_score: 0.221000, score: 10808 },
  { ai_score: 0.007000, score: 10799 },
  { ai_score: 0.580000, score: 10792 },
  { ai_score: 0.000000, score: 10792 },
  { ai_score: 0.212000, score: 10769 },
  { ai_score: 0.377000, score: 10703 },
  { ai_score: 0.410000, score: 10679 },
  { ai_score: 0.000000, score: 10646 },
  { ai_score: 0.352000, score: 10643 },
  { ai_score: 0.000000, score: 10609 },
  { ai_score: 0.135000, score: 10595 },
  { ai_score: 0.382000, score: 10600 },
  { ai_score: 0.702000, score: 10578 },
  { ai_score: 0.327000, score: 10558 },
  { ai_score: 0.296000, score: 10524 },
  { ai_score: 0.000000, score: 10489 },
  { ai_score: 0.011000, score: 10469 },
  { ai_score: 0.506000, score: 10460 },
  { ai_score: 0.577000, score: 10446 },
  { ai_score: 0.749000, score: 10415 },
  { ai_score: 0.324000, score: 10398 },
  { ai_score: 0.206000, score: 10390 },
  { ai_score: 0.265000, score: 10353 },
  { ai_score: 0.658000, score: 10296 },
  { ai_score: 0.000000, score: 10288 },
  { ai_score: 0.000000, score: 10280 },
  { ai_score: 0.000000, score: 10281 },
  { ai_score: 0.028000, score: 10254 },
  { ai_score: 0.154000, score: 10211 },
  { ai_score: 0.402000, score: 10183 },
  { ai_score: 0.000000, score: 10181 },
  { ai_score: 0.576000, score: 10166 },
  { ai_score: 0.309000, score: 10127 },
  { ai_score: 0.183000, score: 10114 },
  { ai_score: 0.000000, score: 10055 },
  { ai_score: 0.269000, score: 10029 },
  { ai_score: 0.000000, score: 10020 },
  { ai_score: 0.000000, score: 10006 },
  { ai_score: 0.425000, score: 9956 },
  { ai_score: 0.000000, score: 9886 },
  { ai_score: 0.036000, score: 9865 },
  { ai_score: 0.000000, score: 9838 },
  { ai_score: 0.000000, score: 9858 },
  { ai_score: 0.071000, score: 9842 },
  { ai_score: 0.356000, score: 9845 },
  { ai_score: 0.000000, score: 9836 },
  { ai_score: 0.568000, score: 9832 },
  { ai_score: 0.147000, score: 9800 },
  { ai_score: 0.000000, score: 9780 },
  { ai_score: 0.345000, score: 9780 },
  { ai_score: 0.000000, score: 9767 },
  { ai_score: 0.441000, score: 9751 },
  { ai_score: 0.000000, score: 9726 },
  { ai_score: 0.579000, score: 9649 },
  { ai_score: 0.000000, score: 9682 },
  { ai_score: 0.320000, score: 9675 },
  { ai_score: 0.268000, score: 9659 },
  { ai_score: 0.176000, score: 9659 },
  { ai_score: 0.000000, score: 9614 },
  { ai_score: 0.000000, score: 9611 },
  { ai_score: 0.588000, score: 9583 },
  { ai_score: 0.122000, score: 9578 },
  { ai_score: 0.439000, score: 9582 },
  { ai_score: 0.613000, score: 9571 },
  { ai_score: 0.235000, score: 9559 },
  { ai_score: 0.405000, score: 9560 },
  { ai_score: 0.243000, score: 9555 },
  { ai_score: 0.000000, score: 9550 },
  { ai_score: 0.097000, score: 9525 },
  { ai_score: 0.586000, score: 9523 },
  { ai_score: 0.673000, score: 9518 },
  { ai_score: 0.125000, score: 9518 },
  { ai_score: 0.581000, score: 9501 },
  { ai_score: 0.567000, score: 9484 },
  { ai_score: 0.006000, score: 9486 },
  { ai_score: 0.000000, score: 9482 },
  { ai_score: 0.598000, score: 9480 },
  { ai_score: 0.581000, score: 9467 },
  { ai_score: 0.370000, score: 9448 },
  { ai_score: 0.443000, score: 9434 },
  { ai_score: 0.586000, score: 9400 },
  { ai_score: 0.368000, score: 9397 },
  { ai_score: 0.699000, score: 9387 },
  { ai_score: 0.000000, score: 9380 },
  { ai_score: 0.000000, score: 9358 },
  { ai_score: 0.585000, score: 9361 },
  { ai_score: 0.006000, score: 9349 },
  { ai_score: 0.309000, score: 9289 },
  { ai_score: 0.474000, score: 9284 },
  { ai_score: 0.589000, score: 9276 },
  { ai_score: 0.589000, score: 9270 },
  { ai_score: 0.504000, score: 9263 },
  { ai_score: 0.038000, score: 9262 },
  { ai_score: 0.596000, score: 9235 },
  { ai_score: 0.019000, score: 9207 },
  { ai_score: 0.044000, score: 9195 },
  { ai_score: 0.145000, score: 9193 },
  { ai_score: 0.587000, score: 9175 },
  { ai_score: 0.000000, score: 9149 },
  { ai_score: 0.057000, score: 9151 },
  { ai_score: 0.016000, score: 9151 },
  { ai_score: 0.294000, score: 9124 },
  { ai_score: 0.168000, score: 9104 },
  { ai_score: 0.052000, score: 9100 },
  { ai_score: 0.000000, score: 9083 },
  { ai_score: 0.232000, score: 9083 },
  { ai_score: 0.532000, score: 9079 },
  { ai_score: 0.478000, score: 9058 },
  { ai_score: 0.359000, score: 9037 },
  { ai_score: 0.329000, score: 9025 },
  { ai_score: 0.572000, score: 9019 },
  { ai_score: 0.283000, score: 8995 },
  { ai_score: 0.000000, score: 8996 },
  { ai_score: 0.000000, score: 8977 },
  { ai_score: 0.253000, score: 8956 },
  { ai_score: 0.021000, score: 8933 },
  { ai_score: 0.528000, score: 8918 },
  { ai_score: 0.313000, score: 8889 },
  { ai_score: 0.000000, score: 8887 },
  { ai_score: 0.320000, score: 8883 },
  { ai_score: 0.107000, score: 8871 },
  { ai_score: 0.012000, score: 8854 },
  { ai_score: 0.595000, score: 8862 },
  { ai_score: 0.000000, score: 8853 },
  { ai_score: 0.065000, score: 8833 },
  { ai_score: 0.195000, score: 8838 },
  { ai_score: 0.406000, score: 8843 },
  { ai_score: 0.000000, score: 8834 },
  { ai_score: 0.098000, score: 8814 },
  { ai_score: 0.129000, score: 8808 },
  { ai_score: 0.598000, score: 8770 },
  { ai_score: 0.386000, score: 8732 },
  { ai_score: 0.438000, score: 8728 },
  { ai_score: 0.011000, score: 8721 },
  { ai_score: 0.000000, score: 8722 },
  { ai_score: 0.089000, score: 8695 },
  { ai_score: 0.000000, score: 8663 },
  { ai_score: 0.051000, score: 8656 },
  { ai_score: 0.441000, score: 8644 },
  { ai_score: 0.000000, score: 8613 },
  { ai_score: 0.000000, score: 8621 },
  { ai_score: 0.201000, score: 8603 },
  { ai_score: 0.000000, score: 8589 },
  { ai_score: 0.407000, score: 8595 },
  { ai_score: 0.000000, score: 8583 },
  { ai_score: 0.240000, score: 8564 },
  { ai_score: 0.579000, score: 8586 },
  { ai_score: 0.000000, score: 8579 },
  { ai_score: 0.433000, score: 8581 },
  { ai_score: 0.030000, score: 8574 },
  { ai_score: 0.272000, score: 8571 },
  { ai_score: 0.000000, score: 8553 },
  { ai_score: 0.353000, score: 8572 },
  { ai_score: 0.668000, score: 8543 },
  { ai_score: 0.155000, score: 8538 },
  { ai_score: 0.000000, score: 8537 },
  { ai_score: 0.015000, score: 8529 },
  { ai_score: 0.000000, score: 8532 },
  { ai_score: 0.000000, score: 8529 },
  { ai_score: 0.000000, score: 8518 },
  { ai_score: 0.202000, score: 8509 },
  { ai_score: 0.598000, score: 8507 },
  { ai_score: 0.589000, score: 8504 },
  { ai_score: 0.000000, score: 8473 },
  { ai_score: 0.112000, score: 8477 },
  { ai_score: 0.000000, score: 8463 },
  { ai_score: 0.000000, score: 8445 },
  { ai_score: 0.000000, score: 8421 },
  { ai_score: 0.000000, score: 8409 },
  { ai_score: 0.538000, score: 8396 },
  { ai_score: 0.106000, score: 8387 },
  { ai_score: 0.291000, score: 8387 },
  { ai_score: 0.243000, score: 8379 },
  { ai_score: 0.337000, score: 8379 },
  { ai_score: 0.000000, score: 8371 },
  { ai_score: 0.041000, score: 8364 },
  { ai_score: 0.159000, score: 8329 },
  { ai_score: 0.174000, score: 8322 },
  { ai_score: 0.254000, score: 8319 },
  { ai_score: 0.392000, score: 8289 },
  { ai_score: 0.548000, score: 8296 },
  { ai_score: 0.070000, score: 8292 },
  { ai_score: 0.194000, score: 8257 },
  { ai_score: 0.585000, score: 8247 },
  { ai_score: 0.000000, score: 8237 },
  { ai_score: 0.000000, score: 8219 },
  { ai_score: 0.590000, score: 8223 },
  { ai_score: 0.292000, score: 8205 },
  { ai_score: 0.000000, score: 8204 },
  { ai_score: 0.000000, score: 8199 },
  { ai_score: 0.444000, score: 8185 },
  { ai_score: 0.000000, score: 8185 },
  { ai_score: 0.486000, score: 8172 },
  { ai_score: 0.554000, score: 8160 },
  { ai_score: 0.173000, score: 8125 },
  { ai_score: 0.031000, score: 8103 },
  { ai_score: 0.000000, score: 8093 },
  { ai_score: 0.000000, score: 8081 },
  { ai_score: 0.582000, score: 8049 },
  { ai_score: 0.450000, score: 8046 },
  { ai_score: 0.000000, score: 8038 },
  { ai_score: 0.368000, score: 8042 },
  { ai_score: 0.000000, score: 8027 },
  { ai_score: 0.276000, score: 8033 },
  { ai_score: 0.654000, score: 8015 },
  { ai_score: 0.324000, score: 7997 },
  { ai_score: 0.000000, score: 7973 },
  { ai_score: 0.592000, score: 7965 },
  { ai_score: 0.549000, score: 7956 },
  { ai_score: 0.110000, score: 7951 },
  { ai_score: 0.000000, score: 7940 },
  { ai_score: 0.000000, score: 7922 },
  { ai_score: 0.000000, score: 7916 },
  { ai_score: 0.000000, score: 7899 },
  { ai_score: 0.481000, score: 7862 },
  { ai_score: 0.068000, score: 7854 },
  { ai_score: 0.143000, score: 7847 },
  { ai_score: 0.028000, score: 7844 },
  { ai_score: 0.234000, score: 7847 },
  { ai_score: 0.562000, score: 7836 },
  { ai_score: 0.383000, score: 7820 },
  { ai_score: 0.213000, score: 7820 },
  { ai_score: 0.021000, score: 7786 },
  { ai_score: 0.395000, score: 7763 },
  { ai_score: 0.136000, score: 7761 },
  { ai_score: 0.510000, score: 7757 },
  { ai_score: 0.000000, score: 7750 },
  { ai_score: 0.480000, score: 7744 },
  { ai_score: 0.607000, score: 7745 },
  { ai_score: 0.145000, score: 7733 },
  { ai_score: 0.601000, score: 7728 },
  { ai_score: 0.097000, score: 7721 },
  { ai_score: 0.160000, score: 7722 },
  { ai_score: 0.059000, score: 7717 },
  { ai_score: 0.200000, score: 7700 },
  { ai_score: 0.000000, score: 7696 },
  { ai_score: 0.504000, score: 7700 },
  { ai_score: 0.591000, score: 7687 },
  { ai_score: 0.000000, score: 7693 },
  { ai_score: 0.247000, score: 7676 },
  { ai_score: 0.170000, score: 7679 },
  { ai_score: 0.000000, score: 7678 },
  { ai_score: 0.174000, score: 7670 },
  { ai_score: 0.000000, score: 7657 },
  { ai_score: 0.579000, score: 7649 },
  { ai_score: 0.000000, score: 7633 },
  { ai_score: 0.000000, score: 7632 },
  { ai_score: 0.000000, score: 7632 },
  { ai_score: 0.354000, score: 7624 },
  { ai_score: 0.000000, score: 7617 },
  { ai_score: 0.119000, score: 7618 },
  { ai_score: 0.445000, score: 7605 },
  { ai_score: 0.118000, score: 7589 },
  { ai_score: 0.322000, score: 7573 },
  { ai_score: 0.000000, score: 7564 },
  { ai_score: 0.391000, score: 7567 },
  { ai_score: 0.286000, score: 7563 },
  { ai_score: 0.341000, score: 7562 },
  { ai_score: 0.000000, score: 7565 },
  { ai_score: 0.400000, score: 7557 },
  { ai_score: 0.102000, score: 7562 },
  { ai_score: 0.578000, score: 7552 },
  { ai_score: 0.000000, score: 7545 },
  { ai_score: 0.069000, score: 7532 },
  { ai_score: 0.373000, score: 7538 },
  { ai_score: 0.051000, score: 7531 },
  { ai_score: 0.000000, score: 7523 },
  { ai_score: 0.190000, score: 7526 },
  { ai_score: 0.000000, score: 7522 },
  { ai_score: 0.177000, score: 7513 },
  { ai_score: 0.036000, score: 7499 },
  { ai_score: 0.000000, score: 7506 },
  { ai_score: 0.013000, score: 7498 },
  { ai_score: 0.003000, score: 7495 },
  { ai_score: 0.596000, score: 7458 },
  { ai_score: 0.306000, score: 7447 },
  { ai_score: 0.000000, score: 7438 },
  { ai_score: 0.598000, score: 7432 },
  { ai_score: 0.397000, score: 7437 },
  { ai_score: 0.553000, score: 7416 },
  { ai_score: 0.000000, score: 7418 },
  { ai_score: 0.044000, score: 7407 },
  { ai_score: 0.000000, score: 7402 },
  { ai_score: 0.194000, score: 7403 },
  { ai_score: 0.000000, score: 7403 },
  { ai_score: 0.000000, score: 7402 },
  { ai_score: 0.000000, score: 7388 },
  { ai_score: 0.108000, score: 7386 },
  { ai_score: 0.000000, score: 7359 },
  { ai_score: 0.346000, score: 7314 },
  { ai_score: 0.000000, score: 7317 },
  { ai_score: 0.073000, score: 7320 },
  { ai_score: 0.000000, score: 7318 },
  { ai_score: 0.000000, score: 7302 },
  { ai_score: 0.000000, score: 7291 },
  { ai_score: 0.000000, score: 7289 },
  { ai_score: 0.585000, score: 7291 },
  { ai_score: 0.000000, score: 7281 },
  { ai_score: 0.211000, score: 7276 },
  { ai_score: 0.000000, score: 7276 },
  { ai_score: 0.509000, score: 7276 },
  { ai_score: 0.145000, score: 7265 },
  { ai_score: 0.519000, score: 7260 },
  { ai_score: 0.000000, score: 7200 },
  { ai_score: 0.433000, score: 7194 },
  { ai_score: 0.008000, score: 7195 },
  { ai_score: 0.000000, score: 7180 },
  { ai_score: 0.358000, score: 7178 },
  { ai_score: 0.587000, score: 7153 },
  { ai_score: 0.227000, score: 7146 },
  { ai_score: 0.000000, score: 7129 },
  { ai_score: 0.585000, score: 7128 },
  { ai_score: 0.000000, score: 7119 },
  { ai_score: 0.000000, score: 7112 },
  { ai_score: 0.219000, score: 7124 },
  { ai_score: 0.091000, score: 7105 },
  { ai_score: 0.031000, score: 7107 },
  { ai_score: 0.000000, score: 7088 },
  { ai_score: 0.040000, score: 7081 },
  { ai_score: 0.376000, score: 7069 },
  { ai_score: 0.000000, score: 7066 },
  { ai_score: 0.190000, score: 7065 },
  { ai_score: 0.586000, score: 7068 },
  { ai_score: 0.005000, score: 7052 },
  { ai_score: 0.590000, score: 7056 },
  { ai_score: 0.582000, score: 7054 },
  { ai_score: 0.000000, score: 7038 },
  { ai_score: 0.297000, score: 7017 },
  { ai_score: 0.432000, score: 7005 },
  { ai_score: 0.598000, score: 6996 },
  { ai_score: 0.249000, score: 6988 },
  { ai_score: 0.228000, score: 6988 },
  { ai_score: 0.000000, score: 6982 },
  { ai_score: 0.000000, score: 6974 },
  { ai_score: 0.000000, score: 6972 },
  { ai_score: 0.584000, score: 6952 },
  { ai_score: 0.000000, score: 6948 },
  { ai_score: 0.120000, score: 6938 },
  { ai_score: 0.000000, score: 6914 },
  { ai_score: 0.000000, score: 6917 },
  { ai_score: 0.068000, score: 6901 },
  { ai_score: 0.000000, score: 6894 },
  { ai_score: 0.567000, score: 6883 },
  { ai_score: 0.000000, score: 6883 },
  { ai_score: 0.000000, score: 6877 },
  { ai_score: 0.000000, score: 6858 },
  { ai_score: 0.579000, score: 6829 },
  { ai_score: 0.000000, score: 6829 },
  { ai_score: 0.599000, score: 6831 },
  { ai_score: 0.552000, score: 6824 },
  { ai_score: 0.556000, score: 6819 },
  { ai_score: 0.000000, score: 6819 },
  { ai_score: 0.000000, score: 6809 },
  { ai_score: 0.399000, score: 6812 },
  { ai_score: 0.000000, score: 6793 },
  { ai_score: 0.125000, score: 6775 },
  { ai_score: 0.356000, score: 6771 },
  { ai_score: 0.112000, score: 6767 },
  { ai_score: 0.000000, score: 6741 },
  { ai_score: 0.443000, score: 6734 },
  { ai_score: 0.000000, score: 6727 },
  { ai_score: 0.000000, score: 6726 },
  { ai_score: 0.631000, score: 6726 },
  { ai_score: 0.000000, score: 6712 },
  { ai_score: 0.366000, score: 6710 },
  { ai_score: 0.374000, score: 6704 },
  { ai_score: 0.025000, score: 6705 },
  { ai_score: 0.221000, score: 6699 },
  { ai_score: 0.000000, score: 6701 },
  { ai_score: 0.642000, score: 6695 },
  { ai_score: 0.000000, score: 6681 },
  { ai_score: 0.000000, score: 6672 },
  { ai_score: 0.393000, score: 6665 },
  { ai_score: 0.278000, score: 6636 },
  { ai_score: 0.056000, score: 6640 },
  { ai_score: 0.421000, score: 6626 },
  { ai_score: 0.390000, score: 6626 },
  { ai_score: 0.598000, score: 6611 },
  { ai_score: 0.000000, score: 6606 },
  { ai_score: 0.000000, score: 6595 },
  { ai_score: 0.027000, score: 6595 },
  { ai_score: 0.581000, score: 6592 },
  { ai_score: 0.338000, score: 6577 },
  { ai_score: 0.477000, score: 6575 },
  { ai_score: 0.097000, score: 6572 },
  { ai_score: 0.061000, score: 6562 },
  { ai_score: 0.000000, score: 6555 },
  { ai_score: 0.228000, score: 6554 },
  { ai_score: 0.456000, score: 6530 },
  { ai_score: 0.000000, score: 6527 },
  { ai_score: 0.489000, score: 6539 },
  { ai_score: 0.199000, score: 6525 },
  { ai_score: 0.000000, score: 6528 },
  { ai_score: 0.000000, score: 6524 },
  { ai_score: 0.000000, score: 6527 },
  { ai_score: 0.691000, score: 6516 },
  { ai_score: 0.244000, score: 6509 },
  { ai_score: 0.000000, score: 6510 },
  { ai_score: 0.102000, score: 6496 },
  { ai_score: 0.003000, score: 6467 },
  { ai_score: 0.000000, score: 6465 },
  { ai_score: 0.000000, score: 6447 },
  { ai_score: 0.000000, score: 6453 },
  { ai_score: 0.645000, score: 6423 },
  { ai_score: 0.595000, score: 6417 },
  { ai_score: 0.183000, score: 6392 },
  { ai_score: 0.000000, score: 6396 },
  { ai_score: 0.000000, score: 6383 },
  { ai_score: 0.183000, score: 6382 },
  { ai_score: 0.096000, score: 6377 },
  { ai_score: 0.388000, score: 6368 },
  { ai_score: 0.175000, score: 6347 },
  { ai_score: 0.000000, score: 6342 },
  { ai_score: 0.000000, score: 6328 },
  { ai_score: 0.583000, score: 6315 },
  { ai_score: 0.023000, score: 6296 },
  { ai_score: 0.236000, score: 6294 },
  { ai_score: 0.130000, score: 6295 },
  { ai_score: 0.334000, score: 6282 },
  { ai_score: 0.190000, score: 6266 },
  { ai_score: 0.280000, score: 6264 },
  { ai_score: 0.249000, score: 6250 },
  { ai_score: 0.000000, score: 6234 },
  { ai_score: 0.000000, score: 6239 },
  { ai_score: 0.603000, score: 6232 },
  { ai_score: 0.582000, score: 6223 },
  { ai_score: 0.372000, score: 6231 },
  { ai_score: 0.612000, score: 6214 },
  { ai_score: 0.583000, score: 6178 },
  { ai_score: 0.000000, score: 6174 },
  { ai_score: 0.010000, score: 6162 },
  { ai_score: 0.321000, score: 6161 },
  { ai_score: 0.000000, score: 6140 },
  { ai_score: 0.080000, score: 6129 },
  { ai_score: 0.671000, score: 6116 },
  { ai_score: 0.000000, score: 6109 },
  { ai_score: 0.138000, score: 6101 },
  { ai_score: 0.000000, score: 6096 },
  { ai_score: 0.000000, score: 6077 },
  { ai_score: 0.210000, score: 6079 },
  { ai_score: 0.000000, score: 6078 },
  { ai_score: 0.000000, score: 6059 },
  { ai_score: 0.405000, score: 6054 },
  { ai_score: 0.000000, score: 6039 },
  { ai_score: 0.355000, score: 6030 },
  { ai_score: 0.000000, score: 6024 },
  { ai_score: 0.228000, score: 6015 },
  { ai_score: 0.354000, score: 6008 },
  { ai_score: 0.335000, score: 6005 },
  { ai_score: 0.600000, score: 6000 },
  { ai_score: 0.000000, score: 6001 },
  { ai_score: 0.000000, score: 6001 },
  { ai_score: 0.000000, score: 5986 },
  { ai_score: 0.591000, score: 5985 },
  { ai_score: 0.000000, score: 5991 },
  { ai_score: 0.000000, score: 5984 },
  { ai_score: 0.098000, score: 5981 },
  { ai_score: 0.359000, score: 5970 },
  { ai_score: 0.000000, score: 5954 },
  { ai_score: 0.353000, score: 5957 },
  { ai_score: 0.085000, score: 5947 },
  { ai_score: 0.271000, score: 5929 },
  { ai_score: 0.000000, score: 5924 },
  { ai_score: 0.000000, score: 5906 },
  { ai_score: 0.153000, score: 5908 },
  { ai_score: 0.416000, score: 5900 },
  { ai_score: 0.000000, score: 5900 },
  { ai_score: 0.000000, score: 5896 },
  { ai_score: 0.239000, score: 5899 },
  { ai_score: 0.000000, score: 5894 },
  { ai_score: 0.512000, score: 5871 },
  { ai_score: 0.000000, score: 5858 },
  { ai_score: 0.277000, score: 5852 },
  { ai_score: 0.596000, score: 5855 },
  { ai_score: 0.454000, score: 5834 },
  { ai_score: 0.065000, score: 5815 },
  { ai_score: 0.398000, score: 5810 },
  { ai_score: 0.000000, score: 5791 },
  { ai_score: 0.004000, score: 5785 },
  { ai_score: 0.003000, score: 5789 },
  { ai_score: 0.457000, score: 5772 },
  { ai_score: 0.035000, score: 5750 },
  { ai_score: 0.051000, score: 5737 },
  { ai_score: 0.493000, score: 5730 },
  { ai_score: 0.595000, score: 5725 },
  { ai_score: 0.333000, score: 5721 },
  { ai_score: 0.346000, score: 5721 },
  { ai_score: 0.368000, score: 5715 },
  { ai_score: 0.000000, score: 5716 },
  { ai_score: 0.000000, score: 5715 },
  { ai_score: 0.720000, score: 5714 },
  { ai_score: 0.266000, score: 5707 },
  { ai_score: 0.078000, score: 5701 },
  { ai_score: 0.016000, score: 5692 },
  { ai_score: 0.588000, score: 5677 },
  { ai_score: 0.000000, score: 5694 },
  { ai_score: 0.289000, score: 5681 },
  { ai_score: 0.467000, score: 5692 },
  { ai_score: 0.184000, score: 5690 },
  { ai_score: 0.000000, score: 5684 },
  { ai_score: 0.608000, score: 5682 },
  { ai_score: 0.000000, score: 5684 },
  { ai_score: 0.384000, score: 5673 },
  { ai_score: 0.000000, score: 5677 },
  { ai_score: 0.000000, score: 5652 },
  { ai_score: 0.596000, score: 5639 },
  { ai_score: 0.258000, score: 5631 },
  { ai_score: 0.602000, score: 5623 },
  { ai_score: 0.329000, score: 5630 },
  { ai_score: 0.012000, score: 5620 },
  { ai_score: 0.403000, score: 5622 },
  { ai_score: 0.071000, score: 5615 },
  { ai_score: 0.053000, score: 5601 },
  { ai_score: 0.422000, score: 5604 },
  { ai_score: 0.013000, score: 5607 },
  { ai_score: 0.018000, score: 5594 },
  { ai_score: 0.349000, score: 5597 },
  { ai_score: 0.467000, score: 5592 },
  { ai_score: 0.302000, score: 5576 },
  { ai_score: 0.000000, score: 5575 },
  { ai_score: 0.000000, score: 5570 },
  { ai_score: 0.000000, score: 5568 },
  { ai_score: 0.408000, score: 5561 },
  { ai_score: 0.000000, score: 5558 },
  { ai_score: 0.375000, score: 5553 },
  { ai_score: 0.579000, score: 5547 },
  { ai_score: 0.064000, score: 5529 },
  { ai_score: 0.143000, score: 5526 },
  { ai_score: 0.000000, score: 5526 },
  { ai_score: 0.232000, score: 5522 },
  { ai_score: 0.516000, score: 5521 },
  { ai_score: 0.553000, score: 5518 },
  { ai_score: 0.000000, score: 5511 },
  { ai_score: 0.006000, score: 5505 },
  { ai_score: 0.423000, score: 5500 },
  { ai_score: 0.500000, score: 5503 },
  { ai_score: 0.000000, score: 5498 },
  { ai_score: 0.000000, score: 5499 },
  { ai_score: 0.000000, score: 5494 },
  { ai_score: 0.192000, score: 5493 },
  { ai_score: 0.281000, score: 5481 },
  { ai_score: 0.000000, score: 5458 },
  { ai_score: 0.022000, score: 5458 },
  { ai_score: 0.075000, score: 5460 },
  { ai_score: 0.391000, score: 5451 },
  { ai_score: 0.000000, score: 5454 },
  { ai_score: 0.587000, score: 5447 },
  { ai_score: 0.000000, score: 5434 },
  { ai_score: 0.255000, score: 5432 },
  { ai_score: 0.308000, score: 5431 },
  { ai_score: 0.000000, score: 5403 },
  { ai_score: 0.000000, score: 5391 },
  { ai_score: 0.418000, score: 5397 },
  { ai_score: 0.638000, score: 5390 },
  { ai_score: 0.000000, score: 5381 },
  { ai_score: 0.531000, score: 5380 },
  { ai_score: 0.000000, score: 5385 },
  { ai_score: 0.023000, score: 5369 },
  { ai_score: 0.420000, score: 5374 },
  { ai_score: 0.000000, score: 5365 },
  { ai_score: 0.185000, score: 5368 },
  { ai_score: 0.000000, score: 5362 },
  { ai_score: 0.000000, score: 5352 },
  { ai_score: 0.264000, score: 5348 },
  { ai_score: 0.241000, score: 5346 },
  { ai_score: 0.012000, score: 5336 },
  { ai_score: 0.000000, score: 5335 },
  { ai_score: 0.060000, score: 5327 },
  { ai_score: 0.000000, score: 5329 },
  { ai_score: 0.336000, score: 5313 },
  { ai_score: 0.582000, score: 5311 },
  { ai_score: 0.004000, score: 5300 },
  { ai_score: 0.021000, score: 5273 },
  { ai_score: 0.095000, score: 5272 },
  { ai_score: 0.807000, score: 5259 },
  { ai_score: 0.017000, score: 5257 },
  { ai_score: 0.586000, score: 5248 },
  { ai_score: 0.000000, score: 5249 },
  { ai_score: 0.000000, score: 5249 },
  { ai_score: 0.000000, score: 5242 },
  { ai_score: 0.285000, score: 5232 },
  { ai_score: 0.579000, score: 5245 },
  { ai_score: 0.047000, score: 5229 },
  { ai_score: 0.000000, score: 5228 },
  { ai_score: 0.000000, score: 5213 },
  { ai_score: 0.139000, score: 5214 },
  { ai_score: 0.020000, score: 5212 },
  { ai_score: 0.373000, score: 5202 },
  { ai_score: 0.000000, score: 5201 },
  { ai_score: 0.485000, score: 5198 },
  { ai_score: 0.590000, score: 5178 },
  { ai_score: 0.000000, score: 5172 },
  { ai_score: 0.220000, score: 5171 },
  { ai_score: 0.000000, score: 5162 },
  { ai_score: 0.464000, score: 5159 },
  { ai_score: 0.648000, score: 5161 },
  { ai_score: 0.189000, score: 5151 },
  { ai_score: 0.000000, score: 5146 },
  { ai_score: 0.322000, score: 5152 },
  { ai_score: 0.000000, score: 5146 },
  { ai_score: 0.340000, score: 5146 },
  { ai_score: 0.024000, score: 5140 },
  { ai_score: 0.067000, score: 5139 },
  { ai_score: 0.000000, score: 5138 },
  { ai_score: 0.389000, score: 5115 },
  { ai_score: 0.000000, score: 5099 },
  { ai_score: 0.000000, score: 5095 },
  { ai_score: 0.000000, score: 5087 },
  { ai_score: 0.031000, score: 5079 },
  { ai_score: 0.000000, score: 5063 },
  { ai_score: 0.030000, score: 5053 },
  { ai_score: 0.056000, score: 5056 },
  { ai_score: 0.172000, score: 5055 },
  { ai_score: 0.374000, score: 5035 },
  { ai_score: 0.290000, score: 5034 },
  { ai_score: 0.012000, score: 5021 },
  { ai_score: 0.049000, score: 5021 },
  { ai_score: 0.570000, score: 5019 },
  { ai_score: 0.000000, score: 5029 },
  { ai_score: 0.013000, score: 5016 },
  { ai_score: 0.000000, score: 5008 },
  { ai_score: 0.528000, score: 5007 },
  { ai_score: 0.000000, score: 5003 },
  { ai_score: 0.164000, score: 4994 },
  { ai_score: 0.721000, score: 4992 },
  { ai_score: 0.058000, score: 4988 },
  { ai_score: 0.000000, score: 4988 },
  { ai_score: 0.248000, score: 4962 },
  { ai_score: 0.000000, score: 4955 },
  { ai_score: 0.094000, score: 4957 },
  { ai_score: 0.000000, score: 4942 },
  { ai_score: 0.000000, score: 4944 },
  { ai_score: 0.000000, score: 4915 },
  { ai_score: 0.106000, score: 4916 },
  { ai_score: 0.000000, score: 4911 },
  { ai_score: 0.014000, score: 4903 },
  { ai_score: 0.575000, score: 4886 },
  { ai_score: 0.000000, score: 4882 },
  { ai_score: 0.540000, score: 4884 },
  { ai_score: 0.000000, score: 4874 },
  { ai_score: 0.633000, score: 4874 },
  { ai_score: 0.000000, score: 4874 },
  { ai_score: 0.000000, score: 4869 },
  { ai_score: 0.580000, score: 4864 },
  { ai_score: 0.521000, score: 4852 },
  { ai_score: 0.000000, score: 4852 },
  { ai_score: 0.149000, score: 4847 },
  { ai_score: 0.000000, score: 4837 },
  { ai_score: 0.000000, score: 4837 },
  { ai_score: 0.366000, score: 4831 },
  { ai_score: 0.172000, score: 4797 },
  { ai_score: 0.609000, score: 4788 },
  { ai_score: 0.000000, score: 4791 },
  { ai_score: 0.281000, score: 4787 },
  { ai_score: 0.000000, score: 4788 },
  { ai_score: 0.000000, score: 4792 },
  { ai_score: 0.179000, score: 4780 },
  { ai_score: 0.027000, score: 4786 },
  { ai_score: 0.212000, score: 4776 },
  { ai_score: 0.568000, score: 4779 },
  { ai_score: 0.000000, score: 4774 },
  { ai_score: 0.233000, score: 4768 },
  { ai_score: 0.000000, score: 4766 },
  { ai_score: 0.069000, score: 4762 },
  { ai_score: 0.022000, score: 4762 },
  { ai_score: 0.384000, score: 4752 },
  { ai_score: 0.530000, score: 4747 },
  { ai_score: 0.154000, score: 4749 },
  { ai_score: 0.356000, score: 4742 },
  { ai_score: 0.000000, score: 4736 },
  { ai_score: 0.519000, score: 4728 },
  { ai_score: 0.377000, score: 4715 },
  { ai_score: 0.123000, score: 4718 },
  { ai_score: 0.022000, score: 4722 },
  { ai_score: 0.151000, score: 4703 },
  { ai_score: 0.384000, score: 4697 },
  { ai_score: 0.000000, score: 4698 },
  { ai_score: 0.000000, score: 4688 },
  { ai_score: 0.332000, score: 4691 },
  { ai_score: 0.260000, score: 4684 },
  { ai_score: 0.210000, score: 4688 },
  { ai_score: 0.000000, score: 4680 },
  { ai_score: 0.067000, score: 4669 },
  { ai_score: 0.492000, score: 4665 },
  { ai_score: 0.589000, score: 4665 },
  { ai_score: 0.594000, score: 4664 },
  { ai_score: 0.000000, score: 4657 },
  { ai_score: 0.000000, score: 4647 },
  { ai_score: 0.000000, score: 4648 },
  { ai_score: 0.000000, score: 4642 },
  { ai_score: 0.104000, score: 4646 },
  { ai_score: 0.000000, score: 4628 },
  { ai_score: 0.000000, score: 4630 },
  { ai_score: 0.026000, score: 4625 },
  { ai_score: 0.607000, score: 4619 },
  { ai_score: 0.000000, score: 4607 },
  { ai_score: 0.349000, score: 4606 },
  { ai_score: 0.247000, score: 4605 },
  { ai_score: 0.128000, score: 4605 },
  { ai_score: 0.000000, score: 4602 },
  { ai_score: 0.577000, score: 4601 },
  { ai_score: 0.074000, score: 4603 },
  { ai_score: 0.013000, score: 4596 },
  { ai_score: 0.000000, score: 4582 },
  { ai_score: 0.184000, score: 4582 },
  { ai_score: 0.002000, score: 4574 },
  { ai_score: 0.000000, score: 4576 },
  { ai_score: 0.321000, score: 4574 },
  { ai_score: 0.000000, score: 4566 },
  { ai_score: 0.385000, score: 4560 },
  { ai_score: 0.047000, score: 4554 },
  { ai_score: 0.363000, score: 4541 },
  { ai_score: 0.000000, score: 4545 },
  { ai_score: 0.675000, score: 4540 },
  { ai_score: 0.014000, score: 4544 },
  { ai_score: 0.000000, score: 4530 },
  { ai_score: 0.000000, score: 4544 },
  { ai_score: 0.243000, score: 4534 },
  { ai_score: 0.000000, score: 4533 },
  { ai_score: 0.330000, score: 4530 },
  { ai_score: 0.286000, score: 4523 },
  { ai_score: 0.000000, score: 4518 },
  { ai_score: 0.000000, score: 4508 },
  { ai_score: 0.113000, score: 4489 },
  { ai_score: 0.380000, score: 4488 },
  { ai_score: 0.495000, score: 4478 },
  { ai_score: 0.000000, score: 4476 },
  { ai_score: 0.170000, score: 4463 },
  { ai_score: 0.000000, score: 4465 },
  { ai_score: 0.445000, score: 4462 },
  { ai_score: 0.444000, score: 4450 },
  { ai_score: 0.158000, score: 4456 },
  { ai_score: 0.586000, score: 4454 },
  { ai_score: 0.509000, score: 4447 },
  { ai_score: 0.054000, score: 4438 },
  { ai_score: 0.596000, score: 4442 },
  { ai_score: 0.000000, score: 4436 },
  { ai_score: 0.000000, score: 4435 },
  { ai_score: 0.000000, score: 4428 },
  { ai_score: 0.384000, score: 4419 },
  { ai_score: 0.404000, score: 4422 },
  { ai_score: 0.000000, score: 4417 },
  { ai_score: 0.000000, score: 4415 },
  { ai_score: 0.607000, score: 4408 },
  { ai_score: 0.590000, score: 4390 },
  { ai_score: 0.092000, score: 4390 },
  { ai_score: 0.032000, score: 4398 },
  { ai_score: 0.598000, score: 4390 },
  { ai_score: 0.261000, score: 4377 },
  { ai_score: 0.299000, score: 4379 },
  { ai_score: 0.026000, score: 4366 },
  { ai_score: 0.257000, score: 4363 },
  { ai_score: 0.440000, score: 4359 },
  { ai_score: 0.000000, score: 4347 },
  { ai_score: 0.000000, score: 4344 },
  { ai_score: 0.021000, score: 4344 },
  { ai_score: 0.000000, score: 4328 },
  { ai_score: 0.000000, score: 4321 },
];

  // ──────────────────────────────────────────────────

  // Prepare data: assign buckets
  const data = rawData
    .filter(d => d.ai_score != null && d.score != null)
    .map(d => ({
      ...d,
      label:
        d.ai_score <= 0.20 ? "Very Likely Human" :
        d.ai_score <= 0.40 ? "Probably Human" :
        d.ai_score <= 0.60 ? "Needs Review" :
        d.ai_score <= 0.80 ? "Likely AI" :
        "Very Likely AI"
    }));

  // Canvas
  const container = d3.select("#ai-vs-upvotes");
  const { width, height } = container.node().getBoundingClientRect();
  const margin = { top: 60, right: 20, bottom: 80, left: 80 };
  const w = width  - margin.left - margin.right;
  const h = height - margin.top  - margin.bottom;

  const svg = container.append("svg")
      .attr("viewBox", `0 0 ${width} ${height}`)
      .attr("preserveAspectRatio", "xMinYMin meet")
    .append("g")
      .attr("transform", `translate(${margin.left},${margin.top})`);

  // Scales
  const x = d3.scaleLinear().domain([0,1]).range([0, w]);
  const y = d3.scaleLinear()
    .domain([0, d3.max(data, d => d.score)]).nice()
    .range([h, 0]);

  // Axes
  svg.append("g")
      .attr("transform", `translate(0,${h})`)
      .call(d3.axisBottom(x).ticks(6))
    .append("text")
      .attr("class","axis-label")
      .attr("x", w/2).attr("y", 50)
      .style("text-anchor","middle")
      .text("Calibrated AI Score");

  svg.append("g")
      .call(d3.axisLeft(y).ticks(6, "~s"))
    .append("text")
      .attr("class","axis-label")
      .attr("transform","rotate(-90)")
      .attr("x",-h/2).attr("y",-60)
      .style("text-anchor","middle")
      .text("Upvotes");

  // Color scale
  const color = d3.scaleOrdinal()
    .domain([
      "Very Likely Human",
      "Probably Human",
      "Needs Review",
      "Likely AI",
      "Very Likely AI"
    ])
    .range(["#2ca02c","#98df8a","#bbbbbb","#ff7f0e","#d62728"]);

  // Points (draw *before* legend to sit underneath)
  svg.selectAll("circle")
    .data(data)
    .join("circle")
      .attr("cx", d => x(d.ai_score) + (Math.random()-0.5)*8)
      .attr("cy", d => y(d.score))
      .attr("r", 4)
      .attr("fill", d => color(d.label))
      .attr("opacity", 0.7);

  // Legend (draw last)
  const legend = svg.append("g")
    .attr("class","legend")
    .attr("transform", `translate(${w-160},0)`);
  color.domain().forEach((key,i) => {
    const g = legend.append("g").attr("transform", `translate(0,${i*20})`);
    g.append("rect").attr("width",12).attr("height",12).attr("fill",color(key));
    g.append("text").attr("x",18).attr("y",10).text(key);
  });

  // Title
  svg.append("text")
    .attr("x", w/2).attr("y",-30)
    .style("text-anchor","middle")
    .style("font-size","18px")
    .style("font-weight","bold")
    .text("AI Score vs. Upvotes");
});
</script>
{% endraw %}

The scatterplot above puts our label breakdown into context: almost all of the breakout 10K+ posts sit at the low end of the AI‐score spectrum (the green “human” buckets) or in the middle “Needs Review” zone.

Let's take a closer look at the underlying "model" I created (if you can call it that) and see how it works.

### An In-Depth Look at our AI Detector

To understand what the model is actually detecting, it helps to look under the hood. Each post in our dataset is assigned a raw score between 0 and 1 based on a weighted combination of ten text features, things like overuse of intensifiers, unnatural quote formatting, or generic sign-offs. This raw score is then normalized into an AI-likelihood score, calibrated so that scores below 0.20 are treated as “very likely human,” and anything above 0.80 is flagged as “very likely AI.”

<div id="aita-score-histogram" style="width:100%; height:auto; margin:auto;"></div>

{% raw %}
<style>
  #aita-score-histogram svg { width:100%; height:100%; font-family:sans-serif; }
  .aita-hist-bar       { fill: #FF4500; opacity: 0.6; }
  .aita-density-curve  { fill: none; stroke: crimson; stroke-width: 2px; }
  .aita-axis path, .aita-axis line { stroke: #999; }
  .aita-axis text      { font-size: 10px; }
</style>

<script src="https://d3js.org/d3.v7.min.js"></script>
<script>
document.addEventListener("DOMContentLoaded", () => {
  // **Make sure you replace this with your real 996‐element array**
  const aitaScores = [
  0.000,
  0.386,
  0.109,
  0.007,
  0.052,
  0.401,
  0.437,
  0.000,
  0.603,
  0.000,
  0.037,
  0.035,
  0.000,
  0.590,
  0.129,
  0.083,
  0.243,
  0.471,
  0.000,
  0.637,
  0.000,
  0.038,
  0.071,
  0.000,
  0.000,
  0.000,
  0.000,
  0.485,
  0.374,
  0.000,
  0.629,
  0.000,
  0.152,
  0.253,
  0.000,
  0.000,
  0.000,
  0.588,
  0.000,
  0.067,
  0.055,
  0.002,
  0.259,
  0.092,
  0.014,
  0.106,
  0.000,
  0.590,
  0.593,
  0.000,
  0.000,
  0.000,
  0.000,
  0.022,
  0.000,
  0.721,
  0.000,
  0.260,
  0.590,
  0.000,
  0.028,
  0.000,
  0.101,
  0.000,
  0.030,
  0.427,
  0.441,
  0.000,
  0.361,
  0.605,
  0.593,
  0.000,
  0.204,
  0.197,
  0.016,
  0.590,
  0.007,
  0.000,
  0.301,
  0.233,
  0.471,
  0.002,
  0.000,
  0.205,
  0.768,
  0.594,
  0.278,
  0.365,
  0.045,
  0.060,
  0.403,
  0.090,
  0.261,
  0.625,
  0.000,
  0.291,
  0.374,
  0.593,
  0.579,
  0.082,
  0.565,
  0.004,
  0.259,
  0.592,
  0.000,
  0.026,
  0.464,
  0.000,
  0.344,
  0.583,
  0.203,
  0.392,
  0.349,
  0.215,
  0.000,
  0.000,
  0.602,
  0.028,
  0.000,
  0.000,
  0.000,
  0.645,
  0.000,
  0.000,
  0.218,
  0.000,
  0.225,
  0.587,
  0.000,
  0.581,
  0.552,
  0.000,
  0.000,
  0.071,
  0.460,
  0.000,
  0.000,
  0.048,
  0.067,
  0.191,
  0.189,
  0.197,
  0.209,
  0.000,
  0.082,
  0.000,
  0.628,
  0.000,
  0.459,
  0.000,
  0.384,
  0.000,
  0.374,
  0.592,
  0.586,
  0.000,
  0.700,
  0.000,
  0.152,
  0.000,
  0.109,
  0.587,
  0.125,
  0.032,
  0.000,
  0.164,
  0.234,
  0.294,
  0.000,
  0.409,
  0.184,
  0.429,
  0.431,
  0.000,
  0.000,
  0.000,
  0.259,
  0.342,
  0.000,
  0.000,
  0.009,
  0.002,
  0.113,
  0.547,
  0.000,
  0.000,
  0.057,
  0.000,
  0.000,
  0.000,
  0.588,
  0.199,
  0.329,
  0.437,
  0.592,
  0.000,
  0.000,
  0.000,
  0.132,
  0.216,
  0.000,
  0.000,
  0.594,
  0.000,
  0.097,
  0.747,
  0.000,
  0.582,
  0.380,
  0.209,
  0.000,
  0.000,
  0.609,
  0.225,
  0.679,
  0.624,
  0.390,
  0.595,
  0.094,
  0.000,
  0.639,
  0.003,
  0.000,
  0.000,
  0.635,
  0.000,
  0.593,
  0.020,
  0.260,
  0.000,
  0.000,
  0.000,
  0.000,
  0.099,
  0.000,
  0.261,
  0.000,
  0.000,
  0.221,
  0.007,
  0.580,
  0.000,
  0.212,
  0.377,
  0.410,
  0.000,
  0.352,
  0.000,
  0.135,
  0.382,
  0.702,
  0.327,
  0.296,
  0.000,
  0.011,
  0.506,
  0.577,
  0.749,
  0.324,
  0.206,
  0.265,
  0.658,
  0.000,
  0.000,
  0.000,
  0.028,
  0.154,
  0.402,
  0.000,
  0.576,
  0.309,
  0.183,
  0.000,
  0.269,
  0.000,
  0.000,
  0.425,
  0.000,
  0.036,
  0.000,
  0.000,
  0.071,
  0.356,
  0.000,
  0.568,
  0.147,
  0.000,
  0.345,
  0.000,
  0.441,
  0.000,
  0.579,
  0.000,
  0.320,
  0.268,
  0.176,
  0.000,
  0.000,
  0.588,
  0.122,
  0.439,
  0.613,
  0.235,
  0.405,
  0.243,
  0.000,
  0.097,
  0.586,
  0.673,
  0.125,
  0.581,
  0.567,
  0.006,
  0.000,
  0.598,
  0.581,
  0.370,
  0.443,
  0.586,
  0.368,
  0.699,
  0.000,
  0.000,
  0.585,
  0.006,
  0.309,
  0.474,
  0.589,
  0.589,
  0.504,
  0.038,
  0.596,
  0.019,
  0.044,
  0.145,
  0.587,
  0.000,
  0.057,
  0.016,
  0.294,
  0.168,
  0.052,
  0.000,
  0.232,
  0.532,
  0.478,
  0.359,
  0.329,
  0.572,
  0.283,
  0.000,
  0.000,
  0.253,
  0.021,
  0.528,
  0.313,
  0.000,
  0.320,
  0.107,
  0.012,
  0.595,
  0.000,
  0.065,
  0.195,
  0.406,
  0.000,
  0.098,
  0.129,
  0.598,
  0.386,
  0.438,
  0.011,
  0.000,
  0.089,
  0.000,
  0.051,
  0.441,
  0.000,
  0.000,
  0.201,
  0.000,
  0.407,
  0.000,
  0.240,
  0.579,
  0.000,
  0.433,
  0.030,
  0.272,
  0.000,
  0.353,
  0.668,
  0.155,
  0.000,
  0.015,
  0.000,
  0.000,
  0.000,
  0.202,
  0.598,
  0.589,
  0.000,
  0.112,
  0.000,
  0.000,
  0.000,
  0.000,
  0.538,
  0.106,
  0.291,
  0.243,
  0.337,
  0.000,
  0.041,
  0.159,
  0.174,
  0.254,
  0.392,
  0.548,
  0.070,
  0.194,
  0.585,
  0.000,
  0.000,
  0.590,
  0.292,
  0.000,
  0.000,
  0.444,
  0.000,
  0.486,
  0.554,
  0.173,
  0.031,
  0.000,
  0.000,
  0.582,
  0.450,
  0.000,
  0.368,
  0.000,
  0.276,
  0.654,
  0.324,
  0.000,
  0.592,
  0.549,
  0.110,
  0.000,
  0.000,
  0.000,
  0.000,
  0.481,
  0.068,
  0.143,
  0.028,
  0.234,
  0.562,
  0.383,
  0.213,
  0.021,
  0.395,
  0.136,
  0.510,
  0.000,
  0.480,
  0.607,
  0.145,
  0.601,
  0.097,
  0.160,
  0.059,
  0.200,
  0.000,
  0.504,
  0.591,
  0.000,
  0.247,
  0.170,
  0.000,
  0.174,
  0.000,
  0.579,
  0.000,
  0.000,
  0.000,
  0.354,
  0.000,
  0.119,
  0.445,
  0.118,
  0.322,
  0.000,
  0.391,
  0.286,
  0.341,
  0.000,
  0.400,
  0.102,
  0.578,
  0.000,
  0.069,
  0.373,
  0.051,
  0.000,
  0.190,
  0.000,
  0.177,
  0.036,
  0.000,
  0.013,
  0.003,
  0.596,
  0.306,
  0.000,
  0.598,
  0.397,
  0.553,
  0.000,
  0.044,
  0.000,
  0.194,
  0.000,
  0.000,
  0.000,
  0.108,
  0.000,
  0.346,
  0.000,
  0.073,
  0.000,
  0.000,
  0.000,
  0.000,
  0.585,
  0.000,
  0.211,
  0.000,
  0.509,
  0.145,
  0.519,
  0.000,
  0.433,
  0.008,
  0.000,
  0.358,
  0.587,
  0.227,
  0.000,
  0.585,
  0.000,
  0.000,
  0.219,
  0.091,
  0.031,
  0.000,
  0.040,
  0.376,
  0.000,
  0.190,
  0.586,
  0.005,
  0.590,
  0.582,
  0.000,
  0.297,
  0.432,
  0.598,
  0.249,
  0.228,
  0.000,
  0.000,
  0.000,
  0.584,
  0.000,
  0.120,
  0.000,
  0.000,
  0.068,
  0.000,
  0.567,
  0.000,
  0.000,
  0.000,
  0.579,
  0.000,
  0.599,
  0.552,
  0.556,
  0.000,
  0.000,
  0.399,
  0.000,
  0.125,
  0.356,
  0.112,
  0.000,
  0.443,
  0.000,
  0.000,
  0.631,
  0.000,
  0.366,
  0.374,
  0.025,
  0.221,
  0.000,
  0.642,
  0.000,
  0.000,
  0.393,
  0.278,
  0.056,
  0.421,
  0.390,
  0.598,
  0.000,
  0.000,
  0.027,
  0.581,
  0.338,
  0.477,
  0.097,
  0.061,
  0.000,
  0.228,
  0.456,
  0.000,
  0.489,
  0.199,
  0.000,
  0.000,
  0.000,
  0.691,
  0.244,
  0.000,
  0.102,
  0.003,
  0.000,
  0.000,
  0.000,
  0.645,
  0.595,
  0.183,
  0.000,
  0.000,
  0.183,
  0.096,
  0.388,
  0.175,
  0.000,
  0.000,
  0.583,
  0.023,
  0.236,
  0.130,
  0.334,
  0.190,
  0.280,
  0.249,
  0.000,
  0.000,
  0.603,
  0.582,
  0.372,
  0.612,
  0.583,
  0.000,
  0.010,
  0.321,
  0.000,
  0.080,
  0.671,
  0.000,
  0.138,
  0.000,
  0.000,
  0.210,
  0.000,
  0.000,
  0.405,
  0.000,
  0.355,
  0.000,
  0.228,
  0.354,
  0.335,
  0.600,
  0.000,
  0.000,
  0.000,
  0.591,
  0.000,
  0.000,
  0.098,
  0.359,
  0.000,
  0.353,
  0.085,
  0.271,
  0.000,
  0.000,
  0.153,
  0.416,
  0.000,
  0.000,
  0.239,
  0.000,
  0.512,
  0.000,
  0.277,
  0.596,
  0.454,
  0.065,
  0.398,
  0.000,
  0.004,
  0.003,
  0.457,
  0.035,
  0.051,
  0.493,
  0.595,
  0.333,
  0.346,
  0.368,
  0.000,
  0.000,
  0.720,
  0.266,
  0.078,
  0.016,
  0.588,
  0.000,
  0.289,
  0.467,
  0.184,
  0.000,
  0.608,
  0.000,
  0.384,
  0.000,
  0.000,
  0.596,
  0.258,
  0.602,
  0.329,
  0.012,
  0.403,
  0.071,
  0.053,
  0.422,
  0.013,
  0.018,
  0.349,
  0.467,
  0.302,
  0.000,
  0.000,
  0.000,
  0.408,
  0.000,
  0.375,
  0.579,
  0.064,
  0.143,
  0.000,
  0.232,
  0.516,
  0.553,
  0.000,
  0.006,
  0.423,
  0.500,
  0.000,
  0.000,
  0.000,
  0.192,
  0.281,
  0.000,
  0.022,
  0.075,
  0.391,
  0.000,
  0.587,
  0.000,
  0.255,
  0.308,
  0.000,
  0.000,
  0.418,
  0.638,
  0.000,
  0.531,
  0.000,
  0.023,
  0.420,
  0.000,
  0.185,
  0.000,
  0.000,
  0.264,
  0.241,
  0.012,
  0.000,
  0.060,
  0.000,
  0.336,
  0.582,
  0.004,
  0.021,
  0.095,
  0.807,
  0.017,
  0.586,
  0.000,
  0.000,
  0.000,
  0.285,
  0.579,
  0.047,
  0.000,
  0.000,
  0.139,
  0.020,
  0.373,
  0.000,
  0.485,
  0.590,
  0.000,
  0.220,
  0.000,
  0.464,
  0.648,
  0.189,
  0.000,
  0.322,
  0.000,
  0.340,
  0.024,
  0.067,
  0.000,
  0.389,
  0.000,
  0.000,
  0.000,
  0.031,
  0.000,
  0.030,
  0.056,
  0.172,
  0.374,
  0.290,
  0.012,
  0.049,
  0.570,
  0.000,
  0.013,
  0.000,
  0.528,
  0.000,
  0.164,
  0.721,
  0.058,
  0.000,
  0.248,
  0.000,
  0.094,
  0.000,
  0.000,
  0.000,
  0.106,
  0.000,
  0.014,
  0.575,
  0.000,
  0.540,
  0.000,
  0.633,
  0.000,
  0.000,
  0.580,
  0.521,
  0.000,
  0.149,
  0.000,
  0.000,
  0.366,
  0.172,
  0.609,
  0.000,
  0.281,
  0.000,
  0.000,
  0.179,
  0.027,
  0.212,
  0.568,
  0.000,
  0.233,
  0.000,
  0.069,
  0.022,
  0.384,
  0.530,
  0.154,
  0.356,
  0.000,
  0.519,
  0.377,
  0.123,
  0.022,
  0.151,
  0.384,
  0.000,
  0.000,
  0.332,
  0.260,
  0.210,
  0.000,
  0.067,
  0.492,
  0.589,
  0.594,
  0.000,
  0.000,
  0.000,
  0.000,
  0.104,
  0.000,
  0.000,
  0.026,
  0.607,
  0.000,
  0.349,
  0.247,
  0.128,
  0.000,
  0.577,
  0.074,
  0.013,
  0.000,
  0.184,
  0.002,
  0.000,
  0.321,
  0.000,
  0.385,
  0.047,
  0.363,
  0.000,
  0.675,
  0.014,
  0.000,
  0.000,
  0.243,
  0.000,
  0.330,
  0.286,
  0.000,
  0.000,
  0.113,
  0.380,
  0.495,
  0.000,
  0.170,
  0.000,
  0.445,
  0.444,
  0.158,
  0.586,
  0.509,
  0.054,
  0.596,
  0.000,
  0.000,
  0.000,
  0.384,
  0.404,
  0.000,
  0.000,
  0.607,
  0.590,
  0.092,
  0.032,
  0.598,
  0.261,
  0.299,
  0.026,
  0.257,
  0.440,
  0.000,
  0.000,
  0.021,
  0.000,
  0.000,
];


  const margin    = {top:20, right:30, bottom:40, left:50};
  const totalW    = 700, totalH = 400;
  const width     = totalW - margin.left - margin.right;
  const height    = totalH - margin.top  - margin.bottom;

  const svg = d3.select("#aita-score-histogram")
    .append("svg")
      .attr("viewBox", `0 0 ${totalW} ${totalH}`)
      .attr("preserveAspectRatio", "xMinYMin meet")
    .append("g")
      .attr("transform", `translate(${margin.left},${margin.top})`);

  const xScale = d3.scaleLinear().domain([0,1]).range([0, width]);
  const bins   = d3.bin()
                   .domain(xScale.domain())
                   .thresholds(xScale.ticks(30))(aitaScores);
  const yScale = d3.scaleLinear()
                   .domain([0, d3.max(bins, d=>d.length)]).nice()
                   .range([height,0]);

  // Bars
  svg.append("g")
    .selectAll("rect")
    .data(bins)
    .join("rect")
      .attr("class","aita-hist-bar")
      .attr("x", d=> xScale(d.x0)+1)
      .attr("y", d=> yScale(d.length))
      .attr("width", d=> Math.max(0, xScale(d.x1)-xScale(d.x0)-1))
      .attr("height", d=> height - yScale(d.length));

  // KDE
  const epane = k => v => Math.abs(v /= k) <= 1 ? 0.75*(1-v*v)/k : 0;
  const kde    = (kernel, X) => V => X.map(x=>[x, d3.mean(V, v=>kernel(x-v))]);
  const density = kde(epane(0.05), xScale.ticks(100))(aitaScores);
  const yDens   = d3.scaleLinear()
                    .domain([0, d3.max(density, d=>d[1])])
                    .range([height,0]);
  const line = d3.line()
                 .curve(d3.curveBasis)
                 .x(d=> xScale(d[0]))
                 .y(d=> yDens(d[1]));
  svg.append("path")
     .datum(density)
     .attr("class","aita-density-curve")
     .attr("d", line);

  // Axes
  svg.append("g")
     .attr("class","aita-axis")
     .attr("transform",`translate(0,${height})`)
     .call(d3.axisBottom(xScale).ticks(10).tickFormat(d3.format(".2f")))
    .append("text")
     .attr("x", width/2).attr("y", 35)
     .style("text-anchor","middle")
     .text("Calibrated AI Score");

  svg.append("g")
     .attr("class","aita-axis")
     .call(d3.axisLeft(yScale).ticks(5))
    .append("text")
     .attr("transform","rotate(-90)")
     .attr("x",-height/2).attr("y",-40)
     .style("text-anchor","middle")
     .text("Number of Posts");

  svg.append("text")
     .attr("x", width/2).attr("y",-5)
     .style("font-size","16px").style("font-weight","bold")
     .style("text-anchor","middle")
     .text("Distribution of r/AmItheAsshole AI-Likelihood Scores");
});
</script>
{% endraw %}

Across 996 top r/AmItheAsshole posts, the mean raw score is 0.267 and the mean calibrated AI score is 0.207. Well below our 0.40 “Needs Review” cutoff and solidly in the human-leaning territory. 

Here’s the full breakdown by label:

| Label             | Count | Mean Raw | Mean AI | Std Dev (AI) |
| ----------------- | ----: | -------: | ------: | -----------: |
| Very Likely AI    |     1 |    0.826 |   0.807 |            — |
| Likely AI         |    45 |    0.686 |   0.651 |        0.045 |
| Needs Review      |   188 |    0.577 |   0.530 |        0.067 |
| Probably Human    |   187 |    0.374 |   0.304 |        0.061 |
| Very Likely Human |   575 |    0.098 |   0.033 |        0.056 |

Notably, only the lone "Very Likely AI" outlier (0.807) and the 45 "Likely AI" posts (mean AI ~ 0.65) push past our 0.60 threshold. The large "Needs Review" group (mean ~ 0.53) occupies that gray zone of polished but still-ambiguous prose, while the vast majority of submissions score unmistakably human.

### Looking at Another Subreddit

To make sure r/AmItheAsshole wasn't a fluke, we ran our detector over 1,000 of this year's top posts from both r/AmItheAsshole and r/AITAH. These two communities cover essentially the same type of content, confession‑drama, moral showdowns, and that kind of thing. But r/AITAH is a bit more lax about moderation, making it a natural second test case.

Here's what we found:

<div id="subreddit-ai-share" style="width:100%;max-width:350px;height:250px;margin:auto;"></div>

<script src="https://d3js.org/d3.v7.min.js"></script>
<script>
(function(){
  // — your data —
  const shareData = [
    { subreddit:"r/AmItheAsshole", ai:0.235, human:0.765 },
    { subreddit:"r/AITAH", ai:0.316, human:0.684 }
  ];

  // — dimensions & container —
  const container = d3.select("#subreddit-ai-share");
  const W = parseInt(container.style("max-width"));
  const H = parseInt(container.style("height"));
  const margin = {top:40,right:20,bottom:50,left:60};
  const innerW = W - margin.left - margin.right;
  const innerH = H - margin.top - margin.bottom;

  const svg = container.append("svg")
      .attr("width",W).attr("height",H)
    .append("g")
      .attr("transform",`translate(${margin.left},${margin.top})`);

  // — scales & stack —
  const x = d3.scaleBand()
      .domain(shareData.map(d=>d.subreddit))
      .range([0,innerW]).padding(0.4);

  const y = d3.scaleLinear()
      .domain([0,1])
      .range([innerH,0]);

  const stackGen = d3.stack().keys(["human","ai"]);
  const layers = stackGen( shareData.map(d=>({human:d.human,ai:d.ai})) );

  const color = d3.scaleOrdinal()
      .domain(["human","ai"])
      .range(["#A6A6A6","#FF4500"]);

  // — draw bars —
  const layerG = svg.selectAll("g.layer")
    .data(layers)
    .join("g")
      .attr("fill",d=>color(d.key));

  layerG.selectAll("rect")
    .data(d=>d)
    .join("rect")
      .attr("x", (d,i)=> x(shareData[i].subreddit) )
      .attr("y", d=> y(d[1]) )
      .attr("height", d=> y(d[0]) - y(d[1]) )
      .attr("width",  x.bandwidth());

  // — add direct % labels —
  // AI label inside top segment
  svg.selectAll("text.ai-label")
    .data(shareData)
    .join("text")
      .attr("class","ai-label")
      .attr("x", d=> x(d.subreddit) + x.bandwidth()/2 )
      .attr("y", d=> y(d.human + d.ai/2) )
      .attr("text-anchor","middle")
      .attr("fill","#fff")
      .style("font-weight","bold")
      .text(d=> `${Math.round(d.ai*100)}% AI`);

  // Human label inside bottom segment
  svg.selectAll("text.hum-label")
    .data(shareData)
    .join("text")
      .attr("class","hum-label")
      .attr("x", d=> x(d.subreddit) + x.bandwidth()/2 )
      .attr("y", d=> y(d.human/2) )
      .attr("text-anchor","middle")
      .attr("fill","#222")
      .style("font-size","12px")
      .text(d=> `${Math.round(d.human*100)}% Human`);

  // — axes & title —
  svg.append("g")
      .attr("transform",`translate(0,${innerH})`)
      .call(d3.axisBottom(x))
      .selectAll("text")
        .attr("transform","rotate(-20)")
        .style("text-anchor","end");

  svg.append("g")
      .call(d3.axisLeft(y).tickFormat(d3.format(".0%")));

  svg.append("text")
      .attr("x", innerW/2).attr("y", -10)
      .attr("text-anchor","middle")
      .style("font-size","16px").style("font-weight","bold");
})();
</script>

We found even more AI content in r/AITAH!

You might shrug off a handful of AI‑penned posts as just "karma farming," but when 32% of r/AITAH’s top posts read like AI, the implications go deeper. 

If this level of synthetic noise is so pervasive in a very a popular corner of Reddit, imagine what it looks like across dozens of other communities too? Or other social platforms without as much moderation or discerning readers. 

AI is quietly flooding the feeds, warping discussions, and hollowing out the content. Maybe we should have an AI sell a course on how you can get AI to sell courses to other AI... No, but we're getting closer.

That’s the existential worry at the heart of Dead Internet Theory: when your fellow "users" or "netizens" start to feel more like ChatGPT than people, the internet loses its soul. If it ever really had one. Fake, it starts to feel more fake. 

So why spend time there?

## Dead Internet Theory

[Dead Internet Theory](https://en.wikipedia.org/wiki/Dead_Internet_theory) is a semi-ironic conspiracy theory that emerged in fringe internet spaces around 2016 or 2017. At its core, it makes the bold claim that the majority of online activity including posts, comments, even people are no longer real. Instead, the internet has become a ghost town visited by bots and generated by corporate interests and AI.

Some versions of the theory go further, alleging that this shift was orchestrated by governments or intelligence agencies to control narratives. But even stripped of that paranoia, the theory taps into something many internet users feel.

That the [web feels different now](https://arxiv.org/abs/2502.00007).

Yes, the number of users has gone up:

<iframe src="https://ourworldindata.org/grapher/number-of-internet-users?tab=chart" loading="lazy" style="width: 100%; height: 600px; border: 0px none;" allow="web-share; clipboard-write"></iframe>

But something else feels off about the web. According to adherents of Dead Internet Theory, this "death" of the human internet happened quietly sometime between 2016 and 2018. Engagement has stayed high, but the platforms have remained distinctly less human. The growth of scams online is one indicator of this. The volume of spam accounts compared to legitimate users has grown. 

Just look at the sheer volume of accounts facebook has had to remove (source: [Fraud Blocker](https://fraudblocker.com/articles/instagram-spam-bots-how-to-stop-the-madness)):

<img src="https://mllehmdz0sx3.i.optimole.com/w:auto/h:auto/q:mauto/f:best/ig:avif/https://fraudblocker.com/wp-content/uploads/2023/10/how-many-fake-accounts-are-on-Instagram-1.png" alt="Emarketer Facebook Accounts Fake Data" loading="lazy" width="100%" height="auto" />

Or Twitter/X (source: [SparkToro](https://sparktoro.com/blog/sparktoro-followerwonk-joint-twitter-analysis-19-42-of-active-accounts-are-fake-or-spam/)):

<img src="https://images.sparktoro.com/blog/wp-content/uploads/2022/05/percent-of-twitter-fake-or-spam-1638-1.png" alt="SparkToro Twitter Accounts Fake Data" loading="lazy" width="100%" height="auto" />

As our analysis has shown, around 25% - 30% of the top posts on two subreddits are probably AI! Which actually matches the [estimated usage of AI chatbots from 18-24 year olds from Emarketer](https://www.emarketer.com/content/facebook-instagram-introduce-ai-bot--user--accounts-10):

<img src="https://www.emarketer.com/content/storage/52fc82e614f9487697e1ef2851a8995a/287281" alt="Emarketer Data" loading="lazy" width="100%" height="auto" />

On its face, the Dead Internet Theory seems extreme. But that’s exactly why our findings here are so strange! We didn’t set out to prove or disprove Dead Internet Theory. And to be clear, we haven't. But we've definitely stumbled on something that implies it may be coming true.

Most of the posts on [r/AmItheAsshole](https://www.reddit.com/r/AmItheAsshole/) read as synthetic copies of each other, because they are. It's a forum with a defined and agreed upon informal template. You state your age and gender, describe the problem. Maybe add some quotes. And try to get some feedback. Useful for real life people for sure. But also a great way to test out a bot. Or gain some easy karma for an account you'd like to use later. It's really hard to say if these posts are AI or not! But they definitely seem more manufactured than not.

This doesn’t "prove" Dead Internet Theory in the conspiratorial sense. But it does suggest the internet is drifting into a state where the appearance of human interaction persists, but much of the underlying substance is generated, gamed, or manufactured.

So is Dead Internet Theory true?

Not literally. There are still real people online. Still real conversations. Still weird, beautiful, chaotic things happening in the margins. The majority of posts are still being made by humans. But enough of the top posts, and surely more of the daily posts, are being made by AI to make us question our sanity, and if we're really getting value out of spending time on social media.

Reddit isn’t dead. But it's becoming hollow. Filled with fake users. 

Just like how it began (cue circle of life reference).

## The Future of the Web

We’re entering an era where even human posts are written with the help of AI. Not by deception, but by delegation. More and more, people are running their thoughts, emails, Reddit replies, and even apologies through ChatGPT. If detection models flag those as “AI,” they're not wrong, but they're not exactly right either. The boundary between human and AI is blurry, at best.

In the near future, AI won't just impersonate humans, it will act on behalf of them. An OpenAI “Operator” might craft and post your sarcastic Reddit comments while you’re at work. Your chatbot might write your angry tweets for you. The post is still "yours," but your fingers never touched the keyboard. So if you detect it as AI, is that true? Maybe.

Now in my mind that's a hellish nightmare universe. Why would you want AI to post for you while you're still at work... Why can't it just work for me? Oh shit that's right, social media wants to have us all selling courses (or something like that) by 2030, so in a sense AI is doing the work.

Meanwhile, institutions, from marketers to academics, still mine Reddit and similar platforms to gauge public opinion. But as more content is synthetic, the signal is diluted. If the training data becomes too noisy, the insights degrade. That has real implications. Policy papers, marketing plans, media narratives could start relying on what are essentially hallucinated sentiments from LLMs. Ironically, the same AI that generated the noise will likely be tasked with interpreting it.

The same pattern is playing out across the internet:

- Search engines are bloating with SEO-optimized junk generated by AI.
- Social platforms are incentivize engagement at all costs, even if that engagement comes from bots.
- Actual humans are increasingly retreating to private group chats, DMs, and niche communities.

But platforms need growth, and synthetic users are technically growth. They fill timelines, juice metrics, and keep the investors happy. We may not be living in a fully "dead internet" just yet. But we do have an internet optimized purely for engagement metrics.

The last decade promised an internet that was more human, more social, more connected, more personal. But the next decade will be different: authenticity must be sought, not assumed.

<hr>

<div class="bibliography">
  <h2>Methods</h2>
  <ul>
    <li>Fetched top 1,000 posts from <code>r/AmItheAsshole</code> and <code>r/AITAH</code> via Reddit</li>
    <li>Extracted titles, bodies, and upvote counts</li>
    <li>Built a heuristic AI-detector in Python using regular expressions, NLTK tokenization, and VADER sentiment analysis</li>
    <li>Computed raw AI-likelihood scores from ten text-features, then calibrated them into human- vs AI-labels</li>
    <li>Visualized label distributions and score vs upvotes scatterplots with Matplotlib</li>
  </ul>

  <h2>Bibliography</h2>
  <ul>
    <li><a href="https://www.youtube.com/watch?v=OZz0GlpPlv8">Neon Sharpe: How to Identify Bots on Reddit</a></li>
    <li><a href="https://www.nltk.org/">NLTK: Natural Language Toolkit</a></li>
    <li><a href="https://github.com/cjhutto/vaderSentiment">VADER Sentiment Analysis</a></li>
    <li><a href="https://reddit.com/r/AmItheAsshole/top/?sort=top&amp;t=year">r/AmItheAsshole top posts (year)</a></li>
    <li><a href="https://reddit.com/r/AITAH/top/?sort=top&amp;t=year">r/AITAH top posts (year)</a></li>
  </ul>

  Data pulled on 5/14/2025
</div>
