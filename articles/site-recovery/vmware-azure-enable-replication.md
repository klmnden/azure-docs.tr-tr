---
title: Azure Site Recovery ile azure'a olağanüstü durum kurtarma için VMware VM çoğaltmayı etkinleştirme | Microsoft Docs
description: Bu makalede, Azure Site Recovery kullanarak VMware Vm'lerini azure'a çoğaltma, olağanüstü durum kurtarma için etkinleştirmek açıklar.
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.date: 4/18/2019
ms.topic: conceptual
ms.author: ramamill
ms.openlocfilehash: ba55afbd62bbbc2290d1daaebf77becc249c1d8b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60922823"
---
# <a name="enable-replication-to-azure-for-vmware-vms"></a>Azure'a VMware Vm'leri için çoğaltmayı etkinleştirme

Bu makalede, şirket içi VMware Vm'lerinden azure'a çoğaltılmasını etkinleştirmeye açıklar.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede, olduğunu varsayar:

- [Şirket içi kaynak ortamınızı](vmware-azure-set-up-source.md).
- [Azure'da, hedef ortamı ayarlama](vmware-azure-set-up-target.md).

## <a name="before-you-start"></a>Başlamadan önce
VMware sanal makinelerini çoğaltma yapıyorsanız, bu bilgileri göz önünde bulundurun:

* Azure kullanıcı hesabınızın belirli olması gerekiyorsa [izinleri](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) yeni bir sanal makinenin azure'a çoğaltılmasını sağlamak.
* VMware Vm'lerini her 15 dakikada bulunur. 15 dakika alabilir veya VM'ler bulunduktan sonra Azure portalında görünmesi için daha uzun. Benzer şekilde, bulma 15 dakika alabilir veya yeni bir vCenter sunucusunda veya vSphere konağı eklediğinizde, daha uzun.
* 15 dakika alabilir veya uzun süre ortam değişiklikleri (örneğin, VMware Araçları yüklemesi) sanal makinede portalda güncelleştirilecek.
* Son keşfedilen zaman VMware Vm'leri için kontrol edebilirsiniz: Bkz: **Last Contact At** alanını **Configuration Servers** vCenter sunucusu/vSphere ana sayfası.
* Zamanlanmış bulmayı beklemeden çoğaltma sanal makineler eklemek için yapılandırma sunucusunu vurgulayın (ancak tıklamayın) seçip **Yenile**.
* Sanal makine hazır değilse çoğaltmayı etkinleştirdiğinizde, işlem sunucusu üzerinde Azure Site Recovery Mobility hizmetini otomatik olarak yükler.

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

Bu bölümdeki adımları izlemeden önce aşağıdaki bilgileri unutmayın:
* Tüm yeni çoğaltmalar için yönetilen diskler için doğrudan Azure Site Recovery artık çoğaltır. İşlem sunucusu, hedef bölgedeki önbellek depolama hesabına çoğaltma günlükleri yazar. Bu günlükler, yönetilen çoğaltma disklerinde kurtarma noktaları oluşturmak için kullanılır.
* Yük devretme sırasında seçtiğiniz kurtarma noktası, hedef tarafından yönetilen disk oluşturmak için kullanılır.
* Hedef depolama hesaplarında çoğaltılmaya önceden yapılandırılmış sanal makineleri etkilenmez.
* Yeni bir sanal makine için depolama hesaplarına çoğaltma, yalnızca bir temsili durum aktarımı (REST) API ve Powershell kullanılabilir. Depolama hesapları için çoğaltmak için Azure REST API sürümü 2016-08-10 veya 2018-01-10 kullanın.

1. Git **2. adım: Uygulama çoğaltma** > **kaynak**. Çoğaltmayı ilk kez etkinleştirdikten sonra seçin **+ Çoğalt** ek sanal makineler için çoğaltma işlemini etkinleştirmek istiyorsanız kasada.
1. İçinde **kaynak** sayfası > **kaynak**, yapılandırma sunucusunu seçin.
1. İçin **makine türü**seçin **sanal makineler** veya **fiziksel makineler**.
1. **vCenter/vSphere Hypervisor** bölümünde vSphere konağını yöneten vCenter sunucusunu veya konağı seçin. Bu ayar, fiziksel bilgisayarları çoğaltıyorsanız geçerli değildir.
1. Yapılandırma sunucusu herhangi bir ek işlem sunucularının oluşturmadıysanız olacağı işlem sunucusunu seçin. Sonra **Tamam**’ı seçin.

    ![Çoğaltma kaynağı pencereyi etkinleştir](./media/vmware-azure-enable-replication/enable-replication2.png)

1. İçin **hedef**, devredilen sanal makineler oluşturmak istediğiniz aboneliği ve kaynak grubunu seçin. Devredilen VM'ler için Azure'da kullanmak istediğiniz dağıtım modelini seçin.

1. Azure ağ ve yük devretme sonrasında Azure Vm'lerinin bağlanacağı alt ağı seçin. Ağ sitesi kurtarma Hizmetleri kasası ile aynı bölgede olması gerekir.

   Seçin **seçili makineler için Şimdi Yapılandır** koruma için seçtiğiniz tüm sanal makinelere ağ ayarını uygulamak için. Seçin **daha sonra yapılandırma** sanal makine başına Azure ağını seçin. Bir ağ yoksa, oluşturmanız gerekir. Azure Resource Manager'ı kullanarak bir ağ oluşturmak için Seç **Yeni Oluştur**. Varsa bir alt ağ seçin ve ardından **Tamam**.
   
   ![Çoğaltma hedefi pencereyi etkinleştir](./media/vmware-azure-enable-replication/enable-rep3.png)

1. İçin **sanal makineler** > **sanal makineleri**, çoğaltmak istediğiniz her bir sanal makineyi seçin. Yalnızca sanal makineler çoğaltma etkinleştirilebilir seçebilirsiniz. Sonra **Tamam**’ı seçin. Bttn'lerinizi göremez veya belirli bir sanal makineyi seçin, bakın [kaynak makine, Azure Portalı'nda listelenmez](https://aka.ms/doc-plugin-VM-not-showing) sorunu çözmek için.

    ![Çoğaltma sanal makineleri Seç penceresi etkinleştir](./media/vmware-azure-enable-replication/enable-replication5.png)

1. İçin **özellikleri** > **özelliklerini yapılandırmak**, işlem sunucusu, Site Recovery Mobility hizmeti sanal makine üzerinde otomatik olarak yüklemek için kullandığı hesabı seçin. Ayrıca, hedef tabanlı verilerinize ilişkin karmaşıklığı desenleri çoğaltmak için yönetilen disk türünü seçin.
10. Varsayılan olarak, kaynak sanal makinenin tüm diskler çoğaltılır. Diskleri çoğaltmanın dışında tutma için temizleyin **INCLUDE** çoğaltmak için istemediğiniz tüm diskler için onay kutusu. Sonra **Tamam**’ı seçin. Daha sonra ek özellikleri ayarlayabilirsiniz. Daha fazla bilgi edinin [diskleri dışlama](vmware-azure-exclude-disk.md).

    ![Etkinleştirme çoğaltmayı yapılandırma Özellikler penceresi](./media/vmware-azure-enable-replication/enable-replication6.png)

1. Konumunda **çoğaltma ayarları** > **çoğaltma ayarlarını yapılandırma**, doğru çoğaltma ilkesinin seçilip seçilmediğini doğrulayın. Çoğaltma İlkesi ayarları değiştirebileceğiniz **ayarları** > **çoğaltma ilkeleri** > ***ilke adı***  >  **Ayarlarını Düzenle**. Bir ilkeyi uygulamak değişiklikleri de uygulamak için çoğaltma ve yeni sanal makine.
1. Etkinleştirme **çoklu VM tutarlılığını** sanal makineler çoğaltma grubuna toplamak istiyorsanız. Grup için bir ad belirtin ve ardından **Tamam**.

    > [!NOTE]
    >    * Bir çoğaltma grubundaki sanal makineler birlikte çoğaltılır ve devrettikleri zaman kilitlenmeyle tutarlı ve uygulamayla tutarlı kurtarma noktalarını paylaştıktan.
    >    * Böylece bunlar iş yüklerinizi yansıtma Vm'leri ve fiziksel sunucuları araya toplayın. Çoklu VM tutarlılığını etkinleştirmek, iş yükü performansını etkileyebilir. Bunu yapmak, yalnızca sanal makineler aynı iş yükünü çalıştırıyorsa ve tutarlılık gerekir.

    ![Çoğaltma pencereyi etkinleştir](./media/vmware-azure-enable-replication/enable-replication7.png)
    
1. **Çoğaltmayı Etkinleştir** seçeneğini belirleyin. İlerleme durumunu izleyebilirsiniz **korumayı etkinleştir** sırasında iş **ayarları** > **işleri** > **Site Recovery işleri**. Sonra **korumayı Sonlandır** işi çalıştığından, sanal makine yük devretme için hazırdır.

## <a name="view-and-manage-vm-properties"></a>VM özelliklerini görüntüleme ve yönetme

Ardından, kaynak sanal makinenin özelliklerini doğrulayın. Azure VM adının karşılaması gerektiğini unutmayın [Azure sanal makine gereksinimlerine](vmware-physical-azure-support-matrix.md#replicated-machines).

1. Git **ayarları** > **çoğaltılan öğeler**ve ardından sanal makineyi seçin. **Essentials** sayfasında sanal makinenin ayarlarını ve durumu hakkında bilgi gösterir.
1. **Özellikler** kısmında VM'nin çoğaltma ve yük devretme bilgilerini inceleyebilirsiniz.
1. İçinde **işlem ve ağ** > **işlem özellikleri**, birden çok VM özelliklerini değiştirebilirsiniz. 

    ![İşlem ve ağ özellikleri penceresi](./media/vmware-azure-enable-replication/vmproperties.png)

    * Azure VM adı: Azure gereksinimleri karşılamak için adı gerekiyorsa değiştirin.
    * Hedef VM boyutu veya VM türü: Varsayılan VM boyutu bağlı Disk sayısı, NIC sayısı, CPU çekirdek sayısı, bellek ve kullanılabilir sanal makine rolü boyutları hedef Azure bölgeniz dahil birkaç parametre olarak seçilir. Azure Site Recovery tüm ölçütleri karşılayan ilk kullanılabilir VM boyutu seçer. Yük devretmeden önce herhangi bir zamanda ihtiyaçlarınıza göre farklı bir VM boyutu seçebilirsiniz. VM disk boyutu Ayrıca kaynak disk boyutu temel alınır ve yalnızca yük devretme işleminden sonra değiştirilebilir unutmayın. Disk boyutları ve IOPS fiyatları hakkında daha fazla bilgi [Windows üzerinde VM diskleri için ölçeklenebilirlik ve performans hedefleri](../virtual-machines/windows/disk-scalability-targets.md).

    *  Kaynak grubu: Seçebileceğiniz bir [kaynak grubu](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-resource-groups-guidelines), öğesinden, bir sanal makine yük devretme sonrasında bir parçası haline gelir. Yük devretmeden önce herhangi bir zamanda bu ayarı değiştirebilirsiniz. Farklı kaynak grubuna, sanal makineyi geçirirseniz, sanal makine için koruma ayarları yük devretme sonrasında bölün.
    * Kullanılabilirlik kümesi: Seçebileceğiniz bir [kullanılabilirlik kümesi](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) , sanal makine yük devretme sonrasında bir parçası olması gerekiyorsa. Bir kullanılabilirlik kümesi seçtiğinizde, aşağıdaki bilgileri göz önünde bulundurun:

        * Belirtilen kaynak grubunda yalnızca kullanılabilirlik kümeleri listelenir.  
        * Farklı sanal ağlardaki sanal makineler aynı kullanılabilirlik kümesinin bir parçası olamaz.
        * Yalnızca aynı boyutta sanal makineler bir kullanılabilirlik kümesinin bir parçası olabilir.
1. Hedef ağ, alt ağ ve Azure VM'sine atanan IP adresi hakkında bilgi de ekleyebilirsiniz.
2. İçinde **diskleri**, çoğaltılacak VM'deki işletim sistemi ve veri disklerini görebilirsiniz.

### <a name="configure-networks-and-ip-addresses"></a>Ağları ve IP adreslerini yapılandırın

Hedef IP adresini ayarlayabilirsiniz. Bir adresi sağlamazsanız, devredilen sanal makine DHCP kullanır. Yük devretmede kullanılamayan bir adres ayarlarsanız yük devretme çalışmaz. Yük devretme ağı testinde adresi varsa, yük devretme testi için aynı hedef IP adresi kullanabilirsiniz.

Ağ bağdaştırıcılarının sayısı, hedef sanal makine için aşağıdaki gibi belirtin boyutu tarafından belirlenir:

- Kaynak sanal makinedeki ağ bağdaştırıcı sayısı hedef sanal makinenin boyutu için izin verilen bağdaştırıcıları sayısına eşit veya daha az ise, hedef kaynak olarak aynı sayıda bağdaştırıcıya sahip.
- Kaynak sanal makinenin bağdaştırıcı sayısı hedef sanal makinenin boyutu için izin verilen sayıyı aşarsa maksimum hedef boyutu kullanılır. Örneğin, kaynak sanal makinenin iki bağdaştırıcısı varsa ve hedef sanal makinenin boyutu dört adet bağdaştırıcıyı destekliyorsa hedef sanal makinenin iki bağdaştırıcısı vardır. Kaynak VM iki bağdaştırıcısı varken hedef boyut yalnızca bağdaştırıcıyı destekliyorsa hedef sanal makine yalnızca bir bağdaştırıcı yoktur.
- Sanal makinede birden çok ağ bağdaştırıcısı varsa, bunların tümü aynı ağa bağlayın. Ayrıca, listede gösterilen ilk bağdaştırıcısı olur *varsayılan* Azure sanal makine ağ bağdaştırıcısı. 

### <a name="azure-hybrid-benefit"></a>Azure Hibrit Avantajı

Microsoft Yazılım Güvencesi müşterileri, Azure hibrit teklifi Azure'a geçirilen bilgisayarlar Windows Server lisanslama maliyetleri kaydetmek için kullanabilirsiniz. Avantajı, Azure olağanüstü durum kurtarma için de geçerlidir. Uygunsanız, sanal makine için bir yük devretme ise Site Recovery oluşturan avantajı atayabilirsiniz. Bunu yapmak için şu adımları uygulayın:
1. Git **bilgisayar ve ağ özellikleri** çoğaltılan sanal makinenin.
2. Azure hibrit avantajı için uygun hale gelir bir Windows Server lisansınız varsa sorulduğunda yanıtlayın.
3. Yük devretme sırasında oluşturulacak VM teklifi uygulamak için kullanabileceğiniz bir Yazılım Güvencesi olan uygun bir Windows Server Lisansı sahip olduğunuzdan emin olun.
4. Çoğaltılmış sanal makine için ayarları kaydedin.

Daha fazla bilgi edinin [Azure hibrit avantajı](https://aka.ms/azure-hybrid-benefit-pricing).

## <a name="resolve-common-issues"></a>Yaygın sorunları çözme

* Her disk, 4 TB'den küçük olmalıdır.
* İşletim sistemi diski, dinamik disk değil temel bir disk olmalıdır.
* Nesil 2/UEFI özellikli sanal makineler için işletim sistemi Windows olmalıdır ve önyükleme diski 300 GB'den daha küçük olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Sanal makine, korumalı bir duruma ulaştıktan sonra deneyin bir [yük devretme](site-recovery-failover.md) uygulamanızı Azure'da görünüp görünmediğini denetlemek için.

* Bilgi nasıl [kayıt ve koruma ayarlarını temizleyin](site-recovery-manage-registration-and-protection.md) çoğaltma devre dışı bırakmak için.
* Bilgi edinmek için nasıl [Powershell kullanarak çoğaltma sanal makineleriniz için otomatik hale](vmware-azure-disaster-recovery-powershell.md).
