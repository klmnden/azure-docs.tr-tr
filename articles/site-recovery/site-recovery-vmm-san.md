---
title: "Azure Site Recovery kullanarak Hyper-V Vm'lerini SAN ile vmm'de çoğaltma | Microsoft Docs"
description: "Bu makale, Hyper-V sanal makinelerini SAN çoğaltması kullanılarak Azure Site Recovery ile iki site arasında çoğaltma açıklar."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: eb480459-04d0-4c57-96c6-9b0829e96d65
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: raynew
ms.openlocfilehash: 3df38802fcdc86e4553253d38c49faff455f873e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-hyper-v-vms-in-vmm-clouds-to-a-secondary-site-with-azure-site-recovery-by-using-san"></a>SAN kullanarak VMM bulutlarındaki Hyper-V sanal makineleri Azure Site Recovery ile ikincil siteye çoğaltma


Dağıtmak istiyorsanız bu makaleyi kullanın [Azure Site Recovery](site-recovery-overview.md) (System Center Virtual Machine Manager bulutta yönetilen) Hyper-V Vm'lerini çoğaltma Klasik portalda Azure Site RECOVERY'yi kullanarak bir ikincil VMM sitesi yönetmek için. Bu senaryo yeni Azure portalında kullanılamaz.



Bu makalenin sonunda yorumları gönderin. Teknik sorulara yanıtlar alın [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="why-replicate-with-san-and-site-recovery"></a>Neden SAN ve Site Recovery ile çoğaltabilir?

* SAN kuruluş düzeyi, ölçeklenebilir çoğaltma çözümünü sunar, böylece Hyper-V SAN ile içeren bir birincil site ikincil bir siteye SAN ile LUN'lar çoğaltabilirsiniz. Depolama VMM tarafından yönetildiğinden ve çoğaltma ve yük devretme Site Recovery ile düzenlenir.
* Site Kurtarma ile birkaç çalışılan [SAN depolama ortakları](http://social.technet.microsoft.com/wiki/contents/articles/28317.deploying-azure-site-recovery-with-vmm-and-san-supported-storage-arrays.aspx) çoğaltma Fiber Kanal ve iSCSI depolama alanı sağlamak için.  
* Hyper-V kümelerinde dağıtılmış görev açısından kritik uygulamaları korumak için varolan SAN altyapınızı kullanın. N katmanlı uygulamalar yerine tutarlı bir şekilde çalışılabilir böylece sanal makineleri bir grup olarak çoğaltılabilir.
* SAN çoğaltması eşitlenmiş çoğaltma düşük RTO ve RPO için olan bir uygulama farklı katmanlar arasında çoğaltma tutarlılığı sağlar ve (bağlı olarak, depolama dizisi yeteneklerini) yüksek esneklik için çoğaltma zaman.  
* VMM doku SAN depolama yönetebilir ve SMI-S varolan depolama bulmak için VMM kullanın.  

Şunlara dikkat edin:
- Siteden siteye çoğaltma SAN ile Azure portalında kullanılamaz. Klasik portalda yalnızca kullanılabilir. Yeni kasa Klasik portalda oluşturulamıyor. Varolan kasalarını korunabilir.
- Azure SAN çoğaltmayı desteklenmiyor.
- Paylaşılan Vhdx'ler ya da iSCSI veya Fiber Kanal VM'ler için doğrudan bağlı mantıksal birim (LUN) çoğaltma yapamaz. Konuk kümeleri çoğaltılabilir.


## <a name="architecture"></a>Mimari

![SAN mimarisi](./media/site-recovery-vmm-san/architecture.png)

- **Azure**: Azure portalında Site Recovery kasası ayarlayın.
- **SAN depolama alanı**: SAN depolama alanı VMM doku yönetilir. Depolama sağlayıcısını ekleyin, depolama sınıflandırması oluşturun ve depolama havuzları ayarlayın.
- **VMM ve Hyper-V**: her sitedeki VMM sunucusu öneririz. VMM özel bulut kurmak ve bu bulutlara Hyper-V kümeleri yerleştirin. Dağıtım sırasında her VMM sunucusunda Azure Site Recovery sağlayıcısı yüklü ve sunucuyu kasaya kayıtlı. Sağlayıcı, çoğaltma, yük devretme ve yeniden çalışma yönetmek için Site Recovery hizmeti ile iletişim kurar.
- **Çoğaltma**: vmm'de depolama ayarlama ve Site Recovery çoğaltma yapılandırdıktan sonra birincil ve ikincil SAN depolama alanı arasında çoğaltma gerçekleşir. Hiç çoğaltma verisi Site Recovery hizmetine gönderilir.
- **Yük devretme**: yük devretme Site Recovery portalında etkinleştirin. Yoktur sıfır veri kaybı yük devretme sırasında çoğaltma zaman uyumlu olduğundan.
- **Yeniden çalışma**: geri dönecek için delta değişiklikleri ikincil siteden birincil siteye aktarmak çoğaltmayı tersine çevirme etkinleştirin. Çoğaltmayı tersine çevirme işlemi tamamlandıktan sonra planlanmış bir yük devretme birincil ikincil çalıştırın. Bu planlanmış bir yük devretme ikincil sitede çoğaltma sanal makineleri durdurur ve birincil sitede başlatır.


## <a name="before-you-start"></a>Başlamadan önce


**Önkoşullar** | **Ayrıntılar**
--- | ---
**Azure** |Bir [Microsoft Azure](https://azure.microsoft.com/) hesabınızın olması gerekir. [Ücretsiz deneme sürümüyle](https://azure.microsoft.com/pricing/free-trial/) başlayabilirsiniz. Site Recovery fiyatlandırması hakkında [daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/site-recovery/). Çoğaltma ve yük devretme yapılandırmak ve yönetmek için bir Azure Site Recovery kasası oluşturun.
**VMM** | Tek bir VMM sunucusu kullanın ve farklı Bulutlar arasında çoğaltılır, ancak birincil sitedeki bir VMM ve bir ikincil sitedeki öneririz. VMM, bir fiziksel veya sanal bağımsız sunucu veya bir küme olarak dağıtılabilir. <br/><br/>VMM sunucusu System Center 2012 R2 çalıştırması gerekir ya da daha sonra en son toplu güncelleştirmelerine sahip.<br/><br/> Korumak istediğiniz birincil VMM sunucusunda yapılandırılmış en az bir bulut ve yük devretme için kullanmak istediğiniz ikincil VMM sunucusunda yapılandırılmış bir bulut gerekir.<br/><br/> Kaynak bulut bir veya daha fazla VMM ana bilgisayar grubu içermesi gerekir.<br/><br/> Tüm VMM Bulutları Hyper-V Kapasite profili kümesine sahip olması gerekir.<br/><br/> VMM bulutlarını ayarlama hakkında daha fazla bilgi için bkz: [bir VM Bulutu dağıtmak](https://technet.microsoft.com/en-us/system-center-docs/vmm/scenario/cloud-overview).
**Hyper-V** | Bir veya daha fazla Hyper-V kümeleri birincil ve ikincil VMM bulutlarında gerekir.<br/><br/> Kaynak Hyper-V kümesi, bir veya daha fazla VM içermesi gerekir.<br/><br/> Birincil ve ikincil sitelerde VMM konak grupları Hyper-V kümeleri en az birini içermelidir.<br/><br/>Ana bilgisayar ve hedef Hyper-V sunucuları Windows Server 2012 veya sonraki sürümünü Hyper-V rolü ve son güncelleştirmelerin yüklü ile çalıştırması gerekir.<br/><br/> Bir kümede Hyper-V çalıştırıyorsanız ve bir statik IP adresi tabanlı küme varsa, küme aracısının otomatik olarak oluşturulmaz. Bunu el ile yapılandırmanız gerekir. Daha fazla bilgi için bkz: [Hyper-V çoğaltma için konak kümeleri hazırlama](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters).
**SAN depolama alanı** | Konuk kümelenmiş sanal makineler iSCSI veya kanal depolama veya paylaşılan sanal sabit diskler (vhdx) kullanarak çoğaltabilirsiniz.<br/><br/> İki SAN Dizileri gerekir: biri birincil sitedeki ve biri de ikincil sitede.<br/><br/> Diziler arasındaki bir ağ altyapısı ayarlanması. Eşleme ve çoğaltma yapılandırılması gerekir. Çoğaltma lisansları depolama dizisi gereksinimlerine uyacak şekilde ayarlanmış olması.<br/><br/>Konak iSCSI veya Fiber Kanal kullanarak depolama LUN'ları ile iletişim kurabilmesi için depolama dizisi ve Hyper-V ana bilgisayar sunucuları ağ yukarı ayarlayın.<br/><br/> Denetleme [desteklenen depolama dizileri](http://social.technet.microsoft.com/wiki/contents/articles/28317.deploying-azure-site-recovery-with-vmm-and-san-supported-storage-arrays.aspx).<br/><br/> Depolama dizisi üreticileri SMI-S sağlayıcılarını yüklenmelidir ve SAN Dizileri sağlayıcı tarafından yönetilmelidir. Sağlayıcı üreticisinin yönergelerine göre ayarlayın.<br/><br/>Dizinin SMI-S sağlayıcısını VMM sunucusu bir IP adresi veya FQDN ile ağ üzerinden erişebildiği bir sunucuya olduğundan emin olun.<br/><br/> Her SAN dizisi, bir veya daha fazla kullanılabilir depolama havuzu olmalıdır.<br/><br/> Birincil VMM sunucusunun birincil dizi yönetmeniz gerekir ve ikincil VMM sunucusu ikincil dizi yönetmeniz gerekir.
**Ağ eşlemesi** | Böylece yük devretme sonrasında çoğaltılmış sanal makinelere en iyi şekilde ikincil Hyper-V ana bilgisayar sunucuları üzerinde yerleştirilir ve böylece uygun VM ağlarına bağlı oldukları ağ eşlemesi ayarlayın. Ağ eşlemesini yapılandırmazsanız, yük devretme sonrasında çoğaltma sanal makineleri, herhangi bir ağa bağlanmayacaktır.<br/><br/> Site Recovery dağıtımı sırasında ağ eşlemesini ayarlayabilirsiniz böylece VMM ağlarına doğru şekilde yapılandırıldığından emin olun. VMM'de, kaynak Hyper-V konak sanal makinelerin bir VMM VM ağına bağlanması gerekir. Bu ağın, bulutla ilişkilendirilen mantıksal bir ağ ile bağlantılı olması gerekir.<br/><br/> Hedef bulut karşılık gelen VM ağı olmalıdır ve bu sırayla hedef Bulutu ile ilişkili değil karşılık gelen bir mantıksal ağ bağlantılı olması gerekir.<br/><br/>.

## <a name="step-1-prepare-the-vmm-infrastructure"></a>1. adım: VMM altyapıyı hazırlama
VMM altyapınızı hazırlamak için aktarmanız gerekir:

1. VMM Bulutları doğrulayın.
2. Tümleştirme ve vmm'de SAN depolama Sınıflandır.
3. LUN oluşturun ve depolama alanı ayır.
4. Çoğaltma grupları oluşturun.
5. VM ağları ayarlayın.

### <a name="verify-vmm-clouds"></a>VMM Bulutları doğrulayın

Site Recovery dağıtımı başlamadan önce VMM Bulutları düzgün ayarlandığından emin olun.

### <a name="integrate-and-classify-san-storage-in-the-vmm-fabric"></a>Tümleştirme ve VMM doku SAN depolama Sınıflandır

1. VMM konsolunda Git **doku** > **depolama** > **kaynak Ekle** > **depolama aygıtları**.
2. İçinde **depolama aygıtları ekleme** seçin **bir depolama sağlayıcısı türü seç** seçip **bulunan ve bir SMI-S sağlayıcısı tarafından yönetilen SAN ve NAS cihazları**.

    ![Sağlayıcı türü](./media/site-recovery-vmm-san/provider-type.png)

3. Üzerinde **belirtin protokolünü ve adresini depolama SMI-S sağlayıcısının** sayfasında, **SMI-S CIMXML** ve sağlayıcıya bağlanmak için ayarları belirtin.
4. İçinde **sağlayıcı IP adresi veya FQDN** ve **TCP/IP bağlantı noktası**, sağlayıcıya bağlanmak için ayarları belirtin. SMI-S CIMXML için yalnızca bir SSL bağlantısı kullanabilirsiniz.

    ![Sağlayıcı Bağlan](./media/site-recovery-vmm-san/connect-settings.png)
5. İçinde **farklı çalıştır hesabı**, sağlayıcısına erişmek veya bir hesap oluşturduğunuz bir VMM farklı çalıştır hesabı belirtin.
6. Üzerinde **bilgi toplayın** sayfasında, VMM otomatik olarak çalışır bulmak ve depolama cihazı bilgilerini almak. Bulma yeniden denemek için tıklatın **tarama sağlayıcısı**. Bulma işlemi başarılı olursa, depolama havuzları, bulunan depolama dizilerinin üreticisini, model ve kapasite sayfasında listelenir.

    ![Depolama Bul](./media/site-recovery-vmm-san/discover.png)
7. İçinde **yönetim altına yerleştirin ve bir sınıflandırma atamak için depolama havuzlarını seçin**, VMM yönetmek ve bunları bir sınıflandırma ATA Depolama havuzlarını seçin. LUN bilgileri depolama havuzlarından içeri aktarılır. LUN'ları korumak için ihtiyaç duydukları uygulamalara, kapasite gereksinimlerine ve birlikte çoğaltmak gerekenler, gereksinimlerinize göre oluşturun.

    ![Depolama Sınıflandır](./media/site-recovery-vmm-san/classify.png)

### <a name="create-luns-and-allocate-storage"></a>LUN'ları oluşturma ve depolama alanı Ayır

LUN'ları korumak için ihtiyaç duydukları uygulamalara, kapasite gereksinimlerini ve birlikte çoğaltmak gerekenler, gereksinimlerinize göre oluşturun.

1. VMM doku depolama göründükten sonra şunları yapabilirsiniz [LUN sağlamak](https://technet.microsoft.com/en-us/system-center-docs/vmm/manage/manage-storage-host-groups#create-a-lun-in-vmm).

     > [!NOTE]
     > VHD'ler, LUN'ları çoğaltma için etkinleştirilmiş VM eklemeyin. Bu LUN'ları bir Site Recovery çoğaltma grubunda değilse, Site Recovery tarafından algılanmaz.
     >

2. VMM sanal makine verilerini sağlanan depolama dağıtabileceğiniz Hyper-V konak kümesi için depolama kapasitesi atayın:

   * Küme için depolama alanı ayrılırken önce küme bulunduğu VMM konak grubuna ayırmak gerekir. Daha fazla bilgi için bkz: [vmm'de bir konak grubuna depolama mantıksal birimlerini ayırmak nasıl](https://technet.microsoft.com/library/gg610686.aspx) ve [vmm'de bir konak grubuna depolama havuzları ayırmak nasıl](https://technet.microsoft.com/library/gg610635.aspx).
   * Bölümünde açıklandığı gibi küme için depolama kapasitesi tahsis [vmm'de bir Hyper-V konak kümesindeki depolamayı yapılandırma](https://technet.microsoft.com/library/gg610692.aspx).

    ![Sağlayıcı türü](./media/site-recovery-vmm-san/provider-type.png)
3. İçinde **belirtin protokolünü ve adresini depolama SMI-S sağlayıcısının**seçin **SMI-S CIMXML**. Sağlayıcıya bağlanmak için ayarları belirtin. Bir SSL bağlantısı yalnızca SMI-S CIMXML için kullanabilirsiniz.

    ![Sağlayıcı Bağlan](./media/site-recovery-vmm-san/connect-settings.png)
4. İçinde **farklı çalıştır hesabı**, sağlayıcısına erişmek veya bir hesap oluşturduğunuz bir VMM farklı çalıştır hesabı belirtin.
5. İçinde **toplamanız bilgiler**, VMM bulmak ve depolama cihazı bilgilerini almak otomatik olarak çalışır. Yeniden denemek gerekiyorsa, tıklatın **tarama sağlayıcısı**. Bulma işlemi başarılı olduğunda, depolama dizileri, depolama havuzlarını üreticisini, model ve kapasite sayfasında listelenir.

    ![Depolama Bul](./media/site-recovery-vmm-san/discover.png)
7. İçinde **yönetim altına yerleştirin ve bir sınıflandırma atamak için depolama havuzlarını seçin**, VMM yönetmek ve bir sınıflandırma atamak, depolama havuzlarını seçin. LUN bilgileri depolama havuzlarından içeri aktarılır.

    ![Depolama Sınıflandır](./media/site-recovery-vmm-san/classify.png)


### <a name="create-replication-groups"></a>Çoğaltma grupları oluşturma

Birlikte çoğaltmak için gereken tüm LUN'ları içeren bir çoğaltma grubu oluşturun.

1. VMM konsolunu Aç **çoğaltma grupları** depolama dizisi özellikleri ve ardından sekmesinde **yeni**.
2. Çoğaltma grubu oluşturun.

    ![SAN çoğaltma grubu](./media/site-recovery-vmm-san/rep-group.png)

### <a name="set-up-networks"></a>Ağları ayarlayın

Ağ eşlemesi yapılandırmak istiyorsanız, aşağıdakileri yapın:

1. Site Recovery ağ eşlemesi bakın.
2. VMM'de VM ağlarına için hazırlık yapın:

   * [Mantıksal ağlar ayarlayın](https://technet.microsoft.com/en-us/system-center-docs/vmm/manage/manage-network-logical-networks).
   * [VM ağları ayarlayın](https://technet.microsoft.com/en-us/system-center-docs/vmm/manage/manage-network-vm-networks).


## <a name="step-2-create-a-vault"></a>2. adım: bir kasa oluşturma

1. Oturum [Azure portal](https://portal.azure.com) kasaya kaydetmek istediğiniz VMM sunucusundan.
2. Genişletme **Veri Hizmetleri** > **kurtarma Hizmetleri**ve ardından **Site Recovery kasası**.
3. **Yeni Oluştur** > **Hızlı Oluştur**'a tıklayın.
4. **Ad** alanına kasayı tanımlamak için kolay bir ad girin.
5. **Bölge** içinde, kasa için coğrafi bölgeyi seçin. Desteklenen bölgeleri kontrol etmek için bkz: [Azure Site Recovery fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/site-recovery/).
6. **Kasa Oluştur**'a tıklayın.

    ![Yeni Kasa](./media/site-recovery-vmm-san/create-vault.png)

Kasanın başarıyla oluşturulduğunu doğrulamak için durum çubuğunu kontrol edin. Kasa olarak listelenen **etkin** ana **kurtarma Hizmetleri** sayfası.

### <a name="register-the-vmm-servers"></a>VMM sunucularını kaydetme

1. Açık **Hızlı Başlangıç** gelen sayfa **kurtarma Hizmetleri** sayfası. Hızlı Başlangıç, istediğiniz zaman simgesini seçerek de açılabilir.

    ![Hızlı Başlangıç Simgesi](./media/site-recovery-vmm-san/quick-start-icon.png)
2. Aşağı açılan kutusunda **arasında Hyper-V şirket içi dizi çoğaltma kullanarak site**.

    ![Kayıt anahtarı](./media/site-recovery-vmm-san/select-san.png)
3. İçinde **VMM sunucularını hazırla**, Azure Site Recovery sağlayıcısı yükleme dosyasının en son sürümünü indirin.
4. Bu dosyayı kaynak VMM sunucusunda çalıştırın. VMM bir kümede dağıtılmışsa ve sağlayıcı ilk kez yüklüyorsanız, etkin bir düğümde bir sağlayıcı yükleyin ve VMM sunucusunu kasaya kaydetmek için yüklemeyi sonlandırın. Ardından, Sağlayıcı'yı diğer düğümlere yükleyin. Sağlayıcı yükseltiyorsanız, böylece aynı sağlayıcı sürümlerinin yüklü tüm düğümlerde yükseltmeniz gerekir.
5. Yükleyici, gereksinimleri ve VMM hizmetini durdurmak için istekleri izin sağlayıcının kurulumunu başlatmak için denetler. Kurulum bittiğinde hizmet otomatik olarak yeniden başlatılır. Bir VMM kümesinde Küme rolünü durdurmanız istenir.
6. İçinde **Microsoft Update**güncelleştirmeleri de seçebilirsiniz ve sağlayıcı güncelleştirmeleri Microsoft Update ilkenize göre yüklenir.

    ![Microsoft Güncelleştirmeleri](./media/site-recovery-vmm-san/ms-update.png)

7. Varsayılan olarak, sağlayıcı yükleme konumudur <SystemDrive>\Program System Center 2012 R2\Virtual Machine Manager\bin. Tıklatın **yükleme** başlamak için.

    ![Yükleme konumu](./media/site-recovery-vmm-san/install-location.png)

8. Sağlayıcı yüklendikten sonra tıklayın **kaydetmek** VMM sunucusunu kasaya kaydetmek için.

    ![Yükleme tamamlandı](./media/site-recovery-vmm-san/install-complete.png)

9. İçinde **Internet bağlantısı**, sağlayıcı Internet'e nasıl bağlandığını belirtin. Seçin **varsayılan sistem Ara sunucu ayarlarını kullan** sunucuda varsayılan Internet bağlantısı ayarlarını kullanmak istiyorsanız.

    ![İnternet Ayarları](./media/site-recovery-vmm-san/proxy-details.png)

   * Özel bir ara sunucu kullanmak isterseniz, sağlayıcı yüklemeden önce bunu ayarlayın. Özel ara sunucu ayarlarını yapılandırırken Ara sunucu bağlantısını denetlemek için bir test çalıştırır.
   * Özel bir ara sunucu kullanırsanız veya varsayılan ara sunucunuz kimlik doğrulaması gerektiriyorsa, adresi ve bağlantı noktası dahil olmak üzere ara sunucu bilgilerini girmeniz gerekir.
   * Gerekli URL'leri VMM sunucusundan erişilebilir olması gerekir.
   * Özel bir ara sunucu kullanırsanız, belirtilen proxy kimlik bilgilerini kullanarak bir VMM farklı çalıştır hesabı (DRAProxyAccount) otomatik olarak oluşturulur. Bu hesap doğrulanabilmesi proxy sunucusunu yapılandırın. Farklı Çalıştır hesap ayarlarını VMM konsolundan değiştirebilirsiniz (**ayarları** > **güvenlik** > **farklı çalıştır hesapları**  >   **DRAProxyAccount**). Değişikliğin etkili olması VMM hizmetini yeniden başlatmanız gerekir.
10. İçinde **kayıt anahtarı**, portalından indirdiğiniz ve VMM sunucusuna kopyaladığınız anahtar seçin.
11. **Kasa adı** alanında, sunucunun kayıtlı olduğu kasanın adını doğrulayın.

    ![Sunucu kaydı](./media/site-recovery-vmm-san/vault-creds.png)
12. Şifreleme ayar, yalnızca Azure çoğaltma VMM için kullanılır. Onu yok sayabilirsiniz.

    ![Sunucu kaydı](./media/site-recovery-vmm-san/encrypt.png)
13. **Sunucu adı** alanında, kasadaki VMM sunucusunu tanımlamak için bir kolay ad belirtin. Bir küme yapılandırmasında VMM küme rolünün adını belirtin.
14. İçinde **ilk bulut meta verileri eşitleme**VMM sunucusundaki tüm Bulutlar için meta verilerini eşitlemek istediğinizi seçin. Bu eylemin her sunucuda yalnızca bir kez gerçekleştirilmesi gerekir. Tüm bulutları eşitlemek istemezseniz bu ayarı işaretlemeden bırakıp her bulutu VMM konsolundaki bulut özelliklerinde bağımsız olarak eşitleyebilirsiniz.

    ![Sunucu kaydı](./media/site-recovery-vmm-san/friendly-name.png)

15. İşlemi tamamlamak için **İleri**'ye tıklayın. Kayıttan sonra Azure Site Recovery tarafından VMM sunucusundan meta veriler alınır. Sunucu görüntülenen **sunucuları** > **VMM sunucuları** kasadaki.

### <a name="command-line-installation"></a>Komut satırı yüklemesi

Azure Site Recovery sağlayıcısı, aşağıdaki komut satırını kullanarak da yüklenebilir. Bu yöntem, çekirdek Windows Server 2012 R2 için sunucu üzerinde sağlayıcı yüklemek için kullanılabilir.

1. Sağlayıcı yükleme dosyasını ve kayıt anahtarını bir klasöre indirin. Örneğin, C:\ASR.
2. VMM hizmetini durdurun.
3. Sağlayıcı yükleyicisini ayıklamak. Bu komutlar bir yönetici olarak çalıştırın:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
4. Sağlayıcıyı yükleyin:

        C:\ASR> setupdr.exe /i
5. Sağlayıcıyı kaydettirin:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>         

Parametreler:

* **/ Credentials**: gerekli parametre için kayıt anahtarı dosyasının bulunduğu konum.  
* **/ FriendlyName**: Azure Site Recovery portalında görünen Hyper-V konak sunucusu adı için gerekli parametre.
* **/ EncryptionEnabled**: yalnızca VMM'den Azure'a çoğaltırken kullanılan isteğe bağlı parametre.
* **/proxyAddress**: Ara sunucunun adresini belirten isteğe bağlı parametre.
* **/proxyport**: Ara sunucunun bağlantı noktasını belirten isteğe bağlı parametre.
* **/ proxyusername**: (proxy kimlik doğrulaması gerektiriyorsa) Ara sunucu kullanıcı adını belirten isteğe bağlı parametre.
* **/ proxypassword**: (proxy kimlik doğrulaması gerektiriyorsa), proxy sunucusu ile kimlik doğrulaması için parolayı belirten isteğe bağlı parametre.

## <a name="step-3-map-storage-arrays-and-pools"></a>3. adım: depolama dizileri ve havuzları eşleme

Hangi ikincil depolama havuzu birincil havuzundan çoğaltma verileri alan belirtmek için birincil ve ikincil diziler eşleyin. Çoğaltma, yapılandırmadan önce çoğaltma gruplarında korumayı etkinleştirdiğinizde eşleme bilgilerini kullanıldığından depolama eşleyin.

Başlamadan önce VMM Bulutları kasaya olup olmadığını denetleyin. Bulut sağlayıcısı yükleme sırasında tüm Bulutları eşitlemek olduğunda veya özel Bulutu VMM konsolundaki eşitlediğinizde algılanır.

1. Tıklatın **kaynakları** > **sunucu depolama** > **harita kaynak ve hedef dizilerinin**.
    ![Sunucu kaydı](./media/site-recovery-vmm-san/storage-map.png)

2. Birincil site depolama dizilerinde seçin ve depolama dizileri ikincil sitede eşleyin. İçinde **depolama havuzları**, bir kaynak seçin ve hedef eşlemek için depolama havuzu.

    ![Sunucu kaydı](./media/site-recovery-vmm-san/storage-map-pool.png)

## <a name="step-4-configure-replication-settings"></a>4. adım: çoğaltma ayarlarını yapılandırın

VMM sunucusu kayıtlı sonra bulut koruma ayarlarını yapılandırın.

1. Üzerinde **Hızlı Başlangıç** sayfasında, **VMM Bulutları için koruma ayarlama**.
2. Üzerinde **korunan öğeler** sekmesinde, bulut seçin **yapılandırma**.
3. İçinde **hedef**seçin **VMM**.
4. İçinde **hedef konum**, kurtarma için kullanmak istediğiniz bulut yönetir VMM sunucusunu seçin.
5. İçinde **hedef bulut**, VM yük devretme için kullanmak istediğiniz hedef Bulutu seçin.
   * Koruduğunuz sanal makineleri kurtarma gereksinimlerini karşılayan bir hedef bulut seçmenizi öneririz.
   * Bir buluta yalnızca bir tek bulut çifti--bir birincil veya bir hedef bulut olarak ait olabilir.
6. Site Recovery Bulutlar SAN depolama alanına erişimi olduğunu ve depolama dizileri eşlendiğini doğrular.
7. Doğrulama, başarılı olursa **çoğaltma türü**seçin **SAN**.

Ayarlar kaydedildikten sonra bir iş oluşturulur izlenebilir **işleri** sekmesi. Ayarları değiştirilebilir **yapılandırma** sekmesi. Hedef konumu veya hedef bulut değiştirmek istiyorsanız, bulut yapılandırmasını kaldırın ve Bulutu yeniden yapılandırmanız gerekir.

## <a name="step-5-enable-network-mapping"></a>5. adım: ağ eşlemesini etkinleştir

1. Üzerinde **Hızlı Başlangıç** sayfasında, **ağları Eşle**.
2. Kaynak VMM sunucusunu seçin ve ağlar eşleştirilecek hedef VMM sunucusunu seçin. Kaynak ağların ve ilişkili hedef ağlarının listesi görüntülenir. Boş bir değer eşlenmemiş ağlar için gösterilir. Her ağın alt ağlarını görüntülemek için kaynak ve hedef ağ adlarının yanındaki bilgi simgesine tıklayın.
3. Bir ağ seçin **kaynak üzerinde ağ**ve ardından **harita**. Hizmet, hedef sunucu üzerindeki VM ağlarını algılar ve bunları görüntüler.

    ![SAN mimarisi](./media/site-recovery-vmm-san/network-map1.png)
4. Hedef VMM sunucusunda VM ağları birini seçin.

    ![SAN mimarisi](./media/site-recovery-vmm-san/network-map2.png)

5. Hedef ağ seçtiğinizde, kaynak ağı kullanan korumalı Bulutlar gösterilir. Kullanılabilir hedef ağlar da görüntülenir. Çoğaltma için kullandığınız tüm bulutların kullanabildiği bir hedef ağ seçmeniz önerilir.
6. Eşleme işlemini tamamlamak için onay işaretine tıklayın. Bir işin ilerleme durumunu izleyen başlatır. Üzerinde görüntülemek **işleri** sekmesi.

## <a name="step-6-enable-replication-for-replication-groups"></a>6. adım: çoğaltma grupları için çoğaltma etkinleştirme

Sanal makineler için korumayı etkinleştirmeden önce depolama çoğaltma grupları için çoğaltma etkinleştirmeniz gerekir.

1. Üzerinde **özellikleri** açık Site Recovery portalında birincil bulut sayfasının **sanal makineleri** sekmesinde **çoğaltma grubuna Ekle**.
2. Birini seçin veya bulutla ilişkili daha fazla VMM çoğaltma grupları, kaynak ve hedef diziler doğrulayın ve çoğaltma sıklığı belirtin.

Site Recovery, VMM ve SMI-S sağlayıcıları hedef site depolama LUN sağlamak ve depolama çoğaltmayı etkinleştirin. Çoğaltma grubu zaten çoğaltılır, Site Recovery mevcut çoğaltma ilişkisinin yeniden kullanır ve bilgileri güncelleştirir.

## <a name="step-7-enable-protection-for-virtual-machines"></a>7. adım: sanal makineler için korumayı etkinleştir


Bir depolama grubu çoğaltırken, aşağıdaki yöntemlerden biriyle VMM konsolunda VM'ler için korumayı etkinleştir:

* **Yeni sanal makine**: VM oluşturduğunuzda, çoğaltmayı etkinleştirip VM çoğaltma grubuyla ilişkilendirin.
  Bu seçenek ile en iyi şekilde VM depolama çoğaltma grubunun LUN'lara yerleştirmek için akıllı yerleştirme VMM kullanır. Site kurtarma, ikincil sitedeki VM gölge oluşturulmasını düzenleyen ve böylece yük devretme sonrasında çoğaltma sanal makineleri başlatılabilir kapasite ayırır.
* **Var olan sanal makine**: bir sanal makine VMM'de dağıtılırsa, çoğaltmayı etkinleştirmek ve bir çoğaltma grubu için bir depolama alanı geçişi gerçekleştirme. Tamamlandıktan sonra VMM ve Site Recovery yeni VM algılamak ve Site kurtarma yönetme başlatın. Gölge VM ikincil sitede oluşturulur ve böylece yük devretme sonrasında çoğaltma VM'de başlatılabilir kapasitesi ayrılır.

![Korumayı etkinleştir](./media/site-recovery-vmm-san/enable-protect.png)

Sanal makineleri çoğaltma için etkinleştirildikten sonra Site Recovery konsolunda görünürler. VM özelliklerini görüntüleme, izleme durumu ve birden fazla VM içermesi yük devretme çoğaltma grupları izlemek. SAN Çoğaltmada, çoğaltma grubuyla ilişkili tüm sanal makineleri birlikte yük devri gerekir. Depolama katmanında önce yük devretme gerçekleşirse olmasıdır. Birlikte, yalnızca ilgili VM'ler yer ve çoğaltma grupları düzgün gruplandırmak önemlidir.

> [!NOTE]
> Bir sanal makine için çoğaltmayı etkinleştirdikten sonra bir Site Recovery çoğaltma grubunda bulundukları sürece, VHD LUN'ları eklemeyin. Bir Site Recovery çoğaltma grubunda bulunuyorsa VHD'ler yalnızca Site Recovery tarafından algılanır.
>
>

İlk çoğaltma dahil olmak üzere ilerleme durumunu izleyebilirsiniz **işleri** sekmesi. Korumayı Sonlandır işini çalıştıktan sonra sanal makine yük devretme için hazır olur.

![Sanal makine koruma işi](./media/site-recovery-vmm-san/job-props.png)

## <a name="step-8-test-the-deployment"></a>8. adım: dağıtımı test etme

Dağıtımınızı VM'ler devreden beklendiği gibi üzerinde emin olmak için test edin. Bunu yapmak için bir kurtarma planı oluşturma ve yük devretme testi çalıştırın.

1. Üzerinde **kurtarma planları** sekmesini tıklatın, **kurtarma planı oluşturma**.
2. Kurtarma planı için bir ad belirtin ve kaynak ve hedef VMM sunucuları seçin. Kaynak sunucu yük devretme ve kurtarma için etkinleştirilmiş sanal makineleri olması gerekir. Seçin **SAN** SAN çoğaltması için yapılandırılmış olan Bulutlar görüntülemek için.

    ![Kurtarma planı oluşturma](./media/site-recovery-vmm-san/r-plan.png)

3. İçinde **sanal makineleri seçin**, çoğaltma gruplarını seçin. Grupla ilişkili tüm sanal makineleri kurtarma planına eklenir. Bu sanal makineleri kurtarma planının varsayılan grubu (Grup 1) eklenir. Gerekirse, daha fazla grup ekleyebilirsiniz. Çoğaltma işleminden sonra sanal makineleri kurtarma planı grupları sıraya göre numaralandırılır.

    ![Sanal makineleri seçin](./media/site-recovery-vmm-san/r-plan-vm.png)
4. Kurtarma planı oluşturulduktan sonra listede göründüğü **kurtarma planları** sekmesi. Planı seçin ve Seç **yük devretme testi**.
5. Üzerinde **onaylayın yük devretme testi** sayfasında, **hiçbiri**. Bu seçeneği etkinleştirdiğinizde, başarısız çoğaltma sanal makineleri üzerinde herhangi bir ağa bağlanmayacaktır. Bu, sanal makineleri üzerinde beklendiği gibi başarısız, ancak ağ ortamında test değil sınar. Diğer ağ seçenekleri hakkında daha fazla bilgi için bkz: [Site Recovery yük devretme](site-recovery-failover.md).

    ![Select test ağı](./media/site-recovery-vmm-san/test-fail1.png)

6. Test VM olarak çoğaltma VM'de bulunduğu ana bilgisayar aynı ana bilgisayardaki oluşturulur. Çoğaltma VM bulunur bulut eklenmez.
2. Çoğaltma işleminden sonra çoğaltma VM birincil sanal makine daha farklı bir IP adresi gerekir. DHCP'den adresleri veren, otomatik olarak güncelleştirilir. DHCP kullanmıyorsanız ve aynı adreslerini istiyorsanız birkaç betikleri çalıştırmanız gerekir.
3. IP adresi almak için bu komut dosyasını çalıştırın:

       $vm = Get-SCVirtualMachine -Name <VM_NAME>
       $na = $vm[0].VirtualNetworkAdapters>
       $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
       $ip.address  

4. DNS'yi güncelleştirmek için bu örnek komut dosyasını çalıştırın. Alınan IP adresi belirtin.

       [string]$Zone,
       [string]$name,
       [string]$IP
       )
       $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
       $newrecord = $record.clone()
       $newrecord.RecordData[0].IPv4Address  =  $IP
       Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord


## <a name="next-steps"></a>Sonraki adımlar

Ortamınızı beklendiği gibi çalıştığından emin olun, görmek için yük devretme testi çalıştırdıktan sonra [Site Recovery yük devretme](site-recovery-failover.md) yerine farklı türler hakkında bilgi edinmek için.
