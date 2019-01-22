---
title: Azure'da Azure Site Recovery ile VMware olağanüstü durum kurtarma için VMware VM çoğaltmayı etkinleştirme | Microsoft Docs
description: Bu makalede, Azure Site Recovery kullanarak azure'a olağanüstü durum kurtarma için VMware VM çoğaltmayı etkinleştirme açıklar.
author: asgang
ms.service: site-recovery
ms.date: 11/27/2018
ms.topic: conceptual
ms.author: asgang
ms.openlocfilehash: f160fc5f15ad9ca8994995c34d9eba7ee375c015
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54424169"
---
# <a name="enable-replication-to-azure-for-vmware-vms"></a>Azure'a VMware Vm'leri için çoğaltmayı etkinleştirme


Bu makalede, şirket içi VMware Vm'lerinden azure'a çoğaltılmasını etkinleştirmeye açıklar.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede, olduğunu varsayar:

1.  [Şirket içi kaynak ortamını ayarlama](vmware-azure-set-up-source.md).
2.  [Azure'da hedef ortamı ayarlama](vmware-azure-set-up-target.md).


## <a name="before-you-start"></a>Başlamadan önce
VMware sanal makineleri çoğaltılırken:

* Azure kullanıcı hesabınızın belirli olması gerekiyorsa [izinleri](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) yeni bir sanal makinenin azure'a çoğaltılmasını sağlamak.
* VMware Vm'lerini her 15 dakikada bulunur. 15 dakika sürebilir veya bunları bulunduktan sonra Azure portalında görünmesi için daha uzun. Benzer şekilde, yeni bir vCenter sunucusunda veya vSphere konağı eklediğinizde, bulma 15 dakika veya daha fazla sürebilir.
* Ortam değişiklikleri (örneğin, VMware Araçları yüklemesi) sanal makinede, 15 dakika veya portalda güncelleştirilmesi daha fazla sürebilir.
* VMware Vm'leri için son bulunma zamanına göz atabilirsiniz **Last Contact At** alan vCenter sunucusu/vSphere konağını **Configuration Servers** sayfası.
* Zamanlanmış bulmayı beklemeden çoğaltma makineler eklemek için yapılandırma sunucusunu vurgulayın (tıklamayın), tıklatıp **Yenile** düğmesi.
* Makinenin hazır değilse çoğaltmayı etkinleştirdiğinizde, işlem sunucusu üzerinde Mobility hizmetini otomatik olarak yükler.


## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

1. Tıklayın **2. adım: Uygulama çoğaltma** > **kaynak**. Çoğaltmayı ilk kez etkinleştirdikten sonra ek makineler için çoğaltma işlemini etkinleştirmek istiyorsanız kasada **+Çoğalt**'a tıklayın.
2. İçinde **kaynak** sayfası > **kaynak**, yapılandırma sunucusunu seçin.
3. İçinde **makine türü**seçin **sanal makineler** veya **fiziksel makineler**.
4. **vCenter/vSphere Hypervisor** bölümünde vSphere konağını yöneten vCenter sunucusunu veya konağı seçin. Bu ayar, fiziksel makineleri çoğaltma yapıyorsanız geçerli değildir.
5. Yapılandırma sunucusu adı herhangi bir ek işlem sunucularının oluşturmadıysanız olacağı işlem sunucusunu seçin. Daha sonra, **Tamam**'a tıklayın.

    ![Çoğaltma kaynağı etkinleştir](./media/vmware-azure-enable-replication/enable-replication2.png)

6. İçinde **hedef**, abonelik ve devredilen sanal makineler oluşturmak istediğiniz kaynak grubunu seçin. Devredilen sanal makineler için Azure'da kullanmak istediğiniz dağıtım modelini seçin.

7. Verileri çoğaltmak için kullanmak istediğiniz Azure depolama hesabını seçin. 

    > [!NOTE]

    >   * Premium veya standart depolama hesabı seçebilirsiniz. Premium hesabı seçerseniz, devam eden çoğaltma günlükleri için ek standart depolama hesabı belirtmeniz gerekir. Hesapları kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.
    >   * Farklı bir depolama hesabı kullanmak istiyorsanız, aşağıdakileri yapabilirsiniz [oluşturmak](../storage/common/storage-create-storage-account.md). Kaynak Yöneticisi'ni kullanarak bir depolama hesabı oluşturmak için tıklayın **Yeni Oluştur**. 

8. Yük devretme sonrasında çalışmaya başlayan Azure VM'lerinin bağlanacağı Azure ağını ve alt ağını seçin. Ağın, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir. Koruma için seçtiğiniz tüm makinelere ağ ayarını uygulamak için **Seçili makineler için şimdi yapılandır**’ı seçin. Makineler için Azure ağını ayrı ayrı seçmek için **Daha sonra yapılandır**'ı seçin. Bir ağ yoksa, yapmanız [oluşturmak](#set-up-an-azure-network). Kaynak Yöneticisi'ni kullanarak bir ağ oluşturmak için tıklayın **Yeni Oluştur**. Varsa bir alt ağ seçin ve ardından **Tamam**.

    ![Çoğaltma hedefi ayarı etkinleştir](./media/vmware-azure-enable-replication/enable-rep3.png)
9. **Sanal Makineler** > **Sanal makineleri seçin** bölümünde, çoğaltmak istediğiniz her makineyi seçin. Yalnızca çoğaltmanın etkinleştirildiği makineleri seçebilirsiniz. Daha sonra, **Tamam**'a tıklayın. Görünüm/herhangi belirli bir sanal makineyi seçmek mümkün değildir, tıklayın [burada](https://aka.ms/doc-plugin-VM-not-showing) sorunu çözmek için.

    ![Çoğaltma select sanal makineleri etkinleştirme](./media/vmware-azure-enable-replication/enable-replication5.png)
10. İçinde **özellikleri** > **özelliklerini yapılandırmak**, Mobility hizmetini makineye otomatik olarak yüklemeniz için işlem sunucusu tarafından kullanılan hesabı seçin.  
11. Varsayılan olarak, tüm diskler çoğaltılır. Diskleri çoğaltmanın dışında tutma için tıklatın **tüm diskleri** çoğaltmak için istemediğiniz tüm diskleri temizleyin.  Daha sonra, **Tamam**'a tıklayın. Daha sonra ek özellikleri ayarlayabilirsiniz. [Daha fazla bilgi edinin](vmware-azure-exclude-disk.md) diskleri dışlama hakkında.

    ![Etkinleştirme çoğaltma özellikleri yapılandırma](./media/vmware-azure-enable-replication/enable-replication6.png)

12. **Çoğaltma ayarları** > **Çoğaltma ayarlarını yapılandırma** bölümünde doğru çoğaltma ilkesinin seçilip seçilmediğini doğrulayın. Çoğaltma İlkesi ayarları değiştirebileceğiniz **ayarları** > **çoğaltma ilkeleri** > (ilke adı) > **ayarlarını Düzenle**. Değişiklikleri uygulamak için bir ilke de uygulamak için çoğaltma ve yeni makine.
13. Etkinleştirme **çoklu VM tutarlılığını** makineler çoğaltma grubuna toplamak istiyorsanız. Grup için bir ad belirtin ve ardından **Tamam**. 

    > [!NOTE]

    >    * Bir çoğaltma grubundaki makineler birlikte çoğaltılır ve devrettikleri zaman kilitlenmeyle tutarlı ve uygulamayla tutarlı kurtarma noktalarını paylaştıktan.
    >    * Böylece bunlar iş yüklerinizi yansıtma Vm'leri ve fiziksel sunucuları araya toplayın. Çoklu VM tutarlılığını etkinleştirmek, iş yükü performansını etkileyebilir. Yalnızca makineler aynı iş yükünü çalıştırıyorsa ve tutarlılık ihtiyacınız varsa bu seçeneği kullanın.

    ![Çoğaltmayı etkinleştirme](./media/vmware-azure-enable-replication/enable-replication7.png)
14. **Çoğaltmayı Etkinleştir**’e tıklayın. **Ayarlar** > **İşler** > **Site Recovery İşleri** bölümünden **Korumayı Etkinleştir** işinin ilerleme durumunu izleyebilirsiniz. **Korumayı Sonlandır** işi çalıştırıldıktan sonra makine yük devretme için hazırdır.



## <a name="view-and-manage-vm-properties"></a>VM özelliklerini görüntüleme ve yönetme

Ardından, kaynak makinenin özelliklerini doğrulayın. Azure VM adının karşılaması gerektiğini unutmayın [Azure sanal makine gereksinimlerine](vmware-physical-azure-support-matrix.md#replicated-machines).

1. Tıklayın **ayarları** > **çoğaltılan öğeler** > ve ardından makine seçin. **Essentials** sayfa makine ayarları ve durumu hakkında bilgileri gösterir.
2. **Özellikler** kısmında VM'nin çoğaltma ve yük devretme bilgilerini inceleyebilirsiniz.
3. **İşlem ve Ağ** > **İşlem özellikleri** seçeneklerinden Azure VM adını ve hedef boyutu belirtebilirsiniz. Gerekirse Azure gereksinimlerine uymak için adı değiştirin.

    ![İşlem ve ağ özellikleri](./media/vmware-azure-enable-replication/vmproperties.png)

4.  Seçebileceğiniz bir [kaynak grubu](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-resource-groups-guidelines) öğesinden, bir makine yük devretme sonrasında bir parçası haline gelir. Yük devretmeden önce dilediğiniz zaman bu ayarı değiştirebilirsiniz. Farklı kaynak grubuna, bu makine sonu için koruma ayarlarını makineyi geçirirseniz, yük devretme gönderin.
5. Seçebileceğiniz bir [kullanılabilirlik kümesi](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) yük devretme sonrasında bir parçası olarak makinenize gerekiyorsa. Bir kullanılabilirlik kümesi seçeneğini belirliyoruz ancak unutmayın:

    * Belirtilen kaynak grubuna ait kullanılabilirlik kümeleri listelenir.  
    * Farklı sanal ağlar ile makineler aynı kullanılabilirlik kümesinin bir parçası olamaz.
    * Yalnızca aynı boyutta sanal makineler bir kullanılabilirlik kümesinin bir parçası olabilir.
5. Ayrıca, görüntüleyebilir ve hedef ağ, alt ağ ve Azure VM'sine atanan IP adresi hakkında bilgi ekleyin.
6. İçinde **diskleri**, çoğaltılacak VM'deki işletim sistemi ve veri disklerini görebilirsiniz.

### <a name="configure-networks-and-ip-addresses"></a>Ağları ve IP adreslerini yapılandırın

- Hedef IP adresini ayarlayabilirsiniz. Bir adresi sağlamazsanız, devredilen makine DHCP kullanır. Yük devretmede kullanılamayan bir adres ayarlarsanız yük devretme çalışmaz. Aynı hedef IP adresi, yük devretme ağı testinde adresi varsa, yük devretme testi için kullanılabilir.
- Ağ bağdaştırıcılarının sayısı, hedef sanal makine için sizin belirlediğiniz boyuta göre aşağıdaki gibi belirlenmiştir:
    - Kaynak makinedeki ağ bağdaştırıcı sayısı hedef makine boyutu için izin verilen bağdaştırıcıları sayısına eşit veya daha az ise, hedef kaynak olarak aynı sayıda bağdaştırıcıya sahip.
    - Kaynak sanal makinenin bağdaştırıcı sayısı hedef boyut için izin verilen sayıyı aşarsa maksimum hedef boyutu kullanılır.
    Örneğin, kaynak makinenin iki bağdaştırıcısı varsa ve hedef makine boyutu dört adet bağdaştırıcıyı destekliyorsa hedef makinenin iki bağdaştırıcısı vardır. Kaynak makinenin iki bağdaştırıcısı varken hedef boyut yalnızca bir destekler, hedef makine yalnızca bir bağdaştırıcı vardır.
    - Sanal makinede birden çok ağ bağdaştırıcısı varsa, bunların tümü aynı ağa bağlayın. Ayrıca, listede ilk gösterilene olur *varsayılan* Azure sanal makine ağ bağdaştırıcısı.

### <a name="azure-hybrid-benefit"></a>Azure Hibrit Avantajı

Microsoft Yazılım Güvencesi müşterileri, Azure'a geçirilen Windows Server makineleri ilgili lisans kaydetmeye veya olağanüstü durum kurtarma için Azure'ı kullanmak için Azure hibrit avantajı kullanabilirsiniz. Azure hibrit Avantajı'nı kullanmak uygun değilse, bir yük devretme ise Azure Site Recovery oluşturur. Bu Avantajdan atanan sanal makine olduğunu belirtebilirsiniz. Bunu yapmak için:
- Çoğaltılan sanal makinenin işlem ve ağ özellikleri bölümüne gidin.
- Azure hibrit avantajı için uygun hale gelir bir Windows Server lisansınız varsa ister soruyu yanıtlayın.
- Yük devretme sırasında oluşturulan makineye Azure hibrit avantajı uygulamak için kullanabileceğiniz Yazılım Güvencesi olan uygun bir Windows Server Lisansı sahip olduğunuzu onaylamak üzere onay kutusunu işaretleyin.
- Çoğaltılan makinelerin için ayarları kaydedin.

Daha fazla bilgi edinin [Azure hibrit avantajı](https://aka.ms/azure-hybrid-benefit-pricing).

## <a name="common-issues"></a>Genel sorunlar

* Her diskin boyutu 1 TB'tan küçük olmalıdır.
* İşletim sistemi diskinin bir temel disk ve dinamik bir diski olmalıdır.
* Nesil 2/UEFI özellikli sanal makineler için işletim sistemi Windows olmalıdır ve önyükleme diskinin boyutu 300 GB'tan fazla olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Koruma tamamlandıktan sonra makineyi korumalı bir duruma ulaştı, deneyebileceğiniz bir [yük devretme](site-recovery-failover.md) uygulamanızı Azure'da veya göründüğünde olup olmadığını denetlemek için.

Koruma devre dışı bırakmak isterseniz, bilgi nasıl [kayıt ve koruma ayarlarını temizleyin](site-recovery-manage-registration-and-protection.md).
