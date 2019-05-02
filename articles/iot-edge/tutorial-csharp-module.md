---
title: Öğretici geliştirme C# Linux - Azure IOT Edge Modülü | Microsoft Docs
description: Bu öğreticide bir IOT Edge modülü ile oluşturma işlemi gösterilmektedir C# kod ve bir Linux IOT Edge cihazına dağıtma.
services: iot-edge
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 04/23/2019
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: 55cec146382f0b64a38bf2d954b81051ba109b27
ms.sourcegitcommit: 2c09af866f6cc3b2169e84100daea0aac9fc7fd0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64876040"
---
# <a name="tutorial-develop-a-c-iot-edge-module-for-linux-devices"></a>Öğretici: Geliştirme bir C# Linux cihazları için IOT Edge Modülü

Geliştirmek için Visual Studio Code'u kullanma C# kod ve Azure IOT Edge çalıştıran bir Linux cihazına dağıtma. 

İş mantığınızı uygulayan kodu doğrudan IoT Edge cihazlarınıza dağıtmak için Azure IoT Edge modüllerini kullanabilirsiniz. Bu öğreticide, algılayıcı verilerini filtreleyen bir IoT Edge modülü oluşturma ve dağıtma işlemlerinin adımları açıklanmaktadır. [Windows](quickstart.md)'da veya [Linux](quickstart-linux.md)'ta sanal bir cihaza Azure IoT Edge dağıtma hızlı başlangıçlarında oluşturduğunuz sanal IoT Edge cihazınızı kullanacaksınız. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:    

> [!div class="checklist"]
> * Visual Studio Code, üzerinde .NET Core 2.1 SDK dayalı bir IOT Edge modülünüzü oluşturmak için kullanın.
> * Visual Studio Code ve Docker kullanarak bir Docker görüntüsü oluşturma ve bunu kayıt defterinizde yayımlama.
> * Modülü IoT Edge cihazınıza dağıtma.
> * Oluşturulan verileri görüntüleme.

Bu öğreticide oluşturacağınız IoT Edge modülü, cihazınız tarafından oluşturulan sıcaklık verilerini filtreler. İletileri yalnızca sıcaklık belirtilen bir eşiğin üzerindeyse yukarı yönde gönderir. Bu tür bir analiz, buluta iletilen ve bulutta depolanan veri miktarını azaltmak için yararlıdır. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="solution-scope"></a>Çözüm kapsamı

Bu öğreticide bir modülde nasıl geliştirilebileceğini gösterir **C#** kullanarak **Visual Studio Code**ve dağıtmak nasıl bir **Linux cihaz**. Windows cihazlar için modülleri geliştiriyorsanız, Git [geliştirme bir C# Windows cihazları için IOT Edge Modülü](tutorial-csharp-module-windows.md) yerine.

Geliştirme ve Linux için C modülleri dağıtma seçeneklerinizi anlamak için aşağıdaki tabloyu kullanın: 

| C# | Visual Studio Code | Visual Studio 2017 | 
| -- | ------------------ | ------------------ |
| **Linux AMD64** | ![C#VS code'da LinuxAMD64 için modüller](./media/tutorial-c-module/green-check.png) | ![C#Visual Studio'da LinuxAMD64 için modüller](./media/tutorial-c-module/green-check.png) |
| **Linux ARM32** | ![C#VS code'da LinuxARM32 için modüller](./media/tutorial-c-module/green-check.png) | ![C#Visual Studio'da LinuxARM64 için modüller](./media/tutorial-c-module/green-check.png) |

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce geliştirme ortamınızı ayarlama için önceki öğreticide çalıştınız [Linux cihaz için bir IOT Edge modülü geliştirme](tutorial-develop-for-linux.md). Bu öğreticiyi tamamladıktan sonra aşağıdaki önkoşulları olmanız gerekir: 

* Azure'da ücretsiz veya standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md).
* A [Azure IOT Edge çalıştıran Linux cihaz](quickstart-linux.md).
* Kapsayıcı kayıt defteri gibi [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/).
* [Visual Studio Code](https://code.visualstudio.com/) ile yapılandırılmış [Azure IOT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) uzantısı.
* [Docker CE](https://docs.docker.com/install/) Linux kapsayıcıları çalıştırmak üzere yapılandırılmış.

Bu öğreticiler tamamlamak için aşağıdaki ek önkoşulları geliştirme makinenizde hazırlayın: 

* [Visual Studio Code için C# (OmniSharp tarafından desteklenen) uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).
* [.NET Core 2.1 SDK'sı](https://www.microsoft.com/net/download).

## <a name="create-a-module-project"></a>Bir modülü projesi oluşturma

Bir IOT Edge modülü projesi için aşağıdaki adımları oluşturma C# Visual Studio Code ve Azure IOT araçları uzantısını kullanarak. Oluşturulan proje şablonu oluşturduktan sonra böylece modül bildirilen özelliklerine göre iletileri filtreler yeni kod ekleyin. 

### <a name="create-a-new-project"></a>Yeni bir proje oluşturma

Kendi yazacağınız kodla özelleştirebileceğiniz bir C# çözüm şablonu oluşturun. 

1. Visual Studio Code'da VS Code komut paletini açmak için **View (Görünüm)** > **Command Palette (Komut Paleti)** öğesini seçin. 

2. Komut Paleti'nde girin ve şu komutu çalıştırın **Azure: Oturum** ve Azure hesabınızda oturum açmak için yönergeleri izleyin. Oturumu önceden açtıysanız bu adımı atlayabilirsiniz.

3. Komut Paleti'nde girin ve şu komutu çalıştırın **Azure IOT Edge: Yeni bir IOT Edge çözüm**. Çözümünüzü oluşturmak için komut paletindeki yönergeleri izleyin.

   | Alan | Değer |
   | ----- | ----- |
   | Klasör seçin | Geliştirme makinenizde VS Code'un çözüm dosyalarını oluşturmak için kullanacağı konumu seçin. |
   | Çözüm adı sağlayın | Çözümünüz için açıklayıcı bir ad girin veya varsayılan değerleri kabul **EdgeSolution**. |
   | Modül şablonunu seçin | Seçin  **C# Modülü**. |
   | Modül adı sağlayın | Modülünüze **CSharpModule** adını verin. |
   | Modül için Docker görüntü deposunu sağlama | Görüntü deposu, kapsayıcı kayıt defterinizin adını ve kapsayıcı görüntünüzün adını içerir. Kapsayıcı görüntünüzü, son adımda sağladığınız adından doldurulur. **localhost:5000** yerine Azure kapsayıcı kayıt defterinizden alacağınız oturum açma sunucusu değerini yazın. Oturum açma sunucusunu Azure portalda kapsayıcı kayıt defterinizin Genel bakış sayfasından alabilirsiniz. <br><br>Son görüntü deposuna benzer \<kayıt defteri adı\>.azurecr.io/csharpmodule. |
 
   ![Docker görüntü deposunu sağlama](./media/tutorial-csharp-module/repository.png)


### <a name="add-your-registry-credentials"></a>Kayıt defteri kimlik bilgilerinizi ekleme

Ortam dosyası, kapsayıcı kayıt defterinizin kimlik bilgilerini depolar ve bu bilgileri IoT Edge çalışma zamanı ile paylaşır. Çalışma zamanı, özel görüntülerinizi IoT Edge cihazına çekmek için bu kimlik bilgilerine ihtiyaç duyar. Kimlik bilgileri kullanmak **erişim anahtarları** Azure kapsayıcı kayıt defterinizin bölümü. 

1. VS Code gezgininde **.env** dosyasını açın. 
2. Alanları ile güncelleştirme **kullanıcıadı** ve **parola** , Azure kapsayıcı kayıt defteri değerleri. 
3. Bu dosyayı kaydedin. 

### <a name="select-your-target-architecture"></a>Hedef Mimarinizi seçin

Şu anda, Visual Studio Code, C modülleri Linux AMD64 ve Linux ARM32v7 cihazlar için geliştirebilirsiniz. Kapsayıcı oluşturulur ve her bir mimari türü için farklı çalıştır olduğundan, her bir çözüm ile hedeflediğiniz hangi mimari seçmeniz gerekir. Linux AMD64 varsayılandır. 

1. Komut paletini açın ve arama **Azure IOT Edge: Varsayılan hedef Platform için Edge çözümü ayarlayın**, veya pencerenin alt kısmındaki kenar çubuğu kısayol simgesini seçin. 

2. Komut paletini hedef mimari seçeneklerini listeden seçin. Bu öğreticide, bir Ubuntu sanal makinesi varsayılan tutacak şekilde IOT Edge cihazı kullandığımız **amd64**. 

### <a name="update-the-module-with-custom-code"></a>Modülü özel kodla güncelleştirme

1. VS Code gezgininde **modules** > **CSharpModule** > **main.py** girişini açın.

2. **CSharpModule** ad alanının en üst kısmına daha sonra kullanılan türler için üç **using** deyimi yazın:

    ```csharp
    using System.Collections.Generic;     // For KeyValuePair<>
    using Microsoft.Azure.Devices.Shared; // For TwinCollection
    using Newtonsoft.Json;                // For JsonConvert
    ```

3. **Program** sınıfına **temperatureThreshold** değişkenini ekleyin. Bu değişken, IoT Hub'a verilerin gönderilmesi için ölçülen sıcaklığın aşması gereken değeri ayarlar. 

    ```csharp
    static int temperatureThreshold { get; set; } = 25;
    ```

4. **Program** sınıfına **MessageBody**, **Machine** ve **Ambient** sınıflarını ekleyin. Bu sınıflar gelen iletilerin gövdesi için beklenen şemayı tanımlar.

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

5. Bulma **Init** işlevi. Bu işlev oluşturur ve yapılandırır bir **ModuleClient** ileti göndermek ve almak için yerel Azure IOT Edge çalışma zamanına bağlanmak modülü sağlayan nesne. **ModuleClient** nesnesi oluşturulduktan sonra, kod modül ikizinin istenen özelliklerinden **temperatureThreshold** değerini okur. Kod, **input1** uç noktası yoluyla IoT Edge hub'ından iletileri almak için bir geri arama kaydeder. **SetInputMessageHandlerAsync** yöntemini yeni bir yöntemle değiştirin ve istenen özelliklerdeki güncelleştirmeler için bir **SetDesiredPropertyUpdateCallbackAsync** yöntemi ekleyin. Bu değişikliği yapmak için, **Init** yönteminin son satırının aşağıdaki kod ile değiştirin:

    ```csharp
    // Register a callback for messages that are received by the module.
    // await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", PipeMessage, iotHubModuleClient);

    // Read the TemperatureThreshold value from the module twin's desired properties
    var moduleTwin = await ioTHubModuleClient.GetTwinAsync();
    await OnDesiredPropertiesUpdate(moduleTwin.Properties.Desired, ioTHubModuleClient);
    
    // Attach a callback for updates to the module twin's desired properties.
    await ioTHubModuleClient.SetDesiredPropertyUpdateCallbackAsync(OnDesiredPropertiesUpdate, null);

    // Register a callback for messages that are received by the module.
    await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FilterMessages, ioTHubModuleClient);
    ```

6. **Program** sınıfına **onDesiredPropertiesUpdate** yöntemini ekleyin. Bu yöntem modül ikizinin istenen özellikleri üzerinde yapılan güncelleştirmeleri alır ve **temperatureThreshold** değişkenini buna göre güncelleştirir. Tüm modüllerin, doğrudan buluttan bir modülün içinde çalışan kodu yapılandırmanıza izin veren kendi modül ikizi vardır.

    ```csharp
    static Task OnDesiredPropertiesUpdate(TwinCollection desiredProperties, object userContext)
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

7. **PipeMessage** yöntemini **FilterMessages** yöntemiyle değiştirin. Modül IoT Edge hub'ından bir ileti aldığında bu yöntem çağrılır. Modül ikizi aracılığıyla ayarlanan sıcaklık eşiğinin altındaki sıcaklıkları rapor eden iletileri filtreler. Ayrıca iletiye **MessageType** özelliğini ekleyip değerini **Alert** olarak ayarlar. 

    ```csharp
    static async Task<MessageResponse> FilterMessages(Message message, object userContext)
    {
        var counterValue = Interlocked.Increment(ref counter);
        try
        {
            ModuleClient moduleClient = (ModuleClient)userContext;
            var messageBytes = message.GetBytes();
            var messageString = Encoding.UTF8.GetString(messageBytes);
            Console.WriteLine($"Received message {counterValue}: [{messageString}]");

            // Get the message body.
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
                await moduleClient.SendEventAsync("output1", filteredMessage);
            }

            // Indicate that the message treatment is completed.
            return MessageResponse.Completed;
        }
        catch (AggregateException ex)
        {
            foreach (Exception exception in ex.InnerExceptions)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", exception);
            }
            // Indicate that the message treatment is not completed.
            var moduleClient = (ModuleClient)userContext;
            return MessageResponse.Abandoned;
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
            // Indicate that the message treatment is not completed.
            ModuleClient moduleClient = (ModuleClient)userContext;
            return MessageResponse.Abandoned;
        }
    }
    ```

8. Program.cs dosyasını kaydedin.

9. VS Code gezgininde IoT Edge çözüm çalışma alanınızdaki **deployment.template.json** dosyasını açın. 

10. Dağıtım bildirimine **CSharpModule** modül ikizini ekleyin. Aşağıdaki JSON içeriğini **modulesContent** bölümünün en altına, **$edgeHub** modül ikizinden sonra ekleyin: 

    ```json
       "CSharpModule": {
           "properties.desired":{
               "TemperatureThreshold":25
           }
       }
    ```

    ![Modül ikizi için dağıtım şablonu Ekle](./media/tutorial-csharp-module/module-twin.png)

11. Deployment.template.json dosyayı kaydedin.


## <a name="build-and-push-your-module"></a>Oluşturun ve modülünüzde gönderin

Önceki bölümde, IOT Edge çözümünü oluşturan ve iletileri kabul edilebilir sınırlar içinde bildirilen makine sıcaklık olduğu filtre CSharpModule kod eklenir. Şimdi çözümü kapsayıcı görüntüsü olarak derlemeniz ve kapsayıcı kayıt defterine göndermeniz gerekiyor.

1. **Görünüm** > **Terminal**'i seçerek VS Code tümleşik terminalini açın.

1. Terminalde aşağıdaki komutu girerek Docker'da oturum açın. Kullanıcı adı, parola ve oturum açma sunucusu, Azure container registry'den oturum açın. Bu değerleri alabilirsiniz **erişim anahtarları** Azure portalında kayıt defterinizin bölümü.
     
   ```bash
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```

   Kullanılmasını öneren bir güvenlik uyarısı alabilirsiniz `--password-stdin`. Bu en iyi uygulama, üretim senaryoları için önerilir, bu öğreticinin kapsamı dışında olan. Daha fazla bilgi için [docker oturum açma](https://docs.docker.com/engine/reference/commandline/login/#provide-a-password-using-stdin) başvuru.

2. VS Code gezgininde **deployment.template.json** dosyasına sağ tıklayıp **Build and Push IoT Edge solution** (IoT Edge Çözümü Oluştur ve Gönder) öğesini seçin.

   Derleme ve gönderme komut üç işlemi başlatır. İlk olarak, adlı bir çözüm içinde yeni bir klasör oluşturur **config** , tam bir dağıtım bildirimi, yerleşik dağıtım şablonu bilgilerinin kullanıma ve diğer çözüm dosyalarını içerir. İkinci olarak, çalıştığında `docker build` uygun dockerfile, hedef mimari için temel kapsayıcı görüntüsünü oluşturmak için. Ardından, çalışan `docker push` kapsayıcı kayıt defterinize görüntü deposuna gönderin.

## <a name="deploy-and-run-the-solution"></a>Çözümü dağıtma ve çalıştırma

IOT Edge cihazınıza modülü projeyi dağıtmak için Visual Studio kod Gezgini ve Azure IOT araçları uzantısını kullanın. Senaryonuz için hazırlanmış bir dağıtım bildirimi zaten **deployment.json** config klasöründeki dosya. Tek yapmanız gereken dağıtımı almak üzere bir cihaz seçmek.

IOT Edge Cihazınızı ve çalışıyor olduğundan emin olun.

1. Visual Studio Code Gezgini'nde **Azure IOT Hub cihazları** IOT cihazlarınızın listesini görmek için bölümü.

2. IoT Edge cihazınızın adına sağ tıklayıp **Create Deployment for Single Device** (Tek bir cihaz için dağıtım oluştur) öğesini seçin. 

5. **config** klasöründeki **deployment.json** dosyasını seçin ve ardından **Select Edge Deployment Manifest** (Edge Dağıtım Bildirimini Seç) öğesine tıklayın. deployment.template.json dosyasını kullanmayın. 

6. Yenile düğmesine tıklayın. Yeni **CSharpModule** ile **TempSensor** modülü ve **$edgeAgent** ile **$edgeHub** bileşenlerinin çalıştığını görmeniz gerekir.  

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

Dağıtım bildirimini IoT Edge cihazınıza uyguladıktan sonra cihazdaki IoT Edge çalışma zamanı yeni dağıtım bilgilerini toplar ve yürütmeye başlar. Cihazda çalışan ve dağıtım bildiriminde bulunmayan modüller durdurulur. Cihazda eksik olan modüller başlatılır.

IoT Edge cihazınızın durumunu görüntülemek için Visual Studio Code gezgininin **Azure IoT Hub Cihazları** bölümünü kullanabilirsiniz. Dağıtılan ve çalışan modüllerin listesini görmek için cihazınızın ayrıntılarını genişletin.

1. Visual Studio Code Gezgininde, IOT Edge cihazınızın adına sağ tıklayıp **izleme D2C iletileri Başlat**.

2. IOT hub'da gelen iletileri görüntüleyin. IOT Edge cihazı, yeni dağıtım almak ve tüm modülleri başlatmak olduğundan bunu ulaşması iletileri için biraz zaman alabilir. Daha sonra iletileri göndermeden önce 25 derece makine sıcaklık ulaşana kadar biz CModule koda yapılan değişiklikleri bekleyin. İleti türü de ekler **uyarı** sıcaklık eşiğe ulaşan tüm iletileri. 

   ![IOT hub'da gelen iletileri görüntüleme](./media/tutorial-csharp-module/view-d2c-message.png)
 
## <a name="edit-the-module-twin"></a>Modül ikizi Düzenle

Biz CSharpModule modül ikizi dağıtım bildiriminde sıcaklık eşik 25 derecede ayarlamak için kullanılır. Modül ikizi modülü kodunu güncelleştirmek zorunda kalmadan işlevlerini değiştirmek için kullanabilirsiniz.

1. Visual Studio Code'da altında çalışan modülleri görmek için IOT Edge cihaz ayrıntıları genişletin. 

2. Sağ **CSharpModule** seçip **düzenleme modül ikizi**. 

3. Bulma **TemperatureThreshold** istenen özellikleri. Değeri 5 10 derece son bildirilen sıcaklık daha yüksek derece için yeni bir sıcaklık değiştirin. 

4. Modül ikizi dosyayı kaydedin.

5. Modül ikizi düzenleme bölmesinde ve seçme içinde herhangi bir yeri sağ **güncelleştirme modül ikizi**. 

5. CİHAZDAN buluta gelen iletileri izleyin. Yeni sıcaklık eşiği ulaşılana kadar Durdur iletileri görmeniz gerekir. 

## <a name="clean-up-resources"></a>Kaynakları temizleme 

Bir sonraki önerilen makaleye geçmeyi planlıyorsanız, oluşturduğunuz kaynaklarla yapılandırmaları tutabilir ve yeniden kullanabilirsiniz. Aynı IoT Edge cihazını test cihazı olarak kullanmaya devam edebilirsiniz. 

Aksi takdirde, yerel yapılandırmaları ve ücretleri önlemek için bu makalede kullanılan Azure kaynaklarını silebilirsiniz. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide IoT Edge cihazınız tarafından üretilen ham verileri filtrelemek için kod içeren bir IoT Edge modülü oluşturdunuz. Kendi modüllerinizi derlemek için hazır olduğunuzda, daha fazla bilgi edinebilirsiniz [kendi IOT Edge modülleri geliştirmek](module-development.md) veya nasıl [modülleri Visual Studio Code ile geliştirme](how-to-vs-code-develop-module.md). Azure IOT Edge, uçta verilerini işlemek ve çözümlemek için Azure bulut Hizmetleri dağıtmayı nasıl yardımcı olabileceğini öğrenmek için sonraki öğreticiler açın devam edebilirsiniz.

> [!div class="nextstepaction"]
> [İşlevleri](tutorial-deploy-function.md)
> [Stream Analytics](tutorial-deploy-stream-analytics.md)
> [makine öğrenimi](tutorial-deploy-machine-learning.md)
> [özel görüntü işleme hizmeti](tutorial-deploy-custom-vision.md)
