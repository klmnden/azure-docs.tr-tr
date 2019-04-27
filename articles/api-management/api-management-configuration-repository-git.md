---
title: API Yönetimi hizmetiniz, Git - Azure kullanarak yapılandırma | Microsoft Docs
description: Kaydet ve Git kullanarak, API Management hizmet yapılandırmasını öğrenin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: mattfarm
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/12/2019
ms.author: apimpm
ms.openlocfilehash: adf4d8d5cfcef2dde8193ce1b7f2805a44e2d93d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60657951"
---
# <a name="how-to-save-and-configure-your-api-management-service-configuration-using-git"></a>Kaydet ve Git kullanarak API Management hizmet yapılandırmanızı yapılandırma

Her API Management hizmet örneği, hizmet örneği için meta verileri ve yapılandırma hakkında bilgi içeren bir yapılandırma veritabanı tutar. Azure portalında bir ayarı değiştirmek, bir PowerShell cmdlet'i kullanılarak veya bir REST API çağrısı yaparak hizmet örneği için değişiklik yapılamaz. Bu yöntemlerin yanı sıra Git kullanarak, hizmet yönetim senaryoları gibi etkinleştirme, hizmet örneği yapılandırmanızı yönetebilirsiniz:

* Yapılandırma sürümü oluşturma - indirin ve hizmet yapılandırmanızı farklı sürümlerini depolamanıza
* Yapılandırma değişiklikleri toplu - hizmet yapılandırmanızı yerel deponuzda birden fazla bölümü değişiklik ve değişiklikleri tekrar sunucuya tek bir işlem ile tümleştirin
* Tanıdık Git araç zinciri ve iş akışı - Git araçları ve, zaten alışık olduğunuz iş akışlarını kullanma

Aşağıdaki diyagram, API Management hizmet örneğinizin yapılandırmak için farklı yollarına genel bakış gösterilir.

![Git'i yapılandırma][api-management-git-configure]

Azure portalı, PowerShell cmdlet'leri ve REST API kullanarak hizmetinize değişiklik yaptığınızda, hizmet yapılandırma veritabanını kullanarak yönettiğiniz `https://{name}.management.azure-api.net` sağ tarafında diyagramda gösterildiği gibi uç nokta. Diyagramın sol tarafında hizmet yapılandırmanızı Git kullanarak nasıl yönetebileceğinizi gösterir ve hizmetiniz için Git deposu konumu `https://{name}.scm.azure-api.net`.

Aşağıdaki adımlar, Git kullanarak API Management hizmet örneğinizin yönetmeye genel bakış sağlar.

1. Hizmet erişim Git yapılandırması
2. Hizmet yapılandırma veritabanınızda Git deponuza Kaydet
3. Git deposunu yerel makinenize kopyalayın
4. En son depoyu yerel makinenize aşağı çeker ve işlemeyi ve göndermeyi değişiklikleri deponuza geri
5. Hizmet yapılandırma veritabanına değişiklikleri deponuza dağıtma

Bu makalede, etkinleştirmek ve hizmet yapılandırmanızı yönetmek için Git'i kullanın açıklar ve dosya ve klasörleri Git deposundaki bir başvuru sağlar.

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="access-git-configuration-in-your-service"></a>Hizmet erişim Git yapılandırması

Görüntüleme ve Git yapılandırma ayarlarınızı yapılandırmak için tıklayabilirsiniz **güvenlik** menü gidin **yapılandırma deposu** sekmesi.

![GIT'i etkinleştir][api-management-enable-git]

> [!IMPORTANT]
> Adlandırılmış değerler olarak tanımlanmamış herhangi bir gizli anahtar deposunda depolanır ve devre dışı bırakın ve Git erişimini yeniden etkinleştirmeniz kadar buna ait geçmişi kalır. Adlandırılmış değerler sabit dize değerleri, bunları doğrudan ilke deyimlerinde depolamak zorunda kalmamak için gizli dizileri, tüm API configuration ve ilkeleri gibi yönetmek için güvenli bir yerde sağlar. Daha fazla bilgi için [adlı değerlerini Azure API Management ilkelerini kullanmak nasıl](api-management-howto-properties.md).
>
>

REST API kullanarak Git erişimini devre dışı bırakma veya etkinleştirme hakkında daha fazla bilgi için bkz: [etkinleştirmek veya devre dışı REST API kullanarak Git erişim](/rest/api/apimanagement/tenantaccess?EnableGit).

## <a name="to-save-the-service-configuration-to-the-git-repository"></a>Git deposuna hizmet yapılandırmasını kaydetmek için

Depoyu kopyalama önce ilk adımı, hizmet yapılandırması geçerli durumunu depoya tasarruf etmektir. Tıklayın **depoya Kaydet**.

Onay ekranında istediğiniz değişiklikleri yapın ve tıklayın **Tamam** kaydetmek için.

Birkaç dakika sonra yapılandırma kaydedilir ve tarih ve saat son yapılandırma değişikliği ve hizmet yapılandırması ve depo arasındaki son eşitleme dahil olmak üzere depo yapılandırma durumu görüntülenir.

Yapılandırmayı depoya kaydedildikten sonra kopyalanabilir.

REST API kullanarak bu işlemi gerçekleştirme hakkında daha fazla bilgi için bkz: [REST API kullanarak anlık görüntü işleme yapılandırma](/rest/api/apimanagement/tenantaccess?CommitSnapshot).

## <a name="to-clone-the-repository-to-your-local-machine"></a>Depoyu yerel makinenize kopyalamak için

Bir depoyu kopyalamak için URL'yi deponuzu, bir kullanıcı adı ve parola gerekir. Kullanıcı adı ve diğer kimlik bilgilerini almak için tıklayın **erişim kimlik bilgilerini** sayfanın üstüne yakın.

Bir parola oluşturmak için öncelikle emin **bitiş** istenen sona erme tarihi ve saati ayarlayın ve ardından **Oluştur**.

> [!IMPORTANT]
> Bu parolayı not edin. Bu sayfadan çıktıktan sonra parolayı yeniden görüntülenmez.
>

Aşağıdaki örneklerde Git Bash aracından [Git için Windows](https://www.git-scm.com/downloads) ancak bilginiz herhangi bir Git aracı kullanabilirsiniz.

Git aracı istenen klasöründe ve git deposunu Azure portalı tarafından sağlanan komutu kullanarak yerel makinenize kopyalamak için aşağıdaki komutu çalıştırın.

```
git clone https://{name}.scm.azure-api.net/
```

Kullanıcı adı ve parola istendiğinde sağlar.

Herhangi bir hata alırsanız, değiştirmeyi deneyin, `git clone` kullanıcı adı ve parola, aşağıdaki örnekte gösterildiği gibi içerecek şekilde komutu.

```
git clone https://username:password@{name}.scm.azure-api.net/
```

Bu hata sağlıyorsa, komut parola kısmı kodlama URL'si deneyin. Bunu yapmanın hızlı bir yolu olan Visual Studio'yu açın ve aşağıdaki komutu yürütün **komut penceresi**. Açmak için **komut penceresi**, herhangi bir çözüm veya proje Visual Studio'da açın (veya yeni bir boş bir konsol uygulaması oluşturun) ve **Windows**, **hemen** gelen **Hata ayıklama** menüsü.

```
?System.NetWebUtility.UrlEncode("password from the Azure portal")
```

Kodlanmış parolayı git komutu oluşturmak için kullanıcı adı ve depo konumu ile birlikte kullanın.

```
git clone https://username:url encoded password@{name}.scm.azure-api.net/
```

Depo kopyalandı sonra görüntüleyebilir ve yerel dosya sisteminize çalışma. Daha fazla bilgi için [dosya ve klasör yerel Git deposu başvuru yapısı](#file-and-folder-structure-reference-of-local-git-repository).

## <a name="to-update-your-local-repository-with-the-most-current-service-instance-configuration"></a>Yerel deponuz ile en son hizmet örneği yapılandırmanızı güncelleştirmek için

API Management hizmet örneğinizin Azure portalı veya REST API'yi kullanarak değişiklik yaparsanız, yerel deponuzu en son değişikliklerle güncelleştirmek için önce bu değişiklikleri depoya kaydetmeniz gerekir. Bunu yapmak için tıklatın **depoya kaydetme yapılandırma** üzerinde **yapılandırma deposu** Azure portalında sekmesine tıklayın ve ardından yerel deponuzda aşağıdaki komutu yürütün.

```
git pull
```

Çalıştırmadan önce `git pull` yerel deponuzu klasörde olduğundan emin olun. Yalnızca tamamladıysanız `git clone` komut sonra dizini aşağıdaki gibi bir komut çalıştırarak deponuza değiştirmeniz gerekir.

```
cd {name}.scm.azure-api.net/
```

## <a name="to-push-changes-from-your-local-repo-to-the-server-repo"></a>Değişiklikleri yerel deponuzdan sunucu depoya göndermek için
Değişiklikleri yerel depodan sunucu depoya göndermek için değişikliklerinizi işleyin ve ardından bunları sunucu depoya gerekir. Değişikliklerinizi kaydetmek için Git komut aracını açın, yerel deponuzun dizinine geçin ve aşağıdaki komutları çalıştırın.

```
git add --all
git commit -m "Description of your changes"
```

Tüm işlemeleri sunucuya göndermek için aşağıdaki komutu çalıştırın.

```
git push
```

## <a name="to-deploy-any-service-configuration-changes-to-the-api-management-service-instance"></a>Hizmet yapılandırma değişiklikleri için API Management hizmet örneği dağıtmak için

Yerel değişikliklerinizi kaydedilmiş ve sunucu deposuna gönderilen sonra API Management hizmet örneğinizin dağıtabilirsiniz.

REST API kullanarak bu işlemi gerçekleştirme hakkında daha fazla bilgi için bkz: [dağıtma Git REST API kullanarak yapılandırma veritabanına değişiklikleri](https://docs.microsoft.com/rest/api/apimanagement/tenantconfiguration).

## <a name="file-and-folder-structure-reference-of-local-git-repository"></a>Dosya ve klasör yapısını başvurusunu yerel Git deposu

Dosya ve klasörleri yerel git deposunu hizmet örneği hakkında yapılandırma bilgilerini içerir.

| Öğe | Açıklama |
| --- | --- |
| kök API Yönetimi klasörü |Hizmet örneği için en üst düzey yapılandırmayı içerir |
| API'leri klasörü |Hizmet örneği API'leri için yapılandırmayı içerir |
| grupları klasörü |Hizmet örneği gruplarında için yapılandırmayı içerir |
| ilkeleri klasörü |Hizmet örneği ilkeleri içerir. |
| portalStyles klasörü |Geliştirici portal özelleştirmeleri hizmet örneği için yapılandırmayı içerir |
| ürünleri klasörü |Hizmet örneği ürünleri için yapılandırmayı içerir |
| Şablonları klasörü |E-posta şablonları hizmet örneği için yapılandırmayı içerir |

Her klasör, bir veya daha fazla dosya içerebilir ve bazı durumlarda bir veya daha fazla klasör, örneğin her API, ürün veya grup için bir klasör durumlarda. Her klasördeki dosyaları, klasör adına göre açıklanan varlık türü için özeldir.

| Dosya türü | Amaç |
| --- | --- |
| json |İlgili varlık hakkında yapılandırma bilgileri |
| html |Genellikle Geliştirici portalında gösterilen bir varlık ile ilgili açıklamalar |
| xml |İlke deyimleri |
| CSS |Geliştirici Portalı özelleştirme için stil sayfaları |

Bu dosyalar oluşturulabilir, silindi, düzenlenebilir ve yerel dosya sisteminize ve değişiklikleri geri, API Management hizmet örneği için Dağıtılmış yönetilen.

> [!NOTE]
> Aşağıdaki varlıkların Git deposunda yer almayan ve Git kullanılarak yapılandırılamaz.
>
> * [Kullanıcılar](https://docs.microsoft.com/en-us/rest/api/apimanagement/user)
> * [Abonelikler](https://docs.microsoft.com/en-us/rest/api/apimanagement/subscription)
> * [Adlandırılmış değerler](https://docs.microsoft.com/en-us/rest/api/apimanagement/property)
> * Geliştirici Portalı varlıkları dışında stilleri
>

### <a name="root-api-management-folder"></a>kök API Yönetimi klasörü
Kök `api-management` klasörünü içeren bir `configuration.json` hizmet örneği şu biçimde hakkında üst düzey bilgileri içeren dosya.

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
    "DelegationValidationKey": "",
    "RequireUserSigninEnabled": "false"
  },
  "$ref-policy": "api-management/policies/global.xml"
}
```

İlk dört ayarları (`RegistrationEnabled`, `UserRegistrationTerms`, `UserRegistrationTermsEnabled`, ve `UserRegistrationTermsConsentRequired`) eşlemek için aşağıdaki ayarları **kimlikleri** sekmesinde **güvenlik** bölümü.

| Kimliği ayarlama | Eşleyen |
| --- | --- |
| RegistrationEnabled |Varlığını **kullanıcı adı ve parola** kimlik sağlayıcısı |
| UserRegistrationTerms |**Kullanıcı kaydı kullanım koşullarını** metin kutusu |
| UserRegistrationTermsEnabled |**Kullanım koşullarını kaydolma sayfasında göster** onay kutusu |
| UserRegistrationTermsConsentRequired |**Onay gerektiren** onay kutusu |
| RequireUserSigninEnabled |**Anonim kullanıcıları oturum açma sayfasına yönlendir** onay kutusu |

Sonraki dört ayarları (`DelegationEnabled`, `DelegationUrl`, `DelegatedSubscriptionEnabled`, ve `DelegationValidationKey`) eşlemek için aşağıdaki ayarları **temsilci** sekmesinde **güvenlik** bölümü.

| Temsilci ayarı | Eşleyen |
| --- | --- |
| DelegationEnabled |**Oturum açma ve kaydolma temsilcisi** onay kutusu |
| DelegationUrl |**Temsilci seçme uç nokta URL'si** metin kutusu |
| DelegatedSubscriptionEnabled |**Ürün aboneliği temsilcisi** onay kutusu |
| DelegationValidationKey |**Temsilci seçme doğrulama anahtarı** metin kutusu |

Son ayar `$ref-policy`, hizmet örneği için genel ilke deyimlerini dosyanın eşleştirir.

### <a name="apis-folder"></a>API'leri klasörü
`apis` Klasörü her API'SİNDE aşağıdaki öğeleri içeren hizmet örneği için bir klasör içerir.

* `apis\<api name>\configuration.json` -Bu API için yapılandırması ve arka uç hizmeti URL'sini ve işlemleri hakkındaki bilgileri içerir. Çağrılacak olsaydı döndürülecek olan aynı olan bilgileri budur [belirli bir API'yi alın](https://docs.microsoft.com/rest/api/apimanagement/apis/get) ile `export=true` içinde `application/json` biçimi.
* `apis\<api name>\api.description.html` -Bu API açıklaması ve karşılık gelen `description` özelliği [API varlığı](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.table._entity_property).
* `apis\<api name>\operations\` -Bu klasörde `<operation name>.description.html` API'sindeki işlemlerle eşlenir dosyaları. Her dosya tek bir işlemle eşlenir API açıklamasını içerir `description` özelliği [işlemi varlık](https://docs.microsoft.com/rest/api/visualstudio/operations/list#operationproperties) REST API'de.

### <a name="groups-folder"></a>grupları klasörü
`groups` Klasörü hizmet örneğinde tanımlanan her grup için bir klasör içerir.

* `groups\<group name>\configuration.json` -Bu grup için bir yapılandırmadır. Çağrılacak olsaydı döndürülecek olan aynı olan bilgileri budur [belirli bir grup alma](https://docs.microsoft.com/rest/api/apimanagement/group/get) işlemi.
* `groups\<group name>\description.html` -Bu grubun açıklamasını ve karşılık gelen `description` özelliği [Grup varlık](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-group-entity).

### <a name="policies-folder"></a>ilkeleri klasörü
`policies` Klasörü hizmet örneğinizin ilke ifadeleri içerir.

* `policies\global.xml` -Hizmet örneğinizin genel kapsamda tanımlanan ilkeleri içerir.
* `policies\apis\<api name>\` -API kapsamında tanımlanan tüm ilkeler varsa, bu klasörde yer alır.
* `policies\apis\<api name>\<operation name>\` klasörü - işlemi kapsamında tanımlanan tüm ilkeler varsa, bu klasörde yer alır `<operation name>.xml` her işlem için ilke bildirimlerine eşleme dosyaları.
* `policies\products\` -Ürün kapsamında tanımlanan tüm ilkeler varsa, bunlar içeren bu klasörde yer `<product name>.xml` her ürün için ilke bildirimlerine eşleme dosyaları.

### <a name="portalstyles-folder"></a>portalStyles klasörü
`portalStyles` Klasörü Geliştirici portal özelleştirmeleri hizmet örneği için yapılandırma ve stil sayfalarını içerir.

* `portalStyles\configuration.json` -Geliştirici Portalı tarafından kullanılan stil sayfaları adlarını içerir
* `portalStyles\<style name>.css` -Her `<style name>.css` dosyasını içeren Geliştirici Portalı stillerini (`Preview.css` ve `Production.css` varsayılan olarak).

### <a name="products-folder"></a>ürünleri klasörü
`products` Klasörü hizmet örneğinde tanımlanan her ürün için bir klasör içerir.

* `products\<product name>\configuration.json` -Bu ürün için yapılandırmadır. Çağrılacak olsaydı döndürülecek olan aynı olan bilgileri budur [belirli bir ürün almak](https://docs.microsoft.com/rest/api/apimanagement/product/get) işlemi.
* `products\<product name>\product.description.html` -Bu ürün açıklamasını ve karşılık gelen `description` özelliği [ürün varlığı](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-product-entity) REST API'de.

### <a name="templates"></a>templates
`templates` Klasörde yapılandırmasını [e-posta şablonları](api-management-howto-configure-notifications.md) hizmet örneği.

* `<template name>\configuration.json` -e-posta şablonu yapılandırması budur.
* `<template name>\body.html` -e-posta şablonunun gövdesini budur.

## <a name="next-steps"></a>Sonraki adımlar
Hizmet örneğinizi yönetmek için diğer yöntemler hakkında daha fazla bilgi için bkz:

* Hizmet örneğinizi aşağıdaki PowerShell cmdlet'lerini kullanarak yönetme
  * [Hizmet dağıtımı PowerShell cmdlet başvurusu](https://docs.microsoft.com/powershell/module/wds)
  * [Hizmet Yönetimi PowerShell cmdlet başvurusu](https://docs.microsoft.com/powershell/azure/servicemanagement/overview)
* Hizmet örneğinizi REST API kullanarak yönetme
  * [API Management REST API Başvurusu](/rest/api/apimanagement/)


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




