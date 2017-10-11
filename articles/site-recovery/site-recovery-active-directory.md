---
title: Active Directory ve DNS Azure Site Recovery ile koruma | Microsoft Docs
description: "Bu makalede, Azure Site RECOVERY'yi kullanarak Active Directory için bir olağanüstü durum kurtarma çözümü uygulamak açıklar."
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: af1d9b26-1956-46ef-bd05-c545980b72dc
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 7/20/2017
ms.author: pratshar
ms.openlocfilehash: 197441fc24c178695d4eada6db59f503b21672ad
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="protect-active-directory-and-dns-with-azure-site-recovery"></a>Active Directory ve DNS Azure Site Recovery ile koruma
SharePoint, Dynamics AX ve SAP gibi kurumsal uygulamalar düzgün çalışması için Active Directory ve DNS altyapısı bağlıdır. Uygulamalar için bir olağanüstü durum kurtarma çözümü oluşturduğunuzda korumak ve olağanüstü durum oluştuğunda şeyler düzgün çalışmasını sağlamak için diğer uygulama bileşenleri önce Active Directory ve DNS kurtarmak gerektiğini unutmayın önemlidir.

Site kurtarma, çoğaltma, yük devretme ve kurtarma sanal makinelerin düzenleyerek olağanüstü durum kurtarma sağlayan bir Azure hizmetidir. Site Recovery tutarlı bir şekilde korumak için çoğaltma senaryolar ve sorunsuz bir şekilde yük devretme sanal makineler ve özel, genel veya barındırma sağlayıcısı bulut uygulamaları destekler.

Site Kurtarma'yı kullanarak, Active Directory için bir tam otomatik olağanüstü durum kurtarma planı oluşturabilirsiniz. Kesinti oluştuğunda yerden saniye içinde bir yük devretme başlatın ve Active Directory birkaç dakika içinde hazır ve çalışır alın. SharePoint ve SAP gibi birden çok uygulama için birincil sitenizde Active Directory dağıttığınıza ve eksiksiz site vermesine istiyorsanız Active Directory edilemeyebilir ilk Site RECOVERY'yi kullanarak ve ardından kullanan diğer uygulamalar başarısız uygulamaya özgü kurtarma planları.

Bu makalede, Active Directory, planlanmış, Planlanmayan gerçekleştirme ve test yük devretmeleri tek tıklatmayla kurtarma planı, desteklenen yapılandırmalar ve önkoşullar kullanarak bir olağanüstü durum kurtarma çözümü oluşturma açıklanmaktadır.  Başlamadan önce Active Directory ve Azure Site Recovery konusunda bilgi sahibi olmanız gerekir.

## <a name="replicating-domain-controller"></a>Çoğaltma etki alanı denetleyicisi

Kurulum için gereken [Site Recovery çoğaltma](#enable-protection-using-site-recovery) en az bir sanal makine etki alanı denetleyicisi ve DNS barındırma üzerinde. Varsa [birden çok etki alanı denetleyicileri](#environment-with-multiple-domain-controllers) ortamınızda, Site Recovery ile etki alanı denetleyicisi sanal makinenin çoğaltıldığını yanı sıra da ayarlamak olurdu bir [ek etki alanı denetleyicisi](#protect-active-directory-with-active-directory-replication) hedef sitede (Azure veya bir ikincil şirket içi veri merkezine). 

### <a name="single-domain-controller-environment"></a>Tek bir etki alanı denetleyici ortamı
Bazı uygulamalar ve yalnızca bir tek etki alanı denetleyicisi varsa ve tüm site birlikte çalışmak isterseniz, sonra etki alanı denetleyicisi (olup, Azure'a veya ikincil bir devretmek ikincil siteye çoğaltmak için Site RECOVERY'yi kullanarak öneririz Site). Aynı etki alanı denetleyicisinin/DNS sanal makine için kullanılabilir çoğaltılan [yük devretme sınamasını](#test-failover-considerations) de.

### <a name="environment-with-multiple-domain-controllers"></a>Birden çok etki alanı denetleyicileriyle ortamı
Birçok uygulama varsa ve ortamında birden fazla etki alanı denetleyicisi yoktur veya birkaç uygulamalar üzerinde aynı anda başarısız planlıyorsanız, Site Recovery ile etki alanı denetleyicisi sanal makinenin çoğaltıldığını yanı sıra da ayarlamanız önerilir bir [ek etki alanı denetleyicisi](#protect-active-directory-with-active-directory-replication) hedef sitede (Azure veya bir ikincil şirket içi veri merkezine). İçin [yük devretme sınamasını](#test-failover-considerations), Site Recovery tarafından ve yük devretme, hedef sitede ek etki alanı denetleyicisi için etki alanı denetleyicisi kullanın. 


Aşağıdaki bölümlerde, Site Recovery, etki alanı denetleyicisi için korumayı etkinleştirme ve bir etki alanı denetleyicisi azure'da ayarlamak nasıl açıklanmıştır.

## <a name="prerequisites"></a>Ön koşullar
* Active Directory ve DNS sunucusu bir şirket içi dağıtımı.
* Bir Microsoft Azure aboneliği Azure Site kurtarma Hizmetleri kasasına.
* Azure'da çoğaltıyorsanız Azure Vm'leri ve Azure Site kurtarma Hizmetleri ile uyumlu olup olmadıklarını sağlamak için sanal makineleri üzerinde Azure sanal makine hazırlık Değerlendirme Aracı'nı çalıştırın.

## <a name="enable-protection-using-site-recovery"></a>Site RECOVERY'yi kullanarak korumayı etkinleştir
### <a name="protect-the-virtual-machine"></a>Sanal makine koru
Etki alanı denetleyicisinin/DNS sanal makinesinin Site Recovery korumasını etkinleştirin. Sanal makine türüne göre (Hyper-V veya VMware) Site Recovery ayarlarını yapılandırın. Site RECOVERY'yi kullanarak kopyalanan etki alanı denetleyicisi için kullanılan [yük devretme sınamasını](#test-failover-considerations). Aşağıdaki gereksinimleri karşıladığından emin olun:

1. Bir genel katalog sunucusu etki alanı denetleyicisidir
2. Etki alanı denetleyicisi FSMO rol sahibi yük devretme testi sırasında gerekli rolleri için olması gerekir (Bu rolleri başka olması gerekir [ele](http://aka.ms/ad_seize_fsmo) yük devretme sonrasında)

### <a name="configure-virtual-machine-network-settings"></a>Sanal makine ağ ayarlarını yapılandırma
Sanal makine için doğru ağ yük devretme sonrasında eklenecek böylece etki alanı denetleyicisinin/DNS sanal makine için Site Recovery ağ ayarlarını yapılandırın. 

![VM ağ ayarları](./media/site-recovery-active-directory/DNS-Target-IP.png)

## <a name="protect-active-directory-with-active-directory-replication"></a>Active Directory Active Directory çoğaltma ile koruma
### <a name="site-to-site-protection"></a>Siteden siteye koruma
İkincil sitede bir etki alanı denetleyicisi oluşturun. Bir etki alanı denetleyicisi rolünü sunucuya dağıttığınızda, birincil sitede kullanılan aynı etki alanının adını belirtin. Kullanabileceğiniz **Active Directory Siteleri ve Hizmetleri** siteler eklenir site bağlantı nesnesi ayarlarını yapılandırmak için ek bileşenini. Bir site bağlantısı ayarlarını yapılandırarak, iki veya daha fazla siteler arasında çoğaltma oluştuğunda denetleyebilir ve ne sıklıkta. Daha fazla bilgi için bkz: [siteler arasında çoğaltma zamanlama](https://technet.microsoft.com/library/cc731862.aspx).

### <a name="site-to-azure-protection"></a>Site Azure koruma
Yönergeleri izleyerek [bir Azure sanal ağındaki bir etki alanı denetleyicisi oluşturmak](../active-directory/active-directory-install-replica-active-directory-domain-controller.md). Bir etki alanı denetleyicisi rolünü sunucuya dağıttığınızda, birincil sitede kullanılan aynı etki alanı adı belirtin.

Ardından [sanal ağın DNS sunucusu yeniden](../active-directory/active-directory-install-replica-active-directory-domain-controller.md#reconfigure-dns-server-for-the-virtual-network), Azure'da DNS sunucusunu kullanacak şekilde.

![Azure Ağı](./media/site-recovery-active-directory/azure-network.png)

**Azure üretim ağındaki DNS**

## <a name="test-failover-considerations"></a>Test yük devretme konuları
Üretim iş yükleri üzerinde hiçbir etkisi yani üretim ağınızdan yalıtılmış olan bir ağda yük devretme testi oluşur.

Çoğu uygulama aynı zamanda bir etki alanı denetleyicisi ve DNS sunucusunun işlevi bulunması gerekir. Uygulama devredilen önce bu nedenle, bir etki alanı denetleyicisi yük devretme sınaması için kullanılacak yalıtılmış ağ oluşturulması gerekir. Bunu yapmak için kolay bir etki alanı denetleyicisinin/DNS sanal makine Site Recovery ile çoğaltmak için yoludur. Daha sonra kurtarma planı uygulama için bir sınama yük devretmesi çalıştırmadan önce etki alanı denetleyicisi sanal makineye bir sınama yük devretmesi çalıştırın. Bunu nasıl aşağıda verilmiştir:

1. [Çoğaltma](site-recovery-replicate-vmware-to-azure.md) Site Recovery kullanarak etki alanı denetleyicisinin/DNS sanal makine.
1. Yalıtılmış bir ağ oluşturun. Varsayılan olarak Azure içinde oluşturulan herhangi bir sanal ağ diğer ağlardan yalıtılır. Bu ağ için IP adresi aralığı, üretim ağınıza aynı olduğunu öneririz. Bu ağ üzerinde siteden siteye bağlantı etkinleştirmeyin.
1. Oluşturulan, almak için DNS sanal makine beklediğiniz IP adresi olarak ağdaki bir DNS IP adresi sağlayın. Azure'a çoğaltma yapıyorsanız, IP adresi için yük devretme kümesinde kullanılan VM sağlayın **hedef IP** ayarı **işlem ve ağ** ayarlar. 

    ![Hedef IP](./media/site-recovery-active-directory/DNS-Target-IP.png) **hedef IP**

    ![Azure Test ağı](./media/site-recovery-active-directory/azure-test-network.png)

    **Azure Test ağındaki DNS**

> [!TIP]
> Site kurtarma denemeleri test sanal makineleri aynı ad ve aynı IP sağlanan olarak kullanarak bir alt ağ oluşturmak **işlem ve ağ** sanal makinenin ayarlarını. Ardından aynı adlı alt ağ yük devretme sınaması için sağlanan Azure sanal ağda kullanılabilir durumda değilse, test sanal makine ağdaki ilk alt alfabetik olarak oluşturulur. Hedef IP seçilen alt ağın parçası ise, Site Recovery, hedef IP kullanarak test yük devretme sanal makine oluşturmaya çalışır. Hedef IP seçilen alt ağının parçası değilse, test yük devretme sanal makine seçilen alt ağında kullanılabilir herhangi bir IP'yi kullanılarak oluşturulur. 
>
>


1. Başka bir şirket içi siteye çoğaltma yapıyorsanız ve DHCP kullanıyorsanız, yönergeleri izleyin [DNS ve DHCP yük devretme sınaması için Kur](site-recovery-test-failover-vmm-to-vmm.md#prepare-dhcp)
1. Yük devretme testi yalıtılmış ağda çalıştırmak etki alanı denetleyicisi sanal makinenin yapın. Kullanım en son kullanılabilir **uygulama tutarlı** yük devretme sınamasını yapmak için etki alanı denetleyicisi sanal makinenin kurtarma noktası. 
1. Bir yük devretme sınaması için uygulamanın sanal makineleri içeren kurtarma planı çalıştırın. 
1. Test tamamlandıktan sonra **temizleme yük devretme testi** etki alanı denetleyicisi sanal makine üzerinde. Bu adım, yük devretme sınaması için oluşturulan etki alanı denetleyicisi siler.


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
Okuma [hangi iş yüklerini koruyabilirim?](site-recovery-workload.md) Azure Site Recovery ile kurumsal iş yüklerini koruma hakkında daha fazla bilgi edinmek için.

