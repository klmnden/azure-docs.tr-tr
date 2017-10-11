İş, algılanan ve izlenen yüzeyleri hakkında meta veriler içeren bir JSON çıkış dosyası oluşturur. Meta veriler yüz gibi tek tek izlenmesi belirten bir nominal kimlik numarası konumunu belirten koordinatlarını içerir. Yüz kimlik numaraları birden çok kimliği atanan bazı kişiler kaynaklanan tamamen çıplak yüz kaybolduğunda veya çerçevede çakışan koşullarda sıfırlamak fazladır.

Çıkış JSON aşağıdaki öznitelikleri içerir:

| Öğesi | Açıklama |
| --- | --- |
| Sürüm |Video API sürümüne başvuruyor. |
| Dizin | (Yalnızca Azure medya Redactor için geçerlidir) geçerli olay çerçeve dizinini tanımlar. |
| Zaman Çizelgesi |Videonun saniyede "çizgilerine". |
| uzaklık |Bu tarih damgası zaman uzaklığı. Video API'leri 1.0 sürümünde, bu her zaman 0 olacaktır. Gelecekte destekliyoruz senaryoları, bu değeri değiştirebilirsiniz. |
| Kare hızı |Videonun Saniyedeki çerçeve sayısı. |
| Parçaları |Meta verileri ayarlama parçaları olarak adlandırılan farklı kesimler halinde öbekli. Her parça başlangıç, süre, aralığı sayısı ve olay içerir. |
| start |'Ticks' ilk olay başlangıç saati. |
| Süre |Parçadaki "çizgilerine" uzunluğu. |
| aralığı |Her olay girişi parçadaki "çizgilerine" içinde aralığı. |
| etkinlikler |Her olay algılandı ve bu süre içinde izlenen yüzeyleri içerir. Olaylar dizisi dizisidir. Dış dizi bir zaman aralığı temsil eder. İç dizi 0 veya zamandaki o noktada daha fazla olayları oluşur. Boş köşeli ayraç [] hiçbir yüzeyleri algılandı anlamına gelir. |
| id |İzlenmekte olan yüz kimliği. Bu sayı, bir yazıtipi algılanmayan hale gelirse yanlışlıkla değişebilir. Belirli bir birey genel video boyunca aynı Kimliğe sahip olmalıdır, ancak bu algılama algoritması (kapanması, vb.) sınırlamaları nedeniyle garanti edilemez |
| x, y |Sol üst X ve Y koordinatları 0.0 ile 1.0 normalleştirilmiş ölçeğinde kutusu sınırlayıcı yazıtipinin. <br/>-X ve Y dikey video (veya görüntülemediğini, iOS söz konusu olduğunda) varsa, buna göre koordinatlar sırasını değiştir gerekecek şekilde koordinatları her zaman yatay olarak göreli. |
| Genişlik, yükseklik |Genişlik ve yükseklik 0.0 ile 1.0 normalleştirilmiş ölçeğinde kutusu sınırlayıcı yazıtipinin. |
| facesDetected |Bu JSON sonuçları sonunda bulunan ve algoritma video sırasında algılanan yüzeyleri sayısını özetler. Bir yazıtipi algılanmayan hale gelirse kimlikleri yanlışlıkla sıfırlanabilir olmadığından (örneğin yüz gider ekranı, hemen görünür), bu her zaman true sayısı, video yüz eşit değil. |

