---
title: "Klasik Azure kapsayıcı kayıt defteri yükseltme"
description: "Genişletilmiş yararlanmak özellik kümesini temel, standart ve Premium yönetilmeyen Klasik kapsayıcı kaydınız yükselterek kapsayıcı kayıt defterleri yönetilen."
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: article
ms.date: 12/20/2017
ms.author: marsma
ms.openlocfilehash: 19090bb69d7165c1e904450dc93b925e23e44782
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="upgrade-a-classic-container-registry"></a>Klasik kapsayıcı kayıt defteri yükseltme

Azure kapsayıcı kayıt defteri (ACR) birkaç hizmet katmanında kullanılabilir [SKU'ları bilinen](container-registry-skus.md). Tek bir SKU ACR ilk sürümünde sunulan temel, standart ve Premium SKU'ları devralınmış çeşitli özellikler eksik Klasik, (topluca bilinen *yönetilen* kayıt defterleri). Bu makalede, böylece bunların gelişmiş özellik kümesi yararlanabilir yönetilmeyen Klasik kaydınız yönetilen SKU'ları birine nasıl geçirileceği ayrıntıları verilmektedir.

## <a name="why-upgrade"></a>Neden yükseltme?

Yönetilmeyen Klasik kayıt defterleri sınırlı özellikleri nedeniyle, temel, standart veya Premium yükseltilen yönetilen kayıt defterleri tüm Klasik kayıt defterleri olmasını öneririz. Bu üst düzey SKU'ları kayıt defterinde Azure özelliklerini daha kapsamlı tümleştirin.

Yönetilen kayıt defterleri sağlar:

* Azure Active Directory tümleştirme için [tek tek oturum açma](container-registry-authentication.md#individual-login-with-azure-ad)
* Görüntü ve etiket silme desteği
* [Coğrafi çoğaltma](container-registry-geo-replication.md)
* [Web kancaları](container-registry-webhook.md)

En önemlisi, Klasik kayıt defteri kayıt oluşturduğunuzda otomatik olarak Azure aboneliğinizde sağlanan bu Azure depolama hesabında bağlıdır. Bunun aksine, temel, standart ve Premium SKU'ları yararlanmak *yönetilen depolama*. Diğer bir deyişle, Azure depolama saydam yönetir,--görüntülerinizin ayrı bir depolama hesabı kendi abonelikte oluşturulmaz.

Yönetilen kayıt defteri depolama aşağıdaki avantajları sağlar:

* Kapsayıcı görüntüleri [şifrelenen](../storage/common/storage-service-encryption.md).
* Görüntüleri kullanılarak depolanmış olan [coğrafi olarak yedekli depolama](../storage/common/storage-redundancy.md#geo-redundant-storage), modemlerin yedekleme bölgeli çoğaltma ile görüntüler.
* Özelliğine ücretsiz [SKU'ları arasında taşıma](container-registry-skus.md#changing-skus), üst düzey bir SKU seçtiğinizde daha yüksek verimlilik etkinleştirme. Gereksinimlerinize arttıkça her SKU ile üretilen iş gereksinimlerinizi ACR karşılayabilir.
* Birleşik güvenlik modeli kayıt defteri ve depolama alanı için Basitleştirilmiş hak yönetimi sağlar. Ayrıca ayrı bir depolama hesabı için izinleri yönetmek zorunda kalmadan yalnızca kapsayıcı kayıt defteri izinlerini yönetin.

## <a name="migration-considerations"></a>Geçiş konuları

Yönetilen bir kayıt defterine Klasik kayıt defterini değiştirdiğinizde, Azure var olan tüm kapsayıcı görüntüleri aboneliğinizde ACR oluşturulan depolama hesabından Azure tarafından yönetilen bir depolama hesabı kopyalamanız gerekir. Kayıt defteri boyutuna bağlı olarak, bu işlem birkaç saat için birkaç dakika sürebilir.

Dönüştürme işlemi sırasında tüm `docker push` operations engellendi, ancak `docker pull` çalışmaya devam eder.

Silmeyin veya Klasik kaydınız dönüştürme işlemi sırasında yedekleme depolama hesabı içeriğini değiştirmeyin. Bunun yapılması, kapsayıcı görüntülerinizin bozulmasına neden olabilir.

Geçiş işlemi tamamlandıktan sonra ilk olarak Klasik kaydınız yedeklenen aboneliğinizde depolama hesabı artık ACR tarafından kullanılır. Geçiş işleminin başarılı olduğunu doğruladıktan sonra maliyeti en aza indirmek için depolama hesabı silmeyi düşünün.

>[!IMPORTANT]
> Yönetilen SKU'ları birine Klasikten yükseltme işlemi bir **tek yönlü işlem**. Klasik kayıt defterine temel, standart veya Premium dönüştürdükten sonra Klasik olarak geri alınamaz. Ancak, ücretsiz olarak yönetilen SKU'ları kayıt için yeterli kapasiteye sahip arasında taşıyabilirsiniz.

## <a name="how-to-upgrade"></a>Yükseltme

Yönetilen SKU'ları çeşitli yollardan biri yönetilmeyen bir Klasik kayıt defteri yükseltebilirsiniz. Aşağıdaki bölümlerde, biz kullanma işlemini açıklayan [Azure CLI] [ azure-cli] ve [Azure portal][azure-portal].

## <a name="upgrade-in-azure-cli"></a>Azure CLI yükseltme

Azure CLI Klasik defterinde yükseltmek için yürütme [az acr güncelleştirme] [ az-acr-update] komut ve kayıt defteri için yeni SKU belirtin. Aşağıdaki örnekte, adlı bir Klasik kayıt defteri *myclassicregistry* Premium SKU'ya yükseltme:

```azurecli-interactive
az acr update --name myclassicregistry --sku Premium
```

Geçiş tamamlandığında, aşağıdakine benzer bir çıktı görmeniz gerekir. Dikkat `sku` "Premium" olduğundan ve `storageAccount` "NULL Azure şimdi bu kayıt defteri görüntü depolama yönetir belirten,".

```JSON
{
  "adminUserEnabled": false,
  "creationDate": "2017-12-12T21:23:29.300547+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry",
  "location": "eastus",
  "loginServer": "myregistry.azurecr.io",
  "name": "myregistry",
  "provisioningState": "Succeeded",
  "resourceGroup": "myresourcegroup",
  "sku": {
    "name": "Premium",
    "tier": "Premium"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

Yönetilen bir kayıt defteri, maksimum kapasite, Klasik kayıt defterinizi boyuttan daha küçük SKU belirtirseniz, aşağıdakine benzer bir hata iletisi alırsınız.

`Cannot update the registry SKU due to reason: Registry size 12936251113 bytes exceeds the quota value 10737418240 bytes for SKU Basic. The suggested SKU is Standard.`

Benzer bir hata alırsanız, çalıştırmak [az acr güncelleştirme] [ az-acr-update] komutunu yeniden ve resimlerinizi uyum sonraki en yüksek düzey SKU önerilen SKU belirtin.

## <a name="upgrade-in-azure-portal"></a>Azure portalında yükseltme

Azure portalı kullanarak bir Klasik kayıt defteri yükselttiğinizde, Azure görüntülerinizi uyum en düşük düzeyde SKU otomatik olarak seçer. Örneğin, kayıt defteri görüntülerinde 12 Gib'den içeriyorsa, Azure otomatik olarak seçer ve standart olarak Klasik kayıt defteri dönüştürür (100 Gib'den maksimum).

Azure portalını kullanarak Klasik kaydınız yükseltmek için kapsayıcı kayıt defterine gidin **genel bakış** seçip **yükseltme yönetilen kayıt defterine**.

![Klasik kayıt defteri yükseltme düğme Azure portalında kullanıcı Arabirimi][update-classic-01-upgrade]

Seçin **Tamam** yönetilen bir kayıt defterine yükseltmek istediğinizi onaylamak için.

![Klasik kayıt defteri yükseltme UI Azure Portalı'nda onay][update-classic-02-confirm]

Geçiş sırasında portal bildiren kayıt defterindeki **sağlama durumu** olan *güncelleştirme*. Daha önce belirtildiği gibi `docker push` işlemler geçiş sırasında devre dışı ve değil silmeniz gerekir veya depolama hesabı geçiş Bunun yapılması--sürerken Klasik kayıt defteri tarafından kullanılan güncelleştirme Görüntü bozulması neden olur.

![Klasik kayıt defteri yükseltme Azure portal UI sürüyor][update-classic-03-updating]

Geçiş tamamlandığında, **sağlama durumu** gösterir *başarılı*, ve bir kez daha `docker push` , kayıt defterine.

![Klasik kayıt defteri güncelleştirme UI Azure portalında tamamlanma durumu][update-classic-04-updated]

## <a name="next-steps"></a>Sonraki adımlar

Klasik kayıt defterine temel, standart veya Premium yükseltme yaparsam sonra Azure Klasik kayıt defteri ilk olarak yedeklenen depolama hesabı artık kullanır. Maliyetini azaltmak için depolama hesabı veya Blob kapsayıcısına eski kapsayıcı görüntülerinizi içeren hesabını silme göz önünde bulundurun.

<!-- IMAGES -->
[update-classic-01-upgrade]: ./media/container-registry-upgrade\update-classic-01-upgrade.png
[update-classic-02-confirm]: ./media/container-registry-upgrade\update-classic-02-confirm.png
[update-classic-03-updating]: ./media/container-registry-upgrade\update-classic-03-updating.png
[update-classic-04-updated]: ./media/container-registry-upgrade\update-classic-04-updated.png

<!-- LINKS - internal -->
[az-acr-update]: /cli/azure/acr#az_acr_update
[azure-cli]: /cli/azure/install-azure-cli
[azure-portal]: https://portal.azure.com