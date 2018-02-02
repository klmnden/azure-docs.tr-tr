---
title: "Bir C# modül Azure IOT Edge geliştirmek için Visual Studio Code kullanma | Microsoft Docs"
description: "Geliştir ve bağlam geçmeden bir C# modül Azure IOT kenar Visual Studio Code ile dağıtın."
services: iot-edge
keywords: 
author: shizn
manager: timlt
ms.author: xshi
ms.date: 01/11/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 4cf07d5c4a21fa989e7de6e996cc62424099e3e5
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="use-visual-studio-code-to-develop-a-c-module-with-azure-iot-edge"></a>Bir C# modül Azure IOT Edge geliştirmek için Visual Studio Code kullanma
Bu makalede kullanmaya yönelik ayrıntılı yönergeler sağlanmaktadır [Visual Studio Code](https://code.visualstudio.com/) geliştirmek ve Azure IOT kenar modüllerinizi dağıtmak için ana geliştirme aracı olarak. 

## <a name="prerequisites"></a>Önkoşullar
Bu öğretici, bir bilgisayar veya geliştirme makine olarak Windows veya Linux çalıştıran sanal makine kullandığınızı varsayar. Geliştirme makinenizde IOT kenar Cihazınızı benzetimini yapabilirsiniz veya başka bir fiziksel cihaz IOT sınır cihazı olabilir.

Bu kılavuza başlamadan önce aşağıdaki öğreticiler tamamlayın:
- Sanal bir cihaz üzerinde Azure IOT kenar dağıtmak [Windows](https://docs.microsoft.com/azure/iot-edge/tutorial-simulate-device-windows) veya [Linux](https://docs.microsoft.com/azure/iot-edge/tutorial-simulate-device-linux)
- [Geliştirme ve sanal cihazınız bir C# IOT kenar modülü dağıtma](https://docs.microsoft.com/azure/iot-edge/tutorial-csharp-module)

Önceki öğreticileri tamamladıktan sonra olmalıdır öğeleri gösteren bir denetim listesi aşağıdadır:

- [Visual Studio Code](https://code.visualstudio.com/) 
- [Visual Studio Code için Azure IOT kenar uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
- [C# Visual Studio Code (OmniSharp tarafından desteklenen) uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 
- [Docker](https://docs.docker.com/engine/installation/)
- [.NET Core 2.0 SDK](https://www.microsoft.com/net/core#windowscmd) 
- [Python 2.7](https://www.python.org/downloads/)
- [IOT kenar denetim komut dosyası](https://pypi.python.org/pypi/azure-iot-edge-runtime-ctl)
- AzureIoTEdgeModule şablonu (`dotnet new -i Microsoft.Azure.IoT.Edge.Module`)
- Etkin bir IOT hub en az bir IOT sınır cihazı ile

Yüklemek yararlıdır [VS Code Docker desteği](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) modülü görüntüler ve kapsayıcıları daha iyi yönetebilmek için.

## <a name="deploy-an-azure-iot-edge-module-in-vs-code"></a>VS Code'da Azure IOT kenar modülünde dağıtma

### <a name="list-your-iot-hub-devices"></a>IOT hub cihazları listeleyin
IOT hub aygıtlarınızı VS code'da listelemek için iki yolu vardır. Devam etmek için her iki durumda seçebilirsiniz.

#### <a name="sign-in-to-your-azure-account-in-vs-code-and-choose-your-iot-hub"></a>VS code'da Azure hesabınızda oturum açın ve IOT hub'ı seçin
1. Komut palette (F1 veya Ctrl + Shift + P) yazın ve seçin **Azure: oturum**. Ardından **kopyalama & Aç**. (Ctrl + V) kodu tarayıcınıza yapıştırın ve seçin **devam**. Ardından Azure hesabınızla oturum açın. Hesap bilgilerinizi VS Code durum çubuğunda görebilirsiniz.
2. Komut palette yazıp seçin **IOT: IOT Hub'ı seçin**. İlk olarak önceki öğreticide IOT hub'ınızı oluşturulduğu abonelik seçin. Ardından, IOT sınır cihazı içeren IOT hub'ı seçin.

    ![Cihaz listesinin ekran görüntüsü](./media/how-to-vscode-develop-csharp-module/device-list.png)

#### <a name="set-the-iot-hub-connection-string"></a>IOT hub bağlantı dizesine ayarlayın
Komut palette yazıp seçin **IOT: IOT Hub bağlantı dizesine ayarlamak**. Bağlantı dizesi ilkesi altında yapıştırdığınız emin olun **iothubowner**. (Bu paylaşılan erişim ilkeleri Azure portalında IOT hub'ınızın bulabilirsiniz.)
 
Sol kenar çubuğu IOT Hub cihazları Explorer'da aygıt listesinde görebilirsiniz.

### <a name="start-your-iot-edge-runtime-and-deploy-a-module"></a>IOT kenar çalışma zamanı başlatma ve bir modül dağıtma
Yükleyin ve Azure IOT kenar çalışma zamanı aygıtınızda başlatın. Azure IOT Hub'ına telemetri verileri gönderen benzetimli algılayıcı modül dağıtın.
1. Komut Palette seçin **kenar: Kurulum kenar** ve cihaz kimliği, IOT kenar'ı seçin Alternatif olarak, IOT kenar cihaz kimliği sağ **cihaz listesi**seçip **Kurulum kenar**.

    ![Kurulum kenar ekran çalışma zamanı](./media/how-to-vscode-develop-csharp-module/setup-edge.png)

2. Komut palette seçin **kenar: Başlangıç kenar** IOT kenar çalışma zamanı başlatmak için. Tümleşik terminal karşılık gelen çıktılarında görebilirsiniz.

    ![Başlangıç kenar ekran çalışma zamanı](./media/how-to-vscode-develop-csharp-module/start-edge.png)

3. Docker Explorer'da IOT kenar çalışma zamanı durumunu kontrol edin. Yeşil çalıştığı ve IOT kenar çalışma zamanı başarıyla başlatıldı anlamına gelir. Bilgisayarınızı şimdi bir IOT sınır cihazının benzetimini yapar.

    ![Ekran görüntüsü, kenar çalışma zamanı durumu](./media/how-to-vscode-develop-csharp-module/edge-runtime.png)

4. IOT kenar Cihazınızı gönderme tutan bir sensörü benzetimi gerçekleştirmeye. Komut palette yazıp seçin **kenar: kenar oluşturmak yapılandırma dosyası**. Bu dosyayı oluşturmak için bir klasör seçin. Oluşturulan deployment.json dosyasında içeriğinin yerine geçecek `<registry>/<image>:<tag>` ile `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`ve dosyayı kaydedin.

    ![Algılayıcı modülünün ekran görüntüsü](./media/how-to-vscode-develop-csharp-module/sensor-module.png)

5. Seçin **kenar: sınır cihazı için dağıtımı oluşturma**ve yeni bir dağıtım oluşturmak için cihaz kimliği IOT kenar'ı seçin. Alternatif olarak, IOT kenar cihaz kimliği aygıt listesinde sağ tıklatın ve seçin **sınır cihazı için dağıtımı oluşturma**. 

6. Docker Explorer'ın sanal algılayıcı ile çalıştırma başlatın, IOT kenar görmeniz gerekir. Docker Explorer'da kapsayıcıya sağ tıklayın. Docker günlükleri her modül için izleyebilirsiniz. Ayrıca, modül listesinde cihaz listesinde görüntüleyebilirsiniz.

    ![Modül listesi ekran görüntüsü](./media/how-to-vscode-develop-csharp-module/module-list.png)

7. IOT kenar cihaz Kimliğinizi sağ tıklayın ve VS code'da D2C iletileri izleyebilirsiniz.
8. IOT kenar çalışma zamanı ve algılayıcı modülü durdurmak için yazın ve seçin **kenar: Durdur kenar** komutu paletindeki.

## <a name="develop-and-deploy-a-c-module-in-vs-code"></a>Geliştirme ve bir C# modül VS code'da dağıtma
Öğreticide [bir C# modül geliştirmek](https://docs.microsoft.com/azure/iot-edge/tutorial-csharp-module), güncelleştirme, yapı ve VS Code'da modülü görüntünüzü yayımlama. Ardından, C# modülünü dağıtmak için Azure portalına gidin. Bu bölüm, dağıtmak ve C# modül izlemek için VS Code kullanma tanıtır.

### <a name="start-a-local-docker-registry"></a>Yerel bir Docker kayıt Başlat
Bu öğretici için Docker uyumlu kayıt kullanabilirsiniz. İki popüler Docker kayıt defteri hizmetlerinin kullanılabilir bulutta [Azure kapsayıcı kayıt defteri](https://docs.microsoft.com/azure/container-registry/) ve [Docker hub'a](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). Bu bölümde kullanan bir [yerel Docker kayıt](https://docs.docker.com/registry/deploying/), erken geliştirme sırasında test etmek için daha kolay olduğu.
VS code'da **tümleşik terminal** (Ctrl + '), yerel bir kayıt defteri başlatmak için aşağıdaki komutu çalıştırın:  

```cmd/sh
docker run -d -p 5000:5000 --name registry registry:2 
```

> [!NOTE]
> Bu örnek yalnızca test etmek için uygun kayıt defteri yapılandırmaları gösterilmiştir. Üretime hazır kayıt defteri TLS tarafından korunması gerekir ve bir erişim denetim mekanizmasını ideal kullanmanız gerekir. Şunu kullanmanızı öneririz [Azure kapsayıcı kayıt defteri](https://docs.microsoft.com/azure/container-registry/) veya [Docker hub'a](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags) üretime hazır IOT kenar modülleri dağıtmak için.

### <a name="create-an-iot-edge-module-project"></a>Bir IOT kenar modülü projesi oluşturma
Aşağıdaki adımlar Visual Studio Code ve Azure IOT kenar uzantısını kullanarak .NET Core 2. 0'dayanan bir IOT kenar modülü oluşturulacağını gösterir. Bu bölümde önceki öğreticide tamamladıysanız, güvenli bir şekilde bu bölümü atlayabilirsiniz.
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
4. Göz atın **FilterModule** klasörü ve tıklatın **Klasör Seç** proje VS Code'da açın.
5. VS Code Explorer'da seçin **Program.cs** açın. Üstündeki **program.cs**, şu ad alanlarından içerir:
   ```csharp
   using Microsoft.Azure.Devices.Shared;
   using System.Collections.Generic;  
   using Newtonsoft.Json;
   ```

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

8. İçinde **Init** yöntemi, kod oluşturur ve yapılandırır bir **DeviceClient** nesnesi. Bu nesne ileti gönderme ve alma için yerel IOT kenar çalışma bağlanmak modülü sağlar. IOT kenar çalışma zamanı bağlantı dizesi kullanılan modülü sağlayan **Init** yöntemi. Oluşturduktan sonra **DeviceClient** nesne kodu IOT kenar hub'dan iletileri almak için bir geri çağırma kaydeder **input1** uç noktası. Değiştir `SetInputMessageHandlerAsync` yeni bir yöntemle ve ekleme bir `SetDesiredPropertyUpdateCallbackAsync` istenen özellikleri güncelleştirmeleri yöntemi. Bu değişikliği yapmak için son satırının yerini **Init** aşağıdaki kod ile yöntemi:

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

10. Değiştir `PipeMessage` yöntemiyle `FilterMessages` yöntemi. Modül bir ileti IOT kenar hub'dan aldığı zaman bu yöntem çağrılır. Modül twin ayarlamak sıcaklık eşiğin altına etme Raporu iletilerini çıkışı filtreler. Ayrıca ekler **MessageType** özellik kümesine değerle iletisi **uyarı**. 

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

11. Projeyi derlemek için sağ **FilterModule.csproj** dosya Gezgini'nde ve seçin **yapı IOT kenar Modülü**. Bu işlem modülü derler ve ikili ve bağımlılıklarını Docker görüntüsünü oluşturmak için kullanılan bir klasöre dışa aktarır. 

    ![VS Code Gezgini'nin ekran görüntüsü](./media/how-to-vscode-develop-csharp-module/build-module.png)

### <a name="create-a-docker-image-and-publish-it-to-your-registry"></a>Docker görüntü oluşturma ve kayıt defterine yayımlama

1. VS Code Explorer'da genişletin **Docker** klasör. Kapsayıcı platformunuz için klasör ya da genişletin **linux x64** veya **windows nano**.
2. Sağ **Dockerfile** dosyasını bulun ve seçin **yapı IOT kenar modülü Docker görüntü**. 

    ![VS Code Gezgini'nin ekran görüntüsü](./media/how-to-vscode-develop-csharp-module/build-docker-image.png)

3. İçinde **Klasör Seç** penceresinde göz atın veya girin `./bin/Debug/netcoreapp2.0/publish`. Seçin **EXE_DIR Klasör Seç**.
4. VS Code pencerenin üstündeki açılır metin kutusuna görüntü adı girin. Örneğin: `<your container registry address>/filtermodule:latest`. Yerel kayıt defterine dağıtıyorsanız, olmalıdır `localhost:5000/filtermodule:latest`.
5. Görüntü Docker deponuza iletin. Kullanım **kenar: anında IOT kenar modülü Docker görüntü** komut ve VS Code pencerenin üstündeki açılır metin kutusuna resim URL'si girin. Önceki adımda kullanılan aynı resim URL'si kullanın. Görüntüyü başarıyla gönderilen emin olmak için konsol günlüğünü denetleyin.

    ![Docker görüntü Ftp'den işleminin ekran görüntüsü](./media/how-to-vscode-develop-csharp-module/push-image.png) ![konsol oturum ekran görüntüsü](./media/how-to-vscode-develop-csharp-module/pushed-image.png)

### <a name="deploy-your-iot-edge-modules"></a>IOT kenar modüllerinizi dağıtma

1. Açık `deployment.json` dosya ve değiştirme **modülleri** aşağıdaki bölümde:
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

2. Değiştir **yollar** aşağıdaki bölümde:
    ```json
    "sensorToFilter": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filtermodule/inputs/input1\")",
    "filterToIoTHub": "FROM /messages/modules/filtermodule/outputs/output1 INTO $upstream"
    ```
   > [!NOTE]
   > Burada bu iletileri akış çalışma zamanında bildirim temelli kuralları tanımlar. Bu öğreticide, iki yol gerekir. İlk yol sıcaklık algılayıcısı iletilerden "input1" uç noktası aracılığıyla filtresi modülü için taşımaları. Bu FilterMessages işleyici ile yapılandırılmış uç noktadır. İkinci yol filtresi modülü gelen iletileri IOT Hub'ına taşımaları. Bu yol, IOT Hub'ına iletileri göndermek için IOT kenar Hub söyler özel bir hedef Yukarı Akış.

3. Bu dosyayı kaydedin.
4. Komut palette seçin **kenar: sınır cihazı için dağıtımı oluşturma**. Ardından IOT kenar cihaz Kimliğinizi bir dağıtım oluşturmak için seçin. Veya, cihaz kimliği aygıt listesinde sağ tıklatın ve seçin **sınır cihazı için dağıtımı oluşturma**.

    ![Dağıtım seçeneği, ekran oluşturma](./media/how-to-vscode-develop-csharp-module/create-deployment.png)

5. Seçin `deployment.json` , güncelleştirildi. Çıktı penceresinde dağıtımınız için karşılık gelen çıkışları görebilirsiniz.

    ![Çıktı penceresi ekran görüntüsü](./media/how-to-vscode-develop-csharp-module/deployment-succeeded.png)

6. IOT kenar çalışma zamanı komutu palette başlatın (seçin **kenar: Başlangıç kenar**).
7. Çalışma zamanı Başlat Docker Explorer'da benzetimli algılayıcı ve filtresi modülü ile çalışmasını, IOT kenar görebilirsiniz.

    ![Docker Gezgini'nin ekran görüntüsü](./media/how-to-vscode-develop-csharp-module/solution-running.png)

8. IOT kenar cihaz Kimliğinizi sağ tıklayın ve VS code'da D2C iletileri izleyebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide bir IOT kenar modülü oluşturulan ve VS code'da IOT sınır cihazı için dağıtılabilir. VS code'da Azure IOT kenar geliştirirken diğer senaryolar hakkında bilgi edinmek için aşağıdaki öğretici bakın:

> [!div class="nextstepaction"]
> [C# VS Code modülünde hata ayıklama](how-to-vscode-debug-csharp-module.md)
