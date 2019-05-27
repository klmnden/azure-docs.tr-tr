---
title: Azure portalında bir Bilişsel Hizmetler hesabı oluşturma
titlesuffix: Azure Cognitive Services
description: Azure portalında bir Azure Bilişsel hizmetler API hesabı oluşturma
services: cognitive-services
author: garyericson
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 03/26/2019
ms.author: garye
ms.openlocfilehash: 831f1d22c4da215bed3ed55b659332aa3b57472b
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66145934"
---
# <a name="quickstart-create-a-cognitive-services-account-in-the-azure-portal"></a>Hızlı Başlangıç: Azure portalında bir Bilişsel Hizmetler hesabı oluşturma

Bu hızlı başlangıçta, Azure Bilişsel hizmetler için kaydolun ve bir hizmet tek veya birden çok hizmet aboneliği oluşturmayı öğreneceksiniz. Bu hizmetler, Azure tarafından temsil edilen [kaynakları](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal), bir veya daha fazla Azure Bilişsel hizmetler API'leri için bağlanmanıza olanak tanır.

## <a name="prerequisites"></a>Önkoşullar

* Geçerli bir Azure aboneliği. [Hesap oluşturma](https://azure.microsoft.com/free/) ücretsiz.

## <a name="create-and-subscribe-to-an-azure-cognitive-services-resource"></a>Oluşturma ve bir Azure Bilişsel hizmetler kaynağı için abone olun

Başlamadan önce Azure Bilişsel hizmetler abonelik iki tür olduğunu bilmek önemlidir. İlk görüntü işleme veya konuşma Hizmetleri gibi tek bir hizmet için bir aboneliktir. Bu kaynak için bir tek hizmet aboneliği sınırlıdır. İkincisi, Azure Bilişsel hizmetler için birden çok hizmet bir aboneliktir. Bu abonelik, Azure Bilişsel Hizmetler'in bir çoğu için tek bir abonelik kullanmanıza olanak sağlar. Bu seçenek ayrıca faturalandırma birleştirir. Bkz: [Bilişsel hizmetler fiyatlandırması](https://azure.microsoft.com/pricing/details/cognitive-services/) ek bilgi için.

>[!WARNING]
> Şu anda bu hizmetler **yoksa** çok hizmet anahtarlarını destekler: Soru-cevap Oluşturucu, konuşma Hizmetleri, özel görüntü işleme ve Anomali algılayıcısı.

Sonraki bölümlerde, tek veya birden çok hizmet aboneliği oluşturmak adım adım açıklanmaktadır.


### <a name="multi-service-subscription"></a>Birden çok hizmet aboneliği

1. Oturum [Azure portalında](https://portal.azure.com), tıklatıp **+ kaynak Oluştur**.

    ![Bilişsel hizmetler API'leri seçin](media/cognitive-services-apis-create-account/azurePortalScreenMulti.png)

2. Arama çubuğu'nu bulun ve girin: **Bilişsel Hizmetler**.

    ![Bilişsel hizmetler için arama](media/cognitive-services-apis-create-account/azureCogServSearchMulti.png)

3. Seçin **Bilişsel Hizmetler**.

    ![Bilişsel hizmetler](media/cognitive-services-apis-create-account/azureMarketplaceMulti.png)

3. Üzerinde **Oluştur** sayfasında, aşağıdaki bilgileri sağlayın:

    |    |    |
    |--|--|
    | **Ad** | Bilişsel hizmetler kaynağınız için açıklayıcı bir ad. Örneğin, açıklayıcı bir ad kullanmanızı öneririz *MyCognitiveServicesAccount*. |
    | **Abonelik** | Kullanılabilir Azure aboneliklerinizi birini seçin. |
    | **Konum** | Bilişsel hizmet örneğinizin konumu. Farklı konumlara gecikmelere neden ancak kaynağınızı çalışma zamanı kullanılabilirliğini etkilemez sahip. |
    | **Fiyatlandırma katmanı** | Bilişsel hizmetler hesabınızın maliyeti, belirttiğiniz seçeneklere ve kullanımınıza bağlıdır. Daha fazla bilgi için bkz. API [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/cognitive-services/).
    | **Kaynak grubu** | [Azure kaynak grubu](https://docs.microsoft.com/azure/architecture/cloud-adoption/governance/resource-consistency/azure-resource-access#what-is-an-azure-resource-group) Bilişsel hizmetler kaynağınızı içerecek. Yeni bir grup oluşturmak veya önceden mevcut olan bir gruba ekleyin. |

    ![Kaynak oluşturma ekranı](media/cognitive-services-apis-create-account/resource_create_screen_multi.png)

### <a name="single-service-subscription"></a>Tek hizmet aboneliği

1. Oturum [Azure portalında](https://portal.azure.com), tıklatıp **+ kaynak Oluştur**.

    ![Bilişsel hizmetler API'leri seçin](media/cognitive-services-apis-create-account/azurePortalScreen.png)

2. Azure Marketi bölümünde seçin **yapay ZEKA + makine öğrenimi**. İlgilendiğiniz hizmet görmüyorsanız, tıklayarak **tümünü gör** Bilişsel hizmetler API'leri, tüm katalogunu görüntülemek için.

    ![Bilişsel hizmetler API'leri seçin](media/cognitive-services-apis-create-account/azureMarketplace.png)

3. Üzerinde **Oluştur** sayfasında, aşağıdaki bilgileri sağlayın:

    |    |    |
    |--|--|
    | **Ad** | Bilişsel hizmetler kaynağınız için açıklayıcı bir ad. Örneğin, açıklayıcı bir ad kullanmanızı öneririz *MyNameFaceAPIAccount*. |
    | **Abonelik** | Kullanılabilir Azure aboneliklerinizi birini seçin. |
    | **Konum** | Bilişsel hizmet örneğinizin konumu. Farklı konumlara gecikmelere neden ancak kaynağınızı çalışma zamanı kullanılabilirliğini etkilemez sahip. |
    | **Fiyatlandırma katmanı** | Bilişsel hizmetler hesabınızın maliyeti, belirttiğiniz seçeneklere ve kullanımınıza bağlıdır. Daha fazla bilgi için bkz. API [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/cognitive-services/).
    | **Kaynak grubu** | [Azure kaynak grubu](https://docs.microsoft.com/azure/architecture/cloud-adoption/governance/resource-consistency/azure-resource-access#what-is-an-azure-resource-group) Bilişsel hizmetler kaynağınızı içerecek. Yeni bir grup oluşturmak veya önceden mevcut olan bir gruba ekleyin. |

    ![Kaynak oluşturma ekranı](media/cognitive-services-apis-create-account/resource_create_screen.png)

## <a name="access-your-resource"></a>Kaynak erişimi

> [!NOTE]
> Abonelik sahipleri, Bilişsel hizmetler hesapları için kaynak gruplarında ve Aboneliklerde oluşturulmasını uygulayarak kapatabilir [Azure İlkesi](https://docs.microsoft.com/azure/governance/policy/overview#policy-definition)"izin verilmeyen kaynak türleri" için bir ilke tanımı atadıktan ve belirtme**Microsoft.CognitiveServices/accounts** hedef kaynak türü.

Bu sabitlenmiş kaynağınızı oluşturduktan sonra bunu Azure panosundan erişebilirsiniz. Aksi takdirde, bulabilirsiniz **kaynak grupları**.

Bilişsel hizmetler kaynağınızı uç nokta URL'si ve anahtarları kullanabilirsiniz **genel bakış** API oluşturmaya başlamak için bölüm uygulamalarınızda çağırır.

![Kaynaklarının ekranı](media/cognitive-services-apis-create-account/resourceScreen.png)

Konum ve anahtarları not edin. Tuşlarını seçerek alabilirsiniz **anahtarları** altında **kaynak yönetimi**.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Bilişsel hizmetler isteklerine kimlik doğrulaması](authentication.md)

## <a name="see-also"></a>Ayrıca bkz.

* [Hızlı Başlangıç: Bir görüntüden el yazısı metinleri ayıklamak](https://docs.microsoft.com/azure/cognitive-services/computer-vision/quickstarts/csharp-hand-text)
* [Öğretici: Çerçevenin bir resimdeki yüz ve algılamak için uygulama oluşturma](https://docs.microsoft.com/azure/cognitive-services/Face/Tutorials/FaceAPIinCSharpTutorial)
* [Bir özel arama Web sayfası oluşturma](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/tutorials/custom-search-web-page)
* [Language Understanding (LUIS) Bot Framework kullanarak bir bot ile tümleştirme](https://docs.microsoft.com/azure/cognitive-services/luis/luis-nodejs-tutorial-build-bot-framework-sample)
