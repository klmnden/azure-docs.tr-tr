---
title: Microsoft Bilişsel hizmetler için görüntü işleme API'si | Microsoft Docs
description: Gelişmiş algoritmalar görüntülerini işlemek ve Microsoft Bilişsel hizmetler bilgi döndürmek için görüntü işleme API'si kullanın.
services: cognitive-services
author: KellyDF
manager: corncar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 08/10/2017
ms.author: kefre
ms.openlocfilehash: 84d931ad79bf32b39a4d771f6afd1c9a05ad2395
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44714828"
---
# <a name="what-is-computer-vision-api-version-10"></a>Bilgisayar işleme API sürüm 1.0 nedir?

> [!IMPORTANT]
> Görüntü işleme API'si için yeni bir sürümü kullanıma sunulmuştur, bakın:
>- [Genel Bakış](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home)
>- [Bilgisayar işleme API sürüm 2.0](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)

Bulut tabanlı Görüntü İşleme API’si, geliştiriciler için görüntüleri işlemeye ve bilgileri döndürmeye yönelik gelişmiş algoritmalara erişim sağlar. Microsoft Görüntü İşleme algoritmaları bir görüntüyü karşıya yükleyerek veya bir görüntü URL’si belirterek girdilere ve kullanıcı seçimlerine göre görsel içerikleri farklı şekillerde analiz edebilir. Görüntü işleme API'si ile kullanıcılar görüntüleri çözümleyebilirsiniz:
* [Resimleri içeriği temelinde etiketleyin.](#Tagging)
* [Görüntüleri kategorilere ayırın.](#Categorizing)
* [Görüntü kalitesini ve türü tanımlayın.](#Identifying)
* [İnsan yüzlerini algılayın ve bunların koordinatlarını döndürür. ](#Faces)
* [Etki alanına özgü içerik tanır.](#Domain-Specific)
* [İçerik açıklamalarını oluşturur.](#Descriptions)
* [Görüntüleri bulunan yazdırılan metin tanımlamak için optik karakter tanıma kullanın.](#OCR)
* [Resimlerdeki el yazısı tanıma.](#RecognizeText)
* [Renk düzenleri ayırmak.](#Color)
* [Yetişkinlere yönelik içeriğe bayrak.](#Adult)
* [Küçük resim olarak kullanılacak kırpma fotoğraflar'ni kullanın.](#Thumbnails)

## <a name="requirements"></a>Gereksinimler
* Giriş yöntemleri desteklenir: ikili bir uygulama/octet stream veya resim URL'si biçiminde ham görüntü.
* Desteklenen görüntü biçimleri: JPEG, PNG, GIF, BMP.
* Görüntü dosyası boyutu: 4 MB'den daha küçük.
* Görüntü boyutu: 50 x 50 piksel büyüktür.

## <a name="tagging-images"></a>Görüntüleri etiketleme
Görüntü işleme API'si, 2000'den fazla tanınabilir nesne, canlı, manzara ve Eylemler alan etiketler döndürür. Etiketlerin belirsiz olduğunda veya bilinmediği API yanıtı 'bilinen bir ayar bağlamında etiketin anlamını açıklamak için ipuçları' sağlar. Etiketleri bir sınıflandırma düzenlenmiş ve devralma hiyerarşi yok. İçerik etiket koleksiyonu, 'description' tam cümlelerden biçimlendirilmiş insan tarafından okunabilir dili olarak görüntülenen bir görüntü için temel oluşturur. Bu noktada İngilizce için görüntü açıklaması yalnızca desteklenen dil olduğunu unutmayın.

Bir görüntü yüklemek veya bir resim URL'si belirtme sonra görüntü işleme API'Sİ'ın algoritmaları nesneleri, canlı ve Eylemler görüntüde tanımlanmış temel alan etiketler çıktı. Etiketleme, ana konu, bir kişi, ön planda gibi sınırlı değildir, ancak ayrıca ayarı (iç veya dış) mobilyası, araçları, tesisin, hayvanlar, Donatılar, vb. araçları içerir.

### <a name="example"></a>Örnek
![House_Yard](./Images/house_yard.png) '

```json
Returned Json
{
   'tags':[
      {
         "name":"grass",
         "confidence":0.999999761581421
      },
      {
         "name":"outdoor",
         "confidence":0.999970674514771
      },
      {
         "name":"sky",
         "confidence":999289751052856
      },
      {
         "name":"building",
         "confidence":0.996463239192963
      },
      {
         "name":"house",
         "confidence":0.992798030376434
      },
      {
         "name":"lawn",
         "confidence":0.822680294513702
      },
      {
         "name":"green",
         "confidence":0.641222536563873
      },
      {
         "name":"residential",
         "confidence":0.314032256603241
      },
   ],
}
```
## <a name="categorizing-images"></a>Kategorilere görüntüleri
Görüntü işleme API'si, etiketleme ve açıklamaları ek olarak, önceki sürümlerde tanımlanmış sınıflandırma tabanlı kategorileri döndürür. Bu kategoriler, üst/alt hereditary hiyerarşileri ile bir sınıflandırma olarak düzenlenir. Tüm kategoriler İngilizce'dir. Bunlar, tek başına veya bizim yeni modelleri ile kullanılabilir.

### <a name="the-86-category-concept"></a>Kategori 86 kavramı
Aşağıdaki diyagramda görüldüğü 86 kavramları listesini bağlı olarak, bir resimde bulunan görsel özellikler geniş özel arasında değişen sınıflandırılabilir. Metin biçimindeki tam sınıflandırma için bkz: [kategori sınıflandırma](https://docs.microsoft.com/azure/cognitive-services/computer-vision/category-taxonomy).

![Çözümleme kategorisi](./Images/analyze_categories.png)

Görüntü                                                  | Yanıt
------------------------------------------------------ | ----------------
![Kadın tavan](./Images/woman_roof.png)                 | kişiler
![Aile fotoğrafı](./Images/family_photo.png)             | people_crowd
![Şirin köpek](./Images/cute_dog.png)                     | animal_dog
![Dış Sıradağlar](./Images/mountain_vista.png)       | outdoor_mountain
![İşleme Gıda ekmek analiz edin](./Images/bread.png)       | food_bread

## <a name="identifying-image-types"></a>Resim türleri tanımlama
Görüntüleri kategorilere ayırmak için birkaç yolu vardır. Görüntü işleme API'si, bir resmin siyah beyaz mı renk olmadığını belirten bir Boole bayrağı ayarlayabilir. Ayrıca, bir resim çizim olup olmadığını belirten bir bayrak da ayarlayabilirsiniz. Bir görüntüyü küçük resim olup ve 0-3, bu nedenle bir ölçekte kalitesini belirten de belirtebilir.

### <a name="clip-art-type"></a>Küçük resim türü
Bir görüntüyü küçük resim olup olmadığını algılar.  

Değer | Anlamı
----- | --------------
0     | Olmayan küçük resim
1     | belirsiz
2     | Normal küçük resim
3     | iyi küçük resim

Görüntü|Yanıt
----|----
![Görüntü işleme peynirlerine ayırıyor küçük resim analiz edin](./Images/cheese_clipart.png)|3 iyi küçük-resmi
![Görüntü merkezi Yard analiz edin](./Images/house_yard.png)|0 olmayan küçük-resmi

### <a name="line-drawing-type"></a>Çizgi çizme türü
Bir resim çizim olup olmadığını algılar.

Görüntü|Yanıt
----|----
![Görüntü işleme Lion çizim analiz edin](./Images/lion_drawing.png)|True
![İşleme çiçek analiz edin](./Images/flower.png)|False

### <a name="faces"></a>Yüzler
Bir resimdeki İnsan yüzlerini algılar ve yüz koordinatları, yüz tanıma, cinsiyet ve yaş için dikdörtgen oluşturur. Bu görsel özellikleri yüz için oluşturulan meta verilerinin bir alt kümesidir. Yüz tanıma API'si (yüz tanıma, poz algılama ve daha fazlası) yüzler için oluşturulan daha kapsamlı meta veriler için kullanın.  

Görüntü|Yanıt
----|----
![İşleme kadın tavan yüz analiz edin](./Images/woman_roof_face.png) | [{"Yaş": 23 "cinsiyet": "Kadın", "faceRectangle": {"sol": "üst" 1379: 320, "width": "yükseklik" 310,: 310}}]
![İşleme Mom kız yüz analiz edin](./Images/mom_daughter_face.png) | [{"Yaş": 28 "cinsiyet": "Kadın", "faceRectangle": {"sol": "üst" 447: 195, "width": 162, "yükseklik": 162}}, {"Yaş": 10 "cinsiyet": "Erkek", "faceRectangle": {"sol": 355, "üst": 87, "width": "yükseklik" 143,: 143}}]
![İşleme ailesi Phot yüz analiz edin](./Images/family_photo_face.png) | [{"Yaş": 11, "cinsiyet": "Erkek", "faceRectangle": {"sol": "üst" 113: 314, "width": 222, "yükseklik": 222}}, {"Yaş": 11, "cinsiyet": "Kadın", "faceRectangle": {"sol": 1200, "üst": 632, "width": "yükseklik" 215,: 215}}, {"Yaş": 41, "cinsiyet": "Erkek", " faceRectangle": {"sol":"üst"514: 223,"width": 205,"yükseklik": 205}}, {"Yaş": 37,"cinsiyet":"Kadın","faceRectangle": {"sol":"üst"1008: 277,"width": 201,"yükseklik": 201}}]


## <a name="domain-specific-content"></a>Etki alanına özgü içerik

Buna ek olarak etiketleme ve üst düzey kategori, görüntü işleme API'si, özelleştirilmiş (veya etki alanına özgü) bilgi de destekler. Özel bilgiler, üst düzey kategori ile veya bir tek başına yöntem olarak uygulanabilir. Daha fazla alana özgü modeller aracılığıyla 86-kategori sınıflandırma iyileştirmek için bir yol olarak işlev görür.

Şu anda desteklenen tek özel bilgi ünlü tanıma ve önemli yer tanıma verilmiştir. Etki alanına özgü daraltmalar kişiler ve kişi grubu kategorileri ve dünyanın dört bir yanındaki yer işareti değildirler.

Alana özgü modeller kullanarak için iki seçenek vardır:

### <a name="option-one---scoped-analysis"></a>One - kapsamlı çözümleme seçeneği
Seçilen model yalnızca bir HTTP POST çağrısına çağırarak analiz edin. Bu seçenek için kullanmak istediğiniz hangi modelle biliyorsanız, modelin adı belirtin ve yalnızca bilgi ilgili Bu modele sahip olursunuz. Örneğin, ünlü tanıma için yalnızca aramak için bu seçeneği kullanın. Yanıt, olası güven puanlarını eşlik ünlüleri, eşleşen bir listesini içerir.

### <a name="option-two---enhanced-analysis"></a>Seçeneği iki - Gelişmiş analiz
Kategorileri 86 kategori Sınıflandırma, ilgili ek ayrıntılar sağlamak için analiz edin. Bu seçenek kullanıcıların bir veya daha fazla alana özgü modeller genel görüntü analizi ek ayrıntıları almak istediğiniz yere uygulamalarda kullanılabilir. Bu yöntem çağrıldığında 86 kategori sınıflandırma sınıflandırıcı önce çağrılır. Kategorilerden herhangi biri, bilinen ve eşleşen modellerin eşleşirse, ikinci bir sınıflandırıcı çağrılarını geçişi izler. Örneğin, ' Ayrıntılar = all', "details" 'ünlüleri' içerir'yi veya 86 kategori sınıflandırıcı çağrıldıktan sonra ünlü sınıflandırıcı yöntemini çağırır. Sonuç 'people_' ile başlayan etiketlerde içerir.

## <a name="generating-descriptions"></a>Açıklamaları oluşturma 
Görüntü işleme API'Sİ'ın algoritmaları, görüntü içeriği analiz edin. Bu analiz, tam cümlelerden okunabilir dili olarak görüntülenen bir 'description' için temel oluşturur. Açıklama, görüntüde bulunan özetler. Görüntü işleme API'Sİ'ın algoritmaları görüntüde tanımlanmış nesnelere göre çeşitli açıklamaları oluşturur. Açıklamaların her biri değerlendirilir ve bir güvenilirlik puanı oluşturulur. Ardından, güvenilirlik puanı için azalan düzende sıralı bir liste döndürülür. Resim yazıları oluşturmak için bu teknolojiyi kullanan bir robotun örneği bulunabilir [burada](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/intelligence-ImageCaption).  

### <a name="example-description-generation"></a>Örnek tanımı oluşturma
![B & W binalar](./Images/bw_buildings.png) '
```json
 Returned Json

'description':{
   "captions":[
      {
         "type":"phrase",
         'text':'a black and white photo of a large city',
         'confidence':0.607638706850331
      }
   ]   
   "captions":[
      {
         "type":"phrase",
         'text':'a photo of a large city',
         'confidence':0.577256764264197
      }
   ]   
   "captions":[
      {
         "type":"phrase",
         'text':'a black and white photo of a city',
         'confidence':0.538493271791207
      }
   ]   
   'description':[
      "tags":{
         "outdoor",
         "city",
         "building",
         "photo",
         "large",
      }
   ]
}
```

## <a name="perceiving-color-schemes"></a>Perceiving renk düzenleri
Görüntü işleme algoritması, bir resimden renkleri ayıklar. Renkler üç farklı bağlamda analiz edilir: ön plan, arka plan ve bütün resim. Bunlar, 12 baskın vurgu rengine ayrılarak gruplandırılır. Vurgu renklerdir siyah, mavi, brown, gri, yeşil, orange, pembe, mor, red, Deniz Mavisi, teknik ve sarı. Görüntü renkleri bağlı olarak, basit siyah beyaz mı vurgu rengine ayrılarak onaltılık renk kodlarını döndürülebilir.

Görüntü                                                       | Ön plan |Arka plan| Renkler
----------------------------------------------------------- | --------- | ------- | ------
![Dış Sıradağlar](./Images/mountain_vista.png)            | Siyah     | Siyah   | Beyaz
![İşleme çiçek analiz edin](./Images/flower.png)               | Siyah     | Beyaz   | Beyaz, siyah, yeşil
![Görüntü işleme Train istasyon analiz edin](./Images/train_station.png) | Siyah     | Siyah   | Siyah

### <a name="accent-color"></a>Vurgu rengi
Baskın renk doygunluğu ve karışık aracılığıyla kullanıcılara en çarpıcı rengi temsil etmek için tasarlanmış bir görüntüden ayıklanan rengi.

Görüntü                                                       | Yanıt
----------------------------------------------------------- | ----
![Dış Sıradağlar](./Images/mountain_vista.png)            | #BC6F0F
![İşleme çiçek analiz edin](./Images/flower.png)               | #CAA501
![Görüntü işleme Train istasyon analiz edin](./Images/train_station.png) | #484B83


### <a name="black--white"></a>Siyah & Beyaz
Bir resmin siyah olmadığını belirten bir Boole bayrağı beyaz veya yok.

Görüntü                                                      | Yanıt
---------------------------------------------------------- | ----
![İşleme analiz oluşturma](./Images/bw_buildings.png)      | True
![Görüntü merkezi Yard analiz edin](./Images/house_yard.png)      | False

## <a name="flagging-adult-content"></a>Yetişkinlere yönelik içeriğe işaretleme
Çeşitli visual kategorileri yetişkinlere yönelik malzemeleri olanak tanır ve görüntülerini cinsel içerik içeren görüntülenmesini sınırlar yetişkinlere yönelik ve müstehcen, grubudur. Yetişkinlere yönelik ve müstehcen içerik algılama için filtre kullanıcı tercihi uyum sağlamak için bir kaydırıcı ölçeğini üzerinde ayarlanabilir.

## <a name="optical-character-recognition-ocr"></a>Optik karakter tanıma (OCR)
OCR teknolojisi, bir resimdeki metin içeriğini algılar ve tanımlanan metin bir makine tarafından okunabilir bir karakter akışı halinde ayıklar. Sonuç, arama ve tıbbi kayıtları, güvenlik ve bankacılığa gibi çeşitli amaçlar için kullanabilirsiniz. Dili otomatik olarak algılar. OCR, zaman tasarrufu sağlar ve kullanıcılara metin yerine metnin fotoğrafını çekme olanağı tanıyarak kolaylık sunar.

OCR, 25 dilleri destekler. Bu diller: Arapça, Basitleştirilmiş Çince, Geleneksel Çince, Çekçe, Danca, Felemenkçe, İngilizce, Fince, Fransızca, Almanca, Yunanca, Macarca, İtalyanca, Japonca, Korece, Norveççe, Lehçe, Portekizce, Rumence, Rusça, Sırpça (Kiril ve Latin) Slovakça, İspanyolca, İsveççe ve Türkçe.

Gerekirse, OCR tanınan metnin yatay görüntü ekseni etrafında derece döndürme düzeltir. OCR, çizimde görüldüğü gibi her bir sözcüğün çerçeve koordinatları sağlar.

![OCR genel bakış](./Images/vision-overview-ocr.png) OCR gereksinimleri:
- Girdi görüntüsünün boyutu, 40 x 40 3200 x 3200 piksel arasında olmalıdır.
- Görüntü 10 megapiksel büyük olamaz.

Girdi görüntüsünün Döndürülmüş 90 derece birden fazla artı küçük bir açı'nın ' 40 tarafından derece.

Metin tanıma doğruluğunu görüntü kalitesini üzerinde bağlıdır. Yanlış bir okuma tarafından aşağıdaki durumlarda oluşabilir:
- Bulanık görüntüler.
- El yazısı veya el yazısı metin.
- Artistik yazı tipi stili.
- Küçük metin boyutu.
- Karmaşık bir arka plan, gölgeler veya içinde metin veya perspektif bozulma önleyici.
- Sözcükleri başındaki eksik veya büyük boyutlu büyük harf
- Alt simge, üst simge ya da üstü çizili metin.

Sınırlamalar: metin baskın olduğu fotoğrafları üzerinde kısmen tanınan sözcükleri hatalı pozitif sonuçları gelebilir. Bazı fotoğraflar, özellikle fotoğraf olmadan herhangi bir metin üzerinde duyarlık çok görüntü türüne bağlı olarak farklılık gösterebilir.

## <a name="recognize-handwritten-text"></a>Resimlerdeki el yazısı tanıma
Bu teknoloji, algılamanıza ve ayıklamanıza resimlerdeki el yazısı notları, harf, deneme, mektup, formlar, vb. kaynağı sağlar. Beyaz kağıt, sarı yapışkan notlar ve beyaz tahtalar gibi farklı yüzey ve arka planlarla çalışır.

El yazısı metinleri tanıma özelliği, hem zaman ve çabadan tasarruf etmenizi sağlar hem de metnin dökümünü çıkarmak yerine fotoğrafını çekme olanağı sunarak daha üretken olmanıza yardımcı olur. Notları dijitalleştirerek mümkün kılar. Bu digitization hızla ve kolayca arama olanak tanır. Tüm bunların yanı sıra kağıt karmaşasını da azaltır.

Giriş gereksinimleri:
- Desteklenen görüntü biçimleri: JPEG, PNG ve BMP.
- Resim dosyasının boyutu 4 MB'tan küçük olması gerekir.
- Görüntü boyutları en az 40 x 40 3200 x 3200'en fazla olmalıdır.

Not: Bu teknoloji şu an için önizleme aşamasındadır ve yalnızca İngilizce metinlerde kullanılabilir.

## <a name="generating-thumbnails"></a>Küçük resimler oluşturma
Bir küçük resim, bir tam boyutlu görüntüyü küçük bir gösterimidir. Telefonlar, tabletler ve bilgisayarlar gibi çeşitli cihazlar farklı bir kullanıcı deneyimi (UX) düzenleri ve küçük resim boyutları için bir gereksinim oluşturun. Bu görüntü işleme API'si özelliği, akıllı kırpma kullanarak sorunu çözmeye yardımcı olur.

Görüntü karşıya yükledikten sonra yüksek kaliteli bir küçük resim oluşturulur ve görüntü işleme API'si algoritması resim içindeki nesneleri analiz eder. Ardından 'ilgi Bölgesi' gereksinimlerini resmi kırpar (ROI). Çıkış, çizimde görüldüğü gibi özel bir çerçeve içinde görüntülenen. Oluşturulan küçük resmin en boy oranını kullanıcının karşılamak için özgün görüntü farklı bir en boy oranını kullanılarak sunulabilir.

Küçük resim algoritması gibi çalışır:

1. Görüntüyü dikkat dağıtıcı öğeleri kaldırır ve ana nesnesi, 'region' ilgi tanır (ROI).
2. İlgilendiğiniz tanımlanan bölgeye göre resmi kırpar.
3. En boy oranını hedef küçük resim boyutları uyacak şekilde değiştirir.

![Küçük Resimler](./Images/thumbnail-demo.png)
