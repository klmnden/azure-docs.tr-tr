---
title: VS Code içinde birden fazla Azure IoT Edge modülünü yönetme | Microsoft Docs
description: Birden çok modül kullanan Azure IoT Edge çözümleri geliştirmek için Visual Studio Code kullanın.
author: shizn
manager: ''
ms.author: xshi
ms.date: 03/18/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 4a624994f93e7a3cbb04db2a0ec0b2b823a12ee7
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763043"
---
# <a name="develop-an-iot-edge-solution-with-multiple-modules-in-visual-studio-code-preview"></a>Visual Studio Code’da birden çok modül ile IoT Edge çözümü geliştirme (önizleme)

Birden çok modül ile Azure IoT Edge çözümünüzü geliştirmek için Visual Studio Code kullanabilirsiniz. Bu makadele, VS Code içinde simülasyonu yapılan bir IoT Edge cihazındaki sensör verilerini gönderen bir IoT Edge çözümünü oluşturma, güncelleştirme ve dağıtma gösterilmektedir. 

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımları tamamlayabilmeniz için şu önkoşullar gereklidir:

- [Visual Studio Code](https://code.visualstudio.com/)
- [Visual Studio Code için Azure IoT Edge uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge)
- [Visual Studio Code için C# (OmniSharp tarafından desteklenen) uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
- [Docker](https://docs.docker.com/engine/installation/)
- [.NET Core 2.0 SDK’sı](https://www.microsoft.com/net/core#windowscmd)
- The AzureIoTEdgeModule şablonu (`dotnet new -i Microsoft.Azure.IoT.Edge.Module`)
- En az bir IoT Edge cihazı olan etkin bir IoT hub

Görüntüleri ve kapsayıcıları yönetmek için Azure IoT Hub Device Explorer tümleştirmesi ile [VS Code için Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) da gereklidir.

## <a name="prepare-your-first-iot-edge-solution"></a>İlk IoT Edge çözümünüzü hazırlama

1. VS Code **Komut Paleti** içinde, **Edge: Yeni IoT Edge çözümü** komutunu girin ve çalıştırın. Çalışma alanı klasörünüzü seçin ve çözüm adını sağlayın (varsayılan değer EdgeSolution). Bu çözümde ilk kullanıcı modülü olarak bir C# modülü (**SampleModule** adıyla) oluşturun. İlk modülünüz için Docker görüntü deposunu da belirtmeniz gerekir. Varsayılan görüntü deposu yerel bir Docker kayıt defterini temel alır (**localhost:5000/<first module name>**). Azure Container Registry veya Docker Hub olarak değiştirebilirsiniz.

   > [!NOTE]
   > Yerel bir Docker kayıt defteri kullanıyorsanız, kayıt defterinin çalıştığından emin olun. Konsol penceresine aşağıdaki komutu girin:
   > 
   > `docker run -d -p 5000:5000 --restart=always --name registry registry:2`

2. VS Code penceresi IoT Edge çözümü çalışma alanınızı yükler. Kök klasör bir **modules** klasörü, bir **.vscode** klasörü ve bir dağıtım bildirimi şablon dosyası içerir. Hata ayıklama yapılandırmaları .vscode klasöründe bulunur. Tüm kullanıcı modül kodları modüller klasörünün alt klasörleridir. Deployment.template.json dosyası dağıtım bildirim şablonudur. Bu dosyadaki bazı parametreler her modül klasöründe bulunan module.json dosyasından ayrıştırılmıştır.

3. İkinci modülünüzü bu çözüm projesine ekleyin. Girin ve **Edge: IoT Edge modülü ekle** komutunu çalıştırın. Güncelleştirilecek dağıtım şablonu dosyasını seçin. **SampleFunction** adlı bir **Azure İşlevi - C#** modülü ve Docker görüntü deposunu seçin.

4. Deployment.template.json dosyasını açın. Dosyanın üç modülü ve çalışma zamanını bildirdiğinden emin olun. İleti tempSensor modülünden oluşturulur. İleti SampleModule ve SampleFunction modülleri ile doğrudan gönderilir ve daha sonra IoT hub’ınıza gönderilir. 

5. Bu modüller için rotaları aşağıdaki içerikle güncelleştirin:

   ```json
   "routes": {
      "SensorToPipeModule": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/SampleModule/inputs/input1\")",
      "PipeModuleToPipeFunction": "FROM /messages/modules/SampleModule/outputs/output1 INTO BrokeredEndpoint(\"/modules/SampleFunction/inputs/input1\")",
      "PipeFunctionToIoTHub": "FROM /messages/modules/SampleFunction/outputs/output1 INTO $upstream"
   },
   ```

6. Bu dosyayı kaydedin.

## <a name="build-and-deploy-your-iot-edge-solution"></a>IoT Edge çözümünüzü oluşturma ve dağıtma

1. VS Code **Komut Paleti** içinde, **Edge: IoT Edge çözümü oluştur** komutunu girin ve çalıştırın. Her modül klasöründeki module.json dosyası temel alınarak, komut her modül Docker görüntüsünü oluşturmaya, kapsayıcılı hale getirmeye ve göndermeye başlar. Komut daha sonra gerekli değeri deployment.template.json dosyasına geçirir ve yapılandırma klasöründen bilgilerle deployment.json dosyasını oluşturur. VS Code içindeki tümleşik terminal derleme ilerlemesini gösterir.

2. Azure IoT Hub **Device Explorer**’da, bir IoT Edge cihaz kimliğine sağ tıklayın ve ardından **Edge cihazı için dağıtım oluştur** komutunu seçin. Yapılandırma klasöründe deployment.json dosyasını seçin. VS Code içindeki tümleştirilmiş terminal bir dağıtım kimliği içinde dağıtımın başarıyla oluşturulduğunu gösterir.

3. Dağıtım makinenizde bir IoT Edge cihazının simülasyonunu yapıyorsanız, tüm modül görüntü kapsayıcılarının birkaç dakika içinde başlatıldığına dikkat edin.

## <a name="view-the-generated-data"></a>Oluşturulan verileri görüntüleme

1. IoT hub’ına gelen verileri izlemek için, **Görünüm** > **Komut Paleti**’ni seçin. **IoT: D2C iletisini izlemeye başla** komutunu seçin. 
2. Verileri izlemeyi durdurmak için, **Komut Paleti**'nde **IoT: D2C iletisini izlemeyi durdur** komutunu kullanın. 

## <a name="next-steps"></a>Sonraki adımlar

Visual Studio Code’da Azure IoT Edge ile geliştirme için diğer senaryolar hakkında bilgi edinin:

* [VS Code içinde C# modülünde hata ayıklama](how-to-vscode-debug-csharp-module.md)
* [VS Code içinde C# işlevinde hata ayıklama](how-to-vscode-debug-azure-function.md)
