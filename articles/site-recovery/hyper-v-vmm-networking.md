---
title: "IP adresi Azure Site Recovery ile yük devretme sonrasında bir ikincil şirket içi siteye bağlanmak için ayarlama | Microsoft Docs"
description: "Bir ikincil şirket içi site vm'lerinin yük devretme sonrasında Azure Site Recovery bağlanmak için IP adresleme yukarı ayarlanacağını açıklar."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 02/12/2018
ms.author: rayne
ms.openlocfilehash: 531705bc704b3366c1c670ecf07c809ade67bc55
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="set-up-ip-addressing-to-connect-to-a-secondary-on-premises-site-after-failover"></a>IP adresi yük devretme sonrasında bir ikincil şirket içi siteye bağlanmak için ayarlama

Hyper-V Vm'lerini ikincil bir siteyi System Center Virtual Machine Manager (VMM) bulutlarında üzerinden başarısız olduktan sonra gerekir çoğalma VM'ler bağlanın. Bu makalede, bunu yapmak için yardımcı olur. 

## <a name="connection-options"></a>Bağlantı seçenekleri

Yük devretme sonrasında, çeşitli şekillerde çoğaltma VM'ler için IP adresleme işlemek için vardır: 

- **Yük devretme sonrasında aynı IP adresini korumak**: Bu senaryoda, çoğaltılmış VM birincil VM aynı IP adresine sahip. Bu basitleştirir ilgili ağ sorunları yük devretme sonrasında, ancak bazı altyapı iş gerektirir.
- **Yük devretme işleminden sonra farklı bir IP adresi kullanmak**: Bu senaryoda VM yük devretme sonrasında yeni bir IP adresi alır. 
 

## <a name="retain-the-ip-address"></a>IP adresi koru

IP adreslerini birincil siteden ikincil siteye yük devretme sonrasında korumak istiyorsanız, şunları yapabilirsiniz:

- Birincil ve ikincil siteler arasında Uzatılan bir alt ağ dağıtın.
- Bir tam alt ağ yük devretme birincil sunucudan ikincil site için gerçekleştirin. IP adreslerini yeni konumunu belirtmek için bir yol güncelleştirmeniz gerekir.


### <a name="deploy-a-stretched-subnet"></a>Esnetilen bir alt ağ dağıtma

Esnetilen yapılandırması, alt ağ aynı anda hem birincil hem de ikincil sitelerde kullanılabilir. İkincil siteye bir makine ve (Katman 3) IP adresi yapılandırmasıyla taşıdığınızda Uzatılan bir alt ağ otomatik olarak trafiğini yeni konumuna yönlendirir. 

- Bir katman 2 (veri bağlantı katmanı) açısından Uzatılan bir VLAN yönetebilirsiniz ağ ekipmanları gerekir.
- VLAN yayarak olası hata etki alanı her iki sitede genişletir. Bu tek hata noktası olur. Olası olsa da, böyle bir senaryoda, bir yayın storm gibi bir olay ayırmak mümkün olmayabilir. 


### <a name="fail-over-a-subnet"></a>Bir alt ağ başarısız

Gerçekte uzatma olmadan esnetilen alt avantajlarından yararlanabilmek için tüm alt ağ üzerinden başarısız olabilir. Bu çözümde, bir alt ağ kullanılabilir kaynak veya hedef site, ancak her ikisi de aynı anda.

- IP adres alanı bir yük devretme durumunda korumak için program aracılığıyla alt ağların bir siteden diğerine taşımak yönlendirici altyapı düzenleyebilirsiniz.
- Bir yük devretme gerçekleştiğinde, alt ağlar ile ilişkili Vm'leri taşıyın.
- Bu yaklaşımın en büyük dezavantajı, bir arıza olması durumunda olduğundan, tüm alt ağa taşıyın gerekir.

#### <a name="example"></a>Örnek

Tam alt ağ yük devretme bir örneği burada verilmiştir. 

- Yük devretme önce birincil site alt 192.168.1.0/24 içinde çalışan uygulamalar vardır.
- Yük devretme sırasında bu alt ağ içindeki VM'ler tümüne ikincil siteye yük devredildi ve IP adreslerini korur. 
- Tüm siteler arasındaki yolları alt 192.168.1.0/24 tüm sanal makineleri ikincil siteye artık taşınmış olgu yansıtacak şekilde değiştirilmesi gerekir.

Aşağıdaki grafik önce ve yük devretme sonrasında alt ağlar gösterilmektedir.


**Önce yük devretme**

![Önce yük devretme](./media/hyper-v-vmm-networking/network-design2.png)

**Yük devretme sonrasında**

![Yük devretme sonrasında](./media/hyper-v-vmm-networking/network-design3.png)

Yük devretme işleminden sonra Site Recovery VM üzerindeki her ağ arabirimi için bir IP adresi ayırır. Adres havuzundan statik IP adresi her VM örneği için ilgili ağ tahsis edilir.

- İkincil sitedeki IP adresi havuzu, aynı kaynak sitede ise Site Recovery çoğaltma VM'de aynı IP adresini (kaynak VM) ayırır. IP adresi VMM'de ayrılmış, ancak Hyper-V ana bilgisayarda yük devretme IP adresi olarak ayarlanmamış. Bir Hyper-v konağı yük devretme IP adresi yalnızca yük devretme önce ayarlanır.
- Aynı IP adresi kullanılabilir değilse, Site Recovery başka bir kullanılabilir IP adresi havuzundan ayırır.
- Sanal makineleri DHCP kullanıyorsanız, Site Recovery IP adreslerini yönetmek değil. Kaynak site ile aynı aralığından ikincil sitede DHCP sunucu adresleri tahsis edebilirsiniz denetlemeniz gerekir.

### <a name="validate-the-ip-address"></a>IP adresini doğrulayın

Bir sanal makine için koruma etkinleştirildikten sonra VM'ye atanan adresi doğrulamak için örnek komut dosyası kullanabilirsiniz. Bu IP adresi yük devretme IP adresi olarak ayarlayın ve yük devretme sırasında VM'ye atanan:

    ```
    $vm = Get-SCVirtualMachine -Name <VM_NAME>
    $na = $vm[0].VirtualNetworkAdapters>
    $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
    $ip.address 
    ```

## <a name="use-a-different-ip-address"></a>Farklı bir IP adresi kullanın

Bu senaryoda, yük devri VM'ler IP adreslerini değiştirilir. Bu çözüm dezavantajı, gerekli bakım olur.  DNS ve önbellek girişlerini güncelleştirilmesi gerekebilir. Bu şekilde azaltılması gereken kapalı kalma sürelerine neden olabilir:

- Düşük TTL değerleri için intranet uygulamalarını kullanın.
- Aşağıdaki komut dosyasını Site Recovery kurtarma planında, DNS sunucusu için bir zamanında güncelleştirme kullanın. Dinamik DNS kaydını kullanırsanız, komut dosyası gerekmez.

    ```
    param(
    string]$Zone,
    [string]$name,
    [string]$IP
    )
    $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
    $newrecord = $record.clone()
    $newrecord.RecordData[0].IPv4Address  =  $IP
    Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord
    ```
    
### <a name="example"></a>Örnek 

Bu örnekte, farklı IP adreslerini birincil ve ikincil siteler arasında sahibiz ve site birincil veya kurtarma üzerinde barındırılan hangi uygulamaların erişilebilen üçüncü bir site yok.

- Yük devretme önce birincil sitede barındırılan alt 192.168.1.0/24 uygulamalardır.
- Yük devretme sonrasında, uygulamalar ikincil sitedeki alt 172.16.1.0/24 yapılandırılır.
- Tüm üç siteleri birbirine erişebilir.
- Yük devretme sonrasında, uygulamalar kurtarma alt ağdaki geri yüklenir.
- Bu senaryoda tüm alt ağ vermesine gerek yoktur ve hiçbir değişiklik VPN veya ağ yolları yapılandırmak için gereklidir. Yük devretme ve bazı DNS güncelleştirmeleri uygulamaları erişilebilir kaldığından emin olun.
- DNS dinamik güncelleştirmelere izin verecek şekilde yapılandırılmışsa, sanal makineleri kendilerini yük devretme sonrasında başlattığınızda yeni IP adresini kullanarak kaydeder.

**Önce yük devretme**

![Yük devretme önce - farklı bir IP adresi](./media/hyper-v-vmm-networking/network-design10.png)

**Yük devretme sonrasında**

![Yük devretme sonrasında - farklı bir IP adresi](./media/hyper-v-vmm-networking/network-design11.png)


## <a name="next-steps"></a>Sonraki adımlar

[Yük devretme gerçekleştirme](hyper-v-vmm-failover-failback.md)

