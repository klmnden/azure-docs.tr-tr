---
title: Active Directory ve DNS Azure Site Recovery ile koruma | Microsoft Docs
description: "Bu makalede, Azure Site RECOVERY'yi kullanarak Active Directory için bir olağanüstü durum kurtarma çözümü uygulamak açıklar."
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: af1d9b26-1956-46ef-bd05-c545980b72dc
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 12/15/2017
ms.author: manayar
ms.openlocfilehash: 5db2d424c176428d31fb99fd8288120f18fcac7a
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2017
---
# <a name="protect-active-directory-and-dns-with-azure-site-recovery"></a>Active Directory ve DNS Azure Site Recovery ile koruma
SharePoint, Dynamics AX ve SAP gibi kurumsal uygulamalar düzgün çalışması için Active Directory ve DNS altyapısı bağlıdır. Uygulamalar için bir olağanüstü durum kurtarma çözümü oluşturduğunuzda, genellikle Active Directory ve DNS doğru uygulama işlevselliği sağlamak için diğer uygulama bileşenleri önce kurtarmanız gerekir.

Site Kurtarma'yı kullanarak, Active Directory için bir tam otomatik olağanüstü durum kurtarma planı oluşturabilirsiniz. Kesinti oluştuğunda yerden saniye içinde bir yük devretme başlatın ve Active Directory birkaç dakika içinde hazır ve çalışır alın. SharePoint ve SAP gibi birden çok uygulama için birincil sitenizde Active Directory dağıttığınıza ve yük devretme eksiksiz site istiyorsanız yük devretme Active Directory için ilk Site RECOVERY'yi kullanarak ve ardından kullanan diğer uygulamalar başarısız uygulamaya özgü kurtarma planları.

Bu makalede Active Directory için bir olağanüstü durum kurtarma çözümü oluşturma açıklanmaktadır tek tıklatmayla kurtarma planı, desteklenen yapılandırmalar ve önkoşullar kullanarak yük devretme işlemlerini gerçekleştirme.  Başlamadan önce Active Directory ve Azure Site Recovery konusunda bilgi sahibi olmanız gerekir.

## <a name="prerequisites"></a>Ön koşullar
* Microsoft Azure aboneliği kurtarma Hizmetleri kasasına.
* Azure'a çoğaltma yapıyorsanız [hazırlama](tutorial-prepare-azure.md) bir Azure aboneliği, Azure sanal ağı ve bir depolama hesabı gibi Azure kaynakları.
* Tüm bileşenler için destek gereksinimlerini gözden geçirin.

## <a name="replicating-domain-controller"></a>Çoğaltma etki alanı denetleyicisi

Kurulum için gereken [Site Recovery çoğaltma](#enable-protection-using-site-recovery) en az bir sanal makine etki alanı denetleyicisi ve DNS barındırma üzerinde. Varsa [birden çok etki alanı denetleyicileri](#environment-with-multiple-domain-controllers) ortamınızda, Site Recovery ile etki alanı denetleyicisi sanal makinenin çoğaltıldığını yanı sıra, ayarlamak de bir [ek etki alanı denetleyicisi](#protect-active-directory-with-active-directory-replication)hedef sitede (Azure veya bir ikincil şirket içi veri merkezine).

### <a name="single-domain-controller-environment"></a>Tek bir etki alanı denetleyici ortamı
Ardından bazı uygulamalar ve yalnızca bir tek etki alanı denetleyicisi varsa ve birlikte tüm site vermesine istediğiniz, etki alanı denetleyicisi (Azure veya bir ikincil şirket içi veri merkezine) hedef siteye çoğaltmak için Site RECOVERY'yi kullanarak öneririz. Aynı etki alanı denetleyicisinin/DNS sanal makine için kullanılabilir çoğaltılan [yük devretme sınamasını](#test-failover-considerations) de.

### <a name="environment-with-multiple-domain-controllers"></a>Birden çok etki alanı denetleyicileriyle ortamı
Birçok uygulama sahipseniz ve ortamında birden fazla etki alanı denetleyicisi yoktur veya birkaç uygulamalar üzerinde aynı anda başarısız planlıyorsanız, Site Recovery ile etki alanı denetleyicisi sanal makinenin çoğaltıldığını yanı sıra öneririz, ayrıca gerekir ayarlanmış bir [ek etki alanı denetleyicisi](#protect-active-directory-with-active-directory-replication) hedef sitede (Azure veya bir ikincil şirket içi veri merkezine). İçin [yük devretme sınamasını](#test-failover-considerations), Site Recovery tarafından ve yük devretme, hedef sitede ek etki alanı denetleyicisi için etki alanı denetleyicisini kullanabilirsiniz.

## <a name="enable-protection-using-site-recovery"></a>Site RECOVERY'yi kullanarak korumayı etkinleştir
### <a name="protect-the-virtual-machine"></a>Sanal makine koru
Etki alanı denetleyicisinin/DNS sanal makinesinin Site Recovery korumasını etkinleştirin. Site RECOVERY'yi kullanarak kopyalanan etki alanı denetleyicisi için kullanılan [yük devretme sınamasını](#test-failover-considerations). Aşağıdaki gereksinimleri karşıladığından emin olun:

1. Bir genel katalog sunucusu etki alanı denetleyicisidir
2. Etki alanı denetleyicisi FSMO rol sahibi yük devretme testi sırasında gerekli rolleri için olması gerekir (Bu rolleri başka olması gerekir [ele](http://aka.ms/ad_seize_fsmo) yük devretme sonrasında)

### <a name="configure-virtual-machine-network-settings"></a>Sanal makine ağ ayarlarını yapılandırma
Etki alanı denetleyicisinin/DNS sanal makine için Site Recovery çoğaltılan sanal makinenin işlem ve ağ ayarlarının altında ağ ayarlarını yapılandırın. Bu sanal makine yük devretme sonrasında doğru ağ iliştirilecek sağlamaktır.

## <a name="protect-active-directory-with-active-directory-replication"></a>Active Directory Active Directory çoğaltma ile koruma
### <a name="site-to-site-protection"></a>Siteden siteye koruma
İkincil sitede bir etki alanı denetleyicisi oluşturun. Bir etki alanı denetleyicisi rolünü sunucuya dağıttığınızda, birincil sitede kullanılan aynı etki alanının adını belirtin. Kullanabileceğiniz **Active Directory Siteleri ve Hizmetleri** siteler eklenir site bağlantı nesnesi ayarlarını yapılandırmak için ek bileşenini. Bir site bağlantısı ayarlarını yapılandırarak, iki veya daha fazla siteler arasında çoğaltma oluştuğunda denetleyebilir ve ne sıklıkta. Daha fazla bilgi için bkz: [siteler arasında çoğaltma zamanlama](https://technet.microsoft.com/library/cc731862.aspx).

### <a name="site-to-azure-protection"></a>Site Azure koruma
Yönergeleri izleyerek [bir Azure sanal ağındaki bir etki alanı denetleyicisi oluşturmak](../active-directory/active-directory-install-replica-active-directory-domain-controller.md). Bir etki alanı denetleyicisi rolünü sunucuya dağıttığınızda, birincil sitede kullanılan aynı etki alanı adı belirtin.

Ardından [sanal ağın DNS sunucusu yeniden](../active-directory/active-directory-install-replica-active-directory-domain-controller.md#reconfigure-dns-server-for-the-virtual-network), Azure'da DNS sunucusunu kullanacak şekilde.

![Azure Ağı](./media/site-recovery-active-directory/azure-network.png)

### <a name="azure-to-azure-protection"></a>Azure Azure koruma
Yönergeleri izleyerek [bir Azure sanal ağındaki bir etki alanı denetleyicisi oluşturmak](../active-directory/active-directory-install-replica-active-directory-domain-controller.md). Bir etki alanı denetleyicisi rolünü sunucuya dağıttığınızda, birincil sitede kullanılan aynı etki alanı adı belirtin.

Ardından [sanal ağın DNS sunucusu yeniden](../active-directory/active-directory-install-replica-active-directory-domain-controller.md#reconfigure-dns-server-for-the-virtual-network), Azure'da DNS sunucusunu kullanacak şekilde.

## <a name="test-failover-considerations"></a>Test yük devretme konuları
Üretim iş yükleri üzerindeki etkisini önlemek için üretim ağdan yalıtılmış bir ağda yük devretme testi oluşur.

Çoğu uygulama aynı zamanda bir etki alanı denetleyicisi ve DNS sunucusunun işlevi bulunması gerekir. Uygulama devredilen önce bu nedenle, bir etki alanı denetleyicisi yük devretme sınaması için kullanılacak yalıtılmış ağ oluşturulması gerekir. Bunu yapmak için kolay bir etki alanı denetleyicisinin/DNS sanal makine Site Recovery ile çoğaltmak için yoludur. Daha sonra kurtarma planı uygulama için bir sınama yük devretmesi çalıştırmadan önce etki alanı denetleyicisi sanal makineye bir sınama yük devretmesi çalıştırın. Bunu nasıl aşağıda verilmiştir:

1. [Çoğaltma](site-recovery-replicate-vmware-to-azure.md) Site Recovery kullanarak etki alanı denetleyicisinin/DNS sanal makine.
2. Yalıtılmış bir ağ oluşturun. Varsayılan olarak Azure içinde oluşturulan herhangi bir sanal ağ diğer ağlardan yalıtılır. Bu ağ için IP adresi aralığı, üretim ağınıza aynı olduğunu öneririz. Bu ağ üzerinde siteden siteye bağlantı etkinleştirmeyin.
3. Oluşturulan, almak için DNS sanal makine beklediğiniz IP adresi olarak ağdaki bir DNS IP adresi sağlayın. Azure'a çoğaltma yapıyorsanız, IP adresi için yük devretme kümesinde kullanılan VM sağlayın **hedef IP** ayarı **işlem ve ağ** çoğaltılan sanal makinenin ayarlarını.

    ![Azure Test ağı](./media/site-recovery-active-directory/azure-test-network.png)

> [!TIP]
> Site kurtarma denemeleri test sanal makineleri aynı ad ve aynı IP sağlanan olarak kullanarak bir alt ağ oluşturmak **işlem ve ağ** sanal makinenin ayarlarını. Ardından aynı adlı bir alt ağ yük devretme sınaması için sağlanan Azure sanal ağda kullanılabilir durumda değilse, test sanal makine ağdaki ilk alt alfabetik olarak oluşturulur. Hedef IP seçilen alt ağın parçası ise, Site Recovery, hedef IP kullanarak test yük devretme sanal makine oluşturmaya çalışır. Hedef IP seçilen alt ağının parçası değilse, ardından test yük devretme sanal makine sonraki kullanılabilir IP seçilen alt ağ kullanılarak oluşturulan.
>
>


1. Başka bir şirket içi siteye çoğaltma yapıyorsanız ve DHCP kullanıyorsanız, yönergeleri izleyin [DNS ve DHCP yük devretme sınaması için Kur](site-recovery-test-failover-vmm-to-vmm.md#prepare-dhcp)
2. Yük devretme testi yalıtılmış ağda çalıştırmak etki alanı denetleyicisi sanal makinenin yapın. Kullanım en son kullanılabilir **uygulama tutarlı** yük devretme sınamasını yapmak için etki alanı denetleyicisi sanal makinenin kurtarma noktası.
3. Bir yük devretme sınaması için uygulamanın sanal makineleri içeren kurtarma planı çalıştırın.
4. Test tamamlandıktan sonra **temizleme yük devretme testi** etki alanı denetleyicisi sanal makine üzerinde. Bu adım, yük devretme sınaması için oluşturulan etki alanı denetleyicisi siler.


### <a name="removing-reference-to-other-domain-controllers"></a>Başvuru diğer etki alanı denetleyicilerine kaldırma
Bir test yük devretme yapılırken, tüm etki alanı denetleyicileri test ağında Getir yok. Üretim ortamınızda mevcut diğer etki alanı denetleyicilerine başvurusunu kaldırmak için gerekebilir [Active Directory FSMO rolleri](http://aka.ms/ad_seize_fsmo) ve [meta veri temizleme](https://technet.microsoft.com/library/cc816907.aspx) eksik etki alanı için denetleyicileri.



> [!IMPORTANT]
> Aşağıdaki bölümde açıklanan yapılandırmalardan bazıları, standart/varsayılan etki alanı denetleyicisi yapılandırması değildir. Üretim etki alanı denetleyicisi bu değişiklikleri yapmak istemiyorsanız, daha sonra bir etki alanı denetleyicisi için bu değişiklikleri yapmak ve Site Recovery yük devretme sınaması için kullanılabilecek atanmış oluşturabilirsiniz.  
>
>

### <a name="issues-because-of-virtualization-safeguards"></a>Sorunları nedeniyle sanallaştırma korumaları

Windows Server 2012 ile başlayan [ek güvenlik önlemleri, Active Directory etki alanı hizmetlerine inşa](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100). Temel alınan hiper yönetici platformunun VM-Generationıd'yi destekleyen sürece bu korumalar USN geri alma, sanallaştırılmış etki alanı denetleyicilerinde koruyun. Azure, Windows Server 2012 ya da daha sonra Azure sanal makineleri çalıştıran etki alanı denetleyicileri ek güvenlik önlemleri sahip VM-Generationıd'yi destekleyen.


VM-Generationıd sıfırladığınızda, çağırma kimliği AD DS veritabanının da sıfırlanır, RID havuzu atılır ve SYSVOL'ü yetkisiz olarak işaretlenir. Daha fazla bilgi için bkz: [Active Directory etki alanı Hizmetleri sanallaştırma giriş](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100) ve [DFSR'yi sanallaştırılması](https://blogs.technet.microsoft.com/filecab/2013/04/05/safely-virtualizing-dfsr/)

Yapabilmesini Azure VM-Generationıd sıfırlama neden olabilir ve etki alanı denetleyicisi sanal makine Azure'da başladığında, ek güvenlik önlemleri içinde başlatır. Bu neden bir **önemli gecikme** etki alanı denetleyicisi sanal makineye oturum açma yetkisi olan kullanıcı. Bu etki alanı denetleyicisi yalnızca bir test yük devretme kümesinde kullanılacak olduğundan, sanallaştırma korumalarını gerekli değildir. VM-Generationıd etki alanı denetleyicisi sanal makine için değiştirmez, ardından 4 şirket içi etki alanı denetleyicisinde aşağıdaki DWORD değerini değiştirebilir sağlamak için.


        HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\gencounter\Start


#### <a name="symptoms-of-virtualization-safeguards"></a>Sanallaştırma korumaları belirtileri

Sanallaştırma korumaları test yük devretme sonrasında koparılan değilse, bir veya daha fazla aşağıdaki belirtilerden birini görebilirsiniz:  

Oluşturma kimliği değişikliği

![Oluşturma kimliği değişikliği](./media/site-recovery-active-directory/Event2170.png)

Çağırma kimliği değişikliği

![Çağırma kimliği değişikliği](./media/site-recovery-active-directory/Event1109.png)

SYSVOL ve Netlogon paylaşımları kullanılabilir değil

![Sysvol paylaşımı](./media/site-recovery-active-directory/sysvolshare.png)

![Ntfrs Sysvol](./media/site-recovery-active-directory/Event13565.png)

Tüm DFSR veritabanlarını sildi

![DFSR DB Sil](./media/site-recovery-active-directory/Event2208.png)


> [!IMPORTANT]
> Aşağıdaki bölümde açıklanan yapılandırmalardan bazıları, standart/varsayılan etki alanı denetleyicisi yapılandırması değildir. Üretim etki alanı denetleyicisi bu değişiklikleri yapmak istemiyorsanız, daha sonra bir etki alanı denetleyicisi için bu değişiklikleri yapmak ve Site Recovery yük devretme sınaması için kullanılabilecek atanmış oluşturabilirsiniz.  
>
>


### <a name="troubleshooting-domain-controller-issues-during-test-failover"></a>Yük devretme testi sırasında etki alanı denetleyicisi sorunlarını giderme


Bir komut isteminde, paylaşılan klasörler SYSVOL ve NETLOGON olup olmadığını denetlemek için aşağıdaki komutu çalıştırın:

    NET SHARE

Komut isteminde, etki alanı denetleyicisinin düzgün çalıştığından emin olmak için aşağıdaki komutu çalıştırın.

    dcdiag /v > dcdiag.txt

Etki alanı denetleyicisinin düzgün çalıştığını doğrulamak aşağıdaki metni çıktı günlüğüne bakın.

* "geçen test bağlantısı"
* "geçen test reklam"
* "geçen test MakineHesabı"

Yukarıdaki koşullar sağlanırsa, etki alanı denetleyicisinin düzgün olasıdır. Aksi durumda, aşağıdaki adımları deneyin.


* Etki alanı denetleyicisinin yetkili geri yükleme yapın.
    * Bu olmasına rağmen [FRS çoğaltma kullanmak için önerilmez](https://blogs.technet.microsoft.com/filecab/2014/06/25/the-end-is-nigh-for-frs/), ancak hala kullanıyorsanız sonra sağlanan adımları izleyin [burada](https://support.microsoft.com/kb/290762) yetkili geri yükleme yapmak için. Burflags hakkında daha fazla açıklandı hakkında önceki bağlantıyı okuyabilirsiniz [burada](https://blogs.technet.microsoft.com/janelewis/2006/09/18/d2-and-d4-what-is-it-for/).
    * DFSR çoğaltma kullanıyorsanız, kullanılabilir adımları [burada](https://support.microsoft.com/kb/2218556) yetkili geri yükleme yapmak için. Ayrıca Powershell işlevleri bu kullanabilirsiniz [bağlantı](https://blogs.technet.microsoft.com/thbouche/2013/08/28/dfsr-sysvol-authoritative-non-authoritative-restore-powershell-functions/) bu amaç için.

* İlk eşitleme gereksinimi 0 şirket içi etki alanı denetleyicisi olarak aşağıdaki kayıt defteri anahtarını ayarlayarak atlama. Bu DWORD yoksa, daha sonra onu düğümünde 'Parameters' oluşturabilirsiniz. Daha fazla bilgiyi ilgili [burada](https://support.microsoft.com/kb/2001093)

        HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters\Repl Perform Initial Synchronizations

* Bir genel katalog sunucusu kayıt defteri anahtarını 1 şirket içi etki alanı denetleyicisi olarak aşağıdaki ayarlayarak kullanıcı oturum açma doğrulamak kullanılabilir olduğunu gereksinimini devre dışı bırakın. Ardından bu DWORD yoksa, bunu 'Lsa' düğümünde oluşturabilirsiniz. Daha fazla bilgiyi ilgili [burada](http://support.microsoft.com/kb/241789)

        HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\IgnoreGCFailures



### <a name="dns-and-domain-controller-on-different-machines"></a>Farklı makinelerde DNS ve etki alanı denetleyicisi
DNS etki alanı denetleyicisi ile aynı sanal makinede değilse, yük devretme sınaması için DNS VM oluşturmanız gerekir. Bunlar aynı VM'de değilseniz, bu bölümü atlayabilirsiniz.

Yeni bir DNS sunucusu kullanın ve gerekli tüm bölgeler oluşturun. Örneğin, Active Directory etki alanı contoso.com ise, ile adı contoso.com DNS bölgesi oluşturabilirsiniz. Active Directory ile ilgili girdileri DNS'de şu şekilde güncelleştirilmesi gerekir:

1. Herhangi bir sanal makine kurtarma planında gelir önce bu ayarları yerine getirildiğinden emin olun:

   * Bölge, orman kök ada dayalı olarak adlandırılmış gerekir.
   * Bölge dosya yedekli olması gerekir.
   * Bölge için güvenli ve güvenli olmayan güncelleştirmeleri etkinleştirilmesi gerekir.
   * Etki alanı denetleyicisi sanal makine çözümleyici DNS sanal makine IP adresine işaret etmelidir.
2. Etki alanı denetleyicisi sanal makinede aşağıdaki komutu çalıştırın:

    `nltest /dsregdns`
3. Bir bölgenin DNS sunucusunda ekleme, güvenli olmayan güncelleştirmelere izin vermek ve bir giriş için DNS'ye ekleyin:

        dnscmd /zoneadd contoso.com  /Primary
        dnscmd /recordadd contoso.com  contoso.com. SOA %computername%.contoso.com. hostmaster. 1 15 10 1 1
        dnscmd /recordadd contoso.com %computername%  A <IP_OF_DNS_VM>
        dnscmd /config contoso.com /allowupdate 1

## <a name="next-steps"></a>Sonraki adımlar
[Daha fazla bilgi edinin](site-recovery-workload.md) Azure Site Recovery ile kurumsal iş yüklerini koruma hakkında.
