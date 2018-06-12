---
title: Kullanım ve Azure İzleyicisi'nde tahmini maliyetleri izleme
description: Azure monitör kullanımı ve tahmini maliyetleri sayfa kullanmanın işlemine genel bakış
author: dalekoetke
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/31/2018
ms.author: mbullwin
ms.reviewer: Dale.Koetke
ms.component: ''
ms.openlocfilehash: edfcc244105403ae33251777c560d4cc21dfe5cb
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35264291"
---
# <a name="monitoring-usage-and-estimated-costs"></a>Kullanım ve tahmini maliyetleri izleme

Azure portal'ın İzleyici hub **kullanım ve tahmini maliyetleri** sayfa çekirdek özellikler gibi izleme kullanımını açıklar [bildirimleri ölçümleri, uyarı](https://azure.microsoft.com/pricing/details/monitor/), [Azure günlük analizi ](https://azure.microsoft.com/pricing/details/log-analytics/), ve [Azure Application Insights](https://azure.microsoft.com/pricing/details/application-insights/). Nisan 2018 önce kullanılabilir fiyatlandırma planları müşteriler için Analytics sunar ve bu da Öngörüler satın alınan günlük analizi kullanım içerir.

Bu sayfada, kullanıcılar kendi kaynak kullanımı son 31 gün için abonelik başına toplanan görüntüleyebilir. Detaylandırma bileşenler 31 gün süresi içinde kullanım eğilimlerini gösterir. Çok miktarda veri birlikte bu tahmin, bu nedenle alınması gerekip sayfa yüklerken lütfen bekleyin.

Bu örnek, izleme kullanımı ve sonuçta elde edilen maliyet tahmini gösterir:

![Kullanım ve tahmini maliyetleri portalı ekran görüntüsü](./media/monitoring-usage-and-estimated-costs/001.png)

Son 31 gün süresi içinde kullanım eğilimleri gösteren bir grafik açmak için aylık kullanım sütununda bağlantıyı seçin:

![Çubuk grafik ekran düğüm başına dahil](./media/monitoring-usage-and-estimated-costs/002.png)

Başka bir benzer kullanım işte ve maliyet özeti. Bu örnek, bir abonelik yeni Nisan 2018 tüketim tabanlı fiyatlandırma modelde gösterir. Tüm düğüm tabanlı faturalama eksikliği unutmayın. Şimdi yeni bir ortak ölçerde veri alımı ve günlük analizi ve Application Insights için bekletme raporlanır.

![Kullanım ve tahmini maliyetleri portalı ekran görüntüsü - Nisan 2018 fiyatlandırma](./media/monitoring-usage-and-estimated-costs/003.png)

## <a name="new-pricing-model"></a>Yeni bir fiyatlandırma modeli

Nisan 2018 içinde bir [fiyatlandırma modeli yeni izleme yayımlanan](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/).  Bu, bulut kolay, tüketim tabanlı fiyatlandırma özellikleri. Yalnızca ne, düğüm tabanlı taahhüt kullandığınız için ödeme yaparsınız. Yeni fiyatlandırma modeli ayrıntılarını kullanılabilir [bildirimleri ölçümleri, uyarı](https://azure.microsoft.com/pricing/details/monitor/), [günlük analizi](https://azure.microsoft.com/pricing/details/log-analytics/) ve [Application Insights](https://azure.microsoft.com/pricing/details/application-insights/). 

Günlük analizi veya Application Insights 2 Nisan 2018 sonra müşteriler eklenmesi için yeni bir fiyatlandırma modelini tek seçenektir. Bu hizmetler zaten kullanan müşteriler için yeni fiyatlandırma modeli taşıma isteğe bağlıdır.

## <a name="assessing-the-impact-of-the-new-pricing-model"></a>Yeni fiyatlandırma modeli etkisini değerlendirme
Yeni fiyatlandırma modeli, kendi izleme kullanım düzenlerini esas alarak her müşteri farklı etkileri sahip olur. Günlük analizi veya Application Insights 2 Nisan 2018 önce kullanmakta olduğunuz müşteriler **kullanım ve tahmini maliyet** sayfa Azure İzleyicisi'nde yeni fiyatlandırma modeli taşırsanız maliyetleri herhangi bir değişikliği tahmin eder. Bir abonelik yeni modeline taşıma olanağı sağlar. Çoğu müşteri için yeni fiyatlandırma modeli yararlı olacaktır. Özellikle yüksek veri kullanım desenlerini veya daha yüksek maliyet bölgelerdeki müşteriler için bu durumda geçerli olmayabilir.

Üzerinde seçtiğiniz maliyetleriniz abonelikler için tahmini görmek için **kullanım ve tahmini maliyetleri** sayfasında, sayfanın üstüne yakın mavi başlığını seçin. En yeni fiyatlandırma modeli benimsenmesi düzeyi olduğu için her seferinde bir Bu abonelik yapmak en iyisidir.

![Kullanımını izleme ve tahmini maliyetleri yeni fiyatlandırma modeli ekran görüntüsü](./media/monitoring-usage-and-estimated-costs/004.png)

Yeni sayfa, önceki sayfanın yeşil bir başlık ile benzer bir sürümünü gösterir:

![Kullanımını izleme ve tahmini maliyetleri geçerli fiyatlandırma modeli ekran görüntüsü](./media/monitoring-usage-and-estimated-costs/005.png)

Sayfa ayrıca yeni fiyatlandırma modeli karşılık ölçümler farklı bir kümesini gösterir. Bu liste bir örnek verilmiştir:

- Insight ve düğüm başına Analytics\Overage
- Insight ve düğüm başına Analytics\Included
- Uygulama Insights\Basic fazla kullanım verileri
- Uygulama Insights\Included verileri

Yeni fiyatlandırma modeli düğümü tabanlı dahil edilen veri ayırma sahip değil. Bu nedenle, bu veri alım ölçümler adlı bir yeni ortak veri alım ölçer birleştirilir **paylaşılan Services\Data alım**. 

Günlük analizi ya da daha yüksek maliyetleri bölgelerdeki Application Insights alınan verileri başka bir değişiklik yoktur. Bu yüksek maliyetli bölgeler için veri Yeni bölgesel ölçümler ile gösterilir. Örnek **veri alımı (BİZE Batı Merkezi)**.

> [!NOTE]
> Abonelik başına maliyetleri değil düğümü yetkilendirmeler Operations Management Suite (OMS) abonelik başına hesap düzeyinde içine faktörü tahmini. Hesabı temsilcinize için daha ayrıntılı bir tartışma yeni fiyatlandırma modeli Bu durumda başvurun.

## <a name="new-pricing-model-and-operations-management-suite-subscription-entitlements"></a>Yeni fiyatlandırma modeli ve Operations Management Suite aboneliği yetkilendirmeler

Microsoft Operations Management Suite E1 ve E2 satın alan müşteriler için düğüm başına veri alım yetkilendirmeleri uygun [günlük analizi](https://www.microsoft.com/cloud-platform/operations-management-suite) ve [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-pricing#the-price-plans). Bu yetkilendirmeler günlük analizi çalışma alanları veya Application Insights kaynaklar için belirli bir aboneliğe almak için: 

- Aboneliğin fiyatlandırma modeli öncesi Nisan 2018 modelinde kalmalıdır.
- Günlük analizi çalışma alanları "fiyatlandırma katmanı düğüm başına (OMS)" kullanmanız gerekir.
- Application Insights kaynakları "planı fiyatlandırma Kurumsal" kullanmalısınız.

Kuruluşunuzun satın aldığı, paketi düğümlerinin sayısına bağlı olarak bazı taşımayı abonelikleri yeni fiyatlandırma modeli için yararlı olabilir, ancak bu dikkat gerektirir. Genel olarak, yalnızca yukarıda açıklandığı gibi öncesi Nisan 2018 modelinde kalmak için önerilir.

> [!WARNING]
> Kuruluşunuz Microsoft Operations Management Suite E1 ve E2 satın aldıysa, aboneliklerinizi öncesi Nisan 2018 fiyatlandırma modeli korumak en iyisidir. 
>

## <a name="changes-when-youre-moving-to-the-new-pricing-model"></a>Yeni fiyatlandırma modeli taşırken değişiklikleri

Yeni fiyatlandırma modeli günlük analizi ve Application Insights seçenekleri yalnızca tek bir katman (veya plan) fiyatlandırma basitleştirir. Bir abonelik yeni fiyatlandırma modeli taşıma:

- (Azure Kaynak Yöneticisi'nde "pergb2018" olarak adlandırılır) yeni bir GB başına katmanına her günlük analizi için fiyatlandırma katmanını değiştirin
- Tüm kurumsal planı Application Insights kaynaklarında temel plana değiştirilir.

Maliyet tahmini bu değişikliklerin etkisini gösterir.

> [!WARNING]
> Burada dağıtmak için Azure Resource Manager veya PowerShell kullanırsanız, önemli bir Not [günlük analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-template-workspace-configuration) veya [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-powershell) yeni taşınmış bir abonelikte fiyatlandırma modeli. Bir fiyatlandırma katmanı/planı "pergb2018" dışında için günlük analizi veya "Temel" için Application Insights belirtirseniz, yerine fiyatlandırma katmanı/plan, geçersiz bir belirtme nedeniyle dağıtım başarısız olan bu başarılı olur **ancak yalnızca geçerli kullanın Fiyatlandırma katmanı/plan** (Bu, geçersiz bir fiyatlandırma katmanı iletisi oluşturulur günlük analizi ücretsiz katmanı geçerli değildir).
>

## <a name="moving-to-the-new-pricing-model"></a>Yeni fiyatlandırma modeline taşıma

Bir abonelik için yeni fiyatlandırma modeli benimsemeye karar verdiyseniz, seçin **model seçimi fiyatlandırma** seçeneği en üstündeki **kullanım ve tahmini maliyetleri** sayfa:

![Kullanımını izleme ve tahmini maliyetleri yeni fiyatlandırma modeli ekran görüntüsü](./media/monitoring-usage-and-estimated-costs/006.png)

**Model seçimi fiyatlandırma** sayfası açılır. Her önceki sayfada görüntülenen Aboneliklerin listesini gösterir:

![Fiyatlandırma modeli seçimi ekran görüntüsü](./media/monitoring-usage-and-estimated-costs/007.png)

Yeni fiyatlandırma modeli bir aboneliği taşımak için yalnızca kutusunu seçin ve ardından **kaydetmek**. Bu gibi durumlarda, eski fiyatlandırma modeli geri aynı şekilde taşıyabilirsiniz. Bu abonelik sahibi göz önünde bulundurun veya katkıda bulunan izinleri fiyatlandırma modeli değiştirmek için gereklidir.

## <a name="automate-moving-to-the-new-pricing-model"></a>Yeni fiyatlandırma modeli taşıma otomatikleştirme

Aşağıdaki komut, Azure PowerShell modülü gerektirir. Bkz. en son sürümü olup olmadığını denetlemek için [yükleme Azure PowerShell Modülü](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-6.1.0).

Azure PowerShell'in en son sürümünü oluşturduktan sonra ilk çalışması gerekebilir ``Connect-AzureRmAccount``.

``` PowerShell
# To check if your subscription is eligible to adjust pricing models.
$ResourceID ="/subscriptions/<Subscription-ID-Here>/providers/microsoft.insights"
Invoke-AzureRmResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action listmigrationdate `
 -Force
```

Bu aboneliğin fiyatlandırma modeli fiyatlandırma modelleriyle arasında taşınabilmesi sonucunda isGrandFatherableSubscription True gösterir. Bu abonelik şu anda eski fiyatlandırma modelidir optedInDate altında bir değer olmaması anlamına gelir.

```
isGrandFatherableSubscription optedInDate
----------------------------- -----------
                         True            
```

Geçirmek için bu aboneliği yeni fiyatlandırma modeli için çalıştırın:

```PowerShell
$ResourceID ="/subscriptions/<Subscription-ID-Here>/providers/microsoft.insights"
Invoke-AzureRmResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action migratetonewpricingmodel `
 -Force
```

Değiştirme başarılı yeniden çalıştır olduğunu onaylamak için:

```PowerShell
$ResourceID ="/subscriptions/<Subscription-ID-Here>/providers/microsoft.insights"
Invoke-AzureRmResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action listmigrationdate `
 -Force
```

Geçiş başarılı olursa, Sonuç kümenizi gibi görünmelidir:

```
isGrandFatherableSubscription optedInDate                      
----------------------------- -----------                      
                         True 2018-05-31T13:52:43.3592081+00:00
```

OptInDate şimdi ne zaman bu abonelik, yeni fiyatlandırma modeli seçti, zaman damgası içeriyor.

Eski fiyatlandırma modeline geri dönmek gerekiyorsa, çalıştırın:

```PowerShell
 $ResourceID ="/subscriptions/<Subscription-ID-Here>/providers/microsoft.insights"
Invoke-AzureRmResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action rollbacktolegacypricingmodel `
 -Force
```

Ardından sahip önceki komut dosyasını yeniden, ``-Action listmigrationdate``, aboneliğinizi döndürdü fiyatlandırma modeli eski belirten bir boş optedInDate değer görmelisiniz.

Aynı Kiracı altında barındırılan, geçirmek istediğiniz birden fazla aboneliğiniz varsa, aşağıdaki betikler parçalarını kullanarak kendi değişken oluşturabilirsiniz:

```PowerShell
#Query tenant and create an array comprised of all of your tenants subscription ids
$TenantId = <Your-tenant-id>
$Tenant =Get-AzureRMSubscription -TenantId $TenantId
$Subscriptions = $Tenant.Id
```

Kiracınızda bulunan tüm abonelikleri yeni fiyatlandırma modeli için uygun olup olmadığını denetlemek için çalıştırabilirsiniz:

```PowerShell
Foreach ($id in $Subscriptions)
{
$ResourceID ="/subscriptions/$id/providers/microsoft.insights"
Invoke-AzureRmResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action listmigrationdate `
 -Force
}
```

Komut dosyası Gelişmiş üç dizi oluşturan kod oluşturma tarafından daha fazla. Bir dizi olan tüm abonelik kimliği oluşur ```isGrandFatherableSubscription``` True olarak ayarlayın ve optedInDate şu anda bir değeri yok. Yeni fiyatlandırma modeli üzerinde şu anda herhangi bir aboneliği ikinci bir dizi. Ve yeni fiyatlandırma modeli için uygun olmayan abonelik kimlikleri kiracınızda doldurulmuş üçüncü bir dizi:

```PowerShell
[System.Collections.ArrayList]$Eligible= @{}
[System.Collections.ArrayList]$NewPricingEnabled = @{}
[System.Collections.ArrayList]$NotEligible = @{}

Foreach ($id in $Subscriptions)
{
$ResourceID ="/subscriptions/$id/providers/microsoft.insights"
$Result= Invoke-AzureRmResourceAction `
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
> Abonelik sayısına bağlı olarak yukarıdaki komut dosyasını çalıştırmak için biraz zaman alabilir. Öğeleri her dizisine eklendikçe .add() yönteminin kullanılması nedeniyle PowerShell penceresinde artacak değerleri echo.

Üç diziye bölünmüş aboneliklerinizi sahip olduğunuza göre sonuçlarınızı dikkatle gözden geçirmelidir. Böylece gelecekte için ihtiyacınız olursa kolayca yaptığınız değişiklikleri geri almak bir yedek diziler içeriğini kopyasını isteyebilirsiniz. Siz karar verdiyseniz, şu anda yeni eski fiyatlandırma modeli Bu görev artık ile gerçekleştirilmesi fiyatlandırma modeli tüm uygun abonelikleri Dönüştür istedi:

```PowerShell
Foreach ($id in $Eligible)
{
$ResourceID ="/subscriptions/$id/providers/microsoft.insights"
Invoke-AzureRmResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action migratetonewpricingmodel `
 -Force
}

```