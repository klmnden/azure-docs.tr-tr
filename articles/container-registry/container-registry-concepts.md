---
title: Depoları ve görüntülerini Azure Container Registry hakkında
description: Azure kapsayıcısı kayıt defterleri, depolar ve kapsayıcı görüntüleri ile ilgili önemli kavramlar giriş.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 07/01/2019
ms.author: danlep
ms.openlocfilehash: a14f0a2a86c5e4922fcddf3c92d48c6dfb1497a3
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67800334"
---
# <a name="about-registries-repositories-and-images"></a>Kayıt defterleri, depolar ve görüntüler hakkında

Bu makalede, kapsayıcı kayıt defterleri, depoları, kapsayıcı görüntülerinin ve ilgili yapıtlardan anahtar kavramlar tanıtılmaktadır. 

## <a name="registry"></a>Kayıt defteri

Bir kapsayıcı *kayıt defteri* depolar ve kapsayıcı görüntülerini dağıtan bir hizmettir. Docker Hub, açık kaynak topluluğundan destekleyen ve bir genel katalog görüntüleri olarak hizmet veren bir ortak kapsayıcı kayıt defteri olur. Azure Container Registry sağlar resimlerinin, tümleşik kimlik doğrulaması ile doğrudan denetimi kullanıcılarla [coğrafi çoğaltma](container-registry-geo-replication.md) genel dağıtım ve güvenilirlik ağa yakın dağıtımlar için destek [ sanal ağ ve güvenlik duvarı yapılandırması](container-registry-vnet.md), [etiketi kilitleme](container-registry-image-lock.md), ve diğer birçok gelişmiş özellikleri. 

Docker kapsayıcı görüntüleri ek olarak, Azure Container Registry destekler ilgili [içerik yapıtları](container-registry-image-formats.md) açık kapsayıcı girişim (OCI) görüntü biçimlerini de dahil olmak üzere.

## <a name="content-addressable-elements-of-an-artifact"></a>Bir yapıtın içeriği adreslenebilir öğeleri

Bir Azure container registry'den bir yapının adresi aşağıdaki öğeleri içerir. 

```
[loginUrl]/[namespace]/[artifact:][tag]
```

* **loginUrl** -kayıt defteri ana bilgisayar tam adı. Azure container registry kayıt defteri ana bilgisayarında şu biçimdedir *myregistry*. azurecr.io (tamamı küçük harflerle). Bir Azure container registry'ye Docker veya diğer yapıtları çekme veya anında iletme istemci araçları kullanılırken loginUrl belirtmeniz gerekir. 
* **ad alanı** - eğik çizgi ile ayrılmış mantıksal gruplandırma ilgili görüntüleri ve yapıtları - Örneğin, bir çalışma grubunda veya uygulama
* **yapıt** -özel görüntü veya yapı için bir depo adı
* **Etiket** -belirli bir resim veya bir depoda depolanan yapıt sürümünü


Örneğin, görüntüyü Azure container registry tam adı aşağıdaki gibi görünmelidir:

```
myregistry.azurecr.io/marketing/campaign10-18/email-sender:v2
```

Aşağıdaki bölümlerde bu öğeler hakkında ayrıntılar için bkz.

## <a name="repository-name"></a>Depo adı

Kapsayıcı kayıt defterleri yönetme *depoları*, kapsayıcı görüntüleri veya diğer yapılar ile aynı adı taşıyan ancak farklı etiketler koleksiyonu. Örneğin, aşağıdaki üç görüntü "acr-helloworld" deposunda şunlardır:

```
acr-helloworld:latest
acr-helloworld:v1
acr-helloworld:v2
```

Depo adı da dahil edebileceğiniz [ad alanları](container-registry-best-practices.md#repository-namespaces). Ad alanları İleri eğik çizgi ile ayrılmış depo adları, örneğin kullanarak Grup görüntülere izin ver:

```
marketing/campaign10-18/web:v2
marketing/campaign10-18/api:v3
marketing/campaign10-18/email-sender:v2
product-returns/web-submission:20180604
product-returns/legacy-integrator:20180715
```

## <a name="image"></a>Image

Bir kapsayıcı görüntüsü veya bir kayıt defteri içinde başka bir yapı bir veya daha fazla etiket ile ilişkilidir, bir veya daha fazla katmanları var ve bir bildirim tarafından tanımlanır. Bu bileşenlerin birbirleriyle nasıl ilişkili olduğunu anlamak, kayıt defterinizin etkili bir şekilde yönetmenize yardımcı olabilir.

### <a name="tag"></a>Etiket

*Etiketi* görüntü veya diğer yapıt sürümü belirtir. Bir havuz içindeki tek bir yapı bir veya daha çok etiketleri atanabilir ve "etiketlenmemiş." olabilir Diğer bir deyişle, görüntünün veri (katmanları) kayıt defterinde kalırken tüm etiketleri bir görüntüden silebilirsiniz.

Depo (veya havuzu ve ad alanı) artı bir etiket görüntünün adını tanımlar. İtme veya çekme işleminde adını belirterek bir görüntüyü çekin ve gönderebilir.

Kapsayıcı görüntülerini nasıl etiketi geliştirebilir veya bunları dağıtmak için senaryolar tarafından yönlendirilir. Örneğin, kararlı etiketleri temel görüntülerini ve görüntülerini dağıtmak için benzersiz etiket koruma için önerilir. Daha fazla bilgi için [kapsayıcı görüntüleri etiketleme ve sürüm oluşturma için öneriler](container-registry-image-tag-version.md).

### <a name="layer"></a>Katman

Kapsayıcı görüntüleri oluşan bir veya daha fazla *katmanları*, her bir satır görüntüyü tanımlayan Dockerfile içinde karşılık gelen. Bir kayıt defterindeki görüntüleri ortak katmanları, depolama verimliliği artırma paylaşın. Örneğin, birkaç görüntüyü farklı depolardaki aynı Alpine Linux temel katmana paylaşabilir, ancak katman yalnızca bir kopyasını kayıt defterinde depolanır.

Katman paylaşımı da ortak katmanları paylaşımı ile birden çok görüntü katmanı dağıtım düğümlerine iyileştirir. Örneğin, görüntüyü bir düğümde oluşturucularını Alpine Linux katmanla içeriyorsa, sonraki çekme aynı katman başvuran farklı bir resim, katman düğüme aktarmaz. Bunun yerine, düğüm üzerinde mevcut katman başvuruyor.

Güvenli bir yalıtım ve olası katman işleme karşı koruma sağlamak için Katmanlar arasında kayıt defterleri paylaşılmaz.

### <a name="manifest"></a>Bildirimi

Her bir kapsayıcı görüntüsü veya bir kapsayıcı kayıt defterine gönderilmiştir yapıt ile ilişkili bir *bildirim*. Bildirimi kayıt defteri tarafından oluşturulan görüntüyü gönderildiğinde yeniden, benzersiz olarak resmi tanımlar ve katmanları belirtir. Azure CLI komutuyla bir depo için bildirimleri listeleyebilirsiniz [az acr depo show-bildirimleri][az-acr-repository-show-manifests]:

```azurecli
az acr repository show-manifests --name <acrName> --repository <repositoryName>
```

Örneğin, "acr-helloworld" depo için bildirim özetleyen listesi:

```console
$ az acr repository show-manifests --name myregistry --repository acr-helloworld
[
  {
    "digest": "sha256:0a2e01852872580b2c2fea9380ff8d7b637d3928783c55beb3f21a6e58d5d108",
    "tags": [
      "latest",
      "v3"
    ],
    "timestamp": "2018-07-12T15:52:00.2075864Z"
  },
  {
    "digest": "sha256:3168a21b98836dda7eb7a846b3d735286e09a32b0aa2401773da518e7eba3b57",
    "tags": [
      "v2"
    ],
    "timestamp": "2018-07-12T15:50:53.5372468Z"
  },
  {
    "digest": "sha256:7ca0e0ae50c95155dbb0e380f37d7471e98d2232ed9e31eece9f9fb9078f2728",
    "tags": [
      "v1"
    ],
    "timestamp": "2018-07-11T21:38:35.9170967Z"
  }
]
```

### <a name="manifest-digest"></a>Bildirim özeti

Bildirimler, benzersiz bir SHA-256 karması ile tanımlanır veya *bildirim Özet*. Her bir görüntü veya yapıt--veya değil--etiketli olup olmadığını, Özet tarafından tanımlanır. Görüntü katmanı verileriyle aynı olan başka bir görüntü olsa bile Özet değeri benzersizdir. Bu mekanizma, tekrar tekrar aynı şekilde etiketli bir registry'ye görüntüleri gönderme geçmesini sağlar ' dir. Örneğin, sürekli olarak gönderebilir `myimage:latest` hatasız kayıt defterinize çünkü her görüntü kendi benzersiz Özet tarafından tanımlanır.

Çekme işlemi, Özet belirterek, bir kayıt defterinden görüntü çekebilirsiniz. Bazı sistemler bile aynı şekilde etiketli görüntüyü daha sonra kayıt defterine gönderildi çekilen görüntü sürümü garanti eder, çünkü Özet çekmek için yapılandırılabilir.

Örneğin, "acr-helloworld" deposundan bir görüntüsü tarafından bildirim Özet çekin:

```console
$ docker pull myregistry.azurecr.io/acr-helloworld@sha256:0a2e01852872580b2c2fea9380ff8d7b637d3928783c55beb3f21a6e58d5d108
```

> [!IMPORTANT]
> Tekrar tekrar aynı etiketleri ile değiştirilmiş görüntüleri gönderirseniz, yalnız bırakılmış görüntüler--etiketsiz kalır, ancak yine de, kayıt defteri alanı kullanma görüntüler oluşturabilirsiniz. Etiketlenmemiş görüntüleri listesinde veya resimleri görüntülemesine Azure CLI veya Azure portalında etikete göre gösterilmez. Ancak, bunların katmanlarında hala mevcut ve kayıt alanı kullanır. Etiketlenmemiş görüntüleri tarafından kullanılan alanı boşaltmayı içeren hakkında daha fazla bilgi için bkz: [Sil kapsayıcı görüntülerini Azure Container Registry'de](container-registry-delete.md).


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [resim depolama](container-registry-storage.md) ve [desteklenen içerik biçimleri](container-registry-image-formats.md) Azure Container Registry'de.

<!-- LINKS - Internal -->
[az-acr-repository-show-manifests]: /cli/azure/acr/repository#az-acr-repository-show-manifests


