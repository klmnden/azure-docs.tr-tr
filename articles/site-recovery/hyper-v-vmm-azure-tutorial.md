---
title: Azure Site Recovery ile azure'a VMM bulutlarındaki şirket içi Hyper-V sanal makineleri, olağanüstü durum kurtarma ayarlama | Microsoft Docs
description: System Center VMM bulutlarında, Azure Site Recovery hizmeti ile şirket içi Hyper-V sanal makineleri, olağanüstü durum kurtarma ayarlanacağını öğrenin.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 05/02/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: a8bbbe0a5aca20222ff7385be9d0ecf0a4224d5c
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="set-up-disaster-recovery-of-on-premises-hyper-v-vms-in-vmm-clouds-to-azure"></a>Azure'a VMM bulutlarındaki şirket içi Hyper-V sanal makineleri, olağanüstü durum kurtarma ayarlayın

[Azure Site Recovery](site-recovery-overview.md) hizmeti, şirket içi makinelerin ve Azure sanal makinelerinin (VM) çoğaltma, yük devretme ve yeniden çalışma işlemlerini yöneterek ve düzenleyerek olağanüstü durum kurtarma stratejinize katkı sağlar.

Bu öğreticide, şirket içi Hyper-V sanal makineleri için Azure’da olağanüstü durum kurtarmanın nasıl ayarlanacağı gösterilmektedir. Öğretici, System Center Virtual Machine Manager (VMM) tarafından yönetilen Hyper-V VM'ler için geçerlidir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Çoğaltma kaynağınızı ve hedefinizi seçme.
> * Kaynak çoğaltma ortamını, şirket içi Site Recovery bileşenleri ve hedef çoğaltma ortamı dahil olmak üzere ayarlayın.
> * VMM VM ağları ve Azure sanal ağlar arasında eşleme için Ağ eşlemesi, ayarlayın.
> * Çoğaltma ilkesi oluşturma
> * VM için çoğaltmayı etkinleştirme

Bu, serideki üçüncü öğreticidir. Bu öğreticide, önceki öğreticilerdeki adımları zaten tamamladığınız varsayılır:

1. [Azure’u hazırlama](tutorial-prepare-azure.md)
2. [Şirket içi Hyper-V’leri hazırlama](tutorial-prepare-on-premises-hyper-v.md)

Başlamadan önce bu olağanüstü durum kurtarma senaryosu için [mimariyi gözden geçirmeniz](concepts-hyper-v-to-azure-architecture.md) yararlı olabilir.



## <a name="select-a-replication-goal"></a>Çoğaltma hedefi seçme

1. İçinde **tüm hizmetleri** > **kurtarma Hizmetleri kasaları**, bu eğitimlerine kullanırız kasa adını tıklatın **ContosoVMVault**.
2. **Başlarken** bölümünde **Site Recovery**’ye tıklayın. Daha sonra **Altyapıyı Hazırlama**’ya tıklayın
3. **Koruma hedefi** > **Makineleriniz nerede** bölümünde **Şirket içi** seçeneğini belirleyin.
4. **Makinelerinizi nereye çoğaltmak istiyorsunuz** bölümünde **Azure’a** seçeneğini belirleyin.
5. İçinde **sanallaştırılmış, makine**seçin **Evet, Hyper-V ile**.
6. İçinde **System Center VMM kullanıyorsanız**seçin **Evet**. Daha sonra, **Tamam**'a tıklayın.

    ![Çoğaltma hedefi](./media/hyper-v-vmm-azure-tutorial/replication-goal.png)



## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Kaynak ortamını ayarlama, Azure Site kurtarma Sağlayıcısı'nı ve Azure kurtarma Hizmetleri aracısını yükleyin ve şirket içi sunucuları kasaya kaydedin. 

1. **Altyapıyı Hazırlama** bölümünde **Kaynak** seçeneğine tıklayın.
2. **Kaynağı ayarla** kısmında, bir VMM sunucusu eklemek için **+ VMM**'ye tıklayın. İçinde **Sunucu Ekle**, denetleyin **System Center VMM sunucusunun** görünür **sunucu türü**.
3. Microsoft Azure Site Recovery sağlayıcısı için yükleyiciyi indirin.
4. Kasa kayıt anahtarını indirin. Sağlayıcı kurulumunu çalıştırırken buna ihtiyacınız olur. Anahtar, oluşturulduktan sonra beş gün boyunca geçerlidir.
5. Kurtarma Hizmetleri Aracısı'nı indirin.

    ![İndirme](./media/hyper-v-vmm-azure-tutorial/download-vmm.png)

### <a name="install-the-provider-on-the-vmm-server"></a>Sağlayıcıyı VMM sunucusuna yükleme

1. Azure Site Kurtarma Sağlayıcısı Kurulum sihirbazındaki **Microsoft Update** bölümünde, Sağlayıcı güncelleştirmelerini denetlemek için Microsoft Update’i kullanmayı kabul edin.
2. İçinde **yükleme**sağlayıcı varsayılan yükleme konumunu kabul edin ve tıklatın **yükleme**. 
3. Yüklemeden sonra Microsoft Azure Site Kurtarma Kayıt Sihirbazı'ndaki > **kasa ayarlarını**, tıklatın **Gözat**hem de **anahtar dosyası**seçin kasa anahtarı dosyasını indirilen.
4. Azure Site Recovery aboneliğini ve kasa adını belirtin (**ContosoVMVault**). VMM sunucusunun kasada tanımlamak için kolay bir ad belirtin.
5. **Proxy Ayarları** bölümünde **Proxy sunucusu olmadan doğrudan Azure Site Recovery hizmetine bağlan** seçeneğini belirleyin.
6. Verileri şifrelemek için kullanılan sertifika için varsayılan konumu kabul edin. Üzerinden başarısız olduğunda şifrelenen verilerin şifresi çözülür.
7. İçinde **Eşitle bulut meta verileri**seçin **bulut meta verilerini Site Recovery portalına Eşitle**. Bu eylemin her sunucuda yalnızca bir kez gerçekleştirilmesi gerekir. Ardından **kaydetmek**.
8. Sunucuyu kasaya kaydedildikten sonra tıklatın **son**.

Kayıt tamamlandıktan sonra sunucu meta verilerini Azure Site Recovery tarafından alınır ve VMM sunucusu görüntülenen **Site Recovery altyapısı**.

### <a name="install-the-recovery-services-agent"></a>Kurtarma Hizmetleri Aracısı'nı yükleme

Aracı çoğaltmak istediğiniz Vm'leri içeren her Hyper-V ana bilgisayara yükleyin.

1. Microsoft Azure kurtarma Hizmetleri Aracısı Kurulum Sihirbazı'ndaki > **Önkoşul denetimi**, tıklatın **sonraki**. Eksik Önkoşullar otomatik olarak yüklenir.
2. İçinde **yükleme ayarları**, yükleme ve önbellek konumunu kabul edin. Ön bellek sürücüsünde en az 5 GB depolama alanı gerekiyor. 600 GB veya daha fazla boş alanı olan bir sürücü öneririz. Ardından **Yükle**'ye tıklayın.
3. İçinde **yükleme**yükleme tamamlandığında, tıklatın **Kapat** Sihirbazı tamamlamak için.

    ![Aracı yükleme](./media/hyper-v-vmm-azure-tutorial/mars-install.png)
    

## <a name="set-up-the-target-environment"></a>Hedef ortamı ayarlama

1. **Altyapıyı Hazırlama** > **Hedef** seçeneklerine tıklayın.
2. Abonelik ve kaynak grubunu seçin (**ContosoRG**) yük devretme sonrasında Azure sanal makinelerini oluşturulacağı içinde.
3. **Kaynak Yöneticisi** dağıtım modelini seçin.

Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler.


## <a name="configure-network-mapping"></a>Ağ eşlemesini yapılandırma

1. **Site Recovery Altyapısı** > **Ağ eşlemeleri** > **Ağ Eşleme** bölümünde, **+Ağ Eşlemesi** simgesine tıklayın.
2. İçinde **ağ eşlemesi Ekle**, kaynak VMM sunucusunu seçin. Seçin **Azure** hedefi olarak.
3. Yük devretme işleminden sonra dağıtım modelini ve aboneliği seçin.
4. İçinde **kaynak ağ**, kaynak şirket içi VM ağını seçin.
5. İçinde **hedef ağ**, çoğaltılan Azure Vm'lerinin Azure ağında konumlandırılacağı yük devretme sonrasında oluşturulduğunda seçin. Daha sonra, **Tamam**'a tıklayın.

    ![Ağ eşlemesi](./media/hyper-v-vmm-azure-tutorial/network-mapping-vmm.png)

## <a name="set-up-a-replication-policy"></a>Çoğaltma ilkesi ayarlama

1. **Altyapıyı hazırlama** > **Çoğaltma Ayarları** > **+Oluştur ve ilişkilendir** seçeneklerine tıklayın.
2. **İlke oluştur ve ilişkilendir** bölümünde bir ilke adı (**ContosoReplicationPolicy**) belirtin.
3. Varsayılan ayarları değiştirmeden **Tamam**'a tıklayın.
    - **Kopyalama sıklığı**, değişim verilerinin (ilk çoğaltmadan sonra) her beş dakikada bir çoğaltılacağını belirtir.
    - **Kurtarma noktası bekletme**, her kurtarma noktası için bekletme pencerelerinin iki saat olacağını belirtir.
    - **Uygulamayla tutarlı anlık görüntü sıklığı**, uygulamayla tutarlı anlık görüntüleri içeren kurtarma noktalarının her saat oluşturulacağını belirtir.
    - **İlk çoğaltma başlangıç zamanı**, ilk çoğaltmanın hemen başlatılacağını belirtir.
    - **Azure'da depolanan verileri şifrele** -varsayılan **kapalı** ayarını gösterir bekleyen şifreli veriler Azure içinde değil.
4. İlke oluşturulduktan sonra **Tamam**’a tıklayın. Yeni bir ilke oluşturduğunuzda, bu ilke otomatik olarak VMM bulutuyla ilişkilendirilir.

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

1. **Uygulama çoğaltma** bölümünde **Kaynak** seçeneğine tıklayın. 
2. İçinde **kaynak**, VMM Bulutu seçin. Daha sonra, **Tamam**'a tıklayın.
3. İçinde **hedef**, kasa abonelik hedefi olarak Azure doğrulayın ve seçin **Resource Manager** modeli.
4. Seçin **contosovmsacct1910171607** depolama hesabı ve **ContosoASRnet** Azure ağı.
5. **Sanal makineler** > **Seç** bölümünde, çoğaltmak istediğiniz sanal makineyi seçin. Daha sonra, **Tamam**'a tıklayın.

 **İşler** > **Site Recovery işleri** bölümünde **Korumayı Etkinleştir** eyleminin ilerleme durumunu izleyebilirsiniz. Sonra **korumayı Sonlandır** işi tamamlandığında, ilk çoğaltma tamamlandıktan ve VM yük devretme için hazırdır.


## <a name="next-steps"></a>Sonraki adımlar
[Olağanüstü durum kurtarma tatbikatı çalıştırma](tutorial-dr-drill-azure.md)
