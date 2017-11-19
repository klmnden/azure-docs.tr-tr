---
title: "IP adresi Azure Site Recovery ile ikincil siteye yük devretme sonrasında bağlanmak için ayarlama | Microsoft Docs"
description: "Azure Site Recovery ile ikincil siteye yük devretme sonrasında Vm'lere bağlanması için IP adresleme yukarı ayarlanacağını açıklar."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: prateek9us
editor: 
ms.assetid: 67d73590-185c-49b2-a097-597bf54747a9
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: pratshar
ms.openlocfilehash: 6baeda08b1c41cc024a02f51ca27be2829c46962
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="set-up-ip-addressing-to-connect-after-failover-to-a-secondary-site"></a>IP adresi ikincil siteye yük devretme sonrasında bağlanmak için ayarlama

Dağıtım önkoşulları gözden geçirdikten sonra Hyper-V sanal makineleri (VM'ler) çoğaltma kullanarak bir ikincil site için System Center Virtual Machine Manager (VMM) bulutlarında yönetilen ağ planlamak için bu makaleyi okuyun [Azure Site Recovery](site-recovery-overview.md) Azure portalında. 

Bu makaleyi okuduktan sonra yapmak istediğiniz tüm yorumları makalenin alt kısmında veya [Azure Kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)'nda paylaşabilirsiniz.

## <a name="overview"></a>Genel Bakış

Çoğaltma ve yük devretme stratejinizi planlarken, anahtar sorulardan biri çoğaltmaya yük devretme sonrasında bağlanmasıdır. Birkaç seçeneğiniz vardır: 

- **Farklı bir IP adresi kullanmak**: çoğaltılan VM için farklı bir IP adresi kullanmayı seçebilirsiniz. Bu senaryoda VM yük devretme sonrasında yeni bir IP adresi alır ve DNS güncelleştirme gereklidir.
- **Aynı IP adresini korumak**: aynı IP adresi VM çoğaltması için kullanmak isteyebilirsiniz. Tutma aynı IP adreslerini basitleştirir kurtarma azaltarak ağ ile ilgili sorunları yük devretme sonrasında. 

## <a name="retaining-ip-addresses"></a>IP adreslerini korur

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




