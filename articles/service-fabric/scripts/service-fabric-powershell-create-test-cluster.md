---
title: "Azure PowerShell Betiği örnek - Service Fabric kümesi oluşturun. | Microsoft Docs"
description: "Azure PowerShell Betiği örnek - üç düğümü test Service Fabric kümesi oluşturur."
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
ms.date: 12/12/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 03348efa4ebdaed987df73756c6b57da0cc76fb5
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2017
---
# <a name="create-a-three-node-test-service-fabric-cluster"></a>Üç düğümü test Service Fabric kümesi oluştur

Bu örnek betik, bir X.509 sertifikası ile güvenli üç düğümü test Service Fabric kümesi oluşturur. Güvenli bir şekilde yükseltme yapmak ve (aynı anda gerçekleşmeden yok sürece) tek tek düğüm hatalarını varlığını sürdürmesi için üç düğüm Küme yapılandırması geliştirme ve test için desteklenir. Üretim kümesi eşzamanlı hatalarına dayanıklı olması için beş veya daha fazla düğüm gerektiriyor.  

Komut otomatik olarak imzalanan bir sertifika oluşturur ve küme ile aynı kaynak grubunda oluşturulan yeni bir anahtar kasasına yükler. Sertifika aynı zamanda bir yerel dizine de kopyalanır.  Küme düğümleri üzerinde çalışan Windows veya Linux sürümünü seçmek için *-OS* parametresini ayarlayın.  Parametreleri gereken şekilde özelleştirin.

Gerekirse, bulunan yönergeleri kullanarak Azure PowerShell'i yükleme [Azure PowerShell Kılavuzu](/powershell/azure/overview) ve ardından çalıştırın `Login-AzureRmAccount` Azure ile bir bağlantı oluşturmak için. 

## <a name="sample-script"></a>Örnek komut dosyası

[!code-powershell[main](../../../powershell_scripts/service-fabric/create-test-cluster/create-test-cluster.ps1 "Create a test Service Fabric cluster")]

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
| [AzureRmServiceFabricCluster yeni](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) | Yeni bir Service Fabric kümesi oluşturur. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](/powershell/azure/overview).

Azure Service Fabric ek Azure Powershell örnekleri bulunabilir [Azure PowerShell örnekleri](../service-fabric-powershell-samples.md).
