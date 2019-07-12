---
title: Kavramlar - Kubernetes için Azure Kubernetes Hizmetleri (AKS) hakkında temel bilgiler
description: Temel küme ve iş yükü bileşenlerinin Kubernetes ve özelliklere Azure Kubernetes Service (AKS) birbirleriyle öğrenin
services: container-service
author: mlearned
ms.service: container-service
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: mlearned
ms.openlocfilehash: 5f387310e737982b824d0ac9662822d9a74f39e9
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67616012"
---
# <a name="kubernetes-core-concepts-for-azure-kubernetes-service-aks"></a>Kubernetes kavramları Azure Kubernetes Service (AKS)

Uygulama geliştirme, kapsayıcı tabanlı bir yaklaşımda taşınırken, düzenlemek ve kaynakları yönetmek için gereken önemlidir. Kubernetes güvenilir hataya dayanıklı uygulama iş yüklerini zamanlama sağlama olanağı sunan önde gelen bir platformdur. Azure Kubernetes Service (AKS), daha fazla kapsayıcı tabanlı uygulama dağıtımını ve yönetimini basitleştirir sunan, yönetilen bir Kubernetes ' dir.

Bu makalede temel Kubernetes altyapı bileşenleri gibi tanıtılır *küme ana*, *düğümleri*, ve *düğüm havuzları*. İş yükü kaynakları gibi *pod'ların*, *dağıtımları*, ve *ayarlar* Ayrıca, Grup kaynakları nasıl birlikte sunulan *ad alanları*.

## <a name="what-is-kubernetes"></a>Kubernetes nedir?

Kubernetes kapsayıcı tabanlı uygulamalar ve ilişkili ağ ve depolama bileşenleri yöneten hızla gelişen bir platformdur. Uygulama iş yükleri üzerinde temel alınan altyapı bileşenlerini değil biridir. Kubernetes dağıtımı, yönetimi işlemleri için sağlam bir API kümesi tarafından desteklenen bildirim temelli bir yaklaşım sağlar.

Yapı ve işlemlerini ve bu uygulama bileşenlerinin kullanılabilirliğini yönetme Kubernetes yararlanan modern, taşınabilir, mikro hizmet tabanlı uygulamalar çalıştırın. Kubernetes mikro hizmet tabanlı uygulamaları benimsenmesini ilerlemeyi takımlar olarak durum bilgisiz ve durum bilgisi olan uygulamaları destekler.

Açık bir platform Kubernetes tercih edilen bir programlama dili, işletim sistemi, kitaplıkları veya ileti veri yolu ile uygulamalarınızı oluşturmanıza olanak tanır. Mevcut bir sürekli tümleştirme ve sürekli teslim (CI/CD) araçları, zamanlama ve sürümleri dağıtmak için Kubernetes ile tümleştirebilirsiniz.

Azure Kubernetes Service (AKS), yükseltme koordine dahil olmak üzere, dağıtım ve çekirdek yönetim görevleri için karmaşıklığı azaltan yönetilen bir Kubernetes hizmeti sağlar. AKS küme yöneticileri Azure platformu tarafından yönetilir ve yalnızca, uygulamalarınızı çalıştırmak AKS düğümleri için ücret ödersiniz. AKS, açık kaynaklı Azure Kubernetes Service altyapısı üzerine kurulmuştur ([aks altyapısı][aks-engine]).

## <a name="kubernetes-cluster-architecture"></a>Kubernetes kümesi mimarisi

Bir Kubernetes kümesi iki bileşene bölünür:

- *Küme Yöneticisi* düğümleri temel Kubernetes Hizmetleri ve uygulama iş yüklerinin düzenleme sağlar.
- *Düğümleri* , uygulama iş yüklerini çalıştırın.

![Kubernetes küme ana vm'lerde ve düğüm bileşenleri](media/concepts-clusters-workloads/cluster-master-and-nodes.png)

## <a name="cluster-master"></a>Küme Yöneticisi

Bir AKS kümesi oluşturduğunuzda, Küme Yöneticisi otomatik olarak oluşturulur ve yapılandırılır. Bu Küme Yöneticisi, kullanıcıdan soyutlanır yönetilen bir Azure kaynağı olarak sağlanır. AKS kümesi parçası olan düğümler için küme yöneticisinin hiçbir ücret yoktur.

Küme Yöneticisi, aşağıdaki temel Kubernetes bileşenleri içerir:

- *kube-apiserver* -API sunucusu, temel alınan Kubernetes API'ları nasıl sunulur. Bu bileşen etkileşimi için yönetim araçları gibi sağlar `kubectl` veya Kubernetes panosunu.
- *etcd* - Kubernetes kümesi ve yüksek oranda kullanılabilir yapılandırma durumunu korumak için *etcd* Kubernetes içinde bir anahtar değer deposu.
- *kube-Zamanlayıcı* - oluşturduğunuzda veya ölçekli uygulamalar, Zamanlayıcı hangi düğümleri, iş yükü çalışabileceğini belirler ve bunları başlatır.
- *kube Denetleyici Yöneticisi* -Denetleyici Yöneticisi, bir pod'ların çoğaltma ve düğüm işlemleri işleme gibi eylemler gerçekleştiren küçük denetleyicilerinin sayısını'yı gözetir.

AKS sağlayan bir tek kiracılı Küme Yöneticisi, bir API ile ayrılmış sunucusu, Zamanlayıcı vb. Sayısı ve düğümlerin boyutu tanımlayın ve Azure platformu küme ana vm'lerde ve düğüm arasında güvenli iletişim yapılandırır. Küme Yöneticisi ile etkileşimi gerçekleşir Kubernetes API'leri aracılığıyla gibi `kubectl` veya Kubernetes panosunu.

Bu yönetilen küme ana bileşenleri gibi yüksek oranda kullanılabilir yapılandırmanıza gerek yoktur anlamına gelir. *etcd* deposu, ancak aynı zamanda anlamına gelir küme asıl doğrudan erişemiyor. Yükseltme için Kubernetes küme asıl ve düğümler yükseltmeleri Azure portalı ve Azure CLI düzenlenir. Olası sorunları gidermek için Azure İzleyici günlüklerine Küme Yöneticisi Günlükleri gözden geçirebilirsiniz.

Küme Yöneticisi belirli bir şekilde yapılandırın veya bunlara doğrudan erişmesi gerekiyorsa kendi Kubernetes kümesi kullanarak dağıtabilirsiniz [aks altyapısı][aks-engine].

İlişkili en iyi yöntemler için bkz: [küme güvenliği ve AKS yükseltmeler için en iyi yöntemler][operator-best-practices-cluster-security].

## <a name="nodes-and-node-pools"></a>Düğüm ve düğüm havuzları

Destek Hizmetleri ve uygulamaları çalıştırmak için bir Kubernetes gerekir *düğüm*. Bir AKS kümesi, bir Azure kapsayıcı çalışma zamanı ve Kubernetes düğümü bileşenleri çalıştıran sanal makineyi (VM) olan bir veya daha fazla düğümü vardır:

- `kubelet` Kubernetes aracının ana ve zamanlama, istenen kapsayıcı çalıştırmaktan kümeden düzenleme isteklerini işler.
- Sanal ağ tarafından işlenir *kube-proxy* her düğümde. Proxy yollarının ağ trafiği ve Hizmetleri ve pod'ları için IP adresleme yönetir.
- *Kapsayıcı çalışma zamanı* kapsayıcılı uygulamalar çalıştırmak ve sanal ağ ve depolama gibi ek kaynaklarla etkileşim kurmak izin veren bileşendir. AKS, Moby kapsayıcı çalışma zamanı kullanılır.

![Azure sanal makine ve bir Kubernetes düğüm destekleyen kaynaklar](media/concepts-clusters-workloads/aks-node-resource-interactions.png)

Azure VM boyutu, düğümleri için kaç CPU'lar, tanımlar ne kadar bellek ve boyutu ile türünü depolama (yüksek performanslı SSD veya HDD normal gibi) kullanılabilir. Büyük miktarda CPU ve bellek veya yüksek performanslı depolama gerektiren uygulamalar için bir öngörüyorsanız düğüm boyutunu buna göre planlayın. Ayrıca, talebi karşılamak üzere, AKS kümenizin sayıda düğüm ölçeklendirebilirsiniz.

AKS kümenizde düğümleri için VM görüntüsü şu anda Ubuntu Linux veya Windows Server 2019 temel alır. AKS kümesi oluşturma veya düğüm sayısını, Azure platformu istenen VM sayısını oluşturur ve bunları yapılandırır. Gerçekleştirmeniz için el ile yapılandırma yoktur. Aracı düğümleri standard sanal makineleri faturalandırılır, böylece indirimleri VM boyutuna sahip, kullanmakta olduğunuz (dahil olmak üzere [Azure ayırmaları][reservation-discounts]) otomatik olarak uygulanır.

İşletim sistemi, kapsayıcı çalışma zamanı, farklı bir konak kullanın veya özel paketler dahil gerekiyorsa kendi Kubernetes kümesi kullanarak dağıtabilirsiniz [aks altyapısı][aks-engine]. Yukarı Akış `aks-engine` özellikleri serbest bırakır ve resmi olarak AKS kümelerde desteklenen önce yapılandırma seçenekleri sağlar. Örneğin, bir kapsayıcı çalışma zamanı Moby dışında kullanmak isterseniz, kullanabileceğiniz `aks-engine` yapılandırmak ve geçerli ihtiyaçlarınıza uygun bir Kubernetes kümesi dağıtmak için.

### <a name="resource-reservations"></a>Kaynak ayırmalar

Her düğümde temel Kubernetes bileşenleri gibi yönetmeniz gerekmez *kubelet*, *kube-proxy*, ve *kube-dns*, ancak bazı kullanılabilir kullanma işlem kaynakları. Düğümü performansı ve işlevselliği korumak için her bir düğümde aşağıdaki işlem kaynaklarını ayrılmıştır:

- **CPU** - 60 ms
- **Bellek** -4 GiB en fazla %20

Bu bellek ayırma miktarını kullanılabilir CPU ve bellek uygulamalarınız için daha az düğüm içeriyor görünebilir anlamına gelir. Kaynak kısıtlamaları nedeniyle çalıştırdığınız uygulamaların sayısı varsa, bu ayırmalar CPU emin olun ve bellek için temel Kubernetes bileşenleri kullanılabilir durumda kalır. Kaynak ayırmalar değiştirilemez.

Örneğin:

- **Standart DS2 v2** düğüm boyutu 2 vCPU ve 7 GiB bellek içerir
    - %20 7 GiB bellek 1.4 GiB =
    - Toplam *(7-1.4) 5.6 GiB =* bellek düğüm için kullanılabilir
    
- **Standart E4s v3** düğüm boyutu 4 vCPU ve 32 GiB bellek içerir
    - %20 32 GiB bellek 6.4 = GiB ancak AKS yalnızca en fazla 4 GiB ayırır
    - Toplam *(32-4) 28 GiB =* düğüm için kullanılabilir
    
Temel alınan düğümün işletim sistemi, ayrıca kendi çekirdek işlevleri tamamlamak için CPU ve bellek kaynakları miktar gerektirir.

İlişkili en iyi yöntemler için bkz: [aks'deki temel Zamanlayıcı özellikleri için en iyi yöntemler][operator-best-practices-scheduler].

### <a name="node-pools"></a>Düğüm havuzları

Aynı yapılandırmaya sahip düğümler halinde gruplandırılır birlikte *düğüm havuzları*. Bir veya daha fazla düğüm havuzları bir Kubernetes kümesi içerir. Oluşturur bir AKS kümesi oluşturduğunuzda, ilk düğüm sayısını ve boyutunu tanımlanan bir *varsayılan düğüm havuzu*. Bu varsayılan düğüm havuzu aks'deki aracınızı düğümleri çalıştıran temel alınan sanal makineler içeriyor. Birden çok düğüm havuzu desteği şu anda önizleme olarak kullanılabilir.

Ölçeklendirme ya da bir AKS kümesini yükseltme eylemi varsayılan düğüm havuzu karşı yapılır. Ölçeklendirin veya belirli bir düğüm havuzu yükseltmek seçebilirsiniz. Tüm düğümleri başarıyla yükseltilene kadar yükseltme işlemlerinde, düğüm havuzdaki diğer düğüm üzerinde çalışan kapsayıcıları zamanlanır.

AKS içindeki birden çok düğüm havuzları kullanma hakkında daha fazla bilgi için bkz. [oluşturun ve bir AKS kümesi için birden çok düğüm havuzları yönetme][use-multiple-node-pools].

### <a name="node-selectors"></a>Düğüm Seçici

Birden fazla düğüm havuzu içeren bir AKS kümesinde belirli bir kaynak için kullanılacak hangi düğüm havuzu Kubernetes Zamanlayıcı bildirmeniz gerekebilir. Örneğin, giriş denetleyicileri (şu anda önizlemede aks'deki) Windows Server düğümlerinde çalıştırmamalısınız. Düğüm seçici bir pod burada zamanlanması gerektiğini denetlemek için işletim sistemi, düğüm gibi çeşitli parametreleri tanımlamanıza olanak sağlar.

Aşağıdaki temel örnek, NGINX örneğini düğüm Seçicisi'ni kullanarak bir Linux düğümde zamanlar *"beta.kubernetes.io/os": linux*:

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: nginx
spec:
  containers:
    - name: myfrontend
      image: nginx:1.15.12
  nodeSelector:
    "beta.kubernetes.io/os": linux
```

Pod'ların nereden zamanlanır denetleme hakkında daha fazla bilgi için bkz. [aks'deki Gelişmiş Zamanlayıcı özellikleri için en iyi yöntemler][operator-best-practices-advanced-scheduler].

## <a name="pods"></a>Pod'ları

Kubernetes kullanan *pod'ların* uygulamanızın bir örneğini çalıştırmak için. Bir pod uygulamanız tek bir örneğini temsil eder. Pod'ların, genellikle birden çok kapsayıcı bir pod burada içerebilir senaryoları var. Gelişmiş olmamakla birlikte bir kapsayıcı ile 1:1 eşleme vardır. Bu çok kapsayıcılı pod'ların aynı düğümde birlikte zamanlanır ve ilgili kaynakları paylaşmak kapsayıcılar izin verin.

Bir pod oluşturduğunuzda, tanımlayabileceğiniz *kaynak sınırları* belirli miktarda CPU veya bellek kaynakları istemek için. Kubernetes Zamanlayıcı pod'ların bir düğüm isteği karşılamak için kullanılabilir kaynaklar ile çalışacak şekilde zamanlamak çalışır. Belirli bir pod temel alınan düğümünden çok fazla işlem kaynak tüketmesini önlemek en fazla kaynak sınırları da belirtebilirsiniz. Kubernetes Zamanlayıcı hangi kaynakların gerektiği ve izin verilen anlamanıza yardımcı olması tüm pod'ları için kaynak sınırları eklemek iyi bir uygulamadır.

Daha fazla bilgi için [Kubernetes pod'ların][kubernetes-pods] and [Kubernetes pod lifecycle][kubernetes-pod-lifecycle].

Bir pod mantıksal bir kaynak, ancak kapsayıcı burada uygulama iş yüklerini çalıştırın. Pod'ları genellikle kısa ömürlü, atılabilir kaynaklarıdır ve ayrı ayrı zamanlanmış pod'ların bazı Kubernetes sağladığı yüksek kullanılabilirlik ve yedekleme özellikleri eksik. Bunun yerine, pod'ları genellikle dağıtılır ve Kubernetes tarafından yönetilen *denetleyicileri*, dağıtım denetleyicisi gibi.

## <a name="deployments-and-yaml-manifests"></a>Dağıtımları ve YAML bildirimler

A *dağıtım* Kubernetes dağıtımı denetleyicisi tarafından yönetilen bir veya daha fazla özdeş pod temsil eder. Bir dağıtım sayısını tanımlayan *çoğaltmaları* oluşturmak için (pod) ve pod'ların veya düğümleri sorunlarla karşılaşırsanız, ek pod'lar sağlıklı düğümlerinde zamanlandığını Kubernetes Zamanlayıcı sağlar.

Pod'ların yapılandırmasını değiştirmek için dağıtımları güncelleştirebilirsiniz, kapsayıcı görüntüsü kullanılan veya bağlantılı depolama. Dağıtım denetleyicisi boşaltır ve çoğaltmaları, verilen sayıda sonlandırır, yeni dağıtım tanımından kopyalar oluşturur ve işlem Dağıtımdaki tüm çoğaltmaları güncelleştirilene kadar devam eder.

AKS çoğu durum bilgisi olmayan uygulamalarda, tek tek pod'ları zamanlama yerine dağıtım modeli kullanmanız gerekir. Kubernetes, durumunu ve gerekli emin olmak için dağıtım durumunu izleyebilir çoğaltmaların sayısı, kümede çalıştırın. Tek tek pod'ların yalnızca zamanladığınızda bir sorunla karşılaşırsanız ve bunların geçerli düğüm bir sorunla karşılaşırsa sağlıklı düğümlerinde filtrelerden olmayan pod'ların yeniden değildir.

Bir uygulama için bir çekirdek Yönetimi kararlarına için her zaman kullanılabilir örneklerinin gerekiyorsa bu özelliği kesintiye bir güncelleştirme işlemi istemezsiniz. *Pod kesintisi bütçelerini* kaç çoğaltmalar bir dağıtımdaki güncelleştirme veya düğüm yükseltme sırasında kapatılabilmesi tanımlamak için kullanılabilir. Örneğin, *5* çoğaltmaları dağıtımınızdaki pod kesintiye tanımlayabilirsiniz *4* silinmesi ve yeniden aynı anda olan yalnızca bir çoğaltma izin vermek için. Olarak pod kaynak sınırları ile en iyi uygulama pod kesintisi bütçelerini çoğaltmaların her zaman mevcut olacak şekilde en az sayıda gerektiren uygulamalar üzerinde tanımlamaktır.

Dağıtımları genellikle oluşturulan ve yönetilen `kubectl create` veya `kubectl apply`. Bir dağıtımı oluşturmak için bir bildirim dosyası YAML (YAML Ain't Markup Language) biçiminde tanımlar. Aşağıdaki örnek, temel bir NGINX web sunucusunun dağıtımını oluşturur. Dağıtım belirtir *3* çoğaltmaların oluşturulması ve bu bağlantı noktasını *80* kapsayıcıdaki açık olması. CPU ve bellek için Ayrıca kaynak isteklerini ve sınırları tanımlanır.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.15.2
        ports:
        - containerPort: 80
        resources:
          requests:
            cpu: 250m
            memory: 64Mi
          limits:
            cpu: 500m
            memory: 256Mi
```

Daha karmaşık uygulamalar YAML bildirimi içindeki yük Dengeleyiciler gibi hizmetleri de dahil olmak üzere tarafından oluşturulabilir.

Daha fazla bilgi için [Kubernetes dağıtımları][kubernetes-deployments].

### <a name="package-management-with-helm"></a>Helm ile paket Yönetimi

Kubernetes'te uygulamaları yönetmek için yaygın bir yaklaşım olan [Helm][helm]. Derleme ve varolan genel Helm kullanın *grafikleri* Kubernetes YAML bildirimleri kaynakları dağıtmayı ve paketlenmiş bir uygulama kodu sürümünü içerir. Bu grafiklerin yerel olarak veya genellikle bir uzak depoda gibi depolanabilir bir [Azure kapsayıcı kayıt defteri Helm grafiği depo][acr-helm].

Helm kullanmak için bir sunucu bileşeni olarak adlandırılan *Tiller* Kubernetes kümenize yüklenir. Tiller grafikleri küme içindeki yüklenmesini yönetir. Helm istemci bilgisayarınızda yerel olarak yüklenir veya içinde kullanılan [Azure Cloud Shell][azure-cloud-shell]. Arayın veya istemciyle Helm grafiklerine oluşturabilir ve Kubernetes kümenize yükleyin.

![Helm istemci bileşeni ve Kubernetes kümesi içindeki kaynakları oluşturan bir sunucu tarafı Tiller bileşeni içerir.](media/concepts-clusters-workloads/use-helm.png)

Daha fazla bilgi için [Azure Kubernetes Service (AKS) Helm ile uygulamaları yükleme][aks-helm].

## <a name="statefulsets-and-daemonsets"></a>StatefulSets ve DaemonSets

Dağıtım denetleyicisi Kubernetes Zamanlayıcı çoğaltmaları, verilen sayıda kullanılabilir herhangi bir düğümde kullanılabilir kaynaklarla çalıştırmak için kullanır. Bu yaklaşım, dağıtımları kullanma yeterli olabilir. durum bilgisiz uygulamalar için ancak değil kalıcı adlandırma kuralı ya da depolama gerektiren uygulamalar için. Her düğümü veya bir küme içinde seçili düğümleri mevcut bir çoğaltma gerektiren uygulamalar için yineleme düğümleri arasında nasıl dağıtıldığını dağıtım Denetleyicisi görünmüyor.

Bu tür uygulamaları yönetmenize olanak sağlayan iki Kubernetes kaynak vardır:

- *StatefulSets* -depolama gibi bir tek pod yaşam döngüsü uygulamaların durumunu korumak.
- *DaemonSets* -Kubernetes önyükleme işlemi her bir düğümde çalışan bir örneği emin olun.

### <a name="statefulsets"></a>StatefulSets

Modern uygulama geliştirme genellikle durum bilgisiz uygulamalar için amaçlar ancak *StatefulSets* veritabanı bileşenlerini içeren uygulamaları gibi durum bilgisi olan uygulamalar için kullanılabilir. Bir veya daha fazla özdeş pod oluşturulabilir ve yönetilebilir bir StatefulSet dağıtıma benzer. Bir StatefulSet yinelemede zarif ve sıralı bir yaklaşım, dağıtım, ölçeklendirme, yükseltme ve sonlandırmalar izleyin. Çoğaltmaları yeniden gibi bir StatefulSet adlandırma kuralı, ağ adları ve depolama ile kalıcı hale getirin.

YAML biçimini kullanarak uygulamayı tanımlayan `kind: StatefulSet`, ve StatefulSet denetleyicisi daha sonra dağıtım ve yönetimini gerekli çoğaltmaların işler. Veri kalıcı depolama, Azure yönetilen diskler veya Azure dosyaları tarafından sağlanan yazılır. StatefulSets ile temel alınan kalıcı depolama StatefulSet silindiğinde bile kalır.

Daha fazla bilgi için [Kubernetes StatefulSets][kubernetes-statefulsets].

Bir StatefulSet yinelemede zamanlanmış ve bir AKS kümesindeki herhangi bir kullanılabilir düğüm üzerinde çalıştırın. Bu en az bir pod içinde emin olmak gerekiyorsa kümenizi düğüm üzerinde çalışır, bunun yerine bir DaemonSet kullanabilirsiniz.

### <a name="daemonsets"></a>DaemonSets

Özel günlük toplama veya izleme ihtiyaçları için belirli bir pod tüm çalıştırmanız gerekebilir veya seçili düğümleri. A *DaemonSet* aynı pod, bir veya daha fazla dağıtmak için tekrar kullanılır ancak DaemonSet denetleyici, belirtilen her düğümün pod örneğini çalıştırmasını sağlar.

DaemonSet denetleyicisi pod'ların düğümlerdeki Küme önyükleme işlemi, Kubernetes Zamanlayıcısı başlatıldı varsayılan önce zamanlayabilirsiniz. Bu yetenek, geleneksel pod'ların dağıtımı veya StatefulSet zamanlanmış önce bir DaemonSet pod ' ların başlatılır sağlar.

Bir YAML tanımı kullanarak bir parçası olarak tanımlanan StatefulSets gibi bir DaemonSet `kind: DaemonSet`.

Daha fazla bilgi için [Kubernetes DaemonSets][kubernetes-daemonset].

> [!NOTE]
> Kullanıyorsanız [sanal düğümü eklenti](virtual-nodes-cli.md#enable-virtual-nodes-addon), DaemonSets pod'ların sanal düğümde oluşturmayacaktır.

## <a name="namespaces"></a>Ad Alanları

Pod'ların ve dağıtımları gibi Kubernetes kaynakları mantıksal olarak gruplandırılır bir *ad alanı*. Koruyabilmesini mantıksal olarak bir AKS kümesi bölün ve oluşturmak, görüntülemek veya kaynakları yönetmek için erişimi kısıtlamak için bir yol sağlar. Örneğin, iş grupları ayırmak için ad alanları oluşturabilirsiniz. Kullanıcılar yalnızca kullanıcıya atanan ad alanlarını içindeki kaynaklarla etkileşim kurabilir.

![Kaynakları ve uygulamaları mantıksal olarak ayırmak için Kubernetes ad alanları](media/concepts-clusters-workloads/namespaces.png)

AKS kümesi oluşturma, şu ad alanlarından kullanılabilir:

- *Varsayılan* -hiçbiri sağlandığında burada pod'ların ve dağıtımları varsayılan olarak oluşturulur, bu ad alanıdır. Daha küçük ortamlarda uygulamalar varsayılan ad alanı doğrudan ek mantıksal ayrılma oluşturmadan dağıtabilirsiniz. Ne zaman etkileşim Kubernetes API ile gibi `kubectl get pods`, belirtilmediğinde varsayılan ad alanı kullanılır.
- *kube-system* -çekirdek kaynakları, DNS ve proxy veya Kubernetes panosunu gibi ağ özellikleri gibi bulunduğu bu ad alanıdır. Genellikle, bu ad alanı, kendi uygulamalarınızı dağıtmayın.
- *kube-genel* - bu ad alanı genellikle kullanılmaz, ancak için kaynaklar için tüm küme genelinde görünür olmasını kullanılması ve herhangi bir kullanıcı tarafından görüntülenebilir.

Daha fazla bilgi için [Kubernetes ad alanları][kubernetes-namespaces].

## <a name="next-steps"></a>Sonraki adımlar

Bu makale, bazı temel Kubernetes bileşenleri ve bunların AKS kümeye nasıl uygulandığını kapsar. Çekirdek Kubernetes hakkında daha fazla bilgi ve AKS kavramlar için aşağıdaki makalelere bakın:

- [Kubernetes / AKS erişim ve kimlik][aks-concepts-identity]
- [Kubernetes / AKS güvenlik][aks-concepts-security]
- [Kubernetes / AKS sanal ağlar][aks-concepts-network]
- [Kubernetes / AKS depolama][aks-concepts-storage]
- [Kubernetes / AKS ölçeklendirin][aks-concepts-scale]

<!-- EXTERNAL LINKS -->
[aks-engine]: https://github.com/Azure/aks-engine
[kubernetes-pods]: https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/
[kubernetes-pod-lifecycle]: https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/
[kubernetes-deployments]: https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
[kubernetes-statefulsets]: https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/
[kubernetes-daemonset]: https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/
[kubernetes-namespaces]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
[helm]: https://helm.sh/
[azure-cloud-shell]: https://shell.azure.com

<!-- INTERNAL LINKS -->
[aks-concepts-identity]: concepts-identity.md
[aks-concepts-security]: concepts-security.md
[aks-concepts-scale]: concepts-scale.md
[aks-concepts-storage]: concepts-storage.md
[aks-concepts-network]: concepts-network.md
[acr-helm]: ../container-registry/container-registry-helm-repos.md
[aks-helm]: kubernetes-helm.md
[operator-best-practices-cluster-security]: operator-best-practices-cluster-security.md
[operator-best-practices-scheduler]: operator-best-practices-scheduler.md
[use-multiple-node-pools]: use-multiple-node-pools.md
[operator-best-practices-advanced-scheduler]: operator-best-practices-advanced-scheduler.md
[reservation-discounts]: ../billing/billing-save-compute-costs-reservations.md