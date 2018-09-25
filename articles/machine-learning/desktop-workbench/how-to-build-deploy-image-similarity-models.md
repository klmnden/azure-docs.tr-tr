---
title: Derleme ve Azure Machine Learning paket için görüntü işleme kullanan bir görüntü benzerlik model dağıtma.
description: Oluşturma, eğitme, sınama ve görüntü işleme için Azure Machine Learning paketi kullanan bilgisayar işleme görüntü benzerlik model dağıtma hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: netahw
author: nhaiby
ms.date: 05/20/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 371903e1ee080d2b98fd46ac4d6d9838416e1335
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46973431"
---
# <a name="build-and-deploy-image-similarity-models-with-azure-machine-learning"></a>Azure Machine Learning ile görüntü benzerlik modellerini Derleme ve dağıtma

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]


Bu makalede, Azure Machine Learning paket bilgisayar işleme (AMLPCV) için bir görüntü benzerlik model dağıtma eğitin ve test için nasıl kullanılacağını öğrenin. Bu paket ve ayrıntılı başvuru belgelerinde genel bakış için bkz: https://aka.ms/aml-packages/vision.

Bilgisayar işleme etki alanındaki sorunları çok sayıda görüntü benzerlik kullanarak çözülebilir. Bu sorunlar, soruları cevaplamak modeller oluşturma içerir:
+ _Bir nesne görüntüde var mı? Örneğin, "köpek", "araba", "gönderilen" vb._
+ _Hangi sınıfın göz Hastalık önem derecesi, bu hastanın retinal taramasıyla evinced?_

Oluşturma ve dağıtma AMLPCV ile bu model, aşağıdaki adımları izleyerek gidin:
1. Veri kümesi oluşturma
2. Görüntü Görselleştirme ve ek açıklama
3. Görüntü güçlendirme
4. Derin sinir ağı (DNN) modeli tanımı
5. Sınıflandırıcı eğitim
6. Değerlendirme ve görselleştirme
7. Web hizmeti dağıtımı
8. Web hizmeti yük testi

[CNTK](https://www.microsoft.com/cognitive-toolkit/) kullanılır derin öğrenme framework eğitim yerel olarak desteklenen GPU makinede gibi gerçekleştirilir ([derin öğrenme veri bilimi sanal makinesi](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview)), ve dağıtımı, Azure ML kullanıma hazır hale getirme CLI'yı kullanır.

## <a name="prerequisites"></a>Önkoşullar

1. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

1. Aşağıdaki hesapları ve uygulama kurmalı ve yüklü:
   - Bir Azure Machine Learning Denemesi hesabı 
   - Bir Azure Machine Learning Model Yönetimi hesabı
   - Azure Machine Learning Workbench'in yüklü olması

   Bu üç henüz oluşturduysanız veya yüklü izleyin [Azure Machine Learning hızlı ve Workbench'i yükleme](../desktop-workbench/quickstart-installation.md) makalesi. 

1. Görüntü işleme için Azure Machine Learning paketi yüklü olmalıdır. Bu paket anlatıldığı gibi yüklemeyi öğrenin https://aka.ms/aml-packages/vision.

## <a name="sample-data-and-notebook"></a>Örnek veriler ve not defteri

### <a name="get-the-jupyter-notebook"></a>Jupyter not defterini alma

Örneği çalıştırmak için Not defterini indirme kendiniz burada açıklanmıştır.

> [!div class="nextstepaction"]
> [Jupyter not defterini alma](https://aka.ms/aml-packages/vision/notebooks/object_detection)

### <a name="load-the-sample-data"></a>Örnek verileri yükleme

Aşağıdaki örnek, 63 yemek takımı görüntülerini oluşan bir veri kümesi kullanır. Her bir görüntü (bowl, Imagine cup, Çatal bıçak, blondan) dört farklı sınıflardan birine ait olarak etiketlenir. Bu örnekte görüntülerinin sayısını, böylece bu örnek yürütülebilir hızlı bir şekilde küçüktür. Uygulamada sınıfı başına en az 100 görüntüleri sağlanmalıdır. Tüm görüntüleri altındadır *".. /sample_data imgs_recycling / "*,"bowl"adlı alt dizinleri,"cup","Çatal bıçak"ve"blonu".

![Azure Machine Learning veri kümesi](media/how-to-build-deploy-image-classification-models/recycling_examples.jpg)

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning paketi hakkında daha fazla görüntü işleme için bu makalelerde öğrenin:

+ Bilgi edinmek için nasıl [Bu modelin doğruluğunu artırmak](how-to-improve-accuracy-for-computer-vision-models.md).

+ Okuma [genel bakış paketini](https://aka.ms/aml-packages/vision).

+ Keşfedin [başvuru belgeleri](https://docs.microsoft.com/python/api/overview/azure-machine-learning/computer-vision) bu paket için.

+ Hakkında bilgi edinin [diğer Azure Machine Learning Python paketleri](reference-python-package-overview.md).
