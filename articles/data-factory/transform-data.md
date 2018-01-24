---
title: "Azure Data Factory kullanarak veri dönüştürme | Microsoft Docs"
description: "Veri ya da Hadoop, Machine Learning veya Azure Data Lake Analytics kullanarak Azure Data Factory işlem verilerde dönüştürme öğrenin."
services: data-factory
documentationcenter: 
author: shengcmsft
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: shengc
ms.openlocfilehash: de9b054a9ef7d6efc64049059f581741eef39035
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="transform-data-in-azure-data-factory"></a>Azure Data Factory veri dönüştürme
> [!div class="op_single_selector"]
> * [Hive](transform-data-using-hadoop-hive.md)  
> * [Pig](transform-data-using-hadoop-pig.md)  
> * [MapReduce](transform-data-using-hadoop-map-reduce.md)  
> * [Hadoop Akışı](transform-data-using-hadoop-streaming.md)
> * [Spark](transform-data-using-spark.md)
> * [Machine Learning](transform-data-using-machine-learning.md) 
> * [Saklı Yordam](transform-data-using-stored-procedure.md)
> * [Data Lake Analytics U-SQL](transform-data-using-data-lake-analytics.md)
> * [.NET özel](transform-data-using-dotnet-custom-activity.md)

## <a name="overview"></a>Genel Bakış
Bu makalede Azure Data factory'de dönüştürmek için kullanabilir ve ham verilerinizi Öngörüler ve öngörü işler veri dönüştürme etkinlikleri açıklar. Azure Hdınsight kümesini veya bir Azure Batch gibi bilgi işlem ortamında dönüştürme etkinliğine yürütür. Makalelerinin bağlantıları her dönüştürme etkinliği hakkında ayrıntılı bilgi sağlar.

Veri Fabrikası destekleyen eklenebilir aşağıdaki veri dönüştürme etkinlikleri [ardışık düzen](concepts-pipelines-activities.md) ya da ayrı ayrı veya başka bir etkinlikle zincirleme.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [dönüştürme etkinlikleri sürüm 1 veri fabrikasında](v1/data-factory-data-transformation-activities.md).
 

## <a name="hdinsight-hive-activity"></a>Hdınsight Hive etkinliği
Data Factory işlem hattı Hdınsight Hive etkinliğinde, Hive sorguları kendi veya isteğe bağlı Windows/Linux tabanlı Hdınsight kümesi yürütür. Bkz: [Hive etkinliğini](transform-data-using-hadoop-hive.md) bu etkinliği hakkında ayrıntılı bilgi için makalenin. 

## <a name="hdinsight-pig-activity"></a>Hdınsight Pig etkinliği
Data Factory işlem hattı Hdınsight Pig etkinliğinde kendi Pig sorgular veya isteğe bağlı Windows/Linux tabanlı Hdınsight kümesi yürütür. Bkz: [Pig etkinlik](transform-data-using-hadoop-pig.md) bu etkinliği hakkında ayrıntılı bilgi için makalenin. 

## <a name="hdinsight-mapreduce-activity"></a>Hdınsight MapReduce etkinliği
Data Factory işlem hattı Hdınsight MapReduce etkinliğinde kendi MapReduce programları veya isteğe bağlı Windows/Linux tabanlı Hdınsight kümesi yürütür. Bkz: [MapReduce etkinliği](transform-data-using-hadoop-map-reduce.md) bu etkinliği hakkında ayrıntılı bilgi için makalenin.

## <a name="hdinsight-streaming-activity"></a>Hdınsight akış etkinliği
Data Factory işlem hattı Hdınsight akış etkinliğinde, Hadoop akış programlar kendi veya isteğe bağlı Windows/Linux tabanlı Hdınsight kümesi yürütür. Bkz: [Hdınsight akış etkinliği](transform-data-using-hadoop-streaming.md) bu etkinliği hakkında ayrıntılı bilgi için.

## <a name="hdinsight-spark-activity"></a>Hdınsight Spark etkinliği
Data Factory işlem hattı Hdınsight Spark etkinliğinde Spark programlar kendi Hdınsight kümesinde yürütür. Ayrıntılar için bkz [Azure Data Factory çağırma Spark programlardan](transform-data-using-spark.md). 

## <a name="machine-learning-activities"></a>Machine Learning etkinlikleri
Azure Data Factory, Tahmine dayalı analiz için yayımlanan bir Azure Machine Learning web hizmetini kullan ardışık düzen kolayca oluşturmanıza olanak sağlar. Kullanarak [toplu iş yürütme etkinliği](transform-data-using-machine-learning.md) bir Azure Data Factory işlem hattı verileri toplu tahminlerde için Machine Learning web hizmeti çağırabilirsiniz.

Zaman içinde denemeler Puanlama Machine Learning Tahmine dayalı modelleri yeni giriş veri kümeleri kullanarak retrained gerekir. Yeniden eğitme ile tamamladıktan sonra ile retrained Machine Learning modeli Puanlama web hizmetini güncelleştirmek istiyor. Kullanabileceğiniz [kaynak güncelleştirme etkinliği](update-machine-learning-models.md) web hizmeti ile yeni eğitilen modeli güncelleştirmek için.  

Bkz: [kullanım Machine Learning etkinlikleri](transform-data-using-machine-learning.md) bu Machine Learning etkinlikler hakkında ayrıntılı bilgi için. 

## <a name="stored-procedure-activity"></a>Depolanan yordam etkinliği
Aşağıdaki veri depolarına birinde bir saklı yordam çağrılacak bir Data Factory işlem hattı ve SQL Server saklı yordam etkinliğinin kullanabilirsiniz: Azure SQL Database, Azure SQL Data Warehouse, SQL Server veritabanı kuruluşunuzdaki veya bir Azure VM. Bkz: [saklı yordam etkinliği](transform-data-using-stored-procedure.md) Ayrıntılar için makale.  

## <a name="data-lake-analytics-u-sql-activity"></a>Data Lake Analytics U-SQL etkinliği
Data Lake Analytics U-SQL etkinliği Azure Data Lake Analytics kümede bir U-SQL komut dosyasını çalıştırır. Bkz: [veri analizi U-SQL etkinliği](transform-data-using-data-lake-analytics.md) Ayrıntılar için makale. 

## <a name="net-custom-activity"></a>.NET özel etkinliği
Veri fabrikası tarafından desteklenmeyen bir şekilde veri dönüştürme ihtiyacınız varsa, kendi veri işleme mantığı ile özel bir etkinlik oluşturmak ve ardışık düzeninde etkinlik kullanın. Bir Azure Batch hizmeti ya da Azure Hdınsight kümesi kullanarak çalıştırmak için özel .NET etkinliği yapılandırabilirsiniz. Bkz: [özel etkinlikleri kullanmak](transform-data-using-dotnet-custom-activity.md) Ayrıntılar için makale. 

R yüklü HDInsight kümenizde R betiklerini çalıştırmak için özel bir etkinlik oluşturabilirsiniz. Bkz. [Azure Data Factory kullanarak R Betiği çalıştırma](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample). 

## <a name="compute-environments"></a>Ortamlar işlem
Bilgi işlem ortamı için bağlı hizmet oluşturup ardından bağlantılı hizmet dönüştürme etkinliğine tanımlarken kullanabilirsiniz. Bilgi işlem ortamları Data Factory ile desteklenen iki tür vardır. 

- **İsteğe bağlı**: Bu durumda, bilgi işlem ortamı Data Factory ile tam olarak yönetilir. Bir işi veri işlemek için gönderildi ve iş tamamlandığında, kaldırılan önce Data Factory hizmeti tarafından otomatik olarak oluşturulur. Yapılandırma ve iş yürütme, küme yönetimi ve eylemler önyükleme için isteğe bağlı işlem ortamının ayrıntılı ayarlarını denetleyebilirsiniz. 
- **Kendi bilgisayarınızı getirin**: Bu durumda, veri fabrikasında bağlı hizmet olarak kendi bilgi işlem ortamı (örneğin, Hdınsight kümesi) kaydedebilirsiniz. Bilgi işlem ortamı sizin tarafınızdan yönetilen ve Data Factory hizmetinin etkinlikleri yürütmek için kullanır. 

Bkz: [işlem bağlı Hizmetleri](compute-linked-services.md) makale Data Factory ile desteklenen işlem hizmetleri hakkında bilgi edinin. 

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki öğretici dönüştürme etkinliğine kullanma örneği için bkz: [Öğreticisi: Spark kullanarak veri dönüştürme](tutorial-transform-data-spark-powershell.md)