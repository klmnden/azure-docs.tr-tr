---
title: Azure Güvenlik Merkezi'nde tam zamanında sanal makine erişimini | Microsoft Docs
description: Bu belge Azure Güvenlik Merkezi yardımcı nasıl tam zamanında VM erişimini denetim gösterir, Azure sanal makinelerine erişim.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: ''
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/28/2019
ms.author: monhaber
ms.openlocfilehash: 66a7171aff7b9bab5f320df1d71e9cab4ce0477d
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58758310"
---
# <a name="manage-virtual-machine-access-using-just-in-time"></a>Tam zamanında kullanarak sanal makine erişimini yönetme

Just-ın-time (JIT) sanal makine (VM) erişimi, Vm'lere gerektiğinde bağlanılabilmesi için kolay erişim sağlamanın yanı sıra saldırılara maruz kalma riskinizi azaltır, Azure Vm'lere gelen trafiği kilitlemek için kullanılabilir.

> [!NOTE]
> Just-ın-time özelliği Güvenlik Merkezi'nin standart katmanında kullanılabilir.  Güvenlik Merkezi’nin fiyatlandırma katmanları hakkında daha fazla bilgi almak için bkz. [Fiyatlandırma](security-center-pricing.md).
>
>

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="attack-scenario"></a>Saldırı senaryosu

Deneme yanılma hedef yönetim noktaları bir sanal makineye erişmek için bir yol olarak genellikle saldırıları. Başarılı olursa, bir saldırganın VM yükü denetimini ve ortamınıza bir köprübaşı oluşturmak.

Bir deneme yanılma saldırısı maruz kalma riskinizi azaltmak için bir bağlantı noktası açık olduğu süre miktarını sınırlamak için yoludur. Yönetim bağlantı noktalarının her zaman açık olması gerekmez. Bunların yalnızca VM’ye bağlı olduğunuzda (örneğin, yönetim veya bakım görevleri gerçekleştirmek için) açık olması gerekir. Tam zamanında etkinleştirildiğinde Güvenlik Merkezi kullanan [ağ güvenlik grubu](../virtual-network/security-overview.md#security-rules) saldırganlar tarafından hedeflenemez yönetim bağlantı noktalarına erişimi kısıtlama (NSG) kuralları.

![Just-ın-time senaryosu](./media/security-center-just-in-time/just-in-time-scenario.png)

## <a name="how-does-jit-access-work"></a>JIT erişim nasıl çalışır?

Tam zamanında etkinleştirildiğinde Güvenlik Merkezi Azure Vm'lere gelen trafiği bir NSG kuralı oluşturarak kilitler. Sizin için gelen trafiği kilitlenir VM bağlantı noktalarını seçin. Bu bağlantı noktalarına tam zamanında çözüm tarafından denetlenir.

Bir kullanıcı bir sanal makine erişimine izin istediğinde, Güvenlik Merkezi kullanıcı olup olmadığını denetler [rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/role-assignments-portal.md) başarıyla VM'ye erişime izin istemek için izin vermek izinleri. İstek onaylanırsa Güvenlik Merkezi, ağ güvenlik grupları (Nsg'ler) seçilen bağlantı noktası ve istenen kaynak IP adresleri veya aralıklar, belirtilen süre için gelen trafiğe izin verecek şekilde otomatik olarak yapılandırır. Süresi dolduktan sonra Güvenlik Merkezi Nsg'ler önceki durumlarına geri yükler. Önceden oluşturulmuş bu bağlantılar, ancak kesilmez.

> [!NOTE]
> Güvenlik Merkezi tam zamanında VM erişimi şu anda yalnızca Vm'lerinde dağıtılan destekleyen Azure Resource Manager üzerinden. Klasik ve Resource Manager dağıtım modelleri hakkında daha fazla bilgi için [Azure Resource Manager ve klasik dağıtım](../azure-resource-manager/resource-manager-deployment-model.md).
>
>

JIT üzerinden erişebilirsiniz:
- [Azure Güvenlik Merkezi'nde JIT erişim kullanma](#jit-asc)
- [Bir Azure VM dikey penceresinde JIT erişim kullanma](#jit-vm)

## Azure Güvenlik Merkezi'nde JIT erişim kullanma <a name="jit-asc"></a>

1. **Güvenlik Merkezi** panosunu açın.

2. Sol bölmede seçin **tam zamanında VM erişimi**.

![Tam zamanında VM erişimi kutucuğu](./media/security-center-just-in-time/just-in-time.png)

**Tam zamanında VM erişimi** penceresi açılır.

![Tam zamanında VM erişimi kutucuğu](./media/security-center-just-in-time/just-in-time-access.png)

**Tam zamanında VM erişimi** , Vm'lerinizin durumu hakkında bilgi sağlar:

- **Yapılandırılmış** -tam zamanında VM erişimi desteklemek için yapılandırılmış VM'ler. Sunulan veriler geçen haftaya aittir ve her VM için onaylanan istekler, son erişim tarihi ve saati ve son kullanıcı sayısını içerir.
- **Önerilen** -tam zamanında VM erişimini destekleyebilen ancak bunun için yapılandırılmamış olan VM'ler. Bu sanal makineler için tam zamanında VM erişiminin etkinleştirmenizi öneririz. Bkz: [tam zamanında erişim ilkesini yapılandırma](#jit-config).
- **Öneri olmayan** - Bir VM’nin önerilmemesinin olası nedenleri şunlardır:
  - Eksik NSG - tam zamanında çözüm bir NSG karşılanması gerekir.
  - Klasik VM - Güvenlik Merkezi tam zamanında VM erişimi şu anda yalnızca Azure Resource Manager üzerinden dağıtılan Vm'leri desteklemektedir. Klasik dağıtım, just-ın-time çözüm tarafından desteklenmiyor.
  - Diğer - bir VM bu just-ın-time çözüm abonelik veya kaynak grubunun güvenlik ilkesinde kapalı değilse veya VM'ye genel bir IP adresi eksik olabilir ve yerinde bir NSG yok kategorisindedir.

### JIT erişim ilkesini yapılandırma<a name="jit-config"></a>

Etkinleştirmek istediğiniz Vm'leri seçmek için:

1. Altında **tam zamanında VM erişimi**seçin **önerilen** sekmesi.

   ![Tam zamanında erişim etkinleştir](./media/security-center-just-in-time/enable-just-in-time-access.png)

2. Altında **sanal makine**, etkinleştirmek istediğiniz Vm'leri seçin. Bu, bir VM yanında bir onay işareti koyar.
3. Seçin **etkinleştirme vm'lerde JIT**.
   1. Bu dikey pencere, Azure Güvenlik Merkezi tarafından önerilen varsayılan bağlantı noktalarını görüntüler:
      - 22 - SSH
      - 3389 - RDP
      - 5985 - WinRM 
      - 5986 - WinRM
   2. Özel bağlantı noktalarını da yapılandırabilirsiniz. Bunu yapmak için **Ekle**. 
   3. İçinde **bağlantı noktası yapılandırması Ekle**, seçtiğiniz yapılandırmak için her bağlantı noktası için her ikisi de varsayılan ve özel, aşağıdaki ayarları özelleştirebilirsiniz:
      - **Protokol türü**-bir istek onaylandığında, bu bağlantı noktasına izin protokolü.
      - **İzin verilen kaynak IP adresleri**-bir istek onaylandığında, bu bağlantı noktasına izin verilen IP aralıkları.
      - **En fazla istek süresi**-en fazla zaman penceresi boyunca belirli bir bağlantı noktası açılabilir.

4. **Kaydet**’i seçin.


> [!NOTE]
>Bir VM için JIT VM erişimi etkinleştirildiğinde, Azure Güvenlik Merkezi seçilen bağlantı noktası için "tüm gelen trafiği engelle" kuralları ile ilişkilendirilmiş ağ güvenlik grupları oluşturur. Seçili bağlantı noktaları için oluşturulmuş olan diğer kuralları, mevcut kurallar önceliği yeni "tüm gelen trafiği engelle" kurallardan yararlanın. Seçili bağlantı noktalarına hiçbir mevcut kurallar varsa, yeni "tüm gelen trafiği engelle" kuralları en yüksek önceliğiniz ağ güvenlik gruplarında yararlanın.
>

### <a name="request-jit-access-to-a-vm"></a>İstek JIT VM erişimi

VM'ye erişime izin istemek için:
1.  Altında **tam zamanında VM erişimi**seçin **yapılandırıldı**.
2.  Altında **Vm'leri**, tam zamanında erişim için etkinleştirmek istediğiniz sanal makineleri denetleyin.
3.  Seçin **erişim isteği**. 
  ![vm için erişim isteği](./media/security-center-just-in-time/request-access-to-a-vm.png)
4.  Altında **erişim isteği**, açmak istediğiniz bağlantı noktası her VM için yapılandırma ve bağlantı noktası üzerinde açık kaynak IP adresleri ve bağlantı noktası olacağı zaman penceresini açın. Yalnızca tam zamanında ilkede yapılandırılan bağlantı noktası erişimi isteme mümkün olacaktır. Her bağlantı noktasının tam zamanında İlkesi'nden türetilen izin verilen süre üst sınırını vardır.
5.  Seçin **bağlantı noktalarını açmak**.

> [!NOTE]
> Erişim isteyen bir kullanıcı bir proxy'nin arkasındayken seçeneği olup olmadığını **IP'mi** çalışmayabilir. Kuruluşunuzun tam IP adresi aralığı tanımlamanız gerekebilir.

### <a name="editing-a-jit-access-policy"></a>JIT erişim ilkesini düzenleme

Bir sanal makinenin var olan tam zamanında ilkesi ekleme ve yapılandırma için bu sanal Makineyi korumak için yeni bir bağlantı noktası, ya tüm diğer zaten korumalı bir bağlantı noktasına ilgili ayarı değiştirerek değiştirebilirsiniz.

Bir sanal makinenin mevcut just-ın-time ilkesini düzenlemek için:
1. İçinde **yapılandırıldı** sekmesindeki **Vm'leri**, o sanal makine için satır içinde üç noktaya tıklayarak bir bağlantı noktası eklenecek bir VM seçin. 
2. **Düzenle**’yi seçin.
3. Altında **JIT VM erişimi Yapılandırması**, mevcut zaten korumalı olan bir bağlantı noktası ayarlarını düzenleyebilir veya yeni bir özel bağlantı noktasını ekleyin. Daha fazla bilgi için [tam zamanında erişim ilkesini yapılandırma](#jit-config). 
  ![jit vm access](./media/security-center-just-in-time/edit-policy.png)

## Bir Azure VM dikey penceresinde JIT erişim kullanma <a name="jit-vm"></a>

Kolaylık olması için VM dikey penceresinde Azure içinde JIT doğrudan kullanarak bir sanal makineye bağlanabilirsiniz.

### <a name="configuring-a-just-in-time-access-policy"></a>Tam zamanında erişim ilkesini yapılandırma 
Sanal makineleriniz arasında tam zamanında erişim alma kolaylaştırmak için yalnızca doğrudan tam zamanında erişim VM içinde izin vermek için bir VM ayarlayabilirsiniz.

1. Azure portalında **sanal makineler**.
2. Just-ın-time erişimi sınırlandırmak istediğiniz sanal makineye tıklayın.
3. Menüde **yapılandırma**.
4. Altında **tam olarak zaman erişim** tıklayın **just-ın-time ilkesini etkinleştir**. 

Bu, aşağıdaki ayarları kullanarak sanal makine için tam zamanında erişim sağlar:

- Windows sunucuları:
    - RDP bağlantı noktası 3389
    - izin verilen en fazla erişim 3 saat
    - İzin verilen kaynak IP adresleri için ayarlanır
- Linux sunucuları:
    - SSH bağlantı noktası 22
    - izin verilen en fazla erişim 3 saat
    - İzin verilen kaynak IP adresleri için ayarlanır
     
Bir VM yalnızca kendi yapılandırma sayfasına gittiğinizde, etkin zamanında zaten varsa, just-ın-time etkinleştirildiğini ve bağlantı ayarlarını görüntülemek ve değiştirmek için Azure Güvenlik Merkezi'nde İlkesi'ni açmak için kullanabileceğiniz olduğunu görmek mümkün olacaktır.

![JIT vm yapılandırması](./media/security-center-just-in-time/jit-vm-config.png)

### <a name="requesting-jit-access-to-a-vm"></a>JIT VM erişimi isteme

Azure portalında Azure VM'ye bağlanmaya çalışırken bu VM üzerinde yapılandırılan bir tam zamanında erişim ilkesi varsa görmek için denetler.

- JIT VM üzerinde yapılandırılan yoksa, JIT İlkesi yapılandırmak istiyorsanız istenir.

  ![JIT istemi](./media/security-center-just-in-time/jit-prompt.png)

- VM üzerinde yapılandırılan bir JIT ilke varsa, tıklayabilirsiniz **erişim isteği** için VM kümesi JIT ilkesine uygun olarak erişmesini sağlamak için. Aşağıdaki varsayılan parametreleri erişim isteniyor:
    - **kaynak IP**: 'Any' (*) (değiştirilemez)
    - **zaman aralığı**: 3 saat (değiştirilemez)
    - **bağlantı noktası numarası** RDP 3389 numaralı bağlantı noktası için Windows / Linux için 22 numaralı bağlantı noktasını (bağlantı noktası numarasını değiştirebilirsiniz **sanal makineye bağlanma** iletişim kutusu.)


  >![JIT erişim isteği](./media/security-center-just-in-time/jit-request-access.png)

## <a name="auditing-jit-access-activity"></a>JIT erişim etkinliğini denetleme

Günlük araması'nı kullanarak VM etkinlikleri hakkında Öngörüler elde edebilirsiniz. Günlükleri görüntülemek için:

1. Altında **tam zamanında VM erişimi**seçin **yapılandırıldı** sekmesi.
2. Altında **Vm'leri**, o sanal makine için satır içinde üç noktaya tıklayarak ilgili bilgileri görüntülemek için bir sanal Makineyi seçin. Bu, bir menü açılır.
3. Seçin **etkinlik günlüğü** menüsünde. Bu açılır **etkinlik günlüğü**.

   ![Etkinlik günlüğü seçin](./media/security-center-just-in-time/select-activity-log.png)

   **Etkinlik günlüğü** önceki işlem saati, tarih ve abonelik yanı sıra bu VM için filtrelenmiş bir görünüm sağlar.

Günlük bilgilerini seçerek indirebilirsiniz **tüm öğeleri CSV olarak indirmek için buraya tıklayın**.

Seç ve filtreleri değiştirmek **Uygula** arama ve günlük oluşturmak için.


## <a name="permissions-needed-to-configure-and-use-jit"></a>Yapılandırma ve JIT kullanma için gereken izinler

Yapılandırma veya bir VM için bir JIT ilkesi düzenlemek bir kullanıcı etkinleştirmek için bu gerekli ayrıcalıkların ayarlayın.

Bu atama *eylemleri* rolü: 
- VM ile ilişkili bir abonelik veya kaynak grubunun kapsamına:
  - Microsoft.Security/locations/jitNetworkAccessPolicies/write
- Bir abonelik veya kaynak grubu veya VM üzerinde kapsamı:
  - Microsoft.Compute/virtualMachines/write 

Bir kullanıcının başarıyla JIT VM erişimi istemek üzere etkinleştirmek için bu ayrıcalıkların ayarlayın: Bu atama *eylemleri* kullanıcı:
- VM ile ilişkili bir abonelik veya kaynak grubunun kapsamına:
  - Microsoft.Security/locations/{the_location_of_the_VM}/jitNetworkAccessPolicies/ Başlat/eylem
- Bir abonelik veya kaynak grubu veya VM üzerinde kapsamı:
  - Microsoft.Compute/virtualMachines/read



## <a name="use-jit-programmatically"></a>JIT program aracılığıyla kullanma
Ayarlama ve tam zamanında REST API'leri aracılığıyla ve PowerShell aracılığıyla kullanın.

### <a name="using-just-in-time-vm-access-via-rest-apis"></a>Tam zamanında VM erişimi REST API'leri aracılığıyla kullanarak

Tam zamanında VM erişimi özelliğini, Azure Güvenlik Merkezi API aracılığıyla kullanılabilir. Yapılandırılan VM'ler hakkında bilgi edinin, yenilerini ekleyin, bir VM ve daha fazlasını bu API aracılığıyla erişim isteyin. Bkz: [JIT ağ erişim ilkelerini](https://docs.microsoft.com/rest/api/securitycenter/jitnetworkaccesspolicies), just-ın-time REST API hakkında daha fazla bilgi edinmek için.

### <a name="using-jit-vm-access-via-powershell"></a>JIT VM erişimi PowerShell aracılığıyla kullanarak 

PowerShell aracılığıyla tam zamanında VM erişimi çözümü kullanmak için resmi Azure Güvenlik Merkezi PowerShell cmdlet'lerini kullanın ve özellikle `Set-AzJitNetworkAccessPolicy`.

Aşağıdaki örnek, bir tam zamanında VM erişimi ilkesi belirli bir sanal makine üzerinde ayarlar ve aşağıdaki ayarlar:
1.  22 ve 3389 numaralı bağlantı noktalarını kapatın.
2.  Onaylanan istek başına açılması için her biri için 3 saatte bir maksimum zaman aralığında ayarlayın.
3.  Kaynak IP adresleri denetlemek için erişim isteme ve onaylanan tam zamanında Erişim İstek başarılı bir oturum oluşturmak kullanıcının kullanıcı sağlar.

Bunu gerçekleştirmek için PowerShell'de aşağıdaki komutu çalıştırın:

1.  Bir VM için tam zamanında VM erişimi ilkesi tutan değişkenine atayın:

        $JitPolicy = (@{
         id="/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Compute/virtualMachines/VMNAME"
        ports=(@{
             number=22;
             protocol="*";
             allowedSourceAddressPrefix=@("*");
             maxRequestAccessDuration="PT3H"},
             @{
             number=3389;
             protocol="*";
             allowedSourceAddressPrefix=@("*");
             maxRequestAccessDuration="PT3H"})})

2.  Bir dizi VM tam zamanında VM erişim ilkesi ekleyin:
    
        $JitPolicyArr=@($JitPolicy)

3.  Seçili VM üzerinde tam zamanında VM erişimi ilkesi yapılandırın:
    
        Set-AzJitNetworkAccessPolicy -Kind "Basic" -Location "LOCATION" -Name "default" -ResourceGroupName "RESOURCEGROUP" -VirtualMachine $JitPolicyArr 

#### <a name="requesting-access-to-a-vm"></a>Bir VM için erişim isteği

Aşağıdaki örnekte, bir tam zamanında VM erişim isteği hangi bağlantı noktası 22 ve belirli bir süre için belirli bir IP adresi açılması için istenen belirli bir VM'ye görebilirsiniz:

PowerShell'de aşağıdaki komutu çalıştırın:
1.  VM istek erişim özelliklerini yapılandır

        $JitPolicyVm1 = (@{
          id="/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Compute/virtualMachines/VMNAME"
        ports=(@{
           number=22;
           endTimeUtc="2018-09-17T17:00:00.3658798Z";
           allowedSourceAddressPrefix=@("IPV4ADDRESS")})})
2.  Bir dizi VM erişimi İstek parametreleri ekleyin:

        $JitPolicyArr=@($JitPolicyVm1)
3.  (Kullanım 1. adımda aldığınız kaynak kimliği) erişim isteği gönder

        Start-AzJitNetworkAccessPolicy -ResourceId "/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Security/locations/LOCATION/jitNetworkAccessPolicies/default" -VirtualMachine $JitPolicyArr

Daha fazla bilgi için PowerShell cmdlet'i belgelerine bakın.


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Güvenlik Merkezi yardımcı nasıl tam zamanında VM erişimini denetim öğrendiniz, Azure sanal makinelerine erişim.

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

- [Güvenlik ilkelerini ayarlama](tutorial-security-policy.md) — Azure Abonelikleriniz ve kaynak grupları için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
- [Güvenlik önerilerini yönetme](security-center-recommendations.md) -önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
- [Güvenlik durumunu izleme](security-center-monitoring.md) — Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
- [Yönetme ve güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md) — yönetme ve güvenlik uyarılarını yanıtlama hakkında bilgi edinin.
- [İş ortağı çözümlerini izleme](security-center-partner-solutions.md) — iş ortağı çözümlerinizin sistem durumunu izleme hakkında bilgi edinin.
- [Güvenlik Merkezi SSS](security-center-faq.md) — hizmet kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
- [Azure Güvenlik blogu](https://blogs.msdn.microsoft.com/azuresecurity/) - Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.

