---
title: Microsoft Bilişsel hizmetler için bilgisayar Vizyon API | Microsoft Docs
description: Gelişmiş algoritmalar bilgisayar görme API görüntüleri işlemek ve Microsoft Bilişsel Hizmetleri'nde döndürmesini yardımcı olması için kullanın.
services: cognitive-services
author: KellyDF
manager: corncar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 08/10/2017
ms.author: kefre
ms.openlocfilehash: 86e0441c600162e479c678d3cb1dbeaad423ddb5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354743"
---
# <a name="what-is-computer-vision-api-version-10"></a>Bilgisayar görme API sürümü 1.0 nedir?

> [!IMPORTANT]
> Yeni bir bilgisayar görme API sürümü artık kullanılabilir, bkz:
>- [Genel Bakış](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home)
>- [Bilgisayar görme API sürümü 2.0](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)

Bulut tabanlı bilgisayar görme API görüntülerin işlenmesi ve bilgileri döndürmek için gelişmiş algoritmalar erişimi geliştiricilere sağlar. Görüntüyü karşıya veya bir resim URL'si belirtme, Microsoft bilgisayar görme algoritmaları girişleri ve kullanıcı seçimlerini göre farklı şekillerde görsel içerik analiz edebilirsiniz. Bilgisayar görme API ile kullanıcılar görüntülere çözümleyebilirsiniz:
* [İçeriğe göre etiketini görüntüler.](#Tagging)
* [Görüntüleri kategorilere ayırma.](#Categorizing)
* [Görüntü kalitesini ve türünü tanımlayın.](#Identifying)
* [İnsan yüz algılama ve bunların koordinatları döndürür. ](#Faces)
* [Etki alanına özgü içerik farkındayız.](#Domain-Specific)
* [İçerik açıklamalarını oluşturur.](#Descriptions)
* [Optik karakter tanıma görüntüleri bulunan yazdırılan metin tanımlamak için kullanın.](#OCR)
* [El yazısı metin kabul eder.](#RecognizeText)
* [Renk düzenleri ayırt etmek.](#Color)
* [Yetişkinlere yönelik içeriğe bayrak.](#Adult)
* [Küçük resim olarak kullanılacak kırpma fotoğraf.](#Thumbnails)

## <a name="requirements"></a>Gereksinimler
* Giriş yöntemleri desteklenir: uygulama/octet akış veya resim URL'si biçiminde ikili ham görüntü.
* Resim biçimleri desteklenir: JPEG, PNG, GIF, BMP.
* Görüntü dosyası boyutu: 4 MB'tan az.
* Görüntü boyutu: 50 x 50 piksel büyüktür.

## <a name="tagging-images"></a>Görüntüleri etiketleme
Bilgisayar görme API 2000'den fazla tanınabilir nesneleri, yaşam önemlidir, manzara ve Eylemler göre etiketleri döndürür. Olmayan ortak bilgi etiketleri belirsiz olduğunda veya API yanıtını 'bilinen bir ayar bağlamında etiketinde anlamını açıklamak için ipuçları' sağlar. Etiketler bir sınıflandırma düzenlenir değil ve hiçbir devralma hiyerarşileri yok. İçerik etiketler koleksiyonunu görüntüyü 'tam cümlelerde biçimlendirilmiş İnsan okunabilir dili olarak görüntülenen açıklama' temelini oluşturur. Bu noktada İngilizce görüntü açıklaması için yalnızca desteklenen dil olduğunu unutmayın.

Görüntüyü karşıya veya bir resim URL'si belirtme sonra bilgisayar görme API'nin algoritmaları nesneleri, yaşam önemlidir ve görüntüde tanımlanan Eylemler temelinde etiketleri çıktı. Etiketleme ana konu, bir kişi ön planda gibi sınırlı değildir, ancak aynı zamanda ayarı (iç veya dış) Mobilya, Araçlar, bitkilerin, hayvanlar, Donatılar, araçlar vb. içerir.

### <a name="example"></a>Örnek
![House_Yard](./Images/house_yard.jpg) '

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
Etiketleme ve açıklamaları ek olarak, bilgisayar görme API önceki sürümlerde tanımlanmış sınıflandırma tabanlı kategorilerini döndürür. Üst/alt hereditary hiyerarşisi olan bir sınıflandırma olarak bu kategoriler halinde düzenlenir. Tüm kategorileri İngilizce'dir. Bunlar, tek başına veya yeni bizim modelleri ile kullanılabilir.

### <a name="the-86-category-concept"></a>86 kategori kavramı
Aşağıdaki diyagramda görüldüğü 86 kavramları listesini bağlı olarak, bir görüntüde bulunan görsel özelliklerini geniş özgü arasında değişen sınıflandırılabilir. Metin biçiminde tam sınıflandırma için bkz: [kategori sınıflandırma](https://docs.microsoft.com/azure/cognitive-services/computer-vision/category-taxonomy).

![Kategoriler Çözümle](./Images/analyze_categories.jpg)

Görüntü                                                  | Yanıt
------------------------------------------------------ | ----------------
![Kadın tavan](./Images/woman_roof.jpg)                 | kişiler
![Aile fotoğrafı](./Images/family_photo.jpg)             | people_crowd
![Şirin köpek](./Images/cute_dog.jpg)                     | animal_dog
![Dış Sıradağlar](./Images/mountain_vista.jpg)       | outdoor_mountain
![Görme analiz yemek ekmek](./Images/bread.jpg)       | food_bread

## <a name="identifying-image-types"></a>Resim türleri tanımlama
Görüntüleri kategorilere ayırmak için birkaç yolu vardır. Bilgisayar görme API görüntü siyah beyaz veya renk olup olmadığını gösteren bir Boole bayrağı ayarlayabilirsiniz. Ayrıca, görüntünün bir çizim olup olmadığını belirten bir bayrak de ayarlayabilirsiniz. Ayrıca, görüntünün küçük resim mi ve bu nedenle bir ölçekte kalitesini 0-3'ün belirtmek belirtebilirsiniz.

### <a name="clip-art-type"></a>Küçük resim türü
Görüntünün küçük resim olup olmadığını algılar.  

Değer | Anlamı
----- | --------------
0     | Olmayan küçük resim
1     | belirsiz
2     | Normal küçük resim
3     | iyi küçük resim

Görüntü|Yanıt
----|----
![Görme analiz Peynir küçük resim](./Images/cheese_clipart.jpg)|3 iyi küçük-resim
![Görme House Bahçe Çözümle](./Images/house_yard.jpg)|0 olmayan küçük-resim

### <a name="line-drawing-type"></a>Çizgi çizme türü
Görüntüyü bir çizim olup olmadığını algılar.

Görüntü|Yanıt
----|----
![Görme analiz Lion çizim](./Images/lion_drawing.jpg)|True
![Görme çiçek Çözümle](./Images/flower.jpg)|False

### <a name="faces"></a>Yüzler
Resim içinde İnsan yüzeyleri algılar ve yüz koordinatları, yüz, cinsiyetiniz ve yaş için dikdörtgen oluşturur. Bu görsel özellikleri yüz için oluşturulan meta verilerinin bir alt kümesidir. Yazıtipleri (yüz tanımlama, poz algılama ve daha fazla) oluşturulan daha kapsamlı meta verilerini, yüz API'yi kullanın.  

Görüntü|Yanıt
----|----
![Görme kadın tavan yüz Çözümle](./Images/woman_roof_face.png) | [{"Yaş": 23 "Cinsiyeti": "Kadın", "faceRectangle": {"sol": "üst" 1379: 320, "genişliği": 310, "yüksekliği": 310}}]
![Görme Mom dişi yüz Çözümle](./Images/mom_daughter_face.png) | [{"Yaş": 28, "Cinsiyeti": "Kadın", "faceRectangle": {"sol": 447, "üst": 195, "genişliği": 162, "yüksekliği": 162}}, {"Yaş": 10, "Cinsiyeti": "Erkek", "faceRectangle": {"sol": 355, "üst": 87, "genişliği": 143, "yüksekliği": 143}}]
![Görme ailesi Phot yüz Çözümle](./Images/family_photo_face.png) | [{"Yaş": 11, "Cinsiyeti": "Erkek", "faceRectangle": {"sol": 113, "üst": 314, "genişliği": 222, "yüksekliği": 222}}, {"Yaş": 11, "Cinsiyeti": "Kadın", "faceRectangle": {"sol": 1200, "üst": 632, "genişliği": 215, "yüksekliği": 215}}, {"Yaş": 41, "Cinsiyeti": "Erkek", " faceRectangle": {"sol": 514,"üst": 223,"genişliği": 205,"yüksekliği": 205}}, {"Yaş": 37,"Cinsiyeti":"Kadın","faceRectangle": {"sol": 1008,"üst": 277,"genişliği": 201,"yüksekliği": 201}}]


## <a name="domain-specific-content"></a>Etki alanına özgü içerik

Ayrıca etiketleme ve en üst düzey kategori, bilgisayar görme API özelleştirilmiş (veya etki alanına özgü) bilgilerini de destekler. Özel bilgiler, bir tek başına yöntem olarak veya üst düzey kategori uygulanabilir. Daha fazla etki alanına özgü modelleri eklenmesi aracılığıyla 86 kategori sınıflandırma daraltmak için bir yol görür.

Şu anda desteklenen tek özel bilgi ünlülerle tanıma ve yer işareti tanıma verilmiştir. Etki alanına özgü düzeltmeler kişiler ve kişi Grup kategorileri ve bilinen yerler dünyanın oldukları.

Etki alanına özgü modelleri kullanarak için iki seçenek vardır:

### <a name="option-one---scoped-analysis"></a>Seçeneği bir - kapsamlı çözümleme
Seçilen model yalnızca bir HTTP POST çağrısına çağırarak analiz edin. Bu seçenek için kullanmak istediğiniz hangi model biliyorsanız, modelin adı belirtin ve, yalnızca bilgi modelin ilgili alırsınız. Örneğin, yalnızca ünlülerle tanıma için aramak için bu seçeneği kullanın. Yanıt güvenirlik puanlarını eşlik çok ünlüler eşleşen olası listesini içerir.

### <a name="option-two---enhanced-analysis"></a>İki - Gelişmiş analiz seçeneği
Kategorilere 86 kategori Sınıflandırma, ilgili ayrıntılı bilgi sağlamayı analiz edin. Bu seçenek kullanıcılar bir veya daha fazla etki alanına özgü modellerinden ayrıntıları yanı sıra genel görüntü analiz almak istediğiniz yere uygulamalarda kullanmak için kullanılabilir. Bu yöntem çağrıldığında, 86 kategori sınıflandırma sınıflandırıcı önce çağırılır. Kategorilerden herhangi biri, bilinen ve eşleşen modellerin eşleşiyorsa, ikinci bir sınıflandırıcı çağrılarını geçişi izler. Örneğin, varsa ' Ayrıntılar = all' veya 'çok ' ünlüler "Ayrıntılar" dahil, 86 kategori sınıflandırıcı çağrıldıktan sonra ünlülerle sınıflandırıcı yöntemini çağırır. Sonuç 'people_' ile başlayan etiketler içerir.

## <a name="generating-descriptions"></a>Açıklamaları oluşturma 
Bilgisayar görme API'nin algoritmaları görüntü içeriği analiz edin. Bu çözümleme 'bütün cümleleri okunabilir dilde olarak görüntülenen bir açıklama' temelini oluşturur. Açıklama görüntüde bulunan özetler. Bilgisayar görme API'nin algoritmaları görüntüde tanımlanan nesneleri göre çeşitli açıklamaları oluşturur. , Her değerlendirilir ve bir güven puan oluşturulan açıklamalardır. Bir liste sonra yüksek güvenilirlik puan düşüğe sıralı döndürülür. Resim yazıları oluşturmak için bu teknolojisini kullanan bir bot örneği bulunabilir [burada](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/intelligence-ImageCaption).  

### <a name="example-description-generation"></a>Örnek açıklaması oluşturma
![B & W binalar](./Images/bw_buildings.jpg) '
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
Bilgisayar görme algoritması renkleri görüntüden ayıklar. Renkler üç farklı bağlamda analiz edilir: ön plan, arka plan ve bütün resim. Bunlar, on iki 12 baskın Vurgu Renkleri gruplandırılır. Bu Vurgu Renkleri siyah, mavi, Kahverengi, gri, yeşil, turuncu, pembe, mor, kırmızı, Deniz, beyaz ve sarı. Görüntü renkleri bağlı olarak, basit siyah beyaz veya vurgu renkleri onaltılı renk kodları döndürülebilir.

Görüntü                                                       | Ön plan |Arka plan| Renkler
----------------------------------------------------------- | --------- | ------- | ------
![Dış Sıradağlar](./Images/mountain_vista.jpg)            | Siyah     | Siyah   | Beyaz
![Görme çiçek Çözümle](./Images/flower.jpg)               | Siyah     | Beyaz   | Beyaz, siyah, yeşil
![Görme analiz tren istasyonu](./Images/train_station.jpg) | Siyah     | Siyah   | Siyah

### <a name="accent-color"></a>Aksan rengi
Baskın renkleri ve Doygunluk bir karışımını üzerinden kullanıcılara en çarpıcı rengi temsil etmek için tasarlanmış bir görüntüden ayıklanan rengi.

Görüntü                                                       | Yanıt
----------------------------------------------------------- | ----
![Dış Sıradağlar](./Images/mountain_vista.jpg)            | #BC6F0F
![Görme çiçek Çözümle](./Images/flower.jpg)               | #CAA501
![Görme analiz tren istasyonu](./Images/train_station.jpg) | #484B83


### <a name="black--white"></a>Siyah beyaz
Görüntüyü siyah olup olmadığını belirten Boolean bayrağı beyaz veya değil.

Görüntü                                                      | Yanıt
---------------------------------------------------------- | ----
![Görme yapı çözümleme](./Images/bw_buildings.jpg)      | True
![Görme House Bahçe Çözümle](./Images/house_yard.jpg)      | False

## <a name="flagging-adult-content"></a>Yetişkinlere yönelik içeriğe işaretleme
Çeşitli visual kategorileri arasında yetişkin malzemeleri algılanmasını sağlar ve cinsel içerik içeren görüntüleri görüntüsünü sınırlayan yetişkin ve saldırganlardan grup değil. Yetişkin ve saldırganlardan içerik algılama için filtre kaydırıcı ölçeğini üzerinde kullanıcı tercihi uyum sağlayacak şekilde ayarlanabilir.

## <a name="optical-character-recognition-ocr"></a>Optik karakter tanıma
OCR teknolojisi metin içeriğini bir resim algılar ve bir makine tarafından okunabilir karakter akışa tanımlanan metin ayıklar. Sonuç, arama ve tıbbi kayıtları, güvenlik ve bankacılık gibi çok sayıda diğer amaçlar için kullanabilirsiniz. Dili otomatik olarak algılar. OCR zamandan tasarruf sağlar ve bunları metin çoğaltmaya yerine metin fotoğraflarını yapılacak vererek kullanıcılar için kolaylık sağlar.

OCR 25 dilleri destekler. Bu diller: Arapça, Çince, Basitleştirilmiş Çince, Geleneksel Çince, Çekçe, Danca, Felemenkçe, İngilizce, Fince, Fransızca, Almanca, Yunanca, Macarca, İtalyanca, Japonca, Korece, Norveççe, Lehçe, Portekizce, Rumence, Rusça, Sırpça (Kiril ve Latin) Slovakça, İspanyolca, İsveççe ve Türkçe.

Gerekirse, OCR tanınan metnin yatay görüntü ekseni etrafında derece döndürme düzeltir. Çizimde görülen her sözcüğün çerçeve koordinatları OCR sağlar.

![OCR genel bakış](./Images/vision-overview-ocr.png) OCR gereksinimleri:
- Giriş görüntü boyutu, 40 x 40 ile 3200 x 3200 piksel arasında olmalıdır.
- Görüntü 10 megapiksel büyük olamaz.

Girdi görüntüsü döndürülüp döndürülemeyeceğini birden fazla 90 derece artı küçük açı, ' 40 tarafından derece.

Metin tanıma doğruluğu görüntü kalitesini bağlıdır. Yanlış bir okuma tarafından aşağıdaki durumlarda kaynaklanabilir:
- Bulanık görüntüler.
- El yazısı veya el yazısı metni.
- Artistik yazı tipi stillerini.
- Küçük metin boyutu.
- Karmaşık arka plan, gölge ya da metin veya perspektif bozulmayı üzerinden önleyici.
- Sözcükler başındaki büyük boyutlu ya da eksik büyük harf
- Alt simge, simgeyi veya üstü çizili metni.

Sınırlamaları: metin baskın olduğu fotoğrafları üzerinde kısmen tanınan sözcükleri hatalı pozitif sonuç ortaya çıkabilir. Özellikle herhangi bir metin olmadan fotoğraf bazı fotoğraflar üzerine duyarlık çok görüntü türüne bağlı olarak farklılık gösterebilir.

## <a name="recognize-handwritten-text"></a>El yazısı metni tanıma
Bu teknoloji algılamak ve el yazısı metni ayıklayın notları, harf, deneme, Beyaz Tahta, formlar, vb. kaynağı olanak sağlar. Beyaz kağıt, sarı yapışkan notlar ve beyaz tahtalar gibi farklı yüzey ve arka planlarla çalışır.

El yazısı metinleri tanıma özelliği, hem zaman ve çabadan tasarruf etmenizi sağlar hem de metnin dökümünü çıkarmak yerine fotoğrafını çekme olanağı sunarak daha üretken olmanıza yardımcı olur. Bu Notlar digitize mümkün kılar. Bu digitization hızlı ve kolay arama uygulanmasını sağlar. Tüm bunların yanı sıra kağıt karmaşasını da azaltır.

Giriş gereksinimleri:
- Resim biçimleri desteklenir: JPEG, PNG ve BMP.
- Görüntü dosya boyutu 4 MB'den az olmalıdır.
- Görüntü boyutları, en az 40 x 40, en çok 3200 x 3200 olmalıdır.

Not: Bu teknoloji şu an için önizleme aşamasındadır ve yalnızca İngilizce metinlerde kullanılabilir.

## <a name="generating-thumbnails"></a>Küçük resimler oluşturma
Küçük resim tam boyutlu görüntüyü küçük bir gösterimidir. Telefon, Tablet ve bilgisayar gibi çeşitli aygıtları farklı bir kullanıcı deneyimi (UX) düzenleri ve küçük resim boyutları gereksinimini oluşturun. Bu bilgisayar görme API özelliği, akıllı kırpma kullanarak sorunu çözmeye yardımcı olur.

Görüntüyü karşıya yükledikten sonra bir yüksek kaliteli küçük resim oluşturulan ve bilgisayar görme API algoritması görüntü nesnelerinde analiz eder. Ardından 'region' ilgi gereksinimlerine uyacak şekilde resmi kırpar (ROI). Çıkış, çizimde görüldüğü gibi özel bir Framework'te görüntülenen. Bir kullanıcının ihtiyaçlarını karşılamak için özgün görüntüsüne en boy oranını farklı bir en/boy oranını kullanılarak oluşturulan küçük resim sunulabilir.

Küçük resim algoritması gibi çalışır:

1. Rahatsız edici öğeleri görüntüden kaldırır ve ana nesne, 'region' ilgi tanıdığı (ROI).
2. İlgilenilen tanımlanan bölgeye göre resmi kırpar.
3. Hedef küçük resim boyutlarına en boy oranını değiştirir.

![Küçük Resimler](./Images/thumbnail-demo.png)
