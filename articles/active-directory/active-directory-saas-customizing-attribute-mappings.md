---
title: Azure AD öznitelik eşlemelerini özelleştirme | Microsoft Docs
description: Hangi öznitelik eşlemelerini Azure Active Directory'de SaaS uygulamaları için bunları iş ihtiyaçlarınızı karşılamak için nasıl değiştirebileceğiniz olduğunu öğrenin.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 549e0b8c-87ce-4c9b-b487-b7bf0155dc77
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2018
ms.author: markvi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 565394664ab59ef5186503f708502eacc040321f
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35295634"
---
# <a name="customizing-user-provisioning-attribute-mappings-for-saas-applications-in-azure-active-directory"></a>Kullanıcı Azure Active Directory'de SaaS uygulamaları için öznitelik eşlemelerini hazırlama özelleştirme
Microsoft Azure AD Salesforce, Google Apps ve diğerleri gibi üçüncü taraf SaaS uygulamalarına kullanıcı hazırlama için destek sağlar. Etkin bir üçüncü taraf SaaS uygulaması için sağlama kullanıcı varsa, Azure portal, öznitelik değerleri "özellik eşlemesi." adlı bir yapılandırma biçiminde denetler.

Önceden yapılandırılmış bir dizi öznitelikleri ve Azure AD kullanıcı ve her SaaS uygulamanın kullanıcı nesneleri arasında öznitelik eşlemelerini yoktur. Bazı uygulamalar diğer kullanıcılar, gruplar gibi ek nesne türlerini yönetin. <br> 
 Varsayılan öznitelik eşlemelerini iş gereksinimlerinize göre özelleştirebilirsiniz. Bu anlamına gelir, değiştirin veya varolan öznitelik eşlemelerini silin veya yeni öznitelik eşlemelerini oluşturun.
 
## <a name="editing-user-attribute-mappings"></a>Kullanıcı öznitelik eşlemelerini düzenleme

Azure AD portalında tıklatarak bu özellik erişebilirsiniz bir **eşlemeleri** yapılandırmada **sağlama** içinde **Yönet** bölümünü bir **Kurumsal uygulama**.


![Salesforce][5] 

Tıklatarak bir **eşlemeleri** yapılandırma, ilgili açılır **eşleme özniteliği** ekran. Doğru çalışması için bir SaaS uygulaması tarafından gerekli öznitelik eşlemelerini vardır. Gerekli öznitelikler için **silmek** özelliği kullanılamıyor.


![Salesforce][6]  

Yukarıdaki örnekte, gördüğünüz **kullanıcıadı** Salesforce içinde yönetilen bir nesnenin özniteliği ile doldurulur **userPrincipalName** bağlı Azure Active Directory nesne değeri.

Varolan özelleştirebilirsiniz **öznitelik eşlemelerini** eşleme tıklatarak. Bu açılır **öznitelik Düzenle** ekran.

![Salesforce][7]  


### <a name="understanding-attribute-mapping-types"></a>Öznitelik eşleme türlerini anlama
Öznitelik eşlemelerini ile öznitelikleri bir üçüncü taraf SaaS uygulamasına nasıl doldurulur denetler. Desteklenen dört farklı eşleme türleri şunlardır:

* **Doğrudan** – Hedef öznitelik Azure AD'de bağlantılı nesne özniteliği değeri ile doldurulur.
* **Sabit** – Hedef öznitelik belirttiğiniz için belirli bir dizeyi doldurulur.
* **İfade** -Hedef öznitelik sonucuna göre bir betik benzeri ifadesi doldurulur. 
  Daha fazla bilgi için bkz: [Azure Active Directory'de özellik eşlemeleri için ifade yazma](active-directory-saas-writing-expressions-for-attribute-mappings.md).
* **Hiçbiri** -Hedef öznitelik sol değiştirilmemiş. Ancak, hedef öznitelik sürekli boşsa, belirttiğiniz varsayılan değeri ile doldurulur.

Bu dört temel özniteliği eşleme türlerine ek olarak özel öznitelik eşlemelerini kavramı, isteğe bağlı destek **varsayılan** değer atama. Hedef nesne ya da Azure AD'de hiçbiri bir değer varsa bir target özniteliği değeri ile doldurulur ve varsayılan değer atamasını sağlar. Bu alanı boş bırakın için en yaygın yapılandırmadır bakın.


### <a name="understanding-attribute-mapping-properties"></a>Öznitelik Eşleme Özellikleri Anlama

Önceki bölümde, zaten öznitelik eşleme türü özelliği tanıtılmıştır.
Bu özellik ek olarak, öznitelik eşlemelerini ayrıca aşağıdaki özniteliklere destekler:

- **Kaynak özniteliği** -kaynak sistemden kullanıcı özniteliği (örnek: Azure Active Directory).
- **Hedef öznitelik** – hedef sistem kullanıcı özniteliğinde (örnek: ServiceNow).
- **Bu öznitelik kullanarak nesneleri eşleşen** – bu eşlemeyi kullanıcılar kaynak ve hedef sistemler arasında benzersiz şekilde tanımlamak için kullanılması gereken olup olmadığına bakılmaksızın. Bu genellikle, userPrincipalName veya posta özniteliği genellikle bir hedef uygulama username alanına eşlenen Azure AD'de ayarlanır.
- **Öncelik eşleşen** – birden çok öznitelikleri eşleşen ayarlanabilir. Olduğunda birden çok, bu alana göre tanımlanan sırayla değerlendirilir. Bir eşleşme olarak başka eşleştirme öznitelikleri değerlendirilir.
- **Bu eşleme Uygula**
    - **Her zaman** – hem kullanıcı oluşturulması bu eşleme uygulamak ve güncelleştirme eylemleri
    - **Yalnızca oluşturma sırasında** -bu eşlemenin yalnızca kullanıcı oluşturma eylemlerini uygulamak


## <a name="editing-group-attribute-mappings"></a>Grup öznitelik eşlemelerini düzenleme

ServiceNow, kutusunu ve Google Apps gibi uygulamaları seçilen çok sayıda kullanıcı nesneleri yanı sıra grup nesneleri sağlamasını yapma yeteneği desteği. Grup nesneleri Grup üyeleri yanı sıra diğer adları, e-posta ve görünen adları gibi Grup özellikleri içerir.

![ServiceNow][8]  

Grup sağlama isteğe bağlı olarak etkinleştirilebilecek ya da altında grup eşlemesi seçerek devre dışı **eşlemeleri**ve ayarı **etkin** istenen seçeneğiyle **özniteliği eşlemesi** ekran.

Grup nesneleri bir parçası olarak sağlanan öznitelikler, kullanıcı nesneleri, daha önce açıklanan aynı şekilde özelleştirilebilir. 

>[!TIP]
>Grup nesnelerin (özellikleri ve üyeleri) sağlama olduğu öğesinden farklı bir kavram [gruplarını atama](manage-apps/assign-user-or-group-access-portal.md) uygulamaya. Bir uygulamaya bir gruba atamak, ancak yalnızca grup içinde bulunan kullanıcı nesneleri sağlamak da mümkündür. Tam Grup nesnelerin sağlama atamalarını gruplarını kullanmak için gerekli değildir.


## <a name="editing-the-list-of-supported-attributes"></a>Desteklenen özniteliklerin listesi düzenleme

Belirli bir uygulama için desteklenen kullanıcı öznitelikleri, önceden yapılandırılmış görüntülenir. Çoğu uygulamanın kullanıcı yönetimi API'leri şema bulma desteklemez, bu nedenle hizmet sağlama Azure AD uygulama çağrı yaparak desteklenen özniteliklerin listesi dinamik olarak oluşturmak mümkün değildir. 

Ancak, bazı uygulamalar özel öznitelikleri destekler. Okuma ve yazma için özel öznitelikleri için sırasıyla hizmet sağlama Azure AD için Azure portal kullanarak tanımlarını girilmelidir **Gelişmiş Seçenekleri Göster** onay kutusunun alt kısmındaki  **Öznitelik eşleme** ekran.

Uygulamalar ve öznitelik listesini özelleştirmesini destekleyen sistemleri şunlardır:

* Salesforce
* ServiceNow
* İş günü
* Azure Active Directory
* Şirket içi Active Directory (parçası olarak bağlayıcı sağlama Workday kullanıcı)
* Destekleyen uygulamalar [SCIM'yi 2.0](https://tools.ietf.org/html/rfc7643), öznitelikler tanımlanan burada [Çekirdek Şeması](https://tools.ietf.org/html/rfc7643) eklenmesi gerekir

>[!NOTE]
>Desteklenen özniteliklerin listesi düzenleme yalnızca uygulamalarını ve sistemler şeması özelleştirdiyseniz ve özel öznitelikleriyle nasıl tanımladığınız, ilk eldeki bilgiye sahip Yöneticiler için önerilir. Bu bazen bir uygulama veya sistem tarafından sağlanan API'leri ve geliştiricilerin araçları aşina gerektirir. 

![Düzenleyici][9]  

Desteklenen özniteliklerin listesi düzenlerken, aşağıdaki özellikleri sağlanır:

* **Ad** -hedef nesnenin şemasında belirtildiği gibi özniteliğin sistem adı. 
* **Tür** -öznitelik depoları, hedef nesnenin şemasında belirtildiği gibi veri türü. Bu aşağıdakilerden biri olabilir:
   * *İkili* -öznitelik ikili verileri içerir.
   * *Boolean* -özniteliği True veya False değerini içeriyor.
   * *DateTime* -özniteliği içeren bir tarih dizesi.
   * *Tamsayı* -özniteliği içeren bir tamsayı.
   * *Başvuru* -özniteliği hedef uygulama başka bir tabloda depolanan bir değer başvuruda bulunan bir kimlik içeriyor.
   * *Dize* -özniteliği içeren bir metin dizesi. 
* **Birincil anahtar?** -Olsun veya olmasın öznitelik hedef nesnenin şeması birincil anahtar alanı olarak tanımlanır.
* **Gerekli?** -Olsun veya olmasın öznitelik hedef uygulama veya sistem doldurulması için gereklidir.
* **Çoklu değer?** -Öznitelik birden çok değer destekleyip desteklemediğini.
* **Tam durum?** -Olsun veya olmasın öznitelikleri değerleri büyük küçük harfe duyarlı bir şekilde değerlendirilir.
* **API ifade** -, belirli bir sağlama bağlayıcı (örneğin, iş günü) belgelerine tarafından Bunu yapmak için belirtilmedikçe kullanmayın.
* **Başvurulan nesne özniteliği** - bu menü özniteliğiyle ilişkili değeri içerir hedef uygulamada özniteliği ve tablo seçmenize olanak sağlar. Bu bir başvuru türü özniteliği ise. Örneğin, "nesne ayrı bir"Departman"tablo içinde saklanan değeri başvuruyor departmanı" adlı bir özniteliği varsa, "Departments.Name" seçersiniz. Başvuru tabloları ve belirli bir uygulama için desteklenen birincil kimlik alanları önceden yapılandırılmış ve şu anda Azure portalını kullanarak düzenlenemez, ancak kullanılarak düzenlenebilir unutmayın [grafik API'si](https://developer.microsoft.com/en-us/graph/docs/api-reference/beta/resources/synchronization-configure-with-custom-target-attributes).

Yeni bir öznitelik eklemek için desteklenen özniteliklerin listesi sonuna kaydırın, sağlanan girişleri kullanarak yukarıdaki alanları doldurun ve seçin **özniteliği eklemek**. Seçin **kaydetmek** öznitelikleri eklemeyi bitirdikten sonra. Daha sonra yeniden yüklemeniz gerekecek **sağlama** özniteliği Eşleme Düzenleyicisi'nde kullanılabilir olana kadar yeni öznitelikler sekmesi.

## <a name="restoring-the-default-attributes-and-attribute-mappings"></a>Varsayılan öznitelikleri ve öznitelik eşlemelerini geri yükleme

Seçebileceğiniz ihtiyacınız olması durumunda başla ve mevcut eşlemeleri varsayılan durumlarına geri sıfırlama, **varsayılan eşlemelerine geri** onay kutusunu işaretleyin ve yapılandırmayı kaydedin. Uygulamanın yalnızca Azure AD kiracınızın uygulama Galeriden eklendikten gibi bu tüm eşlemelerini ayarlar. 

Bu seçeneğin belirlenmesi sağlama hizmet çalışırken etkili bir şekilde yeniden eşitleme, tüm kullanıcıların zorlar. 

>[!IMPORTANT]
>Kesinlikle önerilir **sağlama durumu** ayarlanabilir **devre dışı** bu seçeneği çağırmadan önce.


## <a name="what-you-should-know"></a>Bilmeniz gerekenler

* Microsoft Azure AD eşitleme işlemini verimli bir uygulamasını sağlar. Başlatılmış bir ortamda, bir eşitleme döngüsü sırasında yalnızca güncelleştirmeleri gerektiren nesneler işlenir. 

* Öznitelik eşlemelerini güncelleştirme bir eşitleme döngüsü performans üzerinde bir etkisi vardır. Öznitelik eşleme yapılandırması için bir güncelleştirme tüm yönetilen nesnelerin olmasını gerektirir. 

* Bu ardışık değişiklik sayısı, öznitelik eşlemelerini en az tutmak için bir önerilen en iyi uygulamadır.


## <a name="next-steps"></a>Sonraki adımlar

* [Kullanıcı sağlama/sağlamayı SaaS uygulamaları için otomatik hale getirme](active-directory-saas-app-provisioning.md)
* [Özellik eşlemeleri için ifade yazma](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Kapsam belirleme filtreleri kullanıcı sağlama](active-directory-saas-scoping-filters.md)
* [Kullanıcıların ve grupların Azure Active Directory'den uygulamalara otomatik olarak hazırlanmasını etkinleştirmek için SCIM'yi kullanma](manage-apps/use-scim-to-provision-users-and-groups.md)
* [SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)

<!--Image references-->
[5]: ./media/active-directory-saas-customizing-attribute-mappings/21.png
[6]: ./media/active-directory-saas-customizing-attribute-mappings/22.png
[7]: ./media/active-directory-saas-customizing-attribute-mappings/23.png
[8]: ./media/active-directory-saas-customizing-attribute-mappings/24.png
[9]: ./media/active-directory-saas-customizing-attribute-mappings/25.PNG

