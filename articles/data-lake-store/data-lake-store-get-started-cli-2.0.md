---
title: Azure Data Lake depolama Gen1 ile çalışmaya başlamak için Azure CLI'yı kullanın | Microsoft Docs
description: Bir Data Lake depolama Gen1 hesabı oluşturmak ve temel işlemleri gerçekleştirmek için Azure CLI kullanma
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: twooley
ms.openlocfilehash: 9431cc7fa12b86371ce6b2325aca8e13d264442e
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58880585"
---
# <a name="get-started-with-azure-data-lake-store-using-azure-cli"></a>Azure Data Lake Azure CLI kullanarak Store ile çalışmaya başlama

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)]

> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [Azure CLI](data-lake-store-get-started-cli-2.0.md)
>
> 

Hesabı, silme, bir Azure Data Lake depolama Gen1 hesabı oluşturmak ve klasör oluşturma karşıya yükleme ve veri dosyalarını indirir temel işlemleri gerçekleştirmek için Azure CLI kullanma hakkında bilgi edinin. Data Lake depolama Gen1 hakkında daha fazla bilgi için bkz: [genel bakış Data Lake depolama Gen1](data-lake-store-overview.md).

Azure CLI, Azure kaynaklarını yönetmek için Azure tarafından sunulan komut satırı deneyimidir. MacOS, Linux ve Windows’da kullanılabilir. Daha fazla bilgi için [genel bakış, Azure CLI](https://docs.microsoft.com/cli/azure). Ayrıca bakabilirsiniz [Azure Data Lake depolama Gen1 CLI başvuru](https://docs.microsoft.com/cli/azure/dls) komutlar ve söz dizimi tam listesi için.


## <a name="prerequisites"></a>Önkoşullar
Bu makaleye başlamadan önce aşağıdakilere sahip olmanız ve aşağıdaki işlemleri yapmış olmanız gerekir:

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

* **Azure CLI** -bkz [Azure CLI yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli) yönergeler için.

## <a name="authentication"></a>Authentication

Bu makalede, Data Lake depolama son kullanıcı olarak oturum burada Gen1 ile basit bir kimlik doğrulama yaklaşımı kullanılmaktadır. Data Lake depolama hesabı ve dosya sistemi sonra oturum açmış olan kullanıcının erişim düzeyi tarafından yönetilir Gen1 için erişim düzeyi. Ancak, diğer yaklaşımlar da Data Lake depolama Gen1 ile kimlik doğrulaması için olan vardır **son kullanıcı kimlik doğrulaması** veya **hizmetten hizmete kimlik doğrulaması**. Kimlik doğrulaması gerçekleştirmeyle ilgili yönergeler ve daha fazla bilgi için [Son kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-using-active-directory.md) veya [Hizmetten hizmete kimlik doğrulaması](data-lake-store-authenticate-using-active-directory.md) bölümlerine göz atın.


## <a name="log-in-to-your-azure-subscription"></a>Azure aboneliğinizde oturum açın

1. Azure aboneliğinizde oturum açın.

    ```azurecli
    az login
    ```

    Sonraki adımda kullanmak üzere bir kod alırsınız. Bir web tarayıcısı kullanarak https://aka.ms/devicelogin ve kimlik doğrulaması için kodu girin. Kimlik bilgilerinizi kullanarak oturum açmanız istenir.

2. Oturum açtığınızda, pencerede hesabınızla ilişkili tüm Azure abonelikleri listelenir. Belirli bir aboneliği kullanmak için aşağıdaki komutu kullanın.
   
    ```azurecli
    az account set --subscription <subscription id> 
    ```

## <a name="create-an-azure-data-lake-storage-gen1-account"></a>Azure Data Lake depolama Gen1 hesap oluşturma

1. Yeni bir kaynak grubu oluşturun. Aşağıdaki komut içinde kullanmak istediğiniz parametre değerlerini sağlayın. Konum adı boşluk içeriyorsa adı tırnak işaretleri içine alın. Örneğin, "Doğu ABD 2". 
   
    ```azurecli
    az group create --location "East US 2" --name myresourcegroup
    ```

2. Data Lake depolama Gen1 hesabı oluşturun.
   
    ```azurecli
    az dls account create --account mydatalakestoragegen1 --resource-group myresourcegroup
    ```

## <a name="create-folders-in-a-data-lake-storage-gen1-account"></a>Bir Data Lake depolama Gen1 hesabında klasör oluşturma

Veri depolamak ve yönetmek için Azure Data Lake depolama Gen1 hesabınızın altında klasör oluşturabilirsiniz. Adlı bir klasör oluşturmak için aşağıdaki komutu kullanın **mynewfolder** Data Lake depolama Gen1 hesabının kökü.

```azurecli
az dls fs create --account mydatalakestoragegen1 --path /mynewfolder --folder
```

> [!NOTE]
> `--folder` parametresi, komutun bir klasör oluşturmasını sağlar. Bu parametre yoksa, komut, Data Lake depolama Gen1 hesabının kökünde mynewfolder adlı boş bir dosya oluşturur.
> 
>

## <a name="upload-data-to-a-data-lake-storage-gen1-account"></a>Bir Data Lake depolama Gen1 hesabına veri yükleme

Data Lake depolama Gen1 doğrudan kök düzeyinde veya hesap içinde oluşturduğunuz bir klasöre verileri karşıya yükleyebilirsiniz. Aşağıdaki kod parçacıkları, birtakım örnek verilerin önceki bölümde oluşturduğunuz klasöre (**mynewfolder**) nasıl yükleneceğini göstermektedir.

Karşıya yüklenecek örnek veri arıyorsanız [Azure Data Lake Git Deposu](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData)'ndan **Ambulance Data** klasörünü alabilirsiniz. Dosyayı indirin ve bilgisayarınızda C:\sampledata\ gibi yerel bir dizinde depolayın.

```azurecli
az dls fs upload --account mydatalakestoragegen1 --source-path "C:\SampleData\AmbulanceData\vehicle1_09142014.csv" --destination-path "/mynewfolder/vehicle1_09142014.csv"
```

> [!NOTE]
> Hedef için dosya adı dahil olmak üzere yolun tamamını belirtmeniz gerekir.
> 
>


## <a name="list-files-in-a-data-lake-storage-gen1-account"></a>Bir Data Lake depolama Gen1 hesabındaki dosyaları Listele

Bir Data Lake depolama Gen1 hesabındaki dosyaları listelemek için aşağıdaki komutu kullanın.

```azurecli
az dls fs list --account mydatalakestoragegen1 --path /mynewfolder
```

Bunun çıktısının aşağıdakine benzer olması gerekir:

    [
        {
            "accessTime": 1491323529542,
            "aclBit": false,
            "blockSize": 268435456,
            "group": "1808bd5f-62af-45f4-89d8-03c5e81bac20",
            "length": 1589881,
            "modificationTime": 1491323531638,
            "msExpirationTime": 0,
            "name": "mynewfolder/vehicle1_09142014.csv",
            "owner": "1808bd5f-62af-45f4-89d8-03c5e81bac20",
            "pathSuffix": "vehicle1_09142014.csv",
            "permission": "770",
            "replication": 1,
            "type": "FILE"
        }
    ]

## <a name="rename-download-and-delete-data-from-a-data-lake-storage-gen1-account"></a>Yeniden adlandırma, indirme ve Data Lake depolama Gen1 hesabından verilerini sil 

* **Bir dosyayı yeniden adlandırmak için** aşağıdaki komutu kullanın:
  
    ```azurecli
    az dls fs move --account mydatalakestoragegen1 --source-path /mynewfolder/vehicle1_09142014.csv --destination-path /mynewfolder/vehicle1_09142014_copy.csv
    ```

* **Bir dosyayı indirmek için** aşağıdaki komutu kullanın. Belirttiğiniz hedef yolun önceden var olduğundan emin olun.
  
    ```azurecli     
    az dls fs download --account mydatalakestoragegen1 --source-path /mynewfolder/vehicle1_09142014_copy.csv --destination-path "C:\mysampledata\vehicle1_09142014_copy.csv"
    ```

    > [!NOTE]
    > Bu komut, henüz mevcut değilse hedef klasörü oluşturur.
    > 
    >

* **Bir dosyayı silmek için** aşağıdaki komutu kullanın:
  
    ```azurecli
    az dls fs delete --account mydatalakestoragegen1 --path /mynewfolder/vehicle1_09142014_copy.csv
    ```

    Hem **mynewfolder** klasörünü hem de **vehicle1_09142014_copy.csv** dosyasını tek bir komutla silmek istiyorsanız --recurse parametresini kullanın

    ```azurecli
    az dls fs delete --account mydatalakestoragegen1 --path /mynewfolder --recurse
    ```

## <a name="work-with-permissions-and-acls-for-a-data-lake-storage-gen1-account"></a>Bir Data Lake depolama Gen1 hesabı için izin ve ACL'ler ile çalışma

Bu bölümde ACL'leri ve izinleri Azure CLI kullanarak yönetme hakkında bilgi edinin. Azure Data Lake depolama Gen1 ACL'leri nasıl uygulandığı hakkında ayrıntılı bilgi için bkz: [erişim denetimi, Azure Data Lake depolama Gen1](data-lake-store-access-control.md).

* **Bir dosya veya klasörün sahibini güncelleştirmek için** aşağıdaki komutu kullanın:

    ```azurecli
    az dls fs access set-owner --account mydatalakestoragegen1 --path /mynewfolder/vehicle1_09142014.csv --group 80a3ed5f-959e-4696-ba3c-d3c8b2db6766 --owner 6361e05d-c381-4275-a932-5535806bb323
    ```

* **Bir dosya veya klasörün izinlerini güncelleştirmek için** aşağıdaki komutu kullanın:

    ```azurecli
    az dls fs access set-permission --account mydatalakestoragegen1 --path /mynewfolder/vehicle1_09142014.csv --permission 777
    ```
    
* **Belirli bir yolun ACL’lerini almak için** aşağıdaki komutu kullanın:

    ```azurecli
    az dls fs access show --account mydatalakestoragegen1 --path /mynewfolder/vehicle1_09142014.csv
    ```

    Çıktının aşağıdakine benzer olması gerekir:

        {
            "entries": [
            "user::rwx",
            "group::rwx",
            "other::---"
          ],
          "group": "1808bd5f-62af-45f4-89d8-03c5e81bac20",
          "owner": "1808bd5f-62af-45f4-89d8-03c5e81bac20",
          "permission": "770",
          "stickyBit": false
        }

* **ACL’ye yönelik bir giriş ayarlamak için** aşağıdaki komutu kullanın:

    ```azurecli
    az dls fs access set-entry --account mydatalakestoragegen1 --path /mynewfolder --acl-spec user:6360e05d-c381-4275-a932-5535806bb323:-w-
    ```

* **ACL’ye yönelik bir girişi kaldırmak için** aşağıdaki komutu kullanın:

    ```azurecli
    az dls fs access remove-entry --account mydatalakestoragegen1 --path /mynewfolder --acl-spec user:6360e05d-c381-4275-a932-5535806bb323
    ```

* **Varsayılan ACL’nin tamamını kaldırmak için** aşağıdaki komutu kullanın:

    ```azurecli
    az dls fs access remove-all --account mydatalakestoragegen1 --path /mynewfolder --default-acl
    ```

* **Varsayılan olmayan bir ACL’nin tamamını kaldırmak için** aşağıdaki komutu kullanın:

    ```azurecli
    az dls fs access remove-all --account mydatalakestoragegen1 --path /mynewfolder
    ```
    
## <a name="delete-a-data-lake-storage-gen1-account"></a>Bir Data Lake depolama Gen1 hesabını Sil
Bir Data Lake depolama Gen1 hesabını silmek için aşağıdaki komutu kullanın.

```azurecli
az dls account delete --account mydatalakestoragegen1
```

İstendiğinde, hesabı silmek için **Y** yazın.

## <a name="next-steps"></a>Sonraki adımlar
* [Büyük veri gereksinimleri için Azure Data Lake depolama Gen1 kullanın](data-lake-store-data-scenarios.md) 
* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics'i Data Lake depolama Gen1 ile kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight ile Data Lake depolama Gen1 kullanın](data-lake-store-hdinsight-hadoop-use-portal.md)
