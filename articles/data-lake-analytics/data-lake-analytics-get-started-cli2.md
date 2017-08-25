---
title: "Azure CLI 2.0 aracını kullanarak Azure Data Lake Analytics ile çalışmaya başlama | Microsoft Docs"
description: "Bir Data Lake Analytics hesabı oluşturmak, U-SQL'yi kullanarak Data Lake Analytics işi oluşturmak ve işi göndermek için Azure Komut Satırı Arabirimi 2.0 aracının nasıl kullanılacağını öğrenin. "
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/18/2017
ms.author: jgao
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: cb5872a93bbff99161cd5f61e6e26bdb0d322587
ms.contentlocale: tr-tr
ms.lasthandoff: 08/21/2017

---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-cli-20"></a>Azure CLI 2.0 aracını kullanarak Azure Data Lake Analytics ile çalışmaya başlama
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Bu öğreticide, sekmeyle ayrılmış değerler (TSV) dosyasını okuyan ve bu dosyayı virgülle ayrılmış değerler (CSV) dosyasına dönüştüren bir iş geliştireceksiniz. Öğreticiyi desteklenen diğer araçları kullanarak tamamlamak için bu bölümün üst kısmındaki açılan listeyi kullanın.

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiye başlamadan önce aşağıdaki öğelere sahip olmanız gerekir:

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Azure CLI 2.0**. Bkz. [Azure CLI'yı yükleme ve yapılandırma](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="log-in-to-azure"></a>Azure'da oturum açma

Azure aboneliğinizde oturum açmak için:

```
azurecli
az login
```

Bir URL’ye gitmeniz ve bir kimlik doğrulama kodu girmeniz istenir.  Sonra yönergeleri izleyerek kimlik bilgilerinizi girin.

Oturum açtıktan sonra, oturum komutu aboneliklerinizi listeler.

Belirli bir aboneliği kullanmak için:

```
az account set --subscription <subscription id>
```

## <a name="create-data-lake-analytics-account"></a>Data Lake Analytics hesabı oluşturma
Herhangi bir işi çalıştırmadan önce bir Data Lake Analytics hesabına sahip olmanız gerekir. Bir Data Lake Analytics hesabı oluşturmak için aşağıdaki öğeleri belirtmeniz gerekir:

* **Azure Kaynak Grubu**. Azure Kaynak Grubu'nda bir Data Lake Analytics hesabı oluşturulmalıdır. [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md), uygulamanızdaki kaynaklarla bir grup olarak çalışmanıza olanak sağlar. Uygulamanıza ilişkin tüm kaynakları tek ve eşgüdümlü bir işlem ile dağıtabilir, güncelleştirebilir veya silebilirsiniz.  

Aboneliğiniz altındaki mevcut kaynak gruplarını listelemek için:

```
az group list
```

Yeni bir kaynak grubu oluşturmak için:

```
az group create --name "<Resource Group Name>" --location "<Azure Location>"
```

* **Data Lake Analytics hesap adı**. Her Data Lake Analytics hesabının bir adı vardır.
* **Konum**. Data Lake Analytics'i destekleyen Azure veri merkezlerinden birini kullanın.
* **Varsayılan Data Lake Store hesabı**: Her Data Lake Analytics hesabı, bir varsayılan Data Lake Store hesabına sahiptir.

Mevcut bir Data Lake Store hesabını listelemek için:

```
az dls account list
```

Yeni bir Data Lake Store hesabı oluşturmak için:

```azurecli
az dls account create --account "<Data Lake Store Account Name>" --resource-group "<Resource Group Name>"
```

Bir Data Lake Analytics hesabı oluşturmak için aşağıdaki söz dizimini kullanın:

```
az dla account create --account "<Data Lake Analytics Account Name>" --resource-group "<Resource Group Name>" --location "<Azure location>" --default-data-lake-store "<Default Data Lake Store Account Name>"
```

Hesap oluşturduktan sonra aşağıdaki komutları kullanarak hesapları listeleyebilir ve hesap ayrıntılarını görüntüleyebilirsiniz:

```
az dla account list
az dla account show --account "<Data Lake Analytics Account Name>"            
```

## <a name="upload-data-to-data-lake-store"></a>Data Lake Store'a veri yükleme
Bu eğiticide, bazı arama günlüklerini işleyeceksiniz.  Arama günlüğü, Data Lake Store veya Azure Blob depolama alanında depolanabilir.

Azure portalı, bir arama günlüğü dosyası içeren bazı örnek veri dosyalarını varsayılan Data Lake Store hesabına kopyalamak için bir kullanıcı arabirimi sağlar. Verileri varsayılan Data Lake Store hesabına yüklemek için bkz. [Kaynak verileri hazırlama](data-lake-analytics-get-started-portal.md).

CLI 2.0 aracını kullanarak dosyaları karşıya yüklemek için şu komutları kullanın:

```
az dls fs upload --account "<Data Lake Store Account Name>" --source-path "<Source File Path>" --destination-path "<Destination File Path>"
az dls fs list --account "<Data Lake Store Account Name>" --path "<Path>"
```

Data Lake Analytics ayrıca Azure Blob depolama alanına da erişebilir.  Verileri Azure Blob depolama alanına yüklemek için bkz. [Azure CLI'yı Azure Storage ile kullanma](../storage/common/storage-azure-cli.md).

## <a name="submit-data-lake-analytics-jobs"></a>Data Lake Analytics işlerini gönderme
Data Lake Analytics işleri, U-SQL dilinde yazılır. U-SQL hakkında daha fazla bilgi edinmek için [U-SQL dili ile çalışmaya başlama](data-lake-analytics-u-sql-get-started.md) ve [U-SQL dil başvurusu](http://go.microsoft.com/fwlink/?LinkId=691348) bölümlerine göz atın.

**Data Lake Analytics iş betiği oluşturmak için**

Aşağıdaki U-SQL betiği ile bir metin dosyası oluşturun ve metin dosyasını iş istasyonunuza kaydedin:

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
```

Bu U-SQL betiği, **Extractors.Tsv()** öğesini kullanarak kaynak veri dosyasını okur ve ardından **Outputters.Csv()** öğesini kullanarak bir csv dosyası oluşturur.

Kaynak dosyayı farklı bir konuma kopyalamadıkça bu iki yolu değiştirmeyin.  Data Lake Analytics, mevcut olmaması halinde çıkış klasörünü oluşturur.

Varsayılan Data Lake Store hesaplarında depolanan dosyalar için göreli yolların kullanılması daha basittir. Mutlak yol da kullanabilirsiniz.  Örneğin:

```
adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
```

Mutlak yolları, bağlı Storagehesaplarındaki dosyalara erişmek için kullanmanız gerekir.  Bağlı Azure Storage hesabında depolanan dosyalar için söz dizimi şu şekildedir:

```
wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv
```

> [!NOTE]
> Genel bloblar ile Azure Blob kapsayıcısı desteklenmiyor.      
> Genel kapsayıcılar ile Azure Blob kapsayıcısı desteklenmiyor.      
>

**İşleri göndermek için**

Bir işi göndermek için aşağıdaki söz dizimini kullanın.

```
az dla job submit --account "<Data Lake Analytics Account Name>" --job-name "<Job Name>" --script "<Script Path and Name>"
```

Örneğin:

```
az dla job submit --account "myadlaaccount" --job-name "myadlajob" --script @"C:\DLA\myscript.txt"
```

**İşleri listelemek ve iş ayrıntılarını görüntülemek için**

```
azurecli
az dla job list --account "<Data Lake Analytics Account Name>"
az dla job show --account "<Data Lake Analytics Account Name>" --job-identity "<Job Id>"
```

**İşleri iptal etmek için**

```
az dla job cancel --account "<Data Lake Analytics Account Name>" --job-identity "<Job Id>"
```

##<a name="retrieve-job-results"></a>İş sonuçlarını alma

Bir iş tamamlandıktan sonra, aşağıdaki cmdlet'leri kullanarak çıkış dosyalarını listeleyebilir ve dosyaları indirebilirsiniz:

```
az dls fs list --account "<Data Lake Store Account Name>" --source-path "/Output" --destination-path "<Destintion>"
az dls fs preview --account "<Data Lake Store Account Name>" --path "/Output/SearchLog-from-Data-Lake.csv"
az dls fs preview --account "<Data Lake Store Account Name>" --path "/Output/SearchLog-from-Data-Lake.csv" --length 128 --offset 0
az dls fs downlod --account "<Data Lake Store Account Name>" --source-path "/Output/SearchLog-from-Data-Lake.csv" --destintion-path "<Destination Path and File Name>"
```

Örneğin:

```
az dls fs downlod --account "myadlsaccount" --source-path "/Output/SearchLog-from-Data-Lake.csv" --destintion-path "C:\DLA\myfile.csv"
```

## <a name="next-steps"></a>Sonraki adımlar

* Data Lake Analytics CLI 2.0 başvuru belgesini görmek için bkz. [Data Lake Analytics](https://docs.microsoft.com/cli/azure/dla).
* Data Lake Store CLI 2.0 başvuru belgesini görmek için bkz. [Data Lake Store](https://docs.microsoft.com/cli/azure/dls).
* Daha karmaşık bir sorgu görmek için [Azure Data Lake Analytics'i kullanarak Web sitesi günlüklerini çözümleme](data-lake-analytics-analyze-weblogs.md) makalesine bakın.

