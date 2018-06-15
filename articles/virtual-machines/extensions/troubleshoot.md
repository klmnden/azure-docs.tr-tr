---
title: Windows VM uzantısı hataları sorunlarını giderme | Microsoft Docs
description: Azure Windows VM uzantısı hatalarını giderme hakkında bilgi edinin
services: virtual-machines-windows
documentationcenter: ''
author: kundanap
manager: jeconnoc
editor: ''
tags: top-support-issue,azure-resource-manager
ms.assetid: 878ab9b6-c3e6-40be-82d4-d77fecd5030f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/29/2016
ms.author: kundanap
ms.openlocfilehash: 9973eaa7e930d38e78289219e726b5934d82ee86
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "33945424"
---
# <a name="troubleshooting-azure-windows-vm-extension-failures"></a>Azure Windows VM uzantısı hataları sorunlarını giderme
[!INCLUDE [virtual-machines-common-extensions-troubleshoot](../../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a>Uzantı durumunu görüntüleme
Azure Resource Manager şablonları Azure Powershell'den çalıştırılabilir. Şablon yürütüldükten sonra uzantı durumunu Azure kaynak Gezgini veya komut satırı araçları üzerinden görüntülenebilir.

Örnek aşağıda verilmiştir:

Azure PowerShell:

      Get-AzureRmVM -ResourceGroupName $RGName -Name $vmName -Status

Örnek çıktı aşağıdaki gibidir:

      Extensions:  {
      "ExtensionType": "Microsoft.Compute.CustomScriptExtension",
      "Name": "myCustomScriptExtension",
      "SubStatuses": [
        {
          "Code": "ComponentStatus/StdOut/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "    Directory: C:\\temp\\n\\n\\nMode                LastWriteTime     Length Name
              \\n----                -------------     ------ ----                              \\n-a---          9/1/2015   2:03 AM         11
              test.txt                          \\n\\n",
                      "Time": null
          },
        {
          "Code": "ComponentStatus/StdErr/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "",
          "Time": null
        }
    }
  ]

## <a name="troubleshooting-extension-failures"></a>Uzantı hataları sorunlarını giderme
### <a name="rerun-the-extension-on-the-vm"></a>VM uzantısı yeniden çalıştırın
Özel betik uzantısının kullanarak VM betikleri çalıştırıyorsanız, burada VM başarıyla oluşturuldu, ancak komut başarısız oldu bir hatayla bazen çalıştırabilir. Bu koşullar altında bu hatadan kurtarmak için önerilen uzantıyı kaldırmak ve şablonu yeniden çalıştırın yoldur.
Not: ileride bu işlevselliği uzantısını kaldırma gereksinimini kaldırmak için Gelişmiş.

#### <a name="remove-the-extension-from-azure-powershell"></a>Azure PowerShell uzantısını kaldırma
    Remove-AzureRmVMExtension -ResourceGroupName $RGName -VMName $vmName -Name "myCustomScriptExtension"

Uzantı kaldırıldıktan sonra şablonu VM komut dosyalarını çalıştırmak için yeniden yürütülen olabilir.

