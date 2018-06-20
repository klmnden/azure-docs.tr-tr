---
title: Ağ Azure günlük analizi Performans İzleyicisi çözümde | Microsoft Docs
description: Hizmet uç noktası Yöneticisi özelliği ağ Performans İzleyicisi'nde ağ bağlantısı açık bir TCP bağlantı noktasına sahip herhangi bir uç nokta izlemek için kullanın.
services: log-analytics
documentationcenter: ''
author: abshamsft
manager: carmonm
editor: ''
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2018
ms.author: abshamsft
ms.openlocfilehash: 05abd943d85fcdd709143bf7fce221dcdfb86011
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36215108"
---
# <a name="service-endpoint-monitor"></a>Hizmet uç noktası İzleyicisi

Hizmet uç noktası izleme özelliği kullanabileceğiniz [Ağ Performansı İzleyicisi](log-analytics-network-performance-monitor.md) açık bir TCP bağlantı noktasına sahip herhangi bir uç nokta için ağ bağlantısını izlemeniz. Web siteleri, SaaS uygulamaları, PaaS uygulamaları ve SQL veritabanları gibi uç noktaları içerir. 

Hizmet uç noktası İzleyicisi ile aşağıdaki işlevleri gerçekleştirebilirsiniz: 

- Uygulamalar ve Ağ Hizmetleri Ağ bağlantısını birden çok şube ofisleri veya konumları izleyin. Uygulamalar ve ağ hizmetlerini Office 365, Dynamics CRM, iç iş kolu satır uygulama ve SQL veritabanlarını içerir.
- Office 365 ve Dynamics 365 uç noktaları için ağ bağlantısını izlemeniz yerleşik testleri kullanın. 
- Yanıt süresi, ağ gecikme süresi ve paket kaybı deneyimli uç noktasına bağlanırken belirler.
- Zayıf uygulama performans nedeniyle ağ veya uygulama sağlayıcısının son bazı sorunu nedeniyle olup olmadığını belirler.
- Her bir topoloji Haritası atlamada katkıda bulunan gecikme görüntüleyerek zayıf uygulama performans neden olabilecek ağ üzerinde etkin noktalar tanımlayın.


![Hizmet uç noktası İzleyicisi](media/log-analytics-network-performance-monitor/service-endpoint-intro.png)


## <a name="configuration"></a>Yapılandırma 
Ağ Performansı İzleyicisi Yapılandırması'nı açmak için açın [Ağ Performansı İzleyicisi çözüm](log-analytics-network-performance-monitor.md) seçip **yapılandırma**.

![Ağ Performans İzleyicisi'ni yapılandırma](media/log-analytics-network-performance-monitor/npm-configure-button.png)


### <a name="configure-operations-management-suite-agents-for-monitoring"></a>İzleme için Operations Management Suite aracıları yapılandırma
Böylece, düğümlerinden topoloji hizmet uç noktası çözüm bulabilir izlemek için kullanılan düğümlerde aşağıdaki güvenlik duvarı kurallarını etkinleştirin: 

```
netsh advfirewall firewall add rule name="NPMDICMPV4Echo" protocol="icmpv4:8,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6Echo" protocol="icmpv6:128,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4DestinationUnreachable" protocol="icmpv4:3,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6DestinationUnreachable" protocol="icmpv6:1,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4TimeExceeded" protocol="icmpv4:11,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6TimeExceeded" protocol="icmpv6:3,any" dir=in action=allow 
```

### <a name="create-service-endpoint-monitor-tests"></a>Hizmet uç noktası İzleyicisi testleri oluşturma 

Hizmet uç noktaları için ağ bağlantısını izlemeniz testlerinizi oluşturmaya başlayın.

1. Seçin **hizmet uç noktası İzleyicisi** sekmesi.
2. Seçin **Test Ekle**, test ad ve açıklama girin. 
3. Test türü seçin:<br>

    * Seçin **Web** için outlook.office365.com veya aratıp gibi HTTP/S isteklerini yanıtlayan bir hizmet bağlantı izlemek için.<br>
    * Seçin **ağ** TCP isteklerine yanıt veriyor, ancak SQL server, FTP sunucusu veya SSH bağlantı noktası gibi HTTP/S isteklerine yanıt vermiyor bir hizmet bağlantı izlemek için. 
4. Ağ gecikmesi, paket kaybı ve Topoloji Bulma gibi ağ ölçümleri gerçekleştirmek istemiyorsanız temizleyin **ağ ölçümleri gerçekleştirmek** onay kutusu. En fazla özellikten elde etmek için Seçili Tut. 
5. İçinde **hedef**, ağ bağlantısı izlemek istediğiniz FQDN/URL/IP adresini girin.
6. İçinde **bağlantı noktası numarası**, hedef hizmet bağlantı noktası numarasını girin. 
7. İçinde **Test sıklığı**, ne sıklıkta çalıştırmak için test için istediğiniz bir değer girin. 
8. Hizmet ağ bağlantısını izlemek istediğiniz düğümleri seçin. 

    >[!NOTE]
    > Windows server tabanlı düğümleri için ağ ölçümleri gerçekleştirmek için TCP tabanlı istekler özelliği kullanır. Windows istemci tabanlı düğümleri için ağ ölçümleri gerçekleştirmek için ICMP tabanlı istekler özelliği kullanır. Düğümleri Windows istemci tabanlı olduğunda bazı durumlarda, hedef uygulama gelen ICMP tabanlı istekleri engeller. Çözüm ağ ölçümleri alamıyor. Windows server tabanlı düğümleri gibi durumlarda kullanmanızı öneririz. 

9. Öğelerin sistem durumu olayları oluşturmak istemiyorsanız, Temizle seçtiğiniz **hedefleri izlemesini etkinleştir sistem durumu, bu test tarafından kapsanan**. 
10. İzleme koşulları seçin. Eşik değerleri girerek sistem durumu olayı oluşturma için özel eşiklerini ayarlayabilirsiniz. Seçilen ağ veya alt ağ çifti için seçilen eşiğin üstünde koşul değerini gider olduğunda, bir sistem durumu olayı oluşturulur. 
11. Seçin **kaydetmek** yapılandırmayı kaydetmek için. 

    ![Hizmet uç noktası İzleyicisi test yapılandırmaları](media/log-analytics-network-performance-monitor/service-endpoint-configuration.png)



## <a name="walkthrough"></a>Kılavuz 

Ağ Performansı İzleyicisi Pano görünümüne gidin. Oluşturduğunuz farklı testleri sistem durumu özetini almak için bakmak **hizmet uç noktası İzleyicisi** sayfası. 

![Hizmet uç noktası izleme sayfası](media/log-analytics-network-performance-monitor/service-endpoint-blade.png)

Testleri ayrıntılarını görüntülemek için kutucuk seçin **testleri** sayfası. Sol taraftaki tabloda, zaman içinde nokta sistem durumunu ve hizmet yanıt süresi, ağ gecikme süresi ve paket kaybı tüm testler için değeri görüntüleyebilirsiniz. Geçmişte başka bir zamanda ağ anlık görüntüyü görüntülemek için ağ durumu Kaydedici denetimi kullanın. Test incelemek istediğiniz tabloda seçin. Sağ taraftaki bölmede grafiklerde kaybı, gecikme ve yanıt süresi değerleri geçmiş eğilim görüntüleyebilirsiniz. Seçin **Test ayrıntıları** her düğümden performansını görüntülemek için bağlantı.

![Hizmet uç noktası İzleyicisi sınamaları](media/log-analytics-network-performance-monitor/service-endpoint-tests.png)

İçinde **Test düğümleri** görünüm, ağ bağlantısı her düğümden gözlemlemek. Performans düşüşünü sahip düğümünü seçin. Bu uygulamayı yavaş çalışıyor olması gerektiğini burada gözlenir düğümdür.

Zayıf uygulama performans ağ veya uygulama sağlayıcının ucunda bir sorun nedeniyle uygulama yanıt süresini ve ağ gecikme süresi arasındaki bağıntıyı izlenerek olup olmadığını belirler. 

* **Uygulama sorununu:** yanıt süresi içinde bir ani ancak ağ gecikmesi tutarlılık ağın düzgün çalıştığından ve sorunun uygulama ucunda bir sorun nedeniyle olabilir önerir. 

    ![Hizmet uç noktası İzleyicisi uygulama sorunu](media/log-analytics-network-performance-monitor/service-endpoint-application-issue.png)

* **Ağ sorunu:** ağ gecikme süresi içinde karşılık gelen bir depo ile eşlik yanıt süresi içinde bir ani artış yanıt süresi, ağ gecikme süresi arasında bir artış nedeniyle olabilir önerir. 

    ![Hizmet uç noktası İzleyicisi ağ sorunu](media/log-analytics-network-performance-monitor/service-endpoint-network-issue.png)

Sorunu nedeniyle ağ olduğunu belirledikten sonra Seç **topoloji** topoloji Haritası sorunlu atlamada tanımlamak için görünümü bağlantısı. Aşağıdaki resimde bir örnek gösterilmektedir. 105 ms Toplam arasındaki gecikme süresi dışında düğümü ve uygulama uç noktasını, 96 ms olduğu kırmızı işaretli atlama nedeniyle. Sorunlu atlama tanımladıktan sonra düzeltme işlemleri gerçekleştirebilir. 

![Hizmet uç noktası İzleyicisi sınamaları](media/log-analytics-network-performance-monitor/service-endpoint-topology.png)

## <a name="diagnostics"></a>Tanılama 

Bir abnormality gözlemlerseniz, şu adımları izleyin:

* Hizmet yanıt süresi, Ağ kaybı ve gecikme NA gösteriliyorsa, en az şunlardan biri neden olmuş olabilir:

    - Uygulama çalışmıyor.
    - Hizmet için ağ bağlantısını denetlemek için kullanılan düğümü çalışmıyor.
    - Test yapılandırmasında girilen hedefi geçersiz.
    - Düğüm ağ bağlantısına sahip değil.

* Geçerli hizmet yanıt süresi gösterilir, ancak gecikme yanı sıra ağ kaybı NA gösterilir, en az şunlardan biri neden olabilir:

    - Hizmet ağ bağlantısını denetlemek için kullanılan düğümü Windows istemci makinesi ise, hedef hizmet ICMP isteklerini engelliyor veya bir ağ güvenlik duvarı düğümden kaynaklanan ICMP isteklerini engelliyor.
    - **Ağ ölçümleri gerçekleştirmek** test yapılandırmasında onay kutusu boştur. 

* Hizmet yanıt süresi NA ancak gecikme yanı sıra ağ kaybı geçerli, hedef hizmete bir web uygulaması olmayabilir. Test yapılandırmasını düzenleyin ve bir test türü olarak seçin **ağ** yerine **Web**. 

* Uygulamayı yavaş çalışıyorsa, zayıf uygulama performans ağ veya uygulama sağlayıcının ucunda bir sorun nedeniyle olup olmadığını belirler.


## <a name="next-steps"></a>Sonraki adımlar
[Arama günlüklerini](log-analytics-log-searches.md) ayrıntılı ağ performansı veri kayıtları görüntülemek için.
