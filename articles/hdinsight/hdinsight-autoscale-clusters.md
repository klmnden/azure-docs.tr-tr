---
title: Azure HDInsight kümeleri (Önizleme) otomatik olarak ölçeklendirme
description: Kümeleri otomatik olarak ölçeklendirmek için HDInsight otomatik ölçeklendirme özelliğini kullanın
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/21/2019
ms.author: hrasheed
ms.openlocfilehash: bd1ffcfd915fe9ece683ec88d27f54b3a9214621
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55475695"
---
# <a name="automatically-scale-azure-hdinsight-clusters-preview"></a>Azure HDInsight kümeleri (Önizleme) otomatik olarak ölçeklendirme

Azure HDInsight'ın küme otomatik ölçeklendirme özelliği yukarı ve aşağı kümedeki çalışan düğümü sayısı otomatik olarak ölçeklendirir. yük önceden tanımlanmış bir aralık içindeki temel. Yeni bir HDInsight kümesi oluşturma sırasında çalışan düğümlerinin minimum ve maksimum sayısını ayarlayabilirsiniz. Otomatik ölçeklendirme sonra analytics kaynak gereksinimlerini yüklemek ve çalışan düğümleri sayısını ölçeklendirir izleyiciler yukarı veya aşağı uygun şekilde. Bu özellik için ek ücret yoktur.

## <a name="getting-started"></a>Başlarken

### <a name="create-a-cluster-with-the-azure-portal"></a>Azure portalı ile küme oluşturma

> [!Note]
> Otomatik ölçeklendirme şu anda yalnızca Azure HDInsight Hive, MapReduce ve Spark kümeleri için sürümü 3.6 desteklenir.

Otomatik ölçeklendirme özelliği etkinleştirmek için normal bir küme oluşturma işleminin bir parçası olarak aşağıdakileri yapın:

1. Seçin **özel (boyut, ayarları, uygulamalar)** yerine **hızlı oluşturma**.
2. Üzerinde **özel** 5. adım (**küme boyutu**) denetleyin **çalışan düğümü otomatik ölçeklendirme** onay kutusu.
3. İstenen değerleri girin:  

    * İlk **numarası, çalışan düğümleri**.  
    * **En az** çalışan düğümü sayısı.  
    * **En fazla** çalışan düğümü sayısı.  

![Çalışan düğümü otomatik ölçeklendirme seçeneğini etkinleştirin](./media/hdinsight-autoscale-clusters/usingAutoscale.png)

İlk alt düğüm sayısını, minimum ve maksimum, kapsamlı arasında olması gerekir. Oluşturulduğunda kümenin ilk boyutu bu değer tanımlar. Çalışan düğüm sayısı alt sınırı 0'dan büyük olmalıdır.

Her düğüm türü için VM türü seçtikten sonra tüm küme tahmini maliyet aralığının görmeye olacaktır. Ardından bu ayarları bütçenize uyacak şekilde ayarlayabilirsiniz.

Aboneliğiniz, her bölge için bir kapasite kotası vardır. Kapasite kota toplam çalışan düğümü sayısı ile birlikte, baş düğüm çekirdeği sayısını geçemez. Ancak, bu kotayı geçici bir sınırlıdır; her zaman kolayca artırılmış almak için bir destek bileti oluşturabilirsiniz.

> [!Note]  
> Toplam çekirdek kota sınırını aşarsanız, 'en fazla düğüm bu bölgede kullanılabilir çekirdek sayısı aşıldı, Lütfen başka bir bölge seçin veya Kotayı artırmak için desteğe başvurun.' belirten bir hata iletisi alırsınız.

Azure portalını kullanarak HDInsight küme oluşturma hakkında daha fazla bilgi için bkz. [Azure portalını kullanarak HDInsight oluşturma Linux tabanlı kümelerde](hdinsight-hadoop-create-linux-clusters-portal.md).  

### <a name="create-a-cluster-with-a-resource-manager-template"></a>Resource Manager şablonu ile küme oluşturma

Bir Azure Resource Manager şablonu ile bir HDInsight kümesi oluşturmak için bir `autoscale` düğüme `computeProfile`  >  `workernode` özelliklerini bölümle `minInstanceCount` ve `maxInstanceCount` json aşağıdaki kod parçacığında gösterildiği gibi.

```json
{                            
    "name": "workernode",
    "targetInstanceCount": 4,
    "autoscale": {
        "minInstanceCount": 2,
        "maxInstanceCount": 10
    },
    "hardwareProfile": {
        "vmSize": "Standard_D13_V2"
    },
    "osProfile": {
        "linuxOperatingSystemProfile": {
            "username": "[parameters('sshUserName')]",
            "password": "[parameters('sshPassword')]"
        }
    },
    "virtualNetworkProfile": null,
    "scriptActions": []
}
```

Resource Manager şablonları ile oluşturma hakkında daha fazla bilgi kümeleri için bkz: [Apache Hadoop kümeleri oluşturma HDInsight Resource Manager şablonları kullanarak](hdinsight-hadoop-create-linux-clusters-arm-templates.md).  

### <a name="enable-and-disable-autoscale-for-a-running-cluster"></a>Etkinleştirme ve otomatik ölçeklendirme, çalışan bir küme için devre dışı

Özel önizleme sırasında çalışan bir küme için otomatik ölçeklendirmeyi etkinleştirme desteklenmiyor. Küme oluşturma sırasında etkinleştirilmesi gerekir.

Özel önizlemede otomatik ölçeklendirmeyi devre dışı bırakma veya çalışan bir küme için otomatik ölçeklendirme ayarlarını değiştirme desteklenmez. Kümeyi silmek ve ayarlarını değiştirmek veya silmek için yeni bir küme oluşturmanız gerekir.

## <a name="monitoring"></a>İzleme

Küme ölçeği artırma ve ölçek kümesi ölçümleri bir parçası olarak geçmişi basılı görüntüleyebilirsiniz. Tüm ölçeklendirme eylemleri geçtiğimiz listeleyebilirsiniz gün, hafta veya daha uzun süre.

## <a name="how-it-works"></a>Nasıl çalışır?

### <a name="metrics-monitoring"></a>Ölçümleri izleme

Otomatik ölçeklendirme, sürekli olarak küme izler ve aşağıdaki ölçümleri toplar:

1. **Toplam CPU bekleyen**: Tüm bekleyen kapsayıcıları yürütülmesini başlatmak için gereken çekirdek sayısı toplam sayısı.
2. **Toplam bellek bekleyen**: Tüm kapsayıcıları bekleyen yürütülmesini başlatmak için gereken toplam bellek (MB cinsinden).
3. **Toplam CPU ücretsiz**: Etkin çalışan düğümlerinde kullanılmayan tüm çekirdek toplamı.
4. **Toplam boş bellek**: Toplam etkin çalışan düğümlerinde kullanılmayan bellek (MB cinsinden).
5. **Kullanılan bellek düğüm başına**: Bir çalışan düğümü üzerindeki yükü. Üzerinde 10 GB'a kadar bellek kullanılır, çalışan düğüme bir çalışan kullanılan belleğin 2 GB daha fazla yük altında kabul edilir.
6. **Düğüm başına uygulama ana sunucu sayısı**: Bir çalışan düğümü üzerinde çalışan uygulama Yöneticisi (AM) kapsayıcı sayısı. 02: 00 kapsayıcıları barındıran bir çalışan düğümü 0'da kapsayıcıları barındıran bir çalışan düğümü daha önemli kabul edilir.

Yukarıdaki ölçümleri, 60 saniyede denetlenir. Otomatik ölçeklendirme ölçek oluşturan ve bu ölçümleri temel kararlar aşağı ölçeklendirin.

### <a name="cluster-scale-up"></a>Küme ölçeği artırma

Aşağıdaki koşullar tespit edildiğinde, otomatik ölçeklendirme isteği ölçeği verecek:

* Toplam CPU bekleyen 1 dakikadan fazla bir süre için toplam boş CPU büyüktür.
* Toplam bellek bekleyen 1 dakikadan fazla bir süre için toplam boş belleğin büyüktür.

Belirli bir sayıda yeni çalışan düğümlerindeki geçerli CPU ve bellek gereksinimlerini karşılamak ve ardından bu yeni çalışan düğüm sayısı ekler isteği ölçeği vermek için gerekli olup olmadığını hesaplama yapar.

### <a name="cluster-scale-down"></a>Küme ölçeği azaltma

Aşağıdaki koşullar tespit edildiğinde, otomatik ölçeklendirme isteği aşağı ölçeği verecek:

* Toplam CPU bekleyen toplam boş CPU 10 dakikadan daha küçüktür.
* Toplam bellek bekleyen toplam boş belleğin 10 dakikadan daha küçüktür.

Geçerli CPU ve bellek gereksinimlerini yanı sıra, düğüm başına AM kapsayıcıların sayısına bağlı olarak, otomatik ölçeklendirme hangi düğümleri kaldırma için potansiyel adaylar belirterek belirli bir sayıda düğüm kaldırılması için istekte verecek. Varsayılan olarak, iki düğüm bir döngüsü içinde kaldırılacak.

## <a name="next-steps"></a>Sonraki adımlar

* Kümeleri el ölçeklendirme için en iyi yöntemler hakkında bilgi edinin [en iyi ölçeklendirme uygulamaları](hdinsight-scaling-best-practices.md)
