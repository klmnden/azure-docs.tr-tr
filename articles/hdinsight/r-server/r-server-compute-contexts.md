---
title: İçin işlem bağlamı seçenekleri ML hizmetleri üzerinde HDInsight - Azure
description: Farklı işlem bağlamı seçenekleri ML Hizmetleri ile kullanıcılara HDInsight hakkında bilgi edinin
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/27/2018
ms.openlocfilehash: 9dac7aa19e428c964bd10c3ef62df949393e8d1f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64681777"
---
# <a name="compute-context-options-for-ml-services-on-hdinsight"></a>İçin işlem bağlamı seçenekleri ML hizmetleri üzerinde HDInsight

ML Hizmetleri Azure HDInsight üzerinde işlem bağlamını ayarlayarak çağrıları nasıl yürütülür denetler. Bu makalede görüntülenip görüntülenmeyeceğini ve nasıl yürütme kenar düğümüne veya HDInsight kümesi Çekirdekte paralelleştirildi belirtmek için kullanılabilir seçenekleri açıklar.

Bir küme kenar düğümüne kümeye bağlanın ve, R betikleri çalıştırmak için uygun bir yer sağlar. Bir kenar düğümüne ile çalıştırmanın RevoScaleR uç düğümü sunucusunun çekirdek üzerinde paralel dağıtılmış işlevleri seçeneğiniz vardır. Ayrıca bunları tüm küme düğümlerine RevoScaleR'ın Hadoop Map Reduce kullanarak çalıştırabilirsiniz veya Apache Spark işlem bağlamlarının.

## <a name="ml-services-on-azure-hdinsight"></a>Azure HDInsight üzerinde ML Hizmetleri
[Azure HDInsight üzerinde ML Hizmetleri](r-server-overview.md) R tabanlı analiz için en son özellikleri sağlar. Bir Apache Hadoop HDFS kapsayıcısında depolanan veriler kullanabilirsiniz, [Azure Blob](../../storage/common/storage-introduction.md "Azure Blob Depolama") depolama hesabı, bir Data Lake store veya yerel Linux dosya sistemi. ML Hizmetleri açık kaynak R kurulu olduğundan, oluşturduğunuz R tabanlı uygulama 8000 + açık kaynak R paketlerinin hiçbirini uygulayabilirsiniz. ' Ndaki yordamlara de kullanabilirsiniz [RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler), ML Hizmetleri ile birlikte Microsoft'un büyük veri analizi paketi.  

## <a name="compute-contexts-for-an-edge-node"></a>İşlem bağlamları bir kenar düğümü için
Genel olarak, kenar düğümüne ML Hizmetleri kümede çalıştırılan R betiği İnterpret R içinde bu düğüm üzerinde çalışır. Özel durumlar RevoScaleR işlevi çağıran bu adımlardır. RevoScaleR çağrıları RevoScaleR işlem bağlamına nasıl ayarlanacağını belirlenen işlem ortamında çalıştırın.  Bir edge düğümünü R betiğinizi çalıştırdığınızda, işlem bağlam olası değerler şunlardır:

- Yerel sıralı (*yerel*)
- yerel paralel (*localpar*)
- Harita azaltın
- Spark

*Yerel* ve *localpar* seçenekler farklılık gösterir, yalnızca nasıl **rxExec** çağrıları yürütülür. Her ikisi de diğer rx işlev çağrılarını paralel bir şekilde tüm kullanılabilir çekirdek arasında RevoScaleR kullanarak aksi belirtilmediği sürece yürütme **numCoresToUse** seçeneği, örneğin `rxOptions(numCoresToUse=6)`. Paralel yürütme seçenekleri en iyi performans sunar.

Aşağıdaki tabloda, çeşitli işlem bağlamı seçenekleri çağrıları nasıl yürütülür ayarlamak için özetlenmiştir:

| İşlem bağlamı  | Nasıl ayarlanır                      | Yürütme bağlamı                        |
| ---------------- | ------------------------------- | ---------------------------------------- |
| Yerel sıra | rxSetComputeContext('local')    | Uç düğümü sunucusunun, seri olarak yürütülmesini rxExec çağrıları dışında çekirdek üzerinde Paralel yürütme |
| Yerel paralel   | rxSetComputeContext('localpar') | Uç düğüm sunucusunun çekirdek üzerinde Paralel yürütme |
| Spark            | RxSpark()                       | Paralel dağıtılmış yürütülmesini Spark aracılığıyla düğümler arasında HDI kümesi |
| Harita azaltın       | RxHadoopMR()                    | Paralel dağıtılmış yürütülmesini Map Reduce aracılığıyla düğümler arasında HDI kümesi |

## <a name="guidelines-for-deciding-on-a-compute-context"></a>Bir işlem bağlamı hakkında karar vermeyle ilgili yönergeler

Paralel yürütme sağlayan üç seçenekleri, seçtiğiniz analytics iş, boyutu ve konumu, verilerinizin yapısını bağlıdır. Hangi işlem bağlamında kullanmak için belirten hiçbir basit bir formül yoktur. Ancak, doğru seçim yapın veya en az bir Kıyaslama çalıştırmadan önce seçimleri daraltmak yardımcı yardımcı olabilecek bazı temel ilkeler vardır. Bu ilkeler kılavuzluk şunlardır:

- Yerel Linux dosya sistemi, HDFS hızlıdır.
- Yinelenen analizleri verileri yerel olup olmadığını ve içinde XDF ise daha hızlıdır.
- Az miktarda bir metin veri kaynağından veri akışını daha iyidir. Veri miktarı büyükse, XDF için önce analiz dönüştürün.
- Kopyalama veya akış verileri analiz için kenar düğümüne ek yükü çok büyük miktarlardaki verilere yönelik yönetilemeyen haline gelir.
- ApacheSpark Map Reduce Hadoop analizi için daha hızlıdır.

Aşağıdaki bölümlerde, bu ilkeleri göz önünde bulundurulduğunda, bazı genel kuralları bir işlem bağlamı seçme karşısında sunar.

### <a name="local"></a>Yerel
* Analiz etmek için veri miktarı, küçük ve yinelenen analiz gerektirmez, ardından, doğrudan çözümleme yordamı kullanarak akış *yerel* veya *localpar*.
* Analiz etmek için veri miktarını küçük veya orta ölçekli ve yinelenen bir analiz gerekiyorsa, ardından yerel dosya sistemine kopyalar, XDF için alma ve aracılığıyla analiz *yerel* veya *localpar*.

### <a name="apache-spark"></a>Apache Spark
* Analiz etmek için veri miktarı büyükse, ardından bunu kullanarak bir Spark DataFrame içeri **RxHiveData** veya **RxParquetData**, veya hdfs'deki XDF (depolama bir sorun olmadığı sürece) ve Spark işlem kullanarak analiz edin bağlamı.

### <a name="apache-hadoop-map-reduce"></a>Apache Hadoop harita azaltın
* Genelde daha yavaş olduğundan yalnızca bir Spark işlem bağlamına insurmountable sorun karşılaşırsanız, Map Reduce işlem bağlamını kullanın.  

## <a name="inline-help-on-rxsetcomputecontext"></a>Satır içi Yardım rxSetComputeContext
Daha fazla bilgi ve örnekler RevoScaleR işlem bağlamlarının R rxSetComputeContext yönteminde, örneğin Yardım satır içi bakın:

    > ?rxSetComputeContext

Ayrıca başvurabilirsiniz [dağıtılmış bilgi işleme genel bakış](https://docs.microsoft.com/machine-learning-server/r/how-to-revoscaler-distributed-computing) içinde [Machine Learning sunucusu belgeleri](https://docs.microsoft.com/machine-learning-server/).

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede ve bunun nasıl yapılacağını yürütme kenar düğümüne veya HDInsight kümesi Çekirdekte paralelleştirildi belirtmek için kullanılabilir seçenekleri hakkında bilgi edindiniz. ML Hizmetleri, HDInsight kümeleri ile kullanma hakkında daha fazla bilgi için aşağıdaki konulara bakın:

* [Apache Hadoop için ML hizmetleri genel bakış](r-server-overview.md)
* [Apache Hadoop için ML hizmetleri kullanmaya başlayın](r-server-get-started.md)
* [HDInsight üzerinde ML Hizmetleri için Azure depolama seçenekleri](r-server-storage.md)

