---
title: "HL7 FHIR kaynaklar - Azure Cosmos DB akış değişiklik | Microsoft Docs"
description: "Azure mantıksal uygulamaları, Azure Cosmos DB ve hizmet veri yolu kullanarak HL7 FHIR hasta sağlık kayıtları için değişiklik bildirimlerinin ayarlanacağını öğrenin."
keywords: HL7 fhir
services: cosmos-db
author: hedidin
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 0d25c11f-9197-419a-aa19-4614c6ab2d06
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: b-hoedid
ms.openlocfilehash: 7a041e2121a2762af4307d7044437032cce79f05
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="notifying-patients-of-hl7-fhir-health-care-record-changes-using-logic-apps-and-azure-cosmos-db"></a>Logic Apps ile Azure Cosmos DB HL7 FHIR sağlık kayıt değişiklikleri hastalar bildirme

Azure MVP Howard Edidin, son kullanıcıların hasta portalına yeni işlevler eklemek sağlık bir kuruluş tarafından kurulduğunu. Kendi sistem durumu kayıt güncelleştirildi ve hastalar bu güncelleştirmelere abone olması gerektiği için hastalar bildirimleri göndermek gerekli. 

Bu makalede Azure Cosmos DB, mantıksal uygulamalar ve hizmet veri yolu kullanarak sağlık bu kuruluş için oluşturulan bildirim çözüm akışı değişiklik anlatılmaktadır. 

## <a name="project-requirements"></a>Proje gereksinimleri
- XML biçiminde HL7 birleştirilmiş Klinik belge mimarisi (C-CDA) belgeleri sağlayıcıları gönderin. C CDA belgeleri neredeyse her belge türü, klinik belgelerini ailesi geçmişlerini ve immunization kayıtları, de olarak yönetim gibi iş akışı ve finansal dahil olmak üzere Klinik kapsar. 
- C CDA belgeleri dönüştürülür [HL7 FHIR kaynakları](http://hl7.org/fhir/2017Jan/resourcelist.html) JSON biçiminde.
- Değiştirilen FHIR kaynak belgeler, JSON biçiminde e-postayla gönderilir.

## <a name="solution-workflow"></a>Çözümü iş akışı 

Yüksek düzeyde, aşağıdaki iş akışı adımları proje gerekli: 
1. C CDA belgeleri FHIR kaynaklara dönüştürün.
2. Değiştirilen FHIR kaynaklar için yoklama yinelenen tetikleyici gerçekleştirin. 
2. Özel bir uygulama, Azure Cosmos DB ve yeni veya değiştirilmiş belgeler için sorgu bağlanmak için FhirNotificationApi çağırın.
3. Service Bus kuyruğuna yanıt kaydedin.
4. Service Bus kuyruğuna yeni iletiler için yoklama.
5. E-posta bildirimleri için hastalar gönderin.

## <a name="solution-architecture"></a>Çözüm mimarisi
Bu çözüm, yukarıdaki gereksinimlerini karşılamak ve çözümü iş akışı tamamlamak üç Logic Apps gerektirir. Üç mantıksal uygulamalar şunlardır:
1. **HL7 FHIR eşleme uygulama**: HL7 C-CDA belge alır, FHIR kaynağa dönüştürür ve ardından Azure Cosmos Veritabanına kaydeder.
2. **EHR uygulama**: Azure Cosmos DB FHIR depo sorgular ve Service Bus kuyruğuna yanıtı kaydeder. Bu mantıksal uygulama kullanan bir [API uygulaması](#api-app) yeni ve değiştirilmiş belgeleri alınamadı.
3. **İşlem bildirim uygulama**: gövdesinde FHIR kaynak belgeleri içeren bir e-posta bildirimi gönderir.

![Bu HL7 FHIR sağlık çözümde kullanılan üç Logic Apps](./media/change-feed-hl7-fhir-logic-apps/health-care-solution-hl7-fhir.png)



### <a name="azure-services-used-in-the-solution"></a>Çözümde kullanılan azure Hizmetleri

#### <a name="azure-cosmos-db-sql-api"></a>Azure Cosmos DB SQL API
Azure Cosmos DB FHIR kaynaklar için aşağıdaki resimde gösterildiği gibi depodur.

![Bu HL7 FHIR sağlık öğreticide kullanılan Azure Cosmos DB hesabı](./media/change-feed-hl7-fhir-logic-apps/account.png)

#### <a name="logic-apps"></a>Logic Apps
Mantıksal uygulamalar iş akışı işlemi işleyin. Aşağıdaki ekran görüntüleri, bu çözüm için oluşturulan Logic apps gösterir. 


1. **HL7 FHIR eşleme uygulama**: HL7 C-CDA belge almak ve Logic Apps için Kurumsal tümleştirme paketi kullanarak bir FHIR kaynağa dönüştürün. Enterprise Integration Pack C CDA eşlemeden FHIR kaynaklara işler.

    ![HL7 FHIR sağlık kayıtları almak için kullanılan mantıksal uygulama](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-logic-apps-json-transform.png)


2. **EHR uygulama**: Azure Cosmos DB FHIR depoyu sorgu ve yanıt Service Bus kuyruğuna kaydedin. GetNewOrModifiedFHIRDocuments uygulama kodunu aşağıdadır.

    ![Mantıksal uygulama Azure Cosmos DB sorgulamak için kullanılır](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-logic-apps-api-app.png)

3. **İşlem bildirim uygulama**: gövdesinde FHIR kaynak belgeleri içeren bir e-posta bildirimi gönder.

    ![HL7 FHIR kaynakla hasta e-posta gövdesinde gönderir mantıksal uygulama](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-logic-apps-send-email.png)

#### <a name="service-bus"></a>Service Bus
Aşağıdaki şekilde gösterilmiştir hastalar sırası. Etiket özelliği değeri, e-posta konusu için kullanılır.

![Bu HL7 FHIR öğreticide kullanılan hizmet veri yolu kuyruğu](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-service-bus-queue.png)

<a id="api-app"></a>

#### <a name="api-app"></a>API uygulaması
Bir API uygulaması Azure Cosmos DB ve kaynak türüne göre yeni veya değiştirilmiş FHIR belgeler için sorgular bağlanır. Bu uygulamanın bir denetleyici yok **FhirNotificationApi** tek bir işlemle **GetNewOrModifiedFhirDocuments**, bkz: [API uygulaması için kaynak](#api-app-source).

Kullanıyoruz [ `CreateDocumentChangeFeedQuery` ](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery.aspx) Azure Cosmos DB SQL .NET API sınıfından. Daha fazla bilgi için bkz: [değişiklik akış makale](change-feed.md). 

##### <a name="getnewormodifiedfhirdocuments-operation"></a>GetNewOrModifiedFhirDocuments işlemi

**Girişleri**
- Databaseıd
- CollectionId
- HL7 FHIR kaynak türü adı
- Boolean: Başlangıçtan itibaren Başlat
- Int: Döndürülen belgelerin sayısı

**Çıkışları**
- BAŞARI: Durum kodu: 200, yanıt: belgeleri (JSON dizisi) listesi
- Hata: Durum kodu: 404, yanıt: "hiçbir belge bulundu '*kaynak adı '* kaynak türü"

<a id="api-app-source"></a>

**API uygulaması için kaynak**

```C#

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
                "Query for new or modifed FHIR Documents By Resource Type " +
                "from Last Run Date or Begining of Collection creation"
            )]
            [SwaggerResponse(HttpStatusCode.OK, type: typeof(Task<dynamic>))]
            [SwaggerResponse(HttpStatusCode.NotFound, "No New or Modifed Documents found")]
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

Aşağıdaki resimde nasıl swagger için test etmek için kullanılan gösterir [FhirNotificationApi](#api-app-source).

![API uygulaması test etmek için kullanılan Swagger dosyası](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-testing-app.png)


### <a name="azure-portal-dashboard"></a>Azure portalı panosunun

Aşağıdaki resimde tümünü Azure portalında çalışan bu çözüm için Azure hizmetleri gösterir.

![Bu HL7 FHIR öğreticide kullanılan tüm hizmetleri gösteren Azure portalı](./media/change-feed-hl7-fhir-logic-apps/hl7-fhir-portal.png)


## <a name="summary"></a>Özet

- Azure Cosmos DB yerel suppport bildirimleri için yeni veya değiştirilen belgeler ve kullanmak için ne kadar kolay olduğunu olduğunu öğrendiniz. 
- Logic Apps yararlanarak, herhangi bir kod yazmak zorunda kalmadan iş akışları oluşturabilirsiniz.
- HL7 FHIR belgeler için dağıtım işlemek için Azure Service Bus kuyruklarını kullanma.

## <a name="next-steps"></a>Sonraki adımlar
Azure Cosmos DB hakkında daha fazla bilgi için bkz: [Azure Cosmos DB giriş sayfası](https://azure.microsoft.com/services/cosmos-db/). Logic Apps hakkında daha fazla bilgi için bkz: [Logic Apps](https://azure.microsoft.com/services/logic-apps/).


