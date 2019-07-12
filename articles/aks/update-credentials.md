---
title: Azure Kubernetes Service (AKS) kümesini kimlik bilgilerini Sıfırla
description: Nasıl güncelleştirme veya sıfırlama hizmet sorumlusunu Azure Kubernetes Service (AKS) kümesi için kimlik bilgileri edinin
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 05/31/2019
ms.author: mlearned
ms.openlocfilehash: 5aac941133296d2040d5dd670155b80f5807e1e9
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67614120"
---
# <a name="update-or-rotate-the-credentials-for-a-service-principal-in-azure-kubernetes-service-aks"></a>Güncelleştirme veya hizmet sorumlusu Azure Kubernetes Service (AKS) için kimlik bilgilerini döndürme

Varsayılan olarak, bir yıllık sona erme zamanına sahip bir hizmet sorumlusu AKS kümeleri oluşturulur. Sona erme tarihine yakın,, ek bir süre için hizmet sorumlusu genişletmek için kimlik bilgilerini sıfırlayabilirsiniz. Güncelleştirme veya döndürme, tanımlanmış güvenlik ilkesinin parçası olarak kimlik bilgilerini isteyebilirsiniz. Bu makalede, bir AKS kümesi için bu kimlik bilgilerini güncelleştirme işlemi açıklanmaktadır.

## <a name="before-you-begin"></a>Başlamadan önce

Azure CLI Sürüm 2.0.65 gerekir veya daha sonra yüklü ve yapılandırılmış. Çalıştırma `az --version` sürümü bulmak için. Gerekirse yüklemek veya yükseltmek bkz [Azure CLI yükleme][install-azure-cli].

## <a name="choose-to-update-or-create-a-service-principal"></a>Güncelleştirme veya hizmet sorumlusu oluşturmak seçin

Bir AKS kümesi için kimlik bilgilerini güncelleştirmek istediğiniz zaman için seçebilirsiniz:

* Küme tarafından kullanılan mevcut hizmet sorumlusunun kimlik bilgilerini güncelleştirin veya
* Hizmet sorumlusu oluşturma ve bu yeni kimlik bilgilerini kullanmak için küme güncelleştirin.

Hizmet sorumlusu oluşturma ve sonra AKS kümesi güncelleştirmesi, bu bölümdeki adımları geri kalanını atlayın ve devam istiyorsanız [hizmet sorumlusu oluşturma](#create-a-service-principal). AKS kümesi tarafından kullanılan mevcut hizmet sorumlusunun kimlik bilgilerini güncelleştirmek istiyorsanız, bu bölümdeki adımlar ile devam edin.

### <a name="get-the-service-principal-id"></a>Hizmet sorumlusu Kimliğini alın

Varolan bir hizmet sorumlusunun kimlik bilgilerini güncelleştirmek için küme kullanarak hizmet sorumlusu Kimliğini alın [az aks show][az-aks-show] komutu. Aşağıdaki örnekte adlı Küme için kimliği alır *myAKSCluster* içinde *myResourceGroup* kaynak grubu. Hizmet sorumlusu kimliği adlı bir değişken olarak ayarlanmış *SP_ID* kullanılmak üzere ek komutu.

```azurecli-interactive
SP_ID=$(az aks show --resource-group myResourceGroup --name myAKSCluster \
    --query servicePrincipalProfile.clientId -o tsv)
```

### <a name="update-the-service-principal-credentials"></a>Hizmet sorumlusu kimlik bilgilerini güncelleştirme

Hizmet sorumlusu Kimliğini içeren bir değişken ayarlandığında, artık kullanarak kimlik bilgilerini sıfırlama [az ad sp kimlik bilgilerini Sıfırla][az-ad-sp-credential-reset]. Aşağıdaki örnek yeni bir güvenli gizli dizi için hizmet sorumlusunu Azure platformu sağlar. Bu yeni güvenli gizli dizi, ayrıca bir değişken olarak depolanır.

```azurecli-interactive
SP_SECRET=$(az ad sp credential reset --name $SP_ID --query password -o tsv)
```

Şimdi oturum devam [yeni kimlik bilgileri ile güncelleştirme AKS kümesi](#update-aks-cluster-with-new-credentials).

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Mevcut hizmet sorumlusu kimlik bilgileri önceki bölümde güncelleştirmek isterseniz, bu adımı atlayın. Devam [yeni kimlik bilgileri ile güncelleştirme AKS kümesi](#update-aks-cluster-with-new-credentials).

Bir hizmet sorumlusu oluşturur ve ardından bu yeni kimlik bilgilerini kullanmak için AKS kümesi için kullanın [az ad sp create-for-rbac][az-ad-sp-create] komutu. Aşağıdaki örnekte, `--skip-assignment` parametresi, ek varsayılan atamaların atanmasını engellemektedir:

```azurecli-interactive
az ad sp create-for-rbac --skip-assignment
```

Çıktı aşağıdaki örneğe benzerdir. Kendi `appId` ve `password`’lerinizi not edin. Bu değerler, sonraki adımda kullanılır.

```json
{
  "appId": "7d837646-b1f3-443d-874c-fd83c7c739c5",
  "name": "7d837646-b1f3-443d-874c-fd83c7c739c",
  "password": "a5ce83c9-9186-426d-9183-614597c7f2f7",
  "tenant": "a4342dc8-cd0e-4742-a467-3129c469d0e5"
}
```

Artık çıkışı kendi kullanarak hizmet sorumlusu Kimliğini ve istemci gizli dizi değişkenleri tanımlayın [az ad sp create-for-rbac][az-ad-sp-create] , aşağıdaki örnekte gösterildiği gibi komutu. *SP_ID* olduğundan, *AppID*ve *SP_SECRET* olduğundan, *parola*:

```azurecli-interactive
SP_ID=7d837646-b1f3-443d-874c-fd83c7c739c5
SP_SECRET=a5ce83c9-9186-426d-9183-614597c7f2f7
```

## <a name="update-aks-cluster-with-new-credentials"></a>AKS kümesi yeni kimlik bilgileriyle güncelleştirin

Olup, mevcut bir hizmet sorumlusunun kimlik bilgilerini güncelleştirin veya hizmet sorumlusu oluşturmak seçtiğiniz bağımsız olarak, artık AKS kümesi kullanarak, yeni kimlik bilgileriyle güncelleştirmeniz [az aks güncelleştirme-credentials][az-aks-update-credentials] komutu. Değişkenleri *--hizmet sorumlusu* ve *--CLIENT-secret* kullanılır:

```azurecli-interactive
az aks update-credentials \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --reset-service-principal \
    --service-principal $SP_ID \
    --client-secret $SP_SECRET
```

AKS güncelleştirilmesi hizmet sorumlusu kimlik bilgileri için birkaç dakika sürer.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, hizmet sorumlusu AKS kümesi güncelleştirildi. Bir küme içindeki iş yükleri için kimlik yönetme hakkında daha fazla bilgi için bkz. [en iyi uygulamalar için kimlik doğrulama ve yetkilendirme aks'deki][best-practices-identity].

<!-- LINKS - internal -->
[install-azure-cli]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-aks-update-credentials]: /cli/azure/aks#az-aks-update-credentials
[best-practices-identity]: operator-best-practices-identity.md
[az-ad-sp-create]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-ad-sp-credential-reset]: /cli/azure/ad/sp/credential#az-ad-sp-credential-reset
