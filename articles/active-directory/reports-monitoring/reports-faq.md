---
title: Azure Active Directory raporlama hakkında SSS | Microsoft Docs
description: Azure Active Directory raporlama hakkında SSS.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
ms.assetid: 534da0b1-7858-4167-9986-7a62fbd10439
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: report-monitor
ms.date: 05/10/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: f1683321e23eff82e73dc9bb44941fc390633b8c
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2018
ms.locfileid: "42056103"
---
# <a name="azure-active-directory-reporting-faq"></a>Azure Active Directory raporlama hakkında SSS

Bu makale, Azure Active Directory (Azure AD) hakkında sık sorulan sorular raporlama yanıtlarını içerir. Daha fazla bilgi için bkz. [Azure Active Directory raporlaması](overview-reports.md). 

## <a name="getting-started"></a>Başlarken 

**S: kullanıyorum https://graph.windows.net/&lt; Kiracı adı&gt;/reports/ uç nokta API'leri çekme Azure AD denetim ve tümleşik uygulama kullanım için raporlama sistemlerimizde programlı olarak bildirir. Ne için geçiş miyim?**

**Y:** aramak [API başvuru belgeleri](https://developer.microsoft.com/graph/) erişmek için yeni API'leri nasıl kullanabileceğinizi görmek için [Etkinlik raporlarını](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal). Bu uç nokta eski API uç noktası ' var olan tüm verileri sağlayan iki rapor (Denetim ve oturum açma) sahiptir. Bu yeni uç nokta da, uygulama kullanımını, Aygıt kullanımı ve kullanıcı oturum açma bilgilerini almak için kullanabileceğiniz bir Azure AD Premium lisansına sahip bir oturum açma işlemleri raporu vardır.

--- 

**S: kullanıyorum https://graph.windows.net/&lt; Kiracı adı&gt;/reports/ uç nokta API'leri, Azure AD güvenlik raporları (belirli tür algılamadan dışlanmasını, sızan kimlik bilgileri veya anonim IP adreslerinden oturum açma işlemleri gibi) raporlama sistemlerimizde çekmek için Program aracılığıyla. Ne için geçiş miyim?**

**Y:** kullanabileceğiniz [kimlik koruması risk olayları API](../identity-protection/graph-get-started.md) Microsoft Graph üzerinden erişim güvenlik algılamaları için. Bu yeni biçim, Gelişmiş filtreleme, alan seçimi ve diğer verileri nasıl Sorgulayabileceğiniz büyük esneklik sağlar ve daha kolay tümleştirmeye Sıem'lerden ve diğer veri toplama araçları için bir tür içinde risk olayları standartlaştırır. Verileri farklı bir biçimde olduğundan, yeni bir sorgu için eski sorgularınızı yerine geçemez. Ancak, [Microsoft Graph yeni API'yi kullanan](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/identityriskevent), olduğu gibi API'ler olarak O365 veya Azure AD için Microsoft standart. MS Graph mevcut yatırımlarınızdan veya Yardım ya da genişletebilirsiniz iş gerekli şekilde bu yeni standart platformu geçişinizi başlayın.

--- 

**S: bir premium lisansı nasıl alabilirim?**

**Y:** bkz [Azure Active Directory Premium ile çalışmaya başlama](../fundamentals/active-directory-get-started-premium.md) Bu soruya yanıt.

---

**S: ne kadar kısa sürede etkinliği verileri bir premium lisansı aldıktan sonra görüyorum?**

**Y:** ücretsiz lisans gibi etkinlikler veriler zaten varsa aynı verileri görebilirsiniz. Ardından herhangi bir veri yoksa, bir veya iki gün sürebilir.

---

**S: son aya ilişkin verileri bir Azure AD premium lisansı aldıktan sonra bkz?**

**Y:** (deneme sürümü dahil) bir Premium sürümü için yakın zamanda çalıştıysanız, veri yedekleme 7 gün için başlangıçta görebilirsiniz. Verileri toplanır, 30 güne kadar görürsünüz.

---

**Azure portalında etkinlik oturum açma işlemleri görmek veya API üzerinden veri almak için genel yönetici olmanız gerekiyor s:?**

**C:** Hayır. Olmalıdır bir **güvenlik okuyucusu**, **Güvenlik Yöneticisi**, veya bir **genel yönetici** raporlama API'si aracılığıyla veya Azure portalında verilerini almak için.

---


## <a name="activity-logs"></a>Etkinlik günlükleri


**S: veri saklama için etkinlik günlüklerini (Denetim ve oturum açma) Azure portalında nedir?** 

**Y:** bkz [toplanan verileri ne kadar süreyle saklanır?](reference-reports-data-retention.md#q-for-how-long-is-the-collected-data-stored) Bu soruya yanıt.

--- 

**S: my görevi tamamladığınızda etkinlik verileri görene kadar ne kadar sürer?**

**Y:** denetim etkinlik günlüklerini 15 dakika veya saat arasında bir gecikme vardır. Oturum açma etkinlik günlüklerini 15 dakika veya bazı kayıtları 2 saate kadar sürebilir.

---


**Office 365 etkinlik günlüğü bilgilerini Azure portalı üzerinden almam miyim?**

**Y:** olsa da Office 365 ve Azure AD etkinlik günlükleri dizin kaynaklarının çoğunu paylaşır, Office 365 etkinlik günlüklerinin tam bir görünümünü istiyorsanız, gitmesini Office 365 etkinlik günlüğü bilgilerini almak için Office 365 Yönetim Merkezi için.

---


**S: hangi API'ler Office 365 etkinlik günlükleri hakkında daha fazla bilgi almak için kullanabilirim?**

**Y:** erişmek için Office 365 Yönetim API'leri kullanan [Office 365 etkinlik günlükleri bir API aracılığıyla](https://msdn.microsoft.com/office-365/office-365-managment-apis-overview).

---

**S: kaç kayıtlarını Azure portalından indirebilirim?**

**Y:** en fazla 5000 kayıtlarını Azure portalından indirebilirsiniz. Kayıtları göre sıralanır *en son* ve varsayılan olarak, en son 5000 kayıtları alın.

---

## <a name="risky-sign-ins"></a>Riskli oturum açma işlemleri

**S: bir risk olayını kimlik koruması yoktur ama tüm oturum açma işlemleri ilgili oturum içindeki görüyorum değil. Bu beklenen bir durumdur?**

**Y:** etkileşimli veya etkileşimli olmayan kimlik koruması risk tüm kimlik doğrulama akışları için Evet, değerlendirir. Ancak, tüm oturum açma işlemleri yalnızca rapor yalnızca etkileşimli oturum açma işlemleri gösterir.

---

**S: Azure portalında "Riskli oldukları belirlenen kullanıcılar" raporu indirebilir miyim?**

**Y:** yükleme seçeneği *risk için işaretlenen kullanıcılar* raporu yakında eklenecektir.

---

**S: neden bir oturum açma veya bir kullanıcıya Azure portalında riskli bayrak eklenmiş olan biliyor musunuz?**

**Y:** Premium edition müşterileri bulabilir temel risk olayları hakkında daha fazla "Riskli oldukları belirlenen kullanıcılar" kullanıcının tıklayarak veya "riskli oturum açma işlemleri üzerinde" tıklayarak. Ücretsiz ve temel edition müşterileri, temel alınan risk olayı bilgilerini olmadan oturum açma ve risk altında bulunan kullanıcıları görmek alın.

---

**S: nasıl IP adresleri oturum açma ve riskli oturum açma işlemleri raporu hesaplanır?**

**Y:** IP adresleri, bir IP adresi ve bilgisayarın bu adresine sahip fiziksel olarak bulunduğu yeri arasında kesin bağlantı yoktur şekilde verilir. Bu mobil sağlayıcıları ve VPN istemci cihaz gerçekten kullanıldığı gölgeden uzak IP adresleri merkezi havuzlarından genellikle çok verme gibi faktörlere tarafından karmaşık. Yukarıdaki göz önünde bulundurulduğunda, IP adresi fiziksel bir konuma dönüştürme izlemeleri, kayıt defteri verisi, geriye doğru arama ve diğer bilgilere göre en iyi çaba ilkesi var. 

---

**S: "Oturum açma algılandı ek risk içeren" geldiğiniz risk olayı nedir?**

**Y:** ortamınızda tüm riskli oturum açmaların bir anlayış vermek için "oturum açma ile ek risk algılandı" oturum açma işlemleri için Azure AD kimlik koruması aboneleri için özel olan algılama için yer tutucu işlevini görür.

---

**S: "Oturum açma algılandı ek risk içeren" geldiğiniz risk olayı nedir?**

**Y:** ortamınızda tüm riskli oturum açmaların bir anlayış vermek için "oturum açma ile ek risk algılandı" oturum açma işlemleri için Azure AD kimlik koruması aboneleri için özel olan algılama için yer tutucu işlevini görür.

---

## <a name="conditional-access"></a>Koşullu erişim

**S: Bu özellikle yenilikler nelerdir?**

**Y:** müşteriler artık tüm oturum açma işlemleri raporu aracılığıyla koşullu erişim ilkeleri sorun giderme. Müşteriler, oturum açma ve sonucu her ilke için uygulanan tüm ilkeler ayrıntılarını inceleyin ve koşullu erişim durumunu gözden geçirebilirsiniz.

**S: nasıl başlayabilirim?**

**Y:** kullanmaya başlamak için:
    * Oturum açma işlemleri raporu gidin [Azure portalında](https://portal.azure.com). 
    * Sorun giderme istediğiniz oturum üzerinde tıklayın.
    * Gidin **koşullu erişim** sekmesi. Burada, oturum açma ve her ilke için sonucu etkilenen tüm ilkelerde görüntüleyebilirsiniz. 
    
**S: koşullu erişim durumu için tüm olası değerler nelerdir?**

**Y:** koşullu erişim durumu, aşağıdaki değerleri içerebilir:
    * **Uygulanmamış**: Bu kullanıcı ve uygulamanın kapsamdaki bir CA ilke olduğunu gösterir. 
    * **Başarı**: Bu kullanıcı ve uygulamanın kapsamında olan bir CA ilke vardı ve CA ilkeleri başarıyla memnun anlamına gelir. 
    * **Hata**: Bu kullanıcı ve uygulamanın kapsamında olan bir CA ilke vardı ve CA ilkeleri karşılanmadı anlamına gelir. 
    
**S: koşullu erişim ilkesi sonucu için tüm olası değerler nelerdir?**

**Y:** bir koşullu erişim ilkesi, aşağıdaki sonuçları olabilir:
    * **Başarı**: ilkeyi başarıyla memnun.
    * **Hata**: ilke karşılanmadı.
    * **Uygulanmamış**: ilke koşullarını karşılamıyor olmasından kaynaklanabilir.
    * **Etkin**: Bu ilke devre dışı durumda kaynaklanır. 
    
**S: rapordaki tüm oturum açma ilkesi adı CA ilkesi adı eşleşmiyor. Neden?**

**Y:** rapordaki tüm oturum açma ilkesi adı oturum açma zaman CA ilke adına bağlıdır. İlke adına daha sonra diğer bir deyişle, oturum açma işleminden sonra güncelleştirdiyseniz bu CA ilkesi adı ile tutarsız olabilir.