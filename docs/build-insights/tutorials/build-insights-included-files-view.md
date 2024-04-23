---
title: "Tutorial: Build Insights included files view"
description: "Tutorial on how to use Build Insights function view to troubleshoot the impact of #include files on build time."
ms.date: 4/25/2024
helpviewer_keywords: ["C++ Build Insights", "included files view", "include tree view", "#include analysis", "build time analysis"]
---
# Tutorial: Use Build Insights to troubleshoot #include files on build time

Use Build Insights **Included Files** and **Include Tree** views to troubleshoot the impact of `#include` files on build time.

## Prerequisites

- Visual Studio 2022 17.8 or greater.
- C++ Build insights is enabled by default if you installed either the Desktop development with C++ workload or the Game development with C++ workload.

:::image type="complex" source="./media/installer-desktop-cpp-build-insights.png" alt-text="Screenshot of the Visual Studio Installer with the Desktop development with C++ workload selected.":::
The list of installed components is shown. C++ Build Insights is highlighted and is selected to indicate that it is included in the installation.
:::image-end:::

:::image type="complex" source="./media/installer-gamedev-cpp-build-insights.png" alt-text="Screenshot of the Visual Studio Installer with the Game development with C++ workload selected.":::
The list of installed components is shown. C++ Build Insights is highlighted and is selected to indicate that it is included in the installation.
:::image-end:::

## Overview

Build Insights, now integrated into Visual Studio, is designed to help you optimize your build times--especially for large projects like AAA games. Build Insights provides various analytics such as **Included** view, which helps diagnose the impact of repeatedly parsing `#include` files. It displays the time it takes to generate code for each function, and shows the impact of [`__forceinline`](../../cpp/inline-functions-cpp.md#inline-__inline-and-__forceinline).

Parsing header files has an impact on build time. When a large header file is repeatedly parsed, there is an even greater impact on compile time.  The `__forceinline` directive tells the compiler to inline a function regardless of its size or complexity. Inlining a function can improve runtime performance by reducing the overhead of calling the function, but it can increase the size of the binary and impact your build times. For optimized builds, the time spent generating code is a significant contributor to the total build time. In general, C++ function optimization happens quickly. But in exceptional cases, some functions can become large and complex enough to put pressure on the optimizer and noticeably slow down your builds.

In this article, learn how to use the Build Insights **Functions** view to identify bottlenecks in your build process and improve build time and function inlining.

## Set build options

To measure the results of `__forceinline`, use a **Release** build where optimizations for `__forceinline` impact release build times the most. Set the build for **Release** and **x64**:

- In the **Solution Configurations** dropdown, choose **Release**.
- In the **Solution Platforms** dropdown, choose **x64**.

:::image type="content" source="./media/" alt-text="Screenshot showing the Solution Configuration dropdown set to Release, and the Solution Platform dropdown set to x64":::

Set the optimization level to maximum optimizations:

- In the **Solution Explorer**, right-click the project name and select **Properties**.
- In the project properties, navigate to **C/C++** > **Optimization**.
- Set the **Optimization** dropdown to **Maximum Optimization (Favor Speed) (/O2)**.

:::image type="content" source="./media/" alt-text="Screenshot showing the project property pages dialog. The settings are open to Configuration Properties > C/C++ > Optimization. The Optimization dropdown is set to Maximum Optimization (Favor Speed) (/O2)":::

- Click **OK** to close the dialog.

## Run build insights

On a project of your choosing, and using the **Release** build options set in the previous section, run Build Insights by choosing **Build** > **Run Build Insights on Solution** > **Build**. Or, you can run Build Insights on a specific project in a multi-project solution by right-clicking the project in Solution Explorer and selecting **Run Build Insights**.

When the build finishes, an Event Trace Log (ETL) file opens similar to the example that follows. It's saved in the `%temp%` folder on your machine. The generated name is based on the time of collection. This file shows the time spent processing `#include` files, the build time for each function, and how much `__forceinline` impacted the size of the function.

:::image type="complex" source="./media/" alt-text="alt text stuff":::
big ole’ long description
:::image-end:::

## Included Files View

time is aggregate time spent parsing the file


## Include Tree View

right click on a file to: 

In the window for the ETL file, choose the **Functions** tab. It shows the functions that were compiled and the time it took to compile each function. If a function's code generation time is too small, it won't be displayed because build events with negligible impact are discarded to avoid degrading build event collection performance.

:::image type="complex" source="./media/" alt-text="alt text stuff":::
Just show the functions tab portion of the dialog with the forceinline size column, time column
:::image-end:::

The **Time [sec, %]** column shows how long it took to compile each function. The **Forceinline Size** column shows the impact of each `__forceinline` function in terms of roughly how many intermediate instructions were generated for the inlined function. These numbers are summed, and the impact for all the inlined functions is listed for the containing function. You can sort the list by clicking on the **Time** column to see which functions are taking the most time to compile. A 'fire' icon indicates that cost of generating that function is particularly high and is worth investigating.

The `Project` column indicates which project the function belongs to. Double click the **File** column to go to the source file where the function is defined.

Select the chevron next to a function to expand the function and see the list of inline functions that were expanded inside it. The functions that were inlined inside this function are listed and their individual size is shown in terms of generated instructions. Higher is worse. Highlighting `__forceinline` information is important because excessive use of `__forceinline` functions can significantly slow compilation.

You can search for a specific function by using the **Filter Functions** box. If a function's code generation time is too small, it doesn't appear in the Functions View.

## Improve build time with precompiled headers

show how to build a precompiled header or link to topic for it
    - this topic shows how to build PCH: https://devblogs.microsoft.com/cppblog/faster-builds-with-pch-suggestions-from-c-build-insights/
Talk about header units

## Troubleshooting

- If the Build Insights window doesn't appear, do a rebuild instead of a build: **Build** > **Run Build Insights on Solution** > **Rebuild**.
- If you closed the Build Insights window, reopen it by finding the `.etl` file in your `%temp%` folder, where `%temp%` is a Windows environment variable that contains the path to your temporary files folder.

## See also

video pure virtual c++ 2023: https://youtu.be/P63jEa85pFg

[Inline functions (C++)](../../cpp/inline-functions-cpp.md)\
[Tutorial: vcperf and Windows Performance Analyzer](vcperf-and-wpa.md)\
[Reference: vcperf commands](../reference/vcperf-commands.md)\
[Reference: Windows Performance Analyzer views](../reference/wpa-views.md)\
[Windows Performance Analyzer](/windows-hardware/test/wpt/windows-performance-analyzer)
