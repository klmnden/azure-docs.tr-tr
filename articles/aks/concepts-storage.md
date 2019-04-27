---
title: Kavramlar - Azure Kubernetes hizmeti (AKS) depolama
description: Azure Kubernetes hizmeti (birimler, kalıcı birimleri, depolama sınıfları ve talepleri de dahil olmak üzere AKS), depolama hakkında bilgi edinin
services: container-service
author: rockboyfor
ms.service: container-service
ms.topic: conceptual
origin.date: 03/01/2019
ms.date: 04/08/2019
ms.author: v-yeche
ms.openlocfilehash: cce38eb12d803c0640d9ee774dbc6c98ab5db219
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60466826"
---
# <a name="storage-options-for-applications-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) uygulamaları için Depolama Seçenekleri

Azure Kubernetes Service (AKS) uygulamaları, verileri depolama ve alma için gerekebilir. Bazı uygulama iş yükleri için bu veri depolama, pod'ların silindiğinde, artık gerekli olmadığında düğümde yerel olarak hızlı depolama kullanabilirsiniz. Diğer uygulama iş yüklerini Azure platformu içerisindeki birimleri hakkında daha fazla normal veri kalıcı depolama alanı gerektirebilir. Birden çok podunuz, aynı veri hacimlerini paylaşın veya başka bir düğümde pod zamanlanacağını, veri birimleri yeniden açmanız gerekebilir. Son olarak, hassas veriler ya da uygulama yapılandırma bilgilerini pod'ları ekleme gerekebilir.

![Azure Kubernetes Hizmetleri (AKS) kümesini uygulamalar için Depolama Seçenekleri](media/concepts-storage/aks-storage-options.png)

Bu makalede aks'deki uygulamalarınıza depolama sağlamak için gereken temel kavramlar tanıtılır:

- [Birimleri](#volumes)
- [Kalıcı birimleri](#persistent-volumes)
- [Depolama sınıfları](#storage-classes)
- [Kalıcı hacim talepleri](#persistent-volume-claims)

## <a name="volumes"></a>Birimler

Uygulamaları genellikle veri depolama ve alma gerekir. Kubernetes, tek tek pod'ları genellikle kısa ömürlü, atılabilir kaynaklar olarak değerlendirir. gibi farklı yaklaşımlar uygulamalarının kullanın ve gerektiğinde verileri kalıcı hale getirmek kullanılabilir. A *birim* veri pod'ları arasında ve uygulama yaşam döngüsü boyunca kalıcı olarak depolamak ve almak için bir yol gösterir.

Veri depolama ve alma için geleneksel birimleri, Azure Depolama tarafından yedeklenen Kubernetes kaynakları olarak oluşturulur. El ile pod'ların doğrudan atanmak üzere bu veri birimleri oluşturmak veya bunları otomatik olarak oluşturma Kubernetes sahip. Bu veri birimleri, Azure diskleri veya Azure dosyaları kullanabilirsiniz:

- *Azure diskleri* bir Kubernetes oluşturmak için kullanılan *DataDisk* kaynak. Azure Premium depolama, yüksek performanslı SSD tarafından desteklenen ya da normal HDD'ler ile desteklenen Azure standart depolama diskleri kullanabilirsiniz. Çoğu üretim ve geliştirme iş yükleri için Premium depolama kullanın. Azure disklerin bağlı olarak *ReadWriteOnce*, dolayısıyla yalnızca tek bir düğüm için kullanılabilir. Azure dosyaları aynı anda birden çok düğüm tarafından erişilebilir depolama birimleri için kullanın.
- *Azure dosyaları* pod'ları için bir Azure depolama hesabı tarafından desteklenen SMB 3.0 paylaşımı bağlamak için kullanılabilir. Birden çok düğüm ve pod'ları arasında veri paylaşımı dosyaları sağlar. Şu anda, dosyaları yalnızca normal HDD'ler ile desteklenen Azure standart depolamayı kullanabilirsiniz.

Kubernetes'te, birimler, bilgileri burada depolanır ve alınan daha fazlasını geleneksel disk temsil edebilir. Kubernetes birimleri, ayrıca kapsayıcıları tarafından kullanım için bir pod içinde veri ekleme için bir yol olarak kullanılabilir. Kubernetes'te ortak ek bir birim türleri şunlardır:

- *emptyDir* -bu birim için bir pod genellikle geçici alanı kullanılır. Bir pod içinde tüm kapsayıcılar birim üzerindeki verilere erişebilir. Bu birim türü için yazılan veri devam yalnızca pod - ömrü için birim pod silindiğinde silinir. Bu birimi genellikle yerel düğüm disk depolamanın kullanır, ancak yalnızca düğümün bellekte de bulunabilir.
- *Gizli dizi* -hassas verileri parolalar gibi pod'ların eklemesine bu birimi kullanılır. Önce Kubernetes API'yi kullanarak bir gizli dizi oluşturun. Pod veya dağıtım tanımladığınızda, belirli bir gizli dizi istenebilir. Gizli dizileri için bunu gerektiren zamanlanmış bir pod düğüm yalnızca sağlanır ve gizli dizi depolanır *tmpfs*değil yazılı diske. Gizli dizi gerektiren bir düğüm üzerindeki son pod silindiğinde, gizli dizi düğümün tmpfs silindi. Gizli dizileri belirli bir ad alanı içinde depolanır ve yalnızca pod'ların aynı ad alanında tarafından erişilebilir.
- *configMap* -anahtar-değer çifti özelliklerini uygulama yapılandırma bilgileri gibi pod'ların eklemesine bu birim türü kullanılır. Uygulama yapılandırma bilgilerini bir kapsayıcı görüntüsü içinde tanımlamak yerine kolayca güncelleştirilebilir ve yeni pod'ların örneklerine dağıtılan uygulanan bir Kubernetes kaynak olarak tanımlayabilirsiniz. Gizli dizi kullanarak gibi bir ConfigMap Kubernetes API kullanarak ilk oluşturun. Pod veya dağıtım tanımlarken bu ConfigMap ardından istenebilir. ConfigMaps belirtilen bir ad alanı içinde depolanır ve yalnızca pod'ların aynı ad alanında tarafından erişilebilir.

## <a name="persistent-volumes"></a>Kalıcı birimleri

Tanımlanan ve pod yaşam döngüsü bir parçası olarak oluşturulan birimlerin yalnızca pod silinene kadar mevcut. Pod'ları genellikle bir pod StatefulSets özellikle de bir bakım olayı sırasında farklı bir ana bilgisayarda zamanlanacağını kalır için kendi depolama bekler. A *kalıcı hacim* (PV) olan bir depolama kaynağı oluşturulur ve tek bir pod ömründen uzun bulunabilir Kubernetes API tarafından yönetilir.

Azure diskleri veya dosyaları PersistentVolume sağlamak için kullanılır. Birimlerde önceki bölümde belirtildiği gibi diskleri veya dosyaları genellikle veri ya da performans katmanı eş zamanlı erişim gereksinimini tarafından belirlenir.

![Azure Kubernetes Hizmetleri (AKS) kümesini kalıcı birimleri](media/concepts-storage/persistent-volumes.png)

Bir PersistentVolume olabilir *statik olarak* bir Küme Yöneticisi tarafından oluşturulan veya *dinamik olarak* Kubernetes API sunucusu tarafından oluşturulmuş. Bir pod zamanlandı ve şu anda kullanılamıyor depolama istekleri, Kubernetes temel alınan Azure diski veya dosyaları depolama oluşturabilir ve pod'u ekleyin. Dinamik sağlama kullanan bir *StorageClass* ne tür bir Azure depolama oluşturulmalıdır tanımlamak için.

## <a name="storage-classes"></a>Depolama sınıfları

Depolama, Premium ve standart olmak gibi farklı katmanlarda tanımlamak için oluşturabileceğiniz bir *StorageClass*. Ayrıca StorageClass tanımlar *reclaimPolicy*. Bu reclaimPolicy pod silinir ve kalıcı hacim artık gerekli olmayabilir, temel alınan Azure depolama kaynağı davranışını denetler. Temel alınan depolama kaynağı silinmiş veya gelecekteki bir pod ile kullanılmak üzere saklanır.

AKS, ilk iki StorageClasses oluşturulur:

- *Varsayılan* -bir yönetilen Disk oluşturmak için kullandığı Azure standart depolama. Geri kazan ilke kullanıldıkları pod silindiğinde, temel alınan Azure Disk silinip silinmediğini gösterir.
- *premium yönetilen* -yönetilen Disk oluşturmak için kullandığı Azure Premium depolama. Geri kazan ilke yeniden kullanıldıkları pod silindiğinde, temel alınan Azure Disk silinip silinmediğini gösterir.

Kalıcı bir birim için hiçbir StorageClass belirtilmezse, varsayılan StorageClass kullanılır. Gereksinim duyduğunuz uygun depolama kullanmasını sağlayarak kalıcı birimler isterken ilgileniriz. Bir StorageClass kullanarak ek gereksinimleriniz için oluşturabileceğiniz `kubectl`. Aşağıdaki örnek, Premium yönetilen diskler kullanır ve temel alınan Azure Disk olması gerektiğini belirtir *korunur* pod silindiğinde:

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: managed-premium-retain
provisioner: kubernetes.io/azure-disk
reclaimPolicy: Retain
parameters:
  storageaccounttype: Premium_LRS
  kind: Managed
```

## <a name="persistent-volume-claims"></a>Kalıcı hacim talepleri

Bir PersistentVolumeClaim belirli StorageClass, erişim modu ve boyutunu, Disk veya dosya depolama ister. Üzerinde tanımlanan StorageClass tabanlı talebi karşılamak üzere var olan bir kaynak yok ise, Kubernetes API sunucusu dinamik olarak temel alınan Azure depolama kaynak sağlayabilirsiniz. Pod'u birimi bağlandıktan birim bağlama pod tanımını içerir.

![Azure Kubernetes Hizmetleri (AKS) kümesini Taleplerde kalıcı hacim](media/concepts-storage/persistent-volume-claims.png)

Bir PersistentVolume olan *bağlı* kullanılabilir depolama alanı kaynak istediği pod'u atandıktan sonra bir PersistentVolumeClaim için. Talepler için kalıcı birimlerin 1:1 eşleme vardır.

Aşağıdaki örnek YAML bildirimi kullanan bir kalıcı hacim talep gösterir *premium yönetilen* StorageClass ve bir Disk istekleri *5 GI* boyutu:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azure-managed-disk
spec:
  accessModes:
  - ReadWriteOnce
  storageClassName: managed-premium
  resources:
    requests:
      storage: 5Gi
```

Kalıcı hacim talep, bir pod tanımı oluşturduğunuzda, istenen depolama istemek için belirtilir. Sonra belirttiğiniz *volumeMount* uygulamalarınız veri okuma ve yazma için. Aşağıdaki örnek YAML bildirimi bir birim bağlamak için önceki kalıcı hacim talep'ın nasıl kullanılabileceğini gösterir */mnt/azure*:

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: nginx
spec:
  containers:
    - name: myfrontend
      image: nginx
      volumeMounts:
      - mountPath: "/mnt/azure"
        name: volume
  volumes:
    - name: volume
      persistentVolumeClaim:
        claimName: azure-managed-disk
```

## <a name="next-steps"></a>Sonraki adımlar

İlişkili en iyi yöntemler için bkz: [en iyi uygulamalar için depolama ve yedekleme aks'deki][operator-best-practices-storage].

Azure diskleri veya Azure dosyaları'nı kullanan dinamik ve statik birimleri oluşturma hakkında bilgi için aşağıdaki yapılır makalelerine bakın:

- [Azure diskleri kullanarak statik bir birim oluşturun][aks-static-disks]
- [Azure dosyaları'nı kullanarak statik bir birim oluşturun][aks-static-files]
- [Azure diskleri kullanarak dinamik bir birim oluşturun][aks-dynamic-disks]
- [Azure dosyaları'nı kullanarak dinamik bir birim oluşturun][aks-dynamic-files]

Çekirdek Kubernetes hakkında daha fazla bilgi ve AKS kavramlar için aşağıdaki makalelere bakın:

- [Kubernetes / AKS kümesi ve iş yükleri][aks-concepts-clusters-workloads]
- [Kubernetes / AKS kimlik][aks-concepts-identity]
- [Kubernetes / AKS güvenlik][aks-concepts-security]
- [Kubernetes / AKS sanal ağlar][aks-concepts-network]
- [Kubernetes / AKS ölçeklendirin][aks-concepts-scale]

<!-- EXTERNAL LINKS -->

<!-- INTERNAL LINKS -->
[aks-static-disks]: azure-disk-volume.md
[aks-static-files]: azure-files-volume.md
[aks-dynamic-disks]: azure-disks-dynamic-pv.md
[aks-dynamic-files]: azure-files-dynamic-pv.md
[aks-concepts-clusters-workloads]: concepts-clusters-workloads.md
[aks-concepts-identity]: concepts-identity.md
[aks-concepts-scale]: concepts-scale.md
[aks-concepts-security]: concepts-security.md
[aks-concepts-network]: concepts-network.md
[operator-best-practices-storage]: operator-best-practices-storage.md