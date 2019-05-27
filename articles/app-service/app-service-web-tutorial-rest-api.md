---
title: CORS - Azure App Service ile RESTful API barındırma | Microsoft Docs
description: Azure App Service’in RESTful API’lerinizi CORS desteğiyle barındırmanıza nasıl yardımcı olduğunu öğrenin.
services: app-service\api
documentationcenter: dotnet
author: cephalin
manager: cfowler
editor: ''
ms.assetid: a820e400-06af-4852-8627-12b3db4a8e70
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 11/21/2018
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: b8c1130a45f60b9caaacd365cd1c256f50ed7675
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66138581"
---
# <a name="tutorial-host-a-restful-api-with-cors-in-azure-app-service"></a>Öğretici: Azure App Service'de CORS ile RESTful API barındırma

[Azure App Service](overview.md), yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar. Buna ek olarak, App Service'de RESTful API'ler için yerleşik [Çıkış Noktaları Arası Kaynak Paylaşımı (CORS)](https://wikipedia.org/wiki/Cross-Origin_Resource_Sharing) desteği vardır. Bu öğreticide CORS desteğiyle ASP.NET Core API uygulamasının App Service'e nasıl dağıtılacağı gösterilir. Komut satırı araçlarını kullanarak uygulamayı yapılandırır ve Git kullanarak dağıtırsınız. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure CLI kullanarak App Service kaynaklarını oluşturma
> * Git kullanarak Azure'a RESTful API dağıtımı yapma
> * App Service CORS desteğini etkinleştirme

Bu öğreticideki adımları MacOS, Linux ve Windows üzerinde izleyebilirsiniz.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için:

* [Git’i yükleyin](https://git-scm.com/).
* [.NET Core'u yükleme](https://www.microsoft.com/net/core/).

## <a name="create-local-aspnet-core-app"></a>Yerel ASP.NET Core uygulaması oluşturma

Bu adımda, yerel ASP.NET Core projesini ayarlarsınız. App Service API'ler için diğer dillerde yazılmış aynı iş akışını destekler.

### <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Terminal penceresinde, `cd` ile bir çalışma dizinine gidin.  

Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. 

```bash
git clone https://github.com/Azure-Samples/dotnet-core-api
```

Bu depo şu öğreticiye temel alan oluşturulan bir uygulamayı içerir: [Swagger kullanan ASP.NET Core Web API Yardım sayfaları](/aspnet/core/tutorials/web-api-help-pages-using-swagger?tabs=visual-studio). [Swagger kullanıcı arabirimine](https://swagger.io/swagger-ui/) ve Swagger JSON uç notasına hizmet vermek için bir Swagger oluşturucusu kullanır.

### <a name="run-the-application"></a>Uygulamayı çalıştırma

Gerekli paketleri yüklemek, veritabanı geçişlerini çalıştırmak ve uygulamayı başlatmak için aşağıdaki komutları çalıştırın.

```bash
cd dotnet-core-api
dotnet restore
dotnet run
```

Swagger kullanıcı arabirimiyle çalışmak için tarayıcıda `http://localhost:5000/swagger` adresine gidin.

![Yerel olarak çalışan ASP.NET Core API'si](./media/app-service-web-tutorial-rest-api/local-run.png)

`http://localhost:5000/api/todo` adresine gidin ve ToDo JSON öğelerinin listesine bakın.

`http://localhost:5000` adresine gidin ve tarayıcı uygulamasıyla çalışın. Daha sonra CORS işlevselliğini test etmek için tarayıcı uygulamasının App Service'te bir uzak API'ye işaret etmesini sağlayacaksınız. Tarayıcı uygulamasının kodu deponun _wwwroot_ dizininde bulunabilir.

ASP.NET Core’u dilediğiniz zaman durdurmak için, terminalde `Ctrl+C` tuşlarına basın.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="deploy-app-to-azure"></a>Uygulamayı Azure’da dağıtma

Bu adımda, SQL Veritabanı’na bağlı .NET Core uygulamanızı App Service’e dağıtırsınız.

### <a name="configure-local-git-deployment"></a>Yerel git dağıtımını yapılandırma

[!INCLUDE [Configure a deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturun

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group-no-h.md)]

### <a name="create-an-app-service-plan"></a>App Service planı oluşturma

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan-no-h.md)]

### <a name="create-a-web-app"></a>Web uygulaması oluşturun

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app-dotnetcore-win-no-h.md)] 

### <a name="push-to-azure-from-git"></a>Git üzerinden Azure'a gönderme

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-git-push-to-azure-no-h.md)]

```bash
Counting objects: 98, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (92/92), done.
Writing objects: 100% (98/98), 524.98 KiB | 5.58 MiB/s, done.
Total 98 (delta 8), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: .
remote: Updating submodules.
remote: Preparing deployment for commit id '0c497633b8'.
remote: Generating deployment script.
remote: Project file path: ./DotNetCoreSqlDb.csproj
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling ASP.NET Core Web Application deployment.
remote: .
remote: .
remote: .
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
remote: App container will begin restart within 10 seconds.
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
```

### <a name="browse-to-the-azure-app"></a>Azure uygulamasına göz atma

Tarayıcıda `http://<app_name>.azurewebsites.net/swagger` adresine gidin ve Swagger kullanıcı arabirimiyle çalışın.

![Azure App Service'te çalışan ASP.NET Core API'si](./media/app-service-web-tutorial-rest-api/azure-run.png)

Dağıtılan API'nizin _swagger.json_ dosyasını görmek için `http://<app_name>.azurewebsites.net/swagger/v1/swagger.json` adresine gidin.

Dağıtılan API'nizin çalıştığını görmek için `http://<app_name>.azurewebsites.net/api/todo` adresine gidin.

## <a name="add-cors-functionality"></a>CORS işlevselliği ekleme

Ardından, API'niz için App Service'te yerleşik CORS desteğini etkinleştirirsiniz.

### <a name="test-cors-in-sample-app"></a>Örnek uygulamada CORS'yi test etme

Yerel deponuzda _wwwroot/index.html_ dosyasını açın.

51. satırda, `apiEndpoint` değişkenini dağıtılan API'nizin URL'sine (`http://<app_name>.azurewebsites.net`) ayarlayın. _\<appname>_ değerini App Service'teki uygulamanızın adıyla değiştirin.

Yerel terminal pencerenizde örnek uygulamayı yeniden çalıştırın.

```bash
dotnet run
```

`http://localhost:5000` adresindeki tarayıcı uygulamasına gidin. Tarayıcınızda geliştirici araçları penceresini açın (Windows için Chrome'da `Ctrl`+`Shift`+`i`) ve **Konsol** sekmesini inceleyin. Artık şu hata iletisini görüyor olmalısınız: `No 'Access-Control-Allow-Origin' header is present on the requested resource`.

![Tarayıcı istemcisinde CORS hatası](./media/app-service-web-tutorial-rest-api/cors-error.png)

Tarayıcı uygulaması (`http://localhost:5000`) ile uzak kaynak (`http://<app_name>.azurewebsites.net`) arasındaki etki alanı uyuşmazlığından ve App Service'deki API'nizin `Access-Control-Allow-Origin` üst bilgisi göndermemesinden dolayı, tarayıcınız etki alanları arası içeriğin tarayıcı uygulamanıza yüklenmesini engelledi.

Üretim ortamında, tarayıcı uygulamanızın localhost URL değeri yerine genel URL'si olabilir, ama localhost URL'ye CORS etkinleştirmesi genel URL ile aynıdır.

### <a name="enable-cors"></a>CORS'yi etkinleştirme 

Cloud Shell'de, [`az resource update`](/cli/azure/resource#az-resource-update) komutunu kullanarak istemcinizin URL'sinde CORS'yi etkinleştirin. _&lt;appname>_ yer tutucusunu değiştirin.

```azurecli-interactive
az resource update --name web --resource-group myResourceGroup --namespace Microsoft.Web --resource-type config --parent sites/<app_name> --set properties.cors.allowedOrigins="['http://localhost:5000']" --api-version 2015-06-01
```

`properties.cors.allowedOrigins` içinde birden çok istemci URL'si belirtebilirsiniz (`"['URL1','URL2',...]"`). Ayrıca `"['*']"` ile tüm istemci URL'lerini etkinleştirebilirsiniz.

> [!NOTE]
> Uygulamanızı tanımlama veya kimlik doğrulama belirteçlerinizi gönderilecek gibi kimlik bilgileri gerektiriyorsa, tarayıcı gerektirebilir `ACCESS-CONTROL-ALLOW-CREDENTIALS` yanıtı üstbilgisi. App Service'te etkinleştirmek için ayarlamak `properties.cors.supportCredentials` için `true` , CORS yapılandırmasını. Bu olamaz etkin `allowedOrigins` içerir `'*'`.

### <a name="test-cors-again"></a>CORS'yi yeniden test etme

`http://localhost:5000` adresindeki tarayıcı uygulamasını yenileyin. **Konsol** penceresindeki hata iletisi artık kaldırılır; dağıtılan API'den verileri görebilir ve etkileşimli çalışabilirsiniz. Uzak API'niz artık yerel olarak çalıştırılan tarayıcı uygulamanıza CORS'yi destekler. 

![Tarayıcı istemcisinde CORS başarılı](./media/app-service-web-tutorial-rest-api/cors-success.png)

Tebrikler, Azure App Service'te CORS destekli bir API çalıştırıyorsunuz.

## <a name="app-service-cors-vs-your-cors"></a>App Service CORS'si ile sizin CORS'niz

Daha fazla esneklik elde etmek için App Service CORS'si yerine kendi CORS yardımcı programlarınızı kullanabilirsiniz. Örneğin, farklı yollar veya yöntemler için izin verilen farklı çıkış noktaları belirtmek isteyebilirsiniz. App Service CORS'si tüm API yolları ve yöntemleri için tek bir izin verilen çıkış noktaları kümesi belirtmenize olanak tanıdığından, kendi CORS kodunuzu kullanmak isteyebilirsiniz. [Çıkış Noktaları Arası İstekleri Etkinleştirme (CORS)](/aspnet/core/security/cors) konusunda, ASP.NET Core'un bunu nasıl yaptığını görebilirsiniz.

> [!NOTE]
> App Service CORS'si ile kendi CORS kodunuzu birlikte kullanmaya çalışmayın. Birlikte kullanıldıklarında, App Service CORS'si öncelikli olur ve kendi CORS kodunuzun hiçbir etkisi olmaz.
>
>

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

<a name="next"></a>
## <a name="next-steps"></a>Sonraki adımlar

Öğrendikleriniz:

> [!div class="checklist"]
> * Azure CLI kullanarak App Service kaynaklarını oluşturma
> * Git kullanarak Azure'a RESTful API dağıtımı yapma
> * App Service CORS desteğini etkinleştirme

Kullanıcıların kimlik doğrulamasının ve yetkilendirmesinin nasıl yapılacağını öğrenmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Öğretici: Kimlik doğrulama ve kullanıcıları uçtan uca yetkilendirme](app-service-web-tutorial-auth-aad.md)
