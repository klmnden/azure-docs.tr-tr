---
title: Video Indexer API'ları, bir dil modeli - Azure'ı özelleştirmek için kullanın
titlesuffix: Azure Media Services
description: Bu makalede, Video Indexer API'ları ile bir dil modelini özelleştirin gösterilmektedir.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.topic: article
ms.date: 02/10/2019
ms.author: anzaman
ms.openlocfilehash: ca1e66d20b19c1a5b85a4f4ff1c433331116bee7
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60002043"
---
# <a name="customize-a-language-model-with-the-video-indexer-apis"></a>Video Indexer API'ları ile bir dil modelini özelleştirin

Video Indexer, konuşma tanıma uyarlama metinle, yani sözlük uyum sağlamak üzere altyapısı istediğiniz etki alanından karşıya yükleyerek özelleştirmek için özel dil modelleri oluşturmanıza olanak sağlar. Modelinizi eğitin sonra yeni sözcük uyarlama metinli tanınır. 

Ayrıntılı bir genel ve özel dil modelleri için en iyi yöntemler için bkz: [Video Indexer ile bir dil modelini özelleştirin](customize-language-model-overview.md).

Bu konuda açıklandığı gibi oluşturmak ve özel dil modelleri, hesabınızdaki düzenlemek için Video Indexer API'ları kullanabilirsiniz. Web sitesinde açıklandığı gibi kullanabilirsiniz [özelleştirme dil modeli Video Indexer Web sitesini kullanarak](customize-language-model-with-api.md).

## <a name="create-a-language-model"></a>Dil modeli oluşturma

Aşağıdaki komut, belirtilen hesap yeni bir özel dil modeli oluşturur. Bu çağrı, dil modeli için dosyaları karşıya yükleyebilirsiniz. Alternatif olarak, dil modeli oluşturabilir ve dil modelini güncelleştirerek modeli için dosyaları daha sonra karşıya yükleyin.

> [!NOTE]
> Modeli, dosyaların içeriğini öğrenmek için etkin dosyalarıyla hala modeli eğitme gerekir. Bir dil eğitim şirket yönergeleri sonraki bölümde ' dir.

### <a name="request-url"></a>İstek URL'si

Bir POST isteği budur.

```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/PersonModels?name={name}&accessToken={accessToken}
```

Curl istekte aşağıda verilmiştir.

```curl
curl -v -X POST "https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Language?accessToken={accessToken}&modelName={modelName}&language={language}"

--data-ascii "{body}" 
```

[Gerekli Parametreler bakın ve test Video Indexer Geliştirici portalını kullanarak](https://api-portal.videoindexer.ai/docs/services/operations/operations/Create-Person-Model?).

### <a name="request-parameters"></a>İstek parametreleri

|**Ad**|**Tür**|**Gerekli**|**Açıklama**|
|---|---|---|---|
|location|string|Evet|Çağrı yönlendirileceğini Azure bölgesi. Daha fazla bilgi için [Azure bölgeleri ve Video Indexer](regions.md).|
|Hesap Kimliği|string|Evet|Hesap için genel benzersiz tanıtıcısı|
|accessToken|string|Evet|Erişim belirteci (kapsamı olmalıdır [hesap erişim belirteci](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) karşı çağrı kimliğini doğrulamak için. Erişim belirteci 1 saat içinde süresi dolar.|
|modelName|string|Evet|Dil modeli adı|
|language|string|Evet|Dil modeli dili. <br/>**Dil** parametresi verilen BCP 47 'dil etiketi-region' biçiminde dil gerekir (örn: 'tr-tr'). Desteklenen diller şunlardır İngilizce (en-US), Almanca (de-DE), İspanyolca (es-SP), Arapça (ar-ÖR), Fransızca (fr-FR), Hintçe (Merhaba-yüksek), İtalyanca (it-IT), Japonca (ja-JP), Portekizce (pt-BR), Rusça (ru-RU) ve Çince (zh-CN).  |

### <a name="request-body"></a>İstek gövdesi

Dil modeli eklenecek dosyaları karşıya yüklemek için yukarıdaki gerekli parametreleri için değerleri sağlayarak ek olarak form verileri kullanarak gövdesini dosyalarında yüklemeniz gerekir. Bunu yapmanın iki yolu vardır: 

1. Anahtar dosya adı olacaktır ve değer txt dosyası olacaktır
2. Anahtar dosya adı olacaktır ve değer txt dosyasının URL'si olacaktır

### <a name="response"></a>Yanıt

Yanıt, yeni oluşturulan bir dil modeli meta verileri her örnek JSON çıkış biçimi aşağıdaki modelin dosyalarının yanı sıra meta veri sağlar.

```json
{
    "id": "dfae5745-6f1d-4edd-b224-42e1ab57a891",
    "name": "TestModel",
    "language": "En-US",
    "state": "None",
    "languageModelId": "00000000-0000-0000-0000-000000000000",
    "files": [
    {
        "id": "25be7c0e-b6a6-4f48-b981-497e920a0bc9",
        "name": "hellofile",
        "enable": true,
        "creator": "John Doe",
        "creationTime": "2018-04-28T11:55:34.6733333"
    },
    {
        "id": "33025f5b-2354-485e-a50c-4e6b76345ca7",
        "name": "worldfile",
        "enable": true,
        "creator": "John Doe",
        "creationTime": "2018-04-28T11:55:34.86"
    }
    ]
}

```

## <a name="train-a-language-model"></a>Dil modeli eğitme

Aşağıdaki komut belirtilen hesapta özel dil modeli ile yüklenmiş ve dil modeli etkin dosyaların içeriğini eğitir. 

> [!NOTE]
> İlk, dil modeli oluşturun ve dosyalarını karşıya yükleme gerekir. Dil modeli oluştururken ya da dil modelini güncelleştirerek dosyaları karşıya yükleyebilirsiniz. 

### <a name="request-url"></a>İstek URL'si

Bir PUT İsteği budur.

```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Language/{modelId}/Train?accessToken={accessToken}
```

Curl istekte aşağıda verilmiştir.

```curl
curl -v -X PUT "https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Language/{modelId}/Train?accessToken={accessToken}"
```
 
[Gerekli Parametreler bakın ve test Video Indexer Geliştirici portalını kullanarak](https://api-portal.videoindexer.ai/docs/services/operations/operations/Train-Language-Model?&pattern=train).

### <a name="request-parameters"></a>İstek parametreleri

|**Ad**|**Tür**|**Gerekli**|**Açıklama**|
|---|---|---|---|
|location|string|Evet|Çağrı yönlendirileceğini Azure bölgesi. Daha fazla bilgi için [Azure bölgeleri ve Video Indexer](regions.md).|
|Hesap Kimliği|string|Evet|Hesap için genel benzersiz tanıtıcısı|
|ModelID|string|Evet|Dil modeli kimliği (dil modeli oluşturulduğunda oluşturulur)|
|accessToken|string|Evet|Erişim belirteci (kapsamı olmalıdır [hesap erişim belirteci](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) karşı çağrı kimliğini doğrulamak için. Erişim belirteci 1 saat içinde süresi dolar.|

### <a name="request-body"></a>İstek gövdesi

İstek gövdesi bu çağrı için gerekli başka yoktur.

### <a name="response"></a>Yanıt

Yanıtın meta veri meta verileri her örnek JSON çıkış biçimi aşağıdaki modelin dosyalarının yanı sıra yeni eğitilmiş dil modeli sağlar.

```json
{
    "id": "41464adf-e432-42b1-8e09-f52905d7e29d",
    "name": "TestModel",
    "language": "En-US",
    "state": "Waiting",
    "languageModelId": "531e5745-681d-4e1d-b124-12e5ab57a891",
    "files": [
    {
        "id": "84fcf1ac-1952-48f3-b372-18f768eedf83",
        "name": "RenamedFile",
        "enable": false,
        "creator": "John Doe",
        "creationTime": "2018-04-27T20:10:10.5233333"
    },
    {
        "id": "9ac35b4b-1381-49c4-9fe4-8234bfdd0f50",
        "name": "hellofile",
        "enable": true,
        "creator": "John Doe",
        "creationTime": "2018-04-27T20:10:10.68"
    }
    ]
}
```

Ardından kullanmalısınız **kimliği** için dil modeli değerini **linguisticModelId** parametre olduğunda [dizinine bir video karşıya](https://api-portal.videoindexer.ai/docs/services/operations/operations/Upload-video?) ve  **languageModelId** parametre olduğunda [video ölçeklemek](https://api-portal.videoindexer.ai/docs/services/operations/operations/Re-index-video?).

## <a name="delete-a-language-model"></a>Dil modeli Sil

Aşağıdaki komut, belirtilen hesaptan özel bir dil modeli siler. Videoyu yeniden dizine kadar silinen dil modelini kullanan herhangi bir video aynı dizin tutar. Videoyu yeniden dizine eklerseniz, yeni bir dil modeli videoyu atayabilirsiniz. Aksi takdirde, Video Indexer videoyu yeniden dizine kendi varsayılan modelini kullanır.

### <a name="request-url"></a>İstek URL'si

Bir silme isteği budur.

```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Language/{modelId}?accessToken={accessToken}
```

Curl istekte aşağıda verilmiştir.

```curl
curl -v -X DELETE "https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Language/{modelId}?accessToken={accessToken}"
```
 
[Gerekli Parametreler bakın ve test Video Indexer Geliştirici portalını kullanarak](https://api-portal.videoindexer.ai/docs/services/operations/operations/Delete-Language-Model?&pattern=delete).

### <a name="request-parameters"></a>İstek parametreleri 

|**Ad**|**Tür**|**Gerekli**|**Açıklama**|
|---|---|---|---|
|location|string|Evet|Çağrı yönlendirileceğini Azure bölgesi. Daha fazla bilgi için [Azure bölgeleri ve Video Indexer](regions.md).|
|Hesap Kimliği|string|Evet|Hesap için genel benzersiz tanıtıcısı|
|ModelID|string|Evet|Dil modeli kimliği (dil modeli oluşturulduğunda oluşturulur)|
|accessToken|string|Evet|Erişim belirteci (kapsamı olmalıdır [hesap erişim belirteci](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) karşı çağrı kimliğini doğrulamak için. Erişim belirteci 1 saat içinde süresi dolar.|

### <a name="request-body"></a>İstek gövdesi

İstek gövdesi bu çağrı için gerekli başka yoktur.

### <a name="response"></a>Yanıt

Dil modeli başarıyla silindiğinde döndürülen içerik yok.

## <a name="update-a-language-model"></a>Dil modeli güncelleştirme

Aşağıdaki komut belirtilen hesapta özel bir dil kişi modeli güncelleştirir.

> [!NOTE]
> Dil modeli oluşturmuş olmanız gerekir. Bu çağrı, etkinleştirme veya model altındaki tüm dosyaları devre dışı bırakmak, dil modeli adını güncelleştirin ve dil modeline eklenecek dosyaları karşıya yükleme için kullanabilirsiniz.

### <a name="request-url"></a>İstek URL'si

Bir PUT İsteği budur.

```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Language/{modelId}?accessToken={accessToken}[&modelName][&enable]
```

Curl istekte aşağıda verilmiştir.

```curl
curl -v -X PUT "https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Language/{modelId}?accessToken={accessToken}?modelName={string}&enable={string}"

--data-ascii "{body}" 
```
 
[Gerekli Parametreler bakın ve test Video Indexer Geliştirici portalını kullanarak](https://api-portal.videoindexer.ai/docs/services/operations/operations/Update-Language-Model?&pattern=update).

### <a name="request-parameters"></a>İstek parametreleri 

|**Ad**|**Tür**|**Gerekli**|**Açıklama**|
|---|---|---|---|
|location|string|Evet|Çağrı yönlendirileceğini Azure bölgesi. Daha fazla bilgi için [Azure bölgeleri ve Video Indexer](regions.md).|
|Hesap Kimliği|string|Evet|Hesap için genel benzersiz tanıtıcısı|
|ModelID|string|Evet|Dil modeli kimliği (dil modeli oluşturulduğunda oluşturulur)|
|accessToken|string|Evet|Erişim belirteci (kapsamı olmalıdır [hesap erişim belirteci](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) karşı çağrı kimliğini doğrulamak için. Erişim belirteci 1 saat içinde süresi dolar.|
|modelName|string|Hayır|Modele size yeni bir ad|
|etkinleştir|boolean|Hayır|Bu model altındaki tüm dosyaları olup olmadığını seçin (true) etkin veya devre dışı (false)|

### <a name="request-body"></a>İstek gövdesi

Dil modeli eklenecek dosyaları karşıya yüklemek için yukarıdaki gerekli parametreleri için değerleri sağlayarak ek olarak form verileri kullanarak gövdesini dosyalarında yüklemeniz gerekir. Bunu yapmanın iki yolu vardır: 

1. Anahtar dosya adı olacaktır ve değer txt dosyası olacaktır
2. Anahtar dosya adı olacaktır ve değer txt dosyasının URL'si olacaktır

### <a name="response"></a>Yanıt

Yanıtın meta veri meta verileri her örnek JSON çıkış biçimi aşağıdaki modelin dosyalarının yanı sıra yeni eğitilmiş dil modeli sağlar.

```json
{
    "id": "41464adf-e432-42b1-8e09-f52905d7e29d",
    "name": "TestModel",
    "language": "En-US",
    "state": "Waiting",
    "languageModelId": "531e5745-681d-4e1d-b124-12e5ab57a891",
    "files": [
    {
        "id": "84fcf1ac-1952-48f3-b372-18f768eedf83",
        "name": "RenamedFile",
        "enable": true,
        "creator": "John Doe",
        "creationTime": "2018-04-27T20:10:10.5233333"
    },
    {
        "id": "9ac35b4b-1381-49c4-9fe4-8234bfdd0f50",
        "name": "hellofile",
        "enable": true,
        "creator": "John Doe",
        "creationTime": "2018-04-27T20:10:10.68"
    }
    ]
}
```
Kullanabileceğiniz **kimliği** dosyaları döndürülen burada dosyasının içeriğini indirmek için.

## <a name="update-a-file-from-a-language-model"></a>Dil modeli dosyasından güncelleştir

Aşağıdaki komut, adı güncelleştirmek sağlar ve **etkinleştirme** özel bir dil modeli belirtilen hesapta dosyasında durumu.

### <a name="request-url"></a>İstek URL'si

Bir PUT İsteği budur.

```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Language/{modelId}/Files/{fileId}?accessToken={accessToken}[&fileName][&enable]
```

Curl istekte aşağıda verilmiştir.

```curl
curl -v -X PUT "https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Language/{modelId}/Files/{fileId}?accessToken={accessToken}?fileName={string}&enable={string}"
```
 
[Gerekli Parametreler bakın ve test Video Indexer Geliştirici portalını kullanarak](https://api-portal.videoindexer.ai/docs/services/operations/operations/Update-Language-Model-file?&pattern=update).

### <a name="request-parameters"></a>İstek parametreleri 

|**Ad**|**Tür**|**Gerekli**|**Açıklama**|
|---|---|---|---|
|location|string|Evet|Çağrı yönlendirileceğini Azure bölgesi. Daha fazla bilgi için [Azure bölgeleri ve Video Indexer](regions.md).|
|Hesap Kimliği|string|Evet|Hesap için genel benzersiz tanıtıcısı|
|ModelID|string|Evet|(Dil modeli oluştururken oluşturulan) dosyası tutar dil modeli kimliği|
|fileId|string|Evet|(Dosya karşıya yüklenen oluşturma sırasında veya dil modelinin güncelleştirme olduğunda oluşturulan) güncelleştirilmekte dosyanın kimliği|
|accessToken|string|Evet|Erişim belirteci (kapsamı olmalıdır [hesap erişim belirteci](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) karşı çağrı kimliğini doğrulamak için. Erişim belirteci 1 saat içinde süresi dolar.|
|fileName|string|Hayır|Dosya adına güncelleştirme adı|
|etkinleştir|boolean|Hayır|Bu dosya olup olmadığını güncelleştirin (true) etkin veya dil modeli (false) devre dışı|

### <a name="request-body"></a>İstek gövdesi

İstek gövdesi bu çağrı için gerekli başka yoktur.

### <a name="response"></a>Yanıt

Yanıt aşağıdaki örnek JSON çıkış biçimi güncelleştirilmiş dosya meta veri sağlar.

```json
{
  "id": "84fcf1ac-1952-48f3-b372-18f768eedf83",
  "name": "RenamedFile",
  "enable": false,
  "creator": "John Doe",
  "creationTime": "2018-04-27T20:10:10.5233333"
}
```
Kullanabileceğiniz **kimliği** dosyanın döndürülen burada dosyasının içeriğini indirmek için.

## <a name="get-a-specific-language-model"></a>Belirli bir dil modeli Al

Aşağıdaki komutu, dil ve dil modeli dosyaları gibi belirtilen hesapta belirtilen dil modeli hakkında bilgi döndürür. 

### <a name="request-url"></a>İstek URL'si

Bir GET isteği budur.
```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Language/{modelId}?accessToken={accessToken}
```

Curl istekte aşağıda verilmiştir.

```curl
curl -v -X GET "https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Language/{modelId}?accessToken={accessToken}"
```
 
[Gerekli Parametreler bakın ve test Video Indexer Geliştirici portalını kullanarak](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Language-Model?&pattern=get).

### <a name="request-parameters-and-request-body"></a>İstek parametreleri ve istek gövdesi

|**Ad**|**Tür**|**Gerekli**|**Açıklama**|
|---|---|---|---|
|location|string|Evet|Çağrı yönlendirileceğini Azure bölgesi. Daha fazla bilgi için [Azure bölgeleri ve Video Indexer](regions.md).|
|Hesap Kimliği|string|Evet|Hesap için genel benzersiz tanıtıcısı|
|ModelID|string|Evet|Dil modeli kimliği (dil modeli oluşturulduğunda oluşturulur)|
|accessToken|string|Evet|Erişim belirteci (kapsamı olmalıdır [hesap erişim belirteci](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) karşı çağrı kimliğini doğrulamak için. Erişim belirteci 1 saat içinde süresi dolar.|

### <a name="request-body"></a>İstek gövdesi

İstek gövdesi bu çağrı için gerekli başka yoktur.

### <a name="response"></a>Yanıt

Yanıt, belirtilen dil modeli meta verileri her aşağıdaki örnek JSON çıkış biçimi aşağıdaki modelin dosyalarının yanı sıra meta veriler sağlar.

```json
{
    "id": "dfae5745-6f1d-4edd-b224-42e1ab57a891",
    "name": "TestModel",
    "language": "En-US",
    "state": "None",
    "languageModelId": "00000000-0000-0000-0000-000000000000",
    "files": [
    {
        "id": "25be7c0e-b6a6-4f48-b981-497e920a0bc9",
        "name": "hellofile",
        "enable": true,
        "creator": "John Doe",
        "creationTime": "2018-04-28T11:55:34.6733333"
    },
    {
        "id": "33025f5b-2354-485e-a50c-4e6b76345ca7",
        "name": "worldfile",
        "enable": true,
        "creator": "John Doe",
        "creationTime": "2018-04-28T11:55:34.86"
    }
    ]
}
```

Kullanabileceğiniz **kimliği** dosyanın döndürülen burada dosyasının içeriğini indirmek için.

## <a name="get-all-the-language-models"></a>Tüm dil modelleri Al

Aşağıdaki komutu tüm özel dil modelleri belirtilen hesapta bir liste döndürür.

### <a name="request-url"></a>İstek URL'si

Bir alma isteği budur.

```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Language?accessToken={accessToken}
```

Curl istekte aşağıda verilmiştir.

```curl
curl -v -X GET "https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Language?accessToken={accessToken}"
```
 
[Gerekli Parametreler bakın ve test Video Indexer Geliştirici portalını kullanarak](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Language-Models?&pattern=get).

### <a name="request-parameters"></a>İstek parametreleri

|**Ad**|**Tür**|**Gerekli**|**Açıklama**|
|---|---|---|---|
|location|string|Evet|Çağrı yönlendirileceğini Azure bölgesi. Daha fazla bilgi için [Azure bölgeleri ve Video Indexer](regions.md).|
|Hesap Kimliği|string|Evet|Hesap için genel benzersiz tanıtıcısı|
|accessToken|string|Evet|Erişim belirteci (kapsamı olmalıdır [hesap erişim belirteci](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) karşı çağrı kimliğini doğrulamak için. Erişim belirteci 1 saat içinde süresi dolar.|

### <a name="request-body"></a>İstek gövdesi

İstek gövdesi bu çağrı için gerekli başka yoktur.

### <a name="response"></a>Yanıt

Tüm dil modellerinin hesabınızdaki ve her biri kendi meta verileri ve aşağıdaki örnek JSON çıkış biçimi aşağıdaki dosyaları listesini isteğin yanıtını verir.

```json
[
    {
        "id": "dfae5745-6f1d-4edd-b224-42e1ab57a891",
        "name": "TestModel",
        "language": "En-US",
        "state": "None",
        "languageModelId": "00000000-0000-0000-0000-000000000000",
        "files": [
        {
            "id": "25be7c0e-b6a6-4f48-b981-497e920a0bc9",
            "name": "hellofile",
            "enable": true,
            "creator": "John Doe",
            "creationTime": "2018-04-28T11:55:34.6733333"
        },
        {
            "id": "33025f5b-2354-485e-a50c-4e6b76345ca7",
            "name": "worldfile",
            "enable": true,
            "creator": "John Doe",
            "creationTime": "2018-04-28T11:55:34.86"
        }
        ]
    },
    {
        "id": "dfae5745-6f1d-4edd-b224-42e1ab57a892",
        "name": "AnotherTestModel",
        "language": "En-US",
        "state": "None",
        "languageModelId": "00000000-0000-0000-0000-000000000001",
        "files": []
    }
]
```

## <a name="delete-a-file-from-a-language-model"></a>Bir dosya bir dil modelden Sil

Aşağıdaki komut belirtilen hesapta belirtilen dil modeli belirtilen dosya siler. 

### <a name="request-url"></a>İstek URL'si

Bir silme isteği budur.
```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Language/{modelId}/Files/{fileId}?accessToken={accessToken}
```

Curl istekte aşağıda verilmiştir.

```curl
curl -v -X DELETE "https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Language/{modelId}/Files/{fileId}?accessToken={accessToken}"
```
 
[Gerekli Parametreler bakın ve test Video Indexer Geliştirici portalını kullanarak](https://api-portal.videoindexer.ai/docs/services/operations/operations/Delete-Language-Model-File?&pattern=delete).

### <a name="request-parameters"></a>İstek parametreleri 

|**Ad**|**Tür**|**Gerekli**|**Açıklama**|
|---|---|---|---|
|location|string|Evet|Çağrı yönlendirileceğini Azure bölgesi. Daha fazla bilgi için [Azure bölgeleri ve Video Indexer](regions.md).|
|Hesap Kimliği|string|Evet|Hesap için genel benzersiz tanıtıcısı|
|ModelID|string|Evet|(Dil modeli oluştururken oluşturulan) dosyası tutar dil modeli kimliği|
|fileId|string|Evet|(Dosya karşıya yüklenen oluşturma sırasında veya dil modelinin güncelleştirme olduğunda oluşturulan) güncelleştirilmekte dosyanın kimliği|
|accessToken|string|Evet|Erişim belirteci (kapsamı olmalıdır [hesap erişim belirteci](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) karşı çağrı kimliğini doğrulamak için. Erişim belirteci 1 saat içinde süresi dolar.|

### <a name="request-body"></a>İstek gövdesi

İstek gövdesi bu çağrı için gerekli başka yoktur.

### <a name="response"></a>Yanıt

Dosya dil modeli başarıyla silindiğinde döndürülen içerik yok.

## <a name="get-metadata-on-a-file-from-a-language-model"></a>Bir dil modeli dosya meta verilerini al

Bu meta verileri ve içeriğini belirtilen dosyaya seçtiğiniz dil modeli döndürür hesabınızı.

### <a name="request-url"></a>İstek URL'si

Bir GET isteği budur.

```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/PersonModels?accessToken={accessToken}
```

Curl istekte aşağıda verilmiştir.
```curl
curl -v -X GET "https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Language/{modelId}/Files/{fileId}?accessToken={accessToken}"
```
 
[Gerekli Parametreler bakın ve test Video Indexer Geliştirici portalını kullanarak](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Language-Model-File-Data?&pattern=get%20language%20model).

### <a name="request-parameters"></a>İstek parametreleri 

|**Ad**|**Tür**|**Gerekli**|**Açıklama**|
|---|---|---|---|
|location|string|Evet|Çağrı yönlendirileceğini Azure bölgesi. Daha fazla bilgi için [Azure bölgeleri ve Video Indexer](regions.md).|
|Hesap Kimliği|string|Evet|Hesap için genel benzersiz tanıtıcısı|
|ModelID|string|Evet|(Dil modeli oluştururken oluşturulan) dosyası tutar dil modeli kimliği|
|fileId|string|Evet|(Dosya karşıya yüklenen oluşturma sırasında veya dil modelinin güncelleştirme olduğunda oluşturulan) güncelleştirilmekte dosyanın kimliği|
|accessToken|string|Evet|Erişim belirteci (kapsamı olmalıdır [hesap erişim belirteci](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) karşı çağrı kimliğini doğrulamak için. Erişim belirteci 1 saat içinde süresi dolar.|

### <a name="request-body"></a>İstek gövdesi

İstek gövdesi bu çağrı için gerekli başka yoktur.

### <a name="response"></a>Yanıt

Yanıt içeriği ve JSON biçiminde, şuna benzer bir dosyanın meta verilerini sağlar:

```json
{
    "content": "hello\r\nworld",
    "id": "84fcf1ac-1952-48f3-b372-18f768eedf83",
    "name": "Hello",
    "enable": true,
    "creator": "John Doe",
    "creationTime": "2018-04-27T20:10:10.5233333"
}
```

> [!NOTE]
> Bu örnek dosyanın içeriğini sözcükler "hello" ile dünya iki ayrı satırlarda var."

## <a name="download-a-file-from-a-language-model"></a>Bir dil modeli dosya indirme

Aşağıdaki komut, belirtilen hesabın, belirtilen dil modeli belirtilen dosyanın içeriğini içeren bir metin dosyası indirir. Bu metin dosyasını başlangıçta karşıya yüklenen bir metin dosyasının içeriğini aynı olmalıdır.

### <a name="request-url"></a>İstek URL'si
```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Language/{modelId}/Files/{fileId}/download?accessToken={accessToken}
```

Curl istekte aşağıda verilmiştir.

```curl
curl -v -X GET "https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Language/{modelId}/Files/{fileId}/download?accessToken={accessToken}"
```
 
[Gerekli Parametreler bakın ve test Video Indexer Geliştirici portalını kullanarak](https://api-portal.videoindexer.ai/docs/services/operations/operations/Download-Language-Model-File-Content?).

### <a name="request-parameters"></a>İstek parametreleri 

|**Ad**|**Tür**|**Gerekli**|**Açıklama**|
|---|---|---|---|
|location|string|Evet|Çağrı yönlendirileceğini Azure bölgesi. Daha fazla bilgi için [Azure bölgeleri ve Video Indexer](regions.md).|
|Hesap Kimliği|string|Evet|Hesap için genel benzersiz tanıtıcısı|
|ModelID|string|Evet|(Dil modeli oluştururken oluşturulan) dosyası tutar dil modeli kimliği|
|fileId|string|Evet|(Dosya karşıya yüklenen oluşturma sırasında veya dil modelinin güncelleştirme olduğunda oluşturulan) güncelleştirilmekte dosyanın kimliği|
|accessToken|string|Evet|Erişim belirteci (kapsamı olmalıdır [hesap erişim belirteci](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) karşı çağrı kimliğini doğrulamak için. Erişim belirteci 1 saat içinde süresi dolar.|

### <a name="request-body"></a>İstek gövdesi 

İstek gövdesi bu çağrı için gerekli başka yoktur.

### <a name="response"></a>Yanıt

Yanıt, JSON biçiminde dosyasının içeriğini bir metin dosyası indirmeye olacaktır. 

## <a name="next-steps"></a>Sonraki adımlar

[Web sitesini kullanarak dil modelini özelleştirin](customize-language-model-with-website.md)
