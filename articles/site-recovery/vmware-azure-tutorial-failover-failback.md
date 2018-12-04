---
title: Olağanüstü durum kurtarma sırasında VMware VM’lerde ve fiziksel sunucularda Azure Site Recovery’ye yük devretme ve yeniden çalışma | Microsoft Docs
description: Olağanüstü durum kurtarma sırasında Azure Site Recovery ile VMware VM’lerde ve fiziksel sunucularda Azure’a yük devretme ve şirket içi sitede yeniden çalışma hakkında bilgi edinin
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 11/27/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 517355a32fc7a549370aed2c7a8408c3a0887e13
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52838030"
---
# <a name="fail-over-and-fail-back-vmware-vms-and-physical-servers-replicated-to-azure"></a>VMware VM’lerde ve çoğaltılmış fiziksel sunucularda Azure Site Recovery’ye yük devretme ve yeniden çalışma

Bu öğreticide, bir VMware VM’de Azure’a nasıl yük devredileceği açıklanmaktadır. Yük devrettikten sonra şirket içi siteniz kullanılabilir olduğunda yeniden çalıştırabilirsiniz. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure gereksinimleriyle uyumlu olup olmadığını denetlemek için VMware VM özelliklerini doğrulama
> * Azure'a yük devretme işlemini çalıştırma
> * Yeniden çalıştırma için bir işlem sunucusu ve ana hedef sunucu oluşturma
> * Azure VM’lerini şirket içi sitede yeniden koruma
> * Azure'dan şirket içine yük devretme
> * Tekrar Azure’a çoğaltmaya başlamak için şirket içi VM’leri yeniden koruma

>[!NOTE]
>Öğreticiler, bir senaryo için en basit dağıtım yolunu size göstermek için tasarlanmıştır. Mümkün olduğunca varsayılan seçenekleri kullanır ve tüm olası ayarları ve yolları göstermez. Yük devretme testi adımları hakkında ayrıntılı bilgi edinmek istiyorsanız, [Nasıl Yapılır Kılavuzu](site-recovery-failover.md)’nu okuyun.

Bu, serideki beşinci öğreticidir. Bu öğreticide, önceki öğreticilerdeki adımları zaten tamamladığınız varsayılır.

1. [Azure’u hazırlama](tutorial-prepare-azure.md)
2. [Şirket içi VMware’leri hazırlama](vmware-azure-tutorial-prepare-on-premises.md)
3. [Olağanüstü durum kurtarmayı ayarlama](vmware-azure-tutorial.md)
4. [Olağanüstü durum kurtarma tatbikatı çalıştırma](tutorial-dr-drill-azure.md)
5. Yukarıdaki adımlara ek olarak, olağanüstü durum kurtarma senaryosu için [mimariyi gözden geçirmeniz](vmware-azure-architecture.md) yararlı olabilir.

## <a name="failover-and-failback"></a>Yük devretme ve yeniden çalışma

Yük devretme ve yeniden çalışma dört aşamalıdır:

1. **Azure’a yük devretme**: Makineleri şirket içi siteden Azure’a devredin.
2. **Azure VM’lerini yeniden koruma**: Şirket içi VMware VM’lerine çoğaltılmaya başlamaları için Azure VM’lerini yeniden koruyun. Şirket içi VM yeniden koruma sırasında kapatılır. Bu işlem, çoğaltma sırasında veri tutarlığını sağlar.
3. **Şirket içine yük devretme**: Azure’dan yeniden çalıştırmak için bir yük devretme işlemi çalıştırın.
4. **Şirket içi VM’leri yeniden koruma**: Veriler yeniden çalıştıktan sonra Azure’a çoğaltılmaya başlamaları için yeniden çalışma uyguladığınız şirket içi VM’leri yeniden koruyun.

## <a name="verify-vm-properties"></a>VM özelliklerini doğrulama

VM özelliklerini doğrulayın ve VM’nin [Azure gereksinimleriyle](vmware-physical-azure-support-matrix.md#replicated-machines) uyumlu olduğundan emin olun.

1. **Korunan Öğeler**’de **Çoğaltılan Öğeler** > VM seçeneğine tıklayın.

2. **Çoğaltılan öğe** bölmesinde VM bilgileri ile sistem durumunun bir özeti ve kullanılabilen son kurtarma noktaları yer alır. Daha fazla ayrıntı görüntülemek için **Özellikler**’e tıklayın.

3. **İşlem ve Ağ** bölümünde Azure adını, kaynak grubunu, hedef boyutunu, [kullanılabilirlik kümesini](../virtual-machines/windows/tutorial-availability-sets.md) ve [yönetilen disk ayarlarını](#managed-disk-considerations) değiştirebilirsiniz

4. Ağ ayalarını, yük devretmeden sonra Azure VM’nin yerleştirileceği ağ/alt ağ ve VM’ye atanacak IP adresi dahil olmak üzere görüntüleyebilir ve değiştirebilirsiniz.

5. **Diskler** bölümünde VM’deki işletim sistemi ve veri diskleriyle ilgili bilgileri görüntüleyebilirsiniz.

## <a name="run-a-failover-to-azure"></a>Azure'a yük devretme işlemini çalıştırma

1. **Ayarlar** > **Çoğaltılan öğeler** bölümünde VM > **Yük devretme**’ye tıklayın.

2. **Yük devretme** kısmında, yük devredeceğiniz bir **Kurtarma Noktası** seçin. Şu seçeneklerden birini kullanabilirsiniz:
   - **Varsayılan**: Bu seçenekte öncelikle Site Recovery’ye gönderilen tüm veriler işlenir. Yük devretmeden sonra oluşturulan Azure VM, yük devretme tetiklendiğinde Site Recovery’ye çoğaltılan tüm verilere sahip olduğundan en düşük RPO (Kurtarma Noktası Hedefi) sağlanır.
   - **En son işlenen**: Bu seçenekte VM yükü, Site Recovery tarafından işlenen en son kurtarma noktasına devredilir. İşlenmemiş verileri işlemek için zaman harcanmadığından bu seçenekte düşük bir RTO (Kurtarma Süresi Hedefi) sağlanır.
   - **En son uygulamayla tutarlı**: Bu seçenekte VM yükü, Site Recovery tarafından işlenen, uygulamayla tutarlı en son kurtarma noktasına devredilir.
   - **Özel**: Bir kurtarma noktası belirtin.

3. Yük devretmeyi tetiklemeden önce kaynak sanal makineleri kapatmayı denemek için **Yük devretmeye başlamadan önce makineyi kapat** seçeneğini belirleyin. Kapatma işlemi başarısız olsa bile yük devretme devam eder. Yük devretme işleminin ilerleme durumunu **İşler** sayfasında takip edebilirsiniz.

Bazı senaryolarda yük devretme için sekiz ila on dakikada tamamlanan ek işlem gerekir. 9.8’den daha eski bir sürümün Mobility hizmetini kullanan VMware sanal makineleri, fiziksel sunucular, VMWare Linux sanal makineleri, fiziksel sunucu olarak korunan Hyper-V sanal makineleri, DHCP hizmeti etkinleştirilmemiş VMware VM'leri ve storvsc, vmbus, storflt, intelide, atapi önyükleme sürücüleri olmayan VMware VM'lerinde **daha uzun yük devretme testi süresi** gözlemleyebilirsiniz.

> [!WARNING]
> **Devam eden yük devretme işlemini iptal etmeyin**: Yük devretme başlatılmadan önce VM çoğaltma durdurulur.
> Devam eden bir yük devretme işlemini iptal ederseniz yük devretme durdurulur, ancak VM yeniden çoğaltılmaz.

## <a name="connect-to-failed-over-virtual-machine-in-azure"></a>Azure’da yük devretme sanal makinesine bağlanma

1. Yük devretme sonrasında RDP/SSH kullanarak Azure VM'lerine bağlanmak istiyorsanız [buradaki](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover) tabloda özetlenen gereksinimleri izleyin.
2. Yük devretme işleminden sonra sanal makineye gidin ve [bağlanarak](../virtual-machines/windows/connect-logon.md) doğrulayın.
3. Doğrulama sonrasında, yük devretme işleminin ardından sanal makinenin kurtarma noktasını kesinleştirmek için **İşle**’ye tıklayın. İşleme sonrasında diğer tüm kullanılabilir erişim noktaları silinir. Bu, yük devretme etkinliğini tamamlar.

>[!TIP]
> **Kurtarma noktasını değiştirme**, yük devretme işleminden sonra yükün devredildiği sanal makinedeki kurtarma noktasından memnun değilseniz değiştirmenize yardımcı olur. **İşleme** sonrasında bu seçenek artık kullanılamaz.

Yük devretme sonrasında karşılaştığınız bağlantı sorunlarını gidermek için [burada](site-recovery-failover-to-azure-troubleshoot.md) anlatılan adımları izleyin.

## <a name="preparing-for-reprotection-of-azure-vm"></a>Azure VM’yi yeniden korumaya hazırlanma

- **Azure ExpressRoute bağlantısına** sahipseniz kurulumun bir parçası olarak yapılandırma sunucusuna otomatik olarak yüklenmiş bir şekilde gelen şirket içi işlem sunucusunu (yerleşik işlem sunucusu) kullanabilirsiniz.

> [!IMPORTANT]
> Şirket içi ortamınız ile Azure arasında bir VPN bağlantınız varsa yeniden koruma ve yeniden çalışma için işlem sunucusu olarak bir Azure VM’si kurmanız gerekir. Azure’da bir işlem sunucusu ayarlamak için [bu makaledeki](vmware-azure-set-up-process-server-azure.md) yönergeleri uygulayın.

Yeniden koruma ve yeniden çalışmaya yönelik önkoşullar hakkında daha fazla bilgi için şu bölüme başvurun: [section] ](vmware-azure-reprotect.md##before-you-begin). 

### <a name="configure-the-master-target-server"></a>Ana hedef sunucuyu yapılandırma

Ana hedef sunucu, Azure'dan yeniden çalışma sırasında çoğaltma verilerini alır ve işler. Varsayılan olarak şirket içi yapılandırma sunucusunda bulunur. Bu öğreticide, varsayılan ana hedef sunucuyu kullanalım.

>[!NOTE]
>Linux tabanlı bir sanal makineyi korumak için ayrı Ana Hedef Sunucu oluşturulması gerekir. Daha fazla bilgi için [buraya](vmware-azure-install-linux-master-target.md) tıklayın.

VM, **vCenter sunucusu tarafından yönetilen bir ESXi ana bilgisayarı** üzerindeyse, çoğaltılan verileri VM disklerine yazmak için ana hedef sunucunun VM veri deposuna (VMDK) erişimi olması gerekir. VM veri deposunun, ana hedefin ana bilgisayarına okuma/yazma erişimi ile bağlı olduğundan emin olun.

VM, **vCenter sunucusu tarafından yönetilmeyen bir ESXi** üzerindeyse, Site Recovery hizmeti yeniden koruma sırasında yeni bir VM oluşturur. VM, ana hedefi oluşturduğunuz ESX ana bilgisayarı üzerinde oluşturulur.
VM’nin sabit diski, üzerinde ana hedef sunucunun çalıştığı ana bilgisayar tarafından erişilebilen bir veri deposunda olmalıdır.

VM, **vCenter kullanmıyorsa** makineyi yeniden korumadan önce, üzerinde ana hedef sunucunun çalıştığı ana bilgisayarı bulma işlemini tamamlamanız gerekir. Bu, fiziksel sunucuları yeniden çalıştırma işlemi için de geçerlidir. Başka bir seçenek de şirket içi VM varsa yeniden çalışma gerçekleştirmeden önce VM’yi silmektir. Yeniden çalışma, ana hedef ESX sunucusu ile aynı ana bilgisayarda yeni bir VM oluşturur. Farklı bir konuma yeniden çalışma gerçekleştirdiğinizde, veriler şirket içi ana hedef sunucu tarafından kullanılan aynı veri deposuna ve aynı ESX ana bilgisayarına kurtarılır.

Ana hedef sunucuda Storage vMotion kullanamazsınız. Kullandığınız takdirde diskler işlem için kullanılabilir olmadığından yeniden çalışma gerçekleştirilemez. Ana hedef sunucuları vMotion listenizin dışında tutun.

>[!Warning]
>Çoğaltma grubunu yeniden korumak için farklı bir ana hedef sunucu kullanırsanız, sunucu ortak bir zaman noktası sağlayamaz.

## <a name="reprotect-azure-vms"></a>Azure VM’lerini yeniden koruma

Azure VM’sini yeniden koruma, şirket içi VM’ye veri çoğaltılmasına yol açar. Azure’dan şirket içi VM’ye yük devretme işlemi için bu adım zorunludur. Yeniden korumayı yürütmek için aşağıda verilen yönergeleri izleyin.

1. **Ayarlar** > **Çoğaltılan öğeler** bölümünde yük devretme gerçekleştirilen VM’ye sağ tıklayın ve **Yeniden Koru** seçeneğini belirleyin.
2. **Yeniden koru** bölümünde **Azure’dan Şirket içine** seçeneğinin belirlendiğinden emin olun.
3. Şirket içi ana hedef sunucuyu ve işlem sunucusunu belirtin.
4. **Veri deposu**’nda şirket içi diskleri kurtarmak istediğiniz ana hedef veri deposunu seçin. VM silinirse, yeni diskler bu veri deposunda oluşturulur. Diskler zaten varsa ancak bir değer belirtmeniz gerekiyorsa, bu ayar yoksayılır.
5. Ana hedef bekletme sürücüsünü seçin. Yeniden çalışma ilkesi otomatik olarak seçilir.
6. Yeniden korumaya başlamak için **Tamam**’a tıklayın. Sanal makineyi Azure’dan şirket içi siteye çoğaltan bir iş başlar. **İşler** sekmesinde ilerleme durumunu izleyebilirsiniz.
7. **Çoğaltılan Öğeler**’de VM’nin durumu **Korumalı** olarak değiştiğinde makine şirket içine yük devretmeye hazırdır.

> [!NOTE]
> Azure VM var olan bir şirket içi VM’ye veya alternatif bir konuma kurtarılabilir. Daha fazla bilgi için [bu makaleyi](concepts-types-of-failback.md) okuyun.

## <a name="run-a-failover-from-azure-to-on-premises"></a>Azure’dan şirket içine yük devretme çalıştırma

Yeniden şirket içine çoğaltma yapmak için bir yeniden çalıştırma ilkesi kullanılır. Bu ilke, Azure’a çoğaltma için bir çoğaltma ilkesi oluşturduğunuzda otomatik olarak yeniden oluşturulur:

- İlke, yapılandırma sunucusu ile otomatik olarak ilişkilendirilir.
- İlke değiştirilemez.
- İlke değerleri:
    - RPO eşiği = 15 dakika
    - Kurtarma noktası bekletme = 24 saat
    - Uygulamayla tutarlı anlık görüntü sıklığı = 60 dakika

Yük devretmeyi şu şekilde çalıştırın:

1. **Çoğaltılan Öğeler** sayfasında makineye sağ tıklayın ve **Yük Devretme** seçeneğini belirtin.
2. **Yük Devretmeyi Onayla** bölümünde yük devretme yönünün Azure’dan olduğundan emin olun.
    ![failover-direction](media/vmware-azure-tutorial-failover-failback/failover-direction.PNG)
3. Yük devretme için kullanmak istediğiniz kurtarma noktasını seçin. Uygulamayla tutarlı kurtarma noktası, en yakın zaman noktasından öncedir ve bazı verilerin kaybolmasına neden olur.

    >[!WARNING]
    >Yük devretme çalıştığında Site Recovery, Azure VM’lerini kapatır ve şirket içi VM’yi başlatır. Hizmet bir süre kapalı kalacağından uygun bir zaman seçin.

4. İşin ilerleme durumu **Kurtarma Hizmetleri Kasası** > **İzleme ve Raporlar** > **Site Kurtarma İşleri** bölümlerinden takip edilebilir.
5. Yük devretme bittiğinde sanal makineye sağ tıklayın ve **İşle**’ye tıklayın. Bu, Azure VM’lerini kaldıran bir işi tetikler.
6. Azure VM’lerinin beklenen şekilde kapatıldığından emin olun.

## <a name="reprotect-on-premises-machines-to-azure"></a>Şirket içi makineleri Azure’da yeniden koruma

Veriler artık şirket içi sitenizde tekrar yerleştirilir, ancak Azure’a çoğaltılmaz. Tekrar Azure’a çoğaltmaya başlamak için şunları yapabilirsiniz:

1. Kasada **Korumalı öğeler** >**Çoğaltılan öğeler**’e gidin, yükün devredildiği VM’yi seçin ve **Yeniden Koru**’ya tıklayın.
2. Çoğaltılan verileri Azure’a göndermek için kullanılacak işlem sunucusunu seçin ve **Tamam**’a tıklayın.

Yeniden koruma bittikten sonra VM tekrar Azure’a çoğaltır ve yük devretmeyi gerektiği gibi çalıştırabilirsiniz.
