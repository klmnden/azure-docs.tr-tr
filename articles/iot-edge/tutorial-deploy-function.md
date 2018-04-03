---
title: Azure IoT Edge ile Azure İşlevi'ni dağıtma | Microsoft Docs
description: Azure İşlevi'ni bir modül olarak Edge cihazına dağıtma
services: iot-edge
keywords: ''
author: kgremban
manager: timlt
ms.author: v-jamebr
ms.date: 11/15/2017
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: a43ae8f28fc32b61fb5db985ffae98f093293798
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-azure-function-as-an-iot-edge-module---preview"></a>Azure İşlevi'ni IoT Edge modülü olarak dağıtma - önizleme
İş mantığınızı doğrudan IoT Edge cihazlarınıza uygulayan kodu dağıtmak için Azure İşlevleri'ni kullanabilirsiniz. Bu öğreticide, [Windows][lnk-tutorial1-win] veya [Linux][lnk-tutorial1-lin]'ta simülasyon cihazındaki Azure IoT Edge'e dağıtma öğreticilerinde oluşturduğunuz simülasyon IoT Edge cihazındaki algılayıcı verilerini filtreleyen bir Azure İşlevi oluşturma ve dağıtma işlemlerinde yol gösterilir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:     

> [!div class="checklist"]
> * Visual Studio Code kullanarak Azure İşlevi oluşturma
> * VS Code ve Docker kullanarak Docker görüntüsü oluşturma ve bunu kayıt defterinize yayımlama 
> * Modüle IoT Edge cihazınıza dağıtma
> * Oluşturulan verileri görüntüleme


Bu öğreticide oluşturduğunuz Azure İşlevi cihazınız tarafından oluşturulan sıcaklık verilerini filtreler ve yalnızca sıcaklık belirtilen eşiği aştığında Azure IoT Hub'ına yukarı akışla iletiler gönderir. 

## <a name="prerequisites"></a>Ön koşullar

* Hızlı başlangıçta veya önceki öğreticide oluşturduğunuz Azure IoT Edge cihazı.
* [Visual Studio Code](https://code.visualstudio.com/). 
* [Visual Studio Code için C# (OmniSharp tarafından desteklenen) uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). 
* [Visual Studio Code için Azure IoT Edge uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
* [Docker](https://docs.docker.com/engine/installation/). Platformunuzun Community Edition'ı (CE) bu öğretici için yeterlidir. 
* [.NET Core 2.0 SDK](https://www.microsoft.com/net/core#windowscmd). 

## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma
Bu öğreticide modül hazırlamak ve dosyalardan bir **kapsayıcı görüntüsü** oluşturmak için VS Code için Azure IoT Edge uzantısını kullanırsınız. Ardından bu görüntüyü, görüntülerinizin depolandığı ve yönetildiği **kayıt defterine** gönderirsiniz. Son olarak, görüntünüzü IoT Edge cihazınızda çalıştırmak üzere kayıt defterinizden dağıtırsınız.  

Bu öğretici için Docker ile uyumlu herhangi bir kayıt defteri kullanabilirsiniz. Bulutta sağlanan iki popüler Docker kayıt defteri hizmeti [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) ve [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)'dır. Bu öğreticide Azure Container Registry kullanılır. 

1. [Azure portalında](https://portal.azure.com), **Kaynak oluştur** > **Kapsayıcılar** > **Azure Container Registry**'yi seçin.
2. Kayıt defterinize bir ad verin, abonelik seçin, kaynak grubu seçin ve SKU'yu **Temel** olarak ayarlayın. 
3. **Oluştur**’u seçin.
4. Kapsayıcı kayıt defteriniz oluşturulduktan sonra, bu kayıt defterine gidin ve **Erişim anahtarları**'nı seçin. 
5. **Yönetici kullanıcı** ayarını **Etkinleştir**'e getirin.
6. **Oturum açma sunucusu**, **Kullanıcı adı** ve **Parola** değerlerini kopyalayın. Öğreticinin sonraki bölümlerinde bu değerleri kullanacaksınız. 

## <a name="create-a-function-project"></a>İşlev projesi oluşturma
Aşağıdaki adımlarda, Visual Studio Code'u ve Azure IoT Edge uzantısını kullanarak IoT Edge işlevinin nasıl oluşturulduğu gösterilir.
1. Visual Studio Code'u açın.
2. VS Code tümleşik terminalini açmak için **Görünüm** > **Tümleşik Terminal**'i seçin.
3. Dotnet'te **AzureIoTEdgeFunction** şablonunu yüklemek (veya güncelleştirmek) için, tümleşik terminalde aşağıdaki komutu çalıştırın:

    ```cmd/sh
    dotnet new -i Microsoft.Azure.IoT.Edge.Function
    ```
2. Yeni modül için bir proje oluşturun. Aşağıdaki komut, geçerli çalışma klasöründe **FilterFunction** proje klasörünü oluşturur:

    ```cmd/sh
    dotnet new aziotedgefunction -n FilterFunction
    ```

3. **Dosya** > **Klasör Aç**'ı seçin, **FilterFunction** klasörüne gidin ve VS Code'da projeyi açın.
4. VS Code gezgininde **EdgeHubTrigger-Csharp** klasörünü genişletin, sonra da **run.csx** dosyasını açın.
5. Dosyanın içeriğini aşağıdaki kodla değiştirin:

   ```csharp
   #r "Microsoft.Azure.Devices.Client"
   #r "Newtonsoft.Json"

   using System.IO;
   using Microsoft.Azure.Devices.Client;
   using Newtonsoft.Json;

   // Filter messages based on the temperature value in the body of the message and the temperature threshold value.
   public static async Task Run(Message messageReceived, IAsyncCollector<Message> output, TraceWriter log)
   {
        const int temperatureThreshold = 25;
        byte[] messageBytes = messageReceived.GetBytes();
        var messageString = System.Text.Encoding.UTF8.GetString(messageBytes);

        if (!string.IsNullOrEmpty(messageString))
        {
            // Get the body of the message and deserialize it
            var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);

            if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
            {
                // Send the message to the output as the temperature value is greater than the threashold
                var filteredMessage = new Message(messageBytes);
                // Copy the properties of the original message into the new Message object
                foreach (KeyValuePair<string, string> prop in messageReceived.Properties)
                {
                    filteredMessage.Properties.Add(prop.Key, prop.Value);
                }
                // Add a new property to the message to indicate it is an alert
                filteredMessage.Properties.Add("MessageType", "Alert");
                // Send the message        
                await output.AddAsync(filteredMessage);
                log.Info("Received and transferred a message with temperature above the threshold");
            }
        }
    }

    //Define the expected schema for the body of incoming messages
    class MessageBody
    {
        public Machine machine {get;set;}
        public Ambient ambient {get; set;}
        public string timeCreated {get; set;}
    }
    class Machine
    {
       public double temperature {get; set;}
       public double pressure {get; set;}         
    }
    class Ambient
    {
       public double temperature {get; set;}
       public int humidity {get; set;}         
    }
   ```

11. Dosyayı kaydedin.

## <a name="publish-a-docker-image"></a>Docker görüntüsünü yayımlama

1. Docker görüntüsü oluşturun.
    1. VS Code gezgininde **Docker** klasörünü genişletin. Ardından kapsayıcı platformunuzun klasörünü (**linux-x64** veya **windows-nano**) genişletin. 
    2. **Dockerfile** dosyasına sağ tıklayın ve **IoT Edge modülü Docker görüntüsü oluştur**'a tıklayın. 
    3. **FilterFunction** proje klasörüne gidin ve **EXE_DIR Olarak Klasör Seç**'e tıklayın. 
    4. VS Code penceresinin en üstündeki açılan metin kutusuna görüntü adını girin. Örneğin: `<your container registry address>/filterfunction:latest`. Kapsayıcı kayıt defteri adresi, kayıt defterinizden kopyaladığınız oturum açma sunucusuyla aynıdır. `<your container registry name>.azurecr.io` biçiminde olmalıdır.
 
4. Docker'da oturum açın. Tümleşik terminalde aşağıdaki komutu girin: 

   ```csh/sh
   docker login -u <username> -p <password> <Login server>
   ```
        
   Bu komutta kullanılacak kullanıcı adını, parolayı ve oturum açma sunucusunu bulmak için [Azure portalına] (https://portal.azure.com)) gidin. **Tüm kaynaklar**'da, Azure kapsayıcı kayıt defterinizin kutucuğuna tıklayarak özelliklerini açın ve **Erişim tuşları**'na tıklayın. **Kullanıcı adı**, **Parola** ve **Oturum açma sunucusu** alanlarındaki değerleri kopyalayın. 

3. Görüntüyü Docker deponuza koyun. **Görünüm** > **Komut Paleti...** öğesini seçin ve ardından **Edge: IoT Edge modülü Docker görüntüsünü gönder** için arama yapın.
4. Açılan metin kutusunda, 1.d adımında kullandığınız görüntü adının aynısını girin.

## <a name="add-registry-credentials-to-your-edge-device"></a>Kayıt defteri kimlik bilgilerini Edge cihazınıza ekleme
Kayıt defterinizin kimlik bilgilerini, Edge cihazınızı çalıştırdığınız bilgisayarın Edge çalışma zamanına ekleyin. Bu, kapsayıcıyı çekmek için çalışma zamanı erişimi sağlar. 

- Windows için şu komutu çalıştırın:
    
    ```cmd/sh
    iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

- Linux için şu komutu çalıştırın:
    
    ```cmd/sh
    sudo iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

## <a name="run-the-solution"></a>Çözümü çalıştırın

1. **Azure portalında** IoT hub'ınıza gidin.
2. **IoT Edge (önizleme)** sayfasına gidip IoT Edge cihazınızı seçin.
1. **Modülleri Ayarlama**'yı seçin. 
2. Cihazınıza **tempSensor** modülünü zaten dağıttıysanız, otomatik olarak doldurulabilir. Aksi takdirde, eklemek için aşağıdaki adımları izleyin:
    1. **IoT Edge Modülü Ekle**'yi seçin.
    2. **Ad** alanına `tempSensor` girin.
    3. **Görüntü URI'si** alanına `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview` girin.
    4. Diğer ayarları değiştirmeden bırakın ve **Kaydet**'e tıklayın.
1. **filterFunction** modülünü ekleyin.
    1. **IoT Edge Modülü Ekle**'yi yeniden seçin.
    2. **Ad** alanına `filterFunction` girin.
    3. **Görüntü URI'si** alanına görüntünüzün adresini girin; örneğin `<your container registry address>/filtermodule:0.0.1-amd64`. Tam görüntü adresi, önceki bölümde bulunabilir.
    74. **Kaydet**’e tıklayın.
2. **İleri**’ye tıklayın.
3. **Rota Belirtme** adımında, aşağıdaki JSON’u metin kutusuna kopyalayın. İlk rota, iletileri "input1" uç noktası yoluyla sıcaklık algılayıcısından filtre modülüne taşır. İkinci rota, iletileri filtre modülünden IoT Hub'a taşır. Bu rotada `$upstream`, Edge Hub'a iletileri IoT Hub'a göndermesini bildiren özel bir hedeftir. 

    ```json
    {
       "routes":{
          "sensorToFilter":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filterFunction/inputs/input1\")",
          "filterToIoTHub":"FROM /messages/modules/filterFunction/outputs/* INTO $upstream"
       }
    }
    ```

4. **İleri**’ye tıklayın.
5. **Şablonu Gözden Geçirme** adımında **Gönder**’e tıklayın. 
6. IoT Edge cihaz ayrıntıları sayfasına dönün ve **Yenile**’ye tıklayın. **tempSensor** modülü ve **IoT Edge çalışma zamanı** ile birlikte çalışan yeni **filterfunction** modülünü görürsünüz. 

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

IoT Edge cihazınızdan IoT hub'ınıza cihazdan buluta gönderilen iletileri izlemek için:
1. Azure IoT Toolkit uzantısını IoT hub'ınızın bağlantı dizesiyle yapılandırın: 
    1. Azure portalında, IoT hub'ınıza gidin ve **Paylaşılan erişim ilkeleri**'ni seçin. 
    2. **iothubowner** öğesini seçin ve ardından **Bağlantı dizesi - birincil anahtar**'ın değerini kopyalayın.
    1. VS Code gezgininde **IOT HUB CİHAZLARI**'na ve ardından **...** düğmesine tıklayın. 
    1. **IoT Hub Bağlantı Dizesini Ayarla**'yı seçin ve açılan pencereye IoT Hub bağlantı dizesini girin. 

1. IoT hub'da gelen verileri izlemek için, **Görünüm** > **Komut Paleti...** öğesini seçin ve **IoT: D2C iletisini izlemeye başlama** için arama yapın. 
2. Verileri izlemeyi durdurmak için, Komut Paleti'nde **IoT: D2C iletisini izlemeyi durdur** komutunu kullanın. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, IoT Edge cihazınız tarafından oluşturulan ham verileri filtreleme kodunu içeren bir Azure İşlevi oluşturdunuz. Azure IoT Edge'i keşfetmeye devam etmek için, IoT Edge cihazının nasıl ağ geçidi olarak kullanıldığını öğrenin. 

> [!div class="nextstepaction"]
> [IoT Edge ağ geçidi cihazı kullanma](how-to-create-transparent-gateway.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
