---
title: C# ile Azure Service Fabric güvenilir hizmeti oluşturma
description: Visual Studio ile, Azure Service Fabric üzerinde derlenmiş bir Reliable Services uygulaması oluşturun, dağıtın ve hatalarını ayıklayın.
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
ms.date: 03/14/2018
ms.author: ryanwi
ms.openlocfilehash: 858e322fd7e516f756aa209be92745efa6cf75f7
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="create-your-first-c-service-fabric-stateful-reliable-services-application"></a>İlk C# Service Fabric durum bilgisi olan Reliable Services uygulamanızı oluşturma

Yalnızca birkaç dakika içinde Windows üzerinde .NET için ilk Azure Service Fabric uygulamanızı nasıl dağıtacağınızı öğrenin. Tamamladığınızda, Reliable Services uygulamasıyla çalışan bir yerel kümeniz olacaktır.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce [geliştirme ortamınızı ayarladığınızdan](service-fabric-get-started.md) emin olun. Bu işlem, Service Fabric SDK'sını ve Visual Studio 2017 veya 2015'i yüklemeyi de içerir.

## <a name="create-the-application"></a>Uygulama oluşturma

1. Visual Studio'yu yönetici olarak başlatın.

2. Ctrl+Shift+N tuşlarını seçerek proje oluşturun.

3. **Yeni Proje** iletişim kutusunda **Bulut** > **Service Fabric Uygulaması**'nı seçin.

4. Uygulamaya **MyApplication** adını verin. Sonra **Tamam**’ı seçin.

   ![Visual Studio'da yeni proje iletişim kutusu][1]

5. Sonraki iletişim kutusunda her türde Service Fabric uygulaması oluşturabilirsiniz. Bu hızlı başlangıç için **.Net Core 2.0** > **Durum Bilgisi Olan Hizmet** seçeneklerini belirleyin.

6. Hizmeti **MyStatefulService** olarak adlandırın. Sonra **Tamam**’ı seçin.

    ![Visual Studio'da yeni hizmet iletişim kutusu][2]

    Visual Studio uygulama projesini ve durum bilgisi olan hizmet projesini oluşturur. Ardından bu projeleri Çözüm Gezgini'nde görüntüler.

    ![Durum bilgisi olan hizmeti içeren uygulama oluşturulduktan sonra Çözüm Gezgini][3]

    Uygulama projesi (**MyApplication**) hiçbir kod içermez. Bunun yerine, bir dizi hizmet projesine başvuru sağlar. Ayrıca üç farklı içerik türü daha vardır:

    * **Yayımlama profilleri**  
    Farklı ortamlarda dağıtıma yönelik profiller.

    * **Betikler**  
    Uygulamanızı dağıtmaya veya yükseltmeye yönelik PowerShell betikleri.

    * **Uygulama tanımı**  
*ApplicationPackageRoot* altında uygulamanızın birleşimini açıklayan ApplicationManifest.xml dosyasını içerir. İlişkili uygulama parametresi dosyaları *ApplicationParameters* altındadır ve ortama özgü parametreleri belirtmek için kullanılabilir. Visual Studio, ilişkili yayımlama profilinde belirtilen uygulama parametresi dosyasını seçer.
    
Hizmet projesinin içeriklerine genel bakış için bkz. [Reliable Services ile çalışmaya başlama](service-fabric-reliable-services-quick-start.md).

## <a name="deploy-and-debug-the-application"></a>Uygulamayı dağıtma ve uygulamada hata ayıklama

Artık bir uygulamanız olduğuna göre, aşağıdaki adımları izleyerek bu uygulamayı çalıştırın, dağıtın ve hatalarını ayıklayın.

1. Hata ayıklama amacıyla uygulamayı dağıtmak için Visual Studio'da F5'i seçin.

    >[!NOTE]
    >Uygulamayı yerel olarak ilk kez çalıştırdığınızda ve dağıttığınızda, Visual Studio hata ayıklama için yerel bir küme oluşturur. Bu işlem biraz zaman alabilir. Küme oluşturma durumu, Visual Studio çıkış penceresinde görüntülenir.

    Küme hazır olduğunda, SDK'da bulunan yerel küme sistem tepsisi yöneticisi uygulamasından bir bildirim alırsınız.
    
    ![Yerel küme sistemi tepsisi bildirimi][4]

    Uygulama başlatıldıktan sonra, Visual Studio otomatik olarak Tanılama Olay Görüntüleyicisi'ni getirir. Bu görüntüleyicide hizmetlerinizin izleme çıktısını görürsünüz.
    
    ![Tanılama olayları görüntüleyicisi][5]

    >[!NOTE]
    >Olayların Tanılama Olay Görüntüleyicisi'nde hemen izlenmeye başlaması gerekir. Tanılama Olay Görüntüleyicisi'ni el ile yapılandırmanız gerekiyorsa, önce **MyStatefulService** projesinde yer alan `ServiceEventSource.cs` dosyasını açın. `ServiceEventSource` sınıfının üstündeki `EventSource` özniteliğinin değerini kopyalayın. Aşağıdaki örnekte olay kaynağı `"MyCompany-MyApplication-MyStatefulService"` olarak adlandırılır. Bu, sizin durumunuzda farklılık gösterebilir.
>
    >![Hizmet Olay Kaynağı Adını Bulma][service-event-source-name]


2. Ardından, **ETW Sağlayıcıları** iletişim kutusunu açın. Sonra **Tanılama Olayları** sekmesindeki dişli simgesini seçin. Kopyaladığınız olay kaynağının adını **ETW Sağlayıcıları** giriş kutusuna yapıştırın. Sonra **Uygula** düğmesini seçin. Bu, izleme olaylarını otomatik olarak başlatır.

    ![Tanılama Olayı kaynak adını ayarlama][setting-event-source-name]

    Artık Tanılama Olayları penceresinde olayları görüyor olmalısınız.

    Durum bilgisi olan hizmet şablonu, **MyStatefulService.cs**'nin `RunAsync` yönteminde artan sayaç değerini gösterir.

3. Kodun çalıştığı düğüm dahil olmak üzere daha fazla ayrıntı görmek için olaylardan birini genişletin. Bu durumda, **\_Node\_0** genişletilir ancak sizin makinenize farklı olabilir.
   
    ![Tanılama olayları görüntüleyicisi ayrıntıları][6]

4. Yerel küme, tek bir makinede barındırılan beş düğüm içerir. Üretim ortamında, her düğüm ayrı fiziksel veya sanal makinelerde barındırılır. Visual Studio hata ayıklayıcısını denerken makine kaybı benzetimi gerçekleştirmek için yerel kümedeki düğümlerden birini devre dışı bırakalım.

5. **Çözüm Gezgini** penceresinde, **MyStatefulService.cs**'yi açın. 

6. `RunAsync` yöntemini bulun ve yöntemin ilk satırında bir kesme noktası ayarlayın.

    ![Durum bilgisi olan hizmetin RunAsync yönteminde kesme noktası ayarlama][7]

7. **Yerel Küme Yöneticisi** sistem tepsisi uygulamasına sağ tıklayıp **Yerel Kümeyi Yönet**'i seçerek Service Fabric Explorer aracını başlatın.

    ![Yerel küme yöneticisinden Service Fabric Explorer'ı başlatma][systray-launch-sfx]

    [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) kümenin görsel bir gösterimini sağlar. Kümeye dağıtılan uygulama grubunu ve kümeyi oluşturan fiziksel düğüm grubunu içerir.

8. Sol bölmede **Küme** > **Düğümler** kısmını genişletin ve kodunuzun çalıştığı düğümü bulun. Ardından, makine yeniden başlatma işleminin benzetimini yapmak için **Eylemler** > **Devre Dışı Bırak (Yeniden Çalıştır)** öğesini seçin.

    ![Service Fabric Explorer'da bir düğümü durdurma][sfx-stop-node]

    Bir düğümde yaptığınız işlem sorunsuz şekilde başka bir düğüme devredildiğinden kısa süre içinde Visual Studio'da kesme noktası isabetini göreceksiniz.

9. Ardından, Tanılama Olayları Görüntüleyicisi'ne geri dönün ve iletileri gözlemleyin. Olaylar farklı bir düğümden geliyor olsa bile sayaç artmaya devam etti.

    ![Yük devretme sonrası tanılama olayları görüntüleyicisi][diagnostic-events-viewer-detail-post-failover]

## <a name="clean-up-the-local-cluster-optional"></a>Yerel kümeyi temizleme (isteğe bağlı)

Bu yerel kümenin gerçek olduğunu unutmayın. Hata ayıklayıcının durdurulması uygulama örneğinizi ve uygulama türünün kaydını kaldırır. Ama küme arka planda çalışmaya devam eder. Yerel kümeyi durdurmaya hazır olduğunuzda, birkaç seçeneğiniz vardır.

### <a name="keep-application-and-trace-data"></a>Uygulamayı ve izleme verilerini koruma

**Yerel Küme Yöneticisi** sistem tepsisi uygulamasına sağ tıklayıp **Yerel Kümeyi Durdur**'u seçerek kümeyi kapatın.

### <a name="delete-the-cluster-and-all-data"></a>Kümeyi ve tüm verileri silme

Kümeyi kaldırmak için **Yerel Küme Yöneticisi** sistem tepsisi uygulamasına sağ tıklayın. Ardından **Yerel Kümeyi Kaldır**'ı seçin. 

Bu seçeneği kullanırsanız, uygulamayı bir sonraki çalıştırışınızda Visual Studio kümeyi yeniden dağıtır. Bu seçeneği yalnızca yerel kümeyi bir süre kullanmayı planlamıyorsanız veya kaynaklarınızı geri kazanmanız gerekiyorsa kullanın.

## <a name="next-steps"></a>Sonraki adımlar
[Reliable Services](service-fabric-reliable-services-introduction.md) hakkındaki diğer yazıları okuyun.
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
[service-event-source-name]: ./media/service-fabric-create-your-first-application-in-visual-studio/event-source-attribute-value.png
[setting-event-source-name]: ./media/service-fabric-create-your-first-application-in-visual-studio/setting-event-source-name.png
[switch-cluster-mode]: ./media/service-fabric-create-your-first-application-in-visual-studio/switch-cluster-mode.png
[cluster-setup-success-1-node]: ./media/service-fabric-get-started-with-a-local-cluster/cluster-setup-success-1-node.png
[service-event-source-name]: ./media/service-fabric-create-your-first-application-in-visual-studio/event-source-attribute-value.png
[setting-event-source-name]: ./media/service-fabric-create-your-first-application-in-visual-studio/setting-event-source-name.png
