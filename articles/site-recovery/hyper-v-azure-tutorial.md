---
title: Azure Site Recovery ile şirket içi Hyper-V sanal makineleri (VMM olmadan) için Azure’da olağanüstü durum kurtarma ayarlama | Microsoft Docs
description: Azure Site Recovery hizmeti ile şirket içi Hyper-V sanal makineleri (VMM olmadan) için Azure’da olağanüstü durum kurtarmanın nasıl ayarlanacağını öğrenin.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 03/18/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 2201a8017f82517f287cc0b73346a90eaa2408a4
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58877729"
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


1. **Tüm Hizmetler** > **Kurtarma Hizmetleri kasaları** bölümünde, önceki öğreticide hazırladığımız kasayı (**ContosoVMVault**) seçin.
2. **Başlarken** bölümünde **Site Recovery**’ye tıklayın. Daha sonra **Altyapıyı Hazırlama**’ya tıklayın
3. **Koruma hedefi** > **Makineleriniz nerede** bölümünde **Şirket içi** seçeneğini belirleyin.
4. **Makinelerinizi nereye çoğaltmak istiyorsunuz** bölümünde **Azure’a** seçeneğini belirleyin.
5. **Hyper-V ana bilgisayarlarınızı yönetmek için System Center VMM mi kullanıyorsunuz** başlığında **Hayır**’ı seçin. Daha sonra, **Tamam**'a tıklayın.

    ![Çoğaltma hedefi](./media/hyper-v-azure-tutorial/replication-goal.png)

## <a name="confirm-deployment-planning"></a>Dağıtım planlamasını onaylama

Büyük bir dağıtım planlarken, [Hyper-V çoğaltma için dağıtım planlaması](hyper-v-deployment-planner-overview.md) konusunu tamamladığınızdan emin olmalısınız. Bu öğretici için **Dağıtım planlamasını tamamladınız mı?** seçeneğinde açılan listede **Daha sonra yapacağım**’ı seçin.

![Dağıtım planlaması](./media/hyper-v-azure-tutorial/deployment-planning.png)

## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Kaynak ortamını ayarlamak için bir Hyper-V sitesi oluşturun ve siteye Hyper-V ana bilgisayarları ekleyin. Ardından her ana bilgisayarda Azure Site Recovery Sağlayıcısı ile Azure Kurtarma Hizmetleri aracısını indirip yükleyin ve Hyper-V sitesini kasaya kaydedin. 

1. **Altyapıyı Hazırlama** bölümünde **Kaynak** seçeneğine tıklayın.
2. **+Hyper-V Sitesi**’ne tıklayın ve önceki öğreticide oluşturulan sitenin adını (**ContosoHyperVSite**) belirtin.

    ![Hyper-V sitesi](./media/hyper-v-azure-tutorial/hyperv-site.png)

3. Site oluşturulduktan sonra **+Hyper-V Server**’a tıklayın.

    ![Hyper-V sunucusu](./media/hyper-v-azure-tutorial/hyperv-server.png)

4. Sağlayıcı kurulum dosyasını indirin.
6. Kasa kayıt anahtarını indirin. Sağlayıcı kurulumunu çalıştırırken bu anahtara ihtiyaç duyarsınız. Anahtar, oluşturulduktan sonra beş gün boyunca geçerlidir.

    ![Sağlayıcıyı indirin](./media/hyper-v-azure-tutorial/download.png)
    

### <a name="install-the-provider"></a>Sağlayıcıyı yükleyin

**ContosoHyperVSite** sitesine eklediğiniz her bir Hyper-V konağında Sağlayıcı kurulum dosyasını (AzureSiteRecoveryProvider.exe) çalıştırın. Kurulum işlemi, Azure Site Kurtarma Sağlayıcısı’nı ve Kurtarma Hizmetleri aracısını her bir Hyper-V konağına yükler.

1. Azure Site Kurtarma Sağlayıcısı Kurulum sihirbazındaki **Microsoft Update** bölümünde, Sağlayıcı güncelleştirmelerini denetlemek için Microsoft Update’i kullanmayı kabul edin.
2. **Yükleme** bölümünde, Sağlayıcı ve aracı için varsayılan yükleme konumunu kabul edin ve **Yükle**'ye tıklayın.
3. Yüklemeden sonra Microsoft Azure Site Kurtarma Kayıt Sihirbazı’ndaki **Kasa Ayarları**’nda **Gözat**’a tıklayın ve **Anahtar Dosyası** bölümünde indirdiğiniz kasa anahtarı dosyasını seçin. 
4. Azure Site Recovery aboneliğini, kasa adını (**ContosoVMVault**) ve Hyper-V sunucusunun ait olduğu Hyper-V sitesini (**ContosoHyperVSite**) belirtin.
5. **Proxy Ayarları** bölümünde **Proxy sunucusu olmadan doğrudan Azure Site Recovery hizmetine bağlan** seçeneğini belirleyin.
6. **Kayıt** bölümünde, Sunucu kasaya kaydedildikten sonra **Son**’a tıklayın.

Hyper-V sunucusundaki meta veriler, Azure Site Recovery tarafından alınır ve **Site Recovery Altyapısı** > **Hyper-V Konakları** bölümünde sunucu görüntülenir. Bu işlemin tamamlanması 30 dakika sürebilir.        

Bir Hyper-V çekirdeği sunucusu kullandığınız durumda izleyin aşağıda belirtildiği gibi sağlayıcısını ve kasa kimlik bilgilerini indirme sonra adımları [burada](#set-up-the-source-environment)

1. Çalıştırarak AzureSiteRecoveryProvider.exe dosyaları ayıklayın

    `AzureSiteRecoveryProvider.exe /x:. /q`
 
    Bu yerel dizin dosyalarını ayıklar.
 
2.  Çalıştırın `.\setupdr.exe /i`

    Sonuçları %Programdata%\ASRLogs\DRASetupWizard.log için günlüğe kaydedilir

3.  Komutunu kullanarak sunucuyu kaydedin:

`cd  C:\Program Files\Microsoft Azure Site Recovery Provider\DRConfigurator.exe" /r /Friendlyname "FriendlyName of the Server" /Credentials "path to where the credential file is saved"`
 

## <a name="set-up-the-target-environment"></a>Hedef ortamı ayarlama

Hedef kaynaklarını seçin ve doğrulayın. 

1. **Altyapıyı Hazırlama** > **Hedef** seçeneklerine tıklayın.
2. Yük devretmenin ardından içinde Azure sanal makinelerinin oluşturulacağı **ContosoRG** kaynak grubunu ve aboneliği seçin.
3. **Kaynak Yöneticisi** dağıtım modelini seçin.

Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler.


## <a name="set-up-a-replication-policy"></a>Çoğaltma ilkesi ayarlama

> [!NOTE]
> Hyper-V’den Azure’a çoğaltma ilkeleri için, 15 dakikalık kopyalama sıklığı seçeneği, 5 dakikalık ve 30 saniyelik kopyalama sıklığı ayarları dolayısıyla devre dışı bırakılıyor. 15 dakikalık kopyalama sıklığı kullanan çoğaltma ilkeleri, 5 dakikalık kopyalama sıklığı ayarını kullanmak üzere otomatik olarak güncelleştirilecektir. 15 dakikalık kopyalama sıklığıyla karşılaştırıldığında, 5 dakikalık ve 30 saniyelik kopyalama sıklığı seçenekleri bant genişliği kullanımını ve veri aktarım hacmini en düşük düzeyde etkilemenin yanı sıra, iyileştirilmiş çoğaltma performansı ve daha iyi geri yükleme noktası hedefleri sağlar.

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
4. Çoğaltılan veriler için önceki öğreticide oluşturulan **contosovmsacct1910171607** depolama hesabını ve yük devretmenin ardından Azure sanal makinelerinin konumlandırılacağı **ContosoASRnet** ağını seçin.
5. **Sanal makineler** > **Seç** bölümünde, çoğaltmak istediğiniz sanal makineyi seçin. Daha sonra, **Tamam**'a tıklayın.

   **İşler** > **Site Recovery işleri** bölümünde **Korumayı Etkinleştir** eyleminin ilerleme durumunu izleyebilirsiniz. **Korumayı Sonlandır** işi tamamlandıktan sonra ilk çoğaltma tamamlanır ve sanal makine yük devretme için hazır olur.

## <a name="next-steps"></a>Sonraki adımlar
[Olağanüstü durum kurtarma tatbikatı çalıştırma](tutorial-dr-drill-azure.md)
