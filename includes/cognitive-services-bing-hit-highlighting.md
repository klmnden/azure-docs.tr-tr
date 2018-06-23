Bing isabet vurgulama, sorgu terimlerinin işaretler destekler (veya diğer bu Bing bulur ilgili koşulları) bazı yanıtlar görüntü dizelerde. Örneğin, bir Web sayfasının `name`, `displayUrl`, ve `snippet` alanlar sorgu terimlerine işaretlemek.

Varsayılan olarak, Bing görünen dizeler işaretçilerini vurgulama içermez. İşaretçileri dahil etmenize `textDecorations` isteğiniz parametresinde sorgulamak ve ayarlamak **doğru**. Bing başlangıcını ve bitişini koşulunun işaretlemek için E000 ve E001 Unicode karakterler kullanarak sorgu terimlerine işaretler. Örneğin, sorgu Terime yelken açmaya ne dersiniz Dinghy ise ve her iki terim alanı var, terimi isabet vurgulama karakter aşağıdaki örnekte gösterildiği gibi içine alınır:  
  
![İsabet vurgulama](./media/cognitive-services-bing-hit-highlighting/bing-hit-highlighting.PNG) 

Dize, kullanıcı arabirimini görüntülemeden önce görüntüleme biçimi için uygun olan karakterlerle Unicode karakterler değiştirirler. Metni HTML olarak görüntülüyorsanız, örneğin, sorgu Terime E000 ile değiştirerek vurgulayın < b\> ve E001 ile < /b\>. Biçimlendirmenin uygulanmasını istemiyorsanız, dizeden işaretlerini kaldırın. 

Bing Unicode karakter veya HTML etiketleri işaretleyici olarak kullanma seçeneği sunar. Kullanmak için hangi işaretçileri belirtmek için dahil `textFormat` sorgu parametresi. Unicode karakterler içerikle işaretlemek için ayarlanmış `textFormat` Raw (varsayılan) ve HTML etiketlerini içerikle işaretlemek için ayarlamak `textFormat` HTML. 
  
Varsa `textDecorations` olan **doğru**, Bing yanıtlar görüntü dizelerde aşağıdaki işaretleri içerebilir. HTML eşdeğeri ise, HTML tablo hücresi boştur.

|Unicode|HTML|Açıklama
|-|-|-
|U + E000|\<b >|(İsabet vurgulama) sorgu Terime başlangıcını işaretler
|U + E001|\</b >|Sorgu Terime sonunu işaretler
|U + E002|\<t >|İtalik içerik başlangıcını işaretler 
|U + E003|\</i >|İtalik içerik sonunu işaretler
|U + E004|\<br / >|Bir satır sonunu işaretler
|U + E005||Bir telefon numarası başlangıcını işaretler
|U + E006||Bir telefon numarası sonunu işaretler
|U + E007||Bir adresi başlangıcını işaretler
|U + E008||Bir adresi sonunu işaretler
|U + E009|\&nbsp;|Bölünemez yer işaretleri
|U + E00C|\<strong >|Kalın içerik başlangıcını işaretler
|U + E00D|\</ strong >|Kalın içerik sonunu işaretler
|U + E00E||Arka planının çevresindeki, arka plan açık olması gereken içerik başlangıcını işaretler
|U + E00F||Arka planının çevresindeki, arka plan açık olması gereken içerik sonunu işaretler
|U + E010||Arka planının çevresindeki, arka plan koyu içerik başlangıcını işaretler
|U + E011||Arka planının çevresindeki, arka plan koyu içerik sonunu işaretler
|U + E012|\<DEL >|Aracılığıyla harfi içerik başlangıcını işaretler
|U + E013|\</ DEL >|Aracılığıyla harfi içerik sonunu işaretler
|U + E016|\<Sub >|Alt simge içerik başlangıcını işaretler
|U + E017|\</ sub >|Alt simge içerik sonunu işaretler
|U + E018|\<sup >|Simge içerik başlangıcını işaretler
|U + E019|\</ sup >|Simge içerik sonunu işaretler

Aşağıdaki örnekte gösterildiği bir `Computation` log(2) sorgu dönem için alt simge işaretçileri içeren yanıt. `expression` Alan içerir işaretçileri eksikse `textDecoration` olan **doğru**.

![Hesaplama işaretçileri](./media/cognitive-services-bing-hit-highlighting/bing-markers-computation.PNG) 

İstek düzenlemelerinin istemediyseniz ifade log10(2) olacaktır. 
  
