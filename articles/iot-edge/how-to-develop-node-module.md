---
title: Azure IOT Edge için node.js modüllerini hata ayıklama | Microsoft Docs
description: Geliştirme ve Node.js modüllerini Azure IOT Edge için hata ayıklama için Visual Studio Code'u kullanın
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 06/26/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 78e952b5b1eedc1757cfe636eb13e411044dce54
ms.sourcegitcommit: 0fcd6e1d03e1df505cf6cb9e6069dc674e1de0be
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2018
ms.locfileid: "42056244"
---
# <a name="develop-and-debug-nodejs-modules-with-azure-iot-edge-for-visual-studio-code"></a>Geliştirme ve Node.js modüllerini Visual Studio Code için Azure IOT Edge ile hata ayıklama

İş mantığınızı için Azure IOT Edge modülleri açarak ucuna çalışılacak gönderebilirsiniz. Bu makalede, Node.js modülleri geliştirmek için ana geliştirme aracı olarak Visual Studio Code (VS Code) kullanmaya yönelik ayrıntılı yönergeler sağlar.

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, bir bilgisayarı veya geliştirme makinenize Windows veya Linux çalıştıran sanal makine kullandığınızı varsayar. Geliştirme makinenizde, IOT Edge cihazının simülasyonunu gerçekleştirme veya başka bir fiziksel cihaz IOT Edge Cihazınızı olabilir.

> [!NOTE]
> Hata ayıklama Bu öğretici, bir modül kapsayıcıdaki bir sürece iliştirilip ve VS Code ile hata ayıklama açıklar. Node.js modüllerini linux-amd64, windows ve arm32 kapsayıcıları ayıklayabilirsiniz. Visual Studio Code ile hata ayıklama özelliklerine aşina değilseniz okuyun [hata ayıklama](https://code.visualstudio.com/Docs/editor/debugging). 

Bu makalede ana geliştirme aracı olarak Visual Studio Code kullandığından, VS Code yükleme ve ardından gerekli genişletmeleri ekleyin:
* [Visual Studio Code](https://code.visualstudio.com/) 
* [Azure IOT Edge uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [Docker uzantısı](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)

Bir modülü oluşturmak için Docker modülü görüntüsü ve modülü görüntüsü tutmak için bir kapsayıcı kayıt defteri oluşturmak için proje klasörünü oluşturmak için npm içeren Node.js gerekir:
* [Node.js](https://nodejs.org)
* [Docker](https://docs.docker.com/engine/installation/)
* [Azure kapsayıcı kayıt defteri](https://docs.microsoft.com/azure/container-registry/) veya [Docker hub'ı](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)

   >[!TIP]
   >Yerel bir Docker kayıt defteri prototip ve bulut kayıt defteri yerine test etme amacıyla kullanabilirsiniz. 

Modülünüzün bir cihazda test etmek için etkin bir IOT hub ile en az bir IOT Edge cihazı gerekir. Bilgisayarınızı bir IOT Edge cihazı kullanmak istiyorsanız, bunu için öğreticilerde adımları izleyerek yapabilirsiniz [Windows](quickstart.md) veya [Linux](quickstart-linux.md). 

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
9. Görüntü deposu için modülü sağlar. VS Code autopopulates modül adı, yalnızca değiştirmek zorunda **localhost:5000** kendi kayıt defteri bilgileri. Test etmek için yerel bir Docker kayıt defteri kullanıyorsanız localhost uygundur. Azure Container Registry kullanırsanız, oturum açma sunucusu defterinizin ayarlarından'ni kullanın. Oturum açma sunucusu benzer  **\<kayıt defteri adı\>. azurecr.io**.

VS Code, sağlanan bir IOT Edge çözümü oluşturur, ardından yeni bir pencerede yüklenir bilgilerini alır.

Çözüm içinde üç öğe vardır: 
* A **.vscode** klasörü, hata ayıklama yapılandırmaları içerir.
* A **modülleri** her bir modül için alt klasör içerir. Yalnızca biri varsa, ancak daha komutuyla komut Paleti'nde ekleyebilirsiniz hemen **Azure IOT Edge: IOT Edge Modülü Ekle**. 
* A **.env** dosyası, ortam değişkenleri listeler. Azure Container Registry kaydınız varsa, bir Azure kapsayıcı kayıt defteri kullanıcı adı ve parola bulunması.

   >[!NOTE]
   >Modül için bir görüntü deposuna sağlarsanız, ortam dosyası yalnızca oluşturulur. Test ve yerel olarak hata ayıklama için localhost Varsayılanları kabul ortam değişkenleri gerekmez. 

* A **deployment.template.json** dosyası listeler, yeni bir örnek modülüyle **tempSensor** test etmek için kullanabileceğiniz veri benzetimi gerçekleştiren modülü. Nasıl iş dağıtım bildirimleri hakkında daha fazla bilgi için bkz. [nasıl IOT Edge modülleri, yapılandırılmış, yeniden kaldırılabilir ve anlamak](module-composition.md).

## <a name="develop-your-module"></a>Modülü geliştirme

Çözümünüzle birlikte gelen varsayılan Node.js kodu şu konumdadır **modülleri** > **\<, modül adı\>** > **app.js** . Çözümü derleyin, kapsayıcı kayıt defterinize itme ve herhangi bir kod dokunmadan testi başlatmak için bir aygıta dağıtmak modülü ve deployment.template.json dosya ayarlanır. Modül, yalnızca bir kaynak (Bu durumda, veri benzetimi gerçekleştiren tempSensor Modülü) gelenlerin ve IOT Hub'ına kanal için oluşturulmuştur. 

Kendi kodunuzu ile Node.js şablonu özelleştirmek hazır olduğunuzda kullanın [Azure IOT Hub SDK'ları](../iot-hub/iot-hub-devguide-sdks.md) anahtar gereken güvenlik, cihaz yönetimi ve güvenilirlik gibi IOT çözümleri için bu adrese modüller oluşturmak için. 

## <a name="build-and-deploy-your-module-for-debugging"></a>Derleme ve hata ayıklama için modülünüzde dağıtma

Her modül klasöründe, farklı bir kapsayıcı türü için birden çok Docker dosyası vardır. Uzantısıyla biten bu dosyalardan herhangi biri kullanabileceğiniz **.debug** test etmek için modülü. Şu anda Node.js modüllerini yalnızca linux-amd64, amd64 windows ve linux arm32v7 kapsayıcılarında hata ayıklamayı destekler.

1. VS Code'da gidin `deployment.template.json` dosya. Modül görüntü URL'nizi ekleyerek güncelleştirme **.debug** sonuna.
2. Node.js modülü createOptions içinde değiştirin **deployment.template.json** ile içerik aşağıda ve bu dosya: 
    ```json
    "createOptions": "{\"ExposedPorts\":{\"9229/tcp\":{}},\"HostConfig\":{\"PortBindings\":{\"9229/tcp\":[{\"HostPort\":\"9229\"}]}}}"
    ```

2. VS Code komut paletinde yazın ve şu komutu çalıştırın **Azure IOT Edge: IOT Edge çözüm**.
3. Seçin `deployment.template.json` komut paletini çözümünüzden dosyası. 
4. Azure IOT Hub cihazları Gezgini'nde bir IOT Edge cihaz Kimliğine sağ tıklayın ve ardından **IOT Edge cihazı için dağıtım oluşturma**. 
5. Açık **config** klasörü, çözümünüzün seçip `deployment.json` dosya. **Select Edge Deployment Manifest** (Edge Dağıtım Bildirimini Seç) öğesine tıklayın. 

Dağıtım başarıyla kimliği VS Code tümleşik bir dağıtım ile terminal oluşturulduktan sonra görebilirsiniz.

VS Code Docker Gezgini veya çalıştırma, kapsayıcı durumu kontrol edebilirsiniz `docker ps` terminalde komutu.

## <a name="start-debugging-nodejs-module-in-vs-code"></a>VS code'da node.js modülü hata ayıklamayı Başlat

VS Code tutar hata ayıklama yapılandırma bilgilerini bir `launch.json` dosya bulunan bir `.vscode` çalışma alanınızda bir klasör. Bu `launch.json` dosya yeni bir IOT Edge çözümü oluşturduğunuz zaman oluşturulduğu. Bu, her seferinde hata ayıklama destekleyen yeni Modül Ekle güncelleştirir. 

1. VS Code hata ayıklama görünümüne gidin ve kendi modül için hata ayıklama yapılandırma dosyasını seçin.

2. `app.js` sayfasına gidin. Bu dosyada kesme noktası ekleyin.

3. Tıklayın **hata ayıklamayı Başlat** düğme veya basın **F5**, ekleme işlemini seçin.

4. VS Code hata ayıklama Görünümü'nde Sol paneldeki değişkenlerinde görebilirsiniz. 

Yukarıdaki örnekte Node.js IOT Edge modülleri kapsayıcılarına hata ayıklama gösterilmektedir. Bu, modül kapsayıcı createOptions içinde kullanıma sunulan bağlantı noktası eklendi. Node.js modüllerinizi hata ayıklamasını tamamladığınızda, üretime hazır IOT Edge modülleri için kullanıma sunulan bu bağlantı noktalarını kaldırmanız önerilir.

## <a name="next-steps"></a>Sonraki adımlar

Yerleşik modülünüzde aldıktan sonra bilgi nasıl [Visual Studio code'dan dağıtma Azure IOT Edge modülleri](how-to-deploy-modules-vscode.md)

Cihazlarınızı IOT Edge modülleri geliştirmek için [kavrama ve kullanma Azure IOT Hub SDK'ları](../iot-hub/iot-hub-devguide-sdks.md).
