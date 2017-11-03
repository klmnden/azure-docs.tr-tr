---
title: "Azure Site Recovery (VMM VMM) yük devretme testi | Microsoft Docs"
description: "Azure Site Recovery, çoğaltma, yük devretme ve sanal makinelerin ve fiziksel sunucuları kurtarma düzenler. Azure veya ikincil veri merkezine yük devretme hakkında bilgi edinin."
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: pratshar
ms.openlocfilehash: afc4790d5714ce7145c8f4291a05acc2e9882a9b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="test-failover-vmm-to-vmm-in-site-recovery"></a>Site Recovery (VMM VMM) bir yük devretmeyi sınama


Bu makalede bilgiler sağlanmaktadır ve yük devretme testi veya bir olağanüstü durum kurtarma (DR) ayrıntıya sanal makineleri (VM'ler) ve fiziksel sunucuların, yapmaya yönelik yönergeler Azure Site Recovery ile korunur. System Center Virtual Machine Manager VMM tarafından yönetilen şirket içi site kurtarma sitesi olarak kullanacaksınız.

Çoğaltma stratejinizi doğrulamak veya herhangi bir veri kaybı veya kapalı kalma süresi olmadan DR detayı gerçekleştirmek için yük devretme testi çalıştırın. Yük devretme testi devam eden çoğaltmayı veya üretim ortamınıza etkilerini sahip değil. Bir sanal makine ya da bir çalıştırabilirsiniz [kurtarma planı](site-recovery-create-recovery-plans.md). Yük devretme testi tetiklendiğinde test sanal makineleri bağlanacağı ağ belirtmeniz gerekir. Yük devretme sınaması ilerlemesini takip edebilirsiniz **işleri** sayfası.  

Tüm yorumlarınızı ve sorularınızı varsa, bunları bu makalenin veya üzerinde altındaki sonrası [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="prepare-the-infrastructure-for-test-failover"></a>Yük devretme sınaması için altyapıyı hazırlama
Varolan bir ağ kullanarak yük devretme testi çalıştırmak istiyorsanız, Active Directory, DHCP ve DNS, ağ hazırlayın.

VM ağları otomatik olarak oluşturma seçeneğini kullanarak bir yük devretme testi çalıştırmak isterseniz, Kurtarma planında yük devretme sınaması için kullanacaksanız Grup 1 önce el ile bir adım ekleyin. Daha sonra Yük devretme testini çalıştırmadan önce otomatik olarak oluşturulan ağ altyapısı kaynakları ekleyin.

### <a name="things-to-note"></a>Dikkat edilecek noktalar
İkincil bir siteye çoğaltma yapıyorsanız, çoğaltma makinesi kullanan ağ türünü yük devretme sınaması için kullanılan mantıksal ağ türünde eşleşmesi gerekmez, ancak bazı birleşimleri çalışmayabilir. Çoğaltma, DHCP ve VLAN tabanlı yalıtım kullanıyorsa, VM ağı çoğaltma için bir statik IP adresi havuzu gerekmez. Herhangi bir adres havuzu kullanılabilir olmadığından, böylece yük devretme sınaması için Windows ağ sanallaştırma kullanarak çalışmaz. 

Ayrıca, yük devretme sınaması çoğaltma ağ yalıtımı kullanır ve Windows ağ sanallaştırma test ağı kullanıyorsa çalışmaz. Bu durum, yalıtımsız ağa bir Windows ağ sanallaştırma ağ oluşturmak için gereken alt ağı yok çünkü.

Nasıl yük devretme VM ağı VMM konsolunda nasıl yapılandırıldığına bağlıdır sonra çoğaltma sanal makineleri eşlenen VM ağlarına bağlanır.

#### <a name="vm-network-configured-with-no-isolation-or-vlan-isolation"></a>Hiçbir yalıtım veya VLAN yalıtımı ile yapılandırılmış VM ağı
VM ağı için DHCP tanımlanmışsa, çoğaltma sanal makinesi ilişkili mantıksal ağdaki ağ sitesi için belirtilen ayarları aracılığıyla VLAN ID bağlı. Sanal makinenin IP adresini kullanılabilir DHCP sunucusundan alır. 

Hedef VM ağı için statik bir IP adresi havuzu tanımlamak gerekmez. VM ağı için statik bir IP adresi havuzu kullandıysanız, çoğaltma sanal makinesi ilişkili mantıksal ağdaki ağ sitesi için belirtilen ayarları aracılığıyla VLAN ID bağlı.

Sanal makine, VM ağ için tanımlı havuzundan IP adresini alır. Bir statik IP adresi havuzu hedef VM ağı üzerinde tanımlı değil, IP adresi ayırma başarısız olur. Koruma ve kurtarma için kullanacağınız hem kaynak hem de hedef VMM sunucularında IP adresi havuzu oluşturun.

#### <a name="vm-network-with-windows-network-virtualization"></a>Windows ağ sanallaştırma ile VM ağı
Bir VM ağı Windows ağ sanallaştırma ile yapılandırılmışsa, DHCP veya bir statik IP adresi havuzu kullanmak üzere kaynak VM ağına olup yapılandırılmış bağımsız olarak hedef VM ağı için statik bir havuzu tanımlamanız gerekir. 

DHCP tanımlarsanız, hedef VMM sunucusunda bir DHCP sunucusu gibi davranan ve hedef VM ağı için tanımlanan havuzundan bir IP adresi sağlar. Kaynak sunucu için bir statik IP adresi havuzu kullanımı tanımlanmışsa, hedef VMM Sunucu havuzundan bir IP adresi ayırır. Her iki durumda da, bir statik IP adresi havuzu tanımlanmazsa, IP adresi ayırma başarısız olur.


### <a name="prepare-dhcp"></a>DHCP hazırlama
Sanal makineler katılan yük devretme testi DHCP kullanmak, bir test DHCP sunucusu yük devretme testi amacıyla yalıtılmış ağda oluşturun.

### <a name="prepare-active-directory"></a>Active Directory hazırlama
Bir yük devretme sınaması için sınama uygulama çalıştırmak için test ortamınızda üretim Active Directory ortamında bir kopyasını gerekir. Daha fazla bilgi için gözden [test Active Directory için yük devretme konuları](site-recovery-active-directory.md#test-failover-considerations).

### <a name="prepare-dns"></a>DNS hazırlama
Bir DNS sunucusu yük devretme sınaması için aşağıdaki gibi hazırlayın:

* **DHCP**: sanal makineler DHCP kullanıyorsanız, IP adresi DNS testinin test DHCP sunucusunda güncelleştirilmesi gerekir. Bir ağ türü Windows ağ sanallaştırma kullanıyorsanız, VMM sunucusu DHCP sunucu gibi davranır. Bu nedenle, DNS IP adresi test yük devretme ağında güncelleştirilmesi gerekir. Bu durumda, sanal makineleri kendilerini ilgili DNS sunucusuna kaydedin.
* **Statik adresi**: sanal makinelere statik IP adresi kullanırsanız, test yük devretme ağı testinde test DNS sunucusunun IP adresini güncelleştirilmelidir. DNS test sanal makineleri IP adresiyle güncelleştirmeniz gerekebilir. Bu amaç için aşağıdaki örnek komut dosyası kullanabilirsiniz:

        Param(
        [string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord



## <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma
Bu yordam, bir yük devretme sınaması için bir kurtarma planı Çalıştır açıklar. Alternatif olarak, tek bir sanal makine için yük devretme üzerinde çalışabilir **sanal makineleri** sekmesi.

![Test yük devretme dikey penceresi](./media/site-recovery-test-failover-vmm-to-vmm/TestFailover.png)

1. Seçin **kurtarma planları** > *recoveryplan_name*. Tıklatın **yük devretme** > **yük devretme testi**.
1. Üzerinde **sınama yük devretmesi** dikey penceresinde, test yük devretme sonrasında nasıl sanal makine ağlarına bağlanmalıdır belirtin. Daha fazla bilgi için bkz: [ağ seçenekleri](#network-options-in-site-recovery).
1. Yük devretme işleminin ilerleyişini izlemek **işleri** sekmesi.
1. Yük devretme işlemi tamamlandıktan sonra sanal makinelerin başarılı bir şekilde başlatıldığını doğrulayın.
1. İşiniz bittiğinde tıklatın **temizleme yük devretme testi** kurtarma planı üzerinde. İçinde **notları**, kaydetme ve yük devretme testiyle ilişkili gözlemlerinizi kaydetmek. Bu adım, yük devretme testi sırasında oluşturulan ağlar ve sanal makinelere siler.


## <a name="network-options-in-site-recovery"></a>Site Recovery içindeki ağ seçenekleri

Yük devretme testi çalıştırdığınızda, test yineleme makineler için ağ ayarlarını seçin istenir. Birden fazla seçeneği vardır.  

| **Test yük devretme seçeneği** | **Açıklama** | **Yük devretme denetimi** | **Ayrıntılar** |
| --- | --- | --- | --- |
| **--Ağ olmadan ikincil bir VMM sitesi yük** |Bir VM ağı seçmeyin. |Yük devretme test makinelerini oluşturduğunuz denetler.<br/><br/>Test sanal makinesi, çoğaltma sanal makinesi bulunduğu ana bilgisayarda oluşturulur. Çoğaltma sanal makinesi bulunduğu bulut eklenmez. |<p>Başarısız üzerinden makine herhangi bir ağa bağlı değil.<br/><br/>Oluşturulduktan sonra makinenin bir VM ağına bağlanabilir. |
| **İkincil VMM siteye--ağ ile yük devredin** |Var olan bir VM ağı seçin. |Sanal makineler oluşturulan yük devretme denetler. |Test sanal makinesi, çoğaltma sanal makinesi bulunduğu ana bilgisayarda oluşturulur. Çoğaltma sanal makinesi bulunduğu bulut eklenmez.<br/><br/>Üretim ağınızdan yalıtılmış olan bir VM ağı oluşturun.<br/><br/>VLAN tabanlı ağ kullanıyorsanız (üretimde kullanılmaz) ayrı bir mantıksal ağ oluşturma VMM'de bu amaç için öneririz. Bu mantıksal ağ, yük devretme sınaması işlemlerini için VM ağları oluşturmak için kullanılır.<br/><br/>Mantıksal ağ sunucularının ağ bağdaştırıcıları, sanal makineleri barındıran tüm Hyper-V en az biriyle ilişkili olmalıdır.<br/><br/>VLAN Mantıksal ağlar için mantıksal ağa eklemek ağ siteleri yalıtılmış olması gerekir.<br/><br/>Windows ağ sanallaştırma tabanlı bir mantıksal ağ kullanıyorsanız, Azure Site Recovery yalıtılmış VM ağları otomatik olarak oluşturur. |
| **Yük devri ikincil VMM sitesi--bir ağ oluşturma** |Bir geçici test ağı belirttiğiniz ayarına göre otomatik olarak oluşturulan **mantıksal ağ** ve onun ilişkili ağ siteleri. |Sanal makineler oluşturulan yük devretme denetler. |Kurtarma planı birden fazla VM ağı kullanıyorsa bu seçeneği kullanın. Windows ağ Sanallaştırması ağlarına kullanıyorsanız, bu seçenek çoğaltma sanal makinesi ağında VM ağları (alt ağlar ve IP adresi havuzları) aynı ayarlarla otomatik olarak oluşturabilirsiniz. Test yük devretme işlemi tamamlandıktan sonra bu VM ağları otomatik olarak temizlenir.</p><p>Test sanal makinesi, çoğaltma sanal makinesi bulunduğu ana bilgisayarda oluşturulur. Çoğaltma sanal makinesi bulunduğu bulut eklenmez. |

> [!TIP]
> Bir sanal makine için yük devretme testi sırasında verilen IP adresi, sanal makine alacağı aynı IP adresidir (IP adresi test yük devretme ağda kullanılabilir olduğunu pek fazla) planlanmış veya planlanmamış bir yük devretme için. Aynı IP adresini test yük devretme ağda kullanılabilir değilse, sanal makine test yük devretme ağı testinde kullanılabilirse başka bir IP adresi alır.
>
>


## <a name="test-failover-to-a-production-network-on-a-recovery-site"></a>Kurtarma sitesinde bir üretim ağı için yük devretme sınaması
Yük devretme testi yaparken, sağladığınız üretim ağınızdan kurtarma sitesini farklı bir ağ seçmenizi öneririz [ağ eşlemesi](https://docs.microsoft.com/azure/site-recovery/site-recovery-network-mapping). Ancak gerçekten başarısız üzerinden sanal makinede uçtan uca ağ bağlantısını doğrulamak istiyorsanız, aşağıdaki noktalara dikkat edin:

* Test yük devretme yapılırken birincil sanal makinenin kapatıldığını emin olun. Bunu yapmazsanız, aynı kimliğe sahip iki sanal makineyi aynı ağda birlikte aynı anda çalıştırırsınız. Bu durum, istenmeyen sonuçlara neden olabilir.
* Test yük devretme sanal makinelerini Temizle test yük devretme sanal makinelere yaptığınız tüm değişiklikler kaybolur. Bu değişiklikleri birincil sanal makine çoğaltılmaz.
* Bu şekilde test yapmanın üretim uygulamanız için kesintiye neden neden olmaktadır. DR detayı devam ederken uygulama kullanmamayı uygulama kullanıcılarının isteyin.  


## <a name="next-steps"></a>Sonraki adımlar
Yük devretme testi başarıyla çalıştırdıktan sonra deneyebilirsiniz bir [yük devretme](site-recovery-failover.md).
