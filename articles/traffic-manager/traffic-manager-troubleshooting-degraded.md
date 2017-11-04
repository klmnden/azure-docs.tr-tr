---
title: "Durum üzerinde Azure Traffic Manager düşürülmüş sorunlarını giderme"
description: "Traffic Manager profillerini olarak gösteriliyorsa ile ilgili sorunları giderme durumu düzeyi."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
ms.assetid: 8af0433d-e61b-4761-adcc-7bc9b8142fc6
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2017
ms.author: kumud
ms.openlocfilehash: b1d00fb84695d2289f37647f55a7c56cf28c8c96
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshooting-degraded-state-on-azure-traffic-manager"></a>Durum üzerinde Azure Traffic Manager düşürülmüş sorunlarını giderme

Bu makalede, düzeyi düşürülmüş durumunu gösteren bir Azure Traffic Manager profilini sorun giderme açıklar. Bu senaryo için bazı cloudapp.net barındırılan hizmetlerinizi işaret eden bir Traffic Manager profilini yapılandırdığınız göz önünde bulundurun. Sistem durumu, trafik Yöneticisi'nin görüntülerse bir **Degraded** durum sonra bir veya daha fazla uç noktaları durumunu olabilir **Degraded**:

![düzeyi düşürülmüş uç nokta durumu](./media/traffic-manager-troubleshooting-degraded/traffic-manager-degradedifonedegraded.png)

Sistem durumu, trafik Yöneticisi'nin görüntülerse bir **devre dışı** durum sonra her iki uç noktaları olabilir **devre dışı**:

![Etkin olmayan trafik Yöneticisi durumu](./media/traffic-manager-troubleshooting-degraded/traffic-manager-inactive.png)

## <a name="understanding-traffic-manager-probes"></a>Trafik Yöneticisi anlama yoklamaları

* Trafik Yöneticisi yalnızca araştırma araştırma yolundan bir HTTP 200 yanıtı aldığında çevrimiçi olması için bir uç nokta göz önünde bulundurur. Başka bir harici 200 yanıtı bir hatadır.
* Yeniden yönlendirilen URL bir 200 döndürürse bile 30 x yeniden yönlendirme başarısız olur.
* HTTPs araştırmaları için sertifika hataları göz ardı edilir.
* Bir 200 döndürülen sürece araştırma yolu gerçek içeriği önemli değildir. Yoklama gibi bazı statik içerik için bir URL "/ favicon.ico" ortak bir tekniktir. Uygulama sağlıklı olsa bile ASP sayfalarının gibi dinamik içerik 200, her zaman döndürmeyebilir.
* Sitenin yukarı veya aşağı olduğunu belirlemek için yeterli mantığı içeren bir araştırma yolunu ayarlama en iyi bir uygulamadır. Yolun ayarlayarak önceki örnekte, "/ favicon.ico", yalnızca olduğunuz bu w3wp.exe sınama yanıt. Bu araştırma, web uygulamanızın sağlıklı olduğunu göstermeyebilir. Bir yol gibi bir şeye ayarlamak için daha iyi bir seçenek olacaktır "/ Probe.aspx" sitenin durumunu belirlemek için mantığı vardır. Örneğin, CPU kullanımı için performans sayaçları kullanın veya başarısız istek sayısı ölçün. Veya veritabanı kaynakları veya web uygulamasının çalıştığından emin olmak için oturum durumu erişme girişimi.
* Bir profildeki tüm uç noktaları düşürülmüş, ardından trafik Yöneticisi tüm uç noktalar olarak sağlıklı ve yollar trafiğini tüm uç noktalara değerlendirir. Bu davranış, araştırma mekanizması sorunları hizmetinizin tam bir kesinti içinde yol açmamasını sağlar.

## <a name="troubleshooting"></a>Sorun giderme

Bir araştırma hatası gidermek için HTTP durum kodu araştırma URL'den dönüş gösteren bir aracı gerekir. Kullanılabilir ham HTTP yanıtı gösteren birçok araç vardır.

* [Fiddler](http://www.telerik.com/fiddler)
* [Curl](https://curl.haxx.se/)
* [wget](http://gnuwin32.sourceforge.net/packages/wget.htm)

Ayrıca, HTTP yanıtları görüntülemek için Internet Explorer'da F12 hata ayıklama araçları'nın Ağ sekmesini kullanabilirsiniz.

Bizim araştırma URL'si yanıttan görmeyi istiyoruz Bu, örneğin: http://watestsdp2008r2.cloudapp.net:80/araştırma. Aşağıdaki PowerShell örneğinde sorun gösterilmektedir.

```powershell
Invoke-WebRequest 'http://watestsdp2008r2.cloudapp.net/Probe' -MaximumRedirection 0 -ErrorAction SilentlyContinue | Select-Object StatusCode,StatusDescription
```

Örnek çıktı:

    StatusCode StatusDescription
    ---------- -----------------
           301 Moved Permanently

Yeniden yönlendirme yanıtı aldık dikkat edin. Daha önce tüm StatusCode dışında belirtildiği gibi 200 bir hata olarak kabul edilir. Trafik Yöneticisi uç noktası durumu çevrimdışı değiştirir. Sorunu gidermek için uygun StatusCode araştırma yolundan döndürüldüğünden emin olunması için Web sitesi yapılandırmasını denetleyin. Trafik Yöneticisi araştırma bir 200 döndüren bir yolunu işaret edecek şekilde yeniden yapılandırın.

Araştırma HTTPS protokolünü kullanıyorsanız, sertifika, test sırasında SSL/TLS hatalarını önlemek için denetlemeyi devre dışı bırakmanız gerekebilir. Aşağıdaki PowerShell ifadeleri geçerli PowerShell oturumunda sertifika doğrulaması devre dışı bırakın:

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

[Trafik Yöneticisi nedir](traffic-manager-overview.md)

[Cloud Services](http://go.microsoft.com/fwlink/?LinkId=314074)

[Azure Web Apps](https://azure.microsoft.com/documentation/services/app-service/web/)

[Traffic Manager üzerindeki işlemler (REST API Başvurusu)](http://go.microsoft.com/fwlink/?LinkId=313584)

[Azure Traffic Manager cmdlet'leri][1]

[1]: https://msdn.microsoft.com/library/mt125941(v=azure.200).aspx
