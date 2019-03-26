---
title: Azure portalında Azure Active Directory Kullanıcı Etkinlik raporlarını bulma | Microsoft Docs
description: Azure Active Directory kullanıcı etkinlik raporları Azure portalında nerede olduğunu öğrenin.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: d47072713c57576abe780134792c3a5cbc27127c
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58439048"
---
# <a name="find-activity-reports-in-the-azure-portal"></a>Azure portalında Etkinlik raporlarını bulma

Bu makalede, Azure portalında Azure Active Directory (Azure AD) Kullanıcı Etkinlik raporlarını bulma hakkında bilgi edinin.

## <a name="audit-logs-report"></a>Denetim günlükleri raporu

Denetim günlükleri raporu Bağlam tabanlı raporlama için tek bir görünümde uygulama etkinliklerini geçici olarak çeşitli raporlar birleştirir. Denetim günlükleri raporuna erişebilmek için:

1. [Azure portalına](https://portal.azure.com) gidin.
2. Sağ üst köşeden dizininizi seçin ve ardından **Azure Active Directory** dikey penceresinde sol gezinti bölmesinden.
3. Seçin **denetim günlükleri** gelen **etkinlik** Azure Active Directory dikey bölümü. 

    ![Denetim günlükleri](./media/howto-find-activity-reports/482.png "Denetim günlükleri")

Denetim günlükleri raporu, aşağıdaki raporları birleştirir:

* Denetim raporu
* Parola sıfırlama etkinliği
* Parola sıfırlama kaydı etkinliği
* Self Servis Grup etkinliği
* Office 365 grubunu silmiş ad değişiklikleri
* Hesap sağlama etkinliği
* Parola geçişi durumu
* Hesap hazırlama hataları

### <a name="filtering-on-audit-logs"></a>Denetim günlüklerini filtreleme

Denetim verilerinin belirli bir kategorisi içinde belirterek erişmek için Denetim raporuna Gelişmiş filtreleme kullanabileceğiniz **kategori** filtre. Örneğin, kullanıcılarla ilgili tüm etkinlikleri görüntülemek için seçin **UserManagement** kategorisi. 

Kategorileri şunlardır:

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

Bir hizmete kullanarak filtreleyebilirsiniz **hizmet** filtre açılır. Örneğin, Self Servis parola yönetimi ile ilgili tüm denetim olaylarını almak için seçin **Self Servis parola yönetimi** filtre.

Hizmetlere şunlar dahildir:

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
- Self Servis Parola Yönetimi
- Kullanım Koşulları

## <a name="sign-ins-report"></a>Oturum açma işlemleri raporu 

**Oturum açma işlemleri** görünüm dahil tüm kullanıcı oturum açma işlemleri, hem de **uygulama kullanımı** rapor. Uygulama kullanım bilgileri de görüntüleyebilirsiniz **Yönet** bölümünü **kurumsal uygulamalar** genel bakış.

Oturum açma işlemleri raporu erişmek için:

1. [Azure portalına](https://portal.azure.com) gidin.
2. Sağ üst köşeden dizininizi seçin ve ardından **Azure Active Directory** dikey penceresinde sol gezinti bölmesinden.
3. Seçin **oturum açma bilgilerini** gelen **etkinlik** Azure Active Directory dikey bölümü. 

    ![Oturum açma görünümüne](./media/howto-find-activity-reports/483.png "oturum açmaları görüntüleyin")


### <a name="filtering-on-application-name"></a>Uygulama adı filtresi

Kullanıcı adı veya uygulama adı üzerinde filtreleme yaparak, uygulama kullanımını hakkındaki ayrıntıları görüntülemek için oturum açma işlemleri raporu kullanabilirsiniz.

![Filtre olayları oturum açma sayfası](./media/howto-find-activity-reports/07.png "filtre olayları, oturum açma sayfası")

## <a name="security-reports"></a>Güvenlik raporları

### <a name="anomalous-activity-reports"></a>Anormal etkinlik raporları

Anormal Etkinlik raporlarını, Azure AD algılayan ve rapor güvenlikle ilgili risk olayları hakkında bilgi sağlar.

Aşağıdaki tablo liste Azure AD anormal etkinlik güvenlik raporları ve Azure portalında ilgili risk olayı türleri. Daha fazla bilgi için bkz. [Azure Active Directory risk olayları](concept-risk-events.md).  


| Azure AD anormal Etkinlik Raporu |  Kimlik koruması risk olayı türü|
| :--- | :--- |
| Sızan kimlik bilgilerine sahip kullanıcılar | Sızdırılan kimlik bilgileri |
| Düzensiz oturum açma etkinliği | Alışılmadık konumlara imkansız seyahat |
| Muhtemelen virüs bulaşmış cihazlardan gerçekleştirilen oturum açma işlemleri | Bulaşma olan cihazlardan oturum açma işlemleri|
| Bilinmeyen kaynaklardan gerçekleştirilen oturum açma işlemleri | Anonim IP adreslerinden oturum açma işlemleri |
| Şüpheli etkinlik gösteren IP adreslerinden gerçekleştirilen oturum açma işlemleri | Şüpheli etkinlik gösteren IP adreslerinden gerçekleştirilen oturum açma işlemleri |
| - | Alışılmadık konumlardan oturum açma işlemleri |

Aşağıdaki Azure AD anormal etkinlik güvenlik raporları olarak Azure portalında risk olayları dahil değildir:

* Birden çok hatadan sonra gerçekleştirilen oturum açma işlemleri
* Birden çok coğrafyadan gerçekleştirilen oturum açma işlemleri


### <a name="detected-risk-events"></a>Algılanan risk olayları

Algılanan risk olayları hakkında rapor erişebileceğiniz **güvenlik** bölümünü **Azure Active Directory** dikey penceresinde [Azure portalında](https://portal.azure.com). Algılanan risk olayları aşağıdaki raporlarda izlenir:   

- [Risk altındaki kullanıcılar](concept-user-at-risk.md)
- [Riskli oturum açma işlemleri](concept-risky-sign-ins.md)

    ![Güvenlik raporları](./media/howto-find-activity-reports/04.png "güvenlik raporları")

## <a name="troubleshoot-issues-with-activity-reports"></a>Etkinlik raporları ile ilgili sorunları giderme

### <a name="missing-data-in-the-downloaded-activity-logs"></a>İndirilen etkinlik günlüklerindeki eksik veriler

#### <a name="symptoms"></a>Belirtiler 

Etkinlik günlüklerini (denetim veya oturum açma) indirdim ve seçtiğim süre için tüm kayıtları göremiyorum. Neden? 

 ![Raporlama](./media/troubleshoot-missing-data-download/01.png)
 
#### <a name="cause"></a>Nedeni

Azure Portal'da etkinlik günlüklerini indirdiğinizde ölçek en son gerçekleşen en başta tarafından sıralanan 250000 kayıtlara sınırlıyoruz. 

#### <a name="resolution"></a>Çözüm

Belirli bir noktadaki bir milyon kaydı getirmek için [Azure AD Raporlama API’lerini](concept-reporting-api.md) kullanabilirsiniz.

### <a name="missing-audit-data-for-recent-actions-in-the-azure-portal"></a>Azure portalında son eylemleri için denetim veri eksik

#### <a name="symptoms"></a>Belirtiler

Azure portalında bazı eylemler gerçekleştirdim ve bu eylemlerin denetim günlüklerini `Activity logs > Audit Logs` dikey penceresinde görmeyi umuyordum, ancak bulamıyorum.

 ![Raporlama](./media/troubleshoot-missing-audit-data/01.png)
 
#### <a name="cause"></a>Nedeni

Eylemler, etkinlik günlüklerinde hemen görünmez. Aşağıdaki tabloda etkinlik günlüklerinin gecikme süreleri gösterilmiştir. 

| Rapor | &nbsp; | Gecikme süresi (P95) | Gecikme süresi (P99) |
|--------|--------|---------------|---------------|
| Dizin denetimi | &nbsp; | 2 dk. | 5 dk. |
| Oturum açma etkinliği | &nbsp; | 2 dk. | 5 dk. | 

#### <a name="resolution"></a>Çözüm

15 dakika ile iki saat arasında bekleyin ve eylemlerin günlükte görüntülenip görüntülenmediğine bakın. İki saatten sonra da günlükler görünmüyorsa sorunla ilgilenebilmemiz için [destek bileti oluşturun](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

### <a name="missing-logs-for-recent-user-sign-ins-in-the-azure-ad-sign-ins-activity-log"></a>Azure AD oturum açma işlemleri etkinlik için son kullanıcı oturum açma işlemleri için eksik günlükleri oturum

#### <a name="symptoms"></a>Belirtiler

Azure portalda kısa bir süre önce oturum açtım ve bu oturum açma işleminin günlük girişlerini `Activity logs > Sign-ins` dikey penceresinde görmeyi umuyordum, ancak bulamıyorum.

 ![Raporlama](./media/troubleshoot-missing-audit-data/02.png)
 
#### <a name="cause"></a>Nedeni

Eylemler, etkinlik günlüklerinde hemen görünmez. Aşağıdaki tabloda etkinlik günlüklerinin gecikme süreleri gösterilmiştir. 

| Rapor | &nbsp; | Gecikme süresi (P95) | Gecikme süresi (P99) |
|--------|--------|---------------|---------------|
| Dizin denetimi | &nbsp; | 2 dk. | 5 dk. |
| Oturum açma etkinliği | &nbsp; | 2 dk. | 5 dk. | 

#### <a name="resolution"></a>Çözüm

15 dakika ile iki saat arasında bekleyin ve eylemlerin günlükte görüntülenip görüntülenmediğine bakın. İki saatten sonra da günlükler görünmüyorsa sorunla ilgilenebilmemiz için [destek bileti oluşturun](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

### <a name="i-cant-view-more-than-30-days-of-report-data-in-the-azure-portal"></a>Azure portalda 30 günden daha eski rapor verilerini görüntüleyemiyorum

#### <a name="symptoms"></a>Belirtiler

Azure portalda 30 günden daha eski oturum açma ve denetim verilerini görüntüleyemiyorum. Neden? 

 ![Raporlama](./media/troubleshoot-missing-audit-data/03.png)

#### <a name="cause"></a>Nedeni

Lisansınıza bağlı olarak, etkinlik raporları Azure Active Directory Actions tarafından aşağıdaki sürelerde depolanır:

| Rapor           | &nbsp; |  Azure AD Ücretsiz | Azure AD Premium P1 | Azure AD Premium P2 |
| ---              | ----   |  ---           | ---                 | ---                 |
| Dizin Denetimi  | &nbsp; |   7 gün     | 30 gün             | 30 gün             |
| Oturum Açma Etkinliği | &nbsp; | Kullanılamıyor. Kendi oturum açma etkinliklerinize bireysel kullanıcı profili dikey penceresinden 7 gün boyunca erişebilirsiniz | 30 gün | 30 gün             |

Daha fazla bilgi için bkz. [Azure Active Directory rapor bekletme ilkeleri](reference-reports-data-retention.md).  

#### <a name="resolution"></a>Çözüm

Verileri 30 günden daha uzun bir süre boyunca saklamak için iki seçeneğiniz vardır. [Azure AD Raporlama API'lerini](concept-reporting-api.md) kullanarak verileri program aracılığıyla alabilir ve bir veritabanında kaydedebilirsiniz. Alternatif olarak denetim günlüklerini Splunk veya SumoLogic gibi bir üçüncü taraf SIEM sistemiyle tümleştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Denetim günlükleri'ne genel bakış](concept-audit-logs.md)
* [Oturum açma işlemleri genel bakış](concept-sign-ins.md)
* [Riskli olaylara genel bakış](concept-risk-events.md)
