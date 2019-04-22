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
ms.openlocfilehash: 6fac6531ea0a39796de13f95aee33b30dc91f131
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59274459"
---
# <a name="how-to-move-your-limited-trial-project-to-azure"></a>Sınırlı deneme sürümünde projenizi Azure'a taşıma

Özel görüntü işleme hizmeti, azure'a geçiş tamamlandığında Azure dışındaki sınırlı deneme sürümünde projeleri için destek sona eriyor. Bu belge için bir Azure kaynağı sınırlı deneme sürümünde projenize kopyalamak için özel görüntü işleme API'leri kullanmayı gösterir.

Sınırlı deneme sürümünde projelerin görüntüleme desteği [Custom Vision Web sitesi](https://customvision.ai) 25 Mart 2019 sonlandı. Bu belgeyi artık, özel görüntü işleme API'leriyle kullanma işlemi gösterilmektedir bir [geçiş python betiğini](https://github.com/Azure-Samples/custom-vision-move-project) github'da) projenizi bir Azure kaynağı için çoğaltma.

Sınırlı deneme kullanımdan kaldırma işleminde anahtar teslim tarihlerine dahil olmak üzere daha fazla ayrıntı için lütfen bkz. [sürüm notları](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/release-notes#february-25-2019) veya sınırlı deneme projeleri sahiplerine gönderilen e-posta iletişimi.

[Geçiş öncesinde bir betik](https://github.com/Azure-Samples/custom-vision-move-project) , geçerli yineleme içinde görüntüler ve indiriliyor ve tüm etiketleri, bölgeleri, karşıya bir projeyi yeniden olanak sağlar. Bu, ardından eğitebilirsiniz yeni aboneliğiniz yeni bir proje ile bırakır.

## <a name="prerequisites"></a>Önkoşullar

- Microsoft hesabı veya oturum açmak için kullanmak istediğiniz Azure Active Directory (AAD) hesabı ile ilişkili geçerli bir Azure aboneliğine ihtiyacınız olacak [Custom Vision Web sitesi](https://customvision.ai). 
    - Azure hesabınız yoksa [hesap oluşturma](https://azure.microsoft.com/free/) ücretsiz.
    - Abonelikler ve kaynaklar Azure kavramlarına giriş için bkz [Azure Geliştirici kılavuzuyla.](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#manage-your-subscriptions).
-  [Python](https://www.python.org/downloads/)
- [pip](https://pip.pypa.io/en/stable/installing/)

## <a name="create-custom-vision-resources-in-the-azure-portal"></a>Azure portalında özel görüntü işleme kaynakları oluşturma

Azure ile özel görüntü işleme hizmeti kullanmak için Custom Vision eğitim ve tahmin kaynaklarında oluşturmanız gerekecektir [Azure portalında](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=microsoft_azure_cognitiveservices_customvision#create/Microsoft.CognitiveServicesCustomVision). 

Birden çok proje için tek bir kaynak ilişkilendirilebilir. Hakkında daha fazla ayrıntı [fiyatlandırma ve limitler](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/limits-and-quotas) kullanılabilir. Özel görüntü işleme hizmeti ücretsiz olarak kullanmaya devam etmek için Azure portalında F0 katmanı seçebilirsiniz. 

> [!NOTE]
> Custom Vision projeniz için bir Azure kaynağı taşıdığınızda, temel alınan devralan [izinleri]( https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) ilgili Azure kaynağının. Kuruluşunuzdaki diğer kullanıcılara projenize konusu Azure kaynak sahipleri, bunlar üzerinde projenize erişmek mümkün olacaktır [Custom Vision Web sitesi](https://customvision.ai). Benzer şekilde, kaynaklarınızı silindiğinde, projelerinizi silinir.  

## <a name="find-your-limited-trial-project-information"></a>Sınırlı deneme proje bilgilerinizin

Projenizi taşımak için ihtiyacınız olacak _kimliği proje_ ve _eğitim anahtar_ geçirmeye çalıştığınız proje için. Bu bilgiler yoksa, ziyaret [ https://limitedtrial.customvision.ai/projects ](https://limitedtrial.customvision.ai/projects) kimliği elde edilir ve her projeleriniz için anahtar. 

## <a name="use-the-python-sample-code-to-copy-your-project-to-azure"></a>Azure'a projenize kopyalamak için Python örnek kodu kullanın

İzleyin [örnek kodu yönergeleri](https://github.com/Azure-Samples/custom-vision-move-project), sınırlı deneme anahtarınızı kullanarak ve proje kimliği "kaynak" malzemeler ve anahtar "hedef" olarak oluşturulan yeni Azure kaynağı olarak.

Varsayılan olarak, tüm sınırlı deneme sürümünde projeleri, Güney Orta ABD Azure bölgesi içinde barındırılır.

## <a name="next-steps"></a>Sonraki adımlar

Projenizi şimdi bir Azure kaynağına taşındı. Eğitim ve tahmin anahtarlarınızı yazmış olduğunuz tüm uygulamalarda güncelleştirmeniz gerekir.

Projenizi görüntülemek için [Custom Vision Web sitesi](https://customvision.ai), Azure portalında oturum açmak için kullandığınız hesapla oturum açın. Projenizi görmüyorsanız, aynı dizinde bulunan onaylayın [Custom Vision Web sitesi](https://customvision.ai) kaynaklarınızı Azure Portalı'nda bulunduğu yeri dizini olarak. Hem Azure portalı ve CustomVision.ai, ekranın sağ üst köşedeki aşağı açılan kullanıcı menüden dizininizi seçin.