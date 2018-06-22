---
title: Görüntü sınıflandırma bilgisayar görme ve takım veri bilimi işlem (TDSP) için Azure Machine Learning paketi ile | Microsoft Docs
description: Bilgisayar görme görüntü sınıflandırma için takım veri bilimi işlem (TDSP) ve Azure Machine Learning paket kullanımını açıklar.
services: machine-learning, team-data-science-process
documentationcenter: ''
author: xibingao
manager: deguhath
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2018
ms.author: xibingao
ms.openlocfilehash: f9e88cfb7185845e96f287b39bebaaa24320f537
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36300817"
---
# <a name="skin-cancer-image-classification-with-the-azure-machine-learning-package-for-computer-vision-and-team-data-science-process"></a>Kanseri görüntü sınıflandırma bilgisayar görme ve takım veri bilimi işlemi Azure Machine Learning paketi ile dış görünüm

Bu makalede nasıl kullanılacağı gösterilmektedir [bilgisayar görme için Azure Machine Learning paketi](https://docs.microsoft.com/en-us/python/api/overview/azure-machine-learning/computer-vision?view=azure-ml-py-latest) eğitmek için test etme ve dağıtma bir *görüntü sınıflandırma* modeli. Örnek takım veri bilimi işlem (TDSP) yapısı ve şablonlar kullanır [Azure Machine Learning çalışma ekranı](https://docs.microsoft.com/en-us/azure/machine-learning/service/quickstart-installation). Bu kılavuz, tam bir örnek sağlar. Kullandığı [Microsoft Bilişsel Araç Seti](https://www.microsoft.com/en-us/cognitive-toolkit/) framework öğrenme ve eğitim derin gerçekleştirilir gibi bir [veri bilimi sanal makine](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview) GPU makine. Dağıtım CLI Azure Machine Learning operationalization kullanır.

Birçok uygulama bilgisayar görme etki alanındaki görüntü sınıflandırma sorunları Çerçeveli. Bunlar gibi basit soruları yanıt "nesneyi görüntüde mevcut olduğunu?" model oluşturmaya içerir Burada nesne köpek, car veya sevk olabilir. Ayrıca daha karmaşık soruların yanıtlarını gibi içerir "hangi sınıfın göz Hastalık önem derecesine göre bu hasta'nın retinal tarama evinced?" Bilgisayar görme için Azure Machine Learning paketi görüntü sınıflandırma veri işleme ve modelleme ardışık düzen kolaylaştırır. 

## <a name="link-to-the-github-repository"></a>GitHub depo bağlantı
Bu makalede, örnek hakkında özet bir belgedir. Daha kapsamlı belgeler bulabilirsiniz [GitHub site](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification).

## <a name="team-data-science-process-walkthrough"></a>Takım veri bilimi işlemi gözden geçirme

Bu kılavuzda kullanılır [takım veri bilimi işlemi](https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/overview) yaşam döngüsü. İzlenecek yol aşağıdaki yaşam döngüsü adımları kapsar.

### <a name="1-data-acquisitionhttpsgithubcomazuremachinelearningsamples-amlvisionpackage-isicimageclassificationblobmastercode01dataacquisitionandunderstanding"></a>[1. Veri alma](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification/blob/master/code/01_data_acquisition_and_understanding)
Uluslararası kaplama Imaging işbirliği (ISIC) veri kümesi görüntü sınıflandırma görevi için kullanılır. Eğitimle araştırmak ve melanoma mortality azaltmaya yardımcı olmak için Imaging dijital kaplama uygulamayı kolaylaştırmak için endüstri arasındaki iş ortaklığına ISIC. [ISIC arşiv](https://isic-archive.com/#images) olarak zararsız ya da malignant etiketli 13. 000'den fazla kaplama lesion görüntülerini içerir. Görüntüleri örneği ISIC arşivinden indirin.

### <a name="2-modelinghttpsgithubcomazuremachinelearningsamples-amlvisionpackage-isicimageclassificationblobmastercode02modeling"></a>[2. Modelleme](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification/blob/master/code/02_modeling)
Modelleme adımda, aşağıdaki alt adımlar gerçekleştirilir.

#### <a name="dataset-creation"></a>Veri kümesi oluşturma

Yerel disk üzerindeki bir kök dizin görüntülerinin sağlayarak bilgisayar görme için Azure Machine Learning paketinde bulunan bir veri kümesi nesnesi oluşturur. 

#### <a name="image-visualization-and-annotation"></a>Görüntü Görselleştirme ve ek açıklaması

Veri kümesi nesnesi görüntülerinde görselleştirmek ve etiketleri gerektiği şekilde düzeltin.

#### <a name="image-augmentation"></a>Görüntü augmentation'a

Açıklanan dönüşümleri kullanarak bir veri kümesi nesnesi büyütmek [imgaug](https://github.com/aleju/imgaug) kitaplığı.

#### <a name="dnn-model-definition"></a>DNN modeli tanımı

Kullanılan model mimarisi eğitim adımda tanımlayın. Altı önceden eğitilen derin sinir ağı modelleri bilgisayar görme için Azure Machine Learning paketindeki desteklenmektedir: AlexNet, Resnet 18, Resnet 34, Resnet-50, Resnet 101 ve Resnet 152.

#### <a name="classifier-training"></a>Sınıflandırıcı eğitim

Varsayılan veya özel parametreler sinir ağları eğitmek.

#### <a name="evaluation-and-visualization"></a>Değerlendirme ve görselleştirme

Bir bağımsız sınama veri kümesi üzerinde eğitilen model performansını değerlendirmek için işlevsellik sağlar. Değerlendirme ölçümleri doğruluğu, duyarlık geri çekme ve ROC eğrisi içerir.

Alt adımlar karşılık gelen Jupyter not defteri ayrıntılı açıklanmıştır. Not Defteri öğrenme oranı, mini toplu iş boyutu ve daha fazla model performansını artırmak için çıkarma oranı gibi parametreleri etkinleştirilmesinde yönergeleri de sağlar.

### <a name="3-deploymenthttpsgithubcomazuremachinelearningsamples-amlvisionpackage-isicimageclassificationblobmastercode03deployment"></a>[3. Dağıtım](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification/blob/master/code/03_deployment)

Bu adım modelleme adımdan üretilen modeli operationalizes. Önkoşullar ve gerekli Kurulumu tanıtır. Web hizmeti tüketiminin de açıklanmıştır. Bu öğreticide, Azure Machine Learning paket bilgisayar görme için derin öğrenme modelleri oluşturabilir ve Azure modelinde faaliyete öğrenin.

## <a name="next-steps"></a>Sonraki adımlar
- Hakkında ek belgeleri okuyun [bilgisayar görme için Azure Machine Learning paketi](https://docs.microsoft.com/en-us/python/api/overview/azure-machine-learning/computer-vision?view=azure-ml-py-latest).
- Okuma [takım veri bilimi işlemi](https://aka.ms/tdsp) başlamak için belgeleri.


## <a name="references"></a>Başvurular

* [Azure Machine Learning paketi için bilgisayar görme](https://docs.microsoft.com/en-us/python/api/overview/azure-machine-learning/computer-vision?view=azure-ml-py-latest)
* [Azure Machine Learning Workbench](https://docs.microsoft.com/en-us/azure/machine-learning/service/quickstart-installation)
* [Veri bilimi sanal makine](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview)

