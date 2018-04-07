---
title: Visual Studio Code birden çok IOT kenar modüllerle çalışma | Microsoft Docs
description: Sınır cihazı bir modüle olarak Azure Machine Learning dağıtma
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 03/18/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 6c94701507f86f6ecab2875f952215cc3e4cc719
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="develop-an-iot-edge-solution-with-multiple-modules-in-visual-studio-code---preview"></a>Visual Studio Code birden çok modülleri IOT kenar çözümüyle geliştir - Önizleme
IOT kenar çözümünüzün birden çok modülleri geliştirmek için Visual Studio Code kullanabilirsiniz. Bu makalede oluşturma, güncelleştirme ve Visual Studio Code sanal IOT kenar cihazda bu kanallar algılayıcı verileri IOT kenar çözümünü dağıtma anlatılmaktadır. Bu makalede, bilgi nasıl yapılır:

* Bir IOT kenar çözümü oluşturmak için Visual Studio Code kullanın
* Yeni bir modül, çalışan eklemek için VS Code kullanın IOT uç çözümünün. 
* IOT kenar Cihazınızı IOT uç çözümünün (birden çok modüller) dağıtma
* Oluşturulan verileri görüntüleme

## <a name="prerequisites"></a>Önkoşullar
* Öğreticiler tamamlayın
  * [C# modül dağıtma](tutorial-csharp-module.md)
  * [C# işlevi dağıtma](tutorial-deploy-function.md)
  * [Python modülü dağıtma](tutorial-python-module.md)
* [VS Code'da için docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) görüntüler ve kapsayıcıları yönetmek için Gezgini Tümleştirmesi ile.


## <a name="prepare-your-first-iot-edge-solution"></a>İlk IOT kenar çözümünüzü hazırlama
1. VS Code komutu palette yazın ve şu komutu çalıştırın **kenar: yeni IOT uç çözümünün**. Çalışma klasörü seçin, çözüm adı belirtin (varsayılan ad **EdgeSolution**) ve bir C# modül oluşturun (**SampleModule**) Bu çözümdeki ilk kullanıcı modülü olarak. Ayrıca, ilk modülü için Docker görüntü deposuna belirtmeniz gerekir. Varsayılan görüntü deposu yerel bir Docker kayıt defteri tabanlı (`localhost:5000/<first module name>`). Bunu Azure kapsayıcı kayıt defteri veya Docker hub'a da değiştirebilirsiniz.

> [!NOTE]
> Yerel bir Docker kayıt kullanıyorsanız, lütfen kayıt defteri, komut yazarak çalıştığından emin olun `docker run -d -p 5000:5000 --restart=always --name registry registry:2` konsol penceresinde.

2. VS Code penceresini IOT kenar çözüm çalışma alanınızı yükler. Var olan bir `modules` klasörü, bir `.vscode` klasörü ve bir dağıtım bildirimi şablon dosyası kök klasöründe. Gördüğünüz hata ayıklama yapılandırmalarında `.vscode` klasör. Tüm kullanıcı modülü kodları alt klasörü altında olacak `modules`. `deployment.template.json` Dağıtım bildirim şablonudur. Bazı parametrelerin bu dosyadaki gelen ayrıştırılır `module.json`, her modül klasöründe bulunmaktadır.

3. İkinci modülünüzün Bu çözüm projeye ekleyin. Bu süreyi yazın ve çalıştırma **kenar: eklemek IOT kenar Modülü** ve güncelleştirmek için dağıtım şablonu dosyasını seçin. Ardından bir **Azure işlevi - C#** adla **SampleFunction** ve kendi Docker görüntü deposuna eklemek için.

4. Artık ilk IOT kenar çözümünüzü iki temel modüllerle hazırdır. C# Funtion kanal iletisi işlev olarak görevini yürütürken varsayılan C# modül kanal iletisi modül olarak davranır. İçinde `deployment.template.json`, bu çözüm içeren üç modülleri görürsünüz. İleti kaynaklandığı `tempSensor` modül ve aracılığıyla doğrudan yöneltilen `SampleModule` ve `SampleFunction`, IOT hub'ına gönderilen. İçerik aşağıda bu modülleriyle için rotaları güncelleştirin. 
   ```json
        "routes": {
          "SensorToPipeModule": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/SampleModule/inputs/input1\")",
          "PipeModuleToPipeFunction": "FROM /messages/modules/SampleModule/outputs/output1 INTO BrokeredEndpoint(\"/modules/SampleFunction/inputs/input1\")",
          "PipeFunctionToIoTHub": "FROM /messages/modules/SampleFunction/outputs/output1 INTO $upstream"
        },
   ```

5. Bu dosyayı kaydedin.

## <a name="build-and-deploy-your-iot-edge-solution"></a>Derleme ve IOT kenar çözümünüzü dağıtma
1. VS Code komutu palette yazın ve şu komutu çalıştırın **kenar: derleme IOT uç çözümünün**. Temel `module.json` her modül klasöründe, bu komut dosyasını denetleyin ve yapı, containerize ve her modül docker görüntü gönderme için başlatın. Gerekli değere ayrıştırır sonra `deployment.template.json`, Oluştur `deployment.json` altında gerçek değer ile `config` klasör. VS Code tümleşik terminal derleme sürüyor görebilirsiniz.

2. Azure IOT Hub cihazları Gezgini'nde bir IOT kenar cihaz kimliği sağ tıklayın ve ardından seçin **sınır cihazı için dağıtımı oluşturma**. Seçin `deployment.json` altında `config` klasör. Daha sonra dağıtım başarıyla VS code'da kimliği tümleşik bir dağıtımı ile terminal oluşturulan görebilirsiniz.

3. Kullanıyorsanız [bir IOT sınır cihazının benzetimini yapma](tutorial-simulate-device-linux.md) geliştirme makinenizde. Tüm modül görüntü kapsayıcıları birkaç dakika içinde başlatılacak görürsünüz.

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

1. IoT hub'da gelen verileri izlemek için, **Görünüm** > **Komut Paleti...** öğesini seçin ve **IoT: D2C iletisini izlemeye başlama** için arama yapın. 
2. Verileri izlemeyi durdurmak için, Komut Paleti'nde **IoT: D2C iletisini izlemeyi durdur** komutunu kullanın. 

## <a name="next-steps"></a>Sonraki adımlar

Visual Studio Code Azure IOT Edge'de geliştirirken diğer senaryolar hakkında bilgi edinmek için aşağıdaki makalelere herhangi birini açın devam edebilirsiniz:

* [Bir C# modül VS kodda hata ayıklama](how-to-vscode-debug-csharp-module.md)
* [Bir C# işlevi VS kodda hata ayıklama](how-to-vscode-debug-azure-function.md)