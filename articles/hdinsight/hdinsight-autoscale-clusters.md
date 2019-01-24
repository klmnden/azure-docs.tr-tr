---
title: Azure HDInsight kümeleri otomatik olarak ölçeklendirme
description: Kümeleri otomatik olarak ölçeklendirmek için HDInsight otomatik ölçeklendirme özelliğini kullanın
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/21/2019
ms.author: hrasheed
ms.openlocfilehash: 043c83e2039d87b1650ba17f770ce16a2ad2c13d
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54811171"
---
# <a name="automatically-scale-azure-hdinsight-clusters"></a>Azure HDInsight kümeleri otomatik olarak ölçeklendirme

Azure HDInsight'ın küme otomatik ölçeklendirme özelliği yukarı ve aşağı kümedeki çalışan düğümü sayısı otomatik olarak ölçeklendirir. yük önceden tanımlanmış bir aralık içindeki temel. Yeni bir HDInsight kümesi oluşturma sırasında çalışan düğümlerinin minimum ve maksimum sayısını ayarlayabilirsiniz. Otomatik ölçeklendirme sonra analytics kaynak gereksinimlerini yüklemek ve çalışan düğümleri sayısını ölçeklendirir izleyiciler yukarı veya aşağı uygun şekilde. Bu özellik için ek ücret yoktur.

## <a name="getting-started"></a>Başlarken

### <a name="create-cluster-with-azure-portal"></a>Azure portalı ile küme oluşturma

> [!Note]
> Otomatik ölçeklendirme şu anda yalnızca Azure HDInsight Hive, MapReduce ve Spark kümeleri için sürümü 3.6 desteklenir.

Bağlantısındaki [Azure portalını kullanarak HDInsight oluşturma Linux tabanlı kümelerde](hdinsight-hadoop-create-linux-clusters-portal.md) ve 5. adım ulaştığında **küme boyutu**seçin **otomatik ölçeklendirme (Önizleme) çalışan düğümü** Aşağıda gösterildiği gibi. 

![Çalışan düğümü otomatik ölçeklendirme seçeneğini etkinleştirin](./media/hdinsight-autoscale-clusters/worker-node-autoscale-option.png)

Bu seçeneği işaretlediğinizde, belirtebilirsiniz:

* Çalışan düğümü başlangıç sayısı
* Çalışan düğüm sayısı alt sınırı
* Çalışan düğümlerinin sayısı

İlk alt düğüm sayısını, minimum ve maksimum, kapsamlı arasında olması gerekir. Oluşturulduğunda kümenin ilk boyutu bu değer tanımlar. Çalışan düğüm sayısı alt sınırı 0'dan büyük olmalıdır.

Her düğüm türü için VM türü seçtikten sonra tüm küme tahmini maliyet aralığının görmeye olacaktır. Ardından bu ayarları bütçenize uyacak şekilde ayarlayabilirsiniz.

Aboneliğiniz, her bölge için bir kapasite kotası vardır. Kapasite kota toplam çalışan düğümü sayısı ile birlikte, baş düğüm çekirdeği sayısını geçemez. Ancak, bu kotayı geçici bir sınırlıdır; her zaman kolayca artırılmış almak için bir destek bileti oluşturabilirsiniz.

> [!Note]
> Toplam çekirdek kota sınırını aşarsanız, 'en fazla düğüm bu bölgede kullanılabilir çekirdek sayısı aşıldı, Lütfen başka bir bölge seçin veya Kotayı artırmak için desteğe başvurun.' belirten bir hata iletisi alırsınız.

### <a name="create-cluster-with-an-resource-manager-template"></a>Bir Resource Manager şablonu ile küme oluşturma

Bir Resource Manager şablonu ile bir HDInsight kümesi oluşturduğunuzda, aşağıdaki ayarları "computeProfile" "çalışan düğümü" bölümünde eklemeniz gerekir:

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

### <a name="enable-and-disabling-autoscale-for-a-running-cluster"></a>Etkinleştirme ve devre dışı bırakmak için otomatik ölçeklendirmeyi, çalışan bir küme

Özel önizleme sırasında çalışan bir küme için otomatik ölçeklendirmeyi etkinleştirme desteklenmiyor. Küme oluşturma sırasında etkinleştirilmesi gerekir.

Özel önizlemede otomatik ölçeklendirmeyi devre dışı bırakma veya çalışan bir küme için otomatik ölçeklendirme ayarlarını değiştirme desteklenmez. Kümeyi silmek ve ayarlarını değiştirmek veya silmek için yeni bir küme oluşturmanız gerekir.

## <a name="monitoring"></a>İzleme

Küme ölçek kümesi ölçümleri bir parçası olarak geçmişi yukarı ve aşağı görüntüleyebilirsiniz. Tüm ölçek eylemleri geçtiğimiz listeleyebilirsiniz gün, hafta veya daha uzun süre.

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

N yeni çalışan düğümlerindeki geçerli CPU ve bellek gereksinimlerini karşılamak ve ardından N yeni çalışan düğümlerindeki isteyerek ölçeği isteği vermek için gerekli olup olmadığını hesaplama yapar.

### <a name="cluster-scale-down"></a>Küme ölçeği azaltma

Aşağıdaki koşullar tespit edildiğinde, otomatik ölçeklendirme isteği aşağı ölçeği verecek:

* Toplam CPU bekleyen toplam boş CPU 10 dakikadan daha küçüktür.
* Toplam bellek bekleyen toplam boş belleğin 10 dakikadan daha küçüktür.

Geçerli CPU ve bellek gereksinimlerini yanı sıra, düğüm başına AM kapsayıcıların sayısına bağlı olarak, otomatik ölçeklendirme hangi düğümleri kaldırma için potansiyel adaylar belirten N düğümleri kaldırma isteği göndereceksiniz. Varsayılan olarak, iki düğüm bir döngüsü içinde kaldırılacak.

## <a name="next-steps"></a>Sonraki adımlar

* Kümeleri el ölçeklendirme için en iyi yöntemler hakkında bilgi edinin [en iyi ölçeklendirme uygulamaları](hdinsight-scaling-best-practices.md)
