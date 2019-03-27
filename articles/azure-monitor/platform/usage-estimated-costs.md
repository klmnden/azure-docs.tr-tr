---
title: Kullanım ve Tahmini maliyetler Azure İzleyici'de izleme
description: Azure İzleyici kullanım ve Tahmini maliyetler sayfasında kullanma işlemine genel bakış
author: dalekoetke
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 08/11/2018
ms.author: mbullwin
ms.reviewer: Dale.Koetke
ms.subservice: ''
ms.openlocfilehash: 7911bd398b6760fb4f83382868f040382b86cd1f
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58480559"
---
# <a name="monitoring-usage-and-estimated-costs"></a>Kullanım ve Tahmini maliyetler izleme

> [!NOTE]
> Bu makalede, kullanım ve Tahmini maliyetler arasında farklı fiyatlandırma modelleri için birden çok Azure İzleme özelliklerini görüntülemek açıklar.  İlgili bilgiler için aşağıdaki makalelere göz atın.
> - [Veri hacmi ve saklama Log analytics'te kontrol ederek maliyet yönetme](../../azure-monitor/platform/manage-cost-storage.md) veri saklama döneminizin değiştirerek maliyetlerinizi denetlemek nasıl açıklar.
> - [Log analytics'te veri kullanımını çözümleme](../../azure-monitor/platform/data-usage.md) analiz ve veri kullanımınızı uyarı açıklar.
> - [Application ınsights fiyatlandırma ve veri hacmini yönetme](../../azure-monitor/app/pricing.md) Application ınsights'ta veri kullanımını çözümleme açıklar.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure portal'ın İzleyici hub'ında **kullanım ve Tahmini maliyetler** sayfası izleme özellikleri gibi Çekirdek kullanımını açıklar [bildirimleri ölçümleri, uyarılar](https://azure.microsoft.com/pricing/details/monitor/), [Azure Log Analytics ](https://azure.microsoft.com/pricing/details/log-analytics/), ve [Azure Application Insights](https://azure.microsoft.com/pricing/details/application-insights/). Nisan 2018 tarihinden önce kullanılabilir fiyatlandırma planları müşteriler, bu öngörüleri satın alınan Log Analytics kullanımı da içerir ve analizi sunar.

Bu sayfada, kullanıcılar kendi kaynak kullanımı son 31 gün için abonelik başına toplanan görüntüleyebilir. Bağlantılı bileşenler 31 günlük dönem boyunca kullanım eğilimlerini gösterir. Çok fazla veri için bu tahmin, bu nedenle birlikte gelen gerektiğinde sayfa yüklenirken lütfen sabırlı olun.

Bu örnek, izleme kullanımını ve elde edilen maliyet tahmini gösterir:

![Kullanım ve Tahmini maliyetler portalı ekran görüntüsü](./media/usage-estimated-costs/001.png)

Son 31 gün dönem boyunca kullanım eğilimlerini gösteren bir grafiği'ni açmak için aylık kullanım sütunundaki bağlantıyı seçin:

![Çubuk grafik ekran düğüm başına dahil edilen](./media/usage-estimated-costs/002.png)

İşte başka bir benzer kullanım ve maliyet özeti. Bu örnekte, yeni Nisan 2018 tüketim tabanlı fiyatlandırma modeliyle bir abonelik gösterilmektedir. Tüm düğüm kullanıma dayalı faturalandırma eksikliği unutmayın. Veri alımı ve Log Analytics ve Application Insights için bekletme artık yeni bir ortak ölçüm olarak raporlanır.

![Kullanım ve Tahmini maliyetler portalı ekran görüntüsü - Nisan 2018 fiyatlandırma](./media/usage-estimated-costs/003.png)

## <a name="new-pricing-model"></a>Yeni fiyatlandırma modeli

Nisan 2018'de bir [yeni fiyatlandırma modeli izleme bırakıldığını](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/).  Bu, bulut kullanımı kolay, tüketim tabanlı fiyatlandırma sunar. Yalnızca, düğüm tabanlı taahhüt kullandıklarınız için ödeme yaparsınız. Yeni fiyatlandırma modeline ayrıntılarını kullanılabilir [bildirimleri ölçümleri, uyarılar](https://azure.microsoft.com/pricing/details/monitor/), [Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/) ve [Application Insights](https://azure.microsoft.com/pricing/details/application-insights/). 

Log Analytics veya Application ınsights'ı 2 Nisan 2018 tarihinden sonra müşterilerin eklenmesi için yeni fiyatlandırma modeline tek seçenektir. Bu hizmetler kullanan müşteriler için yeni fiyatlandırma modeline taşıma isteğe bağlıdır.

## <a name="assessing-the-impact-of-the-new-pricing-model"></a>Yeni fiyatlandırma modeline etkisini değerlendirme
Yeni fiyatlandırma modeli, kendi izleme kullanım düzenlerini esas alarak her müşteri farklı etkiler olacaktır. Log Analytics veya Application ınsights'ı 2 Nisan 2018'den önce kullanan müşteriler için **kullanım ve tahmini maliyet** Azure İzleyicisi'nde sayfa bunlar yeni fiyatlandırma modeline geçmeniz durumunda herhangi bir değişiklik maliyetleri tahmin eder. Yeni modele bir aboneliği taşımak için bir yol sunar. Çoğu müşteri için yeni fiyatlandırma modeline yararlı olacaktır. Özellikle yüksek veri kullanım düzenlerine sahip ya da daha yüksek maliyetli bölgelerindeki müşteriler için bu durumda olmayabilir.

Tahmini maliyetlerinizi abonelik üzerinde seçtiğiniz görmek için **kullanım ve Tahmini maliyetler** sayfasında, sayfanın üst kısmındaki mavi renkli bir başlık seçin. Yeni fiyatlandırma modeline önemsenmeksizin devralınabilir düzeyi, çünkü bu bir aboneliği bir zaman yapmanız önerilir.

![Kullanım ve Tahmini maliyetler yeni fiyatlandırma modeli ekran izleme](./media/usage-estimated-costs/004.png)

Yeni sayfa, önceki sayfanın yeşil başlığı ile benzer bir sürümü göstermektedir:

![Kullanım ve Tahmini maliyetler geçerli fiyatlandırma modeli ekran izleme](./media/usage-estimated-costs/005.png)

Sayfa, yeni fiyatlandırma modeline karşılık gelen ölçümleri farklı bir kümesini de gösterir. Bu liste örneği verilmiştir:

- İçgörü ve düğüm başına Analytics\Overage
- İçgörü ve düğüm başına Analytics\Included
- Uygulama Insights\Basic fazlalık veri
- Uygulama Insights\Included verileri

Yeni fiyatlandırma modelinin düğüm tabanlı dahil edilen veri ayırma yok. Bu nedenle, bu veri alımı ölçümlerine adlı yeni ortak veri alımı bir ölçüm birleştirilir **paylaşılan Services\Data alımı**. 

Log Analytics veya Application ınsights'ı daha yüksek maliyetleri bölgelerde alınan verileri başka bir değişiklik yoktur. Bu yüksek maliyetli bölgelere verilerini yeni bölgesel ölçümlere ile gösterilir. Bir örnek **veri alımı (ABD Orta Batı)**.

> [!NOTE]
> Abonelik başına maliyetleri değil hesap düzeyinde düğüm yetkilendirmelerini Operations Management Suite (OMS) abonelik başına içine faktörü tahmini. Hesap temsilcinizle için daha ayrıntılı bir tartışma yeni fiyatlandırma modeli, bu durumda başvurun.

## <a name="new-pricing-model-and-operations-management-suite-subscription-entitlements"></a>Yeni fiyatlandırma modeli ve Operations Management Suite aboneliği destek hakları

Microsoft Operations Management Suite E1 ve E2 satın alan müşteriler için düğüm başına veri alımı yetkilendirmeler için uygun [Log Analytics](https://www.microsoft.com/cloud-platform/operations-management-suite) ve [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-pricing). Belirli bir abonelikte Bu destek haklarını Log Analytics çalışma alanları veya Application Insights kaynakları almak için: 

- Aboneliğin fiyatlandırma modeli, Nisan 2018 öncesi modele kalmalıdır.
- Log Analytics çalışma alanları "fiyatlandırma katmanında düğüm başına (OMS)" kullanmanız gerekir.
- Application Insights kaynakları "Kurumsal" fiyatlandırma planını kullanmanız gerekir.

Kuruluşunuzun satın aldığı, paketin düğüm sayısına bağlı olarak bazı taşıma, yeni fiyatlandırma modeline abonelikleri yararlı olabilir ancak bunu dikkatli gerektirir. Genel olarak, yalnızca yukarıda açıklanan şekilde pre-Nisan 2018 modelinde kalmak için tavsiye edilir.

> [!WARNING]
> Kuruluşunuz Microsoft Operations Management Suite E1 ve E2 satın aldıysa aboneliklerinizi pre-Nisan 2018 fiyatlandırma modelinde tutmak en iyisidir. 
>

## <a name="changes-when-youre-moving-to-the-new-pricing-model"></a>Yeni fiyatlandırma modeline geçerken değişiklikleri

Log Analytics ve Application Insights fiyatlandırma seçenekleri yalnızca tek bir katman (veya planı) için yeni fiyatlandırma modeline basitleştirir. Bir aboneliği yeni fiyatlandırma modeli taşıma:

- (Azure Resource Manager'da "pergb2018" olarak adlandırılır) yeni GB başına katmanı her bir Log Analytics için fiyatlandırma katmanını değiştirme
- Kurumsal plandaki tüm Application Insights kaynakları temel plana değiştirilir.

Maliyet tahmini, bu değişikliklerin etkisini gösterir.

> [!WARNING]
> Buradan dağıtmak için Azure Resource Manager'ı veya PowerShell'i kullanıyorsanız, önemli bir Not [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-template-workspace-configuration) veya [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-powershell) fiyatlandırma modeli yeni taşınmış bir abonelik. Bir fiyatlandırma katmanı/planı "pergb2018" dışında için Log Analytics veya "Temel" için Application ınsights'ı belirtirseniz, yerine geçersiz bir fiyatlandırma katmanı/planını belirten nedeniyle dağıtım başarısız başarılı **ancak yalnızca geçerli kullanın Fiyatlandırma katmanı/planını** (Bu geçersiz bir fiyatlandırma katmanı iletisi burada oluşturulur Log Analytics'i ücretsiz katmanı için geçerli değildir).
>

## <a name="moving-to-the-new-pricing-model"></a>Yeni fiyatlandırma modeline taşıma

Bir aboneliği yeni fiyatlandırma modeline benimsemeye karar verdiyseniz, seçin **fiyatlandırma modeli seçimi** en üstündeki seçeneği **kullanım ve Tahmini maliyetler** sayfası:

![Kullanım ve Tahmini maliyetler yeni fiyatlandırma modeli ekran izleme](./media/usage-estimated-costs/006.png)

**Fiyatlandırma modeli seçimi** sayfası açılır. Bu, her önceki sayfada görüntülenen Aboneliklerin listesini gösterir:

![Fiyatlandırma modeli seçimi ekran görüntüsü](./media/usage-estimated-costs/007.png)

Bir aboneliği yeni fiyatlandırma modeline taşımak için yalnızca kutusunu işaretleyin ve ardından **Kaydet**. Bu gibi durumlarda, eski fiyatlandırma modeline geri aynı şekilde taşıyabilirsiniz. Göz önünde bulundurun, abonelik sahibi veya katkıda bulunan izinleri fiyatlandırma modeli değiştirmek için gereklidir.

## <a name="automate-moving-to-the-new-pricing-model"></a>Yeni fiyatlandırma modeline taşıma otomatikleştirin

Aşağıdaki komut, Azure PowerShell modülünü gerektirir. Bkz. en son sürümü olup olmadığını denetlemek için [Azure PowerShell modülü yükleme](/powershell/azure/install-az-ps).

Azure PowerShell'in en son sürümünü oluşturduktan sonra ilk kez çalıştırmanız gerekir ``Connect-AzAccount``.

``` PowerShell
# To check if your subscription is eligible to adjust pricing models.
$ResourceID ="/subscriptions/<Subscription-ID-Here>/providers/microsoft.insights"
Invoke-AzResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action listmigrationdate `
 -Force
```

Bu aboneliğin fiyatlandırma modeli fiyatlandırma modelleri arasında taşınabilir isGrandFatherableSubscription altında True sonucunu gösterir. OptedInDate altında bir değer olmaması, bu aboneliği eski fiyatlandırma modeline ayarlanmış olduğu anlamına gelir.

```
isGrandFatherableSubscription optedInDate
----------------------------- -----------
                         True            
```

Geçirmek için bu aboneliği yeni fiyatlandırma modeline çalıştırın:

```powershell
$ResourceID ="/subscriptions/<Subscription-ID-Here>/providers/microsoft.insights"
Invoke-AzResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action migratetonewpricingmodel `
 -Force
```

Değişikliği yeniden başarılı olduğunu doğrulamak için:

```powershell
$ResourceID ="/subscriptions/<Subscription-ID-Here>/providers/microsoft.insights"
Invoke-AzResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action listmigrationdate `
 -Force
```

Geçiş başarılı olduysa, sonuç gibi görünmelidir:

```
isGrandFatherableSubscription optedInDate                      
----------------------------- -----------                      
                         True 2018-05-31T13:52:43.3592081+00:00
```

OptInDate artık zaman bu aboneliği yeni fiyatlandırma modeline kabul, bir zaman damgası içerir.

Eski fiyatlandırma modeline geri dönmek gerekiyorsa çalıştırırsınız:

```powershell
 $ResourceID ="/subscriptions/<Subscription-ID-Here>/providers/microsoft.insights"
Invoke-AzResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action rollbacktolegacypricingmodel `
 -Force
```

Sahip önceki betiği yeniden varsa ``-Action listmigrationdate``, aboneliğiniz, eski fiyatlandırma modeline iade edilmiş belirten bir boş optedInDate değeri görmelisiniz.

Aynı kiracısı altında barındırılan, geçirmek istediğiniz birden fazla aboneliğiniz varsa aşağıdaki betikler parçaları kullanarak kendi değişken oluşturabilirsiniz:

```powershell
#Query tenant and create an array comprised of all of your tenants subscription ids
$TenantId = <Your-tenant-id>
$Tenant =Get-AzSubscription -TenantId $TenantId
$Subscriptions = $Tenant.Id
```

Kiracınızdaki tüm abonelikleri için yeni fiyatlandırma modeline uygun olup olmadığını denetlemek için çalıştırabilirsiniz:

```powershell
Foreach ($id in $Subscriptions)
{
$ResourceID ="/subscriptions/$id/providers/microsoft.insights"
Invoke-AzResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action listmigrationdate `
 -Force
}
```

Betik daraltılmış üç diziden oluşturan kod oluşturma tarafından daha fazla. Bir dizi olan tüm abonelik kimliği oluşur ```isGrandFatherableSubscription``` True olarak ayarlayın ve optedInDate şu anda bir değeri yok. Tüm abonelikler şu anda yeni fiyatlandırma modeli, ikinci bir dizisi. Ve yeni fiyatlandırma modeline uygun olmayan abonelik kimlikleri kiracınızdaki doldurulmuş üçüncü bir dizi:

```powershell
[System.Collections.ArrayList]$Eligible= @{}
[System.Collections.ArrayList]$NewPricingEnabled = @{}
[System.Collections.ArrayList]$NotEligible = @{}

Foreach ($id in $Subscriptions)
{
$ResourceID ="/subscriptions/$id/providers/microsoft.insights"
$Result= Invoke-AzResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action listmigrationdate `
 -Force

     if ($Result.isGrandFatherableSubscription -eq $True -and [bool]$Result.optedInDate -eq $False)
     {
     $Eligible.Add($id)
     }

     elseif ($Result.isGrandFatherableSubscription -eq $True -and [bool]$Result.optedInDate -eq $True)
     {
     $NewPricingEnabled.Add($id)
     }

     elseif ($Result.isGrandFatherableSubscription -eq $False)
     {
     $NotEligible.add($id)
     }
}
```

> [!NOTE]
> Abonelik sayısına bağlı olarak yukarıdaki komut dosyasını çalıştırmak için biraz zaman alabilir. Öğeler için her bir dizi eklendikçe .add() yöntemi kullanımı nedeniyle PowerShell penceresi artan değerleri echo.

Artık üç diziye bölünmüş aboneliklerinizi sahip olduğunuza göre sonuçlarınızı dikkatli bir şekilde gözden geçirmelisiniz. Değişikliklerinizi gelecekte ihtiyacınız olursa kolayca geri dönebilirsiniz diziler içeriğini yedek kopyasını yapmak isteyebilirsiniz. Verdiyseniz, şu anda yeni eski fiyatlandırma modelinde bu görev artık ile gerçekleştirilmesi fiyatlandırma modeli tüm uygun abonelikleri dönüştürmek istedi:

```powershell
Foreach ($id in $Eligible)
{
$ResourceID ="/subscriptions/$id/providers/microsoft.insights"
Invoke-AzResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action migratetonewpricingmodel `
 -Force
}

```
