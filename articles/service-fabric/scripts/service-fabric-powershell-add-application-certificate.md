---
title: "Azure PowerShell Betiği örnek - uygulama cert bir kümeye ekleme | Microsoft Docs"
description: "Azure PowerShell Betiği örnek - Service Fabric kümesi için bir uygulama sertifika ekleyin."
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
ms.openlocfilehash: 8a000d797c3bd10606d297ed8da67229fe0c8a86
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="add-an-application-certificate-to-a-service-fabric-cluster"></a>Service Fabric kümesi için bir uygulama sertifika Ekle

Bu örnek komut dosyası içinde belirtilen Azure anahtar kasası otomatik olarak imzalanan bir sertifika oluşturur ve Service Fabric kümesinin tüm düğümlerine yükler. Sertifikanın da yerel bir klasöre yükler. İndirilen sertifika adını anahtar Kasası'nda sertifikanın adı ile aynıdır. Parametreleri gereken şekilde özelleştirin.

Gerekirse, bulunan yönergeleri kullanarak Azure PowerShell'i yükleme [Azure PowerShell Kılavuzu](/powershell/azure/overview) ve ardından çalıştırın `Login-AzureRmAccount` Azure ile bir bağlantı oluşturmak için. 

## <a name="sample-script"></a>Örnek komut dosyası

[!code-powershell[main](../../../powershell_scripts/service-fabric/add-application-certificate/add-new-application-certificate.ps1 "Add an application certificate to a cluster")]

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyasını aşağıdaki komutları kullanır: komutu belirli belgeleri tablo bağlanan her komutu.

| Komut | Notlar |
|---|---|
| [Ekleme AzureRmServiceFabricApplicationCertificate](/powershell/module/azurerm.servicefabric/Add-AzureRmServiceFabricApplicationCertificate) | Kümeyi oluşturan sanal makine ölçek kümesi için yeni bir uygulama sertifika ekleyin.  |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](/powershell/azure/overview).

Azure Service Fabric ek Azure Powershell örnekleri bulunabilir [Azure PowerShell örnekleri](../service-fabric-powershell-samples.md).
