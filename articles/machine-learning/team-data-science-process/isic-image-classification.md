---
title: Görüntü işleme ve Team Data Science işlem (TDSP) Azure Machine Learning paketi ile görüntü sınıflandırma | Microsoft Docs
description: Team Data Science işlem (TDSP) ve Azure Machine Learning paketi görüntü sınıflandırması için görüntü işleme için kullanımını açıklar.
services: machine-learning, team-data-science-process
documentationcenter: ''
author: deguhath
ms.author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2018
ms.openlocfilehash: ee2e797f3838b8b6b36174d14c73e97fe9790315
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49392821"
---
# <a name="skin-cancer-image-classification-with-the-azure-machine-learning-package-for-computer-vision-and-team-data-science-process"></a>Dış Görünüm kanser görüntü işleme ve Team Data Science Process için Azure Machine Learning paketi ile görüntü sınıflandırma

Bu makalede nasıl kullanılacağını gösterir [görüntü işleme için Azure Machine Learning paketi](https://docs.microsoft.com/python/api/overview/azure-machine-learning/computer-vision?view=azure-ml-py-latest) eğitmek için test etme ve dağıtma bir *görüntü sınıflandırma* modeli. Örnek şablonları ve Team Data Science işlem (TDSP) yapısını kullanır [Azure Machine Learning Workbench](https://docs.microsoft.com/azure/machine-learning/service/quickstart-installation). Bu izlenecek yol, tam bir örnek sağlar. Kullandığı [Microsoft Bilişsel Araç Seti](https://www.microsoft.com/en-us/cognitive-toolkit/) framework öğrenme ve eğitim derin gerçekleştirilir gibi bir [veri bilimi sanal makinesi](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview) GPU makine. Azure Machine Learning operasyonel hale getirme CLI dağıtımı kullanır.

Bilgisayar işleme etki alanındaki birçok uygulama, görüntü sınıflandırma sorunları Çerçeveli. Bunlar gibi basit sorular yanıt "görüntüde mevcut bir nesne olduğunu?" modeller oluşturma Burada nesne köpek, araba ve sevk olabilir. Ayrıca daha karmaşık soruların yanıtlarını gibi içerir "göz Hastalık önem derecesi hangi sınıfı bu hastanın retinal taramasıyla evinced?" Görüntü işleme için Azure Machine Learning paketi görüntü sınıflandırma veri işleme ve modelleme işlem hattını basitleştirir. 

## <a name="link-to-the-github-repository"></a>GitHub deponuza bağlayın
Bu makalede, örnek hakkında özet bir belgedir. Daha kapsamlı belgeler bulabilirsiniz [GitHub site](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification).

## <a name="team-data-science-process-walkthrough"></a>Team Data Science Process Kılavuzu

Bu izlenecek yolda [Team Data Science Process](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/overview) yaşam döngüsü. İzlenecek yol aşağıdaki yaşam döngüsü adımları kapsar.

### <a name="1-data-acquisitionhttpsgithubcomazuremachinelearningsamples-amlvisionpackage-isicimageclassificationblobmastercode01dataacquisitionandunderstanding"></a>[1. Veri alma](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification/blob/master/code/01_data_acquisition_and_understanding)
Uluslararası kaplama Imaging işbirliği (ISIC) veri kümesi, görüntü sınıflandırma görev için kullanılır. ISIC eğitimle incelemek ve melanoma mortality azaltmaya yardımcı olmak için görüntüleme dijital dış uygulamayı kolaylaştırmak için sektör arasındaki iş ortaklığı var. [ISIC arşiv](https://isic-archive.com/#images) olarak zararsız ya da malignant etiketli 13. 000'den fazla kaplama lesion görüntüleri içerir. Bir örnek görüntü ISIC arşivden indirin.

### <a name="2-modelinghttpsgithubcomazuremachinelearningsamples-amlvisionpackage-isicimageclassificationblobmastercode02modeling"></a>[2. Modelleme](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification/blob/master/code/02_modeling)
Aşağıdaki alt adımların modelleme adımda gerçekleştirilir.

#### <a name="dataset-creation"></a>Veri kümesi oluşturma

Görüntü işleme için Azure Machine Learning paketi yerel diskte görüntülerin bir kök dizin sağlayarak bir veri kümesi nesnesi oluşturur. 

#### <a name="image-visualization-and-annotation"></a>Görüntü Görselleştirme ve ek açıklama

Veri kümesi nesne görüntülerdeki görselleştirin ve etiketleri gerektiği şekilde düzeltin.

#### <a name="image-augmentation"></a>Görüntü güçlendirme

Açıklanan dönüştürmeleri kullanarak bir veri kümesi nesnesi büyütmek [imgaug](https://github.com/aleju/imgaug) kitaplığı.

#### <a name="dnn-model-definition"></a>DNN model tanımı

Kullanılan modeli mimarisi eğitim adımda tanımlayın. Altı önceden eğitilmiş derin sinir ağı modelleri, görüntü işleme için Azure Machine Learning paketindeki desteklenmektedir: AlexNet, Resnet-18, Resnet-34, Resnet-50, Resnet-101 ve Resnet-152 azaldığını göstermektedir.

#### <a name="classifier-training"></a>Sınıflandırıcı eğitim

Varsayılan veya özel parametreler sinir ağlarıyla eğitin.

#### <a name="evaluation-and-visualization"></a>Değerlendirme ve görselleştirme

Bir bağımsız test veri kümesini eğitilen model performansını değerlendirmek için işlevsellik sağlar. Doğruluk, duyarlık geri çekme ve ROC eğrisi değerlendirme ölçümleri içerir.

Alt adımların karşılık gelen Jupyter not defterine ayrıntılı açıklanmıştır. Not defterini öğrenme oranı, mini toplu iş boyutu ve daha fazla model performansını artırmak için düşme oranı gibi parametreleri kapatmak için yönergeler de sağlar.

### <a name="3-deploymenthttpsgithubcomazuremachinelearningsamples-amlvisionpackage-isicimageclassificationblobmastercode03deployment"></a>[3. Dağıtım](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification/blob/master/code/03_deployment)

Bu adım modelleme adımda oluşturulan model operationalizes. Bu Önkoşullar ve Kurulum sunar. Web hizmeti kullanımını da açıklanmıştır. Bu öğreticide, görüntü işleme için Azure Machine Learning paketi ile ayrıntılı öğrenme modelleri oluşturup azure'da modeli hazır hale getirmek öğrenin.

## <a name="next-steps"></a>Sonraki adımlar
- Ek belgeleri okuyun [görüntü işleme için Azure Machine Learning paketi](https://docs.microsoft.com/python/api/overview/azure-machine-learning/computer-vision?view=azure-ml-py-latest).
- Okuma [Team Data Science Process](https://aka.ms/tdsp) kullanmaya başlamak için belgeler.


## <a name="references"></a>Başvurular

* [Görüntü işleme için Azure Machine Learning paketi](https://docs.microsoft.com/python/api/overview/azure-machine-learning/computer-vision?view=azure-ml-py-latest)
* [Azure Machine Learning Workbench](https://docs.microsoft.com/azure/machine-learning/service/quickstart-installation)
* [Veri bilimi sanal makinesi](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview)

