---
title: Azure IOT kenar sürekli tümleştirme ve sürekli dağıtımı | Microsoft Docs
description: Azure IOT kenar için sürekli dağıtım ve sürekli tümleştirme genel bakış
author: shizn
manager: ''
ms.author: xshi
ms.date: 04/30/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: a8b58eae9aa08d8f6539370fa6e78a7a4813c18f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34631029"
---
# <a name="continuous-integration-and-continuous-deployment-to-azure-iot-edge---preview"></a>Sürekli tümleştirme ve sürekli dağıtımı Azure IOT ucunu - Önizleme
Bu öğretici sürekli tümleştirme nasıl kullanabileceğinizi gösterir ve sürekli dağıtımı özellikleri geliştirmek için Visual Studio Team Services (VSTS) ve Microsoft Team Foundation Server (TFS) için test ve uygulamaları hızlı ve verimli şekilde dağıtmak için Azure IOT kenar. 

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Oluşturun ve bir örnek IOT kenar çözüm içeren birim testleri denetleyin.
> * Azure IOT kenar uzantı için VSTS yükleyin.
> * Sürekli Tümleştirme (CI) çözümü oluşturmak ve birim testleri çalıştırmak için yapılandırın.
> * Çözümü dağıtmak ve yanıtları görüntülemek için sürekli dağıtım (CD) yapılandırın.

Bu öğreticiyi tamamlamak için 30 dakika sürer.

![CI ve CD](./media/how-to-ci-cd/cd.png)

## <a name="create-a-sample-azure-iot-edge-solution-using-visual-studio-code"></a>Visual Studio Code kullanarak örnek bir Azure IOT kenar çözüm oluşturma

Bu bölümde, oluşturma işleminin bir parçası olarak çalışabilecek çözüm birim testleri içeren bir örnek IOT kenar oluşturur. Bu bölümde yer alan yönergeleri izlemeden önce bölümündeki adımları tamamlamanız [IOT kenar çözümünü birden fazla modülü Visual Studio Code ile geliştirme](tutorial-multiple-modules-in-vscode.md).

1. VS Code komutu palette yazın ve şu komutu çalıştırın **kenar: yeni IOT uç çözümünün**. Çalışma klasörü seçin, çözüm adı belirtin (varsayılan ad **EdgeSolution**) ve bir C# modül oluşturun (**FilterModule**) Bu çözümdeki ilk kullanıcı modülü olarak. Ayrıca, ilk modülü için Docker görüntü deposuna belirtmeniz gerekir. Varsayılan görüntü deposu yerel bir Docker kayıt defteri tabanlı (`localhost:5000/filtermodule`). Azure kapsayıcı kayıt defterine değiştirmeniz gerekir (`<your container registry address>/filtermodule`) veya daha fazla sürekli tümleştirme için Docker Hub.

    ![ACR Kurulumu](./media/how-to-ci-cd/acr.png)

2. VS Code penceresini IOT kenar çözüm çalışma alanınızı yükler. İsteğe bağlı olarak yazdığınızda veya çalıştırmak **kenar: eklemek IOT kenar Modülü** daha fazla modül eklemek için. Var olan bir `modules` klasörü, bir `.vscode` klasörü ve bir dağıtım bildirim şablon dosyası kök klasöründe. Tüm kullanıcı modülü kodları alt klasörü altında olacak `modules`. `deployment.template.json` Dağıtım bildirim şablonudur. Bazı parametrelerin bu dosyadaki gelen ayrıştırılır `module.json`, her modül klasöründe bulunmaktadır.

3. Artık IOT uç çözümünün örneğinizi hazırdır. Varsayılan C# modül kanal iletisi modül olarak davranır. İçinde `deployment.template.json`, bu çözüm içeren iki modülleri görürsünüz. İleti kaynaklandığı `tempSensor` modül ve aracılığıyla doğrudan yöneltilen `FilterModule`, IOT hub'ına gönderilen. Tüm Değiştir **Program.cs** altındaki içerik dosya ile. Bu kod parçacığını hakkında daha fazla bilgi için başvurabilirsiniz [bir IOT kenar C# modülü projesi oluşturmak](https://docs.microsoft.com/azure/iot-edge/tutorial-csharp-module#create-an-iot-edge-module-project).

    ```csharp
    namespace FilterModule
    {
        using System;
        using System.IO;
        using System.Runtime.InteropServices;
        using System.Runtime.Loader;
        using System.Security.Cryptography.X509Certificates;
        using System.Text;
        using System.Threading;
        using System.Threading.Tasks;
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Client.Transport.Mqtt;
        using System.Collections.Generic;     // for KeyValuePair<>
        using Microsoft.Azure.Devices.Shared; // for TwinCollection
        using Newtonsoft.Json;                // for JsonConvert

        public class MessageBody
        {
            public Machine machine { get; set; }
            public Ambient ambient { get; set; }
            public string timeCreated { get; set; }
        }
        public class Machine
        {
            public double temperature { get; set; }
            public double pressure { get; set; }
        }
        public class Ambient
        {
            public double temperature { get; set; }
            public int humidity { get; set; }
        }

        public class Program
        {
            static int counter;
            static int temperatureThreshold { get; set; } = 25;

            static void Main(string[] args)
            {
                // The Edge runtime gives us the connection string we need -- it is injected as an environment variable
                string connectionString = Environment.GetEnvironmentVariable("EdgeHubConnectionString");

                // Cert verification is not yet fully functional when using Windows OS for the container
                bool bypassCertVerification = RuntimeInformation.IsOSPlatform(OSPlatform.Windows);
                if (!bypassCertVerification) InstallCert();
                Init(connectionString, bypassCertVerification).Wait();

                // Wait until the app unloads or is cancelled
                var cts = new CancellationTokenSource();
                AssemblyLoadContext.Default.Unloading += (ctx) => cts.Cancel();
                Console.CancelKeyPress += (sender, cpe) => cts.Cancel();
                WhenCancelled(cts.Token).Wait();
            }

            /// <summary>
            /// Handles cleanup operations when app is cancelled or unloads
            /// </summary>
            public static Task WhenCancelled(CancellationToken cancellationToken)
            {
                var tcs = new TaskCompletionSource<bool>();
                cancellationToken.Register(s => ((TaskCompletionSource<bool>)s).SetResult(true), tcs);
                return tcs.Task;
            }

            /// <summary>
            /// Add certificate in local cert store for use by client for secure connection to IoT Edge runtime
            /// </summary>
            static void InstallCert()
            {
                string certPath = Environment.GetEnvironmentVariable("EdgeModuleCACertificateFile");
                if (string.IsNullOrWhiteSpace(certPath))
                {
                    // We cannot proceed further without a proper cert file
                    Console.WriteLine($"Missing path to certificate collection file: {certPath}");
                    throw new InvalidOperationException("Missing path to certificate file.");
                }
                else if (!File.Exists(certPath))
                {
                    // We cannot proceed further without a proper cert file
                    Console.WriteLine($"Missing path to certificate collection file: {certPath}");
                    throw new InvalidOperationException("Missing certificate file.");
                }
                X509Store store = new X509Store(StoreName.Root, StoreLocation.CurrentUser);
                store.Open(OpenFlags.ReadWrite);
                store.Add(new X509Certificate2(X509Certificate2.CreateFromCertFile(certPath)));
                Console.WriteLine("Added Cert: " + certPath);
                store.Close();
            }
            /// <summary>
            /// Initializes the DeviceClient and sets up the callback to receive
            /// messages containing temperature information
            /// </summary>
            static async Task Init(string connectionString, bool bypassCertVerification = false)
            {
                Console.WriteLine("Connection String {0}", connectionString);

                MqttTransportSettings mqttSetting = new MqttTransportSettings(TransportType.Mqtt_Tcp_Only);
                // During dev you might want to bypass the cert verification. It is highly recommended to verify certs systematically in production
                if (bypassCertVerification)
                {
                    mqttSetting.RemoteCertificateValidationCallback = (sender, certificate, chain, sslPolicyErrors) => true;
                }
                ITransportSettings[] settings = { mqttSetting };

                // Open a connection to the Edge runtime
                DeviceClient ioTHubModuleClient = DeviceClient.CreateFromConnectionString(connectionString, settings);
                await ioTHubModuleClient.OpenAsync();
                Console.WriteLine("IoT Hub module client initialized.");

                // Register callback to be called when a message is received by the module
                // await ioTHubModuleClient.SetImputMessageHandlerAsync("input1", PipeMessage, iotHubModuleClient);

                // Read TemperatureThreshold from Module Twin Desired Properties
                var moduleTwin = await ioTHubModuleClient.GetTwinAsync();
                var moduleTwinCollection = moduleTwin.Properties.Desired;
                try {
                    temperatureThreshold = moduleTwinCollection["TemperatureThreshold"];
                } catch(ArgumentOutOfRangeException) {
                    Console.WriteLine("Proerty TemperatureThreshold not exist");
                }

                // Attach callback for Twin desired properties updates
                await ioTHubModuleClient.SetDesiredPropertyUpdateCallbackAsync(onDesiredPropertiesUpdate, null);

                // Register callback to be called when a message is received by the module
                await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FilterMessages, ioTHubModuleClient);
            }

            static Task onDesiredPropertiesUpdate(TwinCollection desiredProperties, object userContext)
            {
                try
                {
                    Console.WriteLine("Desired property change:");
                    Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));

                    if (desiredProperties["TemperatureThreshold"] != null)
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

            public static Message filter(Message message)
            {
                var counterValue = Interlocked.Increment(ref counter);

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
                    return filteredMessage;
                }
                return null;
            }

            static async Task<MessageResponse> FilterMessages(Message message, object userContext)
            {
                try
                {
                    DeviceClient deviceClient = (DeviceClient)userContext;

                    var filteredMessage = filter(message);
                    if (filteredMessage != null)
                    {
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
        }
    }
    ```

4. Bir .net oluşturma çekirdek birim testi projesi. VS Code dosya Gezgini'nde, yeni bir klasör oluşturun **tests\FilterModuleTest** çalışma alanınızdaki. VS Code tümleşik Terminal sonra (**Ctrl + '**), komutları bir xunit test projesi oluşturmak ve başvuru eklemek için aşağıdaki çalışma **FilterModule** projesi.

    ```cmd
    cd tests\FilterModuleTest
    dotnet new xunit
    dotnet add reference ../../modules/FilterModule/FilterModule.csproj
    ```

    ![Klasör yapısı](./media/how-to-ci-cd/add-test-project.png)

5. İçinde **FilterModuleTest** klasör, dosya adı, güncelleştirme **UnitTest1.cs** için **FilterModuleTest.cs**. Seçin ve açın **FilterModuleTest.cs**, FilterModule projesine karşı birim testleri içeren kod parçacığı aşağıda tüm kod ile değiştirin.

    ```csharp
    using Xunit;
    using FilterModule;
    using Newtonsoft.Json;
    using System;
    using System.IO;
    using System.Runtime.InteropServices;
    using System.Runtime.Loader;
    using System.Security.Cryptography.X509Certificates;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Client.Transport.Mqtt;

    namespace FilterModuleTest
    {
        public class FilterModuleTest
        {
            [Fact]
            public void filterLessThanThresholdTest()
            {
                var source = createMessage(25 - 1);
                var result = Program.filter(source);
                Assert.True(result == null);
            }

            [Fact]
            public void filterMoreThanThresholdAlertPropertyTest()
            {
                var source = createMessage(25 + 1);
                var result = Program.filter(source);
                Assert.True(result.Properties["MessageType"] == "Alert");
            }

            [Fact]
            public void filterMoreThanThresholdCopyPropertyTest()
            {
                var source = createMessage(25 + 1);
                source.Properties.Add("customTestKey", "customTestValue");
                var result = Program.filter(source);
                Assert.True(result.Properties["customTestKey"] == "customTestValue");
            }

            private Message createMessage(int temperature)
            {
                var messageBody = createMessageBody(temperature);
                var messageString = JsonConvert.SerializeObject(messageBody);
                var messageBytes = Encoding.UTF8.GetBytes(messageString);
                return new Message(messageBytes);
            }

            private MessageBody createMessageBody(int temperature)
            {
                var messageBody = new MessageBody
                {
                    machine = new Machine
                    {
                        temperature = temperature,
                        pressure = 0
                    },
                    ambient = new Ambient
                    {
                        temperature = 0,
                        humidity = 0
                    },
                    timeCreated = string.Format("{0:O}", DateTime.Now)
                };

                return messageBody;
            }
        }
    }
    ```

6. Tümleşik terminale birim testleri yerel olarak çalıştırmak için komutları girebilirsiniz. 
    ```cmd
    dotnet test
    ```

    ![Birim testi](./media/how-to-ci-cd/unit-test.png)

7. Bu projeleri kaydedin, sonra VSTS veya TFS depoya denetleyin.
    

> [!NOTE]
> Kod depoları VSTS kullanma hakkında daha fazla bilgi için bkz: [kodunuzu paylaşımı Visual Studio ve VSTS Git](https://docs.microsoft.com/vsts/git/share-your-code-in-git-vs?view=vsts).


## <a name="configure-continuous-integration"></a>Sürekli Tümleştirme yapılandırın
Bu bölümde IOT uç çözümünün örnek değişiklikleri iade ettiğinizde otomatik olarak çalışacak şekilde yapılandırılmış bir derleme tanımınız oluşturacak ve içerdiği birim testleri otomatik olarak çalıştırır.

1. VSTS hesabınızda oturum (**https://**_hesabınız_**. visualstudio.com**) ve örnek uygulama yeri işaretli projesini açın.

    ![Giriş kodu](./media/how-to-ci-cd/init-project.png)

1. Ziyaret [VSTS için Azure IOT kenar](https://marketplace.visualstudio.com/items?itemName=vsc-iot.iot-edge-build-deploy) VSTS Market'te. Tıklatın **ücretsiz almak** ve VSTS hesabı veya indirmek için TFS için bu uzantıyı yüklemek için sihirbazı izleyin.

    ![Uzantıyı yükleme](./media/how-to-ci-cd/install-extension.png)

1. VSTS içinde açmak **yapı &amp; sürüm** hub hem de **derlemeler** sekmesinde, seçin **+ yeni tanımı**. Veya derleme tanımı varsa, seçin **+ yeni** düğmesi. 

    ![Yeni bir derleme](./media/how-to-ci-cd/add-new-build.png)

1. İstenirse, seçin **VSTS Git** kaynak türünü; proje, depo ve kodunuzu bulunduğu dalı seçin. Seçin **devam**.

    ![VSTS git seçin](./media/how-to-ci-cd/select-vsts-git.png)

1. İçinde **bir şablon seçin** penceresinde, seçin **boş bir işlemle Başlat**.

    ![Boş Başlat](./media/how-to-ci-cd/start-with-empty.png)

1. Tıklatın **+** sağ tarafında **Aşama 1** aşamasına bir görev eklemek için. Ardından arayın ve seçin **.Net Core**, tıklatıp **Ekle** aşamasına bu görev eklemek için.

    ![DotNet test](./media/how-to-ci-cd/add-dot-net-core.png)

1. Güncelleştirme **görünen adı** için **dotnet test**hem de **komutu** açılır listesinden, **test**. Yolu altındaki eklemek **proje yoluna**.

    ```
    tests/FilterModuleTest/*.csproj
    ```

    ![DotNet test yapılandırın](./media/how-to-ci-cd/dotnet-test.png)

1. Tıklatın **+** sağ tarafında **Aşama 1** aşamasına bir görev eklemek için. Ardından arayın ve seçin **Azure IOT kenar**, tıklatıp **Ekle** düğmesini **iki kez** bu görevleri aşamasına eklemek için.

    ![IoT Edge](./media/how-to-ci-cd/add-azure-iot-edge.png)

1. İlk Azure IOT kenar görevde güncelleştirme **görünen adı** için **modülü oluşturmak ve anında iletme**hem de **eylem** açılır listesinden, **yapı veGönderme**. İçinde **Module.json dosya** metin kutusuna, aşağıdaki yolu ekleyin. Ardından **kapsayıcı kayıt defteri türü**, yapılandırmak ve aynı kayıt defteri kodunuzda seçin emin olun. Bu görev oluşturacak ve tüm modüllerin çözümde anında ve belirttiğiniz kapsayıcı kayıt defterine yayımlayın. 

    ```
    **/module.json
    ```

    ![Modül oluşturma ve gönderme](./media/how-to-ci-cd/module-build-push.png)

1. İkinci Azure IOT kenar görevde güncelleştirme **görünen adı** için **IOT sınır cihazı Dağıt**ve **eylem** açılır listesinden, **IOT ucunu dağıtma Aygıt**. Azure aboneliğinizi seçin ve IOT Hub adınızı girin. Bir IOT kenar dağıtım kimliği ve dağıtım önceliğini belirtebilirsiniz. Tek veya birden çok aygıta dağıtmayı seçebilirsiniz. Birden çok aygıta dağıtıyorsanız, cihaz hedef durumu belirtmeniz gerekir. Örneğin, aygıt etiketleri koşul olarak kullanmak istiyorsanız, karşılık gelen aygıtlarınızı etiketleri dağıtımdan önce güncelleştirmeniz gerekir. 

    ![Kenara dağıtma](./media/how-to-ci-cd/deploy-to-edge.png)

1. Tıklatın **işlem** ve emin olun, **Aracısı sırası** olan **barındırılan Linux Önizleme**.

    ![Yapılandırma](./media/how-to-ci-cd/configure-env.png)

1. Açık **Tetikleyicileri** sekmesinde ve Aç **sürekli tümleştirme** tetikleyici. Kodunuzu içeren şube dahil olduğundan emin olun.

    ![Tetikleyici](./media/how-to-ci-cd/configure-trigger.png)

1. Yeni derleme tanımı kaydedin ve yeni bir yapıyı sıraya al. Tıklatın **sıraya & Kaydet** düğmesi.

1. Yapı bağlantısını görüntülenen ileti çubuğunda seçin. Veya yapı son sıraya alınan derleme işi görmek için tanımı gidin.

    ![Oluşturma](./media/how-to-ci-cd/build-def.png)

1. Yapı tamamlandıktan sonra her görev ve canlı günlük dosyasında sonuçları özeti görürsünüz. 
    
    ![Tamamlama](./media/how-to-ci-cd/complete.png)

1. VS Code geri dönün ve IOT Hub cihaz explorer denetleyin. Sınır cihazı modülüyle çalıştıran başlamanız gerekir (emin olun kenar çalışma zamanı için kayıt defteri kimlik eklediğiniz).

    ![Çalışan sınır](./media/how-to-ci-cd/edge-running.png)

## <a name="continuous-deployment-to-iot-edge-devices"></a>IOT sınır cihazları için sürekli dağıtım

Sürekli dağıtımını etkinleştirmek için temel CI işler uygun IOT sınır cihazları ile ayarlamak etkinleştirilmesi gerekir **Tetikleyicileri** , Dallarınızı projenizdeki için. Klasik bir DevOps uygulamada bir proje iki ana dalları içerir. Ana dala kodu kararlı sürümü olmalıdır ve en son kod değişiklikleri geliştirme şube içerir. Her geliştiricinin ekipteki kaldırmalıdır geliştirme dala çatallaştırma veya tüm anlamına uygulayan kod güncelleştirme başlangıç özelliğini olduğunda kendi özellik şube geliştirme şube dallandırır. Ve her basılmış işleme CI sistem test edilmelidir. Tam kod yerel olarak test sonra bir çekme isteği aracılığıyla geliştirme şube özelliği şube birleştirilemez. Geliştirici şube kodu CI sistem sınandığında bir çekme isteği aracılığıyla ana dala birleştirilebilir.

Bu nedenle, IOT sınır cihazları dağıtırken, üç ana ortamları vardır.
- Özellik dalda, geliştirme makinenizde benzetimli IOT sınır cihazı kullanın ya da fiziksel bir IOT sınır cihazı dağıtma.
- Üzerinde şube geliştirmek, fiziksel bir IOT sınır cihazı dağıtmanız gerekir.
- Ana dala üzerinde hedef IOT sınır cihazları üretim aygıtları olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğretici, sürekli tümleştirme ve sürekli dağıtım özellikleri VSTS veya TFS nasıl kullanabileceğinizi gösterir. 

* IOT kenar dağıtımda anlamak [anlamak IOT kenar dağıtımları tek cihazlar için veya ölçekte](module-deployment-monitoring.md)
* Oluştur, Güncelleştir veya bir dağıtımda silmek için izleyeceğiniz adımlarda size yol [dağıtma ve izleme IOT kenar modülleri ölçekte] [how-to-dağıtma-monitor.md].









