---
title: "Azure Machine Learning Azure IOT kenarıyla dağıtma | Microsoft Docs"
description: "Sınır cihazı bir modüle olarak Azure Machine Learning dağıtma"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: e061e599f365bf3d343cb59b8dc6a61e06627517
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2017
---
# <a name="deploy-azure-machine-learning-as-an-iot-edge-module---preview"></a>Azure Machine Learning IOT kenar modül olarak dağıtma - Önizleme

IOT kenar modülleri, iş mantığınızı IOT sınır cihazları için doğrudan uygulayan kod dağıtmak için kullanabilirsiniz. Bu öğreticide, bir cihaz üzerinde bir sanal cihaz dağıtmak Azure IOT kenarına oluşturduğunuz sanal IOT sınır cihazı algılayıcı verilerini göre başarısız olduğunda tahmin bir Azure Machine Learning modülü dağıtımı aracılığıyla açıklanmaktadır [Windows] [ lnk-tutorial1-win] veya [Linux] [ lnk-tutorial1-lin] öğreticileri. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz: 

> [!div class="checklist"]
> * Bir Azure Machine Learning modülü IOT kenar Cihazınızı dağıtma
> * Oluşturulan görünüm verileri

Ne zaman kullanmak istediğiniz kendi [Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/preview/) şunları yapacaksınız çözümünüzdeki model [bir model dağıtma](https://aka.ms/aml-iot-edge-doc) IOT kenarı ve ana bilgisayar için bir kapsayıcı kayıt defterindeki ister [Azure kapsayıcı kayıt defteri](../container-registry/index.yml) veya Docker.

## <a name="prerequisites"></a>Ön koşullar

* Hızlı Başlangıç ya da ilk öğreticide oluşturduğunuz Azure IOT sınır cihazı.
* IOT kenar cihazın bağlandığı IOT hub'ın IOT Hub bağlantı dizesi.
* Azure ML kapsayıcısı

## <a name="create-the-azure-ml-container"></a>Azure ML kapsayıcı oluşturma
Azure ML kapsayıcı oluşturmak için'ndaki yönergeleri izleyin [AI Araç Seti için Azure IOT kenar](https://aka.ms/aml-iot-edge-anomaly-detection).

## <a name="run-the-solution"></a>Çözümü çalıştırın

1. Üzerinde [Azure portal](https://portal.azure.com), IOT hub'ına gidin.
1. Git **IOT kenar (Önizleme)** ve IOT sınır cihazı seçin.
1. Seçin **ayarlamak modülleri**.
1. Seçin **ekleme IOT kenar Modülü**.
1. İçinde **adı** alanına, `tempSensor`.
1. İçinde **görüntü URI** alanına, `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`.
1. Diğer ayarları değiştirmeden bırakın ve seçin **kaydetmek**.
1. Hala **modülleri Ekle** adım, select **ekleme IOT kenar Modülü** yeniden.
1. İçinde **adı** alanında, önceki bölümde yaptığınız kapsayıcının adını girin. Başvurmak [AI Araç Seti için Azure IOT kenar](https://aka.ms/aml-iot-edge-anomaly-detection) adını bulmak için Yardım için.
1. İçinde **görüntü** alan, görüntü önceki bölümde belirlediğiniz kapsayıcı URI'sini girin. Başvurmak [AI Araç Seti için Azure IOT kenar](https://aka.ms/aml-iot-edge-anomaly-detection) görüntü bulmak için Yardım için.
1. **Kaydet** düğmesine tıklayın.
1. Geri **modülleri Ekle** adımını, **sonraki**.
1. Yollar modülünüzün için güncelleştirme:
1. İçinde **belirtin yollar** adım, JSON altındaki metin kutusuna Kopyala. Modülleri tüm iletileri kenar çalışma zamanına yayımlayın. Burada bu iletileri akış çalışma zamanında bildirim temelli kuralları tanımlar. Bu öğreticide, iki yol gerekir. İlk yol, tüm Azure Machine Learning modüllerinin kullanan uç nokta olduğu sıcaklık algılayıcısı iletilerden "amlInput" uç noktası aracılığıyla makine öğrenme modülü taşımaları. İkinci yol makine öğrenme modülü iletilerden IOT Hub'ına taşımaları. Bu rotadaki '' amlOutput'' veri çıkışı için tüm Azure Machine Learning modülleri kullanan uç nokta ve '' upstream$ '' kenar Hub'ın IOT Hub'ına iletileri göndermek için söyler özel bir hedef. 

    ```json
    {
        "routes": {
            "sensorToMachineLearning":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/machinelearningmodule/inputs/amlInput\")",
            "machineLearningToIoTHub": "FROM /messages/modules/machinelearningmodule/outputs/amlOutput INTO $upstream"
        }
    }
    ``` 

1. **İleri**’ye tıklayın. 
1. '' Gözden geçirme şablonu '' adımda '' Gönder '''i tıklatın. 
1. Cihaz ayrıntıları sayfasına dönün ve ''.'' Yenile  '' '' TempSensor modülü '' ve '' IOT kenar çalışma zamanı '' birlikte çalışan yeni machinelearningmodule'' görmeniz gerekir.

## <a name="view-generated-data"></a>Oluşturulan görünüm verileri

 VS Code'da kullanmak **View | Palet komutu... | IOT: Başlangıç izleme D2C iletileri** IOT hub'ı ulaşan verilerin izlemek için menü komutu. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Machine Learning tarafından desteklenen bir IOT kenar modülü dağıtıldı. İşletme öngörüleri kenarına içine veri kapatmak Azure IOT kenar yardımcı olabilecek diğer yollarını öğrenmek için diğer öğreticileri birini açın devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Bir Azure işlevi bir modül olarak dağıtma](tutorial-deploy-function.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
