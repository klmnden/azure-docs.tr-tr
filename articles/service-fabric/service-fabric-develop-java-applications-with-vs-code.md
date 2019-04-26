---
title: Visual Studio Code ile Azure Service Fabric Java uygulamaları geliştirin | Microsoft Docs
description: Bu makalede, oluşturmanızı, dağıtmanızı ve Visual Studio Code kullanarak Java Service Fabric uygulamalarında hata ayıklamak gösterilmektedir.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2018
ms.author: pepogors
ms.openlocfilehash: 7f60371fb533526ef5bdb154d0c08dface9c0d1f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60394002"
---
# <a name="develop-java-service-fabric-applications-with-visual-studio-code"></a>Visual Studio Code ile Java Service Fabric uygulamaları geliştirin

[VS Code için Service Fabric güvenilir hizmetler uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-service-fabric-reliable-services) Java Service Fabric uygulamaları Windows, Linux ve Macos'ta işletim sistemlerinde oluşturmayı kolaylaştırır.

Bu makalede, oluşturmanızı, dağıtmanızı ve Visual Studio Code'u kullanarak bir Java Service Fabric uygulamasında hata ayıklama işlemini göstermektedir.

> [!IMPORTANT]
> Service Fabric Java uygulamalarını Windows makinelerde geliştirilebilir, ancak yalnızca Azure Linux kümeleri üzerinde dağıtılabilir. Java uygulamalarında hata ayıklama, Windows üzerinde desteklenmiyor.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede, zaten VS Code, VS Code için Service Fabric güvenilir hizmetler uzantısı ve geliştirme ortamınız için gereken herhangi bir bağımlılığın yüklediğinizi varsayar. Daha fazla bilgi için bkz. [Başlarken](./service-fabric-get-started-vs-code.md#prerequisites).

## <a name="download-the-sample"></a>Örneği indirme
Bu makalede oylama uygulamasında kullanan [Service Fabric Java uygulaması hızlı başlangıç örnek GitHub deposunda](https://github.com/Azure-Samples/service-fabric-java-quickstart). 

Depoyu geliştirme makinenize kopyalamak için (Windows komut penceresinde) bir terminal penceresinden aşağıdaki komutu çalıştırın:

```sh
git clone https://github.com/Azure-Samples/service-fabric-java-quickstart.git
```

## <a name="open-the-application-in-vs-code"></a>Uygulama VS Code'da açın

Açık VS kodu.  Gezgini simgesine tıklayın **etkinlik çubuğu** tıklatıp **Klasör Aç**, veya **Dosya -> Klasör Aç**. Gidin *./service-fabric-java-quickstart/Voting* kopyaladığınız deponun ardından klasörden dizinine tıklayın **Tamam**. Çalışma alanı, aşağıdaki ekran görüntüsünde gösterilen aynı dosyaları içermelidir.

![Java oylama uygulaması çalışma](./media/service-fabric-develop-java-applications-with-vs-code/java-voting-application.png)

## <a name="build-the-application"></a>Uygulama oluşturma

1. (Ctrl + Shift + p) açmak için ENTER tuşuna **komut paleti** VS code'da.
2. Arayın ve seçin **Service Fabric: Uygulama derleme** komutu. Derleme çıktısında tümleşik terminale gönderilir.

   ![VS code'da uygulama komutu oluşturun](./media/service-fabric-develop-java-applications-with-vs-code/sf-build-application.png)

## <a name="deploy-the-application-to-the-local-cluster"></a>Uygulamayı yerel kümeye dağıtma
Uygulamayı oluşturduktan sonra yerel kümeye dağıtabilirsiniz. 

> [!IMPORTANT]
> Java uygulamalarını yerel kümeye dağıtma Windows makinelerde desteklenmiyor.

1. Gelen **komut paleti**seçin **Service Fabric: Uygulama (Localhost) komutu dağıtma**. Yükleme işlemini çıktısı için tümleşik Terminalini gönderilir.

   ![VS code'da uygulama komutu dağıtma](./media/service-fabric-develop-java-applications-with-vs-code/sf-deploy-application.png)

4. Dağıtım tamamlandığında, tarayıcıyı başlatın ve Service Fabric Explorer'ı açın: `http://localhost:19080/Explorer`. Uygulamanın çalıştığını görmelisiniz. Bu biraz zaman alabilir. Bu nedenle sabırlı olun. 

   ![Service Fabric Explorer'ın oylama uygulaması](./media/service-fabric-develop-java-applications-with-vs-code/sfx-localhost-java.png)

4. Uygulamanın çalıştığını doğruladıktan sonra tarayıcıyı başlatın ve şu sayfayı açın: `http://localhost:8080`. Bu, uygulamanın ön uç web API'sidir. Öğeleri ekleyebilir ve oylamak için tıklayın.

   ![Tarayıcıda oylama uygulaması](./media/service-fabric-develop-java-applications-with-vs-code/voting-sample-in-browser.png)

5. Uygulamayı kümeden kaldırmak için işaretleyin **Service Fabric: Uygulamayı kaldırma** komutunu **komut paleti**. Kaldırma işlemi çıktısı için tümleşik Terminalini gönderilir. Uygulama yerel kümeden kaldırıldığını doğrulamak için Service Fabric Explorer'ı kullanabilirsiniz.

## <a name="debug-the-application"></a>Uygulamada hata ayıklama
VS code'da uygulamalarında hata ayıklaması yaparken yerel bir kümede uygulamayı çalıştırması gerekir. Kesme noktaları ardından kodu eklenebilir.

> [!IMPORTANT]
> Java uygulamalarında hata ayıklama Windows makinelerde desteklenmiyor.

VotingDataService ve hata ayıklama için oylama uygulaması hazırlamak için aşağıdaki adımları tamamlayın:

1. Güncelleştirme *Voting/VotingApplication/VotingDataServicePkg/Code/entryPoint.sh* dosya.
Açıklama komutunu 6 satırında ('#' kullanın) ve aşağıdaki komut dosyasının alt kısmına ekleyin:

   ```
   java -Xdebug -Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=n -Djava.library.path=$LD_LIBRARY_PATH -jar VotingDataService.jar
   ```

2. Güncelleştirme *Voting/VotingApplication/ApplicationManifest.xml* dosya. Ayarlama **MinReplicaSetSize** ve **TargetReplicaSetSize** öznitelikleri "1" **StatefulService** öğesi:
   
   ```xml
         <StatefulService MinReplicaSetSize="1" ServiceTypeName="VotingDataServiceType" TargetReplicaSetSize="1">
   ```

3. Hata ayıklama simgesini **etkinlik çubuğu** hata ayıklayıcı görünümü VS Code'da açın. Hata ayıklayıcı görünümü üst kısmındaki dişli simgesine tıklayıp **Java** ortam açılan menüden. Launch.json dosyası açılır. 

   ![VS Code çalışma simgesi hata ayıklama](./media/service-fabric-develop-java-applications-with-vs-code/debug-icon-workspace.png)

3. Launch.json dosyasında yapılandırmasını adlı bağlantı noktası değeri ayarlamak **Hata Ayıkla (Ekle)** için **8001**. Dosyayı kaydedin.

   ![Launch.json için hata ayıklama yapılandırması](./media/service-fabric-develop-java-applications-with-vs-code/launch-json-java.png)

4. Kullanarak uygulamayı yerel kümeye dağıtma **Service Fabric: Uygulama (Localhost) dağıtma** komutu. Service Fabric Explorer'da uygulamanın çalıştığını doğrulayın. Artık uygulamanızı hata ayıklama hazırdır.

Bir kesme noktası ayarlamak için aşağıdaki adımları tamamlayın:

1. Gezgini açmak */Voting/VotingDataService/src/statefulservice/VotingDataService.java* dosya. Kod ilk satırında bir kesme noktası ayarlamak `try` engelleyin `addItem` yöntemi (80. satır).
   
   ![Veri hizmetinde oylama kesme noktası Ayarla](./media/service-fabric-develop-java-applications-with-vs-code/breakpoint-set.png)

   > [!IMPORTANT]
   > Yürütülebilir kod satırlarını üzerinde kesme noktaları ayarlayın sağlayın. Örneğin yöntem bildirimlerinde ayarlanan kesme noktaları `try` deyimleri veya `catch` deyimleri hata ayıklayıcı tarafından eksik.
2. Hata ayıklamayı başlatmak için hata ayıklama simgeyi tıklatın **etkinlik çubuğu**seçin **hata ayıklama (Ekle)** hata ayıklama menüsünden yapılandırma ve çalıştırma (yeşil ok) düğmesini tıklatın.

   ![Hata Ayıkla (Ekle) yapılandırma](./media/service-fabric-develop-java-applications-with-vs-code/debug-attach-java.png)

3. Bir web tarayıcısında Git `http://localhost:8080`. Yeni bir öğe metin kutusuna yazıp tıklayın **+ Ekle**. Kesme noktasına isabet. Hata ayıklama araç çubuğu, VS Code üst kısmında yürütme, satırları, yöntemleri, içine Adımlama üzerinden adımla devam etmek için geçerli yöntemi dışında adım kullanabilirsiniz. 
   
   ![Kesme noktası isabet](./media/service-fabric-develop-java-applications-with-vs-code/breakpoint-hit.png)
       
4. Hata ayıklama oturumunu sona erdirmek için VS Code üst kısmındaki hata ayıklama araç çubuğundaki Tak simgeye tıklayın.
   
   ![Hata Ayıklayıcı'dan bağlantısını kes](./media/service-fabric-develop-java-applications-with-vs-code/debug-bar-disconnect.png)
       
5. Hata ayıklama işlemini tamamladığınızda, kullanabileceğiniz **Service Fabric: Uygulamayı kaldırma** yerel kümenize Voting uygulamasını kaldırmak için komutu. 

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [geliştirme ve hata ayıklama C# VS Code ile Service Fabric uygulamaları](./service-fabric-develop-csharp-applications-with-vs-code.md).
