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
ms.date: 01/17/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: f4b3c766ee46233cd4ec2d195e39d0b68516952f
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="create-and-deploy-an-application-with-an-aspnet-core-web-api-front-end-service-and-a-stateful-back-end-service"></a>Bir ASP.NET çekirdek Web API ön uç hizmeti ve durum bilgisi olan bir arka uç hizmeti ile bir uygulama oluşturun ve dağıtın
Bu öğretici bir dizi birini bir parçasıdır.  Azure Service Fabric uygulaması bir ASP.NET çekirdek Web API ön uç ve verilerinizi depolamak için durum bilgisi olan bir arka uç hizmeti ile nasıl oluşturulacağını öğreneceksiniz. Bitirdiğinizde, oylama sonuçlarını kümedeki durum bilgisi içeren arka uç hizmetine kaydeden bir ASP.NET Core web ön ucuna sahip oylama uygulaması sağlanır. El ile oylama uygulaması oluşturmak istemiyorsanız, şunları yapabilirsiniz [kaynak kodunu indirebilir](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/) tamamlanan uygulama için ve için İleri atlayabilirsiniz [üzerinden oylama örnek uygulama yol](#walkthrough_anchor).

![Uygulama Diyagramı](./media/service-fabric-tutorial-create-dotnet-app/application-diagram.png)

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
- [Visual Studio 2017 yükleme](https://www.visualstudio.com/) sürüm 15.3 veya üstü **Azure geliştirme** ve **ASP.NET ve web geliştirme** iş yükleri.
- [Service Fabric SDK yükleme](service-fabric-get-started.md)

## <a name="create-an-aspnet-web-api-service-as-a-reliable-service"></a>Bir ASP.NET Web API hizmeti güvenilir bir hizmet olarak oluşturma
İlk olarak, web ön uç ASP.NET Core kullanarak oylama uygulamasının oluşturun. ASP.NET Core API web ve modern web kullanıcı Arabirimi oluşturmak için kullanabileceğiniz bir basit, platformlar arası web geliştirme altyapısıdır. ASP.NET Core Service Fabric ile nasıl tümleşik çalıştığını tam anlamak için okuma tavsiye [Service Fabric Reliable Services ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md) makalesi. Şimdilik, hızlı bir şekilde başlamak için bu öğreticiyi izleyebilirsiniz. ASP.NET Çekirdeği hakkında daha fazla bilgi için bkz: [ASP.NET Core belgeleri](https://docs.microsoft.com/aspnet/core/).

1. Visual Studio'yu **yönetici** olarak başlatın.

2. Bir proje oluşturun **dosya**->**yeni**->**proje**

3. **Yeni Proje** iletişim kutusunda **Bulut > Service Fabric Uygulaması**'nı seçin.

4. Uygulama adı **oylama** ve basın **Tamam**.

   ![Visual Studio'da yeni proje iletişim kutusu](./media/service-fabric-tutorial-create-dotnet-app/new-project-dialog.png)

5. Üzerinde **yeni yapı hizmeti** sayfasında, **durum bilgisiz ASP.NET Core**ve hizmetinizin adını **VotingWeb**.
   
   ![ASP.NET web hizmeti yeni hizmet iletişim kutusunda seçme](./media/service-fabric-tutorial-create-dotnet-app/new-project-dialog-2.png) 

6. Sonraki sayfanın bir dizi ASP.NET Core proje şablonları sağlar. Bu öğretici için seçin **Web uygulaması (Model-View-Controller)**. 
   
   ![ASP.NET proje türü seçin](./media/service-fabric-tutorial-create-dotnet-app/vs-new-aspnet-project-dialog.png)

   Visual Studio, uygulama ve hizmet projesi oluşturur ve bunları Çözüm Gezgini'nde görüntüler.

   ![ASP.NET core Web API hizmeti içeren uygulama oluşturulduktan sonra Çözüm Gezgini]( ./media/service-fabric-tutorial-create-dotnet-app/solution-explorer-aspnetcore-service.png)

### <a name="add-angularjs-to-the-votingweb-service"></a>AngularJS VotingWeb hizmete ekleyin
Ekleme [AngularJS](http://angularjs.org/) hizmeti kullanmaya [Bower Destek](/aspnet/core/client-side/bower). İlk olarak, bir Bower yapılandırma dosyası projeye ekleyin.  Çözüm Gezgini'nde sağ **VotingWeb** seçip **Ekle -> Yeni öğe**. Seçin **Web** ve ardından **Bower yapılandırma dosyası**.  *Bower.json* dosyası oluşturulur.

Açık *bower.json* ve Açısal ve önyükleme Açısal girişlerini ekleyin, sonra yaptığınız değişiklikleri kaydedin.

```json
{
  "name": "asp.net",
  "private": true,
  "dependencies": {
    "bootstrap": "3.3.7",
    "jquery": "3.2.1",
    "jquery-validation": "1.16.0",
    "jquery-validation-unobtrusive": "3.2.6",
    "angular": "v1.6.8",
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
                        <input id="txtAdd" type="text" class="form-control" placeholder="Add voting option" ng-model="item"/>
                    </div>
                    <button id="btnAdd" class="btn btn-default" ng-click="add(item)">
                        <span class="glyphicon glyphicon-plus" aria-hidden="true"></span>
                        Add
                    </button>
                </form>
            </div>
        </div>

        <hr/>

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
    <meta charset="utf-8"/>
    <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
    <title>@ViewData["Title"]</title>

    <link href="~/lib/bootstrap/dist/css/bootstrap.min.css" rel="stylesheet"/>
    <link href="~/css/site.css" rel="stylesheet"/>

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
        new ServiceInstanceListener(
            serviceContext =>
                new KestrelCommunicationListener(
                    serviceContext,
                    "ServiceEndpoint",
                    (url, listener) =>
                    {
                        ServiceEventSource.Current.ServiceMessage(serviceContext, $"Starting Kestrel on {url}");

                        return new WebHostBuilder()
                            .UseKestrel()
                            .ConfigureServices(
                                services => services
                                    .AddSingleton<HttpClient>(new HttpClient())
                                    .AddSingleton<FabricClient>(new FabricClient())
                                    .AddSingleton<StatelessServiceContext>(serviceContext))
                            .UseContentRoot(Directory.GetCurrentDirectory())
                            .UseStartup<Startup>()
                            .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
                            .UseUrls(url)
                            .Build();
                    }))
    };
}
```

Ayrıca ekleyin `GetVotingDataServiceName` sorgulanan hizmet adı döndürür yöntemi:

```csharp
internal static Uri GetVotingDataServiceName(ServiceContext context)
{
    return new Uri($"{context.CodePackageActivationContext.ApplicationName}/VotingData");
}
```

### <a name="add-the-votescontrollercs-file"></a>VotesController.cs dosyası ekleme
Oylama eylemleri tanımlar bir denetleyici ekleyin. Sağ tıklayın **denetleyicileri** klasörünü seçip **Ekle -> Yeni öğe sınıfı ->**.  "VotesController.cs" dosya adı ve'ı tıklatın **Ekle**.  

Dosya içeriğini yaptığınız değişiklikleri kaydedin sonra aşağıdaki değiştirin.  Daha sonra [VotesController.cs dosyasını güncelleştirme](#updatevotecontroller_anchor), bu dosyayı okuma ve arka uç hizmetinden oylama veri yazma şekilde değiştirilir.  Şimdilik, denetleyici görünümüne statik dize verilerini döndürür.

```csharp
namespace VotingWeb.Controllers
{
    using System;
    using System.Collections.Generic;
    using System.Fabric;
    using System.Fabric.Query;
    using System.Linq;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Text;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Mvc;
    using Newtonsoft.Json;

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
VotingWeb ön uç hizmeti oluşturulduğunda, Visual Studio bir bağlantı noktası üzerinde dinleme hizmeti için rastgele seçer.  VotingWeb hizmeti bu uygulama için ön uç gibi davranır ve dış trafiği kabul eder, sağlandığından hizmet sabit bir bağlayabilir ve bağlantı noktası iyi biliyor.  [Hizmet bildirimi](service-fabric-application-and-service-manifests.md) hizmet uç noktalarına bildirir. Çözüm Gezgini'nde açık *VotingWeb/PackageRoot/ServiceManifest.xml*.  Bul **Endpoint** kaynak **kaynakları** bölümünde ve değiştirme **bağlantı noktası** 80 veya başka bir bağlantı noktası değeri. Dağıtma ve uygulama yerel olarak çalıştırmak için uygulama dinleme bağlantı noktası açık ve bilgisayarınızdaki kullanılabilir olması gerekir.

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
Uygulamada bir ASP.NET Web API hizmetinin çalıştığından, bir tane uygulamada bazı verileri depolamak için bir durum bilgisi olan güvenilir hizmetini ekleyin.

Service Fabric, güvenilir koleksiyonları kullanarak hizmetinizin içinde veri hak tutarlı ve güvenilir bir şekilde depolamanızı sağlar. C# koleksiyonları kullanan herkes için tanıdık yüksek oranda kullanılabilir ve güvenilir koleksiyon sınıfları kümesi güvenilir koleksiyonlarıdır.

Bu öğreticide, güvenilir bir koleksiyonda bir sayaç değeri depolayan bir hizmet oluşturun.

1. Çözüm Gezgini'nde sağ **Hizmetleri** uygulama içinde proje ve seçin **Ekle > Yeni yapı hizmeti**.
    
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
// ------------------------------------------------------------
//  Copyright (c) Microsoft Corporation.  All rights reserved.
//  Licensed under the MIT License (MIT). See License.txt in the repo root for license information.
// ------------------------------------------------------------

namespace VotingData.Controllers
{
    using System.Collections.Generic;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.ServiceFabric.Data;
    using Microsoft.ServiceFabric.Data.Collections;

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
            CancellationToken ct = new CancellationToken();

            IReliableDictionary<string, int> votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

            using (ITransaction tx = this.stateManager.CreateTransaction())
            {
                IAsyncEnumerable<KeyValuePair<string, int>> list = await votesDictionary.CreateEnumerableAsync(tx);

                IAsyncEnumerator<KeyValuePair<string, int>> enumerator = list.GetAsyncEnumerator();

                List<KeyValuePair<string, int>> result = new List<KeyValuePair<string, int>>();

                while (await enumerator.MoveNextAsync(ct))
                {
                    result.Add(enumerator.Current);
                }

                return this.Json(result);
            }
        }

        // PUT api/VoteData/name
        [HttpPut("{name}")]
        public async Task<IActionResult> Put(string name)
        {
            IReliableDictionary<string, int> votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

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
            IReliableDictionary<string, int> votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

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
Bu sonraki adımda, iki hizmet bağlanın ve ön uç Web uygulama alma ve arka uç hizmetinden bilgi oylama ayarlama olun.

Service Fabric reliable services ile nasıl iletişim kuracağını tam esneklik sağlar. Tek bir uygulama içinde TCP üzerinden erişilebilir Hizmetleri olabilir. Bir HTTP REST API erişilebilir diğer hizmetler ve hala diğer hizmetleri web yuvalarını erişilebilir. Kullanılabilir seçenekler ve söz konusu bileşim arka plan için bkz: [hizmetleriyle iletişim kurmasını](service-fabric-connect-and-communicate-with-services.md).

Bu öğreticide kullanmak [ASP.NET çekirdek Web API](service-fabric-reliable-services-communication-aspnetcore.md).

<a id="updatevotecontroller" name="updatevotecontroller_anchor"></a>

### <a name="update-the-votescontrollercs-file"></a>VotesController.cs dosyasını güncelleştirme
İçinde **VotingWeb** proje, açık *Controllers/VotesController.cs* dosya.  Değiştir `VotesController` sınıf tanımı içeriğini aşağıdakiyle sonra yaptığınız değişiklikleri kaydedin.

```csharp
public class VotesController : Controller
{
    private readonly HttpClient httpClient;
    private readonly FabricClient fabricClient;
    private readonly StatelessServiceContext serviceContext;

    public VotesController(HttpClient httpClient, StatelessServiceContext context, FabricClient fabricClient)
    {
        this.fabricClient = fabricClient;
        this.httpClient = httpClient;
        this.serviceContext = context;
    }

    // GET: api/Votes
    [HttpGet("")]
    public async Task<IActionResult> Get()
    {
        Uri serviceName = VotingWeb.GetVotingDataServiceName(this.serviceContext);
        Uri proxyAddress = this.GetProxyAddress(serviceName);

        ServicePartitionList partitions = await this.fabricClient.QueryManager.GetPartitionListAsync(serviceName);

        List<KeyValuePair<string, int>> result = new List<KeyValuePair<string, int>>();

        foreach (Partition partition in partitions)
        {
            string proxyUrl =
                $"{proxyAddress}/api/VoteData?PartitionKey={((Int64RangePartitionInformation) partition.PartitionInformation).LowKey}&PartitionKind=Int64Range";

            using (HttpResponseMessage response = await this.httpClient.GetAsync(proxyUrl))
            {
                if (response.StatusCode != System.Net.HttpStatusCode.OK)
                {
                    continue;
                }

                result.AddRange(JsonConvert.DeserializeObject<List<KeyValuePair<string, int>>>(await response.Content.ReadAsStringAsync()));
            }
        }

        return this.Json(result);
    }

    // PUT: api/Votes/name
    [HttpPut("{name}")]
    public async Task<IActionResult> Put(string name)
    {
        Uri serviceName = VotingWeb.GetVotingDataServiceName(this.serviceContext);
        Uri proxyAddress = this.GetProxyAddress(serviceName);
        long partitionKey = this.GetPartitionKey(name);
        string proxyUrl = $"{proxyAddress}/api/VoteData/{name}?PartitionKey={partitionKey}&PartitionKind=Int64Range";

        StringContent putContent = new StringContent($"{{ 'name' : '{name}' }}", Encoding.UTF8, "application/json");
        putContent.Headers.ContentType = new MediaTypeHeaderValue("application/json");

        using (HttpResponseMessage response = await this.httpClient.PutAsync(proxyUrl, putContent))
        {
            return new ContentResult()
            {
                StatusCode = (int) response.StatusCode,
                Content = await response.Content.ReadAsStringAsync()
            };
        }
    }

    // DELETE: api/Votes/name
    [HttpDelete("{name}")]
    public async Task<IActionResult> Delete(string name)
    {
        Uri serviceName = VotingWeb.GetVotingDataServiceName(this.serviceContext);
        Uri proxyAddress = this.GetProxyAddress(serviceName);
        long partitionKey = this.GetPartitionKey(name);
        string proxyUrl = $"{proxyAddress}/api/VoteData/{name}?PartitionKey={partitionKey}&PartitionKind=Int64Range";

        using (HttpResponseMessage response = await this.httpClient.DeleteAsync(proxyUrl))
        {
            if (response.StatusCode != System.Net.HttpStatusCode.OK)
            {
                return this.StatusCode((int) response.StatusCode);
            }
        }

        return new OkResult();
    }


    /// <summary>
    /// Constructs a reverse proxy URL for a given service.
    /// Example: http://localhost:19081/VotingApplication/VotingData/
    /// </summary>
    /// <param name="serviceName"></param>
    /// <returns></returns>
    private Uri GetProxyAddress(Uri serviceName)
    {
        return new Uri($"http://localhost:19081{serviceName.AbsolutePath}");
    }

    /// <summary>
    /// Creates a partition key from the given name.
    /// Uses the zero-based numeric position in the alphabet of the first letter of the name (0-25).
    /// </summary>
    /// <param name="name"></param>
    /// <returns></returns>
    private long GetPartitionKey(string name)
    {
        return Char.ToUpper(name.First()) - 'A';
    }
}
```
<a id="walkthrough" name="walkthrough_anchor"></a>

## <a name="walk-through-the-voting-sample-application"></a>Oylama örnek uygulamasında izlenecek yol
Oylama uygulaması iki hizmetten oluşur:
- Web ön uç hizmeti (VotingWeb)- Web sayfasına hizmet veren ve arka uç hizmetiyle iletişim için web API'lerini kullanıma sunan bir ASP.NET Core web ön uç hizmeti.
- Arka uç hizmeti (VotingData)- Oy sonuçlarını diskte kalıcı olan güvenilir bir sözlükte depolamak için API'yi kullanıma sunan bir ASP.NET Core web hizmeti.

![Uygulama Diyagramı](./media/service-fabric-tutorial-create-dotnet-app/application-diagram.png)

Uygulamada oy kullandığınızda aşağıdaki olaylar gerçekleşir:
1. Oy isteğini bir JavaScript HTTP PUT isteği olarak web ön uç hizmetindeki web API'sine gönderir.

2. Web ön uç hizmeti bir ara sunucu kullanarak HTTP PUT isteğini bulur ve arka uç hizmetine iletir.

3. Arka uç hizmeti gelen isteği alır ve güncelleştirilmiş sonucu güvenilir bir sözlükte depolar. Bu sözlük küme içinde birden çok düğüme çoğaltılır ve diskte kalıcı olur. Uygulamanın tüm verileri kümede depolandığından, veritabanına gerek yoktur.

## <a name="debug-in-visual-studio"></a>Visual Studio'da hata ayıklama
Visual Studio'da uygulamada hata ayıklaması yaparken yerel bir Service Fabric geliştirme kümesi kullanırsınız. Hata ayıklama deneyiminizi senaryonuza göre ayarlama seçeneğiniz vardır. Bu uygulamada güvenilir sözlüğünü kullanarak arka uç hizmetinde verileri depolar. Hata ayıklayıcıyı durdurduğunuzda Visual Studio varsayılan olarak uygulamayı kaldırır. Uygulamanın kaldırılması arka uç hizmetindeki verilerin de kaldırılmasına neden olur. Hata ayıklama oturumları arasında verilerin kalıcı olmasını sağlamak için, Visual Studio'da **Oylama** projesindeki bir özellik olarak **Uygulama Hata Ayıklama Modu**'nu değiştirebilirsiniz.

Kodda neler olduğuna bakmak için aşağıdaki adımları tamamlayın:
1. Açık **VotesController.cs** dosya ve web API'ın bir kesme noktası belirleyerek **Put** yöntemi (satır 63) - Visual Studio'daki Çözüm Gezgini'nde dosyayı arayabilirsiniz.

2. Açık **VoteDataController.cs** dosya ve bu web API'nin bir kesme noktası belirleyerek **Put** yöntemi (satır 53).

3. Tarayıcıya dönün ve bir oylama seçeneğine tıklayın veya yeni oylama seçeneği ekleyin. Web ön ucunun api denetleyicisinde ilk kesme noktasına ulaşırsınız.
    
    1. Burası, tarayıcıda JavaScript'in ön uç hizmetindeki API denetleyicisine istek gönderdiği yerdir.
    
    ![Oy Ön Uç Hizmeti Ekleme](./media/service-fabric-tutorial-create-dotnet-app/addvote-frontend.png)

    2. İlk arka uç hizmeti ReverseProxy URL'si oluşturmak **(1)**.
    3. PUT HTTP isteği göndermek için ReverseProxy **(2)**.
    4. Son olarak return arka uç hizmetinden gelen yanıt istemciye **(3)**.

4. Devam etmek için **F5** tuşuna basın
    1. Şimdi arka uç hizmetindeki kesme noktasındasınız.
    
    ![Oy Arka Uç Hizmeti Ekleme](./media/service-fabric-tutorial-create-dotnet-app/addvote-backend.png)

    2. Yönteminin ilk satırında **(1)** kullanmak `StateManager` almak veya adlı güvenilir sözlüğü eklemek için `counts`.
    3. Güvenilir bir sözcükteki değerlerle tüm etkileşimler bir işlem gerektirir; bu using deyimi **(2)** o işlemi oluşturur.
    4. İşlemde oylama seçeneği için ilgili anahtarının değerini güncelleştirin ve işlemi tamamlar **(3)**. Commit yöntemi döndüğünde, sözlükteki veriler güncelleştirilir ve kümedeki diğer düğümlere çoğaltılır. Artık veriler güvenli bir şekilde kümede depolanır ve arka uç hizmeti verilerin kullanılabilir olduğu diğer düğümlere yük devretebilir.
5. Devam etmek için **F5** tuşuna basın

Hata ayıklama oturumunu durdurmak için **Shift+F5** tuşlarına basın.


## <a name="next-steps"></a>Sonraki adımlar
Öğreticinin bu bölümünde, öğrenilen nasıl yapılır:

> [!div class="checklist"]
> * Durum bilgisi olan güvenilir bir hizmet olarak bir ASP.NET çekirdek Web API hizmeti oluşturma
> * Durum bilgisiz web hizmeti olarak bir ASP.NET çekirdek Web uygulaması hizmeti oluşturma
> * Durum bilgisi olan hizmeti ile iletişim kurmak için ters proxy kullan

Sonraki öğretici ilerleyin:
> [!div class="nextstepaction"]
> [Uygulamayı Azure'a dağıtma](service-fabric-tutorial-deploy-app-to-party-cluster.md)
