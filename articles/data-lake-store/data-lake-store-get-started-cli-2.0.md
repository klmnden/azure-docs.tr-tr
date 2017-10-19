---
title: "Azure komut satırı arabirimi 2.0 aracını kullanarak Azure Data Lake Store ile çalışmaya başlama | Microsoft Docs"
description: "Bir Data Lake Store hesabı oluşturmak ve temel işlemleri gerçekleştirmek için Azure platformlar arası komut satırı 2.0 aracını kullanma"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 4ffa0f4a-1cca-46ac-803d-1fc8538c685b
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/28/2017
ms.author: nitinme
ms.openlocfilehash: 431c974401c201a76b6d20de9837e44374716417
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-azure-data-lake-store-using-azure-cli-20"></a>Azure CLI 2.0 kullanarak Azure Data Lake Store ile çalışmaya başlama
> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md)
>
> 

Azure Data Lake Store hesabı oluşturma ve klasör oluşturma, veri dosyalarını karşıya yükleme ve indirme, hesabınızı silme gibi temel işlemleri gerçekleştirmek için Azure CLI 2.0 (Önizleme) aracının nasıl kullanılacağını öğrenin. Data Lake Store hakkında daha fazla bilgi için bkz. [Data Lake Store'a Genel Bakış](data-lake-store-overview.md).

Azure CLI 2.0, Azure kaynaklarını yönetmek için Azure tarafından sunulan yeni komut satırı deneyimidir. MacOS, Linux ve Windows’da kullanılabilir. Daha fazla bilgi edinmek için bkz. [Azure CLI 2.0 aracına genel bakış](https://docs.microsoft.com/cli/azure/overview). Tam komut ve söz dizimi listesi için [Azure Data Lake Store CLI 2.0 başvurusuna](https://docs.microsoft.com/cli/azure/dls) da bakabilirsiniz.


## <a name="prerequisites"></a>Ön koşullar
Bu makaleye başlamadan önce aşağıdakilere sahip olmanız ve aşağıdaki işlemleri yapmış olmanız gerekir:

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

* **Azure CLI 2.0** - Yönergeler için kz. [Azure CLI 2.0 aracını yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="authentication"></a>Kimlik Doğrulaması

Bu makalede Data Lake Store için son kullanıcı olarak oturum açtığınız daha basit bir kimlik doğrulama yaklaşımı kullanılmaktadır. Data Lake Store hesabına ve dosya sistemine erişim düzeyi bu durumda oturum açmış kullanıcının erişim düzeyine göre yönetilir. Ancak, Data Lake Store kimlik doğrulaması için **son kullanıcı kimlik doğrulaması** veya **hizmetten hizmete kimlik doğrulama** şeklinde diğer yaklaşımlar da mevcuttur. Kimlik doğrulaması gerçekleştirmeyle ilgili yönergeler ve daha fazla bilgi için [Son kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-using-active-directory.md) veya [Hizmetten hizmete kimlik doğrulaması](data-lake-store-authenticate-using-active-directory.md) bölümlerine göz atın.


## <a name="log-in-to-your-azure-subscription"></a>Azure aboneliğinizde oturum açın

1. Azure aboneliğinizde oturum açın.

    ```azurecli
    az login
    ```

    Sonraki adımda kullanmak üzere bir kod alırsınız. Kimliğinizi doğrulamak için bir web tarayıcısı kullanarak https://aka.ms/devicelogin adresine gidin ve kodu girin. Kimlik bilgilerinizi kullanarak oturum açmanız istenir.

2. Oturum açtığınızda, pencerede hesabınızla ilişkili tüm Azure abonelikleri listelenir. Belirli bir aboneliği kullanmak için aşağıdaki komutu kullanın.
   
    ```azurecli
    az account set --subscription <subscription id> 
    ```

## <a name="create-an-azure-data-lake-store-account"></a>Azure Data Lake Store hesabı oluşturma

1. Yeni bir kaynak grubu oluşturun. Aşağıdaki komut içinde kullanmak istediğiniz parametre değerlerini sağlayın. Konum adı boşluk içeriyorsa adı tırnak işaretleri içine alın. Örneğin, "Doğu ABD 2". 
   
    ```azurecli
    az group create --location "East US 2" --name myresourcegroup
    ```

2. Data Lake Store hesabını oluşturun.
   
    ```azurecli
    az dls account create --account mydatalakestore --resource-group myresourcegroup
    ```

## <a name="create-folders-in-a-data-lake-store-account"></a>Data Lake Store hesabında klasör oluşturma

Veri depolamak ve yönetmek için Azure Data Lake Store hesabınızın altında klasör oluşturabilirsiniz. Aşağıdaki komutu kullanarak Data Lake Store'un kökünde **mynewfolder** adlı bir klasör oluşturun.

```azurecli
az dls fs create --account mydatalakestore --path /mynewfolder --folder
```

> [!NOTE]
> `--folder` parametresi, komutun bir klasör oluşturmasını sağlar. Bu parametre yoksa, komut tarafından Data Lake Store hesabının kökünde mynewfolder adlı boş bir dosya oluşturur.
> 
>

## <a name="upload-data-to-a-data-lake-store-account"></a>Data Lake Store hesabına veri yükleme

Data Lake Store'a doğrudan kök düzeyinde veya hesap içinde oluşturduğunuz bir klasöre yüklenecek şekilde veri yükleyebilirsiniz. Aşağıdaki kod parçacıkları, birtakım örnek verilerin önceki bölümde oluşturduğunuz klasöre (**mynewfolder**) nasıl yükleneceğini göstermektedir.

Karşıya yüklenecek örnek veri arıyorsanız [Azure Data Lake Git Deposu](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData)'ndan **Ambulance Data** klasörünü alabilirsiniz. Dosyayı indirin ve bilgisayarınızda C:\sampledata\ gibi yerel bir dizinde depolayın.

```azurecli
az dls fs upload --account mydatalakestore --source-path "C:\SampleData\AmbulanceData\vehicle1_09142014.csv" --destination-path "/mynewfolder/vehicle1_09142014.csv"
```

> [!NOTE]
> Hedef için dosya adı dahil olmak üzere yolun tamamını belirtmeniz gerekir.
> 
>


## <a name="list-files-in-a-data-lake-store-account"></a>Data Lake Store hesabındaki dosyaları listeleme

Bir Data Lake Store hesabındaki dosyaları listelemek için aşağıdaki komutu kullanın.

```azurecli
az dls fs list --account mydatalakestore --path /mynewfolder
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

## <a name="rename-download-and-delete-data-from-a-data-lake-store-account"></a>Data Lake Store hesabındaki verileri yeniden adlandırma, indirme ve silme 

* **Bir dosyayı yeniden adlandırmak için** aşağıdaki komutu kullanın:
  
    ```azurecli
    az dls fs move --account mydatalakestore --source-path /mynewfolder/vehicle1_09142014.csv --destination-path /mynewfolder/vehicle1_09142014_copy.csv
    ```

* **Bir dosyayı indirmek için** aşağıdaki komutu kullanın. Belirttiğiniz hedef yolun önceden var olduğundan emin olun.
  
    ```azurecli     
    az dls fs download --account mydatalakestore --source-path /mynewfolder/vehicle1_09142014_copy.csv --destination-path "C:\mysampledata\vehicle1_09142014_copy.csv"
    ```

    > [!NOTE]
    > Bu komut, henüz mevcut değilse hedef klasörü oluşturur.
    > 
    >

* **Bir dosyayı silmek için** aşağıdaki komutu kullanın:
  
    ```azurecli
    az dls fs delete --account mydatalakestore --path /mynewfolder/vehicle1_09142014_copy.csv
    ```

    Hem **mynewfolder** klasörünü hem de **vehicle1_09142014_copy.csv** dosyasını tek bir komutla silmek istiyorsanız --recurse parametresini kullanın

    ```azurecli
    az dls fs delete --account mydatalakestore --path /mynewfolder --recurse
    ```

## <a name="work-with-permissions-and-acls-for-a-data-lake-store-account"></a>Data Lake Store hesabı için izin ve ACL’ler ile çalışma

Bu bölümde, Azure CLI 2.0 aracını kullanarak ACL’leri ve izinleri nasıl yönetebileceğiniz hakkında bilgi edineceksiniz. Azure Data Lake Store’da ACL’lerin nasıl uygulandığıyla ilgili ayrıntılı bir tartışma için bkz. [Azure Data Lake Store’da erişim denetimi](data-lake-store-access-control.md).

* **Bir dosya veya klasörün sahibini güncelleştirmek için** aşağıdaki komutu kullanın:

    ```azurecli
    az dls fs access set-owner --account mydatalakestore --path /mynewfolder/vehicle1_09142014.csv --group 80a3ed5f-959e-4696-ba3c-d3c8b2db6766 --owner 6361e05d-c381-4275-a932-5535806bb323
    ```

* **Bir dosya veya klasörün izinlerini güncelleştirmek için** aşağıdaki komutu kullanın:

    ```azurecli
    az dls fs access set-permission --account mydatalakestore --path /mynewfolder/vehicle1_09142014.csv --permission 777
    ```
    
* **Belirli bir yolun ACL’lerini almak için** aşağıdaki komutu kullanın:

    ```azurecli
    az dls fs access show --account mydatalakestore --path /mynewfolder/vehicle1_09142014.csv
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
    az dls fs access set-entry --account mydatalakestore --path /mynewfolder --acl-spec user:6360e05d-c381-4275-a932-5535806bb323:-w-
    ```

* **ACL’ye yönelik bir girişi kaldırmak için** aşağıdaki komutu kullanın:

    ```azurecli
    az dls fs access remove-entry --account mydatalakestore --path /mynewfolder --acl-spec user:6360e05d-c381-4275-a932-5535806bb323
    ```

* **Varsayılan ACL’nin tamamını kaldırmak için** aşağıdaki komutu kullanın:

    ```azurecli
    az dls fs access remove-all --account mydatalakestore --path /mynewfolder --default-acl
    ```

* **Varsayılan olmayan bir ACL’nin tamamını kaldırmak için** aşağıdaki komutu kullanın:

    ```azurecli
    az dls fs access remove-all --account mydatalakestore --path /mynewfolder
    ```
    
## <a name="delete-a-data-lake-store-account"></a>Data Lake Store hesabını silme
Bir Data Lake Store hesabını silmek için aşağıdaki komutu kullanın.

```azurecli
az dls account delete --account mydatalakestore
```

İstendiğinde, hesabı silmek için **Y** yazın.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Data Lake Store'u büyük veri gereksinimleri için kullanma](data-lake-store-data-scenarios.md) 
* [Data Lake Store'da verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics'i Data Lake Store ile kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight'ı Data Lake Store ile kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)
