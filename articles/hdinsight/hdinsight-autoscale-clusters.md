---
title: Azure HDInsight kümeleri (Önizleme) otomatik olarak ölçeklendirme
description: Kümeleri otomatik olarak ölçeklendirmek için HDInsight otomatik ölçeklendirme özelliğini kullanın
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: hrasheed
ms.openlocfilehash: 6ec981164de0ff61b0e83d54255d046a1418ed96
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2019
ms.locfileid: "66000099"
---
# <a name="automatically-scale-azure-hdinsight-clusters-preview"></a>Azure HDInsight kümeleri (Önizleme) otomatik olarak ölçeklendirme

> [!Important]
> Otomatik ölçeklendirme özelliği, yalnızca 8 Mayıs 2019'den sonra oluşturulmuş Spark, Hive ve MapReduce kümeleri için çalışır. 

Azure HDInsight'ın küme otomatik ölçeklendirme özelliği otomatik olarak çalışan düğümü sayısı bir kümede yukarı ve aşağı ölçeklendirir. Kümedeki düğümler diğer türleri şu anda ölçeklendirilemiyor.  Yeni bir HDInsight kümesi oluşturma sırasında çalışan düğümlerinin minimum ve maksimum sayısını ayarlayabilirsiniz. Otomatik ölçeklendirme analytics yükü kaynak gereksinimlerini izler ve çalışan düğüm sayısı yukarı veya aşağı ölçeklendirir. Bu özellik için ek ücret yoktur.

## <a name="cluster-compatibility"></a>Küme uyumluluğu

Aşağıdaki tabloda, küme türlerini ve otomatik ölçeklendirme özelliği ile uyumlu sürümlerini açıklar.

| Version | Spark | Kovan | LLAP | HBase | Kafka | Storm | ML |
|---|---|---|---|---|---|---|---|
| HDInsight 3.6 ESP olmadan | Evet | Evet | Hayır | Hayır | Hayır | Hayır | Hayır |
| HDInsight 4.0 ESP olmadan | Evet | Evet | Hayır | Hayır | Hayır | Hayır | Hayır |
| HDInsight 3.6 ile ESP | Evet | Evet | Hayır | Hayır | Hayır | Hayır | Hayır |
| HDInsight 3.6 ile ESP | Evet | Evet | Hayır | Hayır | Hayır | Hayır | Hayır |

## <a name="how-it-works"></a>Nasıl çalışır?

Yük göre ölçeklendirme veya HDInsight kümeniz için zamanlama tabanlı ölçeklendirme seçebilirsiniz. Yük göre ölçeklendirme en iyi CPU kullanımı sağlamak ve çalıştırma maliyetini en aza indirmek için ayarlanmış bir aralıkta, kümenizdeki düğüm sayısını değiştirir.

Zamanlama tabanlı ölçeklendirme değişiklikleri belirli zamanlarda etkili koşullar, kümenizdeki düğüm sayısını temel. Bu koşullar, kümeye bir istenilen düğüm sayısına ölçeklendirin.

### <a name="metrics-monitoring"></a>Ölçümleri izleme

Otomatik ölçeklendirme, sürekli olarak küme izler ve aşağıdaki ölçümleri toplar:

* **Toplam CPU bekleyen**: Tüm bekleyen kapsayıcıları yürütülmesini başlatmak için gereken çekirdek sayısı toplam sayısı.
* **Toplam bellek bekleyen**: Tüm kapsayıcıları bekleyen yürütülmesini başlatmak için gereken toplam bellek (MB cinsinden).
* **Toplam CPU ücretsiz**: Etkin çalışan düğümlerinde kullanılmayan tüm çekirdek toplamı.
* **Toplam boş bellek**: Toplam etkin çalışan düğümlerinde kullanılmayan bellek (MB cinsinden).
* **Kullanılan bellek düğüm başına**: Bir çalışan düğümü üzerindeki yükü. Üzerinde 10 GB'a kadar bellek kullanılır, çalışan düğüme bir çalışan kullanılan belleğin 2 GB daha fazla yük altında kabul edilir.
* **Düğüm başına uygulama ana sunucu sayısı**: Bir çalışan düğümü üzerinde çalışan uygulama Yöneticisi (AM) kapsayıcı sayısı. İki AM kapsayıcıları barındıran bir çalışan düğümü olarak kabul edilir sıfır AM kapsayıcıları barındıran bir çalışan düğümü çok önemli.

Yukarıdaki ölçümleri, 60 saniyede denetlenir. Otomatik ölçeklendirme, bu ölçümlere göre ölçek büyütme ve ölçek azaltma kararlarını verir.

### <a name="load-based-cluster-scale-up"></a>Yük tabanlı küme ölçeği artırma

Aşağıdaki koşullar tespit edildiğinde, otomatik ölçeklendirme ölçek artırma isteği verir:

* Toplam CPU bekleyen 3 dakikadan fazla bir süre için toplam boş CPU büyüktür.
* Toplam bellek bekleyen 3 dakikadan fazla bir süre için toplam boş belleğin büyüktür.

HDInsight hizmeti kaç yeni çalışan düğümlerindeki geçerli CPU ve bellek gereksinimlerini karşılamak için gerekli olan hesaplar ve ardından gerekli düğüm sayısı eklemek için ölçek artırma isteği yayınlar.

### <a name="load-based-cluster-scale-down"></a>Küme yük tabanlı azaltma

Aşağıdaki koşullar tespit edildiğinde, otomatik ölçeklendirme ölçek azaltma istek verir:

* Toplam CPU bekleyen toplam boş CPU 10 dakikadan daha küçüktür.
* Toplam bellek bekleyen toplam boş belleğin 10 dakikadan daha küçüktür.

Otomatik ölçeklendirme, her düğüm ve geçerli CPU ve bellek gereksinimlerini AM kapsayıcıların sayısına bağlı olarak, belirli bir düğüm sayısı kaldırılması için istekte verir. Hizmet geçerli iş yürütmeye göre kaldırma için aday olan düğümleri algılar. Ölçeği azaltma işlemi ilk düğümleri decommissions ve ardından bunları kümeden kaldırır.

## <a name="get-started"></a>başlarken

### <a name="create-a-cluster-with-load-based-autoscaling"></a>Yük tabanlı otomatik ölçeklendirme ile küme oluşturma

Yük göre ölçeklendirme ile otomatik ölçeklendirme özelliği etkinleştirmek için normal bir küme oluşturma işleminin bir parçası aşağıdaki adımları tamamlayın:

1. Seçin **özel (boyut, ayarları, uygulamalar)** yerine **hızlı oluşturma**.
1. Üzerinde **özel** 5. adım (**küme boyutu**), kontrol **çalışan düğümü otomatik ölçeklendirme** onay kutusu.
1. Seçeneğini **yük tabanlı** altında **otomatik ölçeklendirme türü**.
1. İstenen değerleri için aşağıdaki özellikleri girin:  

    * İlk **numarası, çalışan düğümleri**.  
    * **En az** çalışan düğümü sayısı.  
    * **En fazla** çalışan düğümü sayısı.  

    ![Çalışan düğümü yük tabanlı otomatik ölçeklendirme seçeneğini etkinleştirin](./media/hdinsight-autoscale-clusters/usingAutoscale.png)

İlk alt düğüm sayısını, minimum ve maksimum, kapsamlı arasında olması gerekir. Oluşturulduğunda kümenin ilk boyutu bu değer tanımlar. Çalışan düğüm sayısı alt sınırı 0'dan büyük olmalıdır.

### <a name="create-a-cluster-with-schedule-based-autoscaling"></a>Zamanlama tabanlı otomatik ölçeklendirme ile küme oluşturma

Zamanlama tabanlı ölçeklendirme ve otomatik ölçeklendirme özelliği etkinleştirmek için normal bir küme oluşturma işleminin bir parçası aşağıdaki adımları tamamlayın:

1. Seçin **özel (boyut, ayarları, uygulamalar)** yerine **hızlı oluşturma**.
1. Üzerinde **özel** 5. adım (**küme boyutu**), kontrol **çalışan düğümü otomatik ölçeklendirme** onay kutusu.
1. Girin **numarası, çalışan düğümleri**, küme ölçeklendirme sınırını kontrol eder.
1. Seçeneğini **zamanlama tabanlı** altında **otomatik ölçeklendirme türü**.
1. Tıklayın **yapılandırma** açmak için **otomatik ölçeklendirme Yapılandırması** penceresi.
1. Saat diliminizdeki seçin ve ardından **+ koşul Ekle**
1. Yeni koşul uygulanmasını haftanın günlerini seçin.
1. Koşul etkili olur ve küme için daraltılacağı düğüm sayısını gerçekleştirmesi gereken süreyi düzenleyin.
1. Gerekirse daha fazla koşul ekleyin.

    ![Çalışan düğümü zamanlama tabanlı ölçeklendirme seçeneğini etkinleştirin](./media/hdinsight-autoscale-clusters/hdinsight-autoscale-clusters-schedule-creation.png)

Düğüm sayısı 1 ve koşul eklemeden önce girdiğiniz bir çalışan düğümü sayısı arasında olmalıdır.

### <a name="final-creation-steps"></a>Son oluşturma adımları

Yük tabanlı hem de zamanlama tabanlı ölçeklendirme için VM türü için çalışan düğümü tıklayarak seçin. **çalışan düğümü boyutu** ve **baş düğüm boyutu**. Her düğüm türü için VM türü seçtikten sonra tüm küme tahmini maliyet aralığının görebilirsiniz. Bütçenize uygun VM türleri ayarlayın.

![Çalışan düğümü zamanlama tabanlı ölçeklendirme seçeneğini etkinleştirin](./media/hdinsight-autoscale-clusters/hdinsight-autoscale-clusters-node-size-selection.png)

Aboneliğiniz, her bölge için bir kapasite kotası vardır. Kapasite kota toplam çalışan düğümü sayısı ile birlikte, baş düğüm çekirdeği sayısını geçemez. Ancak, bu kotayı geçici bir sınırlıdır; her zaman kolayca artırılmış almak için bir destek bileti oluşturabilirsiniz.

> [!Note]  
> Toplam çekirdek kota sınırını aşarsanız, 'en fazla düğüm bu bölgede kullanılabilir çekirdek sayısı aşıldı, Lütfen başka bir bölge seçin veya Kotayı artırmak için desteğe başvurun.' belirten bir hata iletisi alırsınız.

Azure portalını kullanarak HDInsight küme oluşturma hakkında daha fazla bilgi için bkz. [Azure portalını kullanarak HDInsight oluşturma Linux tabanlı kümelerde](hdinsight-hadoop-create-linux-clusters-portal.md).  

### <a name="create-a-cluster-with-a-resource-manager-template"></a>Resource Manager şablonu ile küme oluşturma

#### <a name="load-based-autoscaling"></a>Yük tabanlı otomatik ölçeklendirme

Ekleyerek bir Azure Resource Manager şablonu yük tabanlı otomatik ölçeklendirme ile HDInsight kümesi oluşturabilirsiniz bir `autoscale` düğüme `computeProfile`  >  `workernode` özelliklerini bölümle `minInstanceCount` ve `maxInstanceCount` olarak json parçacığında gösterilir.

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

#### <a name="schedule-based-autoscaling"></a>Zamanlama tabanlı otomatik ölçeklendirme

Bir HDInsight kümesi zamanlama tabanlı otomatik ölçeklendirme ile bir Azure Resource Manager şablonu ekleyerek oluşturabileceğiniz bir `autoscale` düğüme `computeProfile`  >  `workernode` bölümü. `autoscale` Düğümü içeren bir `recurrence` olan bir `timezone` ve `schedule` değişikliğin ne zaman gerçekleşecek açıklar.

```json
{
  "autoscale": {
    "recurrence": {
      "timeZone": "Pacific Standard Time",
      "schedule": [
        {
          "days": [
            "Monday",
            "Tuesday",
            "Wednesday",
            "Thursday",
            "Friday"
          ],
          "timeAndCapacity": {
            "time": "11:00",
            "minInstanceCount": 10,
            "maxInstanceCount": 10
          }
        },
      ]
    }
  },
  "name": "workernode",
  "targetInstanceCount": 4,
}
```

### <a name="enable-and-disable-autoscale-for-a-running-cluster"></a>Etkinleştirme ve otomatik ölçeklendirme, çalışan bir küme için devre dışı

Çalışan bir küme üzerinde otomatik ölçeklendirmeyi etkinleştirmek için seçin **küme boyutu** altında **ayarları**. Ardından **etkinleştirmek otomatik ölçeklendirme**. İstediğiniz ve yük veya zamanlama tabanlı ölçeklendirme seçeneklerini girin otomatik ölçeklendirme türünü seçin. Son olarak, tıklayın **Kaydet**.

![Çalışan düğümü zamanlama tabanlı ölçeklendirme seçeneğini etkinleştirin](./media/hdinsight-autoscale-clusters/hdinsight-autoscale-clusters-enable-running-cluster.png)

## <a name="best-practices"></a>En iyi yöntemler

### <a name="choosing-load-based-or-schedule-based-scaling"></a>Yük veya zamanlama tabanlı ölçeklendirme seçme

Hangi modun seçme hakkında bir karar vermeden önce aşağıdaki faktörleri göz önünde bulundurun:

* Varyans yükleyin: yük belirli günlerde belirli zamanlarda tutarlı bir deseni takip ediyor. Aksi durumda, zamanlama temelinde yük daha iyi bir seçenektir.
* SLA'sı gereksinimleri: Otomatik ölçeklendirme ölçeklendirme yerine Tahmine dayalı reaktif. Yük artışı ve küme hedef boyutunda olması gerektiğinde başladığında arasında yeterli bir gecikme olacak? Katı bir SLA'sı gereksinimleri vardır ve sabit bilinen bir deseni yük ise, 'zamanlama tabanlı' daha iyi bir seçenektir.

### <a name="consider-the-latency-of-scale-up-or-scale-down-operations"></a>Ölçek gecikme süresini göz önünde bulundurun veya işlemleri ölçeklendirin

Uygulamanın, bir ölçeklendirme işleminin tamamlanması için 10-20 dakika sürebilir. Özelleştirilmiş zaman çizelgesi ayarlamak, bu gecikme planlayın. Küme boyutu, 20 9: 00'da olması gerekiyorsa, 09:00:00 ile ölçeklendirme işleminin tamamlanması için örneğin, zamanlama tetikleyicisi 8:30 AM gibi önceki bir zamana ayarlayın.

### <a name="preparation-for-scaling-down"></a>Ölçeği azaltma için hazırlama

Küme ölçeklendirme işlemi sırasında otomatik ölçeklendirme hedef boyutu karşılamak için düğümlerinin yetkisini alma. Bu düğümler üzerinde görevleri çalışan varsa, otomatik ölçeklendirme görevler tamamlanana kadar bekler. Her çalışan düğümü de bir rol HDFS'deki gördüğünden, geçici veriler için kalan düğümleri kaydırılır. Bu nedenle Kalan düğümlerde tüm geçici verileri barındırmak için yeterli alanı olduğundan emin olmanız gerekir. 

Çalışan işleri çalıştırmak ve tamamlamak devam eder. Bekleyen işler, daha az kullanılabilir çalışan düğümü ile normal şekilde zamanlanması için bekler.

## <a name="monitoring"></a>İzleme

### <a name="cluster-status"></a>Küme durumu

Küme durumunu Azure portalında listelenen otomatik ölçeklendirme etkinlikleri izlemenize yardımcı olabilir.

![Çalışan düğümü yük tabanlı otomatik ölçeklendirme seçeneğini etkinleştirin](./media/hdinsight-autoscale-clusters/hdinsight-autoscale-clusters-cluster-status.png)

Aşağıdaki listede görebileceğiniz tüm küme durum iletileri açıklanmaktadır.

| Küme durumu | Açıklama |
|---|---|
| Çalışıyor | Kümenin normal olarak çalışıyor. Önceki otomatik ölçeklendirme etkinliklerin tümünü başarıyla tamamladınız. |
| Güncelleştiriliyor  | Küme otomatik ölçeklendirme yapılandırması güncelleştiriliyor.  |
| HdInsight yapılandırması  | Bir kümenin ölçeğini artırır veya ölçeği azaltma işlemi devam ediyor.  |
| Güncelleştirme Hatası  | HDInsight, otomatik ölçeklendirme yapılandırması güncelleştirme sırasında sorunlarla karşılaştı. Müşteriler, güncelleştirmeyi yeniden deneyin ya da otomatik ölçeklendirmeyi devre dışı seçebilir.  |
| Hata  | Kümeyle yanlış bir şeydir ve kullanılabilir değil. Bu kümeyi silin ve yeni bir tane oluşturun.  |

Kümenizde geçerli düğüm sayısını görüntülemek için Git **küme boyutu** üzerindeki grafik **genel bakış** kümeniz için sayfa veya tıklayın **küme boyutu** altında  **Ayarları**.

### <a name="operation-history"></a>İşlem Geçmişi

Küme ölçümleri bir parçası olarak küme ölçek büyütme ve ölçek azaltma geçmişini görüntüleyebilirsiniz. Ayrıca, önceki gün, haftalık veya başka bir süre içinde tüm ölçeklendirme eylemleri listeleyebilirsiniz.

Seçin **ölçümleri** altında **izleme**. Ardından **ölçüm Ekle** ve **etkin çalışan sayısı** gelen **ölçüm** açılan kutusu. Zaman aralığını değiştirmek için sağ üst köşedeki düğmesine tıklayın.

![Çalışan düğümü zamanlama tabanlı ölçeklendirme seçeneğini etkinleştirin](./media/hdinsight-autoscale-clusters/hdinsight-autoscale-clusters-chart-metric.png)


## <a name="next-steps"></a>Sonraki adımlar

* Kümeleri el ölçeklendirme için en iyi yöntemler hakkında bilgi edinin [en iyi ölçeklendirme uygulamaları](hdinsight-scaling-best-practices.md)
