---
title: HDInsight - Azure ML Hizmetleri için Azure depolama çözümleri
description: HDInsight üzerinde ML Hizmetleri ile kullanılabilen farklı depolama seçenekleri hakkında bilgi edinin
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/27/2018
ms.openlocfilehash: cb0c350df3056636701b5ff5d3962e2a0e96f40d
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62124535"
---
# <a name="azure-storage-solutions-for-ml-services-on-azure-hdinsight"></a>Azure HDInsight üzerinde ML Hizmetleri için Azure depolama çözümleri

HDInsight üzerinde ML Hizmetleri, depolama çözümleri çeşitli veri, kod veya Analiz sonuçlarını içeren nesneleri kalıcı hale getirmek için kullanabilirsiniz. Bunlar, aşağıdaki seçenekleri içerir:

- [Azure Blob](https://azure.microsoft.com/services/storage/blobs/)
- [Azure Data Lake depolama](https://azure.microsoft.com/services/storage/data-lake-storage/)
- [Azure dosya depolama](https://azure.microsoft.com/services/storage/files/)

Ayrıca, birden çok Azure depolama hesabı veya HDInsight kümenizle kapsayıcıları erişme seçeneğiniz de vardır. Azure dosya depolama, uygun veri depolama seçeneğini kullanmak için bir Azure depolama alanı dosya paylaşımını bağlayabilmeniz, örneğin, sağlar kenar düğümündeki Linux dosya sistemi ' dir. Ancak, Azure dosya paylaşımlarını bağlanır ve Windows veya Linux gibi desteklenen bir işletim sistemine sahip tüm sistemler tarafından kullanılan. 

HDInsight Apache Hadoop kümesi oluşturduğunuzda, ya da belirttiğiniz bir **Azure depolama** hesabı veya **Data Lake Storage**. Bu hesabı belirli depolama kapsayıcısından (örneğin, Hadoop dağıtılmış dosya sistemi) oluşturduğunuz küme için dosya sistemini barındırır. Daha fazla bilgi ve yönergeler için bkz:

- [HDInsight ile Azure Depolama'yı kullanma](../hdinsight-hadoop-use-blob-storage.md)
- [Data Lake Storage Azure HDInsight kümeleri ile kullanma](../hdinsight-hadoop-use-data-lake-store.md)

## <a name="use-azure-blob-storage-accounts-with-ml-services-cluster"></a>Azure Blob Depolama hesapları ML Hizmetleri kümesi ile kullanma

ML Hizmetleri kümenizi oluştururken birden fazla depolama hesabı belirttiyseniz, aşağıdaki yönergeleri veri erişimi ve ML Hizmetleri küme işlemleri için ikincil bir hesabı kullanmayı açıklar. Aşağıdaki depolama hesapları ve kapsayıcı varsayılır: **storage1** ve varsayılan kapsayıcı olarak adlandırılan **kapsayıcı1**, ve **storage2** ile **container2**.

> [!WARNING]  
> Performans amacıyla, belirttiğiniz birincil depolama hesabı aynı veri merkezinde HDInsight kümesi oluşturulur. HDInsight kümesinden farklı bir konumda bir depolama hesabının kullanılması desteklenmez.

### <a name="use-the-default-storage-with-ml-services-on-hdinsight"></a>ML hizmetlerinde, HDInsight ile varsayılan depolama kullanma

1. Bir SSH istemcisi kullanarak kümenize kenar düğümüne bağlanın. HDInsight kümeleri ile SSH kullanma hakkında daha fazla bilgi için bkz: [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).
  
2. Bir örnek dosya, mysamplefile.csv /share dizinine kopyalayın. 

        hadoop fs –mkdir /share
        hadoop fs –copyFromLocal mycsv.scv /share  

3. R Studio ya da başka bir R konsolunu geçin ve düğüm adı ayarlamak için R kod yazma **varsayılan** ve erişmek istediğiniz dosyanın konumu.  

        myNameNode <- "default"
        myPort <- 0

        #Location of the data:  
        bigDataDirRoot <- "/share"  

        #Define Spark compute context:
        mySparkCluster <- RxSpark(nameNode=myNameNode, consoleOutput=TRUE)

        #Set compute context:
        rxSetComputeContext(mySparkCluster)

        #Define the Hadoop Distributed File System (HDFS) file system:
        hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

        #Specify the input file to analyze in HDFS:
        inputFile <-file.path(bigDataDirRoot,"mysamplefile.csv")

Tüm dizin ve dosya başvuruları depolama hesabına işaret `wasb://container1@storage1.blob.core.windows.net`. Bu **varsayılan depolama hesabı** HDInsight kümesi ile ilişkili.

### <a name="use-the-additional-storage-with-ml-services-on-hdinsight"></a>HDInsight üzerinde ML Hizmetleri ile ek depolama kullanma

Şimdi, /private içinde bulunan mysamplefile1.csv adlı bir dosya işlemek istediğiniz varsayalım dizininde **container2** içinde **storage2**.

R kodunuzu adı düğümü başvuru noktası **storage2** depolama hesabı.

    myNameNode <- "wasb://container2@storage2.blob.core.windows.net"
    myPort <- 0

    #Location of the data:
    bigDataDirRoot <- "/private"

    #Define Spark compute context:
    mySparkCluster <- RxSpark(consoleOutput=TRUE, nameNode=myNameNode, port=myPort)

    #Set compute context:
    rxSetComputeContext(mySparkCluster)

    #Define HDFS file system:
    hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

    #Specify the input file to analyze in HDFS:
    inputFile <-file.path(bigDataDirRoot,"mysamplefile1.csv")

Tüm dizin ve dosya başvuruları artık işaret depolama hesabına `wasb://container2@storage2.blob.core.windows.net`. Bu **adı düğümü** belirttiğiniz.

Yapılandırmak zorunda `/user/RevoShare/<SSH username>` dizininde **storage2** gibi:


    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user
    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user/RevoShare
    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user/RevoShare/<RDP username>

## <a name="use-azure-data-lake-storage-with-ml-services-cluster"></a>Azure Data Lake Storage ML Hizmetleri kümesi ile kullanma 

Data Lake Store ile HDInsight kümenizi kullanmak için her Azure Data Lake, kullanmak istediğiniz depolama için küme erişim vermeniz gerekir. Varsayılan depolama alanı olarak ya da ek depolama alanı olarak Azure Data Lake Storage hesabıyla bir HDInsight kümesi oluşturmak için Azure portalını kullanma hakkında yönergeler için bkz: [AzureportalınıkullanarakDataLakeStoreileHDInsightkümesioluşturma](../../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

İkincil Azure depolama hesabı önceki yordamda açıklandığı gibi yaptığınız gibi ardından depolama R betiğinizde çok kullanın.

### <a name="add-cluster-access-to-your-azure-data-lake-storage"></a>Azure Data Lake depolama kümesi erişimi Ekle
Data Lake depolama, HDInsight kümenizle ilişkili bir Azure Active Directory (Azure AD) hizmet sorumlusu kullanılarak erişim.

1. HDInsight kümenizi oluşturduğunuzda **küme AAD kimlik** gelen **veri kaynağı** sekmesi.

2. İçinde **küme AAD kimlik** iletişim kutusunun **AD hizmet sorumlusu seçin**seçin **Yeni Oluştur**.

Hizmet sorumlusu bir ad verin ve bunun için bir parola oluşturduktan sonra tıklayın **yönetme ADLS erişim** hizmet sorumlusu, Data Lake Storage ile ilişkilendirilecek.

Küme oluşturma aşağıdaki bir veya daha fazla Data Lake Storage hesaplarını için küme erişim eklemek mümkündür. Bir Data Lake Storage için Azure portal girişinde açın ve gidin **Veri Gezgini > erişim > Ekle**. 

### <a name="how-to-access-data-lake-storage-gen1-from-ml-services-on-hdinsight"></a>Data Lake depolama Gen1 HDInsight ML hizmetlerden erişme

Data Lake depolama Gen1 için erişim verilen sonra depolama ML Hizmetleri HDInsight kümesinde bir ikincil Azure depolama hesabınız olduğu gibi kullanabilirsiniz. Tek fark eden önekidir **wasb: / /** değişikliklerini **adl: / /** gibi:


    # Point to the ADL Storage (e.g. ADLtest)
    myNameNode <- "adl://rkadl1.azuredatalakestore.net"
    myPort <- 0

    # Location of the data (assumes a /share directory on the ADL account)
    bigDataDirRoot <- "/share"  

    # Define Spark compute context
    mySparkCluster <- RxSpark(consoleOutput=TRUE, nameNode=myNameNode, port=myPort)

    # Set compute context
    rxSetComputeContext(mySparkCluster)

    # Define HDFS file system
    hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

    # Specify the input file in HDFS to analyze
    inputFile <-file.path(bigDataDirRoot,"mysamplefile.csv")

Aşağıdaki komutları RevoShare dizini ile Data Lake depolama Gen1 hesabı yapılandırın ve önceki örnekte örnek .csv dosyası eklemek için kullanılır:


    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user
    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare
    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare/<user>

    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/share

    hadoop fs -copyFromLocal /usr/lib64/R Server-7.4.1/library/RevoScaleR/SampleData/mysamplefile.csv adl://rkadl1.azuredatalakestore.net/share

    hadoop fs –ls adl://rkadl1.azuredatalakestore.net/share


## <a name="use-azure-file-storage-with-ml-services-on-hdinsight"></a>HDInsight üzerinde ML Hizmetleri ile Azure File storage'ı kullanma

Adlı kenar düğümündeki uygun veri depolama seçeneğini kullanmak için de mevcuttur [Azure dosyaları](https://azure.microsoft.com/services/storage/files/). Bir Azure depolama alanı dosya paylaşımını Linux dosya sistemine olanak tanır. Bu seçenek, veri dosyalarını, R betikleri ve kenar düğümüne HDFS yerine yerel dosya sistemi kullanmak üzere mantıklı özellikle zaman gerekebilecek daha sonra sonuç nesneleri depolamak için kullanışlı olabilir. 

Azure dosyaları'nın önemli bir avantajı dosya paylaşımları bağlı Windows veya Linux gibi desteklenen bir işletim sistemine sahip tüm sistemler tarafından kullanılan olmasıdır. Örneğin, siz veya takımınızdaki birinin sahip başka bir HDInsight kümesi, bir Azure VM veya bile bir şirket içi sistem tarafından kullanılabilir. Daha fazla bilgi için bkz.

- [Azure Dosya depolamayı Linux ile kullanma](../../storage/files/storage-how-to-use-files-linux.md)
- [Windows üzerinde Azure dosya depolama kullanma](../../storage/files/storage-dotnet-how-to-use-files.md)


## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight kümesinde ML hizmetleri genel bakış](r-server-overview.md)
* [Apache Hadoop kümesinde ML hizmetleri kullanmaya başlayın](r-server-get-started.md)
* [HDInsight üzerinde ML Services kümesi için işlem bağlamı seçenekleri](r-server-compute-contexts.md)
* [Azure Data Lake depolama Gen2 Azure HDInsight kümeleri ile kullanma](../hdinsight-hadoop-use-data-lake-storage-gen2.md)
