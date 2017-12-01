---
title: "Azure IOT kenarıyla Azure işlevi dağıtma | Microsoft Docs"
description: "Sınır cihazı bir modüle olarak Azure işlevi dağıtma"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: v-jamebr
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 710a83ba693ad72730ea0dabee6b5d7f4638da95
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2017
---
# <a name="deploy-azure-function-as-an-iot-edge-module---preview"></a>Bir IOT kenar modül Azure işlevi dağıtma - Önizleme
Azure işlevleri, iş mantığınızı IOT sınır cihazları için doğrudan uygulayan kod dağıtmak için kullanabilirsiniz. Bu öğreticide, oluşturma ve dağıtma algılayıcı verileri üzerinde bir sanal cihaz dağıtmak Azure IOT kenarına oluşturduğunuz sanal IOT sınır aygıtında filtreler bir Azure işlevi aracılığıyla açıklanmaktadır [Windows] [ lnk-tutorial1-win]veya [Linux] [ lnk-tutorial1-lin] öğreticileri. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:     

> [!div class="checklist"]
> * Bir Azure işlevi oluşturmak için Visual Studio Code kullanın
> * Docker görüntü oluşturma ve kayıt defterine yayımlama için VS Code ve Docker kullanın 
> * Modül IOT kenar Cihazınızı dağıtma
> * Oluşturulan görünüm verileri


Bu öğreticide oluşturduğunuz Azure işlevi cihazınız tarafından oluşturulan sıcaklık verileri filtreler ve sıcaklık belirtilen eşiğin üzerinde yalnızca upstream iletileri Azure IOT Hub'ına gönderir. 

## <a name="prerequisites"></a>Ön koşullar

* Hızlı Başlangıç ya da önceki öğreticide oluşturduğunuz Azure IOT sınır cihazı.
* [Visual Studio Code](https://code.visualstudio.com/). 
* [C# Visual Studio Code (OmniSharp tarafından desteklenen) uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). (Uzantısı Visual Studio Code uzantıları panelinden yükleyebilirsiniz.)
* [Visual Studio Code için Azure IOT kenar uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). (Uzantısı Visual Studio Code uzantıları panelinden yükleyebilirsiniz.)
* [Docker](https://docs.docker.com/engine/installation/). Bu öğretici için yeterli platformunuzun Community Edition (CE) olur. 
* [.NET 2.0 SDK çekirdek](https://www.microsoft.com/net/core#windowscmd). 

## <a name="set-up-a-docker-registry"></a>Bir Docker kayıt defteri ayarlayın
Bu öğreticide, VS Code'da Azure IOT kenar uzantısı bir Azure işlevi oluşturma ve derleme için kullandığınız bir [Docker görüntü](https://docs.docker.com/glossary/?term=image) onunla. Bu Docker görüntüsüne itme sonra bir [Docker deposu](https://docs.docker.com/glossary/?term=repository) tarafından barındırılan bir [Docker kayıt defteri](https://docs.docker.com/glossary/?term=registry). Son olarak, Docker görüntünüzü olarak paketlenmiş dağıttığınız bir [Docker kapsayıcısı](https://docs.docker.com/glossary/?term=container) IOT kenar aygıtınıza, kayıt defterinden.  

Bu öğretici için Docker uyumlu kayıt kullanabilirsiniz. İki popüler Docker kayıt defteri hizmetlerinin kullanılabilir bulutta **Azure kapsayıcı kayıt defteri** ve **Docker hub'a**:

- [Azure kapsayıcı kayıt defteri](https://docs.microsoft.com/en-us/azure/container-registry/) kullanılabilir bir [Ücretli aboneliği](https://azure.microsoft.com/en-us/pricing/details/container-registry/). Bu öğretici için **temel** abonelik yeterli. 

- [Docker hub'a](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags) , Docker (ücretsiz) bir kimliği için kaydolursanız bir ücretsiz özel depo sunar 
    1. Docker kimliği için kaydolmak için'ndaki yönergeleri izleyin [kaydetmek için bir Docker kimliği](https://docs.docker.com/docker-id/#register-for-a-docker-id) Docker sitesinde. 

    2. Özel bir Docker deposu oluşturmak için yönergeleri izleyin [Docker hub'ına yeni bir havuz oluşturma](https://docs.docker.com/docker-hub/repos/#creating-a-new-repository-on-docker-hub) Docker sitesinde.

Bu öğreticide, uygun olan yerlerde, komutları Azure kapsayıcı kayıt defteri ve Docker hub'a için sağlanır.

## <a name="create-a-function-project"></a>Bir işlev projesi oluşturma
Aşağıdaki adımlar, Visual Studio Code ile Azure IOT kenar uzantısı bir IOT kenar işlevi oluşturma gösterir.
1. Açık VS kodu.
2. Kullanım **View | Terminal tümleşik** menü komutu VS Code tümleşik Terminali açın.
3. Tümleşik terminale yüklemek (veya güncelleştirmek için) aşağıdaki komutu girin **AzureIoTEdgeFunction** dotnet şablonunda:

    ```cmd/sh
    dotnet new -i Microsoft.Azure.IoT.Edge.Function
    ```
2. Tümleşik terminale yeni modül için bir proje oluşturmak için aşağıdaki komutu girin:

    ```cmd/sh
    dotnet new aziotedgefunction -n FilterFunction
    ```

    >[!NOTE]
    > Bu komut, proje klasörünü oluşturur **FilterFunction**, geçerli çalışma klasörü içinde. Başka bir konumda oluşturmak istiyorsanız, dizinleri komutu çalıştırmadan önce değiştirin.

3. Kullanım **dosya | Klasörü açın** menü komutu, Gözat ' **FilterFunction** klasörü ve tıklatın **klasörü seçin** proje VS Code'da açın.
4. VS Code Explorer'da tıklatın **EdgeHubTrigger Csharp** klasörü, ardından **run.csx** açmak için dosya.
5. Sonra aşağıdaki deyimine `#r "Microsoft.Azure.Devices.Client"` deyimi:

    ```csharp
    #r "Newtonsoft.Json"
    ```

5. Aşağıdaki using deyimlerini varolan sonra `using` deyimleri:

    ```csharp
    using Newtonsoft.Json;
    ```

1. Aşağıdaki sınıflar ekleyin. Bu sınıfların gelen ileti gövdesini beklenen şemasını tanımlayın.

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

1. Gövdesini değiştirin **çalıştırmak** aşağıdaki kod ile yöntemi. İleti gövdesi sıcaklık değerinde ve sıcaklık eşik değeri göre iletileri filtreler.

    ```csharp
    const int temperatureThreshold = 25;

    byte[] messageBytes = messageReceived.GetBytes();
    var messageString = System.Text.Encoding.UTF8.GetString(messageBytes);

    if (!string.IsNullOrEmpty(messageString))
    {
        // Get the body of the message and deserialize it
        var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);
        
        if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
        {
            // We will send the message to the output as the temperature value is greater than the threashold
            var filteredMessage = new Message(messageBytes);
            // We need to copy the properties of the original message into the new Message object
            foreach (KeyValuePair<string, string> prop in messageReceived.Properties)
            {
                filteredMessage.Properties.Add(prop.Key, prop.Value);
            }
            // We are adding a new property to the message to indicate it is an alert
            filteredMessage.Properties.Add("MessageType", "Alert");
            // Send the message        
            await output.AddAsync(filteredMessage);
            log.Info("Received and transferred a message with temperature above the threshold");
        }
    }
    ```

11. Dosyayı kaydedin.

## <a name="publish-a-docker-image"></a>Docker görüntü yayımlama

1. Docker görüntüsünü oluşturabilirsiniz.
    1. VS Code Explorer'da tıklatın **Docker** klasörünü açmak için. Kapsayıcı platformunuz için klasör ya da seçin **linux x64** veya **windows nano**. 
    2. Sağ **Dockerfile** dosya ve tıklayın **yapı IOT kenar modülü Docker görüntü**. 
    3. İçinde **Klasör Seç** kutusunda, proje klasöre gidin **FilterFunction**, tıklatıp **EXE_DIR olarak klasör seç**. 
    4. VS Code pencerenin üstündeki açılır metin kutusuna görüntü adı girin. Örneğin, `<docker registry address>/filterfunction:latest`; burada *docker kayıt defteri adresi* Docker hub'a kullanıyorsanız, Docker kimliğidir veya benzer `<your registry name>.azurecr.io`, Azure kapsayıcı kayıt defteri kullanıyorsanız.
 
4. Docker için oturum açın. Tümleşik terminal içinde aşağıdaki komutu girin: 

    - Docker hub'a (istendiğinde, kimlik bilgilerinizi girin):
        
        ```csh/sh
        docker login
        ```

    - Azure kapsayıcı kayıt defteri:
        
        ```csh/sh
        docker login -u <username> -p <password> <Login server>
        ```
        
        Kullanıcı adı, bu komutu kullanmak için parola ve oturum açma sunucusu bulmak için [Azure portalına] gidin (https://portal.azure.com). Gelen **tüm kaynakları**, Azure kapsayıcı kaydınız özelliklerini açın ve ardından kutucuğa tıklayın **erişim anahtarları**. Değerleri kopyalamak **kullanıcıadı**, **parola**, ve **oturum açma sunucusu** alanları. Oturum açma sunucusu sould biçiminde olmalıdır: `<your registry name>.azurecr.io`.

3. Görüntü Docker deponuza iletin. Kullanım **View | Palet komutu... | Edge: Anında iletme IOT kenar modülü Docker görüntü** menü komutu ve VS Code pencerenin üstündeki açılır metin kutusuna görüntü adı girin. Adımda kullanılan aynı görüntü adı kullanmak 1.c.

## <a name="add-registry-credentials-to-your-edge-device"></a>Kayıt defteri kimlik bilgilerini kenar Cihazınızı ekleyin
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

1. İçinde **Azure portal**, IOT hub'ına gidin.
2. Git **IOT kenar (Önizleme)** ve IOT sınır cihazı seçin.
1. Seçin **ayarlamak modülleri**. 
2. Ekleme **tempSensor** modülü. Bu adım yalnızca bağlıdır, değil daha önce dağıttıysanız gerekli **tempSensor** IOT kenar aygıtınıza modülü.
    1. Seçin **IOT kenar Modül Ekle**.
    2. İçinde **adı** alanına, `tempSensor`.
    3. İçinde **görüntü URI** alanına, `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`.
    4. Diğer ayarları değiştirmeden bırakın ve tıklayın **kaydetmek**.
1. Ekleme **filterfunction** modülü.
    1. Seçin **IOT kenar Modül Ekle** yeniden.
    2. İçinde **adı** alanına, `filterfunction`.
    3. İçinde **görüntü** alan, görüntü adresinizi girin; örneğin `<docker registry address>/filterfunction:latest`.
    74. **Kaydet** düğmesine tıklayın.
2. **İleri**’ye tıklayın.
3. İçinde **belirtin yollar** adım, JSON altındaki metin kutusuna Kopyala. Modülleri tüm iletileri kenar çalışma zamanına yayımlayın. Burada bu iletileri akış çalışma zamanında bildirim temelli kuralları tanımlar. Bu öğreticide, iki yol gerekir. İlk yol sıcaklık algılayıcısı iletilerden filtresi modülü ile yapılandırılmış uç noktası "input1" uç nokta aracılığıyla taşımaları **FilterMessages** işleyicisi. İkinci yol filtresi modülü gelen iletileri IOT Hub'ına taşımaları. Bu rotadaki `upstream` kenar Hub'ın IOT Hub'ına iletileri göndermek için söyler özel bir hedef. 

    ```json
    {
       "routes":{
          "sensorToFilter":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filterfunction/inputs/input1\")",
          "filterToIoTHub":"FROM /messages/modules/filterfunction/outputs/* INTO $upstream"
       }
    }
    ```

4. **İleri**’ye tıklayın.
5. İçinde **gözden geçirme şablonu** adımını, **gönderme**. 
6. IOT Edge cihaz ayrıntıları sayfasına dönün ve **yenileme**. Yeni görmelisiniz **filterfunction** modülü ile birlikte çalışan **tempSensor** modülü ve **IOT kenar çalışma zamanı**. 

## <a name="view-generated-data"></a>Oluşturulan görünüm verileri

Cihaz bulut iletilerini IOT kenar cihazınızın IOT hub'ına gönderilen izlemek için:
1. IOT hub'ınız için bağlantı dizesi ile Azure IOT Araç Seti uzantısı yapılandırın: 
    1. Kullanım **View | Explorer** VS Code Gezgini'ni açmak için menü komutu. 
    3. Explorer'ın tıklatın **IOT HUB CİHAZLARI** ve ardından **...** . Tıklatın **IOT Hub bağlantı dizesine ayarlamak** ve açılır pencerede IOT kenar Cihazınızı bağlandığı IOT hub'ına yönelik bağlantı dizesini girin. 

        Bağlantı dizesi bulmak için Azure portalında IOT hub'ınız için kutucuğa tıklayın ve ardından **paylaşılan erişim ilkeleri**. İçinde **paylaşılan erişim ilkeleri**, tıklatın **iothubowner** İlkesi ve kopyalama IOT Hub bağlantı dizesi içinde **iothubowner** penceresi.   

1. IOT hub'ına ulaşan verilerin izlemek için kullanın **View | Palet komutu... | IOT: İzleme D2C iletisi başlangıç** menü komutu. 
2. Veri izlemeyi durdurmak için kullanmak **View | Palet komutu... | IOT: D2C ileti izlemeyi durdurmak** menü komutu. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, IOT kenar cihazınız tarafından oluşturulan ham verileri filtrelemek için kodu içeren bir Azure işlevi oluşturuldu. Azure IOT kenar keşfetme tutmak için bir IOT sınır cihazı bir ağ geçidi olarak kullanmayı öğrenin. 

> [!div class="nextstepaction"]
> [Bir IOT sınır ağ geçidi cihazı oluşturma](how-to-create-transparent-gateway.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
