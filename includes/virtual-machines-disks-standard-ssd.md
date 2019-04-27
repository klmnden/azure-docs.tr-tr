---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 08/14/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: da46687517dbfe189571286087d4ef29d50d1246
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60564290"
---
# <a name="standard-ssd-managed-disks-for-azure-virtual-machine-workloads"></a>Standart SSD yönetilen diskler için Azure sanal makine iş yükleri

Azure standart katı hal sürücüleri (SSD) yönetilen diskler, daha düşük IOPS düzeylerinde tutarlı bir performans gerektiren iş yükleri için en iyi duruma getirilmiş düşük maliyetli depolama bir seçenektir. Standart SSD, özellikle varyansını HDD çözümlerinizi şirket içinde çalışan iş yükleri ile ilgili sorun yaşıyorsanız kullanıcıların buluta taşımak istediğiniz iyi giriş düzeyi deneyimi sunar. Standart SSD daha iyi kullanılabilirlik, tutarlılık, güvenilirlik ve HDD disklere karşılaştırıldığında gecikme süresi sunar ve Web sunucuları, düşük IOPS uygulama sunucuları, az kullanılan kurumsal uygulamalar ve geliştirme/Test iş yükleri için uygundur.

## <a name="standard-ssd-features"></a>Standart SSD özellikleri

**Yönetilen diskler**: Standart SSD, yalnızca yönetilen diskler kullanılabilir. Yönetilmeyen diskler ve sayfa Blobları standart SSD üzerinde desteklenmez. Yönetilen diski oluştururken standart SSD disk türünü belirtin ve boyutu belirtmek duyduğunuz disk ve Azure oluşturur ve diski oluşturup yönetebilmesi.
Standart SSD yönetilen diskler tarafından sunulan tüm Klasik dağıtım modeli işlemlerini destekler. Örneğin, oluşturma, kopyalama veya aynı şekilde standart SSD yönetilen diskler anlık görüntü yönetilen diskler ile yapın.

**Sanal makineler**: Standart SSD Azure Premium diskleri desteklemiyor VM türleri dahil olmak üzere bulunduğu tüm VM'ler, kullanılabilir. Örneğin, bir A serisi VM veya N serisi VM'LERİNDEN ve DS serisi, kullanıyorsanız veya tüm diğer Azure VM serisi, bu VM ile standart bir SSD kullanabilirsiniz. Standart SSD sunulmasıyla birlikte, çok sayıda önceden SSD tabanlı disklere geçişi ve tutarlı bir performans, yüksek kullanılabilirlik, daha iyi gecikme süresi ve genel bir daha iyi deneyimi için HDD tabanlı diskler kullanan iş yükleri etkinleştirdik SSD'ler ile verilen karşılaşırsınız.

**Yüksek oranda dayanıklı ve kullanılabilir**: Standart SSD yüksek kullanılabilirlik ve dayanıklılığı diskleri için tutarlı bir şekilde teslim ettiğini aynı Azure diskleri platformda yerleşik olarak bulunur. Azure diskleri yüzde 99,999 kullanılabilirlik için tasarlanmıştır. Tüm yönetilen diskler gibi standart SSD'ler de yerel olarak yedekli depolama (LRS) sunar. LRS, platform her disk için verilerin birden çok kopyasını tutar ve kurumsal düzeyde tutarlı bir şekilde teslim ettiğini dayanıklılık düzeyi sunmak için Iaas diskleri, endüstri lideri ile yüzde sıfır değer yıllık hata oranı.

**Anlık görüntüleri**: Standart SSD, tüm yönetilen diskler gibi anlık görüntü oluşturmaya da destekler. Anlık görüntü türü veya standart (HDD), hem de Premium (SSD) olabilir. Maliyet tasarrufu için tüm Azure disk türleri için anlık görüntü türü, standart (HDD) öneririz. Anlık görüntüden yönetilen disk oluşturma, size her zaman standart SSD veya Premium SSD gibi daha yüksek bir katmana seçebilir olmasıdır.

## <a name="scalability-and-performance-targets"></a>Ölçeklenebilirlik ve performans hedefleri

Aşağıdaki tabloda, şu anda sunulan için standart SSD disk boyutları içerir. Bir yıldız işaretiyle gösterilen boyutları şu anda Önizleme aşamasındadır.

|Standart SSD Disk türü  |Disk Boyutu  |Disk başına IOPS  |Disk başına aktarım hızı  |
|---------|---------|---------|---------|
|E4     |32 GiB         |En fazla 120         |Saniye başına en fazla 25 MiB         |
|E6     |64 GiB         |En fazla 240         |Saniye başına en fazla 50 MiB         |
|E10     |128 GiB         |En fazla 500         |Saniye başına en fazla 60 MiB         |
|E15     |256 GiB         |En fazla 500         |Saniye başına en fazla 60 MiB         |
|E20     |512 GiB         |En fazla 500         |Saniye başına en fazla 60 MiB         |
|E30     |1.024 giB       |En fazla 500         |Saniye başına en fazla 60 MiB         |
|E40     |2.048 giB       |En fazla 500         |Saniye başına en fazla 60 MiB         |
|E50     |4.095 giB       |En fazla 500         |Saniye başına en fazla 60 MiB         |
|E60 *     |8192 giB       |En fazla 1.300       |Saniye başına en fazla 300 MiB        |
|E70 *    |16.384 giB      |En fazla 2.000       |Saniye başına en fazla 500 MiB        |
|E80 *    |32.767 giB      |En fazla 2.000       |Saniye başına en fazla 500 MiB        |

Standart SSD GÇ işlemlerinin çoğu için Tek haneli milisaniyelik gecikme süreleri sağlamak ve yukarıdaki tabloda örneklerin % 99 oranında açıklanan sınırlara kadar aktarım hızı ve IOPS sunmak için tasarlanmıştır. Bazen gerçek IOPS ve aktarım hızı trafik düzenlerini bağlı olarak değişiklik gösterebilir. Standart SSD daha düşük gecikme süresiyle HDD diskleri daha tutarlı performans sağlar.

Premium SSD diğer taraftan, daha iyi standart SSD'ler, düşük gecikme, yüksek IOPS/aktarım hızı ve sağlanan disk performansı daha da iyi tutarlılık gerçekleştirin. Premium SSD kritik üretim iş yükleri için önerilen disk türüdür. İş yükünüz yüksek performanslı ve düşük gecikme süreli disk desteği gerektiriyorsa, Premium depolama kullanmayı düşünmeniz gerekir.

Standart SSD, Premium SSD gibi 256 KiB GÇ birim boyutunu da kullanın. Aktarılan veriler en fazla 256 KiB ise, 1 g/ç birim olarak kabul edilir. Daha büyük g/ç boyutları olarak birden çok g/ç boyutu 256 sayılır KiB. Örneğin, 1.100 KiB g/ç beş g/ç birimleri kabul edilir.

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama

Standart SSD kullanırken aşağıdaki fatura değerlendirmeleri geçerlidir:

- Yönetilen disk boyutu
- Anlık Görüntüler
- Giden veri aktarımları
- İşlemler

**Yönetilen disk boyutu**: Yönetilen diskler üzerinde sağlanan boyut faturalandırılır. Azure sağlanan boyutu (yukarı yuvarlayarak) en yakın disk boyutu teklifiyle eşleştirir. Sağlanan disk boyutları Ayrıntılar için ölçeklenebilirlik ve performans hedefleri bölümde yukarıdaki tabloya bakın. Her disk, desteklenen sağlanan disk boyutuna eşlenir ve buna göre faturalandırılır. Örneğin, bir 200 GiB standart SSD sağladıysanız, E15 disk boyutu teklifine eşlersiniz (256 GiB). Sağlanmış bir diski için faturalama, saatlere Premium depolama teklif için aylık fiyat kullanılarak eşit olarak dağıtılır. E10 bir disk sağlanır ve 20 saat sonra silindi, için E10 teklif 20 saat eşit olarak dağıtılır. Örneğin, faturalandırılırsınız. Diske yazılan gerçek veri miktarından bağımsız olarak budur.

**Anlık görüntüleri**: Yönetilen diskler, anlık görüntüler varsa anlık görüntüleri, hedef ve kaynak tarafından kullanılan kapasite için faturalandırılırsınız. Anlık görüntüleri hakkında daha fazla bilgi için bkz. [yönetilen Disk anlık görüntülerini](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview#managed-disk-snapshots).

**Giden veri aktarımları**: [Giden veri aktarımları](https://azure.microsoft.com/pricing/details/bandwidth/) (Azure veri merkezlerinden çıkan veriler) bant genişliği kullanımı için fatura doğurur.

**İşlem**: Benzer şekilde standart HDD, faturalandırma işlemleri standart ssd'lerde yansıtılmaz. İşlemler hem okuma hem de içerir ve diskteki yazma. Standart SSD üzerinde işlem hesaplama için kullanılan g/ç birim boyutu 256 KiB ' dir. Daha büyük g/ç boyutları olarak birden çok g/ç boyutu 256 sayılır KiB.

Sanal makineler ve yönetilen diskler için fiyatlandırma hakkında daha fazla bilgi için bkz:

- [Sanal makineleri fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-machines/linux/)
- [Yönetilen diskler fiyatlandırması](https://azure.microsoft.com/pricing/details/managed-disks/)

## <a name="next-steps"></a>Sonraki adımlar

Standart SSD oluşturma hakkında daha fazla bilgi edinmek için birden çok standart SSD'ler ile VM oluşturma işlemini gösteren örnek bakın.

> [!div class="nextstepaction"]
> [Birden çok standart SSD'ler ile VM oluşturma](https://github.com/azure/azure-quickstart-templates/tree/master/101-vm-with-standardssd-disk/)
