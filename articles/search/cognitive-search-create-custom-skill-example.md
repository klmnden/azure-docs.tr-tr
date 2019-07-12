---
title: "Örnek: Bing varlık arama API'si ile - Azure Search özel bir bilişsel beceri oluşturma"
description: Azure Search'te bir bilişsel arama dizini oluşturma ardışık düzeni için eşlenen özel bir yetenek, Bing varlık arama hizmetini kullanmayı gösterir.
manager: pablocas
author: luiscabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 7d90f46ada9b9453b4c1516a4a898456dc73b8e7
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67672138"
---
# <a name="example-create-a-custom-skill-using-the-bing-entity-search-api"></a>Örnek: Bing varlık arama API'si kullanarak özel bir yetenek oluşturma

Bu örnekte, bir web API'si için özel yetenek oluşturmayı öğrenin. Bu yetenek konumları, ortak rakamları ve kuruluşların kabul edin ve onları döndürür. Örnekte bir [Azure işlevi](https://azure.microsoft.com/services/functions/) sarmalamak için [Bing varlık arama API'si](https://azure.microsoft.com/services/cognitive-services/bing-entity-search-api/) böylece özel bir yetenek arabirimini uygular.

## <a name="prerequisites"></a>Önkoşullar

+ Hakkında bilgi edinin [özel bir yetenek arabirimi](cognitive-search-custom-skill-interface.md) özel bir yetenek uygulamalıdır giriş/çıkış arabirimine aşina değilseniz, makalesi.

+ [!INCLUDE [cognitive-services-bing-entity-search-signup-requirements](../../includes/cognitive-services-bing-entity-search-signup-requirements.md)]

+ Yükleme [Visual Studio 2019](https://www.visualstudio.com/vs/) veya daha sonra Azure geliştirme iş yükü dahil.

## <a name="create-an-azure-function"></a>Azure İşlevi oluşturma

Bu örnek bir web API barındırmak için bir Azure işlevi kullansa da, gerekli değildir.  Karşıladıkları sürece [arabirim bilişsel bir beceri gereksinimleri](cognitive-search-custom-skill-interface.md), hangi yaklaşımın kurucularýn. Azure işlevleri, ancak özel bir yetenek oluşturmak kolaylaştırır.

### <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

1. Visual Studio'da **yeni** > **proje** Dosya menüsünden.

1. Yeni Proje iletişim kutusunda, seçmek **yüklü**, genişletme **Visual C#**  > **bulut**seçin **Azure işlevleri**, yazın Projeniz için ad ve seçin **Tamam**. İşlev uygulamasının adı olarak geçerli bir C# ad alanı, böylece alt çizgi, kısa çizgi ve alfasayısal olmayan herhangi bir karakter kullanmayın.

1. Seçin **Azure işlevler v2 (.NET Core)** . Ayrıca, sürüm 1 ile yapabilirsiniz, ancak aşağıda yazılan kodların v2 şablonu temel alan.

1. Olmasını seçin **HTTP tetikleyicisi**

1. Depolama hesabı için seçtiğiniz **hiçbiri**gibi bu işlev için herhangi bir depolama alanı gerekmez.

1. Seçin **Tamam** işlevi oluşturmak için proje ve HTTP ile tetiklenen işlev.

### <a name="modify-the-code-to-call-the-bing-entity-search-service"></a>Bing varlık arama hizmetini çağırmak için kodu değiştirin

Visual Studio bir proje oluşturur ve bu projenin içinde seçili işlev türü için ortak kod içeren bir sınıf bulunur. Metottaki *FunctionName* özniteliği işlevin adını ayarlar. *HttpTrigger* özniteliği, işlevin bir HTTP isteği tarafından tetiklenip tetiklenmediğini belirtir.

Şimdi, tüm dosya içeriğini değiştirmek *Function1.cs* aşağıdaki kod ile:

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Net.Http;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Logging;
using Newtonsoft.Json;

namespace SampleSkills
{
    /// <summary>
    /// Sample custom skill that wraps the Bing entity search API to connect it with a 
    /// cognitive search pipeline.
    /// </summary>
    public static class BingEntitySearch
    {
        #region Credentials
        // IMPORTANT: Make sure to enter your credential and to verify the API endpoint matches yours.
        static readonly string bingApiEndpoint = "https://api.cognitive.microsoft.com/bing/v7.0/entities/";
        static readonly string key = "<enter your api key here>";  
        #endregion

        #region Class used to deserialize the request
        private class InputRecord
        {
            public class InputRecordData
            {
                public string Name { get; set; }
            }

            public string RecordId { get; set; }
            public InputRecordData Data { get; set; }
        }

        private class WebApiRequest
        {
            public List<InputRecord> Values { get; set; }
        }
        #endregion

        #region Classes used to serialize the response

        private class OutputRecord
        {
            public class OutputRecordData
            {
                public string Name { get; set; } = "";
                public string Description { get; set; } = "";
                public string Source { get; set; } = "";
                public string SourceUrl { get; set; } = "";
                public string LicenseAttribution { get; set; } = "";
                public string LicenseUrl { get; set; } = "";
            }

            public class OutputRecordMessage
            {
                public string Message { get; set; }
            }

            public string RecordId { get; set; }
            public OutputRecordData Data { get; set; }
            public List<OutputRecordMessage> Errors { get; set; }
            public List<OutputRecordMessage> Warnings { get; set; }
        }

        private class WebApiResponse
        {
            public List<OutputRecord> Values { get; set; }
        }
        #endregion

        #region Classes used to interact with the Bing API
        private class BingResponse
        {
            public BingEntities Entities { get; set; }
        }
        private class BingEntities
        {
            public BingEntity[] Value { get; set; }
        }

        private class BingEntity
        {
            public class EntityPresentationinfo
            {
                public string[] EntityTypeHints { get; set; }
            }

            public class License
            {
                public string Url { get; set; }
            }

            public class ContractualRule
            {
                public string _type { get; set; }
                public License License { get; set; }
                public string LicenseNotice { get; set; }
                public string Text { get; set; }
                public string Url { get; set; }
            }

            public ContractualRule[] ContractualRules { get; set; }
            public string Description { get; set; }
            public string Name { get; set; }
            public EntityPresentationinfo EntityPresentationInfo { get; set; }
        }
        #endregion

        #region The Azure Function definition

        [FunctionName("EntitySearch")]
        public static async Task<IActionResult> Run(
            [HttpTrigger(AuthorizationLevel.Function, "post", Route = null)] HttpRequest req,
            ILogger log)
        {
            log.LogInformation("Entity Search function: C# HTTP trigger function processed a request.");

            var response = new WebApiResponse
            {
                Values = new List<OutputRecord>()
            };

            string requestBody = new StreamReader(req.Body).ReadToEnd();
            var data = JsonConvert.DeserializeObject<WebApiRequest>(requestBody);

            // Do some schema validation
            if (data == null)
            {
                return new BadRequestObjectResult("The request schema does not match expected schema.");
            }
            if (data.Values == null)
            {
                return new BadRequestObjectResult("The request schema does not match expected schema. Could not find values array.");
            }

            // Calculate the response for each value.
            foreach (var record in data.Values)
            {
                if (record == null || record.RecordId == null) continue;

                OutputRecord responseRecord = new OutputRecord
                {
                    RecordId = record.RecordId
                };

                try
                {
                    responseRecord.Data = GetEntityMetadata(record.Data.Name).Result;
                }
                catch (Exception e)
                {
                    // Something bad happened, log the issue.
                    var error = new OutputRecord.OutputRecordMessage
                    {
                        Message = e.Message
                    };

                    responseRecord.Errors = new List<OutputRecord.OutputRecordMessage>
                    {
                        error
                    };
                }
                finally
                {
                    response.Values.Add(responseRecord);
                }
            }

            return (ActionResult)new OkObjectResult(response);
        }

        #endregion

        #region Methods to call the Bing API
        /// <summary>
        /// Gets metadata for a particular entity based on its name using Bing Entity Search
        /// </summary>
        /// <param name="entityName">The name of the entity to extract data for.</param>
        /// <returns>Asynchronous task that returns entity data. </returns>
        private async static Task<OutputRecord.OutputRecordData> GetEntityMetadata(string entityName)
        {
            var uri = bingApiEndpoint + "?q=" + entityName + "&mkt=en-us&count=10&offset=0&safesearch=Moderate";
            var result = new OutputRecord.OutputRecordData();

            using (var client = new HttpClient())
            using (var request = new HttpRequestMessage {
                Method = HttpMethod.Get,
                RequestUri = new Uri(uri)
            })
            {
                request.Headers.Add("Ocp-Apim-Subscription-Key", key);

                HttpResponseMessage response = await client.SendAsync(request);
                string responseBody = await response?.Content?.ReadAsStringAsync();

                BingResponse bingResult = JsonConvert.DeserializeObject<BingResponse>(responseBody);
                if (bingResult != null)
                {
                    // In addition to the list of entities that could match the name, for simplicity let's return information
                    // for the top match as additional metadata at the root object.
                    return AddTopEntityMetadata(bingResult.Entities?.Value);
                }
            }

            return result;
        }

        private static OutputRecord.OutputRecordData AddTopEntityMetadata(BingEntity[] entities)
        {
            if (entities != null)
            {
                foreach (BingEntity entity in entities.Where(
                    entity => entity?.EntityPresentationInfo?.EntityTypeHints != null
                        && (entity.EntityPresentationInfo.EntityTypeHints[0] == "Person"
                            || entity.EntityPresentationInfo.EntityTypeHints[0] == "Organization"
                            || entity.EntityPresentationInfo.EntityTypeHints[0] == "Location")
                        && !String.IsNullOrEmpty(entity.Description)))
                {
                    var rootObject = new OutputRecord.OutputRecordData
                    {
                        Description = entity.Description,
                        Name = entity.Name
                    };

                    if (entity.ContractualRules != null)
                    {
                        foreach (var rule in entity.ContractualRules)
                        {
                            switch (rule._type)
                            {
                                case "ContractualRules/LicenseAttribution":
                                    rootObject.LicenseAttribution = rule.LicenseNotice;
                                    rootObject.LicenseUrl = rule.License.Url;
                                    break;
                                case "ContractualRules/LinkAttribution":
                                    rootObject.Source = rule.Text;
                                    rootObject.SourceUrl = rule.Url;
                                    break;
                            }
                        }
                    }

                    return rootObject;
                }
            }

            return new OutputRecord.OutputRecordData();
        }
        #endregion
    }
}
```

Girmek kendi emin *anahtarı* değerini `key` Bing varlık arama API'si için kaydolurken aldığınız anahtarı temelinde sabiti.

Bu örnek, tek bir dosyada kolaylık sağlamak için tüm gerekli kodu içerir. Biraz daha yapılandırılmış bir sürümü aynı yetenek bulabilirsiniz [güç yetenekleri depo](https://github.com/Azure-Samples/azure-search-power-skills/tree/master/Text/BingEntitySearch).

Dosyayı kuşkusuz, yeniden adlandırabilirsiniz `Function1.cs` için `BingEntitySearch.cs`.

## <a name="test-the-function-from-visual-studio"></a>Visual Studio'dan işlevi test etme

Tuşuna **F5** program ve test işlevi davranışları çalıştırılacak. Bu durumda, iki varlık aramak için aşağıdaki işlevi kullanacağız. Postman veya fiddler'ı aşağıda gösterilene benzer bir çağrı vermek için kullanın:

```http
POST https://localhost:7071/api/EntitySearch
```

### <a name="request-body"></a>İstek gövdesi
```json
{
    "values": [
        {
            "recordId": "e1",
            "data":
            {
                "name":  "Pablo Picasso"
            }
        },
        {
            "recordId": "e2",
            "data":
            {
                "name":  "Microsoft"
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
            "recordId": "e1",
            "data": {
                "name": "Pablo Picasso",
                "description": "Pablo Ruiz Picasso was a Spanish painter [...]",
                "source": "Wikipedia",
                "sourceUrl": "http://en.wikipedia.org/wiki/Pablo_Picasso",
                "licenseAttribution": "Text under CC-BY-SA license",
                "licenseUrl": "http://creativecommons.org/licenses/by-sa/3.0/"
            },
            "errors": null,
            "warnings": null
        },
        "..."
    ]
}
```

## <a name="publish-the-function-to-azure"></a>İşlevi Azure'a yayımlama

İşlev davranışını ile memnun kaldığınızda, bunu yayımlayabilirsiniz.

1. **Çözüm Gezgini**'nde projeye sağ tıklayın ve **Yayımla**'yı seçin. Seçin **Yeni Oluştur** > **yayımlama**.

1. Visual Studio henüz Azure hesabınıza bağlamadıysanız, seçin **Hesap Ekle...**

1. İzleyin ekrandaki ister. App service, Azure aboneliği, kaynak grubunu, barındırma planı ve kullanmak istediğiniz depolama hesabı için benzersiz bir ad belirtmeniz istenir. Bunlar zaten yoksa, yeni bir kaynak grubu, yeni bir barındırma planı ve bir depolama hesabı oluşturabilirsiniz. İşiniz bittiğinde seçin **oluştur**

1. Dağıtım tamamlandıktan sonra Site URL'si dikkat edin. İşlev uygulamanızda Azure'nın adresidir. 

1. İçinde [Azure portalında](https://portal.azure.com), kaynak grubuna gidin ve Ara `EntitySearch` yayımladığınız işlevi. Altında **Yönet** bölümünde, ana bilgisayar anahtarları görmeniz gerekir. Seçin **kopyalama** simgesi *varsayılan* ana bilgisayar anahtarı.  

## <a name="test-the-function-in-azure"></a>Azure'da işlevi test etme

Varsayılan ana bilgisayar anahtarı olduğuna göre işlevinizi şu şekilde test edin:

```http
POST https://[your-entity-search-app-name].azurewebsites.net/api/EntitySearch?code=[enter default host key here]
```

### <a name="request-body"></a>İstek Gövdesi
```json
{
    "values": [
        {
            "recordId": "e1",
            "data":
            {
                "name":  "Pablo Picasso"
            }
        },
        {
            "recordId": "e2",
            "data":
            {
                "name":  "Microsoft"
            }
        }
    ]
}
```

Bu örnek, yerel ortamda işlevi çalıştırıldığında, daha önce gördüğünüz aynı sonucu üretir.

## <a name="connect-to-your-pipeline"></a>Ardışık düzeninize bağlanma
Yeni özel bir yetenek olduğuna göre bunu standartlarındaki şu ekleyebilirsiniz. Aşağıdaki örnekte, yetenek kuruluşlarda (Bu da konumları ve kişiler üzerinde çalışmak için Genişletilebilir) belge tanımları eklemek için arama işlemini göstermektedir. Değiştirin `[your-entity-search-app-name]` uygulamanızın adına sahip.

```json
{
    "skills": [
      "[... your existing skills remain here]",  
      {
        "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
        "description": "Our new Bing entity search custom skill",
        "uri": "https://[your-entity-search-app-name].azurewebsites.net/api/EntitySearch?code=[enter default host key here]",
          "context": "/document/merged_content/organizations/*",
          "inputs": [
            {
              "name": "name",
              "source": "/document/merged_content/organizations/*"
            }
          ],
          "outputs": [
            {
              "name": "description",
              "targetName": "description"
            }
          ]
      }
  ]
}
```

Burada, biz üzerinde yerleşik sayım [varlık tanıma beceri](cognitive-search-skill-entity-recognition.md) becerilerine içinde mevcut olması ve kuruluşların listesi belgeyle zenginleştirilmiş. Başvuru için ihtiyacımız olan veriler oluşturmak yeterli olacaktır bir varlık ayıklama beceri yapılandırması aşağıda verilmiştir:

```json
{
    "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
    "name": "#1",
    "description": "Organization name extraction",
    "context": "/document/merged_content",
    "categories": [ "Organization" ],
    "defaultLanguageCode": "en",
    "inputs": [
        {
            "name": "text",
            "source": "/document/merged_content"
        },
        {
            "name": "languageCode",
            "source": "/document/language"
        }
    ],
    "outputs": [
        {
            "name": "organizations",
            "targetName": "organizations"
        }
    ]
},
```

## <a name="next-steps"></a>Sonraki adımlar
Tebrikler! İlk, özel enricher oluşturdunuz. Şimdi, kendi özel işlevsellik eklemek için aynı deseni takip edebilirsiniz. 

+ [Bilişsel arama işlem hattı için özel bir yetenek Ekle](cognitive-search-custom-skill-interface.md)
+ [Bir beceri kümesi tanımlama](cognitive-search-defining-skillset.md)
+ [Beceri kümesi (REST) oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Zenginleştirilmiş alanlarını eşleme](cognitive-search-output-field-mapping.md)
