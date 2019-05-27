---
title: Çapraz platform çıkarımı ONNX ile yüksek performans
titleSuffix: Azure Machine Learning service
description: ONNX ve ONNX modelleri daha hızlı sunabilirsiniz için çalışma hakkında bilgi edinin
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: prasantp
author: prasanthpul
ms.date: 04/24/2019
ms.custom: seodec18
ms.openlocfilehash: f1eca5bdd81a384efe04f769ebd12be9d91fc78a
ms.sourcegitcommit: 16cb78a0766f9b3efbaf12426519ddab2774b815
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65849739"
---
# <a name="onnx-and-azure-machine-learning-create-and-accelerate-ml-models"></a>ONNX ve Azure Machine Learning: Oluşturma ve ML modelleri hızlandırın

Bilgi kullanıldığında nasıl [açık sinir ağı Exchange](https://onnx.ai) (ONNX) makine öğrenimi modelinizi çıkarımı en iyi duruma getirme yardımcı olabilir. Çıkarım veya Puanlama modeli, dağıtılan model için tahmin, üretim veri çubuğunda en yaygın olarak kullanıldığı aşamasıdır. 

Model ve donanım özelliklerinden en iyi hale getirmek için çıkarım kitaplığı ayarlamak gerekli olduğundan, en iyi duruma getirme makine öğrenimi modellerini çıkarımı (veya model Puanlama) zordur. Sorun (bulut/edge, CPU/GPU, vb.) platformları farklı türden en iyi performansı elde etmek istiyorsanız, bu yana her biri farklı özellikler ve özelliklerine sahip son derece zor hale geliyor. Modelleri çok çeşitli platformlarda çalıştırmak için gereken çerçeveleri çeşitli varsa karmaşıklığı artırır. Bu çok zaman çerçeveleri ve donanım tüm farklı birleşimlerini iyileştirmek için alır. Tercih edilen çerçeveniz içinde bir kez eğitmek ve buluta veya uca her yerde çalışan bir çözüm gereklidir. Bu, ONNX burada devreye girer.

Microsoft ve iş ortaklarından oluşan bir topluluk ONNX makine öğrenimi modelleri temsil etmek için açık bir standart olarak oluşturuldu. Gelen modeller [birçok](https://onnx.ai/supported-tools) TensorFlow, PyTorch, SciKit-öğrenme, Keras, bağlayıcı, MXNet ve MATLAB dahil aktarılacak veya standart ONNX biçimine dönüştürüldü. Modelleri ONNX biçiminde olduğunda, çeşitli platformlar ve cihazlar üzerinde çalıştırılabilir.

[ONNX çalışma zamanı](https://github.com/Microsoft/onnxruntime) ONNX modelleri üretime dağıtmak için bir yüksek performanslı çıkarımı alt yapısıdır. Hem bulut hem de edge için optimize edilmiştir ve Linux, Windows ve Mac üzerinde çalışır Yazılan C++, ayrıca C, Python, içerir ve C# API'leri. ONNX çalışma zamanı, tüm ONNX-ML'LİSTESİNE belirtimi için destek sağlar ve ayrıca TensorRT NVIDIA GPU'ları üzerinde gibi farklı donanımda Hızlandırıcıları ile tümleşir.

ONNX çalışma zamanı, Bing, Office ve Bilişsel hizmetler gibi büyük ölçekli Microsoft hizmetlerinde kullanılır. Bir dizi faktöre bağlı performans kazancı elde edildi, ancak bu Microsoft Hizmetleri gördünüz bir __ortalama CPU performans kazancı x 2__. ONNX çalışma zamanı, Windows ML yüz milyonlarca cihaz üzerinde bir parçası olarak da kullanılır. Azure Machine Learning Hizmetleri ile çalışma zamanı kullanabilirsiniz. ONNX çalışma zamanı'nı kullanarak üretim düzeyinde kapsamlı iyileştirmeleri, test ve sürekli geliştirmeler yararlı olabilir.

[![ONNX akış eğitim, dönüştürücüler ve dağıtım gösteren diyagram](media/concept-onnx/onnx.png)](./media/concept-onnx/onnx.png#lightbox)

## <a name="get-onnx-models"></a>ONNX modelleri Al

Çeşitli şekillerde ONNX modelleri elde edebilirsiniz:
+ Azure Machine Learning hizmetinde yeni bir ONNX model eğitip (Bu makalenin altındaki örneklere bakın)
+ Varolan modeli ONNX için başka bir biçimden Dönüştür (bkz [öğreticiler](https://github.com/onnx/tutorials)) 
+ Önceden eğitilmiş ONNX modelden alma [ONNX Model Zoo](https://github.com/onnx/models) (Bu makalenin altındaki örneklere bakın)
+ Özelleştirilmiş ONNX modelden üreten [Azure özel görüntü işleme hizmeti](https://docs.microsoft.com/azure/cognitive-services/Custom-Vision-Service/) 

Görüntü sınıflandırması, nesne algılama ve metin işleme gibi çok sayıda model ONNX modelleri olarak gösterilebilir. Ancak bazı modeller başarıyla dönüştürülmesi mümkün olmayabilir. Lütfen bu durumla karşılaşırsanız, sorunu kullandığınızla ilgili dönüştürücü GitHub üzerinden bildirin. Var olan biçimi modelinizi sorun çözümlenene kadar kullanmaya devam edebilirsiniz.

## <a name="deploy-onnx-models-in-azure"></a>Azure'da ONNX modelleri dağıtma

Azure Machine Learning hizmeti ile dağıtma, yönetme ve ONNX modellerinizle izleyin. Standart [dağıtımı iş akışı](concept-model-management-and-deployment.md) ve ONNX çalışma zamanı, bulutta barındırılan bir REST uç noktası oluşturabilirsiniz. Kendiniz denemek için bu makalenin sonunda örnek Jupyter Notebook bakın. 

### <a name="install-and-use-onnx-runtime-with-python"></a>Yükleme ve Python ile ONNX çalışma zamanını kullanma

Python paketlerini ONNX çalışma zamanı için kullanılabilir [PyPi.org](https://pypi.org) ([CPU](https://pypi.org/project/onnxruntime), [GPU](https://pypi.org/project/onnxruntime-gpu)). Lütfen okuma [sistem gereksinimleri](https://github.com/Microsoft/onnxruntime#system-requirements) yüklemeden önce. 

 Python için ONNX çalışma zamanı yüklemek için aşağıdaki komutlardan birini kullanın: 
```python   
pip install onnxruntime       # CPU build
pip install onnxruntime-gpu   # GPU build
``` 

Python betiğinizde ONNX çalışma zamanı çağırmak için kullanın:    
```python   
import onnxruntime  
session = onnxruntime.InferenceSession("path to model") 
``` 

Model genellikle eşlik eden belgeleri giriş ve çıkışları için modeli kullanarak söyler. Bir görselleştirme aracı gibi kullanabilirsiniz [Netron](https://github.com/lutzroeder/Netron) modeli görüntülemek için. ONNX çalışma zamanı ayrıca sorgu model meta verilerini, girdileri ve çıktıları sağlar:    
```python   
session.get_modelmeta() 
first_input_name = session.get_inputs()[0].name 
first_output_name = session.get_outputs()[0].name   
``` 

Çıkarım için kullanım modelinizi `run` ve döndürülen istediğiniz çıkışları (tümünün istiyorsanız bırakın boş) ve giriş değerleri haritasını listesinde geçirin. Sonuç çıktısı listesidir.  
```python   
results = session.run(["output1", "output2"], {"input1": indata1, "input2": indata2})   
results = session.run([], {"input1": indata1, "input2": indata2})   
``` 

Tam Python API Başvurusu için bkz. [ONNX çalışma zamanı başvuru belgeleri](https://aka.ms/onnxruntime-python).    

## <a name="examples"></a>Örnekler

Bkz: [Yardım-How-to-kullanın-azureml/dağıtım/onnx](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/onnx) örneğin ONNX modelleri oluşturup Not.

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="more-info"></a>Daha fazla bilgi

ONNX hakkında daha fazla bilgi edinin veya projeye katkıda:
+ [ONNX proje Web sitesi](https://onnx.ai)
+ [Github'da ONNX kod](https://github.com/onnx/onnx)

ONNX çalışma zamanı hakkında daha fazla bilgi edinin veya projeye katkıda:
+ [ONNX çalışma zamanı GitHub deposu](https://github.com/Microsoft/onnxruntime)


