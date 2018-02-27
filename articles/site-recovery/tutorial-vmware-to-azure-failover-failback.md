---
title: "VMware VM’lerde ve çoğaltılmış fiziksel sunucularda Azure Site Recovery’ye yük devretme ve yeniden çalışma | Microsoft Docs"
description: "Azure Site Recovery ile VMware VM’lerde ve fiziksel sunucularda Azure’a yük devretme ve şirket içi sitede yeniden çalışma hakkında bilgi edinin"
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 02/07/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: f074312ecee39d4b3022df64b51aadd2bb8f968c
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
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

Bu, serideki beşinci öğreticidir. Bu öğreticide, önceki öğreticilerdeki adımları zaten tamamladığınız varsayılır.

1. [Azure’u hazırlama](tutorial-prepare-azure.md)
2. [Şirket içi VMware’leri hazırlama](tutorial-prepare-on-premises-vmware.md)
3. [Olağanüstü durum kurtarmayı ayarlama](tutorial-vmware-to-azure.md)
4. [Olağanüstü durum kurtarma tatbikatı çalıştırma](tutorial-dr-drill-azure.md)

## <a name="preparing-for-failover-and-failback"></a>Yük devretme ve yeniden çalışma için hazırlanma

VM’de anlık görüntü olmadığından emin olun. Şirket içi VM yeniden koruma sırasında kapatılır.
Bu işlem, çoğaltma sırasında veri tutarlığını sağlar. Yeniden koruma bittikten sonra VM’yi açmayın. Çoğaltma grubunu yeniden korumak için aynı hedef sunucuyu kullanın. Çoğaltma grubunu yeniden korumak için farklı bir ana hedef sunucu kullanırsanız, sunucu ortak bir zaman noktası sağlayamaz.

Yük devretme ve yeniden çalışma dört aşamalıdır:

1. **Azure’a yük devretme**: Makineleri şirket içi siteden Azure’a devredin.
2. **Azure VM’lerini yeniden koruma**: Şirket içi VMware VM’lerine çoğaltılmaya başlamaları için Azure VM’lerini yeniden koruyun.
3. **Şirket içine yük devretme**: Azure’dan yeniden çalıştırmak için bir yük devretme işlemi çalıştırın.
4. **Şirket içi VM’leri yeniden koruma**: Veriler yeniden çalıştıktan sonra Azure’a çoğaltılmaya başlamaları için yeniden çalışma uyguladığınız şirket içi VM’leri yeniden koruyun.

## <a name="verify-vm-properties"></a>VM özelliklerini doğrulama

VM özelliklerini doğrulayın ve VM’nin [Azure gereksinimleriyle](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) uyumlu olduğundan emin olun.

1. **Korunan Öğeler**’de **Çoğaltılan Öğeler** > VM seçeneğine tıklayın.

2. **Çoğaltılan öğe** bölmesinde VM bilgileri ile sistem durumunun bir özeti ve kullanılabilen son kurtarma noktaları yer alır. Daha fazla ayrıntı görüntülemek için **Özellikler**’e tıklayın.

3. **İşlem ve Ağ** bölümünde Azure adını, kaynak grubunu, hedef boyutunu, [kullanılabilirlik kümesini](../virtual-machines/windows/tutorial-availability-sets.md) ve [yönetilen disk ayarlarını](#managed-disk-considerations) değiştirebilirsiniz

4. Ağ ayalarını, yük devretmeden sonra Azure VM’nin yerleştirileceği ağ/alt ağ ve VM’ye atanacak IP adresi dahil olmak üzere görüntüleyebilir ve değiştirebilirsiniz.

5. **Diskler** bölümünde VM’deki işletim sistemi ve veri diskleriyle ilgili bilgileri görüntüleyebilirsiniz.

## <a name="run-a-failover-to-azure"></a>Azure'a yük devretme işlemini çalıştırma

1. **Ayarlar** > **Çoğaltılan öğeler** bölümünde VM > **Yük devretme**’ye tıklayın.

2. **Yük devretme**’de yük devretmenin yapılacağı bir **Kurtarma Noktası** seçin. Aşağıdaki seçeneklerden birini kullanabilirsiniz:
   - **En son** (varsayılan): Bu seçenekte öncelikle Site Recovery’ye gönderilen tüm veriler işlenir. Yük devretmeden sonra oluşturulan Azure VM, yük devretme tetiklendiğinde Site Recovery’ye çoğaltılan tüm verilere sahip olduğundan en düşük RPO (Kurtarma Noktası Hedefi) sağlanır.
   - **En son işlenen**: Bu seçenekte VM yükü, Site Recovery tarafından işlenen en son kurtarma noktasına devredilir. İşlenmemiş verileri işlemek için zaman harcanmadığından bu seçenekte düşük bir RTO (Kurtarma Süresi Hedefi) sağlanır.
   - **En son uygulamayla tutarlı**: Bu seçenekte VM yükü, Site Recovery tarafından işlenen, uygulamayla tutarlı en son kurtarma noktasına devredilir.
   - **Özel**: Bir kurtarma noktası belirtin.

3. Yük devretmeyi tetiklemeden önce kaynak sanal makineleri kapatmayı denemek için **Yük devretmeye başlamadan önce makineyi kapat** seçeneğini belirleyin. Kapatma işlemi başarısız olsa bile yük devretme devam eder. Yük devretme işlemini **İşler** sayfasında takip edebilirsiniz.

4. Azure VM’ye bağlanmaya hazırsanız, yük devretmeden sonra VM’yi doğrulamak için bağlanın.

5. Doğruladıktan sonra yük devretmeyi **Yürütün**. Bu, tüm kullanılabilir kurtarma noktalarını siler.

> [!WARNING]
> **Devam eden yük devretme işlemini iptal etmeyin**: Yük devretme başlatılmadan önce VM çoğaltma durdurulur.
> Devam eden bir yük devretme işlemini iptal ederseniz yük devretme durdurulur, ancak VM yeniden çoğaltılamaz.

Bazı senaryolarda yük devretme için sekiz ila on dakikada tamamlanan ek işlem gerekebilir. Fiziksel sunucularda, VMware Linux makinelerinde, DHCP hizmeti etkinleştirilmemiş VMware VM’lerinde ve storvsc, vmbus, storflt, intelide, atapi önyükleme sürücülerine sahip olmayan VMware VM’lerinde uzun yük devretme sınama süreleriyle karşılaşabilirsiniz.

## <a name="create-a-process-server-in-azure"></a>Azure’da bir işlem sunucusu oluşturma

İşlem sunucusu verileri Azure VM’den alır ve şirket içi siteye gönderir. İşlem sunucusu ve korunan VM arasında düşük gecikme süresine sahip bir ağ bağlantısı olması gerekir.

- Bir Azure ExpressRoute bağlantınız varsa test etmek için yapılandırma sunucusuna otomatik olarak kurulan şirket içi işlem sunucusunu kullanabilirsiniz.
- Bir VPN bağlantınız varsa veya üretim ortamında yeniden çalışma gerçekleştiriyorsanız, yeniden çalışma için Azure tabanlı işlem sunucusu olarak bir Azure VM’si ayarlamanız gerekir.
- Azure’da bir işlem sunucusu ayarlamak için [bu makaledeki](site-recovery-vmware-setup-azure-ps-resource-manager.md) yönergeleri uygulayın.

## <a name="configure-the-master-target-server"></a>Ana hedef sunucuyu yapılandırma

Ana hedef sunucu varsayılan olarak şirket için yapılandırma sunucusunda çalışır. Bu öğreticinin amacına uygun olarak varsayılan ana sunucuyu kullanıyoruz. Ana hedef sunucu yeniden çalışma verilerini alır.

VM, vCenter sunucusu tarafından yönetilen bir ESXi ana bilgisayarı üzerindeyse, çoğaltılan verileri VM disklerine yazmak için ana hedef sunucunun VM veri deposuna (VMDK) erişimi olması gerekir. VM veri deposunun, ana hedefin ana bilgisayarına okuma/yazma erişimi ile bağlı olduğundan emin olun.

VM, vCenter sunucusu tarafından yönetilmeyen bir ESXi üzerindeyse, Site Recovery hizmeti yeniden koruma sırasında yeni bir VM oluşturur. VM, ana hedefi oluşturduğunuz ESX ana bilgisayarı üzerinde oluşturulur.
VM’nin sabit diski, üzerinde ana hedef sunucunun çalıştığı ana bilgisayar tarafından erişilebilen bir veri deposunda olmalıdır.

VM, vCenter kullanmıyorsa makineyi yeniden korumadan önce, üzerinde ana hedef sunucunun çalıştığı ana bilgisayarı bulma işlemini tamamlamanız gerekir. Bu, fiziksel sunucuları yeniden çalıştırma işlemi için de geçerlidir. Başka bir seçenek de şirket içi VM varsa yeniden çalışma gerçekleştirmeden önce VM’yi silmektir. Yeniden çalışma, ana hedef ESX sunucusu ile aynı ana bilgisayarda yeni bir VM oluşturur. Farklı bir konuma yeniden çalışma gerçekleştirdiğinizde, veriler şirket içi ana hedef sunucu tarafından kullanılan aynı veri deposuna ve aynı ESX ana bilgisayarına kurtarılır.

Ana hedef sunucuda Storage vMotion kullanamazsınız. Kullandığınız takdirde diskler işlem için kullanılabilir olmadığından yeniden çalışma gerçekleştirilemez. Ana hedef sunucuları vMotion listenizin dışında tutun.

## <a name="reprotect-azure-vms"></a>Azure VM’lerini yeniden koruma

Bu yordamda şirket içi VM’nin kullanılabilir olmadığı ve farklı bir konuma yeniden koruma gerçekleştirdiğiniz varsayılır.

1. **Ayarlar** > **Çoğaltılan öğeler** bölümünde yük devretme gerçekleştirilen VM’ye sağ tıklayın ve **Yeniden Koru** seçeneğini belirleyin.
2. **Yeniden koru** bölümünde **Azure’dan Şirket içine** seçeneğinin belirlendiğinden emin olun.
3. Şirket içi ana hedef sunucuyu ve işlem sunucusunu belirtin.

4. **Veri deposu**’nda şirket içi diskleri kurtarmak istediğiniz ana hedef veri deposunu seçin. Bu seçeneği, şirket içi VM silindiğinde ve yeni diskler oluşturmanız gerektiğinde kullanın. Diskler zaten varsa, ancak bir değer belirtmeniz gerekiyorsa bu ayarlar yok sayılır.
5. Ana hedef bekletme sürücüsünü seçin. Yeniden çalışma ilkesi otomatik olarak seçilir.
6. Yeniden korumaya başlamak için **Tamam**’a tıklayın. Sanal makineyi Azure’dan şirket içi siteye çoğaltan bir iş başlar. **İşler** sekmesinde ilerleme durumunu izleyebilirsiniz.

> [!NOTE]
> Azure VM’sini mevcut bir şirket içi VM’ye kurtarmak istiyorsanız, şirket içi sanal makinenin veri deposunu okuma/yazma erişimiyle ana hedef sunucunun ESXi ana bilgisayarına bağlayın.


## <a name="run-a-failover-from-azure-to-on-premises"></a>Azure’dan şirket içine yük devretme çalıştırma

Yeniden şirket içine çoğaltma yapmak için bir yeniden çalıştırma ilkesi kullanılır. Bu ilke, Azure’a çoğaltma için bir çoğaltma ilkesi oluşturduğunuzda otomatik olarak yeniden oluşturulur:

- İlke, yapılandırma sunucusu ile otomatik olarak ilişkilendirilir.
- İlke değiştirilemez.
- İlke değerleri:
    - RPO eşiği = 15 dakika
    - Kurtarma noktası bekletme = 24 saat
    - Uygulamayla tutarlı anlık görüntü sıklığı = 60 dakika

Yük devretmeyi şu şekilde çalıştırın:

1. **Çoğaltılan Öğeler** sayfasında makineye sağ tıklayın ve **Planlanmamış Yük Devretme** seçeneğini belirleyin.
2. **Yük Devretmeyi Onayla** bölümünde yük devretme yönünün Azure’dan olduğundan emin olun.

3. Yük devretme için kullanmak istediğiniz kurtarma noktasını seçin. Uygulamayla tutarlı kurtarma noktası, en yakın zaman noktasından öncedir ve bazı verilerin kaybolmasına neden olur. Yük devretme çalıştığında Site Recovery, Azure VM’lerini kapatır ve şirket içi VM’yi başlatır. Hizmet bir süre kapalı kalacağından uygun bir zaman seçin.
4. Makineye sağ tıklayın ve **Yürüt**’e tıklayın. Bu, Azure VM’lerini kaldıran bir işi tetikler.
5. Azure VM’lerinin beklenen şekilde kapatıldığından emin olun.


## <a name="reprotect-on-premises-machines-to-azure"></a>Şirket içi makineleri Azure’da yeniden koruma

Veriler artık şirket içi sitenizde tekrar yerleştirilir, ancak Azure’a çoğaltılmaz. Tekrar Azure’a çoğaltmaya başlamak için şunları yapabilirsiniz:

1. Kasa > **Ayarlar** >**Çoğaltılan Öğeler** bölümünde yeniden çalışan VM’leri seçin ve **Yeniden Koru**’ya tıklayın.
2. Çoğaltılan verileri Azure’a göndermek için kullanılan işlem sunucusunu seçin ve **Tamam**’a tıklayın.

Yeniden koruma bittikten sonra VM tekrar Azure’a çoğaltır ve yük devretmeyi gerektiği gibi çalıştırabilirsiniz.
