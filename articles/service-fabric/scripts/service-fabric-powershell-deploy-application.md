---
title: Azure PowerShell Betiği örnek - uygulama bir kümeye dağıtma | Microsoft Docs
description: Azure PowerShell Betiği örnek - Service Fabric kümesi için bir uygulamayı dağıtın.
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
ms.openlocfilehash: c81514fb4b1c1da483ebd55deae149caf22d4b63
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
ms.locfileid: "27927609"
---
# <a name="deploy-an-application-to-a-service-fabric-cluster"></a>Service Fabric kümesi bir uygulamayı dağıtma

Bu örnek betik bir uygulama paketi bir küme görüntü deposuna kopyalar, kümede uygulama türü kaydeder, gereksiz uygulama paketi kaldırır ve uygulama türünden uygulama örneğini oluşturur.  Varsayılan hizmetlerin hedef uygulama türü uygulama bildiriminde tanımlanan yapıyorsanız, bu hizmetleri şu anda oluşturulur. Parametreleri gereken şekilde özelleştirin. 

Gerekirse, ile Service Fabric PowerShell modülünü yüklemek [Service Fabric SDK](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Örnek komut dosyası

[!code-powershell[main](../../../powershell_scripts/service-fabric/deploy-application/deploy-application.ps1 "Deploy an application to a cluster")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Komut dosyası örneği gerçekleştirildikten sonra komut dosyasını [bir uygulamayı kaldırmak](service-fabric-powershell-remove-application.md) uygulama örneğini kaldırma, uygulama türü kaydını kaldırma ve uygulama paketi görüntü deposundan silmek için kullanılabilir.

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyasını aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
|[Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps)| Service Fabric kümesi için bir bağlantı oluşturur. |
|[Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) | Kopya bir uygulama paketi küme görüntüsüne depolar.  |
|[Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps)| Bir uygulama türü ve sürümü küme üzerinde kaydeder. |
|[New-ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps)| Kayıtlı uygulama türünden bir uygulama oluşturur. |
| [Remove-ServiceFabricApplicationPackage](/powershell/module/servicefabric/remove-servicefabricapplicationpackage?view=azureservicefabricps) | Service Fabric uygulama paketi görüntü deposundan kaldırır.|

## <a name="next-steps"></a>Sonraki adımlar

Service Fabric PowerShell modülü hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](/powershell/azure/service-fabric/?view=azureservicefabricps).

Azure Service Fabric ek Powershell örnekleri bulunabilir [Azure PowerShell örnekleri](../service-fabric-powershell-samples.md).
