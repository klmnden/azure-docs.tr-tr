---
title: "Azure Container Registry için en iyi yöntemler"
description: "Bu en iyi yöntemleri izleyerek Azure kapsayıcı kayıt defterinizi nasıl verimli bir şekilde kullanabileceğinizi öğrenin."
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: quickstart
ms.date: 12/20/2017
ms.author: marsma
ms.openlocfilehash: 684b778f57da4adb331958c5daef6b9906b6d253
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="best-practices-for-azure-container-registry"></a>Azure Container Registry için en iyi yöntemler

Bu en iyi yöntemleri izlemek, Azure’daki özel Docker kayıt defterinizin performansını ve maliyetini en iyi duruma getirmenize yardımcı olabilir.

## <a name="network-close-deployment"></a>Yakın ağ dağıtımı

Kapsayıcı kayıt defterinizi, kapsayıcıları dağıttığınız Azure bölgesinde oluşturun. Kayıt defterinizin ağ açısından kapsayıcı konaklarınıza yakın bir bölgeye yerleştirilmesi hem gecikmeyi hem de maliyeti düşürmeye yardımcı olabilir.

Yakın ağ dağıtımı, özel kapsayıcı kayıt defteri kullanmanın birincil nedenlerinden biridir. Docker görüntülerinin kademeli dağıtıma imkan tanıyan etkili bir [katman yapısı](https://docs.docker.com/engine/userguide/storagedriver/imagesandcontainers/) vardır. Bununla birlikte, yeni düğümlerin belirli bir görüntü için gereken tüm katmanları çekmesi gerekir. Bu ilk `docker pull` hızla büyüyerek birkaç gigabayta ulaşabilir. Dağıtımınıza yakın özel bir kayıt defterinin olması bu ağ gecikmesini en aza indirir.
Buna ek olarak, Azure dahil tüm genel bulutlar ağ çıkışı ücretleri uygular. Bir veri merkezinden diğerine görüntü çekmek, gecikmeye ek olarak ağ çıkışı ücretlerine yol açar.

## <a name="geo-replicate-multi-region-deployments"></a>Çok bölgeli dağıtımları coğrafi olarak çoğaltma

Birden çok bölgeye kapsayıcı dağıtıyorsanız Azure Container Registry'nin [coğrafi çoğaltma](container-registry-geo-replication.md) özelliğini kullanın. İster yerel veri merkezlerinden küresel müşterilere hizmet sunuyor olun ister geliştirme takımınız farklı konumlarda çalışıyor olsun, kayıt defterinizi coğrafi olarak çoğaltarak kayıt defteri yönetimini basitleştirebilir ve gecikmeyi en aza indirebilirsiniz. Şu anda önizlemede olan bu özellik [Premium](container-registry-skus.md) kayıt defterleriyle sunulmaktadır.

Coğrafi çoğaltma özelliğini kullanmayı öğrenmek için [Azure Container Registry’de coğrafi çoğaltma](container-registry-tutorial-prepare-registry.md) başlıklı üç bölümlü öğreticiye bakın.

## <a name="repository-namespaces"></a>Depo ad alanları

Depo ad alanlarından yararlanarak kuruluşunuz içinde tek bir kayıt defterinin birden çok grupta paylaşılmasına imkan tanıyabilirsiniz. Kayıt defterleri farklı dağıtımlarla ve takımlarla paylaşılabilir. İç içe ad alanlarını destekleyen Azure Container Registry, grup yalıtımı imkanı sunar.

Örneğin, aşağıdaki kapsayıcı görüntüsü etiketlerini göz önünde bulundurun. Kurumsal çapta kullanılan `aspnetcore` gibi görüntüler kök ad alanına yerleştirilirken, Üretim ve Pazarlama gruplarına ait her bir kapsayıcı görüntüsü kendi ad alanını kullanır.

```
contoso.azurecr.io/aspnetcore:2.0
contoso.azurecr.io/products/widget/web:1
contoso.azurecr.io/products/bettermousetrap/refundapi:12.3
contoso.azurecr.io/marketing/2017-fall/concertpromotions/campaign:218.42
```

## <a name="dedicated-resource-group"></a>Ayrılmış kaynak grubu

Kapsayıcı kayıt defterleri birden çok kapsayıcı konağında kullanılan kaynaklar olduğundan, bir kayıt defteri kendi kaynak grubunda bulunmalıdır.

Azure Container Instances gibi belirli bir konak türüyle denemeler yapabilecek olsanız da muhtemelen işiniz bittiğinde kapsayıcı örneğini silmek isteyeceksinizdir. Bununla birlikte, Azure Container Registry’ye gönderdiğiniz görüntü koleksiyonunu korumak da isteyebilirsiniz. Kayıt defterinizi kendi kaynak grubuna yerleştirerek, kapsayıcı örneğinin kaynak grubunu silerken yanlışlıkla kayıt defterindeki görüntü koleksiyonunu da silme riskini en aza indirirsiniz.

## <a name="authentication"></a>Kimlik Doğrulaması

Bir Azure kapsayıcı kayıt defteri ile kimlik doğrulama için iki ana senaryo vardır: bireysel kimlik doğrulama ve hizmet (veya "gözetimsiz") kimlik doğrulaması. Aşağıdaki tabloda, bu senaryolara kısa bir genel bakış ve her biri için önerilen kimlik doğrulama yöntemi sağlanmıştır.

| Tür | Örnek senaryo | Önerilen metot |
|---|---|---|
| Bireysel kimlik | Geliştirme makinesinden görüntü çeken veya gönderen bir geliştirici. | [az acr login](/cli/azure/acr?view=azure-cli-latest#az_acr_login) |
| Gözetimsiz kimlik/hizmet kimliği | Kullanıcıyla doğrudan ilgili olmayan derleme ve dağıtım işlem hatları. | [Hizmet sorumlusu](container-registry-authentication.md#service-principal) |

Azure Container Registry kimlik doğrulaması hakkında ayrıntılı bilgi edinmek için bkz. [Azure kapsayıcı kayıt defteri ile kimlik doğrulama](container-registry-authentication.md).

## <a name="manage-registry-size"></a>Kayıt defteri boyutunu yönetme

Her bir [kapsayıcı kayıt defteri SKU][container-registry-skus] öğesinde tipik senaryolara uygun depolama alanı kısıtlamaları mevcuttur: Başlangıç için **Temel**, üretim uygulamalarının çoğu için **Standart**, çok geniş ölçekli performans ve [coğrafi çoğaltma][container-registry-geo-replication] için **Premium**. Kayıt defterinizin kullanım ömrü boyunca kullanılmayan içerikleri düzenli olarak silerek boyutunu yönetmeniz gerekir.

Bir kayıt defterinin geçerli kullanım durumunu kapsayıcı kayıt defterinin Azure portalındaki **Genel Bakış** sayfasında bulabilirsiniz:

![Azure portalındaki kayıt defteri kullanım bilgileri][registry-overview-quotas]

Kayıt defterinizin boyutunu [Azure CLI][azure-cli] veya [Azure portalı][azure-portal] aracılığıyla yönetebilirsiniz. Yalnızca yönetilen SKU'lar (Temel, Standart, Premium) depo ve görüntü silme desteği sunar. Klasik kayıt defterindeki depoları, görüntüleri veya etiketleri silemezsiniz.

### <a name="delete-in-azure-cli"></a>Azure CLI ile silme

Bir depoyu veya bir depo içindeki içeriği silmek için [az acr repository delete][az-acr-repository-delete] komutunu kullanın.

Bir depoyu içindeki tüm etiketler ve görüntü katmanı verileriyle birlikte silmek için [az acr repository delete][az-acr-repository-delete] komutuyla birlikte yalnızca depo adını belirtin. Aşağıdaki örnekte *myapplication* deposunu ve içindeki tüm etiketleri ve görüntü katmanı verilerini siliyoruz:

```azurecli
az acr repository delete --name myregistry --repository myapplication
```

`--tag` ve `--manifest` bağımsız değişkenlerini kullanarak bir depodaki görüntü verilerini de silebilirsiniz. Bu bağımsız değişkenler hakkında ayrıntılı bilgi için [az acr repository delete komutu başvurusuna][az-acr-repository-delete] bakın.

### <a name="delete-in-azure-portal"></a>Azure portalında silme

Kayıt defterindeki bir depoyu Azure portalından silmek için ilk olarak kapsayıcı kayıt defterinize gidin. Ardından **HİZMETLER**'in altında **Depolar**'ı seçip silmek istediğiniz depoya sağ tıklayın. Depoyu ve içindeki Docker görüntülerini silmek için **Sil**'i seçin.

![Azure portalındaki bir depoyu silme][delete-repository-portal]

Benzer şekilde bir depodaki etiketleri de silebilirsiniz. Depoya gidin, **ETİKETLER** bölümünde silmek istediğiniz etikete sağ tıklayın ve **Sil**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Container Registry, SKU adı verilen ve her biri farklı özellikler sağlayan birkaç katmanda sunulmaktadır. Sunulan SKU’lar hakkında ayrıntılı bilgi için bkz. [Azure Container Registry SKU’ları](container-registry-skus.md).

<!-- IMAGES -->
[delete-repository-portal]: ./media/container-registry-best-practices/delete-repository-portal.png
[registry-overview-quotas]: ./media/container-registry-best-practices/registry-overview-quotas.png

<!-- LINKS - Internal -->
[az-acr-repository-delete]: /cli/azure/acr/repository#az_acr_repository_delete
[azure-cli]: /cli/azure
[azure-portal]: https://portal.azure.com
[container-registry-geo-replication]: container-registry-geo-replication.md
[container-registry-skus]: container-registry-skus.md
