---
title: Azure Service Fabric terminolojisi öğrenin | Microsoft Docs
description: Service Fabric terminolojisi bakış. Önemli terimleri ve kavramları ve belgeleri geri kalanında kullanılan terimler açıklanmaktadır.
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
ms.date: 09/17/2018
ms.author: ryanwi
ms.openlocfilehash: b670b767a631e453bd58069fd69720bd1ab7c20a
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "45983572"
---
# <a name="service-fabric-terminology-overview"></a>Service Fabric terminolojiye genel bakış
Azure Service Fabric; ölçeklenebilir ve güvenilir mikro hizmetleri paketlemeyi, dağıtmayı ve yönetmeyi kolaylaştırmayı sağlayan bir dağıtılmış sistemler platformudur.  Yapabilecekleriniz [konak Service Fabric kümelerinin her yerden](service-fabric-deploy-anywhere.md): Azure, bir şirket içi veri merkezinde veya tüm bulut sağlayıcıları.  Service Fabric, güç katan orchestrator [Azure Service Fabric Mesh](/azure/service-fabric-mesh). Kendi hizmetlerinizi yazmanızı ve birden çok ortam seçenekleri uygulamayı çalıştırmak hedef konumu seçin, herhangi bir çerçeveyi kullanabilirsiniz. Bu makalede belgelerinde kullanılan terimler anlamak için Service Fabric tarafından kullanılan terimler açıklanmaktadır.

Bu bölümde listelenen kavramları aşağıdaki Microsoft Virtual Academy videoları de ele alınmıştır: <a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965">temel kavramları</a>, <a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tlkI046yC_2906218965">tasarım zamanı kavramları</a>, ve <a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=x7CVH56yC_1406218965">çalışma zamanı kavramları</a>.

## <a name="infrastructure-concepts"></a>Altyapı kavramları
**Küme**: sanal veya fiziksel makinelere içine mikro hizmetlerin dağıtılıp yönetildiği, ağa bağlı kümesi.  Kümeler binlerce makine içerecek şekilde ölçeklendirilebilir.

**Düğüm**: bir makine ya da bir kümenin parçası olan VM olarak adlandırılan bir *düğüm*. Her düğüme bir düğüm adı (bir dize) atanır. Düğümlerin yerleşim özellikleri gibi özellikleri vardır. Otomatik başlatma Windows hizmeti, her bir makine ya da VM sahip `FabricHost.exe`, önyükleme sırasında çalışmaya başlar ve ardından iki yürütülebilir dosyaları başlatır: `Fabric.exe` ve `FabricGateway.exe`. Bu iki yürütülebilir dosyalar düğümü olun. Senaryolarını test etmek için tek bir makine veya sanal makine üzerinde birden fazla düğüm birden çok örneğini çalıştırarak barındırabilirsiniz `Fabric.exe` ve `FabricGateway.exe`.

## <a name="application-and-service-concepts"></a>Uygulama ve hizmeti kavramları

**Service Fabric Mesh uygulaması**: Service Fabric uygulamaları Mesh kaynak modeli (YAML ve JSON kaynak dosyaları) tarafından tanımlanır ve Service Fabric çalıştırıldığı herhangi bir ortama dağıtılabilir.

**Service Fabric yerel uygulaması**: Service Fabric yerel uygulamaları, yerel uygulama modeli tarafından (XML tabanlı bir uygulama ve hizmet bildirimleri) açıklanmıştır.  Service Fabric yerel uygulamaları, Service Fabric Mesh içinde çalıştırılamaz.

### <a name="service-fabric-mesh-application-concepts"></a>Service Fabric Mesh uygulaması kavramları

**Uygulama**: bir uygulama dağıtım, sürüm ve Mesh uygulaması ömrünü birimidir. Her uygulama örneği yaşam döngüsü bağımsız olarak yönetilebilir.  Uygulamalar, bir veya daha fazla hizmet kod paketleri ve ayarları oluşur. Azure kaynak modeli (RM) şemayı kullanarak bir uygulama ile tanımlanır.  Hizmetleri özellikleri RM şablonunda Uygulama kaynağı olarak açıklanmıştır.  Ağ ve uygulama tarafından kullanılan birimleri uygulama tarafından başvurulur.  Bir uygulama oluştururken, uygulama, hizmetler, ağ ve birimlerin Service Fabric kaynak modelini kullanarak modellenir.

**Hizmet**: bir uygulamada bir hizmeti temsil eden bir mikro hizmet ve eksiksiz ve tek başına bir işlevi gerçekleştirir. Her hizmet kodu paket ile ilişkili kapsayıcı görüntüsünü çalıştırmak için gereken her şeyi açıklayan bir veya daha fazla, kod paketleri oluşur.  Bir uygulama Hizmetleri'nde sayısı yukarı ve aşağı ölçeklendirilebilir.

**Ağ**: bir ağ kaynağına, uygulamalarınız için özel bir ağ oluşturur ve uygulamaları ya da ona başvuran Hizmetleri bağımsızdır. Birden çok farklı uygulamalar hizmetlerinden aynı ağın parçası olabilir. Ağları uygulamalar tarafından başvurulan dağıtılabilir kaynaklardır.

**Kod paketi**: kod paketleri aşağıdakiler dahil olmak üzere kod paket ile ilişkili kapsayıcı görüntüsünü çalıştırmak için gereken her şeyi açıklanmaktadır:

* Kapsayıcı adı, sürümü ve kayıt defteri
* Her kapsayıcı için gerekli CPU ve bellek kaynakları
* Ağ uç noktaları
* Ayrı birim kaynağına başvuran kapsayıcısında bağlamak için birimler.

Uygulama kaynağı bir parçası olarak tanımlanan tüm kod paketlerinin dağıtıldığı ve bir grup olarak birlikte etkinleştirildi.

**Birim**: birimlerdir durumu kalıcı hale getirmek için kullanabileceğiniz kapsayıcı örneklerinizin içinde oluşturulmuş dizinleri. Azure dosyaları birim sürücüsü bir kapsayıcıya Azure dosyaları paylaşımına bağlar ve dosya depolamayı destekleyen herhangi bir API aracılığıyla güvenilir veri depolama sağlar. Birimleri uygulamalar tarafından başvurulan dağıtılabilir kaynaklardır.

### <a name="service-fabric-native-application-concepts"></a>Service Fabric yerel uygulama kavramları

**Uygulama**: bir uygulama belirli bir işlev veya işlevler gerçekleştiren bağlı Hizmetleri koleksiyonudur. Her uygulama örneği yaşam döngüsü bağımsız olarak yönetilebilir.

**Hizmet**: bir hizmet tam ve tek başına bir işlevi gerçekleştirir ve başlatabilir ve diğer hizmetler bağımsız olarak çalışır. Bir hizmet, kod, yapılandırma ve veri kümesinden oluşur. Her hizmet için ikili dosyaları yürütülebilir kod oluşur, çalışma zamanında yüklenmesi gereken hizmet ayarlarını yapılandırma oluşur ve veri hizmeti tarafından kullanılacak rastgele bir statik veri oluşur.

**Uygulama türü**: hizmet türlerinin bir koleksiyona atanan adı/sürümü. İçinde tanımlanan bir `ApplicationManifest.xml` dosya ve uygulama paket dizini içinde gömülü. Dizin, ardından Service Fabric kümenin görüntü deposuna kopyalanır. Sonra küme içindeki bu uygulama türünden adlandırılmış bir uygulama da oluşturabilirsiniz.

Okuma [uygulama modeli](service-fabric-application-model.md) makale daha fazla bilgi için.

**Uygulama paketi**: uygulama türün içeren bir disk dizin `ApplicationManifest.xml` dosya. Uygulama türü yaptığı her bir hizmet türünün hizmet paketleri başvuruyor. Uygulama paketi dizinindeki dosyalar için Service Fabric kümenin görüntü deposuna kopyalanır. Örneğin, bir uygulama paketi e-posta uygulama türü için bir kuyruk hizmeti paketi, bir ön uç hizmeti paketi ve bir veritabanı hizmeti paket başvuruları içerebilir.

**Uygulama adında**: bir uygulama paketi görüntü deposuna kopyalamak, sonra kümedeki uygulamanın bir örneğini oluşturun. Adını veya sürüm kullanarak uygulama paket uygulama türü, belirttiğiniz bir örnek oluşturun. Her uygulama türü örneği şuna benzer bir Tekdüzen Kaynak Tanımlayıcısı (URI) adı atanır: `"fabric:/MyNamedApp"`. Bir küme içinde tek bir uygulama türünden birden fazla adlandırılmış uygulamalar oluşturabilirsiniz. Ayrıca, farklı uygulama türleri adlandırılmış uygulamalar oluşturabilirsiniz. Her adlandırılmış bağımsız olarak yönetilen ve tutulan uygulamasıdır.

**Hizmet türünü**: bir hizmetin kod paketleri, veri paketleri ve yapılandırma paketleri için atanan adı/sürümü. Hizmet türünün tanımlanan `ServiceManifest.xml` dosyası ve bir hizmet paketi dizinde katıştırılmış. Hizmet paketi dizini sonra bir uygulama paketin tarafından başvurulan `ApplicationManifest.xml` dosya. Küme içinde adlandırılmış bir uygulama oluşturduktan sonra adlandırılmış bir hizmet uygulaması türün hizmet türlerinden birini oluşturabilirsiniz. Hizmet türün `ServiceManifest.xml` dosya hizmeti açıklar.

Okuma [uygulama modeli](service-fabric-application-model.md) makale daha fazla bilgi için.

İki tür hizmet vardır:

* **Durum bilgisi olmayan**: hizmetin durumunu sürekli bir dış depolama hizmeti, Azure depolama, Azure SQL veritabanı ve Azure Cosmos DB gibi depolandığında durum bilgisi olmayan hizmet kullanın. Hiçbir kalıcı depolama alanı varsa, hizmet durum bilgisi olmayan hizmet kullanın. Örneğin, hizmete değerleri nerede geçirilen bir hesaplayıcı hizmeti için bu değerleri kullanan bir hesaplama gerçekleştirilir ve sonra bir sonuç döndürülür.
* **Durum bilgisi olan**:, güvenilir koleksiyonlar veya Reliable Actors programlama modeli aracılığıyla hizmetinizin durumu yönetmek için Service Fabric durum bilgisi olan bir hizmet kullanın. Adlandırılmış bir hizmet oluşturduğunuzda, kaç bölümleri ölçeklenebilirlik için istediğiniz üzerinden durumunuzu yaymak için belirtin. Ayrıca, güvenilirlik düğümlerde durumunuzu çoğaltmak için kaç kez belirtin. Her adlandırılmış bir hizmet, tek bir birincil çoğaltma ve birden fazla ikincil çoğaltma vardır. Birincil çoğaltmaya yazdığınızda, adlandırılmış hizmetinin durumunu değiştirin. Service Fabric, bu durum durumunuzu eşitlemek için tüm ikincil çoğaltmalara ardından çoğaltır. Birincil çoğaltma başarısız olur ve var olan bir ikincil çoğaltma birincil çoğaltmaya yükseltir Service Fabric otomatik olarak algılar. Service Fabric, ardından yeni bir ikincil çoğaltmaya oluşturur.  

**Çoğaltmalar veya örnekleri** kodu (ve durumu için durum bilgisi olan hizmetler) dağıtılan bir hizmetin bakın ve çalıştırma. Bkz: [çoğaltmalar ve örnekler](service-fabric-concepts-replica-lifecycle.md).

**Yeniden yapılandırma** herhangi bir değişiklik, bir hizmet çoğaltma kümesi işlemi ifade eder. Bkz: [yeniden yapılandırma](service-fabric-concepts-reconfiguration.md).

**Hizmet paketi**: hizmet türün içeren bir disk dizin `ServiceManifest.xml` dosya. Bu dosya, kod, statik veri ve hizmet türü için yapılandırma paketleri başvuruyor. Hizmet paketi dizindeki dosyaların uygulama türün tarafından başvurulan `ApplicationManifest.xml` dosya. Örneğin, bir hizmet paketi, kod, statik veri ve bir veritabanı hizmeti oluşturan yapılandırma paketlerini başvurabilir.

**Adlı hizmetin**: adlandırılmış bir uygulama oluşturduktan sonra küme içindeki hizmet türlerinden bir örneğini oluşturabilirsiniz. Hizmet türü, onun adı/sürümü kullanarak belirtin. Her hizmet türü örneği, adlandırılmış uygulama URI altında kapsamlı bir URI ad atanır. "Veritabanım adlı hizmetin"adlı uygulama MyNamedApp"içinde" oluşturursanız, örneğin, URI aşağıdaki gibi görünür: `"fabric:/MyNamedApp/MyDatabase"`. Adlandırılmış bir uygulama içinde birkaç adlandırılmış hizmeti oluşturabilirsiniz. Adlandırılmış her hizmetin kendi bölüm düzeni ve örnek olabilir veya çoğaltma sayar.

**Kod paketi**: hizmet türün yürütülebilir dosyaları, genellikle EXE/DLL dosyaları içeren bir disk dizin. Kod paketi dizindeki dosyaların hizmet türün tarafından başvurulan `ServiceManifest.xml` dosya. Adlandırılmış bir hizmet oluşturduğunuzda, kod paketi veya adlandırılmış hizmeti çalıştırmak için seçili düğümü için kopyalanır. Ardından kod çalışmaya başlar. Kod Paketi yürütülebilir dosyalar iki tür vardır:

* **Konuk tarafından yürütülebilir uygulama**: Farklı Çalıştır yürütülebilir dosyalar-ana bilgisayar işletim sistemi üzerinde (Windows veya Linux). Bu yürütülebilir dosyaları yok bağlamak veya herhangi bir Service Fabric çalışma zamanı dosyalarını başvurulacağını ve bu nedenle yoksa tüm Service Fabric programlama modellerini kullanın. Bu yürütülebilir dosyaları, adlandırma hizmeti için uç nokta bulma gibi bazı Service Fabric özellikleri kullanamaz. Konuk yürütülebilir dosyaları, her bir hizmet örneği için özel yükleme ölçümleri bildiremezsiniz.
* **Hizmet konak yürütülebilir dosyaları**: hizmet Service Fabric özelliklerini etkinleştirme Fabric Service Fabric çalışma zamanı dosyaları bağlayarak programlama modellerini kullanmak yürütülebilir. Örneğin, adlandırılmış hizmet örneğinde uç noktaları, Service Fabric'in adlandırma hizmeti ile kaydedebilir ve yükleme ölçümleri de bildirebilirsiniz.

**Veri paketi**: hizmet türün statik ve salt okunur veri dosyaları, genellikle fotoğraf, ses ve video dosyaları içeren bir disk dizin. Veri paketi dizini dosyalarında hizmet türün tarafından başvurulan `ServiceManifest.xml` dosya. Adlandırılmış bir hizmet oluşturduğunuzda, veri paketi veya adlandırılmış hizmeti çalıştırmak için seçili düğümü için kopyalanır. Kod çalışmaya başlar ve veri dosyalarını artık erişebilirsiniz.

**Yapılandırma paketi**: hizmet türün statik ve salt okunur yapılandırma dosyaları, genellikle metin içeren bir disk dizin. Yapılandırma paketi dizindeki dosyaların hizmet türün tarafından başvurulan `ServiceManifest.xml` dosya. Adlandırılmış bir hizmet oluşturduğunuzda, kopyalanan bir yapılandırma paketi dosyaları olan veya daha fazla düğüm söz konusu hizmet çalıştırmayı seçtiniz. Ardından kod, çalıştırın ve artık erişim yapılandırma dosyalarını başlatır.

**Kapsayıcıları**: varsayılan olarak, Service Fabric dağıtır ve hizmet işlemleri olarak etkinleştirir. Service Fabric, kapsayıcı görüntüleri Hizmetleri'nde olarak da dağıtabilirsiniz. Uygulamalardan temel işletim sistemini sanallaştırır bir sanallaştırma teknolojisini kapsayıcılardır. Bir uygulama ve onun çalışma zamanı, bağımlılıklar ve sistem kitaplıkları içinde bir kapsayıcı çalıştırın. Kapsayıcı tam, işletim sistemi yapıları yalıtılmış kapsayıcının kendi görünümünü özel erişimi var. Service Fabric, Linux ve Windows Server kapsayıcıları Docker kapsayıcılarını destekler. Daha fazla bilgi için okuma [Service Fabric ve kapsayıcılar](service-fabric-containers-overview.md).

**Bölüm düzeni**: adlandırılmış bir hizmet oluştururken bir bölüm düzeni belirtin. Önemli miktarda durumu hizmetleriyle, küme düğümleri arasında durum yayar, bölümler arasında verileri bölün. Bölümler arasında verileri bölerek adlandırılmış hizmetinizin durumu ölçeklendirebilirsiniz. Durum bilgisi olan hizmetler adlı sahip ise çoğaltmalar bir bölüm içinde adlandırılmış durum bilgisi olmayan hizmetler örnekleri vardır. Genellikle, sahip oldukları iç durumu olmadan adlandırılmış durum bilgisi olmayan hizmetler yalnızca tek bir bölümü vardır. Bölüm örnekler için kullanılabilirlik sağlar. Bir örnek başarısız olursa diğer örnekleri normal şekilde çalışmaya devam eder ve ardından Service Fabric yeni bir örneğini oluşturur. Durum bilgisi olan hizmetler adlı Bakımı durumu eşitlenmiş olarak tutulur, böylece kendi çoğaltma çoğaltmalar ve her bölüm içinde durumlarına sahiptir. Service Fabric, bir çoğaltması başarısız olursa, mevcut çoğaltmaları yeni bir kopya oluşturur.

Okuma [Partition Service Fabric güvenilir Hizmetleri](service-fabric-concepts-partitioning.md) makale daha fazla bilgi için.

## <a name="system-services"></a>Sistem Hizmetleri
Service Fabric platform yeteneklerini sağlayan her kümede oluşturulan sistem hizmetleri vardır.

**Adlandırma Hizmeti**: adlandırma, bir kümedeki bir konuma hizmet adlarını çözümleyen hizmeti, her bir Service Fabric kümesine sahiptir. Hizmet adları ve internet gibi özellikleri yönetmenize küme için etki alanı adı sistemi (DNS). İstemcileri güvenli bir şekilde kümedeki herhangi bir düğüm ile adlandırma hizmeti bir hizmet adı ve konumu çözümlemek için kullanarak iletişim kurar. Uygulamalar, küme içinde taşıyın. Örneğin, bu hatalar, kaynak Dengeleme veya yeniden boyutlandırma kümesi nedeniyle olabilir. Hizmetler ve geçerli ağ konumunu çözümleyen istemcileri geliştirebilirsiniz. İstemciler, gerçek Makine IP adresi ve bağlantı noktası şu anda çalıştığı edinin.

Okuma [hizmetleriyle iletişim](service-fabric-connect-and-communicate-with-services.md) istemci ve hizmet iletişimi adlandırma hizmeti ile çalışacak API'leri hakkında daha fazla bilgi için.

**Görüntü Store hizmet**: her bir Service Fabric kümesi dağıtılan, tutulan uygulama paketleri tutulduğu bir görüntü Store hizmeti vardır. Bir uygulama paketi için görüntü Store kopyalayın ve ardından uygulama paket içinde yer alan uygulama türünü kaydedin. Uygulama türü sağlandıktan sonra adlandırılmış bir uygulama oluşturun. Tüm adlandırılmış uygulamaları silindikten sonra uygulama türü görüntü Store hizmetinden kaydını kaldırabilirsiniz.

Okuma [Imagestoreconnectionstring ayarını anlama](service-fabric-image-store-connection-string.md) görüntü Store hizmeti hakkında daha fazla bilgi için.

Okuma [uygulama dağıtma](service-fabric-deploy-remove-applications.md) makale uygulamaları görüntü Store hizmetine dağıtma hakkında daha fazla bilgi.

**Yük Devretme Yöneticisi hizmeti**: her bir Service Fabric kümesi için aşağıdaki eylemleri sorumlu olan bir Yük Devretme Yöneticisi hizmeti vardır:
   - Yüksek kullanılabilirlik ve tutarlılık Hizmetleri ile ilgili işlevler gerçekleştirir.
   - Uygulama ve Küme yükseltme işlemlerini yönetir.
   - Diğer sistem bileşenlerle etkileşime girer.

**Onarım Yöneticisi hizmeti**: Bu otomatikleştirilebilen ve şeffaf bir şekilde güvenli bir kümeye gerçekleştirilecek eylemler onarım veren bir isteğe bağlı sistem hizmetidir. Onarım Yöneticisi kullanılır:
   - Azure bakım gerçekleştiriliyor onarır üzerinde [Silver ve Gold dayanıklılık](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) Azure Service Fabric kümeleri.
   - Onarım eylemleri için taşıma [düzeltme düzenleme uygulaması](service-fabric-patch-orchestration-application.md)

## <a name="deployment-and-application-models"></a>Dağıtım ve uygulama modelleri 

Hizmetlerinizi dağıtmak için nasıl çalıştırılacağını tanımlamak gerekir. Service Fabric, üç farklı dağıtım modelleri destekler:

### <a name="resource-model-preview"></a>Kaynak Modeli (Önizleme)
Service Fabric kaynakları, Service Fabric için ayrı ayrı dağıtılabilen herhangi bir şey olduğunu; uygulamalar, hizmetler, ağlar ve birimler dahil olmak üzere. Kaynakları bir küme uç noktasına dağıtılabilir bir JSON dosyası kullanılarak tanımlanır.  Service Fabric Mesh için Azure kaynak Model şeması kullanılır. Bir YAML dosyası şeması, daha kolay tanım dosyalarını yazmak için de kullanılabilir. Kaynakları, Service Fabric çalıştıran herhangi bir dağıtılabilir. Kaynak modeli, Service Fabric uygulamalarınızı açıklamak için basit bir yoldur. Kendi ana basit dağıtım ve kapsayıcılı hizmetlerin yönetimini sağlamaktır. Daha fazla bilgi edinmek için [Service Fabric kaynak modeli giriş](/azure/service-fabric-mesh/service-fabric-mesh-service-fabric-resources).

### <a name="native-model"></a>Yerel modeli
Yerel uygulama modeli için Service Fabric uygulamalarınızı tam alt düzey erişim sağlar. Uygulamalar ve hizmetler, XML bildirim dosyalarında kayıtlı türleri olarak tanımlanır.

Yerel modeli, Reliable Services ve Reliable Actors çerçeveleri ve küme yönetimi API'leri C# ve Java Service Fabric çalışma zamanı API erişimi sağlayan destekler. Yerel modeli rastgele kapsayıcıları ve yürütülebilir dosyalar da destekler. Yerel modeli desteklenmiyor [Service Fabric Mesh ortam](/azure/service-fabric-mesh/service-fabric-mesh-overview).

**Reliable Services**: durum bilgisiz ve durum bilgisi olan hizmetler oluşturmak için bir API. Durum bilgisi olan hizmetler durumlarına bir sözlük veya bir kuyruk gibi güvenilir koleksiyonlarında depolar. Web API ve Windows Communication Foundation (WCF) gibi çeşitli iletişim yığın takabilirsiniz.

**Reliable Actors**: sanal aktör programlama modeli aracılığıyla durum bilgisiz ve durum bilgisi olan nesneleri oluşturmak için bir API. Bu modeli, çok sayıda hesaplama veya durumunu bağımsız bir birim olduğunda yararlıdır. Bu model, tüm giden istekleri tamamlanana kadar diğer gelen istekleri tek tek bir aktör işleyemediği diğer aktörler veya hizmetlere çağıran kodu önlemek en iyi şekilde bir sırayla oynadıkları iş parçacığı modeli kullanır.

Service Fabric'te mevcut uygulamalarınızı da çalıştırabilirsiniz:

**Kapsayıcıları**: Service Fabric üzerinde Hyper-V yalıtım modu desteği ile birlikte Windows Server 2016'da, Linux ve Windows Server kapsayıcıları Docker kapsayıcı dağıtımını destekler. Service fabric'te [uygulama modeli](service-fabric-application-model.md), hangi birden çok çoğaltmaları hizmete bir uygulama konağı bir kapsayıcıyı temsil eder. Tüm kapsayıcıları Service Fabric çalıştırabilir ve bir kapsayıcı içinde var olan bir uygulama paketini burada Konuk yürütülebilir senaryosu benzer bir senaryodur. Ayrıca, aşağıdakileri yapabilirsiniz [kapsayıcıların içinde Service Fabric hizmetleri çalıştırmak](service-fabric-services-inside-containers.md) de.

**Konuk tarafından yürütülebilir uygulama**: kod, Node.js, Java veya C++ gibi herhangi bir türde bir hizmet olarak Azure Service fabric'te çalıştırabilirsiniz. Service Fabric durum bilgisi olmayan hizmetler kabul edilir Konuk yürütülebilir dosyaları olarak Hizmetleri bu türlere başvurur. Konuk yürütülebilir bir Service Fabric kümesinde çalışan sağladığı avantajlar arasında yüksek kullanılabilirlik, sistem durumu izleme, uygulama yaşam döngüsü yönetimi, yüksek yoğunluklu ve bulunabilirlik vardır.

Okuma [hizmetiniz için bir programlama modeli seçin](service-fabric-choose-framework.md) makale daha fazla bilgi için.

### <a name="docker-compose"></a>Docker Compose 
[Docker Compose](https://docs.docker.com/compose/) Docker projesindeki bir parçasıdır. Service Fabric için sınırlı destek sağlar [Docker Compose modelini kullanarak dağıtmaya](service-fabric-docker-compose.md).

## <a name="environments"></a>Ortamlar

Service Fabric birkaç farklı hizmet ve ürünleri temel alan bir açık kaynak platformu teknolojisidir. Microsoft aşağıdaki seçenekleri sağlar:

 - **Azure Service Fabric Mesh**: Microsoft Azure'da Service Fabric uygulamaları çalıştırmak için tam olarak yönetilen bir hizmet.
 - **Azure Service Fabric**: Azure'da barındırılan Service Fabric kümesi teklifi. Bu, Service Fabric ile birlikte Service Fabric Küme yükseltme ve yapılandırma yönetimi, Azure altyapı arasında tümleştirme sağlar.
 - **Service Fabric tek başına**: yükleme ve yapılandırma araçları kümesi [herhangi bir Service Fabric kümelerini dağıtmayı](/azure/service-fabric/service-fabric-deploy-anywhere) (şirket içinde veya diğer bulut sağlayıcılarına). Azure tarafından yönetilen değil.
 - **Service Fabric geliştirme kümesi**: Service Fabric uygulamaları geliştirme için Windows, Linux veya Mac üzerinde bir yerel geliştirme deneyimi sağlar.

## <a name="environment-framework-and-deployment-model-support-matrix"></a>Ortam, framework ve dağıtım modeline destek matrisi
Farklı ortamlar, farklı çerçeveler ve dağıtım modelleri için destek düzeyine sahip. Aşağıdaki tabloda, desteklenen çerçevesi ve dağıtım modeli birleşimlerini açıklar.

| Uygulama türü | Tarafından açıklanan | Azure Service Fabric Mesh | Azure Service Fabric kümeleri (herhangi bir işletim sistemi)| Yerel küme | Tek başına küme |
|---|---|---|---|---|---|---|---|---|---|
| Service Fabric kafes uygulamaları | Kaynak Modeli (YAML & JSON) | Desteklenen |Desteklenmiyor | Windows - desteklenir, Linux ve Mac desteklenmeyen | Desteklenen Windows değil |
|Service Fabric yerel uygulamaları | Yerel uygulama modelini (XML) | Desteklenmiyor| Desteklenen|Desteklenen|Windows-desteklenir|

Aşağıdaki tabloda, farklı uygulama modelleri ve bunlar için Service Fabric karşı var olan araçlar açıklanmaktadır.

| Uygulama türü | Tarafından açıklanan | Visual Studio | Eclipse | SFCTL | AZ CLI | PowerShell|
|---|---|---|---|---|---|---|---|---|---|
| Service Fabric kafes uygulamaları | Kaynak Modeli (YAML & JSON) | VS 2017 |Desteklenmiyor |Desteklenmiyor | Destekleniyor - yalnızca ağ ortamı | Desteklenmiyor|
|Service Fabric yerel uygulamaları | Yerel uygulama modelini (XML) | VS 2017 ve VS 2015| Desteklenen|Desteklenen|Desteklenen|Desteklenen|

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar
Service Fabric hakkında daha fazla bilgi için:

* [Service Fabric'e genel bakış](service-fabric-overview.md)
* [Uygulamaları oluşturmak için neden mikro hizmetler yaklaşımı öneriliyor?](service-fabric-overview-microservices.md)
* [Uygulama senaryoları](service-fabric-application-scenarios.md)

Service Fabric Mesh hakkında daha fazla bilgi için:

* [Service Fabric kafes genel bakış](/azure/service-fabric-mesh/service-fabric-mesh-overview)
