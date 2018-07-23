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
ms.openlocfilehash: 4f8df8e7004ca3cee832b6230dc153b21e2a6c18
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39186722"
---
# <a name="cluster-autoscaler-on-azure-kubernetes-service-aks---preview"></a>Otomatik Ölçeklendiricinin küme Azure Kubernetes hizmeti üzerinde (AKS) - Önizleme

Azure Kubernetes Service (AKS), azure'da yönetilen bir Kubernetes kümesi dağıtmak için esnek bir çözümdür. Kaynak olarak taleplerini artırmak, küme ölçeklendiriciyi kümenizi ayarladığınız kısıtlamalarına göre bu talebi karşılamak üzere büyümesine izin verir. Küme otomatik ölçeklendiricinin (CA) aracısı düğümlerinizi pod'ların göre ölçeklendirme tarafından yapar. Bu, düzenli aralıklarla pod'ların ya da boş düğümleri denetlemek için küme tarar ve mümkünse boyutu artar. Varsayılan olarak, CA tarar pod'ların 10 saniyede ve ise bir düğümü kaldırır 10 dakikadan fazla kapatın. Yatay pod otomatik ölçeklendiricinin (HPA) ile kullanıldığında, HPA pod çoğaltmalarının ve kaynakları isteğe göre güncelleştirir. Yok, yeterli düğümleri ya da gereksiz düğümleri bu pod ölçeklendirmeyi aşağıdaki CA yanıt ve yeni düğüm kümesindeki pod'ları zamanlayın.

> [!IMPORTANT]
> Azure Kubernetes Service (AKS) kümesi ölçeklendiriciyi tümleştirmesi, şu anda **Önizleme**. Önizlemeler, [ek kullanım koşullarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.
>

## <a name="prerequisites"></a>Önkoşullar

Bu belge, RBAC özellikli bir AKS kümesi olduğunu varsayar. Bir AKS kümesi gerekirse bkz [Azure Kubernetes Service (AKS) hızlı başlangıç][aks-quick-start].

 Küme ölçeklendiriciyi kullanmak için kümenizi Kubernetes v1.10.X kullanarak veya üzeri ve RBAC etkinleştirilmiş olması gerekir. Kümenizi yükseltmek için makaleye bakın [AKS kümesini yükseltme][aks-upgrade].

## <a name="gather-information"></a>Bilgi toplayın

Aşağıdaki liste, sağlamanız gereken bilgilerin tümünü otomatik ölçeklendiricinin tanımında gösterir.

- *Abonelik kimliği*: Bu küme için kullanılan abonelik karşılık gelen kimliği
- *Kaynak grubu adı* : kümeye ait kaynak grubunun adı 
- *Küme adı*: kümesinin adı
- *İstemci kimliği*: adım oluşturma izni tarafından uygulama kimliği
- *İstemci gizli anahtarı*: adım oluşturma izni tarafından uygulama gizli anahtarı
- *Kiracı kimliği*: (hesap sahibi) Kiracı kimliği
- *Kaynak grubu düğümü*: kümesindeki aracı düğümleri içeren kaynak grubunun adı
- *Düğüm havuzu adı*: düğümün adı havuz ölçeğini istiyor musunuz
- *En düşük düğüm sayısı*: kümesinde düğümlerin sayısı alt sınırı
- *En fazla düğüm numarasını*: kümesinde düğümlerin sayısı üst sınırı
- *VM türü*: hizmet kullanılan Kubernetes kümesi oluşturmak için

Abonelik Kimliğinizi alın: 

``` azurecli
az account show --query id
```

Aşağıdaki komutu çalıştırarak Azure kimlik bilgileri kümesi oluşturur:

```console
$ az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/<subscription-id>" --output json

"appId": <app-id>,
"displayName": <display-name>,
"name": <name>,
"password": <app-password>,
"tenant": <tenant-id>
```

Uygulama Kimliğinizi, parolanızı ve Kiracı kimliği, ClientID, clientSecret ve aşağıdaki adımlarda Tenantıd olacaktır.

Aşağıdaki komutu çalıştırarak, düğüm havuzu adını alın. 

```console
$ kubectl get nodes --show-labels
```

Çıktı:

```console
NAME                       STATUS    ROLES     AGE       VERSION   LABELS
aks-nodepool1-37756013-0   Ready     agent     1h        v1.10.3   agentpool=nodepool1,beta.kubernetes.io/arch=amd64,beta.kubernetes.io/instance-type=Standard_DS1_v2,beta.kubernetes.io/os=linux,failure-domain.beta.kubernetes.io/region=eastus,failure-domain.beta.kubernetes.io/zone=0,kubernetes.azure.com/cluster=MC_[resource-group]\_[cluster-name]_[location],kubernetes.io/hostname=aks-nodepool1-37756013-0,kubernetes.io/role=agent,storageprofile=managed,storagetier=Premium_LRS
 ```

Ardından, etiketin değeri ayıklayın **agentpool**. "Nodepool1" bir kümenin düğüm havuzu için varsayılan addır.

Düğüm kaynak grubunuzun adını almak için etiketin değeri ayıklamak **kubernetes.azure.com<span></span>/küme**. Düğüm kaynak grubu adı genellikle MC_ [kaynak grubu] biçimindedir\_[küme adı] _ [konumu].

VmType parametresi olan burada, AKS kullanılmakta hizmetini ifade eder.

Artık, aşağıdaki bilgileri sahip olmalıdır:

- Subscriptionıd
- ResourceGroup
- Küme adı
- ClientID
- ClientSecret
- Kiracı kimliği
- NodeResourceGroup
- VMType

Ardından, tüm bu değerleri base64 ile kodlayın. Örneğin, base64 VMType değeriyle kodlamak için şunu yazın:

```console
$ echo AKS | base64
QUtTCg==
```

## <a name="create-secret"></a>Gizli dizi oluşturma
Bu verileri kullanarak, önceki adımlarda aşağıdaki biçimde bulunan değerleri kullanarak dağıtımın bir gizli dizi oluşturun:

```yaml
---
apiVersion: v1
kind: Secret
metadata:
  name: cluster-autoscaler-azure
  namespace: kube-system
data:
  ClientID: <base64-encoded-client-id>
  ClientSecret: <base64-encoded-client-secret>
  ResourceGroup: <base64-encoded-resource-group>
  SubscriptionID: <base64-encode-subscription-id>
  TenantID: <base64-encoded-tenant-id>
  VMType: QUtTCg==
  ClusterName: <base64-encoded-clustername>
  NodeResourceGroup: <base64-encoded-node-resource-group>
---
```

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
      - image: k8s.gcr.io/cluster-autoscaler:{{ ca_version }}
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

Kopyalayın ve önceki adımda oluşturduğunuz parolayı yapıştırın ve dosyayı başlangıcında ekleyin.

Ardından, düğümleri aralığını ayarlamak için bağımsız değişken için doldurun `--nodes` altında `command` MIN:MAX:NODE_POOL_NAME biçiminde. Örneğin: `--nodes=3:10:nodepool1` düğüm sayısı alt sınırı 3, 10 düğümleri ve nodepool1 düğüm havuzu adı en fazla sayısını ayarlar.

Ardından, altında görüntü alanını doldurun **kapsayıcıları** küme ölçeklendiriciyi kullanmak istediğiniz sürümü ile. AKS v1.2.2 gerektirir ya da daha yeni. Örnek v1.2.2 kullanır.

## <a name="deployment"></a>Dağıtım

Küme otomatik ölçeklendiricinin çalıştırarak dağıtma

```console
kubectl create -f cluster-autoscaler-containerservice.yaml
```

Küme otomatik ölçeklendiricinin çalışıp çalışmadığını denetlemek için aşağıdaki komutu kullanın ve pod'ların listesini kontrol edin. "Küme-çalışan otomatik ölçeklendiricinin ile" önekli bir pod ise küme ölçeklendirici, dağıtıldı.

```console
kubectl -n kube-system get pods
```

Küme otomatik ölçeklendiricinin durumunu görüntülemek için çalıştırın

```console
kubectl -n kube-system describe configmap cluster-autoscaler-status
```

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
