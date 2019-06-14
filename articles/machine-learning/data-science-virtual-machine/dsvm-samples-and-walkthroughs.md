---
title: Örnekler ve izlenecek yollar için veri bilimi sanal makineleri - Azure | Microsoft Docs
description: Örnekler ve ortak görevler ve senaryolar ile veri bilimi sanal makinesi yerine getirmeyi öğretin izlenecek yollar hakkında bilgi edinin.
keywords: veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi için araçlar, linux veri bilimi
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.custom: seodec18
ms.assetid: ''
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: gokuma
ms.openlocfilehash: e61f0f4ba30b29fea1b2fd5f2a2ab253d3a6710c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60502918"
---
# <a name="samples-on-data-science-virtual-machines"></a>Veri bilimi sanal makineleri üzerinde örnekleri

Örnek kod kapsamlı Azure veri bilimi sanal makineleri içerir. Jupyter Not defterlerinden ve betikler Python ve r gibi dillerde biçiminde örnek kodu. 
> [!NOTE]
> Jupyter not defterleri, veri bilimi sanal makinelerde çalıştırma hakkında daha fazla bilgi için bkz. [erişim Jupyter](#access-jupyter) bölümü.

## <a name="quick-reference-of-samples"></a>Örnekler, hızlı başvuru
| Örnek kategorisi | Açıklama | Konumlar |
| ------------- | ------------- | ------------- |
| R dili  | R örnekleri, Azure bulut veri depoları ile bağlanma gibi senaryolar açıklanmaktadır. Bunlar aynı zamanda açık kaynak R ile Microsoft R. karşılaştırma yapılacağı açıklanmaktadır Ve bunlar üzerindeki Microsoft R Server veya SQL Server Modellerinizi nasıl faaliyete geçireceğinizi açıklar. <br/> [R dili](#r-language) | <br/>`~notebooks` <br/> <br/> `~samples/MicrosoftR` <br/> <br/> `~samples/RSqlDemo` <br/> <br/> `~samples/SQLRServices`<br/> <br/>|
| Python dil  | Python örnekleri, Azure bulut veri depoları ile bağlanma ve Azure Machine Learning ile çalışmak nasıl gibi senaryolar açıklanmaktadır.  <br/> [Python dil](#python-language) | <br/>`~notebooks` <br/><br/>|
| Julia diline  | Çizim ve derin öğrenme Julia ayrıntıları Julia örnek. Bu ayrıca arama C ve Python Julia'dan açıklar. <br/> [Julia diline](#julia-language) |<br/> Windows:<br/> `~notebooks/Julia_notebooks`<br/><br/> Linux:<br/> `~notebooks/julia`<br/><br/> |
| Azure Machine Learning  | Makine öğrenimi ve derin öğrenme ile Machine Learning modelleri oluşturun. Modelleri her yerde dağıtın. Otomatik makine öğrenimi ve akıllı hiper parametre ayarı kullanın. Model yönetimi ve dağıtılmış eğitimi de kullanır. <br/> [Machine Learning](#azureml) | <br/>`~notebooks/AzureML`<br/> <br/>|
| PyTorch Not Defterleri  | Derin sinir ağları PyTorch tabanlı kullanma örnekleri öğrenme. Başlangıç not defterlerini aralığından Gelişmiş senaryolar için.  <br/> [PyTorch Not Defterleri](#pytorch) | <br/>`~notebooks/Deep_learning_frameworks/pytorch`<br/> <br/>|
| TensorFlow  |  Farklı bir sinir ağı örnekleri ve teknikleri TensorFlow çerçevesi kullanılarak uygulanır. <br/> [TensorFlow](#tensorflow) | <br/>`~notebooks/Deep_learning_frameworks/tensorflow`<br/><br/> |
| Microsoft Bilişsel Araç Seti <br/>   | Derin öğrenme Microsoft Bilişsel Araç Seti ekibi tarafından yayımlanan örnekleri.  <br/> [Bilişsel Araç Seti](#cntk) | <br/> `~notebooks/DeepLearningTools/CNTK/Tutorials`<br/><br/> Linux:<br/> `~notebooks/CNTK`<br/> <br/>|
| Caffe2 | Derin sinir ağları caffe2 tabanlı kullanma örnekleri öğrenme. Birkaç not defterlerini kullanıcılar caffe2 ile nasıl etkin kullanılacağı hakkında bilgi edinin. Görüntü önceden işleme ve veri kümesi oluşturma örneklerindendir. Bunlar ayrıca regresyon ve önceden eğitilmiş modeller kullanmayı içerir. <br/> [Caffe2](#caffe2) | <br/>`~notebooks/Deep_learning_frameworks/caffe2`<br/><br/> |
| H2O   | Gerçek dünya senaryoları sorunlar için H2O kullanın Python tabanlı örnekler. <br/> [H2O](#h2o) | <br/>`~notebooks/h2o`<br/><br/> |
| SparkML dil  | PySpark ve MMLSpark--Apache Spark üzerinde Apache Spark için Microsoft Machine Learning ile Spark MLLib toolkit özelliklerini kullanma örnekleri 2.x.  <br/> [SparkML dil](#sparkml) | <br/>`~notebooks/SparkML/pySpark`<br/>`~notebooks/MMLSpark`<br/><br/>  |
| XGBoost | Standart makine öğrenimi XGBoost örnekleri sınıflandırma ve regresyon gibi senaryolar için. <br/> [XGBoost](#xgboost) | <br/>Windows:<br/>`\dsvm\samples\xgboost\demo`<br/><br/> |

<br/>

## <a name="access-jupyter"></a>Erişim Jupyter 

Jupyter erişmek için seçin `Jupyter` Masaüstü veya uygulama menüsündeki simgesi. Veri bilimi sanal makineleri sürümlerinde Linux Jupyter da erişebilirsiniz. Bir web tarayıcısından ziyaret ederek uzaktan erişebilirsiniz `https://<Full Domain Name or IP Address of the DSVM>:8000` ubuntu'da.

Özel durumlar ekleyip bir tarayıcı erişim Jupyter kullanılabilir hale getirmek için aşağıdaki ekran görüntüsüne bakın.


![Jupyter özel durumunu etkinleştirin](./media/ubuntu-jupyter-exception.png)


Oturum açma bilgilerinizi aynı parolaya veri bilimi sanal makineleri için oturum açın.
<br/>

**Jupyter giriş**
<br/>![Jupyter giriş](./media/jupyter-home.png)<br/>

## <a name="r-language"></a>R dili 
<br/>![R örnekleri](./media/r-language-samples.png)<br/>

## <a name="python-language"></a>Python dil
<br/>![Python örnekleri](./media/python-language-samples.png)<br/>

## <a name="julia-language"></a>Julia diline 
<br/>![Julia örnekleri](./media/julia-samples.png)<br/>

## <a name="azureml"></a>AzureML 
<br/>![AzurekML örnekleri](./media/azureml-samples.png)<br/>

## <a name="pytorch"></a>PyTorch
<br/>![PyTorch örnekleri](./media/pytorch-samples.png)<br/>

## <a name="tensorflow"></a>TensorFlow 
<br/>![TensorFlow örnekleri](./media/tensorflow-samples.png)<br/>


## <a name="cntk"></a>CNTK 
<br/>![CNTK örnekleri](./media/cntk-samples.png)<br/>


## <a name="caffe2"></a>Caffe2 
<br/>![caffe2 örnekleri](./media/caffe2-samples.png)<br/>

## <a name="h2o"></a>H2O 
<br/>![H2O örnekleri](./media/h2o-samples.png)<br/>

## <a name="sparkml"></a>SparkML 
<br/>![SparkML örnekleri](./media/sparkml-samples.png)<br/>

## <a name="xgboost"></a>XGBoost 
<br/>![XGBoost örnekleri](./media/xgboost-samples.png)<br/>

