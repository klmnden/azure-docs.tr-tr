---
title: Visual Studio Code ile .NET Core Azure Service Fabric uygulamaları geliştirme | Microsoft Docs
description: Bu makalede, oluşturmak, dağıtmak ve Visual Studio Code kullanarak .NET Core Service Fabric uygulamaları hata ayıklama gösterilmektedir.
services: service-fabric
documentationcenter: .net
author: JimacoMS2
manager: timlt
editor: ''
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2018
ms.author: v-jamebr
ms.openlocfilehash: 27c7c62125f3f559fb1764292729cbbfdc1c4e5f
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37116289"
---
# <a name="develop-c-service-fabric-applications-with-visual-studio-code"></a>Visual Studio Code ile C# Service Fabric uygulamaları geliştirme

[VS Code için Service Fabric Reliable Services Uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-service-fabric-reliable-services) Windows, Linux ve macOS işletim sistemlerinde .NET Core Service Fabric uygulamalar oluşturmanızı kolaylaştırır.

Bu makalede oluşturun, dağıtın ve Visual Studio Code kullanarak bir .NET Core Service Fabric uygulaması hata ayıklama gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

Bu makale, zaten VS Code, VS Code Service Fabric Reliable Services uzantısı ve geliştirme ortamınız için gereken bağımlılıkları yüklediğinizi varsayar. Daha fazla bilgi için bkz: [Başlarken](./service-fabric-get-started-vs-code.md#prerequisites).

## <a name="download-the-sample"></a>Örneği indirme
Bu makalede CounterService uygulamasında kullanan [Service Fabric .NET Core Başlarken örnekleri GitHub deposunu](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started). 

Geliştirme makinenizde depoya kopyalamak için (Windows komut penceresinde) bir terminal penceresinden aşağıdaki komutu çalıştırın:

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started.git
```

## <a name="open-the-application-in-vs-code"></a>Uygulama VS Code'da açın

### <a name="windows"></a>Windows
Başlat menüsü VS Code simgesini sağ tıklatın ve seçin **yönetici olarak çalıştır**. Hata ayıklayıcı hizmetlerinizi eklemek için VS Code yönetici olarak çalıştırmanız gerekir.

### <a name="linux"></a>Linux
Terminal kullanarak, uygulama içinde yerel olarak kopyalandı dizinden /service-fabric-dotnet-core-getting-started/Services/CounterService yoluna gidin.

Hata ayıklayıcı hizmetlerinize ekleyebileceğini kök kullanıcı olarak VS Code açmak için aşağıdaki komutu çalıştırın.
```
sudo code . --user-data-dir='.'
```

Uygulamayı şimdi VS Code çalışma alanınızda gösterilmelidir.

![Çalışma alanında sayaç hizmet uygulaması](./media/service-fabric-develop-csharp-applications-with-vs-code/counter-service-application-in-workspace.png)

## <a name="build-the-application"></a>Uygulama oluşturma
1. Tuşuna (Ctrl + Shift + p) açmak için **komutu palet** VS code'da.
2. Aramak ve seçmek **Service Fabric: derleme uygulama** komutu. Derleme çıktı tümleşik terminal gönderilir.

   ![VS code'da uygulama komutu derleme](./media/service-fabric-develop-csharp-applications-with-vs-code/sf-build-application.png)

## <a name="deploy-the-application-to-the-local-cluster"></a>Uygulamayı yerel kümeye dağıtma
Uygulamayı oluşturduktan sonra yerel kümeye dağıtabilirsiniz. 

1. Gelen **komutu palet**seçin **Service Fabric: uygulama dağıtma (Localhost) komutu**. Yükleme işleminin çıktısı tümleşik terminal gönderilir.

   ![VS code'da uygulama komutu dağıtma](./media/service-fabric-develop-csharp-applications-with-vs-code/sf-deploy-application.png)

4. Dağıtım tamamlandığında, bir tarayıcı başlatmak ve Service Fabric Explorer açın: http://localhost:19080/Explorer. Uygulamanın çalıştığını görmelisiniz. Bu biraz zaman alabilir bu nedenle endişelenmeyin. 

   ![Service Fabric Explorer'da sayaç hizmet uygulaması](./media/service-fabric-develop-csharp-applications-with-vs-code/sfx-verify-deploy.png)

4. Uygulamanın çalıştığı doğrulandıktan sonra bir tarayıcı başlatmak ve bu sayfayı açın: http://localhost:31002. Web uygulamasının ön budur. Sayacın geçerli değeri bu artırır görür için sayfayı yenileyin.

   ![Tarayıcıda sayaç hizmet uygulaması](./media/service-fabric-develop-csharp-applications-with-vs-code/counter-service-running.png)

## <a name="debug-the-application"></a>Uygulamada hata ayıklama
VS kodunu uygulamalarında hata ayıklama sırasında uygulama yerel bir kümede çalıştırması gerekir. Kesme noktaları ardından koda eklenebilir.

Kesme ve hata ayıklama ayarlamak için aşağıdaki adımları tamamlayın:
1. Explorer'da açın */src/CounterServiceApplication/CounterService/CounterService.cs* dosya ve içinde 62 satırında bir kesme noktası belirleyerek `RunAsync` yöntemi.
3. Hata ayıklama simgeyi tıklatın **etkinlik çubuğu** hata ayıklayıcı görünümünü VS Code'da açın. Hata ayıklayıcı görünüm üstündeki dişli simgesine tıklayın ve seçin **.NET Core** açılır ortamı menüsünde. Launch.json'u dosyasını açar. Bu dosyayı kapatabilirsiniz. Şimdi Çalıştır düğmesi (yeşil ok) yanında bulunan hata ayıklama yapılandırması menüde yapılandırma seçeneklerinin görmeniz gerekir.

   ![VS Code çalışma alanında simgesi hata ayıklama](./media/service-fabric-develop-csharp-applications-with-vs-code/debug-icon-workspace.png)

2. Seçin **.NET Core ekleme** hata ayıklama yapılandırması menüsünde.

   ![VS Code çalışma alanında simgesi hata ayıklama](./media/service-fabric-develop-csharp-applications-with-vs-code/debug-start.png)

3. Service Fabric Explorer bir tarayıcıda açın: http://localhost:19080/Explorer. Tıklatın **uygulamaları** ve CounterService çalıştıran birincil düğüm belirlemek için dowwn ayrıntıya gidin. Birincil düğüm CounterService için aşağıdaki görüntüde 0 düğümdür.

   ![Birincil düğüm CounterService için](./media/service-fabric-develop-csharp-applications-with-vs-code/counter-service-primary-node.png)

4. VS Code'da çalışma (yeşil ok) yanındaki simgesini **.NET Core ekleme** hata ayıklama yapılandırması. İşlem seçimi iletişim kutusunda, 4. adımda tanımlanan birincil düğüm üzerinde çalışan CounterService işlemini seçin.

   ![Birincil işlemi](./media/service-fabric-develop-csharp-applications-with-vs-code/select-process.png)

5. Kesme *CounterService.cs* dosya edeceği çok hızlı bir şekilde. Ardından yerel değişkenlerin değerleri da gözden geçirebilirsiniz. Hata ayıklama araç VS Code üstünde yürütme, satırları, yöntemleri, adımla Adımlama devam etmek için geçerli yöntemi dışında adım kullanın. 

   ![Değerleri hata ayıklama](./media/service-fabric-develop-csharp-applications-with-vs-code/breakpoint-hit.png)

6. Hata ayıklama oturumu sona erdirmek için hata ayıklama araç VS Code üstündeki Tak simgeyi tıklatın...
   
   ![Hata Ayıklayıcı'dan bağlantısını kesme](./media/service-fabric-develop-csharp-applications-with-vs-code/debug-bar-disconnect.png)
       
7. Hata ayıklama tamamladığınızda kullanabileceğiniz **Service Fabric: uygulaması Kaldır** yerel kümenizden CounterService uygulamayı kaldırmak için komutu. 

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [geliştirmek ve hata ayıklama Java Service Fabric uygulamaları ile VS Code](./service-fabric-develop-java-applications-with-vs-code.md).



