---
title: .NET kullanarak bir ASP.NET MVC web uygulamasını Azure Cosmos DB ile Önizleme SDK'sı geliştirme Öğreticisi.
description: Bu öğreticide, Azure Cosmos DB kullanarak bir ASP .NET MVC web uygulaması oluşturma işlemini açıklar. Depolayın ve Azure'da barındırılan bir Yapılacaklar uygulamasında JSON veri erişim.
author: deborahc
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 12/03/2018
ms.author: dech
ms.openlocfilehash: bf1da7e8a1041b15076ebda6eeac9b0a75c567c0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61439035"
---
# <a name="tutorial-develop-an-aspnet-mvc-web-application-with-azure-cosmos-db-by-using-net-preview-sdk"></a>Öğretici: .NET Önizleme SDK'sını kullanarak Azure Cosmos DB ile bir ASP.NET MVC web uygulaması geliştirme 

> [!div class="op_single_selector"]
> * [.NET](sql-api-dotnet-application.md)
> * [.NET Önizleme](sql-api-dotnet-application-preview.md)
> * [Java](sql-api-java-application.md)
> * [Node.js](sql-api-nodejs-application.md)
> * [Python](sql-api-python-application.md)
> * [Xamarin](mobile-apps-with-xamarin.md)


Bu öğreticide Azure üzerinde barındırılan bir ASP.NET MVC uygulaması erişim verileri ve depolamak için Azure Cosmos DB kullanmayı gösterir. Bu öğreticide, şu anda önizlemede olan .NET SDK'sı V3 kullanırsınız. Aşağıdaki resimde, bu makaledeki örnek kullanarak derler web sayfası gösterilmektedir:
 
![Yapılacaklar listesi MVC web uygulaması Bu öğretici - ASP NET MVC adım adım Öğreticisi tarafından oluşturulan ekran görüntüsü](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-image01.png)

Öğreticiyi tamamlayacak zamanınız yoksa konumundan örnek projenin tamamını indirebilirsiniz [GitHub][GitHub]. 

Bu öğreticinin içindekiler:

> [!div class="checklist"]
> * Bir Azure Cosmos hesabı oluşturma
> * ASP.NET MVC uygulaması oluşturma
> * Uygulamayı Azure Cosmos DB'ye bağlanma 
> * Veri çubuğunda CRUD işlemleri gerçekleştirme

> [!TIP]
> Bu öğretici, ASP.NET MVC ve Azure Web Siteleri'ni kullanma konusunda deneyim sahibi olduğunuzu varsayar. ASP.NET için yeni başladıysanız veya [önkoşul araçlarında](#prerequisites), konumundan örnek projenin tamamını indirmenizi öneririz [GitHub][GitHub], gerekli NuGet paketlerini ekleyin ve çalıştırın. Projeyi derledikten sonra proje bağlamında kodu daha iyi kavramak için bu makaleyi inceleyebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar 

Bu makaledeki yönergeleri izlemeden önce aşağıdaki kaynaklara sahip olduğunuzdan emin olun:

* **Etkin bir Azure hesabı:** Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [!INCLUDE [cosmos-db-emulator-vs](../../includes/cosmos-db-emulator-vs.md)]  

* Visual Studio 2017 için .NET için Microsoft Azure SDK’sına Visual Studio Yükleyicisi ile erişilebilir.

Bu makaledeki tüm ekran görüntüleri, Microsoft Visual Studio Community 2017 kullanılarak alınmıştır. Sisteminiz farklı bir sürümü ile yapılandırılmışsa ekranlarınızın ve seçeneklerinizin tamamen eşleşmiyor olabilir, ancak yukarıdaki önkoşulları karşılarsanız bu çözümün çalışması gerekir.

## <a name="create-an-azure-cosmos-account"></a>1. adım: Bir Azure Cosmos hesabı oluşturma

Bir Azure Cosmos hesabı oluşturarak başlayalım. Zaten bir Azure Cosmos DB SQL API hesabınızın olması veya Bu öğretici için Azure Cosmos DB öykünücüsü'nü kullanıyorsanız, adımına atlayabilirsiniz [yeni bir ASP.NET MVC uygulaması oluşturma](#create-a-new-mvc-application) bölümü.

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [keys](../../includes/cosmos-db-keys.md)]

Sonraki bölümde, yeni bir ASP.NET MVC uygulaması oluşturun. 

## <a name="create-a-new-mvc-application"></a>2. adım: Yeni bir ASP.NET MVC uygulaması oluşturma

1. Visual Studio'da gelen **dosya** menüsünde seçin **yeni**ve ardından **proje**. **Yeni Proje** iletişim kutusu görünür.

2. İçinde **yeni proje** penceresini genişletin **yüklü** şablonları **Visual C#** , **Web**ve ardından **ASP. NET Web uygulaması**. 

   ![Yeni ASP.NET web uygulaması projesi oluşturma](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-new-project-dialog.png)

3. **Ad** kutusunda projenin adını yazın. Bu öğretici "todo" adını kullanır. Bunun dışında bir şey kullanmayı seçerseniz bu öğreticinin todo ad alanı hakkında konuşuyor ve her yerde daha sonra uygulamanızı adlı kullanmak için sağlanan kod örneklerini ayarlayın. 

4. Seçin **Gözat** istediğiniz projeyi oluşturun ve ardından klasörüne gitmenizi **.NET framework 4.6.1** veya üzeri. **Tamam**’ı seçin. 

5. **Yeni ASP.NET Web Uygulaması** iletişim kutusu görüntülenir. Şablonlar bölmesinde **MVC**'yi seçin.

6. Seçin **Tamam** ve Visual Studio'nun boş ASP.NET MVC şablonu çevresinde iskele yapmak istiyorum. 

7. Visual Studio Demirbaş MVC uygulamasını oluşturmayı tamamlandıktan sonra yerel olarak çalıştırabileceğiniz boş bir ASP.NET uygulamasına sahip olursunuz.

## <a name="add-nuget-packages"></a>3. adım: Azure Cosmos DB NuGet paketini projeye ekleyin.

Bu çözüm için gereken ASP.NET MVC çerçevesi kodu çoğunu, Azure Cosmos DB'ye bağlanmak için gerekli NuGet paketlerini ekleyelim.

1. Azure Cosmos DB .NET SDK’sı, bir NuGet paketi olarak paketlenir ve dağıtılır. Visual Studio'da NuGet paketini almak için NuGet Paket Yöneticisi Visual Studio'da projeye sağ tıklayarak kullanmak **Çözüm Gezgini** seçip **NuGet paketlerini Yönet**.
   
   ![NuGet paketlerini Yönet vurgulanmış ile Çözüm Gezgini'nde web uygulaması projesi için sağ tıklama seçeneklerinin ekran görüntüsü.](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-manage-nuget.png)
   
2. **NuGet Paketlerini Yönet** iletişim kutusu görünür. Nuget **Gözat** kutusuna **Microsoft.Azure.Cosmos**. Sonuçlardan yükleme **Microsoft.Azure.Cosmos** 3.0.0.1-preview sürümü. Bu indirmeleri ve Azure Cosmos DB paketini ve aynı zamanda Newtonsoft.Json gibi bağımlılıkları yükler. Seçin **Tamam** içinde **Önizleme** penceresinde ve **kabul ediyorum** içinde **lisans kabulü** yüklemeyi tamamlamak için penceresi.
   
   Alternatif olarak, NuGet paketini yüklemek için Paket Yöneticisi konsolu kullanabilirsiniz. Bunu yapmak için **Araçları** menüsünde **NuGet Paket Yöneticisi**ve ardından **Paket Yöneticisi Konsolu**. İsteminde aşağıdaki komutu yazın:
   
   ```bash
   Install-Package Microsoft.Azure.Cosmos -Version 3.0.0.1-preview
   ```        

3. Paket yüklendikten sonra Visual Studio çözümünüze iki yeni kitaplığı başvurularını Microsoft.Azure.Cosmos.Client ve Newtonsoft.Json içermelidir.
  
## <a name="set-up-the-mvc-application"></a>4. adım: ASP.NET MVC uygulamasını ayarlama

Şimdi şimdi bu MVC uygulamasına modeller, görünümler ve denetleyiciler ekleyelim:

* [Model ekleme](#add-a-model).
* [Denetleyici ekleme](#add-a-controller).
* [Görünümler ekleme](#add-views).

### <a name="add-a-model"></a> Model ekleme

1. Gelen **Çözüm Gezgini**, sağ **modelleri** klasörüne **Ekle**ve ardından **sınıfı**. **Yeni Öğe Ekle** iletişim kutusu görünür.

1. Yeni sınıfınıza ad **TodoItem.cs** seçip **Ekle**. 

1. Sonraki "Todoıtem" sınıf kodu aşağıdaki kodla değiştirin:

   [!code-csharp[Main](~/samples-cosmosdb-dotnet-web-app/src/Models/TodoItem.cs)]
   
   Azure Cosmos DB'de depolanan veriler kablo üzerinden geçer ve JSON olarak depolanır. Nesnelerinizi JSON.NET tarafından seri hale getirilmiş/serisi biçimini denetlemek için kullanabileceğiniz **Item** özniteliği gösterildiği şekilde **Todoıtem** oluşturduğunuz sınıfı. İle yaptığınız gibi JSON geçer, ayrıca .NET özelliklerinizi yeniden adlandırabilirsiniz, özellik adının biçimi yalnızca denetleyebileceğiniz **açıklama** özelliği. 

### <a name="add-a-controller"></a>Denetleyici ekleme

1. Gelen **Çözüm Gezgini**, sağ **denetleyicileri** klasörüne **Ekle**ve ardından **denetleyicisi**. **İskele Ekle** iletişim kutusu görünür.

1. Seçin **MVC 5 denetleyici - boş** seçip **Ekle**.

   ![MVC 5 denetleyici - boş seçeneği vurgulanmış İskele Ekle iletişim kutusunun ekran görüntüsü](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-controller-add-scaffold.png)

1. Yeni denetleyicinize adı ** Itemcontroller ve o dosyasındaki kodu aşağıdaki kodla değiştirin:

   [!code-csharp[Main](~/samples-cosmosdb-dotnet-web-app/src/Controllers/ItemController.cs)]

   **ValidateAntiForgeryToken** özniteliği burada bu uygulamayı siteler arası istek sahteciliği saldırılarına karşı korumaya yardımcı olmak için kullanılır. Yalnızca bu özniteliği eklemek için daha fazla, kendi görünümlerinizi de bu sahteciliğe karşı koruma belirteci ile çalışması gerekir. Konu ve bunu doğru uygulamaya dair örnekler hakkında daha fazla bilgi için bkz. [siteler arası istek sahteciliğini][Preventing Cross-Site Request Forgery]. [GitHub][GitHub]'da sağlanan kaynak kodu tam uygulamayı içerir.
   
   Biz de **bağlama** korumaya gönderim saldırılarına karşı korunmaya yardımcı olmak için yöntem parametresinde özniteliği. Daha fazla ayrıntı için lütfen bkz [ASP.NET mvc'de temel CRUD işlemleri][Basic CRUD Operations in ASP.NET MVC].
    
### <a name="add-views"></a>Görünümler ekleme

Ardından, aşağıdaki üç görünümleri oluşturalım: 

* [Bir liste öğesi görünümü ekleme](#AddItemIndexView).
* [Yeni öğe görünümü ekleme](#AddNewIndexView).
* [Düzenleme öğesi görünümü ekleme](#AddEditIndexView).

#### <a name="AddItemIndexView"></a>Bir liste öğesi görünümü ekleme

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** klasör boş sağ **öğesi** eklediğiniz zaman Visual Studio için oluşturduğunuz klasöre  **Itemcontroller** daha önce tıklayın **Ekle**ve ardından **görünümü**.
   
   ![Çözüm Gezgini'nin ekran görüntüsü Görünüm Ekle komutları vurgulanmış Visual Studio'nun oluşturduğu öğe klasörünü gösteren](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-add-view.png)

2. İçinde **Görünüm Ekle** iletişim kutusunda, aşağıdaki değerleri güncelleştirin:
   
   * **Görünüm adı** kutusunda ***Index*** yazın.
   * **Şablon** kutusunda ***Liste***'yi seçin.
   * **Model sınıfı** kutusunda ***Öğe (todo.Models)*** seçeneğini belirleyin.
   * Düzen sayfası kutusunda ***~/Views/Shared/_Layout.cshtml*** yazın.
     
   ![Görünüm Ekle iletişim kutusunu gösteren ekran görüntüsü](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-add-view-dialog.png)

3. Bu değerleri ekledikten sonra seçin **Ekle** ve Visual Studio'nun yeni bir şablon görünümü oluşturmasına izin verin. Bunu yaptıktan sonra oluşturulan cshtml dosyasını açar. Sizin için daha sonra geri dönebilir şekilde bu dosyayı Visual Studio'daki kapatabilirsiniz.

#### <a name="AddNewIndexView"></a>Yeni öğe görünümü ekleme

Liste öğeleri için bir görünüm oluşturun aşağıdaki adımları kullanarak öğeleri oluşturmak için yeni bir görünüm nasıl oluşturduğunuz benzer:

1. Gelen **Çözüm Gezgini**, sağ **öğesi** klasörü yeniden seçin **Ekle**ve ardından **görünümü**.

1. İçinde **Görünüm Ekle** iletişim kutusunda, aşağıdaki değerleri güncelleştirin:
   
   * **Görünüm adı** kutusunda ***Create*** yazın.
   * **Şablon** kutusunda ***Oluştur***'u seçin.
   * **Model sınıfı** kutusunda ***Öğe (todo.Models)*** seçeneğini belirleyin.
   * Düzen sayfası kutusunda ***~/Views/Shared/_Layout.cshtml*** yazın.
   * **Add (Ekle)** seçeneğini belirleyin.
   
#### <a name="AddEditIndexView"></a>Düzenleme öğesi görünümü ekleme

Ve son olarak, aşağıdaki adımlarla bir öğeyi düzenlemek için bir görünüm ekleyin:

1. Gelen **Çözüm Gezgini**, sağ **öğesi** klasörü yeniden seçin **Ekle**ve ardından **görünümü**.

1. **Görünüm Ekle** iletişim kutusunda aşağıdakileri yapın:
   
   * **Görünüm adı** kutusunda ***Edit*** yazın.
   * **Şablon** kutusunda ***Düzenle***'yi seçin.
   * **Model sınıfı** kutusunda ***Öğe (todo.Models)*** seçeneğini belirleyin.
   * Düzen sayfası kutusunda ***~/Views/Shared/_Layout.cshtml*** yazın.
   * **Add (Ekle)** seçeneğini belirleyin.

Bunu yaptıktan sonra Bu görünümlere daha sonra geri olarak Visual Studio'daki tüm cshtml belgelerini kapatın.

## <a name="connect-to-cosmosdb"></a>5. adım: Azure Cosmos DB’ye bağlanma 

Standart MVC hallolduğuna göre Azure Cosmos DB'ye bağlanmak ve CRUD işlemleri gerçekleştirmek için kod eklemeye dönelim. 

### <a name="perform-crud-operations"></a> Veri çubuğunda CRUD işlemleri gerçekleştirme

Burada yapılacak ilk şey, bağlanmasına ve Azure Cosmos DB için mantığı içeren bir sınıf eklemektir. Bu öğreticide, biz TodoItemService.cs adlı bir sınıf içinde bu mantığı kapsülleyen. Bu kod, Azure Cosmos DB uç nokta değerleri formun yapılandırma dosyasını okur ve tamamlanmamış öğeleri listeleme, oluşturma, düzenleme ve öğeleri silme gibi CRUD işlemleri gerçekleştirir. 

1. Gelen **Çözüm Gezgini**, adlı projenize altında yeni bir klasör oluşturun **Hizmetleri**.

1. Sağ **Hizmetleri** klasörüne **Ekle**ve ardından **sınıfı**. Yeni bir sınıf adı **TodoItemService** seçip **Ekle**.

1. Aşağıdaki kodu ekleyin **TodoItemService** sınıfı ve o dosyasındaki kodu aşağıdaki kodla değiştirin:

   [!code-csharp[Main](~/samples-cosmosdb-dotnet-web-app/src/Services/TodoItemService.cs)]
 
1. Önceki kod, yapılandırma dosyasından bağlantı dize değerlerini okur. Azure Cosmos hesabınızın bağlantı dizesi değerleri güncelleştirmek için **Web.config** altına aşağıdaki satırları ekleyin ve dosya projenizde `<AppSettings>` bölümü:

   ```csharp
    <add key="endpoint" value="<enter the URI from the Keys blade of the Azure Portal>" />
    <add key="primaryKey" value="<enter the PRIMARY KEY, or the SECONDARY KEY, from the Keys blade of the Azure  Portal>" />
    <add key="database" value="Tasks" />
    <add key="container" value="Items" />
   ```
 
1. Uç nokta ve primarykey değerlerini hizmetinden alınan değerlerle güncelleştirin **anahtarları** Azure portalındaki dikey penceresinde. Kullanma **URI** kullanın ve uç nokta ayarının değeri olarak anahtarlar dikey penceresinden **birincil anahtar**, veya **ikincil anahtar** ayarlama anahtarları için anahtarlar dikey penceresinden. Bu Azure Cosmos uygulamaya artık şimdi DB'yi bağlama üstlenir uygulama mantığı ekleyin.

1. **Global.asax.cs**'yi açın ve **Application_Start** yöntemine aşağıdaki satırı ekleyin 
   
   ```csharp
   TodoItemService.Initialize().GetAwaiter().GetResult();
   ```

   Bu noktada çözümünüzün projenizin herhangi bir hata olmadan oluşturulabiliyor olması gerekir. Uygulamayı şimdi çalıştırırsanız **HomeController** ve **dizin** söz konusu denetleyici görünümünü açması gerekir. Biz başta Seçtiğimiz MVC şablonu projesinin varsayılan davranışı budur. Bu davranışı değiştirmek için bu MVC uygulamasındaki yönlendirmeyi değiştirelim.

1. Açık ***uygulama\_Start\RouteConfig.cs*** ile başlayan satırı bulun "Varsayılan:" ve aşağıdaki kodla güncelleştirin:

   ```csharp
   defaults: new { controller = "Item", action = "Index", id = UrlParameter.Optional }
   ```

   Bu kod, ASP.NET MVC artık olmadığını söyler. yerine yönlendirme davranışını denetlemek için URL'de bir değer belirtmediniz **giriş**, kullandığı **öğesi** denetleyicisi olarak ve **dizin**görünüm olarak.

Şimdi uygulamayı çalıştırırsanız, bunu içine yapılan çağrılar, **Itemcontroller** sonraki bölümde tanımladığınız TodoItemService sınıfından Getıtems yöntemleri çağırır. 

Bu projeyi şimdi oluşturur ve çalıştırırsanız buna benzeyen bir şey görürsünüz.    

![Bu veritabanı Öğreticisi tarafından oluşturulan Yapılacaklar listesi web uygulamasının ekran görüntüsü](./media/sql-api-dotnet-application-preview/build-and-run-the-project-now.png)


## <a name="run-the-application"></a>6. adım: Uygulamayı yerel olarak çalıştırma

Yerel makinenizde uygulamayı test etmek için aşağıdaki adımları kullanın:

1. Uygulamayı hata ayıklama modunda oluşturmak için Visual Studio'da F5 tuşuna basın. Bu işlemin uygulamayı oluşturması ve bir tarayıcıyı daha önce gördüğümüz boş kılavuz sayfasıyla başlatması gerekir:
   
   ![Bu veritabanı Öğreticisi tarafından oluşturulan Yapılacaklar listesi web uygulamasının ekran görüntüsü](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-create-an-item-a.png)
       
2. **Yeni Oluştur** bağlantısına tıklayın ve **Ad** ve **Açıklama** alanlarına değerler ekleyin. Bırakın **tamamlandı** onay kutusunu seçili Aksi halde yeni öğe tamamlanmış durumda eklenir ve ilk listede görünmez.
   
3. Tıklayın **Oluştur** ve geri yönlendirilirsiniz **dizin** görünümü ve, öğe listede görünür. Yapılacaklar listenize birkaç daha fazla öğe ekleyebilirsiniz.

    ![Dizin görünümünün ekran görüntüsü](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-create-an-item.png)
  
4. Listedeki bir **Öğe**'nin yanındaki **Düzenle**'ye tıklayın, böylece **Tamamlandı** bayrağı dahil olmak üzere nesnenizin herhangi bir özelliğini güncelleştirebileceğiniz **Düzenle** görünümüne gidersiniz. **Tamamlandı** bayrağını işaretler ve **Kaydet**'e tıklarsanız **Öğe** tamamlanmamış görevler listesinden kaldırılır.
   
   ![Tamamlandı kutusu işaretlenmiş şekilde dizin görünümünün ekran görüntüsü](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-completed-item.png)

5. Uygulamayı test ettikten sonra, uygulamanın hata ayıklamasını durdurmak için Ctrl+F5'e basın. Dağıtıma hazırsınız!

## <a name="deploy-the-application-to-azure"></a>7. adım: Uygulamayı dağıtma 
Artık uygulamanın tamamı Azure Cosmos DB ile doğru şekilde çalıştığına göre, bu web uygulamasını Azure App Service’e dağıtacağız.  

1. Bu uygulamayı yayımlamak için projeyi sağ **Çözüm Gezgini** seçip **Yayımla**.
   
2. İçinde **Yayımla** iletişim kutusunda **Microsoft Azure App Service**, ardından **Yeni Oluştur** bir App Service profili oluşturun veya seçin **seçin Varolan** mevcut bir profili kullanmak için.

3. Mevcut bir Azure App Service profili varsa, seçin bir **abonelik** açılır listeden. Kullanım **görünümü** filtresi kaynak grubu veya kaynak türüne göre sıralayabilirsiniz. Ardından gerekli Azure App Service'ı arayın ve seçin **Tamam**. 
   
   ![Visual Studio’da App Service iletişim kutusu](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-app-service.png)

4. Yeni bir Azure App Service profili oluşturmak için, **Yayımla** iletişim kutusunda **Yeni Oluştur**’a tıklayın. İçinde **App Service Oluştur** iletişim kutusunda, Web uygulaması adınız ve uygun aboneliği, kaynak grubu ve App Service planı'nı girin ve ardından **Oluştur**.

   ![Visual Studio’da App Service’i Oluştur iletişim kutusu](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-create-app-service.png)

Birkaç saniye içinde Visual Studio web uygulamanıza yayımlar ve Azure'da çalışan projenizi görebileceğiniz bir tarayıcıyı başlatacak!

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide bir ASP.NET MVC Azure Cosmos DB'de depolanan verilere erişmek web uygulaması oluşturmayı öğrendiniz. Şimdi bir sonraki makaleye geçebilirsiniz:

> [!div class="nextstepaction"]
> [Azure Cosmos DB SQL API hesabı içinde depolanan verilere erişmek için bir Java uygulaması oluşturma]( sql-api-java-application.md)


[Visual Studio Express]: https://www.visualstudio.com/products/visual-studio-express-vs.aspx
[Microsoft Web Platform Installer]: https://www.microsoft.com/web/downloads/platform.aspx
[Preventing Cross-Site Request Forgery]: https://go.microsoft.com/fwlink/?LinkID=517254
[Basic CRUD Operations in ASP.NET MVC]: https://go.microsoft.com/fwlink/?LinkId=317598
[GitHub]: https://github.com/Azure-Samples/cosmos-dotnet-todo-app
