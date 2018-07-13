Aşağıdaki tabloda her bir ana kotaları, sınırları, Varsayılanları ve Azure scheduler'da kısıtlamalar açıklanmaktadır.

| Kaynak | Sınırı açıklaması |
| --- | --- |
| **İş boyutu** |En fazla iş boyutu 16 K'dır. Bu sınırlardan daha büyük bir işteki PUT veya bir düzeltme eki sonuçlanırsa, 400 Hatalı istek durum kodu döndürülür. |
| **İstek URL'si boyutu** |İstek URL'si boyutu 2048 karakter olan. |
| **Toplam üstbilgi boyutu** |Maksimum toplam üstbilgi boyutu 4096 karakter var. |
| **Üst bilgi sayısı** |En fazla üst bilgi, 50 üstbilgileri sayısıdır. |
| **Gövde boyutu** |En fazla gövdesi, 8192 karakterden boyutudur. |
| **Yinelenme aralığı** |En fazla yinelenme aralığı 18 aydır. |
| **Başlangıç saati süresi** |"Zaman başlangıç saati için" en fazla 18 aydır. |
| **İş geçmişi** |İş geçmişinde depolanan en yüksek yanıt gövdesi 2048 bayt olabilir. |
| **Sıklık** |Varsayılan en büyük sıklığı kota, 1 saat içindeki bir ücretsiz iş koleksiyonu ve 1 dakika içinde bir standart iş koleksiyonu ' dir. Max sıklığı bir iş koleksiyonu, en yüksek değerden daha düşük olacak şekilde yapılandırılabilir. İş koleksiyonundaki tüm işlerin sınırlı iş koleksiyonu üzerinde ayarlanan değer. İş koleksiyonu üzerinde en fazla sıklığından daha yüksek sıklığa sahip bir iş oluşturmayı denerseniz, isteği 409 çakışma durum kodu ile başarısız olur. |
| **İşler** |Varsayılan en büyük iş kotası 5 ücretsiz iş koleksiyonu işler ve 50 işlerinde standart iş koleksiyonu ' dir. İşlerin sayısı üzerinde bir iş koleksiyonu yapılandırılabilir. İş koleksiyonundaki tüm işlerin sınırlı iş koleksiyonu üzerinde ayarlanan değer. En büyük iş kotası değerinden daha fazla iş oluşturmaya çalışırsanız, istek 409 çakışma durum kodu ile başarısız olur. |
| **İş koleksiyonları** |En fazla iş koleksiyonu abonelik başına 200.000 sayısıdır. |
| **İş Geçmişi bekletme** |İş geçmişi, en fazla 2 ay için ya da son 1000 yürütmeleri kadar korunur. |
| **Tamamlanan ve hatalı iş bekletme** |Tamamlanan ve hatalı işler 60 gün boyunca saklanır. |
| **zaman aşımı** |HTTP eylemleri için 60 saniyelik bir statik (yapılandırılabilir) istek zaman aşımı yoktur. Uzun çalışan işlemleri için zaman uyumsuz HTTP protokolleri izleyin. Örneğin, bir 202 hemen geri döner ancak arka planda çalışmaya devam. |

