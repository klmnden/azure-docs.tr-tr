<properties
   pageTitle="Platformlar arası komut satırı arabirimini kullanarak Data Lake Store ile çalışmaya başlama | Microsoft Azure"
   description="Bir Data Lake Store hesabı oluşturmak ve temel işlemleri gerçekleştirmek için Azure platformlar arası komut satırını kullanma"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="paulettm"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="04/07/2016"
   ms.author="nitinme"/>

# Azure Komut Satırı'nı kullanarak Azure Data Lake Store ile çalışmaya başlama

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Azure Data Lake Store hesabı oluşturmak ve klasör oluşturma, veri dosyalarını karşıya yükleme ve indirme, hesabınızı silme gibi temel işlemleri gerçekleştirmek için Azure komut satırı arabiriminin nasıl kullanılacağını öğrenin. Data Lake Store hakkında daha fazla bilgi için bkz. [Data Lake Store'a Genel Bakış](data-lake-store-overview.md).

Azure CLI, Node.js içinde uygulanmıştır. Windows, Mac ve Linux da dahil olmak üzere, Node.js'yi destekleyen herhangi bir platformda kullanılabilir. Azure CLI açık kaynaktır. Kaynak kodu, GitHub üzerinde <a href= "https://github.com/azure/azure-xplat-cli">https://github.com/azure/azure-xplat-cli</a> adresinde yönetilir. Bu makalede yalnızca Azure CLI'nın Data Lake Store ile kullanımı ele alınmaktadır. Azure CLI'nın kullanımı hakkında genel bir kılavuz için bkz. [Azure CLI'yı kullanma] [azure-command-line-tools].


##Önkoşullar

Bu makaleye başlamadan önce aşağıdakilere sahip olmanız ve aşağıdaki işlemleri yapmış olmanız gerekir:

- **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
- Data Lake Store genel önizlemesi için **Azure aboneliğinizi etkinleştirme**. Bkz. [yönergeler](data-lake-store-get-started-portal.md#signup).
- **Azure CLI** - Yükleme ve yapılandırma bilgileri için bkz. [Azure CLI'yı yükleme ve yapılandırma](../xplat-cli-install.md). CLI'yı yükledikten sonra bilgisayarınızı yeniden başlattığınızdan emin olun.

##Azure aboneliğinizde oturum açma

[Azure Komut Satırı Arabirimi'nden (Azure CLI) bir Azure aboneliğine bağlanma](../xplat-cli-connect.md) konusunda belgelenen adımları izleyin ve __oturum açma__ yöntemini kullanarak aboneliğinze bağlanın.


## Azure Data Lake Store hesabı oluşturma

Bir komut istemi, kabuk veya terminal oturumu açın ve aşağıdaki komutları çalıştırın.

1. Azure aboneliğinizde oturum açın:

        azure login

    Bir web sayfası açmanız ve kimlik doğrulaması kodu girmeniz istenir. Azure aboneliğinizde oturum açmak için sayfadaki yönergeleri izleyin.

2. Şu komutu kullanarak Azure Resource Manager moduna geçin:

        azure config mode arm


3. Hesabınıza yönelik Azure aboneliklerini listeleyin.

        azure account list


4. Birden çok Azure aboneliğiniz varsa Azure CLI komutlarının kullanacağı aboneliği ayarlamak için şu komutu kullanın:

        azure account set <subscriptionname>

5. Yeni bir kaynak grubu oluşturun. Aşağıdaki komut içinde kullanmak istediğiniz parametre değerlerini sağlayın.

        azure group create <resourceGroup> <location>

    Konum adı boşluk içeriyorsa adı tırnak işaretleri içine alın. Örneğin, "Doğu ABD 2".

5. Data Lake Store hesabını oluşturun.

        azure datalake store account create <dataLakeStoreAccountName> <location> <resourceGroup>

## Data Lake Store ürününüzde klasör oluşturma

Veri depolamak ve yönetmek için Azure Data Lake Store hesabınızın altında klasör oluşturabilirsiniz. Data Lake Store'un kökünde "mynewfolder" adlı bir klasör oluşturmak için aşağıdaki komutu kullanın.

    azure datalake store filesystem create <dataLakeStoreAccountName> <path> --folder

Örnek:

    azure datalake store filesystem create mynewdatalakestore /mynewfolder --folder

## Data Lake Store ürününüze veri yükleme

Verilerinizi Data Lake Store'a doğrudan kök düzeyinde veya hesap içinde oluşturduğunuz bir klasöre yüklenecek şekilde yükleyebilirsiniz. Aşağıdaki kod parçacıkları, birtakım örnek verilerin önceki bölümde oluşturduğunuz klasöre (**mynewfolder**) nasıl yükleneceğini göstermektedir.

Karşıya yüklenecek örnek veri arıyorsanız [Azure Data Lake Git Deposu](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData)'ndan **Ambulance Data** klasörünü alabilirsiniz. Dosyayı indirin ve bilgisayarınız üzerinde C:\sampledata gibi yerel bir dizinde depolayın\.

    azure datalake store filesystem import <dataLakeStoreAccountName> "<source path>" "<destination path>"

Örnek:

    azure datalake store filesystem import mynewdatalakestore "C:\SampleData\AmbulanceData\vehicle1_09142014.csv" "/mynewfolder/vehicle1_09142014.csv"


## Data Lake Store'da dosyaları listeleme

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

## Data Lake Store'unuzda verileri yeniden adlandırma, indirme ve silme

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

## Data Lake Store'daki bir klasör için erişim denetimi listesini görüntüleme

Bir Data Lake Store klasörü üzerindeki ACL'leri görüntülemek için aşağıdaki komutu kullanın. Geçerli sürümde, ACL'ler yalnızca Data Lake Store'un kökünde ayarlanabilir. Bu nedenle, yol parametresi her zaman kök (/) olur.

    azure datalake store permissions show <dataLakeStoreName> <path>

Örnek:

    azure datalake store permissions show mynewdatalakestore /


## Data Lake Store hesabınızı silme

Bir Data Lake Store hesabını silmek için aşağıdaki komutu kullanın.

    azure datalake store account delete <dataLakeStoreAccountName>

Örnek:

    azure datalake store account delete mynewdatalakestore

İstendiğinde, hesabı silmek için **Y** yazın.


## Sonraki adımlar

- [Data Lake Store'da verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
- [Azure Data Lake Analytics'i Data Lake Store ile kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight'ı Data Lake Store ile kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)


[azure-command-line-tools]: ../xplat-cli-install.md



<!--HONumber=Jun16_HO2-->


