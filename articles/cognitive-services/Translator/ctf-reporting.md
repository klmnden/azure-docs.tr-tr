---
title: Microsoft Çeviricisi işbirliği çeviri Framework (CTF) raporlama
description: İşbirlikçi çeviri Framework (CTF) raporlama kullanma
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 12/14/2017
ms.author: v-jansko
ms.openlocfilehash: cefc630a82a56703ba4942bcad18f6e0a38b1ee5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352606"
---
# <a name="how-to-use-collaborative-translation-framework-ctf-reporting"></a>İşbirlikçi çeviri Framework (CTF) raporlama kullanma

> [!NOTE]
> Bu yöntem kullanım dışıdır. Çevirici metin API V3.0 içinde kullanılabilir değil.

> İşbirlikçi çevirileri Framework (CTF), Çevirici metin API V2.0 için daha önce kullanılabilir 1 Şubat 2018 sürümünden itibaren kullanımdan kaldırılmıştır. AddTranslation ve AddTranslationArray işlevleri düzeltmeleri işbirliği çeviri çerçevesi aracılığıyla etkinleştirmek açmalarına olanak tanır. 31 Ocak 2018 sonra bu iki işlevler yeni cümleyi göndermeleri kabul etmedi ve kullanıcıların bir hata iletisi alırsınız. Bu işlevler devre dışı ve değiştirilmeyecek. 

>Benzer işlevsellik terminoloji ve stili özel çeviri sistemiyle oluşturmanıza olanak sağlayan Çeviricisi Hub API kullanılabilir ve Kategori Kimliği Çeviricisi metin API kullanarak çalıştırabilirsiniz. Çevirici Hub: [ https://hub.microsofttranslator.com ](https://hub.microsofttranslator.com). Çevirici Hub API: [ https://hub.microsofttranslator.com/swagger ](https://hub.microsofttranslator.com/swagger).

İşbirlikçi çeviri Framework (CTF) raporlama API'si istatistikleri ve gerçek içerik CTF deposunda döndürür. Bu API GetTranslations() yönteminden farklıdır çünkü bunu:
* Çevrilmiş içeriği ve toplam sayımına yalnızca hesabınızdan (AppID veya Azure Market hesabı) döndürür.
* Çevrilmiş içeriği ve toplam sayımına bir eşleşme kaynak tümcenin gerek kalmadan döndürür.
* Otomatik çeviri (makine çevirisi) döndürmez.

## <a name="endpoint"></a>Uç Nokta
CTF raporlama API uç noktası http://api.microsofttranslator.com/v2/beta/ctfreporting.svc
                        

## <a name="methods"></a>Yöntemler
| Ad |    Açıklama|
|:---|:---|
| GetUserTranslationCounts yöntemi | Kullanıcı tarafından oluşturulmuş çevirileri sayısını alır. |
| GetUserTranslations yöntemi | Kullanıcı tarafından oluşturulmuş çevirileri alır. |

Bu yöntemler için etkinleştir:
* Kullanıcı çevirileri ve düzeltmeleri indirmek için hesap kimliği altında tamamını alın.
* Sık katkıda bulunanlar listesini edinin. Doğru kullanıcı adını AddTranslation() sağlandığından emin olun.
* URI ön ekini temel alarak sitenizin bölümünü sınırlı sayesinde güvenilen kullanıcılarınız gerekirse, tüm kullanılabilir adayları görmek bir kullanıcı arabirimi (UI) oluşturun.

> [!NOTE]
> Her iki yöntem görece yavaş ve pahalı olur. Tutumlu kullanmak için önerilir.

## <a name="getusertranslationcounts-method"></a>GetUserTranslationCounts yöntemi

Bu yöntem, kullanıcı tarafından oluşturulmuş çevirileri sayısını alır. Çeviri sayıları kullanıcı, minRating ve maxRating İstek parametreleri için uriPrefix göre gruplandırılmış listesini sağlar.

**Sözdizimi**

> [!div class="tabbedCodeSnippets"]
```cs
UserTranslationCount[]GetUserTranslationCounts(
           string appId,
           string uriPrefix,
           string from,
           string to,
           int? minRating,
           int? maxRating,
           string user, 
           string category
           DateTime? minDateUtc,
           DateTime? maxDateUtc,
           int? skip,
           int? take);
```

**Parametreler**

| Parametre | Açıklama |
|:---|:---|
| AppID | **Gerekli** Authorization Üstbilgisi kullanılırsa, AppID alanı boş bırakın başka belirtin "Bearer" içeren bir dize + "" + erişim belirteci.|
| uriPrefix | **İsteğe bağlı** çeviri URI öneki içeren bir dize.|
| başlangıç | **İsteğe bağlı** çeviri metnin dil kodunu temsil eden dize. |
| - | **İsteğe bağlı** metne çevirmek için dil kodu temsil eden dize.|
| minRating| **İsteğe bağlı** çevrilmiş metni için en düşük Kalite Derecelendirme temsil eden bir tamsayı değeri. Geçerli -10 ile 10 arasında bir değerdir. Varsayılan değer 1'dir.|
| maxRating| **İsteğe bağlı** çevrilmiş metni için en iyi kalite derecelendirme temsil eden bir tamsayı değeri. Geçerli -10 ile 10 arasında bir değerdir. Varsayılan değer 1'dir.|
| kullanıcı | **İsteğe bağlı** sonucu filtre uygulamak için kullanılan bir dize tabanlı gönderenin gönderme. |
| category| **İsteğe bağlı** kategori veya çeviri etki alanı içeren bir dize. Bu parametre yalnızca genel varsayılan seçeneği destekler.|
| minDateUtc| **İsteğe bağlı** çevirileri almak istediğiniz zaman başlangıç tarihi. Tarihi UTC biçiminde olmalıdır. |
| maxDateUtc| **İsteğe bağlı** çevirileri almak istediğiniz zaman kasa tarih. Tarihi UTC biçiminde olmalıdır. |
| Atla| **İsteğe bağlı** bir sayfada atlamak istediğiniz sonuç sayısı. Örneğin, sonuçları ve 21 sonuç kaydının görünümünden ilk 20 satırlarını Atla istiyorsanız, bu parametre için 20 belirtin. Bu parametre için varsayılan değer 0'dır.|
| Al | **İsteğe bağlı** almak istediğiniz sonuç sayısı. Her istek en fazla 100'dür. Varsayılan değer 100'dür.|

> [!NOTE]
> Atlama ve alma İstek parametreleri sayfalandırma çok sayıda sonuç kayıt için etkinleştirin.

**Dönüş değeri**

Dizi sonuç kümesini içerir **UserTranslationCount**. Her UserTranslationCount aşağıdaki öğeleri içerir:

| Alan | Açıklama |
|:---|:---|
| Sayı| Alınan sonuç sayısı|
| Kimden | Kaynak dili|
| Derecelendirme| AddTranslation() yöntemi çağrısında gönderen tarafından uygulanan derecelendirme|
| Alıcı| Hedef Dil|
| Uri| AddTranslation() yöntem çağrısında uygulanan URI|
| Kullanıcı| Kullanıcı adı|

**Özel durumlar**

| Özel durum | İleti | Koşullar |
|:---|:---|:---|
| ArgumentOutOfRangeException | Parametre '**maxDateUtc**'değerinden büyük veya eşit olmalıdır'**minDateUtc**'.| Parametresinin değeri **maxDateUtc** parametresinin değerinden daha düşük olan **minDateUtc**.|
| TranslateApiException | IP kota değil.| <ul><li>Dakika başına istek sayısı için üst sınıra ulaşıldı.</li><li>İstek boyutu 10000 karakter sınırlı kalır.</li><li>Bir saatlik ve günlük kotası Microsoft Çeviricisi API kabul edeceği karakter sayısını sınırlandırın.</li></ul>|
| TranslateApiException | AppID kota ' dir.| Uygulama Kimliği günlük veya saatlik kotasını aştı.|

> [!NOTE]
> Kota, hizmetin tüm kullanıcılarının arasında eşitliği emin olmak için ayarlanır.

**GitHib üzerinde kod örnekleri görüntüle**
* [C#](https://github.com/MicrosoftTranslator/Documentation-Code-TextAPI/blob/master/ctf/ctf-getusertranslationcounts-example-csharp.md)
* [PHP](https://github.com/MicrosoftTranslator/Documentation-Code-TextAPI/blob/master/ctf/ctf-getusertranslationcounts-example-php.md)

## <a name="getusertranslations-method"></a>GetUserTranslations yöntemi

Bu yöntem, kullanıcı tarafından oluşturulmuş çevirileri alır. Bu çevirileri uriPrefix tarafından, kullanıcı ve minRating maxRating istek parametrelerini gruplandırılmış sağlar.

**Sözdizimi**

> [!div class="tabbedCodeSnippets"]
```cs
UserTranslation[] GetUserTranslations (
            string appId,
            string uriPrefix,
            string from,
            string to,
            int? minRating,
            int? maxRating,
            string user, 
            string category
            DateTime? minDateUtc,
            DateTime? maxDateUtc,
            int? skip,
            int? take); 
```

**Parametreler**

| Parametre | Açıklama |
|:---|:---|
| AppID | **Gerekli** Authorization Üstbilgisi kullanılırsa, AppID alanı boş bırakın başka belirtin "Bearer" içeren bir dize + "" + erişim belirteci.|
| uriPrefix| **İsteğe bağlı** çeviri URI öneki içeren bir dize.|
| başlangıç| **İsteğe bağlı** çeviri metnin dil kodunu temsil eden dize.|
| -| **İsteğe bağlı** metne çevirmek için dil kodu temsil eden dize.|
| minRating| **İsteğe bağlı** çevrilmiş metni için en düşük Kalite Derecelendirme temsil eden bir tamsayı değeri. Geçerli -10 ile 10 arasında bir değerdir. Varsayılan değer 1'dir.|
| maxRating| **İsteğe bağlı** çevrilmiş metni için en iyi kalite derecelendirme temsil eden bir tamsayı değeri. Geçerli -10 ile 10 arasında bir değerdir. Varsayılan değer 1'dir.|
| kullanıcı| **İsteğe bağlı. Gönderim originator üzerinde temel sonucu filtre uygulamak için kullanılan bir dize**|
| category| **İsteğe bağlı** kategori veya çeviri etki alanı içeren bir dize. Bu parametre yalnızca genel varsayılan seçeneği destekler.| 
| minDateUtc| **İsteğe bağlı** çevirileri almak istediğiniz zaman başlangıç tarihi. Tarihi UTC biçiminde olmalıdır.| 
| maxDateUtc| **İsteğe bağlı** çevirileri almak istediğiniz zaman kasa tarih. Tarihi UTC biçiminde olmalıdır.|
| Atla| **İsteğe bağlı** bir sayfada atlamak istediğiniz sonuç sayısı. Örneğin, sonuçları ve 21 sonuç kaydının görünümünden ilk 20 satırlarını Atla istiyorsanız, bu parametre için 20 belirtin. Bu parametre için varsayılan değer 0'dır.|
| Al| **İsteğe bağlı** almak istediğiniz sonuç sayısı. Her istek en fazla 100'dür. Varsayılan değer 50'dir.|

> [!NOTE]
> Atlama ve alma İstek parametreleri sayfalandırma çok sayıda sonuç kayıt için etkinleştirin.

**Dönüş değeri**

Dizi sonuç kümesini içerir **UserTranslation**. Her UserTranslation aşağıdaki öğeleri içerir:

| Alan | Açıklama |
|:---|:---|
| CreatedDateUtc| AddTranslation() kullanarak girişini oluşturulma tarihi|
| Kimden| Kaynak dili|
| OriginalText| İstek gönderirken kullanılan kaynak dili metin|
|Derecelendirme |AddTranslation() yöntemi çağrısında gönderen tarafından uygulanan derecelendirme|
|Alıcı|    Hedef Dil|
|TranslatedText|    AddTranslation() yöntem çağrısında gönderildiği haliyle çevirisi|
|Uri|   AddTranslation() yöntem çağrısında uygulanan URI|
|Kullanıcı   |Kullanıcı adı|

**Özel durumlar**

| Özel durum | İleti | Koşullar |
|:---|:---|:---|
| ArgumentOutOfRangeException | Parametre '**maxDateUtc**'değerinden büyük veya eşit olmalıdır'**minDateUtc**'.| Parametresinin değeri **maxDateUtc** parametresinin değerinden daha düşük olan **minDateUtc**.|
| TranslateApiException | IP kota değil.| <ul><li>Dakika başına istek sayısı için üst sınıra ulaşıldı.</li><li>İstek boyutu 10000 karakter sınırlı kalır.</li><li>Bir saatlik ve günlük kotası Microsoft Çeviricisi API kabul edeceği karakter sayısını sınırlandırın.</li></ul>|
| TranslateApiException | AppID kota ' dir.| Uygulama Kimliği günlük veya saatlik kotasını aştı.|

> [!NOTE]
> Kota, hizmetin tüm kullanıcılarının arasında eşitliği emin olmak için ayarlanır.

**GitHib üzerinde kod örnekleri görüntüle**
* [C#](https://github.com/MicrosoftTranslator/Documentation-Code-TextAPI/blob/master/ctf/ctf-getusertranslations-example-csharp.md)
* [PHP](https://github.com/MicrosoftTranslator/Documentation-Code-TextAPI/blob/master/ctf/ctf-getusertranslations-example-php.md)


















