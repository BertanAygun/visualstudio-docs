---
title: "How to: Switch to Another Thread While Debugging | Microsoft Docs"
ms.custom: ""
ms.date: "04/27/2017"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "vs-ide-debug"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "CSharp"
  - "VB"
  - "FSharp"
  - "C++"
helpviewer_keywords: 
  - "threads, switching [debugging]"
ms.assetid: 5cd76c52-76fa-4fcc-b37e-e9f0ecac0e9e
caps.latest.revision: 26
author: "mikejo5000"
ms.author: "mikejo"
manager: "ghogen"
translation.priority.ht: 
  - "cs-cz"
  - "de-de"
  - "es-es"
  - "fr-fr"
  - "it-it"
  - "ja-jp"
  - "ko-kr"
  - "pl-pl"
  - "pt-br"
  - "ru-ru"
  - "tr-tr"
  - "zh-cn"
  - "zh-tw"
---
# How to: Switch to Another Thread While Debugging
When you debug a multithreaded application, you can use any one of several methods to switch the context from the thread that you have been working with to another thread.  
  
### To switch to any thread that appears 
  
-   In the **Threads** or **Parallel Watch** window, double-click the thread.  
  
### To switch to a thread in a source window  
  
-   In the left gutter, right-click a thread indicator, point to **Switch to**, and then click the name of that thread to which you want to switch. The shortcut menu shows only the threads at that specific location.  
  
     If no indicators appear, right-click in the **Threads** window and verify that **Show Threads in Source** is selected.  
  
### To switch to a thread in the Debug Location toolbar  
  
1.  On the **Debug Location** toolbar, click the **Thread** box.  
  
2.  In the list, click the thread to which you want to switch.  
  
## See Also  
 [Debug Multithreaded Applications](../debugger/debug-multithreaded-applications-in-visual-studio.md)