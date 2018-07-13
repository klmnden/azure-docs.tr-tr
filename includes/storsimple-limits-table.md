<!--author=alkohli last changed: 12/15/15-->

| Sınır tanımlayıcı | Sınır | Yorumlar |
| --- | --- | --- |
| Depolama hesabı kimlik bilgileri sayısı |64 | |
| Birim kapsayıcılarının sayısı |64 | |
| En fazla birim sayısı |255 | |
| Zamanlamalar bant genişliği şablonu başına en fazla sayısı |168 |Her saat (24 * 7) haftanın her günü için bir zamanlama belirleyin. |
| Katmanlı birim fiziksel cihazlarda en yüksek boyutu |8100 ve 8600 için 64 TB |8100 ve 8600 fiziksel cihazlardır. |
| Azure sanal cihazda katmanlı birim en büyük boyutu |8010'un 30 TB <br></br> 8020 için 64 TB |8010 ve 8020 sırasıyla standart depolama ve Premium depolama kullanan sanal azure'da cihazlardır. |
| Fiziksel cihazlarda yerel olarak sabitlenmiş bir birim, en büyük boyutu |8100 için 9 TB <br></br> 8600 için 24 TB |8100 ve 8600 fiziksel cihazlardır. |
| İSCSI bağlantı sayısı üst sınırı |512 | |
| İSCSI başlatıcılarının bağlantılarından sayısı |512 | |
| Erişim denetimi kayıtları cihaz başına en fazla sayısı |64 | |
| Yedekleme İlkesi başına birim sayısı |24 | |
| Yedekleme İlkesi korunan yedekleme sayısı |64 | |
| Yedekleme İlkesi başına zamanlamaları sayısı |10 | |
| Birim başına korunabilir herhangi bir türde anlık görüntü sayısı |256 |Bu yerel anlık görüntüleri içerir ve bulut anlık görüntüleri. |
| İçinde herhangi bir cihazda mevcut olabilecek anlık görüntü sayısı |10,000 | |
| En fazla sayıda paralel yedekleme, geri yükleme için işlenen ya da kopyalama |16 |<ul><li>16'dan fazla birim varsa, bunlar işleme yuvaları kullanılabilir oldukça sıralı olarak işlenir.</li><li>İşlemi tamamlanana kadar kopyalanmış bir yeni yedeklerini veya geri yüklenen bir katmanlı birim oluşamaz. Ancak, yerel bir birim için birim çevrimiçi olduktan sonra yedeklemeler izin verilir.</li></ul> |
| Geri yükleme ve kopyalama, katmanlı birimlerin zaman Kurtar |< 2 dakika |<ul><li>Birim boyutu ne olursa olsun, geri yükleme ya da kopyalama işleminin 2 dakika içinde birim kullanılabilir hale getirilir.</li><li>Toplu performans başlangıçta çoğu verileri ve meta veriler yine de bulunduğu olarak bulutta normalden daha yavaş olabilir. StorSimple cihazına buluttan veri akışları olarak performansı artırabilir.</li><li>Meta verileri indirmek için toplam süreyi ayrılmış birimin boyutuna bağlıdır. Meta verileri, cihaz arka planda hızında TB veri ayrılmış birim başına 5 dakika içinde otomatik olarak getirilir. Bu oran, Internet bant genişliği buluta tarafından etkilenebilir.</li><li>Tüm meta verileri cihaz üzerinde olduğunda geri yükleme veya kopyalama işlemi tamamlanmıştır.</li><li>Kopyalama işlemi tam olarak bitmiş durumda veya yedekleme işlemleri kadar geri yükleme gerçekleştirilemez. |
| Yerel olarak sabitlenmiş birimler için Kurtarma geri yükleme |< 2 dakika |<ul><li>Birim boyutu ne olursa olsun geri yükleme işleminin 2 dakika içinde birim kullanılabilir hale getirilir.</li><li>Toplu performans başlangıçta çoğu verileri ve meta veriler yine de bulunduğu olarak bulutta normalden daha yavaş olabilir. StorSimple cihazına buluttan veri akışları olarak performansı artırabilir.</li><li>Meta verileri indirmek için toplam süreyi ayrılmış birimin boyutuna bağlıdır. Meta verileri, cihaz arka planda hızında TB veri ayrılmış birim başına 5 dakika içinde otomatik olarak getirilir. Bu oran, Internet bant genişliği buluta tarafından etkilenebilir.</li><li>Yerel olarak sabitlenmiş birimler, söz konusu olduğunda, katmanlı birimlerin aksine birimdeki verileri de yerel olarak cihazda indirilir. Tüm birim verilerinin duruma zaman cihaza geri yükleme işlemi tamamlanır.</li><li>Geri yükleme işlemlerini uzun olabilir ve geri yüklemenin tamamlanması için toplam süreyi sağlanan yerel birimin, Internet bant genişliğiniz ve cihazdaki mevcut verilerin boyutuna bağlı olacaktır. Geri yükleme işlemi devam ederken yerel olarak sabitlenmiş birim üzerindeki yedekleme işlemlerine izin verilir. |
| Kullanılabilirlik ince-geri yükleme |Son yük devretme | |
| En fazla istemci okuma/yazma performans (SSD katmanından sunulduğunda) * |920/720 MB/s ile tek bir 10GbE ağ arabirimi |En fazla 2 x ile MPIO ve iki ağ arabirimi. |
| En fazla istemci okuma/yazma performans (HDD katmanı sunulduğunda) * |120/250 MB/s | |
| En fazla istemci okuma/yazma performans (bulut katmanından sunulduğunda) * |11/41 MB/s |Okuma aktarım hızı, oluşturma ve yeterli g/ç sıra derinliğini korumak istemcilerde bağlıdır. |

&#42;G/ç türü en fazla üretilen iş yüzde 100 okuma ve yüzde 100 yazma senaryoları ile ölçülmüştür. Gerçek aktarım hızı, daha düşük olabilir ve g/ç üzerinde bağlıdır karışımı ve ağ koşulları.

