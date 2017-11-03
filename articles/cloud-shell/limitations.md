---
title: "Azure bulut Kabuğu (Önizleme) kısıtlamaları | Microsoft Docs"
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
ms.date: 09/25/2017
ms.author: juluk
ms.openlocfilehash: 92c8e4c205043f6c5c2925d9197270fb720969a3
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2017
---
# <a name="limitations-of-azure-cloud-shell"></a>Azure bulut Kabuk sınırlamaları

Azure bulut Kabuk aşağıdaki bilinen sınırlamalara sahiptir:

## <a name="general-limitations"></a>Genel sınırlamalar

### <a name="system-state-and-persistence"></a>Sistem durumu ve sürdürme

Bulut Kabuk oturumunuz sağlar makine geçicidir ve oturumunuz için 20 dakika etkin değil sonra geri dönüştürüldüğünde. Bulut Kabuk bir dosya paylaşımı bağlanmasını gerektirir. Sonuç olarak, aboneliğiniz bulut Kabuk erişmek için depolama kaynakları ayarlamak kurabilmesi gerekir. Diğer konular şunlardır:

* Takılı depolamayla yalnızca değişiklikleri içinde `clouddrive` dizin kaldı. Bash içinde `$Home` dizin de kalıcı.
* Dosya paylaşımları takılı yalnızca içinden, [bölgeye atanan](persisting-shell-storage.md#mount-a-new-clouddrive).
  * Bash'te, çalıştırmak `env` olarak ayarlayın, bölgenizdeki bulmak için `ACC_LOCATION`.
* Azure dosyaları yalnızca yerel olarak yedekli depolama ve coğrafi olarak yedekli depolama hesaplarını destekler.

### <a name="browser-support"></a>Tarayıcı desteği

Bulut Kabuğu'nu Microsoft Edge, Microsoft Internet Explorer, Google Chrome, Mozilla Firefox ve Apple Safari en son sürümlerini destekler. Safari özel modunda desteklenmiyor.

### <a name="copy-and-paste"></a>Kopyala ve Yapıştır

[!include [copy-paste](../../includes/cloud-shell-copy-paste.md)]

### <a name="for-a-given-user-only-one-shell-can-be-active"></a>Belirli bir kullanıcı için yalnızca bir kabuk etkin olabilir

Kullanıcılar yalnızca başlatma Kabuk bir tür aynı anda ya da **Bash** veya **PowerShell**. Bununla birlikte, Bash veya PowerShell aynı anda çalışan birden çok örneği olabilir. Hangi oturumlarına sonlandırır Bash veya PowerShell nedenleri arasında bulut Kabuğu'nu yeniden başlatmak için değiştirme.

### <a name="usage-limits"></a>Kullanım sınırları

Bulut Kabuk etkileşimli kullanım durumları için tasarlanmıştır. Sonuç olarak, tüm uzun süre çalışan etkileşimli olmayan oturumlar uyarmadan sonlandırılır.

## <a name="bash-limitations"></a>Bash sınırlamaları

### <a name="user-permissions"></a>Kullanıcı izinleri

İzinler, sudo erişimi olmadan normal kullanıcı olarak ayarlanır. Dışında herhangi bir yüklemesi, `$Home` dizin kalıcı değildir.
Belirli komutları içinde rağmen `clouddrive` gibi dizin `git clone`, uygun izinlere sahip değilsiniz, `$Home` dizin izinlere sahiptir.

### <a name="editing-bashrc"></a>.Bashrc düzenleme

Bunun yapılması .bashrc düzenleme beklenmeyen hatalara bulut Kabuğu'nda neden olabilir. uyarı alın.

### <a name="bashhistory"></a>.bash_history

Geçmişinizi bash komutların bulut Kabuk oturum kesintisi veya eşzamanlı oturum nedeniyle tutarsız olabilir.

## <a name="powershell-limitations"></a>PowerShell sınırlamaları

### <a name="slow-startup-time"></a>Yavaş başlangıç zamanı

Azure bulut Kabuk PowerShell'de Önizleme sırasında başlatmak için 60 saniye sürebilir.

### <a name="no-home-directory-persistence"></a>No $Home dizin kalıcılığı

Yazılan veri `$Home` herhangi bir uygulama tarafından (gibi: git, VIM ve diğerleri) PowerShell oturumlarında devam etmez. Geçici bir çözüm için [Burada gördüğünüz](troubleshooting.md#powershell-resolutions).

## <a name="next-steps"></a>Sonraki adımlar

[Bulut Kabuk sorunlarını giderme](troubleshooting.md) <br>
[Bash için hızlı başlangıç](quickstart.md) <br>
[PowerShell için hızlı başlangıç](quickstart-powershell.md)
