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
ms.openlocfilehash: f8733ef907b8f31ace7ea72f705ba1b37d1adece
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188161"
---
### <a name="azure-storage-sdk-version-in-functions-1x"></a>Azure depolama SDK'sı sürümü işlevlerde 1.x

Azure depolama SDK'sı sürümünü 7.2.1 işlevlerde 1.x, depolama Tetikleyicileri ve bağlamaları kullanın ([WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/7.2.1) NuGet paketi). Farklı bir depolama SDK'sı sürümü başvurmak ve bir depolama SDK'sı türe, işlev imzasında bağlama, İşlevler çalışma zamanı bu türe bağlanamaz bildirebilir. Çözüm, proje başvurularınızın emin olmaktır [WindowsAzure.Storage 7.2.1](https://www.nuget.org/packages/WindowsAzure.Storage/7.2.1).
