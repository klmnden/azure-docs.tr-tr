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
   ms.date="09/28/2016"
   ms.author="ryanwi"/>



# İlk Azure Service Fabric uygulamanızı oluşturma

> [AZURE.SELECTOR]
- [C#](service-fabric-create-your-first-application-in-visual-studio.md)
- [Java](service-fabric-create-your-first-linux-application-with-java.md)

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

4. Bir sonraki sayfada, uygulamanıza dahil edilecek ilk hizmet türü olarak **Durum bilgisi olan**’ı seçin. Adlandırın ve **Tamam**'a tıklayın.

    ![Visual Studio'da yeni hizmet iletişim kutusu][2]

    >[AZURE.NOTE] Seçenekler hakkında daha fazla bilgi için bkz. [Service Fabric programlama modeline genel bakış](service-fabric-choose-framework.md).

    Visual Studio uygulama projesini ve durum bilgisi olan hizmet projesini oluşturup bu projeleri Çözüm Gezgini'nde görüntüler.

    ![Durum bilgisi olan hizmeti içeren uygulama oluşturulduktan sonra Çözüm Gezgini][3]

    Uygulama projesi doğrudan kod içermez. Bunun yerine, bir dizi hizmet projesine başvuru sağlar. Ayrıca, bunlardan farklı olarak üç tür içerik daha barındırır:

    - **Yayımlama profilleri**: Farklı ortamlar için araç kullanımı tercihlerini yönetmek için kullanılır.

    - **Betikler**: Uygulamanızı dağıtmak/yükseltmek için bir PowerShell betiği içerir. Visual Studio, Visual Studio tarafından sunulan sahne arkası betiği kullanır. Bu betik komut satırından doğrudan da çağrılabilir.

    - **Uygulama tanımı**: *ApplicationPackageRoot* altındaki uygulama bildirimini içerir. İlişkili uygulama parametre dosyaları, uygulamayı tanımlayan ve belirli bir ortam için özel olarak yapılandırmanıza imkan tanıyan *ApplicationParameters* altında bulunur.

    Hizmet projesinin içeriklerine genel bakış için bkz. [Reliable Services ile çalışmaya başlama](service-fabric-reliable-services-quick-start.md).

## Uygulamayı dağıtma ve uygulamada hata ayıklama

Artık bir uygulamanız olduğuna göre uygulamayı çalıştırmayı deneyin.

1. Hata ayıklama için uygulamayı dağıtmak üzere Visual Studio'da F5'e basın.

    >[AZURE.NOTE] Visual Studio geliştirme için yerel bir küme oluşturduğundan, ilk seferde dağıtım biraz zaman alır. Yerel küme, çoklu makine kümesinde derleyeceğiniz platform kodunu yalnızca tek bir makinede çalıştırır. Visual Studio çıktı penceresinde küme oluşturma durumu görüntülenir.

    Küme hazır olduğunda SDK'da bulunan yerel küme sistem tepsisi yöneticisi uygulamasından bir bildirim alırsınız.

    ![Yerel küme sistemi tepsisi bildirimi][4]

2. Uygulama başladığında, Visual Studio otomatik olarak Tanılama Olay Görüntüleyicisi'ni getirir. Bu görüntüleyicide hizmetin izleme çıktısını görürsünüz.

    ![Tanılama olayları görüntüleyicisi][5]

    Durum bilgisi olan hizmet şablonu söz konusu olduğunda, iletiler yalnızca MyStatefulService.cs. öğesinin `RunAsync` yönteminde artan sayaç değerini gösterir.

3. Kodun çalıştığı düğüm dahil olmak üzere daha fazla ayrıntı görmek için olaylardan birini genişletin. Bu durumda, düğüm _Node_2 şeklindedir ancak makinenize göre değişiklik gösterebilir.

    ![Tanılama olayları görüntüleyicisi ayrıntıları][6]

    Yerel küme, tek bir makinede barındırılan beş düğüm içerir. Düğümlerin farklı makinelerde bulunduğu, beş düğümlü kümeyi taklit eder. Makine kaybı benzetimi gerçekleştirirken aynı zamanda Visual Studio hata ayıklayıcısını denemek için yerel kümedeki düğümlerden birini ele alalım.

    >[AZURE.NOTE] Proje şablonu tarafından gösterilen uygulama tanılama olayları pakete eklenen `ServiceEventSource` sınıfını kullanır. Daha fazla bilgi için bkz. [Hizmetleri yerel olarak izleme ve tanılama](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

4. StatefulService'ten (örneğin, MyStatefulService'ten) türetilen hizmet projenizdeki sınıfı bulun ve `RunAsync` yönteminin ilk satırında bir kesme noktası ayarlayın.

    ![Durum bilgisi olan hizmetin RunAsync yönteminde kesme noktası ayarlama][7]

5. Yerel Küme Yöneticisi sistem tepsisine sağ tıklayın ve Service Fabric Explorer'ı başlatmak için **Yerel Kümeyi Yönet**’i seçin.

    ![Yerel Küme Yöneticisi'nden Service Fabric Explorer'ı başlatma][systray-launch-sfx]

    Service Fabric Explorer, kümeye dağıtılan uygulamalar dizisi ve kümeyi oluşturan fiziksel düğümlerin dizisi dahil olmak üzere bir kümenin görsel bir bildirimini sunar. Service Fabric Explorer hakkında daha fazla bilgi edinmek için bkz. [Kümenizi görselleştirme](service-fabric-visualizing-your-cluster.md).

6. Sol bölmede **Küme > Düğümler** kısmını genişletin ve kodunuzun çalıştığı düğümü bulun.

7. Makinenin yeniden çalıştırılmasının benzetimini gerçekleştirmek için **Eylemler > Devre Dışı Bırak (Yeniden Çalıştır)** seçeneklerine tıklayın. (Sol bölmedeki düğüm listesi görünümünde bulunan bağlam menüsünden de devre dışı bırakabileceğinizi unutmayın.)

    ![Service Fabric Explorer'da bir düğümü durdurma][sfx-stop-node]

    Bir düğümde yaptığınız işlem sorunsuz şekilde başka bir düğüme devredildiğinden kısa süre içinde Visual Studio'da kesme noktası isabetini göreceksiniz.

8. Tanılama Olayları Görüntüleyicisi'ne geri dönün ve iletileri gözlemleyin. Olaylar gerçekte farklı bir düğümden geliyor olsa bile sayacın artmaya devam ettiğini unutmayın.

    ![Yük devretme sonrası tanılama olayları görüntüleyicisi][diagnostic-events-viewer-detail-post-failover]

## Küme moduna geçme

Varsayılan olarak, yerel geliştirme kümesi birden fazla düğüme dağıtılmış hizmetlerin hatalarını ayıklamak için yararlı olan 5 düğümlü bir küme olarak çalışacak şekilde yapılandırılmıştır. Ancak, bir uygulamanın 5 düğümlü dağıtım kümesine dağıtılması biraz zaman alabilir. Uygulamanızı 5 düğüm üzerinde çalıştırmadan kod değişikliklerini hızlıca yinelemek istiyorsanız, geliştirme kümesini 1 Düğümlü moda geçirebilirsiniz. Kodunuzu tek düğümlü bir kümede çalıştırmak için sistem tepsisindeki Yerel Küme Yöneticisi’ne sağ tıklayın ve **Küme Modunu Değiştir -> 1 Düğüm** öğesini seçin.  

![Küme moduna geçme][switch-cluster-mode]

Küme modunuzu değiştirdiğinizde geliştirme kümesi sıfırlanır ve sağlanan ya da küme üzerinde çalışan tüm uygulamalar kaldırılır.

## Temizleme

  Sonlandırmadan önce, yerel kümenin büyük ölçüde kaynak kullanımı gerektirdiğini unutmayın. Hata ayıklayıcının durdurulması uygulama örneğinizi ve uygulama türünün kaydını kaldırır. Ancak, küme arka planda çalışmaya devam eder. Kümeyi yönetmek için birkaç seçeneğiniz vardır:

  1. Kümeyi kapatıp uygulama verilerini ve izlemelerini tutmak için sistem tepsisi uygulamasında **Yerel Kümeyi Durdur**'a tıklayın.

  2. Kümeyi tamamen silmek için sistem tepsisi uygulamasında **Yerel Kümeyi Kaldır**'a tıklayın. Visual Studio'da F5'e bir sonraki basışınızda bu seçeneğin başka bir yavaş dağıtımla sonuçlanacağını unutmayın. Kümeyi yalnızca bir süre kullanmayı planlamıyorsanız veya kaynaklarınızı geri kazanmanız gerekiyorsa silin.

## Sonraki adımlar

- [Azure’da küme](service-fabric-cluster-creation-via-portal.md) veya [Windows’ta tek başına küme](service-fabric-cluster-creation-for-windows-server.md) oluşturma hakkında bilgi edinin.
- [Reliable Services](service-fabric-reliable-services-quick-start.md) veya [Reliable Actors](service-fabric-reliable-actors-get-started.md) programlama modelini kullanan bir hizmet oluşturmayı deneyin.
- Hizmetlerinizi bir [web hizmeti ön ucu](service-fabric-add-a-web-frontend.md) ile İnternet’te nasıl gösterebileceğinizi öğrenin.
- [Hands-on-lab](https://msdnshared.blob.core.windows.net/media/2016/07/SF-Lab-Part-I.docx) adımlarını uygulayın ve durum bilgisi olmayan hizmet oluşturun, izleme ve sistem durumu raporlarını yapılandırın ve bir uygulama yükseltmesi gerçekleştirin.

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



<!--HONumber=Sep16_HO5-->


