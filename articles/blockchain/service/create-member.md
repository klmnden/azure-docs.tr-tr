---
title: Azure portalını kullanarak Azure blok zinciri hizmeti oluşturma
description: Consortium üye oluşturmak için Azure blok zinciri hizmetini kullanın.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: quickstart
ms.service: azure-blockchain
ms.reviewer: janders
manager: femila
ms.openlocfilehash: 51775c5534a13fb2515fafa182658beafd38c1eb
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65026896"
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

    ![Hizmet Oluşturma](./media/create-member/create-member.png)

    Ayar | Açıklama
    --------|------------
    Blok zinciri üyesi | Azure Blockchain Service üyelik tanımlayan benzersiz bir ad seçin. Blok zinciri üye adı yalnızca küçük harf ve rakam içerebilir. İlk karakter bir harf olmalıdır. Değeri 2 ile 20 karakter uzunluğunda olmalıdır.
    Abonelik | Hizmetiniz için kullanmak istediğiniz Azure aboneliğini seçin. Birden fazla aboneliğiniz varsa kaynağın faturalandığı aboneliği seçin.
    Kaynak grubu | Yeni bir kaynak grubu adı veya aboneliğinizde var olan bir kaynak grubu.
    Bölge | Konum consortium tüm üyeleri için aynı olması gerekir.
    Üye hesabı parolası | Üye hesabı için yeni bir parola sağlayın. Üye hesabı parolası, temel kimlik doğrulaması kullanarak blok zinciri üyenin genel uç kimlik doğrulaması için kullanılır.
    Consortium adı | Yeni bir Konsorsiyumu için benzersiz bir ad girin. Davet aracılığıyla Konsorsiyumu birleştirilecekse, katıldığınız consortium değerdir.
    Açıklama | Consortium açıklaması.
    Protokol |  Önizleme çekirdek protokolünü destekler.
    Fiyatlandırma | Düğüm yapılandırması yeni hizmetiniz için. Seçin **standart**. 2 Doğrulayıcı düğümü ve 1 işlem varsayılan ayar düğümüdür.

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