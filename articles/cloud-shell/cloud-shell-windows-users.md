---
title: Azure için Cloud Shell Windows kullanıcıları | Microsoft Docs
description: Linux sistemleri ile tanıdık olmayan kullanıcılar için Kılavuzu
services: azure
documentationcenter: ''
author: maertendMSFT
manager: hemantm
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/03/2018
ms.author: damaerte
ms.openlocfilehash: 4fc4f6523eb19294cabdf6b5b910dd346a877502
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62095346"
---
# <a name="powershell-in-azure-cloud-shell-for-windows-users"></a>PowerShell için Azure Cloud Shell Windows kullanıcılar

Mayıs 2018'de yapılan değişiklikleri [duyurulan](https://azure.microsoft.com/blog/pscloudshellrefresh/) Azure Cloud shell'de PowerShell için.
Azure Cloud Shell şimdi çalıştırmalarında PowerShell deneyimi [PowerShell Core 6](https://github.com/powershell/powershell) Linux ortamında.
Bu değişiklik, olabilir bir Windows PowerShell gerekenin karşılaştırıldığında Cloud shell'de PowerShell deneyiminde bazı farklılıklar deneyimi.

## <a name="file-system-case-sensitivity"></a>Dosya sistemi büyük/küçük harfe duyarlılık

Linux üzerinde dosya sistemi büyük/küçük harfe iken dosya sistemi Windows, duyarlıdır.
Daha önce `file.txt` ve `FILE.txt` aynı dosyaya olabilir, ancak bunlar farklı dosyalar olarak değerlendirilir artık kabul.
Doğru büyük/küçük harf kullanılmalıdır sırada `tab-completing` dosya sistemindeki.
Gibi PowerShell özel deneyimler `tab-completing` cmdlet adları, parametreler ve değerler büyük küçük harfe duyarlı değildir.

## <a name="windows-powershell-aliases-vs-linux-utilities"></a>Windows PowerShell diğer adlar vs Linux yardımcı programları

Yerleşik Linux komutları, aynı adları gibi bazı mevcut PowerShell diğer sahip `cat`,`ls`, `sort`, `sleep`vb. PowerShell Core 6'da, yerleşik Linux komutlarıyla birbiriyle çakışır diğer adlar kaldırıldı.
Kaldırılan ortak diğer adlar ve bunun yanı sıra, eşdeğer komutlar aşağıda verilmiştir:  

|Diğer ad kaldırıldı   |Eşdeğer komutu   |
|---|---|
|`cat`    | `Get-Content` |
|`curl`   | `Invoke-WebRequest` |
|`diff`   | `Compare-Object` |
|`ls`     | `dir` <br> `Get-ChildItem` |
|`mv`     | `Move-Item`   |
|`rm`     | `Remove-Item` |
|`sleep`  | `Start-Sleep` |
|`sort`   | `Sort-Object` |
|`wget`   | `Invoke-WebRequest` |

## <a name="persisting-home"></a>Kalıcı $HOME

Daha önceki kullanıcılar yalnızca betikler ve diğer dosyaları kendi bulut sürücülerine kalıcı olamadı.
Şimdi, kullanıcının $HOME dizininizin oturumları arasında kalıcıdır.

## <a name="powershell-profile"></a>PowerShell profili

Varsayılan olarak, bir kullanıcının PowerShell profilini oluşturulmaz.
Profiliniz oluşturmak için bir `PowerShell` altında dizin `$HOME/.config`.

```azurepowershell-interactive
mkdir (Split-Path $profile.CurrentUserAllHosts)
```

Altında `$HOME/.config/PowerShell`, profili dosyalarınızı - oluşturabilirsiniz `profile.ps1` ve/veya `Microsoft.PowerShell_profile.ps1`.

## <a name="whats-new-in-powershell-core-6"></a>PowerShell Core 6'da yenilikler nelerdir?

PowerShell Core 6'da yenilikler hakkında daha fazla bilgi için başvuru [PowerShell docs](https://docs.microsoft.com/powershell/scripting/whats-new/what-s-new-in-powershell-core-60?view=powershell-6) ve [PowerShell Core ile çalışmaya başlama](https://blogs.msdn.microsoft.com/powershell/2017/06/09/getting-started-with-powershell-core-on-windows-mac-and-linux/) blog gönderisi.
