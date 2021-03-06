---
title: Code Snippets with R Tools for Visual Studio | Microsoft Docs
ms.custom: ""
ms.date: 4/28/2017
ms.prod: "visual-studio-dev15"
ms.reviewer: ""
ms.suite: ""
ms.technology:
  - "devlang-r"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 90bf4f87-e276-40cd-bc17-3dfb47ef1870
caps.latest.revision: 1
author: "kraigb"
ms.author: "kraigb"
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

# Code snippets

Code snippets in Visual Studio provide shortcuts to quickly insert code blocks of arbitrary length, helping you avoid retyping similar code over and over. The R Tools for Visual Studio (RTVS) add dozens of useful R snippets to Visual Studio's collection.

To insert a snippet, type the abbreviated name of snippet (IntelliSense is provided), then press Tab to insert.

Some simple examples:

- type `=` then Tab and RTVS expands it to the `<-` assignment operator.
- type `>` then Tab and RTVS expands it the `%>%` pipe operator.

Snippets can be much more than just character completion of characters. For example, they can relieve you from having to remember the names of parameters in complex function call, such as this snippet for reading a CSV file with the `read.csv` function:

![Animation of using a code snippet to insert a call to read.csv](media/code-snippet-expansion.gif)

In this case, as you type `readc`, IntelliSense displays a completion list. Selecting that completion in the drop down and pressing Tab selects `readc`, and pressing Tab again expands the snippet. (For this reason, snippet expansion is often thought of as "type the snippet and press TAB twice"). In most cases, the first Tab completes the IntelliSense selection and the second Tab triggers the expansion.

To see all the available snippets, open the **Tools > Code Snippets Manager...** dialog box (Ctrl+K,B) and select **R** for **Language**. Expand the groups and select individual snippets to see a description and the shortcut text:

![Code snippets dialog box for R](media/code-snippet-dialog.png)

To create custom code snippets, following the instructions on [Walkthrough: Creating a code snippet](../ide/walkthrough-creating-a-code-snippet.md). Ultimately, a code snippet is just an XML file. For example, the following is the snippet for the pipe operation (shortcut `>`)

Note that a code snippet is just an XML file; here's the Code Snippet for the pipe operator:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<CodeSnippets  xmlns="http://schemas.microsoft.com/VisualStudio/2005/CodeSnippet">
  <CodeSnippet Format="1.0.0">
    <Header>
      <Title>Dplyr pipe</Title>
      <Shortcut>&gt;</Shortcut>
      <Description>Code snippet for '%&gt;%' operator</Description>
      <Author>Microsoft Corporation</Author>
      <SnippetTypes>
        <SnippetType>Expansion</SnippetType>
       </SnippetTypes>
    </Header>
    <Snippet>
      <Code Language="R">
        <![CDATA[ %>% $end$]]>
      </Code>
    </Snippet>
  </CodeSnippet>
</CodeSnippets>
```

The XML files for all code snippets are installed with RTVS; the **Location** field in the **Code Snippets Manager** provides the path. You can also find them in the RTVS source code on GitHub under [src/Package/Impl/Snippets](https://github.com/Microsoft/RTVS/tree/master/src/Package/Impl/Snippets).
