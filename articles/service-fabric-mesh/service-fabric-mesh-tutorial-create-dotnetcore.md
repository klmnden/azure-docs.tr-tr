---
title: 'Öğretici: Çok hizmet sunan bir web uygulaması oluşturup Service Fabric Mesh’e dağıtma ve bu uygulamada hata ayıklama | Microsoft Docs'
description: Bu öğreticide bir arka uç web hizmetiyle iletişim kuran bir ASP.NET Core web sitesini içeren çok hizmet sunan bir Azure Service Fabric Mesh uygulaması oluşturacak, yerel ortamda hatalarını ayıklayacak ve Azure'da yayımlayacaksınız.
services: service-fabric-mesh
documentationcenter: .net
author: TylerMSFT
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/17/2018
ms.author: twhitney
ms.custom: mvc, devcenter
ms.openlocfilehash: 692a384c3695001677b7a3d6161b77f3f015ad7c
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39125878"
---
# <a name="tutorial-create-debug-and-deploy-a-multi-service-web-application-to-service-fabric-mesh"></a>Öğretici: Çok hizmet sunan bir web uygulaması oluşturup Service Fabric Mesh’e dağıtma ve bu uygulamada hata ayıklama

Bu öğretici, bir dizinin birinci bölümüdür. ASP.NET web ön ucu ve ASP.NET Core Web API arka uç hizmeti olan bir Azure Service Fabric Mesh uygulaması oluşturmayı öğreneceksiniz. Ardından yerel geliştirme kümenizde uygulamanın hatalarını ayıklayacak ve uygulamayı Azure'da yayımlayacaksınız. İşlemleri tamamladığınızda Azure Service Fabric Mesh'te çalışan bir Service Fabric Mesh uygulamasında hizmetler arası çağrı oluşturmayı gösteren basit bir yapılacak işler uygulamasına sahip olacaksınız.

Yapılacak işler uygulamasını el ile oluşturmak istemiyorsanız, tamamlanmış uygulamanın [kaynak kodunu indirebilir](https://github.com/azure-samples/service-fabric-mesh) ve [Yerel ortamda uygulama hatalarını ayıklama](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md) bölümüne atlayabilirsiniz.

Serinin birinci bölümünde şunları öğrenirsiniz:

> [!div class="checklist"]
> * ASP.NET web ön ucu içeren bir Service Fabric Mesh uygulaması oluşturma.
> * Yapılacak işleri temsil eden bir model oluşturma.
> * Bir arka uç hizmeti oluşturma ve ondan veri alma.
> * Arka uç hizmeti için Model Görünüm Denetleyicisi modelinin bir parçası olarak bir denetleyici ve DataContext ekleme.
> * Yapılacak işleri görüntülemek için bir web sayfası oluşturma.
> * Arka uç hizmetini tanımlayan ortam değişkenlerini oluşturma

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Bir Service Fabric Mesh uygulaması oluşturma
> * [Yerel ortamda uygulama hatalarını ayıklama](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md)
> * [Uygulamayı Azure’da yayımlama](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md)

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce:

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturabilirsiniz.

* [Geliştirme ortamınızı ayarladığınızdan](service-fabric-mesh-howto-setup-developer-environment-sdk.md) ve Service Fabric çalışma zamanı, SDK, Docker ve Visual Studio 2017'yi yüklediğinizden emin olun.

## <a name="create-a-service-fabric-mesh-project"></a>Service Fabric Mesh projesi oluşturma

Visual Studio'yu çalıştırın ve **Dosya** > **Yeni** > **Proje...** yolunu izleyin.

**Yeni Proje** iletişim kutusunun en üstündeki **Ara** kutusuna `mesh` yazın. **Service Fabric Mesh Uygulaması** şablonunu seçin. (Bu şablonu görmüyorsanız [geliştirme ortamınızı ayarlama](service-fabric-mesh-howto-setup-developer-environment-sdk.md) bölümünde anlatılan şekilde Mesh SDK'sını ve VS araçları önizleme sürümünü yüklediğinizden emin olun.)  

**Ad** kutusuna `todolistapp` yazın ve **Konum** kutusunda projenin dosyalarını depolamak istediğiniz klasörü belirtin.

**Çözüm için dizin oluştur** kutusunun işaretli olduğundan emin olun ve **Tamam**'a tıklayarak Service Fabric Mesh projesini oluşturun.

![Visual Studio yeni Service Fabric Mesh projesi iletişim kutusu](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-new-project.png)

Ardından **Yeni Service Fabric Hizmeti** iletişim kutusu açılır.

### <a name="create-the-web-front-end-service"></a>Web ön uç hizmetini oluşturma

**Yeni Service Fabric Hizmeti** iletişim kutusunda **ASP.NET Core** proje türünü seçin ve **Kapsayıcı işletim sistemi** için **Windows**'un seçildiğinden emin olun.

**Hizmet Adı**'nı **WebFrontEnd** yapın. ASP.NET Core hizmetini oluşturmak için **Tamam**'a basın.

![Visual Studio yeni Service Fabric Mesh projesi iletişim kutusu](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-new-service-fabric-service.png)

Ardından **Yeni ASP.NET Core Web Uygulaması** iletişim kutusu açılır. **Yeni ASP.NET Core Web Uygulaması** iletişim kutusunda **Web Uygulaması**'nı seçip **Tamam**'a tıklayın.

![Visual Studio yeni ASP.NET Core uygulaması](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-new-aspnetcore-app.png)

Artık bir Service Fabric Mesh uygulamanız var. Şimdi yapılacak işler bilgilerini içerecek modeli oluşturun.

## <a name="create-the-to-do-items-model"></a>Yapılacak işler öğelerini içeren modeli oluşturma

Kolaylık olması için yapılacak işler bellekte bir liste halinde depolanır. Yapılacak işler için bir sınıf kitaplığı ve onları barındıracak bir liste oluşturun. **todolistapp** yüklü Visual Studio'da **Dosya** > **Ekle** > **Yeni Proje**'yi seçin.

**Yeni Proje** iletişim kutusunun en üstündeki **Ara** kutusuna `C# .net core class` yazın. **Sınıf Kitaplığı (.NET Core)** şablonunu seçin.

**Ad** kutusuna `Model` yazın. Sınıf kitaplığını oluşturmak için **Tamam**'a tıklayın.

Çözüm gezgininin **Model** bölümünde **Class1.cs** öğesine sağ tıklayıp **Yeniden adlandır**'ı seçin. Sınıfı **ToDoItem.cs** olarak yeniden adlandırın. Tüm başvuruları yeniden adlandırmak isteyip istemediğiniz sorulursa **Evet**'e tıklayın.

Boş `class ToDoItem` içeriğini şununla değiştirin:

```csharp
public class ToDoItem
{
    public string Description { get; set; }
    public int Index { get; set; }
    public bool Completed { get; set; }

    public ToDoItem(string description)
    {
        Description = description;
        Index = 0;
    }

    public static ToDoItem Load(string description, int index, bool completed)
    {
        ToDoItem newItem = new ToDoItem(description)
        {
            Index = index,
            Completed = completed
        };

        return newItem;
    }
}
```

Bu sınıf, yapılacak işler öğelerini temsil eder.

Visual Studio'da **Model** sınıf kitaplığına sağ tıklayın ve **Ekle** > **Sınıf...** yolunu izleyerek yapılacak işler öğelerini barındıracak bir liste oluşturun. **Yeni Öğe Ekle** iletişim kutusu açılır. **Ad** için `ToDoList.cs` yazın ve **Ekle**'ye tıklayın.

**ToDoList.cs** içinde boş `class ToDoList` öğesini şununla değiştirin:

```csharp
public class ToDoList
{
    private List<ToDoItem> _items;

    public string Name { get; set; }
    public IEnumerable<ToDoItem> Items { get => _items; }

    public ToDoList(string name)
    {
        Name = name;
        _items = new List<ToDoItem>();
    }

    public ToDoItem Add(string description)
    {
        var item = new ToDoItem(description);
        _items.Add(item);
        item.Index = _items.IndexOf(item);
        return item;
    }
    public void Add(ToDoItem item)
    {
        _items.Add(item);
        item.Index = _items.Count - 1;
    }

    public ToDoItem RemoveAt(int index)
    {
        if (index >= 0 && index < _items.Count)
        {
            var result = _items[index];
            _items.RemoveAt(index);

            // Reorder items
            for (int i = index; i < _items.Count; i++)
            {
                _items[i].Index = i;
            }

            return result;
        }
        else
        {
            throw new IndexOutOfRangeException();
        }
    }
}
```

Ardından yapılacak işler öğelerini takip edecek Service Fabric hizmetini oluşturun.

## <a name="create-the-back-end-service"></a>Arka uç sunucusunu oluşturma

Visual Studio'nun **Çözüm Gezgini** penceresinde **todolistapp** öğesine sağ tıklayın ve **Ekle** > **Yeni Service Fabric Hizmeti...** öğesini seçin.

**Yeni Service Fabric Hizmeti** iletişim kutusu açılır. **ASP.NET Core** proje türünü seçin ve **Kapsayıcı işletim sistemi** ayarının **Windows** olduğundan emin olun.

**Hizmet Adı** olarak **ToDoService** yazın. ASP.NET Core hizmetini oluşturmak için **Tamam**'a tıklayın. Ardından **Yeni ASP.NET Core Web Uygulaması** iletişim kutusu açılır. Bu iletişim kutusunda **API**'yi ve ardından **Tamam**'ı seçin. Çözüme hizmet için bir proje eklenir.

![Visual Studio yeni ASP.NET Core uygulaması](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-new-webapi.png)

Arka uç hizmeti arabirim sağlamadığından hizmet başlatıldığında tarayıcının çalıştırılması ayarını kapatın. **Çözüm Gezgini**'nde **ToDoService** öğesine sağ tıklayıp **Özellikler**'i seçin. Açılan özellikler penceresinin sol tarafındaki **Hata ayıklama** sekmesini seçin ve **Tarayıcıyı başlat** kutusunun seçimini kaldırın. Değişikliği kaydetmek için **Ctrl+S** tuşlarına basın.

Bu hizmet yapılacak işler bilgilerini tuttuğundan Model sınıf kitaplığına başvuru ekleyin. Çözüm Gezgini'nde **ToDoService** öğesine sağ tıklayıp **Ekle** > **Başvuru...** yolunu izleyin. **Başvuru Yöneticisi** iletişim kutusu görüntülenir.

**Başvuru Yöneticisi**'nde **Model** onay kutusunu seçin ve **Tamam**'a tıklayın.

### <a name="add-a-data-context"></a>Veri bağlamı ekleme

Ardından veri modelinden veri sunumunu gerçekleştiren bir veri bağlamı oluşturun.

Veri bağlamı sınıfı eklemek için çözüm gezgininde **ToDoService** öğesine sağ tıklayıp **Ekle** > **Sınıf** yolunu izleyin.
Açılan **Yeni Öğe Ekle** iletişim kutusunda **Sınıf** öğesinin seçili olduğundan emin olun ve **Ad** için `DataContext` yazıp **Ekle**'ye tıklayın.

**DataContext.cs** içinde boş `class DataContext` içeriğini şununla değiştirin:

```csharp
public static class DataContext
{
    public static Model.ToDoList ToDoList { get; } = new Model.ToDoList("Azure learning List");

    static DataContext()
    {
        ToDoList = new Model.ToDoList("Main List");

        // Seed to-do list

        ToDoList.Add(Model.ToDoItem.Load("Learn about microservices", 0, true));
        ToDoList.Add(Model.ToDoItem.Load("Learn about Service Fabric", 1, true));
        ToDoList.Add(Model.ToDoItem.Load("Learn about Service Fabric Mesh", 2, false));
    }
}
```

Bu minimal veri bağlamı, örnek yapılacak işler öğeleri ekler ve bunlara erişim sağlar.

### <a name="add-a-controller"></a>Denetleyici ekleme

HTTP isteklerini işleyen ve HTTP yanıtını oluşturan varsayılan denetleyici, **ToDoService** projesi oluşturulduğunda şablon tarafından sağlanmıştır. **Çözüm Gezgini**'nin **ToDoService** bölümünde **Denetleyiciler** klasörünü açarak **ValuesController.cs** dosyasına ulaşın. 

**ValuesController.cs** dosyasına sağ tıklayıp **Yeniden adlandır**'ı seçin. Dosyayı `ToDoController.cs` olarak yeniden adlandırın. Tüm başvuruları yeniden adlandırmak için bir istem görünürse **Evet**'e tıklayın.

**ToDoController.cs** dosyasını açın ve `class ToDoController` içeriğini şununla değiştirin:

```csharp
[Route("api/[controller]")]
public class ToDoController : Controller
{
    // GET api/todo
    [HttpGet]
    public IEnumerable<Model.ToDoItem> Get()
    {
        return DataContext.ToDoList.Items;
    }

    // GET api/todo/5
    [HttpGet("{index}")]
    public Model.ToDoItem Get(int index)
    {
        return DataContext.ToDoList.Items.ElementAt(index);
    }

    //// POST api/values
    //[HttpPost]
    //public void Post([FromBody]string value)
    //{
    //}

    //// PUT api/values/5
    //[HttpPut("{id}")]
    //public void Put(int id, [FromBody]string value)
    //{
    //}

    // DELETE api/values/5
    [HttpDelete("{index}")]
    public void Delete(int index)
    {
    }
}
```

Odağı başka bir hizmetle iletişim kurma üzerinde tutmak için bu öğreticide ekleme, silme ve diğer işlemler kullanılmamıştır.

## <a name="create-the-web-page-that-displays-to-do-items"></a>Yapılacak işler öğelerini görüntüleyecek web sayfasını oluşturma

Arka uç hizmetini uyguladıktan sonra yapılacak işler öğelerini gösterecek web sayfasını kodlayın. Aşağıdaki adımlar **WebFrontEnd** projesinde gerçekleştirilecektir.

Yapılacak işler öğelerini görüntüleyen web sayfasının **ToDoItem** sınıfına ve listesine erişmesi gerekir.
**Çözüm Gezgini**'nde **WebFrontEnd** öğesine sağ tıklayıp **Ekle** > **Başvuru...** yolunu izleyerek Model projesine başvuru ekleyin. **Başvuru Yöneticisi** iletişim kutusu görüntülenir.

**Başvuru Yöneticisi**'nde **Model** onay kutusuna ve **Tamam**'a tıklayın.

**Çözüm Gezgini**'nde **WebFrontEnd** > **Sayfalar** > **Index.cshtml** yolunu izleyerek Index.cshtml sayfasını açın. **Index.cshtml** sayfasını açın.

Dosyanın içeriğinin tamamını aşağıdaki yapılacak işler öğelerini görüntüleyen basit bir tabloyu tanımlayan HTML koduyla değiştirin:

```HTML
@page
@model IndexModel
@{
    ViewData["Title"] = "Home page";
}

<div>
    <table class="table-bordered">
        <thead>
            <tr>
                <th>Description</th>
                <th>Done?</th>
            </tr>
        </thead>
        <tbody>
            @foreach (var item in Model.Items)
            {
                <tr>
                    <td>@item.Description</td>
                    <td>@item.Completed</td>
                </tr>
            }
        </tbody>
    </table>
</div>
```

**Çözüm Gezgini**'nde **Index.cshtml** sayfasını ve ardından **Index.cshtml.cs** dosyasını açarak Index sayfasının kodunu açın. **Index.cshtml.cs** dosyasının en üstüne `using System.Net.Http;` ifadesini ekleyin.

`public class IndexModel` içeriğini şununla değiştirin:

```csharp
public class IndexModel : PageModel
{
    public Model.ToDoItem[] Items = new Model.ToDoItem[] { };

    public void OnGet()
    {
        HttpClient client = new HttpClient();

        using (HttpResponseMessage response = client.GetAsync(backendUrl).GetAwaiter().GetResult())
        {
            if (response.StatusCode == System.Net.HttpStatusCode.OK)
            {
                Items = Newtonsoft.Json.JsonConvert.DeserializeObject<Model.ToDoItem[]>(response.Content.ReadAsStringAsync().Result);
            }
        }
    }

    private static string backendDNSName = $"{Environment.GetEnvironmentVariable("ServiceName")}";
    private static Uri backendUrl = new Uri($"http://{backendDNSName}:{Environment.GetEnvironmentVariable("ApiHostPort")}/api/todo");
}
```

### <a name="create-environment-variables"></a>Ortam değişkenlerini oluşturma

Arka uç hizmetiyle iletişim kurmak için URL'sini kullanmanız gerekir. Bu öğreticinin amaçları doğrultusunda aşağıdaki kod parçası (IndexModel öğesinin bir parçası olarak yukarıda tanımlanan) ortam değişkenlerini okuyarak URL'yi oluşturur:

```csharp
private static string backendDNSName = $"{Environment.GetEnvironmentVariable("ServiceName")}";
private static Uri backendUrl = new Uri($"http://{backendDNSName}:{Environment.GetEnvironmentVariable("ApiHostPort")}/api/todo");
```

URL, hizmet adından ve bağlantı noktasından oluşur. Bu bilgilerin tümü **ToDoService** projesindeki service.yaml dosyasında yer alır.

**Çözüm Gezgini**'nde **ToDoService** projesine gidin ve **Hizmet Kaynakları** > **service.yaml** dosyasını açın.

![Şekil 1 - ToDoService service.yaml dosyası](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-serviceyaml-port.png)

* Hizmet adı `ToDoService`, `services:` altında ve `name:` sonrasında bulunur, bkz. yukarıdaki şekilde (1).
* Bağlantı noktası `20008`, `endpoints:` altında ve `port:` sonrasında bulunur, bkz. yukarıdaki şekilde (2). Projenizin bağlantı noktası numarası farklı olabilir.

Ardından WebFrontEnd projesinin arka uç hizmetini çağırmasını sağlamak için hizmet adı ve bağlantı noktası numarasını temsil eden ortam değişkenleri tanımlanır.

**Çözüm Gezgini**'nde **WebFrontEnd** > **Hizmet Kaynakları** > **service.yaml** dosyasına gideren arka uç hizmeti adresini belirten değişkenleri tanımlayın.

service.yaml dosyasının `environmentVariables` bölümüne aşağıdaki değişkenleri ekleyin. Boşluklar önemli olduğundan eklediğiniz değişkenleri `environmentVariables:` altındaki diğer değişkenlerle hizalayın.

> [!IMPORTANT]
> service.yaml dosyasındaki değişkenleri girintilemek için sekme değil boşluk kullanılması gerekir, aksi halde dosya derlenmez. Ortam değişkeni eklediğinizde Visual Studio sekme ekleyebilir. Tüm sekmeleri boşluklarla değiştirin. **Derleme** hata ayıklama çıkışında hata görünse de uygulama başlatılacaktır. Ancak sekmeleri boşluklarla değiştirene kadar çalışmayacaktır. service.yaml dosyasında sekme bulunmadığından emin olmak için **Düzenle**  > **Gelişmiş**  > **Boşlukları Görüntüle** yolunu izleyerek Visual Studio düzenleyicisindeki boşlukları görünür duruma getirebilirsiniz.

**WebFrontEnd** projenizin **service.yaml** dosyasının aşağıdakine benzer olması gerekir ancak `ApiHostPort` değeriniz muhtemelen farklı olacaktır:

![WebFrontEnd projesindeki Service.yaml dosyası](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-serviceyaml-envvars.png)

Artık Service Fabric Mesh uygulaması görüntüsünü arka uç web hizmetiyle birlikte yerel kümenizde derlemek ve dağıtmak için hazırsınız.

## <a name="next-steps"></a>Sonraki adımlar

Öğreticinin bu bölümünde, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * ASP.NET web ön ucu içeren bir Service Fabric Mesh uygulaması oluşturma.
> * Yapılacak işleri temsil eden bir model oluşturma.
> * Bir arka uç hizmeti oluşturma ve ondan veri alma.
> * Arka uç hizmeti için Model Görünüm Denetleyicisi modelinin bir parçası olarak bir denetleyici ve DataContext ekleme.
> * Yapılacak işleri görüntülemek için bir web sayfası oluşturma.
> * Arka uç hizmetini tanımlayan ortam değişkenlerini oluşturma

Sonraki öğreticiye ilerleyin:
> [!div class="nextstepaction"]
> [Yerel ortamda çalışan bir Service Fabric Mesh uygulamasında hata ayıklama](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md)
