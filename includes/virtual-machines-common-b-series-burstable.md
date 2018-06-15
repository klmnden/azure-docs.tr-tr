---
title: include dosyası
description: include dosyası
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 03/09/2018
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 8a7207328f49488b0df8f6e1e0ed86c6f965d32f
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
ms.locfileid: "34307446"
---
B-serisi VM ailesi, hangi VM boyutu en fazla % 100'ünü Intel® Broadwell E5-2673 v4 CPU performans veri bloğu olanağı, iş yükü için gerekli temel düzey performans sağlar seçmenize olanak verir 2.3 GHz veya bir Intel® Haswell 2.4 GHz E5-2673 v3 işlemcisi vCPU.

B-serisi VM'ler CPU tam performansını sürekli olarak, web sunucuları gibi küçük veritabanları ve geliştirme gerekir ve ortamlarında test iş yükleri için idealdir. Bu iş yükleri genellikle burstable performans gereksinimleri vardır. B-serisi, bir VM boyutu taban çizgisi performansı ile satın almanıza olanak sağlar ve taban sayısından az kullanırken VM örneği kredilerinizin tamamını oluşturur. VM kredi birikmiş, VM uygulamanız daha yüksek CPU performans gerektirdiğinde % 100 vCPU, kullanarak temel veri bloğu.

B-serisi aşağıdaki altı VM boyutları sunar:

| Boyut          | vCPU'ın | Bellek: GiB | Geçici depolama (SSD) GiB | VM temel CPU performans | En fazla CPU performans VM | Bankaya nakledilen krediler / saat | Max krediler Bankaya nakledilen |
|---------------|--------|-------------|----------------|--------------------------------|---------------------------|-----------------------|--------------------|
| Standard_B1s  | 1      | 1           | 4              | %10                            | 100%                      | 6                     | 144                |
| Standard_B1ms | 1      | 2           | 4              | %20                            | 100%                      | 12                    | 288                |
| Standard_B2s  | 2      | 4           | 8              | 40%                            | 200%                      | 24                    | 576                |
| Standard_B2ms | 2      | 8           | 16             | 60%                            | 200%                      | 36                    | 864                |
| Standard_B4ms | 4      | 16          | 32             | 90%                            | 400%                      | 54                    | 1296               |
| Standard_B8ms | 8      | 32          | 64             | 135%                           | 800%                      | 81                    | 1944               |




## <a name="q--a"></a>Soru-Cevap 

### <a name="q-how-do-you-get-135-baseline-performance-from-a-vm"></a>S: nasıl bir sanal makineden %135 temel performans elde?
**A**: %135 VM boyutu hale 8 vCPU's arasında paylaşılır. Örneğin, uygulamanız üzerindeki toplu işleme çalışma 8 çekirdek 4 kullanıyorsa ve % 30 kullanımı sırasında çalışan her bu 4 vCPU ait bir VM CPU performans toplam miktarı %120 eşit.  VM kredi zamanını temel performans % 15 delta dayalı derleme anlamına gelir.  Ancak, ayrıca, aynı VM tüm 8 vCPU % 100'ünü kullanabileceğiniz krediler varken bu VM %800 en fazla CPU performansını vermiş olduğunu gösterir.


### <a name="q-how-can-i-monitor-my-credit-balance-and-consumption"></a>S: nasıl ı my kredi bakiyesi ve tüketim izleyebilir mi
**A**: biz 2 yeni ölçümleri önümüzdeki haftalarda Tanıtımı **kredi** ölçüm etmenizi sağlar, VM Bankaya nakledilen kaç krediler görüntülemek ve **ConsumedCredit** ölçüm kaç CPU gösterir VM banka tüketmiş krediler.    Bu ölçümler ölçümleri bölmesinden portalında veya Azure İzleyici API'ler aracılığıyla programlı olarak görüntülemek kuramaz.

Ölçüm verilerini Azure için erişim hakkında daha fazla bilgi için bkz: [Microsoft Azure ölçümlerini genel bakış](../articles/monitoring-and-diagnostics/monitoring-overview-metrics.md).

### <a name="q-how-are-credits-accumulated"></a>S: nasıl krediler toplanır?
**A**: VM Birikme ve tüketim ücretleri en temel performans düzeyde tam olarak çalışan bir VM ne net Birikme ya da krediler emniyeti, tüketim sağlayacak şekilde ayarlanır.  Bir VM temel performans düzeyiyle çalıştığında net bir artış krediler içinde sahip olur ve VM temel performans düzeyiyle'birden fazla CPU kullanan her krediler net bir düşüş gerekir.

**Örnek**: küçük zaman ve katılımcı veritabanı Uygulamam için B1ms boyutu kullanarak bir VM'i dağıtma. Bu boyutta bir vCPU % 20'ye kadar banka veya kullanabilirim dakika başına.2 krediler my taban çizgisi olarak kullanılacak Uygulamam sağlar. 

Uygulamam başına ve 7:00-09:00:00 ve 4:00-06:00 saatleri arasında my çalışanların iş günü sonunda meşgul. Diğer 20 sırasında günün, Uygulamam genellikle saattir adresindeki, yalnızca % 10 ' vCPU kullanarak boş. Yoğun olmayan saatler ı dakikada 0.2 krediler kazanma ancak VM'im 60 x.1 banka şekilde, dakikada 0.l iadeleri yalnızca tüketen saat başına 6 krediler =.  20, ı yoğun olmayan saatler için ı 120 krediler banka.  

Yoğun saatlerde Uygulamam % 60 vCPU kullanımı ortalama, ı hala dakikada 0.2 krediler kazanma ancak dakikada 0,6 krediler kullanmak, net maliyetini.4 krediler için bir dakika veya.4 x 60 = 24 saat başına KREDİLERİ. 4 x 24 = 96 maliyetleri için en yüksek kullanım, günde 4 saat sahip my en yüksek kullanımı için iadeleri.

I kazanılan 120 krediler yoğun olmayan almak ve my yoğun saatlerde için kullanılan 96 krediler çıkarma, diğer etkinlik WINS'e için kullanabileceğim günde bir ek 24 krediler banka.


### <a name="q-does-the-b-series-support-premium-storage-data-disks"></a>S: B-serisi Premium depolama veri diskleri destekliyor mu?
**A**: Evet, tüm B-serisi boyutları Premium depolama veri disklerini destekler.   
    
### <a name="q-why-is-my-remaining-credit-are-set-to-0-after-a-redepoy-or-a-stopstart"></a>My kalan kredi neden olan 0 olarak ayarlanmış bir redepoy veya Durdur/Başlat sonra?
**A** : bir VM zamanı "REDPLOYED" olan ve VM başka bir düğüme taşır birikmiş kredi kaybolur. VM durduruldu ve başlatıldı, ancak aynı düğümde kalır, VM birikmiş kredi korur. VM bir düğümünde yeni başlattığında, ilk kredi alır, isteğe bağlı olarak Standard_B8ms için 240 dakika olur.

    

    
