---
title: Azure IOT köşesi işlevleri modülleri debug | Microsoft Docs
description: C# Azure işlevleri Azure IOT kenarıyla hata ayıklamak için Visual Studio Code kullanma
author: shizn
manager: ''
ms.author: xshi
ms.date: 06/26/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 5ee5d368bde0f176442a01ba266791f8a9c2cbc3
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37052836"
---
# <a name="use-visual-studio-code-to-develop-and-debug-azure-functions-for-azure-iot-edge"></a>Geliştirme ve hata ayıklama için Azure IOT kenar Azure işlevleri için Visual Studio Code kullanın

Bu makalede kullanmaya yönelik ayrıntılı yönergeler sağlanmaktadır [Visual Studio (VS) kod](https://code.visualstudio.com/) IOT sınır, Azure işlevlerini hata ayıklamak için.

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, bir bilgisayar veya geliştirme makine olarak Windows veya Linux çalıştıran sanal makine kullandığınızı varsayar. IOT sınır cihazı başka bir fiziksel aygıt olabilir veya geliştirme makinenizde IOT kenar Cihazınızı benzetimini yapabilirsiniz.

> [!NOTE]
> Hata ayıklama Bu öğretici, bir işlemde bir modül kapsayıcı ekleme ve VS Code ile hata ayıklama açıklar. C# linux amd64 kapsayıcıları modüllerde yalnızca ayıklayabilirsiniz. Visual Studio Code hata ayıklama özellikleri alışık değilseniz, bilgiyi [hata ayıklama](https://code.visualstudio.com/Docs/editor/debugging). 

Bu makalede, Visual Studio Code ana geliştirme aracı olarak kullanır. VS Code yükleyin ve ardından gerekli uzantıları ekleyin: 

* [Visual Studio Code](https://code.visualstudio.com/) 
* [Azure IOT kenar uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [C# uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 
* [Docker uzantısı](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)

Bir modül oluşturma modülü görüntü ve modül görüntüsü tutmak için bir kapsayıcı kayıt defteri oluşturmak için Docker proje klasörü oluşturmak için .NET gerekir:
* [.NET core 2.1 SDK](https://www.microsoft.com/net/download).
* [Docker CE](https://docs.docker.com/install/) geliştirme makinenizde. 
* [Azure kapsayıcı kayıt defteri](https://docs.microsoft.com/azure/container-registry/) veya [Docker hub'a](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)

   > [!TIP]
   > Yerel bir Docker kayıt prototip ve bir bulut kayıt defteri yerine test amacıyla kullanabilirsiniz. 

Modülünüzün bir aygıtta test etmek için etkin bir IOT hub ile en az bir IOT sınır cihazı gerekir. Bilgisayarınızı bir IOT sınır cihazı kullanmak istiyorsanız, bunu öğreticileri için adımları izleyerek yapabilirsiniz [Windows](quickstart.md) veya [Linux](quickstart-linux.md). 

## <a name="create-a-new-solution-template"></a>Yeni bir çözüm şablonu oluşturun

Aşağıdaki adımlar bir C# işlev modülü içeren bir IOT uç çözümünün nasıl oluşturulacağını gösterir. Her bir çözüm için birden fazla modülü içerebilir.

1. Visual Studio kod seçin **Görünüm** > **tümleşik Terminal**.
3. Seçin **Görünüm** > **komutu palet**.
4. Komut paletindeki yazın ve şu komutu çalıştırın **Azure IOT kenar: yeni IOT uç çözümünün**. 

   ![Yeni IOT kenar çözümü çalıştırın](./media/how-to-develop-csharp-module/new-solution.png)

5. Yeni bir çözüm oluşturmak istediğiniz klasöre göz atın ve'ı tıklatın **klasörü seçin**. 
6. Çözümünüz için bir ad sağlayın. 
7. Seçin **Azure işlevleri - C#** çözümdeki ilk modülü için şablon olarak.
8. Modül için bir ad sağlayın. Kapsayıcı kaydınız içinde benzersiz bir ad seçin. 
9. Görüntü deposu için modülü sağlar. VS Code autopopulates modül adı, böylece değiştirmek zorunda **localhost:5000** kendi kayıt defteri bilgileri. Test etmek için bir yerel Docker kayıt defteri kullanırsanız, localhost sorun yoktur. Azure kapsayıcı kayıt defteri kullanırsanız, kayıt defterindeki ayarlar oturum açma sunucudan kullanın. Oturum açma sunucusu gibi görünüyor  **\<kayıt defteri adı\>. azurecr.io**.

VS Code sağlanan işlevi projeyle IOT kenar çözümünü oluşturur, ardından yeni bir pencerede yükler bilgilerini alır.

Çözüm içinde üç öğe vardır: 

* A **.vscode** hata ayıklama yapılandırmaları içeren klasör.
* A **modülleri** her modül için alt klasörler içeren klasör. Bir yalnızca vardır, ancak daha fazla komut palet komutuyla aracılığıyla ekleyebilirsiniz hemen **Azure IOT kenar: IOT kenar Modül Ekle**.
* A **.env** dosyası ortam değişkenlerini listeler. ACR kullanıcı adı ve parola içinde sahip artık kaydınız, doğru ACR varsa. 
* A **deployment.template.json** dosyası listeler, yeni bir örnek modülüyle **tempSensor** test etmek için kullanabileceğiniz veri benzetim modülü. Dağıtım iş nasıl bildirimleri hakkında daha fazla bilgi için bkz: [nasıl IOT kenar modülleri, yapılandırılmış, yeniden ve kullanılabilecek anlamak](module-composition.md).

## <a name="build-your-iot-edge-function-module-for-debugging-purpose"></a>Amaç hata ayıklama için IOT kenar işlevi modülünüzün derleme
1. Hata ayıklamayı başlatmak için kullanmanız gerekir **Dockerfile.amd64.debug** docker görüntünüzü oluşturmak ve kenar çözümünüzü yeniden dağıtmak için. VS Code Explorer'da gidin `deployment.template.json` dosya. İşlev resim URL'si ekleyerek güncelleştirme bir `.debug` uçtaki.

    ![Hata ayıklama yansıması oluştur](./media/how-to-debug-csharp-function/build-debug-image.png)

2. Çözümü yeniden derleyin. VS Code komutu palette yazın ve şu komutu çalıştırın **Azure IOT kenar: derleme IOT uç çözümünün**.
3. Azure IOT Hub cihazları Gezgini'nde bir IOT kenar cihaz kimliği sağ tıklayın ve ardından seçin **sınır cihazı için dağıtımı oluşturma**. Seçin `deployment.json` altında dosya `config` klasör. Daha sonra dağıtım başarıyla VS code'da kimliği tümleşik bir dağıtımı ile terminal oluşturulan görebilirsiniz.

Kapsayıcı durumunuzu VS Code Docker Gezgini'nde veya çalıştırarak denetleyebilirsiniz `docker images` Terminal komutu.

## <a name="start-debugging-c-function-in-vs-code"></a>C# VS Code işlevinde hata ayıklamayı Başlat
1. VS Code tutar hata ayıklama yapılandırma bilgilerini bir `launch.json` içinde bulunan dosyasını bir `.vscode` çalışma alanınızdaki klasör. Bu `launch.json` dosya üretilen yeni bir IOT kenar çözüm oluşturduğunuzda. Hata ayıklama destekleyen yeni bir modül eklediğiniz her zaman güncelleştirir. Hata ayıklama görünümüne gidin ve karşılık gelen hata ayıklama yapılandırma dosyasını seçin. Hata ayıklama seçeneği adı gibi olmalıdır **ModuleName uzaktan hata ayıklama (.NET Core)** ![Select hata ayıklama yapılandırması](./media/how-to-debug-csharp-function/select-debug-configuration.jpg)

2. `run.csx` sayfasına gidin. Bir kesme noktası işlevinde ekleyin.
3. Tıklatın **hata ayıklamayı Başlat** düğmesini veya tuşuna **F5**ve ekleme işlemini seçin.
4. VS kodda hata ayıklama Görünümü'nde sol panelinde değişkenleri görebilirsiniz. 


> [!NOTE]
> Yukarıdaki örnekte gösterildiği hata ayıklama .net nasıl kapsayıcılarında çekirdek IOT kenar işlevi. Hata ayıklama sürümünde dayalı `Dockerfile.amd64.debug`, içeren VSDBG (.NET Core komut satırı hata ayıklayıcı) kapsayıcı yansımanıza derlenirken hatalarla. Doğrudan kullanabilir veya özelleştirebilirsiniz öneririz `Dockerfile` C# işlevinizi hata ayıklama işlemini tamamladıktan sonra üretime hazır IOT kenar işlevi için VSDBG olmadan.

## <a name="next-steps"></a>Sonraki adımlar

Yerleşik modülünüzün olduktan sonra bilgi nasıl [Visual Studio Code modüllerden Azure IOT kenar dağıtma](how-to-deploy-modules-vscode.md)
