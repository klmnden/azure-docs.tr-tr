---
title: "Azure Active Directory portalındaki denetim etkinlik raporları - önizleme | Microsoft Docs"
description: "Azure Active Directory portalı önizleme sürümündeki denetim etkinlik raporlarına giriş"
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
ms.date: 04/07/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 757d6f778774e4439f2c290ef78cbffd2c5cf35e
ms.openlocfilehash: aef5bce6f440f4a0a57763f915d307297f50281b
ms.lasthandoff: 04/10/2017


---
# <a name="audit-activity-reports-in-the-azure-active-directory-portal---preview"></a>Azure Active Directory portalındaki denetim etkinlik raporları - önizleme

Azure Active Directory [önizleme sürümündeki](active-directory-preview-explainer.md) raporlama ile ortamınızın nasıl çalıştığını belirlemek için gereken tüm bilgileri edinirsiniz.

Azure Active Directory'nin raporlama mimarisi aşağıdaki bileşenlerden oluşur:

- **Etkinlik** 
    - **Oturum açma etkinlikleri**: Yönetilen uygulamaların kullanımı ve kullanıcıların oturum açma etkinlikleri hakkında bilgiler
    - **Denetim günlükleri**: Kullanıcılar ve grup yönetimi, yönetilen uygulamalarınız ve dizin etkinlikleriniz hakkında sistem etkinliği bilgileri.
- **Güvenlik** 
    - **Riskli oturum açma işlemleri** - Riskli oturum açma işlemi bir kullanıcı hesabının meşru sahibi olmayan bir kişi tarafından gerçekleştirilmiş olabilecek oturum açma girişiminin göstergesidir. Daha fazla bilgi için bkz. Riskli oturum açma işlemleri.
    - **Riskli oldukları belirlenen kullanıcılar** - Riskli kullanıcı, güvenliği tehlikeye girmiş olabilecek bir kullanıcı hesabının göstergesidir. Daha fazla bilgi için bkz. Riskli oldukları belirlenen kullanıcılar.

Bu konu başlığı denetim etkinliklerine genel bakış sunmaktadır.
 


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
- Başlatan
- Kategori
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
- Hesap sağlama
- Otomatik parola geçişi
- Davet edilen kullanıcılar
- MIM hizmeti

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

| Etkinlik Kategorisi| Etkinlik Kaynağı Türü| Etkinlik |
| :-- | :-: | :-- |
| Çekirdek Dizin| Grup| Grup Ayarlarını Silme|
| Çekirdek Dizin| Dizin| Güncelleme Etki Alanı|
| Çekirdek Dizin| Dizin| İş Ortağını Şirketten Kaldırma|
| Çekirdek Dizin| Kullanıcı| Rolü Güncelleştirme|
| Çekirdek Dizin| Kullanıcı| Şablondan Rol Ekleme|
| Çekirdek Dizin| Grup| Gruba Uygulama Rolü Ataması Ekleme|
| Çekirdek Dizin| Grup| Kullanıcılara Grup Tabanlı Lisans Uygulamaya Başlama|
| Çekirdek Dizin| Uygulama| Hizmet Sorumlusu Ekleme|
| Çekirdek Dizin| İlke| İlkeyi Güncelleştirme|
| Çekirdek Dizin| İlke| Hizmet Sorumlusuna İlke Ekleme|
| Çekirdek Dizin| Cihaz| Cihaza Kayıtlı Sahip Ekleme|
| Çekirdek Dizin| Cihaz| Cihaza Kayıtlı Kullanıcılar Ekleme|
| Çekirdek Dizin| Cihaz| Cihaz Yapılandırmasını Güncelleştirme|
| Self Servis Parola Yönetimi| Kullanıcı| Parola Sıfırlama (Self Servis)|
| Self Servis Parola Yönetimi| Kullanıcı| Kullanıcı Hesabının Kilidini Açma (Self Servis)|
| Self Servis Parola Yönetimi| Kullanıcı| Parola Sıfırlama (Yönetici Tarafından)|
| Self Servis Grup Yönetimi| Grup| Onay Bekleyen Bir Gruba Katılma İsteğini Silme|
| Hesap Sağlama| Uygulama| Emanet İşleme|
| Otomatik Parola Geçişi| Uygulama| Otomatik Parola Geçişi|
| Davetli Kullanıcılar| Diğer| İşlenen Toplu Davetler|
| Çekirdek Dizin| Dizin| Doğrulanmamış Etki Alanını Kaldırma|
| Çekirdek Dizin| Dizin| Doğrulanmamış Etki Alanı Ekleme|
| Çekirdek Dizin| Dizin| Doğrulanmış Etki Alanı Ekleme|
| Çekirdek Dizin| Dizin| Kiracıda Dizin Özelliğini Ayarlama|
| Çekirdek Dizin| Dizin| Dirsyncenabled Bayrağını Ayarlama|
| Çekirdek Dizin| Dizin| Şirket Ayarları Oluşturma|
| Çekirdek Dizin| Dizin| Şirket Ayarlarını Güncelleştirme|
| Çekirdek Dizin| Dizin| Şirket Ayarlarını Silme|
| Çekirdek Dizin| Dizin| Şirket Tarafından İzin Verilen Veri Konumunu Ayarlama|
| Çekirdek Dizin| Dizin| Şirket Çok Uluslu Özelliğini Etkin Olarak Ayarlama|
| Çekirdek Dizin| Kullanıcı| Kullanıcıyı Güncelleştirme|
| Çekirdek Dizin| Kullanıcı| Kullanıcıyı Silme|
| Çekirdek Dizin| Grup| Gruptan Üye Kaldırma|
| Çekirdek Dizin| Grup| Grup Lisansı Ayarlama|
| Çekirdek Dizin| Grup| Grup Ayarlarını Oluşturma|
| Çekirdek Dizin| Uygulama| Hizmet Sorumlusunu Güncelleştirme|
| Çekirdek Dizin| Uygulama| Uygulamayı Silme|
| Çekirdek Dizin| Uygulama| Uygulamayı Güncelleştirme|
| Çekirdek Dizin| Uygulama| Hizmet Sorumlusunu Kaldırma|
| Çekirdek Dizin| Uygulama| Hizmet Sorumlusu Kimlik Bilgileri Ekleme|
| Çekirdek Dizin| Uygulama| Hizmet Sorumlusundan Uygulama Rol Atamasını Kaldırma|
| Çekirdek Dizin| Uygulama| Sahibi Uygulamadan Kaldırma|
| Çekirdek Dizin| Cihaz| Kayıtlı Sahibi Cihazdan Kaldırma|
| Self Servis Parola Yönetimi| Kullanıcı| Self Servis Parola Sıfırlama Akış Etkinliği İlerleme Durumu|
| Hesap Sağlama| Uygulama| Yönetim|
| Hesap Sağlama| Uygulama| Dizin İşlemi|
| MIM Hizmeti| Grup| Üye Kaldırma|
| Çekirdek Dizin| İlke| İlke Silme|
| Davetli Kullanıcılar| Kullanıcı| Virüslü Kiracı Oluşturma|
| Çekirdek Dizin| Dizin| Dış Gizli Anahtarları Güncelleştirme|
| Çekirdek Dizin| Dizin| Rights Management Özelliklerini Ayarlama|
| Çekirdek Dizin| Dizin| Şirketi Güncelleştirme|
| Çekirdek Dizin| Kullanıcı| Kullanıcı Ekleme|
| Çekirdek Dizin| Kullanıcı| Federasyon Kullanıcısını Yönetilen Kullanıcıya Dönüştürme|
| Çekirdek Dizin| Kullanıcı| Kullanıcı için Uygulama Parolası Oluşturma|
| Çekirdek Dizin| Grup| Gruba Üye Ekleme|
| Çekirdek Dizin| Grup| Grup Ekleme|
| Çekirdek Dizin| Uygulama| Uygulama Onayı|
| Çekirdek Dizin| Uygulama| Uygulama Ekleme|
| Çekirdek Dizin| Uygulama| Hizmet Sorumlusuna Sahip Ekleme|
| Çekirdek Dizin| Uygulama| Oauth2Permissiongrant’i Kaldırma|
| Çekirdek Dizin| İlke| İlke Kimlik Bilgilerini Kaldırma|
| Çekirdek Dizin| Cihaz| Cihaz Yapılandırmasını Silme|
| Self Servis Grup Yönetimi| Grup| Dinamik Grup Özelliklerini Ayarlama|
| Self Servis Grup Yönetimi| Grup| Yaşam Döngüsü Yönetim İlkesini Güncelleştirme|
| Hesap Sağlama| Uygulama| Eşitleme Kuralı Eylemi|
| Davetli Kullanıcılar| Diğer| Karşıya Yüklenen Toplu Davetler|
| MIM Hizmeti| Grup| Üye Ekleme|
| Çekirdek Dizin| Kullanıcı| Lisans Özelliklerini Ayarlama|
| Çekirdek Dizin| Kullanıcı| Kullanıcıyı Geri Yükleme|
| Çekirdek Dizin| Kullanıcı| Rolden Üye Kaldırma|
| Çekirdek Dizin| Kullanıcı| Kullanıcıdan Uygulama Rol Atamasını Kaldırma|
| Çekirdek Dizin| Kullanıcı| Rolden Kapsamlı Üye Kaldırma|
| Çekirdek Dizin| Grup| Grubu Güncelleştirme|
| Çekirdek Dizin| Grup| Gruba Sahip Ekleme|
| Çekirdek Dizin| Grup| Kullanıcılara Grup Tabanlı Lisans Uygulamayı Sonlandırma|
| Çekirdek Dizin| Grup| Gruptan Uygulama Rol Atamasını Kaldırma|
| Çekirdek Dizin| Grup| Grubu Kullanıcı Tarafından Yönetilecek Şekilde Ayarlama|
| Çekirdek Dizin| Uygulama| Oauth2Permissiongrant ekleme|
| Çekirdek Dizin| Uygulama| Hizmet Sorumlusuna Uygulama Rol Ataması Ekleme|
| Çekirdek Dizin| Uygulama| Hizmet Sorumlusu Kimlik Bilgilerini Kaldırma|
| Çekirdek Dizin| İlke| Hizmet Sorumlusundan İlke Kaldırma|
| Çekirdek Dizin| Cihaz| Cihazı Güncelleştirme|
| Çekirdek Dizin| Cihaz| Cihaz Ekleme|
| Çekirdek Dizin| Cihaz| Cihaz Yapılandırması Ekleme|
| Self Servis Parola Yönetimi| Kullanıcı| Parolayı Değiştirme (Self Servis)|
| Self Servis Parola Yönetimi| Kullanıcı| Self Servis Parola Sıfırlama için Kaydolan Kullanıcı|
| Self Servis Grup Yönetimi| Grup| Onay Bekleyen Bir Gruba Katılma İsteğini Onaylama|
| Çekirdek Dizin| Dizin| Doğrulanmamış Etki Alanını Kaldırma|
| Çekirdek Dizin| Dizin| Etki Alanını Doğrulama|
| Çekirdek Dizin| Dizin| Etki Alanı Kimlik Doğrulaması Ayarlama|
| Çekirdek Dizin| Dizin| Parola İlkesi Ayarlama|
| Çekirdek Dizin| Dizin| Şirkete İş Ortağı Ekleme|
| Çekirdek Dizin| Dizin| Şirketi İş Ortağına Yükseltme|
| Çekirdek Dizin| Dizin| İş Ortaklığı Ayarlama|
| Çekirdek Dizin| Dizin| Yanlışlıkla Silme Eşiği Ayarlama|
| Çekirdek Dizin| Dizin| İş Ortağını İndirgeme|
| Davetli Kullanıcılar| Kullanıcı| Dış Kullanıcı Davet Etme|
| Hesap Sağlama| Uygulama| İçeri Aktarma|
| Çekirdek Dizin| Uygulama| Hizmet Sorumlusundan Sahibi Kaldırma|
| Çekirdek Dizin| Cihaz| Cihazdan Kayıtlı Kullanıcıları Kaldırma|
| Çekirdek Dizin| Dizin| Şirket Bilgilerini Ayarlama|
| Çekirdek Dizin| Dizin| Etki Alanında Federasyon Ayarlarını Belirleme|
| Çekirdek Dizin| Dizin| Şirket Oluşturma|
| Çekirdek Dizin| Dizin| Rights Management Özelliklerini Temizleme|
| Çekirdek Dizin| Dizin| Dirsync Özelliğini Ayarlama|
| Çekirdek Dizin| Dizin| E-posta ile Doğrulanmış Etki Alanını Doğrulama|
| Çekirdek Dizin| Kullanıcı| Kullanıcı Lisansını Değiştirme|
| Çekirdek Dizin| Kullanıcı| Kullanıcı Parolasını Değiştirme|
| Çekirdek Dizin| Kullanıcı| Kullanıcı Parolasını Sıfırlama|
| Çekirdek Dizin| Kullanıcı| Kullanıcıya Uygulama Rolü Ataması İzni Ekleme|
| Çekirdek Dizin| Kullanıcı| Role Üye Ekleme|
| Çekirdek Dizin| Kullanıcı| Kullanıcı için Uygulama Parolasını Silme|
| Çekirdek Dizin| Kullanıcı| Kullanıcı Kimlik Bilgilerini Güncelleştirme|
| Çekirdek Dizin| Kullanıcı| Kullanıcı Yöneticisini Ayarlama|
| Çekirdek Dizin| Kullanıcı| Role Kapsamlı Üye Ekleme|
| Çekirdek Dizin| Grup| Grubu Silme|
| Çekirdek Dizin| Grup| Sahibi Gruptan Kaldırma|
| Çekirdek Dizin| Grup| Grup Ayarlarını Güncelleştirme|
| Çekirdek Dizin| Uygulama| Uygulamaya Sahip Ekleme|
| Çekirdek Dizin| Uygulama| Onayı İptal Etme|
| Çekirdek Dizin| İlke| İlke Ekleme|
| Çekirdek Dizin| Cihaz| Cihaz Silme|
| Self Servis Parola Yönetimi| Kullanıcı| Self Servis Parola Sıfırlaması Engellendi|
| Self Servis Grup Yönetimi| Grup| Bir Gruba Katılma İsteğinde Bulunma|
| Self Servis Grup Yönetimi| Grup| Yaşam Döngüsü Yönetim İlkesi Oluşturma|
| Self Servis Grup Yönetimi| Grup| Onay Bekleyen Bir Gruba Katılma İsteğini Reddetme|
| Self Servis Grup Yönetimi| Grup| Onay Bekleyen Bir Gruba Katılma İsteğini İptal Etme|
| Self Servis Grup Yönetimi| Grup| Grubu Yenileme|
| Hesap Sağlama| Uygulama| Dışarı Aktarma|
| Hesap Sağlama| Uygulama| Diğer|
| Davetli Kullanıcılar| Kullanıcı| Dış Kullanıcı Davetini Kullanma|
| Davetli Kullanıcılar| Kullanıcı| Virüslü Kullanıcı Oluşturma|
| Davetli Kullanıcılar| Kullanıcı| Uygulamaya Dış Kullanıcı Atama|




## <a name="audit-logs-shortcuts"></a>Denetim günlükleri kısayolları

Azure portalı, **Azure Active Directory**’ye ek olarak verileri denetlemeniz için fazladan iki giriş noktası sağlar:

- Kullanıcılar ve gruplar
- Kurumsal uygulamalar

Denetim rapor etkinliklerinin tam listesi için [denetim raporu olaylarının listesi](active-directory-reporting-audit-events.md#list-of-audit-report-events) bölümüne bakın.


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

Yalnızca uygulamalarınızla ilgili denetim verilerini gözden geçirmek istiyorsanız, **Kurumsal uygulamalar** dikey penceresinin **Etkinlik** bölümündeki **Denetim günlükleri** altında filtrelenmiş bir görünüm bulabilirsiniz. Bu giriş noktasında, **Etkinlik Kaynağı Türü** olarak **Uygulama** önceden seçilidir.

![Denetim günlükleri](./media/active-directory-reporting-activity-audit-logs/134.png "Denetim günlükleri")

Bu görünümü yalnızca **grupları** veya yalnızca **kullanıcıları** içerecek şekilde filtreleyebilirsiniz.

![Denetim günlükleri](./media/active-directory-reporting-activity-audit-logs/25.png "Denetim günlükleri")


## <a name="next-steps"></a>Sonraki adımlar
Bkz. [Azure Active Directory Raporlama Kılavuzu](active-directory-reporting-guide.md).


