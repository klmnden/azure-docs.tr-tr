---
title: "Azure Service Fabric CLI komut dosyası Kaldır örneği"
description: "Azure Service Fabric CLI kullanarak bir Azure Service Fabric kümesinden bir uygulamayı kaldırma"
services: service-fabric
documentationcenter: 
author: thraka
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 08/22/2017
ms.author: adegeo
ms.custom: mvc
ms.openlocfilehash: 0b9beb4801d3554d332925456f8df640bf3ffc0b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="remove-an-application-from-a-service-fabric-cluster"></a>Bir uygulama Service Fabric kümeden kaldırma

Bu örnek betik, çalışan bir Service Fabric uygulaması örneği siler, bir uygulama türü ve sürümü kümesinden bir küme kaydını siler.  Uygulama örneğinin silinmesi da siler çalışan tüm hizmet örnekleri uygulama ile ilişkilendirilmiş. Ardından, uygulama dosyaları görüntü deposundan silinir. 

Gerekirse, yükleme [Service Fabric CLI](../service-fabric-cli.md).

## <a name="sample-script"></a>Örnek komut dosyası

[!code-sh[main](../../../cli_scripts/service-fabric/remove-application/remove-application.sh "Remove an application from a cluster")]

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz: [Service Fabric CLI belgelerine](../service-fabric-cli.md).

Azure Service Fabric için ek hizmet doku CLI örnek bulunabilir [Service Fabric CLI örnekleri](../samples-cli.md).
