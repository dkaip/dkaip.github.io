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

for each frame copy any new data that may be needed by the shaders to the GPU
update the descriptor sets that point to the data that the shaders will need

submit the draw commands
{: .fs-5 .fw-300 }

### VkInstance

This is sort of the t
{: .fs-5 .fw-300 }

### Descriptor Sets

A desciptor set is a collection (or set) of descriptors, how profound...I know.  In Vulkan&reg; you 
cannot bind individual descriptors in a shader, you must bind to a descriptor set.
You must create the descriptors as part of a 
descriptor set.

You have memory that is either on the GPU or the GPU has access to it.
You allocate it and put stuff in it
You create descriptor set(s) to inform the GPU as to what is in the
memory...i.e. vertices, image data, transform matricies, etc.
{: .fs-5 .fw-300 }


## Computing in Vulkan&reg; (Compute Shaders)

This section contains information on using Compute Shaders in a in Vulkan&reg; environment.
It (this information) will most likely be shuffled around and moved into a more appropriate location
as this document matures.
{: .fs-5 .fw-300 }

What is Computing in Vulkan (using compute shaders)?
{: .fs-5 .fw-300 }

In short, it is using the computing power available in your graphics card to do some, not necessarily graphics related, computations.
{: .fs-5 .fw-300 }

There are plenty of examples of creating and using compute shaders available on the internet.  When you find
one to your liking you may be able to take it and get it running in relatively short order.  Unfortunately
for me, using these examples I was not really able to properly get my head around using my graphics card for
computations. Yeah sure, I was able to get it working, but, as it turns out most of the examples are not doing
things anything close to efficiently.
{: .fs-5 .fw-300 }

The computing engine, or perhaps paradigm may be the better word, on your graphics card is very different
than that from the regular CPU on your computer.  Even if your CPU has 32+ cores or more.
{: .fs-5 .fw-300 }

For example, let us say that you have defined the following array in C++.
{: .fs-5 .fw-300 }
```cpp
int32_t my_value_array[64];
```
You have an array that contains 64 elements.  Now, lets say you want to initialize the
array so that all of the elements have the value of 1.  You could do something like
the following.
{: .fs-5 .fw-300 }
```cpp
for(int i = 0; i < 64; i++>)
{
    my_value_array[i] = 1;
}

// The array is now initialized
```
This will do nicely.
{: .fs-5 .fw-300 }

However, you are thinking to yourself I have an incredible threading monster of a CPU
in my machine with 64 cores. I can initialize this array much more quickly by doing it this way.
{: .fs-5 .fw-300 }
```cpp
void init_array_slice(int the_array[], int x)
{
  the_array[x] = 1;
}

for(i = 0; i < 64; i++)
{
    new thread(init_array_slice(my_value_array, i));
}

join(all_the_threads_i_created);

// The array is now initialized
```
Yeah I know it is not exactly C++ and I know that this would be a really stupid
thing to do for many reasons, but, work with me on this.
{: .fs-5 .fw-300 }

In this example we will end up with 65 threads (64 + the parent thread) with
all, except one, initializing a piece of the array.
Each thread is running at their own place and pace, i.e. they have their own stack with their own instruction
pointer(counter), etc. Some threads will finish quickly while some may be interrupted and take a little longer.
{: .fs-5 .fw-300 }

Great, so what.  Well now, let's consider running this on the GPU instead of the CPU.
If we run this on the GPU the code will, IN CONCEPT, look more like this.
{: .fs-5 .fw-300 }
```cpp
my_value_array[0] = 1, my_value_array[1] = 1, ... my_value_array[62] = 1, my_value_array[63] = 1;
```
All of these instructions would execute at the same time on the available "compute units".
There would only be one instruction pointer(counter) and all of the units will be on the same
instruction at the same time.
This is conceptual here, it does NOT actually work like this.  Perhaps a better way to say this
is that one instruction is executed, but, it has, in this case, 64 separate variables that it
works on simultaneously.
{: .fs-5 .fw-300 }

Let me include some other concepts that need to be taken into account.
{: .fs-5 .fw-300 }

A modern GPU
has a number of compute units within it.  Let's say in this case a GPU has 1024 shader cores
(a shader core is a "CPU" that can execute shader code) and these
shader cores are broken up into 16 compute units of 64 shader cores each.  In this situation the GPU
might be an AMD&reg; device.  If it were an NVIDIA&reg; device it might have 32 compute units of
32 shader cores each.  The individual compute units are special in that it is possible to have variables
that are visible (shared) between each of the invocations / or "threads" of code running on the group of shader
cores within a single compute unit.  This group of shader cores within a compute unit is called a subgroup
in the Vulkan&reg; vernacular.  In this case we are going to use a subgroup size of 64.
{: .fs-5 .fw-300 }

Let's look at some real shader code now.
{: .fs-5 .fw-300 }
```C
#version 450

layout (local_size_x = 1, local_size_y = 1, local_size_z = 1) in;

layout(std430, binding = 0) buffer Data
{
    int datum[];
} theData;

void main()
{
    theData.datum[gl_GlobalInvocationID.x] = 1;
}
```
In this example we have shader code that actually performs the same function as that of the
C++ code above in that it will set the value of the element(s) of the <code>my_value_array</code>
to a value of <code>1</code>. Yes, there is a large amount of C++ code to actually set this up,
pass the array, and invoke it, but, this is shader code will set the value of the
array element identified by <code>gl_GlobalInvocationID.x</code> to <code>1</code>.
{: .fs-5 .fw-300 }

Here is a snippet of the C++ code used in this example.
{: .fs-5 .fw-300 }
```cpp
cmdBuffer.dispatch(64, 1, 1);
```

Here 64 work groups are submitted in the X coordinate (more on this later)
to the compute shader.  One for each of the 64 elements of the array.
{: .fs-5 .fw-300 }

If we were to actually run the code like this we would be wasting an <b>enormous</b> amount
of processing capacity. In fact we would be running at only about 1.6% of capacity.  Why?
This shader has specified that only 1 invocation be created (<code>local_size_x = 1</code>)
at a time.  We mentioned before that the subgroup size for this example was 64. In this
example it means we will have 1 shader core running and 63 that are inactive because only
one invocation was allowed. In general, you should have the number of invocations allowed
for a shader set to a multiple of the subgroup size.  In this example that would be a
multiple of 64.
{: .fs-5 .fw-300 }



```cpp
int32_t my_value_array[256][256][256];
```
You have an array that contains 256<sup>3</sup> (or 16777216) elements.  Now, lets say you want to initialize the
array so that all of the element have the value of 1.  You could do something like:
{: .fs-5 .fw-300 }
```cpp
int32_t my_value_array[256][256][256];
```
You have an array that contains 256<sup>3</sup> (or 16777216) elements.  Now, lets say you want to initialize the
array so that all of the element have the value of 1.  You could do something like:
{: .fs-5 .fw-300 }
```cpp
for(int i = 0; i < 256; i++>)
{
  for(int j = 0; j < 256; j++)
  {
    for(k = 0; k < 256; k++)
    {
        my_value_array[i][j][k] = 1;
    }
  }
}  However, you are thinking to yourself I have an incredible threading monster of a CPU
in my machine with 256+ cores. I can initialize this array much more quickly by doing it this way.
{: .fs-5 .fw-300 }
```cpp
void init_array_slice(int the_array[256][256][256], int x)In this example
{
  for(j = 0; j < 256; j++)
  {
    for(k = 0; k < 256; k++)
    {
        the_array[x][j][k] = 1;
    }
  }happen
}

for(i = 0; i < 256; i++)
{
    new thread(init_array_slice(my_value_array, i));
}

join(all_the_threads_i_created);

// The array is now initialized
```
Yeah I know it is not exactly C++ and I know that most likely there will only be a thread or two
running because they would finish before the next thread would even be created and started, but,
it is close enough to make a point (work with me on this).
{: .fs-5 .fw-300 }

In this second example
we will end up with 256 threads all initializing a slice of the array. We have 257 (256 + 1)
threads running. Each thread is running at their own place
{: .fs-5 .fw-300 }


```cpp```cpp
int32_t my_value_array[256][256][256];
```
You have an array that contains 256<sup>3</sup> (or 16777216) elements.  Now, lets say you want to initialize the
array so that all of the element have the value of 1.  You could do something like:


// The array is now initialized
```
This will do nicely.  However, you are thinking to yourself I have an incredible threading monster of a CPU
in my machine with 256+ cores. I can initialize this array much more quickly by doing it this way.
{: .fs-5 .fw-300 }
```cpp
void init_array_slice(int the_array[256][256][256], int x)In this example
{
  for(j = 0; j < 256; j++)
  {
    for(k = 0; k < 256; k++)
    {
        the_array[x][j][k] = 1;
    }
  }
}

for(i = 0; i < 256; i++)
{
    new thread(init_array_slice(my_value_array, i));
}

join(all_the_threads_i_created);

// The array is now initialized
```
Yeah I know it is not exactly C++ and I know that most likely there will only be a thread or two
running because they would finish before the next thread would even be created and started, but,
it is close enough to make a point (work with me on this).
{: .fs-5 .fw-300 }

In this second example
we will end up with 256 threads all initializing a slice of the array. We have 257 (256 + 1)
threads running. Each thread is running at their own place
{: .fs-5 .fw-300 }

```cpp
for(int i = 0; i < 256; i++>)
{
  for(int j = 0; j < 256; j++)
  {
    for(k = 0; k < 256; k++)
    {
        my_value_array[i][j][k] = 1;
    }
  }```cpp
int32_t my_value_array[256][256][256];
```
You have an array that contains 256<sup>3</sup> (or 16777216) elements.  Now, lets say you want to initialize the
array so that all of the element have the value of 1.  You could do something like:
{: .fs-5 .fw-300 }
```cpp
for(int i = 0; i < 256; i++>)
{
  for(int j = 0; j < 256; j++)
  {
    for(k = 0; k < 256; k++)
    {
        my_value_array[i][j][k] = 1;
    }
  }
}

// The array is now initialized
```
This will do nicely.  However, you are thinking to yourself I have an incredible threading monster of a CPU
in my machine with 256+ cores. I can initialize this array much more quickly by doing it this way.
{: .fs-5 .fw-300 }
```cpp
void init_array_slice(int the_array[256][256][256], int x)In this example
{
  for(j = 0; j < 256; j++)
  {
    for(k = 0; k < 256; k++)
    {
        the_array[x][j][k] = 1;
    }
  }
}

for(i = 0; i < 256; i++)
{
    new thread(init_array_slice(my_value_array, i));
}

join(all_the_threads_i_created);

// The array is now initialized
```
Yeah I know it is not exactly C++ and I know that most likely there will only be a thread or two
running because they would finish before the next thread would even be created and started, but,
it is close enough to make a point (work with me on this).
{: .fs-5 .fw-300 }

In this second example
we will end up with 256 threads all initializing a slice of the array. We have 257 (256 + 1)
threads running. Each thread is running at their own place
{: .fs-5 .fw-300 }


// The array is now initialized
```
This will do nicely.  However, you are thinking to yourself I have an incredible threading monster of a CPU
in my machine with 256+ cores. I can initialize this array much more quickly by doing it this way.
{: .fs-5 .fw-300 }
```cpp
void init_array_slice(int the_array[256][256][256], int x)In this example
{
  for(j = 0; j < 256; j++)
  {
    for(k = 0; k < 256; k++)
    {
        the_array[x][j][k] = 1;
    }
  }
}

for(i = 0; i < 256; i++)
{
    new thread(init_array_slice(my_value_array, i));
}

join(all_the_threads_i_created);

// The array is now initialized
```
Yeah I know it is not exactly C++ and I know that most likely there will only be a thread or two
running because they would finish before the next thread would even be created and started, but,
it is close enough to make a point (work with me on this).
{: .fs-5 .fw-300 }

In this second example
we will end up with 256 threads all initializing a slice of the array. We have 257 (256 + 1)
threads running. Each thread is running at their own place
{: .fs-5 .fw-300 }

int32_t my_value_array[256][256][256];
```
You have an array that contains 256<sup>3</sup> (or 16777216) elements.  Now, lets say you want to initialize the
array so that all of the element have the value of 1.  You could do something like:
{: .fs-5 .fw-300 }
```cpp
for(int i = 0; i < 256; i++>)
{
  for(int j = 0; j < 256; j++)
  {
    for(k = 0; k < 256; k++)
    {
        my_value_array[i][j][k] = 1;
    }
  }
}

// The array is now initialized
```
This will do nicely.  However, you are thinking to yourself I have an incredible threading monster of a CPU
in my machine with 256+ cores. I can initialize this array much more quickly by doing it this way.
{: .fs-5 .fw-300 }
```cpp
void init_array_slice(int the_array[256][256][256], int x)In this example
{
  for(j = 0; j < 256; j++)
  {
    for(k = 0; k < 256; k++)
    {
        the_array[x][j][k] = 1;
    }
  }
}

for(i = 0; i < 256; i++)
{
    new thread(init_array_slice(my_value_array, i));
}

join(all_the_threads_i_created);

// The array is now initialized
```
Yeah I know it is not exactly C++ and I know that most likely there will only be a thread or two
running because they would finish before the next thread would even be created and started, but,
it is close enough to make a point (work with me on this).
{: .fs-5 .fw-300 }

In this second example
we will end up with 256 threads all initializing a slice of the array. We have 257 (256 + 1)
threads running. Each thread is running at their own place
{: .fs-5 .fw-300 }
