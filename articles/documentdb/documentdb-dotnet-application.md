---
title: "DocumentDB için ASP.NET MVC öğreticisi: Web Uygulaması Geliştirme | Microsoft Belgeleri"
description: "DocumentDB&quot;yi kullanarak bir MVC web uygulaması oluşturmak için hazırlanan ASP.NET MVC öğreticisi. Azure Web Siteleri&quot;nde barındırılan bir yapılacaklar uygulamasında JSON ve erişim verilerini depolayacaksınız - adım adım ASP.NET MVC öğreticisi."
keywords: "asp.net mvc öğreticisi, web uygulaması dağıtımı, mvc web uygulaması, asp net mvc adım adım öğreticisi"
services: documentdb
documentationcenter: .net
author: syamkmsft
manager: jhubbard
editor: cgronlun
ms.assetid: 52532d89-a40e-4fdf-9b38-aadb3a4cccbc
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 12/25/2016
ms.author: syamk
translationtype: Human Translation
ms.sourcegitcommit: 16bff1b5708652a75ea603f596c864901b12a88d
ms.openlocfilehash: 9b24fe8139d50b7c37a380fcc52b7ac302f5ee5d


---
# <a name="a-nametoc395809351aaspnet-mvc-tutorial-web-application-development-with-documentdb"></a><a name="_Toc395809351"></a>ASP.NET MVC Öğreticisi: DocumentDB ile web uygulaması geliştirme
> [!div class="op_single_selector"]
> * [.NET](documentdb-get-started.md)
> * [.NET Core](documentdb-dotnetcore-get-started.md)
> * [Java](documentdb-java-get-started.md)
> * [Node.js](documentdb-nodejs-get-started.md)
> * [C++](documentdb-cpp-get-started.md)
>  
> 

Bu makale, JSON belgelerini depolama ve sorgulama amacıyla Azure DocumentDB'yi nasıl verimli bir şekilde kullanabileceğinizi vurgulamak için, Azure DocumentDB kullanarak bir yapılacaklar uygulamasının nasıl oluşturulacağını gösteren uçtan uca bir kılavuz sağlar. Görevler, JSON belgeleri olarak Azure DocumentDB'de depolanır.

![Bu öğreticiyle oluşturulan yapılacaklar listesi MVC web uygulamasının ekran görüntüsü - adım adım ASP.NET MVC öğreticisi](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-image1.png)

Bu adım adım kılavuz, Azure tarafından sağlanan DocumentDB hizmetinin, Azure üzerinde barındırılan bir ASP.NET MVC web uygulamasında verileri depolamak ve bunlara erişmek için nasıl kullanılacağını gösterir. ASP.NET MVC bileşenleri yerine yalnızca DocumentDB'ye odaklanan bir öğretici arıyorsanız bkz. [DocumentDB C# konsol uygulaması oluşturma](documentdb-get-started.md).

> [!TIP]
> Bu öğretici, ASP.NET MVC ve Azure Web Siteleri'ni kullanma konusunda deneyim sahibi olduğunuzu varsayar. ASP.NET veya [önkoşul araçlarında](#_Toc395637760) yeniyseniz [GitHub][GitHub] konumundan örnek projenin tamamını indirmenizi ve bu örnekteki yönergeleri uygulamanızı öneririz. Oluşturduktan sonra, proje bağlamında kodu daha iyi kavramak için bu makaleyi inceleyebilirsiniz.
> 
> 

## <a name="a-nametoc395637760aprerequisites-for-this-database-tutorial"></a><a name="_Toc395637760"></a>Bu veritabanı öğreticisi için önkoşullar
Bu makaledeki yönergeleri uygulamadan önce aşağıdakilere sahip olduğunuzdan emin olmanız gerekir:

* Etkin bir Azure hesabı. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılar için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/) 

    OR

    Yerel bir [Azure DocumentDB Öykünücüsü](documentdb-nosql-local-emulator.md) yüklemesi.
* [Visual Studio 2015](http://www.visualstudio.com/) veya Visual Studio 2013 Güncelleştirme 4 ya da üzeri. Visual Studio 2013 kullanıyorsanız C# 6.0 desteği eklemek için [Microsoft.Net.Compilers nuget paketi](https://www.nuget.org/packages/Microsoft.Net.Compilers/) yüklemeniz gerekir. 
* [Microsoft Web Platformu Yükleyicisi][Microsoft Web Platform Installer] aracılığıyla kullanılabilen .NET için Azure SDK'sı 2.5.1 veya sonraki bir sürümü.

Bu makaledeki tüm ekran görüntüleri, Güncelleştirme 4 uygulanmış Visual Studio 2013 ve .NET için Azure SDK'sı 2.5.1 sürümü kullanılarak alınmıştır. Sisteminiz farklı sürümlerle yapılandırılmışsa ekranlarınızın ve seçeneklerinizin tamamen eşleşmeme olasılığı bulunur ancak yukarıdaki önkoşulları karşılarsanız bu çözümün çalışması gerekir.

## <a name="a-nametoc395637761astep-1-create-a-documentdb-database-account"></a><a name="_Toc395637761"></a>1. Adım: DocumentDB veritabanı hesabı oluşturma
Bir DocumentDB hesabı oluşturarak başlayalım. Zaten bir hesabınız varsa veya bu öğretici için DocumentDB Öykünücüsü’nü kullanıyorsanız [Yeni bir ASP.NET MVC uygulaması oluşturma](#_Toc395637762) adımına atlayabilirsiniz.

[!INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

[!INCLUDE [documentdb-keys](../../includes/documentdb-keys.md)]

<br/>
Şimdi yeni bir ASP.NET MVC uygulamasının nasıl oluşturulacağını en başından başlayarak inceleyeceğiz. 

## <a name="a-nametoc395637762astep-2-create-a-new-aspnet-mvc-application"></a><a name="_Toc395637762"></a>2. Adım: Yeni bir ASP.NET MVC uygulaması oluşturma
Artık bir hesabınız olduğuna göre yeni ASP.NET projemizi oluşturalım.

1. Visual Studio'da, **Dosya** menüsündeki **Yeni** seçeneğine gidin ve ardından **Proje**'ye tıklayın.
   
       The **New Project** dialog box appears.
2. **Proje türleri** bölmesinde **Şablonlar**, **Visual C#**, **Web**'i genişletin ve ardından **ASP.NET Web Uygulaması**'nı seçin.
   
      ![ASP.NET Web Uygulaması proje türü vurgulanmış şekilde Yeni Proje iletişim kutusunun ekran görüntüsü](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-image10.png)
3. **Ad** kutusunda projenin adını yazın. Bu öğretici "todo" adını kullanır. Bunun dışında bir şey kullanmayı seçerseniz bu öğreticinin todo ad alanından söz ettiği her yerde sağlanan kod örneklerini uygulamanıza verdiğiniz ada göre ayarlamanız gerekir. 
4. Projeyi oluşturmak istediğiniz klasöre gitmek için **Gözat**'a tıklayın ve ardından **Tamam**'a tıklayın.
   
      **Yeni ASP.NET Projesi** iletişim kutusu görünür.
   
      ![MVC uygulama şablonu vurgulanmış ve Bulutta barındır kutusu işaretli şekilde Yeni ASP.NET Projesi iletişim kutusunun ekran görüntüsü](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-image11.png)
5. Şablonlar bölmesinde **MVC**'yi seçin.
6. Uygulamanızı Azure'da barındırmayı düşünüyorsanız Azure'ın uygulamayı barındırmasını sağlamak için sağ alt kısımdaki **Bulutta barındır**'ı seçin. Bulutta barındırmayı ve bir Azure Web Sitesi'nde barındırılan uygulamayı çalıştırmayı seçtik. Bu seçeneğin belirlenmesi, bir Azure Web Sitesi'ni sizin için önceden sağlar ve çalışan uygulamanın son halini dağıtma zamanı geldiğinde işleri çok daha kolaylaştırır. Bunu başka bir yerde barındırmak istiyorsanız veya Azure'ı önceden yapılandırmak istemiyorsanız **Bulutta barındır**'ı temizlemeniz yeterlidir.
7. **Tamam**'a tıklayarak Visual Studio'nun boş ASP.NET MVC şablonu çevresinde iskele oluşturmasını sağlayın. 

    "İsteğiniz işlenirken bir hata oluştu" hatasını alıyorsanız [Sorun giderme](#troubleshooting) bölümüne bakın.

8. Bunu bulutta barındırmayı seçerseniz Azure hesabınızda oturum açmanızı ve yeni web siteniz için bazı değerler sağlamanızı isteyen en az bir ek ekran görürsünüz. Tüm ek değerleri sağlayın ve devam edin. 
   
      Burada bir Azure SQL Database Sunucusu kullanmadığımız için "Veritabanı sunucusu" seçeneğini belirlemedim, Azure Portal'da daha sonra yeni bir Azure DocumentDB hesabını oluşturacağız.
   
    Bir **App Service planı** ve **Kaynak grubu** seçme hakkında daha fazla bilgi için bkz. [Azure App Service planlarına ayrıntılı genel bakış](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
   
      ![Microsoft Azure Web Sitesi Yapılandırma iletişim kutusunun ekran görüntüsü](./media/documentdb-dotnet-application/image11_1.png)
9. Visual Studio demirbaş MVC uygulamasını oluşturmayı tamamlandıktan sonra, yerel olarak çalıştırabileceğiniz boş bir ASP.NET uygulamasına sahip olursunuz.
   
    Hepimizin ASP.NET "Hello World" uygulamasını gördüğünden emin olduğum için, projeyi yerel olarak çalıştırmayı atlayacağız. Şimdi doğrudan bu projeye DocumentDB eklemeye ve uygulamamızı oluşturmaya geçelim.

## <a name="a-nametoc395637767astep-3-add-documentdb-to-your-mvc-web-application-project"></a><a name="_Toc395637767"></a>3. Adım: MVC web uygulaması projenize DocumentDB ekleme
Bu çözüm için gereken ASP.NET MVC altyapısının çoğunu elde ettiğimize göre, bu öğreticinin asıl amacı olan MVC web uygulamamıza Azure DocumentDB'yi eklemeye geçelim.

1. DocumentDB .NET SDK'sı bir NuGet paketi olarak paketlenir ve dağıtılır. NuGet paketini Visual Studio'da almak için, **Çözüm Gezgini**'nde projeye sağ tıklayarak ve ardından **NuGet Paketlerini Yönet**'e tıklayarak Visual Studio'daki NuGet paket yöneticisini kullanın.
   
      ![NuGet Paketlerini Yönet vurgulanmış şekilde, Çözüm Gezgini'nde web uygulaması projesi için sağ tıklama seçeneklerinin ekran görüntüsü.](./media/documentdb-dotnet-application/image21.png)
   
    **NuGet Paketlerini Yönet** iletişim kutusu görünür.
2. NuGet **Gözat** kutusunda ***Azure DocumentDB*** yazın.
   
    Sonuçlardan **Microsoft Azure DocumentDB İstemci Kitaplığı** paketini yükleyin. Bu işlem DocumentDB paketini ve aynı zamanda Newtonsoft.Json gibi tüm bağımlılıkları indirir ve yükler. **Önizleme** penceresinde **Tamam**'a tıklayıp **Lisans Kabulü** penceresinde **Kabul Ediyorum**'a tıklayarak yüklemeyi tamamlayın.
   
      ![Microsoft Azure DocumentDB İstemci Kitaplığı vurgulanmış şekilde NuGet Paketlerini Yönet penceresinin ekran görüntüsü](./media/documentdb-dotnet-application/nuget.png)
   
      Alternatif olarak, paketi yüklemek için Paket Yöneticisi Konsolu'nu kullanabilirsiniz. Bunu yapmak için, **Araçlar** menüsünde **NuGet Paket Yöneticisi**'ne tıklayın ve ardından **Paket Yöneticisi Konsolu**'na tıklayın. İstendiğinde aşağıdakileri yazın.
   
        Install-Package Microsoft.Azure.DocumentDB
3. Paket yüklendikten sonra, Visual Studio çözümünüz Microsoft.Azure.Documents.Client ve Newtonsoft.Json olmak üzere iki yeni başvuru eklenmiş şekilde aşağıdakine benzemelidir.
   
      ![Çözüm Gezgini'nde JSON veri projesine eklenen iki başvurunun ekran görüntüsü](./media/documentdb-dotnet-application/image22.png)

## <a name="a-nametoc395637763astep-4-set-up-the-aspnet-mvc-application"></a><a name="_Toc395637763"></a>4. Adım: ASP.NET MVC uygulamasını ayarlama
Şimdi bu MVC uygulamasına modeller, görünümler ve denetleyiciler ekleyelim:

* [Model ekleme](#_Toc395637764).
* [Denetleyici ekleme](#_Toc395637765).
* [Görünümler ekleme](#_Toc395637766).

### <a name="a-nametoc395637764aadd-a-json-data-model"></a><a name="_Toc395637764"></a>JSON veri modeli ekleme
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
   
    DocumentDB'deki tüm veriler kablo üzerinden geçer ve JSON olarak depolanır. JSON.NET tarafından nesnelerinizin seri hale getirilme/seri durumundan çıkarılma yolunu denetlemek için, yeni oluşturduğumuz **Item** sınıfında gösterildiği şekilde **JsonProperty** özniteliğini kullanabilirsiniz. Bunu yapmak **zorunda** değilsiniz ancak özelliklerimin JSON camelCase adlandırma kurallarına uyduğundan emin olmak istiyorum. 
   
    Özellik adının biçimini JSON'a gittiği zaman denetlemenin yanı sıra, **Açıklama** özelliğinde yaptığım gibi .NET özelliklerinizi tamamen yeniden adlandırabilirsiniz. 

### <a name="a-nametoc395637765aadd-a-controller"></a><a name="_Toc395637765"></a>Denetleyici ekleme
**M** ile işimiz bitti; şimdi de MVC'deki **C**'yi, yani denetleyici sınıfını oluşturalım.

1. **Çözüm Gezgini**'nde **Denetleyiciler** klasörüne sağ tıklayın, **Ekle**'ye tıklayın ve ardından **Denetleyici**'ye tıklayın.
   
    **İskele Ekle** iletişim kutusu görünür.
2. **MVC 5 Denetleyici - Boş**'u seçin ve ardından **Ekle**'ye tıklayın.
   
    ![MVC 5 Denetleyici - Boş seçeneği vurgulanmış şekilde İskele Ekle iletişim kutusunun ekran görüntüsü](./media/documentdb-dotnet-application/image14.png)
3. Yeni Denetleyicinize **ItemController** adını verin.
   
    ![Denetleyici Ekle iletişim kutusunun ekran görüntüsü](./media/documentdb-dotnet-application/image15.png)
   
    Dosya oluşturulduktan sonra, Visual Studio çözümünüz **Çözüm Gezgini**'nde yeni ItemController.cs dosyasıyla aşağıdakine benzemelidir. Daha önce oluşturduğunuz yeni Item.cs dosyası da gösterilmektedir.
   
    ![Yeni ItemController.cs dosyası ve Item.cs dosyası vurgulanmış şekilde Visual Studio çözümü - Çözüm Gezgini'nin ekran görüntüsü](./media/documentdb-dotnet-application/image16.png)
   
    ItemController.cs'yi kapatabilirsiniz, buna daha sonra geri döneceğiz. 

### <a name="a-nametoc395637766aadd-views"></a><a name="_Toc395637766"></a>Görünümler ekleme
Şimdi MVC'deki **V**'yi, yani görünümleri oluşturalım:

* [Öğe Dizini görünümü ekleme](#AddItemIndexView).
* [Yeni Öğe görünümü ekleme](#AddNewIndexView).
* [Düzenleme Öğesi görünümü ekleme](#_Toc395888515).

#### <a name="a-nameadditemindexviewaadd-an-item-index-view"></a><a name="AddItemIndexView"></a>Öğe Dizini görünümü ekleme
1. **Çözüm Gezgini**'nde **Görünümler** klasörünü genişletin, daha önce **ItemController**'ı eklediğinizde Visual Studio'nun sizin için oluşturduğu boş **Öğe** klasörüne sağ tıklayın, **Ekle**'ye tıklayın ve ardından **Görünüm**'e tıklayın.
   
    ![Görünüm Ekle komutları vurgulanmış şekilde Visual Studio'nun oluşturduğu Öğe klasörünü gösteren Çözüm Gezgini'nin ekran görüntüsü](./media/documentdb-dotnet-application/image17.png)
2. **Görünüm Ekle** iletişim kutusunda aşağıdakileri yapın:
   
   * **Görünüm adı** kutusunda ***Index*** yazın.
   * **Şablon** kutusunda ***Liste***'yi seçin.
   * **Model sınıfı** kutusunda ***Öğe (todo.Models)*** seçeneğini belirleyin.
   * **Veri bağlamı sınıfı** kutusunu boş bırakın. 
   * Düzen sayfası kutusunda ***~/Views/Shared/_Layout.cshtml*** yazın.
     
     ![Görünüm Ekle iletişim kutusunu gösteren ekran görüntüsü](./media/documentdb-dotnet-application/image18.png)
3. Bu değerlerin tümü ayarlandıktan sonra, **Ekle**'ye tıklayın ve Visual Studio'nun yeni bir şablon görünümü oluşturmasına izin verin. Tamamlandığında, oluşturulan cshtml dosyasını açar. Daha sonra geri döneceğimiz için Visual Studio'daki bu dosyayı kapatabiliriz.

#### <a name="a-nameaddnewindexviewaadd-a-new-item-view"></a><a name="AddNewIndexView"></a>Yeni Öğe görünümü ekleme
**Öğe Dizini** görünümü oluşturmamıza benzer şekilde, şimdi de yeni **Öğeler** oluşturmak için yeni bir görünüm oluşturacağız.

1. **Çözüm Gezgini**'nde **Öğe** klasörüne tekrar sağ tıklayın, **Ekle**'ye tıklayın ve ardından **Görünüm**'e tıklayın.
2. **Görünüm Ekle** iletişim kutusunda aşağıdakileri yapın:
   
   * **Görünüm adı** kutusunda ***Create*** yazın.
   * **Şablon** kutusunda ***Oluştur***'u seçin.
   * **Model sınıfı** kutusunda ***Öğe (todo.Models)*** seçeneğini belirleyin.
   * **Veri bağlamı sınıfı** kutusunu boş bırakın.
   * Düzen sayfası kutusunda ***~/Views/Shared/_Layout.cshtml*** yazın.
   * **Ekle**'ye tıklayın.

#### <a name="a-nametoc395888515aadd-an-edit-item-view"></a><a name="_Toc395888515"></a>Düzenleme Öğesi görünümü ekleme
Son olarak, bir **Öğe**'yi düzenlemek için daha önce kullandığımız yolu tekrarlayarak son bir görünüm ekleyin.

1. **Çözüm Gezgini**'nde **Öğe** klasörüne tekrar sağ tıklayın, **Ekle**'ye tıklayın ve ardından **Görünüm**'e tıklayın.
2. **Görünüm Ekle** iletişim kutusunda aşağıdakileri yapın:
   
   * **Görünüm adı** kutusunda ***Edit*** yazın.
   * **Şablon** kutusunda ***Düzenle***'yi seçin.
   * **Model sınıfı** kutusunda ***Öğe (todo.Models)*** seçeneğini belirleyin.
   * **Veri bağlamı sınıfı** kutusunu boş bırakın. 
   * Düzen sayfası kutusunda ***~/Views/Shared/_Layout.cshtml*** yazın.
   * **Ekle**'ye tıklayın.

Bunu yaptıktan sonra, bu görünümlere daha sonra geri döneceğimiz için Visual Studio'daki tüm cshtml belgelerini kapatın.

## <a name="a-nametoc395637769astep-5-wiring-up-documentdb"></a><a name="_Toc395637769"></a>5. Adım: DocumentDB'yi bağlama
Standart MVC işleri hallolduğuna göre, DocumentDB için kod eklemeye dönelim. 

Bu bölümde, aşağıdakileri işlemek için kod ekleyeceğiz:

* [Tamamlanmamış Öğeleri listeleme](#_Toc395637770).
* [Öğeler ekleme](#_Toc395637771).
* [Öğeleri düzenleme](#_Toc395637772).

### <a name="a-nametoc395637770alisting-incomplete-items-in-your-mvc-web-application"></a><a name="_Toc395637770"></a>MVC web uygulamanızda tamamlanmamış Öğeleri listeleme
Burada yapılacak ilk şey, DocumentDB'ye bağlanmayı ve kullanmayı sağlayan tüm mantığı içeren bir sınıf eklemektir. Bu öğretici için tüm bu mantığı DocumentDBRepository adlı bir depo sınıfına kapsülleyeceğiz. 

1. **Çözüm Gezgini**'nde projeye sağ tıklayın, **Ekle**'ye tıklayın ve ardından **Sınıf**'a tıklayın. Yeni sınıfa **DocumentDBRepository** adını verin ve **Ekle**'ye tıklayın.
2. Yeni oluşturulan **DocumentDBRepository** sınıfında *ad alanı* bildiriminin üstüne aşağıdaki *using deyimlerini* ekleyin
   
        using Microsoft.Azure.Documents; 
        using Microsoft.Azure.Documents.Client; 
        using Microsoft.Azure.Documents.Linq; 
        using System.Configuration;
        using System.Linq.Expressions;
        using System.Threading.Tasks;
   
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
   
   > [!TIP]
   > Yeni bir DocumentCollection oluştururken, yeni koleksiyonun performans düzeyini belirlemenize izin veren OfferType'ın isteğe bağlı bir RequestOptions parametresini sağlayabilirsiniz. Bu parametre geçirilmezse varsayılan teklif türü kullanılır. DocumentDB teklif türleri hakkında daha fazla bilgi için lütfen [DocumentDB Performans Düzeyleri](documentdb-performance-levels.md)'ne başvurun.
   > 
   > 
3. Bazı değerleri yapılandırmadan okuyoruz, bu nedenle uygulamanızın **Web.config** dosyasını açın ve `<AppSettings>` bölümünün altına aşağıdaki satırları ekleyin.
   
        <add key="endpoint" value="enter the URI from the Keys blade of the Azure Portal"/>
        <add key="authKey" value="enter the PRIMARY KEY, or the SECONDARY KEY, from the Keys blade of the Azure  Portal"/>
        <add key="database" value="ToDoList"/>
        <add key="collection" value="Items"/>
4. Şimdi de Azure Portal'ın Anahtarlar dikey penceresini kullanarak *endpoint* ve *authKey* değerlerini güncelleştirin. Uç nokta ayarının değeri olarak Anahtarlar dikey penceresinden **URI**'yi kullanın ve authKey ayarının değeri olarak Anahtarlar dikey penceresinden **BİRİNCİL ANAHTAR** veya **İKİNCİL ANAHTAR**'ı kullanın.

    DocumentDB'yi bağlamayı tamamladık, şimdi de uygulama mantığımızı ekleyelim.

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

![Bu veritabanı öğreticisi tarafından oluşturulan yapılacaklar listesi web uygulamasının ekran görüntüsü](./media/documentdb-dotnet-application/image23.png)

### <a name="a-nametoc395637771aadding-items"></a><a name="_Toc395637771"></a>Öğeler ekleme
Boş bir kılavuzdan başka şeyler görmek için veritabanımıza biraz öğe ekleyelim.

DocumentDB'deki kaydı kalıcı hale getirmek için DocumentDBRepository ve ItemController'a biraz kod ekleyelim.

1. **DocumentDBRepository** sınıfınıza aşağıdaki yöntemi ekleyin.
   
       public static async Task<Document> CreateItemAsync(T item)
       {
           return await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(DatabaseId, CollectionId), item);
       }
   
   Bu yöntem kendisine geçirilen nesneyi alır ve bunu DocumentDB'de kalıcı hale getirir.
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
   
    **Güvenlik Notu**: **ValidateAntiForgeryToken** özniteliği burada bu uygulamayı siteler arası istek sahteciliği saldırılarına karşı korunmaya yardımcı olmak için kullanılır. Bu özniteliği eklemek tek başına yeterli değildir, görünümlerinizin de bu sahteciliği karşı önleme belirteci ile çalışması gerekir. Bu konu hakkında daha fazla bilgi ve bunu doğru uygulamaya yönelik örnekler için lütfen bkz. [Siteler Arası İstek Sahteciliğini Önleme][Preventing Cross-Site Request Forgery]. [GitHub][GitHub]'da sağlanan kaynak kodu tam uygulamayı içerir.
   
    **Güvenlik Notu**: Aşırı gönderim saldırılarına karşı korunmaya yardımcı olmak için yöntem parametresinde **Bind** özniteliğini de kullanırız. Daha ayrıntılı bilgi için lütfen bkz. [ASP.NET MVC'de Temel CRUD İşlemleri][Basic CRUD Operations in ASP.NET MVC].

Veritabanımıza yeni Öğeler eklemek için gereken kod burada son bulur.

### <a name="a-nametoc395637772aediting-items"></a><a name="_Toc395637772"></a>Öğeleri Düzenleme
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
   
    Bu yöntemlerin ilki olan **GetItem**, DocumentDB'den bir Öğe getirir ve bu öğe **ItemController**'a ve sonra **Düzenle** görünümüne geçirilir.
   
    Yeni eklediğimiz yöntemlerin ikincisi, DocumentDB'deki **Belge**'yi, **ItemController**'dan geçirilen **Belge**'nin sürümüyle değiştirir.
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
   
    İlk yöntem, kullanıcı **Dizin** görünümünden **Düzenle** bağlantısına tıkladığında meydana gelen Http GET'ini işler. Bu yöntem DocumentDB'den bir [**Belge**](http://msdn.microsoft.com/library/azure/microsoft.azure.documents.document.aspx) getirir ve bunu **Düzenle** görünümüne geçirir.
   
    Ardından, **Düzenle** görünümü **IndexController**'a bir Http POST yapar. 
   
    Eklediğimiz ikinci yöntem, güncelleştirilmiş nesneyi veritabanında kalıcı hale getirmek üzere DocumentDB'ye geçirir.

Hepsi bu; uygulamamızı çalıştırmak, tamamlanmamış **Öğeleri** listelemek, yeni **Öğeler** eklemek ve **Öğeleri** düzenlemek için ihtiyacımız olan her şey bu kadar.

## <a name="a-nametoc395637773astep-6-run-the-application-locally"></a><a name="_Toc395637773"></a>6. Adım: Uygulamayı yerel olarak çalıştırma
Yerel makinenizde uygulamayı test etmek için aşağıdakileri yapın:

1. Uygulamayı hata ayıklama modunda oluşturmak için Visual Studio'da F5'e basın. Bu işlemin uygulamayı oluşturması ve bir tarayıcıyı daha önce gördüğümüz boş kılavuz sayfasıyla başlatması gerekir:
   
    ![Bu veritabanı öğreticisi tarafından oluşturulan yapılacaklar listesi web uygulamasının ekran görüntüsü](./media/documentdb-dotnet-application/image24.png)
   
    Visual Studio 2013 kullanıyorsanız ve "Catch yan tümcesinin gövdesinde bekleyemiyor" hatasını alıyorsanız [Microsoft.Net.Compilers nuget paketi](https://www.nuget.org/packages/Microsoft.Net.Compilers/) yüklemeniz gerekir. Ayrıca kodunuzu [GitHub][GitHub] üzerindeki kodunuzla karşılaştırabilirsiniz. 
2. **Yeni Oluştur** bağlantısına tıklayın ve **Ad** ve **Açıklama** alanlarına değerler ekleyin. **Tamamlandı** onay kutusunu seçmeden bırakın, aksi halde yeni **Öğe** tamamlanmış durumda eklenir ve ilk listede görünmez.
   
    ![Oluştur görünümünün ekran görüntüsü](./media/documentdb-dotnet-application/image25.png)
3. **Oluştur**'a tıklayın, böylece **Dizin** görünümüne geri yönlendirilirsiniz ve **Öğeniz** listede görünür.
   
    ![Dizin görünümünün ekran görüntüsü](./media/documentdb-dotnet-application/image26.png)
   
    Yapılacaklar listenize birkaç **Öğe** daha ekleyebilirsiniz.
4. Listedeki bir **Öğe**'nin yanındaki **Düzenle**'ye tıklayın, böylece **Tamamlandı** bayrağı dahil olmak üzere nesnenizin herhangi bir özelliğini güncelleştirebileceğiniz **Düzenle** görünümüne gidersiniz. **Tamamlandı** bayrağını işaretler ve **Kaydet**'e tıklarsanız **Öğe** tamamlanmamış görevler listesinden kaldırılır.
   
    ![Tamamlandı kutusu işaretlenmiş şekilde Dizin görünümünün ekran görüntüsü](./media/documentdb-dotnet-application/image27.png)
5. Uygulamayı test ettikten sonra, uygulamanın hata ayıklamasını durdurmak için Ctrl+F5'e basın. Dağıtıma hazırsınız!

## <a name="a-nametoc395637774astep-7-deploy-the-application-to-azure-websites"></a><a name="_Toc395637774"></a>7. Adım: Uygulamayı Azure Web Siteleri'ne dağıtma
Artık uygulamanın tamamı DocumentDB ile doğru şekilde çalıştığına göre, bu web uygulamasını Azure Web Siteleri'ne dağıtacağız. Boş ASP.NET MVC projesini oluştururken **Bulutta barındır**'ı seçtiyseniz Visual Studio bu işlemi gerçekten kolaylaştırır ve işlerin çoğunu sizin için yapar. 

1. Bu uygulamayı yayımlamak için yapmanız gereken tek şey, **Çözüm Gezgini**'nde projeye sağ tıklamak ve **Yayımla**'ya tıklamaktır.
   
    ![Çözüm Gezgini'nde Yayımla seçeneğinin ekran görüntüsü](./media/documentdb-dotnet-application/image28.png)
2. Her şeyin kimlik bilgilerinize göre zaten yapılandırılmış olması gerekir, hatta Azure'daki **Hedef URL**'de web sitesi sizin için önceden oluşturulmuştur. Tek yapmanız gereken **Yayımla**'ya tıklamaktır.
   
    ![Visual Studio'da Web'i Yayımla iletişim kutusunun ekran görüntüsü - adım adım ASP.NET MVC öğreticisi](./media/documentdb-dotnet-application/image29.png)

Visual Studio birkaç saniye içinde web uygulamanızı yayımlamayı bitirecek ve eserinizi Azure'da çalışırken görebileceğiniz bir tarayıcıyı başlatacak!

## <a name="a-nametroubleshootingatroubleshooting"></a><a name="Troubleshooting"></a>Sorun giderme

Web uygulamasını dağıtmaya çalışırken "İsteğiniz işlenirken bir hata oluştu" hatasını alıyorsanız aşağıdaki adımları uygulayın: 

1. Hata iletisini iptal edin ve **Microsoft Azure Web Apps** öğesini tekrar seçin. 
2. Oturum açın ve ardından **Yeni**’yi seçerek yeni bir web uygulaması oluşturun. 
3. **Microsoft Azure’da Web Uygulaması Oluştur** ekranında aşağıdaki adımları uygulayın: 
    
    - Web Uygulaması adı: "todo-net-app"
    - App Service planı: "todo-net-app" adlı yeni bir plan oluşturun
    - Kaynak grubu: "todo-net-app" adlı yeni bir grup oluşturun
    - Bölge: Uygulamanızın kullanıcılarına en yakın bölgeyi seçin
    - Veritabanı sunucusu: Veritabanı yok’a ve ardından **Oluştur**’a tıklayın. 

4. "todo-net-app * ekranı"nda **Bağlantıyı Doğrula**’ya tıklayın. Bağlantı doğrulandıktan sonra **Yayımla*’ya tıklayın*. 
    
    Bu adımın ardından uygulama tarayıcınızda görüntülenir.


## <a name="a-nametoc395637775anext-steps"></a><a name="_Toc395637775"></a>Sonraki adımlar
Tebrikler! Azure DocumentDB kullanarak ilk ASP.NET MVC web uygulamanızı oluşturdunuz ve bunu Azure Web Siteleri'ne yayımladınız. Bu öğreticide bulunmayan ayrıntı ve silme işlevleri dahil olmak üzere, tüm uygulamanın kaynak kodu [GitHub][GitHub]'dan indirilebilir veya kopyalanabilir. Uygulamanıza bunları eklemek isterseniz kodu alın ve bu uygulamaya ekleyin.

Uygulamanıza işlev eklemek için [DocumentDB .NET Kitaplığı](https://msdn.microsoft.com/library/azure/dn948556.aspx)'ndaki mevcut API'lere başvurun. [GitHub][GitHub]'daki DocumentDB .NET Kitaplığı'na istediğiniz zaman katkıda bulunabilirsiniz. 

[\*]: https://microsoft.sharepoint.com/teams/DocDB/Shared%20Documents/Documentation/Docs.LatestVersions/PicExportError
[Visual Studio Express]: http://www.visualstudio.com/products/visual-studio-express-vs.aspx
[Microsoft Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[Preventing Cross-Site Request Forgery]: http://go.microsoft.com/fwlink/?LinkID=517254
[Basic CRUD Operations in ASP.NET MVC]: http://go.microsoft.com/fwlink/?LinkId=317598
[GitHub]: https://github.com/Azure-Samples/documentdb-net-todo-app



<!--HONumber=Feb17_HO3-->


