---
title: İşbirliğine dayalı Translation Framework (CTF) raporlama - Translator metin çevirisi API'si
titlesuffix: Azure Cognitive Services
description: İşbirliğine dayalı Translation Framework (CTF) raporlama kullanma
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 12/14/2017
ms.author: swmachan
ms.openlocfilehash: 79a645b0b41f200c384c165f244efa679be65171
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67443454"
---
# <a name="how-to-use-collaborative-translation-framework-ctf-reporting"></a>Collaborative Translation Framework (CTF) raporlamasını kullanma

> [!NOTE]
> Bu metot kullanımdan kaldırılmıştır. Translator metin çevirisi API'si, V3.0 içinde kullanılabilir değil.
> 
> İşbirliğine dayalı çevirileri Framework (CTF), daha önce Translator Text API, V2.0 için kullanılabilir, 1 Şubat 2018'den itibaren kullanım dışı bırakıldı. AddTranslation ve AddTranslationArray işlevleri izin düzeltmeleri işbirliğine dayalı çeviri çerçevesi aracılığıyla etkinleştirin. 31 Ocak 2018'den sonra bu iki işlevler yeni cümle gönderimler kabul etmedi ve kullanıcıların bir hata iletisi alırsınız. Bu işlevler, kullanımdan kaldırılmıştır ve değiştirilmeyecek.

Raporlama işbirliğine dayalı Translation Framework (CTF) API istatistikleri ve gerçek içerik CTF deposunda döndürür. Bu API GetTranslations() yönteminden farklıdır çünkü bu:
* Çevrilen içeriğin ve alt toplam sayısı yalnızca hesabınızdan (AppID veya Azure Market hesabı) döndürür.
* Bir eşleşme kaynak cümlenin gerek kalmadan çevrilen içeriğin ve alt toplam sayı bu sayıyı döndürür.
* Bir otomatik çeviri (makine çevirisi) döndürmez.

## <a name="endpoint"></a>Uç Nokta
Raporlama CTF API uç noktası https://api.microsofttranslator.com/v2/beta/ctfreporting.svc


## <a name="methods"></a>Yöntemler
| Ad |    Açıklama|
|:---|:---|
| GetUserTranslationCounts Method | Kullanıcı tarafından oluşturulan çevirileri sayısını alın. |
| GetUserTranslations Method | Kullanıcı tarafından oluşturulan çevirileri alır. |

Bu yöntemler sağlar:
* Kullanıcı çevirileri ve düzeltmeleri indirmek için hesap kimliği altında tam kümesini alır.
* Sık katkıda bulunanlar listesini edinin. Doğru kullanıcı adını AddTranslation() sağlandığından emin olun.
* Bir bölümü, sitenizin URI ön ekini temel alarak kısıtlı sayesinde güvenilen kullanıcılarınız gerekiyorsa, kullanılabilir tüm aday görmek bir kullanıcı arabirimi (UI) oluşturun.

> [!NOTE]
> Her iki yöntem görece yavaş ve pahalı. Tutumlu kullanmak için önerilir.

## <a name="getusertranslationcounts-method"></a>GetUserTranslationCounts method

Bu yöntem, kullanıcı tarafından oluşturulan çevirileri sayısını alır. Bu çeviri sayıları kullanıcı, minRating ve maxRating İstek parametreleri için uriPrefix göre gruplandırılmış listesini sağlar.

**Söz dizimi**

> [!div class="tabbedCodeSnippets"]
> ```cs
> UserTranslationCount[]GetUserTranslationCounts(
>            string appId,
>            string uriPrefix,
>            string from,
>            string to,
>            int? minRating,
>            int? maxRating,
>            string user,
>            string category
>            DateTime? minDateUtc,
>            DateTime? maxDateUtc,
>            int? skip,
>            int? take);
> ```

**Parametreler**

| Parametre | Açıklama |
|:---|:---|
| appId | **Gerekli** yetkilendirme üst bilgisi kullandıysanız, AppID alanı boş bırakın başka belirtin "Bearer" içeren bir dize + "" + erişim belirteci.|
| uriPrefix | **İsteğe bağlı** çeviri URI öneki içeren bir dize.|
| from | **İsteğe bağlı** çeviri metnin dil kodu temsil eden bir dize. |
| - | **İsteğe bağlı** metne çevirmek için dil kodunu temsil eden bir dize.|
| minRating| **İsteğe bağlı** çevrilmiş metin için en düşük kalite sıralaması temsil eden bir tamsayı değeri. Geçerli -10 ile 10 arasında bir değerdir. Varsayılan değer 1’dir.|
| maxRating| **İsteğe bağlı** çevrilmiş metin için en yüksek kalite sıralaması temsil eden bir tamsayı değeri. Geçerli -10 ile 10 arasında bir değerdir. Varsayılan değer 1’dir.|
| kullanıcı | **İsteğe bağlı** sonucu filtrelemek için kullanılan bir dize tabanlı gönderene gönderim, üzerinde. |
| category| **İsteğe bağlı** kategori veya çeviri etki alanı içeren bir dize. Bu parametre yalnızca genel varsayılan seçeneği destekler.|
| minDateUtc| **İsteğe bağlı** zaman çevirileri almak istediğiniz başlangıç tarihi. Tarihi UTC biçiminde olması gerekir. |
| maxDateUtc| **İsteğe bağlı** çevirileri almak istediğiniz zaman kasa tarih. Tarihi UTC biçiminde olması gerekir. |
| Atla| **İsteğe bağlı** bir sayfada atlamak istediğiniz sonuç sayısı. Örneğin, sonuçları ve 21 sonuç kaydının görünümünden ilk 20 satırları atla istiyorsanız, bu parametre için 20 belirtin. Bu parametre için varsayılan değer 0'dır.|
| sınav zamanı | **İsteğe bağlı** almak istediğiniz sonuç sayısı. Her istek sayısının 100'dür. Varsayılan değer 100'dür.|

> [!NOTE]
> Skip ve take İstek parametreleri, çok sayıda sonuç kayıtları için sayfalandırmayı etkinleştirin.

**Dönüş değeri**

Dizi sonuç kümesini içeren **UserTranslationCount**. Her UserTranslationCount aşağıdaki öğeleri içerir:

| Alan | Açıklama |
|:---|:---|
| Count| Alınan sonuç sayısı|
| Başlangıç | Kaynak dili|
| Derecelendirme| Gönderen AddTranslation() yöntemi çağrısında tarafından uygulanan derecelendirme|
| Bitiş| Hedef Dil|
| URI| AddTranslation() yöntem çağrısında uygulanan URI'si|
| Kullanıcı| Kullanıcı adı|

**Özel durumlar**

| Özel durum | `Message` | Koşullar |
|:---|:---|:---|
| Üretiliyor | Parametre '**maxDateUtc**'değerinden büyük veya buna eşit olmalıdır'**minDateUtc**'.| Parametresinin değeri **maxDateUtc** parametresi değerinden daha küçük **minDateUtc**.|
| TranslateApiException | IP kota gerçekleştirilir.| <ul><li>Dakika başına istek sayısı sınırına ulaşıldı.</li><li>İstek boyutunun 10000 karakter sınırlı kalır.</li><li>Bir saatlik ve günlük kota Microsoft Translator API'si kabul karakter sayısını sınırlayın.</li></ul>|
| TranslateApiException | AppID kota ' dir.| Uygulama kimliği, günlük veya saatlik kota aşıldı.|

> [!NOTE]
> Hizmetin tüm kullanıcılar arasındaki eşitliği emin olmak için kota ayarlar.

**GitHib üzerinde kod örnekleri görüntüle**
* [C#](https://github.com/MicrosoftTranslator/Documentation-Code-TextAPI/blob/master/ctf/ctf-getusertranslationcounts-example-csharp.md)
* [PHP](https://github.com/MicrosoftTranslator/Documentation-Code-TextAPI/blob/master/ctf/ctf-getusertranslationcounts-example-php.md)

## <a name="getusertranslations-method"></a>GetUserTranslations yöntemi

Bu yöntem, kullanıcı tarafından oluşturulan çevirileri alır. Çevirileri uriPrefix tarafından, kullanıcı, minRating ve maxRating istek parametrelerini gruplandırılmış sağlar.

**Söz dizimi**

> [!div class="tabbedCodeSnippets"]
> ```cs
> UserTranslation[] GetUserTranslations (
>             string appId,
>             string uriPrefix,
>             string from,
>             string to,
>             int? minRating,
>             int? maxRating,
>             string user,
>             string category
>             DateTime? minDateUtc,
>             DateTime? maxDateUtc,
>             int? skip,
>             int? take);
> ```

**Parametreler**

| Parametre | Açıklama |
|:---|:---|
| appId | **Gerekli** yetkilendirme üst bilgisi kullandıysanız, AppID alanı boş bırakın başka belirtin "Bearer" içeren bir dize + "" + erişim belirteci.|
| uriPrefix| **İsteğe bağlı** çeviri URI öneki içeren bir dize.|
| from| **İsteğe bağlı** çeviri metnin dil kodu temsil eden bir dize.|
| -| **İsteğe bağlı** metne çevirmek için dil kodunu temsil eden bir dize.|
| minRating| **İsteğe bağlı** çevrilmiş metin için en düşük kalite sıralaması temsil eden bir tamsayı değeri. Geçerli -10 ile 10 arasında bir değerdir. Varsayılan değer 1’dir.|
| maxRating| **İsteğe bağlı** çevrilmiş metin için en yüksek kalite sıralaması temsil eden bir tamsayı değeri. Geçerli -10 ile 10 arasında bir değerdir. Varsayılan değer 1’dir.|
| kullanıcı| **İsteğe bağlı. Gönderim oluşturanla üzerinde sonucu filtrelemek için kullanılan bir dize**|
| category| **İsteğe bağlı** kategori veya çeviri etki alanı içeren bir dize. Bu parametre yalnızca genel varsayılan seçeneği destekler.|
| minDateUtc| **İsteğe bağlı** zaman çevirileri almak istediğiniz başlangıç tarihi. Tarihi UTC biçiminde olması gerekir.|
| maxDateUtc| **İsteğe bağlı** çevirileri almak istediğiniz zaman kasa tarih. Tarihi UTC biçiminde olması gerekir.|
| Atla| **İsteğe bağlı** bir sayfada atlamak istediğiniz sonuç sayısı. Örneğin, sonuçları ve 21 sonuç kaydının görünümünden ilk 20 satırları atla istiyorsanız, bu parametre için 20 belirtin. Bu parametre için varsayılan değer 0'dır.|
| sınav zamanı| **İsteğe bağlı** almak istediğiniz sonuç sayısı. Her istek sayısının 100'dür. Varsayılan değer 50'dir.|

> [!NOTE]
> Skip ve take İstek parametreleri, çok sayıda sonuç kayıtları için sayfalandırmayı etkinleştirin.

**Dönüş değeri**

Dizi sonuç kümesini içeren **UserTranslation**. Her UserTranslation aşağıdaki öğeleri içerir:

| Alan | Açıklama |
|:---|:---|
| CreatedDateUtc| Oluşturulma tarihi AddTranslation() kullanarak giriş|
| Başlangıç| Kaynak dili|
| originalText| İstek gönderirken kullanılan kaynak dili metni|
|Derecelendirme |Gönderen AddTranslation() yöntemi çağrısında tarafından uygulanan derecelendirme|
|Bitiş|    Hedef Dil|
|TranslatedText|    AddTranslation() yöntem çağrısında gönderilen çeviri|
|URI|   AddTranslation() yöntem çağrısında uygulanan URI'si|
|Kullanıcı   |Kullanıcı adı|

**Özel durumlar**

| Özel durum | `Message` | Koşullar |
|:---|:---|:---|
| Üretiliyor | Parametre '**maxDateUtc**'değerinden büyük veya buna eşit olmalıdır'**minDateUtc**'.| Parametresinin değeri **maxDateUtc** parametresi değerinden daha küçük **minDateUtc**.|
| TranslateApiException | IP kota gerçekleştirilir.| <ul><li>Dakika başına istek sayısı sınırına ulaşıldı.</li><li>İstek boyutunun 10000 karakter sınırlı kalır.</li><li>Bir saatlik ve günlük kota Microsoft Translator API'si kabul karakter sayısını sınırlayın.</li></ul>|
| TranslateApiException | AppID kota ' dir.| Uygulama kimliği, günlük veya saatlik kota aşıldı.|

> [!NOTE]
> Hizmetin tüm kullanıcılar arasındaki eşitliği emin olmak için kota ayarlar.

**GitHib üzerinde kod örnekleri görüntüle**
* [C#](https://github.com/MicrosoftTranslator/Documentation-Code-TextAPI/blob/master/ctf/ctf-getusertranslations-example-csharp.md)
* [PHP](https://github.com/MicrosoftTranslator/Documentation-Code-TextAPI/blob/master/ctf/ctf-getusertranslations-example-php.md)
