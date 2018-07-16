---
title: Azure işlev modülleri, Azure IOT Edge için hata ayıklama | Microsoft Docs
description: C# Azure işlevleri Azure IOT Edge ile hata ayıklamak için Visual Studio Code'u kullanma
author: shizn
manager: ''
ms.author: xshi
ms.date: 06/26/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 3176a3a4acc6e9ca486d409d861f2ed0e63473ec
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39056513"
---
# <a name="use-visual-studio-code-to-develop-and-debug-azure-functions-for-azure-iot-edge"></a>Geliştirme ve Azure işlevleri Azure IOT Edge için hata ayıklama için Visual Studio Code'u kullanın

Bu makalede nasıl kullanılacağını gösterir [Visual Studio Code (VS Code)](https://code.visualstudio.com/) , Azure işlevleri Azure IOT Edge üzerinde hata ayıklamak için.

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, bir bilgisayar veya geliştirme makinenize Windows veya Linux çalıştıran sanal makine kullandığınızı varsayar. IOT Edge Cihazınızı başka bir fiziksel cihaz olabilir. Veya IOT Edge Cihazınızı geliştirme makinenizde benzetimini yapabilirsiniz.

> [!NOTE]
> Hata ayıklama bu makalede, bir modül kapsayıcıdaki bir sürece iliştirilip ve VS Code ile hata ayıklama gösterilmektedir. Yalnızca Linux amd64-kapsayıcılarında modüller C# hata ayıklama yapabilirsiniz. Visual Studio Code ile hata ayıklama özelliklerine aşina değilseniz okuyun [hata ayıklama](https://code.visualstudio.com/Docs/editor/debugging). 

Bu makalede, Visual Studio Code ana geliştirme aracı olarak kullanır. VS Code yükleme. Daha sonra gerekli genişletmeleri ekleyin: 

* [Visual Studio Code](https://code.visualstudio.com/) 
* [Azure IOT Edge uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [C# uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 
* [Docker uzantısı](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)

Bir modülü oluşturmak için Docker modülü görüntüsü ve modülü görüntüsü tutmak için bir kapsayıcı kayıt defteri oluşturmak için proje klasörünü oluşturmak için .NET gerekir:
* [.NET core 2.1 SDK'sı](https://www.microsoft.com/net/download)
* [Docker Community Edition](https://docs.docker.com/install/) geliştirme makinenizde 
* [Azure kapsayıcı kayıt defteri](https://docs.microsoft.com/azure/container-registry/) veya [Docker hub'ı](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)

   > [!TIP]
   > Prototip ve test etme amacıyla bir bulut kayıt defteri yerine yerel bir Docker kayıt defteri kullanabilirsiniz. 

Modülünüzün bir cihazda test etmek için etkin bir IOT hub ile en az bir IOT Edge cihazı gerekir. Bilgisayarınızı bir IOT Edge cihazı kullanmak için hızlı başlangıç için adımları izleyin. [Windows](quickstart.md) veya [Linux](quickstart-linux.md). 

## <a name="create-a-new-solution-template"></a>Yeni bir çözüm şablonu oluşturma

Bir C# işlev modülü olan bir IOT Edge çözümü oluşturmak için bu adımları uygulayın. Her çözüm, birden çok modül olabilir.

1. Visual Studio Code'da seçin **görünümü** > **tümleşik Terminalini**.
3. Seçin **görünümü** > **komut paleti**.
4. Komut Paleti'nde girin ve şu komutu çalıştırın **Azure IOT Edge: IOT Edge çözüm yeni**. 

   ![Yeni IOT Edge çözümü çalıştırın](./media/how-to-develop-csharp-module/new-solution.png)

5. Yeni çözümü oluşturmak istediğiniz klasöre göz atın. Seçin **klasörü seçin**. 
6. Çözümünüz için bir ad girin. 
7. Seçin **- Azure işlevleri C#** çözüm içinde ilk modül için şablon olarak.
8. Bir modül için bir ad girin. Kapsayıcı kayıt defterinizde içinde benzersiz bir ad seçin. 
9. Görüntü deposu için modülü sağlar. VS Code autopopulates modül adı ile **localhost:5000**. Kayıt defteri kendi bilgilerinizle değiştirin. Yerel bir Docker kayıt defteri test, ardından kullanıyorsanız **localhost** bir sakınca yoktur. Azure Container Registry kullanırsanız, oturum açma sunucusu defterinizin ayarlarından'ni kullanın. Oturum açma sunucusu benzer  **\<kayıt defteri adı\>. azurecr.io**.

VS Code, sağlanan bir IOT Edge çözümü ile bir Azure işlev projesi oluşturur ve ardından yeni bir pencerede yüklenir bilgilerini alır.

Çözüm içinde dört öğe vardır: 

* A **.vscode** klasörü, hata ayıklama yapılandırmaları içerir.
* A **modülleri** klasörü her modül için alt klasörler bulunur. Bu noktada yalnızca biri gerekir. Ancak daha fazla komut paletini komutu aracılığıyla ekleyebilirsiniz **Azure IOT Edge: IOT Edge Modülü Ekle**.
* Bir **.env** dosyası, ortam değişkenleri listeler. Azure Container Registry kaydınız varsa, bir Azure kapsayıcı kayıt defteri kullanıcı adı ve parola bulunması. 

   >[!NOTE]
   >Modül için bir görüntü deposuna sağlarsanız, ortam dosyası yalnızca oluşturulur. Test ve yerel olarak hata ayıklama için localhost Varsayılanları kabul ortam değişkenleri gerekmez. 

* A **deployment.template.json** dosyası listeler, yeni bir örnek modülüyle **tempSensor** veri benzetimi gerçekleştiren modülü test etmek için kullanabilirsiniz. Nasıl iş dağıtım bildirimleri hakkında daha fazla bilgi için bkz. [modülleri dağıtma ve yollar kurmak için dağıtım bildirimleri kullanmayı öğrenin](module-composition.md).

## <a name="devlop-your-module"></a>Devlop modülünüzde

Çözümünüzle birlikte gelen varsayılan Azure işlevi kodu şu konumdadır **modülleri** > **\<, modül adı\>**   >   **EdgeHubTrigger-Csharp** > **run.csx**. Çözümü derleyin, kapsayıcı kayıt defterinize itme ve herhangi bir kod dokunmadan testi başlatmak için bir aygıta dağıtmak modülü ve deployment.template.json dosya ayarlanır. Modül, yalnızca bir kaynak (Bu durumda, veri benzetimi gerçekleştiren tempSensor Modülü) gelenlerin ve IOT Hub'ına kanal için oluşturulmuştur. 

Kendi kod ile Azure işlevi şablonu özelleştirmek hazır olduğunuzda kullanın [Azure IOT Hub SDK'ları](../iot-hub/iot-hub-devguide-sdks.md) anahtar gereken güvenlik, cihaz yönetimi ve güvenilirlik gibi IOT çözümleri için bu adrese modüller oluşturmak için. 

## <a name="build-your-module-for-debugging"></a>Hata ayıklama için modülünüzde oluşturun
1. Hata ayıklamayı başlatmak için kullanmak **Dockerfile.amd64.debug** docker görüntünüzü yeniden oluşturun ve Edge çözümünüzü yeniden dağıtın. VS Code Gezgininde gidin `deployment.template.json` dosya. Ekleyerek, işlev görüntü URL'sini güncelleştirme `.debug` sonuna.

    ![Hata ayıklama görüntüsü oluşturma](./media/how-to-debug-csharp-function/build-debug-image.png)

2. Çözümünüzü yeniden oluşturun. VS Code komut paleti girin ve şu komutu çalıştırın **Azure IOT Edge: IOT Edge çözüm**.
3. Azure IOT Hub cihazları Gezgini'nde, bir IOT Edge cihaz Kimliğine sağ tıklayın ve ardından **Edge cihazı için dağıtım oluşturma**. Seçin `deployment.json` dosyası `config` klasör. Dağıtım kimliği ile bir VS Code tümleşik terminalde başarıyla oluşturuldu. dağıtım görürsünüz.

VS Code Docker Gezgini veya çalıştırarak kapsayıcı durumunuzu denetleme `docker images` terminalde komutu.

## <a name="start-debugging-c-functions-in-vs-code"></a>C# işlevlerini VS code'da hata ayıklamayı Başlat
1. VS Code tutar hata ayıklama yapılandırma bilgilerini bir `launch.json` dosya bulunan bir `.vscode` çalışma alanınızda bir klasör. Bu `launch.json` dosya yeni bir IOT Edge çözümü oluşturduğunuz zaman oluşturulduğu. Bu, her seferinde hata ayıklama destekleyen yeni Modül Ekle güncelleştirir. Hata ayıklama görünümüne gidin. İlgili hata ayıklama yapılandırma dosyasını seçin. Hata ayıklama seçeneği adı şuna benzer olmalıdır **ModuleName uzaktan hata ayıklama (.NET Core)**.

   ![Select hata ayıklama yapılandırması](./media/how-to-debug-csharp-function/select-debug-configuration.jpg)

2. `run.csx` sayfasına gidin. İşlev bir kesme noktası ekleyin.
3. Seçin **hata ayıklamayı Başlat** veya **F5**. Ekleme yapılacak işlem seçin.
4. VS Code hata ayıklama Görünümü'nde sol bölmedeki değişkenleri görürsünüz. 


> [!NOTE]
> Bu örnekte,.Net Core hata ayıklamak gösterilmektedir kapsayıcılarına IOT Edge işlevleri. Hata ayıklama sürümünde temel `Dockerfile.amd64.debug`, içeren .NET Core komut satırı hata ayıklayıcı VSDBG kapsayıcı görüntünüzü oluşturma sırasında. Doğrudan kullanabilir veya özelleştirebilirsiniz öneririz `Dockerfile` VSDBG için üretime hazır olmadan, C# işlevleri hata ayıklama sonra IOT Edge çalışır.

## <a name="next-steps"></a>Sonraki adımlar

Modülünüzün oluşturduktan sonra bilgi nasıl [Visual Studio Code için Azure IOT Edge'e dağıtma modüllerden](how-to-deploy-modules-vscode.md).

Cihazlarınızı IOT Edge modülleri geliştirmek için [kavrama ve kullanma Azure IOT Hub SDK'ları](../iot-hub/iot-hub-devguide-sdks.md).
