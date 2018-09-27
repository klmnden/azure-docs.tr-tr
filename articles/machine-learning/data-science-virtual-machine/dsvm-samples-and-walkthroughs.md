---
title: Örnekler ve izlenecek yollar için veri bilimi sanal makinesi - Azure | Microsoft Docs
description: Örnekler ve izlenecek yollar için veri bilimi sanal makinesi.
keywords: veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi için araçlar, linux veri bilimi
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: gokuma
ms.openlocfilehash: a1f15b805d2f27152d9ba85608ce0dc1d1aac21e
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47392577"
---
# <a name="samples-on-the-data-science-virtual-machines-dsvm"></a>Veri bilimi sanal makineleri (DSVM) örnekleri

Dsvm'leri Jupyter not defterleri ve bunun yanı sıra Python ve r gibi dillerde betikleri şeklinde iki örnek kod kapsamlı bir dizi içerir    
> [!NOTE]
> Başvurmak [erişim Jupyter](#access-jupyter) bölümü Jupyter not defterleri DSVM üzerinde çalıştırma hakkında daha fazla bilgi için.

## <a name="quick-reference-of-samples"></a>Örnekler, hızlı başvuru
| Örnek kategorisi | Açıklama | Konumlar |
| ------------- | ------------- | ------------- |
| **R** dil  | İçinde örnekleri **R** ile Azure bulut verilere bağlanma gibi senaryoları açıklayan depolar, açık kaynak R ile karşılaştırma ve Microsoft R & faaliyete modeller Microsoft R Server veya SQL Server üzerinde. <br/> [Ekran görüntüsü](#r-language) | <br/>`~notebooks` <br/> <br/> `~samples/MicrosoftR` <br/> <br/> `~samples/RSqlDemo` <br/> <br/> `~samples/SQLRServices`<br/> <br/>|
| **Python** dil  | İçinde örnekleri **Python** Azure bulut veri depoları ile bağlanma ve bunlarla çalışma gibi senaryoları açıklayan **Azure Machine Learning**.  <br/> [Ekran görüntüsü](#python-language) | <br/>`~notebooks` <br/><br/>|
| **Julia** dil  | İçinde örnek **Julia** Plotting Julia, derin öğrenme Julia, C ve Python, Julia vb. ' çağırma içinde ayrıntılı. <br/> [Ekran görüntüsü](#julia-language) |<br/> **Windows**:<br/> `~notebooks/Julia_notebooks`<br/><br/> **Linux**:<br/> `~notebooks/julia`<br/><br/> |
| **Azure Machine Learning** AzureML  | ML ve derin öğrenme modelleri ile derleme **Azure Machine Learning** hizmeti ve herhangi bir yere dağıtma modelleri. ML, akıllı hyper parametresi ayarlama, model yönetimi, otomatik gibi yeteneklerden yararlanma eğitim dağıtılmış. <br/> [Ekran görüntüsü](#azureml) | <br/>`~notebooks/AzureML`<br/> <br/>|
| **PyTorch** Not Defterleri  | Örnekleri kullanarak derin öğrenme **PyTorch** sinir ağı tabanlı. Not defterlerini Acemi Gelişmiş senaryoları arasında değişen çeşitli vardır.  <br/> [Ekran görüntüsü](#pytorch) | <br/>`~notebooks/Deep_learning_frameworks/pytorch`<br/> <br/>|
| **TensorFlow**  | Birden çok farklı sinir ağı örneklerini ve tekniklerini kullanılarak uygulanan **TensorFlow** framework. <br/> [Ekran görüntüsü](#tensorflow) | <br/>`~notebooks/Deep_learning_frameworks/tensorflow`<br/><br/> |
| **CNTK** <br/> (Microsoft Bilişsel Araç Seti)  | Derin öğrenme Microsoft Bilişsel Araç Seti ekibi tarafından yayımlanan örnekleri.  <br/> [Ekran görüntüsü](#cntk) | <br/> `~notebooks/DeepLearningTools/CNTK/Tutorials`<br/><br/> **Linux**:<br/> `~notebooks/CNTK`<br/> <br/>|
| **Caffe2** | Örnekleri kullanarak derin öğrenme **caffe2** sinir ağı tabanlı. Görüntü, veri kümesi oluşturma, regresyon, ön işleme ve kullanarak modelleri önceden eğitilmiş gibi örnekler de dahil olmak üzere birkaç dizüstü kullanıcıları caffe2 ve etkili bir şekilde kullanma hakkında bilgi için tasarlanmış vardır. <br/> [Ekran görüntüsü](#caffe2) | <br/>`~notebooks/Deep_learning_frameworks/caffe2`<br/><br/> |
| **H2O**   | Python tabanlı örnekler yararlanarak **H2O** çok sayıda gerçek dünya senaryoları sorunları. <br/> [Ekran görüntüsü](#h2o) | <br/>`~notebooks/h2o`<br/><br/> |
| **SparkML** dil  | Spark'ın özellikleri kullanan örnek **MLlib** araç seti ile **pySpark** ve **MMLSpark - Apache Spark için Microsoft Machine Learning** üzerinde**Apache Spark 2.x**.  <br/> [Ekran görüntüsü](#sparkml) | <br/>`~notebooks/SparkML/pySpark`<br/>`~notebooks/MMLSpark`<br/><br/>  |
| **XGBoost** | Standart Machine Learning örnekleri **XGBoost** sınıflandırma gibi senaryolar için gerileme vs. <br/> [Ekran görüntüsü](#xgboost) | <br/>**Windows**:<br/>`\dsvm\samples\xgboost\demo`<br/><br/> |

<br/>

## <a name="access-jupyter"></a>Erişim Jupyter 

Jupyter tıklayarak erişebileceğiniz `Jupyter` Masaüstü veya uygulama menüsündeki simgesi. Ayrıca Jupyter Linux web tarayıcısından uzaktan DSVM sürümleri üzerinde ederek erişebileceğiniz **`https://<Full Domain Name or IP Address of the DSVM>:8000`** ubuntu'da.

Özel durum ekleyin ve tarayıcı Jupyter erişimi etkinleştirmek için ekran görüntüsüne bakın.


![Jupyter özel durumunu etkinleştirin](./media/ubuntu-jupyter-exception.png)


Oturum açma bilgilerinizi aynı parolaya DSVM için oturum açın.
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

