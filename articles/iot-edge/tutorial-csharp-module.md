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
ms.openlocfilehash: fb7674d8c292e7d571a94ac4625b0858a90704b3
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="develop-and-deploy-a-c-iot-edge-module-to-your-simulated-device---preview"></a>Geliştir ve C# IOT kenar modülünü sanal Cihazınızı dağıtmak - Önizleme

IOT kenar modülleri, iş mantığınızı IOT sınır cihazları için doğrudan uygulayan kod dağıtmak için kullanabilirsiniz. Bu öğreticide, oluşturma ve dağıtma algılayıcı verileri üzerinde bir sanal cihaz dağıtmak Azure IOT kenarına oluşturduğunuz sanal IOT sınır aygıtında filtreler bir IOT kenar modülü aracılığıyla açıklanmaktadır [Windows] [ lnk-tutorial1-win]veya [Linux] [ lnk-tutorial1-lin] öğreticileri. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:    

> [!div class="checklist"]
> * .NET core 2.0 dayalı bir IOT kenar modülü oluşturmak için Visual Studio Code kullanın
> * Docker görüntü oluşturma ve kayıt defterine yayımlama için Visual Studio Code ve Docker kullanın 
> * Modül IOT kenar Cihazınızı dağıtma
> * Oluşturulan görünüm verileri


Bu öğreticide oluşturduğunuz IOT kenar modülü cihazınız tarafından oluşturulan sıcaklık veri filtreler ve sıcaklık belirtilen eşiğin üzerinde olduğunda yalnızca iletileri upstream gönderir. 

## <a name="prerequisites"></a>Ön koşullar

* Hızlı Başlangıç ya da önceki öğreticide oluşturduğunuz Azure IOT sınır cihazı.
* IOT kenar cihazın bağlandığı IOT hub'ın IOT Hub bağlantı dizesi.  
* [Visual Studio Code](https://code.visualstudio.com/). 
* [Visual Studio Code için Azure IOT kenar uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). (Uzantısı Visual Studio Code uzantıları panelinden yükleyebilirsiniz.)
* [C# Visual Studio Code (OmniSharp tarafından desteklenen) uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). (Uzantısı Visual Studio Code uzantıları panelinden yükleyebilirsiniz.)
* Visual Studio Code için Azure IOT kenar uzantısı
* [Docker](https://docs.docker.com/engine/installation/). Bu öğretici için yeterli platformunuzun Community Edition (CE) olur. VS Code çalıştıran bilgisayarda yüklediğinizden emin olun.
* [.NET 2.0 SDK çekirdek](https://www.microsoft.com/net/core#windowscmd). 

## <a name="choose-or-sign-up-for-a-docker-registry"></a>Seçin veya bir Docker kayıt defteri için kaydolun
Bu öğreticide, bir modülü oluşturmak ve oluşturmak için Azure IOT kenar uzantısı VS Code için kullanmak bir [Docker görüntü](https://docs.docker.com/glossary/?term=image). Bu Docker görüntüsüne itme sonra bir [Docker deposu](https://docs.docker.com/glossary/?term=repository) üzerinde barındırılan bir [Docker kayıt defteri](https://docs.docker.com/glossary/?term=registry). Son olarak, Docker görüntünüzü olarak paketlenmiş dağıttığınız bir [Docker kapsayıcısı](https://docs.docker.com/glossary/?term=container) IOT kenar aygıtınıza, kayıt defterinden.  

Bu öğretici için Docker uyumlu kayıt kullanabilirsiniz. İki popüler Docker kayıt defteri hizmetlerinin kullanılabilir bulutta **Azure kapsayıcı kayıt defteri** ve **Docker hub'a**:

- [Azure kapsayıcı kayıt defteri](https://docs.microsoft.com/en-us/azure/container-registry/) kullanılabilir bir [Ücretli aboneliği](https://azure.microsoft.com/en-us/pricing/details/container-registry/). Bu öğretici için **temel** abonelik yeterli. 

- [Docker hub'a](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags) , Docker (ücretsiz) bir kimliği için kaydolursanız bir ücretsiz özel depo sunar 
    1. Docker kimliği için kaydolmak için'ndaki yönergeleri izleyin [kaydetmek için bir Docker kimliği](https://docs.docker.com/docker-id/#register-for-a-docker-id) Docker sitesinde. 

    2. Özel bir Docker deposu oluşturmak için yönergeleri izleyin [Docker hub'ına yeni bir havuz oluşturma](https://docs.docker.com/docker-hub/repos/#creating-a-new-repository-on-docker-hub) Docker sitesinde.

Bu öğreticide, uygun olan yerlerde, komutları Azure kapsayıcı kayıt defteri ve Docker hub'a için sağlanır.

## <a name="create-an-iot-edge-module-project"></a>Bir IOT kenar modülü projesi oluşturma
Bir IOT kenar modülü oluşturmak için .NET tabanlı nasıl 2.0 kullanarak Visual Studio Code ve Azure IOT kenar uzantısı çekirdek aşağıdaki adımları gösterir.
1. Açık VS kodu.
2. Kullanım **View | Terminal tümleşik** menü komutu VS Code tümleşik Terminali açın.
3. Tümleşik terminale yüklemek (veya güncelleştirmek için) aşağıdaki komutu girin **AzureIoTEdgeModule** dotnet şablonunda:

    ```cmd/sh
    dotnet new -i Microsoft.Azure.IoT.Edge.Module
    ```

2. Tümleşik terminale yeni modül için bir proje oluşturmak için aşağıdaki komutu girin:

    ```cmd/sh
    dotnet new aziotedgemodule -n FilterModule
    ```

    >[!NOTE]
    > Bu komut, proje klasörünü oluşturur **FilterModule**, geçerli çalışma klasörü içinde. Başka bir konumda oluşturmak istiyorsanız, dizinleri komutu çalıştırmadan önce değiştirin.
 
3. Kullanım **dosya | Klasörü açın** menü komutu, Gözat ' **FilterModule** klasörü ve tıklatın **klasörü seçin** proje VS Code'da açın.
4. VS Code Explorer'da tıklatın **Program.cs** açın.
5. Aşağıdaki alana ekleme **Program** sınıfı.

    ```csharp
    static int temperatureThreshold { get; set; } = 25;
    ```

1. Aşağıdaki sınıfları ekleme **Program** sınıfı. Bu sınıfların gelen ileti gövdesini beklenen şemasını tanımlayın.

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

1. İçinde **Init** yöntemi, kod oluşturur ve yapılandırır bir **DeviceClient** nesnesi. Bu nesne ileti gönderme ve alma için yerel Azure IOT kenar çalışma bağlanmak modülü sağlar. Kullanılan bağlantı dizesi parametresi **Init** yöntemi, IOT kenar çalışma zamanında ortam değişkeni tarafından modülü sağlanmaktadır **EdgeHubConnectionString**. Oluşturduktan sonra **DeviceClient**, kod IOT kenar hub'dan iletileri almak için bir geri çağırma kaydeder **input1** uç noktası. Geri arama olarak yeni bir tane iletileri işlemek için kullanılan yöntem değiştirin ve bir geri çağırma modülü twin istenen özellikleri güncelleştirmeleri ekleyin. Bu değişikliği yapmak için son satırının yerini **Init** aşağıdaki kod ile yöntemi:

    ```csharp
    // Register callback to be called when a message is received by the module
    // await ioTHubModuleClient.SetImputMessageHandlerAsync("input1", PipeMessage, iotHubModuleClient);

    // Attach callback for Twin desired properties updates
    await ioTHubModuleClient.SetDesiredPropertyUpdateCallbackAsync(onDesiredPropertiesUpdate, null);

    // Register callback to be called when a message is received by the module
    await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FilterMessages, ioTHubModuleClient);
    ```

1. Aşağıdaki yöntemi ekleyin **Program** güncelleştirmek için sınıf **temperatureThreshold** modülü twin aracılığıyla arka uç hizmeti tarafından gönderilen istenen özellikleri temel alan. Tüm modüllerin kendi modülü çiftine sahip. Bir modül twin bir arka uç hizmeti bir modül içinde çalışan kodu yapılandırma sağlar.

    ```csharp
    static Task onDesiredPropertiesUpdate(TwinCollection desiredProperties, object userContext)
    {
        try
        {
            Console.WriteLine("Desired property change:");
            Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));

            if (desiredProperties["TemperatureThreshold"].exists())
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

1. Değiştir **PipeMessage** aşağıdaki yöntemiyle yöntemi. Modül bir ileti kenar Hub'ından gönderilen her bu yöntem çağrılır. İleti gövdesinde sıcaklık değere göre iletileri ve modül twin ayarlamak sıcaklık eşiği filtreler.

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

11. Projeyi derleyin. Kullanım **View | Explorer** VS Code Gezgini'ni açmak için menü komutu. Explorer'ın sağ **FilterModule.csproj** dosyası ve'ı tıklatın **yapı IOT kenar Modülü** modül derlemek ve ikili ve Docker görüntü oluşturduğunuz bir klasöre bağımlılıklarını dışa aktarmak için gelen bir sonraki adımda.

## <a name="create-a-docker-image-and-publish-it-to-your-registry"></a>Docker görüntü oluşturma ve kayıt defterine yayımlama

1. Docker görüntüsünü oluşturabilirsiniz.
    1. VS Code Explorer'da tıklatın **Docker** klasörünü açmak için. Kapsayıcı platformunuz için klasör ya da seçin **linux x64** veya **windows nano**. 
    2. Sağ **Dockerfile** dosya ve tıklayın **yapı IOT kenar modülü Docker görüntü**. 
    3. İçinde **Klasör Seç** kutusunda, her iki göz atın veya girin `./bin/Debug/netcoreapp2.0/publish`. Tıklatın **EXE_DIR Klasör Seç**.
    4. VS Code pencerenin üstündeki açılır metin kutusuna görüntü adı girin. Örneğin:`<docker registry address>/filtermodule:latest`; burada *docker kayıt defteri adresi* Docker hub'a kullanıyorsanız, Docker kimliğidir veya benzer `<your registry name>.azurecr.io`, Azure kapsayıcı kayıt defteri kullanıyorsanız.
 
4. Docker için oturum açın. Tümleşik terminal içinde aşağıdaki komutu girin: 

    - Docker hub'a (istendiğinde, kimlik bilgilerinizi girin):
        
        ```csh/sh
        docker login
        ```

    - Azure kapsayıcı kayıt defteri için:
        
        ```csh/sh
        docker login -u <username> -p <password> <Login server>
        ```
        
        Kullanıcı adı, bu komutu kullanmak için parola ve oturum açma sunucusu bulmak için [Azure portalına] gidin (https://portal.azure.com). Gelen **tüm kaynakları**, Azure kapsayıcı kaydınız özelliklerini açın ve ardından kutucuğa tıklayın **erişim anahtarları**. Değerleri kopyalamak **kullanıcıadı**, **parola**, ve **oturum açma sunucusu** alanları. Oturum açma sunucusu sould biçiminde olmalıdır: `<your registry name>.azurecr.io`.

3. Görüntü Docker deponuza iletin. Kullanım **View | Palet komutu... | Edge: Anında iletme IOT kenar modülü Docker görüntü** menü komutu ve VS Code pencerenin üstündeki açılır metin kutusuna görüntü adı girin. Adımda kullanılan aynı görüntü adı kullanmak 1.c.

## <a name="add-registry-credentials-to-edge-runtime-on-your-edge-device"></a>Edge aygıtınızda kenar çalışma zamanı için kayıt defteri kimlik bilgilerini ekleyin
Sınır cihazı çalıştırdığınız bilgisayarda kenar çalışma zamanı kayıt için kimlik bilgilerini ekleyin. Bu kapsayıcı çıkarmak için çalışma zamanı erişimi verir. 

- Windows için aşağıdaki komutu çalıştırın:
    
    ```cmd/sh
    iotedgectl login --address <docker-registry-address> --username <docker-username> --password <docker-password> 
    ```

- Linux için aşağıdaki komutu çalıştırın:
    
    ```cmd/sh
    sudo iotedgectl login --address <docker-registry-address> --username <docker-username> --password <docker-password> 
    ```

## <a name="run-the-solution"></a>Çözümü çalıştırın

1. İçinde [Azure portal](https://portal.azure.com), IOT hub'ına gidin.
2. Git **IOT kenar (Önizleme)** ve IOT sınır cihazı seçin.
3. Seçin **ayarlamak modülleri**. 
2. Ekleme **tempSensor** modülü. Bu adım yalnızca bağlıdır, değil daha önce dağıttıysanız gerekli **tempSensor** IOT kenar aygıtınıza modülü.
    1. Seçin **IOT kenar Modül Ekle**.
    2. İçinde **adı** alanına, `tempSensor`.
    3. İçinde **görüntü URI** alanına, `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`.
    4. Diğer ayarları değiştirmeden bırakın ve tıklayın **kaydetmek**.
9. Ekleme **filtermodule**
    1. Seçin **IOT kenar Modül Ekle** yeniden.
    2. İçinde **adı** alanına, `filtermodule`.
    3. İçinde **görüntü** alan, görüntü adresinizi girin; örneğin `<docker registry address>/filtermodule:latest`.
    4. Denetleme **düzenleme modülü twin** kutusu.
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
13. İçinde **belirtin yollar** adım, JSON altındaki metin kutusuna Kopyala. Modülleri tüm iletileri kenar çalışma zamanına yayımlayın. Burada bu iletileri akış çalışma zamanında bildirim temelli kuralları tanımlar. Bu öğreticide, iki yol gerekir. İlk yol sıcaklık algılayıcısı iletilerden filtresi modülü ile yapılandırılmış uç noktası "input1" uç nokta aracılığıyla taşımaları **FilterMessages** işleyicisi. İkinci yol filtresi modülü gelen iletileri IOT Hub'ına taşımaları. Bu rotadaki `upstream` kenar Hub'ın IOT Hub'ına iletileri göndermek için söyler özel bir hedef. 

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
    1. Kullanım **View | Explorer** VS Code Gezgini'ni açmak için menü komutu. 
    3. Explorer'ın tıklatın **IOT HUB CİHAZLARI** ve ardından **...** . Tıklatın **IOT Hub bağlantı dizesine ayarlamak** ve açılır pencerede IOT kenar Cihazınızı bağlandığı IOT hub'ına yönelik bağlantı dizesini girin. 

        Bağlantı dizesi bulmak için Azure portalında IOT hub'ınız için kutucuğa tıklayın ve ardından **paylaşılan erişim ilkeleri**. İçinde **paylaşılan erişim ilkeleri**, tıklatın **iothubowner** İlkesi ve kopyalama IOT Hub bağlantı dizesi içinde **iothubowner** penceresi.   

1. IOT hub'ına ulaşan verilerin izlemek için kullanın **View | Palet komutu... | IOT: İzleme D2C iletisi başlangıç** menü komutu. 
2. Veri izlemeyi durdurmak için kullanmak **View | Palet komutu... | IOT: D2C ileti izlemeyi durdurmak** menü komutu. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, IOT kenar cihazınız tarafından oluşturulan ham verileri filtrelemek için kodu içeren bir IOT kenar modülünü oluşturuldu. İşletme öngörüleri kenarına içine veri kapatmak Azure IOT kenar yardımcı olabilecek diğer yollarını öğrenmek için aşağıdaki öğreticileri herhangi birini açın devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Bir modül olarak Azure işlevi dağıtmak](tutorial-deploy-function.md)
> [Azure akış analizi bir modül olarak dağıtma](tutorial-deploy-stream-analytics.md)


<!-- Links -->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
