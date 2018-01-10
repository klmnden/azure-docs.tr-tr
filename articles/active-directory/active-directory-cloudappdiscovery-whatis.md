---
title: "Azure Active Directory'de Cloud App Discovery ile yönetilmeyen bulut uygulamaları bulmak | Microsoft Docs"
description: "Bulma ve Cloud App Discovery, avantajları nelerdir ve nasıl çalıştığı ile uygulamaları yönetme hakkında bilgi sağlar."
services: active-directory
keywords: "cloud app discovery'yi, uygulamaları yönetme"
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: db968bf5-22ae-489f-9c3e-14df6e1fef0a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 1d0ad06fc7eec07f8e1e0ba47121b6eec01c87df
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2018
---
# <a name="find-unmanaged-cloud-applications-with-cloud-app-discovery"></a>Cloud App Discovery ile yönetilmeyen bulut uygulamaları Bul
## <a name="summary"></a>Özet

Azure Active Directory'de cloud App Discovery, artık Microsoft Cloud App Security tarafından desteklenen bir Gelişmiş aracısız bulma deneyimi sağlar. Cloud App Discovery'yi kullanmak için yalnızca Azure AD Premium P1 kimlik bilgilerinizle oturum. Bu güncelleştirme, tüm Azure AD Premium P1 müşteriler için ek ücret ödemeden mevcuttur. Makaleyi ile çalışmaya başlama [Azure AD Cloud App Discovery ayarlamak](https://docs.microsoft.com/azure/active-directory/cloudappdiscovery-get-started), ardından denemenin [Microsoft Cloud App Security](https://portal.cloudappsecurity.com/).

> [!IMPORTANT] 
> Geçerli tabanlı aracı bulma ile 5 Mart 2018 üzerinde kapatılmasına deneyimidir Azure AD Cloud App Discovery sonra aracıları devre dışı bırakılır ve veri olacağı silindi. Lütfen önce hazır ve çalışır hizmet kesintisi yaşamamak için yeni deneyimi almak için 5 Mart eylemi uygulayın.  
 
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

