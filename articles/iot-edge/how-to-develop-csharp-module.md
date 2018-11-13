---
title: C# modülleri, Azure IOT Edge için hata ayıklama | Microsoft Docs
description: Geliştirme, oluşturma ve Azure IOT Edge için bir C# modülü hata ayıklamak için Visual Studio Code kullanın
services: iot-edge
keywords: ''
author: shizn
manager: philmea
ms.author: xshi
ms.date: 09/27/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: abfd65920348bd51a9923d0a7c74f0f980a01540
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51567834"
---
# <a name="use-visual-studio-code-to-develop-and-debug-c-modules-for-azure-iot-edge"></a>Geliştirme ve C# modülleri, Azure IOT Edge için hata ayıklama için Visual Studio Code'u kullanın

İçin Azure IOT Edge modülleri, iş mantığınızı kapatabilirsiniz. Bu makalede Visual Studio Code (VS Code) ana aracı olarak geliştirme ve modüller C# hata ayıklama için nasıl kullanılacağını gösterir.

## <a name="prerequisites"></a>Önkoşullar

Bir bilgisayar veya geliştirme makinenize Windows, macOS veya Linux çalıştıran bir sanal makine kullanabilirsiniz. IOT Edge cihazı başka bir fiziksel cihaz olabilir.

VS Code, C# modülün hatalarını ayıklamak için iki yolu vardır. Modül kod hata ayıklama modunda başlatmak için başka bir yolu, bir modül kapsayıcıdaki bir işlem iliştirmek için bir yoludur. Visual Studio Code ile hata ayıklama özelliklerine aşina değilseniz okuyun [hata ayıklama](https://code.visualstudio.com/Docs/editor/debugging).

Lütfen Visual Studio Code önce yükledikten sonra aşağıdaki gerekli uzantıları ekleyin:
* [Visual Studio Code](https://code.visualstudio.com/) 
* [Azure IOT Edge uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [C# uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 
* [Docker uzantısı](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)

Bir modülü oluşturmak için Docker modülü görüntüsü ve modülü görüntüsü tutmak için bir kapsayıcı kayıt defteri oluşturmak için proje klasörünü oluşturmak için .NET gerekir:
* [.NET Core 2.1 SDK'sı](https://www.microsoft.com/net/download).
* [Docker Community Edition](https://docs.docker.com/install/) geliştirme makinenizde. 
* [Azure kapsayıcı kayıt defteri](https://docs.microsoft.com/azure/container-registry/) veya [Docker hub'ı](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)
   * Prototip ve test etme amacıyla bir bulut kayıt defteri yerine yerel bir Docker kayıt defteri kullanabilirsiniz. 

Yerel kurulumu için hata ayıklamak için geliştirme ortamı ve IOT Edge çözümünüzü test çalıştırmak, gereksinim duyduğunuz [Azure IOT EdgeHub geliştirme aracı](https://pypi.org/project/iotedgehubdev/). Yükleme [(2.7/3.6) Python ve Pip](https://www.python.org/). Yüklemeyi **iotedgehubdev** aşağıdaki komutu, terminalde çalıştırarak.

   ```cmd
   pip install --upgrade iotedgehubdev
   ```

Modülünüzün bir cihazda test etmek için oluşturulan en az bir IOT Edge cihaz Kimliğine sahip etkin bir IOT hub gerekir. IOT Edge arka plan programı geliştirme makinesinde çalıştırıyorsanız, sonraki adıma geçmeden önce EdgeHub ve EdgeAgent durdurmanız gerekebilir. 

## <a name="create-a-new-solution-with-c-module"></a>C# modülü ile yeni bir çözüm oluşturma

Visual Studio Code ve Azure IOT Edge uzantısını kullanarak .NET Core 2.1 üzerinde dayalı bir IOT Edge modülünüzü oluşturmak için bu adımları uygulayın. İlk olarak, bir çözüm oluşturun ve ardından ilk Modül içindeki çözümü oluşturun. Her çözüm, birden fazla modülü içerebilir. 

1. Visual Studio Code'da seçin **görünümü** > **tümleşik Terminalini**.
2. Seçin **görünümü** > **komut paleti**. 
3. Komut Paleti'nde girin ve şu komutu çalıştırın **Azure IOT Edge: IOT Edge çözüm yeni**.

   ![Yeni IOT Edge çözümü çalıştırın](./media/how-to-develop-csharp-module/new-solution.png)

4. Yeni çözümü oluşturmak istediğiniz klasöre göz atın. Seçin **klasörü seçin**. 
5. Çözümünüz için bir ad girin. 
6. Seçin **C# Modülü** çözüm içinde ilk modül için şablon olarak.
7. Bir modül için bir ad girin. Kapsayıcı kayıt defterinizde içinde benzersiz bir ad seçin. 
8. Modülün görüntü deposu adını sağlayın. VS Code autopopulates modül adı ile **localhost:5000**. Kayıt defteri kendi bilgilerinizle değiştirin. Yerel bir Docker kayıt defteri test, ardından kullanıyorsanız **localhost** bir sakınca yoktur. Azure Container Registry kullanırsanız, oturum açma sunucusu defterinizin ayarlarından'ni kullanın. Oturum açma sunucusu benzer  **\<kayıt defteri adı\>. azurecr.io**. Dizenin yalnızca localhost bölümünü değiştirin, modülünüzün adını silmeyin.

   ![Docker görüntü deposunu sağlama](./media/how-to-develop-csharp-module/repository.png)

VS Code, sağlanan bir IOT Edge çözümü oluşturur ve ardından yeni bir pencerede yüklenir bilgilerini alır.

   ![IOT Edge çözümünü görüntüleme](./media/how-to-develop-csharp-module/view-solution.png)

Çözüm içinde dört öğe vardır: 

* A **.vscode** klasörü, hata ayıklama yapılandırmaları içerir.
* A **modülleri** klasörü her modül için alt klasörler bulunur. Bu noktada yalnızca biri gerekir. Ancak daha komutuyla komut Paleti'nde ekleyebilirsiniz **Azure IOT Edge: IOT Edge Modülü Ekle**. 
* Bir **.env** dosyası, ortam değişkenleri listeler. Azure Container Registry kaydınız varsa, bir Azure kapsayıcı kayıt defteri kullanıcı adı ve parola bulunması. 

   >[!NOTE]
   >Modül için bir görüntü deposuna sağlarsanız, ortam dosyası yalnızca oluşturulur. Test ve yerel olarak hata ayıklama için localhost Varsayılanları kabul ortam değişkenleri gerekmez. 

* A **deployment.template.json** dosyası listeler, yeni bir örnek modülüyle **tempSensor** veri benzetimi gerçekleştiren modülü test etmek için kullanabilirsiniz. Nasıl iş dağıtım bildirimleri hakkında daha fazla bilgi için bkz. [modülleri dağıtma ve yollar kurmak için dağıtım bildirimleri kullanmayı öğrenin](module-composition.md). 

## <a name="develop-your-module"></a>Modülü geliştirme

Varsayılan C# çözümünüzle birlikte gelen modülü kod adresindedir **modülleri** > ** [modül adı] ** > **Program.cs**. Çözümü derleyin, kapsayıcı kayıt defterinize itme ve herhangi bir kod dokunmadan testi başlatmak için bir aygıta dağıtmak modülü ve deployment.template.json dosya ayarlanır. Modül, yalnızca bir kaynak (Bu durumda, veri benzetimi gerçekleştiren tempSensor Modülü) gelenlerin ve IOT Hub'ına kanal için oluşturulmuştur. 

Kendi kodunuzu ile C# şablonunu özelleştirmek hazır olduğunuzda kullanın [Azure IOT Hub SDK'ları](../iot-hub/iot-hub-devguide-sdks.md) anahtar gereken güvenlik, cihaz yönetimi ve güvenilirlik gibi IOT çözümleri için bu adrese modüller oluşturmak için. 

VS code'da C# desteği, platformlar arası .NET Core geliştirme için optimize edilmiştir. Daha fazla bilgi edinin [VS Code, C# ile nasıl çalışılacağını](https://code.visualstudio.com/docs/languages/csharp).

## <a name="launch-and-debug-module-code-without-container"></a>Başlatma ve kapsayıcı olmadan modül kodu hatalarını ayıklama

IOT Edge C# modülü olan bir.Net Core uygulaması. Ve Azure IOT C# cihaz SDK'sı üzerinde bağlıdır. IOT C# modülü başlatmak ve çalıştırmak ortam ayarları gerektiğinden, varsayılan modülü kodda, başlatma bir **ModuleClient** ortam ayarlar ve giriş adı. Ayrıca göndermek veya iletileri giriş kanallarına yönlendirmek gerekir. Varsayılan C# modülü yalnızca bir giriş kanalı içerir ve ad **input1**.

### <a name="setup-iot-edge-simulator-for-single-module-app"></a>Kurulum IOT Edge modülü tek uygulama simülatörü

1. Simülatörü başlatın ve kurulum için VS Code komut paleti yazıp seçin **Azure IOT Edge: IOT Edge hub'ı simülatörü başlatın tek modülü için**. Ayrıca tek modülü uygulamanızın türü için giriş adlarını gerekir **input1** ve Enter tuşuna basın. Komut tetikleyecek **iotedgehubdev** CLI ve IOT Edge simülatör ve test yardımcı programını modülü kapsayıcı başlatın. Simülatör tek modülü modunda başarıyla başlatıldı tümleşik terminalde aşağıdaki çıktıları görebilirsiniz. Ayrıca bkz bir `curl` yardımcı olmak için komut üzerinden ileti gönder. Daha sonra bu adı kullanacaksınız.

   ![Kurulum IOT Edge modülü tek uygulama simülatörü](media/how-to-develop-csharp-module/start-simulator-for-single-module.png)

   Docker Gezgini taşıyabilir ve çalışma durumu modülüne bakın.

   ![Simülatör Modül durumu](media/how-to-develop-csharp-module/simulator-status.png)

   **EdgeHubDev** yerel IOT Edge simülatör setinin kapsayıcıdır. Bu IOT Edge güvenlik arka plan programı olmadan geliştirme makinenizde çalıştırın ve uygulamanızı yerel modül veya modül kapsayıcılar için ortam ayarları sağlayın. **Giriş** kapsayıcı kullanıma sunulan restAPIs Giriş kanalı, modül hedef köprüsü iletileri yardımcı olmak için.

2. VS Code komut paleti yazın ve **Azure IOT Edge: kullanıcı ayarları için modülü kimlik bilgilerini ayarla** ortam ayarlarına modülü ayarlanacak `azure-iot-edge.EdgeHubConnectionString` ve `azure-iot-edge.EdgeModuleCACertificateFile` kullanıcı ayarları. Bu ortam ayarlarını başvurulan bulabilirsiniz **.vscode** > **launch.json** ve [VS Code kullanıcı ayarları](https://code.visualstudio.com/docs/getstarted/settings).

### <a name="build-module-app-and-debug-in-launch-mode"></a>Modül uygulaması oluşturma ve başlatma modda hata ayıklama

1. Tümleşik terminalde dizinine **CSharpModule** klasörü,.Net Core oluşturmak için aşağıdaki komutu çalıştırın, uygulama.

    ```cmd
    dotnet build
    ```

2. `program.cs` sayfasına gidin. Bu dosyada kesme noktası ekleyin.

3. VS Code hata ayıklama görünümüne gidin: Görünüm > hata ayıklama. Hata ayıklama Yapılandırması **ModuleName yerel hata ayıklama (.NET Core)** açılır listeden. 

  ![VS code'da hata ayıklama moduna gidin](media/how-to-develop-csharp-module/debug-view.png)

4. Tıklayın **hata ayıklamayı Başlat** veya basın **F5**. Hata ayıklama oturumu başlar.

   > [!NOTE]
   > Varsa,.Net Core `TargetFramework` program yolunuzda ile tutarlı değil `launch.json`. Program yolu el ile güncelleştirmeniz gerekir `launch.json` cevaben `TargetFramework` .csproj dosyanızda. Bu nedenle, VS Code Bu program başarıyla başlatabilirsiniz.

5. VS Code tümleşik terminale göndermek için aşağıdaki komutu çalıştırın bir **Hello World** modülünüzde ileti. Önceki adımda gösterilen komut budur zaman IOT Edge simulator'ı başarıyla ayarlandı.

    ```cmd
    curl --header "Content-Type: application/json" --request POST --data '{"inputName": "input1","data":"hello world"}' http://localhost:53000/api/v1/messages
    ```

   > [!NOTE]
   > Windows kullanıyorsanız, VS Code tümleşik Terminalini Kabuk olduğundan emin olarak **Git Bash** veya **WSL Bash**. Çalıştıramazsınız `curl` komutu PowerShell veya komut isteminde. 
   
   > [!TIP]
   > Ayrıca [PostMan](https://www.getpostman.com/) veya yerine üzerinden ileti göndermek için API araçlara `curl`.

6. VS Code hata ayıklama Görünümü'nde sol bölmedeki değişkenleri görürsünüz. 

    ![Değişkenleri izleyin](media/how-to-develop-csharp-module/single-module-variables.png)

7. Hata ayıklama oturumunu durdurmak için Durdur düğmesini veya tuşuna tıklayın **Shift + F5 tuşlarına basarak**. VS Code komut paleti yazın ve seçin **Azure IOT Edge: IOT Edge simülatör Durdur** durdurun ve simülatör temizleyin.

## <a name="build-module-container-for-debugging-and-debug-in-attach-mode"></a>Hata ayıklama ve hata ayıklama için modül kapsayıcı derleme içinde modu ekleme

İki modül varsayılan çözümünüzü içeren sanal sıcaklık algılayıcısı modülü biridir ve diğer C# kanal modülüdür. Sanal sıcaklık algılayıcısı, C# kanal modülü iletileri göndermeye devam eder ve ardından IOT Hub'ına iletileri yöneltilen. Oluşturduğunuz modül klasöründe birkaç Docker dosya için farklı bir kapsayıcı türü vardır. Uzantısıyla biten bu dosyaları dilediğinizi **.debug** test etmek için modülü. Şu anda C# modüller desteği yalnızca Linux amd64 kapsayıcılarında hata ayıklama modu ekleyin.

### <a name="setup-iot-edge-simulator-for-iot-edge-solution"></a>IOT Edge çözüm Kurulum IOT Edge simülatörü

Geliştirme makinenizde, IOT Edge çözümü çalıştırmak için IOT Edge güvenlik daemon yüklemek yerine IOT Edge simülatör başlayabilirsiniz. 

1. Sol taraftaki cihaz Gezgini'nde, sağ tıklayın, IOT Edge cihaz Kimliğine, select **Kurulum IOT Edge simülatör** cihaz bağlantı dizesiyle simülatörü başlatın.

2. IOT Edge simülatör tümleşik terminalde Kurulum başarıyla verildi görebilirsiniz.

### <a name="build-and-run-container-for-debugging-and-debug-in-attach-mode"></a>Derleme ve hata ayıklama ve hata ayıklama için kapsayıcı çalıştırma modu olarak ekleme

1. VS Code'da gidin `deployment.template.json` dosya. Ekleyerek, C# modülü resim URL'si güncelleştirmesi **.debug** sonuna.

   ![Ekleme *** .debug, görüntü adı](./media/how-to-develop-csharp-module/image-debug.png)

2. `program.cs` sayfasına gidin. Bu dosyada kesme noktası ekleyin.

3. VS Code dosya Gezgini'nde seçin `deployment.template.json` bağlam menüsü, çözümünüz için dosyaya tıklayın **simülatör derleme ve çalıştırma IOT Edge çözümde**. Aynı pencerede modülü kapsayıcı günlüklerini izleyebilirsiniz. Docker kapsayıcı durumu izlemek için Gezgini da gidebilirsiniz.

   ![Değişkenleri izleyin](media/how-to-develop-csharp-module/view-log.png)

4. VS Code hata ayıklama görünümüne gidin. Bir modül için hata ayıklama yapılandırma dosyasını seçin. Hata ayıklama seçeneği adı şuna benzer olmalıdır **ModuleName uzaktan hata ayıklama (.NET Core)**

   ![Yapılandırma seçin](media/how-to-develop-csharp-module/debug-config.png)

5. Seçin **hata ayıklamayı Başlat** veya **F5**. Ekleme yapılacak işlem seçin.

6. VS Code hata ayıklama Görünümü'nde sol bölmedeki değişkenleri görürsünüz.

7. Hata ayıklama oturumunu durdurmak için Durdur düğmesini veya tuşuna tıklayın **Shift + F5 tuşlarına basarak**. VS Code komut paleti yazın ve seçin **Azure IOT Edge: IOT Edge simülatör Durdur**.

    > [!NOTE]
    > Bu örnek, .NET Core IOT Edge modülleri kapsayıcılarına hata ayıklamak nasıl gösterir. Hata ayıklama sürümünde temel `Dockerfile.debug`, içeren Visual Studio .NET Core komut satırı hata ayıklayıcı (VSDBG) kapsayıcı görüntünüzü oluşturma sırasında. Doğrudan kullanabilir veya özelleştirebilirsiniz C# modüllerinizi hata ayıklama sonra öneririz `Dockerfile` VSDBG üretime hazır IOT Edge modülleri için olmadan.


## <a name="next-steps"></a>Sonraki adımlar

Modülünüzün oluşturduktan sonra bilgi nasıl [Visual Studio code'dan Azure IOT Edge modüllerini dağıtmak](how-to-deploy-modules-vscode.md).

Cihazlarınızı IOT Edge modülleri geliştirmek için [kavrama ve kullanma Azure IOT Hub SDK'ları](../iot-hub/iot-hub-devguide-sdks.md).
