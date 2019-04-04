---
title: Karma Bağlantılara genel bakış | Microsoft Belgeleri
description: Karma Bağlantılar, güvenlik, TCP bağlantı noktaları ve desteklenen yapılandırmalar hakkında bilgi edinin. MABS, WABS.
services: biztalk-services
documentationcenter: ''
author: MandiOhlinger
manager: erikre
editor: ''
ms.assetid: 216e4927-6863-46e7-aa7c-77fec575c8a6
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/18/2016
ms.author: ccompy
ms.openlocfilehash: 347ea75673336574f7517f2f7d0c802b0ed16560
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "58918306"
---
# <a name="hybrid-connections-overview"></a>Karma Bağlantılara genel bakış

> [!IMPORTANT]
> BizTalk Karma Bağlantılar kullanımdan kalktı ve yerine App Service Karma Bağlantılar kullanıma sunuldu. Var olan BizTalk Karma Bağlantılarınızı nasıl yöneteceğiniz de dahil olmak üzere daha fazla bilgi için bkz. [Azure App Service Karma Bağlantılar](../app-service/app-service-hybrid-connections.md).

Karma Bağlantılar’a giriş, desteklenen yapılandırmaları listeler ve gerekli TCP bağlantı noktalarını listeler.

## <a name="what-is-a-hybrid-connection"></a>Karma bağlantı nedir
Karma Bağlantılar, Azure BizTalk Services’ın bir özelliğidir. Karma Bağlantılar, Azure App Service’deki Web Uygulamaları özelliğine (önceden Web Siteleri) ve Azure App Service’deki Mobile Apps özelliğine (önceden Mobile Services), güvenlik duvarının ardındaki şirket içi kaynaklara bağlanmanın kolay ve uygun bir yolunu sağlar.

![Karma Bağlantılar][HCImage]

Karma Bağlantılar’ın yararları:

* Web Uygulamaları ve Mobile Apps mevcut şirket içi verilere ve hizmetlere güvenli erişebilir.
* Birden çok Web Uygulaması veya Mobil uygulama şirket içi kaynağa erişmek için Karma Bağlantı’yı paylaşabilir.
* Ağa erişmek için çok az sayıda TCP bağlantı noktası gerekir.
* Karma Bağlantılar’ı kullanan uygulamalar, yalnızca Karma Bağlantılar’la yayımlanmış belirli şirket içi kaynağa erişir.
* SQL Sunucusu, MySQL, HTTP Web API’leri ve çok sayıda özel Web Hizmeti gibi statik TCP bağlantı noktası kullanan şirket içi kaynaklara bağlanabilir.
  
  > [!NOTE]
  > Dinamik bağlantı noktaları (örneğin, FTP Pasif modu veya Genişletilmiş Pasif modu) kullanan TCP tabanlı hizmetler şu anda desteklenmemektedir. LDAP de desteklenmemektedir. LDAP statik bir TCP bağlantı noktasını kullansa da, UDP de kullanmaktadır. Sonuç olarak desteklenmemektedir.
  > 
  > 
* Web Uygulamaları (.NET, PHP, Java, Python, Node.js) ve Mobile Apps (Node.js, .NET) tarafından desteklenen tüm altyapılarla kullanılabilir.
* Web Uygulamaları veya Mobile Apps, tam da yerel ağınızda bulunan Web veya Mobil Uygulama gibi aynı şekilde şirket içi kaynaklara erişebilir. Örneğin, şirket içi kullanılan aynı bağlantı dizesi Azure üzerinde de kullanılabilir.

Karma Bağlantılar, karma uygulamaların eriştiği kurum kaynaklarını kurum yöneticilerinin denetleyip görselleştirmesini de sağlar; bunlar arasında şunlar da yer alır:

* Grup İlkesi ayarlarını kullanarak, yöneticiler Karma Bağlantılar’a ağda izin verebilir ve karma uygulamaların erişebildiği kaynakları atayabilir.
* Şirket ağındaki etkinlik ve denetim günlükleri Karma Bağlantılar’ın eriştiği kaynaklara görünürlük sağlar.

## <a name="example-scenarios"></a>Örnek senaryolar
Karma Bağlantılar aşağıdaki altyapı ve uygulama bileşimlerini destekler:

* SQL Server için .NET altyapı erişimi
* WebClient ile HTTP/HTTPS hizmetleri için .NET altyapı erişimi
* SQL Server, MySQL için PHP erişimi
* SQL Server, MySQL ve Oracle için Java erişimi
* HTTP/HTTPS hizmetleri için Java erişimi

Şirket içi SQL Server’a erişmek için Karma Bağlantılar kullanıldığında şunları göz önünde bulundurun:

* SQL Express Adlandırılmış Örnekleri statik bağlantı noktalarını kullanacak şekilde yapılandırılmalıdır. Varsayılan olarak, SQL Express adlandırılmış örnekleri dinamik bağlantı noktalarını kullanır.
* SQL Express Varsayılan Örnekleri statik bağlantı noktasını kullansa da TCP’nin de etkinleştirilmesi gerekir. Varsayılan olarak, TCP etkin değildir.
* Kümeleme veya Kullanılabilirlik Grupları kullanılırken `MultiSubnetFailover=true` modu şu anda desteklenmiyor.
* `ApplicationIntent=ReadOnly` şu anda desteklenmiyor.
* SQL Kimlik Doğrulaması, Azure uygulamasının ve şirket içi SQL sunucusunun desteklediği uçtan uca yetkilendirme yöntemi olarak gerekebilir.

## <a name="security-and-ports"></a>Güvenlik ve bağlantı noktaları
Karma Bağlantılar, Azure uygulamalarından ve şirket içi Karma Bağlantı Yöneticisi’nden Karma Bağlantılar’a gelen bağlantıların güvenliğini sağlamak için Paylaşılan Erişim İmzası (SAS) yetkilendirmesini kullanır. Uygulama ve şirket içi Karma Bağlantı Yöneticisi için ayrı bağlantı anahtarları oluşturulur. Bu bağlantı anahtarları uzatılabilir ve bağımsız olarak iptal edilebilir.

Karma Bağlantılar, uygulamalara ve şirket içi Karma Bağlantı Yöneticisi’ne anahtarların sorunsuz ve güvenli dağıtılmasını sağlar.

Bkz. [Karma Bağlantıları Oluşturma ve Yönetme](integration-hybrid-connection-create-manage.md).

*Uygulama yetkilendirmesi Karma Bağlantı’dan ayrıdır*. Uygun herhangi bir yetkilendirme yöntemi kullanılabilir. Yetkilendirme yöntemi, Azure bulutu ve şirket içi bileşenler arasında desteklenen uçtan uca yetkilendirme yöntemlerine bağlıdır. Örneğin, Azure uygulamanız bir şirket içi SQL Server’a erişir. Bu senaryoda, SQL etkilendirme uçtan uca destekleyen yetkilendirme yöntemi olabilir.

#### <a name="tcp-ports"></a>TCP bağlantı noktaları
Karma Bağlantılar için, yalnızca özel ağınızdan giden TCP veya HTTP bağlantısı gerekir. Herhangi bir güvenlik duvarı bağlantı noktasını açmanız veya gelen bağlantılara ağınızda izin vermek için ağ çevre yapılandırmasını değiştirmeniz gerekmez.

Aşağıdaki TCP bağlantı noktaları Karma Bağlantılar tarafından kullanılır:

| Bağlantı noktası | Neden gerekiyor |
| --- | --- |
| 9350 - 9354 |Bu bağlantı noktaları veri aktarımı için kullanılır. Hizmet Veri Yolu geçişi yöneticisi, TCP bağlantısı olup olmadığını saptamak için 9350 adlı bağlantı noktasını incelemelidir. Varsa, 9352 adlı bağlantı noktasının da olduğu varsayılır. Veri trafiği 9352 bağlantı noktası üzerinden gider. <br/><br/>Bu bağlantı noktalarına giden bağlantılara izin verin. |
| 5671 |9352 adlı bağlantı noktası veri trafiği için kullanıldığında, 5671 bağlantı noktası denetim kanalını kullanılır. <br/><br/>Bu bağlantı noktasına giden bağlantılara izin verin. |
| 80, 443 |Bu bağlantı noktaları Azure’e bazı veri isteklerini iletmek için kullanılır. Ayrıca, 9352 ve 5671 bağlantı noktaları kullanıma hazır değilse, *bu nedenle* 80 ve 443 bağlantı noktaları, veri iletimi ve denetim kanalı için kullanılan temel bağlantı noktalarıdır.<br/><br/>Bu bağlantı noktalarına giden bağlantılara izin verin. <br/><br/>**Not** Diğer TCP bağlantı noktaları yerine bunların temel bağlantı noktası olarak kullanılması önerilmez. HTTP/WebSocket, veri kanallarına yönelik yerel TCP yerine protokol olarak kullanılır. Düşük performansa neden olabilir. |

## <a name="next-steps"></a>Sonraki adımlar
[Karma Bağlantıları oluşturma ve yönetme](integration-hybrid-connection-create-manage.md)

## <a name="see-also"></a>Ayrıca Bkz.
[Microsoft azure'da BizTalk hizmetlerinin yönetilmesi için REST API](/previous-versions/azure/reference/dn232347(v=azure.100))  
[BizTalk Services: Sürümler Grafiği](biztalk-editions-feature-chart.md)  
[BizTalk Hizmeti oluşturma](biztalk-provision-services.md)  
[BizTalk Services: Pano, İzleyici ve ölçek sekmeleri](biztalk-dashboard-monitor-scale-tabs.md)  

[HCImage]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionImage.png
