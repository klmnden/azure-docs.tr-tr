---
title: Bilgisayar görme (AMLPCV) ve Team veri bilimi işlemi (TDSP) için görüntü sınıflandırma Azure Machine Learning (AML) paketi ile | Microsoft Docs
description: Görüntü sınıflandırma TDSP (Takım veri bilimi işlemi) ve AMLPCV kullanımını açıklar
services: machine-learning, team-data-science-process
documentationcenter: ''
author: xibingao
manager: deguhath
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 18/05/2018
ms.author: xibingao
ms.openlocfilehash: cfb49f08a245fc08ba3fe78333e801ea2d358c4a
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="skin-cancer-image-classification-with-azure-machine-learning-aml-package-for-computer-vision-amlpcv-and-team-data-science-process-tdsp"></a>Dış Görünüm Kanseri görüntü sınıflandırma bilgisayar görme (AMLPCV) ve Team veri bilimi işlem (TDSP) Azure Machine Learning (AML) paketi ile

## <a name="introduction"></a>Giriş

Bu makalede nasıl kullanılacağını gösterir [Azure Machine Learning paket bilgisayar görme (AMLPCV) için](https://docs.microsoft.com/en-us/python/api/overview/azure-machine-learning/computer-vision?view=azure-ml-py-latest) eğitmek için test etme ve dağıtma bir **görüntü sınıflandırma** modeli. Örnek TDSP yapısı ve şablonlar kullanır [Azure Machine Learning çalışma ekranı](https://docs.microsoft.com/en-us/azure/machine-learning/service/quickstart-installation). Bu kılavuzda tam örnek sağlanır. Kullandığı [CNTK](https://www.microsoft.com/en-us/cognitive-toolkit/) framework öğrenme ve eğitim derin gerçekleştirilir gibi bir [veri bilimi VM](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview) GPU makine. Dağıtım Azure ML Operationalization CLI kullanır.

Birçok uygulama bilgisayar görme etki alanındaki görüntü sınıflandırma sorunları Çerçeveli. Bu soruları "olduğu gibi görüntüde mevcut bir nesne?" model oluşturmaya içerir (nesne olabilir *köpek*, *araba*, veya *sevk*) ve daha karmaşık "hangi sınıfın göz Hastalık önem derecesine sahip evinced bu hasta'nın retinal taramasıyla?" gibi sorular Görüntü sınıflandırma veri işleme ve modelleme ardışık düzen AMLPCV kolaylaştırır. 

## <a name="link-to-github-repository"></a>GitHub deponuza bağlayın
Örnek hakkındaki Özet belgelere burada sunuyoruz. Daha kapsamlı belgeler bulunabilir [GitHub site](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification).

## <a name="team-data-science-process-tdsp-walkthrough-with-amlpcv"></a>Takım veri bilimi işlem (TDSP) izlenecek AMLPCV ile

Bu kılavuzda kullanılır [takım veri bilimi işlem (TDSP)](https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/overview) yaşam döngüsü.

İzlenecek yol aşağıdaki yaşam döngüsü adımları kapsar:

### <a name="1-data-acquisionhttpsgithubcomazuremachinelearningsamples-amlvisionpackage-isicimageclassificationblobmastercode01dataacquisitionandunderstanding"></a>[1. Veri acquision](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification/blob/master/code/01_data_acquisition_and_understanding)
ISIC dataset görüntü sınıflandırma görev için kullanılır. ISIC (uluslararası kaplama Imaging işbirliği) partership eğitimle araştırmak ve melanoma mortality azaltmaya yardımcı olmak üzere dijital kaplama Imaging uygulamayı kolaylaştırmak için endüstri arasındaki olur. [ISIC arşiv](https://isic-archive.com/#images) etiketleri zararsız veya malignant 13. 000'den kaplama lesion görüntülerle içerir. Biz ISIC arşivinden görüntüleri örneği indirin.

### <a name="2-modelinghttpsgithubcomazuremachinelearningsamples-amlvisionpackage-isicimageclassificationblobmastercode02modeling"></a>[2. Modelleme](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification/blob/master/code/02_modeling)
Modelleme adımda, aşağıdaki alt adımlar gerçekleştirilir. 

<b>2.1 veri kümesi oluşturma</b><br>

AMLPCV bir veri kümesi nesnesi oluşturmak için yerel diskte bir kök dizin görüntülerinin sağlar. 

<b>2.2 Görselleştirme ve ek açıklama görüntü</b><br>

Veri kümesi nesnesi görüntülerinde görselleştirin ve bazı etiketlerin gerekirse düzeltin.

<b>2.3 görüntü augmentation'a</b><br>

Açıklanan dönüşümleri kullanarak bir veri kümesi nesnesi büyütmek [imgaug](https://github.com/aleju/imgaug) kitaplığı.

<b>2.4 DNN modeli tanımı</b><br>

Eğitim adımda kullanılan model mimarisi tanımlayın. Altı farklı başına eğitilmiş derin sinir ağı modelleri AMLPCV desteklenmektedir: AlexNet, Resnet 18, Resnet-34 ve Resnet-50, Resnet 101 ve Resnet 152.

<b>2.5 sınıflandırıcı eğitim</b><br>

Varsayılan veya özel parametreler sinir ağları eğitmek.

<b>2.6 değerlendirme ve görselleştirme</b><br>

Alt adım eğitilen model bağımsız sınama veri kümesi üzerinde performansını değerlendirmek için işlevsellik sağlar. Değerlendirme ölçümleri doğruluğu, duyarlık ve geri çağırma ve ROC eğrisi içerir.

Bu alt adımlar karşılık gelen Jupyter not defteri ayrıntılı açıklanmıştır. Biz de yönergeleri oranı, mini toplu iş boyutu ve daha fazla model performansını artırmak için çıkarma oranı öğrenme gibi parametreleri etkinleştirilmesinde sağlanır.

### <a name="3-deploymenthttpsgithubcomazuremachinelearningsamples-amlvisionpackage-isicimageclassificationblobmastercode03deployment"></a>[3. Dağıtım](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification/blob/master/code/03_deployment)

Bu adım modelleme adımdan üretilen modeli operationalizes. Operationalization önkoşulları ve Kurulum tanıtır. Son olarak, web hizmetinin tüketimi de açıklanmıştır. Bu öğreticide AMLPCV ile derin öğrenme modelleri oluşturabilir ve Azure modelinde faaliyete geçirmek bilgi edinebilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar
Daha fazla üzerinde belgeleri okuyun [Azure Machine Learning paket bilgisayar görme (AMLPCV) için](https://docs.microsoft.com/en-us/python/api/overview/azure-machine-learning/computer-vision?view=azure-ml-py-latest) ve [takım veri bilimi işlem (TDSP)](https://aka.ms/tdsp) başlamak için.


## <a name="references"></a>Başvurular

* [Azure Machine paket Learning bilgisayar görme (AMLPCV)](https://docs.microsoft.com/en-us/python/api/overview/azure-machine-learning/computer-vision?view=azure-ml-py-latest)
* [Azure Machine Learning Workbench](https://docs.microsoft.com/en-us/azure/machine-learning/service/quickstart-installation)
* [Veri bilimi VM](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview)

