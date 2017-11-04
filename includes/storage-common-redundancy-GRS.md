Coğrafi olarak yedekli depolama (GRS) verileriniz birincil bölge çıktığınızda mil yüzlerce olan ikincil bir bölgeye çoğaltır. Etkin GRS depolama hesabınız varsa, verilerinizi bile tam bölgesel bir kesintinin veya bir olağanüstü durumda birincil bölge kurtarılabilir değil söz konusu olduğunda dayanıklı.

Etkin GRS ile bir depolama hesabı için bir güncelleştirme burada üç kez çoğaltılır birincil bölge için ilk kararlıdır. Daha sonra güncelleştirmeyi zaman uyumsuz olarak burada bunu da üç kez çoğaltılır ikincil bölgeye çoğaltılır.

GRS ile birincil ve ikincil bölgeler çoğaltmalar ayrı hata etki alanlarında yönetmek ve etki alanı ile LRS açıklandığı gibi bir depolama ölçek birimi içinde yükseltin.

Dikkate alınacak noktalar:

* Zaman uyumsuz çoğaltma bir gecikme gerektirdiğinden, bölgesel bir olağanüstü durumda birincil bölgesinden veri kurtarılamazsa, ikincil bölge'ye henüz çoğaltılmamış değişiklikler kaybolacak mümkündür.
* Microsoft yük devretme ikincil bölge başlatır sürece çoğaltma kullanılamıyor. Microsoft bir yük devretme ikincil bölge başlatın varsa, okuduğunuz ve yük devretme sonrasında bu verilere yazma erişimi tamamlandı. Daha fazla bilgi için lütfen bkz [olağanüstü durum kurtarma Kılavuzu](../articles/storage/common/storage-disaster-recovery-guidance.md). 
* Kullanıcı, bir uygulama ikincil bölgesinden okumak isterse, RA-GRS etkinleştirmeniz gerekir.

Bir depolama hesabı oluşturduğunuzda, hesap için birincil bölge seçin. İkincil bölge birincil bölgeye göre belirlenir ve değiştirilemez. Aşağıdaki tabloda birincil ve ikincil bölge eşleştirmeleri gösterilir.

| Birincil | İkincil |
| --- | --- |
| Orta Kuzey ABD | Orta Güney ABD |
| Orta Güney ABD | Orta Kuzey ABD |
| Doğu ABD | Batı ABD |
| Batı ABD | Doğu ABD |
| ABD Doğu 2 | Orta ABD |
| Orta ABD | ABD Doğu 2 |
| Kuzey Avrupa | Batı Avrupa |
| Batı Avrupa | Kuzey Avrupa |
| Güneydoğu Asya | Doğu Asya |
| Doğu Asya | Güneydoğu Asya |
| Doğu Çin | Kuzey Çin |
| Kuzey Çin | Doğu Çin |
| Japonya Doğu | Japonya Batı |
| Japonya Batı | Japonya Doğu |
| Güney Brezilya | Orta Güney ABD |
| Avustralya Doğu | Avustralya Güneydoğu |
| Avustralya Güneydoğu | Avustralya Doğu |
| Hindistan Güney | Hindistan Orta |
| Hindistan Orta | Hindistan Güney |
| Hindistan Batı | Hindistan Güney |
| ABD Devleti Iowa | ABD Devleti Virginia |
| ABD Devleti Virginia | ABD Devleti Texas |
| ABD Devleti Texas | ABD Devleti Arizona |
| ABD Devleti Arizona | ABD Devleti Texas |
| Orta Kanada | Doğu Kanada |
| Doğu Kanada | Orta Kanada |
| Birleşik Krallık Batı | Birleşik Krallık Güney |
| Birleşik Krallık Güney | Birleşik Krallık Batı |
| Almanya Orta | Almanya Kuzeydoğu |
| Almanya Kuzeydoğu | Almanya Orta |
| Batı ABD 2 | Batı Orta ABD |
| Batı Orta ABD | Batı ABD 2 |

Azure tarafından desteklenen bölgeler hakkında güncel bilgiler için bkz: [Azure bölgeleri](https://azure.microsoft.com/regions/).

>[!NOTE]  
> ABD kamu Virginia ikincil BİZE kamu Texas bölgedir. Daha önce BİZE kamu Virginia BİZE kamu Iowa bir ikincil bölge ' kullanılan. Depolama hesapları hala bir ikincil bölge'olarak BİZE kamu Iowa yararlanan bir ikincil bölge BİZE kamu Texas Geçirilmekte olan. 
> 
> 