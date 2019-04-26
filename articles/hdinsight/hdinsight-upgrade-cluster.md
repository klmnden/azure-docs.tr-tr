---
title: Daha yeni bir sürüme yükseltme HDInsight kümesi-Azure
description: Bilgi daha yeni bir sürüme yükseltme HDInsight kümesine nasıl.
services: hdinsight
ms.service: hdinsight
author: omidm1
ms.author: v-yiso
ms.custom: hdinsightactive
ms.topic: conceptual
origin.date: 04/04/2017
ms.date: 02/04/2019
ms.openlocfilehash: 266de596633201b5590a6433d078cc05f2a267b4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60306175"
---
# <a name="upgrade-hdinsight-cluster-to-a-newer-version"></a>HDInsight kümesini yeni sürüme yükseltme
En son HDInsight özelliklerden yararlanmak için HDInsight kümeleri en son sürüme yükseltilmesi özellikle önerilir. İzleyin, HDInsight'ı yükseltmek için yönergeleri küme sürümleri.

> [!NOTE]
> HDInsight'ın desteklenen sürümleri hakkında daha fazla bilgi için bkz: [HDInsight bileşen sürümü](hdinsight-component-versioning.md#supported-hdinsight-versions).
>
>

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

