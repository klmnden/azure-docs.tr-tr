---
title: "Hyper-V çoğaltma Azure Site Recovery ile ikincil VMM sitesi için ağ planı | Microsoft Docs"
description: "Bu makalede, Azure Site Recovery ile ikincil System Center VMM siteye Hyper-V sanal makineleri çoğaltırken ağ planlama açıklanır."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 095f2d73-994e-4fc0-96f2-e03a7d72e492
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: raynew
ms.openlocfilehash: a1f3f6e6cba074647195e2b0cbcdc7b4f3dec475
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="step-3-plan-networking-for-hyper-v-vm-replication-to-a-secondary-vmm-site"></a>3. adım: ağ bağlantısı Hyper-V VM çoğaltma ikincil VMM sitesi için planlama

Dağıtım önkoşulları gözden geçirdikten sonra Hyper-V sanal makineleri (VM'ler) çoğaltma kullanarak bir ikincil site için System Center Virtual Machine Manager (VMM) bulutlarında yönetilen ağ planlamak için bu makaleyi okuyun [Azure Site Recovery](site-recovery-overview.md) Azure portalında. 

Bu makaleyi okuduktan sonra yapmak istediğiniz tüm yorumları makalenin alt kısmında veya [Azure Kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)'nda paylaşabilirsiniz.

## <a name="network-mapping-overview"></a>Ağ eşlemesi genel bakış

Ağ eşlemesi, Hyper-V Vm'lerini (VMM yönetilen) çoğaltırken ikincil veri merkezine için kullanılır. Bir kaynak VMM sunucusunda VM ağları ve bir hedef VMM sunucusunda VM ağları arasında ağ eşlemesi eşler. Eşleme şunları yapar:

- **Ağ bağlantısı**— VM'ler yük devretme sonrasında uygun ağlara bağlanır. Çoğaltma VM kaynak ağa eşlenmiş hedef ağa bağlanır.
- **En iyi yerleştirme**— çoğalma VM'ler Hyper-V ana bilgisayar sunucuları üzerinde en iyi şekilde yerleştirir. Çoğaltma sanal makineleri, eşlenen VM ağlarına erişebilen Konaklara yerleştirilir.
- **Ağ eşleme**— ağ eşlemesini yapılandırmazsanız, çoğaltma sanal makineleri herhangi bir VM ağına yük devretme sonrasında bağlanmayacaktır.


### <a name="example"></a>Örnek

Burada, bu mekanizma göstermek için bir örnek verilmiştir. New York ve Şikago iki konumda bulunduğu bir kuruluşta atalım.

**Konum** | **VMM sunucusu** | **VM ağları** | **Eşlenen**
---|---|---|---
New York | VMM NewYork| VMNetwork1 NewYork | VMNetwork1 Şikago'eşlenmiş
 |  | VMNetwork2 NewYork | Eşlenmedi
Chicago | VMM Chicago| VMNetwork1 Chicago | VMNetwork1-NewYork eşlenmiş
 | | VMNetwork1 Chicago | Eşlenmedi

Bu örnekte:

- Bir çoğaltma sanal makinesi için VMNetwork1-NewYork bağlı herhangi bir sanal makine oluşturulduğunda, VMNetwork1 Şikago'bağlanır.
- Bir çoğaltma sanal makinesi VMNetwork2 NewYork veya VMNetwork2 Chicago oluşturulduğunda, herhangi bir ağa bağlı.

İşte nasıl VMM Bulutları bizim örnek kuruluş ve bulutlarıyla ilişkili mantıksal ağlar olarak ayarlanır.

#### <a name="cloud-protection-settings"></a>Bulut koruma ayarlarını

**Korumalı bulut** | **Bulut koruma** | **Mantıksal ağ (New York)**  
---|---|---
GoldCloud1 | GoldCloud2 |
SilverCloud1| SilverCloud2 |
GoldCloud2 | <p>NA</p><p></p> | <p>LogicalNetwork1 NewYork</p><p>LogicalNetwork1 Chicago</p>
SilverCloud2 | <p>NA</p><p></p> | <p>LogicalNetwork1 NewYork</p><p>LogicalNetwork1 Chicago</p>

#### <a name="logical-and-vm-network-settings"></a>Mantıksal ve VM ağ ayarları

**Konum** | **Mantıksal ağ** | **İlişkili bir VM ağı**
---|---|---
New York | LogicalNetwork1 NewYork | VMNetwork1 NewYork
Chicago | LogicalNetwork1 Chicago | VMNetwork1 Chicago
 | LogicalNetwork2Chicago | VMNetwork2 Chicago

#### <a name="target-network-settings"></a>Hedef ağ ayarları

Hedef VM ağ seçeneğini belirlediğinizde bu ayarları temel alarak, aşağıdaki tabloda kullanılabilir seçenekler gösterilmektedir.

**Seç** | **Korumalı bulut** | **Bulut koruma** | **Hedef ağ yok**
---|---|---|---
VMNetwork1 Chicago | SilverCloud1 | SilverCloud2 | Kullanılabilir
 | GoldCloud1 | GoldCloud2 | Kullanılabilir
VMNetwork2 Chicago | SilverCloud1 | SilverCloud2 | Kullanılamıyor
 | GoldCloud1 | GoldCloud2 | Kullanılabilir


Hedef ağın birden çok alt ağı varsa ve bu alt ağlardan biri kaynak sanal makinenin bulunduğu alt ağ ile aynı ada sahip, ardından çoğaltma sanal makinesi yük devretme işleminden sonra hedef alt ağa bağlanır. Eşleşen ada sahip bir hedef alt ağ yoksa sanal makine ağdaki ilk alt ağa bağlanır.


#### <a name="failback-behavior"></a>Yeniden çalışma davranışı

Yeniden çalışma (çoğaltmayı tersine çevirme) söz konusu olduğunda neler görmek için VMNetwork1 NewYork VMNetwork1-Chicago, aşağıdaki ayarlarla eşleştiğinden emin varsayalım.


**Sanal makine** | **VM ağına bağlı**
---|---
VM1 | VMNetwork1 ağ
VM2 (VM1 çoğaltma) | VMNetwork1 Chicago

Şimdi bu ayarlarla olası senaryolar birkaç içinde neler gözden geçirin.

**Senaryo** | **Sonucu**
---|---
Yük devretme işleminden sonra VM-2 Ağ özelliklerinde değişiklik. | VM 1 kaynak ağına bağlı kalır.
VM-2 ağ özellikleri yük devretme işleminden sonra değiştirilir ve bağlantısı kesilir. | VM 1 kesilir.
VM-2 ağ özellikleri yük devretme işleminden sonra değiştirilir ve VMNetwork2 Şikago'bağlanır. | VMNetwork2 Chicago eşlenmediği olduysa, VM-1 kesilecektir.
Ağ eşlemesi VMNetwork1 Chicago değiştirilir. | VM-1, şimdi VMNetwork1 Şikago'eşlenen ağa bağlanır.



## <a name="prepare-for-network-mapping"></a>Ağ eşlemesi için hazırlanma

1. Kaynak ve hedef VMM sunucularında kaynak ve hedef bulutlarıyla ilişkili bir mantıksal ağ olmalıdır. 
2. Kaynak ve hedef sunucuları, bir VM ağı mantıksal ağa bağlı olmalıdır.
3. Kaynak konumun Hyper-V ana bilgisayarda sanal makineleri kaynak VM ağına bağlantılı olması gerekir. Yalnızca tek bir VMM sunucusu kullanıyorsanız, aynı sunucu üzerinde VM ağları arasındaki eşlemeyi yapılandırabilirsiniz.

Site Recovery dağıtımı sırasında ağ eşlemesini ayarladığınızda şunlar olur:

- Ağ eşlemesi ayarlamanız ve bir hedef VM ağı seçtiğinizde, kaynak VM ağı kullanan VMM kaynak Bulutları, hedef bulut kullanılabilir hedef VM ağlarında birlikte görüntülenir.
- - Eşleme doğru bir şekilde yapılandırıldığından ve çoğaltma etkin olduğunda, bir kaynak VM kendi kaynak VM ağına bağlı olması ve onun çoğaltma hedef konumda, eşlenen VM ağına bağlı.
- Hedef ağın birden çok alt ağı varsa ve bu alt ağlardan biri kaynak sanal makinenin bulunduğu alt ağ ile aynı ada sahip, ardından çoğaltma sanal makinesi yük devretme işleminden sonra hedef alt ağa bağlanır. Eşleşen ada sahip bir hedef alt ağ varsa, VM ağındaki ilk alt ağa bağlanır.

## <a name="connect-to-vms-after-failover"></a>VM'ler için yük devretme sonrasında Bağlan

Çoğaltma ve yük devretme stratejinizi planlarken, anahtar sorulardan biri çoğaltmaya yük devretme sonrasında bağlanmasıdır. Birkaç seçeneğiniz vardır: 

- **Farklı bir IP adresi kullanmak**: çoğaltılan VM için farklı bir IP adresi kullanmayı seçebilirsiniz. Bu senaryoda VM yük devretme sonrasında yeni bir IP adresi alır ve DNS güncelleştirme gereklidir.
- **Aynı IP adresini korumak**: aynı IP adresi VM çoğaltması için kullanmak isteyebilirsiniz. Tutma aynı IP adreslerini basitleştirir kurtarma azaltarak ağ ile ilgili sorunları yük devretme sonrasında. 

## <a name="retain-ip-addresses"></a>IP adreslerini korur

IP adreslerini birincil siteden ikincil siteye yük devretme sonrasında korumak istiyorsanız, tam alt ağ yük devretme işlemi gerçekleştirin ve IP adreslerini yeni konumunu belirtmek için bir yol güncelleştirmek veya alternatif Uzatılan bir alt birincil ve kurtarma siteler arasında dağıtabilirsiniz.

### <a name="stretched-subnet"></a>Esnetilen alt ağ

Esnetilen bir alt ağında alt ağ aynı anda hem birincil ve ikincil sitede kullanılabilir. Bir sunucu ve kendi (Katman 3) IP yapılandırmasını ikincil siteye taşırsanız, ağ trafiğini yeni konuma otomatik olarak yönlendirilecek. 

Bir katman 2 (veri bağlantı katmanı) açısından Uzatılan bir VLAN yönetebilirsiniz ağ ekipmanları gerekir. Buna ek olarak, VLAN yayarak olası hata etki alanı temelde tek hata noktası haline hem sitelere genişletir. Bu bir olası olmakla birlikte, bir yayın storm kullanmaya ve yalıtılmış olamaz meydana gelebilir. 


### <a name="subnet-failover"></a>Alt ağ yük devretme

Gerçekte uzatma olmadan esnetilen alt avantajlarından yararlanabilmek için bir alt ağ yük devretme çalıştırabilirsiniz. Bu çözümde, bir alt ağ kullanılabilir kaynak veya hedef sitede, ancak her ikisi de aynı anda. IP adres alanı bir yük devretme durumunda korumak için program aracılığıyla alt ağların bir siteden diğerine taşımak yönlendirici altyapı düzenleyebilirsiniz. Yük devretme oluştuğunda sonra alt ağlar ile ilişkili VM'ler taşıyabilir. Bir arıza olması durumunda olan asıl sakıncası, tüm alt ağı taşımanız gerekir.

### <a name="example"></a>Örnek

Tam alt ağ yük devretme bir örneği burada verilmiştir. Birincil site alt 192.168.1.0/24 içinde çalışan uygulamalar vardır. Yük devretme, bu alt ağdaki tüm sanal makineleri ikincil siteye yük devredildi ve IP adreslerini korur. Yollar için alt ağ 192.168.1.0/24 ait tüm VM sanal makineler artık ikincil siteye taşınmış olgu yansıtacak şekilde değiştirilmesi gerekir.

Aşağıdaki grafik önce ve yük devretme sonrasında alt ağları göster:

- Yük devretme önce alt 192.168.0.1/24 kaynak sitede yük devretme işleminden sonra ikincil sitede etkin hale etkindir.
- Birincil site kurtarma sitesi, üçüncü sitesi ve birincil site ve üçüncü site ve kurtarma sitesi arasındaki yolları uygun şekilde değiştirilmesi gerekir.

**Önce yük devretme**

![Önce yük devretme](./media/vmm-to-vmm-walkthrough-network/network-design2.png)

**Yük devretme sonrasında**

![Yük devretme sonrasında](./media/vmm-to-vmm-walkthrough-network/network-design3.png)

Yük devretme sonrasında, şunlar olur:

- Site Recovery ilgili ağındaki her VMM örneği için statik IP adres havuzundan VM üzerindeki her ağ arabirimi için bir IP adresi ayırır.
- İkincil sitedeki IP adresi havuzu, aynı kaynak sitede ise Site Recovery aynı IP adresine (kaynak VM) VM çoğaltma ayırır. IP adresi VMM'de ayrılmış, ancak Hyper-V ana bilgisayarda yük devretme IP adresi olarak ayarlanmamış. Bir Hyper-v konağı yük devretme IP adresi yalnızca yük devretme önce ayarlanır.
- Aynı IP adresi kullanılabilir değilse, Site Recovery başka bir kullanılabilir IP adresi havuzundan ayırır.
- Sanal makineleri DHCP kullanıyorsanız, Site Recovery IP adreslerini yönetmek değil. Kaynak site ile aynı aralığından ikincil sitede DHCP sunucusu adresi ayırabilirsiniz denetlemeniz gerekir.

### <a name="validate-the-ip-address"></a>IP adresini doğrulayın

Bir sanal makine için koruma etkinleştirildikten sonra VM'ye atanan adresi doğrulamak için örnek komut dosyası kullanabilirsiniz. Aynı IP adresi yük devretme IP adresi olarak ayarlayın ve yük devretme sırasında VM'ye atanan:

    ```
    $vm = Get-SCVirtualMachine -Name <VM_NAME>
    $na = $vm[0].VirtualNetworkAdapters>
    $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
    $ip.address 
    ```

## <a name="changing-ip-addresses"></a>IP adreslerinin değiştirilmesi

Bu senaryoda, yük devri VM'ler IP adreslerini değiştirilir. Bu çözüm dezavantajı, gerekli bakım olur. Genellikle, çoğaltma sanal makineleri başlattıktan sonra DNS güncelleştirilir. DNS girdilerini değiştirilmesi veya gerekebilir fluster thenetwork ve önbelleğe alınan girdileri güncelleştirildi. Bu, kapalı kalma sürelerine neden olabilir. Kapalı kalma süresi gibi azaltılabilir:

- Düşük TTL değerleri için intranet uygulamalarını kullanın.
- Aşağıdaki komut dosyasını bir Site Recovery kurtarma planında zamanında güncelleştirme sağlamak için DNS sunucusuna güncelleştirmek için kullanın. Dinamik DNS kaydını kullanırsanız, komut dosyası gerekmez.

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

Birincil ve kurtarma siteler arasında farklı IP adreslerini kullanmak planlıyorsanız bir senaryo bakalım. Bu örnekte, farklı IP adreslerini birincil ve ikincil siteler arasında sahibiz ve vardır; s üçüncü bir site birincil veya kurtarma sitesinde barındırılan hangi uygulamalardan erişilebilir.

- Yük devretme önce uygulamalar barındırılan alt 192.168.1.0/24 birincil sitede ve alt ağ 172.16.1.0/24 ikincil sitedeki bir yük devretme sonrasında olması için yapılandırılır.
- Tüm üç siteleri birbirine erişebilmesi için VPN bağlantıları/ağ yolları uygun şekilde yapılandırılmış.
- Yük devretme sonrasında, uygulamalar kurtarma alt ağdaki geri yüklenir. Bu senaryoda tüm alt ağ vermesine gerek yoktur ve hiçbir değişiklik VPN veya ağ yolları yapılandırmak için gereklidir. Yük devretme ve bazı DNS güncelleştirmeleri uygulamaları erişilebilir kaldığından emin olun.
- DNS dinamik güncelleştirmelere izin verecek şekilde yapılandırılmışsa, sanal makineleri kendilerini yük devretme sonrasında başlattığınızda yeni IP adresini kullanarak kaydeder.

**Önce yük devretme**

![Farklı bir IP - yük devretme önce](./media/vmm-to-vmm-walkthrough-network/network-design10.png)

**Yük devretme sonrasında**

![Farklı bir IP - yük devretme sonrasında](./media/vmm-to-vmm-walkthrough-network/network-design11.png)



## <a name="next-steps"></a>Sonraki adımlar

Git [4. adım: VMM ve Hyper-V hazırlama](vmm-to-vmm-walkthrough-vmm-hyper-v.md).


