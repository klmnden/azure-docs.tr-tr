---
title: Sınırlı bir deneme proje Azure'a taşıyın
titlesuffix: Azure Cognitive Services
description: Sınırlı deneme sürümünde proje Azure'a taşımayı öğreneceksiniz.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: anroth
ms.openlocfilehash: a9f49af54f391b159f8b3d626fffc36635f5e51f
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2019
ms.locfileid: "56821319"
---
# <a name="how-to-move-your-limited-trial-project-to-azure-using-the-customvisionai-site"></a>Sınırlı deneme sürümünde projenizi CustomVision.ai site kullanarak Azure'a taşıma


Custom Vision Service'e artık olduğundan [Azure Önizleme](https://azure.microsoft.com/services/preview/), Azure dışındaki sınırlı deneme sürümünde projeleri için destek tarihinde sona eriyor. Bu belgenin nasıl kullanılacağını yardımcı olacak [Custom Vision Web sitesi](https://customvision.ai) sınırlı deneme sürümünde projenizi bir Azure kaynak ile ilişkilendirilecek taşımak için. 

> [!NOTE]
> Bir Azure kaynağı için özel görüntü işleme projelerinizi taşıdığınızda, temel devral [izinleri]( https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) ilgili Azure kaynağının. Kuruluşunuzdaki diğer kullanıcılara projenize konusu Azure kaynak sahipleri, bunlar üzerinde projenize erişmek mümkün olacaktır [Custom Vision Web sitesi](https://customvision.ai). Benzer şekilde, kaynaklarınızı silindiğinde, projelerinizi silinir.  


Abonelikler ve kaynaklar Azure kavramlarına giriş için bkz [Azure Geliştirici Kılavuzu.](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#manage-your-subscriptions)


## <a name="prerequisites"></a>Önkoşullar

Aynı Microsoft hesabı veya oturum açmak için kullandığınız Azure Active Directory (AAD) hesabı ile ilişkili geçerli bir Azure aboneliğine ihtiyacınız olacak [Custom Vision Web sitesi](https://customvision.ai). 

Azure hesabınız yoksa [hesap oluşturma](https://azure.microsoft.com/free/) ücretsiz.


## <a name="create-custom-vision-resources-in-the-azure-portal"></a>Azure portalında özel görüntü işleme kaynakları oluşturma
Azure ile özel görüntü işleme hizmeti kullanmak için Custom Vision eğitim ve tahmin kaynaklarında oluşturmanız gerekecektir [Azure portalında](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=microsoft_azure_cognitiveservices_customvision#create/Microsoft.CognitiveServicesCustomVision). 

 Bunu kullanarak projenize taşımak [Custom Vision Web sitesi](https://customvision.ai) deneyimi oluşturmanız gerekir, kaynaklarınızı Güney Orta ABD bölgesinde, Orta Güney ABD tüm sınırlı deneme sürümünde projelerin barındırıldığından. 

Birden çok proje için tek bir kaynak ilişkilendirilebilir. Hakkında daha fazla ayrıntı [fiyatlandırma ve limitler](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/limits-and-quotas) kullanılabilir. Özel görüntü işleme hizmeti ücretsiz olarak kullanmaya devam etmek için Azure portalında F0 katmanı seçebilirsiniz. 


## <a name="move-your-limited-trial-project-to-an-azure-resource"></a>Sınırlı deneme sürümünde projeniz için bir Azure kaynağı taşıma

1.  Web tarayıcınızda gidin [Custom Vision Web sitesi](https://customvision.ai) seçip __oturum__. Bir Azure hesabına geçirmek istediğiniz projeyi açın. 
2.  Ekranın üst sağ taraftaki köşedeki dişli simgesine tıklayarak projeniz için ayarlar sayfasını açın. 

    ![Proje ayarlarını proje sayfanın sağ üst kısmında dişli simgesinin seçeneğidir.](./media/move-your-project-to-azure/settings-icon.png)


3. Tıklayarak __Azure'a taşımak__.

    ![Proje Ayarları sayfası altındaki sol tarafında taşımadır Azure düğme.](./media/move-your-project-to-azure/move-to-azure.jpg)


4. Üzerinde açılır listeden __Azure'a taşımak__ düğmesi, projenize taşımak istediğiniz Azure kaynağını seçin. Tıklayın __taşıma__. 

5. Özel görüntü işleme hizmeti için daha önce oluşturduğunuz Azure kaynak görmüyorsanız, başka bir dizinde olabilir. Projeniz başka bir dizinde kaynak taşımak için aşağıdaki yönergeleri izleyin. 

    ![Proje geçiş penceresi.](./media/move-your-project-to-azure/Project_Migration_Window.jpg)


## <a name="move-project-to-another-azure-directory"></a>Proje başka bir Azure dizinine taşıyın 

> [!NOTE]
> Hem Azure portalı ve CustomVision.ai, ekranın sağ üst köşedeki aşağı açılan kullanıcı menüden dizininizi seçin.   


1. Azure kaynağınıza hangi dizin belirleyin. Azure portal menü çubuğunun sağ üst kısmında, kullanıcı adınızı altında listelenen dizine bulabilirsiniz. 

    ![Azure portal menü çubuğunun sağ üst kısmında, kullanıcı adınızın altındaki dizininize listelenir. .](./media/move-your-project-to-azure/identify_directory.jpg)

2. Custom Vision eğitim kaynağınızın kaynak Kimliğini bulun. Custom Vision eğitim kaynağınızı açıp "Kaynak yönetimi" bölümünde "Özellikler"'i seçerek bunu Azure portalında bulabilirsiniz. Kaynak Kimliğiniz olacak. 

    ![Kaynak Kimliği, Azure portalında, Custom Vision eğitim kaynağı açma ve "Özellikler" altındaki "Kaynak yönetimi" bölüm seçerek bulun.](./media/move-your-project-to-azure/resource_ID_azure_portal.jpg)


3. Alternatif olarak, özel görüntü işleme kaynağınızın kaynak kimliği doğrudan özel görüntü işleme Web bulabilirsiniz [Ayarları sayfası]( https://www.customvision.ai/projects#/settings). Azure kaynağınıza bulunduğu aynı dizine geçmeniz gerekir.

    ![Her bir kaynak Web sitesinde özel görüntü işleme, Ayarları sayfasında, kaynak kimliği listelenir.](./media/move-your-project-to-azure/resource_ID_CVS_portal.jpg)

4. Artık, kaynak Kimliğine sahip olduğunuza göre sınırlı bir deneme sürümünden bir Azure kaynağı için taşımaya çalışıyorsunuz Custom Vision projeye döndürür. Anımsatıcı bulmak için geri özgün dizininize geçmeniz gerekebilir. Sağlanan yönergeleri izleyin [yukarıda](#move-your-limited-trial-project-to-an-azure-resource) , proje Ayarları sayfası açmak ve seçmek için __Azure'a taşımak__. 


5. Git Azure penceresi, "Farklı bir Azure dizinine taşıma?" için kutuyu işaretleyin. Projenize taşınan kaynağın kaynak kimliği girin ve projenize taşımak istediğiniz dizini seçin. Tıklayın __taşıma__. 



5. Projenizi şimdi farklı bir dizinde olduğunu unutmayın. Projenizi bulmak için projenizin özel görüntü işleme web portalındaki ile aynı dizine geçin gerekecektir. Hem Azure portalında ve [Custom Vision Web sitesi](https://customvision.ai), ekranın sağ üst köşedeki aşağı açılan hesap menüsünden dizininizi seçin. 

## <a name="next-steps"></a>Sonraki adımlar

Projenizi şimdi bir Azure kaynağına taşındı. Eğitim ve tahmin anahtarlarınızı yazmış olduğunuz tüm uygulamalarda güncelleştirmeniz gerekir.
