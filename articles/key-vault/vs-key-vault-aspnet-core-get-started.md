---
title: Visual Studio'da (ASP.NET Core projeleri) anahtar kasası bağlı hizmeti ile çalışmaya başlama | Microsoft Docs
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
ms.openlocfilehash: e591771ee9c9cb12d9ec2ff61ec7f5a76691c8c7
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "42057056"
---
# <a name="get-started-with-key-vault-connected-service-in-visual-studio-aspnet-core-projects"></a>Visual Studio'da (ASP.NET Core projeleri) anahtar kasası bağlı hizmeti ile çalışmaya başlama

> [!div class="op_single_selector"]
> - [Başlarken](vs-key-vault-aspnet-core-get-started.md)
> - [Ne oldu](vs-key-vault-aspnet-core-what-happened.md)

Bir ASP.NET Core projesi için Key Vault ekledikten sonra bu makalede ek yönergeler sunulmuştur **bağlı hizmet Ekle** Visual Studio'da komutu. Hizmet projenize henüz eklediyseniz, herhangi bir zamanda yönergelerini takip ederek bunu yapabilirsiniz [Key Vault'a eklemek Visual Studio bağlı Hizmetler'i kullanarak web uygulamanızı](vs-key-vault-add-connected-service.md).

Bkz: [ASP.NET Core projeme ne oldu?](vs-key-vault-aspnet-core-what-happened.md) projenize bağlı hizmet ekleme sırasında yapılan değişiklikler için.

## <a name="after-you-connect"></a>Bağlandıktan sonra

1. Azure Key Vault gizli dizi ekleyin. Portalı'nda doğru yere almak için bu anahtar Kasası'nda depolanan Yönet gizli bağlantısına tıklayın. Sayfa veya proje kapattıysanız, kendisine gidebilirsiniz [Azure portalında](https://portal.azure.com) seçerek **tüm hizmetleri**altında **güvenlik**, seçin **Key Vault**, yeni oluşturduğunuz anahtar Kasası'nı seçin.

   ![Portalında gezinme](media/vs-key-vault-add-connected-service/manage-secrets-link.jpg)

1. Anahtar için Key Vault bölümünde oluşturduğunuz kasa öğesini **gizli dizileri**, ardından **Oluştur/içeri aktarma**.

   ![Gizli dizi Oluştur/İçeri Aktar](media/vs-key-vault-add-connected-service/generate-secrets.jpg)

1. "Ettiyseniz" gibi bir gizli dizi girin ve bir test olarak herhangi bir dize değeri verin ve ardından seçin **Oluştur** düğmesi.

   ![Gizli anahtar oluşturma](media/vs-key-vault-add-connected-service/create-a-secret.jpg)
 
1. (isteğe bağlı) Başka bir gizli diziyi girin, ancak bu kez, bir kategoriye adlandırma tarafından put **gizli dizileri--ettiyseniz**. Bu sözdizimi bir kategori belirtir **gizli dizileri** bir gizli dizi içeren **ettiyseniz.**
1. Visual Studio projenizde, kodda aşağıdaki ifadeleri kullanarak artık bu gizli dizileri başvurabilirsiniz:
 
   ```csharp
      config["MySecret"] // Access a secret without a section
      config["Secrets:MySecret"] // Access a secret in a section
      config.GetSection("Secrets")["MySecret"] // Get the configuration section and access a secret in it.
   ```

Tebrikler, web uygulamanızı güvenli şekilde depolanan gizli dizileri erişmek için Key Vault kullanmaya şimdi etkinleştirildi.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu silin. Bu, Key Vault ve ilgili kaynakları siler. Kaynak grubunu portal aracılığıyla silmek için:

1. Portalın üst kısmındaki Arama kutusuna kaynak grubunuzun adını girin. Bu Hızlı Başlangıçta kullanılan kaynak grubunu arama sonuçlarında gördüğünüzde seçin.
2. **Kaynak grubunu sil**'i seçin.
3. **KAYNAK GRUBU ADINI YAZIN:** kutusuna kaynak grubunun adını yazın ve **Sil**’i seçin.

# <a name="next-steps"></a>Sonraki adımlar

Anahtar Kasası'nda ile geliştirme hakkında daha fazla bilgi [anahtar kasası Geliştirici Kılavuzu](key-vault-developers-guide.md)