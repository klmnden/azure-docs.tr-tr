---
title: "Azure PowerShell Betiği örnek - uygulama bir kümeye dağıtma | Microsoft Docs"
description: "Azure PowerShell Betiği örnek - Service Fabric kümesi için bir uygulamayı dağıtın."
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 09/29/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 159b856606885c4ee206ba42ec72a8bd1ec5b0f7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
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
|[Kopyalama ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) | Kopya bir uygulama paketi küme görüntüsüne depolar.  |
|[Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps)| Bir uygulama türü ve sürümü küme üzerinde kaydeder. |
|[ServiceFabricApplication yeni](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps)| Kayıtlı uygulama türünden bir uygulama oluşturur. |
| [Remove-ServiceFabricApplicationPackage](/powershell/module/servicefabric/remove-servicefabricapplicationpackage?view=azureservicefabricps) | Service Fabric uygulama paketi görüntü deposundan kaldırır.|

## <a name="next-steps"></a>Sonraki adımlar

Service Fabric PowerShell modülü hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](/powershell/azure/service-fabric/?view=azureservicefabricps).

Azure Service Fabric ek Powershell örnekleri bulunabilir [Azure PowerShell örnekleri](../service-fabric-powershell-samples.md).
