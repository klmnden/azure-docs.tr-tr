---
title: "C# ile Azure Service Fabric güvenilir hizmeti oluşturma"
description: "Visual Studio ile, Azure Service Fabric üzerinde derlenmiş bir Güvenilir Hizmet uygulaması oluşturun, dağıtım ve hatalarını ayıklayın."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: vturecek
ms.assetid: c3655b7b-de78-4eac-99eb-012f8e042109
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/04/2017
ms.author: ryanwi
ms.translationtype: HT
ms.sourcegitcommit: 9afd12380926d4e16b7384ff07d229735ca94aaa
ms.openlocfilehash: f93298e6483fd8c9dfda835964aeebd1a430af69
ms.contentlocale: tr-tr
ms.lasthandoff: 07/15/2017

---

# <a name="create-your-first-c-service-fabric-stateful-reliable-services-application"></a>İlk C# Service Fabric durum bilgisi olan reliable services uygulamanızı oluşturma

Yalnızca birkaç dakika içinde Windows üzerinde .NET için ilk Service Fabric uygulamanızı nasıl dağıtacağınızı öğrenin. Tamamladığınızda, güvenilir hizmet uygulamasıyla çalışan bir yerel kümeniz olacaktır.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce [geliştirme ortamınızı ayarladığınızdan](service-fabric-get-started.md) emin olun. Bu, Service Fabric SDK'sını ve Visual Studio 2017 veya 2015'i yüklemeyi de içerir.

## <a name="create-the-application"></a>Uygulama oluşturma

Visual Studio'yu **yönetici** olarak başlatın.

`CTRL`+`SHIFT`+`N` ile bir proje oluşturun

**Yeni Proje** iletişim kutusunda **Bulut > Service Fabric Uygulaması**'nı seçin.

Uygulamaya **MyApplication** adını verin ve **Tamam**'a basın.

   
![Visual Studio'da yeni proje iletişim kutusu][1]

Sonraki iletişim kutusunda her türde Service Fabric uygulaması oluşturabilirsiniz. Bu Hızlı Başlangıç için **Durum Bilgisi Olan Hizmet**'i seçin.

Hizmeti **MyStatefulService** olarak adlandırın ve **Tamam**'a basın.

![Visual Studio'da yeni hizmet iletişim kutusu][2]


Visual Studio uygulama projesini ve durum bilgisi olan hizmet projesini oluşturup bu projeleri Çözüm Gezgini'nde görüntüler.

![Durum bilgisi olan hizmeti içeren uygulama oluşturulduktan sonra Çözüm Gezgini][3]

Uygulama projesi (**MyApplication**) doğrudan kod içermez. Bunun yerine, bir dizi hizmet projesine başvuru sağlar. Ayrıca, bunlardan farklı olarak üç tür içerik daha barındırır:

* **Yayımlama profilleri**  
Farklı ortamlarda dağıtıma yönelik profiller.

* **Betikler**  
Uygulamanızı dağıtmak/yükseltmek için PowerShell betiği.

* **Uygulama tanımı**  
*ApplicationPackageRoot* altında uygulamanızın birleşimini açıklayan ApplicationManifest.xml dosyasını içerir. İlişkili uygulama parametresi dosyaları *ApplicationParameters* altındadır ve ortama özgü parametreleri belirtmek için kullanılabilir. Visual Studio, belirli bir ortama dağıtım yapılırken ilişkili yayımlama profilinde belirtilen uygulama parametresi dosyasını seçer.
    
Hizmet projesinin içeriklerine genel bakış için bkz. [Reliable Services ile çalışmaya başlama](service-fabric-reliable-services-quick-start.md).

## <a name="deploy-and-debug-the-application"></a>Uygulamayı dağıtma ve uygulamada hata ayıklama

Artık bir uygulamanız olduğuna göre uygulamayı çalıştırın.

Hata ayıklama için uygulamayı dağıtmak üzere Visual Studio'da `F5` tuşuna basın.

>[!NOTE]
>Uygulamayı yerel olarak ilk kez çalıştırdığınızda ve dağıttığınızda, Visual Studio hata ayıklama için yerel bir küme oluşturur. Bu işlem biraz zaman alabilir. Küme oluşturma durumu, Visual Studio çıkış penceresinde görüntülenir.

Küme hazır olduğunda SDK'da bulunan yerel küme sistem tepsisi yöneticisi uygulamasından bir bildirim alırsınız.
   
![Yerel küme sistemi tepsisi bildirimi][4]

Uygulama başlatıldığında, Visual Studio otomatik olarak **Tanılama Olay Görüntüleyicisi**'ni getirir. Bu görüntüleyicide hizmetlerinizin izleme çıktısını görürsünüz.
   
![Tanılama olayları görüntüleyicisi][5]

Kullandığımız durum bilgisi olan hizmet şablonu, yalnızca **MyStatefulService.cs**'nin `RunAsync` yönteminde artan sayaç değerini gösterir.

Kodun çalıştığı düğüm dahil olmak üzere daha fazla ayrıntı görmek için olaylardan birini genişletin. Bu durumda, \_Node\_2 genişletilir ancak makinenize göre değişiklik gösterebilir.
   
![Tanılama olayları görüntüleyicisi ayrıntıları][6]

Yerel küme, tek bir makinede barındırılan beş düğüm içerir. Üretim ortamında, her düğüm ayrı fiziksel veya sanal makinelerde barındırılır. Makine kaybı benzetimi gerçekleştirirken aynı zamanda Visual Studio hata ayıklayıcısını denemek için yerel kümedeki düğümlerden birini ele alalım.

**Çözüm Gezgini** penceresinde, **MyStatefulService.cs**'yi açın. 

`RunAsync` yöntemini bulun ve yöntemin ilk satırında bir kesme noktası ayarlayın.

![Durum bilgisi olan hizmetin RunAsync yönteminde kesme noktası ayarlama][7]

**Yerel Küme Yöneticisi** sistem tepsisine sağ tıklayıp **Yerel Kümeyi Yönet**'i seçerek **Service Fabric Explorer** aracını başlatın.

![Yerel Küme Yöneticisi'nden Service Fabric Explorer'ı başlatma][systray-launch-sfx]

[**Service Fabric Explorer**](service-fabric-visualizing-your-cluster.md) kümenin görsel bir gösterimini sağlar. Kümeye dağıtılan uygulama grubunu ve kümeyi oluşturan fiziksel düğüm grubunu içerir.

Sol bölmede **Küme > Düğümler** kısmını genişletin ve kodunuzun çalıştığı düğümü bulun.

Makinenin yeniden çalıştırılmasının benzetimini gerçekleştirmek için **Eylemler > Devre Dışı Bırak (Yeniden Çalıştır)** seçeneklerine tıklayın.

![Service Fabric Explorer'da bir düğümü durdurma][sfx-stop-node]

Bir düğümde yaptığınız işlem sorunsuz şekilde başka bir düğüme devredildiğinden kısa süre içinde Visual Studio'da kesme noktası isabetini göreceksiniz.


Ardından, Tanılama Olayları Görüntüleyicisi'ne geri dönün ve iletileri gözlemleyin. Olaylar farklı bir düğümden geliyor olsa bile sayaç artmaya devam etti.

![Yük devretme sonrası tanılama olayları görüntüleyicisi][diagnostic-events-viewer-detail-post-failover]

## <a name="cleaning-up-the-local-cluster-optional"></a>Yerel kümeyi temizleme (isteğe bağlı)

Bu yerel kümenin gerçek olduğunu unutmayın. Hata ayıklayıcının durdurulması uygulama örneğinizi ve uygulama türünün kaydını kaldırır. Ama küme arka planda çalışmaya devam eder. Yerel kümeyi durdurmaya hazır olduğunuzda, birkaç seçeneğiniz vardır.

### <a name="keep-application-and-trace-data"></a>Uygulamayı ve izleme verilerini koruma

**Yerel Küme Yöneticisi** sistem tepsisi uygulamasına sağ tıklayıp **Yerel Kümeyi Durdur**'u seçerek kümeyi kapatın.

### <a name="delete-the-cluster-and-all-data"></a>Kümeyi ve tüm verileri silme

**Yerel Küme Yöneticisi** sistem tepsisi uygulamasına sağ tıklayıp **Yerel Kümeyi Kaldır**'ı seçerek kümeyi kaldırın. 

Bu seçeneği kullanırsanız, uygulamayı bir sonraki çalıştırışınızda Visual Studio kümeyi yeniden dağıtır. Bu seçeneği yalnızca yerel kümeyi bir süre kullanmayı planlamıyorsanız veya kaynaklarınızı geri kazanmanız gerekiyorsa kullanın.

## <a name="next-steps"></a>Sonraki adımlar
[Reliable services](service-fabric-reliable-services-introduction.md) hakkındaki diğer yazıları okuyun.
<!-- Image References -->

[1]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog.png
[2]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog-2.png
[3]: ./media/service-fabric-create-your-first-application-in-visual-studio/solution-explorer-stateful-service-template.png
[4]: ./media/service-fabric-create-your-first-application-in-visual-studio/local-cluster-manager-notification.png
[5]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer.png
[6]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer-detail.png
[7]: ./media/service-fabric-create-your-first-application-in-visual-studio/runasync-breakpoint.png
[sfx-stop-node]: ./media/service-fabric-create-your-first-application-in-visual-studio/sfe-deactivate-node.png
[systray-launch-sfx]: ./media/service-fabric-create-your-first-application-in-visual-studio/launch-sfx.png
[diagnostic-events-viewer-detail-post-failover]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer-detail-post-failover.png
[sfe-delete-application]: ./media/service-fabric-create-your-first-application-in-visual-studio/sfe-delete-application.png
[switch-cluster-mode]: ./media/service-fabric-create-your-first-application-in-visual-studio/switch-cluster-mode.png
[cluster-setup-success-1-node]: ./media/service-fabric-get-started-with-a-local-cluster/cluster-setup-success-1-node.png

