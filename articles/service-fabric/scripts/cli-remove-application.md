---
title: Azure Service Fabric CLI (sfctl) Betik Kaldırma Örneği
description: Azure Service Fabric CLI’sini kullanarak bir uygulamayı Azure Service Fabric kümesinden kaldırma
services: service-fabric
documentationcenter: ''
author: aljo-microsoft
manager: timlt
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 12/06/2017
ms.author: aljo
ms.custom: mvc
ms.openlocfilehash: 47f0636437fb903d43c0aaa439bb41309b56dcc9
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2019
ms.locfileid: "56804425"
---
# <a name="remove-an-application-from-a-service-fabric-cluster"></a>Bir uygulamayı Service Fabric kümesinden kaldırma

Bu örnek betik, çalışan bir Service Fabric uygulaması örneğini ve ardından bir uygulama türünün ve sürümünün kaydını kümeden siler.  Uygulama örneğinin silinmesi, bu uygulamayla ilişkili çalışan tüm hizmet örneklerini de siler. Ardından, uygulama dosyaları görüntü deposundan silinir. 

Gerekirse [Service Fabric CLI](../service-fabric-cli.md)'sını da yükleyin.

## <a name="sample-script"></a>Örnek betik

[!code-sh[main](../../../cli_scripts/service-fabric/remove-application/remove-application.sh "Remove an application from a cluster")]

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz. [Service Fabric CLI belgeleri](../service-fabric-cli.md).

Azure Service Fabric’e yönelik ek Service Fabric CLI örnekleri [Service Fabric CLI örnekleri](../samples-cli.md)’nde bulunabilir.
