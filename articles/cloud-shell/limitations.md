---
title: Azure Cloud Shell sınırlamaları | Microsoft Docs
description: Azure Cloud Shell sınırlamaları genel bakış
services: azure
documentationcenter: ''
author: maertendMSFT
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/15/2018
ms.author: damaerte
ms.openlocfilehash: 8fd88221818d28c227c33719c03e522e815a408b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62097074"
---
# <a name="limitations-of-azure-cloud-shell"></a>Azure Cloud Shell sınırlamaları

Azure Cloud Shell, aşağıdaki bilinen sınırlamalara sahiptir:

## <a name="general-limitations"></a>Genel sınırlamalar

### <a name="system-state-and-persistence"></a>Sistem durumu ve kalıcılığı

Cloud Shell oturumunuzu sağlayan makine geçicidir ve oturumunuz için 20 dakika etkin olduktan sonra dönüştürülmeden. Cloud Shell'i Azure dosya paylaşımını bağlanmasını gerektirir. Sonuç olarak, aboneliğiniz Cloud shell'e erişim için depolama kaynaklarını ayarlama mümkün olması gerekir. Dikkat edilecek diğer noktalar şunlardır:

* Takılı depolamayla değişikliklerini içinde `$Home` dizin kalıcı olur.
* Azure dosya paylaşımlarını yalnızca içinde bağlanabilir, [bölgeye atanan](persisting-shell-storage.md#mount-a-new-clouddrive).
  * Bash hizmetinde çalıştırma `env` yap bölgenizi bulmak için `ACC_LOCATION`.

### <a name="browser-support"></a>Tarayıcı desteği

Cloud Shell'i Microsoft Edge, Microsoft Internet Explorer, Google Chrome, Mozilla Firefox ve Safari Apple'nın en son sürümlerini destekler. Safari özel modda desteklenmez.

### <a name="copy-and-paste"></a>Kopyala ve Yapıştır

[!INCLUDE [copy-paste](../../includes/cloud-shell-copy-paste.md)]

### <a name="for-a-given-user-only-one-shell-can-be-active"></a>Belirli bir kullanıcı için yalnızca bir kabuk etkin olabilir

Kullanıcılar yalnızca başlatabilir bir kabuk türünü bir kerede ya da **Bash** veya **PowerShell**. Ancak, Bash veya PowerShell aynı anda çalışan birden çok örneği olabilir. Bash veya PowerShell arasında menüsünü kullanarak takas Cloud yeniden başlatmak mevcut oturumlarını sonlandırır Shell neden olur. Alternatif olarak, PowerShell içinde bash yazarak çalıştırabileceğiniz `bash`, ve PowerShell içinde bash yazarak çalıştırabileceğiniz `pwsh`.

### <a name="usage-limits"></a>Kullanım sınırları

Cloud Shell'de etkileşimli kullanım durumları için tasarlanmıştır. Sonuç olarak, herhangi bir uzun süre çalışan etkileşimli olmayan oturum uyarı vermeden sonlandırılır.

## <a name="bash-limitations"></a>Bash sınırlamaları

### <a name="user-permissions"></a>Kullanıcı izinleri

İzinleri, sudo erişimi olmadan normal kullanıcı olarak ayarlanır. Dışında herhangi bir yükleme, `$Home` dizin kalıcı değil.

### <a name="editing-bashrc-or-profile"></a>.Bashrc veya $PROFILE düzenleme

.Bashrc veya Bunun yapılması, PowerShell'in $PROFILE dosya düzenleme beklenmeyen hatalar Cloud Shell'de neden olabileceği zaman uyarı alın.

## <a name="powershell-limitations"></a>PowerShell sınırlamaları

### <a name="azuread-module-name"></a>`AzureAD` Modül adı

`AzureAD` Modül adı şu anda `AzureAD.Standard.Preview`, modül aynı işlevselliği sağlar.

### <a name="sqlserver-module-functionality"></a>`SqlServer` modül işlevi

`SqlServer` Cloud Shell'de dahil modülü PowerShell Core yalnızca yayın öncesi desteği içeriyor. Özellikle, `Invoke-SqlCmd` henüz kullanıma sunulmamıştır.

### <a name="default-file-location-when-created-from-azure-drive"></a>Azure sürücüsünden oluşturduğunuzda varsayılan dosya konumu:

PowerShell cmdlet'lerini kullanarak, kullanıcılar Azure altındaki dosyaları oluşturulamıyor: sürücü. Kullanıcıların yeni dosyaları vim veya nano gibi diğer araçları kullanarak oluşturduğunuzda, dosyalar için kaydedilir `$HOME` varsayılan olarak. 

### <a name="gui-applications-are-not-supported"></a>GUI uygulamaları desteklenmez.

Kullanıcı bir Windows iletişim kutusu oluşturacak bir komut çalışırsa, bir hata iletisi aşağıdaki gibi görür: `Unable to load DLL 'IEFRAME.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)`.

### <a name="large-gap-after-displaying-progress-bar"></a>İlerleme çubuğunu görüntüleme sonra geniş boşluk

Kullanıcı bir ilerleme çubuğu, böyle bir sekme görüntüler bir eylem gerçekleştirirse içinde çalışırken Tamamlanıyor `Azure:` sürücü imleç düzgün ayarlanmamıştır ve ilerleme çubuğu daha önce olduğu boşluk görünür mümkündür.

## <a name="next-steps"></a>Sonraki adımlar

[Cloud Shell'i sorunlarını giderme](troubleshooting.md) <br>
[Bash için hızlı başlangıç](quickstart.md) <br>
[PowerShell için hızlı başlangıç](quickstart-powershell.md)
