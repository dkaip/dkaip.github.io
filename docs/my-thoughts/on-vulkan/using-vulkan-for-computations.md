---
layout: default
title: Using Vulkan&reg for Computations
nav_order: 2
parent: On Vulkan&reg;
grand_parent: My Thoughts
permalink: docs/my-thoughts/on-vulkan/compute
---

# Using Vulkan&reg; for Computations
{: .no_toc }

What is Computing in Vulkan (using compute shaders)?
{: .fs-5 .fw-300 }

In short, it is using the computing power available in your Graphics Processing Unit (GPU), computer, tablet, phone, etc., to perform some, not necessarily graphics related, &quot;general purpose&quot; computations.
{: .fs-5 .fw-300 }

In this situation &quot;general purpose&quot; is probably
not what you are thinking of. It would not be productive to use a GPU,
via Vulkan&reg; or otherwise, to
compute the distance between a certain point in space and some other point. Your
CPU would be able to perform the computation much more quickly by itself
because it would not have to go through the significant effort required in order
to interface with
your GPU. However, if you need to compute the distance between a certain point
in space and say 500000 other points, the GPU may produce the results much
more quickly.
{: .fs-5 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Introduction

There are plenty of examples of creating and using compute shaders available on the internet.  When you find
one to your liking you may be able to take it and get it running in relatively short order.  Unfortunately
for me, using these examples I was not really able to properly get my head around using my graphics card for
computations. Yeah sure, I was able to get it working, but, as it turns out most of the examples are not doing
things anything close to efficiently.
{: .fs-5 .fw-300 }

The computing engine, or perhaps paradigm may be the better word, on your GPU is very different
than that of the regular CPU on your device.  Even if your CPU has many cores available.
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

All of these instructions would execute at the same time (in lock step) on the available &quot;threads&quot;.
There would only be one instruction pointer(counter) and all of the threads will be on the same
instruction at the same time.
This is conceptual here, it does NOT actually work like this.  Perhaps a better way to say this
is that one instruction is executed, but, it has, in this case, 64 separate variables that it
works on simultaneously.
{: .fs-5 .fw-300 }

Another way to say this might be to say that a CPU executes its instructions on some particular datum
one instruction at a time (in the normal case) while a GPU executes one instruction on
many datum at a time.  This is also known as a SIMD (Single Instruction, Multiple Data) mode
of operation.
{: .fs-5 .fw-300 }

## Important Concepts

Here are some other concepts that need to be taken into account when developing
solutions to problems using compute shaders in Vulkan&reg;.  Pondering these
may help in getting your head around using computer shaders for computations.
{: .fs-5 .fw-300 }

### Blocks of Compute Units

Conceptually, a modern GPU
has a number of compute units within it.  Let's say in this case a GPU has 1024 shader cores
(a shader core may be thought of as a &quot;CPU&quot; that can execute shader code) and these
shader cores are divided up into 16 compute units of 64 shader cores each.  In this situation the GPU
might be an AMD&reg; device.  If it were an NVIDIA&reg; device it might have 32 compute units of
32 shader cores each.
{: .fs-5 .fw-300 }

The individual compute units are special (conceptually) for a couple of reasons.
First, the different threads in each compute unit will perform in lock step with each other.
You might think of this as all of the threads within a compute unit will be executing the
same line of shader code at the same time.  They may be referencing a different portion
of a provided data buffer though.
{: .fs-5 .fw-300 }

In addition, the threads that are executing within a single compute unit may have available to
them variables that are visible (shared) between each of the threads of code running within 
that compute unit.  
{: .fs-5 .fw-300 }

Now, some performance considerations:
{: .fs-5 .fw-300 }

#### Performance Disaster 1

In this case we are going to say that each compute unit can run 64 threads (1 on each of its 64 cores)
at the same time.
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

void main()subgroup
{
    theData.datum[gl_GlobalInvocationID.x] = 1;
}
```

In this example we have shader code that actually performs the same function as that of the
C++ code above in that it will set the value of the element(s) of the <code>my_value_array</code> (in the C++ code)
to a value of <code>1</code>. Yes, there is a large amount of C++ code to actually set this up,
pass the array, and invoke it, but, this is shader code that will set the value of the
array element identified by <code>gl_GlobalInvocationID.x</code> to 1.
{: .fs-5 .fw-300 }

Here is a snippet of the C++ dispatch code used in this example.
{: .fs-5 .fw-300 }

```cpp
cmdBuffer.dispatch(64, 1, 1);
```

Here 64 work groups, one for each of the 64 elements of the array,
are submitted in the X coordinate (more on this later)
to the compute shader.  
{: .fs-5 .fw-300 }

If we were to actually run the code like this we would be wasting an **enormous** amount
of processing capacity. In fact we would be running the GPU at only about 1.6% of capacity.  Why?
This shader has specified that only 1 invocation (thread) be created (<code>local_size_x = 1</code>)
at a time.  We mentioned before that the number of threads in a compute unit for this example was 64. In this
example it means we will have 1 shader core running and 63 that are inactive because only
one invocation was allowed. In general, you should have the number of invocations allowed
for a shader set to a multiple of the something called the **subgroup** size.  In order to improve this example we would set <code>local_size_x = 64</code> or a multiple thereof.  In addition, in this case, our dispatch
command would look like.
{: .fs-5 .fw-300 }

```cpp
cmdBuffer.dispatch(1, 1, 1);
```

In effect, this would create one work group, but, the, shader will create 64 threads that will process the work group.
{: .fs-5 .fw-300 }

#### Performance Disaster 2

Here is another shader example.
{: .fs-5 .fw-300 }

```C
#version 450

layout (local_size_x = 64, local_size_y = 1, local_size_z = 1) in;

layout(std430, binding = 0) buffer Data
{
    int datum[];
} theData;

void main()
{
  if (theData.datum[gl_GlobalInvocationID.x] < 0)
  {
    // do this
  }
  else
  {
    // do that
  }
}
```

Nothing profound here.  We will invoke it the same way as before
with the following C++ code snippet.
{: .fs-5 .fw-300 }

```cpp
cmdBuffer.dispatch(1, 1, 1);
```

If the data we are looking at is negative the "do this" code path is executed,
otherwise the "do that" code path is executed.
The problem is that the GPU wants to run the same
instruction on all of the invocations "threads" at the same time.  In this example, if
50% of the invocations(threads) want to "do this" and the other 50% want to
"do that" the GPU will, in our case, run the first 32 threads through the
"do this" branch until they are complete or at least out of the branch and
then run the other 32 threads through the "do that" branch.  In this example
the GPU is only 50% efficient at getting work done in this section.
{: .fs-5 .fw-300 }

The thing to keep in mind here is that you want your GPU to be as efficient as
possible and to achieve that you want to keep it busy doing work for you when
you need it.  Refer to
<a href="https://www.khronos.org/blog/vulkan-subgroup-tutorial">https://www.khronos.org/blog/vulkan-subgroup-tutorial</a>
for more information on these concepts and how to deal with them.
{: .fs-5 .fw-300 }

### Work Groups

The site <a href="https://www.khronos.org/opengl/wiki/Compute_Shader#Overview">Compute Shader Overview</a>
has information describing work groups.  I am going to try and explain them in different terms.
{: .fs-5 .fw-300 }

The &quot;Work Group&quot;, as explained elsewhere on the WEB, is an abstract concept lacking
a concrete definition.  In my trivial examples above a work group is a collection of units of work.  In these examples, a single
unit of work's &quot;job&quot; is simply to set the value of a location in an array to 1.
{: .fs-5 .fw-300 }

In the first example, there were 64 work groups each one containing 1
unit of work to be done.  This is shown in the dispatch code
<code>cmdBuffer.dispatch(64, 1, 1);</code>. The shader was coded so that it
only uses 1 thread to process 1 unit of work at a time
(<code>layout (local_size_x = 1, ...</code>).
{: .fs-5 .fw-300 }

In the second more efficient example, only 1 work group containing 64 work units (conceptually) was dispatched
<code>cmdBuffer.dispatch(1, 1, 1);</code>. However, this time the shader was coded so that
64 threads are created to handle the work group, of 64 work units, given to it (<code>layout (local_size_x = 64, ...</code>).
{: .fs-5 .fw-300 }

In these trivial examples you will probably not be able to measure the difference in performance, however, if the array size were to be in the millions or billions the
difference in performance would be easily measurable.
{: .fs-5 .fw-300 }

Let's up the game a little bit.  Consider the following C++ code snippet:
{: .fs-5 .fw-300 }

```cpp
float my_value_array[16777216];
```

This will create an array that contains 16777216 (this is 256<sup>3</sup> if you care)elements of 32-bit floating point numbers.  We
want to perform the same operation on the elements of this array that we performed previously;
that would be to set all of the elements to a value of 1.
{: .fs-5 .fw-300 }

Great! All I need to do is use the following dispatch code
{: .fs-5 .fw-300 }

```cpp
cmdBuffer.dispatch(16777216, 1, 1);
```

and Bob's your uncle we are done.  Well, not so fast, at least for me on my machine.
{: .fs-5 .fw-300 }

It turns out, on my machine, when I retrieve the <code>VkPhysicalDeviceLimits</code>
for my device from Vulkan, that <code>maxComputeWorkGroupCount</code> is only
<code>[65535, 65535, 65535]</code>.  This means that I can submit, at most, 65535
work groups in each dimension.  This is certainly not enough to cover the
16777216 elements in my array.  Well, actually it is.  When I look at the
<code>maxComputeWorkGroupInvocations</code> value for my device it has a
value of 1024.  This means that instead of <code>layout (local_size_x = 64, ...</code>
in my shader I can use (<code>layout (local_size_x = 1024, ...</code>).  This means
that I will have 1024 simultaneous invocations (threads) processing the
submitted work. With this being the case I can change my submit code to:
{: .fs-5 .fw-300 }

```cpp
cmdBuffer.dispatch(16777216/1024, 1, 1);

or

cmdBuffer.dispatch(16384, 1, 1);
```

Now I am submitting 16384 work groups each with 1024 threads.  This
is allowing me to still process the entire array in 1 dimension.
{: .fs-5 .fw-300 }

I have performed a little testing and on my old machine (using only the main thread) using my old low end GPU
I was able to achieve better than a 50x performance increase using the GPU to perform the work.  Additionally, this 50x+ performance advantage includes that fact that I am using slowest compatible memory on my machine,
HOST VISIBLE and HOST COHERENT vs the GPU LOCAL memory that is only visible to the GPU.
{: .fs-5 .fw-300 }

### Multiple Dimensions

In the previous examples all of the processing was done using a single dimension
(the X dimension); at least as far as the shader was concerned.  Sometimes processing in multiple dimensions is desirable.  Consider the case where the data we are processing is actually a 2D array.  Say, for example, a 2D image.  In this case it might make much more
sense to process the data in both the X and Y directions rather than to just process
it in X direction direction alone.  The shader code might look like:
{: .fs-5 .fw-300 }

```cpp
#version 450

layout (local_size_x = 64, local_size_y = 16, local_size_z = 1) in;

or perhaps

layout (local_size_x = 32, local_size_y = 32, local_size_z = 1) in;

layout(std430, binding = 0) buffer Data
{
    float datum[4096][4096];
} theData;

void main()
{
    theData.datum[gl_GlobalInvocationID.x][gl_GlobalInvocationID.y] = -1;
}
```

For 3D volume processing it might look like:
{: .fs-5 .fw-300 }

```cpp
#version 450<code>local_size_x</code>

layout (local_size_x = 4, local_size_y = 4, local_size_z = 4) in;

layout(std430, binding = 0) buffer Data
{
    float datum[256][256][256];
} theData;

void main()
{
    theData.datum[gl_GlobalInvocationID.x][gl_GlobalInvocationID.y][gl_GlobalInvocationID.z] = -1;
}
```

Keep in mind, you need to experiment to verify the optimal values for the local
group size, the values for <code>local_size_x</code>, <code>local_size_y</code>,
and <code>local_size_z</code> for your environment. In the examples above, an
array that ultimately had 16777216 elements in it was processed.  In the case
where it was processed as a 1D array it took around 1.5ms to process all of
the elements.  In the case where it was processed as a 2D array it took about
1364ms to process the contents and it took about 362ms to process as a 3D array.
Yes, it looks strange to me too.  Processing the data as a 1D array was 241X
faster than processing it as a 3D array.  Processing the data as a 1D array was 909X
faster than processing it as a 2D array.  I am assuming that on my machine
because the **subgroup** size was 64 that there were many more active cores
in the 1D and 3D cases than in the 2D case because the counts just fit better
to the actual core/compute unit layout..
{: .fs-5 .fw-300 }

**Note:** In my examples I used an array of <code>float</code>s
I could just have easily use
an array of structures containing information needing to be processed.  If
you do use something other than <code>float</code>s, <code>int32_t</code>, <code>uint32_t</code>, etc. make sure you
get your alignments correct and don't get nailed by any padding that may
be added without your knowledge on the C++ side or the GLSL side.
{: .fs-5 .fw-300 }

## Wrap Up

Using a GPU to perform "general purpose" calculations may provide an enormous improvement
in calculation performance, however, in order to get that performance gain your solution
needs to be implemented in a way that allows the GPU to perform efficiently.
{: .fs-5 .fw-300 }

Here are some items to remember:
{: .fs-5 .fw-300 }

- In general, you want to be working on a lot of data.  In most cases, it does not make sense
to go through all of the effort of writing a shader(s), getting your data to the GPU, getting the GPU executing the shader(s),
synchronizing with the GPU, and getting your data back for a small amount of data. A possible exception
might be the case of computing offsets, etc. for data that is already on the GPU that will be
used in subsequent work.
- If possible, engineer your solution so that all, or almost all, of your shader executions will
go through the same code path.
- If possible, set your shader(s) up so that the local work group size is even multiples of your
<code>subgroupSize</code>.  This is done in the shader code line <code>layout (local_size_x = n1, local_size_y = n2, local_size_z = n3) in;</code>.
- Be sure to test out various settings for your local group size
(<code>local_size_x = n1</code>, etc.) in order to make sure
you have numbers that your GPU likes.  The performance difference between the numbers
being optimal and the numbers being suboptimal can be **very** great.  For example,
on my rig with a **subgroup** size of 64, in processing a 3D volume using
<code>layout (local_size_x = 4, local_size_y = 4, local_size_z = 4) in;</code>
was 330%+ faster than using a layout of
<code>layout (local_size_x = 8, local_size_y = 8, local_size_z = 8) in;</code>.
- As with most GPU related things operations can be faster if the data being operated on
is in GPU private memory vs memory that may also be host visible and host coherent.
- Watch out for data alignment issues between your C++ code and the shaders. Refer to:
<a href="https://learnopengl.com/Advanced-OpenGL/Advanced-GLSL">https://learnopengl.com/Advanced-OpenGL/Advanced-GLSL</a> as a starting point in understanding alignment
requirements. Additionally, check out:
<a href="https://community.khronos.org/t/ssbo-std430-layout-rules/109761">https://community.khronos.org/t/ssbo-std430-layout-rules/109761</a>.
{: .fs-5 .fw-300 }

