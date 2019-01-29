---
title: Azure AD öznitelik eşlemelerini özelleştirme | Microsoft Docs
description: Azure Active Directory'de SaaS uygulamaları için hangi öznitelik eşlemelerini bunları iş gereksinimlerinize yönelik olarak nasıl değiştirebileceğiniz olduğunu öğrenin.
services: active-directory
documentationcenter: ''
author: barbkess
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/09/2018
ms.author: barbkess
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: eaf6890223526b213ac4ec1180288b95fe6eaa29
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55149871"
---
# <a name="customizing-user-provisioning-attribute-mappings-for-saas-applications-in-azure-active-directory"></a>Kullanıcı Azure Active Directory'de SaaS uygulamaları için öznitelik eşlemelerini sağlama özelleştirme
Microsoft Azure AD, Salesforce ve Google Apps gibi üçüncü taraf SaaS uygulamalarına kullanıcı hazırlama için destek sağlar. Etkin bir üçüncü taraf SaaS uygulaması için kullanıcı sağlamayı varsa, Azure portalı, öznitelik değerleri öznitelik eşlemeleri formunda denetler.

Önceden yapılandırılmış bir dizi öznitelikleri ve Azure AD kullanıcı nesneleri ve her SaaS uygulamasının kullanıcı nesneleri arasında öznitelik eşlemelerini yoktur. Bazı uygulamalar diğer kullanıcılar, gruplar gibi ek olarak nesne türlerini yönetin. <br> 
 Varsayılan öznitelik eşlemelerini iş gereksinimlerinize göre özelleştirebilirsiniz. Diğer bir deyişle, değiştirin veya varolan öznitelik eşlemelerini silin veya yeni öznitelik eşlemelerini oluşturun.
 
## <a name="editing-user-attribute-mappings"></a>Kullanıcı öznitelik eşlemelerini düzenleme

Azure AD Portalı'nda bu özelliği tıklayarak erişebilirsiniz bir **eşlemeleri** yapılandırmada **sağlama** içinde **Yönet** bölümünü bir  **Kurumsal uygulama**.


![Salesforce](./media/customize-application-attributes/21.png) 

Tıklayarak bir **eşlemeleri** yapılandırma, ilgili açılır **öznitelik eşlemesi** ekran. Bir SaaS uygulaması tarafından düzgün çalışması için gerekli olan öznitelik eşlemelerini vardır. İçin gerekli öznitelikler **Sil** özelliği kullanılamıyor.


![Salesforce](./media/customize-application-attributes/22.png)

Yukarıdaki örnekte, gördüğünüz gibi **kullanıcıadı** öznitelik, salesforce'ta yönetilen bir nesnenin ile doldurulur **userPrincipalName** bağlı Azure Active Directory nesne değeri.

Varolan özelleştirebilirsiniz **öznitelik eşlemelerini** bir eşleme tıklayarak. Bu açılır **özniteliğini Düzenle** ekran.

![Salesforce](./media/customize-application-attributes/23.png)


### <a name="understanding-attribute-mapping-types"></a>Öznitelik eşlemesi türlerini anlama
Öznitelik eşlemeleri ile bir üçüncü taraf SaaS uygulamasında öznitelikleri nasıl doldurulur denetler. Desteklenen dört farklı eşleme türleri şunlardır:

* **Doğrudan** – target özniteliği Azure AD'de bir özniteliği bağlı nesnenin değeri ile doldurulur.
* **Sabit** – Hedef öznitelik belirttiğiniz belirli bir dize ile doldurulur.
* **İfade** -target özniteliği bir betik gibi ifade sonucuna göre doldurulur. 
  Daha fazla bilgi için [Azure Active Directory'deki öznitelik eşlemeleri için ifadeler yazma](functions-for-customizing-application-data.md).
* **Hiçbiri** -Hedef öznitelik bırakılırsa değiştirilmemiş. Ancak, hedef öznitelik sürekli boşsa, belirttiğiniz varsayılan değeri ile doldurulur.

Bu dört temel türlerine ek olarak, özel öznitelik eşlemelerini isteğe bağlı kavramını destekler. **varsayılan** değer atama. Varsayılan değer atama, Azure AD'de hedef nesne üzerinde ya da hiçbiri bir değer varsa bir target özniteliği değeri ile doldurulur sağlar. En yaygın bu alanı boş bırakırsanız yapılandırmadır.


### <a name="understanding-attribute-mapping-properties"></a>Öznitelik eşlemesi özelliklerini anlama

Önceki bölümde, zaten özellik eşlemesi type özelliği için tanıtılmıştır.
Bu özellik ek olarak, öznitelik eşlemelerini de aşağıdaki öznitelikleri destekler:

- **Kaynak özniteliği** -kaynak sistemden kullanıcı özniteliği (örnek: Azure Active Directory).
- **Hedef öznitelik** – hedef sistemde kullanıcı özniteliği (örnek: ServiceNow).
- **Bu özniteliği kullanarak nesneleri eşleşen** – Bu eşleme kullanıcıların kaynak ve hedef sistemleri arasında benzersiz olarak tanımlanabilmesi için kullanılması gereken olup olmadığını. Bu genellikle, userPrincipalName veya posta özniteliğini genellikle hedef uygulama kullanıcı adı alanına eşlenen Azure AD'de ayarlanır.
- **Eşleşen öncelik** – birden çok öznitelikleri eşleşen ayarlanabilir. Olduğunda birden çok, bu alan tarafından tanımlanan sırayla değerlendirilir. Bir eşleşme bulunduğu sürece başka eşleştirme öznitelikleri değerlendirilir.
- **Bu eşlemeyi Uygula**
    - **Her zaman** – bu eşlemeyi Uygula her iki kullanıcı oluşturma ve güncelleştirme eylemleri
    - **Yalnızca oluşturma sırasında** -yalnızca kullanıcı oluşturma eylemleri bu eşlemeyi Uygula


## <a name="editing-group-attribute-mappings"></a>Grup öznitelik eşlemelerini düzenleme

Seçilen sayısını, ServiceNow, Box ve Google Apps gibi uygulamaları, kullanıcı, nesneyi ek olarak Grup nesnelerini sağlamasını yapma özelliği destekler. Grup nesneleri, grup özellikleri gibi görünen adları içeren ve grup üyelerinin yanı sıra diğer adlar, e-posta.

![ServiceNow](./media/customize-application-attributes/24.png)

Grup sağlama isteğe bağlı olarak etkinleştirilebilir veya devre dışı altında grubu eşlemeyi seçerek **eşlemeleri**ve ayarı **etkin** istenen seçeneğiyle **özniteliğieşleme** ekran.

Grup nesnelerini bir parçası olarak sağlanan öznitelikler kullanıcı nesneleri, daha önce açıklanan aynı şekilde özelleştirilebilir. 

>[!TIP]
>Grup nesnelerini (özellikleri ve üyeleri) sağlama olan ayrı bir kavram aşamasından [grupları atama](assign-user-or-group-access-portal.md) uygulamaya. Bir uygulama için bir grup atayabilir, ancak yalnızca grup içinde bulunan kullanıcı nesneleri sağlamak mümkündür. Tam Grup nesnelerin sağlama atamalarını gruplarını kullanmak için gerekli değildir.


## <a name="editing-the-list-of-supported-attributes"></a>Desteklenen öznitelikler listesinde düzenleme

Belirli bir uygulama için desteklenen kullanıcı öznitelikleri, önceden yapılandırılmış. Çoğu uygulamanın kullanıcı yönetimi API'leri şema bulma desteklemez, dolayısıyla sağlama hizmetini Azure AD uygulama çağrıları yaparak desteklenen öznitelik listesini dinamik olarak oluşturmak mümkün değildir. 

Ancak, bazı uygulamalar, özel öznitelikler destekler. Azure portal kullanarak tanımlarını okuma ve yazma için özel öznitelikleri için sırasıyla sağlama hizmetini Azure AD'de girilmelidir **Gelişmiş Seçenekleri Göster** altındaki onay kutusunu  **Öznitelik eşlemesi** ekran.

Uygulama ve öznitelik listesini özelleştirmesini destekleyen sistemleri şunlardır:

* Salesforce
* ServiceNow
* Workday
* Azure Active Directory ([Azure AD Graph API'si varsayılan öznitelikler](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity) ve özel dizin uzantıları desteklenir)
* Destekleyen uygulamalar [SCIM 2.0](https://tools.ietf.org/html/rfc7643), öznitelikler tanımlanan burada [Çekirdek Şeması](https://tools.ietf.org/html/rfc7643) eklenmesi gerekir

>[!NOTE]
>Desteklenen öznitelik listesini düzenlemek, yalnızca kendi uygulamalarınıza ve sistemlerinize şemasını özelleştirmiş ve nasıl kendi özel öznitelikler tanımlanan, birinci elden bilgisine sahip Yöneticiler için önerilir. Bu, bazen bir uygulama veya sistem tarafından sağlanan API'ler ve geliştiricilerin araçları konusunda gerektirir. 

![Düzenleyici](./media/customize-application-attributes/25.png) 

Desteklenen öznitelikler listesinde düzenlerken, aşağıdaki özellikler sunulur:

* **Ad** -hedef nesne şemasında tanımlandığı şekilde öznitelik sistem adı. 
* **Tür** -hedef nesne şemasında tanımlandığı şekilde öznitelik depolarının veri türü. Bu, aşağıdakilerden biri olabilir:
   * *İkili* -özniteliği ikili verileri içerir.
   * *Boole* -özniteliği, True veya False değerini içeriyor.
   * *DateTime* -öznitelik, bir tarih dizesi içerir.
   * *Tamsayı* -özniteliğini içeren bir tamsayı.
   * *Başvuru* -özniteliği başka bir hedef uygulama tablosunda depolanan değeri başvuran bir kimlik içeriyor.
   * *Dize* -özniteliği içeren bir metin dizesi. 
* **Birincil anahtar mı?** -Olup olmadığını veya öznitelik hedef nesnenin şeması birincil anahtar alan olarak tanımlı değil.
* **Gerekli?** -Olsun veya olmasın özniteliği hedef uygulama ya da sistemin doldurulması için gereklidir.
* **Birden çok değerli?** -Öznitelik birden çok değer destekleyip desteklemediğini.
* **Tam çalışması?** -Olsun veya olmasın öznitelikleri değerler büyük küçük harfe duyarlı bir şekilde değerlendirilir.
* **API ifadesi** -, belirli bir sağlama bağlayıcı (örneğin, Workday) belgelerine bunu belirtilmedikçe kullanmayın.
* **Başvurulan nesne özniteliği** - bu menü tablo ve öznitelik öznitelikle ilişkili değeri içerir hedef uygulamada seçmenize olanak sağlar. Bu bir başvuru türü özniteliği ise. Örneğin, "nesne ayrı bir"Bölümler"tablosunda depolanan değeri başvuruyor departmanı" adlı bir öznitelik varsa, "Departments.Name" seçin. Başvuru tabloları ve belirli bir uygulama için desteklenen birincil kimlik alanları önceden yapılandırılmış ve şu anda Azure portalını kullanarak düzenlenemez, ancak kullanılarak düzenlenebilir unutmayın [Graph API'si](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-configure-with-custom-target-attributes).

Yeni bir öznitelik eklemek için desteklenen öznitelik listesini sonuna kaydırın, yukarıda sağlanan girişleri kullanarak alanları doldurun ve seçin **öznitelik Ekle**. Seçin **Kaydet** öznitelikleri eklemeyi bitirdiğinizde. Daha sonra yeniden yüklemeniz gerekir **sağlama** öznitelik eşlemesi Düzenleyicisi'nde kullanılabilir olana kadar yeni öznitelikler için sekmesinde.

## <a name="restoring-the-default-attributes-and-attribute-mappings"></a>Varsayılan öznitelikler ve öznitelik eşlemelerini geri yükleme

Seçebileceğiniz ihtiyacınız başla ve sıfırlama, var olan eşlemeleri, varsayılan durumlarına geri **varsayılan eşlemeleri geri** onay kutusunu işaretleyin ve yapılandırmayı kaydedin. Bu, uygulamanın yalnızca Azure AD kiracınız için uygulama galerisinden eklendikten yokmuş gibi tüm eşlemeleri ayarlar. 

Sağlama hizmeti çalışırken bu seçeneğin belirlenmesi, tüm kullanıcıların bir yeniden eşitleme etkili bir şekilde zorlar. 

>[!IMPORTANT]
>Kesinlikle önerilir **sağlama durumu** ayarlanması **kapalı** bu seçenek çağırmadan önce.


## <a name="what-you-should-know"></a>Bilmeniz gerekenler

* Microsoft Azure AD eşitleme işlemini verimli bir uygulamasını sağlar. Başlatılmış bir ortamda, yalnızca güncelleştirme gerektiren nesneler, bir eşitleme döngüsü sırasında işlenir. 

* Öznitelik eşlemeleri güncelleştirilirken bir eşitleme döngüsü performansı etkisi vardır. Öznitelik eşlemesi yapılandırma için bir güncelleştirme hesaplanması tüm yönetilen nesneleri gerektirir. 

* En azından, öznitelik eşlemelerinde yapılan art arda kaç tutmak için bir önerilen en iyi yöntemdir.


## <a name="next-steps"></a>Sonraki adımlar

* [Kullanıcı sağlama/sağlamayı kaldırma SaaS uygulamaları için otomatik hale getirin](user-provisioning.md)
* [Öznitelik eşlemeleri için ifadeler yazma](functions-for-customizing-application-data.md)
* [Kullanıcı sağlama için kapsam oluşturma filtresi](define-conditional-rules-for-provisioning-user-accounts.md)
* [Kullanıcıların ve grupların Azure Active Directory'den uygulamalara otomatik olarak hazırlanmasını etkinleştirmek için SCIM'yi kullanma](use-scim-to-provision-users-and-groups.md)
* [SaaS uygulamalarını tümleştirme hakkında öğreticiler listesi](../saas-apps/tutorial-list.md)


