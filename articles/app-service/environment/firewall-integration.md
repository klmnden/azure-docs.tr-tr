---
title: App Service ortamı giden trafiği - Azure kilitleme
description: Azure giden trafiğin güvenliğini sağlamak için güvenlik duvarı ile tümleştirmeyi açıklamaktadır
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 955a4d84-94ca-418d-aa79-b57a5eb8cb85
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2019
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 6dae2d40650b9fdb8df2d3bdb74b2df78639dc11
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67058054"
---
# <a name="locking-down-an-app-service-environment"></a>App Service ortamı kilitleme

App Service ortamı (ASE) birkaç şekilde çalışması için erişim gerektiren bir Dış bağımlılıklara sahiptir. ASE, müşterinin Azure sanal ağı (VNet) bulunur. Müşteriler, kendi vnet'ten tüm çıkış kilitlemek istediğiniz müşteriler için bir sorun olduğunu ASE bağımlılık trafiğine izin vermelidir.

Bir ASE sahip gelen bağımlılıklar vardır. Gelen yönetim trafiğinin bir güvenlik duvarı cihazı üzerinden gönderilemez. Bu trafiğe ait kaynak adresleri bilinen ve yayınlanır [App Service ortamı yönetim adresleri](https://docs.microsoft.com/azure/app-service/environment/management-addresses) belge. Gelen trafiği güvenli hale getirmek için bu bilgileri ile ağ güvenlik grubu kuralları oluşturabilirsiniz.

ASE giden bağımlılık neredeyse tamamen statik adresleri arkasına olmayan FQDN ile tanımlanır. Statik adresler olmaması anlamına gelir ağ güvenlik grupları (Nsg'ler) ASE giden trafiği kilitlemek için kullanılamaz. Adresleri sıklıkta biri olamaz geçerli çözünürlüğüne göre kurallarını ayarlama ve Nsg'ler oluşturma kullanan, değiştirin. 

Etki alanı adlarını temel alarak giden trafiği denetleyen bir güvenlik duvarı cihazın kullanımda giden adresleri güvenliğini sağlamak için çözüm arasındadır. Azure güvenlik duvarı hedef FQDN'sini üzerinde giden HTTP ve HTTPS trafiğini kısıtlayabilirsiniz.  

## <a name="system-architecture"></a>Sistem Mimarisi

Bir güvenlik duvarı cihazı üzerinden giden giden trafik ile ASE dağıtma, ASE alt yollara değiştirilmesi gerekir. Yollar bir IP düzeyinde çalışır. Yollarınızı belirlerken dikkatli emin değilseniz, başka bir adresten kaynak TCP yanıt trafiğini zorlayabilirsiniz. Asimetrik yönlendirme adı verilir ve TCP çalışmamasına neden olur.

ASE gelen trafiği geri gelen trafik aynı şekilde yanıt verebilir böylece tanımlı yönlendirmeler olması gerekir. Bu gelen yönetim istekleri için geçerlidir ve gelen uygulama istekleri için geçerlidir.

Bir ASE gelen ve giden trafiği tarafından aşağıdaki kurallara uymanız gerekir

* Azure SQL, depolama ve olay hub'ı trafiği bir güvenlik duvarı cihaz kullanımıyla desteklenmez. Bu trafik, doğrudan bu hizmetlere gönderilmelidir. Gerçekleşen yapmak için bu üç Hizmetleri için hizmet uç noktası yapılandırmak için yoludur. 
* Rota tablosu kuralları gelen yönetim trafiğinin geri nereden geldiğini gönderen tanımlanmalıdır.
* Rota tablosu kuralları geri nereden geldiğini gelen uygulama trafiğini gönderdiği tanımlanmalıdır. 
* ASE bırakarak tüm trafiği, güvenlik duvarı cihazınıza bir rota tablosu kuralla gönderilebilir.

![ASE ile Azure güvenlik duvarı bağlantı akışı][5]

## <a name="configuring-azure-firewall-with-your-ase"></a>Azure güvenlik duvarı ile ASE'nizi yapılandırma 

Azure güvenlik duvarı ile mevcut gidenler çıkış kilitlemek için adımlar şunlardır:

1. ASE alt ağınız üzerindeki hizmet uç noktalarını SQL, depolama ve olay hub'ı etkinleştirin. Bunu yapmak için ağ portalına gidin > alt ağlar ve select Microsoft.EventHub, Microsoft.SQL ve hizmet uç noktaları açılır listeden Microsoft.Storage. Hizmet uç noktaları Azure SQL'e etkin olduğunda, uygulamalarınızı sahip herhangi bir Azure SQL bağımlılığın de hizmet uç noktaları ile yapılandırılması gerekir. 

   ![Hizmet uç noktaları seçin][2]
  
1. Vnet'e ASE'nizi bulunduğu AzureFirewallSubnet adlı bir alt ağ oluşturun. Bölümündeki yönergeleri izleyin [Azure güvenlik duvarınızın belgelerine](https://docs.microsoft.com/azure/firewall/) , Azure güvenlik duvarı oluşturun.
1. Azure güvenlik duvarı arabiriminden > kuralları > uygulama kural koleksiyonu, select Ekle uygulama kuralı koleksiyonu. Öncelik, bir ad sağlayın ve izin ayarlanmalıdır. FQDN etiketler bölümünde, bir ad girin, kaynak adresleri kümesine * App Service ortamı FQDN etiketi ve Windows Update seçin. 
   
   ![Uygulama Kuralı Ekle][1]
   
1. Azure güvenlik duvarı arabiriminden > kuralları > Ağ kural koleksiyonu, Ekle ağ kuralı koleksiyonu seçin. Öncelikli bir ad sağlayın ve izin ayarlanmalıdır. Kuralları bölümünde bir ad belirtin, seçin **herhangi**ayarlayın * adreslere, kaynak ve hedef ve bağlantı noktası 123 için ayarlayın. Bu kural, sistem saati eşitleme NTP kullanarak gerçekleştirmek sağlar. Başka bir kural, sistem sorunları önceliklendirme yardımcı olmak için aynı şekilde 12000 numaralı bağlantı noktasına oluşturun.

   ![NTP ağ kuralı ekleyin][3]

1. Yönetim adresleri ile yönlendirme tablosu oluşturma [App Service ortamı yönetim adresleri]( https://docs.microsoft.com/azure/app-service/environment/management-addresses) bir sonraki atlama internet ile. Asimetrik yönlendirme sorunlarını önlemek için rota tablosu girdileri gerekir. Bir sonraki atlama internet IP adresi bağımlılıklarla aşağıda belirtilen IP adresi bağımlılıklar için yollar ekleyin. Bir sanal gereç yol yol tablonuz 0.0.0.0/0 sonraki atlama, Azure güvenlik duvarı özel IP adresi olması ile ekleyin. 

   ![Bir yol tablosu oluşturma][4]
   
1. ASE alt ağınız için oluşturduğunuz yol tablosu atayın.

#### <a name="deploying-your-ase-behind-a-firewall"></a>ASE'nizi bir güvenlik duvarının arkasındaki dağıtma

ASE'nizi bir güvenlik duvarının arkasındaki dağıtmak için bir Azure güvenlik duvarı dışında mevcut ASE'nizi yapılandırma aynı ASE alt ağınız oluşturun ve ardından önceki adımları izleyerek gerekecektir adımlardır. Önceden var olan bir alt ağda ASE'nizi oluşturmak için Resource Manager şablonu belgesinde açıklandığı kullanmanız gerekir [Resource Manager şablonu ile ASE'nizi oluşturma](https://docs.microsoft.com/azure/app-service/environment/create-from-template).

## <a name="application-traffic"></a>Uygulama trafiği 

Yukarıdaki adımları ASE'nizi sorunsuz çalışmasına izin verir. Yine de şeyler uygulama ihtiyaçlarınıza uyum sağlayacak şekilde yapılandırmanız gerekir. Azure güvenlik duvarı ile yapılandırılmış bir ase'deki uygulamalar için iki sorunu vardır.  

- Uygulama bağımlılıklarını, Azure güvenlik duvarı ya da rota tablosunu eklenmelidir. 
- Asimetrik yönlendirme sorunlarını önlemek uygulama trafiği için rotalar oluşturulmalıdır

Uygulamalarınızı bağımlılıkları varsa, bunların Azure güvenlik duvarını eklenmesi gerekir. HTTP/HTTPS trafiğine izin vermek ve diğer her şey için kuralları ağ uygulama kuralları oluşturun. 

Uygulama isteği trafiğiniz kaynağından gelir adres aralığını biliyorsanız, yol tablosuna, ASE alt ağınız için atanmış ekleyebilirsiniz. Adres aralığı büyük ya da belirtilmemiş olması durumunda, yol tablosuna eklemek için bir adres sağlamak için Application Gateway gibi ağ Gereci kullanabilirsiniz. ILB ASE'nizi bir Application Gateway yapılandırma hakkında ayrıntılı bilgi edinmek için [ILB ASE'nizi bir Application Gateway ile tümleştirme](https://docs.microsoft.com/azure/app-service/environment/integrate-with-application-gateway)

Bu uygulama ağ geçidi kullanımı, sisteminizi yapılandırmak nasıl yalnızca bir örnektir. Bu yolu izlerseniz uygulama ağ geçidine gönderilen yanıt trafiğini doğrudan var. çıkacak şekilde ASE alt ağın yol tablosuna bir yol eklemek gerekir. 

## <a name="logging"></a>Günlüğe kaydetme 

Azure güvenlik duvarı günlükleri Olay hub'ı, Azure Depolama'ya gönderebilir veya Azure İzleyici günlüğe kaydeder. Uygulamanızın desteklenen herhangi bir hedefe ile tümleştirmek için Azure güvenlik duvarı portala gidin > tanılama günlükleri ve istenen hedefiniz için günlükleri etkinleştirin. Azure İzleyici günlüklerine ile tümleştirirseniz, Azure Güvenlik Duvarı'na gönderilen tüm trafik için günlüğü görebilirsiniz. Reddediliyor trafiği görmek için Log Analytics çalışma alanı portalınızı açın > günlükleri gibi bir sorgu girin 

    AzureDiagnostics | where msg_s contains "Deny" | where TimeGenerated >= ago(1h)
 
Azure İzleyici günlüklerine ile Azure güvenlik duvarınızı tümleştirme önce tüm uygulama bağımlılıklarını, uyumlu olmadığında bir uygulama çalışma başlama çok yararlı olur. Azure İzleyici günlükleri hakkında daha fazla bilgi [Azure İzleyici'de günlük verileri](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview)
 
## <a name="dependencies"></a>Bağımlılıkları

Aşağıdaki bilgiler yalnızca olan Azure güvenlik duvarı dışında bir güvenlik duvarı gerecini yapılandırmak isteyip istemediğinizi gerekli. 

- Hizmet uç noktası uyumlu Hizmetleri hizmet uç noktaları ile yapılandırılması gerekir.
- IP adresi bağımlılıklarıdır HTTP/S olmayan trafik için (TCP ve UDP trafiği)
- FQDN HTTP/HTTPS uç noktaları güvenlik duvarı Cihazınızı yerleştirilebilir.
- Joker karakter HTTP/HTTPS uç noktaları ile ASE'nizi niteleyicileri sayısına göre değişebilir bağımlılıklardır. 
- ASE'niz Linux uygulamaları dağıtıyorsanız Linux bağımlılıkları yalnızca bir sorun var. Linux uygulamaları ASE'nizi değil dağıtıyorsanız, ardından bu adresleri güvenlik duvarını eklenmesi gerekmez. 

#### <a name="service-endpoint-capable-dependencies"></a>Hizmet uç noktası özellikli bağımlılıkları 

| Uç Nokta |
|----------|
| Azure SQL |
| Azure Storage |
| Azure Olay Hub'ı |

#### <a name="ip-address-dependencies"></a>IP adresi bağımlılıkları

| Uç Nokta | Ayrıntılar |
|----------| ----- |
| \*:123 | NTP saat denetimi. Trafiği birden fazla uç nokta bağlantı noktası 123 iade edildiğinde |
| \*:12000 | Bu bağlantı noktası, bazı sistem izleme için kullanılır. Bazı sorunlar için değerlendirme daha zor olacaktır ancak ASE'nizi çalışmaya devam edecek engellenirse |
| 40.77.24.27:80 | İzleme ve uyarılar ASE sorunları için gerekli |
| 40.77.24.27:443 | İzleme ve uyarılar ASE sorunları için gerekli |
| 13.90.249.229:80 | İzleme ve uyarılar ASE sorunları için gerekli |
| 13.90.249.229:443 | İzleme ve uyarılar ASE sorunları için gerekli |
| 104.45.230.69:80 | İzleme ve uyarılar ASE sorunları için gerekli |
| 104.45.230.69:443 | İzleme ve uyarılar ASE sorunları için gerekli |
| 13.82.184.151:80 | İzleme ve uyarılar ASE sorunları için gerekli |
| 13.82.184.151:443 | İzleme ve uyarılar ASE sorunları için gerekli |

Azure güvenlik duvarı ile otomatik olarak her şeyi aşağıdaki FQDN etiketlerle sahip olursunuz. 

#### <a name="fqdn-httphttps-dependencies"></a>FQDN HTTP/HTTPS bağımlılıkları 

| Uç Nokta |
|----------|
|graph.windows.net:443 |
|login.live.com:443 |
|Login.Windows.com:443 |
|Login.Windows.NET:443 |
|login.microsoftonline.com:443 |
|client.wns.windows.com:443 |
|definitionupdates.microsoft.com:443 |
|go.microsoft.com:80 |
|go.microsoft.com:443 |
|www.microsoft.com:80 |
|www.microsoft.com:443 |
|wdcpalt.microsoft.com:443 |
|wdcp.microsoft.com:443 |
|ocsp.msocsp.com:443 |
|mscrl.microsoft.com:443 |
|mscrl.microsoft.com:80 |
|crl.microsoft.com:443 |
|CRL.microsoft.com:80 |
|www.Thawte.com:443 |
|crl3.digicert.com:80 |
|OCSP.digicert.com:80 |
|csc3-2009-2.crl.verisign.com:80 |
|CRL.VeriSign.com:80 |
|ocsp.verisign.com:80 |
|cacerts.digicert.com:80 |
|azperfcounters1.BLOB.Core.Windows .net: 443 |
|azurewatsonanalysis prod.core.windows.net:443 |
|Global.Metrics.nsatc.NET:80 |
|Global.Metrics.nsatc.NET:443 |
|az prod.metrics.nsatc.net:443 |
|antares.Metrics.nsatc.NET:443 |
|azglobal black.azglobal.metrics.nsatc.net:443 |
|azglobal red.azglobal.metrics.nsatc.net:443 |
|antares black.antares.metrics.nsatc.net:443 |
|antares red.antares.metrics.nsatc.net:443 |
|maupdateaccount.blob.core.windows.net:443 |
|clientconfig.passport.net:443 |
|Packages.microsoft.com:443 |
|schemas.microsoft.com:80 |
|schemas.microsoft.com:443 |
|Management.Core.Windows.NET:443 |
|Management.Core.Windows.NET:80 |
|management.azure.com:443 |
|www.msftconnecttest.com:80 |
|shavamanifestcdnprod1.azureedge.net:443 |
|doğrulama v2.sls.microsoft.com:443 |
|flighting.CP.WD.microsoft.com:443 |
|DMD.metaservices.microsoft.com:80 |
|Admin.Core.Windows.NET:443 |
|azureprofileruploads.BLOB.Core.Windows.NET:443 |
|azureprofileruploads2.BLOB.Core.Windows .net: 443 |
|azureprofileruploads3.BLOB.Core.Windows .net: 443 |
|azureprofileruploads4.BLOB.Core.Windows .net: 443 |
|azureprofileruploads5.BLOB.Core.Windows .net: 443 |

#### <a name="wildcard-httphttps-dependencies"></a>Joker karakter HTTP/HTTPS bağımlılıkları 

| Uç Nokta |
|----------|
|gr-Prod-\*.cloudapp.net:443 |
| \*.management.azure.com:443 |
| \*. update.microsoft.com:443 |
| \*.windowsupdate.microsoft.com:443 |

#### <a name="linux-dependencies"></a>Linux bağımlılıkları 

| Uç Nokta |
|----------|
|wawsinfraprodbay063.blob.core.windows.net:443 |
|kayıt defteri 1.docker.io:443 |
|auth.docker.io:443 |
|production.cloudflare.docker.com:443 |
|download.docker.com:443 |
|us.archive.ubuntu.com:80 |
|download.Mono project.com:80 |
|Packages.treasuredata.com:80|
|Security.ubuntu.com:80 |

<!--Image references-->
[1]: ./media/firewall-integration/firewall-apprule.png
[2]: ./media/firewall-integration/firewall-serviceendpoints.png
[3]: ./media/firewall-integration/firewall-ntprule.png
[4]: ./media/firewall-integration/firewall-routetable.png
[5]: ./media/firewall-integration/firewall-topology.png
