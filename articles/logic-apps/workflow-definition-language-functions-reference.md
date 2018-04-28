---
title: İş akışı tanımlama dili işlevleri - Azure Logic Apps | Microsoft Docs
description: Mantıksal uygulama iş akışı tanımları kullanabileceğiniz işlevleri hakkında bilgi edinin
services: logic-apps
author: ecfan
manager: SyntaxC4
editor: ''
documentationcenter: ''
ms.assetid: ''
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 04/25/2018
ms.author: estfan; LADocs
ms.openlocfilehash: 0155e35641a0407fe48c4da07400fa188152b0af
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="workflow-definition-language-functions-reference-for-azure-logic-apps"></a>Azure mantıksal uygulamaları için iş akışı tanımlama dili işlevleri başvurusu

Bu makalede iş akışlarıyla oluştururken kullanabileceğiniz işlevleri [Azure Logic Apps](../logic-apps/logic-apps-overview.md). Mantıksal uygulama tanımları hakkında daha fazla bilgi için bkz: [Azure mantıksal uygulamaları için iş akışı tanımlama dili](../logic-apps/logic-apps-workflow-definition-language.md). 

> [!NOTE]
> Parametre tanımları sözdizimi soru bir parametre parametre anlamına gelir sonra görüntülenen işareti (?) isteğe bağlıdır. Örneğin, [getFutureTime()](#getFutureTime).

<a name="action"></a>

## <a name="action"></a>action

Dönüş *geçerli* animasyonun eylem çıktı çalışma zamanı ya da bir ifadeyi atayabilirsiniz diğer JSON ad ve değer çiftleri değerleri. Varsayılan olarak, bu işlev tüm eylem nesnesine başvuruyor, ancak özellik değerini istediğiniz isteğe bağlı olarak belirtebilirsiniz. Ayrıca bkz. [actions()](../logic-apps/workflow-definition-language-functions-reference.md#actions).

Kullanabileceğiniz `action()` işlevi yalnızca bu yerlerde: 

* `unsubscribe` Özelliği özgün sonucu erişebilmesi için bir Web kancası eylemi için `subscribe` isteği
* `trackedProperties` Özelliği için bir eylem
* `do-until` Döngü bir eylem koşulunu

```
action()
action().outputs.body.<property> 
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Özelliği*> | Hayır | Dize | Değer istediğiniz eylem nesnenin özelliği adı: **adı**, **startTime**, **endTime**, **girişleri**,  **çıkarır**, **durum**, **kod**, **Trackingıd**, ve **clientTrackingId**. Azure portalında bir belirli çalıştırma geçmişi ait ayrıntıları gözden geçirerek bu özellikleri bulabilirsiniz. Daha fazla bilgi için bkz: [REST API - iş akışı çalıştırmak eylemleri](https://docs.microsoft.com/rest/api/logic/workflowrunactions/get). | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | -----| ----------- | 
| <*Eylem çıkışı*> | Dize | Geçerli eylem veya özellik çıktısı | 
|||| 

<a name="actionBody"></a>

## <a name="actionbody"></a>actionBody

Bir eylemin dönüş `body` çalışma zamanında çıktı. İçin toplu özelliktir `actions('<actionName>').outputs.body`. Bkz: [body()](#body) ve [actions()](#actions).

```
actionBody('<actionName>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*EylemAdı*> | Evet | Dize | Eylemin adını `body` istediğiniz çıktı | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | -----| ----------- | 
| <*Eylem gövde çıkışı*> | Dize | `body` Belirtilen eylem çıktı | 
|||| 

*Örnek*

Bu örnek alır `body` Twitter eyleminden çıktı `Get user`: 

```
actionBody('Get_user')
```

Ve bu sonucu döndürür:

```json
"body": {
  "FullName": "Contoso Corporation",
  "Location": "Generic Town, USA",
  "Id": 283541717,
  "UserName": "ContosoInc",
  "FollowersCount": 172,
  "Description": "Leading the way in transforming the digital workplace.",
  "StatusesCount": 93,
  "FriendsCount": 126,
  "FavouritesCount": 46,
  "ProfileImageUrl": "https://pbs.twimg.com/profile_images/908820389907722240/gG9zaHcd_400x400.jpg"
}
```

<a name="actionOutputs"></a>

## <a name="actionoutputs"></a>actionOutputs

Çalışma zamanında bir eylemin çıkış döndür. İçin toplu özelliktir `actions('<actionName>').outputs`. Bkz: [actions()](#actions).

```
actionOutputs('<actionName>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*EylemAdı*> | Evet | Dize | İstediğiniz eylem için ad çıktı | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | -----| ----------- | 
| <*Çıktı*> | Dize | Belirtilen eylem çıkışı | 
|||| 

*Örnek*

Bu örnek Twitter eylemden çıktısını alır `Get user`: 

```
actionOutputs('Get_user')
```

Ve bu sonucu döndürür:

```json
{ 
  "statusCode": 200,
  "headers": {
    "Pragma": "no-cache",
    "Vary": "Accept-Encoding",
    "x-ms-request-id": "a916ec8f52211265d98159adde2efe0b",
    "X-Content-Type-Options": "nosniff",
    "Timing-Allow-Origin": "*",
    "Cache-Control": "no-cache",
    "Date": "Mon, 09 Apr 2018 18:47:12 GMT",
    "Set-Cookie": "ARRAffinity=b9400932367ab5e3b6802e3d6158afffb12fcde8666715f5a5fbd4142d0f0b7d;Path=/;HttpOnly;Domain=twitter-wus.azconn-wus.p.azurewebsites.net",
    "X-AspNet-Version": "4.0.30319",
    "X-Powered-By": "ASP.NET",
    "Content-Type": "application/json; charset=utf-8",
    "Expires": "-1",
    "Content-Length": "339"
  },
  "body": {
    "FullName": "Contoso Corporation",
    "Location": "Generic Town, USA",
    "Id": 283541717,
    "UserName": "ContosoInc",
    "FollowersCount": 172,
    "Description": "Leading the way in transforming the digital workplace.",
    "StatusesCount": 93,
    "FriendsCount": 126,
    "FavouritesCount": 46,
    "ProfileImageUrl": "https://pbs.twimg.com/profile_images/908820389907722240/gG9zaHcd_400x400.jpg"
  }
}
```

<a name="actions"></a>

## <a name="actions"></a>Eylemler

Bir eylemin çıkış çalışma zamanı veya değerleri bir ifadeyi atayabilirsiniz diğer JSON ad ve değer çiftleri döndürmek. Varsayılan olarak, tüm eylem nesnesi işlevi başvuruyor, ancak isteğe bağlı olarak bir özellik belirtebilirsiniz istediğiniz değeri. Toplu sürümler için bkz: [actionBody()](#actionBody), [actionOutputs()](#actionOutputs), ve [body()](#body). Geçerli eylem için bkz: [action()](#action).

> [!NOTE] 
> Daha önce kullanabileceğinizi `actions()` işlevi veya `conditions` bir eylem çıktı başka bir eylem tabanlı çalıştırıldığını belirtirken öğesi. Ancak, açıkça eylemleri arasındaki bağımlılıkları bildirmek için şimdi bağımlı eylemin kullanmalısınız `runAfter` özelliği. Daha fazla bilgi edinmek için `runAfter` özelliği, bakın [yakalamak ve işleme hataları runAfter özelliğiyle](../logic-apps/logic-apps-workflow-definition-language.md).

```
actions('<actionName>')
actions('<actionName>').outputs.body.<property> 
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*EylemAdı*> | Evet | Dize | Çıktı istediğiniz eylem nesnesi için ad  | 
| <*Özelliği*> | Hayır | Dize | Değer istediğiniz eylem nesnenin özelliği adı: **adı**, **startTime**, **endTime**, **girişleri**,  **çıkarır**, **durum**, **kod**, **Trackingıd**, ve **clientTrackingId**. Azure portalında bir belirli çalıştırma geçmişi ait ayrıntıları gözden geçirerek bu özellikleri bulabilirsiniz. Daha fazla bilgi için bkz: [REST API - iş akışı çalıştırmak eylemleri](https://docs.microsoft.com/rest/api/logic/workflowrunactions/get). | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | -----| ----------- | 
| <*Eylem çıkışı*> | Dize | Belirtilen eylem veya özellik çıktısı | 
|||| 

*Örnek*

Bu örnek alır `status` Twitter eylemden özellik değeri `Get user` çalışma zamanında: 

```
actions('Get_user').outputs.body.status 
```

Ve bu sonucu döndürür: `"Succeeded"`

<a name="add"></a>

## <a name="add"></a>ekle

İki sayı ekleme sonucu döndürür.

```
add(<summand_1>, <summand_2>)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*summand_1*>, <*summand_2*> | Evet | Tamsayı, kayan nokta, veya karma | Sayıları eklemek için | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | -----| ----------- | 
| <*Sonuç Topla*> | Tamsayı veya kayan nokta | Belirtilen sayı eklemeden sonucu | 
|||| 

*Örnek*

Bu örnek belirtilen sayıları toplar:

```
add(1, 1.5)
```

Ve bu sonucu döndürür: `2.5`

<a name="addDays"></a>

## <a name="adddays"></a>addDays

Bir gün sayısı için bir zaman damgası ekleyin.

```
addDays('<timestamp>', <days>, '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*zaman damgası*> | Evet | Dize | Zaman damgası içeren dize | 
| <*gün*> | Evet | Tamsayı | Pozitif veya negatif bir sayı eklemek için günlük | 
| <*Biçimi*> | Hayır | Dize | Ya da bir [tek biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim deseni](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Zaman damgası için varsayılan biçimi olan ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-aa-ddT:mm:ss:fffffffK), ile uyumlu [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ve saat dilimi bilgilerini korur. |
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*güncelleştirilmiş zaman damgası*> | Dize | Zaman damgası artı belirtilen gün sayısı  | 
|||| 

*Örnek 1*

Bu örnek, belirtilen zaman damgası 10 gün ekler:

```
addDays('2018-03-15T13:00:00Z', 10)
```

Ve bu sonucu döndürür: `"2018-03-25T00:00:0000000Z"`

*Örnek 2*

Bu örnek, belirtilen zaman damgası beş gün çıkarır:

```
addDays('2018-03-15T00:00:00Z', -5)
```

Ve bu sonucu döndürür: `"2018-03-10T00:00:0000000Z"`

<a name="addHours"></a>

## <a name="addhours"></a>addHours

Saat sayısı için bir zaman damgası ekleyin.

```
addHours('<timestamp>', <hours>, '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*zaman damgası*> | Evet | Dize | Zaman damgası içeren dize | 
| <*Saatleri*> | Evet | Tamsayı | Pozitif veya negatif bir sayı saat eklemek için | 
| <*Biçimi*> | Hayır | Dize | Ya da bir [tek biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim deseni](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Zaman damgası için varsayılan biçimi olan ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-aa-ddT:mm:ss:fffffffK), ile uyumlu [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ve saat dilimi bilgilerini korur. |
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*güncelleştirilmiş zaman damgası*> | Dize | Zaman damgası artı belirtilen sayıda saat  | 
|||| 

*Örnek 1*

Bu örnek, 10 saat için belirtilen zaman damgası ekler:

```
addHours('2018-03-15T00:00:00Z', 10)
```

Ve bu sonucu döndürür: `"2018-03-15T10:00:0000000Z"`

*Örnek 2*

Bu örnek, belirtilen zaman damgası beş saat çıkarır:

```
addHours('2018-03-15T15:00:00Z', -5)
```

Ve bu sonucu döndürür: `"2018-03-15T10:00:0000000Z"`

<a name="addMinutes"></a>

## <a name="addminutes"></a>addMinutes

Dakika sayısı için bir zaman damgası ekleyin.

```
addMinutes('<timestamp>', <minutes>, '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*zaman damgası*> | Evet | Dize | Zaman damgası içeren dize | 
| <*dakika*> | Evet | Tamsayı | Pozitif veya negatif bir sayı eklemek için dakika | 
| <*Biçimi*> | Hayır | Dize | Ya da bir [tek biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim deseni](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Zaman damgası için varsayılan biçimi olan ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-aa-ddT:mm:ss:fffffffK), ile uyumlu [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ve saat dilimi bilgilerini korur. |
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*güncelleştirilmiş zaman damgası*> | Dize | Zaman damgası artı belirtilen dakika sayısı | 
|||| 

*Örnek 1*

Bu örnek, belirtilen zaman damgası için 10 dakika ekler:

```
addMinutes('2018-03-15T00:10:00Z', 10)
```

Ve bu sonucu döndürür: `"2018-03-15T00:20:00.0000000Z"`

*Örnek 2*

Bu örnek, belirtilen zaman damgası beş dakikadan çıkarır:

```
addMinutes('2018-03-15T00:20:00Z', -5)
```

Ve bu sonucu döndürür: `"2018-03-15T00:15:00.0000000Z"`

<a name="addProperty"></a>

## <a name="addproperty"></a>addProperty

Bir özellik ve onun değeri veya ad-değer çiftinin bir JSON nesnesi ekleyin ve güncelleştirilmiş nesneyi döndürür. Çalışma zamanında nesne zaten varsa, işlev bir hata oluşturur.

```
addProperty(<object>, '<property>', <value>)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*object*> | Evet | Nesne | Bir özellik eklemek istediğiniz JSON nesnesi | 
| <*Özelliği*> | Evet | Dize | Özellik eklemek için ad | 
| <*Değer*> | Evet | Herhangi biri | Özelliği için değer |
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*güncelleştirilmiş nesnesi*> | Nesne | Belirtilen özellik ile güncelleştirilmiş JSON nesnesi | 
|||| 

*Örnek*

Bu örnek, `accountNumber` özelliğine `customerProfile` JSON ile dönüştürülür nesne [JSON()](#json) işlevi. İşlev tarafından oluşturulan bir değer atar [guid()](#guid) işlev ve güncelleştirilmiş nesneyi döndürür:

```
addProperty(json('customerProfile'), 'accountNumber', guid())
```

<a name="addSeconds"></a>

## <a name="addseconds"></a>saniyeEkle

Saniye sayısı için bir zaman damgası ekleyin.

```
addSeconds('<timestamp>', <seconds>, '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*zaman damgası*> | Evet | Dize | Zaman damgası içeren dize | 
| <*Saniye*> | Evet | Tamsayı | Olumlu veya olumsuz saniyeyi eklemek için | 
| <*Biçimi*> | Hayır | Dize | Ya da bir [tek biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim deseni](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Zaman damgası için varsayılan biçimi olan ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-aa-ddT:mm:ss:fffffffK), ile uyumlu [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ve saat dilimi bilgilerini korur. |
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*güncelleştirilmiş zaman damgası*> | Dize | Zaman damgası artı belirtilen saniye sayısı  | 
|||| 

*Örnek 1*

Bu örnek, belirtilen zaman damgası için 10 saniye ekler:

```
addSeconds('2018-03-15T00:00:00Z', 10)
```

Ve bu sonucu döndürür: `"2018-03-15T00:00:10.0000000Z"`

*Örnek 2*

Bu örnek, belirtilen zaman damgası için beş saniyede çıkarır:

```
addSeconds('2018-03-15T00:00:30Z', -5)
```

Ve bu sonucu döndürür: `"2018-03-15T00:00:25.0000000Z"`

<a name="addToTime"></a>

## <a name="addtotime"></a>addToTime

Zaman birimi sayısı için bir zaman damgası ekleyin. Ayrıca bkz. [getFutureTime()](#getFutureTime).

```
addToTime('<timestamp>', <interval>, '<timeUnit>', '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*zaman damgası*> | Evet | Dize | Zaman damgası içeren dize | 
| <*aralığı*> | Evet | Tamsayı | Eklenecek belirtilen süre birimlerinin sayısı | 
| <*timeUnit*> | Evet | Dize | İle kullanılacak zaman birimini *aralığı*: "Saniye", "Dakika", "Saat", "Gün", "Hafta", "Ay", "Yıl" | 
| <*Biçimi*> | Hayır | Dize | Ya da bir [tek biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim deseni](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Zaman damgası için varsayılan biçimi olan ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-aa-ddT:mm:ss:fffffffK), ile uyumlu [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ve saat dilimi bilgilerini korur. |
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*güncelleştirilmiş zaman damgası*> | Dize | Belirtilen zaman birimleri sayısı artı zaman damgası  | 
|||| 

*Örnek 1*

Bu örnek, bir gün için belirtilen zaman damgası ekler:

```
addToTime('2018-01-01T00:00:00Z', 1, 'Day') 
```

Ve bu sonucu döndürür: `"2018-01-02T00:00:00:0000000Z"`

*Örnek 2*

Bu örnek, bir gün için belirtilen zaman damgası ekler:

```
addToTime('2018-01-01T00:00:00Z', 1, 'Day', 'D')
```

Ve isteğe bağlı "D" biçimini kullanarak sonucunu döndürür: `"Tuesday, January 2, 2018"`

<a name="and"></a>

## <a name="and"></a>ve

Tüm ifadelerin doğru olup olmadığını denetleyin. Tüm ifadeler doğru olduğunda true döndürür veya en az bir ifade yanlış olduğunda false döndürür.

```
and(<expression1>, <expression2>, ...)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*İfade1*>, <*İfade2*>,... | Evet | Boole | Denetlenecek ifadeleri | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | -----| ----------- | 
| TRUE veya false | Boole | Tüm ifadeler doğru olduğunda true döndürür. En az bir ifade yanlış olduğunda false döndürür. | 
|||| 

*Örnek 1*

Bu örnekler belirli Boole değerleri tümü doğru olup olmadığını denetleyin:

```
and(true, true)
and(false, true)
and(false, false)
```

Ve bu sonuçları döndürür:

* İlk örnek: her iki ifade doğruysa, bu nedenle döndürür `true`. 
* İkinci örnek: bir ifade yanlış olduğunda, bu nedenle döndürür `false`.
* Üçüncü örnek: her iki ifade false, bu nedenle döndürür `false`.

*Örnek 2*

Bu örnekler belirtilen ifadelerinin tümü doğru olup olmadığını denetleyin:

```
and(equals(1, 1), equals(2, 2))
and(equals(1, 1), equals(1, 2))
and(equals(1, 2), equals(1, 3))
```

Ve bu sonuçları döndürür:

* İlk örnek: her iki ifade doğruysa, bu nedenle döndürür `true`. 
* İkinci örnek: bir ifade yanlış olduğunda, bu nedenle döndürür `false`.
* Üçüncü örnek: her iki ifade false, bu nedenle döndürür `false`.

<a name="array"></a>

## <a name="array"></a>array

Belirtilen tek bir giriş için bir dizi döndürür. Birden çok giriş için bkz: [createArray()](#createArray). 

```
array('<value>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Değer*> | Evet | Dize | Dize dizisi oluşturmak için | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| [<*değeri*>] | Dizi | Tek belirtilen giriş içeren bir dizi | 
|||| 

*Örnek*

Bu örnek, bir dizi "hello" dizesi oluşturur:

```
array('hello')
```

Ve bu sonucu döndürür: `["hello"]`

<a name="base64"></a>

## <a name="base64"></a>Base64

Base64 ile kodlanmış sürüm bir dize döndürür.

```
base64('<value>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Değer*> | Evet | Dize | Giriş dizesi | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*Base64 dizesi*> | Dize | Giriş dizesi base64 ile kodlanmış sürümü | 
|||| 

*Örnek*

Bu örnekte "hello" dizesini base64 ile kodlanmış dizeye dönüştürür:

```
base64('hello')
```

Ve bu sonucu döndürür: `"aGVsbG8="`

<a name="base64ToBinary"></a>

## <a name="base64tobinary"></a>base64ToBinary

İkili dosya sürümü base64 ile kodlanmış bir dize döndürür.

```
base64ToBinary('<value>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Değer*> | Evet | Dize | Dönüştürülecek base64 ile kodlanmış dize | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*ikili için base64 dizesi*> | Dize | Base64 kodlu dize ikili sürümü | 
|||| 

*Örnek*

Bu örnek dönüştürür "aGVsbG8 =" base64 ile kodlanmış dize ikili bir dize için:

```
base64ToBinary('aGVsbG8=')
```

Ve bu sonucu döndürür: 

`"0110000101000111010101100111001101100010010001110011100000111101"`

<a name="base64ToString"></a>

## <a name="base64tostring"></a>base64ToString

Etkili bir şekilde base64 dizesi kod çözme base64 ile kodlanmış bir dize, string sürümüne dönün. Bu işlevi kullanmak yerine [decodeBase64()](#decodeBase64). Her iki işlevleri aynı şekilde çalışır ancak `base64ToString()` tercih edilir.

```
base64ToString('<value>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Değer*> | Evet | Dize | Kod çözme için base64 ile kodlanmış dize | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*kodunu çözdü-base64-dizesi*> | Dize | Base64 ile kodlanmış bir dize için dize sürümü | 
|||| 

*Örnek*

Bu örnek dönüştürür "aGVsbG8 =" bir dize base64 ile kodlanmış dizeye:

```
base64ToString('aGVsbG8=')
```

Ve bu sonucu döndürür: `"hello"`

<a name="binary"></a>

## <a name="binary"></a>İkili 

İkili dosya sürümü bir dize döndürür.

```
binary('<value>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Değer*> | Evet | Dize | Dönüştürülecek dizeyi | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*ikili için giriş değeri*> | Dize | Belirtilen dize için ikili dosya sürümü | 
|||| 

*Örnek*

Bu örnekte, "hello" dize ikili bir dizeye dönüştürür:

```
binary('hello')
```

Ve bu sonucu döndürür: 

`"0110100001100101011011000110110001101111"`

<a name="body"></a>

## <a name="body"></a>body

Bir eylemin dönüş `body` çalışma zamanında çıktı. İçin toplu özelliktir `actions('<actionName>').outputs.body`. Bkz: [actionBody()](#actionBody) ve [actions()](#actions).

```
body('<actionName>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*EylemAdı*> | Evet | Dize | Eylemin adını `body` istediğiniz çıktı | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | -----| ----------- | 
| <*Eylem gövde çıkışı*> | Dize | `body` Belirtilen eylem çıktı | 
|||| 

*Örnek*

Bu örnek alır `body` çıktısı `Get user` Twitter eylem: 

```
body('Get_user')
```

Ve bu sonucu döndürür: 

```json
"body": {
    "FullName": "Contoso Corporation",
    "Location": "Generic Town, USA",
    "Id": 283541717,
    "UserName": "ContosoInc",
    "FollowersCount": 172,
    "Description": "Leading the way in transforming the digital workplace.",
    "StatusesCount": 93,
    "FriendsCount": 126,
    "FavouritesCount": 46,
    "ProfileImageUrl": "https://pbs.twimg.com/profile_images/908820389907722240/gG9zaHcd_400x400.jpg"
}
```

<a name="bool"></a>

## <a name="bool"></a>bool

Boole sürümü için bir değer döndürür.

```
bool(<value>)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Değer*> | Evet | Herhangi biri | Dönüştürülecek değer | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| TRUE veya false | Boole | Belirtilen değer için Boolean sürümü | 
|||| 

*Örnek*

Bu örnekler belirtilen değerlerini Boolean değerlerine dönüştürmek: 

```
bool(1)
bool(0)
```

Ve bu sonuçları döndürür: 

* İlk örnek: `true` 
* İkinci örnek: `false`

<a name="coalesce"></a>

## <a name="coalesce"></a>birleşim

İlk değeri null olmayan bir veya daha fazla parametrelerinden döndür. Boş dizeler, boş diziler ve boş nesneleri null değil.

```
coalesce(<object_1>, <object_2>, ...)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*object_1*>, <*object_2*>,... | Evet | Herhangi biri, türleri karışımı | Bir veya daha fazla öğe null için denetleyin | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*ilk olmayan-null-öğesi*> | Herhangi biri | İlk öğeyi veya null olmayan değer. Tüm parametreleri null ise, bu işlev null döndürür. | 
|||| 

*Örnek*

Tüm değerleri null olduğunda bu örnekler ilk değeri null olmayan belirtilen değerleri veya null Döndür:

```
coalesce(null, true, false)
coalesce(null, 'hello', 'world')
coalesce(null, null, null)
```

Ve bu sonuçları döndürür: 

* İlk örnek: `true` 
* İkinci örnek: `"hello"`
* Üçüncü örnek: `null`

<a name="concat"></a>

## <a name="concat"></a>concat

İki veya daha fazla dizeleri birleştirmek ve birleştirilmiş dizeyi döndürür. 

```
concat('<text1>', '<text2>', ...)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Metin1*>, <*Metin2*>,... | Evet | Dize | En az iki dizeyi birleştirmek için | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*text1text2...*> | Dize | Birleşik giriş dizelerden oluşturulan bir dize | 
|||| 

*Örnek*

Bu örnekte, "Hello" ve "World" dizeleri birleştirir:

```
concat('Hello', 'World')
```

Ve bu sonucu döndürür: `"HelloWorld"`

<a name="contains"></a>

## <a name="contains"></a>içerir

Bir koleksiyon belirli bir öğeye sahip olup olmadığını denetleyin. Öğesi bulunduğunda true döndürür veya return false olduğunda bulunamadı. Bu işlev büyük/küçük harf duyarlıdır.

```
contains('<collection>', '<value>')
contains([<collection>], '<value>')
```

Özellikle, bu işlev bu koleksiyon türleri üzerinde çalışır: 

* A *dize* bulmak için bir *substring*
* Bir *dizi* bulmak için bir *değeri*
* A *sözlük* bulmak için bir *anahtarı*

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Koleksiyonu*> | Evet | Dize, dizi veya sözlük | Denetlenecek koleksiyonu | 
| <*Değer*> | Evet | Dize, dizi veya sözlük, sırasıyla | Bulunacak öğe | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| TRUE veya false | Boole | Öğesi bulunduğunda true döndürür. Ne zaman bulunamadı false döndürür. |
|||| 

*Örnek 1*

Bu örnek alt dizeyi "world" için "hello world" dizesi denetler ve true değerini döndürür:

```
contains('hello world', 'world')
```

*Örnek 2*

Bu örnek alt dizeyi "universe" için "hello world" dizesi arar ve false değerini döndürür:

```
contains('hello world', 'universe')
```

<a name="convertFromUtc"></a>

## <a name="convertfromutc"></a>convertFromUtc

Bir zaman damgası (UTC) Evrensel zaman Eşgüdümlü gelen hedef zaman dilimine dönüştürün.

```
convertFromUtc('<timestamp>', '<destinationTimeZone>', '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*zaman damgası*> | Evet | Dize | Zaman damgası içeren dize | 
| <*destinationTimeZone*> | Evet | Dize | Hedef saat dilimi adı. Daha fazla bilgi için bkz: [saat dilimi kimlikleri](https://docs.microsoft.com/previous-versions/windows/embedded/gg154758(v=winembedded.80)). | 
| <*Biçimi*> | Hayır | Dize | Ya da bir [tek biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim deseni](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Zaman damgası için varsayılan biçimi olan ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-aa-ddT:mm:ss:fffffffK), ile uyumlu [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ve saat dilimi bilgilerini korur. |
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*Dönüştürülen zaman damgası*> | Dize | Hedef zaman dilimine dönüştürülen zaman damgası | 
|||| 

*Örnek 1*

Bu örnekte bir zaman damgası belirtilen saat dilimine dönüştürür: 

```
convertFromUtc('2018-01-01T08:00:00.0000000Z', 'Pacific Standard Time')
```

Ve bu sonucu döndürür: `"2018-01-01T00:00:00.0000000"`

*Örnek 2*

Bu örnekte bir zaman damgası biçimi ve belirtilen saat dilimi dönüştürür:

```
convertFromUtc('2018-01-01T08:00:00.0000000Z', 'Pacific Standard Time', 'D')
```

Ve bu sonucu döndürür: `"Monday, January 1, 2018"`

<a name="convertTimeZone"></a>

## <a name="converttimezone"></a>convertTimeZone

Bir zaman damgası hedef zaman dilimine kaynak saat dilimine dönüştürür.

```
convertTimeZone('<timestamp>', '<sourceTimeZone>', '<destinationTimeZone>', '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*zaman damgası*> | Evet | Dize | Zaman damgası içeren dize | 
| <*sourceTimeZone*> | Evet | Dize | Kaynak saat dilimi adı. Daha fazla bilgi için bkz: [saat dilimi kimlikleri](https://docs.microsoft.com/previous-versions/windows/embedded/gg154758(v=winembedded.80)). | 
| <*destinationTimeZone*> | Evet | Dize | Hedef saat dilimi adı. Daha fazla bilgi için bkz: [saat dilimi kimlikleri](https://docs.microsoft.com/previous-versions/windows/embedded/gg154758(v=winembedded.80)). | 
| <*Biçimi*> | Hayır | Dize | Ya da bir [tek biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim deseni](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Zaman damgası için varsayılan biçimi olan ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-aa-ddT:mm:ss:fffffffK), ile uyumlu [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ve saat dilimi bilgilerini korur. |
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*Dönüştürülen zaman damgası*> | Dize | Hedef zaman dilimine dönüştürülen zaman damgası | 
|||| 

*Örnek 1*

Bu örnekte kaynak saat dilimi hedef saat dilimine dönüştürür: 

```
convertTimeZone('2018-01-01T08:00:00.0000000Z', 'UTC', 'Pacific Standard Time')
```

Ve bu sonucu döndürür: `"2018-01-01T00:00:00.0000000"`

*Örnek 2*

Bu örnek bir saat dilimi belirtilen saat dilimi ve biçimine dönüştürür:

```
convertTimeZone('2018-01-01T80:00:00.0000000Z', 'UTC', 'Pacific Standard Time', 'D')
```

Ve bu sonucu döndürür: `"Monday, January 1, 2018"`

<a name="convertToUtc"></a>

## <a name="converttoutc"></a>convertToUtc

Bir zaman damgası (UTC) saati Eşgüdümlü Evrensel Kaynak saat dilimine dönüştürün.

```
convertToUtc('<timestamp>', '<sourceTimeZone>', '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*zaman damgası*> | Evet | Dize | Zaman damgası içeren dize | 
| <*sourceTimeZone*> | Evet | Dize | Kaynak saat dilimi adı. Daha fazla bilgi için bkz: [saat dilimi kimlikleri](https://docs.microsoft.com/previous-versions/windows/embedded/gg154758(v=winembedded.80)). | 
| <*Biçimi*> | Hayır | Dize | Ya da bir [tek biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim deseni](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Zaman damgası için varsayılan biçimi olan ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-aa-ddT:mm:ss:fffffffK), ile uyumlu [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ve saat dilimi bilgilerini korur. |
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*Dönüştürülen zaman damgası*> | Dize | UTC'ye dönüştürülür zaman damgası | 
|||| 

*Örnek 1*

Bu örnek için UTC zaman damgası dönüştürür: 

```
convertToUtc('01/01/2018 00:00:00', 'Pacific Standard Time')
```

Ve bu sonucu döndürür: `"2018-01-01T08:00:00.0000000Z"`

*Örnek 2*

Bu örnek için UTC zaman damgası dönüştürür:

```
convertToUtc('01/01/2018 00:00:00', 'Pacific Standard Time', 'D')
```

Ve bu sonucu döndürür: `"Monday, January 1, 2018"`

<a name="createArray"></a>

## <a name="createarray"></a>createArray

Birden çok girişlerinde bir dizi döndürür. Tek giriş diziler için bkz: [array()](#array).

```
createArray('<object1>', '<object2>', ...)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*object1*>, <*object2*>,... | Evet | Tüm, karışık değil | Dizi oluşturmak için en az iki öğe | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| [<*object1*>, <*object2*>,...] | Dizi | Giriş öğelerinden oluşan dizi | 
|||| 

*Örnek*

Bu örnek, bir dizi bu girişlerinde oluşturur:

```
createArray('h', 'e', 'l', 'l', 'o')
```

Ve bu sonucu döndürür: `["h", "e", "l", "l", "o"]`

<a name="dataUri"></a>

## <a name="datauri"></a>dataUri

Dize veri Tekdüzen Kaynak Tanımlayıcısı (URI) döndürür. 

```
dataUri('<value>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Değer*> | Evet | Dize | Dönüştürülecek dizeyi | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*Veri URI'sı*> | Dize | Giriş dizesi veri URI'si | 
|||| 

*Örnek*

Bu örnek, "hello" dizesi için bir veri URI oluşturur:

```
dataUri('hello') 
```

Ve bu sonucu döndürür: `"data:text/plain;charset=utf-8;base64,aGVsbG8="`

<a name="dataUriToBinary"></a>

## <a name="datauritobinary"></a>dataUriToBinary

Bir veri Tekdüzen Kaynak Tanımlayıcısı (URI) için ikili dosya sürümü döndürür. Bu işlevi kullanmak yerine [decodeDataUri()](#decodeDataUri). Her iki işlevleri aynı şekilde çalışır ancak `decodeDataUri()` tercih edilir.

```
dataUriToBinary('<value>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Değer*> | Evet | Dize | Verileri dönüştürmek için URI | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*ikili için veri URI*> | Dize | Veri URI'si için ikili dosya sürümü | 
|||| 

*Örnek*

Bu örnek, bu veri URI'si için ikili dosya sürümü oluşturur:

```
dataUriToBinary('data:text/plain;charset=utf-8;base64,aGVsbG8=')
```

Ve bu sonucu döndürür: 

`"01100100011000010111010001100001001110100111010001100101011110000111010000101111011100000
1101100011000010110100101101110001110110110001101101000011000010111001001110011011001010111
0100001111010111010101110100011001100010110100111000001110110110001001100001011100110110010
10011011000110100001011000110000101000111010101100111001101100010010001110011100000111101"`

<a name="dataUriToString"></a>

## <a name="datauritostring"></a>dataUriToString

Bir veri Tekdüzen Kaynak Tanımlayıcısı (URI) dize sürümü döndürür.

```
dataUriToString('<value>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Değer*> | Evet | Dize | Verileri dönüştürmek için URI | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*dize için veri URI*> | Dize | Veri URI'si için dize sürümü | 
|||| 

*Örnek*

Bu örnek, bu veri URI'si için bir dize oluşturur:

```
dataUriToString('data:text/plain;charset=utf-8;base64,aGVsbG8=')
```

Ve bu sonucu döndürür: `"hello"`

<a name="dayOfMonth"></a>

## <a name="dayofmonth"></a>DayOfMonth

Bir zaman damgası ayın gününü döndür. 

```
dayOfMonth('<timestamp>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*zaman damgası*> | Evet | Dize | Zaman damgası içeren dize | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*Ayın günü*> | Tamsayı | Belirtilen zaman damgası gelen ayın günü | 
|||| 

*Örnek*

Bu örnek sayısı bu zaman damgası ayın günü döndürür:

```
dayOfMonth('2018-03-15T13:27:36Z')
```

Ve bu sonucu döndürür: `15`

<a name="dayOfWeek"></a>

## <a name="dayofweek"></a>DayOfWeek

Bir zaman damgası haftanın gününü döndür.  

```
dayOfWeek('<timestamp>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*zaman damgası*> | Evet | Dize | Zaman damgası içeren dize | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*Haftanın günü*> | Tamsayı | 1 ve benzeri Pazar Pazartesi 0 olduğu belirtilen zaman damgası gelen haftanın günü olduğunu | 
|||| 

*Örnek*

Bu örnek sayısı, bu zaman damgası haftanın günü için döndürür:

```
dayOfWeek('2018-03-15T13:27:36Z')
```

Ve bu sonucu döndürür: `3`

<a name="dayOfYear"></a>

## <a name="dayofyear"></a>DayOfYear

Bir zaman damgası yılın gününü döndür. 

```
dayOfYear('<timestamp>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*zaman damgası*> | Evet | Dize | Zaman damgası içeren dize | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*yılın günü*> | Tamsayı | Belirtilen zaman damgası gelen yılın günü | 
|||| 

*Örnek*

Bu örnekte, bu zaman damgası yılın günü sayısını döndürür:

```
dayOfYear('2018-03-15T13:27:36Z')
```

Ve bu sonucu döndürür: `74`

<a name="decodeBase64"></a>

## <a name="decodebase64"></a>decodeBase64

Etkili bir şekilde base64 dizesi kod çözme base64 ile kodlanmış bir dize, string sürümüne dönün. Kullanmayı [base64ToString()](#base64ToString) yerine `decodeBase64()`. Her iki işlevleri aynı şekilde çalışır ancak `base64ToString()` tercih edilir.

```
decodeBase64('<value>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Değer*> | Evet | Dize | Kod çözme için base64 ile kodlanmış dize | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*kodunu çözdü-base64-dizesi*> | Dize | Base64 ile kodlanmış bir dize için dize sürümü | 
|||| 

*Örnek*

Bu örnek, base64 ile kodlanmış bir dize için bir dize oluşturur:

```
decodeBase64('aGVsbG8=')
```

Ve bu sonucu döndürür: `"hello"`

<a name="decodeDataUri"></a>

## <a name="decodedatauri"></a>decodeDataUri

Bir veri Tekdüzen Kaynak Tanımlayıcısı (URI) için ikili dosya sürümü döndürür. Kullanmayı [dataUriToBinary()](#dataUriToBinary), yerine `decodeDataUri()`. Her iki işlevleri aynı şekilde çalışır ancak `dataUriToBinary()` tercih edilir.

```
decodeDataUri('<value>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Değer*> | Evet | Dize | Kod çözme için URI dizesi verileri | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*ikili için veri URI*> | Dize | Veri URI dizesini için ikili dosya sürümü | 
|||| 

*Örnek*

Bu örnekte, bu veri URI'si için ikili dosya sürümü döndürür:

```
decodeDataUri('data:text/plain;charset=utf-8;base64,aGVsbG8=')
```

Ve bu sonucu döndürür: 

`"01100100011000010111010001100001001110100111010001100101011110000111010000101111011100000
1101100011000010110100101101110001110110110001101101000011000010111001001110011011001010111
0100001111010111010101110100011001100010110100111000001110110110001001100001011100110110010
10011011000110100001011000110000101000111010101100111001101100010010001110011100000111101"`

<a name="decodeUriComponent"></a>

## <a name="decodeuricomponent"></a>Decodeurıcomponent

Return değiştirir kodu çözülmüş sürümleriyle karakterlerinden Çık bir dize. 

```
decodeUriComponent('<value>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Değer*> | Evet | Dize | Kod çözme için kaçış karakterli dize | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*kodunu çözdü URI*> | Dize | Kodu çözülmüş kaçış karakterleri güncelleştirilmiş dizesiyle | 
|||| 

*Örnek*

Bu örnek kodu çözülmüş sürümleriyle bu dizesindeki kaçış karakterleri değiştirir:

```
decodeUriComponent('http%3A%2F%2Fcontoso.com')
```

Ve bu sonucu döndürür: `"https://contoso.com"`

<a name="div"></a>

## <a name="div"></a>div

İki sayı bölen tamsayı sonuç döndürür. Kalan sonuç almak için bkz: [mod()](#mod).

```
div(<dividend>, <divisor>)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Bölünen*> | Evet | Tamsayı veya kayan nokta | Sayı bölün *bölen* | 
| <*bölen*> | Evet | Tamsayı veya kayan nokta | Bölen sayıyı *bölünen*, 0 olamaz, ancak | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*sayının sonucu*> | Tamsayı | İkinci sayı ilk sayıyla bölerek gelen tamsayı sonuç | 
|||| 

*Örnek*

Örneklerin her ikisi de ikinci sayı ilk sayıya bölme:

```
div(10, 5)
div(11, 5)
```

Ve bu sonucu döndürür: `2`

<a name="encodeUriComponent"></a>

## <a name="encodeuricomponent"></a>Encodeurıcomponent

Kaçış karakterli URL güvenli olmayan karakterleri değiştirerek bir dize için bir Tekdüzen Kaynak Tanımlayıcısı (URI) kodlu sürümü döndürür. Kullanmayı [uriComponent()](#uriComponent), yerine `encodeUriComponent()`. Her iki işlevleri aynı şekilde çalışır ancak `uriComponent()` tercih edilir.

```
encodeUriComponent('<value>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Değer*> | Evet | Dize | Dize URI kodlanmış biçimine dönüştürmek için | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*Kodlanmış URI*> | Dize | Kaçış karakterli URI ile kodlanmış dize | 
|||| 

*Örnek*

Bu örnek, bu dize için bir URI kodlu sürümü oluşturur:

```
encodeUriComponent('https://contoso.com')
```

Ve bu sonucu döndürür: `"http%3A%2F%2Fcontoso.com"`

<a name="empty"></a>

## <a name="empty"></a>boş

Bir koleksiyonu boş olup olmadığını denetleyin. Koleksiyon boş olduğunda true döndürür veya boş değilse, false döndürür.

```
empty('<collection>')
empty([<collection>])
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Koleksiyonu*> | Evet | Dize, dizi veya nesne | Denetlenecek koleksiyonu | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| TRUE veya false | Boole | Koleksiyon boş olduğunda true döndürür. Boş değil olduğunda false döndürür. | 
|||| 

*Örnek* 

Bu örnekler belirtilen koleksiyonlarla boş olup olmadığını denetleyin:

```
empty('')
empty('abc')
```

Ve bu sonuçları döndürür: 

* İlk örneği: işlev döndürecek şekilde boş bir dize geçirir `true`. 
* İkinci örneği: işlev döndürecek şekilde "abc" dizesini geçirir `false`. 

<a name="endswith"></a>

## <a name="endswith"></a>endsWith

Bir dizeyi belirli bir alt dizesi ile biten olup olmadığını denetleyin. Alt dizeyi bulunduğunda true döndürür veya return false olduğunda bulunamadı. Bu işlev büyük küçük harfe duyarlı değildir.

```
endsWith('<text>', '<searchText>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Metin*> | Evet | Dize | Denetlenecek dize | 
| <*Aramametni*> | Evet | Dize | Bitiş alt dizeyi bulur | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| TRUE veya false  | Boole | Bitiş alt dizeyi bulunduğunda true döndürür. Ne zaman bulunamadı false döndürür. | 
|||| 

*Örnek 1* 

Bu örnekte, "hello world" dize "world" dizesi ile biten olup olmadığını denetler:

```
endsWith('hello world', 'world')
```

Ve bu sonucu döndürür: `true`

*Örnek 2*

Bu örnekte, "hello world" dize "universe" dizesiyle biten olup olmadığını denetler:

```
endsWith('hello world', 'universe')
```

Ve bu sonucu döndürür: `false`

<a name="equals"></a>

## <a name="equals"></a>şuna eşittir:

Hem değerleri, ifadeler ve nesneleri eşdeğer olup olmadığını denetleyin. Her ikisi de eşdeğer veya eşdeğer olmadıklarını olduğunda false döndürür, true döndürür.

```
equals('<object1>', '<object2>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*object1*>, <*object2*> | Evet | Çeşitli | Değerleri, ifadeler veya karşılaştırmak için nesneleri | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| TRUE veya false | Boole | Her ikisi de eşdeğer olduğunda true döndürür. Eşdeğer değil olduğunda false döndürür. | 
|||| 

*Örnek*

Bu örnekler belirtilen girişleri eşdeğer olup olmadığını denetleyin. 

```
equals(true, 1)
equals('abc', 'abcd')
```

Ve bu sonuçları döndürür: 

* İlk örneği: işlev döndürecek şekilde her iki değerin eşdeğerdir `true`.
* İkinci exmaple: işlevi döndürecek şekilde her iki değerin eşdeğer olmayan `false`.

<a name="first"></a>

## <a name="first"></a>ilk

Bir dizenin veya dizinin ilk öğesi döndürür.

```
first('<collection>')
first([<collection>])
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Koleksiyonu*> | Evet | Dize veya dizi | Koleksiyon ilk öğe nerede bulacağını |
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*ilk koleksiyon öğesi*> | Herhangi biri | Koleksiyondaki ilk öğe | 
|||| 

*Örnek*

Bu örnekler, bu koleksiyonları ilk öğe bulun:

```
first('hello')
first([0, 1, 2])
```

Ve bu sonuçları döndürür: 

* İlk örnek: `"h"`
* İkinci exmaple: `0`

<a name="float"></a>

## <a name="float"></a>Kayan nokta

Kayan nokta sayısı için bir dize sürüm gerçek bir kayan nokta sayısı dönüştürün. Yalnızca özel parametreler bir mantıksal uygulama gibi bir uygulama için geçirirken bu işlevi kullanabilirsiniz.

```
float('<value>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Değer*> | Evet | Dize | Dönüştürme için geçerli bir kayan noktalı sayı olan dize |
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*float değeri*> | Kayan nokta | Belirtilen dize için kayan nokta sayısı | 
|||| 

*Örnek*

Bu örnek, bir dize sürüm için bu kayan noktalı sayı oluşturur:

```
float('10.333')
```

Ve bu sonucu döndürür: `10.333`

<a name="formatDateTime"></a>

## <a name="formatdatetime"></a>FormatDateTime

Bir zaman damgası belirtilen biçimde döndürür.

```
formatDateTime('<timestamp>', '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*zaman damgası*> | Evet | Dize | Zaman damgası içeren dize | 
| <*Biçimi*> | Hayır | Dize | Ya da bir [tek biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim deseni](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Zaman damgası için varsayılan biçimi olan ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-aa-ddT:mm:ss:fffffffK), ile uyumlu [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ve saat dilimi bilgilerini korur. |
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*yeniden biçimlendirilen zaman damgası*> | Dize | Belirtilen biçiminde güncelleştirilmiş zaman damgası | 
|||| 

*Örnek*

Bu örnekte bir zaman damgası belirtilen biçimine dönüştürür:

```
formatDateTime('03/15/2018 12:00:00', 'yyyy-MM-ddTHH:mm:ss')
```

Ve bu sonucu döndürür: `"2018-03-15T12:00:00"`

<a name="formDataMultiValues"></a>

## <a name="formdatamultivalues"></a>formDataMultiValues

Bir eylemin bir anahtar adına uyan değerleri içeren bir dizi döndürecek *form verilerini* veya *form kodlu* çıktı. 

```
formDataMultiValues('<actionName>', '<key>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*EylemAdı*> | Evet | Dize | İstediğiniz çıktısı anahtar değerine sahip eylemi | 
| <*Anahtarı*> | Evet | Dize | Değer istediğiniz anahtar için ad | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| [<*dizisi ile anahtar değerleri*>] | Dizi | Belirtilen anahtarı uyan tüm değerleri içeren bir dizi | 
|||| 

*Örnek* 

Bu örnek, belirtilen eylemin form verilerini ya da form kodlu çıkış "Konu" anahtarın değerinden bir dizi oluşturur:  

```
formDataMultiValues('Send_an_email', 'Subject')
```

Ve örneğin bir dizide konu metni döndürür: `["Hello world"]`

<a name="formDataValue"></a>

## <a name="formdatavalue"></a>formDataValue

Bir eylemin bir anahtar adı ile eşleşen tek bir değer döndürmesi *form verilerini* veya *form kodlu* çıktı. İşlev işlevi birden fazla eşleşme bulursa bir hata oluşturur.

```
formDataValue('<actionName>', '<key>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*EylemAdı*> | Evet | Dize | İstediğiniz çıktısı anahtar değerine sahip eylemi | 
| <*Anahtarı*> | Evet | Dize | Değer istediğiniz anahtar için ad |
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*anahtar-değer*> | Dize | Belirtilen anahtar değeri  | 
|||| 

*Örnek* 

Bu örnek, "Konu" anahtarın değerden belirtilen eylemin form verileri veya çıkış form kodlu bir dize oluşturur:  

```
formDataValue('Send_an_email', 'Subject')
```

Ve konu metni örneğin bir dize döndürür: `"Hello world"`

<a name="getFutureTime"></a>

## <a name="getfuturetime"></a>getFutureTime

Geçerli zaman damgası artı belirtilen zaman birimlerini döndür.

```
getFutureTime(<interval>, <timeUnit>, <format>?)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*aralığı*> | Evet | Tamsayı | Çıkarmak için belirtilen süre birimlerinin sayısı | 
| <*timeUnit*> | Evet | Dize | İle kullanılacak zaman birimini *aralığı*: "Saniye", "Dakika", "Saat", "Gün", "Hafta", "Ay", "Yıl" | 
| <*Biçimi*> | Hayır | Dize | Ya da bir [tek biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim deseni](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Zaman damgası için varsayılan biçimi olan ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-aa-ddT:mm:ss:fffffffK), ile uyumlu [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ve saat dilimi bilgilerini korur. | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*güncelleştirilmiş zaman damgası*> | Dize | Belirtilen zaman birimleri sayısı artı geçerli zaman damgası | 
|||| 

*Örnek 1*

Geçerli zaman damgası olduğunu varsayın "2018-03-01T00:00:00.0000000Z". Bu örnek, o zaman damgası beş gün ekler:

```
getFutureTime(5, 'Day')
```

Ve bu sonucu döndürür: `"2018-03-06T00:00:00.0000000Z"`

*Örnek 2*

Geçerli zaman damgası olduğunu varsayın "2018-03-01T00:00:00.0000000Z". Bu örnek, beş gün ekler ve sonucu "D" biçimine dönüştürür:

```
getFutureTime(5, 'Day', 'D')
```

Ve bu sonucu döndürür: `"Tuesday, March 6, 2018"`

<a name="getPastTime"></a>

## <a name="getpasttime"></a>getPastTime

Belirtilen zaman birimlerini eksi geçerli zaman damgası döndür.

```
getPastTime(<interval>, <timeUnit>, <format>?)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*aralığı*> | Evet | Tamsayı | Çıkarmak için belirtilen süre birimlerinin sayısı | 
| <*timeUnit*> | Evet | Dize | İle kullanılacak zaman birimini *aralığı*: "Saniye", "Dakika", "Saat", "Gün", "Hafta", "Ay", "Yıl" | 
| <*Biçimi*> | Hayır | Dize | Ya da bir [tek biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim deseni](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Zaman damgası için varsayılan biçimi olan ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-aa-ddT:mm:ss:fffffffK), ile uyumlu [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ve saat dilimi bilgilerini korur. | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*güncelleştirilmiş zaman damgası*> | Dize | Belirtilen sayıda zaman birimleri eksi geçerli zaman damgası | 
|||| 

*Örnek 1*

Geçerli zaman damgası olduğunu varsayın "2018-02-01T00:00:00.0000000Z". Bu örnekte, zaman damgası beş gün çıkarır:

```
getPastTime(5, 'Day')
```

Ve bu sonucu döndürür: `"2018-01-27T00:00:00.0000000Z"`

*Örnek 2*

Geçerli zaman damgası olduğunu varsayın "2018-02-01T00:00:00.0000000Z". Bu örnekte, beş gün çıkarır ve sonuç "D" biçimine dönüştürür:

```
getPastTime(5, 'Day', 'D')
```

Ve bu sonucu döndürür: `"Saturday, January 27, 2018"`

<a name="greater"></a>

## <a name="greater"></a>büyük

İlk değer ikinci değerden daha büyük olup olmadığını denetleyin. İlk değer daha fazla olduğunda true döndürür veya ne zaman return false daha az.

```
greater(<value>, <compareTo>)
greater('<value>', '<compareTo>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Değer*> | Evet | Tamsayı, kayan noktalı sayı veya dize | İlk değer ikinci değerden büyük olmadığını denetleyin | 
| <*CompareTo*> | Evet | Tamsayı, kayan noktalı sayı veya dize, sırasıyla | Karşılaştırma değeri | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| TRUE veya false | Boole | İlk değer ikinci değerden daha büyük olduğunda true döndürür. İlk değer eşit veya ikinci değerden daha az olduğunda false döndürür. | 
|||| 

*Örnek*

Bu örnekler ilk değer ikinci değerden daha büyük olup olmadığını denetleyin:

```
greater(10, 5)
greater('apple', 'banana')
```

Ve bu sonuçları döndürür: 

* İlk örnek: `true`
* İkinci örnek: `false`

<a name="greaterOrEquals"></a>

## <a name="greaterorequals"></a>greaterOrEquals

İlk değer ikinci değerine eşit veya daha büyük olup olmadığını denetleyin.
İlk değeri sıfırdan büyük veya eşit olduğunda true döndürür veya ilk değeri daha az olduğunda false döndürür.

```
greaterOrEquals(<value>, <compareTo>)
greaterOrEquals('<value>', '<compareTo>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Değer*> | Evet | Tamsayı, kayan noktalı sayı veya dize | İkinci değerine eşit veya daha büyük olup olmadığını denetlemek için ilk değeri | 
| <*CompareTo*> | Evet | Tamsayı, kayan noktalı sayı veya dize, sırasıyla | Karşılaştırma değeri | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| TRUE veya false | Boole | İlk değer ikinci değerine eşit veya daha büyük olduğunda true döndürür. İkinci değer'den küçük ilk değer olduğunda false döndürür. | 
|||| 

*Örnek*

Bu örnekler, ilk değer ikinci değerinden büyük veya eşit olup olmadığını denetleyin:

```
greaterOrEquals(5, 5)
greaterOrEquals('apple', 'banana')
```

Ve bu sonuçları döndürür: 

* İlk örnek: `true`
* İkinci örnek: `false`

<a name="guid"></a>

## <a name="guid"></a>GUID

Genel benzersiz tanıtıcısı (GUID) dizesi, örneğin, "c2ecc88d-88c8-4096-912c-d6f2e2b138ce" oluşturun: 

```
guid()
```

Ayrıca, varsayılan biçimi, tire ile ayrılmış 32 basamak "D" dışında GUID için farklı bir biçim belirtin.

```
guid('<format>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Biçimi*> | Hayır | Dize | Tek bir [belirticisi biçimlendirmek](https://msdn.microsoft.com/library/97af8hh4) döndürülen GUID. Varsayılan olarak, "D" biçimidir, ancak "N", "D", "B", "P" veya "X" kullanabilirsiniz. | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*GUID değeri*> | Dize | Rastgele oluşturulmuş bir GUID | 
|||| 

*Örnek* 

Bu örnek aynı GUID oluşturur ama 32 basamaklıdır tire ile ayrılmış ve parantez içine alınmış: 

```
guid('P')
```

Ve bu sonucu döndürür: `"(c2ecc88d-88c8-4096-912c-d6f2e2b138ce)"`

<a name="if"></a>

## <a name="if"></a>if

Bir ifadenin true veya false olup olmadığını denetleyin. Sonucuna bağlı olarak, belirtilen bir değeri döndürür.

```
if(<expression>, <valueIfTrue>, <valueIfFalse>)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*ifade*> | Evet | Boole | Denetlenecek ifade | 
| <*Koşul*> | Evet | Herhangi biri | Bir ifade doğru ise döndürülecek değeri | 
| <*valueIfFalse*> | Evet | Herhangi biri | İfade yanlış olduğunda döndürülecek değer | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*Belirtilen-dönüş-değeri*> | Herhangi biri | Olup olmadığına göre tabanlı döndüren belirtilen değeri true veya false ifadesidir | 
|||| 

*Örnek* 

Bu örnek döndürür `"yes"` çünkü belirtilen ifade true döndürür. Aksi takdirde, örnek döndürür `"no"`:

```
if(equals(1, 1), 'yes', 'no')
```

<a name="indexof"></a>

## <a name="indexof"></a>IndexOf

Başlangıç konumu veya bir alt dizesi için bir dizin değeri döndürür. Bu işlev büyük küçük harfe duyarlı değildir ve dizinleri 0 rakamla başlayamaz. 

```
indexOf('<text>', '<searchText>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Metin*> | Evet | Dize | Bulunacak alt dizeyi içeren dizeyi | 
| <*Aramametni*> | Evet | Dize | Bulunacak alt dizeyi | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*Dizin değeri*>| Tamsayı | Belirtilen alt dizeyi başlangıç konumu ya da dizin değeri. <p>Dize bulunamazsa numarası -1 döndürür. </br>Dize boşsa 0 sayısını döndürür. | 
|||| 

*Örnek* 

Bu örnek başlangıç dizini değerini "hello world" dizesindeki "world" alt dizeyi bulur:

```
indexOf('hello world', 'world')
```

Ve bu sonucu döndürür: `6`

<a name="int"></a>

## <a name="int"></a>Int

Tamsayı sürüm bir dize döndürür.

```
int('<value>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Değer*> | Evet | Dize | Dönüştürülecek dizeyi | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*tamsayı sonuç*> | Tamsayı | Belirtilen dize için tamsayı sürümü | 
|||| 

*Örnek* 

Bu örnek, bir tamsayı sürümü "10" dizesini oluşturur:

```
int('10')
```

Ve bu sonucu döndürür: `10`

<a name="item"></a>

## <a name="item"></a>Öğesi

İçinde bir yinelenen eylemi bir dizi kullanıldığında, geçerli öğenin dizideki eylemin geçerli yineleme sırasında döndür. Ayrıca, bu öğenin özelliklerinden değerleri alabilirsiniz. 

```
item()
```

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*Geçerli dizi öğesi*> | Herhangi biri | Dizi eylemin geçerli yineleme için geçerli öğesi | 
|||| 

*Örnek* 

Bu örnek alır `body` öğesi için-her döngünün geçerli yineleme içinde "Send_an_email" eylemi için geçerli iletisi:

```
item().body
```

<a name="items"></a>

## <a name="items"></a>öğeler

Geçerli öğe için-her bir döngü her döngüsünde döndürmek. Bu işlev için-her bir döngü içinde kullanın.

```
items('<loopName>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*loopName*> | Evet | Dize | Ad için-her bir döngü için | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*Öğesi*> | Herhangi biri | Belirtilen için-her bir döngü geçerli döngüsünde öğesinden | 
|||| 

*Örnek* 

Bu örnek belirtilen için-her döngüden geçerli öğeyi alır:

```
items('myForEachLoopName')
```

<a name="json"></a>

## <a name="json"></a>JSON

JavaScript nesne gösterimi (JSON) türü değeri veya bir dize veya XML nesnesi döndürür.

```
json('<value>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Değer*> | Evet | Dize veya XML | Dize veya XML dönüştürmek için | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*JSON sonuç*> | JSON yerel tür veya nesne | JSON yerel tür değer veya belirtilen dize veya XML için nesne. Dize null ise, işlev boş bir nesne döndürür. | 
|||| 

*Örnek 1* 

Bu örnekte bu dize JSON değerine dönüştürür:

```
json('[1, 2, 3]')
```

Ve bu sonucu döndürür: `[1, 2, 3]`

*Örnek 2*

Bu örnek, JSON olarak bu dize dönüştürür: 

```
json('{"fullName": "Sophia Owen"}')
```

Ve bu sonucu döndürür:

```
{
  "fullName": "Sophia Owen"
}
```

*Örnek 3*

Bu örnek, JSON için bu XML dönüştürür: 

```
json(xml('<?xml version="1.0"?> <root> <person id='1'> <name>Sophia Owen</name> <occupation>Engineer</occupation> </person> </root>'))
```

Ve bu sonucu döndürür:

```json
{ 
   "?xml": { "@version": "1.0" }, 
   "root": { 
      "person": [ { 
         "@id": "1", 
         "name": "Sophia Owen", 
         "occupation": "Engineer" 
       } ] 
   } 
}
```

<a name="intersection"></a>

## <a name="intersection"></a>kesişim

Sahip bir koleksiyonun dönmek *yalnızca* belirtilen koleksiyonlarla arasında ortak öğeler. Sonuçta görünmesini için bir öğe bu işleve tüm koleksiyonlarında görünmesi gerekir. Bir veya daha fazla öğe aynı ada sahipse, bu adı taşıyan son öğe sonucunda görünür.

```
intersection([<collection1>], [<collection2>], ...)
intersection('<collection1>', '<collection2>', ...)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Collection1*>, <*collection2*>,... | Evet | Dizi veya nesne, ancak ikisini | Burada istediğiniz koleksiyonları *yalnızca* ortak öğeler | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*Ortak öğeler*> | Dizi veya nesne, sırasıyla | Yalnızca ortak öğeler belirtilen koleksiyonlar genelinde sahip bir koleksiyonun | 
|||| 

*Örnek* 

Bu örnekte bu diziler arasında ortak öğeleri bulur:  

```
intersection([1, 2, 3], [101, 2, 1, 10], [6, 8, 1, 2])
```

Ve bir dizi döndürür *yalnızca* bu öğeleri: `[1, 2]`

<a name="join"></a>

## <a name="join"></a>join

Bir dizinin tüm öğeleri ve her bir karakteri sahip bir dize ayırarak return bir *sınırlayıcı*.

```
join([<collection>], '<delimiter>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Koleksiyonu*> | Evet | Dizi | Katılmak için öğeleri içeren dizi |  
| <*sınırlayıcı*> | Evet | Dize | Her karakteri oluşan dizedeki arasında görünen ayırıcıyı | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*char1*><*sınırlayıcı*><*char2*><*sınırlayıcı*>... | Dize | Belirtilen dizinin tüm öğeleri oluşturulan sonuç dizesi |
|||| 

*Örnek* 

Bu örnek bir dizeyi belirtilen karakterle bu dizinin tüm öğeleri sınırlayıcısı oluşturur:

```
join([a, b, c], '.')
```

Ve bu sonucu döndürür: `"a.b.c"`

<a name="last"></a>

## <a name="last"></a>Son

Son öğeyi koleksiyondan döndürür.

```
last('<collection>')
last([<collection>])
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Koleksiyonu*> | Evet | Dize veya dizi | Koleksiyon son öğeyi nerede bulacağını | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*Son koleksiyon öğesi*> | Dize veya dizi, sırasıyla | Koleksiyondaki son öğe | 
|||| 

*Örnek* 

Bu örnekler son öğeyi bu koleksiyonları bulun:

```
last('abcd')
last([0, 1, 2, 3])
```

Ve bu sonuçları döndürür: 

* İlk örnek: `"d"`
* İkinci örnek: `3`

<a name="lastindexof"></a>

## <a name="lastindexof"></a>lastIndexOf

Bir alt dizesi bitiş konumu ya da dizin değeri döndürür. Bu işlev büyük küçük harfe duyarlı değildir ve dizinleri 0 rakamla başlayamaz.

```
lastIndexOf('<text>', '<searchText>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Metin*> | Evet | Dize | Bulunacak alt dizeyi içeren dizeyi | 
| <*Aramametni*> | Evet | Dize | Bulunacak alt dizeyi | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*Bitiş dizin değeri*> | Tamsayı | Belirtilen alt dizeyi bitiş konumu ya da dizin değeri. <p>Dize bulunamazsa numarası -1 döndürür. </br>Dize boşsa 0 sayısını döndürür. | 
|||| 

*Örnek* 

Bu örnek bitiş dizin değerini "hello world" dizesindeki "world" alt dizeyi bulur:

```
lastIndexOf('hello world', 'world')
```

Ve bu sonucu döndürür: `10`

<a name="length"></a>

## <a name="length"></a>uzunluğu

Bir koleksiyondaki öğe sayısını döndürür.

```
length('<collection>')
length([<collection>])
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Koleksiyonu*> | Evet | Dize veya dizi | Sayılacak öğelerle koleksiyonu | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*Uzunluk veya sayısı*> | Tamsayı | Koleksiyondaki öğe sayısı | 
|||| 

*Örnek*

Bu örnekler bu koleksiyonlardaki öğe sayısı: 

```
length('abcd')
length([0, 1, 2, 3])
```

Ve bu sonucu döndürür: `4`

<a name="less"></a>

## <a name="less"></a>daha az

İlk değer olup değerinden ikinci denetleyin.
İlk değer daha az olduğunda true döndürür veya ilk değeri daha fazla olduğunda false döndürür.

```
less(<value>, <compareTo>)
less('<value>', '<compareTo>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Değer*> | Evet | Tamsayı, kayan noktalı sayı veya dize | Küçüktür olup olmadığını denetlemek için ilk değer ikinci değer | 
| <*CompareTo*> | Evet | Tamsayı, kayan noktalı sayı veya dize, sırasıyla | Karşılaştırma öğesi | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| TRUE veya false | Boole | İkinci değer'den küçük ilk değer, true döndürür. İlk değer eşit veya değerinden ikinci olduğunda false döndürür. | 
|||| 

*Örnek*

Bu örnekler, ilk değer olup değerinden ikinci denetleyin.

```
less(5, 10)
less('banana', 'apple')
```

Ve bu sonuçları döndürür: 

* İlk örnek: `true`
* İkinci örnek: `false`

<a name="lessOrEquals"></a>

## <a name="lessorequals"></a>lessOrEquals

İlk değer küçük veya bu ikinci değer eşit olup olmadığını denetleyin.
İlk değer küçük veya buna eşit olduğunda true döndürür veya ilk değeri daha fazla olduğunda false döndürür.

```
lessOrEquals(<value>, <compareTo>)
lessOrEquals('<value>', '<compareTo>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Değer*> | Evet | Tamsayı, kayan noktalı sayı veya dize | Küçüktür olup olmadığını denetlemek için ilk değeri veya ikinci değere eşittir | 
| <*CompareTo*> | Evet | Tamsayı, kayan noktalı sayı veya dize, sırasıyla | Karşılaştırma öğesi | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| TRUE veya false  | Boole | İlk değer ikinci değerine eşit veya daha küçük olduğunda true döndürür. İlk değer ikinci değerden daha büyük olduğunda false döndürür. |  
|||| 

*Örnek*

Bu örnekler, ilk değer ikinci değerinden az veya ona eşit olup olmadığını denetleyin.

```
lessOrEquals(10, 10)
lessOrEquals('apply', 'apple')
```

Ve bu sonuçları döndürür: 

* İlk örnek: `true`
* İkinci örnek: `false`

<a name="listCallbackUrl"></a>

## <a name="listcallbackurl"></a>listCallbackUrl

"Bir tetikleyici ya da eylem çağıran geri çağırma URL'si" döndürür. Bu işlev yalnızca tetikleyiciler ve Eylemler için birlikte çalışır **HttpWebhook** ve **ApiConnectionWebhook** bağlayıcı türleri, ancak **el ile**,  **Yineleme**, **HTTP**, ve **APIConnection** türleri. 

```
listCallbackUrl()
```

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*geri çağırma URL'si*> | Dize | Bir tetikleyici veya eylem için geri çağırma URL'si |  
|||| 

*Örnek*

Bu örnek, örnek geri çağırma URL'si bu işlev döndürebilir gösterir:

`"https://prod-01.westus.logic.azure.com:443/workflows/<*workflow-ID*>/triggers/manual/run?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=<*signature-ID*>"`

<a name="max"></a>

## <a name="max"></a>en çok

Bir listeden veya her iki uçta dahil numaralarıyla dizi en yüksek değeri döndürür. 

```
max(<number1>, <number2>, ...)
max([<number1>, <number2>, ...])
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Sayı1*>, <*sayı2*>,... | Evet | Tamsayı, kayan nokta veya her ikisi | En yüksek değerini istediğiniz sayı kümesi | 
| [<*Sayı1*>, <*sayı2*>,...] | Evet | Dizi - tamsayı, kayan nokta veya her ikisi | En yüksek değerini istediğiniz sayı dizisi | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*en yüksek değeri*> | Tamsayı veya kayan nokta | Belirtilen dizi ya da sayı kümesi en yüksek değeri | 
|||| 

*Örnek* 

Bu örneklerin en yüksek değeri sayı ve dizi kümesinden alın:

```
max(1, 2, 3)
max([1, 2, 3])
```

Ve bu sonucu döndürür: `3`

<a name="min"></a>

## <a name="min"></a>dk

En düşük değer bir dizi numaraları veya bir dizi döndürür.

```
min(<number1>, <number2>, ...)
min([<number1>, <number2>, ...])
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Sayı1*>, <*sayı2*>,... | Evet | Tamsayı, kayan nokta veya her ikisi | En düşük değerini istediğiniz sayı kümesi | 
| [<*Sayı1*>, <*sayı2*>,...] | Evet | Dizi - tamsayı, kayan nokta veya her ikisi | En düşük değerini istediğiniz sayı dizisi | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*en küçük değer*> | Tamsayı veya kayan nokta | En düşük değer, sayı veya belirtilen dizi belirtilen kümesi | 
|||| 

*Örnek* 

Bu örnekler en düşük değer, sayı ve dizi kümesinde alın:

```
min(1, 2, 3)
min([1, 2, 3])
```

Ve bu sonucu döndürür: `1`

<a name="mod"></a>

## <a name="mod"></a>mod

İki sayı bölen kalanı döndürür. Tamsayı sonuç almak için bkz: [div()](#div).

```
mod(<dividend>, <divisor>)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Bölünen*> | Evet | Tamsayı veya kayan nokta | Sayı bölün *bölen* | 
| <*bölen*> | Evet | Tamsayı veya kayan nokta | Bölen sayıyı *bölünen*, 0 olamaz. | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*Modulo sonucu*> | Tamsayı veya kayan nokta | İkinci sayı ilk sayıyla bölerek gelen kalan | 
|||| 

*Örnek* 

Bu örnekte, ikinci sayı ilk sayıyı böler:

```
mod(3, 2)
```

Ve bu sonucu döndürür: `1`

<a name="mul"></a>

## <a name="mul"></a>mul

İki sayı çarparak ürün döndür.

```
mul(<multiplicand1>, <multiplicand2>)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*multiplicand1*> | Evet | Tamsayı veya kayan nokta | Tarafından çarpılacağı sayı *multiplicand2* | 
| <*multiplicand2*> | Evet | Tamsayı veya kayan nokta | Sayı, katları *multiplicand1* | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*Ürün sonucu*> | Tamsayı veya kayan nokta | İkinci sayı ilk sayıyla çarparak gelen ürün | 
|||| 

*Örnek* 

Bu örnekleri birden çok ilk sayıyla ikinci sayı:

```
mul(1, 2)
mul(1.5, 2)
```

Ve bu sonuçları döndürür:

* İlk örnek: `2`
* İkinci örneği `3`

<a name="multipartBody"></a>

## <a name="multipartbody"></a>multipartBody

Gövdesi belirli bir bölümü için birden çok bölümü olan bir eylemin çıkış döndür.

```
multipartBody('<actionName>', <index>)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*EylemAdı*> | Evet | Dize | Birden çok bölümleriyle çıktısı bir eylemin adı | 
| <*Dizin*> | Evet | Tamsayı | İstediğiniz bölümü için dizin değeri | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*Gövde*> | Dize | Belirtilen bölümü için gövdesi | 
|||| 

<a name="not"></a>

## <a name="not"></a>değil

Bir ifade yanlış olup olmadığını denetleyin. İfade yanlış olduğunda true döndürür veya doğru olduğunda false döndürür.

```
not(<expression>)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*ifade*> | Evet | Boole | Denetlenecek ifade | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| TRUE veya false | Boole | İfade yanlış olduğunda true döndürür. İfadenin true olduğunda false döndürür. |  
|||| 

*Örnek 1*

Bu örnekler belirtilen ifadelerinin false olup olmadığını denetleyin: 

```
not(false)
not(true)
```

Ve bu sonuçları döndürür:

* İlk örneği: işlev döndürecek şekilde ifadeyi false'tur `true`.
* İkinci örneği: işlev döndürecek şekilde ifadeyi doğrudur `false`.

*Örnek 2*

Bu örnekler belirtilen ifadelerinin false olup olmadığını denetleyin: 

```
not(equals(1, 2))
not(equals(1, 1))
```

Ve bu sonuçları döndürür:

* İlk örneği: işlev döndürecek şekilde ifadeyi false'tur `true`.
* İkinci örneği: işlev döndürecek şekilde ifadeyi doğrudur `false`.

<a name="or"></a>

## <a name="or"></a>or

En az bir ifadenin doğru olup olmadığını denetleyin. En az bir ifadenin true olduğunda true döndürür veya tümü false olduğunda false döndürür.

```
or(<expression1>, <expression2>, ...)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*İfade1*>, <*İfade2*>,... | Evet | Boole | Denetlenecek ifadeleri | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| TRUE veya false | Boole | En az bir ifadenin true olduğunda true döndürür. Tüm ifadeler false olduğunda false döndürür. |  
|||| 

*Örnek 1*

Bu örnekler, en az bir ifadenin doğru olup olmadığını denetleyin:

```
or(true, false)
or(false, false)
```

Ve bu sonuçları döndürür:

* İlk örnek: en az bir ifade doğruysa, işlev döndürecek şekilde `true`.
* İkinci örneği: işlev döndürecek şekilde her iki ifade yanlış `false`.

*Örnek 2*

Bu örnekler, en az bir ifadenin doğru olup olmadığını denetleyin:

```
or(equals(1, 1), equals(1, 2))
or(equals(1, 2), equals(1, 3))
```

Ve bu sonuçları döndürür:

* İlk örnek: en az bir ifade doğruysa, işlev döndürecek şekilde `true`.
* İkinci örneği: işlev döndürecek şekilde her iki ifade yanlış `false`.

<a name="parameters"></a>

## <a name="parameters"></a>parametreler

Logic app tanımı'nda açıklanan bir parametre için değer döndürür. 

```
parameters('<parameterName>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*parameterName*> | Evet | Dize | Değer istediğiniz parametre adı | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*parametre değeri*> | Herhangi biri | Belirtilen parametre değeri | 
|||| 

*Örnek* 

Bu JSON değer olduğunu varsayalım:

```json
{
  "fullName": "Sophia Owen"
}
```

Bu örnek belirtilen parametresinin değerini alır:

```
parameters('fullName')
```

Ve bu sonucu döndürür: `"Sophia Owen"`

<a name="rand"></a>

## <a name="rand"></a>rand

Yalnızca başlangıç sonunda dahil belirtilen bir aralıktan rasgele bir tamsayı döndürür.

```
rand(<minValue>, <maxValue>)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*MinValue*> | Evet | Tamsayı | En düşük tam sayı | 
| <*MaxValue*> | Evet | Tamsayı | En büyük tamsayıyı işlevi döndürebilen aralığında izleyen tamsayı | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*rastgele bir sonuç*> | Tamsayı | Belirtilen aralıktan döndürülen rastgele tamsayı |  
|||| 

*Örnek*

Bu örnek, maksimum değer hariç belirtilen aralıktan rasgele bir tamsayı alır: 

```
rand(1, 5)
```

Ve sonuç olarak bu numaraları döndürür: `1`, `2`, `3`, veya `4` 

<a name="range"></a>

## <a name="range"></a>Aralık

Belirtilen bir tamsayı başlayan tamsayı dizisi döndürür.

```
range(<startIndex>, <count>)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*startIndex*> | Evet | Tamsayı | Dizinin ilk öğesi olarak başlatır tamsayı değeri | 
| <*Sayısı*> | Evet | Tamsayı | Dizideki tamsayılar sayısı | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| [<*aralığı sonuç*>] | Dizi | Belirtilen dizinden başlayarak tamsayılar diziyle |  
|||| 

*Örnek*

Bu örnek, belirtilen dizinden başlayan ve belirtilen tamsayılar sayısı bir tamsayı dizisi oluşturur:

```
range(1, 4)
```

Ve bu sonucu döndürür: `[1, 2, 3, 4]`

<a name="replace"></a>

## <a name="replace"></a>Değiştir

Bir alt dizenin belirtilen dizesi ile değiştirin ve sonucu dize döndürür. Bu işlev büyük/küçük harf duyarlıdır.

```
replace('<text>', '<oldText>', '<newText>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Metin*> | Evet | Dize | Değiştirileceği alt dizeyi içeren dizeyi | 
| <*YeniMetin*> | Evet | Dize | Değiştirileceği alt dizeyi | 
| <*newText*> | Evet | Dize | Değiştirme dizesi | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*Güncelleştirilen metin*> | Dize | Alt dizeyi değiştirildikten sonra güncelleştirilmiş dize <p>Alt dizeyi bulunmazsa, özgün dize döndürür. | 
|||| 

*Örnek* 

Bu örnekte, "eski dizesinde" "eski" alt dizeyi bulur ve "eski"Yeni"ile" değiştirir: 

```
replace('the old string', 'old', 'new')
```

Ve bu sonucu döndürür: `"the new string"`

<a name="removeProperty"></a>

## <a name="removeproperty"></a>removeProperty

Bir nesneden bir özelliğini kaldırın ve güncelleştirilmiş nesneyi döndürür.

```
removeProperty(<object>, '<property>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*object*> | Evet | Nesne | Bir özelliği kaldırmak istediğiniz JSON nesnesi | 
| <*Özelliği*> | Evet | Dize | Kaldırmak özellik için ad | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*güncelleştirilmiş nesnesi*> | Nesne | Belirtilen özellik olmadan güncelleştirilmiş JSON nesnesi | 
|||| 

*Örnek*

Bu örnek kaldırır `"accountLocation"` özelliğinden bir `"customerProfile"` JSON ile dönüştürülür nesne [JSON()](#json) işlev ve güncelleştirilmiş nesneyi döndürür:

```
removeProperty(json('customerProfile'), 'accountLocation')
```

<a name="setProperty"></a>

## <a name="setproperty"></a>setProperty

Bir nesnenin özellik değerini ayarlayın ve güncelleştirilmiş nesneyi döndürür. Yeni bir özellik eklemek için bu işlevi kullanabilirsiniz veya [addProperty()](#addProperty) işlevi.

```
setProperty(<object>, '<property>', <value>)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*object*> | Evet | Nesne | Özellik ayarlamak istediğiniz JSON nesnesi | 
| <*Özelliği*> | Evet | Dize | Ayarlamak için mevcut veya yeni özellik adı | 
| <*Değer*> | Evet | Herhangi biri | Belirtilen özellik için ayarlanacak değer |
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*güncelleştirilmiş nesnesi*> | Nesne | Özelliği, ayarladığınız güncelleştirilmiş JSON nesnesi | 
|||| 

*Örnek*

Bu örnek ayarlar `"accountNumber"` özelliği bir `"customerProfile"` JSON ile dönüştürülür nesne [JSON()](#json) işlevi. İşlev tarafından oluşturulan değer atayan [guid()](#guid) işlev ve güncelleştirilmiş JSON nesnesi döndürür:

```
setProperty(json('customerProfile'), 'accountNumber', guid())
```

<a name="skip"></a>

## <a name="skip"></a>Atla

Bir koleksiyon Önden öğeleri kaldırmak ve geri dönüp *diğer tüm* öğeleri.

```
skip([<collection>], <count>)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Koleksiyonu*> | Evet | Dizi | Kaldırmak istediğiniz öğeleri koleksiyonu | 
| <*Sayısı*> | Evet | Tamsayı | Öne kaldırılacak öğe sayısı için pozitif bir tamsayı | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| [<*güncelleştirilmiş koleksiyonu*>] | Dizi | Belirtilen öğeleri kaldırdıktan sonra güncelleştirilmiş koleksiyonu | 
|||| 

*Örnek*

Bu örnek, bir öğe, belirtilen dizi önüne 0, numarasından kaldırır: 

```
skip([0, 1, 2, 3], 1)
```

Ve diğer öğelerle bu bir dizi döndürür: `[1,2,3]`

<a name="split"></a>

## <a name="split"></a>split

Bir dizeden tüm karakterler ve her bir karakteri sahip bir dizi ayırarak return bir *sınırlayıcı*.

```
split('<text>', '<separator>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Metin*> | Evet | Dize | Bölme karakterden dize |  
| <*ayırıcı*> | Evet | Dize | Sonuçta elde edilen dizisindeki her karakter arasında görünen ayırıcıyı | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| [<*char1*><*ayırıcı*><*char2*><*ayırıcı*>...] | Dizi | Belirtilen dizeyi gösteren tüm öğeleri oluşturulan Sonuç dizisi |
|||| 

*Örnek* 

Bu örnek, her bir karakteri ayırıcı olarak virgül ile ayırarak belirtilen dizesinden bir dizi oluşturur:

```
split('abc', ',')
```

Ve bu sonucu döndürür: `[a, b, c]`

<a name="startOfDay"></a>

## <a name="startofday"></a>startOfDay

Bir zaman damgası için günün başlangıcını döndür. 

```
startOfDay('<timestamp>', '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*zaman damgası*> | Evet | Dize | Zaman damgası içeren dize | 
| <*Biçimi*> | Hayır | Dize | Ya da bir [tek biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim deseni](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Zaman damgası için varsayılan biçimi olan ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-aa-ddT:mm:ss:fffffffK), ile uyumlu [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ve saat dilimi bilgilerini korur. |
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*güncelleştirilmiş zaman damgası*> | Dize | Belirtilen zaman damgası ancak günü için sıfır saatlik işareti başlayarak | 
|||| 

*Örnek* 

Bu örnekte, bu zaman damgası için günün başlangıcını bulur:

```
startOfDay('2018-03-15T13:30:30Z')
```

Ve bu sonucu döndürür: `"2018-03-15T00:00:00.0000000Z"`

<a name="startOfHour"></a>

## <a name="startofhour"></a>startOfHour

Bir zaman damgası için saatin başlangıcını döndür. 

```
startOfHour('<timestamp>', '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*zaman damgası*> | Evet | Dize | Zaman damgası içeren dize | 
| <*Biçimi*> | Hayır | Dize | Ya da bir [tek biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim deseni](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Zaman damgası için varsayılan biçimi olan ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-aa-ddT:mm:ss:fffffffK), ile uyumlu [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ve saat dilimi bilgilerini korur. |
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*güncelleştirilmiş zaman damgası*> | Dize | Belirtilen zaman damgası ancak saat sıfır dakikalık işareti başlayarak | 
|||| 

*Örnek* 

Bu örnekte, bu zaman damgası için saatin başlangıcını bulur:

```
startOfHour('2018-03-15T13:30:30Z')
```

Ve bu sonucu döndürür: `"2018-03-15T13:00:00.0000000Z"`

<a name="startOfMonth"></a>

## <a name="startofmonth"></a>startOfMonth

Bir zaman damgası için ayın başlangıcını döndürür. 

```
startOfMonth('<timestamp>', '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*zaman damgası*> | Evet | Dize | Zaman damgası içeren dize | 
| <*Biçimi*> | Hayır | Dize | Ya da bir [tek biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim deseni](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Zaman damgası için varsayılan biçimi olan ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-aa-ddT:mm:ss:fffffffK), ile uyumlu [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ve saat dilimi bilgilerini korur. |
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*güncelleştirilmiş zaman damgası*> | Dize | Belirtilen zaman damgası ancak sıfır saatlik işaretten ayın ilk günü başlayan | 
|||| 

*Örnek* 

Bu örnekte, bu zaman damgası için ayın başlangıcını döndürür:

```
startOfMonth('2018-03-15T13:30:30Z')
```

Ve bu sonucu döndürür: `"2018-03-01T00:00:00.0000000Z"`

<a name="startswith"></a>

## <a name="startswith"></a>startsWith

Bir dizeyi belirli bir alt dizesi ile başlayıp başlamadığını denetleyin. Alt dizeyi bulunduğunda true döndürür veya return false olduğunda bulunamadı. Bu işlev büyük küçük harfe duyarlı değildir.

```
startsWith('<text>', '<searchText>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Metin*> | Evet | Dize | Denetlenecek dize | 
| <*Aramametni*> | Evet | Dize | Bulmak için başlangıç dizesi | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| TRUE veya false  | Boole | Başlangıç substring bulunduğunda true döndürür. Ne zaman bulunamadı false döndürür. | 
|||| 

*Örnek 1* 

Bu örnekte, "hello world" dize "hello" alt dizeyi ile başlayıp başlamadığını denetler:

```
startsWith('hello world', 'hello')
```

Ve bu sonucu döndürür: `true`

*Örnek 2*

Bu örnekte, "hello world" dize "Tebrikler" alt dizeyi ile başlayıp başlamadığını denetler:

```
startsWith('hello world', 'greetings')
```

Ve bu sonucu döndürür: `false`

<a name="string"></a>

## <a name="string"></a>string

Dize sürümü için bir değer döndürür.

```
string(<value>)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Değer*> | Evet | Herhangi biri | Dönüştürülecek değer | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*dize değeri*> | Dize | Belirtilen değer dizesi sürümü | 
|||| 

*Örnek 1* 

Bu örnek, bu sayı dize sürümü oluşturur:

```
string(10)
```

Ve bu sonucu döndürür: `"10"`

*Örnek 2*

Bu örnek belirtilen JSON nesnesinin bir dize oluşturur ve ters eğik çizgi karakteri kullanır (\\) çift tırnak işareti (") için çıkış karakteri olarak.

```
string( { "name": "Sophie Owen" } )
```

Ve bu sonucu döndürür: `"{ \\"name\\": \\"Sophie Owen\\" }"`

<a name="sub"></a>

## <a name="sub"></a>Sub

İkinci sayı ilk sayıdan çıkarılmasıyla sonucu döndürür.

```
sub(<minuend>, <subtrahend>)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*minuend*> | Evet | Tamsayı veya kayan nokta | Çıkartılacak sayı *subtrahend* | 
| <*subtrahend*> | Evet | Tamsayı veya kayan nokta | Den çıkartılacak sayı *minuend* | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*Sonuç*> | Tamsayı veya kayan nokta | İkinci sayı ilk sayıdan çıkarılmasıyla gelen sonucu | 
|||| 

*Örnek* 

Bu örnekte, ikinci sayı ilk sayıdan çıkarır:

```
sub(10.3, .3)
```

Ve bu sonucu döndürür: `10`

<a name="substring"></a>

## <a name="substring"></a>substring

Bir dizeden belirtilen konuma veya dizin başlangıç karakteri döndürür. Sayı 0 ile değerleri başlangıç dizini. 

```
substring('<text>', <startIndex>, <length>)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Metin*> | Evet | Dize | Karakter istediğiniz dize | 
| <*startIndex*> | Evet | Tamsayı | Başlangıç konumu veya dizin değeri pozitif bir sayı | 
| <*uzunluğu*> | Evet | Tamsayı | Substring istediğiniz karakterlerin pozitif bir sayı | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*substring sonucu*> | Dize | Belirtilen sayıda karakteri, kaynak dizedeki belirtilen dizinden başlayarak bir dizeyle | 
|||| 

*Örnek* 

Bu örnek, başlangıç dizini değeri 6 belirtilen dizesinden beş karakterli substring oluşturur:

```
substring('hello world', 6, 5)
```

Ve bu sonucu döndürür: `"world"`

<a name="subtractFromTime"></a>

## <a name="subtractfromtime"></a>subtractFromTime

Bir zaman damgası zaman birimlerinden sayısı çıkarın. Ayrıca bkz. [getPastTime](#getPastTime).

```
subtractFromTime('<timestamp>', <interval>, '<timeUnit>', '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*zaman damgası*> | Evet | Dize | Zaman damgası içeren dize | 
| <*aralığı*> | Evet | Tamsayı | Çıkarmak için belirtilen süre birimlerinin sayısı | 
| <*timeUnit*> | Evet | Dize | İle kullanılacak zaman birimini *aralığı*: "Saniye", "Dakika", "Saat", "Gün", "Hafta", "Ay", "Yıl" | 
| <*Biçimi*> | Hayır | Dize | Ya da bir [tek biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim deseni](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Zaman damgası için varsayılan biçimi olan ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-aa-ddT:mm:ss:fffffffK), ile uyumlu [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ve saat dilimi bilgilerini korur. | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*güncelleştirilmiş zaman damgası*> | Dize | Belirtilen sayıda zaman birimleri eksi zaman damgası | 
|||| 

*Örnek 1*

Bu örnek bir günden bu zaman damgası çıkarır:

```
subtractFromTime('2018-01-02T00:00:00Z', 1, 'Day') 
```

Ve bu sonucu döndürür: `"2018-01-01T00:00:00:0000000Z"`

*Örnek 2*

Bu örnek bir günden bu zaman damgası çıkarır:

```
subtractFromTime('2018-01-02T00:00:00Z', 1, 'Day', 'D') 
```

Ve isteğe bağlı "D" biçimini kullanarak bu sonucunu döndürür: `"Monday, January, 1, 2018"`

<a name="take"></a>

## <a name="take"></a>Al

Öğeleri bir koleksiyon Önden döndür. 

```
take('<collection>', <count>)
take([<collection>], <count>)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Koleksiyonu*> | Evet | Dize veya dizi | İstediğiniz öğeleri koleksiyonu | 
| <*Sayısı*> | Evet | Tamsayı | Önden istediğiniz öğe sayısı için pozitif bir tamsayı | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*alt*> veya [<*alt*>] | Dize veya dizi, sırasıyla | Belirtilen sayıda öğeyi özgün koleksiyonunu Önden gerçekleştirilecek olan bir dize veya | 
|||| 

*Örnek*

Bu örnekler belirtilen sayıda öğeyi bu koleksiyonları Önden alın:

```
take('abcde`, 3)
take([0, 1, 2, 3, 4], 3)
```

Ve bu sonuçları döndürür:

* İlk örnek: `"abc"`
* İkinci örnek: `[0, 1, 2]`

<a name="ticks"></a>

## <a name="ticks"></a>işaretleri

Dönüş `ticks` belirtilen bir zaman damgası için özellik değeri. A *onay* 100 nanosaniyelik aralık.

```
ticks('<timestamp>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*zaman damgası*> | Evet | Dize | Bir zaman damgası dizesi | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*Ticks numarası*> | Tamsayı | Belirtilen zaman damgası itibaren sayısı | 
|||| 

<a name="toLower"></a>

## <a name="tolower"></a>toLower

Küçük harf biçiminde bir dize döndürür. Bir karakter dizesini küçük sürümü yoksa, bu karakteri döndürülen dize değişmeden kalır.

```
toLower('<text>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Metin*> | Evet | Dize | Küçük harf biçiminde döndürmek için dize | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*küçük metin*> | Dize | Özgün biçiminde dize olarak küçük harf | 
|||| 

*Örnek* 

Bu örnekte bu dizeyi küçük harfe dönüştürür: 

```
toLower('Hello World')
```

Ve bu sonucu döndürür: `"hello world"`

<a name="toUpper"></a>

## <a name="toupper"></a>toUpper

Büyük harf biçiminde bir dize döndürür. Dizedeki karakter büyük bir sürümü yüklü değilse bu karakteri döndürülen dize değişmeden kalır.

```
toUpper('<text>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Metin*> | Evet | Dize | Büyük harfli biçime geri dönmek için dize | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*büyük harf metin*> | Dize | Özgün biçiminde dize olarak büyük harf | 
|||| 

*Örnek* 

Bu örnekte bu dizenin büyük harfe dönüştürür:

```
toUpper('Hello World')
```

Ve bu sonucu döndürür: `"HELLO WORLD"`

<a name="trigger"></a>

## <a name="trigger"></a>Tetikleyici

Çalışma zamanı veya değerleri tetikleyici ait çıktı bir ifadeyi atayabilirsiniz diğer JSON ad ve değer çiftleri döndürmek. 

* İçinde bir tetikleyicinin girişleri, bu işlev önceki yürütülmesinden çıktıyı döndürür. 

* Bir tetikleyici koşul içinde bu işlev geçerli yürütülmesinden çıktıyı döndürür. 

Varsayılan olarak, tüm tetikleyicisi nesnesini işlevi başvuruyor, ancak isteğe bağlı olarak bir özellik belirtebilirsiniz istediğiniz değeri. Ayrıca, bu işlev kullanılabilir toplu sürümlerini sahiptir, bkz: [triggerOutputs()](#triggerOutputs) ve [triggerBody()](#triggerBody). 

```
trigger()
```

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*Tetikleyici çıktı*> | Dize | Çalışma zamanında bir tetikleyici çıktısı | 
|||| 

<a name="triggerBody"></a>

## <a name="triggerbody"></a>triggerBody

Bir tetikleyici dönmek `body` çalışma zamanında çıktı. İçin toplu özelliktir `trigger().outputs.body`. Bkz: [trigger()](#trigger). 

```
triggerBody()
```

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*Tetikleyici gövde çıktı*> | Dize | `body` Tetikleyiciyle çıktı | 
|||| 

<a name="triggerFormDataMultiValues"></a>

## <a name="triggerformdatamultivalues"></a>triggerFormDataMultiValues

Bir tetikleyici bir anahtar adına uyan değerleri içeren bir dizi döndürecek *form verilerini* veya *form kodlu* çıktı. 

```
triggerFormDataMultiValues('<key>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Anahtarı*> | Evet | Dize | Değer istediğiniz anahtar için ad | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| [<*dizisi ile anahtar değerleri*>] | Dizi | Belirtilen anahtarı uyan tüm değerleri içeren bir dizi | 
|||| 

*Örnek* 

Bu örnek, bir RSS tetikleyicinin form verilerini ya da form kodlu çıkış "feedUrl" anahtar değerinden bir dizi oluşturur: 

```
triggerFormDataMultiValues('feedUrl')
```

Ve örnek sonuç olarak bu dizisi döndürür: `["http://feeds.reuters.com/reuters/topNews"]`

<a name="triggerFormDataValue"></a>

## <a name="triggerformdatavalue"></a>triggerFormDataValue

Bir tetikleyici bir anahtar adı ile eşleşen tek bir değer içeren bir dize döndürecek *form verilerini* veya *form kodlu* çıktı. İşlev işlevi birden fazla eşleşme bulursa bir hata oluşturur.

```
triggerFormDataValue('<key>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Anahtarı*> | Evet | Dize | Değer istediğiniz anahtar için ad |
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*anahtar-değer*> | Dize | Belirtilen anahtar değeri | 
|||| 

*Örnek* 

Bu örnek bir RSS tetikleyicinin form verilerini ya da form kodlu çıkış "feedUrl" anahtar değeri bir dize oluşturur:

```
triggerFormDataValue('feedUrl')
```

Ve örnek sonuç olarak bu dize döndürür: `"http://feeds.reuters.com/reuters/topNews"` 

<a name="triggerMultipartBody"></a>

Gövdesi belirli bir bölümü için birden çok bölümü olan bir tetikleyicinin çıkış döndür. 

```
triggerMultipartBody(<index>)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Dizin*> | Evet | Tamsayı | İstediğiniz bölümü için dizin değeri |
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*Gövde*> | Dize | Belirtilen bir tetikleyicinin çok bölümlü çıktı bölümünde gövdesi | 
|||| 

<a name="triggerOutputs"></a>

## <a name="triggeroutputs"></a>triggerOutputs

Çalışma zamanı veya değerleri bir tetikleyicinin çıkış diğer JSON ad/değer çiftlerinden döndür. İçin toplu özelliktir `trigger().outputs`. Bkz: [trigger()](#trigger). 

```
triggerOutputs()
```

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*Tetikleyici çıktı*> | Dize | Çalışma zamanında bir tetikleyici çıktısı  | 
|||| 

<a name="trim"></a>

## <a name="trim"></a>Kırpma

Bir dizeden öndeki ve sondaki boşlukları kaldırın ve güncelleştirilmiş dize döndürür.

```
trim('<text>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Metin*> | Evet | Dize | Kaldırmak için öndeki ve sondaki boşlukları sahip dize | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*updatedText*> | Dize | Güncelleştirilmiş sürümü için özgün dizeye başında veya sonunda boşluk olmadan | 
|||| 

*Örnek* 

Bu örnekte, "Hello World" dizesi öndeki ve sondaki boşlukları kaldırır:  

```
trim(' Hello World  ')
```

Ve bu sonucu döndürür: `"Hello World"`

<a name="union"></a>

## <a name="union"></a>birleşim

Sahip bir koleksiyonun dönmek *tüm* belirtilen koleksiyonlarla öğelerinden. Sonuçta görünmesini için bir öğe bu işleve herhangi bir koleksiyon içinde yer alabilir. Bir veya daha fazla öğe aynı ada sahipse, bu adı taşıyan son öğe sonucunda görünür. 

```
union('<collection1>', '<collection2>', ...)
union([<collection1>], [<collection2>], ...)
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Collection1*>, <*collection2*>,...  | Evet | Dizi veya nesne, ancak ikisini | Burada istediğiniz koleksiyonları *tüm* öğeleri | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*updatedCollection*> | Dizi veya nesne, sırasıyla | Belirtilen koleksiyonlardan - yinelenen tüm öğeleri içeren bir koleksiyon | 
|||| 

*Örnek* 

Bu örnek alır *tüm* bu koleksiyonları öğelerinden: 

```
union([1, 2, 3], [1, 2, 10, 101])
```

Ve bu sonucu döndürür: `[1, 2, 3, 10, 101]`

<a name="uriComponent"></a>

## <a name="uricomponent"></a>uriComponent

Kaçış karakterli URL güvenli olmayan karakterleri değiştirerek bir dize için bir Tekdüzen Kaynak Tanımlayıcısı (URI) kodlu sürümü döndürür. Bu işlevi kullanmak yerine [encodeUriComponent()](#encodeUriComponent). Her iki işlevleri aynı şekilde çalışır ancak `uriComponent()` tercih edilir.

```
uriComponent('<value>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Değer*> | Evet | Dize | Dize URI kodlanmış biçimine dönüştürmek için | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*Kodlanmış URI*> | Dize | Kaçış karakterli URI ile kodlanmış dize | 
|||| 

*Örnek*

Bu örnek, bu dize için bir URI kodlu sürümü oluşturur:

```
uriComponent('https://contoso.com')
```

Ve bu sonucu döndürür: `"http%3A%2F%2Fcontoso.com"`

<a name="uriComponentToBinary"></a>

## <a name="uricomponenttobinary"></a>uriComponentToBinary

Bir Tekdüzen Kaynak Tanımlayıcısı (URI) bileşeni için ikili dosya sürümü döndürür.

```
uriComponentToBinary('<value>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Değer*> | Evet | Dize | Dönüştürmek için URI ile kodlanmış dize | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*ikili için-kodlanmış-URI'sı*> | Dize | URI kodlu dize ikili dosya sürümü. İkili içeriği base64 ile kodlanmış ve tarafından gösterilen `$content`. | 
|||| 

*Örnek*

Bu örnek, bu URI kodlu dize ikili sürümü oluşturur: 

```
uriComponentToBinary('http%3A%2F%2Fcontoso.com')
```

Ve bu sonucu döndürür: 

`"001000100110100001110100011101000111000000100101001100
11010000010010010100110010010001100010010100110010010001
10011000110110111101101110011101000110111101110011011011
110010111001100011011011110110110100100010"`

<a name="uriComponentToString"></a>

## <a name="uricomponenttostring"></a>uriComponentToString

Tekdüzen Kaynak Tanımlayıcısı (URI) dize sürümü kodlanmış etkili bir şekilde URI ile kodlanmış dize kod çözme dizesi döndürür.

```
uriComponentToString('<value>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Değer*> | Evet | Dize | Kod çözme için URI ile kodlanmış dize | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*kodunu çözdü URI*> | Dize | URI ile kodlanmış dize kodu çözülmüş sürümü | 
|||| 

*Örnek*

Bu örnek kodu çözülmüş dize sürümü bu URI kodlanmış bir dize oluşturur: 

```
uriComponentToString('http%3A%2F%2Fcontoso.com')
```

Ve bu sonucu döndürür: `"https://contoso.com"` 

<a name="uriHost"></a>

## <a name="urihost"></a>uriHost

Dönüş `host` Tekdüzen Kaynak Tanımlayıcısı (URI) değeri.

```
uriHost('<uri>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*URI*> | Evet | Dize | URI, `host` istediğiniz değer | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*ana bilgisayar değeri*> | Dize | `host` Belirtilen URI değeri | 
|||| 

*Örnek*

Bu örnek bulur `host` bu URI değeri: 

```
uriHost('https://www.localhost.com:8080')
```

Ve bu sonucu döndürür: `"www.localhost.com"`

<a name="uriPath"></a>

## <a name="uripath"></a>uriPath

Dönüş `path` Tekdüzen Kaynak Tanımlayıcısı (URI) değeri. 

```
uriPath('<uri>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*URI*> | Evet | Dize | URI, `path` istediğiniz değer | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*yol değeri*> | Dize | `path` Belirtilen URI değeri. Varsa `path` bir değere sahip değil, "/" karakteri döndürür. | 
|||| 

*Örnek*

Bu örnek bulur `path` bu URI değeri: 

```
uriPath('http://www.contoso.com/catalog/shownew.htm?date=today')
```

Ve bu sonucu döndürür: `"/catalog/shownew.htm"`

<a name="uriPathAndQuery"></a>

## <a name="uripathandquery"></a>uriPathAndQuery

Dönüş `path` ve `query` Tekdüzen Kaynak Tanımlayıcısı (URI) değerleri.

```
uriPathAndQuery('<uri>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*URI*> | Evet | Dize | URI, `path` ve `query` istediğiniz değerleri | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*yol sorgu değeri*> | Dize | `path` Ve `query` değerleri belirtilen URI. Varsa `path` olmayan bir değer belirtin, "/" karakteri döndürür. | 
|||| 

*Örnek*

Bu örnek bulur `path` ve `query` bu URI için değerler:

```
uriPathAndQuery('http://www.contoso.com/catalog/shownew.htm?date=today')
```

Ve bu sonucu döndürür: `"/catalog/shownew.htm?date=today"`

<a name="uriPort"></a>

## <a name="uriport"></a>uriPort

Dönüş `port` Tekdüzen Kaynak Tanımlayıcısı (URI) değeri.

```
uriPort('<uri>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*URI*> | Evet | Dize | URI, `port` istediğiniz değer | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*bağlantı noktası değeri*> | Tamsayı | `port` Belirtilen URI değeri. Varsa `port` olmayan bir değer belirtin, protokolü için varsayılan bağlantı noktası dönün. | 
|||| 

*Örnek*

Bu örnek döndürür `port` bu URI değeri:

```
uriPort('http://www.localhost:8080')
```

Ve bu sonucu döndürür: `8080`

<a name="uriQuery"></a>

## <a name="uriquery"></a>uriQuery

Dönüş `query` Tekdüzen Kaynak Tanımlayıcısı (URI) değeri.

```
uriQuery('<uri>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*URI*> | Evet | Dize | URI, `query` istediğiniz değer | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*Sorgu değeri*> | Dize | `query` Belirtilen URI değeri | 
|||| 

*Örnek*

Bu örnek döndürür `query` bu URI değeri: 

```
uriQuery('http://www.contoso.com/catalog/shownew.htm?date=today')
```

Ve bu sonucu döndürür: `"?date=today"`

<a name="uriScheme"></a>

## <a name="urischeme"></a>UriScheme

Dönüş `scheme` Tekdüzen Kaynak Tanımlayıcısı (URI) değeri.

```
uriScheme('<uri>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*URI*> | Evet | Dize | URI, `scheme` istediğiniz değer | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*düzeni değeri*> | Dize | `scheme` Belirtilen URI değeri | 
|||| 

*Örnek*

Bu örnek döndürür `scheme` bu URI değeri:

```
uriScheme('http://www.contoso.com/catalog/shownew.htm?date=today')
```

Ve bu sonucu döndürür: `"http"`

<a name="utcNow"></a>

## <a name="utcnow"></a>utcNow

Geçerli zaman damgası döndür. 

```
utcNow('<format>')
```

İsteğe bağlı olarak, farklı bir biçime sahip belirtebilirsiniz <*biçimi*> parametre. 


| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Biçimi*> | Hayır | Dize | Ya da bir [tek biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim deseni](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Zaman damgası için varsayılan biçimi olan ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-aa-ddT:mm:ss:fffffffK), ile uyumlu [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ve saat dilimi bilgilerini korur. | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*Geçerli zaman damgası*> | Dize | Geçerli tarih ve saat | 
|||| 

*Örnek 1*

Bugün 15 Nisan 2018 1:00: 00'da olduğunu varsayın. Bu örnek, geçerli zaman damgası alır: 

```
utcNow()
```

Ve bu sonucu döndürür: `"2018-04-15T13:00:00.0000000Z"`

*Örnek 2*

Bugün 15 Nisan 2018 1:00: 00'da olduğunu varsayın. Bu örnek isteğe bağlı "D" biçimini kullanarak geçerli zaman damgası alır:

```
utcNow('D')
```

Ve bu sonucu döndürür: `"Sunday, April 15, 2018"`

<a name="variables"></a>

## <a name="variables"></a>değişkenleri

Belirtilen bir değişken değeri döndürür. 

```
variables('<variableName>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*variableName*> | Evet | Dize | Değer istediğiniz değişkeni adı | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*değişken değeri*> | Herhangi biri | Belirtilen değişken için değeri | 
|||| 

*Örnek*

20 "numItems" değişkeni için geçerli değer olduğunu varsayın. Bu örnekte, bu değişken için tamsayı değeri alır:

```
variables('numItems')
```

Ve bu sonucu döndürür: `20`

<a name="workflow"></a>

## <a name="workflow"></a>iş akışı

İş akışı ile ilgili tüm ayrıntıları çalışma zamanı sırasında döndür. 

```
workflow().<property>
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Özelliği*> | Hayır | Dize | Değer istediğiniz iş akışı özelliği adı <p>Bir iş akışı nesnesi şu özelliklere sahiptir: **adı**, **türü**, **kimliği**, **konumu**, ve **çalıştırmak**. **Çalıştırmak** özellik değeri olduğundan Ayrıca bu özellikleri olan bir nesneyi: **adı**, **türü**, ve **kimliği**. | 
||||| 

*Örnek*

Bu örnek bir iş akışının geçerli çalışma adını döndürür:

```
workflow().run.name
```

<a name="xml"></a>

## <a name="xml"></a>xml

XML sürümü için bir JSON nesnesi içeren bir dize döndürür. 

```
xml('<value>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*Değer*> | Evet | Dize | Dönüştürülecek JSON nesnesi içeren dize <p>JSON nesnesi, yalnızca bir kök özelliği olması gerekir. <br>Eğik çizgi karakterini kullanın (\\) çift tırnak işareti (") için çıkış karakteri olarak. | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*XML sürümü*> | Nesne | Belirtilen dize veya JSON nesnesi kodlanmış XML | 
|||| 

*Örnek 1*

Bu örnek bir JSON nesnesi içeren bu dize, XML sürümü oluşturur: 

`xml( '{ \"name\": \"Sophia Owen\" }' )`

Ve bu sonuç XML döndürür: 

```xml
<name>Sophia Owen</name>
```

*Örnek 2*

Bu JSON nesnesinin olduğunu varsayalım:

```json
{ 
  "person": { 
    "name": "Sophia Owen", 
    "city": "Seattle" 
  } 
}
```

Bu örnek XML bu JSON nesnesi içeren bir dize oluşturur:

`xml( '{ \"person\": { \"name\": \"Sophia Owen\", \"city\": \"Seattle\" } }' )`

Ve bu sonuç XML döndürür: 

```xml
<person>
  <name>Sophia Owen</name>
  <city>Seattle</city>
<person>
```

<a name="xpath"></a>

## <a name="xpath"></a>XPath

XML düğüm veya XPath (XML Path dili) ifadenin eşleşen değerleri olup olmadığını denetleyin ve eşleşen düğümleri veya değerleri döndürür. Bir XPath ifadesi ya da yalnızca "XPath", bir XML belge yapısı XML içeriğinde düğümleri veya işlem değerleri seçebilmeniz gezinmenize yardımcı olur.

```
xpath('<xml>', '<xpath>')
```

| Parametre | Gerekli | Tür | Açıklama | 
| --------- | -------- | ---- | ----------- | 
| <*XML*> | Evet | Herhangi biri | Düğümleri veya bir XPath ifadesi değeri eşleşen değerleri aramak için XML dizesi | 
| <*XPath*> | Evet | Herhangi biri | Eşleşen XML düğümlerini veya değerlerini bulmak için kullanılan XPath ifadesi | 
||||| 

| Dönüş değeri | Tür | Açıklama | 
| ------------ | ---- | ----------- | 
| <*XML düğümü*> | XML | Yalnızca tek bir düğüm belirtilen XPath ifadesi eşleştiğinde bir XML düğümü | 
| <*Değer*> | Herhangi biri | Yalnızca tek bir değer belirtilen XPath ifadesi eşleştiğinde bir XML düğümü değerinden | 
| [<*xml Düğüm1*>, <*xml Düğüm2*>,...] </br>-veya- </br>[<*value1*>, <*value2*>,...] | Dizi | Bir dizi XML düğümlerini veya belirtilen XPath ifadesi eşleşen değerleri | 
|||| 

*Örnek 1*

Bu örnek eşleşen düğümleri bulur `<name></name>` belirtilen bağımsız değişkenler düğümünde ve bu düğüm değerleri içeren bir dizi döndürür: 

`xpath(xml(parameters('items')), '/produce/item/name')`

Bağımsız değişkenleri şunlardır:

* Bu XML içeriyor "öğeler" dizesi:

  `"<?xml version="1.0"?> <produce> <item> <name>Gala</name> <type>apple</type> <count>20</count> </item> <item> <name>Honeycrisp</name> <type>apple</type> <count>10</count> </item> </produce>"`

  Örnek kullanır [parameters()](#parameters) XML dizesi "öğeler" bağımsız değişkenden almaya çalışır, ancak aynı zamanda dize XML biçimine kullanarak dönüştürmeniz gerekir [xml()](#xml) işlevi. 

* Bir dize olarak geçirilen bu XPath ifadesi:

  `"/produce/item/name"`

Eşleşen düğümleri sonuç diziyle işte `<name></name`:

`[ <name>Gala</name>, <name>Honeycrisp</name> ]`

*Örnek 2*

Aşağıdaki örnek 1'de, bu örnek eşleşen düğümleri bulur `<count></count>` düğümü ve bu düğüm değerleri ile ekler `sum()` işlevi:

`xpath(xml(parameters('items')), 'sum(/produce/item/count)')`

Ve bu sonucu döndürür: `30`

*Örnek 3*

Bu örnekte, her iki ifadeleri eşleşen düğümleri Bul `<location></location>` düğümünde XML içeren bir ad dahil belirtilen bağımsız. Ters eğik çizgi karakteri ifadeleri kullanma (\\) çift tırnak işareti (") için çıkış karakteri olarak.

* *İfade 1*

  `xpath(xml(body('Http')), '/*[name()=\"file\"]/*[name()=\"location\"]')`

* *İfade 2* 

  `xpath(xml(body('Http')), '/*[local-name=()=\"file\"] and namespace-uri()=\"http://contoso.com\"/*[local-name()]=\"location\" and namespace-uri()=\"\"]')`

Bağımsız değişkenleri şunlardır:

* XML belge ad alanı içerir, bu XML `xmlns="http://contoso.com"`: 

  ```xml
  <?xml version="1.0"?> <file xmlns="http://contoso.com"> <location>Paris</location> </file>
  ```

* Her iki XPath ifadesi burada:

  * `/*[name()=\"file\"]/*[name()=\"location\"]`

  * `/*[local-name=()=\"file\"] and namespace-uri()=\"http://contoso.com\"/*[local-name()]=\"location\" and namespace-uri()=\"\"]`

Eşleşen sonuç düğümü işte `<location></location` düğümü:

```xml
<location xmlns="https://contoso.com">Paris</location>
```

*Örnek 4*

Aşağıdaki örnek 3'te, bu örnek değeri bulur `<location></location>` düğümü: 

`xpath(xml(body('Http')), 'string(/*[name()=\"file\"]/*[name()=\"location\"])')`

Ve bu sonucu döndürür: `"Paris"`

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [iş akışı tanımlama dili](../logic-apps/logic-apps-workflow-definition-language.md)
