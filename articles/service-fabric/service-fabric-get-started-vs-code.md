---
title: Azure Service Fabric Başlarken VS koduyla | Microsoft Docs
description: Bu makalede, Visual Studio Code kullanarak Service Fabric uygulamaları oluşturma bir genel bakıştır.
services: service-fabric
documentationcenter: .net
author: JimacoMS2
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2018
ms.author: v-jamebr
ms.openlocfilehash: 367829c269bd1d96e6aa5fab1be008483a4ab5ab
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37116204"
---
# <a name="service-fabric-for-visual-studio-code"></a>Visual Studio Code için Service Fabric

[VS Code için Service Fabric Reliable Services Uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-service-fabric-reliable-services) oluşturmak, yapı ve Windows, Linux ve macOS işletim sistemlerine Service Fabric uygulamaları hata ayıklamak için gerekli araçları sağlar.

Bu makalede gereksinimlere genel bakış ve Kurulum uzantısı'nın yanı sıra uzantısı tarafından sağlanan çeşitli komutlar kullanımını sağlar. 

> [!IMPORTANT]
> Service Fabric Java uygulamalarını Windows makinelerde geliştirilmiş olmalıdır, ancak yalnızca Azure Linux kümeleri üzerinde dağıtılabilir. Windows, Java uygulamalarında hata ayıklama desteklenmiyor.

## <a name="prerequisites"></a>Önkoşullar

Tüm ortamlar üzerinde aşağıdaki önkoşullar yüklenmelidir.

* [Visual Studio Code](https://code.visualstudio.com/)
* [Node.js](https://nodejs.org/)
* [Git](https://git-scm.com/)
* [Service Fabric SDK](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started)
* Yeoman oluşturucuları--uygulamanız için uygun oluşturucuları yükleme

   ```sh
   npm install -g yo
   npm install -g generator-azuresfjava
   npm install -g generator-azuresfcsharp
   npm install -g generator-azuresfcontainer
   npm install -g generator-azuresfguest
   ```

Java geliştirme için aşağıdaki önkoşullar yüklenmelidir:

* [Java SDK'sı](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) (sürüm 1,8)
* [Gradle](https://gradle.org/install/)
* [Java VS Code uzantısı için hata ayıklayıcı](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debug) Java hizmetlerde hata ayıklamak için gerekli. Java hizmetlerinde hata ayıklama Linux'ta yalnızca desteklenir. Uzantıları simgesine tıklayarak ya da yükleyebilirsiniz **etkinlik çubuğu** VS Code ve uzantısı veya arama VS Code Marketi'nden.

.NET Core için aşağıdaki önkoşullar yüklenmelidir / C# geliştirme:

* [.NET core](https://www.microsoft.com/net/learn/get-started) (sürüm 2.0.0 veya üzeri)
* [C# (OmniSharp tarafından desteklenen) Visual Studio Code VS Code uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) C# hizmetlerde hata ayıklamak için gerekli. Uzantıları simgesine tıklayarak ya da yükleyebilirsiniz **etkinlik çubuğu** VS Code ve uzantısı veya arama VS Code Marketi'nden.

## <a name="setup"></a>Kurulum

1. Açık VS kodu.
2. Uzantıları simgesini **etkinlik çubuğu** VS Code sol tarafındaki. "Service Fabric" arayın. Tıklatın **yükleme** Service Fabric Reliable Services uzantısı.

## <a name="commands"></a>Komutlar
Service Fabric Reliable Services uzantısı VS Code için Service Fabric projeleri oluşturup geliştiricilere yardımcı olmak için birçok komutları sağlar. Komutları çağırabilir **komutu palet** basarak `(Ctrl + Shift + p)`giriş çubuğuna komut adını yazarak ve istenen komut istemi listeden seçerek. 

* Service Fabric: Uygulama oluşturma 
* Service Fabric: Uygulama yayımlama 
* Service Fabric: Uygulama dağıtma 
* Service Fabric: Uygulamayı kaldırma  
* Service Fabric: Uygulama oluşturma 
* Service Fabric: Temiz uygulama 

### <a name="service-fabric-create-application"></a>Service Fabric: Uygulama oluşturma

**Service Fabric: uygulama oluşturma** komutu, geçerli çalışma alanınızda yeni bir Service Fabric uygulaması oluşturur. Hangi yeoman oluşturucuları geliştirme makinenizde yüklü bağlı olarak, Service Fabric uygulaması, Java, C#, kapsayıcı ve Konuk projeleri dahil olmak üzere çeşitli türleri oluşturabilirsiniz. 

1.  Seçin **Service Fabric: Hizmet Ekle** komutu
2.  Yeni Service Fabric uygulamanızı türünü seçin. 
3.  Oluşturmak istediğiniz uygulamanın adını girin
3.  Service Fabric uygulamanızı eklemek istediğiniz hizmet türünü seçin. 
4.  Ad hizmeti için istemleri izleyin. 
5.  Yeni Service Fabric uygulaması çalışma alanında görüntülenir.
6.  Çalışma alanı kök klasöründe hale yeni uygulama klasörünü açın. Buradan komutları yürütülürken devam edebilirsiniz.

### <a name="service-fabric-add-service"></a>Service Fabric: Hizmet Ekle
**Service Fabric: Hizmet Ekle** komutu, varolan bir Service Fabric uygulamaya yeni bir hizmet ekler. Hizmet eklenecek uygulamanın çalışma alanının kök dizinine olması gerekir. 

1.  Seçin **Service Fabric: Hizmet Ekle** komutu.
2.  Geçerli Service Fabric uygulamanızı türünü seçin. 
3.  Service Fabric uygulamanızı eklemek istediğiniz hizmet türünü seçin. 
4.  Ad hizmeti için istemleri izleyin. 
5.  Yeni hizmet proje dizininde görünür. 

### <a name="service-fabric-publish-application"></a>Service Fabric: Uygulama yayımlama
**Service Fabric: uygulama yayımlama** komutu Service Fabric uygulamanızı uzak bir küme üzerinde dağıtır. Hedef küme güvenli veya güvenli olmayan bir kümesi olabilir. Parametreleri Cloud.json ayarlanmamışsa, uygulamayı yerel kümeye dağıtılır.

1.  Uygulama yerleşik olarak bulunan ilk kez proje dizininde bir Cloud.json dosyası oluşturulur.
2.  Cloud.json dosyasında bağlanmak istediğiniz küme için değerleri girin.
3.  Seçin **Service Fabric: uygulama yayımlama** komutu.
4.  Service Fabric Explorer'ı, uygulamanın yüklendiğini doğrulamak için hedef kümeyle görüntüleyin. 

### <a name="service-fabric-deploy-application-localhost"></a>Service Fabric: Uygulama (Localhost) dağıtma
**Service Fabric: uygulama dağıtma** komutu Service Fabric uygulamanızı yerel kümenize dağıtır. Yerel kümenizdeki komutunu kullanmadan önce çalışır durumda olduğundan emin olun. 

1.  Seçin **Service Fabric: uygulama dağıtma** komutu
2.  Service Fabric Explorer ile yerel küme görüntülemek (http://localhost:19080/Explorer) uygulamanın yüklü olduğunu doğrulamak için. Bu biraz zaman alabilir bu nedenle endişelenmeyin.
3.  De kullanabilirsiniz **Service Fabric: uygulama yayımlama** yerel bir küme dağıtmak için Cloud.json dosyasında ayarlanan hiçbir parametre komutu.

> [!NOTE]
> Yerel küme için Java uygulamalarını dağıtma Windows makinelerde desteklenmiyor.

### <a name="service-fabric-remove-application"></a>Service Fabric: Uygulamayı kaldırma
**Service Fabric: uygulaması Kaldır** komutu, daha önce VS Code uzantısını kullanarak dağıtıldığı kümeden bir Service Fabric uygulaması kaldırır. 

1.  Seçin **Service Fabric: uygulaması Kaldır** komutu.
2.  Uygulama kaldırıldığını doğrulamak için Service Fabric Explorer ile küme görüntüleyin. Bu biraz zaman alabilir bu nedenle endişelenmeyin.

### <a name="service-fabric-build-application"></a>Service Fabric: Uygulama oluşturma
**Service Fabric: uygulaması Kaldır** komutu, Java veya C# Service Fabric uygulamaları oluşturabilir. 

1.  Bu komutu çalıştırmadan önce uygulama kök klasöründe olduğundan emin olun. Komut uygulama (C# veya Java) türünü tanımlayan ve uygulamanızı buna uygun olarak oluşturur.
2.  Seçin **Service Fabric: derleme uygulama** komutu.
3.  Yapı işleminin çıktısı tümleşik terminal yazılır.

### <a name="service-fabric-clean-application"></a>Service Fabric: Temiz uygulama
**Service Fabric: temiz uygulama** komut, tüm jar dosyalarını ve yapı tarafından oluşturulan yerel kitaplıkları siler. Yalnızca Java uygulamaları için geçerlidir. 

1.  Bu komutu çalıştırmadan önce uygulama kök klasöründe olduğundan emin olun. 
2.  Seçin **Service Fabric: temiz uygulama** komutu.
3.  Temiz işleminin çıktısı tümleşik terminal yazılır.

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [geliştirmek ve VS Code ile C# Service Fabric uygulamalarında hata ayıklama](./service-fabric-develop-csharp-applications-with-vs-code.md).
* Bilgi edinmek için nasıl [geliştirmek ve hata ayıklama Java Service Fabric uygulamaları ile VS Code](./service-fabric-develop-java-applications-with-vs-code.md).
