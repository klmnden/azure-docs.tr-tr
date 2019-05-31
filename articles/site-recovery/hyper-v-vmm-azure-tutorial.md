---
title: Site Recovery ile azure'a VMM bulutlarındaki şirket içi Hyper-V Vm'leri için olağanüstü durum kurtarma ayarlama | Microsoft Docs
description: Site RECOVERY'yi kullanarak System Center VMM bulutlarında şirket içi Hyper-V sanal makineleri olağanüstü durum kurtarma ayarlama konusunda bilgi edinin.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 653db1497fcce5981bba7416f073b0330ca2861f
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66398148"
---
# <a name="set-up-disaster-recovery-of-on-premises-hyper-v-vms-in-vmm-clouds-to-azure"></a>Azure'a VMM bulutlarındaki şirket içi Hyper-V sanal makineleri olağanüstü durum kurtarmayı ayarlayın

Bu makalede kullanarak azure'a olağanüstü durum kurtarma için System Center Virtual Machine Manager (VMM) tarafından yönetilen şirket içi Hyper-V Vm'leri için çoğaltmayı etkinleştirme [Azure Site Recovery](site-recovery-overview.md) hizmeti. VMM, kullanmıyorsanız [bu öğreticiden yararlanın](hyper-v-azure-tutorial.md) yerine.

Bu, şirket içi VMware Vm'leri için Azure'da olağanüstü durum kurtarma ayarlama gösteren serideki üçüncü öğreticidir. Önceki öğreticide, biz [şirket içi Hyper-V ortamına hazır](hyper-v-prepare-on-premises-tutorial.md) azure'a olağanüstü durum kurtarma için.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Çoğaltma kaynağınızı ve hedefinizi seçme.
> * Şirket içi Site Recovery bileşenleri ve hedef çoğaltma ortamı dahil olmak üzere kaynak çoğaltma ortamını, ayarlayın.
> * VMM VM ağları ve Azure sanal ağları arasında eşleme için Ağ eşlemesi ayarlayın.
> * Bir çoğaltma ilkesi oluşturma.
> * Sanal makine için çoğaltmayı etkinleştirme.

> [!NOTE]
> Öğreticiler bir senaryo için en basit dağıtım yolu gösterir. Mümkün olduğunca varsayılan seçenekleri kullanır ve tüm olası ayarları ve yolları göstermez. Ayrıntılı yönergeler için makaleleri inceleyin **nasıl yapılır kılavuzlarından** bölümünü [Site Recovery belgeleri](https://docs.microsoft.com/azure/site-recovery).

## <a name="before-you-begin"></a>Başlamadan önce

Bu, serideki üçüncü öğreticidir. Bu önceki öğreticilerdeki görevleri zaten tamamladığınız varsayılır:

1. [Azure’u hazırlama](tutorial-prepare-azure.md)
2. [Şirket içi Hyper-V’leri hazırlama](tutorial-prepare-on-premises-hyper-v.md)

## <a name="select-a-replication-goal"></a>Çoğaltma hedefi seçme

1. Azure portalında Git **kurtarma Hizmetleri kasaları** ve kasayı seçin. Hazırladığımız kasayı **ContosoVMVault** önceki öğreticide.
2. İçinde **Başlarken**seçin **Site Recovery**ve ardından **altyapıyı hazırlama**.
3. İçinde **koruma hedefi** > **makineleriniz nerede?** seçin **şirket içi**.
4. İçinde **nerede makinelerinizi çoğaltmak istiyorsunuz?** seçin **Azure'a**.
5. İçinde **makineleriniz sanallaştırıldı mı?** seçin **Evet, Hyper-V ile**.
6. İçinde **System Center VMM, Hyper-V ana bilgisayarları yönetmek için kullandığınız?** seçin **Evet**.
7.  **Tamam**’ı seçin.

    ![Çoğaltma hedefi](./media/hyper-v-vmm-azure-tutorial/replication-goal.png)

## <a name="confirm-deployment-planning"></a>Dağıtım planlamasını onaylama

1. İçinde **dağıtım planlaması**, büyük bir dağıtımın planlıyorsanız, Hyper-V için dağıtım Planlayıcısı sayfasındaki bağlantısını indirin. [Daha fazla bilgi edinin](hyper-v-deployment-planner-overview.md) Hyper-V dağıtım planlama hakkında daha fazla.
2. Bu öğreticide, dağıtım Planlayıcısını gerekmez. İçinde **dağıtım planlamasını tamamladınız mı?** seçin **daha sonra yapacağım**ve ardından **Tamam**.

## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Kaynak ortamı ayarlama, Azure Site Recovery sağlayıcısını VMM sunucusuna yükleyin ve sunucuyu kasaya kaydedin. Her Hyper-V konağında'de Azure kurtarma Hizmetleri aracısını yükleyin.

1. İçinde **altyapıyı hazırlama**seçin **kaynak**.
2. İçinde **kaynağı hazırla**seçin **+ VMM** VMM sunucusu eklemek için. **Sunucu Ekle** bölümünde **Sunucu türü** olarak **System Center VMM sunucusu** girişinin olduğundan emin olun.
3. Microsoft Azure Site Recovery sağlayıcısı yükleyicisini indirin.
4. Kasa kayıt anahtarını indir Sağlayıcı kurulumunu çalıştırırken bu anahtar gerekir. Anahtar, oluşturulduktan sonra beş gün boyunca geçerlidir.
5. Microsoft Azure kurtarma Hizmetleri aracısı için yükleyiciyi indirin.

    ![Aracı sağlayıcısı ve kayıt anahtarını indirin](./media/hyper-v-vmm-azure-tutorial/download-vmm.png)

### <a name="install-the-provider-on-the-vmm-server"></a>Sağlayıcıyı VMM sunucusuna yükleme

1. Azure Site Kurtarma Sağlayıcısı Kurulum sihirbazındaki **Microsoft Update** bölümünde, Sağlayıcı güncelleştirmelerini denetlemek için Microsoft Update’i kullanmayı kabul edin.
2. İçinde **yükleme**, sağlayıcı için varsayılan yükleme konumunu kabul edin ve seçin **yükleme**.
3. Yüklemeden sonra Microsoft Azure Site Kurtarma Kayıt Sihirbazı'nda > **kasa ayarları**seçin **Gözat**hem de **anahtar dosyası**seçin kasa anahtarını dosyası indirdiğiniz.
4. Azure Site Recovery aboneliğini ve kasa adını belirtin (**ContosoVMVault**). VMM sunucusunu kasada tanımlamak için için bir kolay ad belirtin.
5. **Proxy Ayarları** bölümünde **Proxy sunucusu olmadan doğrudan Azure Site Recovery hizmetine bağlan** seçeneğini belirleyin.
6. Verileri şifrelemek için kullanılan sertifika için varsayılan konumu kabul edin. Yük devretme sırasında şifrelenmiş verilerin şifresi çözülür.
7. İçinde **bulut meta verilerini eşitle**seçin **eşitleme bulut meta verilerini Site Recovery portalında**. Bu eylem, her sunucuda yalnızca bir kere gerçekleşmesi gerekir. Ardından, **kaydetme**.
8. Sunucu kasaya kaydedildikten sonra seçin **son**.

Kayıt tamamlandıktan sonra sunucusundan meta veriler, Azure Site Recovery tarafından alınır ve VMM sunucusu görüntülenen **Site Recovery altyapısı**.

### <a name="install-the-recovery-services-agent-on-hyper-v-hosts"></a>Kurtarma Hizmetleri aracısını Hyper-V ana bilgisayarlarına yükleme

Çoğaltmak istediğiniz Vm'leri içeren her bir Hyper-V konağında aracıyı yükleyin.

1. Microsoft Azure kurtarma Hizmetleri Aracısı Kurulum Sihirbazı'ndaki > **Önkoşul denetimi**seçin **sonraki**. Eksik Önkoşullar otomatik olarak yüklenir.
2. İçinde **yükleme ayarları**, yükleme konumu ve önbellek konumunu kabul edin. Önbellek sürücüsü en az 5 GB depolama alanı gerekir. 600 GB veya daha fazla boş alana sahip bir sürücü öneririz. Ardından **Yükle**’yi seçin.
3. İçinde **yükleme**yükleme tamamlandığında, seçin **Kapat** Sihirbazı tamamlamak için.

    ![Aracıyı yükleme](./media/hyper-v-vmm-azure-tutorial/mars-install.png)

## <a name="set-up-the-target-environment"></a>Hedef ortamı ayarlama

1. **Altyapıyı hazırla** > **Hedef** seçeneğini belirleyin.
2. Abonelik ve kaynak grubunu seçin (**ContosoRG**) de, yük devretme sonrasında Azure Vm'leri oluşturulur.
3. Seçin **Resource Manager** dağıtım modeli.

Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler.

## <a name="configure-network-mapping"></a>Ağ eşlemesini yapılandırma

1. İçinde **Site Recovery altyapısı** > **ağ eşlemeleri** > **ağ eşlemesi**seçin **+ ağ eşlemesi** simgesi.
2. İçinde **ağ eşlemesi Ekle**, kaynak VMM sunucusunu seçin. Seçin **Azure** hedefi olarak.
3. Yük devretme işleminden sonra dağıtım modelini ve aboneliği seçin.
4. İçinde **kaynak ağ**, kaynak şirket içi VM ağını seçin.
5. İçinde **hedef ağ**, çoğaltılan Azure Vm'lerinin Azure ağını konumlandırılacağı yük devretme sonrasında oluşturulduğunda seçin. Sonra **Tamam**’ı seçin.

    ![Ağ eşlemesi](./media/hyper-v-vmm-azure-tutorial/network-mapping-vmm.png)

## <a name="set-up-a-replication-policy"></a>Çoğaltma ilkesi ayarlama

1. Seçin **altyapıyı hazırlama** > **çoğaltma ayarları** >  **+ oluştur ve ilişkilendir**.
2. **İlke oluştur ve ilişkilendir** bölümünde bir ilke adı belirtin. Kullandığımız **ContosoReplicationPolicy**.
3. Varsayılan ayarları bırakın ve seçin **Tamam**.
    - **Kopyalama sıklığı** ilk çoğaltmadan sonra değişim verilerini beş dakikada çoğaltmak, gösterir.
    - **Kurtarma noktası bekletme** her kurtarma noktası için iki saat tutulacağını belirtir.
    - **Uygulamayla tutarlı anlık görüntü sıklığı**, uygulamayla tutarlı anlık görüntüleri içeren kurtarma noktalarının her saat oluşturulacağını belirtir.
    - **İlk çoğaltma başlangıç zamanı** ilk çoğaltmanın hemen başlatılacağını belirtir.
    - **Azure'da depolanan verileri şifrele** Varsayılana Ayarla (**kapalı**) ve azure'da bekleyen verilerin şifrelenip değil belirtir.
4. İlke oluşturulduktan sonra seçin **Tamam**. Yeni bir ilke oluşturduğunuzda, otomatik olarak VMM bulutuyla ilişkilendirilir.

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

1. İçinde **uygulama çoğaltma**seçin **kaynak**.
2. İçinde **kaynak**, VMM Bulutu seçin. Sonra **Tamam**’ı seçin.
3. İçinde **hedef**seçin ve hedef (Azure) Kasa aboneliğini doğrulamak **Resource Manager** modeli.
4. Seçin **contosovmsacct1910171607** depolama hesabı ve **ContosoASRnet** Azure ağı.
5. İçinde **sanal makineler** > **seçin**, çoğaltmak istediğiniz VM'yi seçin. Sonra **Tamam**’ı seçin.

   **İşler** > **Site Recovery işleri** bölümünde **Korumayı Etkinleştir** eyleminin ilerleme durumunu izleyebilirsiniz. Sonra **korumayı Sonlandır** iş tamamlanana, ilk çoğaltma tamamlandıktan ve sanal makine yük devretme için hazırdır.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Olağanüstü durum kurtarma tatbikatı çalıştırma](tutorial-dr-drill-azure.md)
