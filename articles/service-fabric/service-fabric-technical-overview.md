---
title: Azure Service Fabric terminolojisi öğrenin | Microsoft Docs
description: Service Fabric terminolojisi bakış. Önemli terminolojiyi kavramlar ve belgelere geri kalanı kullanılan terimler açıklanmaktadır.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: chackdan;subramar
ms.assetid: 3a970679-e19e-43b3-9be8-71773f307c57
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/26/2018
ms.author: ryanwi
ms.openlocfilehash: e3da081f9b327031d6d1e0afd2f2fb52383bf933
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34212077"
---
# <a name="service-fabric-terminology-overview"></a>Service Fabric terminolojisi genel bakış
Azure Service Fabric; ölçeklenebilir ve güvenilir mikro hizmetleri paketlemeyi, dağıtmayı ve yönetmeyi kolaylaştırmayı sağlayan bir dağıtılmış sistemler platformudur. Bu makalede belgelerde kullanılan terimler anlamak için Service Fabric tarafından kullanılan terminolojiyi ayrıntılarını verir.

Bu bölümde listelenen kavramları aşağıdaki Microsoft Virtual Academy videoları ele alınmıştır: <a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965">temel kavramları</a>, <a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tlkI046yC_2906218965">tasarım zamanı kavramları</a>, ve <a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=x7CVH56yC_1406218965">çalışma zamanı kavramları</a>.

## <a name="infrastructure-concepts"></a>Altyapı kavramları
**Küme**: içine, mikro dağıtılır ve yönetilen sanal veya fiziksel makineler ağa bağlı bir dizi.  Kümeler binlerce makine içerecek şekilde ölçeklendirilebilir.

**Düğüm**: makine ya da bir kümenin parçasıysa VM adlı bir *düğümü*. Her düğüme bir düğüm adı (bir dize) atanır. Düğümleri yerleşim özellikleri gibi özelliklere sahiptir. Her makine ya da VM bir otomatik başlatılan Windows hizmet sahip `FabricHost.exe`, önyükleme sırasında çalışmaya başlar ve ardından iki yürütülebilir dosyalar başlatır: `Fabric.exe` ve `FabricGateway.exe`. Bu iki yürütülebilir dosyalar düğümü olun. Senaryoları test etmek için bir tek makine ya da VM birden çok düğümde birden çok örneğini çalıştırarak barındırabilir `Fabric.exe` ve `FabricGateway.exe`.

## <a name="application-concepts"></a>Uygulama kavramları
**Uygulama türü**: hizmet türlerinin bir koleksiyona atanan ad/sürüm. İçinde tanımlanan bir `ApplicationManifest.xml` dosyası ve bir uygulama paketi dizininde katıştırılmış. Dizin sonra Service Fabric kümenin görüntü deposuna kopyalanır. Bu uygulama türü küme içindeki bir adlandırılmış uygulama sonra oluşturabilirsiniz.

Okuma [uygulama modeli](service-fabric-application-model.md) daha fazla bilgi için makalenin.

**Uygulama paketi**: uygulama tür içeren bir disk dizini `ApplicationManifest.xml` dosya. Uygulama türü yapar her hizmet türünün hizmet paketleri başvurur. Uygulama paketi dizindeki dosyaların Service Fabric kümenin görüntü deposuna kopyalanır. Örneğin, e-posta uygulama türü için bir uygulama paketi sıra hizmeti paketi, ön uç hizmet paketi ve veritabanı hizmet paketi başvurular içeriyor olabilir.

**Uygulama adlı**: bir uygulama paketi görüntü deposuna kopyaladıktan sonra küme içindeki uygulama örneği oluşturun. Ad veya sürüm kullanarak uygulama paketin uygulama türü belirttiğinizde örneği oluşturun. Her uygulama türü örneğe benzer bir Tekdüzen Kaynak Tanımlayıcısı (URI) adı atanmış: `"fabric:/MyNamedApp"`. Bir küme içinde tek bir uygulama türünden birden çok adlandırılmış uygulamaları oluşturabilirsiniz. Ayrıca, farklı uygulama türlerinden adlandırılmış uygulamaları oluşturabilirsiniz. Her adlandırılmış bağımsız olarak yönetilen ve sürümü tutulan uygulamasıdır.      

**Hizmet türü**: bir hizmetin kod paketler, veri paketleri ve yapılandırma paketleri atanan ad/sürüm. Hizmet türü tanımlanan `ServiceManifest.xml` dosyası ve bir hizmet paketi dizinde katıştırılmış. Hizmet paketi dizini sonra bir uygulama paketin tarafından başvurulan `ApplicationManifest.xml` dosya. Küme içinde adlandırılmış bir uygulama oluşturduktan sonra adlandırılmış bir hizmet uygulaması tür hizmet türlerinden birini oluşturabilirsiniz. Hizmet türünün `ServiceManifest.xml` dosya hizmeti tanımlar.

Okuma [uygulama modeli](service-fabric-application-model.md) daha fazla bilgi için makalenin.

İki tür hizmet vardır:

* **Durum bilgisiz**: Azure Storage, Azure SQL Database veya Azure Cosmos DB gibi bir dış depolama hizmeti hizmetin sürekli durumu depolandığında durumsuz bir hizmet kullanın. Hizmet hiçbir kalıcı depolama alanına sahip bir durum bilgisi olmayan hizmetin kullanın. Örneğin, burada değerleri hizmetine geçirilir ve hesaplayıcı hizmet için bu değerleri kullanan bir hesaplama gerçekleştirilir ve sonra bir sonuç döndürdü.
* **Durum bilgisi olan**: Hizmetinizin durumunu kendi güvenilir koleksiyonları veya Reliable Actors modellerini programlama aracılığıyla yönetmek için Service Fabric istediğinizde, durum bilgisi olan bir hizmet kullanın. Adlandırılmış bir hizmet oluşturduğunuzda, ölçeklenebilirlik, durumu üzerinden yayılan istediğiniz kaç bölümleri belirtin. Ayrıca, güvenilirlik için düğümleri arasında durumunuza çoğaltmak için kaç kez belirtin. Tek bir birincil çoğaltma ve birden fazla ikincil çoğaltmaları her adlandırılmış hizmetin vardır. Birincil çoğaltmadan yazdığınızda adlandırılmış Hizmetinizin durumunu değiştirin. Service Fabric, durumunuza eşitlenmiş tutmak için tüm ikincil çoğaltmalar için daha sonra bu durum çoğaltır. Birincil çoğaltma işlemi başarısız olur ve var olan bir ikincil çoğaltma birincil çoğaltmaya yükseltir Service Fabric otomatik olarak algılar. Service Fabric daha sonra yeni bir ikincil çoğaltma oluşturur.  

**Çoğaltmalar veya örnekleri** çalıştıran ve kod (ve durum bilgisi olan hizmetler için durumu) dağıtılan bir hizmetin bakın. Bkz: [çoğaltmaları ve örnekleri](service-fabric-concepts-replica-lifecycle.md).

**Yeniden yapılandırma** çoğaltma kümesi bir hizmeti, herhangi bir değişiklik işlemini başvuruyor. Bkz: [yeniden yapılandırma](service-fabric-concepts-reconfiguration.md).

**Hizmet paketi**: hizmet türünün içeren bir disk dizini `ServiceManifest.xml` dosya. Bu dosya kodu, statik verileri ve yapılandırma paketleri hizmet türünün başvurur. Hizmet paketi dizindeki dosyaların uygulama tür tarafından başvurulan `ApplicationManifest.xml` dosya. Örneğin, bir hizmet paketi, kod, statik verileri ve veritabanı hizmeti oluşturan yapılandırma paketleri başvurabilir.

**Adlı hizmetin**: adlandırılmış bir uygulama oluşturduktan sonra küme içindeki hizmet türlerinden birini örneği oluşturabilirsiniz. Hizmet türü, ad/sürüm kullanarak belirtin. Her hizmet türü örneği, adlandırılmış uygulamanın URI altında kapsamlı bir URI ad atanır. "Hizmet"uygulama adlı bir MyNamedApp"içinde adlı bir Veritabanım" oluşturursanız, örneğin, URI gibi görünüyor: `"fabric:/MyNamedApp/MyDatabase"`. Adlandırılmış bir uygulama içinde birden fazla adlandırılmış hizmetler oluşturabilirsiniz. Adlandırılmış her hizmetin kendi bölüm düzeni ve örnek olabilir veya çoğaltma sayar.

**Kod paketi**: hizmet türünün yürütülebilir dosyaları, genellikle EXE/DLL dosyaları içeren bir disk dizini. Kod paketi dizindeki dosyaların hizmet türünün tarafından başvurulan `ServiceManifest.xml` dosya. Adlandırılmış bir hizmet oluşturduğunuzda, kod paketi düğüme veya adlandırılmış hizmetini çalıştırmak için seçili düğümlere kopyalanır. Ardından kod çalışmaya başlar. Kod Paketi yürütülebilir dosyalar iki tür vardır:

* **Konuk yürütülebilir dosyalar**: Farklı Çalıştır yürütülebilir dosyalar-ana bilgisayar işletim sistemindeki (Windows veya Linux). Bu yürütülebilir dosyalar yoksa bağlamak veya herhangi bir Service Fabric çalışma zamanı dosya başvuru ve bu nedenle yok modellerini programlama herhangi Service Fabric kullanın. Bu yürütülebilir dosyaları Adlandırma Hizmeti uç noktası bulma gibi bazı Service Fabric özelliklerini kullanma sorunu yaşıyor. Konuk yürütülebilir dosyaları her hizmet örneği için belirli yük ölçümleri raporlayamaz.
* **Hizmet ana yürütülebilir dosyalar**: Service Service Fabric çalışma zamanı dosyaları bağlayarak modellerini programlama Service Fabric özelliklerini etkinleştirme Fabric kullanan yürütülebilir. Örneğin, bir adlandırılmış hizmet örneği uç noktaları Service Fabric'ın adlandırma hizmeti ile kaydedebilir ve yük ölçümleri de bildirebilirsiniz.      

**Veri paketi**: hizmet türünün statik ve salt okunur veri dosyaları, genellikle fotoğraf, ses ve video dosyaları içeren bir disk dizini. Veri paketi dizindeki dosyaların hizmet türünün tarafından başvurulan `ServiceManifest.xml` dosya. Adlandırılmış bir hizmet oluşturduğunuzda, veri paketi düğüme veya adlandırılmış hizmetini çalıştırmak için seçili düğümlere kopyalanır. Kod çalışmaya başlar ve artık veri dosyalarına erişebilir.

**Yapılandırma paketi**: hizmet türünün statik ve salt okunur yapılandırma dosyalarını, genellikle metin dosyalarını içeren bir disk dizini. Yapılandırma paketi dizindeki dosyaların hizmet türünün tarafından başvurulan `ServiceManifest.xml` dosya. Adlandırılmış bir hizmet oluşturduğunuzda, dosyalar kopyalanan bir yapılandırma paketi veya adlandırılmış hizmetini çalıştırmak için seçili daha fazla düğüm. Ardından kod, yapılandırma dosyalarını ve artık erişim başlatır.

**Kapsayıcıları**: varsayılan olarak, Service Fabric dağıtır ve Hizmetleri işlemler olarak etkinleştirir. Service Fabric kapsayıcı görüntüleri Hizmetleri'nde de dağıtabilirsiniz. Uygulamaları temel işletim sisteminden sanallaştıran sanallaştırma teknolojisi kapsayıcılardır. Bir uygulama ve onun çalışma zamanı, bağımlılıklar ve sistem kitaplıkları içinde bir kapsayıcı çalıştırın. Kapsayıcı tam, işletim sistemi yapıları yalıtılmış kapsayıcının kendi görünümünü özel erişimi var. Service Fabric, Linux ve Windows Server kapsayıcılarında Docker kapsayıcıları destekler. Daha fazla bilgi için okuma [Service Fabric ve kapsayıcıları](service-fabric-containers-overview.md).

**Bölüm düzeni**: adlandırılmış bir hizmet oluşturduğunuzda, bir bölüm düzeni belirtin. Durum önemli miktarda hizmetleriyle durumu küme düğümleri arasında yayar bölümleri arasında verileri bölün. Bölümler veri bölerek adlandırılmış hizmetinizin durumu ölçeklendirebilirsiniz. Durum bilgisi olan hizmetler adlı sahip olurken çoğaltmaları bir bölüm içinde örnekleri, durum bilgisiz adlandırılmış Hizmetleri sahiptir. Genellikle, hiçbir iç durum sahip oldukları için yalnızca bir bölüm, durum bilgisiz adlandırılmış Hizmetleri sahiptir. Bölüm örnekleri için kullanılabilirlik sağlar. Bir örnek başarısız olursa, diğer örnekleri normal şekilde çalışmaya devam eder ve daha sonra Service Fabric yeni bir örneğini oluşturur. Durum bilgisi olan hizmetler adlı korumak durumu eşitlenmiş tutulur, bu nedenle, kendi çoğaltma durumlarına çoğaltmaları ve her bölüm içinde vardır. Bir çoğaltma işlemi başarısız olursa, Service Fabric mevcut çoğaltmaların yeni bir kopya oluşturur.

Okuma [bölüm Service Fabric güvenilir hizmetler](service-fabric-concepts-partitioning.md) daha fazla bilgi için makalenin.

## <a name="system-services"></a>Sistem Hizmetleri
Service Fabric platform özelliklerini sağlayan her kümede oluşturulan sistem hizmetleri vardır.

**Adlandırma Hizmeti**: her Service Fabric kümesi, bir adlandırma, küme içindeki bir konuma hizmet adlarını çözümlenen hizmeti, sahiptir. Hizmet adları ve bir internet gibi özellikleri yönetmenize küme için etki alanı adı sistemi (DNS). İstemcileri güvenli kümedeki herhangi bir düğümün sahip bir hizmet adı ve konumu çözümlemek için adlandırma hizmeti kullanarak iletişim kurar. Uygulamalar küme içinde taşıyın. Örneğin, bu hataları, kaynak Dengeleme veya kümesini yeniden boyutlandırma nedeniyle olabilir. Hizmetler ve geçerli ağ konumu çözümlemek istemcileri geliştirebilirsiniz. İstemciler gerçek Makine IP adresi ve bağlantı noktası şu anda çalıştığı edinin.

Okuma [Hizmetleri ile iletişim kurun](service-fabric-connect-and-communicate-with-services.md) hizmet ve istemci iletişimi adlandırma hizmeti ile çalışacak API'ler hakkında daha fazla bilgi için.

**Görüntü Deposu hizmetini**: her Service Fabric kümesi dağıtılan, sürümlü uygulama paketleri tutulduğu bir görüntü Deposu hizmetini sahiptir. Bir uygulama paketi görüntü deposuna kopyalama ve uygulama paket içinde bulunan uygulama türünün kaydedin. Uygulama türü sağlandıktan sonra adlandırılmış uygulama ondan oluşturun. Tüm adlandırılmış uygulamaları sildikten sonra Image Store hizmetinden uygulama türü kaydını kaldırabilirsiniz.

Okuma [ImageStoreConnectionString ayarı anlamak](service-fabric-image-store-connection-string.md) Image Store hizmeti hakkında daha fazla bilgi için.

Okuma [bir uygulamayı dağıtmak](service-fabric-deploy-remove-applications.md) Image Store hizmet uygulamalarını dağıtma hakkında daha fazla bilgi için makalenin.

**Yük Devretme Yöneticisi hizmeti**: her Service Fabric kümesi için aşağıdaki eylemleri sorumlu olduğu bir Yük Devretme Yöneticisi hizmeti sahiptir:
   - Yüksek kullanılabilirlik ve Hizmetleri tutarlılığını ilgili işlevleri gerçekleştirir.
   - Uygulama ve küme yükseltmeleri düzenler.
   - Diğer sistem bileşenlerini ile etkileşime girer.

**Yöneticisi hizmetini Onar**: automatable ve saydam bir şekilde güvenli, küme üzerinde gerçekleştirilecek eylemleri onarım izin veren bir isteğe bağlı sistem hizmeti budur. Onarım Yöneticisi kullanılır:
   - Azure bakım gerçekleştirme onarır [Gümüş ve altın dayanıklılık](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) Azure Service Fabric kümeleri.
   - Onarım eylemleri gerçekleştirmesini [düzeltme eki düzenleme uygulama](service-fabric-patch-orchestration-application.md)

## <a name="built-in-programming-models"></a>Yerleşik programlama modelleri
Service Fabric hizmetleri oluşturmak için kullanılabilen .NET Framework ve Java programlama modeli vardır:

**Güvenilir hizmetler**: durum bilgisiz ve durum bilgisi olan hizmetler oluşturmak için bir API. Durum bilgisi olan hizmetler durumlarına bir sözlük veya bir kuyruk gibi güvenilir koleksiyonlarında depolar. Web API ve Windows Communication Foundation (WCF) gibi çeşitli iletişim yığınlardaki ekleyebilirsiniz.

**Güvenilir aktörler**: sanal aktör programlama modeli aracılığıyla durum bilgisiz ve durum bilgisi olan nesneleri oluşturmak için bir API. Bu model, çok sayıda hesaplama veya durumunu bağımsız bir birim olduğunda yararlıdır. Tüm giden istekleri tamamlanana kadar tek bir aktör diğer gelen istekleri işleyemiyor çünkü diğer aktörler veya hizmetleri çağıran kodu önlemek en iyisidir Bu model bir Aç tabanlı iş parçacığı modeli kullanır.

Service Fabric mevcut uygulamalarınızı da çalıştırabilirsiniz:

**Kapsayıcıları**: Service Fabric Hyper-V yalıtım modunu desteğinin yanı sıra Windows Server 2016, Linux ve Windows Server kapsayıcılarında Docker kapsayıcıları dağıtımını destekler. Service Fabric içinde [uygulama modeli](service-fabric-application-model.md), hangi birden çok çoğaltmaları hizmete bir uygulama konağı bir kapsayıcıyı temsil eder. Service Fabric herhangi bir kapsayıcıdaki çalıştırabilir ve kapsayıcı içinde var olan bir uygulama paketi burada Konuk yürütülebilir senaryosu benzer bir senaryodur. Buna ek olarak, şunları yapabilirsiniz [kapsayıcılar içinde Service Fabric hizmetlerini çalıştırmak](service-fabric-services-inside-containers.md) de.

**Konuk yürütülebilir dosyalar**: hizmet olarak Azure Service Fabric kod, Node.js, Java ya da C++ gibi herhangi bir türde çalıştırabilirsiniz. Service Fabric Hizmetleri bu tür için durum bilgisi olmayan hizmetler davranılır Konuk yürütülebilir dosyalar olarak gösterir. Bir Service Fabric kümesi yürütülebilir Konuk çalıştırmak için yüksek kullanılabilirlik, sistem durumu izleme, uygulama yaşam döngüsü yönetimi, yüksek yoğunluklu ve bulunabilirlik sayılabilir.

Okuma [hizmetiniz için programlama modelini seçin](service-fabric-choose-framework.md) daha fazla bilgi için makalenin.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar
Service Fabric hakkında daha fazla bilgi için:

* [Service Fabric genel bakış](service-fabric-overview.md)
* [Uygulamaları oluşturmak için neden mikro hizmetler yaklaşımı öneriliyor?](service-fabric-overview-microservices.md)
* [Uygulama senaryoları](service-fabric-application-scenarios.md)


