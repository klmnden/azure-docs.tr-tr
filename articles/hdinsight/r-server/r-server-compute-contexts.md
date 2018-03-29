---
title: R Server için içerik seçeneklerini hdınsight'ta - Azure işlem | Microsoft Docs
description: Farklı işlem bağlamı kullanılabilir seçenekler R Server kullanıcılarla hdınsight'ta hakkında bilgi edinin
services: HDInsight
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 0deb0b1c-4094-459b-94fc-ec9b774c1f8a
ms.service: HDInsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/22/2018
ms.author: nitinme
ms.openlocfilehash: 7d10f58c345eff334f40c0ec64d7b9427d6a70e0
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="compute-context-options-for-r-server-on-hdinsight"></a>Hdınsight'ta R Server için içerik seçeneklerini işlem

Microsoft Azure hdınsight'ta R Server işlem bağlamı ayarlayarak çağrıları nasıl yürütülür denetler. Bu makalede olup olmadığını ve nasıl yürütme kenar düğümüne veya Hdınsight küme çekirdeği arasında paralel birkaç ölçeklendirin belirtmek için kullanılabilir olan seçenekler özetlenmektedir.

Kümenin kenar düğümü kümeye bağlanın ve R komut dosyalarını çalıştırmak için uygun bir yer sağlar. Bir kenar düğümüne ile RevoScaleR parallelized dağıtılmış işlevleri kenar düğümü sunucu Çekirdeğinde çalıştırırken seçeneğiniz vardır. Ayrıca bunları küme düğümleri arasında RevoScaleR'ın eşlemesi Hadoop azaltmak kullanarak çalıştırabilirsiniz veya Spark işlem bağlamı.

## <a name="microsoft-r-server-on-azure-hdinsight"></a>Azure hdınsight'ta Microsoft R Server
[Microsoft Azure hdınsight'ta R Server](r-server-overview.md) R tabanlı analytics için en son özellikleri sağlar. HDFS kapsayıcısında depolanan verileri kullanabilir, [Azure Blob](../../storage/common/storage-introduction.md "Azure Blob Depolama") depolama hesabı, bir Data Lake deposu veya yerel Linux dosya sistemi. R Server açık kaynak R kurulu olduğundan, yapı R tabanlı uygulamalar 8000 + R açık kaynak paketlerinden birini uygulayabilirsiniz. Yordamları de kullanabilir [RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler), R Server'ın içerdiği Microsoft'un büyük veri analizi paket.  

## <a name="compute-contexts-for-an-edge-node"></a>Bir kenar düğümüne bağlamları işlem
Genel olarak, R Server edge düğümde çalışan bir R betiği içinde R yorumlayıcı bu düğüm üzerinde çalışır. Özel durumlar RevoScaleR işlevini çağırın bu adımlardır. RevoScaleR işlem bağlamı nasıl ayarlanacağını belirlenen bir işlem ortamında RevoScaleR çağrıları çalıştırın.  R betiği bir kenar düğümden çalıştırdığınızda, işlem bağlamının olası değerler şunlardır:

- Yerel sıralı (*yerel*)
- yerel paralel (*localpar*)
- Harita azaltın
- Spark

*Yerel* ve *localpar* seçenekleri yalnızca nasıl farklı **rxExec** çağrıları çalıştırılır. Her ikisi de diğer rx işlev çağrılarını paralel bir şekilde kullanılabilir tüm çekirdek arasında RevoScaleR kullanımla aksi belirtilmediği sürece yürütme **numCoresToUse** seçeneği, örneğin `rxOptions(numCoresToUse=6)`. Paralel yürütme seçeneklerini en iyi performans sunar.

Aşağıdaki tabloda çağrıları nasıl yürütülen ayarlamak için çeşitli işlem bağlamı seçenekler özetlenmektedir:

| İşlem bağlamı  | Ayarlama                      | Yürütme bağlamı                        |
| ---------------- | ------------------------------- | ---------------------------------------- |
| Yerel sıralı | rxSetComputeContext('local')    | Paralel birkaç ölçeklendirin yürütülmesi Çekirdeğinde kenar düğümü sunucu seri olarak yürütülen rxExec çağrıları dışında |
| Yerel paralel   | rxSetComputeContext('localpar') | Paralel birkaç ölçeklendirin yürütülmesi Çekirdeğinde kenar düğümü sunucusu |
| Spark            | RxSpark()                       | Dağıtılmış yürütülmesi Spark aracılığıyla düğümleri arasındaki HDI kümesi paralel birkaç ölçeklendirin |
| Harita azaltın       | RxHadoopMR()                    | Dağıtılmış yürütülmesi harita azaltmak aracılığıyla düğümleri arasındaki HDI kümesi paralel birkaç ölçeklendirin |

## <a name="guidelines-for-deciding-on-a-compute-context"></a>Bir işlem bağlamı karar verme yönergeleri

Paralel birkaç ölçeklendirin yürütme sağlayan üç seçenekleri seçtiğiniz analytics iş, boyutunu ve konumunu verilerinizin yapısını bağlıdır. Hangi kullanılacak bağlam işlem belirten Basit formül yoktur. Ancak, doğru seçim yapın veya en azından bir Kıyaslama çalıştırmadan önce seçimleri daraltmak yardımcı yardımcı olabilecek temel bazı ilkeler vardır. Bu ilkeler yönlendirmede şunları içerir:

- Yerel Linux dosya sistemi HDFS hızlıdır.
- Yinelenen çözümlemeler verileri yerel olmadığını ve XDF içinde olup olmadığını hızlıdır.
- Bir metin veri kaynağından küçük miktarda akış için tercih edilir. Veri miktarını büyükse, önce analiz için XDF dönüştürün.
- Kopyalama veya akış veri çözümleme için kenar düğümüne yükünü için çok büyük miktarlarda verinin yönetilemeyen haline gelir.
- Spark harita azaltmak için Hadoop analizi daha hızlıdır.

Bu ilkeler verildiğinde, aşağıdaki bölümlerde bazı genel kurallar için bir işlem bağlamında seçme altın sunar.

### <a name="local"></a>Yerel
* Analiz etmek için veri miktarını küçük ise ve yinelenen analiz gerektirmez, ardından bunu doğrudan çözümleme yordamı kullanarak akış *yerel* veya *localpar*.
* Çözümlemek için veri miktarını küçük veya orta ölçekli ve yinelenen analiz gerektiriyorsa, sonra yerel dosya sistemine kopyalamak için XDF almak ve aracılığıyla analiz *yerel* veya *localpar*.

### <a name="hadoop-spark"></a>Hadoop Spark
* Analiz etmek için veri miktarını büyükse, ardından bunu kullanarak bir Spark DataFrame içeri **RxHiveData** veya **RxParquetData**, veya hdfs'deki XDF (depolama birimi bir sorun olmadığı sürece) ve Spark işlem kullanarak analiz edin bağlamı.

### <a name="hadoop-map-reduce"></a>Hadoop harita azaltın
* Genellikle daha yavaş olduğundan yalnızca Spark işlem bağlamına sahip insurmountable bir sorunla karşılaşırsanız harita azaltmak işlem bağlamı kullanın.  

## <a name="inline-help-on-rxsetcomputecontext"></a>Satır içi Yardım rxSetComputeContext
Daha fazla bilgi ve örnekler RevoScaleR işlem bağlamı için R rxSetComputeContext yöntemi, örneğin yardımcı satır içi bakın:

    > ?rxSetComputeContext

Ayrıca başvurabilirsiniz [dağıtılmış bilgi işlem genel bakış](https://docs.microsoft.com/machine-learning-server/r/how-to-revoscaler-distributed-computing) içinde [Machine Learning sunucusu belgelerine](https://docs.microsoft.com/machine-learning-server/).

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede ve bunun nasıl yürütme kenar düğümüne veya Hdınsight küme çekirdeği arasında paralel birkaç ölçeklendirin belirtmek için kullanılabilir seçenekler hakkında öğrendiniz. R Server Hdınsight kümeleri ile kullanma hakkında daha fazla bilgi için aşağıdaki konulara bakın:

* [R Server Hadoop için genel bakış](r-server-overview.md)
* [R Server ile için Hadoop kullanmaya başlama](r-server-get-started.md)
* [HDInsight üzerinde R Server için Azure Depolama seçenekleri](r-server-storage.md)

