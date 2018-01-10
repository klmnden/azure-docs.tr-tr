---
title: "Yük devri ve Azure Site Recovery ile çoğaltılan geri fiziksel sunucuları başarısız | Microsoft Docs"
description: "Fiziksel sunucuları azure'a yük devri ve Azure Site Recovery ile şirket içi siteye geri başarısız hakkında bilgi edinin"
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 08/01/2018
ms.author: raynew
ms.openlocfilehash: 2fbad3b53319c8dc8be3480162a4fa67bab94306
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2018
---
# <a name="fail-over-and-fail-back-physical-servers-replicated-to-azure"></a>Yük devri ve geri fiziksel sunucuları Azure'a kopyalanan başarısız

Bu öğretici, Azure fiziksel bir sunucuya yük devri açıklar. Devredilir sonra kullanılabilir olduğunda, şirket içi sitenize sunucunun başarısız. 

## <a name="preparing-for-failover-and-failback"></a>Yük devretme ve yeniden çalışma için hazırlanılıyor

Site RECOVERY'yi kullanarak Azure'a çoğaltılan fiziksel sunucuların yalnızca geri VMware Vm'lerini başarısız olabilir. Yeniden çalışmak için bir VMware altyapı gerekir. 

Yük devretme ve yeniden çalışma dört aşama bulunur:

1. **Azure'a yük**: başarısız makineler üzerinden şirket içi sitesinden Azure'a.
2. **Azure Vm'leri koruyun**: şirket içi VMware sanal yeniden başlatmaları böylece Azure Vm'leri koruyun.
3. **Şirket içi yük devri**: Azure'dan başarısız olmasına bağlı olarak, yük devretme gerçekleştirme.
4. **Yeniden koruma şirket içi sanal makineleri**: veri geri başarısız oldu sonra geri için başarısız şirket içi VMware sanal makinelerini Azure'a çoğaltma başlatılması koruyun.

## <a name="verify-server-properties"></a>Sunucu özelliklerini doğrulayın

Sunucu özelliklerini doğrulayın ve ile uyumlu olduğundan emin olun [Azure gereksinimleri](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) Azure VM'ler için.

1. İçinde **korunan öğeler**, tıklatın **çoğaltılan öğeler**ve makineyi seçin.

2. İçinde **yinelenmiş öğesi** bölmesinde makine bilgilerini, sistem durumu özetini yoktur ve en son kullanılabilir kurtarma noktaları. Tıklatın **özellikleri** daha fazla ayrıntı görüntülemek için.
3. İçinde **işlem ve ağ**, değiştirebilirsiniz Azure ad, kaynak grubu, hedef boyutu [kullanılabilirlik kümesi](../virtual-machines/windows/tutorial-availability-sets.md), ve [disk ayarları yönetilen](#managed-disk-considerations)
4. Görüntüleyin ve hangi Azure VM yük devretme ve kendisine atanmış IP adresi sonra yer alacağı ağını/alt ağını dahil olmak üzere ağ ayarlarını değiştirin.
5. İçinde **diskleri**, makine işletim sistemi ve veri diskleri hakkındaki bilgileri görebilirsiniz.

## <a name="run-a-failover-to-azure"></a>Azure'a yük devretme gerçekleştirme

1. İçinde **ayarları** > **öğeleri çoğaltılan** makineye tıklayın > **yük devretme**.
2. İçinde **yük devretme** seçin bir **kurtarma noktası** devretmesini. Aşağıdaki seçeneklerden birini kullanabilirsiniz:
   - **En son** (varsayılan): Bu seçenek ilk Site Recovery hizmetine gönderilen tüm veriler işler. Yük devretme yük devretme tetiklendiğinde Site Recovery çoğaltılan tüm verilerini sahip olduktan sonra Azure VM oluşturduğundan düşük RPO (kurtarma noktası hedefi) sağlar.
   - **En son işlenen**: Site Recovery tarafından işlenen en son kurtarma noktası makineye bu seçenek yöneltilir. Bu seçenek, hiçbir zaman işlenmemiş verileri işlerken harcanan çünkü düşük RTO (Kurtarma süresi hedefi) sağlar.
   - **Son uygulama tutarlı**: Site Recovery tarafından işlenen son uygulamayla tutarlı kurtarma noktası makineye bu seçenek yöneltilir.
   - **Özel**: bir kurtarma noktası belirtin.

3. Seçin **yük devretme işlemine başlamadan önce makineyi kapatın** yük devretme tetiklemeden önce kaynak makine kapatılamıyor denemek için Site Recovery istiyorsanız. Kapatma başarısız olsa bile yük devretme devam eder. Yük devretme işleminin ilerleyişini izleyin **işleri** sayfası.
4. Azure VM'ye bağlanabilmeniz için hazırlanan, yük devretme sonrasında doğrulamak için Bağlan.
5. Doğruladıktan sonra **yürütme** yük devretme. Bu, tüm kullanılabilir kurtarma noktalarını siler.

> [!WARNING]
> Bir yük devretme devam ediyor iptal etme. Yük devretme başlamadan önce makine çoğaltma durur. Yük devretme iptal ederseniz, onu durdurur, ancak makine yeniden çoğaltma olmaz.
> Fiziksel sunucuları için ek yük devretme işlemi tamamlamak için yaklaşık sekiz ile on dakika sürebilir. 

## <a name="create-a-process-server-in-azure"></a>Bir işlem sunucusu oluşturma

İşlem sunucusu Azure sanal makineden verileri alır ve şirket içi siteye gönderir. İşlem sunucusu ve korumalı makine arasında düşük gecikme süreli ağ gereklidir.

- Bir Azure ExpressRoute bağlantınız varsa, test amaçları için yapılandırma sunucusunda otomatik olarak yüklenen şirket içi işlem sunucusu kullanabilirsiniz.
- Bir VPN bağlantısı veya geri dönme bir üretim ortamında çalıştırıyorsanız varsa, yeniden çalışma için bir Azure tabanlı işlem sunucusu olarak bir Azure VM ayarlamanız gerekir.
- ' Ndaki yönergeleri izleyin [bu makalede](site-recovery-vmware-setup-azure-ps-resource-manager.md) bir işlem sunucusu azure'da ayarlamak için.

## <a name="configure-the-master-target-server"></a>Ana hedef sunucusu yapılandırma

Varsayılan olarak, ana hedef sunucusu yeniden çalışma verileri alır. Şirket içi yapılandırma sunucusunda çalışır.

- İçin size geri dönecek VMware VM VMware vCenter Server tarafından yönetilen bir ESXi ana bilgisayarda ise, ana hedef sunucusu çoğaltılan veriler için VM diskleri yazmak için VM'nin veri deposu (VMDK) erişimi olmalıdır. VM veri deposu okuma/yazma erişimi olan ana hedefin konak bağlandığından emin olun.
- Site Recovery hizmeti bir vCenter sunucusu tarafından yönetilmiyor ESXi ana yükü sırasında yeni bir VM oluşturursa. VM ana hedef VM oluşturma ESX ana bilgisayarda oluşturulur. Sanal sabit disk tarafından erişilebilen bir veri deposu olmalıdır ana hedef sunucusu çalıştıran ana.
- Geri dönecek fiziksel makineler için bulma makine koruyun önce ana hedef sunucusu, çalıştığı konağının tamamlamanız gerekir.
- Şirket içi VM yeniden çalışma için zaten varsa, başka bir seçenek, bir yeniden çalışma yapmadan önce silme sağlar. Yeniden çalışma yeni bir VM ana hedef ESX konağı olarak aynı ana bilgisayardaki sonra oluşturur. Farklı bir konuma başarısız olduğunda, aynı veri deposu ve aynı ESX ana bilgisayarın şirket içi ana hedef sunucusu tarafından kullanılan veri kurtarılır.
- Ana hedef sunucusunda depolama VMotion'ı kullanamazsınız. Bunu yaparsanız, yeniden çalışma diskleri için kullanılabilir olmadığından, çalışmaz. Ana hedef sunucusu VMotion'ı listenizden dışlayın.

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

