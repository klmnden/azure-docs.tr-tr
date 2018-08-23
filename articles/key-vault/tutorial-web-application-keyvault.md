---
title: Anahtar Kasasından gizli dizi okumak için bir Azure web uygulaması yapılandırma öğreticisi | Microsoft Docs
description: Öğretici Anahtar Kasasından gizli dizi okumak için bir ASP.NET Core uygulaması yapılandırma
services: key-vault
documentationcenter: ''
author: barclayn
manager: mbaldwin
ms.assetid: 0e57f5c7-6f5a-46b7-a18a-043da8ca0d83
ms.service: key-vault
ms.workload: identity
ms.topic: tutorial
ms.date: 05/17/2018
ms.author: barclayn
ms.custom: mvc
ms.openlocfilehash: 91e2047998d6e743691821c631e15c94cd63cf15
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41920938"
---
# <a name="tutorial-configure-an-azure-web-application-to-read-a-secret-from-key-vault"></a>Öğretici: Anahtar Kasasından gizli dizi okumak için bir Azure web uygulaması yapılandırma

Bu öğreticide, Azure web uygulamasının yönetilen hizmet kimliklerini kullanarak Anahtar Kasasından bilgi okumasını sağlamak için gerekli adımların üzerinden geçeceksiniz. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Anahtar Kasası oluşturun.
> * Anahtar Kasasında bir gizli dizi depolayın.
> * Azure Web Uygulaması oluşturun.
> * Yönetilen hizmet kimliklerini etkinleştirme
> * Uygulamanın Anahtar Kasasından verileri okuması için gereken izinleri verin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

CLI kullanarak Azure'da oturum açmak için şunu yazabilirsiniz:

```azurecli
az login
```

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#az-group-create) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki örnek *eastus* konumunda *ContosoResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli
# To list locations: az account list-locations --output table
az group create --name "ContosoResourceGroup" --location "East US"
```

Az önce oluşturduğunuz kaynak grubu bu öğretici boyunca kullanılır.

## <a name="create-an-azure-key-vault"></a>Azure Key Vault oluşturma

Daha sonra, önceki adımda oluşturulan kaynak grubunda bir Anahtar Kasası oluşturursunuz. Bu öğretici boyunca Anahtar Kasası için “ContosoKeyVault” adı kullanılıyor olsa da, sizin benzersiz bir ad kullanmanız gerekir. Şu bilgileri belirtin:

* **ContosoKeyVault** kasa adı.
* **ContosoResourceGroup** kaynak grubu adı.
* **Doğu ABD** konumu.

```azurecli
az keyvault create --name "ContosoKeyVault" --resource-group "ContosoResourceGroup" --location "East US"
```

Bu komutun çıkışı, yeni oluşturulan Anahtar Kasasının özelliklerini gösterir. Aşağıda listelenen iki özelliği not edin:

* **Kasa Adı**: Örnekte **ContosoKeyVault**'tur. Anahtar Kasanızın adını tüm Anahtar Kasası komutları için kullanacaksınız.
* **Kasa URI'si**: Örnekte https://<YourKeyVaultName>.vault.azure.net/ şeklindedir. REST API'si aracılığıyla kasanızı kullanan uygulamaların bu URI'yi kullanması gerekir.

>[!IMPORTANT]
> 'vault_name' parametresinin şu desene uyması gerekir: '^[a-zA-Z0-9-]{3,24}$' hatasını alırsanız, -name parametre değeri benzersiz değildir veya 3 ile 24 karakter uzunluğunda alfasayısal karakterlerden oluşmuş bir dizeye uymuyordur.

Bu noktada Azure hesabınız, bu yeni anahtar kasasında herhangi bir işlemi gerçekleştirmeye yetkili olan tek hesaptır.

## <a name="add-a-secret-to-key-vault"></a>Anahtar kasasına gizli dizi ekleme

Bunun nasıl çalıştığını göstermemize yardımcı olması için bir gizli dizi ekliyoruz. Güvenle korumanız ama uygulamanıza da sağlamanız gereken bir SQL bağlantı dizesini veya başka bir bilgiyi depoluyor olabilirsiniz. Bu öğreticide, parola **AppSecret** olarak adlandırılır ve içinde **MySecret** değeri depolanır.

Anahtar Kasasında **MySecret** değerini depolayacak **AppSecret** adlı gizli dizi oluşturmak için aşağıdaki komutları yazın:

```azurecli
az keyvault secret set --vault-name "ContosoKeyVault" --name "AppSecret" --value "MySecret"
```

Gizli dizi içindeki değeri düz metin olarak görüntülemek için:

```azurecli
az keyvault secret show --name "AppSecret" --vault-name "ContosoKeyVault"
```

Bu komut, URI de dahil olmak üzere gizli bilgiyi gösterir. Bu adımları tamamladıktan sonra, Azure Key Vault'ta bu gizli dizinin URI'sine sahip olmalısınız. Bu bilgileri not alın. Sonraki adımlardan birinde gerekecektir.

## <a name="create-a-web-app"></a>Web uygulaması oluşturma

Bu bölümde bir ASP.NET MVC uygulaması oluşturacak ve bunu Azure'da bir Web Uygulaması olarak dağıtacaksınız. Azure Web Apps hakkında daha fazla bilgi için bkz. [Web Apps'e genel bakış](../app-service/app-service-web-overview.md).

1. Visual Studio'da **Dosya > Yeni > Proje**’yi seçerek bir proje oluşturun. 

2. **Yeni Proje** iletişim kutusunda **Visual C# > Web > ASP.NET Core Web Uygulaması** öğesini seçin.

3. Uygulamayı **WebKeyVault** olarak adlandırın ve ardından **Tamam**'ı seçin.
   >[!IMPORTANT]
   > Kopyalayıp yapıştırdığınız kodun ad alanını (namespace) eşleştirebilmesi için uygulamaya WebKeyVault adı vermelisiniz. Siteye başka bir ad verirseniz, site adıyla eşleşecek şekilde kodu değiştirmeniz gerekir.

    ![Yeni ASP.NET Projesi iletişim kutusu](media/tutorial-web-application-keyvault/aspnet-dialog.png)

4. Azure’a herhangi bir türde ASP.NET Core web uygulaması dağıtabilirsiniz. Bu öğreticide **Web Uygulaması** şablonunu seçin ve kimlik doğrulamasının **Kimlik Doğrulaması Yok** olarak ayarlandığından emin olun.

    ![ASPNET kimlik doğrulaması yok iletişim kutusu](media/tutorial-web-application-keyvault/aspnet-noauth.png)

5. **Tamam**’ı seçin.

6. ASP.NET Core projesi oluşturulduktan sonra, başlamanıza yardımcı olacak çeşitli kaynak bağlantıları sağlayan ASP.NET Core karşılama sayfası gösterilir.

7. Menüden **Hata Ayıkla > Hata Ayıklamadan Başla**’yı seçerek web uygulamasını yerel olarak çalıştırın.

## <a name="modify-the-web-app"></a>Web uygulamasını değiştirme

Web uygulamanız için yüklemiş olmanız gereken iki NuGet paketi vardır. Bunları yüklemek için aşağıdaki adımları izleyin:

1. Çözüm gezgininde web sitenizin adına sağ tıklayın.
2. **Çözüm için NuGet paketlerini yönet...** öğesini seçin.
3. Arama kutusunun yanındaki onay kutusunu seçin. **Ön sürümü dahil et**
4. Aşağıda listelenen iki NuGet paketi için arama yapın ve bunların çözümünüze eklenmesini kabul edin:

    * [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) - Hizmetten Azure Hizmetine kimlik doğrulama senaryoları için erişim belirteçlerinin getirilmesini kolaylaştırır. 
    * [Microsoft.Azure.KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault) - Anahtar Kasası ile etkileşim kurma yöntemleri içerir.

5. Çözüm Gezgini'ni kullanarak `Program.cs` dosyasını açın ve Program.cs dosyasının içeriğini aşağıdaki kodla değiştirin. ```<YourKeyVaultName>``` yerine anahtar kasanızın adını yazın:

    ```csharp
    
    using Microsoft.AspNetCore;
    using Microsoft.AspNetCore.Hosting;
    using Microsoft.Azure.KeyVault;
    using Microsoft.Azure.Services.AppAuthentication;
    using Microsoft.Extensions.Configuration;
    using Microsoft.Extensions.Configuration.AzureKeyVault;
    
    namespace WebKeyVault
    {
       public class Program
       {
           public static void Main(string[] args)
           {
               BuildWebHost(args).Run();
           }

           public static IWebHost BuildWebHost(string[] args) =>
           WebHost.CreateDefaultBuilder(args)
               .ConfigureAppConfiguration((ctx, builder) =>
               {
                   var keyVaultEndpoint = GetKeyVaultEndpoint();
                   if (!string.IsNullOrEmpty(keyVaultEndpoint))
                   {
                       var azureServiceTokenProvider = new AzureServiceTokenProvider();
                       var keyVaultClient = new KeyVaultClient(
                           new KeyVaultClient.AuthenticationCallback(
                               azureServiceTokenProvider.KeyVaultTokenCallback));
                       builder.AddAzureKeyVault(
                           keyVaultEndpoint, keyVaultClient, new DefaultKeyVaultSecretManager());
                   }
               }
            ).UseStartup<Startup>()
             .Build();

           private static string GetKeyVaultEndpoint() => "https://<YourKeyVaultName>.vault.azure.net";
         }
    }
    ```

6. Çözüm Gezgini'ni kullanarak **Sayfalar** bölümüne gidin ve `About.cshtml` dosyasını açın. **About.cshtml.cs** dosyasını içeriğini aşağıdaki kodla değiştirin:

    ```csharp
    
    using Microsoft.AspNetCore.Mvc.RazorPages;
    using Microsoft.Extensions.Configuration;
    
    namespace WebKeyVault.Pages
    {
        public class AboutModel : PageModel
        {
            public AboutModel(IConfiguration configuration)
            {
                _configuration = configuration;
            }
    
            private readonly IConfiguration _configuration = null;
            public string Message { get; set; }
    
            public void OnGet()
            {
                Message = "My key val = " +  _configuration["AppSecret"];
            }
        }
    }
    
    ```

7. Ana menüden **Hata Ayıkla** > **Hata Ayıklama Olmadan Başlat**'ı seçin. Tarayıcı görüntülendiğinde **Hakkında** sayfasına gidin. AppSecret'in değeri görüntülenir.

>[!IMPORTANT]
> HTTP Hatası 502.5 - İşlem Hatası iletisini alırsanız
> > `Program.cs` içinde belirtilen Key Vault adını doğrulayın

## <a name="publish-the-web-application-to-azure"></a>Web uygulamasını Azure’a yayımlama

1. Düzenleyicinin üst kısmında **WebKeyVault**'u seçin.
2. **Yayımla**'yı ve ardından **Başlat**'ı seçin.
3. Yeni bir **App Service** oluşturun ve **Yayımla**'yı seçin.
4. **Oluştur**’u seçin.

>[!IMPORTANT]
> Tarayıcı penceresi açılır ve 502.5 - İşlem Hatası iletisini görürsünüz. Bu beklenen bir durumdur. Anahtar Kasasından gizli dizileri okumak için uygulama kimliği hakları vermeniz gerekir.

## <a name="enable-managed-service-identity"></a>Yönetilen Hizmet Kimliğini etkinleştirme

Azure Key Vault kimlik bilgilerini ve diğer anahtarlarla gizli dizileri güvenle depolamak için bir yol sağlar, ama bunları alabilmek için kodunuzun Key Vault'ta kimlik doğrulaması yapması gerekir. Yönetilen Hizmet Kimliği (MSI), Azure hizmetlerine Azure Active Directory (Azure AD) üzerinde otomatik olarak yönetilen bir kimlik vererek bu soruna daha basit bir çözüm getirir. Bu kimliği kullanarak, Key Vault da dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen tüm hizmetlerde kodunuzda kimlik bilgileri bulunmasına gerek kalmadan kimlik doğrulaması yapabilirsiniz.

1. Azure CLI'ye dönme
2. Bu uygulamanın kimliğini oluşturmak için assign-identity komutunu çalıştırın:

```azurecli
az webapp identity assign --name "WebKeyVault" --resource-group "ContosoResourcegroup"
```

>[!NOTE]
>Bu komut, portala gidip web uygulaması özelliklerinde **Yönetilen hizmet kimliği** ayarını **Açık** duruma getirmekle eşdeğerdir.

## <a name="grant-rights-to-the-application-identity"></a>Uygulama kimliğine haklar verme

Azure portalı kullanarak Anahtar Kasasının erişim ilkelerine gidin ve kendinize Anahtar Kasasında Gizli Dizi Yönetimi erişimi verin. Bu sayede uygulamayı yerel geliştirme makinenizde çalıştırabilirsiniz.

1. Azure portaldaki **Kaynakları Ara** iletişim kutusunda Anahtar Kasanızı arayın.
2. **Erişim ilkeleri**'ni seçin.
3. **Yeni Ekle**'yi seçin, **Gizli dizi izinleri** bölümünde **Alma** ve **Liste**'yi seçin.
4. **Sorumlu Seç** öğesini seçin ve uygulama kimliğini ekleyin. Bunun adı uygulamanın adıyla aynı olur.
5. **Tamam**’ı seçin.

Artık Azure'daki hesabınız ve uygulama kimliği Anahtar Kasasından bilgi okuma haklarına sahiptir. Sayfayı yenilediğinizde, sitenin giriş sayfasını görüyor olmalısınız. **Hakkında**'yı seçerseniz, Anahtar Kasasında depoladığınız değeri görürsünüz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynak grubuyla birlikte bu kaynak grubunun tüm kaynaklarını silmek için **az group delete** komutunu kullanın.

  ```azurecli
  az group delete -n "ContosoResourceGroup"
  ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Key Vault Geliştirici Kılavuzu](key-vault-developers-guide.md)