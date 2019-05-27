---
title: Azure Active Directory otomatik kullanıcı hesabı SaaS uygulamaları için hazırlama raporlama | Microsoft Docs
description: Otomatik kullanıcı hesabı işleri sağlama durumunu denetlemek ve bireysel kullanıcıları sağlama sorunlarını gidermek öğrenin.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.date: 09/09/2018
ms.author: mimart
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 70ca1e2f4fd831619cc3cd443d98018a35f4e1ef
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65963083"
---
# <a name="tutorial-reporting-on-automatic-user-account-provisioning"></a>Öğretici: Raporlama hesabı otomatik kullanıcı hazırlama


Azure Active Directory içeren bir [kullanıcı hesabı sağlama hizmeti](user-provisioning.md) yardımcı sağlama SaaS uygulamaları ve diğer sistemleri, uçtan uca kimlik yaşam döngüsü amacıyla kullanıcı hesaplarına sağlamayı otomatikleştirin yönetimi. Azure AD destekleyen tüm uygulamalar ve sistemler "Öne çıkan uygulamalar" bölümünde bağlayıcılarının sağlama önceden tümleştirilmiş kullanıcı [Azure AD uygulama Galerisi](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps?page=1&subcategories=featured).

Bu makalede, bunlar ayarlanan sonra işleri sağlama durumunu denetleme ve bireysel kullanıcılar ve grupları sağlama sorunlarını giderme açıklanmaktadır.

## <a name="overview"></a>Genel Bakış

Sağlama bağlayıcılar ayarlanır ve kullanılarak yapılandırılan [Azure portalında](https://portal.azure.com), izleyerek [sağlanan belgeler](../saas-apps/tutorial-list.md) desteklenen uygulama için. Yapılandırılmış ve çalışıyor sonra işleri sağlama iki yöntemden biri kullanılarak bildirilebilir:

* **Azure Yönetim Portalı** -bu makalede, öncelikli olarak rapor bilgileri alınırken açıklanır [Azure portalında](https://portal.azure.com), hem bir özet raporu sağlama hem de sağlama ayrıntılı denetim günlükleri için sağlayan bir Belirtilen uygulama.

* **API denetim** -Azure Active Directory sağlama ayrıntılı denetim günlüklerini programlı alınmasını sağlayan denetim API'si de sağlar. Bkz: [Azure Active Directory denetim API Başvurusu](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) bu API'yi kullanarak belirli belgeleri. Bu makalede özellikle API'SİNİN nasıl kullanılacağı ele alınmamaktadır olsa da Denetim günlüğüne kaydedilen olayları sağlama türleri açıklamaktadır.

### <a name="definitions"></a>Tanımlar

Bu makalede, aşağıda tanımlanan aşağıdaki terimler kullanılmaktadır:

* **Kaynak sistem** -gelen sağlama hizmetini Azure AD'ye eşitlenen bir kullanıcı deposu. Azure Active Directory, önceden tümleştirilmiş sağlama bağlayıcılar çoğunu ilişkin kaynak sistemi, ancak bazı özel durumlar (örnek: Workday gelen eşitleme).

* **Hedef sistem** -için sağlama hizmetini Azure AD'ye eşitlenen bir kullanıcı deposu. Bu genellikle bir SaaS uygulamasıdır (örnekler: Salesforce, ServiceNow, G Suite, iş için Dropbox), ancak bazı durumlarda, Active Directory gibi şirket içi bir sistemdeki olabilir (örnek: Workday gelen eşitleme için Active Directory).


## <a name="getting-provisioning-reports-from-the-azure-management-portal"></a>Azure Yönetim Portalı'ndan raporları sağlama alma

Rapor bilgilerini belirli bir uygulama için sağlama almak için başlangıç başlatarak [Azure Yönetim Portalı](https://portal.azure.com) ve kuruluş için sağlama yapılandırılmış uygulamasına göz atma. Örneğin, LinkedIn yükseltmesine kullanıcılara sağlıyorsanız, uygulama ayrıntıları Gezinti yolu şöyledir:

**Azure Active Directory > Kurumsal uygulamalar > tüm uygulamaları > LinkedIn Yükselt**

Buradan, hem sağlama özet raporu hem de sağlama denetim günlüklerine erişmek, her ikisi de aşağıda açıklanmıştır.


## <a name="provisioning-summary-report"></a>Özet raporu sağlama

Sağlama Özet raporunda görünür **sağlama** uygulama sekmesinde için. Bulunur **eşitleme ayrıntıları** altında bölümünde **ayarları**, aşağıdaki bilgileri sağlar:

* Kullanıcıların toplam sayısı ve / grupları eşitlenmemiş ve şu anda kaynak ve hedef sistemleri arasında sağlama kapsamına dahildir.

* Son eşitleme çalıştırıldı. Eşitlemeler düğümlerin her 20-40 dakika sonra bir [ilk eşitleme](user-provisioning.md#what-happens-during-provisioning) tamamlandı.

* Olup olmadığı bir [ilk eşitleme](user-provisioning.md#what-happens-during-provisioning) tamamlandı.

* Sağlama işlemini karantinasında yerleştirilmiş olup olmadığını ve (örneğin, geçersiz yönetici kimlik bilgileri nedeniyle hedef sistemiyle iletişim kurulamıyor) karantina durum nedeni nedir.

Sağlama özet raporu sağlama işin işlem durumunu denetlemek için ilk yerde yöneticileri Ara olmalıdır.

 ![Özet rapor](./media/check-status-user-account-provisioning/summary_report.PNG)

## <a name="provisioning-audit-logs"></a>Sağlama denetim günlükleri
Sağlama hizmeti tarafından gerçekleştirilen tüm etkinlikler görüntülenebilir Azure AD denetim günlükleri kaydedilir **denetim günlükleri** sekmesinde altında **hesap sağlama** kategorisi. Oturum etkinliği olay türleri şunlardır:

* **Olayları içeri aktar** -Azure AD sağlama hizmeti, bir kaynak sistemi veya hedef sistem bir kullanıcının veya grubun ilgili bilgileri alır her zaman bir "Al" olay kaydedilir. Eşitleme sırasında kullanıcılar kaynak sistemden ilk olarak, "olay içeri aktarmanızı gibi" kaydedilen sonuçları ile alınır. Alınan kullanıcı eşleşen kimlikler, de "Al" olaylar olarak kaydedilen sonuçlarla varsa denetlemek için hedef sistemde karşı seçmeleri istenir. Bu olaylar, tüm eşlenen kullanıcı özniteliklerini ve değerlerini sağlama hizmeti olayın zaman Azure AD tarafından görülen kaydedin. 

* **Eşitleme kuralı olayları** - öznitelik eşleme kurallarını sonuçlarına bu olayları bildirmek ve sonra kullanıcı verilerini içeri aktarılan ve kaynak ve hedef sistemlerden değerlendirilen herhangi kapsam oluşturma filtresi yapılandırılmış. Örneğin, bir kullanıcı bir kaynak sistemde sağlama kapsamında kabul ve bu olay kayıtları daha sonra hedef sistemde mevcut değil kabul kullanıcı hedef sistemde sağlanır. 

* **Olay dışarı aktarmanızı** -Azure AD sağlama hizmeti, bir hedef sistem için bir kullanıcı hesabı veya grup nesnesi Yazar her zaman bir "export" olay kaydedilir. Bu olaylar, tüm kullanıcı özniteliklerini ve Olay sırasındaki sağlama hizmetini Azure AD tarafından yazılan değerleri kaydedin. Bir hata olduğunda hedef sistem için kullanıcı hesabını veya grubunu nesne yazarken, burada görüntülenir.

* **Emanet olay işleme** -işlem escrows ortaya sağlama hizmeti bir işlem çalışırken bir hatayla karşılaşırsa ve geri alma aralığı zaman yeniden başlar. Sağlama işlemi yapılan yeniden deneme her zaman bir "emanet" olay kaydedilir.

Olayları tek tek bir kullanıcı için sağlama sırasında baktığımda olaylar normalde şu sırayla gerçekleşir:

1. Olay içeri aktarın: Kullanıcı kaynak sistemden alınır.

2. Olay içeri aktarın: Hedef sistem alınan kullanıcı varlığını denetlemek için sorgulanır.

3. Eşitleme kuralı olay: Kullanıcı verileri kaynak ve hedef sistemde hangi eylemin gerçekleştirileceğini, varsa belirlemek için kapsam belirleme filtrelerini ve yapılandırılmış öznitelik eşleme kurallarını karşı değerlendirilir.

4. Olay dışarı aktarın: Eşitleme kuralı olay eylem olması gerektiğini belirler, sonra eylemin sonuçlarını dışarı aktarma olaya kaydedilir (ekleme, güncelleştirme, silme) gerçekleştirilir.

![Bir Azure AD test kullanıcısı oluşturma](./media/check-status-user-account-provisioning/audit_logs.PNG)


### <a name="looking-up-provisioning-events-for-a-specific-user"></a>Belirli bir kullanıcı için olaylar sağlamayı ayarlama aranıyor

Sağlama denetim günlükleri için en yaygın kullanım örneği, tek bir kullanıcı hesabı sağlama durumunu denetlemektir. Belirli bir kullanıcı için son sağlama olaylarını aramak için:

1. Git **denetim günlükleri** bölümü.

2. Gelen **kategori** menüsünde **hesap sağlama**.

3. İçinde **tarih aralığı** menüsünde, aramak istediğiniz tarih aralığını seçin.

4. İçinde **arama** çubuğunda, aramak istediğiniz kullanıcının kullanıcı kimliği girin. Kimliği değerinin biçimi ne olursa olsun öznitelik eşlemesi yapılandırma (örneğin, userPrincipalName veya çalışan kimlik numarası) birincil eşleşen kimliği olarak seçtiğiniz eşleşmesi gerekir. Gerekli kimlik değerini hedef sütununda görünür.

5. Aramak için Enter tuşuna basın. En son sağlama olayların ilk döndürülür.

6. Olayları döndürülürse, etkinlik türleri ve bunların başarılı veya başarısız oldu unutmayın. Hiçbir sonuç döndürmezse, ardından bu kullanıcı yok veya tam bir eşitleme henüz tamamlanmadı, henüz Hazırlama işlemi tarafından algılanmadığı anlamına gelir.

7. Olayları tek tek alınan, değerlendirilen veya olay bir parçası olarak yazılan tüm kullanıcı özelliklerini de dahil olmak üzere ilave ayrıntıları görüntülemek için tıklayın.

Denetim günlüklerini nasıl kullanılacağına ilişkin bir örnek için aşağıdaki videoya bakın. Denetim günlüklerini 5:30 geçici olarak sunulan işaretleyin:

> [!VIDEO https://www.youtube.com/embed/pKzyts6kfrw]

### <a name="tips-for-viewing-the-provisioning-audit-logs"></a>Sağlama denetim günlüklerini görüntülemek için ipuçları

Azure portalında en iyi okunabilirlik açısından seçin **sütunları** düğmesine tıklayın ve bu sütunları seçin:

* **Tarih** -olayın gerçekleştiği tarih gösterilir.
* **Listelenebilir** -olayın konular uygulama adı ve kullanıcı Kimliğini gösterir.
* **Etkinlik** -daha önce açıklandığı gibi etkinlik türü.
* **Durum** - olay veya başarılı olup olmadığını.
* **Durum nedeni** -sağlama durumunda ne bir özeti.


## <a name="troubleshooting"></a>Sorun giderme

Sağlama özet raporu ve Denetim günlükleri, yöneticiler çeşitli kullanıcı hesabı sağlama sorunlarını gidermenize yardımcı olacak önemli bir rol oynar.

Senaryo tabanlı otomatik kullanıcı hazırlama sorunlarını giderme konusunda yönergeler için bkz. [yapılandırmak ve uygulamaya kullanıcı hazırlama sorunlarını](application-provisioning-config-problem.md).


## <a name="additional-resources"></a>Ek Kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](what-is-single-sign-on.md)
