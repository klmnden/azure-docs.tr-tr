---
title: Klasik Azure kapsayıcı kayıt defteri yükseltme
description: Genişletilmiş yararlanmak özellik kümesini temel, standart ve Premium yönetilen kapsayıcı kayıt defterleri yönetilmeyen Klasik kapsayıcı kayıt defterinizde yükselterek.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 08/28/2018
ms.author: danlep
ms.openlocfilehash: 077ca3c876a3078e7e627dbfefdff38e09ec57b9
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55228364"
---
# <a name="upgrade-a-classic-container-registry"></a>Klasik kapsayıcı kayıt defterini yükseltme

Azure Container Registry (ACR) birkaç hizmet katmanlarında kullanılabilir [SKU'ları bilinen](container-registry-skus.md). ACR ilk sürümünde sunulan tek bir SKU temel, standart ve Premium SKU'ları devralınan çeşitli özellikler eksik Klasik, (toplu adıyla *yönetilen* kayıt defterleri).

Klasik SKU kullanımdan kaldırılıyor ve Mart 2019 sonra kullanılamaz. Bu makalede, bunların gelişmiş özellik kümesi yararlanabilir, böylece yönetilmeyen Klasik kayıt defterinizin yönetilen SKU'lar birine geçirme işlemi açıklanmaktadır.

## <a name="why-upgrade"></a>Neden yükseltilsin mi?

Klasik kayıt defteri SKU okunuyor **kullanım dışı**ve kullanılamaz durumda gelen **Mart 2019**. Tüm mevcut Klasik kayıt defterleri Mart 2019'öncesinde yükseltilmelidir.

Planlanan kullanımdan kaldırma ve yönetilmeyen Klasik kayıt defterleri sınırlı yeteneklerini nedeniyle tüm Klasik kayıt defterleri temel, standart veya Premium yükseltilen yönetilen kayıt defterleri olması. Bu üst düzey bir SKU'ları, kayıt defteri serimizde, Azure'un daha derin bir şekilde tümleştirin.

Yönetilen kayıt defterleri sağlar:

* Azure Active Directory tümleştirmesinin [tek oturum açma](container-registry-authentication.md#individual-login-with-azure-ad)
* Görüntü ve etiket silme desteği
* [Coğrafi çoğaltma](container-registry-geo-replication.md)
* [Web kancaları](container-registry-webhook.md)

Klasik kayıt defterini, kayıt defteri oluşturduğunuzda, Azure otomatik olarak Azure aboneliğinizdeki sağlayan depolama hesabına bağlıdır. Bunun aksine, temel, standart ve Premium SKU'ları Azure'nın yararlanmak [gelişmiş depolama özellikleri](container-registry-storage.md) tarafından şeffaf bir şekilde görüntülerinizin depolama, işleme. Kendi aboneliğinizde ayrı bir depolama hesabı oluşturulmaz.

Yönetilen kayıt defteri depolamasını aşağıdaki avantajları sağlar:

* Kapsayıcı görüntüleri [şifrelenen](container-registry-storage.md#encryption-at-rest).
* Görüntüleri kullanarak depolanan [coğrafi olarak yedekli depolama](container-registry-storage.md#geo-redundant-storage), işlemlerini görüntülerinizi çok bölgeli çoğaltma ile yedeklemeyi.
* Olanağı serbestçe [SKU'lar arasında taşıma](container-registry-skus.md#changing-skus), daha yüksek performans, daha üst düzey bir SKU seçtiğinizde etkinleştirme. Her bir SKU ile gereksinimlerinizi arttıkça ACR aktarım hızı gereksinimlerinizi karşılayabilir.
* Basitleştirilmiş rights management kayıt defteri ve depolama alanı için birleşik güvenlik modeli sağlar. Ayrıca ayrı bir depolama hesabı için izinleri yönetmenize gerek kalmadan kapsayıcı kayıt defteri için yalnızca izinleri yönetirsiniz.

ACR görüntü depolama ile ilgili ek ayrıntılar için bkz. [kapsayıcı görüntüsü Azure Container Registry depolamada](container-registry-storage.md).

## <a name="migration-considerations"></a>Geçiş konuları

Yönetilen kayıt defteri için Klasik kayıt defterini değiştirdiğinizde, Azure var olan tüm kapsayıcı görüntülerini aboneliğinizdeki ACR oluşturulan depolama hesabından Azure tarafından yönetilen bir depolama hesabına kopyalamanız gerekir. Kayıt defterinizin boyutuna bağlı olarak, bu işlem birkaç saat için birkaç dakika sürebilir.

Dönüştürme işlemi sırasında tüm `docker push` işlemleri engellendi, ancak `docker pull` çalışmaya devam eder.

Silinmemesi veya değiştirilmemesi dönüştürme işlemi sırasında Klasik kayıt defterini yedekleme depolama hesabı içeriği. Bunun yapılması, kapsayıcı görüntülerinizi bozulmasına neden olabilir.

Geçiş işlemi tamamlandıktan sonra aboneliğinizdeki ilk Klasik kayıt defterinizin desteklenen depolama hesabı artık ACR tarafından kullanılır. Geçiş işleminin başarılı olduğunu doğruladıktan sonra maliyetini en aza indirmek için depolama hesabını silerken göz önünde bulundurun.

>[!IMPORTANT]
> Klasikten yönetilen SKU'lar birine yükseltme bir **tek yönlü işlem**. Klasik kayıt defterine temel, standart veya Premium dönüştürdükten sonra Klasik olarak geri alınamaz. Ancak ücretsiz kayıt defteriniz için yeterli kapasiteye sahip yönetilen SKU'lar arasında taşıyabilirsiniz.

## <a name="how-to-upgrade"></a>Yükseltme

Yönetilmeyen bir Klasik kayıt defterini, çeşitli yollarla yönetilen SKU'lar birine yükseltebilirsiniz. Aşağıdaki bölümlerde, biz kullanma işlemi açıklanmaktadır [Azure CLI] [ azure-cli] ve [Azure portalında][azure-portal].

## <a name="upgrade-in-azure-cli"></a>Azure CLI'de yükseltme

Azure CLI Klasik kayıt defterindeki yükseltmek için yürütme [az acr update] [ az-acr-update] komut ve yeni SKU kayıt defteri için belirtin. Aşağıdaki örnekte, bir Klasik kayıt defterini adlı *myclassicregistry* Premium SKU için yükseltme:

```azurecli-interactive
az acr update --name myclassicregistry --sku Premium
```

Geçiş tamamlandığında, aşağıdakine benzer bir çıktı görmeniz gerekir. Dikkat `sku` "Premium" ve `storageAccount` "Azure artık bu kayıt defteri için resim depolama yönetir gösteren," null.

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

Yönetilen kayıt defteri SKU, kapasite üst sınırı Klasik kayıt defterinizin boyutunu'dan küçük belirtirseniz, aşağıdakine benzer bir hata iletisi alırsınız.

`Cannot update the registry SKU due to reason: Registry size 12936251113 bytes exceeds the quota value 10737418240 bytes for SKU Basic. The suggested SKU is Standard.`

Benzer bir hata alırsanız, çalıştırma [az acr update] [ az-acr-update] komutunu tekrar ve görüntülerinizi uyum sonraki en yüksek düzey bir SKU önerilen SKU, belirtin.

## <a name="upgrade-in-azure-portal"></a>Azure portalında yükseltme

Azure portalını kullanarak Klasik kayıt defterini yükseltme yaptığınızda Azure kendi görüntülerinizi uyum en düşük düzey SKU otomatik olarak seçer. Örneğin, kayıt defterinizin resimlerdeki 12 GiB içeriyorsa, Azure otomatik olarak seçer ve standart Klasik kayıt defterini dönüştürür (GiB maksimum 100).

Azure portalını kullanarak Klasik kayıt defterinizin yükseltmek için kapsayıcı kayıt defterine gidin **genel bakış** seçip **yönetilen kayıt defterine yükseltme**.

![Klasik kayıt defterini yükseltme Azure portalı kullanıcı arabirimini düğmesi][update-classic-01-upgrade]

Seçin **Tamam** yönetilen kayıt defterine yükseltmek istediğinizi onaylayın.

![Klasik kayıt defterini yükseltme onay Azure portalındaki kullanıcı Arabirimi][update-classic-02-confirm]

Portal, geçiş sırasında gösterir defterinin **sağlama durumu** olduğu *güncelleştirme*. Daha önce belirtildiği `docker push` işlemleri geçiş sırasında devre dışı değil silmeniz gerekir ve güncelleştirme geçiş Bunun yapılması--sürerken Klasik kayıt defteri tarafından kullanılan depolama hesabı görüntüsü bozulmasına neden olabilir.

![Klasik kayıt defterini yükseltme ilerleme durumu Azure portalındaki kullanıcı Arabirimi][update-classic-03-updating]

Geçiş tamamlandığında **sağlama durumu** gösterir *başarılı*, ve bir kez daha `docker push` kayıt defterinize.

![Klasik kayıt defterini yükseltme tamamlanma durumu Azure portalındaki kullanıcı Arabirimi][update-classic-04-updated]

## <a name="next-steps"></a>Sonraki adımlar

Klasik kayıt defterine temel, standart veya Premium yükselttikten sonra Azure, artık başlangıçta Klasik kayıt defterini desteklenen depolama hesabı kullanır. Maliyeti azaltmak için depolama hesabı veya eski kapsayıcı görüntülerinizi içeren hesabı içinde Blob kapsayıcısını silme göz önünde bulundurun.

<!-- IMAGES -->
[update-classic-01-upgrade]: ./media/container-registry-upgrade/update-classic-01-upgrade.png
[update-classic-02-confirm]: ./media/container-registry-upgrade/update-classic-02-confirm.png
[update-classic-03-updating]: ./media/container-registry-upgrade/update-classic-03-updating.png
[update-classic-04-updated]: ./media/container-registry-upgrade/update-classic-04-updated.png

<!-- LINKS - internal -->
[az-acr-update]: /cli/azure/acr#az-acr-update
[azure-cli]: /cli/azure/install-azure-cli
[azure-portal]: https://portal.azure.com