---
title: Azure Container Registry için en iyi yöntemler
description: Bu en iyi yöntemleri izleyerek Azure kapsayıcı kayıt defterinizi nasıl verimli bir şekilde kullanabileceğinizi öğrenin.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 09/27/2018
ms.author: danlep
ms.openlocfilehash: 2cf64c7c4f99a57c4a4a6cf03e68e8af803ceca9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60787361"
---
# <a name="best-practices-for-azure-container-registry"></a>Azure Container Registry için en iyi yöntemler

Bu en iyi yöntemleri izlemek, Azure’daki özel Docker kayıt defterinizin performansını ve maliyetini en iyi duruma getirmenize yardımcı olabilir.

## <a name="network-close-deployment"></a>Yakın ağ dağıtımı

Kapsayıcı kayıt defterinizi, kapsayıcıları dağıttığınız Azure bölgesinde oluşturun. Kayıt defterinizin ağ açısından kapsayıcı konaklarınıza yakın bir bölgeye yerleştirilmesi hem gecikmeyi hem de maliyeti düşürmeye yardımcı olabilir.

Yakın ağ dağıtımı, özel kapsayıcı kayıt defteri kullanmanın birincil nedenlerinden biridir. Docker görüntülerinin kademeli dağıtıma imkan tanıyan etkili bir [katman yapısı](https://docs.docker.com/engine/userguide/storagedriver/imagesandcontainers/) vardır. Bununla birlikte, yeni düğümlerin belirli bir görüntü için gereken tüm katmanları çekmesi gerekir. Bu ilk `docker pull` hızla büyüyerek birkaç gigabayta ulaşabilir. Dağıtımınıza yakın özel bir kayıt defterinin olması bu ağ gecikmesini en aza indirir.
Buna ek olarak, Azure dahil tüm genel bulutlar ağ çıkışı ücretleri uygular. Bir veri merkezinden diğerine görüntü çekmek, gecikmeye ek olarak ağ çıkışı ücretlerine yol açar.

## <a name="geo-replicate-multi-region-deployments"></a>Çok bölgeli dağıtımları coğrafi olarak çoğaltma

Birden çok bölgeye kapsayıcı dağıtıyorsanız Azure Container Registry'nin [coğrafi çoğaltma](container-registry-geo-replication.md) özelliğini kullanın. İster yerel veri merkezlerinden küresel müşterilere hizmet sunuyor olun ister geliştirme takımınız farklı konumlarda çalışıyor olsun, kayıt defterinizi coğrafi olarak çoğaltarak kayıt defteri yönetimini basitleştirebilir ve gecikmeyi en aza indirebilirsiniz. Coğrafi çoğaltma yalnızca [Premium](container-registry-skus.md) kayıt defterleriyle kullanılabilir.

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

Bir kayıt defteri, kapsayıcı kayıt defterleri birden çok kapsayıcı konağında kullanılan kaynaklar olduğundan, kendi kaynak grubunda yer almalıdır.

Azure Container Instances gibi belirli bir konak türüyle denemeler yapabilecek olsanız da muhtemelen işiniz bittiğinde kapsayıcı örneğini silmek isteyeceksinizdir. Bununla birlikte, Azure Container Registry’ye gönderdiğiniz görüntü koleksiyonunu korumak da isteyebilirsiniz. Kayıt defterinizi kendi kaynak grubuna yerleştirerek, kapsayıcı örneğinin kaynak grubunu silerken yanlışlıkla kayıt defterindeki görüntü koleksiyonunu da silme riskini en aza indirirsiniz.

## <a name="authentication"></a>Kimlik Doğrulaması

Bir Azure kapsayıcı kayıt defteri ile kimlik doğrulama için iki ana senaryo vardır: bireysel kimlik doğrulama ve hizmet (veya "gözetimsiz") kimlik doğrulaması. Aşağıdaki tabloda, bu senaryolara kısa bir genel bakış ve her biri için önerilen kimlik doğrulama yöntemi sağlanmıştır.

| Tür | Örnek senaryo | Önerilen metot |
|---|---|---|
| Bireysel kimlik | Geliştirme makinesinden görüntü çeken veya gönderen bir geliştirici. | [az acr login](/cli/azure/acr?view=azure-cli-latest#az-acr-login) |
| Gözetimsiz kimlik/hizmet kimliği | Kullanıcıyla doğrudan ilgili olmayan derleme ve dağıtım işlem hatları. | [Hizmet sorumlusu](container-registry-authentication.md#service-principal) |

Azure Container Registry kimlik doğrulaması hakkında ayrıntılı bilgi edinmek için bkz. [Azure kapsayıcı kayıt defteri ile kimlik doğrulama](container-registry-authentication.md).

## <a name="manage-registry-size"></a>Kayıt defteri boyutunu yönetme

Her depolama alanı kısıtlamaları [kapsayıcı kayıt defteri SKU] [ container-registry-skus] tipik senaryolara yöneliktir: **Temel** başlangıç için **standart** üretim uygulamalarının çoğu için ve **Premium** hiper ölçekli performans ve [coğrafi çoğaltma][container-registry-geo-replication]. Kayıt defterinizin kullanım ömrü boyunca kullanılmayan içerikleri düzenli olarak silerek boyutunu yönetmeniz gerekir.

Azure CLI komutunu [az acr show-usage] [ az-acr-show-usage] geçerli kayıt defterinizin boyutunu görüntülemek için:

```console
$ az acr show-usage --resource-group myResourceGroup --name myregistry --output table
NAME      LIMIT         CURRENT VALUE    UNIT
--------  ------------  ---------------  ------
Size      536870912000  185444288        Bytes
Webhooks  100                            Count
```

Kullanılan geçerli depolama da bulabilirsiniz **genel bakış** Azure portalında kayıt defterinizin:

![Azure portalındaki kayıt defteri kullanım bilgileri][registry-overview-quotas]

### <a name="delete-image-data"></a>Görüntü verilerini sil

Azure Container Registry, kapsayıcı kayıt defterinden görüntü verilerini silmek için çeşitli yöntemler destekler. Etikete göre görüntüleri silin veya Özet bildirim veya tam bir depoyu Sil.

Kayıt defterinizden görüntü verileri silme hakkında daha fazla ayrıntı için de dahil olmak üzere (bazen çağrılan "sarkan" veya "yalnız bırakılmış") etiketlenmemiş görüntüleri görmek [Sil kapsayıcı görüntülerini Azure Container Registry'de](container-registry-delete.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure Container Registry, SKU adı verilen ve her biri farklı özellikler sağlayan birkaç katmanda sunulmaktadır. Sunulan SKU’lar hakkında ayrıntılı bilgi için bkz. [Azure Container Registry SKU’ları](container-registry-skus.md).

<!-- IMAGES -->
[delete-repository-portal]: ./media/container-registry-best-practices/delete-repository-portal.png
[registry-overview-quotas]: ./media/container-registry-best-practices/registry-overview-quotas.png

<!-- LINKS - Internal -->
[az-acr-repository-delete]: /cli/azure/acr/repository#az-acr-repository-delete
[az-acr-show-usage]: /cli/azure/acr#az-acr-show-usage
[azure-cli]: /cli/azure
[azure-portal]: https://portal.azure.com
[container-registry-geo-replication]: container-registry-geo-replication.md
[container-registry-skus]: container-registry-skus.md
