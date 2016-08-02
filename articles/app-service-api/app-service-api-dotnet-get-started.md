<properties
    pageTitle="App Service’de API Apps’i ve ASP.NET’i kullanmaya başlama | Microsoft Azure"
    description="Azure App Service’de bir ASP.NET API uygulamasını Visual Studio 2015’i kullanarak oluşturmayı, dağıtmayı ve kullanmayı öğrenin."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="dotnet"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="05/27/2016"
    ms.author="tdykstra"/>

# Azure App Service’de API Apps, ASP.NET ve Swagger kullanmaya başlama

[AZURE.INCLUDE [selector](../../includes/app-service-api-get-started-selector.md)]

Bu, RESTful API’lerini geliştirme ve barındırma için yardımcı olara Azure App Service özelliklerini kullanmayı gösteren öğretici serisinde ilktir.  Bu öğretici, Swagger biçiminde API meta veri desteğini ele alınmaktadır.

Şunları öğreneceksiniz:

* Azure App Service’de Visual Studio 2015 yerleşik araçlarını kullanarak [API Uygulamaları](app-service-api-apps-why-best-platform.md) oluşturma ve dağıtma.
* Dinamik olarak Swagger API’si meta verileri oluşturmak için Swashbuckle NuGet paketini kullanmak üzere API keşfetmeyi otomatik hale getirmeyi öğrenin.
* Bir API uygulaması için otomatik olarak istemci kodu oluşturmak üzere Swagger API’si meta verilerini otomatik olarak kullanma.

## Örnek uygulamaya genel bakış

Bu öğreticide, bir basit bir yapılacaklar listesi örnek uygulaması ile çalışırsınız. Uygulama, tek sayfalı uygulama (SPA) ön ucuna, ASP.NET Web API’si orta katmanına ve ASP.NET Web API’si veri katmanına sahip.

![API Apps örnek uygulama diyagramı](./media/app-service-api-dotnet-get-started/noauthdiagram.png)

Burada [AngularJS](https://angularjs.org/) ön ucu ekran görüntüsü yer almaktadır.

![API Apps örnek uygulama yapılacaklar listesi](./media/app-service-api-dotnet-get-started/todospa.png)

Visual Studio çözümü üç projeyi içerir:

![](./media/app-service-api-dotnet-get-started/projectsinse.png)

* **ToDoListAngular** -Ön uç: Orta katmanı çağıran bir AngularJS SPA’sı. 

* **ToDoListAPI** -Orta katman: Yapılacak işler öğelerinde CRUD işlemleri gerçekleştirmek üzere veri katmanını çağıran bir ASP.NET Web API projesi.

* **ToDoListDataAPI** -Orta katman: Yapılacak işler öğelerinde CRUD işlemleri gerçekleştiren bir ASP.NET Web API projesi. 

Üç katmanlı yapı, API Apps kullanarak uygulayabileceğiniz birçok yapıdan biridir ve burada yalnızca gösterim amacıyla kullanılmıştır. Her katmandaki kod API Apps özelliklerini göstermek üzere mümkün olduğu kadar basittir: örneğin, veri katmanı kalıcılık mekanizması olarak veritabanı yerine sunucu belleği kullanır.

Bu öğreticiyi tamamladığınızda, App Service API uygulamalarında bulutta çalışmaya hazır iki adet Web API projesine sahip olacaksınız.

Serideki sonraki öğretici SPA ön ucunu buluta dağıtır.

## Ön koşullar

* ASP.NET Web API - Öğretici yönergeleri, Visual Studio’da ASP.NET [Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) ile çalışmaya ilişkin temel bilgiye sahip olduğunuzu varsayar.

* Azure hesabı - [Ücretsiz bir Azure hesabı açabilir](/pricing/free-trial/?WT.mc_id=A261C142F) veya [Visual Studio abonelik avantajlarını etkinleştirebilirsiniz](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

    Azure hesabı için kaydolmadan önce Azure App Service’i kullanmaya başlamak istiyorsanız [App Service’i Deneyin](http://go.microsoft.com/fwlink/?LinkId=523751)’e gidin. Burada, App Services’de hemen bir kısa süreli başlangıç uygulaması oluşturabilirsiniz; kredi kartı gerekmez ve hiçbir taahhüt yoktur.

* [.NET için Azure SDK](http://go.microsoft.com/fwlink/?linkid=518003) ile Visual Studio 2015 - Halihazırda yoksa SDK, Visual Studio 2015’i otomatik olarak yükler.

    >[AZURE.NOTE] Makinenizde zaten bulunan SDK bağımlılığı sayısına bağlı olarak, SDK’nin yüklenmesi birkaç dakikadan yarım saate uzanacak şekilde biraz uzun sürebilir.

## Örnek uygulamayı indirin: 

1. [Azure-Samples/app-service-api-dotnet-to-do-list](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) deposunu indirin.

    **ZIP’i İndir** düğmesine tıklayabilir ya da depoyu yerel makinenize kopyalayabilirsiniz. 

2. Visual Studio 2015 ya da 2013’te ToDoList çözümünü açın.

2. NuGet paketlerini geri yüklemek için çözümü oluşturun.

    Dağıtmadan önce uygulamanın çalışmasını görmek istiyorsanız, yerel olarak çalıştırabilirsiniz. Üç projenin de başlangıç projeleri olduğundan emin olmanız yeterlidir. Bu tarayıcılar `http://localhost` URL’lerine çıkış noktaları arası JavaScript çağrılarına izin verdiğinden, Internet Explorer ya da Edge kullanmalısınız. 

## Swagger API meta verileri ve kullanıcı arabirimi kullanma

[Swagger](http://swagger.io/) 2.0 API meta verileri desteği Azure App Service’de yerleşiktir. Her API, API meta verilerini Swagger JSON biçiminde döndüren bir URL uç noktası belirtebilir. Bu uç noktadan döndürülen meta veriler istemci kodu oluşturmak için kullanılabilir. 

Bir ASP.NET Web API projesi [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) NuGet paketi kullanarak dinamik olarak Swagger meta verileri oluşturabilir. Swashbuckle NuGet paketi indirdiğiniz ToDoListDataAPI ve ToDoListAPI projelerinde zaten yüklüdür.

Öğreticinin bu bölümünde, Swagger 2.0 Swagger meta verileri oluşturmayı inceler ve ardından Swagger meta verilerini temel alan bir kullanıcı arabirimini denersiniz.

2. Başlangıç projesi olarak ToDoListDataAPI projesi (ToDoListAPI projesi **değil**) ayarlayın. 
 
4. Projeyi hata ayıklama modunda çalıştırmak için ,F5 tuşuna basın veya **Hata Ayıklama > Hata Ayıklamayı Başlat**’a tıklayın.

    Tarayıcı açılır ve HTTP 403 hata sayfası görüntülenir.

12. Tarayıcınızın adres çubuğuna, satırın sonuna `swagger/docs/v1` ekleyin ve Return tuşuna basın. (URL `http://localhost:45914/swagger/docs/v1` şeklindedir.)

    Bu API için Swagger 2.0 JSON meta verileri döndürmek üzere Swashbuckle tarafından kullanılan varsayılan URL'dir. 

    Internet Explorer kullanıyorsanız, tarayıcı bir *v1.json* dosyası yüklemenizi ister.

    ![IE’de JSON meta veriler indirme](./media/app-service-api-dotnet-get-started/iev1json.png)

    Chrome, Firefox veya Edge kullanıyorsanız, tarayıcıyı JSON dosyasını tarayıcı penceresinde görüntüler. Farklı tarayıcılar JSON dosyasını farklı şekilde işler ve tarayıcı pencereniz tam olarak örnekteki gibi görünmeyebilir.

    ![Chrome’da JSON meta verileri](./media/app-service-api-dotnet-get-started/chromev1json.png)

    Aşağıdaki örnek, Get yöntemi tanımıyla, API için Swagger meta verilerinin ilk bölümü gösterir. Bu meta veriler aşağıdaki adımlarda kullandığınız Swagger kullanıcı arabirimini yönlendirir ve bunu öğreticinin sonraki bölümünde otomatik olarak istemci kodu oluşturmak üzere kullanırsınız.

        {
          "swagger": "2.0",
          "info": {
            "version": "v1",
            "title": "ToDoListDataAPI"
          },
          "host": "localhost:45914",
          "schemes": [ "http" ],
          "paths": {
            "/api/ToDoList": {
              "get": {
                "tags": [ "ToDoList" ],
                "operationId": "ToDoList_GetByOwner",
                "consumes": [ ],
                "produces": [ "application/json", "text/json", "application/xml", "text/xml" ],
                "parameters": [
                  {
                    "name": "owner",
                    "in": "query",
                    "required": true,
                    "type": "string"
                  }
                ],
                "responses": {
                  "200": {
                    "description": "OK",
                    "schema": {
                      "type": "array",
                      "items": { "$ref": "#/definitions/ToDoItem" }
                    }
                  }
                },
                "deprecated": false
              },

1. Tarayıcıyı kapatın ve Visual Studio hata ayıklamayı durdurun.

3. **Çözüm Gezgini**’ndeki ToDoListDataAPI projesinde, açık *App_Start\SwaggerConfig.cs* dosyasını açın ve ardından aşağıdaki koda kaydırın ve açıklamasını kaldırın.

        /*
            })
        .EnableSwaggerUi(c =>
            {
        */

    Projede Swashbuckle paketini yüklediğinizde *SwaggerConfig.cs* dosyası oluşturulur. Dosya Swashbuckle yapılandırmak için çeşitli yöntemler sağlar.

    Açıklamasını kaldırdığını kod aşağıdaki adımlarda kullandığınız Swagger kullanıcı arabirimini etkinleştirir. API uygulaması proje şablonunu kullanarak bir Web API projesi oluşturduğunuzda, bu kod varsayılan bir güvenlik önlemi olarak eklenir.

5. Projeyi tekrar çalıştırın.

3. Tarayıcınızın adres çubuğuna, satırın sonuna `swagger` ekleyin ve Return tuşuna basın. (URL `http://localhost:45914/swagger` şeklindedir.)

4. Swagger kullanıcı arabirimi sayfası görüntülendiğinde, kullanılabilecek yöntemleri görmek için**ToDoList**’e tıklayın.

    ![Swagger kullanıcı arabirimi kullanılabilir yöntemleri](./media/app-service-api-dotnet-get-started/methods.png)

5. Listede ilk **Al** düğmesine tıklayın.

6. **Parametreler** bölümünde, `owner` parametresi değerini bir yıldız işareti olarak girin ve ardından **Deneyin**’e tıklayın.

    Sonraki öğreticilerde kimlik doğrulama eklediğinizde, orta katman veri katmanına asıl kullanıcı kimliğini sağlar. Şimdilik, tüm görevler kendi kimlikleri olarak yıldız işaretine sahip olurken uygulama kimlik doğrulama etkin olmadan çalışır.

    ![Swagger kullanıcı arabirimini deneyin](./media/app-service-api-dotnet-get-started/gettryitout1.png)

    Swagger kullanıcı arabirimi ToDoList Alma yöntemini çağırır ve yanıt kodunu ve JSON sonuçlarını görüntüler.

    ![Swagger kullanıcı arabirimini deneyin sonuçları](./media/app-service-api-dotnet-get-started/gettryitout.png)

6. **Gönder**’e tıklayın ve ardından **Model Şeması** altındaki kutuya tıklayın.

    Model şemasına tıklamak, Gönderme yöntemi için parametre değerini belirtebileceğiniz giriş kutusunu doldurur. (Internet Explorer'da bu işe yaramazsa, farklı bir tarayıcı kullanın veya sonraki adımda parametre değerini el ile girin.)  

    ![Swagger kullanıcı arabirimini deneyin Gönder](./media/app-service-api-dotnet-get-started/post.png)

7. `todo` parametresi giriş kutusunda JSON’ı aşağıdaki örnekte görünecek şekilde değiştirin ya da kendi açıklama metninizle değiştirin:

        {
          "ID": 2,
          "Description": "buy the dog a toy",
          "Owner": "*"
        }

10. **Deneyin**’e tıklayın.

    ToDoList API’sı başarı belirten bir HTTP 204 yanıtı döndürür.

11. İlk **Al** düğmesine tıklayın ve ardından sayfanın bu bölümünde **Deneyin** düğmesine tıklayın.

    Get yöntemi yanıtı şimdi yeni yapılacak iş öğesini içerir. 

12. İsteğe bağlı: Ayrıca kimlik yöntemlerine göre Koy, Sil ve Al seçeneklerini deneyin.

14. Tarayıcıyı kapatın ve Visual Studio hata ayıklamayı durdurun.

Swashbuckle her ASP.NET Web API projesi ile çalışır. Mevcut bir projeye Swagger meta verileri oluşturma olanağı eklemek istiyorsanız, yalnızca Swashbuckle paketini yüklemeniz yeterlidir. 

**Not:** Swagger meta verileri her API işlemi için benzersiz bir kimlik içerir. Varsayılan olarak, Swashbuckle Web API denetleyicisi yöntemleriniz için yinelenen Swagger işlemi kimlikleri oluşturabilir. Bu durum, denetleyiciniz `Get()` ve `Get(id)` gibi, HTTP yöntemleriyle aşırı yüklü olduğunda gerçekleşir. Aşırı yükleri işleme hakkında daha fazla bilgi için bkz. [Swashbuckle tarafından oluşturulan API tanımlarını özelleştirme](app-service-api-dotnet-swashbuckle-customize.md). Visual Studio'da Azure API Uygulaması şablonu kullanarak bir Web API projesi oluşturursanız, benzersiz işlem kimlikleri oluşturan kod otomatik olarak *SwaggerConfig.cs* dosyasına eklenir.  

## <a id="createapiapp"></a> Azure’da API uygulaması oluşturma ve buna kod dağıtma

Bu bölümde, Azure’da yeni bir API uygulaması oluşturmak için Visual Studio **Web’i Yayımla** sihirbazına tümleştirilen Azure araçlarını kullanırsınız. Ardından, yeni API uygulamasına ToDoListDataAPI projesini dağıtır ve Swagger kullanıcı arabirimini çalıştırarak API’yi çağırırsınız.

1. **Çözüm Gezgini**’nde ToDoListDataAPI projesine sağ tıklayın ve ardından **Yayımla**’ya tıklayın.

    ![Visual Studio’ya Yayımla'ya tıklama](./media/app-service-api-dotnet-get-started/pubinmenu.png)

3.  **Web’de Yayımla** sihirbazının **Profil** adımında, **Microsoft Azure App Service**’e tıklayın.

    ![Web’de Yayımla sihirbazında Azure App Service’e tıklama](./media/app-service-api-dotnet-get-started/selectappservice.png)

4. Henüz yapmadıysanız, Azure hesabınızda oturum açın veya süresi dolduysa, kimlik bilgilerinizi yenileyin.

4. App Service iletişim kutusunda, kullanmak istediğiniz Azure **Aboneliğini** seçin ve ardından **Yeni**’ye tıklayın.

    ![App Service iletişim kutusunda Yeni’ye tıklama](./media/app-service-api-dotnet-get-started/clicknew.png)

    **App Service oluşturma** iletişim kutusunun **Barındırma** sekmesi görüntülenir.

    Swashbuckle yüklü olan bir Web API projesi dağıttığınız için, Visual Studio bir API uygulaması oluşturmak istediğinizi varsayar. Bu **API Uygulaması Adı** başlığı ve **Türü Değiştir** açılır listesinin **API Uygulaması** olarak ayarlanmasıyla belirtilir.

    ![App Service iletişim kutusunda Uygulama türü](./media/app-service-api-dotnet-get-started/apptype.png)

4. *azurewebsites.net* etki alanında benzersiz bir **API Uygulaması Adı** girin. Visual Studio’nun önerdiği varsayılan adı kabul edebilirsiniz.

    Başka birinin önceden kullandığı bir adı girerseniz, sağda kırmızı bir ünlem işareti görürsünüz.

    API uygulamasının URL’si `{APi app name}.azurewebsites.net` olacaktır.

6. **Kaynak Grubu** açılır menüsünde, **Yeni**’ye tıklayın ve ardından isterseniz "ToDoListGroup" ya da başka bir ad girin. 

    Kaynak grubu API uygulamaları, veritabanları, sanal makineler ve benzerleri gibi Azure kaynakları koleksiyonudur. Bu öğreticide, en iyi uygulama yeni bir kaynak grubu oluşturulmasıdır; böylece, öğretici için oluşturduğunuz Azure kaynaklarını tek bir adımda kolayca silebilirsiniz.

    Bu kutu mevcut [kaynak grubunu](../azure-portal/resource-group-portal.md) seçmenize ya da aboneliklerinizdeki kaynak grubunda olanlardan farklı bir ad yazarak yeni bir tane oluşturmanıza olanak tanır.

4. **App Service Planı** açılır menüsünün yanındaki **Yeni** düğmesine tıklayın.

    Ekran görüntüsü **API Uygulaması Adı**, **Abonelik** ve **Kaynak Grubu** için örnek değerleri gösterir - sizin değerlerinizi farklı olacaktır.

    ![App Service Oluştur iletişim kutusu](./media/app-service-api-dotnet-get-started/createas.png)

    Aşağıdaki adımlarda, yeni kaynak grubu için bir App Service planı oluşturursunuz. App Service planı, API uygulamanızın üzerinde çalışacağı işlem kaynaklarını belirtir. Örneğin, ücretsiz katmanı seçerseniz API uygulamanız paylaşılan sanal makineler üzerinde çalışır, bazı ücretli katmanlarda ise ayrılmış sanal makineler üzerinde çalışır. App Service planları hakkında bilgi için bkz. [App Service planlarına genel bakış](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

5. **App Service Planı Yapılandır** iletişim kutusunda, "ToDoListPlan" ifadesini veya tercih ettiğiniz başka bir adı girin.

5. **Konum** açılır listesinde, size en yakın konumu seçin.

    Bu ayar, uygulamanızın hangi Azure veri merkezinde çalıştırılacağını belirtir. [Gecikmeyi](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090) en aza indirmek için yakın bir konum seçin.

5. **Boyut** açılır menüsünde **Ücretsiz**’e tıklayın.

    Bu öğretici için ücretsiz fiyatlandırma katmanı yeterli ölçüde performans sunacaktır.

6. **App Service Planı Yapılandır** iletişim kutusunda **Tamam**’a tıklayın.

    ![App Service Planı Yapılandır iletişim kutusunda Tamam’a tıklama](./media/app-service-api-dotnet-get-started/configasp.png)

7. **App Service Oluştur** iletişim kutusunda **Oluştur**’a tıklayın.

    ![App Service Oluştur iletişim kutusunda Oluştur’a tıklama](./media/app-service-api-dotnet-get-started/clickcreate.png)

    Visual Studio API uygulamasını ve API uygulaması için gereken tüm ayarları içeren bir yayımlama profili oluşturur. Sonra, projeyi dağıtmak için kullanacağınız **Web’i Yayımla** sihirbazını açar.

    **Web’i Yayımla** sihirbazı **Bağlantı** sekmesinde (aşağıda gösterilen) açılır. 

    **Bağlantı** sekmesinde **Sunucu** ve **Site adı** ayarları API uygulamanızı işaret eder. **Kullanıcı adı** ve **Parola** Azure’un sizin için oluşturduğu dağıtım kimlik bilgileridir. Dağıtımdan sonra, Visual Studio **Hedef URL’si**nde (bu **Hedef URL**’sinin tek amacıdır) bir tarayıcı açar.  

8. **İleri**'ye tıklayın. 

    ![Web'i Yayımla sihirbazının Bağlantı sekmesinde İleri'ye tıklama](./media/app-service-api-dotnet-get-started/connnext.png)

    Sonraki sekme **Ayarlar** sekmesidir (aşağıda gösterilen). Burada, [uzaktan hata ayıklama](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug) için hata ayıklama derlemesi dağıtmak üzere derleme yapılandırma sekmesini değiştirebilirsiniz. Ayrıca, bu sekmede birkaç farklı **Dosya Yayımlama Seçeneği** bulunur:

    * Hedefteki ek dosyaları kaldır
    * Yayımlama sırasında ön derleme yap
    * App_Data klasöründeki dosyaları dışarıda bırak

    Bu öğreticide bunlardan biri gerekmez. Bunların ne yaptıklarına ilişkin ayrıntılı açıklamalar için bkz. [Nasıl yapılır: Visual Studio’da Tek Tıklamayla Yayımlamayı Kullanarak Web Projesi Dağıtma](https://msdn.microsoft.com/library/dd465337.aspx).

14. **İleri**'ye tıklayın.

    ![Web’i Yayımla sihirbazının Ayarlar sekmesi İleri’ye tıklama](./media/app-service-api-dotnet-get-started/settingsnext.png)

    Burada, İleri seçeneği hangi dosyaların projenizden API uygulamasına kopyalanacağını görme fırsatı veren **Önizleme** sekmesidir (aşağıda gösterilen). Daha önce dağıttığınız bir API uygulamasına bir proje dağıtıyorsanız, yalnızca değiştirilen dosyalar kopyalanır. Kopyalanacak dosyaların bir listesi görmek isterseniz, **Önizlemeyi Başlat** düğmesine tıklayabilirsiniz.

15. **Yayımla**’ta tıklayın.

    ![Web’i Yayımla sihirbazının Önizleme sekmesinde Yayımla’ya tıklama](./media/app-service-api-dotnet-get-started/clickpublish.png)

    Visual Studio ToDoListDataAPI projesini yeni API uygulamasına dağıtır. **Çıktı** penceresi başarılı dağıtımı günlüğe kaydeder ve API uygulamasının URL’sinde açılan bir tarayıcı penceresinde “başarıyla oluşturuldu” sayfası görüntülenir.

    ![Çıktı penceresi başarılı dağıtım](./media/app-service-api-dotnet-get-started/deploymentoutput.png)

    ![Yeni API uygulaması başarıyla oluşturuldu sayfası](./media/app-service-api-dotnet-get-started/appcreated.png)

11. Tarayıcının adres çubuğundaki URL’ye "swagger" ifadesini ekleyin ve ardından Enter tuşuna basın. (URL `http://{apiappname}.azurewebsites.net/swagger` şeklindedir.)

    Tarayıcı daha önce gördüğünüzle aynı Swagger kullanıcı arabirimini görüntüler, ancak artık bulutta çalışmaktadır. Alma yöntemini deneyin ve yeniden varsayılan 2 yapılacak işler öğelerine döndüğünüzü görün. Daha önce yaptığınız değişiklikler yerel makinede bulunan belleğe kaydedildi.

12. [Azure portalı](https://portal.azure.com/) açın.

    Azure portal, API uygulamaları gibi Azure kaynaklarını yönetmek için bir web arabirimidir.
 
14. **Gözat > App Services**’a tıklayın.

    ![App Services’a Gözatma](./media/app-service-api-dotnet-get-started/browseas.png)

15. **App Service** dikey penceresinde,yeni API uygulamanızı bulun ve tıklayın. (Azure portalda, sağda açılan pencerelere *dikey pencere* adı verilir.)

    ![App Services dikey penceresi](./media/app-service-api-dotnet-get-started/choosenewapiappinportal.png)

    İki dikey pencere açılır. Bir dikey pencerede API uygulamasına genel bakış, diğerinde görüntüleyebileceğiniz ve değiştirebileceğini ayarlayın uzun bir listesi yer alır.

16. **Ayarlar** dikey penceresinde, **API** bölümünü bulun ve **API Tanımı**’na tıklayın. 

    ![Ayarlar dikey penceresinde API Tanımı](./media/app-service-api-dotnet-get-started/apidefinsettings.png)

    **API tanımı** dikey penceresi JSON biçiminde Swagger 2.0 meta verilerine döndüren URL’yi belirtmenize olanak sağlar. Visual Studio API uygulaması oluşturduğunda, API uygulamasının temel URL’si artı `/swagger/docs/v1` olan, daha önce gördüğünüz Swashbuckle tarafından oluşturulan meta verilerinin varsayılan değerine API tanımı URL’sini ayarlar. 

    ![API tanımı URL'si](./media/app-service-api-dotnet-get-started/apidefurl.png)

    Kendisi için istemci kodu oluşturmak üzere API uygulamasını seçtiğinizde, Visual Studio meta verileri bu URL’den alır. 

## <a id="codegen"></a> Veri katmanı için istemci kodu oluşturma

Swagger’ı Azure API uygulamalarına tümleştirmenin avantajlarından biri otomatik kod oluşturmadır. Oluşturulan istemci sınıfları API uygulamasını çağıran bir kod yazmayı kolaylaştırır.

ToDoListAPI projesinin oluşturulan istemci kodu zaten vardır, ancak aşağıdaki adımlarda bunu silecek ve kod oluşturmanın nasıl yapıldığını görmek için yeniden oluşturacaksınız.

1. Visual Studio **Çözüm Gezgini**’nde, ToDoListAPI projesinde *ToDoListDataAPI* klasörünü silin. **Dikkat: Yalnızca klasörü silin, ToDoListDataAPI projesini değil.**

    ![Oluşturulan istemci kodunu silme](./media/app-service-api-dotnet-get-started/deletecodegen.png)

    Bu klasör, üzerinden geçmek üzere olduğunuz kod oluşturma işlemi kullanılarak oluşturuldu.

2. ToDoListAPI projesine sağ tıklayın ve ardından **Ekle > REST API İstemci**’ye tıklayın.

    ![Visual Studio'da REST API istemcisi ekleme](./media/app-service-api-dotnet-get-started/codegenmenu.png)

3. **REST API İstemci Ekle** iletişim kutusunda, **Swagger URL’si**’ne tıklayın ve ardından **Azure Varlığını Seç**’e tıklayın.

    ![Azure Varlığını Seç](./media/app-service-api-dotnet-get-started/codegenbrowse.png)

8. **App Service** iletişim kutusunda, bu öğretici için kullandığınız kaynak grubunu genişletin ve API uygulamanızı seçin ve ardından **Tamam**’a tıklayın.

    ![Kod oluşturma için API uygulaması seçme](./media/app-service-api-dotnet-get-started/codegenselect.png)

    **REST API İstemcisi Ekle** iletişim kutusuna döndüğünüzde, metin kutusunun portalda daha önce gördüğünüz API tanımı URL değeriyle doldurulduğuna dikkat edin. 

    ![API Tanımı URL'si](./media/app-service-api-dotnet-get-started/codegenurlplugged.png)

    >[AZURE.TIP] Kod oluşturma için meta verileri almanın alternatif bir yolu, gözat iletişim kutusunu kullanmak yerine doğrudan URL'yi girmektir. Veya Azure’a dağıtmadan önce istemci kodunu oluşturmak istiyorsanız, Web API projesini yerel olarak çalıştırabilir, Swagger JSON dosyasını sağlayan URL’ye gidebilir, dosyayı kaydedebilir ve **Mevcut bir Swagger meta veri dosyasını seç** seçeneğini kullanabilirsiniz.

9. **REST API İstemcisi Ekle** iletişim kutusunda, **Tamam**’a tıklayın.

    Visual Studio API uygulamasının adını alan bir klasör oluşturur ve istemci sınıfları oluşturur.

    ![Oluşturulan istemci için kod dosyaları](./media/app-service-api-dotnet-get-started/codegenfiles.png)

5. ToDoListAPI projesinde, oluşturulan istemciyi kullanarak API’yi çağıran kodu görmek için *Controllers\ToDoListController.cs*’yi açın. 

    Aşağıdaki kod parçası, kodun istemci nesnesini nasıl başlattığını Alma yöntemini nasıl çağırdığını gösterir.

        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));
            return client;
        }
        
        public async Task<IEnumerable<ToDoItem>> Get()
        {
            using (var client = NewDataAPIClient())
            {
                var results = await client.ToDoList.GetByOwnerAsync(owner);
                return results.Select(m => new ToDoItem
                {
                    Description = m.Description,
                    ID = (int)m.ID,
                    Owner = m.Owner
                });
            }
        }

    Oluşturucu parametresi uç nokta URL’sini `toDoListDataAPIURL` uygulama ayarından alır. Uygulamayı yerel olarak çalıştırabilmeniz için, Web.config dosyasında, değer API projesinin yerel IIS Express URL’sine ayarlanır. Oluşturucu parametresini atlarsanız, varsayılan uç nokta koddan oluşturulan URL'dir.

6. İstemci sınıfınız, API uygulaması adınıza göre farklı bir adla oluşturulacaktır; projenizde oluşturulanla tür adının eşleşmesi için, *Controllers\ToDoListController.cs*’deki kodda değişiklik yapın. Örneğin, API Uygulamanıza ToDoListDataAPI0121 adını verdiyseniz, kodu değiştirin:

        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));

şu şekilde:

        private static ToDoListDataAPI0121 NewDataAPIClient()
        {
            var client = new ToDoListDataAPI0121(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));


## Orta katmanı barındırmak için API uygulaması oluşturma

Daha önce [veri katmanı API uygulaması oluşturdunuz ve kodu buna dağıttınız](#createapiapp).  Şimdi orta katman API uygulaması için aynı yordamı izleyin. 

1. **Çözüm Gezgini**’nde, orta katman ToDoListAPI projesine (veri katmanı ToDoListDataAPI değil) sağ tıklayın ve ardından **Yayımla**’ya tıklayın.

    ![Visual Studio’ya Yayımla'ya tıklama](./media/app-service-api-dotnet-get-started/pubinmenu2.png)

3.  **Web’de Yayımla** sihirbazının **Profil** sekmesinde, **Microsoft Azure App Service**’ne tıklayın.

5. **App Service** iletişim kutusunda **Yeni**’ye tıklayın.

3. **App Service Oluştur** iletişim kutusunun **Barındırma** sekmesinde, varsayılan **API Uygulaması Adı**’nı kabul edin ya da *azurewebsites.net* etki alanında benzersiz bir ad girin. 

5. Çalışmakta olduğunuz Azure **Aboneliğini** seçin.

6. **Kaynak Grubu** açılır listesinde, önceden oluşturduğunuz kaynak grubunu seçin.

4. **App Service Planı** açılır listesinde, önceden oluşturduğunuz planı seçin. Bu, bu değeri varsayılan olarak alır. 

7. **Oluştur**’a tıklayın.

    Visual Studio API uygulamasını oluşturur, bunun için bir yayımlama profili oluşturur ve **Web’i Yayımla** sihirbazının **Bağlantı** adımını görüntüler.

3.  **Web’i Yayımla** sihirbazının **Bağlantı** adımında, **Yayımla**’ya tıklayın.

    Visual Studio, ToDoListAPI projesini yeni API uygulamasına dağıtır ve API uygulaması URL'sini bir tarayıcı penceresinde açar. "Başarıyla oluşturuldu" sayfası görüntülenir.

## Veri katmanını çağırmak için orta katmanı yapılandırma

Orta katman API uygulamasını çağırdıysanız, hala Web.config dosyasında bulunan localhost URL’sini kullanarak veri katmanını çağırmaya çalışabilir. Bu bölümde, veri katmanı API uygulaması URL’sini orta katman API uygulamasındaki bir ortam ayarına girersiniz. Orta katman API uygulamasındaki kod veri katmanı URL ayarını aldığında, ortam ayarı Web.config dosyasındakini geçersiz kılar. 
 
1. [Azure portalına](https://portal.azure.com/) gidin ve ardından TodoListAPI (orta katman) projesini barındırmak için oluşturduğunuz **API uygulaması** dikey penceresine gidin.

2. API uygulamasının **Ayarlar** dikey penceresinde, **Uygulama ayarları**’na tıklayın.
 
4. API uygulamasının **Uygulama Ayarları** dikey penceresinde, **Uygulama Ayarları** bölümüne aşağı kaydırın ve aşağıdaki anahtarı ve değeri ekleyin:

  	| **Anahtar** | toDoListDataAPIURL |
  	|---|---|
  	| **Değer** | https://{your data tier API app name}.azurewebsites.net |
  	| **Örnek** | https://todolistdataapi0121.azurewebsites.net |

4. **Kaydet**’e tıklayın.

    ![Uygulama Ayarları için Kaydet’e tıklama](./media/app-service-api-dotnet-get-started/asinportal.png)

    Kod Azure içinde çalıştığında, bu değer Web.config dosyasındaki localhost URL’sini geçersiz kılar. 

## Test etme

11. Bir tarayıcı penceresinde, ToDoListAPI için oluşturduğunuz yeni orta katman API uygulamasının URL'sine gidin. Buraya portalda API uygulamasının ana dikey penceresindeki URL’ye tıklayarak gidebilirsiniz.

13. Tarayıcının adres çubuğundaki URL’ye "swagger" ifadesini ekleyin ve ardından Enter tuşuna basın. (URL `http://{apiappname}.azurewebsites.net/swagger` şeklindedir.)

    Tarayıcı ToDoListDataAPI için daha önce gördüğünüzle aynı Swagger kullanıcı arabirimini görüntüler, ancak orta kamanı API uygulaması sizin için bu değeri veri katmanı API uygulamasına gönderdiği için Alma işlemi için `owner` şimdi gerekli bir alan değildir. (Kimlik doğrulaması öğreticilerini gerçekleştirdiğinizde, orta katman `owner` parametresi için gerçek kullanıcı kimliklerini gönderir; şimdilik bu kodlanmış bir yıldız işaretidir.)

12. Orta katman API uygulamasının veri katmanı API uygulamasını başarılı şekilde çağırdığını doğrulamak için Alma yöntemini ve diğer yöntemleri deneyin.

    ![Swagger kullanıcı arabirimi Alma yöntemi](./media/app-service-api-dotnet-get-started/midtierget.png)

## Sorun giderme

Bu öğreticiyi izlerken bir sorunla karşılaşırsanız, bazı sorun giderme fikirlerini burada bulabilirsiniz.

* [.NET için Azure SDK](http://go.microsoft.com/fwlink/?linkid=518003)’nin en yeni sürümünü kullandığınızdan emin olun.

* İki proje adı benzerdir (ToDoListAPI, ToDoListDataAPI). Projeyle çalışırken bir şeylerinde yönergelerde açıklandığı gibi görünmüyorsa, doğru projeyi açtığınızdan emin olun.

* Kurumsal bir ağda bulunuyorsanız ve Azure App Service’e bir güvenlik duvarı aracılığıyla dağıtmaya çalışıyorsanız, 443 ve 8172 numaralı bağlantı noktalarının Web Dağıtımı için açık olduklarından emin olun. Bu bağlantı noktalarını açamıyorsanız, diğer dağıtım yöntemlerini kullanabilirsiniz.  Bkz. [Uygulamanızı Azure App Service’e dağıtma](../app-service-web/web-sites-deploy.md).

* “Rota adları benzersiz olmalıdır” hataları - Yanlışlıkla bir API uygulamasına yanlış projeyi dağıttığınızda ve sonrasında doğru projeyi dağıtırsınız bu hataları alabilirsiniz. Bu sorunu gidermek için, API uygulamasına doğru projeyi yeniden dağıtın ve **Web’i Yayımla** sihirbazının **Ayarlar** sekmesinde **Hedefteki ek dosyaları kaldır**’ı seçin.

Azure App Service’de ASP.NET API uygulamanızı çalıştırdıktan sonra, sorun giderme sürecini kolaylaştıracak Visual Studio özellikleri hakkında daha fazla bilgi edinmek isteyebilirsiniz. Günlüğe kaydetme, uzaktan hata ayıklama ve çok daha fazlası hakkında bilgi edinmek için bkz. [Visual Studio’da Azure App Service uygulamalarının sorunlarını giderme](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).

## Sonraki adımlar

Mevcut Web API projelerini API uygulamalarına dağıtmayı, API uygulamaları için istemci kodu oluşturmayı ve .NET istemcilerinden alınan API uygulamalarını kullanmayı gördünüz. Bu serideki sonraki öğretici, [JavaScript istemcilerden alınan API uygulamalarını kullanmak için CORS kullanma](app-service-api-cors-consume-javascript.md)yı gösterir.
 
İstemci kodu oluşturma hakkında daha fazla bilgi için, GitHub.com’da [Azure/AutoRest](https://github.com/azure/autorest) deposuna bakın. Oluşturulan istemciyi kullanma ile ilgili sorunlarda yardım için, [AutoRest deposunda bir sorun açın](https://github.com/azure/autorest/issues).

Sıfırdan yeni API uygulaması projeleri oluşturmak istiyorsanız, **Azure API Uygulaması** şablonu kullanın.

![Visual Studio’da API Uygulaması şablonu](./media/app-service-api-dotnet-get-started/apiapptemplate.png)

**Azure API Uygulaması** proje şablonu, **Boş** ASP.NET 4.5.2 şablonu seçmeye, Web API desteği eklemek için onay kutusuna tıklamaya ve Swashbuckle NuGet paketi yüklemeye eşdeğerdir. Ayrıca, şablon yinelenen Swagger işlem kimlikleri oluşturulmasını önlemek için tasarlanmış bazı Swashbuckle yapılandırma kodları ekler. Bir API uygulaması projesi oluşturduktan sonra, bu öğreticide gördüğünüz şekilde bunu bir API uygulamasına dağıtabilirsiniz.



<!--HONumber=Jun16_HO2-->


