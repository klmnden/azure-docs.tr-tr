---
title: Azure'da sanal makine ölçek kümeleri için bakım bildirimlerini işleme | Microsoft Docs
description: Azure'da sanal makine ölçek kümeleri için bakım bildirimleri görüntüleyin ve Self Servis Bakımı Başlat.
services: virtual-machine-scale-sets
documentationcenter: ''
author: shants123
editor: ''
tags: azure-service-management,azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/09/2018
ms.author: shants
ms.openlocfilehash: 4ce984686c2bb320d5d32771d31b81cdec1153af
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38314409"
---
# <a name="handling-planned-maintenance-notifications-for-virtual-machine-scale-sets"></a>Planlı bakım bildirimlerini sanal makine ölçek kümeleri için işleme

Azure sanal makine konak altyapısının güvenilirlik, performans ve güvenliğini iyileştirmek için düzenli olarak güncelleştirmeler yapar. Barındırma ortamına düzeltme eki veya yükseltme ve donanım yetkisini alma gibi değişiklik güncelleştirmelerdir. Bu güncelleştirmelerin çoğu barındırılan sanal makinelere sunucuları etkilenmeden gerçekleştirilir. Ancak, güncelleştirmeleri bir etkiye sahip olduğu durumlar da vardır:

- Bakım yeniden başlatma gerektirmez, Azure VM konak güncelleştirilirken duraklatmak için yerinde geçiş kullanır. Hata etki alanı tarafından uygulanan hata etki alanı bu rebootful olmayan bakım işlemleridir ve hiçbir uyarı sistem durumu sinyali alınırsa ilerleme durduruldu.

- Bakım yeniden başlatma gerektirirse, ne zaman bunu planlı bakım, bir bildirim alın. Bu durumlarda, burada başlatabilir bakım kendiniz bir zaman penceresi verilir ne zaman çalıştığını sizin için.


Dalgaları içinde bir yeniden başlatma gerektiren planlı bakım zamanlandı. Her dalgada farklı kapsamı (bölge) vardır.

- Bir dalga müşterilere bir bildirim ile başlar. Varsayılan olarak, abonelik sahibi ve ikincil sahipler bildirim gönderilir. Daha fazla alıcı ve e-posta, SMS ve Web kancaları gibi Mesajlaşma seçenekleri için Azure'u kullanarak bildirimleri ekleyebileceğiniz [etkinlik günlüğü uyarıları](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).  
- Bildirimi, anında bir *Self Servis penceresi* kullanılabilir hale getirilir. Bu pencere sırasında bu dalgada dahil olduğu sanal makinelerinizin bulabilir ve Bakım zamanlama kendi gereksinimlerine göre proaktif olarak başlat.
- Sonra Self Servis penceresinde bir *zamanlanan bakım penceresinden* başlar. Belirli bir noktada bu penceresi sırasında Azure zamanlar ve gerekli bakım sanal makineniz için geçerlidir. 

Bakımı Başlat ve ne zaman Azure bakım otomatik olarak başlatılacak bilerek sanal makinenizi yeniden başlatmanız yeterli süre vermek amacıyla iki pencereleri aşağıdakiler amacı olan.


Azure portalı, PowerShell, REST API kullanabilirsiniz ve sorgulamak için ölçek kümenizdeki sanal makine için bakım pencerelerini CLI vm'leri ve Self Servis Bakımı Başlat.

  
## <a name="should-you-start-maintenance-during-the-self-service-window"></a>Self Servis penceresi sırasında bakım başlamanız gerekir?  

Aşağıdaki yönergeler bu özelliği kullanın ve kendi zaman bakım başlatmak karar vermenize yardımcı olmalıdır.

> [!NOTE] 
> Self Servis bakım tüm Vm'leriniz için kullanılabilir olmayabilir. Önleyici yeniden dağıtma, sanal makine için kullanılabilir olup olmadığını belirlemek için Aranan **Şimdi Başlat** bakım durumu. Self Servis bakım şu anda Cloud Services (Web/çalışan rolü) ve Service Fabric için kullanılabilir değildir.


Self Servis bakım kullanarak dağıtımları için önerilmez **kullanılabilirlik kümeleri** bu yüksek oranda kullanılabilir ayarlar, belirli bir zamanda yalnızca bir güncelleme etki alanı burada etkilenir olduğundan. 

- Bakım tetikleyicisi Azure'a bırakın. Yeniden başlatma gerektiren bakım, Bakımı güncelleme etki alanı güncelleme etki alanlarının mutlaka bakım sırasıyla almalarını güncelleme etki alanına göre yapılır ve güncelleştirme etki alanları arasında 30 dakikalık duraklama olduğunu unutmayın.
- Kapasite (1/güncelleştirme etki alanı sayısı) bazıları geçici kaybı önemliyse, kolayca için bakım süresi boyunca ek örnekleri ayırarak dengelenebilecek.
- Gerektirmez ve bakım için yeniden başlatma, güncelleştirme hata etki alanı düzeyinde uygulanır. 
    
**Yoksa** Self Servis bakım aşağıdaki senaryolarda kullanın: 

- Vm'lerinizi sık kapatırsanız ya da el ile DevTest Labs'i kullanarak otomatik kapatma veya izleyen bir zamanlama, kullanarak bunu bakım durumu dönmek ve bu nedenle ek kapalı kalma durumlarına neden.
- Kısa süreli Vm'lerde bildiğinizden nedeni bakım dalgasının bitiminden önce silinecek. 
- Güncelleştirme tutulması için istenen yerel (kısa ömürlü) disk depolanan büyük durumuna sahip iş yükleri için. 
- Burada, genellikle sanal makinenizi yeniden boyutlandırma durumlarda, bakım durumu olarak geri döndürülemedi. 
- 15 dakika önce bakım kapanma başlangıcı proaktif yük devretme veya iş yükünüz, normal şekilde kapatılmasını etkinleştirmek zamanlanmış olaylar benimseyen durumunda.

**Kullanım** VM'nizi zamanlanan bakım aşaması sırasında kesintisiz çalıştırılacak planlıyor ve yukarıda belirtilen karşı göstergelerden hiçbiri geçerli olan Self Servis Bakım. 

Self Servis bakım aşağıdaki durumlarda kullanmak en iyisidir:

- Bir tam bakım penceresi yönetim ya da son müşteri iletişim kurması gerekir. 
- Belirli bir tarihte bakım tamamlanması gerekir. 
- Bakım, örneğin, çok katmanlı uygulaması güvenli kurtarma garanti sırasını denetlemek gerekir.
- 30 dakikadan fazla VM kurtarma zamanı arasında iki güncelleştirme etki alanları (UD) ihtiyacınız vardır. Güncelleştirme etki alanları arasındaki zamanı denetlemek için bakım teker teker Vm'leri bir güncelleştirme etki alanınızda (UD) tetiklemesi gerekir.

 
## <a name="view-virtual-machine-scale-sets-impacted-by-maintenance-in-the-portal"></a>Portalda bakım etkilenen sanal makine ölçek kümeleri görüntüleyin

Bir planlı bakım dalgası zamanlandıysa, Azure portalını kullanarak gelecek bakım dalgasından etkilenen sanal makine ölçek kümeleri listesini görüntüleyebilirsiniz. 

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **tüm hizmetleri** sol gezinti çubuğunda ve **sanal makine ölçek kümeleri**.
3. Üzerinde **sanal makine ölçek kümeleri** sayfasında **sütunları Düzenle** kullanılabilir sütunlar listesini açmak için üst seçeneği.
4. İçinde **kullanılabilir sütunlar bölümünde**seçin **Self Servis Bakım** içine taşımak için ok tuşlarını kullanın ve öğe **Seçili sütunlar** listesi. Açılır listede, geçiş **kullanılabilir sütunlar** bölümü **tüm** için **özellikleri** yapmak **Self Servis Bakım** bulunacak öğe daha kolay. Yapılandırmasını tamamladıktan **Self Servis Bakım** öğesi **Seçili sütunlar** bölümünden **Uygula** sayfanın alt kısmındaki düğmesi. 

Aşağıdaki adımları izleyerek, sonra **Self Servis Bakım** sütunu, sanal makine ölçek kümeleri listesinde görünür. Her sanal makine ölçek kümesi, Self Servis bakım sütun için aşağıdaki değerlerden biri olabilir:

| Değer | Açıklama |
|-------|-------------|
| Evet | Sanal makine ölçek kümenizdeki en az bir sanal makine bir Self Servis penceresinde bulunuyor. Bu Self Servis penceresi sırasında herhangi bir zamanda bakım başlayabilirsiniz. | 
| Hayır | Bir Self Servis penceresinde etkilenen sanal makine ölçek kümesindeki hiçbir sanal makine vardır. | 
| - | Sanal makine ölçek kümeleri, planlı bakım dalgası parçası değildir.| 

## <a name="notification-and-alerts-in-the-portal"></a>Bildirim ve Portalı'nda uyarılar

Azure aboneliğine sahip ve ikincil sahipler gruba e-posta göndererek planlı bakım için zamanlama iletişim kurar. Azure etkinlik günlüğü uyarıları oluşturarak, bu iletişim için ek alıcılar ve kanallar ekleyebilirsiniz. Daha fazla bilgi için bkz. [Azure etkinlik günlüğü ile abonelik etkinliğini izleme] (.. / monitoring-and-diagnostics/monitoring-overview-activity-logs.md)

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Soldaki menüde **İzleyici**. 
3. İçinde **İzleyici - uyarılar (Klasik)** bölmesinde tıklayın **+ etkinlik günlüğü uyarısı Ekle**.
4. Bilgileri tamamlayın **etkinlik günlüğü uyarısı Ekle** kümesinde aşağıdaki emin olun ve sayfa **ölçütleri**:
   - **Olay kategorisi**: Hizmet durumu
   - **Hizmetleri**: sanal makine ölçek kümeleri ve sanal makineler
   - **Tür**: planlı bakım 
    
Etkinlik günlüğü uyarılarının nasıl yapılandırılacağı hakkında daha fazla bilgi edinmek için [etkinlik günlüğü uyarıları oluşturma](../monitoring-and-diagnostics/monitoring-activity-log-alerts.md)
    
    
## <a name="start-maintenance-on-your-virtual-machine-scale-set-from-the-portal"></a>Portaldan sanal makine ölçek kümesinde Bakımı Başlat

Sanal makine ölçek kümeleri, genel bakış arama sırasında daha fazla bakım görüyor olmanız ilgili ayrıntıları. Sanal makine ölçek kümesindeki en az bir sanal makine içinde planlı bakım dalgası yer alıyorsa, yeni bir bildirim Şerit sayfanın üst kısımda eklenir. Gitmek için bildirim Şerit tıklayabilirsiniz **Bakım** hangi sanal makine örneği planlı bakımdan etkilenir görmek için sayfayı. 

Buradan, kutusu etkilenen sanal makineye karşılık gelen ve ardından tıklayarak kontrol ederek bakım başlatmak mümkün olmayacak **Başlat Bakım** seçeneği.

Bakım başlattıktan sonra etkilenen sanal makineler, sanal makine ölçek kümesindeki bakım yapmak ve geçici olarak kullanılamıyor. Self Servis penceresi kaçırdıysanız, yine de, sanal makine ölçek kümesi, Azure tarafından korunur, pencere görmeniz mümkün olacaktır.
 
## <a name="check-maintenance-status-using-powershell"></a>PowerShell kullanarak bakım durumunu denetleme

Sanal makine ölçek kümelerinizdeki Vm'lere bakım için zamanlanmış zaman görmek için Azure Powershell kullanabilirsiniz. Planlı bakım bilgileri kullanılabilir [Get-AzureRmVmss](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss) cmdlet'ini kullandığınızda `-InstanceView` parametresi.
 
Planlı bakım yoksa bakım bilgileri döndürülür. Cmdlet'i, VM örneği etkileyen herhangi bir bakım zamanlanırsa, herhangi bir bakım bilgi döndürmez. 

```powershell
Get-AzureRmVmss -ResourceGroupName rgName -VMScaleSetName vmssName -InstanceId id -InstanceView
```

Aşağıdaki özellikler altında MaintenanceRedeployStatus döndürülür: 
| Değer | Açıklama   |
|-------|---------------|
| IsCustomerInitiatedMaintenanceAllowed | Bakım VM üzerinde şu anda başlatabilirsiniz olup olmadığını gösterir ||
| PreMaintenanceWindowStartTime         | Sanal makinenizde bakım başlatabilir, bakım penceresinin Self Servis başına ||
| PreMaintenanceWindowEndTime           | Son bakım sanal makinenizde başlatabilir, bakım penceresinin Self Servis ||
| MaintenanceWindowStartTime            | Zamanlanmış bakım başlangıcını Azure vm'nizdeki bakım işlemini başlatır ||
| MaintenanceWindowEndTime              | Azure vm'nizdeki bakım işlemini başlatır zamanlanmış bakım penceresinin sonu ||
| LastOperationResultCode               | VM'de bakım başlatmak için son girişimi sonucu ||



### <a name="start-maintenance-on-your-vm-instance-using-powershell"></a>PowerShell kullanarak VM örneğinde Bakımı Başlat

Bakım, bir VM'de başlatabilirsiniz **IsCustomerInitiatedMaintenanceAllowed** true kullanarak kümesine [Set-AzureRmVmss](/powershell/module/azurerm.compute/set-azurermvmss) cmdlet'iyle `-PerformMaintenance` parametresi.

```powershell
Set-AzureRmVmss -ResourceGroupName rgName -VMScaleSetName vmssName -InstanceId id -PerformMaintenance 
```

## <a name="check-maintenance-status-using-cli"></a>CLI kullanarak bakım durumunu denetleme

Planlı bakım bilgileri kullanarak görülebilir [az vmss seznamu](/cli/azure/vmss?view=azure-cli-latest#az-vmss-list-instances).
 
Planlı bakım yoksa bakım bilgileri döndürülür. Zamanlanmış bir bakım yok ise, sanal makine örneği etkiler, komutun herhangi bir bakım bilgi döndürmez. 

```azure-cli
az vmss list-instances -g rgName -n vmssName --expand instanceView
```

Aşağıdaki özellikler, her sanal makine örneği için MaintenanceRedeployStatus altında döndürülür: 
| Değer | Açıklama   |
|-------|---------------|
| IsCustomerInitiatedMaintenanceAllowed | Bakım VM üzerinde şu anda başlatabilirsiniz olup olmadığını gösterir ||
| PreMaintenanceWindowStartTime         | Sanal makinenizde bakım başlatabilir, bakım penceresinin Self Servis başına ||
| PreMaintenanceWindowEndTime           | Son bakım sanal makinenizde başlatabilir, bakım penceresinin Self Servis ||
| MaintenanceWindowStartTime            | Zamanlanmış bakım başlangıcını Azure vm'nizdeki bakım işlemini başlatır ||
| MaintenanceWindowEndTime              | Azure vm'nizdeki bakım işlemini başlatır zamanlanmış bakım penceresinin sonu ||
| LastOperationResultCode               | VM'de bakım başlatmak için son girişimi sonucu ||


### <a name="start-maintenance-on-your-vm-instance-using-cli"></a>CLI kullanarak VM örneğinde Bakımı Başlat

Aşağıdaki çağrısı, bir sanal makine örneğinde bakım başlatacak `IsCustomerInitiatedMaintenanceAllowed` ayarlanır true.

```azure-cli
az vmss perform-maintenance -g rgName -n vmssName --instance-ids id
```

## <a name="faq"></a>SSS


**S: neden sanal makinelerim şimdi yeniden başlatmak gerekiyor mu?**

**Y:** sanal makinenin kullanılabilirlik çoğu güncelleştirmeler ve yükseltmeler Azure platformuna yönelik durum olsa da burada biz olamaz önlemek Azure'da barındırılan sanal makineleri yeniden durumlar vardır. Şu sanal makinelerin yeniden başlatılmasına yol açacak sunucularımızı yeniden bırakmamızı değişiklikler birikti.

**S: önerilerinizi yüksek kullanılabilirlik için bir kullanılabilirlik kümesi'ni kullanarak takip ettiğim aldıysam güvenli miyim?**

**Y:** bir kullanılabilirlik dağıtılan sanal makinelerin ayarlayın veya sanal makine ölçek kümeleri güncelleştirme etki alanları (UD) kavram vardır. Bakımın gerçekleştirildiği sırada Azure UD kısıtlamasına uyar ve (aynı kullanılabilirlik kümesindeki) farklı ud'den sanal makineleri yeniden başlatmaz.  Azure ayrıca sanal makinelerin sonraki grubuna taşımadan önce en az 30 dakika bekler. 

Yüksek kullanılabilirlik hakkında daha fazla bilgi için bkz: [azure'da sanal makineler için kullanılabilirlik ve bölgeler](../virtual-machines/windows/regions-and-availability.md).

**S: nasıl planlı bakımla ilgili bildirim?**

**Y:** planlı bakım dalgası bir veya daha fazla Azure bölgesine bir zamanlama ayarlayarak başlar. Kısa süre sonra abonelik sahipleri (bir e-posta abonelik başına) için bir e-posta bildirimi gönderilir. Etkinlik günlüğü uyarıları kullanarak ek Kanallar ve bu bildirim için alıcı yapılandırılabilir. Burada planlı bakım zaten zamanlandı bir bölge için bir sanal makine dağıtma durumda bildirim almayacaksınız ancak yerine VM'nin bakım durumunu denetleme gerekir.

**S: portalı, Powershell veya CLI planlı bakıma dair herhangi bir göstergesi göremiyorum. Ne oldu?**

**Y:** planlı bakımla ilgili bilgi kullanılabilir bir etkilenecek edecek sanal makineleri için planlı bakım dalgası sırasında. Diğer bir deyişle, verileri değil görürseniz, nedeni bakım dalgasının zaten tamamlanmış (veya başlatılmadı,) veya sanal makinenizin zaten güncelleştirilmiş bir sunucuda barındırılıyor olabilir.

**S: ne zaman tam olarak sanal Makinem etkileneceğini bilmenin bir yolu var mı?**

**Y:** zamanlamayı ayarladığınızda, birkaç gün zaman penceresi tanımlarız. Bununla birlikte, sunucuların (ve VM'lerin) Bu pencere içindeki tam sıralaması bilinmiyor. Kendi Vm'leri için kesin zaman bilmek isteyen müşteriler [zamanlanmış olaylar](../virtual-machines/windows/scheduled-events.md) sanal makinede bulunan sorgu gelen ve VM yeniden başlatmadan önce 15 dakikalık bildirim alırsınız.

**S: sanal Makinem yeniden başlatmanızı ne kadar sürer?**

**Y:** VM'NİZİN boyutuna bağlı olarak, yeniden başlatma için Self Servis bakım penceresi sırasında birkaç dakika sürebilir. Yeniden başlatma sırasında Azure tarafından başlatılan bir zamanlanan bakım penceresinde başlatma işlemi genellikle yaklaşık 25 dakika sürer. Cloud Services (Web/çalışan rolü), sanal makine ölçek kümeleri veya kullanılabilirlik kümeleri kullanmanız durumunda, 30 dakika arasında her grup, Vm'leri (UD) zamanlanmış bakım penceresi sırasında verileceğini unutmayın. 

**S: Vm'lerimi üzerinde herhangi bir bakım bilgi göremiyorum. Sorun nedir?**

**Y:** neden değil gördüğünüz herhangi bir bakım bilgi Vm'lerinizde birkaç nedeni vardır:
   - Microsoft iç olarak işaretlenmiş bir aboneliği kullanıyorsunuz.
   - Sanal makinelerinizin bakım için zamanlanmış değil. Bunun nedeni bakım dalgasının, sanal makineleriniz tarafından artık etkilendiğini şekilde değiştirilebilir veya iptal sona erdiğini olabilir.
   - Sahip olmadığınız **Bakım** sütun için VM listesi görünümünüze eklendi. Biz varsayılan görünüme bu sütun eklenmiş olsa da varsayılan olmayan sütunları görmek için yapılandırılmış müşteriler el ile eklemelisiniz **Bakım** kendi VM liste görünümünde sütun.

**S: VM'me ikinci kez bakım için zamanlanır. Neden?**

**Y:** VM'niz zaten bakım-redeploy tamamladıktan sonra bakım için zamanlanmış göreceğiniz çeşitli kullanım örnekleri vardır:
   - Biz nedeni bakım dalgasının iptal edildi ve farklı bir yük ile yeniden. Hatalı yükü algıladık ve yalnızca bir ek yükü dağıtmak gerekiyor olabilir.
   - Makinenizin olduğu *taşınarak hizmet* başka bir düğüme bir donanım hatası nedeniyle.
   - Durdurmak için seçtiğiniz (serbest bırakın) ve VM'yi yeniden başlatın.
   - Sahip olduğunuz **otomatik kapatma** VM için etkinleştirilmiş.

## <a name="next-steps"></a>Sonraki adımlar

Bakım olayları kullanarak VM'yi içinde nasıl kaydolabilirsiniz öğrenin [zamanlanmış olaylar](../virtual-machines/windows/scheduled-events.md).
