---
title: "Bir C# modül Azure IOT Edge geliştirmek için Visual Studio Code kullanma | Microsoft Docs"
description: "Geliştirme ve bağlam geçmeden VS code'da Azure IOT kenarıyla bir C# modül dağıtma"
services: iot-edge
keywords: 
author: shizn
manager: timlt
ms.author: xshi
ms.date: 12/06/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 269f77e5015175e45e0078926ef06699811889a4
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="use-visual-studio-code-to-develop-c-module-with-azure-iot-edge"></a>C# modül Azure IOT Edge geliştirmek için Visual Studio Code kullanma
Bu makalede kullanmaya yönelik ayrıntılı yönergeler sağlanmaktadır [Visual Studio Code](https://code.visualstudio.com/) geliştirmek ve IOT kenar modüllerinizi dağıtmak için ana geliştirme aracı olarak. 

## <a name="prerequisites"></a>Önkoşullar
Bu öğretici, bir bilgisayar veya geliştirme makine olarak Windows veya Linux çalıştıran sanal makine kullandığınızı varsayar. IOT sınır cihazı başka bir fiziksel aygıt olabilir veya geliştirme makinenizde IOT kenar Cihazınızı benzetimini yapabilirsiniz.

Bu kılavuza başlamadan önce aşağıdaki öğreticiler tamamlanmış olduğundan emin olun.
- Sanal bir cihaz üzerinde Azure IOT kenar dağıtmak [Windows](https://docs.microsoft.com/azure/iot-edge/tutorial-simulate-device-windows) veya [Linux](https://docs.microsoft.com/azure/iot-edge/tutorial-simulate-device-linux)
- [Geliştirme ve sanal cihazınız bir C# IOT kenar modülü dağıtma](https://docs.microsoft.com/azure/iot-edge/tutorial-csharp-module)

Önceki öğreticileri tamamladıktan sonra olmalıdır öğeleri gösteren bir denetim listesi aşağıda verilmiştir.

- [Visual Studio Code](https://code.visualstudio.com/). 
- [Visual Studio Code için Azure IOT kenar uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
- [C# Visual Studio Code (OmniSharp tarafından desteklenen) uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). 
- [Docker](https://docs.docker.com/engine/installation/)
- [.NET 2.0 SDK çekirdek](https://www.microsoft.com/net/core#windowscmd). 
- [Python 2.7](https://www.python.org/downloads/)
- [IOT kenar denetim komut dosyası](https://pypi.python.org/pypi/azure-iot-edge-runtime-ctl)
- AzureIoTEdgeModule şablonu (`dotnet new -i Microsoft.Azure.IoT.Edge.Module`)
- Etkin bir IOT hub ile en az bir IOT sınır cihazı.

Yüklemek için de önerilen [VS Code Docker desteği](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) modülü görüntüler ve kapsayıcıları daha iyi yönetebilmek için.

## <a name="deploy-an-azure-iot-edge-module-in-vs-code"></a>VS Code'da Azure IOT kenar modülünde dağıtma

### <a name="list-your-iot-hub-devices"></a>IOT hub cihazları listeleyin
IOT hub aygıtlarınızı VS kodunuzda listelemek için iki yolu vardır. Devam etmek için her iki durumda seçebilirsiniz.

#### <a name="sign-in-your-azure-account-in-vscode-and-choose-your-iot-hub"></a>VSCode Azure hesabınızda oturum açın ve IOT hub'ı seçin
1. Komut Palette (F1 veya Ctrl + Shift + P) yazın ve seçin **Azure: oturum**. Ardından  **kopya* & açık** pencerede. (Ctrl + V) kodu tarayıcınıza yapıştırın ve Devam Et düğmesine tıklayın. Ardından Azure hesabınızla oturum açın. Hesap bilgilerinizi VS Code durum çubuğunda görebilirsiniz.
2. Komut Palette (F1 veya Ctrl + Shift + P) yazın ve seçin **IOT: IOT Hub'ı seçin**. Önce önceki öğreticide IOT hub'ınızı oluşturulduğu abonelik de seçin. Ardından IOT sınır cihazı içeren IOT hub'ı seçin.


#### <a name="set-iot-hub-connection-string"></a>IOT hub bağlantı dizesine ayarlayın
1. Komut Palette (F1 veya Ctrl + Shift + P) yazın ve seçin **IOT: IOT Hub bağlantı dizesine ayarlamak**. Bağlantı dizesi ilkesi altında yapıştırdığınız emin olun **iothubowner** (bunu IOT hub'ınızın paylaşılan erişim ilkeleri Azure portalında bulabilirsiniz).
 

IOT Hub cihazları sol kenar çubuğu Explorer'da aygıt listesinde görebilirsiniz.

### <a name="start-your-iot-edge-runtime-and-deploy-a-module"></a>IOT kenar çalışma zamanı başlatma ve bir modül dağıtma
Yükleyin ve Azure IOT kenar çalışma zamanı aygıtınızda başlatın. Ve IOT Hub'ına telemetri verileri gönderecek benzetimli algılayıcı modül dağıtın.
1. Komut Palette seçin **kenar: Kurulum kenar** ve cihaz kimliği, IOT kenar'ı seçin Veya Edge cihaz kimliği aygıt listesinde sağ tıklatın ve seçin **Kurulum kenar**.
2. Komut Palette seçin **kenar: Başlangıç kenar** kenar çalışma zamanı başlatmak için. Tümleşik terminal karşılık gelen çıktılarında görebilirsiniz.
3. Docker Explorer'da kenar çalışma zamanı durumunu kontrol edin. Yeşil gelir çalışıyor. IOT kenar çalışma zamanı başarıyla başlatıldı.
4. Edge çalışma zamanı çalıştıran artık, bilgisayarınızı şimdi başka bir deyişle, bir sınır cihazının benzetimini yapar. Edge Cihazınızı gönderme tutar sensorthing benzetimini sonraki adımdır. Komut Palette yazıp seçin **kenar: kenar oluşturmak yapılandırma dosyası**. Ve bu dosyayı oluşturmak için bir klasör seçin. Oluşturulan deployment.json dosyasında satır Değiştir "<registry>/<image>:<tag>" ile `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`.
5. Seçin **kenar: sınır cihazı için dağıtımı oluşturma** ve yeni bir dağıtım oluşturmak için kenar cihaz kimliği seçin. Edge cihaz kimliği aygıt listesinde sağ tıklatın ve seçin **sınır cihazı için dağıtımı oluşturma**. 
6. Docker explorer'ın sanal algılayıcı ile çalıştırma başlatın, IOT kenar görmeniz gerekir. Docker Explorer'da kapsayıcıya sağ tıklayın. Docker günlükleri her modül için izleyebilirsiniz.
7. Edge cihaz Kimliğinizi sağ tıklayın ve VS code'da D2C iletileri izleyebilirsiniz.
8. IOT kenar çalışma zamanı ve algılayıcı modülü durdurmak için türü seçin ve **kenar: Durdur kenar** komutu paletindeki.

## <a name="develop-and-deploy-a-c-module-in-vs-code"></a>Geliştirme ve bir C# modül VS code'da dağıtma
Öğreticideki [bir C# modül geliştirmek](https://docs.microsoft.com/azure/iot-edge/tutorial-csharp-module), güncelleştirme, yapı, VS Code'da modülü görüntünüzü yayımlama ve C# modülünü dağıtmak için Azure portalını ziyaret edin. Bu bölüm, dağıtmak ve C# modül izlemek için VS Code kullanma tanıtır.

### <a name="start-a-local-docker-registry"></a>Bir yerel docker kayıt defteri Başlat
Bu öğretici için Docker uyumlu kayıt kullanabilirsiniz. İki popüler Docker kayıt defteri hizmetlerinin kullanılabilir bulutta [Azure kapsayıcı kayıt defteri](https://docs.microsoft.com/azure/container-registry/) ve [Docker hub'a](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). Bu bölümde kullanan bir [yerel Docker kayıt](https://docs.docker.com/registry/deploying/), erken geliştirme sırasında amacı test etmek için daha kolay olduğu.
VS code'da **tümleşik terminal**(Ctrl + '), komutları yerel bir kayıt defteri başlatmak için aşağıdaki Çalıştır.  

```cmd/sh
docker run -d -p 5000:5000 --name registry registry:2 
```

> [!NOTE]
> Yukarıdaki örnekte yalnızca test etmek için uygun kayıt defteri yapılandırmaları gösterilmiştir. Üretime hazır kayıt defteri TLS tarafından korunması gerekir ve bir erişim denetim mekanizmasını ideal kullanmanız gerekir. Şunu kullanmanızı öneririz [Azure kapsayıcı kayıt defteri](https://docs.microsoft.com/azure/container-registry/) veya [Docker hub'a](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags) üretime hazır IOT kenar modülleri dağıtmak için.

### <a name="create-an-iot-edge-module-project"></a>Bir IOT kenar modülü projesi oluşturma
Bir IOT kenar modülü oluşturmak için .NET tabanlı nasıl 2.0 kullanarak Visual Studio Code ve Azure IOT kenar uzantısı çekirdek aşağıdaki adımları gösterir. Bu bölümde önceki öğreticide tamamladıysanız, güvenli bir şekilde bu bölümü atlayabilirsiniz.
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


### <a name="create-a-docker-image-and-publish-it-to-your-registry"></a>Docker görüntü oluşturma ve kayıt defterine yayımlama

1. VS Code Explorer'da genişletin **Docker** klasör. Kapsayıcı platformunuz için klasör ya da genişletin **linux x64** veya **windows nano**.
2. Sağ **Dockerfile** dosya ve tıklayın **yapı IOT kenar modülü Docker görüntü**. 
3. İçinde **Klasör Seç** penceresinde göz atın veya girin `./bin/Debug/netcoreapp2.0/publish`. Tıklatın **EXE_DIR Klasör Seç**.
4. VS Code pencerenin üstündeki açılır metin kutusuna görüntü adı girin. Örneğin: `<your container registry address>/filtermodule:latest`. Yerel kayıt defterine dağıtıyorsanız, olmalıdır `localhost:5000/filtermodule:latest`.
5. Görüntü Docker deponuza iletin. Kullanım **kenar: anında IOT kenar modülü Docker görüntü** komut ve VS Code pencerenin üstündeki açılır metin kutusuna resim URL'si girin. Yukarıdaki adım içinde kullanılan aynı resim URL'si kullanın.

### <a name="deploy-your-iot-edge-modules"></a>IOT kenar modüllerinizi dağıtma

1. Açık `deployment.json` dosya, yerine **modülleri** altındaki içerik bölümü ile:
    ```json
    "tempSensor": {
        "version": "1.0",
        "type": "docker",
        "status": "running",
        "restartPolicy": "always",
        "settings": {
            "image": "microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview",
            "createOptions": ""
        }
    },
    "filtermodule": {
        "version": "1.0",
        "type": "docker",
        "status": "running",
        "restartPolicy": "always",
        "settings": {
            "image": "localhost:5000/filtermodule:latest",
            "createOptions": ""
        }
    }
    ```

2. Değiştir **yollar** altındaki içerik bölümü ile:
    ```json
    {
        "routes": {
            "sensorToFilter": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filtermodule/inputs/input1\")",
            "filterToIoTHub": "FROM /messages/modules/filtermodule/outputs/output1 INTO $upstream"
        }
    }
    ```
   > [!NOTE]
   > Burada bu iletileri akış çalışma zamanında bildirim temelli kuralları tanımlar. Bu öğreticide, iki yol gerekir. İlk yol FilterMessages işleyici ile yapılandırılmış uç noktası olduğu sıcaklık algılayıcısı iletilerden "input1" uç noktası aracılığıyla filtresi modülü için taşımaları. İkinci yol filtresi modülü gelen iletileri IOT Hub'ına taşımaları. Bu rotadaki kenar Hub'ın IOT Hub'ına iletileri göndermek için söyler özel bir hedef Yukarı Akış.

3. Bu dosyayı kaydedin.
4. Komut Palette seçin **kenar: sınır cihazı için dağıtımı oluşturma**. Ardından IOT kenar cihaz Kimliğinizi bir dağıtım oluşturmak için seçin. Cihaz kimliği aygıt listesinde sağ tıklatın ve seçin **sınır cihazı için dağıtımı oluşturma**.
5. Seçin `deployment.json` , güncelleştirildi. Çıktı penceresinde dağıtımınız için karşılık gelen çıkışları görebilirsiniz.
6. Edge çalışma zamanı komutu Palette başlatın. **Edge: Başlangıç köşesi**
7. Çalışma zamanı Başlat Docker Explorer'da benzetimli algılayıcı ve filtresi modülü ile çalışmasını, IOT kenar görebilirsiniz.
8. Edge cihaz Kimliğinizi sağ tıklayın ve VS code'da D2C iletileri izleyebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide bir IOT kenar modülü oluşturulan ve VS code'da IOT sınır cihazı için dağıtılır. VS code'da Azure IOT kenar geliştirirken diğer senaryolar hakkında bilgi edinmek için aşağıdaki öğreticileri herhangi birini açın devam edebilirsiniz.

> [!div class="nextstepaction"]
> [C# VS Code modülünde hata ayıklama](how-to-vscode-debug-csharp-module.md)
