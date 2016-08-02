<properties
   pageTitle="Visual Studio'da ilk Service Fabric uygulamanızı oluşturma | Microsoft Azure"
   description="Visual Studio'yu kullanarak bir Service Fabric uygulaması oluşturma, dağıtma ve uygulamada hata ayıklama"
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/27/2016"
   ms.author="ryanwi"/>

# Visual Studio'da ilk Azure Service Fabric uygulamanızı oluşturma

Service Fabric SDK, Service Fabric uygulamaları oluşturmak, dağıtmak ve bu uygulamalarda hata ayıklamak için şablonlar ve araçlar sağlayan Visual Studio ürününe yönelik bir eklenti içerir. Bu konu başlığı, Visual Studio'da ilk uygulamanızı oluşturma işleminizde size kılavuzluk eder.

## Önkoşullar

Başlamadan önce [geliştirme ortamınızı ayarladığınızdan](service-fabric-get-started.md) emin olun.

## Görüntülü kılavuz

Aşağıdaki video bu öğreticideki adımlarda size kılavuzluk eder:

>[AZURE.VIDEO creating-your-first-service-fabric-application-in-visual-studio]

## Uygulama oluşturma

Service Fabric uygulaması bir veya birden çok hizmet içerebilir. Bu hizmetlerin her biri uygulamanın işlevselliğini aktarma konusunda belirli bir role sahiptir. Yeni Proje sihirbazını kullanarak ilk hizmet projenizin yanı sıra bir uygulama projesi de oluşturabilirsiniz. Daha sonra ilave hizmetler ekleyebilirsiniz.

1. Visual Studio'yu yönetici olarak başlatın.

2. **Dosya > Yeni Proje > Bulut > Service Fabric Uygulaması** seçeneklerine tıklayın.

3. Uygulamaya bir ad verin ve **Tamam**'a tıklayın.

    ![Visual Studio'da yeni proje iletişim kutusu][1]

4. Bir sonraki sayfada uygulamanıza dahil edeceğiniz ilk hizmet türü seçiminiz sorulur. Bu öğreticinin amaçları doğrultusunda **Durum Bilgisi Olan** seçeneğini belirledik. Adlandırın ve **Tamam**'a tıklayın.

    ![Visual Studio'da yeni hizmet iletişim kutusu][2]

    >[AZURE.NOTE] Seçenekler hakkında daha fazla bilgi için bkz. [Altyapı seçme](service-fabric-choose-framework.md).

    Visual Studio uygulama projesini ve durum bilgisi olan hizmet projesini oluşturup bu projeleri Çözüm Gezgini'nde görüntüler.

    ![Durum bilgisi olan hizmeti içeren uygulama oluşturulduktan sonra Çözüm Gezgini][3]

    Uygulama projesi doğrudan kod içermez. Bunun yerine, bir dizi hizmet projesine başvuru sağlar. Ayrıca, bunlardan farklı olarak üç tür içerik daha barındırır:

    - **Yayımlama profilleri**: Farklı ortamlar için araç kullanımı tercihlerini yönetmek için kullanılır.

    - **Betikler**: Uygulamanızı dağıtmak/yükseltmek için bir PowerShell betiği içerir. Bu betik, Visual Studio tarafından arka planda kullanılır ve doğrudan komut satırından çağırılabilir.

    - **Uygulama tanımı**: *ApplicationPackageRoot* kısmında uygulama bildirimi içerir. Ayrıca, uygulamayı tanımlayan ve uygulamanızı belirli bir ortam için özel olarak yapılandırmanıza olanak sağlayan *ApplicationParameters* kısmında da ilişkili uygulama parametreleri dosyalarını barındırır.

    Hizmet projesinin içeriklerine genel bakış için bkz. [Reliable Services ile çalışmaya başlama](service-fabric-reliable-services-quick-start.md).

## Uygulamayı dağıtma ve uygulamada hata ayıklama

Artık bir uygulamanız olduğuna göre uygulamayı çalıştırmayı deneyebilirsiniz.

1. Hata ayıklama için uygulamayı dağıtmak üzere Visual Studio'da F5'e basın.

    >[AZURE.NOTE] Visual Studio geliştirme için yerel bir küme oluşturduğundan ilk uygulamada bu işlem biraz zaman alabilir. Yerel küme, çoklu makine kümesinde derleyeceğiniz platform kodunu yalnızca tek bir makinede çalıştırır. Küme oluşturma durumunu Visual Studio çıktı penceresinde görürsünüz.

    Küme hazır olduğunda SDK'da bulunan yerel küme sistemi tepsisi yöneticisi uygulamasından bir bildirim alırsınız.

    ![Yerel küme sistemi tepsisi bildirimi][4]

2. Uygulama başladığında Visual Studio, otomatik olarak Tanılama Olay Görüntüleyicisi'ni getirir. Bu görüntüleyicide hizmetin izleme çıktısını görürsünüz.

    ![Tanılama olayları görüntüleyicisi][5]

    Durum bilgisi olan hizmet şablonu durumunda, iletiler yalnızca MyStatefulService.cs. öğesinin `RunAsync` yönteminde artan sayaç değerini gösterir.

3. Kodun çalıştığı düğüm dahil olmak üzere daha fazla ayrıntı görmek için olaylardan birini genişletin. Bu durumda, düğüm _Node_2 şeklindedir ancak makinenize göre değişiklik gösterebilir.

    ![Tanılama olayları görüntüleyicisi ayrıntıları][6]

    Yerel küme, tek bir makinede barındırılan beş düğüm içerir. Düğümlerin farklı makinelerde bulunduğu, beş düğümlü kümeyi taklit eder. Makine kaybı benzetimi gerçekleştirmek ve aynı zamanda Visual Studio hata ayıklayıcısını denemek için yerel kümedeki düğümlerden birini ele alalım.

    >[AZURE.NOTE] Proje şablonu tarafından gösterilen uygulama tanılama olayları dahili `ServiceEventSource` sınıfını kullanır. Daha fazla bilgi için bkz. [Hizmetleri yerel olarak izleme ve tanılama](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

4. StatefulService'ten (örneğin, MyStatefulService'ten) türetilen hizmet projenizdeki sınıfı bulun ve `RunAsync` yönteminin ilk satırında bir kesme noktası ayarlayın.

    ![Durum bilgisi olan hizmetin RunAsync yönteminde kesme noktası ayarlama][7]

5. Yerel Küme Yöneticisi sistem tepsisine sağ tıklayın ve Service Fabric Explorer'ı başlatmak için **Yerel Kümeyi Yönet** seçeneğini belirleyin.

    ![Yerel Küme Yöneticisi'nden Service Fabric Explorer'ı başlatma][systray-launch-sfx]

    Service Fabric Explorer, kümeye dağıtılan uygulamalar dizisi ve kümeyi oluşturan fiziksel düğümlerin dizisi dahil olmak üzere bir kümenin görsel bir bildirimini sunar. Service Fabric Explorer hakkında daha fazla bilgi edinmek için bkz. [Kümenizi görselleştirme](service-fabric-visualizing-your-cluster.md).

6. Sol bölmede **Küme > Düğümler** kısmını genişletin ve kodunuzun çalıştığı düğümü bulun.

7. Makinenin yeniden çalıştırılmasının benzetimini gerçekleştirmek için **Eylemler > Devre Dışı Bırak (Yeniden Çalıştır)** seçeneklerine tıklayın. (Ayrıca, sol bölmedeki düğüm listesi görünümünde bulunan bağlam menüsünden üç nokta seçerek de bu işlemi gerçekleştirebileceğinizi unutmayın).

    ![Service Fabric Explorer'da bir düğümü durdurma][sfx-stop-node]

    Bir düğümde yaptığınız işlem sorunsuz şekilde başka bir düğüme devredildiğinden kısa süre içinde Visual Studio'da kesme noktası isabetini göreceksiniz.

8. Tanılama Olayları Görüntüleyicisi'ne geri dönün ve iletileri gözlemleyin. Olaylar gerçekte farklı bir düğümden geliyor olsa bile sayacın artmaya devam ettiğini unutmayın.

    ![Yük devretme sonrası tanılama olayları görüntüleyicisi][diagnostic-events-viewer-detail-post-failover]

### Temizleme

  Sonlandırmadan önce, yerel kümenin büyük ölçüde kaynak kullanımı gerektirdiğini unutmayın. Hata ayıklayıcıyı durdurduktan ve Visual Studio'yu kapattıktan sonra bile uygulamalarınız arka planda çalışmaya devam eder. Uygulamalarınızın yapısına bağlı olarak bu arka plan etkinliği makinenizde önemli miktarda kaynağı kullanabilir. Bu durumu yönetmek için birçok seçenek sunulur:

  1. Tek bir uygulamayı ve tüm verilerini kaldırmak için **EYLEMLER** menüsü veya sol bölmedeki uygulama listesi görünümündeki bağlam menüsü ile birlikte Service Fabric Explorer hizmetinde bulunan **Uygulamayı sil** eylemini kullanın.

    ![Service Fabric Explorer'da uygulama silme][sfe-delete-application]

  2. Uygulamayı kümeden sildikten sonra kod, yapılandırma ve kümenin görüntü deposu dahil olmak üzere uygulamanın paketini kaldıran uygulama için **Sağlamayı Kaldırma Türü** seçeneğini belirleyebilirsiniz.
  3. Kümeyi kapatıp uygulama verilerini ve izlemelerini tutmak için sistem tepsisi uygulamasında **Yerel Kümeyi Durdur**'a tıklayın.

  4. Kümeyi tamamen silmek için sistem tepsisi uygulamasında **Yerel Kümeyi Kaldır**'a tıklayın. Visual Studio'da F5'e bir sonraki basışınızda bu seçeneğin başka bir yavaş dağıtımla sonuçlanacağını unutmayın. Bu seçeneği yalnızca yerel kümeyi bir süre kullanmayı planlamıyorsanız veya kaynaklarınızı geri kazanmanız gerekiyorsa kullanın.



## Sonraki adımlar

<!--
Temporarily removing this link because we have removed the ASP.NET template.

 - [See how you can expose your services to the Internet with a web service front end](service-fabric-add-a-web-frontend.md)
-->
- [Azure'da küme oluşturmayı öğrenin](service-fabric-cluster-creation-via-portal.md)
- [Reliable Services hakkında daha fazla bilgi edinin](service-fabric-reliable-services-quick-start.md)
- [Reliable Actors programlama modelini kullanan bir hizmet oluşturmayı deneyin](service-fabric-reliable-actors-get-started.md)

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



<!--HONumber=Jun16_HO2-->


