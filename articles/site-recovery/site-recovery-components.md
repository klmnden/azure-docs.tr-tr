<properties
    pageTitle="Site Recovery nasıl çalışır? | Microsoft Azure"
    description="Bu makalede Site Recovery mimarisine genel bir bakış sağlanır."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="03/27/2016"
    ms.author="raynew"/>

# Azure Site Recovery nasıl çalışır?

Azure Site Recovery hizmetinin temel mimarisini ve bu hizmetin çalışmasını sağlayan bileşenleri anlamak için bu makaleyi okuyun. 

Tüm yorumlarınızı ve sorularınızı bu makalenin alt kısmında veya [Azure Kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)'nda paylaşabilirsiniz.


## Genel Bakış

Kuruluşlar; uygulamaların, iş yüklerinin ve verilerin planlanmış ve planlanmamış kesinti süreleri içinde nasıl kullanılabilir durumda kaldığını ve mümkün olan en kısa zamanda normal çalışma koşullarına nasıl döndüğünü belirleyen bir iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejisine ihtiyaç duyar.

Site Recovery, şirket içi fiziksel sunucuların ve sanal makinelerin buluta (Azure) veya ikincil bir siteye çoğaltılmasını düzenleyerek BCDR stratejinize katkı sağlayan bir Azure hizmetidir. Kesinti birincil konumunuzda meydana gelirse uygulamaları ve iş yüklerini kullanılabilir durumda tutmak için ikincil siteye yük devredersiniz. Normal çalışma koşullarına dönüldüğünde de yükü birincil konuma geri alabilirsiniz.

Site Recovery, birçok senaryoda çoğaltmayı düzenlemek için dağıtılabilir:

- **VMware sanal makinelerini çoğaltma**: Şirket içi VMware sanal makinelerini [Azure](site-recovery-vmware-to-azure-classic.md)'a veya [ikincil veri merkezine](site-recovery-vmware-to-vmware.md) çoğaltabilirsiniz.
- **Fiziksel makineleri çoğaltma**: Windows veya Linux çalıştıran fiziksel makineleri [Azure](site-recovery-vmware-to-azure-classic.md)'a veya [ikincil veri merkezine](site-recovery-vmware-to-vmware.md) çoğaltabilirsiniz.
- **System Center VMM bulutlarında yönetilen Hyper-V VM'lerini çoğaltma**: VMM bulutlarındaki şirket içi Hyper-V sanal makinelerini [Azure](site-recovery-vmm-to-azure.md)'a veya [ikincil veri merkezine](site-recovery-vmm-to-vmm.md) çoğaltabilirsiniz. 
- **Hyper-V VM'lerini (VMM olmadan) çoğaltma**: VMM tarafından yönetilmeyen Hyper-V VM'lerini [Azure](site-recovery-hyper-v-site-to-azure.md)'a çoğaltabilirsiniz.
- **VM'leri geçirme**: [Azure IaaS VM'leri](site-recovery-migrate-azure-to-azure.md) bölgeler arasında geçirmek veya [AWS Windows örneklerini](site-recovery-migrate-aws-to-azure.md) Azure IaaS VM'lerine geçirmek için Site Recovery'yi kullanabilirsiniz. Şu anda yalnızca bu VM'lere yük aktarımı işlevini gerçekleştiren ancak yeniden çalıştırılmayan geçirme işlemleri desteklenir.

Site Recovery, bu VM'lerde ve fiziksel sunucularda çalışan çoğu uygulamayı çoğaltabilir. [Azure Site Recovery hangi iş yüklerini koruyabilir?](site-recovery-workload.md) bölümünden desteklenen uygulamaların tam özetine ulaşabilirsiniz.

## Şirket içi VMware sanal makinelerini/fiziksel sunucularını Azure'a çoğaltma

Şu anda Azure'da VMware VM'lerini veya fiziksel Windows/Linux sunucularını çoğaltmak için iki farklı mimari sunulmaktadır:

- [Eski mimari](site-recovery-vmware-to-azure-classic-legacy.md): Bu mimarinin yeni dağıtımlar için kullanılmaması gerekir. 
- [Gelişmiş mimari](site-recovery-vmware-to-azure-classic.md): Bu en son mimaridir ve tüm yeni dağıtımlar için kullanılması gerekir. Bu senaryoyu eski mimariyi kullanarak dağıttıysanız gelişmiş dağıtıma [geçiş hakkında bilgi edinin](site-recovery-vmware-to-azure-classic-legacy.md#migrate-to-the-enhanced-deployment).

Gelişmiş dağıtımda, tüm Site Recovery bileşenlerini içeren bir şirket içi yönetim sunucusu ayarlamanız gerekir. Korumak istediğiniz her makinede otomatik olarak (veya elle yükleyerek) Mobility hizmetini gönderirsiniz. İlk çoğaltmadan sonra makinedeki Mobility hizmeti, çoğaltma değişim verilerini işlem sunucusuna gönderir. İşlem sunucusu da bu verileri Azure depolama alanına göndermeden önce iyileştirir.

![Gelişmiş](./media/site-recovery-components/arch-enhanced.png)
![Gelişmiş](./media/site-recovery-components/arch-enhanced2.png)

### Şirket içi
İşte şirket içinde ihtiyaç duyacaklarınız:

- **Yönetim sunucusu**: Yönetim sunucusu olarak davranacak şekilde bir Windows Server 2012 R2 makinesi gerekir. Bu sunucuda tek bir yükleme dosyasıyla Site Recovery'nin şu bileşenlerini yükleyeceksiniz:

    - **Yapılandırma sunucusu bileşeni**: Şirket içi ortamınız ile Azure arasındaki iletişimi düzenler ve veri çoğaltma ile kurtarma işlemlerini yönetir.
    - **İşlem sunucusu bileşeni**: Çoğaltma ağ geçidi olarak davranır. Korumalı kaynak makinelerden çoğaltma verilerini alıp bu verileri önbelleğe alma, sıkıştırma ve şifreleme işlemleriyle iyileştirir ve Azure depolama alanına gönderir. Ayrıca Mobility hizmetinin korunan makinelere göndermeli yüklemesini ele alır ve VMware VM'lerinin otomatik olarak bulunmasını gerçekleştirir. Dağıtımınız büyüdükçe artan çoğaltma trafiği hacimlerini idare etmek için ilave olarak ayrı ayrı adanmış işlem sunucuları ekleyebilirsiniz.
    - **Ana hedef sunucusu bileşeni**: Azure'dan yeniden çalışma sırasında çoğaltma verilerini işler. 
- **VMware ESX/ESXi ana bilgisayarları ve vCenter sunucusu**: VMware VM'lerini çalıştıran bir veya daha fazla ESX/ESXi ana bilgisayar sunucusuna sahip olmanız gerekir. Bu ana bilgisayarları yönetmek için bir vCenter sunucusu dağıtmanızı öneririz. **Not:** **Fiziksel sunucuları çoğaltıyor olsanız bile bunları VMware'de yeniden çalıştırmanız gerekir.**. Bir fiziksel sunucu çoğaltırken, Azure'a yük devrettiğinizde bu sunucu bir Azure VM gibi çalışır. Mantıksal sunucu şirket içine bir VMware gibi geri dönecektir. 
    
- **VM'ler/fiziksel sunucular**: Azure'a çoğaltmak istediğiniz her makinede Mobility hizmeti bileşeninin yüklü olması gerekir. Bu hizmet verileri yakalar, makinede yazar ve işlem sunucusuna gönderir. Bu bileşen elle yüklenebilir veya bir makine için çoğaltma işlemini etkinleştirdiğinizde işlem sunucusu tarafından otomatik olarak gönderilip yüklenebilir.

### Azure

İşte Azure altyapısı için gerekenler:     - **Azure hesabı**: Bir Microsoft Azure hesabına sahip olmanız gerekir.
    - **Azure depolama alanı**: Çoğaltılan verileri depolamak için bir Azure depolama alanı hesabınızın olması gerekir. Çoğaltılan veriler Azure depolama alanında depolanır ve yük devretme durumunda Azure VM'leri çalışmaya başlar. 
    - **Azure ağı**: Yük devretme durumunda çalışmaya başlayan Azure VM'lerinin bağlanması için bir Azure sanal ağınızın olması gerekir. 
    
    
### Yeniden çalışma

Fiziksel bir sunucuya yük devretseniz bile şirket içi yeniden çalışma her zaman VMware VM'lerinde gerçekleşir. İşte gerekenler:

- **Azure'da geçici işlem sunucusu**: Yük devretmeden sonra Azure'da yeniden çalıştırma işlemini gerçekleştirmek isterseniz Azure'dan çoğaltmayı düzenlemek için işlem sunucusu olarak yapılandırılan bir Azure VM ayarlamanız gerekir. Yeniden çalışma sona erdikten sonra bu VM'yi silebilirsiniz.
- **VPN bağlantısı**: Yeniden çalışma için, Azure ağından şirket içi siteye olacak şekilde ayarlanmış bir VPN bağlantısına (veya Azure ExpressRoute) sahip olmanız gerekir.
- **Ayrı şirket içi ana hedef sunucusu**: Şirket içi ana hedef sunucusu yeniden çalışma işlemini yürütür. Ana hedef sunucusu varsayılan olarak yönetim sunucusunda yüklüdür. Ancak daha fazla hacimli trafikte yeniden çalıştırma işlemini gerçekleştiriyorsanız bu işlem için ayrı bir şirket içi ana hedef sunucusu ayarlamanız gerekir. 

![Gelişmiş yeniden çalışma](./media/site-recovery-components/enhanced-failback.png)

Gelişmiş dağıtım gereksinimleri hakkında [daha fazla bilgi edinin](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment).
Gelişmiş dağıtıma yönelik yeniden çalışma hakkında [daha fazla bilgi edin](site-recovery-failback-azure-to-vmware-classic.md).




## VMM bulutlarındaki Hyper-V VM'lerini Azure'a çoğaltma

Site Recovery dağıtımı sırasında bu senaryoyu dağıtmak için VMM sunucusuna Azure Site Recovery Sağlayıcısı'nı yükleyeceksiniz. Sağlayıcı, internet üzerinden Site Recovery hizmetiyle gerçekleştirilen çoğaltma işlemini düzenler ve yönetir. Azure Kurtarma Hizmetleri aracısı, Hyper-V ana bilgisayar sunucusunda Site Recovery dağıtımı sırasında yüklenir ve veriler HTTPS 443 üzerinden bu aracı ile Azure depolama alanı arasında çoğaltılır. Sağlayıcı ve aracı arasındaki iletişimler şifrelenir ve güvence altına alınır. Azure depolama alanında çoğaltılan veriler de şifrelenir.

- Şirket içi: 
    - **VMM sunucusu**: En az bir VMM sunucusu en az bir VMM özel bulutuyla ayarlanır. Sunucunun System Center 2012 R2'de çalışıyor olması ve internet bağlantısının olması gerekir. Yük devretme sonrasında Azure VM'lerinin bir ağa bağlandığından emin olmak istiyorsanız ağ eşlemesi ayarlamanız gerekir. Bu işlemi gerçekleştirmek için kaynak VM'lerin bir VMM VM ağına bağlı olması gerekir. Bu VM ağının da bulutla ilişkilendirilen bir mantıksal ağ ile bağlantılı olması gerekir.
    - **Hyper-V sunucusu**: VMM bulutta bulunan en az bir Hyper-V ana bilgisayar sunucusu. Hyper-V ana bilgisayarlarının Windows Server 2012 R2'yi çalıştırması gerekir.
    - **Korunan makineler**: Kaynak Hyper-V ana bilgisayar sunucusunun korumak istediğiniz en az bir VM'ye sahip olması gerekir.
    
- Azure: 
    - **Azure hesabı**: Bir Microsoft Azure hesabınızın olması gerekir.
    - **Azure depolama alanı**: Çoğaltılan verileri depolamak için bir Azure depolama alanı hesabınızın olması gerekir. Çoğaltılan veriler Azure depolama alanında depolanır ve yük devretme durumunda Azure VM'leri çalışmaya başlar.
    - **Azure ağı**: Yük devretmenin ardından Azure VM'lerinin ağlara bağlanması için ağ eşlemesi ayarlamak istiyorsanız bir Azure ağı ayarlamanız gerekir.

    ![VMM'den Azure'a](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)

[Dağıtım gereksinimlerinin](site-recovery-vmm-to-azure.md#before-you-start) tamamı hakkında daha fazla bilgi edinin.

## VMware sanal makineleri veya fiziksel sunucuları ikincil bir siteye çoğaltma

VMware VM'leri veya Windows/Linux fiziksel sunucuları ikincil bir siteye çoğaltmak için Azure Site Recovery aboneliğine dahil olan InMage Scout hizmetini indirirsiniz. Her sitede bileşen sunucularını (yapılandırma, işlem, ana hedef) ayarlayıp çoğaltmak istediğiniz makinelere Birleşik Aracı'yı yükleyin. İlk çoğaltmanın ardından makinelerdeki aracılar çoğaltma değişimleri işlem sunucusuna gönderir. İşlem sunucusu verileri iyileştirir ve ikincil sitedeki ana hedef sunucusuna aktarır. Yapılandırma sunucusu çoğaltma sürecini yönetir.

![VMware'den VMware'e](./media/site-recovery-components/vmware-to-vmware.png)

### Şirket içi birincil site

- **İşlem sunucusu**: Önbelleğe alma, sıkıştırma ve veri iyileştirme işlemlerini yürütmek için birincil sitenizde işlem sunucusu bileşenini ayarlayın. Ayrıca bu sunucu, Birleşik Aracı'nın korumak istediğiniz makinelere göndermeli yükleme işlemini yürütür. 
- **VMware ESX/ESXi ve vCenter sunucusu**: VMware VM'lerini koruyorsanız hiper yöneticileri yönetmek için bir VMware EXS/EXSi hiper yöneticisine ve isteğe bağlı olarak VMware vCenter sunucusuna sahip olmanız gerekir.
- **VM'ler/fiziksel sunucular**: Korumak istediğiniz VMware VM'leri veya Windows/Linux fiziksel sunucuları, Birleşik Aracı'nın yüklü olmasını gerektirir. Ayrıca, makinelere yüklenen Birleşik Aracı ana hedef sunucusu gibi davranır. Aracı,tüm bileşenler arasındaki bir iletişim sağlayıcısı gibi davranır. 
    
### Şirket içi ikincil site
 
- **Yapılandırma sunucusu**: Yapılandırma sunucusu yüklediğiniz ilk bileşendir ve yönetim Web sitesini veya vContinuum konsolunu kullanarak dağıtımınızı yönetme, yapılandırma ve izleme işlemleri için ikincil siteye yüklenir. Bir dağıtım işleminde yalnızca bir yapılandırma sunucusu vardır ve bu sunucunun Windows Server 2012 R2 çalıştıran bir makineye yüklenmesi gerekir.
- **vContinuum sunucusu**: Yapılandırma sunucusuyla aynı konuma (ikincil site) yüklenir. Korunan ortamınızı yönetmeye ve izlemeye yönelik bir konsol sağlar. Varsayılan yüklemede, vContinuum ilk ana hedef sunucusudur ve Birleşik Aracı bu sunucuda yüklüdür.
- **Ana hedef sunucusu**: Ana hedef sunucusu çoğaltılan verileri tutar. İşlem sunucusundan verileri alır, ikincil sitede çoğaltılan bir makine oluşturur ve veri bekletme noktalarını tutar. İhtiyacınız olan ana hedef sunucusu sayısı koruduğunuz makine sayısına bağlıdır. Birincil sitede yeniden çalıştırmak isterseniz burada da bir ana hedef sunucusuna sahip olmanız gerekir. 

### Azure

Bu senaryoyu, InMage Scout kullanarak dağıtırsınız. Bunu elde etmek için bir Azure aboneliğine sahip olmanız gerekir. Bir Site Recovery kasası oluşturduktan sonra InMage Scout hizmetini indirip dağıtımı ayarlamak üzere en son güncelleştirmeleri yüklersiniz.


## Hyper-V VM'lerini Azure'a çoğaltma (VMM olmadan)

VMM bulutlarında yönetilmeyen Hyper-V VM'lerini Azure'a çoğaltmak için Site Recovery dağıtımı sırasında Hyper-V ana bilgisayarına Azure Site Recovery Sağlayıcısı'nı ve Azure Kurtarma Hizmetleri aracısını yüklersiniz. Sağlayıcı, internet üzerinden Site Recovery hizmetiyle gerçekleştirilen çoğaltma işlemini düzenler ve yönetir. Aracı, veri çoğaltma işlemine ait verileri HTTPS 443 üzerinden işler. Sağlayıcı ve aracı arasındaki iletişimler şifrelenir ve güvence altına alınır. Azure depolama alanında çoğaltılan veriler de şifrelenir.

![Azure'da Hyper-V sitesi](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)

### Şirket içi

- **Hyper-V sunucusu**: En az bir Hyper-V ana bilgisayar sunucusu. Hyper-V ana bilgisayarlarının Windows Server 2012 R2'yi çalıştırması gerekir.
- **Korunan makineler**: Kaynak Hyper-V ana bilgisayar sunucusunun korumak istediğiniz en az bir VM'ye sahip olması gerekir.
    
### Azure

- **Azure hesabı**: Bir Microsoft Azure hesabınızın olması gerekir.
- **Azure depolama alanı**: Çoğaltılan verileri depolamak için bir Azure depolama alanı hesabınızın olması gerekir. Çoğaltılan veriler Azure depolama alanında depolanır ve yük devretme durumunda Azure VM'leri çalışmaya başlar.

Dağıtım gereksinimleri hakkında [daha fazla bilgi edinin](site-recovery-hyper-v-site-to-azure.md#before-you-start).


## VMM bulutlarındaki Hyper-V VM'lerini Azure'a çoğaltma

Bu senaryoyu dağıtmak için Site Recovery dağıtımı sırasında VMM sunucusuna Azure Site Recovery Sağlayıcısı'nı ve Hyper-V ana bilgisayarına da Azure Kurtarma Hizmetleri aracısını yüklersiniz. Sağlayıcı, internet üzerinden Site Recovery hizmetiyle gerçekleştirilen çoğaltma işlemini düzenler ve yönetir. Aracı, veri çoğaltma işlemine ait verileri HTTPS 443 üzerinden işler. Sağlayıcı ve aracı arasındaki iletişimler şifrelenir ve güvence altına alınır. Azure depolama alanında çoğaltılan veriler (bekleyen) de şifrelenir.

![VMM'den Azure'a](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)

### Şirket içi

- **VMM sunucusu**: En az bir VMM sunucusu en az bir VMM özel bulutuyla ayarlanır. Sunucunun System Center 2012 R2'de çalışıyor olması ve internet bağlantısının olması gerekir. Yük devretme sonrasında Azure VM'lerinin bir ağa bağlandığından emin olmak istiyorsanız ağ eşlemesi ayarlamanız gerekir. Bunu yapmak için kaynak VM'leri bir VMM VM ağına bağlamanız gerekir. Bu ağın, bulutla ilişkilendirilen mantıksal bir ağ ile bağlantılı olması gerekir.
- **Hyper-V sunucusu**: VMM bulutta bulunan en az bir Hyper-V ana bilgisayar sunucusu. Hyper-V ana bilgisayarlarının Windows Server 2012 R2'yi çalıştırması gerekir.
- **Korunan makineler**: Kaynak Hyper-V ana bilgisayar sunucusunun korumak istediğiniz en az bir VM'ye sahip olması gerekir.
    
### Azure

- **Azure hesabı**: Bir Microsoft Azure hesabınızın olması gerekir.
- **Azure depolama alanı**: Çoğaltılan verileri depolamak için bir Azure depolama alanı hesabınızın olması gerekir. Çoğaltılan veriler Azure depolama alanında depolanır ve yük devretme durumunda Azure VM'leri çalışmaya başlar.
- **Azure ağı**: Yük devretme sonrasında Azure VM'lerinin ağlara bağlandığından emin olmak istiyorsanız ağ eşlemesi ayarlamanız gerekir. Bunu yapmak için bir Azure ağı gerekir.

Dağıtım gereksinimleri hakkında [daha fazla bilgi edinin](site-recovery-vmm-to-azure.md#before-you-start).

## Hyper-V sanal makinelerini ikincil veri merkezine çoğaltma

Site Recovery dağıtımı sırasında bu senaryoyu dağıtmak için VMM sunucusuna Azure Site Recovery Sağlayıcısı'nı yüklersiniz. Sağlayıcı, internet üzerinden Site Recovery hizmetiyle gerçekleştirilen çoğaltma işlemini düzenler ve yönetir. Veriler, Kerberos veya sertifika kimlik doğrulaması kullanan LAN ya da VPN üzerinden birincil ve ikincil Hyper-V ana bilgisayar sunucuları arasında çoğaltılır. Sağlayıcı ve Hyper-V ana bilgisayarları arasındaki iletişimler şifrelenir ve güvence altına alınır. 

![Şirket içinden şirket içine](./media/site-recovery-components/arch-onprem-onprem.png)

### Şirket içi

- **VMM sunucusu**: En az bir VMM özel bulutu içeren bir VMM sunucusunun hem birincil sitede hem de ikincil sitede bir tane olacak şekilde bulundurulmasını öneririz. Sunucunun, en son güncelleştirmeleri içeren System Center 2012 SP1 sürümünde veya sonraki bir sürümde çalıştırılması ve internete bağlı olması gerekir. Bulutların Hyper-V işlev profili kümesine sahip olması gerekir.
- **Hyper-V sunucusu**: Hyper-V ana bilgisayar sunucularının birincil ve ikincil VMM bulutlarında yer alması gerekir. Ana bilgisayar sunucularının, en son güncelleştirmeleri içeren Windows Server 2012 sürümünde veya sonraki bir sürümde çalıştırılması ve internete bağlı olması gerekir.
- **Korunan makineler**: Kaynak Hyper-V ana bilgisayar sunucusunun korumak istediğiniz en az bir VM'ye sahip olması gerekir.
    
### Azure

Bir Azure aboneliğine sahip olmanız gerekir.

Dağıtım gereksinimleri hakkında [daha fazla bilgi edinin](site-recovery-vmm-to-vmm.md#before-you-start).


## Hyper-V VM'lerini SAN çoğaltmasını içeren ikincil veri merkezine çoğaltma

Bu senaryoda, Site Recovery dağıtımı sırasında VMM sunucularına Azure Site Recovery Sağlayıcısı'nı yüklersiniz. Sağlayıcı, internet üzerinden Site Recovery hizmetiyle gerçekleştirilen çoğaltma işlemini düzenler ve yönetir. Veriler, zaman uyumlu SAN çoğaltması kullanılarak birincil ve ikincil depolama dizileri arasında çoğaltılır.

![SAN çoğaltması](./media/site-recovery-components/arch-onprem-onprem-san.png)

### Şirket içi

- **SAN dizisi**: Birincil VMM sunucusu tarafından yönetilen [ desteklenen SAN dizisi](http://social.technet.microsoft.com/wiki/contents/articles/28317.deploying-azure-site-recovery-with-vmm-and-san-supported-storage-arrays.aspx). SAN, ikincil sitede başka bir SAN dizisiyle bir ağ altyapısı paylaşır.
- **VMM sunucusu**: En az bir VMM özel bulutu içeren bir VMM sunucusunun hem birincil sitede hem de ikincil sitede bir tane olacak şekilde bulundurulmasını öneririz. Sunucunun, en son güncelleştirmeleri içeren System Center 2012 SP1 sürümünde veya sonraki bir sürümde çalıştırılması ve internete bağlı olması gerekir. Bulutların Hyper-V işlev profili kümesine sahip olması gerekir.
- **Hyper-V sunucusu**: Birincil ve ikincil VMM bulutlarında yer alan Hyper-V ana bilgisayar sunucuları. Ana bilgisayar sunucularının, en son güncelleştirmeleri içeren Windows Server 2012 sürümünde veya sonraki bir sürümde çalıştırılması ve internete bağlı olması gerekir.
- **Korunan makineler**: Kaynak Hyper-V ana bilgisayar sunucusunun korumak istediğiniz en az bir VM'ye sahip olması gerekir.
    
### Azure

Bir Azure aboneliğine sahip olmanız gerekir.  

Dağıtım gereksinimleri hakkında [daha fazla bilgi edinin](site-recovery-vmm-san.md#before-you-start).


## Hyper-V koruması yaşam döngüsü

Bu iş akışı, Hyper-V sanal makineleri üzerinde gerçekleşen koruma, çoğaltma ve yük devretme işlemlerinin süreçlerini gösterir. 

1. **Korumayı etkinleştirme**: Site Recovery kasasını oluşturup, VMM bulutu veya Hyper-V sitesi için çoğaltma ayarlarını yapılandırırsınız ve VM'ler için korumayı etkinleştirirsiniz. **Korumayı Etkinleştir** olarak adlandırılan bir iş başlatılır ve **İşler** sekmesinden izlenebilir. İş, makinenin önkoşullarla uyumlu olup olmadığını denetler ve ardından daha önce yapılandırdığınız ayarlarla Azure'a çoğaltma işlemini ayarlayan [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) yöntemini çağırır. Ayrıca, **Korumayı Etkinleştir** işi tam bir VM çoğaltmasını başlatmak için [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) yöntemini de çağırır.
2. **İlk çoğaltma**: Bir sanal makine anlık görüntüsü alınır ve tamamı Azure'a veya ikincil bir veri merkezine kopyalanana dek sanal sabit diskler çoğaltılır. Bu işlemi tamamlamak için gereken süre VM boyutuna, ağ bant genişliğine ve iş çoğaltma yöntemine bağlıdır. İlk çoğaltma sırasında disk değişimi meydana gelirse Hyper-V Çoğaltma'nın Çoğaltma İzleyicisi bu değişiklikleri, disklerle aynı klasörde yer alan Hyper-V Çoğaltma Günlükleri (.hrl) olarak izler. Her diskin ikincil depolamaya gönderilecek bir ilişkili .hrl dosyası vardır. İlk çoğaltma sırasında anlık görüntü ve günlük dosyalarının disk kaynaklarını kullandığını unutmayın. İlk çoğaltma tamamlandığında VM anlık görüntüsü silinir, günlükteki değişim diski değişiklikleri eşitlenir ve birleştirilir.
3. **Korumayı sonlandırma**: İlk çoğaltma sonlandırıldıktan sonra **Korumayı sonlandır** işi, ağ ve diğer çoğaltma sonrası ayarlarını yapılandırır ve sanal makine korunur. Azure'da çoğaltma yapıyorsanız sanal makinede ince ayar yapmanız gerekebilir. Böylece sanal makine yük devretme için hazır hale gelir. Bu noktada, her şeyin istendiği şekilde çalıştığını denetlemek için yük devretme testi çalıştırabilirsiniz.
4. **Çoğaltma**: İlk çoğaltma sonrasında çoğaltma ayarlarına uygun şekilde değişim eşitlemesi başlar. 
    - **Çoğaltma hatası**: Değişim çoğaltması başarısız olursa ve tam çoğaltma bant genişliği ve zaman konusunda fazla kaynak tüketimine yol açarsa yeniden eşitleme meydana gelir. Örneğin, .hrl dosyası disk boyutunun %50'sine ulaşırsa VM yeniden eşitleme için işaretlenir. Yeniden eşitleme, kaynak ve hedef sanal makinelerin işlem sağlama toplamları tarafından gönderilen verilerin miktarını azaltır ve yalnızca değişim verilerini gönderir. Yeniden eşitleme bittikten sonra değişim çoğaltması devam eder. Varsayılan olarak, yeniden eşitleme ofis saatleri dışında otomatik olarak çalışacak şekilde planlanmıştır ancak sanal makineyi elle yeniden eşitleyebilirsiniz.
    - **Çoğaltma hatası**: Bir çoğaltma hatası meydana gelmesi durumunda yerleşik olarak yeniden deneme işlevi bulunur. Kimlik doğrulama veya yetkilendirme ya da çoğaltılan makinenin geçersiz durumda olması gibi kurtarılamaz bir hata olursa yeniden deneme işlevi uygulanmaz. Ağ hatası veya düşük disk alanı/belleği gibi kurtarılabilir bir hata oluşursa artan aralıklarla yeniden denemeler meydana gelir (her 1, 2, 4, 8, 10 ve ardından 30 dakikada bir şeklinde).
4. **Planlanan/planlanmamış yük devretme işlemleri**: Gerektiğinde planlanan veya planlanmamış yük devretmeler çalıştırabilirsiniz. Planlanan bir yük devretme çalıştırırsanız kaynak VM'ler veri kaybı olmamasını sağlamak için kapatılır. Çoğaltılan VM'ler oluşturulduktan sonra yürütme bekleme durumuna yerleştirilir. Yürütmenin otomatik olduğu SAN ile çoğaltma işlemini gerçekleştirmiyorsanız yük devretmeyi tamamlamak için VM'leri yürütmeniz gerekir. Birincil site açılıp çalıştığında yeniden çalışma meydana gelir. Çoğaltma Azure'a yapılıyorsa ters çoğaltma otomatiktir. Aksi halde, ters çoğaltmayı elle tetiklersiniz.
 

![iş akışı](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

## Sonraki adımlar

[Dağıtım için hazırlanma](site-recovery-best-practices.md)



<!----HONumber=Jun16_HO2-->


