---
title: Azure Site RECOVERY'yi kullanarak ikincil bir siteye Hyper-V vm'lerinin olağanüstü durum kurtarma tatbikatı çalıştırma | Microsoft Docs
description: Azure Site Recovery kullanarak bir ikincil şirket içi veri merkezinde VMM bulutlarındaki Hyper-V Vm'leri için DR detaylandırması çalıştırmayı öğrenin.
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: rajanaki
ms.openlocfilehash: dc8deb16f7d124c5fb11568f25050eee99a245b8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60865529"
---
# <a name="run-a-dr-drill-for-hyper-v-vms-to-a-secondary-site"></a>DR tatbikatı Hyper-V Vm'leri için ikincil bir siteye çalıştırın.


Bu makalede bir ikincil şirket içi siteye System Center Virtual Machine Manager V(MM) bulutlarında yönetilen Hyper-V Vm'leri için olağanüstü durum kurtarma (DR) tatbikatı yapmak nasıl kullanarak [Azure Site Recovery](site-recovery-overview.md).

Çoğaltma stratejinizi doğrulamak için yük devretme testi çalıştırma ve veri kaybı veya kesinti süresi olmadan bir DR tatbikatı gerçekleştirme. Yük devretme testi sürmekte olan çoğaltmayı veya üretim ortamınız üzerinde hiçbir etkisi yok. 

## <a name="how-do-test-failovers-work"></a>Yük devretme iş nasıl test?

Birincil düğümden ikincil siteye yük devretme testi çalıştırın. VM yük devreder denetlemek istiyorsanız, ikincil sitede ayarlama, hiçbir şey olmadan bir yük devretme testi çalıştırabilirsiniz. Beklendiği gibi uygulama yük devretme çalıştığını doğrulamak istiyorsanız, ağ ve altyapı ikincil konumdaki ayarlamanız gerekir.
- Tek bir VM üzerinde veya yük devretme testi çalıştırma bir [kurtarma planı](site-recovery-create-recovery-plans.md).
- Var olan bir ağ veya otomatik olarak oluşturulan bir ağ ile bir ağ olmadan bir yük devretme testi çalıştırabilirsiniz. Aşağıdaki tabloda bu seçenekler hakkında daha fazla ayrıntı sağlanır.
    - Ağ olmadan bir yük devretme testi çalıştırabilirsiniz. Bu seçenek, yalnızca bir sanal makine yük devretme yapabilir, ancak herhangi bir ağ yapılandırma doğrulamak mümkün olmayacaktır denetlemek istiyorsanız yararlıdır.
    - Var olan bir ağ ile yük devretme çalıştırın. Bir üretim ağı kullanmayın öneririz.
    - Yük devretme çalıştırın ve Site Recovery otomatik olarak bir test ağı oluşturmak istiyorum. Bu durumda Site Recovery ağ otomatik olarak oluşturun ve yük devretme testi tamamlandığında yukarı temizleyin.
- Yük devretme testi için bir kurtarma noktası seçmeniz gerekir: 
    - **En son işlenen**: Bu seçenek bir VM'nin Site Recovery tarafından işlenen en son kurtarma noktasına devreder. İşlenmemiş verileri işlemek için zaman harcanmadığından bu seçenekte düşük bir RTO (Kurtarma Süresi Hedefi) sağlanır.
    - **Uygulamayla tutarlı olan sonuncu**: Bu seçeneği bir VM'nin Site Recovery tarafından işlenen en son uygulamayla tutarlı kurtarma noktasına devredilir. 
    - **En son**: Bu seçenek ilk önce yük devretme için her VM için bir kurtarma noktası oluşturmak için Site Recovery hizmetine gönderilen tüm verileri işler. Yük devretme yük devretme tetiklendiğinde Site Recovery'ye çoğaltılan tüm verilere sahip sonra VM oluşturulduğundan, bu seçenek en düşük RPO (kurtarma noktası hedefi) sağlar.
    - **En son işlenen VM'li**: Çoklu VM tutarlılığı etkin olan bir veya daha fazla VM içeren kurtarma planları için kullanılabilir. En son ortak çoklu VM tutarlı kurtarma noktası ayarı etkin Vm'lerle devredin. Diğer VM'ler en son işlenen kurtarma noktasına yük devredebilirsiniz.
    - **En son çoklu VM uygulamayla tutarlı**: Bu seçenek, çoklu VM tutarlılığı etkin olan bir veya daha fazla VM içeren kurtarma planları için kullanılabilir. En son ortak çoklu VM uygulamayla tutarlı kurtarma noktası çoğaltma grubunun parçası olan Vm'leri devredin. Bunların en son uygulamayla tutarlı kurtarma noktası için diğer Vm'lere devredin.
    - **Özel**: Belirli bir VM'ye belirli bir kurtarma noktasına yük devretmek için bu seçeneği kullanın.



## <a name="prepare-networking"></a>Ağ hazırlama

Yük devretme testi çalıştırdığınızda, test çoğaltma makineler için ağ ayarlarını seçin tabloda özetlenen istenir.

| **Seçeneği** | **Ayrıntılar** | |
| --- | --- | --- |
| **Yok.** | Test sanal makinesi, çoğaltma VM'si bulunduğu konak üzerinde oluşturulur. Buluta eklenmez ve herhangi bir ağa bağlı değil.<br/><br/> Oluşturulduktan sonra makinenin bir VM ağı'na bağlanabilirsiniz.| |
| **Var olanı kullan** | Test sanal makinesi, çoğaltma VM'si bulunduğu konak üzerinde oluşturulur. Bu buluta eklenmez.<br/><br/>Üretim ağınızdan yalıtılmış olan bir VM ağı oluşturun.<br/><br/>Bir VLAN tabanlı ağ kullanıyorsanız (üretimde kullanılmaz) ayrı bir mantıksal ağ oluşturma VMM'de bu amaçla öneririz. Bu mantıksal ağ, yük devretme testi için VM ağları oluşturmak için kullanılır.<br/><br/>Mantıksal ağın en az bir sanal makine barındıran Hyper-V sunucularının ağ bağdaştırıcıları ile ilişkili olması gerekir.<br/><br/>VLAN için mantıksal ağları, ağ sitelerini mantıksal ağa ekleyin yalıtılmış olması gerekir.<br/><br/>Windows ağ sanallaştırma tabanlı bir mantıksal ağ kullanıyorsanız, Azure Site Recovery otomatik olarak yalıtılmış VM ağları oluşturur. | |
| **Bir ağ oluşturma** | Bir geçici bir test ağı belirttiğiniz ayarına göre otomatik olarak oluşturulan **mantıksal ağ** ve onun ilişkili ağ siteleri.<br/><br/> Yük devretme sanal makineleri oluşturulur denetler.<br/><br/> Bir kurtarma planı birden fazla VM ağı kullanıyorsa bu seçeneği kullanmanız gerekir.<br/><br/> Windows ağ Sanallaştırması ağlarına kullanıyorsanız, bu seçenek otomatik olarak çoğaltma sanal makinesinin ağ (alt ağlar ve IP adresi havuzları) aynı ayarlarla bir VM ağları oluşturabilir. Test yük devretmesi tamamlandıktan sonra bu VM ağları otomatik olarak temizlenir.<br/><br/> Test çoğaltma sanal makinesi bulunduğu konakta VM oluşturulur. Bu buluta eklenmez.|

### <a name="best-practices"></a>En iyi uygulamalar

- Bir üretim ağı test, üretim iş yükleri için kesinti neden olur. Olağanüstü durum kurtarma tatbikatı devam ederken, ilişkili uygulamaları kullanmayacak şekilde eşitlemelerini isteyin.

- Test ağı, yük devretme testi için kullanılan VMM mantıksal ağı türüyle eşleşmesi gerekmez. Ancak, bazı birleşimleri çalışmaz:

     - Çoğaltma, DHCP ve VLAN tabanlı yalıtım kullanıyorsa, statik IP adresi havuzu VM ağı için çoğaltma gerektirmez. Hiçbir adres havuzları kullanılabilir olmadığından, yük devretme testi için Windows ağ sanallaştırma kullanarak işe yaramaz. 
        
     - Test yük devretme, çoğaltma ağ yalıtımsız kullanır ve Windows ağ Sanallaştırması test ağı kullanıyorsa işe yaramaz. Bir Windows ağ sanallaştırma ağ oluşturmak için gereken alt ağ yalıtımsız ağ içermiyor olmasıdır.
        
- Yük devretme testi için Ağ eşlemesi için seçtiğiniz ağ kullanmamanızı öneririz.

- Nasıl yük devretme VM ağı VMM konsolunda nasıl yapılandırıldığına bağlıdır sonra çoğaltma sanal makinelerini eşlenen VM ağlarına bağlanır.


### <a name="vm-network-configured-with-no-isolation-or-vlan-isolation"></a>Hiçbir yalıtım veya VLAN yalıtımı ile yapılandırılmış VM ağı

Bir VM ağı VMM'de yalıtımı ve VLAN yalıtımı ile yapılandırılmışsa, aşağıdakileri unutmayın:

- VM ağı için DHCP tanımlanmazsa, çoğaltma sanal makinesi VLAN kimliği ilişkili bir mantıksal ağdaki ağ alanı için belirtilen ayarları aracılığıyla bağlanır. Sanal makineyi kullanılabilir bir DHCP sunucusundan IP adresini alır.
- Hedef VM ağı için statik bir IP adresi havuzu tanımlamanız gerekmez. Bir statik IP adresi havuzu VM ağı için kullanılırsa, çoğaltma sanal makinesi VLAN kimliği ilişkili bir mantıksal ağdaki ağ alanı için belirtilen ayarları aracılığıyla bağlanır.
- Sanal makineyi, VM ağ için tanımlanan havuzundan IP adresini alır. Hedef VM ağında bir statik IP adresi havuzu tanımlanmadı, IP adresi ayırma başarısız olur. Koruma ve kurtarma için kullanacağınız hem kaynak hem de hedef VMM sunucularında IP adresi havuzu oluşturun.

### <a name="vm-network-with-windows-network-virtualization"></a>Windows ağ sanallaştırma ile bir VM ağı

Bir VM ağı VMM Windows ağ sanallaştırma ile yapılandırılmışsa, aşağıdakileri unutmayın:

- Kaynak VM ağına DHCP veya statik bir IP adresi havuzu kullanmak için yapılandırılmış olup bağımsız olarak hedef VM ağ için statik bir havuzu tanımlamanız gerekir. 
- DHCP tanımlarsanız, hedef VMM sunucusunu bir DHCP sunucusu gibi davranan ve hedef VM ağı için tanımlanan havuzdan bir IP adresi sağlar.
- Kaynak sunucu için bir statik IP adresi havuzu kullanımı tanımlanmazsa, hedef VMM sunucusunu havuzdan bir IP adresi ayırır. Her iki durumda da, bir statik IP adresi havuzu tanımlanmamışsa, IP adresi ayırma başarısız olur.



## <a name="prepare-the-infrastructure"></a>Altyapıyı hazırlama

Bir VM devredebilir denetlemek istiyorsanız, bir altyapı olmadan bir yük devretme testi çalıştırabilirsiniz. Uygulama yük devretme testi için tam bir DR tatbikatı yapmak istiyorsanız, ikincil sitede altyapıyı hazırlamanız gerekir:

- Var olan bir ağı kullanarak bir yük devretme testi çalıştırırsanız, Active Directory, DHCP ve DNS, ağ hazırlayın.
- Otomatik olarak bir VM ağı oluşturma seçeneği ile yük devretme testi çalıştırırsanız, yük devretme testini çalıştırmadan önce otomatik olarak oluşturulan ağ altyapı kaynaklarını eklemeniz gerekir. Bir kurtarma planında, bu test yük devretmesi için kullanılacak gideceğinizi kurtarma planında el ile bir adım önce Grup 1 ekleyerek kolaylaştırabilir. Ardından, yük devretme testini çalıştırmadan önce otomatik olarak oluşturulan ağ altyapı kaynakları ekleyin.


### <a name="prepare-dhcp"></a>DHCP hazırlama
Katılan sanal makinelerin yük devretme testi DHCP kullanmak, test DHCP sunucusuna test yük devretmesi amacıyla yalıtılmış ağ içinde oluşturun.


### <a name="prepare-active-directory"></a>Active Directory hazırlama
Uygulama testi için bir yük devretme testi çalıştırmak için test ortamında üretim Active Directory ortamında bir kopyasına ihtiyacınız vardır. Daha fazla bilgi için gözden [test Active Directory için yük devretme konuları](site-recovery-active-directory.md#test-failover-considerations).

### <a name="prepare-dns"></a>DNS hazırlama
Bir DNS sunucusu yük devretme testi için aşağıdaki gibi hazırlayın:

* **DHCP**: Sanal makineler DHCP kullanıyorsanız, IP adresi DNS testin test DHCP sunucusunda güncelleştirilmesi gerekir. Bir ağ türü Windows ağ sanallaştırma kullanıyorsanız, VMM sunucusu DHCP sunucusu olarak görev yapar. Bu nedenle, DNS IP adresi, yük devretme ağı testinde güncelleştirilmelidir. Bu durumda, sanal makinelerin kendilerini ilgili DNS sunucusuna kaydedin.
* **Statik adres**: Sanal makinelere statik IP adresi kullanırsanız, test DNS sunucusunun IP adresini yük devretme ağı testinde güncelleştirilmelidir. DNS test sanal makinelerin IP adresiyle güncelleştirmeniz gerekebilir. Aşağıdaki örnek betik, bu amaç için kullanabilirsiniz:

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

Bu yordamda, bir kurtarma planı için bir yük devretme testi çalıştırma açıklanmaktadır. Alternatif olarak, tek bir sanal makine için yük devretme çalıştırabileceğiniz **sanal makineler** sekmesi.

1. Seçin **kurtarma planları** > *recoveryplan_name*. Tıklayın **yük devretme** > **yük devretme testi**.
2. Üzerinde **sınama Yük Devretmesini** dikey penceresinde nasıl çoğaltma Vm'leri yük devretme testinden sonra bağlı ağlara belirtin.
3. Yük devretme işleminin ilerleyişini izlemek **işleri** sekmesi.
4. Yük devretme işlemi tamamlandıktan sonra sanal makineleri başarılı bir şekilde başlatıldığını doğrulayın.
5. İşiniz bittiğinde tıklayın **yük devretme testini Temizle** kurtarma planında. Yük devretme testiyle ilişkili gözlemlerinizi **Notlar**’da kaydedin veya saklayın. Bu adım, tüm Vm'leri ve Site Recovery tarafından yük devretme testi sırasında oluşturulan ağlar siler. 

![Yük devretme testi](./media/hyper-v-vmm-test-failover/TestFailover.png)
 


> [!TIP]
> Yük devretme testi sırasında bir sanal makineye verilen IP adresini, sanal makine alırsınız aynı IP adresidir (pek fazla IP adresi yük devretme ağı testinde kullanılabilir) planlanmış veya planlanmamış bir yük devretme için. Yük devretme ağı testinde aynı IP adresi yoksa, sanal makine test yük devretme ağı olan başka bir IP adresi alır.



### <a name="run-a-test-failover-to-a-production-network"></a>Bir üretim ağı için yük devretme testi çalıştırma

Yük devretme testi sırasında ağ eşlemesini belirttiğiniz üretim kurtarma sitesi ağ çalıştırma öneririz. Ancak, devredilen VM'deki uçtan uca ağ bağlantısını doğrulamak gerekiyorsa aşağıdaki noktalara dikkat edin:

* Yük devretme Testi gerçekleştirirken birincil VM kapatılmış olduğundan emin olun. Aksi takdirde, aynı kimliğe sahip iki sanal makine aynı ağda aynı anda çalıştırırsınız. Bu durum istenmeyen sonuçlara yol açabilir.
* Test yük devretme sanal makinelerini Temizle, Vm'leri yük devretme testi için yaptığınız tüm değişiklikler kaybolur. Bu değişiklikleri tekrar birincil Vm'lere çoğaltılmaz.
* Şu şekilde test üretim uygulamanız için kesintiye neden neden olur. DR tatbikatı devam ederken uygulama kullanmayacak şekilde uygulama kullanıcılarının isteyin.  


## <a name="next-steps"></a>Sonraki adımlar
DR tatbikatı başarıyla çalıştırdıktan sonra [tam bir yük devretme çalıştırma](site-recovery-failover.md).



