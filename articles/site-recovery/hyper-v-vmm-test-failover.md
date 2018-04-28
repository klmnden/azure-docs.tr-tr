---
title: Azure Site RECOVERY'yi kullanarak ikincil bir siteye DR detayı Hyper-V vm'lerinde çalıştırmak | Microsoft Docs
description: DR detayı VMM bulutlarındaki Hyper-V sanal makineleri için Azure Site RECOVERY'yi kullanarak bir ikincil veri merkezine çalıştırmayı öğrenin.
services: site-recovery
author: ponatara
manager: abhemraj
ms.service: site-recovery
ms.topic: article
ms.date: 02/12/2018
ms.author: ponatara
ms.openlocfilehash: c389776f62db5fd04f67ef22822e21fd4aee368f
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="run-a-dr-drill-for-hyper-v-vms-to-a-secondary-site"></a>DR detayı için Hyper-V Vm'lerini ikincil bir siteye çalıştırın.


Bu makalede bir ikincil şirket içi siteye Sistem Merkezi Sanal Makine Yöneticisi V(MM) bulutlarında yönetilen Hyper-V VM'ler için olağanüstü durum kurtarma (DR) ayrıntıya nasıl kullanarak [Azure Site Recovery](site-recovery-overview.md).

Çoğaltma stratejinizi doğrulamak için bir yük devretme testi çalıştırmak ve herhangi bir veri kaybı veya kapalı kalma süresi olmadan DR detayı gerçekleştirin. Yük devretme testi devam eden çoğaltmayı veya üretim ortamınıza etkilerini sahip değil. 

## <a name="how-do-test-failovers-work"></a>Yük devretme iş test nasıl?

Birincil sunucudan ikincil siteye yük devretme testi çalıştırın. Yalnızca bir VM üzerinde başarısız olduğunu denetlemek istiyorsanız, ikincil sitede herhangi bir şey ayarlama olmadan bir yük devretme testi çalıştırabilirsiniz. Uygulama yük devretme works beklendiği gibi doğrulamak istiyorsanız, ağ ve altyapı ikincil konumda ayarlamanız gerekir.
- Tek bir VM'de veya üzerinde bir yük devretme testi çalıştırabilirsiniz bir [kurtarma planı](site-recovery-create-recovery-plans.md).
- Varolan bir ağ ile veya otomatik olarak oluşturulan bir ağ ile yük devretme testi, ağ olmadan çalıştırabilirsiniz. Bu seçenekler hakkında daha fazla ayrıntı, aşağıdaki tabloda verilmiştir.
    - Yük devretme testi ağ olmadan çalıştırabilirsiniz. Bu seçenek, yalnızca bir VM yük devri açabildi ancak herhangi bir ağ yapılandırması doğrulamak göremezsiniz denetlemek istiyorsanız yararlıdır.
    - Yük devretme mevcut bir ağ ile çalıştırın. Bir üretim ağı kullanmayan öneririz.
    - Yük devretme çalıştırın ve Site Recovery otomatik olarak bir test ağı oluşturmak istiyorum. Bu durumda Site Recovery ağ otomatik olarak oluşturur ve test yük devretmesi tamamlandığında yukarı temizleyin.
- Yük devretme sınaması için bir kurtarma noktası seçmeniz gerekir: 
    - **En son işlenen**: Bu seçenek bir VM üzerinden Site Recovery tarafından işlenen en son kurtarma noktası için başarısız olur. İşlenmemiş verileri işlemek için zaman harcanmadığından bu seçenekte düşük bir RTO (Kurtarma Süresi Hedefi) sağlanır.
    - **Son uygulama tutarlı**: VM en son uygulama tutarlı kurtarma noktası için bu seçeneği devreden Site Recovery tarafından işlenir. 
    - **En son**: Bu seçenek ilk Site Recovery hizmetine ona üzerinden başarısız olmadan önce her bir VM için bir kurtarma noktası oluşturmak üzere gönderilen tüm verileri işler. Yük devretme yük devretme tetiklendiğinde Site Recovery çoğaltılan tüm verileri sonra VM oluşturduğundan bu seçenek en düşük RPO (kurtarma noktası hedefi) sağlar.
    - **En son çoklu işlenen VM**: çoklu VM tutarlılığı etkin olan bir veya daha fazla sanal makineleri içeren kurtarma planları için kullanılabilir. En son ortak çoklu VM tutarlı bir kurtarma noktası ayarı etkin olan VM'ler devredin. Diğer Vm'leri üzerinde en son işlenen kurtarma noktası başarısız.
    - **En son çoklu VM uygulamayla tutarlı**: Bu seçenek çoklu VM tutarlılığı etkin olan bir veya daha fazla sanal makineleri içeren kurtarma planları için kullanılabilir. Bir çoğaltma grubunun parçası olan VM'ler üzerinden son ortak çoklu VM uygulamaları tutarlı kurtarma noktası başarısız. Bunların en son uygulamaları tutarlı kurtarma noktasına diğer VM'ler devredin.
    - **Özel**: belirli bir kurtarma noktası için belirli bir VM'de yük devri için bu seçeneği kullanın.



## <a name="prepare-networking"></a>Ağ hazırlama

Yük devretme testi çalıştırdığınızda, test yineleme makineler için ağ ayarlarını seçin tabloda özetlendiği gibi istenir.

**Seçeneği** | **Ayrıntılar** 
--- | --- 
**Yok** | Test VM VM çoğaltma bulunduğu ana bilgisayarda oluşturulur. Buluta eklenen değildir ve herhangi bir ağa bağlı değil.<br/><br/> Oluşturulduktan sonra makinenin bir VM ağına bağlanabilir.
**Varolanı kullanma** | Test VM VM çoğaltma bulunduğu ana bilgisayarda oluşturulur. Buluta eklenen değil.<br/><br/>Üretim ağınızdan yalıtılmış olan bir VM ağı oluşturun.<br/><br/>VLAN tabanlı ağ kullanıyorsanız (üretimde kullanılmaz) ayrı bir mantıksal ağ oluşturma VMM'de bu amaç için öneririz. Bu mantıksal ağ, yük devretme sınaması işlemlerini için VM ağları oluşturmak için kullanılır.<br/><br/>Mantıksal ağ sunucularının ağ bağdaştırıcıları, sanal makineleri barındıran tüm Hyper-V en az biriyle ilişkili olmalıdır.<br/><br/>VLAN Mantıksal ağlar için mantıksal ağa eklemek ağ siteleri yalıtılmış olması gerekir.<br/><br/>Windows ağ sanallaştırma tabanlı bir mantıksal ağ kullanıyorsanız, Azure Site Recovery yalıtılmış VM ağları otomatik olarak oluşturur. 
**Ağ oluşturma** | Bir geçici test ağı belirttiğiniz ayarına göre otomatik olarak oluşturulan **mantıksal ağ** ve onun ilişkili ağ siteleri.<br/><br/> Yük devretme VM'ler oluşturduğunuz denetler. |Bir kurtarma planı birden fazla VM ağı kullanıyorsa bu seçeneği kullanmanız gerekir.<br/><br/> Windows ağ Sanallaştırması ağlarına kullanıyorsanız, bu seçenek çoğaltma sanal makinesi ağında VM ağları (alt ağlar ve IP adresi havuzları) aynı ayarlarla otomatik olarak oluşturabilirsiniz. Test yük devretme işlemi tamamlandıktan sonra bu VM ağları otomatik olarak temizlenir.<br/><br/> Test VM çoğaltma sanal makinesi bulunduğu ana bilgisayarda oluşturulur. Buluta eklenen değil.

### <a name="best-practices"></a>En iyi uygulamalar

- Bir üretim ağı test üretim iş yükleri için kapalı kalma süresi neden olur. Olağanüstü durum kurtarma ayrıntıya devam ederken ilgili uygulamalar kullanmamayı, kullanıcılarınızın isteyin.

- Test ağı yük devretme sınaması için kullanılan VMM mantıksal ağ türü eşleşmesi gerekmez. Ancak, bazı birleşimleri çalışmıyor:

     - Çoğaltma, DHCP ve VLAN tabanlı yalıtım kullanıyorsa, VM ağı çoğaltma için bir statik IP adresi havuzu gerekmez. Herhangi bir adres havuzu kullanılabilir olmadığından, böylece yük devretme sınaması için Windows ağ sanallaştırma kullanarak çalışmaz. 
        
     - Yük devretme testi çoğaltma ağ yalıtımı kullanır ve Windows ağ sanallaştırma test ağı kullanıyorsa çalışmaz. Bu durum, yalıtımsız ağa bir Windows ağ sanallaştırma ağ oluşturmak için gereken alt ağı yok çünkü.
        
- Yük devretme sınaması için Ağ eşlemesi için seçtiğiniz ağ kullanmamanızı öneririz.

- Nasıl yük devretme VM ağı VMM konsolunda nasıl yapılandırıldığına bağlıdır sonra çoğaltma sanal makineleri eşlenen VM ağlarına bağlanır.


### <a name="vm-network-configured-with-no-isolation-or-vlan-isolation"></a>Hiçbir yalıtım veya VLAN yalıtımı ile yapılandırılmış VM ağı

Bir VM ağı VMM yalıtımı ve VLAN yalıtımı ile yapılandırılmışsa, aşağıdakileri unutmayın:

- VM ağı için DHCP tanımlanmışsa, çoğaltma sanal makinesi ilişkili mantıksal ağdaki ağ sitesi için belirtilen ayarları aracılığıyla VLAN ID bağlı. Sanal makinenin IP adresini kullanılabilir DHCP sunucusundan alır.
- Hedef VM ağı için statik bir IP adresi havuzu tanımlamak gerekmez. VM ağı için statik bir IP adresi havuzu kullandıysanız, çoğaltma sanal makinesi ilişkili mantıksal ağdaki ağ sitesi için belirtilen ayarları aracılığıyla VLAN ID bağlı.
- Sanal makine, VM ağ için tanımlı havuzundan IP adresini alır. Bir statik IP adresi havuzu hedef VM ağı üzerinde tanımlı değil, IP adresi ayırma başarısız olur. Koruma ve kurtarma için kullanacağınız hem kaynak hem de hedef VMM sunucularında IP adresi havuzu oluşturun.

### <a name="vm-network-with-windows-network-virtualization"></a>Windows ağ sanallaştırma ile VM ağı

Bir VM ağı VMM Windows ağ sanallaştırma ile yapılandırılmışsa, aşağıdakileri unutmayın:

- Kaynak VM ağına DHCP veya bir statik IP adresi havuzu kullanmak üzere mi yapılandırılmış bağımsız olarak hedef VM ağı için statik bir havuzu tanımlamanız gerekir. 
- DHCP tanımlarsanız, hedef VMM sunucusunda bir DHCP sunucusu gibi davranan ve hedef VM ağı için tanımlanan havuzundan bir IP adresi sağlar.
- Kaynak sunucu için bir statik IP adresi havuzu kullanımı tanımlanmışsa, hedef VMM Sunucu havuzundan bir IP adresi ayırır. Her iki durumda da, bir statik IP adresi havuzu tanımlanmazsa, IP adresi ayırma başarısız olur.



## <a name="prepare-the-infrastructure"></a>Altyapıyı hazırlama

Yalnızca bir VM devredebildiğini denetlemek istiyorsanız, yük devretme testi bir altyapı olmadan çalıştırabilirsiniz. Uygulama yük devretmeyi sınamak için tam bir DR detayı yapmak istiyorsanız, ikincil sitede altyapıyı hazırlama gerekir:

- Varolan bir ağ kullanarak yük devretme testi çalıştırırsanız, Active Directory, DHCP ve DNS, ağ hazırlayın.
- Bir VM ağı otomatik olarak oluşturmak için seçeneğiyle bir test yük devretme çalıştırırsanız, yük devretme testini çalıştırmadan önce otomatik olarak oluşturulan ağ altyapısı kaynakları eklemeniz gerekir. Kurtarma planında, bu kurtarma planında yük devretme sınaması için kullanacaksanız Grup 1 önce el ile adım ekleyerek kolaylaştırabilir. Daha sonra Yük devretme testini çalıştırmadan önce otomatik olarak oluşturulan ağ altyapısı kaynakları ekleyin.


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

1. Seçin **kurtarma planları** > *recoveryplan_name*. Tıklatın **yük devretme** > **yük devretme testi**.
2. Üzerinde **sınama yük devretmesi** dikey penceresinde, test yük devretme sonrasında nasıl çoğalma VM'ler ağlara bağlanmalıdır belirtin.
3. Yük devretme işleminin ilerleyişini izlemek **işleri** sekmesi.
4. Yük devretme işlemi tamamlandıktan sonra sanal makineleri başarıyla başlatıldığını doğrulayın.
5. İşiniz bittiğinde tıklatın **temizleme yük devretme testi** kurtarma planı üzerinde. Yük devretme testiyle ilişkili gözlemlerinizi **Notlar**’da kaydedin veya saklayın. Bu adım, tüm sanal makineleri ve Site Recovery tarafından yük devretme testi sırasında oluşturulan ağlar siler. 

![Yük devretme sınaması](./media/hyper-v-vmm-test-failover/TestFailover.png)
 


> [!TIP]
> Bir sanal makine için yük devretme testi sırasında verilen IP adresi, sanal makine alacağı aynı IP adresidir (IP adresi test yük devretme ağda kullanılabilir olduğunu pek fazla) planlanmış veya planlanmamış bir yük devretme için. Aynı IP adresini test yük devretme ağda kullanılabilir değilse, sanal makine test yük devretme ağı testinde kullanılabilirse başka bir IP adresi alır.



### <a name="run-a-test-failover-to-a-production-network"></a>Bir üretim ağı için yük devretme testi çalıştırma

Yük devretme testi sırasında ağ eşlemesini belirtilen üretim kurtarma sitesi ağınıza çalıştırmayın öneririz. Ancak bir yükü VM uçtan uca ağ bağlantısını doğrulamak gerekiyorsa aşağıdaki noktalara dikkat edin:

* Test yük devretme yapılırken birincil VM kapatıldığını emin olun. Bunu yapmazsanız, aynı kimliğe sahip iki sanal makineyi aynı ağda birlikte aynı anda çalıştırırsınız. Bu durum, istenmeyen sonuçlara neden olabilir.
* Test yük devretme sanal makinelerini Temizle yük devretme sınaması için yaptığınız tüm değişiklikler VM'ler kaybolur. Bu değişiklikler birincil VM çoğaltılmaz.
* Bu gibi sınama üretim uygulamanız için kesintiye neden yol açar. DR detayı devam ederken uygulama kullanmamayı uygulama kullanıcılarının isteyin.  


## <a name="next-steps"></a>Sonraki adımlar
DR detayı başarıyla çalıştırdıktan sonra [tam bir yük devretmeyi çalıştırma](site-recovery-failover.md).



