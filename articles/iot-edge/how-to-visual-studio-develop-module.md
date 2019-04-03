---
title: Geliştirme ve hata ayıklama, Visual Studio - Azure IOT Edge modülleri | Microsoft Docs
description: Geliştirme ve modülleri, Azure IOT Edge için hata ayıklama için Visual Studio 2017 kullanın
services: iot-edge
author: shizn
manager: philmea
ms.author: xshi
ms.date: 04/03/2019
ms.topic: article
ms.service: iot-edge
ms.custom: seodec18
ms.openlocfilehash: f2228726d4edc25efe46a660d25d398959c3ea59
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58851946"
---
# <a name="use-visual-studio-2017-to-develop-and-debug-modules-for-azure-iot-edge-preview"></a>Geliştirme ve modülleri, Azure IOT Edge (Önizleme) için hata ayıklama için Visual Studio 2017 kullanın

İçin Azure IOT Edge modülleri, iş mantığınızı kapatabilirsiniz. Bu makalede, Visual Studio 2017 ana aracı olarak geliştirme ve hata ayıklama modüller için nasıl kullanılacağını gösterir.

Visual Studio için Azure IOT Edge araçları aşağıdaki avantajları sağlar:

- Oluşturma, düzenleme, oluşturma, çalıştırma ve Azure IOT Edge çözümlerini ve modüller, yerel geliştirme bilgisayarınızda hata ayıklama.
- Azure IOT Hub aracılığıyla Azure IOT Edge cihazı için Azure IOT Edge çözümünüzü dağıtın.
- Azure IOT modüllerinizi c kodu veya C# tüm Visual Studio geliştirme avantajları sahip oluştu.
- Azure IOT Edge cihazları ve kullanıcı Arabirimi modülleri yönetir.

Bu makalede, IOT Edge modülleri geliştirmek için Visual Studio 2017 için Azure IOT Edge araçlarını kullanmayı gösterir. Ayrıca, projenizi Azure IOT Edge cihazınıza dağıtmayı öğrenin.

> [!TIP]
> Visual Studio tarafından oluşturulan IOT Edge Proje yapısı Visual Studio Code ile aynı değil.
  
## <a name="prerequisites"></a>Önkoşullar

Bu makalede, bir bilgisayar veya geliştirme makinenize Windows çalıştıran sanal makine kullandığınızı varsayar. IOT Edge Cihazınızı başka bir fiziksel cihaz olabilir.

Bu makalede ana geliştirme aracı olarak Visual Studio 2017 kullandığından, Visual Studio'yu yükleyin. Dahil olduğundan emin olun **Azure geliştirme** ve **C++ ile masaüstü geliştirme** Visual Studio 2017 yüklemenizi iş yükleri. Yapabilecekleriniz [değiştirmek, Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/modify-visual-studio?view=vs-2017) gerekli iş yüklerini eklemek için.

Visual Studio 2017 hazır olduktan sonra aşağıdaki araçları ve bileşenleri de gerekir:

- İndirme ve yükleme [Azure IOT Edge uzantısını (Önizleme)](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools) Visual Studio 2017'de bir IOT Edge projesi oluşturmak için Visual Studio Market'ten.

- İndirme ve yükleme [Docker Community Edition](https://docs.docker.com/install/) geliştirme makinenizde derlemek ve çalıştırmak, modül görüntüleri için. Linux kapsayıcı modu veya Windows kapsayıcı modu çalıştırmak için Docker CE kümesi gerekir.

- Çalıştırma, hata ayıklamak için yerel geliştirme ortamınızı ayarlama ve IOT Edge çözümünüzü yükleyerek test [Azure IOT EdgeHub geliştirme aracı](https://pypi.org/project/iotedgehubdev/). Yükleme [(2.7/3.6) Python ve Pip](https://www.python.org/) ve yüklemeyi **iotedgehubdev** terminalinizde şu komutu çalıştırarak paketi. Azure IOT EdgeHub geliştirme aracı sürümünüz 0.3.0 büyük olduğundan emin olun.

   ```cmd
   pip install --upgrade iotedgehubdev
   ```

- Depoyu kopyalamak ve Vcpkg Kitaplık Yöneticisi'ni yükleyin ve yüklemeyi **azure-iot-sdk-c paket** Windows için.

  ```cmd
  git clone https://github.com/Microsoft/vcpkg
  cd vcpkg
  bootstrap-vcpkg.bat
  ```

  ```cmd
  vcpkg.exe install azure-iot-sdk-c:x64-windows
  vcpkg.exe --triplet x64-windows integrate install
  ```

- [Azure kapsayıcı kayıt defteri](https://docs.microsoft.com/azure/container-registry/) veya [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags).

  > [!TIP]
  > Prototip ve test etme amacıyla bir bulut kayıt defteri yerine yerel bir Docker kayıt defteri kullanabilirsiniz.

- Modülünüzün bir cihazda test etmek için etkin bir IOT hub ile en az bir IOT Edge cihazı gerekir. Bilgisayarınızı bir IOT Edge cihazı kullanmak için hızlı başlangıç için adımları izleyin. [Linux](quickstart-linux.md) veya [Windows](quickstart.md). IOT Edge arka plan programı, geliştirme makinenizde çalıştırıyorsanız, Visual Studio'da geliştirme başlamadan önce EdgeHub ve EdgeAgent durdurmanız gerekebilir.

### <a name="check-your-tools-version"></a>Araçlar sürümünüzü kontrol edin

1. Gelen **Araçları** menüsünde **Uzantılar ve güncelleştirmeler**. Genişletin **yüklü > Araçları** bulabilirsiniz **Azure IOT Edge araçlarını** ve **Visual Studio için Cloud Explorer**.

1. Yüklü sürüm unutmayın. Bu sürüm Visual Studio Market'te en son sürümle karşılaştırabilirsiniz ([Cloud Explorer](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS), [Azure IOT Edge](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools))

1. Sürümünüzü Visual Studio Market'te kullanılabilir eskiyse, Visual Studio Araçları aşağıdaki bölümde gösterildiği gibi güncelleştirin.

### <a name="update-your-tools"></a>Sizin araçlarınız güncelleştir

1. İçinde **Uzantılar ve güncelleştirmeler** iletişim kutusunda Genişlet **güncelleştirmeleri > Visual Studio Market**seçin **Azure IOT Edge araçlarını** veya **görsel için Cloud Explorer Studio** seçip **güncelleştirme**.

1. Araçları güncelleştirme yüklendikten sonra Visual Studio Araçları VSIX Yükleyicisi'ni kullanarak güncelleştirme tetikleyiciye kapatın.

1. Yükleyicide **Tamam** başlatmak için ve ardından **Değiştir** araçları güncelleştirmek için.

1. Güncelleştirme tamamlandıktan sonra seçin **Kapat** ve Visual Studio'yu yeniden başlatın.

### <a name="create-an-azure-iot-edge-project"></a>Azure IOT Edge projesi oluşturma

Visual Studio'da Azure IOT Edge proje şablonu, Azure IOT hub'ı Azure IOT Edge cihazlarına dağıtılabilir bir proje oluşturur. İlk olarak, Azure IOT Edge çözümünü oluşturun ve ardından ilk Modül içindeki çözümü oluşturun. Her IOT Edge çözüm, birden fazla modülü içerebilir.

1. Visual Studio'da **Dosya** menüsünden **Yeni** > **Proje**’yi seçin.

1. İçinde **yeni proje** iletişim kutusunda **yüklü**seçin **Azure IOT**seçin **Azure IOT Edge**, projeniz için bir ad girin ve konumu belirtin ve ardından **Tamam**. Varsayılan proje adı **AzureIoTEdgeApp1**.

   ![Yeni Proje](./media/how-to-visual-studio-develop-csharp-module/create-new.jpg)

1. İçinde **IOT Edge uygulama ekleyin ve modülün** penceresinde **Linux Amd64**, **Windows Amd64**, veya her ikisi de olarak uygulama platformu. Her ikisi de seçerseniz, varsayılan kod modülü başvurusu her iki proje ile bir çözüm oluşturun.

   > [!TIP]
   > Visual Studio için Azure IOT Edge uzantısı, ARM platformu için proje oluşturma şu anda desteklemiyor. Bkz. Bu [IOT Geliştirici blog girişine](https://devblogs.microsoft.com/iotdev/easily-build-and-debug-iot-edge-modules-on-your-remote-device-with-azure-iot-edge-for-vs-code-1-9-0/) ARM32v7/armhf için bir çözüm geliştirmek için Visual Studio Code'u kullanma örneği için.

1. Şunlardan birini seçin  **C# Modülü** veya **C Modülü** ve modül görüntü deposuna ve modül adı belirtin. Visual Studio autopopulates modül adı ile **localhost:5000 / <, modül adı\>**. Kayıt defteri kendi bilgilerinizle değiştirin. Yerel bir Docker kayıt defteri test, ardından kullanıyorsanız **localhost** bir sakınca yoktur. Azure Container Registry kullanırsanız, oturum açma sunucusu defterinizin ayarlarından'ni kullanın. Oturum açma sunucusu benzer ***\<kayıt defteri adı\>*. azurecr.io**. Yalnızca değiştirmek **localhost:5000** nihai sonucu şu şekilde görünür, böylece dize parçası **\<* kayıt defteri adı*\>.azurecr.io/* \<, modül adı\>***. Varsayılan modül adı **IoTEdgeModule1**

1. Seçin **Tamam** kullanan bir modül ile Azure IOT Edge çözümü oluşturmak için C# veya C.

Artık, bir **AzureIoTEdgeApp1.Linux.Amd64** proje ya da bir **AzureIoTEdgeApp1.Windows.Amd64** proje ya da her ikisi de ve ayrıca bir **IoTEdgeModule1** projesi, Çözüm. Her **AzureIoTEdgeApp1** proje içeren bir `deployment.template.json` istediğiniz derlemek ve dağıtmak için IOT Edge çözümünüz için modülleri tanımlar ve ayrıca modüller arasındaki yolları tanımlar, dosya. Varsayılan çözümü olan bir **tempSensor** modülü ve **IoTEdgeModule1** modülü. **TempSensor** modülü sanal veri oluşturur **IoTEdgeModule1** modülü, varsayılan kodunda çalışırken **IoTEdgeModule1** modülü doğrudan alınan kanallar Azure IOT hub'ına iletileri.

**IoTEdgeModule1** bir .NET Core 2.1 konsol uygulaması projesidir. Bu kapsayıcı Windows veya Linux kapsayıcısı ile çalışan IOT Edge cihazınız için gereksinim duyduğunuz gerekli Docker dosyaları içerir. `module.json` Dosya bir modül meta verilerini açıklar. Azure IOT cihaz SDK'sı bağımlılık olarak alan, gerçek modülü kod bulunan `Program.cs` veya `main.c` dosya.

## <a name="develop-your-module"></a>Modülü geliştirme

Çözümünüzle birlikte gelen varsayılan modülü kodu şu konumdadır **IoTEdgeModule1** > **Program.cs** (için C#) veya **main.c** (C). Modül ve `deployment.template.json` dosya ayarlanır böylece Çözümü derleyin, kapsayıcı kayıt defterinize itme ve herhangi bir kod dokunmadan testi başlatmak için bir cihaza dağıtın. Modül bir kaynaktan giriş yararlanmak için oluşturulan (Bu durumda, **tempSensor** veri benzetimi gerçekleştiren Modülü) ve Azure IOT Hub'ına kanal.

Kendi kodunuzu ile modülü şablonu özelleştirmek hazır olduğunuzda kullanın [Azure IOT Hub SDK'ları](../iot-hub/iot-hub-devguide-sdks.md) anahtar gereken güvenlik, cihaz yönetimi ve güvenilirlik gibi IOT çözümleri için bu adrese modüller oluşturmak için.

## <a name="initialize-iotedgehubdev-with-iot-edge-device-connection-string"></a>IOT Edge cihaz bağlantı dizesiyle iotedgehubdev Başlat

1. Herhangi bir IOT Edge CİHAZDAN bağlantı dizesini kopyalayın **PRIMARY CONNECTION Strıng'i** Visual Studio bulut Gezgini'nde. IOT Edge cihazı simgesini kenar-olmayan cihaz simgesinden farklı olduğu gibi bir kenar-olmayan cihaz bağlantı dizesi değil kopyaladığınızdan emin olun.

   ![Edge cihaz bağlantı dizesini kopyalayın](./media/how-to-visual-studio-develop-csharp-module/copy-edge-conn-string.png)

1. Sağ **AzureIoTEdgeApp1** proje ve ardından **Edge cihaz bağlantı dizesini ayarlamak** Azure IOT Edge Kurulum penceresini açın.

   ![Küme kenar bağlantı dizesi penceresini açın](./media/how-to-visual-studio-develop-csharp-module/set-edge-conn-string.png)

1. İlk adımda bağlantı dizesini girin ve ardından **Tamam**.

> [!NOTE]
> Yalnızca geliştirme bilgisayarınızda sonuçları otomatik olarak sonraki tüm Azure IOT Edge çözümlerini uygulanır sonra şu adımları izlemesi gerekir. Farklı bir bağlantı dizesi değiştirmeniz gerekirse bu yordamı yeniden izlenebilir.

## <a name="build-and-debug-single-module"></a>Tek bir modülün hatalarını ayıklamak ve derleme

Genellikle, test ve birden çok modülle bütün bir çözüm içerisinde çalıştırmadan önce her modülü hata ayıklama isteyebilirsiniz.

1. Sağ **IoTEdgeModule1** seçip **başlangıç projesi olarak ayarla** bağlam menüsünden.

   ![Başlangıç projesi ayarlama](./media/how-to-visual-studio-develop-csharp-module/module-start-up-project.png)

1. Tuşuna **F5** veya modül çalıştırmak için aşağıdaki düğmeye tıklayın; 10 sürebilir&ndash;20 saniye ilk kez bunu.

   ![Modül çalıştırın](./media/how-to-visual-studio-develop-csharp-module/run-module.png)

1. Bir .NET Core konsol uygulaması modülü başarıyla başlatılmışsa Başlat görmeniz gerekir.

   ![Çalışan Modülü](./media/how-to-visual-studio-develop-csharp-module/single-module-run.png)

1. İçinde geliştiriyorsanız C#, bir kesim noktası `PipeMessage()` işlevi **Program.cs**; C kullanarak ayarlarsanız bir kesme noktası `InputQueue1Callback()` işlevi **main.c**. Ardından aşağıdaki komutu çalıştırarak bir ileti göndererek sınayabilirsiniz bir **Git Bash** veya **WSL Bash** Kabuğu. (Çalıştıramazsınız `curl` komutu PowerShell veya komut istemi.)

    ```bash
    curl --header "Content-Type: application/json" --request POST --data '{"inputName": "input1","data":"hello world"}' http://localhost:53000/api/v1/messages
    ```

   ![Tek bir modülün hatalarını ayıklamak](./media/how-to-visual-studio-develop-csharp-module/debug-single-module.png)

    Kesme noktasını tetikledi. Visual Studio değişkenlerini izlediğiniz **Yereller** penceresi.

   > [!TIP]
   > Ayrıca [PostMan](https://www.getpostman.com/) veya yerine ileti göndermek için API araçlara `curl`.

1. Tuşuna **Ctrl + F5 tuşlarına basarak** veya hata ayıklamayı durdurmak için Durdur düğmesini tıklatın.

## <a name="build-and-debug-iot-edge-solution-with-multiple-modules"></a>Derleme ve IOT Edge çözüm birden çok modül ile hata ayıklama

Bitirdikten sonra tek bir modüle geliştirmek, çalıştırın ve tüm bir çözümü birden çok modül ile hata ayıklamak isteyebilirsiniz.

1. İkinci bir modül çözüme sağ tıklayarak ekleyin **AzureIoTEdgeApp1** seçerek **Ekle** > **yeni IOT Edge Modülü**. İkinci modül varsayılan adıdır **IoTEdgeModule2** ve başka bir kanal modülün davranacak.

1. Dosyayı açmak `deployment.template.json` göreceksiniz **IoTEdgeModule2** içinde eklenen **modülleri** bölümü. Değiştirin **yollar** aşağıdaki bölümü. Modül adlarınızı özelleştirdiyseniz, bu adlar eşleşecek şekilde güncelleştirme emin olun.

    ```json
        "routes": {
          "IoTEdgeModule1ToIoTHub": "FROM /messages/modules/IoTEdgeModule1/outputs/* INTO $upstream",
          "sensorToIoTEdgeModule1": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/IoTEdgeModule1/inputs/input1\")",
          "IoTEdgeModule2ToIoTHub": "FROM /messages/modules/IoTEdgeModule2/outputs/* INTO $upstream",
          "sensorToIoTEdgeModule2": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/IoTEdgeModule2/inputs/input1\")"
        },
    ```

1. Sağ **AzureIoTEdgeApp1** seçip **başlangıç projesi olarak ayarla** bağlam menüsünden.

1. Kesme noktalarınız oluşturun ve tuşuna **F5** çalıştırın ve birden çok modülle aynı anda hata ayıklamak için. Birden çok .NET Core konsol uygulaması windows, farklı bir modülü temsil eden her hangi bir pencere görmeniz gerekir.

   ![Birden çok modül hata ayıklama](./media/how-to-visual-studio-develop-csharp-module/debug-multiple-modules.png)

1. Tuşuna **Ctrl + F5 tuşlarına basarak** veya hata ayıklamayı durdurmak için Durdur düğmesini seçin.

## <a name="build-and-push-images"></a>Derleme ve görüntüleri gönderme

1. Emin **AzureIoTEdgeApp1** başlangıç projedir. Şunlardan birini seçin **hata ayıklama** veya **yayın** olarak, modül görüntüleri için oluşturma yapılandırma.

    > [!NOTE]
    > Seçerken **hata ayıklama**, Visual Studio kullanan `Dockerfile.(amd64|windows-amd64).debug` Docker görüntülerinizi oluşturmak için. Bu .NET Core komut satırı hata ayıklayıcı VSDBG kapsayıcı görüntünüzü oluşturma sırasında içerir. Kullanmanızı tavsiye ederiz üretime hazır IOT Edge modülleri için **yayın** kullanan yapılandırma `Dockerfile.(amd64|windows-amd64)` VSDBG olmadan.

1. Azure Container Registry gibi özel bir kayıt defteri kullanıyorsanız, oturum açmak için aşağıdaki Docker komutu kullanın. Yerel kayıt defteri kullanıyorsanız, yapabilecekleriniz [yerel bir kayıt defteri çalıştırma](https://docs.docker.com/registry/deploying/#run-a-local-registry).

    ```cmd
    docker login -u <ACR username> -p <ACR password> <ACR login server>
    ```

1. Azure Container Registry gibi özel bir kayıt defteri kullanıyorsanız, çalışma zamanı ayarları dosyasında bulunan kayıt defteri oturum açma bilgilerini eklemeniz gerekir `deployment.template.json`. Yer tutucuları gerçek ACR yönetici kullanıcı adı, parola ve kayıt defteri adınızla değiştirin.

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

1. Sağ **AzureIoTEdgeApp1** seçip **derleme ve uç çözümü anında iletme** oluşturup her bir modül için Docker görüntüsünü gönderme.

   ![Derleme ve görüntüleri gönderme](./media/how-to-visual-studio-develop-csharp-module/build-and-push.png)

## <a name="deploy-the-solution"></a>Çözümü dağıtma

IoT Edge cihazınızı ayarlamak için kullandığınız hızlı başlangıç makalesinde Azure portalı kullanarak bir modül dağıttınız. Modüller için Visual Studio Cloud Explorer'ı kullanarak da dağıtabilirsiniz. Senaryonuz için hazırlanmış bir dağıtım bildirimi zaten `deployment.json` dosyasını ve tüm yapmanız gereken dağıtım almak için bir cihaz seçin.

1. Açık **Cloud Explorer** tıklayarak **görünümü** > **Cloud Explorer**. Visual Studio 2017'ye açtınız emin olun.

1. İçinde **Cloud Explorer**, aboneliğinizi ve Azure IOT Hub'ınıza ve dağıtmak istediğiniz Azure IOT Edge cihazı bulun.

1. Bir dağıtım oluşturmak için IOT Edge cihazı sağ tıklayın, altında dağıtım bildirimi dosyasını seçmek gereken `$AzureIoTEdgeAppSolutionDir\config\deployment.(amd64|amd64.debug|windows-amd64).json`.

   > [!NOTE]
   > Seçmelisiniz değil `$AzureIoTEdgeAppSolutionDir\config\deployment_for_local_debug.json`

1. İle birlikte çalışan yeni modüller görmek için yenile düğmesine tıklamanız **TempSensor** modülü ve **$edgeAgent** ve **$edgeHub**.

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

1. Belirli bir cihaz için D2C iletisini izlemek için listeden cihazı seçin ve ardından **izleme D2C iletileri Başlat** içinde **eylem** penceresi.

1. Verileri izlemeyi durdurmak için listeden cihazı seçin ve ardından **izleme D2C iletileri Durdur** içinde **eylem** penceresi.

## <a name="next-steps"></a>Sonraki adımlar

IOT Edge cihazlarınız için özel modüller geliştirmek için [kavrama ve kullanma Azure IOT Hub SDK'ları](../iot-hub/iot-hub-devguide-sdks.md).
