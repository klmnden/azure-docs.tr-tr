<!--author=alkohli last changed: 12/15/15-->

| Sınır tanımlayıcı | Sınır | Yorumlar |
| --- | --- | --- |
| Depolama hesabının kimlik bilgilerini maksimum sayısı |64 | |
| Birim kapsayıcıları maksimum sayısı |64 | |
| En fazla sayıda birime |255 | |
| Zamanlamalar bant genişliği şablonu başına maksimum sayısı |168 |Her gün (24 * 7) haftanın her saat için bir zamanlama. |
| Katmanlı birim fiziksel aygıtlarda en büyük boyutu |8100 ve 8600 64 TB |8100 ve 8600 fiziksel aygıtlardır. |
| Azure'da sanal cihazlarda katmanlı birim en büyük boyutu |8010 30 TB <br></br> 8020 için 64 TB |8010 hem de 8020 sırasıyla standart depolama ve Premium depolama kullanan sanal Azure aygıtlardır. |
| Fiziksel cihazları yerel olarak sabitlenmiş bir birimde en büyük boyutu |8100 9 TB <br></br> 8600 24 TB |8100 ve 8600 fiziksel aygıtlardır. |
| İSCSI bağlantısı sayısı |512 | |
| İSCSI başlatıcıları bağlantılarından maksimum sayısı |512 | |
| Erişim denetimi kayıtları aygıt başına maksimum sayısı |64 | |
| En fazla sayıda birime yedekleme İlkesi başına |24 | |
| Yedekleme İlkesi korunur yedeklemeleri sayısı |64 | |
| Zamanlamalar yedekleme İlkesi başına maksimum sayısı |10 | |
| Maksimum sayıda anlık görüntü birim başına korunabilir herhangi bir türde |256 |Bu yerel anlık görüntülerini içerir ve bulut anlık görüntüleri. |
| Maksimum sayıda içinde herhangi bir cihazda mevcut olabilecek anlık görüntü |10,000 | |
| En fazla sayıda paralel yedekleme, geri yükleme için işlenen ya da kopyalama birime |16 |<ul><li>16'dan fazla birim varsa, bunlar işleme yuvaları kullanılabilir duruma geldiğinde sıralı olarak işlenir.</li><li>İşlemi tamamlanana kadar bir kopyalanan yeni yedeklerini veya geri yüklenen bir katmanlı birim oluşamaz. Ancak, yerel bir birim için birim çevrimiçi olduktan sonra yedeklemeler izin verilir.</li></ul> |
| Geri yükleme ve kurtarma zamanı katmanlı birimler için kopyalama |< 2 dakika |<ul><li>Birim boyutu bağımsız olarak, geri yükleme veya kopyalama işleminin 2 dakika içinde birim kullanılabilir hale getirilir.</li><li>Birim performans başlangıçta veri ve meta veriler çoğunu hala yer aldığı bulutta normalden daha yavaş olabilir. StorSimple cihazı için buluttan veri akışları olarak performansı artırabilir.</li><li>Meta veri indirmek için toplam süreyi ayrılmış birimin boyutuna bağlıdır. Meta verileri otomatik olarak ayrılmış birim verilerini TB başına 5 dakika hızında arka planda bir aygıt halinde duruma getirilir. Internet bant genişliği bulut için bu oran etkilenebilir.</li><li>Tüm meta verilerin cihazda olduğunda geri yükleme veya kopyalama işlemi tamamlanır.</li><li>Kopya işlemi tam olarak tamamlandıktan veya yedekleme işlemleri kadar geri yükleme gerçekleştirilemez. |
| Yerel olarak sabitlenmiş birimler için kurtarma süresi geri yükleme |< 2 dakika |<ul><li>Geri yükleme işleminin bağımsız olarak birim boyutu 2 dakika içinde birim kullanılabilir hale getirilir.</li><li>Birim performans başlangıçta veri ve meta veriler çoğunu hala yer aldığı bulutta normalden daha yavaş olabilir. StorSimple cihazı için buluttan veri akışları olarak performansı artırabilir.</li><li>Meta veri indirmek için toplam süreyi ayrılmış birimin boyutuna bağlıdır. Meta verileri otomatik olarak ayrılmış birim verilerini TB başına 5 dakika hızında arka planda bir aygıt halinde duruma getirilir. Internet bant genişliği bulut için bu oran etkilenebilir.</li><li>Yerel olarak sabitlenmiş birimler, söz konusu olduğunda, katmanlı birimleri aksine birim verilerini de yerel olarak cihazda indirilir. Tüm birim verilerini duruma zaman cihaza geri yükleme işlemi tamamlanır.</li><li>Geri yükleme işlemlerini uzun olabilir ve geri yüklemeyi tamamlamak için gereken toplam süre, sağlanan yerel birim, Internet bant genişliği ve cihazda mevcut veri boyutuna bağlıdır. Geri yükleme işlemi devam ederken yerel olarak sabitlenmiş birim yedekleme işlemlerine izin verilir. |
| Kullanılabilirlik ince-geri yükle |Son yük devretme | |
| (SSD Katmanı'ndan sunulduğunda) en fazla istemci okuma/yazma verimlilik * |920/720 MB/s ile tek bir 10GbE ağ arabirimi |En çok 2 x MPIO ile ve iki ağ arabirimi. |
| (HDD Katmanı'ndan sunulduğunda) en fazla istemci okuma/yazma verimlilik * |120/250 MB/s | |
| (Bulut Katmanı'ndan sunulduğunda) en fazla istemci okuma/yazma verimlilik * |11/41 MB/s |Okuma üretilen işi oluşturmak ve yeterli g/ç sıra derinliğini koruyarak istemcilerde bağlıdır. |

&#42;G/ç türü başına en fazla üretilen iş yüzde 100 okuma ve yüzde 100 yazma senaryoları ile ölçülen. Gerçek verimlilik daha düşük olabilir ve g/ç üzerinde bağlı karışımı ve ağ koşulları.

