---
title: include dosyası
description: include dosyası
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 04/17/2019
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 17806abe60236a9c9face1ee16e1d9982975fff6
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64734696"
---
Depolama en iyi duruma getirilmiş VM boyutları, yüksek disk aktarım hızı ve g/ç sunar ve büyük veri, SQL, NoSQL veritabanları, veri ambarı ve büyük işlem veritabanları için idealdir.  Cassandra, MongoDB, Cloudera ve Redis örneklerindendir. Bu makalede, Vcpu, veri diskleri ve NIC hem de yerel depolama aktarım hızı ve ağ bant genişliği için en iyi duruma getirilmiş her boyut sayısı hakkında bilgi sağlar.

Lsv2 serisi özellikleri yüksek aktarım hızı, düşük gecikme süresi, doğrudan yerel NVMe depolama üzerinde çalışan eşlenen [AMD EPYC &trade; 7551 İşlemci](https://www.amd.com/en/products/epyc-7000-series) 2.55 GHz ve en fazla bir boost 3.0 GHz, tüm bir çekirdek boost ile. Lsv2 serisi VM’ler, eş zamanlı bir çoklu iş parçacığı yapılandırmasında 8 ile 80 vCPU arasında değişen boyutlarda sunulur.  vCPU başına 8 GiB bellek ve 8 vCPU başına 1,92 TB NVMe SSD M.2 cihazı sunulurken, L80s v2 adlı en üst model 19,2 TB (10x1,92 TB) depolama içerir.

> [!NOTE]
> Lsv2 serisi VM'ler, kalıcı veri diskleri kullanmak yerine doğrudan sanal Makineye bağlı bir düğümde yerel diski kullanacak şekilde iyileştirilmiştir. Böylece büyük IOPS / aktarım hızı iş yükleriniz için. Ls serisi ve Lsv2 kalıcı veri diskleri tarafından ulaşılabilir IOPS artırmak için yerel bir önbellek oluşturulmasını desteklemez.
>
> Yüksek aktarım hızı ve yerel disk IOPS sağlar Ls serisi VM'ler ve Lsv2 Apache Cassandra ve MongoDB gibi tek bir VM bir arıza olması durumunda kalıcılığı sağlamak için birden çok VM arasında veri çoğaltmak NoSQL depoları için idealdir.
>
> Daha fazla bilgi için bkz. [Lsv2 serisi sanal makineler üzerinde performansı en iyi duruma](../articles/virtual-machines/linux/storage-performance.md).  


## <a name="lsv2-series"></a>Lsv2 serisi

ACU: 150-175

Premium Depolama: Desteklenen

Premium depolama önbelleğe alma: Desteklenmiyor

| Boyut          | Sanal işlemci | Bellek (GiB) | Geçici disk<sup>1</sup> (GiB) | NVMe diskleri<sup>2</sup> | NVMe Disk aktarım hızı<sup>3</sup> (okuma IOPS / MB/sn) | Maksimum önbelleğe alınmamış veri diski aktarım hızı (IOPS/MB/sn)<sup>4</sup> | Maksimum veri diskleri | Maks NIC / beklenen ağ bant genişliği (MB/sn) |
|---------------|-----------|-------------|--------------------------|----------------|---------------------------------------------------|-------------------------------------------|------------------------------|------------------------------| 
| Standard_L8s_v2   |  8 |  64 |  80 |  1x1.92 TB  | 400,000 / 2,000 | 8,000/160 | 16 | 2 / 3,200  | 
| Standard_L16s_v2  | 16 | 128 | 160 |  2x1.92 TB  | 800,000 / 4,000 | 16,000/320 | 32 | 4 / 6,400  | 
| Standard_L32s_v2  | 32 | 256 | 320 |  4x1.92 TB  | 1,5 MİLYON / 8000    | 32,000/640 | 32 | 8 / 12,800 | 
| Standard_L64s_v2  | 64 | 512 | 640 |  8x1.92 TB  | 2.9 M / 16.000   | 64,000/1,280 | 32 | 8 / 25,600 |
| Standard_L80s_v2  | 80 | 640 | 800 | 10x1.92TB   | 3.8 M / 20.000   | 80,000/1,400 | 32 | 8 / 32,000 |

<sup>1</sup> Lsv2 serisi VM'ler, standart bir SCSI tabanlı geçici kaynak disk için işletim sistemi disk belleği/takas dosyası kullanımı (Windows, Linux üzerinde /dev/sdb D:) sahip. 80 MB/sn aktarım hızı (örneğin Standard_L80s_v2 800 GiB 40.000 IOPS ve 800 MB/sn sağlar) her 8 Vcpu için ve bu disk, depolama, 80 GiB, 4000 IOPS sağlar. Bu, NVMe sürücüler tam uygulama kullanımı için ayrılmış sağlar. Kısa ömürlü bu diskidir ve Durdur/serbest tüm veriler kaybolur.

<sup>2</sup> yerel NVMe diskleri kısa ömürlü, veriler, Durdur /, VM'yi serbest bırakın, bu diskler üzerindeki kaybolacak.

<sup>3</sup> NVMe doğrudan Hyper-V teknolojisi Konuk VM alanı güvenli bir şekilde eşlenen yerel NVMe sürücüler kısıtlanmamışsa erişim sağlar.  En yüksek performansı elde etmek için en son WS2019 derleme veya Ubuntu 18.04 veya 16.04 Azure marketi'ndeki gerektirir.  Yazma performansı g/ç boyutu, sürücü yükleme ve kapasite kullanımı göre değişir.

<sup>4</sup> Lsv2 serisi VM'ler Lsv2 iş yükleri fayda gibi veri diski için konak önbelleği sağlamaz.  Ancak, Lsv2 Vm'leri, Azure'nın kısa ömürlü VM işletim sistemi disk seçeneği (30 GiB) barındırabilir.

## <a name="size-table-definitions"></a>Boyut tablosu tanımları

- Depolama kapasitesi GiB veya 1024^3 bayt cinsinden gösterilmiştir. GB (1000^3 bayt) ile ölçülen diskleri GiB (1024^3 bayt) ile ölçülen disklerle karşılaştırırken GiB cinsinden verilen kapasite rakamlarının daha küçük görünebileceğini unutmayın. Örneğin: 1023 GiB = 1098,4 GB
- Disk aktarım hızı, saniye başına giriş/çıkış işlemi sayısı (IOPS) ve MB/sn (MB/sn = 10^6 bayt/sn) üzerinden ölçülür.
- Sanal makineleriniz için en iyi performansı elde etmek istiyorsanız, veri diskleri için vCPU başına 2 disk sayısını sınırlamanız gerekir.
- **Beklenen ağ bant genişliği** en büyük toplu [VM türü ayrılan bant genişliği](../articles/virtual-network/virtual-machine-network-throughput.md) tüm hedefler için tüm NIC'ler arasında. Üst sınırlar garanti edilmez, ancak hedeflenen uygulama için doğru VM türünün seçilmesine ilişkin kılavuzluk sağlamak için tasarlanmıştır. Gerçek ağ performansı, ağ tıkanıklığı, uygulama yükleri ve ağ ayarları gibi çeşitli faktörlere bağlıdır. Ağ aktarım hızını iyileştirme hakkında bilgi için bkz. [Windows ve Linux için ağ aktarım hızını iyileştirme](../articles/virtual-network/virtual-network-optimize-network-bandwidth.md). Linux veya Windows üzerinde beklenen ağ performansını gerçekleştirmek için belirli bir sürümü seçmeniz veya VM’nizi en iyi duruma getirmeniz gerekebilir. Daha fazla bilgi için bkz. [Sanal makine aktarım hızını güvenilir bir şekilde test etme](../articles/virtual-network/virtual-network-bandwidth-testing.md).
