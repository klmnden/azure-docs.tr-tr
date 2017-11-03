---
title: "API Management hizmetiniz Git - Azure kullanarak yapılandırma | Microsoft Docs"
description: "Kaydet ve Git kullanarak API Management hizmeti yapılandırmanızı yapılandırma hakkında bilgi edinin."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: mattfarm
ms.assetid: 364cd53e-88fb-4301-a093-f132fa1f88f5
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 87d4e3fc4f30d5c7b147fb460fb43367aef19118
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2017
---
# <a name="how-to-save-and-configure-your-api-management-service-configuration-using-git"></a>Kaydet ve Git kullanarak API Management hizmeti yapılandırmanızı yapılandırma
> 
> 

Her API Management hizmet örneği yapılandırma ve hizmet örneği için meta veriler hakkında bilgi içeren bir yapılandırma veritabanı tutar. Yayımcı Portalı'ndaki bir ayarı değiştirmek, bir PowerShell cmdlet'ini kullanarak veya bir REST API çağrısı yapma değişiklikleri hizmet örneği için yapılabilir. Bu yöntemleri ek olarak, Git kullanarak, hizmet yönetim senaryoları gibi etkinleştirme örneği hizmetinizi yönetebilirsiniz:

* Yapılandırma sürüm oluşturma - karşıdan yüklemek ve hizmetinizi farklı sürümlerini depolamak
* Yapılandırma değişiklikleri toplu - hizmetinizi yerel deponuzun birden fazla bölümü değişiklik ve değişikliklerin sunucu tek bir işlem ile tümleştirme
* Tanıdık Git araç zinciri ve iş akışı - Git araçları ve zaten aşina iş akışlarını kullanma

Aşağıdaki diyagram, API Management hizmet örneği yapılandırmak için çeşitli yollar özetini gösterir.

![Git yapılandırın][api-management-git-configure]

Yayımcı portalı, PowerShell cmdlet'leri ve REST API kullanarak hizmetinize değişiklikler yaptığınızda, hizmet yapılandırma veritabanı kullanarak yönettiğiniz `https://{name}.management.azure-api.net` diyagram sağ tarafta gösterildiği gibi endpoint. Diyagram sol tarafındaki hizmeti yapılandırmanızı Git kullanarak nasıl yönetebileceğinizi gösterir ve hizmetiniz için Git deposu bulunan `https://{name}.scm.azure-api.net`.

Aşağıdaki adımlarda, Git kullanarak, API Management hizmet örneğinizin yönetimine genel bir bakış sunulmaktadır.

1. Hizmet erişim Git yapılandırması
2. Git deponuz hizmeti yapılandırma veritabanına kaydedin
3. Yerel makinenizde Git deposuna kopyalama
4. En son depoyu yerel makinenize aşağıya doğru çekin ve tamamlama ve anında iletme değişiklikleri, deposuna geri
5. Hizmet yapılandırma veritabanınıza, depodaki değişikliklerden dağıtma

Bu makalede etkinleştirmek ve hizmet yapılandırmasını yönetmek için Git nasıl kullanılacağını açıklar ve dosya ve klasörleri Git deposu için bir başvuru sağlar.

## <a name="access-git-configuration-in-your-service"></a>Hizmet erişim Git yapılandırması
Yayımcı portalının sağ üst köşesinde Git simge görüntüleyerek hızla Git yapılandırmanızı durumunu görüntüleyebilirsiniz. Bu örnekte, kaydedilmemiş değişiklikler var. deponuza durum iletisi gösterir. API Management hizmeti yapılandırma veritabanına depoya henüz kaydedilmedi olmasıdır.

![Git durumu][api-management-git-icon-enable]

Git yapılandırma ayarlarını görüntülemek ve yapılandırmak için Git simgesine tıklayın veya tıklatın **güvenlik** menü gidin **yapılandırma deposu** sekmesi.

![GIT etkinleştir][api-management-enable-git]

> [!IMPORTANT]
> Özellikleri deposunda saklanır ve onun geçmişinde dek kalacak şekilde, tanımlı değil tüm gizli devre dışı bırakın ve Git erişim yeniden etkinleştirin. Özellikler, doğrudan ilke deyimlerinde depolamaya zorunda kalmamak için gizli tüm API yapılandırması ve ilkeleri de dahil olmak üzere sabit dize değerlerini yönetmek için güvenli bir yerde sağlar. Daha fazla bilgi için bkz: [özelliklerinin Azure API Management ilkeleri nasıl kullanılacağını](api-management-howto-properties.md).
> 
> 

Etkinleştirme veya REST API kullanarak Git erişimini devre dışı bırakma hakkında daha fazla bilgi için bkz: [etkinleştirmek veya devre dışı REST API kullanarak Git erişim](https://msdn.microsoft.com/library/dn781420.aspx#EnableGit).

## <a name="to-save-the-service-configuration-to-the-git-repository"></a>Git deposuna hizmet yapılandırmasını kaydetmek için
Depoyu kopyalama önce ilk adımı, hizmet yapılandırması geçerli durumunu depoya tasarruf etmektir. Tıklatın **Kaydet yapılandırma deposu için**.

![Yapılandırmayı kaydedin][api-management-save-configuration]

Onay ekranda istediğiniz değişiklikleri yapın ve tıklatın **Tamam** kaydetmek için.

![Yapılandırmayı kaydedin][api-management-save-configuration-confirm]

Birkaç dakika sonra yapılandırma kaydedilir ve son yapılandırma değişikliği ve hizmet yapılandırmasını ve depo arasındaki son eşitleme saati ve tarihi dahil olmak üzere depo yapılandırma durumu görüntülenir.

![Yapılandırma durumu][api-management-configuration-status]

Yapılandırma deposu kaydedildikten sonra kopyalanabilir.

REST API kullanarak bu işlemi gerçekleştirme hakkında daha fazla bilgi için bkz: [REST API kullanarak anlık görüntü yürütme yapılandırma](https://msdn.microsoft.com/library/dn781420.aspx#CommitSnapshot).

## <a name="to-clone-the-repository-to-your-local-machine"></a>Depoyu yerel makinenize kopyalamak için
Bir depo kopyalamak için URL deponuz, bir kullanıcı adı ve parola gerekir. Kullanıcı adı ve URL en üstüne yakın görüntülenir **yapılandırma deposu** sekmesi.

![Git kopyalama][api-management-configuration-git-clone]

En altında oluşturulan parola **yapılandırma deposu** sekmesi.

![Parola oluştur][api-management-generate-password]

Bir parola oluşturmak için öncelikle emin **süre sonu** istenen sona erme tarihini ve saatini ayarlayın ve ardından **belirteç Oluştur**.

![Parola][api-management-password]

> [!IMPORTANT]
> Bu parolayı not edin. Bu sayfayı çıktıktan sonra parolayı yeniden görüntülenmez.
> 
> 

Aşağıdaki örnekler Git Bash aracından kullanmak [Windows için Git](http://www.git-scm.com/downloads) ancak bilginiz herhangi bir Git aracını kullanabilirsiniz.

Git aracınızı istenen klasöründe açın ve yayımcı portalı tarafından sağlanan komutunu kullanarak uygulamanızı yerel makinenizde git deposuna kopyalamak için aşağıdaki komutu çalıştırın.

```
git clone https://bugbashdev4.scm.azure-api.net/
```

Kullanıcı adı ve istendiğinde parolayı belirtin.

Herhangi bir hata alırsanız, değiştirmeyi deneyin, `git clone` kullanıcı adı ve parola, aşağıdaki örnekte gösterildiği gibi içerecek şekilde komutu.

```
git clone https://username:password@bugbashdev4.scm.azure-api.net/
```

Bu bir hata sağlıyorsa, URL komutun parola bölümü kodlama deneyin. Bunu yapmak için bir hızlı yoludur Visual Studio'yu açın ve aşağıdaki komutu yürütün **komut penceresi**. Açmak için **komut penceresi**, herhangi bir çözüm veya proje Visual Studio'da açın (veya yeni bir boş konsol uygulaması oluşturun) ve seçin **Windows**, **hemen** gelen **hata ayıklama** menüsü.

```
?System.NetWebUtility.UrlEncode("password from publisher portal")
```

Kodlanmış parolayı git komutu oluşturmak için kullanıcı adını ve depo konumu ile birlikte kullanın.

```
git clone https://username:url encoded password@bugbashdev4.scm.azure-api.net/
```

Depo klonlanmış sonra görüntülemek ve, yerel dosya sistemi ile çalışır. Daha fazla bilgi için bkz: [dosya ve klasör yapısı yerel Git deposu başvuru](#file-and-folder-structure-reference-of-local-git-repository).

## <a name="to-update-your-local-repository-with-the-most-current-service-instance-configuration"></a>En güncel hizmet örneği yapılandırmasıyla yerel deponuza güncelleştirmek için
API Management hizmet örneği yayımcı portalı veya REST API kullanarak değişiklik yaparsanız, en son değişikliklerle yerel deponuza güncelleştirme yapabilmeniz için önce bu değişiklikleri depoya kaydetmeniz gerekir. Bunu yapmak için tıklatın **Kaydet yapılandırma deposu için** üzerinde **yapılandırma deposu** yayımcı portalında sekmesini tıklatın ve ardından yerel deponuza aşağıdaki komutu yürütün.

```
git pull
```

Çalıştırmadan önce `git pull` için yerel deponuza klasörde olduğundan emin olun. Yalnızca tamamladıysanız `git clone` komutu, sonra da dizini aşağıdaki gibi bir komutu çalıştırarak, deposuna değiştirmeniz gerekir.

```
cd bugbashdev4.scm.azure-api.net/
```

## <a name="to-push-changes-from-your-local-repo-to-the-server-repo"></a>Değişiklikleri, Yerel Depodaki sunucu deposuna göndermek için
Değişiklikler yerel depodan sunucu depoya göndermek üzere değişikliklerinizi kaydetmek ve bunları sunucu depoya gönderme. Değişikliklerinizi uygulamak için Git komut aracını açın, yerel deponuza dizinine geçin ve aşağıdaki komutları verin.

```
git add --all
git commit -m "Description of your changes"
```

Tüm işlemeleri sunucuya göndermek için aşağıdaki komutu çalıştırın.

```
git push
```

## <a name="to-deploy-any-service-configuration-changes-to-the-api-management-service-instance"></a>API Management hizmet örneği için hizmet yapılandırma değişikliklerini dağıtmak için
Yerel değişikliklerinizi kaydedilen ve sunucu havuzuna gönderilen sonra API Management hizmet örneğiniz dağıtabilirsiniz.

![Dağıtma][api-management-configuration-deploy]

REST API kullanarak bu işlemi gerçekleştirme hakkında daha fazla bilgi için bkz: [dağıtmak Git değişiklikleri REST API kullanarak yapılandırma veritabanına](https://docs.microsoft.com/en-us/rest/api/apimanagement/tenantconfiguration).

## <a name="file-and-folder-structure-reference-of-local-git-repository"></a>Yerel Git deposu dosya ve klasör yapısı başvurusu
Dosya ve klasörlerin yerel git deposu içinde hizmet örneği hakkında yapılandırma bilgileri içerir.

| Öğe | Açıklama |
| --- | --- |
| Kök API management klasörü |Hizmet örneği için en üst düzey yapılandırmayı içerir |
| API klasörü |Hizmet örneği API'lerinde için yapılandırmayı içerir |
| grupları klasörü |Hizmet örneği gruplarında için yapılandırmayı içerir |
| ilkeleri klasörü |Hizmet örneği ilkelerinde içerir |
| portalStyles klasörü |Hizmet örneği Geliştirici Portalı özelleştirmeleri için yapılandırmayı içerir |
| Ürünler klasörü |Hizmet örneği ürünleri için yapılandırmayı içerir |
| Şablonlar klasörü |E-posta şablonlarını hizmet örneği için yapılandırmayı içerir |

Her klasör bir veya daha fazla içerebilir ve bazı durumlarda bir veya daha fazla klasör, örneğin her API, ürün veya grup için bir klasör durumda. Her klasördeki dosyalar, klasör adı tarafından açıklanan varlık türü için özeldir.

| Dosya türü | Amaç |
| --- | --- |
| JSON |İlgili varlık hakkında yapılandırma bilgileri |
| HTML |Genellikle Geliştirici Portalı'nda görüntülenen varlık hakkında açıklamaları |
| xml |İlke deyimleri |
| CSS |Geliştirici Portalı özelleştirme için stil sayfaları |

Bu dosyalar oluşturulabilir, silinen, düzenlenemez ve yerel dosya sisteminde yönetilen ve dağıtılan değişiklikleri yeniden API Management hizmet örneği.

> [!NOTE]
> Aşağıdaki varlıklar Git deposunda yer almayan ve Git kullanılarak yapılandırılamaz.
> 
> * Kullanıcılar
> * Abonelikler
> * Özellikler
> * Geliştirici Portalı varlıkları stilleri dışında
> 
> 

### <a name="root-api-management-folder"></a>Kök API management klasörü
Kök `api-management` klasörünü içeren bir `configuration.json` şu biçimde hizmet örneği hakkında üst düzey bilgileri içeren dosya.

```json
{
  "settings": {
    "RegistrationEnabled": "True",
    "UserRegistrationTerms": null,
    "UserRegistrationTermsEnabled": "False",
    "UserRegistrationTermsConsentRequired": "False",
    "DelegationEnabled": "False",
    "DelegationUrl": "",
    "DelegatedSubscriptionEnabled": "False",
    "DelegationValidationKey": ""
  },
  "$ref-policy": "api-management/policies/global.xml"
}
```

İlk dört ayarları (`RegistrationEnabled`, `UserRegistrationTerms`, `UserRegistrationTermsEnabled`, ve `UserRegistrationTermsConsentRequired`) eşlemek için aşağıdaki ayarları **kimlikleri** sekmesinde **güvenlik** bölümü.

| Kimlik ayarı | Eşler |
| --- | --- |
| RegistrationEnabled |**Anonim kullanıcılar oturum açma sayfasına yeniden yönlendir** onay kutusu |
| UserRegistrationTerms |**Kullanıcı Kaydolma kullanım koşullarını** metin kutusu |
| UserRegistrationTermsEnabled |**Kullanım koşulları kaydolma sayfasında göster** onay kutusu |
| UserRegistrationTermsConsentRequired |**Onay gerektiren** onay kutusu |

![Kimlik ayarları][api-management-identity-settings]

Sonraki dört ayarları (`DelegationEnabled`, `DelegationUrl`, `DelegatedSubscriptionEnabled`, ve `DelegationValidationKey`) eşlemek için aşağıdaki ayarları **temsilci** sekmesinde **güvenlik** bölümü.

| Temsilci ayarı | Eşler |
| --- | --- |
| DelegationEnabled |**Oturum açma ve kaydolma temsilci** onay kutusu |
| DelegationUrl |**Temsilci uç nokta URL'si** metin kutusu |
| DelegatedSubscriptionEnabled |**Temsilci ürün aboneliği** onay kutusu |
| DelegationValidationKey |**Doğrulama anahtarı temsilci** metin kutusu |

![Temsilci ayarları][api-management-delegation-settings]

Son ayar `$ref-policy`, hizmet örneği için genel ilke deyimleri dosyası eşleştirir.

### <a name="apis-folder"></a>API klasörü
`apis` Klasörü her API aşağıdaki öğeleri içeren hizmet örneği için bir klasör içerir.

* `apis\<api name>\configuration.json`-Bu API için yapılandırması ve arka uç hizmeti URL'si ve işlemler hakkında bilgi içerir. Bu çağrı için olsaydı, döndürülür, aynı bilgilerdir [belirli bir API alma](https://msdn.microsoft.com/library/azure/dn781423.aspx#GetAPI) ile `export=true` içinde `application/json` biçimi.
* `apis\<api name>\api.description.html`-Bu API açıklaması ve karşılık gelen `description` özelliği [API varlık](https://msdn.microsoft.com/library/azure/dn781423.aspx#EntityProperties).
* `apis\<api name>\operations\`-Bu klasörde `<operation name>.description.html` API işlemlerinde Eşle dosyaları. Her dosya eşleştiren API tek bir işlemde açıklamasını içerir `description` özelliği [işlemi varlık](https://msdn.microsoft.com/library/azure/dn781423.aspx#OperationProperties) REST API'sindeki.

### <a name="groups-folder"></a>grupları klasörü
`groups` Klasörü, hizmet örneği içinde tanımlanan her grup için bir klasör içerir.

* `groups\<group name>\configuration.json`-Bu grup için bir yapılandırmadır. Bu çağrı için olsaydı, döndürülür, aynı bilgilerdir [belirli bir grup alma](https://msdn.microsoft.com/library/azure/dn776329.aspx#GetGroup) işlemi.
* `groups\<group name>\description.html`-Bu grubun açıklaması ve karşılık gelen `description` özelliği [Grup varlık](https://msdn.microsoft.com/library/azure/dn776329.aspx#EntityProperties).

### <a name="policies-folder"></a>ilkeleri klasörü
`policies` Klasörü hizmet Örneğiniz için ilke ifadeleri içerir.

* `policies\global.xml`-Hizmet Örneğiniz için genel kapsamda tanımlanan ilkeleri içerir.
* `policies\apis\<api name>\`-API kapsamda tanımlanan tüm ilkeler varsa, bu klasörde yer alır.
* `policies\apis\<api name>\<operation name>\`Klasör - işlemi kapsamında tanımlı tüm ilkeler varsa, bu klasörde yer alır `<operation name>.xml` her işlem için ilke deyimleri eşleme dosyaları.
* `policies\products\`-Ürün kapsamda tanımlanan tüm ilkeler varsa, bunlar içeren bu klasörde yer alan `<product name>.xml` her ürün için ilke deyimleri eşleme dosyaları.

### <a name="portalstyles-folder"></a>portalStyles klasörü
`portalStyles` Klasörü, Geliştirici Portalı özelleştirmeleri hizmet örneği için yapılandırma ve stil sayfalarını içerir.

* `portalStyles\configuration.json`-Geliştirici Portalı tarafından kullanılan stil sayfaları adlarını içerir
* `portalStyles\<style name>.css`-Her `<style name>.css` dosyasını içeren Geliştirici Portalı için stiller (`Preview.css` ve `Production.css` varsayılan olarak).

### <a name="products-folder"></a>Ürünler klasörü
`products` Klasörü, hizmet örneği içinde tanımlanan her ürün için bir klasör içerir.

* `products\<product name>\configuration.json`-Bu ürün için bir yapılandırmadır. Bu çağrı için olsaydı, döndürülür, aynı bilgilerdir [belirli bir ürün almak](https://msdn.microsoft.com/library/azure/dn776336.aspx#GetProduct) işlemi.
* `products\<product name>\product.description.html`-Bu ürünün açıklaması ve karşılık gelen `description` özelliği [ürün varlığı](https://msdn.microsoft.com/library/azure/dn776336.aspx#Product) REST API'sindeki.

### <a name="templates"></a>şablonları
`templates` İçeren klasör için yapılandırma [e-posta şablonları](api-management-howto-configure-notifications.md) hizmet örneği.

* `<template name>\configuration.json`-Bu e-posta şablonu için bir yapılandırmadır.
* `<template name>\body.html`-e-posta şablonunun gövdesini budur.

## <a name="next-steps"></a>Sonraki adımlar
Hizmet örneğinizi yönetmenin başka yolları hakkında daha fazla bilgi için bkz:

* Hizmet örneğinizi aşağıdaki PowerShell cmdlet'lerini kullanarak yönetme
  * [Hizmet dağıtımı PowerShell cmdlet başvurusu](https://msdn.microsoft.com/library/azure/mt619282.aspx)
  * [Hizmet Yönetimi PowerShell cmdlet başvurusu](https://msdn.microsoft.com/library/azure/mt613507.aspx)
* Hizmet örneğinizi yayımcı portalında Yönet
  * [İlk API'nizi yönetme](api-management-get-started.md)
* Hizmet örneğinizi REST API kullanarak yönetme
  * [API Management REST API Başvurusu](https://msdn.microsoft.com/library/azure/dn776326.aspx)

## <a name="watch-a-video-overview"></a>Video genel izleyin
> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Configuration-over-Git/player]
> 
> 

[api-management-enable-git]: ./media/api-management-configuration-repository-git/api-management-enable-git.png
[api-management-git-enabled]: ./media/api-management-configuration-repository-git/api-management-git-enabled.png
[api-management-save-configuration]: ./media/api-management-configuration-repository-git/api-management-save-configuration.png
[api-management-save-configuration-confirm]: ./media/api-management-configuration-repository-git/api-management-save-configuration-confirm.png
[api-management-configuration-status]: ./media/api-management-configuration-repository-git/api-management-configuration-status.png
[api-management-configuration-git-clone]: ./media/api-management-configuration-repository-git/api-management-configuration-git-clone.png
[api-management-generate-password]: ./media/api-management-configuration-repository-git/api-management-generate-password.png
[api-management-password]: ./media/api-management-configuration-repository-git/api-management-password.png
[api-management-git-configure]: ./media/api-management-configuration-repository-git/api-management-git-configure.png
[api-management-configuration-deploy]: ./media/api-management-configuration-repository-git/api-management-configuration-deploy.png
[api-management-identity-settings]: ./media/api-management-configuration-repository-git/api-management-identity-settings.png
[api-management-delegation-settings]: ./media/api-management-configuration-repository-git/api-management-delegation-settings.png
[api-management-git-icon-enable]: ./media/api-management-configuration-repository-git/api-management-git-icon-enable.png




