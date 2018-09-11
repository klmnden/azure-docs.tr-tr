---
title: Azure IOT Edge sürekli tümleştirme ve sürekli dağıtım | Microsoft Docs
description: Sürekli tümleştirme ve sürekli dağıtım Azure IOT Edge için genel bakış
author: shizn
manager: ''
ms.author: xshi
ms.date: 06/27/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 5099ca70503ba2ed4ae8f4969a9199816c4986fb
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44302580"
---
# <a name="continuous-integration-and-continuous-deployment-to-azure-iot-edge"></a>Sürekli tümleştirme ve sürekli dağıtım için Azure IOT Edge

Bu makalede, derleme, test etme ve uygulamaları, Azure IOT Edge için hızlı ve verimli bir şekilde dağıtmak için sürekli tümleştirme ve sürekli dağıtım Özelliği Azure DevOps Hizmetleri ve Microsoft Team Foundation Server (TFS) nasıl kullanabileceğinizi gösterir. 

Bu makalede, öğreneceksiniz nasıl yapılır:
* Oluşturma ve bir örnek IOT Edge çözümü içeren birim testleri denetleyin.
* Azure DevOps için Azure IOT Edge uzantısını yükleyin.
* Sürekli Tümleştirme (CI) çözümü oluşturun ve birim testlerini çalıştırmak için yapılandırın.
* Çözümü dağıtmak ve yanıtları görüntülemek için sürekli dağıtım (CD) yapılandırın.

Bu makaledeki adımlarda tamamlanması 30 dakika sürer.

![CI ve CD](./media/how-to-ci-cd/cd.png)

## <a name="create-a-sample-azure-iot-edge-solution-using-visual-studio-code"></a>Visual Studio Code kullanarak örnek bir Azure IOT Edge çözüm oluşturma

Bu bölümde, yapı işleminin bir parçası olarak yürütebilen çözüm birim testleri içeren örnek bir IOT Edge oluşturacaksınız. Bu bölümdeki yönergeleri izlemeden önce bölümündeki adımları tamamlamanız [bir IOT Edge çözümü Visual Studio code'da birden çok modül ile geliştirme](tutorial-multiple-modules-in-vscode.md).

1. VS Code komut paleti yazın ve şu komutu çalıştırın **Edge: IOT Edge yeni çözüm**. Çalışma alanı klasörünüzde'ı seçin, çözüm adı sağlayın (varsayılan ad **EdgeSolution**) ve bir C# modülü oluşturun (**FilterModule**) Bu çözümdeki ilk kullanıcı modülü olarak. İlk modülünüz için Docker görüntü deposunu da belirtmeniz gerekir. Varsayılan görüntü deposu yerel bir Docker kayıt defteri bağlıdır (`localhost:5000/filtermodule`). Azure Container Registry'ye değiştirmeniz gerekir (`<your container registry address>/filtermodule`) veya daha fazla sürekli tümleştirme için Docker hub'ı.

    ![ACR Kurulumu](./media/how-to-ci-cd/acr.png)

2. VS Code penceresinin, IOT Edge çözüm çalışma alanı yükler. İsteğe bağlı olarak yazın ve çalıştırabilirsiniz **Edge: IOT Edge Modülü Ekle** daha fazla modül eklemek için. Var olan bir `modules` klasöründe bir `.vscode` klasörü ve dağıtım bildirim şablonu dosyası kök klasöründe. Tüm kullanıcı modülü kodları alt klasörü altında olacak `modules`. `deployment.template.json` Dağıtım bildirimi şablonudur. Bazı parametreler bu dosyadaki gelen ayrıştırılacak `module.json`, her modül klasöründe bulunmaktadır.

3. Artık IOT Edge çözüm örneğinizi hazırdır. Varsayılan C# modülü kanal iletisi modül olarak görev yapar. İçinde `deployment.template.json`, bu çözümü içeren iki modül görürsünüz. İleti kaynaklandığı `tempSensor` modülü ve aracılığıyla doğrudan yöneltilen `FilterModule`, ardından IOT hub'ına gönderilen. Tüm değiştirin **Program.cs** dosya içeriği. Bu kod parçacığı hakkında daha fazla bilgi için başvurabilirsiniz [bir IOT Edge C# modülü projesi oluşturma](https://docs.microsoft.com/azure/iot-edge/tutorial-csharp-module#create-an-iot-edge-module-project).

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
                Init().Wait();

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
            /// Initializes the ModuleClient and sets up the callback to receive
            /// messages containing temperature information
            /// </summary>
            static async Task Init()
            {
                MqttTransportSettings mqttSetting = new MqttTransportSettings(TransportType.Mqtt_Tcp_Only);
                ITransportSettings[] settings = { mqttSetting };

                // Open a connection to the Edge runtime
                ModuleClient ioTHubModuleClient = await ModuleClient.CreateFromEnvironmentAsync(settings);
                await ioTHubModuleClient.OpenAsync();
                Console.WriteLine("IoT Hub module client initialized.");

                // Register callback to be called when a message is received by the module
                await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FilterMessage, ioTHubModuleClient);
            }

            /// <summary>
            /// This method is called whenever the module is sent a message from the EdgeHub. 
            /// It just pipe the messages without any change.
            /// It prints all the incoming messages.
            /// </summary>
            static async Task<MessageResponse> FilterMessage(Message message, object userContext)
            {
                int counterValue = Interlocked.Increment(ref counter);

                var moduleClient = userContext as ModuleClient;
                if (moduleClient == null)
                {
                    throw new InvalidOperationException("UserContext doesn't contain " + "expected values");
                }

                byte[] messageBytes = message.GetBytes();
                string messageString = Encoding.UTF8.GetString(messageBytes);
                Console.WriteLine($"Received message: {counterValue}, Body: [{messageString}]");

                var filteredMessage = filter(message);

                if (filteredMessage != null && !string.IsNullOrEmpty(messageString))
                {
                    var pipeMessage = new Message(messageBytes);
                    foreach (var prop in message.Properties)
                    {
                        pipeMessage.Properties.Add(prop.Key, prop.Value);
                    }
                    await moduleClient.SendEventAsync("output1", pipeMessage);
                    Console.WriteLine("Received message sent");
                }
                return MessageResponse.Completed;
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
        }
    }
    ```

4. Oluşturma bir.Net Core birim testi projesi. VS Code dosya Gezgini'nde, yeni bir klasör oluşturun **tests\FilterModuleTest** çalışma alanınızdaki. VS Code tümleşik Terminalini sonra (**Ctrl + '**), komutları bir xunit test projesi oluşturun ve referansı eklemek için aşağıdaki çalıştırma **FilterModule** proje.

    ```cmd
    cd tests\FilterModuleTest
    dotnet new xunit
    dotnet add reference ../../modules/FilterModule/FilterModule.csproj
    ```

    ![Klasör yapısı](./media/how-to-ci-cd/add-test-project.png)

5. İçinde **FilterModuleTest** klasör, dosya adını güncelleştirme **UnitTest1.cs** için **FilterModuleTest.cs**. Seçin ve açın **FilterModuleTest.cs**, FilterModule projesine karşı birim testleri içeren kod parçacığı aşağıda tüm kod ile değiştirin.

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

6. Tümleşik terminalde aşağıdaki komutları yerel olarak birim testlerini çalıştırmak için girebilirsiniz. 
    ```cmd
    dotnet test
    ```

    ![Birim testi](./media/how-to-ci-cd/unit-test.png)

7. Bu projeler kaydettikten sonra Azure DevOps veya TFS depoya denetleyin.
    

> [!NOTE]
> Azure depoları kullanma hakkında daha fazla bilgi için bkz. [Visual Studio ve Azure depoları ile kodunuzu paylaşmaya](https://docs.microsoft.com/azure/devops/repos/git/share-your-code-in-git-vs?view=vsts).


## <a name="configure-continuous-integration"></a>Sürekli tümleştirmeyi yapılandırın
Bu bölümde, IOT Edge çözüm örnek değişiklikleri iade ettiğinizde otomatik olarak çalışacak şekilde yapılandırılmış bir derleme işlem hattı oluşturur ve içerdiği birim testlerini otomatik olarak çalıştırır.

1. Azure DevOps kuruluşunuz oturum (**https://**_hesabınızı_**. visualstudio.com**) ve örnek uygulamada nereye iade projeyi açın.

    ![Kod iade etme](./media/how-to-ci-cd/init-project.png)

1. Ziyaret [Azure DevOps için Azure IOT Edge](https://marketplace.visualstudio.com/items?itemName=vsc-iot.iot-edge-build-deploy) Azure DevOps Market'te. Tıklayın **Ücretsiz edinin** ve Azure DevOps kuruluşunuz veya indirmek için TFS için bu uzantıyı yüklemek için sihirbazı izleyin.

    ![Uzantıyı yükleme](./media/how-to-ci-cd/install-extension.png)

1. Azure DevOps açın **derleme &amp; yayın** hub hem de **oluşturur** sekmesini, **+ yeni işlem hattı**. Ya da derleme işlem hatlarını zaten varsa, seçin **+ yeni** düğmesi. 

    ![Yeni derleme](./media/how-to-ci-cd/add-new-build.png)

1. İstenirse, seçin **Azure DevOps Git** kaynak türü; ardından Proje, depo ve dal kodunuzu nerede'ı seçin. Seçin **devam**.

    ![Azure DevOps gıt'i seçin](./media/how-to-ci-cd/select-vsts-git.png)

1. İçinde **bir şablon seçin** penceresinde seçin **boş bir işlemle başlangıç**.

    ![Boş Başlat](./media/how-to-ci-cd/start-with-empty.png)

1. Tıklayın **+** sağ alt tarafında **1. Aşama** aşamaya bir görev eklemek için. Seçin ve ardından arama **.Net Core**, tıklatıp **Ekle** aşaması için bu görev eklemek için.

    ![DotNet testi](./media/how-to-ci-cd/add-dot-net-core.png)

1. Güncelleştirme **görünen ad** için **dotnet testi**hem de **komut** açılan listesinden **test**. Aşağıdaki yolu Ekle **projelerin yolu**.

    ```
    tests/FilterModuleTest/*.csproj
    ```

    ![DotNet testi Yapılandır](./media/how-to-ci-cd/dotnet-test.png)

1. Tıklayın **+** sağ alt tarafında **1. Aşama** aşamaya bir görev eklemek için. Seçin ve ardından arama **Azure IOT Edge**, tıklatıp **Ekle** düğmesi **iki kez** aşaması için bu görevleri eklemek için.

    ![IoT Edge](./media/how-to-ci-cd/add-azure-iot-edge.png)

1. İlk Azure IOT Edge görevi güncelleştirme **görünen ad** için **modülü oluşturmak ve anında iletme**ve **eylem** açılan listesinden **oluşturun vegönderin**. İçinde **Module.json dosya** metin kutusuna aşağıdaki yolu ekleyin. Ardından **kapsayıcı kayıt defteri türü**, yapılandırma ve kodunuzda aynı kayıt defteri seçin emin olun. Bu görev oluşturacak ve Çözümdeki tüm modüllerin gönderin ve belirttiğiniz kapsayıcı kayıt defterine yayımlama. Farklı kayıt defterlerinde modüllerinizi gönderilir, birden çok olabilir **modülü oluşturmak ve anında iletme** görevleri.

    ```
    **/module.json
    ```

    ![Modül derleme ve gönderme](./media/how-to-ci-cd/module-build-push.png)

1. İkinci Azure IOT Edge görevde güncelleştirme **görünen ad** için **IOT Edge cihazına dağıtma**hem de **eylem** açılan listesinden **IOT Edge dağıtma cihaz**. Azure aboneliğinizi seçin ve IOT Hub adınızı girin. Bir IOT Edge dağıtımı kimliği ile dağıtım önceliğini belirtebilirsiniz. Tek veya birden çok cihaza dağıtmayı seçebilirsiniz. Birden çok aygıta dağıtıyorsanız, cihaz hedef koşulu belirtmek gerekir. Örneğin, cihaz etiketleri koşul olarak kullanmak istiyorsanız, ilgili cihazlarınızı etiketleri dağıtımdan önce güncelleştirmeniz gerekiyor. 

    ![Uca dağıtma](./media/how-to-ci-cd/deploy-to-edge.png)

1. Tıklayın **işlem** ve emin olun, **aracı kuyruğu** olduğu **barındırılan Linux Önizleme**.

    ![Yapılandırma](./media/how-to-ci-cd/configure-env.png)

1. Açık **Tetikleyicileri** sekmesini ve açma **sürekli tümleştirme** tetikleyici. Kodunuzu içeren dal dahil olduğundan emin olun.

    ![Tetikleyici](./media/how-to-ci-cd/configure-trigger.png)

1. Yeni derleme işlem hattı kaydedin ve yeni bir yapıyı kuyruğa alın. Tıklayın **Kaydet ve kuyruğa** düğmesi.

1. Yapı bağlantısını görüntülenen ileti çubuğu seçin. Veya en son sıraya alınan derleme işi görmek için işlem hattı oluşturma gidin.

    ![Oluşturma](./media/how-to-ci-cd/build-def.png)

1. Yapı tamamlandıktan sonra her görev ve canlı günlük dosyasında sonuçları özeti görürsünüz. 
    
    ![Tamamlama](./media/how-to-ci-cd/complete.png)

1. VS Code için geri dönün ve IOT Hub cihaz Gezgini denetleyin. Sınır cihazı modülü ile çalışan başlamalıdır (emin olun, kayıt defteri kimlik bilgilerini Edge çalışma zamanı'na eklediğiniz).

    ![Edge çalıştırma](./media/how-to-ci-cd/edge-running.png)

## <a name="continuous-deployment-to-iot-edge-devices"></a>IOT Edge cihazları için sürekli dağıtım

Sürekli dağıtımı etkinleştirme için temel CI işler doğru IOT Edge cihazları ile ayarlamak etkinleştirilmesi gerekir **Tetikleyicileri** projenizdeki dallarınız için. Klasik bir DevOps uygulamada iki ana dalı bir proje içerir. Ana daldaki kodu kararlı bir sürüm olmalıdır ve en son kod değişikliklerini geliştirme dalı içerir. Takım her geliştiricinin geliştirme dalı HIS çatal oluşturun veya anlamına gelen tüm işlemeler kodunu güncelleştirme başlatılıyor özelliği olduğunda kendi özellik dalı dallar geliştirme dalı devre dışı. Ve CI sistemi gönderilen her işleme test edilmelidir. Tam olarak kodu yerel olarak test sonra özellik geliştirme dalına bir çekme isteği aracılığıyla dalıyla. Geliştirici dal kodu CI sistemi sınanırken, ana dala bir çekme isteği aracılığıyla birleştirilebilir.

Bu nedenle, IOT Edge cihazlarına dağıtım yapılırken üç ana ortamları vardır.
- Özellik dalı üzerinde geliştirme makinenizde sanal IOT Edge cihazı kullanın ya da fiziksel bir IOT Edge cihazına dağıtma.
- Dal üzerinde geliştirmek, fiziksel bir IOT Edge cihazına dağıtmanız gerekir.
- Ana dalda, hedef IOT Edge cihazları üretim cihazları olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

* IOT Edge dağıtımı anlamak [IOT Edge dağıtımlarını anlama tek tek cihazlarda veya uygun ölçekte](module-deployment-monitoring.md)
* Oluşturmak, güncelleştirmek veya bir dağıtımda silmek için adımlarında yol [dağıtma ve izleme uygun ölçekte IOT Edge modülleri](how-to-deploy-monitor.md).
