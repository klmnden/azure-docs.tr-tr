---
title: "Azure service fabric güvenlik denetim listesi | Microsoft Docs"
description: "Bu makalede, bir dizi denetim listesi için Azure doku güvenlik güvenlik sağlar."
services: security
documentationcenter: na
author: unifycloud
manager: swadhwa
editor: tomsh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/04/2017
ms.author: tomsh
ms.openlocfilehash: d0441f1e96e94352d4112ec387058b47074d8b0b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-service-fabric-security-checklist"></a>Azure Service Fabric güvenlik denetim listesi
Bu makale Azure Service Fabric ortamınızın güvenliğini sağlamanıza yardımcı olacak bir kullanımı kolay denetim sağlar.

## <a name="introduction"></a>Giriş
Azure Service Fabric; ölçeklenebilir ve güvenilir mikro hizmetleri paketlemeyi, dağıtmayı ve yönetmeyi kolaylaştırmayı sağlayan bir dağıtılmış sistemler platformudur. Service Fabric ayrıca bulut uygulamalarını geliştirme ve yönetme sürecinde karşılaşılan başlıca sorunların giderilmesini de sağlar. Geliştiriciler ve yöneticiler, karmaşık altyapı sorunlarını çözmeye çalışmak yerine görev açısından kritik, zorlu iş yüklerini uygulamaya odaklanabilir. Service Fabric, bu iş yüklerinin ölçeklenebilir, güvenilir ve yönetilebilir olmasını sağlar.

## <a name="checklist"></a>Denetim listesi
Yönetim ve yapılandırma, güvenli Azure Service Fabric çözümünün önemli sorunları atlamış henüz emin olun yardımcı olması için aşağıdaki denetim listesini kullanın.


|Denetim listesi kategorisi| Açıklama |
| ------------ | -------- |
|[Rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-security-roles) | <ul><li>Erişim denetimi, belirli küme işlemleri farklı küme daha güvenli hale getirme kullanıcı grupları için erişimi sınırlamak Küme Yöneticisi sağlar.</li><li>Yöneticiler için yönetim özellikleri (okuma/yazma özellikleri dahil) tam erişime sahip. </li><li>   Kullanıcıların varsayılan olarak, yalnızca yönetim özellikleri (örneğin, sorgu özellikleri) okuma erişimi ve uygulamaları ve Hizmetleri çözümleme olanağı vardır.</li></ul>|
|[X.509 sertifikaları ve Service Fabric](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-security) | <ul><li>[Sertifikaları](https://docs.microsoft.com/en-us/dotnet/framework/wcf/feature-details/working-with-certificates) çalışan üretim iş yükleri doğru yapılandırılmış bir Windows Server sertifika hizmetini kullanarak oluşturulmamalı veya bir onaylanan alınan kümelerinde kullanılan [sertifika yetkilisi (CA)](https://en.wikipedia.org/wiki/Certificate_authority).</li><li>Hiçbir zaman kullanmak [geçici veya test sertifikaları](https://docs.microsoft.com/en-us/dotnet/framework/wcf/feature-details/how-to-create-temporary-certificates-for-use-during-development) gibi araçları ile oluşturulmuş üretimde [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968.aspx). </li><li>Kullanabileceğiniz bir [otomatik olarak imzalanan sertifika](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-windows-cluster-x509-security) ancak yalnızca test kümeleri için alan ve üretim yapmalısınız.</li></ul>|
|[Küme güvenliği](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-security) | <ul><li>Küme güvenlik senaryoları düğümü düğümü güvenlik, istemci düğümü güvenliği dahil [rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-security-roles).</li></ul>|
|[Küme kimlik doğrulaması](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-creation-via-arm) | <ul><li>Kimlik doğrulaması [düğümü düğümü iletişimi](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/service-fabric/service-fabric-cluster-security.md) küme Federasyon. </li></ul>|
|[Sunucu kimlik doğrulaması](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-creation-via-arm) | <ul><li>Kimlik doğrulaması [yönetim uç noktalarının küme](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-creation-via-portal) bir yönetim istemcisi için.</li></ul>|
|[Uygulama güvenliği](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-creation-via-arm)| <ul><li>Şifreleme ve şifre çözme uygulama yapılandırma değerlerini.</li><li> Çoğaltma sırasında şifreleme düğümler arasında veri.</li></ul>|
|[Küme sertifikası](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-windows-cluster-x509-security) | <ul><li>Bu sertifika, bir küme düğümlerinde arasındaki iletişimin güvenliğini sağlamak için gereklidir.</li><li>  Parmak izi bölüm ve ikincil ThumbprintSecondary değişkenlerine sertifikanın parmak izi birincil ayarlayın.</li></ul>|
|[ServerCertificate](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-windows-cluster-x509-security)| <ul><li>Bu kümeye bağlanmaya çalıştığında bu sertifikayı istemciye sunulur. İki farklı sunucu sertifikaları, birincil ve ikincil bir yükseltme için kullanabilirsiniz.</li></ul>|
|ClientCertificateThumbprints| <ul><li>Kimliği doğrulanmış istemcilerde yüklemek istediğiniz sertifika kümesidir. </li></ul>|
|ClientCertificateCommonNames| <ul><li>İlk istemci sertifikasının ortak adı için CertificateCommonName ayarlayın. Bu sertifika verenin parmak izini CertificateIssuerThumbprint olur. </li></ul>|
|ReverseProxyCertificate| <ul><li>Bu, olabilir, isteğe bağlı bir sertifikadır güvenli isteyip istemediğinizi belirtilen, [Ters Proxy](https://docs.microsoft.com/en-in/azure/service-fabric/service-fabric-reverseproxy). </li></ul>|
|Anahtar Kasası| <ul><li>Azure Service Fabric kümeleri sertifikalarını yönetmek için kullanılır.  </li></ul>|


## <a name="next-steps"></a>Sonraki adımlar
- [Service Fabric kümesi yükseltme işlemi ve sizden beklentileri](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-upgrade)
- [Visual Studio'da, Service Fabric uygulamaları yönetme](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-manage-application-in-visual-studio).
- [Service Fabric sistem durumu modeli giriş](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-health-introduction).
