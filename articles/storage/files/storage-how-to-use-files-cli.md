---
title: Azure CLI kullanarak Azure dosya paylaşımlarını yönetme
description: Azure CLI kullanarak Azure Dosyaları'nı yönetmeyi öğrenin.
services: storage
documentationcenter: na
author: wmgries
manager: jeconnoc
editor: ''
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/26/2018
ms.author: wgries
ms.openlocfilehash: 066a43b553be18a5a0bc889fff441824df301b98
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
---
# <a name="managing-azure-file-shares-using-the-azure-cli"></a>Azure CLI kullanarak Azure dosya paylaşımlarını yönetme
[Azure Dosyaları](storage-files-introduction.md), Windows'un kolay kullanılan bulut dosya sistemidir. Azure dosya paylaşımları, Windows, Linux ve macOS platformlarına bağlanabilir. Bu kılavuzda, Azure CLI'yi kullanarak Azure dosya paylaşımlarında çalışmanın temelleri gösterilir. Şunları nasıl yapacağınızı öğrenin: 

> [!div class="checklist"]
> * Kaynak grubu ve depolama hesabı oluşturma
> * Azure dosya paylaşımı oluşturma 
> * Dizin oluşturma
> * Dosyayı karşıya yükleme 
> * Dosya indirme
> * Paylaşım anlık görüntüsü oluşturma ve paylaşma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmaya karar verirseniz bu kılavuz için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için **az --version** komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). 

Varsayılan olarak Azure CLI komutları, REST API'lerden ileti gönderip almanın asıl yolu olan JSON (JavaScript Nesne Gösterimi) döndürür. JSON yanıtlarıyla çalışmayı kolaylaştırmak için, bu kılavuzdaki bazı örneklerde Azure CLI komutlarında sorgu parametresi kullanılır. Bu parametre [JMESPath sorgu dilini](http://jmespath.org/) kullanarak JSON'u ayrıştırır. Azure CLI komutlarının sonuçlarını işleme hakkında daha fazla bilgi edinmek için [JMESPath Öğreticisi](http://jmespath.org/tutorial.html)'ni izleyebilirsiniz.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Henüz bir Azure kaynak grubunuz yoksa, [az group create](/cli/azure/group#create) komutuyla yeni bir grup oluşturabilirsiniz. 

Aşağıdaki örnek *EastUS* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma
Depolama hesabı, Azure dosya paylaşımını veya bloblar veya sorgular gibi diğer depolama kaynaklarını dağıtabileceğiniz, paylaşılan bir depolama havuzudur. Bir depolama hesabında sınırsız sayıda dosya paylaşımı olabilir; paylaşım da, depolama hesabının kapasite sınırları içinde sınırsız sayıda dosya depolayabilir.

Bu örnekte, [az storage account create](/cli/azure/storage/account#create) komutu kullanılarak `mystorageaccount<random number>` adlı bir depolama hesabı oluşturulur ve bu depolama hesabının adını `$STORAGEACCT` değişkenine yerleştirilir. Depolama hesabı adları benzersiz olmalıdır; `$RANDOM` kullanılarak depolama hesabı adının sonuna bir sayı eklenir ve ad benzersiz hale getirilir. 

```azurecli-interactive 
STORAGEACCT=$(az storage account create \
    --resource-group "myResourceGroup" \
    --name "mystorageacct$RANDOM" \
    --location eastus \
    --sku Standard_LRS \
    --query "name" | tr -d '"')
```

### <a name="get-the-storage-account-key"></a>Depolama hesabı anahtarını alma
Depolama hesabı anahtarları, depolama hesabındaki kaynaklara erişimi denetlemek için kullanılır. Bunlar, depolama hesabını oluşturduğunuzda otomatik olarak oluşturulur. [az storage account keys list](/cli/azure/storage/account/keys#list) komutunu kullanarak depolama hesabının depolama hesabı anahtarlarını alabilirsiniz. 

```azurecli-interactive 
STORAGEKEY=$(az storage account keys list \
    --resource-group "myResourceGroup" \
    --account-name $STORAGEACCT \
    --query "[0].value" | tr -d '"')
```

## <a name="create-an-azure-file-share"></a>Azure dosya paylaşımı oluşturma
Artık ilk Azure dosya paylaşımınızı oluşturabilirsiniz. Dosya paylaşımlarını oluşturmak için [az storage share create](/cli/azure/storage/share#create) komutunu kullanabilirsiniz. Bu örnekte `myshare` adlı bir Azure dosya paylaşımı oluşturulur. 

```azurecli-interactive
az storage share create \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare" 
```

> [!Important]  
> Paylaşım adları tümüyle küçük harf, sayı ve tek kısa çizgilerden oluşmalı ve kısa çizgiyle başlamamalıdır. Dosya paylaşımlarının ve dosyaların adlandırılması hakkında tüm ayrıntılara ulaşmak için bkz. [Paylaşımları, Dizinleri, Dosyaları ve Meta Verileri Adlandırma ve Bunlara Başvuruda Bulunma](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Shares--Directories--Files--and-Metadata)

## <a name="manipulating-the-contents-of-the-azure-file-share"></a>Azure dosya paylaşımının içindekileri işleme
Artık Azure dosya paylaşımını oluşturduğunuza göre, [Windows](storage-how-to-use-files-windows.md), [Linux](storage-how-to-use-files-linux.md) veya [macOS](storage-how-to-use-files-mac.md)'ta SMB ile dosya paylaşımını bağlayabilirsiniz. Alternatif olarak, Azure dosya paylaşımınızı Azure CLI'yle de işleyebilirsiniz. Bu yöntem dosya paylaşımını SMB ile bağlamaktan daha avantajlıdır çünkü Azure CLI ile yapılan tüm istekler Dosya REST API ile yapıldığından, dosya paylaşımınızdaki dosyaları ve dizinleri şuralardan oluşturabilir, değiştirebilir ve silebilirsiniz:

- Bash Cloud Shell (SMB üzerinden dosya paylaşımlarını bağlayamaz).
- Engeli kaldırılmış 445 numaralı bağlantı noktası olmayan şirket içi istemcileri gibi, SMB paylaşımlarını bağlayamayan istemciler.
- [Azure İşlevleri](../../azure-functions/functions-overview.md) gibi sunucusuz senaryolar. 

### <a name="create-directory"></a>Dizin oluşturma
Azure dosya paylaşımınızın kökünde *myDirectory* adlı yeni bir dizin oluşturmak için [`az storage directory create`](/cli/azure/storage/directory#az_storage_directory_create) komutunu kullanın.

```azurecli-interactive
az storage directory create \
   --account-name $STORAGEACCT \
   --account-key $STORAGEKEY \
   --share-name "myshare" \
   --name "myDirectory" 
```

### <a name="upload-a-file"></a>Dosyayı karşıya yükleme
[`az storage file upload`](/cli/azure/storage/file#az_storage_file_upload) komutunu kullanarak dosyanın nasıl karşıya yükleneceğini göstermek için, önce karşıya yüklemek üzere Azure CLI Cloud Shell'inizin karalama sürücüsünün içinde bir dosya oluşturmalısınız. Aşağıdaki örnekte dosyayı oluşturuyor ve ardından karşıya yüklüyoruz.

```azurecli-interactive
date > ~/clouddrive/SampleUpload.txt

az storage file upload \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --source "~/clouddrive/SampleUpload.txt" \
    --path "myDirectory/SampleUpload.txt"
```

Azure CLI'yi yerel olarak çalıştırıyorsanız, `~/clouddrive` bölümünün yerine makinenizde var olan bir yol kullanmalısınız.

Dosyayı karşıya yükledikten sonra, [`az storage file list`](/cli/azure/storage/file#az_storage_file_list) komutunu kullanarak dosyanın Azure dosya paylaşımınıza yüklendiğinden emin olabilirsiniz.

```azurecli-interactive
az storage file list \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --path "myDirectory" \
    --output table
```

### <a name="download-a-file"></a>Dosya indirme
Cloud Shell'inizin karalama sürücüsüne yüklediğiniz dosyanın bir kopyasını indirmek için [`az storage file download`](/cli/azure/storage/file#az_storage_file_download) komutunu kullanabilirsiniz.

```azurecli-interactive
# Delete an existing file by the same name as SampleDownload.txt, if it exists because you've run this example before
rm -rf ~/clouddrive/SampleDownload.txt

az storage file download \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --path "myDirectory/SampleUpload.txt" \
    --dest "~/clouddrive/SampleDownload.txt"
```

### <a name="copy-files"></a>Dosyaları kopyalama
Yaygın görevlerden biri dosyaları bir dosya paylaşımından diğerine veya Azure Blob depolama kapsayıcısına/kapsayıcısından kopyalamaktır. Bu işlevi göstermek için, yeni bir paylaşım oluşturabilir ve bu yeni paylaşıma yüklediğiniz dosyayı [az storage file copy](/cli/azure/storage/file/copy) komutunu kullanarak kopyalayabilirsiniz. 

```azurecli-interactive
az storage share create \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare2"

az storage directory create \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare2" \
    --name "myDirectory2"

az storage file copy start \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --source-share "myshare" \
    --source-path "myDirectory/SampleUpload.txt" \
    --destination-share "myshare2" \
    --destination-path "myDirectory2/SampleCopy.txt"
```

Şimdi, yeni paylaşımdaki dosyaları listelerseniz kopyalanan dosyanızı görmeniz gerekir.

```azurecli-interactive
az storage file list \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare2" \
    --output table
```

`az storage file copy start` komutu Azure dosya paylaşımları ve Azure Blob depolama kapsayıcıları arasında plansız dosya taşıma işlemlerinde kullanışlı olsa da, daha büyük taşıma işlemleri (taşınan dosyaların sayısı ve boyutu açısından) için AzCopy kullanılmasını öneririz. [Linux için AzCopy](../common/storage-use-azcopy-linux.md) ve [Windows için AzCopy](../common/storage-use-azcopy.md) hakkında daha fazla bilgi edinin. AzCopy yerel olarak yüklenmelidir; Cloud Shell'de kullanılamaz 

## <a name="create-and-modify-share-snapshots"></a>Paylaşım anlık görüntülerini oluşturma ve değiştirme
Azure dosya paylaşımıyla yerine getirebileceğiniz kullanışlı bir diğer görev de paylaşım anlık görüntüleri oluşturmaktır. Anlık görüntü, Azure dosya paylaşımının zamanın bir noktasındaki durumunu saklar. Paylaşım anlık görüntüleri, aşağıdakiler gibi zaten tanıyor olabileceğiniz işletim sistemi teknolojilerine benzer:
- Linux sistemleri için [Mantıksal Birim Yöneticisi (LVM)](https://en.wikipedia.org/wiki/Logical_Volume_Manager_(Linux)#Basic_functionality) anlık görüntüleri
- macOS için [Apple Dosya Sistemi (APFS)](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/APFS_Guide/Features/Features.html) anlık görüntüleri. 
- NTFS ve ReFS gibi Windows dosya sistemleri için [Birim Gölge Kopyası Hizmeti (VSS)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee923636)

[`az storage share snapshot`](/cli/azure/storage/share#az_storage_share_snapshot) komutunu kullanarak bir paylaşım anlık görüntüsü oluşturabilirsiniz.

```azurecli-interactive
SNAPSHOT=$(az storage share snapshot \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare" \
    --query "snapshot" | tr -d '"')
```

### <a name="browse-share-snapshot-contents"></a>Paylaşım anlık görüntüsünün içeriğine göz atma
`$SNAPSHOT` değişkeninde yakaladığımız paylaşım anlık görüntüsünün zaman damgasını `az storage file list` komutuna geçirerek, paylaşım anlık görüntüsünün içeriğine göz atabilirsiniz.

```azurecli-interactive
az storage file list \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --snapshot $SNAPSHOT \
    --output table
```

### <a name="list-share-snapshots"></a>Paylaşım anlık görüntülerini listeleme
Paylaşımınız için aldığınız anlık görüntülerin listesini aşağıdaki komutla görebilirsiniz.

```azurecli-interactive
az storage share list \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --include-snapshot \
    --query "[? name=='myshare' && snapshot!=null]" | tr -d '"'
```

### <a name="restore-from-a-share-snapshot"></a>Paylaşım anlık görüntüsünden geri yükleme
Daha önce kullandığımız `az storage file copy start` komutu kullanarak dosyayı geri yükleyebilirsiniz. İlk olarak, karşıya yüklediğimiz `SampleUpload.txt` dosyamızı sileceğiz. Böylelikle bu dosyayı anlık görüntüden geri yükleyebiliriz.

```azurecli-interactive
# Delete SampleUpload.txt
az storage file delete \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --path "myDirectory/SampleUpload.txt"

# Build the source URI for snapshot restore
URI=$(az storage account show \
    --resource-group "myResourceGroup" \
    --name $STORAGEACCT \
    --query "primaryEndpoints.file" | tr -d '"')

URI=$URI"myshare/myDirectory/SampleUpload.txt?sharesnapshot="$SNAPSHOT

# Restore SampleUpload.txt from the share snapshot
az storage file copy start \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --source-uri $URI \
    --destination-share "myshare" \
    --destination-path "myDirectory/SampleUpload.txt"
```

### <a name="delete-a-share-snapshot"></a>Paylaşım anlık görüntüsünü silme
[`az storage share delete`](/cli/azure/storage/share#az_storage_share_delete) komutunu `--snapshot` parametresine `$SNAPSHOT` başvurusu içeren bir değişkenle kullanarak paylaşım anlık görüntüsünü silebilirsiniz.

```azurecli-interactive
az storage share delete \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare" \
    --snapshot $SNAPSHOT
```

## <a name="clean-up-resources"></a>Kaynakları temizleme
İşiniz bittiğinde, [`az group delete`](/cli/azure/group#delete) komutunu kullanarak kaynak grubunu ve tüm ilgili kaynakları kaldırabilirsiniz. 

```azurecli-interactive 
az group delete --name "myResourceGroup"
```

Alternatif olarak kaynakları tek tek de kaldırabilirsiniz:
- Bu hızlı başlangıç için oluşturduğumuz Azure dosya paylaşımlarını kaldırmak için.

```azurecli-interactive
az storage share delete \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare" \
    --delete-snapshots include

az storage share delete \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare2" \
    --delete-snapshots include
```

- Depolama hesabının kendisini kaldırmak için (bu işlem örtülü olarak hem oluşturduğumuz Azure dosya paylaşımlarını hem de oluşturmuş olabileceğiniz Azure Blob depolama kapsayıcısı gibi diğer depolama kaynaklarını kaldırır).

```azurecli-interactive
az storage account delete \
    --resource-group "myResourceGroup" \
    --name $STORAGEACCT \
    --yes
```

## <a name="next-steps"></a>Sonraki adımlar
- [Dosya paylaşımlarını Azure portalıyla yönetme](storage-how-to-use-files-portal.md)
- [Dosya paylaşımlarını Azure PowerShell ile yönetme](storage-how-to-use-files-powershell.md)
- [Depolama Gezgini'yle dosya paylaşımlarını yönetme](storage-how-to-use-files-storage-explorer.md)
- [Azure Dosyaları dağıtımını planlama](storage-files-planning.md)