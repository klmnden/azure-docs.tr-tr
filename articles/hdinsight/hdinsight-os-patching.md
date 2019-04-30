---
title: İşletim sistemi düzeltme eki uygulama zamanlamasını Linux tabanlı HDInsight kümeleri - Azure için yapılandırma
description: İşletim sistemi düzeltme eki uygulama zamanlamasını Linux tabanlı HDInsight kümeleri için yapılandırmayı öğrenin.
author: omidm1
ms.author: omidm
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/24/2019
ms.openlocfilehash: a887d6c69b9fa80f3144434e72a097e80d123a1b
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63766484"
---
# <a name="os-patching-for-hdinsight"></a>HDInsight için düzeltme eki uygulama işletim sistemi 

> [!IMPORTANT]
> Ubuntu görüntülerinde yayımlanmasını, 3 ay içinde yeni HDInsight kümesi oluşturmak için kullanılabilir hale gelir. Ocak 2019'den itibaren çalışan kümeleridir **değil** otomatik düzeltme eki uygulandı. Müşteriler, çalışan bir küme düzeltme eki için betik eylemleri veya başka mekanizmalar kullanmalısınız. Yeni oluşturulan küme, her zaman en son güvenlik düzeltme ekleri dahil olmak üzere en son güncelleştirmeleri, sahip olur.

## <a name="how-to-configure-the-os-patching-schedule-for-linux-based-hdinsight-clusters"></a>İşletim sistemi düzeltme eki uygulama zamanlamasını Linux tabanlı HDInsight kümeleri için yapılandırma
Bir HDInsight kümesinde sanal makineler, böylece önemli güvenlik düzeltme eklerinin yüklü bazen başlatılması gerekir. 

Bu makalede açıklanan betik eylemi kullanarak işletim sistemi gibi düzeltme eki uygulama zamanlamasını değiştirebilirsiniz:
1. Etkinleştirmek veya devre dışı otomatik yeniden başlatma
2. Kümesi sıklığını (gün önyüklemeler) ile yeniden başlatır.
3. Bir yeniden başlatma işlemi gerçekleştiğinde haftanın günü ayarlayın

> [!NOTE]  
> Bu betik eylemi, yalnızca 1 Ağustos 2016'dan sonra oluşturulan Linux tabanlı HDInsight kümeleri ile çalışır. Yalnızca VM'ler yeniden başlatıldığı zaman düzeltme ekleri tarihinden itibaren geçerli olacaktır. 

## <a name="how-to-use-the-script"></a>Komut dosyası kullanma 

Ne zaman bu betiği kullanarak, aşağıdaki bilgileri gerektirir:
1. Betik konumu: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv01/os-patching-reboot-config.sh.  HDInsight bu URI'yi bulmak ve kümedeki tüm sanal makinelerde betiğini çalıştırmak için kullanır.
  
2. Betik uygulanan küme düğümü türlerini: baş düğüm, workernode, zookeeper. Bu betik, kümedeki tüm düğüm türleri uygulanması gerekir. Bir düğüm türü için uygulanmamış durumunda, sanal makineler, düğüm türü için önceki düzeltme eki uygulama zamanlamasını kullanmayı sürdürecektir.


3.  Parametre: Bu betik, üç sayısal parametre kabul eder:

    | Parametre | Tanım |
    | --- | --- |
    | Otomatik yeniden başlatmaları etkinleştirmek/devre dışı bırak |0 veya 1. 0 değeri 1 otomatik yeniden başlatmaları etkinleştirse otomatik yeniden başlatmaları devre dışı bırakır. |
    | Sıklık |7-90 (sınırlar dahil). Yeniden başlatma gerektiren düzeltme ekleri için sanal makineleri yeniden başlatmadan önce beklenecek gün sayısı. |
    | Haftanın günü |1 ile 7 (sınırlar dahil). 1 değeri, yeniden başlatma Pazartesi günü olmamalıdır, 7 parametrelerini kullanarak bir Sunday.For örnek belirtir 1 60 2 sonuçları otomatik yeniden Başlatmalara 60 günde (en fazla) Salı günü. |
    | Kalıcılık |Betik eylemi mevcut bir kümeye uygularken, komut dosyasını kalıcı olarak işaretleyebilirsiniz. Küme ölçeklendirme işlemlerinin aracılığıyla eklenen yeni workernodes kalıcı duruma getirilmiş betiklerin uygulanır. |

> [!NOTE]  
> Bu betik, mevcut bir kümeye uygularken kalıcı olarak işaretlemeniz gerekir. Aksi takdirde, ölçeklendirme işlemleri aracılığıyla oluşturulan tüm yeni düğümler, düzeltme eki uygulama zamanlamasını varsayılan kullanır.  Küme oluşturma işlemi kapsamında betiği uygularsanız, otomatik olarak kalıcıdır.

## <a name="next-steps"></a>Sonraki adımlar

Betik eylemi kullanarak belirli adımları görmek için aşağıdaki bölümlerdeki [özelleştirme Linux tabanlı HDInsight kümelerini betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md):

* [Küme oluşturma sırasında bir betik eylemi kullanın](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation)
* [Betik eylemi çalıştıran bir kümeye uygulama](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster)
