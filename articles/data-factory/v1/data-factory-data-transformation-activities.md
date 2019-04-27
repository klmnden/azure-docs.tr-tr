---
title: 'Veri dönüştürme: İşlem & dönüştürme veri | Microsoft Docs'
description: Veri veya Hadoop, Machine Learning ve Azure Data Lake Analytics'i kullanarak Azure Data factory'de işlem verileri dönüştürme hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 39786731-1e4b-40a4-81b7-d06e127427aa
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: b1a99f2872a69e01232c69a73f36319552429ca0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60486609"
---
# <a name="transform-data-in-azure-data-factory"></a>Azure Data factory'de veri dönüştürme
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
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [Data factory'de veri dönüştürme etkinlikleri](../transform-data.md).

Bu makalede Azure Data factory'de dönüştürmek için kullanabilir ve Öngörüler ve öngörüleri ham verilerinizi işleyen veri dönüştürme etkinlikleri açıklar. Azure HDInsight kümesi veya bir Azure Batch gibi bilgi işlem ortamında bir dönüştürme etkinliği yürütür. Her dönüştürme etkinliğine ilişkin ayrıntılı bilgiler içeren makalelerin bağlantıları sağlar.

Data Factory için eklenen aşağıdaki veri dönüştürme etkinliklerini destekler [işlem hatları](data-factory-create-pipelines.md) ya da tek tek veya başka bir etkinlikle zincirleme.

> [!NOTE]
> Adım adım yönergeler içeren bir kılavuz için bkz. [ile Hive dönüşüm işlem hattı oluşturma](data-factory-build-your-first-pipeline.md) makalesi.  
> 
> 

## <a name="hdinsight-hive-activity"></a>HDInsight Hive etkinliği
Hive sorguları kendi sunucunuzda veya isteğe bağlı Windows/Linux tabanlı HDInsight kümesi Data Factory işlem hattındaki HDInsight Hive etkinliği yürütür. Bkz: [Hive etkinliği](data-factory-hive-activity.md) makalede bu etkinliği hakkında ayrıntılı bilgi için. 

## <a name="hdinsight-pig-activity"></a>HDInsight Pig etkinliği
HDInsight Pig etkinliği bir Data Factory işlem hattında, Pig sorgu kendiniz veya isteğe bağlı Windows/Linux tabanlı HDInsight kümesi yürütür. Bkz: [Pig etkinliği](data-factory-pig-activity.md) makalede bu etkinliği hakkında ayrıntılı bilgi için. 

## <a name="hdinsight-mapreduce-activity"></a>HDInsight MapReduce etkinliği
MapReduce programlarını kendi sunucunuzda veya isteğe bağlı Windows/Linux tabanlı HDInsight kümesi Data Factory işlem hattındaki HDInsight MapReduce etkinliği yürütür. Bkz: [MapReduce etkinliği](data-factory-map-reduce.md) makalede bu etkinliği hakkında ayrıntılı bilgi için.

## <a name="hdinsight-streaming-activity"></a>HDInsight akış etkinliği
HDInsight akış etkinliği bir Data Factory işlem hattında, Hadoop akış programları kendi sunucunuzda veya isteğe bağlı Windows/Linux tabanlı HDInsight kümesi yürütür. Bkz: [HDInsight akış etkinliği](data-factory-hadoop-streaming-activity.md) bu etkinliği hakkında ayrıntılı bilgi için.

## <a name="hdinsight-spark-activity"></a>HDInsight Spark Etkinliği
HDInsight Spark etkinliği bir Data Factory işlem hattı, Spark programlarını kendi HDInsight kümesinde yürütür. Ayrıntılar için bkz [Factory'den Spark programlarını Azure Data Factory](data-factory-spark.md). 

## <a name="machine-learning-activities"></a>Machine Learning etkinlikleri
Azure Data Factory, kolayca Tahmine dayalı analiz için yayımlanan bir Azure Machine Learning web hizmetini kullanan işlem hatları oluşturmanıza olanak sağlar. Kullanarak [Batch yürütme etkinliği](data-factory-azure-ml-batch-execution-activity.md#invoking-a-web-service-using-batch-execution-activity) bir Azure Data Factory işlem hattı, verileri toplu tahminlerde bulunmak üzere Machine Learning web hizmetini çağırabilirsiniz.

Zaman içinde yeni bir giriş veri kümeleri kullanarak eğitilebileceği denemeleri Puanlama Machine learning'de Tahmine dayalı modelleri gerekir. Yeniden eğitme ile işiniz bittiğinde, Puanlama web hizmeti ile retrained Machine Learning modeli güncelleştirmek istiyorsunuz. Kullanabileceğiniz [güncelleştirmek kaynak etkinliği](data-factory-azure-ml-batch-execution-activity.md#updating-models-using-update-resource-activity) web hizmeti ile yeni eğitilen modeli güncelleştirmek için.  

Bkz: [kullanım Machine Learning etkinlikleri](data-factory-azure-ml-batch-execution-activity.md) bu Machine Learning etkinlikleri hakkında ayrıntılı bilgi için. 

## <a name="stored-procedure-activity"></a>Depolanan yordam etkinliği
Aşağıdaki veri depolarını birinde bir saklı yordam çağırmak için bir Data Factory işlem hattı, SQL Server saklı yordam etkinliğine kullanabilirsiniz: Azure SQL veritabanı, Azure SQL veri ambarı, SQL Server veritabanı kuruluşunuza veya bir Azure VM. Bkz: [saklı yordam etkinliğine](data-factory-stored-proc-activity.md) makale Ayrıntılar için.  

## <a name="data-lake-analytics-u-sql-activity"></a>Data Lake Analytics U-SQL etkinliği
Data Lake Analytics U-SQL etkinliği, bir Azure Data Lake Analytics kümesinde bir U-SQL betiği çalıştırır. Bkz: [Data Analytics U-SQL etkinliği](data-factory-usql-activity.md) makale Ayrıntılar için. 

## <a name="net-custom-activity"></a>.NET özel etkinliği
Verileri Data Factory tarafından desteklenmeyen bir şekilde dönüştürmek isterseniz, kendi veri işleme mantığı ile özel bir etkinlik oluşturma ve işlem hattı etkinliğini kullanın. Bir Azure Batch hizmeti ya da bir Azure HDInsight kümesi kullanarak çalıştırmak için özel bir .NET etkinliği yapılandırabilirsiniz. Bkz: [özel etkinlikler kullanma](data-factory-use-custom-activities.md) makale Ayrıntılar için. 

R yüklü HDInsight kümenizde R betiklerini çalıştırmak için özel bir etkinlik oluşturabilirsiniz. Bkz. [Azure Data Factory kullanarak R Betiği çalıştırma](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample). 

## <a name="compute-environments"></a>Ortam işlem
İşlem ortamı için bağlı hizmet oluşturup ardından bağlı hizmet bir dönüştürme etkinliği tanımlarken kullanılır. Data Factory tarafından desteklenen işlem ortamlarının iki türü vardır. 

1. **İsteğe bağlı**:  Bu durumda, bilgi işlem ortamınız, Data Factory tarafından tamamen yönetilir. Bir iş verileri işlemek için gönderilen ve iş tamamlandığında kaldırıldı önce Data Factory hizmeti tarafından otomatik olarak oluşturulur. Yapılandırma ve iş yürütme, küme yönetimi ve önyükleme eylemleri için isteğe bağlı işlem ortamının ayrıntılı ayarları denetler. 
2. **Kendi işleyicinizi getirin**: Bu durumda, Data Factory öğesinde bağlantılı hizmet olarak kendi bilgi işlem ortamı (örneğin, HDInsight kümesi) kaydedebilirsiniz. Bilgi işlem ortamınız sizin tarafınızdan yönetilen ve Data Factory hizmetinin etkinlikleri yürütmek için kullanır. 

Bkz: [işlem bağlı Hizmetleri](data-factory-compute-linked-services.md) makale, Data Factory tarafından desteklenen işlem hizmetlerinin hakkında bilgi edinmek için. 

## <a name="summary"></a>Özet
Azure Data Factory aşağıdaki veri dönüştürme etkinlikleri ve işlem ortamlarını etkinlikler için destekler. Dönüştürme etkinlikleri işlem hatlarına tek tek ya da eklenebilir ya da başka bir etkinlikle zincirleme.

| Veri dönüştürme etkinliği | İşlem ortamı |
|:--- |:--- |
| [Hive](data-factory-hive-activity.md) |HDInsight [Hadoop] |
| [Pig](data-factory-pig-activity.md) |HDInsight [Hadoop] |
| [MapReduce](data-factory-map-reduce.md) |HDInsight [Hadoop] |
| [Hadoop Akışı](data-factory-hadoop-streaming-activity.md) |HDInsight [Hadoop] |
| [Machine Learning etkinlikleri: Toplu yürütme ve kaynak güncelleştirme](data-factory-azure-ml-batch-execution-activity.md) |Azure VM |
| [Saklı Yordam](data-factory-stored-proc-activity.md) |Azure SQL, Azure SQL Veri Ambarı veya SQL Server |
| [Data Lake Analytics U-SQL](data-factory-usql-activity.md) |Azure Data Lake Analytics |
| [DotNet](data-factory-use-custom-activities.md) |HDInsight [Hadoop] veya Azure Batch |

