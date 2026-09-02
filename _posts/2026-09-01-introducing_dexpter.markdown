---
layout: post
title:  "Introducing dexpter: A Lightweight Experiment Tracking Library"
date:   2026-09-01
last_modified_at: 2026-09-01
categories: [economics]
pinned: false
mathjax: true
---

&nbsp;

For a long time I've struggled with the problem of tracking data science type work
(whether it be statistical, ML, econometrics, etc.). Yes, tools like MLFlow exist.
The problem with MLFlow and similar tools, though, is that they really work best
once you're dealing with an established mature project where core data, metrics, goals,
and what not have been established. Such tools, at least imho, have never really been
great for in flight work that hasn't reached V1 yet. Early on in data science projects
things can change by the day as you figure stuff out - there is no well defined
schema of things to track. Heck, sometimes part of early work is figuring out what
is *worth* tracking! For too long I've handled this somewhat haphazardly...I've tried
adapting MLFlow, I've tried notebooks, I've tried throwing stuff in Obsidean, and so on
and so on. But none of this was ever really ideal.

&nbsp;

Over the weekend, I decided to see if I could take a crack at seeing if me and Claude[^1]
could build out something that could solve my problem. The result: dexpter (**D**ata
Science **Exp**eriment **T**rack**er**), a Python library that provides a lightweight, flexible JSON backend for tracking data science work (of course, it could track other work too!). Emphasis on lightweight and flexible: With respect to the former, dexpter uses
*only* the standard Python library. Whereas tools like MLFlow have upwards of 20 declared dependencies, dexpter has 0. And with respect to the latter, dexpter has minimal schema enforcing. Whereas tools like MLflow make force rigid data schemas, dexpter is just a
free-form JSON record you shape however the project needs today. Now make no mistake - these are not knocks against the likes of MLFlow. As alluded to above, for well established projects, the structure provided by such
tools is excellent. But when you're in uncharted territory - it can be a lot of
overhead for things that might be vastly different in a week. Its the latter type work
that dexpter is for. It isn't meant to be your production tracking system - but rather,
its meant to be an easy, low stakes tool for tracking work during early development.

&nbsp;

If this sounds like it might be useful for you, you can install it now with `pip`

&nbsp;

```
pip install dexpter
```

&nbsp;

or you can install from source:

&nbsp;

```
git clone https://github.com/rjapplin/dexpter
cd dexpter
pip install .
```

&nbsp;

You can find all the documentation in the github repo: [https://github.com/rjapplin/dexpter](https://github.com/rjapplin/dexpter).

---

[^1]: Yes, dexpter is mostly vibe-coded. If you're not down with that - totally fair! dexpter does what I need it to do and does it good enough. If you try it out and find it useful, that's great! If you try it out and think it sucks, that's also great!
