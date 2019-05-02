---
title: Daha yeni bir sürüme yükseltme HDInsight kümesi-Azure
description: Bilgi daha yeni bir sürüme yükseltme HDInsight kümesine nasıl.
ms.service: hdinsight
author: omidm1
ms.author: omidm
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/04/2017
ms.openlocfilehash: b5048266fe17bc16fba8228f7cc17d0ee9f3bc0b
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64714767"
---
# <a name="upgrade-hdinsight-cluster-to-a-newer-version"></a>HDInsight kümesini yeni sürüme yükseltme
En son HDInsight özelliklerden yararlanmak için HDInsight kümeleri en son sürüme yükseltilmesi özellikle önerilir. İzleyin, HDInsight'ı yükseltmek için yönergeleri küme sürümleri.

> [!NOTE]  
> HDInsight'ın desteklenen sürümleri hakkında daha fazla bilgi için bkz: [HDInsight bileşen sürümü](hdinsight-component-versioning.md#supported-hdinsight-versions).

## <a name="upgrade-tasks"></a>Yükseltme görevleri
HDInsight kümesini yükseltme iş akışı aşağıdaki gibidir.

![Yükseltme iş akışı diyagramı](./media/hdinsight-upgrade-cluster/upgrade-workflow.png)

1. Bu belgede, HDInsight kümenizi yükseltme sırasında gerekli olabilecek değişiklikleri anlamak için her bir bölümünü okuyun.
2. Bir küme bir test/kalite güvencesi ortamı oluşturun. Küme oluşturma ile ilgili daha fazla bilgi için bkz: [Linux tabanlı HDInsight kümeleri oluşturmayı öğrenin](hdinsight-hadoop-provision-linux-clusters.md)
3. Mevcut işleri, veri kaynağı ve havuz yeni ortama kopyalayın. Bkz: [Test ortamı için kopya veri](hdinsight-migrate-from-windows-to-linux.md#copy-data-to-the-test-environment) daha fazla ayrıntı için.
4. İşlerinizi yeni kümede beklendiği gibi çalıştığından emin olmak için doğrulama sınaması gerçekleştirin.


Her şeyin beklendiği gibi çalıştığını doğruladıktan sonra geçiş kapalı kalma süresi zamanlayın. Bu kesinti sırasında aşağıdaki eylemleri gerçekleştirin:

1.  Küme düğümleri üzerinde yerel olarak depolanan tüm geçici verileri yedekleyin. Örneğin, doğrudan bir baş düğüm üzerinde depolanan verileri varsa.
2.  Var olan kümeyi silin.
3.  Bir küme önceki kümeyle aynı varsayılan veri deposunu kullanarak en son (veya desteklenen) HDI sürümü ile aynı sanal ağ alt ağda oluşturun. Bu, var olan üretim verileriniz çalışmaya devam etmek yeni kümeye sağlar.
4.  Yedeklediğiniz herhangi bir geçici veri içeri aktarın.
5.  Başlangıç işleri/yeni küme kullanarak işleme devam edin.

## <a name="next-steps"></a>Sonraki Adımlar
* [Linux tabanlı HDInsight kümeleri oluşturmayı öğrenin](hdinsight-hadoop-provision-linux-clusters.md)
* [SSH kullanarak HDInsight’a bağlanma](hdinsight-hadoop-linux-use-ssh-unix.md)
* [Apache Ambari kullanarak Linux tabanlı küme yönetme](hdinsight-hadoop-manage-ambari.md)

