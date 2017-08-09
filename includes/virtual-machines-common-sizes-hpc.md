<!-- A-series - compute-intensive instances, H-series -->

A8-A11 ve H Serisi boyutlar *yoğun işlem gücü kullanımlı örnekler* olarak da bilinir. Bu boyutları çalıştıran donanım; yüksek performanslı bilgi işlem (HPC) kümesi uygulamaları, modellemeler ve simülasyonlar gibi yoğun işlem ve ağ kullanımlı uygulamalar için tasarlanmış ve iyileştirilmiştir. A8-A11 Serisinde, Intel Xeon E5-2670 @ 2,6 GHZ, H Serisinde ise Intel Xeon E5-2667 v3 @ 3,2 GHz işlemciler kullanılmaktadır. 

Azure H Serisi sanal makineler, moleküler modelleme ve hesaplamalı akışkanlar dinamiği gibi üst düzey işlem hesaplama gereksinimlerine hitap eden yeni nesil yüksek performanslı bilgi işlem VM'leridir. Intel Haswell E5-2667 V3 işlemci teknolojisini kullanan bu 8 ve 16 vCPU VM'ler, DDR4 belleğe ve SSD tabanlı geçici depolamaya sahiptir. 

H Serisi önemli miktarda CPU gücünün yanı sıra, FDR InfiniBand ile düşük gecikmeli RDMA ağ iletişimi için farklı seçeneklere ek olarak yoğun bellek kullanımlı işlem gereksinimlerini için çok sayıda bellek yapılandırması sunar.



## <a name="h-series"></a>H Serisi

ACU: 290-300

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum diski aktarım hızı: IOPS | En fazla NIC |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_H8 |8 |56 |1000 |16 |16x500 |2  |
| Standard_H16 |16 |112 |2000 |32 |32x500 |4 |
| Standard_H8m |8 |112 |1000 |16 |16x500 |2  |
| Standard_H16m |16 |224 |2000 |32 |32x500 |4  |
| Standard_H16r* |16 |112 |2000 |32 |32x500 |4  |
| Standard_H16mr* |16 |224 |2000 |32 |32x500 |4 |

*MPI uygulamaları için, adanmış RDMA arka uç ağı ultra düşük gecikme süresi ve yüksek bant genişliği sağlayan FDR InfiniBand ağı tarafından etkinleştirilir.

<br>



## <a name="a-series---compute-intensive-instances"></a>A Serisi - Yoğun işlem gücü kullanımlı örnekler

ACU: 225

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (HDD): GiB | Maksimum veri diskleri | Maksimum veri diski aktarım hızı: IOPS | En fazla NIC|
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A8* |8 |56 |382 |16 |16x500 |2 |
| Standard_A9* |16 |112 |382 |16 |16x500 |4 |
| Standard_A10 |8 |56 |382 |16 |16x500 |2  |
| Standard_A11 |16 |112 |382 |16 |16x500 |4 |

*MPI uygulamaları için, adanmış RDMA arka uç ağı ultra düşük gecikme süresi ve yüksek bant genişliği sağlayan FDR InfiniBand ağı tarafından etkinleştirilir.

<br>



