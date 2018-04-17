---
title: Linux tabanlı Hdınsight kümeleri - Azure zamanlama düzeltme eki uygulama işletim sistemi yapılandırma | Microsoft Docs
description: Linux tabanlı Hdınsight kümeleri için zamanlama düzeltme eki uygulama işletim sistemi yapılandırma konusunda bilgi edinin.
services: hdinsight
documentationcenter: ''
author: bprakash
manager: asadk
editor: bprakash
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 03/21/2017
ms.author: bhanupr
ms.openlocfilehash: 42771b9ff0f177b6b31f626d1dd2d07046a53965
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="os-patching-for-hdinsight"></a>İşletim sistemi Hdınsight için düzeltme eki uygulama 
Yönetilen bir Hadoop hizmet olarak Hdınsight Hdınsight kümeleri tarafından kullanılan temel VM'ler OS düzeltme eki uygulama mvc'deki. 1 Ağustos 2016 itibariyle biz Linux tabanlı Hdınsight kümeleri (sürüm 3.4 veya büyük) için konuk işletim sistemi düzeltme eki uygulama ilkesi değişti. Düzeltme eki uygulama nedeniyle yeniden başlatma sayısını önemli ölçüde azaltmak için yeni ilke belirtilir. Yeni ilke düzeltme eki sanal makineleri (VM'ler) Linux kümeleri her Pazartesi ya da verilmiş bir küme düğümleri arasında 00: 00 UTC aşamalı bir şekilde başlayarak Perşembe devam eder. Ancak, belirli bir VM yalnızca en fazla 30 konuk işletim sistemi düzeltme eki uygulama nedeniyle günde bir kez yeniden başlatılır. Ayrıca, ilk yeniden başlatma yeni oluşturulan bir küme için küme oluşturma tarihten itibaren 30 gün daha erken yapılmaz. Sanal makineleri yeniden sonra düzeltme eklerini etkili olacaktır.

## <a name="how-to-configure-the-os-patching-schedule-for-linux-based-hdinsight-clusters"></a>Linux tabanlı Hdınsight kümeleri için zamanlama düzeltme eki uygulama işletim sistemi yapılandırma
Hdınsight kümesi'nde sanal makinelerin önemli güvenlik düzeltme ekleri yüklenmemiş böylece bazen yeniden başlatılması gerekir. 1 Ağustos 2016 itibariyle yeni Linux tabanlı Hdınsight kümeleri (sürüm 3.4 veya daha büyük) yeniden şu zamanlamaya kullanarak:

1. Küme içindeki bir sanal makine yalnızca düzeltme ekleri için en fazla bir kez bir 30 günlük süre içinde yeniden başlatabilirsiniz.
2. 00: 00 UTC başlayarak yeniden başlatma gerçekleşir.
3. Küme yeniden başlatma işlemi sırasında hala kullanılabilir olacak şekilde yeniden başlatma işlemi kümedeki sanal makineler arasında aşamalı.
4. İlk yeniden başlatma yeni oluşturulan bir küme için küme oluşturulma tarihinden sonra 30 günden daha erken yapılmaz.

Bu makalede açıklanan betik eylemi kullanarak, aşağıdaki gibi zamanlama düzeltme eki uygulama işletim sistemi değiştirebilirsiniz:
1. Etkinleştirmek veya devre dışı otomatik yeniden başlatma
2. Kümesi sıklığı (gün yeniden başlatmalar arasında) yeniden başlatır
3. Yeniden başlatma oluştuğunda haftanın gününü ayarlama

> [!NOTE]
> Bu betik eylemi yalnızca 1 Ağustos 2016'dan sonra oluşturulan Linux tabanlı Hdınsight kümeleri ile çalışır. Yalnızca sanal makineleri yeniden, düzeltme ekleri etkili olur. 
>

## <a name="how-to-use-the-script"></a>Komut dosyası kullanma 

Ne zaman bu komut dosyasını kullanarak, aşağıdaki bilgileri gerektirir:
1. Komut dosyası konumu: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv01/os-patching-reboot-config.sh.  Hdınsight bu URI bulmak ve kümedeki tüm sanal makinelerde komut dosyasını çalıştırmak için kullanır.
  
2. Komut dosyası uygulandığı Küme düğüm türleri: headnode, workernode, zookeeper. Bu komut, kümedeki tüm düğüm türleri uygulanması gerekir. Bir düğüm türü uygulanmamış durumunda, bu düğüm türü için sanal makineleri önceki düzeltme eki uygulama zamanlamayı kullan devam eder.


3.  Parametre: Bu komut dosyasını üç sayısal parametreleri kabul eder:

    | Parametre | Tanım |
    | --- | --- |
    | Otomatik yeniden başlatmalar etkinleştir/devre dışı bırak |0 veya 1. Otomatik yeniden başlatmalar 1 olanak sağlarken otomatik yeniden başlatmalar 0 değerini devre dışı bırakır. |
    | Sıklık |7-90 (dahil). Sanal makineler için yeniden başlatılması düzeltme ekleri yeniden başlatmadan önce beklenecek gün sayısı. |
    | Haftanın günü |1 ile 7 (dahil). 1 değeri, yeniden başlatma Pazartesi günü gerçekleşeceğini, 7 parametrelerini kullanarak bir Sunday.For örnek belirtir 1 60 2 sonuçları otomatik olarak yeniden başlatılır 60 günde (en çok) Salı günü. |
    | Kalıcılığı |Betik eylemi varolan bir kümeye uygularken, komut dosyasını kalıcı olarak işaretleyebilirsiniz. Yeni workernodes işlemleri ölçeklendirme aracılığıyla kümeye eklendiğinde kalıcı betikleri uygulanır. |

> [!NOTE]
> Bu komut dosyasını varolan bir kümeye uygularken kalıcı olarak işaretlemeniz gerekir. Aksi takdirde, ölçeklendirme işlemleri aracılığıyla oluşturulan tüm yeni düğümler zamanlama düzeltme eki uygulama varsayılan kullanır.
Küme oluşturma işleminin bir parçası olarak komut dosyası uygularsanız, otomatik olarak kalıcıdır.
>

## <a name="next-steps"></a>Sonraki adımlar

Betik eylemi kullanarak belirli adımlar görmek için aşağıdaki bölümlerde [özelleştirme Linuz tabanlı Hdınsight kümeleri betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md):

* [Küme oluşturma sırasında bir betik eylemi kullanın](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation)
* [Betik eylemi çalıştıran bir kümeye uygulama](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster)
