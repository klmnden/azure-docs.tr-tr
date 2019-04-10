---
title: Azure Site Recovery ile şirket içi Hyper-V sanal makineleri (VMM olmadan) için Azure’da olağanüstü durum kurtarma ayarlama | Microsoft Docs
description: Azure Site Recovery hizmeti ile şirket içi Hyper-V sanal makineleri (VMM olmadan) için Azure’da olağanüstü durum kurtarmanın nasıl ayarlanacağını öğrenin.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 04/08/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: da643a4d7a1dc74385b3854c1952af5ba93bd241
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59358089"
---
# <a name="set-up-disaster-recovery-of-on-premises-hyper-v-vms-to-azure"></a>Şirket içi Hyper-V sanal makineleri için Azure’da olağanüstü durum kurtarma ayarlama

c

> [!div class="checklist"]
> * Çoğaltma kaynağınızı ve hedefinizi seçme.
> * Kaynak çoğaltma ortamını, şirket içi Site Recovery bileşenleri ve hedef çoğaltma ortamı dahil olmak üzere ayarlayın.
> * Bir çoğaltma ilkesi oluşturma.
> * Sanal makine için çoğaltmayı etkinleştirme.

> [!NOTE]
> Öğreticiler bir senaryo için en basit dağıtım yolu gösterir. Mümkün olduğunca varsayılan seçenekleri kullanır ve tüm olası ayarları ve yolları göstermez. Ayrıntılı yönergeler için Site Recovery İçindekiler bölümünde nasıl yapılır makalesine gözden geçirin.

## <a name="before-you-begin"></a>Başlamadan önce
Bu, serideki üçüncü öğreticidir. Bu öğreticide, önceki öğreticilerdeki adımları zaten tamamladığınız varsayılır:

1. [Azure’u hazırlama](tutorial-prepare-azure.md)
2. [Şirket içi Hyper-V’leri hazırlama](tutorial-prepare-on-premises-hyper-v.md)



## <a name="select-a-replication-goal"></a>Çoğaltma hedefi seçme

1. **Kurtarma Hizmetleri kasaları** bölümünde kasayı seçin. Hazırladığımız kasayı **ContosoVMVault** önceki öğreticide.
2. **Başlarken** bölümünde **Site Recovery**’ye tıklayın. Daha sonra **Altyapıyı Hazırlama**’ya tıklayın
3. İçinde **koruma hedefi** > **makineleriniz nerede?** seçin **şirket içi**.
4. İçinde **nerede makinelerinizi çoğaltmak istiyorsunuz?** seçin **Azure'a**.
5. İçinde **makineleriniz sanallaştırıldı mı?** seçin **Evet, Hyper-V ile**.
6. **Hyper-V ana bilgisayarlarınızı yönetmek için System Center VMM mi kullanıyorsunuz** başlığında **Hayır**’ı seçin. Daha sonra, **Tamam**'a tıklayın.

    ![Çoğaltma hedefi](./media/hyper-v-azure-tutorial/replication-goal.png)

## <a name="confirm-deployment-planning"></a>Dağıtım planlamasını onaylama

1. İçinde **dağıtım planlaması**, büyük bir dağıtımın planlıyorsanız, Hyper-V için dağıtım Planlayıcısı sayfasındaki bağlantısını indirin. [Daha fazla bilgi edinin](hyper-v-deployment-planner-overview.md) Hyper-V dağıtım planlama hakkında daha fazla.
2. Bu öğreticinin amaçları doğrultusunda, dağıtım Planlayıcısını gerekmez. İçinde **dağıtım planlamasını tamamladınız mı?** seçin **daha sonra yapacağım**. Daha sonra, **Tamam**'a tıklayın.

    ![Dağıtım planlaması](./media/hyper-v-azure-tutorial/deployment-planning.png)

## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Kaynak ortamı ayarlamak için Hyper-V sitesi oluşturun ve bir siteye çoğaltmak istediğiniz Vm'leri içeren Hyper-V konakları ekleyin. Ardından her ana bilgisayarda Azure Site Recovery Sağlayıcısı ile Azure Kurtarma Hizmetleri aracısını indirip yükleyin ve Hyper-V sitesini kasaya kaydedin. 

1. Altında **altyapıyı hazırlama**, tıklayın **kaynak**.
2. İçinde **kaynağı hazırla**, tıklayın **+ Hyper-V sitesi**.
3. İçinde **oluşturma Hyper-V sitesi**ve site adı belirtin. Kullandığımız **ContosoHyperVSite**.

    ![Hyper-V sitesi](./media/hyper-v-azure-tutorial/hyperv-site.png)

3. Sitenin, oluşturulduktan sonra **kaynağı hazırla** > **1. adım: Hyper-V site Seç**, oluşturduğunuz sitesini seçin.
4. **+Hyper-V Sunucusu**’na tıklayın.

    ![Hyper-V sunucusu](./media/hyper-v-azure-tutorial/hyperv-server.png)

4. Microsoft Azure Site Recovery sağlayıcısı yükleyicisini indirin.
6. Kasa kayıt anahtarını indir Sağlayıcı yüklemek için bu anahtarı gerekir. Anahtar, oluşturulduktan sonra beş gün boyunca geçerlidir.

    ![Sağlayıcıyı indirin](./media/hyper-v-azure-tutorial/download.png)
    

### <a name="install-the-provider"></a>Sağlayıcıyı yükleyin

İndirilen kurulum dosyasını (AzureSiteRecoveryProvider.exe) Hyper-V sitesine eklemek istediğiniz her Hyper-V konağına yükleyin. Kurulum işlemi, Azure Site Kurtarma Sağlayıcısı’nı ve Kurtarma Hizmetleri aracısını her bir Hyper-V konağına yükler.

1. Kurulum dosyasını çalıştırın.
2. Azure Site Kurtarma Sağlayıcısı Kurulum sihirbazındaki **Microsoft Update** bölümünde, Sağlayıcı güncelleştirmelerini denetlemek için Microsoft Update’i kullanmayı kabul edin.
3. **Yükleme** bölümünde, Sağlayıcı ve aracı için varsayılan yükleme konumunu kabul edin ve **Yükle**'ye tıklayın.
4. Yüklemeden sonra Microsoft Azure Site Kurtarma Kayıt Sihirbazı’ndaki **Kasa Ayarları**’nda **Gözat**’a tıklayın ve **Anahtar Dosyası** bölümünde indirdiğiniz kasa anahtarı dosyasını seçin. 
5. Azure Site Recovery aboneliğini, kasa adını (**ContosoVMVault**) ve Hyper-V sunucusunun ait olduğu Hyper-V sitesini (**ContosoHyperVSite**) belirtin.
6. **Proxy Ayarları** bölümünde **Proxy sunucusu olmadan doğrudan Azure Site Recovery hizmetine bağlan** seçeneğini belirleyin.
7. **Kayıt** bölümünde, Sunucu kasaya kaydedildikten sonra **Son**’a tıklayın.

Hyper-V sunucusundaki meta veriler, Azure Site Recovery tarafından alınır ve **Site Recovery Altyapısı** > **Hyper-V Konakları** bölümünde sunucu görüntülenir. Bu işlemin tamamlanması 30 dakika sürebilir.        

#### <a name="install-on-hyper-v-core-server"></a>Hyper-V çekirdeği sunucusuna yükleme

Bir Hyper-V çekirdeği sunucusu çalıştırıyorsanız, kurulum dosyasını indirirsiniz ve aşağıdakileri yapın:

1. Dosyaları çalıştırarak AzureSiteRecoveryProvider.exe bir yerel dizine ayıklayın

    `AzureSiteRecoveryProvider.exe /x:. /q`
 
2.  Run `.\setupdr.exe /i. Sonuçları % Programdata%\ASRLogs\DRASetupWizard.log günlüğe kaydedilir.

3.  Sunucu, bu komutla birlikte kaydedin:

    ```
    cd  C:\Program Files\Microsoft Azure Site Recovery Provider\DRConfigurator.exe" /r /Friendlyname "FriendlyName of the Server" /Credentials "path to where the credential file is saved"
    ```
 

## <a name="set-up-the-target-environment"></a>Hedef ortamı ayarlama

Hedef kaynaklarını seçin ve doğrulayın. 

1. **Altyapıyı Hazırlama** > **Hedef** seçeneklerine tıklayın.
2. Yük devretmenin ardından içinde Azure sanal makinelerinin oluşturulacağı **ContosoRG** kaynak grubunu ve aboneliği seçin.
3. **Kaynak Yöneticisi** dağıtım modelini seçin.

Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler.


## <a name="set-up-a-replication-policy"></a>Çoğaltma ilkesi ayarlama

1. **Altyapıyı hazırlama** > **Çoğaltma Ayarları** > **+Oluştur ve ilişkilendir** seçeneklerine tıklayın.
2. **İlke oluştur ve ilişkilendir** bölümünde bir ilke adı belirtin. Kullandığımız **ContosoReplicationPolicy**.
3. Bu öğretici için varsayılan ayarları bırakacağız.
    - **Kopyalama sıklığı** ne sıklıkta gösterir (ilk çoğaltmadan sonra) değişim verileri çoğaltılacağını, varsayılan olarak her beş dakika.
    - **Kurtarma noktası bekletme** kurtarma noktaları için iki saat tutulacağını belirtir.
    - **Uygulamayla tutarlı anlık görüntü sıklığı**, uygulamayla tutarlı anlık görüntüleri içeren kurtarma noktalarının her saat oluşturulacağını belirtir.
    - **İlk çoğaltma başlangıç zamanı**, ilk çoğaltmanın hemen başlatılacağını belirtir.
4. İlke oluşturulduktan sonra **Tamam**’a tıklayın. Yeni bir ilke oluşturduğunuzda, otomatik olarak belirtilen Hyper-V sitesiyle ilişkili (müşterilerimize öğreticide o **ContosoHyperVSite**)

    ![Çoğaltma ilkesi](./media/hyper-v-azure-tutorial/replication-policy.png)


## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme


1. **Uygulama çoğaltma** bölümünde **Kaynak** seçeneğine tıklayın. 
2. **Kaynak** bölümünde **ContosoHyperVSite** sitesini seçin. Daha sonra, **Tamam**'a tıklayın.
3. **Hedef** bölümünde, **Kaynak Yöneticisi** dağıtım modelini, kasa aboneliğini ve hedef olarak Azure’ı doğrulayın.
4. Öğretici ayarları kullanıyorsanız seçin **contosovmsacct1910171607** çoğaltılan veriler için önceki öğreticide oluşturduğunuz depolama hesabına ve **ContosoASRnet** ağında Azure Vm'leri oluşturacak Yük devretmeden sonra bulunması.
5. **Sanal makineler** > **Seç** bölümünde, çoğaltmak istediğiniz sanal makineyi seçin. Daha sonra, **Tamam**'a tıklayın.

   **İşler** > **Site Recovery işleri** bölümünde **Korumayı Etkinleştir** eyleminin ilerleme durumunu izleyebilirsiniz. **Korumayı Sonlandır** işi tamamlandıktan sonra ilk çoğaltma tamamlanır ve sanal makine yük devretme için hazır olur.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Olağanüstü durum kurtarma tatbikatı çalıştırma](tutorial-dr-drill-azure.md)
