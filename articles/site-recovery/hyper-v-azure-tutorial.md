---
title: Azure Site Recovery ile şirket içi Hyper-V Vm'lerini (VMM olmadan) için olağanüstü durum kurtarma ayarlama | Microsoft Docs
description: Site Recovery kullanarak azure'a şirket içi Hyper-V Vm'lerini (VMM olmadan), olağanüstü durum kurtarma ayarlamayı öğrenin.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 05/30/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 9d45e0f0c759685f9d35285ee7718585d5961333
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66399403"
---
# <a name="set-up-disaster-recovery-of-on-premises-hyper-v-vms-to-azure"></a>Şirket içi Hyper-V sanal makineleri için Azure’da olağanüstü durum kurtarma ayarlama

[Azure Site Recovery](site-recovery-overview.md) Hizmet Yönetimi ve çoğaltma, yük devretme ve şirket içi makinelerin ve Azure sanal makineleri (VM) yeniden çalışma işlemlerini olağanüstü durum kurtarma stratejinize katkıda bulunur.

Bu, serideki üçüncü öğreticidir. Bu, şirket içi Hyper-V vm'lerini Azure'da olağanüstü durum kurtarma ayarlama işlemini göstermektedir. Bu öğretici, Microsoft System Center Virtual Machine Manager (VMM) tarafından yönetilmeyen Hyper-V Vm'lerini geçerlidir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Çoğaltma kaynağınızı ve hedefinizi seçme.
> * Şirket içi Site Recovery bileşenleri ve hedef çoğaltma ortamı dahil olmak üzere kaynak çoğaltma ortamını, ayarlayın.
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
6. İçinde **System Center VMM, Hyper-V ana bilgisayarları yönetmek için kullandığınız?** seçin **Hayır**.
7. **Tamam**’ı seçin.

    ![Çoğaltma hedefi](./media/hyper-v-azure-tutorial/replication-goal.png)

## <a name="confirm-deployment-planning"></a>Dağıtım planlamasını onaylama

1. İçinde **dağıtım planlaması**, büyük bir dağıtımın planlıyorsanız, Hyper-V için dağıtım Planlayıcısı sayfasındaki bağlantısını indirin. [Daha fazla bilgi edinin](hyper-v-deployment-planner-overview.md) Hyper-V dağıtım planlama hakkında daha fazla.
2. Bu öğreticide, dağıtım Planlayıcısını gerekmez. İçinde **dağıtım planlamasını tamamladınız mı?** seçin **daha sonra yapacağım**ve ardından **Tamam**.

    ![Dağıtım planlaması](./media/hyper-v-azure-tutorial/deployment-planning.png)

## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Kaynak ortamı ayarlamak için bir Hyper-V sitesi oluşturun ve o siteye çoğaltmak istediğiniz Vm'leri içeren Hyper-V konakları ekleyin. İndirin ve her konakta Azure Site Recovery sağlayıcısı ve Azure kurtarma Hizmetleri Aracısı'nı yükledikten sonra Hyper-V sitesini kasaya kaydedin.

1. Altında **altyapıyı hazırlama**seçin **kaynak**.
2. İçinde **kaynağı hazırla**seçin **+ Hyper-V sitesi**.
3. İçinde **oluşturma Hyper-V sitesi**, site adı belirtin. Kullandığımız **ContosoHyperVSite**.

    ![Hyper-V sitesi](./media/hyper-v-azure-tutorial/hyperv-site.png)

4. Sitenin, oluşturulduktan sonra **kaynağı hazırla** > **1. adım: Hyper-V site Seç**, oluşturduğunuz sitesini seçin.
5. Seçin **+ Hyper-V Server**.

    ![Hyper-V sunucusu](./media/hyper-v-azure-tutorial/hyperv-server.png)

6. Microsoft Azure Site Recovery sağlayıcısı yükleyicisini indirin.
7. Kasa kayıt anahtarını indir Sağlayıcı yüklemek için bu anahtarı gerekir. Anahtar, oluşturulduktan sonra beş gün boyunca geçerlidir.

    ![Sağlayıcı ve kayıt anahtarını indirin](./media/hyper-v-azure-tutorial/download.png)
    

### <a name="install-the-provider"></a>Sağlayıcıyı yükleyin

İndirilen kurulum dosyasını (AzureSiteRecoveryProvider.exe) Hyper-V sitesine eklemek istediğiniz her Hyper-V konağına yükleyin. Kurulum Azure Site Recovery sağlayıcısı ve kurtarma Hizmetleri aracısını her Hyper-V ana bilgisayarına yükler.

1. Kurulum dosyasını çalıştırın.
2. Azure Site Kurtarma Sağlayıcısı Kurulum sihirbazındaki **Microsoft Update** bölümünde, Sağlayıcı güncelleştirmelerini denetlemek için Microsoft Update’i kullanmayı kabul edin.
3. İçinde **yükleme**, sağlayıcı ve aracı için varsayılan yükleme konumunu kabul edin ve seçin **yükleme**.
4. Yüklemeden sonra Microsoft Azure Site Kurtarma Kayıt Sihirbazı'nda > **kasa ayarları**seçin **Gözat**hem de **anahtar dosyası**seçin kasa anahtarını dosyası indirdiğiniz.
5. Azure Site Recovery aboneliğini, kasa adını (**ContosoVMVault**) ve Hyper-V sunucusunun ait olduğu Hyper-V sitesini (**ContosoHyperVSite**) belirtin.
6. **Proxy Ayarları** bölümünde **Proxy sunucusu olmadan doğrudan Azure Site Recovery hizmetine bağlan** seçeneğini belirleyin.
7. İçinde **kayıt**, sunucu kasaya, select kaydedildikten sonra **son**.

Hyper-V sunucusundaki meta veriler, Azure Site Recovery tarafından alınır ve **Site Recovery Altyapısı** > **Hyper-V Konakları** bölümünde sunucu görüntülenir. Bu işlemin tamamlanması 30 dakika sürebilir.

#### <a name="install-the-provider-on-a-hyper-v-core-server"></a>Bir Hyper-V çekirdeği sunucusuna sağlayıcıyı yükleyin

Bir Hyper-V çekirdeği sunucusu çalıştırıyorsanız, kurulum dosyasını indirirsiniz ve aşağıdaki adımları izleyin:

1. Dosyaları, bu komutu çalıştırarak AzureSiteRecoveryProvider.exe bir yerel dizine ayıklayın:

    `AzureSiteRecoveryProvider.exe /x:. /q`
 
2. `.\setupdr.exe /i` öğesini çalıştırın. Sonuçları % Programdata%\ASRLogs\DRASetupWizard.log günlüğe kaydedilir.

3. Bu komutu çalıştırarak sunucuyu kaydedin:

    ```
    cd  C:\Program Files\Microsoft Azure Site Recovery Provider\DRConfigurator.exe" /r /Friendlyname "FriendlyName of the Server" /Credentials "path to where the credential file is saved"
    ```

## <a name="set-up-the-target-environment"></a>Hedef ortamı ayarlama

Seçin ve hedef kaynaklarını doğrulama:

1. **Altyapıyı hazırla** > **Hedef** seçeneğini belirleyin.
2. Abonelik ve kaynak grubunu seçin **ContosoRG** de hangi yük devretme sonrasında Azure Vm'leri oluşturulur.
3. **Kaynak Yöneticisi** dağıtım modelini seçin.

Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler.

## <a name="set-up-a-replication-policy"></a>Çoğaltma ilkesi ayarlama

1. Seçin **altyapıyı hazırlama** > **çoğaltma ayarları** >  **+ oluştur ve ilişkilendir**.
2. **İlke oluştur ve ilişkilendir** bölümünde bir ilke adı belirtin. Kullandığımız **ContosoReplicationPolicy**.
3. Bu öğretici için varsayılan ayarları bırakacağız:
    - **Kopyalama sıklığı** ne sıklıkta gösterir (ilk çoğaltmadan sonra) değişim verileri çoğaltılacağını. Varsayılan sıklık her beş dakikadır.
    - **Kurtarma noktası bekletme** kurtarma noktaları için iki saat tutulacağını belirtir.
    - **Uygulamayla tutarlı anlık görüntü sıklığı**, uygulamayla tutarlı anlık görüntüleri içeren kurtarma noktalarının her saat oluşturulacağını belirtir.
    - **İlk çoğaltma başlangıç zamanı** ilk çoğaltmanın hemen başlatılacağını belirtir.
4. İlke oluşturulduktan sonra seçin **Tamam**. Yeni bir ilke oluşturduğunuzda, otomatik olarak belirtilen Hyper-V sitesiyle ilişkili. Müşterilerimize öğreticide o **ContosoHyperVSite**.

    ![Çoğaltma ilkesi](./media/hyper-v-azure-tutorial/replication-policy.png)

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

1. İçinde **uygulama çoğaltma**seçin **kaynak**.
2. **Kaynak** bölümünde **ContosoHyperVSite** sitesini seçin. Sonra **Tamam**’ı seçin.
3. İçinde **hedef**, hedef (Azure) kasası aboneliği doğrulayın ve **Resource Manager** dağıtım modeli.
4. Öğretici ayarları kullanıyorsanız seçin **contosovmsacct1910171607** çoğaltılan veriler için önceki öğreticide oluşturulan depolama hesabı. Ayrıca seçin **ContosoASRnet** Azure sanal ağ konumlandırılacağı yük devretmeden sonra.
5. İçinde **sanal makineler** > **seçin**, çoğaltmak istediğiniz VM'yi seçin. Sonra **Tamam**’ı seçin.

   **İşler** > **Site Recovery işleri** bölümünde **Korumayı Etkinleştir** eyleminin ilerleme durumunu izleyebilirsiniz. Sonra **korumayı Sonlandır** iş tamamlanana, ilk çoğaltma tamamlandıktan ve sanal makine yük devretme için hazırdır.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Olağanüstü durum kurtarma tatbikatı çalıştırma](tutorial-dr-drill-azure.md)
