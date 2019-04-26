---
title: Windows Server ve Linux üzerinde Azure Service Fabric kümeleri oluşturma | Microsoft Docs
description: Windows Server ve Linux, yani sizin dağıtmayı mümkün olacaktır ve konak Service Fabric uygulamaları her yerde çalışan Service Fabric kümeleri, Windows Server veya Linux çalıştırabilirsiniz.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/01/2019
ms.author: dekapur
ms.openlocfilehash: d1681aee9dc11f0dbd3133bced0b919a8c1623b8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60310930"
---
# <a name="overview-of-service-fabric-clusters-on-azure"></a>Azure'da Service Fabric'e genel bakış kümeleri
Service Fabric kümesi bir ağa bağlı, mikro hizmetlerin dağıtıldığı ve yönetildiği sanal veya fiziksel makine kümesidir. Bir makine ya da bir kümenin parçası olan sanal makine bir küme düğümü adı verilir. Kümeler binlerce düğümde için ölçeklendirme yapabilir. Kümeye yeni düğümler eklerseniz, Service Fabric örnekleri ve hizmet bölüm çoğaltmaları sayısının artması düğümleri arasında yeniden dengeler. Genel uygulama performansını artıran ve bellek erişim çekişmesini azaltır. Kümedeki düğümler verimli bir şekilde kullanılmayan, kümedeki düğümlerin sayısını azaltabilirsiniz. Service Fabric yeniden örnekleri ve bölüm çoğaltmalarını azalan her düğümde donanım daha iyi kullanabilmesine için düğüm sayısını arasında yeniden dengeler.

Düğüm türü, kümedeki boyutunu, sayı ve bir dizi düğümü (sanal makineler) için özellikleri tanımlar. Daha sonra, her düğüm türünün ölçeği birbirinden bağımsız olarak artırılabilir veya azaltılabilir, her düğüm türünde farklı bağlantı noktası kümeleri açık olabilir ve farklı kapasite ölçümleri yapılabilir. Düğüm türleri, bir düğüm kümesinin "ön uç" veya "arka uç" şeklindeki rolünün tanımlanması için kullanılır. Kümenizde birden çok düğüm türü olabilir, ancak üretim kümeleri için birincil düğüm türünde en az beş VM (veya test kümeleri için en az üç VM) olmalıdır. [Service Fabric sistem hizmetleri](service-fabric-technical-overview.md#system-services), birincil düğüm türündeki düğümlere yerleştirilir. 

## <a name="cluster-components-and-resources"></a>Küme bileşenleri ve kaynaklar
Azure'da bir Service Fabric kümesi kullanan ve etkileşime giren bir Azure kaynağı diğer Azure kaynaklarıyla verilmiştir:
* VM'ler ve sanal ağ kartları
* sanal makine ölçek kümeleri
* sanal ağlar
* yük dengeleyiciler
* depolama hesabı
* genel IP adresleri

![Service Fabric Kümesi][Image]

### <a name="virtual-machine"></a>Sanal makine
A [sanal makine](/azure/virtual-machines/) bir küme düğümü teknik olarak, bir Service Fabric çalışma zamanı işleminde olsa da bu bir kümenin parçası vm'lere düğüm denir. Her düğüme bir düğüm adı (bir dize) atanır. Düğüm varsa, özellikleri gibi [yerleşim özellikleri](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints). Her bir makine veya sanal makine bir otomatik başlatılan hizmet sahip *FabricHost.exe*, önyükleme sırasında çalışmasını başlar ve ardından iki yürütülebilir dosyaları, başlatır *Fabric.exe* ve *FabricGateway.exe* , düğümü olun. Bir üretim dağıtımı, fiziksel veya sanal makine başına bir düğümdür. Senaryolarını test etmek için tek bir makine veya sanal makine üzerinde birden fazla düğüm birden çok örneğini çalıştırarak barındırabilirsiniz *Fabric.exe* ve *FabricGateway.exe*.

Her bir VM sanal ağ arabirim kartı (NIC) ile ilişkilendirilir ve her NIC, özel bir IP adresi atanır.  Bir VM sanal ağ ve yerel dengeleyici NIC üzerinden atanır

Bir kümedeki tüm sanal makinelerin sanal ağ içinde yer alır.  Aynı düğüm türü/ölçek kümesindeki tüm düğümler aynı alt ağda sanal ağa yerleştirilir.  Bu düğümler, yalnızca özel IP adresleri ve sanal ağ dışında doğrudan adreslenebilir değildir.  İstemciler Hizmetleri düğümlerinde Azure yük dengeleyici üzerinden erişebilirsiniz.

### <a name="scale-setnode-type"></a>Ölçek kümesi/düğüm türü
Bir küme oluştururken, bir veya daha fazla düğüm türleri tanımlar.  Düğümü veya sanal makineleri, bir düğüm türü aynı büyüklük ve sayısı, CPU, bellek, diskler ve disk g/ç sayısı gibi özelliklere sahip.  Örneğin, veri işlem büyük, arka uç VM'ler için başka bir düğüm türü olabilir ancak küçük, ön uç bağlantı noktalarını Vm'lerle internet'e açık bir düğüm türü olabilir. Azure kümelerinde, her düğüm türü için eşlenmiş bir [sanal makine ölçek kümesi](/azure/virtual-machine-scale-sets/).

Ölçek kümeleri, dağıtmak ve sanal makine koleksiyonunu bir küme olarak yönetmek için kullanabilirsiniz. Bir Azure Service Fabric kümesinde tanımladığınız her düğüm türü ayrı bir ölçek kümesi ayarlar. Service Fabric çalışma zamanı, Azure VM uzantıları kullanılarak ölçek kümesindeki her sanal makinede oturum önyüklenemez. Bağımsız olarak her düğüm türünün ölçeği artırın veya azaltın, her küme düğümünde çalışan işletim sistemi SKU'su değiştirme farklı bağlantı noktası kümeleri açık olan ve farklı kapasite ölçümleri kullanın. Bir ölçek kümesi beş sahip [yükseltme etki alanlarında](service-fabric-cluster-resource-manager-cluster-description.md#upgrade-domains) ve beş [hata etki alanları](service-fabric-cluster-resource-manager-cluster-description.md#fault-domains) ve en fazla 100 VM'ye sahip olabilir.  100'den fazla düğüm kümeleri, birden çok ölçek kümeleri/düğüm türleri oluşturarak oluşturun.

> [!IMPORTANT]
> Düğüm türleri kümenizi için sayısı ve her düğüm türünün (boyut, birincil, internet'e yönelik, VM'ler, vb. sayısı) özelliklerini seçme önemli bir görevdir.  Daha fazla bilgi için okuma [küme kapasite planlaması konuları](service-fabric-cluster-capacity.md).

Daha fazla bilgi için okuma [Service Fabric düğüm türleri ve sanal makine ölçek kümeleri](service-fabric-cluster-nodetypes.md).

### <a name="azure-load-balancer"></a>Azure Load Balancer
Sanal makine örnekleri arkasında birleştirilir bir [Azure yük dengeleyici](/azure/load-balancer/load-balancer-overview), ile ilişkili bir [genel IP adresi](/azure/virtual-network/virtual-network-ip-addresses-overview-arm#public-ip-addresses) ve DNS etiketi.  Sağlama zaman bir kümeyle  *&lt;clustername&gt;*, DNS adı  *&lt;clustername&gt;.&lt; Konum&gt;. cloudapp.azure.com* ölçek kümesinin önündeki yük dengeleyici ile ilişkili DNS etiketi.

Bir kümedeki sanal makinelerin yalnızca sahip [özel IP adresleri](/azure/virtual-network/virtual-network-ip-addresses-overview-arm#private-ip-addresses).  Yönetim ve hizmet trafiği, genel kullanıma yönelik Yük Dengeleyici üzerinden yönlendirilir.  Ağ trafiği, bu makinelere NAT kuralları (istemcilerin bağlanmak için belirli düğümler/örnek) veya Yük Dengeleme kuralları aracılığıyla yönlendirilir (trafiği Vm'lere gider hepsini bir kez deneme).  Bir yük dengeleyici DNS adı ile ilişkili bir genel IP bir biçime sahip:  *&lt;clustername&gt;.&lt; Konum&gt;. cloudapp.azure.com*.  Bir genel IP başka bir Azure kaynak grubunda bir kaynaktır.  Bir kümede birden çok düğüm türleri tanımlarsanız, bir yük dengeleyici için her düğüm türü/ölçek kümesi oluşturulur. Alternatif olarak, tek bir yük dengeleyici birden çok düğüm türleri için ayarlayabilirsiniz.  DNS etiketini birincil düğüm türünde  *&lt;clustername&gt;.&lt; Konum&gt;. cloudapp.azure.com*, diğer düğüm türleri DNS etiketine sahip  *&lt;clustername&gt;-&lt;nodetype&gt;.&lt; Konum&gt;. cloudapp.azure.com*.

### <a name="storage-accounts"></a>Depolama hesapları
Her küme düğümü türü tarafından desteklenen bir [Azure depolama hesabı](/azure/storage/common/storage-introduction) ve yönetilen diskler.

## <a name="cluster-security"></a>Küme güvenliği
Bir Service Fabric kümesine sahip olduğunuz bir kaynaktır.  Yetkisiz kullanıcıların bunlara bağlanmasını önlemeye yardımcı olmak için kümeleri güvenli hale getirmek için sizin sorumluluğunuzdur. Üretim iş yükleri küme üzerinde çalışan, güvenli bir kümeye özellikle önemlidir. 

### <a name="node-to-node-security"></a>Düğümler için güvenlik
Düğümden düğüme güvenlik VM'ler veya bir küme içindeki bilgisayarları arasındaki iletişimi korur. Bu güvenlik senaryo, kümeye katılmak için yetkili bilgisayar uygulamaları ve Hizmetleri kümedeki barındırma katılabilir sağlar. Service Fabric küme güvenliğini sağlama ve uygulama güvenlik özellikler sağlamak için X.509 sertifikaları kullanır.  Küme trafiği güvenli hale getirme ve küme ve sunucu kimlik doğrulaması sağlamak için bir küme sertifikası gereklidir.  Kendinden imzalı sertifikaları test kümeleri için kullanılabilir, ancak üretim kümeleri güvenli hale getirmek için bir güvenilen sertifika yetkilisinden bir sertifika kullanılmalıdır.

Daha fazla bilgi için okuma [düğümler-güvenlik](service-fabric-cluster-security.md#node-to-node-security)

### <a name="client-to-node-security"></a>İstemci düğümü güvenlik
İstemci düğümü güvenlik, istemcilerin kimliğini doğrular ve bir istemci ve kümedeki tek tek düğümler arasında güvenli iletişim yardımcı olur. Bu tür bir güvenlik, küme ve kümede dağıtılan uygulamalar yalnızca yetkili kullanıcıların erişebildiğinden emin olun yardımcı olur. İstemcileridir benzersiz olarak aracılığıyla X.509 sertifikası güvenlik kimlik bilgilerini tanımlanmış. Herhangi bir sayıda isteğe bağlı istemci sertifikaları, küme yönetici veya kullanıcı istemcilerin kimliğini doğrulamak için kullanılabilir.

Ek olarak istemci sertifikaları, Azure Active Directory küme istemcilerin kimliğini doğrulamak için de yapılandırılabilir.

Daha fazla bilgi için okuma [istemci düğümü güvenlik](service-fabric-cluster-security.md#client-to-node-security)

### <a name="role-based-access-control"></a>Rol Tabanlı Access Control
Rol tabanlı erişim denetimi (RBAC), Azure kaynakları üzerinde ayrıntılı erişim denetimleri atamak sağlar.  Abonelikler, kaynak grupları ve kaynaklar için farklı erişim kuralları atayabilirsiniz.  RBAC kuralları daha düşük bir düzeyde geçersiz kılınmadığı sürece kaynak hiyerarşi devralınır.  Herhangi bir kullanıcı veya kullanıcı grupları, AAD ile RBAC kurallar üzerinde böylece kullanıcılar atanan atayabilirsiniz ve kümenize gruplarını değiştirebilirsiniz.  Daha fazla bilgi için okuma [Azure RBAC genel bakış](/azure/role-based-access-control/overview).

Service Fabric Ayrıca, belirli küme işlemleri farklı kullanıcı grupları için erişimi sınırlandırmak için erişim denetimi destekler. Bu, küme daha güvenli olmasına yardımcı olur. İstemciler bir kümeye bağlanmak için iki erişim denetim türleri desteklenir: Yönetici rolü ve kullanıcı rolü.  

Daha fazla bilgi için okuma [Service Fabric Role-Based erişim denetimi (RBAC)](service-fabric-cluster-security.md#role-based-access-control-rbac).

### <a name="network-security-groups"></a>Ağ güvenlik grupları 
Ağ güvenlik grupları (Nsg'ler) bir alt ağ, VM veya belirli bir NIC gelen ve giden trafiği denetleme  Aynı sanal ağda birden çok VM yerleştirdiğinizde varsayılan olarak, bunlar birbiriyle herhangi bir bağlantı noktası iletişim kurabilir.  Makineler arasındaki iletişimi kısıtlamak istiyorsanız, ağ ayırmak veya sanal makineleri birbirinden yalıtmak için Nsg'leri tanımlayabilirsiniz.  Bir kümede birden çok düğüm türü varsa, birbirleriyle iletişim kurmak için farklı bir düğüme türlerinin ait makineler önlemek için alt ağ için Nsg uygulayabilirsiniz.  

Hakkında daha fazla bilgi için okuma [güvenlik grupları](/azure/virtual-network/security-overview)

## <a name="scaling"></a>Ölçeklendirme

Uygulama talepleri zamanla değişir. Daha yüksek uygulama iş yükü veya ağ trafiği karşılamak ya da küme kaynaklarını talep düştüğünde azaltmak için küme kaynaklarını artırmam gerekiyor. Service Fabric kümesi oluşturduktan sonra küme yatay yönde ölçeklendirebilirsiniz (düğüm sayısını değiştirme) ya da dikey yönde (düğümlerin kaynakları değiştirin). Kümedeki herhangi bir zamanda iş yükleri küme üzerinde çalışırken bile ölçeklendirebilirsiniz. Küme ölçekler gibi uygulamalarınızı otomatik olarak da ölçeklendirin.

Daha fazla bilgi için okuma [Azure ölçekleme kümeleri](service-fabric-cluster-scaling.md).

## <a name="upgrading"></a>Yükseltiliyor
Bir Azure Service Fabric kümesine sahip, ancak kısmen Microsoft tarafından yönetilen bir kaynaktır. Microsoft, temel işletim sistemi düzeltme eki uygulama ve kümenizde Service Fabric çalışma zamanını yükseltme gerçekleştirme sorumludur. Microsoft yeni bir sürümü yayımlandığında otomatik çalışma zamanını yükseltme, alma, kümesi veya istediğiniz bir desteklenen çalışma zamanı sürümü seçmek seçim yapabilirsiniz. Çalışma zamanı yükseltmeleri ek olarak, küme yapılandırması sertifikaları ya da uygulama bağlantı noktaları gibi güncelleştirebilirsiniz.

Daha fazla bilgi için okuma [kümelerini yükseltme](service-fabric-cluster-upgrade.md).

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri
Bu işletim sistemlerini çalıştıran sanal makinelere kümeleri oluşturmak kullanabilirsiniz:

* Windows Server 2012 R2
* Windows Server 2016 
* Windows Server 1709
* Windows Server 1803
* Linux Ubuntu 16.04
* Red Hat Enterprise Linux 7.4 (Önizleme desteği)

> [!NOTE]
> Service Fabric'te Windows Server 1709 dağıtmaya karar verirseniz, (1), uzun süreli bakım dalı, sürüm gelecekte taşımanız gerekebilir ve (2) kapsayıcıları dağıtma, kapsayıcıları Windows Server 2016'da oluşturulmuş Windows Server üzerinde çalışmaz olmadığını unutmayın  1709 ve bunun tersi de geçerlidir (bunları dağıtmak için bunları yeniden oluşturmak gerekir).
>


## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [güvenli hale getirme](service-fabric-cluster-security.md), [ölçeklendirme](service-fabric-cluster-scaling.md), ve [yükseltme](service-fabric-cluster-upgrade.md) Azure kümeleri.

Hakkında bilgi edinin [Service Fabric destek seçenekleri](service-fabric-support.md).

[Image]: media/service-fabric-azure-clusters-overview/Cluster.PNG