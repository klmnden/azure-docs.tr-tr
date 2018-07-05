İş, algılanan ve izlenen yüzleri hakkında meta veriler içeren bir JSON çıktı dosyası üretir. Meta veriler, ayrı ayrı izlenmesi belirten bir yüz kimliği sayı yanı sıra yüz konumunu belirten koordinatları içerir. Yüz Kimliği numaraları atanan birden çok kimlikler bazı kişiler kaynaklanan koşullar altında tamamen çıplak yüz kayıp veya çerçevede çakışan sıfırlama eğilimlidir.

Çıkış JSON aşağıdaki öğeleri içerir:

### <a name="root-json-elements"></a>Kök JSON öğeleri

| Öğe | Açıklama |
| --- | --- |
| sürüm |Bu Video API'si sürümüne başvurur. |
| Zaman Çizelgesi |Videoyu saniye başına "ticks". |
| uzaklık |Zaman damgaları için zaman uzaklığını budur. Video API'leri 1.0 sürümünde, bu her zaman 0 olacaktır. Gelecekte senaryoları destekliyoruz, bu değeri değiştirebilirsiniz. |
| Genişlik, yüksek |Genişlik ve piksel cinsinden çıkış video çerçevenin yüksek.|
| kare hızı |Videonun Saniyedeki çerçeve sayısı. |
| [parçaları](#fragments-json-elements) |Meta verileri ayarlama parçaları olarak adlandırılan farklı parçalara öbekli. Her parça bir başlangıç süresi, aralığı sayısı ve olay'ı içerir. |

### <a name="fragments-json-elements"></a>Parçaları JSON öğeleri

|Öğe|Açıklama|
|---|---|
| start |"Ticks." ilk olay başlangıç saati |
| süre |Uzunluğu parçasında "ticks." |
| dizin | (Yalnızca Azure Media Redactor için geçerlidir) geçerli olay çerçeve dizinini tanımlar. |
| interval |Her olay girişi parçasında "ticks." içinde aralığı |
| etkinlikler |Her bir olay algılandı ve bu süre içinde izlenen yüzleri içerir. Olayların bir dizidir. Dış dizi bir zaman aralığını temsil eder. İç diziyi 0 ya da bu noktada zamanında gerçekleşen daha fazla olay oluşur. Bir boş köşeli ayraç [] yok yüz algılandı anlamına gelir. |
| id |İzlenmekte olan yüz kimliği. Bu sayı, yanlışlıkla bir yüz algılanmayan hale gelirse değişebilir. Belirli bir kişiye genel video boyunca aynı Kimliğe sahip olmalıdır, ancak bu (kapatma, vb.) algılama algoritması sınırlamaları nedeniyle garanti edilemez. |
| x, y |Sol üst tarafında X ve Y koordinatları sınırlayıcı kutu normalleştirilmiş bir ölçek 0.0 ile 1.0, yüz tanıma. <br/>-X ve Y koordinatları her zaman yatay olarak göreli bir dikey görüntü (veya baş aşağı, iOS söz konusu olduğunda) varsa, buna göre koordinatlar sırasını değiştirmek zorunda. |
| Genişlik, yükseklik |0.0 ile 1.0 normalleştirilmiş bir ölçek kümesinde genişlik ve yükseklik yüz sınırlayıcı kutu. |
| facesDetected |Bu JSON sonuçları sonunda bulunan ve algoritma videonun sırasında algılanan yüzlerin sayısı özetler. Bir yüzü algılanmayan hale gelirse, yanlışlıkla kimlikleri sıfırlanabilir olmadığından (örneğin, yüz tanıma hemen görünür ekranı kapattıktan sağlanabilir), bu sayı her zaman true videoda yüzlerin sayısı eşit değil. |
