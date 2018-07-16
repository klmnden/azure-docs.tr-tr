---
title: VS code'da birden çok Azure IOT Edge modülleri ile çalışma | Microsoft Docs
description: Birden çok modül Azure IOT Edge için tek seferde geliştirme için Visual Studio Code için IOT uzantısı kullanma
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 06/27/2018
ms.topic: conceptual
ms.service: iot-edge
ms.openlocfilehash: 31fe210b87a052438956d813db0d104e0f2cdb6e
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39041269"
---
# <a name="develop-an-iot-edge-solution-with-multiple-modules-in-visual-studio-code"></a>Visual Studio code'da birden çok modül ile bir IOT Edge çözüm geliştirin

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

## <a name="create-your-iot-edge-solution"></a>IOT Edge çözümünüzü oluşturun

1. Visual Studio code'da tümleşik Terminalini açmak **görünümü** > **tümleşik Terminalini**. 

1. VS code'da **komut paleti**girin ve şu komutu çalıştırın **Azure IOT Edge: IOT Edge yeni çözüm**. Çalışma alanı klasörünüzü seçin ve çözüm adını sağlayın (varsayılan değer EdgeSolution). Bir C# modülü oluşturma (adıyla **PipeModule**) Bu çözümdeki ilk kullanıcı modülü olarak. C# modülü varsayılan şablonunu doğrudan iletileri için Yukarı Akış aşağı yönde kanallar kanal modülüdür. İlk modülünüz için Docker görüntü deposunu da belirtmeniz gerekir. Varsayılan görüntü deposu yerel bir Docker kayıt defterini temel alır (**localhost:5000/<first module name>**). Azure Container Registry veya Docker Hub olarak değiştirebilirsiniz. 

2. VS Code penceresi IoT Edge çözümü çalışma alanınızı yükler. Kök klasör bir **modules** klasörü, bir **.vscode** klasörü ve bir dağıtım bildirimi şablon dosyası içerir. Hata ayıklama yapılandırmaları .vscode klasöründe bulunur. Tüm kullanıcı modül kodları modüller klasörünün alt klasörleridir. Deployment.template.json dosyası dağıtım bildirim şablonudur. Bu dosyadaki bazı parametreler her modül klasöründe bulunan module.json dosyasından ayrıştırılmıştır.

3. İkinci modülünüzü bu çözüm projesine ekleyin. Geçerli çözüme yeni bir modül eklemek için çeşitli yollar vardır. Girin ve şu komutu çalıştırın **Azure IOT Edge: IOT Edge Modülü Ekle**. Güncelleştirilecek dağıtım şablonu dosyasını seçin. Veya modülleri klasörü sağ tıklatın veya deployment.template.json dosyasını sağ tıklatın ve seçin **IOT Edge Modülü Ekle**. Ardından modül türü seçmek için bir açılan listedeki olacaktır. Seçin bir **- Azure işlevleri C#** adlı modülün **PipeFunction** ve kendi Docker görüntü deposu. C# işlevleri modülünün varsayılan şablonu doğrudan iletileri için Yukarı Akış aşağı yönde kanallar kanal modülüdür.

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

1. VS code'da **komut paleti**girin ve şu komutu çalıştırın **Azure IOT Edge: IOT Edge çözüm**. Her modül klasöründeki module.json dosyası temel alınarak, komut her modül Docker görüntüsünü oluşturmaya, kapsayıcılı hale getirmeye ve göndermeye başlar. Komut daha sonra gerekli değeri deployment.template.json dosyasına geçirir ve yapılandırma klasöründen bilgilerle deployment.json dosyasını oluşturur. VS Code içindeki tümleşik terminal derleme ilerlemesini gösterir. 

2. Azure IoT Hub **Device Explorer**’da, bir IoT Edge cihaz kimliğine sağ tıklayın ve ardından **Edge cihazı için dağıtım oluştur** komutunu seçin. Yapılandırma klasöründe deployment.json dosyasını seçin. VS Code içindeki tümleştirilmiş terminal bir dağıtım kimliği içinde dağıtımın başarıyla oluşturulduğunu gösterir.

3. Geliştirme makinenizde bir IOT Edge cihazını benzetme, tüm modül görüntüsü kapsayıcılar birkaç dakika içinde başlayacağını izleyebilirsiniz.

## <a name="view-the-generated-data"></a>Oluşturulan verileri görüntüleme

1. IoT hub’ına gelen verileri izlemek için, **Görünüm** > **Komut Paleti**’ni seçin. **IoT: D2C iletisini izlemeye başla** komutunu seçin. 
2. Verileri izlemeyi durdurmak için, **Komut Paleti**'nde **IoT: D2C iletisini izlemeyi durdur** komutunu kullanın. 

## <a name="next-steps"></a>Sonraki adımlar

Visual Studio Code’da Azure IoT Edge ile geliştirme için diğer senaryolar hakkında bilgi edinin:

* VS Code ile modülleri geliştirme [C#](how-to-develop-csharp-module.md) veya [Node.js](how-to-develop-node-module.md).
* Azure işlevleri ile VS code'da geliştirme [C#](how-to-develop-csharp-function.md).

Cihazlarınızı IOT Edge modülleri geliştirmek için [kavrama ve kullanma Azure IOT Hub SDK'ları](../iot-hub/iot-hub-devguide-sdks.md).