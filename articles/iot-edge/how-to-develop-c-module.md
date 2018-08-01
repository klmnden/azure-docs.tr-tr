---
title: C modülleri, Azure IOT Edge için hata ayıklama | Microsoft Docs
description: Geliştirme, derleme ve bir C modül Azure IOT Edge için hata ayıklama için Visual Studio Code'u kullanın
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 07/20/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 9fc067c46828079f7369683b5edec682747cd5c7
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39391461"
---
# <a name="use-visual-studio-code-to-develop-and-debug-c-modules-for-azure-iot-edge"></a>Geliştirme ve C modülleri, Azure IOT Edge için hata ayıklama için Visual Studio Code'u kullanın

İçin Azure IOT Edge modülleri, iş mantığınızı kapatabilirsiniz. Bu makalede, Visual Studio Code (VS Code) ana aracı olarak geliştirme ve hata ayıklama C modüller için nasıl kullanılacağını gösterir.

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, bir bilgisayar veya geliştirme makinenize Windows veya Linux çalıştıran sanal makine kullandığınızı varsayar. Ve geliştirme makinenizde, IOT Edge cihazının simülasyonunu gerçekleştirme.

> [!NOTE]
> Hata ayıklama bu makalede, bir modül kapsayıcıdaki bir sürece iliştirilip ve VS Code ile hata ayıklama gösterilmektedir. Yalnızca C modülleri Linux amd64-kapsayıcılarında hata ayıklama yapabilirsiniz. Visual Studio Code ile hata ayıklama özelliklerine aşina değilseniz okuyun [hata ayıklama](https://code.visualstudio.com/Docs/editor/debugging). 

Bu makalede ana geliştirme aracı olarak Visual Studio Code kullandığından, VS Code yükleme. Daha sonra gerekli genişletmeleri ekleyin:
* [Visual Studio Code](https://code.visualstudio.com/) 
* [Azure IOT Edge uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [C/C++ uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools) Visual Studio Code için.
* [Docker uzantısı](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)

Bir modül oluşturma modülü görüntüsü ve modülü görüntüsü tutmak için bir kapsayıcı kayıt defteri oluşturmak için Docker gerekir:
* [Docker Community Edition](https://docs.docker.com/install/) geliştirme makinenizde. 
* [Azure kapsayıcı kayıt defteri](https://docs.microsoft.com/azure/container-registry/) veya [Docker hub'ı](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)

   > [!TIP]
   > Prototip ve test etme amacıyla bir bulut kayıt defteri yerine yerel bir Docker kayıt defteri kullanabilirsiniz. 

Modülünüzün bir cihazda test etmek için etkin bir IOT hub ile en az bir IOT Edge cihazı gerekir. Bilgisayarınızı bir IOT Edge cihazı kullanmak için hızlı başlangıç için adımları izleyin. [Windows](quickstart.md) veya [Linux](quickstart-linux.md). 

## <a name="create-a-new-solution-template"></a>Yeni bir çözüm şablonu oluşturma

Azure IOT C Visual Studio Code ve Azure IOT Edge uzantısını kullanarak SDK alan bir IOT Edge modülünüzü oluşturmak için bu adımları uygulayın. İlk olarak, bir çözüm oluşturun ve ardından ilk Modül içindeki çözümü oluşturun. Her çözüm, birden fazla modülü içerebilir. 

1. Visual Studio Code'da seçin **görünümü** > **tümleşik Terminalini**.
3. Seçin **görünümü** > **komut paleti**. 
4. Komut Paleti'nde girin ve şu komutu çalıştırın **Azure IOT Edge: IOT Edge çözüm yeni**.

   ![Yeni IOT Edge çözümü çalıştırın](./media/how-to-develop-csharp-module/new-solution.png)

5. Yeni çözümü oluşturmak istediğiniz klasöre göz atın. Seçin **klasörü seçin**. 
6. Çözümünüz için bir ad girin. 
7. Seçin **C Modülü** çözüm içinde ilk modül için şablon olarak.
8. Bir modül için bir ad girin. Kapsayıcı kayıt defterinizde içinde benzersiz bir ad seçin. 
9. Modülün görüntü deposu adını sağlayın. VS Code autopopulates modül adı ile **localhost:5000**. Kayıt defteri kendi bilgilerinizle değiştirin. Yerel bir Docker kayıt defteri test, ardından kullanıyorsanız **localhost** bir sakınca yoktur. Azure Container Registry kullanırsanız, oturum açma sunucusu defterinizin ayarlarından'ni kullanın. Oturum açma sunucusu benzer  **\<kayıt defteri adı\>. azurecr.io**.

VS Code, sağlanan bir IOT Edge çözümü oluşturur ve ardından yeni bir pencerede yüklenir bilgilerini alır.

   ![IOT Edge çözümünü görüntüleme](./media/how-to-develop-c-module/view-solution.png)

Çözüm içinde dört öğe vardır: 
* A **.vscode** klasörü, hata ayıklama yapılandırmaları içerir.
* A **modülleri** klasörü her modül için alt klasörler bulunur. Bu noktada yalnızca biri gerekir. Ancak daha komutuyla komut Paleti'nde ekleyebilirsiniz **Azure IOT Edge: IOT Edge Modülü Ekle**. 
* Bir **.env** dosyası, ortam değişkenleri listeler. Azure Container Registry kaydınız varsa, bir Azure kapsayıcı kayıt defteri kullanıcı adı ve parola bulunması. 

   > [!NOTE]
   > Modül için bir görüntü deposuna sağlarsanız, ortam dosyası yalnızca oluşturulur. Test ve yerel olarak hata ayıklama için localhost Varsayılanları kabul ortam değişkenleri gerekmez. 

* A **deployment.template.json** dosyası listeler, yeni bir örnek modülüyle **tempSensor** veri benzetimi gerçekleştiren modülü test etmek için kullanabilirsiniz. Nasıl iş dağıtım bildirimleri hakkında daha fazla bilgi için bkz. [modülleri dağıtma ve yollar kurmak için dağıtım bildirimleri kullanmayı öğrenin](module-composition.md). 

## <a name="develop-your-module"></a>Modülü geliştirme

Çözümünüzle birlikte gelen varsayılan C modülü kodu şu konumdadır **modülleri** > **\<, modül adı\>** > **main.c** . Çözümü derleyin, kapsayıcı kayıt defterinize itme ve herhangi bir kod dokunmadan testi başlatmak için bir aygıta dağıtmak modülü ve deployment.template.json dosya ayarlanır. Modül, yalnızca bir kaynak (Bu durumda, veri benzetimi gerçekleştiren tempSensor Modülü) gelenlerin ve IOT Hub'ına kanal için oluşturulmuştur. 

Kendi kodunuzu ile C şablonu özelleştirmek hazır olduğunuzda kullanın [Azure IOT Hub SDK'ları](../iot-hub/iot-hub-devguide-sdks.md) anahtar gereken güvenlik, cihaz yönetimi ve güvenilirlik gibi IOT çözümleri için bu adrese modüller oluşturmak için. 

## <a name="build-and-deploy-your-module-for-debugging"></a>Derleme ve hata ayıklama için modülünüzde dağıtma

Her modül klasöründe birkaç Docker dosya için farklı bir kapsayıcı türü vardır. Uzantısıyla biten bu dosyaları dilediğinizi **.debug** test etmek için modülü. Şu anda C Modüller yalnızca Linux amd64 kapsayıcılarında hata ayıklamayı destekler.

1. VS Code'da gidin `deployment.template.json` dosya. Modül görüntü URL'nizi ekleyerek güncelleştirme **.debug** sonuna.

    ![Ekleme *** .debug, görüntü adı](./media/how-to-develop-c-module/image-debug.png)

2. Node.js modülü createOptions içinde değiştirin **deployment.template.json** ile içerik aşağıda ve bu dosya: 
    
    ```json
    "createOptions": "{\"HostConfig\": {\"Privileged\": true}}"
    ```

2. VS Code komut paleti girin ve şu komutu çalıştırın **Edge: IOT Edge çözüm**.
3. Seçin `deployment.template.json` komut paletini çözümünüzden dosyası. 
4. Azure IOT hub'ı Device Explorer içinde bir IOT Edge cihaz kimliğini sağ tıklayın. Ardından **IOT Edge cihazı için dağıtım oluşturma**. 
5. Çözümünüzün açın **config** klasör. Ardından `deployment.json` dosya. Seçin **seçin Edge dağıtım bildirimi**. 

Dağıtım kimliği ile bir VS Code tümleşik terminalde başarıyla oluşturuldu. dağıtım görürsünüz.

VS Code Docker Gezgini veya çalıştırarak kapsayıcı durumunuzu denetleme `docker ps` terminalde komutu.

## <a name="start-debugging-c-module-in-vs-code"></a>VS code'da C modülü hata ayıklamayı Başlat
VS Code tutar hata ayıklama yapılandırma bilgilerini bir `launch.json` dosya bulunan bir `.vscode` çalışma alanınızda bir klasör. Bu `launch.json` dosya yeni bir IOT Edge çözümü oluşturduğunuz zaman oluşturulduğu. Bu, her seferinde hata ayıklama destekleyen yeni Modül Ekle güncelleştirir. 

1. VS Code hata ayıklama görünümüne gidin. Bir modül için hata ayıklama yapılandırma dosyasını seçin. Hata ayıklama seçeneği adı şuna benzer olmalıdır **ModuleName Remote Debug (C)**

   ![Select hata ayıklama yapılandırması](./media/how-to-develop-c-module/debug-config.png).

2. `main.c` sayfasına gidin. Bu dosyada kesme noktası ekleyin.

3. Seçin **hata ayıklamayı Başlat** veya **F5**. Ekleme yapılacak işlem seçin.

4. VS Code hata ayıklama Görünümü'nde sol bölmedeki değişkenleri görürsünüz. 

Yukarıdaki örnekte, C IOT Edge modülleri kapsayıcılarına hata ayıklamak gösterilmektedir. Bu, modül kapsayıcı createOptions içinde kullanıma sunulan bağlantı noktası eklendi. Node.js modüllerinizi hata ayıklamasını tamamladığınızda, üretime hazır IOT Edge modülleri için kullanıma sunulan bu bağlantı noktalarını kaldırmanız önerilir.

## <a name="next-steps"></a>Sonraki adımlar

Modülünüzün oluşturduktan sonra bilgi nasıl [Visual Studio code'dan Azure IOT Edge modüllerini dağıtmak](how-to-deploy-modules-vscode.md).

Cihazlarınızı IOT Edge modülleri geliştirmek için [kavrama ve kullanma Azure IOT Hub SDK'ları](../iot-hub/iot-hub-devguide-sdks.md).
