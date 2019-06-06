---
title: İşleç en iyi uygulamalar - Azure Kubernetes Hizmetleri (AKS) küme güvenliği
description: Küme güvenliği ve yükseltmeleri Azure Kubernetes Service (AKS) nasıl yönetileceği küme işleci en iyi uygulamaları öğrenin
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: iainfou
ms.openlocfilehash: 54f1455467295e786d9e634b64dfab0933d948db
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66475595"
---
# <a name="best-practices-for-cluster-security-and-upgrades-in-azure-kubernetes-service-aks"></a>Küme güvenliği ve yükseltmeler Azure Kubernetes Service (AKS) için en iyi uygulamalar

Azure Kubernetes Service (AKS) kümeleri yönetme gibi iş yüklerinde ve veri güvenliğini önemli bir noktadır. Özellikle, mantıksal yalıtım kullanarak çok kiracılı kümeleri çalıştırdığınızda, kaynaklar ve iş yüklerini güvenli erişim gerekir. Saldırı riskini en aza indirmek için Ayrıca düğümün işletim sistemi güvenlik güncelleştirmelerini ve en son Kubernetes geçerli olduğundan emin olmamız gerekir.

Bu makalede, AKS kümenizin güvenliğini sağlama konusunda odaklanır. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * API sunucusu erişimi güvenli hale getirmek için Azure Active Directory ve rol tabanlı erişim denetimleri kullanma
> * Kapsayıcı düğümü kaynaklarına güvenli erişim
> * Bir AKS kümesi son Kubernetes sürümüne yükseltmek için
> * Düğümleri güncelleştirme güncel tutulmasını ve güvenlik düzeltme eklerini otomatik olarak Uygula

Ayrıca, en iyi yöntemler için okuyabilirsiniz [kapsayıcı görüntüsü Yönetimi] [ best-practices-container-image-management] ve [pod güvenlik][best-practices-pod-security].

## <a name="secure-access-to-the-api-server-and-cluster-nodes"></a>API sunucu ile küme düğümleri erişimin güvenliğini sağlama

**En iyi uygulama kılavuzunu** -Kubernetes API sunucuya erişim güvenliğini sağlama, kümenizin güvenliğini sağlamak için yapabileceğiniz en önemli şeylerden biridir. Kubernetes rol tabanlı erişim denetimi (RBAC) API sunucusuna erişimi denetlemek için Azure Active Directory ile tümleştirin. Bu denetimler, AKS Azure aboneliklerinize erişimi güvenli şekilde güvenli hale getirmenize olanak tanır.

Kubernetes API sunucusuna bir küme içindeki eylemleri gerçekleştirmek istekleri için bir tek bağlantı noktası sağlar. Güvenli ve API sunucusuna erişimi denetlemek için erişimi sınırlamak ve gereken en az ayrıcalıklı erişim izinleri sağlar. Bu yaklaşım, Kubernetes için benzersiz değildir, ancak çok kiracılı kullanılmak üzere mantıksal olarak yalıtılmış bir AKS kümesi olduğunda özellikle önemlidir.

Azure Active Directory (AD), AKS kümeleri ile tümleşen bir kurumsal kullanıma hazır kimlik yönetimi çözümü sağlar. Kubernetes, bir kimlik yönetimi çözümü sağlamayan gibi Aksi takdirde API sunucusuna erişimi kısıtlamak için ayrıntılı bir şekilde sağlamak zor olabilir. Azure AD ile tümleşik kümeleriyle aks'deki, API sunucusu için kullanıcıların kimliğini doğrulamak için mevcut kullanıcı ve grup hesapları kullanırsınız.

![AKS kümeleri için Azure Active Directory Tümleştirmesi](media/operator-best-practices-cluster-security/aad-integration.png)

Kubernetes RBAC ve Azure API sunucusunun güvenliğini sağlama ve az sağlamak için AD tümleştirme tek bir ad alanı gibi kaynaklar kapsamlı bir dizi gerekli izinler sayısı. Farklı kullanıcılar veya gruplar Azure AD'de farklı RBAC rollerini verilebilir. Bu ayrıntılı izinler API sunucusuna erişimi kısıtlamak ve gerçekleştirilen eylemlerin bir açık denetim izi sağlamak belirlemenizi sağlar.

Önerilen en iyi uygulama grupları dosyalara ve klasörlere ayrı kimlik ve erişim sağlamak için Azure AD kullanma kullanmaktır *grubu* kullanıcıları tek tek yerine RBAC rollerini bağlamak için üyelik *kullanıcılar*. Bir kullanıcının grup üyeliği değişiklikleri olarak bunların erişim izinlerini AKS kümesinde uygun şekilde değiştirirsiniz. Bir role kullanıcı bağlarsanız, kendi iş işlevi değiştirebilir. Azure AD grup üyeliklerini güncelleştirmeniz gerekir, ancak izinleri AKS kümesi, yansıtmıyor. Bu senaryoda, kullanıcı, bir kullanıcı gerektirir. daha fazla izin verilmeden sona erer.

Azure AD tümleştirmesi ve RBAC hakkında daha fazla bilgi için bkz. [en iyi uygulamalar için kimlik doğrulama ve yetkilendirme aks'deki][aks-best-practices-identity].

## <a name="secure-container-access-to-resources"></a>Kapsayıcı kaynaklarına güvenli erişim

**En iyi uygulama kılavuzunu** -kapsayıcıları gerçekleştirebileceği eylemleri erişimi. En az sağlayan izinleri sayısı ve kök kullanmaktan kaçının / ayrıcalıklı yükseltme.

En az kullanıcılar veya gruplar vermelisiniz aynı şekilde ayrıcalıkları sayısı gerekli, kapsayıcılar da eylemleri ve ihtiyaç duydukları işlemleri yalnızca sınırlı olmalıdır. Saldırı riskini en aza indirmek için uygulamaları ve ilerletilen ayrıcalıkları gerektiren veya kök erişim kapsayıcıları yapılandırmayın. Örneğin, `allowPrivilegeEscalation: false` pod katıştırır. Bunlar *güvenlik kapsamları pod* Kubernetes ve kullanıcı veya grup gibi ek izinler olarak çalıştırmak için tanımlamanıza imkan tanır yerleşiktir ya da Linux özellikleri göstermek için. Diğer en iyi yöntemleri için bkz. [kaynaklara erişimi güvenli pod][pod-security-contexts].

Kapsayıcı işlemlerin daha ayrıntılı denetim için yerleşik Linux güvenlik özellikleri gibi kullanabilirsiniz *AppArmor* ve *seccomp*. Bu özellikler düğümü düzeyinde tanımlanan ve ardından bir pod bildirimi aracılığıyla uygulanır. Yerleşik Linux güvenlik özellikleri, yalnızca Linux düğümleri ve pod'ları üzerinde kullanılabilir.

> [!NOTE]
> AKS veya başka bir yerde, Kubernetes ortamlarını tehlikeli çok kiracılı kullanım için tamamen güvenli değildir. Ek güvenlik özellikleri gibi *AppArmor*, *seccomp*, *Pod güvenlik ilkeleri*, veya daha fazla ayrıntılı rol tabanlı erişim denetimleri (RBAC) düğümleri için davranışları daha zor. Ancak, tehlikeli çok kiracılı iş yüklerini çalıştırırken doğru güvenlik için bir hiper yönetici yalnızca güvenip güvenmeyeceğini güvenlik düzeyidir. Kubernetes için güvenlik etki alanı, tüm küme, tek bir düğüm olur. Bu tür tehlikeli çok kiracılı iş yükleri için fiziksel olarak izole edilmiş kümeleri kullanmanız gerekir.

### <a name="app-armor"></a>Uygulama Armor

Kapsayıcıları gerçekleştirebileceği eylemleri sınırlamak için kullanabileceğiniz [AppArmor] [ k8s-apparmor] Linux çekirdek güvenlik modülü. AppArmor kullanılabilir işletim sistemi, temel alınan AKS düğümü bir parçası olarak ve varsayılan olarak etkindir. AppArmor eylemler gibi kısıtlama profillerini okuma, yazma veya yürütme ya da dosya sistemleri bağlama gibi sistem işlevlerini oluşturursunuz. Varsayılan AppArmor profilleri çeşitli erişimi kısıtlamak `/proc` ve `/sys` konumları ve temel alınan düğümünden kapsayıcılar mantıksal olarak ayırmak için bir yol sağlar. Yalnızca Kubernetes pod'larını Linux üzerinde çalışan herhangi bir uygulama için AppArmor çalışır.

![Kapsayıcı eylemlerine sınırlamak için bir AKS kümesi kullanımda AppArmor profilleri](media/operator-best-practices-container-security/apparmor.png)

AppArmor nasıl çalıştığını görmek için aşağıdaki örnek dosyalara yazma engelleyen bir profili oluşturur. [SSH] [ aks-ssh] bir AKS düğüme adlı bir dosya oluşturup *Reddet write.profile* ve aşağıdaki içeriği yapıştırın:

```
#include <tunables/global>
profile k8s-apparmor-example-deny-write flags=(attach_disconnected) {
  #include <abstractions/base>
  
  file,
  # Deny all file writes.
  deny /** w,
}
```

AppArmor profilleri kullanılarak eklenir `apparmor_parser` komutu. Profili eklemek için AppArmor ve önceki adımda oluşturulan profilin adını belirtin:

```console
sudo apparmor_parser deny-write.profile
```

Profilin doğru bir şekilde ayrıştırılır ve AppArmor için uygulanan olursa çıktı yok. Komut istemine geri dönersiniz.

Yerel makinenizden artık adlı pod bildirim oluşturmak *aks apparmor.yaml* ve aşağıdaki içeriği yapıştırın. Bu bildirim için bir ek açıklama tanımlar `container.apparmor.security.beta.kubernetes` başvurular ekleme *Reddet yazma* önceki adımlarda oluşturulan profil:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hello-apparmor
  annotations:
    container.apparmor.security.beta.kubernetes.io/hello: localhost/k8s-apparmor-example-deny-write
spec:
  containers:
  - name: hello
    image: busybox
    command: [ "sh", "-c", "echo 'Hello AppArmor!' && sleep 1h" ]
```

Örnek kullanılarak pod dağıtma [kubectl uygulamak] [ kubectl-apply] komutu:

```console
kubectl apply -f aks-apparmor.yaml
```

Dağıtılan pod ile kullanmak [kubectl exec] [ kubectl-exec] bir dosyaya yazmak için komutu. Aşağıdaki örnek çıktıda gösterildiği gibi komutu yürütülemiyor:

```
$ kubectl exec hello-apparmor touch /tmp/test

touch: /tmp/test: Permission denied
command terminated with exit code 1
```

AppArmor hakkında daha fazla bilgi için bkz: [Kubernetes AppArmor profillerinde][k8s-apparmor].

### <a name="secure-computing"></a>Bilgi işlem güvenliğini sağlama

Herhangi bir Linux uygulaması için AppArmor çalışırken [seccomp (*sn*üvenli *comp*retim akışı)] [ seccomp] işlem düzeyinde çalışır. Seccomp aynı zamanda bir Linux çekirdek güvenlik modülü olan ve AKS düğümleri tarafından kullanılan Docker çalışma zamanı tarafından yerel olarak desteklenir. Kapsayıcıları gerçekleştirebilirsiniz işlem çağrıları seccomp ile sınırlıdır. İzin verme veya reddetme eylemleri tanımlayın. filtre oluşturma ve seccomp filtresiyle ilişkilendirilebilmesi için ek açıklamalar içindeki bir pod YAML bildirimi'i kullanın. Bu, yalnızca kapsayıcı çalıştırmak için gereken en az düzeyde izinleri verme ve artık en iyi uygulama hizalar.

Seccomp iş başında görmek için değiştirme engelleyen bir filtre oluşturun. bir dosya üzerindeki izinleri. [SSH] [ aks-ssh] bir AKS düğümüne, ardından adlı seccomp filtre oluşturma */var/lib/kubelet/seccomp/prevent-chmod* ve aşağıdaki içeriği yapıştırın:

```
{
  "defaultAction": "SCMP_ACT_ALLOW",
  "syscalls": [
    {
      "name": "chmod",
      "action": "SCMP_ACT_ERRNO"
    }
  ]
}
```

Yerel makinenizden artık adlı pod bildirim oluşturmak *aks seccomp.yaml* ve aşağıdaki içeriği yapıştırın. Bu bildirim için bir ek açıklama tanımlar `seccomp.security.alpha.kubernetes.io` ve başvurur *önlemek chmod* önceki adımda oluşturduğunuz Filtresi:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: chmod-prevented
  annotations:
    seccomp.security.alpha.kubernetes.io/pod: localhost/prevent-chmod
spec:
  containers:
  - name: chmod
    image: busybox
    command:
      - "chmod"
    args:
     - "777"
     - /etc/hostname
  restartPolicy: Never
```

Örnek kullanılarak pod dağıtma [kubectl uygulamak] [ kubectl-apply] komutu:

```console
kubectl apply -f ./aks-seccomp.yaml
```

Kullanarak pod'ların durumunu görüntülemek [kubectl pod'ları alma] [ kubectl-get] komutu. Pod bir hata bildirir. `chmod` Komut çalışması engellenir seccomp filtre tarafından aşağıdaki örnek çıktıda gösterildiği gibi:

```
$ kubectl get pods

NAME                      READY     STATUS    RESTARTS   AGE
chmod-prevented           0/1       Error     0          7s
```

Kullanılabilir filtreleri hakkında daha fazla bilgi için bkz. [Seccomp güvenlik profilleri için Docker][seccomp].

## <a name="regularly-update-to-the-latest-version-of-kubernetes"></a>Düzenli olarak kubernetes en son sürüme güncelleştirme

**En iyi uygulama kılavuzunu** - yeni özellikler ve hata düzeltmeleri, AKS kümenizde Kubernetes sürümüne yükseltmek düzenli olarak güncel kalın.

Kubernetes, yeni özellikleri daha geleneksel altyapı platformları daha hızlı bir hızda serbest bırakır. Kubernetes güncelleştirmeleri, yeni özellikler ve hata veya güvenlik düzeltmeleri içerir. Yeni özellikler genellikle taşıma yoluyla bir *alfa* ardından *beta* haline gelmeden önce durumu *kararlı* ve genel olarak kullanılabilir ve üretim kullanımı için önerilen. Bu sürüm döngüsü Kubernetes düzenli olarak yeni değişiklikler karşılaşıldığında veya dağıtımları ve şablonları ayarlama olmadan güncelleştirmenize izin vermelidir.

AKS, dört alt Kubernetes sürümlerini destekler. Bu, yeni bir ikincil düzeltme eki sürümü eklendiğinde, desteklenen en eski ikincil sürüm ve yama sürümler kullanımdan anlamına gelir. Kubernetes için küçük güncelleştirmeler düzenli aralıklarla gerçekleşecek. Denetleyin ve destek kapsamı dışında kalan yoksa için gereken şekilde yükseltmek için idare işlemi olduğundan emin olun. Daha fazla bilgi için [AKS desteklenen Kubernetes sürümleri][aks-supported-versions]

Kümeniz için kullanılabilir sürümlerini denetlemek için kullanmak [az aks get-yükseltmeleri] [ az-aks-get-upgrades] komutu aşağıdaki örnekte gösterildiği gibi:

```azurecli-interactive
az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster
```

Ardından bölümünü kullanarak AKS kümenizin yükseltebilirsiniz [az aks yükseltme] [ az-aks-upgrade] komutu. Yükseltme işlemi güvenli bir şekilde cordons ve aynı anda bir düğümü boşaltır, pod'ların Kalan düğümlerde zamanlar ve sonra en son işletim sistemi ve Kubernetes sürümlerini çalıştıran yeni bir düğüm dağıtır.

```azurecli-interactive
az aks upgrade --resource-group myResourceGroup --name myAKSCluster --kubernetes-version 1.11.8
```

AKS yükseltmeler hakkında daha fazla bilgi için bkz. [aks'deki Kubernetes desteklenen sürümlerini] [ aks-supported-versions] ve [AKS kümesini yükseltme][aks-upgrade].

## <a name="process-linux-node-updates-and-reboots-using-kured"></a>İşlem Linux düğümü güncelleştirir ve kured kullanarak yeniden başlatır.

**En iyi uygulama kılavuzunu** - AKS otomatik olarak indirir ve yükler güvenlik düzeltmeleri her Linux düğümlerinde, ancak otomatik olarak gerekirse yeniden başlatmaz. Kullanım `kured` izlemesi yeniden başlatmalar, ardından güvenli bir şekilde kordon altına alma ve düğümü yeniden başlatmak izin vermek için düğüm boşaltma için güncelleştirmelerini ve işletim sistemi ile ilgili mümkün olduğu kadar güvenli. (Şu anda önizlemede aks'deki) Windows Server düğümleri için düzenli olarak güvenli bir şekilde kordon altına alma ve pod'ların boşaltabilir ve güncelleştirilmiş düğümlerini dağıtmak için bir AKS yükseltme işlemi gerçekleştirin.

Her Akşam aks'deki Linux düğümleri güvenlik düzeltme ekleri, distro güncelleştirme kanalı kullanıma alın. Bu davranış, bir AKS kümesinde düğümlere dağıtılmış olarak otomatik olarak yapılandırılır. Kesintisi ve çalışan iş yükleri için olası etkisini en aza indirmek için düğümleri otomatik olarak bir güvenlik düzeltme eki, yeniden başlatılır değil veya çekirdek güncelleştirme gerektiriyor.

Açık kaynak [kured (KUbernetes yeniden arka plan programı)] [ kured] Weaveworks tarafından proje düğümünü yeniden başlatma için izler. Bir Linux düğümü yeniden başlatma gerektiren güncelleştirmeler uygulandığında, düğüm güvenli bir şekilde kordonlanır ve taşıma ve kümedeki diğer düğümlere pod'ları planlamak için boşaltılır. Düğüm yeniden başlatıldıktan sonra Kubernetes sürdürür üzerindeki pod'ları zamanlama ve küme yeniden içine eklenir. Uğramasını azaltmak için aynı anda yalnızca tek bir düğüm tarafından başlatılması izin `kured`.

![Kured kullanarak AKS düğümü yeniden başlatma işlemi](media/operator-best-practices-cluster-security/node-reboot-process.png)

Ne zaman yeniden başlatma gerçekleşir, ince detaylı denetim istiyorsanız `kured` diğer bakım olayları veya devam eden küme sorunları varsa, yeniden başlatma işlemlerini önlemek için Prometheus ile tümleştirebilirsiniz. Bu tümleştirme ek zorluklar diğer sorunlarını giderme etkin durumdayken düğümlerin yeniden başlatılması en aza indirir.

Düğümü yeniden başlatma işlemlerini işlemek nasıl hakkında daha fazla bilgi için bkz. [AKS düğümleri için güvenlik ve çekirdek güncelleştirmeleri uygulamak][aks-kured].

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, AKS kümenizin güvenliğini sağlama konusunda odaklanır. Bu alanlardan bazıları uygulamak için aşağıdaki makalelere bakın:

* [AKS ile Azure Active Directory Tümleştirme][aks-aad]
* [En son sürüme kubernetes AKS kümesini yükseltme][aks-upgrade]
* [İşlem güvenlik güncelleştirmeleri ve node ile kured yeniden başlatır][aks-kured]

<!-- EXTERNAL LINKS -->
[kured]: https://github.com/weaveworks/kured
[k8s-apparmor]: https://kubernetes.io/docs/tutorials/clusters/apparmor/
[seccomp]: https://kubernetes.io/docs/concepts/policy/pod-security-policy/#seccomp
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-exec]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#exec
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get

<!-- INTERNAL LINKS -->
[az-aks-get-upgrades]: /cli/azure/aks#az-aks-get-upgrades
[az-aks-upgrade]: /cli/azure/aks#az-aks-upgrade
[aks-supported-versions]: supported-kubernetes-versions.md
[aks-upgrade]: upgrade-cluster.md
[aks-best-practices-identity]: concepts-identity.md
[aks-kured]: node-updates-kured.md
[aks-aad]: azure-ad-integration.md
[best-practices-container-image-management]: operator-best-practices-container-image-management.md
[best-practices-pod-security]: developer-best-practices-pod-security.md
[pod-security-contexts]: developer-best-practices-pod-security.md#secure-pod-access-to-resources
[aks-ssh]: ssh.md
