---
title: Azure - küme Ölçeklendiriciyi azure'da Kubernetes
description: Küme otomatik ölçeklendiricinin kümenizi talebi karşılamak üzere otomatik olarak ölçeklendirmek için Azure Kubernetes Service (AKS) kullanmayı öğrenin.
services: container-service
author: sakthivetrivel
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/19/18
ms.author: sakthivetrivel
ms.custom: mvc
ms.openlocfilehash: 3bac6534f43d62e6eb9381b8513025ba9117ed04
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2018
ms.locfileid: "48857015"
---
# <a name="cluster-autoscaler-on-azure-kubernetes-service-aks---preview"></a>Otomatik Ölçeklendiricinin küme Azure Kubernetes hizmeti üzerinde (AKS) - Önizleme

Azure Kubernetes Service (AKS), azure'da yönetilen bir Kubernetes kümesi dağıtmak için esnek bir çözümdür. Kaynak olarak taleplerini artırmak, küme ölçeklendiriciyi kümenizi ayarladığınız kısıtlamalarına göre bu talebi karşılamak üzere büyümesine izin verir. Küme otomatik ölçeklendiricinin (CA) aracısı düğümlerinizi pod'ların göre ölçeklendirme tarafından yapar. Bu, düzenli aralıklarla pod'ların ya da boş düğümleri denetlemek için küme tarar ve mümkünse boyutu artar. Varsayılan olarak, CA tarar pod'ların 10 saniyede ve ise bir düğümü kaldırır 10 dakikadan fazla kapatın. İle kullanıldığında [yatay pod otomatik ölçeklendiricinin](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) (HPA) HPA pod çoğaltmalarının ve kaynakları isteğe göre güncelleştirir. Yeterli düğümleri ya da gereksiz düğümleri bu pod ölçeklendirmeyi aşağıdaki değilseniz, CA yanıt ve yeni düğüm kümesindeki pod'ları zamanlayın.

Bu makalede, aracı düğümlerdeki Küme ölçeklendiriciyi dağıtmayı açıklar. Küme otomatik ölçeklendiricinin kube-system ad alanında dağıtılmış olduğundan, ancak ölçeklendirici, pod çalıştıran düğümü ölçeği.

> [!IMPORTANT]
> Azure Kubernetes Service (AKS) kümesi ölçeklendiriciyi tümleştirmesi, şu anda **Önizleme**. Önizlemeler, [ek kullanım koşullarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.
>

## <a name="prerequisites"></a>Önkoşullar

Bu belge, RBAC özellikli bir AKS kümesi olduğunu varsayar. Bir AKS kümesi gerekirse bkz [Azure Kubernetes Service (AKS) hızlı başlangıç][aks-quick-start].

 Küme ölçeklendiriciyi kullanmak için kümenizi Kubernetes v1.10.X kullanarak veya üzeri ve RBAC etkinleştirilmiş olması gerekir. Kümenizi yükseltmek için makaleye bakın [AKS kümesini yükseltme][aks-upgrade].

## <a name="gather-information"></a>Bilgi toplayın

Kümenizde çalıştırmak, küme ölçeklendiriciyi izinlerini oluşturmak için bu bash betiğini çalıştırın:

```sh
#! /bin/bash
ID=`az account show --query id -o json`
SUBSCRIPTION_ID=`echo $ID | tr -d '"' `

TENANT=`az account show --query tenantId -o json`
TENANT_ID=`echo $TENANT | tr -d '"' | base64`

read -p "What's your cluster name? " cluster_name
read -p "Resource group name? " resource_group

CLUSTER_NAME=`echo $cluster_name | base64`
RESOURCE_GROUP=`echo $resource_group | base64`

PERMISSIONS=`az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/$SUBSCRIPTION_ID" -o json`
CLIENT_ID=`echo $PERMISSIONS | sed -e 's/^.*"appId"[ ]*:[ ]*"//' -e 's/".*//' | base64`
CLIENT_SECRET=`echo $PERMISSIONS | sed -e 's/^.*"password"[ ]*:[ ]*"//' -e 's/".*//' | base64`

SUBSCRIPTION_ID=`echo $ID | tr -d '"' | base64 `

NODE_RESOURCE_GROUP=`az aks show --name $cluster_name  --resource-group $resource_group -o tsv --query 'nodeResourceGroup' | base64`

echo "---
apiVersion: v1
kind: Secret
metadata:
    name: cluster-autoscaler-azure
    namespace: kube-system
data:
    ClientID: $CLIENT_ID
    ClientSecret: $CLIENT_SECRET
    ResourceGroup: $RESOURCE_GROUP
    SubscriptionID: $SUBSCRIPTION_ID
    TenantID: $TENANT_ID
    VMType: QUtTCg==
    ClusterName: $CLUSTER_NAME
    NodeResourceGroup: $NODE_RESOURCE_GROUP
---"
```

Betik adımları uyguladıktan sonra komut dosyası biçiminde bir gizli dizi ayrıntılarınızı çıkarır şu şekilde:

```yaml
---
apiVersion: v1
kind: Secret
metadata:
  name: cluster-autoscaler-azure
  namespace: kube-system
data:
  ClientID: <base64-encoded-client-id>
  ClientSecret: <base64-encoded-client-secret>$
  ResourceGroup: <base64-encoded-resource-group>  SubscriptionID: <base64-encode-subscription-id>
  TenantID: <base64-encoded-tenant-id>
  VMType: QUtTCg==
  ClusterName: <base64-encoded-clustername>
  NodeResourceGroup: <base64-encoded-node-resource-group>
---
```

Ardından, aşağıdaki komutu çalıştırarak, düğüm havuzu adını alın. 

```console
$ kubectl get nodes --show-labels
```

Çıktı:

```console
NAME                       STATUS    ROLES     AGE       VERSION   LABELS
aks-nodepool1-37756013-0   Ready     agent     1h        v1.10.3   agentpool=nodepool1,beta.kubernetes.io/arch=amd64,beta.kubernetes.io/instance-type=Standard_DS1_v2,beta.kubernetes.io/os=linux,failure-domain.beta.kubernetes.io/region=eastus,failure-domain.beta.kubernetes.io/zone=0,kubernetes.azure.com/cluster=MC_[resource-group]\_[cluster-name]_[location],kubernetes.io/hostname=aks-nodepool1-37756013-0,kubernetes.io/role=agent,storageprofile=managed,storagetier=Premium_LRS
 ```

Ardından, etiketin değeri ayıklayın **agentpool**. "Nodepool1" bir kümenin düğüm havuzu için varsayılan addır.

Gizli dizi ve düğüm havuzunuz kullanarak artık dağıtım grafik oluşturabilirsiniz.

## <a name="create-a-deployment-chart"></a>Bir dağıtım grafik oluşturma

Adlı bir dosya oluşturun `aks-cluster-autoscaler.yaml`ve dosyayı aşağıdaki YAML koduna kopyalayın.

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  labels:
    k8s-addon: cluster-autoscaler.addons.k8s.io
    k8s-app: cluster-autoscaler
  name: cluster-autoscaler
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRole
metadata:
  name: cluster-autoscaler
  labels:
    k8s-addon: cluster-autoscaler.addons.k8s.io
    k8s-app: cluster-autoscaler
rules:
- apiGroups: [""]
  resources: ["events","endpoints"]
  verbs: ["create", "patch"]
- apiGroups: [""]
  resources: ["pods/eviction"]
  verbs: ["create"]
- apiGroups: [""]
  resources: ["pods/status"]
  verbs: ["update"]
- apiGroups: [""]
  resources: ["endpoints"]
  resourceNames: ["cluster-autoscaler"]
  verbs: ["get","update"]
- apiGroups: [""]
  resources: ["nodes"]
  verbs: ["watch","list","get","update"]
- apiGroups: [""]
  resources: ["pods","services","replicationcontrollers","persistentvolumeclaims","persistentvolumes"]
  verbs: ["watch","list","get"]
- apiGroups: ["extensions"]
  resources: ["replicasets","daemonsets"]
  verbs: ["watch","list","get"]
- apiGroups: ["policy"]
  resources: ["poddisruptionbudgets"]
  verbs: ["watch","list"]
- apiGroups: ["apps"]
  resources: ["statefulsets"]
  verbs: ["watch","list","get"]
- apiGroups: ["storage.k8s.io"]
  resources: ["storageclasses"]
  verbs: ["get", "list", "watch"]

---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: Role
metadata:
  name: cluster-autoscaler
  namespace: kube-system
  labels:
    k8s-addon: cluster-autoscaler.addons.k8s.io
    k8s-app: cluster-autoscaler
rules:
- apiGroups: [""]
  resources: ["configmaps"]
  verbs: ["create"]
- apiGroups: [""]
  resources: ["configmaps"]
  resourceNames: ["cluster-autoscaler-status"]
  verbs: ["delete","get","update"]

---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: cluster-autoscaler
  labels:
    k8s-addon: cluster-autoscaler.addons.k8s.io
    k8s-app: cluster-autoscaler
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-autoscaler
subjects:
  - kind: ServiceAccount
    name: cluster-autoscaler
    namespace: kube-system

---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: RoleBinding
metadata:
  name: cluster-autoscaler
  namespace: kube-system
  labels:
    k8s-addon: cluster-autoscaler.addons.k8s.io
    k8s-app: cluster-autoscaler
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: cluster-autoscaler
subjects:
  - kind: ServiceAccount
    name: cluster-autoscaler
    namespace: kube-system

---
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  labels:
    app: cluster-autoscaler
  name: cluster-autoscaler
  namespace: kube-system
spec:
  replicas: 1
  selector:
    matchLabels:
      app: cluster-autoscaler
  template:
    metadata:
      labels:
        app: cluster-autoscaler
    spec:
      serviceAccountName: cluster-autoscaler
      containers:
      - image: gcr.io/google-containers/cluster-autoscaler:v1.2.2
        imagePullPolicy: Always
        name: cluster-autoscaler
        resources:
          limits:
            cpu: 100m
            memory: 300Mi
          requests:
            cpu: 100m
            memory: 300Mi
        command:
        - ./cluster-autoscaler
        - --v=3
        - --logtostderr=true
        - --cloud-provider=azure
        - --skip-nodes-with-local-storage=false
        - --nodes=1:10:nodepool1
        env:
        - name: ARM_SUBSCRIPTION_ID
          valueFrom:
            secretKeyRef:
              key: SubscriptionID
              name: cluster-autoscaler-azure
        - name: ARM_RESOURCE_GROUP
          valueFrom:
            secretKeyRef:
              key: ResourceGroup
              name: cluster-autoscaler-azure
        - name: ARM_TENANT_ID
          valueFrom:
            secretKeyRef:
              key: TenantID
              name: cluster-autoscaler-azure
        - name: ARM_CLIENT_ID
          valueFrom:
            secretKeyRef:
              key: ClientID
              name: cluster-autoscaler-azure
        - name: ARM_CLIENT_SECRET
          valueFrom:
            secretKeyRef:
              key: ClientSecret
              name: cluster-autoscaler-azure
        - name: ARM_VM_TYPE
          valueFrom:
            secretKeyRef:
              key: VMType
              name: cluster-autoscaler-azure
        - name: AZURE_CLUSTER_NAME
          valueFrom:
            secretKeyRef:
              key: ClusterName
              name: cluster-autoscaler-azure
        - name: AZURE_NODE_RESOURCE_GROUP
          valueFrom:
            secretKeyRef:
              key: NodeResourceGroup
              name: cluster-autoscaler-azure
      restartPolicy: Always
```

Kopyalama önceki adımda oluşturduğunuz parolayı yapıştırın ve dosyanın başında ekleyin.

Ardından, düğümleri aralığını ayarlamak için bağımsız değişken için doldurun `--nodes` altında `command` MIN:MAX:NODE_POOL_NAME biçiminde. Örneğin: `--nodes=3:10:nodepool1` düğüm sayısı alt sınırı 3, 10 düğümleri ve nodepool1 düğüm havuzu adı en fazla sayısını ayarlar.

Ardından, altında görüntü alanını doldurun **kapsayıcıları** küme ölçeklendiriciyi kullanmak istediğiniz sürümü ile. AKS v1.2.2 gerektirir ya da daha yeni. Örnek v1.2.2 kullanır.

## <a name="deployment"></a>Dağıtım

Küme otomatik ölçeklendiricinin çalıştırarak dağıtma

```console
kubectl create -f aks-cluster-autoscaler.yaml
```

Küme otomatik ölçeklendiricinin çalışıp çalışmadığını denetlemek için aşağıdaki komutu kullanın ve pod'ların listesini kontrol edin. "Küme-çalışan otomatik ölçeklendiricinin ile" önekli bir pod olması gerekir. Bunu görürseniz, küme ölçeklendiriciyi dağıtıldı.

```console
kubectl -n kube-system get pods
```

Küme otomatik ölçeklendiricinin durumunu görüntülemek için çalıştırın

```console
kubectl -n kube-system describe configmap cluster-autoscaler-status
```

## <a name="interpreting-the-cluster-autoscaler-status"></a>Küme otomatik ölçeklendiricinin durumunu yorumlama

```console
$ kubectl -n kube-system describe configmap cluster-autoscaler-status
Name:         cluster-autoscaler-status
Namespace:    kube-system
Labels:       <none>
Annotations:  cluster-autoscaler.kubernetes.io/last-updated=2018-07-25 22:59:22.661669494 +0000 UTC

Data
====
status:
----
Cluster-autoscaler status at 2018-07-25 22:59:22.661669494 +0000 UTC:
Cluster-wide:
  Health:      Healthy (ready=1 unready=0 notStarted=0 longNotStarted=0 registered=1 longUnregistered=0)
               LastProbeTime:      2018-07-25 22:59:22.067828801 +0000 UTC
               LastTransitionTime: 2018-07-25 00:38:36.41372897 +0000 UTC
  ScaleUp:     NoActivity (ready=1 registered=1)
               LastProbeTime:      2018-07-25 22:59:22.067828801 +0000 UTC
               LastTransitionTime: 2018-07-25 00:38:36.41372897 +0000 UTC
  ScaleDown:   NoCandidates (candidates=0)
               LastProbeTime:      2018-07-25 22:59:22.067828801 +0000 UTC
               LastTransitionTime: 2018-07-25 00:38:36.41372897 +0000 UTC

NodeGroups:
  Name:        nodepool1
  Health:      Healthy (ready=1 unready=0 notStarted=0 longNotStarted=0 registered=1 longUnregistered=0 cloudProviderTarget=1 (minSize=1, maxSize=5))
               LastProbeTime:      2018-07-25 22:59:22.067828801 +0000 UTC
               LastTransitionTime: 2018-07-25 00:38:36.41372897 +0000 UTC
  ScaleUp:     NoActivity (ready=1 cloudProviderTarget=1)
               LastProbeTime:      2018-07-25 22:59:22.067828801 +0000 UTC
               LastTransitionTime: 2018-07-25 00:38:36.41372897 +0000 UTC
  ScaleDown:   NoCandidates (candidates=0)
               LastProbeTime:      2018-07-25 22:59:22.067828801 +0000 UTC
               LastTransitionTime: 2018-07-25 00:38:36.41372897 +0000 UTC


Events:  <none>
```

Küme otomatik ölçeklendiricinin durumunu iki farklı düzeyde küme otomatik ölçeklendiricinin durumunu görmenize olanak sağlar: küme çapında ve her düğüm grubu içinde. AKS şu anda yalnızca bir düğüm havuzu desteklediğinden, bu ölçümleri aynıdır.

* Sistem durumu düğümleri genel durumunu gösterir. Küme otomatik ölçeklendiricinin oluşturmak struggles veya kümedeki düğümleri kaldırır, bu durum "Uygun değil" olarak değişecektir. Farklı düğümlerin durumunun dökümünü de mevcuttur:
    * "Hazır" bir düğüm üzerinde zamanlanmış pod'ları için hazır olduğu anlamına gelir.
    * "Hazır olmayan" başladıktan sonra bozulduğu bir düğümü anlamına gelir.
    * "NotStarted", bir düğümü tam olarak henüz başlatıldı değil anlamına gelir.
    * "LongNotStarted", bir düğüm makul bir sınırı içinde başlatılamadı anlamına gelir.
    * "Bir düğüm grubunda kayıtlı kayıtlı anlamına gelir
    * "Kayıtsız" bir düğüm kümesi sağlayıcısı tarafında var, ancak Kubernetes'te kaydedilemedi anlamına gelir.
  
* ScaleUp küme ölçeği kümenizde gerçekleşmelidir belirlediğinde denetlemenizi sağlar.
    * Geçiş, kümedeki düğüm sayısını değiştiğinde veya bir düğüm değişiklikleri durumunu ' dir.
    * Kullanılabilir ve hazır kümedeki düğüm sayısını hazır düğümler sayısıdır. 
    * CloudProviderTarget küme ölçeklendirici, küme iş yükünü işlemek gereken belirledi düğümler sayısıdır.

* ScaleDown varsa aşağı ölçeklendirme için aday denetlemenizi sağlar. 
    * Bir ölçeği azaltma için bir düğüm kümesi ölçeklendiriciyi belirledi, kümenin kendi iş yükü işleme yeteneği etkilemeden kaldırılabilir adaydır. 
    * Sağlanan kez ölçeği azaltma adayları son kez küme denetlendi ve son geçiş süresini gösterir.

Son olarak, olaylar'ın altında herhangi bir ölçekte bakın veya olay, başarısız veya başarılı ve küme ölçeklendiriciyi gerçekleştirmiş süreleri, ölçeği.

## <a name="next-steps"></a>Sonraki adımlar

Yatay pod otomatik ölçeklendiricinin ile küme ölçeklendiriciyi kullanmak için kullanıma [Kubernetes uygulamasını ve altyapısını ölçeklendirme][aks-tutorial-scale].

AKS öğreticileri ile AKS dağıtma ve yönetme hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [AKS Öğreticisi][aks-tutorial-prepare-app]

<!-- LINKS - internal -->
[aks-quick-start]: ./kubernetes-walkthrough.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-scale]: ./tutorial-kubernetes-scale.md
[aks-upgrade]: ./upgrade-cluster.md

<!-- LINKS - external -->
[cluster-autoscale]: https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md
