
NC ve NV boyutları, GPU özellikli örnekler olarak da bilinir. NVIDIA GPU kartlarına sahip bu özel amaçlı sanal makineler, farklı senaryolar ve kullanım amaçları için iyileştirilmiştir. NV boyutları uzaktan görselleştirme, içerik akışı, oyun ve kodlamanın yanı sıra OpenGL ve DirectX gibi çerçeveleri kullanan VDI senaryoları için tasarlanmış ve iyileştirilmiştir. NC boyutları ise CUDA ve OpenCL tabanlı uygulama ve simülasyonlar gibi yoğun işlem ve ağ kullanımlı uygulamalar ve algoritmalar için iyileştirilmiştir. 


NVIDIA'nın Tesla M60 GPU kartının yanı sıra hızlandırılmış masaüstü uygulamaları ve sanal masaüstü düzenleri için NVIDIA GRID özelliğine sahip olan NV örnekleri, müşterilerin verilerini veya simülasyonlarını görselleştirmek için kullanabileceği sistemlerdir. Kullanıcılar yoğun grafik kullanımlı iş akışlarını NV örneklerinde görselleştirerek üstün grafik özelliğine sahip olurken kodlama ve işleme gibi hassas iş yüklerini de tek başına çalıştırabilir. Tesla M60, çift GPU tasarımda 4096 CUDA çekirdeği ve en fazla 36 adet 1080p H.264 akışı sunar. 

NC örnekleri, NVIDIA Tesla K80 kartını kullanmaktadır. Kullanıcılar artık enerji keşif uygulamaları, kaza simülasyonları, ışın izlemeli işleme, derin öğrenme ve çok daha fazlası için CUDA yardımıyla verileri çok daha hızlı işleyebilir. Tesla K80, çift GPU tasarımında 4992 CUDA çekirdeği, 2,91 Teraflop'a kadar çift duyarlıklı ve 8,93 Teraflop'a kadar tek duyarlıklı performans sunar.

## <a name="nv-instances"></a>NV Örnekleri

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | En fazla veri diski |
| --- | --- | --- | --- | --- | --- |
| Standard_NV6 |6 |56 |380 | 1 | 8 |
| Standard_NV12 |12 |112 |680 | 2 | 16 |
| Standard_NV24 |24 |224 |1440 | 4 | 32 |

1 GPU = M60 kartın yarısı.

## <a name="nc-instances"></a>NC Örnekleri

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | En fazla veri diski |
| --- | --- | --- | --- | --- | --- |
| Standard_NC6 |6 |56 | 380 | 1 | 8 |
| Standard_NC12 |12 |112 | 680 | 2 | 16 |
| Standard_NC24 |24 |224 | 1440 | 4 | 32 |
| Standard_NC24r* |24 |224 | 1440 | 4 | 32 |

1 GPU = K80 kartın yarısı.

*RDMA özellikli


