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
ms.date: 07/03/2018
ms.author: damaerte
ms.openlocfilehash: 5e318a0f64033aa0c4b306e547c11e1994afa229
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37861898"
---
# <a name="powershell-in-azure-cloud-shell-for-windows-users"></a>PowerShell için Azure Cloud Shell Windows kullanıcılar

Mayıs 2018'de yapılan değişiklikleri [duyurulan](https://azure.microsoft.com/blog/pscloudshellrefresh/) Azure Cloud shell'de PowerShell için.  Azure Cloud shell'de PowerShell deneyimi, Linux'ta PowerShell Core 6 sunulmuştur.
Bu değişiklik, Windows PowerShell 5.1 beklenenden farklı Cloud Shell'deki PowerShell hizmetinde bazı yönlerini vardır.

## <a name="case-sensitivity"></a>Büyük/küçük harfe duyarlılık

Windows dosya sistemi büyük/küçük harf duyarlıdır.  Linux'ta, dosya sistemi büyük/küçük harf duyarlıdır.
Daha önce buna `file.txt` ve `FILE.txt` aynı dosyanın farklı dosya olarak kabul edilir artık olarak kabul.
Doğru büyük/küçük harf kullanılmalıdır sırada `tab` dosya sisteminde tamamlanıyor.  Gibi PowerShell özel deneyimler `tab` cmdlet'ler, büyük küçük harfe duyarlı değildir. 

## <a name="windows-powershell-alias-vs-linux-utilities"></a>Windows PowerShell diğer vs Linux yardımcı programları

Varolan, Linux'ta gibi komutları `ls`, `sort`, ve `sleep` PowerShell diğer adlarını önceliklidir.  Eşdeğer komutları yanı sıra ortak kaldırılan diğer adlar aşağıda verilmiştir:  

|Diğer ad kaldırıldı   |Eşdeğer komutu   |
|---|---|
|`ls`     | `dir` <br> `Get-ChildItem` |
|`sort`   | `Sort-Object` |
|`sleep`  | `Start-Sleep` |

## <a name="persisting-home-vs-homeclouddrive"></a>$Home vs $home\clouddrive kalıcı yapma

Betikleri ve diğer dosyaları kendi bulut sürücülerine kalıcı olan kullanıcılar için $HOME dizininizin artık oturumları arasında kalıcıdır.

## <a name="powershell-profile"></a>PowerShell profili

Varsayılan olarak, bir PowerShell profili oluşturulmaz.  Profiliniz oluşturmak için bir `PowerShell` altında dizin `$HOME/.config`.  İçinde `$HOME/.config/PowerShell`, profilinizi adı altında oluşturduğunuz `Microsoft.PowerShell_profile.ps1`.

## <a name="whats-new-in-powershell-core-6"></a>PowerShell Core 6'da yenilikler nelerdir?

PowerShell Core 6'da yenilikler hakkında daha fazla bilgi için başvuru [PowerShell docs](https://docs.microsoft.com/powershell/scripting/whats-new/what-s-new-in-powershell-core-60?view=powershell-6) ve [PowerShell Core ile çalışmaya başlama](https://blogs.msdn.microsoft.com/powershell/2017/06/09/getting-started-with-powershell-core-on-windows-mac-and-linux/) blog gönderisi
