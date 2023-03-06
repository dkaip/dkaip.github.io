---
layout: default
title: On Vulkan&reg;
parent: My Thoughts
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

Wow, that a great, concise, and pretty complete description of Vulkan&reg;.  Does it mean more to you than marketing blather?  If not, read on.
{: .fs-5 .fw-300 }

## Why am I writing this?

Vulkan&reg; is kind of the new kid on the block (even though it has been around since 2016 or so) in terms of an API to create 3D graphics.
Prior to Vulkan&reg; you had OpenGL and of course Microsoft's DirectX?? and perhaps some others.  In fact, you still have OpenGL and
Microsoft's DirectX??.  Why do we need another API?  Well, a number of reasons. First Microsoft wants to keep as much as possible
in their domain to keep people locked in to them and their software. (Think \$\$\$\$\$\$\$ here and lack of competition to share that \$\$\$\$\$ with.)
The effects of the lack of competition leads to higher costs and a decrease in innovation.  What about OpenGL? OpenGL is certainly
much more cross platform and does work, however, as the world of &quot;graphics&quot;, and &quot;graphics&quot; computation, has moved on. OpenGL has
pretty much been constrained by backwards compatibility constraints.
{: .fs-5 .fw-300 }

Enter Vulkan&reg;, it was designed from the ground up with &quot;modern&quot; GPUs in mind.  In addition it is much more cross platform
allowing it to be much closer to the holy grail of write for one API and run on all platforms.  Sigh, if only it were that simple.
{: .fs-5 .fw-300 }

So, to answer the question of why I am writing this...In short, because almost all of the articles (and examples) I have found are written
by people who know what they are talking about.  That sounds great, but, if you are not a person that already knows what they
are talking about you are left in a position of climbing a near vertical learning curve and / or just on faith taking the
examples, coding them up, and poking at them with changes until you just get something that &quot;works&quot;.  My hope in providing some
information here is that you can start out with a better understanding of what is going on and ultimately product a better
result for yourself.
{: .fs-5 .fw-300 }

## Let's Dive In

Splash!
{: .fs-5 .fw-300 }
