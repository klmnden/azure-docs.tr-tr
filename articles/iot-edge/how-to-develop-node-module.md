---
title: Azure IOT Edge için node.js modüllerini hata ayıklama | Microsoft Docs
description: Geliştirme ve Node.js modüllerini Azure IOT Edge için hata ayıklama için Visual Studio Code'u kullanın
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 09/21/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: a1459e3cbd433e2997ffd822b961ac781a72ca90
ms.sourcegitcommit: 42405ab963df3101ee2a9b26e54240ffa689f140
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47423536"
---
# <a name="use-visual-studio-code-to-develop-and-debug-nodejs-modules-for-azure-iot-edge"></a>Geliştirme ve Node.js modüllerini Azure IOT Edge için hata ayıklama için Visual Studio Code'u kullanın

İş mantığınızı için Azure IOT Edge modülleri açarak ucuna çalışılacak gönderebilirsiniz. Bu makalede, Node.js modülleri geliştirmek için ana geliştirme aracı olarak Visual Studio Code (VS Code) kullanmaya yönelik ayrıntılı yönergeler sağlar.

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, bir bilgisayarı veya Windows, macOS veya Linux geliştirme makinenizde olarak çalıştıran sanal makine kullandığınızı varsayar. IOT Edge Cihazınızı başka bir fiziksel cihaz olabilir.

> [!NOTE]
> Hata ayıklama bu makalede, VS Code, Node.js modülün hatalarını ayıklamak için iki normal şekilde gösterilmektedir. Diğer lanuch için modül kod hata ayıklama modunda iken bir modül kapsayıcıdaki bir bir sürece iliştirilip bir yoludur. Visual Studio Code ile hata ayıklama özelliklerine aşina değilseniz okuyun [hata ayıklama](https://code.visualstudio.com/Docs/editor/debugging).

Bu makalede ana geliştirme aracı olarak Visual Studio Code kullandığından, VS Code yükleme ve ardından gerekli genişletmeleri ekleyin:
* [Visual Studio Code](https://code.visualstudio.com/) 
* [Azure IOT Edge uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [Docker uzantısı](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)

Bir modülü oluşturmak için Docker modülü görüntüsü ve modülü görüntüsü tutmak için bir kapsayıcı kayıt defteri oluşturmak için proje klasörünü oluşturmak için npm içeren Node.js gerekir:
* [Node.js](https://nodejs.org)
* [Docker](https://docs.docker.com/engine/installation/)
* [Azure kapsayıcı kayıt defteri](https://docs.microsoft.com/azure/container-registry/) veya [Docker hub'ı](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)
   * Yerel bir Docker kayıt defteri prototip ve bulut kayıt defteri yerine test etme amacıyla kullanabilirsiniz. 

Yerel kurulumu için hata ayıklamak için geliştirme ortamı ve IOT Edge çözümünüzü test çalıştırmak, gereksinim duyduğunuz [Azure IOT EdgeHub geliştirme aracı](https://pypi.org/project/iotedgehubdev/). Yükleme [(2.7/3.6) Python ve Pip](https://www.python.org/). Pip, Python Yükleyici ile birlikte gelir. Yüklemeyi **iotedgehubdev** aşağıdaki komutu, terminalde çalıştırarak.

   ```cmd
   pip install --upgrade iotedgehubdev
   ```

Modülünüzün bir cihazda test etmek için oluşturulan en az bir IOT Edge cihaz Kimliğine sahip etkin bir IOT hub gerekir. IOT Edge arka plan programı geliştirme makinesinde çalıştırıyorsanız, sonraki adıma geçmeden önce EdgeHub ve EdgeAgent durdurmanız gerekebilir. 

## <a name="create-a-new-solution-template"></a>Yeni bir çözüm şablonu oluşturma

Aşağıdaki adımlar Visual Studio Code ve Azure IOT Edge uzantısını kullanarak Node.js dosyasını temel bir IOT Edge modülü oluşturma işlemini gösterir. Bir çözüm oluşturup sonra bu çözümde ilk modülü oluşturma başlatın. Her çözüm, birden çok modül içerebilir. 

1. Visual Studio Code'da seçin **görünümü** > **tümleşik Terminalini**.
2. Tümleşik terminalde, Node.js için Azure IOT Edge modülü şablonunun en son sürümünü yüklemek (veya güncelleştirmek için) aşağıdaki komutu girin:

   ```cmd/sh
   npm install -g yo generator-azure-iot-edge-module
   ```

3. Visual Studio Code’da, **Görünüm** > **Komut Paleti**’ni seçin. 
4. Yazın ve şu komutu çalıştırın komut Paleti'nde **Azure IOT Edge: IOT Edge çözüm yeni**.

   ![Yeni IOT Edge çözümü çalıştırın](./media/how-to-develop-csharp-module/new-solution.png)

5. Yeni çözümü oluşturmak istediğiniz klasörü bulun ve tıklayın **klasörü seçin**. 
6. Çözümünüz için bir ad sağlayın. 
7. Seçin **Node.js modülünü** çözüm içinde ilk modül için şablon olarak.
8. Modülünüzün için bir ad belirtin. Kapsayıcı kayıt defterinizde içinde benzersiz bir ad seçin. 
9. Görüntü deposu için modülü sağlar. VS Code autopopulates modül adı, yalnızca değiştirmek zorunda **localhost:5000** kendi kayıt defteri bilgileri. Test etmek için yerel bir Docker kayıt defteri kullanıyorsanız localhost uygundur. Azure Container Registry kullanırsanız, oturum açma sunucusu defterinizin ayarlarından'ni kullanın. Oturum açma sunucusu benzer  **\<kayıt defteri adı\>. azurecr.io**. Dizenin yalnızca localhost bölümünü değiştirin, modülünüzün adını silmeyin.

   ![Docker görüntü deposunu sağlama](./media/how-to-develop-node-module/repository.png)

VS Code, sağlanan bir IOT Edge çözümü oluşturur, ardından yeni bir pencerede yüklenir bilgilerini alır.

Çözüm içinde üç öğe vardır: 
* A **.vscode** klasörü, hata ayıklama yapılandırmaları içerir.
* A **modülleri** her bir modül için alt klasör içerir. Yalnızca biri varsa, ancak daha komutuyla komut Paleti'nde ekleyebilirsiniz hemen **Azure IOT Edge: IOT Edge Modülü Ekle**. 
* A **.env** dosyası, ortam değişkenleri listeler. Azure Container Registry kaydınız varsa, bir Azure kapsayıcı kayıt defteri kullanıcı adı ve parola bulunması.

   >[!NOTE]
   >Modül için bir görüntü deposuna sağlarsanız, ortam dosyası yalnızca oluşturulur. Test ve yerel olarak hata ayıklama için localhost Varsayılanları kabul ortam değişkenleri gerekmez. 

* A **deployment.template.json** dosyası listeler, yeni bir örnek modülüyle **tempSensor** test etmek için kullanabileceğiniz veri benzetimi gerçekleştiren modülü. Nasıl iş dağıtım bildirimleri hakkında daha fazla bilgi için bkz. [nasıl IOT Edge modülleri, yapılandırılmış, yeniden kaldırılabilir ve anlamak](module-composition.md).

## <a name="develop-your-module"></a>Modülü geliştirme

Çözümünüzle birlikte gelen varsayılan Node.js kodu şu konumdadır **modülleri** > [, modül adı] > **app.js**. Çözümü derleyin, kapsayıcı kayıt defterinize itme ve herhangi bir kod dokunmadan testi başlatmak için bir aygıta dağıtmak modülü ve deployment.template.json dosya ayarlanır. Modül, yalnızca bir kaynak (Bu durumda, veri benzetimi gerçekleştiren tempSensor Modülü) gelenlerin ve IOT Hub'ına kanal için oluşturulmuştur. 

Kendi kodunuzu ile Node.js şablonu özelleştirmek hazır olduğunuzda kullanın [Azure IOT Hub SDK'ları](../iot-hub/iot-hub-devguide-sdks.md) anahtar gereken güvenlik, cihaz yönetimi ve güvenilirlik gibi IOT çözümleri için bu adrese modüller oluşturmak için. 

Visual Studio Code, Node.js için destek sunar. Daha fazla bilgi edinin [VS code'da Node.js ile nasıl çalışılacağını](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial).

## <a name="launch-and-debug-module-code-without-container"></a>Başlatma ve kapsayıcı olmadan modül kodu hatalarını ayıklama

IOT Edge Node.js modülü, Azure IOT Node.js cihaz SDK'sı üzerinde bağlıdır. Varsayılan modülü kodda, başlatma bir **ModuleClient** ortam ayarlar ve giriş adı, IOT Edge Node.js modülünü başka bir deyişle, başlatmak ve çalıştırmak ortam ayarları gerektirir ve ayrıca göndermek veya iletileri yönlendirmek gerekir Giriş kanala. Varsayılan Node.js modülünüzde yalnızca bir giriş kanalı içerir ve ad **input1**.

### <a name="setup-iot-edge-simulator-for-single-module-app"></a>Kurulum IOT Edge modülü tek uygulama simülatörü

1. Simülatörü başlatın ve kurulum için VS Code komut paleti yazıp seçin **Azure IOT Edge: IOT Edge hub'ı simülatörü başlatın tek modülü için**. Ayrıca tek modülü uygulamanızın türü için giriş adlarını gerekir **input1** ve Enter tuşuna basın. Komut tetikleyecek **iotedgehubdev** CLI ve IOT Edge simülatör ve test yardımcı programını modülü kapsayıcı başlatın. Simülatör tek modülü modunda başarıyla başlatıldı tümleşik terminalde aşağıdaki çıktıları görebilirsiniz. Ayrıca bkz bir `curl` yardımcı olmak için komut üzerinden ileti gönder. Daha sonra bu adı kullanacaksınız.

   ![Kurulum IOT Edge modülü tek uygulama simülatörü](media/how-to-develop-csharp-module/start-simulator-for-single-module.png)

   Docker Gezgini taşıyabilir ve çalışma durumu modülüne bakın.

   ![Simülatör Modül durumu](media/how-to-develop-csharp-module/simulator-status.png)

   **EdgeHubDev** yerel IOT Edge simülatör setinin kapsayıcıdır. Bu IOT Edge güvenlik arka plan programı olmadan geliştirme makinenizde çalıştırın ve uygulamanızı yerel modül veya modül kapsayıcılar için ortam ayarları sağlayın. **Giriş** kapsayıcı kullanıma sunulan restAPIs Giriş kanalı, modül hedef köprüsü iletileri yardımcı olmak için.

2. VS Code komut paleti yazın ve **Azure IOT Edge: kullanıcı ayarları için modülü kimlik bilgilerini ayarla** ortam ayarlarına modülü ayarlanacak `azure-iot-edge.EdgeHubConnectionString` ve `azure-iot-edge.EdgeModuleCACertificateFile` kullanıcı ayarları. Bu ortam ayarlarını başvurulan bulabilirsiniz **.vscode** > **launch.json** ve [VS Code kullanıcı ayarları](https://code.visualstudio.com/docs/getstarted/settings).

### <a name="debug-nodejs-module-in-launch-mode"></a>Node.js modülü başlatma modda hata ayıklama

1. Tümleşik terminalde dizinine **NodeModule** klasörü, düğüm paketleri yüklemek için aşağıdaki komutu çalıştırın

   ```cmd
   npm install
   ```

2. `app.js` sayfasına gidin. Bu dosyada kesme noktası ekleyin.

3. VS Code hata ayıklama görünümüne gidin. Hata ayıklama Yapılandırması **ModuleName yerel hata ayıklama (Node.js)**. 

4. Tıklayın **hata ayıklamayı Başlat** veya basın **F5**. Hata ayıklama oturumu başlar.

5. VS Code tümleşik terminale göndermek için aşağıdaki komutu çalıştırın bir **Hello World** modülünüzde ileti. Önceki adımda gösterilen komut budur zaman IOT Edge simulator'ı başarıyla ayarlandı. Geçerli bir engellenirse oluşturamaz ve başka bir tümleşik terminale gerekebilir.

    ```cmd
    curl --header "Content-Type: application/json" --request POST --data '{"inputName": "input1","data":"hello world"}' http://localhost:53000/api/v1/messages
    ```

   > [!NOTE]
   > Windows kullanıyorsanız, VS Code tümleşik Terminalini Kabuk olduğundan emin olarak **Git Bash** veya **WSL Bash**. Çalıştıramazsınız `curl` komutu PowerShell veya komut isteminde. 
   
   > [!TIP]
   > Ayrıca [PostMan](https://www.getpostman.com/) veya yerine üzerinden ileti göndermek için API araçlara `curl`.

6. VS Code hata ayıklama Görünümü'nde sol bölmedeki değişkenleri görürsünüz. 

7. Hata ayıklama oturumunu durdurmak için Durdur düğmesini veya tuşuna tıklayın **Shift + F5 tuşlarına basarak**. VS Code komut paleti yazın ve seçin **Azure IOT Edge: IOT Edge simülatör Durdur** durdurup simülatör temizlemek için.


## <a name="build-module-container-for-debugging-and-debug-in-attach-mode"></a>Hata ayıklama ve hata ayıklama için modül kapsayıcı derleme içinde modu ekleme

İki modül varsayılan çözümünüzü içeren, bir sanal sıcaklık algılayıcısı modül biridir ve diğer Node.js kanal modülüdür. Sanal sıcaklık algılayıcısı Node.js kanal modülüne iletileri göndermeye devam eder ve sonra iletileri IOT Hub'ına yöneltilen. Oluşturduğunuz modül klasöründe birkaç Docker dosya için farklı bir kapsayıcı türü vardır. Uzantısıyla biten bu dosyaları dilediğinizi **.debug** test etmek için modülü. Şu anda Node.js modüllerini yalnızca linux-amd64, amd64 windows ve linux arm32v7 kapsayıcılarında hata ayıklamayı destekler.

### <a name="setup-iot-edge-simulator-for-iot-edge-solution"></a>IOT Edge çözüm Kurulum IOT Edge simülatörü

Geliştirme makinenizde, IOT Edge çözümü çalıştırmak için IOT Edge güvenlik daemon yüklemek yerine IOT Edge simülatör başlayabilirsiniz. 

1. Sol taraftaki cihaz Gezgini'nde, sağ tıklayın, IOT Edge cihaz Kimliğine, select **Kurulum IOT Edge simülatör** cihaz bağlantı dizesiyle simülatörü başlatın.

2. IOT Edge simülatör tümleşik terminalde Kurulum başarıyla verildi görebilirsiniz.

### <a name="build-and-run-container-for-debugging-and-debug-in-attach-mode"></a>Derleme ve hata ayıklama ve hata ayıklama için kapsayıcı çalıştırma modu olarak ekleme

1. VS Code'da gidin `deployment.template.json` dosya. Modül görüntü URL'nizi ekleyerek güncelleştirme **.debug** sonuna.

2. Node.js modülü createOptions içinde değiştirin **deployment.template.json** ile içerik aşağıda ve bu dosya: 
    ```json
    "createOptions": "{\"ExposedPorts\":{\"9229/tcp\":{}},\"HostConfig\":{\"PortBindings\":{\"9229/tcp\":[{\"HostPort\":\"9229\"}]}}}"
    ```

3. VS Code hata ayıklama görünümüne gidin. Bir modül için hata ayıklama yapılandırma dosyasını seçin. Hata ayıklama seçeneği adı şuna benzer olmalıdır **ModuleName uzaktan hata ayıklama (Node.js)** veya **ModuleName uzaktan hata ayıklama (Windows kapsayıcı node.js'de)**, bağımlı olan geliştirme makinesinde, bir kapsayıcı türü.

4. Seçin **hata ayıklamayı Başlat** veya **F5**. Ekleme yapılacak işlem seçin.

5. VS Code hata ayıklama Görünümü'nde sol bölmedeki değişkenleri görürsünüz.

6. Hata ayıklama oturumunu durdurmak için Durdur düğmesini veya tuşuna tıklayın **Shift + F5 tuşlarına basarak**. VS Code komut paleti yazın ve seçin **Azure IOT Edge: IOT Edge simülatör Durdur**.

> [!NOTE]
> Yukarıdaki örnekte Node.js IOT Edge modülleri kapsayıcılarına hata ayıklama gösterilmektedir. Bu, modül kapsayıcı createOptions içinde kullanıma sunulan bağlantı noktası eklendi. Node.js modüllerinizi hata ayıklamasını tamamladığınızda, üretime hazır IOT Edge modülleri için kullanıma sunulan bu bağlantı noktalarını kaldırmanız önerilir.

## <a name="next-steps"></a>Sonraki adımlar

Yerleşik modülünüzde aldıktan sonra bilgi nasıl [Visual Studio code'dan dağıtma Azure IOT Edge modülleri](how-to-deploy-modules-vscode.md)

Cihazlarınızı IOT Edge modülleri geliştirmek için [kavrama ve kullanma Azure IOT Hub SDK'ları](../iot-hub/iot-hub-devguide-sdks.md).
