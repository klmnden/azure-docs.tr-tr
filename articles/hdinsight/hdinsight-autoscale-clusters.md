---
title: Azure HDInsight kümeleri (Önizleme) otomatik olarak ölçeklendirme
description: Kümeleri otomatik olarak ölçeklendirmek için HDInsight otomatik ölçeklendirme özelliğini kullanın
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/11/2019
ms.author: hrasheed
ms.openlocfilehash: 11828b3b056519d0ebe3233f078c6b3f6fc2ea1c
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59997504"
---
# <a name="automatically-scale-azure-hdinsight-clusters-preview"></a>Azure HDInsight kümeleri (Önizleme) otomatik olarak ölçeklendirme

>[!Important]
>HDInsight otomatik ölçeklendirme özelliği, şu anda Önizleme aşamasındadır. Lütfen bir e-posta gönderin hdiautoscalepm@microsoft.com aboneliğiniz için etkin otomatik ölçeklendirme sağlamak için.

Azure HDInsight'ın küme otomatik ölçeklendirme özelliği yukarı ve aşağı kümedeki çalışan düğümü sayısı otomatik olarak ölçeklendirir. yük önceden tanımlanmış bir aralık içindeki temel. Yeni bir HDInsight kümesi oluşturma sırasında çalışan düğümlerinin minimum ve maksimum sayısını ayarlayabilirsiniz. Otomatik ölçeklendirme sonra analytics kaynak gereksinimlerini yüklemek ve çalışan düğümleri sayısını ölçeklendirir izleyiciler yukarı veya aşağı uygun şekilde. Bu özellik için ek ücret yoktur.

## <a name="getting-started"></a>Başlarken

### <a name="create-a-cluster-with-the-azure-portal"></a>Azure portalı ile küme oluşturma

> [!Note]
> Otomatik ölçeklendirme şu anda yalnızca Azure HDInsight Hive, MapReduce ve Spark kümeleri için sürümü 3.6 desteklenir.

Otomatik ölçeklendirme özelliği etkinleştirmek için normal bir küme oluşturma işleminin bir parçası olarak aşağıdakileri yapın:

1. Seçin **özel (boyut, ayarları, uygulamalar)** yerine **hızlı oluşturma**.
2. Üzerinde **özel** 5. adım (**küme boyutu**) denetleyin **çalışan düğümü otomatik ölçeklendirme** onay kutusu.
3. İstenen değerleri için aşağıdaki özellikleri girin:  

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
        "capacity": {
            "minInstanceCount": 2,
            "maxInstanceCount": 10
        }        
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

Yalnızca etkinleştirebilir veya yeni HDInsight kümeleri için otomatik ölçeklendirmeyi devre dışı.

## <a name="monitoring"></a>İzleme

Küme ölçümleri bir parçası olarak küme ölçek büyütme ve ölçek azaltma geçmişini görüntüleyebilirsiniz. Tüm ölçeklendirme eylemleri geçtiğimiz listeleyebilirsiniz gün, hafta veya daha uzun süre.

## <a name="how-it-works"></a>Nasıl çalışır?

### <a name="metrics-monitoring"></a>Ölçümleri izleme

Otomatik ölçeklendirme, sürekli olarak küme izler ve aşağıdaki ölçümleri toplar:

1. **Toplam CPU bekleyen**: Tüm bekleyen kapsayıcıları yürütülmesini başlatmak için gereken çekirdek sayısı toplam sayısı.
2. **Toplam bellek bekleyen**: Tüm kapsayıcıları bekleyen yürütülmesini başlatmak için gereken toplam bellek (MB cinsinden).
3. **Toplam CPU ücretsiz**: Etkin çalışan düğümlerinde kullanılmayan tüm çekirdek toplamı.
4. **Toplam boş bellek**: Toplam etkin çalışan düğümlerinde kullanılmayan bellek (MB cinsinden).
5. **Kullanılan bellek düğüm başına**: Bir çalışan düğümü üzerindeki yükü. Üzerinde 10 GB'a kadar bellek kullanılır, çalışan düğüme bir çalışan kullanılan belleğin 2 GB daha fazla yük altında kabul edilir.
6. **Düğüm başına uygulama ana sunucu sayısı**: Bir çalışan düğümü üzerinde çalışan uygulama Yöneticisi (AM) kapsayıcı sayısı. İki AM kapsayıcıları barındıran bir çalışan düğümü olarak kabul edilir sıfır AM kapsayıcıları barındıran bir çalışan düğümü çok önemli.

Yukarıdaki ölçümleri, 60 saniyede denetlenir. Ölçek büyütme ve ölçek azaltma kararları bu ölçümlere göre otomatik ölçeklendirme yapar.

### <a name="cluster-scale-up"></a>Küme ölçeği artırma

Aşağıdaki koşullar tespit edildiğinde, otomatik ölçeklendirme ölçek artırma isteği verir:

* Toplam CPU bekleyen 3 dakikadan fazla bir süre için toplam boş CPU büyüktür.
* Toplam bellek bekleyen 3 dakikadan fazla bir süre için toplam boş belleğin büyüktür.

Belirli bir sayıda yeni çalışan düğümlerindeki geçerli CPU ve bellek gereksinimlerini karşılamak ve ardından bu yeni çalışan düğüm sayısı ekler bir ölçek artırma isteği vermek için gerekli olup olmadığını hesaplama yapar.

### <a name="cluster-scale-down"></a>Küme ölçeği azaltma

Aşağıdaki koşullar tespit edildiğinde, otomatik ölçeklendirme ölçek azaltma istek verir:

* Toplam CPU bekleyen toplam boş CPU 10 dakikadan daha küçüktür.
* Toplam bellek bekleyen toplam boş belleğin 10 dakikadan daha küçüktür.

Her düğüm ve geçerli CPU ve bellek gereksinimlerini AM kapsayıcıların sayısına bağlı olarak, otomatik ölçeklendirme hangi düğümleri kaldırma için potansiyel adaylar belirterek belirli bir sayıda düğüm kaldırılması için istekte verecek. Ölçeği azaltma düğümlerinin yetkisini alma işlemini tetikler ve düğümlerin tümüyle yetkisi alınmış olduktan sonra bunlar kaldırılır.

## <a name="next-steps"></a>Sonraki adımlar

* Kümeleri el ölçeklendirme için en iyi yöntemler hakkında bilgi edinin [en iyi ölçeklendirme uygulamaları](hdinsight-scaling-best-practices.md)
