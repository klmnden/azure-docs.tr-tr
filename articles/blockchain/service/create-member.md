---
title: Azure portalını kullanarak Azure blok zinciri hizmeti oluşturma
description: Consortium üye oluşturmak için Azure blok zinciri hizmetini kullanın.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/29/2019
ms.topic: quickstart
ms.service: azure-blockchain
ms.reviewer: janders
manager: femila
ms.openlocfilehash: 357dc47027582d5c638bb3c7344c839f37f93dc5
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66399144"
---
# <a name="quickstart-create-an-azure-blockchain-service-using-the-azure-portal"></a>Hızlı Başlangıç: Azure portalını kullanarak Azure blok zinciri hizmeti oluşturma

Azure Blockchain hizmeti, İş mantığınızın bir akıllı sözleşme yürütebilir blok zinciri bir platformdur. Bu hızlı başlangıçta Azure portalını kullanarak yönetilen bir kayıt defteri oluşturmaya başlama gösterilmektedir.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-managed-ledger"></a>Yönetilen bir kayıt defteri oluşturma

Azure Blockchain hizmeti, bir dizi işlem ve depolama kaynakları oluşturulur.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesini seçin.
1. Seçin **blok zinciri** > **Azure blok zinciri hizmet**.
1. Şablon tamamlayın.

    ![Hizmet oluşturma](./media/create-member/create-member.png)

    Ayar | Açıklama
    --------|------------
    Blok zinciri üyesi | Azure Blockchain Service üyelik tanımlayan benzersiz bir ad seçin. Blok zinciri üye adı yalnızca küçük harf ve rakam içerebilir. İlk karakter bir harf olmalıdır. Değeri 2 ile 20 karakter uzunluğunda olmalıdır.
    Abonelik | Hizmetiniz için kullanmak istediğiniz Azure aboneliğini seçin. Birden fazla aboneliğiniz varsa kaynağın faturalandığı aboneliği seçin.
    Kaynak grubu | Yeni bir kaynak grubu adı veya aboneliğinizde var olan bir kaynak grubu.
    Bölge | Konum consortium tüm üyeleri için aynı olması gerekir.
    Üye hesabı parolası | Üye hesabı parolası, bu üye için oluşturulan Ethereum hesabı için özel anahtarını şifrelemek için kullanılır. Üye hesabı ve üyesi hesap parolası consortium yönetimi için kullanın.
    Consortium adı | Yeni bir Konsorsiyumu için benzersiz bir ad girin. Davet aracılığıyla Konsorsiyumu birleştirilecekse, katıldığınız consortium değerdir.
    Açıklama | Consortium açıklaması.
    Protocol |  Önizleme çekirdek protokolünü destekler.
    Fiyatlandırma | Düğüm yapılandırması yeni hizmetiniz için. Seçin **standart**. 2 Doğrulayıcı düğümü ve 1 işlem varsayılan ayar düğümüdür.
    İşlem düğümü parola | Üyenin varsayılan işlem düğümü için parola. Parola, blok zinciri üyenin varsayılan işlem düğümü genel uç bağlanırken için temel kimlik doğrulaması kullanın.

1. Seçin **Oluştur** hizmeti sağlanamadı. Sağlama yaklaşık 10 dakika sürer.
1. Seçin **bildirimleri** dağıtım işlemini izlemek için araç çubuğunda.
1. Dağıtımdan sonra blok zinciri üyesine gidin.

Seçin **genel bakış**, hizmetinizi RootContract adresi ve üye hesapları dahil olmak üzere hakkında temel bilgileri görüntüleyebilirsiniz.

![Blok zinciri üyelerine genel bakış](./media/create-member/overview.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sonraki hızlı veya öğretici için oluşturduğunuz üye kullanabilirsiniz. Artık gerekli olmadığında silerek kaynakları silebilirsiniz `myResourceGroup` oluşturduğunuz kaynak grubunu Azure Blockchain hizmeti tarafından.

Kaynak grubunu silmek için:

1. Azure portalında gidin **kaynak grubu** sol gezinti bölmesi ve silmek istediğiniz kaynak grubunu seçin.
2. **Kaynak grubunu sil**'i seçin. Kaynak grubu adı girerek silme doğrulamak **Sil**.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [MetaMask kullanarak bağlanmak ve akıllı bir sözleşme dağıtma](connect-metamask.md)