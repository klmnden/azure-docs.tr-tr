---
title: Visual Studio uygulamanızda hata ayıklama | Microsoft Docs
description: Güvenilirliğini ve performansını hizmetlerinizi geliştirmek ve bunları Visual Studio'da bir yerel geliştirme kümede hata ayıklama tarafından geliştirin.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: ''
ms.assetid: cb888532-bcdb-4e47-95e4-bfbb1f644da4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 4582fd16d08ae8d51460dc8cabfd282e1f2cd43e
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="debug-your-service-fabric-application-by-using-visual-studio"></a>Service Fabric uygulamanızı Visual Studio kullanarak hata ayıklama
> [!div class="op_single_selector"]
> * [Visual Studio/CSharp](service-fabric-debugging-your-application.md) 
> * [Eclipse/Java](service-fabric-debugging-your-application-java.md)
>


## <a name="debug-a-local-service-fabric-application"></a>Yerel bir Service Fabric uygulama hata ayıklama
Saat ve para dağıtma ve yerel bilgisayar geliştirme küme Azure Service Fabric uygulamanızda hata ayıklama kaydedebilirsiniz. Visual Studio 2017 veya Visual Studio 2015, uygulamayı yerel kümeye dağıtın ve hata ayıklayıcısı uygulamanız tüm örneklerini otomatik olarak bağlan.

1. İçindeki adımları izleyerek yerel bir geliştirme kümesi Başlat [, Service Fabric geliştirme ortamını ayarlama](service-fabric-get-started.md).
2. Tuşuna **F5** veya **hata ayıklama** > **hata ayıklamayı Başlat**.
   
    ![Bir uygulamanın hata ayıklamayı Başlat][startdebugging]
3. Kesme kod ve uygulama üzerinden adım komutlarda tıklayarak **hata ayıklama** menüsü.
   
   > [!NOTE]
   > Visual Studio, uygulamasının tüm örneklerini ekler. Kod atlama olsa da, kesme noktaları eşzamanlı oturumlarında kaynaklanan birden çok işlemler tarafından isabet. Bunlar, her kesme iş parçacığı kimliğine koşullu yaparak veya tanılama olayları kullanarak isabet sonra kesme noktaları devre dışı bırakmayı deneyin.
   > 
   > 
4. **Tanılama olayları** penceresi gerçek zamanlı olarak tanılama olayları görebilecek şekilde otomatik olarak açılır.
   
    ![Gerçek zamanlı tanılama olaylarını görüntüle][diagnosticevents]
5. Ayrıca açabilirsiniz **tanılama olayları** Cloud Explorer penceresinde.  Altında **Service Fabric**, herhangi bir düğüme sağ tıklayın ve seçin **görünüm akış izlemeleri**.
   
    ![Tanılama Olayları penceresini açın][viewdiagnosticevents]
   
    Belirli bir hizmet veya uygulama için izlemeleri filtrelemek istiyorsanız, yalnızca belirli bir hizmeti veya uygulamayı akış izlemeleri etkinleştirin.
6. Tanılama Olayları görülebilir otomatik olarak oluşturulan içinde **ServiceEventSource.cs** dosya ve uygulama kodundan denir.
   
    ```csharp
    ServiceEventSource.Current.ServiceMessage(this, "My ServiceMessage with a parameter {0}", result.Value.ToString());
    ```
7. **Tanılama olayları** penceresi filtreleme, duraklatma ve gerçek zamanlı olayları inceleniyor destekler.  Olay iletisi içeriği de dahil olmak üzere, bir basit bir dize arama filtredir.
   
    ![Filtre, duraklatma ve sürdürme veya gerçek zamanlı olayları inceleyin.][diagnosticeventsactions]
8. Hata Ayıklama Hizmetleri başka bir uygulama hata ayıklama gibi değildir. Kesme noktaları Visual Studio aracılığıyla kolay hata ayıklamak için normal olarak ayarlarsınız. Güvenilir koleksiyonları birden çok düğümü arasında çoğaltma olsa da, bunlar hala IEnumerable uygulayın. Bu, sonuçlar görünümü Visual Studio'da hata ayıklarken içine koyduğunuz görmek için kullanabileceğiniz anlamına gelir. Yalnızca bir kesme noktası kodunuzda herhangi bir yere ayarlayın.
   
    ![Bir uygulamanın hata ayıklamayı Başlat][breakpoint]

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="debug-a-remote-service-fabric-application"></a>Uzak bir Service Fabric uygulaması hata ayıklama
Service Fabric uygulamalarınızı Azure Service Fabric kümesi üzerinde çalıştırıyorsanız, bu, doğrudan Visual Studio uzaktan hata ayıklama imkanınız olur.

> [!NOTE]
> Özellik gerektirir [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) ve [.NET 2.9 için Azure SDK](https://azure.microsoft.com/downloads/).    
> 
> 

<!-- -->
> [!WARNING]
> Uzaktan hata ayıklama çalışan uygulamaları üzerindeki etkiyi nedeniyle üretim ortamlarında kullanılmamalıdır ve geliştirme ve test senaryoları için tasarlanmıştır.
> 
> 

1. Kümenizdeki gidin **Cloud Explorer**seçin ve sağ tıklatıp **etkinleştirmek hata ayıklama**
   
    ![Uzaktan hata ayıklamayı etkinleştirme][enableremotedebugging]
   
    Bu, küme düğümleri, yanı gerekli ağ yapılandırmaları uzaktan hata ayıklama uzantısı etkinleştirme işlemini devre dışı tetiklersiniz.
2. Küme düğümünde sağ **Cloud Explorer**ve seçin **ekleme hata ayıklayıcı**
   
    ![Hata ayıklayıcıyı Ekle][attachdebugger]
3. İçinde **ekleme işlemi için** iletişim kutusunda, hata ayıklama ve istediğiniz işlemi seçin **Ekle**
   
    ![İşlem seçin][chooseprocess]
   
    İçin iliştirmek istediğiniz işlemin adını, hizmet projesi derleme adı eşittir.
   
    Hata ayıklayıcı işlem çalışan tüm düğümlere ekleyecek.
   
   * Bir durum bilgisi olmayan Hizmetin nerede ayıkladığınız durumda da, tüm düğümlerde hizmetin tüm örneklerine ait hata ayıklama oturumu bir parçasıdır.
   * Durum bilgisi olan hizmet hata ayıklama, herhangi bir bölümünün yalnızca birincil çoğaltma etkin ve bu nedenle, hata ayıklayıcı tarafından yakalanan olacaktır. Birincil çoğaltma hata ayıklama oturumu sırasında geçerse, bu çoğaltma işlemi hata ayıklama oturumunun parçası olmaya devam edecektir.
   * Yalnızca ilgili bölümleri veya belirli bir hizmeti örnekleri yakalamak için yalnızca belirli bir bölüm veya örnek ayırmak için koşullu kesme noktaları kullanabilirsiniz.
     
     ![Koşullu kesme noktası][conditionalbreakpoint]
     
     > [!NOTE]
     > Şu an bir Service Fabric kümesi birden çok örneğini aynı hizmet yürütülebilir adı ile hata ayıklama desteklemez.
     > 
     > 
4. Uygulamanızın hatalarını ayıklama tamamladıktan sonra uzaktan hata ayıklama uzantısı kümede sağ tıklayarak devre dışı bırakabilirsiniz **Cloud Explorer** ve **devre dışı bırakmak hata ayıklama**
   
    ![Uzaktan hata ayıklama devre dışı bırak][disableremotedebugging]

## <a name="streaming-traces-from-a-remote-cluster-node"></a>Uzak küme düğümüne izlemeleri akış
Ayrıca akış izlemeleri için doğrudan bir uzak küme düğümünden Visual Studio mümkün değildir. Bu özellik, akış ETW İzleme olayları, Service Fabric küme düğümünde üretilen olanak tanır.

> [!NOTE]
> Bu özellik gerektirir [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) ve [.NET 2.9 için Azure SDK](https://azure.microsoft.com/downloads/).
> Bu özellik yalnızca Azure üzerinde çalışan kümelerle destekler.
> 
> 

<!-- -->
> [!WARNING]
> İzlemeler akış çalışan uygulamaları üzerindeki etkiyi nedeniyle üretim ortamlarında kullanılmamalıdır ve geliştirme ve test senaryoları için tasarlanmıştır.
> Bir üretim senaryosunda, Azure Tanılama'yı kullanarak olayları iletme yararlanmalıdır.
> 
> 

1. Kümenizdeki gidin **Cloud Explorer**seçin ve sağ tıklatıp **akış izlemeleri etkinleştir**
   
    ![Uzak akış izlemeleri etkinleştir][enablestreamingtraces]
   
    Bu, küme düğümleri, yanı gerekli ağ yapılandırmaları akış izlemeleri uzantı etkinleştiriliyor işlemi kapatmak tetiklersiniz.
2. Genişletme **düğümleri** öğesinde **Cloud Explorer**, izlemeleri akış ve seçmek için düğüme sağ tıklatın **görünüm akış izlemeleri**
   
    ![İzlemeler akış uzak görünümü][viewremotestreamingtraces]
   
    İzlemeler görmek istediğiniz sayıda düğümleri için 2. adımı yineleyin. Her düğüm akış adanmış bir pencerede gösterir.
   
    Artık Service Fabric ve hizmetlerinizi tarafından gösterilen izlemeleri görüyor. Yalnızca belirli bir uygulama göstermek için olayları filtrelemek istiyorsanız, uygulama filtresi adını yazmanız yeterlidir.
   
    ![İzlemeler akış görüntüleme][viewingstreamingtraces]
3. Akış izlemeleri kümenizden tamamladıktan sonra uzak akış izlemeleri, kümeye sağ tıklayarak devre dışı bırakabilirsiniz **Cloud Explorer** ve **akış izlemelerini devre dışı bırak**
   
    ![Uzak akış izlemelerini devre dışı bırak][disablestreamingtraces]

## <a name="next-steps"></a>Sonraki adımlar
* [Service Fabric hizmeti test](service-fabric-testability-overview.md).
* [Visual Studio'da, Service Fabric uygulamaları yönetmek](service-fabric-manage-application-in-visual-studio.md).

<!--Image references-->
[startdebugging]: ./media/service-fabric-debugging-your-application/startdebugging.png
[diagnosticevents]: ./media/service-fabric-debugging-your-application/diagnosticevents.png
[viewdiagnosticevents]: ./media/service-fabric-debugging-your-application/viewdiagnosticevents.png
[diagnosticeventsactions]: ./media/service-fabric-debugging-your-application/diagnosticeventsactions.png
[breakpoint]: ./media/service-fabric-debugging-your-application/breakpoint.png
[enableremotedebugging]: ./media/service-fabric-debugging-your-application/enableremotedebugging.png
[attachdebugger]: ./media/service-fabric-debugging-your-application/attachdebugger.png
[chooseprocess]: ./media/service-fabric-debugging-your-application/chooseprocess.png
[conditionalbreakpoint]: ./media/service-fabric-debugging-your-application/conditionalbreakpoint.png
[disableremotedebugging]: ./media/service-fabric-debugging-your-application/disableremotedebugging.png
[enablestreamingtraces]: ./media/service-fabric-debugging-your-application/enablestreamingtraces.png
[viewingstreamingtraces]: ./media/service-fabric-debugging-your-application/viewingstreamingtraces.png
[viewremotestreamingtraces]: ./media/service-fabric-debugging-your-application/viewremotestreamingtraces.png
[disablestreamingtraces]: ./media/service-fabric-debugging-your-application/disablestreamingtraces.png
