---
title: "Azure SQL veritabanı ile bir ASP.NET uygulaması oluşturma | Microsoft Docs"
description: "Bir SQL veritabanına bağlantı ile Azure içinde çalışan bir ASP.NET uygulaması alma hakkında bilgi."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 03c584f1-a93c-4e3d-ac1b-c82b50c75d3e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 06/09/2017
ms.author: cephalin
ms.custom: mvc, devcenter
ms.openlocfilehash: db3be8068ef9e560614daa0e7f0dcf62467fd338
ms.sourcegitcommit: 3e3a5e01a5629e017de2289a6abebbb798cec736
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="build-an-aspnet-app-in-azure-with-sql-database"></a>Azure SQL veritabanı ile bir ASP.NET uygulaması oluşturma

[Azure Web Apps](app-service-web-overview.md) yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar. Bu öğreticide, bir veri tabanlı ASP.NET web uygulamasını Azure dağıtmak ve buna bağlanmak nasıl gösterilir [Azure SQL veritabanı](../sql-database/sql-database-technical-overview.md). İşlemi tamamladığınızda, Azure'da çalışan bir ASP.NET uygulamanız ve SQL veritabanına bağlı.

![Azure web uygulamasında yayımlanan ASP.NET uygulaması](./media/app-service-web-tutorial-dotnet-sqldatabase/azure-app-in-browser.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure SQL veritabanı oluşturma
> * Bir ASP.NET uygulaması SQL veritabanına bağlan
> * Uygulamayı Azure'a dağıtma
> * Veri modeli güncelleştirme ve uygulamayı yeniden dağıtın
> * Azure akış günlükleri, terminal
> * Azure portalında uygulama yönetme

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

* [Visual Studio 2017](https://www.visualstudio.com/downloads/)’yi aşağıdaki iş yükleri ile yükleyin:
  - **ASP.NET ve web geliştirme**
  - **Azure geliştirme**

  ![ASP.NET ve web geliştirme ile Azure geliştirme (Web ve Bulut altında)](media/app-service-web-tutorial-dotnet-sqldatabase/workloads.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Örneği indirme

[Örnek Proje indirme](https://github.com/Azure-Samples/dotnet-sqldb-tutorial/archive/master.zip).

Extract (sıkıştırmasını açın) *dotnet-sqldb-öğretici-master.zip* dosya.

Temel bir örnek proje içeren [ASP.NET MVC](https://www.asp.net/mvc) CRUD (Oluştur-okunur-güncelleştirme-Sil) uygulamasını kullanarak [Entity Framework Code First](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).

### <a name="run-the-app"></a>Uygulamayı çalıştırma

Açık *dotnet-sqldb-öğretici-ana/DotNetAppSqlDb.sln* dosyasını Visual Studio'da. 

Tür `Ctrl+F5` hata ayıklama olmadan uygulamayı çalıştırmak için. Uygulamanın varsayılan tarayıcınızda görüntülenir. Seçin **Yeni Oluştur** bağlayın ve birkaç oluşturun *Yapılacaklar* öğeleri. 

![Yeni ASP.NET Projesi iletişim kutusu](media/app-service-web-tutorial-dotnet-sqldatabase/local-app-in-browser.png)

Test **Düzenle**, **ayrıntıları**, ve **silmek** bağlantılar.

Uygulama, veritabanı ile bağlantı için bir veritabanı bağlamını kullanır. Bu örnekte, veritabanı bağlamı adlı bir bağlantı dizesi kullanır `MyDbConnection`. Bağlantı dizesi kümesinde *Web.config* dosya ve başvurulan *Models/MyDatabaseContext.cs* dosya. Bağlantı dizesi adı daha sonra öğreticide, Azure web uygulaması bir Azure SQL veritabanına bağlanmak için kullanılır. 

## <a name="publish-to-azure-with-sql-database"></a>Azure SQL veritabanı ile yayımlama

İçinde **Çözüm Gezgini**, sağ tıklatın, **DotNetAppSqlDb** proje ve seçin **Yayımla**.

![Çözüm Gezgini'nden yayımlama](./media/app-service-web-tutorial-dotnet-sqldatabase/solution-explorer-publish.png)

**Microsoft Azure App Service**’in seçili olduğundan emin olup **Yayımla**’ya tıklayın.

![Projeye genel bakış sayfasından yayımlama](./media/app-service-web-tutorial-dotnet-sqldatabase/publish-to-app-service.png)

Açılır yayımlama **App Service Oluştur** iletişim kutusu, Azure'da ASP.NET web uygulamanızı çalıştırmak için gereken tüm Azure kaynaklarına oluşturmanıza yardımcı olur.

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

**App Service Oluştur** iletişim kutusunda **Hesap ekle**’ye tıklayın ve ardından Azure aboneliğinizde oturum açın. Bir Microsoft hesabında zaten oturum açtıysanız hesabın Azure aboneliğinizi barındırdığından emin olun. Oturum açtığınız Microsoft hesabında Azure aboneliğiniz yoksa, doğru hesabı eklemek için tıklayın.
   
![Azure'da oturum açma](./media/app-service-web-tutorial-dotnet-sqldatabase/sign-in-azure.png)

Oturum açtıktan sonra bu iletişim kutusunda Azure web uygulamanız için gereken tüm kaynakları oluşturmaya hazır olursunuz.

### <a name="configure-the-web-app-name"></a>Web uygulaması adını yapılandırma

Oluşturulan web uygulaması adı tutun veya için başka bir benzersiz adını değiştirin (geçerli karakterler `a-z`, `0-9`, ve `-`). Web uygulaması adı varsayılan URL bir parçası olarak uygulamanız için kullanılır (`<app_name>.azurewebsites.net`, burada `<app_name>` web uygulaması adınız). Web uygulaması adı Azure tüm uygulamalar arasında benzersiz olması gerekir. 

![App service Oluştur iletişim kutusu](media/app-service-web-tutorial-dotnet-sqldatabase/wan.png)

> [!NOTE]
> ' A tıklamayın **oluşturma**. Önce bir SQL veritabanı sonraki adımda ayarlamanız gerekir.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[!INCLUDE [resource-group](../../includes/resource-group.md)]

**Kaynak Grubu**’nun yanındaki **Yeni** öğesine tıklayın.

![Kaynak grubu yanında, yeni'yi tıklatın.](media/app-service-web-tutorial-dotnet-sqldatabase/new_rg2.png)

Kaynak grubu adı **myResourceGroup**.

### <a name="create-an-app-service-plan"></a>App Service planı oluşturma

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

**App Service Planı**’nın yanındaki **Yeni** öğesine tıklayın. 

**App Service Planını Yapılandır** iletişim kutusunda yeni App Service planını aşağıdaki ayarlarla yapılandırın:

![App Service planı oluşturma](./media/app-service-web-tutorial-dotnet-sqldatabase/configure-app-service-plan.png)

| Ayar  | Önerilen değer | Daha fazla bilgi için |
| ----------------- | ------------ | ----|
|**Uygulama hizmeti planı**| myAppServicePlan | [App Service planları](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) |
|**Konum**| Batı Avrupa | [Azure bölgeleri](https://azure.microsoft.com/regions/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) |
|**Boyut**| Ücretsiz | [Fiyatlandırma katmanları](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)|

### <a name="create-a-sql-server-instance"></a>SQL Server örneği oluşturma

Bir veritabanı oluşturmadan önce gereken bir [Azure SQL Database mantıksal sunucusu](../sql-database/sql-database-features.md). Mantıksal sunucu, grup olarak yönetilen bir veritabanı grubu içerir.

Seçin **diğer Azure hizmetlerini keşfedin**.

![Web uygulaması adını yapılandırma](media/app-service-web-tutorial-dotnet-sqldatabase/web-app-name.png)

İçinde **Hizmetleri** sekmesini tıklatın,  **+**  yanındaki simge **SQL veritabanı**. 

![Hizmetler sekmesini tıklatın + SQL veritabanı yanındaki simge.](media/app-service-web-tutorial-dotnet-sqldatabase/sql.png)

İçinde **SQL veritabanını yapılandırma** iletişim kutusunda, tıklatın **yeni** yanına **SQL Server**. 

Benzersiz sunucu adı oluşturulur. Bu ad mantıksal sunucunuz için varsayılan URL bir parçası olarak kullanılır `<server_name>.database.windows.net`. Bunu Azure tüm mantıksal sunucu örnekleri arasında benzersiz olması gerekir. Sunucu adını değiştirmek, ancak bu öğreticide, üretilen değer tutun.

Bir yönetici kullanıcı adı ve parola ekleyin. Parola karmaşıklık gereksinimleri için bkz: [parola ilkesi](/sql/relational-databases/security/password-policy).

Bu kullanıcı adı ve parola unutmayın. Mantıksal sunucu örneğini daha sonra yönetmek için gereksinim duyarsınız.

![SQL Server örneği oluşturma](media/app-service-web-tutorial-dotnet-sqldatabase/configure-sql-database-server.png)

**Tamam** düğmesine tıklayın. Kapatmayın **SQL veritabanını yapılandırma** henüz iletişim.

### <a name="create-a-sql-database"></a>SQL Veritabanı oluşturma

İçinde **SQL veritabanını yapılandırma** iletişim: 

* Varsayılan olarak oluşturulan tutmak **veritabanı adı**.
* İçinde **bağlantı dizesi adı**, türü *MyDbConnection*. Bu ad başvuru bağlantı dizesi eşleşmelidir *Models/MyDatabaseContext.cs*.
* **Tamam**’ı seçin.

![SQL veritabanını Yapılandır](media/app-service-web-tutorial-dotnet-sqldatabase/configure-sql-database.png)

**App Service Oluştur** iletişim oluşturduğunuz kaynakları gösterir. **Oluştur**'a tıklayın. 

![oluşturduğunuz kaynakları](media/app-service-web-tutorial-dotnet-sqldatabase/app_svc_plan_done.png)

Azure kaynaklarını Oluşturma Sihirbazı'nı tamamladıktan sonra ASP.NET uygulamanızı Azure'da yayımlar. Dağıtılan uygulama URL'si ile varsayılan tarayıcı başlatılır. 

Birkaç Yapılacaklar öğelerini ekleyin.

![Azure web uygulamasında yayımlanan ASP.NET uygulaması](./media/app-service-web-tutorial-dotnet-sqldatabase/azure-app-in-browser.png)

Tebrikler! Veri tabanlı ASP.NET uygulamanızı Azure App Service'te çalışıyor.

## <a name="access-the-sql-database-locally"></a>Yerel olarak SQL veritabanına erişim

Visual Studio keşfedin ve yeni SQL veritabanınızı kolayca içinde yönetmenize olanak tanır **SQL Server Nesne Gezgini**.

### <a name="create-a-database-connection"></a>Bir veritabanı bağlantısı oluşturma

Gelen **Görünüm** menüsünde, select **SQL Server Nesne Gezgini**.

Üstündeki **SQL Server Nesne Gezgini**, tıklatın **SQL Server Ekle** düğmesi.

### <a name="configure-the-database-connection"></a>Veritabanı bağlantısını yapılandır

İçinde **Bağlan** iletişim kutusunda, genişletin **Azure** düğümü. Tüm SQL veritabanı örnekleri Azure burada listelenir.

Daha önce oluşturduğunuz SQL veritabanını seçin. Daha önce oluşturduğunuz bağlantı altındaki otomatik olarak doldurulur.

Daha önce oluşturduğunuz veritabanı yönetici parolasını yazın ve'ı tıklatın **Bağlan**.

![Visual Studio'dan veritabanı bağlantısını yapılandır](./media/app-service-web-tutorial-dotnet-sqldatabase/connect-to-sql-database.png)

### <a name="allow-client-connection-from-your-computer"></a>İstemci bağlantısı bilgisayarınızdan izin ver

**Yeni bir güvenlik duvarı kuralı oluşturmak** iletişim kutusu açılır. Varsayılan olarak, SQL veritabanı örneğinde bağlantılar Azure web uygulamanızın gibi Azure hizmetlerinden yalnızca izin verir. Veritabanınıza bağlanmak için SQL veritabanı örneğinde bir güvenlik duvarı kuralı oluşturun. Güvenlik duvarı kuralını yerel bilgisayarınızın genel IP adresi sağlar.

İletişim kutusu zaten bilgisayarınızın ortak IP adresi ile doldurulur.

Olduğundan emin olun **my istemci IP'si Ekle** seçilir ve tıklatın **Tamam**. 

![SQL veritabanı örneği için güvenlik duvarını ayarlama](./media/app-service-web-tutorial-dotnet-sqldatabase/sql-set-firewall.png)

Visual Studio, SQL veritabanı örneği için güvenlik duvarı ayarı oluşturma tamamlandığında, bağlantınızı görünür **SQL Server Nesne Gezgini**.

Burada, en sık karşılaşılan gerçekleştirebilirsiniz çalışma sorguları gibi veritabanı işlemlerini oluşturma görünümleri ve saklı yordamları ve daha fazla. 

Bağlantınızı genişletin > **veritabanları** > **&lt;veritabanınızı >** > **tabloları**. Sağ `Todoes` tablo ve seçin **görünüm verilerini**. 

![SQL veritabanı nesneleri keşfedin](./media/app-service-web-tutorial-dotnet-sqldatabase/explore-sql-database.png)

## <a name="update-app-with-code-first-migrations"></a>Code First geçişleri uygulamayı güncelleştirme

Veritabanı ve web uygulamanızda Azure güncelleştirmek için Visual Studio'da alıştığınız araçları kullanabilirsiniz. Bu adımda, Code First Migrations Entity Framework veritabanı şemanızı değişiklik ve Azure'da yayımlamak için kullanın.

Entity Framework Code First Migrations kullanma hakkında daha fazla bilgi için bkz: [Entity Framework 6 kod MVC 5 kullanarak ilk ile çalışmaya başlama](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).

### <a name="update-your-data-model"></a>Veri modelinizi güncelleştir

Açık _Models\Todo.cs_ Kod düzenleyicisinde. Aşağıdaki özellik ekleme `ToDo` sınıfı:

```csharp
public bool Done { get; set; }
```

### <a name="run-code-first-migrations-locally"></a>Code First Migrations yerel olarak çalıştırma

Yerel veritabanınızı güncelleştirme yapmak için birkaç komutları çalıştırın. 

Gelen **Araçları** menüsünde tıklatın **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.

Paket Yöneticisi konsolu penceresinde Code First geçişleri etkinleştir:

```PowerShell
Enable-Migrations
```

Bir geçiş ekleyin:

```PowerShell
Add-Migration AddProperty
```

Yerel veritabanı güncelleştirin:

```PowerShell
Update-Database
```

Tür `Ctrl+F5` uygulamayı çalıştırmak için. Düzenleme, Ayrıntılar, test ve bağlantıları oluşturun.

Uygulama hatasız yüklerse, Code First Migrations başarılı oldu. Uygulama mantığınızın bu yeni özellik henüz kullanmadığından ancak sayfanızı hala aynı görünür. 

### <a name="use-the-new-property"></a>Yeni özelliğini kullanın

Kodunuzu kullanmak için bazı değişiklikler yapmak `Done` özelliği. Bu öğreticide kolaylık olması için yalnızca değiştirme oluşturacağız `Index` ve `Create` görünümleri eylem özelliğine bakın.

Açık _Controllers\TodosController.cs_.

Bul `Create()` yöntemi satır 52 ve ekleme `Done` özelliklerinde listesine `Bind` özniteliği. İşiniz bittiğinde, `Create()` yöntemi imza aşağıdaki kod gibi görünür:

```csharp
public ActionResult Create([Bind(Include = "Description,CreatedDate,Done")] Todo todo)
```

Açık _Views\Todos\Create.cshtml_.

Razor kodunda görmelisiniz bir `<div class="form-group">` kullanan öğesi `model.Description`ve ardından başka bir `<div class="form-group">` kullanan öğesi `model.CreatedDate`. Bu iki öğenin hemen ardından, eklemek başka `<div class="form-group">` kullanan öğesi `model.Done`:

```csharp
<div class="form-group">
    @Html.LabelFor(model => model.Done, htmlAttributes: new { @class = "control-label col-md-2" })
    <div class="col-md-10">
        <div class="checkbox">
            @Html.EditorFor(model => model.Done)
            @Html.ValidationMessageFor(model => model.Done, "", new { @class = "text-danger" })
        </div>
    </div>
</div>
```

Açık _Views\Todos\Index.cshtml_.

Boş bir Ara `<th></th>` öğesi. Bu öğe yalnızca aşağıdaki Razor kodu ekleyin:

```csharp
<th>
    @Html.DisplayNameFor(model => model.Done)
</th>
```

Bul `<td>` içeren öğeyi `Html.ActionLink()` yardımcı yöntemler. _Yukarıdaki_ bu `<td>`, başka bir tane eklemek `<td>` aşağıdaki Razor kod bir öğesiyle:

```csharp
<td>
    @Html.DisplayFor(modelItem => item.Done)
</td>
```

Tüm değişiklikleri görmek için ihtiyacınız olan `Index` ve `Create` görünümleri. 

Tür `Ctrl+F5` uygulamayı çalıştırmak için.

Şimdi Yapılacaklar öğesi ekleyin ve denetleme **Bitti**. Ardından, giriş sayfanız tamamlanmış bir öğe olarak gösterilmesi gerekir. Unutmayın `Edit` değil görünümü göster `Done` alan, değişmedi çünkü `Edit` görünümü.

### <a name="enable-code-first-migrations-in-azure"></a>Azure'da Code First geçişleri etkinleştir

Kodunuzu değiştirmek artık göre works veritabanı geçiş dahil olmak üzere, Azure web uygulamanızı yayınlama ve SQL veritabanınızı Code First Migrations ile çok güncelleştirin.

Tıpkı, projenize sağ tıklayın ve önce seçin **Yayımla**.

Tıklatın **ayarları** yayımlama sihirbazını açın.

![Açık yayımlama ayarları](./media/app-service-web-tutorial-dotnet-sqldatabase/publish-settings.png)

Sihirbazı'nda tıklatın **sonraki**.

SQL veritabanı bağlantı dizesi içinde doldurulur emin olun **MyDatabaseContext (MyDbConnection)**. Seçmek için gerek duyabileceğiniz **myToDoAppDb** veritabanından açılır. 

Seçin **yürütme önce kod uygulamalı geçişler (uygulama başlatılırken çalışır)**, ardından **kaydetmek**.

![Azure web uygulamasında Code First geçişleri etkinleştir](./media/app-service-web-tutorial-dotnet-sqldatabase/enable-migrations.png)

### <a name="publish-your-changes"></a>Yaptığınız değişiklikleri yayımlama

Azure web uygulamanızda Code First Migrations etkinleştirildi, kod değişikliklerinizin yayımlayın.

Yayımlama sayfasında **Yayımla**'ya tıklayın.

Yapılacaklar öğelerini yeniden eklemeyi deneyin ve seçin **Bitti**, ve bunlar sayfanız tamamlanmış bir öğe olarak içinde gösterilmesi gerekir.

![Kod ilk geçişten sonra Azure web uygulaması](./media/app-service-web-tutorial-dotnet-sqldatabase/this-one-is-done.png)

Tüm mevcut Yapılacaklar öğelerini hala görüntülenir. ASP.NET uygulamanızı yayımladığınızda, SQL veritabanında var olan veri kaybı olmadığından. Ayrıca, Code First Migrations yalnızca veri şemasını değiştirir ve varolan verilerinizi dokunmaz.


## <a name="stream-application-logs"></a>Akış uygulama günlükleri

İzleme iletileri doğrudan Azure web uygulamasından Visual Studio'ya akışını sağlayabilirsiniz.

Açık _Controllers\TodosController.cs_.

Her eylem ile başlayan bir `Trace.WriteLine()` yöntemi. Bu kod, Azure web uygulamanızın izleme iletileri ekleme göstermek için eklenir.

### <a name="open-server-explorer"></a>Açık Sunucu Gezgini

Gelen **Görünüm** menüsünde, select **Sunucu Gezgini**. Azure web uygulamanız için günlük kaydını yapılandırmak **Sunucu Gezgini**. 

### <a name="enable-log-streaming"></a>Günlük akışı etkinleştir

İçinde **Sunucu Gezgini**, genişletin **Azure** > **uygulama hizmeti**.

Genişletme **myResourceGroup** kaynak grubu, oluşturduğunuz ilk Azure web uygulaması oluşturduğunuzda.

Azure web uygulamanızın sağ tıklatıp **akış günlüklerini görüntüle**.

![Günlük akışı etkinleştir](./media/app-service-web-tutorial-dotnet-sqldatabase/stream-logs.png)

Günlükleri şimdi içine akışı **çıkış** penceresi. 

![Çıktı penceresinde akış günlük](./media/app-service-web-tutorial-dotnet-sqldatabase/log-streaming-pane.png)

Bununla birlikte, izleme iletilerini henüz görmüyorsanız. Bu ilk seçtiğinizde, çünkü **akış günlüklerini görüntüle**, Azure web uygulamanızı izleme düzeyini ayarlar `Error`, hangi yalnızca günlükleri hata olayları (ile `Trace.TraceError()` yöntemi).

### <a name="change-trace-levels"></a>Değişiklik izleme düzeyi

Diğer izleme iletilerini çıktısını almak için izleme düzeyi değiştirmek için geri dönüp **Sunucu Gezgini**.

Azure web uygulamanızı yeniden sağ tıklatıp **görünüm ayarlarını**.

İçinde **uygulama günlüğü (dosya sistemi)** açılan listesinde, select **ayrıntılı**. **Kaydet** düğmesine tıklayın.

![Verbose için değişiklik izleme düzeyi](./media/app-service-web-tutorial-dotnet-sqldatabase/trace-level-verbose.png)

> [!TIP]
> Ne tür iletileri her düzeyi için görüntülenen görmek için farklı izleme düzeyleri ile deneyebilirsiniz. Örneğin, **bilgi** düzeyi tarafından oluşturulan tüm günlükleri içeren `Trace.TraceInformation()`, `Trace.TraceWarning()`, ve `Trace.TraceError()`, ancak tarafından oluşturulan günlükleri `Trace.WriteLine()`.
>
>

Web uygulamanızı yeniden tarayıcınızda gidin *http://&lt;, uygulama adı >. azurewebsites.net*, azure'da Yapılacaklar listesi uygulaması geçici tıklatmayı deneyin. İzleme iletileri şimdi akışı **çıkış** Visual Studio'daki.

```console
Application: 2017-04-06T23:30:41  PID[8132] Verbose     GET /Todos/Index
Application: 2017-04-06T23:30:43  PID[8132] Verbose     GET /Todos/Create
Application: 2017-04-06T23:30:53  PID[8132] Verbose     POST /Todos/Create
Application: 2017-04-06T23:30:54  PID[8132] Verbose     GET /Todos/Index
```



### <a name="stop-log-streaming"></a>Akış Günlüğü Durdur

Günlük akış hizmetini durdurmak için tıklatın **izlemeyi durdurmak** düğmesini **çıkış** penceresi.

![Akış Günlüğü Durdur](./media/app-service-web-tutorial-dotnet-sqldatabase/stop-streaming.png)

## <a name="manage-your-azure-web-app"></a>Azure web uygulamanızı yönetme

Git [Azure portal](https://portal.azure.com) oluşturduğunuz web uygulaması görmek için. 



Sol menüden **App Service**’e ve ardından Azure web uygulamanızın adına tıklayın.

![Portaldan Azure web uygulamasına gitme](./media/app-service-web-tutorial-dotnet-sqldatabase/access-portal.png)

Web uygulamanızın sayfasında indiniz. 

Varsayılan olarak, portal gösterir **genel bakış** sayfası. Bu sayfa, uygulamanızın nasıl çalıştığını gösterir. Buradan ayrıca göz atma, durdurma, başlatma, yeniden başlatma ve silme gibi temel yönetim görevlerini gerçekleştirebilirsiniz. Sayfanın sol tarafında sekmeleri açabilir farklı yapılandırma sayfalarında gösterilir. 

![Azure portalında App Service sayfası](./media/app-service-web-tutorial-dotnet-sqldatabase/web-app-blade.png)

[!INCLUDE [Clean up section](../../includes/clean-up-section-portal-web-app.md)]

<a name="next"></a>

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure SQL veritabanı oluşturma
> * Bir ASP.NET uygulaması SQL veritabanına bağlan
> * Uygulamayı Azure'a dağıtma
> * Veri modeli güncelleştirme ve uygulamayı yeniden dağıtın
> * Azure akış günlükleri, terminal
> * Azure portalında uygulama yönetme

Web uygulaması için özel bir DNS adı eşleme öğrenmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Mevcut bir özel DNS adını Azure Web Apps ile eşleme](app-service-web-tutorial-custom-domain.md)
