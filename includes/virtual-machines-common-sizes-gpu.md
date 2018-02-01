
GPU en iyi duruma getirilmiş VM ile tek veya birden çok NVIDIA GPU kullanılabilir özelleştirilmiş sanal makineler boyutlarıdır. Bu boyutlar işlem yoğunluklu, grafik yoğun ve görselleştirme iş yükleri için tasarlanmıştır. Bu makale numarası ve gpu, Vcpu, veri diskleri ve NIC yanı sıra depolama üretilen iş ve ağ bant genişliğini her boyutu Bu gruplandırma türü hakkında bilgi sağlar. 

* **NC, NCv2 ve ND** boyutları işlem yoğunluklu ve ağ yoğunluklu uygulamalar ve CUDA ve OpenCL tabanlı uygulamalar ve benzetimleri, AI ve derin öğrenme gibi algoritmaları için iyileştirilmiştir. 
* **NV** boyutları en iyi duruma getirilmiş ve uzak görselleştirme, akış, oyun, kodlama ve OpenGL ve DirectX gibi çerçeveler kullanılarak VDI senaryoları için tasarlanmıştır.  


## <a name="nc-instances"></a>NC Örnekleri

NC örnekleri tarafından sağlanmıştır [NVIDIA Tesla K80](http://images.nvidia.com/content/pdf/kepler/Tesla-K80-BoardSpec-07317-001-v05.pdf) kart. Kullanıcılar enerji araştırması uygulamalar için CUDA yararlanarak daha hızlı verilerine uğraşmaları, benzetimleri kilitlenme, izlenen işleme, derin öğrenme ve daha fazlasını ışın. NC24r yapılandırma, düşük gecikme, sıkı şekilde bağlı paralel bilgi işlem iş yükleri için en iyi duruma getirilmiş yüksek verimlilik ağ arabirimi sağlar.


| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | En fazla veri diski |
| --- | --- | --- | --- | --- | --- |
| Standard_NC6 |6 |56 | 380 | 1 | 24 |
| Standard_NC12 |12 |112 | 680 | 2 | 48 |
| Standard_NC24 |24 |224 | 1440 | 4 | 64 |
| Standard_NC24r* |24 |224 | 1440 | 4 | 64 |

1 GPU = K80 kartın yarısı.

*RDMA özellikli

## <a name="ncv2-instances"></a>NCv2 örnekleri

NCv2 tarafından sağlanmıştır NC-serisi makineler, yeni nesil örnekleridir [NVIDIA Tesla P100](http://images.nvidia.com/content/tesla/pdf/nvidia-tesla-p100-datasheet.pdf) GPU. Bu GPU hesaplama performans geçerli NC serinin x 2'den fazla sağlayabilir. Müşteriler bu güncelleştirilmiş GPU rezervuarı modelleme, DNA sıralama, protein analiz, Monte Carlo benzetimleri ve diğerleri gibi geleneksel HPC iş yükleri için yararlanabilir. NC serisi gibi düşük gecikme, sıkı şekilde bağlı paralel bilgi işlem iş yükleri için en iyi duruma getirilmiş yüksek verimlilik ağ arabirimi ile bir yapılandırma NCv2 serisi sunar.

> [!IMPORTANT]
> Bu boyut ailesi için aboneliğinizde vCPU (Temel) kota başlangıçta her bölgede 0 olarak ayarlanır. [VCPU kota artışı isteği](../articles/azure-supportability/resource-manager-core-quotas-request.md) bu ailesi için bir [kullanılabilir bölge](https://azure.microsoft.com/regions/services/).
>

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | En fazla veri diski |
| --- | --- | --- | --- | --- | --- |
| Standard_NC6s_v2 |6 |112 | 336 | 1 | 12 |
| Standard_NC12s_v2 |12 |224 | 672 | 2 | 24 |
| Standard_NC24s_v2 |24 |448 | 1344 | 4 | 32 |
| Standard_NC24rs_v2* |24 |1448 | 1344 | 4 | 32 |

1 GPU = bir P100 kart.

*RDMA özellikli

## <a name="nd-instances"></a>ND örnekleri

ND-serisi sanal makine AI ve derin öğrenme iş yükleri için tasarlanmış GPU ailesi için yeni bir toplama yok. Bunlar, eğitim ve çıkarım için mükemmel performans sunar. Tarafından desteklenir ve örnekleri [NVIDIA Tesla P40](http://images.nvidia.com/content/pdf/tesla/184427-Tesla-P40-Datasheet-NV-Final-Letter-Web.pdf) GPU. Bu örnekler tek duyarlıklı kayan nokta işlemleri için Microsoft Bilişsel araç seti, TensorFlow, Caffe ve diğer çerçeveler kullanılarak AI iş yükleri için mükemmel performans sağlar. Ayrıca, ND serisi çok daha büyük bir GPU bellek boyutu (24 GB) sunarak daha büyük sinir ağı modellerinin sığdırılmasına imkan tanır. NC serisi gibi ND-ikincil düşük gecikmeli, yüksek verimlilik ağ RDMA üzerinden bir yapılandırmayla sunar ve büyük ölçekli eğitim işleri birçok GPU kapsayıcı çalıştırabilmeniz için InfiniBand bağlantı.

> [!IMPORTANT]
> Bu boyut ailesi için aboneliğinizi bölgede başına vCPU (Temel) kota başlangıçta 0 olarak ayarlanır. [VCPU kota artışı isteği](../articles/azure-supportability/resource-manager-core-quotas-request.md) bu ailesi için bir [kullanılabilir bölge](https://azure.microsoft.com/regions/services/).
>

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | En fazla veri diski |
| --- | --- | --- | --- | --- | --- |
| Standard_ND6s |6 |112 | 336 | 1 | 12 |
| Standard_ND12s |12 |224 | 672 | 2 | 24 |
| Standard_ND24s |24 |448 | 1344 | 4 | 32 |
| Standard_ND24rs * |24 |1448 | 1344 | 4 | 32 |

1 GPU = bir P40 kart.

*RDMA özellikli

## <a name="nv-instances"></a>NV Örnekleri



NV örnekleri tarafından sağlanmıştır [NVIDIA Tesla M60 ](http://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) GPU ve NVIDIA kılavuz Masaüstü teknolojinin hızlandırılmış uygulamalar ve sanal masaüstlerini müşteriler, veri veya benzetimleri görselleştirmek mümkün olduğu. Kullanıcılar kendi grafik yoğun iş akışları üstün grafik yeteneği almak ve ayrıca kodlama ve işleme gibi tek duyarlıklı iş yüklerini çalıştırmak için NV örneklerinde görselleştirmek kullanabilirsiniz. 

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | En fazla veri diski |
| --- | --- | --- | --- | --- | --- |
| Standard_NV6 |6 |56 |380 | 1 | 24 |
| Standard_NV12 |12 |112 |680 | 2 | 48 |
| Standard_NV24 |24 |224 |1440 | 4 | 64 |

1 GPU = M60 kartın yarısı.


