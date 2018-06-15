---
title: Örnekler ve izlenecek yollar için veri bilimi sanal makine - Azure | Microsoft Docs
description: Örnekler ve izlenecek yollar için veri bilimi sanal makine.
keywords: Veri bilimi araçları, veri bilimi sanal makine, veri bilimi, linux veri bilimi için Araçlar
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
ms.date: 09/11/2017
ms.author: gokuma
ms.openlocfilehash: 3e3ee232b6342601e44d728148d32e70e6f3f00b
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31419985"
---
# <a name="samples-on-the-data-science-virtual-machines-dsvm"></a>Örnek veri bilimi sanal makinelerde (DSVM)

Jupyter not defterlerini ve Jupyter üzerinde dayalı olmayan bazı biçiminde dahil tam olarak çalışan genişletme örnekleri DSVMs gelir. Tıklayarak Jupyter erişebilirsiniz `Jupyter` Masaüstü veya uygulama menü çubuğunda simge.  
> [!NOTE]
> Başvurmak [erişim Jupyter](#access-jupyter) , DSVM Jupyter not defterlerini etkinleştirmek için bölüm.

## <a name="quick-reference-of-samples"></a>Hızlı Başvuru örnekleri
| Örnekleri kategorisi | Açıklama | Konumlar |
| ------------- | ------------- | ------------- |
| **R** dili  | İçinde örnekleri **R** Azure bulut verilerle bağlanma gibi senaryoları açıklayan depolar, açık kaynak R karşılaştırma ve Microsoft R & faaliyete geçirmeye yönelik modeller Microsoft R Server veya SQL Server üzerinde. <br/> [ekran görüntüsü](#r-language) | <br/>`~notebooks` <br/> <br/> `~samples/MicrosoftR` <br/> <br/> `~samples/RSqlDemo` <br/> <br/> `~samples/SQLRServices`<br/> <br/>|
| **Python** dili  | İçinde örnekleri **Python** Azure bulut veri depoları ile bağlanma ve bunlarla çalışma gibi senaryoları açıklayan **Azure Machine Learning**.  <br/> [ekran görüntüsü](#python-language) | <br/>`~notebooks` <br/><br/>|
| **Jale** dili  | İçinde örnek **Jale** derin Jale öğrenme, C ve Python Jale vb. çağırma Jale Plotting ayrıntı. <br/> [ekran görüntüsü](#julia-language) |<br/> **Windows**:<br/> `~notebooks/Julia_notebooks`<br/><br/>`~notebooks`<br/><br/> **Linux**:<br/> `~notebooks/julia`<br/><br/> |
| **CNTK** <br/> (Microsoft Bilişsel Araç Seti)  | Bilişsel Araç Seti ekibi Microsoft tarafından yayımlanan örnekleri öğrenme derin.  <br/> [ekran görüntüsü](#cntk) | <br/>**Windows**:<br/> `~notebooks/CNTK/Tutorials`<br/><br/>`~/samples/CNTK-Samples-2-0/Examples`<br/><br/> **Linux**:<br/> `~notebooks/CNTK`<br/> <br/>|
| **MXNet** dizüstü bilgisayarlar  | Derin kullanılarak örnekleri öğrenme **MXNet** sinir ağları tabanlı. Gelişmiş senaryolar için başlangıç değişen not defterlerini çeşitli vardır.  <br/> [ekran görüntüsü](#mxnet) | <br/>`~notebooks/mxnet`<br/> <br/>|
| **Azure Machine Learning** AzureML  | Etkileşimde **Azure Machine Learning** Studio ve bulut tabanlı Puanlama iş akışları için yerel olarak eğitilmiş modeller web hizmeti uç noktaları oluşturuluyor. <br/> [ekran görüntüsü](#azureml) | <br/>`~notebooks/azureml`<br/> <br/>|
| **Caffe2** | Derin kullanılarak örnekleri öğrenme **caffe2** sinir ağları tabanlı. Veri kümesi oluşturma, regresyon, önceden işlenmesi ve kullanarak görüntü eğitilmiş modeller önceden gibi örnekler dahil olmak üzere birkaç dizüstü kullanıcıları caffe2 ve etkili bir şekilde kullanma hakkında bilgi edinmek için tasarlanmış vardır. <br/> [ekran görüntüsü](#caffe2) | <br/>`~notebooks/caffe2`<br/><br/> |
| **H2O**   | Python tabanlı örnekleri kullanan **H2O** pek çok gerçek dünya senaryoları sorunları. <br/> [ekran görüntüsü](#h2o) | <br/>`~notebooks/h2o`<br/><br/> |
| **SparkML** dili  | Özellik ve yetenekler Spark ın kullanan örnek **Mllib'i** araç takımı aracılığıyla **pySpark 2.0** üzerinde **Apache Spark 2.0**.  <br/> [ekran görüntüsü](#sparkml) | <br/>`~notebooks/SparkML/pySpark`<br/><br/> |
| **MMLSpark** dili  | Örnekleri yararlanarak çeşitli **MMLSpark - Apache Spark Microsoft Machine Learning'de**, hangi birkaç derin öğrenme ve veri bilimi araçlar sağlayan bir çerçevedir **Apache Spark**. <br/> [ekran görüntüsü](#sparkml) | <br/>`~notebooks/MMLSpark`<br/><br/> |
| **TensorFlow**  | Birden çok farklı sinir ağı örnekleri ve teknikler kullanılarak uygulanan **TensorFlow** framework. <br/> [ekran görüntüsü](#tensorflow) | <br/>`~notebooks/tensorflow`<br/><br/> |
| **XGBoost** | Standart Machine Learning örnekleri **XGBoost** sınıflandırma gibi senaryolar için regresyon vs. <br/> [ekran görüntüsü](#xgboost) | <br/>`~samples/xgboost/demo`<br/><br/> |

<br/>

## <a name="access-jupyter"></a>Erişim Jupyter 

Giderek ziyaret Jupyter giriş **`https://localhost:9999`** Windows veya **`https://localhost:8000`** Ubuntu üzerinde.


### <a name="enabling-jupyter-access-from-browser"></a>Tarayıcıdan Jupyter erişimini etkinleştirme

**Windows DSVM**

Çalıştırma **`Jupyter SetPassword`** masaüstü kısayolu ve izleme kümesi/parolanızı Jupyter için sıfırlama ve Jupyter başlatmak için istemi işlem. 
<br/>![Jupyter özel durumunu etkinleştirin](./media/jupyter-setpassword.png)<br/>
Jupyter işlemi başarıyla ziyaret ederek de kendi VM'nizi başladıktan sonra Jupyter giriş erişebilirsiniz **`https://localhost:9999`** tarayıcınızda. Özel durum ekleyin ve tarayıcı Jupyter erişimi etkinleştirmek için ekran görüntüsüne bakın
<br/>![Jupyter özel durumunu etkinleştirin](./media/windows-jupyter-exception.png)<br/>
Ayarlamış olduğunuz yeni parolayla oturum açın.
<br/>
**Linux DSVM**

Ziyaret ederek de kendi VM'nizi Jupyter giriş erişebilirsiniz **`https://localhost:8000`** tarayıcınızda. Özel durum ekleyin ve tarayıcı Jupyter erişimi etkinleştirmek için ekran görüntüsüne bakın.
<br/>![Jupyter özel durumunu etkinleştirin](./media/ubuntu-jupyter-exception.png)<br/>
Aynı parola, oturum açma olarak DSVM için oturum açın.
<br/>

**Jupyter giriş**
<br/>![Jupyter giriş](./media/jupyter-home.png)<br/>

## <a name="r-language"></a>R dili 
<br/>![R örnekleri](./media/r-language-samples.png)<br/>

## <a name="python-language"></a>Python dili
<br/>![Python örnekleri](./media/python-language-samples.png)<br/>

## <a name="julia-language"></a>Jale dili 
<br/>![Jale örnekleri](./media/julia-samples.png)<br/>

## <a name="cntk"></a>CNTK 
<br/>![CNTK örnekleri](./media/cntk-samples2.png)<br/>
<br/>![CNTK örnekleri](./media/cntk-samples.png)<br/>

## <a name="mxnet"></a>MXNet
<br/>![MXnet örnekleri](./media/mxnet-samples.png)<br/>

## <a name="azureml"></a>AzureML 
<br/>![AzurekML örnekleri](./media/azureml-samples.png)<br/>

## <a name="caffe2"></a>Caffe2 
<br/>![caffe2 örnekleri](./media/caffe2-samples.png)<br/>

## <a name="h2o"></a>H2O 
<br/>![H2O örnekleri](./media/h2o-samples.png)<br/>

## <a name="sparkml"></a>SparkML 
<br/>![SparkML örnekleri](./media/sparkml-samples.png)<br/>

## <a name="tensorflow"></a>TensorFlow 
<br/>![TensorFlow örnekleri](./media/tensorflow-samples.png)<br/>

## <a name="xgboost"></a>XGBoost 
<br/>![XGBoost örnekleri](./media/xgboost-samples.png)<br/>

