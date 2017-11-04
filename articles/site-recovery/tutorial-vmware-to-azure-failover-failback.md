---
title: "Yük devri ve başarısız geri VMware Vm'lerini ve fiziksel sunucuları Azure Site Recovery ile çoğaltılan | Microsoft Docs"
description: "VMware Vm'lerini ve fiziksel sunucuları azure'a yük devri ve Azure Site Recovery ile şirket içi siteye geri başarısız hakkında bilgi edinin"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/01/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 28a14a9b28dfe9c2014add9b9f691bce6ba91a4c
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="fail-over-and-fail-back-vmware-vms-and-physical-servers-replicated-to-azure"></a>Yük devri ve başarısız geri VMware Vm'lerini ve fiziksel sunucuları Azure'a kopyalanan

Bu öğretici, Azure'da bir VMware VM üzerinden vermesine açıklar. Devredilir sonra kullanılabilir olduğunda, şirket içi sitenize başarısız. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * VMware VM özelliklerini denetlemek için Azure gereksinimlerine uygun doğrulayın
> * Azure'a yük devretme gerçekleştirme
> * İşlem sunucusu ve ana hedef sunucusu yeniden çalışma için oluşturma
> * Şirket içi siteye Azure Vm'leri koruyun
> * Azure'dan şirket içine yük devri
> * Azure'a çoğaltma yeniden başlatmak için şirket içi VM'ler koruyun

Beşinci öğretici serisinde budur. Bu öğreticinin önceki eğitimlerine görevleri önceden tamamlamış varsayar.

1. [Azure’u hazırlama](tutorial-prepare-azure.md)
2. [Şirket içi VMware’leri hazırlama](tutorial-prepare-on-premises-vmware.md)
3. [Olağanüstü durum kurtarmayı ayarlama](tutorial-vmware-to-azure.md)
4. [Olağanüstü durum kurtarma tatbikatı çalıştırma](tutorial-dr-drill-azure.md)

## <a name="preparing-for-failover-and-failback"></a>Yük devretme ve yeniden çalışma için hazırlanılıyor

VM anlık görüntü yok olduğundan emin olun. Şirket içi VM yükü sırasında devre dışı bırakılır.
Bu, çoğaltma sırasında veri tutarlılığını sağlamaya yardımcı olur. Yükü bittikten sonra VM kapatmayın. Aynı ana hedef sunucusunda bir çoğaltma grubunu yeniden korumak için kullanın. Bir çoğaltma grubunu yeniden korumak için farklı bir ana hedef sunucusu kullanıyorsanız, sunucunun ortak bir noktaya zamanında sağlayamaz.

Yük devretme ve yeniden çalışma dört aşama bulunur:

1. **Azure'a yük**: başarısız makineler üzerinden şirket içi sitesinden Azure'a.
2. **Azure Vm'leri koruyun**: şirket içi VMware sanal yeniden başlatmaları böylece Azure Vm'leri koruyun.
3. **Şirket içi yük devri**: Azure'dan başarısız olmasına bağlı olarak, yük devretme gerçekleştirme.
4. **Yeniden koruma şirket içi sanal makineleri**: veri geri başarısız oldu sonra böylece bunlar Azure'a çoğaltma işlemi başlatma geri için başarısız şirket içi sanal makineleri yeniden koruyun.

## <a name="verify-vm-properties"></a>VM özelliklerini doğrulayın

VM özelliklerini doğrulayın ve VM ile uyumlu olduğundan emin olun [Azure gereksinimleri](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

1. İçinde **korunan öğeler**, tıklatın **çoğaltılan öğeler** > VM.

2. İçinde **yinelenmiş öğesi** bölmesinde VM bilgileri, sistem durumu özetini yoktur ve en son kullanılabilir kurtarma noktaları. Tıklatın **özellikleri** daha fazla ayrıntı görüntülemek için.

3. İçinde **işlem ve ağ**, değiştirebilirsiniz Azure ad, kaynak grubu, hedef boyutu [kullanılabilirlik kümesi](../virtual-machines/windows/tutorial-availability-sets.md), ve [disk ayarları yönetilen](#managed-disk-considerations)

4. Görüntüleyin ve hangi Azure VM yük devretme ve kendisine atanmış IP adresi sonra yer alacağı ağını/alt ağını dahil olmak üzere ağ ayarlarını değiştirin.

5. İçinde **diskleri**, VM işletim sistemi ve veri diskleri hakkındaki bilgileri görebilirsiniz.

## <a name="run-a-failover-to-azure"></a>Azure'a yük devretme gerçekleştirme

1. İçinde **ayarları** > **öğeleri çoğaltılan** VM tıklayın > **yük devretme**.

2. İçinde **yük devretme** seçin bir **kurtarma noktası** devretmesini. Aşağıdaki seçeneklerden birini kullanabilirsiniz:
   - **En son** (varsayılan): Bu seçenek ilk Site Recovery hizmetine gönderilen tüm veriler işler. Yük devretme yük devretme tetiklendiğinde Site Recovery çoğaltılan tüm verilerini sahip olduktan sonra Azure VM oluşturduğundan düşük RPO (kurtarma noktası hedefi) sağlar.
   - **En son işlenen**: Bu seçenek, Site Recovery tarafından işlenen en son kurtarma noktası VM'ye yöneltilir. Bu seçenek, hiçbir zaman işlenmemiş verileri işlerken harcanan çünkü düşük RTO (Kurtarma süresi hedefi) sağlar.
   - **Son uygulama tutarlı**: Bu seçenek, Site Recovery tarafından işlenen son uygulamayla tutarlı kurtarma noktası VM'ye yöneltilir.
   - **Özel**: bir kurtarma noktası belirtin.

3. Seçin **yük devretme işlemine başlamadan önce makineyi kapatın** yük devretme tetiklemeden önce kaynak sanal makinelerin bir kapatma yapma girişiminde. Kapatma başarısız olsa bile yük devretme devam eder. Yük devretme işleminin ilerleyişini izleyin **işleri** sayfası.

4. Azure VM'ye bağlanabilmeniz için hazırlanan, yük devretme sonrasında doğrulamak için Bağlan.

5. Doğruladıktan sonra **yürütme** yük devretme. Bu, tüm kullanılabilir kurtarma noktalarını siler.

> [!WARNING]
> **Bir yük devretme devam ediyor iptal etme**: yük devretme başlatılmadan önce VM çoğaltma durduruldu.
> Bir yük devretme devam ediyor, yük devretme durduruyor, iptal ancak VM yeniden çoğaltma olmaz

Bazı senaryolarda, yük devretme tamamlamak için yaklaşık sekiz ile on dakika sürer ek işleme gerektirir. Artık fark edebilirsiniz test fiziksel sunucuları, VMware Linux makineler, DHCP hizmetini etkinleştirir yok VMware Vm'lerini ve aşağıdaki önyükleme sürücüleri yok VMware Vm'leri için yük devretme süreleri: storvsc, vmbus, storflt, Intelide, ATAPI.

## <a name="create-a-process-server-in-azure"></a>Bir işlem sunucusu oluşturma

İşlem sunucusu Azure sanal makineden verileri alır ve şirket içi siteye gönderir. İşlem sunucusu ve korumalı VM arasında düşük gecikme süreli ağ gereklidir.

- Bir Azure ExpressRoute bağlantınız varsa, test amaçları için yapılandırma sunucusunda otomatik olarak yüklenen şirket içi işlem sunucusu kullanabilirsiniz.
- Bir VPN bağlantısı veya geri dönme bir üretim ortamında çalıştırıyorsanız varsa, yeniden çalışma için bir Azure tabanlı işlem sunucusu olarak bir Azure VM ayarlamanız gerekir.
- Bir işlem sunucusu azure'da ayarlamak için yönergeleri takip edin [bu makalede](site-recovery-vmware-setup-azure-ps-resource-manager.md).

## <a name="configure-the-master-target-server"></a>Ana hedef sunucusu yapılandırma

Varsayılan olarak, ana hedef sunucusu şirket içi yapılandırma sunucusunda çalışır. Bu öğreticinin amaçları doğrultusunda biz varsayılan asıl kullanıyorsunuz. Ana hedef sunucusu yeniden çalışma verileri alır.

VM bir vCenter sunucusu tarafından yönetilen bir ESXi ana bilgisayarda ise, ana hedef sunucusu çoğaltılan veriler için VM diskleri yazmak için VM'nin veri deposu (VMDK) erişimi olmalıdır. VM veri deposu okuma/yazma erişimi olan ana hedefin konak bağlandığından emin olun.

VM bir vCenter sunucusu tarafından yönetilmiyor bir ESXi açıksa, Site Recovery hizmeti yükü sırasında yeni bir VM oluşturur. VM ana hedef oluşturma ESX ana bilgisayarda oluşturulur.
Sanal sabit disk tarafından erişilebilen bir veri deposu olmalıdır ana hedef sunucusu çalıştıran ana.

VM vCenter kullanmıyorsa, makine koruyun önce ana hedef sunucusu, çalıştığı konağının bulma tamamlamanız gerekir. Bu, geri fiziksel çok başarısız sunucular için geçerlidir. Şirket içi VM varsa, başka bir seçenek, bir yeniden çalışma yapmadan önce onu silmektir. Yeniden çalışma yeni bir VM ana hedef ESX konağı olarak aynı ana bilgisayardaki sonra oluşturur. Farklı bir konuma başarısız olduğunda, aynı veri deposu ve aynı ESX ana bilgisayarın şirket içi ana hedef sunucusu tarafından kullanılan veri kurtarılır.

Ana hedef sunucusunda depolama VMotion'ı kullanamazsınız. Bunu yaparsanız, yeniden çalışma diskleri için kullanılabilir olmadığından, çalışmaz. Ana hedef sunucusu VMotion'ı listenizden dışlayın.

## <a name="reprotect-azure-vms"></a>Azure VM'ler koruyun

Bu yordam, şirket içi VM kullanılabilir değil ve alternatif bir konuma yeniden korumayı varsayar.

1. İçinde **ayarları** > **öğeleri çoğaltılan**, üzerinde başarısız oldu VM'ye sağ tıklayın > **yeniden koruma**.
2. İçinde **yeniden koruma**, doğrulayın **şirket içi Azure**, seçilir.
3. Şirket içi ana hedef sunucusu ve işlem sunucusu belirtin.

4. İçinde **veri deposu**, diskleri şirket içi kurtarmak istediğiniz ana hedef veri deposu seçin. Şirket içi VM silindi ve yeni disk oluşturmanız gerektiğinde bu seçeneği kullanın. Bu ayarları sayılır diskleri zaten var, ancak bir değer belirtmeniz gerekir.
5. Ana hedef bekletme sürücü seçin. Yeniden çalışma ilkesi otomatik olarak seçilir.
6. Tıklatın **Tamam** yükü başlamak için. Sanal makine Azure'dan şirket içi siteye çoğaltmak bir iş başlatır. Üzerinde ilerleme durumunu izleyebilirsiniz **işleri** sekmesi.

> [!NOTE]
> Kurtarmak istiyorsanız, varolan bir Azure VM VM şirket, şirket içi sanal makinenin veri deposu okuma/yazma erişimi olan ana hedef sunucunun ESXi ana bilgisayarda takma.


## <a name="run-a-failover-from-azure-to-on-premises"></a>Bir yük devretme Azure'dan şirket içi çalıştırın.

Geri şirket içi çoğaltmak için yeniden çalışma ilkesi kullanılır. Azure'a çoğaltma için bir çoğaltma ilkesi oluşturduğunuzda bu ilke otomatik olarak oluşturulur:

- İlke yapılandırma sunucusuyla otomatik olarak ilişkilendirilir.
- İlke değiştirilemez.
- İlke değerleri şunlardır:
    - RPO eşiği = 15 dakika
    - Kurtarma noktası bekletme = 24 saat
    - Uygulamayla tutarlı anlık görüntü sıklığı = 60 dakika

Yük devretme aşağıdaki gibi çalıştırın:

1. Üzerinde **çoğaltılan öğeler** sayfasında, makine adını sağ tıklayın > **planlanmamış yük devretme**.
2. İçinde **onaylayın yük devretme**, yük devretme yönü Azure'dan olduğunu doğrulayın.

3. Yük devretme için kullanmak istediğiniz kurtarma noktasını seçin. Bir uygulama tutarlı bir kurtarma noktası süre önce en son noktası oluşur ve bazı veri kaybına neden olur. Yük devretme çalıştığında, Site Recovery Azure sanal makinelerini kapatır ve şirket içi VM yedeklemesi önyüklenir. Bulunur miktar kapalı kalma süresi olmadan, bu nedenle uygun bir saat seçin.
4. Makine sağ tıklayın ve **yürütme**. Azure sanal makinelerini kaldıran bir işi tetikler.
5. Azure VM'ler beklendiği gibi kapatılmışsa olduğunu doğrulayın.


## <a name="reprotect-on-premises-machines-to-azure"></a>Azure için şirket içi makineler koruyun

Veriler artık, şirket içi sitenizdeki olmalıdır, ancak Azure'a çoğaltılmıyor. Azure'a çoğaltma başlatabilirsiniz yeniden gibi:

1. Kasadaki > **ayarları** > **çoğaltılan öğeler**, başarısız geri başarısız olmuş ve tıklatın VM'ler Geri'yi seçin **yeniden koruma**.
2. Azure'a çoğaltılan verileri göndermek ve tıklatın için kullanılan işlem sunucusunu seçin **Tamam**.

Yükü tamamlandıktan sonra geri Azure VM yinelenir ve bir yük devretme gerekli olarak çalıştırabilirsiniz.
