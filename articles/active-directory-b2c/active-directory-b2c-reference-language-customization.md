---
title: "Azure Active Directory B2C: dil özelleştirme kullanarak | Microsoft Docs"
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
ms.date: 04/25/2017
ms.author: sama
ms.openlocfilehash: 3c7c49ee5fbd98762da0eef6f02e7c2f036453cb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-active-directory-b2c-using-language-customization"></a>Azure Active Directory B2C: Kullanarak dil özelleştirme

>[!NOTE] 
>Bu özellik genel önizlemede değil.  Bu özelliği kullanırken bir test Kiracı kullanmanız önerilir.  Biz Önizleme tüm önemli değişiklikler genel kullanılabilirlik yayın üzerinde planı yoktur, ancak biz özelliği artırmak için bu değişiklikleri yapma hakkı yedek.  Lütfen, özellik denemek için bir ettikten sonra deneyimlerinizi geribildirim ve nasıl biz bunu daha iyi duruma getirebilirsiniz belirtin.  Sağ üstteki gülen yüz aracıyla Azure Portalı aracılığıyla geribildirim sağlayabilir.   Bir iş gereksinimi, bu özellik Önizleme aşamasında kullanarak canlı gidin, bize bildirin ise senaryolarınızı ve biz uygun Kılavuzu ve Yardım sağlayabilir.  Adresinden bize başvurun [ aadb2cpreview@microsoft.com ](mailto:aadb2cpreview@microsoft.com).

Dil özelleştirme müşteri gereksinimlerinize uygun olarak farklı bir dil için kullanıcı Yolculuğunuzun değiştirmenize izin verir.  36 diller için çeviriler sağladığımız (bkz [ek bilgi](#additional-information)).  Deneyiminizi yalnızca tek bir dil için sağlanan olsa bile, gereksinimlerinize uygun olarak sayfalarında herhangi bir metin özelleştirebilirsiniz.  

## <a name="how-does-language-customization-work"></a>Dil özelleştirme nasıl çalışır?
Dil özelleştirme kullanıcı Yolculuğunuzun kullanılabilir diller seçmenize olanak sağlar.  Bu özellik etkinleştirildikten sonra sorgu dizesi parametresi ui_locales, uygulamanızdan sağlayabilir.  Azure AD B2C çağırdığınızda, biz belirttiniz yerel sayfanıza çevir.  Yapılandırma türünü kullanan kullanıcı Yolculuğunuzun dilleri üzerinde tam denetim verir ve Müşteri'nin tarayıcı dil ayarlarını yoksayar. Alternatif olarak, müşteriniz bkz hangi dilde üzerinde denetim düzeyini gerekmeyebilir.  Ui_locales parametresi sağlamazsanız, müşteri deneyimi, tarayıcının ayarları tarafından dikte edilir.  Kullanıcı Yolculuğunuzun desteklenen bir dil ekleyerek çevrilir hangi dilleri hala kontrol edebilirsiniz.  Müşteri'nin tarayıcı desteklemek istemediğiniz bir dili göstermek üzere ayarlarsanız, varsayılan olarak desteklenen kültürler seçilen dil yerine gösterilir.

1. **kullanıcı arabirimi yerel ayarlar belirtilen dil** -dil özelleştirme etkinleştirdiğinizde, kullanıcı Yolculuğunuzun burada belirtilen dil çevrilmiştir
2. **Tarayıcı istenen dil** -kullanıcı arabirimi yerel belirtildi, tarayıcıya çevirir dil, istenen **desteklenen dilleri eklediyseniz**
3. **İlke varsayılan dil** -tarayıcı bir dil belirtmeyen veya desteklenmeyen bir belirtir, ilkesi varsayılan dili çevirir

>[!NOTE]
>Özel kullanıcı özniteliklerini kullanıyorsanız, kendi çevirileri sağlamanız gerekir. Bkz. '[dizelerinizi özelleştirme](#customize-your-strings)' Ayrıntılar için.
>

## <a name="support-uilocales-requested-languages"></a>Dil desteği ui_locales istendi 
Bir ilkedeki ' dil özelleştirme' etkinleştirerek ui_locales parametresini ekleyerek kullanıcı gezisine dilinin şimdi kontrol edebilirsiniz.
1. [Azure portalındaki B2C özellikleri dikey penceresine gitmek için aşağıdaki adımları izleyin.](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-app-registration#navigate-to-b2c-settings)
2. Çevirileri için etkinleştirmek istediğiniz ilkesine gidin.
3. Tıklatın **dil özelleştirme**.
4. Uyarıyı okuyun dikkatle.  Etkinleştirildiğinde, 'Devre dışı dil özelleştirme' kapatamazsınız.
5. Özelliği etkinleştirmek ve tıklayın **Tamam**.
6. Sol üst köşesinin üzerinde ilkeniz kaydetmek, **Düzenle İlkesi** dikey.

## <a name="select-which-languages-your-user-journey-supports"></a>Kullanıcı Yolculuğunuzun destekleyen hangi dilleri seçin 
Kullanıcı Yolculuğunuzun ui_locales parametresi sağlanmadığında içinde çevrilmesi için izin verilen dillerin bir listesi oluşturun.
1. 'Önceki yönergeleri etkin dil özelleştirme' ilkeniz olduğundan emin olun.
2. Öğesinden, **Düzenle İlkesi** dikey penceresinde, select **dil özelleştirme**.
3. Gittiğiniz, **desteklenen diller** dikey.  Buradan, seçtiğiniz **dil eklemek**.
4. Desteklenen istediğiniz tüm dilleri seçin.  

>[!NOTE]
>Ui_locales parametresi sağlanmazsa, yalnızca bu listede ise daha sonra sayfa Müşteri'nin tarayıcı dil çevrilmiştir.
>

5. Tıklatın **Tamam** altındaki
6. Kapat **dil özelleştirme** dikey ve **kaydetmek** ilkeniz.

## <a name="customize-your-strings"></a>Dizelerinizi özelleştirme
'Dil özelleştirme' kullanıcı Yolculuğunuzun herhangi bir dize özelleştirmenizi sağlar.
1. 'Önceki yönergeleri etkin dil özelleştirme' ilkeniz olduğundan emin olun.
2. Öğesinden, **Düzenle İlkesi** dikey penceresinde, select **dil özelleştirme**.
3. Sol taraftaki gezinti menüsünde seçin **içeriği indirirken**.
4. Düzenlemek istediğiniz sayfasını seçin.
5. Açılır listede, düzenlemek istediğiniz dili seçin.
6. **İndir**’e tıklayın. 

Bu adımları dizelerinizi düzenlemeye başlamak için kullanabileceğiniz bir JSON dosyası verin.

### <a name="changing-any-string-on-the-page"></a>Sayfadaki herhangi bir dize değiştirme
1. Önceki yönergeleri bir JSON Düzenleyicisi'ni indirilen JSON dosyasını açın.
2. Değiştirmek istediğiniz öğesi bulunamıyor.  Bulabileceğiniz `StringId` aramakta olduğunuz veya aramak dizenin `Value` değiştirmek istediğinizde.
3. Güncelleştirme `Value` görüntülenmesini istediğiniz ile özniteliği.
4. Dosyayı kaydedin ve değişikliklerinizi karşıya yükleyin.

### <a name="changing-extension-attributes"></a>Uzantı özniteliklerini değiştirme
Özel kullanıcı öznitelik dizesini değiştirebilir veya JSON birine eklemek istediğiniz arıyorsanız, biçimi aşağıda gösterilmiştir:
```JSON
{
  "LocalizedStrings": [
    {
      "ElementType": "ClaimType",
      "ElementId": "extension_<ExtensionAttribute>",
      "StringId": "DisplayName",
      "Override": false,
      "Value": "<ExtensionAttributeValue>"
    }
    [...]
}
```
>[!IMPORTANT]
>Bir dize geçersiz kılmak gerekiyorsa, ayarladığınızdan emin olun `Override` değeri `true`.  Giriş değeri değiştirilmez, göz ardı edilir. 
>

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
      "Override": false,
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
>[!IMPORTANT]
>Bir dize geçersiz kılmak gerekiyorsa, ayarladığınızdan emin olun `Override` değeri `true`.  Giriş değeri değiştirilmez, göz ardı edilir. 
>

* `ElementId`Kullanıcı bu özniteliği olan `LocalizedCollections` yanıt olarak
* `Name`değer, kullanıcıya gösterilir
* `Value`Bu seçenek belirlendiğinde talepte döndürülür olduğu

### <a name="upload-your-changes"></a>Değişikliklerinizi karşıya yüklemek
1. JSON dosyanız değişiklikleri tamamladıktan sonra B2C kiracınızın geri gidin.
2. Öğesinden, **Düzenle İlkesi** dikey penceresinde, select **dil özelleştirme**.
3. Sol taraftaki gezinti menüsünde seçin **içerik Yükle**.
4. Değişikliklerinizi için karşıya yüklemek istediğiniz sayfasını seçin.
5. JSON için önceden sağladığınız bir dil yüklemek istiyorsanız, önceki girdi silmeniz gerekir.  Tıklatarak silebilirsiniz `...` , dil ve seçin, sağ taraftaki **silmek**.
6. Tıklatın **Ekle** üst sol.
7. JSON dosyanız dili seçin.
8. Sağdaki klasör düğmesini tıklatın ve JSON dosyanız için göz atın.
9. Tıklatın **karşıya** dikey pencerenin altındaki düğmesinde.
10. Geri dönerek, **Düzenle İlkesi** tıklayın ve dikey **kaydetmek**.

## <a name="additional-information"></a>Ek bilgiler
### <a name="recommendations-for-language-customization"></a>'Dil özelleştirme' için öneriler
Dil kaynaklarınıza değiştirmek için istediğiniz dizeleri girişler yalnızca koyma öneririz.  Biz, bir boyut sınırı dışında tüm JSON çevirileri derlenmiş dosyasına uygulayın.  Dosyalarınızı çok büyük alırsanız, kullanıcı Yolculuğunuzun performansını etkiler.
### <a name="page-ui-customization-labels-are-removed-once-language-customization-is-enabled"></a>'Dil özelleştirme' etkinleştirildikten sonra sayfası kullanıcı Arabirimi özelleştirme etiketleri kaldırılır
'Dil özelleştirme' etkinleştirdiğinizde, önceki düzenlemeleriniz sayfa UI Özelleştirme kullanarak etiketleri için özel kullanıcı özniteliklerini dışında kaldırılır.  Bu değişiklik, dizelerinizi düzenleyebileceğiniz, çakışmaları önlemek için yapılır.  Dil Kaynakları 'Dil özelleştirmesinde' yükleyerek etiketleri ve diğer dizeleri değiştirmeye devam edebilirsiniz.
### <a name="microsoft-is-committed-to-provide-the-most-up-to-date-translations-for-your-use"></a>Microsoft kullanımınız için en güncel Çeviriler sağlamayı taahhüt eder
Biz sürekli çevirileri geliştirmek ve bunları sizin için Uyumluluk tutun.  Biz hatalar ve genel terminolojisi değişiklikleri tanımlamak ve çalışacak güncelleştirmeleri sorunsuz kullanıcı Yolculuğunuzun yapın.
### <a name="support-for-right-to-left-languages"></a>Sağdan sola yazılan diller için destek
Bu özellik Lütfen oy bu özelliği için gerekiyorsa sağdan sola yazılan diller için destek yok olduğundan [Azure geri bildirim](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/19393000-provide-language-support-for-right-to-left-languag).
### <a name="social-identity-provider-translations"></a>Sosyal kimlik sağlayıcısı çevirileri
Sosyal oturum açmalar ui_locales OIDC parametresi sağladığımız ancak Facebook ve Google dahil olmak üzere bazı sosyal kimlik sağlayıcıları tarafından onaylanmaz. 
### <a name="browser-behavior"></a>Tarayıcı davranışı
Chrome ve Firefox her ikisini de kümesi dillerini iste ve desteklenen bir dil ise, önce varsayılan görüntülenir.  Edge şu anda bir dil istemez ve düz varsayılan dili geçer.
### <a name="known-issues"></a>Bilinen sorunlar
* Bir profili düzenlemek ilke MFA sayfa için Türkçe kaynaklar karşıya yükleme şu anda kullanılamıyor.
* `LocalizedCollections`yanıt türü tarafından istendiğinde değerlerini oluşturulan değil
### <a name="what-if-i-want-a-language-that-isnt-supported"></a>Desteklenmeyen bir dil ne istiyorum?
Uzantı 'özel diller' doğrultusunda JSON kaynak karşıya yüklemenize olanak sağlayan bu özelliğin sağlamak planlıyorsanız.  Bu özellik, herhangi bir dil için ad ve dil kodu belirtin ve sağlamanıza olanak sağlar *tüm* çevirileri o dil için.  Bu özellik gerekiyorsa, senaryonuza en bize gönderin [ aadb2cpreview@microsoft.com ](mailto:aadb2cpreview@microsoft.com).  

### <a name="what-languages-are-supported"></a>Hangi dilleri destekleniyor mu?

| Dil              | Dil kodu |
|-----------------------|---------------|
| Bangla                | Bn            |
| Çekçe                 | cs            |
| Danca                | da            |
| Almanca                | de            |
| Yunanca                 | el            |
| Türkçe               | en            |
| İspanyolca               | es            |
| Fince               | fi            |
| Fransızca                | fr            |
| Gucerat dili              | gu            |
| Hintçe                 | yüksek            |
| Hırvatça              | İK            |
| Macarca             | hu            |
| İtalyanca               | it            |
| Japonca              | ja            |
| Kannada dili               | KN            |
| Kore dili                | ko            |
| Malayalam dili             | ML            |
| Marathi               | MR            |
| Malay dili                 | MS            |
| Norveççe Bokmal      | nb            |
| Hollanda dili                 | nl            |
| Pencap dili               | Pa            |
| Lehçe                | pl            |
| Portekizce - Brezilya   | pt-br         |
| Portekizce - Portekiz | pt-pt         |
| Rumence              | ro            |
| Rusça               | ru            |
| Slovakça                | SK            |
| İsveç dili               | sv            |
| Tamil dili                 | eri            |
| Telugu dili                | metin            |
| Tay dili                  | TH            |
| Türkçe               | tr            |
| Çince - Basitleştirilmiş  | zh-atanır       |
| Geleneksel Çince- | zh-hant       |
