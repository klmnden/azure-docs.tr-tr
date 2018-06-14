---
title: Anahtar kasası bağlı hizmeti Visual Studio (ASP.NET projeleri) kullanmaya başlama | Microsoft Docs
description: Bir ASP.NET veya ASP.NET Core web uygulamasına anahtar kasası desteği eklemek öğrenmenize yardımcı olmak için bu öğreticiyi kullanın.
services: key-vault
author: ghogen
manager: douge
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 04/15/2018
ms.author: ghogen
ms.openlocfilehash: cd305801f10c899682aa6d751e48f30b6e8303fa
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33787405"
---
# <a name="get-started-with-key-vault-connected-service-in-visual-studio-aspnet-projects"></a>Anahtar kasası bağlı hizmeti Visual Studio (ASP.NET projeleri) kullanmaya başlama

> [!div class="op_single_selector"]
> - [Başlarken](vs-key-vault-aspnet-get-started.md)
> - [Ne oldu](vs-key-vault-aspnet-what-happened.md)

Bir ASP.NET MVC projesine anahtar kasası ekledikten sonra bu makalede, ek yönergeler sağlanmaktadır. **bağlı Hizmetleri Ekle** Visual Studio'da komutu. Projeniz için hizmet henüz eklediyseniz, herhangi bir zamanda konusundaki yönergeleri uygulayarak bunu yapabilirsiniz [Visual Studio bağlı hizmetleri kullanarak, web uygulamanıza eklemek anahtar kasası](vs-key-vault-add-connected-service.md).

Bkz: [my ASP.NET projesi ne?](vs-key-vault-aspnet-core-what-happened.md) projenize bağlı hizmet eklerken değişikliklerinin.

## <a name="after-you-connect"></a>Bağlandıktan sonra

1. Azure anahtar kasası bir gizlilik ekleyin. Portal doğru yerde almak için için bağlantıyı tıklatın **bu anahtar kasasında depolanan parolaları yönetme**. Sayfa veya proje kapattıysanız, kendisine gidebilirsiniz [Azure portal](https://portal.azure.com) seçerek **tüm hizmetleri**altında **güvenlik**, seçin **anahtar kasası**, yeni oluşturduğunuz anahtar Kasası'ı seçin.

   ![Portala gezinme](media/vs-key-vault-add-connected-service/manage-secrets-link.jpg)

1. Anahtar için anahtar kasası bölümünde oluşturduğunuz kasa, seçin **gizli**, ardından **Oluştur/içe aktarma**.

   ![Bir gizli anahtarı oluştur/içe aktarma](media/vs-key-vault-add-connected-service/generate-secrets.jpg)

1. Bir parola girin **ettiyseniz**ve bir test olarak herhangi bir dize değeri verin, sonra seçin **oluşturma** düğmesi.

   ![Gizli anahtar oluşturma](media/vs-key-vault-add-connected-service/create-a-secret.jpg)
 
1. (isteğe bağlı) Başka bir parolası girin, ancak bu kez, bir kategoriye adlandırma tarafından put **gizli--ettiyseniz**. Bir kategori bu sözdizimini belirtir **gizli** bir gizlilik içeren **ettiyseniz**.

1. Web.config aşağıdaki gibi değiştirin. Anahtarları, gizli anahtar kasasında değerlerle AzureKeyVault ConfigurationBuilder tarafından değiştirilecek yer tutucuları şunlardır.

   ```xml
     <appSettings configBuilders="AzureKeyVault">
       <add key="webpages:Version" value="3.0.0.0" />
       <add key="webpages:Enabled" value="false" />
       <add key="ClientValidationEnabled" value="true" />
       <add key="UnobtrusiveJavaScriptEnabled" value="true" />
       <add key="MySecret" value="dummy1"/>
       <add key="Secrets--MySecret" value="dummy2"/>
     </appSettings>
   ```

1. Hakkında denetleyici yönteminde HomeController ViewBag depolamak ve gizli anahtarı almak için aşağıdaki satırları ekleyin.
 
   ```csharp
            var secret = ConfigurationManager.AppSettings["MySecret"];
            var secret2 = ConfigurationManager.AppSettings["Secrets--MySecret"];
            ViewBag.Secret = $"Secret: {secret}";
            ViewBag.Secret2 = $"Secret2: {secret2}";
   ```

1. About.cshtml Görünümü'nde (yalnızca test için) gizli anahtarı değerini görüntülemek için aşağıdakini ekleyin.

   ```csharp
      <h3>@ViewBag.Secret</h3>
      <h3>@ViewBag.Secret2</h3>
   ```

Tebrikler, anahtar kasası güvenli şekilde depolanan gizli erişmek için kullanılacak web uygulamanız şimdi etkinleştirildi.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olduğunda, kaynak grubunu silin. Bu, anahtar kasası ve ilgili kaynakları siler. Kaynak grubunu portal aracılığıyla silmek için:

1. Portalın üst kısmındaki Arama kutusuna kaynak grubunuzun adını girin. Bu Hızlı Başlangıçta kullanılan kaynak grubunu arama sonuçlarında gördüğünüzde seçin.
2. **Kaynak grubunu sil**'i seçin.
3. **KAYNAK GRUBU ADINI YAZIN:** kutusuna kaynak grubunun adını yazın ve **Sil**’i seçin.

# <a name="next-steps"></a>Sonraki adımlar

Anahtar kasası ile geliştirme hakkında daha fazla bilgi edinin [anahtar kasası Geliştirici Kılavuzu](key-vault-developers-guide.md)