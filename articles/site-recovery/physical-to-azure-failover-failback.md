---
title: "Yük devri ve Azure Site Recovery ile çoğaltılan geri fiziksel sunucuları başarısız | Microsoft Docs"
description: "Fiziksel sunucuları azure'a yük devri ve Azure Site Recovery ile şirket içi siteye geri başarısız hakkında bilgi edinin"
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 02/22/2018
ms.author: raynew
ms.openlocfilehash: bbad2a0ea1a58834eaf32e0d3286f6e8a794d364
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2018
---
# <a name="fail-over-and-fail-back-physical-servers-replicated-to-azure"></a>Yük devri ve geri fiziksel sunucuları Azure'a kopyalanan başarısız

Bu öğretici, Azure fiziksel bir sunucuya yük devri açıklar. Devredilir sonra kullanılabilir olduğunda, şirket içi sitenize sunucunun başarısız. 

## <a name="preparing-for-failover-and-failback"></a>Yük devretme ve yeniden çalışma için hazırlanma

Site RECOVERY'yi kullanarak Azure'a çoğaltılan fiziksel sunucuların yalnızca geri VMware Vm'lerini başarısız olabilir. Yeniden çalışmak için bir VMware altyapı gerekir. 

Yük devretme ve yeniden çalışma dört aşamalıdır:

1. **Azure’a yük devretme**: Makineleri şirket içi siteden Azure’a devredin.
2. **Azure Vm'leri koruyun**: şirket içi VMware sanal yeniden başlatmaları böylece Azure Vm'leri koruyun.
3. **Şirket içine yük devretme**: Azure’dan yeniden çalıştırmak için bir yük devretme işlemi çalıştırın.
4. **Yeniden koruma şirket içi sanal makineleri**: veri geri başarısız oldu sonra geri için başarısız şirket içi VMware sanal makinelerini Azure'a çoğaltma başlatılması koruyun.

## <a name="verify-server-properties"></a>Sunucu özelliklerini doğrulayın

Sunucu özelliklerini doğrulayın ve ile uyumlu olduğundan emin olun [Azure gereksinimleri](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) Azure VM'ler için.

1. İçinde **korunan öğeler**, tıklatın **çoğaltılan öğeler**ve makineyi seçin.

2. İçinde **yinelenmiş öğesi** bölmesinde makine bilgilerini, sistem durumu özetini yoktur ve en son kullanılabilir kurtarma noktaları. Daha fazla ayrıntı görüntülemek için **Özellikler**’e tıklayın.
3. **İşlem ve Ağ** bölümünde Azure adını, kaynak grubunu, hedef boyutunu, [kullanılabilirlik kümesini](../virtual-machines/windows/tutorial-availability-sets.md) ve [yönetilen disk ayarlarını](#managed-disk-considerations) değiştirebilirsiniz
4. Ağ ayalarını, yük devretmeden sonra Azure VM’nin yerleştirileceği ağ/alt ağ ve VM’ye atanacak IP adresi dahil olmak üzere görüntüleyebilir ve değiştirebilirsiniz.
5. İçinde **diskleri**, makine işletim sistemi ve veri diskleri hakkındaki bilgileri görebilirsiniz.

## <a name="run-a-failover-to-azure"></a>Azure'a yük devretme işlemini çalıştırma

1. İçinde **ayarları** > **öğeleri çoğaltılan** makineye tıklayın > **yük devretme**.
2. **Yük devretme**’de yük devretmenin yapılacağı bir **Kurtarma Noktası** seçin. Aşağıdaki seçeneklerden birini kullanabilirsiniz:
   - **En son** (varsayılan): Bu seçenekte öncelikle Site Recovery’ye gönderilen tüm veriler işlenir. Yük devretmeden sonra oluşturulan Azure VM, yük devretme tetiklendiğinde Site Recovery’ye çoğaltılan tüm verilere sahip olduğundan en düşük RPO (Kurtarma Noktası Hedefi) sağlanır.
   - **En son işlenen**: Site Recovery tarafından işlenen en son kurtarma noktası makineye bu seçenek yöneltilir. İşlenmemiş verileri işlemek için zaman harcanmadığından bu seçenekte düşük bir RTO (Kurtarma Süresi Hedefi) sağlanır.
   - **Son uygulama tutarlı**: Site Recovery tarafından işlenen son uygulamayla tutarlı kurtarma noktası makineye bu seçenek yöneltilir.
   - **Özel**: Bir kurtarma noktası belirtin.

3. Seçin **yük devretme işlemine başlamadan önce makineyi kapatın** yük devretme tetiklemeden önce kaynak makine kapatılamıyor denemek için Site Recovery istiyorsanız. Kapatma işlemi başarısız olsa bile yük devretme devam eder. Yük devretme işlemini **İşler** sayfasında takip edebilirsiniz.
4. Azure VM’ye bağlanmaya hazırsanız, yük devretmeden sonra VM’yi doğrulamak için bağlanın.
5. Doğruladıktan sonra yük devretmeyi **Yürütün**. Bu, tüm kullanılabilir kurtarma noktalarını siler.

> [!WARNING]
> Bir yük devretme devam ediyor iptal etme. Yük devretme başlamadan önce makine çoğaltma durur. Yük devretme iptal ederseniz, onu durdurur, ancak makine yeniden çoğaltma olmaz.
> Fiziksel sunucuları için ek yük devretme işlemi tamamlamak için yaklaşık sekiz ile on dakika sürebilir. 

## <a name="create-a-process-server-in-azure"></a>Azure’da bir işlem sunucusu oluşturma

İşlem sunucusu verileri Azure VM’den alır ve şirket içi siteye gönderir. İşlem sunucusu ve korumalı makine arasında düşük gecikme süreli ağ gereklidir.

- Bir Azure ExpressRoute bağlantınız varsa test etmek için yapılandırma sunucusuna otomatik olarak kurulan şirket içi işlem sunucusunu kullanabilirsiniz.
- Bir VPN bağlantınız varsa veya üretim ortamında yeniden çalışma gerçekleştiriyorsanız, yeniden çalışma için Azure tabanlı işlem sunucusu olarak bir Azure VM’si ayarlamanız gerekir.
- ' Ndaki yönergeleri izleyin [bu makalede](site-recovery-vmware-setup-azure-ps-resource-manager.md) bir işlem sunucusu azure'da ayarlamak için.

## <a name="configure-the-master-target-server"></a>Ana hedef sunucuyu yapılandırma

Varsayılan olarak, ana hedef sunucusu yeniden çalışma verileri alır. Şirket içi yapılandırma sunucusunda çalışır.

- İçin size geri dönecek VMware VM VMware vCenter Server tarafından yönetilen bir ESXi ana bilgisayarda ise, ana hedef sunucusu çoğaltılan veriler için VM diskleri yazmak için VM'nin veri deposu (VMDK) erişimi olmalıdır. VM veri deposunun, ana hedefin ana bilgisayarına okuma/yazma erişimi ile bağlı olduğundan emin olun.
- Site Recovery hizmeti bir vCenter sunucusu tarafından yönetilmiyor ESXi ana yükü sırasında yeni bir VM oluşturursa. VM ana hedef VM oluşturma ESX ana bilgisayarda oluşturulur. VM’nin sabit diski, üzerinde ana hedef sunucunun çalıştığı ana bilgisayar tarafından erişilebilen bir veri deposunda olmalıdır.
- Geri dönecek fiziksel makineler için bulma makine koruyun önce ana hedef sunucusu, çalıştığı konağının tamamlamanız gerekir.
- Şirket içi VM yeniden çalışma için zaten varsa, başka bir seçenek, bir yeniden çalışma yapmadan önce silme sağlar. Yeniden çalışma, ana hedef ESX sunucusu ile aynı ana bilgisayarda yeni bir VM oluşturur. Farklı bir konuma yeniden çalışma gerçekleştirdiğinizde, veriler şirket içi ana hedef sunucu tarafından kullanılan aynı veri deposuna ve aynı ESX ana bilgisayarına kurtarılır.
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

