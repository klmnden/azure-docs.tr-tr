---
title: include dosyası
description: include dosyası
services: virtual-machines
author: ayshakeen
ms.service: virtual-machines
ms.topic: include
ms.date: 03/09/2018
ms.author: azcspmt;ayshak;cynthn
ms.custom: include file
ms.openlocfilehash: ecf70bbbeae8fd68309f3343615f021038fb10b6
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188367"
---
B serisi VM ailesi, hangi sanal makine boyutu %100 Intel® Broadwell E5-2673 v4'üne kadar CPU performans düzeyine çıkış yapması özelliği sayesinde, iş yükü için gereken temel düzeyde performans sağlar seçmenizi sağlar 2.3 GHz veya bir Intel® Haswell 2.4 GHz E5-2673 v3 işlemci vCPU.

B serisi sanal makineler, web sunucuları gibi kavramları, küçük veritabanları ve geliştirme yapı ortamları sağlama CPU'nun tam performansta sürekli olarak ihtiyacınız olan iş yükleri için idealdir. Bu iş yükleri genellikle seri aktarıma uygun performans gereksinimleri vardır. B serisi, bir VM boyutu temel performans satın almanıza olanak sağlar ve taban sayısından az kullanılırken, sanal makine örneği kredi oluşturur. VM VM kredi toplandığında, daha yüksek bir CPU performans uygulamanızın gerektirdiği kadar vCPU %100 kullandığınızda temel veri bloğu.

B serisi, aşağıdaki altı VM boyutlarında gelir:

| Boyut             | Sanal işlemci  | Bellek: GiB | Geçici depolama (SSD) GiB | Sanal makinenin temel bir CPU performans | En fazla CPU performans VM | Başlangıç kredisi | Bankaya nakledilen KREDİLERİ / saat | En fazla KREDİLERİ Bankaya nakledilen | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS / MB/sn | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS / MB/sn | En fazla NIC |          
|---------------|-------------|----------------|----------------------------|-----------------------|--------------------|--------------------|--------------------|----------------|----------------------------------------|-------------------------------------------|-------------------------------------------|----------|
| Standard_B1ls<sup>1</sup>  | 1           | 0,5              | 4                          | %5                   | 100%                   | 30                   | 3                  | 72            | 2                                      | 200 / 10                                  | 160 / 10                                  | 2  |
| Standard_B1s  | 1           | 1              | 4                          | %10                   | 100%                   | 30                   | 6                  | 144            | 2                        | 400 / 10                                  | 320 / 10                                  | 2  |
| Standard_B1ms | 1           | 2              | 4                          | %20                   | 100%                   | 30                   | 12                 | 288           | 2                         | 800 / 10                                  | 640 / 10                                  | 2  |
| Standard_B2s  | 2           | 4              | 8                          | 40%                   | 200%                   | 60                   | 24                 | 576            | 4                                      | 1600 / 15                                 | 1280 / 15                                 | 3  |
| Standard_B2ms | 2           | 8              | 16                         | 60%                   | 200%                   | 60                   | 36                 | 864            | 4                                      | 2400 / 22.5                               | 1920 / 22.5                               | 3  |
| Standard_B4ms | 4           | 16             | 32                         | 90%                   | 400%                   | 120                   | 54                 | 1296           | 8                                      | 3600 / 35                                 | 2880 / 35                                 | 4  |
| Standard_B8ms | 8           | 32             | 64                         | 135%                  | 800%                   | 240                   | 81                 | 1944           | 16                                     | 4320 / 50                                 | 4320 / 50                                 | 4  |

<sup>1</sup> B1ls, yalnızca Linux üzerinde desteklenir

## <a name="q--a"></a>Soru - Yanıt 

### <a name="q-how-do-you-get-135-baseline-performance-from-a-vm"></a>S: Nasıl bir VM'den %135 temel performans elde ederim?
**A**: %135 yaptığınız VM boyutu 8 vCPU's arasında paylaşılır. Örneğin, 4, 8 çekirdek toplu işlem üzerinde çalışan uygulama kullanıyorsa ve % 30 kullanımı sırasında çalışan her biri bu 4 vCPU's VM CPU performansı toplam miktarı %120 eşit.  Sanal makinenizin temel performansınızı % 15 deltasında dayalı kredi zaman oluşturursunuz anlamına gelir.  Ancak, aynı VM tüm 8 vCPU, %100 kullanabileceğiniz krediler olduğunda bu VM bir en fazla CPU performans %800 vererek, ayrıca anlamına gelir.


### <a name="q-how-can-i-monitor-my-credit-balance-and-consumption"></a>S: My kredi bakiyesi ve tüketimini nasıl izleyebilirim
**A**: Biz 2 yeni ölçümler önümüzdeki haftalarda sunmuyoruz **kredi** ölçüm, kaç tane VM'niz Bankaya nakledilen KREDİLERİ görüntülemenize olanak sağlayacaktır ve **ConsumedCredit** ölçüm VM'nizi sahip kaç CPU kredisi gösterir Banka tüketilen.    Portalında veya Azure İzleyici API'ler aracılığıyla programlı olarak ölçümleri bölmesinden bu ölçümleri görüntüleme olanağınız olacaktır.

Ölçüm verilerini Azure için erişim hakkında daha fazla bilgi için bkz. [Microsoft azure'da ölçümlere genel bakış](../articles/monitoring-and-diagnostics/monitoring-overview-metrics.md).

### <a name="q-how-are-credits-accumulated"></a>S: Krediler ne toplanır?
**A**: VM birikmesi ve Tüketim oranları, tamamen kendi temel performans düzeyinde çalışan bir VM ağ birikmesi veya KREDİLERİ Patlaması, tüketim ne sağlayacak şekilde ayarlanır.  Bir VM, temel performans düzeyinin altında çalışır durumda olduğunda krediler net bir artış olacaktır ve VM temel performans düzeyiyle birden çok CPU kullanan her KREDİLERİ net bir düşüş olur.

**Örnek**:  Küçük zaman ve katılım veritabanı uygulaması B1ms boyutunu kullanarak VM dağıtabilir. Bu boyut en fazla %20 bir vCPU, 0.2 kredi veya banka kullanabilirim dakika başına my taban çizgisi olarak kullanılacak Uygulamam sağlar. 

Uygulamamın 7:00-9:00 ÖÖ ve 4:00-18:00:00 arasında my çalışanların iş günü sonunda ve başındaki meşgul. Diğer 20 saatleri günün Uygulamam genellikle olduğu anda, yalnızca % 10 ' vCPU kullanarak boş. Yoğun olmayan saatlere miyim dakika başına 0.2 KREDİLERİ kazanın, ancak sanal Makinem 0,1 x 60 = 6 banka şekilde yalnızca dakika başına 0.l aktarımla birlikte bunların KREDİLERİ / saat.  20, ı yoğun olmayan saatler için ı 120 KREDİLERİ banka.  

Yoğun saatlerde Uygulamam % 60 vCPU kullanımı ortalama, ı hala dakika başına 0.2 KREDİLERİ kazanın ancak ben dakika başına 0,6 aktarımla birlikte bunların, 0,4 KREDİLERİ net maliyeti için bir dakika ya da 0.4 x 60 = 24 saatlik kredisi. En yüksek kullanımı, günde 4 saat sahibim 4 x 24 = 96 maliyetleri için en yüksek kullanımı için kredi.

120 miyim kazanılan krediler yoğun alın ve benim için tepe zamanları kullandım 96 KREDİLERİ çıkarma, ı kullanabileceğim diğer etkinlik patlamalarına günde bir ek 24 KREDİLERİ banka.


### <a name="q-does-the-b-series-support-premium-storage-data-disks"></a>S: B serisi, Premium depolama diskleri destekliyor mu?
**A**: Evet, Premium depolama diskleri tüm B serisi boyutları destekler.   
    
### <a name="q-why-is-my-remaining-credit-set-to-0-after-a-redeploy-or-a-stopstart"></a>S: Neden kalan kredilerimi sonra bir yeniden dağıtın veya durdurmak/başlatmak 0 olarak ayarlanır?
**A** : Olduğunda bir VM "REDPLOYED" ve sanal Makineyi başka bir düğüme taşır birikmiş kredi kaybolur. VM durduruldu ve başlatıldı ancak aynı düğümde kalır, VM birikmiş kredi korur. Sanal makine bir düğüm üzerinde yeni başlatıldığında bir başlangıç kredisi alır, Standard_B8ms için 240 dakika.
    
### <a name="q-what-happens-if-i-deploy-an-unsupported-os-image-on-b1ls"></a>S: Desteklenmeyen bir işletim sistemi görüntüsüne B1ls üzerinde dağıtabilirim ne olur?
**A** : B1ls yalnızca Linux görüntüleri destekler ve herhangi başka bir işletim sistemi görüntüsünü dağıtırsanız, en iyi müşteri deneyimi alamayabilirsiniz.
    
### <a name="q-why-is-there-no-pricing-information-for-b1ls-windows"></a>S: Neden B1ls windows için fiyatlandırma bilgisi var mı?
**A** : B1ls yalnızca Linux görüntüleri destekler ve herhangi başka bir işletim sistemi görüntüsünü dağıtırsanız, en iyi müşteri deneyimi alamayabilirsiniz ancak tutarında olur.


    

    
