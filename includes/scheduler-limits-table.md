Aşağıdaki tabloda her ana kotaları, sınırları, Varsayılanları ve Azure Scheduler kısıtlamaları açıklar.

| Kaynak | Sınır açıklaması |
| --- | --- |
| **İş boyutu** |Maksimum iş boyutu 16 K'dır. PUT veya bir düzeltme eki bu sınırlardan daha büyük bir işin sonuçlanırsa, 400 Hatalı istek durum kodu döndürülür. |
| **İstek URL boyutu** |En fazla istek URL'si 2048 karakter boyutudur. |
| **Toplam üstbilgi boyutu** |En fazla toplam üstbilgi boyutu 4096 karakter değil. |
| **Üstbilgi sayısı** |En fazla üstbilgi, 50 üstbilgileri sayısıdır. |
| **Gövde boyutu** |En fazla gövdesi, 8192 karakter boyutudur. |
| **Yineleme aralığı** |En fazla yineleme aralığı 18 ay ' dir. |
| **Başlangıç zamanı** |"Başlangıç saati için zaman" en fazla 18 ay ' dir. |
| **İş geçmişi** |İş geçmişinde depolanan en büyük yanıt gövdesi 2048 bayttır. |
| **Sıklık** |Varsayılan max sıklığı kota, 1 saat içinde bir ücretsiz iş koleksiyonu ve 1 dakika içinde bir standart iş koleksiyonu ' dir. Max sıklığı en yüksek değerden daha düşük olacak şekilde bir iş koleksiyonu üzerinde yapılandırılabilir. İş koleksiyonundaki tüm işleri sınırlı iş koleksiyonu üzerinde ayarlanan değer. İş koleksiyonu üzerinde en fazla sıklığından daha yüksek bir sıklıkta bir iş oluşturmayı denerseniz, isteği 409 çakışma durum kodu ile başarısız olur. |
| **İşler** |Varsayılan en büyük iş kotası ücretsiz iş koleksiyonu 5 işler ve 50 işler standart iş koleksiyonu ' dir. Maksimum sayıda işi bir iş koleksiyonu yapılandırılabilir. İş koleksiyonundaki tüm işleri sınırlı iş koleksiyonu üzerinde ayarlanan değer. En büyük iş kotası'den daha fazla iş oluşturmayı denerseniz, istek 409 çakışma durum kodu ile başarısız olur. |
| **İş koleksiyonları** |En fazla iş koleksiyonu abonelik başına 200.000 sayısıdır. |
| **İş Geçmişi bekletme** |İş geçmişi, en çok 2 ay için veya son 1000 yürütmeleri kadar korunur. |
| **Tamamlanan ve hatalı iş bekletme** |Tamamlanan ve hatalı işler 60 gün boyunca saklanır. |
| **Zaman aşımı** |Bir statik (yapılandırılamaz) istek zaman aşımı süresi 60 saniye HTTP eylemler için yoktur. Uzun çalışan işlemleri için zaman uyumsuz HTTP protokolleri izleyin; Örneğin, bir 202 hemen geri ancak arka planda çalışmaya devam edin. |

