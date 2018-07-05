---
title: Dil özelleştirme, Azure Active Directory B2C | Microsoft Docs
description: Dil deneyimini özelleştirme hakkında bilgi edinin.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/26/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: affd52352dcc745557dd66c61ccfa1e7a99dcdb7
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37442330"
---
# <a name="language-customization-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de dil özelleştirme

Dil özelleştirme, Azure Active Directory B2C'yi (Azure AD B2C) ilkeniz farklı dillerde müşterilerinizin ihtiyaçlarına uyacak şekilde uyum sağlayacak şekilde izin verir.  Microsoft'un sağladığı çevirilerini [36 diller](#supported-languages), ancak herhangi bir dilde kendi çevirilerinizi de sağlayabilirsiniz. Deneyiminizi yalnızca tek bir dil için sağlanan bile herhangi bir metin sayfalarında özelleştirebilirsiniz.  

## <a name="how-language-customization-works"></a>Dil özelleştirme nasıl çalışır?
Dil özelleştirme, kullanıcı yolculuğu kullanılabilir diller seçmek için kullanın. Bu özellik etkinleştirildikten sonra sorgu dizesi parametresi sağlayabilirsiniz `ui_locales`, uygulamanızdan. Azure AD B2C'ye çağırdığınızda, sayfanız belirttiniz yerel ayar çevrilir. Bu tür bir yapılandırma, kullanıcı yolculuğunda dilleri üzerinde tam denetim verir ve müşterinin tarayıcı dil ayarı yok sayar. 

Müşterinizin görür hangi dilleri üzerinde denetim düzeyi gerekmeyebilir. Sağlamıyorsa bir `ui_locales` parametresi, müşteri deneyimi, tarayıcı ayarları tarafından belirlenir.  Hangi diller, kullanıcı yolculuğunun desteklenen bir dil ekleyerek çevrilir yine de denetleyebilirsiniz. Bir müşterinin tarayıcı desteklemek istemediğiniz bir dili göstermek üzere ayarlarsanız, bunun yerine varsayılan olarak desteklenen kültürler seçtiğiniz dil gösterilir.

- **kullanıcı arabirimi yerel belirtilen dil**: dil özelleştirme etkinleştirdikten sonra kullanıcı yolculuğunuza burada belirtilen dile çevrilir.
- **Tarayıcı istenen dil**: Hayır ise `ui_locales` parametre belirtildi, kullanıcı yolculuğunuza tarayıcı tarafından istenen dili için çevrilmiş *dil destekleniyorsa*.
- **İlkenin varsayılan dili**: tarayıcı bir dil belirtmiyor veya desteklenmeyen bir belirtir, kullanıcı yolculuğu ilkesi varsayılan dile çevrilir.

>[!NOTE]
>Özel kullanıcı öznitelikleri kullanıyorsanız, kendi çevirilerinizi sağlamanız gerekir. Daha fazla bilgi için [dizelerinizi özelleştirme](#customize-your-strings).
>

## <a name="support-requested-languages-for-uilocales"></a>Ui_locales ayarlarını için istenen dilleri destekler 
Dil özelleştirme genel kullanılabilirliğini önce oluşturulan ilkeleri önce bu özelliği etkinleştirmeniz gerekir. Dil özelleştirme, varsayılan olarak etkin sonra oluşturulan ilkeleri vardır. 

Dil özelleştirme ilkesinde etkinleştirdiğinizde ekleyerek kullanıcı yolculuğu dili denetleyebilirsiniz `ui_locales` parametresi.
1. [Azure portalındaki B2C özellikleri sayfasına gidin](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-app-registration#navigate-to-b2c-settings).
2. Çevirileri için etkinleştirmek istediğiniz bir ilke göz atın.
3. Seçin **dil özelleştirme**.  
4. Seçin **dil özelleştirmeyi etkinleştirin**.
5. İletişim kutusundaki bilgileri okuyun ve seçin **Evet**.

## <a name="select-which-languages-in-your-user-journey-are-enabled"></a>Kullanıcı Yolculuğunuzun hangi dillerde etkinleştirileceğini seçin 
Bir dizi için tarayıcı tarafından istendiğinde çevrilecek, kullanıcı yolculuğu için dilleri etkinleştirmek `ui_locales` parametresi.
1. İlkenizi dil özelleştirme önceki yönergeleri etkin olduğundan emin olun.
2. Gelen **ilkesini Düzenle** sayfasında **dil özelleştirme**.
3. Desteklemek istediğiniz bir dil seçin.
4. Özellikler bölmesinde değiştirmek **etkin** için **Evet**.  
5. Seçin **Kaydet** özellikler bölmesinin üst.

>[!NOTE]
>Varsa bir `ui_locales` parametresi sağlanmadığından, yalnızca etkinse sayfa müşterinin tarayıcı dile çevrilir.
>

## <a name="customize-your-strings"></a>Dizelerinizi özelleştirme
Dil özelleştirme, herhangi bir dize kullanıcı yolculuğunuzda özelleştirmenize olanak sağlar.
1. İlkenizi dil özelleştirme önceki yönergeleri etkin olduğundan emin olun.
2. Gelen **ilkesini Düzenle** sayfasında **dil özelleştirme**.
3. Özelleştirmek istediğiniz dili seçin.
4. Düzenlemek istediğiniz sayfayı seçin.
5. Seçin **Varsayılanları indirme** (veya **geçersiz kılmaları indirme** daha önce bu dil düzenlediyseniz). 

Bu adımları dizelerinizi düzenlemeye başlamak için kullanabileceğiniz bir JSON dosyası belirtin.

### <a name="change-any-string-on-the-page"></a>Sayfadaki herhangi bir dize değiştirme
1. Önceki yönergeleri bir JSON düzenleyicisinde indirilen JSON dosyasını açın.
2. Değiştirmek istediğiniz öğeyi bulun.  Bulabilirsiniz `StringId` aradığınız ya da Aranan dize için `Value` değiştirmek istediğiniz özniteliği.
3. Güncelleştirme `Value` görüntülenmesini istediğiniz ile özniteliği.
4. Değiştirmek istediğiniz her dize için değiştirme `Override` için `true`.
5. Dosyayı kaydedin ve değişikliklerinizi karşıya yükleyin. (Karşıya yükleme denetimi JSON dosyasını indirdiğiniz olarak aynı yerde bulabilirsiniz.) 

>[!IMPORTANT]
>Dize geçersiz kılmak isterseniz ayarladığınızdan emin olun `Override` değerini `true`.  Giriş değeri değiştirilmez, göz ardı edilir. 
>

### <a name="change-extension-attributes"></a>Uzantı özniteliklerini değiştirme
Dize için bir özel kullanıcı özniteliğini değiştirmek istediğinizde ya da bir JSON eklemek istediğiniz, şu biçimde şöyledir:
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

Değiştirin `<ExtensionAttribute>` ile kendi özel kullanıcı özniteliğinin adı.  

Değiştirin `<ExtensionAttributeValue>` görüntülenecek yeni bir dize ile.

### <a name="provide-a-list-of-values-by-using-localizedcollections"></a>LocalizedCollections kullanarak değerlerin bir listesini sağlar.
Yanıtlar için değerler kümesi listesini sağlamak istiyorsanız, oluşturmak gereken bir `LocalizedCollections` özniteliği.  `LocalizedCollections` dizisidir `Name` ve `Value` çiftleri. Öğelerin sırasını görüntülendikleri sıralaması olacaktır.  Eklenecek `LocalizedCollections`, aşağıdaki biçimi kullanın:

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
* `Name` Kullanıcıya gösterilen değerdir.
* `Value` Bu seçenek belirlendiğinde talepte döndürülür olur.

### <a name="upload-your-changes"></a>Değişikliklerinizi karşıya yükleme
1. JSON dosyanız için Değişiklikleri tamamladıktan sonra B2C kiracınıza geri dönün.
2. Gelen **ilkesini Düzenle** sayfasında **dil özelleştirme**.
3. Çevirmek için istediğiniz dili seçin.
4. Çevirileri sağlamak istediğiniz sayfayı seçin.
5. Klasör simgesini seçin ve karşıya yüklemek için JSON dosyasını seçin.
 
Değişiklikler ilkeniz için otomatik olarak kaydedilir.

## <a name="customize-the-page-ui-by-using-language-customization"></a>Dil özelleştirmesi kullanılarak sayfası kullanıcı Arabirimi özelleştirme

HTML, içeriği yerelleştirmek için iki yolu vardır. Açmak için bir yolu olan [dil özelleştirme](active-directory-b2c-reference-language-customization.md). Bu özellik çalışabilmelerini Open ID Connect parametresi iletmek için Azure AD B2C `ui-locales`, uç noktanıza.  İçerik sunucunuzu Bu parametre, dile özgü olan özelleştirilmiş HTML sayfalarını sağlamak için kullanabilirsiniz.

Alternatif olarak, farklı konumlardan kullanılan yerel ayarları temel alarak içerik çekin. CORS özellikli uç noktanızı içinde belirli diller için ana içerik için bir klasör yapısı ayarlayabilirsiniz. Joker karakter değeri kullanırsanız, sizi doğru olanı ararız `{Culture:RFC5646}`.  Örneğin, bu, özel sayfa URI'si olduğunu varsayın:

```
https://wingtiptoysb2c.blob.core.windows.net/{Culture:RFC5646}/wingtip/unified.html
```
Sayfanın yükleyebilir `fr`. Sayfanın HTML ve CSS içerik çeker, gelen çekiyor:
```
https://wingtiptoysb2c.blob.core.windows.net/fr/wingtip/unified.html
```

## <a name="add-custom-languages"></a>Özel dil Ekle

Microsoft şu anda çevirileri için sağlamaz dilleri de ekleyebilirsiniz. İlke tüm dizeler için çevirileri sağlamanız gerekir.  Dili ve yerel ayar kodu olan ISO 639-1 standart sınırlıdır. 

1. Gelen **ilkesini Düzenle** sayfasında **dil özelleştirme**.
2. Seçin **özel dil Ekle** sayfanın üst.
3. Açılır bulunan bağlam bölmesinde, hangi dil çevirileri için geçerli yerel ayar kodunu girerek sunuyorsunuz belirleyin.
4. Her bir sayfa için İngilizce için bir dizi geçersiz kılmaları indirme ve çevirileri üzerinde çalışır.
5. JSON dosyaları ile bitirdikten sonra her sayfa için karşıya yükleyebilirsiniz.
6. Seçin **etkinleştirme**, ve ilkeniz bu dil kullanıcılarınız için şimdi gösterebilir.
7. Dili Kaydet.

>[!IMPORTANT]
>Özel dil etkinleştirmek veya kaydedebilmek için geçersiz kılmalar karşıya gerekir.
>

## <a name="additional-information"></a>Ek bilgiler

### <a name="page-ui-customization-labels-as-overrides"></a>Sayfa UI özelleştirmesi etiketleri geçersiz kılma olarak
Dil özelleştirme etkinleştirdiğinizde, önceki düzenlemeleriniz sayfa UI özelleştirmesi kullanılarak etiketleri için İngilizce (TR) için bir JSON dosyasında kalıcıdır. Dil özelleştirme dil kaynakları yükleyerek etiketleri ve diğer dizeleri değiştirmeye devam edebilirsiniz.
### <a name="up-to-date-translations"></a>Güncel çevirileri
Microsoft, kendi kullanımınız için en güncel çevirileri sağlamaya odaklanmıştır. Microsoft, sürekli olarak çevirileri artırır ve bunları sizin için Uyumluluk tutar. Microsoft hataları ve değişiklikleri genel terminolojisinde tanımlar ve çalışacak güncelleştirmeleri, kullanıcı yolculuğunda sorunsuz bir şekilde yapın.
### <a name="support-for-right-to-left-languages"></a>Sağdan sola dil desteği
Microsoft, şu anda sağdan sola diller için destek sağlamaz. Bu, özel yerel ayarlar kullanarak ve dizeleri görüntülenme şeklini değiştirmek için CSS kullanarak gerçekleştirebilirsiniz.  Bu özellik gerekiyorsa, lütfen için oy verin [Azure geri bildirim](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/19393000-provide-language-support-for-right-to-left-languag).
### <a name="social-identity-provider-translations"></a>Sosyal kimlik sağlayıcısı çevirileri
Microsoft'un sağladığı `ui_locales` OIDC parametresi sosyal oturum açma bilgileri. Ancak, Facebook ve Google gibi bazı sosyal kimlik sağlayıcıları dikkate yok. 
### <a name="browser-behavior"></a>Tarayıcı davranışı
Chrome ve Firefox hem de set dillerini isteyin. Desteklenen bir dil etkinleştirilmişse, önce varsayılan görüntülenir. Edge şu anda bir dil istemez ve düz varsayılan dili gider.

### <a name="supported-languages"></a>Desteklenen diller

| Dil              | Dil kodu |
|-----------------------|---------------|
| Bangla                | Bn            |
| Çekçe                 | cs            |
| Danca                | da            |
| Almanca                 | de            |
| Yunanca                 | el            |
| Türkçe               | tr            |
| İspanyolca                | es            |
| Fince               | fi            |
| Fransızca                 | fr            |
| Gucerat dili              | gu            |
| Hintçe                 | Merhaba            |
| Hırvatça              | sa.            |
| Macarca             | hu            |
| İtalyanca               | it            |
| Japonca              | ja            |
| Kannada dili               | KN            |
| Kore dili                | ko            |
| Malayalam dili             | ML            |
| Marathi dili               | MR            |
| Malay dili                 | ms            |
| Norveççe Bokmal      | nb            |
| Felemenkçe                 | nl            |
| Pencap dili               | Pa            |
| Lehçe                | pl            |
| Portekizce - Brezilya   | pt-br         |
| Portekizce - Portekiz | pt-pt         |
| Rumence              | ro            |
| Rusça               | ru            |
| Slovakça                | SK            |
| İsveç dili               | sv            |
| Tamil dili                 | veri            |
| Telugu dili                | metin            |
| Tay Dili                  | .            |
| Türkçe               | tr            |
| Çince - Basitleştirilmiş  | zh-hans       |
| Çince - Geleneksel | zh-hant       |
