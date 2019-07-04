---
title: Azure Kubernetes Service (AKS) pod güvenlik ilkelerini kullanma
description: Azure Kubernetes Service (AKS) PodSecurityPolicy kullanarak pod kullandılar denetlemeyi öğrenin
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 04/17/2019
ms.author: iainfou
ms.openlocfilehash: 9da722006651cfc9e9f2a175d5c330ba5df08123
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67447080"
---
# <a name="preview---secure-your-cluster-using-pod-security-policies-in-azure-kubernetes-service-aks"></a>Önizleme - pod güvenlik ilkelerini Azure Kubernetes Service (AKS) kullanarak kümenizin güvenliğini sağlama

AKS kümenizin güvenliğini artırmak için pod'ların olabilir sınırlayabilirsiniz zamanlandı. İzin verme kaynakları isteği pod'ları, AKS kümesinde çalıştırılamaz. Pod güvenlik ilkelerini kullanarak bu erişimi tanımlarsınız. Bu makalede aks'deki pod'ların dağıtımı sınırlamak için pod güvenlik ilkeleri kullanmayı gösterir.

> [!IMPORTANT]
> AKS Önizleme özellikleri, Self Servis, kabul etme. Görüş ve hata topluluğumuza toplamak için sağlanır. Önizleme'de, bu özelliklerin üretim kullanılmak üzere geliştirilmiş değildir. Genel Önizleme Özellikleri 'en yüksek çaba' destek kapsamında ayrılır. İş saatleri Pasifik Saat dilimi sırasında (Pasifik Saati) yalnızca AKS teknik destek ekipleri Yardım kullanılabilir. Ek bilgi için lütfen aşağıdaki destek makaleleri bakın:
>
> * [AKS destek ilkeleri][aks-support-policies]
> * [Azure desteği SSS][aks-faq]

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, var olan bir AKS kümesi olduğunu varsayar. AKS hızlı bir AKS kümesi gerekirse bkz [Azure CLI kullanarak][aks-quickstart-cli] or [using the Azure portal][aks-quickstart-portal].

Azure CLI Sürüm 2.0.61 gerekir veya daha sonra yüklü ve yapılandırılmış. Çalıştırma `az --version` sürümü bulmak için. Gerekirse yüklemek veya yükseltmek bkz [Azure CLI yükleme][install-azure-cli].

### <a name="install-aks-preview-cli-extension"></a>Aks önizlemesini CLI uzantısını yükleme

Pod güvenlik ilkelerini kullanmak için gerekir *aks önizlemesini* CLI 0.4.1 uzantı sürümü veya üzeri. Yükleme *aks önizlemesini* uzantısını Azure CLI kullanarak [az uzantısı ekleme][az-extension-add] command, then check for any available updates using the [az extension update][az-extension-update] komut::

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

### <a name="register-pod-security-policy-feature-provider"></a>Pod güvenlik ilkesi özellik sağlayıcısını Kaydet

Oluşturma veya güncelleştirme pod güvenlik ilkelerini kullanmak için bir AKS kümesi için önce aboneliğinizde özellik bayrağı etkinleştirin. Kaydedilecek *PodSecurityPolicyPreview* özellik bayrağı, kullanın [az özelliği kayıt][az-feature-register] komutu aşağıdaki örnekte gösterildiği gibi:

> [!CAUTION]
> Bir Abonelikteki bir özellik kaydettiğinizde, bu özellik şu anda kaydını yapamazsınız. Bazı Önizleme özellikleri etkinleştirdikten sonra varsayılan ardından aboneliği için oluşturulan tüm AKS kümeleri için kullanılabilir. Önizleme özellikleri üretim Aboneliklerde etkinleştirmeyin. Önizleme özellikleri test ve geri bildirim toplamak için ayrı bir abonelik kullanın.

```azurecli-interactive
az feature register --name PodSecurityPolicyPreview --namespace Microsoft.ContainerService
```

Gösterilecek durum için birkaç dakika sürer *kayıtlı*. Kayıt kullanarak durumu denetleyebilirsiniz [az özellik listesi][az-feature-list] komutu:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/PodSecurityPolicyPreview')].{Name:name,State:properties.state}"
```

Hazır olduğunuzda, kayıt yenileme *Microsoft.ContainerService* kullanarak kaynak sağlayıcısını [az provider register][az-provider-register] komutu:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="overview-of-pod-security-policies"></a>Pod güvenlik ilkelerine genel bakış

İçindeki bir Kubernetes kümesi, bir kaynak oluşturulacak olduğunda API sunucusu için isteklerin kesilmesi için giriş denetleyicisini kullanılır. Giriş denetleyicisine böylece *doğrulamak* kuralları, bir dizi kaynak isteğiyle veya *bulunmamalıdır* kaynağın dağıtım parametrelerini değiştirin.

*PodSecurityPolicy* pod belirtimi doğrulayan bir giriş denetleyicisine tanımlanmış gereksinimlerinizi karşılayan olduğu. Bu gereksinimler, ayrıcalıklı kapsayıcıları sınırlandırmak, belirli türde depolama veya kullanıcı veya grubun kapsayıcı olarak çalıştırabilirsiniz erişim. Pod belirtimleri pod güvenlik ilkesinde belirtilen gereksinimleri burada uymayan bir kaynak dağıtma çalıştığınızda, istek reddedilir. Bu denetleyebilme pod'ların olabilir zamanlanmış AKS kümesi bazı olası güvenlik açıklarını veya ayrıcalık yükseltmeleri engeller.

Bir AKS kümesindeki pod güvenlik ilkesini etkinleştirmek, bazı varsayılan ilkeleri uygulanır. Bu varsayılan ilkeler ne pod'ları tanımlamak için bir giden kutusu deneyimi zamanlanabilir sağlar. Ancak, küme kullanıcılar kendi ilkelerinizi tanımladığınız kadar pod'ların dağıtımı sorunlarla karşılaşırsanız çalıştırabilirsiniz. İçin önerilen yaklaşım şöyledir:

* AKS kümesi oluşturma
* Kendi pod güvenlik ilkelerini tanımlama
* Pod güvenlik ilkesi özelliğini etkinleştir

Gösterilecek varsayılan ilkeler pod dağıtımları sınırlamak nasıl bu makaledeki biz ilk pod güvenlik ilkeleri özelliğini etkinleştirir ve ardından özel bir ilke oluşturun.

## <a name="enable-pod-security-policy-on-an-aks-cluster"></a>AKS kümesinde pod güvenlik ilkesini etkinleştirmek

Etkinleştirmek ya da pod güvenlik ilkesi kullanılarak devre dışı [az aks güncelleştirme][az-aks-update] komutu. Aşağıdaki örnek etkinleştirir pod güvenlik ilkesi küme adı *myAKSCluster* adlı kaynak grubunda *myResourceGroup*.

> [!NOTE]
> Kendi özel ilkelerinizi tanımladığınız kadar gerçek kullanım için pod güvenlik ilkesini etkinleştirmeyin. Bu makalede, ilk adım olarak nasıl pod varsayılan ilkelerini sınırlamak görmek için pod güvenlik ilkesini etkinleştirmek dağıtımları.

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --enable-pod-security-policy
```

## <a name="default-aks-policies"></a>Varsayılan AKS ilkeleri

Pod güvenlik ilkesini etkinleştirmek, AKS adlı iki varsayılan ilke oluşturur *ayrıcalıklı* ve *kısıtlı*. Yoksa, düzenlemek veya bu varsayılan ilkelerini kaldırın. Bunun yerine, istediğiniz ayarları denetimi tanımlayan kendi ilkeleri oluşturun. Şimdi bu varsayılan ilkeler ilk göz olan bunların pod dağıtımları nasıl etkilediği.

Kullanılabilir ilkeleri görüntülemek için kullanın [kubectl alma psp][kubectl-get] , aşağıdaki örnekte gösterildiği gibi komutu. Varsayılan bir parçası olarak *kısıtlı* İlkesi, kullanıcı reddedildi *PRIV* ayrıcalıklı pod yükseltme ve kullanıcı kullanmak *MustRunAsNonRoot*.

```console
$ kubectl get psp

NAME         PRIV    CAPS   SELINUX    RUNASUSER          FSGROUP     SUPGROUP    READONLYROOTFS   VOLUMES
privileged   true    *      RunAsAny   RunAsAny           RunAsAny    RunAsAny    false            *
restricted   false          RunAsAny   MustRunAsNonRoot   MustRunAs   MustRunAs   false            configMap,emptyDir,projected,secret,downwardAPI,persistentVolumeClaim
```

*Kısıtlı* pod güvenlik ilkesi, AKS kümesinde herhangi bir kimliği doğrulanmış kullanıcı için uygulanır. Bu atama ClusterRoles ve ClusterRoleBindings tarafından denetlenir. Kullanım [kubectl alma clusterrolebindings][kubectl-get] komut ve arama *varsayılan: kısıtlı:* bağlama:

```console
kubectl get clusterrolebindings default:restricted -o yaml
```

Aşağıdaki sıkıştırılmış çıktıda gösterildiği gibi *psp: kısıtlı* ClusterRole birine atanır *sistem: kimliği doğrulanmış* kullanıcılar. Bu yetenek kısıtlamaları temel bir düzeyde tanımlanan kendi ilkelerinizi sağlar.

```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  [...]
  name: default:restricted
  [...]
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: psp:restricted
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: Group
  name: system:authenticated
```

Bu varsayılan ilkeler kendi pod güvenlik ilkeleri oluşturmaya başlamadan önce pod'ların zamanlamak için kullanıcı isteklerini ile nasıl etkileşimde bulunduğunu anlamak önemlidir. Sonraki birkaç bölümde, şimdi bu varsayılan ilkeler iş başında görmek için bazı pod'ları zamanlayın.

## <a name="create-a-test-user-in-an-aks-cluster"></a>Bir AKS kümesinde bir test kullanıcısı oluşturun

Varsayılan olarak kullandığınızda, [az aks get-credentials][az-aks-get-credentials] komutu *yönetici* AKS kümesi için kimlik bilgileri ve eklenir, `kubectl` yapılandırma. Yönetici kullanıcı, pod güvenlik ilkelerinin uygulanması atlar. AKS kümeleriniz için Azure Active Directory Tümleştirmesi kullanırsanız, eylem ilkesini zorlama görmek için bir yönetici olmayan kullanıcı kimlik bilgileriyle oturum. Bu makalede, bir test kullanıcı hesabı kullanarak bir AKS kümesi oluşturalım.

Adlandırılmış bir örnek ad alanı oluşturma *psp aks* kullanarak test kaynakları için [kubectl ad alanı oluşturma][kubectl-create] komutu. Adlı bir hizmet hesabı oluşturup *nonadmin kullanıcı* kullanarak [kubectl oluşturma serviceaccount][kubectl-create] komutu:

```console
kubectl create namespace psp-aks
kubectl create serviceaccount --namespace psp-aks nonadmin-user
```

Ardından, bir RoleBinding için oluşturma *nonadmin kullanıcı* ad alanını kullanarak temel Eylemler gerçekleştirileceğini [kubectl oluşturma rolebinding][kubectl-create] komutu:

```console
kubectl create rolebinding \
    --namespace psp-aks \
    psp-aks-editor \
    --clusterrole=edit \
    --serviceaccount=psp-aks:nonadmin-user
```

### <a name="create-alias-commands-for-admin-and-non-admin-user"></a>Yönetici ve yönetici olmayan kullanıcı için diğer ad komutları oluşturma

Normal bir yönetici kullanıcıyı kullanırken arasındaki fark vurgulamak için `kubectl` ve önceki adımlarda oluşturduğunuz yönetici olmayan kullanıcıların oluşturacağı iki komut satırı diğer adları:

* **Kubectl yönetici** diğer adı için normal bir yönetici kullanıcı ve kapsamı *psp aks* ad alanı.
* **Kubectl nonadminuser** diğer ad olduğu için *nonadmin kullanıcı* önceki adımda oluşturduğunuz ve kapsamı *psp aks* ad alanı.

Bu iki diğer adlar, aşağıdaki komutlarda gösterildiği gibi oluşturun:

```console
alias kubectl-admin='kubectl --namespace psp-aks'
alias kubectl-nonadminuser='kubectl --as=system:serviceaccount:psp-aks:nonadmin-user --namespace psp-aks'
```

## <a name="test-the-creation-of-a-privileged-pod"></a>Test etme ayrıcalıklı bir pod oluşturma

Şimdi ilk güvenlik bağlamı ile birlikte bir pod zamanladığınızda ne test `privileged: true`. Bu güvenlik bağlamı pod's ayrıcalıkları iletir. Güvenlik ilkeleri, varsayılan AKS pod gösterdi önceki bölümde *kısıtlı* İlkesi bu isteği reddetmek.

Adlı bir dosya oluşturun `nginx-privileged.yaml` aşağıdaki YAML bildirimi yapıştırın:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-privileged
spec:
  containers:
    - name: nginx-privileged
      image: nginx:1.14.2
      securityContext:
        privileged: true
```

Pod kullanarak oluşturduğunuz [kubectl uygulamak][kubectl-apply] komut ve YAML bildiriminizi adını belirtin:

```console
kubectl-nonadminuser apply -f nginx-privileged.yaml
```

Aşağıdaki örnek çıktıda gösterildiği gibi zamanlanması pod başarısız olur:

```console
$ kubectl-nonadminuser apply -f nginx-privileged.yaml

Error from server (Forbidden): error when creating "nginx-privileged.yaml": pods "nginx-privileged" is forbidden: unable to validate against any pod security policy: [spec.containers[0].securityContext.privileged: Invalid value: true: Privileged containers are not allowed]
```

Pod planlama aşaması ulaşmaz, vardır, bu nedenle, geçmeden önce silmek için kaynak yok.

## <a name="test-creation-of-an-unprivileged-pod"></a>Ayrıcalıksız bir pod test oluşturma

Önceki örnekte, pod belirtimi ayrıcalık istedi. Varsayılan olarak bu isteği reddedildi *kısıtlı* pod güvenlik ilkesi, zamanlanmış pod başarısız olur. Artık, aynı NGINX pod ayrıcalık yükseltme isteği olmadan çalışan deneyelim.

Adlı bir dosya oluşturun `nginx-unprivileged.yaml` aşağıdaki YAML bildirimi yapıştırın:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-unprivileged
spec:
  containers:
    - name: nginx-unprivileged
      image: nginx:1.14.2
```

Pod kullanarak oluşturduğunuz [kubectl uygulamak][kubectl-apply] komut ve YAML bildiriminizi adını belirtin:

```console
kubectl-nonadminuser apply -f nginx-unprivileged.yaml
```

Kubernetes Zamanlayıcı pod isteği kabul eder. Ancak, pod kullanarak durum baktığınızda `kubectl get pods`, bir hata oluştu:

```console
$ kubectl-nonadminuser get pods

NAME                 READY   STATUS                       RESTARTS   AGE
nginx-unprivileged   0/1     CreateContainerConfigError   0          26s
```

Kullanım [kubectl açıklayan pod][kubectl-describe] pod olayları görmek için komutu. Biz istememiş olsa da aşağıdaki sıkıştırılmış örneğe kapsayıcı ve görüntü kök izinleri gerektiren gösterir:

```console
$ kubectl-nonadminuser describe pod nginx-unprivileged

Name:               nginx-unprivileged
Namespace:          psp-aks
Priority:           0
PriorityClassName:  <none>
Node:               aks-agentpool-34777077-0/10.240.0.4
Start Time:         Thu, 28 Mar 2019 22:05:04 +0000
[...]
Events:
  Type     Reason     Age                     From                               Message
  ----     ------     ----                    ----                               -------
  Normal   Scheduled  7m14s                   default-scheduler                  Successfully assigned psp-aks/nginx-unprivileged to aks-agentpool-34777077-0
  Warning  Failed     5m2s (x12 over 7m13s)   kubelet, aks-agentpool-34777077-0  Error: container has runAsNonRoot and image will run as root
  Normal   Pulled     2m10s (x25 over 7m13s)  kubelet, aks-agentpool-34777077-0  Container image "nginx:1.14.2" already present on machine
```

Biz herhangi bir ayrıcalıklı erişim talep etmediyseniz olsa da, NGINX için kapsayıcı görüntüsü bağlama bağlantı noktası oluşturmak için ihtiyaç duyduğu *80*. Bağlantı noktaları bağlamak için *1024* ve aşağıdaki *kök* kullanıcı gereklidir. Pod'u başlatmak çalıştığında *kısıtlı* pod güvenlik ilkesi bu isteği reddeder.

Bu örnek, AKS tarafından oluşturulan varsayılan pod güvenlik ilkelerinin geçerli olduğu ve bir kullanıcının gerçekleştirebileceği işlemleri kısıtlamak gösterir. Engellenmesi için temel bir NGINX pod beklediğiniz değil gibi bu varsayılan ilkeler davranışını anlamak önemlidir.

Pod kullanarak bu test, sonraki adıma geçmeden önce silme [kubectl Sil pod][kubectl-delete] komutu:

```console
kubectl-nonadminuser delete -f nginx-unprivileged.yaml
```

## <a name="test-creation-of-a-pod-with-a-specific-user-context"></a>Belirli kullanıcı bağlamı ile birlikte bir pod test oluşturma

Önceki örnekte, kök NGINX 80 numaralı bağlantı noktasına bağlamak için kullanılacak kapsayıcı görüntüsünü otomatik olarak çalıştı. Varsayılan olarak bu isteği reddedildi *kısıtlı* pod güvenlik ilkesi pod'u başlatmak başarısız olur. Artık, aynı NGINX pod gibi belirli kullanıcı bağlamı ile çalışan deneyelim `runAsUser: 2000`.

Adlı bir dosya oluşturun `nginx-unprivileged-nonroot.yaml` aşağıdaki YAML bildirimi yapıştırın:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-unprivileged-nonroot
spec:
  containers:
    - name: nginx-unprivileged
      image: nginx:1.14.2
      securityContext:
        runAsUser: 2000
```

Pod kullanarak oluşturduğunuz [kubectl uygulamak][kubectl-apply] komut ve YAML bildiriminizi adını belirtin:

```console
kubectl-nonadminuser apply -f nginx-unprivileged-nonroot.yaml
```

Kubernetes Zamanlayıcı pod isteği kabul eder. Ancak, pod kullanarak durum baktığınızda `kubectl get pods`, önceki örnekten farklı bir hata oluştu:

```console
$ kubectl-nonadminuser get pods

NAME                         READY   STATUS              RESTARTS   AGE
nginx-unprivileged-nonroot   0/1     CrashLoopBackOff    1          3s
```

Kullanım [kubectl açıklayan pod][kubectl-describe] pod olayları görmek için komutu. Aşağıdaki sıkıştırılmış örneğe pod olayları gösterir:

```console
$ kubectl-nonadminuser describe pods nginx-unprivileged

Name:               nginx-unprivileged
Namespace:          psp-aks
Priority:           0
PriorityClassName:  <none>
Node:               aks-agentpool-34777077-0/10.240.0.4
Start Time:         Thu, 28 Mar 2019 22:05:04 +0000
[...]
Events:
  Type     Reason     Age                   From                               Message
  ----     ------     ----                  ----                               -------
  Normal   Scheduled  2m14s                 default-scheduler                  Successfully assigned psp-aks/nginx-unprivileged-nonroot to aks-agentpool-34777077-0
  Normal   Pulled     118s (x3 over 2m13s)  kubelet, aks-agentpool-34777077-0  Container image "nginx:1.14.2" already present on machine
  Normal   Created    118s (x3 over 2m13s)  kubelet, aks-agentpool-34777077-0  Created container
  Normal   Started    118s (x3 over 2m12s)  kubelet, aks-agentpool-34777077-0  Started container
  Warning  BackOff    105s (x5 over 2m11s)  kubelet, aks-agentpool-34777077-0  Back-off restarting failed container
```

Olaylar, kapsayıcı oluşturulur ve başlatılmış olduğunu gösterir. Pod başarısız durumda olmadığı neden hemen açık bir şey yoktur. Pod günlükleri kullanarak bakalım [kubectl günlükleri][kubectl-logs] komutu:

```console
kubectl-nonadminuser logs nginx-unprivileged-nonroot --previous
```

Aşağıdaki örnek günlük çıktısını gittiğinin belirtisini, NGINX yapılandırmasında kendisi içinde verir, hizmet başlatmaya çalıştığında bir izin hatasıyla olur. Bu hata, 80 numaralı bağlantı noktasına bağlamak gerek tarafından yeniden kaynaklanır. Normal bir kullanıcı hesabı pod tanımlanan olsa da, bu kullanıcı hesabı işletim sistemi başlatmak ve Kısıtlı bağlantı noktasına bağlamak için düzeyi NGINX hizmeti için yeterli değil.

```console
$ kubectl-nonadminuser logs nginx-unprivileged-nonroot --previous

2019/03/28 22:38:29 [warn] 1#1: the "user" directive makes sense only if the master process runs with super-user privileges, ignored in /etc/nginx/nginx.conf:2
nginx: [warn] the "user" directive makes sense only if the master process runs with super-user privileges, ignored in /etc/nginx/nginx.conf:2
2019/03/28 22:38:29 [emerg] 1#1: mkdir() "/var/cache/nginx/client_temp" failed (13: Permission denied)
nginx: [emerg] mkdir() "/var/cache/nginx/client_temp" failed (13: Permission denied)
```

Yeniden varsayılan pod güvenlik ilkeleri davranışını anlamak önemlidir. Bu hata izlemek biraz daha zor ve yeniden engellenmesi için temel bir NGINX pod beklenenden değil.

Pod kullanarak bu test, sonraki adıma geçmeden önce silme [kubectl Sil pod][kubectl-delete] komutu:

```console
kubectl-nonadminuser delete -f nginx-unprivileged-nonroot.yaml
```

## <a name="create-a-custom-pod-security-policy"></a>Bir özel pod güvenlik ilkesi oluşturma

Varsayılan pod güvenlik ilkeleri davranışını gördüğünüze göre şimdi bir yol sağlamak *nonadmin kullanıcı* başarıyla pod'ları zamanlama.

Ayrıcalıklı erişim isteğinde pod'ların reddetmek için bir ilke oluşturalım. Gibi diğer seçenekleri *farklıkullanıcı* veya izin *birimleri*, açıkça sınırlı değildir. Bu ilke türü, ayrıcalıklı erişim isteği reddeder, ancak Aksi takdirde istenen pod'ları çalıştırmak kümesi sağlar.

Adlı bir dosya oluşturun `psp-deny-privileged.yaml` aşağıdaki YAML bildirimi yapıştırın:

```yaml
apiVersion: policy/v1beta1
kind: PodSecurityPolicy
metadata:
  name: psp-deny-privileged
spec:
  privileged: false
  seLinux:
    rule: RunAsAny
  supplementalGroups:
    rule: RunAsAny
  runAsUser:
    rule: RunAsAny
  fsGroup:
    rule: RunAsAny
  volumes:
  - '*'
```

İlke kullanarak oluşturduğunuz [kubectl uygulamak][kubectl-apply] komut ve YAML bildiriminizi adını belirtin:

```console
kubectl apply -f psp-deny-privileged.yaml
```

Kullanılabilir ilkeleri görüntülemek için kullanın [kubectl alma psp][kubectl-get] , aşağıdaki örnekte gösterildiği gibi komutu. Karşılaştırma *psp Reddet ayrıcalıklı* varsayılan ilkesiyle *kısıtlı* bir pod oluşturmak için önceki örneklerde zorlandı ilkesi. Sadece kullanımını *PRIV* yükseltme, ilke tarafından reddedildi. Kullanıcı veya grup için hiçbir kısıtlama vardır *psp Reddet ayrıcalıklı* ilkesi.

```console
$ kubectl get psp

NAME                  PRIV    CAPS   SELINUX    RUNASUSER          FSGROUP     SUPGROUP    READONLYROOTFS   VOLUMES
privileged            true    *      RunAsAny   RunAsAny           RunAsAny    RunAsAny    false            *
psp-deny-privileged   false          RunAsAny   RunAsAny           RunAsAny    RunAsAny    false            *
restricted            false          RunAsAny   MustRunAsNonRoot   MustRunAs   MustRunAs   false            configMap,emptyDir,projected,secret,downwardAPI,persistentVolumeClaim
```

## <a name="allow-user-account-to-use-the-custom-pod-security-policy"></a>Özel pod Güvenlik İlkesi kullanmak kullanıcı hesabına izin ver

Önceki adımda isteği ayrıcalıklı erişimin pod'ların reddetmek için bir pod güvenlik ilkesi oluşturdunuz. Kullanılacak ilkeyi izin vermek için oluşturduğunuz bir *rol* veya *ClusterRole*. Bu rollerden kullanarak daha sonra ilişkilendirmek bir *RoleBinding* veya *ClusterRoleBinding*.

Bu örnekte, izin veren bir ClusterRole oluşturma *kullanın* *psp Reddet ayrıcalıklı* önceki adımda oluşturduğunuz ilke. Adlı bir dosya oluşturun `psp-deny-privileged-clusterrole.yaml` aşağıdaki YAML bildirimi yapıştırın:

```yaml
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: psp-deny-privileged-clusterrole
rules:
- apiGroups:
  - extensions
  resources:
  - podsecuritypolicies
  resourceNames:
  - psp-deny-privileged
  verbs:
  - use
```

ClusterRole kullanarak oluşturduğunuz [kubectl uygulamak][kubectl-apply] komut ve YAML bildiriminizi adını belirtin:

```console
kubectl apply -f psp-deny-privileged-clusterrole.yaml
```

Şimdi önceki adımda oluşturduğunuz ClusterRole kullanmak için bir ClusterRoleBinding oluşturun. Adlı bir dosya oluşturun `psp-deny-privileged-clusterrolebinding.yaml` aşağıdaki YAML bildirimi yapıştırın:

```yaml
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: psp-deny-privileged-clusterrolebinding
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: psp-deny-privileged-clusterrole
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: Group
  name: system:serviceaccounts
```

Kullanarak bir ClusterRoleBinding oluşturma [kubectl uygulamak][kubectl-apply] komut ve YAML bildiriminizi adını belirtin:

```console
kubectl apply -f psp-deny-privileged-clusterrolebinding.yaml
```

> [!NOTE]
> Bu makalede, ilk adımda AKS kümesinde pod güvenlik ilkesi özelliği etkinleştirildi. Kendi ilkelerinizi tanımladıktan sonra yalnızca pod güvenlik ilkesi özelliğini etkinleştirmek için önerilen uygulama oldu. Burada pod güvenlik ilkesi özelliği etkinleştirme aşaması budur. Bir veya daha fazla özel ilkeler tanımlanan ve kullanıcı hesaplarını bu ilkeleri ile ilişkili olan. Güvenli bir şekilde pod güvenlik ilkesi artık özelliği ve varsayılan ilkelerden neden olduğu sorunları en aza indirin.

## <a name="test-the-creation-of-an-unprivileged-pod-again"></a>Ayrıcalıksız bir pod yeniden oluşturulmasını test

Uygulanan özel pod güvenlik ilkeniz ve ilkeyi kullanmak için kullanıcı hesabı için bir bağlama ile ayrıcalıksız bir pod oluşturmayı yeniden deneyelim. Aynı `nginx-privileged.yaml` kullanılarak pod oluşturmak için bildirim [kubectl uygulamak][kubectl-apply] komutu:

```console
kubectl-nonadminuser apply -f nginx-unprivileged.yaml
```

Pod başarıyla zamanlandı. Kullanarak pod durumunu iade [kubectl pod'ları alma][kubectl-get] komutunu pod olan *çalıştıran*:

```
$ kubectl-nonadminuser get pods

NAME                 READY   STATUS    RESTARTS   AGE
nginx-unprivileged   1/1     Running   0          7m14s
```

Bu örnek, AKS kümesini farklı kullanıcılar veya gruplar için erişimi tanımlamak için özel pod güvenlik ilkeleri nasıl oluşturabileceğinizi gösterir. Hangi pod'ların sıkı denetimlerini çalıştırabilirsiniz, AKS ilkeleri sağlar. varsayılan, bu nedenle doğru ihtiyacınız kısıtlamaları tanımlamak için kendi özel ilkelerinizi oluşturun.

NGINX ayrıcalıksız kullanılarak pod Sil [kubectl Sil][kubectl-delete] komut ve YAML bildiriminizi adını belirtin:

```console
kubectl-nonadminuser delete -f nginx-unprivileged.yaml
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Pod güvenlik ilkesini devre dışı bırakmak için [az aks güncelleştirme][az-aks-update] yeniden komutu. Aşağıdaki örnek devre dışı bırakır pod güvenlik ilkesi küme adı *myAKSCluster* adlı kaynak grubunda *myResourceGroup*:

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --disable-pod-security-policy
```

Ardından, ClusterRole ve ClusterRoleBinding silin:

```console
kubectl delete -f psp-deny-privileged-clusterrolebinding.yaml
kubectl delete -f psp-deny-privileged-clusterrole.yaml
```

Ağ İlkesi kullanarak silme [kubectl Sil][kubectl-delete] komut ve YAML bildiriminizi adını belirtin:

```console
kubectl delete -f psp-deny-privileged.yaml
```

Son olarak, Sil *psp aks* ad alanı:

```console
kubectl delete namespace psp-aks
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede bir pod Güvenlik İlkesi'ayrıcalıklı erişim kullanılmasını önlemek için nasıl oluşturulacağını gösterir. Çok sayıda bir ilke uygulayabilir, birim veya RunAs kullanıcısının türünü gibi özellikleri vardır. Kullanılabilir seçenekler hakkında daha fazla bilgi için bkz. [Kubernetes pod güvenlik ilkesi başvuru belgeleri][kubernetes-policy-reference].

Pod ağ trafiğini sınırlandırma hakkında daha fazla bilgi için bkz. [güvenli ağ ilkeleri kullanarak AKS pod'ları arasındaki trafiği][network-policies].

<!-- LINKS - external -->
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-logs]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
[kubernetes-policy-reference]: https://kubernetes.io/docs/concepts/policy/pod-security-policy/#policy-reference

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[network-policies]: use-network-policies.md
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[az-aks-update]: /cli/azure/ext/aks-preview/aks#ext-aks-preview-az-aks-update
[az-extension-add]: /cli/azure/extension#az-extension-add
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
