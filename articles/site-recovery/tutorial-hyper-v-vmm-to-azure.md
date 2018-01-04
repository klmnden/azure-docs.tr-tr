---
title: "Azure Site Recovery ile azure'a VMM bulutlarındaki şirket içi Hyper-V sanal makineleri, olağanüstü durum kurtarma ayarlama | Microsoft Docs"
description: "System Center VMM bulutlarında, Azure Site Recovery hizmeti ile şirket içi Hyper-V sanal makineleri, olağanüstü durum kurtarma ayarlanacağını öğrenin."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 12/31/2017
ms.author: raynew
ms.openlocfilehash: 5f364c6f8a88ad53c12909f0e9f10afcce5ef8af
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
# <a name="set-up-disaster-recovery-of-on-premises-hyper-v-vms-in-vmm-clouds-to-azure"></a>Azure'a VMM bulutlarındaki şirket içi Hyper-V sanal makineleri, olağanüstü durum kurtarma ayarlayın

[Azure Site Recovery](site-recovery-overview.md) hizmeti yönetme ve çoğaltma, yük devretme ve yeniden çalışma şirket içi makineler ve Azure sanal makineleri (VM'ler) yönetme olağanüstü durum kurtarma stratejiniz katkıda bulunur.

Bu öğretici, şirket içi Hyper-V sanal makineleri olağanüstü durum kurtarma Azure ayarlamak nasıl gösterir. Öğretici, System Center Virtual Machine Manager (VMM) tarafından yönetilen Hyper-V VM'ler için geçerlidir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Çoğaltma kaynağı ve hedef seçin.
> * Şirket içi Site Recovery bileşenleri de dahil olmak üzere kaynak çoğaltma ortamı ve hedef çoğaltma ortamı ayarlayın.
> * VMM VM ağları ve Azure sanal ağlar arasında eşleme için Ağ eşlemesi, ayarlayın.
> * Çoğaltma ilkesi oluşturma
> * Bir sanal makine için çoğaltmayı etkinleştirme

Bir dizi üçüncü öğreticide budur. Bu öğreticinin önceki eğitimlerine görevleri önceden tamamlamış varsayılır:

1. [Azure’u hazırlama](tutorial-prepare-azure.md)
2. [Şirket içi Hyper-V hazırlama](tutorial-prepare-on-premises-hyper-v.md)

Başlamadan önce için yararlı [mimarisi gözden](concepts-hyper-v-to-azure-architecture.md) bu olağanüstü durum kurtarma senaryosuna.



## <a name="select-a-replication-goal"></a>Çoğaltma hedefi seçin

1. İçinde **tüm hizmetleri** > **kurtarma Hizmetleri kasaları**, bu eğitimlerine kullanırız kasa adını tıklatın **ContosoVMVault**.
2. İçinde **Başlarken**, tıklatın **Site Recovery**. Ardından **altyapıyı hazırlama**
3. İçinde **koruma hedefi** > **bulunan makinelerinizi nerede**seçin **şirket içi**.
4. İçinde **istediğiniz makinelerinizi çoğaltmak**seçin **için Azure**.
5. İçinde **sanallaştırılmış, makine**seçin **Evet, Hyper-V ile**.
6. İçinde **System Center VMM kullanıyorsanız**seçin **Evet**. Daha sonra, **Tamam**'a tıklayın.

    ![Çoğaltma hedefi](./media/tutorial-hyper-v-vmm-to-azure/replication-goal.png)



## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Kaynak ortamını ayarlama, Azure Site kurtarma Sağlayıcısı'nı ve Azure kurtarma Hizmetleri aracısını yükleyin ve şirket içi sunucuları kasaya kaydedin. 

1. İçinde **altyapıyı hazırlama**, tıklatın **kaynak**.
2. **Kaynağı ayarla** kısmında, bir VMM sunucusu eklemek için **+ VMM**'ye tıklayın. İçinde **Sunucu Ekle**, denetleyin **System Center VMM sunucusunun** görünür **sunucu türü**.
3. Microsoft Azure Site Recovery sağlayıcısı için yükleyiciyi indirin.
4. Kasa kayıt anahtarını indir Sağlayıcı Kurulum çalıştırdığınızda bu gerekir. Anahtar, oluşturulduktan sonra beş gün boyunca geçerlidir.
5. Kurtarma Hizmetleri Aracısı'nı indirin.

    ![İndirme](./media/tutorial-hyper-v-vmm-to-azure/download-vmm.png)

### <a name="install-the-provider-on-the-vmm-server"></a>Sağlayıcıyı VMM sunucusuna yükleme

1. Azure Site kurtarma Sağlayıcısı Kurulumu Sihirbazı'nda > **Microsoft Update**, sağlayıcı güncelleştirmeleri denetlemek için Microsoft Update'i kullanmayı kabul.
2. İçinde **yükleme**sağlayıcı varsayılan yükleme konumunu kabul edin ve tıklatın **yükleme**. 
3. Yüklemeden sonra Microsoft Azure Site Kurtarma Kayıt Sihirbazı'ndaki > **kasa ayarlarını**, tıklatın **Gözat**hem de **anahtar dosyası**seçin kasa anahtarı dosyasını indirilen.
4. Azure Site Recovery aboneliğini ve kasa adını belirtin (**ContosoVMVault**). VMM sunucusunun kasada tanımlamak için kolay bir ad belirtin.
5. İçinde **Proxy ayarlarını**seçin **Azure Site Recovery Ara sunucu olmadan doğrudan bağlan**.
6. Verileri şifrelemek için kullanılan sertifika için varsayılan konumu kabul edin. Üzerinden başarısız olduğunda şifrelenen verilerin şifresi çözülür.
7. İçinde **Eşitle bulut meta verileri**seçin **bulut meta verilerini Site Recovery portalına Eşitle**. Bu eylemin her sunucuda yalnızca bir kez gerçekleştirilmesi gerekir. Ardından **kaydetmek**.
8. Sunucuyu kasaya kaydedildikten sonra tıklatın **son**.

Kayıt tamamlandıktan sonra sunucu meta verilerini Azure Site Recovery tarafından alınır ve VMM sunucusu görüntülenen **Site Recovery altyapısı**.

### <a name="install-the-recovery-services-agent"></a>Kurtarma Hizmetleri Aracısı'nı yükleme

Aracı çoğaltmak istediğiniz Vm'leri içeren her Hyper-V ana bilgisayara yükleyin.

1. Microsoft Azure kurtarma Hizmetleri Aracısı Kurulum Sihirbazı'ndaki > **Önkoşul denetimi**, tıklatın **sonraki**. Eksik Önkoşullar otomatik olarak yüklenir.
2. İçinde **yükleme ayarları**, yükleme ve önbellek konumunu kabul edin. Ön bellek sürücüsünde en az 5 GB depolama alanı gerekiyor. 600 GB veya daha fazla boş alanı olan bir sürücü öneririz. Ardından **Yükle**'ye tıklayın.
3. İçinde **yükleme**yükleme tamamlandığında, tıklatın **Kapat** Sihirbazı tamamlamak için.

    ![Aracı yükleme](./media/tutorial-hyper-v-vmm-to-azure/mars-install.png)
    

## <a name="set-up-the-target-environment"></a>Hedef ortamı ayarlama

1. Tıklatın **altyapıyı hazırlama** > **hedef**.
2. Abonelik ve kaynak grubunu seçin (**ContosoRG**) yük devretme sonrasında Azure sanal makinelerini oluşturulacağı içinde.
3. Seçin **Resource Manager "** dağıtım modeli.

Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler.


## <a name="configure-network-mapping"></a>Ağ eşlemesini yapılandırma

1. **Site Recovery Altyapısı** > **Ağ eşlemeleri** > **Ağ Eşleme** bölümünde, **+Ağ Eşlemesi** simgesine tıklayın.
2. İçinde **ağ eşlemesi Ekle**, kaynak VMM sunucusunu seçin. Seçin **Azure** hedefi olarak.
3. Yük devretme işleminden sonra dağıtım modelini ve aboneliği seçin.
4. İçinde **kaynak ağ**, kaynak şirket içi VM ağını seçin.
5. İçinde **hedef ağ**, çoğaltılan Azure Vm'lerinin Azure ağında konumlandırılacağı yük devretme sonrasında oluşturulduğunda seçin. Daha sonra, **Tamam**'a tıklayın.

    ![Ağ eşlemesi](./media/tutorial-hyper-v-vmm-to-azure/network-mapping-vmm.png)

## <a name="set-up-a-replication-policy"></a>Bir çoğaltma ilkesini ayarlayın

1. Tıklatın **altyapıyı hazırlama** > **çoğaltma ayarları** > **+ oluştur ve ilişkilendir**.
2. İçinde **ilke oluştur ve ilişkilendir**, ilke adını belirtebilir **ContosoReplicationPolicy**.
3. Varsayılan ayarları bırakın ve tıklayın **Tamam**.
    - **Kopyalama sıklığı** bu delta gösterir (sonra ilk çoğaltma) verileri her beş dakikada çoğaltmak.
    - **Kurtarma noktası bekletme** her kurtarma noktası bekletme windows iki olacağını gösterir iki saat.
    - **Uygulamayla tutarlı anlık görüntü sıklığı** uygulamayla tutarlı anlık görüntüleri içeren kurtarma noktaları her saat oluşturulacak gösterir.
    - **İlk çoğaltma başlangıç zamanı**, ilk çoğaltma hemen başlayacak gösterir.
    - **Azure'da depolanan verileri şifrele** -varsayılan **kapalı** ayarını gösterir bekleyen şifreli veriler Azure içinde değil.
4. İlkesi oluşturulduktan sonra tıklatın **Tamam**. Yeni bir ilke oluşturduğunuzda, bu ilke otomatik olarak VMM bulutuyla ilişkilendirilir.

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

1. İçinde **uygulama çoğaltma**, tıklatın **kaynak**. 
2. İçinde **kaynak**, VMM Bulutu seçin. Daha sonra, **Tamam**'a tıklayın.
3. İçinde **hedef**, kasa abonelik hedefi olarak Azure doğrulayın ve seçin **Resource Manager** modeli.
4. Seçin **contosovmsacct1910171607** depolama hesabı ve **ContosoASRnet** Azure ağı.
5. İçinde **sanal makineleri** > **seçin**, çoğaltmak istediğiniz VM seçin. Daha sonra, **Tamam**'a tıklayın.

 İlerleme durumunu izleyebilirsiniz **korumayı etkinleştir** eylemde **işleri** > **Site Recovery işleri**. Sonra **korumayı Sonlandır** işi tamamlandığında, ilk çoğaltma tamamlandıktan ve VM yük devretme için hazırdır.


## <a name="next-steps"></a>Sonraki adımlar
[Olağanüstü durum kurtarma tatbikatı çalıştırma](tutorial-dr-drill-azure.md)
