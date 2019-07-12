---
title: İşletim sistemi düzeltme eki uygulama zamanlamasını Linux tabanlı HDInsight kümeleri - Azure için yapılandırma
description: İşletim sistemi düzeltme eki uygulama zamanlamasını Linux tabanlı HDInsight kümeleri için yapılandırmayı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 07/01/2019
ms.openlocfilehash: efe74618b269000749f7ba6c24d35903e540dcfb
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67657047"
---
# <a name="configure-the-os-patching-schedule-for-linux-based-hdinsight-clusters"></a>İşletim sistemi düzeltme eki uygulama zamanlamasını Linux tabanlı HDInsight kümeleri için yapılandırma 

> [!IMPORTANT]
> Ubuntu görüntülerinde yayımlanmasını, üç ay içinde yeni bir Azure HDInsight küme oluşturma için kullanılabilir hale gelir. Ocak 2019'den itibaren otomatik düzeltme eki çalıştıran kümeleri değildir. Müşteriler, çalışan bir küme düzeltme eki için betik eylemleri veya başka mekanizmalar kullanmalısınız. Yeni oluşturulan küme, her zaman en son güvenlik düzeltme ekleri dahil olmak üzere en son güncelleştirmeleri, sahip olur.

Bazen, önemli güvenlik düzeltmelerini yüklemek için bir HDInsight kümesinde sanal makineler (VM) yeniden başlatmalısınız.

Bu makalede açıklanan betik eylemlerini kullanarak işletim sistemi gibi düzeltme eki uygulama zamanlamasını değiştirebilirsiniz:

1. Tüm güncelleştirmeleri yükleyin ya da yalnızca çekirdek + güvenlik güncelleştirmeleri veya çekirdek güncelleştirmeleri yükleyin.
2. Hemen bir yeniden başlatma yapmanız veya VM üzerinde bir yeniden zamanlayın.

> [!NOTE]  
> Bu makalede açıklanan betik eylemleri, 1 Ağustos 2016'dan sonra oluşturulan Linux tabanlı HDInsight kümeleri ile çalışır. Düzeltme ekleri, yalnızca sanal makineleri yeniden başlatıldıktan sonra etkili olur.
> Betik eylemleri, tüm gelecek güncelleştirmelerden döngüleri güncelleştirmeleri otomatik olarak uygulanmaz. Yeni güncelleştirmeler güncelleştirmeleri yüklemek ve VM'yi yeniden uygulanması gereken her zaman komut dosyasını çalıştırın.

## <a name="add-information-to-the-script"></a>Komut dosyası bilgilerini ekleyin

Betik kullanarak, aşağıdaki bilgileri gerektirir:

- Yükleme-güncelleştirme-zamanlama-yeniden başlatma komut dosyası konumu: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/install-updates-schedule-reboots.sh.
    
   HDInsight, bu URI'yi bulmak ve kümedeki tüm sanal makineler komut dosyasını çalıştırmak için kullanır. Bu betik VM'yi yeniden başlatın ve güncelleştirmeleri yüklemek için seçenekler sağlar.
  
- Zamanlama-yeniden başlatma komut dosyası konumu: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/schedule-reboots.sh.
    
   HDInsight, bu URI'yi bulmak ve kümedeki tüm sanal makineler komut dosyasını çalıştırmak için kullanır. Bu betik, VM'yi yeniden başlatır.
  
- Betik uygulanan küme düğümü baş düğüm, workernode ve zookeeper türleridir. Betik kümedeki tüm düğüm türleri için geçerlidir. Betik, bir düğüm türü için uygulanmaz, bu düğüm türü için Vm'leri güncelleştirildi veya yeniden olmaz.

- Güncelleştirmeleri zamanlama yeniden yükleme betiği, iki sayısal parametre kabul eder:

    | Parametre | Tanım |
    | --- | --- |
    | Yalnızca çekirdek güncelleştirmeleri yükleme / yükleme tüm güncelleştirmeleri/yükleme çekirdek + security yalnızca güncelleştirir|0, 1 veya 2. 0 değeri, yalnızca çekirdek güncelleştirmeleri yükler. 1 değeri, tüm güncelleştirmeleri ve yalnızca çekirdek 2 yükler ve güvenlik güncelleştirmeleri yükler. Hiçbir parametre sağlanmazsa, varsayılan değer 0'dır. |
    | Yeniden başlatma/etkinleştirme zamanlaması yeniden başlatma/etkinleştirme hemen yeniden başlatma |0, 1 veya 2. 0 değeri yeniden devre dışı bırakır. 1 değeri, zamanlamayı yeniden etkinleştirir ve hemen yeniden 2 sağlar. Hiçbir parametre sağlanmazsa, varsayılan değer 0'dır. Kullanıcı giriş parametresi 1 girdi parametresi 2 değiştirmeniz gerekir. |
   
 - Zamanlama-yeniden başlatma betiği bir sayısal parametre kabul eder:

    | Parametre | Tanım |
    | --- | --- |
    | Zamanlamayı yeniden başlatma/etkinleştirme hemen yeniden etkinleştirme |1 veya 2. 1 değeri, zamanlamayı yeniden başlatma (12-24 saat içindeki zamanlanmış) sağlar. Değeri 2'dir (5 dakika cinsinden) hemen yeniden etkinleştirir. Parametre belirtilmezse, varsayılan değer 1'dir. |  

> [!NOTE]
> Bir komut dosyası, mevcut bir kümeye uyguladıktan sonra kalıcı olarak işaretlemeniz gerekir. Aksi takdirde, ölçeklendirme işlemleri aracılığıyla oluşturulan tüm yeni düğümler, düzeltme eki uygulama zamanlamasını varsayılan kullanır. Küme oluşturma işlemi kapsamında betiği uygularsanız, otomatik olarak kaydetti.


## <a name="next-steps"></a>Sonraki adımlar

Betik eylemlerini kullanarak belirli adımları görmek için aşağıdaki bölümlerdeki [özelleştirme Linux tabanlı HDInsight kümelerini betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md):

* [Küme oluşturma sırasında bir betik eylemi kullanın](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation)
* [Betik eylemi çalıştıran bir kümeye uygulama](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster)
