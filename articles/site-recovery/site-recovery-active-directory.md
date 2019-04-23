---
title: Active Directory ve DNS Azure Site Recovery ile olağanüstü durum kurtarma ayarlama | Microsoft Docs
description: Bu makalede, Active Directory ve DNS Azure Site Recovery ile olağanüstü durum kurtarma çözümü uygulamak açıklar.
services: site-recovery
documentationcenter: ''
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 4/9/2019
ms.author: mayg
ms.openlocfilehash: 58e360bb355c7faf9608b00dd65b14f27aca4367
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59790552"
---
# <a name="set-up-disaster-recovery-for-active-directory-and-dns"></a>Active Directory ve DNS için olağanüstü durum kurtarmayı ayarlayın

SharePoint, Dynamics AX ve SAP gibi Kurumsal uygulamaları düzgün çalışması için Active Directory ve DNS altyapısı bağlıdır. Uygulamalar için olağanüstü durum kurtarma ayarladığınızda, genellikle doğru uygulama işlevselliğinden emin olmak için diğer uygulama bileşenleri kurtarmadan önce Active Directory ve DNS kurtarmak gerekir.

Kullanabileceğiniz [Site Recovery](site-recovery-overview.md) Active Directory için bir olağanüstü durum kurtarma planı oluşturmak için. Bir kesinti oluştuğunda, bir yük devretme başlatabilirsiniz. Active Directory'yi ayarlama ve birkaç dakika içinde çalışan olabilir. Active Directory birden çok uygulama için birincil sitenizde dağıttıysanız, örneğin, SharePoint ve SAP, için üzerinde tam siteyi başarısız olmasına isteyebilirsiniz. İlk Site RECOVERY'yi kullanarak Active Directory başarısız olabilir. Ardından, uygulamaya özgü kurtarma planlarını kullanarak başka bir uygulama, başarısız.

Bu makalede, Active Directory için bir olağanüstü durum kurtarma çözümü oluşturma açıklanmaktadır. Bu, önkoşulları ve yük devretme için yönergeler içerir. Başlamadan önce Active Directory ve Site Recovery ile ilgili bilgi sahibi olmalıdır.

## <a name="prerequisites"></a>Önkoşullar

* Azure'a çoğaltma yapıyorsanız [Azure kaynaklarını hazırlama](tutorial-prepare-azure.md)bir abonelik, bir Azure sanal ağ, depolama hesabı ve bir kurtarma Hizmetleri kasası dahil olmak üzere.
* Tüm bileşenler için [destek gereksinimlerini](site-recovery-support-matrix-to-azure.md) gözden geçirin.

## <a name="replicate-the-domain-controller"></a>Etki alanı denetleyicisi çoğaltma

- Site Recovery çoğaltma, bir etki alanı denetleyicisini barındıran en az bir VM veya DNS ayarlamanız gerekir.
- Ortamınızda birden çok etki alanı denetleyicisi varsa, hedef sitedeki bir ek etki alanı denetleyicisi de ayarlamanız gerekir. Ek etki alanı denetleyicisi, azure'da veya ikincil şirket içi veri merkezinde olabilir.
- Yalnızca bazı uygulamalar ve bir etki alanı denetleyicisi varsa, tüm site üzerinde birlikte yük isteyebilirsiniz. Bu durumda, hedef siteye (ya da azure'da bir ikincil şirket içi veri merkezinde) etki alanı denetleyicisi çoğaltmak için Site Recovery kullanmanızı öneririz. Aynı yinelenen etki alanı denetleyicisi veya DNS sanal makine için kullanabileceğiniz [yük devretme testi](#test-failover-considerations).
- - Ortamınızda birçok uygulama ve birden fazla etki alanı denetleyicisi varsa veya birkaç uygulamalar Site Recovery ile etki alanı denetleyicisi sanal makinesini çoğaltmak için teker teker ayrıca başarısız. planlıyorsanız ayarlamanız önerilir bir Ek etki alanı denetleyicisi hedef sitede (ya da azure'da bir ikincil şirket içi veri merkezinde). İçin [yük devretme testi](#test-failover-considerations), Site Recovery tarafından çoğaltılmış etki alanı denetleyicisini kullanabilirsiniz. Yük devretme için hedef sitede ek etki alanı denetleyicisini kullanabilirsiniz.

## <a name="enable-protection-with-site-recovery"></a>Site Recovery korumasını etkinleştirin

Etki alanı denetleyicisi veya DNS barındıran sanal makineyi korumak için Site RECOVERY'yi kullanabilirsiniz.

### <a name="protect-the-vm"></a>VM koruma
Site Recovery kullanarak yinelenen etki alanı denetleyicisi için kullanılan [yük devretme testi](#test-failover-considerations). Aşağıdaki gereksinimleri karşıladığından emin olun:

1. Genel katalog sunucusu etki alanı denetleyicisidir.
2. Etki alanı denetleyicisi, yük devretme testi sırasında gerekli olan roller için FSMO rol sahibi olmalıdır. Aksi takdirde, bu rolleri olması gerekir [ele](https://aka.ms/ad_seize_fsmo) yük devretmeden sonra.

### <a name="configure-vm-network-settings"></a>VM ağ ayarlarını yapılandırma
Site recovery'de DNS veya etki alanı denetleyicisi barındıran sanal makine için ağ ayarları altında yapılandırma **işlem ve ağ** çoğaltılan sanal makinenin ayarlarını. Bu, sanal makine yük devretme sonrasında doğru ağa bağlı olmasını sağlar.

## <a name="protect-active-directory"></a>Active Directory koruma

### <a name="site-to-site-protection"></a>Siteden siteye koruma
İkincil sitede bir etki alanı denetleyicisi oluşturun. Bir etki alanı denetleyicisi rolünü sunucuyu yükselttiğinizde, birincil sitede kullanılan aynı etki alanının adını belirtin. Kullanabileceğiniz **Active Directory Siteleri ve Hizmetleri** siteleri eklenir site bağlantı nesnesi ayarlarını yapılandırmak için ek bileşenini. Üzerinde bir site bağlantısı ayarlarını yapılandırarak, iki veya daha fazla site ve ne sıklıkta gerçekleşir arasında çoğaltmanın zaman denetleyebilirsiniz. Daha fazla bilgi için [siteler arasında çoğaltma zamanlama](https://technet.microsoft.com/library/cc731862.aspx).

### <a name="site-to-azure-protection"></a>Azure için site koruması
İlk olarak bir Azure sanal ağındaki bir etki alanı denetleyicisi oluşturun. Bir etki alanı denetleyicisi rolünü sunucuyu yükselttiğinizde, birincil sitede kullanılan aynı etki alanı adını belirtin.

Ardından, DNS sunucusu sanal ağın DNS sunucusu Azure'da kullanmak üzere yeniden yapılandırın.

![Azure Ağı](./media/site-recovery-active-directory/azure-network.png)

### <a name="azure-to-azure-protection"></a>Azure'dan Azure'a koruma
İlk olarak bir Azure sanal ağındaki bir etki alanı denetleyicisi oluşturun. Bir etki alanı denetleyicisi rolünü sunucuyu yükselttiğinizde, birincil sitede kullanılan aynı etki alanı adını belirtin.

Ardından, DNS sunucusu sanal ağın DNS sunucusu Azure'da kullanmak üzere yeniden yapılandırın.

## <a name="test-failover-considerations"></a>Test yük devretme konuları
Üretim iş yükleri üzerindeki etkiyi önlemek için üretim ağınızdan yalıtılmış olan bir ağ yük devretme testi gerçekleşir.

Çoğu uygulama, bir etki alanı denetleyicisi veya DNS sunucusu bulunması gerekir. Bu nedenle, uygulama devreder önce yalıtılmış ağda test yük devretmesi için kullanılacak bir etki alanı denetleyicisi oluşturmanız gerekir. Bunu yapmanın en kolay yolu, bir etki alanı denetleyicisi veya DNS barındıran bir sanal makineyi çoğaltmak için Site Recovery kullanmaktır. Ardından, bir uygulama için bir kurtarma planı yük devretme çalıştırmadan önce etki alanı denetleyicisi sanal makinesinin test yük devretme çalıştırın. Bunu nasıl aşağıda verilmiştir:

1. Site RECOVERY'yi [çoğaltmak](vmware-azure-tutorial.md) etki alanı denetleyicisi veya DNS barındıran sanal makine.
2. Yalıtılmış ağ oluşturun. Azure'da oluşturduğunuz herhangi bir sanal ağ, diğer ağlardan varsayılan olarak yalıtılmıştır. Üretim ağınızda kullanan bu ağ için aynı IP adresi aralığı kullanmanızı öneririz. Bu ağ üzerinde siteden siteye bağlantı etkinleştirmeyin.
3. Yalıtılmış ağda bir DNS IP adresi sağlayın. Beklediğiniz almak için DNS sanal makinenin IP adresini kullanın. Azure'da çoğaltma yapıyorsanız, yük devretme işleminde kullanılan sanal makine için IP adresi sağlayın. Çoğaltılan sanal makinenin IP adresini girmek için **işlem ve ağ** ayarları, select **hedef IP** ayarları.

    ![Azure test ağı](./media/site-recovery-active-directory/azure-test-network.png)

    > [!TIP]
    > Site kurtarma denemeleri test sanal makineleri aynı ada sahip ve sağlanan aynı IP adresini kullanarak bir alt ağ oluşturmak **işlem ve ağ** sanal makinenin ayarlarını. Aynı ada sahip bir alt test yük devretmesi için sağlanan Azure sanal ağında kullanılabilir değilse, test sanal makinesi alfabetik olarak ilk alt ağ içinde oluşturulur.
    >
    > Hedef IP adresi seçilen alt ağın parçası ise, Site Recovery, hedef IP adresini kullanarak test yük devretme sanal makine oluşturmayı dener. Hedef IP Seçili alt ağın bir parçası değilse, seçilen alt ağda bir sonraki kullanılabilir IP'yi kullanarak test yük devretme sanal makine oluşturulur.
    >
    >

### <a name="test-failover-to-a-secondary-site"></a>İkincil siteye yük devretme testi

1. Başka bir şirket içi siteye çoğaltma yapıyorsanız ve DHCP kullanıyorsanız [DNS ve DHCP yük devretme testi için ayarlama](hyper-v-vmm-test-failover.md#prepare-dhcp).
2. Etki alanı denetleyicisi sanal makinesinin yalıtılmış ağ içinde çalıştırılan bir yük devretme testi yaparsınız. En son kullanılabilir *uygulamayla tutarlı* etki alanı denetleyicisi sanal makinesinin test yük devretmesi yapmak için kurtarma noktası.
3. Uygulamanın çalıştığı sanal makineler içeren bir kurtarma planı için bir yük devretme testi çalıştırın.
4. Test tamamlandığında, *yük devretme testini Temizleme* etki alanı denetleyicisi sanal makinesinde. Bu adım, yük devretme testi için oluşturulan etki alanı denetleyicisi siler.


### <a name="remove-references-to-other-domain-controllers"></a>Diğer etki alanı denetleyicilerine başvurularını Kaldır
Yük devretme testi başlattığınızda, tüm etki alanı denetleyicileri test ağında dahil değildir. Üretim ortamınızda mevcut diğer etki alanı denetleyicilerine başvuruları kaldırmak için için ihtiyacınız olabilecek [Active Directory FSMO rollerini ele geçirmek](https://aka.ms/ad_seize_fsmo) yapıp [meta veri temizleme](https://technet.microsoft.com/library/cc816907.aspx) etki alanı denetleyicileri eksik .


### <a name="issues-caused-by-virtualization-safeguards"></a>Sorun yaparak sanallaştırma korumalarını neden oldu

> [!IMPORTANT]
> Bu bölümde açıklanan yapılandırmalardan bazıları, standart veya varsayılan etki alanı denetleyicisi yapılandırması değildir. Üretim etki alanı denetleyicisi bu değişiklikleri yapmak istemiyorsanız, test yük devretmesi için kullanılacak Site Recovery için ayrılmış bir etki alanı denetleyicisi oluşturabilirsiniz. Bu değişiklik yalnızca, etki alanı denetleyicisi.  
>
>

Windows Server 2012 ile başlayan [ek güvenlik önlemleri, Active Directory etki alanı Hizmetleri (AD DS) içinde yerleşiktir](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100). Bu korumalar temel alınan hiper yönetici platformuna destekliyorsa, sanallaştırılmış etki alanı denetleyicilerinde USN geri alma işlemleri korumak **VM-Generationıd**. Azure'un destekledikleri **VM-Generationıd**. Bu nedenle, Windows Server 2012 veya daha sonra Azure sanal makineleri çalıştıran etki alanı denetleyicilerinin bu ek korumalar vardır.


Zaman **VM-Generationıd** sıfırlama, **Invocationıd** AD DS veritabanını değerini de sıfırlanır. Ayrıca, RID havuzu atılır ve sysvol klasörünü yetkisiz olarak işaretlenir. Daha fazla bilgi için [Active Directory Domain Services sanallaştırma giriş](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100) ve [güvenli bir şekilde DFSR sanallaştırılma](https://blogs.technet.microsoft.com/filecab/2013/04/05/safely-virtualizing-dfsr/).

Azure'a yük devretme neden olabilecek **VM-Generationıd** sıfırlanır. Sıfırlama **VM-Generationıd** etki alanı denetleyicisi sanal makinesini Azure'da başladığında ek korumalarını tetikler. Bu neden bir *önemli gecikmeye* de olan etki alanı denetleyicisi sanal makineye oturum açabilir.

Bu etki alanı denetleyicisi yalnızca bir sınama yük devretme kümesinde kullanıldığından, sanallaştırma korumalarını gerekli değildir. Emin olmak için **VM-Generationıd** etki alanı denetleyicisi sanal makinesini değerini değiştirmez, için aşağıdaki DWORD değerini değiştirebilirsiniz **4** şirket içi etki alanı denetleyicisinde:


`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\gencounter\Start`


#### <a name="symptoms-of-virtualization-safeguards"></a>Sanallaştırma korumaları belirtileri

Yük devretme testinden sonra sanallaştırma korumaları tetiklenir, bir veya daha fazla aşağıdaki belirtilerden birini görebilirsiniz:  

* **Generationıd** değişiklikleri değeri.

    ![Oluşturma kimliği değişikliği](./media/site-recovery-active-directory/Event2170.png)

* **Invocationıd** değişiklikleri değeri.

    ![Çağırma kimliği değişikliği](./media/site-recovery-active-directory/Event1109.png)

* Klasör SYSVOL ve NETLOGON paylaşımları kullanılamıyor.

    ![Sysvol klasörünü paylaş](./media/site-recovery-active-directory/sysvolshare.png)

    ![NtFrs sysvol klasörü](./media/site-recovery-active-directory/Event13565.png)

* DFSR veritabanlarını silinir.

    ![DFSR veritabanlarını sildi](./media/site-recovery-active-directory/Event2208.png)


### <a name="troubleshoot-domain-controller-issues-during-test-failover"></a>Yük devretme testi sırasında etki alanı denetleyicisi sorunlarını giderme

> [!IMPORTANT]
> Bu bölümde açıklanan yapılandırmalardan bazıları, standart veya varsayılan etki alanı denetleyicisi yapılandırması değildir. Üretim etki alanı denetleyicisi bu değişiklikleri yapmak istemiyorsanız, Site Recovery yük devretme testi için özel bir etki alanı denetleyicisi oluşturabilirsiniz. Değişiklik yalnızca, özel etki alanı denetleyicisi.  
>
>

1. Komut isteminde sysvol ve NETLOGON klasörlerini paylaşılan olup olmadığını denetlemek için aşağıdaki komutu çalıştırın:

    `NET SHARE`

2. Komut isteminde, etki alanı denetleyicisi düzgün çalıştığından emin olmak için aşağıdaki komutu çalıştırın:

    `dcdiag /v > dcdiag.txt`

3. Aşağıdaki metni için çıktı günlüğüne bakın. Metin, etki alanı denetleyicisi düzgün çalıştığını doğrular.

    * "geçen test bağlantısı"
    * "geçen test reklam"
    * "geçen test MakineHesabı"

Önceki koşul sağlanırsa, etki alanı denetleyicisi düzgün çalıştığını olasıdır. Yüklü değilse, aşağıdaki adımları tamamlayın:

1. Etki alanı denetleyicisinin yetkili geri yükleme yapın. Aşağıdaki bilgileri göz önünde bulundurun:
    * Önermemekteyiz rağmen [FRS çoğaltma](https://blogs.technet.microsoft.com/filecab/2014/06/25/the-end-is-nigh-for-frs/), FRS çoğaltma kullanıyorsanız, yetkisiz bir geri yükleme adımlarını izleyin. İşlem açıklanan [dosya çoğaltma hizmeti yeniden başlatmak için BurFlags kayıt defteri anahtarını kullanarak](https://support.microsoft.com/kb/290762).

        BurFlags hakkında daha fazla bilgi için blog gönderisine bakın [D2 ve D4: Ne işe yarar? ](https://blogs.technet.microsoft.com/janelewis/2006/09/18/d2-and-d4-what-is-it-for/).
    * DFSR çoğaltma kullanırsanız, yetkisiz bir geri yükleme adımlarını tamamlayın. İşlem açıklanan [DFSR ile çoğaltılan sysvol klasörü (örneğin, "D4/D2" FRS için) için yetkilendirmeli ve yetkilendirmesiz bir eşitlemeyi zorlayarak](https://support.microsoft.com/kb/2218556).

        PowerShell işlevleri de kullanabilirsiniz. Daha fazla bilgi için [SYSVOL DFSR yetkili/yetkili olmayan geri yükleme PowerShell işlevleri](https://blogs.technet.microsoft.com/thbouche/2013/08/28/dfsr-sysvol-authoritative-non-authoritative-restore-powershell-functions/).

2. Aşağıdaki kayıt defteri anahtarını ayarlayarak ilk eşitleme gereksinimini atlamak **0** şirket içi etki alanı denetleyicisinde. DWORD yoksa, bunun altında oluşturabilirsiniz **parametreleri** düğümü.

    `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters\Repl Perform Initial Synchronizations`

    Daha fazla bilgi için [DNS olay kimliği 4013 sorunlarını giderme: DNS sunucusu yükleyemedi AD tümleşik DNS bölgeleri](https://support.microsoft.com/kb/2001093).

3. Genel katalog sunucusu kullanıcı oturum açma doğrulamak kullanılabilir gereksinim devre dışı bırakın. Bu, şirket içi etki alanı denetleyicisi yapmak için aşağıdaki kayıt defteri anahtarını ayarlamak **1**. DWORD yoksa, bunun altında oluşturabilirsiniz **Lsa** düğümü.

    `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\IgnoreGCFailures`

    Daha fazla bilgi için [genel katalog sunucusu kullanıcı oturum açma işlemi doğrulamak kullanılabilir gereksinimini devre dışı bırakmak](https://support.microsoft.com/kb/241789).

### <a name="dns-and-domain-controller-on-different-machines"></a>Farklı makinelerde DNS ve etki alanı denetleyicisi

Aynı sanal makinede etki alanı denetleyicisi ve DNs çalıştırıyorsanız, bu yordamı atlayabilirsiniz.


DNS etki alanı denetleyicisi olarak aynı VM'de yoksa, yük devretme testi için bir DNS VM oluşturmak gerekir. Yeni bir DNS sunucusu kullanın ve gerekli tüm bölgeler oluşturun. Örneğin, Active Directory etki alanı contoso.com ise, ile adı contoso.com DNS bölgesi oluşturabilirsiniz. Active Directory'ye karşılık gelen girişler DNS'de şu şekilde güncelleştirilmesi gerekir:

1. Kurtarma planında başka bir sanal makine başlatılmadan önce bu ayarları yerinde olduğundan emin olun:
   * Orman kök adından sonra bölgeyi yeniden adlandırılması gerekir.
   * Bölge, dosya yedekli olması gerekir.
   * Bölge için güncelleştirmeleri güvenli hem de etkinleştirilmesi gerekir.
   * Etki alanı denetleyicisi sanal makinesinin çözümleyici DNS sanal makinenin IP adresine işaret etmelidir.

2. Etki alanı denetleyicisini barındıran VM üzerinde aşağıdaki komutu çalıştırın:

    `nltest /dsregdns`

3. DNS sunucusuna bir bölgesine ekleyin, güvenli olmayan güncelleştirmelere izin ver ve için DNS bölgesi için bir giriş eklemek için aşağıdaki komutları çalıştırın:

    `dnscmd /zoneadd contoso.com  /Primary`

    `dnscmd /recordadd contoso.com  contoso.com. SOA %computername%.contoso.com. hostmaster. 1 15 10 1 1`

    `dnscmd /recordadd contoso.com %computername%  A <IP_OF_DNS_VM>`

    `dnscmd /config contoso.com /allowupdate 1`

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [kurumsal iş yüklerini Azure Site Recovery ile koruma](site-recovery-workload.md).
