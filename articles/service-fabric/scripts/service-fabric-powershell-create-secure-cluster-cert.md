---
title: "Azure PowerShell Betiği örnek - Service Fabric kümesi oluşturun. | Microsoft Docs"
description: "Azure PowerShell Betiği örnek - Service Fabric kümesi oluşturur."
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 0f9c8bc5-3789-4eb3-8deb-ae6e2200795a
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 01/19/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: b0d33b714092a826012677c95124d74bf2c72999
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/20/2018
---
# <a name="create-a-service-fabric-cluster"></a>Service Fabric kümesi oluştur

Bu örnek betik, bir X.509 sertifikası ile güvenli bir beş düğümlü Service Fabric kümesi oluşturur.  Bu komut otomatik olarak imzalanan bir sertifika oluşturur ve bunu yeni bir anahtar kasasına yükler. Sertifika aynı zamanda bir yerel dizine de kopyalanır.  Küme düğümleri üzerinde çalışan Windows veya Linux sürümünü seçmek için *-OS* parametresini ayarlayın.  Parametreleri gereken şekilde özelleştirin.

Gerekirse, bulunan yönergeleri kullanarak Azure PowerShell'i yükleme [Azure PowerShell Kılavuzu](/powershell/azure/overview) ve ardından çalıştırın `Login-AzureRmAccount` Azure ile bir bağlantı oluşturmak için. 

## <a name="sample-script"></a>Örnek komut dosyası

[!code-powershell[main](../../../powershell_scripts/service-fabric/create-secure-cluster/create-secure-cluster.ps1 "Create a Service Fabric cluster")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Komut dosyası örneği çalıştırdıktan sonra kaynak grubu, küme ve tüm ilgili kaynaklar kaldırmak için aşağıdaki komutu kullanılabilir.

```powershell
$groupname="mysfclustergroup"
Remove-AzureRmResourceGroup -Name $groupname -Force
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyasını aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [New-AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) | Yeni bir Service Fabric kümesi oluşturur. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](/powershell/azure/overview).

Azure Service Fabric ek Azure Powershell örnekleri bulunabilir [Azure PowerShell örnekleri](../service-fabric-powershell-samples.md).
