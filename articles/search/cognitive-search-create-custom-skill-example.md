---
title: 'Örnek: özel bir yetenek bilişsel arama ardışık (Azure Search) oluşturun. | Microsoft Docs'
description: Ardışık Düzen Azure Search'te dizin bilişsel bir arama eşlenen özel nitelik Çevir metin API kullanarak gösterir.
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 056cff192b25068fa2e895fd46d143a834b7af0b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34641093"
---
# <a name="example-create-a-custom-skill-using-the-text-translate-api"></a>Örnek: metin Çevir API kullanarak özel bir yetenek oluşturma

Bu örnekte, bir web herhangi bir dilde kabul edip İngilizce'ye çevirir API özel nitelik oluşturmayı öğrenin. Örnek kullanır bir [Azure işlevi](https://azure.microsoft.com/services/functions/) sarmalamak için [Çevir metin API](https://azure.microsoft.com/services/cognitive-services/translator-text-api/) böylece özel nitelik arabirimini uygular.

## <a name="prerequisites"></a>Önkoşullar

+ Hakkında bilgi edinin [özel nitelik arabirimi](cognitive-search-custom-skill-interface.md) özel bir yetenek uygulamalıdır giriş/çıkış arabirimiyle bilmiyorsanız makalesi.

+ [Çevirici metin API için kaydolun](../cognitive-services/translator/translator-text-how-to-signup.md)ve bunu kullanmak için bir API anahtarı edinin.

+ Yükleme [Visual Studio 2017 sürüm 15,5](https://www.visualstudio.com/vs/) veya daha sonra Azure geliştirme iş yükü de dahil olmak üzere.

## <a name="create-an-azure-function"></a>Azure İşlevi oluşturma

Bu örnek bir web API barındırmak için bir Azure işlevi kullansa da, gerekli değildir.  Karşılamanız sürece [arabirim bilişsel yetenek gereksinimleri](cognitive-search-custom-skill-interface.md), gerçekleştireceğiniz kurucularýn yaklaşımdır. Azure işlevleri, ancak özel bir yetenek oluşturmak kolaylaştırır.

### <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

1. Visual Studio'da seçin **yeni** > **proje** Dosya menüsünden.

1. Yeni Proje iletişim kutusuna seçin **yüklü**, genişletin **Visual C#** > **bulut**seçin **Azure işlevleri**, tür a Projeniz için ad ve seçin **Tamam**. İşlev uygulamasının adı, bir C# ad alanı olarak geçerli olmalıdır; bu nedenle alt çizgi, kısa çizgi veya alfasayısal olmayan herhangi bir karakter kullanmayın.

1. Olmasını seçin **HTTP tetikleyici**

1. Depolama hesabı için seçtiğiniz **hiçbiri**gibi bu işlev için herhangi bir depolama alanı gerekmez.

1. Seçin **Tamam** işlevi oluşturmak için proje ve HTTP işlevi tetiklenir.

### <a name="modify-the-code-to-call-the-translate-cognitive-service"></a>Çevir Bilişsel hizmetini çağırmak için kodu değiştirin

Visual Studio bir proje oluşturur ve bu projenin içinde seçili işlev türü için ortak kod içeren bir sınıf bulunur. Metottaki *FunctionName* özniteliği işlevin adını ayarlar. *HttpTrigger* özniteliği, işlevin bir HTTP isteği tarafından tetiklenip tetiklenmediğini belirtir.

Şimdi, tüm dosya içeriğini değiştirmek *Function1.cs* aşağıdaki kod ile:

```csharp
using System.IO;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.AspNetCore.Http;
using Microsoft.Azure.WebJobs.Host;
using Newtonsoft.Json;
using System.Net.Http;
using System.Collections.Generic;
using System.Threading.Tasks;
using System.Text;

namespace TranslateFunction
{
    // This function will simply translate messages sent to it.
    public static class Function1
    {
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


        /// <summary>
        /// Note that this function can translate up to 1000 characters. If you expect to need to translate more characters, use 
        /// the paginator skill before calling this custom enricher
        /// </summary>
        [FunctionName("Translate")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Function, "post", Route = null)]HttpRequest req, 
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");

            string recordId = null;
            string originalText = null;
            string originalLanguage = null;
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
            originalLanguage = data?.values?.First?.data?.language?.Value as string;

            if (recordId == null)
            {
                return new BadRequestObjectResult("recordId cannot be null");
            }

            // Only translate records that actually need to be translated. 
            if (!originalLanguage.Contains("en"))
            {
                translatedText = TranslateText(originalText, "en-us").Result;
            }
            else
            {
                // text is already in English.
                translatedText = originalText;
            }

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
        /// Use Cognitive Service to translate text from one language to antoher.
        /// </summary>
        /// <param name="myText">The text to translate</param>
        /// <param name="destinationLanguage">The language you want to translate to.</param>
        /// <returns>Asynchronous task that returns the translated text. </returns>
        async static Task<string> TranslateText(string myText, string destinationLanguage)
        {
            string host = "https://api.microsofttranslator.com";
            string path = "/V2/Http.svc/Translate";

            // NOTE: Replace this example key with a valid subscription key.
            string key = "064d8095730d4a99b49f4bcf16ac67f8";

            HttpClient client = new HttpClient();
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", key);

            List<KeyValuePair<string, string>> list = new List<KeyValuePair<string, string>>() {
                new KeyValuePair<string, string>(myText, "en-us")
            };

            StringBuilder totalResult = new StringBuilder();

            foreach (KeyValuePair<string, string> i in list)
            {
                string uri = host + path + "?to=" + i.Value + "&text=" + System.Net.WebUtility.UrlEncode(i.Key);

                HttpResponseMessage response = await client.GetAsync(uri);

                string result = await response.Content.ReadAsStringAsync();

                // Parse the response XML
                System.Xml.XmlDocument xmlResponse = new System.Xml.XmlDocument();
                xmlResponse.LoadXml(result);
                totalResult.Append(xmlResponse.InnerText); 
            }

            return totalResult.ToString();
        }
    }
}
```

Girmek kendi emin *anahtar* değeri *TranslateText* yöntemine temel Çevir metin API için kaydolduğunuzda aldığınız anahtar.

Bu, aynı anda yalnızca tek bir kayıt üzerinde çalışan basit bir enricher örneğidir. Bu bulgu daha sonra skillset toplu iş boyutu ayarlarken önemli hale gelir.

## <a name="test-the-function-from-visual-studio"></a>Visual Studio'dan işlevi test etme

Tuşuna **F5** program ve test işlevi davranışları çalıştırmak için. Postman veya Fiddler aşağıda gösterilene benzer bir çağrı vermek için kullanın:

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
               "language": "es"
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

## <a name="publish-the-function-to-azure"></a>İşlev için Azure yayımlama

İşlev davranışını ile memnun kaldığınızda, yayımlayabilirsiniz.

1. **Çözüm Gezgini**'nde projeye sağ tıklayın ve **Yayımla**'yı seçin. Seçin **Yeni Oluştur** > **yayımlama**.

1. Visual Studio Azure hesabınızda zaten bağlanmadıysanız, seçin **Hesap Ekle...**

1. İzleyin ekran ister. Azure hesabı, kaynak grubu, barındırma planı ve kullanmak istediğiniz depolama hesabını belirtmeniz istenir. Bunlar zaten yoksa, yeni bir kaynak grubu, yeni bir barındırma planı ve bir depolama hesabı oluşturabilirsiniz. Tamamlandığında, seçin **oluştur**

1. Dağıtım tamamlandıktan sonra Site URL'si unutmayın. Azure'da işlevi uygulamanız adresidir. 

1. İçinde [Azure portal](https://portal.azure.com), kaynak grubuna gidin ve Çevir yayımladığınız işlevi için bakın. Altında **Yönet** bölümünde, ana bilgisayar anahtarları görmeniz gerekir. Seçin **kopya** simgesi *varsayılan* ana makine anahtarı.  


## <a name="test-the-function-in-azure"></a>Azure'da işlevi test etme

Varsayılan ana bilgisayar anahtarı sahip olduğunuza göre aşağıdaki gibi işlevinizi test edin:

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
               "language": "es"
            }
        }
   ]
}
```

Bu işlev yerel Ortamı'nda çalışırken daha önce gördüğünüz bir benzer bir sonuç üretir.

## <a name="connect-to-your-pipeline"></a>Ardışık düzeninize Bağlan
Yeni bir özel yetenek sahip olduğunuza göre skillset ekleyebilirsiniz. Aşağıdaki örnekte nasıl yetenek çağrılacağını gösterir. Yetenek toplu işlemiyor olduğundan, Maksimum toplu iş boyutu yalnızca bir yönerge ekleyin ```1``` göndermek için birer birer belgeleri.

```json
{
    "skills": [
      ...,  
      {
        "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
        "description": "Our new translator custom skill",
        "uri": "http://translatecogsrch.azurewebsites.net/api/Translate?code=[enter default host key here]",
        "batchSize":1,
        "context": "/document",
        "inputs": [
          {
            "name": "text",
            "source": "/document/content"
          },
          {
            "name": "language",
            "source": "/document/languageCode"
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

## <a name="next-steps"></a>Sonraki Adımlar
Tebrikler! İlk, özel enricher oluşturdunuz. Şimdi kendi özel işlevsellik eklemek için aynı deseni takip edebilirsiniz. 

+ [Özel bir yetenek bilişsel arama ardışık düzene ekleyin](cognitive-search-custom-skill-interface.md)
+ [Bir skillset tanımlama](cognitive-search-defining-skillset.md)
+ [Skillset (REST) oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Zenginleştirilmiş alanlarını eşleme](cognitive-search-output-field-mapping.md)