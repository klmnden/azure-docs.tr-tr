---
title: Azure AD öznitelik eşlemelerini özelleştirme | Microsoft Docs
description: Azure Active Directory'de SaaS uygulamaları için hangi öznitelik eşlemelerini bunları iş gereksinimlerinize yönelik olarak nasıl değiştirebileceğiniz olduğunu öğrenin.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: celested
ms.custom: H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8a2965fecd3aca17d6c4df7e49ad466377de9762
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60291696"
---
# <a name="customizing-user-provisioning-attribute-mappings-for-saas-applications-in-azure-active-directory"></a>Kullanıcı Azure Active Directory'de SaaS uygulamaları için öznitelik eşlemelerini sağlama özelleştirme
Microsoft Azure AD, Salesforce ve Google Apps gibi üçüncü taraf SaaS uygulamalarına kullanıcı hazırlama için destek sağlar. Bir üçüncü taraf SaaS uygulaması için kullanıcı sağlamayı etkinleştirin, Azure portalında öznitelik eşlemelerini öznitelik değerlerini denetler.

Önceden yapılandırılmış bir dizi öznitelikleri ve Azure AD kullanıcı nesneleri ve her SaaS uygulamasının kullanıcı nesneleri arasında öznitelik eşlemelerini yoktur. Bazı uygulamalar diğer kullanıcılar, gruplar gibi birlikte nesne türlerini yönetin.

Varsayılan öznitelik eşlemelerini iş gereksinimlerinize göre özelleştirebilirsiniz. Bu nedenle, değiştirin veya varolan öznitelik eşlemelerini silin veya yeni öznitelik eşlemelerini oluşturun.
 
## <a name="editing-user-attribute-mappings"></a>Kullanıcı öznitelik eşlemelerini düzenleme

Erişmek için bu adımları **eşlemeleri** kullanıcı sağlama özelliği:

1. Oturum [Azure Active Directory portalında](https://aad.portal.azure.com).

1. Seçin **kurumsal uygulamalar** sol bölmeden. Galeriden eklenen uygulamaları dahil olmak üzere tüm yapılandırılmış uygulamaların bir listesi gösterilir.

1. Raporları görüntülemek ve uygulama ayarlarını yönetme, uygulama yönetimi bölmesinde yüklemek için herhangi bir uygulama seçin.

1. Seçin **sağlama** hazırlama ayarları seçili uygulama için kullanıcı hesabını yönetmek için.

1. Genişletin **eşlemeleri** görüntüleme ve Azure AD arasında akış kullanıcı özniteliklerini düzenleyin ve hedef uygulama. Hedef uygulama destekliyorsa, bu bölümde, isteğe bağlı olarak gruplar ve hesaplar sağlama yapılandırmanıza olanak sağlar.

   ![Salesforce](./media/customize-application-attributes/21.png) 

1. Seçin bir **eşlemeleri** ilgili açmak için yapılandırma **eşleme özniteliği** ekran. Bazı öznitelik eşlemelerini bir SaaS uygulaması tarafından düzgün çalışması için gereklidir. İçin gerekli öznitelikler **Sil** özelliği kullanılamıyor.

   ![Salesforce](./media/customize-application-attributes/22.png)

   Bu ekran görüntüsünde görebilirsiniz **kullanıcıadı** öznitelik, salesforce'ta yönetilen bir nesnenin ile doldurulur **userPrincipalName** bağlı Azure Active Directory nesne değeri.

1. Mevcut bir seçin **eşleme özniteliği** açmak için **özniteliğini Düzenle** ekran. Burada, Azure AD arasında akan kullanıcı öznitelikleri düzenleyebileceğiniz ve hedef uygulama.

   ![Salesforce](./media/customize-application-attributes/23.png)


### <a name="understanding-attribute-mapping-types"></a>Öznitelik eşlemesi türlerini anlama
Öznitelik eşlemeleri ile bir üçüncü taraf SaaS uygulamasında öznitelikleri nasıl doldurulur denetler. Desteklenen dört farklı eşleme türleri şunlardır:

* **Doğrudan** – target özniteliği Azure AD'de bir özniteliği bağlı nesnenin değeri ile doldurulur.
* **Sabit** – Hedef öznitelik belirttiğiniz belirli bir dize ile doldurulur.
* **İfade** -target özniteliği bir betik gibi ifade sonucuna göre doldurulur. 
  Daha fazla bilgi için [Azure Active Directory'deki öznitelik eşlemeleri için ifadeler yazma](functions-for-customizing-application-data.md).
* **Hiçbiri** -Hedef öznitelik bırakılırsa değiştirilmemiş. Ancak, hedef öznitelik sürekli boşsa, belirttiğiniz varsayılan değeri ile doldurulur.

Bu dört temel türleri ile birlikte özel öznitelik eşlemelerini isteğe bağlı kavramını destekler. **varsayılan** değer atama. Varsayılan değer atama değil Azure AD'de veya hedef nesne üzerinde bir değer varsa bir target özniteliği değeri ile doldurulur sağlar. En yaygın bu alanı boş bırakırsanız yapılandırmadır.


### <a name="understanding-attribute-mapping-properties"></a>Öznitelik eşlemesi özelliklerini anlama

Önceki bölümde için öznitelik eşlemesi type özelliği zaten eklenmiştir.
Bu özellik ile birlikte öznitelik eşlemelerini de aşağıdaki öznitelikleri destekler:

- **Kaynak özniteliği** -kaynak sistemden kullanıcı özniteliği (örnek: Azure Active Directory).
- **Hedef öznitelik** – hedef sistemde kullanıcı özniteliği (örnek: ServiceNow).
- **Bu özniteliği kullanarak nesneleri eşleşen** : Bu eşleme, kullanıcılar kaynak ve hedef sistemleri arasında benzersiz olarak tanımlanabilmesi için kullanılmalıdır. Bu genellikle, userPrincipalName veya posta özniteliğini genellikle hedef uygulama kullanıcı adı alanına eşlenen Azure AD'de ayarlanır.
- **Eşleşen öncelik** – birden çok öznitelikleri eşleşen ayarlanabilir. Olduğunda birden çok, bu alan tarafından tanımlanan sırayla değerlendirilir. Bir eşleşme bulunduğu sürece başka eşleştirme öznitelikleri değerlendirilir.
- **Bu eşlemeyi Uygula**
    - **Her zaman** – bu eşlemeyi Uygula her iki kullanıcı oluşturma ve güncelleştirme eylemleri.
    - **Yalnızca oluşturma sırasında** -yalnızca kullanıcı oluşturma eylemleri bu eşlemeyi Uygula.


## <a name="editing-group-attribute-mappings"></a>Grup öznitelik eşlemelerini düzenleme

Seçilen sayısını, ServiceNow, Box ve Google Apps gibi uygulamaları Grup nesnelerini ve kullanıcı, nesneyi sağlamasını yapma özelliği destekler. Grup nesneleri, grup özellikleri gibi görünen adları içeren ve grup üyelerinin yanı sıra diğer adlar, e-posta.

![ServiceNow](./media/customize-application-attributes/24.png)

Grup sağlama isteğe bağlı olarak etkinleştirilebilir veya devre dışı altında grubu eşlemeyi seçerek **eşlemeleri**ve ayarı **etkin** istediğiniz seçeneği **öznitelikeşlemesi** ekran.

Grup nesnelerini bir parçası olarak sağlanan öznitelikler kullanıcı nesneleri, daha önce açıklanan aynı şekilde özelleştirilebilir. 

>[!TIP]
>Grup nesnelerini (özellikleri ve üyeleri) sağlama olan ayrı bir kavram aşamasından [grupları atama](assign-user-or-group-access-portal.md) uygulamaya. Bir uygulama için bir grup atayabilir, ancak yalnızca grup içinde bulunan kullanıcı nesneleri sağlamak mümkündür. Tam Grup nesnelerin sağlama atamalarını gruplarını kullanmak için gerekli değildir.


## <a name="editing-the-list-of-supported-attributes"></a>Desteklenen öznitelikler listesinde düzenleme

Belirli bir uygulama için desteklenen kullanıcı öznitelikleri, önceden yapılandırılmış. Çoğu uygulamanın kullanıcı yönetimi API'leri şema bulma desteklemez. Bu nedenle, Azure AD sağlama hizmeti uygulama çağrıları yaparak desteklenen öznitelik listesini dinamik olarak oluşturmak mümkün değildir. 

Ancak, bazı uygulamalar, özel öznitelikler destekler ve Azure AD sağlama hizmeti okuma ve yazma için özel öznitelikleri. Azure portalında tanımlarını girmek için seçin **Gelişmiş Seçenekleri Göster** altındaki onay kutusunu **eşleme özniteliği** ekran ve ardından **içinözniteliklistesinidüzenle** uygulamanızı.

Uygulama ve öznitelik listesini özelleştirmesini destekleyen sistemleri şunlardır:

* Salesforce
* ServiceNow
* Workday
* Azure Active Directory ([Azure AD Graph API'si varsayılan öznitelikler](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity) ve özel dizin uzantıları desteklenir)
* Destekleyen uygulamalar [SCIM 2.0](https://tools.ietf.org/html/rfc7643), öznitelikler tanımlanan burada [Çekirdek Şeması](https://tools.ietf.org/html/rfc7643) eklenmesi gerekir

>[!NOTE]
>Desteklenen öznitelik listesini düzenlemek, yalnızca kendi uygulamalarınıza ve sistemlerinize şemasını özelleştirmiş ve nasıl kendi özel öznitelikler tanımlanan, birinci elden bilgisine sahip Yöneticiler için önerilir. Bu, bazen bir uygulama veya sistem tarafından sağlanan API'ler ve geliştirici araçları konusunda gerektirir. 

Desteklenen öznitelikler listesinde düzenlerken, aşağıdaki özellikler sunulur:

* **Ad** -hedef nesne şemasında tanımlandığı şekilde öznitelik sistem adı. 
* **Tür** -veri türü özniteliği, şu türlerden biri olabilir hedef nesnenin şemada tanımlanan depolar:
   * *İkili* -özniteliği ikili verileri içerir.
   * *Boole* -özniteliği, True veya False değerini içeriyor.
   * *DateTime* -öznitelik, bir tarih dizesi içerir.
   * *Tamsayı* -özniteliğini içeren bir tamsayı.
   * *Başvuru* -özniteliği başka bir hedef uygulama tablosunda depolanan değeri başvuran bir kimlik içeriyor.
   * *Dize* -özniteliği içeren bir metin dizesi. 
* **Birincil anahtar mı?** -Özniteliği hedef nesnenin şeması birincil anahtar alan olarak tanımlı olup olmadığı.
* **Gerekli?** -Özniteliği hedef uygulama ya da sistemin doldurulacak gerekli olup olmadığı.
* **Birden çok değerli?** -Olup öznitelik birden çok değer destekler.
* **Tam çalışması?** -Olup öznitelik değerleri büyük küçük harfe duyarlı bir şekilde değerlendirilir.
* **API ifadesi** -belirli bir sağlama bağlayıcı (örneğin, Workday) belgelerine bunu belirtilmedikçe kullanmayın.
* **Başvurulan nesne özniteliği** - bu menü öznitelikle ilişkili değeri içerir hedef uygulamada, özniteliği ve tablo seçmenize olanak sağlar, bir başvuru türü özniteliği. Örneğin, "nesne ayrı bir"Bölümler"tablosunda depolanan değeri başvuruyor departmanı" adlı bir öznitelik varsa, "Departments.Name" seçin. Başvuru tabloları ve belirli bir uygulama için desteklenen birincil kimlik alanları önceden yapılandırılmış ve şu anda Azure portalını kullanarak düzenlenemez, ancak kullanılarak düzenlenebilir [Graph API'si](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-configure-with-custom-target-attributes).

Yeni bir öznitelik eklemek için desteklenen öznitelik listesini sonuna kaydırın, yukarıda sağlanan girişleri kullanarak alanları doldurun ve seçin **öznitelik Ekle**. Seçin **Kaydet** öznitelikleri eklemeyi bitirdiğinizde. Daha sonra yeniden yüklenmesi gerekiyor **sağlama** öznitelik eşlemesi Düzenleyicisi'nde kullanılabilir olana kadar yeni öznitelikler için sekmesinde.

## <a name="restoring-the-default-attributes-and-attribute-mappings"></a>Varsayılan öznitelikler ve öznitelik eşlemelerini geri yükleme

Seçebileceğiniz ihtiyacınız başla ve sıfırlama, var olan eşlemeleri, varsayılan durumlarına geri **varsayılan eşlemeleri geri** onay kutusunu işaretleyin ve yapılandırmayı kaydedin. Bunun yapılması, uygulamanın yalnızca Azure AD kiracınız için uygulama galerisinden olarak eklendiyse, tüm eşlemeleri ayarlar. 

Sağlama hizmeti çalışırken bu seçeneğin belirlenmesi, tüm kullanıcıların bir eşitleme etkili bir şekilde zorlar. 

>[!IMPORTANT]
>Kesinlikle öneririz **sağlama durumu** ayarlanması **kapalı** bu seçenek çağırmadan önce.


## <a name="what-you-should-know"></a>Bilmeniz gerekenler

* Microsoft Azure AD eşitleme işlemini verimli bir uygulamasını sağlar. Başlatılmış bir ortamda, yalnızca güncelleştirme gerektiren nesneler, bir eşitleme döngüsü sırasında işlenir. 

* Öznitelik eşlemeleri güncelleştirilirken bir eşitleme döngüsü performansı etkisi vardır. Öznitelik eşlemesi yapılandırma için bir güncelleştirme hesaplanması tüm yönetilen nesneleri gerektirir. 

* Önerilen en iyi yöntem en azından, öznitelik eşlemelerinde yapılan art arda kaç tutmaktır.


## <a name="next-steps"></a>Sonraki adımlar

* [Kullanıcı sağlama/sağlamayı kaldırma SaaS uygulamaları için otomatik hale getirin](user-provisioning.md)
* [Öznitelik eşlemeleri için ifadeler yazma](functions-for-customizing-application-data.md)
* [Kullanıcı sağlama için kapsam oluşturma filtresi](define-conditional-rules-for-provisioning-user-accounts.md)
* [Kullanıcıların ve grupların Azure Active Directory'den uygulamalara otomatik olarak hazırlanmasını etkinleştirmek için SCIM'yi kullanma](use-scim-to-provision-users-and-groups.md)
* [SaaS uygulamalarını tümleştirme hakkında öğreticiler listesi](../saas-apps/tutorial-list.md)


