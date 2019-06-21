---
title: Azure Container Registry'de resim kaynakları Sil
description: Etkili bir şekilde kapsayıcı görüntüsünü verileri silerek kayıt defteri boyutunu yönetme hakkında ayrıntılar.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 06/17/2019
ms.author: danlep
ms.openlocfilehash: c544c8ed6fbfcb859ff1ff01e7bedf46cfb21418
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67295144"
---
# <a name="delete-container-images-in-azure-container-registry"></a>Kapsayıcı görüntülerini Azure Container Registry'de Sil

Azure kapsayıcı kayıt defterinizin boyutunu korumak için düzenli aralıklarla eski görüntü verilerini silmeniz gerekir. Üretim ortamına dağıtmış bazı kapsayıcı görüntülerini daha uzun vadeli depolama gerektirirken, diğerleri genellikle daha hızlı bir şekilde silinebilir. Örneğin, bir otomatik yapı ve test senaryosu, kayıt defterinizin hızla hiçbir zaman dağıtılmış olabilir ve kısa süre içinde derleme ve test geçişi tamamladıktan sonra temizlenebilir görüntüleri ile doldurabilirsiniz.

Görüntü verilerini birkaç farklı yolla silebilirsiniz çünkü her silme işlemi depolama kullanımını nasıl etkilediğini anlamak önemlidir. Bu makalede ilk Docker kayıt defteri ve kapsayıcı görüntüleri alan bileşenleri tanıtır ve ardından görüntü verilerini silmek için çeşitli yöntemler ele alınmaktadır. Örnek betikler, silme işlemleri otomatikleştirmek için sağlanır.

## <a name="registry"></a>Kayıt defteri

Bir kapsayıcı *kayıt defteri* depolar ve kapsayıcı görüntülerini dağıtan bir hizmettir. Azure Container Registry özel Docker kapsayıcısı kayıt defterleri Azure sağlarken docker hub'ı genel bir Docker kapsayıcısı kayıt ' dir.

## <a name="repository"></a>Havuz

Kapsayıcı kayıt defterleri yönetme *depoları*, kapsayıcı görüntüleri ile aynı adı taşıyan ancak farklı etiketler koleksiyonu. Örneğin, aşağıdaki üç görüntü "acr-helloworld" deposunda şunlardır:

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

## <a name="components-of-an-image"></a>Görüntünün bileşenleri

Bir kayıt defteri içinde bir kapsayıcı görüntüsü bir veya daha fazla etiket ile ilişkilidir, bir veya daha fazla katmanları var ve bir bildirim tarafından tanımlanır. Bu bileşenlerin birbirleriyle nasıl ilişki kuracağını, kayıt defteri alanı boşaltmak için en iyi yöntemi belirlemenize yardımcı olabileceğini anlama.

### <a name="tag"></a>Etiket

Görüntünün *etiketi* sürümünü belirtir. Bir havuz içindeki tek bir görüntü, bir veya daha çok etiketleri atanabilir ve "etiketlenmemiş." olabilir Diğer bir deyişle, görüntünün veri (katmanları) kayıt defterinde kalırken tüm etiketleri bir görüntüden silebilirsiniz.

Depo (veya havuzu ve ad alanı) artı bir etiket görüntünün adını tanımlar. İtme veya çekme işleminde adını belirterek bir görüntüyü çekin ve gönderebilir.

Azure Container Registry gibi bir özel kayıt defterindeki görüntü adı kayıt defteri ana bilgisayar tam adı da içerir. ACR görüntü kayıt defteri konak biçimindedir *acrname.azurecr.io* (tamamı küçük harflerle). Örneğin, "Pazarlama" ad alanı önceki bölümde ilk görüntüde tam adı olacaktır:

```
myregistry.azurecr.io/marketing/campaign10-18/web:v2
```

En iyi etiketleme, görüntüye bir tartışma için bkz [Docker etiketleme: Docker görüntüleri etiketleme ve sürüm oluşturma için en iyi yöntemler][tagging-best-practices] blog gönderisi MSDN'de.

### <a name="layer"></a>Katman

Görüntüleri oluşan bir veya daha fazla *katmanları*, her bir satır görüntüyü tanımlayan Dockerfile içinde karşılık gelen. Bir kayıt defterindeki görüntüleri ortak katmanları, depolama verimliliği artırma paylaşın. Örneğin, birkaç görüntüyü farklı depolardaki aynı Alpine Linux temel katmana paylaşabilir, ancak katman yalnızca bir kopyasını kayıt defterinde depolanır.

Katman paylaşımı da ortak katmanları paylaşımı ile birden çok görüntü katmanı dağıtım düğümlerine iyileştirir. Örneğin, görüntüyü bir düğümde oluşturucularını Alpine Linux katmanla içeriyorsa, sonraki çekme aynı katman başvuran farklı bir resim, katman düğüme aktarmaz. Bunun yerine, düğüm üzerinde mevcut katman başvuruyor.

### <a name="manifest"></a>Bildirimi

Bir kapsayıcı kayıt defterine gönderilmiştir her bir kapsayıcı görüntüsü ile ilişkili bir *bildirim*. Bildirimi kayıt defteri tarafından oluşturulan görüntüyü gönderildiğinde yeniden, benzersiz olarak resmi tanımlar ve katmanları belirtir. Azure CLI komutuyla bir depo için bildirimleri listeleyebilirsiniz [az acr depo show-bildirimleri][az-acr-repository-show-manifests]:

```azurecli
az acr repository show-manifests --name <acrName> --repository <repositoryName>
```

Örneğin, bildirim listelemek için "acr-helloworld" depo özetleyen:

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

Bildirimler, benzersiz bir SHA-256 karması ile tanımlanır veya *bildirim Özet*. Her bir görüntü--veya değil--etiketli olup olmadığını, Özet tarafından tanımlanır. Görüntü katmanı verileriyle aynı olan başka bir görüntü olsa bile Özet değeri benzersizdir. Bu mekanizma, tekrar tekrar aynı şekilde etiketli bir registry'ye görüntüleri gönderme geçmesini sağlar ' dir. Örneğin, sürekli olarak gönderebilir `myimage:latest` hatasız kayıt defterinize çünkü her görüntü kendi benzersiz Özet tarafından tanımlanır.

Çekme işlemi, Özet belirterek, bir kayıt defterinden görüntü çekebilirsiniz. Bazı sistemler bile aynı şekilde etiketli görüntüyü daha sonra kayıt defterine gönderildi çekilen görüntü sürümü garanti eder, çünkü Özet çekmek için yapılandırılabilir.

Örneğin, bir görüntü tarafından bildirim Özeti "acr-helloworld" depodan çekerek:

```console
$ docker pull myregistry.azurecr.io/acr-helloworld@sha256:0a2e01852872580b2c2fea9380ff8d7b637d3928783c55beb3f21a6e58d5d108
```

> [!IMPORTANT]
> Tekrar tekrar aynı etiketleri ile değiştirilmiş görüntüleri gönderirseniz, yalnız bırakılmış görüntüler--etiketsiz kalır, ancak yine de, kayıt defteri alanı kullanma görüntüler oluşturabilirsiniz. Etiketlenmemiş görüntüleri listesinde veya resimleri görüntülemesine Azure CLI veya Azure portalında etikete göre gösterilmez. Ancak, bunların katmanlarında hala mevcut ve kayıt alanı kullanır. [Etiketlenmemiş görüntüleri silin](#delete-untagged-images) bu makalenin etiketlenmemiş görüntüleri tarafından kullanılan boşaltma alanı ele alınmaktadır.

## <a name="delete-image-data"></a>Görüntü verilerini sil

Görüntü verilerini çeşitli şekillerde kapsayıcı kayıt defterinizin silebilirsiniz:

* Silme bir [depo](#delete-repository): Tüm görüntüleri ve havuz içindeki tüm benzersiz katmanları siler.
* Silin [etiketi](#delete-by-tag): Görüntü, etiket, görüntü tarafından başvurulan tüm benzersiz katmanları ve resimle ilişkili tüm etiketleri siler.
* Silin [bildirim Özet](#delete-by-manifest-digest): Görüntü, görüntü tarafından başvurulan tüm benzersiz katmanları ve resimle ilişkili tüm etiketleri siler.

## <a name="delete-repository"></a>Depoyu Sil

Bir depoyu silme görüntülerinin depodaki tüm etiketleri, benzersiz katmanlar ve bildirimler gibi tüm siler. Bir depo sildiğinizde, depodaki benzersiz katmanları başvuru görüntüleri tarafından kullanılan depolama alanı kurtarın.

"Acr-helloworld" depo ve tüm etiketleri ve depo içindeki bildirimlerini aşağıdaki Azure CLI komutu siler. Katmanlar silinen bildirimleri tarafından başvurulan kayıt defterinde herhangi bir görüntü tarafından başvurulmayan, katmanı verilerini de, Kurtarma depolama alanı silindi.

```azurecli
 az acr repository delete --name myregistry --repository acr-helloworld
```

## <a name="delete-by-tag"></a>Etikete göre Sil

Depo adını ve etiketini silme işleminde belirterek bir depodan ayrı görüntüleri silebilirsiniz. Etikete göre sildiğinizde, herhangi bir benzersiz katman görüntüde (herhangi bir görüntü kayıt defteri tarafından paylaşılmayan katmanları) tarafından kullanılan depolama alanı kurtarın.

Etikete göre silmek için kullanın [az acr depo silme][az-acr-repository-delete] ve görüntü adını `--image` parametresi. Tüm Katmanlar görüntüye benzersiz ve resimle ilişkili etiketleri silinir.

Örneğin, silme "acr-helloworld:latest" Görüntü "myregistry" kayıt defterinden:

```azurecli
$ az acr repository delete --name myregistry --image acr-helloworld:latest
This operation will delete the manifest 'sha256:0a2e01852872580b2c2fea9380ff8d7b637d3928783c55beb3f21a6e58d5d108' and all the following images: 'acr-helloworld:latest', 'acr-helloworld:v3'.
Are you sure you want to continue? (y/n): y
```

> [!TIP]
> Silme *etikete göre* (etiketi kaldırma) bir etiketi silme işlemine kafanız olmamalıdır. Azure CLI komutuyla bir etiketi silebilirsiniz [az acr depo etiketini Kaldır][az-acr-repository-untag]. Görüntü çünkü'etiketini kaldırın, hiçbir alanı boşaltılana kendi [bildirim](#manifest) ve katman verileri kayıt defterinde kalır. Yalnızca etiket başvuru kendisini silinir.

## <a name="delete-by-manifest-digest"></a>Bildirim özeti tarafından Sil

A [bildirim Özet](#manifest-digest) birini, nBir veya birden çok etiketi ile ilişkili olabilir. Özet sildiğinizde, görüntüye benzersiz herhangi bir katman için veri katmanı olarak listesi tarafından başvurulan tüm etiketleri silinir. Veriler silinmez katman paylaşılan.

Özet silmek için silmek istediğiniz görüntü içeren depo için ilk listeden bildirim özetleyen. Örneğin:

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
  }
]
```

Ardından, silmek istediğiniz özet belirtin [az acr depo silme][az-acr-repository-delete] komutu. Komut biçimi şu şekildedir:

```azurecli
az acr repository delete --name <acrName> --image <repositoryName>@<digest>
```

Örneğin, yukarıdaki çıktıda ("v2" etiketiyle) son bildirimi silmek için listelenen:

```console
$ az acr repository delete --name myregistry --image acr-helloworld@sha256:3168a21b98836dda7eb7a846b3d735286e09a32b0aa2401773da518e7eba3b57
This operation will delete the manifest 'sha256:3168a21b98836dda7eb7a846b3d735286e09a32b0aa2401773da518e7eba3b57' and all the following images: 'acr-helloworld:v2'.
Are you sure you want to continue? (y/n): y
```

`acr-helloworld:v2` Görüntüsü herhangi bir katman veri görüntüsünü benzersiz olarak kayıt defterinden silindi. Bir bildirim birden çok etiketi ile ilişkili ise, ilişkili tüm etiketleri de silinir.

## <a name="delete-digests-by-timestamp"></a>Zaman damgası tarafından özetler Sil

Depo veya kayıt defteri boyutunu korumak için düzenli olarak belirli bir tarihten daha eski bildirim özetler silmeniz gerekebilir.

Aşağıdaki Azure CLI komutunu artan sırada belirtilen zaman damgası, daha eski bir depodaki tüm bildirim Özet listeler. Değiştirin `<acrName>` ve `<repositoryName>` ortamınız için uygun değerlerle. Tam tarih-saat ifade veya bu örnekte olduğu gibi bir tarih, zaman damgası olabilir.

```azurecli
az acr repository show-manifests --name <acrName> --repository <repositoryName> \
--orderby time_asc -o tsv --query "[?timestamp < '2019-04-05'].[digest, timestamp]"
```

Eski bildirim özetler belirledikten sonra belirtilen bir zaman damgası eski bildirim özetler silmek için aşağıdaki Bash betiğini çalıştırabilirsiniz. Azure CLI'yı gerektirir ve **xargs**. Varsayılan olarak, betik, hiçbir silme gerçekleştirir. Değişiklik `ENABLE_DELETE` değerini `true` görüntü silme işlemini etkinleştirmek için.

> [!WARNING]
> Aşağıdaki betiğe dikkatle--silinmiş bir görüntü veriler KURTARILAMAZ kullanılır. Bildirim özeti (aksine, görüntü adı) tarafından görüntüleri çekmek sistemleri varsa, bu betikleri çalıştırmamanız gerekir. Bildirim özetler silinmesi, bu sistemlerin görüntülerini kayıt defterinizden çekme öğesinden engeller. Bildirimi tarafından çekmek yerine benimsemeyi göz önünde bir *benzersiz etiketleme* düzeni, bir [önerilen en iyi][tagging-best-practices]. 

```bash
#!/bin/bash

# WARNING! This script deletes data!
# Run only if you do not have systems
# that pull images via manifest digest.

# Change to 'true' to enable image delete
ENABLE_DELETE=false

# Modify for your environment
# TIMESTAMP can be a date-time string such as 2019-03-15T17:55:00.
REGISTRY=myregistry
REPOSITORY=myrepository
TIMESTAMP=2019-04-05  

# Delete all images older than specified timestamp.

if [ "$ENABLE_DELETE" = true ]
then
    az acr repository show-manifests --name $REGISTRY --repository $REPOSITORY \
    --orderby time_asc --query "[?timestamp < '$TIMESTAMP'].digest" -o tsv \
    | xargs -I% az acr repository delete --name $REGISTRY --image $REPOSITORY@% --yes
else
    echo "No data deleted."
    echo "Set ENABLE_DELETE=true to enable deletion of these images in $REPOSITORY:"
    az acr repository show-manifests --name $REGISTRY --repository $REPOSITORY \
   --orderby time_asc --query "[?timestamp < '$TIMESTAMP'].[digest, timestamp]" -o tsv
fi
```

## <a name="delete-untagged-images"></a>Etiketlenmemiş görüntüleri Sil

Belirtildiği gibi [bildirim Özet](#manifest-digest) bölümünde, varolan bir etiketi kullanarak değiştirilmiş bir görüntüyü dağıtmaya **untags** yalnız bırakılmış (veya "Sallantıdaki") görüntüyü daha önce görüntü. Daha önce görüntünün bildirim--ve--katman verileri kayıt defterinde kalır. Aşağıdaki olaylar dizisi göz önünde bulundurun:

1. Görüntü gönderme *acr-helloworld* etiketiyle **son**: `docker push myregistry.azurecr.io/acr-helloworld:latest`
1. Depo için bildirimleri denetleyin *acr-helloworld*:

   ```console
   $ az acr repository show-manifests --name myregistry --repository acr-helloworld
   [
     {
       "digest": "sha256:d2bdc0c22d78cde155f53b4092111d7e13fe28ebf87a945f94b19c248000ceec",
       "tags": [
         "latest"
       ],
       "timestamp": "2018-07-11T21:32:21.1400513Z"
     }
   ]
   ```

1. Değiştirme *acr-helloworld* Dockerfile
1. Görüntü gönderme *acr-helloworld* etiketiyle **son**: `docker push myregistry.azurecr.io/acr-helloworld:latest`
1. Depo için bildirimleri denetleyin *acr-helloworld*:

   ```console
   $ az acr repository show-manifests --name myregistry --repository acr-helloworld
   [
     {
       "digest": "sha256:7ca0e0ae50c95155dbb0e380f37d7471e98d2232ed9e31eece9f9fb9078f2728",
       "tags": [
         "latest"
       ],
       "timestamp": "2018-07-11T21:38:35.9170967Z"
     },
     {
       "digest": "sha256:d2bdc0c22d78cde155f53b4092111d7e13fe28ebf87a945f94b19c248000ceec",
       "tags": [],
       "timestamp": "2018-07-11T21:32:21.1400513Z"
     }
   ]
   ```

Dizisi son adımda Çıkışta gördüğünüz gibi yoktur yalnız bırakılmış bir bildirim şimdi ayarlanmış `"tags"` özelliği boş bir listedir. Bu bildirimi hala başvurduğu herhangi bir benzersiz katmanı verileriyle birlikte kayıt defteri içinde yok. **Örneğin silmek için görüntüler ve katman verilerine yalnız bırakılmış tarafından bildirim Özet silmelisiniz**.

## <a name="delete-all-untagged-images"></a>Etiketlenmemiş tüm görüntüleri silin

Deponuzda aşağıdaki Azure CLI komutunu kullanarak tüm etiketlenmemiş görüntüleri listeleyebilirsiniz. Değiştirin `<acrName>` ve `<repositoryName>` ortamınız için uygun değerlerle.

```azurecli
az acr repository show-manifests --name <acrName> --repository <repositoryName> --query "[?tags[0]==null].digest"
```

Bu komutu bir betikte kullanarak bir depodaki tüm etiketlenmemiş görüntülerini silebilirsiniz.

> [!WARNING]
> Aşağıdaki örnek betikler silinmiş--dikkatli görüntü verilerdir KURTARILAMAZ. Bildirim özeti (aksine, görüntü adı) tarafından görüntüleri çekmek sistemleri varsa, bu betikleri çalıştırmamanız gerekir. Etiketlenmemiş görüntüleri siliniyor, bu sistemlerin görüntülerini kayıt defterinizden çekme öğesinden engeller. Bildirimi tarafından çekmek yerine benimsemeyi göz önünde bir *benzersiz etiketleme* düzeni, bir [önerilen en iyi][tagging-best-practices].

**Bash Azure CLI**

Aşağıdaki Bash betiği, bir depodan tüm etiketlenmemiş görüntüleri siler. Azure CLI'yı gerektirir ve **xargs**. Varsayılan olarak, betik, hiçbir silme gerçekleştirir. Değişiklik `ENABLE_DELETE` değerini `true` görüntü silme işlemini etkinleştirmek için.

```bash
#!/bin/bash

# WARNING! This script deletes data!
# Run only if you do not have systems
# that pull images via manifest digest.

# Change to 'true' to enable image delete
ENABLE_DELETE=false

# Modify for your environment
REGISTRY=myregistry
REPOSITORY=myrepository

# Delete all untagged (orphaned) images
if [ "$ENABLE_DELETE" = true ]
then
    az acr repository show-manifests --name $REGISTRY --repository $REPOSITORY  --query "[?tags[0]==null].digest" -o tsv \
    | xargs -I% az acr repository delete --name $REGISTRY --image $REPOSITORY@% --yes
else
    else
    echo "No data deleted."
    echo "Set ENABLE_DELETE=true to enable image deletion of these images in $REPOSITORY:"
    az acr repository show-manifests --name $REGISTRY --repository $REPOSITORY --query "[?tags[0]==null]" -o tsv
fi
```

**PowerShell'de Azure CLI**

Aşağıdaki PowerShell betiğini bir depodan tüm etiketlenmemiş görüntüleri siler. PowerShell ve Azure CLI'yı gerektirir. Varsayılan olarak, betik, hiçbir silme gerçekleştirir. Değişiklik `$enableDelete` değerini `$TRUE` görüntü silme işlemini etkinleştirmek için.

```powershell
# WARNING! This script deletes data!
# Run only if you do not have systems
# that pull images via manifest digest.

# Change to '$TRUE' to enable image delete
$enableDelete = $FALSE

# Modify for your environment
$registry = "myregistry"
$repository = "myrepository"

if ($enableDelete) {
    az acr repository show-manifests --name $registry --repository $repository --query "[?tags[0]==null].digest" -o tsv `
    | %{ az acr repository delete --name $registry --image $repository@$_ --yes }
} else {
    Write-Host "No data deleted."
    Write-Host "Set `$enableDelete = `$TRUE to enable image deletion."
    az acr repository show-manifests --name $registry --repository $repository --query "[?tags[0]==null]" -o tsv
}
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Container Registry'de resim depolama hakkında daha fazla bilgi için bkz. [kapsayıcı görüntüsü Azure Container Registry depolamada](container-registry-storage.md).

<!-- IMAGES -->
[manifest-digest]: ./media/container-registry-delete/01-manifest-digest.png

<!-- LINKS - External -->
[docker-manifest-inspect]: https://docs.docker.com/edge/engine/reference/commandline/manifest/#manifest-inspect
[portal]: https://portal.azure.com
[tagging-best-practices]: https://stevelasker.blog/2018/03/01/docker-tagging-best-practices-for-tagging-and-versioning-docker-images/

<!-- LINKS - Internal -->
[az-acr-repository-delete]: /cli/azure/acr/repository#az-acr-repository-delete
[az-acr-repository-show-manifests]: /cli/azure/acr/repository#az-acr-repository-show-manifests
[az-acr-repository-untag]: /cli/azure/acr/repository#az-acr-repository-untag
