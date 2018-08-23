---
title: Azure Active Directory portalındaki denetim etkinliği raporları | Microsoft Docs
description: Azure Active Directory portalındaki denetim etkinliği raporlarına giriş
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: a1f93126-77d1-4345-ab7d-561066041161
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 04/19/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: b6fa26cb7947658af77496831d7239b4331aa1f2
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2018
ms.locfileid: "42059194"
---
# <a name="audit-activity-reports-in-the-azure-active-directory-portal"></a>Azure Active Directory portalındaki denetim etkinliği raporları 

Azure Active Directory’deki (Azure AD) raporlama özelliğiyle ortamınızın nasıl çalıştığını belirlemek için gereken bilgileri alabilirsiniz.

Azure AD'nin raporlama mimarisi aşağıdaki bileşenlerden oluşur:

- **Etkinlik** 
    - **Oturum açma etkinlikleri**: Yönetilen uygulamaların kullanımı ve kullanıcıların oturum açma etkinlikleri hakkında bilgiler
    - **Denetim günlükleri** - Azure AD içindeki çeşitli özellikler tarafından yapılan tüm değişiklikler için günlükler aracılığıyla izlenebilirlik sağlar. Azure AD içindeki kullanıcı, uygulama, grup, rol, ilke, kimlik doğrulama vb. herhangi bir kaynakta yapılan değişiklikler, denetim günlüklerinin örnekleridir.
- **Güvenlik** 
    - **Riskli oturum açma işlemleri** - Riskli oturum açma işlemi bir kullanıcı hesabının meşru sahibi olmayan bir kişi tarafından gerçekleştirilmiş olabilecek oturum açma girişiminin göstergesidir. Daha fazla bilgi için bkz. Riskli oturum açma işlemleri.
    - **Riskli oldukları belirlenen kullanıcılar** - Riskli kullanıcı, güvenliği tehlikeye girmiş olabilecek bir kullanıcı hesabının göstergesidir. Daha fazla bilgi için bkz. Riskli oldukları belirlenen kullanıcılar.

Bu konu başlığı denetim etkinliklerine genel bakış sunmaktadır.
 
## <a name="who-can-access-the-data"></a>Verilere kimler erişebilir?
* Güvenlik Yöneticisi veya Güvenlik Okuyucusu rolündeki kullanıcılar
* Genel Yöneticiler
* Bireysel kullanıcılar (yönetici olmayanlar) kendi etkinliklerini görebilir


## <a name="audit-logs"></a>Denetim günlükleri

Azure Active Directory'deki denetim günlükleri uyumluluk amacıyla sistem etkinliklerinin kayıtlarını sağlar.  
Tüm denetim verilerine ilk giriş noktanız, **Azure Active Directory**’nin **Etkinlik** bölümünde bulunan **Denetim günlükleri** kısmıdır.

![Denetim günlükleri](./media/concept-audit-logs/61.png "Denetim günlükleri")

Denetim günlüklerinin aşağıdakileri gösteren bir varsayılan liste görünümü vardır:

- Olayın tarihi ve saati
- Bir etkinliğin başlatıcısı/aktörü (*kim*) 
- Etkinlik (*ne*) 
- Hedef

![Denetim günlükleri](./media/concept-audit-logs/18.png "Denetim günlükleri")

Araç çubuğunda **Sütunlar**’a tıklayarak liste görünümünü özelleştirebilirsiniz.

![Denetim günlükleri](./media/concept-audit-logs/19.png "Denetim günlükleri")

Bu sayede ek alanları görüntüleyebilir ya da zaten görüntülenen alanları kaldırabilirsiniz.

![Denetim günlükleri](./media/concept-audit-logs/21.png "Denetim günlükleri")


Liste görünümündeki bir öğeye tıklayarak bu öğe hakkında mevcut olan tüm ayrıntıları öğrenebilirsiniz.

![Denetim günlükleri](./media/concept-audit-logs/22.png "Denetim günlükleri")


## <a name="filtering-audit-logs"></a>Denetim günlüklerini filtreleme

Raporlanan verileri istediğiniz düzeye gelecek şekilde daraltmak için, aşağıdaki alanları kullanarak denetim verilerini filtreleyebilirsiniz:

- Tarih aralığı
- Başlatan (Aktör)
- Kategori
- Etkinlik kaynak türü
- Etkinlik

![Denetim günlükleri](./media/concept-audit-logs/23.png "Denetim günlükleri")


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

Graph API'si (https://graph.windows.net/$tenantdomain/activities/auditActivityTypes?api-version=beta; burada $tenantdomain = etki alanınızın adıdır) kullanarak tüm Denetim Etkinliklerinin listesini alabilir veya [denetim raporu olayları](concept-audit-logs.md) makalesine bakabilirsiniz.


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

Yalnızca kullanıcı ve gruplarla ilgili denetim verilerini gözden geçirmek istiyorsanız, **Kullanıcılar ve Gruplar**’ın **Etkinlik** bölümündeki **Denetim günlükleri** altında filtrelenmiş bir görünüm bulabilirsiniz. Bu giriş noktasında, **Etkinlik Kaynağı Türü** olarak **Kullanıcılar ve gruplar** önceden seçilidir.

![Denetim günlükleri](./media/concept-audit-logs/93.png "Denetim günlükleri")

### <a name="enterprise-applications-audit-logs"></a>Kurumsal uygulamaların denetim günlükleri

Uygulama tabanlı denetim raporları ile aşağıdakiler gibi soruların yanıtlarını alabilirsiniz:

* Eklenmiş veya güncelleştirilmiş olan uygulamalar hangileridir?
* Kaldırılmış olan uygulamalar hangileridir?
* Belirli bir uygulamaya ait bir hizmet ilkesi değiştirildi mi?
* Uygulamaların adları değiştirildi mi?
* Belirli bir uygulama için kim onay verdi?

Yalnızca uygulamalarınızla ilgili denetim verilerini gözden geçirmek istiyorsanız, **Kurumsal uygulamalar** dikey penceresinin **Etkinlik** bölümündeki **Denetim günlükleri** altında filtrelenmiş bir görünüm bulabilirsiniz. Bu giriş noktasında, **Etkinlik Kaynağı Türü** olarak **Kurumsal uygulamalar** önceden seçilidir.

![Denetim günlükleri](./media/concept-audit-logs/134.png "Denetim günlükleri")

Bu görünümü yalnızca **grupları** veya yalnızca **kullanıcıları** içerecek şekilde filtreleyebilirsiniz.

![Denetim günlükleri](./media/concept-audit-logs/25.png "Denetim günlükleri")



## <a name="next-steps"></a>Sonraki adımlar

- Raporlamaya genel bir bakış için bkz. [Azure Active Directory raporlama](overview-reports.md).

- Tüm denetim etkinliklerinin tam listesi için bkz. [Azure AD denetim etkinliği başvurusu](reference-audit-activities.md)

