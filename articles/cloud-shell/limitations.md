---
title: "Azure bulut Kabuk kısıtlamaları | Microsoft Docs"
description: "Azure bulut Kabuk sınırlamaları genel bakış"
services: azure
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/15/2018
ms.author: juluk
ms.openlocfilehash: 245e2e1ca52e7d3c5bd22d5f2569e3e8d7ae6671
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="limitations-of-azure-cloud-shell"></a>Azure bulut Kabuk sınırlamaları

Azure bulut Kabuk aşağıdaki bilinen sınırlamalara sahiptir:

## <a name="general-limitations"></a>Genel sınırlamalar

### <a name="system-state-and-persistence"></a>Sistem durumu ve sürdürme

Bulut Kabuk oturumunuz sağlar makine geçicidir ve oturumunuz için 20 dakika etkin değil sonra geri dönüştürüldüğünde. Bulut Kabuk Azure dosya paylaşımının bağlanmasını gerektirir. Sonuç olarak, aboneliğiniz bulut Kabuk erişmek için depolama kaynakları ayarlamak kurabilmesi gerekir. Diğer konular şunlardır:

* Takılı depolamayla yalnızca değişiklikleri içinde `clouddrive` dizin kaldı. Bash içinde `$Home` dizin de kalıcı.
* Azure dosya paylaşımları takılı yalnızca içinden, [bölgeye atanan](persisting-shell-storage.md#mount-a-new-clouddrive).
  * Bash'te, çalıştırmak `env` olarak ayarlayın, bölgenizdeki bulmak için `ACC_LOCATION`.

### <a name="browser-support"></a>Tarayıcı desteği

Bulut Kabuğu'nu Microsoft Edge, Microsoft Internet Explorer, Google Chrome, Mozilla Firefox ve Apple Safari en son sürümlerini destekler. Safari özel modunda desteklenmiyor.

### <a name="copy-and-paste"></a>Kopyala ve Yapıştır

[!INCLUDE [copy-paste](../../includes/cloud-shell-copy-paste.md)]

### <a name="for-a-given-user-only-one-shell-can-be-active"></a>Belirli bir kullanıcı için yalnızca bir kabuk etkin olabilir

Kullanıcılar yalnızca başlatma Kabuk bir tür aynı anda ya da **Bash** veya **PowerShell**. Bununla birlikte, Bash veya PowerShell aynı anda çalışan birden çok örneği olabilir. Hangi oturumlarına sonlandırır Bash veya PowerShell nedenleri arasında bulut Kabuğu'nu yeniden başlatmak için değiştirme.

### <a name="usage-limits"></a>Kullanım sınırları

Bulut Kabuk etkileşimli kullanım durumları için tasarlanmıştır. Sonuç olarak, tüm uzun süre çalışan etkileşimli olmayan oturumlar uyarmadan sonlandırılır.

## <a name="bash-limitations"></a>Bash sınırlamaları

### <a name="user-permissions"></a>Kullanıcı izinleri

İzinler, sudo erişimi olmadan normal kullanıcı olarak ayarlanır. Dışında herhangi bir yüklemesi, `$Home` dizin kalıcı değildir.

### <a name="editing-bashrc"></a>.Bashrc düzenleme

Bunun yapılması .bashrc düzenleme beklenmeyen hatalara bulut Kabuğu'nda neden olabilir. uyarı alın.

## <a name="powershell-limitations"></a>PowerShell sınırlamaları

### <a name="slow-startup-time"></a>Yavaş başlangıç zamanı

Azure bulut Kabuğu (Önizleme) PowerShell Önizleme sırasında başlatmak için 60 saniye sürebilir.

### <a name="no-home-directory-persistence"></a>No $Home dizin kalıcılığı

Yazılan veri `$Home` herhangi bir uygulama tarafından (gibi: git, VIM ve diğerleri) PowerShell oturumlarında devam etmez. Geçici bir çözüm için [Burada gördüğünüz](troubleshooting.md#powershell-troubleshooting).

### <a name="default-file-location-when-created-from-azure-drive"></a>Azure sürücüsünden oluşturduğunuzda varsayılan dosya konumu:

PowerShell cmdlet'lerini kullanarak, kullanıcılar dosyaları Azure sürücüsü altında oluşturulamıyor. Kullanıcıların yeni dosyaları VIM veya nano, gibi diğer araçları kullanarak oluşturduğunuzda dosyalar varsayılan olarak C:\Users klasörüne kaydedilir. 

### <a name="gui-applications-are-not-supported"></a>GUI uygulamaları desteklenmez.

Kullanıcı bir Windows iletişim kutusu gibi oluşturacak bir komut çalıştırırsa `Connect-AzureAD` veya `Login-AzureRMAccount`, bir görür bir hata iletisi gibi: `Unable to load DLL 'IEFRAME.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)`.

## <a name="next-steps"></a>Sonraki adımlar

[Bulut Kabuk sorunlarını giderme](troubleshooting.md) <br>
[Bash için hızlı başlangıç](quickstart.md) <br>
[PowerShell için hızlı başlangıç](quickstart-powershell.md)
