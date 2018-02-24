---
title: "Ağ Azure günlük analizi Performans İzleyicisi çözümde | Microsoft Docs"
description: "Ağ Performans İzleyicisi'nde hizmet uç noktası Yöneticisi özelliği, ağ bağlantısı açık bir TCP bağlantı noktasına sahip herhangi bir uç nokta izlemenize olanak tanır."
services: log-analytics
documentationcenter: 
author: abshamsft
manager: carmonm
editor: 
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2018
ms.author: abshamsft
ms.openlocfilehash: 7eb31b91480b6e57135581cfa2f5503de3189e10
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2018
---
# <a name="service-endpoint-manager"></a>Hizmet uç noktası Yöneticisi

Hizmet uç noktası Yöneticisi özelliği [Ağ Performansı İzleyicisi](log-analytics-network-performance-monitor.md) açık bir TCP bağlantı noktasına sahip herhangi bir uç nokta ağ bağlantısı izlemenize olanak tanır. Web siteleri, SaaS uygulamaları, PaaS uygulamaları ve SQL veritabanları gibi uç noktaları içerir. 

İle aşağıdaki işlevleri gerçekleştirebilirsiniz **hizmet uç noktası İzleyicisi**: 

- Ağ bağlantısını (örneğin, Office 365, Dynamics CRM iç satır iş uygulamaları, SQL veritabanı, vb.) ağ hizmetleri ve uygulamaları için birden çok şube ofisleri/konumlardan izler. 
- Ağ bağlantısı Office365 ve Dynamics365 Uç noktalara izlemek için yerleşik testleri 
- Yanıt süresi, ağ gecikme, uç noktasına bağlanırken yaşadı paket kaybı belirleme 
- Zayıf uygulama performans nedeniyle ağ veya uygulama sağlayıcının ucunda bazı sorunu nedeniyle olup olmadığını belirleme 
- Her bir topoloji Haritası atlamada katkıda bulunan gecikme görüntüleyerek zayıf uygulama performans neden olabilecek ağ üzerinde etkin noktalar tanımlayın. 


![Hizmet uç noktası İzleyicisi](media/log-analytics-network-performance-monitor/service-endpoint-intro.png)


## <a name="configuration"></a>Yapılandırma 
Ağ Performansı İzleyicisi Yapılandırması'nı açmak için açın [Ağ Performansı İzleyicisi çözüm](log-analytics-network-performance-monitor.md) tıklatıp **yapılandırma** düğmesi.

![Ağ Performans İzleyicisi'ni yapılandırma](media/log-analytics-network-performance-monitor/npm-configure-button.png)


### <a name="configure-oms-agents-for-the-monitoring"></a>İzleme için OMS Aracısı yapılandırın.  
Böylece, düğümlerinden topoloji hizmet uç noktası çözüm bulabilir izlemek için kullanılan düğümlerde aşağıdaki güvenlik duvarı kurallarını etkinleştirin: 

```
netsh advfirewall firewall add rule name="NPMDICMPV4Echo" protocol="icmpv4:8,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6Echo" protocol="icmpv6:128,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4DestinationUnreachable" protocol="icmpv4:3,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6DestinationUnreachable" protocol="icmpv6:1,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4TimeExceeded" protocol="icmpv4:11,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6TimeExceeded" protocol="icmpv6:3,any" dir=in action 
```

### <a name="create-service-endpoint-monitor-tests"></a>Hizmet uç noktası İzleyicisi testleri oluşturma 

Hizmet uç noktaları için ağ bağlantısını izlemeniz testlerinizi oluşturmaya başla 

1. Tıklayın **hizmet uç noktası İzleyicisi** sekmesi.
2. Tıklatın **Test Ekle** ve Test ad ve açıklama girin. 
3. Test türü seçin:<br>Seçin **Web testi** aratıp outlook.office365.com gibi HTTP/S isteklerini yanıtlayan bir hizmet bağlantı izliyorsanız.<br>Seçin **ağ sınaması** TCP isteğine yanıt veriyor, ancak bir SQL server gibi HTTP/S isteğine yanıt olmayan bir hizmet bağlantı izliyorsanız FTP sunucusu, SSH bağlantı noktası vb. 
4. Ağ ölçümleri (ağ gecikmesi, paket kaybı, Topoloji Bulma) gerçekleştirmek istemiyorsanız, metin kutusunun işaretini kaldırın. Bu özellikten en fazla elde etmek için kullanıma tutmak öneririz. 
5. Ağ bağlantısı izlemek istediğiniz hedef URL/FQDN/IP adresini girin.  
6. Hedef hizmet bağlantı noktası numarasını girin. 
7. Test çalıştırmak istediğiniz sıklığı girin. 
8. Hizmet ağ bağlantısını izlemek istediğiniz düğümleri seçin. 

    >[!NOTE]
    > Windows server tabanlı düğümleri için ağ ölçümleri gerçekleştirmek için TCP tabanlı istekler özelliği kullanır. Windows istemci tabanlı düğümleri için ağ ölçümleri gerçekleştirmek için ICMP tabanlı istekler özelliği kullanır. Bazı durumlarda, hedef uygulama gelen ICMP tabanlı istek düğümleri Windows istemci tabanlı olduğunda nedeniyle çözüm ağ ölçümleri gerçekleştiremiyor olduğu engeller. Bu nedenle, bu gibi durumlarda Windows server tabanlı düğümleri kullanmanız önerilir. 

9. Seçtiğiniz, ardından öğeleri için sistem durumu olayları Temizle oluşturmak istemiyorsanız, **bu test tarafından kapsanan hedeflerde sistem durumu izlemeyi etkinleştir**. 
10. İzleme koşulları seçin. Eşik değerleri yazarak, sistem olay oluşturma için özel eşikler ayarlayabilirsiniz. Seçilen ağ/alt ağ çifti için seçilen eşiğin üstünde koşul değerini gider olduğunda, bir sistem durumu olayı oluşturulur. 
11. tıklatın **kaydetmek** yapılandırmayı kaydetmek için. 

 ![Hizmet uç noktası izleme yapılandırması](media/log-analytics-network-performance-monitor/service-endpoint-configuration.png)



## <a name="walkthrough"></a>Kılavuz 

Taşıma ağ performansı izleme Pano görünümüne ve uyun **hizmet uç noktası İzleyicisi** oluşturduğunuz farklı testleri sistem durumu özetini almak için sayfa.  

![Hizmet uç noktası izleme sayfası](media/log-analytics-network-performance-monitor/service-endpoint-blade.png)

Ayrıntıya için olan kutucuğuna tıklayın ve testleri ayrıntılarını görüntüleyin **testleri** sayfası. LHS tabloda, zaman içinde nokta sistem durumunu ve hizmet yanıt süresi, ağ gecikme süresi ve paket kaybı tüm testler için değeri görüntüleyebilirsiniz. Başka bir ağ anlık görüntü görüntülemek için ağ durumu Kaydedici denetim kullanabilirsiniz zaman geçti. İncelemek istediğiniz tablonun testinde tıklayın. Geçmiş eğilim kaybı, gecikme ve grafikler yanıt süresi değerleri RHS Bölmesi'nde görüntüleyebilirsiniz. Her düğümden performansını görüntülemek için Test Ayrıntıları bağlantıyı tıklatın. 

![Hizmet uç noktası izleme testleri](media/log-analytics-network-performance-monitor/service-endpoint-tests.png)

Üzerinde **Test düğümleri** görünüm, ağ bağlantısı her düğümden gözlemlemek. Performans düşüşünü sahip düğümüne tıklayın.  Bu uygulamayı yavaş çalışıyor olması gerektiğini burada gözlenir gelen düğümdür. 

Zayıf uygulama performans nedeniyle ağ veya uygulama sağlayıcının ucunda bazı sorunu nedeniyle uygulama yanıt süresini ve ağ gecikme süresi arasındaki bağıntıyı izlenerek olup olmadığını belirleme 

**Uygulama sorununu:** yanıt süresi, bir depo olan ancak ağ gecikmesi tutarlı olması durumunda bu ağ düzgün çalıştığını ve uygulama sonunda bir sorun nedeniyle sorunu olduğunu önerir.  

![Hizmet uç noktası İzleyicisi uygulama sorunu](media/log-analytics-network-performance-monitor/service-endpoint-application-issue.png)

**Ağ sorunu:** bir ani yanıt süresi, ağ gecikme karşılık gelen bir depo ile eşlik sonra bu artış yanıt süresi, ağ gecikme süresi arasında bir artış nedeniyle olduğunu önerir.  

![Hizmet uç noktası İzleyicisi ağ sorunu](media/log-analytics-network-performance-monitor/service-endpoint-network-issue.png)

Sorunu nedeniyle ağ olduğunu saptadıktan sonra tıklatabilirsiniz **topoloji** topoloji Haritası sorunlu atlamada tanımlamak için görünümü bağlantısı. Örneğin, aşağıdaki görüntüde 105 ms toplam gecikme dışında düğümü ile uygulama uç noktası arasındaki 96 ms kırmızı işaretli atlama nedeniyle görebilirsiniz. Sorunlu atlama tanımladıktan sonra düzeltme işlemleri gerçekleştirebilir.  

![Hizmet uç noktası izleme testleri](media/log-analytics-network-performance-monitor/service-endpoint-topology.png)

## <a name="diagnostics"></a>Tanılama 

Bir abnormality gözlemlerseniz, şu adımları izleyin:

Bir veya daha fazla şunlardan biri nedeniyle olabilir sonra hizmet yanıt süresi, Ağ kaybı ve gecikme NA gösteriliyorsa:
- Uygulama çalışmıyor.
- Hizmet için ağ bağlantısını denetlemek için kullanılan düğümü çalışmıyor.
- Test yapılandırmasında girilen hedefi geçersiz.
- Düğümün bir ağ bağlantısı yok.

Geçerli hizmet yanıt süresi gösterilir, ancak gecikme yanı sıra ağ kaybı NA gösterilir, sonra da bir veya daha fazla şunlardan biri nedeniyle olabilir:
- Hizmet ağ bağlantısını denetlemek için kullanılan düğümü Windows istemci makinesi ise, hedef hizmet ICMP isteklerini engelliyor veya bir ağ güvenlik duvarı düğümden kaynaklanan ICMP isteklerini engelliyor.
- Onay kutusunu **ağ ölçümleri gerçekleştirmek** test yapılandırmasında işareti kaldırıldı. 

Ardından bu ciddi bir şekilde hizmet yanıt süresi NA ancak gecikme yanı sıra ağ kaybı geçerli varsa, hedef hizmete bir web uygulaması değil önerir. Test yapılandırmasını düzenleyin ve test türü seçin ağ sınaması Web testi yerine olarak. 

Uygulamayı yavaş çalışıyorsa, zayıf uygulama performans nedeniyle ağ veya uygulama sağlayıcının ucunda bazı sorunu nedeniyle olup olmadığını belirlemeniz gerekir.


## <a name="next-steps"></a>Sonraki adımlar
* [Arama günlüklerini](log-analytics-log-searches.md) ayrıntılı ağ performansı veri kayıtları görüntülemek için.
