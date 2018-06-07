---
title: include dosyası
description: include dosyası
services: functions
author: tdykstra
manager: cfowler
ms.service: functions
ms.topic: include
ms.date: 05/17/2018
ms.author: tdykstra
ms.custom: include file
ms.openlocfilehash: 97c56b07833f7d93541bb0b3747889f5a50a8203
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34675284"
---
### <a name="azure-storage-sdk-version-in-functions-1x"></a>Azure depolama SDK'sı sürüm işlevlerinde 1.x

İşlevlerde 1.x, depolama Tetikleyicileri ve bağlamaları 7.2.1 Azure depolama SDK'sını kullanın ([WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/7.2.1) NuGet paketi). Depolama SDK'yı farklı bir sürümü başvuru ve bir depolama SDK'sı türe işlevi imzanız bağlama işlevleri çalışma zamanı bu türe bağlanamıyor bildirebilir. Proje başvuruları emin olmak için çözümdür [WindowsAzure.Storage 7.2.1](https://www.nuget.org/packages/WindowsAzure.Storage/7.2.1).
