---
title: Ağ Performansı İzleyicisi çözümü Azure Log analytics'te | Microsoft Docs
description: Hizmet Bağlantı İzleyicisi özelliği ağ Performans İzleyicisi'nde açık bir TCP bağlantı noktasına sahip herhangi bir uç noktası için ağ bağlantılarını izlemek için kullanın.
services: log-analytics
documentationcenter: ''
author: abshamsft
manager: carmonm
editor: ''
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/20/2018
ms.author: absha
ms.openlocfilehash: c5285ac95a2f5813949f22aae3849fd7f55b1ada
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67052099"
---
# <a name="service-connectivity-monitor"></a>Hizmet Bağlantısı İzleyicisi

Hizmet Bağlantı İzleyicisi özelliği kullanabileceğiniz [Ağ Performansı İzleyicisi](network-performance-monitor.md) açık bir TCP bağlantı noktasına sahip herhangi bir uç noktası için ağ bağlanabilirliğini izlemek için. Bu uç noktaları, Web siteleri, SaaS uygulamaları, PaaS uygulamalarının ve SQL veritabanlarını içerir. 

Hizmet Bağlantı İzleyicisi ile aşağıdaki işlevleri gerçekleştirebilirsiniz: 

- Uygulamalar ve Ağ Hizmetleri Ağ bağlantısını birden çok şube ofisleri veya konumları izleyin. Office 365, Dynamics CRM, iç iş kolu satır uygulama ve SQL veritabanları, uygulamalar ve ağ hizmetlerini içerir.
- Office 365 ve Dynamics 365 uç noktalarına ağ bağlanabilirliğini izlemek için yerleşik testleri kullanın. 
- Yanıt süresi, ağ gecikme süresi ve paket kaybı deneyimli uç noktasına bağlanırken belirleyin.
- Kötü uygulama performansı nedeniyle ağ veya uygulama sağlayıcısının son bazı sorunu nedeniyle olup olmadığını belirler.
- Ayırma-birleştirme ağ üzerinde yer alan her bir topoloji Haritası durakta tarafından katkıda bulunulan gecikme görüntüleyerek kötü uygulama performansı neden olabilecek belirleyin.


![Hizmet Bağlantısı İzleyicisi](media/network-performance-monitor-service-endpoint/service-endpoint-intro.png)


## <a name="configuration"></a>Yapılandırma 
Yapılandırma için Ağ Performansı İzleyicisi'ni açmak için açık [Ağ Performansı İzleyicisi çözüm](network-performance-monitor.md) seçip **yapılandırma**.

![Ağ Performansı İzleyicisi'ni yapılandırma](media/network-performance-monitor-service-endpoint/npm-configure-button.png)


### <a name="configure-log-analytics-agents-for-monitoring"></a>İzleme için log Analytics aracılarını yapılandırma
Çözüm düğümlerinizi topolojisinden hizmet uç noktası keşfedebilmesi için izlemek için kullanılan düğümlerde aşağıdaki güvenlik duvarı kuralları etkinleştirin: 

```
netsh advfirewall firewall add rule name="NPMDICMPV4Echo" protocol="icmpv4:8,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6Echo" protocol="icmpv6:128,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4DestinationUnreachable" protocol="icmpv4:3,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6DestinationUnreachable" protocol="icmpv6:1,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4TimeExceeded" protocol="icmpv4:11,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6TimeExceeded" protocol="icmpv6:3,any" dir=in action=allow 
```

### <a name="create-service-connectivity-monitor-tests"></a>Hizmet Bağlantı İzleyicisi sınamaları oluşturun 

Hizmet uç noktalarına ağ bağlanabilirliğini izlemek için testleri oluşturmaya başlayın.

1. Seçin **Hizmet Bağlantı İzleyicisi** sekmesi.
2. Seçin **Test Ekle**, test adı ve açıklama girin. Çalışma alanı başına en fazla 450 testleri oluşturabilirsiniz. 
3. Testi türünü seçin:<br>

    * Seçin **Web** outlook.office365.com veya bing.com gibi HTTP/S istekleri, yanıt veren bir hizmet bağlantısı izlemek için.<br>
    * Seçin **ağ** TCP isteklerine yanıt verir, ancak SQL server, FTP sunucusu veya SSH bağlantı noktası gibi HTTP/S isteklerine yanıt vermiyor hizmetine bağlantıyı izlemek için. 
    * Örneğin: Bir web testine bir blob depolama hesabı oluşturmak için Seç **Web** ve hedef olarak girin *yourstorageaccount*. blob.core.windows.net. Benzer şekilde, diğer tablo depolama, kuyruk depolama ve Azure dosyaları'nı kullanarak testleri oluşturabilirsiniz [Bu bağlantı.](https://docs.microsoft.com/azure/storage/common/storage-account-overview#storage-account-endpoints)
4. Topoloji Bulma, ağ gecikme süresi ve paket kaybı gibi ağ ölçümlerini gerçekleştirmek istemiyorsanız Temizle **ağ ölçümlerini gerçekleştirmek** onay kutusu. En fazla özellikten faydalanmak için seçili devam eder. 
5. İçinde **hedef**, ağ bağlantılarını izlemek istediğiniz URL/FQDN/IP adresini girin.
6. İçinde **bağlantı noktası numarası**, hedef hizmet bağlantı noktası numarasını girin. 
7. İçinde **Test sıklığı**, testin çalışmasını istediğiniz için ne sıklıkla bir değer girin. 
8. Hizmet için ağ bağlantılarını izlemek istediğiniz düğümleri seçin. Test başına eklediğiniz aracı sayısı 150'den az olduğundan emin olun. Herhangi bir aracı en fazla 150 uç noktalar/aracıları test edebilirsiniz.

    >[!NOTE]
    > Windows server tabanlı düğümler için özelliği istekler TCP tabanlı ağ ölçümlerini gerçekleştirmek için kullanır. Windows istemcisi tabanlı düğümler için ağ ölçümlerini gerçekleştirmek için ICMP tabanlı istekleri özelliği kullanır. Windows istemcisi tabanlı düğümler olduğunda bazı durumlarda, hedef uygulama gelen ICMP tabanlı istekleri engeller. Ağ ölçümlerini gerçekleştirmek çözüm silemiyor. Böyle durumlarda, Windows server tabanlı düğümleri kullanmanızı öneririz. 

9. Öğeler için sistem durumu olayları oluşturmak istemiyorsanız, Temizle seçtiğiniz **hedefler sistem durumu izlemeyi etkinleştir, bu test tarafından kapsanan**. 
10. İzleme koşulları seçin. İçin sistem durumu olayı oluşturma eşiği değerler girerek, özel eşikler ayarlayabilirsiniz. Seçilen ağ veya alt ağ çifti için seçilen eşiğin üzerinde koşul değeri gider her sistem durumu olayı oluşturulur. 
11. Seçin **Kaydet** yapılandırmayı kaydetmek için. 

    ![Hizmet Bağlantı İzleyicisi test yapılandırmaları](media/network-performance-monitor-service-endpoint/service-endpoint-configuration.png)



## <a name="walkthrough"></a>Kılavuz 

Ağ Performansı İzleyicisi Pano görünümüne gidin. Sistem, oluşturduğunuz farklı testlerin bir özetini almak için bakmak **Hizmet Bağlantı İzleyicisi** sayfası. 

![Hizmet bağlantı İzlenecekler sayfası](media/network-performance-monitor-service-endpoint/service-endpoint-blade.png)

Testin ayrıntılarını görüntülemek için kutucuğu seçin **testleri** sayfası. Sol taraftaki tabloda, zaman içinde nokta sistem durumu ve hizmet yanıt süresi, ağ gecikme süresi ve paket kaybı tüm testlerin değerini görüntüleyebilirsiniz. Ağ durumu Kaydedici denetimi ağ anlık görüntü daha önce başka bir zaman görüntülemek için kullanın. Test içinde araştırmak istediğiniz tabloyu seçin. Sağ taraftaki bölmede grafiklerde geçmiş eğilimini ve hataların kaybı, gecikme süresi ve yanıt süresi değerleri görüntüleyebilirsiniz. Seçin **Test ayrıntıları** her düğümden performansını görüntülemek için bağlantı.

![Hizmet Bağlantı İzleyicisi sınamaları](media/network-performance-monitor-service-endpoint/service-endpoint-tests.png)

İçinde **Test düğümleri** görünüm, ağ bağlantısı her düğümden gözlemleyin. Performans düşüşü olan düğümü seçin. Uygulama yavaş çalışıyor olması için burada gözlemlenen düğüm budur.

Kötü uygulama performansı ağ veya uygulama sağlayıcısının son bir sorun nedeniyle uygulama yanıt süresini ve ağ gecikme süresi arasındaki bağıntıyı gözlemleyerek olup olmadığını belirler. 

* **Uygulama sorunu:** Yanıt süresi bir ani değişiklik ancak ağ gecikme süresi, tutarlılık ağın düzgün çalıştığından ve sorun uygulama ucundaki ilgili bir sorun nedeniyle olabilir önerir. 

    ![Hizmet Bağlantı İzleyicisi uygulama sorunu](media/network-performance-monitor-service-endpoint/service-endpoint-application-issue.png)

* **Ağ sorun:** Yanıt süresinde ağ gecikme süresine karşılık gelen bir depo ile birlikte bir ani artış yanıt süresi, ağ gecikme süresi arasında bir artış nedeniyle olabilir önerir. 

    ![Hizmet Bağlantı İzleyicisi ağ sorunu](media/network-performance-monitor-service-endpoint/service-endpoint-network-issue.png)

Sorunu nedeniyle ağ olduğunu belirledikten sonra seçin **topolojisi** topoloji haritasında sorunlu atlama tanımlamak için Görünüm bağlantısı. Aşağıdaki görüntüde bir örnek gösterilir. 105-ms toplam gecikme dışında düğümü ile uygulama uç noktası arasındaki 96 ms kırmızı işaretli atlama nedeniyle olur. Sorunlu atlama tanımladıktan sonra düzeltici eylemi gerçekleştirebilir. 

![Hizmet Bağlantı İzleyicisi sınamaları](media/network-performance-monitor-service-endpoint/service-endpoint-topology.png)

## <a name="diagnostics"></a>Tanılama 

Bir abnormality gözlemlerseniz, şu adımları izleyin:

* Hizmet yanıt süresi, Ağ kaybı ve gecikme süresi kullanılamaz olarak gösteriliyorsa, en az aşağıdakilerden biri neden olmuş olabilir:

    - Uygulama çalışmıyor.
    - Hizmet için ağ bağlantısını denetlemek için kullanılan düğümü çalışmıyor.
    - Testi Yapılandırması'nda girilen hedef doğru değil.
    - Düğüm, herhangi bir ağ bağlantısı yok.

* Geçerli hizmet yanıt süresi gösterilir, ancak gecikme süresi yanı sıra ağ kaybı kullanılamaz olarak gösterilen, aşağıdaki nedenlerden biri veya nedeni olabilir:

    - Bir Windows istemci makine hizmete ağ bağlantısını denetlemek için kullanılan düğümün ise hedef hizmet ICMP istekleri engellediğinde veya bir ağ güvenlik duvarı düğümden kaynaklanan ICMP isteği engelliyor.
    - **Ağ ölçümlerini gerçekleştirmek** testi Yapılandırması'nda onay kutusu boştur. 

* DI hizmet yanıt süresi, ancak gecikme süresi yanı sıra ağ kaybı geçerli, hedef hizmeti bir web uygulaması olmayabilir. Test yapılandırmasını düzenleyin ve test türü olarak seçin **ağ** yerine **Web**. 

* Uygulamayı yavaş çalışıyorsa, kötü uygulama performansı ağ veya uygulama sağlayıcısının son bir sorun nedeniyle olup olmadığını belirler.

## <a name="gcc-office-urls-for-us-government-customers"></a>ABD kamu müşterilerine yönelik GCC Office URL'leri
ABD Devleti Virginia bölge için yalnızca DOD URL'leri, yerleşik NPM noktalardır. GCC URL'leri kullanan müşteriler, özel testleri oluşturun ve her URL ayrı ayrı eklemeniz gerekir.

| Alan | GCC |
|:---   |:--- |
| Office 365 portalı ve paylaşılan | Portal.Apps.mil |
| Office 365 kimlik doğrulaması ve kimlik | * login.microsoftonline.us <br> * api.login.microsoftonline.com <br> * clientconfig.microsoftonline-p.net <br> * login.microsoftonline.com <br> * login.microsoftonline p.com <br> * login.windows.net <br> * loginex.microsoftonline.com <br> * oturum açma-us.microsoftonline.com <br> * nexus.microsoftonline-p.com <br> * mscrl.microsoft.com <br> * secure.aadcdn.microsoftonline-p.com |
| Office Online | * adminwebservice.gov.us.microsoftonline.com <br>  * adminwebservice-s1-bn1a.microsoftonline.com <br> * adminwebservice-s1-dm2a.microsoftonline.com <br> * becws.gov.us.microsoftonline.com <br> * provisioningapi.gov.us.microsoftonline.com <br> * officehome.msocdn.us <br> * prod.msocdn.us <br> * portal.office365.us <br> * webshell.suite.office365.us <br> * www .office365.us <br> * activation.sls.microsoft.com <br> * crl.microsoft.com <br> * go.microsoft.com <br> * insertmedia.bing.office.net <br> * ocsa.officeapps.live.com <br> * ocsredir.officeapps.live.com <br> * ocws.officeapps.live.com <br> * office15client.microsoft.com <br>* officecdn.microsoft.com <br> * officecdn.microsoft.com.edgesuite.net <br> * officepreviewredir.microsoft.com <br> * officeredir.microsoft.com <br> * ols.officeapps.live.com  <br> * r.office.microsoft.com <br> * cdn.odc.officeapps.live.com <br> * odc.officeapps.live.com <br> * officeclient.microsoft.com |
| Exchange Online | * outlook.office365.us <br> * attachments.office365-net.us <br> * autodiscover-s.office365.us <br> * manage.office365.us <br> * scc.office365.us |
| MS Teams | gov.Teams.microsoft.us | 

## <a name="next-steps"></a>Sonraki adımlar
[Arama günlüklerini](../../azure-monitor/log-query/log-query-overview.md) ayrıntılı ağ performansı verileri kayıtları görüntülemek için.
