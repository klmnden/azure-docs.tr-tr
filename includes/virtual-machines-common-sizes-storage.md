Depolama en iyi duruma getirilmiş VM boyutları yüksek disk işleme ve g/ç sağlar ve büyük veri, SQL ve NoSQL veritabanları için idealdir. Bu makale Vcpu, veri diskleri ve NIC yanı sıra bu gruplandırmadaki her boyutu için depolama üretilen iş ve ağ bant sayısı hakkında bilgi sağlar. 

Ls serisi, [Intel® Xeon İşlemci E5 v3 ailesi](http://www.intel.com/content/www/us/en/processors/xeon/xeon-e5-solutions.html) ile 32’ye kadar vCPU kullanım olanağı sunar. Ls serisi, G/GS serisi ile aynı CPU performansı sunar ve her vCPU başına 8 GiB bellek içerir.  

## <a name="ls-series"></a>Ls serisi

ACU: 180-240
 
| Boyut          | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | En büyük geçici depolama verimliliğini: IOP / MB/sn | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | Max NIC'ler / beklenen ağ bant genişliği (Mbps) | 
|---------------|-----------|-------------|--------------------------|----------------|-------------------------------------------------------------|-------------------------------------------|------------------------------| 
| Standart_L4s   | 4    | 32   | 678   | 16    | 20,000 / 200   | 10.000/250        | 2 / 4,000  | 
| Standart_L8s   | 8    | 64   | 1,388 | 32   | 40,000 / 400   | 20.000/500       | 4 / 8,000  | 
| Standart_L16s  | 16   | 128  | 2,807 | 64   | 80,000 / 800   | 40.000/1000       | 8 / 6,000 - 16,000 &#8224; | 
| Standard_L32s <sup>1</sup> | 32   | 256  | 5,630 | 64   | 160,000 / 1,600   | 80.000/2000     | 8 / 20,000 | 
 

Ls Serisi VM ile maksimum disk aktarım hızı, tüm ekli disklerin sayısı, boyutu ve bölümleme türüyle sınırlı olabilir. Ayrıntılar için bkz. [Premium Depolama: Azure sanal makine iş yükleri için yüksek performanslı depolama](../articles/virtual-machines/windows/premium-storage.md). 

<sup>1</sup> örneği için tek bir müşteriye ayrılmış donanım için ayrılmış.

