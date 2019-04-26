---
title: Azure Active Directory portalındaki denetim etkinliği raporları | Microsoft Docs
description: Azure Active Directory portalındaki denetim etkinliği raporlarına giriş
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: a1f93126-77d1-4345-ab7d-561066041161
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3da96c09026baff3965e0a90d1f461fd948a3a50
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60441146"
---
# <a name="audit-activity-reports-in-the-azure-active-directory-portal"></a>Azure Active Directory portalındaki denetim etkinliği raporları 

Azure Active Directory (Azure AD) raporları ile ortamınızın nasıl çalıştığını belirlemek için gereken bilgileri alabilirsiniz.

Raporlama mimarisi aşağıdaki bileşenlerden oluşur:

- **Etkinlik** 
    - **Oturum açma işlemleri** – [oturum açma işlemleri raporu](concept-sign-ins.md) oturum açma etkinlikleri yönetilen uygulamalar ve kullanıcı kullanımı hakkında bilgi sağlar.
    - **Denetim günlükleri** - Azure AD içindeki çeşitli özellikler tarafından yapılan tüm değişiklikler için günlükler aracılığıyla izlenebilirlik sağlar. Kullanıcılar, uygulamaları, gruplar, rolleri ve ilkeleri ekleyerek veya kaldırarak gibi Azure AD içindeki herhangi bir kaynakta yapılan değişiklikler denetim günlüklerinin örnekleridir.
- **Güvenlik** 
    - **Riskli oturum açma işlemleri** - [riskli oturum açma](concept-risky-sign-ins.md) bir kullanıcı hesabının meşru sahibi olmayan biri tarafından gerçekleştirilmiş olabilecek bir oturum açma girişiminin göstergesidir. 
    - **Risk için işaretlenen kullanıcılar** - [riskli kullanıcı](concept-user-at-risk.md) gizliliği bozulmuş olabilecek bir kullanıcı hesabının göstergesidir.

Bu makalede, Denetim raporuna genel bir bakış sağlar.
 
## <a name="who-can-access-the-data"></a>Verilere kimler erişebilir?

* Kullanıcıların **güvenlik yönetici**, **güvenlik okuyucusu**, **rapor okuyucu** veya **genel yönetici** rolleri
* Ayrıca, tüm kullanıcılar (Yönetici olmayanlar) kendi denetim etkinliklerini görebilir

## <a name="audit-logs"></a>Denetim günlükleri

Azure AD denetim günlükleri uyumluluk amacıyla sistem etkinliklerinin kayıtlarını sağlar. Denetim raporuna erişebilmek için seçin **denetim günlükleri** içinde **etkinlik** bölümünü **Azure Active Directory**. Kadar bu görevi tamamladıktan sonra portalda gösterilmesi denetim etkinlik verilerini uzun sürebilir denetim günlüklerini saat, en fazla bir gecikme olabileceğini unutmayın.

![Denetim günlükleri](./media/concept-audit-logs/61.png "Denetim günlükleri")

Denetim günlüklerinin aşağıdakileri gösteren bir varsayılan liste görünümü vardır:

- Olayın tarihi ve saati
- oluşum oturum hizmeti
- Kategori ve etkinlik adını (*ne*) 
- Etkinlik (başarı veya başarısızlık) durumu
- Hedef
- Başlatıcısı / aktörü (kim) bir etkinlik

![Denetim günlükleri](./media/concept-audit-logs/listview.png "Denetim günlükleri")

Araç çubuğunda **Sütunlar**’a tıklayarak liste görünümünü özelleştirebilirsiniz.

![Denetim günlükleri](./media/concept-audit-logs/columns.png "Denetim günlükleri")

Bu sayede ek alanları görüntüleyebilir ya da zaten görüntülenen alanları kaldırabilirsiniz.

![Denetim günlükleri](./media/concept-audit-logs/columnselect.png "Denetim günlükleri")

Daha ayrıntılı bilgi almak için liste görünümünde bir öğe seçin.

![Denetim günlükleri](./media/concept-audit-logs/details.png "Denetim günlükleri")


## <a name="filtering-audit-logs"></a>Denetim günlüklerini filtreleme

Aşağıdaki alanlarda denetim verilerini filtreleyebilirsiniz:

- Hizmet
- Kategori
- Etkinlik
- Durum
- Hedef
- Başlatan (Aktör)
- Tarih aralığı

![Denetim günlükleri](./media/concept-audit-logs/filter.png "Denetim günlükleri")

**Hizmet** filtresi aşağıdaki hizmetlerin açılan listeden seçim olanak tanır:

- Tümü
- Erişim Gözden Geçirmeleri
- Hesap Sağlama 
- Uygulama SSO
- Kimlik Doğrulaması Yöntemleri
- B2C
- Koşullu Erişim
- Çekirdek Dizin
- Yetkilendirme Yönetimi
- Kimlik Koruması
- Davetli Kullanıcılar
- PIM
- Self Servis Grup Yönetimi
- Self Servis Passord Yönetimi
- Kullanım Koşulları

**Kategori** filtresi, aşağıdaki filtrelerden birini seçmenize olanak tanır:

- Tümü
- AdministrativeUnit
- ApplicationManagement
- Authentication
- Yetkilendirme
- İletişim
- Cihaz
- DeviceConfiguration
- DirectoryManagement
- EntitlementManagement
- GroupManagement
- Diğer
- İlke
- ResourceManagement
- RoleManagement
- UserManagement

**Etkinlik** filtresi, yaptığınız kategori ve etkinlik kaynağı türü seçimine dayanır. Görmek istediğiniz belirli bir etkinliği ya da tüm etkinlikleri seçebilirsiniz. 

Graph API'si (https://graph.windows.net/$tenantdomain/activities/auditActivityTypes?api-version=beta; burada $tenantdomain = etki alanınızın adıdır) kullanarak tüm Denetim Etkinliklerinin listesini alabilir veya [denetim raporu olayları](reference-audit-activities.md) makalesine bakabilirsiniz.

**Durumu** filtre filtrelemenize olanak sağlayan bir denetim işlemin durumu temelinde. Durumu aşağıdakilerden biri olabilir:

- Tümü
- Başarılı
- Hata

**Hedef** filtre adı veya kullanıcı asıl adı (UPN) göre belirli bir hedef için arama olanak tanır. UPN ve hedef adı büyük/küçük harf duyarlıdır. 

**Başlatan** filtre, bir aktörün adını ya da evrensel asıl adını (UPN) tanımlamanıza imkan tanır. UPN ve adı büyük/küçük harf duyarlıdır.

**Tarih aralığı** filtresi, döndürülen veriler için bir zaman çerçevesi tanımlamanıza olanak.  
Olası değerler şunlardır:

- 1 ay
- 7 gün
- 24 saat
- Özel

Özel bir zaman çerçevesi seçerken başlangıç ve bitiş zamanını yapılandırabilirsiniz.

Ayrıca en fazla 250.000 kayıtları, filtrelenmiş verileri seçerek indirmeyi seçebilir **indirme** düğmesi. CSV veya JSON biçiminde günlükleri indirmek tercih edebilirsiniz. İndirebilirsiniz kayıt sayısı tarafından sınırlı [Azure Active Directory rapor saklama ilkeleri](reference-reports-data-retention.md).

![Denetim günlükleri](./media/concept-audit-logs/download.png "Denetim günlükleri")

## <a name="audit-logs-shortcuts"></a>Denetim günlükleri kısayolları

Azure portalı, **Azure Active Directory**’ye ek olarak verileri denetlemeniz için fazladan iki giriş noktası sağlar:

- Kullanıcılar ve gruplar
- Kurumsal uygulamalar

### <a name="users-and-groups-audit-logs"></a>Kullanıcı ve gruplara yönelik denetim günlükleri

Kullanıcı ve grup tabanlı denetim raporları ile aşağıdakiler gibi soruların yanıtlarını alabilirsiniz:

- Kullanıcılara hangi tür güncelleştirmeler uygulanmış?

- Kaç adet kullanıcı değiştirildi?

- Kaç adet parola değiştirildi?

- Bir yönetici bir dizinde neler yaptı?

- Eklenmiş olan gruplar hangileridir?

- Üyelik değişiklikleri olan gruplar var mı?

- Grubun sahipleri değişti mi?

- Bir grup veya kullanıcıya hangi lisanslar atanmış?

Kullanıcılarla ilgili denetim verilerini gözden geçirmek istiyorsanız altında filtrelenmiş bir görünüm bulabilirsiniz **denetim günlükleri** içinde **etkinlik** bölümünü **kullanıcılar** sekmesi. Bu giriş noktasında, **UserManagement** olarak seçilmiş kategorisi.

![Denetim günlükleri](./media/concept-audit-logs/users.png "Denetim günlükleri")

Grupları ile ilgili denetim verilerini gözden geçirmek istiyorsanız altında filtrelenmiş bir görünüm bulabilirsiniz **denetim günlükleri** içinde **etkinlik** bölümünü **grupları** sekmesi. Bu giriş noktasında, **GroupManagement** olarak seçilmiş kategorisi.

![Denetim günlükleri](./media/concept-audit-logs/groups.png "Denetim günlükleri")

### <a name="enterprise-applications-audit-logs"></a>Kurumsal uygulamaların denetim günlükleri

Uygulama tabanlı denetim raporları ile aşağıdakiler gibi soruların yanıtlarını alabilirsiniz:

* Hangi uygulamaların eklenen veya güncelleştirilen?
* Hangi uygulamaların kaldırdınız?
* Bir uygulama için bir hizmet sorumlusu değişti mi?
* Uygulamaların adları değiştirildi mi?
* Belirli bir uygulama için kim onay verdi?

Uygulamalarınızla ilgili denetim verilerini gözden geçirmek istiyorsanız, altında filtrelenmiş bir görünüm bulabilirsiniz **denetim günlükleri** içinde **etkinlik** bölümünü **kurumsal uygulamalar** Dikey pencere. Bu giriş noktasında, **kurumsal uygulamalar** olarak seçilmiş **uygulama türü**.

![Denetim günlükleri](./media/concept-audit-logs/enterpriseapplications.png "Denetim günlükleri")

## <a name="office-365-activity-logs"></a>Office 365 etkinlik günlükleri

Office 365 etkinlik günlüklerini görüntüleyebilirsiniz [Microsoft 365 Yönetim merkezini](https://docs.microsoft.com/office365/admin/admin-overview/about-the-admin-center). Olsa da Microsoft 365 Yönetim Merkezi yalnızca Office 365 ve Azure AD etkinlik günlükleri dizin kaynaklarının çoğunu paylaşır, Office 365 etkinlik günlüklerinin tam bir görünümünü sağlar. 

Office 365 etkinlik günlüklerini programlı olarak kullanarak da erişebilirsiniz [Office 365 Yönetim API'leri](https://docs.microsoft.com/office/office-365-management-api/office-365-management-apis-overview).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD denetim etkinliği başvurusu](reference-audit-activities.md)
- [Azure AD raporlar bekletme başvurusu](reference-reports-data-retention.md)
- [Azure AD günlük gecikmeleri başvurusu](reference-reports-latencies.md)
