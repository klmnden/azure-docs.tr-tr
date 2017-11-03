---
title: "Azure sanal makine ölçek kümeleri dikey olarak ölçeklendirmek | Microsoft Docs"
description: "Azure Otomasyonu uyarılarla izleme yanıt dikey olarak bir sanal makine ölçeklendirme"
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: madhana
editor: 
tags: azure-resource-manager
ms.assetid: 16b17421-6b8f-483e-8a84-26327c44e9d3
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 08/03/2016
ms.author: guybo
ms.openlocfilehash: 9159a5a9041864fe06785829121233379c46bb03
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="vertical-autoscale-with-virtual-machine-scale-sets"></a>Sanal makine ölçek kümeleri ile dikey otomatik ölçeklendirme
Bu makalede Azure dikey olarak ölçeklendirmek nasıl [sanal makine ölçek kümeleri](https://azure.microsoft.com/services/virtual-machine-scale-sets/) ile veya sağlama işleminin olmadan. Dikey Ölçek kümelerinde olmayan vm'lere ölçekleme için bkz [Azure Automation ile Azure sanal makine dikey olarak ölçeklendirmek](../virtual-machines/windows/vertical-scaling-automation.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Dikey olarak da bilinen ölçeklendirme, *ölçeği* ve *ölçeklendirmeyi azaltın*, artan veya azalan yanıt olarak bir iş yükü sanal makine (VM) boyutları anlamına gelir. Bu karşılaştırma [yatay ölçekleme](virtual-machine-scale-sets-autoscale-overview.md), de denilen *ölçeğini* ve *içinde ölçeklendirmek*, VM'lerin sayısını iş yüküne bağlı olarak burada değiştirilemez.

Sağlama işleminin, mevcut bir VM'yi kaldırarak ve yeni bir tane ile değiştirerek anlamına gelir. Artırma veya VM ölçek kümesindeki sanal makineleri boyutunu azaltma, bazı durumlarda, var olan sanal makineleri yeniden boyutlandırma ve diğer durumlarda, yeni boyutu, yeni sanal makineleri dağıtmak gerekirken, verilerinizi korumak istediğiniz. Bu belge her iki durumda da kapsar.

Dikey ölçekleme durumlarda yararlı olabilir:

* Sanal makineler üzerinde oluşturulmuş bir (örneğin sonları) altında kullanılan hizmetidir. VM boyutunu küçültmeyi aylık maliyetlerini azaltabilir.
* Ek sanal makineleri oluşturmadan büyük taleple başa çıkacak şekilde VM boyutunu artırma.

Dikey ölçeklendirmeyi Tetiklenmiş göre ölçüm dayalı uyarılar, VM ölçek kümesi'den olacak şekilde ayarlayabilirsiniz. Uyarı etkinleştirildiğinde, bir Web Kancası Yukarı veya aşağı, Ölçek ölçeklenebilen bir runbook ayarlamak bu Tetikleyicileri tetikler. Dikey ölçeklendirme, aşağıdaki adımları izleyerek yapılandırılabilir:

1. Çalıştır özelliğine sahip bir Azure Otomasyonu hesabı oluşturun.
2. Azure Otomasyonu dikey ölçek runbook'ları VM ölçek kümesi için aboneliğinizi alın.
3. Bir Web kancası runbook'a ekleyin.
4. Bir uyarı, VM ölçek bir Web kancası bildirim kullanarak kümesine ekleyin.

> [!NOTE]
> Dikey otomatik ölçeklendirmeyi yalnızca belirli aralıklarında VM boyutları gerçekleşebilir. Başka bir ölçeklendirme karar vermeden önce her boyutu belirtimleri karşılaştırma (daha yüksek sayı her zaman belirtmez büyük VM boyutu). Aşağıdaki boyutları çiftleri ölçeklendirmek seçebilirsiniz:
> 
> | VM boyutları çifti ölçeklendirme |  |
> | --- | --- |
> | Standard_A0 |Standard_A11 |
> | Standard_D1 |Standard_D14 |
> | Standard_DS1 |Standard_DS14 |
> | Standard_D1v2 |Standard_D15v2 |
> | Standard_G1 |Standard_G5 |
> | Standard_GS1 |Standard_GS5 |
> 
> 

## <a name="create-an-azure-automation-account-with-run-as-capability"></a>Çalıştır özelliğine sahip Azure Automation hesabı oluşturma
Yapmanız gereken ilk şey VM ölçek kümesi örneklerinin ölçeklendirmek için kullanılan runbook'ları barındıracak bir Azure Otomasyonu hesabı oluşturmaktır. Yakın zamanda [Azure Otomasyonu](https://azure.microsoft.com/services/automation/) otomatik olarak çalışan runbook'ları bir kullanıcı adına çok kolay için ayarı yukarı hizmet sorumlusu kılan "Farklı Çalıştır hesabı" özellik sunulmuştur. Daha fazla bilgiyi bu konuda aşağıdaki makalede:

* [Azure Farklı Çalıştır hesabıyla Runbook Kimlik Doğrulaması](../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Aboneliğinizi Azure Otomasyon dikey ölçek runbook'ları alma
VM ölçek kümesi dikey olarak ölçeklendirmek için gereken runbook'ları, Azure Automation Runbook Galerisi zaten yayımlanır. İçeri aktarmak için bunları aboneliğinizi bu makaledeki adımları izleyin:

* [Azure Otomasyonu Runbook ve modül galerileri](../automation/automation-runbook-gallery.md)

Runbook'ları menüsünden Gözat galeri seçenek belirleyin:

![İçeri aktarılacak runbook'ları][runbooks]

İçeri aktarılması gereken runbook'lar gösterilir. Ölçek ile veya olmadan sağlama işleminin dikey istemediğinizi üzerinde tabanlı runbook seçin:

![Runbook Galerisi][gallery]

## <a name="add-a-webhook-to-your-runbook"></a>Bir Web kancası runbook'a ekleyin
Alınan sonra gerekir runbook'ları VM ölçek kümesi bir uyarıdan tarafından tetiklenebilir için bir Web kancası runbook'a ekleyin. Runbook için bir Web kancası oluşturma ayrıntılarını bu makalede açıklanan:

* [Azure Otomasyonu Web kancası](../automation/automation-webhooks.md)

> [!NOTE]
> Bu sonraki bölümde ihtiyaç duyacağınız Web kancası iletişim kutusunu kapatmadan önce Web kancası URI kopyaladığınızdan emin olun.
> 
> 

## <a name="add-an-alert-to-your-vm-scale-set"></a>Bir uyarı, VM ölçek kümesi'ne Ekle
Bir VM ölçek kümesi için bir uyarı ekleme gösteren bir PowerShell Betiği aşağıdadır. Hakkında uyarı tetiklenecek ölçüm adı almak için aşağıdaki makaleye bakın: [Azure İzleyici otomatik ölçeklendirmeyi ortak ölçümleri](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md).

```
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail user@contoso.com
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri <uri-of-the-webhook>
$threshold = <value-of-the-threshold>
$rg = <resource-group-name>
$id = <resource-id-to-add-the-alert-to>
$location = <location-of-the-resource>
$alertName = <name-of-the-resource>
$metricName = <metric-to-fire-the-alert-on>
$timeWindow = <time-window-in-hh:mm:ss-format>
$condition = <condition-for-the-threshold> # Other valid values are LessThanOrEqual, GreaterThan, GreaterThanOrEqual
$description = <description-for-the-alert>

Add-AzureRmMetricAlertRule  -Name  $alertName `
                            -Location  $location `
                            -ResourceGroup $rg `
                            -TargetResourceId $id `
                            -MetricName $metricName `
                            -Operator  $condition `
                            -Threshold $threshold `
                            -WindowSize  $timeWindow `
                            -TimeAggregationOperator Average `
                            -Actions $actionEmail, $actionWebhook `
                            -Description $description
```

> [!NOTE]
> Dikey ölçekleme ve ilişkili hizmet kesintisi, çok sık tetikleme önlemek için makul zaman penceresi uyarı için yapılandırılması önerilir. Pencerenin en az 20-30 dakika veya daha fazla göz önünde bulundurun. Tüm kesintiye uğramaması gerekiyorsa ölçeklendirme yatay göz önünde bulundurun.
> 
> 

Uyarılar oluşturma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure İzleyici PowerShell hızlı başlangıç örnekleri](../monitoring-and-diagnostics/insights-powershell-samples.md)
* [Azure İzleyici platformlar arası CLI hızlı başlangıç örnekleri](../monitoring-and-diagnostics/insights-cli-samples.md)

## <a name="summary"></a>Özet
Bu makalede basit dikey ölçekleme örnekler gösterilmiştir. Bu yapı taşları ile - Automation hesabı, runbook'ları, Web kancalarını, uyarıları - zengin olayları çeşitli eylemler özelleştirilmiş bir dizi bağlanabilir.

[runbooks]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks.png
[gallery]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks-gallery.png
