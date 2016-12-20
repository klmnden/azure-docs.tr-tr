---
title: "Platformlar arası komut satırı arabirimini kullanarak Data Lake Store ile çalışmaya başlama | Microsoft Belgeleri"
description: "Bir Data Lake Store hesabı oluşturmak ve temel işlemleri gerçekleştirmek için Azure platformlar arası komut satırını kullanma"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 380788f3-041d-4ae5-b6be-37ca74ca333d
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/21/2016
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: c157da7bf53e2d0762624e8e71e56e956db04a24
ms.openlocfilehash: 236563a8814640391130ba53886c5cebfea67a8f


---
# <a name="get-started-with-azure-data-lake-store-using-azure-command-line"></a>Azure Komut Satırı'nı kullanarak Azure Data Lake Store ile çalışmaya başlama
> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Azure CLI](data-lake-store-get-started-cli.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Azure Data Lake Store hesabı oluşturmak ve klasör oluşturma, veri dosyalarını karşıya yükleme ve indirme, hesabınızı silme gibi temel işlemleri gerçekleştirmek için Azure komut satırı arabiriminin nasıl kullanılacağını öğrenin. Data Lake Store hakkında daha fazla bilgi için bkz. [Data Lake Store'a Genel Bakış](data-lake-store-overview.md).

Azure CLI, Node.js içinde uygulanmıştır. Windows, Mac ve Linux da dahil olmak üzere, Node.js'yi destekleyen herhangi bir platformda kullanılabilir. Azure CLI açık kaynaktır. Kaynak kodu, GitHub üzerinde <a href= "https://github.com/azure/azure-xplat-cli">https://github.com/azure/azure-xplat-cli</a> adresinde yönetilir. Bu makalede yalnızca Azure CLI'nın Data Lake Store ile kullanımı ele alınmaktadır. Azure CLI'nın kullanımı hakkında genel bir kılavuz için bkz. [Azure CLI'yı kullanma][azure-command-line-tools].

## <a name="prerequisites"></a>Ön koşullar
Bu makaleye başlamadan önce aşağıdakilere sahip olmanız ve aşağıdaki işlemleri yapmış olmanız gerekir:

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
* **Azure CLI** - Yükleme ve yapılandırma bilgileri için bkz. [Azure CLI'yı yükleme ve yapılandırma](../xplat-cli-install.md). CLI'yı yükledikten sonra bilgisayarınızı yeniden başlattığınızdan emin olun.

## <a name="authentication"></a>Kimlik Doğrulaması
Bu makalede Data Lake Store için son kullanıcı olarak oturum açtığınız daha basit bir kimlik doğrulama yaklaşımı kullanılmaktadır. Data Lake Store hesabına ve dosya sistemine erişim düzeyi bu durumda oturum açmış kullanıcının erişim düzeyine göre yönetilir. Ancak, Data Lake Store kimlik doğrulaması için **son kullanıcı kimlik doğrulaması** veya **hizmetten hizmete kimlik doğrulama** şeklinde diğer yaklaşımlar da mevcuttur. Kimlik doğrulaması hakkında yönergeler ve daha fazla bilgi için bkz. [Azure Active Directory kullanarak Data Lake Store kimlik doğrulaması yapma](data-lake-store-authenticate-using-active-directory.md).

## <a name="login-to-your-azure-subscription"></a>Azure aboneliğinizde oturum açma
1. [Azure Komut Satırı Arabirimi'nden (Azure CLI) bir Azure aboneliğine bağlanma](../xplat-cli-connect.md) konusunda belgelenen adımları izleyin ve `azure login` yöntemini kullanarak aboneliğinze bağlanın.
2. `azure account list` komutunu kullanarak hesabınızla ilişkili abonelikleri listeleyin.
   
        info:    Executing command account list
        data:    Name              Id                                    Current
        data:    ----------------  ------------------------------------  -------
        data:    Azure-sub-1       ####################################  true
        data:    Azure-sub-2       ####################################  false
   
    Yukarıdaki çıktıda **Azure-sub-1** şu anda etkindir ve diğer abonelik **Azure-sub-2**’dir. 
3. Altında çalışmak isteğiniz aboneliği seçin. Azure-sub-2 aboneliği altında çalışmak istiyorsanız `azure account set` komutunu kullanın.
   
        azure account set Azure-sub-2

## <a name="create-an-azure-data-lake-store-account"></a>Azure Data Lake Store hesabı oluşturma
Bir komut istemi, kabuk veya terminal oturumu açın ve aşağıdaki komutları çalıştırın.

1. Şu komutu kullanarak Azure Resource Manager moduna geçin:
   
        azure config mode arm
2. Yeni bir kaynak grubu oluşturun. Aşağıdaki komut içinde kullanmak istediğiniz parametre değerlerini sağlayın.
   
        azure group create <resourceGroup> <location>
   
    Konum adı boşluk içeriyorsa adı tırnak işaretleri içine alın. Örneğin, "Doğu ABD 2".
3. Data Lake Store hesabını oluşturun.
   
        azure datalake store account create <dataLakeStoreAccountName> <location> <resourceGroup>

## <a name="create-folders-in-your-data-lake-store"></a>Data Lake Store ürününüzde klasör oluşturma
Veri depolamak ve yönetmek için Azure Data Lake Store hesabınızın altında klasör oluşturabilirsiniz. Data Lake Store'un kökünde "mynewfolder" adlı bir klasör oluşturmak için aşağıdaki komutu kullanın.

    azure datalake store filesystem create <dataLakeStoreAccountName> <path> --folder

Örnek:

    azure datalake store filesystem create mynewdatalakestore /mynewfolder --folder

## <a name="upload-data-to-your-data-lake-store"></a>Data Lake Store ürününüze veri yükleme
Verilerinizi Data Lake Store'a doğrudan kök düzeyinde veya hesap içinde oluşturduğunuz bir klasöre yüklenecek şekilde yükleyebilirsiniz. Aşağıdaki kod parçacıkları, birtakım örnek verilerin önceki bölümde oluşturduğunuz klasöre (**mynewfolder**) nasıl yükleneceğini göstermektedir.

Karşıya yüklenecek örnek veri arıyorsanız [Azure Data Lake Git Deposu](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData)'ndan **Ambulance Data** klasörünü alabilirsiniz. Dosyayı indirin ve bilgisayarınız üzerinde C:\sampledata\. gibi yerel bir dizinde depolayın

    azure datalake store filesystem import <dataLakeStoreAccountName> "<source path>" "<destination path>"

Örneğin:

    azure datalake store filesystem import mynewdatalakestore "C:\SampleData\AmbulanceData\vehicle1_09142014.csv" "/mynewfolder/vehicle1_09142014.csv"


## <a name="list-files-in-data-lake-store"></a>Data Lake Store'da dosyaları listeleme
Bir Data Lake Store hesabındaki dosyaları listelemek için aşağıdaki komutu kullanın.

    azure datalake store filesystem list <dataLakeStoreAccountName> <path>

Örnek:

    azure datalake store filesystem list mynewdatalakestore /mynewfolder

Bunun çıktısının aşağıdakine benzer olması gerekir:

    info:    Executing command datalake store filesystem list
    data:    accessTime: 1446245025257
    data:    blockSize: 268435456
    data:    group: NotSupportYet
    data:    length: 1589881
    data:    modificationTime: 1446245105763
    data:    owner: NotSupportYet
    data:    pathSuffix: vehicle1_09142014.csv
    data:    permission: 777
    data:    replication: 0
    data:    type: FILE
    data:    ------------------------------------------------------------------------------------
    info:    datalake store filesystem list command OK

## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a>Data Lake Store'unuzda verileri yeniden adlandırma, indirme ve silme
* **Bir dosyayı yeniden adlandırmak için** aşağıdaki komutu kullanın:
  
        azure datalake store filesystem move <dataLakeStoreAccountName> <path/old_file_name> <path/new_file_name>
  
    Örnek:
  
        azure datalake store filesystem move mynewdatalakestore /mynewfolder/vehicle1_09142014.csv /mynewfolder/vehicle1_09142014_copy.csv
* **Bir dosyayı indirmek için** aşağıdaki komutu kullanın. Belirttiğiniz hedef yolun önceden var olduğundan emin olun.
  
        azure datalake store filesystem export <dataLakeStoreAccountName> <source_path> <destination_path>
  
    Örnek:
  
        azure datalake store filesystem export mynewdatalakestore /mynewfolder/vehicle1_09142014_copy.csv "C:\mysampledata\vehicle1_09142014_copy.csv"
* **Bir dosyayı silmek için** aşağıdaki komutu kullanın:
  
        azure datalake store filesystem delete <dataLakeStoreAccountName> <path>
  
    Örnek:
  
        azure datalake store filesystem delete mynewdatalakestore /mynewfolder/vehicle1_09142014_copy.csv
  
    İstendiğinde, öğeyi silmek için **Y** yazın.

## <a name="view-the-access-control-list-for-a-folder-in-data-lake-store"></a>Data Lake Store'daki bir klasör için erişim denetimi listesini görüntüleme
Bir Data Lake Store klasörü üzerindeki ACL'leri görüntülemek için aşağıdaki komutu kullanın. Geçerli sürümde, ACL'ler yalnızca Data Lake Store'un kökünde ayarlanabilir. Bu nedenle, yol parametresi her zaman kök (/) olur.

    azure datalake store permissions show <dataLakeStoreName> <path>

Örnek:

    azure datalake store permissions show mynewdatalakestore /


## <a name="delete-your-data-lake-store-account"></a>Data Lake Store hesabınızı silme
Bir Data Lake Store hesabını silmek için aşağıdaki komutu kullanın.

    azure datalake store account delete <dataLakeStoreAccountName>

Örnek:

    azure datalake store account delete mynewdatalakestore

İstendiğinde, hesabı silmek için **Y** yazın.

## <a name="next-steps"></a>Sonraki adımlar
* [Data Lake Store'da verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics'i Data Lake Store ile kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight'ı Data Lake Store ile kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)

[azure-command-line-tools]: ../xplat-cli-install.md



<!--HONumber=Nov16_HO4-->


