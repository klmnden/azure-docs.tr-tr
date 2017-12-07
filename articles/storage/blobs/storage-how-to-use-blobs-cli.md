---
title: "Azure CLI ile Azure Blob depolama (nesne depolama) üzerinde işlemler gerçekleştirme | Microsoft Docs"
description: "Azure Blob depolamada blobları karşıya yükleme ve indirmenin yanı sıra depolama hesabınızdaki bir bloba erişimi yönetmek için bir paylaşılan imza erişimi (SAS) oluşturmayı öğrenin."
services: storage
documentationcenter: na
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/15/2017
ms.author: tamram
ms.openlocfilehash: 0c3fc2d73a0caf0e0331cb9073bfcc0574240dac
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2017
---
# <a name="perform-blob-storage-operations-with-azure-cli"></a>Azure CLI ile Blob depolama işlemleri gerçekleştirme

Azure Blob Storage; HTTP veya HTTPS aracılığıyla dünyanın her yerinde erişilebilen metin veya ikili veriler gibi büyük miktarda yapılandırılmamış nesne verilerinin depolanması için bir hizmettir. Bu öğretici, Azure Blob’da blobları karşıya yükleme, indirme ve silme gibi temel işlemleri kapsar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Bir kapsayıcı oluşturma
> * Kapsayıcıya dosya (blob) yükleme
> * Blob’ları bir kapsayıcıda listeleme
> * Kapsayıcıdan blob indirme
> * Bir blobu depolama hesapları arasında kopyalama
> * Blob silme
> * Blob özelliklerini ve meta verilerini görüntüleme ve değiştirme
> * Paylaşılan erişim imzası (SAS) ile güvenliği yönetme

Bu öğretici, Azure CLI 2.0.4 veya üzeri bir sürümü gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [storage-quickstart-tutorial-intro-include-cli](../../../includes/storage-quickstart-tutorial-intro-include-cli.md)]

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Kapsayıcılar bilgisayarınızdaki dizinlere benzer ve kapsayıcıdaki blob gruplarını, dizindeki dosyaları düzenlediğiniz gibi düzenlemenize olanak sağlar. Bir depolama hesabında herhangi bir sayıda kapsayıcı olabilir. Bir kapsayıcıda, bir depolama hesabındaki en yüksek veri miktarı olan 500 TB blob verisi depolayabilirsiniz.

[az storage container create](/cli/azure/storage/container#create) komutunu kullanarak blobları depolamak için bir kapsayıcı oluşturun.

```azurecli-interactive
az storage container create --name mystoragecontainer
```

Kapsayıcı adları bir harf veya rakamla başlamalıdır ve yalnızca harf, rakam ve kısa çizgi (-) karakterini içerebilir. Kapsayıcıları ve blobları adlandırma kuralları hakkında daha fazla bilgi için bkz. [Kapsayıcıları, blobları ve meta verileri adlandırma ve bunlara başvurma](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

## <a name="enable-public-read-access-for-a-container"></a>Bir kapsayıcı için genel okuma erişimini etkinleştirme

Yeni oluşturulan bir kapsayıcı varsayılan olarak özeldir. Diğer bir deyişle, [paylaşılan erişim imzası](#create-a-shared-access-signature-sas) veya depolama hesabının erişim anahtarları olmadan hiç kimse kapsayıcıya erişemez. Azure CLI’yı kullanarak, kapsayıcı izinlerini aşağıdaki üç düzeyden birinde ayarlayarak bu davranışı değiştirebilirsiniz:

| | |
|---|---|
| `--public-access off` | (Varsayılan) Genel okuma erişimi yok |
| `--public-access blob` | Yalnızca bloblara genel okuma erişimi |
| `--public-access container` | Bloblara genel okuma erişimi ve kapsayıcıdaki blobları listeleme izni |

Genel erişimi `blob` veya `container` olarak ayarladığınızda, salt okunur erişimi İnternet’teki herkes için etkinleştirirsiniz. Örneğin, web sitenizde bloblar olarak depolanan görüntüleri görüntülemek istiyorsanız, genel okuma erişimini etkinleştirmeniz gerekir. Okuma/yazma erişimini etkinleştirmek istiyorsanız, bunu yerine bir [paylaşılan erişim imzası (SAS)](#create-a-shared-access-signature-sas) kullanmanız gerekir.

[az storage container set-permission](/cli/azure/storage/container#create) komutu ile kapsayıcınız için genel okuma erişimini etkinleştirin.

```azurecli-interactive
az storage container set-permission \
    --name mystoragecontainer \
    --public-access container
```

## <a name="upload-a-blob-to-a-container"></a>Bir kapsayıcıya blob yükleme

Blob depolama blok blobları, ekleme bloblarını ve sayfa bloblarını destekler. Blok blobları Azure Depolama içinde depolanan en yaygın blob türüdür. Ekleme blobları, verilerin mevcut içeriği değiştirmeden mevcut bir bloba eklenmesi gerektiğinde (örneğin günlüğe kaydetme için) kullanılır. Sayfa blobları IaaS sanal makinelerinin VHD dosyalarını yedekler.

Bu örnekte, son adımda [az storage blob upload](/cli/azure/storage/blob#upload) komutuyla oluşturduğumuz kapsayıcıya bir blob yükleyeceğiz.

```azurecli-interactive
az storage blob upload \
    --container-name mystoragecontainer \
    --name blobName \
    --file ~/path/to/local/file
```

Bu işlemle, daha önce oluşturulmadıysa bir blob oluşturulur, aksi takdirde üzerine yazılacaktır. Sonraki adımda blobları listelediğinizde, birden çok giriş görmek için birkaç dosyayı karşıya yükleyin.

## <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme

[az storage blob list](/cli/azure/storage/blob#list) komutuyla kapsayıcıdaki blobları listeleyin.

```azurecli-interactive
az storage blob list \
    --container-name mystoragecontainer \
    --output table
```

Örnek çıktı:

```
Name            Blob Type      Length  Content Type              Last Modified
--------------  -----------  --------  ------------------------  -------------------------
ISSUE_TEMPLATE  BlockBlob         761  application/octet-stream  2017-04-21T18:29:50+00:00
LICENSE         BlockBlob       18653  application/octet-stream  2017-04-21T18:28:50+00:00
LICENSE-CODE    BlockBlob        1090  application/octet-stream  2017-04-21T18:29:02+00:00
README.md       BlockBlob        6700  application/octet-stream  2017-04-21T18:27:33+00:00
dir1/file1.txt  BlockBlob        6700  application/octet-stream  2017-04-21T18:32:51+00:00
```

## <a name="download-a-blob"></a>Blob indirme

Önceki bir adımda [az storage blob download](/cli/azure/storage/blob#download) komutunu kullanarak karşıya yüklediğiniz blobu indirin.

```azurecli-interactive
az storage blob download \
    --container-name mystoragecontainer \
    --name blobName \
    --file ~/destination/path/for/file
```

## <a name="copy-a-blob-between-storage-accounts"></a>Bir blobu depolama hesapları arasında kopyalama

Blobları, depolama hesaplarıyla bölgeler içinde veya bunların arasında zaman uyumsuz olarak kopyalayabilirsiniz.

Aşağıdaki örnekte blobların bir depolama hesabından diğerine nasıl kopyalandığı gösterilmektedir. Önce kaynak depolama hesabında bir kapsayıcı oluşturacağız ve blobları için genel okuma erişimi belirteceğiz. Ardından, kapsayıcıya bir dosya yükleyip son olarak da bu kapsayıcıdan blobu hedef depolama hesabındaki bir bloba kopyalayacağız.

Bu örnekte, kopyalama işleminin başarılı olması için hedef kapsayıcının hedef depolama hesabında mevcut olması gerekir. Ayrıca, `--source-uri` bağımsız değişkeninde belirtilen kaynak blobun bir paylaşılan erişim imzası (SAS) içermesi veya bu örnekte olduğu gibi genel olarak erişilebilir olması gerekir.

```azurecli
# Create container in source account
az storage container create \
    --account-name sourceaccountname \
    --account-key sourceaccountkey \
    --name sourcecontainer \
    --public-access blob

# Upload blob to container in source account
az storage blob upload \
    --account-name sourceaccountname \
    --account-key sourceaccountkey \
    --container-name sourcecontainer \
    --file ~/path/to/sourcefile \
    --name sourcefile

# Copy blob from source account to destination account (destcontainer must exist)
az storage blob copy start \
    --account-name destaccountname \
    --account-key destaccountkey \
    --destination-blob destfile.png \
    --destination-container destcontainer \
    --source-uri https://sourceaccountname.blob.core.windows.net/sourcecontainer/sourcefile.png
```

## <a name="delete-a-blob"></a>Blob silme

[az storage blob delete](/cli/azure/storage/blob#delete) komutunu kullanarak blobu kapsayıcıdan silin.

```azurecli-interactive
az storage blob delete \
    --container-name mystoragecontainer \
    --name blobName
```

## <a name="set-the-content-type"></a>İçerik türünü ayarlama

Aynı zamanda MIME türü olarak da bilinen içerik türü, blob verilerinin biçimini belirler. Tarayıcılar ve diğer yazılımlar, verilerin nasıl işleneceğini belirlemek için içerik türünü kullanır. Aşağıdaki örnekte içerik türü `image/png` olarak ayarlanır.

```azurecli-interactive
# Set the content type
az storage blob update
    --container-name mystoragecontainer 
    --name blobName 
    --content-type image/png
```

## <a name="display-and-modify-blob-properties-and-metadata"></a>Blob özelliklerini ve meta verilerini görüntüleme ve değiştirme

Her blobda, [az storage blob show](/cli/azure/storage/blob#show) komutuyla görüntüleyebileceğiniz hizmet tanımlı çeşitli özellikler (adı, türü, uzunluğu gibi) bulunur. Ayrıca [az storage blob metadata update](/cli/azure/storage/blob/metadata#update) komutunu kullanarak kendi özellikleriniz ve bunların değerleriyle bir blob yapılandırabilirsiniz.

Bu örnekte, önce bir blobun hizmet tarafından tanımlanan özelliklerini görüntüleyip ardından kendi meta veri özelliklerimizden iki tanesiyle güncelleştireceğiz. Son olarak, [az storage blob metadata show](/cli/azure/storage/blob/metadata#show) komutuyla blobun meta veri özelliklerini ve bunların değerlerini görüntüleyeceğiz.

```azurecli-interactive
# Show properties of a blob
az storage blob show \
    --container-name mystoragecontainer \
    --name blobName \
    --output table

# Set metadata properties of a blob (replaces all existing metadata)
az storage blob metadata update \
    --container-name mystoragecontainer \
    --name blobName \
    --metadata "key1=value1" "key2=value2"

# Show metadata properties of a blob
az storage blob metadata show \
    --container-name mystoragecontainer \
    --name blobName
```

## <a name="create-a-shared-access-signature-sas"></a>Paylaşılan erişim imzası (SAS) oluşturma

Paylaşılan erişim imzaları (SAS), depolama hesabı erişim anahtarlarınızı açığa çıkarmadan depolama hesabınızdaki nesnelere sınırlı erişim izni vermenin bir yolunu sunar. Özel bir kaynağa erişim izni veren bir URI oluşturmak için, istenen izinlere ve geçerlilik penceresine (etkili olacağı süre) sahip bir SAS belirteci oluşturarak bir kaynağın URL’sine sorgu dizesi olarak ekler ve tam SAS URI’sini oluşturursunuz. Bu SAS URI’sine (kaynağın URL’si ve SAS belirteci) sahip olan herkes SAS belirtecinin geçerlilik penceresi boyunca öğeye erişebilir. Örneğin, birinin görüntüleyebilmesi için bir özel bloba iki dakika boyunca okuma erişimi vermek isteyebilirsiniz.

Aşağıdaki adımlarda, kapsayıcınıza genel erişimi devre dışı bırakıp yalnızca özel erişimi test eder ve bir SAS URI’si oluşturup test edersiniz.

### <a name="disable-container-public-access"></a>Kapsayıcı genel erişimini devre dışı bırakma

Öncelikle kapsayıcının erişim düzeyini `off` olarak ayarlayın. Bu, kapsayıcıya veya bloblarına bir paylaşılan erişim imzası veya depolama hesabı erişim anahtarı olmadan erişilemeyeceğini belirtir.

```azurecli-interactive
az storage container set-permission \
    --name mystoragecontainer \
    --public-access off
```

### <a name="verify-private-access"></a>Özel erişimi doğrulama

Kapsayıcıdaki bloblara genel erişim olmadığını doğrulamak için, [az storage blob url](/cli/azure/storage/blob#url) komutuyla bloblarından birinin URL’sini alın.

```azurecli-interactive
az storage blob url \
    --container-name mystoragecontainer \
    --name blobName \
    --output tsv
```

Özel bir tarayıcı penceresinde blobun URL’sine gidin. Blob özel olduğu bir paylaşılan erişim anahtarı eklemediğiniz için bir `ResourceNotFound` hatası alırsınız.

### <a name="create-a-sas-uri"></a>SAS URI’si oluşturma

Şimdi bloba erişim izni veren bir SAS URI’si oluşturacağız. Aşağıdaki örnekte, önce bir değişkeni [az storage blob url](/cli/azure/storage/blob#url) ile blob için URL’yle doldurur ve daha sonra başka bir değişkeni [az storage blob generate-sas](/cli/azure/storage/blob#generate-sas) komutuyla oluşturulmuş bir SAS belirteciyle doldururuz. Son olarak, ikisini birleştirerek blob için `?` sorgu dizesi ayırıcıyla ayrılmış tam SAS URI’si çıkışını alacağız.

```azurecli-interactive
# Get UTC datetimes for SAS start and expiry (Example: 1994-11-05T13:15:30Z)
sas_start=`date -u +'%Y-%m-%dT%H:%M:%SZ'`
sas_expiry=`date -u +'%Y-%m-%dT%H:%M:%SZ' -d '+2 minute'`

# Get the URL for the blob
blob_url=`az storage blob url \
    --container-name mystoragecontainer \
    --name blobName \
    --output tsv`

# Obtain a SAS token granting read (r) access between the
# SAS start and expiry times
sas_token=`az storage blob generate-sas \
    --container-name mystoragecontainer \
    --name blobName \
    --start $sas_start \
    --expiry $sas_expiry \
    --permissions r \
    --output tsv`

# Display the full SAS URI for the blob
echo $blob_url?$sas_token
```

### <a name="test-the-sas-uri"></a>SAS URI’sini test etme

Özel bir tarayıcı penceresinde, önceki kod parçacığında `echo` komutuyla görüntülediğiniz tam SAS URI’sine gidin. Bu kez bir hata almazsınız ve blobu görüntüleyebilir veya indirebilirsiniz.

URL’nin süresinin dolması için bekleyin (bu örnekte iki dakika) ve ardından başka bir özel tarayıcı penceresinde URI’ye gidin. Şimdi SAS belirtecinin süresi dolduğu için bir `AuthenticationFailed` görürsünüz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Oluşturduğunuz depolama hesabı ve bu öğreticide karşıya yüklediğiniz bloblar dahil, kaynak grubunuzdaki hiçbir kaynağa artık ihtiyacınız yoksa [az group delete](/cli/azure/group#delete) komutuyla kaynak grubunu silin.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide Azure Depolama’da bloblar ile çalışma hakkındaki temel bilgileri edindiniz:

> [!div class="checklist"]
> * Bir kapsayıcı oluşturma
> * Kapsayıcıya dosya (blob) yükleme
> * Blob’ları bir kapsayıcıda listeleme
> * Kapsayıcıdan blob indirme
> * Bir blobu depolama hesapları arasında kopyalama
> * Blob silme
> * Blob özelliklerini ve meta verilerini görüntüleme ve değiştirme
> * Paylaşılan erişim imzası (SAS) ile güvenliği yönetme

Aşağıdaki kaynaklar Azure CLI ile çalışma ve depolama hesabınızdaki kaynaklarla çalışma hakkında ek bilgileri sağlar.

* Azure CLI
  * [Azure CLI 2.0 ile oturum açma](/cli/azure/authenticate-azure-cli) - Katılımsız Azure CLI betiklerini yürütmek için [hizmet sorumlusu](/cli/azure/authenticate-azure-cli#logging-in-with-a-service-principal) aracılığıyla kullanıcı etkileşimi gerektirmeyen oturum açma dahil olmak üzere CLI ile farklı kimlik doğrulama yöntemleri hakkında bilgi edinin.
  * [Azure CLI 2.0 komut başvurusu](/cli/azure/)
* Microsoft Azure Depolama Gezgini
  * [Microsoft Azure Depolama Gezgini](../../vs-azure-tools-storage-manage-with-storage-explorer.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json), Microsoft tarafından sunulan ve Windows, macOS ve Linux üzerinde Azure Depolama verileriyle görsel olarak çalışmanıza olanak sağlayan ücretsiz bir tek başına uygulamadır.
