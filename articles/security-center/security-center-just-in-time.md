---
title: Azure Güvenlik Merkezi'nde tam zamanında sanal makine erişimini | Microsoft Docs
description: Bu belge Azure Güvenlik Merkezi yardımcı nasıl tam zamanında VM erişimini denetim gösterir, Azure sanal makinelerine erişim.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: 671930b1-fc84-4ae2-bf7c-d34ea37ec5c7
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 6/17/2019
ms.author: v-mohabe
ms.openlocfilehash: eb9366acf82c94bdf99c4d4f0c7c6bdf4f51e06d
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67294998"
---
# <a name="manage-virtual-machine-access-using-just-in-time"></a>Tam zamanında kullanarak sanal makine erişimini yönetme

Just-ın-time (JIT) sanal makine (VM) erişimi, Vm'lere gerektiğinde bağlanılabilmesi için kolay erişim sağlamanın yanı sıra saldırılara maruz kalma riskinizi azaltır, Azure Vm'lere gelen trafiği kilitlemek için kullanılabilir.

> [!NOTE]
> Just-ın-time özelliği Güvenlik Merkezi'nin standart katmanında kullanılabilir.  Güvenlik Merkezi’nin fiyatlandırma katmanları hakkında daha fazla bilgi almak için bkz. [Fiyatlandırma](security-center-pricing.md).


> [!NOTE]
> Güvenlik Merkezi tam zamanında VM erişimi şu anda yalnızca Vm'lerinde dağıtılan destekleyen Azure Resource Manager üzerinden. Klasik ve Resource Manager dağıtım modelleri hakkında daha fazla bilgi için [Azure Resource Manager ve klasik dağıtım](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="attack-scenario"></a>Saldırı senaryosu

Deneme yanılma hedef yönetim noktaları bir sanal makineye erişmek için bir yol olarak genellikle saldırıları. Başarılı olursa, bir saldırganın VM yükü denetimini ve ortamınıza bir köprübaşı oluşturmak.

Bir deneme yanılma saldırısı maruz kalma riskinizi azaltmak için bir bağlantı noktası açık olduğu süre miktarını sınırlamak için yoludur. Yönetim bağlantı noktalarının her zaman açık olması gerekmez. Bunların yalnızca VM’ye bağlı olduğunuzda (örneğin, yönetim veya bakım görevleri gerçekleştirmek için) açık olması gerekir. Tam zamanında etkinleştirildiğinde Güvenlik Merkezi kullanan [ağ güvenlik grubu](../virtual-network/security-overview.md#security-rules) (NSG) ve Azure güvenlik duvarı kuralları, saldırganlar tarafından hedeflenemez hangi yönetim bağlantı noktalarına erişimi kısıtlayın.

![Just-ın-time senaryosu](./media/security-center-just-in-time/just-in-time-scenario.png)

## <a name="how-does-jit-access-work"></a>JIT erişim nasıl çalışır?

Tam zamanında etkinleştirildiğinde Güvenlik Merkezi Azure Vm'lere gelen trafiği bir NSG kuralı oluşturarak kilitler. Sizin için gelen trafiği kilitlenir VM bağlantı noktalarını seçin. Bu bağlantı noktalarına tam zamanında çözüm tarafından denetlenir.

Bir kullanıcı bir sanal makine erişimine izin istediğinde, Güvenlik Merkezi kullanıcı olup olmadığını denetler [rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/role-assignments-portal.md) başarıyla VM'ye erişime izin istemek için izin vermek izinleri. İstek onaylanırsa Güvenlik Merkezi otomatik olarak Azure güvenlik duvarı ve ağ güvenlik grupları (Nsg'ler) seçilen bağlantı noktası ve istenen kaynak IP adresleri veya aralıklar, belirtilen süre için gelen trafiğe izin verecek şekilde yapılandırır. Süresi dolduktan sonra Güvenlik Merkezi Nsg'ler önceki durumlarına geri yükler. Önceden oluşturulmuş bu bağlantılar, ancak kesilmez.

 > [!NOTE]
 > Ardından JIT erişim isteği için bir Azure güvenlik duvarı arkasındaki bir VM'ye onaylanırsa Güvenlik Merkezi hem NSG hem de güvenlik duvarı ilkesi kuralları otomatik olarak değiştirir. Belirtilen süre miktarı kuralları seçilen bağlantı noktası ve istenen kaynak IP adresi veya aralığı gelen trafiğe izin. Süre bittikten sonra Güvenlik Merkezi güvenlik duvarı ve NSG kuralları önceki durumlarına geri yükler.


## <a name="permissions-needed-to-configure-and-use-jit"></a>Yapılandırma ve JIT kullanma için gereken izinler

| Bir kullanıcıya etkinleştirmek için: | İzinleri ayarlamak için|
| --- | --- |
| Yapılandırma veya bir VM için bir JIT ilkesini Düzenle | *Bu Eylemler, role atayın:*  VM ile ilişkili bir abonelik veya kaynak grubunun kapsamına: ```Microsoft.Security/locations/jitNetworkAccessPolicies/write``` Bir abonelik veya kaynak grubu veya VM üzerinde kapsamı: ```Microsoft.Compute/virtualMachines/write``` | 
| ||
|İstek JIT VM erişimi | *Bu Eylemler, kullanıcıya atayın:*  VM ile ilişkili bir abonelik veya kaynak grubunun kapsamına:  ```Microsoft.Security/locations/{the_location_of_the_VM}/jitNetworkAccessPolicies/initiate/action``` Bir abonelik veya kaynak grubu veya VM üzerinde kapsamı: ```Microsoft.Compute/virtualMachines/read``` |


## <a name="configure-jit-on-a-vm"></a>JIT VM üzerinde yapılandırma

Bir VM'de bir JIT ilkesini yapılandırmak için izleyebileceğiniz 3 yol vardır:

- [Azure Güvenlik Merkezi'nde JIT erişimi yapılandırma](#jit-asc)
- [Bir Azure VM dikey penceresinde JIT erişimi yapılandırma](#jit-vm)
- [Bir VM'de programlama JIT ilkesi yapılandırma](#jit-program)

## <a name="configure-jit-in-asc"></a>ARTAN düzende JIT yapılandırın

ASC ile JIT İlkesi ve istek erişimi JIT İlkesi'ni kullanarak bir VM yapılandırabilirsiniz.


### Bir VM'de ASC JIT erişimi yapılandırma <a name="jit-asc"></a>

1. **Güvenlik Merkezi** panosunu açın.

2. Sol bölmede seçin **tam zamanında VM erişimi**.

    ![Tam zamanında VM erişimi kutucuğu](./media/security-center-just-in-time/just-in-time.png)

    **Tam zamanında VM erişimi** penceresi açılır.

      ![Tam zamanında erişim etkinleştir](./media/security-center-just-in-time/enable-just-in-time.png)

    **Tam zamanında VM erişimi** , Vm'lerinizin durumu hakkında bilgi sağlar:

    - **Yapılandırılmış** -tam zamanında VM erişimi desteklemek için yapılandırılmış VM'ler. Sunulan veriler geçen haftaya aittir ve her VM için onaylanan istekler, son erişim tarihi ve saati ve son kullanıcı sayısını içerir.
    - **Önerilen** -tam zamanında VM erişimini destekleyebilen ancak bunun için yapılandırılmamış olan VM'ler. Bu sanal makineler için tam zamanında VM erişiminin etkinleştirmenizi öneririz. 
    - **Öneri olmayan** - Bir VM’nin önerilmemesinin olası nedenleri şunlardır:
      - Eksik NSG - tam zamanında çözüm bir NSG karşılanması gerekir.
      - Klasik VM - Güvenlik Merkezi tam zamanında VM erişimi şu anda yalnızca Azure Resource Manager üzerinden dağıtılan Vm'leri desteklemektedir. Klasik dağıtım, just-ın-time çözüm tarafından desteklenmiyor. 
      - Diğer - bir VM Bu abonelik veya kaynak grubunun güvenlik ilkesinde tam zamanında çözüm kapalıysa veya VM'ye genel bir IP adresi eksik olabilir ve bir NSG yerinde yoksa kategorisindedir.

3. Seçin **önerilen** sekmesi.

4. Altında **sanal makine**, etkinleştirmek istediğiniz Vm'leri tıklayın. Bu, bir VM yanında bir onay işareti koyar.

5. Tıklayın **etkinleştirme vm'lerde JIT**.
   -. Bu dikey pencere, Azure Güvenlik Merkezi tarafından önerilen varsayılan bağlantı noktalarını görüntüler:
      - 22 - SSH
      - 3389 - RDP
      - 5985 - WinRM 
      - 5986 - WinRM
6. Özel bağlantı noktalarını da yapılandırabilirsiniz:

      1. **Ekle**'yi tıklatın. **Bağlantı noktası yapılandırması Ekle** penceresi açılır.
      2. Her bağlantı noktası için yapılandırmak, her iki varsayılan seçin ve özel, aşağıdaki ayarları özelleştirebilirsiniz:

    - **Protokol türü**-bir istek onaylandığında, bu bağlantı noktasına izin protokolü.
    - **İzin verilen kaynak IP adresleri**-bir istek onaylandığında, bu bağlantı noktasına izin verilen IP aralıkları.
    - **En fazla istek süresi**-en fazla zaman penceresi boyunca belirli bir bağlantı noktası açılabilir.

     3. **Tamam**'ı tıklatın.

1. **Kaydet**’e tıklayın.

> [!NOTE]
>Bir VM için JIT VM erişimi etkinleştirildiğinde, Azure Güvenlik Merkezi "tüm gelen trafiği engelle" kuralları Seçili bağlantı noktaları için ilişkili ağ güvenlik grupları ve Azure güvenlik duvarı ile oluşturur. Seçili bağlantı noktaları için oluşturulmuş olan diğer kuralları, mevcut kurallar önceliği yeni "tüm gelen trafiği engelle" kurallardan yararlanın. Seçili bağlantı noktalarına hiçbir mevcut kurallar varsa, yeni "tüm gelen trafiği engelle" kuralları ağ güvenlik grupları ve Azure güvenlik duvarı en yüksek önceliğiniz yararlanın.


## <a name="request-jit-access-via-asc"></a>ASC aracılığıyla JIT erişim isteği

Bir VM'yi ASC aracılığıyla erişim istemek için:

1. Altında **tam zamanında VM erişimi**seçin **yapılandırıldı** sekmesi.

2. Altında **sanal makine**, erişim istemek için istediğiniz VM'lerin tıklayın. Bu sanal Makinenin yanında bir onay işareti koyar.


    - Simge **bağlantı ayrıntıları** sütunu JIT NSG veya FW etkin olup olmadığını gösterir. Hem de etkinleştirilirse, yalnızca güvenlik duvarı simgesi görünür.

    - **Bağlantı ayrıntıları** sütun VM'ye bağlanmak için gereken doğru bilgileri sağlar hem de açık bağlantı noktalarını gösterir.

      ![Tam zamanında erişim isteme](./media/security-center-just-in-time/request-just-in-time-access.png)

3. Tıklayın **erişim isteği**. **Erişim isteği** penceresi açılır.

      ![JIT ayrıntıları](./media/security-center-just-in-time/just-in-time-details.png)

4. Altında **erişim isteği**, açmak istediğiniz bağlantı noktası her VM için yapılandırma ve bağlantı noktası üzerinde açık kaynak IP adresleri ve bağlantı noktası olacağı zaman penceresini açın. Yalnızca tam zamanında ilkede yapılandırılan bağlantı noktası erişimi isteme mümkün olacaktır. Her bağlantı noktasının tam zamanında İlkesi'nden türetilen izin verilen süre üst sınırını vardır.

5. Tıklayın **bağlantı noktalarını açmak**.

> [!NOTE]
> Erişim isteyen bir kullanıcı bir proxy'nin arkasındayken seçeneği olup olmadığını **IP'mi** çalışmayabilir. Kuruluşunuzun tam IP adresi aralığı tanımlamanız gerekebilir.

## <a name="edit-a-jit-access-policy-via-asc"></a>ASC aracılığıyla bir JIT erişim ilkesini Düzenle

Bir sanal makinenin var olan tam zamanında ilkesi ekleme ve yapılandırma için bu sanal Makineyi korumak için yeni bir bağlantı noktası, ya tüm diğer zaten korumalı bir bağlantı noktasına ilgili ayarı değiştirerek değiştirebilirsiniz.

Bir sanal makinenin mevcut just-ın-time ilkesini düzenlemek için:
1. İçinde **yapılandırıldı** sekmesindeki **Vm'leri**, o sanal makine için satır içinde üç noktaya tıklayarak bir bağlantı noktası eklenecek bir VM seçin. 

1. **Düzenle**’yi seçin.
1. Altında **JIT VM erişimi Yapılandırması**, mevcut zaten korumalı olan bir bağlantı noktası ayarlarını düzenleyebilir veya yeni bir özel bağlantı noktasını ekleyin. 
  ![jit vm access](./media/security-center-just-in-time/edit-policy.png)

## <a name="audit-jit-access-activity-in-asc"></a>ASC, JIT erişim etkinliğini denetleme

Günlük araması'nı kullanarak VM etkinlikleri hakkında Öngörüler elde edebilirsiniz. Günlükleri görüntülemek için:

1. Altında **tam zamanında VM erişimi**seçin **yapılandırıldı** sekmesi.
2. Altında **Vm'leri**, o sanal makine için satır içinde üç noktaya tıklayarak ilgili bilgileri görüntülemek için bir VM'ı seçip **etkinlik günlüğü** menüsünde. **Etkinlik günlüğü** açılır.

   ![Etkinlik günlüğü seçin](./media/security-center-just-in-time/select-activity-log.png)

   **Etkinlik günlüğü** önceki işlem saati, tarih ve abonelik yanı sıra bu VM için filtrelenmiş bir görünüm sağlar.

Günlük bilgilerini seçerek indirebilirsiniz **tüm öğeleri CSV olarak indirmek için buraya tıklayın**.

Filtreleri değiştirmek ve tıklayın **Uygula** arama ve günlük oluşturmak için.



## Bir Azure VM dikey penceresinde JIT erişimi yapılandırma <a name="jit-vm"></a>

Kolaylık olması için VM dikey penceresinde Azure içinde JIT doğrudan kullanarak bir sanal makineye bağlanabilirsiniz.

### <a name="configure-jit-access-on-a-vm-via-the-azure-vm-blade"></a>Azure VM dikey penceresi aracılığıyla bir VM'de JIT erişimi yapılandırma

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

### <a name="request-jit-access-to-a-vm-via-the-azure-vm-blade"></a>Azure VM dikey penceresinde yoluyla VM'ye JIT erişim isteği

Azure portalında Azure VM'ye bağlanmaya çalışırken bu VM üzerinde yapılandırılan bir tam zamanında erişim ilkesi varsa görmek için denetler.

- VM üzerinde yapılandırılan bir JIT ilke varsa, tıklayabilirsiniz **erişim isteği** için VM kümesi JIT ilkesine uygun olarak erişmesini sağlamak için. 

  >![JIT isteği](./media/security-center-just-in-time/jit-request.png)

  Aşağıdaki varsayılan parametreleri erişim isteniyor:

  - **kaynak IP**: 'Any' (*) (değiştirilemez)
  - **zaman aralığı**: 3 saat (değiştirilemez)  <!--Isn't this set in the policy-->
  - **bağlantı noktası numarası** RDP 3389 numaralı bağlantı noktası için Windows / Linux (değiştirilebilir) için 22 numaralı bağlantı noktası

    > [!NOTE]
    > Azure güvenlik duvarıyla korunan bir VM için bir istek onaylandıktan sonra Güvenlik Merkezi VM bağlanmak için kullanılacak uygun bağlantı ayrıntıları (bağlantı noktası eşlemesi dnat'ı tablosundan) ile kullanıcı sağlar.

- JIT VM üzerinde yapılandırılan yoksa, JIT İlkesi yapılandırmak istiyorsanız istenir.

  ![JIT istemi](./media/security-center-just-in-time/jit-prompt.png)

## Bir VM'de programlama JIT ilkesi yapılandırma  <a name="jit-program"></a>

Ayarlama ve tam zamanında REST API'leri aracılığıyla ve PowerShell aracılığıyla kullanın.

## <a name="jit-vm-access-via-rest-apis"></a>REST API'leri aracılığıyla JIT VM erişimi

Tam zamanında VM erişimi özelliğini, Azure Güvenlik Merkezi API aracılığıyla kullanılabilir. Yapılandırılan VM'ler hakkında bilgi edinin, yenilerini ekleyin, bir VM ve daha fazlasını bu API aracılığıyla erişim isteyin. Bkz: [JIT ağ erişim ilkelerini](https://docs.microsoft.com/rest/api/securitycenter/jitnetworkaccesspolicies), just-ın-time REST API hakkında daha fazla bilgi edinmek için.

## <a name="jit-vm-access-via-powershell"></a>PowerShell aracılığıyla JIT VM erişimi

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

#### <a name="request-access-to-a-vm-via-powershell"></a>PowerShell yoluyla VM'ye erişim isteği

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

