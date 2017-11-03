---
title: "Azure Active Directory'de Cloud App Discovery ile yönetilmeyen bulut uygulamaları bulmak | Microsoft Docs"
description: "Bulma ve Cloud App Discovery, avantajları nelerdir ve nasıl çalıştığı ile uygulamaları yönetme hakkında bilgi sağlar."
services: active-directory
keywords: "cloud app discovery'yi, uygulamaları yönetme"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: db968bf5-22ae-489f-9c3e-14df6e1fef0a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 35b898aa3c03aeef914a7df574ac65a22a6c7bec
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="find-unmanaged-cloud-applications-with-cloud-app-discovery"></a>Cloud App Discovery ile yönetilmeyen bulut uygulamaları Bul
## <a name="summary"></a>Özet

Cloud App Discovery, Azure Active Directory kuruluşunuzdaki kişiler tarafından kullanılan yönetilmeyen bulut uygulamaları bulmanızı sağlayan Premium özelliğidir. Modern kuruluşlarda, BT departmanları işlerini yapmak için kuruluşları üyeleri kullanan tüm bulut uygulamaları tanımaz genellikle. Yöneticileri Kurumsal verileri, olası veri sıkıntılarına ve diğer güvenlik risklerine yetkisiz erişimi endişeniz neden olurdu görmek kolaydır. Bu tanıma olmaması, bu güvenlik riskleri postalarla göründüğü için göz korkutucu bir plan oluşturma yapabilirsiniz.

> [!TIP] 
> Azure Active Directory'de tarafından geliştirilmiş (Azure AD), Cloud App Discovery geliştirmeleri kullanıma [Microsoft Cloud App Security ile tümleştirme](https://portal.cloudappsecurity.com).

**Cloud App Discovery ile şunları yapabilirsiniz:**

* Kullanılan bulut uygulamalarını bulmak ve bu kullanım ölçme kullanıcı sayısı, akış hacmine veya uygulamaya web isteklerinin sayısı.
* Bir uygulama kullanarak kullanıcıları belirleyin.
* Çevrimdışı analiz için verileri dışa aktarın.
* Bu uygulamaları BT denetimindeki Getir ve kullanıcı yönetimi için çoklu oturum açmayı etkinleştirin.

## <a name="how-it-works"></a>Nasıl çalışır?
1. Uygulama kullanımı aracıları kullanıcının bilgisayarlara yüklenir.
2. Aracıları tarafından yakalanan uygulama kullanım bilgilerini bulut uygulama bulma hizmeti için güvenli, şifrelenmiş bir kanal üzerinden gönderilir.
3. Cloud App Discovery hizmetine veri değerlendirir ve raporları oluşturur.

![Cloud App Discovery diyagramı](./media/active-directory-cloudappdiscovery/cad01.png)


## <a name="next-steps"></a>Sonraki adımlar
* [Cloud App Discovery güvenlik ve gizlilik konuları](active-directory-cloudappdiscovery-security-and-privacy-considerations.md)  
* [Özel bağlantı noktaları ile Proxy sunucuları için bulut uygulama bulma kayıt defteri ayarları](active-directory-cloudappdiscovery-registry-settings-for-proxy-services.md)
* [Cloud App Discovery Aracısı değişim günlüğü](http://social.technet.microsoft.com/wiki/contents/articles/24616.cloud-app-discovery-agent-changelog.aspx)
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](active-directory-apps-index.md)

