---
title: Kimlik doğrulama ve yetkilendirme kullanıcılar için uçtan uca Linux'ta - Azure App Service | Microsoft Docs
description: App Service kimlik doğrulaması ve yetkilendirme çalıştıran uzak API'lere erişim de dahil olmak üzere, Linux üzerinde App Service uygulamalarınızın güvenliğini sağlamak için kullanmayı öğrenin.
keywords: app service, azure app service, authN, authZ, güvenli, güvenlik, çok katmanlı, azure active directory, azure ad
services: app-service\web
documentationcenter: dotnet
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 04/26/2018
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: ed056bf28881f391ed1ba16a875259e8e420b39d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66137923"
---
# <a name="tutorial-authenticate-and-authorize-users-end-to-end-in-azure-app-service-on-linux"></a>Öğretici: Kimlik doğrulama ve kullanıcıları uçtan uca Linux üzerinde Azure App Service'te yetkilendirme

[Linux’ta App Service](app-service-linux-intro.md) Linux işletim sistemini kullanan yüksek oranda ölçeklenebilir, otomatik olarak düzeltme eki uygulayan bir web barındırma hizmeti sağlar. Ayrıca, App Service [kullanıcı kimlik doğrulaması ve yetkilendirmesi](../overview-authentication-authorization.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) için yerleşik destek sunar. Bu öğreticide, App Service kimlik doğrulaması ve yetkilendirmesi ile uygulamalarınızın nasıl güvenli hale getirileceği gösterilmektedir. Yalnızca bir örnek olarak, Angular.js ön ucu ile birlikte bir ASP.NET Core uygulaması kullanılmaktadır. App Service kimlik doğrulaması ve yetkilendirmesi tüm dil çalışma zamanlarını destekler ve öğreticiyi takip ederek tercih ettiğiniz dilde nasıl uygulanacağını öğrenebilirsiniz.

Öğretici, kendi içindeki bir uygulamanın güvenliğini sağlama işlemini göstermek için örnek uygulamayı kullanır ([Arka uç uygulaması içn kimlik doğrulaması ve yetkilendirmeyi etkinleştirme](#enable-authentication-and-authorization-for-back-end-app) içinde).

![Basit kimlik doğrulaması ve yetkilendirme](./media/tutorial-auth-aad/simple-auth.png)

Ayrıca, kimliği doğrulanmış kullanıcı adına hem [sunucu kodundan](#call-api-securely-from-server-code) hem de [tarayıcı kodundan](#call-api-securely-from-browser-code) güvenliği sağlanmış bir arka uç API’sine erişerek çok katmanlı bir uygulamanın güvenliğini sağlamayı gösterir.

![Gelişmiş kimlik doğrulaması ve yetkilendirme](./media/tutorial-auth-aad/advanced-auth.png)

Bunlar, App Service’teki olası kimlik doğrulaması ve yetkilendirme senaryolarından yalnızca bazılarıdır. 

Öğreticide bilgi edineceğiniz konuların daha kapsamlı bir listesi aşağıda verilmiştir:

> [!div class="checklist"]
> * Yerleşik kimlik doğrulaması ve yetkilendirmeyi etkinleştirme
> * Kimliği doğrulanmamış isteklere karşı uygulamaların güvenliğini sağlama
> * Azure Active Directory’yi kimlik sağlayıcısı olarak kullanma
> * Oturum açmış kullanıcının adına uzak bir uygulamaya erişme
> * Belirteç kimlik doğrulaması ile hizmetten hizmete güvenli çağrılar
> * Sunucu kodundan erişim belirteçleri kullanma
> * İstemci (tarayıcı) kodundan erişim belirteçleri kullanma

Bu öğreticideki adımları MacOS, Linux ve Windows üzerinde izleyebilirsiniz.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için:

* [Git’i yükleyin](https://git-scm.com/).
* [.NET Core 2.0’ı yükleyin](https://www.microsoft.com/net/core/).

## <a name="create-local-net-core-app"></a>Yerel .NET Core uygulaması oluşturma

Bu adımda, yerel .NET Core projesini ayarlarsınız. Bir arka uç API uygulaması ile ön uç web uygulamasını dağıtmak için aynı projeyi kullanırsınız.

### <a name="clone-and-run-the-sample-application"></a>Örnek uygulamayı kopyalama ve çalıştırma

Örnek depoyu kopyalamak ve çalıştırmak için aşağıdaki komutları çalıştırın.

```bash
git clone https://github.com/Azure-Samples/dotnet-core-api
cd dotnet-core-api
dotnet run
```

`http://localhost:5000` sayfasına gidip todo öğeleri ekleme, düzenleme ve kaldırma işlemlerini deneyin. 

![Yerel olarak çalışan ASP.NET Core API'si](./media/tutorial-auth-aad/local-run.png)

ASP.NET Core’u dilediğiniz zaman durdurmak için, terminalde `Ctrl+C` tuşlarına basın.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="deploy-apps-to-azure"></a>Uygulamaları Azure'a dağıtma

Bu adımda projeyi iki App Service uygulamasına dağıtacaksınız. Bunlardan biri ön uç uygulama, diğeri ise arka uç uygulamadır.

### <a name="create-azure-resources"></a>Azure kaynakları oluşturma

Cloud Shell'de iki App Service uygulamaları oluşturmak için aşağıdaki komutları çalıştırın. _&lt;front\_end\_app\_name>_ ve _&lt;back\_end\_app\_name>_ değerlerini genel olarak benzersiz olan iki uygulama adı ile değiştirin (geçerli karakterler `a-z`, `0-9` ve `-`). Her komut hakkında daha fazla bilgi için bkz. [Linux üzerinde App Service'te .NET Core uygulaması oluşturma](quickstart-dotnetcore.md).

```azurecli-interactive
az group create --name myAuthResourceGroup --location "West Europe"
az appservice plan create --name myAuthAppServicePlan --resource-group myAuthResourceGroup --sku B1 --is-linux
az webapp create --resource-group myAuthResourceGroup --plan myAuthAppServicePlan --name <front_end_app_name> --runtime "dotnetcore|2.0" --deployment-local-git --query deploymentLocalGitUrl
az webapp create --resource-group myAuthResourceGroup --plan myAuthAppServicePlan --name <back_end_app_name> --runtime "dotnetcore|2.0" --deployment-local-git --query deploymentLocalGitUrl
```

> [!NOTE]
> Ön uç uygulama ve arka uç uygulamanızın Git uzak öğelerine ait URL’leri, `az webapp create` çıktısında gösterilen şekilde kaydedin.
>

### <a name="configure-cors"></a>CORS Yapılandırma

Bu adım, kimlik doğrulama ve yetkilendirme ile ilgili değildir. Ancak tarayıcınızın Angular.js uygulamanızdan gelen etki alanları arası API çağrılarına izin vermesi için daha sonra [ön uç tarayıcı kodundan arka uç API’yi çağırmak](#call-api-securely-from-browser-code) için buna ihtiyacınız olacaktır. Linux üzerinde App Service, [Windows’daki karşılığı gibi](../app-service-web-tutorial-rest-api.md#add-cors-functionality) yerleşik CORS işlevselliğine sahip değildir, bu nedenle arka uç uygulaması için bunu el ile eklemeniz gerekir.

Yerel dizinde _Startup.cs_ dosyasını açın. `ConfigureServices(IServiceCollection services)` yönteminde aşağıdaki kod satırını ekleyin:

```csharp
services.AddCors();
```

`Configure(IApplicationBuilder app)` yönteminde en başa aşağıdaki kod satırını ekleyin (*\<front_end_app_name>* kısmını değiştirin):

```csharp
app.UseCors(builder =>
    builder.WithOrigins("http://<front_end_app_name>.azurewebsites.net"));
```

Yaptığınız değişiklikleri kaydedin. _Yerel terminal penceresinde_ aşağıdaki komutları çalıştırarak değişikliklerinizi Git deposuna kaydedin.

```bash
git add .
git commit -m "add CORS to back end"
```

> [!NOTE]
> Bu kodu ön uç ve arka uç uygulamaları arasında paylaşmaktan endişelenmeyin. Ön uç uygulamada herhangi bir CORS etkisi yoktur.
> 

### <a name="push-to-azure-from-git"></a>Git üzerinden Azure'a gönderme

Yerel terminal penceresinde, arka uç uygulamasına dağıtmak için aşağıdaki Git komutlarını çalıştırın. _&lt;deploymentLocalGitUrl-of-back-end-app>_ değerini, [Azure kaynakları oluşturma](#create-azure-resources) bölümünde kaydettiğiniz Git uzak öğesine ait URL ile değiştirin. Git Kimlik Bilgisi Yöneticisi kimlik bilgilerini sorduğunda, Azure portalında oturum açmak için kullandığınız kimlik bilgileri yerine [dağıtım kimlik bilgilerini](../deploy-configure-credentials.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) girdiğinizden emin olun.

```bash
git remote add backend <deploymentLocalGitUrl-of-back-end-app>
git push backend master
```

Yerel terminal penceresinde aynı kodu ön uç uygulamasına dağıtmak için aşağıdaki Git komutlarını çalıştırın. _&lt;deploymentLocalGitUrl-of-front-end-app>_ değerini, [Azure kaynakları oluşturma](#create-azure-resources) bölümünde kaydettiğiniz Git uzak öğesine ait URL ile değiştirin.

```bash
git remote add frontend <deploymentLocalGitUrl-of-front-end-app>
git push frontend master
```

### <a name="browse-to-the-azure-apps"></a>Azure uygulamalarına göz atma

Bir tarayıcıda aşağıdaki URL'lere gittiğinizde iki uygulamanın çalıştığını görebilirsiniz.

```
http://<back_end_app_name>.azurewebsites.net
http://<front_end_app_name>.azurewebsites.net
```

![Azure App Service'te çalışan ASP.NET Core API'si](./media/tutorial-auth-aad/azure-run.png)

> [!NOTE]
> Uygulamanız yeniden başlatılırsa, yeni verilerin silindiğini fark etmiş olabilirsiniz. Örnek ASP.NET Core uygulaması bir bellek içi veritabanı kullandığından bu davranış tasarım gereğidir.
>
>

## <a name="call-back-end-api-from-front-end"></a>Ön uçtan arka uç API’si çağırma

Bu adımda, ön uç uygulamanın sunucu kodunu arka uç API’sine erişecek şekilde yönlendireceksiniz. Daha sonra, ön uçtan arka uca kimliği doğrulanmış erişimi etkinleştireceksiniz.

### <a name="modify-front-end-code"></a>Ön uç kodunu değiştirme

Yerel depoda _Controllers/TodoController.cs_ dosyasını açın. `TodoController` sınıfının başına aşağıdaki satırları ekleyin ve _&lt;back\_end\_app\_name>_ değerini arka uç uygulamanızın adıyla değiştirin:

```cs
private static readonly HttpClient _client = new HttpClient();
private static readonly string _remoteUrl = "https://<back_end_app_name>.azurewebsites.net";
```

`GetAll()` yöntemini bulun ve küme ayraçlarının içindeki kodu şununla değiştirin:

```cs
var data = _client.GetStringAsync($"{_remoteUrl}/api/Todo").Result;
return JsonConvert.DeserializeObject<List<TodoItem>>(data);
```

İlk satır, arka uç API uygulamasına bir `GET /api/Todo` çağrısı yapar.

Sonra, `GetById(long id)` yöntemini bulun ve küme ayraçlarının içindeki kodu şununla değiştirin:

```cs
var data = _client.GetStringAsync($"{_remoteUrl}/api/Todo/{id}").Result;
return Content(data, "application/json");
```

İlk satır, arka uç API uygulamasına bir `GET /api/Todo/{id}` çağrısı yapar.

Sonra, `Create([FromBody] TodoItem item)` yöntemini bulun ve küme ayraçlarının içindeki kodu şununla değiştirin:

```cs
var response = _client.PostAsJsonAsync($"{_remoteUrl}/api/Todo", item).Result;
var data = response.Content.ReadAsStringAsync().Result;
return Content(data, "application/json");
```

İlk satır, arka uç API uygulamasına bir `POST /api/Todo` çağrısı yapar.

Sonra, `Update(long id, [FromBody] TodoItem item)` yöntemini bulun ve küme ayraçlarının içindeki kodu şununla değiştirin:

```cs
var res = _client.PutAsJsonAsync($"{_remoteUrl}/api/Todo/{id}", item).Result;
return new NoContentResult();
```

İlk satır, arka uç API uygulamasına bir `PUT /api/Todo/{id}` çağrısı yapar.

Sonra, `Delete(long id)` yöntemini bulun ve küme ayraçlarının içindeki kodu şununla değiştirin:

```cs
var res = _client.DeleteAsync($"{_remoteUrl}/api/Todo/{id}").Result;
return new NoContentResult();
```

İlk satır, arka uç API uygulamasına bir `DELETE /api/Todo/{id}` çağrısı yapar.

Yaptığınız tüm değişiklikleri kaydedin. Yerel terminal penceresinde, ön uç uygulamasında yaptığınız değişiklikleri aşağıdaki Git komutları ile dağıtın:

```bash
git add .
git commit -m "call back-end API"
git push frontend master
```

### <a name="check-your-changes"></a>Değişikliklerinizi kontrol edin

`http://<front_end_app_name>.azurewebsites.net` sayfasına gidip `from front end 1` ve `from front end 2` gibi birkaç öğe ekleyin.

Ön uç uygulamasından eklenen öğeleri görmek için `http://<back_end_app_name>.azurewebsites.net` sayfasına gidin. Ayrıca, `from back end 1` ve `from back end 2` gibi birkaç öğe ekleyin ve ardından ön uç uygulamasını yenileyerek değişiklikleri yansıtıp yansıtmadığını görün.

![Azure App Service'te çalışan ASP.NET Core API'si](./media/tutorial-auth-aad/remote-api-call-run.png)

## <a name="configure-auth"></a>Kimlik doğrulamasını yapılandırma

Bu adımda, iki uygulama için kimlik doğrulama ve yetkilendirmeyi etkinleştireceksiniz. Ayrıca, arka uç uygulamasına kimliği doğrulanmış çağrılar yapmak için kullanabileceğiniz bir erişim belirteci oluşturacak şekilde ön uç uygulamasını yapılandıracaksınız.

Azure Active Directory’yi kimlik sağlayıcısı olarak kullanacaksınız. Daha fazla bilgi için bkz. [Azure Active Directory kimlik doğrulamasını App Service uygulamanız için yapılandırma](../configure-authentication-provider-aad.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json).

### <a name="enable-authentication-and-authorization-for-back-end-app"></a>Arka uç uygulaması için kimlik doğrulama ve yetkilendirmeyi etkinleştirme

İçinde [Azure portalında](https://portal.azure.com), soldaki menüden tıklayarak arka uç uygulamanızın Yönetim sayfasını açın: **Kaynak grupları** > **myAuthResourceGroup** > _\<geri\_son\_uygulama\_adı >_.

![Azure App Service'te çalışan ASP.NET Core API'si](./media/tutorial-auth-aad/portal-navigate-back-end.png)

Arka uç uygulamanızın soldaki menüsünde **Kimlik doğrulaması / Yetkilendirme** öğesine tıklayın, sonra da **Açık**’a tıklayarak App Service Kimlik Doğrulamasını etkinleştirin.

**İsteğin kimliği doğrulanmamış olduğunda gerçekleştirilecek eylem** menüsünde **Azure Active Directory ile oturum aç**’ı seçin.

**Kimlik Doğrulama Sağlayıcıları** altında **Azure Active Directory**’ye tıklayın. 

![Azure App Service'te çalışan ASP.NET Core API'si](./media/tutorial-auth-aad/configure-auth-back-end.png)

**Hızlı**’ya tıklayın, ardından yeni bir AD uygulaması oluşturmak için varsayılan ayarları kabul edip **Tamam**’a tıklayın.

**Kimlik Doğrulaması / Yetkilendirme** sayfasında **Kaydet**’e tıklayın. 

`Successfully saved the Auth Settings for <back_end_app_name> App` iletisini içeren bildirimi gördüğünüzde sayfayı yenileyin.

**Azure Active Directory**’ye yeniden tıklayıp **Uygulamayı Yönet**’e tıklayın.

AD uygulamasının yönetim sayfasından **Uygulama Kimliği**’ni bir not defterine kopyalayın. Bu değer daha sonra gerekli olacaktır.

![Azure App Service'te çalışan ASP.NET Core API'si](./media/tutorial-auth-aad/get-application-id-back-end.png)

### <a name="enable-authentication-and-authorization-for-front-end-app"></a>Ön uç uygulaması için kimlik doğrulama ve yetkilendirmeyi etkinleştirme

Ön uç uygulaması için de aynı adımları izleyin, ancak son adımı atlayın. Ön uç uygulaması için **Uygulama Kimliği** gerekli değildir. **Azure Active Directory Ayarları** sayfasını açık tutun.

İsterseniz `http://<front_end_app_name>.azurewebsites.net` sayfasına gidin. Şimdi oturum açma sayfasına yönlendirilmeniz gerekir. Oturum açtıktan sonra hala yapmanız gereken üç işlem olduğu için, arka uç uygulamasından verilere erişemezsiniz:

- Arka uca ön uç erişimi verme
- App Service’i kullanılabilir bir belirteç döndürecek şekilde yapılandırma
- Belirteci kodunuzda kullanma

> [!TIP]
> Hatalarla karşılaşırsanız ve uygulamanızın kimlik doğrulaması/yetkilendirme ayarlarını yeniden yapılandırırsanız, belirteç deposundaki belirteçler yeni ayarlarla yeniden oluşturulamayabilir. Belirteçlerinizin yeniden oluşturulduğundan emin olmak için oturumu kapatıp uygulamanızda yeniden oturum açmanız gerekir. Bunu yapmanın kolay bir yolu, tarayıcınızı özel modda kullanmak ve uygulamanızdaki ayarları değiştirdikten sonra tarayıcıyı kapatıp özel modda yeniden açmaktır.

### <a name="grant-front-end-app-access-to-back-end"></a>Arka uca ön uç uygulaması erişimi verme

Her iki uygulamanızda da kimlik doğrulaması ve yetkilendirmeyi etkinleştirdikten sonra uygulamaların her biri bir AD uygulaması tarafından desteklenir. Bu adımda, ön uç uygulamasına kullanıcı adına arka uca erişme izni vereceksiniz. (Teknik açıdan, ön ucun _AD uygulamasına_ arka ucun _AD uygulaması_ için erişim izinleri vereceksiniz.)

Bu noktada, ön uç uygulamasının **Azure Active Directory Ayarları** sayfasında olmanız gerekir. Değilseniz, o sayfaya geri dönün. 

**İzinleri Yönet** > **Ekle** > **API Seç** öğesine tıklayın.

![Azure App Service'te çalışan ASP.NET Core API'si](./media/tutorial-auth-aad/add-api-access-front-end.png)

**API Seç** sayfasında, arka uç uygulamanızın varsayılan olarak arka uç uygulaması adıyla aynı olan AD uygulama adını girin. Listeden seçim yapıp **Seç**’e tıklayın.

**Erişim _&lt;AD\_application\_name>_** seçeneğinin yanındaki onay kutusunu işaretleyin. **Seç** > **Bitti**’ye tıklayın.

![Azure App Service'te çalışan ASP.NET Core API'si](./media/tutorial-auth-aad/select-permission-front-end.png)

### <a name="configure-app-service-to-return-a-usable-access-token"></a>App Service’i kullanılabilir bir erişim belirteci döndürecek şekilde yapılandırma

Ön uç uygulama artık gerekli izinlere sahiptir. Bu adımda, App Service kimlik doğrulaması ve yetkilendirmesini, arka uca erişmeniz için kullanılabilir bir erişim belirteci verecek şekilde yapılandıracaksınız. Bu adım için, [Arka uç uygulaması için kimlik doğrulama ve yetkilendirmeyi etkinleştirme](#enable-authentication-and-authorization-for-back-end-app) bölümünden kopyaladığınız arka ucun Uygulama Kimliği gereklidir.

[Azure Kaynak Gezgini](https://resources.azure.com)’nde oturum açın. Sayfanın üst kısmındaki **Oku/Yaz**’a tıklayarak Azure kaynaklarınızın düzenlenmesini etkinleştirin.

![Azure App Service'te çalışan ASP.NET Core API'si](./media/tutorial-auth-aad/resources-enable-write.png)

Sol tarayıcıda **abonelikler** > **_&lt;sizin\_aboneliğiniz>_** > **resourceGroups** > **myAuthResourceGroup** > **sağlayıcılar** > **Microsoft.Web** > **siteler** > **_\<ön\_uç\_uygulama\_adı>_** > **config** > **authsettings** öğesine tıklayın.

**authsettings** görünümünde **Düzenle**’ye tıklayın. Kopyaladığınız Uygulama Kimliğini kullanarak aşağıdaki JSON dizesini `additionalLoginParams` olarak ayarlayın. 

```json
"additionalLoginParams": ["response_type=code id_token","resource=<back_end_application_id>"],
```

![Azure App Service'te çalışan ASP.NET Core API'si](./media/tutorial-auth-aad/additional-login-params-front-end.png)

**PUT** öğesine tıklayarak ayarlarınızı kaydedin.

Uygulamalarınız artık yapılandırılmıştır. Ön uç artık doğru bir erişim belirteci ile arka uca erişmeye hazırdır.

Bunu diğer sağlayıcılar için yapılandırma hakkında daha fazla bilgi için bkz. [Erişim belirteçlerini yenileme](../app-service-authentication-how-to.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#refresh-identity-provider-tokens).

## <a name="call-api-securely-from-server-code"></a>Sunucu kodundan güvenli bir şekilde API çağırma

Bu adımda, daha önce değiştirdiğiniz sunucu kodunu etkinleştirerek arka uç API’sine kimliği doğrulanmış çağrılar yapacaksınız.

Ön uç uygulamanız artık gerekli izne sahiptir ve aynı zamanda arka ucun Uygulama Kimliği oturum açma parametrelerine ekler. Bu nedenle, arka uç uygulaması ile kimlik doğrulaması için bir erişim belirteci elde edebilirsiniz. App Service, kimliği doğrulanmış her bir isteğe bir `X-MS-TOKEN-AAD-ACCESS-TOKEN` üst bilgisi ekleyerek sunucu kodunuza bu belirteci sağlar (bkz. [Uygulama kodunda belirteçleri alma](../app-service-authentication-how-to.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#retrieve-tokens-in-app-code)).

> [!NOTE]
> Bu üst bilgiler desteklenen tüm diller için eklenir. Üst bilgilere ilgili her dil için standart deseni kullanarak erişebilirsiniz.

Yerel depoda _Controllers/TodoController.cs_ dosyasını yeniden açın. `TodoController(TodoContext context)` oluşturucusu altında aşağıdaki kodu ekleyin:

```cs
public override void OnActionExecuting(ActionExecutingContext context)
{
    base.OnActionExecuting(context);

    _client.DefaultRequestHeaders.Accept.Clear();
    _client.DefaultRequestHeaders.Authorization =
        new AuthenticationHeaderValue("Bearer", Request.Headers["x-ms-token-aad-access_token"]);
}
```

Bu kod, standart `Authorization: Bearer <access_token>` HTTP üst bilgisini tüm uzak API çağrılarına ekler. ASP.NET Core MVC istek yürütme işlem hattında `OnActionExecuting`, ilgili eylem yönteminden (`GetAll()` gibi) hemen önce yürütülür; bu nedenle giden API çağrınız artık erişim belirtecini sunar.

Yaptığınız tüm değişiklikleri kaydedin. Yerel terminal penceresinde, ön uç uygulamasında yaptığınız değişiklikleri aşağıdaki Git komutları ile dağıtın:

```bash
git add .
git commit -m "add authorization header for server code"
git push frontend master
```

`http://<front_end_app_name>.azurewebsites.net` oturumunu yeniden açın. Kullanıcı veri kullanımı sözleşmesi sayfasında **Kabul Et**’e tıklayın.

Artık daha önce olduğu gibi arka uç uygulamanızdan verileri oluşturabilir, okuyabilir, güncelleştirebilir ve silebilirsiniz. Şimdiki tek fark, her iki uygulamanın da, hizmetten hizmete çağrılar dahil olmak üzere, App Service kimlik doğrulama ve yetkilendirmesi ile güvenli hale getirilmesidir.

Tebrikler! Sunucu kodunuz artık kimliği doğrulanmış kullanıcı adına arka uç verilerine erişir.

## <a name="call-api-securely-from-browser-code"></a>Tarayıcı kodundan güvenli bir şekilde API çağırma

Bu adımda, ön uç Angular.js uygulamasını arka uç API’sine yönlendireceksiniz. Bu şekilde, erişim belirtecini nasıl alacağınızı ve onu kullanarak arka uç uygulamasına API çağrılarını nasıl yapacağınızı öğreneceksiniz.

Sunucu kodu istek üst bilgilerine erişebilse de, istemci kodu aynı erişim belirteçlerini almak için `GET /.auth/me` erişimine sahiptir (bkz. [Uygulama kodunda belirteçleri alma](../app-service-authentication-how-to.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#retrieve-tokens-in-app-code)).

> [!TIP]
> Bu bölümde güvenli HTTP çağrılarını göstermek için standart HTTP yöntemleri kullanılır. Ancak, Angular.js uygulama desenini kolaylaştırmaya yardımcı olmak üzere [JavaScript için Active Directory Authentication Library (ADAL)](https://github.com/AzureAD/azure-activedirectory-library-for-js) kullanabilirsiniz.
>

### <a name="point-angularjs-app-to-back-end-api"></a>Angular.js uygulamasını arka uç API’sine yönlendirme

Yerel depoda _wwwroot/index.html_ dosyasını açın.

51. satırda, `apiEndpoint` değişkenini arka uç uygulamanızın URL'sine (`http://<back_end_app_name>.azurewebsites.net`) ayarlayın. _\<back\_end\_app\_name>_ değerini App Service’teki uygulamanızın adıyla değiştirin.

Yerel depoda _wwwroot/app/scripts/todoListSvc.js_ dosyasını açıp tüm API çağrılarının başına `apiEndpoint` ekinin getirildiğini görün. Angular.js uygulamanız artık arka uç API'lerini çağırır. 

### <a name="add-access-token-to-api-calls"></a>API çağrılarına erişim belirteci ekleme

_wwwroot/app/scripts/todoListSvc.js_ dosyasındaki API çağrıları listesinin üstünde (`getItems : function(){`. satırın üzerinde) listeye aşağıdaki işlevi ekleyin:

```javascript
setAuth: function (token) {
    $http.defaults.headers.common['Authorization'] = 'Bearer ' + token;
},
```

Bu işlev, erişim belirteci ile birlikte varsayılan `Authorization` üst bilgisini ayarlamak için çağrılır. Sonraki adımda bu belirteci çağıracaksınız.

Yerel depoda _wwwroot/app/scripts/app.js_ dosyasını açıp aşağıdaki kodu bulun:

```javascript
$routeProvider.when("/Home", {
    controller: "todoListCtrl",
    templateUrl: "/App/Views/TodoList.html",
}).otherwise({ redirectTo: "/Home" });
```

Tüm kod bloğunu aşağıdaki kod ile değiştirin:

```javascript
$routeProvider.when("/Home", {
    controller: "todoListCtrl",
    templateUrl: "/App/Views/TodoList.html",
    resolve: {
        token: ['$http', 'todoListSvc', function ($http, todoListSvc) {
            return $http.get('/.auth/me').then(function (response) {
                todoListSvc.setAuth(response.data[0].access_token);
                return response.data[0].access_token;
            });
        }]
    },
}).otherwise({ redirectTo: "/Home" });
```

Yeni değişiklik, `/.auth/me` çağrısını yapıp erişim belirtecini ayarlayan `revolve` eşlemesini ekler. `todoListCtrl` denetleyicisinin örneğini oluşturmadan önce erişim belirtecine sahip olduğunuzdan emin olur. Bu şekilde, denetleyicinin yaptığı tüm API çağrıları belirteci içerir.

### <a name="deploy-updates-and-test"></a>Güncelleştirmeleri dağıtma ve test etme

Yaptığınız tüm değişiklikleri kaydedin. Yerel terminal penceresinde, ön uç uygulamasında yaptığınız değişiklikleri aşağıdaki Git komutları ile dağıtın:

```bash
git add .
git commit -m "add authorization header for Angular"
git push frontend master
```

`http://<front_end_app_name>.azurewebsites.net` sayfasına yeniden gidin. Artık doğrudan Angular.js uygulamasında veri oluşturabilir, okuyabilir, güncelleştirebilir ve silebilirsiniz.

Tebrikler! İstemci kodunuz artık kimliği doğrulanmış kullanıcı adına arka uç verilerine erişir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Önceki adımlarda, bir kaynak grubunda Azure kaynakları oluşturdunuz. Bu kaynakların gelecekte gerekli olacağını düşünmüyorsanız, Cloud Shell’de aşağıdaki komutu çalıştırarak kaynak grubunu silin:

```azurecli-interactive
az group delete --name myAuthResourceGroup
```

Bu komutun çalıştırılması bir dakika sürebilir.

<a name="next"></a>
## <a name="next-steps"></a>Sonraki adımlar

Öğrendikleriniz:

> [!div class="checklist"]
> * Yerleşik kimlik doğrulaması ve yetkilendirmeyi etkinleştirme
> * Kimliği doğrulanmamış isteklere karşı uygulamaların güvenliğini sağlama
> * Azure Active Directory’yi kimlik sağlayıcısı olarak kullanma
> * Oturum açmış kullanıcının adına uzak bir uygulamaya erişme
> * Belirteç kimlik doğrulaması ile hizmetten hizmete güvenli çağrılar
> * Sunucu kodundan erişim belirteçleri kullanma
> * İstemci (tarayıcı) kodundan erişim belirteçleri kullanma

Uygulamanıza özel bir DNS adı eşlemeyle ilgili bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Mevcut bir özel DNS adını Azure App Service'e eşlemek](../app-service-web-tutorial-custom-domain.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
