---
title: Azure Site Recovery işlem sunucusu İzleyicisi
description: Bu makalede Azure Site Recovery işlem sunucusu izlemeyi öğrenin.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 08/24/2019
ms.author: raynew
ms.openlocfilehash: f3ba08bc1d557d53bca2421b5052f496c740b185
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65969331"
---
# <a name="monitor-the-process-server"></a>İşlem sunucusu İzleyicisi

Bu makalede izleme [Site Recovery](site-recovery-overview.md) işlem sunucusu.

- İşlem sunucusu, şirket içi VMware Vm'leri ve fiziksel sunucuları azure'a olağanüstü durum kurtarmayı ayarlarken kullanılır.
- Varsayılan olarak, işlem sunucusu yapılandırma sunucusunda çalışır. Yapılandırma sunucusunu dağıttığınızda, varsayılan olarak yüklenir.
- İsteğe bağlı olarak, çoğaltılmış makineler ölçek ve tutamacı daha fazla sayıda ve daha yüksek çoğaltma trafiği hacimlerini, ek olarak, ölçeği genişletilmiş işlem sunucularını dağıtabilirsiniz.

[Daha fazla bilgi edinin](vmware-physical-azure-config-process-server-overview.md) rolü ve işlem sunucusu dağıtımı hakkında.

## <a name="monitoring-overview"></a>İzleme görünümü

İşlem sunucusu çoğaltılan verilerin önbelleğe alma, sıkıştırma ve aktarım azure'a, özellikle de çok sayıda rolleri olduğundan işlem sunucusu durumu düzenli olarak izlemek önemlidir.

Genellikle işlem sunucusu performansını etkileyen durumlar vardır. Performansı etkileyen sorunları VM sistem durumu kritik duruma hem işlem sunucusu ve çoğaltılan makineler sonunda gönderme, geçişli bir etkisi olacaktır. Durumlar şunlardır:

- VM'ler yüksek sayıda yaklaştığını veya önerilen sınırlamaları aşan bir işlem sunucusu kullanın.
- İşlem sunucusunu kullanan VM'ler yüksek bir karmaşıklık oranı sahiptir.
- VM'ler ve işlem sunucusu arasında ağ aktarım hızı çoğaltma verilerini işlem sunucusuna yüklemek için yeterli değildir.
- Ağ aktarım hızı işlem sunucusu ve Azure arasında çoğaltma işlem sunucusu verileri azure'a yüklemek yeterli değil.

Bu sorunların tümü, Vm'leri kurtarma noktası hedefi (RPO) etkileyebilir. 

**Neden?** Bir VM için bir kurtarma noktası oluşturma VM'nin ortak noktası tüm disklerde gerektirdiğinden. Yüksek bir karmaşıklık oranı bir diski olduğundan, çoğaltma yavaşsa veya işlem sunucusu uygun değilse, Kurtarma noktaları nasıl verimli bir şekilde oluşturulur etkiler.

## <a name="monitor-proactively"></a>Proaktif izleme

İşlem sunucusu ile ilgili sorunları önlemek için önemlidir:

- İşlem sunucularını kullanarak belirli gereksinimlerini anlamak [kapasite ve rehberlik boyutlandırma](site-recovery-plan-capacity-vmware.md#capacity-considerations), işlem sunucularının dağıtıldığı emin olun ve öneriler göre çalıştırma.
- Uyarıları izleme ve sorunlarını ortaya çıktıkları verimli bir şekilde çalışmasını işlem sunucularını korumak için.


## <a name="process-server-alerts"></a>İşlem sunucusu uyarıları

İşlem sunucusu, sistem durumu uyarıları, aşağıdaki tabloda özetlenen bir dizi oluşturur.

**Uyarı türü** | **Ayrıntılar**
--- | ---
![İyi Durumda][green] | İşlem sunucusu bağlı ve iyi durumda.
![Uyarı][yellow] | Son 15 dakika boyunca % CPU kullanımı > 80
![Uyarı][yellow] | Son 15 dakika boyunca % bellek kullanımı > 80
![Uyarı][yellow] | Önbellek klasörü boş alan < % 30 son 15 dakika
![Uyarı][yellow] | Son 15 dakika için işlem sunucusu hizmetlerinin çalıştırmadığınız
![Kritik][red] | Son 15 dakika için CPU kullanımı > %95
![Kritik][red] | Bellek kullanımı > % 95'son 15 dakika boyunca
![Kritik][red] | Önbellek klasörü boş alan < %25 son 15 dakika
![Kritik][red] | 15 dakika boyunca işlem sunucusundan sinyal alınmadı.

![Tablo anahtarı](./media/vmware-physical-azure-monitor-process-server/table-key.png)

> [!NOTE]
> İşlem Sunucusu genel sistem durumunu oluşturulan kötü uyarıya dayanır.



## <a name="monitor-process-server-health"></a>İşlem sunucusu durumunu izleme

Aşağıdaki şekilde, işlem sunucularının sistem durumunu izleyebilirsiniz: 

1. Durum çoğaltılan bir makine ve kasa içinde kendi işlem sunucusu ve çoğaltma durumunu izlemek için > **çoğaltılan öğeler**, izlemek istediğiniz makineye tıklayın.
2. İçinde **çoğaltma durumu**, VM durumunu izleyebilirsiniz. Hata ayrıntıları için detaya gitmek için duruma tıklayın.

    ![Sanal makine Panosu'ndan işlem sunucusu durumu](./media/vmware-physical-azure-monitor-process-server/vm-ps-health.png)

4. İçinde **işlem sunucusu durumu**, işlem sunucusunun durumunu izleyebilirsiniz. Ayrıntılar için detayına gidin.

    ![Sanal makine Panosu'ndan işlem sunucusu ayrıntıları](./media/vmware-physical-azure-monitor-process-server/ps-summary.png)

5. Sistem durumu, ayrıca VM sayfasında grafik gösterimi kullanılarak izlenebilir.
    - Bir genişleme işlem sunucusu vurgulanan ilişkili uyarıları varsa turuncu ve kritik sorunları varsa kırmızı olur. 
    - İşlem sunucusu yapılandırma sunucusunda varsayılan dağıtım çalışıyorsa, yapılandırma sunucusu uygun şekilde vurgulanır.
    - Detaya gitmek için yapılandırma sunucusu veya işlem sunucusu'nı tıklatın. Sorunları ve düzeltme önerisi unutmayın.

Ayrıca izleyebilirsiniz işlem sunucuları altında kasadaki **Site Recovery altyapısı**. İçinde **Site Recovery altyapınızı yönetin**, tıklayın **Configuration Servers**. Aşağı işlem sunucusu ve tatbikatı işlem sunucusu ayrıntıları ilişkili yapılandırma sunucusunu seçin.


## <a name="next-steps"></a>Sonraki adımlar

- Tüm işlem sunucuları sorunları, izleyin varsa bizim [sorun giderme rehberi](vmware-physical-azure-troubleshoot-process-server.md)
- Daha fazla yardıma ihtiyacınız varsa, sorunuzu gönderin [Azure Site Recovery Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr). 

[green]: ./media/vmware-physical-azure-monitor-process-server/green.png
[yellow]: ./media/vmware-physical-azure-monitor-process-server/yellow.png
[red]: ./media/vmware-physical-azure-monitor-process-server/red.png
