---
title: "App Service’de CORS desteği | Microsoft Belgeleri"
description: "Azure App Service’de CORS desteğini kullanmayı öğrenin."
services: app-service\api
documentationcenter: .net
author: tdykstra
manager: wpickett
editor: 
ms.assetid: 4f980a97-b9f5-4d1d-87ab-82b60bb96e1c
ms.service: app-service-api
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/27/2016
ms.author: rachelap
translationtype: Human Translation
ms.sourcegitcommit: a0580f8d303c7ce33a65f0ce6faecf2492f851b0
ms.openlocfilehash: b0b701b7ea7a608f114d3a82f0403c2ae506854f


---
# <a name="consume-an-api-app-from-javascript-using-cors"></a>CORS kullanarak JavaScript’ten bir API uygulaması kullanma
App Service, JavaScript istemcilerinin API uygulamalarında barındırılan API’lere etki alanları arası çağrılar yapmasını sağlayan [Çıkış Noktaları Arası Kaynak Paylaşımı (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) için yerleşik destek sunar. App Service, API’nizde herhangi bir kod yazmak zorunda kalmadan API’nize CORS erişimini yapılandırmanıza olanak sağlar.

Bu makale iki bölüm içerir:

* [CORS yapılandırma](#corsconfig) bölümünde herhangi bir API uygulaması, web uygulaması veya mobil uygulama için CORS’nin nasıl yapılandırılacağı genel olarak açıklanmaktadır. .NET, Node.js ve Java dahil App Service tarafından desteklenen tüm çerçeveler için eşit oranda geçerlidir. 
* Makale, [.NET kullanmaya başlarken öğreticilerine devam etme](#tutorialstart) bölümünden başlayarak, [ilk API Apps kullanmaya başlarken öğreticisinde](app-service-api-dotnet-get-started.md) yaptıklarınızın üzerine ekleme yaparak CORS desteğini gösteren bir öğreticidir. 

## <a name="a-idcorsconfiga-how-to-configure-cors-in-azure-app-service"></a><a id="corsconfig"></a> Azure Uygulama Hizmeti’nde CORS’yi yapılandırma
CORS’yi Azure portalında veya [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) araçlarını kullanarak yapılandırabilirsiniz.

#### <a name="configure-cors-in-the-azure-portal"></a>CORS’yi Azure portalında yapılandırın
1. Bir tarayıcıda [Azure portalına](https://portal.azure.com/) gidin.
2. **App Services**’e ve ardından API uygulamanızın adına tıklayın.
   
    ![Portalda API uygulamasını seçin](./media/app-service-api-cors-consume-javascript/browseapiapps.png)
3. **API uygulaması** dikey penceresinin sağ tarafında açılan **Ayarlar** dikey penceresinde, **API** bölümünü bulun ve ardından **CORS**’ye tıklayın.
   
   ![Ayarlar dikey penceresinden CORS’yi seçin](./media/app-service-api-cors-consume-javascript/clicksettings.png)
4. Metin kutusunda JavaScript çağrılarının alınmasına izin vermek istediğiniz URL’yi veya URL'leri girin.

    Örneğin, JavaScript uygulamanızı todolistangular adlı bir web uygulamasına dağıttıysanız, "https://todolistangular.azurewebsites.net" girin. Alternatif olarak, tüm kaynak etki alanlarının kabul edildiğini belirtmek için bir yıldız işareti (*) girebilirsiniz.


1. **Kaydet**’e tıklayın.
   
   ![Kaydet’e tıklayın.](./media/app-service-api-cors-consume-javascript/corsinportal.png)
   
   **Kaydet**’e tıkladıktan sonra, API uygulaması belirtilen URL’lerden JavaScript çağrılarını kabul eder.

#### <a name="configure-cors-by-using-azure-resource-manager-tools"></a>Azure Resource Manager araçlarını kullanarak CORS’yi yapılandırın
CORS’yi [Azure PowerShell](/powershell/azureps-cmdlets-docs) ve [Azure CLI](../xplat-cli-install.md) gibi komut satırı araçlarında [Azure Resource Manager şablonlarını](../azure-resource-manager/resource-group-authoring-templates.md) kullanarak da yapılandırabilirsiniz. 

CORS özelliğini ayarlayan bir Azure Resource Manager şablonu örneği için, [bu öğreticinin örnek uygulamasının deposundaki azuredeploy.json dosyasını](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json) açın. Aşağıdaki gibi görünen şablon bölümünü bulun:

        "cors": {
            "allowedOrigins": [
                "todolistangular.azurewebsites.net"
            ]
        }

## <a name="a-idtutorialstarta-continuing-the-net-getting-started-tutorial"></a><a id="tutorialstart"></a> .NET’i kullanmaya başlama eğiticisinin devamı
API uygulamaları için Node.js ve Java kullanmaya başlama serisini takip ediyorsanız, kullanmaya başlama serisini tamamladınız. API Apps hakkında daha fazla bilgi edinmenizi sağlayacak öneriler için [Sonraki adımlar](#next-steps) bölümüne geçin.

Bu makalenin sonraki bölümleri .NET kullanmaya başlarken serisinin devamıdır ve [ilk öğreticiyi](app-service-api-dotnet-get-started.md) tamamladığınız varsayılır.

## <a name="deploy-the-todolistangular-project-to-a-new-web-app"></a>ToDoListAngular projesini yeni bir web uygulamasına dağıtma
[İlk öğreticide](app-service-api-dotnet-get-started.md), bir orta katman API uygulaması ve bir veri katmanı API uygulaması oluşturdunuz. Bu öğreticide, orta katman API uygulamasını çağıran bir tek sayfalı uygulama (SPA) web uygulaması oluşturacaksınız. SPA’nın çalışması için, CORS orta katman API uygulaması üzerinde etkin hale getirilmelidir. 

[ToDoList örnek uygulamasında](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list), ToDoListAngular projesi orta katman ToDoListAPI Web API projesini çağıran basit bir AngularJS istemcisidir. *app/scripts/todoListSvc.js* dosyasındaki JavaScript kodu, AngularJS HTTP sağlayıcısını kullanarak API’yi çağırır. 

        angular.module('todoApp')
        .factory('todoListSvc', ['$http', function ($http) {

            $http.defaults.useXDomain = true;
            delete $http.defaults.headers.common['X-Requested-With']; 

            return {
                getItems : function(){
                    return $http.get(apiEndpoint + '/api/TodoList');
                },

                /* Get by ID, Put, and Delete methods not shown */

                postItem : function(item){
                    return $http.post(apiEndpoint + '/api/TodoList', item);
                }
            };
        }]);

### <a name="create-a-new-web-app-for-the-todolistangular-project"></a>ToDoListAngular projesi için yeni bir web uygulaması oluşturma
Yeni bir App Service web uygulaması oluşturma ve buna bir proje dağıtma yordamı, [serideki ilk öğreticide bir API uygulaması oluşturma ve dağıtma için](app-service-api-dotnet-get-started.md#createapiapp) yer alan yordama benzer. Tek farkı uygulama türünün **API Uygulaması** yerine **Web Uygulaması** olmasıdır.  İletişim kutularının ekran görüntüleri için bkz. 

1. **Çözüm Gezgini**’nde ToDoListAngular projesine sağ tıklayın ve ardından **Yayımla**’ya tıklayın.
2. **Web’de Yayımla** sihirbazının **Profil** sekmesinde, **Microsoft Azure App Service**’ne tıklayın.
3. **App Service** iletişim kutusunda **Yeni**’ye tıklayın.
4. **App Service Oluştur** iletişim kutusunun **Barındırma** sekmesinde, *azurewebsites.net* etki alanında benzersiz bir **Web Uygulaması Adı** girin. 
5. Çalışmak istediğiniz Azure **Aboneliğini** seçin.
6. **Kaynak Grubu** açılan listesinde, önceden oluşturduğunuz kaynak grubunu seçin.
7. **App Service Planı** açılır listesinde, önceden oluşturduğunuz planı seçin. 
8. **Oluştur**’a tıklayın.
   
    Visual Studio web uygulamasını oluşturur, bunun için bir yayımlama profili oluşturur ve **Web’i Yayımla** sihirbazının **Bağlantı** adımını görüntüler.
   
    Henüz **Yayımla**’ya tıklamayın. Aşağıdaki bölümde, App Service’de çalışan orta katman API uygulamasını çağırmak için yeni web uygulamasını yapılandıracaksınız. 

### <a name="set-the-middle-tier-url-in-web-app-settings"></a>Web uygulaması ayarları içinde orta katman URL'yi ayarlayın
1. [Azure portalına](https://portal.azure.com/) gidin ve ardından TodoListAngular (ön uç) projesini barındırmak için oluşturduğunuz **Web Uygulaması** dikey penceresine gidin.
2. **Ayarlar > Uygulama Ayarları**’na tıklayın.
3. **Uygulama Ayarları** bölümünde, aşağıdaki anahtar ve değeri ekleyin:
   
   | Anahtar | Değer | Örnek |
   | --- | --- | --- |
   | toDoListAPIURL |https://{orta katman API uygulamanızın adı}.azurewebsites.net |https://todolistapi0121.azurewebsites.net |
4. **Kaydet** düğmesine tıklayın.
   
    Kod Azure içinde çalıştığında, bu değer *Web.config* dosyasındaki localhost URL’sini geçersiz kılar. 
   
    Ayar değerini alan kod *index.cshtml* içindedir:
   
        <script type="text/javascript">
            var apiEndpoint = "@System.Configuration.ConfigurationManager.AppSettings["toDoListAPIURL"]";
        </script>
        <script src="app/scripts/todoListSvc.js"></script>
   
    *todoListSvc.js* içindeki kod şu ayarı kullanır:
   
        return {
            getItems : function(){
                return $http.get(apiEndpoint + '/api/TodoList');
            },
            getItem : function(id){
                return $http.get(apiEndpoint + '/api/TodoList/' + id);
            },
            postItem : function(item){
                return $http.post(apiEndpoint + '/api/TodoList', item);
            },
            putItem : function(item){
                return $http.put(apiEndpoint + '/api/TodoList/', item);
            },
            deleteItem : function(id){
                return $http({
                    method: 'DELETE',
                    url: apiEndpoint + '/api/TodoList/' + id
                });
            }
        };

### <a name="deploy-the-todolistangular-web-project-to-the-new-web-app"></a>ToDoListAngular web projesini yeni web uygulamasına dağıtma
* Visual Studio’da, **Web’i Yayımla** sihirbazının **Bağlantı** adımında, **Yayımla**’ya tıklayın.
  
   Visual Studio, ToDoListAngular projesini yeni web uygulamasına dağıtır ve web uygulaması URL'sini bir tarayıcı penceresinde açar. 

### <a name="test-the-application-without-cors-enabled"></a>CORS’yi etkinleştirmeden uygulamayı test etme
1. Tarayıcınızda Geliştirici Araçları’nda Konsol penceresini açın.
2. AngularJS kullanıcı arabirimini gösteren tarayıcı penceresinde **Yapılacaklar Listesi** bağlantısına tıklayın.
   
    JavaScript kodu orta katman API uygulamasını çağırmayı dener ancak ön uç arka uçtan farklı bir etki alanında çalıştığından başarısız olur. Tarayıcının Geliştirici Araçları Konsol penceresi bir çıkış noktaları arası hata iletisi gösterir.
   
    ![Çıkış noktaları arası hata iletisi](./media/app-service-api-cors-consume-javascript/consoleaccessdenied.png)

## <a name="configure-cors-for-the-middle-tier-api-app"></a>CORS’yi orta katman API uygulaması için yapılandırma
Bu bölümde, Azure’da orta katman ToDoListAPI API uygulaması için CORS ayarını yapılandıracaksınız. Bu ayar, orta katman API uygulamasının ToDoListAngular projesi için oluşturduğunuz web uygulamasından JavaScript çağrılarını almasına olanak sağlar.

1. Bir tarayıcıda [Azure portalına](https://portal.azure.com/) gidin.
2. **App Services**’e ve ardından ToDoListAPI (orta katman) API uygulamasına tıklayın.
   
    ![Portalda API uygulamasını seçin](./media/app-service-api-cors-consume-javascript/browseapiapps.png)
3. **API uygulaması** dikey penceresinin sağ tarafında açılan **Ayarlar** dikey penceresinde, **API** bölümünü bulun ve ardından **CORS**’ye tıklayın.
   
   ![Portalda CORS’yi seçme](./media/app-service-api-cors-consume-javascript/clicksettings.png)
4. Metin kutusunda, ToDoListAngular (ön uç) web uygulamasının URL'sini girin. Örneğin, ToDoListAngular projesini todolistangular0121 adlı bir web uygulamasına dağıttıysanız, `https://todolistangular0121.azurewebsites.net` URL’sinden gelen çağrılara izin verin.
   
   Alternatif olarak, tüm kaynak etki alanlarının kabul edildiğini belirtmek için bir yıldız işareti (*) girebilirsiniz.
5. **Kaydet**’e tıklayın.
   
   ![Kaydet’e tıklayın.](./media/app-service-api-cors-consume-javascript/corsinportal.png)
   
   **Kaydet**’e tıkladıktan sonra, API uygulaması belirtilen URL’den JavaScript çağrılarını kabul eder. Bu ekran görüntüsünde, ToDoListAPI0223 API uygulaması ToDoListAngular web uygulamasından gelen JavaScript istemci çağrılarını kabul eder.

### <a name="test-the-application-with-cors-enabled"></a>CORS’yi etkinleştirerek uygulamayı test etme
* Web uygulamasının HTTPS URL'sini bir tarayıcıda açın. 
  
    Bu sefer uygulama yapılacaklar öğelerini görüntülemenize, eklemenize, düzenlemenize ve silmenize olanak sağlar. 
  
    ![Örnek uygulamanın Yapılacaklar Listesi sayfası](./media/app-service-api-cors-consume-javascript/corssuccess.png)

## <a name="app-service-cors-versus-web-api-cors"></a>App Service CORS ile Web API CORS karşılaştırması
Bir Web API projesinde, API’nizin hangi etki alanlarından JavaScript çağrılarını kabul edeceğini kodla belirtmek için [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet paketini yükleyebilirsiniz.

Web API CORS desteği App Service CORS desteğinden daha esnektir. Örneğin, kodda farklı eylem yöntemleri için farklı kabul edilen çıkış noktaları belirtebilirken, App Service CORS’de bir API uygulamasının tüm yöntemleri için bir kabul edilen çıkış noktası kümesi belirtirsiniz.

> [!NOTE]
> Web API CORS ve App Service CORS’yi aynı API uygulamasında kullanmayı denemeyin. App Service CORS öncelik kazanır ve Web API CORS’nin hiçbir etkisi olmaz. Örneğin, App Service içinde bir çıkış noktası etki alanını etkinleştirir ve Web API kodunuzdaki tüm çıkış noktası etki alanlarını etkinleştirirseniz, Azure API uygulamanız yalnızca Azure içinde belirtilen etki alanı çağrılarını kabul eder.
> 
> 

### <a name="how-to-enable-cors-in-web-api-code"></a>Web API kodunda CORS’yi etkinleştirme
Aşağıdaki adımlarda Web API CORS desteğini etkinleştirme işlemi özetlenir. Daha fazla bilgi için bkz. [ASP.NET Web API 2’de Çıkış Noktaları Arası İstekleri Etkinleştirme](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api).

1. Bir Web API projesinde, [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet paketini yükleyin.
2. **WebApiConfig** sınıfının **Register** yöntemine aşağıdaki örnekteki gibi bir `config.EnableCors()` kod satırı ekleyin. 
   
        public static class WebApiConfig
        {
            public static void Register(HttpConfiguration config)
            {
                // Web API configuration and services
   
                // The following line enables you to control CORS by using Web API code
                config.EnableCors();
   
                // Web API routes
                config.MapHttpAttributeRoutes();
   
                config.Routes.MapHttpRoute(
                    name: "DefaultApi",
                    routeTemplate: "api/{controller}/{id}",
                    defaults: new { id = RouteParameter.Optional }
                );
            }
        }
3. Web API denetleyicinizde, `System.Web.Http.Cors` ad alanı için bir `using` bildirimi ekleyin ve denetleyici sınıfına veya tek tek eylem yöntemlerine `EnableCors` özniteliğini ekleyin. Aşağıdaki örnekte, CORS desteği tüm denetleyiciye uygulanır.
   
        namespace ToDoListAPI.Controllers 
        {
            [HttpOperationExceptionFilterAttribute]
            [EnableCors(origins:"https://todolistangular0121.azurewebsites.net", headers:"accept,content-type,origin,x-my-header", methods: "get,post")]
            public class ToDoListController : ApiController

## <a name="using-azure-api-management-with-api-apps"></a>API Apps ile Azure API Management kullanma
Bir API uygulamasıyla Azure API Management kullanıyorsanız, CORS’yi API uygulamasında değil API Management içinde yapılandırın. Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure API Management’a Genel Bakış (video: CORS 12:10’da başlar)](https://azure.microsoft.com/documentation/videos/azure-api-management-overview/)
* [API Management etki alanları arası ilkeler](https://msdn.microsoft.com/library/azure/dn894084.aspx#CORS)

## <a name="troubleshooting"></a>Sorun giderme
Bu öğreticiyi izlerken bir sorunla karşılaşırsanız, bazı sorun giderme fikirlerini burada bulabilirsiniz.

* [Visual Studio 2015 için .NET için Azure SDK](http://go.microsoft.com/fwlink/?linkid=518003)’nin en yeni sürümünü kullandığınızdan emin olun.
* CORS ayarında `https` değerini girdiğinizden ve ön uç web uygulamasını çalıştırmak için `https` kullandığınızdan emin olun.
* CORS ayarını ön uç web uygulamasında değil orta katman API uygulamasında girdiğinizden emin olun.
* CORS’yi hem uygulama kodunda hem de Azure App Service içinde yapılandırıyorsanız, App Service CORS ayarının uygulama kodunda yaptığınız ayarları geçersiz kılacağına dikkat edin. 

Sorun gidermeyi basitleştiren Visual Studio özellikleri hakkında daha fazla bilgi için bkz. [Visual Studio’da Azure App Service uygulamalarıyla ilgili sorunları giderme](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, istemci JavaScript kodunun farklı bir etki alanındaki bir API’yi çağırması için App Service CORS desteğini nasıl etkinleştireceğinizi öğrendiniz. API uygulamaları hakkında daha fazla bilgi için, [App Service’de kimlik doğrulamasına giriş](../app-service/app-service-authentication-overview.md) bölümünü okuyun ve ardından [API uygulamaları için kullanıcı kimlik doğrulamaları](app-service-api-dotnet-user-principal-auth.md) öğreticisine gidin.




<!--HONumber=Dec16_HO3-->


