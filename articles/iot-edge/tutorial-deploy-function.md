---
title: "Azure IOT kenarıyla Azure işlevi dağıtma | Microsoft Docs"
description: "Sınır cihazı bir modüle olarak Azure işlevi dağıtma"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: v-jamebr
ms.date: 11/15/2017
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 1dfe46d307a076ae02362c4bba292602001ed915
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2017
---
# <a name="deploy-azure-function-as-an-iot-edge-module---preview"></a>Bir IOT kenar modül Azure işlevi dağıtma - Önizleme
Azure işlevleri, iş mantığınızı IOT sınır cihazları için doğrudan uygulayan kod dağıtmak için kullanabilirsiniz. Bu öğreticide, oluşturma ve dağıtma algılayıcı verileri üzerinde bir sanal cihaz dağıtmak Azure IOT kenarına oluşturduğunuz sanal IOT sınır aygıtında filtreler bir Azure işlevi aracılığıyla açıklanmaktadır [Windows] [ lnk-tutorial1-win]veya [Linux] [ lnk-tutorial1-lin] öğreticileri. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:     

> [!div class="checklist"]
> * Bir Azure işlevi oluşturmak için Visual Studio Code kullanın
> * Docker görüntü oluşturma ve kayıt defterine yayımlama için VS Code ve Docker kullanın 
> * Modül IOT kenar Cihazınızı dağıtma
> * Oluşturulan görünüm verileri


Bu öğreticide oluşturduğunuz Azure işlevi cihazınız tarafından oluşturulan sıcaklık verileri filtreler ve sıcaklık belirtilen eşiğin üzerinde yalnızca upstream iletileri Azure IOT Hub'ına gönderir. 

## <a name="prerequisites"></a>Ön koşullar

* Hızlı Başlangıç ya da önceki öğreticide oluşturduğunuz Azure IOT sınır cihazı.
* [Visual Studio Code](https://code.visualstudio.com/). 
* [C# Visual Studio Code (OmniSharp tarafından desteklenen) uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). 
* [Visual Studio Code için Azure IOT kenar uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
* [Docker](https://docs.docker.com/engine/installation/). Bu öğretici için yeterli platformunuzun Community Edition (CE) olur. 
* [.NET 2.0 SDK çekirdek](https://www.microsoft.com/net/core#windowscmd). 

## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma
Bu öğreticide, bir modülü oluşturmak ve oluşturmak için Azure IOT kenar uzantısı VS Code için kullanmak bir **kapsayıcı görüntü** dosyalarından. Bu görüntü için anında sonra bir **kayıt defteri** depolar ve resimlerinizi yönetir. Son olarak, IOT kenar aygıtınızda çalıştırmak için kayıt defterinden görüntünüzü dağıtın.  

Bu öğretici için Docker uyumlu kayıt kullanabilirsiniz. İki popüler Docker kayıt defteri hizmetlerinin kullanılabilir bulutta [Azure kapsayıcı kayıt defteri](https://docs.microsoft.com/azure/container-registry/) ve [Docker hub'a](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). Bu öğretici, Azure kapsayıcı kayıt defteri kullanır. 

1. İçinde [Azure portal](https://portal.azure.com)seçin **kaynak oluşturma** > **kapsayıcıları** > **Azure kapsayıcı kayıt defteri** .
2. Kayıt bir ad verin, bir abonelik seçin, bir kaynak grubu seçin ve SKU kümesine **temel**. 
3. **Oluştur**’u seçin.
4. Kapsayıcı kaydınız oluşturulduktan sonra kendisine gidin ve seçin **erişim anahtarları**. 
5. İki durumlu **yönetici kullanıcı** için **etkinleştirmek**.
6. Değerleri kopyalamak **oturum açma sunucusu**, **kullanıcıadı**, ve **parola**. Öğreticide daha sonra bu değerleri kullanacaksınız. 

## <a name="create-a-function-project"></a>Bir işlev projesi oluşturma
Aşağıdaki adımlar, Visual Studio Code ile Azure IOT kenar uzantısı bir IOT kenar işlevi oluşturma gösterir.
1. Visual Studio Code'da açın.
2. VS Code tümleşik Terminali açın, seçin **Görünüm** > **tümleşik Terminal**.
3. Yüklemek (veya güncelleştirmek için) **AzureIoTEdgeFunction** şablonunda dotnet, tümleşik terminale aşağıdaki komutu çalıştırın:

    ```cmd/sh
    dotnet new -i Microsoft.Azure.IoT.Edge.Function
    ```
2. Yeni modül için bir proje oluşturun. Aşağıdaki komut proje klasörünü oluşturur **FilterFunction**, geçerli çalışma klasörü içinde:

    ```cmd/sh
    dotnet new aziotedgefunction -n FilterFunction
    ```

3. Seçin **dosya** > **klasörünü Aç**, ardından gözatın **FilterFunction** klasörü ve VS code'da Proje Aç.
4. VS Code Explorer'da genişletin **EdgeHubTrigger Csharp** klasörünü açın **run.csx** dosya.
5. Dosyasının içeriğini aşağıdaki kodla değiştirin:

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

## <a name="publish-a-docker-image"></a>Docker görüntü yayımlama

1. Docker görüntüsünü oluşturabilirsiniz.
    1. VS Code Explorer'da genişletin **Docker** klasör. Kapsayıcı platformunuz için klasör ya da genişletin **linux x64** veya **windows nano**. 
    2. Sağ **Dockerfile** dosya ve tıklayın **yapı IOT kenar modülü Docker görüntü**. 
    3. Gidin **FilterFunction** proje klasörünü ve tıklatın **EXE_DIR olarak klasör seç**. 
    4. VS Code pencerenin üstündeki açılır metin kutusuna görüntü adı girin. Örneğin: `<your container registry address>/filterfunction:latest`. Kapsayıcı kayıt defteri adresi kayıt defterinden kopyaladığınız oturum açma sunucusu ile aynıdır. Biçiminde olmalıdır `<your container registry name>.azurecr.io`.
 
4. Docker için oturum açın. Tümleşik terminale aşağıdaki komutu girin: 

   ```csh/sh
   docker login -u <username> -p <password> <Login server>
   ```
        
   Kullanıcı adı, bu komutu kullanmak için parola ve oturum açma sunucusu bulmak için [Azure portalına] gidin (https://portal.azure.com). Gelen **tüm kaynakları**, Azure kapsayıcı kaydınız özelliklerini açın ve ardından kutucuğa tıklayın **erişim anahtarları**. Değerleri kopyalamak **kullanıcıadı**, **parola**, ve **oturum açma sunucusu** alanları. 

3. Görüntü Docker deponuza iletin. Seçin **Görünüm** > **komutu palet...**  için arama **kenar: anında IOT kenar modülü Docker görüntü**.
4. Adımda kullanılan aynı görüntü adı açılır metin kutusuna girin 1.d.

## <a name="add-registry-credentials-to-your-edge-device"></a>Kayıt defteri kimlik bilgilerini kenar Cihazınızı ekleyin
Sınır cihazı çalıştırdığınız bilgisayarda kenar çalışma zamanı kayıt için kimlik bilgilerini ekleyin. Bu kapsayıcı çıkarmak için çalışma zamanı erişimi verir. 

- Windows için aşağıdaki komutu çalıştırın:
    
    ```cmd/sh
    iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

- Linux için aşağıdaki komutu çalıştırın:
    
    ```cmd/sh
    sudo iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

## <a name="run-the-solution"></a>Çözümü çalıştırın

1. İçinde **Azure portal**, IOT hub'ına gidin.
2. Git **IOT kenar (Önizleme)** ve IOT sınır cihazı seçin.
1. Seçin **ayarlamak modülleri**. 
2. Zaten dağıtılmışsa, **tempSensor** modülü bu cihaz için bunu otomatik olarak doldurulmuş. Değilse, bunu eklemek için aşağıdaki adımları izleyin:
    1. Seçin **IOT kenar Modül Ekle**.
    2. İçinde **adı** alanına, `tempSensor`.
    3. İçinde **görüntü URI** alanına, `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`.
    4. Diğer ayarları değiştirmeden bırakın ve tıklayın **kaydetmek**.
1. Ekleme **filterFunction** modülü.
    1. Seçin **IOT kenar Modül Ekle** yeniden.
    2. İçinde **adı** alanına, `filterFunction`.
    3. İçinde **görüntü** alan, görüntü adresinizi girin; örneğin `<docker registry address>/filterfunction:latest`.
    74. **Kaydet** düğmesine tıklayın.
2. **İleri**’ye tıklayın.
3. İçinde **belirtin yollar** adım, JSON altındaki metin kutusuna Kopyala. İlk yol sıcaklık algılayıcısı iletilerden "input1" uç noktası aracılığıyla filtresi modülü için taşımaları. İkinci yol filtresi modülü gelen iletileri IOT Hub'ına taşımaları. Bu rotadaki `$upstream` kenar Hub'ın IOT Hub'ına iletileri göndermek için söyler özel bir hedef. 

    ```json
    {
       "routes":{
          "sensorToFilter":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filterFunction/inputs/input1\")",
          "filterToIoTHub":"FROM /messages/modules/filterFunction/outputs/* INTO $upstream"
       }
    }
    ```

4. **İleri**’ye tıklayın.
5. İçinde **gözden geçirme şablonu** adımını, **gönderme**. 
6. IOT Edge cihaz ayrıntıları sayfasına dönün ve **yenileme**. Yeni görmelisiniz **filterfunction** modülü ile birlikte çalışan **tempSensor** modülü ve **IOT kenar çalışma zamanı**. 

## <a name="view-generated-data"></a>Oluşturulan görünüm verileri

Cihaz bulut iletilerini IOT kenar cihazınızın IOT hub'ına gönderilen izlemek için:
1. IOT hub'ınız için bağlantı dizesi ile Azure IOT Araç Seti uzantısı yapılandırın: 
    1. Azure portalında, IOT hub'ına gidin ve seçin **paylaşılan erişim ilkeleri**. 
    2. Seçin **iothubowner** değerini kopyalayın **bağlantı dize birincil anahtarı**.
    1. VS Code Explorer'da tıklatın **IOT HUB CİHAZLARI** ve ardından **...** . 
    1. Seçin **IOT Hub bağlantı dizesine ayarlamak** ve açılır penceresinde IOT Hub bağlantı dizesi girin. 

1. IOT hub'ına ulaşan verilerin izlemek için seçin **Görünüm** > **komutu palet...**  arayın ve **IOT: izleme D2C iletisi Başlat**. 
2. Veri izlemeyi durdurmak için kullanmak **IOT: D2C ileti izlemeyi durdurmak** komutu paletindeki komutu. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, IOT kenar cihazınız tarafından oluşturulan ham verileri filtrelemek için kodu içeren bir Azure işlevi oluşturuldu. Azure IOT kenar keşfetme tutmak için bir IOT sınır cihazı bir ağ geçidi olarak kullanmayı öğrenin. 

> [!div class="nextstepaction"]
> [Bir IOT sınır ağ geçidi cihazı oluşturma](how-to-create-transparent-gateway.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
