---
title: Windows VM uzantısı hatalarında sorun giderme | Microsoft Docs
description: Azure Windows VM uzantısı hatalarında sorun giderme hakkında bilgi edinin
services: virtual-machines-windows
documentationcenter: ''
author: kundanap
manager: gwallace
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
ms.openlocfilehash: f2b85e9a156d0e6264ec39282b803118963cbbbb
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67706654"
---
# <a name="troubleshooting-azure-windows-vm-extension-failures"></a>Azure Windows VM uzantısı hatalarında sorun giderme
[!INCLUDE [virtual-machines-common-extensions-troubleshoot](../../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a>Uzantı durumu görüntüleme
Azure Resource Manager şablonları, Azure Powershell'den çalıştırılabilir. Şablon yürütüldükten sonra uzantı durumunun Azure kaynak Gezgini veya komut satırı araçları üzerinden görüntülenebilir.

Örnek aşağıda verilmiştir:

Azure PowerShell:

      Get-AzVM -ResourceGroupName $RGName -Name $vmName -Status

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

## <a name="troubleshooting-extension-failures"></a>Uzantı sorunlarını giderme
### <a name="rerun-the-extension-on-the-vm"></a>VM uzantısı yeniden çalıştırın
Özel betik uzantısı kullanarak VM üzerinde betikleri çalıştırıyorsanız, burada VM başarıyla oluşturuldu, ancak komut dosyası başarısızdır hatayla bazen çalıştırabilirsiniz. Bu koşullar altında bu hatadan kurtulmak için önerilen yol uzantısını kaldırın ve şablonu yeniden çalıştırmayı sağlamaktır.
Not: Gelecekte bu işlevsellik uzantısını kaldırmayı gereksinimini kaldırmak için artırılmış.

#### <a name="remove-the-extension-from-azure-powershell"></a>Azure Powershell'den uzantısını kaldırma
    Remove-AzVMExtension -ResourceGroupName $RGName -VMName $vmName -Name "myCustomScriptExtension"

Uzantı kaldırıldıktan sonra şablonu VM üzerinde betikleri çalıştırma yeniden çalıştırılır olabilir.

