---
title: Azure'da Windows VM'ler için bakım bildirimlerini işleme | Microsoft Docs
description: Azure'da çalışan Windows sanal makineleri için bakım bildirimleri görüntüleyin ve Self Servis Bakımı Başlat.
services: virtual-machines-windows
documentationcenter: ''
author: shants123
editor: ''
tags: azure-service-management,azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/02/2018
ms.author: shants
ms.openlocfilehash: db8fd40b9c573d04d9442c64fb058902a771eca0
ms.sourcegitcommit: 72cc94d92928c0354d9671172979759922865615
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58418882"
---
# <a name="handling-planned-maintenance-notifications-for-windows-virtual-machines"></a>Planlı bakım bildirimlerini Windows sanal makineler için işleme

Azure sanal makine konak altyapısının güvenilirlik, performans ve güvenliğini iyileştirmek için düzenli olarak güncelleştirmeler yapar. Barındırma ortamına düzeltme eki veya yükseltme ve donanım yetkisini alma gibi değişiklik güncelleştirmelerdir. Bu güncelleştirmelerin çoğu barındırılan sanal makinelere sunucuları etkilenmeden gerçekleştirilir. Ancak, güncelleştirmeleri bir etkiye sahip olduğu durumlar da vardır:

- Bakım yeniden başlatma gerektirmez, Azure VM konak güncelleştirilirken duraklatmak için yerinde geçiş kullanır. Hata etki alanı tarafından uygulanan hata etki alanı bu rebootful olmayan bakım işlemleridir ve hiçbir uyarı sistem durumu sinyali alınırsa ilerleme durduruldu. 

- Bakım yeniden başlatma gerektirirse, ne zaman bunu planlı bakım, bir bildirim alın. Bu durumlarda, burada başlatabilir bakım kendiniz bir zaman penceresi verilir ne zaman çalıştığını sizin için.


Dalgaları içinde bir yeniden başlatma gerektiren planlı bakım zamanlandı. Her dalgada farklı kapsamı (bölge) vardır.

- Bir dalga müşterilere bir bildirim ile başlar. Varsayılan olarak, abonelik sahibi ve ikincil sahipler bildirim gönderilir. Daha fazla alıcı ve e-posta, SMS ve Web kancaları gibi Mesajlaşma seçenekleri için Azure'u kullanarak bildirimleri ekleyebileceğiniz [etkinlik günlüğü uyarıları](../../azure-monitor/platform/activity-logs-overview.md).  
- Bildirimi, anında bir *Self Servis penceresi* kullanılabilir hale getirilir. Bu pencere sırasında bu dalgada dahil olduğu sanal makinelerinizin bulabilir ve Bakım zamanlama kendi gereksinimlerine göre proaktif olarak başlat.
- Sonra Self Servis penceresinde bir *zamanlanan bakım penceresinden* başlar. Belirli bir noktada bu penceresi sırasında Azure zamanlar ve gerekli bakım sanal makineniz için geçerlidir. 

Bakımı Başlat ve ne zaman Azure bakım otomatik olarak başlatılacak bilerek sanal makinenizi yeniden başlatmanız yeterli süre vermek amacıyla iki pencereleri aşağıdakiler amacı olan.


Vm'leriniz için bakım pencereleri sorgulamak ve Self Servis bakım başlatmak için Azure portalı, PowerShell, REST API ve CLI'yı kullanabilirsiniz.

  
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
- 15 dakika önce bakım kapanma başlangıcı proaktif yük devretme veya iş yükünüz, normal şekilde kapatılmasını etkinleştirmek zamanlanmış olaylar benimseyen varsa

**Kullanım** VM'nizi zamanlanan bakım aşaması sırasında kesintisiz çalıştırılacak planlıyor ve yukarıda belirtilen karşı göstergelerden hiçbiri geçerli olan Self Servis Bakım. 

Self Servis bakım aşağıdaki durumlarda kullanmak en iyisidir:

- Bir tam bakım penceresi yönetim ya da son müşteri iletişim kurması gerekir. 
- Belirli bir tarihte bakım tamamlanması gerekir. 
- Bakım, örneğin, çok katmanlı uygulaması güvenli kurtarma garanti sırasını denetlemek gerekir.
- 30 dakikadan fazla VM kurtarma zamanı arasında iki güncelleştirme etki alanları (UD) ihtiyacınız vardır. Güncelleştirme etki alanları arasındaki zamanı denetlemek için bakım teker teker Vm'leri bir güncelleştirme etki alanınızda (UD) tetiklemesi gerekir.

 

[!INCLUDE [virtual-machines-common-maintenance-notifications](../../../includes/virtual-machines-common-maintenance-notifications.md)]

## <a name="check-maintenance-status-using-powershell"></a>PowerShell kullanarak bakım durumunu denetleme

Azure Powershell, bakım için zamanlanmış Vm'leri zaman görmek için de kullanabilirsiniz. Planlı bakım bilgileri kullanılabilir [Get-AzVM](https://docs.microsoft.com/powershell/module/az.compute/get-azvm) cmdlet'ini kullandığınızda `-status` parametresi.
 
Planlı bakım yoksa bakım bilgileri döndürülür. Cmdlet'i, VM etkileyen herhangi bir bakım zamanlanırsa, herhangi bir bakım bilgi döndürmez. 

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

```powershell
Get-AzVM -ResourceGroupName rgName -Name vmName -Status
```

Aşağıdaki özellikler altında MaintenanceRedeployStatus döndürülür: 

| Değer | Açıklama   |
|-------|---------------|
| IsCustomerInitiatedMaintenanceAllowed | Bakım VM üzerinde şu anda başlatabilirsiniz olup olmadığını gösterir |
| PreMaintenanceWindowStartTime         | Sanal makinenizde bakım başlatabilir, bakım penceresinin Self Servis başına |
| PreMaintenanceWindowEndTime           | Son bakım sanal makinenizde başlatabilir, bakım penceresinin Self Servis |
| MaintenanceWindowStartTime            | Zamanlanmış bakım başlangıcını Azure vm'nizdeki bakım işlemini başlatır |
| MaintenanceWindowEndTime              | Azure vm'nizdeki bakım işlemini başlatır zamanlanmış bakım penceresinin sonu |
| LastOperationResultCode               | VM'de bakım başlatmak için son girişimi sonucu |



Da bakım durumu için bir kaynak grubundaki tüm Vm'leri kullanarak alabilirsiniz [Get-AzVM](https://docs.microsoft.com/powershell/module/az.compute/get-azvm) ve bir VM belirterek değil.
 
```powershell
Get-AzVM -ResourceGroupName rgName -Status
```

Aşağıdaki PowerShell işlevi, abonelik Kimliğiniz alır ve bakım için zamanlanmış Vm'leri listesini yazdırır.

```powershell

function MaintenanceIterator
{
    Select-AzSubscription -SubscriptionId $args[0]

    $rgList= Get-AzResourceGroup 

    for ($rgIdx=0; $rgIdx -lt $rgList.Length ; $rgIdx++)
    {
        $rg = $rgList[$rgIdx]        
    $vmList = Get-AzVM -ResourceGroupName $rg.ResourceGroupName 
        for ($vmIdx=0; $vmIdx -lt $vmList.Length ; $vmIdx++)
        {
            $vm = $vmList[$vmIdx]
            $vmDetails = Get-AzVM -ResourceGroupName $rg.ResourceGroupName -Name $vm.Name -Status
              if ($vmDetails.MaintenanceRedeployStatus )
            {
                Write-Output "VM: $($vmDetails.Name)  IsCustomerInitiatedMaintenanceAllowed: $($vmDetails.MaintenanceRedeployStatus.IsCustomerInitiatedMaintenanceAllowed) $($vmDetails.MaintenanceRedeployStatus.LastOperationMessage)"               
            }
          }
    }
}

```

### <a name="start-maintenance-on-your-vm-using-powershell"></a>PowerShell kullanarak sanal makinenizde Bakımı Başlat

İşlev bilgileri önceki bölümde kullanarak, aşağıdaki bakım bir VM'de başlıyorsa **IsCustomerInitiatedMaintenanceAllowed** ayarlanır true.

```powershell
Restart-AzVM -PerformMaintenance -name $vm.Name -ResourceGroupName $rg.ResourceGroupName 
```

## <a name="classic-deployments"></a>Klasik dağıtımlar

Olan eski Vm'leri hala varsa Klasik dağıtım modeli kullanılarak dağıtılan PowerShell sorguya VM'ler için ve kullanabilirsiniz Bakımı Başlat.

Sanal Makinenin bakım durumunu almak için şunu yazın:

```
Get-AzureVM -ServiceName <Service name> -Name <VM name>
```

Klasik sanal makinenizde bakım başlatmak için şunu yazın:

```
Restart-AzureVM -InitiateMaintenance -ServiceName <service name> -Name <VM name>
```


## <a name="faq"></a>SSS


**S: Sanal makinelerime şimdi yeniden önyüklemek neden ihtiyacınız var?**

**C:** Sanal makinenin kullanılabilirlik çoğu güncelleştirmeler ve yükseltmeler Azure platformuna yönelik durum olsa da burada biz Azure'da barındırılan sanal makineleri yeniden yoksayılamaz durumlar vardır. Şu sanal makinelerin yeniden başlatılmasına yol açacak sunucularımızı yeniden bırakmamızı değişiklikler birikti.

**S: Önerilerinizi yüksek kullanılabilirlik için bir kullanılabilirlik kümesi'ni kullanarak takip ettiğim, güvenli muyum?**

**C:** Bir kullanılabilirlik kümesinde veya sanal makine ölçek kümelerinde dağıtılan sanal makineler için Güncelleştirme Etki Alanları (UD) kavramı vardır. Bakımın gerçekleştirildiği sırada Azure UD kısıtlamasına uyar ve (aynı kullanılabilirlik kümesindeki) farklı ud'den sanal makineleri yeniden başlatmaz.  Azure ayrıca sanal makinelerin sonraki grubuna taşımadan önce en az 30 dakika bekler. 

Yüksek kullanılabilirlik hakkında daha fazla bilgi için bkz: [azure'da sanal makineler için kullanılabilirlik ve bölgeler](regions-and-availability.MD).

**S: Nasıl planlı bakımla ilgili bildirim?**

**C:** Bir planlı bakım dalgası bir veya daha fazla Azure bölgesine bir zamanlama ayarlayarak başlar. Kısa süre sonra abonelik sahipleri (bir e-posta abonelik başına) için bir e-posta bildirimi gönderilir. Etkinlik günlüğü uyarıları kullanarak ek Kanallar ve bu bildirim için alıcı yapılandırılabilir. Burada planlı bakım zaten zamanlandı bir bölge için bir sanal makine dağıtma durumda bildirim almayacaksınız ancak yerine VM'nin bakım durumunu denetleme gerekir.

**S: Portal, Powershell veya CLI planlı bakıma dair herhangi bir göstergesi göremiyorum. Ne oldu?**

**C:** Planlı bakımla ilgili bilgileri, bir yalnızca etkilenecek olacak sanal makineler için planlı bakım dalgası sırasında kullanılabilir. Diğer bir deyişle, verileri değil görürseniz, nedeni bakım dalgasının zaten tamamlanmış (veya başlatılmadı,) veya sanal makinenizin zaten güncelleştirilmiş bir sunucuda barındırılıyor olabilir.

**S: Ne zaman tam olarak sanal Makinem etkileneceğini bilmenin bir yolu var mı?**

**C:** Zamanlamayı ayarladığınızda, birkaç gün zaman penceresi tanımlarız. Bununla birlikte, sunucuların (ve VM'lerin) Bu pencere içindeki tam sıralaması bilinmiyor. Kendi Vm'leri için kesin zaman bilmek isteyen müşteriler [zamanlanmış olaylar](scheduled-events.md) sanal makinede bulunan sorgu gelen ve VM yeniden başlatmadan önce 15 dakikalık bildirim alırsınız.

**S: Sanal Makinem yeniden başlatılması ne kadar sürer?**

**C:**  Sanal makinenizin boyutuna bağlı olarak, yeniden başlatma için Self Servis bakım penceresi sırasında birkaç dakika sürebilir. Yeniden başlatma sırasında Azure tarafından başlatılan bir zamanlanan bakım penceresinde başlatma işlemi genellikle yaklaşık 25 dakika sürer. Cloud Services (Web/çalışan rolü), sanal makine ölçek kümeleri veya kullanılabilirlik kümeleri kullanmanız durumunda, 30 dakika arasında her grup, Vm'leri (UD) zamanlanmış bakım penceresi sırasında verileceğini unutmayın. 

**S: İçin deneyim nasıl sanal makine ölçek kümeleri nedir?**

**C:** Planlı bakım için sanal makine ölçek kümeleri kullanıma sunuldu. Self Servis bakım başlatmak yönergeler başvurmak için [VMSS için planlı Bakım](../../virtual-machine-scale-sets/virtual-machine-scale-sets-maintenance-notifications.md) belge.

**S: Deneyimle karşılaşacak Cloud Services (Web/çalışan rolü) ve Service Fabric nedir?**

**C:** Bu platformlar planlı bakımdan etkilenir, ancak herhangi bir zamanda yalnızca tek bir Güncelleştirme Etki Alanındaki (UD) VM’ler etkileneceğinden bu platformları kullanan müşterilerin güvende olduğu düşünülür. Self Servis bakım şu anda Cloud Services (Web/çalışan rolü) ve Service Fabric için kullanılabilir değildir.

**S: Vm'lerimi üzerinde herhangi bir bakım bilgi göremiyorum. Sorun nedir?**

**C:** Neden, herhangi bir bakım bilgi Vm'lerinizde görmediğiniz birkaç nedeni vardır:
1.  Microsoft iç olarak işaretlenmiş bir aboneliği kullanıyorsunuz.
2.  Sanal makinelerinizin bakım için zamanlanmış değil. Bunun nedeni bakım dalgasının, sanal makineleriniz tarafından artık etkilendiğini şekilde değiştirilebilir veya iptal sona erdiğini olabilir.
3.  Sahip olmadığınız **Bakım** sütun için VM listesi görünümünüze eklendi. Biz varsayılan görünüme bu sütun eklenmiş olsa da varsayılan olmayan sütunları görmek için yapılandırılmış müşteriler el ile eklemelisiniz **Bakım** kendi VM liste görünümünde sütun.

**S: Sanal Makinem için ikinci kez bakım için zamanlanır. Neden?**

**C:** Bakım amaçlı yeniden dağıtım işleminizi tamamladıktan sonra sanal makinenizin bakım için zamanlandığını göreceğiniz çeşitli kullanım örnekleri vardır:
1.  Biz nedeni bakım dalgasının iptal edildi ve farklı bir yük ile yeniden. Hatalı yükü algıladık ve yalnızca bir ek yükü dağıtmak gerekiyor olabilir.
2.  Makinenizin olduğu *taşınarak hizmet* başka bir düğüme bir donanım hatası nedeniyle
3.  Durdurmak için seçtiğiniz (serbest bırakın) ve VM'yi yeniden başlatın
4.  Sahip olduğunuz **otomatik kapatma** VM için açık


## <a name="next-steps"></a>Sonraki adımlar

Bakım olayları kullanarak VM'yi içinde nasıl kaydolabilirsiniz öğrenin [zamanlanmış olaylar](scheduled-events.md).
