
Ls serisi NoSQL veritabanları (Cassandra, MongoDB, Cloudera ve Redis gibi) gibi düşük gecikme süresine sahip geçici depolama alanları gerektiren iş yükleri için optimize edilmiştir. Ls serisi, [Intel® Xeon İşlemci E5 v3 ailesi](http://www.intel.com/content/www/us/en/processors/xeon/xeon-e5-solutions.html) ile 32’ye kadar vCPU kullanım olanağı sunar. Ls serisi, G/GS serisi ile aynı CPU performansı sunar ve her vCPU başına 8 GiB bellek içerir.  

## <a name="ls-series"></a>Ls serisi

ACU: 180-240
 
| Boyut          | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS-MB/sn (önbellek boyutu GiB biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | Maks NIC / Beklenen ağ performansı (Mbps) | 
|---------------|-----------|-------------|--------------------------|----------------|-------------------------------------------------------------|-------------------------------------------|------------------------------| 
| Standart_L4s  | 4    | 32   | 678   | 8              | Yok / Yok (0)          | 5000/125                               | 2 / 4000       | 
| Standart_L8s  | 8    | 64   | 1,388 | 16             | Yok / Yok (0)          | 10.000/250                              | 4 / 8000  | 
| Standart_L16s | 16   | 128  | 2,807 | 32             | Yok / Yok (0)          | 20.000/500                              | 8 / 6000 - 16000 &#8224; | 
| Standard_L32s* | 32 | 256  | 5,630 | 64             | Yok / Yok (0)          | 40.000/1000                            | 8 / 20000 | 
 

Ls Serisi VM ile maksimum disk aktarım hızı, tüm ekli disklerin sayısı, boyutu ve bölümleme türüyle sınırlı olabilir. Ayrıntılar için bkz. [Premium Depolama: Azure sanal makine iş yükleri için yüksek performanslı depolama](../articles/virtual-machines/windows/premium-storage.md). 

*Örnek, tek bir müşteriye özel donanımla yalıtılmıştır.

