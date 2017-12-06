---
title: "Otomatik ölçeklendirme için en iyi uygulamaları | Microsoft Docs"
description: "Azure Web uygulamaları, sanal makine ölçek kümeleri ve bulut Hizmetleri için otomatik ölçeklendirme düzenleri"
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 9fa2b94b-dfa5-4106-96ff-74fd1fba4657
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2017
ms.author: ancav
ms.openlocfilehash: 70ec03d2ed32cb0362bf2f7b24c66979093603be
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="best-practices-for-autoscale"></a>Otomatik ölçeklendirme için en iyi uygulamalar
Bu makalede, azure'da otomatik ölçeklendirme için en iyi yöntemler öğretir. Azure İzleyici otomatik ölçeklendirme uygular yalnızca [sanal makine ölçek kümeleri](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [bulut Hizmetleri](https://azure.microsoft.com/services/cloud-services/), ve [uygulama hizmeti - Web Apps](https://azure.microsoft.com/services/app-service/web/). Diğer Azure hizmetleriyle farklı ölçekleme yöntemlerini kullanın.

## <a name="autoscale-concepts"></a>Otomatik ölçeklendirme kavramları
* Bir kaynak yalnızca olabilir *bir* otomatik ölçeklendirme ayarı
* Otomatik ölçeklendirme ayarına sahip olabilir veya daha fazla profilleri ve her bir profili bir veya daha fazla otomatik ölçeklendirme kurallarını sağlayabilirsiniz.
* Otomatik ölçeklendirme ayarı olduğu örnekleri yatay olarak ölçeklendirir. *çıkışı* örnekleri artırarak ve *içinde* örneklerinin sayısını azaltarak tarafından.
  Otomatik ölçeklendirme ayarı, maksimum, minimum ve varsayılan değer örneklerinin sahiptir.
* Otomatik ölçeklendirme iş genişleme veya ölçek bileşenini için yapılandırılmış eşiği aşıldığında değilse denetimi tarafından ölçeklendirmek için ilişkili ölçüm her zaman okur. Listesini görüntüleyebileceğiniz ölçümlerini, otomatik ölçeklendirme tarafından adresindeki ölçeklendirebilirsiniz [Azure İzleyici otomatik ölçeklendirmeyi ortak ölçümleri](insights-autoscale-common-metrics.md).
* Tüm eşikler bir örnek düzeyinde hesaplanır. Örneğin, "ölçek dışarı 1 örneği tarafından ne zaman ortalama CPU > % 80'e örnek sayısı 2 olduğunda", ortalama CPU tüm örneklerde % 80 ' büyük olduğunda genişleme anlamına gelir.
* Tüm otomatik ölçeklendirme hataları etkinlik günlüğüne kaydedilir. Ardından yapılandırabileceğiniz bir [etkinlik günlüğü Uyarısı](./monitoring-activity-log-alerts.md) böylece e-posta aracılığıyla, SMS, Web kancası, otomatik ölçeklendirme başarısız olduğunda vb. bildirilebilir.
* Benzer şekilde, tüm başarılı ölçeklendirme eylemi etkinlik günlüğü nakledilir. E-posta aracılığıyla, SMS, Web kancalarını vb. başarılı otomatik ölçeklendirme eylemi olduğunda size bildirilebilir böylece daha sonra bir etkinlik günlüğü uyarı yapılandırabilirsiniz. Otomatik ölçeklendirme ayarında bildirimleri sekmesi aracılığıyla başarılı ölçeklendirme eylemi için bildirim almak için e-posta veya Web kancası bildirimleri de yapılandırabilirsiniz.

## <a name="autoscale-best-practices"></a>Otomatik ölçeklendirme en iyi uygulamalar
Otomatik ölçeklendirme kullanırken aşağıdaki en iyi yöntemleri kullanın.

### <a name="ensure-the-maximum-and-minimum-values-are-different-and-have-an-adequate-margin-between-them"></a>Maksimum ve minimum değerler farklı olduğundan ve aralarındaki yeterli bir kenar boşluğu bulunduğundan emin olun
En az olan bir ayarı varsa = 2, en fazla = 2 ve geçerli örnek sayısı, 2, herhangi bir ölçek eylemi oluşabilir. Kapsayıcı maksimum ve minimum örnek sayısı arasında yeterli bir kenar boşluğu tutun. Otomatik ölçeklendirme, her zaman bu sınırlar arasında ölçeklendirir.

### <a name="manual-scaling-is-reset-by-autoscale-min-and-max"></a>Otomatik ölçeklendirme min ve max tarafından el ile ölçeklendirme sıfırlanır
Bir değere üstüne veya altına en fazla örnek sayısı el ile güncelleştirirseniz, otomatik ölçeklendirme altyapısı dön (üstündeyse) veya en küçük (yüksekse) otomatik olarak ölçeklendirir. Örneğin, 3 ile 6 arasındaki bir aralıkta ayarlayın. Bir çalışan örneği varsa, otomatik ölçeklendirme altyapısı sonraki çalıştırılmasında 3 örneklerine ölçeklendirir. 8 örneklerine el ile ölçek ayarlarsanız, benzer şekilde, sonraki çalıştırma otomatik ölçeklendirme, geri örneklerine 6 sonraki çalıştırılmasında ölçeklendirir.  Otomatik ölçeklendirme kurallarını da sıfırlama sürece, el ile ölçeklendirme çok geçicidir.

### <a name="always-use-a-scale-out-and-scale-in-rule-combination-that-performs-an-increase-and-decrease"></a>Her zaman bir artırma ve azaltma yapan bir genişleme ve ölçek bileşenini kural bileşimi kullanın
Otomatik ölçeklendirme ölçek maksimum veya en düşük gereksinim, kadar tek çıkışı veya, bileşeni yalnızca bir bölümü birleşimi kullanırsanız, ulaşıldı.

### <a name="do-not-switch-between-the-azure-portal-and-the-azure-classic-portal-when-managing-autoscale"></a>Azure portalı ve Azure Klasik portalı arasında otomatik ölçeklendirme yönetirken geçme
Bulut Hizmetleri ve uygulama Hizmetleri (Web uygulamaları) için Azure portal (portal.azure.com) oluşturmak ve otomatik ölçeklendirme ayarlarını yönetmek için kullanın. Sanal makine ölçek kümeleri oluşturmak ve otomatik ölçeklendirme ayarı yönetmek için PowerShell'i, CLI veya REST API'yi kullanın. Klasik Azure portalı (manage.windowsazure.com) ve Azure Portalı'nı (portal.azure.com) arasında otomatik ölçeklendirme yapılandırmaları yönetirken geçiş değil. Klasik Azure portalı ve arka plandaki kendi arka uç sınırlamalara sahiptir. Otomatik ölçeklendirme bir grafik kullanıcı arabirimini kullanarak yönetmek için Azure portalında taşıyın. Otomatik ölçeklendirme PowerShell'i, CLI veya REST API (üzerinden Azure kaynak Gezgini) kullanmak için Seçenekler şunlardır.

### <a name="choose-the-appropriate-statistic-for-your-diagnostics-metric"></a>Tanılama ölçümü için uygun istatistiği seçin
Tanılama ölçümleri arasından seçim yapabilirsiniz *ortalama*, *Minimum*, *maksimum* ve *toplam* göre ölçeklendirme ölçüm olarak. En yaygın istatistik *ortalama*.

### <a name="choose-the-thresholds-carefully-for-all-metric-types"></a>Tüm ölçüm türleri eşikler dikkatle seçin
Dikkatli bir şekilde genişleme ve ölçek-üzerinde pratik durumlarda bağlı olarak farklı eşikler seçme öneririz.

Biz *değil önerilir* gibi otomatik ölçeklendirme ayarları aynı veya benzer eşik değerleri out ve koşullar aşağıda örnekler:

* Örnek 1 artırmasını ne zaman saymak iş parçacığı sayısı < = 600
* 1 ile örnekleri azaltmak ne zaman saymak iş parçacığı sayısı > = 600

Karmaşık görünebilir bir davranışa neden olabilir, bir örneğe bakalım. Aşağıdaki sırada göz önünde bulundurun.

1. 2 örneği vardır başından itibaren ve ardından iş parçacığı örneği başına ortalama sayısı için 625 büyütmeleri varsayalım.
2. Otomatik ölçeklendirme 3 örneğini eklemede çıkışı ölçeklendirir.
3. Ardından, örnek arasında ortalama iş parçacığı sayısı için 575 düştüğünü varsayalım.
4. Ölçeklendirme önce son durum tahmin etmek için otomatik ölçeklendirme çalıştığında, ölçeği, olacaktır. Örneğin, 575 x 3 (geçerli örnek sayısı) 1,725 = / 2 (ölçeklendirilmiş durumlarda son sayısı) 862.5 iş parçacığı =. Başka bir deyişle, hemen yeniden ortalama iş parçacığı sayısı aynı kalır ya da yalnızca kısa süreli bile döner bile, ölçeği sonra genişleme için otomatik ölçeklendirme sahip olması gerekir. Tüm işlemini yeniden ölçeklendirilmiş, ancak, sonsuz bir döngüde baştaki yineleyin.
5. ("Flapping" olarak adlandırılır) bu durumu önlemek için otomatik ölçeklendirme hiç ölçeklenmez. Bunun yerine, atlar ve hizmetin iş tekrar tekrar çalıştırıldığında koşul reevaluates. Otomatik ölçeklendirme ortalama iş parçacığı sayısı 575 değiştirildiği çalışmaya görünmeyecektir çünkü bu birçok karıştırır.

Tahmin bir ölçek sırasında "durumlarda, ölçek ve genişletme Eylemler sürekli geri ve İleri nereye dalgalanma" önlemek için tasarlanmıştır. Bu davranış genişleme için aynı eşikler seçtiğinizde unutmayın ve içinde kalmasını sağlayın.

Genişleme arasında ve eşikleri yeterli bir kenar boşluğu seçme öneririz. Örnek olarak, aşağıdaki daha iyi kural birleşimi göz önünde bulundurun.

* Örnek 1 artırmasını ne zaman saymak CPU % > = 80
* 1 ile örnekleri azaltmak ne zaman saymak CPU % < = 60

Bu durumda  

1. 2 örneği vardır başlamak varsayalım.
2. Örnekler arasında ortalama CPU % 80 kalırsa, üçüncü bir örneğini ekleme çıkışı otomatik ölçeklendirme ölçeklendirir.
3. Şimdi zamanla CPU % 60 olarak döner varsayalım.
4. Ölçek için olsaydı otomatik ölçeklendirme'nın ölçek, kuralı son durum tahmin eder. Örneğin, 60 x 3 (geçerli örnek sayısı) 180 = / 2 (ölçeklendirilmiş durumlarda son sayısı) 90 =. Otomatik ölçeklendirme ölçek hemen yeniden genişletmek sahip için açma biçimde değil. Bunun yerine, ölçekleme atlar.
5. Sonraki saat otomatik ölçeklendirme 50'ye düşmesine CPU devam denetler. Yeniden - 50 x 3 örneği tahminleri 150 = / 2 örnekleri 80 genişleme eşiğin altına bunu içinde başarıyla 2 örneklerine ölçeklendirir şekilde olan 75 =.

### <a name="considerations-for-scaling-threshold-values-for-special-metrics"></a>Eşik değerleri için özel ölçümleri ölçeklendirme dikkat edilmesi gereken noktalar
 Depolama veya hizmet veri yolu kuyruğu uzunluğu ölçüm gibi özel ölçümleri eşiği iletileri geçerli örnek sayısı kullanılabilir ortalama sayısıdır. Bu ölçüm için eşik değer dikkatle seçin.

Şimdi daha iyi davranış anladığınızdan emin olmak için bir örnek gösterilmektedir.

* Depolama kuyruğu iletisi gönderdiğinde sayısı 1 sayısına göre örneklerini artırın > = 50
* Depolama kuyruğu iletisi gönderdiğinde sayısı 1 sayısına göre örnekleri azaltmak < = 10

Aşağıdaki sırada göz önünde bulundurun:

1. 2 depolama kuyruğu örneği vardır.
2. İleti gelmeye devam ve depolama kuyruğu incelediğinizde, toplam sayısı 50 okur. Bu otomatik ölçeklendirme bir ölçeklendirme eylemi başlaması gereken varsayabilirsiniz. Ancak, 50/2 olduğunu unutmayın = örneği başına 25 iletileri. Bu nedenle, genişleme gerçekleşmez. İlk gerçekleşecek şekilde genişleme için depolama sırasındaki toplam ileti sayısı 100 olmalıdır.
3. Ardından, toplam ileti sayısı 100 ulaştığında varsayalım.
4. 3 depolama sıra örneği bir ölçeklendirme eylemi nedeniyle eklenir.  Sıradaki toplam ileti sayısı nedeniyle 150 ulaşana kadar sonraki ölçeklendirme eylemi yapılmaz 150/3 = 50.
5. Şimdi daha küçük sırasındaki ileti sayısını alır. Tüm sıralardaki toplam ileti için en fazla 30 eklediğinizde 3 örnekleriyle ilk ölçek eylemi olur 30/3 = için ölçek eşik örneği başına 10 iletileri.

### <a name="considerations-for-scaling-when-multiple-profiles-are-configured-in-an-autoscale-setting"></a>Otomatik ölçeklendirme ayarında birden çok profil yapılandırıldığında ölçeklendirme dikkat edilmesi gereken noktalar
Otomatik ölçeklendirme ayarında zamanlama veya zaman bağımlılıkları olmadan her zaman uygulanır, bir varsayılan profili seçebilir veya bir tarih ve saat aralığı ile sabit bir dönem için yinelenen bir profil ya da profili seçebilirsiniz.

Otomatik ölçeklendirme hizmeti bunları işlediğinde, her zaman şu sırayla denetler:

1. Sabit tarih profili
2. Yinelenen profili
3. Varsayılan ("her zaman") profil

Bir profil koşul karşılandığında, otomatik ölçeklendirme altındaki sonraki profili koşulu denetlemez. Otomatik ölçeklendirme, aynı anda yalnızca bir profil işler. Başka bir deyişle, ayrıca bir alt katmanlı profili işleme durumundan dahil etmek istiyorsanız, bu kurallar da geçerli profilinde eklemeniz gerekir.

Şimdi bu örneği kullanarak gözden geçirin:

Aşağıdaki resimde bir otomatik ölçeklendirme ayarı gösterir 2 ve en fazla örnekleri minimum örnekleri ile bir varsayılan profili = = 10. Bu örnekte, kuralları sıradaki ileti sayısı 10'dan büyük olduğunda genişletmek için yapılandırılmış ve ölçek bileşenini sıradaki ileti sayısı 3'ten az olduğunda. Artık kaynak 2 ile 10 örnekleri arasında ölçeklendirebilirsiniz.

Ayrıca, yinelenen bir profili Pazartesi kümesi yok. Minimum örnekleri için ayarlanmış 3 ve en fazla örnekleri = = 10. Yani Pazartesi günü, bu koşul için ilk zaman otomatik ölçeklendirme denetler örnek sayısı 2 ise, yeni en az 3 için ölçeklendirir. Bu profil koşul bulmak otomatik ölçeklendirme devam ettiği sürece (Pazartesi) eşleşen, yalnızca bu profil için yapılandırılmış CPU tabanlı ölçek genişletme/bileşenini kuralların işler. Şu anda bu kuyruk uzunluğu için denetlemez. Denetlenecek kuyruk uzunluğu koşul da istiyorsanız, ancak, varsayılan profil bu kurallardan de Pazartesi profilinizde içermelidir.

Otomatik ölçeklendirme varsayılan profiline geçtiğinde, benzer şekilde, onu önce minimum ve maksimum koşulların karşılandığından denetler. Zaman örneklerinin sayısını 12 ise, bu, 10, varsayılan profili için izin verilen maksimum ölçeklendirir.

![otomatik ölçeklendirme ayarları](./media/insights-autoscale-best-practices/insights-autoscale-best-practices-2.png)

### <a name="considerations-for-scaling-when-multiple-rules-are-configured-in-a-profile"></a>Birden çok kural, bir profilde yapılandırıldığında ölçeklendirme dikkat edilmesi gereken noktalar
Burada bir profilde birden çok kural ayarlamak için sahip olduğu durumlar vardır. Otomatik ölçeklendirme kurallarını aşağıdaki kümesini birden çok kural ayarlandığında Hizmetleri kullanım tarafından kullanılır.

Üzerinde *ölçeğini*, herhangi bir kural karşılanır otomatik ölçeklendirme çalıştırır.
Üzerinde *ölçek bileşenini*, otomatik ölçeklendirme karşılanması gereken tüm kuralları gerektirir.

Göstermek için aşağıdaki 4 otomatik ölçeklendirme kurallarını sahip olduğunuzu varsayın:

* Varsa CPU < % 30, Ölçek 1 ile açma
* Varsa bellek < % 50, Ölçek-1 ile
* Varsa CPU > %75, 1 ile genişletme
* Varsa bellek > %75, 1 ile genişletme

Ardından izleme oluşur:

* % 76 CPU, bellek % 50 ise, biz genişletme.
* % 50 CPU, bellek % 76 ise, biz genişletme.

Diğer taraftan, CPU ise % 25 ve bellektir % 51 otomatik ölçeklendirme yapar **değil** ölçek açma. Aşağıdakileri yapmak için ölçek içinde CPU %29 ve bellek % 49 olması gerekir.

### <a name="always-select-a-safe-default-instance-count"></a>Her zaman güvenli varsayılan örnek sayısı seçin
Varsayılan örnek sayısı, ölçümleri kullanılabilir olmadığında otomatik ölçeklendirme, count hizmetinize ölçeklendirir önemlidir. Bu nedenle, iş yükleriniz için güvenli bir varsayılan örnek sayısı seçin.

### <a name="configure-autoscale-notifications"></a>Otomatik ölçeklendirme bildirimleri yapılandırma
Aşağıdaki koşullardan herhangi biri oluştuğunda otomatik ölçeklendirme etkinlik günlüğü gönderin:

* Otomatik ölçeklendirme bir ölçeklendirme işlemi sorunları
* Otomatik ölçeklendirme hizmeti bir ölçek eylemi başarıyla tamamlanır
* Bir ölçek eylemi almak otomatik ölçeklendirme hizmet başarısız.
* Ölçümleri ölçek karar vermek için otomatik ölçeklendirme hizmeti kullanılabilir değil.
* (Yeniden bir ölçek karar vermek için kullanılabilir kurtarma) ölçümleridir.

Bir etkinlik günlüğü uyarı, otomatik ölçeklendirme altyapısı sağlığını izlemek için de kullanabilirsiniz. Örnekler için [bir etkinlik günlüğü aboneliğinizi tüm otomatik ölçeklendirme motoru işlemleri izlemek için uyarı oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert) veya [bir etkinlik günlüğü tüm başarısız otomatik ölçeklendirme ölçek izlemek / aboneliğinizi işlemlerini genişletmek için uyarı oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).

Etkinlik günlüğü uyarıları kullanarak ek olarak, otomatik ölçeklendirme ayarında bildirimleri sekmesi aracılığıyla başarılı ölçeklendirme eylemi için bildirim almak için e-posta veya Web kancası bildirimleri de yapılandırabilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar
- [Bir etkinlik günlüğü aboneliğinizi tüm otomatik ölçeklendirme motoru işlemleri izlemek için uyarı oluştur.](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert)
- [Bir etkinlik günlüğü tüm başarısız otomatik ölçeklendirme ölçek izlemek / aboneliğinizi işlemlerini genişletmek için uyarı oluştur](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert)
