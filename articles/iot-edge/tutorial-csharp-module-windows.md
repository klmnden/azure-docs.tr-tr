---
title: Öğretici geliştirme C# için Windows - Azure IOT Edge Modülü | Microsoft Docs
description: Bu öğreticide bir IOT Edge modülü ile oluşturma işlemi gösterilmektedir C# kod ve bir Windows IOT Edge cihazına dağıtma.
services: iot-edge
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 04/23/2019
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: d7ccce1f21b1caa2268317b7239617a80ddce10b
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67485924"
---
# <a name="tutorial-develop-a-c-iot-edge-module-for-windows-devices"></a>Öğretici: Geliştirme bir C# Windows cihazları için IOT Edge Modülü

Geliştirmek için Visual Studio kullanma C# kod ve Azure IOT Edge çalıştıran bir Windows cihazına dağıtma. 

İş mantığınızı uygulayan kodu doğrudan IoT Edge cihazlarınıza dağıtmak için Azure IoT Edge modüllerini kullanabilirsiniz. Bu öğreticide, algılayıcı verilerini filtreleyen bir IoT Edge modülü oluşturma ve dağıtma işlemlerinin adımları açıklanmaktadır. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:    

> [!div class="checklist"]
> * Temel alan bir IOT Edge modülünüzü oluşturmak için Visual Studio'yu kullanın C# SDK.
> * Bir Docker görüntüsü oluşturma ve bunu kayıt defterinize yayımlama için Visual Studio ve Docker'ı kullanın.
> * Modülü IoT Edge cihazınıza dağıtma.
> * Oluşturulan verileri görüntüleme.

Bu öğreticide oluşturacağınız IoT Edge modülü, cihazınız tarafından oluşturulan sıcaklık verilerini filtreler. İletileri yalnızca sıcaklık belirtilen bir eşiğin üzerindeyse yukarı yönde gönderir. Bu tür bir analiz, buluta iletilen ve bulutta depolanan veri miktarını azaltmak için yararlıdır. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="solution-scope"></a>Çözüm kapsamı

Bu öğreticide bir modülde nasıl geliştirilebileceğini gösterir **C#** kullanarak **Visual Studio 2019**ve dağıtmak nasıl bir **Windows cihaz**. Modüller için Linux cihazlarını geliştiriyorsanız, Git [geliştirme bir C# Linux cihazları için IOT Edge Modülü](tutorial-csharp-module.md) yerine. 

Geliştirme ve C modülleri Windows cihazlarına dağıtma seçeneklerinizi anlamak için aşağıdaki tabloyu kullanın: 

| C# | Visual Studio Code | Visual Studio 2017/2019 | 
| -- | ------------------ | ------------------ |
| **Windows AMD64 geliştirin** | ![Geliştirme C# modüller için VS code'da WinAMD64](./media/tutorial-c-module/green-check.png) | ![Geliştirme C# WinAMD64 Visual Studio için modüller](./media/tutorial-c-module/green-check.png) |
| **Windows AMD64 hata ayıklama** |   | ![Hata ayıklama C# WinAMD64 Visual Studio için modüller](./media/tutorial-c-module/green-check.png) |

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce geliştirme ortamınızı ayarlama için önceki öğreticide çalıştınız [Windows cihaz için bir IOT Edge modülü geliştirme](tutorial-develop-for-windows.md). Bu öğreticiyi tamamladıktan sonra aşağıdaki önkoşulları olmanız gerekir: 

* Azure'da ücretsiz veya standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md).
* A [Azure IOT Edge çalıştıran Windows cihazı](quickstart.md).
* Kapsayıcı kayıt defteri gibi [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/).
* [Visual Studio 2019](https://docs.microsoft.com/visualstudio/install/install-visual-studio) ile yapılandırılmış [Azure IOT Edge araçlarını](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vs16iotedgetools) uzantısı.
* [Docker CE](https://docs.docker.com/install/) Windows kapsayıcılarını çalıştırmaya yönelik yapılandırılmış.

> [!TIP]
> Visual Studio 2017 (sürüm 15.7 veya üzeri) kullanıyorsanız, plrease yükleyip [Azure IOT Edge araçlarını](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools) VS 2017 Visual Studio Market

## <a name="create-a-module-project"></a>Bir modülü projesi oluşturma

Aşağıdaki adımlar Visual Studio ve Azure IOT Edge araçları uzantısını kullanarak bir IOT Edge modülü projesi oluşturur. Oluşturulan proje şablonu oluşturduktan sonra böylece modül bildirilen özelliklerine göre iletileri filtreler yeni kod ekleyin. 

### <a name="create-a-new-project"></a>Yeni bir proje oluşturma

Azure IOT Edge araçları, için desteklenen tüm IOT Edge modülü dilleri Visual Studio Proje şablonları sağlar. Bu şablonları tüm dosyaları ve IOT Edge test etmek için bir çalışma modül dağıtmak için ihtiyacınız olan kod veya kendi iş mantığına sahip şablonu özelleştirmek için bir başlangıç noktası sağlar. 

1. Visual Studio 2019 başlatın ve seçin **yeni proje oluştur**.

2. Yeni Proje penceresinde, arama **IOT Edge** projesini ve ardından **Azure IOT Edge (Windows amd64)** proje. **İleri**’ye tıklayın. 

   ![Yeni Azure IOT Edge projesi oluşturma](./media/tutorial-csharp-module-windows/new-project.png)

3. Yapılandırma, yeni proje penceresini benzer bir şey açıklayıcı projeyi ve çözümü yeniden adlandır **CSharpTutorialApp**. Tıklayın **Oluştur** projeyi oluşturmak için. 

   ![Yeni bir Azure IOT Edge proje yapılandırma](./media/tutorial-csharp-module-windows/configure-project.png)

4. IOT Edge uygulama ve modül penceresinde, projenize aşağıdaki değerleri yapılandırın: 

   | Alan | Değer |
   | ----- | ----- |
   | Bir şablon seçin | Seçin  **C# Modülü**. | 
   | Modül proje adı | Modülünüze **CSharpModule** adını verin. | 
   | Docker görüntü deposu | Görüntü deposu, kapsayıcı kayıt defterinizin adını ve kapsayıcı görüntünüzün adını içerir. Kapsayıcı görüntünüzü modülü proje adı değerini doldurulur. **localhost:5000** yerine Azure kapsayıcı kayıt defterinizden alacağınız oturum açma sunucusu değerini yazın. Oturum açma sunucusunu Azure portalda kapsayıcı kayıt defterinizin Genel bakış sayfasından alabilirsiniz. <br><br> Son görüntü deposuna benzer \<kayıt defteri adı\>.azurecr.io/csharpmodule. |

   ![Hedef cihaz, modül türü ve kapsayıcı kayıt defteri için projenizi yapılandırın](./media/tutorial-csharp-module-windows/add-application-and-module.png)

5. Seçin **Tamam** yaptığınız değişiklikleri uygulamak için. 

### <a name="add-your-registry-credentials"></a>Kayıt defteri kimlik bilgilerinizi ekleme

Dağıtım bildirimi IOT Edge çalışma zamanı ile kapsayıcı kayıt defterinizin kimlik bilgilerini paylaşır. Çalışma zamanı, özel görüntülerinizi IoT Edge cihazına çekmek için bu kimlik bilgilerine ihtiyaç duyar. Kimlik bilgileri kullanmak **erişim anahtarları** Azure kapsayıcı kayıt defterinizin bölümü. 

1. Visual Studio Çözüm Gezgini'nde açın **deployment.template.json** dosya. 

2. Bulma **registryCredentials** $edgeAgent özelliğinde istenen özellikleri. 

3. Özelliği bu biçim izleyen, kimlik bilgileriyle güncelleştirin: 

   ```json
   "registryCredentials": {
     "<registry name>": {
       "username": "<username>",
       "password": "<password>",
       "address": "<registry name>.azurecr.io"
     }
   }
   ```

4. Deployment.template.json dosyayı kaydedin. 

### <a name="update-the-module-with-custom-code"></a>Modülü özel kodla güncelleştirme

Varsayılan modülü kodu, bir giriş kuyruğundaki iletileri alır ve bunları boyunca bir çıkış kuyruğuna aktarır. Böylece IOT Hub'ına iletmeden önce modülün uçta iletileri işleyen ek biraz kod ekleyelim. Modül güncelleştirin, böylece her ileti sıcaklık verileri analiz eder ve yalnızca sıcaklık belirli bir eşiği aşarsa, IOT Hub'ına ileti gönderir. 

1. Visual Studio'da açın **CSharpModule** > **Program.cs**.

2. **CSharpModule** ad alanının en üst kısmına daha sonra kullanılan türler için üç **using** deyimi yazın:

    ```csharp
    using System.Collections.Generic;     // For KeyValuePair<>
    using Microsoft.Azure.Devices.Shared; // For TwinCollection
    using Newtonsoft.Json;                // For JsonConvert
    ```

3. Ekleme **temperatureThreshold** değişkenini **Program** sayaç değişkeni sonra sınıfı. IOT hub'ına gönderilecek veriler için ölçülen sıcaklık aşması gereken değer temperatureThreshold değişkeni ayarlar. 

    ```csharp
    static int temperatureThreshold { get; set; } = 25;
    ```

4. Ekleme **MessageBody**, **makine**, ve **Ambient** için sınıflar **Program** değişken bildirimlerini sonra sınıfı. Bu sınıflar gelen iletilerin gövdesi için beklenen şemayı tanımlar.

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

5. Bulma **Init** yöntemi. Bu yöntem, oluşturur ve yapılandırır bir **ModuleClient** ileti göndermek ve almak için yerel Azure IOT Edge çalışma zamanına bağlanmak modülü sağlayan nesne. Kod ayrıca bir IOT Edge hub'ından ileti almak için bir geri çağırma kaydeder **input1** uç noktası.

   Tüm Init yöntemine aşağıdaki kodla değiştirin:
   
   ```csharp
   static async Task Init()
   {
       AmqpTransportSettings amqpSetting = new AmqpTransportSettings(TransportType.Amqp_Tcp_Only);
       ITransportSettings[] settings = { amqpSetting };

       // Open a connection to the Edge runtime
       ModuleClient ioTHubModuleClient = await ModuleClient.CreateFromEnvironmentAsync(settings);
       await ioTHubModuleClient.OpenAsync();
       Console.WriteLine("IoT Hub module client initialized.");

       // Read the TemperatureThreshold value from the module twin's desired properties
       var moduleTwin = await ioTHubModuleClient.GetTwinAsync();
       await OnDesiredPropertiesUpdate(moduleTwin.Properties.Desired, ioTHubModuleClient);

       // Attach a callback for updates to the module twin's desired properties.
       await ioTHubModuleClient.SetDesiredPropertyUpdateCallbackAsync(OnDesiredPropertiesUpdate, null);

       // Register a callback for messages that are received by the module.
       await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FilterMessages, ioTHubModuleClient);
   }
   ```
   
   Bu güncelleştirilmiş Init yöntemi yine IOT Edge çalışma zamanı ModuleClient ile bağlantı kurar, ancak ayrıca yeni işlevler ekler. Modül ikizinin istenen özellikleri almak için okuduğu **temperatureThreshold** değeri. Sonra ileride gerçekleştirilecek güncelleştirmelerde modül ikizinin istenen özellikleri için bekleyen bir geri çağırma oluşturur. Bu geri araması ile modül ikizi sıcaklık Eşikte uzaktan güncelleştirin ve değişiklikleri modüle eklenecektir. 

   Var olan güncelleştirilmiş Init yöntemi de değişir **SetInputMessageHandlerAsync** yöntemi. Örnek kodda, gelen iletileri üzerinde *input1* ile işlendi *PipeMessage* işlevi, ancak kullanacak şekilde değiştirmek istediğiniz *FilterMessages* , işlevi Aşağıdaki adımlarda oluşturacağız. 

6. Yeni bir **onDesiredPropertiesUpdate** yönteme **Program** sınıfı. Bu yöntem modül ikizinin istenen özellikleri üzerinde yapılan güncelleştirmeleri alır ve **temperatureThreshold** değişkenini buna göre güncelleştirir. Tüm modüllerin, doğrudan buluttan bir modülün içinde çalışan kodu yapılandırmanıza izin veren kendi modül ikizi vardır.

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

7. Örnek kaldırma **PipeMessage** yöntemi yeni bir değiştirin **FilterMessages** yöntemi. Modül IoT Edge hub'ından bir ileti aldığında bu yöntem çağrılır. Modül ikizi aracılığıyla ayarlanan sıcaklık eşiğinin altındaki sıcaklıkları rapor eden iletileri filtreler. Ayrıca iletiye **MessageType** özelliğini ekleyip değerini **Alert** olarak ayarlar. 

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

9. Açık **deployment.template.json** IOT Edge çözümünüzdeki dosya. Bu dosya, bu durumda dağıtmak için hangi modülü IOT Edge Aracısı söyler **tempSensor** ve **CSharpModule**ve bunlar arasında iletileri yönlendirme hakkında IOT Edge hub'ı söyler.

10. Dağıtım bildirimine **CSharpModule** modül ikizini ekleyin. Aşağıdaki JSON içeriğini **modulesContent** bölümünün en altına, **$edgeHub** modül ikizinden sonra ekleyin: 

    ```json
       "CSharpModule": {
           "properties.desired":{
               "TemperatureThreshold":25
           }
       }
    ```

    ![Modül ikizi için dağıtım şablonu Ekle](./media/tutorial-csharp-module-windows/module-twin.png)

11. Deployment.template.json dosyayı kaydedin.


## <a name="build-and-push-your-module"></a>Oluşturun ve modülünüzde gönderin

Bir önceki bölümde bir IoT Edge çözümü oluşturdunuz ve **CSharpModule** modülüne makine sıcaklığının kabul edilebilir eşiğin altında olduğunu bildiren iletileri filtreleyen kodu eklediniz. Şimdi çözümü kapsayıcı görüntüsü olarak derlemeniz ve kapsayıcı kayıt defterine göndermeniz gerekiyor. 

1. Docker için geliştirme makinenizde oturum açmak için aşağıdaki komutu kullanın. Kullanıcı adı, parola ve oturum açma sunucusu, Azure container registry'den kullanın. Bu değerleri alabilirsiniz **erişim anahtarları** Azure portalında kayıt defterinizin bölümü.

   ```cmd
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```

   Kullanılmasını öneren bir güvenlik uyarısı alabilirsiniz `--password-stdin`. Bu en iyi uygulama, üretim senaryoları için önerilir, bu öğreticinin kapsamı dışında olan. Daha fazla bilgi için [docker oturum açma](https://docs.docker.com/engine/reference/commandline/login/#provide-a-password-using-stdin) başvuru.

2. Visual Studio Çözüm Gezgini'nde, derlemek istediğiniz proje adına sağ tıklayın. Varsayılan ad **AzureIotEdgeApp1** ve Windows modülü oluştururken bu yana uzantısı olmalıdır **Windows.Amd64**. 

3. Seçin **oluşturun ve gönderin, IOT Edge modülleri**. 

   Derleme ve gönderme komut üç işlemi başlatır. İlk olarak, adlı bir çözüm içinde yeni bir klasör oluşturur **config** , tam bir dağıtım bildirimi, yerleşik dağıtım şablonu bilgilerinin kullanıma ve diğer çözüm dosyalarını içerir. İkinci olarak, çalıştığında `docker build` uygun dockerfile, hedef mimari için temel kapsayıcı görüntüsünü oluşturmak için. Ardından, çalışan `docker push` kapsayıcı kayıt defterinize görüntü deposuna gönderin. 

## <a name="deploy-modules-to-device"></a>Modüller cihazına dağıtma

IOT Edge cihazınıza modülü projeyi dağıtmak için Visual Studio cloud explorer ve Azure IOT Edge araçları uzantısını kullanın. Senaryonuz için hazırlanmış bir dağıtım bildirimi zaten **deployment.json** config klasöründeki dosya. Tek yapmanız gereken dağıtımı almak üzere bir cihaz seçmek.

IOT Edge Cihazınızı ve çalışıyor olduğundan emin olun. 

1. Visual Studio cloud Explorer'da kaynakları IOT cihazlarınızın listesini görmek için genişletin. 

2. Dağıtım almak istediğiniz IOT Edge cihaz adına sağ tıklayın. 

3. Seçin **dağıtım oluşturma**.

4. Dosya Gezgini'nde seçin **deployment.windows amd64** çözümünüzün config klasöründeki dosya. 

5. Cihazınızı altında listelenen dağıtılan modüller görmek için cloud explorer'ı yenileyin. 

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

Dağıtım bildirimini IoT Edge cihazınıza uyguladıktan sonra cihazdaki IoT Edge çalışma zamanı yeni dağıtım bilgilerini toplar ve yürütmeye başlar. Cihazda çalışan ve dağıtım bildiriminde bulunmayan modüller durdurulur. Cihazda eksik olan modüller başlatılır. 

IOT Edge araçları uzantısı IOT Hub'ınıza geldikçe iletilerini görüntülemek için kullanabilirsiniz. 

1. Visual Studio cloud explorer'ın IOT Edge Cihazınızı adını seçin. 

2. İçinde **eylemleri** listesinden **Başlat yerleşik olay uç nokta izleme**. 

3. IOT hub'da gelen iletileri görüntüleyin. Biz CSharpModule koda yapılan değişiklikleri iletileri göndermeden önce 25 derece makine sıcaklık ulaşana kadar bekleyin, çünkü bu ulaşması iletileri için biraz zaman alabilir. İleti türü de ekler **uyarı** sıcaklık eşiğe ulaşan tüm iletileri. 

   ![IOT hub'da gelen iletileri görüntüleme](./media/tutorial-csharp-module-windows/view-d2c-message.png)

## <a name="edit-the-module-twin"></a>Modül ikizi Düzenle

25 derecede sıcaklık eşiği ayarlamak için CSharpModule modül ikizi kullandık. Modül ikizi modülü kodunu güncelleştirmek zorunda kalmadan işlevlerini değiştirmek için kullanabilirsiniz.

1. Visual Studio'da açın **deployment.windows amd64.json** dosya. (Deployment.template dosyası değil. Dağıtım yapılandırma dosyası Çözüm Gezgini'nde bildirimi, seçin görmüyorsanız **tüm dosyaları göster** Gezgini araç çubuğundaki simgesi.)

2. CSharpModule ikizi bulun ve değiştirin **temperatureThreshold** parametresi için yeni bir sıcaklık 5 derece 10 derece son bildirilen sıcaklık daha yüksek. 

3. Kaydet **deployment.windows amd64.json** dosya.

4. Güncelleştirilmiş bir dağıtım bildirimi için Cihazınızı yeniden uygulamak için dağıtım adımları izleyin. 

5. CİHAZDAN buluta gelen iletileri izleyin. Yeni sıcaklık eşiği ulaşılana kadar Durdur iletileri görmeniz gerekir. 

## <a name="clean-up-resources"></a>Kaynakları temizleme 

Bir sonraki önerilen makaleye geçmeyi planlıyorsanız, oluşturduğunuz kaynaklarla yapılandırmaları tutabilir ve yeniden kullanabilirsiniz. Aynı IoT Edge cihazını test cihazı olarak kullanmaya devam edebilirsiniz. 

Aksi takdirde, yerel yapılandırmaları ve ücretleri önlemek için bu makalede kullanılan Azure kaynaklarını silebilirsiniz. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide IoT Edge cihazınız tarafından üretilen ham verileri filtreleme kodunu içeren bir IoT Edge modülü oluşturdunuz. Kendi modüllerinizi derlemek için hazır olduğunuzda, daha fazla bilgi edinebilirsiniz [kendi IOT Edge modülleri geliştirmek](module-development.md) veya nasıl [modülleri Visual Studio ile geliştirin](how-to-visual-studio-develop-module.md). Azure IOT Edge, uçta verilerini işlemek ve çözümlemek için Azure bulut Hizmetleri dağıtmayı nasıl yardımcı olabileceğini öğrenmek için sonraki öğreticiler açın devam edebilirsiniz.

> [!div class="nextstepaction"]
> [İşlevleri](tutorial-deploy-function.md)
> [Stream Analytics](tutorial-deploy-stream-analytics.md)
> [makine öğrenimi](tutorial-deploy-machine-learning.md)
> [özel görüntü işleme hizmeti](tutorial-deploy-custom-vision.md)
