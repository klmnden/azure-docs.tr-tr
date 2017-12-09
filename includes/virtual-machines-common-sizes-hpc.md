
A8-A11 ve H Serisi boyutlar *yoğun işlem gücü kullanımlı örnekler* olarak da bilinir. Bu boyutları çalıştıran donanım; yüksek performanslı bilgi işlem (HPC) kümesi uygulamaları, modellemeler ve simülasyonlar gibi yoğun işlem ve ağ kullanımlı uygulamalar için tasarlanmış ve iyileştirilmiştir. A8-A11 Serisinde, Intel Xeon E5-2670 @ 2,6 GHZ, H Serisinde ise Intel Xeon E5-2667 v3 @ 3,2 GHz işlemciler kullanılmaktadır.  Bu makale Vcpu, veri diskleri ve NIC yanı sıra bu gruplandırmadaki her boyutu için depolama üretilen iş ve ağ bant sayısı hakkında bilgi sağlar. 

Azure H Serisi sanal makineler, moleküler modelleme ve hesaplamalı akışkanlar dinamiği gibi üst düzey işlem hesaplama gereksinimlerine hitap eden yeni nesil yüksek performanslı bilgi işlem VM'leridir. Bu 8 ve 16 vCPU VM'ler Intel Haswell E5-2667 V3 işlemci teknolojisi ekranlarını DDR4 bellek ve SSD tabanlı geçici depolama üzerinde oluşturulmuştur. 

H Serisi önemli miktarda CPU gücünün yanı sıra, FDR InfiniBand ile düşük gecikmeli RDMA ağ iletişimi için farklı seçeneklere ek olarak yoğun bellek kullanımlı işlem gereksinimlerini için çok sayıda bellek yapılandırması sunar.



## <a name="h-series"></a>H Serisi

ACU: 290-300

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum diski aktarım hızı: IOPS | En fazla NIC |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_H8 |8 |56 |1000 |32 |32x500 |2  |
| Standard_H16 |16 |112 |2000 |64 |64x500 |4 |
| Standard_H8m |8 |112 |1000 |32 |32x500 |2  |
| Standard_H16m |16 |224 |2000 |64 |64x500 |4  |
| Standart_h16r <sup>1</sup> |16 |112 |2000 |64 |64x500 |4  |
| Standart_h16mr <sup>1</sup> |16 |224 |2000 |64 |64x500 |4 |

<sup>1</sup> MPI uygulamaları için ayrılmış RDMA arka uç ağ çok düşük gecikme ve yüksek bant genişliği sunar FDR InfiniBand ağ tarafından etkinleştirilir.

<br>



## <a name="a-series---compute-intensive-instances"></a>A Serisi - Yoğun işlem gücü kullanımlı örnekler

ACU: 225

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (HDD): GiB | Maksimum veri diskleri | Maksimum veri diski aktarım hızı: IOPS | En fazla NIC|
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A8 <sup>1</sup> |8 |56 |382 |32 |32x500 |2 |
| Standard_A9 <sup>1</sup> |16 |112 |382 |64 |64 x 500 |4 |
| Standard_A10 |8 |56 |382 |32 |32x500 |2  |
| Standard_A11 |16 |112 |382 |64 |64 x 500 |4 |

<sup>1</sup>MPI uygulamaları için ayrılmış RDMA arka uç ağ çok düşük gecikme ve yüksek bant genişliği sunar FDR InfiniBand ağ tarafından etkinleştirilir.

<br>



