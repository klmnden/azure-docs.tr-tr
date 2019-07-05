---
title: İşletim sistemi düzeltme eki uygulama zamanlamasını Linux tabanlı HDInsight kümeleri - Azure için yapılandırma
description: İşletim sistemi düzeltme eki uygulama zamanlamasını Linux tabanlı HDInsight kümeleri için yapılandırmayı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 07/01/2019
ms.openlocfilehash: a73866a8898042b546fa47d9c3d14ab4e58e9a12
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67503248"
---
# <a name="os-patching-for-hdinsight"></a>HDInsight için düzeltme eki uygulama işletim sistemi 

> [!IMPORTANT]
> Ubuntu görüntülerinde yayımlanmasını, üç ay içinde yeni HDInsight kümesi oluşturmak için kullanılabilir hale gelir. Ocak 2019'den itibaren çalışan kümeleridir **değil** otomatik düzeltme eki uygulandı. Müşteriler, çalışan bir küme düzeltme eki için betik eylemleri veya başka mekanizmalar kullanmalısınız. Yeni oluşturulan küme, her zaman en son güvenlik düzeltme ekleri dahil olmak üzere en son güncelleştirmeleri, sahip olur.

## <a name="how-to-configure-the-os-patching-schedule-for-linux-based-hdinsight-clusters"></a>İşletim sistemi düzeltme eki uygulama zamanlamasını Linux tabanlı HDInsight kümeleri için yapılandırma
Bir HDInsight kümesinde sanal makineler, böylece önemli güvenlik düzeltme eklerinin yüklü bazen başlatılması gerekir. 

Bu makalede açıklanan betik eylemlerini kullanarak işletim sistemi gibi düzeltme eki uygulama zamanlamasını değiştirebilirsiniz:
1. Tüm güncelleştirmeleri yüklemek veya yükleme çekirdek + security yalnızca güncelleştirir veya çekirdek güncelleştirmeleri yükleyin.
2. Hemen yeniden başlatma veya yeniden başlatma VM'deki zamanlama.

> [!NOTE]  
> Bu betik eylemleri yalnızca 1 Ağustos 2016'dan sonra oluşturulan Linux tabanlı HDInsight kümeleri ile çalışır. Yalnızca VM'ler yeniden başlatıldığı zaman düzeltme ekleri tarihinden itibaren geçerli olacaktır. Bu komut tüm gelecek güncelleştirmelerden döngüleri güncelleştirmeleri otomatik olarak uygulanmaz. Her yeni güncelleştirme VM'yi yeniden başlatın ve güncelleştirmeleri yüklemek için uygulanması gereken komut dosyaları çalıştırın.

## <a name="how-to-use-the-script"></a>Komut dosyası kullanma 

Bu komut dosyası kullanarak, aşağıdaki bilgileri gerektirir:
1. Yükleme-güncelleştirme-zamanlama-yeniden başlatma komut dosyası konumu: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/install-updates-schedule-reboots.sh.
    
   HDInsight bu URI'yi bulmak ve kümedeki tüm sanal makinelerde betiğini çalıştırmak için kullanır. Bu betik VM'yi yeniden başlatın ve güncelleştirmeleri yüklemek için seçenekler sağlar.
  
2. Zamanlama-yeniden başlatma komut dosyası konumu: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/schedule-reboots.sh.
    
   HDInsight bu URI'yi bulmak ve kümedeki tüm sanal makinelerde betiğini çalıştırmak için kullanır. Bu betik, VM yeniden başlatılır.
  
3. Betik uygulanan küme düğümü türlerini: baş düğüm, workernode, zookeeper. Bu betik, kümedeki tüm düğüm türleri uygulanması gerekir. Bir düğüm türü için uygulanmaz, ardından düğüm türü için sanal makineleri yeniden veya güncelleştirilmez değil.

4. Parametre: Güncelleştirmeleri zamanlama yeniden yükleme betiği, iki sayısal parametre kabul eder:

    | Parametre | Tanım |
    | --- | --- |
    | Yalnızca çekirdek güncelleştirmeleri yükleme / yükleme tüm güncelleştirmeleri/yükleme çekirdek + security yalnızca güncelleştirir |0 veya 1 veya 2. 0 değeri, tüm güncelleştirmeler ve 2 yükler çekirdek + güvenlik güncelleştirmeleri sadece 1 yüklemeleri sırasında yalnızca, çekirdek güncelleştirmeleri yükler. Hiçbir parametre sağlanmazsa, varsayılan değer 0'dır. |
    | Yeniden başlatma/etkinleştirme zamanlaması yeniden başlatma/etkinleştirme hemen yeniden başlatma |0 veya 1 veya 2. 0 değeri 1 zamanlamayı yeniden etkinleştirir ve hemen yeniden 2 sağlar ancak yeniden başlatma, devre dışı bırakır. Hiçbir parametre sağlanmazsa, varsayılan değer 0'dır. Kullanıcı giriş parametresi 1 girdi parametresi 2 gerekir. |
   
 5. Parametre: Zamanlama-yeniden başlatma betiği bir sayısal parametre kabul eder:

    | Parametre | Tanım |
    | --- | --- |
    | Zamanlamayı yeniden başlatma/etkinleştirme hemen yeniden etkinleştirme |1 veya 2. 1 değeri, zamanlamayı yeniden etkinleştirir (sonraki 12-24 saat içinde yeniden başlatma zamanlanmış) hemen 2 etkinleştirir (5 dakika olarak) yeniden başlatma sırasında. Hiçbir parametre sağlanmazsa, varsayılan değer 1'dir. |  

> [!NOTE] 
> Betik, mevcut bir kümeye uygularken kalıcı olarak işaretlemeniz gerekir. Aksi takdirde, ölçeklendirme işlemleri aracılığıyla oluşturulan tüm yeni düğümler, düzeltme eki uygulama zamanlamasını varsayılan kullanır.  Küme oluşturma işlemi kapsamında betiği uygularsanız, otomatik olarak kalıcıdır.


## <a name="next-steps"></a>Sonraki adımlar

Betik eylemi kullanarak belirli adımları görmek için aşağıdaki bölümlerdeki [özelleştirme Linux tabanlı HDInsight kümelerini betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md):

* [Küme oluşturma sırasında bir betik eylemi kullanın](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation)
* [Betik eylemi çalıştıran bir kümeye uygulama](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster)
