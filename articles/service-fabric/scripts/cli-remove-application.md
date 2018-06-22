---
title: Azure Service Fabric CLI (sfctl) Betik Kaldırma Örneği
description: Azure Service Fabric CLI’sini kullanarak bir uygulamayı Azure Service Fabric kümesinden kaldırma
services: service-fabric
documentationcenter: ''
author: thraka
manager: timlt
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 12/06/2017
ms.author: adegeo
ms.custom: mvc
ms.openlocfilehash: 83f2fc52debd24afd97a391466cb5a0b1a8cd93c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34642725"
---
# <a name="remove-an-application-from-a-service-fabric-cluster"></a>Bir uygulamayı Service Fabric kümesinden kaldırma

Bu örnek betik, çalışan bir Service Fabric uygulaması örneğini ve ardından bir uygulama türünün ve sürümünün kaydını kümeden siler.  Uygulama örneğinin silinmesi, bu uygulamayla ilişkili çalışan tüm hizmet örneklerini de siler. Ardından, uygulama dosyaları görüntü deposundan silinir. 

Gerekirse [Service Fabric CLI](../service-fabric-cli.md)'sını da yükleyin.

## <a name="sample-script"></a>Örnek betik

[!code-sh[main](../../../cli_scripts/service-fabric/remove-application/remove-application.sh "Remove an application from a cluster")]

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz. [Service Fabric CLI belgeleri](../service-fabric-cli.md).

Azure Service Fabric’e yönelik ek Service Fabric CLI örnekleri [Service Fabric CLI örnekleri](../samples-cli.md)’nde bulunabilir.
