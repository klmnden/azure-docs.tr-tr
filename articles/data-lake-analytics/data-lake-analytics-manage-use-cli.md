---
title: Azure komut satırı arabirimi kullanarak Azure Data Lake Analytics yönetme
description: Bu makalede, Data Lake Analytics işleri, veri kaynakları ve kullanıcıları yönetmek için Azure CLI kullanmayı açıklar.
services: data-lake-analytics
author: jasonwhowell
ms.author: jasonh
manager: kfile
ms.assetid: 4e5a3a0a-6d7f-43ed-aeb5-c3b3979a1e0a
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 01/29/2018
ms.openlocfilehash: 86fa41db2d21beac08015d067b79ce1375cd3ddf
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34736098"
---
# <a name="manage-azure-data-lake-analytics-using-the-azure-command-line-interface-cli"></a>Azure komut satırı arabirimi (CLI) kullanarak Azure Data Lake Analytics yönetme

[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Azure Data Lake Analytics hesaplarını, veri kaynakları, kullanıcılar ve işleri Azure CLI kullanarak yönetmeyi öğrenin. Yönetimi konuları diğer araçları kullanarak görmek için yukarıdaki sekme seçimine tıklayın.


**Önkoşullar**

Bu öğreticiye başlamadan önce aşağıdaki kaynaklara sahip olmalıdır:

* Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

* Azure CLI. Bkz. [Azure CLI'yı yükleme ve yapılandırma](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

   * Bu demoyu tamamlamak için **yayın öncesi sürüm** [Azure CLI araçlarını](https://github.com/MicrosoftBigData/AzureDataLake/releases) indirip yükleyin.

* Kullanarak kimlik doğrulaması `az login` komut ve kullanmak istediğiniz aboneliği seçin. Bir iş veya okul hesabı kullanarak kimlik doğrulama gerçekleştirme konusunda daha fazla bilgi için bkz. [Azure CLI'dan Azure aboneliğine bağlanma](/cli/azure/authenticate-azure-cli).

   ```azurecli
   az login
   az account set --subscription <subscription id>
   ```

   Data Lake Analytics ve Data Lake Store komutları artık erişebilirsiniz. Data Lake Store ve Data Lake Analytics komutları listelemek için aşağıdaki komutu çalıştırın:

   ```azurecli
   az dls -h
   az dla -h
   ```

## <a name="manage-accounts"></a>Hesapları yönetme

Herhangi bir Data Lake Analytics işi çalıştırmadan önce bir Data Lake Analytics hesabınızın olması gerekir. Azure Hdınsight, bir iş çalışmadığı zaman Analytics hesabı için ödemeniz gerekmez. Yalnızca bir iş çalışırken zaman için ödeme yaparsınız.  Daha fazla bilgi için bkz: [Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md).  

### <a name="create-accounts"></a>Hesapları oluşturma

Bir Data Lake hesabı oluşturmak için aşağıdaki komutu çalıştırın 

   ```azurecli
   az dla account create --account "<Data Lake Analytics account name>" --location "<Location Name>" --resource-group "<Resource Group Name>" --default-data-lake-store "<Data Lake Store account name>"
   ```

### <a name="update-accounts"></a>Güncelleştirme hesapları

Aşağıdaki komut varolan bir Data Lake Analytics hesabı özelliklerini güncelleştirir

   ```azurecli
   az dla account update --account "<Data Lake Analytics Account Name>" --firewall-state "Enabled" --query-store-retention 7
   ```

### <a name="list-accounts"></a>Hesapları listeleme

Belirli bir kaynak grubunun içinde listesi Data Lake Analytics hesapları

   ```azurecli
   az dla account list "<Resource group name>"
   ```

## <a name="get-details-of-an-account"></a>Bir hesap ayrıntılarını alma

   ```azurecli
   az dla account show --account "<Data Lake Analytics account name>" --resource-group "<Resource group name>"
   ```

### <a name="delete-an-account"></a>Hesap silme

   ```azurecli
   az dla account delete --account "<Data Lake Analytics account name>" --resource-group "<Resource group name>"
   ```

## <a name="manage-data-sources"></a>Veri kaynaklarını yönet

Data Lake Analytics şu anda aşağıdaki iki veri kaynaklarını destekler:

* [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md)
* [Azure Depolama](../storage/common/storage-introduction.md)

Analytics hesabı oluşturduğunuzda, varsayılan depolama hesabı olacak şekilde bir Azure Data Lake Store hesabına atamanız gerekir. Varsayılan Data Lake storage hesabına iş meta verileri ve iş denetim günlüklerini depolamak için kullanılır. Analytics hesabı oluşturduktan sonra ek Data Lake Storage hesaplarını ve/veya Azure Storage hesabı ekleyebilirsiniz. 

### <a name="find-the-default-data-lake-store-account"></a>Varsayılan Data Lake Store hesabı bulunamadı

Çalıştıran tarafından kullanılan varsayılan Data Lake Store hesabı görüntüleyebilirsiniz `az dla account show` komutu. Varsayılan hesap adı altında defaultDataLakeStoreAccount özellik listelenir.

   ```azurecli
   az dla account show --account "<Data Lake Analytics account name>"
   ```

### <a name="add-additional-blob-storage-accounts"></a>Ek Blob storage hesapları ekleme

   ```azurecli
   az dla account blob-storage add --access-key "<Azure Storage Account Key>" --account "<Data Lake Analytics account name>" --storage-account-name "<Storage account name>"
   ```

> [!NOTE]
> Yalnızca Blob Depolama kısa adları desteklenir. FQDN, örneğin "myblob.blob.core.windows.net" kullanmayın.
> 

### <a name="add-additional-data-lake-store-accounts"></a>Ek Data Lake Store hesapları ekleme

Aşağıdaki komut, belirtilen Data Lake Analytics hesabı ek bir Data Lake Store hesabı ile güncelleştirir:

   ```azurecli
   az dla account data-lake-store add --account "<Data Lake Analytics account name>" --data-lake-store-account-name "<Data Lake Store account name>"
   ```

### <a name="update-existing-data-source"></a>Güncelleştirme mevcut veri kaynağı

Mevcut bir Blob Depolama hesabı anahtarı güncelleştirmek için:

   ```azurecli
   az dla account blob-storage update --access-key "<New Blob Storage Account Key>" --account "<Data Lake Analytics account name>" --storage-account-name "<Data Lake Store account name>"
   ```

### <a name="list-data-sources"></a>Liste veri kaynakları:

Data Lake Store hesaplarını listelemek için:

   ```azurecli
   az dla account data-lake-store list --account "<Data Lake Analytics account name>"
   ```

Blob storage hesabı listelemek için:

   ```azurecli
   az dla account blob-storage list --account "<Data Lake Analytics account name>"
   ```

![Data Lake Analytics veri kaynağı listesi](./media/data-lake-analytics-manage-use-cli/data-lake-analytics-list-data-source.png)

### <a name="delete-data-sources"></a>Veri kaynaklarını silin:
Bir Data Lake Store hesabını silmek için:

   ```azurecli
   az dla account data-lake-store delete --account "<Data Lake Analytics account name>" --data-lake-store-account-name "<Azure Data Lake Store account name>"
   ```

Bir Blob Depolama hesabını silmek için:

   ```azurecli
   az dla account blob-storage delete --account "<Data Lake Analytics account name>" --storage-account-name "<Data Lake Store account name>"
   ```

## <a name="manage-jobs"></a>İşleri yönetme
Bir proje oluşturmadan önce bir Data Lake Analytics hesabı olması gerekir.  Daha fazla bilgi için bkz: [yönetmek Data Lake Analytics hesapları](#manage-accounts).

### <a name="list-jobs"></a>Liste işleri

   ```azurecli
   az dla job list --account "<Data Lake Analytics account name>"
   ```

   ![Data Lake Analytics veri kaynağı listesi](./media/data-lake-analytics-manage-use-cli/data-lake-analytics-list-jobs.png)

### <a name="get-job-details"></a>İş ayrıntılarını almak

   ```azurecli
   az dla job show --account "<Data Lake Analytics account name>" --job-identity "<Job Id>"
   ```

### <a name="submit-jobs"></a>İşlerini gönderme

> [!NOTE]
> Bir işin varsayılan öncelik 1000'dir ve varsayılan paralellik derecesi bir iş için 1'dir.
> 
   ```azurecli
   az dla job submit --account "<Data Lake Analytics account name>" --job-name "<Name of your job>" --script "<Script to submit>"
   ```

### <a name="cancel-jobs"></a>İşlerini iptal edin
İş kimliği ve ardından işi iptal etmek için İptal bulmak için liste komutunu kullanın.

   ```azurecli
   az dla job cancel --account "<Data Lake Analytics account name>" --job-identity "<Job Id>"
   ```

## <a name="pipelines-and-recurrences"></a>İşlem hatları ve tekrarlar

**İşlem hatları ve tekrarlar hakkında bilgi edinin**

Önceden gönderilmiş işler hakkında işlem hatları bilgilerini görmek için `az dla job pipeline` komutlarını kullanın.

```
az dla job pipeline list --account "<Data Lake Analytics Account Name>"

az dla job pipeline show --account "<Data Lake Analytics Account Name>" --pipeline-identity "<Pipeline ID>"
```

Önceden gönderilmiş işler hakkında tekrar bilgilerini görmek için `az dla job recurrence` komutlarını kullanın.

```
az dla job recurrence list --account "<Data Lake Analytics Account Name>"

az dla job recurrence show --account "<Data Lake Analytics Account Name>" --recurrence-identity "<Recurrence ID>"
```

## <a name="see-also"></a>Ayrıca bkz.
* [Microsoft Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* [Azure portalını kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md)
* [Azure Data Lake Analytics'i Azure portalını kullanarak yönetme](data-lake-analytics-manage-use-portal.md)
* [Azure portalını kullanarak Azure Data Lake Analytics işlerini izleme ve sorun giderme](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

