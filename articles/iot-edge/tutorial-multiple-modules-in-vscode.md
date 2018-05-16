---
title: VS code'da birden çok Azure IOT kenar modüllerini yönetmek | Microsoft Docs
description: Birden fazla modülü kullanmak IOT kenar çözümleri geliştirmek için Visual Studio Code kullanın.
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 03/18/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 4d9caa7aa95c99fa30e8ec76c5b6362615725622
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="develop-an-iot-edge-solution-with-multiple-modules-in-visual-studio-code---preview"></a>Visual Studio Code birden çok modülleri IOT kenar çözümüyle geliştir - Önizleme
IOT kenar çözümünüzün birden çok modülleri geliştirmek için Visual Studio Code kullanabilirsiniz. Bu makalede oluşturma, güncelleştirme ve Visual Studio Code sanal IOT kenar cihazda bu kanallar algılayıcı verileri IOT kenar çözümünü dağıtma anlatılmaktadır. 

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki tüm adımları tamamlamak için yerinde aşağıdaki önkoşullara sahip:

- [Visual Studio Code](https://code.visualstudio.com/) 
- [Visual Studio Code için Azure IOT kenar uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
- [C# Visual Studio Code (OmniSharp tarafından desteklenen) uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 
- [Docker](https://docs.docker.com/engine/installation/)
- [.NET core 2.0 SDK'sı](https://www.microsoft.com/net/core#windowscmd) 
- AzureIoTEdgeModule şablonu (`dotnet new -i Microsoft.Azure.IoT.Edge.Module`)
- Etkin bir IOT hub en az bir IOT sınır cihazı ile


* [VS Code'da için docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) görüntüler ve kapsayıcıları yönetmek için Gezgini Tümleştirmesi ile.


## <a name="prepare-your-first-iot-edge-solution"></a>İlk IOT kenar çözümünüzü hazırlama
1. VS Code komutu palette yazın ve şu komutu çalıştırın **kenar: yeni IOT uç çözümünün**. Çalışma klasörü seçin, çözüm adı belirtin (varsayılan ad **EdgeSolution**) ve bir C# modül oluşturun (**SampleModule**) Bu çözümdeki ilk kullanıcı modülü olarak. Ayrıca, ilk modülü için Docker görüntü deposuna belirtmeniz gerekir. Varsayılan görüntü deposu yerel bir Docker kayıt defteri tabanlı (`localhost:5000/<first module name>`). Bunu Azure kapsayıcı kayıt defteri veya Docker hub'a da değiştirebilirsiniz.

   > [!NOTE]
   > Yerel bir Docker kayıt kullanıyorsanız, kayıt defteri, komut yazarak çalıştığından emin olun `docker run -d -p 5000:5000 --restart=always --name registry registry:2` konsol penceresinde.

2. VS Code penceresini IOT kenar çözüm çalışma alanınızı yükler. Kök klasörünü içeren bir `modules` klasörü, bir `.vscode` klasörü ve bir dağıtım bildirim şablon dosyası. Gördüğünüz hata ayıklama yapılandırmalarında `.vscode` klasör. Tüm kullanıcı modülü kodları alt klasörü altında olacak `modules`. `deployment.template.json` Dağıtım bildirim şablonudur. Bazı parametrelerin bu dosyadaki gelen ayrıştırılır `module.json`, her modül klasöründe bulunmaktadır.

3. İkinci modülünüzün Bu çözüm projeye ekleyin. Bu süreyi yazın ve çalıştırma **kenar: eklemek IOT kenar Modülü** ve güncelleştirmek için dağıtım şablonu dosyasını seçin. Ardından bir **Azure işlevi - C#** adla **SampleFunction** ve kendi Docker görüntü deposu.

4. Açık `deployment.template.json` dosya ve çalışma zamanı yanı sıra üç modülleri bildirir doğrulayın. İleti kaynaklandığı `tempSensor` modül ve aracılığıyla doğrudan yöneltilen `SampleModule` ve `SampleFunction`, IOT hub'ına gönderilen. 
5. Bu modüller için rotaları aşağıdaki içerik ile güncelleştirin:
   ```json
   "routes": {
      "SensorToPipeModule": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/SampleModule/inputs/input1\")",
      "PipeModuleToPipeFunction": "FROM /messages/modules/SampleModule/outputs/output1 INTO BrokeredEndpoint(\"/modules/SampleFunction/inputs/input1\")",
      "PipeFunctionToIoTHub": "FROM /messages/modules/SampleFunction/outputs/output1 INTO $upstream"
   },
   ```

6. Bu dosyayı kaydedin.

## <a name="build-and-deploy-your-iot-edge-solution"></a>Derleme ve IOT kenar çözümünüzü dağıtma
1. VS Code komutu palette yazın ve şu komutu çalıştırın **kenar: derleme IOT uç çözümünün**. Temel `module.json` dosyası her modül klasöründe, yapı, containerize ve her modül docker görüntü gönderme için bu komutu Başlat. Gerekli değere geçirmeden sonra `deployment.template.json` ve oluşturur `deployment.json` dosya bilgileriyle `config` klasör. VS Code tümleşik terminal derleme sürüyor görebilirsiniz.

2. Azure IOT Hub cihazları Gezgini'nde bir IOT kenar cihaz kimliği sağ tıklayın ve ardından seçin **sınır cihazı için dağıtımı oluşturma**. Seçin `deployment.json` altında `config` klasör. Daha sonra dağıtım başarıyla VS code'da kimliği tümleşik bir dağıtımı ile terminal oluşturulan görebilirsiniz.

3. Geliştirme makinenizde bir IOT sınır cihazının benzetimini, tüm modül görüntü kapsayıcıları birkaç dakika içinde başlatılacak görürsünüz.

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

1. IoT hub'da gelen verileri izlemek için, **Görünüm** > **Komut Paleti...** öğesini seçin ve **IoT: D2C iletisini izlemeye başlama** için arama yapın. 
2. Verileri izlemeyi durdurmak için, Komut Paleti'nde **IoT: D2C iletisini izlemeyi durdur** komutunu kullanın. 

## <a name="next-steps"></a>Sonraki adımlar

Visual Studio Code Azure IOT Edge'de geliştirmek için diğer senaryolar hakkında bilgi edinin:

* [Bir C# modül VS kodda hata ayıklama](how-to-vscode-debug-csharp-module.md)
* [Bir C# işlevi VS kodda hata ayıklama](how-to-vscode-debug-azure-function.md)