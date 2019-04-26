---
title: Visual Studio Code ile .NET Core Azure Service Fabric uygulamalar geliştirin | Microsoft Docs
description: Bu makale oluşturmanızı, dağıtmanızı ve .NET Core Service Fabric uygulamaları Visual Studio Code kullanarak hata ayıklama işlemini gösterir.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2018
ms.author: pepogors
ms.openlocfilehash: 680c141e32333c4747ee69919229bd9381f536a4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60393798"
---
# <a name="develop-c-service-fabric-applications-with-visual-studio-code"></a>Geliştirme C# Visual Studio Code ile Service Fabric uygulamaları

[VS Code için Service Fabric güvenilir hizmetler uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-service-fabric-reliable-services) işletim sistemlerinde Windows, Linux ve Macos'ta .NET Core Service Fabric uygulamaları oluşturmayı kolaylaştırır.

Bu makalede, oluşturmanızı, dağıtmanızı ve .NET Core Service Fabric uygulamasını Visual Studio Code kullanarak hata ayıklama işlemini göstermektedir.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede, zaten VS Code, VS Code için Service Fabric güvenilir hizmetler uzantısı ve geliştirme ortamınız için gereken herhangi bir bağımlılığın yüklediğinizi varsayar. Daha fazla bilgi için bkz. [Başlarken](./service-fabric-get-started-vs-code.md#prerequisites).

## <a name="download-the-sample"></a>Örneği indirme
Bu makalede CounterService uygulamada kullandığı [Service Fabric .NET Core kullanmaya başlama örnekleri GitHub deposunda](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started). 

Depoyu geliştirme makinenize kopyalamak için (Windows komut penceresinde) bir terminal penceresinden aşağıdaki komutu çalıştırın:

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started.git
```

## <a name="open-the-application-in-vs-code"></a>Uygulama VS Code'da açın

### <a name="windows"></a>Windows
Başlat menüsündeki VS Code simgesini sağ tıklatın ve seçin **yönetici olarak çalıştır**. Hizmetlerinize hata ayıklayıcıyı iliştirmek için VS Code'u yönetici olarak çalıştırmanız gerekir.

### <a name="linux"></a>Linux
Terminal kullanarak, uygulama içinde yerel olarak kopyalanan dizinden /service-fabric-dotnet-core-getting-started/Services/CounterService yoluna gidin.

Hizmetlerinize hata ayıklayıcıyı iliştirmek bir kök kullanıcı olarak VS Code açmak için aşağıdaki komutu çalıştırın.
```
sudo code . --user-data-dir='.'
```

Uygulama artık VS Code çalışma alanınızda görünmelidir.

![Sayaç hizmet uygulaması çalışma](./media/service-fabric-develop-csharp-applications-with-vs-code/counter-service-application-in-workspace.png)

## <a name="build-the-application"></a>Uygulama oluşturma
1. (Ctrl + Shift + p) açmak için ENTER tuşuna **komut paleti** VS code'da.
2. Arayın ve seçin **Service Fabric: Uygulama derleme** komutu. Derleme çıktısında tümleşik terminale gönderilir.

   ![VS code'da uygulama komutu oluşturun](./media/service-fabric-develop-csharp-applications-with-vs-code/sf-build-application.png)

## <a name="deploy-the-application-to-the-local-cluster"></a>Uygulamayı yerel kümeye dağıtma
Uygulamayı oluşturduktan sonra yerel kümeye dağıtabilirsiniz. 

1. Gelen **komut paleti**seçin **Service Fabric: Uygulama (Localhost) komutu dağıtma**. Yükleme işlemini çıktısı için tümleşik Terminalini gönderilir.

   ![VS code'da uygulama komutu dağıtma](./media/service-fabric-develop-csharp-applications-with-vs-code/sf-deploy-application.png)

4. Dağıtım tamamlandığında, tarayıcıyı başlatın ve Service Fabric Explorer'ı açın: http:\//localhost:19080 / Gezgini. Uygulamanın çalıştığını görmelisiniz. Bu biraz zaman alabilir. Bu nedenle sabırlı olun. 

   ![Service Fabric Explorer hizmet uygulamasında sayaç](./media/service-fabric-develop-csharp-applications-with-vs-code/sfx-verify-deploy.png)

4. Uygulamanın çalıştığı doğruladıktan sonra tarayıcıyı başlatın ve şu sayfayı açın: http:\//localhost:31002. Bu, uygulamanın ön uç web API'sidir. Geçerli sayaç değerini artırır olarak görmek için sayfayı yenileyin.

   ![Tarayıcıda sayacı hizmet uygulaması](./media/service-fabric-develop-csharp-applications-with-vs-code/counter-service-running.png)

## <a name="debug-the-application"></a>Uygulamada hata ayıklama
VS code'da uygulamalarında hata ayıklaması yaparken yerel bir kümede uygulamayı çalıştırması gerekir. Kesme noktaları ardından kodu eklenebilir.

Bir kesme noktası ve hata ayıklama ayarlamak için aşağıdaki adımları tamamlayın:
1. Gezgini açmak */src/CounterServiceApplication/CounterService/CounterService.cs* içinde 62 satırında bir kesme noktası ayarlayın ve dosya `RunAsync` yöntemi.
3. Hata ayıklama simgesini **etkinlik çubuğu** hata ayıklayıcı görünümü VS Code'da açın. Hata ayıklayıcı görünümü üst kısmındaki dişli simgesine tıklayıp **.NET Core** ortam açılan menüden. Launch.json dosyası açılır. Bu dosya kapatabilirsiniz. Artık hata ayıklama yapılandırma menüde Çalıştır düğmesinin yanındaki (yeşil ok) bulunan yapılandırma seçeneklerinin görmeniz gerekir.

   ![VS Code çalışma simgesi hata ayıklama](./media/service-fabric-develop-csharp-applications-with-vs-code/debug-icon-workspace.png)

2. Seçin **.NET Core ekleme** hata ayıklama yapılandırması menüsünden.

   ![VS Code çalışma simgesi hata ayıklama](./media/service-fabric-develop-csharp-applications-with-vs-code/debug-start.png)

3. Service Fabric Explorer bir tarayıcıda açın: http:\//localhost:19080 / Gezgini. Tıklayın **uygulamaları** ve detaya gitmesine CounterService çalıştıran birincil düğüm belirleyin. Birincil düğüm CounterService için aşağıdaki görüntüde 0 düğümüdür.

   ![Birincil düğüm CounterService için](./media/service-fabric-develop-csharp-applications-with-vs-code/counter-service-primary-node.png)

4. VS Code'da çalıştırma (yeşil ok) yanındaki simgeye **.NET Core ekleme** hata ayıklama yapılandırması. İşlem seçimi iletişim kutusunda, adım 4'te tanımlanan birincil düğüm üzerinde çalışan CounterService işlemi seçin.

   ![Birincil işlemi](./media/service-fabric-develop-csharp-applications-with-vs-code/select-process.png)

5. Kesme noktasına *CounterService.cs* dosya isabet çok hızlı bir şekilde. Ardından, yerel değişkenlerin değerleri de keşfedebilirsiniz. Hata ayıklama araç çubuğu VS Code üst kısmında yürütme, satırları, yöntemleri, içine Adımlama üzerinden adımla devam etmek için geçerli yöntemi dışında adımı kullanın. 

   ![Değerleri hata ayıklama](./media/service-fabric-develop-csharp-applications-with-vs-code/breakpoint-hit.png)

6. Hata ayıklama oturumunu sona erdirmek için VS Code üst kısmındaki hata ayıklama araç çubuğundaki Tak simgeye tıklayın...
   
   ![Hata Ayıklayıcı'dan bağlantısını kes](./media/service-fabric-develop-csharp-applications-with-vs-code/debug-bar-disconnect.png)
       
7. Hata ayıklama işlemini tamamladığınızda, kullanabileceğiniz **Service Fabric: Uygulamayı kaldırma** CounterService uygulamayı yerel kümenize kaldırmak için komutu. 

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [geliştirme ve hata ayıklama, VS Code ile Java Service Fabric uygulamaları](./service-fabric-develop-java-applications-with-vs-code.md).



