---
title: IP adresi Azure Site Recovery ile yük devretme sonrasında bir ikincil şirket içi siteye bağlanmak için ayarlama | Microsoft Docs
description: Olağanüstü durum kurtarma ve Azure Site Recovery ile yük devretme sonrasında bir ikincil şirket içi sitedeki vm'lere bağlanmak için IP adresini ayarlama işlemi açıklanmaktadır.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: raynew
ms.openlocfilehash: 8e4dca61016adce209bdce356ea4280fee525c05
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66397958"
---
# <a name="set-up-ip-addressing-to-connect-to-a-secondary-on-premises-site-after-failover"></a>IP adresi yük devretmenin ardından ikincil şirket içi siteye bağlanmak için ayarlama

İkincil bir siteye System Center Virtual Machine Manager (VMM) bulutlarındaki Hyper-V sanal makinelerini yük devretme sonra olması gereken çoğalma VM'ler için bağlanın. Bu makalede bunu yapmamıza yardımcı olur. 

## <a name="connection-options"></a>Bağlantı seçenekleri

Yük devretmeden sonra birkaç çoğaltma VM'ler için IP adresleme işlemek için yolu vardır: 

- **Yük devretme sonrasında aynı IP adresini korumak**: Bu senaryoda, çoğaltılan VM birincil VM aynı IP adresine sahiptir. Bu kolaylaştırır ilgili ağ sorunları yük devretme işleminden sonra ancak bazı altyapı çalışmasını gerektirir.
- **Yük devretmeden sonra farklı bir IP adresi kullanmak**: Bu senaryoda, sanal makine yük devretme işleminden sonra yeni bir IP adresi alır. 
 

## <a name="retain-the-ip-address"></a>IP adresini koruma

IP adreslerini birincil siteden ikincil siteye yük devretmenin ardından korumak istiyorsanız, şunları yapabilirsiniz:

- Esnetilmiş bir alt birincil ve ikincil siteler arasında dağıtın.
- Tam bir alt ağ yük devretme birincil düğümden ikincil site için gerçekleştirin. IP adreslerini yeni konumunu belirtmek için rotalar güncelleştirmeniz gerekir.


### <a name="deploy-a-stretched-subnet"></a>Esnetilmiş bir alt ağ dağıtma

Esnetilmiş bir yapılandırmada, alt ağ aynı anda hem birincil hem de ikincil sitelerde kullanılabilir. İkincil site için bir makine ve (Katman 3) IP adresi yapılandırmasını taşıdığınızda Uzatılan bir alt ağ içinde ağ otomatik olarak trafik yeni konuma yönlendirir. 

- Bir katman 2 (veri bağlantı katmanı) açısından bakıldığında, esnetilmiş VLAN yönetebilirsiniz ağ ekipmanları gerekir.
- VLAN yayarak olası hata etki alanı her iki sitede genişletir. Bu, bir tek hata noktası haline gelir. Düşük ihtimalle de olsa, böyle bir senaryoda, bir yayın storm gibi bir olay ayırmak mümkün olmayabilir. 


### <a name="fail-over-a-subnet"></a>Bir alt ağ başarısız

Aslında uzatma olmadan esnetilen alt avantajlarından yararlanabilmek için tüm alt devredebilir. Bu çözümde bir alt ağ kullanılabilir kaynak veya hedef sitede, ancak her ikisi de aynı anda.

- Yük devretme durumunda IP adres alanı sağlamak için program aracılığıyla alt ağlar bir konumdan diğerine taşımak yönlendirici altyapı düzenleyebilirsiniz.
- Bir yük devretme işlemi gerçekleştiğinde, alt ağlar ile ilişkili Vm'lerini taşıyın.
- Bu yaklaşımın asıl sakıncası, bir hata olması durumunda olduğundan, tüm alt taşıma gerekir.

#### <a name="example"></a>Örnek

Eksiksiz bir alt ağ yük devretme örneği aşağıda verilmiştir. 

- Önce yük devretme, birincil sitenin alt 192.168.1.0/24 içinde çalışan uygulamalar vardır.
- Yük devretme sırasında tüm bu alt ağdaki sanal makinelerin ikincil siteye yük devretme ve IP adreslerini korur. 
- Tüm siteler arasındaki yolları alt 192.168.1.0/24 tüm sanal makineleri ikincil siteye artık taşınmış olgu yansıtacak şekilde değiştirilmesi gerekir.

Aşağıdaki grafik, önce ve sonra Yük devretme alt ağlar gösterilmektedir.


**Önce yük devretme**

![Önce yük devretme](./media/hyper-v-vmm-networking/network-design2.png)

**Yük devretmeden sonra**

![Yük devretmeden sonra](./media/hyper-v-vmm-networking/network-design3.png)

Site Recovery, yük devretme işleminden sonra VM'deki her ağ arabirimi için IP adresi ayırır. Adresi ilgili ağdaki her bir VM örneği için statik IP adresi havuzundan ayrılır.

- IP adresi havuzu ikincil sitede aynı kaynak sitesinde ise Site Recovery çoğaltma VM'sine aynı IP adresini (kaynak VM) ayırır. IP adresi VMM'de ayrılmış, ancak Hyper-V konağında yük devretme IP adresi olarak ayarlanmamış. Bir Hyper-v konağı yük devretme IP adresi, yalnızca yük devretme önce ayarlanır.
- Site Recovery, aynı IP adresi kullanılabilir değilse, başka bir kullanılabilir IP adresi havuzundan ayırır.
- Site Recovery, VM'lerin DHCP kullanıyorsanız, IP adreslerini yönetmez. Kaynak site olarak aynı aralıktan ikincil sitede DHCP sunucusu adresleri ayırabilirsiniz denetlemek gerekir.

### <a name="validate-the-ip-address"></a>IP adresini doğrula

Bir sanal makine için koruma etkinleştirdikten sonra VM'ye atanmış olan adresi doğrulamak için örnek komut dosyası kullanabilirsiniz. Bu IP adresi, yük devretme IP adresi olarak ayarlayın ve yük devretme sırasında sanal Makineye atanan:

    ```
    $vm = Get-SCVirtualMachine -Name <VM_NAME>
    $na = $vm[0].VirtualNetworkAdapters>
    $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
    $ip.address 
    ```

## <a name="use-a-different-ip-address"></a>Farklı bir IP adresi kullanın

Bu senaryoda, yük devretme sanal IP adreslerini değiştirilir. Bu çözümünün dezavantajı, gerekli bakım olmasıdır.  DNS ve önbellek girdilerinin güncelleştirilmesi gerekebilir. Bu, şu şekilde azaltılması gereken kapalı kalma neden olabilir:

- İntranet uygulamaları için düşük TTL değerleri kullanın.
- Aşağıdaki betiği bir Site Recovery kurtarma planında bir DNS sunucusu zamanında güncelleştirmesi kullanın. Dinamik DNS kaydı kullanırsanız komut dosyası gerekmez.

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

Bu örnekte biz farklı IP adreslerini birincil ve ikincil siteler arasında olması ve site birincil veya kurtarma barındırılan hangi uygulamalardan erişilebilir üçüncü bir site yoktur.

- Yük devretme önce birincil sitede barındırılan alt 192.168.1.0/24 uygulamalardır.
- Yük devretmeden sonra uygulamaları alt 172.16.1.0/24 de ikincil sitede yapılandırılır.
- Üç tüm siteleri birbirine erişebilir.
- Yük devretmeden sonra kurtarma alt ağda uygulamalar geri yüklenir.
- Bu senaryoda tüm alt ağ başarısız olmasına gerek yoktur ve hiçbir değişiklik VPN ya da ağ yolları yapılandırmak için gereklidir. Yük devretme ve bazı DNS güncelleştirmeleri uygulamaların erişilebilir kalmasını sağlayın.
- DNS dinamik güncelleştirmeleri izin verecek şekilde yapılandırılmışsa, Vm'leri kendilerini kullanıcıdan yük devretmeden sonra başlatırken yeni IP adresini kullanarak kaydeder.

**Önce yük devretme**

![Yük devretmeden önce - farklı bir IP adresi](./media/hyper-v-vmm-networking/network-design10.png)

**Yük devretmeden sonra**

![Yük devretmeden sonra - farklı bir IP adresi](./media/hyper-v-vmm-networking/network-design11.png)


## <a name="next-steps"></a>Sonraki adımlar

[Yük devretme çalıştırma](hyper-v-vmm-failover-failback.md)

