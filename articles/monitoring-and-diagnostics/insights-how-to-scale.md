---
title: "Örnek sayısı el ile veya otomatik ölçeklendirme Azure Portal ile ölçeklendirin | Microsoft Docs"
description: "Azure hizmetlerinizi ölçeklendirmek öğrenin."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 2397596a-071f-4d49-8893-bec5f735bd7b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: ancav
ms.openlocfilehash: d171538ea57839eccddcc74ca099a39aee34ea10
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="scale-instance-count-manually-or-automatically"></a>Örnek sayısı el ile veya otomatik olarak ölçeklendirme
İçinde [Azure Portal](https://portal.azure.com/), hizmetiniz örnek sayısını elle ayarlayabilirsiniz veya, parametreleri sağlamak için talebe göre ölçeklendirin otomatik olarak ayarlayabilirsiniz. Bu tipik olarak adlandırılır *ölçeğini* veya *içinde ölçeklendirmek*.

Örnek sayısına göre ölçeklendirme önce tarafından ölçeklendirme etkilenir düşünmelisiniz **fiyatlandırma katmanı** yanı sıra örnek sayısı. Farklı fiyatlandırma katmanlarına sahip farklı numaraları çekirdek ve bellek ve bu nedenle bunlar olacaktır örnekleri aynı sayıda daha iyi performans (olduğu *ölçeği* veya *ölçeklendirmeyi azaltın*). Bu makalede özellikle kapsayan *içinde ölçeklendirmek* ve *çıkışı*.

Portalda ölçeklendirebilirsiniz ve aynı zamanda [REST API](https://msdn.microsoft.com/library/azure/dn931953.aspx) veya [.NET SDK'sı](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor) ölçeği el ile veya otomatik olarak ayarlamak için.

> [!NOTE]
> Bu makalede portalında bir otomatik ölçeklendirme ayarı oluşturmak nasıl [http://portal.azure.com](http://portal.azure.com). Bu Portalı'nda oluşturulan otomatik ölçeklendirme ayarlarını olamaz Klasik Portalı'nı düzenlenebilir ([http://manage.windowsazure.com](http://manage.windowsazure.com)).
> 
> 

## <a name="scaling-manually"></a>El ile ölçeklendirme
1. İçinde [Azure Portal](https://portal.azure.com/), tıklatın **Gözat**, gibi ölçeklendirmek istediğiniz kaynak gidin bir **uygulama hizmeti planı**.
2. Tıklatın **ayarlar > Ölçek genişletme (uygulama hizmeti planı).**
3. Üstündeki **ölçek** dikey hizmetinin otomatik ölçeklendirme eylemleri geçmişini görebilirsiniz.
   
    ![Ölçek dikey penceresi](./media/insights-how-to-scale/Insights_ScaleBladeDayZero.png)
   
   > [!NOTE]
   > Otomatik ölçeklendirme tarafından gerçekleştirilen eylemler Bu grafikte gösterir. Örnek sayısı el ile ayarlarsanız bu grafikte değişiklik yansıtılmaz.
   > 
   > 
4. Sayı el ile ayarlayabilirsiniz **örnekleri** kaydırıcı ile.
5. Tıklatın **kaydetmek** komutunu ölçeklendirilmiş bu sayıda örneğe hemen.

## <a name="scaling-based-on-a-pre-set-metric"></a>Üzerinde önceden ayarlanmış bir ölçümü tabanlı ölçeklendirme
Otomatik olarak ayarlamak için örnek sayısı üzerinde bir ölçümü tabanlı istiyorsanız, istediğiniz ölçümü seçin **göre Ölçeklendirmeniz** açılır. Örneğin, bir **uygulama hizmeti planı** tarafından ölçeklendirebilirsiniz **CPU yüzdesi**.

1. Ölçüm seçtiğinizde kaydırıcıyı ve/veya, arasında ölçeklendirmek istediğiniz örneklerinin sayısını girmek için metin kutuları elde edersiniz:
   
    ![Ölçek dikey CPU yüzdesi](./media/insights-how-to-scale/Insights_ScaleBladeCPU.png)
   
    Otomatik ölçeklendirme, hizmetinizin altında veya olsun, yük ayarlamak sınırları üzerinde hiçbir zaman olur.
2. İkinci olarak, metrik için hedef aralık seçin. Örneğin, seçtiğiniz **CPU yüzdesi**, bir hedef için ortalama CPU tüm örnekleri arasında hizmetinizi ayarlayabilirsiniz. Ortalama CPU tanımladığınız maksimum aştığında genişletme olacağını, ortalama CPU en düştüğünde her bir ölçek benzer şekilde, gerçekleşir.
3. Tıklatın **kaydetmek** komutu. Otomatik ölçeklendirme, metrik için hedef ve örnek aralığı içinde olduğundan emin olmak için birkaç dakikada kontrol eder. Hizmetinizi ek trafiği aldığında, herhangi bir şey yapmadan daha fazla örnekleri alırsınız.

## <a name="scale-based-on-other-metrics"></a>Diğer ölçümleri temel ölçek
Temel alınarak ölçümleri görünür hazır dışında ölçeklendirebilirsiniz **göre Ölçeklendirmeniz** açılan listesinde ve hatta ölçek genişletme karmaşık kümesine sahiptir ve ölçeklendirme kuralları.

### <a name="adding-or-changing-a-rule"></a>Ekleme veya bir kural değiştirme
1. Seçin **zamanlama ve performans kuralları** içinde **göre Ölçeklendirmeniz** açılır: ![performans kuralları](./media/insights-how-to-scale/Insights_PerformanceRules.png)
2. Üzerinde daha önce sahip olduğunuz otomatik ölçeklendirme, sahip olduğunuz kesin kurallar görünümünü görürsünüz.
3. Başka bir ölçüm tıklatıldığında dayalı ölçek **Kuralı Ekle** satır. Ayrıca, daha önce tarafından ölçeklendirmek istediğiniz ölçümü için sahip olduğunuz ölçüm değiştirmek için varolan satırlardan birini de tıklatabilirsiniz.
   ![Kural Ekle](./media/insights-how-to-scale/Insights_AddRule.png)
4. Ölçüm seçmenize gerek artık tarafından ölçeklendirmek istiyorsunuz. Dikkate alınması gereken birkaç şey vardır ölçüm seçerken:
   
   * *Kaynak* ölçüm alanından gelir. Genellikle, bu ölçekleme kaynak ile aynı olacaktır. Ancak, bir depolama kuyruğu derinliği tarafından ölçeklendirmek istiyorsanız, kaynak tarafından ölçeklendirmek istediğiniz sıra değil.
   * *Ölçüm adı* kendisi.
   * *Zaman toplama* ölçümün. Bu nasıl veri birleştirme üzerinden numarasıdır *süresi*.
5. Ölçüm seçtikten sonra ölçüm ve işlecini eşiği seçin. Örneğin, diyebilirsiniz **büyük** **% 80**.
6. Ardından gerçekleştirmek istediğiniz eylemi seçin. Birkaç farklı türde eylemler vardır:
   
   * Bu Ekle veya Kaldır- azaltabileceğinden **değeri** tanımladığınız örneklerinin sayısı
   * Artırma veya azaltma yüzde - Bu örnek sayısı bir oranında değiştirir. Örneğin, 25 içine **değeri** alan, ve 8 örnekleri şu anda olsaydı 2 eklenir.
   * Artırmak veya azaltmak için - bu örnek sayısı ayarlanacağını **değeri** tanımlarsınız.
7. Son olarak, aşağı - bu kural yeniden ölçeklendirmek için önceki ölçek eylemi sonra ne kadar beklemesi gerektiğini cool seçebilirsiniz.
8. Kural yapılandırdıktan sonra isabet **Tamam**.
9. Tüm istediğiniz kuralları yapılandırdıktan sonra isabet mutlaka **kaydetmek** komutu.

### <a name="scaling-with-multiple-steps"></a>Birden çok adımı ile ölçeklendirme
Yukarıdaki örnek oldukça temel verilebilir. Ancak, Yukarı (veya aşağı) ölçeklendirme hakkında daha agresif olmasını istiyorsanız, bile aynı ölçümü için birden fazla ölçek kuralı ekleyebilirsiniz. Örneğin, CPU yüzdesi iki ölçek kurallar tanımlayabilirsiniz:

1. CPU yüzdesi % 60 ise 1 örneği tarafından ölçeğini genişletme
2. CPU yüzdesi % 85 ise 3 örnekleri tarafından ölçeğini genişletme

![Birden fazla ölçek kuralı](./media/insights-how-to-scale/Insights_MultipleScaleRules.png)

Bu ek kuralıyla bir ölçek eylemi önce % 85 yük aşarsa, bir yerine iki ek örnekleri alırsınız.

## <a name="scale-based-on-a-schedule"></a>Bir zamanlamaya göre ölçeği
Ölçek kuralı oluşturduğunuzda, varsayılan olarak, bu her zaman uygular. Profil başlığındaki tıklattığınızda, görebilirsiniz:

![Profil](./media/insights-how-to-scale/Insights_Profile.png)

Ancak, daha katı gün veya hafta sırasında daha hafta sonlarında Ölçeklendirmesi isteyebilirsiniz. Hizmetinizi çalışma saatleri tamamen devre dışı bile kapatmak.

1. Bu, sahip olduğunuz profilinde yapmak için seçin **yineleme** yerine **her zaman,** ve uygulanacak profili istediğiniz saatleri seçin.
2. Örneğin, hafta içinde geçerli bir profil için **gün** açılır işaretini **Cumartesi** ve **Pazar**.
3. Daytime sırasında geçerli bir profil olacak şekilde ayarlanmış **başlangıç zamanı** başlangıç istediğiniz saati için.
   
    ![Varsayılan yineleme](./media/insights-how-to-scale/Insights_ProfileRecurrence.png)
4. **Tamam** düğmesine tıklayın.
5. Ardından, diğer saatlerde uygulamak istediğiniz profili eklemeniz gerekir. Tıklatın **eklemek profili** satır.
    ![İş devre dışı](./media/insights-how-to-scale/Insights_ProfileOffWork.png)
6. Yeni, ikinci, profil adı, örneğin çağırabilirsiniz **kapalı iş**.
7. Ardından **yineleme** yeniden ve bu süre boyunca istediğiniz örnek sayısı aralığı seçin.
8. Varsayılan profili ile seçerken **gün** , uygulamak için bu profili istediğiniz ve **başlangıç zamanı** gün.
   
   > [!NOTE]
   > Otomatik ölçeklendirme Yaz Saati kuralları hangisi için kullanacağınız **saat dilimi** seçin. UTC uzaklığı temel saat dilimi konumu, gün ışığından yararlanma tasarrufları UTC uzaklığı gösterir ancak günışığından sırasında.
   > 
   > 
9. **Tamam** düğmesine tıklayın.
10. Şimdi, ikinci profilinizi sırasında uygulamak istediğiniz her kuralları eklemeniz gerekir. Tıklatın **Kuralı Ekle**, ve ardından varsayılan profil sırasında sahip aynı kuralı oluşturabilir.
    
    ![İş kapalı Kuralı Ekle](./media/insights-how-to-scale/Insights_RuleOffWork.png)
11. Genişleme için hem bir kural oluşturmak emin olun ve ölçek içinde or else sayımına profili sırasında yalnızca büyütür (azaltın veya).
12. Son olarak, tıklatın **kaydetmek**.

## <a name="next-steps"></a>Sonraki adımlar
* [İzleme hizmeti ölçümleri](insights-how-to-customize-monitoring.md) hizmetinizi kullanılabilir ve yanıt verebilir durumda olduğundan emin olmak için.
* [İzleme ve tanılama](insights-how-to-use-diagnostics.md) hizmetinizde ayrıntılı yüksek sıklıkta ölçümleri toplamak için.
* İşletimsel olaylar gerçekleştiğinde ya da ölçümler bir eşiği aştığında [uyarı bildirimleri alın](insights-receive-alert-notifications.md).
* [Uygulama performansı izleme](../application-insights/app-insights-azure-web-apps.md) tam olarak kodunuzu bulutta nasıl gerçekleştiriyor anlamak istiyorsanız.
* [Olayları ve etkinlik günlüğü görüntüle](insights-debugging-with-events.md) hizmetinizi gerçekleştirilmedi her şeyi öğrenin.
* [Kullanılabilirlik ve yanıt hızını herhangi bir web sayfası izleme](../application-insights/app-insights-monitor-web-app-availability.md) , sayfanızın aşağı olup olmadığını öğrenmek için Application Insights ile.

