---
title: "Azure IOT kenar C# modül | Microsoft Docs"
description: "C# kodu ile bir IOT kenar modül oluşturun ve bir uç cihazın dağıtma"
services: iot-edge
keywords: 
author: JimacoMS2
manager: timlt
ms.author: v-jamebr
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 0f0591a9f2b8c932cb9a03e7186dbf6877b96197
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2017
---
# <a name="develop-and-deploy-a-c-iot-edge-module-to-your-simulated-device---preview"></a>Geliştir ve C# IOT kenar modülünü sanal Cihazınızı dağıtmak - Önizleme

IOT kenar modülleri, iş mantığınızı IOT sınır cihazları için doğrudan uygulayan kod dağıtmak için kullanabilirsiniz. Bu öğreticide, oluşturma ve dağıtma algılayıcı verileri filtreler bir IOT kenar modülü aracılığıyla açıklanmaktadır. Bir sanal cihaz dağıtmak Azure IOT kenarına oluşturduğunuz sanal IOT sınır cihazı kullanacağınız [Windows] [ lnk-tutorial1-win] veya [Linux] [ lnk-tutorial1-lin]öğreticileri. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:    

> [!div class="checklist"]
> * .NET core 2.0 dayalı bir IOT kenar modülü oluşturmak için Visual Studio Code kullanın
> * Docker görüntü oluşturma ve kayıt defterine yayımlama için Visual Studio Code ve Docker kullanın 
> * Modül IOT kenar Cihazınızı dağıtma
> * Oluşturulan görünüm verileri


Bu öğreticide oluşturduğunuz IOT kenar modülü cihazınız tarafından oluşturulan sıcaklık verileri filtreler. Sıcaklık belirtilen eşiğin üzerindeyse, yalnızca iletileri upstream gönderir. Bu türde bir kenara çözümlemesini SDK'ya ve bulutta depolanan veri miktarını azaltmak için yararlıdır. 

## <a name="prerequisites"></a>Ön koşullar

* Hızlı Başlangıç ya da ilk öğreticide oluşturduğunuz Azure IOT sınır cihazı.
* IOT sınır cihazı için birincil anahtar bağlantı dizesi.  
* [Visual Studio Code](https://code.visualstudio.com/). 
* [Visual Studio Code için Azure IOT kenar uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
* [C# Visual Studio Code (OmniSharp tarafından desteklenen) uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). 
* [Docker](https://docs.docker.com/engine/installation/) aynı bilgisayarda Visual Studio Code sahiptir. Community Edition (CE), Bu öğretici için yeterlidir. 
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

## <a name="create-an-iot-edge-module-project"></a>Bir IOT kenar modülü projesi oluşturma
Bir IOT kenar modülü oluşturmak için .NET tabanlı nasıl 2.0 kullanarak Visual Studio Code ve Azure IOT kenar uzantısı çekirdek aşağıdaki adımları gösterir.
1. Visual Studio kod seçin **Görünüm** > **tümleşik Terminal** VS Code tümleşik Terminali açın.
3. Tümleşik terminale yüklemek (veya güncelleştirmek için) aşağıdaki komutu girin **AzureIoTEdgeModule** dotnet şablonunda:

    ```cmd/sh
    dotnet new -i Microsoft.Azure.IoT.Edge.Module
    ```

2. Yeni modül için bir proje oluşturun. Aşağıdaki komut proje klasörünü oluşturur **FilterModule**, geçerli çalışma klasörü içinde:

    ```cmd/sh
    dotnet new aziotedgemodule -n FilterModule
    ```
 
3. Seçin **dosya** > **klasörü açın**.
4. Gözat **FilterModule** klasörü ve tıklatın **klasörü seçin** proje VS Code'da açın.
5. VS Code Explorer'da tıklatın **Program.cs** açın.

   ![Program.cs açın][1]

6. Ekleme `temperatureThreshold` değişkenini **Program** sınıfı. Bu değişken, IOT Hub'ına gönderilecek verileri sırayla ölçülen sıcaklık aşmalıdır değeri ayarlar. 

    ```csharp
    static int temperatureThreshold { get; set; } = 25;
    ```

7. Ekleme `MessageBody`, `Machine`, ve `Ambient` için sınıfları **Program** sınıfı. Bu sınıfların gelen ileti gövdesini beklenen şemasını tanımlayın.

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

8. İçinde **Init** yöntemi, kod oluşturur ve yapılandırır bir **DeviceClient** nesnesi. Bu nesne ileti gönderme ve alma için yerel Azure IOT kenar çalışma bağlanmak modülü sağlar. Kullanılan bağlantı dizesi **Init** yöntemi modülü IOT kenar çalışma zamanı tarafından sağlanmaktadır. Oluşturduktan sonra **DeviceClient**, kod IOT kenar hub'dan iletileri almak için bir geri çağırma kaydeder **input1** uç noktası. Değiştir `SetInputMessageHandlerAsync` yeni bir yöntemle ve ekleme bir `SetDesiredPropertyUpdateCallbackAsync` istenen özellikleri güncelleştirmeleri yöntemi. Bu değişikliği yapmak için son satırının yerini **Init** aşağıdaki kod ile yöntemi:

    ```csharp
    // Register callback to be called when a message is received by the module
    // await ioTHubModuleClient.SetImputMessageHandlerAsync("input1", PipeMessage, iotHubModuleClient);

    // Attach callback for Twin desired properties updates
    await ioTHubModuleClient.SetDesiredPropertyUpdateCallbackAsync(onDesiredPropertiesUpdate, null);

    // Register callback to be called when a message is received by the module
    await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FilterMessages, ioTHubModuleClient);
    ```

9. Ekleme `onDesiredPropertiesUpdate` yönteme **Program** sınıfı. Bu yöntem modülü twin İstenen özelliklerde güncelleştirmeleri alır ve güncelleştirmeleri **temperatureThreshold** eşleşecek şekilde değişkeni. Tüm modülleri doğrudan buluttan bir modül içinde çalışan kodu yapılandırmanıza olanak sağlayan kendi modülü çiftine sahip.

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

10. Değiştir `PipeMessage` yöntemiyle `FilterMessages` yöntemi. Modül bir ileti kenar IOT hub'ından aldığı zaman bu yöntem çağrılır. Modül twin ayarlamak sıcaklık eşiğin altına etme Raporu iletilerini çıkışı filtreler. Ayrıca ekler **MessageType** özellik kümesine değerle iletisi **uyarı**. 

    ```csharp
    static async Task<MessageResponse> FilterMessages(Message message, object userContext)
    {
        int counterValue = Interlocked.Increment(ref counter);

        try {
            DeviceClient deviceClient = (DeviceClient)userContext;

            byte[] messageBytes = message.GetBytes();
            string messageString = Encoding.UTF8.GetString(messageBytes);
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
            DeviceClient deviceClient = (DeviceClient)userContext;
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

11. Projeyi derlemek için sağ **FilterModule.csproj** dosya Gezgini ve tıklatın **yapı IOT kenar Modülü**. Bu işlem modülü derler ve ikili ve bağımlılıklarını Docker görüntüsünü oluşturmak için kullanılan bir klasöre dışa aktarır.

   ![IOT kenar modülü oluşturmak][2]

## <a name="create-a-docker-image-and-publish-it-to-your-registry"></a>Docker görüntü oluşturma ve kayıt defterine yayımlama

1. VS Code Explorer'da genişletin **Docker** klasör. Kapsayıcı platformunuz için klasör ya da genişletin **linux x64** veya **windows nano**.

   ![Docker kapsayıcısı platformu seçin][3]

2. Sağ **Dockerfile** dosya ve tıklayın **yapı IOT kenar modülü Docker görüntü**. 
3. İçinde **Klasör Seç** penceresinde göz atın veya girin `./bin/Debug/netcoreapp2.0/publish`. Tıklatın **EXE_DIR Klasör Seç**.
4. VS Code pencerenin üstündeki açılır metin kutusuna görüntü adı girin. Örneğin: `<your container registry address>/filtermodule:latest`. Kapsayıcı kayıt defteri adresi kayıt defterinden kopyaladığınız oturum açma sunucusu ile aynıdır. Biçiminde olmalıdır `<your container registry name>.azurecr.io`.
5. VS Code tümleşik terminale aşağıdaki komutu girerek Docker için oturum açın: 
     
   ```csh/sh
   docker login -u <username> -p <password> <Login server>
   ```
        
   Kullanıcı adı, parola ve oluşturduğunuz sırada, Azure kapsayıcı kayıt defterinden kopyaladığınız oturum açma sunucusu kullanın.

3. Görüntü Docker deponuza iletin. Seçin **Görünüm** > **komutu palet** arayın ve **kenar: anında IOT kenar modülü Docker görüntü** menü komutu. VS Code pencerenin üstündeki açılır metin kutusuna görüntü adı girin. Adımda kullanılan aynı görüntü adı kullanmak 1.d.

## <a name="add-registry-credentials-to-edge-runtime"></a>Kayıt defteri kimlik bilgilerini kenar çalışma zamanına ekleyin
Sınır cihazı çalıştırdığınız bilgisayarda kenar çalışma zamanı kayıt için kimlik bilgilerini ekleyin. Bu kimlik bilgileri kapsayıcı çıkarmak için çalışma zamanı erişim verin. 

- Windows için aşağıdaki komutu çalıştırın:
    
    ```cmd/sh
    iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

- Linux için aşağıdaki komutu çalıştırın:
    
    ```cmd/sh
    sudo iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

## <a name="run-the-solution"></a>Çözümü çalıştırın

1. İçinde [Azure portal](https://portal.azure.com), IOT hub'ına gidin.
2. Git **IOT kenar (Önizleme)** ve IOT sınır cihazı seçin.
3. Seçin **ayarlamak modülleri**. 
2. Denetleyin **tempSensor** modülü otomatik olarak doldurulur. Değilse, bunu eklemek için aşağıdaki adımları kullanın:
    1. Seçin **IOT kenar Modül Ekle**.
    2. İçinde **adı** alanına, `tempSensor`.
    3. İçinde **görüntü URI** alanına, `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`.
    4. Diğer ayarları değiştirmeden bırakın ve tıklayın **kaydetmek**.
9. Ekleme **filterModule** önceki kısımlarında oluşturduğunuz modül. 
    1. Seçin **IOT kenar Modül Ekle**.
    2. İçinde **adı** alanına, `filterModule`.
    3. İçinde **görüntü URI** alan, görüntü adresinizi girin; örneğin `<your container registry address>/filtermodule:latest`.
    4. Denetleme **etkinleştirmek** modülü twin düzenleyebilmek kutusu. 
    5. Metin kutusunda JSON modülü çiftinin aşağıdaki JSON ile değiştirin: 

        ```json
        {
           "properties.desired":{
              "TemperatureThreshold":25
           }
        }
        ```
 
    6. **Kaydet** düğmesine tıklayın.
12. **İleri**’ye tıklayın.
13. İçinde **belirtin yollar** adım, JSON altındaki metin kutusuna Kopyala. Modülleri tüm iletileri kenar çalışma zamanına yayımlayın. Çalışma zamanında bildirim temelli kuralları burada iletileri akış tanımlayın. Bu öğreticide, iki yol gerekir. İlk yol sıcaklık algılayıcısı iletilerden filtresi modülü ile yapılandırılmış uç noktası "input1" uç nokta aracılığıyla taşımaları **FilterMessages** işleyicisi. İkinci yol filtresi modülü gelen iletileri IOT Hub'ına taşımaları. Bu rotadaki `upstream` kenar Hub'ın IOT Hub'ına iletileri göndermek için söyler özel bir hedef. 

    ```json
    {
       "routes":{
          "sensorToFilter":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filtermodule/inputs/input1\")",
          "filterToIoTHub":"FROM /messages/modules/filtermodule/outputs/output1 INTO $upstream"
       }
    }
    ```

4. **İleri**’ye tıklayın.
5. İçinde **gözden geçirme şablonu** adımını, **gönderme**. 
6. IOT Edge cihaz ayrıntıları sayfasına dönün ve **yenileme**. Yeni görmelisiniz **filtermodule** ile birlikte çalışan **tempSensor** modülü ve **IOT kenar çalışma zamanı**. 

## <a name="view-generated-data"></a>Oluşturulan görünüm verileri

Cihaz bulut iletilerini IOT kenar cihazınızın IOT hub'ına gönderilen izlemek için:
1. IOT hub'ınız için bağlantı dizesi ile Azure IOT Araç Seti uzantısı yapılandırın: 
    1. VS Code'da explorer'ı seçerek açın **Görünüm** > **Explorer**. 
    3. Explorer'ın tıklatın **IOT HUB CİHAZLARI** ve ardından **...** . Tıklatın **IOT Hub bağlantı dizesine ayarlamak** ve açılır pencerede IOT kenar Cihazınızı bağlandığı IOT hub'ına yönelik bağlantı dizesini girin. 

        Bağlantı dizesi bulmak için Azure portalında IOT hub'ınız için kutucuğa tıklayın ve ardından **paylaşılan erişim ilkeleri**. İçinde **paylaşılan erişim ilkeleri**, tıklatın **iothubowner** İlkesi ve kopyalama IOT Hub bağlantı dizesi içinde **iothubowner** penceresi.   

1. IOT hub'ına ulaşan verilerin izlemek için seçin **Görünüm** > **komutu palet** arayın ve **IOT: izleme D2C iletisi Başlat** menü komutu. 
2. Veri izlemeyi durdurmak için kullanmak **IOT: D2C ileti izlemeyi durdurmak** menü komutu. 

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