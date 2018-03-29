---
title: Hdınsight'ta - Azure R Server için Azure depolama çözümleri | Microsoft Docs
description: Hdınsight'ta R Server ile birlikte farklı depolama seçenekleri hakkında bilgi edinin
services: HDInsight
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 1cf30096-d3ca-45ea-b526-aa3954402f66
ms.service: HDInsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/22/2018
ms.author: nitinme
ms.openlocfilehash: f0333252e404c52f123da916df01d198234ef5af
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="azure-storage-solutions-for-r-server-on-azure-hdinsight"></a>Azure hdınsight'ta R Server için Azure depolama çözümleri

Hdınsight'ta R Server depolama çözümleri verileri, kod veya Analiz sonuçlarını içeren nesneleri kalıcı olması için çeşitli vardır. Bunlar, aşağıdaki seçenekleri içerir:

- [Azure Blob](https://azure.microsoft.com/services/storage/blobs/)
- [Azure Data Lake Storage](https://azure.microsoft.com/services/data-lake-store/)
- [Azure dosya depolama](https://azure.microsoft.com/services/storage/files/)

Ayrıca birden çok Azure depolama hesapları veya Hdınsight kümenize ile kapsayıcıları erişme seçeneğiniz vardır. Azure File storage sağlayan bir Azure depolama dosya paylaşımı bağlamak Örneğin, kenar düğümüne Linux dosya sistemi üzerinde kullanım için uygun veri depolama seçeneği ' dir. Ancak Azure dosya paylaşımları bağlanır ve Windows veya Linux gibi desteklenen bir işletim sistemine sahip tüm sistemler tarafından kullanılan. 

Hdınsight'ta Hadoop kümesi oluşturduğunuzda ya da belirttiğiniz bir **Azure depolama** hesabı veya bir **Data Lake deposu**. Bu hesaptan belirli depolama kapsayıcısı dosya sistemi (örneğin, Hadoop dağıtılmış dosya sistemi) oluşturmak küme tutar. Daha fazla bilgi ve yönergeler için bkz:

- [Hdınsight ile Azure depolama kullanma](../hdinsight-hadoop-use-blob-storage.md)
- [Azure Hdınsight kümeleri ile kullanım Data Lake Store](../hdinsight-hadoop-use-data-lake-store.md)

## <a name="use-azure-blob-storage-accounts-with-r-server-cluster"></a>R Server kümesi ile Azure Blob storage hesapları kullanın

R Server kümenizi oluştururken birden fazla depolama hesabı belirtilmişse, aşağıdaki yönergeleri veri erişimi ve R sunucu kümesi işlemleri için ikincil bir hesabı kullanacak şekilde açıklanmaktadır. Aşağıdaki depolama hesapları ve kapsayıcı varsayalım: **storage1** ve varsayılan kapsayıcı adlı **container1**, ve **storage2** ile **container2**.

> [!WARNING]
> Performans nedeniyle, belirttiğiniz birincil depolama hesabıyla aynı veri merkezinde Hdınsight kümesi oluşturulur. Hdınsight kümesi farklı bir konumda bir depolama hesabıyla desteklenmiyor.

### <a name="use-the-default-storage-with-r-server-on-hdinsight"></a>Varsayılan depolama hdınsight'ta R Server ile kullanma

1. Bir SSH İstemcisi'ni kullanarak kümenizi kenar düğümüne bağlanma. Hdınsight kümeleri ile SSH kullanma hakkında daha fazla bilgi için bkz: [Hdınsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).
  
2. Bir örnek dosyası mysamplefile.csv, /share dizinine kopyalayın. 

        hadoop fs –mkdir /share
        hadoop fs –copyFromLocal mycsv.scv /share  

3. Geçiş R Studio veya başka bir R konsol ve R kodu adı düğümü ayarlamak için yazma **varsayılan** ve erişmek istediğiniz dosyanın konumu.  

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

Depolama hesabı için tüm dizin ve dosya başvuruları noktası `wasb://container1@storage1.blob.core.windows.net`. Bu **varsayılan depolama hesabı** Hdınsight kümesi ile ilişkili.

### <a name="use-the-additional-storage-with-r-server-on-hdinsight"></a>Hdınsight'ta R Server ile ek depolama alanı kullanın

Şimdi, /private bulunan mysamplefile1.csv adlı bir dosya işlemek istediğiniz varsayalım dizininde **container2** içinde **storage2**.

R kodunuzda adı düğümü referansı noktası **storage2** depolama hesabı.

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

Tüm dizin ve dosya başvurularını şimdi noktası depolama hesabına `wasb://container2@storage2.blob.core.windows.net`. Bu **adı düğümü** belirttiğiniz.

/ User/RevoShare/yapılandırmak zorunda<SSH username> dizininde **storage2** gibi:


    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user
    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user/RevoShare
    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user/RevoShare/<RDP username>

## <a name="use-an-azure-data-lake-store-with-r-server-cluster"></a>Bir Azure Data Lake Store ile R Server kümesi kullanın 

Data Lake Store Hdınsight kümenizle kullanmak için her Azure Data Lake kullanmak istediğiniz deposu için küme erişim vermeniz gerekir. Varsayılan depolama veya ek bir deposu olarak Azure Data Lake Store hesabı olan bir Hdınsight kümesi oluşturmak için Azure portalını kullanma hakkında daha fazla yönerge için bkz: [Azure portalını kullanarak Data Lake Store ile bir Hdınsight kümesi oluşturmayı](../../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

Önceki yordamda açıklandığı şekilde ikincil Azure depolama hesabı olduğu gibi ardından deposu R komut dosyanıza çok kullanın.

### <a name="add-cluster-access-to-your-azure-data-lake-stores"></a>Azure Data Lake mağazalarının küme erişim Ekle
Hdınsight kümenizle ilişkili bir Azure Active Directory (Azure AD) hizmet sorumlusu kullanarak Data Lake deposu erişin.

1. Hdınsight kümenize oluşturduğunuzda seçmeniz **kümeye özgü AAD kimliği** gelen **veri kaynağı** sekmesi.

2. İçinde **kümeye özgü AAD kimliği** iletişim kutusunda **AD hizmet sorumlusu seçin**seçin **Yeni Oluştur**.

Hizmet sorumlusu bir ad verin ve parola oluşturulduktan sonra tıklayın **ADLS erişimini yönetme** hizmet sorumlusu Data Lake Store ile ilişkilendirilecek.

Küme oluşturma aşağıdaki bir veya daha fazla veri Gölü deposu hesaplarına küme erişim eklemek mümkündür. Bir Data Lake Store için Azure portal giriş açın ve gidin **Veri Gezgini > erişim > Ekle**. 

### <a name="how-to-access-the-data-lake-store-from-r-server-on-hdinsight"></a>Data Lake deposu hdınsight'ta R Server erişmek nasıl

Bir Data Lake Store'a erişim verilen sonra ikincil Azure depolama hesabı yaptığınız şekilde Hdınsight'ta R Server küme deposunda kullanabilirsiniz. Tek fark, önektir **wasb: / /** değişikliklerini **adl: / /** gibi:


    # Point to the ADL store (e.g. ADLtest)
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

Aşağıdaki komutlar, Data Lake Store hesabı RevoShare dizini ile yapılandırın ve önceki örnekten örnek .csv dosyası eklemek için kullanılır:


    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user
    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare
    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare/<user>

    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/share

    hadoop fs -copyFromLocal /usr/lib64/R Server-7.4.1/library/RevoScaleR/SampleData/mysamplefile.csv adl://rkadl1.azuredatalakestore.net/share

    hadoop fs –ls adl://rkadl1.azuredatalakestore.net/share


## <a name="use-azure-file-storage-with-r-server-on-hdinsight"></a>Hdınsight'ta R Server ile Azure File storage'ı kullanma

[Azure dosyaları] adlı edge düğüm üzerinde kullanım için uygun veri depolama seçeneği bulunmaktadır ((https://azure.microsoft.com/services/storage/files/). Bir Azure depolama alanı dosya paylaşımını Linux dosya sistemine bağlama sağlar. Bu seçenek, veri dosyalarını, R betiklerini ve özellikle kenar düğümüne HDFS yerine yerel dosya sistemini kullanmak üzere mantıklıdır zaman gerekebilecek daha sonra sonuç nesneleri depolamak için kullanışlı olabilir. 

Azure dosyaları önemli bir avantajı dosya paylaşımları takılı ve Windows veya Linux gibi desteklenen bir işletim sistemi sahip tüm sistemler tarafından kullanılan olmasıdır. Örneğin, bunu sizin veya ekibiniz birisi sahip başka bir Hdınsight kümesi, bir Azure VM veya bile bir şirket içi sistem tarafından kullanılabilir. Daha fazla bilgi için bkz.

- [Azure Dosya depolamayı Linux ile kullanma](../../storage/files/storage-how-to-use-files-linux.md)
- [Windows Azure File storage kullanma](../../storage/files/storage-dotnet-how-to-use-files.md)


## <a name="next-steps"></a>Sonraki adımlar

* [Hdınsight R Server kümesine genel bakış](r-server-overview.md)
* [R Server kümesinde Hadoop kullanmaya başlama](r-server-get-started.md)
* [Hdınsight'ta R Server kümesi için içerik seçeneklerini işlem](r-server-compute-contexts.md)

