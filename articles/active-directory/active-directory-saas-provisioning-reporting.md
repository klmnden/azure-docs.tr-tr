---
title: Azure Active Directory'ye otomatik olarak bir kullanıcı hesabı SaaS uygulamaları için sağlama üzerinde raporlama | Microsoft Docs
description: İşlerini sağlama otomatik olarak bir kullanıcı hesabı durumunu denetlemek nasıl, bireysel kullanıcılar sağlama ile ilgili sorunları giderme öğrenin.
services: active-directory
documentationcenter: ''
author: asmalser-msft
writer: asmalser-msft
manager: mtillman
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: asmalser-msft
ms.openlocfilehash: faccaa4496eb1deda23bbfcf335088a023d229d6
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35293186"
---
# <a name="tutorial-reporting-on-automatic-user-account-provisioning"></a>Öğretici: otomatik olarak bir kullanıcı hesabı sağlama raporlama


Azure Active Directory içeren bir [hizmet sağlama kullanıcı hesabı](active-directory-saas-app-provisioning.md) yardımcı sağlama SaaS uygulamaları ve diğer sistemler uçtan uca kimlik yaşam döngüsü amacıyla kullanıcı hesaplarının sağlamayı kaldırma özelliklerini otomatikleştirme yönetimi. Azure AD destekleyen tüm uygulamalar ve sistemler "Öne çıkan" bölümündeki bağlayıcılarının sağlama önceden tümleştirilmiş kullanıcı [Azure AD uygulama galerisinde](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/azure-active-directory-apps?page=1&subcategories=featured).

Bu makalede, bunlar ayarlanan sonra işleri sağlama durumunu kontrol etme ve tek tek kullanıcılar ve gruplar sağlama ile ilgili sorunları giderme açıklanmaktadır.

## <a name="overview"></a>Genel Bakış

Sağlama bağlayıcılar öncelikle ayarlanır ve kullanılarak yapılandırılan [Azure Yönetim Portalı](https://portal.azure.com), izleyerek [belgelerine sağlanan](active-directory-saas-tutorial-list.md) burada kullanıcı hesabı sağlama, uygulama için İstenen. Yapılandırılmış ve çalışan sonra bir uygulama için işleri sağlama iki yöntemden birini kullanarak bildirilebilir:

* **Azure Yönetim Portalı** -bu makalede, öncelikle rapor bilgilerini alma açıklanır [Azure Yönetim Portalı](https://portal.azure.com), hem bir sağlama özet raporu yanı sıra sağlama ayrıntılı denetim sağlar belirli bir uygulamada günlükleri.

* **API denetim** -Azure Active Directory ayrıca bir denetim ayrıntılı sağlama denetim günlüklerini programlı alınmasını sağlayan API sağlar. Bkz: [Azure Active Directory denetim API Başvurusu](active-directory-reporting-api-audit-reference.md) bu API'yi kullanarak belirli belgeleri için. Bu makalede API kullanma özellikle kapsamaz olsa da, Denetim günlüğüne kaydedilen olayları sağlama türleri detaylandırır.

### <a name="definitions"></a>Tanımlar

Bu makalede aşağıda tanımlanan aşağıdaki terimler kullanır:

* **Kaynak sistem** -hizmet sağlama Azure AD alanından eşitler kullanıcı deposu. Azure Active Directory kaynak sistem çoğunluğu için bağlayıcıları sağlama önceden tümleştirilmiştir, ancak kopyalanamayan bazı özel durumlar (örnek: Workday gelen eşitleme).

* **Hedef sistem** -için hizmet sağlama Azure AD eşitler kullanıcı deposu. Bu genellikle bir SaaS uygulaması olur (örnek: Salesforce, ServiceNow, Google Apps, iş için Dropbox), ancak bazı durumlarda Active Directory gibi şirket içi sistem olabilir (örnek: Active Directory'ye Workday gelen eşitleme).


## <a name="getting-provisioning-reports-from-the-azure-management-portal"></a>Azure Yönetim Portalı'ndan raporları sağlama alma

Başlatarak sağlama belirli bir uygulamada rapor bilgilerini almak için başlangıç [Azure Yönetim Portalı](https://portal.azure.com) ve kuruluş için sağlama yapılandırılmış bir uygulama için gözatma. Örneğin, LinkedIn yükseltmesine kullanıcılara sağlama, uygulama ayrıntıları Gezinti yolu şöyledir:

**Azure Active Directory > Kurumsal uygulamalar > tüm uygulamaları > LinkedIn Yükselt**

Buradan, hazırlama özet raporu hem sağlama denetim günlüklerini erişebilir, her ikisi de aşağıda açıklanmıştır.


### <a name="provisioning-summary-report"></a>Sağlama özet raporu

Sağlama özet raporu görünür **sağlama** uygulama sekmesinde için. Eşitleme ayrıntıları bölümü altında bulunan **ayarları**ve aşağıdaki bilgileri sağlar:

* Toplam sayısı, kullanıcı ve / grupları, eşitlenmemiş ve şu anda kapsamında kaynak ve hedef sistemleri arasında sağlama.

* En son ne zaman eşitleme çalıştırıldı. Eşitlemeler genellikle 20-40 tam eşitleme tamamlandıktan sonra dakikada oluşur.

* Desteklemediğini ilk tam Eşitleme tamamlandı.

* Sağlama işlemini Karantinadaki yerleştirilmiş olup olmadığına ve karantina durum nedeni örn (geçersiz yönetici kimlik bilgileri nedeniyle hedef sistemiyle iletişim kurulamıyor) nedir

Sağlama özet raporu sağlama işi işletimsel durumunu denetlemek için ilk yer admins görünüm olmalıdır.

 ![Özet rapor](./media/active-directory-saas-provisioning-reporting/summary_report.PNG)

### <a name="provisioning-audit-logs"></a>Sağlama denetim günlükleri
Sağlama hizmeti tarafından gerçekleştirilen tüm etkinlikler görüntülenebilir Azure AD denetim günlüklerine kaydedilir **denetim günlüklerini** altında sekmesinde **hesap sağlama** kategorisi. Günlüğe kaydedilen etkinlik olay türleri şunlardır:

* **Olayları alma** -hizmet sağlama Azure AD, bir kaynak sistemi veya hedef sistem tek bir kullanıcı veya grup hakkında bilgi alır her zaman bir "alma" olayı kaydedilir. Eşitleme sırasında kullanıcılar kaynak sistemden ilk olarak, "olayları Al"olarak kaydedilen sonuçlarıyla alınır. Alınan kullanıcı eşleşen kimlikleri, de "Al" olayları olarak kaydedilen sonuçlarla varsa denetlemek için hedef sistemine karşı seçmeleri istenir. Bu olaylar, tüm eşlenen kullanıcı öznitelikleri ve olay aynı anda hizmet sağlama Azure AD tarafından görülen değerlerine kaydeder. 

* **Eşitleme kuralı olayları** - bu olayları özniteliği eşleme kurallarını sonuçlarını rapor ve kullanıcı verilerini içe ve kaynak ve hedef sistemlerden hesaplanan sonra herhangi bir kapsam filtreleri, yapılandırılmış. Örneğin, bir kullanıcı bir kaynak sistemde sağlama için kapsam olarak kabul ve hedef sistemde mevcut değil, bu olay kayıtlarını sonra kabul kullanıcı hedef sistemde sağlanacak. 

* **Olay Ver** -hizmet sağlama Azure AD hedef sistem için bir kullanıcı hesabı veya grup nesnesi Yazar her zaman "export" olay kaydedilir. Bu olaylar, tüm kullanıcı özniteliklerini ve Azure AD tarafından hizmet olayı aynı anda sağlama yazılmış değerleri kaydedin. Oluştu hata oluşursa hedef sisteme kullanıcı hesabı veya grup nesnesi yazılırken, burada görüntülenir.

* **İşlem emanet olayları** -işlem escrows ortaya sağlama hizmeti bir işlem çalışırken bir hatayla karşılaşırsa ve bir geri alma aralığı süre işlemi yeniden deneyin başlar. Bir "emanet" olay sağlama işlemi devre dışı bırakılan her zaman kaydedilir.

Olayları tek bir kullanıcı için sağlama sırasında bakarken olaylar normalde bu sırada oluşur:

1. Alma olayı: kullanıcı, kaynak sistemden alınır.

2. Alma olayı: hedef sistem sorgulanan alınan kullanıcı varlığını denetlemek için.

3. Eşitleme kuralı olayı: kullanıcı verilerini hedef ve kaynak sistemlerden eşleme kurallarını ve kapsam belirleme filtreleri varsa, hangi eylemin gerçekleştirileceğini belirlemek için yapılandırılmış öznitelik karşı değerlendirilir.

4. Olayı ver: eşitleme kuralını olay eylem olması gerektiğini dikte varsa (örneğin ekleme, güncelleştirme, silme), eylem sonuçlarını dışarı aktarma olayda kaydedilir sonra gerçekleştirilen.

![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-provisioning-reporting/audit_logs.PNG)


### <a name="looking-up-provisioning-events-for-a-specific-user"></a>Belirli bir kullanıcı için olayları sağlama yukarı aranıyor

En yaygın kullanım için sağlama denetim günlüklerini bireysel bir kullanıcı hesabı sağlama durumunu denetlemek için geçerlidir. Belirli bir kullanıcı için son sağlama olayları aramak için:

1. Git **denetim günlüklerini** bölümü.

2. Gelen **kategori** menüsünde, select **hesap sağlama**.

3. İçinde **tarih aralığı** menüsünde, aramak istediğiniz tarih aralığını seçin

4. İçinde **arama** çubuğu, aramak istediğiniz kullanıcının kullanıcı Kimliğini girin. Kimliği değerinin biçimi ne olursa olsun özniteliği eşleme yapılandırması (örn. userPrincipalName veya çalışan kimlik numarası) birincil eşleşen kimliği olarak seçtiğiniz eşleşmesi gerekir. Gerekli kimlik değeri hedeflere sütununda görünür.

5. Arama için Enter tuşuna basın. En son sağlama olayların ilk döndürülür.

6. Olayları döndürülürse, etkinlik türleri ve olup bunlar başarılı veya başarısız unutmayın. Ardından hiç sonuç döndürmedi, kullanıcı yok ya da bir tam eşitleme henüz tamamlanmadı, henüz Hazırlama işlemi tarafından algılanmadı demektir.

7. Alınan, değerlendirilen ya da olay bir parçası olarak yazılan tüm kullanıcı özellikler de dahil olmak üzere ilave ayrıntıları görüntülemek için olayları tek tek tıklatın.


### <a name="tips-for-viewing-the-provisioning-audit-logs"></a>Sağlama denetim günlüklerini görüntülemek için ipuçları

Azure Portalı'ndaki en iyi okunabilirlik için seçin **sütunları** düğmesini tıklatın ve bu sütunları seçin:

* **Tarih** -olayın tarihi gösterir.
* **Hedeflere** -olay konular uygulama adı ve kullanıcı Kimliğini gösterir.
* **Etkinlik** -daha önce açıklandığı gibi etkinlik türü.
* **Durum** - olay veya başarılı olup olmadığını.
* **Durum Açıklaması** -sağlama durumunda ne Özet.


## <a name="troubleshooting"></a>Sorun giderme

Sağlama özet raporu ve Denetim günlükleri sorunları sağlama çeşitli kullanıcı hesabı sorun giderme admins yardımcı olacak önemli bir rol oynar.

Senaryo tabanlı otomatik kullanıcı sağlamayı ile ilgili sorunları giderme hakkında yönergeler için bkz [yapılandırma ve uygulama kullanıcılara sağlama sorunları](active-directory-application-provisioning-content-map.md).


## <a name="additional-resources"></a>Ek Kaynaklar

* [Kullanıcı hesabı Kurumsal uygulamaları için sağlama yönetme](manage-apps/configure-automatic-user-provisioning-portal.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)
