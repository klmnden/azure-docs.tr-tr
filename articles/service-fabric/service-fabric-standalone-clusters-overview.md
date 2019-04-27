---
title: Tek başına Service Fabric kümeleri genel bakış | Microsoft Docs
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
ms.openlocfilehash: b4b7759d1dc23c1a1b3a9b5aeb2a181923e14d40
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60543745"
---
# <a name="overview-of-service-fabric-standalone-clusters"></a>Service Fabric tek başına kümeler genel bakış

Service Fabric kümesi bir ağa bağlı, mikro hizmetlerin dağıtıldığı ve yönetildiği sanal veya fiziksel makine kümesidir. Bir makine ya da bir kümenin parçası olan sanal makine bir küme düğümü adı verilir. Kümeler binlerce düğümde için ölçeklendirme yapabilir. Kümeye yeni düğümler eklerseniz, Service Fabric örnekleri ve hizmet bölüm çoğaltmaları sayısının artması düğümleri arasında yeniden dengeler. Genel uygulama performansını artıran ve bellek erişim çekişmesini azaltır. Kümedeki düğümler verimli bir şekilde kullanılmayan, kümedeki düğümlerin sayısını azaltabilirsiniz. Service Fabric yeniden örnekleri ve bölüm çoğaltmalarını azalan her düğümde donanım daha iyi kullanabilmesine için düğüm sayısını arasında yeniden dengeler.

Düğüm türü, kümedeki boyutunu, sayı ve bir dizi düğümü için özellikleri tanımlar. Daha sonra, her düğüm türünün ölçeği birbirinden bağımsız olarak artırılabilir veya azaltılabilir, her düğüm türünde farklı bağlantı noktası kümeleri açık olabilir ve farklı kapasite ölçümleri yapılabilir. Düğüm türleri, bir düğüm kümesinin "ön uç" veya "arka uç" şeklindeki rolünün tanımlanması için kullanılır. Kümenizde birden çok düğüm türü olabilir, ancak üretim kümeleri için birincil düğüm türünde en az beş VM (veya test kümeleri için en az üç VM) olmalıdır. [Service Fabric sistem hizmetleri](service-fabric-technical-overview.md#system-services), birincil düğüm türündeki düğümlere yerleştirilir.

Şirket içi bir Service Fabric kümesi oluşturma işlemi, bir VM kümesi ile kendi tercih ettiğiniz herhangi bir bulut üzerindeki bir küme oluşturma işlemi benzerdir. Vm'leri sağlama işlemi ilk adımlarında, kullanmakta olduğunuz şirket içi ortam ve bulut sağlayıcısı tarafından yönetilir. Bunlar arasında etkin ağ bağlantısı ile bir VM kümesi oluşturduktan sonra sonra adımları, Service Fabric paketi küme ayarları düzenleyin ve küme oluşturma çalıştırın ve yönetim komut dosyaları aynıdır. Bu deneyimi ve Service Fabric kümeleri yönetmek ve Bilgi Bankası aktarılamaz, yeni barındırma ortamları hedeflemek seçtiğinizde sağlar.

## <a name="cluster-security"></a>Küme güvenliği
Bir Service Fabric kümesine sahip olduğunuz bir kaynaktır.  Yetkisiz kullanıcıların bunlara bağlanmasını önlemeye yardımcı olmak için kümeleri güvenli hale getirmek için sizin sorumluluğunuzdur. Üretim iş yükleri küme üzerinde çalışan, güvenli bir kümeye özellikle önemlidir.

### <a name="node-to-node-security"></a>Düğümler için güvenlik
Düğümden düğüme güvenlik VM'ler veya bir küme içindeki bilgisayarları arasındaki iletişimi korur. Bu güvenlik senaryo, kümeye katılmak için yetkili bilgisayar uygulamaları ve Hizmetleri kümedeki barındırma katılabilir sağlar. Service Fabric küme güvenliğini sağlama ve uygulama güvenlik özellikler sağlamak için X.509 sertifikaları kullanır.  Küme trafiği güvenli hale getirme ve küme ve sunucu kimlik doğrulaması sağlamak için bir küme sertifikası gereklidir.  Kendinden imzalı sertifikaları test kümeleri için kullanılabilir, ancak üretim kümeleri güvenli hale getirmek için bir güvenilen sertifika yetkilisinden bir sertifika kullanılmalıdır.

Windows tek başına küme için Windows Güvenlik etkinleştirilebilir. Windows Server 2012 R2 ve Windows Active Directory varsa, Grup yönetilen hizmet hesapları ile Windows Güvenlik'i kullanmanızı öneririz. Aksi takdirde, Windows Güvenlik ile Windows hesapları kullanın.

Daha fazla bilgi için okuma [düğümler-güvenlik](service-fabric-cluster-security.md#node-to-node-security)

### <a name="client-to-node-security"></a>İstemci düğümü güvenlik
İstemci düğümü güvenlik, istemcilerin kimliğini doğrular ve bir istemci ve kümedeki tek tek düğümler arasında güvenli iletişim yardımcı olur. Bu tür bir güvenlik, küme ve kümede dağıtılan uygulamalar yalnızca yetkili kullanıcıların erişebildiğinden emin olun yardımcı olur. İstemcileridir benzersiz olarak aracılığıyla X.509 sertifikası güvenlik kimlik bilgilerini tanımlanmış. Herhangi bir sayıda isteğe bağlı istemci sertifikaları, küme yönetici veya kullanıcı istemcilerin kimliğini doğrulamak için kullanılabilir.

Ek olarak istemci sertifikaları, Azure Active Directory küme istemcilerin kimliğini doğrulamak için de yapılandırılabilir.

Daha fazla bilgi için okuma [istemci düğümü güvenlik](service-fabric-cluster-security.md#client-to-node-security)

### <a name="role-based-access-control-rbac"></a>Rol Tabanlı Erişim Denetimi (RBAC)
Service Fabric Ayrıca, belirli küme işlemleri farklı kullanıcı grupları için erişimi sınırlandırmak için erişim denetimi destekler. Bu, küme daha güvenli olmasına yardımcı olur. İstemciler bir kümeye bağlanmak için iki erişim denetim türleri desteklenir: Yönetici rolü ve kullanıcı rolü.  

Daha fazla bilgi için okuma [rol tabanlı erişim denetimi (RBAC)](service-fabric-cluster-security.md#role-based-access-control-rbac).

## <a name="scaling"></a>Ölçeklendirme

Uygulama talepleri zamanla değişir. Daha yüksek uygulama iş yükü veya ağ trafiği karşılamak ya da küme kaynaklarını talep düştüğünde azaltmak için küme kaynaklarını artırmam gerekiyor. Service Fabric kümesi oluşturduktan sonra küme yatay yönde ölçeklendirebilirsiniz (düğüm sayısını değiştirme) ya da dikey yönde (düğümlerin kaynakları değiştirin). Kümedeki herhangi bir zamanda iş yükleri küme üzerinde çalışırken bile ölçeklendirebilirsiniz. Küme ölçekler gibi uygulamalarınızı otomatik olarak da ölçeklendirin.

Daha fazla bilgi için okuma [tek başına kümeler ölçeklendirme](service-fabric-cluster-scaling-standalone.md).

## <a name="upgrading"></a>Yükseltiliyor

Tek başına küme bir kaynaktır, tamamen kendi. Temel işletim sistemi düzeltme eki uygulama ve yapı yükseltmeleri başlatmaktan sorumlu olursunuz. Microsoft yeni bir sürümü yayımlandığında otomatik çalışma zamanını yükseltme, alma, kümesi veya istediğiniz bir desteklenen çalışma zamanı sürümü seçmek seçim yapabilirsiniz. Yapı yükseltmeleri ek olarak, işletim sistemi düzeltme eki ve sertifikaları ya da uygulama bağlantı noktaları gibi küme yapılandırmasını güncelleştirin. 

Daha fazla bilgi için okuma [tek başına kümeler yükseltme](service-fabric-cluster-upgrade-standalone.md).

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri
Vm'leri veya (Linux henüz desteklenmiyor) bu işletim sistemlerini çalıştıran bilgisayarlara kümeleri oluşturmak kullanabilirsiniz:

* Windows Server 2012 R2
* Windows Server 2016 

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [güvenli hale getirme](service-fabric-cluster-security.md), [ölçeklendirme](service-fabric-cluster-scaling-standalone.md), ve [yükseltme](service-fabric-cluster-upgrade-standalone.md) tek başına kümeler.

Hakkında bilgi edinin [Service Fabric destek seçenekleri](service-fabric-support.md).