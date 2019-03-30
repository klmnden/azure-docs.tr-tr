---
title: Azure sanal makine ölçek kümeleri dikey olarak ölçeklendirme | Microsoft Docs
description: Bir sanal makine izleme uyarıları Azure Otomasyonu ile yanıt dikey olarak ölçeklendirme
services: virtual-machine-scale-sets
documentationcenter: ''
author: mayanknayar
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 16b17421-6b8f-483e-8a84-26327c44e9d3
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 08/03/2016
ms.author: manayar
ms.openlocfilehash: d3821f6a2bad56b46bccbcca8830be09ad1e44c7
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58648274"
---
# <a name="vertical-autoscale-with-virtual-machine-scale-sets"></a>Sanal makine ölçek ile dikey otomatik ölçeklendirme ayarlar

Bu makalede Azure dikey olarak ölçeklendirmek nasıl [sanal makine ölçek kümeleri](https://azure.microsoft.com/services/virtual-machine-scale-sets/) ile veya olmadan çıkış. Dikey Ölçek kümesinde olmayan VM'lerin ölçeklendirme için bkz [Azure Otomasyonu ile Azure sanal makine dikey olarak ölçeklendirme](../virtual-machines/windows/vertical-scaling-automation.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Dikey olarak da bilinen ölçeklendirme, *ölçeği* ve *ölçeğini*, artan veya azalan yanıt olarak bir iş yükü sanal makine (VM) boyutları anlamına gelir. Bu davranışı ile karşılaştırma [yatay ölçeklendirme](virtual-machine-scale-sets-autoscale-overview.md), de denilen *ölçeğini* ve *ölçeklendirme*, VM'lerin sayısını iş yüküne bağlı olarak burada değiştirilemez.

Çıkış, varolan bir VM'yi kaldırarak ve yeni bir tane ile değiştirerek anlamına gelir. Ne zaman artırın veya bir sanal makine ölçek VM boyutunu azaltın, var olan Vm'leri yeniden boyutlandırma ve diğer durumlarda yeni bir boyutta yeni Vm'leri dağıtmak düzeltmeniz gerekirken, verilerinizi korumak istediğiniz bazı durumlarda ayarlayın. Bu belge her iki durumda kapsar.

Dikey ölçeklendirme ne zaman yararlı olabilir:

* Sanal makineler üzerinde oluşturulmuş (örneğin sonları) altında kullanılan hizmetidir. VM boyutunu azaltma aylık maliyetleri azaltabilir.
* VM boyutu ile daha büyük bir isteğe bağlı ek VM'ler oluşturmak zorunda kalmadan başa çıkabilmelidir artırma.

Dikey ölçeklendirme, sanal makine ölçek kümesinden tetiklenen göre ölçüm tabanlı uyarıları olmasını ayarlayabilirsiniz. Uyarı etkinleştirildiğinde, Ölçek kümenizi ölçeklendirilebilir bir runbook yukarı veya aşağı ayarlayın, Tetikleyiciler bir Web kancasını tetikler. Dikey ölçeklendirme, aşağıdaki adımları izleyerek yapılandırılabilir:

1. Çalıştırma kapasitesine sahip bir Azure Otomasyonu hesabı oluşturun.
2. Azure Otomasyonu dikey ölçeklendirme runbook'lar sanal makine ölçek kümeleri için aboneliğinize içeri aktarın.
3. Bir Web kancası runbook uygulamanıza ekleyin.
4. Bir uyarı, bir Web kancası bildirimi kullanarak, sanal makine ölçek kümesine ekleyin.

> [!NOTE]
> İlk sanal makine boyutları için ölçeklendirilebilir boyutu nedeniyle geçerli sanal makine dağıtılır kullanılabilirlik kümesindeki diğer boyutlarının nedeniyle sınırlanabilir. Bu makalede kullanılan yayımlanan Otomasyon runbook'ları biz ilgileniriz bu durumda ve yalnızca içinde ölçeklendirme VM boyutu çiftleri aşağıda. Bu, bir Standard_D1v2 sanal makine değil aniden işler için standart_g5 için yukarı ölçeklendirilemez veya kaldırılacak Basic_A0 için ölçeği, anlamına gelir. Ayrıca kısıtlı sanal makine boyutları ölçeğini artırmanızı/azaltmanızı desteklenmiyor. Aşağıdaki boyutları çiftleri arasında ölçeklendirme seçebilirsiniz:
> 
> | Ölçeklendirme çifti VM boyutları |  |
> | --- | --- |
> | Basic_A0 |Basic_A4 |
> | Standard_A0 |Standard_A4 |
> | Standard_A5 |Standard_A7 |
> | Standard_A8 |Standard_A9 |
> | Standard_A10 |Standard_A11 |
> | Standard_A1_v2 |Standard_A8_v2 |
> | Standard_A2m_v2 |Standard_A8m_v2  |
> | Standard_B1s |Standard_B2s |
> | Standard_B1ms |Standard_B8ms |
> | Standard_D1 |Standard_D4 |
> | Standard_D11 |Standard_D14 |
> | Standard_DS1 |Standard_DS4 |
> | Standard_DS11 |Standard_DS14 |
> | Standard_D1_v2 |Standard_D5_v2 |
> | Standard_D11_v2 |Standard_D14_v2 |
> | Standard_DS1_v2 |Standard_DS5_v2 |
> | Standard_DS11_v2 |Standard_DS14_v2 |
> | Standard_D2_v3 |Standard_D64_v3 |
> | Standard_D2s_v3 |Standard_D64s_v3 |
> | Standard_DC2s |Standard_DC4s |
> | Standard_E2_v3 |Standard_E64_v3 |
> | Standard_E2s_v3 |Standard_E64s_v3 |
> | Standard_F1 |Standard_F16 |
> | Standard_F1s |Standard_F16s |
> | Standard_F2sv2 |Standard_F72sv2 |
> | Standard_G1 |Standard_G5 |
> | Standard_GS1 |Standard_GS5 |
> | Standard_H8 |Standard_H16 |
> | Standard_H8m |Standard_H16m |
> | Standart_L4s |Standart_L32s |
> | Standard_L8s_v2 |Standard_L80s_v2 |
> | İşler için Standard_M8ms  |İşler için standart_m128ms |
> | Standard_M32ls  |İşler için standart_m64ls |
> | İşler için standart_m64s  |İşler için standart_m128s |
> | Standard_M64  |Standard_M128 |
> | Standard_M64m  |Standard_M128m |
> | Standard_NC6 |Standard_NC24 |
> | Standard_NC6s_v2 |Standard_NC24s_v2 |
> | Standard_NC6s_v3 |Standard_NC24s_v3 |
> | Standard_ND6s |Standard_ND24s |
> | Standard_NV6 |Standard_NV24 |
> | Standard_NV6s_v2 |Standard_NV24s_v2 |
> 
> 

## <a name="create-an-azure-automation-account-with-run-as-capability"></a>Çalıştırma kapasitesine sahip bir Azure Otomasyonu hesabı oluşturma
Yapmanız gereken ilk şey, sanal makine ölçek kümesi örneklerine ölçeklendirme için kullanılan runbook'ları barındıran Azure Otomasyonu hesabı oluşturmaktır. Yakın zamanda [Azure Otomasyonu](https://azure.microsoft.com/services/automation/) ayarı oluşturan hizmet sorumlusunu otomatik olarak bir kullanıcı adına runbook'ları çalıştırmak için yaptığı "Farklı Çalıştır hesabı" özelliğini getirmiştir. Daha fazla bilgi için bkz.

* [Azure Farklı Çalıştır hesabıyla Runbook Kimlik Doğrulaması](../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Aboneliğinize Azure Otomasyon dikey ölçeklendirme runbook'ları alma

Sanal makine ölçek kümeleri dikey olarak ölçeklendirmek için gereken runbook'ları, Azure Otomasyonu Runbook Galerisi'nde zaten yayımlanır. İçeri aktarmak için bunları aboneliğinizi bu makaledeki adımları izleyin:

* [Azure Otomasyonu için runbook ve modül galerileri](../automation/automation-runbook-gallery.md)

Galeriye Gözat seçeneği runbook'ları menüsünden seçin:

![İçeri aktarılacak runbook'ları][runbooks]

İçeri aktarılması gereken runbook'lar gösterilir. Mı, dikey içeren veya çıkış içermeyen ölçeklendirme istediğinize bağlı runbook seçin:

![Runbook'lar Galerisi][gallery]

## <a name="add-a-webhook-to-your-runbook"></a>Runbook'unuza bir Web kancası Ekle

Runbook'ları aktardıktan sonra sanal makine ölçek kümesinden bir uyarı tetiklenebilir bir Web kancası runbook'a ekleyin. Runbook için bir Web kancası oluşturma ayrıntıları bu makalede açıklanmıştır:

* [Azure Otomasyonu Web kancaları](../automation/automation-webhooks.md)

> [!NOTE]
> Sonraki bölümde bu adresi gerekeceğinden, Web kancası iletişim kutusunu kapatmadan önce Web kancası URI kopyaladığınızdan emin olun.
> 
> 

## <a name="add-an-alert-to-your-virtual-machine-scale-set"></a>Uyarı, sanal makine ölçek kümesine ekleme

Aşağıda gösteren bir uyarı için bir sanal makine ölçek eklemek bir PowerShell Betiği ayarlanır. Ölçüm uyarı ateşlenmesine adını almak için şu makaleye başvurun: [Azure İzleyici otomatik ölçeklendirme ortak ölçümleri](../azure-monitor/platform/autoscale-common-metrics.md).

```powershell
$actionEmail = New-AzAlertRuleEmail -CustomEmail user@contoso.com
$actionWebhook = New-AzAlertRuleWebhook -ServiceUri <uri-of-the-webhook>
$threshold = <value-of-the-threshold>
$rg = <resource-group-name>
$id = <resource-id-to-add-the-alert-to>
$location = <location-of-the-resource>
$alertName = <name-of-the-resource>
$metricName = <metric-to-fire-the-alert-on>
$timeWindow = <time-window-in-hh:mm:ss-format>
$condition = <condition-for-the-threshold> # Other valid values are LessThanOrEqual, GreaterThan, GreaterThanOrEqual
$description = <description-for-the-alert>

Add-AzMetricAlertRule  -Name  $alertName `
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
> Dikey ölçeklendirme ve ilişkili hizmet kesintilerini, çok sık harekete önlemek için bir uyarı için makul süre penceresi yapılandırmak için önerilir. Bir pencerenin en az 20-30 dakika veya daha fazla göz önünde bulundurun. Herhangi bir kesintiye uğramasını önlemek gerekiyorsa ölçeklendirme yatay göz önünde bulundurun.
> 
> 

Uyarılar oluşturma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure İzleyici PowerShell hızlı başlangıç örnekleri](../azure-monitor/platform/powershell-quickstart-samples.md)
* [Azure İzleyici platformlar arası CLI hızlı başlangıç örnekleri](../azure-monitor/platform/cli-samples.md)

## <a name="summary"></a>Özet

Bu makalede, basit dikey ölçeklendirme örnekleri gösterilmiştir. Bu yapı taşları ile - Otomasyon hesabı, runbook'ları, Web kancaları, uyarılar - zengin çeşitli olayları özelleştirilmiş bir eylemler kümesi ile bağlanabilirsiniz.

[runbooks]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks.png
[gallery]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks-gallery.png