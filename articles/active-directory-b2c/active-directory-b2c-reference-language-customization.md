---
title: "Dil özelleştirme - Azure AD B2C kullanarak | Microsoft Docs"
description: 
services: active-directory-b2c
documentationcenter: 
author: sama
ms.assetid: eec4d418-453f-4755-8b30-5ed997841b56
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 02/26/2018
ms.author: sama
ms.openlocfilehash: 332c6b4ffd841a2c7aef9db5c8ba9e843790f4d6
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="azure-active-directory-b2c-using-language-customization"></a>Azure Active Directory B2C: Kullanarak dil özelleştirme

>[!NOTE]
>Bu özellik genel önizlemede değil.
>

Dil özelleştirme ilkeniz müşteri gereksinimlerinize uygun olarak farklı dillerde uyum sağlar.  Microsoft, 36 diller için çeviriler sağlar (bkz [ek bilgi](#additional-information)), ancak herhangi bir dil için kendi Çeviriler de sağlayabilirsiniz.  Deneyiminizi yalnızca tek bir dil için sağlanan olsa bile herhangi bir metin sayfalarında özelleştirebilirsiniz.  

## <a name="how-does-language-customization-work"></a>Dil özelleştirme nasıl çalışır?
Dil özelleştirme kullanıcı Yolculuğunuzun kullanılabilir diller seçmenize olanak sağlar.  Bu özellik etkinleştirildikten sonra sorgu dizesi parametresi ui_locales, uygulamanızdan sağlayabilir.  Azure AD B2C çağırdığınızda, biz belirttiniz yerel sayfanıza çevir.  Bu tür bir yapılandırma kullanıcı Yolculuğunuzun dilleri üzerinde tam denetim verir ve Müşteri'nin tarayıcı dil ayarlarını yoksayar. Alternatif olarak, müşteriniz bkz hangi dilde üzerinde denetim düzeyini gerekmeyebilir.  Ui_locales parametresi sağlamazsanız, müşteri deneyimi, tarayıcının ayarları tarafından dikte edilir.  Kullanıcı Yolculuğunuzun desteklenen bir dil ekleyerek çevrilir hangi dilleri hala kontrol edebilirsiniz.  Müşteri'nin tarayıcı desteklemek istemediğiniz bir dili göstermek üzere ayarlarsanız, varsayılan olarak desteklenen kültürler seçilen dil yerine gösterilir.

1. **kullanıcı arabirimi yerel ayarlar belirtilen dil** -dil özelleştirme etkinleştirdiğinizde, kullanıcı Yolculuğunuzun burada belirtilen dil çevrilmiştir
2. **Tarayıcı istenen dil** -kullanıcı arabirimi yerel belirtildi, tarayıcıya çevirir dil, istenen **desteklenen dilleri eklediyseniz**
3. **İlke varsayılan dil** -tarayıcı bir dil belirtmeyen veya desteklenmeyen bir belirtir, ilkesi varsayılan dili çevirir

>[!NOTE]
>Özel kullanıcı özniteliklerini kullanıyorsanız, kendi çevirileri sağlamanız gerekir. Bkz. '[dizelerinizi özelleştirme](#customize-your-strings)' Ayrıntılar için.
>

## <a name="support-uilocales-requested-languages"></a>Dil desteği ui_locales istendi 
Dil özelleştirme genel kullanılabilirlik sürümü önce oluşturulan ilkeleri bu özellik ilk etkinleştirmeniz gerekir.  Sonra oluşturulan ilkeler, varsayılan olarak etkin dil özelleştirme sahip olur.  Bir ilkedeki ' dil özelleştirme' etkinleştirerek ui_locales parametresini ekleyerek kullanıcı gezisine dilinin şimdi kontrol edebilirsiniz.
1. [Azure portalındaki B2C özellikleri sayfasına gitmek için aşağıdaki adımları izleyin.](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-app-registration#navigate-to-b2c-settings)
2. Çevirileri için etkinleştirmek istediğiniz ilkesine gidin.
3. Tıklatın **dil özelleştirme**.  
4. Tıklayın **etkinleştirmek dil özelleştirme** üstte.
5. İletişim kutusunu okuyun ve 'Evet'i tıklatın.

## <a name="select-which-languages-in-your-user-journey-are-enabled"></a>Kullanıcı Yolculuğunuzun hangi dillerde etkin seçin 
Kullanıcı Yolculuğunuzun ui_locales parametresi sağlanmadığında içinde çevrilmesi için dil kümesini sağlar.
1. 'Önceki yönergeleri etkin dil özelleştirme' ilkeniz olduğundan emin olun.
2. Öğesinden, **Düzenle İlkesi** sayfasında, **dil özelleştirme**.
3. Desteklemek istediğiniz dili seçin.
4. Özellikler bölmesinde geçiş **etkin** Evet.  
5. Tıklatın **kaydetmek** Özellikler bölmesinde üstünde.

>[!NOTE]
>Ui_locales parametresi sağlanmazsa, yalnızca etkinse, sonra sayfanın Müşteri'nin tarayıcı dil çevrilmiştir.
>

## <a name="customize-your-strings"></a>Dizelerinizi özelleştirme
'Dil özelleştirme' kullanıcı Yolculuğunuzun herhangi bir dize özelleştirmenizi sağlar.
1. 'Önceki yönergeleri etkin dil özelleştirme' ilkeniz olduğundan emin olun.
2. Öğesinden, **Düzenle İlkesi** sayfasında, **dil özelleştirme**.
3. Özelleştirmek istediğiniz dili seçin.
4. Düzenlemek istediğiniz sayfasını seçin.
5. Tıklatın **karşıdan varsayılanları** (veya **karşıdan geçersiz kılmaları** bu dilde daha önce düzenlediyseniz). 

Bu adımları dizelerinizi düzenlemeye başlamak için kullanabileceğiniz bir JSON dosyası verin.

### <a name="changing-any-string-on-the-page"></a>Sayfadaki herhangi bir dize değiştirme
1. Önceki yönergeleri bir JSON Düzenleyicisi'ni indirilen JSON dosyasını açın.
2. Değiştirmek istediğiniz öğesi bulunamıyor.  Bulabileceğiniz `StringId` aramakta olduğunuz veya aramak dizenin `Value` değiştirmek istediğinizde.
3. Güncelleştirme `Value` görüntülenmesini istediğiniz ile özniteliği.
4. Geçiş yapmak değiştirmek istediğiniz her dize için unutmayın `Override` için **doğru**.
5. Dosyayı kaydedin ve değişikliklerinizi (karşıya yükleme denetimi JSON dosyasını indirdiğiniz olarak aynı yerde bulabilirsiniz) karşıya yükleyin. 

>[!IMPORTANT]
>Bir dize geçersiz kılmak gerekiyorsa, ayarladığınızdan emin olun `Override` değeri `true`.  Giriş değeri değiştirilmez, göz ardı edilir. 
>

### <a name="changing-extension-attributes"></a>Uzantı özniteliklerini değiştirme
Özel kullanıcı öznitelik dizesini değiştirebilir veya JSON birine eklemek istediğiniz arıyorsanız, biçimi aşağıda gösterilmiştir:
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

### <a name="using-localizedcollections"></a>LocalizedCollections kullanma
Yanıtlar için bir değer kümesi listesi sağlamak istiyorsanız, oluşturmak gereken bir `LocalizedCollections`.  A `LocalizedCollections` dizisidir `Name` ve `Value` çiftleri.  `Name` Görüntülenenleri olduğu ve `Value` talep döndürülen değil.  Eklemek için bir `LocalizedCollections`, aşağıdaki biçime sahiptir:

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

* `ElementId` Kullanıcı bu özniteliği olan `LocalizedCollections` yanıt olarak
* `Name` değer, kullanıcıya gösterilir
* `Value` Bu seçenek belirlendiğinde talepte döndürülür olduğu

### <a name="upload-your-changes"></a>Değişikliklerinizi karşıya yüklemek
1. JSON dosyanız değişiklikleri tamamladıktan sonra B2C kiracınızın geri gidin.
2. Öğesinden, **Düzenle İlkesi** sayfasında, **dil özelleştirme**.
3. Çeviriler sağlamak istediğiniz dili seçin.
4. Çeviriler sağlamak istediğiniz sayfasını seçin.
5. Klasör simgesine tıklayın ve karşıya yüklemek için JSON dosyasını seçin.
6. Bu değiştirilen ilkeniz için otomatik olarak kaydedilir.

## <a name="using-page-ui-customization-with-language-customization"></a>Sayfanın UI Özelleştirme dil özelleştirme ile kullanma

HTML içeriğinizi yerelleştirme için iki yolu vardır.  Açarak ['Dil özelleştirme'](active-directory-b2c-reference-language-customization.md).  Bu özelliği etkinleştirmek sağlayan Open ID Connect parametre iletmek Azure AD B2C `ui-locales`, uç noktanız için.  İçeriğinize dile özgü özelleştirilmiş HTML sayfaları sağlamak için bu parametreyi kullanabilirsiniz.

Alternatif olarak, biz kullanılan bölgesel ayarına göre farklı konumlardan içerik çeker.  CORS'yi uç noktanızı belirli diller için içeriği barındırmak için bir klasör yapısı ayarlayabilirsiniz ve joker karakter değeri yerleştirirseniz doğru olanı arayacağız `{Culture:RFC5646}`.  Örneğin, bu my özel sayfa URI sahipsem:

```
https://wingtiptoysb2c.blob.core.windows.net/{Culture:RFC5646}/wingtip/unified.html
```
Sayfamda yük yükleyebilirsiniz `fr` ve html ve css içeriği çekme, gelen çeker:
```
https://wingtiptoysb2c.blob.core.windows.net/fr/wingtip/unified.html
```

## <a name="custom-locales"></a>Özel yerel ayarlar

Microsoft çevirileri için şu anda sağlamaz dilleri de ekleyebilirsiniz.  İlke tüm dizelerde çevirileri sağlamanız gerekir.

1. Öğesinden, **Düzenle İlkesi** sayfasında, **dil özelleştirme**.
2. Seçin **özel dil eklemek** sayfasının üstten.
3. Açılır içerik bölmesinde, hangi dil geçerli yerel ayar kodunu girerek çevirileri için sağlanmaktadır tanımlayın.
4. Her bir sayfa için geçersiz kılmaları kümesi için İngilizce indirin ve çevirileri üzerinde çalışır.
5. JSON dosyalarıyla tamamladıktan sonra her bir sayfa için karşıya yükleyebilirsiniz.
6. Seçin **etkinleştirmek** ve ilkeniz artık, kullanıcı için bu dil gösterebilir.
7. Bunu etkinleştirdikten sonra dil kaydetmeyi unutmayın.

## <a name="additional-information"></a>Ek bilgiler

### <a name="page-ui-customization-labels-are-persisted-as-your-first-set-of-overrides-once-language-customization-is-enabled"></a>'Dil özelleştirme' etkinleştirildikten sonra sayfası kullanıcı Arabirimi özelleştirme etiketleri ilk geçersiz kılmaları kümesi olarak kalıcı
'Dil özelleştirme' etkinleştirdiğinizde, önceki düzenlemeleriniz sayfa UI Özelleştirme kullanarak etiketleri için İngilizce (TR) için bir JSON dosyası kalıcıdır.  Dil Kaynakları 'Dil özelleştirmesinde' yükleyerek etiketleri ve diğer dizeleri değiştirmeye devam edebilirsiniz.
### <a name="microsoft-is-committed-to-provide-the-most-up-to-date-translations-for-your-use"></a>Microsoft kullanımınız için en güncel Çeviriler sağlamayı taahhüt eder
Biz sürekli çevirileri geliştirmek ve bunları sizin için Uyumluluk tutun.  Biz hatalar ve genel terminolojisi değişiklikleri tanımlamak ve çalışacak güncelleştirmeleri sorunsuz kullanıcı Yolculuğunuzun yapın.
### <a name="support-for-right-to-left-languages"></a>Sağdan sola yazılan diller için destek
Bu özellik Lütfen oy bu özelliği için gerekiyorsa biz şu anda desteği sağdan sola yazılan diller için sağlama olmayan [Azure geri bildirim](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/19393000-provide-language-support-for-right-to-left-languag).
### <a name="social-identity-provider-translations"></a>Sosyal kimlik sağlayıcısı çevirileri
Sosyal oturum açmalar ui_locales OIDC parametresi sağladığımız ancak Facebook ve Google dahil olmak üzere bazı sosyal kimlik sağlayıcıları tarafından onaylanmaz. 
### <a name="browser-behavior"></a>Tarayıcı davranışı
Chrome ve Firefox her ikisini de kümesi dillerini iste ve desteklenen bir dil ise, önce varsayılan görüntülenir.  Edge şu anda bir dil istemez ve düz varsayılan dili geçer.

### <a name="what-languages-are-supported"></a>Hangi dilleri destekleniyor mu?

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
