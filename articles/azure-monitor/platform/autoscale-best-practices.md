---
title: Otomatik ölçeklendirme için en iyi yöntemler
description: Azure Web Apps, sanal makine ölçek kümeleri ve bulut Hizmetleri için otomatik ölçeklendirme desenleri
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 07/07/2017
ms.author: ancav
ms.subservice: autoscale
ms.openlocfilehash: 3700fb90318da3787830f9b6c202436c0e45e2fe
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61063403"
---
# <a name="best-practices-for-autoscale"></a>Otomatik ölçeklendirme için en iyi uygulamalar
Azure İzleyici otomatik ölçeklendirme için yalnızca geçerlidir [sanal makine ölçek kümeleri](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [Cloud Services](https://azure.microsoft.com/services/cloud-services/), [App Service - Web Apps](https://azure.microsoft.com/services/app-service/web/), ve [APIManagementHizmetleri](https://docs.microsoft.com/azure/api-management/api-management-key-concepts).

## <a name="autoscale-concepts"></a>Otomatik ölçeklendirme kavramları
* Bir kaynak yalnızca olabilir *bir* otomatik ölçeklendirme ayarı
* Otomatik ölçeklendirme ayarına sahip olabilir veya daha fazla profilleri ve her bir profili bir veya daha fazla otomatik ölçeklendirme kuralları sahip olabilir.
* Bir otomatik ölçeklendirme ayarı olduğu örnekleri yatay olarak ölçeklendirir. *kullanıma* örnekleri artırarak ve *içinde* örneklerinin sayısını azaltmayı tarafından.
  Bir otomatik ölçeklendirme ayarı, maksimum, minimum ve varsayılan değer örnekleri vardır.
* Otomatik ölçeklendirme işi her zaman ölçek genişletme veya ölçek açma için yapılandırılan eşiği geçtiği denetleniyor göre ölçeklendirmek için ilişkili ölçüm okur. Listesini görüntüleyebileceğiniz ölçümlerini, otomatik ölçeklendirme sırasında ölçeğini [Azure İzleyici otomatik ölçeklendirme ortak ölçümleri](autoscale-common-metrics.md).
* Tüm eşikler bir örnek düzeyinde hesaplanır. Örneğin, "ölçek ölçeği bir örnek tarafından ne zaman ortalama CPU > %80 örnek sayısı 2 olduğunda", ortalama CPU tüm örneklerdeki % 80'den büyük olduğunda genişleme anlamına gelir.
* Tüm otomatik ölçeklendirme hataları, etkinlik günlüğüne kaydedilir. Ardından yapılandırabileceğiniz bir [etkinlik günlüğü Uyarısı](./../../azure-monitor/platform/activity-log-alerts.md) böylece bir otomatik ölçeklendirme başarısız olduğunda, e-posta, SMS veya Web kancaları bildirim alabilir.
* Benzer şekilde, tüm başarılı bir ölçeklendirme eylemi etkinlik günlüğüne gönderilir. Böylece başarılı otomatik ölçeklendirme eylemi olduğunda size e-posta, SMS veya Web kancaları bildirim alabilir, ardından bir etkinlik günlüğü uyarısı yapılandırabilirsiniz. Otomatik ölçeklendirme ayarında bildirimler sekmesindeki aracılığıyla başarılı bir ölçeklendirme eylemi için bildirim almak için e-posta veya Web kancası bildirimleri de yapılandırabilirsiniz.

## <a name="autoscale-best-practices"></a>Otomatik ölçeklendirme en iyi uygulamalar
Otomatik ölçeklendirme kullanırken aşağıdaki en iyi yöntemleri kullanın.

### <a name="ensure-the-maximum-and-minimum-values-are-different-and-have-an-adequate-margin-between-them"></a>Maksimum ve minimum değerler farklıdır ve bunlar arasında yeterli bir kenar boşluğu sahip olun
En az olan bir ayarı varsa = 2, en fazla = 2 ve geçerli örnek sayısı, 2, herhangi bir ölçek eylemi ortaya çıkabilir. Dahil olan maksimum ve minimum örnek sayısı arasında yeterli bir kenar boşluğu tutun. Otomatik ölçeklendirme her zaman bu sınırlar arasında olacak şekilde ölçeklendirir.

### <a name="manual-scaling-is-reset-by-autoscale-min-and-max"></a>Otomatik ölçeklendirme MIN ve max tarafından el ile ölçeklendirme sıfırlanır
Bir değere üstünde veya altında en fazla örnek sayısını el ile güncelleştirirseniz, otomatik ölçeklendirme altyapısı dön (üstündeyse) en düşük veya en fazla (yüksekse) otomatik olarak ölçeklendirir. Örneğin, 3 ile 6 arasındaki aralığı ayarlayın. Çalışan bir örneği varsa, otomatik ölçeklendirme altyapısı sonraki çalıştırılmasında üç örneğe ölçeklendirir. Sekiz örneklerine el ile ölçek kümesi, benzer şekilde, sonraki çalıştırma otomatik ölçeklendirme geri sonraki çalıştırılmasında altı örneklerinde ölçeklendirir.  Otomatik ölçeklendirme kurallarını da sıfırlama sürece, el ile ölçeklendirme geçicidir.

### <a name="always-use-a-scale-out-and-scale-in-rule-combination-that-performs-an-increase-and-decrease"></a>Her zaman bir artırma ve azaltma gerçekleştiren bir ölçek genişletme ve ölçeklendirme kural bileşimini kullanın
Birleşimi yalnızca bir parçası kullanıyorsanız, otomatik ölçeklendirme yalnızca tek bir yönde (out veya in Ölçek) maksimum ulaştığında veya profilinde tanımlanan, en az örnek sayısı kadar işlem yapar. Bu en iyi değil, ideal olarak, kaynağınızın kullanılabilir olmasını sağlamak için yüksek kullanım zamanlarda ölçeği artırma işleminin istediğiniz. Benzer şekilde, bazen ölçeğini kaynağınızı istediğiniz düşük kullanımını bu nedenle, maliyet tasarrufu sağlayabilir.

### <a name="choose-the-appropriate-statistic-for-your-diagnostics-metric"></a>Tanılama ölçümünüzün için uygun istatistiği seçin
Tanılama ölçümlerini arasından seçim yapabilirsiniz *ortalama*, *Minimum*, *maksimum* ve *toplam* göre ölçeklendirmek için bir ölçüm olarak. En yaygın istatistiği *ortalama*.

### <a name="choose-the-thresholds-carefully-for-all-metric-types"></a>Dikkatli bir şekilde tüm ölçüm türleri için eşikler seçin
Ölçek genişletme ve ölçeklendirme-pratik durumlar üzerinde bağlı olarak farklı eşikler dikkatle seçerek öneririz.

Biz *önermediğiniz* gibi aşağıda örnekleri aynı veya çok benzer eşik değerleri için dışarı ve içeri koşullar ile otomatik ölçeklendirme ayarları:

* Örnek 1 artırıyorsunuz ne zaman sayısı iş parçacığı sayısı < = 600
* Örnek 1 azaltmak ne zaman sayısı iş parçacığı sayısı > 600 =

Karmaşık görünebilir ancak bir davranışa neden olabilir, bir örneğe bakalım. Aşağıdakiler göz önünde bulundurun.

1. Vardır iki örneği ile başlaması ve iş parçacıkları örnek başına ortalama sayısı için 625'in ardından büyüdükçe varsayılır.
2. Otomatik ölçeklendirme, üçüncü bir örneğini ekleme kullanıma ölçeklendirir.
3. Ardından, örneği arasında ortalama iş parçacığı sayısı için 575 düştüğünü varsayılır.
4. Ölçeği önce son durum tahmin etmek için otomatik ölçeklendirme çalıştığında, ölçeği, olacaktır. Örneğin, 575 x 3 (geçerli örnek sayısı) 1,725 = / 2 (son ölçeklendirildiğinde örneklerinin sayısı) 862.5 iş parçacığı =. Bu, otomatik ölçeklendirme hemen yeniden sonra ortalama iş parçacığı sayısı aynı kalır veya daha küçük bir miktar denk bile bunu, ölçeği genişletmek sahip anlamına gelir. Tüm işlemini yeniden ölçeği, Bununla birlikte, sonsuz bir döngü için önde gelen tekrarlayın.
5. ("Çırpma" olarak adlandırılır) bu durumu önlemek için otomatik ölçeklendirme hiç ölçeğini değil. Bunun yerine, atlar ve hizmetin işi tekrar tekrar çalıştırıldığında koşul reevaluates. Ortalama iş parçacığı sayısı 575 olduğunda çalışması için otomatik ölçeklendirme görünmeyecektir çünkü bu birçok insanların aklını karıştırabilir.

Tahmini bir ölçeklendirme sırasında "durumlarda, ölçek ve ölçeklendirme eylemleri sürekli geri ve İleri nereye kanatların" önlemek için tasarlanmıştır. Bu davranış, Ölçek genişletme için aynı eşikler seçtiğinizde unutmayın ve içinde tutun.

Eşikleri ve ölçek genişletme arasında yeterli bir kenar boşluğu seçme öneririz. Örneğin, aşağıdaki daha iyi kural birleşimi göz önünde bulundurun.

* Örnek 1 artırıyorsunuz ne zaman sayısı % CPU > = 80
* Örnek 1 azaltmak ne zaman sayısı % CPU < 60 =

Bu durumda  

1. 2 örnekler vardır başlamak varsayılır.
2. Otomatik ölçeklendirme örneklerdeki ortalama CPU % 80 aşması durumunda, üçüncü bir örneğini ekleme kullanıma olacak şekilde ölçeklendirir.
3. Şimdi zamanla CPU % 60 düştüğünü varsayılır.
4. Ölçek için olsaydı otomatik ölçeklendirme'nın ölçek daraltma kuralı son durumunu tahmin eder. Örneğin, 60 x 3 (geçerli örnek sayısı) 180 = / 2 (son ölçeklendirildiğinde örneklerinin sayısı) 90 =. Bu nedenle otomatik ölçeklendirme ölçek hemen yeniden genişletmek sahip için açma değil. Bunun yerine, ölçeği atlar.
5. Sonraki saat otomatik ölçeklendirme denetler, CPU 50'ye denk devam eder. Yeniden - 50 x 3 örnek tahminleri 150 = / 2 örnekleri = 75, içinde başarıyla 2 örneklerine ölçeklendirir bırakmayarak 80 genişleme eşiğin altında.

### <a name="considerations-for-scaling-threshold-values-for-special-metrics"></a>Özel ölçüm için eşik değerleri ölçeklendirme dikkat edilmesi gereken noktalar
 Depolama veya Service Bus kuyruğu uzunluğu ölçüm gibi özel ölçümler için eşiği mevcut örnek sayısı kullanılabilir iletilerinin ortalama sayısıdır. Bu ölçüm için eşik değeri dikkatle seçin.

Şimdi daha iyi davranışını anladığınızdan emin olmak için ilgili bir örnek gösterilmektedir.

* Depolama kuyruğu iletisi zaman sayısı örnekleri 1 sayıyı şu kadar Artır > = 50
* Depolama kuyruğu iletisi zaman sayısı 1 sayısı tarafından örnekleri azaltmak < 10 =

Aşağıdakiler göz önünde bulundurun:

1. İki depolama kuyruğu örnekler vardır.
2. İleti gelmeye devam ve depolama kuyruğu gözden geçirirken, toplam sayısı 50 okur. Bu otomatik ölçeklendirme ölçek genişletme eylemi başlaması gerektiğini varsayabilirsiniz. Ancak, 50/2 olduğunu unutmayın. örnek başına 25 iletileri =. Bu nedenle, ölçeği genişletme gerçekleşmez. İlk olmasını genişleme için depolama kuyruğundaki toplam ileti sayısı 100 olması gerekir.
3. Ardından, toplam ileti sayısı 100 ulaştığını varsayılır.
4. 3 bir depolama kuyruğu örneği, bir ölçeklendirme eylemi nedeniyle eklenir.  Çünkü kuyruğundaki toplam ileti sayısı 150 ulaşana kadar sonraki ölçek genişletme eylemi yapılmaz 150/3 = 50.
5. Şimdi daha küçük sırasındaki ileti sayısını alır. Tüm sıralardaki toplam ileti için en fazla 30 eklediğinizde üç örnekleriyle ilk ölçek daraltma eylemi'olmuyor 30/3 = ölçeğini eşiğin örneği başına 10 iletileri.

### <a name="considerations-for-scaling-when-multiple-profiles-are-configured-in-an-autoscale-setting"></a>Birden çok profil bir otomatik ölçeklendirme ayarında yapılandırılan ölçeklendirme konuları
Bir otomatik ölçeklendirme ayarında zamanlama veya zaman herhangi bir bağımlılığı olmadan her zaman uygulanır, bir varsayılan profili seçebilir veya bir tarih ve saat aralığını içeren sabit bir süre için yinelenen bir profil ya da profili seçebilirsiniz.

Otomatik ölçeklendirme hizmeti bunları işlediğinde, her zaman aşağıdaki sırayla denetler:

1. Sabit tarih profili
2. Yinelenen profili
3. Varsayılan profil ("her zaman")

Bir profili koşul karşılanıyorsa, otomatik ölçeklendirme altındaki sonraki profili koşulu denetlemez. Otomatik ölçeklendirme, yalnızca bir profil teker teker işler. Başka bir deyişle, ayrıca daha düşük katmanlı profilinden işleme koşul eklemek istiyorsanız, bu kurallar da geçerli profilinde eklemeniz gerekir.

Bu örnek kullanıldığında gözden geçirelim:

Aşağıdaki resimde bir otomatik ölçeklendirme ayarı gösterir 2 ve maksimum örnek en düşük örnekleri ile bir varsayılan profili = = 10. Bu örnekte, kuralları sıradaki ileti sayısı 10'dan büyük olduğunda genişletmek için yapılandırılır ve ölçek sırasındaki ileti sayısını üçten daha az olduğunda açma. Şimdi kaynak iki ile on örnekleri arasında ölçeklendirebilirsiniz.

Ayrıca, bir yinelenen profili Pazartesi kümesi yok. En düşük örnekleri için ayarlanmış 3 ve en fazla örnekleri = = 10. Örnek sayısı iki ise Pazartesi günü, yani ilk kez otomatik ölçeklendirme denetimler için bu durum, yeni en az üç için ölçeklendirir. Bu profil durumu bulmak otomatik ölçeklendirme devam sürece (Pazartesi) ile eşleşen, yalnızca bu profil için yapılandırılmış CPU tabanlı ölçek-out/kuralları işler. Şu anda kuyruk uzunluğu için bunu denetlemez. Denetlenecek kuyruk uzunluğu koşulu da isterseniz, ancak, bu kurallardan varsayılan profil de Pazartesi profilinizde içermelidir.

Otomatik ölçeklendirme varsayılan profiline geçirildiğinde, benzer şekilde, onu önce minimum ve maksimum koşullar karşılandığında, denetler. 12 zaman örneği sayısını ise, 10, varsayılan profili için izin verilen maksimum ölçeklendirir.

![Otomatik ölçeklendirme ayarları](./media/autoscale-best-practices/insights-autoscale-best-practices-2.png)

### <a name="considerations-for-scaling-when-multiple-rules-are-configured-in-a-profile"></a>Birden çok kural, bir profilde yapılandırıldığında, ölçeklendirme için dikkat edilmesi gerekenler
Burada bir profilinde birden çok kural ayarlamanız gerekebilir durumlar vardır. Otomatik ölçeklendirme kuralları aşağıdaki kümesini birden çok kural ayarlandığında Hizmetleri kullanımı tarafından kullanılır.

Üzerinde *ölçeğini*, otomatik ölçeklendirme, herhangi bir kural karşılanırsa çalıştırır.
Üzerinde *ölçeğini*, otomatik ölçeklendirme gerektiren karşılanması için tüm kurallar.

Göstermek için aşağıdaki dört otomatik ölçeklendirme kuralları olduğunu varsayın:

* 1 ile ölçek, CPU < %30
* Ölçek-1 olarak bellek < %50
* CPU > %75, 1 ile ölçeklendirme
* Bellek > %75, 1 ile ölçeklendirme

Ardından izleme oluşur:

* % 76 CPU ve bellek % 50'dir, biz ölçek genişletme.
* CPU %50 ve % 76 bellek ise, biz ölçek genişletme.

Öte yandan, CPU'nun % 25 olduğundan ve bellek % 51 otomatik ölçeklendirme yapar **değil** ölçeğini. İçin ölçek-CPU % 29 ve bellek % 49 olması gerekir.

### <a name="always-select-a-safe-default-instance-count"></a>Her zaman güvenli varsayılan örnek sayısı seçin
Varsayılan örnek sayısı, ölçümleri mevcut olmadığı durumlarda otomatik ölçeklendirme hizmetiniz söz konusu sayısına ölçeklendirir önemlidir. Bu nedenle, iş yükleriniz için güvenli bir varsayılan örnek sayısı seçin.

### <a name="configure-autoscale-notifications"></a>Otomatik ölçeklendirme bildirimleri yapılandırma
Otomatik ölçeklendirme, aşağıdaki koşullardan herhangi biri meydana gelirse etkinlik günlüğüne sonrası:

* Otomatik ölçeklendirme, bir ölçeklendirme işlemi sorunları
* Otomatik ölçeklendirme hizmeti bir ölçeklendirme eylemi başarıyla tamamlar.
* Bir ölçek eylemi gerçekleştirmek otomatik ölçeklendirme hizmeti başarısız.
* Ölçümleri ölçek karar vermek, otomatik ölçeklendirme hizmeti kullanılabilir değil.
* (Yeniden ölçek karar vermek için kullanılabilir kurtarma) ölçümleridir.

Etkinlik günlüğü Uyarısı, otomatik ölçeklendirme altyapısı durumunu izlemek için de kullanabilirsiniz. Örnekler için [bir etkinlik günlüğü aboneliğinizdeki tüm otomatik ölçeklendirme altyapısı işlemleri izlemek için uyarı oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert) veya [bir etkinlik günlüğü içindeki tüm başarısız otomatik ölçek ölçeği İzleyici / ölçeklendirme işlemleri için uyarı oluşturma, Abonelik](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).

Etkinlik günlüğü uyarıları kullanmanın yanı sıra, otomatik ölçeklendirme ayarında bildirimler sekmesindeki aracılığıyla başarılı bir ölçeklendirme eylemi için bildirim almak için e-posta veya Web kancası bildirimleri de yapılandırabilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar
- [Bir etkinlik günlüğü aboneliğinizdeki tüm otomatik ölçeklendirme altyapısı işlemleri izlemek için uyarı oluştur.](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert)
- [Bir etkinlik günlüğü içindeki tüm başarısız otomatik ölçek ölçeği İzleyici / işlemleri aboneliğinizde ölçeği genişletme için uyarı oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert)

