---
title: C# modülleri için Azure IOT kenar hata ayıklama | Microsoft Docs
description: Geliştirme, derleme ve Azure IOT köşesi bir C# modülü hata ayıklama için Visual Studio Code kullanın
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 06/27/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: c237b1eb9874357afa922f809109cf8f2181561e
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37048195"
---
# <a name="use-visual-studio-code-to-develop-and-debug-c-modules-for-azure-iot-edge"></a>Visual Studio Code geliştirmek ve C# modülleri için Azure IOT kenar hata ayıklamak için kullanın

Azure IOT köşesi modüllerine kapatarak sınırında çalışmak için iş mantığınızı gönderebilirsiniz. Bu makalede, C# modülleri geliştirmek için Visual Studio Code (VS Code) ana geliştirme aracı olarak kullanmak için ayrıntılı yönergeler sağlar.

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, bir bilgisayar veya geliştirme makine olarak Windows veya Linux çalıştıran sanal makine kullandığınızı varsayar. Geliştirme makinenizde IOT kenar Cihazınızı benzetimini yapabilirsiniz veya başka bir fiziksel cihaz IOT sınır cihazı olabilir.

> [!NOTE]
> Hata ayıklama Bu öğretici, bir işlemde bir modül kapsayıcı ekleme ve VS Code ile hata ayıklama açıklar. C# işlevleri linux amd64 kapsayıcılarında yalnızca ayıklayabilirsiniz. Visual Studio Code hata ayıklama özellikleri alışık değilseniz, bilgiyi [hata ayıklama](https://code.visualstudio.com/Docs/editor/debugging). 

Bu makalede, Visual Studio Code ana geliştirme aracı olarak kullandığından, VS Code yükleyin ve ardından gerekli uzantıları ekleyin:
* [Visual Studio Code](https://code.visualstudio.com/) 
* [Azure IOT kenar uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [C# uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 
* [Docker uzantısı](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)

Bir modül oluşturmak için proje klasörünü modülü görüntü ve modül görüntüsü tutmak için bir kapsayıcı kayıt defteri oluşturmak için Docker derlemeler .NET gerekir:
* [.NET core 2.1 SDK](https://www.microsoft.com/net/download).
* [Docker CE](https://docs.docker.com/install/) geliştirme makinenizde. 
* [Azure kapsayıcı kayıt defteri](https://docs.microsoft.com/azure/container-registry/) veya [Docker hub'a](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)

   > [!TIP]
   > Yerel bir Docker kayıt prototip ve bir bulut kayıt defteri yerine test amacıyla kullanabilirsiniz. 

Modülünüzün bir aygıtta test etmek için etkin bir IOT hub ile en az bir IOT sınır cihazı gerekir. Bilgisayarınızı bir IOT sınır cihazı kullanmak istiyorsanız, bunu öğreticileri için adımları izleyerek yapabilirsiniz [Windows](quickstart.md) veya [Linux](quickstart-linux.md) 

## <a name="create-a-new-solution-template"></a>Yeni bir çözüm şablonu oluşturun

Aşağıdaki adımları Visual Studio Code ve Azure IOT kenar uzantısını kullanarak nasıl .NET Core 2.0 dayalı bir IOT kenar modülü oluşturulacağını gösterir. Bir çözüm oluşturarak ve ardından bu çözümde ilk modülü oluşturma başlatın. Her bir çözüm için birden fazla modülü içerebilir. 

1. Visual Studio kod seçin **Görünüm** > **tümleşik Terminal**.
3. Seçin **Görünüm** > **komutu palet**. 
4. Komut paletindeki yazın ve şu komutu çalıştırın **Azure IOT kenar: yeni IOT uç çözümünün**.

   ![Yeni IOT kenar çözümü çalıştırın](./media/how-to-develop-csharp-module/new-solution.png)

5. Yeni bir çözüm oluşturmak istediğiniz klasöre göz atın ve'ı tıklatın **klasörü seçin**. 
6. Çözümünüz için bir ad sağlayın. 
7. Seçin **C# modül** çözümdeki ilk modülü için şablon olarak.
8. Modül için bir ad sağlayın. Kapsayıcı kaydınız içinde benzersiz bir ad seçin. 
9. Görüntü deposu için modülü sağlar. VS Code autopopulates modül adı, böylece değiştirmek zorunda **localhost:5000** kendi kayıt defteri bilgileri. Test etmek için bir yerel Docker kayıt defteri kullanırsanız, localhost sorun yoktur. Azure kapsayıcı kayıt defteri kullanırsanız, kayıt defterindeki ayarlar oturum açma sunucudan kullanın. Oturum açma sunucusu gibi görünüyor  **\<kayıt defteri adı\>. azurecr.io**.

VS Code sağlanan IOT kenar çözümünü oluşturur, ardından yeni bir pencerede yükler bilgilerini alır.

   ![IOT kenar çözümü görüntüle](./media/how-to-develop-csharp-module/view-solution.png)

Çözüm içinde üç öğe vardır: 
* A **.vscode** klasörü, hata ayıklama yapılandırmaları içerir.
* A **modülleri** klasörü, her modül için alt klasörler içerir. Bir yalnızca vardır, ancak daha komutuyla komutu paletindeki ekleyebilirsiniz hemen **Azure IOT kenar: IOT kenar Modül Ekle**. 
* A **.env** dosyası ortam değişkenlerini listeler. ACR kullanıcı adı ve parola içinde sahip artık kaydınız, doğru ACR varsa. 
* A **deployment.template.json** dosyası listeler, yeni bir örnek modülüyle **tempSensor** test etmek için kullanabileceğiniz veri benzetim modülü. Dağıtım iş nasıl bildirimleri hakkında daha fazla bilgi için bkz: [nasıl IOT kenar modülleri, yapılandırılmış, yeniden ve kullanılabilecek anlamak](module-composition.md).

## <a name="build-and-deploy-your-module-for-debugging"></a>Derleme ve hata ayıklama için modülünüzün dağıtma

Her modül klasöründe farklı kapsayıcı türleri için birden çok Docker dosyası vardır. Uzantısıyla biten bu dosyalar kullanabileceğiniz **.debug** test etmek için modülü oluşturmak. Şu anda, C# Modüller yalnızca linux amd64 kapsayıcılarında hata ayıklama destekler.

1. VS Code'da gidin `deployment.template.json` dosya. İşlev resim URL'si ekleyerek güncelleştirme **.debug** sonuna.

   ![Görüntü adınızı .debug ekleme](./media/how-to-develop-csharp-module/image-debug.png)

2. VS Code komutu palette yazın ve şu komutu çalıştırın **kenar: derleme IOT uç çözümünün**.
3. Seçin `deployment.template.json` çözümünüzü command paletindeki dosya. 
4. Azure IOT Hub cihazları Gezgini'nde bir IOT kenar cihaz kimliği sağ tıklayın ve ardından seçin **IOT sınır cihazı için dağıtımı oluşturma**. 
5. Açık **config** klasörü, çözümün seçip `deployment.json` dosya. Tıklatın **seçin kenar dağıtım bildirimi**. 

Daha sonra dağıtım başarıyla VS code'da kimliği tümleşik bir dağıtımı ile terminal oluşturulan görebilirsiniz.

Kapsayıcı durumunuzu VS Code Docker Gezgini'nde veya Çalıştır tarafından kontrol edebilirsiniz `docker ps` Terminal komutu.

## <a name="start-debugging-c-module-in-vs-code"></a>C# VS Code modülünde hata ayıklamayı Başlat
VS Code tutar hata ayıklama yapılandırma bilgilerini bir `launch.json` içinde bulunan dosyasını bir `.vscode` çalışma alanınızdaki klasör. Bu `launch.json` dosya üretilen yeni bir IOT kenar çözüm oluşturduğunuzda. Hata ayıklama destekleyen yeni bir modül eklediğiniz her zaman güncelleştirir. 

1. VS Code hata ayıklama görünümüne gidin ve modülünüzün için hata ayıklama yapılandırma dosyası seçin. Hata ayıklama seçeneği adı gibi olmalıdır **ModuleName uzaktan hata ayıklama (.NET Core)** ![Select hata ayıklama yapılandırması](./media/how-to-develop-csharp-module/debug-config.png)

2. `program.cs` sayfasına gidin. Bu dosyada bir kesme noktası ekleyin.

3. Tıklatın **hata ayıklamayı Başlat** düğmesini veya tuşuna **F5**ve ekleme işlemini seçin.

4. VS kodda hata ayıklama Görünümü'nde sol panelinde değişkenleri görebilirsiniz. 

> [!NOTE]
> Önceki örnekte, .NET Core IOT kenar modülleri kapsayıcılarında hata ayıklama gösterilmektedir. Hata ayıklama sürümünde dayalı `Dockerfile.debug`, içeren VSDBG (.NET Core komut satırı hata ayıklayıcı) kapsayıcı yansımanıza derlenirken hatalarla. C# modüllerinizi hata ayıklama tamamladıktan sonra doğrudan kullanabilir veya özelleştirebilirsiniz öneririz `Dockerfile` VSDBG üretime hazır IOT kenar modüller olmadan.

## <a name="next-steps"></a>Sonraki adımlar

Yerleşik modülünüzün olduktan sonra bilgi nasıl [Visual Studio Code modüllerden Azure IOT kenar dağıtma](how-to-deploy-modules-vscode.md)

