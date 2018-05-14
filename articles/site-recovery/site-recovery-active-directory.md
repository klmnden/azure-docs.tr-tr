---
title: Active Directory ve DNS Azure Site Recovery ile koruma | Microsoft Docs
description: Bu makalede, Azure Site RECOVERY'yi kullanarak Active Directory için bir olağanüstü durum kurtarma çözümü uygulamak açıklar.
services: site-recovery
documentationcenter: ''
author: mayanknayar
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 05/11/2018
ms.author: manayar
ms.openlocfilehash: 97923af5ed4191f66434166c4743e398f8ac635a
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="use-azure-site-recovery-to-protect-active-directory-and-dns"></a>Active Directory ve DNS korumak için Azure Site Recovery kullanma

SharePoint, Dynamics AX ve SAP gibi kurumsal uygulamalar düzgün çalışması için Active Directory ve DNS altyapısı bağlıdır. Uygulamalar için olağanüstü durum kurtarma ayarladığınızda, genellikle doğru uygulama işlevselliği sağlamak için diğer uygulama bileşenleri kurtarmadan önce Active Directory ve DNS kurtarmak gerekir.

Kullanabileceğiniz [Site Recovery](site-recovery-overview.md) Active Directory için bir olağanüstü durum kurtarma planı oluşturmak için. Bir kesinti oluştuğunda, bir yük devretme işlemi başlatabilirsiniz. Active Directory'yi ayarlama ve birkaç dakika içinde çalışıyor olabilir. Birincil sitede birden çok uygulama için Active Directory dağıttıysanız, örneğin, SharePoint ve SAP, eksiksiz site vermesine istediğiniz. Madde Kurtarma kullanarak Active Directory ilk başarısız olabilir. Ardından, uygulamaya özgü kurtarma planları kullanan diğer uygulamalar başarısız.

Bu makalede, Active Directory için bir olağanüstü durum kurtarma çözümü oluşturma açıklanmaktadır. Önkoşullar ve yük devretme yönergeleri içerir. Başlamadan önce Active Directory ve Site kurtarma konusunda bilgi sahibi olmanız gerekir.

## <a name="prerequisites"></a>Önkoşullar

* Azure'a çoğaltma yapıyorsanız [Azure kaynaklarını hazırlama](tutorial-prepare-azure.md)bir abonelik, Azure sanal ağı, bir depolama hesabı ve bir kurtarma Hizmetleri kasası dahil olmak üzere.
* Tüm bileşenler için [destek gereksinimlerini](site-recovery-support-matrix-to-azure.md) gözden geçirin.

## <a name="replicate-the-domain-controller"></a>Etki alanı denetleyicisi çoğaltma

Bunu ayarlamanız gerekir [Site Recovery çoğaltma](#enable-protection-using-site-recovery), bir etki alanı denetleyicisi ya da DNS barındıran en az bir VM üzerinde. Varsa [birden çok etki alanı denetleyicileri](#environment-with-multiple-domain-controllers) ortamınızda, bunu da ayarlamanız gerekir bir [ek etki alanı denetleyicisi](#protect-active-directory-with-active-directory-replication) hedef sitede. Ek etki alanı denetleyicisi, Azure veya bir ikincil şirket içi veri merkezinde olabilir.

### <a name="single-domain-controller"></a>Tek etki alanı denetleyicisi
Yalnızca birkaç uygulamaları ve bir etki alanı denetleyicisi varsa, tüm site birlikte başarısız isteyebilirsiniz. Bu durumda, hedef siteye (veya azure'da bir ikincil şirket içi veri merkezinde) etki alanı denetleyicisi çoğaltmak için Site Recovery kullanmanızı öneririz. Aynı çoğaltılmış etki alanı denetleyicisi veya DNS sanal makine için kullanabileceğiniz [yük devretme sınamasını](#test-failover-considerations).

### <a name="multiple-domain-controllers"></a>Birden çok etki alanı denetleyicileri
Ortamınızda çok sayıda uygulama ve birden fazla etki alanı denetleyicisi olması veya birkaç uygulamalar Site Recovery ile etki alanı denetleyicisi sanal makine çoğaltma için bir defada ayrıca başarısız. planlıyorsanız bir ayarlamanızıöneririz[ek etki alanı denetleyicisi](#protect-active-directory-with-active-directory-replication) hedef sitede (veya azure'da bir ikincil şirket içi veri merkezinde). İçin [yük devretme sınamasını](#test-failover-considerations), Site Recovery tarafından kopyalanan etki alanı denetleyicisini kullanabilirsiniz. Yük devretme için hedef sitede ek etki alanı denetleyicisini kullanabilirsiniz.

## <a name="enable-protection-with-site-recovery"></a>Site Recovery ile korumayı etkinleştir

Etki alanı denetleyicisi veya DNS barındıran sanal makineyi korumak için Site RECOVERY'yi kullanabilirsiniz.

### <a name="protect-the-vm"></a>VM'yi koruma
Site RECOVERY'yi kullanarak kopyalanan etki alanı denetleyicisi için kullanılan [yük devretme sınamasını](#test-failover-considerations). Aşağıdaki gereksinimleri karşıladığından emin olun:

1. Bir genel katalog sunucusu etki alanı denetleyicisidir.
2. Etki alanı denetleyicisi, yük devretme testi sırasında gerekli olan roller için FSMO rol sahibi olmalıdır. Aksi takdirde, bu rolleri olması gerekecektir [ele](http://aka.ms/ad_seize_fsmo) yük devretme sonrasında.

### <a name="configure-vm-network-settings"></a>VM ağ ayarlarını yapılandırma
Etki alanı denetleyicisi veya Site kurtarma hizmetinde DNS barındıran sanal makinenin altında ağ ayarlarını yapılandırma **işlem ve ağ** çoğaltılan sanal makinenin ayarlarını. Bu sanal makinenin yük devretme sonrasında doğru ağa bağlı sağlar.

## <a name="protect-active-directory"></a>Active Directory koruma

### <a name="site-to-site-protection"></a>Siteden siteye koruma
İkincil sitede bir etki alanı denetleyicisi oluşturun. Bir etki alanı denetleyicisi rolünü sunucuya dağıttığınızda, birincil sitede kullanılan aynı etki alanının adını belirtin. Kullanabileceğiniz **Active Directory Siteleri ve Hizmetleri** siteler eklenir site bağlantı nesnesi ayarlarını yapılandırmak için ek bileşenini. Bir site bağlantısı ayarlarını yapılandırarak, iki veya daha fazla site ve ne sıklıkta oluştuğunu arasında çoğaltmanın ne zaman denetleyebilirsiniz. Daha fazla bilgi için bkz: [siteler arasında çoğaltma zamanlama](https://technet.microsoft.com/library/cc731862.aspx).

### <a name="site-to-azure-protection"></a>Site Azure koruma
İlk olarak, bir Azure sanal ağındaki bir etki alanı denetleyicisi oluşturun. Bir etki alanı denetleyicisi rolünü sunucuya dağıttığınızda, birincil sitede kullanılan aynı etki alanı adı belirtin.

Ardından, Azure'da DNS sunucusu kullanmak üzere sanal ağ için DNS sunucusu yeniden yapılandırın.

![Azure Ağı](./media/site-recovery-active-directory/azure-network.png)

### <a name="azure-to-azure-protection"></a>Azure Azure koruma
İlk olarak, bir Azure sanal ağındaki bir etki alanı denetleyicisi oluşturun. Bir etki alanı denetleyicisi rolünü sunucuya dağıttığınızda, birincil sitede kullanılan aynı etki alanı adı belirtin.

Ardından, Azure'da DNS sunucusu kullanmak üzere sanal ağ için DNS sunucusu yeniden yapılandırın.

## <a name="test-failover-considerations"></a>Test yük devretme konuları
Üretim iş yükleri üzerindeki etkisini önlemek için üretim ağınızdan yalıtılmış olan bir ağ yük devretme testi oluşur.

Birçok uygulama, bir etki alanı denetleyicisi veya bir DNS sunucusu bulunması gerekir. Bu nedenle, uygulama yöneltilir önce yük devretme sınaması için kullanılacak yalıtılmış ağ içindeki bir etki alanı denetleyicisi oluşturmanız gerekir. Bunu yapmanın en kolay yolu, bir etki alanı denetleyicisi ya da DNS barındıran bir sanal makine çoğaltmak için Site Recovery kullanmaktır. Ardından, etki alanı denetleyicisi sanal makinesi kurtarma planı uygulama için bir sınama yük devretmesi çalıştırmadan önce bir sınama yük devretmesi çalıştırın. Bunu nasıl aşağıda verilmiştir:

1. Site RECOVERY'yi kullanın [çoğaltmak](vmware-azure-tutorial.md) etki alanı denetleyicisi veya DNS barındıran sanal makine.
2. Yalıtılmış bir ağ oluşturun. Azure içinde oluşturduğunuz herhangi bir sanal ağ diğer ağlardan varsayılan olarak ayrı tutulur. Üretim ağınızda kullandığınız bu ağ için aynı IP adresi aralığı kullanmanızı öneririz. Bu ağ üzerinde siteden siteye bağlantı etkinleştirmeyin.
3. Yalıtılmış ağdaki bir DNS IP adresi sağlayın. Almak için DNS sanal makine beklediğiniz IP adresi kullanın. Azure'da çoğaltıyorsanız Yük Devretmesini kullanılan sanal makine için IP adresi verin. Çoğaltılmış sanal makinede IP adresi girmek için **işlem ve ağ** ayarları, select **hedef IP** ayarlar.

    ![Azure test ağı](./media/site-recovery-active-directory/azure-test-network.png)

    > [!TIP]
    > Site kurtarma denemeleri test sanal makineleri aynı ada sahip ve sağlanan aynı IP adresini kullanarak bir alt ağ oluşturmak **işlem ve ağ** sanal makinenin ayarlarını. Aynı ada sahip bir alt ağ yük devretme sınaması için sağlanan Azure sanal ağındaki kullanılabilir değilse, sınama sanal makinesini alfabetik olarak ilk alt ağ içinde oluşturulur.
    >
    > Hedef IP adresi seçilen alt ağın parçası ise, hedef IP adresini kullanarak test yük devretme sanal makine oluşturmak Site Recovery çalışır. Hedef IP seçilen alt ağının parçası değilse, test yük devretme sanal makinesi seçilen alt ağ içindeki bir sonraki kullanılabilir IP kullanılarak oluşturulur.
    >
    >

### <a name="test-failover-to-a-secondary-site"></a>İkincil siteye yük devretme sınaması

1. Başka bir şirket içi siteye çoğaltma yapıyorsanız ve DHCP kullanıyorsanız [DNS ve DHCP yük devretme sınaması için ayarlama](hyper-v-vmm-test-failover.md#prepare-dhcp).
2. Bir yük devretme testi yalıtılmış ağda çalışan etki alanı denetleyicisi sanal makine yapın. En son kullanılabilir kullanmak *uygulama tutarlı* yük devretme sınamasını yapmak için etki alanı denetleyicisi sanal makinenin kurtarma noktası.
3. Bir yük devretme sınaması için uygulamanın çalıştığı sanal makineler içeren kurtarma planı çalıştırın.
4. Test tamamlandığında, *yük devretme sınaması Temizleme* etki alanı denetleyicisi sanal makine üzerinde. Bu adım, yük devretme sınaması için oluşturulan etki alanı denetleyicisi siler.


### <a name="remove-references-to-other-domain-controllers"></a>Diğer etki alanı denetleyicilerine başvuruları kaldırın
Yük devretme testi başlattığınızda, tüm etki alanı denetleyicileri test ağında eklemeyin. Üretim ortamınızda mevcut diğer etki alanı denetleyicilerine başvuruları kaldırmak için gerekebilir [Active Directory FSMO rolleri](http://aka.ms/ad_seize_fsmo) ve [meta veri temizleme](https://technet.microsoft.com/library/cc816907.aspx) etki alanı denetleyicileri eksik .


### <a name="issues-caused-by-virtualization-safeguards"></a>Şunları yaparak sanallaştırma korumalarını nedeniyle oluşan sorunları

> [!IMPORTANT]
> Bu bölümde açıklanan yapılandırmalardan bazıları, standart veya varsayılan etki alanı denetleyicisi yapılandırması değildir. Üretim etki alanı denetleyicisi bu değişiklikleri yapmak istemiyorsanız, yük devretme sınaması için kullanılacak Site kurtarma için ayrılmış bir etki alanı denetleyicisi oluşturabilirsiniz. Bu değişiklikler yalnızca o etki alanı denetleyicisi yapın.  
>
>

Windows Server 2012 ile başlayan [ek güvenlik önlemleri, Active Directory etki alanı Hizmetleri (AD DS) içinde yerleşiktir](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100). Bu korumalar temel hiper yönetici platformuna destekliyorsa, sanallaştırılmış etki alanı denetleyicileri USN geri alma karşı korunmasına yardımcı olma **VM-Generationıd**. Azure destekler **VM-Generationıd**. Bu nedenle, Windows Server 2012 ya da daha sonra Azure sanal makineleri çalıştıran etki alanı denetleyicilerinin bu ek güvenlik önlemleri sahip.


Zaman **VM-Generationıd** sıfırlanır, **Invocationıd** değeri AD DS veritabanının, ayrıca sıfırlanır. Ayrıca, RID havuzu atılır ve SYSVOL'ü yetkisiz olarak işaretlenir. Daha fazla bilgi için bkz: [Active Directory etki alanı Hizmetleri sanallaştırma giriş](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100) ve [DFSR güvenli şekilde sanallaştırılmasını](https://blogs.technet.microsoft.com/filecab/2013/04/05/safely-virtualizing-dfsr/).

Azure yapabilmesini neden olabilecek **VM-Generationıd** sıfırlanır. Sıfırlama **VM-Generationıd** etki alanı denetleyicisi sanal makine Azure'da başladığında ek korumalarını tetikler. Bu sonuçlanabilir bir *önemli gecikme* etki alanı denetleyicisi sanal makinede oturum durum içinde.

Bu etki alanı denetleyicisi yalnızca bir test yük devretme kümesinde kullanıldığından, sanallaştırma korumalarını gerekli değildir. Emin olmak için **VM-Generationıd** etki alanı denetleyicisi sanal makine için değer değiştirmez, aşağıdaki DWORD değerini değiştirebilir **4** şirket içi etki alanı denetleyicisinde:


`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\gencounter\Start`


#### <a name="symptoms-of-virtualization-safeguards"></a>Sanallaştırma korumaları belirtileri

Bir test yük devretme sonrasında sanallaştırma korumaları tetiklenir, bir veya daha fazla aşağıdaki belirtilerden birini görebilirsiniz:  

* **Generationıd** değer değişiklikleri.

    ![Oluşturma kimliği değişikliği](./media/site-recovery-active-directory/Event2170.png)

* **Invocationıd** değer değişiklikleri.

    ![Çağırma kimliği değişikliği](./media/site-recovery-active-directory/Event1109.png)

* SYSVOL ve NETLOGON paylaşımları kullanılamaz.

    ![SYSVOL paylaşımı](./media/site-recovery-active-directory/sysvolshare.png)

    ![NtFrs SYSVOL](./media/site-recovery-active-directory/Event13565.png)

* DFSR veritabanlarını sildi.

    ![DFSR veritabanlarını sildi](./media/site-recovery-active-directory/Event2208.png)


### <a name="troubleshoot-domain-controller-issues-during-test-failover"></a>Yük devretme testi sırasında etki alanı denetleyicisi sorunlarını giderme

> [!IMPORTANT]
> Bu bölümde açıklanan yapılandırmalardan bazıları, standart veya varsayılan etki alanı denetleyicisi yapılandırması değil. Üretim etki alanı denetleyicisi bu değişiklikleri yapmak istemiyorsanız, Site Recovery yük devretme sınaması için ayrılmış bir etki alanı denetleyicisi oluşturabilirsiniz. Yalnızca bu özel etki alanı denetleyicisi değişiklikleri yapın.  
>
>

1. Komut isteminde paylaşılan SYSVOL ve NETLOGON klasörleri olup olmadığını denetlemek için aşağıdaki komutu çalıştırın:

    `NET SHARE`

2. Komut isteminde, etki alanı denetleyicisinin düzgün çalıştığından emin olmak için aşağıdaki komutu çalıştırın:

    `dcdiag /v > dcdiag.txt`

3. Çıktı günlüğünde aşağıdaki metni arayın. Metin, etki alanı denetleyicisinin düzgün çalıştığını doğrular.

    * "geçen test bağlantısı"
    * "geçen test reklam"
    * "geçen test MakineHesabı"

Yukarıdaki koşullar sağlanırsa, etki alanı denetleyicisinin düzgün olasıdır. Değilse, aşağıdaki adımları tamamlayın:

1. Etki alanı denetleyicisinin yetkili geri yükleme yapın. Aşağıdaki bilgileri unutmayın:
    * Öneririz yoktur ancak [FRS çoğaltma](https://blogs.technet.microsoft.com/filecab/2014/06/25/the-end-is-nigh-for-frs/), FRS çoğaltma kullanıyorsanız, yetkili geri yükleme adımlarını izleyin. İşlem açıklanan [dosya çoğaltma hizmeti yeniden başlatmak için BurFlags kayıt anahtarının kullanılması](https://support.microsoft.com/kb/290762).

        BurFlags hakkında daha fazla bilgi için blog gönderisine bakın [D2 ve D4: için nedir?](https://blogs.technet.microsoft.com/janelewis/2006/09/18/d2-and-d4-what-is-it-for/).
    * DFSR çoğaltma kullanırsanız, yetkili geri yükleme adımlarını tamamlayın. İşlem açıklanan [DFSR ile çoğaltılan SYSVOL (örneğin, "D4/D2" FRS için) bir yetkili ve yetkili olmayan eşitleme zorla](https://support.microsoft.com/kb/2218556).

        PowerShell işlevleri de kullanabilirsiniz. Daha fazla bilgi için bkz: [SYSVOL DFSR yetkili/yetkili olmayan geri yükleme PowerShell işlevleri](https://blogs.technet.microsoft.com/thbouche/2013/08/28/dfsr-sysvol-authoritative-non-authoritative-restore-powershell-functions/).

2. Aşağıdaki kayıt defteri anahtarını ayarlayarak ilk eşitleme gereksinimi atlama **0** şirket içi etki alanı denetleyicisinde. DWORD yoksa, bunun altında oluşturabilirsiniz **parametreleri** düğümü.

    `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters\Repl Perform Initial Synchronizations`

    Daha fazla bilgi için bkz: [DNS olay kimliği 4013 sorunlarını giderme: DNS sunucusu yükleyemedi AD tümleşik DNS bölgeleri](https://support.microsoft.com/kb/2001093).

3. Bir genel katalog sunucusu kullanıcı oturum açma doğrulamak kullanılabilir olması gereksinimini devre dışı bırakın. Bu, şirket içi etki alanı denetleyicisi yapmak için aşağıdaki kayıt defteri anahtarını ayarlamak **1**. DWORD yoksa, bunun altında oluşturabilirsiniz **Lsa** düğümü.

    `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\IgnoreGCFailures`

    Daha fazla bilgi için bkz: [bir genel katalog sunucusu kullanıcı oturumları doğrulamak kullanılabilir olması gereksinimini devre dışı bırakmak](http://support.microsoft.com/kb/241789).

### <a name="dns-and-domain-controller-on-different-machines"></a>Farklı makinelerde DNS ve etki alanı denetleyicisi
DNS etki alanı denetleyicisi ile aynı sanal makinede değilse, bir yük devretme sınaması için DNS sanal makine oluşturmanız gerekir. DNS ve etki alanı denetleyicisi aynı sanal makineye değilseniz, bu bölümü atlayabilirsiniz.

Yeni bir DNS sunucusu kullanın ve gerekli tüm bölgeler oluşturun. Örneğin, Active Directory etki alanı contoso.com ise, ile adı contoso.com DNS bölgesi oluşturabilirsiniz. Active Directory'ye karşılık gelen girişlerin DNS'de şu şekilde güncelleştirilmesi gerekir:

1. Herhangi bir sanal makine kurtarma planında başlamadan önce bu ayarları yerinde olduğundan emin olun:
   * Bölge, orman kök ada dayalı olarak adlandırılmış gerekir.
   * Bölge dosya yedekli olması gerekir.
   * Bölge için güvenli ve güvenli olmayan güncelleştirmeleri etkinleştirilmesi gerekir.
   * Etki alanı denetleyicisi barındıran sanal makinenin çözümleyici DNS sanal makine IP adresine işaret etmelidir.

2. Etki alanı denetleyicisi barındıran VM aşağıdaki komutu çalıştırın:

    `nltest /dsregdns`

3. Bir bölgenin DNS sunucusunda ekleme, güvenli güncelleştirmelere izin vermek ve için DNS bölge için bir giriş eklemek için aşağıdaki komutları çalıştırın:

    `dnscmd /zoneadd contoso.com  /Primary`

    `dnscmd /recordadd contoso.com  contoso.com. SOA %computername%.contoso.com. hostmaster. 1 15 10 1 1`

    `dnscmd /recordadd contoso.com %computername%  A <IP_OF_DNS_VM>`

    `dnscmd /config contoso.com /allowupdate 1`

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek [Azure Site Recovery ile kurumsal iş yüklerinin korunması](site-recovery-workload.md).
