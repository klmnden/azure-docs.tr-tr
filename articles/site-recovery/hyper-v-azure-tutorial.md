---
title: "Azure Site Recovery ile şirket içi Hyper-V sanal makineleri (VMM olmadan) için Azure’da olağanüstü durum kurtarma ayarlama | Microsoft Docs"
description: "Azure Site Recovery hizmeti ile şirket içi Hyper-V sanal makineleri (VMM olmadan) için Azure’da olağanüstü durum kurtarmanın nasıl ayarlanacağını öğrenin."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 02/14/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: b946964c162f47a283c37c6eae7e7152e27b6033
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2018
---
# <a name="set-up-disaster-recovery-of-on-premises-hyper-v-vms-to-azure"></a>Şirket içi Hyper-V sanal makineleri için Azure’da olağanüstü durum kurtarma ayarlama

[Azure Site Recovery](site-recovery-overview.md) hizmeti, şirket içi makinelerin ve Azure sanal makinelerinin (VM) çoğaltma, yük devretme ve yeniden çalışma işlemlerini yöneterek ve düzenleyerek olağanüstü durum kurtarma stratejinize katkı sağlar.

Bu öğreticide, şirket içi Hyper-V sanal makineleri için Azure’da olağanüstü durum kurtarmanın nasıl ayarlanacağı gösterilmektedir. Bu öğretici, System Center Virtual Machine Manager (VMM) tarafından yönetilmeyen Hyper-V sanal makineleri için geçerlidir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Çoğaltma kaynağınızı ve hedefinizi seçme.
> * Kaynak çoğaltma ortamını, şirket içi Site Recovery bileşenleri ve hedef çoğaltma ortamı dahil olmak üzere ayarlayın.
> * Bir çoğaltma ilkesi oluşturma.
> * Sanal makine için çoğaltmayı etkinleştirme.

Bu, serideki üçüncü öğreticidir. Bu öğreticide, önceki öğreticilerdeki adımları zaten tamamladığınız varsayılır:

1. [Azure’u hazırlama](tutorial-prepare-azure.md)
2. [Şirket içi Hyper-V’leri hazırlama](tutorial-prepare-on-premises-hyper-v.md)

Başlamadan önce bu olağanüstü durum kurtarma senaryosu için [mimariyi gözden geçirmeniz](concepts-hyper-v-to-azure-architecture.md) yararlı olabilir.

## <a name="select-a-replication-goal"></a>Çoğaltma hedefi seçme


1. **Tüm Hizmetler** > **Kurtarma Hizmetleri kasaları** bölümünde, önceki öğreticide hazırladığımız kasanın adına (**ContosoVMVault**) tıklayın.
2. **Başlarken** bölümünde **Site Recovery**’ye tıklayın. Daha sonra **Altyapıyı Hazırlama**’ya tıklayın
3. **Koruma hedefi** > **Makineleriniz nerede** bölümünde **Şirket içi** seçeneğini belirleyin.
4. **Makinelerinizi nereye çoğaltmak istiyorsunuz** bölümünde **Azure’a** seçeneğini belirleyin.
5. **Makineleriniz sanallaştırılmış mı?** bölümünde **Hayır**’ı seçin. Daha sonra, **Tamam**'a tıklayın.

    ![Çoğaltma hedefi](./media/hyper-v-azure-tutorial/replication-goal.png)

## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Kaynak ortamı ayarlamak için Hyper-V konaklarını bir Hyper-V sitesine ekleyin, Azure Site Kurtarma Sağlayıcısı'nı ve Azure Kurtarma Hizmetleri aracısını yükleyin ve Hyper-V sitesini kasaya kaydedin. 

1. **Altyapıyı Hazırlama** bölümünde **Kaynak** seçeneğine tıklayın.
2. **+Hyper-V Sitesi**’ne tıklayın ve önceki öğreticide oluşturduğumuz sitenin adını (**ContosoHyperVSite**) belirtin.
3. **+Hyper-V Sunucusu**’na tıklayın.
4. Sağlayıcı kurulum dosyasını indirin.
5. Kasa kayıt anahtarını indirin. Sağlayıcı kurulumunu çalıştırırken buna ihtiyacınız olur. Anahtar, oluşturulduktan sonra beş gün boyunca geçerlidir.

    ![Sağlayıcıyı indirin](./media/hyper-v-azure-tutorial/download.png)
    

### <a name="install-the-provider"></a>Sağlayıcıyı yükleyin

**ContosoHyperVSite** sitesine eklediğiniz her bir Hyper-V konağında Sağlayıcı kurulum dosyasını (AzureSiteRecoveryProvider.exe) çalıştırın. Kurulum işlemi, Azure Site Kurtarma Sağlayıcısı’nı ve Kurtarma Hizmetleri aracısını her bir Hyper-V konağına yükler.

1. Azure Site Kurtarma Sağlayıcısı Kurulum sihirbazındaki **Microsoft Update** bölümünde, Sağlayıcı güncelleştirmelerini denetlemek için Microsoft Update’i kullanmayı kabul edin.
2. **Yükleme** bölümünde, Sağlayıcı ve aracı için varsayılan yükleme konumunu kabul edin ve **Yükle**'ye tıklayın.
3. Yüklemeden sonra Microsoft Azure Site Kurtarma Kayıt Sihirbazı’ndaki **Kasa Ayarları**’nda **Gözat**’a tıklayın ve **Anahtar Dosyası** bölümünde indirdiğiniz kasa anahtarı dosyasını seçin. 
4. Azure Site Recovery aboneliğini, kasa adını (**ContosoVMVault**) ve Hyper-V sunucusunun ait olduğu Hyper-V sitesini (**ContosoHyperVSite**) belirtin.
5. **Proxy Ayarları** bölümünde **Proxy sunucusu olmadan doğrudan Azure Site Recovery hizmetine bağlan** seçeneğini belirleyin.
6. **Kayıt** bölümünde, Sunucu kasaya kaydedildikten sonra **Son**’a tıklayın.

Hyper-V sunucusundaki meta veriler, Azure Site Recovery tarafından alınır ve **Site Recovery Altyapısı** > **Hyper-V Konakları** bölümünde sunucu görüntülenir. Bu işlem 30 dakika sürebilir.


## <a name="set-up-the-target-environment"></a>Hedef ortamı ayarlama

Hedef kaynaklarını seçin ve doğrulayın. 

1. **Altyapıyı Hazırlama** > **Hedef** seçeneklerine tıklayın.
2. Yük devretmenin ardından içinde Azure sanal makinelerinin oluşturulacağı **ContosoRG** kaynak grubunu ve aboneliği seçin.
3. **Kaynak Yöneticisi** dağıtım modelini seçin.

Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler.


## <a name="set-up-a-replication-policy"></a>Çoğaltma ilkesi ayarlama

1. **Altyapıyı hazırlama** > **Çoğaltma Ayarları** > **+Oluştur ve ilişkilendir** seçeneklerine tıklayın.
2. **İlke oluştur ve ilişkilendir** bölümünde bir ilke adı (**ContosoReplicationPolicy**) belirtin.
3. Varsayılan ayarları değiştirmeden **Tamam**'a tıklayın.
    - **Kopyalama sıklığı**, değişim verilerinin (ilk çoğaltmadan sonra) her beş dakikada bir çoğaltılacağını belirtir.
    - **Kurtarma noktası bekletme**, her kurtarma noktası için bekletme pencerelerinin iki saat olacağını belirtir.
    - **Uygulamayla tutarlı anlık görüntü sıklığı**, uygulamayla tutarlı anlık görüntüleri içeren kurtarma noktalarının her saat oluşturulacağını belirtir.
    - **İlk çoğaltma başlangıç zamanı**, ilk çoğaltmanın hemen başlatılacağını belirtir.
4. İlke oluşturulduktan sonra **Tamam**’a tıklayın. Yeni bir ilke oluşturduğunuzda bu ilke otomatik olarak belirtilen Hyper-V sitesi (**ContosoHyperVSite**) ile ilişkilendirilir

    ![Çoğaltma ilkesi](./media/hyper-v-azure-tutorial/replication-policy.png)


## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme


1. **Uygulama çoğaltma** bölümünde **Kaynak** seçeneğine tıklayın. 
2. **Kaynak** bölümünde **ContosoHyperVSite** sitesini seçin. Daha sonra, **Tamam**'a tıklayın.
3. **Hedef** bölümünde, **Kaynak Yöneticisi** dağıtım modelini, kasa aboneliğini ve hedef olarak Azure’ı doğrulayın.
4. Çoğaltılan veriler için önceki öğreticide oluşturduğumuz **contosovmsacct1910171607** depolama hesabını ve yük devretmenin ardından Azure sanal makinelerinin konumlandırılacağı **ContosoASRnet** ağını seçin.
5. **Sanal makineler** > **Seç** bölümünde, çoğaltmak istediğiniz sanal makineyi seçin. Daha sonra, **Tamam**'a tıklayın.

 **İşler** > **Site Recovery işleri** bölümünde **Korumayı Etkinleştir** eyleminin ilerleme durumunu izleyebilirsiniz. **Korumayı Sonlandır** işi tamamlandıktan sonra ilk çoğaltma tamamlanır ve sanal makine yük devretme için hazır olur.

## <a name="next-steps"></a>Sonraki adımlar
[Olağanüstü durum kurtarma tatbikatı çalıştırma](tutorial-dr-drill-azure.md)
