| Kaynak | Varsayılan Sınır |
| --- | --- |
| Abonelik başına depolama hesabı sayısı | 200<sup>1</sup> |
| En fazla depolama hesabı kapasitesi | 500 Tıb<sup>2</sup> |
| En fazla blob kapsayıcıları, BLOB'lar, dosya paylaşımları, tablolar, kuyruklar, varlık veya depolama hesabı başına ileti sayısı | Sınırsız |
| En fazla giriş<sup>3</sup> her depolama hesabı (BİZE bölgelerde) | 10 Gbps GRS/ZRS,<sup>4</sup> etkinse, LRS 20 GB/sn<sup>2</sup> |
| En büyük çıkış<sup>3</sup> her depolama hesabı (BİZE bölgelerde) | RA-GRS/GRS/ZRS ise 20 GB/sn<sup>4</sup> etkinse, LRS için 30 GB/sn<sup>2</sup> |
| En fazla giriş<sup>3</sup> her depolama hesabı (ABD olmayan bölgeleri için) | GRS/ZRS, 5 GB/sn<sup>4</sup> etkinse, LRS için 10 GB/sn<sup>2</sup> |
| En büyük çıkış<sup>3</sup> her depolama hesabı (ABD olmayan bölgeleri için) | 10 Gbps RA-GRS/GRS/ZRS,<sup>4</sup> etkinse, LRS 15 GB/sn<sup>2</sup> |

<sup>1</sup>standart ve Premium depolama hesaplarını içerir. 200'den fazla depolama hesabı gerekiyorsa, [Azure Destek](https://azure.microsoft.com/support/faq/) üzerinden bir istek oluşturun. Azure Depolama ekibi, işinizin durumunu inceler ve 250’ye kadar depolama hesabı için onay verebilir. 

<sup>2</sup> kapasite, giriş/çıkış ve istek oranı tanıtılan sınırları geçmiş büyüme için standart depolama hesaplarınızı almak için lütfen aracılığıyla istekte [Azure Destek](https://azure.microsoft.com/support/faq/). Azure depolama ekibi istekleri incelemeli ve olay temelinde daha yüksek sınırları onaylama.

<sup>3</sup> yalnızca hesabın giriş/çıkış sınırları tarafından tutulabilir. *Giriş* bir depolama hesabına gönderilen tüm veriler (istek) başvuruyor. *Çıkış*, bir depolama hesabından alınan tüm verileri (yanıtlar) ifade eder.  

<sup>4</sup>azure Depolama artıklık seçenekleri:
* **RA-GRS**: coğrafi olarak yedekli depolamaya okuma erişimi. RA-GRS etkinleştirilirse, çıkış hedefler ve ikincil konum için birincil konum olanlarla aynıdır.
* **GRS**: coğrafi olarak yedekli depolama. 
* **ZRS**: bölge olarak yedekli depolama. Yalnızca blok bloblar için kullanılabilir. 
* **LRS**: yerel olarak yedekli depolama. 