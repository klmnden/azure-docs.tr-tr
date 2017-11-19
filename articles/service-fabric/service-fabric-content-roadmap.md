---
title: "Azure Service Fabric hakkında daha fazla bilgi | Microsoft Docs"
description: "Azure Service Fabric ana alanları ve temel kavramlar hakkında bilgi edinin. Service Fabric ve mikro hizmetler oluşturma Genişletilmiş genel bakış sağlar."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/30/2017
ms.author: ryanwi
ms.openlocfilehash: 52cd6de5b6caa215ff1726d3099cb7c49576774f
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
# <a name="so-you-want-to-learn-about-service-fabric"></a>Bu nedenle Service Fabric hakkında bilgi edinmek istiyorsunuz?
Azure Service Fabric; ölçeklenebilir ve güvenilir mikro hizmetleri paketlemeyi, dağıtmayı ve yönetmeyi kolaylaştırmayı sağlayan bir dağıtılmış sistemler platformudur.  Ancak, büyük bir yüzey alanı Service Fabric sahiptir ve öğrenmek için çok yok.  Bu makale, Service Fabric özeti sağlar ve modeller, uygulama yaşam döngüsü, test, kümeler ve sistem durumu izleme programlama temel kavramlarını açıklar. Okuma [genel bakış](service-fabric-overview.md) ve [mikro nelerdir?](service-fabric-overview-microservices.md) giriş ve Service Fabric mikro oluşturmak için nasıl kullanılabilir. Bu makalede kapsamlı bir içerik listesi içermiyor, ancak genel bakış ve her Service Fabric alanı için Başlarken makaleleri bağlayın. 

## <a name="core-concepts"></a>Temel kavramlar
[Service Fabric terminolojisi](service-fabric-technical-overview.md), [uygulama modeli](service-fabric-application-model.md), ve [desteklenen programlama modelleri](service-fabric-choose-framework.md) daha fazla kavramları ve açıklamalar sağlar, ancak temel kavramları şunlardır.

<table><tr><th>Temel kavramlar</th><th>Tasarım zamanı</th><th>Çalışma zamanı</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965">
<img src="./media/service-fabric-content-roadmap/CoreConceptsVid.png" WIDTH="240" HEIGHT="162"></a></td>
<td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tlkI046yC_2906218965"><img src="./media/service-fabric-content-roadmap/RunTimeVid.png" WIDTH="240" HEIGHT="162"></a></td>
<td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=x7CVH56yC_1406218965">
<img src="./media/service-fabric-content-roadmap/RunTimeVid.png" WIDTH="240" HEIGHT="162"></a></td></tr>
</table>

### <a name="design-time-application-type-service-type-application-package-and-manifest-service-package-and-manifest"></a>Tasarım zamanı: uygulama türü, hizmet türü, uygulama paketi ve bildirim, hizmet paketi ve bildirimi
Hizmet türlerinin bir koleksiyona atanan ad/sürüm bir uygulama türüdür. Bu tanımlanan bir *ApplicationManifest.xml* bir uygulama paketi dizininde ekli dosya. Uygulama paketi sonra Service Fabric kümenin görüntü deposuna kopyalanır. Daha sonra küme içinde çalışan uygulama türünden adlandırılmış bir uygulama oluşturabilirsiniz. 

Bir hizmetin kod paketler, veri paketleri ve yapılandırma paketleri atanan ad/sürüm hizmeti türüdür. Bu, bir hizmet paketi dizinde katıştırılmış bir ServiceManifest.xml dosyasında tanımlanır. Hizmet paketi dizini sonra bir uygulama paketin tarafından başvurulan *ApplicationManifest.xml* dosya. Küme içinde adlandırılmış bir uygulama oluşturduktan sonra adlandırılmış bir hizmet uygulaması tür hizmet türlerinden birini oluşturabilirsiniz. Bir hizmet türü tarafından açıklanan kendi *ServiceManifest.xml* dosya. Hizmet türü çalışma zamanında yüklenir, yürütülebilir kod hizmeti yapılandırma ayarlarını ve hizmet tarafından tüketilen statik veri kümesinden oluşur.

![Service Fabric uygulama türleri ve hizmet türleri][cluster-imagestore-apptypes]

Uygulama paketi uygulama tür içeren bir disk dizindir *ApplicationManifest.xml* için uygulama türü yapar her hizmet türünün hizmet paketleri başvuruda bulunan dosya. Örneğin, e-posta uygulama türü için bir uygulama paketi sırası hizmet paketi, ön uç hizmet paketi ve veritabanı hizmet paketi başvurular içerebilir. Uygulama paketi dizindeki dosyaların Service Fabric kümenin görüntü deposuna kopyalanır. 

Bir hizmet paketi hizmet türünün içeren bir disk dizindir *ServiceManifest.xml* kodu, statik verileri ve yapılandırma paketleri hizmet türünün başvuran dosya. Hizmet paketi dizindeki dosyaların uygulama tür tarafından başvurulan *ApplicationManifest.xml* dosya. Örneğin, bir hizmet paketi, kod, statik verileri ve veritabanı hizmeti oluşturan yapılandırma paketleri başvurabileceğiniz.

### <a name="run-time-clusters-and-nodes-named-applications-named-services-partitions-and-replicas"></a>Çalışma zamanı: kümeleri ve uygulamalar, hizmetler, bölümler ve çoğaltmalar adlı adlı düğümler
[Service Fabric kümesi](service-fabric-deploy-anywhere.md), mikro hizmetlerin dağıtılıp yönetildiği, ağa bağlı bir sanal veya fiziksel makine kümesidir. Kümeleri makineler binlerce ölçeklendirebilirsiniz.

Bir makine ya da bir kümenin parçasıysa VM düğüm denir. Her düğüme bir düğüm adı (dize) atanır. Düğümleri yerleşim özellikleri gibi özelliklere sahiptir. Her makine ya da VM bir otomatik başlatılan Windows hizmet sahip `FabricHost.exe`, önyükleme sırasında çalışmaya başlar ve ardından iki yürütülebilir dosyalar başlatır: `Fabric.exe` ve `FabricGateway.exe`. Bu iki yürütülebilir dosyalar düğümü olun. Geliştirme veya test senaryoları için birden çok örneğini çalıştırarak birden çok düğümde tek makine ya da VM barındırabilir `Fabric.exe` ve `FabricGateway.exe`.

Belirli bir işlevi veya işlevleri gerçekleştiren bir hizmetler koleksiyonunu içeren adlandırılmış bir adlandırılmış uygulamasıdır. Bir hizmet (bunu başlatmak ve diğer hizmetler bağımsız olarak çalışır) tam ve tek başına bir işlev gerçekleştirir ve kod, yapılandırma ve verileri oluşur. Bir uygulama paketi görüntü deposuna kopyalandıktan sonra uygulama paketin uygulama türü (kendi ad/sürüm kullanarak) belirterek küme içindeki uygulama örneği oluşturun. Her uygulama türü örneğe benzer bir URI ad atanmış *fabric: / MyNamedApp*. Bir küme içinde tek bir uygulama türünden birden çok adlandırılmış uygulamaları oluşturabilirsiniz. Ayrıca, farklı uygulama türlerinden adlandırılmış uygulamaları oluşturabilirsiniz. Her adlandırılmış bağımsız olarak yönetilen ve sürümü tutulan uygulamasıdır.

Adlandırılmış bir uygulama oluşturduktan sonra hizmet türü (kendi ad/sürüm kullanarak) belirterek küme içinde kendi hizmet türlerinden (adlandırılmış bir hizmeti) örneği oluşturabilirsiniz. Her hizmet türü örneği, adlandırılmış uygulamanın URI altında kapsamlı bir URI ad atanır. "Hizmet"uygulama adlı bir MyNamedApp"içinde adlı bir Veritabanım" oluşturursanız, örneğin, URI gibi görünüyor: *fabric: / MyNamedApp/Veritabanım*. Adlandırılmış bir uygulama içinde bir veya daha fazla adlandırılmış hizmetler oluşturabilirsiniz. Adlandırılmış her hizmetin kendi bölüm düzeni olabilir ve örnek/çoğaltma sayar. 

İki tür hizmet vardır: durum bilgisiz ve durum bilgisi. Durum bilgisi olmayan hizmetler kalıcı durum bir dış depolama hizmeti Azure Storage, Azure SQL Database veya Azure Cosmos DB gibi depolayabilirsiniz. Hizmet kalıcı depolama hiç varsa, durum bilgisi olmayan bir hizmet kullanın. Bir durum bilgisi olan hizmet Service Fabric Hizmetinizin durumunu kendi güvenilir koleksiyonları veya Reliable Actors programlama modeli aracılığıyla yönetmek için kullanır. 

Adlandırılmış hizmet oluşturulurken bir bölüm düzeni belirtin. Hizmetleri durumu büyük miktarda veri bölümler bölün. Her bölüm, bir kümenin düğümlere yayılır hizmetinin tam durumunu kısmı sorumludur.  

Aşağıdaki diyagram, uygulamaları ve hizmet örnekleri, bölümler ve çoğaltmalar arasındaki ilişkiyi gösterir.

![Bölümler ve çoğaltmalar bir hizmet kapsamındaki][cluster-application-instances]

### <a name="partitioning-scaling-and-availability"></a>Bölümlendirme, ölçeklendirme ve kullanılabilirlik
[Bölümleme](service-fabric-concepts-partitioning.md) Service Fabric benzersiz değil. Veri bölümlendirme veya parçalama bölümleme iyi bilinen biçimi değil. Durum bilgisi olan hizmetler durumu büyük miktarda veri bölümler bölün. Her bölüm hizmetinin tam durumunu bir kısmı için sorumludur. 

Her bölüm kopyalarını adlandırılmış hizmetinizin durumuna sağlayan küme düğümleri arasında yayılır [ölçek](service-fabric-concepts-scalability.md). Veri büyüme gereksinimlerine göre bölümleri büyümesine ve Service Fabric bölümleri donanım kaynaklarını verimli kullanılmasını sağlamak için düğümleri arasında yeniden dengeler. Kümeye yeni düğümler eklerseniz, Service Fabric sayısının artması düğümler arasında çoğaltmalarını yeniden dengelemeniz. Genel uygulama performansını artıran ve bellek erişimi için Çekişme azaltır. Kümedeki düğümler verimli bir şekilde kullanılmayan, kümedeki düğüm sayısını azaltabilirsiniz. Service Fabric yeniden çoğaltmalarını azalmasına her bir düğümüne donanım daha iyi kullanılmasını sağlamak için düğüm sayısı arasında yeniden dengeler.

Durum bilgisi olan adlandırılmış Hizmetleri çoğaltmaları olsa da bir bölüm içinde durum bilgisiz adlandırılmış Hizmetleri örneğe sahip. Genellikle, hiçbir iç durumu sahip olduğu durum bilgisiz adlandırılmış hizmetleri her zaman sadece bir bölümü olmalıdır. Bölüm örnekleri sağlamak [kullanılabilirlik](service-fabric-availability-services.md). Bir örnek başarısız olursa, diğer örnekleri normal şekilde çalışmaya devam eder ve daha sonra Service Fabric yeni bir örneğini oluşturur. Durum bilgisi olan hizmetler adlı korumak durumlarına çoğaltmaları ve her bir bölümü içinde kendi çoğaltma kümesi vardır. Okuma ve yazma işlemleri (birincil olarak adlandırılır) bir yinelemede gerçekleştirilir. İşlemler (etkin ikincil kopya olarak adlandırılır) birden çok diğer çoğaltmalar için çoğaltılır gelen durum değişiklikleri yazma. Bir çoğaltma işlemi başarısız olursa, Service Fabric mevcut çoğaltmaların yeni bir kopya oluşturur.

## <a name="stateless-and-stateful-microservices-for-service-fabric"></a>Durum bilgisiz ve durum bilgisi olan mikro Service Fabric için
Service Fabric mikro veya kapsayıcıları oluşması uygulamalar oluşturmanıza olanak sağlar. Durum bilgisiz mikro (örneğin, protokol ağ geçitleri ve web proxy) dışında bir istek ve yanıt hizmetinden değişebilir bir duruma bulundurmayan. Azure bulut Hizmetleri çalışan rolleri, bir durum bilgisi olmayan hizmetin bir örnektir. Durum bilgisi olan mikro (örneğin, kullanıcı hesapları, veritabanları, aygıtlar, Alışveriş sepetleri ve Kuyruklar) istek ve yanıt ötesinde değişebilir, yetkili bir durumu korurlar. Bugünün Internet ölçekli uygulamalar durum bilgisiz ve durum bilgisi olan mikro birleşiminden oluşur. 

Service Fabric ile anahtar differentation ile ya da durum bilgisi olan hizmetler oluşturma, güçlü odaklanmasına olan [modellerini programlama yerleşik ](service-fabric-choose-framework.md) veya kapsayıcılı durum bilgisi olan hizmetler ile. [Uygulama senaryoları](service-fabric-application-scenarios.md) durum bilgisi olan hizmetler kullanıldığı senaryolar açıklanmaktadır.

Durum bilgisiz olanları birlikte durum bilgisi olan mikro neden var mı? İki ana nedeni şunlardır:

* Yüksek verimlilik, düşük gecikme süreli, hataya dayanıklı çevrimiçi işlem işleme (OLTP) Hizmetleri aynı makinede kod ve veri yakın tutarak oluşturabilirsiniz. Bazı, etkileşimli veriş, arama, nesnelerin interneti (IOT) sistemleri, ticaret sistemleri, kredi kartı işleme ve sahtekarlık algılama sistemleri ve kişisel kayıt yönetimi örnektir.
* Uygulama tasarımı basitleştirebilirsiniz. Durum bilgisi olan mikro ek kuyruklar ve tamamen durum bilgisiz uygulamanızın kullanılabilirlik ve gecikme süresi gereksinimlerine geleneksel gerekli önbellekleri gereksinimini kaldırın. Durum bilgisi olan doğal olarak yüksek kullanılabilirlik ve düşük uygulamanızı bir bütün olarak yönetmek için taşıma bölümlerinin sayısını azaltır Gecikmeli, hizmetleridir.

## <a name="supported-programming-models"></a>Desteklenen programlama modelleri
Service Fabric yazma ve hizmetlerinizi yönetmek için birden çok yol sunar. Hizmetleri platformun özellikleri ve uygulama çerçeveleri tam anlamıyla yararlanabilmek için Service Fabric API'ları kullanabilirsiniz. Hizmetleri de herhangi dilinde yazılmıştır ve Service Fabric kümesi üzerinde barındırılan herhangi derlenmiş bir yürütülebilir program olabilir. Daha fazla bilgi için bkz: [desteklenen programlama modelleri](service-fabric-choose-framework.md).

### <a name="containers"></a>Kapsayıcılar
Varsayılan olarak, Service Fabric dağıtır ve Hizmetleri işlemler olarak etkinleştirir. Service Fabric da Hizmetleri'nde dağıtım [kapsayıcıları](service-fabric-containers-overview.md). Önemlisi işlemlerde Hizmetleri ve Hizmetleri aynı uygulamada kapsayıcılardaki karıştırabilirsiniz. Service Fabric Windows Server 2016 Linux kapsayıcıları Windows kapsayıcıları dağıtımını destekler. Var olan uygulamalar, durum bilgisi olmayan hizmetler veya durum bilgisi olan hizmetleri kapsayıcılarında dağıtabilirsiniz. 

### <a name="reliable-services"></a>Reliable Services
[Güvenilir hizmetler](service-fabric-reliable-services-introduction.md) Service Fabric platformu ile tümleştirme ve platform özelliklerinin tam kümesinden yararlanabilirsiniz Hizmetleri yazmak için basit bir çerçevedir. Güvenilir hizmetler durum bilgisiz olabilir (örneğin web sunucuları veya çalışan rolleri Azure Cloud Services çoğu hizmet platformlar için benzer), burada durumu kalıcıdır Azure DB veya Azure Table Storage gibi harici bir çözüm içinde. Güvenilir hizmetler, durum hizmeti kendisini güvenilir koleksiyonları kullanarak doğrudan burada kalıcı durum bilgisi olan, da olabilir. Durum yapıldığında [yüksek oranda kullanılabilir](service-fabric-availability-services.md) çoğaltmayla ve aracılığıyla dağıtılmış [bölümleme](service-fabric-concepts-partitioning.md), tüm yönetilen Service Fabric tarafından otomatik olarak.

### <a name="reliable-actors"></a>Reliable Actors
Güvenilir hizmetler üstünde oluşturulmuş [güvenilir aktör](service-fabric-reliable-actors-introduction.md) çerçevedir aktör tasarım deseni temel alınarak sanal aktör deseni uygulayan bir uygulama çerçevesi. Güvenilir aktör çerçevesi yürütme aktörler olarak adlandırılan tek iş parçacıklı işlem ve durumunun bağımsız bir birim kullanır. Güvenilir bir çerçeve sağlar aktör iletişimi aktörler için yerleşik ve durumu kalıcılığını ve genişleme yapılandırmaları önceden ayarlanmış.

### <a name="aspnet-core"></a>ASP.NET Core
Service Fabric tümleşir [ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md) web ve API uygulamaları oluşturmak için birinci sınıf bir programlama modeli olarak.  ASP.NET Core Service Fabric iki farklı şekillerde kullanılabilir:

- Bir konuk yürütülebilir dosya barındırılan. Bu, öncelikle hiçbir kod değişikliklerle Service Fabric mevcut ASP.NET Core uygulamaları çalıştırmak için kullanılır.
- İçinde güvenilir bir hizmet farklı çalıştır. Bu, Service Fabric çalışma zamanı ile daha iyi tümleştirme sağlar ve durum bilgisi olan ASP.NET Core hizmetleri sağlar.

### <a name="guest-executables"></a>Konuk yürütülebilir dosyalar
A [Konuk yürütülebilir](service-fabric-deploy-existing-app.md) olduğu diğer hizmetlerin yanı sıra bir Service Fabric kümesi üzerinde barındırılan (herhangi bir dilde yazılmış) var olan, rasgele çalıştırılabilir. Konuk yürütülebilir dosyaları doğrudan hizmet doku API'leri ile tümleştirmeye değil. Ancak bunlar hala gibi özel durumu platform sunar özelliklerinden yararlanır ve raporlama yük ve REST API'ları çağırarak bulunabilirliği hizmet. Aynı zamanda tam uygulama yaşam döngüsü destek sahiptirler. 

## <a name="application-lifecycle"></a>Uygulama yaşam döngüsü
Diğer platformlar ile bir uygulama Service Fabric genellikle aşağıdaki aşamaları geçtikçe: tasarım, geliştirme, test, dağıtım, yükseltme, Bakım ve kaldırma. Service Fabric, nihai yetkisini için bulut uygulamalarından, geliştirme aşamasından dağıtım, günlük yönetim ve Bakım tam uygulama yaşam döngüsü için birinci sınıf destek sağlar. Hizmet modeli bağımsız olarak uygulama yaşam döngüsü katılmak birçok farklı rol sağlar. [Service Fabric uygulama yaşam döngüsü](service-fabric-application-lifecycle.md) API'ler ve Service Fabric uygulama yaşam döngüsü aşamaları boyunca farklı rolleri tarafından nasıl kullanıldıkları hakkında genel bir bakış sağlar. 

Tüm uygulama yaşam döngüsü kullanılarak yönetilebilir [PowerShell cmdlet'leri](/powershell/module/ServiceFabric/), [C# API'leri](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient), [Java API](/java/api/system.fabric._application_management_client), ve [REST API'leri](/rest/api/servicefabric/). Gibi araçları kullanılarak sürekli tümleştirme/sürekli dağıtım ardışık düzen de ayarlayabilirsiniz [Visual Studio Team Services](service-fabric-tutorial-deploy-app-with-cicd-vsts.md) veya [Jenkins](service-fabric-cicd-your-linux-java-application-with-jenkins.md).

Aşağıdaki Microsoft Virtual Academy video, uygulama yaşam döngüsü açıklar:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=My3Ka56yC_6106218965">
<img src="./media/service-fabric-content-roadmap/AppLifecycleVid.png" WIDTH="360" HEIGHT="244">
</a></center>

## <a name="test-applications-and-services"></a>Uygulama ve hizmetleri test etme
Gerçekten bulut ölçekli hizmetler oluşturmak için uygulama ve hizmetlerinize gerçek hataları dayanabilir doğrulamak önemlidir. Hata analizi hizmeti Service Fabric yerleşik Hizmetleri test etmek için tasarlanmıştır. İle [hata analizi hizmeti](service-fabric-testability-overview.md), anlamlı hataları anlamına ve tam test senaryoları uygulamalarınızı karşı çalıştırmak. Bu hataları ve senaryoları çalışma ve çok sayıda durumları ve yaşam süresi, tüm denetimli, güvenli ve tutarlı bir şekilde boyunca bir hizmet yaşar geçişleri doğrulayın.

[Eylemler](service-fabric-testability-actions.md) tek tek hataları kullanarak test etmek için bir hizmet hedefi. Bir hizmet Geliştirici yapı taşı olarak karmaşık senaryolarda yazmak için kullanabilirsiniz. Benzetimli hataları örnekleri şunlardır:

* Burada bir makine ya da VM yeniden durumlarda herhangi bir sayıda benzetimini yapmak için bir düğümü yeniden başlatın.
* Yük Dengeleme, yük devretme veya uygulama yükseltme benzetimini yapmak için durum bilgisi olan hizmet çoğaltmasını taşıyın.
* Çekirdek kayıp yeni verileri kabul etmek için yeterli "yedek" veya "ikincil" çoğaltmaları olmadığından burada yazma işlemleri devam edemiyor bir durum oluşturmak için durum bilgisi olan bir hizmet olarak çağırır.
* Burada tüm bellek içi durumu tamamen silinmeden çıkışı bir durum oluşturmak için bir durum bilgisi olan hizmet veri kaybı çağırır.

[Senaryolar](service-fabric-testability-scenarios.md) karmaşık işlemleri bir veya daha fazla Eylemler oluşur. Hata analizi hizmeti iki yerleşik tam senaryoları sağlar:

* [Chaos senaryo](service-fabric-controlled-chaos.md)-küme boyunca sürekli, araya eklemeli hataları (normal ve durunda) uzun süreler boyunca benzetimini yapar.
* [Yük devretme senaryosu](service-fabric-testability-scenarios.md#failover-test)-diğer hizmetler etkilenmemesini bırakarak belirli hizmet bölüm hedefler chaos test senaryosu sürümü.

## <a name="clusters"></a>Kümeleri
[Service Fabric kümesi](service-fabric-deploy-anywhere.md), mikro hizmetlerin dağıtılıp yönetildiği, ağa bağlı bir sanal veya fiziksel makine kümesidir. Kümeleri makineler binlerce ölçeklendirebilirsiniz. Bir küme düğümü bir makine ya da bir kümenin parçasıysa VM adı verilir. Her düğüme bir düğüm adı (dize) atanır. Düğümleri yerleşim özellikleri gibi özelliklere sahiptir. Her makine ya da VM otomatik başlatılan hizmet sahip `FabricHost.exe`, önyükleme sırasında çalışmaya başlar ve ardından iki yürütülebilir dosyalar başlatır: Fabric.exe ve FabricGateway.exe. Bu iki yürütülebilir dosyalar düğümü olun. Senaryoları test etmek için bir tek makine ya da VM birden çok düğümde birden çok örneğini çalıştırarak barındırabilir `Fabric.exe` ve `FabricGateway.exe`.

Service Fabric kümeleri, Windows Server veya Linux çalıştıran sanal veya fiziksel makineler üzerinde oluşturulabilir. Dağıtıp Service Fabric uygulamaları birbirine bağlı bir Windows Server veya Linux bilgisayarlar kümesi sahip olduğu herhangi bir ortamda çalıştırılabilir: şirket içi, Microsoft Azure veya herhangi bir bulut sağlayıcısı.

Aşağıdaki Microsoft Virtual Academy video Service Fabric kümeleri açıklanmaktadır:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965">
<img src="./media/service-fabric-content-roadmap/ClusterOverview.png" WIDTH="360" HEIGHT="244">
</a></center>

### <a name="clusters-on-azure"></a>Azure’da kümeler
Service Fabric kümeleri Azure üzerinde çalışan işlemleri ve küme yönetiminin daha kolay ve daha güvenilir yapar diğer Azure özellikleri ve Hizmetleri ile tümleştirme sağlar. Azure'daki diğer kaynakları gibi kümeleri model için bir Azure Resource Manager kaynak kümedir. Kaynak Yöneticisi'ni de tek bir birim olarak küme tarafından kullanılan tüm kaynakları kolay yönetim sağlar. Azure kümelerinde Azure tanılama ve günlük analizi ile tümleşiktir. Küme düğümü türleri [sanal makine ölçek kümeleri](/azure/virtual-machine-scale-sets/index), otomatik ölçeklendirmeyi işlevsellik oluşturulmuştur.

Azure üzerinde bir küme oluşturmak [Azure portal](service-fabric-cluster-creation-via-portal.md), gelen bir [şablonu](service-fabric-cluster-creation-via-arm.md), veya [Visual Studio](service-fabric-cluster-creation-via-visual-studio.md).

Service Fabric Linux üzerinde derleme, dağıtma ve Windows'da gibi Linux üzerinde yüksek oranda kullanılabilir, ölçeklendirilebilir uygulamalar yönetmenize olanak sağlar. Service Fabric çerçeveleri (Reliable Services ve Reliable Actors) yanı sıra C# (.NET Core) Linux üzerinde Java mevcuttur. Ayrıca, oluşturabilirsiniz [Konuk yürütülebilir Hizmetleri](service-fabric-deploy-existing-app.md) herhangi bir dil veya framework ile. Docker kapsayıcıları yönetme de desteklenir. Docker kapsayıcıları Konuk yürütülebilir dosyalar veya Service Fabric çerçeveleri kullanan yerel Service Fabric hizmetler çalıştırabilirsiniz. Hakkında daha fazla bilgi için okuma [Service Fabric Linux'ta](service-fabric-deploy-anywhere.md).

Windows, ancak Linux desteklenen bazı özellikler vardır. Daha fazla bilgi edinmek için okuma [Service Fabric Linux ve Windows arasındaki farklar](service-fabric-linux-windows-differences.md).

### <a name="standalone-clusters"></a>Tek başına kümeler
Service Fabric, tek başına Service Fabric kümeleri şirket içi oluşturmak için veya herhangi bir bulut sağlayıcısına üzerinde bir yükleme paketi sağlar. Tek başına kümelerinin istediğiniz yere bir küme ana bilgisayar özgürlüğü sağlar. Verilerinizi uyumluluk ve Mevzuat kısıtlamaları maruz kalır veya verilerinizi yerel tutmak istediğiniz, kendi küme ve uygulamalarını barındırabilir. Uygulamaları oluşturma bilginiz bir barındırma ortamdan diğerine taşır için Service Fabric uygulamaları herhangi bir değişiklik ile birden çok barındırma ortamlarında çalıştırabilirsiniz. 

[İlk Service Fabric tek başına kümenizi oluşturun](service-fabric-get-started-standalone-cluster.md)

Linux tek başına kümelerinin henüz desteklenmiyor.

### <a name="cluster-security"></a>Küme güvenliği
Kümeler, yetkisiz kullanıcıların özellikle üretim iş yükleri üzerinde çalıştırılan sahip olduğunda, kümeniz için bağlanmasını önlemek için güvenli hale getirilmelidir. Güvenli olmayan bir küme oluşturmak mümkün olsa da, bunun nedenle yönetim uç noktalarının genel internet'e gösteriliyorsa bağlanmak anonim kullanıcıların sağlar. Daha sonra güvenlik güvenli olmayan bir kümede etkinleştirmek mümkün değildir: küme güvenlik, küme oluşturma sırasında etkindir.

Küme güvenlik senaryolar şunlardır:
* Düğümü düğümü güvenlik
* İstemcisi düğümü güvenlik
* Rol tabanlı erişim denetimi (RBAC)

Daha fazla bilgi için okuma [bir küme güvenli](service-fabric-cluster-security.md).

### <a name="scaling"></a>Ölçeklendirme
Kümeye yeni düğümler eklerseniz, Service Fabric çoğaltmalarını ve örnekleri sayısının artması düğümleri arasında yeniden dengeler. Genel uygulama performansını artıran ve bellek erişimi için Çekişme azaltır. Kümedeki düğümler verimli bir şekilde kullanılmayan, kümedeki düğüm sayısını azaltabilirsiniz. Service Fabric yeniden örnekleri ve çoğaltmalarını azalmasına her bir düğümüne donanım daha iyi kullanılmasını sağlamak için düğüm sayısı arasında yeniden dengeler. Azure kümelerinde ya da ölçeklendirebilirsiniz [el ile](service-fabric-cluster-scale-up-down.md) veya [program aracılığıyla](service-fabric-cluster-programmatic-scaling.md). Tek başına kümelerinin Genişletilmiş [el ile](service-fabric-cluster-windows-server-add-remove-nodes.md).

### <a name="cluster-upgrades"></a>Küme yükseltme
Service Fabric çalışma zamanı yeni sürümleri düzenli aralıklarla yayınlanır. Çalışma zamanı ya da doku, yükseltmeler, kümenizde gerçekleştirin, böylelikle her zaman çalıştırdığınız bir [sürümünü desteklenen](service-fabric-support.md). Fabric yükseltmelerinin ek olarak, küme yapılandırması sertifikalar veya uygulama bağlantı noktaları gibi güncelleştirebilirsiniz.

Service Fabric kümesi sahip, ancak kısmen Microsoft tarafından yönetilen bir kaynaktır. Microsoft, temel işletim sistemi düzeltme eki uygulama ve kümenizde doku yükseltme gerçekleştirme sorumludur. Microsoft yeni bir sürümü yayımlandığında otomatik fabric yükseltmelerinin almak için küme ayarlayın veya istediğiniz bir desteklenen yapı sürümü seçmek seçin. Yapı ve yapılandırma yükseltmeleri Azure portalı üzerinden veya Kaynak Yöneticisi aracılığıyla ayarlanabilir. Daha fazla bilgi için okuma [Service Fabric kümesi yükseltme](service-fabric-cluster-upgrade.md). 

Tek başına küme bir kaynaktır, tamamen kendi. Temel işletim sistemi düzeltme eki uygulama ve fabric yükseltmelerinin başlatmaktan sorumludur. Kümeniz için bağlanabiliyorsa [https://www.microsoft.com/download](https://www.microsoft.com/download), kümenizi otomatik olarak karşıdan yükle ve yeni Service Fabric çalışma zamanı paketi sağlamak için ayarlayabilirsiniz. Yükseltmeden sonra başlatmak. Kümenizi erişemiyorsanız [https://www.microsoft.com/download](https://www.microsoft.com/download), el ile bir Internet bağlantılı makineden yeni çalışma zamanı paketini karşıdan yüklemek ve ardından yükseltme başlatın. Daha fazla bilgi için okuma [bir tek başına Service Fabric kümesi yükseltme](service-fabric-cluster-upgrade-windows-server.md).

## <a name="health-monitoring"></a>Sistem durumunu izleme
Service Fabric tanıtır bir [sistem durumu modeli](service-fabric-health-introduction.md) sağlıksız küme ve belirli varlıkları (örneğin, küme düğümlerini ve hizmet çoğaltmaları) uygulama koşullara bayrak için tasarlanmıştır. Sistem durumu modeli sistem durumu reporters (sistem bileşenleri ve watchdogs) kullanır. , Kolay ve hızlı tanı ve onarım hedeftir. Beklemeleri gerekir sistem durumu hakkında ön düşünmek ve nasıl [sistem durumu raporlama tasarım](service-fabric-report-health.md#design-health-reporting). Özellikle, kök yakın bayrağı sorunları yardımcı olur, sistem durumu etkileyebilir herhangi bir koşul, bildirilmelidir. Sistem durumu bilgisi zamandan tasarruf edebilirsiniz ve hata ayıklama ve Araştırma Hizmeti çaba üretimde ölçekte da çalışır durumda.

Service Fabric reporters İzleyicisi'ni faiz koşullarını tanımlanır. Kendi yerel bir görünümü temel alarak bu koşullara bildirin. [Sistem durumu deposu](service-fabric-health-introduction.md#health-store) varlıklar genel sağlıklı olup olmadığını belirlemek için tüm reporters tarafından gönderilen sistem durumu verileri toplar. Model, zengin, esnek ve kullanımı kolay olacak şekilde tasarlanmıştır. Sistem durumu raporlarının kalitesini küme sistem durumu görünümünü doğruluğunu belirler. Yanlış sağlıksız sorunları göstermek hatalı pozitif sonuç, yükseltme veya sistem durumu verileri kullanan diğer hizmetler olumsuz yönde etkileyebilir. Bu tür hizmetlerin onarım hizmetleri ve uyarı mekanizmaları gösterilebilir. Bu nedenle, bazı düşünce olabilecek en iyi şekilde ilgi koşullarını yakalama raporları sağlamak için gereklidir.

Raporlama alanından yapılabilir:
* İzlenen Service Fabric hizmeti çoğaltma veya örnek.
* İç watchdogs Service Fabric hizmeti (koşullar izler ve raporları sorunları Örneğin, bir Service Fabric durum bilgisiz hizmet) dağıtılmış. Watchdogs tüm düğümler üzerinde dağıtılabilir veya izlenen hizmete affinitized.
* Service Fabric düğümlerinde çalıştırın, ancak Service Fabric Hizmetleri olarak uygulanmamıştır iç watchdogs.
* Service Fabric kümesi (örneğin, izleme hizmeti Gomez gibi) dışında kaynak araştırma dış watchdogs.

Kutudan çıktığında, Service Fabric bileşenleri kümedeki tüm varlıklar sistem durumu raporlamak. [Sistem durumu raporlarını](service-fabric-understand-and-troubleshoot-with-system-health-reports.md) küme ve uygulama işlevselliğini ve bayrağı sorunları sistem durumu aracılığıyla görünürlük sağlar. Uygulamalar ve hizmetler için sistem durumu raporlarını varlıkları uygulanır ve Service Fabric çalışma zamanı perspektifinden doğru davranmakta olduğunu doğrulayın. Raporlar tüm sistem durumu hizmeti iş mantığı ile izleme sağlamaz veya askıdaki işlemleri algılamak. Özel durum bilgileri hizmetinizin mantığı eklemek için [özel durumu raporlama uygulamak](service-fabric-report-health.md) hizmetinizde.

Service Fabric için birden çok yöntemleri sağlayan [görüntülemek sistem durumu raporlarının](service-fabric-view-entities-aggregated-health.md) health store içinde toplanır:
* [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) veya diğer görsel araçlar.
* Sistem durumu sorgularının (aracılığıyla [PowerShell](/powershell/module/ServiceFabric/), [C# FabricClient API'leri](/dotnet/api/system.fabric.fabricclient.healthclient) ve [Java FabricClient API'leri](/java/api/system.fabric._health_client), veya [REST API'leri](/rest/api/servicefabric)).
* Genel sistem durumu (PowerShell, API veya aracılığıyla REST) özelliklerinden biri olarak varlıklar, bu dönüş listesini sorgular.

Aşağıdaki Microsoft Virtual Academy video Service Fabric sistem durumu modeli ve nasıl kullanıldığı açıklanmaktadır:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tevZw56yC_1906218965">
<img src="./media/service-fabric-content-roadmap/HealthIntroVid.png" WIDTH="360" HEIGHT="244">
</a></center>

## <a name="next-steps"></a>Sonraki adımlar
* [Azure’da küme](service-fabric-cluster-creation-via-portal.md) veya [Windows’ta tek başına küme](service-fabric-cluster-creation-for-windows-server.md) oluşturma hakkında bilgi edinin.
* [Reliable Services](service-fabric-reliable-services-quick-start.md) veya [Reliable Actors](service-fabric-reliable-actors-get-started.md) programlama modelini kullanan bir hizmet oluşturmayı deneyin.
* Bilgi edinmek için nasıl [bulut hizmetlerinden geçiş](service-fabric-cloud-services-migration-differences.md).
* Öğrenme [izlemek ve Hizmetleri tanılamak](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). 
* Öğrenme [uygulamaları ve Hizmetleri test](service-fabric-testability-overview.md).
* Öğrenme [yönetmek ve küme kaynaklarının düzenlemek](service-fabric-cluster-resource-manager-introduction.md).
* Konum [Service Fabric örnekleri](http://aka.ms/servicefabricsamples).
* Hakkında bilgi edinin [Service Fabric destek seçenekleri](service-fabric-support.md).
* Okuma [blog ekip](https://blogs.msdn.microsoft.com/azureservicefabric/) makaleleri ve duyuruları için.


[cluster-application-instances]: media/service-fabric-content-roadmap/cluster-application-instances.png
[cluster-imagestore-apptypes]: ./media/service-fabric-content-roadmap/cluster-imagestore-apptypes.png
