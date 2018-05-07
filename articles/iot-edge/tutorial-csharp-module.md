---
title: Azure IOT kenar C# modül | Microsoft Docs
description: C# kodu ile bir IOT kenar modül oluşturun ve bir uç cihazın dağıtma
services: iot-edge
keywords: ''
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 03/14/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 09e20d9a80b881075d9bb6be7d4daafc739340a1
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="develop-and-deploy-a-c-iot-edge-module-to-your-simulated-device---preview"></a>Geliştir ve C# IOT kenar modülünü sanal Cihazınızı dağıtmak - Önizleme

IOT kenar modülleri, iş mantığınızı IOT sınır cihazları için doğrudan uygulayan kod dağıtmak için kullanabilirsiniz. Bu öğreticide, oluşturma ve dağıtma algılayıcı verileri filtreler bir IOT kenar modülü aracılığıyla açıklanmaktadır. Bir sanal cihaz dağıtmak Azure IOT kenarına oluşturduğunuz sanal IOT sınır cihazı kullanacağınız [Windows] [ lnk-tutorial1-win] veya [Linux] [ lnk-tutorial1-lin]öğreticileri. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:    

> [!div class="checklist"]
> * .NET core 2.0 dayalı bir IOT kenar modülü oluşturmak için Visual Studio Code kullanın
> * Docker görüntü oluşturma ve kayıt defterine yayımlama için Visual Studio Code ve Docker kullanın 
> * Modüle IoT Edge cihazınıza dağıtma
> * Oluşturulan verileri görüntüleme


Bu öğreticide oluşturduğunuz IOT kenar modülü cihazınız tarafından oluşturulan sıcaklık verileri filtreler. Sıcaklık belirtilen eşiğin üzerindeyse, yalnızca iletileri upstream gönderir. Bu türde bir kenara çözümlemesini SDK'ya ve bulutta depolanan veri miktarını azaltmak için yararlıdır. 

## <a name="prerequisites"></a>Önkoşullar

* Hızlı Başlangıç ya da ilk öğreticide oluşturduğunuz Azure IOT sınır cihazı.
* IoT Edge cihazı için birincil anahtar bağlantı dizesi.  
* [Visual Studio Code](https://code.visualstudio.com/). 
* [Visual Studio Code için Azure IoT Edge uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
* [Visual Studio Code için C# (OmniSharp tarafından desteklenen) uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). 
* [Docker](https://docs.docker.com/engine/installation/) aynı bilgisayarda Visual Studio Code sahiptir. Community Edition (CE), Bu öğretici için yeterlidir. 
* [.NET Core 2.0 SDK](https://www.microsoft.com/net/core#windowscmd). 

## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma
Bu öğreticide modül hazırlamak ve dosyalardan bir **kapsayıcı görüntüsü** oluşturmak için VS Code için Azure IoT Edge uzantısını kullanırsınız. Ardından bu görüntüyü, görüntülerinizin depolandığı ve yönetildiği **kayıt defterine** gönderirsiniz. Son olarak, görüntünüzü IoT Edge cihazınızda çalıştırmak üzere kayıt defterinizden dağıtırsınız.  

Bu öğretici için Docker ile uyumlu herhangi bir kayıt defteri kullanabilirsiniz. Bulutta sağlanan iki popüler Docker kayıt defteri hizmeti [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) ve [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)'dır. Bu öğreticide Azure Container Registry kullanılır. 

1. [Azure portalında](https://portal.azure.com), **Kaynak oluştur** > **Kapsayıcılar** > **Azure Container Registry**'yi seçin.
2. Kayıt defterinize bir ad verin, abonelik seçin, kaynak grubu seçin ve SKU'yu **Temel** olarak ayarlayın. 
3. **Oluştur**’u seçin.
4. Kapsayıcı kayıt defteriniz oluşturulduktan sonra, bu kayıt defterine gidin ve **Erişim anahtarları**'nı seçin. 
5. **Yönetici kullanıcı** ayarını **Etkinleştir**'e getirin.
6. **Oturum açma sunucusu**, **Kullanıcı adı** ve **Parola** değerlerini kopyalayın. Kayıt defterine Docker görüntü yayımladığınızda ve kayıt defteri kimlik bilgileri kenar çalışma eklediğinizde, bu değerleri daha sonra öğreticide kullanırsınız. 

## <a name="create-an-iot-edge-module-project"></a>Bir IOT kenar modülü projesi oluşturma
Bir IOT kenar modülü oluşturmak için .NET tabanlı nasıl 2.0 kullanarak Visual Studio Code ve Azure IOT kenar uzantısı çekirdek aşağıdaki adımları gösterir.
1. Visual Studio kod seçin **Görünüm** > **tümleşik Terminal** VS Code tümleşik Terminali açın.
2. Tümleşik terminale yüklemek (veya güncelleştirmek için) aşağıdaki komutu girin **AzureIoTEdgeModule** dotnet şablonunda:

    ```cmd/sh
    dotnet new -i Microsoft.Azure.IoT.Edge.Module
    ```

3. Yeni modül için bir proje oluşturun. Aşağıdaki komut proje klasörünü oluşturur **FilterModule**, kapsayıcı deponuz ile. Azure kapsayıcı kayıt defterini kullanıyorsanız ikinci parametre `<your container registry name>.azurecr.io` biçiminde olmalıdır. Geçerli çalışma klasörüne aşağıdaki komutu girin:

    ```cmd/sh
    dotnet new aziotedgemodule -n FilterModule -r <your container registry address>/filtermodule
    ```
 
4. Seçin **dosya** > **klasörü açın**.
5. Gözat **FilterModule** klasörü ve tıklatın **klasörü seçin** proje VS Code'da açın.
6. VS Code Explorer'da tıklatın **Program.cs** açın.

   ![Program.cs açın][1]

7. Üstündeki **FilterModule** ad alanı, üç eklemek `using` deyimleri daha sonra kullanılan türleri için:

    ```csharp
    using System.Collections.Generic;     // for KeyValuePair<>
    using Microsoft.Azure.Devices.Shared; // for TwinCollection
    using Newtonsoft.Json;                // for JsonConvert
    ```

8. Ekleme `temperatureThreshold` değişkenini **Program** sınıfı. Bu değişken, IOT Hub'ına gönderilecek verileri sırayla ölçülen sıcaklık aşmalıdır değeri ayarlar. 

    ```csharp
    static int temperatureThreshold { get; set; } = 25;
    ```

9. Ekleme `MessageBody`, `Machine`, ve `Ambient` için sınıfları **Program** sınıfı. Bu sınıfların gelen ileti gövdesini beklenen şemasını tanımlayın.

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

10. İçinde **Init** yöntemi, kod oluşturur ve yapılandırır bir **DeviceClient** nesnesi. Bu nesne ileti gönderme ve alma için yerel Azure IOT kenar çalışma bağlanmak modülü sağlar. Kullanılan bağlantı dizesi **Init** yöntemi modülü IOT kenar çalışma zamanı tarafından sağlanmaktadır. Oluşturduktan sonra **DeviceClient**, kod modülü Twin'ın istenen özelliklerinden TemperatureThreshold okur ve IOT kenar hub'dan iletileri almak için bir geri çağırma kaydeder **input1**uç noktası. Değiştir `SetInputMessageHandlerAsync` yeni bir yöntemle ve ekleme bir `SetDesiredPropertyUpdateCallbackAsync` istenen özellikleri güncelleştirmeleri yöntemi. Bu değişikliği yapmak için son satırının yerini **Init** aşağıdaki kod ile yöntemi:

    ```csharp
    // Register callback to be called when a message is received by the module
    // await ioTHubModuleClient.SetImputMessageHandlerAsync("input1", PipeMessage, iotHubModuleClient);

    // Read TemperatureThreshold from Module Twin Desired Properties
    var moduleTwin = await ioTHubModuleClient.GetTwinAsync();
    var moduleTwinCollection = moduleTwin.Properties.Desired;
    try {
        temperatureThreshold = moduleTwinCollection["TemperatureThreshold"];
    } catch(ArgumentOutOfRangeException e) {
        Console.WriteLine("Proerty TemperatureThreshold not exist");
    }

    // Attach callback for Twin desired properties updates
    await ioTHubModuleClient.SetDesiredPropertyUpdateCallbackAsync(onDesiredPropertiesUpdate, null);

    // Register callback to be called when a message is received by the module
    await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FilterMessages, ioTHubModuleClient);
    ```

11. Ekleme `onDesiredPropertiesUpdate` yönteme **Program** sınıfı. Bu yöntem modülü twin İstenen özelliklerde güncelleştirmeleri alır ve güncelleştirmeleri **temperatureThreshold** eşleşecek şekilde değişkeni. Tüm modülleri doğrudan buluttan bir modül içinde çalışan kodu yapılandırmanıza olanak sağlayan kendi modülü çiftine sahip.

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

12. Değiştir `PipeMessage` yöntemiyle `FilterMessages` yöntemi. Modül bir ileti kenar IOT hub'ından aldığı zaman bu yöntem çağrılır. Modül twin ayarlamak sıcaklık eşiğin altına etme Raporu iletilerini çıkışı filtreler. Ayrıca ekler **MessageType** özellik kümesine değerle iletisi **uyarı**. 

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

2. VS Code gezgininde **module.json** dosyasına sağ tıklayın ve **IoT Edge modülü Docker görüntüsü derle ve gönder** seçeneğine tıklayın. VS Code penceresinin açılır kutusunda, Linux kapsayıcı için **amd64** ve Windows kapsayıcı için **windows-amd64** olacak şekilde kapsayıcı platformunuzu seçin. VS Code'da sonra kodunuzu oluşturur, containerize `FilterModule.dll` ve belirttiğiniz kapsayıcı kayıt defterine gönderme.


3. VS Code tümleşik terminalinde etiketle tam kapsayıcı görüntü adresini alabilirsiniz. Derleme ve gönderme tanımı hakkında daha fazla bilgi için `module.json` dosyasına bakabilirsiniz.

## <a name="add-registry-credentials-to-edge-runtime"></a>Kayıt defteri kimlik bilgilerini kenar çalışma zamanına ekleyin
Kayıt defterinizin kimlik bilgilerini, Edge cihazınızı çalıştırdığınız bilgisayarın Edge çalışma zamanına ekleyin. Bu kimlik bilgileri kapsayıcı çıkarmak için çalışma zamanı erişim verin. 

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
4. Denetleyin **tempSensor** modülü otomatik olarak doldurulur. Değilse, bunu eklemek için aşağıdaki adımları kullanın:
    1. **IoT Edge Modülü Ekle**'yi seçin.
    2. **Ad** alanına `tempSensor` girin.
    3. **Görüntü URI'si** alanına `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview` girin.
    4. Diğer ayarları değiştirmeden bırakın ve **Kaydet**'e tıklayın.
5. Ekleme **filterModule** önceki kısımlarında oluşturduğunuz modül. 
    1. **IoT Edge Modülü Ekle**'yi seçin.
    2. **Ad** alanına `filterModule` girin.
    3. **Görüntü URI'si** alanına görüntünüzün adresini girin; örneğin `<your container registry address>/filtermodule:0.0.1-amd64`. Tam görüntü adresi, önceki bölümde bulunabilir.
    4. Denetleme **etkinleştirmek** modülü twin düzenleyebilmek kutusu. 
    5. Metin kutusunda JSON modülü çiftinin aşağıdaki JSON ile değiştirin: 

        ```json
        {
           "properties.desired":{
              "TemperatureThreshold":25
           }
        }
        ```
 
    6. **Kaydet**’e tıklayın.
6. **İleri**’ye tıklayın.
7. **Rota Belirtme** adımında, aşağıdaki JSON’u metin kutusuna kopyalayın. Modülleri tüm iletileri kenar çalışma zamanına yayımlayın. Çalışma zamanında bildirim temelli kuralları burada iletileri akış tanımlayın. Bu öğreticide, iki yol gerekir. İlk yol sıcaklık algılayıcısı iletilerden filtresi modülü ile yapılandırılmış uç noktası "input1" uç nokta aracılığıyla taşımaları **FilterMessages** işleyicisi. İkinci rota, iletileri filtre modülünden IoT Hub'a taşır. Bu rotada `upstream`, Edge Hub'a iletileri IoT Hub'a göndermesini bildiren özel bir hedeftir. 

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
10. IoT Edge cihaz ayrıntıları sayfasına dönün ve **Yenile**’ye tıklayın. Yeni görmelisiniz **filtermodule** ile birlikte çalışan **tempSensor** modülü ve **IOT kenar çalışma zamanı**. 

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

IoT Edge cihazınızdan IoT hub'ınıza cihazdan buluta gönderilen iletileri izlemek için:
1. Azure IoT Toolkit uzantısını IoT hub'ınızın bağlantı dizesiyle yapılandırın: 
    1. VS Code'da explorer'ı seçerek açın **Görünüm** > **Explorer**. 
    2. Explorer'ın tıklatın **IOT HUB CİHAZLARI** ve ardından **...** . Tıklatın **IOT Hub bağlantı dizesine ayarlamak** ve açılır pencerede IOT kenar Cihazınızı bağlandığı IOT hub'ına yönelik bağlantı dizesini girin. 

        Bağlantı dizesi bulmak için Azure portalında IOT hub'ınız için kutucuğa tıklayın ve ardından **paylaşılan erişim ilkeleri**. İçinde **paylaşılan erişim ilkeleri**, tıklatın **iothubowner** İlkesi ve kopyalama IOT Hub bağlantı dizesi içinde **iothubowner** penceresi.   

2. IOT hub'ına ulaşan verilerin izlemek için seçin **Görünüm** > **komutu palet** arayın ve **IOT: izleme D2C iletisi Başlat** menü komutu. 
3. Veri izlemeyi durdurmak için kullanmak **IOT: D2C ileti izlemeyi durdurmak** menü komutu. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, IOT kenar cihazınız tarafından oluşturulan ham verileri filtrelemek için kodu içeren bir IOT kenar modülünü oluşturuldu. İşletme öngörüleri kenarına içine veri kapatmak Azure IOT kenar yardımcı olabilecek diğer yollarını öğrenmek için aşağıdaki öğreticileri herhangi birini açın devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Bir modül olarak Azure işlevi dağıtmak](tutorial-deploy-function.md)
> [Azure akış analizi bir modül olarak dağıtma](tutorial-deploy-stream-analytics.md)


<!-- Links -->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md

<!-- Images -->
[1]: ./media/tutorial-csharp-module/programcs.png
[2]: ./media/tutorial-csharp-module/build-module.png
[3]: ./media/tutorial-csharp-module/docker-os.png
