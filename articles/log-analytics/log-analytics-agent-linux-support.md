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
ms.date: 12/07/2017
ms.author: magoedte
ms.openlocfilehash: 4f9eaa330ba90052080fc18def974d5dec43c1e0
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2017
---
# <a name="how-to-troubleshoot-issues-with-the-linux-agent-for-log-analytics"></a>Günlük analizi için Linux Aracısı ile sorunlarını giderme

Bu makalede günlük analizi için Linux Aracısı ile karşılaşabilirsiniz ve bunları gidermek için olası çözümleri önerir hatalarında sorun giderme yardım sağlar.

### <a name="issue-unable-to-connect-through-proxy-to-log-analytics"></a>Sorun: Günlük analizi için proxy ile bağlantı kurulamıyor

#### <a name="probable-causes"></a>Olası nedenler
* Onboarding işlemi sırasında belirtilen proxy yanlış
* Günlük analizi ve Azure Automation hizmet uç noktaları, veri merkezinizdeki listede değilsiniz 

#### <a name="resolutions"></a>Çözümler
1. Günlük analizi hizmetine Reonboard seçeneği ile aşağıdaki komutu kullanarak Linux için OMS Aracısı ile `-v` etkin. Bu OMS hizmetine proxy üzerinden bağlanma Aracısı'nın ayrıntılı çıktı sağlar. 
`/opt/microsoft/omsagent/bin/omsadmin.sh -w <OMS Workspace ID> -s <OMS Workspace Key> -p <Proxy Conf> -v`

2. Bölümünü gözden [aracı kullanmak için bir proxy sunucusu veya OMS ağ geçidi yapılandırma](#configuring the-agent-for-use-with-a-proxy-server-or-oms-gateway) düzgün bir şekilde yapılandırdığınız bir proxy sunucu üzerinden iletişim kurmak için aracıyı doğrulanamadı.    
* Aşağıdaki günlük analizi Hizmeti uç noktalarını Güvenilenler listesine olduğunu kontrol edin:

    |Aracı Kaynağı| Bağlantı Noktaları |  
    |------|---------|  
    |*.ods.opinsights.azure.com | Bağlantı noktası 443|   
    |*.oms.opinsights.azure.com | Bağlantı noktası 443|   
    |ods.systemcenteradvisor.com | Bağlantı noktası 443|   
    |*.BLOB.Core.Windows.NET/ | Bağlantı noktası 443|   

### <a name="issue-you-receive-a-403-error-when-trying-to-onboard"></a>Sorun: Bir 403 hatası için yerleşik çalışırken alıyorsunuz

#### <a name="probable-causes"></a>Olası nedenler
* Tarih ve saat, Linux sunucusu üzerinde yanlış 
* Çalışma alanı kimliği ve çalışma alanı anahtarı doğru değil

#### <a name="resolution"></a>Çözüm

1. Linux sunucunuzla komutu tarih zamanını denetleyin. Saati geçerli saatten 15 dakika +/-ise ekleme başarısız olur. Doğru Bu güncelleştirme için tarih ve/veya saat dilimi Linux sunucunuzun. 
2. Linux için OMS aracısının en son sürümünün yüklü olduğunu doğrulayın.  En yeni sürümü şimdi zaman farkı, onboarding hataya neden olduğunu bildirir.
3. Doğru çalışma alanı kimliği ve çalışma alanı bu konunun önceki kısımlarında yükleme yönergeleri izleyerek anahtarı kullanarak Reonboard.

### <a name="issue-you-see-a-500-and-404-error-in-the-log-file-right-after-onboarding"></a>Sorun: Sağ onboarding sonra 500 ve 404 Hata günlük dosyasına bakın
Bu ilk Linux veri yükleme günlük analizi çalışma alanına ortaya çıkan bilinen bir sorundur. Bu verilerin gönderilen veya servis deneyimi olmasıyla etkilemez.

### <a name="issue-you-are-not-seeing-any-data-in-the-azure-portal"></a>Sorun: Azure portalındaki herhangi bir veri gördüğünüz değil

#### <a name="probable-causes"></a>Olası nedenler

- Günlük analizi hizmetine ekleme başarısız oldu
- Günlük analizi hizmeti ile bağlantı engellendi
- Linux veriler için OMS aracısının yedeklenir

#### <a name="resolutions"></a>Çözümler
1. Onboarding günlük analizi hizmeti aşağıdaki dosya olup olmadığını kontrol ederek başarılı olup olmadığını kontrol edin:`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`
2. Reonboard kullanarak `omsadmin.sh` komut satırı yönergeleri
3. Bir proxy sunucu kullanıyorsanız, daha önce sağlanan proxy çözüm adımlarına bakın.
4. Linux için OMS Aracısı hizmetiyle iletişim kuramadığında bazı durumlarda, aracı veri 50 MB'tır tam arabellek boyutu için sıraya alındı. Linux için OMS aracısı aşağıdaki komutu çalıştırarak yeniden başlatılması gerekir: `/opt/microsoft/omsagent/bin/service_control restart [<workspace id>]`. 

    >[!NOTE]
    >Aracı sürümü 1.1.0-28 ve daha sonra bu sorun düzeltilmiştir.

### <a name="issue-omsagent-creates-excessive-number-of-user-process-on-computer-and-never-terminates-them"></a>Sorun: OMSAgent kullanıcı işlemi çok fazla sayıda bilgisayarda oluşturur ve asla sona erer
Linux sanal makineleri yönetme desteği yönetim çözümleri etkinleştirdiğinizde, Linux Aracısı'nı işlemlerde bir dizi başlatır. Ancak işlem sona ermeden önce başka bir işlem bilinen bir sorun nedeniyle başlatılır. 

#### <a name="resolutions"></a>Çözümler
OMSAgent tarafından oluşturulan kullanıcı işlemleri sayısını değiştirmek için omsadmin.sh kullanarak aracısını yapılandırın.  Varsayılan olarak oluşturulan işlemlerin sayısı 75 ve sınırını değiştirmeden önce ilk kaç OMSAgent işlemleri çalışmakta olan görmek için aşağıdaki komutu çalıştırmanız gerekir: `ps aux | grep -E '^omsagent' | wc -l`.  
Ne geçerli aşağıdaki komutu çalıştırarak sınırı doğrulayabilirsiniz:`cat /etc/security/limits.conf | grep -E '^omsagent'`

Ya da bir özel işlem sınırı yapılandırmak veya geri varsayılan değeri için işlem sınırı ayarlamak için aşağıdaki komutları kullanın.

1. OMSAgent işlem sınırı ayarlamak için: `sudo /opt/microsoft/omsagent/bin/omsadmin.sh -n <specific number limit>`.<br>Bunu ayarlanabilir alt sınırı 5 olduğuna dikkat edin.  

2. Varsayılan değere geri OMSAgent işlem sınırı ayarlamak için:`sudo /opt/microsoft/omsagent/bin/omsadmin.sh -N`

Aşağıdaki komutu çalıştırarak yeni bir ayar uygulanmış olduğunu doğrulayın: `cat /etc/security/limits.conf | grep -E '^omsagent'`.  Uygulanan yeni yapılandırma görmüyorsanız, işlem sınırı çok düşük ayarlanmış olabilir.  