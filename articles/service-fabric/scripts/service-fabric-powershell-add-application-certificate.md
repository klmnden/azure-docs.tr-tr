---
title: Azure PowerShell Betiği Örneği - Bir kümeye uygulama sertifikası ekleme | Microsoft Docs
description: Azure PowerShell Betiği Örneği - Service Fabric kümesine uygulama sertifikası ekleme.
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
ms.openlocfilehash: be097f88f774df9e4a6429af444c6c742737f4c9
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
ms.locfileid: "31598719"
---
# <a name="add-an-application-certificate-to-a-service-fabric-cluster"></a>Service Fabric kümesine uygulama sertifikası ekleme

Bu örnek betik, belirtilen Azure anahtar kasasında otomatik olarak imzalanan bir sertifika oluşturur ve bunu Service Fabric kümesinin tüm düğümlerine yükler. Sertifika, yerel bir klasöre de indirilir. İndirilen sertifikanın adı, anahtar kasasındaki sertifikanın adıyla aynıdır. Parametreleri gereken şekilde özelleştirin.

Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure/overview) bulunan yönergeleri kullanarak Azure PowerShell’i yükleyin ve ardından Azure ile bir bağlantı oluşturmak için `Connect-AzureRmAccount` öğesini çalıştırın. 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/service-fabric/add-application-certificate/add-new-application-certificate.ps1 "Add an application certificate to a cluster")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır: Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Add-AzureRmServiceFabricApplicationCertificate](/powershell/module/azurerm.servicefabric/Add-AzureRmServiceFabricApplicationCertificate) | Kümeyi oluşturan sanal makine ölçek kümesine yeni bir uygulama sertifikası ekleyin.  |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Azure Service Fabric için ek Azure PowerShell örneklerini [Azure PowerShell örnekleri](../service-fabric-powershell-samples.md) bölümünde bulabilirsiniz.
