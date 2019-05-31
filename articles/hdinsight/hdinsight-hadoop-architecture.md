---
title: Apache Hadoop mimarisi - Azure HDInsight
description: HDInsight kümelerinde, Apache Hadoop depolama ve işleme açıklar.
author: ashishthaps
ms.author: ashishth
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/27/2019
ms.openlocfilehash: 3fd85232ff7044c699a3e68ce34b267bf50c4dc3
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66257868"
---
# <a name="apache-hadoop-architecture-in-hdinsight"></a>HDInsight’ta Apache Hadoop mimarisi

[Apache Hadoop](https://hadoop.apache.org/) iki çekirdek bileşenleri içerir: [Apache Hadoop dağıtılmış dosya sistemi (HDFS)](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsUserGuide.html) depolama sağlayan ve [Apache Hadoop henüz başka bir Resource Negotiator (YARN)](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html) , işleme sağlar. Depolama ve işleme özellikleri ile bir küme çalıştırılması yeteneği olur [MapReduce](https://hadoop.apache.org/docs/current/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html) istenen veri işleme gerçekleştirmek için programlar.

> [!NOTE]  
> Bir HDFS genellikle depolama sağlamak için HDInsight küme içinde dağıtılmaz. Bunun yerine, HDFS uyumlu bir arabirim katmanına Hadoop bileşenleri tarafından kullanılır. Gerçek depolama özelliği, Azure depolama veya Azure Data Lake Storage tarafından sağlanır. Hadoop, HDInsight kümesinde yürütme MapReduce işleri bir HDFS mevcut değilmiş gibi çalıştırın ve depolama gereksinimlerini desteklemek için herhangi bir değişiklik zorunlu. HDInsight üzerinde Hadoop, depolama dış kaynaklı, ancak temel bir bileşenidir YARN işleme kalır. Daha fazla bilgi için [Azure HDInsight giriş](hadoop/apache-hadoop-introduction.md).

Bu makalede, YARN ve yürütme HDInsight uygulamalarının nasıl koordine eden tanıtır.

## <a name="apache-hadoop-yarn-basics"></a>Apache Hadoop YARN temelleri 

YARN Hadoop veri işlemeye düzenler ve yönetir. YARN kümedeki düğümlere Çalıştır iki Çekirdek Hizmetleri sahiptir: 

* ResourceManager 
* NodeManager

ResourceManager MapReduce işleri gibi uygulamalar için küme bilgi işlem kaynaklarını verir. ResourceManager bu kaynakların her kapsayıcı CPU Çekirdeği ve RAM bellek ayırma işlemi burada oluşur kapsayıcıları olarak verir. Birleştirilmiş bir kümede bulunan tüm kaynaklara ve ardından dağıtılmış çekirdek ve bellek blokları, kaynakların her blok bir kapsayıcıdır. Belirli bir kapsayıcı sayısı için kapasite kümedeki her düğüme sahiptir, bu nedenle sabit bir sınır küme kapsayıcı yok sayısına sahip. Bir kapsayıcıda kaynak ayırmayı yapılandırılabilir. 

MapReduce uygulama küme üzerinde çalıştığında, ResourceManager yürütüleceği kapsayıcılarda uygulama sağlar. ResourceManager çalışan uygulamalar, mevcut küme kapasitesi durumunu izler ve uygulamaları tamamlandı olarak izler ve kaynaklarını serbest bırakın. 

ResourceManager uygulamalarının durumunu izlemek için bir web kullanıcı arabirimi sağlayan bir web sunucusu işlemi de çalışır.

Bir kullanıcı kümesi üzerinde bir MapReduce uygulamasını gönderdiğinde, uygulama için ResourceManager gönderilir. Buna karşılık, ResourceManager kullanılabilir NodeManager düğümlerinde bir kapsayıcı ayırır. Burada uygulamanın gerçekten yürütür NodeManager düğümlerdir. Ayrılan ilk kapsayıcı ApplicationMaster adında özel bir uygulama çalıştırır. Gönderilen bir uygulamayı çalıştırmak için gereken sonraki kapsayıcıları biçiminde kaynakları almak için bu ApplicationMaster sorumludur. ApplicationMaster Uygulama Haritası aşama gibi aşamalarını inceler ve aşama ve ne kadar veri işlenmesi gereken unsurlar azaltın. Ardından ApplicationMaster istekleri (*görüşür*) ResourceManager uygulama adına kaynaklar. ResourceManager sırayla kaynakları kümedeki NodeManagers'ndan uygulama yürütülürken kullanılacak ApplicationMaster bunun için verir. 

Uygulamayı oluşturan görevleri çalıştırma NodeManagers sonra rapor, ilerleme ve durumu geri ApplicationMaster için. ApplicationMaster sırayla ResourceManager uygulamaya geri durumunu raporlar. ResourceManager istemciye herhangi bir sonuç döndürür.

## <a name="yarn-on-hdinsight"></a>YARN üzerinde HDInsight

Tüm HDInsight küme türleri YARN dağıtın. Küme içindeki ilk ve ikinci baş düğümler hakkında sırasıyla çalıştıran bir birincil ve ikincil örneği ile yüksek kullanılabilirlik için ResourceManager dağıtılır. Bir kerede yalnızca bir örneğini ResourceManager etkindir. Kümedeki kullanılabilir çalışan düğümler arasında NodeManager örneği çalıştırın.

![YARN üzerinde HDInsight](./media/hdinsight-hadoop-architecture/yarn-on-hdinsight.png)

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight üzerinde Apache Hadoop ile MapReduce'u kullanma](hadoop/hdinsight-use-mapreduce.md)
* [Azure HDInsight giriş](hadoop/apache-hadoop-introduction.md)
