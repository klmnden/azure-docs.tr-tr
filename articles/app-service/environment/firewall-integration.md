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
ms.date: 09/24/2018
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 52051ea221a3d49d86cc6b95e020e1075ce8cba2
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53275559"
---
# <a name="locking-down-an-app-service-environment"></a>App Service ortamı kilitleme

App Service ortamı (ASE) birkaç şekilde çalışması için erişim gerektiren bir Dış bağımlılıklara sahiptir. ASE, müşterinin Azure sanal ağı (VNet) bulunur. Müşteriler, kendi vnet'ten tüm çıkış kilitlemek istediğiniz müşteriler için bir sorun olduğunu ASE bağımlılık trafiğine izin vermelidir.

Bir ASE sahip gelen bağımlılıklar vardır. Gelen yönetim trafiğinin bir güvenlik duvarı cihazı üzerinden gönderilemez. Bu trafiğe ait kaynak adresleri bilinen ve yayınlanır [App Service ortamı yönetim adresleri](https://docs.microsoft.com/azure/app-service/environment/management-addresses) belge. Gelen trafiği güvenli hale getirmek için bu bilgileri ile ağ güvenlik grubu kuralları oluşturabilirsiniz.

ASE giden bağımlılık neredeyse tamamen statik adresleri arkasına olmayan FQDN ile tanımlanır. Statik adresler olmaması anlamına gelir ağ güvenlik grupları (Nsg'ler) ASE giden trafiği kilitlemek için kullanılamaz. Adresleri sıklıkta biri olamaz geçerli çözünürlüğüne göre kurallarını ayarlama ve Nsg'ler oluşturma kullanan, değiştirin. 

Etki alanı adlarını temel alarak giden trafiği denetleyen bir güvenlik duvarı cihazın kullanımda giden adresleri güvenliğini sağlamak için çözüm arasındadır. Azure güvenlik duvarı hedef FQDN'sini üzerinde giden HTTP ve HTTPS trafiğini kısıtlayabilirsiniz.  

## <a name="configuring-azure-firewall-with-your-ase"></a>Azure güvenlik duvarı ile ASE'nizi yapılandırma 

Azure güvenlik duvarı ile gidenler çıkış kilitlemek için adımlar şunlardır:

1. Bir Azure güvenlik duvarı burada ASE'nizi, veya olacaktır sanal ağda oluşturun. [Azure güvenlik duvarı platformlarının](https://docs.microsoft.com/azure/firewall/)
2. Azure güvenlik duvarı Arabiriminden App Service ortamı FQDN etiketi seçin
3. Yönetim adresleri ile yönlendirme tablosu oluşturma [App Service ortamı yönetim adresleri]( https://docs.microsoft.com/azure/app-service/environment/management-addresses) bir sonraki atlama internet ile. Asimetrik yönlendirme sorunlarını önlemek için rota tablosu girdileri gerekir.
4. Bir sonraki atlama internet IP adresi bağımlılıklarla aşağıda belirtilen IP adresi bağımlılıklar için yollar ekleyin.
5. Bir rota, 0.0.0.0/0 için rota tablosu, Azure güvenlik duvarı olan sonraki atlama ile ekleyin.
6. ASE alt ağınız Azure SQL ve Azure depolama için hizmet uç noktaları oluşturun.
7. ASE alt ağınız için oluşturduğunuz yol tablosu atayın.

## <a name="application-traffic"></a>Uygulama trafiği 

Yukarıdaki adımları ASE'nizi sorunsuz çalışmasına izin verir. Yine de şeyler uygulama ihtiyaçlarınıza uyum sağlayacak şekilde yapılandırmanız gerekir. Azure güvenlik duvarı ile yapılandırılmış bir ase'deki uygulamalar için iki sorunu vardır.  

- Azure güvenlik duvarı ya da rota tablosunu uygulama bağımlılığı FQDN'leri eklenmelidir
- Yol için asimetrik yönlendirme sorunlarını önlemek için kaynağından trafiği gelir adresleri oluşturulmalıdır

Uygulamalarınızı bağımlılıkları varsa, bunların Azure güvenlik duvarını eklenmesi gerekir. HTTP/HTTPS trafiğine izin vermek ve diğer her şey için kuralları ağ uygulama kuralları oluşturun. 

Uygulama isteği trafiğiniz kaynağından gelir adres aralığını biliyorsanız, yol tablosuna, ASE alt ağınız için atanmış ekleyebilirsiniz. Adres aralığı büyük ya da belirtilmemiş olması durumunda, yol tablosuna eklemek için bir adres sağlamak için Application Gateway gibi ağ Gereci kullanabilirsiniz. ILB ASE'nizi bir Application Gateway yapılandırma hakkında ayrıntılı bilgi edinmek için [ILB ASE'nizi bir Application Gateway ile tümleştirme](https://docs.microsoft.com/azure/app-service/environment/integrate-with-application-gateway)


## <a name="dependencies"></a>Bağımlılıklar

Azure App Service, birkaç dış bağımlılık vardır. Bunlar güveninin birkaç ana bölüme ayrılabilir:

- Hizmet uç noktası hizmet uç noktaları ile yukarı giden ağ trafiği kilitlemek istediğiniz özellikli hizmetler ayarlanmalıdır.
- Bir etki alanı adı ile IP adresi uç noktalarını yönelik değildir. Bu etki alanı adları için tüm HTTPS trafiğini beklediğiniz güvenlik duvarı cihazları için bir sorun olabilir. ASE alt ağda ayarlanan yol tablosuna IP adresi uç noktalarını eklenmesi gerekir.
- FQDN HTTP/HTTPS uç noktaları güvenlik duvarı Cihazınızı yerleştirilebilir.
- Joker karakter HTTP/HTTPS uç noktaları ile ASE'nizi niteleyicileri sayısına göre değişebilir bağımlılıklardır. 
- ASE'niz Linux uygulamaları dağıtıyorsanız Linux bağımlılıkları yalnızca bir sorun var. Linux uygulamaları ASE'nizi değil dağıtıyorsanız, ardından bu adresleri güvenlik duvarını eklenmesi gerekmez. 


#### <a name="service-endpoint-capable-dependencies"></a>Hizmet uç noktası özellikli bağımlılıkları 

| Uç Nokta |
|----------|
| Azure SQL |
| Azure Storage |
| Azure anahtar kasası |


#### <a name="ip-address-dependencies"></a>IP adresi bağımlılıkları 

| Uç Nokta |
|----------|
| 40.77.24.27:443 |
| 13.82.184.151:443 |
| 13.68.109.212:443 |
| 13.90.249.229:443 |
| 13.91.102.27:443 |
| 104.45.230.69:443 |
| 168.62.226.198:12000 |


#### <a name="fqdn-httphttps-dependencies"></a>FQDN HTTP/HTTPS bağımlılıkları 

| Uç Nokta |
|----------|
|graph.windows.net:443 |
|login.live.com:443 |
|Login.Windows.com:443 |
|Login.Windows.NET:443 |
|login.microsoftonline.com:443 |
|Client.WNS.Windows.com:443 |
|definitionupdates.microsoft.com:443 |
|go.microsoft.com:80 |
|go.microsoft.com:443 |
|www.microsoft.com:80 |
|www.microsoft.com:443 |
|wdcpalt.microsoft.com:443 |
|wdcp.microsoft.com:443 |
|OCSP.msocsp.com:443 |
|mscrl.microsoft.com:443 |
|mscrl.microsoft.com:80 |
|CRL.microsoft.com:443 |
|CRL.microsoft.com:80 |
|www.Thawte.com:443 |
|crl3.digicert.com:80 |
|OCSP.digicert.com:80 |
|csc3 2009 2.crl.verisign.com:80 |
|CRL.VeriSign.com:80 |
|OCSP.VeriSign.com:80 |
|azperfcounters1.BLOB.Core.Windows .net: 443 |
|azurewatsonanalysis prod.core.windows.net:443 |
|Global.Metrics.nsatc.NET:80   |
|az prod.metrics.nsatc.net:443 |
|antares.Metrics.nsatc.NET:443 |
|azglobal black.azglobal.metrics.nsatc.net:443 |
|azglobal red.azglobal.metrics.nsatc.net:443 |
|antares black.antares.metrics.nsatc.net:443 |
|antares red.antares.metrics.nsatc.net:443 |
|maupdateaccount.BLOB.Core.Windows.NET:443 |
|clientconfig.Passport.NET:443 |
|Packages.microsoft.com:443 |
|schemas.microsoft.com:80 |
|schemas.microsoft.com:443 |
|Management.Core.Windows.NET:443 |
|Management.Core.Windows.NET:80 |
|www.msftconnecttest.com:80 |
|shavamanifestcdnprod1.azureedge .net: 443 |
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
|Gr-Prod -\*. cloudapp.net:443 |
| \*. management.azure.com:443 |
| \*. update.microsoft.com:443 |
| \*. windowsupdate.microsoft.com:443 |
|grmdsprod\*mini\*. servicebus.windows.net:443 |
|grmdsprod\*lini\*. servicebus.windows.net:443 |
|grsecprod\*mini\*. servicebus.windows.net:443 |
|grsecprod\*lini\*. servicebus.windows.net:443 |
|graudprod\*mini\*. servicebus.windows.net:443 |
|graudprod\*lini\*. servicebus.windows.net:443 |

#### <a name="linux-dependencies"></a>Linux bağımlılıkları 

| Uç Nokta |
|----------|
|wawsinfraprodbay063.BLOB.Core.Windows .net: 443 |
|kayıt defteri 1.docker.io:443 |
|auth.docker.io:443 |
|Production.cloudflare.docker.com:443 |
|download.docker.com:443 |
|US.archive.ubuntu.com:80 |
|download.Mono project.com:80 |
|Packages.treasuredata.com:80|
|Security.ubuntu.com:80 |

