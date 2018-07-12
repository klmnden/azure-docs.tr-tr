---
title: Azure IoT Edge C# öğreticisi | Microsoft Docs
description: Bu öğreticide C# koduyla IoT Edge modülü oluşturma ve bir Edge cihazına dağıtma adımları gösterilmektedir
services: iot-edge
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/27/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 73b6397ecc97b9e289749aabddfdc4c6161375d4
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38667354"
---
# <a name="tutorial-develop-a-c-iot-edge-module-and-deploy-to-your-simulated-device"></a>Öğretici: C# IoT Edge modülü geliştirme ve simülasyon cihazınıza dağıtma

İş mantığınızı uygulayan kodu doğrudan IoT Edge cihazlarınıza dağıtmak için IoT Edge modüllerini kullanabilirsiniz. Bu öğreticide, algılayıcı verilerini filtreleyen bir IoT Edge modülü oluşturma ve dağıtma işlemlerinin adımları açıklanmaktadır. [Windows][lnk-tutorial1-win]'ta veya [Linux][lnk-tutorial1-lin]'ta sanal bir cihaza Azure IoT Edge dağıtma öğreticilerinde oluşturduğunuz sanal IoT Edge cihazınızı kullanacaksınız. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:    

> [!div class="checklist"]
> * Visual Studio Code kullanarak .NET Core 2.0'a dayalı bir IoT Edge modülü oluşturma
> * Visual Studio Code ve Docker kullanarak bir Docker görüntüsü oluşturma ve bunu kayıt defterinizde yayımlama 
> * Modüle IoT Edge cihazınıza dağıtma
> * Oluşturulan verileri görüntüleme


Bu öğreticide oluşturacağınız IoT Edge modülü, cihazınız tarafından oluşturulan sıcaklık verilerini filtreler. İletileri yalnızca sıcaklık belirtilen bir eşiğin üzerindeyse yukarı yönde gönderir. Bu tür bir analiz, buluta iletilen ve bulutta depolanan veri miktarını azaltmak için yararlıdır. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturun.


## <a name="prerequisites"></a>Ön koşullar

* Hızlı başlangıçta [Linux](quickstart-linux.md) veya [Windows cihazları](quickstart.md) için oluşturduğunuz Azure IoT Edge cihazı.
* IoT Edge cihazı için birincil anahtar bağlantı dizesi.  
* [Visual Studio Code](https://code.visualstudio.com/). 
* [Visual Studio Code için C# (OmniSharp tarafından desteklenen) uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).
* Visual Studio Code için [Azure IoT Edge uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
* [.NET Core 2.1 SDK'sı](https://www.microsoft.com/net/download).
* Geliştirme makinenizde [Docker CE](https://docs.docker.com/install/). 


## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma
Bu öğreticide modül hazırlamak ve dosyalardan bir **kapsayıcı görüntüsü** oluşturmak için VS Code için Azure IoT Edge uzantısını kullanırsınız. Ardından bu görüntüyü, görüntülerinizin depolandığı ve yönetildiği **kayıt defterine** gönderirsiniz. Son olarak, görüntünüzü IoT Edge cihazınızda çalıştırmak üzere kayıt defterinizden dağıtırsınız.  

Bu öğretici için Docker ile uyumlu herhangi bir kayıt defteri kullanabilirsiniz. Bulutta sağlanan iki popüler Docker kayıt defteri hizmeti [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) ve [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)'dır. Bu öğreticide Azure Container Registry kullanılır. 

1. [Azure portalında](https://portal.azure.com), **Kaynak oluştur** > **Kapsayıcılar** > **Azure Container Registry**'yi seçin.
2. Kayıt defterinize bir ad verin, abonelik seçin, kaynak grubu seçin ve SKU'yu **Temel** olarak ayarlayın. 
3. **Oluştur**’u seçin.
4. Kapsayıcı kayıt defteriniz oluşturulduktan sonra, bu kayıt defterine gidin ve **Erişim anahtarları**'nı seçin. 
5. **Yönetici kullanıcı** ayarını **Etkinleştir**'e getirin.
6. **Oturum açma sunucusu**, **Kullanıcı adı** ve **Parola** değerlerini kopyalayın. Bu değerleri öğreticide ileride Docker görüntüsünü kayıt defterinizde yayımladığınızda ve kayıt defteri kimlik bilgilerini Edge çalışma zamanına eklediğinizde kullanacaksınız. 

## <a name="create-an-iot-edge-module-project"></a>IoT Edge modülü projesi oluşturma
Aşağıdaki adımlarda, Visual Studio Code ve Azure IoT Edge uzantısı kullanılarak .NET Core 2.0'a dayalı bir IoT Edge modülü projesinin nasıl oluşturulduğu gösterilmektedir.
1. Visual Studio Code'da, VS Code ile tümleşik terminali açmak için **Görünüm** > **Tümleşik Terminal**'i seçin.
2. VS Code komut paletini açmak için **View (Görünüm)** > **Command Palette (Komut Paleti)** öğesini seçin. 
3. Komut paletinde **Azure: Sign in** komutunu yazıp çalıştırdıktan sonra yönergeleri izleyerek Azure hesabınızda oturum açın. Oturumu önceden açtıysanız bu adımı atlayabilirsiniz.
4. Komut paletinde **Azure IoT Edge: New IoT Edge solution** komutunu yazıp çalıştırın. Komut paletinde çözümünüzü oluşturmak için aşağıdaki bilgileri girin: 
   1. Çözümü oluşturmak istediğiniz klasörü seçin. 
   2. Çözümünüz için bir ad girin veya varsayılan **EdgeSolution** adını kabul edin.
   3. Modül şablonu olarak **C# Module** girişini seçin. 
   4. Modülünüze **CSharpModule** adını verin. 
   5. İlk modülünüz için görüntü deposu olarak önceki bölümde oluşturduğunuz Azure Container Registry bileşenini belirtin. **localhost:5000** yerine kopyaladığınız oturum açma sunucusu değerini yazın. Dizenin son hali **\<kayıt adı\>.azurecr.io/csharpmodule** ifadesine benzer olmalıdır.
 
4. VS Code penceresi IoT Edge çözümü çalışma alanınızı yükler. **modules** klasörü, **.vscode** klasörü, dağıtım bildirimi şablon dosyası ve **.env** dosyası bulunur. **modules** > **CSharpModule** > **Program.cs** dosyasını açın.

5. **CSharpModule** ad alanının en üst kısmına daha sonra kullanılan türler için üç `using` deyimi yazın:

    ```csharp
    using System.Collections.Generic;     // for KeyValuePair<>
    using Microsoft.Azure.Devices.Shared; // for TwinCollection
    using Newtonsoft.Json;                // for JsonConvert
    ```

6. **Program** sınıfına `temperatureThreshold` değişkenini ekleyin. Bu değişken, IoT Hub'a veri gönderilmesi için ölçülen sıcaklığın aşması gereken değeri ayarlar. 

    ```csharp
    static int temperatureThreshold { get; set; } = 25;
    ```

7. **Program** sınıfına `MessageBody`, `Machine` ve `Ambient` sınıflarını ekleyin. Bu sınıflar gelen iletilerin gövdesi için beklenen şemayı tanımlar.

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

8. **Init** yöntemindeki kod, bir **ModuleClient** nesnesi oluşturur ve yapılandırır. Bu nesne, modülün ileti göndermek ve almak için yerel Azure IoT Edge çalışma zamanına bağlanmasını sağlar. **Init** yönteminde kullanılan bağlantı dizesi, modüle IoT T Edge çalışma zamanı tarafından sağlanır. Kod, **ModuleClient**'ı oluşturduktan sonra modül ikizinin istenen özelliklerinden TemperatureThreshold'u okur ve IoT Edge hub'ından **input1** uç noktası aracılığıyla iletiler almak için bir geri çağrı kaydeder. `SetInputMessageHandlerAsync` yöntemini yeni bir yöntemle değiştirin ve istenen özellik güncelleştirmeleri için bir `SetDesiredPropertyUpdateCallbackAsync` yöntemi ekleyin. Bu değişikliği yapmak için, **Init** yönteminin son satırının aşağıdaki kod ile değiştirin:

    ```csharp
    // Register callback to be called when a message is received by the module
    // await ioTHubModuleClient.SetImputMessageHandlerAsync("input1", PipeMessage, iotHubModuleClient);

    // Read TemperatureThreshold from Module Twin Desired Properties
    var moduleTwin = await ioTHubModuleClient.GetTwinAsync();
    var moduleTwinCollection = moduleTwin.Properties.Desired;
    try {
        temperatureThreshold = moduleTwinCollection["TemperatureThreshold"];
    } catch(ArgumentOutOfRangeException e) {
        Console.WriteLine($"Property TemperatureThreshold not exist: {e.Message}"); 
    }

    // Attach callback for Twin desired properties updates
    await ioTHubModuleClient.SetDesiredPropertyUpdateCallbackAsync(onDesiredPropertiesUpdate, null);

    // Register callback to be called when a message is received by the module
    await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FilterMessages, ioTHubModuleClient);
    ```

9. **Program** sınıfına `onDesiredPropertiesUpdate` yöntemini ekleyin. Bu yöntem modül ikizinin istenen özellikleri üzerinde yapılan güncelleştirmeleri alır ve **temperatureThreshold** değişkenini buna göre güncelleştirir. Tüm modüllerin, doğrudan buluttan bir modülün içinde çalışan kodu yapılandırmanıza izin veren kendi modül ikizi vardır.

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

10. `PipeMessage` yöntemini `FilterMessages` yöntemiyle değiştirin. Modül IoT Edge hub'ından bir ileti aldığında bu yöntem çağrılır. Modül ikizi aracılığıyla ayarlanan sıcaklık eşiğinin altındaki sıcaklıkları rapor eden iletileri filtreler. Ayrıca iletiye **MessageType** özelliğini ekleyip değerini **Alert** olarak ayarlar. 

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
                await moduleClient.SendEventAsync("output1", filteredMessage);
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
            var moduleClient = (ModuleClient)userContext;
            return MessageResponse.Abandoned;
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
            // Indicate that the message treatment is not completed
            ModuleClient moduleClient = (ModuleClient)userContext;
            return MessageResponse.Abandoned;
        }
    }
    ```

11. Bu dosyayı kaydedin.

## <a name="build-your-iot-edge-solution"></a>IoT Edge çözümünüzü derleyin

Bir önceki bölümde bir IoT Edge çözümü oluşturdunuz ve CSharpModule modülüne makine sıcaklığının kabul edilebilir eşiğin altında olduğunu bildiren iletileri filtreleyecek kodu eklediniz. Şimdi çözümü kapsayıcı görüntüsü olarak derlemeniz ve kapsayıcı kayıt defterine göndermeniz gerekiyor. 

1. Modül görüntüsünü ACR'ye gönderebilmek için Visual Studio Code tümleşik terminaline aşağıdaki komutu girerek Docker oturumu açın: 
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Birinci bölümde Azure Container Registry'den kopyaladığınız kullanıcı adını, parolayı ve oturum açma sunucusunu kullanın. Veya Azure portalındaki kaydınızın **Erişim anahtarları** bölümünden tekrar alın.

2. VS Code gezgininde IoT Edge çözüm çalışma alanınızdaki **deployment.template.json** dosyasını açın. Bu dosya `$edgeAgent` aracısına iki modülü dağıtmasını söyler: **tempSensor** ve **CSharpModule**. `CSharpModule.image`, görüntünün Linux amd64 sürümüne göre ayarlanmıştır. Dağıtım bildirimleri hakkında daha fazla bilgi edinmek için bkz. [IoT Edge modüllerinin kullanılmasını, yapılandırılmasını ve yeniden kullanılmasını anlama](module-composition.md).

3. **deployment.template.json** dosyasında, Docker kayıt defteri kimlik bilgilerinizin yer aldığı **registryCredentials** bölümü bulunur. Gerçek kullanıcı adı ve parola çiftleri, git tarafından yoksayılan .env dosyasında depolanır.

4. Dağıtım bildirimine CSharpModule modül ikizini ekleyin. Aşağıdaki JSON içeriğini `moduleContent` bölümünün en altına, `$edgeHub` modül ikizinin arkasına ekleyin: 
    ```json
        "CSharpModule": {
            "properties.desired":{
                "TemperatureThreshold":25
            }
        }
    ```

4. Bu dosyayı kaydedin.
5. VS Code gezgininde **deployment.template.json** dosyasına sağ tıklayıp **Build IoT Edge solution** (IoT Edge çözümü oluştur) öğesini seçin. 

Visual Studio Code uygulamasına çözümünüzü derleme komutu verdiğinizde dağıtım şablonundaki bilgileri alır ve yeni bir **config** klasöründe bir `deployment.json` dosyası oluşturur. Ardından tümleşik terminalde `docker build` ve `docker push` komutlarını çalıştırır. Bu iki komut kodunuzu derler, `CSharpModule.dll` ile kapsayıcı oluşturur ve bunu çözümü başlatırken belirttiğiniz kapsayıcı kayıt defterine gönderir. 

VS Code tümleşik terminalinde etiketle tam kapsayıcı görüntü adresini görebilirsiniz. Görüntü adresi `module.json` dosyasından **\<depo\>:\<sürüm\>-\<platform\>** biçiminde derlenir. Bu öğretici için **registryname.azurecr.io/csharpmodule:0.0.1-amd64** şeklinde olmalıdır.

## <a name="deploy-and-run-the-solution"></a>Çözümü dağıtma ve çalıştırma

1. Azure IoT Toolkit uzantısını IoT hub'ınızın bağlantı dizesiyle yapılandırın: 
    1. **Görünüm** > **Gezgin**'i seçerek VS Code gezginini açın. 
    2. Gezginde **AZURE IOT HUB CİHAZLARI**'na ve ardından **...** düğmesine tıklayın. **Select IoT Hub** (IoT Hub'ını Seçin) öğesine tıklayın. Yönergeleri izleyerek Azure hesabınızda oturum açın ve IoT hub'ınızı seçin. 
       **Set IoT Hub Connection String** (IoT Hub Bağlantı Dizesini Ayarla) öğesine tıklayarak da kurulumu gerçekleştirebilirsiniz. Açılır pencerede IoT Edge cihazınızın bağlandığı IoT hub'ının bağlantı dizesini girin.

2. Azure IoT Hub Devices (Azure IoT Hub Cihazları) gezgininde IoT Edge cihazınıza ve ardından **Create Deployment for IoT Edge device** (IoT Edge cihazı için dağıtım oluştur) öğesine tıklayın. **config** klasöründeki **deployment.json** dosyasını seçin ve ardından **Select Edge Deployment Manifest** (Edge Dağıtım Bildirimini Seç) öğesine tıklayın.

3. Yenile düğmesine tıklayın. Yeni **CSharpModule** ile **TempSensor** modülü ve **$edgeAgent** ile **$edgeHub** bileşenlerinin çalıştığını görmeniz gerekir. 

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

1. IoT hub'a gelen verileri izlemek için **...** simgesine tıklayıp **Start Monitoring D2C Messages** (D2C İletilerini İzlemeye Başla) öğesini seçin.
2. Belirli bir cihazın D2C iletilerini izlemek için listeden cihaza sağ tıklayıp **Start Monitoring D2C Messages** (D2C İletilerini İzlemeye Başla) öğesini seçin.
3. Verileri izlemeyi durdurmak için komut paletinden **Azure IoT Hub: Stop monitoring D2C message** komutunu seçin. 
4. Modül ikizini görüntülemek veya güncelleştirmek için listeden modüle sağ tıklayıp **Edit module twin** (Modül ikizini düzenle) öğesini seçin. Modül ikizini güncelleştirmek için ikiz JSON dosyasını kaydedip düzenleyici alanına sağ tıklayın ve **Update Module Twin** (Modül İkizini Güncelleştir) öğesini seçin.
5. Docker günlüklerini görüntülemek isterseniz VS Code için [Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) uygulamasını yükleyebilir ve çalışan modüllerinizi Docker gezgininde yerel olarak bulabilirsiniz. Tümleşik terminalde görüntülemek için bağlam menüsünde **Show Logs** (Günlükleri Göster) öğesine tıklayın.
 
## <a name="clean-up-resources"></a>Kaynakları temizleme 

<!--[!INCLUDE [iot-edge-quickstarts-clean-up-resources](../../includes/iot-edge-quickstarts-clean-up-resources.md)] -->

Bir sonraki önerilen makaleye geçecekseniz oluşturduğunuz kaynaklarla yapılandırmaları tutabilir ve yeniden kullanabilirsiniz.

Geçmeyecekseniz ücret kesilmesini önlemek için yerel yapılandırmalarınızı ve bu makalede oluşturulan Azure kaynaklarını silebilirsiniz. 

> [!IMPORTANT]
> Azure kaynaklarını ve kaynak grubunu silme işlemi geri alınamaz. Silindikten sonra kaynak grubu ve içindeki tüm kaynaklar kalıcı olarak silinir. Yanlış kaynak grubunu veya kaynakları yanlışlıkla silmediğinizden emin olun. IoT Hub'ı tutmak istediğiniz kaynakların bulunduğu mevcut bir kaynak grubunda oluşturduysanız kaynak grubunu silmek yerine IoT Hub kaynağını silin.
>

Yalnızca IoT Hub'ı silmek için hub adını ve kaynak grubu adını kullanarak aşağıdaki komutu çalıştırın:

```azurecli-interactive
az iot hub delete --name MyIoTHub --resource-group TestResources
```


Kaynak grubunun tamamını adıyla silmek için:

1. [Azure portalında](https://portal.azure.com) oturum açın ve **Kaynak grupları**’na tıklayın.

2. **Ada göre filtrele...** metin kutusuna IoT Hub'ınızın bulunduğu kaynak grubunun adını girin. 

3. Sonuç listesinde kaynak grubunuzun sağ tarafında **...** ve sonra **Kaynak grubunu sil**'e tıklayın.

4. Kaynak grubunun silinmesini onaylamanız istenir. Onaylamak için kaynak grubunuzun adını tekrar yazın ve **Sil**'e tıklayın. Birkaç dakika sonra kaynak grubu ve içerdiği kaynakların tümü silinir.



## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide IoT Edge cihazınız tarafından üretilen ham verileri filtrelemek için kod içeren bir IoT Edge modülü oluşturdunuz. Kendi modüllerinizi oluşturmaya hazır olduğunuzda [Visual Studio Code için Azure IoT Edge ile bir C# modülü geliştirme](how-to-develop-csharp-module.md) hakkında daha fazla bilgi edinebilirsiniz. Azure IoT Edge'in verileri iş içgörüsüne çevirmenize yardımcı olabilecek diğer yollar hakkında bilgi edinmek için bir sonraki öğreticiye geçebilirsiniz.

> [!div class="nextstepaction"]
> [SQL Server veritabanları ile uç cihazlarda veri depolama](tutorial-store-data-sql-server.md)

<!-- Links -->
[lnk-tutorial1-win]: quickstart.md
[lnk-tutorial1-lin]: quickstart-linux.md

<!-- Images -->
[1]: ./media/tutorial-csharp-module/programcs.png
[2]: ./media/tutorial-csharp-module/build-module.png
[3]: ./media/tutorial-csharp-module/docker-os.png
