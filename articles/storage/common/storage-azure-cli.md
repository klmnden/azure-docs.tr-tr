---
title: Azure CLI, Azure depolama ile kullanma | Microsoft Docs
description: Azure komut satırı arabirimi (Azure CLI) oluşturma ve depolama hesaplarını yönetme ve Azure BLOB'ları ve dosyalarla çalışmak için Azure depolama ile öğreneceksiniz.
services: storage
author: roygara
ms.service: storage
ms.devlang: azurecli
ms.topic: article
ms.date: 06/02/2017
ms.author: rogarana
ms.subservice: common
ms.openlocfilehash: f485f38d4c580937b027bb76d0c34c98f699ed93
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61483573"
---
# <a name="using-the-azure-cli-with-azure-storage"></a>Azure Storage ile Azure CLI kullanma

Açık kaynaklı, platformlar arası Azure CLI, Azure platformu ile çalışmaya yönelik komut kümesini sağlar. Bulunan aynı işlevselliğinin sağlar [Azure portalında](https://portal.azure.com), zengin verilere erişim de dahil olmak üzere.

Bu kılavuzda, nasıl kullanılacağını göstereceğiz [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) Azure depolama hesabınızdaki kaynaklarla çalışma çeşitli görevleri gerçekleştirmek için. İndirin ve yükleyin veya bu kılavuzu kullanmadan önce CLI'ın en son sürüme yükseltme öneririz.

Bu kılavuzdaki örnekler üzerinde Ubuntu'da Bash kabuğunu kullanımını varsayılır, ancak diğer platformlarda benzer şekilde gerçekleştirmeniz gerekir. 

[!INCLUDE [storage-cli-versions](../../../includes/storage-cli-versions.md)]

## <a name="prerequisites"></a>Önkoşullar
Bu kılavuzda, Azure Depolama'nın temel kavramlarını anladığınızı varsayar. Aşağıda Azure ve depolama hizmeti için belirtilen hesap oluşturma gerekliliklerini karşılayabildiğiniz de varsayılmaktadır.

### <a name="accounts"></a>Hesaplar
* **Azure hesabı**: Azure aboneliğiniz yoksa, [ücretsiz bir Azure hesabı oluşturun](https://azure.microsoft.com/free/).
* **Depolama hesabı**: Bkz: [depolama hesabı oluşturma](storage-quickstart-create-account.md) içinde [Azure depolama hesapları hakkında](storage-create-storage-account.md).

### <a name="install-the-azure-cli"></a>Azure CLI'yı yükleme

Bölümünde açıklanan yönergeleri takip ederek Azure CLI'yı yükleme ve indirme [Azure CLI'yı yükleme](/cli/azure/install-az-cli2).

> [!TIP]
> Yüklemeyle ilgili bir sorun varsa, kullanıma [yükleme sorunlarını giderme](/cli/azure/install-az-cli2) makalenin bölümüne ve [yükleme sorunlarını giderme](https://github.com/Azure/azure-cli/blob/master/doc/install_troubleshooting.md) github'da Kılavuzu.
>

## <a name="working-with-the-cli"></a>CLI ile çalışma

CLI'yı yükledikten sonra kullanabileceğiniz `az` komutunu, komut satırı arabirimi (Bash, Terminal, komut istemi) Azure CLI komutlarına erişmek için. Tür `az` komutu (Aşağıdaki örnek çıktı kesildi) temel komutlar tam listesini görmek için:

```
     /\
    /  \    _____   _ _ __ ___
   / /\ \  |_  / | | | \'__/ _ \
  / ____ \  / /| |_| | | |  __/
 /_/    \_\/___|\__,_|_|  \___|


Welcome to the cool new Azure CLI!

Here are the base commands:

    account          : Manage subscriptions.
    acr              : Manage Azure container registries.
    acs              : Manage Azure Container Services.
    ad               : Synchronize on-premises directories and manage Azure Active Directory
                       resources.
    ...
```

Komutu, komut satırı arabiriminde yürütün `az storage --help` listesine `storage` alt grupları komutu. Alt gruplar açıklamalarını depolama kaynaklarınızla çalışmak için Azure CLI'yı sağladığı işlevsellik genel bir bakış sağlar.

```
Group
    az storage: Durable, highly available, and massively scalable cloud storage.

Subgroups:
    account  : Manage storage accounts.
    blob     : Object storage for unstructured data.
    container: Manage blob storage containers.
    cors     : Manage Storage service Cross-Origin Resource Sharing (CORS).
    directory: Manage file storage directories.
    entity   : Manage table storage entities.
    file     : File shares that use the standard SMB 3.0 protocol.
    logging  : Manage Storage service logging information.
    message  : Manage queue storage messages.
    metrics  : Manage Storage service metrics.
    queue    : Use queues to effectively scale applications according to traffic.
    share    : Manage file shares.
    table    : NoSQL key-value storage using semi-structured datasets.
```

## <a name="connect-the-cli-to-your-azure-subscription"></a>CLI'yı Azure aboneliğinize bağlama

Azure aboneliğinizdeki kaynaklarla çalışmak için öncelikle Azure hesabınızla oturum açmanız gerekir `az login`. Oturum açın, birkaç yolu vardır:

* **Etkileşimli oturum açma**: `az login`
* **Kullanıcı adı ve parolayla oturum**: `az login -u johndoe@contoso.com -p VerySecret`
  * Bu, Microsoft hesapları veya çok faktörlü kimlik doğrulaması kullanan hesapları ile çalışmaz.
* **Hizmet sorumlusu ile oturum**: `az login --service-principal -u http://azure-cli-2016-08-05-14-31-15 -p VerySecret --tenant contoso.onmicrosoft.com`

## <a name="azure-cli-sample-script"></a>Azure CLI örnek betiği

Ardından, size Azure Storage kaynakları ile etkileşim kurmak için birkaç temel Azure CLI komutlarını veren küçük bir kabuk betiği ile çalışırsınız. Betik ilk depolama hesabınızda yeni bir kapsayıcı oluşturur, ardından (blob) olarak mevcut bir dosyayı kapsayıcıya yükler. Ardından tüm blobları kapsayıcıda listeler ve son olarak, yerel bilgisayarınızda, belirttiğiniz bir hedef dosya indirir.

```bash
#!/bin/bash
# A simple Azure Storage example script

export AZURE_STORAGE_ACCOUNT=<storage_account_name>
export AZURE_STORAGE_KEY=<storage_account_key>

export container_name=<container_name>
export blob_name=<blob_name>
export file_to_upload=<file_to_upload>
export destination_file=<destination_file>

echo "Creating the container..."
az storage container create --name $container_name

echo "Uploading the file..."
az storage blob upload --container-name $container_name --file $file_to_upload --name $blob_name

echo "Listing the blobs..."
az storage blob list --container-name $container_name --output table

echo "Downloading the file..."
az storage blob download --container-name $container_name --name $blob_name --file $destination_file --output table

echo "Done"
```

**Yapılandırma ve betiği çalıştırın**

1. Sık kullandığınız metin düzenleyicisinde açın, sonra kopyalayın ve yukarıdaki betik düzenleyiciye yapıştırın.

2. Ardından, betiğin değişkenleri yapılandırma ayarlarınızı yansıtacak şekilde güncelleştirin. Aşağıdaki değerleri belirtilen şekilde değiştirin:

   * **\<storage_account_name\>**  depolama hesabınızın adı.
   * **\<storage_account_key\>**  depolama hesabınız için birincil veya ikincil erişim anahtarı.
   * **\<container_name\>**  bir ad, "azure CLI-örnek-kapsayıcı gibi" oluşturmak için yeni bir kapsayıcı.
   * **\<blob_name\>**  kapsayıcıda hedef blob için bir ad.
   * **\<file_to_upload\>**  dosyasının yerel bilgisayarınızda gibi küçük yolu "~ / images/HelloWorld.png".
   * **\<destination_file\>**  hedef dosya yolu, aşağıdaki gibi "~ / downloadedImage.png".

3. Gerekli değişkenleri güncelleştirdikten sonra komut dosyasını kaydedin ve düzenleyicinizi çıkın. Sonraki adımlarda betiğinizi adlı varsayılır **my_storage_sample.sh**.

4. Gerekirse, betiği şu yürütülebilir, şekilde işaretle: `chmod +x my_storage_sample.sh`

5. Bu betiği yürütün. Örneğin, Bash hizmetinde: `./my_storage_sample.sh`

Aşağıdakine benzer bir çıktı görmeniz gerekir ve **\<destination_file\>** belirttiğiniz komut dosyası yerel bilgisayarınızda görünmelidir.

```
Creating the container...
{
  "created": true
}
Uploading the file...
Percent complete: %100.0
Listing the blobs...
Name       Blob Type      Length  Content Type              Last Modified
---------  -----------  --------  ------------------------  -------------------------
README.md  BlockBlob        6700  application/octet-stream  2017-05-12T20:54:59+00:00
Downloading the file...
Name
---------
README.md
Done
```

> [!TIP]
> Önceki çıktısı **tablo** biçimi. Hangi belirterek kullanılacak biçim çıktı belirtebilirsiniz `--output` bağımsız değişkeni, CLI komutlarında veya genel olarak kullanarak ayarlayın `az configure`.
>

## <a name="manage-storage-accounts"></a>Depolama hesaplarını yönetme

### <a name="create-a-new-storage-account"></a>Yeni depolama hesabı oluşturma
Azure Depolama kullanmak için bir depolama hesabınız olması gerekir. Aboneliğinize bağlanmak için bilgisayarınızı yeniden yapılandırdıktan sonra yeni bir Azure depolama hesabı oluşturabilirsiniz.

```azurecli
az storage account create \
    --location <location> \
    --name <account_name> \
    --resource-group <resource_group> \
    --sku <account_sku>
```

* `--location` [Gerekli]: Konum. Örneğin, "Batı ABD".
* `--name` [Gerekli]: Depolama hesabı adı. Ad 3 ile 24 karakter uzunluğunda olmalı ve yalnızca küçük harf alfasayısal karakterler kullanın.
* `--resource-group` [Gerekli]: Kaynak grubunun adı.
* `--sku` [Gerekli]: Depolama hesabı SKU. İzin verilen değerler:
  * `Premium_LRS`
  * `Standard_GRS`
  * `Standard_LRS`
  * `Standard_RAGRS`
  * `Standard_ZRS`

### <a name="set-default-azure-storage-account-environment-variables"></a>Varsayılan Azure depolama hesabı ortam değişkenlerini ayarlama

Azure aboneliğinizde birden fazla depolama hesabı olabilir. Tüm sonraki depolama komutlarını kullanmak için bunlardan birini seçmek için bu ortam değişkenleri ayarlayabilirsiniz:

Öncelikle [az storage account keys list](/cli/azure/storage/account/keys) komutunu kullanarak depolama hesabı anahtarlarınızı görüntüleyin:

```azurecli-interactive
az storage account keys list \
    --account-name <account_name> \
    --resource-group <resource_group> \
    --output table
```

Anahtar olduğuna göre ortam değişkenleri olarak ve hesap adı tanımlayabilirsiniz:

```azurecli
export AZURE_STORAGE_ACCOUNT=<account_name>
export AZURE_STORAGE_KEY=<key>
```

Başka bir yolu varsayılan depolama hesabı bağlantı dizesi kullanmaktır. İlk olarak, bağlantı dizesiyle alın `show-connection-string` komutu:

```azurecli
az storage account show-connection-string \
    --name <account_name> \
    --resource-group <resource_group>
```

Ardından, çıkış bağlantı dizesini kopyalayın ve ayarlama `AZURE_STORAGE_CONNECTION_STRING` ortam değişkeni (bağlantı dizesini tırnak içine alın gerekebilir):

```azurecli
export AZURE_STORAGE_CONNECTION_STRING="<connection_string>"
```

> [!NOTE]
> Aşağıdaki bölümlerde, bu makalenin tüm örnekler, ayarlamış olduğunuz varsayılır `AZURE_STORAGE_ACCOUNT` ve `AZURE_STORAGE_KEY` ortam değişkenleri.

## <a name="create-and-manage-blobs"></a>Oluşturma ve BLOB'ları yönetme
Azure Blob Depolama, büyük miktarlarda herhangi bir HTTP veya HTTPS aracılığıyla dünyanın erişilebilen metin veya ikili veriler gibi yapılandırılmamış verileri depolamak için kullanılan bir hizmettir. Bu bölümde, zaten ile Azure Blob Depolama kavramları hakkında bilgi sahibi olduğunuz varsayılır. Ayrıntılı bilgi için bkz. [.NET kullanarak Azure Blob depolamayı kullanmaya başlama](../blobs/storage-dotnet-how-to-use-blobs.md) ve [Blob hizmeti kavramları](/rest/api/storageservices/blob-service-concepts).

### <a name="create-a-container"></a>Bir kapsayıcı oluşturma
Her blob Azure depolamadaki bir kapsayıcıda olmalıdır. Kullanarak kapsayıcı oluşturabilirsiniz `az storage container create` komutu:

```azurecli
az storage container create --name <container_name>
```

İsteğe bağlı olarak belirterek üç düzeyinden birini okuma erişimi için yeni bir kapsayıcı ayarlayabilirsiniz `--public-access` bağımsız değişkeni:

* `off` (varsayılan): Kapsayıcı verilerini hesap sahibine özeldir.
* `blob`: Blobları için genel okuma erişimi.
* `container`: Ortak okuma ve liste tüm kapsayıcıya erişimi.

Daha fazla bilgi için bkz. [Kapsayıcılara ve bloblara anonim okuma erişimini yönetme](../blobs/storage-manage-access-to-resources.md).

### <a name="upload-a-blob-to-a-container"></a>Bir kapsayıcıya blob yükleme
Sayfa blobları ve ekleme veya Azure Blob Depolama blok destekler. Kullanarak bir kapsayıcıya BLOB karşıya `blob upload` komutu:

```azurecli
az storage blob upload \
    --file <local_file_path> \
    --container-name <container_name> \
    --name <blob_name>
```

Doğrudan depolama hesabınızdaki kapsayıcı içinde bir klasöre yüklemek isterseniz değiştirme `--name <blob_name>` ile `--name <folder/blob_name>`.

 Varsayılan olarak, `blob upload` komut sayfa BLOB'ları veya blok blobları aksi *.vhd dosyaları karşıya yükler. Bir blob karşıya yüklediğinizde, başka bir tür belirtmek için kullanabileceğiniz `--type` izin verilen değerler değişken-- `append`, `block`, ve `page`.

 Farklı bir blob türleri hakkında daha fazla bilgi için bkz. [anlama blok Blobları, ekleme Blobları ve sayfa Blobları](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs).


### <a name="download-a-blob-from-a-container"></a>Kapsayıcıdan blob indirme
Bu örnek, bir kapsayıcıdan blob indirme gösterilmektedir:

```azurecli
az storage blob download \
    --container-name mycontainer \
    --name myblob.png \
    --file ~/mydownloadedblob.png
```

### <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme

İle bir kapsayıcıdaki blobları listelemek [az storage blob listesi](/cli/azure/storage/blob) komutu.

```azurecli
az storage blob list \
    --container-name mycontainer \
    --output table
```

### <a name="copy-blobs"></a>Blobları kopyalama
Blobları, depolama hesaplarıyla bölgeler içinde veya bunların arasında zaman uyumsuz olarak kopyalayabilirsiniz.

Aşağıdaki örnekte blobların bir depolama hesabından diğerine nasıl kopyalandığı gösterilmektedir. Önce kaynak depolama hesabında bir kapsayıcı oluşturacağız ve blobları için genel okuma erişimi belirteceğiz. Ardından, kapsayıcıya bir dosya yükleyip son olarak da bu kapsayıcıdan blobu hedef depolama hesabındaki bir bloba kopyalayacağız.

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
    --file ~/Pictures/sourcefile.png \
    --name sourcefile.png

# Copy blob from source account to destination account (destcontainer must exist)
az storage blob copy start \
    --account-name destaccountname \
    --account-key destaccountkey \
    --destination-blob destfile.png \
    --destination-container destcontainer \
    --source-uri https://sourceaccountname.blob.core.windows.net/sourcecontainer/sourcefile.png
```

Yukarıdaki örnekte, hedef kapsayıcı kopyalama işleminin başarılı olması hedef depolama hesabındaki mevcut olmalıdır. Ayrıca, `--source-uri` bağımsız değişkeninde belirtilen kaynak blobun bir paylaşılan erişim imzası (SAS) içermesi veya bu örnekte olduğu gibi genel olarak erişilebilir olması gerekir.

### <a name="delete-a-blob"></a>Blob silme
Bir blobu silmek için kullanın `blob delete` komutu:

```azurecli
az storage blob delete --container-name <container_name> --name <blob_name>
```

## <a name="create-and-manage-file-shares"></a>Oluşturma ve dosya paylaşımlarını yönetme
Azure dosyaları için sunucu ileti bloğu (SMB) protokolünü kullanarak uygulamalara paylaşılan depolama alanı sunar. Şirket içi uygulamaların yanı sıra Microsoft Azure sanal makineler ve bulut Hizmetleri, bağlanan paylaşımlar yoluyla dosya verileri paylaşabilir. Dosya paylaşımları ve dosya verilerini Azure CLI aracılığıyla yönetebilirsiniz. Azure dosyaları hakkında daha fazla bilgi için bkz. [Azure dosyaları'na giriş](../files/storage-files-introduction.md).

### <a name="create-a-file-share"></a>Dosya paylaşımı oluşturma
Bir Azure dosya paylaşımı azure'daki bir SMB dosyası paylaşımıdır. Tüm dizin ve dosyaların bir dosya paylaşımı oluşturulması gerekir. Bir hesapta sınırsız sayıda paylaşım olabilir ve dosyaları, depolama hesabının kapasite sınırları sınırsız sayıda paylaşım depolayabilir. Aşağıdaki örnekte adlı bir dosya paylaşımı oluşturur **myshare**.

```azurecli
az storage share create --name myshare
```

### <a name="create-a-directory"></a>Dizin oluşturma
Azure dosya paylaşımını hiyerarşik bir yapıda bir dizin sağlar. Aşağıdaki örnekte adlı bir dizin oluşturur **Dizinim** Dosya paylaşımındaki.

```azurecli
az storage directory create --name myDir --share-name myshare
```

Bir dizin yolu birden çok düzeyi, örneğin içerebilir **dizin1/dir2**. Ancak, bir alt dizini oluşturmadan önce tüm üst dizinlerin var emin olmanız gerekir. Örneğin, yol **dizin1/dir2**, dizin oluşturmak **dizin1**, dizin oluşturup **dir2**.

### <a name="upload-a-local-file-to-a-share"></a>Bir paylaşıma yerel bir dosya karşıya yükleme
Aşağıdaki örnek, bir dosyadan yükler **~/temp/samplefile.txt** kökünün **myshare** dosya paylaşımı. `--source` Karşıya yüklemek için var olan yerel dosya bağımsız değişkeni belirtir.

```azurecli
az storage file upload --share-name myshare --source ~/temp/samplefile.txt
```

Olarak dizin oluşturma ile paylaşımdaki mevcut bir dizine dosya yüklemek için paylaşımı içinde bir dizin yolu belirtebilirsiniz:

```azurecli
az storage file upload --share-name myshare/myDir --source ~/temp/samplefile.txt
```

Paylaşımdaki bir dosya boyutu 1 TB'ye kadar olabilir.

### <a name="list-the-files-in-a-share"></a>Bir paylaşımdaki dosyaları Listele
Kullanarak dosyaları ve dizinleri bir paylaşımda listeleyebilirsiniz `az storage file list` komutu:

```azurecli
# List the files in the root of a share
az storage file list --share-name myshare --output table

# List the files in a directory within a share
az storage file list --share-name myshare/myDir --output table

# List the files in a path within a share
az storage file list --share-name myshare --path myDir/mySubDir/MySubDir2 --output table
```

### <a name="copy-files"></a>Dosyaları kopyalama      
Bir dosya için başka bir dosyaya, bir dosyayı bir bloba veya bir blobu bir dosyaya kopyalayabilirsiniz. Örneğin, bir dosyayı farklı bir paylaşımda bir dizin kopyalama için şunu yazın:        
        
```azurecli
az storage file copy start \
--source-share share1 --source-path dir1/file.txt \
--destination-share share2 --destination-path dir2/file.txt     
```

## <a name="create-share-snapshot"></a>Paylaşım anlık görüntüsü oluşturma
Kullanarak bir paylaşım anlık görüntüsü oluşturabilirsiniz `az storage share snapshot` komutu:

```cli
az storage share snapshot -n <share name>
```

Örnek çıktı
```json
{
  "metadata": {},
  "name": "<share name>",
  "properties": {
    "etag": "\"0x8D50B7F9A8D7F30\"",
    "lastModified": "2017-10-04T23:28:22+00:00",
    "quota": null
  },
  "snapshot": "2017-10-04T23:28:35.0000000Z"
}
```

### <a name="list-share-snapshots"></a>Paylaşım anlık görüntülerini listeleme

Belirli bir paylaşımı kullanarak bir paylaşım anlık görüntülerini listeleyebilir. `az storage share list --include-snapshots`

```cli
az storage share list --include-snapshots
```

**Örnek çıktı**
```json
[
  {
    "metadata": null,
    "name": "sharesnapshotdefs",
    "properties": {
      "etag": "\"0x8D50B5F4005C975\"",
      "lastModified": "2017-10-04T19:36:46+00:00",
      "quota": 5120
    },
    "snapshot": "2017-10-04T19:44:13.0000000Z"
  },
  {
    "metadata": null,
    "name": "sharesnapshotdefs",
    "properties": {
      "etag": "\"0x8D50B5F4005C975\"",
      "lastModified": "2017-10-04T19:36:46+00:00",
      "quota": 5120
    },
    "snapshot": "2017-10-04T19:45:18.0000000Z"
  },
  {
    "metadata": null,
    "name": "sharesnapshotdefs",
    "properties": {
      "etag": "\"0x8D50B5F4005C975\"",
      "lastModified": "2017-10-04T19:36:46+00:00",
      "quota": 5120
    },
    "snapshot": null
  }
]
```

### <a name="browse-share-snapshots"></a>Paylaşım anlık görüntülerine göz atma
Belirli bir paylaşımına içerik kullanarak görüntülemek için anlık görüntü de göz atabilirsiniz `az storage file list`. Paylaşım adı belirtmek bir tane var `--share-name <snare name>` ve zaman damgası `--snapshot '2017-10-04T19:45:18.0000000Z'`

```azurecli-interactive
az storage file list --share-name sharesnapshotdefs --snapshot '2017-10-04T19:45:18.0000000Z' -otable
```

**Örnek çıktı**
```
Name            Content Length    Type    Last Modified
--------------  ----------------  ------  ---------------
HelloWorldDir/                    dir
IMG_0966.JPG    533568            file
IMG_1105.JPG    717711            file
IMG_1341.JPG    608459            file
IMG_1405.JPG    652156            file
IMG_1611.JPG    442671            file
IMG_1634.JPG    1495999           file
IMG_1635.JPG    974058            file

```
### <a name="restore-from-share-snapshots"></a>Paylaşım anlık görüntülerindeki geri yükleme

Kopyalama veya anlık görüntü kullanarak bir paylaşımdan dosya indirme dosyayı geri yükleyebilirsiniz `az storage file download` komutu

```azurecli-interactive
az storage file download --path IMG_0966.JPG --share-name sharesnapshotdefs --snapshot '2017-10-04T19:45:18.0000000Z'
```
**Örnek çıktı**
```
{
  "content": null,
  "metadata": {},
  "name": "IMG_0966.JPG",
  "properties": {
    "contentLength": 533568,
    "contentRange": "bytes 0-533567/533568",
    "contentSettings": {
      "cacheControl": null,
      "contentDisposition": null,
      "contentEncoding": null,
      "contentLanguage": null,
      "contentType": "application/octet-stream"
    },
    "copy": {
      "completionTime": null,
      "id": null,
      "progress": null,
      "source": null,
      "status": null,
      "statusDescription": null
    },
    "etag": "\"0x8D50B5F49F7ACDF\"",
    "lastModified": "2017-10-04T19:37:03+00:00",
    "serverEncrypted": true
  }
}
```
## <a name="delete-share-snapshot"></a>Paylaşım anlık görüntüsünü silme
Kullanarak bir paylaşım anlık görüntüsünü silebilirsiniz `az storage share delete` komutunu girerek `--snapshot` paylaşım anlık görüntü zaman damgasına sahip parametre:

```cli
az storage share delete -n <share name> --snapshot '2017-10-04T23:28:35.0000000Z' 
```

Örnek çıktı
```json
{
  "deleted": true
}
```

## <a name="next-steps"></a>Sonraki adımlar
Azure CLI ile çalışma hakkında daha fazla bilgi edinmek için bazı ek kaynaklar aşağıda verilmiştir. 

* [Azure CLI ile çalışmaya başlama](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2)
* [Azure CLI komut başvurusu](/cli/azure)
* [Github'da Azure CLI](https://github.com/Azure/azure-cli)
