---
title: "Azure kapsayıcı hizmeti için sık sorulan sorular"
description: "Bazı Azure kapsayıcı hizmeti ilgili sık sorulan soruların yanıtlarını içerir."
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 2/01/2018
ms.author: nepeters
ms.openlocfilehash: 73c49510512c9148f4fee98423b14770fa8602b9
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2018
---
# <a name="frequently-asked-questions-about-azure-container-service-aks"></a>Azure kapsayıcı hizmeti (AKS) hakkında sık sorulan sorular

Bu makale adresleri soruları Azure kapsayıcı hizmeti (AKS) hakkında sık.

## <a name="which-azure-regions-will-have-azure-container-service-aks"></a>Hangi Azure bölgeleri Azure kapsayıcı hizmeti (AKS) gerekiyor mu? 

- Orta Kanada 
- Doğu Kanada 
- Orta ABD 
- Doğu ABD 
- Güneydoğu Asya 
- Batı Avrupa 
- Batı ABD 2 

## <a name="when-will-additional-regions-be-added"></a>Ek bölgeler zaman eklenir? 

Ek bölgeler talep arttıkça eklenir.

## <a name="are-security-updates-applied-to-aks-nodes"></a>Güvenlik güncelleştirmeleri AKS düğümlerine uygulanır? 

Ancak, bir yeniden başlatma gerçekleştirilemiyor gecelik bir zamanlamaya göre kümenizdeki düğümlerin işletim sistemi güvenlik yamaları uygulanır. Gerekirse, düğümleri portalı veya Azure CLI aracılığıyla yeniden. Bir küme yükseltme yaparken, en son Ubuntu görüntüsü kullanılır ve tüm güvenlik düzeltme eklerinin (yeniden başlatma ile) uygulanır.

## <a name="do-you-recommend-customers-use-acs-or-akss"></a>Kullanım ACS veya AKSs müşteriler önerilir? 

Azure kapsayıcı hizmeti (AKS) sonraki bir tarihte GA olacak koşuluyla, PT'ın yapı, geliştirme ve test kümeleri içinde AKS ancak ACS-Kubernetes üretim kümeleri öneririz.  

## <a name="when-will-acs-be-deprecated"></a>Ne zaman ACS kullanım dışı kalacaktır? 

ACS AKS İST olduğu zaman kullanım dışı kalacaktır Kümeler için AKS geçirmek için bu tarihten sonraki 12 ay sahip olur. 12 aylık dönem boyunca tüm ACS işlemleri çalıştırabilirsiniz.

## <a name="does-aks-support-node-autoscaling"></a>AKS düğümü otomatik ölçeklendirmeyi destekliyor mu? 

Düğüm otomatik ölçeklendirmeyi desteklenmez, ancak yol haritası üzerinde değil. Bu açık kaynaklıdır bir göz atalım isteyebilirsiniz [otomatik ölçeklendirmeyi uygulama][auto-scaler].

## <a name="why-are-two-resource-groups-created-with-aks"></a>Neden iki kaynak grubu ile AKS oluşturulur? 

Her Azure kapsayıcı hizmeti (AKS) küme iki kaynak grubunda yer alıyor. İlk oluşturulur ve yalnızca AKS kaynak içerir. İkinci kaynak grubu olduğundan otomatik olarak dağıtım sırasında oluşturulan ve VM gibi tüm küme infrastructural kaynaklarını içeren ağ ve depolama kaynakları. Bu kaynak grubu için kolay kaynak temizleme oluşturulur. 

Otomatik oluşturulan kaynak grubu için benzer bir adı vardır:

```
MC_myResourceGRoup_myAKSCluster_eastus
```

Depolama hesapları veya ayrılmış genel IP adresi gibi Kubernetes kümesi ile kullanılacak Azure kaynaklarını eklerken, bu kaynakları otomatik olarak oluşturulan kaynak grubu içinde oluşturulması gerekir.   

## <a name="is-azure-key-vault-integrated-with-aks"></a>Azure anahtar kasası AKS ile tümleştirilir? 

Hayır, bu değildir ancak bu tümleştirme planlanmış. Bu arada, aşağıdaki çözümden deneyebilirsiniz [Hexadite][hexadite]. 

<!-- LINKS - external -->
[auto-scaler]: https://github.com/kubernetes/autoscaler
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent  