---
title: Daha yeni bir sürüme yükseltme Hdınsight küme-Azure | Microsoft Docs
description: Bilgi daha yeni bir sürüme yükseltme Hdınsight kümesi nasıl.
services: hdinsight
documentationcenter: ''
author: bhanupr
manager: asadk
editor: bhanupr
ms.assetid: 60eb573c-e639-4815-9fc6-ea8b106d8dbc
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: bhanupr
ms.openlocfilehash: d6cd0dd8b326f5340e5014bba4babc6dd945744d
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31406839"
---
# <a name="upgrade-hdinsight-cluster-to-a-newer-version"></a>Hdınsight kümesi daha yeni bir sürüme yükseltin
En son Hdınsight özelliklerden yararlanmak için Hdınsight kümeleri en son sürümüne yükseltilmesi önerilir. İzleyin, Hdınsight yükseltmek için yönergeleri sürümleri küme.

> [!NOTE]
> Hdınsight kümesi sürüm 3.2 ve 3.3 tarihten dolmak üzere. Hdınsight'ın desteklenen sürümünü hakkında daha fazla bilgi için bkz: [Hdınsight bileşen sürümü](hdinsight-component-versioning.md#supported-hdinsight-versions).
>
>

## <a name="upgrade-tasks"></a>Yükseltme görevleri
Hdınsight Küme yükseltme iş akışı aşağıdaki gibidir.

![Yükseltme iş akışı diyagramı](./media/hdinsight-upgrade-cluster/upgrade-workflow.png)

1. Hdınsight kümenize yükseltirken gerekli olabilecek değişiklikleri anlamak için bu belgenin her bölümünü okuyun.
2. Bir küme bir test/kalite güvence ortamı oluşturun. Küme oluşturma ile ilgili daha fazla bilgi için bkz: [Linux tabanlı Hdınsight kümeleri oluşturma hakkında bilgi edinin](hdinsight-hadoop-provision-linux-clusters.md)
3. Var olan işleri, veri kaynakları ve havuzlarını yeni ortamına kopyalayın. Bkz: [Test ortamı için kopyalama veri](hdinsight-migrate-from-windows-to-linux.md#copy-data-to-the-test-environment) daha fazla ayrıntı için.
4. İşleriniz yeni kümede beklendiği gibi çalıştığından emin olmak için doğrulama testleri gerçekleştirme.


Her şeyin beklendiği gibi çalıştığını doğruladıktan sonra geçiş kapalı kalma süresini zamanlayın. Bu kesinti sırasında aşağıdaki eylemleri gerçekleştirebilirsiniz:

1.  Küme düğümlerinde yerel olarak depolanan tüm geçici verileri yedekleyin. Örneğin, doğrudan bir baş düğüm üzerinde depolanan verileri varsa.
2.  Var olan küme silin.
3.  Bir küme aynı sanal alt ağdaki önceki küme tarafından kullanılan varsayılan veri deposu kullanarak en son (veya desteklenen) HDI sürümü ile oluşturun. Bu, var olan üretim verilerinizi karşı çalışmaya devam etmek yeni küme sağlar.
4.  Yedeklediğiniz herhangi bir geçici veri içeri aktarın.
5.  Başlangıç işleri/yeni küme kullanarak işlemeye devam et.

## <a name="next-steps"></a>Sonraki Adımlar
* [Linux tabanlı Hdınsight kümeleri oluşturma hakkında bilgi edinin](hdinsight-hadoop-provision-linux-clusters.md)
* [SSH kullanarak Hdınsight Bağlan](hdinsight-hadoop-linux-use-ssh-unix.md)
* [Ambari kullanarak Linux tabanlı bir kümeyi yönetmek](hdinsight-hadoop-manage-ambari.md)

