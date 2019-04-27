---
title: Azure'da sanal makine ölçek için bakım bildirimleri ayarlar | Microsoft Docs
description: Bakım bildirimleri görüntüleyin ve Azure'da sanal makine ölçek kümeleri için Self Servis bakım başlatın.
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
ms.openlocfilehash: 31d4829c6adaf4bd5392ef393dcaefbeb7dc6255
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60618464"
---
# <a name="planned-maintenance-notifications-for-virtual-machine-scale-sets"></a>Sanal makine ölçek kümeleri için planlı bakım bildirimlerini


Azure güncelleştirmeleri güvenilirlik, performans ve sanal makineler (VM) konak altyapısının güvenliğini iyileştirmek için düzenli olarak gerçekleştirir. Güncelleştirmeleri barındırma ortamına düzeltme eki veya yükseltme ve donanım yetkisini alma içerebilir. Çoğu güncelleştirme, barındırılan sanal makineleri etkilemez. Ancak, güncelleştirmeleri bu senaryolarda Vm'leri etkiler:

- Bakım yeniden başlatma gerektirmez, Azure VM konak güncelleştirilirken duraklatmak için yerinde geçiş kullanır. Yeniden başlatma gerektirmez bakım işlemleri hata etki alanı tarafından uygulanan hata etki alanı var. Tüm uyarı sistem durumu sinyali alınırsa ilerleme durduruldu.

- Bakım yeniden başlatma gerektirirse, ne zaman bunu planlı bakım gösteren bir bildirim alırsınız. Bu gibi durumlarda, bir zaman penceresi sizin için en uygun, bakım kendiniz başlatabilirsiniz sunulur.


Yeniden başlatma gerektiren bir planlı bakım içinde Dalgalar zamanlanır. Her dalgada farklı kapsamı (bölge) vardır:

- Bir dalga müşterilere bir bildirim ile başlar. Varsayılan olarak, ikincil sahipler ve abonelik sahibi için bildirim gönderilir. Azure'ı kullanarak bildirimlere alıcıları ve e-posta, SMS ve Web kancaları gibi Mesajlaşma seçenekleri ekleyebilirsiniz [etkinlik günlüğü uyarıları](../azure-monitor/platform/activity-logs-overview.md).  
- Bildirimi, bir *Self Servis penceresi* kullanılabilir hale getirilir. Bu pencere sırasında sanal makinelerinizin dalgada içerdiği bulabilirsiniz. Bakım zamanlama kendi gereksinimlerine göre proaktif olarak başlatabilirsiniz.
- Sonra Self Servis penceresinde bir *zamanlanan bakım penceresinden* başlar. Belirli bir noktada bu penceresi sırasında Azure zamanlar ve gerekli bakım, VM için geçerlidir. 

Bakımı Başlat ve ne zaman Azure bakım otomatik olarak başlatılacak bilerek, sanal Makinenizin yeniden başlatma için yeterli süre vermek amacıyla iki pencereleri aşağıdakiler amacı var.

Azure portalı, PowerShell, REST API ve Azure CLI için bakım pencerelerini, sanal makine ölçek kümesi Vm'lerine sorgulamaya ve Self Servis bakım başlatmak için kullanabilirsiniz.

## <a name="should-you-start-maintenance-during-the-self-service-window"></a>Self Servis penceresi sırasında bakım başlamanız gerekir?  

Aşağıdaki yönergeler bakım seçtiğiniz bir zamanda başlatmak karar vermenize yardımcı olabilir.

> [!NOTE] 
> Self Servis bakım tüm Vm'leriniz için kullanılabilir olmayabilir. Önleyici yeniden dağıtma VM'niz için kullanılabilir olup olmadığını belirlemek için nelere **Şimdi Başlat** bakım durumu. Şu anda Azure Cloud Services (Web/çalışan rolü) ve Azure Service Fabric için Self Servis bakım kullanılamıyor.


Self Servis bakım kullanan dağıtımlar için önerilen değil *kullanılabilirlik kümeleri*. Kullanılabilirlik, yüksek oranda kullanılabilir ayarlar yalnızca bir hangi güncelleştirme etki alanı herhangi bir zamanda etkilenir kümeleridir. Kullanılabilirlik kümeleri için:

- Bakım tetikleyicisi Azure'a bırakın. Yeniden başlatma gerektiren bir bakım Bakımı güncelleme etki alanı güncelleştirme etki alanına göre yapılır. Güncelleştirme etki alanlarını mutlaka sırayla bakım almaz. Bir güncelleştirme etki alanları arasında 30 dakikalık duraklama yoktur.
- Kapasite (1/güncelleştirme etki alanı sayısı) bazıları geçici kaybı önemliyse, bakım süresi boyunca ek örnekleri ayırarak kaybı kolayca dengeleyebilirsiniz.
- Yeniden başlatma gerektirmez bakım güncelleştirmeleri hata etki alanı düzeyinde uygulanır. 
    
**Yoksa** Self Servis bakım aşağıdaki senaryolarda kullanın: 

- Vm'lerinizi sıkça el ile DevTest Labs'i kullanarak, otomatik kapatma kullanarak veya bir zamanlamaya uygun olarak kapatırsanız. Bu senaryolarda, Self Servis bakım, bakım durumu geri dönmesi ve ek kapalı kalma durumlarına neden.
- Kısa süreli Vm'lerde bildiğinizden nedeni bakım dalgasının bitiminden önce silinecek. 
- Güncelleştirmeden sonra korumak istediğiniz yerel (kısa ömürlü) disk depolanan büyük durumuna sahip iş yükleri için. 
- Genellikle, sanal Makinenizin boyutlandırırsanız. Bu senaryo, bakım durumu geri. 
- 15 dakika önce proaktif bir yük devretme veya iş yükü normal şekilde kapatılmasını etkinleştirmek zamanlanmış olaylar benimseyen bakım kapatma başlar.

**Yapmak** VM'nizi zamanlanan bakım aşaması sırasında kesintisiz çalıştırmayı planladığınız ve önceki counterindications hiçbiri geçerli Self Servis Bakımı kullanın. 

Self Servis bakım aşağıdaki durumlarda kullanmak en iyisidir:

- Yönetim veya müşteriniz için bir tam bakım penceresi iletişim kurması gerekir. 
- Belirli bir tarihte bakım tamamlanması gerekir. 
- Çok katmanlı uygulama güvenli kurtarma sağlamak için bir bakım, örneğin, bir dizi denetim gerekir.
- 30 dakikadan fazla VM kurtarma süresi iki güncelleştirme etki alanları arasında ihtiyacınız vardır. Güncelleştirme etki alanları arasındaki zamanı denetlemek için bakım teker teker Vm'leri bir güncelleştirme etki alanınızda tetiklemesi gerekir.

 
## <a name="view-virtual-machine-scale-sets-that-are-affected-by-maintenance-in-the-portal"></a>Portalda bakım tarafından etkilenen sanal makine ölçek kümeleri

Bir planlı bakım dalgası zamanlandıysa, Azure portalını kullanarak gelecek bakım dalgasından etkilenen sanal makine ölçek kümeleri listesini görüntüleyebilirsiniz. 

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol menüde **tüm hizmetleri**ve ardından **sanal makine ölçek kümeleri**.
3. Altında **sanal makine ölçek kümeleri**seçin **sütunları Düzenle** kullanılabilir sütunlar listesini açın.
4. İçinde **kullanılabilir sütunlar** bölümünden **Self Servis Bakım**, ve ardından taşımak için **Seçili sütunlar** listesi. **Uygula**’yı seçin.  

    Yapmak **Self Servis Bakım** bulmak için aşağı açılan seçeneğinde değiştirebilirsiniz daha kolay öğesi **kullanılabilir sütunlar** bölümü **tüm** için  **Özellikleri**.

**Self Servis Bakım** sütunu şimdi sanal makine ölçek kümeleri listesinde görünür. Her sanal makine ölçek kümesi, Self Servis bakım sütun için aşağıdaki değerlerden biri olabilir:

| Değer | Açıklama |
|-------|-------------|
| Evet | Sanal makine ölçek kümenizdeki en az bir VM içinde Self Servis bir penceredir. Bu Self Servis penceresi sırasında herhangi bir zamanda bakım başlayabilirsiniz. | 
| Hayır | VM, bir Self Servis penceresinde etkilenen sanal makine ölçek ayarlanır. | 
| - | Sanal makine ölçek kümeleri, planlı bakım dalgası parçası değildir.| 

## <a name="notification-and-alerts-in-the-portal"></a>Bildirim ve Portalı'nda uyarılar

Azure aboneliğine sahip ve ikincil sahipler gruba e-posta göndererek planlı bakım için zamanlama iletişim kurar. Etkinlik günlüğü uyarıları oluşturarak bu iletişimi alıcıları ve kanallar ekleyebilirsiniz. Daha fazla bilgi için [Azure etkinlik günlüğü ile abonelik etkinliğini İzle](../azure-monitor/platform/activity-logs-overview.md).

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol menüde **İzleyici**. 
3. İçinde **İzleyici - uyarılar (Klasik)** bölmesinde **+ etkinlik günlüğü uyarısı Ekle**.
4. Üzerinde **etkinlik günlüğü uyarısı Ekle** sayfasında, seçin veya istenen bilgileri girin. İçinde **ölçütleri**, aşağıdaki değerleri ayarlayın sağlayın:
   - **Olay kategorisi**: Seçin **hizmet durumunu**.
   - **Hizmetleri**: Seçin **sanal makine ölçek kümeleri ve sanal makineler**.
   - **Tür**: Seçin **planlı Bakım**. 
    
Etkinlik günlüğü uyarılarının nasıl yapılandırılacağı hakkında daha fazla bilgi için bkz: [oluşturma etkinlik günlüğü uyarıları](../azure-monitor/platform/activity-log-alerts.md)
    
    
## <a name="start-maintenance-on-your-virtual-machine-scale-set-from-the-portal"></a>Portaldan sanal makine ölçek kümesinde Bakımı Başlat

Sanal makine ölçek kümeleri genel bakış, bakım ile ilgili daha fazla ayrıntı görebilirsiniz. Sanal makine ölçek kümesindeki en az bir VM içinde planlı bakım dalgası yer alıyorsa, yeni bir bildirim Şerit sayfanın üst kısımda eklenir. Gitmek için bildirim Şerit seçin **Bakım** sayfası. 

Üzerinde **Bakım** sayfasında, hangi sanal makine örneği planlı bakımdan etkilenir görebilirsiniz. Bakım başlatmak için etkilenen sanal Makineye karşılık gelen onay kutusunu seçin. Ardından, **Başlat Bakım**.

Bakım başlattıktan sonra etkilenen sanal makine ölçek kümenizdeki VM'lerin bakım yapmak ve geçici olarak kullanılamıyor. Self Servis penceresi Kaçırıldı, sanal makine ölçek kümeniz, Azure tarafından korunur, zaman penceresi yine de görebilirsiniz.
 
## <a name="check-maintenance-status-by-using-powershell"></a>PowerShell kullanarak bakım durumunu denetleme

Sanal makine ölçek kümelerinizdeki Vm'lere bakım için zamanlanmış zaman görmek için Azure PowerShell kullanabilirsiniz. Planlı bakım bilgileri kullanılabilir kullanarak [Get-AzVmss](https://docs.microsoft.com/powershell/module/az.compute/get-azvmss) cmdlet'ini kullandığınızda `-InstanceView` parametresi.
 
Yalnızca bunu planlı bakım bakım bilgileri döndürülür. Sanal makine örneği etkileyen herhangi bir bakım zamanlanırsa, cmdlet herhangi bir bakım bilgi döndürmüyor. 

```powershell
Get-AzVmss -ResourceGroupName rgName -VMScaleSetName vmssName -InstanceId id -InstanceView
```

Aşağıdaki özellikler altında döndürülür **MaintenanceRedeployStatus**: 

| Değer | Açıklama |

|-------|---------------| | IsCustomerInitiatedMaintenanceAllowed | Bakım VM üzerinde şu anda başlatabilirsiniz olup olmadığını gösterir. | | PreMaintenanceWindowStartTime | Sanal makinenizde bakım başlatabilir, bakım penceresinin Self Servis başlangıcı. | | PreMaintenanceWindowEndTime | Sanal makinenizde bakım başlatabilir, bakım penceresinin Self Servis bitiş olayı. | | MaintenanceWindowStartTime | Başlangıç zamanlanmış bakım Azure vm'nizdeki bakım işlemini başlatır. | | MaintenanceWindowEndTime | Azure vm'nizdeki bakım işlemini başlatır zamanlanmış bakım penceresinin sonu. | | LastOperationResultCode | VM'de bakım başlatmak için son girişimi sonucu. |



### <a name="start-maintenance-on-your-vm-instance-by-using-powershell"></a>PowerShell kullanarak sanal makine Örneğinize Bakımı Başlat

Bakım, bir VM'de başlatabilirsiniz **IsCustomerInitiatedMaintenanceAllowed** ayarlanır **true**. Kullanım [kümesi AzVmss](/powershell/module/az.compute/set-azvmss) cmdlet'iyle `-PerformMaintenance` parametresi.

```powershell
Set-AzVmss -ResourceGroupName rgName -VMScaleSetName vmssName -InstanceId id -PerformMaintenance 
```

## <a name="check-maintenance-status-by-using-the-cli"></a>CLI kullanarak bakım durumunu denetleme

Planlı bakım bilgileri kullanarak görüntüleyebileceğiniz [az vmss seznamu](/cli/azure/vmss?view=azure-cli-latest#az-vmss-list-instances).
 
Yalnızca bunu planlı bakım bakım bilgileri döndürülür. Sanal makine örneği etkileyen herhangi bir bakım planlandıysa komutu herhangi bir bakım bilgi döndürmüyor. 

```azure-cli
az vmss list-instances -g rgName -n vmssName --expand instanceView
```

Aşağıdaki özellikler altında döndürülür **MaintenanceRedeployStatus** her bir VM örneği için: 

| Değer | Açıklama |

|-------|---------------| | IsCustomerInitiatedMaintenanceAllowed | Bakım VM üzerinde şu anda başlatabilirsiniz olup olmadığını gösterir. | | PreMaintenanceWindowStartTime | Sanal makinenizde bakım başlatabilir, bakım penceresinin Self Servis başlangıcı. | | PreMaintenanceWindowEndTime | Sanal makinenizde bakım başlatabilir, bakım penceresinin Self Servis bitiş olayı. | | MaintenanceWindowStartTime | Başlangıç zamanlanmış bakım Azure vm'nizdeki bakım işlemini başlatır. | | MaintenanceWindowEndTime | Azure vm'nizdeki bakım işlemini başlatır zamanlanmış bakım penceresinin sonu. | | LastOperationResultCode | VM'de bakım başlatmak için son girişimi sonucu. |


### <a name="start-maintenance-on-your-vm-instance-by-using-the-cli"></a>CLI kullanarak sanal makine Örneğinize Bakımı Başlat

Aşağıdaki çağrısı, bir sanal makine örneğinde bakım başlatır `IsCustomerInitiatedMaintenanceAllowed` ayarlanır **true**:

```azure-cli
az vmss perform-maintenance -g rgName -n vmssName --instance-ids id
```

## <a name="faq"></a>SSS

**S: Neden Vm'lerimin şimdi yeniden başlatmak gerekiyor?**

**C:** Çoğu güncelleştirmeler ve yükseltmeler Azure platformuna yönelik bazı durumlarda, sanal makine kullanılabilirliği etkilemez ancak biz Azure'da barındırılan sanal makineleri yeniden başlatılmasını önlemek olamaz. Biz, VM yeniden başlatılmasına yol açacak sunucularımızı yeniden bırakmamızı değişiklikler birikti.

**S: Takip ettiğim, önerilerinizi kullanılabilirlik kullanarak yüksek kullanılabilirlik için ayarlayın, güvenli karşılıyor muyum?**

**C:** Bir kullanılabilirlik dağıtılan sanal makinelerin ayarlamak veya sanal makine ölçek kümelerini güncelleştirme etki alanları kullanın. Bakımın gerçekleştirildiği sırada Azure güncelleştirme etki alanı kısıtlamasına uyar ve farklı güncelleştirme etki alanı (içinde aynı kullanılabilirlik kümesine) Vm'lerden yeniden değil. Ayrıca Azure Vm'leri için sonraki grubuna taşımadan önce en az 30 dakika bekler. 

Yüksek kullanılabilirlik hakkında daha fazla bilgi için bkz: [azure'da sanal makineler için kullanılabilirlik ve bölgeler](../virtual-machines/windows/regions-and-availability.md).

**S: Planlı bakım hakkında nasıl uyarılabilirim?**

**C:** Bir planlı bakım dalgası bir veya daha fazla Azure bölgesine bir zamanlama ayarlayarak başlar. Kısa süre sonra abonelik sahipleri (bir e-posta abonelik başına) için bir e-posta bildirimi gönderilir. Etkinlik günlüğü uyarıları kullanarak Kanallar ve bu bildirim için alıcı ekleyebilirsiniz. Planlı bakım zaten zamanlanmış bir bölge için bir VM dağıtırsanız, bildirim almaz. Bunun yerine, VM'nin bakım durumunu denetleyin.

**S: Portal, PowerShell veya CLI planlı bakıma dair herhangi bir göstergesi göremiyorum. Ne oldu?**

**C:** İlgili bilgiler için planlı bakım bir planlı bakımdan etkilenen sanal makineleri için planlı bakım dalgası sırasında kullanılabilir. Veri görmüyorsanız, nedeni bakım dalgasının zaten olabilir, tamamlanmış (veya başlatılmamış) ya da sanal makinenizin zaten güncelleştirilmiş bir sunucuda barındırılabilir.

**S: Tam olarak ne zaman VM'me etkilenecek bilmenin bir yolu var mı?**

**C:** Biz zamanlamayı ayarladığınızda, birkaç gün zaman penceresi tanımlarız. Sunucuların (ve VM’lerin) bu pencere içindeki sırası tam olarak bilinmez. Sanal makinelerinizin güncelleştirilmesi kesin zaman bilmek istiyorsanız, kullanabileceğiniz [zamanlanmış olaylar](../virtual-machines/windows/scheduled-events.md). Zamanlanmış olaylar kullandığınızda, sorgu içinde VM ve VM yeniden başlatmadan önce 15 dakikalık bildirim alırsınız.

**S: Sanal Makinem yeniden başlatılması ne kadar sürer?**

**C:**  Sanal makinenizin boyutuna bağlı olarak, yeniden başlatma için Self Servis bakım penceresi sırasında birkaç dakika sürebilir. Azure tarafından başlatılan yeniden başlatma sırasında zamanlanan bakım penceresinde, başlatma işlemi genellikle yaklaşık 25 dakika sürer. Cloud Services (Web/çalışan rolü), sanal makine ölçek kümeleri, kullandığınız ya da kullanılabilirlik kümeleri, zamanlanan bakım penceresi sırasında (güncelleştirme etki alanı) sanal makinelerinin her grubu arasında 30 dakika verilir. 

**S: Vm'lerimi üzerinde herhangi bir bakım bilgi göremiyorum. Sorun nedir?**

**C:** Neden tüm bakım bilgileri Vm'lerinizde göremeyebilirsiniz birkaç nedeni vardır:
   - Olarak işaretlenmiş bir abonelik kullanıyorsanız *Microsoft Internal*.
   - Sanal makineleriniz için bakım zamanlanmaz. Nedeni bakım dalgasının sona erdi, iptal edildi veya Vm'lerinizi artık bundan etkilenen böylece değiştirilmiş olabilir.
   - Sahip olmadığınız **Bakım** sütun için VM listesi görünümünüze eklendi. Görünümü el ile eklemeniz gerekir, varsayılan olmayan sütunları görmek için yapılandırırsanız bu sütun varsayılan görünüme ekledik ancak **Bakım** VM listesi görünümünüze sütunu.

**S: Sanal Makinem için ikinci kez bakım için zamanlanır. Neden?**

**C:** Sonra zaten, bakım tamamlandı ve yeniden çeşitli kullanım örnekleri, sanal Makinenizin bakım için zamanlandı:
   - Biz nedeni bakım dalgasının iptal edildi ve farklı bir yük ile yeniden. Hatalı bir yükü algıladık ve yalnızca bir ek yükü dağıtmak gerekiyor olabilir.
   - Makinenizin olduğu *taşınarak hizmet* başka bir düğüme bir donanım hatası nedeniyle.
   - Durdurmak için seçtiğiniz (serbest bırakın) ve VM'yi yeniden başlatın.
   - Sahip olduğunuz **otomatik kapatma** VM için etkinleştirilmiş.

## <a name="next-steps"></a>Sonraki adımlar

Kullanarak VM içinden bakım olayları kaydetme hakkında bilgi [zamanlanmış olaylar](../virtual-machines/windows/scheduled-events.md).
