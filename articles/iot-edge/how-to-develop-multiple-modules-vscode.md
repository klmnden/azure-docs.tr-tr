---
title: VS code'da birden çok Azure IOT kenar modüllerle çalışma | Microsoft Docs
description: Azure IOT köşesi aynı anda birden çok modülleri geliştirmek için Visual Studio Code IOT uzantısı kullanın
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 06/27/2018
ms.topic: conceptual
ms.service: iot-edge
ms.openlocfilehash: 4e9aac5f19fa75613dee2aba3853a0243d7d966b
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37048269"
---
# <a name="develop-an-iot-edge-solution-with-multiple-modules-in-visual-studio-code"></a>Visual Studio Code birden çok modülleri IOT kenar çözümüyle geliştirin

Birden çok modül ile Azure IoT Edge çözümünüzü geliştirmek için Visual Studio Code kullanabilirsiniz. Bu makadele, VS Code içinde simülasyonu yapılan bir IoT Edge cihazındaki sensör verilerini gönderen bir IoT Edge çözümünü oluşturma, güncelleştirme ve dağıtma gösterilmektedir. 

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları tamamlayabilmeniz için şu önkoşullar gereklidir:

- [Visual Studio Code](https://code.visualstudio.com/)
- [Visual Studio Code için Azure IoT Edge uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge)
- [Visual Studio Code için C# (OmniSharp tarafından desteklenen) uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
- [Docker](https://docs.docker.com/engine/installation/)
- [.NET Core 2.1 SDK'sı](https://www.microsoft.com/net/download)
- En az bir IoT Edge cihazı olan etkin bir IoT hub

Görüntüleri ve kapsayıcıları yönetmek için Azure IoT Hub Device Explorer tümleştirmesi ile [VS Code için Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) da gereklidir.

## <a name="create-your-iot-edge-solution"></a>IOT kenar çözümünüzü oluşturun

1. Visual Studio kodda seçerek tümleşik Terminali açın **Görünüm** > **tümleşik terminal**. 

1. VS code'da **komutu palet**komutunu çalıştırın ve girin **Azure IOT kenar: yeni IOT uç çözümünün**. Çalışma alanı klasörünüzü seçin ve çözüm adını sağlayın (varsayılan değer EdgeSolution). Bir C# modül oluşturun (adıyla **PipeModule**) Bu çözümdeki ilk kullanıcı modülü olarak. C# modülünün varsayılan şablonu doğrudan iletileri için Yukarı Akış aşağı kanallar kanal modülüdür. İlk modülünüz için Docker görüntü deposunu da belirtmeniz gerekir. Varsayılan görüntü deposu yerel bir Docker kayıt defterini temel alır (**localhost:5000/<first module name>**). Azure Container Registry veya Docker Hub olarak değiştirebilirsiniz. 

2. VS Code penceresi IoT Edge çözümü çalışma alanınızı yükler. Kök klasör bir **modules** klasörü, bir **.vscode** klasörü ve bir dağıtım bildirimi şablon dosyası içerir. Hata ayıklama yapılandırmaları .vscode klasöründe bulunur. Tüm kullanıcı modül kodları modüller klasörünün alt klasörleridir. Deployment.template.json dosyası dağıtım bildirim şablonudur. Bu dosyadaki bazı parametreler her modül klasöründe bulunan module.json dosyasından ayrıştırılmıştır.

3. İkinci modülünüzü bu çözüm projesine ekleyin. Geçerli çözüme yeni bir modül eklemek için birkaç yolu vardır. Girin ve şu komutu çalıştırın **Azure IOT kenar: eklemek IOT kenar Modülü**. Güncelleştirilecek dağıtım şablonu dosyasını seçin. Veya modüller klasörüne sağ tıklayın veya deployment.template.json dosyasını sağ tıklatın ve seçin **IOT kenar Modül Ekle**. Ardından modül türü seçmek için açılır liste olacaktır. Seçin bir **Azure işlevleri - C#** adında modül **PipeFunction** ve kendi Docker görüntü deposu. C# işlevleri modülünün varsayılan şablonu doğrudan iletileri için Yukarı Akış aşağı kanallar kanal modülüdür.

4. Deployment.template.json dosyasını açın. Dosyanın üç modülü ve çalışma zamanını bildirdiğinden emin olun. İleti tempSensor modülünden oluşturulur. İleti SampleModule ve SampleFunction modülleri ile doğrudan gönderilir ve daha sonra IoT hub’ınıza gönderilir. 

5. Bu modüller için rotaları aşağıdaki içerikle güncelleştirin:

   ```json
        "routes": {
          "SensorToPipeModule": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/PipeModule/inputs/input1\")",
          "PipeModuleToPipeFunction": "FROM /messages/modules/PipeModule/outputs/output1 INTO BrokeredEndpoint(\"/modules/PipeFunction/inputs/input1\")",
          "PipeFunctionToIoTHub": "FROM /messages/modules/PipeFunction/outputs/output1 INTO $upstream"
        },
   ```

5. Bu dosyayı kaydedin.

## <a name="build-and-deploy-your-iot-edge-solution"></a>IoT Edge çözümünüzü oluşturma ve dağıtma

1. VS code'da **komutu palet**komutunu çalıştırın ve girin **Azure IOT kenar: derleme IOT uç çözümünün**. Her modül klasöründeki module.json dosyası temel alınarak, komut her modül Docker görüntüsünü oluşturmaya, kapsayıcılı hale getirmeye ve göndermeye başlar. Komut daha sonra gerekli değeri deployment.template.json dosyasına geçirir ve yapılandırma klasöründen bilgilerle deployment.json dosyasını oluşturur. VS Code içindeki tümleşik terminal derleme ilerlemesini gösterir. 

2. Azure IoT Hub **Device Explorer**’da, bir IoT Edge cihaz kimliğine sağ tıklayın ve ardından **Edge cihazı için dağıtım oluştur** komutunu seçin. Yapılandırma klasöründe deployment.json dosyasını seçin. VS Code içindeki tümleştirilmiş terminal bir dağıtım kimliği içinde dağıtımın başarıyla oluşturulduğunu gösterir.

3. Geliştirme makinenizde bir IOT sınır cihazının benzetimini varsa, tüm modül görüntü kapsayıcıların birkaç dakika içinde Başlat görmek izleyebilirsiniz.

## <a name="view-the-generated-data"></a>Oluşturulan verileri görüntüleme

1. IoT hub’ına gelen verileri izlemek için, **Görünüm** > **Komut Paleti**’ni seçin. **IoT: D2C iletisini izlemeye başla** komutunu seçin. 
2. Verileri izlemeyi durdurmak için, **Komut Paleti**'nde **IoT: D2C iletisini izlemeyi durdur** komutunu kullanın. 

## <a name="next-steps"></a>Sonraki adımlar

Visual Studio Code’da Azure IoT Edge ile geliştirme için diğer senaryolar hakkında bilgi edinin:

* [Bir C# modül VS code'da geliştirin](how-to-develop-csharp-module.md)
* [VS code'da C# işlevi geliştirin](how-to-develop-csharp-function.md)
