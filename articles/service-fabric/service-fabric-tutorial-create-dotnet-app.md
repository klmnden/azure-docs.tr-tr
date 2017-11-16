---
title: "Service Fabric için .NET uygulaması oluşturma | Microsoft Docs"
description: "Bir ASP.NET Core ön uç ve bir güvenilir hizmet durum bilgisi olan arka uç ile uygulama oluşturma ve bir küme için uygulama dağıtma hakkında bilgi edinin."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/08/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 497582138504250b3c4a77dab440d29ad928a7d8
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
# <a name="create-and-deploy-an-application-with-an-aspnet-core-web-api-front-end-service-and-a-stateful-back-end-service"></a>Bir ASP.NET çekirdek Web API ön uç hizmeti ve durum bilgisi olan bir arka uç hizmeti ile bir uygulama oluşturun ve dağıtın
Bu öğretici bir dizi birini bir parçasıdır.  Azure Service Fabric uygulaması bir ASP.NET çekirdek Web API ön uç ve verilerinizi depolamak için durum bilgisi olan bir arka uç hizmeti ile nasıl oluşturulacağını öğreneceksiniz. İşlemi tamamladığınızda, oylama bir durum bilgisi olan bir arka uç hizmetinde kümedeki Oylama sonuçlarını kaydettiği ön uç bir ASP.NET Core web uygulamasıyla sahip. El ile oylama uygulaması oluşturmak istemiyorsanız, şunları yapabilirsiniz [kaynak kodunu indirebilir](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/) tamamlanan uygulama için ve için İleri atlayabilirsiniz [üzerinden oylama örnek uygulama yol](#walkthrough_anchor).

![Uygulama diyagramı](./media/service-fabric-tutorial-create-dotnet-app/application-diagram.png)

Bölümünde bir dizi öğrenin nasıl yapılır:

> [!div class="checklist"]
> * Durum bilgisi olan güvenilir bir hizmet olarak bir ASP.NET çekirdek Web API hizmeti oluşturma
> * Durum bilgisiz web hizmeti olarak bir ASP.NET çekirdek Web uygulaması hizmeti oluşturma
> * Durum bilgisi olan hizmeti ile iletişim kurmak için ters proxy kullan

Bu öğretici serisinde öğrenin nasıl yapılır:
> [!div class="checklist"]
> * Bir .NET Service Fabric uygulaması oluşturma
> * [Uzak bir küme için uygulama dağıtma](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * [CI/CD Visual Studio Team Services kullanarak yapılandırma](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)
> * [İzleme ve tanılama uygulama için ayarlama](service-fabric-tutorial-monitoring-aspnet.md)

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiye başlamadan önce:
- Bir Azure aboneliğiniz yoksa, oluşturma bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- [Visual Studio 2017 yükleme](https://www.visualstudio.com/) yükleyip **Azure geliştirme** ve **ASP.NET ve web geliştirme** iş yükleri.
- [Service Fabric SDK yükleme](service-fabric-get-started.md)

## <a name="create-an-aspnet-web-api-service-as-a-reliable-service"></a>Bir ASP.NET Web API hizmeti güvenilir bir hizmet olarak oluşturma
İlk olarak, web ön uç ASP.NET Core kullanarak oylama uygulamasının oluşturun. ASP.NET Core API web ve modern web kullanıcı Arabirimi oluşturmak için kullanabileceğiniz bir basit, platformlar arası web geliştirme altyapısıdır. ASP.NET Core Service Fabric ile nasıl tümleşik çalıştığını tam anlamak için okuma tavsiye [Service Fabric Reliable Services ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md) makalesi. Şimdilik, hızlı bir şekilde başlamak için bu öğreticiyi izleyebilirsiniz. ASP.NET Çekirdeği hakkında daha fazla bilgi için bkz: [ASP.NET Core belgeleri](https://docs.microsoft.com/aspnet/core/).

> [!NOTE]
> Bu öğretici dayanır [ASP.NET Core araçları için Visual Studio 2017](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/start-mvc). Visual Studio 2015 için .NET Core araçları artık güncelleştiriliyor.

1. Visual Studio'yu **yönetici** olarak başlatın.

2. Bir proje oluşturun **dosya**->**yeni**->**proje**

3. **Yeni Proje** iletişim kutusunda **Bulut > Service Fabric Uygulaması**'nı seçin.

4. Uygulama adı **oylama** ve basın **Tamam**.

   ![Visual Studio'da yeni proje iletişim kutusu](./media/service-fabric-tutorial-create-dotnet-app/new-project-dialog.png)

5. Üzerinde **yeni yapı hizmeti** sayfasında, **durum bilgisiz ASP.NET Core**ve hizmetinizin adını **VotingWeb**.
   
   ![ASP.NET web hizmeti yeni hizmet iletişim kutusunda seçme](./media/service-fabric-tutorial-create-dotnet-app/new-project-dialog-2.png) 

6. Sonraki sayfanın bir dizi ASP.NET Core proje şablonları sağlar. Bu öğretici için seçin **Web uygulaması (MVC)**. 
   
   ![ASP.NET proje türü seçin](./media/service-fabric-tutorial-create-dotnet-app/vs-new-aspnet-project-dialog.png)

   Visual Studio, uygulama ve hizmet projesi oluşturur ve bunları Çözüm Gezgini'nde görüntüler.

   ![ASP.NET core Web API hizmeti içeren uygulama oluşturulduktan sonra Çözüm Gezgini]( ./media/service-fabric-tutorial-create-dotnet-app/solution-explorer-aspnetcore-service.png)

### <a name="add-angularjs-to-the-votingweb-service"></a>AngularJS VotingWeb hizmete ekleyin
Ekleme [AngularJS](http://angularjs.org/) yerleşik kullanarak hizmetinize [Bower Destek](/aspnet/core/client-side/bower). Açık *bower.json* ve Açısal ve önyükleme Açısal girişlerini ekleyin, sonra yaptığınız değişiklikleri kaydedin.

```json
{
  "name": "asp.net",
  "private": true,
  "dependencies": {
    "bootstrap": "3.3.7",
    "jquery": "2.2.0",
    "jquery-validation": "1.14.0",
    "jquery-validation-unobtrusive": "3.2.6",
    "angular": "v1.6.5",
    "angular-bootstrap": "v1.1.0"
  }
}
```
Kaydetme sırasında *bower.json* dosya, Angular projenizin içinde yüklü *wwwroot/lib* klasör. Ayrıca, içinde listelenir *bağımlılıkları/Bower* klasör.

### <a name="update-the-sitejs-file"></a>Site.js dosyasını güncelleştirme
Açık *wwwroot/js/site.js* dosya.  Giriş görünümleri tarafından kullanılan JavaScript ile içeriğini değiştirin:

```javascript
var app = angular.module('VotingApp', ['ui.bootstrap']);
app.run(function () { });

app.controller('VotingAppController', ['$rootScope', '$scope', '$http', '$timeout', function ($rootScope, $scope, $http, $timeout) {

    $scope.refresh = function () {
        $http.get('api/Votes?c=' + new Date().getTime())
            .then(function (data, status) {
                $scope.votes = data;
            }, function (data, status) {
                $scope.votes = undefined;
            });
    };

    $scope.remove = function (item) {
        $http.delete('api/Votes/' + item)
            .then(function (data, status) {
                $scope.refresh();
            })
    };

    $scope.add = function (item) {
        var fd = new FormData();
        fd.append('item', item);
        $http.put('api/Votes/' + item, fd, {
            transformRequest: angular.identity,
            headers: { 'Content-Type': undefined }
        })
            .then(function (data, status) {
                $scope.refresh();
                $scope.item = undefined;
            })
    };
}]);
```

### <a name="update-the-indexcshtml-file"></a>Index.cshtml dosyasını güncelleştirme
Açık *Views/Home/Index.cshtml* dosya, giriş denetleyiciye belirli görünüm.  Yaptığınız değişiklikleri kaydedin sonra aşağıdaki içeriğini değiştirin.

```html
@{
    ViewData["Title"] = "Service Fabric Voting Sample";
}

<div ng-controller="VotingAppController" ng-init="refresh()">
    <div class="container-fluid">
        <div class="row">
            <div class="col-xs-8 col-xs-offset-2 text-center">
                <h2>Service Fabric Voting Sample</h2>
            </div>
        </div>

        <div class="row">
            <div class="col-xs-8 col-xs-offset-2">
                <form class="col-xs-12 center-block">
                    <div class="col-xs-6 form-group">
                        <input id="txtAdd" type="text" class="form-control" placeholder="Add voting option" ng-model="item" />
                    </div>
                    <button id="btnAdd" class="btn btn-default" ng-click="add(item)">
                        <span class="glyphicon glyphicon-plus" aria-hidden="true"></span>
                        Add
                    </button>
                </form>
            </div>
        </div>

        <hr />

        <div class="row">
            <div class="col-xs-8 col-xs-offset-2">
                <div class="row">
                    <div class="col-xs-4">
                        Click to vote
                    </div>
                </div>
                <div class="row top-buffer" ng-repeat="vote in votes.data">
                    <div class="col-xs-8">
                        <button class="btn btn-success text-left btn-block" ng-click="add(vote.key)">
                            <span class="pull-left">
                                {{vote.key}}
                            </span>
                            <span class="badge pull-right">
                                {{vote.value}} Votes
                            </span>
                        </button>
                    </div>
                    <div class="col-xs-4">
                        <button class="btn btn-danger pull-right btn-block" ng-click="remove(vote.key)">
                            <span class="glyphicon glyphicon-remove" aria-hidden="true"></span>
                            Remove
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
```

### <a name="update-the-layoutcshtml-file"></a>_Layout.cshtml dosyasını güncelleştirme
Açık *Views/Shared/_Layout.cshtml* dosya, ASP.NET uygulaması için varsayılan düzeni.  Yaptığınız değişiklikleri kaydedin sonra aşağıdaki içeriğini değiştirin.

```html
<!DOCTYPE html>
<html ng-app="VotingApp" xmlns:ng="http://angularjs.org">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>@ViewData["Title"]</title>

    <link href="~/lib/bootstrap/dist/css/bootstrap.min.css" rel="stylesheet" />
    <link href="~/css/site.css" rel="stylesheet" />

</head>
<body>
    <div class="container body-content">
        @RenderBody()
    </div>

    <script src="~/lib/jquery/dist/jquery.js"></script>
    <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
    <script src="~/lib/angular/angular.js"></script>
    <script src="~/lib/angular-bootstrap/ui-bootstrap-tpls.js"></script>
    <script src="~/js/site.js"></script>

    @RenderSection("Scripts", required: false)
</body>
</html>
```

### <a name="update-the-votingwebcs-file"></a>VotingWeb.cs dosyasını güncelleştirme
Açık *VotingWeb.cs* WebListener web sunucusu kullanarak durum bilgisi olmayan hizmetin ASP.NET Core WebHost oluşturur dosya.  

Ekleme `using System.Net.Http;` dosyanın en üstüne yönergesi.  

Değiştir `CreateServiceInstanceListeners()` işlev şununla sonra yaptığınız değişiklikleri kaydedin.

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    return new ServiceInstanceListener[]
    {
        new ServiceInstanceListener(serviceContext =>
            new WebListenerCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
            {
                ServiceEventSource.Current.ServiceMessage(serviceContext, $"Starting WebListener on {url}");

                return new WebHostBuilder().UseWebListener()
                            .ConfigureServices(
                                services => services
                                    .AddSingleton<StatelessServiceContext>(serviceContext)
                                    .AddSingleton<HttpClient>())
                            .UseContentRoot(Directory.GetCurrentDirectory())
                            .UseStartup<Startup>()
                            .UseApplicationInsights()
                            .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
                            .UseUrls(url)
                            .Build();
            }))
    };
}
```

### <a name="add-the-votescontrollercs-file"></a>VotesController.cs dosyası ekleme
Oylama Eylemler tanımlayan bir denetleyici ekleyin. Sağ tıklayın **denetleyicileri** klasörünü seçip **Ekle -> Yeni öğe sınıfı ->**.  "VotesController.cs" dosya adı ve'ı tıklatın **Ekle**.  

Dosya içeriğini yaptığınız değişiklikleri kaydedin sonra aşağıdaki değiştirin.  Daha sonra [VotesController.cs dosyasını güncelleştirme](#updatevotecontroller_anchor), bu dosyayı okumak ve arka uç hizmetinden oylama veri yazmak için değiştirilecek.  Şimdilik, denetleyici görünümüne statik dize verilerini döndürür.

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using Newtonsoft.Json;
using System.Text;
using System.Net.Http;
using System.Net.Http.Headers;

namespace VotingWeb.Controllers
{
    [Produces("application/json")]
    [Route("api/Votes")]
    public class VotesController : Controller
    {
        private readonly HttpClient httpClient;

        public VotesController(HttpClient httpClient)
        {
            this.httpClient = httpClient;
        }

        // GET: api/Votes
        [HttpGet]
        public async Task<IActionResult> Get()
        {
            List<KeyValuePair<string, int>> votes= new List<KeyValuePair<string, int>>();
            votes.Add(new KeyValuePair<string, int>("Pizza", 3));
            votes.Add(new KeyValuePair<string, int>("Ice cream", 4));

            return Json(votes);
        }
     }
}
```

### <a name="configure-the-listening-port"></a>Dinleme bağlantı noktasını yapılandırın
VotingWeb ön uç hizmeti oluşturulduğunda, Visual Studio bir bağlantı noktası üzerinde dinleme hizmeti için rastgele seçer.  VotingWeb hizmeti bu uygulama için ön uç gibi davranır ve dış trafiği kabul eder, sağlandığından hizmet sabit bir bağlayabilir ve bağlantı noktası iyi biliyor. Çözüm Gezgini'nde açık *VotingWeb/PackageRoot/ServiceManifest.xml*.  Bul **Endpoint** kaynak **kaynakları** bölümünde ve değiştirme **bağlantı noktası** 80 veya başka bir bağlantı noktası değeri. Dağıtma ve uygulama yerel olarak çalıştırmak için uygulama dinleme bağlantı noktası açık ve bilgisayarınızdaki kullanılabilir olması gerekir.

```xml
<Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" Port="80" />
    </Endpoints>
  </Resources>
```

Ayrıca 'F5' kullanarak hata ayıklamasını yaparken doğru bağlantı noktasına bir web tarayıcısı açar şekilde oylama Proje uygulama URL'si özelliği değeri güncelleştirin.  Çözüm Gezgini'nde seçin **oylama** proje ve güncelleştirme **uygulama URL'si** özelliği.

![Uygulama URL'si](./media/service-fabric-tutorial-deploy-app-to-party-cluster/application-url.png)

### <a name="deploy-and-run-the-application-locally"></a>Dağıtma ve uygulama yerel olarak çalıştırma
Şimdi devam edin ve uygulamayı çalıştırın. Hata ayıklama için uygulamayı dağıtmak üzere Visual Studio'da `F5` tuşuna basın. `F5`Visual Studio olarak daha önce açılmazsa başarısız **yönetici**.

> [!NOTE]
> Uygulamayı yerel olarak ilk kez çalıştırdığınızda ve dağıttığınızda, Visual Studio hata ayıklama için yerel bir küme oluşturur.  Küme oluşturma biraz zaman alabilir. Küme oluşturma durumu, Visual Studio çıkış penceresinde görüntülenir.

Bu noktada, web uygulamanızı aşağıdaki gibi görünmelidir:

![ASP.NET Core ön uç](./media/service-fabric-tutorial-create-dotnet-app/debug-front-end.png)

Uygulamanın hata ayıklamasını durdurmak için Visual Studio ve tuşuna dön **Shift + F5**.

## <a name="add-a-stateful-back-end-service-to-your-application"></a>Durum bilgisi olan bir arka uç hizmeti uygulamanıza ekleyin
Biz uygulamamız içinde çalışan bir ASP.NET Web API hizmeti sahip olduğunuza göre devam edelim ve uygulamamızı içinde bazı verileri depolamak için bir durum bilgisi olan güvenilir hizmet ekleyin.

Service Fabric, güvenilir koleksiyonları kullanarak hizmetinizin içinde veri hak tutarlı ve güvenilir bir şekilde depolamanızı sağlar. C# koleksiyonları kullanan herkes için tanıdık yüksek oranda kullanılabilir ve güvenilir koleksiyon sınıfları kümesi güvenilir koleksiyonlarıdır.

Bu öğreticide, güvenilir bir koleksiyonda bir sayaç değeri depolayan bir hizmet oluşturun.

1. Çözüm Gezgini'nde sağ **Hizmetleri** uygulama içinde proje ve seçin **Ekle > Yeni yapı hizmeti**.
   
    ![Varolan bir uygulamaya yeni bir hizmet ekleme](./media/service-fabric-tutorial-create-dotnet-app/vs-add-new-service.png)

2. İçinde **yeni yapı hizmeti** iletişim kutusunda, seçin **durum bilgisi olan ASP.NET Core**ve ad hizmeti **VotingData** ve basın **Tamam**.

    ![Visual Studio'da yeni hizmet iletişim kutusu](./media/service-fabric-tutorial-create-dotnet-app/add-stateful-service.png)

    Hizmet projeniz oluşturulduktan sonra uygulamanızda iki hizmetler sahiptir. Uygulamanızı yapılandırmaya devam ederken, daha fazla hizmet aynı şekilde ekleyebilirsiniz. Her bağımsız olarak sürümlü hem de yükseltilmiş olabilir.

3. Sonraki sayfanın bir dizi ASP.NET Core proje şablonları sağlar. Bu öğretici için seçin **Web API**.

    ![ASP.NET proje türü seçin](./media/service-fabric-tutorial-create-dotnet-app/vs-new-aspnet-project-dialog2.png)

    Visual Studio hizmet projesi oluşturur ve bunları Çözüm Gezgini'nde görüntüler.

    ![Çözüm Gezgini](./media/service-fabric-tutorial-create-dotnet-app/solution-explorer-aspnetcore-webapi-service.png)

### <a name="add-the-votedatacontrollercs-file"></a>VoteDataController.cs dosyası ekleme

İçinde **VotingData** projesine sağ tıklayın **denetleyicileri** klasörünü seçip **Ekle -> Yeni öğe sınıfı ->**. "VoteDataController.cs" dosya adı ve'ı tıklatın **Ekle**. Dosya içeriğini yaptığınız değişiklikleri kaydedin sonra aşağıdaki değiştirin.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.ServiceFabric.Data;
using System.Threading;
using Microsoft.ServiceFabric.Data.Collections;

namespace VotingData.Controllers
{
    [Route("api/[controller]")]
    public class VoteDataController : Controller
    {
        private readonly IReliableStateManager stateManager;

        public VoteDataController(IReliableStateManager stateManager)
        {
            this.stateManager = stateManager;
        }

        // GET api/VoteData
        [HttpGet]
        public async Task<IActionResult> Get()
        {
            var ct = new CancellationToken();

            var votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

            using (ITransaction tx = this.stateManager.CreateTransaction())
            {
                var list = await votesDictionary.CreateEnumerableAsync(tx);

                var enumerator = list.GetAsyncEnumerator();

                var result = new List<KeyValuePair<string, int>>();

                while (await enumerator.MoveNextAsync(ct))
                {
                    result.Add(enumerator.Current);
                }

                return Json(result);
            }
        }

        // PUT api/VoteData/name
        [HttpPut("{name}")]
        public async Task<IActionResult> Put(string name)
        {
            var votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

            using (ITransaction tx = this.stateManager.CreateTransaction())
            {
                await votesDictionary.AddOrUpdateAsync(tx, name, 1, (key, oldvalue) => oldvalue + 1);
                await tx.CommitAsync();
            }

            return new OkResult();
        }

        // DELETE api/VoteData/name
        [HttpDelete("{name}")]
        public async Task<IActionResult> Delete(string name)
        {
            var votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

            using (ITransaction tx = this.stateManager.CreateTransaction())
            {
                if (await votesDictionary.ContainsKeyAsync(tx, name))
                {
                    await votesDictionary.TryRemoveAsync(tx, name);
                    await tx.CommitAsync();
                    return new OkResult();
                }
                else
                {
                    return new NotFoundResult();
                }
            }
        }
    }
}
```


## <a name="connect-the-services"></a>Hizmetlere bağlanma
Bu sonraki adımda size iki hizmet bağlanmak ve uygulama alma ve arka uç hizmetinden bilgi oylama ayarlama ön uç Web olun.

Service Fabric reliable services ile nasıl iletişim kuracağını tam esneklik sağlar. Tek bir uygulama içinde TCP üzerinden erişilebilir Hizmetleri olabilir. Bir HTTP REST API erişilebilir diğer hizmetler ve hala diğer hizmetleri web yuvalarını erişilebilir. Kullanılabilir seçenekler ve söz konusu bileşim arka plan için bkz: [hizmetleriyle iletişim kurmasını](service-fabric-connect-and-communicate-with-services.md).

Bu öğreticide, kullanıyoruz [ASP.NET çekirdek Web API](service-fabric-reliable-services-communication-aspnetcore.md).

<a id="updatevotecontroller" name="updatevotecontroller_anchor"></a>

### <a name="update-the-votescontrollercs-file"></a>VotesController.cs dosyasını güncelleştirme
İçinde **VotingWeb** proje, açık *Controllers/VotesController.cs* dosya.  Değiştir `VotesController` sınıf tanımı içeriğini aşağıdakiyle sonra yaptığınız değişiklikleri kaydedin.

```csharp
    public class VotesController : Controller
    {
        private readonly HttpClient httpClient;
        string serviceProxyUrl = "http://localhost:19081/Voting/VotingData/api/VoteData";
        string partitionKind = "Int64Range";
        string partitionKey = "0";

        public VotesController(HttpClient httpClient)
        {
            this.httpClient = httpClient;
        }

        // GET: api/Votes
        [HttpGet]
        public async Task<IActionResult> Get()
        {
            IEnumerable<KeyValuePair<string, int>> votes;

            HttpResponseMessage response = await this.httpClient.GetAsync($"{serviceProxyUrl}?PartitionKind={partitionKind}&PartitionKey={partitionKey}");

            if (response.StatusCode != System.Net.HttpStatusCode.OK)
            {
                return this.StatusCode((int)response.StatusCode);
            }

            votes = JsonConvert.DeserializeObject<List<KeyValuePair<string, int>>>(await response.Content.ReadAsStringAsync());

            return Json(votes);
        }

        // PUT: api/Votes/name
        [HttpPut("{name}")]
        public async Task<IActionResult> Put(string name)
        {
            string payload = $"{{ 'name' : '{name}' }}";
            StringContent putContent = new StringContent(payload, Encoding.UTF8, "application/json");
            putContent.Headers.ContentType = new MediaTypeHeaderValue("application/json");

            string proxyUrl = $"{serviceProxyUrl}/{name}?PartitionKind={partitionKind}&PartitionKey={partitionKey}";

            HttpResponseMessage response = await this.httpClient.PutAsync(proxyUrl, putContent);

            return new ContentResult()
            {
                StatusCode = (int)response.StatusCode,
                Content = await response.Content.ReadAsStringAsync()
            };
        }

        // DELETE: api/Votes/name
        [HttpDelete("{name}")]
        public async Task<IActionResult> Delete(string name)
        {
            HttpResponseMessage response = await this.httpClient.DeleteAsync($"{serviceProxyUrl}/{name}?PartitionKind={partitionKind}&PartitionKey={partitionKey}");

            if (response.StatusCode != System.Net.HttpStatusCode.OK)
            {
                return this.StatusCode((int)response.StatusCode);
            }

            return new OkResult();

        }
    }
```
<a id="walkthrough" name="walkthrough_anchor"></a>

## <a name="walk-through-the-voting-sample-application"></a>Yol üzerinden oylama örnek uygulama
Oylama uygulaması iki hizmetinden oluşur:
- Web ön uç hizmeti (VotingWeb) - bir ASP.NET Core web web sayfası hizmet ön uç hizmeti ve düzenlemenizi sağlayan web arka uç hizmeti ile iletişim için API'ler.
- Arka uç hizmetine (VotingData)-oy sonuçları güvenilir sözlükte depolamak için bir API sunar bir ASP.NET Core web hizmeti kalıcı disk üzerinde.

![Uygulama diyagramı](./media/service-fabric-tutorial-create-dotnet-app/application-diagram.png)

Aşağıdaki olaylar, uygulamada oy oluşur:
1. JavaScript oy isteği için web ön uç hizmeti web API'si bir HTTP PUT İsteği gönderir.

2. Web ön uç hizmeti bulun ve bir HTTP PUT İsteği arka uç hizmetine iletmek için bir proxy kullanır.

3. Arka uç hizmetine gelen isteği alır ve güncelleştirilmiş sonuç kümesi içinde birden çok düğüm çoğaltılan alır ve diskte kalıcı bir güvenilir bir sözlük depolar. Hiçbir veritabanı gerektiği şekilde uygulamanın tüm veri kümesinde depolanır.

## <a name="debug-in-visual-studio"></a>Visual Studio'da hata ayıklama
Visual Studio uygulamasında hata ayıklama sırasında yerel bir Service Fabric geliştirme küme kullanıyor. Hata ayıklama deneyiminizi senaryonuz için ayarlamak için seçeneğiniz vardır. Bu uygulamada, veri arka uç hizmetimizi güvenilir sözlüğünü kullanarak depolarız. Hata ayıklayıcıyı durdurduğunuzda visual Studio uygulama varsayılan başına kaldırır. Uygulama kaldırma verileri de kaldırılacak arka uç hizmetinde neden olur. Hata ayıklama oturumları arasında veri kalıcı hale getirmek için değiştirebileceğiniz **uygulama hata ayıklama modu** bir özellik olarak **oylama** Visual Studio projesi.

Kod içinde neler aramak için aşağıdaki adımları tamamlayın:
1. Açık **VotesController.cs** dosya ve web API'ın bir kesme noktası belirleyerek **Put** yöntemi (satır 47) - Visual Studio'daki Çözüm Gezgini'nde dosyayı arayabilirsiniz.

2. Açık **VoteDataController.cs** dosya ve bu web API'nin bir kesme noktası belirleyerek **Put** yöntemi (satır 50).

3. Tarayıcıya geri dönün ve oylama seçeneği tıklatın veya yeni bir oylama seçeneği ekleyin. Web ön uç 's API denetleyicisi içinde ilk kesme noktası isabet.
    
    1. Burada tarayıcısında JavaScript bir isteği ön uç hizmeti olan web API denetleyicisi gönderir budur.
    
    ![Oy ön uç Hizmet Ekle](./media/service-fabric-tutorial-create-dotnet-app/addvote-frontend.png)

    2. İlk biz ReverseProxy URL'si için arka uç hizmetimizi oluşturmak **(1)**.
    3. Biz PUT HTTP isteği için ReverseProxy Gönder sonra **(2)**.
    4. Son olarak yanıt arka uç hizmetinden istemciye döndürürüz **(3)**.

4. Tuşuna **F5** devam etmek için
    1. Artık arka uç hizmetinde kesme noktasında bulunur.
    
    ![Oy arka uç hizmeti ekleme](./media/service-fabric-tutorial-create-dotnet-app/addvote-backend.png)

    2. Yönteminin ilk satırında **(1)** kullanıyoruz `StateManager` almak veya adlı güvenilir sözlüğü eklemek için `counts`.
    3. Kullanarak bu işlem, güvenilir bir sözlükteki değerlerle tüm etkileşimleri gerektiren deyimi **(2)** bu işlem oluşturur.
    4. İşlemde biz sonra oylama seçeneği için ilgili anahtar değerini güncelleştirin ve işlemi tamamlar **(3)**. Yürütme yöntemi döndürür, veri sözlüğünde güncelleştirilmiş ve kümedeki diğer düğümlere çoğaltılan sonra. Veri şimdi güvenle kümede depolanır ve arka uç hizmeti üzerinden kullanılabilir veri yaşamaya diğer düğümlere başarısız olabilir.
5. Tuşuna **F5** devam etmek için

Hata ayıklama oturumu durdurmak için basın **Shift + F5**.


## <a name="next-steps"></a>Sonraki adımlar
Öğreticinin bu bölümünde, öğrenilen nasıl yapılır:

> [!div class="checklist"]
> * Durum bilgisi olan güvenilir bir hizmet olarak bir ASP.NET çekirdek Web API hizmeti oluşturma
> * Durum bilgisiz web hizmeti olarak bir ASP.NET çekirdek Web uygulaması hizmeti oluşturma
> * Durum bilgisi olan hizmeti ile iletişim kurmak için ters proxy kullan

Sonraki öğretici ilerleyin:
> [!div class="nextstepaction"]
> [Uygulamayı Azure'a dağıtma](service-fabric-tutorial-deploy-app-to-party-cluster.md)
