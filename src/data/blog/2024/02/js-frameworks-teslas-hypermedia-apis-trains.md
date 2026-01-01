---
slug: js-frameworks-are-teslas-hypermedia-apis-are-trains
title: React, Svelte, Solid, etc. are EVs. HTMX, Alpine, Unpoly are MagLev Trains.
author: Arun Vickram
featured: true
draft: false
description: A commentary on both the absymal state of web development and the United States' crumbling infrastructure.
tags:
  - javascript
  - tesla
  - htmx
pubDatetime: 2024-02-09T01:34:26.491Z
ogImage: "https://upload.wikimedia.org/wikipedia/commons/thumb/d/d1/A_maglev_train_coming_out%2C_Pudong_International_Airport%2C_Shanghai.jpg/1200px-A_maglev_train_coming_out%2C_Pudong_International_Airport%2C_Shanghai.jpg"
---

There’s a odd cultural similarity between the churn that we see within the tech sector: especially the Javascript ecosystem. React (and by extension Preact), Svelte, Solid, Qwik, and a dozen more by the time I’ve written this are effectively like electric cars (consider React as Tesla: an icon in the zeitgeist). They’re the client-side Javascript libraries that keep “innovating” and require an expensive upkeep to solve incessant problems such as bundle size, state management, unidirectional data, etc. The similarity between these frameworks and electric cars is that we’re constantly churning these makework frameworks with no end in sight. The ever-looming problem with these frontend frameworks is dealing with *software complexity*.

How do we deal with maintaining state? Make a library. What if the libraries that exist right now don’t fit our particular use case? Make another library. Maybe a small version of a reducer library (Zustand), another atom-based state management library like Jotai or Recoil, or maybe a immutability-focused state management library. Do we just consider it normal that Facebook maintains **two** separate state management libraries: Recoil and Redux? How are you supposed to leverage your way out of coding interviews without a Github repository having at least 9,000 stars? 

How do we bundle Javascript? `vite` is the cool kid these days, but for a long time it was `webpack` or `parcel`. By the way, remember `rollup`? There was a time when Javascript task runners like `grunt` and `gulp` were the workhorses of every mature project, until people decided that was too brittle, and moved on to just sticking tasks in `package.json` and shell scripts. But wait, why did we just abandon Gulp and Grunt, flirt with npm and shell scripts, swarm around Webpack and Parcel, only to stumble on `vite`? We’re certainly not the first people to approach this sort of problem, there is a history that we can learn from.

---

The ever-looming systemic problems in American society include the climate crisis and the crumbling of the American public infrastructure. Sales for electric vehicles have never been higher, and everyone is getting them under the premise that the lithium-laden cars will cut down carbon emissions thus theoretically saving the planet. And yet, many of our public infrastructure plans are being sabotaged by aggrieved techno-optimist billionaires with grandiose ideas about how tech is the end-all-be-all solution to the problems *they* consider important. How are we supposed to improve substantially when the targets keep moving?

One of the most infamous examples of this sabotage is the Hyperloop, which was originally billed to the public as an incredibly fast mode of transportation that was to beam you from Los Angeles to Las Vegas in around 30 minutes, an homage to the bullet trains of a competently governed Japan. In contrast the same travel in a car is roughly 4 hours with minimal traffic, a feat barely achievable on a sunny day with a Tesla. Elon's promise was that this supernatural proposal would make any mandate for public transportation obsolete because the Boring Company had magically figured out the true solution to LA's and Vegas's public transport woes. Let’s see how the Hyperloop improved transportation in the region:

<iframe width="826" height="1469" src="https://www.youtube.com/embed/pfPqQZaMADs" title="TRAFFIC JAM In The Tesla Hyperloop?! Las Vegas CES" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Oh, that's right. It literally turned into a fire hazard. If traffic gets jammed in that tunnel which is already a non-theoretical daily experience *outside of tunnels*, and one of the cars spontaneously combusts, then everyone in that tunnel is going to die because the Las Vegas government in their infinite wisdom decided to give Elon this bloody tunnel. The Americans reading this are probably wondering what the alternative would be whereas the rest of the world is dying from laughter because we haven't implemented the solutions to an already solved problem: fault-tolerant eco-friendly transit. In fact over the past several decades due to the automotive industry we've done the opposite: we've torn down what used to be maintainable and scalable cities to make way for more highways and roads. 

The Katy Freeway in Houston is a perfect example of how just building more of the same thing doesn't always solve the problem. That highway is the largest in the US with twelve lanes in each direction.

<iframe width="720" height="480" src="https://www.youtube.com/embed/qrV_OrQMiBE" title="WTH?! The Katy Freeway massive traffic nightmare" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

The common thread between the Javascript ecosystem churn and techno-optimism is the "solutions" we're seeing are symptoms of far more systemic issues. It's the absence of workers' rights thus leading to the extreme fungibility of software engineers. It’s the pressure in a competitive job market to build the "next best thing" so that employers will hire based on their Github. It’s employers kidnapping the engineer away from the work on those very projects that end up languishing in tech debt. This is an **utter mockery** of the free spirit of open source; companies are merely treating the open source ecosystem as a way of siphoning engineering talent away from said ecosystem. 

In the case of the United States it's the systemic corruption from billionaires, hedge funds, venture capital firms that has bought our politicians, such that any hope of reorganizing our cities to be more livable, walkable, and more local community oriented have been all but obliterated. They've been replaced with ideas that are so ghastly they would leave Dr. Frankenstein himself in utter disbelief. 

So what do [HTMX](https://htmx.org/), [Unpoly](https://unpoly.com/), [Alpine](https://alpinejs.dev/), and [data-star](https://data-star.dev) have in common with MagLev trains? They are building on top of sensible solutions to already solved problems. With these libraries’ advent, languages that have been considered marginally valuable or deadweight like Smalltalk and Raku are now peers of web-first languages. These libraries are minimally extending HTML: the commonly agreed upon language for presenting websites. They are utilizing `GET` and `POST` requests, the backbone of the internet. 

I'm about to make a pretty controversial claim: React and it’s ilk are rot foisted upon us by the incentives of capital. The future doesn’t have to be boondoggles on the backs of workers, software or otherwise, when we have such a rich history we can draw from. Trains were used by the Indian National Congress during the twilight years of the British occupation in India to connect members of the burgeoning revolutionary movement. If trains are good enough for revolutionaries, they’re good enough for us.
