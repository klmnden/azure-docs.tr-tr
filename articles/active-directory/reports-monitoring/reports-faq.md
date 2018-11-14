---
title: Azure Active Directory rapor ile ilgili SSS | Microsoft Docs
description: Azure Active Directory raporlarını geçici quesitons sık sorulan.
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
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 5cbf0895274672c053158cf07acb344908b37831
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51623477"
---
# <a name="frequently-asked-questions-around-azure-active-directory-reports"></a>Sık sorulan sorular Azure Active Directory raporlarını geçici bir çözüm

Bu makale, Azure Active Directory (Azure AD) hakkında sık sorulan sorular raporlama yanıtlarını içerir. Daha fazla bilgi için bkz. [Azure Active Directory raporlaması](overview-reports.md). 

## <a name="getting-started"></a>Başlarken 

**S: kullanmakta https://graph.windows.net/&lt; Kiracı adı&gt;/reports/ uç nokta API'leri çekme Azure AD denetim ve tümleşik uygulama kullanım için raporlama sistemlerimizde programlı olarak bildirir. Ne için geçiş miyim?**

**Y:** aramak [API Başvurusu](https://developer.microsoft.com/graph/) öğrenmek için [etkinliği raporlarına erişmek için API'leri kullanan](concept-reporting-api.md). Bu uç noktaya sahip iki rapor (**denetim** ve **oturum açma işlemleri**) eski API uç noktası aldığınız tüm verileri sağlar. Bu yeni uç nokta da, uygulama kullanımını, Aygıt kullanımı ve kullanıcı oturum açma bilgilerini almak için kullanabileceğiniz bir Azure AD Premium lisansına sahip bir oturum açma işlemleri raporu vardır.

--- 

**S: kullanmakta https://graph.windows.net/&lt; Kiracı adı&gt;/reports/ uç nokta API'leri, Azure AD güvenlik raporları (belirli tür algılamadan dışlanmasını, sızan kimlik bilgileri veya anonim IP adreslerinden oturum açma işlemleri gibi) raporlama sistemlerimizde çekmek için Program aracılığıyla. Ne için geçiş miyim?**

**Y:** kullanabileceğiniz [kimlik koruması risk olayları API](../identity-protection/graph-get-started.md) Microsoft Graph üzerinden erişim güvenlik algılamaları için. Bu yeni biçim, Gelişmiş filtreleme, alan seçimi ve diğer verileri nasıl Sorgulayabileceğiniz büyük esneklik sağlar ve daha kolay tümleştirmeye Sıem'lerden ve diğer veri toplama araçları için bir tür içinde risk olayları standartlaştırır. Verileri farklı bir biçimde olduğundan, yeni bir sorgu için eski sorgularınızı yerine geçemez. Ancak, [Microsoft Graph yeni API'yi kullanan](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/identityriskevent), olduğu gibi API'ler olarak O365 veya Azure AD için Microsoft standart. MS Graph mevcut yatırımlarınızdan veya Yardım ya da genişletebilirsiniz iş gerekli şekilde bu yeni standart platformu geçişinizi başlayın.

--- 

**S: bir premium lisansı nasıl alabilirim?**

**Y:** bkz [Azure Active Directory Premium ile çalışmaya başlama](../fundamentals/active-directory-get-started-premium.md) , Azure Active Directory sürümünü yükseltmek için.

---

**S: ne kadar kısa sürede etkinliği verileri bir premium lisansı aldıktan sonra görüyorum?**

**Y:** ücretsiz lisans gibi etkinlikler veriler zaten varsa hemen görebilirsiniz. Herhangi bir veri yoksa, veriler raporlarda görünmesi için bir veya iki gün sürebilir.

---

**Son aya ilişkin verileri bir Azure AD premium lisansı aldıktan sonra görebilir miyim?**

**Y:** (deneme sürümü dahil) bir Premium sürümü için yakın zamanda çalıştıysanız, veri yedekleme 7 gün için başlangıçta görebilirsiniz. Verileri toplanır, son 30 güne ilişkin verileri görebilirsiniz.

---

**S: Azure portalında etkinlik oturum açma işlemleri görmek veya API üzerinden veri almak için genel yönetici olmanız gerekiyor?**

**Y:** olduğunuz Hayır, aynı zamanda raporlama verilerini portalı veya API üzerinden erişebilirsiniz bir **güvenlik okuyucusu** veya **Güvenlik Yöneticisi** Kiracı. Elbette, **genel Yöneticiler** de bu verilere erişebilir.

---


## <a name="activity-logs"></a>Etkinlik günlükleri


**S: veri saklama için etkinlik günlüklerini (Denetim ve oturum açma) Azure portalında nedir?** 

**Y:** veri saklama süresi etkinlik günlükleri için aşağıdaki tabloda listelenmiştir. Daha fazla bilgi için [veri bekletme ilkeleri için Azure AD raporlar](reference-reports-data-retention.md).

| Rapor                 | Azure AD Ücretsiz | Azure AD Premium P1 | Azure AD Premium P2 |
| :--                    | :--           | :--                 | :--                 |
| Denetim günlükleri             | 7 gün        | 30 gün             | 30 gün             |
| Oturum açma işlemleri               | Yok           | 30 gün             | 30 gün             |
| Azure MFA kullanımı        | 30 gün       | 30 gün             | 30 gün             |

--- 

**S: my görevi tamamladığınızda etkinlik verileri görene kadar ne kadar sürer?**

**Y:** denetim günlüklerini 15 dakika veya saat arasında bir gecikme vardır. Oturum açma etkinlik günlüklerini 15 dakika veya bazı kayıtları 2 saate kadar sürebilir.

---

**Office 365 etkinlik günlüğü bilgilerini Azure portalı üzerinden almam miyim?**

**Y:** olsa da Office 365 ve Azure AD etkinlik günlükleri dizin kaynaklarının çoğunu paylaşır, Office 365 etkinlik günlüklerinin tam bir görünümünü istiyorsanız, gitmesini Office 365 etkinlik günlüğü bilgilerini almak için Office 365 Yönetim Merkezi için.

---

**S: hangi API'ler Office 365 etkinlik günlükleri hakkında daha fazla bilgi almak için kullanabilirim?**

**Y:** kullanım [Office 365 Yönetim API'leri](https://msdn.microsoft.com/office-365/office-365-managment-apis-overview) bir API aracılığıyla Office 365 etkinlik günlüklerine erişmek için.

---

**S: kaç kayıtlarını Azure portalından indirebilirim?**

**Y:** en fazla 5000 kayıtlarını Azure portalından indirebilirsiniz. Kayıtları göre sıralanır *en son* ve varsayılan olarak, en son 5000 kayıtları alın.

---

## <a name="risky-sign-ins"></a>Riskli oturum açma işlemleri

**S: bir risk olayını kimlik koruması yoktur ama karşılık gelen oturum açma, oturum açma işlemleri raporu görüyorum değil. Bu beklenen bir durumdur?**

**Y:** etkileşimli veya etkileşimli olmayan kimlik koruması risk tüm kimlik doğrulama akışları için Evet, değerlendirir. Ancak, tüm oturum açma işlemleri yalnızca rapor yalnızca etkileşimli oturum açma işlemleri gösterir.

---

**S: neden bir oturum açma veya bir kullanıcıya Azure portalında riskli bayrak eklenmiş olan biliyor musunuz?**

**Y:** varsa bir **Azure AD Premium** abonelik edinebilirsiniz temel risk olayları hakkında daha fazla kullanıcı seçerek **risk için işaretlenen kullanıcılar** veya bir kayıt seçerek **Riskli oturum açma işlemleri** rapor. Varsa bir **ücretsiz** veya **temel** abonelik olduktan sonra kullanıcılar, risk ve riskli oturum açma işlemleri raporları görüntüleyebilir, ancak temel alınan risk olayı bilgilerini göremez.

---

**S: nasıl IP adresleri oturum açma ve riskli oturum açma işlemleri raporu hesaplanır?**

**Y:** IP adresleri, bir IP adresi ve bilgisayarın bu adresine sahip fiziksel olarak bulunduğu yeri arasında kesin bağlantı yoktur şekilde verilir. IP adreslerini eşleme daha fazla mobil sağlayıcıları ve VPN istemci cihaz gerçekten kullanıldığı gölgeden uzak IP adresleri merkezi havuzlarından genellikle çok verme gibi faktörlere tarafından karmaşık. Şu anda Azure AD raporlarında, IP adresi fiziksel bir konuma dönüştürme izlemeleri, kayıt defteri verisi, geriye doğru arama ve diğer bilgilere göre en iyi çaba ilkesi var. 

---

**S: "Oturum açma algılandı ek risk içeren" geldiğiniz risk olayı nedir?**

**Y:** ortamınızda tüm riskli oturum açma hakkında bilgi vermek için "oturum açma ile ek risk algılandı" oturum açma işlemleri için Azure AD kimlik koruması aboneleri için özel olan algılama için yer tutucu işlevini görür.

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

**S: Alanım oturum açma bir koşullu erişim ilkesi nedeniyle engellendi, ancak oturum açma etkinliği raporunu, oturum açma başarılı olduğunu gösterir. Neden?**

**Y:** koşullu erişim uygulandığında şu anda oturum açma raporu Exchange ActiveSync senaryoları için daha doğru sonuçlar gösterilmeyebilir. Bir oturum açma başarılı oturum açma sonuç raporunda gösterdiği durumlarda durumları olabilir, ancak oturum açma aslında bir koşullu erişim ilkesi nedeniyle başarısız oldu. 
