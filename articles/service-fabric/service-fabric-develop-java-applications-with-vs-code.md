---
title: Visual Studio Code ile Java Azure Service Fabric uygulamaları geliştirme | Microsoft Docs
description: Bu makalede, oluşturmak, dağıtmak ve Visual Studio Code kullanarak Java Service Fabric uygulamaları hata ayıklama gösterilmektedir.
services: service-fabric
documentationcenter: .net
author: JimacoMS
manager: timlt
editor: ''
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2018
ms.author: v-jamebr
ms.openlocfilehash: 54c94c50f6292694e947d97a10fd6976c14e19df
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37116180"
---
# <a name="develop-java-service-fabric-applications-with-visual-studio-code"></a>Visual Studio Code ile Java Service Fabric uygulamaları geliştirme

[VS Code için Service Fabric Reliable Services Uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-service-fabric-reliable-services) Windows, Linux ve macOS işletim sistemlerinde Java Service Fabric uygulamalar oluşturmanızı kolaylaştırır.

Bu makalede oluşturun, dağıtın ve Visual Studio Code kullanarak bir Java Service Fabric uygulaması hata ayıklama gösterilmektedir.

> [!IMPORTANT]
> Service Fabric Java uygulamalarını Windows makinelerde geliştirilmiş olmalıdır, ancak yalnızca Azure Linux kümeleri üzerinde dağıtılabilir. Windows, Java uygulamalarında hata ayıklama desteklenmiyor.

## <a name="prerequisites"></a>Önkoşullar

Bu makale, zaten VS Code, VS Code Service Fabric Reliable Services uzantısı ve geliştirme ortamınız için gereken bağımlılıkları yüklediğinizi varsayar. Daha fazla bilgi için bkz: [Başlarken](./service-fabric-get-started-vs-code.md#prerequisites).

## <a name="download-the-sample"></a>Örneği indirme
Bu makalede oylama uygulamada kullanan [Service Fabric Java uygulaması hızlı başlangıç örnek GitHub deposunu](https://github.com/Azure-Samples/service-fabric-java-quickstart). 

Geliştirme makinenizde depoya kopyalamak için (Windows komut penceresinde) bir terminal penceresinden aşağıdaki komutu çalıştırın:

```sh
git clone https://github.com/Azure-Samples/service-fabric-java-quickstart.git
```

## <a name="open-the-application-in-vs-code"></a>Uygulama VS Code'da açın

Açık VS kodu.  Explorer simgeyi tıklatın **etkinlik çubuğu** tıklatıp **Klasör Aç**, veya **Dosya -> Klasör Aç**. Gidin *./service-fabric-java-quickstart/Voting* kopyaladığınız deponun ardından klasörden dizininde tıklatın **Tamam**. Çalışma alanı aşağıdaki ekran görüntüsünde gösterilen aynı dosyaları içermelidir.

![Java oylama uygulaması çalışma](./media/service-fabric-develop-java-applications-with-vs-code/java-voting-application.png)

## <a name="build-the-application"></a>Uygulama oluşturma

1. Tuşuna (Ctrl + Shift + p) açmak için **komutu palet** VS code'da.
2. Aramak ve seçmek **Service Fabric: derleme uygulama** komutu. Derleme çıktı tümleşik terminal gönderilir.

   ![VS code'da uygulama komutu derleme](./media/service-fabric-develop-java-applications-with-vs-code/sf-build-application.png)

## <a name="deploy-the-application-to-the-local-cluster"></a>Uygulamayı yerel kümeye dağıtma
Uygulamayı oluşturduktan sonra yerel kümeye dağıtabilirsiniz. 

> [!IMPORTANT]
> Yerel küme için Java uygulamalarını dağıtma Windows makinelerde desteklenmiyor.

1. Gelen **komutu palet**seçin **Service Fabric: uygulama dağıtma (Localhost) komutu**. Yükleme işleminin çıktısı tümleşik terminal gönderilir.

   ![VS code'da uygulama komutu dağıtma](./media/service-fabric-develop-java-applications-with-vs-code/sf-deploy-application.png)

4. Dağıtım tamamlandığında, bir tarayıcı başlatmak ve Service Fabric Explorer açın: http://localhost:19080/Explorer. Uygulamanın çalıştığını görmelisiniz. Bu biraz zaman alabilir bu nedenle endişelenmeyin. 

   ![Service Fabric Explorer'da oylama uygulaması](./media/service-fabric-develop-java-applications-with-vs-code/sfx-localhost-java.png)

4. Uygulamanın çalıştığını doğruladıktan sonra bir tarayıcı başlatmak ve bu sayfayı açın: http://localhost:8080. Web uygulamasının ön budur. Öğeler ekleme ve bunlar üzerinde oylamak için tıklatın.

   ![Tarayıcıda oylama uygulaması](./media/service-fabric-develop-java-applications-with-vs-code/voting-sample-in-browser.png)

5. Uygulamayı kümeden kaldırmak için seçin **Service Fabric: uygulaması Kaldır** komutunu **komutu palet**. Kaldırma işlemi çıktısını tümleşik terminal gönderilir. Uygulamayı yerel kümeden kaldırıldığını doğrulamak için Service Fabric Explorer'ı kullanabilirsiniz.

## <a name="debug-the-application"></a>Uygulamada hata ayıklama
VS kodunu uygulamalarında hata ayıklama sırasında uygulama yerel bir kümede çalıştırması gerekir. Kesme noktaları ardından koda eklenebilir.

> [!IMPORTANT]
> Java uygulamalarında hata ayıklama Windows makinelerde desteklenmiyor.

VotingDataService ve hata ayıklama için oylama uygulaması hazırlamak için aşağıdaki adımları tamamlayın:

1. Güncelleştirme *Voting/VotingApplication/VotingDataServicePkg/Code/entryPoint.sh* dosya.
Komutunu satır 6 ('#' ı kullanın) açıklama ve aşağıdaki komut dosyasının en altına ekleyin:

   ```
   java -Xdebug -Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=n -Djava.library.path=$LD_LIBRARY_PATH -jar VotingDataService.jar
   ```

2. Güncelleştirme *Voting/VotingApplication/ApplicationManifest.xml* dosya. Ayarlama **MinReplicaSetSize** ve **TargetReplicaSetSize** öznitelikleri "1" **StatefulService** öğe:
   
   ```xml
         <StatefulService MinReplicaSetSize="1" ServiceTypeName="VotingDataServiceType" TargetReplicaSetSize="1">
   ```

3. Hata ayıklama simgeyi tıklatın **etkinlik çubuğu** hata ayıklayıcı görünümünü VS Code'da açın. Hata ayıklayıcı görünüm üstündeki dişli simgesine tıklayın ve seçin **Java** açılır ortamı menüsünde. Launch.json'u dosyasını açar. 

   ![VS Code çalışma alanında simgesi hata ayıklama](./media/service-fabric-develop-java-applications-with-vs-code/debug-icon-workspace.png)

3. Bağlantı noktası değeri adlı yapılandırmasında Launch.json'u dosyasında ayarlanan **(Ekle) hata ayıklama** için **8001**. Dosyayı kaydedin.

   ![Launch.json'u için hata ayıklama yapılandırması](./media/service-fabric-develop-java-applications-with-vs-code/launch-json-java.png)

4. Kullanarak uygulamayı yerel kümeye dağıtın **Service Fabric: uygulama dağıtma (Localhost)** komutu. Service Fabric Explorer'da uygulama çalıştığından emin olun. Uygulamanız artık ayıklanacak hazırdır.

Bir kesme noktası ayarlamak için aşağıdaki adımları tamamlayın:

1. Explorer'da açın */Voting/VotingDataService/src/statefulservice/VotingDataService.java* dosya. Kod ilk satırında bir kesme noktası belirleyerek `try` engelleyin `addItem` yöntemi (satır 80).
   
   ![Oylama veri hizmetinde kesme noktası ayarlayın](./media/service-fabric-develop-java-applications-with-vs-code/breakpoint-set.png)

   > [!IMPORTANT]
   > Kesme noktaları yürütülebilir kod satırlarında ayarladığınızdan emin olun. Örneğin yöntemi bildirimlerinde ayarlamak kesme noktaları `try` deyimleri veya `catch` deyimleri hata ayıklayıcı tarafından eksik.
2. Hata ayıklama başlamak için hata ayıklama simgeyi tıklatın **etkinlik çubuğu**seçin **hata ayıklama (Ekle)** debug menüsünden, yapılandırma ve çalıştırma düğmesine (yeşil ok) tıklayın.

   ![Hata ayıklama (Ekle) yapılandırma](./media/service-fabric-develop-java-applications-with-vs-code/debug-attach-java.png)

3. Bir web tarayıcısında Git http://localhost:8080. Yeni bir öğe metin kutusuna yazın ve'ı tıklatın **+ Ekle**. Kesme noktası isabet. Yürütme, satırları, yöntemleri, adımla Adımlama devam etmek için geçerli yöntemi dışında adım VS Code üstünde Debug araç kullanabilirsiniz. 
   
   ![Kesme noktası isabet](./media/service-fabric-develop-java-applications-with-vs-code/breakpoint-hit.png)
       
4. Hata ayıklama oturumu sonlandırmak için hata ayıklama araç VS Code üstündeki Tak simgesine tıklayın.
   
   ![Hata Ayıklayıcı'dan bağlantısını kesme](./media/service-fabric-develop-java-applications-with-vs-code/debug-bar-disconnect.png)
       
5. Hata ayıklama tamamladığınızda kullanabileceğiniz **Service Fabric: uygulaması Kaldır** yerel kümenizden oylama uygulamayı kaldırmak için komutu. 

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [geliştirmek ve VS Code ile C# Service Fabric uygulamalarında hata ayıklama](./service-fabric-develop-csharp-applications-with-vs-code.md).
