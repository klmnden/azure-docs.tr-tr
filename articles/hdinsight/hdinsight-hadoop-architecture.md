---
title: Hadoop mimarisi - Azure HDInsight
description: HDInsight kümeleri, Hadoop depolama ve işleme açıklar.
services: hdinsight
author: ashishthaps
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/19/2018
ms.author: ashishth
ms.openlocfilehash: 754f4538f7c2a8de6286198094b38d40c466a15f
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39599481"
---
# <a name="hadoop-architecture-in-hdinsight"></a>HDInsight’ta Hadoop mimarisi

Hadoop, iki temel bileşenleri, yüksek yoğunluklu dosya sistemi (depolama sağlayan HDFS) ve henüz başka bir Resource Negotiator (işleme sağlayan YARN) içerir. Depolama ve işleme özellikleri ile bir küme gerçekleştirmek istediğiniz veri işleme için MapReduce programlarını çalıştırabilen haline gelir.

> [!NOTE]
> Bir HDFS genellikle depolama sağlamak için HDInsight küme içinde dağıtılmaz. Bunun yerine, HDFS uyumlu bir arabirim katmanına Hadoop bileşenleri tarafından kullanılır. Gerçek depolama özelliği, Azure depolama veya Azure Data Lake Store tarafından sağlanır. Hadoop, HDInsight kümesinde yürütme MapReduce işleri bir HDFS mevcut değilmiş gibi çalıştırın ve depolama gereksinimlerini desteklemek için herhangi bir değişiklik zorunlu. HDInsight üzerinde Hadoop, depolama dış kaynaklı, ancak temel bir bileşenidir YARN işleme kalır. Daha fazla bilgi için [Azure HDInsight giriş](hadoop/apache-hadoop-introduction.md).

Bu makalede, YARN ve yürütme HDInsight uygulamalarının nasıl koordine eden tanıtır.

## <a name="yarn-basics"></a>YARN temelleri 

YARN Hadoop veri işlemeye düzenler ve yönetir. YARN kümedeki düğümlere Çalıştır iki Çekirdek Hizmetleri sahiptir: 

* ResourceManager 
* NodeManager

ResourceManager MapReduce işleri gibi uygulamalar için küme bilgi işlem kaynaklarını verir. ResourceManager bu kaynakların her kapsayıcı CPU Çekirdeği ve RAM bellek ayırma işlemi burada oluşur kapsayıcıları olarak verir. Birleştirilmiş bir kümede bulunan tüm kaynaklara ve bunları belirli sayıda çekirdek ve bellek bloklarını dağıtılmış, kaynakların her blok bir kapsayıcıdır. Kümedeki her düğümün belirli bir kapsayıcı sayısı için kapasite, ve bu nedenle küme sabit bir sınır kapsayıcı yok sayısına sahiptir. Bir kapsayıcıda kaynak ayırmayı yapılandırılabilir. 

MapReduce uygulama küme üzerinde çalıştığında, ResourceManager yürütüleceği kapsayıcılarda uygulama sağlar. ResourceManager çalışan uygulamalar, mevcut küme kapasitesi durumunu izler ve uygulamaları tamamlandı olarak izler ve kaynaklarını serbest bırakın. 

ResourceManager uygulamalarının durumunu izlemek için kullanabileceğiniz bir web kullanıcı arabirimi sağlayan bir web sunucusu işlemi de çalışır. 

Bir kullanıcı kümesi üzerinde bir MapReduce uygulamasını gönderdiğinde, uygulama için ResourceManager gönderilir. Buna karşılık, ResourceManager kullanılabilir NodeManager düğümlerinde bir kapsayıcı ayırır. Burada uygulamanın gerçekten yürütür NodeManager düğümlerdir. Ayrılan ilk kapsayıcı ApplicationMaster adında özel bir uygulama çalıştırır. Gönderilen bir uygulamayı çalıştırmak için gereken sonraki kapsayıcıları biçiminde kaynakları almak için bu ApplicationMaster sorumludur. Uygulama aşamalarını ApplicationMaster inceler (harita hazırlayın ve aşama azaltma) ve Etkenler işlenecek ne kadar veri gerekiyor. Ardından ApplicationMaster istekleri (*görüşür*) ResourceManager uygulama adına kaynaklar. ResourceManager sırayla kaynakları kümedeki NodeManagers'ndan uygulama yürütülürken kullanılacak ApplicationMaster bunun için verir. 

Uygulamayı oluşturan görevleri çalıştırma NodeManagers sonra rapor, ilerleme ve durumu geri ApplicationMaster için. ApplicationMaster sırayla ResourceManager uygulamaya geri durumunu raporlar. ResourceManager istemciye herhangi bir sonuç döndürür.

## <a name="yarn-on-hdinsight"></a>YARN üzerinde HDInsight

Tüm HDInsight küme türleri YARN dağıtın. ResourceManager, küme içindeki ilk ve ikinci baş düğümler hakkında sırasıyla çalıştığı birincil ve ikincil bir örneği ile yüksek kullanılabilirlik için dağıtılır. Bir kerede yalnızca bir örneğini ResourceManager etkindir. Kümedeki kullanılabilir çalışan düğümler arasında NodeManager örneği çalıştırın.

![YARN üzerinde HDInsight](./media/hdinsight-hadoop-architecture/yarn-on-hdinsight.png)

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight üzerinde Hadoop MapReduce kullanma](hadoop/hdinsight-use-mapreduce.md)
* [Azure HDInsight giriş](hadoop/apache-hadoop-introduction.md)
