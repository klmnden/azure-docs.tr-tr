---
title: Azure portalında bir Bilişsel Hizmetler hesabı oluşturma
titlesuffix: Azure Cognitive Services
description: Azure portalında bir Azure Bilişsel hizmetler API hesabı oluşturma
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 06/11/2019
ms.author: aahi
ms.openlocfilehash: b857ee0395c447c8699b8f6a812853528812a7bd
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67445866"
---
# <a name="create-a-cognitive-services-account-using-the-azure-portal"></a>Azure portalını kullanarak bir Bilişsel Hizmetler hesabı oluşturma

Bu hızlı başlangıçta, Azure Bilişsel hizmetler için kaydolun ve bir hizmet tek veya birden çok hizmet aboneliği olan bir hesap oluşturmayı öğreneceksiniz. Bu hizmetler, Azure tarafından temsil edilen [kaynakları](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal), bir veya daha fazla Azure Bilişsel hizmetler API'leri için bağlanmanıza olanak tanır.

## <a name="prerequisites"></a>Önkoşullar

* Geçerli bir Azure aboneliği. [Hesap oluşturma](https://azure.microsoft.com/free/) ücretsiz.

[!INCLUDE [cognitive-services-subscription-types](../../includes/cognitive-services-subscription-types.md)]

## <a name="create-a-new-azure-cognitive-services-resource"></a>Yeni bir Azure Bilişsel hizmetler kaynağı oluşturma

Bir kaynak oluşturmadan önce bir Azure kaynak grubu olması gerekir. Tüm Bilişsel Hizmetler hesabı (ve ilişkili Azure kaynağı) bir Azure kaynak grubuna ait olmalıdır. Hesap oluşturduğunuzda, yeni bir kaynak grubu oluşturabilir veya mevcut bir seçeneğiniz vardır. Bu makalede yeni bir kaynak grubu oluşturma işlemini gösterir.

1. Oturum [Azure portalında](https://portal.azure.com), tıklatıp **+ kaynak Oluştur**.

    ![Bilişsel hizmetler API'leri seçin](media/cognitive-services-apis-create-account/azurePortalScreenMulti.png)

2. Kullanılabilir Bilişsel hizmetler ile aşağıdaki şekillerde bulabilirsiniz:
    * Arama çubuğunu kullanın ve abone olmak istediğiniz hizmetin adını girin.
        * Birden çok hizmet bir abonelik için bir kaynak oluşturmak için girin **Bilişsel Hizmetler** arama çubuğu ve select **Bilişsel Hizmetler** kaynak.

        ![Bilişsel hizmetler için arama](media/cognitive-services-apis-create-account/azureCogServSearchMulti.png)

    * Kullanılabilen tüm bilişsel hizmetler görmek için seçin **yapay ZEKA + makine öğrenimi**altında **Azure Marketi**. İlgilendiğiniz hizmet görmüyorsanız, tıklayarak **tümünü gör** kaydırın **Bilişsel Hizmetler**. Tıklayın **daha fazla** Bilişsel hizmetler API'leri, tüm katalogunu görüntülemek için.
    
        ![Bilişsel hizmetler API'leri seçin](media/cognitive-services-apis-create-account/azureMarketplace.png)

3. Üzerinde **Oluştur** sayfasında, aşağıdaki bilgileri sağlayın:

    > [!IMPORTANT]
    > Azure Bilişsel hizmetler çağırırken gerekebilir Azure konumunuz unutmayın.

    |    |    |
    |--|--|
    | **Ad** | Bilişsel hizmetler kaynağınız için açıklayıcı bir ad. Örneğin, açıklayıcı bir ad kullanmanızı öneririz *MyCognitiveServicesAccount*. |
    | **Abonelik** | Kullanılabilir Azure aboneliklerinizi birini seçin. |
    | **Location** | Bilişsel hizmet örneğinizin konumu. Farklı konumlara gecikmelere neden ancak kaynağınızı çalışma zamanı kullanılabilirliğini etkilemez sahip. |
    | **Fiyatlandırma katmanı** | Bilişsel hizmetler hesabınızın maliyeti, belirttiğiniz seçeneklere ve kullanımınıza bağlıdır. Daha fazla bilgi için bkz. API [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/cognitive-services/).
    | **Kaynak grubu** | [Azure kaynak grubu](https://docs.microsoft.com/azure/architecture/cloud-adoption/governance/resource-consistency/azure-resource-access#what-is-an-azure-resource-group) Bilişsel hizmetler kaynağınızı içerecek. Yeni bir grup oluşturmak veya önceden mevcut olan bir gruba ekleyin. |

    ![Kaynak oluşturma ekranı](media/cognitive-services-apis-create-account/resource_create_screen.png)


## <a name="get-the-keys-for-your-subscription"></a>Aboneliğiniz için anahtarları alma

Bu sabitlenmiş kaynağınızı oluşturduktan sonra bunu Azure panosundan erişebilirsiniz. Aksi takdirde, bulabilirsiniz **kaynak grupları**. Kaynağınızı seçtikten sonra tuşlarını seçerek alabilirsiniz **anahtarları** altında **kaynak yönetimi**.

[!INCLUDE [cognitive-services-environment-variables](../../includes/cognitive-services-environment-variables.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Temizleme ve Bilişsel hizmetler abonelik kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, kaynak grubuyla ilişkili diğer tüm kaynakları siler.

Azure portalını kullanarak kaynak grubunu kaldırmak için:

1. Azure portalında sol taraftaki menüyü genişleterek hizmet menüsünü açın ve **Kaynak Grupları**'nı seçerek kaynak gruplarınızın listesini görüntüleyin.
2. Silinecek kaynak grubunu bulun ve listenin sağ tarafında daha fazla düğmesini (…) sağ tıklayın.
3. **Kaynak grubunu sil**'i seçip onaylayın.

## <a name="see-also"></a>Ayrıca bkz.

* [Azure Bilişsel hizmetler isteklerine kimlik doğrulaması](authentication.md)
* [Azure Bilişsel hizmetler nelerdir?](Welcome.md)
* [Doğal dil desteği](language-support.md)
* [Docker kapsayıcı desteği](cognitive-services-container-support.md)
