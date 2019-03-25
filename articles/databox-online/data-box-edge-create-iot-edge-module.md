---
title: Azure veri kutusu Edge için C# IOT Edge Modülü | Microsoft Docs
description: Veri kutusu Ucunuzdaki dağıtılabilir bir C# IOT Edge modülü geliştirme konusunda bilgi edinin.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 03/19/2019
ms.author: alkohli
ms.openlocfilehash: 522dddde4994bb019e6547fcd18465b201f048d8
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58401723"
---
# <a name="develop-a-c-iot-edge-module-to-move-files-on-data-box-edge"></a>Geliştirme bir C# IOT Edge modülü, veri kutusu edge'de dosyaları taşıma

Bu makalede veri kutusu Edge Cihazınızı dağıtım için bir IOT Edge modülü oluşturma adımları. Azure Data Box Edge verileri işlemenizi ve ağ üzerinden Azure'a göndermenizi sağlayan bir depolama çözümüdür.

Verileri Azure'a taşıdık şekilde dönüştürmek için Azure IOT Edge modülleri, veri kutusu Edge ile kullanabilirsiniz. Bu makalede kullanılan modül bir bulut paylaşımı veri kutusu Edge Cihazınızda yerel paylaşımından bir dosyayı kopyalama mantığını uygular.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Modüllerinizi (Docker görüntüleri) yönetmek için bir kapsayıcı kayıt defteri oluşturun.
> * Veri kutusu Edge Cihazınızda dağıtmak için bir IOT Edge modülünü oluşturun.


## <a name="about-the-iot-edge-module"></a>IOT Edge modülü hakkında

Veri kutusu Edge cihazınıza dağıtabilir ve IOT Edge modüllerini çalıştırın. Edge, temelde, belirli bir görevi gerçekleştirmek için gibi bir CİHAZDAN bir ileti alma, bir iletiyi dönüştürmek veya bir IOT Hub'ına ileti göndermek Docker kapsayıcıları modüllerdir. Bu makalede, bir bulut paylaşımına veri kutusu Edge Cihazınızda yerel bir paylaşımdan dosyaları kopyalayan bir modül oluşturur.

1. Dosyalar, veri kutusu Edge Cihazınızda yerel paylaşıma yazılır.
2. Dosya olay Oluşturucu yerel paylaşıma yazılan her dosya için bir dosya olayı oluşturur. Bir dosya değiştirildiğinde dosya olaylar da oluşturulur. Dosya olayları, sonra IOT Edge Hub'ına (IOT Edge çalışma zamanı) gönderilir.
3. IOT Edge Özel Modül dosya için göreli bir yol da içeren bir dosya olay nesnesi oluşturmak için dosya olayını işler. Modül göreli dosya yolu kullanarak mutlak bir yol oluşturur ve dosyayı yerel paylaşımından bulut paylaşıma kopyalar. Modül, ardından dosyayı yerel paylaşımından siler.

![Veri kutusu Edge üzerinde Azure IOT Edge modülü nasıl çalışır?](./media/data-box-edge-create-iot-edge-module/how-module-works.png)

Dosyayı bulut paylaşımda olduktan sonra Azure depolama hesabınıza otomatik olarak karşıya.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdakilere sahip olduğunuzdan emin olun:

- Çalışan bir veri kutusu Edge cihazı.

    - Cihaz Ayrıca, ilişkili bir IOT hub'ı kaynak vardır.
    - Cihaz bilgi işlem rolü yapılandırılmış bir sınır vardır.
    Daha fazla bilgi için Git [işlem yapılandırma](data-box-edge-deploy-configure-compute.md#configure-compute) veri kutusu Ucunuzdaki için.

- Aşağıdaki geliştirme kaynaklarıdır:

    - [Visual Studio Code](https://code.visualstudio.com/).
    - [Visual Studio Code için C# (OmniSharp tarafından desteklenen) uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).
    - [Visual Studio Code için Azure IoT Edge uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge).
    - [.NET Core 2.1 SDK'sı](https://www.microsoft.com/net/download).
    - [Docker CE](https://store.docker.com/editions/community/docker-ce-desktop-windows). Yazılımı karşıdan yüklenip kurulacak bir hesap oluşturmanız gerekebilir.

## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma

Azure kapsayıcı kayıt defteri, Azure’da özel Docker kapsayıcısı görüntülerinizi depolayıp yönetebileceğiniz özel bir Docker kayıt defteridir. Sağlanan iki popüler Docker kayıt defteri hizmetler bulutta, Azure Container Registry ve Docker Hub ' dir. Bu makalede, kapsayıcı kayıt defteri kullanır.

1. [https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.
2. Seçin **kaynak Oluştur > kapsayıcıları > kapsayıcı kayıt defteri**. **Oluştur**’a tıklayın.
3. Sağlar:

   1. Benzersiz bir **kayıt defteri adı** Azure'un içinde 5-50 alfasayısal karakterler içeriyor.
   2. Seçin bir **abonelik**.
   3. Yeni oluşturduğunuz veya mevcut bir **kaynak grubu**.
   4. Bir **Konum** seçin. Veri kutusu Edge kaynakla ilişkilendirilmiş olduğundan bu konum aynı olmasını öneririz.
   5. **Yönetici kullanıcı** ayarını **Etkinleştir**'e getirin.
   6. SKU kümesine **temel**.

      ![Kapsayıcı kayıt defteri oluşturma](./media/data-box-edge-create-iot-edge-module/create-container-registry-1.png)
 
4. **Oluştur**’u seçin.
5. Kapsayıcı kayıt defteriniz oluşturulduktan sonra, bu kayıt defterine gidin ve **Erişim anahtarları**'nı seçin.

    ![Erişim anahtarı alma](./media/data-box-edge-create-iot-edge-module/get-access-keys-1.png)
 
6. **Oturum açma sunucusu**, **Kullanıcı adı** ve **Parola** değerlerini kopyalayın. Bu değerleri daha sonra Docker görüntüsünü kayıt defterinize yayımlama ve Azure IOT Edge çalışma zamanı için kayıt defteri kimlik bilgilerini eklemek için kullanırsınız.


## <a name="create-an-iot-edge-module-project"></a>IoT Edge modülü projesi oluşturma

Aşağıdaki adımlar üzerinde .NET Core 2.1 SDK dayalı bir IOT Edge modülü projesi oluşturur. Proje, Visual Studio Code ve Azure IOT Edge uzantısını kullanır.

### <a name="create-a-new-solution"></a>Yeni çözüm oluşturma

Kendi yazacağınız kodla özelleştirebileceğiniz bir C# çözüm şablonu oluşturun.

1. Visual Studio Code'da seçin **Görüntüle > komut paleti** VS Code komut paletini açın.
2. Komut Paleti'nde girin ve şu komutu çalıştırın **Azure: Oturum** ve Azure hesabınızda oturum açmak için yönergeleri izleyin. Oturumu önceden açtıysanız bu adımı atlayabilirsiniz.
3. Komut Paleti'nde girin ve şu komutu çalıştırın **Azure IOT Edge: Yeni bir IOT Edge çözüm**. Komut paletinde çözümünüzü oluşturmak için aşağıdaki bilgileri girin:

    1. Çözümü oluşturmak istediğiniz klasörü seçin.
    2. Çözümünüz için bir ad girin veya varsayılan **EdgeSolution** adını kabul edin.
    
        ![1 yeni çözüm oluşturma](./media/data-box-edge-create-iot-edge-module/create-new-solution-1.png)

    3. Modül şablonu olarak **C# Module** girişini seçin.
    4. Varsayılan modül adı atamak istediğiniz adı ile değiştirin, bu durumda, **FileCopyModule**.
    
        ![2 yeni çözüm oluşturma](./media/data-box-edge-create-iot-edge-module/create-new-solution-2.png)

    5. Görüntü deposu olarak ilk modülünüzde için önceki bölümde oluşturulan kapsayıcı kayıt defteri belirtin. **localhost:5000** yerine kopyaladığınız oturum açma sunucusu değerini yazın.

        Son dize şuna benzer `<Login server name>/<Module name>`. Bu örnekte, dizedir: `mycontreg2.azurecr.io/filecopymodule`.

        ![3 yeni çözüm oluşturma](./media/data-box-edge-create-iot-edge-module/create-new-solution-3.png)

4. Git **Dosya > Klasör Aç**.

    ![Yeni çözüm 4 oluşturma](./media/data-box-edge-create-iot-edge-module/create-new-solution-4.png)

5. Bulun ve üzerine **EdgeSolution** daha önce oluşturduğunuz klasör. VS Code penceresinin, IOT Edge çözüm çalışma alanı ile beş düzey bileşenleri yükler. Düzenlemeyeceği **.vscode** klasöründe **.gitignore** dosyası **.env** dosyasını ve **deployment.template.json** bu makaledeki.
    
    Değişiklik yalnızca modülleri klasöründe bileşendir. Bu klasör, modül olarak bir kapsayıcı görüntüsü oluşturmak için Docker dosya ve modülü için C# kodu içerir.

    ![5 yeni çözüm oluşturma](./media/data-box-edge-create-iot-edge-module/create-new-solution-5.png)

### <a name="update-the-module-with-custom-code"></a>Modülü özel kodla güncelleştirme

1. VS Code Gezgininde açın **modülleri > FileCopyModule > Program.cs**.
2. Üst kısmındaki **FileCopyModule ad alanı**, aşağıdaki ekleyin, daha sonra kullanılan türler için using deyimleri. **Microsoft.Azure.Devices.Client.Transport.Mqtt** IOT Edge Hub'ına ileti göndermek için bir protokoldür.

    ```
    using Microsoft.Azure.Devices.Client.Transport.Mqtt;
    using Newtonsoft.Json;
    ```
3. Ekleme **InputFolderPath** ve **OutputFolderPath** Program sınıfına değişken.

    ```
    class Program
        {
            static int counter;
            private const string InputFolderPath = "/home/input";
            private const string OutputFolderPath = "/home/output";
    ```

4. Ekleme **MessageBody** Program sınıfına sınıfı. Bu sınıflar gelen iletilerin gövdesi için beklenen şemayı tanımlar.

    ```
    /// <summary>
    /// The MessageBody class defines the expected schema for the body of incoming messages. 
    /// </summary>
    private class FileEvent
    {
        public string ChangeType { get; set; }

        public string ShareRelativeFilePath { get; set; }

        public string ShareName { get; set; }
    }
    ```

5. **Init** yöntemindeki kod, bir **ModuleClient** nesnesi oluşturur ve yapılandırır. Bu nesne, ileti göndermek ve almak için MQTT protokolünü kullanarak yerel Azure IOT Edge çalışma zamanı için bağlanmak bir modül sağlar. Init yönteminde kullanılan bağlantı dizesi modüle IOT Edge çalışma zamanı tarafından sağlanır. Kod bir IOT Edge hub'ı iletileri alacak şekilde bir FileCopy geri çağırma kaydeder **input1** uç noktası.

    ```
    /// <summary>
    /// Initializes the ModuleClient and sets up the callback to receive
    /// messages containing file event information
    /// </summary>
    static async Task Init()
    {
        MqttTransportSettings mqttSetting = new MqttTransportSettings(TransportType.Mqtt_Tcp_Only);
        ITransportSettings[] settings = { mqttSetting };

        // Open a connection to the IoT Edge runtime
        ModuleClient ioTHubModuleClient = await ModuleClient.CreateFromEnvironmentAsync(settings);
        await ioTHubModuleClient.OpenAsync();
        Console.WriteLine("IoT Hub module client initialized.");

        // Register callback to be called when a message is received by the module
        await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FileCopy, ioTHubModuleClient);
    }
    ```

6. Ekleme kodunu **FileCopy**.

    ```
        /// <summary>
        /// This method is called whenever the module is sent a message from the IoT Edge Hub. 
        /// This method deserializes the file event, extracts the corresponding relative file path, and creates the absolute input file path using the relative file path and the InputFolderPath.
        /// This method also forms the absolute output file path using the relative file path and the OutputFolderPath. It then copies the input file to output file and deletes the input file after the copy is complete.
        /// </summary>
        static async Task<MessageResponse> FileCopy(Message message, object userContext)
        {
            int counterValue = Interlocked.Increment(ref counter);

            try
            {
                byte[] messageBytes = message.GetBytes();
                string messageString = Encoding.UTF8.GetString(messageBytes);
                Console.WriteLine($"Received message: {counterValue}, Body: [{messageString}]");

                if (!string.IsNullOrEmpty(messageString))
                {
                    var fileEvent = JsonConvert.DeserializeObject<FileEvent>(messageString);

                    string relativeFileName = fileEvent.ShareRelativeFilePath.Replace("\\", "/");
                    string inputFilePath = InputFolderPath + relativeFileName;
                    string outputFilePath = OutputFolderPath + relativeFileName;

                    if (File.Exists(inputFilePath))                
                    {
                        Console.WriteLine($"Moving input file: {inputFilePath} to output file: {outputFilePath}");
                        var outputDir = Path.GetDirectoryName(outputFilePath);
                        if (!Directory.Exists(outputDir))
                        {
                            Directory.CreateDirectory(outputDir);
                        }

                        File.Copy(inputFilePath, outputFilePath, true);
                        Console.WriteLine($"Copied input file: {inputFilePath} to output file: {outputFilePath}");
                        File.Delete(inputFilePath);
                        Console.WriteLine($"Deleted input file: {inputFilePath}");
                    } 
                    else
                    {
                        Console.WriteLine($"Skipping this event as input file doesn't exist: {inputFilePath}");   
                    }
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine("Caught exception: {0}", ex.Message);
                Console.WriteLine(ex.StackTrace);
            }

            Console.WriteLine($"Processed event.");
            return MessageResponse.Completed;
        }
    ```

7. Bu dosyayı kaydedin.

## <a name="build-your-iot-edge-solution"></a>IoT Edge çözümünüzü derleyin

Önceki bölümde IOT Edge çözümünü oluşturan ve kod dosyalarını yerel paylaşımından bulut paylaşımı kopyalamak FileCopyModule eklenir. Şimdi çözümü kapsayıcı görüntüsü olarak derlemeniz ve kapsayıcı kayıt defterine göndermeniz gerekiyor.

1. VSCode içinde terminale gidin > Yeni yeni bir Visual Studio Code tümleşik Terminalini açmak için Terminal.
2. Tümleşik terminalde aşağıdaki komutu girerek Docker'da oturum açın.

    `docker login <ACR login server> -u <ACR username>`

    Kapsayıcı kayıt defterinizden kopyaladığınız kullanıcı adını ve oturum açma sunucusu kullanın. 

    ![Oluşturun ve IOT Edge çözüm gönderin](./media/data-box-edge-create-iot-edge-module/build-iot-edge-solution-1.png)

2. Parola istendiğinde parolayı girin. Oturum açma sunucusu, kullanıcı adı ve paroladan değerlerini de alabilirsiniz **erişim anahtarlarını** Azure portalındaki kapsayıcı kayıt defterinizde.
 
3. Kimlik bilgileri sağlanan sonra Azure kapsayıcı kayıt defterinizde modülü görüntünüzü gönderebilirsiniz. VS Code Gezgininde sağ **module.json** seçin ve dosya **derleme ve anında iletme IOT Edge çözüm**.

    ![Oluşturun ve IOT Edge çözüm gönderin](./media/data-box-edge-create-iot-edge-module/build-iot-edge-solution-2.png)
 
    Çözümünüzü derlemek için Visual Studio Code size, tümleşik terminalde iki komutu çalıştırır: docker derleme ve docker gönderme. Bu iki komut kodunuzu derler, CSharpModule.dll ile kapsayıcı oluşturur ve ardından kodu, çözümü başlatırken belirttiğiniz kapsayıcı kayıt defterine gönderir.

    Modül platformu seçin istenir. Seçin *amd64* Linux için karşılık gelen.

    ![Platform seçin](./media/data-box-edge-create-iot-edge-module/select-platform.png)

    > [!IMPORTANT] 
    > Yalnızca Linux modülleri desteklenir.

    Aşağıdaki uyarıyı gözardı edebileceğiniz görebilirsiniz:

    *Program.cs(77,44): CS1998 Uyarı: Bu zaman uyumsuz yöntem 'await' işleçleri olmadığından ve zaman uyumlu çalışacak. 'Await' işleci, API çağrıları engelleyici olmayan await kullanmayı göz önünde bulundurun veya 'Task.Run(...) await' arka plan iş parçacığında CPU bağımlı iş yapma.*

4. VS Code tümleşik terminalinde etiketle tam kapsayıcı görüntü adresini görebilirsiniz. Görüntü adresi biçiminde module.json dosyasındaki bilgileri oluşturulur `<repository>:<version>-<platform>`. Bu makalede gibi görünmelidir `mycontreg2.azurecr.io/filecopymodule:0.0.1-amd64`.

## <a name="next-steps"></a>Sonraki adımlar

Dağıtma ve bu modülü veri kutusu Edge üzerinde çalıştırmak için adımları görmek [bir modül ekleyecek](data-box-edge-deploy-configure-compute.md#add-a-module).
