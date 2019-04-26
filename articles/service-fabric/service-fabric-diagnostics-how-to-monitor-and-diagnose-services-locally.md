---
title: Hata ayıklama Azure Service Fabric Windows uygulamalarında | Microsoft Docs
description: Microsoft Azure Service Fabric yerel geliştirme makinenizde kullanılarak yazılmış, hizmetleri izleme ve Tanılama hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: chackdan
editor: ''
ms.assetid: edcc0631-ed2d-45a3-851d-2c4fa0f4a326
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/25/2019
ms.author: srrengar
ms.openlocfilehash: 31c559c1ab314b7e1f29bd96f74d6d82cfcc0420
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60392844"
---
# <a name="monitor-and-diagnose-services-in-a-local-machine-development-setup"></a>İçinde bir yerel makine dağıtım kurulumunda Hizmetleri izleme ve tanılama
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
> * [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
> 
> 

İzleme, algılama, tanılama ve sorun giderme kullanıcı deneyimi için çok az kesinti devam etmek hizmetler sağlar. İzleme ve tanılama bir gerçek dağıtılan üretim ortamı için kritik olsa da, benzer bir model, gerçek kurulum taşıdığınızda, bunlar çalıştığından emin olmak amacıyla hizmetlerin geliştirme sırasında benimseme verimliliğini bağlıdır. Service Fabric, tek makinede yerel geliştirme ayarları ve gerçek üretim kümesi ayarlar arasında sorunsuz bir şekilde çalışabilir tanılama uygulamak hizmet geliştiricileri kolaylaştırır.

## <a name="event-tracing-for-windows"></a>Windows için olay izleme
[İçin olay izleme Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW), Service fabric'te izleme iletileri için önerilen teknolojisidir. ETW kullanmanın bazı avantajları şunlardır:

* **ETW hızlıdır.** Kod yürütme süreleri çok az etkisi olan izleme teknolojisi olarak oluşturulmuştur.
* **ETW İzleme, yerel geliştirme ortamları ve ayrıca gerçek küme ayarları arasında sorunsuz çalışır.** Başka bir deyişle, gerçek bir küme için kodunuzu dağıtmaya hazır olduğunuzda, izleme kodu yeniden yazmanız gerekmez.
* **Service Fabric sistem kodu ETW iç izleme için de kullanır.** Bu uygulama izlemelerinizi kullanarak Service Fabric sistem izlemeleri aralıklı görüntülemenize olanak sağlar. Bu ayrıca daha kolay dizileri ve uygulama kodu ve olaylar birbiriyle alttaki sistemde anlamanıza yardımcı olur.
* **ETW olaylarını görüntülemek için Service Fabric Visual Studio Araçları'nda yerleşik desteği mevcuttur.** Visual Studio Service Fabric ile doğru şekilde yapılandırıldıktan sonra ETW olayları Visual Studio Tanılama Olayları görünümünde görünür. 

## <a name="view-service-fabric-system-events-in-visual-studio"></a>Visual Studio'da Service Fabric sistem olaylarını görüntüleyin
Service Fabric uygulama geliştiricilerin platform neler olduğunu anlamanıza yardımcı olmak için ETW olayları gösterir. Zaten yapmadıysanız, devam edin ve adımları izleyerek [Visual Studio'da ilk uygulamanızı oluşturma](service-fabric-tutorial-create-dotnet-app.md). Bu bilgiler, uygulama Tanılama Olayları Görüntüleyicisi izleme iletilerini gösteren ayarlayıp çalıştırmaya başlamasına yardımcı olur.

1. Tanılama Olayları penceresinde otomatik olarak göstermiyor, Git, **görünümü** sekmesini Visual Studio'da, **diğer Windows** ardından **Tanılama Olayları Görüntüleyicisi**.
2. Her olay, düğüm, uygulama ve hizmet olayı geldiğini belirten standart meta veri bilgilerini içerir. Kullanarak ayrıca olayların listesini filtreleyebilirsiniz **filtre olayları** olayları penceresinin üst kısmındaki kutusu. Örneğin, filtreleyebilirsiniz **düğüm adı** veya **hizmet adı.** Ve olay ayrıntılarını da bakarken, ayrıca kullanarak duraklatabilirsiniz **duraklatmak** olayları penceresinin en üstünde düğmesini tıklatın ve daha sonra olayları herhangi bir kayıp devam.
   
   ![Visual Studio Tanılama Olayları Görüntüleyicisi](./media/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally/DiagEventsExamples2.png)

## <a name="add-your-own-custom-traces-to-the-application-code"></a>Uygulama kodu kendi özel izleme ekleyin
Service Fabric Visual Studio Proje şablonları, örnek kod içerir. Kod, Service Fabric sistem izlemeleri yanı sıra Visual Studio ETW görüntüleyicide görünmesini özel uygulama kodu ETW izlemelerini ekleme işlemi gösterilmektedir. Bu yöntemin avantajı, meta verileri izlemeleri için otomatik olarak eklenir ve bunları görüntülemek için Visual Studio Tanılama Olayları Görüntüleyicisi zaten yapılandırılmış olmanızdır.

Oluşturulan projeleri için **hizmet şablonları** yalnızca arayın (durum bilgisi olan veya) `RunAsync` uygulama:

1. Çağrı `ServiceEventSource.Current.ServiceMessage` içinde `RunAsync` yöntem uygulama kodundan özel bir ETW İzleme örneği gösterilmektedir.
2. İçinde **ServiceEventSource.cs** dosyası için bir aşırı yükleme bulacaksınız `ServiceEventSource.ServiceMessage` performansla ilgili nedenlerden dolayı yüksek sıklık düzeyindeki olayları için kullanılması gereken yöntemini.

Oluşturulan projeleri için **aktör şablonları** (durum bilgisi olan veya):

1. Açık **"ProjectName".cs** dosya nerede *ProjectName* Visual Studio projeniz için seçtiğiniz adı.  
2. Kodun bulunacağı `ActorEventSource.Current.ActorMessage(this, "Doing Work");` içinde *DoWorkAsync* yöntemi.  Bu, uygulama kodundan yazılan özel bir ETW İzleme örneğidir.  
3. Dosyasındaki **ActorEventSource.cs**, aşırı için bulabilirsiniz `ActorEventSource.ActorMessage` performansla ilgili nedenlerden dolayı yüksek sıklık düzeyindeki olayları için kullanılması gereken yöntemini.

Özel ETW İzleme hizmeti kodunuza ekledikten sonra derleme, dağıtma ve tanılama olayları Görüntüleyicisi'nde, olaylar yeniden görmek için uygulamayı çalıştırın. Uygulama hatalarını ayıklıyorsanız **F5**, tanılama olayları Görüntüleyicisi'ni otomatik olarak açılır.

## <a name="next-steps"></a>Sonraki adımlar
Yerel tanılama için, uygulamanıza eklediğiniz aynı izleme kodu, uygulamanızı bir Azure kümesinde çalıştırırken bu olayları görüntülemek için kullanabileceğiniz araçları ile çalışır. Araçlar için farklı seçenekler hakkında konuşmak ve nasıl bunları ayarlayabilirsiniz açıklayan şu makalelere göz atın.

* [Azure Tanılama ile günlük toplama](service-fabric-diagnostics-how-to-setup-wad.md)
* [Olay toplama ve koleksiyon EventFlow kullanma](service-fabric-diagnostics-event-aggregation-eventflow.md)

