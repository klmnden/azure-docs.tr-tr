---
title: Azure PowerShell Betiği örnek - bir kümeden kaldır uygulama | Microsoft Docs
description: Azure PowerShell Betiği örnek - uygulama bir Service Fabric kümesinden Kaldır.
services: service-fabric
documentationcenter: ''
author: rwike77
manager: timlt
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 01/18/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: ea7037fc3655298fc4c03b8d9f988a55e42c9fe9
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
ms.locfileid: "27927871"
---
# <a name="remove-an-application-from-a-service-fabric-cluster"></a>Bir uygulama Service Fabric kümeden kaldırma

Bu örnek betik, çalışan bir Service Fabric uygulaması örneği siler ve bir uygulama türü ve sürümü kümesinden bir küme kaydını siler.  Uygulama örneğinin silinmesi da siler çalışan tüm hizmet örnekleri uygulama ile ilişkilendirilmiş. Parametreleri gereken şekilde özelleştirin. 

Gerekirse, ile Service Fabric PowerShell modülünü yüklemek [Service Fabric SDK](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Örnek komut dosyası

[!code-powershell[main](../../../powershell_scripts/service-fabric/remove-application/remove-application.ps1 "Remove an application from a cluster")]

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyasını aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [Remove-ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) | Çalışan bir Service Fabric uygulaması örneği kümeden kaldırır.  |
| [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) | Bir Service Fabric uygulama türü ve sürümü kümesinden bir küme kaydını siler. |

## <a name="next-steps"></a>Sonraki adımlar

Service Fabric PowerShell modülü hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](/powershell/azure/service-fabric/?view=azureservicefabricps).

Azure Service Fabric ek Powershell örnekleri bulunabilir [Azure PowerShell örnekleri](../service-fabric-powershell-samples.md).
