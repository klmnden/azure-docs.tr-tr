---
title: Visual Studio'da uygulamanızın hatalarını ayıklama | Microsoft Docs
description: Geliştirme ve bunları Visual Studio'da bir yerel geliştirme kümesinde hata ayıklama güvenilirliğini ve performansını hizmetlerinizi geliştirin.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: ''
ms.assetid: cb888532-bcdb-4e47-95e4-bfbb1f644da4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.custom: vs-azure
ms.workload: azure-vs
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 682059914b5d86f5e670e373a4acf3e4ac6246ba
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66428218"
---
# <a name="debug-your-service-fabric-application-by-using-visual-studio"></a>Visual Studio kullanarak Service Fabric uygulamanızı hata ayıklama
> [!div class="op_single_selector"]
> * [Visual Studio/CSharp](service-fabric-debugging-your-application.md) 
> * [Eclipse/Java](service-fabric-debugging-your-application-java.md)
>


## <a name="debug-a-local-service-fabric-application"></a>Yerel bir Service Fabric uygulamasının hatalarını ayıklama
Dağıtarak ve Azure Service Fabric uygulamanızı yerel bilgisayar geliştirme kümedeki hata ayıklama zamandan ve paradan tasarruf. Visual Studio 2019 veya 2015'i, uygulamayı yerel kümeye dağıtma ve otomatik olarak hata ayıklayıcı uygulamanızın tüm örneklerine bağlanabilirsiniz. Visual Studio hata ayıklayıcıya bağlanmak için yönetici olarak çalıştırmanız gerekir.

1. İçindeki adımları izleyerek bir yerel geliştirme kümesi Başlat [Service Fabric geliştirme ortamınızı ayarlama](service-fabric-get-started.md).
2. Tuşuna **F5** veya **hata ayıklama** > **hata ayıklamayı Başlat**.
   
    ![Bir uygulamanın hatalarını ayıklamaya başlamadan][startdebugging]
3. Kesme noktası ayarlama kod ve uygulama Adımlama komutları tıklayarak **hata ayıklama** menüsü.
   
   > [!NOTE]
   > Visual Studio, uygulamanızın tüm örneklerine ekler. Kod içerisinde ilerlemeye sırasında eş zamanlı oturumlarda sonuçlanan birden çok işlem tarafından kesme noktaları isabet. Bunlar, her bir kesme noktası iş parçacığı kimliğinde koşullu yaparak veya tanılama olaylarını kullanarak isabet sonra kesme noktalarını devre dışı bırakmayı deneyin.
   > 
   > 
4. **Tanılama olayları** penceresi tanılama olaylarını gerçek zamanlı görebilecek şekilde otomatik olarak açılır.
   
    ![Gerçek zamanlı tanılama olaylarını görüntüle][diagnosticevents]
5. Ayrıca açabilirsiniz **tanılama olayları** Cloud Explorer penceresinde.  Altında **Service Fabric**, herhangi bir düğüme sağ tıklayın ve seçin **akış görünümü izlemeleri**.
   
    ![Tanılama Olayları penceresinde açın][viewdiagnosticevents]
   
    Bir hizmete veya uygulamaya, izlemeleri filtrelemek istiyorsanız, belirli bir hizmet veya uygulama akış izlemeleri etkinleştirin.
6. Tanılama Olayları görülebilir içinde otomatik olarak oluşturulan **ServiceEventSource.cs** dosya ve uygulama kodundan olarak adlandırılır.
   
    ```csharp
    ServiceEventSource.Current.ServiceMessage(this, "My ServiceMessage with a parameter {0}", result.Value.ToString());
    ```
7. **Tanılama olayları** penceresi, filtreleme, duraklatma ve gerçek zamanlı olayları inceleyerek destekler.  Olay iletisi içeriği de dahil olmak üzere, bir basit dize arama filtresidir.
   
    ![Filtre, duraklatma ve sürdürme veya etkinliklerini gerçek zamanlı olarak İnceleme][diagnosticeventsactions]
8. Hata Ayıklama Hizmetleri başka bir uygulama gibidir. Visual Studio ile kesme noktaları, kolayca hata ayıklama için normal olarak ayarlarsınız. Reliable Collections birden fazla düğümde çoğaltmak olsa da, bunlar yine de IEnumerable uygulayın. Bu uygulama içinde depoladığınız görmek için hata ayıklama sırasında sonuçları görünümü Visual Studio'da kullanabileceğiniz anlamına gelir. Bunu yapmak için kodunuzdaki herhangi bir yerde bir kesme noktası ayarlayın.
   
    ![Bir uygulamanın hatalarını ayıklamaya başlamadan][breakpoint]

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="debug-a-remote-service-fabric-application"></a>Uzak bir Service Fabric uygulamasının hatalarını ayıklama
Service Fabric uygulamalarınızı azure'da bir Service Fabric kümesinde çalıştırıyorsanız, bu uygulamaların doğrudan Visual Studio'dan uzaktan hata ayıklaması yapabilirsiniz.

> [!NOTE]
> Özellik gerektirir [Service Fabric SDK 2.0](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) ve [.NET 2.9 için Azure SDK'sı](https://azure.microsoft.com/downloads/).    
> 
> 

<!-- -->
> [!WARNING]
> Uzaktan hata ayıklama, üretim ortamlarında çalışan uygulamaları üzerindeki etkisi nedeniyle kullanılamaz ve geliştirme/test senaryoları için tasarlanmıştır.
> 
> 

1. Kümenizde gidin **Cloud Explorer**. Sağ tıklatın ve seçin **Etkinleştir'hata ayıklama**
   
    ![Uzaktan hata ayıklamayı etkinleştirme][enableremotedebugging]
   
    Bu eylem kapalı küme düğümlerini ve gerekli ağ yapılandırmaları uzaktan hata ayıklama uzantısı etkinleştirilirken işlemini başlatır.
2. Küme düğümü **Cloud Explorer**ve **hata ayıklayıcı Ekle**
   
    ![Hata ayıklayıcının][attachdebugger]
3. İçinde **iliştirme** iletişim kutusunda, hata ayıklama ve istediğiniz işlemi seçin **Ekle**
   
    ![İşlem seçin][chooseprocess]
   
    İçin iliştirmek istediğiniz işlemin adını, hizmet projesi derleme adı eşittir.
   
    İşlem çalışan tüm düğümler için hata ayıklayıcının.
   
   * Durum bilgisi olmayan hizmet hata ayıklama işlemi burada durumda da, tüm düğümlerde hizmetin tüm örneklerini hata ayıklama oturumu bir parçasıdır.
   * Durum bilgisi olan hizmet hata ayıklama, herhangi bir bölümü yalnızca birincil çoğaltması active ve bu nedenle, hata ayıklayıcı tarafından yakalanan olacaktır. Birincil çoğaltma hata ayıklama oturumu sırasında taşınırsa, bu çoğaltmanın işleme hata ayıklama oturumunun parçası görünmeye devam edecektir.
   * Yalnızca ilgili bölümlerin veya belirli bir hizmetin örneklerine yakalamak için koşullu kesme noktaları yalnızca belirli bir bölüm veya örnek ayırmak için kullanabilirsiniz.
     
     ![Koşullu kesme noktası][conditionalbreakpoint]
     
     > [!NOTE]
     > Şu anda bir Service Fabric kümesi birden fazla aynı hizmet yürütülebilir adı ile hata ayıklamayı desteklemiyoruz.
     > 
     > 
4. Uygulamanızı hata ayıklama tamamladıktan sonra uzaktan hata ayıklama uzantısı kümeye sağ tıklayarak devre dışı bırakabilirsiniz **Cloud Explorer** ve **hata ayıklama devre dışı bırak**
   
    ![Uzaktan hata ayıklama devre dışı bırak][disableremotedebugging]

## <a name="streaming-traces-from-a-remote-cluster-node"></a>Akış izlemelerinden uzak küme düğümü
Siz de stream izlemeleri için doğrudan Visual Studio için uzak bir küme düğümü yapabiliyoruz. Bu özellik, bir Service Fabric küme düğümünde üretilen stream ETW İzleme olayları sağlar.

> [!NOTE]
> Bu özellik gerektirir [Service Fabric SDK 2.0](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) ve [.NET 2.9 için Azure SDK'sı](https://azure.microsoft.com/downloads/).
> Bu özellik yalnızca Azure'da çalışan kümelerini destekler.
> 
> 

<!-- -->
> [!WARNING]
> İzlemeleri akış çalışan uygulamaları üzerindeki etkisi nedeniyle üretim ortamlarında kullanılmamalıdır ve geliştirme/test senaryoları için tasarlanmıştır.
> Bir üretim senaryosunda, Azure Tanılama'yı kullanarak olayları iletme üzerinde yararlanmalıdır.
> 
> 

1. Kümenizde gidin **Cloud Explorer**. Sağ tıklatın ve seçin **akış izlemeleri etkinleştir**
   
    ![Uzak akış izlemeleri etkinleştir][enablestreamingtraces]
   
    Bu eylem, küme düğümlerinin yanı sıra gerekli ağ yapılandırmaları akış izlemeleri uzantısı etkinleştirilirken işleminin kazandırın.
2. Genişletin **düğümleri** öğesinde **Cloud Explorer**, seçin ve izlemelerinden akış istediğiniz düğüme sağ **akış görünümü izlemeleri**
   
    ![Uzak izlemeleri akış görünümü][viewremotestreamingtraces]
   
    İzlemelerinden görmek istediğiniz sayıda düğümler için 2. adımı yineleyin. Her düğüm akış, adanmış bir pencerede gösterilir.
   
    Artık Service Fabric ve hizmetlerinizin yayılan izlemeleri görebilirsiniz. Olayları yalnızca belirli bir uygulama gösterecek şekilde filtrelemek istiyorsanız, uygulama filtresine adını yazmanız yeterlidir.
   
    ![Akış izlemeleri görüntüleme][viewingstreamingtraces]
3. Akış izlemeleri kümenizden tamamladıktan sonra uzak akış izleme, kümede tıklanarak devre dışı bırakabilirsiniz **Cloud Explorer** ve **akış izlemelerini devre dışı bırak**
   
    ![Uzak akış izlemelerini devre dışı bırak][disablestreamingtraces]

## <a name="next-steps"></a>Sonraki adımlar
* [Bir Service Fabric hizmeti test](service-fabric-testability-overview.md).
* [Visual Studio'da Service Fabric uygulamalarınızı yönetmek](service-fabric-manage-application-in-visual-studio.md).

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
