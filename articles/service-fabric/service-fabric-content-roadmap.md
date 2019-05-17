---
title: Azure Service Fabric hakkında daha fazla bilgi edinin | Microsoft Docs
description: Azure Service Fabric temel alanları ve temel kavramlar hakkında bilgi edinin. Service Fabric ve mikro hizmetler oluşturma genişletilmiş bir genel bakış sağlar.
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/08/2017
ms.author: atsenthi
ms.openlocfilehash: a95baeb60ddff38e2aa1e36e7728c012d9d44930
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65540713"
---
# <a name="so-you-want-to-learn-about-service-fabric"></a>Bu nedenle, Service Fabric hakkında öğrenmek ister misiniz?
Azure Service Fabric; ölçeklenebilir ve güvenilir mikro hizmetleri paketlemeyi, dağıtmayı ve yönetmeyi kolaylaştırmayı sağlayan bir dağıtılmış sistemler platformudur.  Ancak, Service Fabric, büyük bir yüzey alanı vardır ve öğreneceğiniz çok şey yoktur.  Bu makale, Service fabric'in bir özeti sağlar ve programlama modellerini, uygulama yaşam döngüsü, test, kümeler ve sistem durumu izleme temel kavramları açıklar. Okuma [genel bakış](service-fabric-overview.md) ve [mikro hizmetler nedir?](service-fabric-overview-microservices.md) giriş ve Service Fabric mikro hizmetler oluşturmak için nasıl kullanılabilir. Bu makalede, kapsamlı bir içerik listesi içermiyor, ancak genel bakış ve Service Fabric için her bir alanı Başlarken makaleleri bağlantı. 

## <a name="core-concepts"></a>Temel kavramlar
[Service Fabric terminolojisi](service-fabric-technical-overview.md), [uygulama modeli](service-fabric-application-model.md), ve [desteklenen programlama modelleri](service-fabric-choose-framework.md) kavramları ve açıklamaları daha fazla sağlar, ancak temel bilgiler aşağıda verilmiştir.

### <a name="design-time-application-type-service-type-application-package-and-manifest-service-package-and-manifest"></a>Tasarım zamanı: uygulama türü, hizmet türü, uygulama paketini ve bildirimi, hizmet paketi ve bildirimi
Hizmet türlerinin bir koleksiyona atanan adı/sürümü bir uygulama türüdür. Bu tanımlanan bir *ApplicationManifest.xml* dosyasını bir uygulama paketi dizinine eklenir. Uygulama paketi daha sonra Service Fabric kümenin görüntü deposuna kopyalanır. Ardından, ardından kümede çalışan uygulama türünden adlandırılmış bir uygulama oluşturabilirsiniz. 

Hizmet adı/sürümü hizmetin kod paketleri, veri paketleri ve yapılandırma paketleri için atanan türüdür. Bu, bir hizmet paketi dizinde katıştırılmış bir ServiceManifest.xml dosyasında tanımlanır. Hizmet paketi dizini sonra bir uygulama paketin tarafından başvurulan *ApplicationManifest.xml* dosya. Küme içinde adlandırılmış bir uygulama oluşturduktan sonra adlandırılmış bir hizmet uygulaması türün hizmet türlerinden birini oluşturabilirsiniz. Hizmet türü tarafından açıklanan kendi *ServiceManifest.xml* dosya. Hizmet türü yürütülebilir kod ve çalışma zamanında yüklenen, hizmet yapılandırma ayarları ve hizmet tarafından kullanılan statik veri kümesinden oluşur.

![Service Fabric uygulama türleri ve hizmet türleri][cluster-imagestore-apptypes]

Uygulama paketi uygulama türün içeren bir disk dizindir *ApplicationManifest.xml* uygulama türü yaptığı her bir hizmet türünün hizmet paketleri başvuran dosya. Örneğin, bir uygulama paketi e-posta uygulama türü için bir kuyruk hizmeti paketi, bir ön uç hizmeti paketi ve bir veritabanı hizmeti paketi başvuruları içerebilir. Uygulama paketi dizindeki dosyaların, Service Fabric kümenin görüntü deposuna kopyalanır. 

Hizmet Paketi hizmet türün içeren bir disk dizindir *ServiceManifest.xml* kod, statik veri ve hizmet türü için yapılandırma paketleri başvuran dosya. Hizmet paketi dizindeki dosyaların uygulama türün tarafından başvurulan *ApplicationManifest.xml* dosya. Örneğin, bir hizmet paketi, kod, statik veri ve bir veritabanı hizmeti oluşturan yapılandırma paketlerini başvurabileceğiniz.

### <a name="run-time-clusters-and-nodes-named-applications-named-services-partitions-and-replicas"></a>Çalışma zamanı: kümeler ve uygulamalar, hizmetler, bölümler ve çoğaltmalar adlı adlı düğümleri
[Service Fabric kümesi](service-fabric-deploy-anywhere.md), mikro hizmetlerin dağıtılıp yönetildiği, ağa bağlı bir sanal veya fiziksel makine kümesidir. Kümeler binlerce makine içerecek şekilde ölçeklendirilebilir.

Bir kümenin parçası olan makine veya VM’lere düğüm denir. Her düğüme bir düğüm adı (bir dize) atanır. Düğümlerin yerleşim özellikleri gibi özellikleri vardır. Otomatik başlatma Windows hizmeti, her bir makine ya da VM sahip `FabricHost.exe`, önyükleme sırasında çalışmaya başlar ve ardından iki yürütülebilir dosyaları başlatır: `Fabric.exe` ve `FabricGateway.exe`. Bu iki yürütülebilir dosyalar düğümü olun. Geliştirme veya test senaryoları için birden çok örneğini çalıştırarak tek makineye veya sanal makine üzerinde birden fazla düğüm barındırabilir `Fabric.exe` ve `FabricGateway.exe`.

Adlandırılmış bir uygulama belirli bir işlev veya işlevler gerçekleştiren bir adlandırılmış Hizmetleri koleksiyonudur. Bir hizmet (bunu başlatabilir ve diğer hizmetler bağımsız olarak çalışır) tam ve tek başına bir işlevi gerçekleştirir ve kod, yapılandırma ve veri oluşur. Bir uygulama paketi görüntü deposuna kopyalandıktan sonra (adı/sürümü kullanarak) uygulama paketin uygulama türünü belirterek küme içindeki uygulamanın bir örneğini oluşturun. Her uygulama türü örneği şuna benzer bir URI ad atanan *fabric: / MyNamedApp*. Bir küme içinde tek bir uygulama türünden birden fazla adlandırılmış uygulamalar oluşturabilirsiniz. Ayrıca, farklı uygulama türleri adlandırılmış uygulamalar oluşturabilirsiniz. Her adlandırılmış bağımsız olarak yönetilen ve tutulan uygulamasıdır.

Adlandırılmış bir uygulama oluşturduktan sonra hizmet türü (adı/sürümü kullanarak) belirterek küme içinde hizmet türlerinden (adlandırılmış bir hizmeti) bir örneğini oluşturabilirsiniz. Her hizmet türü örneği, adlandırılmış uygulama URI altında kapsamlı bir URI ad atanır. "Veritabanım adlı hizmetin"adlı uygulama MyNamedApp"içinde" oluşturursanız, örneğin, URI aşağıdaki gibi görünür: *fabric: / MyNamedApp/Veritabanım*. Adlandırılmış bir uygulama içinde bir veya daha fazla adlandırılmış hizmetler oluşturabilirsiniz. Adlandırılmış her hizmetin kendi bölüm düzeni olabilir ve örnek/çoğaltma sayar. 

İki tür hizmet vardır: durum bilgisiz ve durum bilgisi olan. Durum bilgisi olmayan hizmetler hizmetinde durum depolamayın. Durum bilgisi olmayan hizmetler hiçbir kalıcı depolama alanı hiç sahip olmanız veya Azure depolama, Azure SQL veritabanı ve Azure Cosmos DB gibi bir dış depolama hizmeti kalıcı durumunu depolar. Durum bilgisi olan hizmet durumu hizmetinde depolar ve durumu yönetmek için güvenilir koleksiyonlar veya Reliable Actors programlama modelini kullanır. 

Adlandırılmış bir hizmet oluştururken bir bölüm düzeni belirtin. Büyük miktarlarda durumu hizmetleriyle verileri bölümler arasında bölün. Her bölüm, bir bölümü kümenin düğümlere yayılır hizmet tamamlandı durumunun sorumludur.  

Aşağıdaki diyagramda, uygulamalar ve hizmet örnekleri, bölümler ve çoğaltmalar arasındaki ilişkiyi gösterir.

![Bölümleri ve çoğaltmalarını hizmetinden][cluster-application-instances]

### <a name="partitioning-scaling-and-availability"></a>Bölümleme, ölçeklendirme ve kullanılabilirlik
[Bölümleme](service-fabric-concepts-partitioning.md) Service Fabric'e benzersiz değil. Bölümlemenin bir bilinen verileri bölümleme veya parçalama biçimidir. Durum bilgisi olan hizmetler durumu büyük miktarda verileri bölümler arasında bölün. Her bölüm tam hizmet durumunu değerinin bir bölümü için sorumlu değildir. 

Her bölüm çoğaltmalarını adlandırılmış hizmetinizin durumuna sağlayan küme düğümleri arasında yayılır [ölçek](service-fabric-concepts-scalability.md). Veri büyümesi gerektiğinde gibi bölümlerini büyütmenize ve Service Fabric bölümleri, donanım kaynakları verimli kullanılmasını sağlamak için düğümleri arasında yeniden dengeler. Kümeye yeni düğümler eklerseniz, Service Fabric düğümleri artan sayısı üzerinden bölüm çoğaltmalarını yeniden Dengeleme. Genel uygulama performansını artıran ve bellek erişim çekişmesini azaltır. Kümedeki düğümler verimli bir şekilde kullanılmayan, kümedeki düğümlerin sayısını azaltabilirsiniz. Service Fabric yeniden bölüm çoğaltmalarını azalan her düğümde donanım daha iyi kullanabilmesine için düğüm sayısını arasında yeniden dengeler.

Adlandırılmış durum bilgisi olan hizmetler çoğaltmalar bir bölüm içinde adlandırılmış durum bilgisi olmayan hizmetler örnekleri bulunur. Genellikle, bunlar iç durumu olmadan olduğundan adlandırılmış durum bilgisi olmayan hizmetler her zaman sadece bir bölüm vardır. Bölüm örnekleri sağlamak [kullanılabilirlik](service-fabric-availability-services.md). Bir örnek başarısız olursa diğer örnekleri normal şekilde çalışmaya devam eder ve ardından Service Fabric yeni bir örneğini oluşturur. Durum bilgisi olan hizmetler adlı korumak durumlarına çoğaltmalar ve her bölüm içinde kendi çoğaltma kümesi vardır. Okuma ve yazma işlemleri (birincil olarak adlandırılır) bir çoğaltma üzerinde gerçekleştirilir. Gelen durum değişiklik işlemleri (etkin ikincil veritabanı olarak adlandırılır) birden çok diğer kopyaya çoğaltılır yazın. Service Fabric, bir çoğaltması başarısız olursa, mevcut çoğaltmaları yeni bir kopya oluşturur.

## <a name="stateless-and-stateful-microservices-for-service-fabric"></a>Service Fabric için durum bilgisi olan ve olmayan mikro hizmetler
Service Fabric, mikro hizmetlerden veya kapsayıcılardan oluşmuş uygulamalar hazırlamanıza olanak tanır. Durum bilgisi olmayan mikro hizmetler (protokol ağ geçitleri ve web ara sunucuları gibi), isteğin veya hizmetten gelen yanıtının dışında değişebilir bir durum bilgisi bulundurmaz. Azure Cloud Services çalışan rolleri, durum bilgisi olmayan hizmet örnekleridir. Durum bilgisi olan mikro hizmetler (kullanıcı hesapları, veritabanları, cihazlar, alışveriş sepetleri ve kuyruklar gibi), isteğin ve yanıtının ötesinde değişebilir, yetkili bir durum bilgisi bulundurur. Günümüzün İnternet ölçeğindeki uygulamaları, durum bilgisi olan ve olmayan mikro hizmetlerin bileşiminden oluşur. 

Güçlü odağı ile ya da durum bilgisi olan hizmetler oluşturmaya odaklanmasıdır Service Fabric olan [yerleşik programlama modelleriyle](service-fabric-choose-framework.md) veya kapsayıcılı durum bilgisi olan hizmetler. [Uygulama senaryoları](service-fabric-application-scenarios.md), durum bilgisi olan hizmetlerin kullanıldığı senaryoları açıklar.

Durum bilgisi olmayan olanları yanı sıra durum bilgisi olan mikro hizmetler neden gerekiyor? İki temel neden şunlardır:

* Yüksek performanslı, düşük gecikme süreli, hataya dayanıklı çevrimiçi işlem işleme (OLTP) Hizmetleri kod ve verileriniz yakınınızda aynı makinede barındırarak oluşturabilirsiniz. Etkileşimli vitrinler, arama, nesnelerin interneti (IOT) sistemleri, ticaret sistemleri, kredi kartı işlemleri ve sahtekarlık algılama sistemleri ve kişisel kayıt yönetimi örnek verilebilir.
* Uygulama tasarımını basitleştirebilirsiniz. Durum bilgisi olan mikro Hizmetler ek kuyruklar ve önbellekler, yalnızca durum bilgisi olmayan bir uygulama kullanılabilirlik ve gecikme süresi gereksinimlerini karşılamak için geleneksel olarak gerekli olan gereksinimini kaldırın. Durum bilgisi olan doğal olarak yüksek kullanılabilirlik ve uygulamanızı bir bütün olarak yönetmek için hareketli parçadan sayısını azaltan gecikme süresi düşük, hizmetleridir.

## <a name="supported-programming-models"></a>Desteklenen programlama modelleri
Service Fabric, yazma ve hizmetlerinizi yönetmek için birden çok yol sunar. Hizmetleri, tam anlamıyla platformun özellikleri ve uygulama çerçeveleri için Service Fabric API'leri kullanabilirsiniz. Hizmetleri, herhangi bir dilde yazılmış ve bir Service Fabric kümede barındırılan derlenmiş bir yürütülebilir programı da olabilir. Daha fazla bilgi için [desteklenen programlama modelleri](service-fabric-choose-framework.md).

### <a name="containers"></a>Kapsayıcılar
Varsayılan olarak, Service Fabric dağıtır ve hizmet işlemleri olarak etkinleştirir. Service Fabric ayrıca Hizmetleri dağıtma [kapsayıcıları](service-fabric-containers-overview.md). Önemlisi, hizmetleri ve kapsayıcıları aynı uygulama Hizmetleri'nde karıştırabilirsiniz. Service Fabric Linux kapsayıcıları ve Windows kapsayıcıları dağıtımı, Windows Server 2016'da destekler. Var olan uygulamalar, durum bilgisi olmayan hizmetler veya durum bilgisi olan hizmetlerle kapsayıcılardaki dağıtabilirsiniz. 

### <a name="reliable-services"></a>Reliable Services
[Reliable Services](service-fabric-reliable-services-introduction.md) hizmetler Service Fabric platformu ile tümleştirin ve platform özellikleri tam kümesinden yararlanmak yazmak için basit bir çerçevedir. Güvenilir Hizmetleri durum bilgisiz olabilir (benzer şekilde, web sunucuları veya Azure Cloud Services çalışan rolleri gibi çoğu hizmet platformu), burada durumu kalıcıdır Azure DB veya Azure tablo depolama gibi harici bir çözümde. Reliable Services hizmeti kendisini güvenilir koleksiyonlar kullanarak doğrudan durum burada kalıcı durum bilgisi olan, de olabilir. Durum yapılan [yüksek oranda kullanılabilir](service-fabric-availability-services.md) çoğaltmayla ve dağıtılmış aracılığıyla [bölümleme](service-fabric-concepts-partitioning.md), Service Fabric tarafından otomatik olarak tüm yönetilen.

### <a name="reliable-actors"></a>Reliable Actors
Reliable Services üzerinde oluşturulmuş [Reliable Actor](service-fabric-reliable-actors-introduction.md) framework, aktör tasarım deseni temel alınarak sanal gerçekleştiren deseni, uygulayan bir uygulama altyapısıdır. Reliable Actor çerçeve yürütme aktörler olarak adlandırılan tek iş parçacıklı işlem durumu ve bağımsız bir birim kullanır. Reliable Actor çerçevesi sağlar, iletişim aktörler için yerleşik ve durumu kalıcı ve genişleme yapılandırmaları önceden ayarlayın.

### <a name="aspnet-core"></a>ASP.NET Core
Service Fabric ile tümleştirilir [ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md) web ve API uygulamaları oluşturmak için birinci sınıf bir programlama modeli olarak.  ASP.NET Core, Service Fabric iki farklı şekillerde kullanılabilir:

- Konuk tarafından yürütülebilir bir dosya olarak barındırılan. Bu, öncelikli olarak mevcut ASP.NET Core uygulamaları kod değişikliği olmadan Service Fabric üzerinde çalıştırmak için kullanılır.
- Güvenilir bir hizmet içinde çalıştırın. Bu, Service Fabric çalışma zamanı ile daha iyi tümleştirme sağlar ve durum bilgisi olan ASP.NET Core hizmetleri sağlar.

### <a name="guest-executables"></a>Konuk yürütülebilir dosyaları
A [Konuk yürütülebilir dosyası](service-fabric-guest-executables-introduction.md) olan diğer hizmetlerin yanı sıra bir Service Fabric kümesi üzerinde barındırılan (herhangi bir dilde yazılmış) var olan ve isteğe bağlı çalıştırılabilir. Konuk yürütülebilir dosyaları doğrudan Service Fabric API'leri ile tümleştirerek değil. Ancak bunlar yine de bir platform sunar, özel sistem durumu gibi özelliklerinden yararlanmak ve raporlama yüklemek ve REST API'lerini çağırarak bulunabilirliği hizmet. Bunlar ayrıca, tam uygulama yaşam döngüsü desteği de vardır. 

## <a name="application-lifecycle"></a>Uygulama yaşam döngüsü
Diğer platformlar ile Service fabric'te uygulama genellikle aşağıdaki aşamaları geçtikçe: tasarım, geliştirme, test, dağıtım, yükseltme, Bakım ve kaldırma. Service Fabric, nihai yetkisinin alınması için bulut uygulamaları, geliştirme, dağıtım, günlük yönetim ve Bakım tam uygulama yaşam döngüsü için birinci sınıf destek sağlar. Hizmet modeli, uygulama yaşam döngüsü içinde bağımsız olarak katılmak birkaç farklı rol sağlar. [Service Fabric uygulama yaşam döngüsü](service-fabric-application-lifecycle.md) API'leri ve Service Fabric uygulama yaşam döngüsünün aşamaları boyunca farklı rolleri tarafından nasıl kullanıldıkları hakkında genel bir bakış sağlar. 

Tüm uygulama yaşam döngüsü kullanılarak yönetilebilir [PowerShell cmdlet'leri](/powershell/module/ServiceFabric/), [CLI komutları](service-fabric-sfctl.md), [C# API](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient), [Java API](/java/api/overview/azure/servicefabric), ve [ REST API'leri](/rest/api/servicefabric/). Gibi araçları kullanarak sürekli tümleştirme/sürekli dağıtım işlem hatlarını de ayarlayabilirsiniz [Azure işlem hatları](service-fabric-set-up-continuous-integration.md) veya [Jenkins](service-fabric-cicd-your-linux-applications-with-jenkins.md).

## <a name="test-applications-and-services"></a>Uygulama ve hizmetleri test etme
Gerçek anlamda bulut ölçekli hizmetler oluşturmak için uygulamalarınızı ve hizmetlerinizi gerçek hataları dayanabilir doğrulamak önemlidir. Hata analizi hizmeti, Service Fabric'te yerleşik hizmetlere test etmek için tasarlanmıştır. İle [hata analizi hizmeti](service-fabric-testability-overview.md), anlamlı hataları anlamına ve uygulamalarınızı karşı tam test senaryoları çalıştırın. Bu hataları ve senaryoları çalışma ve çeşitli durumları ve hizmet ömrü boyunca, tüm denetimli, güvenli ve tutarlı bir şekilde yaşar geçişleri doğrulayın.

[Eylemler](service-fabric-testability-actions.md) bireysel hataları kullanarak test etmek için bir hizmet hedef. Bir hizmet Geliştirici karmaşık senaryoları yazmak için bu yapı taşları olarak kullanabilirsiniz. Benzetimli hataların örnekleri şunlardır:

* Burada bir makine veya sanal makine yeniden durumlar herhangi bir sayıda benzetimini yapmak için bir düğümü yeniden başlatın.
* Yük Dengeleme, yük devretme veya uygulama yükseltmesi benzetimini yapmak için durum bilgisi olan hizmet çoğaltmasını taşıyın.
* Çekirdek kayıp yeni verileri kabul etmek için yeterli "yedekleme" veya "ikincil" çoğaltma olmadığından burada yazma işlemleri devam edemiyor bir durum oluşturmak için bir durum bilgisi olan hizmet olarak çağırır.
* Burada tüm bellek içi durumu tamamen silindikten bir durum oluşturmak için bir durum bilgisi olan hizmet veri kaybı çağırma.

[Senaryoları](service-fabric-testability-scenarios.md) karmaşık işlemleri bir veya daha fazla eylem oluşur. Hata analizi hizmeti, iki yerleşik tam senaryoları sağlar:

* [Kaos senaryo](service-fabric-controlled-chaos.md)-küme boyunca sürekli ve çift aralıklı hatalar (normal ve yaşanmamasını) uzun süreler boyunca benzetimini yapar.
* [Yük devretme senaryosu](service-fabric-testability-scenarios.md#failover-test)-chaos test senaryosunun, diğer hizmetleri etkilenmeyen bırakarak belirli hizmet bölüm hedefleyen bir sürümü.

## <a name="clusters"></a>Kümeler
[Service Fabric kümesi](service-fabric-deploy-anywhere.md), mikro hizmetlerin dağıtılıp yönetildiği, ağa bağlı bir sanal veya fiziksel makine kümesidir. Kümeler binlerce makine içerecek şekilde ölçeklendirilebilir. Bir makine ya da bir kümenin parçası olan sanal makine bir küme düğümü adı verilir. Her düğüme bir düğüm adı (bir dize) atanır. Düğümlerin yerleşim özellikleri gibi özellikleri vardır. Her bir makine veya sanal makine bir otomatik başlatılan hizmet sahip `FabricHost.exe`, önyükleme sırasında çalışmaya başlar ve ardından iki yürütülebilir dosyaları başlatır: Fabric.exe ve FabricGateway.exe. Bu iki yürütülebilir dosyalar düğümü olun. Senaryolarını test etmek için tek bir makine veya sanal makine üzerinde birden fazla düğüm birden çok örneğini çalıştırarak barındırabilirsiniz `Fabric.exe` ve `FabricGateway.exe`.

Windows Server veya Linux çalıştıran sanal veya fiziksel makinelere Service Fabric kümeleri oluşturulabilir. Dağıtmak ve Service Fabric uygulamaları birbirlerine bağlanış Windows Server veya Linux bilgisayarlar kümesi sahip olduğu herhangi bir ortamda çalıştırmak için: şirket içi, Microsoft Azure üzerinde veya tüm bulut sağlayıcıları.

### <a name="clusters-on-azure"></a>Azure’da kümeler
Service Fabric kümeleri Azure üzerinde çalışan işlemleri ve küme yönetimi daha kolay ve daha güvenilir olmasını sağlayan diğer Azure özellikleri ve Hizmetleri ile tümleştirme sağlar. Bir kümenin bir Azure Resource Manager kaynağı olduğundan kümeleri gibi azure'daki diğer kaynakları modelleyebilirsiniz. Kaynak Yöneticisi, ayrıca tek bir birim olarak küme tarafından kullanılan tüm kaynakları kolay yönetim sağlar. Azure'da kümeler Azure Tanılama ile tümleştirilir ve Azure İzleyici günlüğe kaydeder. Küme düğümü türleri [sanal makine ölçek kümeleri](/azure/virtual-machine-scale-sets/index), otomatik ölçeklendirme işlevine oluşturulmuştur.

Azure'da bir küme oluşturabilirsiniz [Azure portalında](service-fabric-cluster-creation-via-portal.md), gelen bir [şablon](service-fabric-cluster-creation-via-arm.md), veya [Visual Studio](service-fabric-cluster-creation-via-visual-studio.md).

Linux üzerinde Service Fabric oluşturmanızı, dağıtmanızı ve Windows üzerinde yaptığınız gibi Linux üzerinde yüksek oranda kullanılabilir ve ölçeklenebilir uygulamaları yönetmenize olanak sağlar. Service Fabric çerçeveleri (Reliable Services ve Reliable Actors) yanı sıra C# (.NET Core), Linux üzerinde Java kullanılabilir. Ayrıca yapı [Konuk yürütülebilir Hizmetleri](service-fabric-guest-executables-introduction.md) ile herhangi bir dil veya çerçeve. Docker kapsayıcıları işlemlerini de desteklenir. Docker kapsayıcıları, Konuk yürütülebilir dosyalar veya Service Fabric çerçeveleri kullanan yerel Service Fabric Hizmetleri çalıştırabilirsiniz. Hakkında daha fazla bilgi için okuma [Linux üzerindeki Service Fabric](service-fabric-deploy-anywhere.md).

Windows üzerinde desteklenip Linux'ta desteklenmeyen bazı özellikler mevcuttur. Daha fazla bilgi edinmek için [Windows ve Linux üzerindeki Service Fabric arasındaki farklar](service-fabric-linux-windows-differences.md).

### <a name="standalone-clusters"></a>Tek başına kümeler
Service Fabric, tek başına Service Fabric kümeleri şirket içi oluşturmak için veya tüm bulut sağlayıcılarından bir yükleme paketi sağlar. Tek başına kümeler, küme, istediğiniz yerde barındırın özgürlüğü sağlar. Verilerinizi uyumluluğu veya yasal kısıtlamalar tabi olan veya verilerinizi yerel tutmak isterseniz kendi kümeyi ve uygulamalarını barındırabilir. Uygulamaları oluşturma bilginiz barındırma bir ortamdan diğerine yaşamanız için Service Fabric uygulamaları birden çok barındırma ortamları değişikliğine gerek olmadan çalıştırabilirsiniz. 

[İlk Service Fabric tek başına kümenizi oluşturma](service-fabric-cluster-creation-for-windows-server.md)

Linux tek başına kümeler henüz desteklenmemektedir.

### <a name="cluster-security"></a>Küme güvenliği
Özellikle de üretim iş yükleri üzerinde çalıştırılan olduğunda, kümeye bağlanma yetkisiz kullanıcıların önlemek için kümeleri güvenli hale getirilmelidir. Güvenli olmayan bir kümeye oluşturmak mümkün olsa da, bunun yapılması bu nedenle yönetim uç noktalarını genel internet'e gösteriliyorsa için anonim bağlanmasına olanak sağlar. Daha sonra güvenli olmayan bir kümeye güvenliği etkinleştirmek mümkün değildir: küme güvenliği, küme oluşturma sırasında etkinleştirilir.

Küme güvenliği senaryoları şunlardır:
* Düğümler için güvenlik
* İstemci düğümü güvenlik
* Rol tabanlı erişim denetimi (RBAC)

Daha fazla bilgi için okuma [küme güvenliğini sağlama](service-fabric-cluster-security.md).

### <a name="scaling"></a>Ölçeklendirme
Kümeye yeni düğümler eklerseniz, Service Fabric örnekleri ve bölüm çoğaltmaları sayısının artması düğümleri arasında yeniden dengeler. Genel uygulama performansını artıran ve bellek erişim çekişmesini azaltır. Kümedeki düğümler verimli bir şekilde kullanılmayan, kümedeki düğümlerin sayısını azaltabilirsiniz. Service Fabric yeniden örnekleri ve bölüm çoğaltmalarını azalan her düğümde donanım daha iyi kullanabilmesine için düğüm sayısını arasında yeniden dengeler. Azure'da kümeler ya da ölçekleyebilirsiniz [el ile](service-fabric-cluster-scale-up-down.md) veya [program aracılığıyla](service-fabric-cluster-programmatic-scaling.md). Tek başına kümeler ölçeklenebilen [el ile](service-fabric-cluster-windows-server-add-remove-nodes.md).

### <a name="cluster-upgrades"></a>Küme yükseltme
Service Fabric çalışma zamanının yeni sürümlerini düzenli aralıklarla yayınlanır. Böylece her zaman çalıştırdığınız kümenizde çalışma zamanı veya yapı, yükseltmeleri gerçekleştirme bir [sürüm desteklenen](service-fabric-support.md). Yapı yükseltmeleri ek olarak, küme yapılandırması sertifikaları ya da uygulama bağlantı noktaları gibi güncelleştirebilirsiniz.

Bir Service Fabric kümesine sahip, ancak kısmen Microsoft tarafından yönetilen bir kaynaktır. Microsoft, temel işletim sistemi düzeltme eki uygulama ve kümeniz üzerinde yapı yükseltmeleri gerçekleştirme sorumludur. Microsoft yeni bir sürümü yayımlandığında otomatik yapı yükseltmeleri, almak için Küme veya istediğiniz bir desteklenen yapı sürümü seçmek seçim yapabilirsiniz. Doku ve yapılandırma yükseltmeleri, Azure portalı üzerinden ya da Resource Manager aracılığıyla ayarlanabilir. Daha fazla bilgi için okuma [bir Service Fabric kümesini yükseltme](service-fabric-cluster-upgrade.md). 

Tek başına küme bir kaynaktır, tamamen kendi. Temel işletim sistemi düzeltme eki uygulama ve yapı yükseltmeleri başlatmaktan sorumlu olursunuz. Kümenize bağlanabilir, [ https://www.microsoft.com/download ](https://www.microsoft.com/download), kümenizi otomatik olarak indirmek ve yeni Service Fabric çalışma zamanı paketi sağlamak için ayarlayabilirsiniz. Ardından, yükseltmeyi başlatmak. Kümenizi erişemiyorsanız [ https://www.microsoft.com/download ](https://www.microsoft.com/download), el ile İnternet'e bağlı makineden yeni çalışma zamanı paketi indirebilir ve ardından yükseltmeyi başlatın. Daha fazla bilgi için okuma [tek başına Service Fabric kümesini yükseltme](service-fabric-cluster-upgrade-windows-server.md).

## <a name="health-monitoring"></a>Sistem durumunu izleme
Service Fabric tanıtır bir [sistem durumu modeli](service-fabric-health-introduction.md) sağlıksız küme ve özel varlıklar (örneğin, küme düğümleri ve hizmet çoğaltmalar) uygulama koşullara bayrağı için tasarlanmıştır. Sistem durumu raporlayıcıları (sistem bileşenleri ve watchdogs) sistem durumu modeli kullanır. , Kolay ve hızlı tanılama ve onarma hedeftir. Beklemeleri gereken ön durumu hakkında düşünmek ve nasıl [sağlık raporlaması tasarım](service-fabric-report-health.md#design-health-reporting). Özellikle bayrağı sorunların kök yakın yardımcı olabilir, sistem durumu etkileyebilecek herhangi bir koşul, bildirilmelidir. Sistem durumu bilgileri zamandan tasarruf edebilirsiniz ve çaba araştırma hizmet hata ayıklama ve üretim ölçekli hazır ve çalışır durumda.

Service Fabric raporlayıcıları İzleyici ilgi koşullar belirledik. Bunlar, yerel bir görünümü temel alarak bu koşullara rapor. [Sistem durumu deposu](service-fabric-health-introduction.md#health-store) varlıkları genel olarak sağlıklı olup olmadığını belirlemek için tüm raporlayıcıları tarafından gönderilen sistem durumu verilerini toplar. Model, zengin, esnek ve kullanımı kolay olacak şekilde tasarlanmıştır. Sistem Durumu raporu Kalite küme sistem durumu görünümünü doğruluğunu belirler. Yanlış sağlıksız sorunları Göster hatalı pozitif sonuçları, yükseltme ya da sistem durumu verileri kullanan diğer hizmetlerde olumsuz yönde etkileyebilir. Onarım hizmetleri ve uyarı mekanizmaları gibi hizmetlere örnektir. Bu nedenle, bazı döndürülebileceği bakımından, olabilecek en iyi şekilde gösterdiğiniz ilgi koşullarını yakalama raporları sağlamak için gereklidir.

Raporlama alanından yapılabilir:
* İzlenen Service Fabric hizmeti çoğaltma veya örnek.
* İç watchdogs (koşullar izler ve raporlar sorunları gibi bir Service Fabric durum bilgisi olmayan hizmet) Service Fabric hizmet olarak dağıtılabilir. Watchdogs tüm düğümler üzerinde dağıtılabilir veya izlenen hizmete affinitized.
* Service Fabric düğümleri üzerinde çalışmak, ancak Service Fabric Hizmetleri uygulanmaz ve iç watchdogs.
* Service Fabric kümesine (örneğin, izleme hizmeti Gomez gibi) dışında kaynak araştırma dış watchdogs.

Kullanıma hazır, Service Fabric bileşenleri kümedeki tüm varlıklarda sistem durumu rapor. [Sistem durumu raporlarını](service-fabric-understand-and-troubleshoot-with-system-health-reports.md) küme ve uygulama işlevselliğini ve bayrağı sorunları üzerinden sistem durumu görünürlük sağlar. Uygulamalar ve hizmetler için sistem durumu raporlarını varlıkları uygulanır ve Service Fabric çalışma açısından doğru davrandığını doğrulayın. Raporlar tüm sistem durumu hizmetinin iş mantığını izleme sağlamaz veya yanıt vermeyi durdurdu işlemlerini algılama. Hizmetinizin mantığı, özel sistem durumu bilgileri eklemek [özel durum raporlama uygulamak](service-fabric-report-health.md) hizmetlerinizdeki.

Service Fabric için birden çok yol sağlar [sistem durumu raporları görüntüleme](service-fabric-view-entities-aggregated-health.md) health store içinde toplanır:
* [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) veya diğer görselleştirme araçları.
* Sistem durumu sorgularının sayısı (aracılığıyla [PowerShell](/powershell/module/ServiceFabric/), [CLI](service-fabric-sfctl.md), [C# FabricClient API'leri](/dotnet/api/system.fabric.fabricclient.healthclient) ve [Java FabricClient API'leri](/java/api/system.fabric), veya [REST API'leri](/rest/api/servicefabric)).
* Genel sistem durumu özellikleri (aracılığıyla, PowerShell, CLI, API'leri veya REST) biri olarak varlıklar listesi, dönüş sorgular.

## <a name="monitoring-and-diagnostics"></a>İzleme ve tanılama
[İzleme ve tanılama](service-fabric-diagnostics-overview.md) geliştirme, test ve uygulamaları ve Hizmetleri her türlü ortamda dağıtmak için kritik öneme sahiptir. Service Fabric çözümlerini planlama ve izleme uygulayın ve yardımcı olan tanılama uygulamaları sağlamak ve hizmetler, yerel geliştirme ortamında veya üretim beklendiği gibi çalışıyor en iyi çalışır.

Başlıca amaçlarından biri, izleme ve tanılama vardır:

- Donanım ve altyapı sorunları algılayın ve tanılayın
- Yazılım ve uygulama sorunları algılayın, hizmet kapalı kalma süresini azaltın
- Kaynak tüketimi ve Yardım sürücü operations kararlarını anlama
- Uygulama, hizmet ve altyapı performansını iyileştirme
- İşe yönelik öngörüleri oluşturmak ve geliştirilecek alanlar tanımlayın
 
Genel iş akışının izleme ve tanılama üç adımdan oluşur:

1. Olay oluşturma: altyapı (küme), platform ve uygulama / hizmet düzeyinde bu olayları (günlüklerini, izlemeleri, özel olaylar) içerir
2. Olay toplama: oluşturulan olay gereken toplanacağı ve bunların görüntülenebilmesi toplanır
3. Analiz: olayları görselleştirilmiş ve analiz için izin vermek ve gerektiğinde görüntülemek için bazı biçiminde erişilebilir olması gerekir

Birden çok ürünlerin olan kullanılabilir, bu üç alanlarını kapsar ve her biri için farklı teknolojileri seçebilirsiniz. Daha fazla bilgi için okuma [Azure Service Fabric için izleme ve tanılama](service-fabric-diagnostics-overview.md).

## <a name="next-steps"></a>Sonraki adımlar
* [Azure’da küme](service-fabric-cluster-creation-via-portal.md) veya [Windows’ta tek başına küme](service-fabric-cluster-creation-for-windows-server.md) oluşturma hakkında bilgi edinin.
* [Reliable Services](service-fabric-reliable-services-quick-start.md) veya [Reliable Actors](service-fabric-reliable-actors-get-started.md) programlama modelini kullanan bir hizmet oluşturmayı deneyin.
* Bilgi edinmek için nasıl [Cloud Services'tan geçiş](service-fabric-cloud-services-migration-differences.md).
* Öğrenme [Hizmetleri izleme ve tanılama](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). 
* Öğrenme [uygulamalarınızı ve hizmetlerinizi test](service-fabric-testability-overview.md).
* Öğrenme [yönetmek ve küme kaynakları düzenlemek](service-fabric-cluster-resource-manager-introduction.md).
* Konum [Service Fabric örneklerine](https://aka.ms/servicefabricsamples).
* Hakkında bilgi edinin [Service Fabric destek seçenekleri](service-fabric-support.md).
* Okuma [ekip blogu](https://blogs.msdn.microsoft.com/azureservicefabric/) makaleleri ve duyuruları.


[cluster-application-instances]: media/service-fabric-content-roadmap/cluster-application-instances.png
[cluster-imagestore-apptypes]: ./media/service-fabric-content-roadmap/cluster-imagestore-apptypes.png
