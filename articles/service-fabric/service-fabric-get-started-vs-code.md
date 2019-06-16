---
title: Azure Service Fabric ile çalışmaya başlama VS Code'u | Microsoft Docs
description: Bu makalede, Visual Studio Code kullanarak Service Fabric uygulamaları oluşturmaya genel bakış ' dir.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2018
ms.author: pepogors
ms.openlocfilehash: f977a48338f784562ec84355aabb212e5a3dade4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60946584"
---
# <a name="service-fabric-for-visual-studio-code"></a>Visual Studio Code için Service Fabric

[VS Code için Service Fabric güvenilir hizmetler uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-service-fabric-reliable-services) oluşturmak, derlemek ve Service Fabric uygulamaları Windows, Linux ve Macos'ta işletim sistemlerinde hata ayıklama için gereken araçları sağlar.

Bu makalede gereksinimlerine genel bir bakış ve Kurulum uzantısı'nın yanı sıra uzantısı tarafından sağlanan çeşitli komutlara kullanımını sağlar. 

> [!IMPORTANT]
> Service Fabric Java uygulamalarını Windows makinelerde geliştirilebilir, ancak yalnızca Azure Linux kümeleri üzerinde dağıtılabilir. Java uygulamalarında hata ayıklama, Windows üzerinde desteklenmiyor.

## <a name="prerequisites"></a>Önkoşullar

Tüm ortamlarda aşağıdaki önkoşulların yüklü olması gerekir.

* [Visual Studio Code](https://code.visualstudio.com/)
* [Node.js](https://nodejs.org/)
* [Git](https://git-scm.com/)
* [Service fabric SDK'sı](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started)
* Yeoman oluşturucularını--uygulamanız için uygun oluşturucuları yükleyin

   ```sh
   npm install -g yo
   npm install -g generator-azuresfjava
   npm install -g generator-azuresfcsharp
   npm install -g generator-azuresfcontainer
   npm install -g generator-azuresfguest
   ```

Java geliştirme için aşağıdaki önkoşulların yüklü olması gerekir:

* [Java SDK'sı](https://aka.ms/azure-jdks) (sürüm 1.8)
* [Gradle](https://gradle.org/install/)
* [Java VS Code uzantısı için hata ayıklayıcı](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debug) Java hizmetlerinden hatalarını ayıklamak için gerekli. Java hizmetlerinde hata ayıklama Linux üzerinde yalnızca desteklenir. Uzantıları simgesine tıklayarak ya da yükleyebileceğiniz **etkinlik çubuğu** VS Code ve uzantıyı veya arama VS Code Marketi'nde gelen.

.NET Core için aşağıdaki önkoşullar yüklenmelidir /C# geliştirme:

* [.NET core](https://www.microsoft.com/net/learn/get-started) (2.0.0 sürümü veya üzeri)
* [C#Visual Studio Code (OmniSharp tarafından desteklenen) VS Code uzantısı için](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) hatalarını ayıklamak için gerekli C# Hizmetleri. Uzantıları simgesine tıklayarak ya da yükleyebileceğiniz **etkinlik çubuğu** VS Code ve uzantıyı veya arama VS Code Marketi'nde gelen.

## <a name="setup"></a>Kurulum

1. Açık VS kodu.
2. Uzantıları simgesini **etkinlik çubuğu** VS Code sol tarafındaki. "Service Fabric" arayın. Tıklayın **yükleme** Service Fabric güvenilir hizmetler uzantısı.

## <a name="commands"></a>Komutlar
VS Code için Service Fabric güvenilir hizmetler uzantısı oluşturun ve Service Fabric projelerini dağıtma geliştiricilerin yardımcı olmak için birçok komutlar sağlar. Komutları çağırabilirsiniz **komut paleti** tuşuna basarak `(Ctrl + Shift + p)`, komut adı giriş çubuğuna yazarak ve istenen komut istemi listeden seçerek. 

* Service Fabric: Uygulama oluşturma 
* Service Fabric: Uygulama yayımlama 
* Service Fabric: Uygulamayı dağıtma 
* Service Fabric: Uygulamayı kaldırma  
* Service Fabric: Uygulama oluşturma 
* Service Fabric: Uygulamayı Temizle 

### <a name="service-fabric-create-application"></a>Service Fabric: Uygulama oluşturma

**Service Fabric: Uygulama oluşturma** komut, geçerli çalışma alanınızda yeni bir Service Fabric uygulaması oluşturur. Hangi yeoman oluşturucularını geliştirme makinenizde yüklü bağlı olarak, Service Fabric uygulaması, Java, dahil olmak üzere çeşitli türleri oluşturabilirsiniz C#, kapsayıcı ve Konuk projeleri. 

1.  Seçin **Service Fabric: Hizmet Ekle** komutu
2.  Yeni Service Fabric uygulamanızı türünü seçin. 
3.  Oluşturmak istediğiniz uygulamanın adını girin
3.  Service Fabric uygulamanıza eklemek istediğiniz hizmet türünü seçin. 
4.  Hizmet adı için istemleri izleyin. 
5.  Yeni Service Fabric uygulama çalışma alanında görüntülenir.
6.  Çalışma alanı kök klasöründe haline gelebilmesi yeni uygulama klasörü açın. Komutları yürütmeden buradan devam edebilirsiniz.

### <a name="service-fabric-add-service"></a>Service Fabric: Hizmet Ekle
**Service Fabric: Hizmet Ekle** komutu, var olan bir Service Fabric uygulaması için yeni bir hizmet ekler. Hizmet eklenecek uygulama çalışma alanının kök dizini olmalıdır. 

1.  Seçin **Service Fabric: Hizmet Ekle** komutu.
2.  Geçerli Service Fabric uygulamanızı türünü seçin. 
3.  Service Fabric uygulamanıza eklemek istediğiniz hizmet türünü seçin. 
4.  Hizmet adı için istemleri izleyin. 
5.  Proje dizininizde yeni görünür. 

### <a name="service-fabric-publish-application"></a>Service Fabric: Uygulama yayımlama
**Service Fabric: Uygulama yayımlama** komutu, Service Fabric uygulamanızı uzak bir küme dağıtır. Hedef küme güvenli veya güvenli olmayan bir kümeye olabilir. Parametreleri Cloud.json ayarlanmamışsa, uygulamayı yerel kümeye dağıtılır.

1.  Uygulama yerleşik olarak bulunan ilk kez Cloud.json dosya proje dizininde oluşturulur.
2.  Publishprofiles dosyasında bağlamak istediğiniz küme için değerleri girin.
3.  Seçin **Service Fabric: Uygulama yayımlama** komutu.
4.  Service Fabric Explorer'ı, uygulamanın yüklendiğini doğrulamak için hedef kümeyle görüntüleyin. 

### <a name="service-fabric-deploy-application-localhost"></a>Service Fabric: Uygulama (Localhost) dağıtma
**Service Fabric: Uygulamayı dağıtma** komutu, Service Fabric uygulamanızı yerel kümenize dağıtır. Yerel kümenize komutu kullanmadan önce çalışır durumda olduğundan emin olun. 

1. Seçin **Service Fabric: Uygulamayı dağıtma** komutu
2. Service Fabric Explorer ile yerel küme görüntüleyebilirsiniz (http:\//localhost:19080 / Explorer) uygulamanın yüklendiğini doğrulamak için. Bu biraz zaman alabilir. Bu nedenle sabırlı olun.
3. Ayrıca **Service Fabric: Uygulama yayımlama** komutunu parametresiz Cloud.json dosyanın yerel bir kümeye dağıtmak için ayarlayın.

> [!NOTE]
> Java uygulamalarını yerel kümeye dağıtma Windows makinelerde desteklenmiyor.

### <a name="service-fabric-remove-application"></a>Service Fabric: Uygulamayı kaldırma
**Service Fabric: Uygulamayı kaldırma** komut bir Service Fabric uygulaması, daha önce VS Code uzantısı kullanarak dağıtıldığı kümeden kaldırır. 

1.  Seçin **Service Fabric: Uygulamayı kaldırma** komutu.
2.  Uygulama kaldırıldığını doğrulamak için Service Fabric Explorer ile küme görüntüleyebilirsiniz. Bu biraz zaman alabilir. Bu nedenle sabırlı olun.

### <a name="service-fabric-build-application"></a>Service Fabric: Uygulama oluşturma
**Service Fabric: Uygulama derleme** komut ya da Java oluşturabilirsiniz veya C# Service Fabric uygulamaları. 

1.  Bu komutu çalıştırmadan önce uygulama kök klasöründe olduğundan emin olun. Komut uygulama türlerini tanımlar (C# veya Java) ve uygun şekilde uygulamanızı oluşturur.
2.  Seçin **Service Fabric: Uygulama derleme** komutu.
3.  Yapı işleminin çıkış için tümleşik Terminalini yazılır.

### <a name="service-fabric-clean-application"></a>Service Fabric: Uygulamayı Temizle
**Service Fabric: Uygulamayı Temizle** komut tüm jar dosyalarını ve derleme tarafından oluşturulan yerel kitaplıkları siler. Yalnızca Java uygulamaları için geçerlidir. 

1.  Bu komutu çalıştırmadan önce uygulama kök klasöründe olduğundan emin olun. 
2.  Seçin **Service Fabric: Uygulamayı Temizle** komutu.
3.  Temizleme işleminin çıktısı için tümleşik Terminalini yazılır.

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [geliştirme ve hata ayıklama C# VS Code ile Service Fabric uygulamaları](./service-fabric-develop-csharp-applications-with-vs-code.md).
* Bilgi edinmek için nasıl [geliştirme ve hata ayıklama, VS Code ile Java Service Fabric uygulamaları](./service-fabric-develop-java-applications-with-vs-code.md).
