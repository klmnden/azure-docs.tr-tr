---
title: Azure IoT Edge C# modülü | Microsoft Belgeleri
description: C# kodu ile bir IoT Edge modülü oluşturma ve bir Edge cihazına dağıtma
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 03/14/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 1da3a246a2ad33a4563f491058f5d4d115f3954d
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34631080"
---
# <a name="develop-and-deploy-a-c-iot-edge-module-to-your-simulated-device---preview"></a>C# IoT Edge modülü geliştirme ve sanal cihazınıza dağıtma - önizleme

İş mantığınızı uygulayan kodu doğrudan IoT Edge cihazlarınıza dağıtmak için IoT Edge modüllerini kullanabilirsiniz. Bu öğreticide, algılayıcı verilerini filtreleyen bir IoT Edge modülü oluşturma ve dağıtma işlemlerinin adımları açıklanmaktadır. [Windows][lnk-tutorial1-win]'ta veya [Linux][lnk-tutorial1-lin]'ta sanal bir cihaza Azure IoT Edge dağıtma öğreticilerinde oluşturduğunuz sanal IoT Edge cihazınızı kullanacaksınız. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:    

> [!div class="checklist"]
> * Visual Studio Code kullanarak .NET Core 2.0'a dayalı bir IoT Edge modülü oluşturma
> * Visual Studio Code ve Docker kullanarak bir Docker görüntüsü oluşturma ve bunu kayıt defterinizde yayımlama 
> * Modüle IoT Edge cihazınıza dağıtma
> * Oluşturulan verileri görüntüleme


Bu öğreticide oluşturacağınız IoT Edge modülü, cihazınız tarafından oluşturulan sıcaklık verilerini filtreler. İletileri yalnızca sıcaklık belirtilen bir eşiğin üzerindeyse yukarı yönde gönderir. Bu tür bir analiz, buluta iletilen ve bulutta depolanan veri miktarını azaltmak için yararlıdır. 

## <a name="prerequisites"></a>Ön koşullar

* Hızlı başlangıçta veya ilk öğreticide oluşturduğunuz Azure IoT Edge cihazı.
* IoT Edge cihazı için birincil anahtar bağlantı dizesi.  
* [Visual Studio Code](https://code.visualstudio.com/). 
* [Visual Studio Code için Azure IoT Edge uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
* [Visual Studio Code için C# (OmniSharp tarafından desteklenen) uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). 
* Visual Studio Code olan bilgisayarda [Docker](https://docs.docker.com/engine/installation/). Community Edition (CE) bu öğretici için yeterlidir. 
* [.NET Core 2.0 SDK](https://www.microsoft.com/net/core#windowscmd). 

## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma
Bu öğreticide modül hazırlamak ve dosyalardan bir **kapsayıcı görüntüsü** oluşturmak için VS Code için Azure IoT Edge uzantısını kullanırsınız. Ardından bu görüntüyü, görüntülerinizin depolandığı ve yönetildiği **kayıt defterine** gönderirsiniz. Son olarak, görüntünüzü IoT Edge cihazınızda çalıştırmak üzere kayıt defterinizden dağıtırsınız.  

Bu öğretici için Docker ile uyumlu herhangi bir kayıt defteri kullanabilirsiniz. Bulutta sağlanan iki popüler Docker kayıt defteri hizmeti [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) ve [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)'dır. Bu öğreticide Azure Container Registry kullanılır. 

1. [Azure portalında](https://portal.azure.com), **Kaynak oluştur** > **Kapsayıcılar** > **Azure Container Registry**'yi seçin.
2. Kayıt defterinize bir ad verin, abonelik seçin, kaynak grubu seçin ve SKU'yu **Temel** olarak ayarlayın. 
3. **Oluştur**’u seçin.
4. Kapsayıcı kayıt defteriniz oluşturulduktan sonra, bu kayıt defterine gidin ve **Erişim anahtarları**'nı seçin. 
5. **Yönetici kullanıcı** ayarını **Etkinleştir**'e getirin.
6. **Oturum açma sunucusu**, **Kullanıcı adı** ve **Parola** değerlerini kopyalayın. Bu değerleri öğreticide ileride Docker görüntüsünü kayıt defterinizde yayımladığınızda ve kayıt defteri kimlik bilgilerini Edge çalışma zamanına eklediğinizde kullanacaksınız. 

## <a name="create-an-iot-edge-module-project"></a>IoT Edge modülü projesi oluşturma
Aşağıdaki adımlarda, Visual Studio Code ve Azure IoT Edge uzantısı kullanılarak .NET Core 2.0'a dayalı bir IoT Edge modülünün nasıl oluşturulduğu gösterilmektedir.
1. Visual Studio Code'da, VS Code ile tümleşik terminali açmak için **Görünüm** > **Tümleşik Terminal**'i seçin.
2. **AzureIoTEdgeModule** şablonunu dotnet'e yüklemek (veya dotnet'te güncelleştirmek) için tümleşik terminale şu kodu girin:

    ```cmd/sh
    dotnet new -i Microsoft.Azure.IoT.Edge.Module
    ```

3. Yeni modül için bir proje oluşturun. Aşağıdaki komut, kapsayıcı deponuzla birlikte **FilterModule** proje klasörünü oluşturur. Azure kapsayıcı kayıt defterini kullanıyorsanız ikinci parametre `<your container registry name>.azurecr.io` biçiminde olmalıdır. Geçerli çalışma klasörüne aşağıdaki komutu girin:

    ```cmd/sh
    dotnet new aziotedgemodule -n FilterModule -r <your container registry address>/filtermodule
    ```
 
4. **Dosya** > **Klasör Aç**'ı seçin.
5. **FilterModule** klasörüne gidin ve **Klasör Seç**'e tıklayarak projeyi VS Code'da açın.
6. VS Code gezgininde, açmak için **Program.cs** dosyasını tıklatın.

   ![Program.cs dosyasını açma][1]

7. **FilterModule** ad alanının en üst kısmına daha sonra kullanılan türler için üç `using` deyimi yazın:

    ```csharp
    using System.Collections.Generic;     // for KeyValuePair<>
    using Microsoft.Azure.Devices.Shared; // for TwinCollection
    using Newtonsoft.Json;                // for JsonConvert
    ```

8. **Program** sınıfına `temperatureThreshold` değişkenini ekleyin. Bu değişken, IoT Hub'a veri gönderilmesi için ölçülen sıcaklığın aşması gereken değeri ayarlar. 

    ```csharp
    static int temperatureThreshold { get; set; } = 25;
    ```

9. **Program** sınıfına `MessageBody`, `Machine` ve `Ambient` sınıflarını ekleyin. Bu sınıflar gelen iletilerin gövdesi için beklenen şemayı tanımlar.

    ```csharp
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

10. **Init** yöntemindeki kod, bir **DeviceClient** nesnesi oluşturur ve yapılandırır. Bu nesne, modülün ileti göndermek ve almak için yerel Azure IoT Edge çalışma zamanına bağlanmasını sağlar. **Init** yönteminde kullanılan bağlantı dizesi, modüle IoT T Edge çalışma zamanı tarafından sağlanır. Kod, **DeviceClient**'ı oluşturduktan sonra modül ikizinin istenen özelliklerinden TemperatureThreshold'u okur ve IoT Edge hub'ından **input1** uç noktası aracılığıyla iletiler almak için bir geri çağrı kaydeder. `SetInputMessageHandlerAsync` yöntemini yeni bir yöntemle değiştirin ve istenen özellik güncelleştirmeleri için bir `SetDesiredPropertyUpdateCallbackAsync` yöntemi ekleyin. Bu değişikliği yapmak için, **Init** yönteminin son satırının aşağıdaki kod ile değiştirin:

    ```csharp
    // Register callback to be called when a message is received by the module
    // await ioTHubModuleClient.SetImputMessageHandlerAsync("input1", PipeMessage, iotHubModuleClient);

    // Read TemperatureThreshold from Module Twin Desired Properties
    var moduleTwin = await ioTHubModuleClient.GetTwinAsync();
    var moduleTwinCollection = moduleTwin.Properties.Desired;
    try {
        temperatureThreshold = moduleTwinCollection["TemperatureThreshold"];
    } catch(ArgumentOutOfRangeException e) {
        Console.WriteLine("Property TemperatureThreshold not exist");
    }

    // Attach callback for Twin desired properties updates
    await ioTHubModuleClient.SetDesiredPropertyUpdateCallbackAsync(onDesiredPropertiesUpdate, null);

    // Register callback to be called when a message is received by the module
    await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FilterMessages, ioTHubModuleClient);
    ```

11. **Program** sınıfına `onDesiredPropertiesUpdate` yöntemini ekleyin. Bu yöntem modül ikizinin istenen özellikleri üzerinde yapılan güncelleştirmeleri alır ve **temperatureThreshold** değişkenini buna göre güncelleştirir. Tüm modüllerin, doğrudan buluttan bir modülün içinde çalışan kodu yapılandırmanıza izin veren kendi modül ikizi vardır.

    ```csharp
    static Task onDesiredPropertiesUpdate(TwinCollection desiredProperties, object userContext)
    {
        try
        {
            Console.WriteLine("Desired property change:");
            Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));

            if (desiredProperties["TemperatureThreshold"]!=null)
                temperatureThreshold = desiredProperties["TemperatureThreshold"];

        }
        catch (AggregateException ex)
        {
            foreach (Exception exception in ex.InnerExceptions)
            {
                Console.WriteLine();
                Console.WriteLine("Error when receiving desired property: {0}", exception);
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error when receiving desired property: {0}", ex.Message);
        }
        return Task.CompletedTask;
    }
    ```

12. `PipeMessage` yöntemini `FilterMessages` yöntemiyle değiştirin. Modül IoT Edge hub'ından bir ileti aldığında bu yöntem çağrılır. Modül ikizi aracılığıyla ayarlanan sıcaklık eşiğinin altındaki sıcaklıkları rapor eden iletileri filtreler. Ayrıca iletiye **MessageType** özelliğini ekleyip değerini **Alert** olarak ayarlar. 

    ```csharp
    static async Task<MessageResponse> FilterMessages(Message message, object userContext)
    {
        var counterValue = Interlocked.Increment(ref counter);

        try {
            DeviceClient deviceClient = (DeviceClient)userContext;

            var messageBytes = message.GetBytes();
            var messageString = Encoding.UTF8.GetString(messageBytes);
            Console.WriteLine($"Received message {counterValue}: [{messageString}]");

            // Get message body
            var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);

            if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
            {
                Console.WriteLine($"Machine temperature {messageBody.machine.temperature} " +
                    $"exceeds threshold {temperatureThreshold}");
                var filteredMessage = new Message(messageBytes);
                foreach (KeyValuePair<string, string> prop in message.Properties)
                {
                    filteredMessage.Properties.Add(prop.Key, prop.Value);
                }

                filteredMessage.Properties.Add("MessageType", "Alert");
                await deviceClient.SendEventAsync("output1", filteredMessage);
            }

            // Indicate that the message treatment is completed
            return MessageResponse.Completed;
        }
        catch (AggregateException ex)
        {
            foreach (Exception exception in ex.InnerExceptions)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", exception);
            }
            // Indicate that the message treatment is not completed
            var deviceClient = (DeviceClient)userContext;
            return MessageResponse.Abandoned;
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
            // Indicate that the message treatment is not completed
            DeviceClient deviceClient = (DeviceClient)userContext;
            return MessageResponse.Abandoned;
        }
    }
    ```

13. Bu dosyayı kaydedin.

## <a name="create-a-docker-image-and-publish-it-to-your-registry"></a>Docker görüntüsü oluşturma ve bunu kayıt defterinize yayımlama

1. VS Code tümleşik terminale aşağıdaki komutu girerek Docker’da oturum açın: 
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Bu komutta kullanılacak kullanıcı adını, parolayı ve oturum açma sunucusunu bulmak için [Azure portalına] (https://portal.azure.com)) gidin. **Tüm kaynaklar**'da, Azure kapsayıcı kayıt defterinizin kutucuğuna tıklayarak özelliklerini açın ve **Erişim tuşları**'na tıklayın. **Kullanıcı adı**, **Parola** ve **Oturum açma sunucusu** alanlarındaki değerleri kopyalayın. 

2. VS Code gezgininde **module.json** dosyasına sağ tıklayın ve **IoT Edge modülü Docker görüntüsü derle ve gönder** seçeneğine tıklayın. VS Code penceresinin açılır kutusunda, Linux kapsayıcı için **amd64** ve Windows kapsayıcı için **windows-amd64** olacak şekilde kapsayıcı platformunuzu seçin. VS Code daha sonra kodunuzu derler, `FilterModule.dll` öğesini kapsayıcıya alır ve belirttiğiniz kapsayıcı kayıt defterine gönderir.


3. VS Code tümleşik terminalinde etiketle tam kapsayıcı görüntü adresini alabilirsiniz. Derleme ve gönderme tanımı hakkında daha fazla bilgi için `module.json` dosyasına bakabilirsiniz.

## <a name="add-registry-credentials-to-edge-runtime"></a>Edge çalışma zamanına kayıt defteri kimlik bilgileri ekleme
Kayıt defterinizin kimlik bilgilerini, Edge cihazınızı çalıştırdığınız bilgisayarın Edge çalışma zamanına ekleyin. Bu kimlik bilgileri, çalışma zamanına kapsayıcıyı çekmek için erişim sağlar. 

- Windows için şu komutu çalıştırın:
    
    ```cmd/sh
    iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

- Linux için şu komutu çalıştırın:
    
    ```cmd/sh
    sudo iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

## <a name="run-the-solution"></a>Çözümü çalıştırın

1. [Azure portalında](https://portal.azure.com) IoT hub'ınıza gidin.
2. **IoT Edge (önizleme)** sayfasına gidip IoT Edge cihazınızı seçin.
3. **Modülleri Ayarlama**'yı seçin. 
4. **tempSensor** modülünün otomatik olarak doldurulduğundan emin olun. Doldurulmamışsa, eklemek için aşağıdaki adımları kullanın:
    1. **IoT Edge Modülü Ekle**'yi seçin.
    2. **Ad** alanına `tempSensor` girin.
    3. **Görüntü URI'si** alanına `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview` girin.
    4. Diğer ayarları değiştirmeden bırakın ve **Kaydet**'e tıklayın.
5. Önceki bölümlerde oluşturduğunuz **filterModule** modülünü ekleyin. 
    1. **IoT Edge Modülü Ekle**'yi seçin.
    2. **Ad** alanına `filterModule` girin.
    3. **Görüntü URI'si** alanına görüntünüzün adresini girin; örneğin `<your container registry address>/filtermodule:0.0.1-amd64`. Tam görüntü adresi, önceki bölümde bulunabilir.
    4. Modül ikizini düzenleyebilmeniz için **Etkinleştir** kutusunu işaretleyin. 
    5. Modül ikizinin metin kutusundaki JSON'ı aşağıdaki JSON ile değiştirin: 

        ```json
        {
           "properties.desired":{
              "TemperatureThreshold":25
           }
        }
        ```
 
    6. **Kaydet**’e tıklayın.
6. **İleri**’ye tıklayın.
7. **Rota Belirtme** adımında, aşağıdaki JSON’u metin kutusuna kopyalayın. Modüller tüm iletileri Edge çalışma zamanına yayımlar. Çalışma zamanındaki bildirim kuralları, iletilerin akış yönünü tanımlar. Bu öğreticide iki yön gerekir. Birinci yön, sıcaklık algılayıcısından gelen iletileri, **FilterMessages** işleyicisi ile yapılandırdığınız uç nokta olan "input1" aracılığıyla filtre modülüne taşır. İkinci rota, iletileri filtre modülünden IoT Hub'a taşır. Bu rotada `upstream`, Edge Hub'a iletileri IoT Hub'a göndermesini bildiren özel bir hedeftir. 

    ```json
    {
       "routes":{
          "sensorToFilter":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filterModule/inputs/input1\")",
          "filterToIoTHub":"FROM /messages/modules/filterModule/outputs/output1 INTO $upstream"
       }
    }
    ```

8. **İleri**’ye tıklayın.
9. **Şablonu Gözden Geçirme** adımında **Gönder**’e tıklayın. 
10. IoT Edge cihaz ayrıntıları sayfasına dönün ve **Yenile**’ye tıklayın. Yeni **filtermodule** modülünün **tempSensor** modülü ve **IoT Edge çalışma zamanı** ile birlikte çalıştığını görmeniz gerekir. 

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

IoT Edge cihazınızdan IoT hub'ınıza cihazdan buluta gönderilen iletileri izlemek için:
1. Azure IoT Toolkit uzantısını IoT hub'ınızın bağlantı dizesiyle yapılandırın: 
    1. **Görünüm** > **Gezgin**'i seçerek VS Code gezginini açın. 
    2. Gezginde **IOT HUB CİHAZLARI**'na ve ardından **...** düğmesine tıklayın. **IoT Hub Bağlantı Dizesini Ayarla** seçeneğine tıklayın ve açılır pencerede IoT Edge cihazınızın bağlandığı IoT hub'ının bağlantı dizesini girin. 

        Bağlantı dizesini bulmak için, Azure portalda IoT hub'ınızın kutucuğuna, sonra **Paylaşılan erişim ilkeleri**'ne tıklayın. **Paylaşılan erişim ilkeleri** penceresinde, **iothubowner** ilkesine tıklayın ve **iothubowner** penceresindeki IoT Hub bağlantı dizesini kopyalayın.   

2. IoT hub'a gelen verileri izlemek için, **Görünüm** > **Komut Paleti**'ni seçin ve **IoT: D2C iletisini izlemeye başla** menü komutunu arayın. 
3. Verileri izlemeyi durdurmak için **IoT: D2C iletisini izlemeyi durdur** menü komutunu kullanın. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide IoT Edge cihazınız tarafından üretilen ham verileri filtrelemek için kod içeren bir IoT Edge modülü oluşturdunuz. Azure IoT Edge'in verileri iş içgörüsüne dönüştürmenize yardımcı olabilecek diğer yollar hakkında bilgi edinmek için aşağıdaki öğreticilerden birine devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Azure Function'ı modül olarak dağıtma](tutorial-deploy-function.md)
> [Azure Stream Analytics'i modül olarak dağıtma](tutorial-deploy-stream-analytics.md)


<!-- Links -->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md

<!-- Images -->
[1]: ./media/tutorial-csharp-module/programcs.png
[2]: ./media/tutorial-csharp-module/build-module.png
[3]: ./media/tutorial-csharp-module/docker-os.png
