---
title: "Azure günlük analizi Linux Aracısı sorunlarını giderme | Microsoft Docs"
description: "Belirtiler, nedenleri ve çözümleme en yaygın sorunları için günlük analizi Linux Aracısı ile açıklanmaktadır."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2018
ms.author: magoedte
ms.openlocfilehash: 80d7e39b284554ebfa8cac4488e1663b3e3648e8
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="how-to-troubleshoot-issues-with-the-linux-agent-for-log-analytics"></a>Günlük analizi için Linux Aracısı ile sorunlarını giderme

Bu makalede günlük analizi için Linux Aracısı ile karşılaşabilirsiniz ve bunları gidermek için olası çözümleri önerir hatalarında sorun giderme yardım sağlar.

## <a name="issue-unable-to-connect-through-proxy-to-log-analytics"></a>Sorun: Günlük analizi için proxy ile bağlantı kurulamıyor

### <a name="probable-causes"></a>Olası nedenler
* Onboarding işlemi sırasında belirtilen proxy yanlış
* Günlük analizi ve Azure Automation hizmet uç noktaları, veri merkezinizdeki listede değilsiniz 

### <a name="resolutions"></a>Çözümler
1. Günlük analizi hizmetine Reonboard seçeneği ile aşağıdaki komutu kullanarak Linux için OMS Aracısı ile `-v` etkin. Bu OMS hizmetine proxy üzerinden bağlanma Aracısı'nın ayrıntılı çıktı sağlar. 
`/opt/microsoft/omsagent/bin/omsadmin.sh -w <OMS Workspace ID> -s <OMS Workspace Key> -p <Proxy Conf> -v`

2. Bölümünü gözden [proxy ayarlarını güncelleştirmek](log-analytics-agent-manage.md#update-proxy-settings) düzgün bir şekilde yapılandırdığınız bir proxy sunucu üzerinden iletişim kurmak için aracıyı doğrulanamadı.    
* Aşağıdaki günlük analizi Hizmeti uç noktalarını Güvenilenler listesine olduğunu kontrol edin:

    |Aracı Kaynağı| Bağlantı Noktaları | Yön |
    |------|---------|----------|  
    |*.ods.opinsights.azure.com | Bağlantı noktası 443| Gelen ve giden |  
    |*.oms.opinsights.azure.com | Bağlantı noktası 443| Gelen ve giden |  
    |*.blob.core.windows.net | Bağlantı noktası 443| Gelen ve giden |  
    |*.azure-automation.net | Bağlantı noktası 443| Gelen ve giden | 

## <a name="issue-you-receive-a-403-error-when-trying-to-onboard"></a>Sorun: Bir 403 hatası için yerleşik çalışırken alıyorsunuz

### <a name="probable-causes"></a>Olası nedenler
* Tarih ve saat, Linux sunucusu üzerinde yanlış 
* Çalışma alanı kimliği ve çalışma alanı anahtarı doğru değil

### <a name="resolution"></a>Çözüm

1. Linux sunucunuzla komutu tarih zamanını denetleyin. Saati geçerli saatten 15 dakika +/-ise ekleme başarısız olur. Doğru Bu güncelleştirme için tarih ve/veya saat dilimi Linux sunucunuzun. 
2. Linux için OMS aracısının en son sürümünün yüklü olduğunu doğrulayın.  En yeni sürümü şimdi zaman farkı, onboarding hataya neden olduğunu bildirir.
3. Doğru çalışma alanı kimliği ve çalışma alanı bu konunun önceki kısımlarında yükleme yönergeleri izleyerek anahtarı kullanarak Reonboard.

## <a name="issue-you-see-a-500-and-404-error-in-the-log-file-right-after-onboarding"></a>Sorun: Sağ onboarding sonra 500 ve 404 Hata günlük dosyasına bakın
Bu ilk Linux veri yükleme günlük analizi çalışma alanına ortaya çıkan bilinen bir sorundur. Bu verilerin gönderilen veya servis deneyimi olmasıyla etkilemez.

## <a name="issue-you-are-not-seeing-any-data-in-the-azure-portal"></a>Sorun: Azure portalındaki herhangi bir veri gördüğünüz değil

### <a name="probable-causes"></a>Olası nedenler

- Günlük analizi hizmetine ekleme başarısız oldu
- Günlük analizi hizmeti ile bağlantı engellendi
- Linux veriler için OMS aracısının yedeklenir

### <a name="resolutions"></a>Çözümler
1. Onboarding günlük analizi hizmeti aşağıdaki dosya olup olmadığını kontrol ederek başarılı olup olmadığını kontrol edin: `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`
2. Reonboard kullanarak `omsadmin.sh` komut satırı yönergeleri
3. Bir proxy sunucu kullanıyorsanız, daha önce sağlanan proxy çözüm adımlarına bakın.
4. Linux için OMS Aracısı hizmetiyle iletişim kuramadığında bazı durumlarda, aracı veri 50 MB'tır tam arabellek boyutu için sıraya alındı. Linux için OMS aracısı aşağıdaki komutu çalıştırarak yeniden başlatılması gerekir: `/opt/microsoft/omsagent/bin/service_control restart [<workspace id>]`. 

    >[!NOTE]
    >Aracı sürümü 1.1.0-28 ve daha sonra bu sorun düzeltilmiştir.

