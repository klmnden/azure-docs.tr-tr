---
title: "Yük devri ve başarısız geri Hyper-V sanal makinelerini Azure Site Recovery ile çoğaltılan | Microsoft Docs"
description: "Hyper-V Vm'lerini azure'a yük devri ve Azure Site Recovery ile şirket içi siteye geri başarısız hakkında bilgi edinin"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 72065d01-ed0f-430e-b117-a5885cecd1db
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 09/15/2017
ms.author: raynew
ms.openlocfilehash: e1cc21661450a983c25b24fe2a6228e26ceecec6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="fail-over-and-fail-back-hyper-v-vms-replicated-to-azure"></a>Yük devri ve başarısız geri Hyper-V Azure'a kopyalanan VM

[Azure Site Recovery](site-recovery-overview.md) hizmet yöneten ve çoğaltma, yük devretme ve yeniden çalışma şirket içi makineler ve Azure sanal makineleri (VM'ler) yönetir.

Bu öğretici, Azure'da bir Hyper-V VM yük devri açıklar. Devredilir sonra kullanılabilir olduğunda, şirket içi sitenize başarısız. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Şirket içi Hyper-V Vm'lerini azure'a yük devri
> * Azure'dan şirket içi başarısız ve Azure'da yeniden çoğaltma Başlat

## <a name="overview"></a>Genel Bakış
Yük devretme ve yeniden çalışma iki aşama vardır:

1. **Azure'a yük**: başarısız makineler üzerinden şirket içi sitesinden Azure'a.
2. **Azure'dan şirket içi başarısız**: planlanmış bir yük devretme Azure'dan şirket içi çalıştırın. Planlı yük devretme sonrasında yeniden şirket içinden Azure'a çoğaltma başlatmak çoğaltmayı tersine çevirme, etkinleştirin. 


## <a name="fail-over-to-azure"></a>Azure'a yük

### <a name="failover-prerequisites"></a>Yük devretme önkoşulları

Yük devretmeyi tamamlamak için:

- Tamamlandı emin olun bir [olağanüstü durum kurtarma ayrıntıya](tutorial-dr-drill-azure.md) her şeyin beklendiği gibi çalıştığını denetlemek için.
- Yük devretme önce Azure Vm'lerine bağlanmak isteğe bağlı olarak hazırlayın.
- Yük devretme çalıştırmadan önce VM özelliklerini doğrulayın.

#### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Yük devretmeden sonra Azure VM'lerine bağlanmak için hazırlık yapma

Yük devretmeden sonra RDP kullanarak Azure vm'lerine bağlanmak isterseniz, aşağıdakileri yapın:

##### <a name="azure-vms-running-windows"></a>Windows çalıştıran azure VM'ler

1. Azure VM'ler internet üzerinden erişmek için yük devretmeden önce şirket içi VM üzerinde RDP etkinleştirin. Bu TCP emin olun ve UDP kuralları için eklenir **ortak** profili ve RDP izin verildiğinden emin **Windows Güvenlik Duvarı** > **izin verilen uygulamaları** tüm profiller için.
2. Siteden siteye VPN üzerinden erişmek için şirket içi makinede RDP etkinleştirin. RDP izin içinde **Windows Güvenlik Duvarı** -> **verilen uygulamalar ve Özellikler** için **etki alanı ve özel** ağlar. İşletim sisteminin SAN ilkesinin kümesine onay **OnlineAll**. [Daha fazla bilgi edinin](https://support.microsoft.com/kb/3031135). Olmamalıdır bekleyen hiçbir Windows güncelleştirmeleri VM üzerinde bir yük devretme tetiklemek olduğunda. Varsa, güncelleştirme tamamlanana kadar sanal makineye oturum açamaz olmayacaktır. 
3. Yük devretme sonrasında Windows Azure VM üzerinde kontrol **önyükleme tanılama** ekran VM görüntüsünü görüntülemek için. Bağlanamıyorsanız, VM çalışıp çalışmadığını denetleyin ve bu gözden [sorun giderme ipuçları](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


##### <a name="azure-vms-running-linux"></a>Linux çalıştıran azure VM'ler

1. Yük devretmeden önce şirket içi makinede Secure Shell hizmetinin sistem önyüklemesinde otomatik olarak başlamaya ayarlanmıştır denetleyin. Güvenlik duvarı kuralları bir SSH bağlantısı izin verdiğinden emin olun.
2. Yük devretme sonrasında Azure VM'de, SSH bağlantı noktası için ağ güvenlik grubu kurallarının devredilen VM'ye ve bağlı olduğu Azure alt ağ için gelen bağlantılara izin verin.  [Bir ortak IP adresi eklemek](site-recovery-monitoring-and-troubleshooting.md#adding-a-public-ip-on-a-resource-manager-virtual-machine) VM için. Kontrol edebilirsiniz **önyükleme tanılama** ekran VM görüntüsünü görüntülemek için.


#### <a name="verify-vm-properties"></a>VM özelliklerini doğrulayın

VM özelliklerini doğrulayın ve VM ile uyumlu olduğundan emin olun [Azure gereksinimleri](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

1. İçinde **korunan öğeler**, tıklatın **çoğaltılan öğeler** > VM.
2. İçinde **yinelenmiş öğesi** bölmesinde VM bilgileri, sistem durumu özetini yoktur ve en son kullanılabilir kurtarma noktaları. Tıklatın **özellikleri** daha fazla ayrıntı görüntülemek için.
3. İçinde **işlem ve ağ**, değiştirebilirsiniz Azure ad, kaynak grubu, hedef boyutu [kullanılabilirlik kümesi](../virtual-machines/windows/tutorial-availability-sets.md), ve [disk ayarları yönetilen](#managed-disk-considerations)
4. Görüntüleyin ve hangi Azure VM yük devretme ve kendisine atanmış IP adresi sonra yer alacağı ağını/alt ağını dahil olmak üzere ağ ayarlarını değiştirin.
5. İçinde **diskleri**, VM işletim sistemi ve veri diskleri hakkındaki bilgileri görebilirsiniz.


### <a name="run-a-failover-from-on-premises-to-azure"></a>Şirket içinden Azure'a yük devretme gerçekleştirme

Hyper-V VM'ler için normal veya planlı bir yük devretme çalıştırabilirsiniz

- Normal bir yük devretme için beklenmedik kesintileri kullanın. Bu yük devretme çalıştırdığınızda, Site kurtarma Azure VM oluşturur ve yukarı çalıştırır. Belirli bir kurtarma noktası karşı yük devretme çalıştırın. Kullandığınız kurtarma noktası bağlı olarak veri kaybı oluşabilir.
- Planlanmış bir yük devretme sırasında beklenen kesinti veya bakım için kullanılabilir. Bu seçenek, sıfır veri kaybı sağlar. Planlanmış bir yük devretme tetiklendiğinde, kaynak sanal makineleri kapatılır. Eşitlenmemiş veriler eşitlenir ve yük devretme tetiklenir.

Bu yordamda, normal bir yük devretmeyi çalıştırma açıklanmaktadır.


1. İçinde **ayarları** > **öğeleri çoğaltılan** VM tıklayın > **yük devretme**.
2. İçinde **yük devretme** seçin bir **kurtarma noktası** devretmesini. Aşağıdaki seçeneklerden birini kullanabilirsiniz:
    - **En son** (varsayılan): Bu seçenek ilk Site Recovery hizmetine gönderilen tüm veriler işler. Yük devretme yük devretme tetiklendiğinde Site Recovery çoğaltılan tüm verilerini sahip olduktan sonra Azure VM oluşturduğundan düşük RPO (kurtarma noktası hedefi) sağlar.
    - **En son işlenen**: Bu seçenek, Site Recovery tarafından işlenen en son kurtarma noktası VM'ye yöneltilir. Bu seçenek, hiçbir zaman işlenmemiş verileri işlerken harcanan çünkü düşük RTO (Kurtarma süresi hedefi) sağlar.
    - **Son uygulama tutarlı**: Bu seçenek, Site Recovery tarafından işlenen son uygulamayla tutarlı kurtarma noktası VM'ye yöneltilir. 
    - **Özel**: bir kurtarma noktası belirtin.
3. Hyper-V Vm'lerini azure'a, System Center VMM bulutlarında devretmek ve içinde bulut için veri şifreleme etkinleştirilmişse **şifreleme anahtarı**, sağlayıcı kurulumu sırasında veri şifreleme etkinleştirildiğinde verilmiş sertifikayı seçin VMM sunucusu...
4. Seçin **yük devretme işlemine başlamadan önce makineyi kapatın** Site Kurtarma'nın bir kapanma kaynak sanal makinelerin yük devretme tetiklemeden yapmaya istiyorsanız. Site Recovery, yük devretme tetiklemeden önce Azure için henüz gönderilmedi şirket içi verileri eşitlemek de deneyecek. Kapatma başarısız olsa bile, yük devretme devam unutmayın. Yük devretme işleminin ilerleyişini izleyin **işleri** sayfası.
5. Azure VM'ye bağlanabilmeniz için hazırlanan, yük devretme sonrasında doğrulamak için Bağlan.
6. Doğruladıktan sonra **yürütme** yük devretme. Bu, tüm kullanılabilir kurtarma noktalarını siler.

> [!WARNING]
> **Bir yük devretme devam ediyor iptal etme**: yük devretme başlatılmadan önce VM çoğaltma durduruldu. Bir yük devretme devam ediyor, yük devretme durduruyor, iptal ancak VM yeniden çoğaltma olmaz  


## <a name="fail-back-from-azure-to-on-premises"></a>Azure'dan şirket içi başarısız

### <a name="prerequisites-for-failback"></a>Yeniden çalışma için Önkoşullar

Yeniden çalışma tamamlamak için birincil site VMM sunucusu/Hyper-V sunucusu Site Recovery hizmetine bağlı olduğundan emin olun.


### <a name="run-a-failback-and-reprotect-on-premises-vms"></a>Bir yeniden çalışma çalıştırın ve şirket içi Vm'leri koruyun

Azure'dan şirket içi siteye yük devri ve şirket içi siteden Vm'lerini Azure'a çoğaltma başlatın.

1. İçinde **ayarları** > **öğeleri çoğaltılan** VM tıklayın > **planlı yük devretme**.
2. İçinde **planlı yük devretme onaylayın**, yük devretme yönünden (Azure) doğrulayın ve kaynak ve hedef konumları seçin. 
3. İçinde **veri eşitleme**, nasıl eşitlemek istediğinizi belirtin:
    - **Yük devretme önce verileri eşitle (yalnızca delta değişiklikler eşitleme)**— VM kapatmadan eşitlediğinden bu seçeneği VM kapalı kalma süresini en aza indirir. İşte ne yapar:
        1. Azure VM anlık görüntüsünü alır ve şirket içi Hyper-V konağına kopyalar. VM, Azure'da çalışan tutar.
        2. Yeni değişiklik yok gerçekleşmesi Azure VM kapatır. Delta değişikliklerin son kümesi, şirket içi sunucusuna aktarılır ve şirket içi VM başlatılır.
    - **Verileri (tam yükleme) yalnızca yük devretme sırasında eşitleme**— Azure'da uzun süredir çalışan, bu seçeneği kullanın. Biz birden çok disk değişiklikler beklediğiniz ve sağlama toplamı hesaplamaları süre beklemesini yoktur çünkü bu seçenek daha hızlıdır. Bu seçenek, bir disk yükleme gerçekleştirir. Şirket içi VM silindiğinde de yararlıdır.
4. Hyper-V Vm'lerini azure'a, System Center VMM bulutlarında devretmek ve içinde bulut için veri şifreleme etkinleştirilmişse **şifreleme anahtarı**, sağlayıcı kurulumu sırasında veri şifreleme etkinleştirildiğinde verilmiş sertifikayı seçin VMM sunucusu.
5. Yük devretme başlatın. Yük devretme işleminin ilerleyişini izleyin **işleri** sekmesi.
6. Yük devretme önce verileri eşitlemek için seçtiyseniz, ilk veri eşitlemeyi yapılır ve Azure VM'ler tıklatın kapatmaya hazır sonra **işleri** > Planlı yük devretme iş adı > **yükdevretmeyitamamlamak**. Bu Azure VM kapatan, en son değişiklikleri şirket içi aktarır ve şirket içi VM başlatır.
7. Beklendiği gibi kullanılabilir denetlemek için şirket içi VM oturum açın.
8. Şirket içi VM yürütme bekleme durumunda sunulmuştur. Tıklatın **tamamlama**, yük devretme kaydedilemedi. Bu, Azure Vm'leri ve kendi diskleri siler ve şirket içi VM çoğaltmayı tersine çevirme için hazırlar.
9. Şirket içi VM Azure'a çoğaltma başlatmak için etkinleştirme **ters çoğaltma**.


> [!NOTE]
> Çoğaltmayı tersine çevirme yalnızca Azure VM kapalı ve yalnızca delta değişiklikler gönderilen bu yana gerçekleşen değişiklikleri çoğaltır.

