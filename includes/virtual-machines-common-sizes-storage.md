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
ms.openlocfilehash: 840d3737efe4314359ba3a3bf0f5c4f888f92567
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36329578"
---
Depolama en iyi duruma getirilmiş VM boyutları yüksek disk işleme ve g/ç sağlar ve büyük veri, SQL ve NoSQL veritabanları için idealdir. Bu makale Vcpu, veri diskleri ve NIC yanı sıra bu gruplandırmadaki her boyutu için depolama üretilen iş ve ağ bant sayısı hakkında bilgi sağlar. 

Ls serisi, [Intel® Xeon İşlemci E5 v3 ailesi](http://www.intel.com/content/www/us/en/processors/xeon/xeon-e5-solutions.html) ile 32’ye kadar vCPU kullanım olanağı sunar. Ls serisi, G/GS serisi ile aynı CPU performansı sunar ve her vCPU başına 8 GiB bellek içerir.  

## <a name="ls-series"></a>Ls serisi

ACU: 180-240
 
| Boyut          | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | En büyük geçici depolama verimliliğini: IOP / MB/sn | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | Max NIC'ler / beklenen ağ bant genişliği (Mbps) | 
|---------------|-----------|-------------|--------------------------|----------------|-------------------------------------------------------------|-------------------------------------------|------------------------------| 
| Standart_L4s   | 4    | 32   | 678   | 16    | 20,000 / 200   | 5000/125        | 2 / 4,000  | 
| Standart_L8s   | 8    | 64   | 1,388 | 32   | 40,000 / 400   | 10.000/250       | 4 / 8,000  | 
| Standart_L16s  | 16   | 128  | 2,807 | 64   | 80,000 / 800   | 20.000/500       | 8 / 16,000 | 
| Standard_L32s <sup>1</sup> | 32   | 256  | 5,630 | 64   | 160,000 / 1,600   | 40.000/1000     | 8 / 20,000 | 
 

Ls-serisi VM'ler ile olası en fazla disk verimlilik olabilir sayısı, boyutu ve şeritleme herhangi biri tarafından sınırlandırılır bağlı diskler. Ayrıntılar için bkz. [Premium Depolama: Azure sanal makine iş yükleri için yüksek performanslı depolama](../articles/virtual-machines/windows/premium-storage.md). En yüksek yerel depolama kullanımı ile iş yüklerini ls-serisi VM'ler hedeflenen ve genellikle Ls-serisi desteklemediği bu gibi durumlarda etkisiz diskleri bağlı kullanmak için ilk yükleme ve önbelleğe alma olarak günlüğe kaydetme, yalnızca ana bilgisayar için önbelleğe almayı diskleri ekli, diskler bağlı olması gerekir önbelleğe alınmamış modda. 

<sup>1</sup> örneği için tek bir müşteriye ayrılmış donanım için ayrılmış.

## <a name="size-table-definitions"></a>Boyut tablosu tanımları

- Depolama kapasitesi GiB veya 1024^3 bayt cinsinden gösterilmiştir. GB (1000^3 bayt) ile ölçülen diskleri GiB (1024^3 bayt) ile ölçülen disklerle karşılaştırırken GiB cinsinden verilen kapasite rakamlarının daha küçük görünebileceğini unutmayın. Örneğin: 1023 GiB = 1098,4 GB
- Disk aktarım hızı, saniye başına giriş/çıkış işlemi sayısı (IOPS) ve MB/sn (MB/sn = 10^6 bayt/sn) üzerinden ölçülür.
- Ls-serisi veri diskleri, önbelleğe alınmış modda çalışamıyor, konak önbelleği modu ayarlamak **hiçbiri**.
- Vm'leriniz için en iyi performansı elde etmek istiyorsanız, veri diskleri vCPU başına 2 disklere sayısını sınırlamanız gerekir.
- **Beklenen ağ bant genişliği** maksimum toplanır [başına VM türü ayrılan bant genişliği](../articles/virtual-network/virtual-machine-network-throughput.md) tüm hedefler için tüm NIC'ler arasında. Üst sınırlar garanti edilmez, ancak hedeflenen uygulama için doğru VM türünün seçilmesine ilişkin kılavuzluk sağlamak için tasarlanmıştır. Gerçek ağ performansı, ağ tıkanıklığı, uygulama yükleri ve ağ ayarları gibi çeşitli faktörlere bağlıdır. Ağ aktarım hızını iyileştirme hakkında bilgi için bkz. [Windows ve Linux için ağ aktarım hızını iyileştirme](../articles/virtual-network/virtual-network-optimize-network-bandwidth.md). Linux veya Windows üzerinde beklenen ağ performansını gerçekleştirmek için belirli bir sürümü seçmeniz veya VM’nizi en iyi duruma getirmeniz gerekebilir. Daha fazla bilgi için bkz. [Sanal makine aktarım hızını güvenilir bir şekilde test etme](../articles/virtual-network/virtual-network-bandwidth-testing.md).
