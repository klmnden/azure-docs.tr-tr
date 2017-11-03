---
title: "Veri dönüştürme: İşlem & dönüştürme veri | Microsoft Docs"
description: "Veri ya da Hadoop, Machine Learning veya Azure Data Lake Analytics kullanarak Azure Data Factory işlem verilerde dönüştürme öğrenin."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 39786731-1e4b-40a4-81b7-d06e127427aa
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: shlo
robots: noindex
ms.openlocfilehash: 2ec40ee232ab50f79169f42b6573d294b77110a2
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="transform-data-in-azure-data-factory"></a>Azure Data Factory veri dönüştürme
> [!div class="op_single_selector"]
> * [Hive](data-factory-hive-activity.md)  
> * [Pig](data-factory-pig-activity.md)  
> * [MapReduce](data-factory-map-reduce.md)  
> * [Hadoop Akışı](data-factory-hadoop-streaming-activity.md)
> * [Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
> * [Saklı Yordam](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL](data-factory-usql-activity.md)
> * [.NET özel](data-factory-use-custom-activities.md)

## <a name="overview"></a>Genel Bakış
> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [sürüm 2 veri fabrikasında veri dönüştürme etkinlikleri](../transform-data.md).

Bu makalede Azure Data factory'de dönüştürmek için kullanabilir ve ham verilerinizi Öngörüler ve öngörü işler veri dönüştürme etkinlikleri açıklar. Azure Hdınsight kümesini veya bir Azure Batch gibi bilgi işlem ortamında dönüştürme etkinliğine yürütür. Makalelerinin bağlantıları her dönüştürme etkinliği hakkında ayrıntılı bilgi sağlar.

Veri Fabrikası destekleyen eklenebilir aşağıdaki veri dönüştürme etkinlikleri [ardışık düzen](data-factory-create-pipelines.md) ya da ayrı ayrı veya başka bir etkinlikle zincirleme.

> [!NOTE]
> Adım adım yönergeler içeren bir anlatım için bkz: [ile Hive dönüşüm işlem hattı oluşturma](data-factory-build-your-first-pipeline.md) makalesi.  
> 
> 

## <a name="hdinsight-hive-activity"></a>Hdınsight Hive etkinliği
Data Factory işlem hattı Hdınsight Hive etkinliğinde, Hive sorguları kendi veya isteğe bağlı Windows/Linux tabanlı Hdınsight kümesi yürütür. Bkz: [Hive etkinliği](data-factory-hive-activity.md) bu etkinliği hakkında ayrıntılı bilgi için makalenin. 

## <a name="hdinsight-pig-activity"></a>Hdınsight Pig etkinliği
Data Factory işlem hattı Hdınsight Pig etkinliğinde kendi Pig sorgular veya isteğe bağlı Windows/Linux tabanlı Hdınsight kümesi yürütür. Bkz: [Pig etkinlik](data-factory-pig-activity.md) bu etkinliği hakkında ayrıntılı bilgi için makalenin. 

## <a name="hdinsight-mapreduce-activity"></a>Hdınsight MapReduce etkinliği
Data Factory işlem hattı Hdınsight MapReduce etkinliğinde kendi MapReduce programları veya isteğe bağlı Windows/Linux tabanlı Hdınsight kümesi yürütür. Bkz: [MapReduce etkinliği](data-factory-map-reduce.md) bu etkinliği hakkında ayrıntılı bilgi için makalenin.

## <a name="hdinsight-streaming-activity"></a>Hdınsight akış etkinliği
Hdınsight akış etkinliğinde Data Factory işlem hattı Hadoop akış programlar kendi veya isteğe bağlı Windows/Linux tabanlı Hdınsight kümesi yürütür. Bkz: [Hdınsight akış etkinliği](data-factory-hadoop-streaming-activity.md) bu etkinliği hakkında ayrıntılı bilgi için.

## <a name="hdinsight-spark-activity"></a>HDInsight Spark Etkinliği
Data Factory işlem hattı Hdınsight Spark etkinliğinde Spark programlar kendi Hdınsight kümesinde yürütür. Ayrıntılar için bkz [Azure Data Factory çağırma Spark programlardan](data-factory-spark.md). 

## <a name="machine-learning-activities"></a>Machine Learning etkinlikleri
Azure Data Factory, Tahmine dayalı analiz için yayımlanan bir Azure Machine Learning web hizmetini kullan ardışık düzen kolayca oluşturmanıza olanak sağlar. Kullanarak [toplu iş yürütme etkinliği](data-factory-azure-ml-batch-execution-activity.md#invoking-a-web-service-using-batch-execution-activity) bir Azure Data Factory işlem hattı verileri toplu tahminlerde için Machine Learning web hizmeti çağırabilirsiniz.

Zaman içinde denemeler Puanlama Machine Learning Tahmine dayalı modelleri yeni giriş veri kümeleri kullanarak retrained gerekir. Yeniden eğitme ile tamamladıktan sonra ile retrained Machine Learning modeli Puanlama web hizmetini güncelleştirmek istiyor. Kullanabileceğiniz [güncelleştirme kaynak etkinliği](data-factory-azure-ml-batch-execution-activity.md#updating-models-using-update-resource-activity) web hizmeti ile yeni eğitilen modeli güncelleştirmek için.  

Bkz: [kullanım Machine Learning etkinlikleri](data-factory-azure-ml-batch-execution-activity.md) bu Machine Learning etkinlikler hakkında ayrıntılı bilgi için. 

## <a name="stored-procedure-activity"></a>Saklı yordam etkinliği
Aşağıdaki veri depolarına birinde bir saklı yordam çağrılacak bir Data Factory işlem hattı ve SQL Server saklı yordam etkinliğinin kullanabilirsiniz: Azure SQL Database, Azure SQL Data Warehouse, SQL Server veritabanı kuruluşunuzdaki veya bir Azure VM. Bkz: [saklı yordam etkinliği](data-factory-stored-proc-activity.md) Ayrıntılar için makale.  

## <a name="data-lake-analytics-u-sql-activity"></a>Data Lake Analytics U-SQL etkinliği
Data Lake Analytics U-SQL etkinliği Azure Data Lake Analytics kümede bir U-SQL komut dosyasını çalıştırır. Bkz: [veri analizi U-SQL etkinliği](data-factory-usql-activity.md) Ayrıntılar için makale. 

## <a name="net-custom-activity"></a>.NET özel etkinliği
Veri fabrikası tarafından desteklenmeyen bir şekilde veri dönüştürme ihtiyacınız varsa, kendi veri işleme mantığı ile özel bir etkinlik oluşturmak ve ardışık düzeninde etkinlik kullanın. Bir Azure Batch hizmeti ya da Azure Hdınsight kümesi kullanarak çalıştırmak için özel .NET etkinliği yapılandırabilirsiniz. Bkz: [özel etkinlikleri kullanmak](data-factory-use-custom-activities.md) Ayrıntılar için makale. 

R yüklü HDInsight kümenizde R betiklerini çalıştırmak için özel bir etkinlik oluşturabilirsiniz. Bkz. [Azure Data Factory kullanarak R Betiği çalıştırma](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample). 

## <a name="compute-environments"></a>Ortamlar işlem
Bilgi işlem ortamı için bağlı hizmet oluşturup ardından bağlantılı hizmet dönüştürme etkinliğine tanımlarken kullanabilirsiniz. Bilgi işlem ortamları Data Factory ile desteklenen iki tür vardır. 

1. **İsteğe bağlı**: Bu durumda, bilgi işlem ortamı Data Factory ile tam olarak yönetilir. Bir işi veri işlemek için gönderildi ve iş tamamlandığında, kaldırılan önce Data Factory hizmeti tarafından otomatik olarak oluşturulur. Yapılandırma ve iş yürütme, küme yönetimi ve eylemler önyükleme için isteğe bağlı işlem ortamının ayrıntılı ayarlarını denetleyebilirsiniz. 
2. **Kendi bilgisayarınızı getirin**: Bu durumda, veri fabrikasında bağlı hizmet olarak kendi bilgi işlem ortamı (örneğin, Hdınsight kümesi) kaydedebilirsiniz. Bilgi işlem ortamı sizin tarafınızdan yönetilen ve Data Factory hizmetinin etkinlikleri yürütmek için kullanır. 

Bkz: [işlem bağlı Hizmetleri](data-factory-compute-linked-services.md) makale Data Factory ile desteklenen işlem hizmetleri hakkında bilgi edinin. 

## <a name="summary"></a>Özet
Azure Data Factory etkinlikleri için aşağıdaki veri dönüştürme etkinlikleri ve bilgi işlem ortamları destekler. Dönüştürme etkinlikleri için ardışık düzen ya da ayrı ayrı eklenemez veya başka bir etkinlikle zincirleme.

| Veri dönüştürme etkinliği | İşlem ortamı |
|:--- |:--- |
| [Hive](data-factory-hive-activity.md) |HDInsight [Hadoop] |
| [Pig](data-factory-pig-activity.md) |HDInsight [Hadoop] |
| [MapReduce](data-factory-map-reduce.md) |HDInsight [Hadoop] |
| [Hadoop Akışı](data-factory-hadoop-streaming-activity.md) |HDInsight [Hadoop] |
| [Machine Learning etkinlikleri: Toplu Yürütme ve Kaynak Güncelleştirme](data-factory-azure-ml-batch-execution-activity.md) |Azure VM |
| [Saklı Yordam](data-factory-stored-proc-activity.md) |Azure SQL, Azure SQL Veri Ambarı veya SQL Server |
| [Data Lake Analytics U-SQL](data-factory-usql-activity.md) |Azure Data Lake Analytics |
| [DotNet](data-factory-use-custom-activities.md) |HDInsight [Hadoop] veya Azure Batch |

