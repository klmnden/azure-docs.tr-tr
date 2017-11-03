---
title: "Bir Azure Cosmos DB giriş bağlamaya sahip bir HTTP tetikleyicisi oluştur | Microsoft Docs"
description: "Azure işlevleri olan HTTP tetikleyici sorgu Azure Cosmos DB kullanmayı öğrenin."
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: mvc
ms.date: 09/25/2017
ms.author: mimig
ms.openlocfilehash: 86a660309fd3fd80f10f706ff460af2309c12174
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-azure-functions-http-trigger-with-an-azure-cosmos-db-input-binding"></a>Bir Azure Cosmos DB giriş bağlaması ile bir Azure işlevleri HTTP tetikleyicisi oluşturun

Azure Cosmos DB şemasız ve sunucusuz Genel dağıtılmış, birden çok model bir veritabanıdır. Azure işlevi kodu isteğe bağlı çalıştırmanıza olanak sağlayan bir sunucusuz hesaplama hizmetidir. Bu iki Azure Hizmetleri eşleştirin ve harika uygulamaları oluşturmaya odaklanmanıza olanak sağlayan sunucusuz bir mimari için temel varsa ve sağlama ve sunucuları, hesaplama için koruma hakkında endişelenmeniz değil ve veritabanının gerekiyor.

Bu öğreticide oluşturduğunuz kodu derlemeler [.NET için grafik API'si Hızlı Başlangıç](create-graph-dotnet.md). Bu öğretici içeren bir Azure işlevi ekler bir [HTTP tetikleyicisini](https://github.com/MicrosoftDocs/azure-docs-pr/azure-functions/functions-bindings-http-webhook.md#http-trigger). Bir Azure Cosmos DB HTTP tetikleyicisi kullanan [bağlama giriş](https://github.com/MicrosoftDocs/azure-docs-pr/azure-functions/functions-triggers-bindings.md) başlangıcın oluşturulan grafik veritabanından veri almak için. Bu belirli HTTP tetikleyicisi sorgularında Azure Cosmos DB veriler, ancak Azure Cosmos DB giriş bağlamaları ne olursa olsun, işlev gerektirir için giriş değerleri veri almak için kullanılabilir.

Bu öğretici, aşağıdaki görevleri içerir:

> [!div class="checklist"]
> * Bir Azure işlevi projesi oluşturma 
> * Bir HTTP tetikleyicisi oluşturma
> * Azure işlevi yayımlama
> * Azure işlevi Azure Cosmos DB veritabanına bağlan

## <a name="prerequisites"></a>Ön koşullar

- **Azure geliştirme** iş yükü dahil [Visual Studio 2017 sürüm 15.3](https://www.visualstudio.com/vs/preview/).

    ![Azure geliştirme iş yüküyle Visual Studio 2017’yi yükleyin](./media/tutorial-functions-http-trigger/functions-vs-workloads.png)
    
- Yükleme veya Visual Studio 2017 sürüm 15.3 yükselttikten sonra Azure işlevleri için Visual Studio 2017 araçları el ile güncelleştirmeniz gerekir. Araçları'ndan güncelleştirebilirsiniz **Araçları** menüsünün altında **Uzantılar ve güncelleştirmeler...**   >  **Güncelleştirmeleri** > **Visual Studio Marketi'ine** > **Azure işlevleri ve Web Araçları işleri**  >  **Güncelleştirme**.

- Tamamlamak [grafik API'sini kullanarak bir .NET uygulaması oluşturma](tutorial-develop-graph-dotnet.md) öğretici veya örnek kod get [azure-cosmos-db-graph-dotnet-getting-started](https://github.com/Azure-Samples/azure-cosmos-db-graph-dotnet-getting-started) GitHub deposuna ve projeyi oluşturun.
 
## <a name="build-a-function-in-visual-studio"></a>Visual Studio'da bir işlev oluşturun

1. Ekle bir **Azure işlevleri** çözüm düğümünde sağ tıklanarak çözümünüzü projeye **Çözüm Gezgini**, choose **Ekle**  >   **Yeni proje**. Seçin **Azure işlevleri** iletişim kutusundan kutusunda ve adlandırın **PeopleDataFunctions**.

   ![Bir Azure işlevi proje çözüme ekleyin](./media/tutorial-functions-http-trigger/01-add-function-project.png)

2. Sonra Azure işlevleri projesi oluşturmak, ilgili birkaç NuGet güncelleştirir ve gerçekleştirmek için yükler. 

    a. Son işlevleri SDK'sını sahip olduğunuzdan emin olmak için güncelleştirmek için NuGet yöneticisini kullanın **Microsoft.NET.Sdk.Functions** paket. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve seçin **NuGet paketlerini Yönet**. İçinde **yüklü** sekmesinde, Microsoft.NET.Sdk.Functions seçin ve ardından **güncelleştirme**.

   ![NuGet paketlerini güncelleştirme](./media/tutorial-functions-http-trigger/02-update-functions-sdk.png)

    b. İçinde **Gözat** sekmesinde, girin **azure.graphs** bulmak için **Microsoft.Azure.Graphs** paketini ve ardından **yükleme**. Bu paket Graph API .NET İstemci SDK'sı içerir.

   ![Grafik API'si yükleyin](./media/tutorial-functions-http-trigger/03-add-azure-graphs.png)

    c. İçinde **Gözat** sekmesinde, girin **mono.csharp** bulmak için **Mono.CSharp** paketini ve ardından **yükleme**.

   ![Mono.CSharp yükleyin](./media/tutorial-functions-http-trigger/04-add-mono.png)

3. Çözüm Gezgini şimdi aşağıda gösterildiği gibi yüklü paketleri içermesi gerekir. 
   
   Ardından, ekleyeceğiz yeni bir şekilde biraz kod yazmaya ihtiyacımız **Azure işlevi** proje öğesi. 

    a. **Çözüm Gezgini**'nde proje düğümüne sağ tıklayın, ardından **Ekle** > **Yeni Öğe** seçeneğini belirleyin.   
    b. İçinde **Yeni Öğe Ekle** iletişim kutusunda **Visual C# öğeleri**seçin **Azure işlevi**, türü **arama** , projenizin adı olarak ve ardından tıklatın **Ekle**.  
 
   ![Arama adlı yeni bir işlev oluşturun](./media/tutorial-functions-http-trigger/05-add-function.png)

4. Http tetikleyicisi şablon burada uygun olacak şekilde Azure işlevi HTTP isteklerine yanıt verir.
   
   İçinde **yeni Azure işlevi** kutusunda **Http tetikleyicisini**. Ayarlarız şekilde bu Azure işlevin çok, "geniş açın," olmasını istiyoruz **erişim hakları** için **anonim**, olanak sağlayan herkesin. **Tamam** düğmesine tıklayın.

   ![Anonim erişim hakları ayarlama](./media/tutorial-functions-http-trigger/06-http-trigger.png)

5. Azure işlevi projeye Search.cs ekledikten sonra bu kopyalama **kullanarak** üzerinden deyimleri using deyimleri var:

   ```csharp
   using Microsoft.Azure.Documents;
   using Microsoft.Azure.Documents.Client;
   using Microsoft.Azure.Documents.Linq;
   using Microsoft.Azure.Graphs;
   using Microsoft.Azure.WebJobs;
   using Microsoft.Azure.WebJobs.Extensions.Http;
   using Microsoft.Azure.WebJobs.Host;
   using System;
   using System.Collections.Generic;
   using System.Configuration;
   using System.Linq;
   using System.Net;
   using System.Net.Http;
   using System.Threading.Tasks;
   ```

6. Ardından, Azure işlevin sınıfı kodu aşağıdaki kodla değiştirin. Kod tarafından tanımlanan belirli kişi veya ya da tüm kişiler için grafik API'sini kullanarak Azure Cosmos DB veritabanı arar `name` sorgu dizesi parametresi.

   ```csharp
   public static class Search
   {
       static string endpoint = ConfigurationManager.AppSettings["Endpoint"];
       static string authKey = ConfigurationManager.AppSettings["AuthKey"];

       [FunctionName("Search")]
       public static async Task<HttpResponseMessage> Run(
           [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]HttpRequestMessage req,
           TraceWriter log)
       {
           log.Info("C# HTTP trigger function processed a request.");

           // the person objects are free-form in structure
           List<dynamic> results = new List<dynamic>();

           // open the client's connection
           using (DocumentClient client = new DocumentClient(
               new Uri(endpoint),
               authKey,
               new ConnectionPolicy
               {
                   ConnectionMode = ConnectionMode.Direct,
                   ConnectionProtocol = Protocol.Tcp
               }))
           {
               // get a reference to the database the console app created
               Database database = await client.CreateDatabaseIfNotExistsAsync(
                   new Database
                   {
                       Id = "graphdb"
                   });

               // get an instance of the database's graph
               DocumentCollection graph = await client.CreateDocumentCollectionIfNotExistsAsync(
                   UriFactory.CreateDatabaseUri("graphdb"),
                   new DocumentCollection { Id = "graphcollz" },
                   new RequestOptions { OfferThroughput = 1000 });

               // build a gremlin query based on the existence of a name parameter
               string name = req.GetQueryNameValuePairs()
                   .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
                   .Value;

               IDocumentQuery<dynamic> query = (!String.IsNullOrEmpty(name))
                   ? client.CreateGremlinQuery<dynamic>(graph, string.Format("g.V('{0}')", name))
                   : client.CreateGremlinQuery<dynamic>(graph, "g.V()");

               // iterate over all the results and add them to the list
               while (query.HasMoreResults)
                   foreach (dynamic result in await query.ExecuteNextAsync())
                       results.Add(result);
           }

           // return the list with an OK response
           return req.CreateResponse<List<dynamic>>(HttpStatusCode.OK, results);
       }
   }
   ```

   Temel veritabanıyla eşleşen kayıtları almak için basit bir sorgu sağlanmış özgün konsol uygulaması olduğu gibi aynı bağlantı mantığı kodudur.

## <a name="debug-the-azure-function-locally"></a>Azure işlevi yerel olarak hata ayıklama

Kod tamamlandığına göre yerel olarak test etmek için kodu çalıştırmak için Azure işlevin yerel hata ayıklama araçları ve öykünücüsü kullanabilirsiniz.

1. Kodu düzgün çalıştırmadan önce Azure Cosmos DB bağlantı bilgilerinizi ile yerel yürütme için yapılandırmanız gerekir. Yerel yürütme için Azure işlevi çok aynı şekilde yapılandırmak için local.settings.json dosyasını kullanabilirsiniz kullandığınız App.config dosyasını yürütme için özgün konsol uygulamasını yapılandırmak üzere gibi.

    Bunu yapmak için local.settings.json için kod aşağıdaki satırları ekleyin ve uç nokta ve aşağıdaki görüntüde gösterildiği gibi GraphGetStarted projesinde App.Config dosyasından AuthKey kopyalayın.

   ```json
    "Endpoint": "",
    "AuthKey": ""
    ```

   ![Uç nokta ve yetkilendirme anahtarı local.settings.json dosyasında ayarlayın](./media/tutorial-functions-http-trigger/07-local-functions-settings.png)

2. Başlangıç projesi yeni işlevler uygulamasına değiştirin. İçinde **Çözüm Gezgini**, sağ **PeopleDataFunctions**seçip **başlangıç projesi olarak ayarla**.

3. İçinde **Çözüm Gezgini**, sağ tıklatın **bağımlılıkları** içinde **PeopleDataFunctions** proje ve ardından **Başvuru Ekle**. Listeden System.Configuration seçin ve ardından **Tamam**.

3. Şimdi uygulamayı şimdi çalıştırın. Yerel hata ayıklama aracını barındırılan ve kullanılmaya hazır Azure işlevi koduyla func.exe başlatmak için F5 tuşuna basın.

   Func.exe ilk çıkışı sonunda Azure işlevi localhost:7071 barındırılan bakın. Bu, bir istemcinin, test etmek yararlıdır.

   ![İstemci testi](./media/tutorial-functions-http-trigger/08-functions-emulator.png)

4. Azure işlevi sınamak için kullanın [Visual Studio Code](http://code.visualstudio.com/) Huachao Mao'nın uzantılı [REST istemcisi](https://marketplace.visualstudio.com/items?itemName=humao.rest-client). REST istemcisi tek bir sağ yerel veya uzak HTTP isteği yeteneği sunar. 

    Bunu yapmak için test işlevi locally.http adlı yeni bir dosya oluşturun ve aşağıdaki kodu ekleyin:

    ```http
    get http://localhost:7071/api/Search

    get http://localhost:7071/api/Search?name=ben
   ```

    Şimdi kodun ilk satırı sağ tıklayın ve ardından seçin **İsteği Gönder** aşağıdaki görüntüde gösterildiği gibi.

   ![Visual Studio koddan bir REST İsteği Gönder](./media/tutorial-functions-http-trigger/09-rest-client-in-vs-code.png)

   JSON gövdesi içeriği yerel olarak çalışan Azure işlevi üstbilgileri ham HTTP yanıtının ile sunulan her şeyi.

   ![REST yanıt](./media/tutorial-functions-http-trigger/10-general-results.png)

5. Şimdi ikinci kod satırı seçin ve ardından **İsteği Gönder**. Ekleyerek `name` sorgu dizesi parametresi veritabanında olduğu bilinen bir değerle biz Azure işlevi döndürür sonuçları filtreleyebilir.

   ![Azure işlevinin sonuçlarını filtreleme](./media/tutorial-functions-http-trigger/11-search-for-ben.png)

Azure işlevi doğrulanır ve düzgün çalışıyor gibi görünüyor sonra Azure App Service'te yayımlama ve bulutta çalışacak şekilde yapılandırmak için son adımı olur.

## <a name="publish-the-azure-function"></a>Azure işlevi yayımlama

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve sonra seçin **Yayımla**.

   ![Yeni Proje Yayımlama](./media/tutorial-functions-http-trigger/12-publish-function.png)

2. Biz bu genel kullanıma açık bir senaryoda test etmek için bulut yayımlamak hazırsınız. İçinde **Yayımla** sekmesine **Azure işlev uygulaması**seçin **Yeni Oluştur** Azure aboneliğinizde bir Azure işlevi oluşturmak için ardından **Yayımla** .

   ![Yeni bir Azure işlevi uygulaması oluşturma](./media/tutorial-functions-http-trigger/13-publish-panel.png)

3. İçinde **Yayımla** iletişim kutusunda, aşağıdakileri yapın:
   
    a. İçinde **App Name**, işlevi benzersiz bir ad verin.

    b. İçinde **abonelik**, kullanılacak Azure aboneliğini seçin.
   
    c. İçinde **kaynak grubu**, yeni bir kaynak grubu oluşturun ve uygulama adı olarak aynı adı kullanın.
   
    d. İçin **App Service planı**, tıklatın **yeni** biz sunucusuz Azure işlevi için kullanım başına ödeme fatura yöntemi kullanmayı düşündüğünüz bir yeni tüketim tabanlı uygulama hizmeti planı oluşturmak için. Varsayılanları kullanın **App Service planı Yapılandır** sayfasında ve ardından **Tamam**.
   
    e. İçin **depolama hesabı**,'ı da **yeni** durumunda her zamankinden BLOB'lar, tablolar veya sıralara diğer işlevleri yürütülmesini tetiklemek desteği ihtiyacımız olan Azure işlevi kullanmak için yeni bir depolama hesabı oluşturmak için. Varsayılanları kullanın **depolama hesabı** sayfasında ve ardından **Tamam**.

    f. Ardından **oluşturma** Azure aboneliğinizdeki tüm kaynakları oluşturmak için iletişim kutusunda düğme. Visual Studio Azure işlevi kodunuzun bir sonraki yayımladığınızda kullanan bir yayımlama profili (basit bir XML dosyası) yükler.

   ![Depolama hesabı oluşturma](./media/tutorial-functions-http-trigger/14-new-function-app.png)

    Visual Studio sonra işlev için değişiklikleri yapın ve yeniden yayımlayabilirsiniz gerekiyorsa kullanabileceğiniz bir yayımlama sayfası görüntüler. Bu sayfada artık herhangi bir eylemde bulunmanız gerekmez.

4. Azure işlevi yayımlandıktan sonra gidebilirsiniz [Azure portal](https://portal.azure.com/) Azure işlevinizi sayfası. Burada, Azure işlevin bağlantısını görebilirsiniz **uygulama ayarları**. Kişi verilerinizle Canlı Azure işlevi için Azure Cosmos DB veritabanı bağlantısını yapılandırmak için bu bağlantıyı açın.

   ![Uygulama ayarları gözden geçirin](./media/tutorial-functions-http-trigger/15-function-in-portal.png)

5. Daha önce konsol uygulamanın App.config dosyasında ve Azure işlevi uygulamanın local.settings.json dosya yaptığınız gibi uç nokta ve AuthKey yayımlanan işlevi Azure Cosmos DB veritabanına eklemeniz gerekir. Bu şekilde, hiçbir zaman anahtarlarınızı içeren yapılandırma kodunuzu iade zorunda - portalında yapılandırın ve kaynak denetiminde depolandıkları değil emin olun. Her değer eklemek için tıklatın **yeni ayar Ekle** düğmesini tıklatın, eklemek **Endpoint** ve ardından, app.config, değerinden **yeni ayar eklemeye** yeniden ekleyin **AuthKey**  ve, özel bir değer. Ekledikten ve değerleri kaydettikten sonra ayarlarınızı aşağıdaki gibi görünmelidir.

   ![Uç nokta ve AuthKey yapılandırın](./media/tutorial-functions-http-trigger/16-app-settings.png)

6. Azure işlevi, Azure aboneliğinizde düzgün şekilde yapılandırıldıktan sonra genel kullanıma açık Azure işlevi URL sorgulamak için Visual Studio kod REST istemci uzantısı yeniden kullanabilirsiniz. Test-işlevi-locally.http için bu iki kod satırı ekleyin ve ardından bu işlevi sınamak için her bir satır çalıştırın. URL'deki işlevin adını işlevinizi adıyla değiştirin.

    ```json
    get https://peoplesearchfunction.azurewebsites.net/api/Search

    get https://peoplesearchfunction.azurewebsites.net/api/Search?name=thomas
    ```

    İşlev Azure Cosmos Veritabanından alınan veri ile yanıt verir.

    ![REST istemcisi Azure işlevi sorgulamak için kullanın](./media/tutorial-functions-http-trigger/17-calling-function-from-code.png)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdakileri yaptığınızdan:

> [!div class="checklist"]
> * Bir Azure işlevi proje oluşturuldu 
> * Bir HTTP tetikleyicisi oluşturuldu
> * Yayımlanan Azure işlevi
> * Azure Cosmos DB veritabanına işlevi

Şimdi Cosmos DB hakkında daha fazla bilgi için kavramları bölümüne geçebilirsiniz.

> [!div class="nextstepaction"]
> [Genel dağıtım](distribute-data-globally.md) 

Bu makalede, bir blog dayalı [Brady Gaster'ın Schemaless & sunucusuz](http://www.bradygaster.com/category/%20Serverless%20&%20Schemaless) blog dizisini. Kendi Web günlüğü serisi ek gönderilerde için ziyaret edin.
