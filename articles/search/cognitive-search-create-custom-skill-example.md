---
title: 'Örnek: Bilişsel arama hattında - Azure Search özel bir yetenek oluşturma'
description: Bilişsel arama dizinleme işlem hattı, Azure Search eşlenen özel nitelik içinde metin çevirme API'sini kullanmayı gösterir.
manager: pablocas
author: luiscabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: efa85491f4b183a044ec5d9e5e6e3d11eebedbe3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66428440"
---
# <a name="example-create-a-custom-skill-using-the-text-translate-api"></a>Örnek: Metni Çevir API'sini kullanarak özel bir yetenek oluşturma

Bu örnekte, bir web API'si için özel yetenek oluşturmayı öğrenin. Bu yetenek, herhangi bir dilde kabul eder ve İngilizceye çevirir. Örnekte bir [Azure işlevi](https://azure.microsoft.com/services/functions/) sarmalamak için [Çevir metin API'si](https://azure.microsoft.com/services/cognitive-services/translator-text-api/) böylece özel bir yetenek arabirimini uygular.

## <a name="prerequisites"></a>Önkoşullar

+ Hakkında bilgi edinin [özel bir yetenek arabirimi](cognitive-search-custom-skill-interface.md) özel bir yetenek uygulamalıdır giriş/çıkış arabirimine aşina değilseniz, makalesi.

+ [Translator metin çevirisi API'si için kaydolun](../cognitive-services/translator/translator-text-how-to-signup.md)ve onu kullanmak üzere bir API anahtarınızı alın.

+ Yükleme [Visual Studio 2019](https://www.visualstudio.com/vs/) veya daha sonra Azure geliştirme iş yükü dahil.

## <a name="create-an-azure-function"></a>Azure İşlevi oluşturma

Bu örnek bir web API barındırmak için bir Azure işlevi kullansa da, gerekli değildir.  Karşıladıkları sürece [arabirim bilişsel bir beceri gereksinimleri](cognitive-search-custom-skill-interface.md), hangi yaklaşımın kurucularýn. Azure işlevleri, ancak özel bir yetenek oluşturmak kolaylaştırır.

### <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

1. Visual Studio'da **yeni** > **proje** Dosya menüsünden.

1. Yeni Proje iletişim kutusunda, seçmek **yüklü**, genişletme **Visual C#**  > **bulut**seçin **Azure işlevleri**, yazın Projeniz için ad ve seçin **Tamam**. İşlev uygulamasının adı, bir C# ad alanı olarak geçerli olmalıdır; bu nedenle alt çizgi, kısa çizgi veya alfasayısal olmayan herhangi bir karakter kullanmayın.

1. Seçin **Azure işlevler v2 (.NET Core)** . Ayrıca, sürüm 1 ile yapabilirsiniz, ancak aşağıda yazılan kodların v2 şablonu temel alan.

1. Olmasını seçin **HTTP tetikleyicisi**

1. Depolama hesabı için seçtiğiniz **hiçbiri**gibi bu işlev için herhangi bir depolama alanı gerekmez.

1. Seçin **Tamam** işlevi oluşturmak için proje ve HTTP ile tetiklenen işlev.

### <a name="modify-the-code-to-call-the-translate-cognitive-service"></a>Çevirme Bilişsel hizmet çağırmak için kodu değiştirin

Visual Studio bir proje oluşturur ve bu projenin içinde seçili işlev türü için ortak kod içeren bir sınıf bulunur. Metottaki *FunctionName* özniteliği işlevin adını ayarlar. *HttpTrigger* özniteliği, işlevin bir HTTP isteği tarafından tetiklenip tetiklenmediğini belirtir.

Şimdi, tüm dosya içeriğini değiştirmek *Function1.cs* aşağıdaki kod ile:

```csharp
using System;
using System.Net.Http;
using System.Collections.Generic;
using System.Threading.Tasks;
using System.IO;
using System.Text;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.AspNetCore.Http;
using Microsoft.Azure.WebJobs.Host;
using Newtonsoft.Json;

namespace TranslateFunction
{
    // This function will simply translate messages sent to it.
    public static class Function1
    {
        static string path = "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0";

        // NOTE: Replace this example key with a valid subscription key.
        static string key = "<enter your api key here>";

        #region classes used to serialize the response
        private class WebApiResponseError
        {
            public string message { get; set; }
        }

        private class WebApiResponseWarning
        {
            public string message { get; set; }
        }

        private class WebApiResponseRecord
        {
            public string recordId { get; set; }
            public Dictionary<string, object> data { get; set; }
            public List<WebApiResponseError> errors { get; set; }
            public List<WebApiResponseWarning> warnings { get; set; }
        }

        private class WebApiEnricherResponse
        {
            public List<WebApiResponseRecord> values { get; set; }
        }
        #endregion

        [FunctionName("Translate")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Function, "post", Route = null)]HttpRequest req,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");

            string recordId = null;
            string originalText = null;
            string toLanguage = null;
            string translatedText = null;

            string requestBody = new StreamReader(req.Body).ReadToEnd();
            dynamic data = JsonConvert.DeserializeObject(requestBody);

            // Validation
            if (data?.values == null)
            {
                return new BadRequestObjectResult(" Could not find values array");
            }
            if (data?.values.HasValues == false || data?.values.First.HasValues == false)
            {
                // It could not find a record, then return empty values array.
                return new BadRequestObjectResult(" Could not find valid records in values array");
            }

            recordId = data?.values?.First?.recordId?.Value as string;
            originalText = data?.values?.First?.data?.text?.Value as string;
            toLanguage = data?.values?.First?.data?.language?.Value as string;

            if (recordId == null)
            {
                return new BadRequestObjectResult("recordId cannot be null");
            }

            translatedText = TranslateText(originalText, toLanguage).Result;
        
            // Put together response.
            WebApiResponseRecord responseRecord = new WebApiResponseRecord();
            responseRecord.data = new Dictionary<string, object>();
            responseRecord.recordId = recordId;
            responseRecord.data.Add("text", translatedText);

            WebApiEnricherResponse response = new WebApiEnricherResponse();
            response.values = new List<WebApiResponseRecord>();
            response.values.Add(responseRecord);

            return (ActionResult)new OkObjectResult(response);
        }


        /// <summary>
        /// Use Cognitive Service to translate text from one language to another.
        /// </summary>
        /// <param name="originalText">The text to translate.</param>
        /// <param name="toLanguage">The language you want to translate to.</param>
        /// <returns>Asynchronous task that returns the translated text. </returns>
        async static Task<string> TranslateText(string originalText, string toLanguage)
        {
            System.Object[] body = new System.Object[] { new { Text = originalText } };
            var requestBody = JsonConvert.SerializeObject(body);

            var uri = $"{path}&to={toLanguage}";

            string result = "";

            using (var client = new HttpClient())
            using (var request = new HttpRequestMessage())
            {
                request.Method = HttpMethod.Post;
                request.RequestUri = new Uri(uri);
                request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");
                request.Headers.Add("Ocp-Apim-Subscription-Key", key);

                var response = await client.SendAsync(request);
                var responseBody = await response.Content.ReadAsStringAsync();

                dynamic data = JsonConvert.DeserializeObject(responseBody);
                result = data?.First?.translations?.First?.text?.Value as string;

            }
            return result;
        }
    }
}
```

Girmek kendi emin *anahtarı* değerini *TranslateText* Çevir metin API'si için kaydolurken aldığınız anahtarı temelinde yöntemi.

Bu, yalnızca tek bir kayıt üzerinde aynı anda çalışan basit bir enricher örneğidir. Bunu daha sonra beceri kümesi için toplu iş boyutu ayarlarken önemli hale gelir.

## <a name="test-the-function-from-visual-studio"></a>Visual Studio'dan işlevi test etme

Tuşuna **F5** program ve test işlevi davranışları çalıştırılacak. Bu durumda, İspanyolca, İngilizce için bir metni çevirmek için aşağıdaki işlevi kullanacağız. Postman veya fiddler'ı aşağıda gösterilene benzer bir çağrı vermek için kullanın:

```http
POST https://localhost:7071/api/Translate
```
### <a name="request-body"></a>İstek gövdesi
```json
{
   "values": [
        {
            "recordId": "a1",
            "data":
            {
               "text":  "Este es un contrato en Inglés",
               "language": "en"
            }
        }
   ]
}
```
### <a name="response"></a>Yanıt
Aşağıdaki örneğe benzer bir yanıt görmeniz gerekir:

```json
{
    "values": [
        {
            "recordId": "a1",
            "data": {
                "text": "This is a contract in English"
            },
            "errors": null,
            "warnings": null
        }
    ]
}
```

## <a name="publish-the-function-to-azure"></a>İşlevi Azure'a yayımlama

İşlev davranışını ile memnun kaldığınızda, bunu yayımlayabilirsiniz.

1. **Çözüm Gezgini**'nde projeye sağ tıklayın ve **Yayımla**'yı seçin. Seçin **Yeni Oluştur** > **yayımlama**.

1. Visual Studio henüz Azure hesabınıza bağlamadıysanız, seçin **Hesap Ekle...**

1. İzleyin ekrandaki ister. Azure hesabı, kaynak grubunu, barındırma planı ve kullanmak istediğiniz depolama hesabı belirtmeniz istenir. Bunlar zaten yoksa, yeni bir kaynak grubu, yeni bir barındırma planı ve bir depolama hesabı oluşturabilirsiniz. İşiniz bittiğinde seçin **oluştur**

1. Dağıtım tamamlandıktan sonra Site URL'si dikkat edin. İşlev uygulamanızda Azure'nın adresidir. 

1. İçinde [Azure portalında](https://portal.azure.com), kaynak grubuna gidin ve çevirme yayımladığınız işlevi bakın. Altında **Yönet** bölümünde, ana bilgisayar anahtarları görmeniz gerekir. Seçin **kopyalama** simgesi *varsayılan* ana bilgisayar anahtarı.  

## <a name="test-the-function-in-azure"></a>Azure'da işlevi test etme

Varsayılan ana bilgisayar anahtarı olduğuna göre işlevinizi şu şekilde test edin:

```http
POST https://translatecogsrch.azurewebsites.net/api/Translate?code=[enter default host key here]
```
### <a name="request-body"></a>İstek Gövdesi
```json
{
   "values": [
        {
            "recordId": "a1",
            "data":
            {
               "text":  "Este es un contrato en Inglés",
               "language": "en"
            }
        }
   ]
}
```

Bu örnek, yerel ortamda işlevi çalıştırıldığında, daha önce gördüğünüz bir benzer bir sonuç üretir.

## <a name="connect-to-your-pipeline"></a>Ardışık düzeninize bağlanma
Yeni özel bir yetenek olduğuna göre bunu standartlarındaki şu ekleyebilirsiniz. Aşağıdaki örnekte, yetenek çağırmak nasıl gösterir. Yetenek toplu işlemiyor olduğundan, en yüksek toplu iş boyutu yalnızca olması için bir yönerge ekleyin ```1``` göndermek için tek tek belgeler.

```json
{
    "skills": [
      ...,  
      {
        "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
        "description": "Our new translator custom skill",
        "uri": "https://translatecogsrch.azurewebsites.net/api/Translate?code=[enter default host key here]",
        "batchSize":1,
        "context": "/document",
        "inputs": [
          {
            "name": "text",
            "source": "/document/content"
          },
          {
            "name": "language",
            "source": "/document/destinationLanguage"
          }
        ],
        "outputs": [
          {
            "name": "text",
            "targetName": "translatedText"
          }
        ]
      }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar
Tebrikler! İlk, özel enricher oluşturdunuz. Şimdi, kendi özel işlevsellik eklemek için aynı deseni takip edebilirsiniz. 

+ [Bilişsel arama işlem hattı için özel bir yetenek Ekle](cognitive-search-custom-skill-interface.md)
+ [Bir beceri kümesi tanımlama](cognitive-search-defining-skillset.md)
+ [Beceri kümesi (REST) oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Zenginleştirilmiş alanlarını eşleme](cognitive-search-output-field-mapping.md)
