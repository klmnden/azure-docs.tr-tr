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
ms.openlocfilehash: 01f3995a80375f2deada13c36c48600243a17623
ms.sourcegitcommit: 1902adaa68c660bdaac46878ce2dec5473d29275
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2019
ms.locfileid: "57733424"
---
# <a name="move-your-automation-account-to-another-subscription"></a>Otomasyon hesabınızı başka bir aboneliğe taşıma

Azure, bazı kaynakları yeni kaynak grubu ya da Azure portal, PowerShell, Azure CLI veya REST API aracılığıyla aynı kiracının aboneliğe taşıma olanağı sağlar. İşlemi hakkında daha fazla bilgi için bkz. [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../../azure-resource-manager/resource-group-move-resources.md). Otomasyon hesapları taşınabilir kaynakları biridir ancak Otomasyon hesabınızı taşırken, gereken ek adımlar vardır.

Otomasyon hesabınıza taşıma için üst düzey adımlar şunlardır:

* Çözümlerinizi Kaldır
* Çalışma alanının bağlantısını Kaldır
* Otomasyonu hesabını taşıma
* Silin ve farklı çalıştır hesaplarını yeniden oluşturma
* Çözümlerinizi yeniden etkinleştirin

## <a name="remove-solutions"></a>Çözümleri kaldırma

Çalışma alanınızdan Otomasyon hesabınızda, değişiklik ve sayım bağlantısını kaldırmak için güncelleştirme yönetimi ve Vm'leri başlatma/durdurma sırasında saat çözümleri kapalı çalışma alanınızdan kaldırılmalıdır.

Kaynak grubunuzda her seçin **çözüm** tıklatıp **Sil**. Üzerinde **kaynakları silme** sayfasında, kaldırılacak ve kaynakları onaylayın **Sil**.

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

İçin **VM başlatma/durdurma** çözümü, ayrıca gereken çözümü tarafından oluşturulan uyarı kuralları kaldırın.

Azure portalında, kaynak grubuna gidin ve seçin **uyarılar** altında **izleme**. Üzerinde **uyarılar** sayfasında **uyarı kurallarını yönet**

![Uyarılar sayfası gösteren yönetmek için uyarı kurallarında tıklayın](../media/move-account/alert-rules.png)

Üzerinde **kuralları** sayfasında, bu kaynak grubunda yapılandırılmış uyarıların bir listesini görmelisiniz. **Vm'leri başlatma/durdurma** çözüm 3 uyarı kuralları oluşturur

* AutoStop_VM_Child
* ScheduledStartStop_Parent
* SequencedStartStop_Parent

Bu 3 uyarı kuralları seçin ve tıklayın **Sil**. Bu eylem, bu uyarı kuralları kaldırır.

![Seçilen kurallar sayfası ve alınan kuralları silindi](../media/move-account/delete-rules.png)

> [!NOTE]
> Üzerinde hiçbir uyarı kuralı görmüyorsanız **kuralları** sayfasında, değişiklik **durumu** gösterilecek **devre dışı** uyarıları gibi bunları devre dışı bırakmış olabilir.

Uyarı kuralları kaldırdıktan sonra Başlat/Durdur VM çözümü için bildirimleri için oluşturulan eylem grubunu kaldırmak gerekir.

Azure portalında Git **İzleyici**seçin **uyarılar**, tıklatıp **Eylem grupları yönetme**.

Listeden, eylem grubu seçin, adı olacaktır **StartStop_VM_Notification**. Eylem gruplarını sayfasında tıklatın **Sil**

![Eylem grubu sayfası DELETE tuşuna basarak,](../media/move-account/delete-action-group.png)

Benzer şekilde, PowerShell ile bir eylem grubu silebilirsiniz. Bu eylem yapılır [Remove-AzureRmActionGroup](/powershell/module/azurerm.insights/remove-azurermactiongroup) aşağıdaki örnekte görüldüğü gibi cmdlet:

```azurepowershell-interactive
Remove-AzureRmActionGroup -ResourceGroupName <myResourceGroup> -Name StartStop_VM_Notification
```

## <a name="unlink-your-workspace"></a>Çalışma alanının bağlantısını Kaldır

Azure portalında, Git, **Otomasyon hesabı**. Altında **ilgili kaynakları**, tıklayın **bağlantılı çalışma**. Tıklayın **çalışma alanının bağlantısını Kaldır** Otomasyon hesabınızdan çalışma alanının bağlantısının kaldırılması.

![Bir Otomasyon hesabı bir çalışma alanının bağlantısı kaldırılıyor](../media/move-account/unlink-workspace.png)

## <a name="move-your-automation-account"></a>Otomasyon hesabınızı Taşı

Önceki tüm öğeleri kaldırıldıktan sonra Otomasyon hesabınızı ve kendi runbook'ları kaldırmak devam edebilirsiniz. Azure portalında Otomasyon hesabınızı kaynak grubuna gidin. Seçin **taşıma** ardından **taşımak, başka bir aboneliğe**.

![Kaynak grubu sayfasını seçerek başka bir aboneliğe taşıma](../media/move-account/move-resources.png)

Taşımak istediğiniz, kaynak grubunuzda kaynakları seçin. Dahil olun, **Otomasyon hesabı**, **Runbook**, ve **Log Analytics çalışma alanı** kaynakları.

Taşıma tamamlandıktan sonra her şeyi çalışma için alınması gereken ek adımlar vardır.

## <a name="recreate-run-as-accounts"></a>Farklı Çalıştır hesapları olarak yeniden oluşturun.

[Farklı Çalıştır hesapları](../manage-runas-account.md) Azure kaynaklarıyla kimlik doğrulaması için Azure Active Directory'de Hizmet sorumlusu oluşturma. Abonelikleri değiştirdiğinizde, var olan farklı çalıştır hesabı Otomasyon hesabı tarafından artık kullanılamaz.

Yeni Abonelik içindeki Otomasyon hesabınıza gidin ve seçin **farklı çalıştır hesapları** altında **hesap ayarları**. Farklı Çalıştır hesapları artık tam olarak göster görürsünüz.

![Tamamlanmamış gösteren hesapları çalıştırma](../media/move-account/run-as-accounts.png)

Her farklı çalıştır hesabı ve tıklayın **özellikleri** sayfasında **Sil** farklı çalıştır hesabı silinecek.

> ! [NOT] Oluşturun veya farklı çalıştır hesapları aşağıdaki iletiyi görürsünüz görüntülemek için izinleri yoksa `You do not have permissions to create an Azure Run As account (service principal) and grant the Contributor role to the service principal.`. Bir farklı çalıştır hesabı yapılandırmak için gereken izinler hakkında bilgi edinmek için [farklı çalıştır hesaplarını yapılandırmak için gereken izinler](../manage-runas-account.md#permissions).

Farklı Çalıştır hesapları silindikten sonra tıklayın **Oluştur** üzerinde **Azure farklı çalıştır hesabı**. Üzerinde **ekleme Azure farklı çalıştır hesabı** sayfasında **Oluştur** farklı çalıştır hesabı ve hizmet sorumlusu oluşturmak için. İle önceki adımları yineleyin **Azure Klasik farklı çalıştır hesabı**.

## <a name="enable-solutions"></a>Çözümleri etkinleştirme

Farklı Çalıştır hesaplarını yeniden sonra taşımadan önce kaldırdığınız çözümleri yeniden etkinleştirmeniz. Etkinleştirmek için **değişiklik izleme ve stok** ve **güncelleştirme yönetimi**, ilgili özellik Otomasyon hesabınızı seçin. Taşınan üzerinden Log Analytics çalışma alanını seçin ve tıklayın **etkinleştirme**.

![Taşınan Otomasyon hesabınızdaki çözümleri yeniden etkinleştirin](../media/move-account/reenable-solutions.png)

Mevcut Log Analytics çalışma alanı bağlanmakta olduğunuz beri çözümlerinizi eklenmesi, makine yeniden görüntülenecektir.

Vm'leri başlatma/durdurma sırasında yoğun olmayan saatlerde çözümü yeniden etkinleştirmek için çözümü yeniden dağıtmak gerekir. Altında **ilgili kaynakları**seçin **VM başlatma/durdurma**. Tıklayın **daha fazla bilgi edinin ve çözümü etkinleştirmek** tıklatıp **Oluştur** dağıtımı başlatmak için.

Üzerinde **Çözüm Ekle** Log Analytics çalışma alanı ve Otomasyon hesabı seçin.  

![Tamamlanmamış gösteren hesapları çalıştırma](../media/move-account/add-solution-vm.png)

Çözümü yapılandırma hakkında ayrıntılı yönergeler için bkz: [sırasında Azure Otomasyonu çözümde yoğun olmayan saatlerde Vm'leri başlatma/durdurma](../automation-solution-vm-management.md)

## <a name="post-move-verification"></a>POST taşıma doğrulama

Taşıma tamamlandıktan sonra her şeyin beklendiği gibi çalıştığından emin olmak için bunları Otomasyon hesabınıza farklı senaryolarda doğrulamak emin olun. Aşağıdaki tabloda, geçiş tamamlandıktan sonra doğrulanması gereken görevler listesini gösterir:

|Özellik|Testler|Bağlantı sorunlarını giderme|
|---|---|---|
|Runbook'lar|Runbook başarıyla çalıştırın ve Azure kaynaklarına bağlama.|[Runbook sorunlarını giderme](../troubleshoot/runbooks.md)
| Kaynak Denetimi|El ile eşitleme, kaynak denetim deposunu üzerinde çalıştırabilirsiniz.|[Kaynak denetimi tümleştirmesi](../source-control-integration.md)|
|Değişiklik İzleme ve Stok|Geçerli Envanter verilerini makinelerinizden gördüğünüz doğrulayın.|[Değişiklik izleme için sorun giderme](../troubleshoot/change-tracking.md)|
|Güncelleştirme Yönetimi|Makinelerinizi bakın ve iyi durumda olduklarını doğrulayın</br>Bir test yazılım güncelleştirme dağıtımı'nı çalıştırın.|[Güncelleştirme yönetimi sorunlarını giderme](../troubleshoot/update-management.md)|

## <a name="next-steps"></a>Sonraki adımlar

Azure'da kaynakları taşıma hakkında daha fazla bilgi için bkz: [Azure'da kaynakları taşıma](../../azure-resource-manager/move-support-resources.md)
