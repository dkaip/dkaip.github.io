---
layout: default
title: Semiconductor Manufacturing Related Projects
nav_order: 2
---

# Semiconductor Manufacturing Related Projects
{: .no_toc }

These are some projects that provide APIs and or utilities that are useful when needing to produce automation software in the domain of Semiconductor Manufacturing.
{: .fs-5 .fw-300 }

These projects are only really appropriate for those that are working primarily in the realm of shop floor automation in the semiconductor industry. If you do not work in that industry or you are not familiar with the SEMI Standards **E4**, **E5**, **E30**, **E37**, et al., these projects are probably not for you.
{: .fs-5 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## CSharpSECSTools
This project is a collection of API libraries and utilities, written in the programming language C\#, that are useful in the creation of software that communicates with the equipment that is typically found on the shop floor in a Semiconductor Manufacturing facility (a FAB).  This equipment typically communicates using the SEMI SECS-II protocol over a SECS-I (RS-232) link or, more likely, an HSMS link (network).
{: .fs-5 .fw-300 }

Find it here <a href="https://github.com/dkaip/CSharpSECSTools">https://github.com/dkaip/CSharpSECSTools</a>.
{: .fs-5 .fw-300 }

## JavaSECSTools
This project is a collection of API libraries and utilities, written in the programming language Java, that are useful in the creation of software that communicates with the equipment that is typically found on the shop floor in a Semiconductor Manufacturing facility (a FAB).  This equipment typically communicates using the SEMI SECS-II protocol over a SECS-I (RS-232) link or, more likely, an HSMS link (network).
{: .fs-5 .fw-300 }

Find it here <a href="https://github.com/dkaip/JavaSECSTools">https://github.com/dkaip/JavaSECSTools</a>.
{: .fs-5 .fw-300 }

## sc_open
**sc_open** is the Station Controller (sc) application originally created by John F. Poirier DBA Edge Integration circa early this century or late in the previous one. It is written in C. It has been updated to run properly in 64-bit software environments.
{: .fs-5 .fw-300 }

This application may look old and clunky when compared with more  modern "shiny" GUI applications, but make no mistake this is a very powerful application. For simple applications this is all that is needed to create a powerful station controller for a piece of equipment on the shop floor.  In a 300mm+ environment it would be less practical to use this as a station controller, however, it is invaluable in the role of creating equipment simulators to test EIs against using real tool output from log files.  In the future I hope to be able to add scripts and documentation that will make the process even easier.
{: .fs-5 .fw-300 }

In addition it can be quite useful when needing to test certain operating scenarios against a tool during EI development, enhancement, debugging.
{: .fs-5 .fw-300 }

Find it here <a href="https://github.com/dkaip/sc_open">https://github.com/dkaip/sc_open</a>.
{: .fs-5 .fw-300 }


