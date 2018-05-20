---
title: Windows Azure mikro hata ayıklama | Microsoft Docs
description: İzleme ve bir yerel geliştirme makinede Microsoft Azure Service Fabric kullanılarak yazılmış hizmetlerinizi tanılama öğrenin.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: edcc0631-ed2d-45a3-851d-2c4fa0f4a326
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: 93cf4985e94c0af480d9f4e2e38b792cffe4df6e
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="monitor-and-diagnose-services-in-a-local-machine-development-setup"></a>İzleme ve yerel makine geliştirme Kurulum Hizmetleri'nde tanılama
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
> * [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
> 
> 

İzleme, algılama, tanılama ve sorun giderme Hizmetleri kullanıcı deneyimini en az kesintiyi devam etmek izin verir. İzleme ve tanılama gerçek dağıtılmış üretim ortamında kritik olsa da, verimliliği benzer bir model için gerçek kurulum taşıdığınızda, bunlar çalıştığından emin olmak için Hizmetleri geliştirme sırasında benimsenmesi bağlı olacaktır. Service Fabric tek makineli yerel geliştirme kurulumları ve gerçek üretim küme kurulumları arasında sorunsuz bir şekilde çalışabilirsiniz tanılama uygulamak hizmet geliştiricileri için kolaylaştırır.

## <a name="event-tracing-for-windows"></a>Windows için olay izleme
[Windows için olay izleme](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) Service Fabric izleme iletiler için önerilen teknolojisidir. ETW kullanmanın bazı avantajları şunlardır:

* **ETW hızlıdır.** Kod yürütme süreleri etkisi en az bir izleme teknoloji olarak oluşturuldu.
* **ETW İzleme, yerel geliştirme ortamları ve ayrıca gerçek küme kurulumları arasında sorunsuz çalışır.** Başka bir deyişle, gerçek bir kümeye kodunuzu dağıtmaya hazır olduğunuzda, izleme kodu yeniden yazmanız gerekmez.
* **Service Fabric sistem kodu ETW iç izleme için de kullanır.** Bu, Service Fabric sistem izlemeleri ile araya eklemeli, uygulama izlemelerini görüntülemenize olanak sağlar. Ayrıca, daha kolay sıraları ve uygulama kodu ve olaylar birbiriyle temel sistemde anlamanıza yardımcı olur.
* **Service Fabric Visual Studio Araçları ETW olayları görüntülemek üzere yerleşik desteği yoktur.** Visual Studio Service Fabric ile doğru şekilde yapılandırıldıktan sonra ETW olayları Visual Studio Tanılama Olayları görünümünde görüntülenir. 

## <a name="view-service-fabric-system-events-in-visual-studio"></a>Visual Studio'da Service Fabric sistem olayları görüntülemek
Service Fabric uygulama geliştiricileri platform neler olduğunu anlamanıza yardımcı olmak için ETW olayları gösterir. Zaten yapmadıysanız, bir tane adımları [Visual Studio'da ilk uygulamanızı oluşturma](service-fabric-create-your-first-application-in-visual-studio.md). Bu bilgiler Tanılama Olayları Görüntüleyicisi izleme iletilerini gösteren bir uygulama başlamak ve çalıştırmak yardımcı olur.

1. Olayları penceresi otomatik olarak göstermiyor, tanılama gidin, **Görünüm** sekmesinde Visual Studio'da, seçin **diğer pencereler** ve ardından **Tanılama Olayları Görüntüleyicisi**.
2. Her bir olay düğümü, uygulama ve hizmet olayı geldiği belirten standart meta veri bilgisi yok. Kullanarak olayların listesini filtreleyebilirsiniz **filtre olayları** olayları pencerenin üstündeki kutusu. Örneğin, filtreleyebilirsiniz **düğüm adı** veya **hizmet adı.** Ve olay ayrıntılarını bakarken, ayrıca kullanarak duraklatabilirsiniz **duraklatma** olayları penceresinin en üstünde düğmesine tıklayın ve daha sonra olayları herhangi bir kayıp devam.
   
   ![Visual Studio Tanılama Olayları Görüntüleyicisi](./media/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally/DiagEventsExamples2.png)

## <a name="add-your-own-custom-traces-to-the-application-code"></a>Uygulama kodu kendi özel izlemeleri ekleyin
Service Fabric Visual Studio Proje şablonları örnek kod içerir. Kod hizmet yapıdan sistem izlemeleri yanı sıra Visual Studio ETW görüntüleyicide görünmesini özel uygulama kodu ETW izleri ekleme gösterir. Bu yöntemin avantajı meta verileri izlemeleri için otomatik olarak eklenir ve Visual Studio Tanılama Olayları Görüntüleyicisi bunları görüntülemek için zaten yapılandırıldı ' dir.

Oluşturulan projeleri için **hizmet şablonları** (durum bilgisiz veya durum bilgisi olan) yalnızca arama `RunAsync` uygulama:

1. Çağrı `ServiceEventSource.Current.ServiceMessage` içinde `RunAsync` yöntemi bir özel ETW İzleme örneği uygulama kodundan gösterir.
2. İçinde **ServiceEventSource.cs** dosyası için bir aşırı bulacaksınız `ServiceEventSource.ServiceMessage` performans nedenlerden dolayı yüksek sıklıkta olayları için kullanılması gereken yöntemi.

Oluşturulan projeleri için **aktör şablonları** (durum bilgisiz veya durum bilgisi olan):

1. Açık **"ProjectName".cs** dosya nerede *ProjectName* Visual Studio projeniz için seçtiğiniz adıdır.  
2. Kod Bul `ActorEventSource.Current.ActorMessage(this, "Doing Work");` içinde *DoWorkAsync* yöntemi.  Bu, uygulama kodundan yazılmış özel bir ETW İzleme örneğidir.  
3. Dosyasındaki **ActorEventSource.cs**, aşırı için bulacaksınız `ActorEventSource.ActorMessage` performans nedenlerden dolayı yüksek sıklıkta olayları için kullanılması gereken yöntemi.

Özel ETW İzleme hizmeti kodunuzu eklendikten sonra derleme, dağıtma ve yeniden Tanılama Olayları Görüntüleyicisi'nde, olay görmek için uygulamayı çalıştırın. Uygulama ile ayıklaması **F5**, tanılama olayları Görüntüleyicisi'ni otomatik olarak açılır.

## <a name="next-steps"></a>Sonraki adımlar
Yerel tanılama için, uygulamanıza eklediğiniz aynı izleme kodu, uygulamanızı Azure bir kümede çalışırken bu olayları görüntülemek için kullanabileceğiniz araçları ile çalışır. Araçları için farklı seçenekleri ele ve nasıl bunları ayarlayabilirsiniz açıklayan bu makalelere göz atın.

* [Azure Tanılama ile günlükleri toplamak nasıl](service-fabric-diagnostics-how-to-setup-wad.md)
* [Olay toplama ve EventFlow kullanarak koleksiyonu](service-fabric-diagnostics-event-aggregation-eventflow.md)

