---
title: Geliştirme ve hata ayıklama C# Visual Studio - Azure IOT Edge modülleri | Microsoft Docs
description: Geliştirmek ve Azure IOT Edge için C# modülünde hata ayıklama için Visual Studio 2017 kullanın
services: iot-edge
author: shizn
manager: philmea
ms.author: xshi
ms.date: 09/24/2018
ms.topic: article
ms.service: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 547989152320678ec195c4e8a93965cfbbd0f341
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53097854"
---
# <a name="use-visual-studio-2017-to-develop-and-debug-c-modules-for-azure-iot-edge-preview"></a>Geliştirmek ve Azure IOT Edge (Önizleme) için C# modülleri hata ayıklamak için Visual Studio 2017 kullanın

İçin Azure IOT Edge modülleri, iş mantığınızı kapatabilirsiniz. Bu makalede Visual Studio 2017 ana aracı olarak geliştirme ve modüller C# hata ayıklama için nasıl kullanılacağını gösterir.

Visual Studio için Azure IOT Edge araçları aşağıdaki avantajları sağlar:

- Oluşturma, düzenleme, derleme, çalıştırın ve Azure IOT Edge çözümlerini ve modüller, yerel geliştirme bilgisayarınızda hata ayıklayın.
- Azure IOT Hub aracılığıyla Azure IOT Edge cihazı için Azure IOT Edge çözümünüzü dağıtın.
- Azure IOT modüllerinizi C# ' de, tüm Visual Studio geliştirme avantajları yaparken kodu.
- Azure IOT Edge cihazları ve kullanıcı Arabirimi modülleri yönetir. 

Bu makalede, C# IOT Edge modülleri geliştirmek için Visual Studio 2017 için Azure IOT Edge araçlarını kullanmayı gösterir. Ayrıca, projenizi Azure IOT Edge cihazınıza dağıtmayı öğrenin.

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, bir bilgisayar veya geliştirme makinenize Windows çalıştıran sanal makine kullandığınızı varsayar. IOT Edge Cihazınızı başka bir fiziksel cihaz olabilir.

Bu makalede ana geliştirme aracı olarak Visual Studio 2017 kullandığından, Visual Studio'yu yükleyin. Eklediğiniz emin **Azure geliştirme iş yükü** Visual Studio 2017'yi yüklemenize. Yapabilecekleriniz [değiştirmek, Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/modify-visual-studio?view=vs-2017) ve Azure geliştirme iş yükü ekleyin.

Visual Studio 2017 hazır olduktan sonra ayrıca gerekir:

- İndirme ve yükleme [Azure IOT Edge uzantısını](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools) IOT Edge oluşturmak için Visual Studio Market'ten Visual Studio 2017'de proje.
- [Docker Community Edition](https://docs.docker.com/install/) geliştirme makinenizde derlemek ve çalıştırmak, modül görüntüleri için. Linux kapsayıcı modunda veya Windows kapsayıcı modunda çalışan Docker CE düzgün şekilde ayarlamanız gerekir.
- Hata ayıklama, çalıştırma ve IOT Edge çözümünüzü yerel geliştirme ortamını ayarlamak için ihtiyacınız [Azure IOT EdgeHub geliştirme aracı](https://pypi.org/project/iotedgehubdev/). Yükleme [(2.7/3.6) Python ve Pip](https://www.python.org/). Yüklemeyi **iotedgehubdev** aşağıdaki komutu, terminalde çalıştırarak. Azure IOT EdgeHub geliştirme aracı sürümünüz 0.3.0 büyük olduğundan emin olun.

   ```cmd
   pip install --upgrade iotedgehubdev
   ```

- [Azure kapsayıcı kayıt defteri](https://docs.microsoft.com/azure/container-registry/) veya [Docker hub'ı](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)
   - Prototip ve test etme amacıyla bir bulut kayıt defteri yerine yerel bir Docker kayıt defteri kullanabilirsiniz. 

- Modülünüzün test etmek için oluşturulan en az bir IOT Edge cihaz Kimliğine sahip etkin bir IOT hub gerekir. IOT Edge güvenlik arka plan programı geliştirme makinesinde çalıştırıyorsanız, Visual Studio'da geliştirme başlamadan önce EdgeHub ve EdgeAgent durdurmanız gerekebilir. 

### <a name="check-your-tools-version"></a>Araçlar sürümünüzü kontrol edin

1. Gelen **Araçları** menüsünde seçin **Uzantılar ve güncelleştirmeler**. Genişletin **yüklü > Araçları** bulabilirsiniz **Azure IOT Edge** ve **Cloud Explorer**.

2. Yüklü sürüm unutmayın. Bu sürüm Visual Studio Market'te en son sürümle karşılaştırabilirsiniz ([Cloud Explorer](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS), [Azure IOT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge))

3. Sürümünüzün eski ise, Visual Studio Araçları aşağıdaki bölümde gösterildiği gibi güncelleştirin.

### <a name="update-your-tools"></a>Sizin araçlarınız güncelleştir

1. İçinde **Uzantılar ve güncelleştirmeler** iletişim kutusunda Genişlet **güncelleştirmeleri > Visual Studio Market**, seçin **Azure IOT Edge** veya **Cloud Explorer**seçip **güncelleştirme**.

2. Araçları güncelleştirme yüklendikten sonra Visual Studio Araçları VSIX Yükleyicisi'ni kullanarak güncelleştirme tetikleyiciye kapatın.

3. Yükleyicide seçin **Tamam** başlatın ve ardından araçlarını güncelleştirmek üzere değiştirmek için.

4. Güncelleştirme tamamlandıktan sonra Kapat'ı seçin ve Visual Studio'yu yeniden başlatın.

### <a name="create-an-azure-iot-edge-project"></a>Azure IOT Edge projesi oluşturma

Visual Studio'da Azure IOT Edge proje şablonu, Azure IOT hub'ı Azure IOT Edge cihazlarına dağıtılabilir bir proje oluşturur. İlk olarak, Azure IOT Edge çözümünü oluşturun ve ardından ilk C# modülü içindeki çözümü oluşturun. Her IOT Edge çözüm, birden fazla modülü içerebilir. 

1. Visual Studio'da **Dosya** menüsünden **Yeni** > **Proje**’yi seçin.

2. İçinde **yeni proje** iletişim kutusunda **yüklü**, genişletme **Visual C# > bulut**seçin **Azure IOT Edge**, tür a  **Adı** projeniz için konumu belirtin ve tıklayın **Tamam**. Varsayılan proje adı **AzureIoTEdgeApp1**. 

   ![Yeni Proje](./media/how-to-visual-studio-develop-csharp-module/create-new.jpg)

3. İçinde **IOT Edge modülü Yapılandırması** penceresinde **C# Modülü** yazın ve modül görüntü deposuna ve modül adı belirtin.  VS autopopulates modül adı ile **localhost:5000**. Kayıt defteri kendi bilgilerinizle değiştirin. Test etmek için yerel bir Docker kayıt defteri kullanıyorsanız localhost uygundur. Azure Container Registry kullanırsanız, oturum açma sunucusu defterinizin ayarlarından'ni kullanın. Oturum açma sunucusu benzer  **<registry name>. azurecr.io**. Dizenin yalnızca localhost bölümünü değiştirin, modülünüzün adını silmeyin. Varsayılan modül adı **IoTEdgeModule1**

4. Tıklayın **Tamam** bir C# modülü ile Azure IOT Edge projeyi oluşturmak için.

Artık elinizde bir **AzureIoTEdgeApp1** projesi ve bir **IoTEdgeModule1** Çözümümüzü projesinde. **AzureIoTEdgeApp1** proje içeren bir **deployment.template.json** dosya. Bu dosya, IOT Edge çözümünüz için derlemek ve dağıtmak için istediğiniz modülleri tanımlar ve modüller arasındaki yolları tanımlar. Varsayılan çözümü olan bir **tempSensor** modülü ve **IoTEdgeModule1** modülü. **TempSensor** modülü sanal veri oluşturur **IoTEdgeModule1** modülü, varsayılan kodunda çalışırken **IoTEdgeModule1** modülüdür bir kanal iletisi modülü, Kanal alınan iletileri IOT hub'ına doğrudan.

**IoTEdgeModule1** projedir bir.Net Core 2.1 konsol uygulaması. İçerdiği gerekli **dockerfile'ları** Windows kapsayıcı ya da Linux kapsayıcısı çalıştıran IOT Edge cihazınız için gerekir. **Module.json** dosya bir modül meta verilerini açıklar. **Program.cs** Azure IOT cihaz SDK'sı bağımlılık olarak alır gerçek modülü kodu.

## <a name="develop-your-module"></a>Modülü geliştirme

Çözümünüzle birlikte gelen varsayılan C# modülü kodu şu konumdadır **IoTEdgeModule1** > **Program.cs**. Çözümü derleyin, kapsayıcı kayıt defterinize itme ve herhangi bir kod dokunmadan testi başlatmak için bir aygıta dağıtmak modülü ve deployment.template.json dosya ayarlanır. Modül, yalnızca bir kaynak (Bu durumda, veri benzetimi gerçekleştiren tempSensor Modülü) gelenlerin ve IOT Hub'ına kanal için oluşturulmuştur. 

Kendi kodunuzu ile C# şablonunu özelleştirmek hazır olduğunuzda kullanın [Azure IOT Hub SDK'ları](../iot-hub/iot-hub-devguide-sdks.md) anahtar gereken güvenlik, cihaz yönetimi ve güvenilirlik gibi IOT çözümleri için bu adrese modüller oluşturmak için. 

## <a name="initialize-iotegehubdev-with-iot-edge-device-connection-string"></a>Başlatma **iotegehubdev** IOT Edge cihaz bağlantı dizesiyle

1. Tüm IOT Edge cihaz bağlantı dizesini almak gereken, Cloud Explorer takip Visual Studio 2017'den "Primary CONNECTION Strıng'i" değerini Kopyala. Lütfen kenar-olmayan cihaz bağlantı dizesini kopyalamayın, IOT Edge cihazı simgesini kenar-olmayan cihaz birinden farklıdır.

   ![Edge cihaz bağlantı dizesini kopyalayın](./media/how-to-visual-studio-develop-csharp-module/copy-edge-conn-string.png)

2. Sağ tıklayın gerek **AzureIoTEdgeApp1** bağlam menüsünü açın ve ardından Proje **Edge cihaz bağlantı dizesini ayarlamak**, Azure IOT Edge Kurulum penceresi görüntülenir.

   ![Küme kenar bağlantı dizesi penceresini açın](./media/how-to-visual-studio-develop-csharp-module/set-edge-conn-string.png)

3. Kurulumu penceresinde Lütfen ilk adımda aldığınız bağlantı dizesini girebilirsiniz öğesini tıklatıp **Tamam** düğmesi.

>[!NOTE]
>Bu tek seferlik iş, yalnızca bu adımı bir kez bir makine, tüm sonraki Azure IOT çözümleri için ücretsiz alırsa Edge üzerinde çalıştırmanız gerekir. Elbette, bu adım bağlantı dizesi geçersiz veya başka bir bağlantı dizesini değiştirmek gerekirse yeniden çalıştırabilirsiniz.

## <a name="build-and-debug-single-c-module"></a>Derleme ve tek C# modülünde hata ayıklama

Genellikle, biz bütün bir çözüm içinde birden çok modül ile çalışmasını yapmadan önce her modülü test/debug istiyoruz.

1. Seçin **IoTEdgeModule1** bağlam menüsünden başlangıç projesi olarak.

   ![Başlangıç projesi ayarlama](./media/how-to-visual-studio-develop-csharp-module/module-start-up-project.png)

2. Tuşuna **F5** veya modül çalıştırmak için aşağıdaki düğmeye tıklayın, 10 sürebilir ~ süresi ilk 20 saniye.

   ![Modül çalıştırın](./media/how-to-visual-studio-develop-csharp-module/run-module.png)


3. Bir.Net Core görmelisiniz modülü başarıyla başlatılmışsa konsol uygulaması başlatıldı.

   ![Çalışan Modülü](./media/how-to-visual-studio-develop-csharp-module/single-module-run.png)

4. Bir kesme noktası ayarlayabilirsiniz artık **PipeMessage** içinde **Program.cs**, ardından aşağıdaki komutu çalıştırarak ileti gönderme, **Git Bash** veya **WSL Bash**  (çalıştırma, Powershell ve CMD çözümlenmediğinde) (Bu komut ayrıca VS çıkış penceresinde bulabilirsiniz):

    ```cmd
    curl --header "Content-Type: application/json" --request POST --data '{"inputName": "input1","data":"hello world"}' http://localhost:53000/api/v1/messages
    ```

   ![Tek bir modülün hatalarını ayıklamak](./media/how-to-visual-studio-develop-csharp-module/debug-single-module.png)

    Kesme noktasını tetikledi. Visual Studio Yereller penceresinde değişkenleri izleyebilirsiniz.

   > [!TIP]
   > Ayrıca [PostMan](https://www.getpostman.com/) veya yerine üzerinden ileti göndermek için API araçlara `curl`.

5. Tuşuna **Ctrl + F5 tuşlarına basarak** veya hata ayıklamayı durdurmak için Durdur düğmesini tıklatın.

## <a name="build-and-debug-iot-edge-solution-with-multiple-modules"></a>Derleme ve IOT Edge çözüm birden çok modül ile hata ayıklama

Tek modülü geliştirme tamamladıktan sonra İleri, çalıştırın ve tüm çözümü birden çok modül ile hata ayıklama istiyoruz.

1. İkinci, C# modülü çözüme ekleyin. Sağ **AzureIoTEdgeApp1** seçip **Ekle** -> **yeni IOT Edge Modülü**. İkinci modül varsayılan adıdır **IoTEdgeModule2** ve bunu hala bir kanal modüldür.

2. Gidin **deployment.template.json**. Göreceğiniz **IoTEdgeModule2** içinde eklenen **modülleri** bölümü. Değiştirin **yollar** aşağıdaki bölümü. Modül adlarınızı özelleştirdiyseniz, adlarını değiştirme sonra aşağıdaki emin olun.

    ```json
        "routes": {
          "IoTEdgeModule1ToIoTHub": "FROM /messages/modules/IoTEdgeModule1/outputs/* INTO $upstream",
          "sensorToIoTEdgeModule1": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/IoTEdgeModule1/inputs/input1\")",
          "IoTEdgeModule2ToIoTHub": "FROM /messages/modules/IoTEdgeModule2/outputs/* INTO $upstream",
          "sensorToIoTEdgeModule2": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/IoTEdgeModule2/inputs/input1\")"
        },
    ```

3. Ayarlama **AzureIoTEdgeApp1** proje bağlam menüsünde başlangıç projesi olarak.

4. Kesme noktaları ayarlayın ve ardından çalıştırın ve birden çok modülle aynı anda hata ayıklama, F5 tuşuna basın. Birden çok.Net Core görmelisiniz konsol uygulaması windows, her pencere, C# modülü gösterir. 

   ![Birden çok modül hata ayıklama](./media/how-to-visual-studio-develop-csharp-module/debug-multiple-modules.png)

5. Tuşuna **Ctrl + F5 tuşlarına basarak** veya hata ayıklamayı durdurmak için Durdur düğmesini tıklatın.

## <a name="build-and-push-images"></a>Derleme ve görüntüleri gönderme

1. Emin **AzureIoTEdgeApp1** başlangıç projeniz. Seçin **hata ayıklama** veya **yayın** modülü görüntülerinizi oluşturmak için yapılandırma.

    > [!NOTE]
    > Seçerken **hata ayıklama**, VS kullanacağı `Dockerfile.debug` Docker görüntülerinizi oluşturmak için. Bu .NET Core komut satırı hata ayıklayıcı VSDBG kapsayıcı görüntünüzü oluşturma sırasında içerir. Kullanmanızı öneririz **yayın** kullanan yapılandırma `Dockerfile` VSDBG üretime hazır IOT Edge modülleri için olmadan.

2. Azure Container Registry gibi özel kayıt defteri kullanıyorsanız, terminalinizde Docker oturum aşağıdaki komutu çalıştırın. Yerel kayıt defteri kullanıyorsanız, yapabilecekleriniz [yerel bir kayıt defteri çalıştırma](https://docs.docker.com/registry/deploying/#run-a-local-registry).

    ```cmd
    docker login -u <ACR username> -p <ACR password> <ACR login server> 
    ```

3. Azure Container Registry gibi özel kayıt defteri kullanıyorsanız, kayıt defteri kullanıcı adı ve parola yerleştirmek gereken `deployment.template.json` çalışma zamanı ayarları aşağıdaki içeriğe sahip. Yer tutucusunu, gerçek bir yönetici kullanıcı adı ve parola ile değiştirmeyi unutmayın.

    ```json
          "settings": {
            "minDockerVersion": "v1.25",
            "loggingOptions": "",
            "registryCredentials": {
              "registry1": {
                "username": "<username>",
                "password": "<password>",
                "address": "<registry name>.azurecr.io"
              }
            }
          }
    ```

4. Sağ tıklayın **AzureIoTEdgeApp1** ve bağlam menüsü öğesi **derleme ve uç çözümü anında iletme**, oluşturur ve her bir modül için docker görüntüsü gönderme.

   ![Derleme ve görüntüleri gönderme](./media/how-to-visual-studio-develop-csharp-module/build-and-push.png)


## <a name="deploy-the-solution"></a>Çözümü dağıtma

IoT Edge cihazınızı ayarlamak için kullandığınız hızlı başlangıç makalesinde Azure portalı kullanarak bir modül dağıttınız. Modüller için Visual Studio Cloud Explorer'ı kullanarak da dağıtabilirsiniz. Senaryonuz için hazırlanmış bir dağıtım bildirimi zaten `deployment.json` dosya. Tek yapmanız gereken dağıtımı almak üzere bir cihaz seçmek.

1. Açık **Cloud Explorer** tıklayarak **görünümü** > **Cloud Explorer**. Visual Studio 2017'de, hesabınızla oturum açtığınız emin olun.

2. İçinde **Cloud Explorer**, aboneliğinizi ve Azure IOT Hub'ınıza ve dağıtmak istediğiniz Azure IOT Edge cihazı bulun.

3. IOT Edge cihazı için dağıtımı oluşturmak için sağ tıklayın, dağıtım bildirimi dosyası altında seçmeniz gerekebilir `$AzureIoTEdgeAppSolutionDir\config\deployment.(amd64|amd64.debug|windows-amd64).json`.

>>[!NOTE]
>>Seçmelisiniz değil `$AzureIoTEdgeAppSolutionDir\config\deployment_for_local_debug.json`

4. Yenile düğmesine tıklayın. İle birlikte çalışan yeni modüller görmelisiniz **TempSensor** modülü ve **$edgeAgent** ve **$edgeHub**.

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

1. Belirli bir cihaz için D2C iletisini izlemek için listedeki bir cihaza tıklayın ve tıklayın **izleme D2C iletileri Başlat** içinde **eylem** penceresi.

2. Verileri izlemeyi durdurmak için listedeki bir cihaza tıklayın ve seçin **izleme D2C iletileri Durdur** içinde **eylem** penceresi.

## <a name="next-steps"></a>Sonraki adımlar

Cihazlarınızı IOT Edge modülleri geliştirmek için [kavrama ve kullanma Azure IOT Hub SDK'ları](../iot-hub/iot-hub-devguide-sdks.md).
