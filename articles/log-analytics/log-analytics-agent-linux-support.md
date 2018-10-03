---
title: Azure Log Analytics Linux Aracısı sorunlarını giderme | Microsoft Docs
description: Belirtiler, nedenleri ve en sık karşılaşılan sorunlara yönelik çözümler, Log Analytics Linux Aracısı ile açıklanmaktadır.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/14/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: baa81a52d4b007cd690a2b01df642cd3775f7d6b
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2018
ms.locfileid: "48044145"
---
# <a name="how-to-troubleshoot-issues-with-the-linux-agent-for-log-analytics"></a>Log Analytics için Linux Aracısı ile ilgili sorunları giderme

Bu makale, Log Analytics için Linux Aracısı ile karşılaşabilirsiniz ve bunların çözülmesine yönelik olası çözümler önerir hatalarını giderme hakkında Yardım sağlar.

## <a name="issue-unable-to-connect-through-proxy-to-log-analytics"></a>Sorun: Log Analytics için Ara sunucu ile bağlantı kurulamıyor

### <a name="probable-causes"></a>Olası nedenleri
* Ekleme sırasında belirtilen proxy yanlış
* Log Analytics ve Azure Otomasyonu hizmet uç noktaları, veri merkezinizdeki izin verilenler listesinde değil 

### <a name="resolutions"></a>Çözümleri
1. Log Analytics hizmetine Reonboard seçeneği ile aşağıdaki komutu kullanarak Linux için OMS Aracısı ile `-v` etkin. Bu ayrıntılı çıkış aracısının OMS hizmetine proxy üzerinden bağlanma sağlar. 
`/opt/microsoft/omsagent/bin/omsadmin.sh -w <OMS Workspace ID> -s <OMS Workspace Key> -p <Proxy Conf> -v`

2. Bölümü gözden geçirin [proxy ayarlarını güncelleştirme](log-analytics-agent-manage.md#update-proxy-settings) aracının bir proxy sunucu üzerinden iletişim kurmak için düzgün şekilde yapılandırdığınızdan doğrulayın.    
* Çift aşağıdaki Log Analytics Hizmeti uç noktaların izin verilenler listesinde olup olmadığını denetleyin:

    |Aracı Kaynağı| Bağlantı Noktaları | Yön |
    |------|---------|----------|  
    |*.ods.opinsights.azure.com | Bağlantı noktası 443| Gelen ve giden |  
    |*.oms.opinsights.azure.com | Bağlantı noktası 443| Gelen ve giden |  
    |*.blob.core.windows.net | Bağlantı noktası 443| Gelen ve giden |  
    |*.azure-automation.net | Bağlantı noktası 443| Gelen ve giden | 

## <a name="issue-you-receive-a-403-error-when-trying-to-onboard"></a>Sorun: Bir 403 hatası ekleme çalışırken alıyorsunuz

### <a name="probable-causes"></a>Olası nedenleri
* Tarih ve saat Linux sunucusu üzerinde yanlış 
* Çalışma alanı kimliği ve çalışma alanı anahtarı doğru değil

### <a name="resolution"></a>Çözüm

1. Komut tarih ile Linux sunucunuzdaki zamanını kontrol edin. Saati geçerli saatten 15 dakika +/-ise, ardından ekleme başarısız olur. İçin doğru Bu güncelleştirme tarih ve/veya saat dilimi Linux sunucunuzun. 
2. Linux için OMS Aracısı'nın en son sürümü yüklü olmadığını doğrulayın.  En yeni sürümü artık zaman farkı, onboarding hataya neden olduğunu bildirir.
3. Doğru çalışma alanı Kimliğiniz ve çalışma alanı anahtarı bu konunun önceki kısımlarında yükleme yönergelerini kullanarak Reonboard.

## <a name="issue-you-see-a-500-and-404-error-in-the-log-file-right-after-onboarding"></a>Sorun: Sağ ekledikten sonra bir 500 ve 404 hatası günlük dosyasına bakın
Log Analytics çalışma alanına ilk Linux veri yükleme oluşma zamanı bilinen bir sorundur. Bu, gönderilen veya hizmet deneyimi olan verileri etkilemez.

## <a name="issue-you-are-not-seeing-any-data-in-the-azure-portal"></a>Sorun: Azure portalında herhangi bir veri görmediğinizden

### <a name="probable-causes"></a>Olası nedenleri

- Log Analytics hizmetinin eklenmesi başarısız oldu
- Log Analytics hizmetine bağlantı engellenir
- Veri Linux için OMS Aracısı yedeklenir

### <a name="resolutions"></a>Çözümleri
1. Onboarding Log Analytics hizmeti şu dosyayı varsa denetleyerek başarılı olup olmadığını kontrol edin: `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`
2. Reonboard kullanarak `omsadmin.sh` komut satırı yönergeleri
3. Bir ara sunucu kullanıyorsanız, daha önce sağlanan proxy çözümleme adımlarına bakın.
4. Linux için OMS Aracısı hizmetiyle iletişim kuramadığında bazı durumlarda, aracı üzerinde veri 50 MB'tır tam arabellek boyutu için sıraya alınır. Linux için OMS aracısı aşağıdaki komutu çalıştırarak yeniden başlatılması gerekiyor: `/opt/microsoft/omsagent/bin/service_control restart [<workspace id>]`. 

    >[!NOTE]
    >Aracı sürümü 1.1.0-28 ve daha sonra bu sorun düzeltilmiştir.

