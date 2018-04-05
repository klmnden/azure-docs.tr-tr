---
title: Azure AD B2C dil özelleştirme | Microsoft Docs
description: Dil deneyimini özelleştirme hakkında bilgi edinin.
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 02/26/2018
ms.author: davidmu
ms.openlocfilehash: 3d0f1f2ffd02873df2e2e7eab9894d9c3421b0f7
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="language-customization-in-azure-active-directory-b2c"></a>Azure Active Directory B2C dil özelleştirme

>[!NOTE]
>Bu özellik genel önizlemede değil.
>

Azure Active Directory B2C dil özelleştirmesinde (Azure AD B2C) ilkeniz müşteri gereksinimlerinize uygun olarak farklı dillerde uyum sağlar.  Microsoft, çevirilerini sağlar [36 dilleri](#supported-languages), ancak herhangi bir dil için kendi Çeviriler de sağlayabilirsiniz. Deneyiminizi yalnızca tek bir dil için sağlanan olsa bile, herhangi bir metin sayfalarında özelleştirebilirsiniz.  

## <a name="how-language-customization-works"></a>Dil özelleştirme nasıl çalışır?
Dil özelleştirme kullanıcı Yolculuğunuzun kullanılabilir diller seçmek için kullanın. Bu özellik etkinleştirildikten sonra sorgu dizesi parametresi sağlayabilirsiniz `ui_locales`, uygulamanızdan. Azure AD B2C çağırdığınızda, sayfanızın belirttiniz yerel çevrilir. Bu tür bir yapılandırma kullanıcı Yolculuğunuzun dilleri üzerinde tam denetim verir ve Müşteri'nin tarayıcı dil ayarlarını yoksayar. 

Müşterinizin görür hangi dilde üzerinde denetim düzeyini gerekmeyebilir. Sağlamıyorsa bir `ui_locales` parametresi, müşteri deneyimi, tarayıcının ayarları tarafından dikte edilir.  Kullanıcı Yolculuğunuzun desteklenen bir dil ekleyerek çevrilir hangi dilleri hala kontrol edebilirsiniz. Müşteri'nin tarayıcı desteklemek istemediğiniz bir dili göstermek üzere ayarlarsanız, varsayılan olarak desteklenen kültürler seçilen dil yerine gösterilir.

- **kullanıcı arabirimi yerel ayarlar belirtilen dil**: dil özelleştirme etkinleştirdikten sonra kullanıcı Yolculuğunuzun burada belirtilen dili çevrilir.
- **Tarayıcı istenen dil**: yoksa `ui_locales` parametre belirtildi, kullanıcı Yolculuğunuzun tarayıcı istenen dil için çevrilmiş *dil destekleniyorsa*.
- **İlke varsayılan dili**: tarayıcı bir dil belirtmeyen veya desteklenmeyen bir belirtir, kullanıcı gezisine ilkesi varsayılan dili çevrilir.

>[!NOTE]
>Özel kullanıcı özniteliklerini kullanıyorsanız, kendi çevirileri sağlamanız gerekir. Daha fazla bilgi için bkz: [dizelerinizi özelleştirme](#customize-your-strings).
>

## <a name="support-requested-languages-for-uilocales"></a>Ui_locales için istenen dil desteği 
Dil özelleştirme genel kullanılabilirlik önce oluşturulan ilkeleri bu özellik ilk etkinleştirmeniz gerekir. Varsayılan olarak etkin dil özelleştirme sonra oluşturulan ilkeleri vardır. 

Dil özelleştirme ilkesinde etkinleştirdiğinizde ekleyerek kullanıcı gezisine dilinin denetleyebilirsiniz `ui_locales` parametresi.
1. [Azure portalındaki B2C özellikleri sayfasına gidin](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-app-registration#navigate-to-b2c-settings).
2. Çevirileri için etkinleştirmek istediğiniz bir ilke göz atın.
3. Seçin **dil özelleştirme**.  
4. Seçin **etkinleştirmek dil özelleştirme**.
5. İletişim kutusu bilgileri okuyun ve seçin **Evet**.

## <a name="select-which-languages-in-your-user-journey-are-enabled"></a>Kullanıcı Yolculuğunuzun hangi dillerde etkin seçin 
Diller için ne zaman çevrilecek kullanıcı Yolculuğunuzun için bir dizi etkinleştirmek `ui_locales` parametresi sağlanmadı.
1. İlkeniz dil özelleştirme önceki yönergeleri etkin olduğundan emin olun.
2. Gelen **Düzenle İlkesi** sayfasında, **dil özelleştirme**.
3. Desteklemek istediğiniz bir dil seçin.
4. Özellikler bölmesinde değiştirmek **etkin** için **Evet**.  
5. Seçin **kaydetmek** Özellikler bölmesinde üstünde.

>[!NOTE]
>Varsa bir `ui_locales` parametresi sağlanmadığından, yalnızca etkinse, sayfa için Müşteri'nin tarayıcının dili çevrilir.
>

## <a name="customize-your-strings"></a>Dizelerinizi özelleştirme
Dil özelleştirme kullanıcı Yolculuğunuzun herhangi bir dize özelleştirmenize olanak tanır.
1. İlkeniz dil özelleştirme önceki yönergeleri etkin olduğundan emin olun.
2. Gelen **Düzenle İlkesi** sayfasında, **dil özelleştirme**.
3. Özelleştirmek istediğiniz dili seçin.
4. Düzenlemek istediğiniz sayfasını seçin.
5. Seçin **karşıdan varsayılanları** (veya **karşıdan geçersiz kılmaları** bu dilde daha önce düzenlediyseniz). 

Bu adımları dizelerinizi düzenlemeye başlamak için kullanabileceğiniz bir JSON dosyası verin.

### <a name="change-any-string-on-the-page"></a>Sayfadaki herhangi bir dize değiştirme
1. Önceki yönergeleri bir JSON Düzenleyicisi'ni indirilen JSON dosyasını açın.
2. Değiştirmek istediğiniz öğesi bulunamıyor.  Bulabileceğiniz `StringId` dize aramakta olduğunuz veya aramak için `Value` değiştirmek istediğiniz özniteliği.
3. Güncelleştirme `Value` görüntülenmesini istediğiniz ile özniteliği.
4. Değiştirmek istediğiniz her dizesini değiştirin `Override` için `true`.
5. Dosyayı kaydedin ve değişikliklerinizi karşıya yükleyin. (Karşıya yükleme denetimi JSON dosyasını indirdiğiniz olarak aynı yerde bulabilirsiniz.) 

>[!IMPORTANT]
>Bir dize geçersiz kılmak gerekiyorsa, ayarladığınızdan emin olun `Override` değeri `true`.  Giriş değeri değiştirilmez, göz ardı edilir. 
>

### <a name="change-extension-attributes"></a>Uzantı öznitelikleri Değiştir
Bir JSON eklemek istediğiniz veya özel kullanıcı özniteliği dizesi değiştirmek istediğinizde, aşağıdaki biçimde şöyledir:
```JSON
{
  "LocalizedStrings": [
    {
      "ElementType": "ClaimType",
      "ElementId": "extension_<ExtensionAttribute>",
      "StringId": "DisplayName",
      "Override": true,
      "Value": "<ExtensionAttributeValue>"
    }
    [...]
}
```

Değiştir `<ExtensionAttribute>` ile özel kullanıcı özniteliğinin adı.  

Değiştir `<ExtensionAttributeValue>` görüntülenecek yeni dizesiyle.

### <a name="provide-a-list-of-values-by-using-localizedcollections"></a>LocalizedCollections kullanarak değerler listesini sağlayın
Yanıtlar için bir değer kümesi listesi sağlamak istiyorsanız, oluşturmak gereken bir `LocalizedCollections` özniteliği.  `LocalizedCollections` dizisidir `Name` ve `Value` çiftleri. Eklemek için `LocalizedCollections`, aşağıdaki biçimi kullanın:

```JSON
{
  "LocalizedStrings": [...],
  "LocalizedCollections": [{
      "ElementType":"ClaimType", 
      "ElementId":"<UserAttribute>",
      "TargetCollection":"Restriction",
      "Override": true,
      "Items":[
           {
                "Name":"<Response1>",
                "Value":"<Value1>"
           },
           {
                "Name":"<Response2>",
                "Value":"<Value2>"
           }
     ]
  }]
}
```

* `ElementId` Kullanıcı bu özniteliği olan `LocalizedCollections` yanıt olarak bir özniteliktir.
* `Name` kullanıcıya görüntülenen bir değerdir.
* `Value` Bu seçenek belirlendiğinde talepte döndürülür olur.

### <a name="upload-your-changes"></a>Değişikliklerinizi karşıya yüklemek
1. JSON dosyanız değişiklikleri tamamladıktan sonra B2C kiracınızın geri dönün.
2. Gelen **Düzenle İlkesi** sayfasında, **dil özelleştirme**.
3. İçin çevirmek istediğiniz dili seçin.
4. Çeviriler sağlamak istediğiniz sayfasını seçin.
5. Klasör simgesini seçin ve karşıya yüklemek için JSON dosyasını seçin.
 
Değişiklikler ilkeniz için otomatik olarak kaydedilir.

## <a name="customize-the-page-ui-by-using-language-customization"></a>Dil özelleştirmesi kullanarak UI sayfasını özelleştirme

HTML içeriğinizi yerelleştirme için iki yolu vardır. Açmak için tek yönlü olduğu [dil özelleştirme](active-directory-b2c-reference-language-customization.md). Bu özelliği etkinleştirmek sağlayan Open ID Connect parametre iletmek Azure AD B2C `ui-locales`, uç noktanız için.  İçeriğinize dile özgü özelleştirilmiş HTML sayfaları sağlamak için bu parametreyi kullanabilirsiniz.

Alternatif olarak, farklı yerlerde kullanılan bölgesel ayarına göre içerik isteyecek. CORS etkin uç noktanızı içinde belirli diller için içeriği barındırmak için bir klasör yapısı ayarlayabilirsiniz. Joker karakter değeri kullanırsanız, doğru olanı çağırmanız `{Culture:RFC5646}`.  Örneğin, bu özel sayfanızı URI olduğunu varsayın:

```
https://wingtiptoysb2c.blob.core.windows.net/{Culture:RFC5646}/wingtip/unified.html
```
Sayfanın yükleyebilir `fr`. Sayfanın HTML ve CSS içerik çeker, gelen çekiyor:
```
https://wingtiptoysb2c.blob.core.windows.net/fr/wingtip/unified.html
```

## <a name="add-custom-locales"></a>Özel yerel ayarlar ekleme

Microsoft çevirileri için şu anda sağlamaz dilleri de ekleyebilirsiniz. İlke tüm dizelerde çevirileri sağlamak gerekir.

1. Gelen **Düzenle İlkesi** sayfasında, **dil özelleştirme**.
2. Seçin **özel dil eklemek** sayfasının üstten.
3. Dili, çevirileri için geçerli bir yerel ayar kodunu girerek sağlama açan bağlam bölmesinde tanımlayın.
4. Her bir sayfa için geçersiz kılmaları kümesi için İngilizce indirin ve çevirileri üzerinde çalışır.
5. JSON dosyalarıyla bitirdikten sonra her bir sayfa için karşıya yükleyebilirsiniz.
6. Seçin **etkinleştirmek**, ve ilkeniz şimdi kullanıcılarınız için bu dilde gösterebilir.
7. Dil kaydedin.

## <a name="additional-information"></a>Ek bilgiler

### <a name="page-ui-customization-labels-as-overrides"></a>Sayfanın UI Özelleştirme etiketleri olarak geçersiz kılmaları
Dil özelleştirme etkinleştirdiğinizde, önceki düzenlemeleriniz sayfası kullanıcı arabirimini özelleştirme kullanarak etiketleri için İngilizce (TR) için bir JSON dosyası kalıcıdır. Dil özelleştirme dil kaynaklarının yükleyerek etiketleri ve diğer dizeleri değiştirmeye devam edebilirsiniz.
### <a name="up-to-date-translations"></a>Güncel çevirileri
Microsoft, kullanımınız için en güncel çevirileri sağlamayı taahhüt eder. Microsoft, sürekli olarak çevirileri artırır ve bunları sizin için Uyumluluk tutar. Microsoft hatalar ve genel terminolojisi değişiklikleri tanımlamak ve çalışacak güncelleştirmeleri sorunsuz kullanıcı Yolculuğunuzun yapın.
### <a name="support-for-right-to-left-languages"></a>Sağdan sola yazılan diller için destek
Microsoft, şu anda sağdan sola yazılan diller için destek sağlamaz. Bu özellik gerekiyorsa, lütfen için oy [Azure geri bildirim](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/19393000-provide-language-support-for-right-to-left-languag).
### <a name="social-identity-provider-translations"></a>Sosyal kimlik sağlayıcısı çevirileri
Microsoft sağlar `ui_locales` sosyal oturum açmalar OIDC parametresi. Ancak, Facebook ve Google, dahil olmak üzere bazı sosyal kimlik sağlayıcıları dikkate yok. 
### <a name="browser-behavior"></a>Tarayıcı davranışı
Chrome ve Firefox her ikisi de kümesi dillerini isteyin. Desteklenen bir dil ise, önce varsayılan görüntülenir. Edge şu anda bir dil istemez ve düz varsayılan dili geçer.

### <a name="supported-languages"></a>Desteklenen diller

| Dil              | Dil kodu |
|-----------------------|---------------|
| Bangla dili                | bn            |
| Çekçe                 | cs            |
| Danca                | da            |
| Almanca                | de            |
| Yunanca                 | el            |
| Türkçe               | tr            |
| İspanyolca               | es            |
| Fince               | fi            |
| Fransızca                | fr            |
| Gucerat dili              | gu            |
| Hintçe                 | hi            |
| Hırvatça              | sa.            |
| Macarca             | hu            |
| İtalyanca               | it            |
| Japonca              | ja            |
| Kannada dili               | kn            |
| Kore dili                | ko            |
| Malayalam dili             | ml            |
| Marathi dili               | mr            |
| Malay dili                 | ms            |
| Norveççe Bokmal      | nb            |
| Hollanda dili                 | nl            |
| Pencap dili               | pa            |
| Lehçe                | pl            |
| Portekizce - Brezilya   | pt-br         |
| Portekizce - Portekiz | pt-pt         |
| Rumence              | ro            |
| Rusça               | ru            |
| Slovakça                | SK            |
| İsveç dili               | sv            |
| Tamil dili                 | ta            |
| Telugu dili                | metin            |
| Tay Dili                  | TH            |
| Türkçe               | tr            |
| Çince - Basitleştirilmiş  | zh-atanır       |
| Geleneksel Çince- | zh-hant       |
