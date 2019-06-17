---
title: Yük devretme ve yeniden fiziksel sunucuları Azure Site Recovery ile olağanüstü durum kurtarma için döndürme | Microsoft Docs
description: Fiziksel sunucuları azure'a yük devretme ve şirket içi sitede Azure Site Recovery ile olağanüstü durum kurtarma için geri yüklenemedi hakkında bilgi edinin
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 05/30/2019
ms.author: raynew
ms.openlocfilehash: 14fa5822c575f2d2d60a956263cf916ee8f9bb4d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66400032"
---
# <a name="fail-over-and-fail-back-physical-servers-replicated-to-azure"></a>Yük devretme ve yeniden fiziksel sunucuları Azure'a çoğaltılan döndürme

Bu öğreticide, bir fiziksel sunucudan azure'a yük devretme işlemini açıklamaktadır. Yük devrettikten sonra uygun olduğunda şirket içi sitenize geri sunucunun başarısız.

## <a name="preparing-for-failover-and-failback"></a>Yük devretme ve yeniden çalışma için hazırlanma

Site Recovery kullanılarak Azure'da çoğaltılmış fiziksel sunucularda, VMware Vm'lerini ' yalnızca başarısız olabilir. Yeniden çalışma için bir VMware altyapınız olmalıdır.

Yük devretme ve yeniden çalışma dört aşamalıdır:

1. **Azure'a yük devretme**: Makineleri şirket içi siteden Azure'a yük devretme.
2. **Azure Vm'lerini yeniden koruma**: Böylece geri şirket içi VMware Vm'lerine çoğaltılmaya başlatmaları Azure Vm'lerini yeniden koruyun.
3. **Şirket içine yük devretme**: Azure'dan yeniden çalışma için bir yük devretme çalıştırın.
4. **Yeniden koruma şirket içi Vm'leri**: Veri çalıştıktan sonra böylece bunlar Azure'a çoğaltmaya başlamak başlamaları için şirket içi VMware Vm'lerini yeniden koruyun.

## <a name="verify-server-properties"></a>Sunucu özelliklerini doğrulayın

Sunucu özelliklerini doğrulayın ve ile uyumlu olduğundan emin olun [Azure gereksinimleri](vmware-physical-azure-support-matrix.md#replicated-machines) Azure Vm'leri için.

1. İçinde **korunan öğeler**, tıklayın **çoğaltılan öğeler**, makineyi seçin.

2. İçinde **çoğaltılan öğe** bölmesinde makine bilgileri, sistem durumu özetini yoktur ve son kullanılabilir kurtarma noktası. Daha fazla ayrıntı görüntülemek için **Özellikler**’e tıklayın.
3. İçinde **işlem ve ağ**, değiştirebileceğiniz Azure ad, kaynak grubunu, hedef boyutunu, [kullanılabilirlik kümesi](../virtual-machines/windows/tutorial-availability-sets.md)ve yönetilen disk ayarlarını
4. Ağ ayalarını, yük devretmeden sonra Azure VM’nin yerleştirileceği ağ/alt ağ ve VM’ye atanacak IP adresi dahil olmak üzere görüntüleyebilir ve değiştirebilirsiniz.
5. İçinde **diskleri**, makine işletim sistemi ve veri diskleri hakkında bilgi bulabilirsiniz.

## <a name="run-a-failover-to-azure"></a>Azure'a yük devretme işlemini çalıştırma

1. **Ayarlar** > **Çoğaltılan öğeler** bölümünde makine > **Yük devretme**’ye tıklayın.
2. **Yük devretme**’de yük devretmenin yapılacağı bir **Kurtarma Noktası** seçin. Şu seçeneklerden birini kullanabilirsiniz:
   - **En son**: Bu seçenek öncelikle Site Recovery'ye gönderilen tüm verileri işler. Yük devretmeden sonra oluşturulan Azure VM, yük devretme tetiklendiğinde Site Recovery’ye çoğaltılan tüm verilere sahip olduğundan en düşük RPO (Kurtarma Noktası Hedefi) sağlanır.
   - **En son işlenen**: Bu seçeneği, makine Site Recovery tarafından işlenen en son kurtarma noktasına devreder. İşlenmemiş verileri işlemek için zaman harcanmadığından bu seçenekte düşük bir RTO (Kurtarma Süresi Hedefi) sağlanır.
   - **Uygulamayla tutarlı olan sonuncu**: Bu seçenek makinenin Site Recovery tarafından işlenen en son uygulamayla tutarlı kurtarma noktasına devreder.
   - **Özel**: Bir kurtarma noktası belirtin.

3. Seçin **yük devretmeye başlamadan önce makineyi Kapat** Site Recovery, yük devretmeyi tetiklemeden önce kaynak makineyi kapatmanız denemek istiyorsanız. Kapatma işlemi başarısız olsa bile yük devretme devam eder. Yük devretme işlemini **İşler** sayfasında takip edebilirsiniz.
4. Azure VM’ye bağlanmaya hazırsanız, yük devretmeden sonra VM’yi doğrulamak için bağlanın.
5. Doğruladıktan sonra yük devretmeyi **Yürütün**. Bu, tüm kullanılabilir kurtarma noktalarını siler.

> [!WARNING]
> Devam eden bir yük devretme işlemini iptal etmeyin. Yük devretme başlamadan önce makine çoğaltmayı durdurur. Yük devretmeyi iptal ederseniz, onu durdurur, ancak makine yeniden çoğaltılmaz.
> Fiziksel sunucular için ek yük devretme işlemi, tamamlanması yaklaşık sekiz ila on dakika sürebilir.

## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Yük devretmeden sonra Azure VM'lerine bağlanmak için hazırlık yapma

Yük devretme sonrasında RDP/SSH kullanarak Azure VM'lerine bağlanmak istiyorsanız [buradaki](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover) tabloda özetlenen gereksinimleri izleyin.

Yük devretme sonrasında karşılaştığınız bağlantı sorunlarını gidermek için [burada](site-recovery-failover-to-azure-troubleshoot.md) anlatılan adımları izleyin.

## <a name="create-a-process-server-in-azure"></a>Azure’da bir işlem sunucusu oluşturma

İşlem sunucusu verileri Azure VM’den alır ve şirket içi siteye gönderir. Korumalı makine ve işlem sunucusu arasında düşük gecikmeli bir ağ gereklidir.

- Bir Azure ExpressRoute bağlantınız varsa test etmek için yapılandırma sunucusuna otomatik olarak kurulan şirket içi işlem sunucusunu kullanabilirsiniz.
- Bir VPN bağlantınız varsa veya üretim ortamında yeniden çalışma gerçekleştiriyorsanız, yeniden çalışma için Azure tabanlı işlem sunucusu olarak bir Azure VM’si ayarlamanız gerekir.
- Bölümündeki yönergeleri [bu makalede](vmware-azure-set-up-process-server-azure.md) azure'da bir işlem sunucusu ayarlamak için.

## <a name="configure-the-master-target-server"></a>Ana hedef sunucuyu yapılandırma

Varsayılan olarak, ana hedef sunucusu yeniden çalışma verilerini alır. Bu, şirket içi yapılandırma sunucusu üzerinde çalışır.

- İçin yeniden çalışma özelliğini VMware VM VMware vCenter Server tarafından yönetilen bir ESXi ana bilgisayarı üzerindeyse, ana hedef sunucusu çoğaltılan verileri VM disklerine yazmak VM veri deposuna (VMDK) erişiminiz olması gerekir. VM veri deposunun, ana hedefin ana bilgisayarına okuma/yazma erişimi ile bağlı olduğundan emin olun.
- Site Recovery hizmeti bir vCenter sunucusu tarafından yönetilmeyen bir ESXi konağı yeniden koruma sırasında yeni bir sanal makine oluşturursa. VM VM ana hedefi oluşturduğunuz ESX konağında oluşturulur. VM’nin sabit diski, üzerinde ana hedef sunucunun çalıştığı ana bilgisayar tarafından erişilebilen bir veri deposunda olmalıdır.
- Yeniden çalışma özelliğini fiziksel makineler için makineyi yeniden korumadan önce üzerinde ana hedef sunucusu, çalıştığı ana bilgisayarı bulma tamamlamanız gerekir.
- Şirket içi VM yeniden çalışma için zaten varsa, başka bir seçenek, bir yeniden çalışma gerçekleştirmeden önce VM'yi silmektir oluşturmaktır. Yeniden çalışma, ana hedef ESX sunucusu ile aynı ana bilgisayarda yeni bir VM oluşturur. Farklı bir konuma yeniden çalışma gerçekleştirdiğinizde, veriler şirket içi ana hedef sunucu tarafından kullanılan aynı veri deposuna ve aynı ESX ana bilgisayarına kurtarılır.
- Ana hedef sunucuda Storage vMotion kullanamazsınız. Kullandığınız takdirde diskler işlem için kullanılabilir olmadığından yeniden çalışma gerçekleştirilemez. Ana hedef sunucuları vMotion listenizin dışında tutun.

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
