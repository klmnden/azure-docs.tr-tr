---
title: Otomatik ölçeklendirme sanal makine ölçek ayarlar Azure PowerShell ile | Microsoft Docs
description: Azure PowerShell ile nasıl sanal makine ölçek otomatik ölçeklendirme kurallar oluşturmak ayarlar
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 88886cad-a2f0-46bc-8b58-32ac2189fc93
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: iainfou
ms.openlocfilehash: 8928e56f353858234db314714d411a9c2990eb4e
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2018
ms.locfileid: "30201475"
---
# <a name="automatically-scale-a-virtual-machine-scale-set-with-azure-powershell"></a>Bir sanal makineyi ölçeği Azure PowerShell ile Ayarla otomatik olarak ölçeklendirin
Ölçek kümesi oluşturduğunuzda, çalıştırmak istediğiniz VM örneği sayısını tanımlayın. Uygulama talep değiştikçe otomatik olarak artırın veya VM örneği sayısını azaltın. Otomatik ölçeklendirme özelliği ile isteğe bağlı müşteri takip edin veya uygulamanızın yaşam döngüsü boyunca uygulama performans değişikliklerine yanıt verme olanak sağlar.

Bu makalede, Ölçek kümesi VM örnekleri performansını izlemek Azure PowerShell ile otomatik ölçeklendirme kurallarını oluşturulacağını gösterir. Otomatik ölçeklendirme kurallar artırın veya bu performans ölçümleri yanıta VM örnekleri sayısını azaltın. Bu adımları tamamlayabilmeniz için [Azure CLI 2.0](virtual-machine-scale-sets-autoscale-cli.md) veya [Azure portal](virtual-machine-scale-sets-autoscale-portal.md).


## <a name="prerequisites"></a>Önkoşullar
Otomatik ölçeklendirme kuralları oluşturmak için mevcut bir sanal makine gereksinim ölçek kümesi. Bir ölçek kümesi oluşturabileceğiniz [Azure portal](virtual-machine-scale-sets-create-portal.md), [Azure PowerShell](virtual-machine-scale-sets-create-powershell.md), veya [Azure CLI 2.0](virtual-machine-scale-sets-create-cli.md).

Otomatik ölçeklendirme kurallarını oluşturmayı kolaylaştırmak için ölçek kümesi için bazı değişkenler tanımlayın. Aşağıdaki örnek, ölçeği adlandırılmış Ayarla değişkenleri tanımlar *myScaleSet* kaynak grubunda adlı *myResourceGroup* ve *Doğu ABD* bölge. Aboneliğinizi kimliği elde edilir [Get-AzureRmSubscription](/powershell/module/azurerm.profile/get-azurermsubscription). Hesabınızla ilişkili birden çok aboneliğiniz varsa, yalnızca ilk abonelik döndürülür. Adları ve abonelik kimliği aşağıdaki gibi ayarlayın:

```powershell
$mySubscriptionId = (Get-AzureRmSubscription).Id
$myResourceGroup = "myResourceGroupScaleSet"
$myScaleSet = "myScaleSet"
$myLocation = "East US"
```


## <a name="create-a-rule-to-automatically-scale-out"></a>Otomatik olarak genişletmek için kural oluşturma
Uygulama talep artarsa, Ölçek VM örnekleri üzerindeki yük artar ayarlayın. Bu artan yükün tutarlı, ise yalnızca kısa isteğe bağlı yerine, Ölçek kümesi VM örnekleri sayısını artırmak için otomatik ölçeklendirme kuralları yapılandırabilirsiniz. Bu VM örnekleri oluşturulur ve uygulamalarınızı dağıtılan bunlara trafiğini yük dengeleyici üzerinden dağıtmak ölçek kümesini başlar. CPU veya disk, ne kadar süreyle uygulama yük belirli bir eşiği karşılamalı ve ölçek eklemek için kaç VM örnekleri kümesi gibi izlemek için hangi ölçümleri denetler.

Bir kuralla oluşturalım [yeni AzureRmAutoscaleRule](/powershell/module/AzureRM.Insights/New-AzureRmAutoscaleRule) bir ölçeği ortalama CPU yükü 5 dakikalık bir süre içinde % 70'den büyük olduğunda Ayarla VM örnekleri sayısını artırır. Kural harekete geçirdiğinde VM örneklerinin sayısını 20 oranında artar. Ölçek kümesi VM örnekleri az sayıda'de, dışlamayı `-ScaleActionScaleType` ve yalnızca belirtin `-ScaleActionValue` tarafından artırmak için *1* veya *2* örnekleri. Çok sayıda VM örneği, % %10 20 veya bir artış ölçek kümesi VM örnekleri daha uygun olabilir.

Bu kural için aşağıdaki parametreleri kullanılır:

| Parametre               | Açıklama                                                                                                         | Değer          |
|-------------------------|---------------------------------------------------------------------------------------------------------------------|----------------|
| *-MetricName*           | İzleme ve ölçek uygulamak için performans ölçüm Eylemler ayarlayın.                                                   | CPU Yüzdesi |
| *-TimeGrain*            | Ne sıklıkta ölçümleri analiz için toplanır.                                                                   | 1 dakika       |
| *-MetricStatistic*      | Toplanan ölçümleri analiz için nasıl toplanması gerektiğini tanımlar.                                                | Ortalama        |
| *-TimeWindow*           | Ölçüm ve eşik değerlerini karşılaştırılır önce izlenen süre miktarı.                                   | 10 dakika      |
| *-Operator*             | Ölçüm verilerinin eşikle karşılaştırmak için kullanılan işleci.                                                     | Büyüktür   |
| *-Eşiği*            | Otomatik ölçeklendirme kuralın bir eylemi tetikleyen neden değeri.                                                      | 70%            |
| *-ScaleActionDirection* | Ölçek kümesini yukarı veya aşağı kuralın geçerli olduğunda ölçeklendirmeniz gerekir tanımlar.                                             | Artır       |
| *–ScaleActionScaleType* | VM örneği sayısı yüzdesi miktarda değiştirilmesi gerektiğini belirtir.                                 | Yüzde değişikliği |
| *-ScaleActionValue*     | Kural harekete geçirdiğinde VM örnekleri yüzdesi değiştirilmelidir.                                            | 20             |
| *-ScaleActionCooldown*  | Otomatik ölçeklendirme eylemleri etkili olması için zamanı sağlayacak şekilde kural önce beklenecek süreyi yeniden uygulanır. | 5 dakika      |

Aşağıdaki örnek adlı bir nesne oluşturur *myRuleScaleOut* , bu ölçek kuralı tutar. *- MetricResourceId* abonelik kimliği, kaynak grubu adı ve ölçek kümesi adı için önceden tanımlanmış değişkenleri kullanır:

```powershell
$myRuleScaleOut = New-AzureRmAutoscaleRule `
  -MetricName "Percentage CPU" `
  -MetricResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -TimeGrain 00:01:00 `
  -MetricStatistic Average `
  -TimeWindow 00:10:00 `
  -Operator GreaterThan `
  -Threshold 70 `
  -ScaleActionDirection Increase `
  –ScaleActionScaleType PercentChangeCount `
  -ScaleActionValue 20 `
  -ScaleActionCooldown 00:05:00
```


## <a name="create-a-rule-to-automatically-scale-in"></a>İçinde otomatik olarak ölçeklendirmek için kural oluşturma
Bir akşam veya hafta sonu, uygulamayı isteğe bağlı azaltabilir. Bu azalmasına yükü bir süre boyunca tutarlı ise, Ölçek kümesi VM örnekleri sayısını azaltmak için otomatik ölçeklendirme kuralları yapılandırabilirsiniz. Bu ölçek eylemi geçerli talebi karşılamak için gerekli örnek sayısı yalnızca çalışacak şekilde ayarlayın, Ölçek çalıştırmak için maliyeti azaltır.

Bir kural oluştururken [yeni AzureRmAutoscaleRule](/powershell/module/AzureRM.Insights/New-AzureRmAutoscaleRule) bir ölçeği ortalama CPU yükünü 10 dakikalık bir süre içinde % 30 daha sonra düştüğünde Ayarla VM örnekleri sayısını azaltır. Kural harekete geçirdiğinde VM örneklerinin sayısını 20 oranında azalır. Aşağıdaki örnek adlı bir nesne oluşturur *myRuleScaleDown* , bu ölçek kuralı tutar. *- MetricResourceId* abonelik kimliği, kaynak grubu adı ve ölçek kümesi adı için önceden tanımlanmış değişkenleri kullanır:

```powershell
$myRuleScaleIn = New-AzureRmAutoscaleRule `
  -MetricName "Percentage CPU" `
  -MetricResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -Operator LessThan `
  -MetricStatistic Average `
  -Threshold 30 `
  -TimeGrain 00:01:00 `
  -TimeWindow 00:10:00 `
  -ScaleActionCooldown 00:05:00 `
  -ScaleActionDirection Decrease `
  –ScaleActionScaleType PercentChangeCount `
  -ScaleActionValue 20
```


## <a name="define-an-autoscale-profile"></a>Otomatik ölçeklendirme profili tanımlayın
Otomatik ölçeklendirme kurallarınızı ölçek kümesi ile ilişkilendirmek üzere bir profili oluşturun. Otomatik ölçeklendirme profili tanımlar varsayılan, minimum ve maksimum ölçek kümesi kapasitesi ve otomatik ölçeklendirme kurallarınızı ilişkilendirir. Otomatik ölçeklendirme profili ile oluşturma [yeni AzureRmAutoscaleProfile](/powershell/module/AzureRM.Insights/New-AzureRmAutoscaleProfile). Aşağıdaki örnek, varsayılan ve en az kapasitesi ayarlar *2* VM örnekleri ve en fazla *10*. Ölçek genişletme ve önceki adımlarda oluşturduğunuz kuralları'nda ölçek sonra eklenir:

```powershell
$myScaleProfile = New-AzureRmAutoscaleProfile `
  -DefaultCapacity 2  `
  -MaximumCapacity 10 `
  -MinimumCapacity 2 `
  -Rules $myRuleScaleOut,$myRuleScaleIn `
  -Name "autoprofile"
```


## <a name="apply-autoscale-rules-to-a-scale-set"></a>Otomatik ölçeklendirme kurallarını ölçek kümesine Uygula
Son adım, Ölçek kümenize otomatik ölçeklendirme profili uygulamaktır. Ölçek sonra uygulama talebe göre otomatik olarak içeri veya dışarı ölçeklendirebilirsiniz. Otomatik ölçeklendirme profili ile uygulama [Ekle AzureRmAutoscaleSetting](/powershell/module/AzureRM.Insights/Add-AzureRmAutoscaleSetting) gibi:

```powershell
Add-AzureRmAutoscaleSetting `
  -Location $myLocation `
  -Name "autosetting" `
  -ResourceGroup $myResourceGroup `
  -TargetResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -AutoscaleProfiles $myScaleProfile
```


## <a name="monitor-number-of-instances-in-a-scale-set"></a>Ölçek kümesi örneği monitör sayısı
Sayısı ve VM örneği durumunu görmek için ölçek kümesi örneklerinin bir listesini görüntüleyin. [Get-AzureRmVmssVM](/powershell/module/AzureRM.Compute/Get-AzureRmVmssVM). Ölçeği otomatik olarak ayarla çıkışı ölçeklendirir veya ölçek içinde otomatik olarak ölçeklendirir gibi sağlamayı VM örneği sağlama durumunda durumu gösterir. Aşağıdaki örnek ölçeği adlandırılmış Ayarla VM örneği durumunun görünümleri *myScaleSet* kaynak grubunda adlı *myResourceGroup*:

```powershell
Get-AzureRmVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
```


## <a name="autoscale-based-on-a-schedule"></a>Bir zamanlamaya göre otomatik ölçeklendirme
Önceki örneklerde otomatik olarak içeri veya dışarı temel ana ölçümleri CPU kullanımı gibi kümesiyle ölçeği genişletilmiş. Zamanlamada göre otomatik ölçeklendirme kuralları da oluşturabilirsiniz. Zamanlama tabanlı kurallar çekirdek çalışma saatleri gibi uygulama talep beklenen bir artış öncesinde VM örneklerinin sayısını otomatik olarak ölçeğini ve örnek sayısı daha az düşündüğünüz bir anda otomatik olarak ölçekleme izin ver hafta sonu gibi talep.

Konak yerine zamanlama ölçümleri temel otomatik ölçeklendirme kuralları oluşturmak için Azure portalını kullanın. Zamanlama tabanlı kurallar Azure PowerShell ile şu anda oluşturulamıyor.


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, yatay olarak ölçekleme ve artırmak veya azaltmak için otomatik ölçeklendirme kurallarını kullanmayı öğrendiniz *numarası* , Ölçek VM örnekleri kümesi. Ayrıca dikey olarak artırmak veya azaltmak VM örneği için ölçeklendirmek *boyutu*. Daha fazla bilgi için bkz: [dikey otomatik ölçeklendirme sanal makine ölçek kümeleri ile](virtual-machine-scale-sets-vertical-scale-reprovision.md).

Üzerindeki VM örneklerinize yönetme hakkında daha fazla bilgi için bkz: [Yönet sanal makine ölçek ayarlar Azure PowerShell ile](virtual-machine-scale-sets-windows-manage.md).

Tetikleyici, otomatik ölçeklendirme kurallarını taktirde uyarı oluşturma konusunda bilgi almak için bkz: [e-posta ve Web kancası Azure İzleyicisi'nde uyarı bildirimleri göndermek için otomatik ölçeklendirme eylemlerini kullanmak](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md). Ayrıca [e-posta ve Web kancası Azure İzleyicisi'nde uyarı bildirimleri göndermek için kullanım denetim günlüklerini](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md).
