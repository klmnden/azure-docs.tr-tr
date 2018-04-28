---
title: Bir gizli anahtar Kasası'nı okumak için bir Azure web uygulaması yapılandırma | Microsoft Docs
description: Öğretici bir gizli anahtar Kasası'nı okumak için bir ASP.Net core uygulama yapılandırma
services: key-vault
documentationcenter: ''
author: barclayn
manager: mbaldwin
ms.assetid: ''
ms.service: key-vault
ms.workload: identity
ms.topic: article
ms.date: 04/16/2018
ms.author: barclayn
ms.custom: mvc
ms.openlocfilehash: 4a765b314b9879877bb6ff926e4a6584456b7823
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="tutorial-configure-an-azure-web-application-to-read-a-secret-from-key-vault"></a>Öğretici: bir gizli anahtar Kasası'nı okumak için bir Azure web uygulaması yapılandırma

Bu öğreticide, anahtar Kasası'nı kullanarak yönetilen hizmet kimlikleri bilgileri okumak için bir Azure web uygulaması için gerekli adımlar üzerine gidin. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Bir anahtar kasası oluşturun.
> * Bir gizli anahtar kasasına depolar.
> * Bir Azure Web uygulaması oluşturun.
> * Yönetilen hizmet kimlikleri etkinleştir
> * Uygulamanın anahtar Kasası'nı verileri okumak gerekli izinleri verin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

CLI kullanarak Azure'da oturum açma için yazabilirsiniz:

```azurecli
az login
```

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#az_group_create) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli
az group create --name ContosoResourceGroup --location eastus
```

Yeni oluşturduğunuz kaynak grubu Bu öğretici kullanılır.

## <a name="create-an-azure-key-vault"></a>Azure Key Vault oluşturma

Ardından önceki adımda oluşturduğunuz kaynak grubunda bir anahtar kasası oluşturun. Bazı bilgileri sağlamanız gerekir:

>[!NOTE]
> Bu öğretici boyunca bizim anahtar kasası adı olarak "ContosoKeyVault" kullanılsa da, benzersiz bir ad kullanmanız gerekir.

* Kasa adı **ContosoKeyVault**.
* Kaynak grubu adı **ContosoResourceGroup**.
* Konumun **Doğu ABD**.

```azurecli
az keyvault create --name '<YourKeyVaultName>' --resource-group ContosoResourceGroup --location eastus
```

Bu komutun çıktısı, yeni oluşturduğunuz anahtar kasasının özelliklerini gösterir. Aşağıda listelenen iki özellik not edin:

* **Kasa Adı**: Örnekte **ContosoKeyVault**'tur. Tüm anahtar kasası komutları için anahtar kasasının adını kullanır.
* **Kasa URI'si**: https:// budur örnekte<YourKeyVaultName>.vault.azure.net/. REST API'si aracılığıyla kasanızı kullanan uygulamaların bu URI'yi kullanması gerekir.

>[!IMPORTANT]
> Parametre 'vault_name' da aşağıdaki deseni uygun olmalıdır hata alırsanız: ' ^ [bir-zA-Z0 - 9-] {3,24} $' adı parametre değeri benzersiz değil veya 3 alfa sayısal karakterler-24 uzun oluşan bir dizeye uymuyor.

Bu noktada, Azure hesabınızı bu yeni kasa herhangi bir işlemi gerçekleştirme yetkisine sahip tek bir ' dir.

## <a name="add-a-secret-to-key-vault"></a>Bir gizli anahtar Kasası'na ekleyin

Bunun nasıl çalıştığı göstermeye yardımcı olmak için bir gizlilik ekliyoruz. Bir SQL bağlantı dizesi veya güvenli bir şekilde tutmak ancak uygulamanız için kullanılabilir hale gereken diğer bilgileri depolamak. Bu öğreticide parola çağrılacağı **AppSecret** ve değerini depolar **ettiyseniz** da.

Gizli anahtarı kasaya adlı oluşturmak için aşağıdaki komutları yazın **AppSecret** değeri depolar **ettiyseniz**:

```azurecli
az keyvault secret set --vault-name '<YourKeyVaultName>' --name 'AppSecret' --value 'MySecret'
```

Gizli dizi içindeki değeri düz metin olarak görüntülemek için:

```azurecli
az keyvault secret show --name 'AppSecret' --vault-name '<YourKeyVaultName>'
```

Bu komut, URI gizli bilgilerini gösterir. Bu adımları tamamladıktan sonra bir Azure anahtar kasasına gizli bir URI olmalıdır. Bu bilgileri not edin. Bir sonraki adımda ihtiyaç.

## <a name="create-a-web-app"></a>Web uygulaması oluşturma

Bu bölümde bir ASP.NET MVC uygulaması oluşturma ve Azure Web uygulaması olarak dağıtın. Azure Web Apps hakkında daha fazla bilgi için bkz: [Web Apps'e genel bakış](../app-service/app-service-web-overview.md).

1. Visual Studio'da **Dosya > Yeni > Proje**’yi seçerek bir proje oluşturun. 

2. **Yeni Proje** iletişim kutusunda **Visual C# > Web > ASP.NET Core Web Uygulaması** öğesini seçin.

3. Uygulama adı **WebKeyVault**ve ardından **Tamam**.
   >[!IMPORTANT]
   > Kopyalama ve yapıştırma kod ad eşleşecek şekilde WebKeyVault uygulama adı olmalıdır. Site başka bir şey adlandırırsanız site adı ile eşleşmesi için kodu değiştirmeniz gerekir.

    ![Yeni ASP.NET Projesi iletişim kutusu](media/tutorial-web-application-keyvault/aspnet-dialog.png)

4. Azure’a herhangi bir türde ASP.NET Core web uygulaması dağıtabilirsiniz. Bu öğretici için seçin **Web uygulaması** şablonu ve kimlik doğrulaması ayarlandığından emin olun **doğrulaması yok**.

    ![ASP.NET kimlik doğrulaması iletişim kutusu](media/tutorial-web-application-keyvault/aspnet-noauth.png)

5. **Tamam**’ı seçin.

6. ASP.NET Core projesi oluşturulduktan sonra, başlamanıza yardımcı olacak çeşitli kaynak bağlantıları sağlayan ASP.NET Core karşılama sayfası gösterilir.

7. Menüden **Hata Ayıkla > Hata Ayıklamadan Başla**’yı seçerek web uygulamasını yerel olarak çalıştırın.

## <a name="modify-the-web-app"></a>Web uygulamasını değiştirme

Yüklü web uygulamanız gereken iki NuGet paketi vardır. Yüklemek için aşağıdaki adımları izleyin:

1. Çözüm Gezgini, Web sitesi adına sağ tıklayın.
2. Seçin **çözüm için Manage NuGet paketlerini...**
3. Arama kutusuna yanındaki onay kutusunu seçin. **Yayın öncesi içerir**
4. İki NuGet paketlerini Ara aşağıda listelenen ve bunları çözümünüze eklenmesi için kabul edin:

    * [Microsoft.Azure.Services.AppAuthentication (Önizleme)](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) -hizmeti için Azure hizmeti kimlik doğrulama senaryoları için erişim belirteci getirme kolay hale getirir. 
    * [Microsoft.Azure.KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault/2.4.0-preview) -anahtar kasası ile etkileşim için yöntemler içerir.

5. Çözüm Gezgini açmak için kullanmak `Program.cs` ve Program.cs dosyasının içeriğini aşağıdaki kodla değiştirin. Yedek ```<YourKeyVaultName>``` anahtar kasanızı adı:

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
             )
                .UseStartup<Startup>()
                .Build();
    
            private static string GetKeyVaultEndpoint() => "https://<YourKeyVaultName>.vault.azure.net";
        }
        }
    ```

6. Çözüm Gezgini gitmek için kullanın **sayfaları** bölümünde ve açık `About.cshtml`. Değiştir **About.cshtml.cs** aşağıdaki kod ile:

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

7. Ana menüden **hata ayıklama** > **Başlat hata ayıklama olmadan**. Tarayıcı belirdiğinde gidin **hakkında** sayfası. AppSecret değeri görüntülenir.

>[!IMPORTANT]
> İşlem hatası iletisi bir HTTP hata 502.5 - alırsanız belirtilen anahtar kasasının adını doğrulayın `Program.cs`

## <a name="publish-the-web-application-to-azure"></a>Azure web uygulaması yayımlama

1. Düzenleyicisi'ni seçin **WebKeyVault**.
2. Seçin **yayımlama**.
3. seçin **Yayımla** yeniden.
4. Seçin **oluşturma**.

>[!IMPORTANT]
> Bir tarayıcı penceresi açar ve bir 502.5 - işlem hatası iletisi görürsünüz. Bu beklenen bir durumdur. Gizli anahtar Kasası'nı okumak için uygulama kimliği hakları gerekir.

## <a name="enable-managed-service-identity"></a>Yönetilen hizmet kimliği etkinleştir

Azure anahtar kasası kimlik bilgileri ve diğer anahtarları ve gizli anahtarları güvenli bir şekilde depolamak için bir yol sağlar, ancak bunları almak için anahtar Kasası'na kimlik doğrulaması kodunuzu gerekiyor. Yönetilen hizmet kimliği (MSI), Azure hizmetleri otomatik olarak yönetilen bir kimliği Azure Active Directory (Azure AD) vererek daha basit bu sorunun çözümüne yapar. Bu kimlik, anahtar kasası, kodunuzda herhangi bir kimlik bilgisi olmadan dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmeti için kimlik doğrulaması kullanabilirsiniz.

1. Azure CLI Döndür
2. Bu uygulama için kimlik oluşturmak için Ata-identity komutu çalıştırın:

```azurecli
az webapp assign-identity --name WebKeyVault --resource-group ContosoResourcegroup
```

>[!NOTE]
>Bu portala giderek ve geçiş eşdeğerdir **yönetilen hizmet kimliği** için **üzerinde** web uygulama özellikleri.

## <a name="grant-rights-to-the-application-identity"></a>Uygulama Kimliği hakkı verin

Azure Portalı'nı kullanarak, anahtar Kasası'nın erişim ilkelerini gidin ve kendinize anahtar kasasına gizli yönetim erişimi verin. Bu, yerel geliştirme makinenizde uygulamayı çalıştırmak olanak tanır.

1. Arama anahtar kasanız için **arama kaynakları** Azure portalında iletişim kutusu.
2. Seçin **erişim ilkeleri**.
3. Seçin **yeni Ekle**, **gizli izinleri** bölümünde seçin **almak** ve **listesi**.
4. Seçin **seçin asıl**ve uygulama kimliği ekleyin. Uygulama aynı ada sahip.
5. Seçin **Tamam**

Artık hesabınızı Azure ve uygulama kimliği, anahtar Kasası'nı bilgileri okumak için haklarına sahip. Sayfa yenileme sitenin giriş sayfası görmeniz gerekir. Seçerseniz **hakkında**. Anahtar kasasında depolanan değere bakın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir kaynak grubu ve tüm kaynaklarını silmek için kullanın **az grubu Sil** komutu.

  ```azurecli
  az group delete -n ContosoResourceGroup
  ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure anahtar kasası Geliştirici Kılavuzu](key-vault-developers-guide.md)
