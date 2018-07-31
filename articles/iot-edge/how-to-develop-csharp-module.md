---
title: C# modülleri, Azure IOT Edge için hata ayıklama | Microsoft Docs
description: Geliştirme, oluşturma ve Azure IOT Edge için bir C# modülü hata ayıklamak için Visual Studio Code kullanın
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 06/27/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: a895f21bc061763b1d5d45b2bedb44fc932190dc
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39345234"
---
# <a name="use-visual-studio-code-to-develop-and-debug-c-modules-for-azure-iot-edge"></a>Geliştirme ve C# modülleri, Azure IOT Edge için hata ayıklama için Visual Studio Code'u kullanın

İçin Azure IOT Edge modülleri, iş mantığınızı kapatabilirsiniz. Bu makalede Visual Studio Code (VS Code) ana aracı olarak geliştirme ve modüller C# hata ayıklama için nasıl kullanılacağını gösterir.

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, bir bilgisayar veya geliştirme makinenize Windows veya Linux çalıştıran sanal makine kullandığınızı varsayar. IOT Edge Cihazınızı başka bir fiziksel cihaz olabilir. Veya IOT Edge Cihazınızı geliştirme makinenizde benzetimini yapabilirsiniz.

> [!NOTE]
> Hata ayıklama bu makalede, bir modül kapsayıcıdaki bir sürece iliştirilip ve VS Code ile hata ayıklama gösterilmektedir. Yalnızca Linux amd64-kapsayıcılarında modüller C# hata ayıklama yapabilirsiniz. Visual Studio Code ile hata ayıklama özelliklerine aşina değilseniz okuyun [hata ayıklama](https://code.visualstudio.com/Docs/editor/debugging). 

Bu makalede ana geliştirme aracı olarak Visual Studio Code kullandığından, VS Code yükleme. Daha sonra gerekli genişletmeleri ekleyin:
* [Visual Studio Code](https://code.visualstudio.com/) 
* [Azure IOT Edge uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [C# uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 
* [Docker uzantısı](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)

Bir modülü oluşturmak için Docker modülü görüntüsü ve modülü görüntüsü tutmak için bir kapsayıcı kayıt defteri oluşturmak için proje klasörünü oluşturmak için .NET gerekir:
* [.NET Core 2.1 SDK'sı](https://www.microsoft.com/net/download).
* [Docker Community Edition](https://docs.docker.com/install/) geliştirme makinenizde. 
* [Azure kapsayıcı kayıt defteri](https://docs.microsoft.com/azure/container-registry/) veya [Docker hub'ı](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)

   > [!TIP]
   > Prototip ve test etme amacıyla bir bulut kayıt defteri yerine yerel bir Docker kayıt defteri kullanabilirsiniz. 

Modülünüzün bir cihazda test etmek için etkin bir IOT hub ile en az bir IOT Edge cihazı gerekir. Bilgisayarınızı bir IOT Edge cihazı kullanmak için hızlı başlangıç için adımları izleyin. [Windows](quickstart.md) veya [Linux](quickstart-linux.md). 

## <a name="create-a-new-solution-template"></a>Yeni bir çözüm şablonu oluşturma

Visual Studio Code ve Azure IOT Edge uzantısını kullanarak .NET Core 2.0 üzerinde dayalı bir IOT Edge modülünüzü oluşturmak için bu adımları uygulayın. İlk olarak, bir çözüm oluşturun ve ardından ilk Modül içindeki çözümü oluşturun. Her çözüm, birden fazla modülü içerebilir. 

1. Visual Studio Code'da seçin **görünümü** > **tümleşik Terminalini**.
3. Seçin **görünümü** > **komut paleti**. 
4. Komut Paleti'nde girin ve şu komutu çalıştırın **Azure IOT Edge: IOT Edge çözüm yeni**.

   ![Yeni IOT Edge çözümü çalıştırın](./media/how-to-develop-csharp-module/new-solution.png)

5. Yeni çözümü oluşturmak istediğiniz klasöre göz atın. Seçin **klasörü seçin**. 
6. Çözümünüz için bir ad girin. 
7. Seçin **C# Modülü** çözüm içinde ilk modül için şablon olarak.
8. Bir modül için bir ad girin. Kapsayıcı kayıt defterinizde içinde benzersiz bir ad seçin. 
9. Modülün görüntü deposu adını sağlayın. VS Code autopopulates modül adı ile **localhost:5000**. Kayıt defteri kendi bilgilerinizle değiştirin. Yerel bir Docker kayıt defteri test, ardından kullanıyorsanız **localhost** bir sakınca yoktur. Azure Container Registry kullanırsanız, oturum açma sunucusu defterinizin ayarlarından'ni kullanın. Oturum açma sunucusu benzer  **\<kayıt defteri adı\>. azurecr.io**.

VS Code, sağlanan bir IOT Edge çözümü oluşturur ve ardından yeni bir pencerede yüklenir bilgilerini alır.

   ![IOT Edge çözümünü görüntüleme](./media/how-to-develop-csharp-module/view-solution.png)

Çözüm içinde dört öğe vardır: 
* A **.vscode** klasörü, hata ayıklama yapılandırmaları içerir.
* A **modülleri** klasörü her modül için alt klasörler bulunur. Bu noktada yalnızca biri gerekir. Ancak daha komutuyla komut Paleti'nde ekleyebilirsiniz **Azure IOT Edge: IOT Edge Modülü Ekle**. 
* Bir **.env** dosyası, ortam değişkenleri listeler. Azure Container Registry kaydınız varsa, bir Azure kapsayıcı kayıt defteri kullanıcı adı ve parola bulunması. 

   >[!NOTE]
   >Modül için bir görüntü deposuna sağlarsanız, ortam dosyası yalnızca oluşturulur. Test ve yerel olarak hata ayıklama için localhost Varsayılanları kabul ortam değişkenleri gerekmez. 

* A **deployment.template.json** dosyası listeler, yeni bir örnek modülüyle **tempSensor** veri benzetimi gerçekleştiren modülü test etmek için kullanabilirsiniz. Nasıl iş dağıtım bildirimleri hakkında daha fazla bilgi için bkz. [modülleri dağıtma ve yollar kurmak için dağıtım bildirimleri kullanmayı öğrenin](module-composition.md). 

## <a name="devlop-your-module"></a>Devlop modülünüzde

Çözümünüzle birlikte gelen varsayılan Azure işlevi kodu şu konumdadır **modülleri** > **\<, modül adı\>**   >   **Program.cs**. Çözümü derleyin, kapsayıcı kayıt defterinize itme ve herhangi bir kod dokunmadan testi başlatmak için bir aygıta dağıtmak modülü ve deployment.template.json dosya ayarlanır. Modül, yalnızca bir kaynak (Bu durumda, veri benzetimi gerçekleştiren tempSensor Modülü) gelenlerin ve IOT Hub'ına kanal için oluşturulmuştur. 

Kendi kodunuzu ile C# şablonunu özelleştirmek hazır olduğunuzda kullanın [Azure IOT Hub SDK'ları](../iot-hub/iot-hub-devguide-sdks.md) anahtar gereken güvenlik, cihaz yönetimi ve güvenilirlik gibi IOT çözümleri için bu adrese modüller oluşturmak için. 

## <a name="build-and-deploy-your-module-for-debugging"></a>Derleme ve hata ayıklama için modülünüzde dağıtma

Her modül klasöründe birkaç Docker dosya için farklı bir kapsayıcı türü vardır. Uzantısıyla biten bu dosyaları dilediğinizi **.debug** test etmek için modülü. Şu anda C# Modüller yalnızca Linux amd64 kapsayıcılarında hata ayıklama desteği.

1. VS Code'da gidin `deployment.template.json` dosya. Ekleyerek, işlev görüntü URL'sini güncelleştirme **.debug** sonuna.

   ![Ekleme *** .debug, görüntü adı](./media/how-to-develop-csharp-module/image-debug.png)

2. VS Code komut paleti girin ve şu komutu çalıştırın **Edge: IOT Edge çözüm**.
3. Seçin `deployment.template.json` komut paletini çözümünüzden dosyası. 
4. Azure IOT hub'ı Device Explorer içinde bir IOT Edge cihaz kimliğini sağ tıklayın. Ardından **IOT Edge cihazı için dağıtım oluşturma**. 
5. Çözümünüzün açın **config** klasör. Ardından `deployment.json` dosya. Seçin **seçin Edge dağıtım bildirimi**. 

Dağıtım kimliği ile bir VS Code tümleşik terminalde başarıyla oluşturuldu. dağıtım görürsünüz.

VS Code Docker Gezgini veya çalıştırarak kapsayıcı durumunuzu denetleme `docker ps` terminalde komutu.

## <a name="start-debugging-c-module-in-vs-code"></a>C# modülü VS code'da hata ayıklamayı Başlat
VS Code tutar hata ayıklama yapılandırma bilgilerini bir `launch.json` dosya bulunan bir `.vscode` çalışma alanınızda bir klasör. Bu `launch.json` dosya yeni bir IOT Edge çözümü oluşturduğunuz zaman oluşturulduğu. Bu, her seferinde hata ayıklama destekleyen yeni Modül Ekle güncelleştirir. 

1. VS Code hata ayıklama görünümüne gidin. Bir modül için hata ayıklama yapılandırma dosyasını seçin. Hata ayıklama seçeneği adı şuna benzer olmalıdır **ModuleName uzaktan hata ayıklama (.NET Core)**

   ![Select hata ayıklama yapılandırması](./media/how-to-develop-csharp-module/debug-config.png).

2. `program.cs` sayfasına gidin. Bu dosyada kesme noktası ekleyin.

3. Seçin **hata ayıklamayı Başlat** veya **F5**. Ekleme yapılacak işlem seçin.

4. VS Code hata ayıklama Görünümü'nde sol bölmedeki değişkenleri görürsünüz. 

> [!NOTE]
> Bu örnek, .NET Core IOT Edge modülleri kapsayıcılarına hata ayıklamak nasıl gösterir. Hata ayıklama sürümünde temel `Dockerfile.debug`, içeren .NET Core komut satırı hata ayıklayıcı VSDBG kapsayıcı görüntünüzü oluşturma sırasında. Doğrudan kullanabilir veya özelleştirebilirsiniz C# modüllerinizi hata ayıklama sonra öneririz `Dockerfile` VSDBG üretime hazır IOT Edge modülleri için olmadan.

## <a name="next-steps"></a>Sonraki adımlar

Modülünüzün oluşturduktan sonra bilgi nasıl [Visual Studio code'dan Azure IOT Edge modüllerini dağıtmak](how-to-deploy-modules-vscode.md).

Cihazlarınızı IOT Edge modülleri geliştirmek için [kavrama ve kullanma Azure IOT Hub SDK'ları](../iot-hub/iot-hub-devguide-sdks.md).
