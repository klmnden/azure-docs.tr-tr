---
title: "Azure Site Recovery ile azure'a şirket içi Hyper-V VM'ler için olağanüstü durum kurtarma ayarlama | Microsoft Docs"
description: "Olağanüstü durum kurtarma şirket içi Hyper-V Vm'lerini azure'a Azure Site Recovery hizmeti ile ayarlama öğrenin."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 45e9b4dc-20ca-4c14-bee3-cadb8d9b8b24
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2017
ms.author: raynew
ms.openlocfilehash: 4d43fb03ce1c54a47315b8c3a5c83ec2082bcab9
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2017
---
# <a name="set-up-disaster-recovery-of-on-premises-hyper-v-vms-to-azure"></a>Şirket içi Hyper-V sanal makineleri olağanüstü durum kurtarma Azure ayarlama

[Azure Site Recovery](site-recovery-overview.md) hizmeti yönetme ve çoğaltma, yük devretme ve yeniden çalışma şirket içi makineler ve Azure sanal makineleri (VM'ler) yönetme olağanüstü durum kurtarma stratejiniz katkıda bulunur.

Bu öğretici, şirket içi Hyper-V sanal makineleri olağanüstü durum kurtarma Azure ayarlamak nasıl gösterir. Öğretici, System Center Virtual Machine Manager (VMM) bulutlarında yönetilen Hyper-V VM'ler ve olmayan olanlar için geçerlidir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure ve şirket içi önkoşulları ayarlama
> * Site kurtarma için bir kurtarma Hizmetleri kasası oluşturma 
> * Kaynağı ayarlama ve çoğaltma ortamları hedef 
> * (Hyper-V System Center VMM tarafından yönetiliyorsa) ağ eşlemesi ayarlamak
> * Çoğaltma ilkesi oluşturma
> * Bir sanal makine için çoğaltmayı etkinleştirme


## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

- Anladığınızdan emin olun [senaryo mimarisi ve bileşenleri](concepts-hyper-v-to-azure-architecture.md).
- Gözden geçirme [destek gereksinimleri](site-recovery-support-matrix-to-azure.md) tüm bileşenler için.
- Çoğaltmak istediğiniz sanal makineleri ile uyumlu olduğundan emin olun [Azure VM gereksinimleri](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
- Kapasite planlama bulmak için:
    - Kullanım [Hyper-V kapasite planlayıcısı aracı](http://aka.ms/asr-capacity-planner-excel), karmaşıklık oranı bulmak için.
    - Kullanım [Site kurtarma kapasite Planlayıcısı](http://aka.ms/asr-capacity-planner-excel) kaynak ortamınızı çözümlemek için bant genişliği tahmin etmek ve hedef gereksinimleri.
- Azure hazırlayın. Bir Azure aboneliği, Azure sanal ağı ve bir depolama hesabı gerekir.
- Şirket içi Hyper-V konaklarını ve VMM sunucularını ilgiliyse hazırlayın.





### <a name="set-up-an-azure-account"></a>Bir Azure hesabı ayarlama

Bir Microsoft alma [Azure hesabı](http://azure.microsoft.com/).

- [Ücretsiz deneme sürümüyle](https://azure.microsoft.com/pricing/free-trial/) başlayabilirsiniz.
- Hakkında bilgi edinin [Site Recovery fiyatlandırma](site-recovery-faq.md#pricing)ve alma [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/site-recovery/).
- Hangi bulma [bölgeleri desteklenir](https://azure.microsoft.com/pricing/details/site-recovery/) Site kurtarma.

### <a name="verify-azure-account-permissions"></a>Azure hesabı izinleri doğrulayın

Azure hesabınızın sanal makineleri çoğaltmak için gereken izinleri olduğundan emin olun.

- Gözden geçirme [izinleri](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) makineleri azure'a gerekir
- Doğrulama ve değiştirme [rol tabanlı erişim](../active-directory/role-based-access-control-configure.md) izinleri. 


### <a name="set-up-an-azure-network"></a>Azure ağı ayarlama

Ayarlanmış bir [Azure ağ](../virtual-network/virtual-network-get-started-vnet-subnet.md).

- Yük devretme sonrasında oluşturulduğunda azure sanal makineleri bu ağda yerleştirilir.
- Ağın, Kurtarma Hizmetleri kasasıyla aynı konumda olması gerekir.


### <a name="set-up-an-azure-storage-account"></a>Azure depolama hesabı ayarlama

Ayarlanmış bir [Azure depolama hesabı](../storage/common/storage-create-storage-account.md#create-a-storage-account).

- Site Recovery, şirket içi makineler Azure depolama alanına çoğaltır. Yük devretme gerçekleştikten sonra azure VM'ler depolama biriminden oluşturulur.
- Depolama hesabı kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.
- Depolama hesabı standart olabilir veya [premium](../virtual-machines/windows/premium-storage.md).
- Bir premium hesabı ayarlamanız, ek bir standart hesap için günlük veri gerekir.

### <a name="prepare-hyper-v-hosts"></a>Hyper-V ana bilgisayarları hazırlama

1. Hyper-V konakları karşılayan doğrulayın [destek gereksinimleri](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers).
2. Ana bilgisayarlar bu URL'leri erişebildiğinden emin olun:

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
    - Herhangi bir IP adresi tabanlı güvenlik duvarı kuralı Azure ile iletişim kurmaya izin vermelidir.
    - [Azure Veri Merkezi IP Aralıkları](https://www.microsoft.com/download/confirmation.aspx?id=41653)'na ve HTTPS (443) bağlantı noktasına izin verin.
    - IP adres aralıklarını aboneliğinizin Azure bölgesi ve Batı ABD (erişim denetimi ve kimlik yönetimi için kullanılan) izin verir.

### <a name="prepare-vmm-servers"></a>VMM sunucuları hazırlama

Hyper-V konakları VMM tarafından yönetiliyorsa, şirket içi VMM sunucusu hazırlamanız gerekir. 

- VMM sunucusu karşıladığını doğrulamak [destek gereksinimleri](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers).
- VMM sunucusu bir veya daha fazla konak grubu olan en az bir buluta sahip olduğundan emin olun. Sanal makineleri çalıştırdığınız Hyper-V konağı bulutta bulunmalıdır.
- VMM sunucusunun aşağıdaki URL'lere erişim ile internet erişimi gerekir:

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
    - Herhangi bir IP adresi tabanlı güvenlik duvarı kuralı Azure ile iletişim kurmaya izin vermelidir.
    - [Azure Veri Merkezi IP Aralıkları](https://www.microsoft.com/download/confirmation.aspx?id=41653)'na ve HTTPS (443) bağlantı noktasına izin verin.
    - Aboneliğinizin Azure bölgesi ve Batı ABD (Access Control ve Identity Management için kullanılır) için IP adresi aralıklarına izin verin.
- VMM sunucusu, ağ eşlemesi için hazırlayın.


#### <a name="prepare-vmm-for-network-mapping"></a>VMM ağ eşlemesi için hazırlanma

VMM, kullanıyorsanız [ağ eşlemesi](site-recovery-network-mapping.md) şirket içi VMM VM ağları ve Azure sanal ağlar arasındaki eşlemeleri. Eşleme, yük devretme sonrasında oluşturulduğunda Azure VM'ler için doğru ağ bağlanmasını sağlar.

VMM, ağ eşlemesi için aşağıdaki gibi hazırlayın:

1. Olduğundan emin olun bir [VMM mantıksal ağı](https://docs.microsoft.com/system-center/vmm/network-logical) , Hyper-V konaklarının bulunduğu bulutla ilişkili.
2. Olduğundan emin olun bir [VM ağı](https://docs.microsoft.com/system-center/vmm/network-virtual) mantıksal ağa bağlı.
3. VM'ler, VM ağına bağlayın.

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]


## <a name="select-a-protection-goal"></a>Koruma hedefi seçin

Ne çoğaltılacağı ve için çoğaltma yeri seçin.

1. Kasaya tıklayın **Site Recovery** > **altyapıyı hazırlama** > **koruma hedefi**.
2. İçinde **koruma hedefi**seçin **için Azure**> **Evet, Hyper-V ile**. 
    - Hyper-V konakları VMM tarafından yönetilmeyen, seçin **Hayır** değil VMM kullandığınızı doğrulamak için.
    - Konaklar VMM bulutlarında yönetilen, seçin **Evet**.

## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Kaynak ortamını ayarlama, Azure Site kurtarma Sağlayıcısı'nı ve Azure kurtarma Hizmetleri aracısını yükleyin ve şirket içi sunucuları kasaya kaydedin. 

1. İçinde **altyapıyı hazırlama**, tıklatın **kaynak**. 
    - VMM kullanmıyorsanız tıklatın **+ Hyper-V sitesi**ve bir site adı belirtin. Ardından **+ Hyper-V Server**ve siteye konağa veya kümeye ekleyin.
    - İçinde VMM, kullanıyorsanız **kaynağı**, tıklatın **+ VMM** bir VMM sunucusu eklemek için. İçinde **Sunucu Ekle**, denetleyin **System Center VMM sunucusunun** görünür **sunucu türü**.
2. Ortamınıza bağlı olarak sağlayıcı ve aracı bileşenlerine indirin.
    - Hyper-V için yalnızca, sağlayıcı yükleme dosyasını indirin. Dosya sağlayıcı ve aracı yüklemek için her Hyper-V ana bilgisayar üzerinde çalıştırın.
        
        ![Sağlayıcı VMM olmadan](./media/tutorial-hyper-v-to-azure/download-no-vmm.png)
    
    - Hyper-V için VMM ile sağlayıcı ve aracının ayrı olarak yükleyin. VMM sunucusunda sağlayıcı yüklemesini çalıştırın. Her Hyper-V ana bilgisayarda aracı yüklemesini çalıştırın.
    
        ![Sağlayıcı ve aracı VMM ile](./media/tutorial-hyper-v-to-azure/download-vmm.png)
    
3. Kasa kayıt anahtarını indir Sağlayıcı Kurulum çalıştırdığınızda bu gerekir. Anahtar, oluşturulduktan sonra beş gün boyunca geçerlidir.

### <a name="install-components"></a>Bileşenlerini yükle

Tablodaki summmarized olarak bileşenlerini yükleyin. 

**Bileşen** | **Ayrıntılar** | **Hyper-V yalnızca** | **VMM ile Hyper-V**
--- | --- | --- | ---
**Azure Site Recovery sağlayıcısı** | Azure'a çoğaltma için düzenler | Her Hyper-V konak üzerinde yükleyin | VMM sunucusuna yükleyin
**Kurtarma Hizmetleri Aracısı** | Veri çoğaltma işleme | Her Hyper-V konak üzerinde yükleyin | Hyper-V konak üzerinde yükleyin


#### <a name="install-the-provider-on-hyper-v-without-vmm"></a>Sağlayıcı VMM olmadan Hyper-V yükleyin

Hyper-V siteye eklenen her Hyper-V ana bilgisayarda sağlayıcısını yükleyin. Hyper-V kümesi üzerinde yüklüyorsanız, her küme düğümünde Kurulumu çalıştırın. Düğümleri arasında geçirmek olsa bile yükleme ve her Hyper-V küme düğümü kaydetme VM'ler korunduğunu sağlar.

1. **Microsoft Update** kısmından güncelleştirmeleri seçebilirsiniz. Böylece, Sağlayıcı güncelleştirmeleri Microsoft Update ilkenize uygun şekilde yüklenir.
2. **Yükleme** bölümünde varsayılan Sağlayıcı yükleme konumunu kabul edin veya değiştirin ve **Yükle**'ye tıklayın.
4. İçinde **kasa ayarlarını**, tıklatın **Gözat** indirdiğiniz kasa anahtarı dosyasını seçin. Azure Site Recovery abonelik, kasa adını ve Hyper-V sunucusuna ait olduğu Hyper-V sitesi belirtin.
5. İçinde **Proxy ayarlarını**, VMM sunucusunun veya Hyper-V ana bilgisayar üzerinde çalışan sağlayıcı internet üzerinden Site Recovery'e nasıl bağlandığını belirtin.
    * Doğrudan bir bağlantı için seçin **Azure Site Recovery Ara sunucu olmadan doğrudan bağlan**.
    * Bir proxy seçin **Azure Site Recovery proxy sunucu kullanma Bağlan**. Gerekiyorsa proxy adresi, bağlantı noktası ve kimlik bilgilerini belirtin.
6. Sunucuyu kasaya kaydedildikten sonra tıklatın **son**.

Hyper-V sunucusundan meta veriler, Azure Site Recovery tarafından alınır ve sunucu görüntülenen **Site Recovery altyapısı** > **Hyper-V konakları**.


#### <a name="install-the-recovery-services-agent-on-hyper-v-host-without-vmm"></a>Kurtarma Hizmetleri aracısını Hyper-V konağı VMM olmadan yükleyin

Dağıtımınızda VMM kullanıyorsanız, Kurtarma Hizmetleri Aracısı Kurulum her Hyper-V ana bilgisayarda çalıştırın.

1. Kurulum Sihirbazı'ndaki > **Önkoşul denetimi**, tıklatın **sonraki**. Eksik önkoşullar otomatik olarak yüklenir.

    ![Kurtarma Hizmetleri Aracısı Önkoşulları](./media/tutorial-hyper-v-to-azure/hyperv-agent-prerequisites.png)
3. **Yükleme Ayarları**’nda yükleme ve önbellek konumunu kabul edin veya değiştirin. Ön bellek sürücüsünde en az 5 GB depolama alanı gerekiyor. 600 GB veya daha fazla boş alanı olan bir sürücü öneririz. Ardından **Yükle**'ye tıklayın.
4. İçinde **yükleme**yükleme tamamlandığında, tıklatın **Kapat** Sihirbazı tamamlamak için.

##### <a name="set-up-internet-access-via-a-proxy"></a>Bir proxy sunucu üzerinden internet erişimi ayarlama

Kurtarma Hizmetleri aracısını Hyper-V konağı VM çoğaltması için Azure internet erişimi gerekir. İnternet'e bir ara sunucu aracılığıyla erişiyorsanız ara sunucuyu aşağıdaki gibi ayarlayın:

1. Hyper-V ana bilgisayarındaki Microsoft Azure Backup MMC ek bileşenini açın. Varsayılan olarak, Microsoft Azure yedekleme için bir kısayol masaüstünde veya C:\Program Files\Microsoft Azure Recovery Services agent\bin\wabadmin yolunda kullanılabilir.
2. Ek bileşende **Özellikleri Değiştir**'e tıklayın.
3. Üzerinde **Proxy Yapılandırması** sekmesinde, proxy bilgilerini belirtin.

    ![MARS Aracısı'nı Kaydetme](./media/tutorial-hyper-v-to-azure/mars-proxy.png)
4. Aracı ulaşabilir onay [URL'leri gerekli](#prepare-hyper-v-hosts).

#### <a name="install-the-provider-on-the-vmm-server-hyper-v-with-vmm"></a>(Hyper-V VMM ile) VMM sunucusunda Sağlayıcı yükleme

1. **Microsoft Update** kısmından güncelleştirmeleri seçebilirsiniz. Böylece, Sağlayıcı güncelleştirmeleri Microsoft Update ilkenize uygun şekilde yüklenir.
2. **Yükleme** bölümünde varsayılan Sağlayıcı yükleme konumunu kabul edin veya değiştirin ve **Yükle**'ye tıklayın. 
3. Artık VMM sunucusunu kasaya kaydedin. İçinde **kasa ayarlarını**, tıklatın **Gözat** indirdiğiniz kasa anahtarı dosyasını seçin. Azure Site Recovery aboneliğini ve kasa adını belirtin.

    ![Sunucu kaydı](./media/tutorial-hyper-v-to-azure/provider-vault-settings.png)

4. İçinde **Proxy ayarlarını**, VMM sunucusunun veya Hyper-V ana bilgisayar üzerinde çalışan sağlayıcı internet üzerinden Site Recovery'e nasıl bağlandığını belirtin.

    * Doğrudan bir bağlantı için seçin **Azure Site Recovery Ara sunucu olmadan doğrudan bağlan**.
    * Bir proxy seçin **Azure Site Recovery proxy sunucu kullanma Bağlan**. Gerekiyorsa proxy adresi, bağlantı noktası ve kimlik bilgilerini belirtin.
    - VMM RunAs hesabı (DRAProxyAccount), belirtilen proxy kimlik bilgileri kullanılarak otomatik olarak oluşturulur. Bu hesabın kimlik doğrulamasını başarıyla gerçekleştirebilmesi için ara sunucuyu yapılandırın. RunAs hesabı ayarları VMM konsolundan değiştirilebilir > **ayarları** > **güvenlik** > **farklı çalıştır hesapları**. Değişiklikleri güncelleştirme için VMM hizmetini yeniden başlatın.
5. İçinde **veri şifrelemesi**, şifrelenmiş Azure'a gönderilen tüm veriler belirtin. Bu seçeneği belirlerseniz Site Recovery bir sertifika verir. Daha sonra verilerin şifresini çözmek için bu sertifika gerekir.
6. İçinde **VMM sunucusu**, kasadaki VMM sunucusunu tanımlamak için bir kolay ad belirtin. Küme yapılandırmasında VMM kümesi rolü adını belirtin. İçinde **Eşitle bulut meta verilerini kasayla**VMM sunucusundaki tüm bulutların meta verilerini kasayla eşitlemek istediğinizi seçin. Bu eylemin her sunucuda yalnızca bir kez gerçekleştirilmesi gerekir. Tüm Bulutları eşitlemek istemiyorsanız bu ayarı işaretsiz bırakın ve her Bulutu VMM konsolundaki bulut özelliklerinde tek tek eşitleyin.

Kayıt tamamlandıktan sonra sunucu meta verilerini Azure Site Recovery tarafından alınır ve VMM sunucusu görüntülenen **Site Recovery altyapısı**.






## <a name="set-up-the-target-environment"></a>Hedef ortamı ayarlama

Seçin ve hedef kaynaklarını doğrulayın. 

1. Tıklatın **altyapıyı hazırlama** > **hedef**.
2. Abonelik ve Azure sanal makinelerini yük devretme sonrasında oluşturulacak kaynak grubu seçin. Azure VM'ler için kullanmak istediğiniz dağıtım modelini seçin.

3. Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler.

## <a name="configure-network-mapping-with-vmm"></a>(VMM ile) ağ eşlemesini yapılandırma

VMM kullanıyorsanız, ağ eşlemesi ayarlayın.

1. **Site Recovery Altyapısı** > **Ağ eşlemeleri** > **Ağ Eşleme** bölümünde, **+Ağ Eşlemesi** simgesine tıklayın.
2. İçinde **ağ eşlemesi Ekle**, kaynak VMM sunucusunu seçin. Seçin **Azure** hedefi olarak.
3. Yük devretme işleminden sonra dağıtım modelini ve aboneliği seçin.
4. İçinde **kaynak ağ**, kaynak şirket içi VM ağını seçin.
5. İçinde **hedef ağ**, çoğaltılan Azure Vm'lerinin Azure ağında konumlandırılacağı yük devretme sonrasında oluşturulduğunda seçin. Daha sonra, **Tamam**'a tıklayın.

    ![Ağ eşlemesi](./media/tutorial-hyper-v-to-azure/network-mapping-vmm.png)

## <a name="create-a-replication-policy"></a>Çoğaltma ilkesi oluşturma

1. Tıklatın **altyapıyı hazırlama** > **çoğaltma ayarları** > **+ oluştur ve ilişkilendir**.
2. **İlke oluştur ve ilişkilendir** bölümünde bir ilke adı belirtin.
3. **Kopyalama sıklığı** kısmında, ilk çoğaltmadan sonra değişim verilerini ne sıklıkta çoğaltacağınızı belirleyin (30 saniyede, 5 veya 15 dakikada bir).

    > [!NOTE]
    >  Premium depolama hesabına çoğaltırken 30 saniyelik aralık desteklenmez. Sınırlama premium depolama tarafından desteklenen blob başına anlık görüntü sayısıyla (100) belirlenir. [Daha fazla bilgi edinin](../virtual-machines/windows/premium-storage.md#snapshots-and-copy-blob).

4. İçinde **kurtarma noktası bekletme**, ne kadar bekletme aralığı (saat) için her kurtarma noktası belirtin. Korumalı makineler, bu süre içindeki herhangi bir noktaya kurtarılabilir.
5. **Uygulamayla tutarlı anlık görüntü sıklığı** kısmında, uygulamayla tutarlı anlık görüntüleri içeren kurtarma noktasının hangi sıklıkta oluşturulacağını (1-12 saat) belirtin. Hyper-V iki çeşit anlık kullanır:
    - **Standart anlık görüntü**: tüm sanal makinenin artımlı bir anlık görüntü sağlar.
    - **Uygulamayla tutarlı anlık görüntü**: VM içindeki uygulama verilerinin bir zaman içinde nokta anlık görüntüsünü alır. Birim Gölge Kopyası Hizmeti (VSS) anlık görüntü alınırken uygulamaların tutarlı bir durumda olmasını sağlar. Uygulamayla tutarlı anlık görüntüleri etkinleştirilmesi, kaynak VM'ler uygulama performansını etkiler. Yapılandırdığınız ilave kurtarma noktası sayısından küçük bir değere ayarlayın.
6. **İlk çoğaltma başlangıç zamanı** bölümünde, ilk çoğaltmanın başlatılacağı zamanını belirtin. Yoğun saatler dışında zamanlamak isteyebilirsiniz çoğaltma işlemi internet bant gerçekleşir.
7. **Azure'da depolanan verileri şifrele** bölümünde, Azure depolama alanındaki bekleyen verilerin şifrelenip şifrelenmeyeceğini belirtin. Daha sonra, **Tamam**'a tıklayın.

    ![Çoğaltma ilkesi](./media/tutorial-hyper-v-to-azure/replication-policy.png)


Yeni bir ilke oluşturduğunuzda, otomatik olarak VMM Bulutu veya Hyper-V sitesi ile ilişkili.

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme


1. Tıklatın **uygulama çoğaltma** > **kaynak**. 
2. İçinde **kaynak**, Hyper-V sitesi/VMM sunucusu/Bulutu seçin. Daha sonra, **Tamam**'a tıklayın.
3. İçinde **hedef**, hedef, kasa abonelik ve yük devretme sonrasında Azure içinde kullanmak istediğiniz modeli olarak Azure doğrulayın.
4. Çoğaltılan veriler ve hangi Azure Vm'lerinin yük devretme sonrasında yer alacağı Azure ağı için depolama hesabı seçin.
5. İçinde **sanal makineleri** > **seçin**, çoğaltmak istediğiniz sanal makineleri seçin. Daha sonra, **Tamam**'a tıklayın.

 İlerleme durumunu izleyebilirsiniz **korumayı etkinleştir** eylemde **işleri** > **Site Recovery işleri**. Sonra **korumayı Sonlandır** işi tamamlandığında, ilk çoğaltma tamamlandıktan ve sanal makine yük devretme için hazırdır.

## <a name="next-steps"></a>Sonraki adımlar
[Olağanüstü durum kurtarma tatbikatı çalıştırma](tutorial-dr-drill-azure.md)
