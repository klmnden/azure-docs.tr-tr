<properties 
   pageTitle="Azure Komut Satırı Arabirimi'ni kullanarak Azure Data Lake Analytics ile çalışmaya başlama | Microsoft Azure" 
   description="Bir Data Lake Store hesabı oluşturmak, U-SQL'yi kullanarak Data Lake Analytics işi oluşturmak ve işi göndermek için Azure Komut Satırı Arabirimi'nin nasıl kullanılacağını öğrenin. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="paulettm" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="04/26/2016"
   ms.author="edmaca"/>

# Öğretici: Azure Komut Satırı Arabirimi'ni (CLI) kullanarak Azure Data Lake Analytics ile çalışmaya başlama

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


Azure Data Lake Analytics hesapları oluşturmak, Data Lake Analytics işlerini [U-SQL](data-lake-analytics-u-sql-get-started.md) içinde tanımlamak ve Data Lake Analytics hesaplarına iş göndermek için Azure CLI olanağının nasıl kullanılacağını öğrenin. Data Lake Analytics hakkında daha fazla bilgi için bkz. [Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md).

Bu öğreticide, bir sekmeyle ayrılmış değerler (TSV) dosyasını okuyan ve bunu virgülle ayrılmış değerler (CSV) dosyasına dönüştüren bir iş geliştireceksiniz. Öğreticiyi desteklenen diğer araçları kullanarak tamamlamak için bu bölümün üst kısmındaki sekmelere tıklayın.

[AZURE.INCLUDE [basic-process-include](../../includes/data-lake-analytics-basic-process.md)]

##Önkoşullar

Bu öğreticiye başlamadan önce aşağıdakilere sahip olmanız gerekir:

- **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
- **Azure CLI**. Bkz. [Azure CLI'yı yükleme ve yapılandırma](../xplat-cli-install.md).
    - Bu demoyu tamamlamak için **yayın öncesi sürüm** [Azure CLI araçlarını](https://github.com/MicrosoftBigData/AzureDataLake/releases) indirip yükleyin.
- Şu komutu kullanarak **kimlik doğrulama**:

        azure login
    Bir iş veya okul hesabı kullanarak kimlik doğrulama gerçekleştirme konusunda daha fazla bilgi için bkz. [Azure CLI'dan Azure aboneliğine bağlanma](../xplat-cli-connect.md).
- Şu komutu kullanarak **Azure Resource Manager moduna geçin**:

        azure config mode arm
        
## Data Lake Analytics hesabı oluşturma

Herhangi bir işi çalıştırmadan önce bir Data Lake Analytics hesabına sahip olmanız gerekir. Bir Data Lake Analytics hesabı oluşturmak için aşağıdakileri belirtmeniz gerekir:

- **Azure Kaynak Grubu**: Data Lake Analytics hesabı bir Azure Kaynak grubu içinde oluşturulmalıdır. [Azure Resource Manager](../resource-group-overview.md), uygulamanızdaki kaynaklarla bir grup olarak çalışmanıza olanak sağlar. Uygulamanıza yönelik tüm kaynakları tek ve eş güdümlü bir işlemle dağıtabilir, güncelleştirebilir veya silebilirsiniz.  

    Aboneliğinizdeki kaynak gruplarını numaralandırmak için:
    
        azure group list 
    
    Yeni bir kaynak grubu oluşturmak için:

        azure group create -n "<Resource Group Name>" -l "<Azure Location>"

- **Data Lake Analytics hesap adı**
- **Konum**: Data Lake Analytics'i destekleyen Azure veri merkezlerinden biri.
- **Varsayılan Data Lake hesabı**: Her Data Lake Analytics hesabı, bir varsayılan Data Lake hesabına sahiptir.

    Var olan bir Data Lake hesabını listelemek için:
    
        azure datalake store account list

    Yeni bir Data Lake hesabı oluşturmak için:

        azure datalake store account create "<Data Lake Store Account Name>" "<Azure Location>" "<Resource Group Name>"

    > [AZURE.NOTE] Data Lake hesap adı yalnızca küçük harflerden ve rakamlardan oluşmalıdır.



**Data Lake Analytics hesabı oluşturmak için**

        azure datalake analytics account create "<Data Lake Analytics Account Name>" "<Azure Location>" "<Resource Group Name>" "<Default Data Lake Account Name>"

        azure datalake analytics account list
        azure datalake analytics account show "<Data Lake Analytics Account Name>"          

![Data Lake Analytics hesabı gösterme](./media/data-lake-analytics-get-started-cli/data-lake-analytics-show-account-cli.png)

> [AZURE.NOTE] Data Lake Analytics hesap adı yalnızca küçük harflerden ve rakamlardan oluşmalıdır.


## Data Lake Store'a veri yükleme

Bu öğreticide, bazı arama günlüklerini işleyeceksiniz.  Arama günlüğü, Data Lake Store veya Azure Blob depolama alanında depolanabilir. 

Azure Portal, bir arama günlüğü dosyası içeren bazı örnek veri dosyalarını varsayılan Data Lake hesabına kopyalamak için bir kullanıcı arabirimi sağlar. Verileri varsayılan Data Lake Store hesabına yüklemek için bkz. [Kaynak verileri hazırlama](data-lake-analytics-get-started-portal.md#prepare-source-data).

Dosyaları cli özelliğini kullanarak yüklemek için şu komutu kullanın:

    azure datalake store filesystem import "<Data Lake Store Account Name>" "<Path>" "<Destination>"
    azure datalake store filesystem list "<Data Lake Store Account Name>" "<Path>"

Data Lake Analytics ayrıca Azure Blob depolama alanına da erişebilir.  Verileri Azure Blob depolama alanına yüklemek için bkz. [Azure CLI'yı Azure Storage ile kullanma](../storage/storage-azure-cli.md).

## Data Lake Analytics işlerini gönderme

Data Lake Analytics işleri, U-SQL dilinde yazılır. U-SQL hakkında daha fazla bilgi için bkz. [U-SQL dili ile çalışmaya başlama](data-lake-analytics-u-sql-get-started.md) ve [U-SQL dili başvurusu](http://go.microsoft.com/fwlink/?LinkId=691348).

**Bir Data Lake Analytics işi betiği oluşturmak için**

- Aşağıdaki U-SQL betiği ile bir metin dosyası oluşturun ve metin dosyasını iş istasyonunuza kaydedin:

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv"
            USING Extractors.Tsv();
        
        OUTPUT @searchlog   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    Bu U-SQL betiği, **Extractors.Tsv()** öğesini kullanarak kaynak veri dosyasını okur ve ardından **Outputters.Csv()** öğesini kullanarak bir csv dosyası oluşturur. 
    
    Kaynak dosyayı farklı bir konuma kopyalamadıkça bu iki yolu değiştirmeyin.  Data Lake Analytics, mevcut olmaması halinde çıkış klasörünü oluşturur.
    
    Varsayılan Data Lake hesaplarında depolanan dosyalar için göreli yolların kullanılması daha basittir. Mutlak yol da kullanabilirsiniz.  Örneğin: 
    
        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
        
    Mutlak yolları, bağlı Storagehesaplarındaki dosyalara erişmek için kullanmanız gerekir.  Bağlı Azure Storage hesabında depolanan dosyalar için söz dizimi şu şekildedir:
    
        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Ortak blob veya ortak kapsayıcı erişim izinlerine sahip Azure Blob kapsayıcısı şu an için desteklenmemektedir.      

    
**İşi göndermek için**


    azure datalake analytics job create  "<Data Lake Analytics Account Name>" "<Job Name>" "<Script>"
    
    
Aşağıdaki komutlar, işleri listelemek, iş ayrıntılarını almak ve işleri iptal etmek için kullanılabilir:

    azure datalake analytics job cancel "<Data Lake Analytics Account Name>" "<Job Id>"
    azure datalake analytics job list "<Data Lake Analytics Account Name>"
    azure datalake analytics job show "<Data Lake Analytics Account Name>" "<Job Id>"

İş tamamlandıktan sonra, dosyayı listelemek için aşağıdaki cmdlet'leri kullanabilir ve dosyayı indirebilirsiniz:
    
    azure datalake store filesystem list "<Data Lake Store Account Name>" "/Output"
    azure datalake store filesystem export "<Data Lake Store Account Name>" "/Output/SearchLog-from-Data-Lake.csv" "<Destination>"
    azure datalake store filesystem read "<Data Lake Store Account Name>" "/Output/SearchLog-from-Data-Lake.csv" <Length> <Offset>

## Ayrıca bkz.

- Aynı öğreticiyi diğer araçları kullanarak görmek için sayfanın üst kısmındaki sekme seçicilerine tıklayın.
- Daha karmaşık bir sorgu görmek için bkz. [Azure Data Lake Analytics'i kullanarak Web sitesi günlüklerini çözümleme](data-lake-analytics-analyze-weblogs.md).
- U-SQL uygulamalarını geliştirmeye başlamak için bkz. [Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md).
- U-SQL öğrenmek için bkz. [Azure Data Lake Analytics U-SQL dili ile çalışmaya başlama](data-lake-analytics-u-sql-get-started.md).
- Yönetim görevleri için bkz. [Azure Portal'ı kullanarak Azure Data Lake Analytics'i yönetme](data-lake-analytics-manage-use-portal.md).
- Data Lake Analytics'e yönelik bir genel bakış için bkz. [Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md).




<!----HONumber=Jun16_HO2-->


