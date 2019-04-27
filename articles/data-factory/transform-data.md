---
title: Azure Data Factory kullanarak verileri dönüştürme | Microsoft Docs
description: Veri veya Hadoop, Machine Learning ve Azure Data Lake Analytics'i kullanarak Azure Data factory'de işlem verileri dönüştürme hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 07/31/2018
author: nabhishek
ms.author: abnarain
manager: craigg
ms.openlocfilehash: 2c674f77ef0f779c9764771e2e0ae7a4aea47548
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60622116"
---
# <a name="transform-data-in-azure-data-factory"></a>Azure Data factory'de veri dönüştürme
> [!div class="op_single_selector"]
> * [Hive](transform-data-using-hadoop-hive.md)  
> * [Pig](transform-data-using-hadoop-pig.md)  
> * [MapReduce](transform-data-using-hadoop-map-reduce.md)  
> * [HDInsight akış](transform-data-using-hadoop-streaming.md)
> * [HDInsight Spark](transform-data-using-spark.md)
> * [Machine Learning](transform-data-using-machine-learning.md) 
> * [Saklı Yordam](transform-data-using-stored-procedure.md)
> * [Data Lake Analytics U-SQL](transform-data-using-data-lake-analytics.md)
> * [Databricks not defteri](transform-data-databricks-notebook.md)
> * [Databricks Jar](transform-data-databricks-jar.md)
> * [Databricks Python](transform-data-databricks-python.md)
> * [.NET özel](transform-data-using-dotnet-custom-activity.md)

## <a name="overview"></a>Genel Bakış
Bu makalede Azure Data factory'de dönüştürmek için kullanabilir ve Öngörüler ve öngörüleri ham verilerinizi işleyen veri dönüştürme etkinlikleri açıklar. Azure HDInsight kümesi veya bir Azure Batch gibi bilgi işlem ortamında bir dönüştürme etkinliği yürütür. Her dönüştürme etkinliğine ilişkin ayrıntılı bilgiler içeren makalelerin bağlantıları sağlar.

Data Factory için eklenen aşağıdaki veri dönüştürme etkinliklerini destekler [işlem hatları](concepts-pipelines-activities.md) ya da tek tek veya başka bir etkinlikle zincirleme.

## <a name="hdinsight-hive-activity"></a>HDInsight Hive etkinliği
Hive sorguları kendi sunucunuzda veya isteğe bağlı Windows/Linux tabanlı HDInsight kümesi Data Factory işlem hattındaki HDInsight Hive etkinliği yürütür. Bkz: [Hive etkinliği](transform-data-using-hadoop-hive.md) makalede bu etkinliği hakkında ayrıntılı bilgi için. 

## <a name="hdinsight-pig-activity"></a>HDInsight Pig etkinliği
HDInsight Pig etkinliği bir Data Factory işlem hattında, Pig sorgu kendiniz veya isteğe bağlı Windows/Linux tabanlı HDInsight kümesi yürütür. Bkz: [Pig etkinliği](transform-data-using-hadoop-pig.md) makalede bu etkinliği hakkında ayrıntılı bilgi için. 

## <a name="hdinsight-mapreduce-activity"></a>HDInsight MapReduce etkinliği
MapReduce programlarını kendi sunucunuzda veya isteğe bağlı Windows/Linux tabanlı HDInsight kümesi Data Factory işlem hattındaki HDInsight MapReduce etkinliği yürütür. Bkz: [MapReduce etkinliği](transform-data-using-hadoop-map-reduce.md) makalede bu etkinliği hakkında ayrıntılı bilgi için.

## <a name="hdinsight-streaming-activity"></a>HDInsight akış etkinliği
HDInsight akış etkinliği bir Data Factory işlem hattında, Hadoop akış programları kendi sunucunuzda veya isteğe bağlı Windows/Linux tabanlı HDInsight kümesi yürütür. Bkz: [HDInsight akış etkinliği](transform-data-using-hadoop-streaming.md) bu etkinliği hakkında ayrıntılı bilgi için.

## <a name="hdinsight-spark-activity"></a>HDInsight Spark etkinliği
HDInsight Spark etkinliği bir Data Factory işlem hattı, Spark programlarını kendi HDInsight kümesinde yürütür. Ayrıntılar için bkz [Factory'den Spark programlarını Azure Data Factory](transform-data-using-spark.md). 

## <a name="machine-learning-activities"></a>Machine Learning etkinlikleri
Azure Data Factory, kolayca Tahmine dayalı analiz için yayımlanan bir Azure Machine Learning web hizmetini kullanan işlem hatları oluşturmanıza olanak sağlar. Kullanarak [Batch yürütme etkinliği](transform-data-using-machine-learning.md) bir Azure Data Factory işlem hattı, verileri toplu tahminlerde bulunmak üzere Machine Learning web hizmetini çağırabilirsiniz.

Zaman içinde yeni bir giriş veri kümeleri kullanarak eğitilebileceği denemeleri Puanlama Machine learning'de Tahmine dayalı modelleri gerekir. Yeniden eğitme ile işiniz bittiğinde, Puanlama web hizmeti ile retrained Machine Learning modeli güncelleştirmek istiyorsunuz. Kullanabileceğiniz [kaynak güncelleştirme etkinliği](update-machine-learning-models.md) web hizmeti ile yeni eğitilen modeli güncelleştirmek için.  

Bkz: [kullanım Machine Learning etkinlikleri](transform-data-using-machine-learning.md) bu Machine Learning etkinlikleri hakkında ayrıntılı bilgi için. 

## <a name="stored-procedure-activity"></a>Depolanan yordam etkinliği
Aşağıdaki veri depolarını birinde bir saklı yordam çağırmak için bir Data Factory işlem hattı, SQL Server saklı yordam etkinliğine kullanabilirsiniz: Azure SQL veritabanı, Azure SQL veri ambarı, SQL Server veritabanı kuruluşunuza veya bir Azure VM. Bkz: [saklı yordam etkinliği](transform-data-using-stored-procedure.md) makale Ayrıntılar için.  

## <a name="data-lake-analytics-u-sql-activity"></a>Data Lake Analytics U-SQL etkinliği
Data Lake Analytics U-SQL etkinliği, bir Azure Data Lake Analytics kümesinde bir U-SQL betiği çalıştırır. Bkz: [Data Analytics U-SQL etkinliği](transform-data-using-data-lake-analytics.md) makale Ayrıntılar için. 

## <a name="databricks-notebook-activity"></a>Databricks not defteri etkinliği

Azure Databricks not defteri etkinliği bir Data Factory işlem hattı, Azure Databricks çalışma alanınızda bir Databricks not defteri çalıştırır. Azure Databricks, Apache Spark'ı çalıştırmaya yönelik bir yönetilen bir platformdur. Bkz: [bir Databricks not defteri çalıştırarak verileri dönüştürme](transform-data-databricks-notebook.md).

## <a name="databricks-jar-activity"></a>Databricks Jar etkinliği

Azure Databricks Jar etkinliği bir Data Factory işlem hattı, Azure Databricks kümesinde bir Spark Jar çalıştırır. Azure Databricks, Apache Spark'ı çalıştırmaya yönelik bir yönetilen bir platformdur. Bkz: [Azure Databricks'te bir Jar etkinliği çalıştırarak verileri dönüştürme](transform-data-databricks-jar.md).

## <a name="databricks-python-activity"></a>Databricks Python etkinliği

Data Factory işlem hattı Azure Databricks Python etkinliğinde bir Python dosyası, Azure Databricks kümesinde çalışır. Azure Databricks, Apache Spark'ı çalıştırmaya yönelik bir yönetilen bir platformdur. Bkz: [Azure Databricks'te bir Python etkinliği çalıştırarak verileri dönüştürme](transform-data-databricks-python.md).

## <a name="custom-activity"></a>Özel etkinlik
Verileri Data Factory tarafından desteklenmeyen bir şekilde dönüştürmek isterseniz, kendi veri işleme mantığı ile özel bir etkinlik oluşturma ve işlem hattı etkinliğini kullanın. Bir Azure Batch hizmeti ya da bir Azure HDInsight kümesi kullanarak çalıştırmak için özel bir .NET etkinliği yapılandırabilirsiniz. Bkz: [özel etkinlikler kullanma](transform-data-using-dotnet-custom-activity.md) makale Ayrıntılar için. 

R yüklü HDInsight kümenizde R betiklerini çalıştırmak için özel bir etkinlik oluşturabilirsiniz. Bkz. [Azure Data Factory kullanarak R Betiği çalıştırma](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample). 

## <a name="compute-environments"></a>Ortam işlem
İşlem ortamı için bağlı hizmet oluşturup ardından bağlı hizmet bir dönüştürme etkinliği tanımlarken kullanılır. Data Factory tarafından desteklenen işlem ortamlarının iki türü vardır. 

- **İsteğe bağlı**:  Bu durumda, bilgi işlem ortamınız, Data Factory tarafından tamamen yönetilir. Bir iş verileri işlemek için gönderilen ve iş tamamlandığında kaldırıldı önce Data Factory hizmeti tarafından otomatik olarak oluşturulur. Yapılandırma ve iş yürütme, küme yönetimi ve önyükleme eylemleri için isteğe bağlı işlem ortamının ayrıntılı ayarları denetler. 
- **Kendi işleyicinizi getirin**: Bu durumda, Data Factory öğesinde bağlantılı hizmet olarak kendi bilgi işlem ortamı (örneğin, HDInsight kümesi) kaydedebilirsiniz. Bilgi işlem ortamınız sizin tarafınızdan yönetilen ve Data Factory hizmetinin etkinlikleri yürütmek için kullanır. 

Bkz: [işlem bağlı Hizmetleri](compute-linked-services.md) makale, Data Factory tarafından desteklenen işlem hizmetlerinin hakkında bilgi edinmek için. 

## <a name="next-steps"></a>Sonraki adımlar
Bir dönüştürme etkinliği kullanma örneği için aşağıdaki öğreticiye bakın: [Öğretici: Spark kullanarak verileri dönüştürme](tutorial-transform-data-spark-powershell.md)
