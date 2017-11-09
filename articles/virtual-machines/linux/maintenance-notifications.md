---
title: "Azure Linux VM'ler için bakım bildirimleri işleme | Microsoft Docs"
description: "Azure'da çalışan Linux sanal makineleri için bakım bildirimleri görüntülemek ve Self Servis bakım başlatın."
services: virtual-machines-linux
documentationcenter: 
author: zivraf
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/26/2017
ms.author: zivr
ms.openlocfilehash: b31955e19883f9fe2e7ed6cf7f5076eaf52577c0
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2017
---
# <a name="handling-planned-maintenance-notifications-for-linux-virtual-machines"></a>Linux sanal makineleri için işleme planlı bakım bildirimleri

Azure güvenilirliği, performansı ve sanal makineler için konak altyapısının güvenliğini artırmak için güncelleştirmeleri düzenli olarak gerçekleştirir. Barındırma ortamı düzeltme eki uygulama veya yükseltme ve donanım yetkisini gibi değişiklikler güncelleştirmelerdir. Bu güncelleştirmeler çoğunluğu barındırılan sanal makineler için herhangi bir etkisi olmadan gerçekleştirilir. Ancak, güncelleştirmeler bir etkiye sahip olduğu durumlar vardır:

- Bakım yeniden başlatma gerektirmez, Azure VM konak güncelleştirilirken duraklatmak için yerinde geçiş kullanır.

- Bakım bir yeniden başlatma gerektirirse, ne zaman bunu planlı bakım, bir bildirim alın. Bu durumlarda, burada başlatabilirsiniz bakım kendiniz bir zaman penceresi verilir ne zaman çalıştığını sizin için.


Bir yeniden başlatma gerektiren planlı bakım içinde Dalgalar zamanlandı. Her wave farklı bir kapsam (bölge) sahiptir.

- Bir bildirim müşterilere bir wave başlar. Varsayılan olarak, abonelik sahibi ve ikincil sahipler bildirim gönderilir. Bildirimleri göndermek için daha fazla alıcı ve e-posta, SMS ve Web Kancalarını, gibi Mesajlaşma seçenekleri ekleyebilirsiniz. 
- Bildirim hemen sonra bir Self Servis penceresi ayarlanır. Bu pencere sırasında bu wave ve başlangıç bakım öngörülü dağıtın kullanarak sanal makinelerinizin dahil bulabilirsiniz. 
- Self Servis penceresinde, zamanlanmış bir bakım penceresi başlar. Şu anda Azure zamanlar ve gerekli bakım, sanal makine için geçerlidir.  

İki windows sahip amacı, bakım başlatmak ve ne zaman Azure bakım otomatik olarak başlatılacak bilerek sanal makinenizi yeniden başlatmanız için yeterli süre vermektir.

Bakım pencereleri Vm'leriniz için sorgu ve Self Servis bakım başlatmak için Azure CLI, PowerShell, REST API ve Azure Portalı'nı kullanabilirsiniz.

 > [!NOTE]
 > Bakım ve başarısız başlatmayı denerseniz, Azure VM olarak işaretler **atlandı** ve zamanlanmış bakım penceresi sırasında yeniden başlatma yok. Bunun yerine, daha sonra yeni bir zamanlama ile olarak kurulur. 

## <a name="find-vms-scheduled-for-maintenance-using-cli"></a>CLI kullanarak bakım için zamanlanmış sanal makineleri Bul

Planlı bakım bilgileri kullanarak görülme [azure vm get örneği Görünüm](/cli/azure/vm?view=azure-cli-latest#az_vm_get_instance_view).
 
Yalnızca planlı bakım ise bakım bilgileri döndürülür. Varsa bu etkileri VM herhangi bir bakım zamanlanmış, komut herhangi bir bakım bilgi döndürmez. 

```azure-cli
az vm get-instance-view  - g rgName  -n vmName 
```

Aşağıdaki değerleri MaintenanceRedeployStatus altında döndürülür: 

| Değer | Açıklama   |
|-------|---------------|
| IsCustomerInitiatedMaintenanceAllowed | Bakım VM üzerinde şu anda başlatabilirsiniz olup olmadığını gösterir ||
| PreMaintenanceWindowStartTime         | VM üzerinde bakım başlatabilir, bakım Self Servis penceresi başlangıcı ||
| PreMaintenanceWindowEndTime           | VM üzerinde bakım başlatabilir, bakım Self Servis penceresi sonu ||
| MaintenanceWindowStartTime            | VM üzerinde bakım başlatabilir, bakım zamanlanmış penceresi başlangıcı ||
| MaintenanceWindowEndTime              | VM üzerinde bakım başlatabilir, bakım zamanlanmış penceresi sonu ||
| LastOperationResultCode               | Son VM bakım başlatma girişimi sonucu ||




## <a name="start-maintenance-on-your-vm-using-cli"></a>CLI kullanarak, VM bakımını Başlat

Aşağıdaki arama, bir VM üzerinde bakım başlatır `IsCustomerInitiatedMaintenanceAllowed` ayarlanmış true.

```azure-cli
az vm perform-maintenance rgName vmName 
```

[!INCLUDE [virtual-machines-common-maintenance-notifications](../../../includes/virtual-machines-common-maintenance-notifications.md)]

## <a name="classic-deployments"></a>Klasik dağıtımlar

Olan eski VM'ler yaşamaya devam ediyorsanız Klasik dağıtım modeli kullanılarak dağıtılan CLI 1.0 sorguya VM'ler için ve kullanabilirsiniz bakım başlatın.

Yazarak Klasik VM ile çalışmak için doğru moda olduğundan emin olun:

```
azure config mode asm
```

Adlı bir VM bakım durumunu almak için *myVM*, türü:

```
azure vm show myVM 
``` 

Bakım, üzerinde başlatmak için Klasik VM adlı *myVM* içinde *myService* hizmet ve *myDeployment* dağıtım, türü:

```
azure compute virtual-machine initiate-maintenance --service-name myService --name myDeployment --virtual-machine-name myVM
```


## <a name="faq"></a>SSS


**S: neden my sanal makineleri şimdi yeniden başlatmak gerekiyor mu?**

**Y:** güncelleştirmeleri ve yükseltmeleri Azure platformu çoğunluğu değil etkisi sırasında sanal makinenin kullanılabilirlik burada biz olamaz kaçının Azure üzerinde barındırılan sanal makineleri yeniden başlatıldığı durumlar vardır. Biz sanal makine yeniden başlatma sonucunda sunucularımızda yeniden başlatmak bize gerektiren bazı değişiklikler toplanan.

**S: ı önerilerinizi yüksek kullanılabilirlik için bir kullanılabilirlik kümesi kullanarak izlerseniz, güvenli miyim?**

**Y:**kullanılabilirlik dağıtılan sanal makineleri ayarlama veya sanal makine ölçek kümeleri güncelleştirme etki alanları (UD) kavram vardır. Bakımı gerçekleştirirken Azure UD kısıtlaması geliştirir ve sanal makinelerden farklı UD (içinde aynı kullanılabilirlik kümesinde) yeniden değil.  Azure, sanal makinelerin sonraki grubuna geçmeden önce en az 30 dakika bekler. 

Yüksek kullanılabilirlik hakkında daha fazla bilgi için azure'da Windows sanal makinelerin kullanılabilirliğini yönetin başvurun veya azure'daki Linux sanal makinelerin kullanılabilirliğini yönetin.

**S: olağanüstü durum kurtarma başka bir bölgede kümesi sahibim. Güvenli miyim?**

**Y:** her Azure bölgesindeki başka bir bölge içinde aynı coğrafi konum (örneğin, ABD, Avrupa veya Asya) olarak eşlenmiş. Kapalı kalma süresini ve uygulama kesintisi riskini azaltmak amacıyla, planlı Azure güncelleştirmeleri, bölge çiftlerine tek tek uygulanır. Zamanlanmış bakım penceresi eşleştirilmiş bölgeler arasında farklılık gösterir ancak planlı bakım sırasında bakım başlatmak kullanıcılar için benzer bir pencere Azure zamanlama.  

Azure bölgeleri hakkında daha fazla bilgi için azure'da sanal makineler için kullanılabilirlik ve bölgeler bakın.  Burada Bölgesel çiftleri tam listesini görebilirsiniz.

**S: nasıl planlı bakım hakkında bildirim?**

**Y:** bir veya daha fazla Azure bölgeleri için bir zamanlama ayarlayarak planlı bakım wave başlatır. Hemen sonra abonelik sahipleri (bir e-posta abonelik başına) için bir e-posta bildirimi gönderilir. Ek Kanallar ve bu bildirim için alıcıları etkinlik günlüğü uyarıları kullanarak yapılandırılabilir. Burada planlı bakım zaten zamanlanmış bir bölge için bir sanal makine dağıtma durumda bildirim almaz ancak VM bakım durumunu denetlemek yerine gerekir.

**S: portalında, Powershell veya CLI sorunun ne olduğunu, planlı bakım herhangi bir gösterge görmüyorum?**

**Y:** planlı bakım için ilgili bilgiler kullanılabilir olduğu tarafından etkilenmiş olacak VM'ler için planlı bakım wave sırasında. Diğer bir deyişle, verileri değil görürseniz, bakım wave zaten tamamlandı (veya başlatılmamış olduğunu) veya sanal makinenize güncelleştirilmiş bir sunucu zaten barındırılan olabilir.

**S: sanal Makinem bakım başlamanız gerekir?**

**Y:** genel olarak, bir bulut hizmeti, kullanılabilirlik kümesi veya sanal makine ölçek kümesi, dağıtılmış iş yükleri için planlı bakım esnektir.  Planlı bakım sırasında herhangi bir anda yalnızca bir tek güncelleştirme etki alanı etkilenmez. Güncelleme etki alanına etkilenip sırasını mutlaka sıralı olarak gerçekleşmez olduğunu unutmayın.

Bakım kendiniz aşağıdaki durumlarda başlatmak isteyebilirsiniz:
- Uygulamanızın tek bir sanal makinede çalışır ve saatlerde tüm bakım uygulanması gerekiyor
- Bakım süresi, SLA'ın bir parçası olarak koordine olmanız
- Birden fazla 30 dakika arasında bir kullanılabilirlik içinde bile her VM yeniden ayarlamanız.
- Tüm uygulama (birden çok katmanları, birden çok güncelleştirme etki alanı) bakım daha hızlı tamamlamak için yapmak istiyorsunuz.

**S: sanal Makinem tam olarak etkilenecek zaman bilmenin bir yolu var mı?**

**Y:** zamanlama ayarlarken, birkaç gün zaman penceresi tanımlarız. Ancak, tam sıralama ve sunucuları (VM'ler) bu pencereyi içinde bilinmiyor. Vm'leri için tam zaman bilmek ister misiniz müşteriler zamanlanmış olaylar ve sanal makine içinde sorgu kullanın ve VM yeniden başlatma öncesindeki 10 dakika bildirimi.
  
**S: ne kadar süreyle sanal Makinem yeniden başlatılmasını sürer?**

**Y:** VM boyutuna bağlı olarak, yeniden başlatma için birkaç dakika sürebilir. Kullanılabilirlik kümesi, her grup, sanal makineleri (UD) arasında 30 dakika verilir veya Bulut hizmetlerini kullanma durumda ölçeklendirme Not belirler. 

**S: Bulut Hizmetleri, Ölçek kümeleri ve Service Fabric söz konusu olduğunda deneyimi ne olacak?**

**Y:** bu platformlar tarafından planlı bakım etkilenen olsa da, müşteriler bu platformu kullanarak güvenli bir tek yükseltme etki alanı (UD içinde) Bu yalnızca VM'ler verilen herhangi bir zamanda etkilenecek olarak kabul edilir.  

**S: donanım yetki alma hakkında bir e-posta aldığınız, planlı bakım ile aynıdır?**

**Y:** donanım yetkisini planlanan bakım olayı olsa da, henüz edildi Bu yeni deneyimi kullanım örneğine sahip olduğumuz.  Bunlar iki benzer iki farklı planlı bakım Dalgalar hakkında e-postaları durumda kafası almak için müşteriler bekliyoruz.

**S: herhangi bir bakım bilgi my Vm'lerinde görmüyorum. Nelerin yanlış gittiğini?**

**Y:** neden değil gördüğünüz herhangi bir bakım bilgi Vm'leriniz birkaç nedeni vardır:
1.  Microsoft iç olarak işaretlenmiş bir aboneliği kullanıyorsunuz.
2.  Vm'leriniz için bakım zamanlanmış değil. Bakım wave, böylece Vm'leriniz artık tarafından etkilenen değiştiren veya iptal sona erdiğini olabilir.
3.  VM liste görünümüne eklenen 'Bakım' sütunu yok. Bu sütun için varsayılan görünüm ekledik olsa da, varsayılan olmayan sütunları görmek için yapılandırılmış müşteriler el ile eklemelisiniz **Bakım** kendi VM liste görünümü için sütun.

**S: VM'im bakım için ikinci kez zamanlandı. Neden?**

**Y:** bakım-dağıtın zaten tamamladıktan sonra bakım için zamanlanmış VM göreceğiniz birkaç kullanım örnekleri vardır:
1.  Biz bakım wave iptal ve farklı bir yükü ile yeniden başlatılır. Hatalı yükü algıladık ve kimliğinizi yalnızca bir ek yükü dağıtma gerekiyor olabilir.
2.  VM edildi *gösteriyor hizmet* bir donanım hatası nedeniyle başka bir düğüme
3.  Durdurmak için seçtiğiniz (deallocate) ve VM yeniden başlatma
4.  Sahip olduğunuz **otomatik kapatma** VM için açık


## <a name="next-steps"></a>Sonraki Adımlar

VM kullanarak içinde bakım olaylarından ne kaydolabilirsiniz öğrenin [zamanlanmış olayları](scheduled-events.md).