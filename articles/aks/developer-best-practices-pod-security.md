---
title: Geliştirici en iyi uygulamalar - Pod güvenlik Azure Kubernetes Hizmetleri (AKS)
description: Geliştirici için en iyi pod'ları Azure Kubernetes Service (AKS) güvenli hale getirmeyi öğrenin
services: container-service
author: rockboyfor
ms.service: container-service
ms.topic: conceptual
origin.date: 12/06/2018
ms.date: 04/08/2019
ms.author: v-yeche
ms.openlocfilehash: 1c2c5cbee91ddaee5f1f6af8ec17c48326f68e84
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60466893"
---
# <a name="best-practices-for-pod-security-in-azure-kubernetes-service-aks"></a>Pod güvenlik Azure Kubernetes Service (AKS) için en iyi uygulamalar

Geliştirmek ve Azure Kubernetes Service (AKS) uygulamaları çalıştırma gibi podlarınız güvenliğini önemli bir noktadır. Uygulamalarınız için gereken ayrıcalıklar en az sayıda ilkesini tasarlanmalıdır. Özel verilerini güvende tutmanın üstünde müşteriler için göz önünde yer alır. Veritabanı bağlantı dizelerini, anahtarları veya gizli diziler ve sertifikalar burada bir saldırgan bu gizli dizileri avantajlarından kötü amaçlar için ele geçirebilir dış dünyaya açık gibi kimlik bilgilerini istemezsiniz. Bunları kodunuza ekleyin veya kapsayıcı görüntülerinizi katıştırmanız kullanmayın. Bu yaklaşım bir maruz kalma riskini oluşturun ve ancak kapsayıcı görüntülerini yeniden açmanız gerekeceğinden bu kimlik bilgilerini döndürmek için özelliğini sınırlayabilir.

Bu en iyi yöntemler makalesi, nasıl güvenli pod'ların aks'deki odaklanır. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * İşlemler ve hizmetler veya ayrıcalık yükseltme saldırısı erişimi sınırlamak için pod güvenlik bağlamını kullanır.
> * Pod yönetilen kimliklerle diğer Azure kaynaklarıyla kimlik doğrulaması
> * Azure Key Vault gibi dijital bir kasa kimlik bilgilerini almak ve istek

Ayrıca, en iyi yöntemler için okuyabilirsiniz [küme güvenlik] [ best-practices-cluster-security] ve [kapsayıcı görüntüsü Yönetimi][best-practices-container-image-management].

## <a name="secure-pod-access-to-resources"></a>Pod kaynaklarına güvenli erişim

**En iyi uygulama kılavuzunu** - farklı bir kullanıcı veya grup ve sınırı erişim temel alınan düğüm işlemleri ve hizmetleri çalıştırmak, pod güvenlik bağlamı ayarları tanımlar. En az atama ayrıcalıklarına sayısı.

Uygulamalarınızın düzgün çalışması için tanımlanan kullanıcı veya grup pod'ların çalışması gerektiğini olarak değil *kök*. `securityContext` Pod ya da kapsayıcı ayarları gibi tanımlamanızı sağlar *farklıkullanıcı* veya *fsGroup* uygun izinleri varsaymak. Yalnızca gerekli kullanıcı veya grup izinleri atayın ve güvenlik bağlamı bir şekilde ek izinler varsaymak kullanmayın. Kök olmayan kullanıcı olarak çalıştırdığınızda, kapsayıcılar ayrıcalıklı bağlantı noktalarına altında 1024 bağlanamıyor. Bu senaryoda, Kubernetes Hizmetleri bir uygulamanın belirli bir bağlantı noktası üzerinde çalıştığı olgu gizlemek için kullanılabilir.

Pod güvenlik bağlamı da ek özellikler veya işlemleri ve Hizmetleri erişmek için izinler tanımlayabilirsiniz. Aşağıdaki ortak güvenlik bağlamı tanımları ayarlanabilir:

* **allowPrivilegeEscalation** pod varsayabilirsiniz tanımlar *kök* ayrıcalıkları. Bu ayar her zaman ayarlanır böylece uygulamalarınızı tasarlamak *false*.
* **Linux özellikleri** erişim temel düğüm işleme pod olanak tanır. Bu özellikler atama ile dikkatli olun. En az Ata ayrıcalıkları gerekli sayısı. Daha fazla bilgi için [Linux özellikleri][linux-capabilities].
* **SELinux etiketleri** Hizmetleri, işlemleri ve dosya sistemi erişimi için erişim ilkeleri tanımlamanıza olanak sağlayan bir Linux çekirdek güvenlik modüldür. En az yeniden ata ayrıcalıkları gerekli sayısı. Daha fazla bilgi için [kubernetes'te SELinux seçenekleri][selinux-labels]

Aşağıdaki örnek pod YAML bildirim güvenlik bağlamı ayarları tanımlamak için ayarlar:

* Pod çalıştıran kullanıcı kimliği *1000* ve grup kimliği bölümünü *2000*
* Kullanılacak ayrıcalıkları ilerletebilir olamaz `root`
* Ağ arabirimleri ve ana bilgisayarın (Donanım) gerçek zamanlı saatini erişmek Linux özellikleri sağlar

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: security-context-demo
spec:
  containers:
    - name: security-context-demo
      image: dockerhub.azk8s.cn/nginx:1.15.5
    securityContext:
      runAsUser: 1000
      fsGroup: 2000
      allowPrivilegeEscalation: false
      capabilities:
        add: ["NET_ADMIN", "SYS_TIME"]
```

Gereksinim duyduğunuz güvenlik bağlamı ayarları belirlemek için küme işleci ile çalışır. Ek izinler ve erişim pod gerektirir en aza indirmek için uygulamalarınızı tasarlamak deneyin. AppArmor ve küme operatörleri tarafından uygulanabilir (güvenli bilgi işlem) seccomp kullanarak erişimi kısıtlamak için ek güvenlik özellikleri vardır. Daha fazla bilgi için [güvenli kaynaklara erişim kapsayıcı][apparmor-seccomp].

## <a name="limit-credential-exposure"></a>Kimlik bilgisi ifşa sınırı

**En iyi uygulama kılavuzunu** -kimlik bilgileri uygulama kodunuzda tanımlama yok. Azure kaynakları için yönetilen kimlikleri pod isteği erişiminizi diğer kaynaklara izin vermek için kullanın. Azure Key Vault gibi dijital bir kasa, depolamak ve dijital anahtarlarını ve kimlik bilgilerini almak için de kullanılmalıdır.

Kimlik bilgileri uygulama kodunuzda maruz kalma riskini sınırlamak için sabit ya da paylaşılan kimlik bilgilerini kullanmaktan kaçının. Kimlik bilgilerine veya anahtarlara doğrudan kodunuza eklenmemelidir. Bu kimlik bilgilerinin ifşa edildiği imzalanmasını ve güncelleştirilmiş uygulama gerekir. Kendi kimlik ve kendi kimliğini doğrulamasını veya otomatik olarak dijital bir kasadan kimlik bilgilerini almak için yol pod'ların vermek daha iyi bir yaklaşımdır.

Aşağıdaki [ilişkili AKS açık kaynaklı projelerin] [ aks-associated-projects] otomatik olarak pod'ların veya istek kimlik bilgileri ve dijital Kasası'ndaki anahtarları kimlik doğrulaması sağlar:

* Yönetilen Azure kaynakları için kimlikleri ve
* Azure anahtar kasası FlexVol sürücüsü

İlişkili AKS açık kaynak projeleri, Azure teknik destek birimi tarafından desteklenmez. Görüş ve hata topluluğumuza toplamak için sağlanır. Bu projeler üretim kullanımı için önerilmez.

### <a name="use-pod-managed-identities"></a>Kullanım pod kimlikler yönetilen

Azure kaynakları için yönetilen bir kimlik, Azure'da yer alan depolama, SQL gibi destekleyen herhangi bir hizmeti kendi kimliğini doğrulamak için bir pod olanak tanır. Pod, bunları Azure Active Directory kimlik doğrulaması ve dijital belirteç almasını sağlayan bir Azure kimlik atanır. Bu dijital belirteci pod hizmete erişmek ve gerekli eylemleri gerçekleştirmek için yetkili olup olmadığını denetleyen diğer Azure hizmetlerine sunulabilir. Bu yaklaşım, gizli dizi örnek veritabanı bağlantı dizeleri için gerekli olduğu anlamına gelir. Pod yönetilen kimlik için Basitleştirilmiş iş akışı aşağıdaki diyagramda gösterilmiştir:

![Azure'da kimlik pod için basitleştirilmiş bir iş akışı yönetilen](media/developer-best-practices-pod-security/basic-pod-identity.png)

Yönetilen bir kimlik ile uygulama kodunuzu Azure depolama gibi bir hizmete erişmek için kimlik bilgilerini eklemek gerekmez. Bu nedenle her pod kendi kimliği ile kimlik doğrulaması gibi denetim ve erişim gözden geçirebilirsiniz. Uygulamanız diğer Azure Hizmetleri ile bağlanıyorsa, kimlik bilgilerini yeniden sınırı ve ifşa riskini yönetilen kimlikleri kullanın.

Pod kimlikler hakkında daha fazla bilgi için bkz. [yönetilen pod kimlikleri kullanmak için bir AKS kümesi yapılandırma ve uygulamalarınız ile][aad-pod-identity]

### <a name="use-azure-key-vault-with-flexvol"></a>Azure Key Vault ile FlexVol kullanma

Yönetilen pod kimlikler destekleyici Azure Hizmetleri karşı kimlik doğrulaması yapmak için harika çalışır. Kendi Hizmetleri veya Azure kaynakları için yönetilen kimlikleri olmadan uygulamalar için yine de kimlik bilgilerine veya anahtarlara kullanarak kimlik doğrulaması. Dijital bir kasa, bu kimlik bilgilerini depolamak için kullanılabilir.

Bunlar dijital kasa ile iletişim kurmak uygulamaların bir kimlik bilgisi gerektiğinde, en son kimlik bilgilerini almak ve daha sonra gerekli hizmete bağlanın. Azure Key Vault bu dijital kasası olabilir. Azure Key Vault'taki pod yönetilen kimliklerle bir kimlik bilgisi almak için basitleştirilmiş bir iş akışı aşağıdaki diyagramda gösterilmiştir:

![Yönetilen bir pod kullanarak Key Vault'tan bir kimlik bilgisi almak için basitleştirilmiş bir iş akışı kimliği](media/developer-best-practices-pod-security/basic-key-vault-flexvol.png)

Key Vault ile depolayın ve kimlik bilgilerini, depolama hesabı anahtarlarını veya sertifika gibi gizli dizileri düzenli olarak döndür. Azure Key Vault bir FlexVolume kullanarak bir AKS kümesi ile tümleştirebilirsiniz. FlexVolume sürücü yerel olarak kimlik Key Vault'tan almanız ve güvenli bir şekilde yalnızca isteyen pod'u sağlayacağını AKS kümesi sağlar. Anahtar kasası FlexVol sürücü AKS düğümleri üzerine dağıtmak için küme işleci ile çalışır. Pod yönetilen kimlik, anahtar kasası erişim istemek ve FlexVolume sürücüyü gereken kimlik bilgilerini almak için kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, pod'ların güvenli nasıl odaklanır. Bu alanlardan bazıları uygulamak için aşağıdaki makalelere bakın:

* [AKS ile Azure kaynakları için yönetilen kimlikleri kullanmak][aad-pod-identity]
* [AKS ile Azure anahtar kasası tümleştirme][aks-keyvault-flexvol]

<!-- EXTERNAL LINKS -->
[aad-pod-identity]: https://github.com/Azure/aad-pod-identity#demo-pod
[aks-keyvault-flexvol]: https://github.com/Azure/kubernetes-keyvault-flexvol
[linux-capabilities]: http://man7.org/linux/man-pages/man7/capabilities.7.html
[selinux-labels]: https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.12/#selinuxoptions-v1-core
[aks-associated-projects]: https://github.com/Azure/AKS/blob/master/previews.md#associated-projects

<!-- INTERNAL LINKS -->
[best-practices-cluster-security]: operator-best-practices-cluster-security.md
[best-practices-container-image-management]: operator-best-practices-container-image-management.md
[aks-pod-identities]: operator-best-practices-identity.md#use-pod-identities
[apparmor-seccomp]: operator-best-practices-cluster-security.md#secure-container-access-to-resources