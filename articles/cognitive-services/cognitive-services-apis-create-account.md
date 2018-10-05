---
title: Azure portalında bir Bilişsel hizmetler API hesabı oluşturma
titlesuffix: Azure Cognitive Services
description: Azure portalında Microsoft Bilişsel hizmetler API hesabı oluşturma
services: cognitive-services
author: garyericson
manager: cgronlun
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 02/01/2018
ms.author: garye
ms.openlocfilehash: 9c4aea2493ffed12b90f82113baf81c6c77a0ea2
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48801412"
---
# <a name="quickstart-create-a-cognitive-services-account-in-the-azure-portal"></a>Hızlı Başlangıç: Azure portalında bir Bilişsel Hizmetler hesabı oluşturma

Bu hızlı başlangıçta, Azure Bilişsel hizmetler'ı kullanmaya başlamak için kullanın. Bu hizmetler, Azure tarafından temsil edilen [kaynakları](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal), bir veya daha fazla birçok Bilişsel hizmetler API'leri kullanılabilir bağlanmanıza olanak tanır.

## <a name="prerequisites"></a>Önkoşullar

* Geçerli bir Azure aboneliği. yapabilecekleriniz [hesap oluşturma](https://azure.microsoft.com/free/) ücretsiz.

## <a name="create-and-subscribe-to-an-azure-cognitive-services-resource"></a>Oluşturma ve bir Azure Bilişsel hizmetler kaynağı için abone olun

1. Oturum [Azure portalında](http://portal.azure.com), tıklatıp **+ kaynak Oluştur**.
    
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
    | **Kaynak grubu** | [Azure kaynak grubu](https://docs.microsoft.com/azure/architecture/cloud-adoption/getting-started/azure-resource-access#what-is-an-azure-resource-group) Bilişsel hizmetler kaynağınızı içerecek. Yeni bir grup oluşturmak veya önceden var olan bir gruba ekleyin. |

    ![Kaynak oluşturma ekranı](media/cognitive-services-apis-create-account/resource_create_screen.png)

## <a name="access-your-resource"></a>Kaynak erişimi 

> [!NOTE]
> Abonelik sahipleri, Bilişsel hizmetler hesapları için kaynak gruplarında ve Aboneliklerde oluşturulmasını uygulayarak kapatabilir [Azure İlkesi](https://docs.microsoft.com/en-us/azure/governance/policy/overview#policy-definition)"izin verilmeyen kaynak türleri" için bir ilke tanımı atadıktan ve belirtme**Microsoft.CognitiveServices/accounts** hedef kaynak türü.

Bu sabitlenmiş kaynağınızı oluşturduktan sonra bunu Azure panosundan erişebilirsiniz. Aksi takdirde, bulabilirsiniz **kaynak grupları**.

Bilişsel hizmetler kaynağınızı uç nokta URL'si ve anahtarları kullanabilirsiniz **genel bakış** API oluşturmaya başlamak için bölüm uygulamalarınızda çağırır.

![Kaynaklarının ekranı](media/cognitive-services-apis-create-account/resourceScreen.png)

## <a name="next-steps"></a>Sonraki Adımlar

> [!div class="nextstepaction"]
> [Bilgisayar görüntü işleme API'si C# Öğreticisi](https://docs.microsoft.com/azure/cognitive-services/computer-vision/tutorials/csharptutorial)

## <a name="see-also"></a>Ayrıca bkz.

* [Hızlı Başlangıç: resimlerdeki el yazısı bir görüntüden ayıklayın.](https://docs.microsoft.com/azure/cognitive-services/computer-vision/quickstarts/csharp-hand-text)
* [Öğretici: algılamak ve bir görüntüdeki yüzleri çerçeve için uygulama oluşturma](https://docs.microsoft.com/azure/cognitive-services/Face/Tutorials/FaceAPIinCSharpTutorial)
* [Bir özel arama Web sayfası oluşturma](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/tutorials/custom-search-web-page)
* [Language Understanding (LUIS) Bot Framework kullanarak bir bot ile tümleştirme](https://docs.microsoft.com/azure/cognitive-services/luis/luis-nodejs-tutorial-build-bot-framework-sample)
