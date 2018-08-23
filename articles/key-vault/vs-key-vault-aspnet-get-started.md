---
title: Visual Studio'da (ASP.NET projeleri) anahtar kasası bağlı hizmeti ile çalışmaya başlama | Microsoft Docs
description: Bir ASP.NET veya ASP.NET Core web uygulaması için Key Vault desteği eklemek öğrenmenize yardımcı olmak için bu öğreticiyi kullanın.
services: key-vault
author: ghogen
manager: douge
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 04/15/2018
ms.author: ghogen
ms.openlocfilehash: 3ca62d47d8e7682c80985bf5409b8540382fbf45
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "42057148"
---
# <a name="get-started-with-key-vault-connected-service-in-visual-studio-aspnet-projects"></a>Visual Studio'da (ASP.NET projeleri) anahtar kasası bağlı hizmeti ile çalışmaya başlama

> [!div class="op_single_selector"]
> - [Başlarken](vs-key-vault-aspnet-get-started.md)
> - [Ne oldu](vs-key-vault-aspnet-what-happened.md)

Key Vault bir ASP.NET MVC projeye ekledikten sonra bu makalede ek yönergeler sunulmuştur **bağlı hizmet Ekle** Visual Studio'da komutu. Hizmet projenize henüz eklediyseniz, herhangi bir zamanda yönergelerini takip ederek bunu yapabilirsiniz [Key Vault'a eklemek Visual Studio bağlı Hizmetler'i kullanarak web uygulamanızı](vs-key-vault-add-connected-service.md).

Bkz: [ASP.NET projeme ne oldu?](vs-key-vault-aspnet-core-what-happened.md) projenize bağlı hizmet ekleme sırasında yapılan değişiklikler için.

## <a name="after-you-connect"></a>Bağlandıktan sonra

1. Azure Key Vault gizli dizi ekleyin. Portal doğru yerdesiniz almak için için bağlantıya tıklayın **bu anahtar Kasası'nda depolanan gizli dizileri Yönet**. Sayfa veya proje kapattıysanız, kendisine gidebilirsiniz [Azure portalında](https://portal.azure.com) seçerek **tüm hizmetleri**altında **güvenlik**, seçin **Key Vault**, yeni oluşturduğunuz anahtar Kasası'nı seçin.

   ![Portalında gezinme](media/vs-key-vault-add-connected-service/manage-secrets-link.jpg)

1. Anahtar için Key Vault bölümünde oluşturduğunuz kasa öğesini **gizli dizileri**, ardından **Oluştur/içeri aktarma**.

   ![Gizli dizi Oluştur/İçeri Aktar](media/vs-key-vault-add-connected-service/generate-secrets.jpg)

1. Bir gizli dizi girin; örn **ettiyseniz**ve bir test olarak herhangi bir dize değeri verin ve ardından **Oluştur** düğmesi.

   ![Gizli anahtar oluşturma](media/vs-key-vault-add-connected-service/create-a-secret.jpg)
 
1. (isteğe bağlı) Başka bir gizli diziyi girin, ancak bu kez, bir kategoriye adlandırma tarafından put **gizli dizileri--ettiyseniz**. Bu sözdizimi bir kategori belirtir **gizli dizileri** bir gizli dizi içeren **ettiyseniz**.

1. Web.config şu şekilde değiştirin. Anahtarları Key Vault'ta gizli diziler değerleriyle AzureKeyVault ConfigurationBuilder ile değiştirilecek tutuculardır.

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

1. Hakkında denetleyici yönteminde HomeController sırrı alınmaya ve içinde görünüm paketini depolamak için aşağıdaki satırları ekleyin.
 
   ```csharp
            var secret = ConfigurationManager.AppSettings["MySecret"];
            var secret2 = ConfigurationManager.AppSettings["Secrets--MySecret"];
            ViewBag.Secret = $"Secret: {secret}";
            ViewBag.Secret2 = $"Secret2: {secret2}";
   ```

1. About.cshtml Görünümü'nde (yalnızca test için) gizli dizi değerini görüntülemek için aşağıdakini ekleyin.

   ```csharp
      <h3>@ViewBag.Secret</h3>
      <h3>@ViewBag.Secret2</h3>
   ```

Tebrikler, web uygulamanızı güvenli şekilde depolanan gizli dizileri erişmek için Key Vault kullanmaya şimdi etkinleştirildi.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu silin. Bu, Key Vault ve ilgili kaynakları siler. Kaynak grubunu portal aracılığıyla silmek için:

1. Portalın üst kısmındaki Arama kutusuna kaynak grubunuzun adını girin. Bu Hızlı Başlangıçta kullanılan kaynak grubunu arama sonuçlarında gördüğünüzde seçin.
2. **Kaynak grubunu sil**'i seçin.
3. **KAYNAK GRUBU ADINI YAZIN:** kutusuna kaynak grubunun adını yazın ve **Sil**’i seçin.

# <a name="next-steps"></a>Sonraki adımlar

Anahtar Kasası'nda ile geliştirme hakkında daha fazla bilgi [anahtar kasası Geliştirici Kılavuzu](key-vault-developers-guide.md)