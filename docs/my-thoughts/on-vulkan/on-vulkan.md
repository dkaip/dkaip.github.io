---
layout: default
title: On Vulkan&reg;
nav_order: 3
parent: My Thoughts
has_children: true
permalink: docs/my-thoughts/on-vulkan
---

# Vulkan&reg;
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## What is it?

From Wikipedia:
{: .fs-5 .fw-300 }

Vulkan&reg; is a low-overhead, cross-platform API, open standard for 3D graphics and computing. Vulkan&reg; targets high-performance real-time 3D graphics applications, such as video games, interactive media, and highly parallelized computing.
{: .fs-5 .fw-300 }

Wow, that is a great, concise, and pretty complete description of Vulkan&reg;.  Does it mean more to you than marketing blather?  If not, read on.
{: .fs-5 .fw-300 }

## Why am I writing this?

Vulkan&reg; is kind of the new kid on the block (even though it has been around since 2016 or so) in terms of an API to create 3D graphics.
Prior to Vulkan&reg; you had OpenGL and of course Microsoft's DirectX?? and perhaps some others.  In fact, you still have OpenGL and
Microsoft's DirectX??.  Why do we need another API?  Well, a number of reasons. First Microsoft wants to keep as much as possible
in their domain to keep people locked in to them and their software. (Think \$\$\$\$\$\$\$ here and lack of competition to share that \$\$\$\$\$ with.)
The effects of the lack of competition leads to higher costs and a decrease in innovation.  What about OpenGL? OpenGL is certainly
much more cross platform and does work, however, as the world of &quot;graphics&quot;, and &quot;graphics&quot; computation, has moved on. OpenGL has
pretty much been hamstrung by backwards compatibility constraints.
{: .fs-5 .fw-300 }

Enter Vulkan&reg;, it was designed from the ground up with &quot;modern&quot; GPUs in mind.  In addition it is much more cross platform
allowing it to be much closer to the holy grail of write for one API and run on all platforms.  Sigh, if only it were that simple.
{: .fs-5 .fw-300 }

So, to answer the question of why I am writing this...In short, because almost all of the articles (and examples) I have found are written
by people who know what they are talking about.  That sounds great, but, if you are not a person that already knows what they
are talking about you are left in a position of climbing a near vertical learning curve and / or just living on faith taking the
examples, coding them up, and poking at them with changes until you just get something that &quot;works&quot;.  My hope in providing some
information here is that you can start out with a better understanding of what is going on and ultimately produce a better
result for yourself.
{: .fs-5 .fw-300 }

## Let's Dive In

It might help you, in the short term, to think of Vulkan&reg; as an abstraction of a type of 3D rendering hardware.
In short, you create an application that uses this abstraction and then it should run on any hardware that is supported
by Vulkan&reg;.  As I mentioned before, it would be nice if it were really that easy.  In simple applications it
actually can be that simple.
{: .fs-5 .fw-300 }

Vulkan&reg; was created with performance in mind, and at least partially because of it, it presents a low
level abstraction which results in the user thereof needing to specifically initialize or define a large number
of parameters ahead of time.  In addition, most of those parameters are not allowed to change.  This paradigm
allows the Vulkan&reg; driver software to perform optimizations that it could otherwise not perform.
{: .fs-5 .fw-300 }

Vulkan&reg; like OpenGL has &quot;states&quot; and other information that it needs to create and maintain during the
course of its operation.  Unlike OpenGL which has a global state, Vulkan&reg; maintains its state in a `VkInstance` object.
This `VkInstance` object is the first object you create when using Vulkan&reg;.  Some of the documentation
you will find on the WEB mentions that your application should only have one `VkInstance` object. I disagree,
I have seen situations such as you writing a library for others to use.  Your library needs to use Vulkan&reg;.
Well, you create your own `VkInstance` do your work and let the user of your library create their own
`VkInstance` and do their work.  Yes, you will need to have mechanisms of allowing the users of your API
to interact with it.  For example, say you are at a company writing a &quot;space&quot; game.  Your job is
to deal with the graphics for the cockpit and / or inside of the space ship.  Your graphics will pretty much only
deal with &quot;static&quot; objects with simplified lighting requirements. Perhaps, blinking lights, ship display
updates, etc. Another team at the company is responsible for what goes on outside the spacecraft.  Rendering ships,
explosions, laser beams, complex lighting, planets, stars, etc.  It would be safe to say that the rendering
requirements for each team would be quite different so having and using multiple `VkInstance` object may be
very appropriate.  Anyway, yeah, a very large percentage of the applications
using Vulkan&reg; probably only need one `VkInstance`, but, if you understand them and what they are for
you will better understand why and when using more that one `VkInstance` may be the way to go.
{: .fs-5 .fw-300 }

## The Flow

- Create the VkInstance object
- Query to identify the GPU(s) that are available
- Choose the GPU(s) that will be used and create the VkDevice objects for them
- Create the various pipelines that will be needed

For each frame copy any new data that may be needed by the shaders to the GPU,
update the descriptor sets that point to the data that the shaders will need

submit the draw commands
{: .fs-5 .fw-300 }

### VkInstance

This is sort of the t
{: .fs-5 .fw-300 }

### Descriptor Sets

A descriptor set is a collection (or set) of descriptors, how profound...I know.  In Vulkan&reg; you 
cannot bind individual descriptors in a shader, you must bind to a descriptor set.
You must create the descriptors as part of a 
descriptor set.

You have memory that is either on the GPU or the GPU has access to it.
You allocate it and put stuff in it
You create descriptor set(s) to inform the GPU as to what is in the
memory...i.e. vertices, image data, transform matrices, etc.
{: .fs-5 .fw-300 }

