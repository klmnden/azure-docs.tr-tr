---
title: İşletim sistemi düzeltme eki uygulama zamanlamasını Linux tabanlı HDInsight kümeleri - Azure için yapılandırma
description: İşletim sistemi düzeltme eki uygulama zamanlamasını Linux tabanlı HDInsight kümeleri için yapılandırmayı öğrenin.
author: omidm1
ms.author: omidm
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/24/2019
ms.openlocfilehash: cfbd68e66730fc338130bc16849fe0b2f4abd6be
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66244418"
---
# <a name="os-patching-for-hdinsight"></a>HDInsight için düzeltme eki uygulama işletim sistemi 

> [!IMPORTANT]
> Ubuntu görüntülerinde yayımlanmasını, 3 ay içinde yeni HDInsight kümesi oluşturmak için kullanılabilir hale gelir. Ocak 2019'den itibaren çalışan kümeleridir **değil** otomatik düzeltme eki uygulandı. Müşteriler, çalışan bir küme düzeltme eki için betik eylemleri veya başka mekanizmalar kullanmalısınız. Yeni oluşturulan küme, her zaman en son güvenlik düzeltme ekleri dahil olmak üzere en son güncelleştirmeleri, sahip olur.

## <a name="how-to-configure-the-os-patching-schedule-for-linux-based-hdinsight-clusters"></a>İşletim sistemi düzeltme eki uygulama zamanlamasını Linux tabanlı HDInsight kümeleri için yapılandırma
Bir HDInsight kümesinde sanal makineler, böylece önemli güvenlik düzeltme eklerinin yüklü bazen başlatılması gerekir. 

Bu makalede açıklanan betik eylemi kullanarak işletim sistemi gibi düzeltme eki uygulama zamanlamasını değiştirebilirsiniz:
1. Tam işletim sistemi güncelleştirmeleri yüklemek veya yalnızca güvenlik güncelleştirmelerini yükleyin
2. VM'yi yeniden başlatın

> [!NOTE]  
> Bu betik eylemi, yalnızca 1 Ağustos 2016'dan sonra oluşturulan Linux tabanlı HDInsight kümeleri ile çalışır. Yalnızca VM'ler yeniden başlatıldığı zaman düzeltme ekleri tarihinden itibaren geçerli olacaktır. Bu betik, tüm gelecek güncelleştirmelerden döngüleri güncelleştirmeleri otomatik olarak uygulanmaz. Her yeni güncelleştirme VM'yi yeniden başlatın ve güncelleştirmeleri yüklemek için uygulanması gerekiyor betiği çalıştırın.

## <a name="how-to-use-the-script"></a>Komut dosyası kullanma 

Ne zaman bu betiği kullanarak, aşağıdaki bilgileri gerektirir:
1. Betik konumu: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/os-patching-reboot-config.sh.  HDInsight bu URI'yi bulmak ve kümedeki tüm sanal makinelerde betiğini çalıştırmak için kullanır.
  
2. Betik uygulanan küme düğümü türlerini: baş düğüm, workernode, zookeeper. Bu betik, kümedeki tüm düğüm türleri uygulanması gerekir. Bir düğüm türü için uygulanmadı, sanal makineler, düğüm türü için güncelleştirilmez.


3.  Parametre: Bu betik, bir sayısal parametre kabul eder:

    | Parametre | Tanım |
    | --- | --- |
    | Yükleme tam işletim sisteminde güncelleştirmeler/güvenlik güncelleştirmeleri yükle |0 veya 1. 0 değeri, yalnızca tam işletim sistemi güncelleştirme 1 yüklerken güvenlik güncelleştirmeleri yükler. Hiçbir parametre sağlanmazsa varsayılan değer 0'dır. |

> [!NOTE]  
> Bu betik, mevcut bir kümeye uygularken kalıcı olarak işaretlemeniz gerekir. Aksi takdirde, ölçeklendirme işlemleri aracılığıyla oluşturulan tüm yeni düğümler, düzeltme eki uygulama zamanlamasını varsayılan kullanır.  Küme oluşturma işlemi kapsamında betiği uygularsanız, otomatik olarak kalıcıdır.


## <a name="next-steps"></a>Sonraki adımlar

Betik eylemi kullanarak belirli adımları görmek için aşağıdaki bölümlerdeki [özelleştirme Linux tabanlı HDInsight kümelerini betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md):

* [Küme oluşturma sırasında bir betik eylemi kullanın](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation)
* [Betik eylemi çalıştıran bir kümeye uygulama](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster)
