---
title: Oluşturun, yayımlayın, yanıt
titleSuffix: QnA Maker - Azure Cognitive Services
description: Bu REST tabanlı öğretici, program aracılığıyla bilgi bankası oluşturup yayımlama ve bunu kullanarak soruları cevaplama adımlarını göstermektedir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: tutorial
ms.date: 01/24/2019
ms.author: diberry
ms.openlocfilehash: d209d73d67af96e99589dddcb71b6b50214356ee
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58877287"
---
# <a name="tutorial-using-c-create-knowledge-base-then-answer-question"></a>Öğretici: Kullanarak C#, bilgi tabanı oluşturan sonra soru yanıtlayın

Bu öğretici, program aracılığıyla bilgi bankası (KB) oluşturup yayımlama ve bunu kullanarak müşteri sorularını cevaplama adımlarını göstermektedir. 

> [!div class="checklist"]
> * Bilgi bankası oluşturma 
> * Oluşturma durumunu denetleme
> * Bilgi bankasını eğitme ve yayımlama
> * Uç nokta bilgisi alma
> * Curl kullanarak bilgi bankasını sorgulama


Bu hızlı başlangıç şu Soru-Cevap Oluşturma API'lerini çağırır:

* [Bilgi bankası (KB) oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)
* [İşlem Ayrıntılarını Alma](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/operations_getoperationdetails)
* [Bilgi bankası ayrıntılarını alma](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/knowledgebases_getknowledgebasedetails) 
* [Bilgi bankası taban uç noktalarını alma](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/endpointkeys_getendpointkeys)
* [Yayımlama](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fe) 

## <a name="prerequisites"></a>Önkoşullar

* En son [**Visual Studio Community sürümü**](https://www.visualstudio.com/downloads/).
* [Soru-Cevap Oluşturma hizmetine](../How-To/set-up-qnamaker-service-azure.md) sahip olmanız gerekir. Anahtarınızı almak için, panonuzda **Kaynak Yönetimi** altında **Anahtarlar** öğesini seçin. 

> [!NOTE] 
> Eksiksiz bir çözüm dosyaları kullanılabilir [ **Azure-Samples/bilişsel-services-qnamaker-csharp** GitHub deposu](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/tree/master/documentation-samples/tutorials/create-publish-answer-knowledge-base).

## <a name="create-a-knowledge-base-project"></a>Bilgi bankası projesi oluşturma

[!INCLUDE [Create Visual Studio Project](../../../../includes/cognitive-services-qnamaker-quickstart-csharp-create-project.md)] 

## <a name="add-the-required-dependencies"></a>Gerekli bağımlılıkları ekleme

Program.cs dosyasının en üst kısmındaki tek _using_ deyimini aşağıdaki satırlarla değiştirerek projeye gerekli bağımlılıkları ekleyin:

[!code-csharp[Add the required dependencies](~/samples-qnamaker-csharp/documentation-samples/tutorials/create-publish-answer-knowledge-base/QnaMakerQuickstart/Program.cs?range=1-11 "Add the required dependencies")]

## <a name="add-a-kbdetails-class"></a>KBDetails sınıfı ekleme
Bu KBDetails sınıfını Namespace köşeli ayraçlarının içine ekleyin. Bu sınıf, NewtonSoft kitaplığının JSON yanıtını seri durumdan çıkararak C# sınıfına almasını sağlar.

[!code-csharp[Add a KBDetails class](~/samples-qnamaker-csharp/documentation-samples/tutorials/create-publish-answer-knowledge-base/QnaMakerQuickstart/Program.cs?range=15-26 "Add a KBDetails class")]

## <a name="add-the-required-constants"></a>Gerekli sabitleri ekleme

Program sınıfının en üstüne Soru-Cevap Oluşturma erişimi için aşağıdaki sabitleri ekleyin:

[!code-csharp[Add the required constants](~/samples-qnamaker-csharp/documentation-samples/tutorials/create-publish-answer-knowledge-base/QnaMakerQuickstart/Program.cs?range=30-57 "Add the required constants")]

## <a name="add-the-kb-definition"></a>KB tanımını ekleme

Sabitlerden sonra aşağıdaki KB modeli tanımını ekleyin:

[!code-csharp[Add the KB definition](~/samples-qnamaker-csharp/documentation-samples/tutorials/create-publish-answer-knowledge-base/QnaMakerQuickstart/Program.cs?range=59-85 "Add the KB definition")]

## <a name="add-supporting-functions-and-structures"></a>Destekleyici işlevleri ve yapıları ekleme
Aşağıdaki kod bloğunu Program sınıfına ekleyin:

[!code-csharp[Add supporting functions and structures](~/samples-qnamaker-csharp/documentation-samples/tutorials/create-publish-answer-knowledge-base/QnaMakerQuickstart/Program.cs?range=87-123 "Add supporting functions and structures")]

## <a name="add-a-post-request-to-create-kb"></a>KB oluşturmak için POST isteği ekleme

Aşağıdaki kod KB oluşturmak için Soru-Cevap Oluşturma API'sine bir HTTPS isteği gönderir ve yanıtı alır:

[!code-csharp[Add a POST request to create KB](~/samples-qnamaker-csharp/documentation-samples/tutorials/create-publish-answer-knowledge-base/QnaMakerQuickstart/Program.cs?range=124-141 "Add a POST request to create KB")]

Bu API çağrısı, işlem kimliğini içeren bir JSON yanıtı döndürür. Program daha sonra KB'nin başarıyla oluşturulup oluşturulmadığını belirlemek için işlem kimliğini kullanır. 

```JSON
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-09-26T05:19:01Z",
  "lastActionTimestamp": "2018-09-26T05:19:01Z",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "YYYe12ff-5d04-4b73-b594-8575f9787963"
}
```

## <a name="add-get-request-to-determine-creation-status"></a>Oluşturma durumunu belirlemek için GET isteği ekleme

Oluşturma işleminin durumunu denetleyin.

[!code-csharp[Add GET request to determine creation status](~/samples-qnamaker-csharp/documentation-samples/tutorials/create-publish-answer-knowledge-base/QnaMakerQuickstart/Program.cs?range=142-151 "Add GET request to determine creation status")]

Bu API çağrısı, işlem durumunu içeren bir JSON yanıtı döndürür: 

```JSON
{
  "operationState": "Running",
  "createdTimestamp": "2018-09-26T05:22:53Z",
  "lastActionTimestamp": "2018-09-26T05:22:53Z",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "YYYe12ff-5d04-4b73-b594-8575f9787963"
}
```

Başarılı veya başarısız bir sonuç alana kadar çağrıyı tekrarlayın: 

```JSON
{
  "operationState": "Succeeded",
  "createdTimestamp": "2018-09-26T05:22:53Z",
  "lastActionTimestamp": "2018-09-26T05:23:08Z",
  "resourceLocation": "/knowledgebases/XXX7892b-10cf-47e2-a3ae-e40683adb714",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "YYYe12ff-5d04-4b73-b594-8575f9787963"
}
```

## <a name="add-createkb-method"></a>CreateKB metodu ekleme

Aşağıdaki yöntem KB oluşturma ve durum denetleme çağrılarını kapsüller.  _create_ **Operation ID**, POST yanıtı üst bilgisinin **Location** alanında döndürülür ve GET isteğindeki yolun bir parçası olarak kullanılır. KB oluşturma işlemi zaman alabileceğinden başarılı veya başarısız bir sonuç alana kadar durum denetimi çağrılarını tekrarlamanız gerekir. İşlem başarılı olduğunda KB Kimliği **resourceLocation** içinde döndürülür. 

[!code-csharp[Add GET request to determine creation status](~/samples-qnamaker-csharp/documentation-samples/tutorials/create-publish-answer-knowledge-base/QnaMakerQuickstart/Program.cs?range=152-227 "Add GET request to determine creation status")]

## <a name="add-publish-method"></a>Yayımlama yöntemi ekleme

Bilgi bankası başarıyla oluşturulduktan sonra yayımlama adımına geçebilirsiniz. Eğitimi API'sine çağrı yapmanız gerektiğini düşünmüş olabilirsiniz. Bu sürümde bu işleme gerek yoktur. 

Aşağıdaki kod KB yayımlamak için Soru-Cevap Oluşturma API'sine bir HTTPS isteği gönderir ve yanıtı alır:

[!code-csharp[Add publish method](~/samples-qnamaker-csharp/documentation-samples/tutorials/create-publish-answer-knowledge-base/QnaMakerQuickstart/Program.cs?range=228-259 "Add publish method")]

Yayımlama başarılı olursa API çağrısı boş yanıt gövdesiyle 204 durumunu döndürür. Bu hızlı başlangıç kodu, sonucu görebilmeniz için 204 yanıtlarına metin ekler.

Diğer yanıtlarda döndürülen yanıt değiştirilmez.

## <a name="generating-an-answer"></a>Yanıt oluşturma
Program, soru sormak ve en iyi cevabı almak üzere KB'ye erişim sağlayabilmek için KB ayrıntılar API'sinden _uç nokta ana bilgisayarı_ ve Uç Nokta API'sinden _birincil uç nokta anahtarı_ değerlerine ihtiyaç duyar. Bu yöntemler cevap oluşturma yöntemiyle birlikte aşağıdaki bölümlerde verilmiştir. 

Aşağıdaki tabloda verilerin URI oluşturmak için nasıl kullanıldığı gösterilmiştir:

|Yanıt URI şablonu oluşturma|
|--|
|https://**HOSTNAME**.azurewebsites.net/qnamaker/knowledgebases/**KBID**/generateAnswer|

Cevap oluşturulması amacıyla isteğin kimlik doğrulamasından geçirilmesi için _birincil uç nokta_ bir üst bilgi olarak geçirilir:

|Üst bilgi adı|Üst bilgi değeri|
|--|--|
|Yetkilendirme|`Endpoint` + **birincil uç nokta**<br>Örnek: `Endpoint xxxxxxx`<br>`Endpoint` metni ile birincil uç nokta değeri arasında boşluk olduğuna dikkat edin. 

İstek gövdesinde uygun JSON kodunun geçirilmesi gerekir:

```JSON
{
    question: "What languages does QnA Maker support?"
}
```

## <a name="get-kb-details"></a>KB ayrıntılarını alma
KB ayrıntılarını almak için aşağıdaki yöntemi kullanın. Bu ayrıntılar, KB'nin ana bilgisayar adını içerir. Ana bilgisayar adı, Soru-Cevap Oluşturma kaynağını oluştururken girdiğiniz Soru-Cevap Oluşturma Azure web hizmeti adıdır. 

[!code-csharp[Get KB Details](~/samples-qnamaker-csharp/documentation-samples/tutorials/create-publish-answer-knowledge-base/QnaMakerQuickstart/Program.cs?range=260-273 "Add publish method")]

Bu API çağrısı bir JSON yanıtı döndürür: 

```JSON
{
  "id": "ZZZ31e19-cba7-48d1-8594-5c4297ecc9c1",
  "hostName": "https://qnamaker-s0-s.azurewebsites.net",
  "lastAccessedTimestamp": "2018-10-11T18:10:05Z",
  "lastChangedTimestamp": "2018-10-11T18:09:37Z",
  "lastPublishedTimestamp": "2018-10-11T18:10:15Z",
  "name": "QnA Maker FAQ from quickstart",
  "userId": "AAAc3841df0b42cdb00f53a49d51a89c",
  "urls": [
    "https://docs.microsoft.com/en-in/azure/cognitive-services/qnamaker/faqs",
    "https://docs.microsoft.com/bot-framework/resources-bot-framework-faq"
  ],
  "sources": [
    "Custom Editorial"
  ]
}
```

## <a name="get-kb-endpoints"></a>KB uç noktalarını alma
Soru-Cevap Oluşturma hizmetinin birincil uç noktalarını almak için aşağıdaki yöntemi ekleyin. Bu uç noktalar KB'ye özgü değildir, Azure portaldaki Soru-Cevap Oluşturma kaynak anahtarlarıyla ilişkilendirilmiş olan tüm KB'ler için geçerlidir.  

[!code-csharp[Get KB Endpoints](~/samples-qnamaker-csharp/documentation-samples/tutorials/create-publish-answer-knowledge-base/QnaMakerQuickstart/Program.cs?range=274-289 "Get KB Endpoints")]

Bu API çağrısı bir JSON yanıtı döndürür: 

```JSON
{
  "primaryEndpointKey": "AAAb5719-e2f7-4a33-937d-7a3b4736a1be",
  "secondaryEndpointKey": "BBBcba78-c1d2-4166-b98f-c77255aefaba",
  "installedVersion": "4.2.0",
  "lastStableVersion": "4.2.0"
}
```

## <a name="get-an-answer"></a>Cevap alma
Kullanıcının sorusuna cevap almak için aşağıdaki yöntemi ekleyin. 

[!code-csharp[Get Answer](~/samples-qnamaker-csharp/documentation-samples/tutorials/create-publish-answer-knowledge-base/QnaMakerQuickstart/Program.cs?range=290-315 "Get Answer")]

Bu API çağrısı bir JSON yanıtı döndürür: 

```JSON
{
  "answers": [
    {
      "questions": [
        "Does QnA Maker support non-English languages?"
      ],
      "answer": "See more details about [supported languages](https://docs.microsoft.com/en-in/azure/cognitive-services/qnamaker/overview/languages-supported).\n\n\nIf you have content from multiple languages, be sure to create a separate service for each language.",
      "score": 82.19,
      "id": 11,
      "source": "https://docs.microsoft.com/en-in/azure/cognitive-services/qnamaker/faqs",
      "metadata": []
    }
  ]
}
```

## <a name="main-method"></a>Main yöntemi
Main yöntemi cevabı oluşturmak, yayımlamak ve üretmek için kullanılan zaman uyumlu çağrıları gösterir. 

[!code-csharp[Main method](~/samples-qnamaker-csharp/documentation-samples/tutorials/create-publish-answer-knowledge-base/QnaMakerQuickstart/Program.cs?range=316-337 "Main method")]

## <a name="build-and-run-the-program"></a>Programı derleme ve çalıştırma

Programı derleyin ve çalıştırın. 

Bilgi bankanız oluşturulduktan sonra Soru-Cevap Oluşturma Portalı’nızdaki [Bilgi bankalarım](https://www.qnamaker.ai/Home/MyServices) sayfasından görüntüleyebilirsiniz. Cevap API'si oluşturmayı öğrendikten sonra API'yi istediğiniz dil veya HTTP isteği çerçevesi ile kullanabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)
