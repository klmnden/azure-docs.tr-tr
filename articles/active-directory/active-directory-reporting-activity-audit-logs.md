---
title: "Azure Active Directory portalındaki denetim etkinliği raporları | Microsoft Docs"
description: "Azure Active Directory portalındaki denetim etkinliği raporlarına giriş"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: a1f93126-77d1-4345-ab7d-561066041161
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.translationtype: Human Translation
ms.sourcegitcommit: c785ad8dbfa427d69501f5f142ef40a2d3530f9e
ms.openlocfilehash: d8c49272789e7d33c6f0684875765a1ecea5a2ff
ms.contentlocale: tr-tr
ms.lasthandoff: 05/26/2017

---
# Azure Active Directory portalındaki denetim etkinliği raporları
<a id="audit-activity-reports-in-the-azure-active-directory-portal" class="xliff"></a> 

Azure Active Directory’deki (Azure AD) raporlama özelliğiyle ortamınızın nasıl çalıştığını belirlemek için gereken bilgileri alabilirsiniz.

Azure AD'nin raporlama mimarisi aşağıdaki bileşenlerden oluşur:

- **Etkinlik** 
    - **Oturum açma etkinlikleri**: Yönetilen uygulamaların kullanımı ve kullanıcıların oturum açma etkinlikleri hakkında bilgiler
    - **Denetim günlükleri**: Kullanıcılar ve grup yönetimi, yönetilen uygulamalarınız ve dizin etkinlikleriniz hakkında sistem etkinliği bilgileri.
- **Güvenlik** 
    - **Riskli oturum açma işlemleri** - Riskli oturum açma işlemi bir kullanıcı hesabının meşru sahibi olmayan bir kişi tarafından gerçekleştirilmiş olabilecek oturum açma girişiminin göstergesidir. Daha fazla bilgi için bkz. Riskli oturum açma işlemleri.
    - **Riskli oldukları belirlenen kullanıcılar** - Riskli kullanıcı, güvenliği tehlikeye girmiş olabilecek bir kullanıcı hesabının göstergesidir. Daha fazla bilgi için bkz. Riskli oldukları belirlenen kullanıcılar.

Bu konu başlığı denetim etkinliklerine genel bakış sunmaktadır.
 
## Verilere kimler erişebilir?
<a id="who-can-access-the-data" class="xliff"></a>
* Güvenlik Yöneticisi veya Güvenlik Okuyucusu rolündeki kullanıcılar
* Genel Yöneticiler
* Bireysel kullanıcılar (yönetici olmayanlar) kendi etkinliklerini görebilir


## Denetim günlükleri
<a id="audit-logs" class="xliff"></a>

Azure Active Directory'deki denetim günlükleri uyumluluk amacıyla sistem etkinliklerinin kayıtlarını sağlar.  
Tüm denetim verilerine ilk giriş noktanız, **Azure Active Directory**’nin **Etkinlik** bölümünde bulunan **Denetim günlükleri** kısmıdır.

![Denetim günlükleri](./media/active-directory-reporting-activity-audit-logs/61.png "Denetim günlükleri")

Denetim günlüklerinin aşağıdakileri gösteren bir varsayılan liste görünümü vardır:

- Olayın tarihi ve saati
- Bir etkinliğin başlatıcısı/aktörü (*kim*) 
- Etkinlik (*ne*) 
- Hedef

![Denetim günlükleri](./media/active-directory-reporting-activity-audit-logs/18.png "Denetim günlükleri")

Araç çubuğunda **Sütunlar**’a tıklayarak liste görünümünü özelleştirebilirsiniz.

![Denetim günlükleri](./media/active-directory-reporting-activity-audit-logs/19.png "Denetim günlükleri")

Bu sayede ek alanları görüntüleyebilir ya da zaten görüntülenen alanları kaldırabilirsiniz.

![Denetim günlükleri](./media/active-directory-reporting-activity-audit-logs/21.png "Denetim günlükleri")


Liste görünümündeki bir öğeye tıklayarak bu öğe hakkında mevcut olan tüm ayrıntıları öğrenebilirsiniz.

![Denetim günlükleri](./media/active-directory-reporting-activity-audit-logs/22.png "Denetim günlükleri")


## Denetim günlüklerini filtreleme
<a id="filtering-audit-logs" class="xliff"></a>

Raporlanan verileri istediğiniz düzeye gelecek şekilde daraltmak için, aşağıdaki alanları kullanarak denetim verilerini filtreleyebilirsiniz:

- Tarih aralığı
- Başlatan (Aktör)
- Kategori
- Etkinlik kaynak türü
- Etkinlik

![Denetim günlükleri](./media/active-directory-reporting-activity-audit-logs/23.png "Denetim günlükleri")


**Tarih aralığı** filtresi, döndürülen veriler için bir zaman çerçevesi tanımlamanıza olanak sağlar.  
Olası değerler şunlardır:

- 1 ay
- 7 gün
- 24 saat
- Özel

Özel bir zaman çerçevesi seçerken başlangıç ve bitiş zamanını yapılandırabilirsiniz.

**Başlatan** filtresi, bir aktörün adını ya da evrensel asıl adını (UPN) tanımlamanıza imkan tanır.

**Kategori** filtresi, aşağıdaki filtrelerden birini seçmenize imkan tanır:

- Tümü
- Çekirdek kategori
- Çekirdek dizin
- Self servis parola yönetimi
- Self servis grup yönetimi
- Hesap sağlama - Otomatik parola geçişi
- Davetli kullanıcılar
- MIM hizmeti
- Kimlik Koruması
- B2C

**Etkinlik kaynağı türü** filtresi, aşağıdaki filtrelerden birini seçmenize imkan tanır:

- Tümü 
- Grup
- Dizin
- Kullanıcı
- Uygulama
- İlke
- Cihaz
- Diğer

**Etkinlik kaynağı türü** olarak **Grup**’u seçtiğinizde, bir **Kaynak** sağlamanıza da imkan tanıyan ek bir filtre kategorisine sahip olursunuz:

- Azure AD
- O365


**Etkinlik** filtresi, yaptığınız kategori ve Etkinlik kaynağı türü seçimine bağlıdır. Görmek istediğiniz belirli bir etkinliği ya da tüm etkinlikleri seçebilirsiniz. 

Grafik API'si ($tenantdomain = etki alanı adınız olacak şekilde https://graph.windows.net/$tenantdomain/activities/auditActivityTypes?api-version=beta) kullanarak tüm Denetim Etkinliklerinin listesini alabilir veya [denetim raporu olayları](active-directory-reporting-audit-events.md#list-of-audit-report-events) makalesine bakabilirsiniz.


## Denetim günlükleri kısayolları
<a id="audit-logs-shortcuts" class="xliff"></a>

Azure portalı, **Azure Active Directory**’ye ek olarak verileri denetlemeniz için fazladan iki giriş noktası sağlar:

- Kullanıcılar ve gruplar
- Kurumsal uygulamalar

### Kullanıcı ve gruplara yönelik denetim günlükleri
<a id="users-and-groups-audit-logs" class="xliff"></a>

Kullanıcı ve grup tabanlı denetim raporları ile aşağıdakiler gibi soruların yanıtlarını alabilirsiniz:

- Kullanıcılara hangi tür güncelleştirmeler uygulanmış?

- Kaç adet kullanıcı değiştirildi?

- Kaç adet parola değiştirildi?

- Bir yönetici bir dizinde neler yaptı?

- Eklenmiş olan gruplar hangileridir?

- Üyelik değişiklikleri olan gruplar var mı?

- Grubun sahipleri değişti mi?

- Bir grup veya kullanıcıya hangi lisanslar atanmış?

Yalnızca kullanıcı ve gruplarla ilgili denetim verilerini gözden geçirmek istiyorsanız, **Kullanıcılar ve Gruplar**’ın **Etkinlik** bölümündeki **Denetim günlükleri** altında filtrelenmiş bir görünüm bulabilirsiniz. Bu giriş noktasında, **Etkinlik Kaynağı Türü** olarak **Kullanıcılar ve gruplar** önceden seçilidir.

![Denetim günlükleri](./media/active-directory-reporting-activity-audit-logs/93.png "Denetim günlükleri")

### Kurumsal uygulamaların denetim günlükleri
<a id="enterprise-applications-audit-logs" class="xliff"></a>

Uygulama tabanlı denetim raporları ile aşağıdakiler gibi soruların yanıtlarını alabilirsiniz:

* Eklenmiş veya güncelleştirilmiş olan uygulamalar hangileridir?
* Kaldırılmış olan uygulamalar hangileridir?
* Belirli bir uygulamaya ait bir hizmet ilkesi değiştirildi mi?
* Uygulamaların adları değiştirildi mi?
* Belirli bir uygulama için kim onay verdi?

Yalnızca uygulamalarınızla ilgili denetim verilerini gözden geçirmek istiyorsanız, **Kurumsal uygulamalar** dikey penceresinin **Etkinlik** bölümündeki **Denetim günlükleri** altında filtrelenmiş bir görünüm bulabilirsiniz. Bu giriş noktasında, **Etkinlik Kaynağı Türü** olarak **Kurumsal uygulamalar** önceden seçilidir.

![Denetim günlükleri](./media/active-directory-reporting-activity-audit-logs/134.png "Denetim günlükleri")

Bu görünümü yalnızca **grupları** veya yalnızca **kullanıcıları** içerecek şekilde filtreleyebilirsiniz.

![Denetim günlükleri](./media/active-directory-reporting-activity-audit-logs/25.png "Denetim günlükleri")


## Sonraki adımlar
<a id="next-steps" class="xliff"></a>
Bkz. [Azure Active Directory Raporlama Kılavuzu](active-directory-reporting-guide.md).


