---
title: Azure IOT Edge için Python modüllerini hata ayıklama | Microsoft Docs
description: Geliştirme, oluşturma ve Azure IOT Edge için bir Python modülü hata ayıklamak için Visual Studio Code kullanın
services: iot-edge
keywords: ''
author: shizn
manager: philmea
ms.author: xshi
ms.date: 09/13/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 3097e6fcfcd219f73927131106e1f35ff54210b0
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52679435"
---
# <a name="use-visual-studio-code-to-develop-and-debug-python-modules-for-azure-iot-edge"></a>Geliştirmek ve Azure IOT Edge için Python modüllerini hata ayıklamak için Visual Studio Code kullanın

İçin Azure IOT Edge modülleri, iş mantığınızı kapatabilirsiniz. Bu makalede, Visual Studio Code (VS Code) ana aracı olarak geliştirme ve hata ayıklama Python modüllerini nasıl kullanacağınızı gösterir.

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, bir bilgisayar veya geliştirme makinenize Windows veya Linux çalıştıran sanal makine kullandığınızı varsayar. Ve IOT Edge güvenlik daemon ile geliştirme makinenizde, IOT Edge cihazının simülasyonunu gerçekleştirme.

> [!NOTE]
> Hata ayıklama bu makalede, bir modül kapsayıcıdaki bir sürece iliştirilip ve VS Code ile hata ayıklama gösterilmektedir. Yalnızca Linux amd64 kapsayıcıları Python modüllerde ayıklayabilirsiniz. Visual Studio Code ile hata ayıklama özelliklerine aşina değilseniz okuyun [hata ayıklama](https://code.visualstudio.com/Docs/editor/debugging). 

Bu makalede ana geliştirme aracı olarak Visual Studio Code kullandığından, VS Code yükleme. Daha sonra gerekli genişletmeleri ekleyin:
* [Visual Studio Code](https://code.visualstudio.com/) 
* [Azure IOT Edge uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [Docker uzantısı](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)
* Visual Studio Code için [Python uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-python.python). 
* [Python](https://www.python.org/downloads/).
* Python paketlerini (genellikle Python yüklemenize dahildir) yüklemek için [PIP](https://pip.pypa.io/en/stable/installing/#installation).
* Yükleme **Cookiecutter** aşağıdaki komutla
   
    ```cmd/sh
    pip install --upgrade --user cookiecutter
    ```

Bir modül oluşturma modülü görüntüsü ve modülü görüntüsü tutmak için bir kapsayıcı kayıt defteri oluşturmak için Docker gerekir:
* [Docker Community Edition](https://docs.docker.com/install/) geliştirme makinenizde. 
* [Azure kapsayıcı kayıt defteri](https://docs.microsoft.com/azure/container-registry/) veya [Docker hub'ı](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)
   * Prototip ve test etme amacıyla bir bulut kayıt defteri yerine yerel bir Docker kayıt defteri kullanabilirsiniz. 

Modülünüzün bir cihazda test etmek için etkin bir IOT hub ile en az bir IOT Edge cihazı gerekir. Bilgisayarınızı bir IOT Edge cihazı kullanmak için hızlı başlangıç için adımları izleyin. [Linux](quickstart-linux.md). 

## <a name="create-a-new-solution-template"></a>Yeni bir çözüm şablonu oluşturma

Azure IOT Python Visual Studio Code ve Azure IOT Edge uzantısını kullanarak SDK alan bir IOT Edge modülünüzü oluşturmak için bu adımları uygulayın. İlk olarak, bir çözüm oluşturun ve ardından ilk Modül içindeki çözümü oluşturun. Her çözüm, birden fazla modülü içerebilir. 

1. Visual Studio Code'da seçin **görünümü** > **tümleşik Terminalini**.

2. Seçin **görünümü** > **komut paleti**. 

3. Komut Paleti'nde girin ve şu komutu çalıştırın **Azure IOT Edge: IOT Edge çözüm yeni**.

   ![Yeni IOT Edge çözümü çalıştırın](./media/how-to-develop-csharp-module/new-solution.png)

4. Yeni çözümü oluşturmak istediğiniz klasöre göz atın. Seçin **klasörü seçin**. 

5. Çözümünüz için bir ad girin. 

6. Seçin **Python Modülü** çözüm içinde ilk modül için şablon olarak.

7. Bir modül için bir ad girin. Kapsayıcı kayıt defterinizde içinde benzersiz bir ad seçin. 

8. Modülün görüntü deposu adını sağlayın. VS Code autopopulates modül adı ile **localhost:5000**. Kayıt defteri kendi bilgilerinizle değiştirin. Yerel bir Docker kayıt defteri test, ardından kullanıyorsanız **localhost** bir sakınca yoktur. Azure Container Registry kullanırsanız, oturum açma sunucusu defterinizin ayarlarından'ni kullanın. Oturum açma sunucusu benzer  **\<kayıt defteri adı\>. azurecr.io**. Dizenin yalnızca localhost bölümünü değiştirin, modülünüzün adını silmeyin. Son dize şuna benzer \<kayıt defteri adı\>.azurecr.io/\<modulename\>.

   ![Docker görüntü deposunu sağlama](./media/how-to-develop-c-module/repository.png)

VS Code, sağlanan bir IOT Edge çözümü oluşturur ve ardından yeni bir pencerede yüklenir bilgilerini alır.

Çözüm içinde dört öğe vardır: 
* A **.vscode** klasörü, hata ayıklama yapılandırmaları içerir.
* A **modülleri** klasörü her modül için alt klasörler bulunur. Bu noktada yalnızca biri gerekir. Ancak daha komutuyla komut Paleti'nde ekleyebilirsiniz **Azure IOT Edge: IOT Edge Modülü Ekle**. 
* Bir **.env** dosyası, ortam değişkenleri listeler. Azure Container Registry kaydınız varsa, bir Azure kapsayıcı kayıt defteri kullanıcı adı ve parola bulunması. 

   > [!NOTE]
   > Modül için bir görüntü deposuna sağlarsanız, ortam dosyası yalnızca oluşturulur. Test ve yerel olarak hata ayıklama için localhost Varsayılanları kabul ortam değişkenleri gerekmez. 

* A **deployment.template.json** dosyası listeler, yeni bir örnek modülüyle **tempSensor** veri benzetimi gerçekleştiren modülü test etmek için kullanabilirsiniz. Nasıl iş dağıtım bildirimleri hakkında daha fazla bilgi için bkz. [modülleri dağıtma ve yollar kurmak için dağıtım bildirimleri kullanmayı öğrenin](module-composition.md). 
* A **deployment.debug.template.json** dosya kapsayıcıları modülünüzün hata ayıklama sürümü, uygun kapsayıcı seçeneklerle görüntüler.

## <a name="develop-your-module"></a>Modülü geliştirme

Çözümünüzle birlikte gelen varsayılan Python modülü kodu şu konumdadır **modülleri** > [, modül adı] > **main.py**. Çözümü derleyin, kapsayıcı kayıt defterinize itme ve herhangi bir kod dokunmadan testi başlatmak için bir aygıta dağıtmak modülü ve deployment.template.json dosya ayarlanır. Modül, yalnızca bir kaynak (Bu durumda, veri benzetimi gerçekleştiren tempSensor Modülü) gelenlerin ve IOT Hub'ına kanal için oluşturulmuştur. 

Kendi kodunuzu ile Python şablonu özelleştirmek hazır olduğunuzda kullanın [Azure IOT Hub SDK'ları](../iot-hub/iot-hub-devguide-sdks.md) anahtar gereken güvenlik, cihaz yönetimi ve güvenilirlik gibi IOT çözümleri için bu adrese modüller oluşturmak için. 

## <a name="build-and-deploy-your-module-for-debugging"></a>Derleme ve hata ayıklama için modülünüzde dağıtma

Her modül klasöründe birkaç Docker dosya için farklı bir kapsayıcı türü vardır. Uzantısıyla biten bu dosyaları dilediğinizi **.debug** test etmek için modülü. Şu anda Python modüllerini yalnızca Linux amd64 kapsayıcılarında hata ayıklamayı destekler. 

1. VS Code'da gidin `deployment.debug.template.json` dosya. Bu dosya, modülün hata ayıklama sürümünü içerir uygun görüntülerle oluşturma seçenekleri. 

3. Gidin `main.py`, içeri aktarma bölümünden sonra kodu ekleyin
    
    ```python
    import ptvsd
    ptvsd.enable_attach(('0.0.0.0',  5678))
    ```

4. Aşağıdaki tek satır kod hatalarını ayıklamak istediğiniz geri ekleyin.

    ```python
    ptvsd.break_into_debugger()
    ```

   Örneğin, hata ayıklamak istiyorsanız `receive_message_callback` yöntemi. Tek satır aşağıdaki gibi kod ekleyebilirsiniz.

    ```python
    def receive_message_callback(message, hubManager):
        ptvsd.break_into_debugger()
        global RECEIVE_CALLBACKS
        message_buffer = message.get_bytearray()
        size = len(message_buffer)
        print ( "    Data: <<<%s>>> & Size=%d" % (message_buffer[:size].decode('utf-8'), size) )
        map_properties = message.properties()
        key_value_pair = map_properties.get_internals()
        print ( "    Properties: %s" % key_value_pair )
        RECEIVE_CALLBACKS += 1
        print ( "    Total calls received: %d" % RECEIVE_CALLBACKS )
        hubManager.forward_event_to_output("output1", message, 0)
        return IoTHubMessageDispositionResult.ACCEPTED
    ```

2. VS Code komut paleti girin ve şu komutu çalıştırın **Azure IOT Edge: derleme ve anında iletme IOT Edge çözüm**.
3. Seçin `deployment.debug.template.json` komut paletini çözümünüzden dosyası. 
4. Azure IOT hub'ı Device Explorer içinde bir IOT Edge cihaz kimliğini sağ tıklayın. Ardından **tek cihaz için dağıtım oluşturma**. 
5. Çözümünüzün açın **config** klasör. Ardından `deployment.debug.amd64.json` dosya. Seçin **seçin Edge dağıtım bildirimi**. 

Dağıtım kimliği ile bir VS Code tümleşik terminalde başarıyla oluşturuldu. dağıtım görürsünüz.

VS Code Docker Gezgini veya çalıştırarak kapsayıcı durumunuzu denetleme `docker ps` terminalde komutu.

## <a name="start-debugging-python-module-in-vs-code"></a>Python modülü VS code'da hata ayıklamayı Başlat
VS Code tutar hata ayıklama yapılandırma bilgilerini bir `launch.json` dosya bulunan bir `.vscode` çalışma alanınızda bir klasör. Bu `launch.json` dosya yeni bir IOT Edge çözümü oluşturduğunuz zaman oluşturulduğu. Bu, her seferinde hata ayıklama destekleyen yeni Modül Ekle güncelleştirir. 

1. VS Code hata ayıklama görünümüne gidin. Bir modül için hata ayıklama yapılandırma dosyasını seçin. Hata ayıklama seçeneği adı şuna benzer olmalıdır **ModuleName uzaktan hata ayıklama (Python)**

2. `main.c` sayfasına gidin. Yerleştirdiğiniz geri çağırma yöntemi bir kesme noktası ekleyin `ptvsd.break_into_debugger()` da.

3. Seçin **hata ayıklamayı Başlat** veya **F5**. Ekleme yapılacak işlem seçin.

4. VS Code hata ayıklama Görünümü'nde sol bölmedeki değişkenleri görürsünüz. 

Önceki örnek Python IOT Edge modülleri kapsayıcılarına hata ayıklamak nasıl gösterir. Bu, modül kapsayıcı createOptions içinde kullanıma sunulan bağlantı noktası eklendi. Hata ayıklama, Python modüllerini tamamladıktan sonra üretime hazır IOT Edge modülleri için kullanıma sunulan bu bağlantı noktalarını kaldırmanız önerilir.

## <a name="next-steps"></a>Sonraki adımlar

Modülünüzün oluşturduktan sonra bilgi nasıl [Visual Studio code'dan Azure IOT Edge modüllerini dağıtmak](how-to-deploy-modules-vscode.md).

Cihazlarınızı IOT Edge modülleri geliştirmek için [kavrama ve kullanma Azure IOT Hub SDK'ları](../iot-hub/iot-hub-devguide-sdks.md).
