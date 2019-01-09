---
title: Geliştirme ve hata ayıklama modüller için Azure IOT Edge | Microsoft Docs
description: Geliştirme, derleme ve Azure IOT Edge kullanımı için bir modülün hatalarını ayıklamak için Visual Studio Code kullanın C#, Python, Node.js, Java veya C
services: iot-edge
keywords: ''
author: shizn
manager: philmea
ms.author: xshi
ms.date: 01/04/2019
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 463ab617051bf97bb3b1c38ed431c4b6936a9c90
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2019
ms.locfileid: "54118702"
---
# <a name="use-visual-studio-code-to-develop-and-debug-modules-for-azure-iot-edge"></a>Geliştirme ve modülleri, Azure IOT Edge için hata ayıklama için Visual Studio Code'u kullanın

İçin Azure IOT Edge modülleri, iş mantığınızı kapatabilirsiniz. Bu makalede, Visual Studio Code ana aracı olarak geliştirme ve hata ayıklama modüller için nasıl kullanılacağını gösterir.

## <a name="prerequisites"></a>Önkoşullar

Bir bilgisayar veya geliştirme makinenize Windows, macOS veya Linux çalıştıran bir sanal makine kullanabilirsiniz. IOT Edge cihazı başka bir fiziksel cihaz olabilir.

Yazılmış modüller için C#, Node.js ve Java, Visual Studio Code, modülün hatalarını ayıklamak için iki yolu vardır: Bir modül kapsayıcıdaki bir sürece iliştirilip veya modül kod hata ayıklama modunda başlatın. Python veya C ile mi yazılmış modüller için bunlar yalnızca Linux amd64 kapsayıcıları bir işlemde ekleyerek hata ayıklama gerçekleştirilebilir.

> [!TIP]
> Visual Studio Code ile hata ayıklama özelliklerine aşina değilseniz okuyun [hata ayıklama](https://code.visualstudio.com/Docs/editor/debugging).

Yükleme [Visual Studio Code](https://code.visualstudio.com/) ilk ve ardından aşağıdaki uzantılar ekleyin:

- [Azure IOT araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)
- [Docker uzantısı](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)
- Visual Studio uzantısı, geliştirmekte olduğunuz bir dile özgü:
  - C#, Azure işlevleri dahil olmak üzere: [C# uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
  - Python: [Python uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
  - Java: [Visual Studio Code için Java uzantı paketi](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack)
  - C: [C/C++ uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)

Ayrıca, modülü geliştirme için bazı ek, dile özgü araçları yüklemek gerekecektir:

- C#, Azure işlevleri dahil olmak üzere: [.NET Core 2.1 SDK'sı](https://www.microsoft.com/net/download)

- Python: [Python](https://www.python.org/downloads/) ve [Pip](https://pip.pypa.io/en/stable/installing/#installation) (genellikle Python yüklemenizi dahil) Python paketlerini yükleme. Pip yüklendikten sonra yükleme **Cookiecutter** aşağıdaki komutla paket:

    ```cmd/sh
    pip install --upgrade --user cookiecutter
    ```

- Node.js: [Node.js](https://nodejs.org). Ayrıca yüklemek isteyeceksiniz [Yeoman](https://www.npmjs.com/package/yo) ve [Azure IOT Edge Node.js modül Oluşturucu](https://www.npmjs.com/package/generator-azure-iot-edge-module).

- Java: [Java SE Geliştirme Seti 10](https://aka.ms/azure-jdks) ve [Maven](https://maven.apache.org/). Şunları yapmanız gerekir [ayarlamak `JAVA_HOME` ortam değişkeni](https://docs.oracle.com/cd/E19182-01/820-7851/inst_cli_jdk_javahome_t/) JDK yüklemenize işaret edecek şekilde.

Derleme ve modül görüntünüzü dağıtmak için modülü görüntüsü ve modülü görüntüsü tutmak için bir kapsayıcı kayıt defteri oluşturmak için Docker gerekir:

- [Docker Community Edition](https://docs.docker.com/install/) geliştirme makinenizde.

- [Azure kapsayıcı kayıt defteri](https://docs.microsoft.com/azure/container-registry/) veya [Docker hub'ı](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)

    > [!TIP]
    > Prototip ve test etme amacıyla bir bulut kayıt defteri yerine yerel bir Docker kayıt defteri kullanabilirsiniz.

C modülünüzde geliştirmekte olduğunuz sürece Python tabanlı etmeniz [Azure IOT EdgeHub geliştirme aracı](https://pypi.org/project/iotedgehubdev/) hata ayıklamak için yerel geliştirme ortamınızı ayarlama hakkında bilgi için çalıştırın ve IOT Edge çözümünüzü test edin. Zaten yapmadıysanız, yükleme [(2.7/3.6) Python ve Pip](https://www.python.org/) ve yüklemeyi **iotedgehubdev** terminalinizde şu komutu çalıştırarak.

   ```cmd
   pip install --upgrade iotedgehubdev
   ```

> [!NOTE]
> Modülünüzün bir cihazda test etmek için etkin bir IOT hub ile en az bir IOT Edge cihazı gerekir. Bilgisayarınızı bir IOT Edge cihazı kullanmak için hızlı başlangıç için adımları izleyin. [Linux](quickstart-linux.md) veya [Windows](quickstart.md). IOT Edge arka plan programı, geliştirme makinenizde çalıştırıyorsanız, sonraki adıma geçmeden önce EdgeHub ve EdgeAgent durdurmanız gerekebilir.

## <a name="create-a-new-solution-template"></a>Yeni bir çözüm şablonu oluşturma

Aşağıdaki adımlar, tercih ettiğiniz geliştirme dilinde bir IOT Edge modülü oluşturma işlemi gösterilmektedir (Azure işlevleri, yazılan dahil olmak üzere C#) Visual Studio Code ve Azure IOT araçları kullanarak. Bir çözüm oluşturup sonra bu çözümde ilk modülü oluşturma başlatın. Her çözüm, birden çok modül içerebilir.

1. Seçin **görünümü** > **komut paleti**.

1. Komut Paleti'nde girin ve şu komutu çalıştırın **Azure IOT Edge: Yeni bir IOT Edge çözüm**.

   ![Yeni IOT Edge çözümü çalıştırın](./media/how-to-develop-csharp-module/new-solution.png)

1. Yeni çözüm oluşturma ve ardından istediğiniz klasöre göz atın **klasörü seçin**.

1. Çözümünüz için bir ad girin.

1. Bir çözümdeki ilk modülü gibi tercih ettiğiniz geliştirme Dil modülü şablonu seçin.

1. Bir modül için bir ad girin. Kapsayıcı kayıt defterinizde içinde benzersiz bir ad seçin.

1. Modülün görüntü deposu adını sağlayın. Visual Studio Code autopopulates modül adı ile **localhost:5000 / <, modül adı\>**. Kayıt defteri kendi bilgilerinizle değiştirin. Yerel bir Docker kayıt defteri test, ardından kullanıyorsanız **localhost** bir sakınca yoktur. Azure Container Registry kullanırsanız, oturum açma sunucusu defterinizin ayarlarından'ni kullanın. Oturum açma sunucusu benzer ***\<kayıt defteri adı\>*. azurecr.io**. Yalnızca değiştirmek **localhost:5000** nihai sonucu şu şekilde görünür, böylece dize parçası **\<* kayıt defteri adı*\>.azurecr.io/* \<, modül adı\>***.

   ![Docker görüntü deposunu sağlama](./media/how-to-develop-csharp-module/repository.png)

Visual Studio Code, sağlanan bir IOT Edge çözümü oluşturur ve ardından yeni bir pencerede yüklenir bilgilerini alır.

Çözüm içinde dört öğe vardır:

- A **.vscode** klasörü, hata ayıklama yapılandırmaları içerir.

- A **modülleri** klasörü her modül için alt klasörler bulunur. Bu noktada yalnızca biri gerekir. Ancak daha komutuyla komut Paleti'nde ekleyebilirsiniz **Azure IOT Edge: IOT Edge Modülü Ekle**.

- Bir **.env** dosyası, ortam değişkenleri listeler. Azure Container Registry kaydınız varsa, bir Azure kapsayıcı kayıt defteri kullanıcı adı ve parola bulunması.

  > [!NOTE]
  > Modül için bir görüntü deposuna sağlarsanız, ortam dosyası yalnızca oluşturulur. Test ve yerel olarak hata ayıklama için localhost Varsayılanları kabul ortam değişkenleri gerekmez.

- A **deployment.template.json** dosyası listeler, yeni bir örnek modülüyle **tempSensor** veri benzetimi gerçekleştiren modülü test etmek için kullanabilirsiniz. Nasıl iş dağıtım bildirimleri hakkında daha fazla bilgi için bkz. [modülleri dağıtma ve yollar kurmak için dağıtım bildirimleri kullanmayı öğrenin](module-composition.md).

## <a name="add-additional-modules"></a>Ek modüller ekleme

Ek modüller çözümünüze eklemek için komutu çalıştırmak **Azure IOT Edge: IOT Edge Modülü Ekle** komut paletinden. Ayrıca sağ **modülleri** klasör veya `deployment.template.json` Visual Studio kod Gezgini görünümü'nde dosya ve ardından **IOT Edge Modülü Ekle**.

## <a name="develop-your-module"></a>Modülü geliştirme

Çözümünüzle birlikte gelen varsayılan modülü kodu şu konumda bulunur:

- Azure işlevi (C#): **modülleri >  *&lt;, modül adı&gt;* > *&lt;, modül adı&gt;*.cs**
- C#: **modülleri > *&lt;, modül adı&gt;* > Program.cs**
- Python: **modülleri > *&lt;, modül adı&gt;* > main.py**
- Node.js: **modülleri > *&lt;, modül adı&gt;* > app.js**
- Java: **modülleri > *&lt;, modül adı&gt;* > src > ana > java > com > edgemodulemodules > App.java**
- C: **modülleri > *&lt;, modül adı&gt;* > main.c**

Çözümü derleyin, kapsayıcı kayıt defterinize itme ve herhangi bir kod dokunmadan testi başlatmak için bir aygıta dağıtmak modülü ve deployment.template.json dosya ayarlanır. Modül, yalnızca bir kaynak (Bu durumda, veri benzetimi gerçekleştiren tempSensor Modülü) gelenlerin ve IOT Hub'ına kanal için oluşturulmuştur.

Kendi kodunuzu ile şablonu özelleştirmek hazır olduğunuzda kullanın [Azure IOT Hub SDK'ları](../iot-hub/iot-hub-devguide-sdks.md) anahtar gereken güvenlik, cihaz yönetimi ve güvenilirlik gibi IOT çözümleri için bu adrese modüller oluşturmak için.

## <a name="debug-a-module-without-a-container-c-nodejs-java"></a>Bir kapsayıcı olmadan bir modülün hatalarını ayıklamak (C#, Node.js, Java)

İçinde geliştiriyorsanız C#, Node.js ve Java, modülünüzde kullanılmasını gerektiren bir **ModuleClient** , Başlat, çalıştırın ve rota, iletileri varsayılan modülü kodda nesne. Varsayılan Giriş kanalı da kullanacağınız **input1** modülü iletiler geldiğinde işlem.

### <a name="set-up-iot-edge-simulator-for-iot-edge-solution"></a>IOT Edge çözüm IOT Edge simülatörü ayarlama

Geliştirme makinenizde IOT Edge çözümünüzü çalıştırabileceğiniz bir IOT Edge güvenlik arka plan programı yüklemek yerine bir IOT Edge simülatör başlayabilirsiniz.

1. Cihaz Gezgini sol tarafta, IOT Edge cihazı Kimliğinizi sağ tıklayın ve ardından **Kurulum IOT Edge simülatör** cihaz bağlantı dizesiyle simülatörü başlatın.
1. IOT Edge simülatör başarıyla tümleşik terminalde ilerleme ayrıntıları okuyarak ayarlandığını gösterdiğinde görebilirsiniz.

### <a name="set-up-iot-edge-simulator-for-single-module-app"></a>IOT Edge modülü tek uygulama simülatörü ayarlama

Ayarlama ve simülatör başlangıç için komutu çalıştırın **Azure IOT Edge: IOT Edge hub'ı simülatör tek modülü için başlangıç** Visual Studio Code komut paletinden. İstendiğinde, değerini kullanın **input1** uygulamanız için giriş adı olarak varsayılan modülü kodu (veya eşdeğeri değer kodunuzdan). Komut Tetikleyiciler **iotedgehubdev** CLI ve IOT Edge simülatör'ü ve test yardımcı programını modülü kapsayıcı başlatır. Simülatör tek modülü modunda başarıyla başlatıldı tümleşik terminalde aşağıdaki çıktıları görebilirsiniz. Ayrıca bkz bir `curl` yardımcı olmak için komut üzerinden ileti gönder. Daha sonra bu adı kullanacaksınız.

   ![IOT Edge modülü tek uygulama simülatörü ayarlama](media/how-to-develop-csharp-module/start-simulator-for-single-module.png)

   Modülün çalıştırma durumunu görmek için Visual Studio Code'da Docker Gezgini görünümünü kullanın.

   ![Simülatör Modül durumu](media/how-to-develop-csharp-module/simulator-status.png)

   **EdgeHubDev** yerel IOT Edge simülatör setinin kapsayıcıdır. Geliştirme makinenizde IOT Edge güvenlik daemon olmadan çalıştırabilirsiniz ve uygulamanızı yerel modül veya modül kapsayıcılar için ortam ayarları sağlar. **Giriş** kapsayıcı modülünüzde Giriş kanalı hedef köprüsü iletileri yardımcı olmak için REST API'lerini kullanıma sunar.

### <a name="debug-module-in-launch-mode"></a>Modül başlatma modda hata ayıklama

1. Geliştirme dilini gereksinimlerine göre hata ayıklama için ortamınızı hazırlama, modülünüzde bir kesme noktası ayarlayın ve kullanmak için hata ayıklama yapılandırmasını seçin:
   - **C#**
     - Visual Studio Code tümleşik terminale dizinine ***&lt;, modül adı&gt;*** uygulama klasörü ve.Net Core oluşturmak için aşağıdaki komutu çalıştırın.

       ```cmd
       dotnet build
       ```

     - Dosyayı açmak `program.cs` ve bir kesme noktası ekleyin.

     - Visual Studio kod hatalarını görünümü seçerek gidin **Görüntüle > hata ayıklama**. Hata ayıklama Yapılandırması  ***&lt;, modül adı&gt;* yerel hata ayıklama (.NET Core)** açılır listeden.

        > [!NOTE]
        > Varsa,.Net Core `TargetFramework` program yolunuzda ile tutarlı değil `launch.json`, program yolu el ile güncelleştirmeniz gerekecektir `launch.json` eşleştirilecek `TargetFramework` .csproj dosyanızda, Visual Studio Code başarıyla bu başlatabilirsiniz şekilde Program.

   - **Node.js**
     - Visual Studio Code tümleşik terminale dizinine ***&lt;, modül adı&gt;*** klasörü ve düğüm paketleri yüklemek için aşağıdaki komutu çalıştırın

       ```cmd
       npm install
       ```

     - Dosyayı açmak `app.js` ve bir kesme noktası ekleyin.

     - Visual Studio kod hatalarını görünümü seçerek gidin **Görüntüle > hata ayıklama**. Hata ayıklama Yapılandırması  ***&lt;, modül adı&gt;* yerel hata ayıklama (Node.js)** açılır listeden.
   - **Java**
     - Dosyayı açmak `App.java` ve bir kesme noktası ekleyin.

     - Visual Studio kod hatalarını görünümü seçerek gidin **Görüntüle > hata ayıklama**. Hata ayıklama Yapılandırması  ***&lt;, modül adı&gt;* yerel hata ayıklama (Java)** açılır listeden.

1. Tıklayın **hata ayıklamayı Başlat** veya basın **F5** hata ayıklama oturumu başlatmak için.

1. Visual Studio Code tümleşik terminale göndermek için aşağıdaki komutu çalıştırın bir **Hello World** modülünüzde ileti. Bu, IOT Edge simülatör'ü ayarladığınızda, önceki adımlarda gösterildiği komuttur.

    ```bash
    curl --header "Content-Type: application/json" --request POST --data '{"inputName": "input1","data":"hello world"}' http://localhost:53000/api/v1/messages
    ```

   > [!NOTE]
   > Windows kullanıyorsanız, Visual Studio Code tümleşik Terminalini Kabuk olduğundan emin olarak **Git Bash** veya **WSL Bash**. Çalıştıramazsınız `curl` PowerShell ya da komut isteminden komutu.
   > [!TIP]
   > Ayrıca [PostMan](https://www.getpostman.com/) veya yerine üzerinden ileti göndermek için API araçlara `curl`.

1. Visual Studio kod hatalarını görünümünde değişkenleri sol bölmesinde görürsünüz.

1. Hata ayıklama oturumunu durdurmak için Durdur düğmesini veya ENTER tuşuna seçin **Shift + F5 tuşlarına basarak**ve ardından çalıştırın **Azure IOT Edge: IOT Edge simülatör Durdur** simülatör durdurmak ve temizlemek için komut Paleti'nde.

## <a name="debug-in-attach-mode-with-iot-edge-simulator-c-nodejs-java-azure-functions"></a>Hata ayıklama modu ile IOT Edge simülatör ekleme (C#, Node.js, Java, Azure işlevleri)

İki modül varsayılan çözümünüzü içeren, bir sanal sıcaklık algılayıcısı modül biridir ve diğer kanal modülüdür. Sanal sıcaklık algılayıcısı kanal modülü iletileri gönderir ve sonra iletileri IOT Hub'ına yöneltilen. Oluşturduğunuz modül klasöründe birkaç Docker dosya için farklı bir kapsayıcı türü vardır. Uzantısıyla biten dosyaları dilediğinizi **.debug** test etmek için modülü.

Şu anda, ekleme, hata ayıklamayı modu, yalnızca aşağıdaki gibi desteklenir:

- C#Modüller, Azure işlevleri için dahil olmak üzere Linux amd64 kapsayıcılarında hata ayıklama desteği
- Node.js modüllerini Linux amd64 ve arm32v7 kapsayıcılar ve Windows amd64 kapsayıcıları hata ayıklama desteği
- Java modülleri Linux amd64 ve arm32v7 kapsayıcılarında hata ayıklama desteği

> [!TIP]
> Varsayılan platformu seçenekleri arasından, IOT Edge çözümünüz için Visual Studio Code durum çubuğunda bir öğeye tıklayarak geçiş yapabilirsiniz.

### <a name="set-up-iot-edge-simulator-for-iot-edge-solution"></a>IOT Edge çözüm IOT Edge simülatörü ayarlama

Geliştirme makinenizde IOT Edge çözümünüzü çalıştırabileceğiniz bir IOT Edge güvenlik arka plan programı yüklemek yerine bir IOT Edge simülatör başlayabilirsiniz.

1. Cihaz Gezgini sol tarafta, IOT Edge cihazı Kimliğinizi sağ tıklayın ve ardından **Kurulum IOT Edge simülatör** cihaz bağlantı dizesiyle simülatörü başlatın.

1. IOT Edge simülatör başarıyla tümleşik terminalde ilerleme ayrıntıları okuyarak ayarlandığını gösterdiğinde görebilirsiniz.

### <a name="build-and-run-container-for-debugging-and-debug-in-attach-mode"></a>Derleme ve hata ayıklama ve hata ayıklama için kapsayıcı çalıştırma modu olarak ekleme

1. Modül dosyanızı açın (`program.cs`, `app.js`, `App.java`, veya `<your module name>.cs`) ve bir kesme noktası ekleyin.

1. Visual Studio kod Gezgini görünümü'nde sağ `deployment.debug.template.json` çözümünüz için dosya ve ardından **simülatör derleme ve çalıştırma IOT Edge çözümde**. Aynı pencerede modülü kapsayıcı günlüklerini izleyebilirsiniz. Kapsayıcı durumu izlemek için Docker görünümüne da gidebilirsiniz.

   ![Değişkenleri izleyin](media/how-to-develop-csharp-module/view-log.png)

1. Visual Studio kod hatalarını görünümüne gidin ve kendi modül için hata ayıklama yapılandırma dosyasını seçin. Hata ayıklama seçeneği adı şuna benzer olmalıdır  ***&lt;, modül adı&gt;* uzaktan hata ayıklama**

1. Seçin **hata ayıklamayı Başlat** veya basın **F5**. Ekleme yapılacak işlem seçin.

1. Visual Studio kod hatalarını Görünümü'nde sol bölmedeki değişkenleri görürsünüz.

1. Hata ayıklama oturumunu durdurmak için önce Durdur düğmesini veya ENTER tuşuna seçin **Shift + F5 tuşlarına basarak**ve ardından **Azure IOT Edge: IOT Edge simülatör Durdur** komut paletinden.

> [!NOTE]
> Yukarıdaki örnekte, IOT Edge modülleri kapsayıcılarına hata ayıklamak gösterilmektedir. Kullanıma sunulan bağlantı noktaları, modülün kapsayıcıya eklemiş `createOptions` ayarları. Hata ayıklama modüllerinizi tamamladıktan sonra üretime hazır IOT Edge modülleri için kullanıma sunulan bu bağlantı noktalarını kaldırmanız önerilir.
>
> Yazılmış modüller için C#, Azure işlevleri dahil olmak üzere, bu örnekte hata ayıklama sürümünde dayanır `Dockerfile.amd64.debug`, içeren .NET Core komut satırı hata ayıklayıcı (VSDBG) kapsayıcı görüntünüzü oluşturma sırasında. Hata ayıklama sonra C# modülleri, doğrudan VSDBG olmadan Dockerfile üretime hazır IOT Edge modülleri için kullanmanız önerilir.

## <a name="debug-a-module-with-iot-edge-runtime"></a>IOT Edge çalışma zamanı ile bir modülün hatalarını ayıklamak

Her modül klasöründe birkaç Docker dosya için farklı bir kapsayıcı türü vardır. Uzantısıyla biten dosyaları dilediğinizi **.debug** test etmek için modülü.

IOT Edge çalışma zamanı modülleriyle hata ayıklarken modüllerinizi IOT Edge çalışma zamanı üzerinde çalışıyor. IOT Edge cihazı ve VS Code, aynı makinede veya genellikle farklı makinelerde yapılan olabilir (VS Code geliştirme makinede olan ve IOT Edge çalışma zamanı ve modüller başka bir fiziksel makinede çalışıyor). Aşağıdaki adımlar, VS code'da hata ayıklama oturumu için yapılması gerekir.

- IOT Edge Cihazınızı ayarlamak, IOT Edge modul ile yapı **.debug** , Dockerfile ve IOT Edge cihazına dağıtma. 
- IP ve bağlantı noktası için hata ayıklayıcısının modülünün kullanıma sunar.
- Güncelleştirme `launch.json` VS Code, uzak makine kapsayıcısında işlem ekleyebilirsiniz, böylece dosya.

### <a name="build-and-deploy-your-module-and-deploy-to-iot-edge-device"></a>Yapı, modül dağıtma ve IOT Edge cihazına dağıtma

1. Visual Studio Code'da açmak `deployment.debug.template.json` modülü görüntülerinizin uygun ile hata ayıklama sürümü içeren dosya `createOptions` değerleri kümesi.

1. Python modülünüzde geliştiriyorsanız, devam etmeden önce aşağıdaki adımları izleyin:
   - Dosyayı açmak `main.py` ve sonra içeri aktarma bölümü şu kodu ekleyin:

      ```python
      import ptvsd
      ptvsd.enable_attach(('0.0.0.0',  5678))
      ```
   - Aşağıdaki tek satır kod hatalarını ayıklamak istediğiniz geri çağırma ekleyin:

      ```python
      ptvsd.break_into_debugger()
      ```

     Örneğin, hata ayıklamak istiyorsanız `receive_message_callback` yöntemi, aşağıda gösterildiği gibi kodun o satırına ekleme:

      ```python
      def receive_message_callback(message, hubManager):
          ptvsd.break_into_debugger()
          global RECEIVE_CALLBACKS
          message_buffer = message.get_bytearray()
          size = len(message_buffer)
          print ( "    Data: <<<%s>>> & Size=%d" % (message_buffer[:size].decode ('utf-8'), size) )
          map_properties = message.properties()
          key_value_pair = map_properties.get_internals()
          print ( "    Properties: %s" % key_value_pair )
          RECEIVE_CALLBACKS += 1
          print ( "    Total calls received: %d" % RECEIVE_CALLBACKS )
          hubManager.forward_event_to_output("output1", message, 0)
          return IoTHubMessageDispositionResult.ACCEPTED
      ```

1. Visual Studio Code komut paleti:
   1. Komutunu çalıştırın **Azure IOT Edge: Derleme ve anında iletme IOT Edge çözüm**.

   1. Seçin `deployment.debug.template.json` çözümünüz için dosya.

1. İçinde **Azure IOT Hub cihazları** bölümünde Visual Studio kod Gezgini görünümü:
   1. Bir IOT Edge cihaz Kimliğine sağ tıklayın ve ardından **tek cihaz için dağıtım oluşturma**.

   1. Gidin, çözümünüzün **config** klasörüne `deployment.debug.amd64.json` dosya ve ardından **Edge dağıtım bildirimi seçin**.

Dağıtım kimliği ile tümleşik terminalde başarıyla oluşturuldu. dağıtım görürsünüz.

Çalıştırarak, kapsayıcı durumu kontrol edebilirsiniz `docker ps` terminalde komutu. VS Code ve IOT Edge çalışma zamanı aynı makine üzerinde çalıştırıyorsanız, ayrıca Visual Studio kod Docker görünüm durumu kontrol edebilirsiniz.

### <a name="expose-the-ip-and-port-of-the-module-for-the-debugger-to-attach"></a>IP ve bağlantı noktası modülü hata ayıklayıcısının için kullanıma sunma

VS Code, aynı makineye modüllerinizi çalıştırıyorsanız. Kapsayıcı eklemek için localhost kullanıyorsanız ve doğru bağlantı noktası ayarlarını zaten sahip **.debug** Dockerfile, modül kapsayıcı CreateOptions, ve `launch.json`. Bu bölümü atlayabilirsiniz. VS Code ve modüller ayrı makinelerde çalıştırıyorsanız, her dil için aşağıdaki adımları izleyin.

  - **C#, C# İşlevi**: [SSH kanal, geliştirme makineniz ve IOT Edge cihazı yapılandırma](https://github.com/OmniSharp/omnisharp-vscode/wiki/Attaching-to-remote-processes), Düzen `launch.json` eklemek için dosya.
  - **Node.js**: Modül eklemek hata ayıklayıcıları için hazır ve hata ayıklanan makinenin 9229 bağlantı noktası dışında erişilebilir emin olun. Bu açarak doğrulayın [http://%3cdebuggee-machine-IP%3e:9229/json] http:// < hata ayıklanan Makine IP >: hata ayıklayıcı makinede 9229/json. Bu URL, ayıklanacak Node.js hakkında bilgi göstermelidir. Ve ardından hata ayıklayıcıyı makine açık VS Code düzenleyin `launch.json` , adresi değeri "< modül adı > uzaktan hata ayıklama (Node.js)" profil için dosya (veya "< modül adı > uzaktan hata ayıklama (Windows kapsayıcı node.js'de)" Profil modülü olarak çalışıyorsa bir Windows kapsayıcısı) hata ayıklanan makinenin IP ' dir.
  - **Java**: Derleme bir ssh edge cihazına çalıştırarak tünel `ssh -f <username>@<edgedevicehost> -L 5005:127.0.0.1:5005 -N`, i `launch.json` eklemek için dosya. Ayarlar hakkında daha fazla bilgi [burada](https://code.visualstudio.com/docs/java/java-debugging). 
  - **Python**: Kodda `ptvsd.enable_attach(('0.0.0.0', 5678))`, IOT Edge cihazı için IP adresi 0.0.0.0 değiştirin. Oluşturun, gönderin ve sonra da, IOT Edge modülleri yeniden dağıtın. İçinde `launch.json` geliştirme makinenizi `"host"` `"localhost"` değiştirme `"localhost"` uzak IOT Edge cihazınızın genel IP adresine sahip.


### <a name="debug-your-module"></a>Modülünüzün hata ayıklama

Visual Studio Code tutar hata ayıklama yapılandırma bilgilerini bir `launch.json` dosya bulunan bir `.vscode` çalışma alanınızda bir klasör. Bu `launch.json` dosya yeni bir IOT Edge çözümü oluşturduğunuz zaman oluşturulduğu. Bu, her seferinde hata ayıklama destekleyen yeni Modül Ekle güncelleştirir.

1. Visual Studio kod hatalarını görünümünde, bir modül için hata ayıklama yapılandırma dosyasını seçin. Hata ayıklama seçeneği adı şuna benzer olmalıdır  ***&lt;, modül adı&gt;* uzaktan hata ayıklama**

1. Geliştirme dilini için modül dosyasını açın ve bir kesme noktası ekleyin:
   - **C#, C# İşlevi**: Dosyayı açmak `Program.cs` ve bir kesme noktası ekleyin.
   - **Node.js**: Dosyayı açmak `app.js` ve bir breakpont ekleyin.
   - **Java**: Dosyayı açmak `App.java` ve bir kesme noktası ekleyin.
   - **Python**: Açık `main.py` ve bir kesme noktası ekleyin geri çağırma yöntemine eklediğiniz `ptvsd.break_into_debugger()` satır.
   - **C**: Dosyayı açmak `main.c` ve bir kesme noktası ekleyin.

1. Seçin **hata ayıklamayı Başlat** veya **F5**. Ekleme yapılacak işlem seçin.

1. Visual Studio kod hatalarını görünümünde değişkenleri sol bölmesinde görürsünüz.

> [!NOTE]
> Yukarıdaki örnekte, IOT Edge modülleri kapsayıcılarına hata ayıklamak gösterilmektedir. Kullanıma sunulan bağlantı noktaları, modülün kapsayıcıya eklemiş `createOptions` ayarları. Hata ayıklama modüllerinizi tamamladıktan sonra üretime hazır IOT Edge modülleri için kullanıma sunulan bu bağlantı noktalarını kaldırmanız önerilir.

## <a name="next-steps"></a>Sonraki adımlar

Modülünüzün oluşturduktan sonra bilgi nasıl [Visual Studio code'dan Azure IOT Edge modüllerini dağıtmak](how-to-deploy-modules-vscode.md).

Cihazlarınızı IOT Edge modülleri geliştirmek için [kavrama ve kullanma Azure IOT Hub SDK'ları](../iot-hub/iot-hub-devguide-sdks.md).
