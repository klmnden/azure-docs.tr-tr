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
ms.openlocfilehash: 2b78479c257930669729a7781b3893b3e2064bab
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
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

İkinci kaynak grubu kolay silinmesini AKS dağıtımı ile ilişkili tüm kaynaklar için otomatik olarak oluşturulur.

## <a name="is-azure-key-vault-integrated-with-aks"></a>Azure anahtar kasası AKS ile tümleştirilir? 

Hayır, bu değildir ancak bu tümleştirme planlanmış. Bu arada, aşağıdaki çözümden deneyebilirsiniz [Hexadite][hexadite]. 

<!-- LINKS - external -->
[auto-scaler]: https://github.com/kubernetes/autoscaler
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent  