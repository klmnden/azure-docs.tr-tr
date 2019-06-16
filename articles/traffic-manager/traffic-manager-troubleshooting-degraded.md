---
title: Sorun giderme durumu Azure Traffic Manager'da düşürülmüş
description: Traffic Manager profillerini olarak gösteriliyorsa sorun giderme durumu düşürülmüş.
services: traffic-manager
documentationcenter: ''
author: chadmath
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2017
ms.author: genli
ms.openlocfilehash: f01dfe78d5d5e322258b0ee98cec314f9afe33c0
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60329760"
---
# <a name="troubleshooting-degraded-state-on-azure-traffic-manager"></a>Sorun giderme durumu Azure Traffic Manager'da düşürülmüş

Bu makalede bir düşürülmüş durumunu gösteren bir Azure Traffic Manager profili sorunlarını giderme. Bu senaryo için bazı cloudapp.net barındırılan hizmetlerinizi işaret eden bir Traffic Manager profili yapılandırdığınız göz önünde bulundurun. Traffic Manager'ın sistem durumunu görüntüler, bir **Degraded** durumu ve ardından bir veya daha fazla uç nokta durumunu olabilir **Degraded**:

![düzeyi düşürülmüş bir uç nokta durumu](./media/traffic-manager-troubleshooting-degraded/traffic-manager-degradedifonedegraded.png)

Traffic Manager'ın sistem durumunu görüntüler, bir **Inactive** durumu ve ardından her iki bitiş noktalarını olabilir **devre dışı bırakılmış**:

![Etkin olmayan Traffic Manager durumu](./media/traffic-manager-troubleshooting-degraded/traffic-manager-inactive.png)

## <a name="understanding-traffic-manager-probes"></a>Traffic Manager anlama araştırmaları

* Traffic Manager uç nokta yalnızca araştırma araştırma yoldan bir HTTP 200 yanıtı aldığında çevrimiçi olması göz önünde bulundurur. Herhangi bir 200 yanıt hatasıdır.
* 200 yeniden yönlendirilen URL'yi döndürür, 30 x yeniden yönlendirme başarısız olur.
* HTTPs araştırmaları için sertifika hataları göz ardı edilir.
* 200 döndürdüğü sürece yoklama yolu gerçek içeriği önemli değildir. Bazı statik içerik gibi bir URL araştırma "/ favicon.ico" yaygın bir tekniktir. ASP sayfalarının gibi dinamik içerik uygulama iyi durumda olduğunda bile her zaman 200 döndürmeyebilir.
* Site yukarı veya aşağı olduğunu belirlemek için yeterli mantığı içeren bir araştırma yolunu ayarlayın. en iyi bir uygulamadır. Yolun ayarlayarak önceki örnekte, "/ favicon.ico", yalnızca işiniz, w3wp.exe test yanıt. Bu araştırma, web uygulamanızın sağlıklı olduğunu göstermeyebilir. Gibi bir şey için bir yol ayarlamak için daha iyi bir seçenek olacaktır "/ Probe.aspx" sitenin durumunu belirlemek için mantığı vardır. Örneğin, CPU kullanımı için performans sayaçlarını veya başarısız istek sayısı ölçün. Veya veritabanı kaynakları veya web uygulamasının çalıştığından emin olmak için oturum durumu erişme girişiminde bulunabilir.
* Bir profildeki tüm uç noktalar düşürülmüş, ardından Traffic Manager tüm uç noktalar olarak sağlıklı ve trafiği yönlendirir tüm uç noktalar için değerlendirir. Bu davranış, araştırma mekanizması ile ilgili sorunlar hizmetinizin tam bir kesintisi neden sağlar.

## <a name="troubleshooting"></a>Sorun giderme

Bir araştırma hatası sorunlarını gidermek için HTTP durum kodu araştırma URL'den dönüş gösteren bir aracı gerekir. Ham HTTP yanıtı gösteren birçok araç vardır.

* [Fiddler](https://www.telerik.com/fiddler)
* [Curl](https://curl.haxx.se/)
* [wget](http://gnuwin32.sourceforge.net/packages/wget.htm)

Ayrıca, HTTP yanıtlarını görüntülemek için Internet Explorer'da F12 hata ayıklama araçları ağı sekmesini kullanabilirsiniz.

İstediğimiz bizim araştırma URL'si yanıtı görmek Bu, örneğin: http:\//watestsdp2008r2.cloudapp.net:80/Probe. Aşağıdaki PowerShell örneği, bir sorunu gösterir.

```powershell
Invoke-WebRequest 'http://watestsdp2008r2.cloudapp.net/Probe' -MaximumRedirection 0 -ErrorAction SilentlyContinue | Select-Object StatusCode,StatusDescription
```

Örnek çıktı:

    StatusCode StatusDescription
    ---------- -----------------
           301 Moved Permanently

Bir yeniden yönlendirme yanıt aldık dikkat edin. 200 herhangi StatusCode dışında daha önce belirtildiği gibi bir hata olarak kabul edilir. Traffic Manager uç nokta durumu çevrimdışı olarak değiştirir. Bu sorunu gidermek için uygun StatusCode araştırma yolundan döndürülebilir emin olmak için Web sitesi yapılandırmasını denetleyin. Traffic Manager araştırma 200 döndüren bir yolu işaret edecek şekilde yeniden yapılandırın.

Araştırma HTTPS protokolünü kullanıyorsanız, sertifika testiniz sırasında SSL/TLS hatalarını önlemek için denetlemeyi devre dışı bırakmak gerekebilir. Aşağıdaki PowerShell ifadeleri geçerli PowerShell oturumunda sertifika doğrulaması devre dışı bırakın:

```powershell
add-type @"
using System.Net;
using System.Security.Cryptography.X509Certificates;
public class TrustAllCertsPolicy : ICertificatePolicy {
    public bool CheckValidationResult(
    ServicePoint srvPoint, X509Certificate certificate,
    WebRequest request, int certificateProblem) {
    return true;
    }
}
"@
[System.Net.ServicePointManager]::CertificatePolicy = New-Object TrustAllCertsPolicy
```

## <a name="next-steps"></a>Sonraki Adımlar

[Traffic Manager trafik yönlendirme yöntemleri hakkında](traffic-manager-routing-methods.md)

[Traffic Manager nedir](traffic-manager-overview.md)

[Cloud Services](https://go.microsoft.com/fwlink/?LinkId=314074)

[Azure uygulama hizmeti](https://azure.microsoft.com/documentation/services/app-service/web/)

[Traffic Manager üzerindeki işlemler (REST API Başvurusu)](https://go.microsoft.com/fwlink/?LinkId=313584)

[Azure Traffic Manager cmdlet'leri][1]

[1]: https://docs.microsoft.com/powershell/module/az.trafficmanager
