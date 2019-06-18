---
title: Azure AD hak yönetimi nedir? (Önizleme) - Azure Active Directory
description: Azure Active Directory hak yönetimi ve grupları, uygulamaları ve SharePoint Online siteleri iç ve dış kullanıcılar için erişimi yönetmek için kullanma hakkında genel bir bakış edinin.
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 05/30/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: efd3ff8a6e7ddf2aa6242cc322d8a6536a6bd26b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66474058"
---
# <a name="what-is-azure-ad-entitlement-management-preview"></a>Azure AD hak yönetimi nedir? (Önizleme)

> [!IMPORTANT]
> Azure Active Directory (Azure AD) Yetkilendirme Yönetimi, şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Kuruluşta çalışanlara çeşitli grupları, uygulamaları ve sitelerin işlerini gerçekleştirmek için erişim gerekir. Bu erişimi yönetme zordur. Çoğu durumda, bir kullanıcının bir proje için gereken tüm kaynakların düzenlenmiş bir listesini yoktur. Proje yöneticisinin proje en son gerekli kaynakları kişiler dahil olan ve ne kadar iyi bir anlayışa sahiptir. Ancak, Proje Yöneticisi genellikle onaylayın veya diğer erişim izni yok. Bu senaryo, dış kişiler veya şirketler ile çalışmak çalıştığınızda daha karmaşık alır.

Azure Active Directory (Azure AD) yetkilendirme Yönetim grupları, uygulamaları ve SharePoint Online siteleri iç kullanıcılar ayrıca, kuruluşunuzun dışındaki kullanıcılar için erişimi yönetmenize yardımcı olabilir.

## <a name="why-use-entitlement-management"></a>Hak Yönetimi neden kullanmalısınız?

Büyük kuruluşlar genellikle gibi kaynaklara erişimi yönetirken güçlüklerle karşılaşır:

- Kullanıcılar hangi erişim sahip olması gereken bilmeyebilir
- Kullanıcı doğru kişilere veya doğru kaynakları bulmakta bulunabilir
- Kullanıcıları, bulmak ve bir kaynağa erişim izni alırlar sonra bunlar iş amacıyla gerekenden daha uzun erişim tutunabilir

Bu sorunlar, tedarik zinciri kuruluşlar ya da diğer iş ortaklarıyla olan dış kullanıcılar gibi başka bir dizinden erişmesi gereken kullanıcılar için compounded. Örneğin:

- Kuruluşların tüm belirli kişilerin bunları davet edebilirsiniz diğer dizinlerde bilmeyebilir
- Kuruluşlar bu davet edebilirsiniz olsa bile, kuruluşların tüm kullanıcı erişimini tutarlı bir şekilde yönetmek hatırlayabilirsiniz değil

Azure AD hak yönetimi, bu sorunların üstesinden gelmenize yardımcı olabilir.

## <a name="what-can-i-do-with-entitlement-management"></a>Hak Yönetimi ile ne yapabilirim?

Hak Yönetimi özellikleri bazıları şunlardır:

- Kullanıcılar isteyebilir ilgili kaynak paketleri oluşturma
- İstek kaynakları ve erişim süresinin sona erdiği için nasıl kurallarını tanımlayın
- Yaşam döngüsünü iç ve dış kullanıcılar için erişimi yönetme
- Temsilci Yönetimi kaynakları
- Onaylayanlar istekleri onaylamak için belirtme
- Geçmişini izlemek için raporlar oluşturma

Kimlik Yönetimi ve hak yönetimi genel bakış için Ignite 2018 konferansına ait şu videoyu izleyin:

>[!VIDEO https://www.youtube.com/embed/aY7A0Br8u5M]

## <a name="what-resources-can-i-manage"></a>Hangi kaynakların yönetebilirim?

Hak Yönetimi ile erişimi yönetmek kaynak türleri şunlardır:

- Azure AD güvenlik grupları
- Office 365 grupları
- SaaS uygulaması ve Federasyon destekleyen özel tümleşik uygulamalar gibi veya sağlama azure AD kurumsal uygulamalar
- SharePoint Online site koleksiyonları ve siteler

Azure AD güvenlik grupları veya Office 365 grupları'nı kullanan diğer kaynaklara erişim de denetleyebilirsiniz.  Örneğin:

- Azure AD güvenlik grubu kullanarak bir erişim paketinde ve yapılandırma için Microsoft Office 365 kullanıcı lisansları verebilirsiniz [grup tabanlı lisanslama](../users-groups-roles/licensing-groups-assign.md) bu grup için
- Kullanıcıların bir Azure AD güvenlik grubu kullanarak bir erişim paketinde ve oluşturarak Azure kaynaklarını yönetmek için erişim verebilirsiniz bir [Azure rol ataması](../../role-based-access-control/role-assignments-portal.md) bu grup için

## <a name="what-are-access-packages-and-policies"></a>Erişim paketler ve ilkeleri nelerdir?

Hak Yönetimi kavramını sunar bir *erişim paket*. Bir kullanıcı bir projede çalışmak veya işlerini gerçekleştirmek için gereken tüm kaynakların bir paketin bir erişim paketidir. Kaynakları, gruplar, uygulamalara veya sitelere erişimi içerir. Erişim paketler iç çalışanlarınıza ve ayrıca, kuruluşunuzun dışındaki kullanıcılar için erişimi yönetmek için kullanılır. Erişim paketleri adlı kapsayıcılarda tanımlanmış *katalogları*.

Erişim paketleri de dahil bir veya daha fazla *ilkeleri*. İlke kuralları veya erişim pakete guardrails tanımlar. Bir ilkeyi etkinleştirmek, yalnızca doğru kullanıcının doğru süre miktarını yanı sıra, doğru kaynaklara erişim izni verildiğini zorlar.

![Paket erişim ve ilkeleri](./media/entitlement-management-overview/elm-overview-access-package.png)

Bir access paketi ve onun ilkelerini ile erişim Paket Yöneticisi tanımlar:

- Kaynaklar
- Kaynaklar için kullanıcıların rollerini gerekir
- İç ve erişim istemek uygun olan dış kullanıcılar
- Onay işlemini ve onaylayabilir veya erişimi engelleyeceği kullanıcıları
- Kullanıcının erişim süresi

Aşağıdaki diyagramda, Hak Yönetimi'nde farklı öğelerinin bir örneği gösterilmektedir. Bu iki örnek erişim paketler gösterilmektedir.

- **Erişim paket 1** bir kaynak olarak tek bir grup içerir. Erişim sağlayan bir dizi dizine erişim istemek için kullanıcılara bir ilke ile tanımlanır.
- **Access Paketi 2** bir grubu, uygulama ve SharePoint Online sitesi kaynakları içerir. Erişim, iki farklı ilkeleri ile tanımlanır. İlk ilke erişim istemek için kullanıcıların dizindeki kümesini etkinleştirir. İkinci ilkeyi erişim istemek kullanıcıları bir dış dizininde sağlar.

![Hak Yönetimi'ne genel bakış](./media/entitlement-management-overview/elm-overview.png)

## <a name="external-users"></a>Dış kullanıcılar

Kullanırken [Azure AD--işletmeler arası (B2B)](../b2b/what-is-b2b.md) deneyimi, davet e-posta adreslerini kaynak dizininizi taşıyın ve çalışmak istediğiniz dış konuk kullanıcılara önceden bilmeniz gerekir. Daha küçük ya da kısa süreli bir proje üzerinde çalışıyorsanız ve tüm katılımcıları bildiğiniz, ancak bu kullanıcılar, birlikte çalışmak istediğiniz sayıda varsa yönetmek için zordur veya katılımcılar zamanla değişiyorsa harika bu çalışır.  Örneğin, başka bir kuruluşla çalışma ve söz konusu kuruluştaki ile bir iletişim noktası olmalıdır, ancak zaman içinde o kuruluştan ek kullanıcılar ayrıca erişim gerekir.

Hak Yönetimi ile Azure AD erişim paket isteği için de kullanıyorsanız, belirttiğiniz kuruluşlardan izin veren bir ilke tanımlayabilirsiniz. Onay gerekli olup olmadığını ve erişim için bir sona erme tarihi belirtebilirsiniz. Onay gerekli olursa, onaylayan olarak da belirleyebilirsiniz kuruluşlarında hangi dış kullanıcıların erişim gerektiğini öğrenmek olası olduğundan, daha önce - davet dış kuruluştan bir veya daha fazla kullanıcı. Erişim paketi yapılandırdıktan sonra dış kuruluşta, kişi için erişimi paketi bir bağlantı gönderebilirsiniz. Bu kişiyi dış kuruluşunuzdaki diğer kullanıcılarla paylaşabilir ve paket erişim istemek için bu bağlantıyı kullanabilirsiniz.  Dizininize davet edildiniz söz konusu kuruluştaki kullanıcılar, bu bağlantıyı da kullanabilirsiniz.

Bir istek onaylandığında, hak yönetimi değil zaten dizininizde iseler kullanıcı davet içerebilecek gerekli erişimi olan kullanıcı sağlanır. Azure AD B2B hesabı kendileri için otomatik olarak oluşturur.  Yöneticinin önceden ayarlayarak, hangi kuruluşların işbirliği için izin verilen sınırlı olabileceğini unutmayın bir [B2B izin verme veya reddetme](../b2b/allow-deny-list.md) izin vermeyi veya engellemeyi, diğer kuruluşlar için davet eder.  Kullanıcı izin verilenler veya Engellenenler listesi tarafından izin verilmiyor, ardından bunların davet edilir değil.

Sonsuza kadar son dış kullanıcının erişim istemediğiniz olduğundan, 180 gün gibi ilke içinde bir sona erme tarihi belirtin. Erişimleri yenilenmezse, 180 gün sonra hak yönetimi, erişim paket ile ilişkili tüm erişim kaldırın.  Hak Yönetimi üzerinden davet edilen kullanıcının başka bir erişim paket atamaları varsa, ardından kendi son Ataması'na kaybettiğinde B2B hesabını 30 gün boyunca oturum açma engellendi ve daha sonra kaldırıldı.  Bu, gereksiz hesapları çoğalan engeller.  

## <a name="terminology"></a>Terminoloji

Hak Yönetimi ve belgelerini daha iyi anlamak için aşağıdaki koşulları gözden geçirmelidir.

| Kavram veya sözleşme | Açıklama |
| --- | --- |
| Hak Yönetimi | Atar, iptal eder ve erişim paketleri yöneten hizmet. |
| Paket erişim | İzinleri ve ilkeleri kullanıcılar isteyebilir kaynakları koleksiyonudur. Bir erişim paket her zaman bir katalogda yer alır. |
| erişim isteği | Bir erişim paket erişimi için istek. Bir istek genellikle bir iş akışı gider. |
| policy | Nasıl kullanıcıları, erişim elde kimler onaylayabilir ve ne kadar süreyle kullanıcıların erişimi gibi erişim yaşam döngüsünü tanımlayan kuralları kümesi. Örnek ilkeleri ve dış çalışan erişimi içerir. |
| catalog | İlgili kaynakları ve erişim paketleri bir kapsayıcısı. |
| Genel katalog | Her zaman kullanılabilir yerleşik bir kataloğu. Genel kataloğa kaynakları eklemek için belirli izinler gerektirir. |
| resource | Bir varlık veya bir kullanıcı için izinler verilebilir hizmet (örneğin, grubu, uygulama veya site). |
| Kaynak türü | Grupları, uygulamaları ve SharePoint Online siteleri içeren bir kaynak türü. |
| Kaynak rolü | Kaynakla ilişkili izinler koleksiyonudur. |
| Kaynak dizini | Paylaşmak için bir veya daha fazla kaynağa sahip bir dizin. |
| atanan kullanıcılar | Bir kullanıcı veya grup için bir erişim paketinin atama. |
| Etkinleştirme | Bir access paketi istemek kullanıcılar için kullanılabilir hale getirme işlemidir. |

## <a name="roles-and-permissions"></a>Roller ve izinler

Hak Yönetimi iş işleve göre farklı rol yok.

| Rol | Açıklama |
| --- | --- |
| [Kullanıcı Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#user-administrator) | Hak Yönetimi tüm özelliklerini yönetebilir.<br/>Kullanıcıları ve grupları oluşturun. |
| Katalog Oluşturucusu | Oluşturun ve kataloglarını yönetin. Genellikle BT yöneticisi veya kaynak sahibi. Katalog otomatik olarak oluşturan kişi Kataloğu'nun ilk katalog sahibi olur. |
| Katalog sahibi | Düzenle ve mevcut kataloglarını yönetin. Genellikle BT yöneticisi veya kaynak sahibi. |
| Erişim Paket Yöneticisi | Düzenle ve Katalog içindeki tüm var olan erişim paketleri yönetin. |
| Onaylayan | Paket erişim istekleri onaylayın. |
| İstek sahibi | Paket erişim isteyin. |

Aşağıdaki tabloda, bu rollerin her birini izinleri listeler.

| Görev | Kullanıcı Yöneticisi | Katalog Oluşturucusu | Katalog sahibi | Erişim Paket Yöneticisi | Onaylayan |
| --- | :---: | :---: | :---: | :---: | :---: |
| [Genel kataloğa yeni erişim paketi oluştur](entitlement-management-access-package-create.md) | :heavy_check_mark: |  :heavy_check_mark: |  |  |  |
| [Bir katalogda yeni erişim paketi oluştur](entitlement-management-access-package-create.md) | :heavy_check_mark: |   | :heavy_check_mark: |  |  |
| [Gelen/giden erişim paket kaynak rolleri Ekle/Kaldır](entitlement-management-access-package-edit.md) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Kimin bir erişim paketini talep edebilir belirtin](entitlement-management-access-package-edit.md#add-a-new-policy) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Doğrudan bir erişim paketi için kullanıcı atama](entitlement-management-access-package-edit.md#directly-assign-a-user) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Bir erişim paketi atamaya sahip görüntüle](entitlement-management-access-package-edit.md#view-who-has-an-assignment) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Bir erişim paketin istekleri görüntüleme](entitlement-management-access-package-edit.md#view-requests) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Bir isteğin teslim hataları görüntüleyin](entitlement-management-access-package-edit.md#view-a-requests-delivery-errors) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Bekleyen isteği iptal et](entitlement-management-access-package-edit.md#cancel-a-pending-request) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Bir access paketi Gizle](entitlement-management-access-package-edit.md#change-the-hidden-setting) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Bir erişim paketini Sil](entitlement-management-access-package-edit.md#delete) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Erişim isteği Onayla](entitlement-management-request-approve.md) |  |  |  |  | :heavy_check_mark: |
| [Katalog oluşturma](entitlement-management-catalog-create.md) | :heavy_check_mark: | :heavy_check_mark: |  |  |  |
| [Genel katalog içine/dışına kaynakları Ekle/Kaldır](entitlement-management-catalog-create.md#add-resources-to-a-catalog) | :heavy_check_mark: |  |  |  |  |
| [Kaynakların bir kataloğu içine/dışına Ekle/Kaldır](entitlement-management-catalog-create.md#add-resources-to-a-catalog) | :heavy_check_mark: |  | :heavy_check_mark: |  |  |
| [Katalog sahip ekleme ya da paket yöneticileri erişimi](entitlement-management-catalog-create.md#add-catalog-owners-or-access-package-managers) | :heavy_check_mark: |  | :heavy_check_mark: |  |  |
| [Katalog düzenleme/silme](entitlement-management-catalog-create.md#edit-a-catalog) | :heavy_check_mark: |  | :heavy_check_mark: |  |  |

## <a name="license-requirements"></a>Lisans gereksinimleri

[!INCLUDE [Azure AD Premium P2 license](../../../includes/active-directory-p2-license.md)]

Azure kamu, Azure Almanya ve Azure Çin 21Vianet gibi özel Bulutlar, bu önizlemede şu anda kullanılabilir değildir.

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: İlk erişim paketinizi oluşturmak](entitlement-management-access-package-first.md)
- [Yaygın senaryolar](entitlement-management-scenarios.md)
