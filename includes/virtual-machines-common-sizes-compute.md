<!-- F-series, Fs-series* -->

F Serisi, Intel Turbo Boost Technology 2.0 ile 3,1 GHz'e varan saat hızlarına ulaşabilen 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell) işlemciyi temel alır. Bu değer Dv2 Serisi VM'lerdeki CPU performansıyla aynıdır.  Daha düşük bir saatlik liste fiyatına sahip olan F Serisi, vCPU başına Azure İşlem Birimi (ACU) açısından fiyat-performans alanında Azure portföyündeki en iyi seçenektir. 

F Serisi VM'ler, daha hızlı CPU'ya ihtiyaç duyan ancak çok fazla belleğe veya vCPU başına geçici depolamaya ihtiyaç duymayan iş yükleri için harika bir seçenektir.  Analitikler, oyun sunucuları, web sunucuları ve toplu işleme gibi iş yükleri, F Serisinin özelliklerinden en fazla yarar sağlayacak iş yükleridir.

Fs serisi, Premium depolamaya ek olarak F serisinin tüm avantajlarını sağlar.

## <a name="fs-series"></a>Fs Serisi*

ACU: 210 - 250

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS-MB/sn (önbellek boyutu GiB biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | Maks NIC / Beklenen ağ performansı (Mbps) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_F1s |1 |2 |4 |2 |4000/32 (12) |3200/48 |2 / 750 |
| Standard_F2s |2 |4 |8 |4 |8000/64 (24) |6400/96 |2 / 1500 |
| Standard_F4s |4 |8 |16 |8 |16.000/128 (48) |12.800/192 |4 / 3000 |
| Standard_F8s |8 |16 |32 |16 |32.000/256 (96) |25.600/384 |8 / 6000 |
| Standard_F16s |16 |32 |64 |32 |64.000/512 (192) |51.200/768 |8 / 6000-12000 &#8224; |

MB/sn = 10^6 bayt/saniye ve GiB = 1024^3 bayt.

*Fs Serisi VM ile maksimum disk aktarım hızı (IOPS veya MB/sn), ekli disklerin sayısı, boyutu ve bölümleme türüyle sınırlı olabilir.  Ayrıntılar için bkz. [Premium Depolama: Azure sanal makine iş yükleri için yüksek performanslı depolama](../articles/storage/storage-premium-storage.md).


<br>

## <a name="f-series"></a>F Serisi

ACU: 210 - 250

| Boyut         | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum geçici depolama aktarım hızı: IOPS / Okuma MB/sn / Yazma MB/sn | Maksimum veri diski/aktarım hızı: IOPS | Maks NIC / Beklenen ağ performansı (Mbps) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_F1  | 1         | 2           | 16             | 3000/46/23                                           | 2/2x500                         | 2 / 750                 |
| Standard_F2  | 2         | 4           | 32             | 6000/93/46                                           | 4/4x500                         | 2 / 1500                     |
| Standard_F4  | 4         | 8           | 64             | 12000/187/93                                         | 8/8x500                         | 4 / 3000                     |
| Standard_F8  | 8         | 16          | 128            | 24000/375/187                                        | 16/16x500                       | 8 / 6000                     |
| Standard_F16 | 16        | 32          | 256            | 48000/750/375                                        | 32/32x500                       | 8 / 6000 - 12000 &#8224;           |


<br>


