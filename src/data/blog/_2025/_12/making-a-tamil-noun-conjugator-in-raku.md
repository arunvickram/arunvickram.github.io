---
title: Making a Tamil Noun Conjugator in Raku
author: Arun Vickram
pubDatetime: 2025-12-24T01:34:26.491Z
slug: making-a-tamil-noun-conjugator-in-raku
featured: true
draft: true
tags:
  - raku
  - tamil
  - noun
  - perl
# ogImage: ../../assets/images/Camelia-Raku.png # src/assets/images/example.png
ogImage: "https://upload.wikimedia.org/wikipedia/commons/thumb/8/85/Camelia.svg/2880px-Camelia.svg.png" # remote URL
description: Easily defining your domain problem using the Swiss army knife of programming language
# canonicalURL: https://example.org/my-article-was-already-posted-here
---

I'm working on a dictionary web application for all of South Asia's languages. The reason is it can be used as a learning tool for students or 
anyone looking to learn a South Asian language.

In doing so, I started working on a bunch of scripts to help me generate entries and sample data for the dictionary, starting with my
heritage tongue, Tamil.

Tamil is a Dravidian language native to India and the northeastern coasts of the island of Lanka, and it's spoken in other countries like
Malaysia, Singapore, and South Africa, as well as other places where diaspora Tamils live.

There is a lot of regularity in the Tamil grammar that makes it easy to program conjugators for it. This weekend I was working on a Tamil noun conjugator.

I decided to do it in Raku for a couple of reasons:
- [I've decided to go all in on Raku.](#)
- 

Tamil nouns can largely be classified into four major categories based on their endings:

- nouns ending in -அம் (words like தாகம், அகம், அறம், இடம், திட்டம், etc.)
- nouns ending in -டு or -று (words like நாடு, காடு, வரலாறு, ஆறு, etc.)
- words like சில (meaning "few"), பல (meaning "many"), எல்லா (meaning "all")
- every other noun

```raku
subset AmNoun of Str where / <?after any('க'..'வ')> ம் $ /;
subset TtuNoun of Str where / <!after ட்> டு $ /;
subset TruNoun of Str where / <!after ற்> று $ /;
subset GroupNoun of Str where 'சில' | 'பல' | 'எல்லா';
```
