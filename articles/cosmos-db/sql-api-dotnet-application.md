---
title: 'Azure Cosmos DB için ASP.NET MVC Öğreticisi: Web uygulaması geliştirme'
description: Azure Cosmos DB'yi kullanarak bir MVC web uygulaması oluşturmak için hazırlanan ASP.NET MVC öğreticisi. Azure Web Siteleri'nde barındırılan bir yapılacaklar uygulamasında JSON ve erişim verilerini depolayacaksınız - adım adım ASP.NET MVC öğreticisi.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 05/21/2019
ms.author: sngun
ms.openlocfilehash: 15cf3b1316cc35e22538ca353302c4a82d2d418b
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65979030"
---
# <a name="_Toc395809351"></a>ASP.NET MVC Öğreticisi: Azure Cosmos DB ile Web uygulaması geliştirme

> [!div class="op_single_selector"]
> * [.NET](sql-api-dotnet-application.md)
> * [Java](sql-api-java-application.md)
> * [Node.js](sql-api-nodejs-application.md)
> * [Python](sql-api-python-application.md)
> * [Xamarin](mobile-apps-with-xamarin.md)
> 

Bu makale, JSON belgelerini depolama ve sorgulama amacıyla Azure Cosmos DB'yi nasıl verimli bir şekilde kullanabileceğinizi vurgulamak için, Azure Cosmos DB kullanarak bir yapılacaklar uygulamasının nasıl oluşturulacağını gösteren uçtan uca bir kılavuz sağlar. Görevler, JSON belgeleri olarak Azure Cosmos DB'de depolanır.

![Yapılacaklar listesi MVC web uygulaması Bu öğretici - ASP NET MVC adım adım Öğreticisi tarafından oluşturulan ekran görüntüsü](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-image01.png)

Bu adım adım kılavuz, Azure Cosmos DB hizmetinin, Azure üzerinde barındırılan bir ASP.NET MVC web uygulamasında verileri depolamak ve bunlara erişmek için nasıl kullanılacağını gösterir. ASP.NET MVC bileşenleri yerine yalnızca Azure Cosmos DB'ye odaklanan bir öğretici arıyorsanız bkz. [Azure Cosmos DB C# konsol uygulaması oluşturma](sql-api-get-started.md).

> [!TIP]
> Bu öğretici, ASP.NET MVC ve Azure Web Siteleri'ni kullanma konusunda deneyim sahibi olduğunuzu varsayar. ASP.NET veya [önkoşul araçlarında](#_Toc395637760) yeniyseniz [GitHub][GitHub] konumundan örnek projenin tamamını indirmenizi ve bu örnekteki yönergeleri uygulamanızı öneririz. Oluşturduktan sonra, proje bağlamında kodu daha iyi kavramak için bu makaleyi inceleyebilirsiniz.
> 
> 

## <a name="_Toc395637760"></a>Bu veritabanı öğreticisi için önkoşullar
Bu makaledeki yönergeleri uygulamadan önce aşağıdakilere sahip olduğunuzdan emin olmanız gerekir:

* Etkin bir Azure hesabı.  Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [!INCLUDE [cosmos-db-emulator-vs](../../includes/cosmos-db-emulator-vs.md)]  
* Visual Studio 2017 için .NET için Microsoft Azure SDK’sına Visual Studio Yükleyicisi ile erişilebilir.

Bu makaledeki tüm ekran görüntüleri, Microsoft Visual Studio Community 2017 kullanılarak alınmıştır. Sisteminiz farklı bir sürümle yapılandırılmışsa ekranlarınızın ve seçeneklerinizin tamamen eşleşmeme olasılığı bulunur ancak yukarıdaki önkoşulları karşılarsanız bu çözümün çalışması gerekir.

## <a name="_Toc395637761"></a>1. adım: Bir Azure Cosmos DB veritabanı hesabı oluşturma
İlk olarak bir Azure Cosmos DB hesabı oluşturalım. Zaten Azure Cosmos DB için bir hesabınız varsa veya bu öğretici için Azure Cosmos DB Öykünücüsü’nü kullanıyorsanız [Yeni bir ASP.NET MVC uygulaması oluşturma](#_Toc395637762) adımına atlayabilirsiniz.

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [keys](../../includes/cosmos-db-keys.md)]

<br/>
Şimdi yeni bir ASP.NET MVC uygulamasının nasıl oluşturulacağını en başından başlayarak inceleyeceğiz. 

## <a name="_Toc395637762"></a>2. adım: Yeni bir ASP.NET MVC uygulaması oluşturma

1. Visual Studio'da, **Dosya** menüsündeki **Yeni** seçeneğine gidin ve ardından **Proje**'ye tıklayın. **Yeni Proje** iletişim kutusu görünür.

2. **Proje türleri** bölmesinde **Şablonlar**, **Visual C#**, **Web**'i genişletin ve ardından **ASP.NET Web Uygulaması**'nı seçin.

      ![Vurgulanan ASP.NET Web uygulaması proje türü ile yeni proje iletişim kutusunun ekran görüntüsü](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-new-project-dialog.png)

3. **Ad** kutusunda projenin adını yazın. Bu öğretici "todo" adını kullanır. Bunun dışında bir şey kullanmayı seçerseniz bu öğreticinin todo ad alanından söz ettiği her yerde sağlanan kod örneklerini uygulamanıza verdiğiniz ada göre ayarlamanız gerekir. 
4. Projeyi oluşturmak istediğiniz klasöre gitmek için **Gözat**'a tıklayın ve ardından **Tamam**'a tıklayın.
   
      **Yeni ASP.NET Web Uygulaması** iletişim kutusu görüntülenir.
   
    ![MVC uygulama şablonu vurgulanmış ile yeni bir ASP.NET Web uygulaması iletişim kutusunun ekran görüntüsü](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-MVC.png)
5. Şablonlar bölmesinde **MVC**'yi seçin.

6. **Tamam**'a tıklayarak Visual Studio'nun boş ASP.NET MVC şablonu çevresinde iskele oluşturmasını sağlayın. 

          
7. Visual Studio demirbaş MVC uygulamasını oluşturmayı tamamlandıktan sonra, yerel olarak çalıştırabileceğiniz boş bir ASP.NET uygulamasına sahip olursunuz.
   
    Hepimizin ASP.NET "Hello World" uygulamasını gördüğünden emin olduğum için, projeyi yerel olarak çalıştırmayı atlayacağız. Şimdi doğrudan bu projeye Azure Cosmos DB eklemeye ve uygulamamızı oluşturmaya geçelim.

## <a name="_Toc395637767"></a>3. adım: MVC web uygulaması projenize Azure Cosmos DB ekleme
Bu çözüm için gereken ASP.NET MVC altyapısının çoğunu elde ettiğimize göre, bu öğreticinin asıl amacı olan MVC web uygulamamıza Azure Cosmos DB'yi eklemeye geçelim.

1. Azure Cosmos DB .NET SDK’sı, bir NuGet paketi olarak paketlenir ve dağıtılır. NuGet paketini Visual Studio'da almak için, **Çözüm Gezgini**'nde projeye sağ tıklayarak ve ardından **NuGet Paketlerini Yönet**'e tıklayarak Visual Studio'daki NuGet paket yöneticisini kullanın.
   
    ![NuGet paketlerini Yönet vurgulanmış ile Çözüm Gezgini'nde web uygulaması projesi için sağ tıklama seçeneklerinin ekran görüntüsü.](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-manage-nuget.png)
   
    **NuGet Paketlerini Yönet** iletişim kutusu görünür.
2. NuGet **Gözat** kutusunda ***Azure DocumentDB*** yazın. (Paket adı, Azure Cosmos DB olarak güncelleştirilmemiştir.)
   
    Sonuçlardan **Microsoft’a ait Microsoft.Azure.DocumentDB** paketini yükleyin. Bu işlem, Azure Cosmos DB paketini ve aynı zamanda Newtonsoft.Json gibi tüm bağımlılıkları indirir ve yükler. **Önizleme** penceresinde **Tamam**'a tıklayıp **Lisans Kabulü** penceresinde **Kabul Ediyorum**'a tıklayarak yüklemeyi tamamlayın.
   
    ![Microsoft Azure Cosmos DB istemci kitaplığı ile NuGet paketlerini Yönet penceresinin Sreenshot](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-install-nuget.png)
   
      Alternatif olarak, paketi yüklemek için Paket Yöneticisi Konsolu'nu kullanabilirsiniz. Bunu yapmak için, **Araçlar** menüsünde **NuGet Paket Yöneticisi**'ne tıklayın ve ardından **Paket Yöneticisi Konsolu**'na tıklayın. İstendiğinde aşağıdakileri yazın.
   
        Install-Package Microsoft.Azure.DocumentDB
        
3. Paket yüklendikten sonra, Visual Studio çözümünüz Microsoft.Azure.Documents.Client ve Newtonsoft.Json olmak üzere iki yeni başvuru eklenmiş şekilde aşağıdakine benzemelidir.
   
    ![Çözüm Gezgini'nde JSON veri projesine eklenen iki başvurunun ekran görüntüsü](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-added-references.png)

## <a name="_Toc395637763"></a>4. adım: ASP.NET MVC uygulamasını ayarlama
Şimdi bu MVC uygulamasına modeller, görünümler ve denetleyiciler ekleyelim:

* [Model ekleme](#_Toc395637764).
* [Denetleyici ekleme](#_Toc395637765).
* [Görünümler ekleme](#_Toc395637766).

### <a name="_Toc395637764"></a>JSON veri modeli ekleme
MVC'de **M** olan modeli oluşturarak başlayalım. 

1. **Çözüm Gezgini**'nde **Modeller** klasörüne sağ tıklayın, **Ekle**'ye tıklayın ve ardından **Sınıf**'a tıklayın.
   
      **Yeni Öğe Ekle** iletişim kutusu görünür.
2. Yeni sınıfınıza **Item.cs** adını verin ve **Ekle**'ye tıklayın. 
3. Bu yeni **Item.cs** dosyasında, son *using deyiminden* sonra aşağıdakini ekleyin.
   
        using Newtonsoft.Json;
4. Şimdi de bu kodu 
   
        public class Item
        {
        }
   
    aşağıdaki kodla değiştirin.
   
        public class Item
        {
            [JsonProperty(PropertyName = "id")]
            public string Id { get; set; }
   
            [JsonProperty(PropertyName = "name")]
            public string Name { get; set; }
   
            [JsonProperty(PropertyName = "description")]
            public string Description { get; set; }
   
            [JsonProperty(PropertyName = "isComplete")]
            public bool Completed { get; set; }
        }
   
    Azure Cosmos DB'deki tüm veriler kablo üzerinden geçer ve JSON olarak depolanır. JSON.NET tarafından nesnelerinizin seri hale getirilme/seri durumundan çıkarılma yolunu denetlemek için, yeni oluşturduğumuz **Item** sınıfında gösterildiği şekilde **JsonProperty** özniteliğini kullanabilirsiniz. Bunu yapmak **zorunda** değilsiniz ancak özelliklerimin JSON camelCase adlandırma kurallarına uyduğundan emin olmak istiyorum. 
   
    Özellik adının biçimini JSON'a gittiği zaman denetlemenin yanı sıra, **Açıklama** özelliğinde yaptığım gibi .NET özelliklerinizi tamamen yeniden adlandırabilirsiniz. 

### <a name="_Toc395637765"></a>Denetleyici ekleme
**M** ile işimiz bitti; şimdi de MVC'deki **C**'yi, yani denetleyici sınıfını oluşturalım.

1. **Çözüm Gezgini**'nde **Denetleyiciler** klasörüne sağ tıklayın, **Ekle**'ye tıklayın ve ardından **Denetleyici**'ye tıklayın.
   
    **İskele Ekle** iletişim kutusu görünür.
2. **MVC 5 Denetleyici - Boş**'u seçin ve ardından **Ekle**'ye tıklayın.
   
    ![MVC 5 denetleyici - boş seçeneği vurgulanmış İskele Ekle iletişim kutusunun ekran görüntüsü](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-controller-add-scaffold.png)
3. Yeni Denetleyicinize **ItemController** adını verin.
   
    ![Denetleyici Ekle iletişim kutusunun ekran görüntüsü](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-add-controller.png)
   
    Dosya oluşturulduktan sonra, Visual Studio çözümünüz **Çözüm Gezgini**'nde yeni ItemController.cs dosyasıyla aşağıdakine benzemelidir. Daha önce oluşturduğunuz yeni Item.cs dosyası da gösterilmektedir.
   
    ![Visual Studio çözümünün - yeni Itemcontroller.cs dosyası ve Item.cs dosyası vurgulanmış şekilde çözüm Gezgini'nin ekran görüntüsü](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-new-item-solution-explorer.png)
   
    ItemController.cs'yi kapatabilirsiniz, buna daha sonra geri döneceğiz. 

### <a name="_Toc395637766"></a>Görünümler ekleme
Şimdi MVC'deki **V**'yi, yani görünümleri oluşturalım:

* [Öğe Dizini görünümü ekleme](#AddItemIndexView).
* [Yeni Öğe görünümü ekleme](#AddNewIndexView).
* [Düzenleme Öğesi görünümü ekleme](#_Toc395888515).

#### <a name="AddItemIndexView"></a>Öğe Dizini görünümü ekleme
1. **Çözüm Gezgini**'nde **Görünümler** klasörünü genişletin, daha önce **ItemController**'ı eklediğinizde Visual Studio'nun sizin için oluşturduğu boş **Öğe** klasörüne sağ tıklayın, **Ekle**'ye tıklayın ve ardından **Görünüm**'e tıklayın.
   
    ![Çözüm Gezgini'nin ekran görüntüsü Görünüm Ekle komutları vurgulanmış Visual Studio'nun oluşturduğu öğe klasörünü gösteren](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-add-view.png)
2. **Görünüm Ekle** iletişim kutusunda aşağıdakileri yapın:
   
   * **Görünüm adı** kutusunda ***Index*** yazın.
   * **Şablon** kutusunda ***Liste***'yi seçin.
   * **Model sınıfı** kutusunda ***Öğe (todo.Models)*** seçeneğini belirleyin.
   * Düzen sayfası kutusunda ***~/Views/Shared/_Layout.cshtml*** yazın.
     
   ![Görünüm Ekle iletişim kutusunu gösteren ekran görüntüsü](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-add-view-dialog.png)
3. Bu değerlerin tümü ayarlandıktan sonra, **Ekle**'ye tıklayın ve Visual Studio'nun yeni bir şablon görünümü oluşturmasına izin verin. Tamamlandığında, oluşturulan cshtml dosyasını açar. Daha sonra geri döneceğimiz için Visual Studio'daki bu dosyayı kapatabiliriz.

#### <a name="AddNewIndexView"></a>Yeni Öğe görünümü ekleme
**Öğe Dizini** görünümü oluşturmamıza benzer şekilde, şimdi de yeni **Öğeler** oluşturmak için yeni bir görünüm oluşturacağız.

1. **Çözüm Gezgini**'nde **Öğe** klasörüne tekrar sağ tıklayın, **Ekle**'ye tıklayın ve ardından **Görünüm**'e tıklayın.
2. **Görünüm Ekle** iletişim kutusunda aşağıdakileri yapın:
   
   * **Görünüm adı** kutusunda ***Create*** yazın.
   * **Şablon** kutusunda ***Oluştur***'u seçin.
   * **Model sınıfı** kutusunda ***Öğe (todo.Models)*** seçeneğini belirleyin.
   * Düzen sayfası kutusunda ***~/Views/Shared/_Layout.cshtml*** yazın.
   * **Ekle**'ye tıklayın.
   
#### <a name="_Toc395888515"></a>Düzenleme Öğesi görünümü ekleme
Son olarak, bir **Öğe**'yi düzenlemek için daha önce kullandığımız yolu tekrarlayarak son bir görünüm ekleyin.

1. **Çözüm Gezgini**'nde **Öğe** klasörüne tekrar sağ tıklayın, **Ekle**'ye tıklayın ve ardından **Görünüm**'e tıklayın.
2. **Görünüm Ekle** iletişim kutusunda aşağıdakileri yapın:
   
   * **Görünüm adı** kutusunda ***Edit*** yazın.
   * **Şablon** kutusunda ***Düzenle***'yi seçin.
   * **Model sınıfı** kutusunda ***Öğe (todo.Models)*** seçeneğini belirleyin.
   * Düzen sayfası kutusunda ***~/Views/Shared/_Layout.cshtml*** yazın.
   * **Ekle**'ye tıklayın.

Bunu yaptıktan sonra, bu görünümlere daha sonra geri döneceğimiz için Visual Studio'daki tüm cshtml belgelerini kapatın.

## <a name="_Toc395637769"></a>5. adım: Azure Cosmos DB'yi bağlama
Standart MVC işleri hallolduğuna göre, Azure Cosmos DB için kod eklemeye dönelim. 

Bu bölümde, aşağıdakileri işlemek için kod ekleyeceğiz:

* [Tamamlanmamış Öğeleri listeleme](#_Toc395637770).
* [Öğeler ekleme](#_Toc395637771).
* [Öğeleri düzenleme](#_Toc395637772).

### <a name="_Toc395637770"></a>MVC web uygulamanızda tamamlanmamış Öğeleri listeleme
Burada yapılacak ilk şey, Azure Cosmos DB'ye bağlanmayı ve kullanmayı sağlayan tüm mantığı içeren bir sınıf eklemektir. Bu öğretici için tüm bu mantığı DocumentDBRepository adlı bir depo sınıfına kapsülleyeceğiz. 

1. **Çözüm Gezgini**'nde projeye sağ tıklayın, **Ekle**'ye tıklayın ve ardından **Sınıf**'a tıklayın. Yeni sınıfa **DocumentDBRepository** adını verin ve **Ekle**'ye tıklayın.
2. Yeni oluşturulan **DocumentDBRepository** sınıfında *ad alanı* bildiriminin üstüne aşağıdaki *using deyimlerini* ekleyin
   
        using Microsoft.Azure.Documents; 
        using Microsoft.Azure.Documents.Client; 
        using Microsoft.Azure.Documents.Linq; 
        using System.Configuration;
        using System.Linq.Expressions;
        using System.Threading.Tasks;
        using System.Net;
        
    Şimdi de bu kodu 
   
        public class DocumentDBRepository
        {
        }
   
    aşağıdaki kodla değiştirin.
   
        public static class DocumentDBRepository<T> where T : class
        {
            private static readonly string DatabaseId = ConfigurationManager.AppSettings["database"];
            private static readonly string CollectionId = ConfigurationManager.AppSettings["collection"];
            private static DocumentClient client;
   
            public static void Initialize()
            {
                client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["endpoint"]), ConfigurationManager.AppSettings["authKey"]);
                CreateDatabaseIfNotExistsAsync().Wait();
                CreateCollectionIfNotExistsAsync().Wait();
            }
   
            private static async Task CreateDatabaseIfNotExistsAsync()
            {
                try
                {
                    await client.ReadDatabaseAsync(UriFactory.CreateDatabaseUri(DatabaseId));
                }
                catch (DocumentClientException e)
                {
                    if (e.StatusCode == System.Net.HttpStatusCode.NotFound)
                    {
                        await client.CreateDatabaseAsync(new Database { Id = DatabaseId });
                    }
                    else
                    {
                        throw;
                    }
                }
            }
   
            private static async Task CreateCollectionIfNotExistsAsync()
            {
                try
                {
                    await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri(DatabaseId, CollectionId));
                }
                catch (DocumentClientException e)
                {
                    if (e.StatusCode == System.Net.HttpStatusCode.NotFound)
                    {
                        await client.CreateDocumentCollectionAsync(
                            UriFactory.CreateDatabaseUri(DatabaseId),
                            new DocumentCollection { Id = CollectionId },
                            new RequestOptions { OfferThroughput = 1000 });
                    }
                    else
                    {
                        throw;
                    }
                }
            }
        }
   
    
3. Bazı değerleri yapılandırmadan okuyoruz, bu nedenle uygulamanızın **Web.config** dosyasını açın ve `<AppSettings>` bölümünün altına aşağıdaki satırları ekleyin.
   
        <add key="endpoint" value="enter the URI from the Keys blade of the Azure Portal"/>
        <add key="authKey" value="enter the PRIMARY KEY, or the SECONDARY KEY, from the Keys blade of the Azure  Portal"/>
        <add key="database" value="ToDoList"/>
        <add key="collection" value="Items"/>
4. Şimdi de Azure Portal'ın Anahtarlar dikey penceresini kullanarak *endpoint* ve *authKey* değerlerini güncelleştirin. Uç nokta ayarının değeri olarak Anahtarlar dikey penceresinden **URI**'yi kullanın ve authKey ayarının değeri olarak Anahtarlar dikey penceresinden **BİRİNCİL ANAHTAR** veya **İKİNCİL ANAHTAR**'ı kullanın.

    Azure Cosmos DB deposunu bağlamayı tamamladık, şimdi de uygulama mantığımızı ekleyelim.

1. Bir yapılacaklar listesi uygulamasıyla yapabilmeyi istediğimiz ilk şey, tamamlanmamış öğeleri görüntülemektir.  **DocumentDBRepository** sınıfının içinde herhangi bir yere aşağıdaki kod parçacığını kopyalayıp yapıştırın.
   
        public static async Task<IEnumerable<T>> GetItemsAsync(Expression<Func<T, bool>> predicate)
        {
            IDocumentQuery<T> query = client.CreateDocumentQuery<T>(
                UriFactory.CreateDocumentCollectionUri(DatabaseId, CollectionId))
                .Where(predicate)
                .AsDocumentQuery();
   
            List<T> results = new List<T>();
            while (query.HasMoreResults)
            {
                results.AddRange(await query.ExecuteNextAsync<T>());
            }
   
            return results;
        }
2. Daha önce eklediğimiz **ItemController**'ı açın ve ad alanı bildiriminin üstüne aşağıdaki *using deyimlerini* ekleyin.
   
        using System.Net;
        using System.Threading.Tasks;
        using todo.Models;
   
    Projeniz "todo" olarak adlandırılmamışsa using "todo.Models" deyimini projenizin adını yansıtacak şekilde güncelleştirmeniz gerekir.
   
    Şimdi de bu kodu
   
        //GET: Item
        public ActionResult Index()
        {
            return View();
        }
   
    aşağıdaki kodla değiştirin.
   
        [ActionName("Index")]
        public async Task<ActionResult> IndexAsync()
        {
            var items = await DocumentDBRepository<Item>.GetItemsAsync(d => !d.Completed);
            return View(items);
        }
3. **Global.asax.cs**'yi açın ve **Application_Start** yöntemine aşağıdaki satırı ekleyin 
   
        DocumentDBRepository<todo.Models.Item>.Initialize();

Bu noktada çözümünüzün herhangi bir hata olmadan oluşturulabiliyor olması gerekir.

Uygulamayı şimdi çalıştırırsanız **HomeController**'a ve söz konusu denetleyicinin **Dizin** görünümüne gidersiniz. Başta seçtiğimiz MVC şablonu projesinin varsayılan davranışı budur ancak biz bunu istemiyoruz! Bu davranışı değiştirmek için bu MVC uygulamasındaki yönlendirmeyi değiştirelim.

***App\_Start\RouteConfig.cs***'yi açın, "defaults:" ile başlayan satırı bulun ve aşağıdakine benzeyecek şekilde değiştirin.

        defaults: new { controller = "Item", action = "Index", id = UrlParameter.Optional }

Bu, yönlendirme davranışını denetlemek için URL'de bir değer belirtmemeniz durumunda, denetleyici olarak **Giriş** yerine **Öğe**'yi ve görünüm olarak **Dizin**'i kullanmasını ASP.NET MVC'ye söyler.

Uygulamayı şimdi çalıştırırsanız uygulama depo sınıfını çağıran ve tamamlanmamış tüm öğeleri **Görünümler**\\**Öğe**\\**Dizin** görünümüne getiren GetItems yöntemini kullanan **ItemController**'ınızı çağırır. 

Bu projeyi şimdi oluşturur ve çalıştırırsanız buna benzeyen bir şey görürsünüz.    

![Bu veritabanı Öğreticisi tarafından oluşturulan Yapılacaklar listesi web uygulamasının ekran görüntüsü](./media/sql-api-dotnet-application/build-and-run-the-project-now.png)

### <a name="_Toc395637771"></a>Öğeler ekleme
Boş bir kılavuzdan başka şeyler görmek için veritabanımıza biraz öğe ekleyelim.

Azure Cosmos DB'deki kaydı kalıcı hale getirmek için Azure Cosmos DBRepository ve ItemController'a biraz kod ekleyelim.

1. **DocumentDBRepository** sınıfınıza aşağıdaki yöntemi ekleyin.
   
       public static async Task<Document> CreateItemAsync(T item)
       {
           return await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(DatabaseId, CollectionId), item);
       }
   
   Bu yöntem kendisine geçirilen bir nesneyi alır ve bunu Azure Cosmos DB’de kalıcı hale getirir.
2. ItemController.cs dosyasını açın ve sınıfın içine aşağıdaki kod parçacığını ekleyin. ASP.NET MVC **Oluştur** eylemi için ne yapacağını bu şekilde bilir. Bu durumda yalnızca daha önce oluşturulan ilişkili Create.cshtml görünümünü işler.
   
        [ActionName("Create")]
        public async Task<ActionResult> CreateAsync()
        {
            return View();
        }
   
    Şimdi bu denetleyicide **Oluştur** görünümünden gönderim kabul edecek biraz daha koda ihtiyacımız var.
3. Bu denetleyici için bir POST formuyla ASP.NET MVC'ye ne yapacağını söyleyen ItemController.cs sınıfına bir sonraki kod blokunu ekleyin.
   
        [HttpPost]
        [ActionName("Create")]
        [ValidateAntiForgeryToken]
        public async Task<ActionResult> CreateAsync([Bind(Include = "Id,Name,Description,Completed")] Item item)
        {
            if (ModelState.IsValid)
            {
                await DocumentDBRepository<Item>.CreateItemAsync(item);
                return RedirectToAction("Index");
            }
   
            return View(item);
        }
   
    Bu kod DocumentDBRepository'ye çağrı yapar ve yeni todo öğesini veritabanında kalıcı hale getirmek için CreateItemAsync yöntemini kullanır. 
   
    **Güvenlik Notu**: **ValidateAntiForgeryToken** özniteliği burada bu uygulamayı siteler arası istek sahteciliği saldırılarına karşı korumaya yardımcı olmak için kullanılır. Bu özniteliği eklemek tek başına yeterli değildir, görünümlerinizin de bu sahteciliği karşı önleme belirteci ile çalışması gerekir. Bu konu hakkında daha fazla bilgi ve bunu doğru uygulamaya yönelik örnekler için lütfen bkz. [Siteler Arası İstek Sahteciliğini Önleme][Preventing Cross-Site Request Forgery]. [GitHub][GitHub]'da sağlanan kaynak kodu tam uygulamayı içerir.
   
    **Güvenlik Notu**: Biz de **bağlama** korumaya gönderim saldırılarına karşı korunmaya yardımcı olmak için yöntem parametresinde özniteliği. Daha ayrıntılı bilgi için lütfen bkz. [ASP.NET MVC'de Temel CRUD İşlemleri][Basic CRUD Operations in ASP.NET MVC].

Veritabanımıza yeni Öğeler eklemek için gereken kod burada son bulur.

### <a name="_Toc395637772"></a>Öğeleri Düzenleme
Yapacağımız son bir şey kaldı, bu da veritabanında **Öğeler**'i düzenleme ve bunları tamamlanmış olarak işaretleme özelliğini eklemektir. Düzenleme görünümü projeye zaten eklenmişti, bu nedenle yalnızca denetleyicimize ve **DocumentDBRepository** sınıfına biraz kod eklememiz gereklidir.

1. **DocumentDBRepository** sınıfına aşağıdakileri ekleyin.
   
        public static async Task<Document> UpdateItemAsync(string id, T item)
        {
            return await client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(DatabaseId, CollectionId, id), item);
        }
   
        public static async Task<T> GetItemAsync(string id)
        {
            try
            {
                Document document = await client.ReadDocumentAsync(UriFactory.CreateDocumentUri(DatabaseId, CollectionId, id));
                return (T)(dynamic)document;
            }
            catch (DocumentClientException e)
            {
                if (e.StatusCode == HttpStatusCode.NotFound)
                {
                    return null;
                }
                else
                {
                    throw;
                }
            }
        }
   
    Bu yöntemlerin ilki olan **GetItem**, Azure Cosmos DB'den bir Öğe getirir ve bu öğe **ItemController**'a ve sonra **Düzenle** görünümüne geçirilir.
   
    Yeni eklediğimiz yöntemlerin ikincisi, Azure Cosmos DB'deki **Belge**'yi, **ItemController**'dan geçirilen **Belge**'nin sürümüyle değiştirir.
2. Aşağıdakileri **ItemController** sınıfına ekleyin.
   
        [HttpPost]
        [ActionName("Edit")]
        [ValidateAntiForgeryToken]
        public async Task<ActionResult> EditAsync([Bind(Include = "Id,Name,Description,Completed")] Item item)
        {
            if (ModelState.IsValid)
            {
                await DocumentDBRepository<Item>.UpdateItemAsync(item.Id, item);
                return RedirectToAction("Index");
            }
   
            return View(item);
        }
   
        [ActionName("Edit")]
        public async Task<ActionResult> EditAsync(string id)
        {
            if (id == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
   
            Item item = await DocumentDBRepository<Item>.GetItemAsync(id);
            if (item == null)
            {
                return HttpNotFound();
            }
   
            return View(item);
        }
   
    İlk yöntem, kullanıcı **Dizin** görünümünden **Düzenle** bağlantısına tıkladığında meydana gelen Http GET'ini işler. Bu yöntem Azure Cosmos DB'den bir [**Belge**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.document.aspx) getirir ve bunu **Düzenle** görünümüne geçirir.
   
    Ardından, **Düzenle** görünümü **IndexController**'a bir Http POST yapar. 
   
    Eklediğimiz ikinci yöntem, güncelleştirilmiş nesneyi veritabanında kalıcı hale getirmek üzere Azure Cosmos DB'ye geçirir.

Hepsi bu; uygulamamızı çalıştırmak, tamamlanmamış **Öğeleri** listelemek, yeni **Öğeler** eklemek ve **Öğeleri** düzenlemek için ihtiyacımız olan her şey bu kadar.

## <a name="_Toc395637773"></a>6. adım: Uygulamayı yerel olarak çalıştırma
Yerel makinenizde uygulamayı test etmek için aşağıdakileri yapın:

1. Uygulamayı hata ayıklama modunda oluşturmak için Visual Studio'da F5'e basın. Bu işlemin uygulamayı oluşturması ve bir tarayıcıyı daha önce gördüğümüz boş kılavuz sayfasıyla başlatması gerekir:
   
    ![Bu veritabanı Öğreticisi tarafından oluşturulan Yapılacaklar listesi web uygulamasının ekran görüntüsü](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-create-an-item-a.png)
   
     
2. **Yeni Oluştur** bağlantısına tıklayın ve **Ad** ve **Açıklama** alanlarına değerler ekleyin. **Tamamlandı** onay kutusunu seçmeden bırakın, aksi halde yeni **Öğe** tamamlanmış durumda eklenir ve ilk listede görünmez.
   
    ![Oluştur görünümünün ekran görüntüsü](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-create-new-item.png)
3. **Oluştur**'a tıklayın, böylece **Dizin** görünümüne geri yönlendirilirsiniz ve **Öğeniz** listede görünür.
   
    ![Dizin görünümünün ekran görüntüsü](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-create-an-item.png)
   
    Yapılacaklar listenize birkaç **Öğe** daha ekleyebilirsiniz.
    
4. Listedeki bir **Öğe**'nin yanındaki **Düzenle**'ye tıklayın, böylece **Tamamlandı** bayrağı dahil olmak üzere nesnenizin herhangi bir özelliğini güncelleştirebileceğiniz **Düzenle** görünümüne gidersiniz. **Tamamlandı** bayrağını işaretler ve **Kaydet**'e tıklarsanız **Öğe** tamamlanmamış görevler listesinden kaldırılır.
   
    ![Tamamlandı kutusu işaretlenmiş şekilde dizin görünümünün ekran görüntüsü](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-completed-item.png)
5. Uygulamayı test ettikten sonra, uygulamanın hata ayıklamasını durdurmak için Ctrl+F5'e basın. Dağıtıma hazırsınız!

## <a name="_Toc395637774"></a>7. adım: Uygulamanızı Azure App Service'e dağıtma 
Artık uygulamanın tamamı Azure Cosmos DB ile doğru şekilde çalıştığına göre, bu web uygulamasını Azure App Service’e dağıtacağız.  

1. Bu uygulamayı yayımlamak için yapmanız gereken tek şey, **Çözüm Gezgini**'nde projeye sağ tıklamak ve **Yayımla**'ya tıklamaktır.
   
    ![Çözüm Gezgini'nde Yayımla seçeneğinin ekran görüntüsü](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-publish.png)

2. **Yayımla** iletişim kutusunda **Microsoft Azure App Service**’e tıklayın, ardından **Yeni Oluştur**’u seçerek bir App Service profili oluşturun veya **Mevcut Olanı Seç**’e tıklayarak mevcut bir profili kullanın.

    ![Visual Studio’da Yayımla iletişim kutusu](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-publish-to-existing.png)

3. Mevcut bir Azure App Service profiliniz varsa, abonelik adınızı girin. **Görünüm** filtresini kullanarak kaynak grubuna veya kaynak türüne göre sıralayın ve sonra Azure App Service’inizi seçin. 
   
    ![Visual Studio’da App Service iletişim kutusu](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-app-service.png)

4. Yeni bir Azure App Service profili oluşturmak için, **Yayımla** iletişim kutusunda **Yeni Oluştur**’a tıklayın. **Uygulama Hizmetini Oluştur** iletişim kutusunda, Web uygulamanızın adını ve uygun aboneliği, kaynak grubunu ve App Service planını girip **Oluştur**’a tıklayın.

    ![Visual Studio’da App Service’i Oluştur iletişim kutusu](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-create-app-service.png)

Visual Studio birkaç saniye içinde web uygulamanızı yayımlamayı bitirecek ve eserinizi Azure’da çalışırken görebileceğiniz bir tarayıcı başlatacak!



## <a name="_Toc395637775"></a>Sonraki adımlar
Tebrikler! Azure Cosmos DB kullanarak ilk ASP.NET MVC web uygulamanızı oluşturdunuz ve bunu Azure’a yayımladınız. Bu öğreticide bulunmayan ayrıntı ve silme işlevleri dahil olmak üzere, tüm uygulamanın kaynak kodu [GitHub][GitHub]'dan indirilebilir veya kopyalanabilir. Uygulamanıza bunları eklemek isterseniz kodu alın ve bu uygulamaya ekleyin.

Uygulamanıza ilave işlevler eklemek için [Azure Cosmos DB .NET Kitaplığı](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet)’ndaki mevcut API’leri gözden geçirin ve [GitHub][GitHub]’daki Azure Cosmos DB .NET Kitaplığı’na katkıda bulunmaktan çekinmeyin. 

[Visual Studio Express]: https://www.visualstudio.com/products/visual-studio-express-vs.aspx
[Microsoft Web Platform Installer]: https://www.microsoft.com/web/downloads/platform.aspx
[Preventing Cross-Site Request Forgery]: https://go.microsoft.com/fwlink/?LinkID=517254
[Basic CRUD Operations in ASP.NET MVC]: https://go.microsoft.com/fwlink/?LinkId=317598
[GitHub]: https://github.com/Azure-Samples/documentdb-net-todo-app
