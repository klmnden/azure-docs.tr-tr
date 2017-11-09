---
title: Azure Storage ile Azure CLI 2.0 kullanma | Microsoft Docs
description: "Azure komut satırı arabirimi (Azure CLI) 2.0 oluşturmak ve depolama hesaplarını yönetme ve çalışmak için Azure Storage ile Azure BLOB'ları ve dosyalar ile kullanmayı öğrenin. Azure CLI 2.0 Python içinde yazılmış platformlar arası bir araçtır."
services: storage
documentationcenter: na
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 06/02/2017
ms.author: tamram
ms.openlocfilehash: 4f4070c5a02e559bd299033865aa5258532498aa
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2017
---
# <a name="using-the-azure-cli-20-with-azure-storage"></a>Azure Storage ile Azure CLI 2.0 kullanma

Açık kaynak, platformlar arası Azure CLI 2.0 ile Azure platformu çalışmaya yönelik bir komut kümesi sağlar. Bulunan aynı işlevlerinin çoğunu sağlayan [Azure portal](https://portal.azure.com), zengin veri erişimi de dahil olmak üzere.

Bu kılavuzda, nasıl kullanılacağını gösteriyoruz [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) Azure depolama hesabınızdaki kaynaklara çalışma çeşitli görevleri gerçekleştirmek için. İndirin ve yükleyin veya bu kılavuzu kullanmadan önce CLI 2.0 son sürüme yükseltme öneririz.

Kılavuzdaki örnekler Ubuntu üzerinde Bash kabuğunda kullanımını varsayar ancak diğer platformlar benzer şekilde gerçekleştirmeniz gerekir. 

[!INCLUDE [storage-cli-versions](../../../includes/storage-cli-versions.md)]

## <a name="prerequisites"></a>Ön koşullar
Bu kılavuz, Azure Storage temel kavramlarını anladığınızı varsayar. Aşağıda Azure ve depolama hizmeti için belirtilen hesap oluşturma gerekliliklerini karşılayabildiğiniz de varsayılmaktadır.

### <a name="accounts"></a>Hesaplar
* **Azure hesabı**: zaten bir Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı oluşturma](https://azure.microsoft.com/free/).
* **Storage hesabı**: Bkz. [Azure Storage hesapları hakkında](storage-create-storage-account.md) sayfası, [Storage hesabı oluşturma](storage-create-storage-account.md#create-a-storage-account) bölümü.

### <a name="install-the-azure-cli-20"></a>Azure CLI 2.0 sürümünü yükleme

Karşıdan yükleyip Azure CLI 2.0 özetlenen yönergeleri izleyerek [Azure CLI 2.0 yükleme](/cli/azure/install-az-cli2).

> [!TIP]
> Yükleme konusunda sorun yaşıyorsanız, kullanıma [yükleme sorunlarını giderme](/cli/azure/install-az-cli2#installation-troubleshooting) makalenin bölümüne ve [yükleme sorun giderme](https://github.com/Azure/azure-cli/blob/master/doc/install_troubleshooting.md) github'da Kılavuzu.
>

## <a name="working-with-the-cli"></a>CLI ile çalışma

CLI yükledikten sonra kullanabileceğiniz `az` , komut satırı arabirimi (Bash, Terminal, komut istemi) Azure CLI komutlara erişmek için komutu. Tür `az` komutu (Aşağıdaki örnek çıkış kesildi) temel komutları tam listesini görmek için:

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

Komutu, komut satırı arabirimi yürütün `az storage --help` listesine `storage` alt grupları komutu. Alt gruplar açıklamalarını depolama kaynaklarla çalışmak için Azure CLI sağlar işlevlerine genel bakış sağlar.

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

## <a name="connect-the-cli-to-your-azure-subscription"></a>CLI Azure aboneliğinize bağlanma

Azure aboneliğinizde kaynaklarla çalışmak için öncelikle Azure hesabınızla oturum açmanız gerekir `az login`. Oturum açabilir birkaç yolu vardır:

* **Etkileşimli oturum açma**:`az login`
* **Kullanıcı adı ve parolayla oturum**:`az login -u johndoe@contoso.com -p VerySecret`
  * Bu, Microsoft hesapları veya çok faktörlü kimlik doğrulamasını kullanan ile çalışmaz.
* **Bir hizmet sorumlusu oturum oturum**:`az login --service-principal -u http://azure-cli-2016-08-05-14-31-15 -p VerySecret --tenant contoso.onmicrosoft.com`

## <a name="azure-cli-20-sample-script"></a>Azure CLI 2.0 örnek komut dosyası

Ardından, size Azure Storage kaynakları ile etkileşim kurmak için birkaç temel Azure CLI 2.0 komutları sorunları küçük Kabuk betiği çalışmak. Komut dosyası ilk depolama hesabınızı yeni bir kapsayıcı oluşturur ve sonra bu kapsayıcıya (blob) olarak varolan bir dosyayı yükler. Kapsayıcıdaki tüm blob'lara listeler ve son olarak, yerel bilgisayarınızda belirttiğiniz bir hedefe dosyasını indirir.

```bash
#!/bin/bash
# A simple Azure Storage example script

export AZURE_STORAGE_ACCOUNT=<storage_account_name>
export AZURE_STORAGE_ACCESS_KEY=<storage_account_key>

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

**Yapılandırma ve komut dosyası çalıştırma**

1. Sık kullandığınız metin düzenleyicisinde açın, sonra kopyalayın ve önceki komut düzenleyiciye yapıştırın.

2. Ardından, komut dosyanızın değişken yapılandırma ayarlarınızı yansıtacak şekilde güncelleştirin. Aşağıdaki değerleri belirtilen şekilde değiştirin:

   * **\<storage_account_name\>**  depolama hesabınızın adını.
   * **\<storage_account_key\>**  depolama hesabınız için birincil veya ikincil erişim anahtarı.
   * **\<container_name\>**  A, "azure CLI-örnek-kapsayıcı gibi" oluşturmak için yeni kapsayıcı adı.
   * **\<blob_name\>**  hedef blob kapsayıcısında için bir ad.
   * **\<file_to_upload\>**  dosyasının yolu küçük, yerel bilgisayarınızda gibi "~ / images/HelloWorld.png".
   * **\<destination_file\>**  hedef dosya yolu, gibi "~ / downloadedImage.png".

3. Gerekli değişkenleri güncelleştirdikten sonra komut dosyasını kaydedin ve düzenleyicinizi çıkın. Sonraki adımlar kodunuzu adlı varsayın **my_storage_sample.sh**.

4. Komut dosyası yürütülebilir, olarak gerekiyorsa işaretleyin:`chmod +x my_storage_sample.sh`

5. Betiğini yürütün. Örneğin, Bash içinde:`./my_storage_sample.sh`

Aşağıdakine benzer bir çıktı görmeniz gerekir ve  **\<destination_file\>**  , belirtilen komut dosyası yerel bilgisayarınızda görünmelidir.

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
> Önceki çıktı olarak **tablo** biçimi. Hangi belirterek kullanılacak biçim çıktı belirtebilirsiniz `--output` CLI komutlarınızı değişkeninde veya genel kullanarak ayarlayın `az configure`.
>

## <a name="manage-storage-accounts"></a>Depolama hesaplarını yönetme

### <a name="create-a-new-storage-account"></a>Yeni depolama hesabı oluşturma
Azure Depolama kullanmak için bir depolama hesabınız olması gerekir. Bilgisayarınıza yapılandırdıktan sonra yeni bir Azure depolama hesabı oluşturabilirsiniz [aboneliğinize bağlanma](#connect-to-your-azure-subscription).

```azurecli
az storage account create \
    --location <location> \
    --name <account_name> \
    --resource-group <resource_group> \
    --sku <account_sku>
```

* `--location`[Gerekli]: konum. Örneğin, "Batı ABD".
* `--name`[Gerekli]: depolama hesabı adı. Adı 3-24 karakter uzunluğunda olmalıdır ve yalnızca küçük harf alfasayısal karakterler kullanın.
* `--resource-group`[Gerekli]: kaynak grubunun adı.
* `--sku`[Gerekli]: depolama hesabı SKU. İzin verilen değerler:
  * `Premium_LRS`
  * `Standard_GRS`
  * `Standard_LRS`
  * `Standard_RAGRS`
  * `Standard_ZRS`

### <a name="set-default-azure-storage-account-environment-variables"></a>Varsayılan Azure depolama hesabı ortam değişkenlerini ayarlama
Azure aboneliğinizde birden çok depolama hesabı olabilir. Tüm sonraki depolama komutlarını kullanmak için bunlardan birini seçmek için bu ortam değişkenleri ayarlayabilirsiniz:

```azurecli
export AZURE_STORAGE_ACCOUNT=<account_name>
export AZURE_STORAGE_ACCESS_KEY=<key>
```

Varsayılan depolama hesabı ayarlamak için başka bir yolu, bir bağlantı dizesi kullanmaktır. İlk olarak, bağlantı dizesi alma `show-connection-string` komutu:

```azurecli
az storage account show-connection-string \
    --name <account_name> \
    --resource-group <resource_group>
```

Ardından çıktı bağlantı dizesini kopyalayın ve ayarlama `AZURE_STORAGE_CONNECTION_STRING` ortam değişkeni (bağlantı dizesi tırnak içine aldığınızdan gerekebilir):

```azurecli
export AZURE_STORAGE_CONNECTION_STRING="<connection_string>"
```

> [!NOTE]
> Aşağıdaki bölümlerde tüm örneklerde, bu makalenin ayarladığınızdan emin varsayın `AZURE_STORAGE_ACCOUNT` ve `AZURE_STORAGE_ACCESS_KEY` ortam değişkenleri.
>

## <a name="create-and-manage-blobs"></a>Oluşturma ve BLOB'ları yönetme
Azure Blob Depolama, büyük miktarlarda herhangi bir yere HTTP veya HTTPS aracılığıyla erişilebilen metin veya ikili veriler gibi yapılandırılmamış verileri depolamak için bir hizmettir. Bu bölümde, zaten Azure Blob Depolama kavramlarına alışık olduğunuz varsayılır. Ayrıntılı bilgi için bkz: [.NET kullanarak Azure Blob storage'ı kullanmaya başlama](../blobs/storage-dotnet-how-to-use-blobs.md) ve [Blob hizmeti kavramları](/rest/api/storageservices/blob-service-concepts).

### <a name="create-a-container"></a>Bir kapsayıcı oluşturma
Azure depolama her blob bir kapsayıcıda olmalıdır. Kullanarak bir kapsayıcı oluşturabilirsiniz `az storage container create` komutu:

```azurecli
az storage container create --name <container_name>
```

İsteğe bağlı belirterek üç düzeyinden birini okuma erişimi için yeni bir kapsayıcı ayarlayabilirsiniz `--public-access` bağımsız değişkeni:

* `off`(varsayılan): kapsayıcı verileri hesap sahibinin özel.
* `blob`: BLOB'lar ortak okuma erişimi.
* `container`: Ortak okuma ve liste erişimi kapsayıcının tamamı.

Daha fazla bilgi için bkz: [kapsayıcılar ve bloblar için anonim okuma erişimini yönetme](../blobs/storage-manage-access-to-resources.md).

### <a name="upload-a-blob-to-a-container"></a>Bir kapsayıcıya blob yükleme
Azure Blob storage blok destekler, ekleme ve sayfa BLOB'ları. BLOB'ları kullanarak bir kapsayıcıya karşıya `blob upload` komutu:

```azurecli
az storage blob upload \
    --file <local_file_path> \
    --container-name <container_name> \
    --name <blob_name>
```

 Varsayılan olarak, `blob upload` komutu, sayfa bloblarını veya blok blobları aksi *.vhd dosyaları yükler. Bir blob karşıya yüklediğinizde başka bir tür belirtmek için kullanabileceğiniz `--type` izin verilen değerler bağımsız değişken-- `append`, `block`, ve `page`.

 Farklı blob türleri hakkında daha fazla bilgi için bkz: [anlama blok Blobları, ekleme Blobları ve sayfa Bloblarını](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs).


### <a name="download-a-blob-from-a-container"></a>Kapsayıcıdan blob indirme
Bu örnek, bir kapsayıcıdan blob indirmek gösterilmiştir:

```azurecli
az storage blob download \
    --container-name mycontainer \
    --name myblob.png \
    --file ~/mydownloadedblob.png
```

### <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme

BLOB'ları bir kapsayıcıda ile listesinde [az depolama blob listesi](/cli/azure/storage/blob#list) komutu.

```azurecli
az storage blob list \
    --container-name mycontainer \
    --output table
```

### <a name="copy-blobs"></a>BLOB kopyalama
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

Yukarıdaki örnekte, hedef kapsayıcıyı kopyalama işleminin başarılı olması hedef depolama hesabındaki varolmalıdır. Ayrıca, `--source-uri` bağımsız değişkeninde belirtilen kaynak blobun bir paylaşılan erişim imzası (SAS) içermesi veya bu örnekte olduğu gibi genel olarak erişilebilir olması gerekir.

### <a name="delete-a-blob"></a>Blob silme
Bir blobu silmek için kullanın `blob delete` komutu:

```azurecli
az storage blob delete --container-name <container_name> --name <blob_name>
```

## <a name="create-and-manage-file-shares"></a>Oluşturun ve dosya paylaşımlarını yönetmek
Azure dosyaları sunucu ileti bloğu (SMB) protokolünü kullanan uygulamalar için paylaşılan depolama alanı sunar. Microsoft Azure sanal makineler ve bulut Hizmetleri yanı sıra, şirket içi uygulamalara bağlı paylaşımlar üzerinden dosya verileri paylaşabilir. Dosya paylaşımları ve dosya verilerini Azure CLI aracılığıyla yönetebilirsiniz. Azure dosyaları hakkında daha fazla bilgi için bkz: [Azure dosyaları giriş](../files/storage-files-introduction.md).

### <a name="create-a-file-share"></a>Dosya paylaşımı oluşturma
Bir Azure dosya paylaşımı azure'da SMB dosya paylaşımı değil. Tüm dizin ve dosyaların bir dosya paylaşımı oluşturulmalıdır. Bir hesapta sınırsız sayıda paylaşım olabilir ve bir paylaşım dosyaları, depolama hesabının kapasite sınırlarına kadar sınırsız sayıda depolayabilirsiniz. Aşağıdaki örnek adlı bir dosya paylaşımı oluşturur **paylaşımım**.

```azurecli
az storage share create --name myshare
```

### <a name="create-a-directory"></a>Dizin oluşturma
Bir dizin Azure dosya paylaşımının hiyerarşik yapıda sağlar. Aşağıdaki örnek adlı bir dizin oluşturur **Dizinim** dosya paylaşımında.

```azurecli
az storage directory create --name myDir --share-name myshare
```

Bir dizin yolu birden çok düzeyi, örneğin içerebilir **dizin1/dir2**. Ancak, tüm üst dizinleri bir alt dizin oluşturmadan önce mevcut olduğundan emin olmalısınız. Örneğin, yol **dizin1/dir2**, dizin oluşturmak **dizin1**, dizin oluşturma **dir2**.

### <a name="upload-a-local-file-to-a-share"></a>Yerel bir dosya paylaşımı için karşıya yükleme
Aşağıdaki örnekte bir dosyadan yükler **~/temp/samplefile.txt** kökünün **paylaşımım** dosya paylaşımı. `--source` Bağımsız değişkeni karşıya yüklemek için var olan yerel dosyayı belirtir.

```azurecli
az storage file upload --share-name myshare --source ~/temp/samplefile.txt
```

Olarak dizin oluşturma ile bir dizin yolu dosya paylaşımı içinde varolan bir dizin karşıya paylaşımındaki belirtebilirsiniz:

```azurecli
az storage file upload --share-name myshare/myDir --source ~/temp/samplefile.txt
```

Bir dosya paylaşımında boyutu 1 TB'ye kadar olabilir.

### <a name="list-the-files-in-a-share"></a>Bir paylaşım dosyaları listesi
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
Bir dosyayı başka bir dosyaya, bir dosya bir blob veya bir blobu bir dosyaya kopyalayabilirsiniz. Örneğin, bir dosyayı farklı bir paylaşım dizininde kopyalamak için şunu yazın:        
        
```azurecli
az storage file copy start \
--source-share share1 --source-path dir1/file.txt \
--destination-share share2 --destination-path dir2/file.txt     
```

## <a name="create-share-snapshot"></a>Paylaşım anlık görüntü oluşturma
Kullanarak bir paylaşım anlık görüntü oluşturabilirsiniz `az storage share snapshot` komutu:

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

### <a name="list-share-snapshots"></a>Liste paylaşımı anlık görüntüler

Belirli paylaşımı kullanarak bir paylaşım anlık görüntüleri listeleyebilir`az storage share list --include-snapshots`

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

### <a name="browse-share-snapshots"></a>Paylaşım anlık görüntüleri Gözat
İçerik kullanarak görüntülemek için anlık görüntü belirli bir paylaşım içine göz atabilirsiniz `az storage file list`. Paylaşım adını belirtmek bir tane var `--share-name <snare name>` ve zaman damgası`--snapshot '2017-10-04T19:45:18.0000000Z'`

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
### <a name="restore-from-share-snapshots"></a>Paylaşım anlık görüntülerden geri yükleme

Kopyalayarak veya dosya kullanarak anlık görüntü bir paylaşımdan indirme bir dosyayı geri `az storage file download` komutu

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
## <a name="delete-share-snapshot"></a>Paylaşım anlık görüntüyü Sil
Paylaşım anlık görüntü kullanarak silebilirsiniz `az storage share delete` sağlayarak komutu `--snapshot` paylaşımı anlık görüntü zaman damgasına sahip parametre:

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
Azure CLI 2.0 ile çalışma hakkında daha fazla bilgi edinmek için bazı ek kaynaklar aşağıda verilmiştir.

* [Azure CLI 2.0 ile çalışmaya başlama](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2)
* [Azure CLI 2.0 komut başvurusu](/cli/azure)
* [Github'daki Azure CLI 2.0](https://github.com/Azure/azure-cli)
