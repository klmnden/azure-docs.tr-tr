---
title: Azure Active Directory portalındaki denetim etkinliği raporları | Microsoft Docs
description: Azure Active Directory portalındaki denetim etkinliği raporlarına giriş
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: a1f93126-77d1-4345-ab7d-561066041161
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/31/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 97ea32a1e0f8815accff6201251771ab8c088859
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="audit-activity-reports-in-the-azure-active-directory-portal"></a>Azure Active Directory portalındaki denetim etkinliği raporları 

Azure Active Directory’deki (Azure AD) raporlama özelliğiyle ortamınızın nasıl çalıştığını belirlemek için gereken bilgileri alabilirsiniz.

Azure AD'nin raporlama mimarisi aşağıdaki bileşenlerden oluşur:

- **Etkinlik** 
    - **Oturum açma etkinlikleri**: Yönetilen uygulamaların kullanımı ve kullanıcıların oturum açma etkinlikleri hakkında bilgiler
    - **Denetim günlükleri**: Kullanıcılar ve grup yönetimi, yönetilen uygulamalarınız ve dizin etkinlikleriniz hakkında sistem etkinliği bilgileri.
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


## <a name="filtering-audit-logs"></a>Denetim günlüklerini filtreleme

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

Graph API'si (https://graph.windows.net/$tenantdomain/activities/auditActivityTypes?api-version=beta; burada $tenantdomain = etki alanınızın adıdır) kullanarak tüm Denetim Etkinliklerinin listesini alabilir veya [denetim raporu olayları](active-directory-reporting-audit-events.md) makalesine bakabilirsiniz.


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

![Denetim günlükleri](./media/active-directory-reporting-activity-audit-logs/93.png "Denetim günlükleri")

### <a name="enterprise-applications-audit-logs"></a>Kurumsal uygulamaların denetim günlükleri

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



## <a name="azure-ad-audit-activity-list"></a>Azure AD denetim etkinliği listesi

Bu bölümde, günlüğe kaydedilebilecek tüm etkinliklerin listesi size sağlanmaktadır. 


|Hizmet Adı|Denetim Kategorisi|Etkinlik Kaynağı Türü|Etkinlik|
|---|---|---|---|
|Hesap Sağlama|Uygulama Yönetimi|Uygulama|Yönetim|
|Hesap Sağlama|Uygulama Yönetimi|Uygulama|Dizin işlemi|
|Hesap Sağlama|Uygulama Yönetimi|Uygulama|Dışarı Aktarma|
|Hesap Sağlama|Uygulama Yönetimi|Uygulama|İçeri Aktarma|
|Hesap Sağlama|Uygulama Yönetimi|Uygulama|Diğer|
|Hesap Sağlama|Uygulama Yönetimi|Uygulama|Emanet işleme|
|Hesap Sağlama|Uygulama Yönetimi|Uygulama|Eşitleme kuralı eylemi|
|Uygulama Ara Sunucusu|Uygulama Yönetimi|Uygulama|Uygulama ekleme|
|Uygulama Ara Sunucusu|Kaynak|Kaynak|Uygulama SSL sertifikası ekleme|
|Uygulama Ara Sunucusu|Kaynak|Kaynak|SSL bağlamasını silme|
|Uygulama Ara Sunucusu|Uygulama Yönetimi|Uygulama|Uygulamayı silme|
|Uygulama Ara Sunucusu|Dizin Yönetimi|Dizin|Masaüstü Sso’yu devre dışı bırakma|
|Uygulama Ara Sunucusu|Dizin Yönetimi|Dizin|Belirli bir etki alanı için Masaüstü Sso’yu devre dışı bırakma|
|Uygulama Ara Sunucusu|Dizin Yönetimi|Dizin|Uygulama ara sunucusunu devre dışı bırakma|
|Uygulama Ara Sunucusu|Dizin Yönetimi|Dizin|Geçişli kimlik doğrulamasını devre dışı bırakma|
|Uygulama Ara Sunucusu|Dizin Yönetimi|Dizin|Masaüstü Sso’yu etkinleştirme|
|Uygulama Ara Sunucusu|Dizin Yönetimi|Dizin|Belirli bir etki alanı için Masaüstü Sso’yu etkinleştirme|
|Uygulama Ara Sunucusu|Dizin Yönetimi|Dizin|Uygulama ara sunucusunu etkinleştirme|
|Uygulama Ara Sunucusu|Dizin Yönetimi|Dizin|Geçişli kimlik doğrulamasını etkinleştirme|
|Uygulama Ara Sunucusu|Kaynak|Kaynak|Bağlayıcıyı kaydetme|
|Uygulama Ara Sunucusu|Uygulama Yönetimi|Uygulama|Uygulamayı güncelleştirme|
|Uygulama Ara Sunucusu|Uygulama Yönetimi|Uygulama|Uygulamada çoklu oturum açma modunu güncelleştirme|
|Otomatik Parola Geçişi|Uygulama Yönetimi|Uygulama|Otomatik Parola Geçişi|
|B2C|Uygulama Yönetimi|Uygulama|V2 uygulama izinleri ekleme|
|B2C|Yetkilendirme|Yetkilendirme|V2 uygulama izinleri ekleme|
|B2C|Anahtar|Anahtar|CPIM anahtar kapsayıcısına ASCII gizli anahtarına dayalı bir anahtar ekleme|
|B2C|Yetkilendirme|Yetkilendirme|CPIM anahtar kapsayıcısına ASCII gizli anahtarına dayalı bir anahtar ekleme|
|B2C|Anahtar|Anahtar|CPIM anahtar kapsayıcısına bir anahtar ekleme|
|B2C|Yetkilendirme|Yetkilendirme|CPIM anahtar kapsayıcısına bir anahtar ekleme|
|B2C|Kaynak|Kaynak|AdminPolicyDatas-RemoveResources|
|B2C|Yetkilendirme|Yetkilendirme|AdminPolicyDatas-SetResources|
|B2C|Kaynak|Kaynak|AdminPolicyDatas-SetResources|
|B2C|Kaynak|Kaynak|AdminUserJourneys-GetResources|
|B2C|Yetkilendirme|Yetkilendirme|AdminUserJourneys-GetResources|
|B2C|Yetkilendirme|Yetkilendirme|AdminUserJourneys-RemoveResources|
|B2C|Kaynak|Kaynak|AdminUserJourneys-RemoveResources|
|B2C|Kaynak|Kaynak|AdminUserJourneys-SetResources|
|B2C|Yetkilendirme|Yetkilendirme|AdminUserJourneys-SetResources|
|B2C|Yetkilendirme|Yetkilendirme|IdentityProvider oluşturma|
|B2C|Kaynak|Kaynak|IdentityProvider oluşturma|
|B2C|Yetkilendirme|Yetkilendirme|V1 uygulaması oluşturma|
|B2C|Uygulama Yönetimi|Uygulama|V1 uygulaması oluşturma|
|B2C|Uygulama Yönetimi|Uygulama|V2 uygulaması oluşturma|
|B2C|Yetkilendirme|Yetkilendirme|V2 uygulaması oluşturma|
|B2C|Dizin Yönetimi|Dizin|Kiracıda özel etki alanları oluşturma|
|B2C|Yetkilendirme|Yetkilendirme|Kiracıda özel etki alanları oluşturma|
|B2C|Yetkilendirme|Yetkilendirme|Yeni bir AdminUserJourney oluşturma|
|B2C|Kaynak|Kaynak|Yeni bir AdminUserJourney oluşturma|
|B2C|Kaynak|Kaynak|Yerelleştirilmiş kaynak json oluşturma|
|B2C|Yetkilendirme|Yetkilendirme|Yerelleştirilmiş kaynak json oluşturma|
|B2C|Yetkilendirme|Yetkilendirme|Yeni Özel IDP oluşturma|
|B2C|Kaynak|Kaynak|Yeni Özel IDP oluşturma|
|B2C|Kaynak|Kaynak|Yeni IDP oluşturma|
|B2C|Yetkilendirme|Yetkilendirme|Yeni IDP oluşturma|
|B2C|Yetkilendirme|Yetkilendirme|B2C dizin kaynağı oluşturma veya güncelleştirme|
|B2C|Kaynak|Kaynak|B2C dizin kaynağı oluşturma veya güncelleştirme|
|B2C|Kaynak|Kaynak|İlke oluşturma|
|B2C|Yetkilendirme|Yetkilendirme|İlke oluşturma|
|B2C|Yetkilendirme|Yetkilendirme|trustFramework ilkesi oluşturma|
|B2C|Kaynak|Kaynak|trustFramework ilkesi oluşturma|
|B2C|Yetkilendirme|Yetkilendirme|Yapılandırılabilir önek ile trustFramework ilkesi oluşturma|
|B2C|Kaynak|Kaynak|Yapılandırılabilir önek ile trustFramework ilkesi oluşturma|
|B2C|Kaynak|Kaynak|Kullanıcı özniteliği oluşturma|
|B2C|Yetkilendirme|Yetkilendirme|Kullanıcı özniteliği oluşturma|
|B2C|Yetkilendirme|Yetkilendirme|CreateTrustFrameworkPolicy|
|B2C|Kaynak|Kaynak|CreateTrustFrameworkPolicy|
|B2C|Yetkilendirme|Yetkilendirme|Yeni bir AdminUserJourney oluşturur veya güncelleştirir|
|B2C|Kaynak|Kaynak|IDP’yi silme|
|B2C|Yetkilendirme|Yetkilendirme|IDP’yi silme|
|B2C|Kaynak|Kaynak|IdentityProvider’ı silme|
|B2C|Yetkilendirme|Yetkilendirme|IdentityProvider’ı silme|
|B2C|Uygulama Yönetimi|Uygulama|V1 uygulamasını silme|
|B2C|Yetkilendirme|Yetkilendirme|V1 uygulamasını silme|
|B2C|Uygulama Yönetimi|Uygulama|V2 uygulamasını silme|
|B2C|Yetkilendirme|Yetkilendirme|V2 uygulamasını silme|
|B2C|Yetkilendirme|Yetkilendirme|V2 uygulama iznini silme|
|B2C|Uygulama Yönetimi|Uygulama|V2 uygulama iznini silme|
|B2C|Kaynak|Kaynak|B2C dizini kaynağını silme|
|B2C|Yetkilendirme|Yetkilendirme|B2C dizini kaynağını silme|
|B2C|Anahtar|Anahtar|CPIM anahtar kapsayıcısını silme|
|B2C|Yetkilendirme|Yetkilendirme|CPIM anahtar kapsayıcısını silme|
|B2C|Anahtar|Anahtar|Anahtar kapsayıcısını silme|
|B2C|Kaynak|Kaynak|trustFramework ilkesini silme|
|B2C|Yetkilendirme|Yetkilendirme|trustFramework ilkesini silme|
|B2C|Kaynak|Kaynak|Kullanıcı özniteliğini silme|
|B2C|Yetkilendirme|Yetkilendirme|Kullanıcı özniteliğini silme|
|B2C|Yetkilendirme|Yetkilendirme|B2C özelliğini etkinleştirme|
|B2C|Dizin Yönetimi|Dizin|B2C özelliğini etkinleştirme|
|B2C|Kaynak|Kaynak|Bir kaynak grubundaki B2C dizin kaynaklarını alma|
|B2C|Kaynak|Kaynak|Bir abonelikteki B2C dizin kaynaklarını alma|
|B2C|Yetkilendirme|Yetkilendirme|Bir abonelikteki B2C dizin kaynaklarını alma|
|B2C|Yetkilendirme|Yetkilendirme|Özel IDP alma|
|B2C|Kaynak|Kaynak|Özel IDP alma|
|B2C|Kaynak|Kaynak|IDP alma|
|B2C|Yetkilendirme|Yetkilendirme|IDP alma|
|B2C|Yetkilendirme|Yetkilendirme|V1 ve V2 uygulamalarını alma|
|B2C|Uygulama Yönetimi|Uygulama|V1 ve V2 uygulamalarını alma|
|B2C|Yetkilendirme|Yetkilendirme|V1 uygulaması alma|
|B2C|Uygulama Yönetimi|Uygulama|V1 uygulaması alma|
|B2C|Yetkilendirme|Yetkilendirme|V1 uygulamaları alma|
|B2C|Uygulama Yönetimi|Uygulama|V1 uygulamaları alma|
|B2C|Uygulama Yönetimi|Uygulama|V2 uygulaması alma|
|B2C|Yetkilendirme|Yetkilendirme|V2 uygulaması alma|
|B2C|Uygulama Yönetimi|Uygulama|V2 uygulamaları alma|
|B2C|Yetkilendirme|Yetkilendirme|V2 uygulamaları alma|
|B2C|Kaynak|Kaynak|B2C dizin kaynağı alma|
|B2C|Yetkilendirme|Yetkilendirme|B2C dizin kaynağı alma|
|B2C|Yetkilendirme|Yetkilendirme|Kiracıdaki özel etki alanlarının listesini alma|
|B2C|Dizin Yönetimi|Dizin|Kiracıdaki özel etki alanlarının listesini alma|
|B2C|Yetkilendirme|Yetkilendirme|Kullanıcı yolculuğu alma|
|B2C|Kaynak|Kaynak|Kullanıcı yolculuğu alma|
|B2C|Kaynak|Kaynak|Kullanıcı yolculuğu için izin verilen uygulama taleplerini alma|
|B2C|Yetkilendirme|Yetkilendirme|Kullanıcı yolculuğu için izin verilen uygulama taleplerini alma|
|B2C|Kaynak|Kaynak|Kullanıcı yolculuğu için izin verilen otomatik olarak onaylanan talepler alma|
|B2C|Yetkilendirme|Yetkilendirme|Kullanıcı yolculuğu için izin verilen otomatik olarak onaylanan talepler alma|
|B2C|Kaynak|Kaynak|İlkenin izin verilen otomatik olarak onaylanan taleplerini alma|
|B2C|Yetkilendirme|Yetkilendirme|İlkenin izin verilen otomatik olarak onaylanan taleplerini alma|
|B2C|Kaynak|Kaynak|Kullanılabilir çıktı talepleri listesini alma|
|B2C|Yetkilendirme|Yetkilendirme|Kullanılabilir çıktı talepleri listesini alma|
|B2C|Kaynak|Kaynak|Kullanıcı yolculuğu için içerik tanımlarını alma|
|B2C|Yetkilendirme|Yetkilendirme|Kullanıcı yolculuğu için içerik tanımlarını alma|
|B2C|Yetkilendirme|Yetkilendirme|Belirli bir yönetim akışı için idp’leri alma|
|B2C|Kaynak|Kaynak|Belirli bir yönetim akışı için idp’leri alma|
|B2C|Anahtar|Anahtar|JWK içindeki anahtar kapsayıcısı etkin anahtar meta verilerini alma|
|B2C|Yetkilendirme|Yetkilendirme|JWK içindeki anahtar kapsayıcısı etkin anahtar meta verilerini alma|
|B2C|Anahtar|Anahtar|Anahtar kapsayıcısı meta verilerini alma|
|B2C|Kaynak|Kaynak|Tüm yönetim akışlarının listesini alma|
|B2C|Yetkilendirme|Yetkilendirme|Tüm yönetim akışlarının listesini alma|
|B2C|Yetkilendirme|Yetkilendirme|Tüm kullanıcılar için tüm yönetim akışlarının etiketlerinin listesini alma|
|B2C|Kaynak|Kaynak|Tüm kullanıcılar için tüm yönetim akışlarının etiketlerinin listesini alma|
|B2C|Kaynak|Kaynak|Bir kullanıcı için kiracıların listesini alma|
|B2C|Yetkilendirme|Yetkilendirme|Bir kullanıcı için kiracıların listesini alma|
|B2C|Kaynak|Kaynak|Yerel hesapların otomatik olarak onaylanan taleplerini alma|
|B2C|Yetkilendirme|Yetkilendirme|Yerel hesapların otomatik olarak onaylanan taleplerini alma|
|B2C|Kaynak|Kaynak|Yerelleştirilmiş kaynak json alma|
|B2C|Yetkilendirme|Yetkilendirme|Yerelleştirilmiş kaynak json alma|
|B2C|Yetkilendirme|Yetkilendirme|Microsoft.AzureActiveDirectory kaynak sağlayıcısının işlemlerini alma|
|B2C|Kaynak|Kaynak|Microsoft.AzureActiveDirectory kaynak sağlayıcısının işlemlerini alma|
|B2C|Kaynak|Kaynak|İlkeleri alma|
|B2C|Yetkilendirme|Yetkilendirme|İlkeleri alma|
|B2C|Kaynak|Kaynak|İlke alma|
|B2C|Yetkilendirme|Yetkilendirme|İlke alma|
|B2C|Dizin Yönetimi|Dizin|Bir kiracının kaynak özelliklerini alma|
|B2C|Yetkilendirme|Yetkilendirme|Bir kiracının kaynak özelliklerini alma|
|B2C|Kaynak|Kaynak|Desteklenen IDP listesini alma|
|B2C|Yetkilendirme|Yetkilendirme|Desteklenen IDP listesini alma|
|B2C|Kaynak|Kaynak|Kullanıcı yolculuğunun desteklenen IDP listesini alma|
|B2C|Yetkilendirme|Yetkilendirme|Kullanıcı yolculuğunun desteklenen IDP listesini alma|
|B2C|Dizin Yönetimi|Dizin|Kiracı Bilgilerini alma|
|B2C|Yetkilendirme|Yetkilendirme|Kiracı Bilgilerini alma|
|B2C|Dizin Yönetimi|Dizin|Kiracının izin verilen özelliklerini alma|
|B2C|Yetkilendirme|Yetkilendirme|Kiracının izin verilen özelliklerini alma|
|B2C|Yetkilendirme|Yetkilendirme|Kiracı tanımlı Özel IDP listesini alma|
|B2C|Kaynak|Kaynak|Kiracı tanımlı Özel IDP listesini alma|
|B2C|Kaynak|Kaynak|Kiracı tanımlı IDP listesini alma|
|B2C|Yetkilendirme|Yetkilendirme|Kiracı tanımlı IDP listesini alma|
|B2C|Kaynak|Kaynak|Kiracı tanımlı yerel IDP listesini alma|
|B2C|Yetkilendirme|Yetkilendirme|Kiracı tanımlı yerel IDP listesini alma|
|B2C|Kaynak|Kaynak|Kaynak oluşturma için bir kullanıcının kiracı ayrıntılarını alma|
|B2C|Yetkilendirme|Yetkilendirme|Kaynak oluşturma için bir kullanıcının kiracı ayrıntılarını alma|
|B2C|Yetkilendirme|Yetkilendirme|Kiracı listesini alma|
|B2C|Yetkilendirme|Yetkilendirme|tenantDomains alma|
|B2C|Dizin Yönetimi|Dizin|tenantDomains alma|
|B2C|Kaynak|Kaynak|CPIM için varsayılan desteklenen kültürü alma|
|B2C|Yetkilendirme|Yetkilendirme|CPIM için varsayılan desteklenen kültürü alma|
|B2C|Kaynak|Kaynak|Bir yönetim akışının ayrıntılarını alma|
|B2C|Yetkilendirme|Yetkilendirme|Bir yönetim akışının ayrıntılarını alma|
|B2C|Yetkilendirme|Yetkilendirme|Bu kiracı için UserJourneys listesini alma|
|B2C|Kaynak|Kaynak|Bu kiracı için UserJourneys listesini alma|
|B2C|Yetkilendirme|Yetkilendirme|CPIM için kullanılabilir desteklenen kültürler kümesini alma|
|B2C|Kaynak|Kaynak|CPIM için kullanılabilir desteklenen kültürler kümesini alma|
|B2C|Yetkilendirme|Yetkilendirme|trustFramework ilkesini alma|
|B2C|Kaynak|Kaynak|trustFramework ilkesini alma|
|B2C|Yetkilendirme|Yetkilendirme|trustFramework ilkesini xml olarak alma|
|B2C|Kaynak|Kaynak|trustFramework ilkesini xml olarak alma|
|B2C|Kaynak|Kaynak|Kullanıcı özniteliğini alma|
|B2C|Yetkilendirme|Yetkilendirme|Kullanıcı özniteliğini alma|
|B2C|Yetkilendirme|Yetkilendirme|Kullanıcı özniteliklerini alma|
|B2C|Kaynak|Kaynak|Kullanıcı özniteliklerini alma|
|B2C|Yetkilendirme|Yetkilendirme|Kullanıcı yolculuğu listesini alma|
|B2C|Kaynak|Kaynak|Kullanıcı yolculuğu listesini alma|
|B2C|Yetkilendirme|Yetkilendirme|GetIEFPolicies|
|B2C|Kaynak|Kaynak|GetIEFPolicies|
|B2C|Yetkilendirme|Yetkilendirme|GetIdentityProviders|
|B2C|Kaynak|Kaynak|GetIdentityProviders|
|B2C|Kaynak|Kaynak|GetTrustFrameworkPolicy|
|B2C|Yetkilendirme|Yetkilendirme|GetTrustFrameworkPolicy|
|B2C|Anahtar|Anahtar|Bir CPIM anahtar kapsayıcısını jwk biçiminde alır|
|B2C|Yetkilendirme|Yetkilendirme|Bir CPIM anahtar kapsayıcısını jwk biçiminde alır|
|B2C|Anahtar|Anahtar|Kiracıdaki anahtar kapsayıcılarının listesini alır|
|B2C|Yetkilendirme|Yetkilendirme|Kiracıdaki anahtar kapsayıcılarının listesini alır|
|B2C|Yetkilendirme|Yetkilendirme|Kiracı türünü alır|
|B2C|Dizin Yönetimi|Dizin|Kiracı türünü alır|
|B2C|Kimlik Doğrulaması|Kimlik Doğrulaması|Uygulamaya bir erişim belirteci verme|
|B2C|Kimlik Doğrulaması|Kimlik Doğrulaması|Uygulamaya bir yetkilendirme kodu verme|
|B2C|Diğer|Diğer|Uygulamaya bir yetkilendirme kodu verme|
|B2C|Kimlik Doğrulaması|Kimlik Doğrulaması|Uygulamaya bir id_token verme|
|B2C|Diğer|Diğer|Uygulamaya bir id_token verme|
|B2C|Yetkilendirme|Yetkilendirme|MigrateTenantMetadata|
|B2C|Kaynak|Kaynak|MigrateTenantMetadata|
|B2C|Kaynak|Kaynak|Kaynakları taşıma|
|B2C|Yetkilendirme|Yetkilendirme|IdentityProvider için düzeltme eki uygulama|
|B2C|Kaynak|Kaynak|IdentityProvider için düzeltme eki uygulama|
|B2C|Kaynak|Kaynak|PutTrustFrameworkPolicy|
|B2C|Yetkilendirme|Yetkilendirme|PutTrustFrameworkPolicy|
|B2C|Yetkilendirme|Yetkilendirme|PutTrustFrameworkpolicy|
|B2C|Kaynak|Kaynak|PutTrustFrameworkpolicy|
|B2C|Kaynak|Kaynak|Kullanıcı yolculuğunu kaldırma|
|B2C|Yetkilendirme|Yetkilendirme|Kullanıcı yolculuğunu kaldırma|
|B2C|Yetkilendirme|Yetkilendirme|CPIM anahtar kapsayıcısı yedeklemesini geri yükleme|
|B2C|Anahtar|Anahtar|CPIM anahtar kapsayıcısı yedeklemesini geri yükleme|
|B2C|Uygulama Yönetimi|Uygulama|V2 uygulama izinleri alma|
|B2C|Yetkilendirme|Yetkilendirme|V2 uygulama izinleri alma|
|B2C|Uygulama Yönetimi|Uygulama|Geçerli kiracıdaki V2 uygulama hizmet sorumlularını alma|
|B2C|Yetkilendirme|Yetkilendirme|Geçerli kiracıdaki V2 uygulama hizmet sorumlularını alma|
|B2C|Anahtar|Anahtar|Anahtar kapsayıcısını kaydetme|
|B2C|Yetkilendirme|Yetkilendirme|Özel IDP’yi güncelleştirme|
|B2C|Kaynak|Kaynak|Özel IDP’yi güncelleştirme|
|B2C|Kaynak|Kaynak|IDP’yi güncelleştirme|
|B2C|Yetkilendirme|Yetkilendirme|IDP’yi güncelleştirme|
|B2C|Kaynak|Kaynak|Yerel IDP’yi güncelleştirme|
|B2C|Yetkilendirme|Yetkilendirme|Yerel IDP’yi güncelleştirme|
|B2C|Uygulama Yönetimi|Uygulama|V1 uygulamasını güncelleştirme|
|B2C|Yetkilendirme|Yetkilendirme|V1 uygulamasını güncelleştirme|
|B2C|Uygulama Yönetimi|Uygulama|V2 uygulamasını güncelleştirme|
|B2C|Yetkilendirme|Yetkilendirme|V2 uygulamasını güncelleştirme|
|B2C|Uygulama Yönetimi|Uygulama|V2 uygulama iznini güncelleştirme|
|B2C|Yetkilendirme|Yetkilendirme|V2 uygulama iznini güncelleştirme|
|B2C|Kaynak|Kaynak|B2C dizini kaynağını güncelleştirme|
|B2C|Kaynak|Kaynak|İlkeyi güncelleştirme|
|B2C|Yetkilendirme|Yetkilendirme|İlkeyi güncelleştirme|
|B2C|Kaynak|Kaynak|Abonelik durumunu güncelleştirme|
|B2C|Kaynak|Kaynak|Kullanıcı özniteliğini güncelleştirme|
|B2C|Yetkilendirme|Yetkilendirme|Kullanıcı özniteliğini güncelleştirme|
|B2C|Anahtar|Anahtar|CPIM şifrelenmiş anahtarını karşıya yükleme|
|B2C|Yetkilendirme|Yetkilendirme|CPIM şifrelenmiş anahtarını karşıya yükleme|
|B2C|Yetkilendirme|Yetkilendirme|Kullanıcı Yetkilendirmesi: Kiracı featureset için API devre dışı bırakıldı|
|B2C|Yetkilendirme|Yetkilendirme|Kullanıcı Yetkilendirmesi: Kullanıcıya 'Kiracı Yöneticisi' olarak erişim izni verildi|
|B2C|Yetkilendirme|Yetkilendirme|Kullanıcı Yetkilendirmesi: Kullanıcıya 'Kimliği Doğrulanmış Kullanıcılar' erişim hakları verildi|
|B2C|Kimlik Doğrulaması|Kimlik Doğrulaması|Yerel hesap kimlik bilgilerini doğrulama|
|B2C|Kaynak|Kaynak|Kaynakları taşımayı doğrulama|
|B2C|Kimlik Doğrulaması|Kimlik Doğrulaması|Kullanıcı kimlik doğrulamasını doğrulama|
|B2C|Dizin Yönetimi|Dizin|B2C özelliğinin etkinleştirildiğini doğrulama|
|B2C|Yetkilendirme|Yetkilendirme|B2C özelliğinin etkinleştirildiğini doğrulama|
|B2C|Yetkilendirme|Yetkilendirme|Özelliğin etkinleştirildiğini doğrulama|
|B2C|Dizin Yönetimi|Dizin|Özelliğin etkinleştirildiğini doğrulama|
|Çekirdek Dizin|Uygulama Yönetimi|Uygulama|OAuth2PermissionGrant ekleme|
|Çekirdek Dizin|Yönetim Birimi Yönetimi|AdministrativeUnit|Yönetim birimi ekleme|
|Çekirdek Dizin|Kullanıcı Yönetimi|Kullanıcı|Kullanıcıya uygulama rolü atama izni ekleme|
|Çekirdek Dizin|Grup Yönetimi|Grup|Gruba uygulama rolü ataması ekleme|
|Çekirdek Dizin|Uygulama Yönetimi|Uygulama|Hizmet sorumlusuna uygulama rol ataması ekleme|
|Çekirdek Dizin|Uygulama Yönetimi|Uygulama|Uygulama ekleme|
|Çekirdek Dizin|Kaynak|Kaynak|Cihaz ekleme|
|Çekirdek Dizin|Kaynak|Kaynak|Cihaz yapılandırması ekleme|
|Çekirdek Dizin|Rol Yönetimi|Rol|Role uygun üye ekleme|
|Çekirdek Dizin|Grup Yönetimi|Grup|Grup ekleme|
|Çekirdek Dizin|Yönetim Birimi Yönetimi|AdministrativeUnit|Yönetim birimine üye ekleme|
|Çekirdek Dizin|Grup Yönetimi|Grup|Gruba üye ekleme|
|Çekirdek Dizin|Rol Yönetimi|Rol|Role üye ekleme|
|Çekirdek Dizin|Uygulama Yönetimi|Uygulama|Uygulamaya sahip ekleme|
|Çekirdek Dizin|Grup Yönetimi|Grup|Gruba sahip ekleme|
|Çekirdek Dizin|İlke Yönetimi|İlke|İlkeye sahip ekleme|
|Çekirdek Dizin|Uygulama Yönetimi|Uygulama|Hizmet sorumlusuna sahip ekleme|
|Çekirdek Dizin|Dizin Yönetimi|Dizin|Şirkete iş ortağı ekleme|
|Çekirdek Dizin|İlke Yönetimi|İlke|İlke ekleme|
|Çekirdek Dizin|Uygulama Yönetimi|Uygulama|Hizmet sorumlusuna ilke ekleme|
|Çekirdek Dizin|Kaynak|Kaynak|Cihaza kayıtlı sahip ekleme|
|Çekirdek Dizin|Kaynak|Kaynak|Cihaza kayıtlı kullanıcılar ekleme|
|Çekirdek Dizin|Rol Yönetimi|Rol|Rol tanımına rol ataması ekleme|
|Çekirdek Dizin|Rol Yönetimi|Rol|Şablondan rol ekleme|
|Çekirdek Dizin|Rol Yönetimi|Rol|Role kapsamlı üye ekleme|
|Çekirdek Dizin|Uygulama Yönetimi|Uygulama|Hizmet sorumlusu ekleme|
|Çekirdek Dizin|Uygulama Yönetimi|Uygulama|Hizmet sorumlusu kimlik bilgileri ekleme|
|Çekirdek Dizin|Dizin Yönetimi|Dizin|Doğrulanmamış etki alanı ekleme|
|Çekirdek Dizin|Kullanıcı Yönetimi|Kullanıcı|Kullanıcı ekle|
|Çekirdek Dizin|Kullanıcı Yönetimi|Kullanıcı|Kullanıcılar için güçlü kimlik doğrulaması telefon uygulaması ayrıntılarını ekleme|
|Çekirdek Dizin|Dizin Yönetimi|Dizin|Doğrulanmış etki alanı ekleme|
|Çekirdek Dizin|Kullanıcı Yönetimi|Kullanıcı|Kullanıcı lisansını değiştirme|
|Çekirdek Dizin|Kullanıcı Yönetimi|Kullanıcı|Kullanıcı parolasını değiştirme|
|Çekirdek Dizin|Uygulama Yönetimi|Uygulama|Uygulama onayı|
|Çekirdek Dizin|Kullanıcı Yönetimi|Kullanıcı|Federasyon kullanıcısını yönetilen kullanıcıya dönüştürme|
|Çekirdek Dizin|Kullanıcı Yönetimi|Kullanıcı|Kullanıcı için uygulama parolası oluşturma|
|Çekirdek Dizin|Dizin Yönetimi|Dizin|Şirket oluşturma|
|Çekirdek Dizin|Dizin Yönetimi|Dizin|Şirket ayarları oluşturma|
|Çekirdek Dizin|Grup Yönetimi|Grup|Grup ayarları oluşturma|
|Çekirdek Dizin|Yönetim Birimi Yönetimi|AdministrativeUnit|Yönetim birimini silme|
|Çekirdek Dizin|Uygulama Yönetimi|Uygulama|Uygulamayı silme|
|Çekirdek Dizin|Kullanıcı Yönetimi|Kullanıcı|Kullanıcı için uygulama parolasını silme|
|Çekirdek Dizin|Dizin Yönetimi|Dizin|Şirket ayarlarını silme|
|Çekirdek Dizin|Kaynak|Kaynak|Cihazı silme|
|Çekirdek Dizin|Kaynak|Kaynak|Cihaz yapılandırmasını silme|
|Çekirdek Dizin|Grup Yönetimi|Grup|Grubu silme|
|Çekirdek Dizin|Grup Yönetimi|Grup|Grup ayarlarını silme|
|Çekirdek Dizin|İlke Yönetimi|İlke|İlkeyi silme|
|Çekirdek Dizin|Kullanıcı Yönetimi|Kullanıcı|Kullanıcıyı silme|
|Çekirdek Dizin|Dizin Yönetimi|Dizin|İş ortağını indirgeme|
|Çekirdek Dizin|Kaynak|Kaynak|Cihaz artık uyumlu değil|
|Çekirdek Dizin|Kaynak|Kaynak|Cihaz artık yönetilen cihaz değil|
|Çekirdek Dizin|Dizin Yönetimi|Dizin|Dizin silindi|
|Çekirdek Dizin|Dizin Yönetimi|Dizin|Dizin kalıcı olarak silindi|
|Çekirdek Dizin|Dizin Yönetimi|Dizin|Dizin silinmek üzere zamanlandı|
|Çekirdek Dizin|Kullanıcı Yönetimi|Kullanıcı|Hesabı devre dışı bırak|
|Çekirdek Dizin|Kullanıcı Yönetimi|Kullanıcı|Güçlü Kimlik Doğrulamasını Etkinleştirme|
|Çekirdek Dizin|Grup Yönetimi|Grup|Kullanıcılara grup tabanlı lisans uygulamayı sonlandırma|
|Çekirdek Dizin|Uygulama Yönetimi|Uygulama|Uygulamayı Kalıcı Olarak Silme|
|Çekirdek Dizin|Grup Yönetimi|Grup|Grubu Kalıcı Olarak Silme|
|Çekirdek Dizin|Kullanıcı Yönetimi|Kullanıcı|Kullanıcıyı Kalıcı Olarak Silme|
|Çekirdek Dizin|Dizin Yönetimi|Dizin|Şirketi iş ortağına yükseltme|
|Çekirdek Dizin|Dizin Yönetimi|Dizin|Hak yönetimi özelliklerini temizleme|
|Çekirdek Dizin|Uygulama Yönetimi|Uygulama|OAuth2PermissionGrant’ı kaldırma|
|Çekirdek Dizin|Grup Yönetimi|Grup|Gruptan uygulama rolü atamasını kaldırma|
|Çekirdek Dizin|Uygulama Yönetimi|Uygulama|Hizmet sorumlusundan uygulama rolü atamasını kaldırma|
|Çekirdek Dizin|Kullanıcı Yönetimi|Kullanıcı|Kullanıcıdan uygulama rolü atamasını kaldırma|
|Çekirdek Dizin|Rol Yönetimi|Rol|Rolden uygun üyeyi kaldırma|
|Çekirdek Dizin|Yönetim Birimi Yönetimi|AdministrativeUnit|Yönetim biriminden üyeyi kaldırma|
|Çekirdek Dizin|Grup Yönetimi|Grup|Gruptan üyeyi kaldırma|
|Çekirdek Dizin|Rol Yönetimi|Rol|Rolden üyeyi kaldırma|
|Çekirdek Dizin|Uygulama Yönetimi|Uygulama|Uygulamadan sahibi kaldırma|
|Çekirdek Dizin|Grup Yönetimi|Grup|Gruptan sahibi kaldırma|
|Çekirdek Dizin|Uygulama Yönetimi|Uygulama|Hizmet sorumlusundan sahibi kaldırma|
|Çekirdek Dizin|Dizin Yönetimi|Dizin|Şirketten iş ortağını kaldırma|
|Çekirdek Dizin|İlke Yönetimi|İlke|İlke kimlik bilgilerini kaldırma|
|Çekirdek Dizin|Uygulama Yönetimi|Uygulama|Hizmet sorumlusundan ilkeyi kaldırma|
|Çekirdek Dizin|Kaynak|Kaynak|Cihazdan kayıtlı sahibi kaldırma|
|Çekirdek Dizin|Kaynak|Kaynak|Cihazdan kayıtlı kullanıcıları kaldırma|
|Çekirdek Dizin|Rol Yönetimi|Rol|Rol tanımından rol atamasını kaldırma|
|Çekirdek Dizin|Rol Yönetimi|Rol|Rolden kapsamlı üyeyi kaldırma|
|Çekirdek Dizin|Uygulama Yönetimi|Uygulama|Hizmet sorumlusunu kaldırma|
|Çekirdek Dizin|Uygulama Yönetimi|Uygulama|Hizmet sorumlusu kimlik bilgilerini kaldırma|
|Çekirdek Dizin|Dizin Yönetimi|Dizin|Doğrulanmamış etki alanını kaldırma|
|Çekirdek Dizin|Kullanıcı Yönetimi|Kullanıcı|Kullanıcılar için güçlü kimlik doğrulaması telefon uygulaması ayrıntılarını kaldırma|
|Çekirdek Dizin|Dizin Yönetimi|Dizin|Doğrulanmış etki alanını kaldırma|
|Çekirdek Dizin|Kullanıcı Yönetimi|Kullanıcı|Kullanıcı parolasını sıfırlama|
|Çekirdek Dizin|Grup Yönetimi|Grup|Grubu geri yükleme|
|Çekirdek Dizin|Uygulama Yönetimi|Uygulama|Uygulamayı geri yükleme|
|Çekirdek Dizin|Kullanıcı Yönetimi|Kullanıcı|Kullanıcıyı geri yükleme|
|Çekirdek Dizin|Uygulama Yönetimi|Uygulama|İzni iptal etme|
|Çekirdek Dizin|Dizin Yönetimi|Dizin|Şirket Bilgilerini Ayarlama|
|Çekirdek Dizin|Dizin Yönetimi|Dizin|DirSync özelliğini ayarlama|
|Çekirdek Dizin|Dizin Yönetimi|Dizin|DirSyncEnabled bayrağını ayarlama|
|Çekirdek Dizin|Dizin Yönetimi|Dizin|İş Ortaklığı Ayarlama|
|Çekirdek Dizin|Dizin Yönetimi|Dizin|Yanlışlıkla silme eşiğini ayarlama|
|Çekirdek Dizin|Dizin Yönetimi|Dizin|Şirket tarafından izin verilen veri konumunu ayarlama|
|Çekirdek Dizin|Dizin Yönetimi|Dizin|Şirket çok uluslu özelliğini etkin olarak ayarlama|
|Çekirdek Dizin|Dizin Yönetimi|Dizin|Kiracıda dizin özelliğini ayarlama|
|Çekirdek Dizin|Dizin Yönetimi|Dizin|Etki alanı kimlik doğrulaması ayarlama|
|Çekirdek Dizin|Dizin Yönetimi|Dizin|Etki alanında federasyon ayarlarını belirleme|
|Çekirdek Dizin|Kullanıcı Yönetimi|Kullanıcı|Kullanıcı parolasını değiştirmeye zorlamayı ayarlama|
|Çekirdek Dizin|Grup Yönetimi|Grup|Grup lisansı ayarlama|
|Çekirdek Dizin|Grup Yönetimi|Grup|Grubu kullanıcı tarafından yönetilecek şekilde ayarlama|
|Çekirdek Dizin|Dizin Yönetimi|Dizin|Parola ilkesi ayarlama|
|Çekirdek Dizin|Dizin Yönetimi|Dizin|Rights management özelliklerini ayarlama|
|Çekirdek Dizin|Kullanıcı Yönetimi|Kullanıcı|Kullanıcı yöneticisini ayarlama|
|Çekirdek Dizin|Kullanıcı Yönetimi|Kullanıcı|Kullanıcılar için kimlik doğrulaması belirteci meta verilerini etkin olarak ayarlama|
|Çekirdek Dizin|Grup Yönetimi|Grup|Kullanıcılara grup tabanlı lisans uygulamaya başlama|
|Çekirdek Dizin|Grup Yönetimi|Grup|Tetikleyici grubu lisans yeniden hesaplaması|
|Çekirdek Dizin|Kullanıcı Yönetimi|Kullanıcı|StsRefreshTokenValidFrom Zaman Damgasını Güncelleştirme|
|Çekirdek Dizin|Yönetim Birimi Yönetimi|AdministrativeUnit|Yönetim birimini güncelleştirme|
|Çekirdek Dizin|Uygulama Yönetimi|Uygulama|Uygulamayı güncelleştirme|
|Çekirdek Dizin|Dizin Yönetimi|Dizin|Şirketi güncelleştirme|
|Çekirdek Dizin|Dizin Yönetimi|Dizin|Şirket ayarlarını güncelleştirme|
|Çekirdek Dizin|Kaynak|Kaynak|Cihazı güncelleştirme|
|Çekirdek Dizin|Kaynak|Kaynak|Cihaz yapılandırmasını güncelleştirme|
|Çekirdek Dizin|Dizin Yönetimi|Dizin|Etki alanını güncelleştirme|
|Çekirdek Dizin|Kullanıcı Yönetimi|Kullanıcı|Dış gizli anahtarları güncelleştirme|
|Çekirdek Dizin|Uygulama Yönetimi|Uygulama|Dış gizli anahtarları güncelleştirme|
|Çekirdek Dizin|Grup Yönetimi|Grup|Grubu güncelleştirme|
|Çekirdek Dizin|Grup Yönetimi|Grup|Grup ayarlarını güncelleştirme|
|Çekirdek Dizin|İlke Yönetimi|İlke|İlkeyi güncelleştirme|
|Çekirdek Dizin|Rol Yönetimi|Rol|Rolü güncelleştirme|
|Çekirdek Dizin|Uygulama Yönetimi|Uygulama|Hizmet sorumlusunu güncelleştirme|
|Çekirdek Dizin|Kullanıcı Yönetimi|Kullanıcı|Kullanıcıyı güncelleştirme|
|Çekirdek Dizin|Dizin Yönetimi|Dizin|Etki alanını doğrulama|
|Çekirdek Dizin|Dizin Yönetimi|Dizin|E-posta ile doğrulanmış etki alanını doğrulama|
|Kimlik Koruması|Kullanıcı Yönetimi|Kullanıcı|Yönetici geçici bir parola oluşturur|
|Kimlik Koruması|Kullanıcı Yönetimi|Kullanıcı|Yöneticiler, kullanıcının parolasını sıfırlamasını zorunlu tutar|
|Kimlik Koruması|Diğer|Diğer|Tek bir risk olayı türünü indirme|
|Kimlik Koruması|Diğer|Diğer|Yöneticileri ve haftalık özet katılımı durumunu indirme|
|Kimlik Koruması|Diğer|Diğer|Tüm risk olayı türlerini indirme|
|Kimlik Koruması|Diğer|Diğer|Ücretsiz kullanıcı risk olaylarını indirme|
|Kimlik Koruması|Diğer|Diğer|Riskli olduğu belirlenen kullanıcıları indirme|
|Kimlik Koruması|Dizin Yönetimi|Dizin|Ekleme|
|Kimlik Koruması|İlke Yönetimi|İlke|MFA kayıt ilkesini ayarlama|
|Kimlik Koruması|İlke Yönetimi|İlke|Oturum açma riski ilkesini ayarlama|
|Kimlik Koruması|İlke Yönetimi|İlke|Kullanıcı riski ilkesini ayarlama|
|Kimlik Koruması|Dizin Yönetimi|Dizin|Uyarı ayarlarını güncelleştirme|
|Kimlik Koruması|Dizin Yönetimi|Dizin|Haftalık özet ayarlarını güncelleştirme|
|Davetli Kullanıcılar|Kullanıcı Yönetimi|Kullanıcı|Uygulamaya dış kullanıcı atama|
|Davetli Kullanıcılar|Diğer|Diğer|İşlenen toplu davetler|
|Davetli Kullanıcılar|Diğer|Diğer|Karşıya yüklenen toplu davetler|
|Davetli Kullanıcılar|Kullanıcı Yönetimi|Kullanıcı|E-posta gönderilmedi, kullanıcının aboneliği kaldırıldı|
|Davetli Kullanıcılar|Kullanıcı Yönetimi|Kullanıcı|Dış kullanıcıyı davet etme|
|Davetli Kullanıcılar|Kullanıcı Yönetimi|Kullanıcı|Dış kullanıcı davetini kullanma|
|Davetli Kullanıcılar|Kullanıcı Yönetimi|Kullanıcı|Virüslü kiracı oluşturma|
|Davetli Kullanıcılar|Kullanıcı Yönetimi|Kullanıcı|Virüslü kullanıcı oluşturma|
|Microsoft Identity Manager (MIM)|Grup Yönetimi|Grup|Üye Ekleme|
|Microsoft Identity Manager (MIM)|Grup Yönetimi|Grup|Grup Oluşturma|
|Microsoft Identity Manager (MIM)|Grup Yönetimi|Grup|Grubu Silme|
|Microsoft Identity Manager (MIM)|Grup Yönetimi|Grup|Üye Kaldırma|
|Microsoft Identity Manager (MIM)|Grup Yönetimi|Grup|Grubu Güncelleştirme|
|Microsoft Identity Manager (MIM)|Kullanıcı Yönetimi|Kullanıcı|Kullanıcı Parolası Kaydı|
|Microsoft Identity Manager (MIM)|Kullanıcı Yönetimi|Kullanıcı|Kullanıcı Parolası Sıfırlama|
|Privileged Identity Management|Rol Yönetimi|Rol|AccessReview_Review|
|Privileged Identity Management|Rol Yönetimi|Rol|AccessReview_Update|
|Privileged Identity Management|Rol Yönetimi|Rol|ActivationAborted|
|Privileged Identity Management|Rol Yönetimi|Rol|ActivationApproved|
|Privileged Identity Management|Rol Yönetimi|Rol|ActivationCanceled|
|Privileged Identity Management|Rol Yönetimi|Rol|ActivationRequested|
|Privileged Identity Management|Rol Yönetimi|Rol|Eklendi|
|Privileged Identity Management|Rol Yönetimi|Rol|Ata|
|Privileged Identity Management|Rol Yönetimi|Rol|Yükselt|
|Privileged Identity Management|Rol Yönetimi|Rol|Kaldırıldı|
|Privileged Identity Management|Rol Yönetimi|Rol|Rol Ayarı değişiklikleri|
|Privileged Identity Management|Rol Yönetimi|Rol|ScanAlertsNow|
|Privileged Identity Management|Rol Yönetimi|Rol|Kaydol|
|Privileged Identity Management|Rol Yönetimi|Rol|Yetkiyi Kaldır|
|Privileged Identity Management|Rol Yönetimi|Rol|UpdateAlertSettings|
|Privileged Identity Management|Rol Yönetimi|Rol|UpdateCurrentState|
|Self Servis Grup Yönetimi|Grup Yönetimi|Grup|Onay bekleyen bir gruba katılma isteğini onaylama|
|Self Servis Grup Yönetimi|Grup Yönetimi|Grup|Onay bekleyen bir gruba katılma isteğini iptal etme|
|Self Servis Grup Yönetimi|Grup Yönetimi|Grup|Yaşam döngüsü yönetim ilkesi oluşturma|
|Self Servis Grup Yönetimi|Grup Yönetimi|Grup|Onay bekleyen bir gruba katılma isteğini silme|
|Self Servis Grup Yönetimi|Grup Yönetimi|Grup|Onay bekleyen bir gruba katılma isteğini reddetme|
|Self Servis Grup Yönetimi|Grup Yönetimi|Grup|Grubu yenileme|
|Self Servis Grup Yönetimi|Grup Yönetimi|Grup|Bir gruba katılma isteğinde bulunma|
|Self Servis Grup Yönetimi|Grup Yönetimi|Grup|Dinamik grup özelliklerini ayarlama|
|Self Servis Grup Yönetimi|Grup Yönetimi|Grup|Yaşam döngüsü yönetim ilkesini güncelleştirme|
|Self Servis Parola Yönetimi|Kullanıcı Yönetimi|Kullanıcı|Self servis parola sıfırlaması engellendi|
|Self Servis Parola Yönetimi|Kullanıcı Yönetimi|Kullanıcı|Parolayı değiştirme (self servis)|
|Self Servis Parola Yönetimi|Dizin Yönetimi|Dizin|Dizin için parola geri yazmayı devre dışı bırakma|
|Self Servis Parola Yönetimi|Dizin Yönetimi|Dizin|Dizin için parola geri yazmayı etkinleştirme|
|Self Servis Parola Yönetimi|Kullanıcı Yönetimi|Kullanıcı|Parola sıfırlama (yönetici tarafından)|
|Self Servis Parola Yönetimi|Kullanıcı Yönetimi|Kullanıcı|Parola sıfırlama (self servis)|
|Self Servis Parola Yönetimi|Kullanıcı Yönetimi|Kullanıcı|Self servis parola sıfırlama akış etkinliği ilerleme durumu|
|Self Servis Parola Yönetimi|Kullanıcı Yönetimi|Kullanıcı|Self servis parola sıfırlama akış etkinliği ilerleme durumu|
|Self Servis Parola Yönetimi|Kullanıcı Yönetimi|Kullanıcı|Kullanıcı hesabının kilidini açma (self servis)|
|Self Servis Parola Yönetimi|Kullanıcı Yönetimi|Kullanıcı|Self servis parola sıfırlama için kaydolan kullanıcı|
|Kullanım Koşulları|İlke Yönetimi|İlke|Kullanım Koşullarını Kabul Etme|
|Kullanım Koşulları|İlke Yönetimi|İlke|Kullanım Koşulları Oluşturma|
|Kullanım Koşulları|İlke Yönetimi|İlke|Kullanım Koşullarını Reddetme|
|Kullanım Koşulları|İlke Yönetimi|İlke|Kullanım Koşullarını Silme|
|Kullanım Koşulları|İlke Yönetimi|İlke|Kullanım Koşullarını Düzenleme|
|Kullanım Koşulları|İlke Yönetimi|İlke|Kullanım Koşullarını Yayımlama|
|Kullanım Koşulları|İlke Yönetimi|İlke|Kullanım Koşullarını Yayımdan Kaldırma|




## <a name="next-steps"></a>Sonraki adımlar

Raporlamaya genel bir bakış için bkz. [Azure Active Directory raporlama](active-directory-reporting-azure-portal.md).

