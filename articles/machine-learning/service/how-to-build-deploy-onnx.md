---
title: Oluşturma ve birlikte çalışabilen ONNX modelleri dağıtma
titleSuffix: Azure Machine Learning service
description: ONNX ve Azure Machine Learning ONNX modelleri oluşturup kullanma hakkında bilgi edinin
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: prasantp
author: prasanthpul
ms.date: 12/3/2018
ms.custom: seodec18
ms.openlocfilehash: 716286fdfb684a277c7147936f162089a2cdefd8
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62118475"
---
# <a name="onnx-and-azure-machine-learning-create-and-deploy-interoperable-ai-models"></a>ONNX ve Azure Machine Learning: Oluşturma ve birlikte çalışabilen yapay ZEKA modelleri dağıtma

[Açın sinir ağı Exchange](https://onnx.ai) (ONNX) biçimidir açık bir standart machine learning modellerini temsil etmek için. ONNX tarafından desteklenen bir [iş ortaklarının topluluk](https://onnx.ai/supported-tools), uyumlu çerçeveleri ve araçları oluşturan Microsoft, dahil. Böylece veri bilimcileri ve geliştiricileri için Microsoft açık ve birlikte çalışabilen yapay ZEKA taahhüt etmektedir:

+ Oluşturma ve eğitme modelleri için kendi seçtikleri çerçevesini kullanın
+ Modelleri platformlar en az bir tümleştirme çalışma ile dağıtma

Microsoft ürünlerinden dahil olmak üzere bu hedefleri gerçekleştirmeye yardımcı olmak üzere, Azure ile Windows arasında ONNX destekler.  

## <a name="why-choose-onnx"></a>ONNX neden seçmeliyim?

Birlikte çalışabilirlik ile ONNX alma harika fikirler üretime daha hızlı erişim sağlamak mümkün kılar. ONNX ile kendi tercih edilen framework iş için veri bilimcilerine seçebilirsiniz. Benzer şekilde, geliştiriciler, üretim için modelleri hazırlanıyor daha az zaman harcayın ve Bulut ve uç arasında dağıtın.  

PyTorch, bağlayıcı, MXNet, ML.Net, TensorFlow, Keras, SciKit-öğrenme, Microsoft Bilişsel Araç Seti ve daha fazlası dahil olmak üzere birçok çerçevelerinden ONNX modelleri oluşturabilirsiniz.

Bir kaynak ekosisteminiz ONNX modelleri hızlandırma ve görselleştirme araçları yoktur. Bir dizi önceden eğitilmiş ONNX modelleri, ayrıca yaygın senaryoları için kullanılabilir.

[ONNX modelleri dağıtılabilir](#deploy) Azure Machine Learning ve ONNX çalışma zamanı'nı kullanarak bulutta. Kullanarak Windows 10 cihazlarına da dağıtılabilir [Windows ML](https://docs.microsoft.com/windows/ai/). Bunlar bile ONNX Topluluğu'ndan kullanılabilen dönüştürücüleri kullanarak diğer platformlar için dağıtılabilir. 

[![ONNX akış eğitim, dönüştürücüler ve dağıtım gösteren diyagram](media/concept-onnx/onnx.png)](./media/concept-onnx/onnx.png#lightbox)

## <a name="get-onnx-models"></a>ONNX modelleri Al

Çeşitli şekillerde ONNX modelleri elde edebilirsiniz:
+ Önceden eğitilmiş ONNX modelden alma [ONNX Model Zoo](https://github.com/onnx/models) (Bu makalenin altındaki örneğe bakın)
+ Özelleştirilmiş ONNX modelden üreten [Azure özel görüntü işleme hizmeti](https://docs.microsoft.com/azure/cognitive-services/Custom-Vision-Service/) 
+ Varolan modeli ONNX için başka bir biçimden Dönüştür (Bu makalenin altındaki örneğe bakın) 
+ Azure Machine Learning hizmetinde yeni bir ONNX model eğitip (Bu makalenin altındaki örneğe bakın)

## <a name="saveconvert-your-models-to-onnx"></a>Kaydetme/modellerinize ONNX Dönüştür

Mevcut modelleri için ONNX dönüştürmek veya eğitim sonunda ONNX kaydedin.

|Model için bir çerçeve|Dönüştürme örnek veya aracı|
|-----|-------|
|PyTorch|[Jupyter not defteri](https://github.com/onnx/tutorials/blob/master/tutorials/PytorchOnnxExport.ipynb)|
|TensorFlow|[tensorflow onnx dönüştürücü](https://github.com/onnx/tensorflow-onnx)|
|Chainer|[Jupyter not defteri](https://github.com/onnx/tutorials/blob/master/tutorials/ChainerOnnxExport.ipynb)|
|MXNet|[Jupyter not defteri](https://github.com/onnx/tutorials/blob/master/tutorials/MXNetONNXExport.ipynb)|
|Keras, ScitKit-Learn ve CoreML<br/>XGBoost ve libSVM|[WinMLTools](/windows/ai/windows-ml/convert-model-winmltools)|
|Microsoft&nbsp;Bilişsel&nbsp;Araç Seti|[Jupyter not defteri](https://github.com/onnx/tutorials/blob/master/tutorials/CntkOnnxExport.ipynb)|

Desteklenen çerçeveler ve dönüştürücüler en son listesini bulabilirsiniz [ONNX öğreticiler site](https://github.com/onnx/tutorials).

<a name="deploy"></a>

## <a name="deploy-onnx-models-in-azure"></a>Azure'da ONNX modelleri dağıtma

Azure Machine Learning hizmeti ile dağıtma, yönetme ve ONNX modellerinizle izleyin. Standart [dağıtımı iş akışı](concept-model-management-and-deployment.md) ve ONNX çalışma zamanı, bulutta barındırılan bir REST uç noktası oluşturabilirsiniz. Kendiniz denemek için bu makalenin sonunda tam bir örnek Jupyter not defteri bakın. 

### <a name="install-and-configure-onnx-runtime"></a>Yükleme ve yapılandırma ONNX çalışma zamanı

Bir açık kaynak yüksek performanslı çıkarımı altyapısının ONNX modelleri için ONNX çalışma zamanıdır. Hem CPU hem de Python için kullanılabilen API'ler ile bir GPU donanım hızlandırmasını sağlar C#, ve C. ONNX çalışma zamanını destekleyen ONNX 1.2 + modeller ve Linux, Windows ve Mac üzerinde çalışır Python paketlerini kullanılabilir [PyPi.org](https://pypi.org) ([CPU](https://pypi.org/project/onnxruntime), [GPU](https://pypi.org/project/onnxruntime-gpu)), ve [ C# paket](https://www.nuget.org/packages/Microsoft.ML.OnnxRuntime/) açıktır [Nuget.org](https://www.nuget.org). Proje hakkında daha fazla bakın [GitHub](https://github.com/Microsoft/onnxruntime). Lütfen okuma [sistem gereksinimleri](https://github.com/Microsoft/onnxruntime#system-requirements) yüklemeden önce.

Python için ONNX çalışma zamanı yüklemek için kullanın:
```python
pip install onnxruntime
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

### <a name="example-deployment-steps"></a>Örnek dağıtım adımları

ONNX model dağıtmak için bir örnek aşağıda verilmiştir:

1. Azure Machine Learning hizmeti çalışma alanınızı başlatın. Henüz yoksa, bilgi nasıl [çalışma alanı oluşturma](setup-create-workspace.md).

   ```python
   from azureml.core import Workspace

   ws = Workspace.from_config()
   print(ws.name, ws.resource_group, ws.location, ws.subscription_id, sep = '\n')
   ```

2. Azure Machine Learning modeli kaydedin.

   ```python
   from azureml.core.model import Model

   model = Model.register(model_path = "model.onnx",
                          model_name = "MyONNXmodel",
                          tags = ["onnx"],
                          description = "test",
                          workspace = ws)
   ```

3. Model ve bağımlılıkları bir görüntü oluşturun.

   ```python
   from azureml.core.image import ContainerImage

   image_config = ContainerImage.image_configuration(execution_script = "score.py",
                                                     runtime = "python",
                                                     conda_file = "myenv.yml",
                                                     description = "test",
                                                     tags = ["onnx"]
                                                    )

   image = ContainerImage.create(name = "myonnxmodelimage",
                                 # this is the model object
                                 models = [model],
                                 image_config = image_config,
                                 workspace = ws)

   image.wait_for_creation(show_output = True)
   ```

   > [!TIP]
   > Önceki örnekte Azure Machine Learning hizmeti tarafından sağlanan varsayılan görüntüsünü kullanır. Ayrıca, özel bir görüntü kullanabilirsiniz. Daha fazla bilgi için bkz. configure ve görüntü bölümünü kaydetmek [modelleri dağıtma](how-to-deploy-and-where.md#configureimage).

   Dosya `score.py` Puanlama mantığı içerir ve görüntüsüne eklenmesi gerekir. Bu dosya, model görüntüde çalıştırmak için kullanılır. Bkz. Bu [öğretici](tutorial-deploy-models-with-aml.md#create-scoring-script) betik bir Puanlama oluşturma hakkında yönergeler için. ONNX model için bir örnek dosyası aşağıda gösterilmiştir:

   ```python
   import onnxruntime
   import json
   import numpy as np
   import sys
   from azureml.core.model import Model

   def init():
       global session
       model = Model.get_model_path(model_name = 'MyONNXModel')
       session = onnxruntime.InferenceSession(model)

   def preprocess(input_data_json):
       # convert the JSON data into the tensor input
       return np.array(json.loads(input_data_json)['data']).astype('float32')

   def postprocess(result):
       return np.array(result).tolist()

   def run(input_data_json):
       try:
           start = time.time()   # start timer
           input_data = preprocess(input_data_json)
           input_name = session.get_inputs()[0].name  # get the id of the first input of the model   
           result = session.run([], {input_name: input_data})
           end = time.time()     # stop timer
           return {"result": postprocess(result),
                   "time": end - start}
       except Exception as e:
           result = str(e)
           return {"error": result}
   ```

   Dosya `myenv.yml` görüntü için gereken bağımlılıklar açıklanmaktadır. Bkz. Bu [öğretici](tutorial-deploy-models-with-aml.md#create-environment-file) yönelik bu örnek dosyası gibi bir ortam dosyası oluşturmak yönergeler:

   ```python
   from azureml.core.conda_dependencies import CondaDependencies 

   myenv = CondaDependencies.create(pip_packages=["numpy","onnxruntime","azureml-core"])

   with open("myenv.yml","w") as f:
    f.write(myenv.serialize_to_string())
   ```

4. Modelinizi dağıtmak için bkz. [nasıl dağıtılacağı ve nerede](how-to-deploy-and-where.md) belge.


## <a name="examples"></a>Örnekler

Bkz: [Yardım-How-to-kullanın-azureml/dağıtım/onnx](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/onnx) örneğin ONNX modelleri oluşturup Not.

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="more-info"></a>Daha fazla bilgi

ONNX hakkında daha fazla bilgi edinin veya projeye katkıda:
+ [ONNX proje Web sitesi](https://onnx.ai)

+ [Github'da ONNX kod](https://github.com/onnx/onnx)

ONNX çalışma zamanı hakkında daha fazla bilgi edinin veya projeye katkıda:
+ [ONNX çalışma zamanı GitHub deposu](https://github.com/Microsoft/onnxruntime)


