---
title: "Azure komut satırı arabirimi kullanarak Azure Data Lake Analytics yönetme | Microsoft Docs"
description: "Data Lake Analytics hesapları, veri kaynaklarını, işlerini ve kullanıcıları Azure CLI kullanarak yönetmeyi öğrenin"
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: 4e5a3a0a-6d7f-43ed-aeb5-c3b3979a1e0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
ms.openlocfilehash: f90bada3572c0ed40b07d76ec02c1b499bbd1428
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="manage-azure-data-lake-analytics-using-azure-command-line-interface-cli"></a>Azure komut satırı arabirimi (CLI) kullanarak Azure Data Lake Analytics yönetme
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Azure Data Lake Analytics hesaplarını, veri kaynakları, kullanıcılar ve işleri Azure CLI kullanarak yönetmeyi öğrenin. Yönetimi konuları diğer araçları kullanarak görmek için yukarıdaki sekme seçimine tıklayın.


**Önkoşullar**

Bu öğreticiye başlamadan önce aşağıdakilere sahip olmanız gerekir:

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
* **Azure CLI**. Bkz. [Azure CLI'yı yükleme ve yapılandırma](../cli-install-nodejs.md).
  * Bu demoyu tamamlamak için **yayın öncesi sürüm** [Azure CLI araçlarını](https://github.com/MicrosoftBigData/AzureDataLake/releases) indirip yükleyin.
* Şu komutu kullanarak **kimlik doğrulama**:
  
        azure login
    Bir iş veya okul hesabı kullanarak kimlik doğrulama gerçekleştirme konusunda daha fazla bilgi için bkz. [Azure CLI'dan Azure aboneliğine bağlanma](../xplat-cli-connect.md).
* Şu komutu kullanarak **Azure Resource Manager moduna geçin**:
  
        azure config mode arm

**Data Lake Store ve Data Lake Analytics komutları listelemek için:**

    azure datalake store
    azure datalake analytics

<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-accounts"></a>Hesapları yönetme
Herhangi bir Data Lake Analytics işi çalıştırmadan önce bir Data Lake Analytics hesabınızın olması gerekir. Azure Hdınsight, bir iş çalışmadığı zaman Analytics hesabı için ödemeniz gerekmez.  Yalnızca bir iş çalışırken zaman için ödeme yaparsınız.  Daha fazla bilgi için bkz: [Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md).  

### <a name="create-accounts"></a>Hesapları oluşturma
      azure datalake analytics account create "<Data Lake Analytics Account Name>" "<Azure Location>" "<Resource Group Name>" "<Default Data Lake Account Name>"


### <a name="update-accounts"></a>Güncelleştirme hesapları
Aşağıdaki komut varolan bir Data Lake Analytics hesabı özelliklerini güncelleştirir

    azure datalake analytics account set "<Data Lake Analytics Account Name>"


### <a name="list-accounts"></a>Hesapları listeleme
Liste Data Lake Analytics hesapları 

    azure datalake analytics account list

Belirli bir kaynak grubunun içinde listesi Data Lake Analytics hesapları

    azure datalake analytics account list -g "<Azure Resource Group Name>"

Belirli bir Data Lake Analytics hesabı ayrıntılarını alma

    azure datalake analytics account show -g "<Azure Resource Group Name>" -n "<Data Lake Analytics Account Name>"

### <a name="delete-data-lake-analytics-accounts"></a>Data Lake Analytics hesapları silme
      azure datalake analytics account delete "<Data Lake Analytics Account Name>"


<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-account-data-sources"></a>Hesabı veri kaynaklarını yönet
Data Lake Analytics şu anda aşağıdaki veri kaynaklarını destekler:

* [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md)
* [Azure Depolama](../storage/common/storage-introduction.md)

Analytics hesabı oluşturduğunuzda, varsayılan depolama hesabı olacak şekilde bir Azure Data Lake Store hesabına atamanız gerekir. Varsayılan ADL depolama hesabı, iş meta verileri ve iş denetim günlüklerini depolamak için kullanılır. Analytics hesabı oluşturduktan sonra ek Data Lake Storage hesaplarını ve/veya Azure Storage hesabı ekleyebilirsiniz. 

### <a name="find-the-default-adl-storage-account"></a>Varsayılan ADL depolama hesabı bulunamadı
    azure datalake analytics account show "<Data Lake Analytics Account Name>"

Değer özellikler: datalakeStoreAccount:name altında listelenir.

### <a name="add-additional-azure-blob-storage-accounts"></a>Ek Azure Blob storage hesapları ekleme
      azure datalake analytics account datasource add -n "<Data Lake Analytics Account Name>" -b "<Azure Blob Storage Account Short Name>" -k "<Azure Storage Account Key>"

> [!NOTE]
> Yalnızca Blob Depolama kısa adları desteklenir.  FQDN, örneğin "myblob.blob.core.windows.net" kullanmayın.
> 
> 

### <a name="add-additional-data-lake-store-accounts"></a>Ek Data Lake Store hesapları ekleme
      azure datalake analytics account datasource add -n "<Data Lake Analytics Account Name>" -l "<Data Lake Store Account Name>" [-d]

[-d] eklenmekte olan Data Lake varsayılan Data Lake hesabı olup olmadığını belirtmek için isteğe bağlı bir anahtardır. 

### <a name="update-existing-data-source"></a>Güncelleştirme mevcut veri kaynağı
Varsayılan olarak mevcut bir Data Lake Store hesabı ayarlamak için:

      azure datalake analytics account datasource set -n "<Data Lake Analytics Account Name>" -l "<Azure Data Lake Store Account Name>" -d

Mevcut bir Blob Depolama hesabı anahtarı güncelleştirmek için:

      azure datalake analytics account datasource set -n "<Data Lake Analytics Account Name>" -b "<Blob Storage Account Name>" -k "<New Blob Storage Account Key>"

### <a name="list-data-sources"></a>Liste veri kaynakları:
    azure datalake analytics account show "<Data Lake Analytics Account Name>"

![Data Lake Analytics veri kaynağı listesi](./media/data-lake-analytics-manage-use-cli/data-lake-analytics-list-data-source.png)

### <a name="delete-data-sources"></a>Veri kaynaklarını silin:
Bir Data Lake Store hesabını silmek için:

      azure datalake analytics account datasource delete "<Data Lake Analytics Account Name>" "<Azure Data Lake Store Account Name>"

Bir Blob Depolama hesabını silmek için:

      azure datalake analytics account datasource delete "<Data Lake Analytics Account Name>" "<Blob Storage Account Name>"

## <a name="manage-jobs"></a>İşleri yönetme
Bir proje oluşturmadan önce bir Data Lake Analytics hesabı olması gerekir.  Daha fazla bilgi için bkz: [yönetmek Data Lake Analytics hesapları](#manage-accounts).

### <a name="list-jobs"></a>Liste işleri
      azure datalake analytics job list -n "<Data Lake Analytics Account Name>"

![Data Lake Analytics veri kaynağı listesi](./media/data-lake-analytics-manage-use-cli/data-lake-analytics-list-jobs.png)

### <a name="get-job-details"></a>İş ayrıntılarını almak
      azure datalake analytics job show -n "<Data Lake Analytics Account Name>" -j "<Job ID>"

### <a name="submit-jobs"></a>İşlerini gönderme
> [!NOTE]
> Bir işin varsayılan öncelik 1000'dir ve varsayılan paralellik derecesi bir iş için 1'dir.
> 
> 

    azure datalake analytics job create  "<Data Lake Analytics Account Name>" "<Job Name>" "<Script>"

### <a name="cancel-jobs"></a>İşlerini iptal edin
İş kimliği ve ardından işi iptal etmek için İptal bulmak için liste komutunu kullanın.

      azure datalake analytics job list -n "<Data Lake Analytics Account Name>"
      azure datalake analytics job cancel "<Data Lake Analytics Account Name>" "<Job ID>"

## <a name="manage-catalog"></a>Katalog Yönetimi
U-SQL kataloğunu, U-SQL komut dosyaları tarafından paylaşılabilmesi veri ve kod yapısı için kullanılır. Katalog Azure Data Lake verilerle olası en yüksek performans sağlar. Daha fazla bilgi için bkz. [U-SQL kataloğunu kullanma](data-lake-analytics-use-u-sql-catalog.md).

### <a name="list-catalog-items"></a>Liste katalog öğeleri
    #List databases
    azure datalake analytics catalog list -n "<Data Lake Analytics Account Name>" -t database

    #List tables
    azure datalake analytics catalog list -n "<Data Lake Analytics Account Name>" -t table

Veritabanı, şema, derleme, dış veri kaynağı, tablo, tablo değerli fonksiyon veya tablo istatistikleri türleri içerir.

<!-- ################################ -->
<!-- ################################ -->
## <a name="use-arm-groups"></a>ARM grupları kullanma
Uygulamalar genellikle web uygulaması, veritabanı, veritabanı sunucusu, depolama alanı ve 3. taraf hizmetleri gibi birçok bileşenden oluşur. Azure Resource Manager (ARM), uygulamanızdaki kaynaklarla Azure Kaynak Grubu şeklinde adlandırılan bir grup olarak çalışmanıza olanak sağlar. Uygulamanıza yönelik tüm kaynakları tek ve eşgüdümlü bir işlemle dağıtabilir, güncelleştirebilir, izleyebilir veya silebilirsiniz. Dağıtım için bir şablon kullanabilirsiniz. Üstelik bu şablon test, hazırlık ve üretim gibi farklı ortamlarda da çalışabilir. Tüm grup için aktarılmış maliyetleri görüntüleyerek kuruluşunuzun fatura işlemlerine açıklık getirebilirsiniz. Daha fazla bilgi için bkz. [Azure Resource Manager'a Genel Bakış](../azure-resource-manager/resource-group-overview.md). 

Bir Data Lake Analytics hizmeti aşağıdaki bileşenleri şunları içerebilir:

* Azure Data Lake Analytics hesabı
* Gerekli varsayılan Azure Data Lake Storage hesabı
* Ek Azure Data Lake Storage hesapları
* Ek Azure depolama hesapları

Tüm bu bileşenlerin yönetmeyi kolaylaştırmak için bir ARM grubu altında oluşturabilirsiniz.

![Azure Data Lake Analytics hesabı ve depolama](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-arm-structure.png)

Bir Data Lake Analytics hesabı ve bağımlı depolama hesapları aynı Azure veri merkezinde yer almalıdır.
ARM Grup ancak farklı veri merkezinde yer alabilir.  

## <a name="see-also"></a>Ayrıca bkz.
* [Microsoft Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* [Azure Portal'ı kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md)
* [Azure Data Lake Analytics'i Azure Portal'ı kullanarak yönetme](data-lake-analytics-manage-use-portal.md)
* [Azure Portal'ı kullanarak Azure Data Lake Analytics işlerini izleme ve sorun giderme](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

