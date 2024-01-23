---
layout: default
title: Vulkan&reg; Related Projects
nav_order: 3
---

# Vulkan&reg; Related Projects
{: .no_toc }

The Vulkan&reg; API is similar to OpenGL&reg; and DirectX&reg;. It allows a programmer to access the capabilities of their graphics card or the graphics capability in their APU.  DirectX&reg; is probably only available of Microsoft&reg; "platforms". OpenGL&reg; is a bit bloated and probably near end of life, although I expect it will be available and supported for a long time due to the sheer volume of software that uses it.
Vulkan&reg; is sort of the new kid on the block. It was "recently" designed from the ground up with modern computer architectures in mind and is lean and mean. A programmer can use Vulkan&reg; to access the capabilities of their graphics card or the graphics capability in their APU in a way that will pretty much be the same over all of the supported platforms (Windose, Nintendo Switch, MoltenVK(MacOS, iOS, tvOS), Linux, and Andriod, as of this writing). Unfortunately, "accessing" the local window systems will most likely require "accommodation". This will need to be done using the appropriate WSI interface available for your platform.
{: .fs-5 .fw-300 }

Unfortunately, for me anyway, the Vulkan&reg; API is created for the C and C++ programming languages. Although I can program in these languages I want to use Vulkan&reg; from Java (currently) and C\# in the future. There is a project available that currently exposes Vulkan&reg; bindings to Java, however, it is written in a way that makes sense to a C programmer and is not "natural" for a Java programmer, specifically in terms of dealing with garbage. The projects that I have created allow an interface that is much more natural for a Java programmer using Java objects, object scope, etc.  This does NOT mean that Vulkan&reg; objects will automatically be cleaned up when they go out of scope.  This will have to be done properly even in Java, but, the creation of objects that will be needed by Vulkan&reg; will be done in a much more natural way for a Java programmer, and hopefully for the C\# programmer as well.
{: .fs-5 .fw-300 }

Yes, I expect the performance to be less than that of an application written in C or C++, but, since I can easily use the multi-threading capabilities of Java it is fast enough for me. If you know anything about Vulkan&reg; you will know that feeding the card (GPU) data and instructions properly is the key to good performance. I have not currently noticed any performance issues from using Java, plus, I am able to use JMS and Wildfly, etc.  If you need absolute performance you are going to write in C or C++ anyway.
{: .fs-5 .fw-300 }

The current version of these projects are based on an older version of the Vulkan&reg; API. They should still work. The Vulkan&reg; standard is moving pretty quickly in terms of releasing updates and new features. Given this fact and the amount of work involved I cannot currently keep up with all of the new changes. I am working on some software that I can run against the specification to generate a current version of the interface automatically or at least with minimal manual intervention. This will hopefully make staying much more up to date possible.
{: .fs-5 .fw-300 }

Currently these projects, well the natives anyway, are only compiled for a Linux X86_64 environment. I currently do not have the ability to compile, generate code for, and test in other environments.
{: .fs-5 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## jVulkan
This is a Java version of the Vulkan&reg; API that is available from [LunarG](https://www.lunarg.com/vulkan-sdk/). It provides a Java version of all of the objects that are available when using C++.  The objects are created in a "Java standard manner" and manipulated using Java standard setters and getters.  In addition arguments are passed to the API functions in a Java standard manner.  Unfortunately, in a few instances native arguments (ints, doubles, etc.) are passed to the LunarG implementation by reference. In these cases a Java object needs to be passed, but, that is done in a manner that is consistent and easy to understand.
{: .fs-5 .fw-300 }

Find it here <a href="https://github.com/dkaip/jvulkan">https://github.com/dkaip/jvulkan</a>.
{: .fs-5 .fw-300 }

## jVulkan Natives
The jVulkan API makes calls into the Vulkan&reg; API that is created and maintained by LunarG. The library provided by LunarG is written in C and or C++. This project, jVulkan Natives, is the JNI layer (written in C++) that handles the conversion of Java objects to and from c++ objects. If you use jVulkan in your Java application you will need this project as well.
{: .fs-5 .fw-300 }

Find it here <a href="https://github.com/dkaip/jvulkan-natives-Linux-x86_64">https://github.com/dkaip/jvulkan-natives-Linux-x86_64</a>.
{: .fs-5 .fw-300 }

## jvma
Similar to the jVulkan project above this project provides Java access to the [Vulkan Memory Allocator Project](https://github.com/GPUOpen-LibrariesAndSDKs/VulkanMemoryAllocator). The Vulkan Memory Allocator Project provides memory allocator functionality to those writing software using the Vulkan&reg; API. This is a pretty indispensible need when doing more than trivial things in Vulkan&reg;.
{: .fs-5 .fw-300 }

Find it here <a href="https://github.com/dkaip/jvma">https://github.com/dkaip/jvma</a>.
{: .fs-5 .fw-300 }

## jvma Natives
Similar to the jVulkan Natives project above this project implements the JNI layer (written in C++) that "links" the jvma project above to the library produced when the [Vulkan Memory Allocator Project](https://github.com/GPUOpen-LibrariesAndSDKs/VulkanMemoryAllocator)is built.
{: .fs-5 .fw-300 }

Find it here <a href="https://github.com/dkaip/jvma-natives-Linux-x86_64">https://github.com/dkaip/jvma-natives-Linux-x86_64</a>.
{: .fs-5 .fw-300 }

## jVulkan Examples
This project is basically a Java version of the code available for the [Vulkan Tutorial](https://vulkan-tutorial.com). It provides the examples (more or less) that are provided in the [Vulkan Tutorial](https://vulkan-tutorial.com), but, they are written in Java. In order to use these examples you will need to successfully build and install the previous projects since this project will need them.
{: .fs-5 .fw-300 }

Find it here <a href="https://github.com/dkaip/jvulkan-examples">https://github.com/dkaip/jvulkan-examples</a>.
{: .fs-5 .fw-300 }


