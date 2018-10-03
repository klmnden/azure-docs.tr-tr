---
title: Azure Machine Learning hizmetinden IOT Edge cihazlarına modelleri dağıtma | Microsoft Docs
description: Azure IOT Edge cihazları Azure Machine Learning hizmetinden alınan bir model dağıtma konusunda bilgi edinin. Bir model edge cihazına dağıtma, verileri buluta göndermeden ve bir yanıt bekliyor yerine cihazın puanlamanıza olanak sağlar.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: shipatel
author: shivanipatel
manager: cgronlun
ms.reviewer: larryfr
ms.date: 09/24/2018
ms.openlocfilehash: 20469e127c8e04f4c6418fe28c49b63fc3b363d8
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48239209"
---
# <a name="prepare-to-deploy-models-on-iot-edge"></a>IOT Edge üzerinde modelleri dağıtmaya hazırlanma

Bu belgede, Azure Machine Learning hizmeti bir Azure IOT Edge cihazına dağıtım eğitilen bir modelin hazırlamak için kullanmayı öğrenin.

Azure IOT Edge cihazı, bir Linux veya Azure IOT Edge çalışma zamanı çalışan Windows tabanlı bir cihaz örneğidir. Makine öğrenimi modelleri için bu cihazları IOT Edge modülleri dağıtılabilir. IOT Edge cihazına bir model dağıtımına model bulut işleme için veri göndermek zorunda yerine doğrudan kullanmak cihazın verir. Daha hızlı yanıt süreleri ve daha az veri aktarımı olursunuz.

Edge cihazına bir model dağıtmadan önce model ve cihazı hazırlamak için bu belgedeki adımlarda kullanın.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

* Bir Azure Machine Learning hizmeti çalışma alanı. Oluşturmak için adımları kullanın. [Azure Machine Learning hizmeti ile çalışmaya başlama](quickstart-get-started.md) belge.

* Bir geliştirme ortamı. Daha fazla bilgi için [bir geliştirme ortamı yapılandırma](how-to-configure-environment.md) belge.

* Bir [Azure IOT hub'ı](../../iot-hub/iot-hub-create-through-portal.md) Azure aboneliğinizdeki. 

* Eğitilen bir modeli. Bir model eğitip ilişkin bir örnek için bkz: [bir Azure Machine Learning ile görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md) belge.

## <a name="prepare-the-iot-device"></a>IOT cihazı hazırlama

Cihazınızı kaydedemedik ve IOT çalışma zamanı yükleme konusunda bilgi almak için adımları izleyin. [hızlı başlangıç: Linux x64 cihaza, ilk IOT Edge modülü dağıtma](../../iot-edge/quickstart-linux.md) belge.

## <a name="register-the-model"></a>Modeli kaydedin

Azure IOT Edge modülleri, kapsayıcı görüntüleri üzerinde temel alır. Modelinizi IOT Edge cihazına dağıtmak için bir Azure Machine Learning hizmeti çalışma alanında, modelinize kaydetmek ve bir Docker görüntüsü oluşturmak için aşağıdaki adımları kullanın. 

> [!IMPORTANT]
> Bu durumda, Azure Machine Learning çalışma alanınızda zaten kaydedilebilir, modeli eğitmek için kullanılan, 3. adımına geçin.

1. Çalışma alanını Başlat ve config.json dosyası yüklenemiyor:

    ```python
    from azureml.core  import Workspace

    #Load existing workspace from the config file info.
    ws  = Workspace.from_config()
    ```    

1. Çalışma alanınıza modeli kaydedin. Varsayılan metni modeli yolu, adınızı, etiketler ve açıklaması ile değiştirin:

    ```python
    from azureml.core.model import Model
    model = Model.register(model_path = "model.pkl", # this path points to the local file
                        model_name = "best_model", # the model gets registered as this name
                        tags = {'attribute': "myattribute", 'classification': "myclassification"},
                        description = "My awesome model",
                        workspace = ws)
    ```    

1. Kayıtlı modeli alın: 

    ```python
    from azureml.core.model import Model

    model_name = "best_model"
    model = Model(ws, model_name)                     
    ```    

## <a name="create-a-docker-image"></a>Docker görüntüsü oluşturma

1. Oluşturma bir **Puanlama betik** adlı `score.py`. Bu dosya, resim içindeki model çalıştırmak için kullanılır. Aşağıdaki işlevleri içermesi gerekir:

    * `init()` İşlevin genellikle modeli genel bir nesnesine yükler. Bu işlev, yalnızca Docker kapsayıcı başlatıldığında bir kez çalıştırılır. 

    * `run(input_data)` İşlevi, giriş verileri temel alan bir değer tahmin modeli kullanır. Girişler ve çıkışlar farklı çalıştır genellikle JSON için seri hale getirme ve serinin kullanır, ancak diğer biçimleri desteklenir.

    Bir örnek için bkz. [görüntü sınıflandırma Öğreticisi](tutorial-deploy-models-with-aml.md#make-script).

1. Oluşturma bir **ortam dosyası** adlı `myenv.yml`. Bu dosya, Conda ortam belirtimi ve tüm betik ve model tarafından gerekli bağımlılıkları listeler. Bir örnek için bkz. [görüntü sınıflandırma Öğreticisi](tutorial-deploy-models-with-aml.md#make-myenv).

1. Docker görüntüsünü kullanarak yapılandırma `score.py` ve `myenv.yml` dosyaları:
    
    ```python
    from azureml.core.image import Image, ContainerImage
    
    #Image configuration
    image_config = ContainerImage.image_configuration( runtime = "python", 
                           execution_script = "score.py",
                           conda_file = "myenv.yml", 
                           tags = {"attributes", "calssification"},
                           description = "Image that contains my model",
                           
                        )
    ```    

1. Model ve görüntü Yapılandırması'nı kullanarak görüntü oluştur:

    ```python
    image = ContainerImage.create (name = "myimage", 
                           models = [model], #this is the model object
                           image_config = image_config,
                           workspace = ws
                        )
    ```     

    Görüntü oluşturulması yaklaşık 5 dakika sürer.

## <a name="get-the-container-registry-credentials"></a>Kapsayıcı kayıt defteri kimlik bilgilerini alma

Azure IOT, kapsayıcı kayıt defterine docker görüntüleri depolayan Azure Machine Learning hizmeti için kimlik bilgileri gerekir. Kimlik bilgilerini almak için aşağıdaki adımları kullanın:

1. [Azure Portal](https://portal.azure.com/signin/index)’da oturum açın.

1. Azure Machine Learning hizmeti çalışma alanınıza gidin ve seçin __genel bakış__. Kapsayıcı kayıt defteri ayarları'na gidin, seçin __kayıt defteri__ bağlantı.

    ![Bir kapsayıcı kayıt defteri girdisinin görüntüsü](./media/how-to-deploy-to-iot/findregisteredcontainer.png)

1. Kapsayıcı kayıt defterinde seçmek **erişim anahtarlarını** ve yönetici kullanıcıyı etkinleştirin.

    ![Erişim anahtarları ekran görüntüsü](./media/how-to-deploy-to-iot/findaccesskey.png)

1. Oturum açma sunucusu, kullanıcı adı ve parola değerleri kaydedin. 

   Bu kimlik bilgileri, özel kapsayıcı kayıt defterinizde görüntülerine IOT edge cihaz erişim sağlamak gereklidir.

## <a name="next-steps"></a>Sonraki adımlar

Dağıtım için hazırlıklar tamamladınız. Şimdi, içindeki adımları kullanın [dağıtma Azure IOT Edge modülleri Azure portalından](../../iot-edge/how-to-deploy-modules-portal.md) belge edge cihazınıza dağıtma. Yapılandırma sırasında __kayıt defteri ayarları__ cihaz için daha önce yapılandırdığınız kimlik bilgilerini kullanın.
