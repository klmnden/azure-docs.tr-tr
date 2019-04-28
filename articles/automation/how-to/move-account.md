---
title: Azure Otomasyonu hesabınızı başka bir aboneliğe taşıma
description: Bu makalede Otomasyon hesabınızın başka bir aboneliğe taşıma
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/11/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 59d433bfb888eaa41cc8f66bdf3ad28c16efbe5c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61302619"
---
# <a name="move-your-azure-automation-account-to-another-subscription"></a>Azure Otomasyonu hesabınızı başka bir aboneliğe taşıma

Azure, bazı kaynakları yeni kaynak grubuna veya aboneliğe taşıma olanağı sağlar. Azure portalı, PowerShell, Azure CLI veya REST API'si aracılığıyla kaynaklarını taşıyabilirsiniz. İşlemi hakkında daha fazla bilgi için bkz. [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../../azure-resource-manager/resource-group-move-resources.md). 

Azure Otomasyonu hesaplarını taşınabilir kaynakları biridir. Bu makalede, Otomasyon hesaplarını başka bir kaynak veya aboneliğe taşıma adımları öğreneceksiniz.

Otomasyon hesabınızı taşıma için üst düzey adımlar şunlardır:

1. Çözümlerinizi kaldırın.
2. Çalışma alanının bağlantısını Kaldır.
3. Otomasyonu hesabını taşıma.
4. Silin ve farklı çalıştır hesaplarını yeniden oluşturun.
5. Çözümlerinizi yeniden etkinleştirin.

## <a name="remove-solutions"></a>Çözümleri kaldırma

Çalışma alanınızdan Otomasyon hesabınızın bağlantısını kaldırmak için bu çözümlerin çalışma alanınızdan kaldırılması gerekir:
- **Değişiklik izleme ve stok**
- **Güncelleştirme yönetimi** 
- **Mesai saatleri dışında başlatma/durdurma Vm'leri** 

Kaynak grubunuzda, her bir çözüm bulmak ve seçmek **Sil**. Üzerinde **kaynakları silme** sayfasında, kaldırılacak ve kaynakları onaylayın **Sil**.

![Çözümleri Azure portalından silin.](../media/move-account/delete-solutions.png)

İle aynı görevi başarmak [Remove-AzureRmResource](/powershell/module/azurerm.resources/remove-azurermresource) aşağıdaki örnekte gösterildiği gibi cmdlet:

```azurepowershell-interactive
$workspaceName = <myWorkspaceName>
$resourceGroupName = <myResourceGroup>
Remove-AzureRmResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "ChangeTracking($workspaceName)" -ResourceGroupName $resourceGroupName
Remove-AzureRmResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "Updates($workspaceName)" -ResourceGroupName $resourceGroupName
Remove-AzureRmResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "Start-Stop-VM($workspaceName)" -ResourceGroupName $resourceGroupName
```

### <a name="additional-steps-for-startstop-vms"></a>Vm'leri başlatma/durdurma için ek adımlar

İçin **Vm'leri başlatma/durdurma** çözümü, ayrıca gereken çözümü tarafından oluşturulan uyarı kuralları kaldırın.

Azure portalında, kaynak grubuna gidin ve seçin **izleme** > **uyarılar** > **uyarı kurallarını yönet**.

![Uyarıları yönetme uyarı kuralları sayfasını gösteren seçimi](../media/move-account/alert-rules.png)

Üzerinde **kuralları** sayfasında, bu kaynak grubunda yapılandırılmış uyarıların bir listesini görmelisiniz. **Vm'leri başlatma/durdurma** çözüm üç uyarı kuralları oluşturur:

* AutoStop_VM_Child
* ScheduledStartStop_Parent
* SequencedStartStop_Parent

Bu üç uyarı kuralları, seçin ve ardından **Sil**. Bu eylem, bu uyarı kuralları kaldırır.

![Seçili kuralları için silme onayı isteyen kurallar sayfası](../media/move-account/delete-rules.png)

> [!NOTE]
> Uyarı kuralları görmüyorsanız **kuralları** sayfasında, değişiklik **durumu** göstermek için **devre dışı** uyarıları, bunları devre dışı bırakmış olabilir çünkü.

Uyarı kuralları kaldırıldığında için oluşturulan eylem grubunu kaldırmaya **Vm'leri başlatma/durdurma** çözüm bildirimleri.

Azure portalında **İzleyici** > **uyarılar** > **Eylem grupları yönetme**.

Seçin **StartStop_VM_Notification** listeden. Eylem grubu sayfasında **Sil**.

![Eylem grubu sayfası, silmeyi seçme](../media/move-account/delete-action-group.png)

Benzer şekilde, PowerShell kullanarak, Eylem grubunu silebilmeniz için [Remove-AzureRmActionGroup](/powershell/module/azurerm.insights/remove-azurermactiongroup) aşağıdaki örnekte görüldüğü gibi cmdlet:

```azurepowershell-interactive
Remove-AzureRmActionGroup -ResourceGroupName <myResourceGroup> -Name StartStop_VM_Notification
```

## <a name="unlink-your-workspace"></a>Çalışma alanının bağlantısını Kaldır

Azure portalında **Otomasyon hesabı** > **ilgili kaynakları** > **bağlantılı çalışma**. Seçin **çalışma alanının bağlantısını Kaldır** Otomasyon hesabınızdan çalışma alanının bağlantısının kaldırılması.

![Bir Otomasyon hesabı bir çalışma alanının bağlantısını Kaldır](../media/move-account/unlink-workspace.png)

## <a name="move-your-automation-account"></a>Otomasyon hesabınızı Taşı

Önceki öğeleri kaldırdıktan sonra Otomasyon hesabınızı ve kendi runbook'ları kaldırmak devam edebilirsiniz. Azure portalında Otomasyon hesabınızı kaynak grubuna gidin. Seçin **taşıma** > **taşımak, başka bir aboneliğe**.

![Kaynak grubu sayfasını, başka bir aboneliğe taşıma](../media/move-account/move-resources.png)

Kaynak grubunuzda taşımak istediğiniz kaynakları seçin. Dahil olun, **Otomasyon hesabı**, **Runbook**, ve **Log Analytics çalışma alanı** kaynakları.

Taşıma tamamlandıktan sonra her şey çalışır hale getirmeniz için gereken ek adımlar vardır.

## <a name="re-create-run-as-accounts"></a>Farklı Çalıştır hesaplarını yeniden oluşturun

[Farklı Çalıştır hesapları](../manage-runas-account.md) Azure kaynaklarıyla kimlik doğrulaması için Azure Active Directory'de Hizmet sorumlusu oluşturma. Abonelikleri değiştirdiğinizde, Otomasyon hesabı artık mevcut farklı çalıştır hesabını kullanır.

Yeni Abonelik içindeki Otomasyon hesabınıza gidin ve seçin **farklı çalıştır hesapları** altında **hesap ayarları**. Farklı Çalıştır hesapları artık tam olarak göster görürsünüz.

![Tamamlanmamış farklı çalıştır hesapları](../media/move-account/run-as-accounts.png)

Her farklı çalıştır hesabı seçin. Üzerinde **özellikleri** sayfasında **Sil** farklı çalıştır hesabı silinecek.

> [!NOTE]
> Farklı Çalıştır hesaplarını görüntülemek veya oluşturmak için izniniz yok, şu iletiyi görürsünüz: `You do not have permissions to create an Azure Run As account (service principal) and grant the Contributor role to the service principal.` Bir farklı çalıştır hesabı yapılandırmak için gereken izinler hakkında bilgi edinmek için [farklı çalıştır hesaplarını yapılandırmak için gereken izinler](../manage-runas-account.md#permissions).

Farklı Çalıştır hesaplarını silindikten sonra seçin **Oluştur** altında **Azure farklı çalıştır hesabı**. Üzerinde **ekleme Azure farklı çalıştır hesabı** sayfasında **Oluştur** farklı çalıştır hesabı ve hizmet sorumlusu oluşturmak için. İle önceki adımları yineleyin **Azure Klasik Run As hesabı**.

## <a name="enable-solutions"></a>Çözümleri etkinleştirme

Farklı Çalıştır hesaplarını yeniden oluşturduktan sonra taşımadan önce kaldırdığınız çözümleri yeniden etkinleştirmeniz. Açmak için **değişiklik izleme ve stok** ve **güncelleştirme yönetimi**, ilgili özellik Otomasyon hesabınızı seçin. Taşıdığınız üzerinden Log Analytics çalışma alanını seçip **etkinleştirme**.

![Taşınan Otomasyon hesabınızdaki çözümleri yeniden etkinleştirin](../media/move-account/reenable-solutions.png)

Mevcut Log Analytics çalışma alanı bağlandığınıza göre çözümlerinizi eklenmesi olan makinelere görünür hale gelir.

Açmak için **Vm'leri başlatma/durdurma** sırasında yoğun olmayan saatlerde çözümü, çözümü yeniden dağıtmak ihtiyacınız olacak. Altında **ilgili kaynakları**seçin **Vm'leri başlatma/durdurma** > **daha fazla bilgi edinin ve çözümü etkinleştirmek** > **Oluştur** dağıtımı başlatmak için.

Üzerinde **Çözüm Ekle** sayfasında, Log Analytics çalışma alanı ve Otomasyon hesabınızı seçin.  

![Çözüm menü ekleme](../media/move-account/add-solution-vm.png)

Çözümü yapılandırma hakkında ayrıntılı yönergeler için bkz. [Başlat/Durdur Vm'leri çalışma saatleri çözümü Azure automation'da](../automation-solution-vm-management.md).

## <a name="post-move-verification"></a>Doğrulama sonrası Taşı

Taşıma tamamlandıktan sonra aşağıdaki doğrulanması gereken görevler listesini kontrol edin:

|Özellik|Testler|Bağlantı sorunlarını giderme|
|---|---|---|
|Runbook'lar|Runbook başarıyla çalıştırın ve Azure kaynaklarına bağlama.|[Runbook sorunlarını giderme](../troubleshoot/runbooks.md)
| Kaynak denetimi|El ile eşitleme, kaynak denetim deposunu üzerinde çalıştırabilirsiniz.|[Kaynak denetimi tümleştirmesi](../source-control-integration.md)|
|Değişiklik izleme ve stok|Geçerli Envanter verilerini makinelerinizden gördüğünüz doğrulayın.|[Değişiklik izleme için sorun giderme](../troubleshoot/change-tracking.md)|
|Güncelleştirme yönetimi|Makinelerinizi bakın ve iyi durumda olduklarını doğrulayın.</br>Bir test yazılım güncelleştirme dağıtımı'nı çalıştırın.|[Güncelleştirme yönetimi sorunlarını giderme](../troubleshoot/update-management.md)|

## <a name="next-steps"></a>Sonraki adımlar

Azure'da kaynakları taşıma hakkında daha fazla bilgi için bkz: [Azure'da kaynakları taşıma](../../azure-resource-manager/move-support-resources.md).
