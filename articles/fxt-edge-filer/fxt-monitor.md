---
title: Azure FXT Edge dosyalayıcı izleyin
description: Azure FXT Edge dosyalayıcı karma depolama önbelleği için donanım durumunu izleme
author: ekpgh
ms.service: fxt-edge-filer
ms.topic: conceptual
ms.date: 06/20/2019
ms.author: v-erkell
ms.openlocfilehash: e7395c69d99884a5c662e545a69778ed195aec55
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67543083"
---
# <a name="monitor-azure-fxt-edge-filer-hardware-status"></a>Azure FXT Edge dosyalayıcı donanım durumunu izleme

Azure FXT Edge dosyalayıcı karma depolama önbellek sistemi kasasına yöneticilerin donanım nasıl çalıştığını anlamanıza yardımcı olmak için yerleşik birden çok durum ışıklar vardır.

## <a name="system-health-status"></a>Sistem durumu

Daha yüksek düzeyde bir önbellek işlemleri izlemek için yazılımın Denetim Masası'nı kullanın. **Pano** sayfasında açıklandığı gibi [Denetim Masası Pano Kılavuzu](https://azure.github.io/Avere/legacy/dashboard/4_7/html/ops_dashboard_index.html)

## <a name="hardware-status-leds"></a>Donanım durumunu LED'leri

Bu bölümde Azure FXT Edge dosyalayıcı donanıma yerleşik çeşitli durumu ışıkları açıklanmaktadır.

### <a name="hard-drive-status-leds"></a>Sabit sürücü durumu LED'leri

![Yatay belirtme çizgisi etiketlerle 2 sabit sürücüsü ön resmi (sol üst köşede), 1 (sol alt köşesinde) ve 3 (sağ taraf)](media/fxt-monitor/fxt-drive-callouts.png)

İki durum LED'lerini her sürücü taşıyıcı vardır: bir etkinliği göstergesini (1) ve bir durum göstergesi (2). 

* Sürücü kullanımda olmadığında etkinlik (1) ışıkları GEREKTİRİYORDU.  
* LED (2) durumu, aşağıdaki tabloda kodları kullanarak sürücüsü durumunu gösterir.

| Sürücü LED durumu durumu              | Anlamı  |
|-------------------------------------|----------------------------------------------------------|
| Saniye başına iki kez yeşil yanıp sönen      | Sürücü tanımlayan *veya* <br> Sürücü temizleme için hazırlama  |
| (Işıklandırılmamış)                         | Sistem başlatma tamamlandı değil *veya* <br>Kaldırılacak sürücü hazır |
| Yanıp sönen amber, yeşil ve kapatma       | Bir sürücü hatası tahmin   |
| Saniyede dört kez yanıp sönen amber | Sürücü başarısız oldu   |
| Düz yeşil                         | Sürücü çevrimiçi |

(3) sürücü sağ tarafında, sürücü kapasite ve diğer bilgileri ile etiketlenir.

Sürücü numaraları sürücüleri arasındaki boşluk yazdırılır. Azure FXT Edge dosyalayıcı 0 sürücü sol üst sürücüdür ve 1 doğrudan o altında sürücüdür. Numaralandırma, bu düzende devam eder. 

![Fotoğraf bir sabit sürücünün sürücü numaraları ve kapasite etiketleri gösteren FXT kasa içinde bölmesi](media/fxt-drives-photo.png)

## <a name="left-control-panel"></a>Sol Denetim Masası

Sol Ön Denetim Masası çeşitli durumu LED Göstergeleri (1) ve çok ışıklı sistem durumu göstergesi (2) sahiptir. 

![1 durum göstergeleri sol ve sağda ışık büyük sistem sistem durumu göstergesi etiketleme 2 etiketleme sol durum bölmesi](media/fxt-monitor/fxt-control-panel-left.jpg)

### <a name="control-panel-status-indicators"></a>Denetim Masası Durum göstergeleri 

Durum göstergeleri sol konumunda düz amber ışık bu sistemdeki bir hata olup olmadığını gösterir. Aşağıdaki tabloda, hatalar için olası nedenler ve çözümler açıklanmaktadır. 

Yine de bu çözümleri denedikten sonra hata varsa, Yardım için desteğe başvurun. 

| Simge | Açıklama | Hata koşulu | Olası çözümler |
|----------------|---------------|--------------------|----------------------|
| ![Sürücü simgesi](media/fxt-monitor/fxt-hd-icon.jpg) | Sürücü durumu | Sürücü hatası | Sürücünün bir hata olup olmadığını belirlemek için sistem olay günlüğünü kontrol edin veya <br>Uygun çevrimiçi Tanılama testi çalıştırın; sistemi yeniden başlatın ve ekli tanılama (ePSA) çalıştırın veya <br>Sürücüleri bir RAID dizisinde yapılandırıldıysa, sistemi yeniden başlatın ve ana bilgisayar bağdaştırıcısı yapılandırma yardımcı programı girin |
|![Sıcaklık simgesi](media/fxt-monitor/fxt-temp-icon.jpg) | Sıcaklık durumu | Sıcaklık hata - Örneğin, bir sporseverseniz başarısız olursa veya ortam sıcaklığı je mimo rozsah | Aşağıdaki adreslenebilir koşulları denetleyin: <br>Bir soğutma fan eksik ya da başarısız oldu <br>Sistemin kapak, hava shroud bellek modülü boş ya da arka dolgusu köşeli ayraç kaldırılır <br>Ortam sıcaklığı çok yüksek <br>Dış hava akışı engellendiği |
|![Elektrik simgesi](media/fxt-monitor/fxt-electric-icon.jpg) | Elektrik durumu | Elektrik hata - Örneğin, aralık dışında voltaj PSU ya da başarısız gerilim düzenleyicisi başarısız oldu. |  Özel konu için sistem iletisi ve sistem olay günlüğünü denetleyin. PSU ile ilgili bir sorun varsa, LED PSU durumunu denetleyin ve gerekirse PSU yeniden takın. | 
|![bellek simgesi](media/fxt-monitor/fxt-memory-icon.jpg) | Bellek durumu | Bellek hatası | Başarısız bellek konumu için sistem iletisi ve sistem olay günlüğünü kontrol edin; bellek modülü yeniden takın. |
|![PCIe simgesi](media/fxt-monitor/fxt-pcie-icon.jpg) | PCIe durumu | PCIe kart hatası | Sistemi yeniden başlatın. PCIe kart sürücüleri güncelleştirme; kart yeniden yükleyin |


### <a name="system-health-status-indicator"></a>Sistem sağlık durumu göstergesi

Sol Denetim Masası'nı sağ büyük aydınlatılmış düğme genel sistem durumunu gösterir ve ayrıca sistem kimliği modunda bir birim Bulucu açık olarak kullanılır.

Sistem durumu ve sistem kimliği modu ve sistem durumu modu arasında geçiş yapmak için kimliği düğmesine basın.

|Sistem sağlık durumu | Koşul |
|-------------------------------------------|-----------------------------------------------|
| Düz mavi | Normal işlem: sistemin normal, açık ve sistem kimliği modu etkin değil. <br/>Sistem Kimliği moduna geçmek istiyorsanız sistem durumu ve kimliği düğmesine basın. |
| Yanıp sönen mavi | Sistem Kimliği modu etkin değil. Sistem durumu moduna geçmek istiyorsanız sistem durumu ve sistem kimliği düğmesine basın. |
| Düz amber | Sistem emniyet modundadır. Sorun devam ederse Microsoft Müşteri Hizmetleri ve desteği ile iletişime geçin. |
| Amber yakıp söndürme | Sistem hatası. Belirli hata iletileri için sistem olay günlüğünü denetleyin. Sistem bellenimi ve sistem bileşenleri izleme aracıları tarafından oluşturulan olay ve hata iletileri hakkında daha fazla bilgi için qrl.dell.com hata kodunu arama sayfasına bakın. |


