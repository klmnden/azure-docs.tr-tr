---
title: Azure Active Directory Raporlama ile ilgili SSS | Microsoft Docs
description: Azure Active Directory Raporlama ile ilgili SSS.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: 534da0b1-7858-4167-9986-7a62fbd10439
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 5fa52099f5cf55b78fd2fea407c34f29237939d3
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="azure-active-directory-reporting-faq"></a>SSS raporlama Azure Active Directory

Bu makalede raporlama Azure Active Directory (Azure AD) hakkında sık sorulan soruların yanıtlarını içerir. Daha fazla bilgi için bkz. [Azure Active Directory raporlaması](active-directory-reporting-azure-portal.md). 

**S: kullanıyorum https://graph.windows.net/&lt; Kiracı adı&gt;API'leri çekme Azure AD denetleme ve tümleşik uygulama kullanımı için Raporlar Raporlama bizim sistemlere program aracılığıyla /reports/ uç noktası. Ne için geçiş yaptığım?**

**Y:** aramak [API başvuru belgeleri](https://developer.microsoft.com/graph/) erişmek için yeni API'leri nasıl kullanabileceğinizi görmek için [etkinlik raporları](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal). Bu uç noktaya eski API uç aldığınız tüm verileri sağlayan iki rapor (Denetim ve oturum açma işlemleri) sahiptir. Bu yeni uç nokta da, uygulama kullanımı, Aygıt kullanımı ve kullanıcı oturum açma bilgilerini almak için kullanabileceğiniz Azure AD Premium lisansı oturum açma işlemleri raporu sahiptir.


--- 

**S: kullanıyorum https://graph.windows.net/&lt; Kiracı adı&gt;/reports/ endpoint Azure AD güvenlik raporları (algılama sızan kimlik veya anonim IP adreslerinden gerçekleştirilen oturum açma işlemleri gibi belirli türleri) raporlama sistemlerinin çekmek için API'ler Program aracılığıyla. Ne için geçiş yaptığım?**

**Y:** kullanabileceğiniz [kimlik koruması risk olaylarını API](active-directory-identityprotection-graph-getting-started.md) Microsoft Graph üzerinden erişim güvenlik algılamaları için. Bu yeni biçimin nasıl Gelişmiş filtreleme, alan seçimi ve daha fazla ile verileri sorgulayabilir büyük esneklik sağlar ve daha kolay tümleştirme Sıem'lerden ve diğer veri toplama araçları için bir türü içine risk olaylarını standartlaştıran. Verileri farklı bir biçimde olduğundan, yeni bir sorgu eski sorgularınızı yerine geçemez. Ancak, [Microsoft Graph yeni API kullanır](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/identityriskevent), bu tür API'leri O365 veya Azure AD olarak Microsoft standart olduğu. İş ya da geçerli MS Graph Yatırımlar veya Yardım genişletebilirsiniz gereken şekilde bu yeni standart platform geçişinizin başlayın.

--- 

**S: veri bekletme etkinlik günlükleri (Denetim ve oturum açma işlemleri) için Azure portalında nedir?** 

**Y:** bkz [toplanan verileri ne kadar süreyle depolanır?](active-directory-reporting-retention.md#q-for-how-long-is-the-collected-data-stored) bu yanıtını için.

--- 

**S: my görev tamamladığınızda etkinlik verileri görene kadar ne kadar sürer?**

**Y:** denetim etkinlik günlükleri 15 dakika arasında bir saat arasında bir gecikme vardır. Oturum açma etkinliği günlükleri 15 dakika arasında bazı kayıtları 2 saate kadar sürebilir.

---

**S: etkinlik oturum açma işlemlerine Azure Portalı'nı görmek veya API aracılığıyla veri almak için genel yönetici olmanız gerekiyor mu?**

**C:** Hayır. Olmalıdır bir **güvenlik okuyucu**, **güvenlik yönetici**, veya bir **genel yönetici** raporlama verilerini Azure portalında veya API'si aracılığıyla almak için.

---

**S: Azure portalı üzerinden Office 365 etkinlik günlüğü bilgilerini al?**

**Y:** olsa bile Office 365 etkinliği ve Azure AD etkinlik günlükleri paylaşım çok fazla dizin kaynak, Office 365 etkinlik günlükleri tam görünümünü istiyorsanız, gitmesini Office 365 etkinlik günlüğü bilgilerini almak için Office 365 Yönetim Merkezi için.

---


**S: Office 365 etkinlik günlükleri hakkında bilgi almak için hangi API'leri kullanıyor?**

**Y:** erişmek için Office 365 Yönetim API'ları kullanmak [Office 365 etkinlik günlükleri bir API aracılığıyla](https://msdn.microsoft.com/office-365/office-365-managment-apis-overview).

---

**S: kaç kayıt Azure portalından karşıdan yükleyebilir miyim?**

**Y:** en fazla 120 K kayıtları Azure Portalı'ndan yükleyebilirsiniz. Kayıtları göre sıralanır *en son* ve varsayılan olarak, en son 120 K kayıtları alın. 

---

**S: kaç kayıt ı etkinlikleri API sorgulama yapabilirsiniz?**

**Y:** 1 milyon kayıtlarını sorgulama yapabilirsiniz (çoğu tarafından kaydı sıralar top işleci kullanmazsanız son). "Top" işleci kullanırsanız, Yukarı 500 K kayıtlara sorgulayabilirsiniz. Örnek sorgular API kullanımı konusunda bulabilirsiniz [burada](active-directory-reporting-api-getting-started.md).

---

**S: premium lisansı nasıl sağlarım?**

**Y:** bkz [Azure Active Directory Premium ile çalışmaya başlama](active-directory-get-started-premium.md) bu yanıtını için.

---

**S: kadar kısa sürede ı etkinlikleri veri premium lisansı aldıktan sonra görmeniz gerekir?**

**Y:** ücretsiz bir lisans gibi etkinlikler veriler zaten sonra aynı verileri görebilirsiniz. Herhangi bir veri yoksa, bir veya iki gün sürer.

---

**S: Son ayın verilerini bir Azure AD premium lisansı aldıktan sonra görüyor musunuz?**

**Y:** (bir deneme sürümü dahil) bir Premium sürüme son geçtiyseniz, veri yukarı 7 gün için başlangıçta görebilirsiniz. Veri öğelerden 30 güne kadar görürsünüz.

---

**S: bir risk olayı kimlik koruması olmakla birlikte tüm oturum açma işlemleri karşılık gelen oturum içinde görüyorum değil. Bu beklenen bir durumdur?**

**Y:** etkileşimli ya da etkileşimli olmayan kimlik koruması risk tüm kimlik doğrulama akışlar için Evet, değerlendirir. Ancak, tüm oturum açma işlemleri yalnızca rapor yalnızca etkileşimli oturum açma işlemleri gösterir.

---

**S: Azure portalında "İçin risk bayrak eklenen kullanıcılar" rapor nasıl karşıdan yükleyebilir?**

**Y:** yükleme seçeneği *bayrak eklenen kullanıcılar için risk* rapor yakında eklenecek.

---

**S: neden bir oturum açma veya bir kullanıcı Azure portalında riskli bayrakla edildi nasıl biliyor musunuz?**

**Y:** Premium edition müşteriler öğrenin temel alınan risk olaylar hakkında daha fazla bilgi "İçin risk bayrak eklenen kullanıcılar" kullanıcı tıklayarak veya "riskli oturum açma işlemleri üzerinde" tıklatarak. Oturum açma işlemleri temel alınan risk olay bilgileri olmadan ve at-risk kullanıcıları görmek ücretsiz ve temel sürümü müşteriler alın.

---

**S: nasıl IP adresleri oturum açma işlemleri ve riskli oturum açma işlemleri raporu hesaplanır?**

**Y:** IP adresleri, bir IP adresi ve bilgisayarın adresine sahip fiziksel olarak bulunduğu arasında kesin bağlantı yoktur şekilde yayınlanır. Bu etkenler mobil sağlayıcıları ve VPN istemci aygıt gerçekte kullanıldığı gölgeden uzak IP adresleri merkezi havuzlarından genellikle çok veren gibi karmaşık. Yukarıda verilen, IP adresi fiziksel konuma dönüştürme izlemeleri, kayıt defteri verilerini, geriye doğru arama ve diğer bilgilere göre en iyi çaba olur. 

---

**S: "İle oturum açma algılandı ek risk" belirtmek risk olayı nedir?**

**Y:** ortamınızda tüm riskli oturum açma işlemleri bir anlayış vermek için "ek risk ile oturum açma algılandı" oturum açma işlemleri Azure AD Identity Protection abonelere dışlar algılamaları için yer tutucu görür.

---
