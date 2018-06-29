---
title: Hdınsight'ta - Azure ML Hizmetleri için içerik seçeneklerini işlem | Microsoft Docs
description: Farklı işlem bağlamı kullanılabilir seçenekler ML Hizmetleri ile kullanıcılar için hdınsight'ta hakkında bilgi edinin
services: hdinsight
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 0deb0b1c-4094-459b-94fc-ec9b774c1f8a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: nitinme
ms.openlocfilehash: 57480cef48182a56b315d7d6932883c485f5a7c8
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37050117"
---
# <a name="compute-context-options-for-ml-services-on-hdinsight"></a>Hdınsight üzerinde ML Hizmetleri için içerik seçeneklerini işlem

ML Hizmetleri Azure Hdınsight üzerinde işlem bağlamı ayarlayarak çağrıları nasıl yürütülür denetler. Bu makalede olup olmadığını ve nasıl yürütme kenar düğümüne veya Hdınsight küme çekirdeği arasında paralel birkaç ölçeklendirin belirtmek için kullanılabilir olan seçenekler özetlenmektedir.

Kümenin kenar düğümü kümeye bağlanın ve R komut dosyalarını çalıştırmak için uygun bir yer sağlar. Bir kenar düğümüne ile RevoScaleR parallelized dağıtılmış işlevleri kenar düğümü sunucu Çekirdeğinde çalıştırırken seçeneğiniz vardır. Ayrıca bunları küme düğümleri arasında RevoScaleR'ın eşlemesi Hadoop azaltmak kullanarak çalıştırabilirsiniz veya Spark işlem bağlamı.

## <a name="ml-services-on-azure-hdinsight"></a>Azure hdınsight'ta ML Hizmetleri
[Azure hdınsight'ta ML Hizmetleri](r-server-overview.md) R tabanlı analytics için en son özellikleri sağlar. HDFS kapsayıcısında depolanan verileri kullanabilir, [Azure Blob](../../storage/common/storage-introduction.md "Azure Blob Depolama") depolama hesabı, bir Data Lake deposu veya yerel Linux dosya sistemi. ML Hizmetleri açık kaynak R kurulu olduğundan, yapı R tabanlı uygulamalar 8000 + R açık kaynak paketlerinden birini uygulayabilirsiniz. Yordamları de kullanabilir [RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler), ML Services'da bulunan Microsoft'un büyük veri analizi paket.  

## <a name="compute-contexts-for-an-edge-node"></a>Bir kenar düğümüne bağlamları işlem
Genel olarak, ML Hizmetleri kümede kenar düğümü üzerinde çalışacak bir R betiği içinde R yorumlayıcı bu düğüm üzerinde çalışır. Özel durumlar RevoScaleR işlevini çağırın bu adımlardır. RevoScaleR işlem bağlamı nasıl ayarlanacağını belirlenen bir işlem ortamında RevoScaleR çağrıları çalıştırın.  R betiği bir kenar düğümden çalıştırdığınızda, işlem bağlamının olası değerler şunlardır:

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
Bu makalede ve bunun nasıl yürütme kenar düğümüne veya Hdınsight küme çekirdeği arasında paralel birkaç ölçeklendirin belirtmek için kullanılabilir seçenekler hakkında öğrendiniz. ML Hizmetleri Hdınsight kümeleri ile kullanma hakkında daha fazla bilgi için aşağıdaki konulara bakın:

* [Hadoop için ML hizmetlerine genel bakış](r-server-overview.md)
* [ML Hizmetleri için Hadoop kullanmaya başlama](r-server-get-started.md)
* [Hdınsight üzerinde ML Hizmetleri için Azure depolama seçenekleri](r-server-storage.md)

