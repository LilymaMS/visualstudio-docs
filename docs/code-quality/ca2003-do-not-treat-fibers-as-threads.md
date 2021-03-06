---
title: "CA2003: Do not treat fibers as threads | Microsoft Docs"
ms.custom: ""
ms.date: "11/04/2016"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "vs-devops-test"
ms.tgt_pltfrm: ""
ms.topic: "article"
f1_keywords: 
  - "CA2003"
  - "DoNotTreatFibersAsThreads"
helpviewer_keywords: 
  - "CA2003"
  - "DoNotTreatFibersAsThreads"
ms.assetid: 15398fb1-f384-4bcc-ad93-00e1c0fa9ddf
caps.latest.revision: 16
author: "gewarren"
ms.author: "gewarren"
manager: "ghogen"
translation.priority.ht: 
  - "de-de"
  - "es-es"
  - "fr-fr"
  - "it-it"
  - "ja-jp"
  - "ko-kr"
  - "ru-ru"
  - "zh-cn"
  - "zh-tw"
translation.priority.mt: 
  - "cs-cz"
  - "pl-pl"
  - "pt-br"
  - "tr-tr"
---
# CA2003: Do not treat fibers as threads
|||  
|-|-|  
|TypeName|DoNotTreatFibersAsThreads|  
|CheckId|CA2003|  
|Category|Microsoft.Reliability|  
|Breaking Change|Non-breaking|  
  
## Cause  
 A managed thread is being treated as a Win32 thread.  
  
## Rule Description  
 Do not assume a managed thread is a Win32 thread. It is a fiber. The common language runtime (CLR) will run managed threads as fibers in the context of real threads that are owned by SQL. These threads can be shared across AppDomains and even databases in the SQL Server process. Using managed thread local storage will work, but you may not use unmanaged thread local storage or assume that your code will run on the current OS thread again. Do not change settings such as the locale of the thread. Do not call CreateCriticalSection or CreateMutex via P/Invoke because they require that the thread that enters a lock must also exit the lock. Because this will not be the case when you use fibers, Win32 critical sections and mutexes will be useless in SQL. You may safely use most of the state on a managed System.Thread object. This includes managed thread local storage and the current user interface (UI) culture of the thread. However, for programming model reasons, you will not be able to change the current culture of a thread when you  use SQL; this will be enforced through a new permission.  
  
## How to Fix Violations  
 Examine your usage of threads and change your code accordingly.  
  
## When to Suppress Warnings  
 You should not suppress this rule.