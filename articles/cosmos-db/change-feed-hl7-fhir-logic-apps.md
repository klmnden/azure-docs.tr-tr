---
title: Değişiklik akışı HL7 FHIR kaynaklar - Azure Cosmos DB
description: Azure Logic Apps, Azure Cosmos DB ve Service Bus'ı kullanarak HL7 FHIR hasta sağlık kaydı için değişiklik bildirimleri ayarlama konusunda bilgi edinin.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 05/28/2019
ms.author: sngun
ms.openlocfilehash: 49ef63969bd603c25d120dc5cb93ed30dda04241
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66257266"
---
# <a name="notifying-patients-of-hl7-fhir-health-care-record-changes-using-logic-apps-and-azure-cosmos-db"></a>Logic Apps ve Azure Cosmos DB'yi kullanarak HL7 FHIR sağlık kaydı değişikliklerinin hastalara bildirme

Azure MVP Howard Edidin, son kullanıcıların bir Hasta portalı yeni işlevler eklemek için bir sağlık hizmeti kuruluşunda tarafından bağlanıldı. Kendi sistem durumu kayıt güncelleştirildi ve bu güncelleştirmeler için abone için hastalarla gerektiği hastalara bildirimleri göndermek gerekli. 

Bu makale, Azure Cosmos DB, Logic Apps ve Service Bus'ı kullanarak sağlık bu kuruluş için oluşturulan bildirim çözüm akışı değişiklik rehberlik yapacaktır. 

## <a name="project-requirements"></a>Proje gereksinimleri
- XML biçiminde HL7 birleştirilmiş Klinik belge mimarisi (C-CDA) belgeleri sağlayıcıları gönderin. C CDA belgeleri kapsayabilir ve neredeyse her türde aile geçmişleri ve de yönetici olarak immunization kayıtlar gibi Klinik belgeleri, iş akışı ve Finans belgeleri de dahil olmak üzere Klinik belge. 
- C CDA belgeleri dönüştürülür [HL7 FHIR kaynakları](https://hl7.org/fhir/2017Jan/resourcelist.html) JSON biçiminde.
- Değiştirilen FHIR kaynak belgeleri, JSON biçimindeki e-postayla gönderilir.

## <a name="solution-workflow"></a>Çözümü iş akışı 

Yüksek düzeyde, aşağıdaki iş akışı adımları proje gerekli: 
1. C CDA belgeleri FHIR kaynaklarına dönüştürün.
2. Değiştirilen FHIR kaynaklar için yoklama yinelenen tetikleme eylemini gerçekleştirin. 
2. Azure Cosmos DB ile yeni veya değiştirilen belgeler için sorgu bağlanmak için FhirNotificationApi, özel bir uygulama arayın.
3. Service Bus kuyruğuna bir yanıt kaydedin.
4. Service Bus kuyruğundaki yeni iletiler için yoklama.
5. Hastalara e-posta bildirimleri gönderin.

## <a name="solution-architecture"></a>Çözüm mimarisi
Bu çözüm, yukarıdaki gereksinimlerini ve çözüm iş akışını tamamlamak üç Logic Apps gerektirir. Üç mantıksal uygulamalar şunlardır:
1. **HL7 FHIR eşleme uygulama**: HL7 C-CDA belge alır, FHIR kaynağa dönüştürür ve ardından Azure Cosmos DB'ye kaydeder.
2. **EHR uygulama**: Azure Cosmos DB FHIR depo sorgular ve Service Bus kuyruğuna bir yanıt kaydeder. Bu mantıksal uygulama kullanan bir [API uygulaması](#api-app) yeni ve değiştirilen belgeler alınamadı.
3. **İşlem bildirimi uygulama**: Gövdesinde FHIR kaynak belgeleri içeren bir e-posta bildirimi gönderir.

![Bu HL7 FHIR sağlık çözümde kullanılan üç Logic Apps](./media/change-feed-hl7-fhir-logic-apps/health-care-solution-hl7-fhir.png)



### <a name="azure-services-used-in-the-solution"></a>Çözümde kullanılan azure Hizmetleri

#### <a name="azure-cosmos-db-sql-api"></a>Azure Cosmos DB SQL API
Azure Cosmos DB FHIR kaynaklar için aşağıdaki şekilde gösterildiği gibi depodur.

![Bu HL7 FHIR sağlık öğreticide kullanılan Azure Cosmos DB hesabı](./media/change-feed-hl7-fhir-logic-apps/account.png)

#### <a name="logic-apps"></a>Logic Apps
Logic Apps iş akışı işlemi işleyin. Aşağıdaki ekran görüntüleri, bu çözüm için oluşturulan mantıksal uygulamaları göster. 


1. **HL7 FHIR eşleme uygulama**: HL7 C-CDA belge alır ve için Logic Apps Enterprise Integration Pack kullanarak bir FHIR kaynağa dönüştürün. Enterprise Integration Pack C CDA eşlemesinden FHIR kaynaklara işler.

    ![HL7 FHIR sağlık kayıtları almak için kullanılan mantıksal uygulama](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-logic-apps-json-transform.png)


2. **EHR uygulama**: Azure Cosmos DB FHIR deposunu sorgulama ve Service Bus kuyruğuna bir yanıt kaydedin. GetNewOrModifiedFHIRDocuments uygulama kodunu aşağıda verilmiştir.

    ![Mantıksal uygulamayı Azure Cosmos DB'yi sorgulama için kullanılır](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-logic-apps-api-app.png)

3. **İşlem bildirimi uygulama**: Gövdesinde FHIR kaynak belgeleri içeren bir e-posta bildirimi gönderin.

    ![Gövdesinde HL7 FHIR kaynakla hasta e-posta gönderen bir mantıksal uygulama](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-logic-apps-send-email.png)

#### <a name="service-bus"></a>Service Bus
Hastalara aşağıdaki şekilde gösterilmiştir kuyruk. Tag özelliği değerine e-posta konusu için kullanılır.

![Bu HL7 FHIR öğreticide kullanılan Service Bus kuyruğu](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-service-bus-queue.png)

<a id="api-app"></a>

#### <a name="api-app"></a>API uygulaması
Azure Cosmos DB ve sorguları kaynak türüne göre yeni veya değiştirilmiş FHIR belgeler için bir API uygulamasına bağlar. Bu uygulamanın bir denetleyici yok **FhirNotificationApi** tek bir işlemle **GetNewOrModifiedFhirDocuments**, bkz: [API uygulaması için kaynak](#api-app-source).

Kullanıyoruz [ `CreateDocumentChangeFeedQuery` ](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery.aspx) Azure Cosmos DB SQL .NET API sınıfı. Daha fazla bilgi için [değişiklik akışı makale](change-feed.md). 

##### <a name="getnewormodifiedfhirdocuments-operation"></a>GetNewOrModifiedFhirDocuments işlemi

**Giriş**
- Databaseıd
- CollectionId
- HL7 FHIR kaynak türü adı
- Boole: Baştan başlayın
- Int: Sayı, döndürülen belgelerin

**Çıkışlar**
- Başarılı: Durum kodu: 200, Response: Belgeleri (JSON dizisi) listesi
- Hata: Durum kodu: 404 yanıtı: "Hiçbir belge için bulunamadı '*kaynak adı '* kaynak türü"

<a id="api-app-source"></a>

**API uygulaması için kaynak**

```csharp

    using System.Collections.Generic;
    using System.Linq;
    using System.Net;
    using System.Net.Http;
    using System.Threading.Tasks;
    using System.Web.Http;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.Client;
    using Swashbuckle.Swagger.Annotations;
    using TRex.Metadata;
    
    namespace FhirNotificationApi.Controllers
    {
        /// <summary>
        ///     FHIR Resource Type Controller
        /// </summary>
        /// <seealso cref="System.Web.Http.ApiController" />
        public class FhirResourceTypeController : ApiController
        {
            /// <summary>
            ///     Gets the new or modified FHIR documents from Last Run Date 
            ///     or create date of the collection
            /// </summary>
            /// <param name="databaseId"></param>
            /// <param name="collectionId"></param>
            /// <param name="resourceType"></param>
            /// <param name="startfromBeginning"></param>
            /// <param name="maximumItemCount">-1 returns all (default)</param>
            /// <returns></returns>
            [Metadata("Get New or Modified FHIR Documents",
                "Query for new or modified FHIR Documents By Resource Type " +
                "from Last Run Date or Beginning of Collection creation"
            )]
            [SwaggerResponse(HttpStatusCode.OK, type: typeof(Task<dynamic>))]
            [SwaggerResponse(HttpStatusCode.NotFound, "No New or Modified Documents found")]
            [SwaggerOperation("GetNewOrModifiedFHIRDocuments")]
            public async Task<dynamic> GetNewOrModifiedFhirDocuments(
                [Metadata("Database Id", "Database Id")] string databaseId,
                [Metadata("Collection Id", "Collection Id")] string collectionId,
                [Metadata("Resource Type", "FHIR resource type name")] string resourceType,
                [Metadata("Start from Beginning ", "Change Feed Option")] bool startfromBeginning,
                [Metadata("Maximum Item Count", "Number of documents returned. '-1 returns all' (default)")] int maximumItemCount = -1
            )
            {
                var collectionLink = UriFactory.CreateDocumentCollectionUri(databaseId, collectionId);
    
                var context = new DocumentDbContext();  
    
                var docs = new List<dynamic>();
    
                var partitionKeyRanges = new List<PartitionKeyRange>();
                FeedResponse<PartitionKeyRange> pkRangesResponse;
    
                do
                {
                    pkRangesResponse = await context.Client.ReadPartitionKeyRangeFeedAsync(collectionLink);
                    partitionKeyRanges.AddRange(pkRangesResponse);
                } while (pkRangesResponse.ResponseContinuation != null);
    
                foreach (var pkRange in partitionKeyRanges)
                {
                    var changeFeedOptions = new ChangeFeedOptions
                    {
                        StartFromBeginning = startfromBeginning,
                        RequestContinuation = null,
                        MaxItemCount = maximumItemCount,
                        PartitionKeyRangeId = pkRange.Id
                    };
    
                    using (var query = context.Client.CreateDocumentChangeFeedQuery(collectionLink, changeFeedOptions))
                    {
                        do
                        {
                            if (query != null)
                            {
                                var results = await query.ExecuteNextAsync<dynamic>().ConfigureAwait(false);
                                if (results.Count > 0)
                                    docs.AddRange(results.Where(doc => doc.resourceType == resourceType));
                            }
                            else
                            {
                                throw new HttpResponseException(new HttpResponseMessage(HttpStatusCode.NotFound));
                            }
                        } while (query.HasMoreResults);
                    }
                }
                if (docs.Count > 0)
                    return docs;
                var msg = new StringContent("No documents found for " + resourceType + " Resource");
                var response = new HttpResponseMessage
                {
                    StatusCode = HttpStatusCode.NotFound,
                    Content = msg
                };
                return response;
            }
        }
    }
    
```

### <a name="testing-the-fhirnotificationapi"></a>FhirNotificationApi test etme 

Aşağıdaki görüntüde, swagger test etmek için nasıl kullanıldığını gösterilmektedir [FhirNotificationApi](#api-app-source).

![API uygulamasını test etmek için kullanılan bir Swagger dosyası](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-testing-app.png)


### <a name="azure-portal-dashboard"></a>Azure portalı Panosu

Aşağıdaki görüntüde, tüm Azure Hizmetleri çalıştıran Azure portalında Bu çözüm için gösterir.

![Bu HL7 FHIR öğreticide kullanılan tüm hizmetleri gösteren Azure portal](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-portal.png)


## <a name="summary"></a>Özet

- Azure Cosmos DB için yeni veya değiştirilen belgeler ve kullanmak için ne kadar kolay olduğunu bildirimleri için yerel destek olduğunu öğrendiniz. 
- Logic Apps yararlanarak herhangi bir kod yazmadan iş akışları oluşturabilirsiniz.
- Dağıtım HL7 FHIR belgeleri işlemek için Azure Service Bus kuyruklarını kullanma.

## <a name="next-steps"></a>Sonraki adımlar
Azure Cosmos DB hakkında daha fazla bilgi için bkz. [Azure Cosmos DB giriş sayfası](https://azure.microsoft.com/services/cosmos-db/). Logic Apps hakkında daha fazla bilgi için bkz. [Logic Apps](https://azure.microsoft.com/services/logic-apps/).


