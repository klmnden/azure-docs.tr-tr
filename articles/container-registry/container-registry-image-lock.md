---
title: Görüntüyü Azure Container Registry Kilitle
description: Bu nedenle, silinemez veya bir Azure container Registry'de üzerine bir kapsayıcı görüntüsü ya da depo için öznitelikler ayarlayın.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 02/19/2019
ms.author: danlep
ms.openlocfilehash: ebbfaba158e7ddb669111f097eb1adde2373aa6c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60828656"
---
# <a name="lock-a-container-image-in-an-azure-container-registry"></a>Bir Azure container registry'den kapsayıcı görüntüsü Kilitle

Bir Azure container Registry'de silinmiş veya güncelleştirilmiş bir görüntü sürümü veya bir depo kilitleyebilirsiniz. Görüntü veya bir depo kilitlemek için Azure CLI komutunu kullanarak özniteliklerini güncelleştirme [az acr deposunun güncelleştirilmesi][az-acr-repository-update]. 

Bu makalede, Azure Cloud Shell'i Azure CLI'da çalıştırın veya yerel olarak gerektirir (2.0.55 sürümü veya üzeri önerilir). Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli].

## <a name="scenarios"></a>Senaryolar

Varsayılan olarak, bir Azure Container Registry'de etiketli görüntüsüdür *değişebilir*, uygun izinlerle tekrar tekrar güncelleştirebilir ve görüntünün aynı etikete sahip bir kayıt defterine iletin. Kapsayıcı görüntülerini de olabilir [silinmiş](container-registry-delete.md) gerektiğinde. Bu davranış, görüntüleri geliştirdiğinizde ve kayıt defteriniz için bir boyut sürdürmeniz gerekir yararlıdır.

Ancak, bir kapsayıcı görüntüsü üretime dağıtırken ihtiyacınız olabilecek bir *değişmez* kapsayıcı görüntüsü. Bir sabit görüntü, yanlışlıkla üzerine veya silemeyeceğiniz biridir. Kullanım [az acr deposunun güncelleştirilmesi] [ az-acr-repository-update] komutu, böylece depo özniteliklerini ayarlayın:

* Görüntü sürümü ya da bir deponun tamamı Kilitle

* Görüntü sürümü ya da depo silinmeye karşı korunmasına ancak güncelleştirmelere izin ver

* Görüntü sürümü ya da bir deponun tamamı üzerinde okuma (çekin) işlemleri engelle

Örnekler için aşağıdaki bölümlere bakın.

## <a name="lock-an-image-or-repository"></a>Bir resim veya depo Kilitle 

### <a name="show-the-current-repository-attributes"></a>Geçerli depo öznitelikleri göster
Geçerli bir depo özniteliklerini görmek için aşağıdaki komutu çalıştırın. [az acr depo show] [ az-acr-repository-show] komutu:

```azurecli
az acr repository show \
    --name myregistry --repository myrepo
    --output jsonc
```

### <a name="show-the-current-image-attributes"></a>Geçerli görüntü öznitelikleri göster
Bir etiketin geçerli öznitelikleri görmek için aşağıdaki komutu çalıştırın. [az acr depo show] [ az-acr-repository-show] komutu:

```azurecli
az acr repository show \
    --name myregistry --image image:tag \
    --output jsonc
```

### <a name="lock-an-image-by-tag"></a>Etikete göre bir görüntü Kilitle

Kilidi *myrepo / myimage:tag* görüntüsü *myregistry*, aşağıdakini çalıştırarak [az acr deposunun güncelleştirilmesi] [ az-acr-repository-update] komutu:

```azurecli
az acr repository update \
    --name myregistry --image myrepo/myimage:tag \
    --write-enabled false
```

### <a name="lock-an-image-by-manifest-digest"></a>Görüntü tarafından bildirim Özet Kilitle

Kilidi bir *myrepo/myımage* bildirim Özet tarafından tanımlanan görüntüsü (SHA-256 karma değeri olarak temsil edilen `sha256:...`), aşağıdaki komutu çalıştırın. (Bir veya daha fazla görüntü etiketler ile ilişkili bildirim Özet bulmak için çalıştırın [az acr depo show-bildirimleri] [ az-acr-repository-show-manifests] komutu.)

```azurecli
az acr repository update \
    --name myregistry --image myrepo/myimage@sha256:123456abcdefg \
    --write-enabled false
```

### <a name="lock-a-repository"></a>Bir depo Kilitle

Kilidi *myrepo/myımage* depoyu ve içindeki tüm görüntüleri aşağıdaki komutu çalıştırın:

```azurecli
az acr repository update \
    --name myregistry --repository myrepo/myimage \
    --write-enabled false
```

## <a name="protect-an-image-or-repository-from-deletion"></a>Bir resim veya depo silinmeye karşı koru

### <a name="protect-an-image-from-deletion"></a>Görüntü silinmeye karşı koru

İzin vermek için *myrepo / myimage:tag* görüntü güncelleştirildi ancak silinmez, aşağıdaki komutu çalıştırın:

```azurecli
az acr repository update \
    --name myregistry --image myrepo/myimage:tag \
    --delete-enabled false --write-enabled true
```

### <a name="protect-a-repository-from-deletion"></a>Bir depo silinmeye karşı koru

Aşağıdaki komut kümelerini *myrepo/myımage* silinemez bu nedenle depo. Ayrı görüntüleri hala güncelleştirildi veya silindi.

```azurecli
az acr repository update \
    --name myregistry --repository myrepo/myimage \
    --delete-enabled false --write-enabled true
```

## <a name="prevent-read-operations-on-an-image-or-repository"></a>Bir resim veya depo okuma işlemlerinin engelle

Üzerinde okuma (çekin) işlemleri önlemek için *myrepo / myimage:tag* görüntü, aşağıdaki komutu çalıştırın:

```azurecli
az acr repository update \
    --name myregistry --image myrepo/myimage:tag \
    --read-enabled false
```

' Ndaki tüm görüntüler okuma işlemlerinin önlemek için *myrepo/myımage* depo, aşağıdaki komutu çalıştırın:

```azurecli
az acr repository update \
    --name myregistry --repository myrepo/myimage \
    --read-enabled false
```

## <a name="unlock-an-image-or-repository"></a>Bir resim veya depo kilidini aç

Varsayılan davranışı geri yüklemek için *myrepo / myimage:tag* silinecek ve güncelleştirilmiş, böylece görüntü aşağıdaki komutu çalıştırın:

```azurecli
az acr repository update \
    --name myregistry --image myrepo/myimage:tag \
    --delete-enabled true --write-enabled true
```

Varsayılan davranışı geri yüklemek için *myrepo/myımage* depo ve tüm görüntüleri böylece bunlar silinecek ve güncelleştirilmiş, aşağıdaki komutu çalıştırın:

```azurecli
az acr repository update \
    --name myregistry --repository myrepo/myimage \
    --delete-enabled true --write-enabled true
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, kullanma hakkında bilgi edindiniz [az acr deposunun güncelleştirilmesi] [ az-acr-repository-update] silme veya bir depodaki görüntü sürümlerinin güncelleştirme önlemek için komutu. Ek öznitelikler ayarlamak için bkz [az acr deposunun güncelleştirilmesi] [ az-acr-repository-update] komut başvurusu.

Öznitelik bir görüntü sürümü ya da depo kümesini görmek için [az acr depo show] [ az-acr-repository-show] komutu.

Silme işlemleri hakkında daha fazla ayrıntı için bkz: [Sil kapsayıcı görüntülerini Azure Container Registry'de][container-registry-delete].

<!-- LINKS - Internal -->
[az-acr-repository-update]: /cli/azure/acr/repository#az-acr-repository-update
[az-acr-repository-show]: /cli/azure/acr/repository#az-acr-repository-show
[az-acr-repository-show-manifests]: /cli/azure/acr/repository#az-acr-repository-show-manifests
[azure-cli]: /cli/azure/install-azure-cli
[container-registry-delete]: container-registry-delete.md

