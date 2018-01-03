---
title: "Şirket içi Hyper-V Vm'lerini (VMM olmadan) Azure Site Recovery ile azure'a için olağanüstü durum kurtarma ayarlama | Microsoft Docs"
description: "Şirket içi Hyper-V Vm'lerini (VMM olmadan), olağanüstü durum kurtarma ayarlamak için Azure Azure Site Recovery hizmetiyle öğrenin."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 12/31`/2017
ms.author: raynew
ms.openlocfilehash: 633c14bd25bc8a1419196b2c76ca94c26db68991
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
# <a name="set-up-disaster-recovery-of-on-premises-hyper-v-vms-to-azure"></a>Şirket içi Hyper-V sanal makineleri olağanüstü durum kurtarma Azure ayarlama

[Azure Site Recovery](site-recovery-overview.md) hizmeti yönetme ve çoğaltma, yük devretme ve yeniden çalışma şirket içi makineler ve Azure sanal makineleri (VM'ler) yönetme olağanüstü durum kurtarma stratejiniz katkıda bulunur.

Bu öğretici, şirket içi Hyper-V sanal makineleri olağanüstü durum kurtarma Azure ayarlamak nasıl gösterir. Öğretici, System Center Virtual Machine Manager (VMM) tarafından yönetilmeyen Hyper-V VM'ler için geçerlidir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Çoğaltma kaynağı ve hedef seçin.
> * Şirket içi Site Recovery bileşenleri de dahil olmak üzere kaynak çoğaltma ortamı ve hedef çoğaltma ortamı ayarlayın.
> * Bir çoğaltma ilkesi oluşturun.
> * Bir sanal makine için çoğaltmayı etkinleştirin.

Bir dizi üçüncü öğreticide budur. Bu öğreticinin önceki eğitimlerine görevleri önceden tamamlamış varsayılır:

1. [Azure’u hazırlama](tutorial-prepare-azure.md)
2. [Şirket içi Hyper-V hazırlama](tutorial-prepare-on-premises-hyper-v.md)

Başlamadan önce için yararlı [mimarisi gözden](concepts-hyper-v-to-azure-architecture.md) bu olağanüstü durum kurtarma senaryosuna.

## <a name="select-a-replication-goal"></a>Çoğaltma hedefi seçin


1. İçinde **tüm hizmetleri** > **kurtarma Hizmetleri kasaları**, biz hazırlanmış önceki öğreticide kasasının adını tıklatın **ContosoVMVault**.
2. İçinde **Başlarken**, tıklatın **Site Recovery**. Ardından **altyapıyı hazırlama**
3. İçinde **koruma hedefi** > **bulunan makinelerinizi nerede**seçin **şirket içi**.
4. İçinde **istediğiniz makinelerinizi çoğaltmak**seçin **için Azure**.
5. İçinde **sanallaştırılmış, makine**seçin **Evet, Hyper-V ile**.
6. İçinde **System Center VMM kullanıyorsanız**seçin **Hayır**. Daha sonra, **Tamam**'a tıklayın.

    ![Çoğaltma hedefi](./media/tutorial-hyper-v-to-azure/replication-goal.png)

## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Kaynak ortamı ayarlamak için Hyper-V konakları için Hyper-V sitesi indirin ve Azure Site kurtarma Sağlayıcısı'nı ve Azure kurtarma Hizmetleri aracısını yükleyin ve Hyper-V sitesi kasaya kaydetmek ekleyin. 

1. İçinde **altyapıyı hazırlama**, tıklatın **kaynak**.
2. Tıklatın **+ Hyper-V sitesi**ve önceki öğreticide oluşturduğumuz site adını belirtin **ContosoHyperVSite**.
3. Tıklatın **+ Hyper-V Server**.
4. Sağlayıcı kurulum dosyasını karşıdan yükleyin.
5. Kasa kayıt anahtarını indir Sağlayıcı Kurulum çalıştırdığınızda bu gerekir. Anahtar, oluşturulduktan sonra beş gün boyunca geçerlidir.

    ![Sağlayıcı indirin](./media/tutorial-hyper-v-to-azure/download.png)
    

### <a name="install-the-provider"></a>Sağlayıcıyı yükleyin

Sağlayıcı kurulum dosyasını (AzureSiteRecoveryProvider.exe) her çalıştırın Hyper-V konağı eklediğiniz **ContosoHyperVSite** site. Kurulum Azure Site Recovery sağlayıcısı ve kurtarma Hizmetleri Aracısı, her Hyper-V ana bilgisayarına yükler.

1. Azure Site kurtarma Sağlayıcısı Kurulumu Sihirbazı'nda > **Microsoft Update**, sağlayıcı güncelleştirmeleri denetlemek için Microsoft Update'i kullanmayı kabul.
2. İçinde **yükleme**sağlayıcı ve aracı için varsayılan yükleme konumunu kabul edin ve tıklatın **yükleme**.
3. Yüklemeden sonra Microsoft Azure Site Kurtarma Kayıt Sihirbazı'ndaki > **kasa ayarlarını**, tıklatın **Gözat**hem de **anahtar dosyası**seçin kasa anahtarı dosyasını indirilen. 
4. Azure Site Recovery abonelik, kasa adını belirtin (**ContosoVMVault**) ve Hyper-V sitesi (**ContosoHyperVSite**) Hyper-V sunucusuna ait olduğu.
5. İçinde **Proxy ayarlarını**seçin **Azure Site Recovery Ara sunucu olmadan doğrudan bağlan**.
6. İçinde **kayıt**, sunucuyu kasaya kaydedildikten sonra tıklatın **son**.

Hyper-V sunucusundan meta veriler, Azure Site Recovery tarafından alınır ve sunucu görüntülenen **Site Recovery altyapısı** > **Hyper-V konakları**. Bu 30 dakika kadar sürebilir.


## <a name="set-up-the-target-environment"></a>Hedef ortamı ayarlama

Seçin ve hedef kaynaklarını doğrulayın. 

1. Tıklatın **altyapıyı hazırlama** > **hedef**.
2. Abonelik ve kaynak grubu seçin **ContosoRG**, yük devretme sonrasında Azure sanal makinelerini oluşturulacağı içinde.
3. Seçin **Resource Manager "** dağıtım modeli.

Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler.


## <a name="set-up-a-replication-policy"></a>Bir çoğaltma ilkesini ayarlayın

1. Tıklatın **altyapıyı hazırlama** > **çoğaltma ayarları** > **+ oluştur ve ilişkilendir**.
2. İçinde **ilke oluştur ve ilişkilendir**, ilke adını belirtebilir **ContosoReplicationPolicy**.
3. Varsayılan ayarları bırakın ve tıklayın **Tamam**.
    - **Kopyalama sıklığı** bu delta gösterir (sonra ilk çoğaltma) verileri her beş dakikada çoğaltmak.
    - **Kurtarma noktası bekletme** her kurtarma noktası bekletme windows iki olacağını gösterir iki saat.
    - **Uygulamayla tutarlı anlık görüntü sıklığı** uygulamayla tutarlı anlık görüntüleri içeren kurtarma noktaları her saat oluşturulacak gösterir.
    - **İlk çoğaltma başlangıç zamanı**, ilk çoğaltma hemen başlayacak gösterir.
4. İlkesi oluşturulduktan sonra tıklatın **Tamam**. Bu otomatik olarak ilişkilendirilen belirtilen Hyper-V sitesi ile yeni bir ilke oluşturduğunuzda (**ContosoHyperVSite**)

    ![Çoğaltma ilkesi](./media/tutorial-hyper-v-to-azure/replication-policy.png)


## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme


1. İçinde **uygulama çoğaltma**, tıklatın **kaynak**. 
2. İçinde **kaynak**seçin **ContosoHyperVSite** site. Daha sonra, **Tamam**'a tıklayın.
3. İçinde **hedef**, kasa abonelik hedefi olarak Azure doğrulayın ve **Resource Manager** dağıtım modeli.
4. Seçin **contosovmsacct1910171607** oluşturduğumuz önceki öğreticide çoğaltılan veriler için depolama hesabı ve **ContosoASRnet** ağında Azure Vm'lerinin kaydedileceği yük devretme sonrasında.
5. İçinde **sanal makineleri** > **seçin**, çoğaltmak istediğiniz VM seçin. Daha sonra, **Tamam**'a tıklayın.

 İlerleme durumunu izleyebilirsiniz **korumayı etkinleştir** eylemde **işleri** > **Site Recovery işleri**. Sonra **korumayı Sonlandır** işi tamamlandığında, ilk çoğaltma tamamlandıktan ve sanal makine yük devretme için hazırdır.

## <a name="next-steps"></a>Sonraki adımlar
[Olağanüstü durum kurtarma tatbikatı çalıştırma](tutorial-dr-drill-azure.md)
