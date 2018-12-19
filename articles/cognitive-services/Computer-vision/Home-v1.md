---
title: Görüntü İşleme API’si nedir?
titlesuffix: Azure Cognitive Services
description: Görüntü İşleme API'si geliştiricilerin görüntü işlemeye ve bilgi döndürmeye yönelik gelişmiş algoritmalara erişmesini sağlar.
services: cognitive-services
author: KellyDF
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: overview
ms.date: 08/10/2017
ms.author: kefre
ms.custom: seodec18
ms.openlocfilehash: acd6d41e8b6d1fb834697ec3d026419ee6b69ec9
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2018
ms.locfileid: "53582363"
---
# <a name="what-is-computer-vision-api-version-10"></a>Görüntü İşleme API'si Sürüm 1.0 nedir?

> [!IMPORTANT]
> Görüntü İşleme API'sinin yeni sürümü kullanıma sunulmuştur. Bkz:
>- [Genel Bakış](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home)
>- [Görüntü İşleme API'si Sürüm 2.0](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)

Bulut tabanlı Görüntü İşleme API’si, geliştiriciler için görüntüleri işlemeye ve bilgileri döndürmeye yönelik gelişmiş algoritmalara erişim sağlar. Microsoft Görüntü İşleme algoritmaları bir görüntüyü karşıya yükleyerek veya bir görüntü URL’si belirterek girdilere ve kullanıcı seçimlerine göre görsel içerikleri farklı şekillerde analiz edebilir. Görüntü İşleme API'siyle kullanıcılar görüntüleri analiz ederek şunları yapabilir:
* [Görüntüleri içeriğine göre etiketleme.](#Tagging)
* [Görüntüleri kategorilere ayırma.](#Categorizing)
* [Görüntülerin türünü ve kalitesini tanımlama.](#Identifying)
* [İnsan yüzlerini algılama ve bunların koordinatlarını döndürme. ](#Faces)
* [Etki alanına özgü içeriği tanıma.](#Domain-Specific)
* [İçerik açıklamaları oluşturma.](#Descriptions)
* [Görüntülerde bulunan basılı metni tanımlamak için optik karakter tanıma özelliğini kullanma.](#OCR)
* [El yazısı metinleri tanıma.](#RecognizeText)
* [Renk düzenlerini ayırt etme.](#Color)
* [Yetişkinlere yönelik içeriğe bayrak ekleme.](#Adult)
* [Fotoğrafları küçük resim olarak kullanılabilecek şekilde kırpma.](#Thumbnails)

## <a name="requirements"></a>Gereksinimler
* Giriş yöntemleri desteklenir: İkili bir uygulama/octet stream veya resim URL'si biçiminde ham görüntü.
* Resim biçimleri desteklenir: JPEG, PNG, GIF, BMP.
* Resim dosyasının boyutu: 4 MB'den daha küçük.
* Görüntü boyutu: 50 x 50 piksel büyüktür.

## <a name="tagging-images"></a>Görüntüleri Etiketleme
Görüntü işleme API'si, tanınabilir nesne, canlı, manzara ve Eylemler binlerce alan etiketler döndürür. Belirsiz veya herkesçe bilinmeyen etiketler söz konusu olduğunda, API yanıtı, etiketin anlamının bilinen bir ortama ilişkin bağlamda açıklığa kavuşturulması için "ipuçları" sağlar. Etiketler taksonomi olarak tanınmaz ve hiçbir devralma hiyerarşisi yoktur. Bir içerik etiketi koleksiyonu, tam tümceler halinde biçimlendirilmiş insan tarafından okunabilir dilde görüntülenen bir görüntü 'açıklamasının' temelini oluşturur. Şu noktada görüntü açıklaması için desteklenen tek dilin İngilizce olduğunu unutmayın.

Görüntüyü karşıya yükledikten veya bir görüntü URL'si belirttikten sonra, Görüntü İşleme API'sinin algoritmaları görüntüde tanımlanan eşyalar, canlılar ve eylemlere dayalı olarak etiketleri verir. Etiketleme yalnızca temel konu ile sınırlı kalmayıp ortam (iç mekân veya dış mekân), mobilyalar, aletler, bitkiler, hayvanlar, aksesuarlar, araçlar ve benzeri öğeleri de kapsar.

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
## <a name="categorizing-images"></a>Görüntüleri Kategorilere Ayırma
Etiketlerin ve açıklamaların yanı sıra, Görüntü İşleme API'si önceki sürümlerde tanımlanmış taksonomi tabanlı kategorileri de döndürür. Bu kategoriler, üst/alt kalıtsal hiyerarşileriyle bir taksonomi olarak düzenlenmiştir. Tüm kategoriler İngilizcedir. Bunlar tek başına veya yeni modellerimizle birlikte kullanılabilir.

### <a name="the-86-category-concept"></a>86 kategori kavramı
Aşağıdaki diyagramda gösterilen 86 kavramın listesi temelinde, bir görüntüde bulunan görsel özellikler genelden özele doğru kategorilere ayrılabilir. Metin biçiminde tam taksonomi için bkz. [Kategori Taksonomisi](https://docs.microsoft.com/azure/cognitive-services/computer-vision/category-taxonomy).

![Kategorileri Analiz Etme](./Images/analyze_categories.png)

Görüntü                                                  | Yanıt
------------------------------------------------------ | ----------------
![Çatıda Kadın](./Images/woman_roof.png)                 | people
![Aile Fotoğrafı](./Images/family_photo.png)             | people_crowd
![Şirin Köpek](./Images/cute_dog.png)                     | animal_dog
![Dış Mekanda Dağ](./Images/mountain_vista.png)       | outdoor_mountain
![Görüntü Analizi Yiyecek Ekmek](./Images/bread.png)       | food_bread

## <a name="identifying-image-types"></a>Görüntü Türünü Tanımlama
Görüntüler çeşitli yollarla kategorilere ayrılabilir. Görüntü İşleme API'si, bir görüntünün siyah beyaz mı yoksa renkli mi olacağını belirten bir Boole bayrağı ayarlayabilir. Ayrıca, görüntünün bir çizim olup olmadığını belirten bir bayrak da ayarlayabilir. Görüntünün küçük resim olup olmadığını ve 0-3 ölçeğinde kalitesini belirtebilir.

### <a name="clip-art-type"></a>Küçük resim türü
Görüntünün küçük resim olup olmadığını algılar.  

Değer | Anlamı
----- | --------------
0     | Küçük resim değil
1     | belirsiz
2     | normal küçük resim
3     | iyi küçük resim

Görüntü|Yanıt
----|----
![Görüntü Analizi Peynir Küçük Resmi](./Images/cheese_clipart.png)|3 iyi küçük resim
![Görüntü Analizi Ev Bahçesi](./Images/house_yard.png)|0 Küçük resim değil

### <a name="line-drawing-type"></a>Çizim türü
Görüntünün bir çizim olup olmadığını algılar.

Görüntü|Yanıt
----|----
![Görüntü Analizi Aslan Çizimi](./Images/lion_drawing.png)|True
![Görüntü Analizi Çiçek](./Images/flower.png)|False

### <a name="faces"></a>Yüzler
Resmin içindeki insan yüzlerini algılar ve yüz koordinatlarını, yüz için dikdörtgen, cinsiyet ve yaş bilgilerini oluşturur. Bu görsel özellikler, yüz için oluşturulan meta verilerin bir alt kümesidir. Yüz için oluşturulan daha kapsamlı meta veriler (yüz tanımlama, poz algılama ve diğerleri) için, Yüz Tanıma API'sini kullanın.  

Görüntü|Yanıt
----|----
![Görüntü Analizi Damdaki Kadının Yüzü](./Images/woman_roof_face.png) | [{"Yaş": "cinsiyet" 23: "Kadın", "faceRectangle": {"sol": 1379, "üst": 320, "width": 310, "yükseklik": 310}}]
![Görüntü Analizi Anne Kız Yüzü](./Images/mom_daughter_face.png) | [{"Yaş": 28, "cinsiyet": "Kadın", "faceRectangle": {"sol": 447, "üst": 195, "width": 162, "yükseklik": 162}}, {"Yaş": "cinsiyet" 10: "Erkek", "faceRectangle": {"sol": 355, "üst": 87, "width": 143 "yükseklik": 143}}]
![Görüntü Analizi Aile Fot Yüzü](./Images/family_photo_face.png) | [{"Yaş": "cinsiyet" 11: "Erkek", "faceRectangle": {"sol": 113 "üst": 314 "width": 222, "yükseklik": 222}}, {"Yaş": "cinsiyet" 11: "Kadın", "faceRectangle": {"sol": 1200, "üst": 632, "width": 215 "yükseklik": 215}}, {"Yaş": 41, "cinsiyet": "Erkek", "faceRectangle": {"sol": 514, "üst": "width" 223: 205, "yükseklik": 205}}, {"Yaş": "cinsiyet" 37: "Kadın", "faceRectangle": {"sol": 1008, "üst": 277, "width": 201, "yükseklik": 201}}]


## <a name="domain-specific-content"></a>Etki Alanına Özgü İçerik

Üst düzey kategorileri etiketlemeye ek olarak, Görüntü İşleme API'si özelleştirilmiş (veya etki alanına özgü) bilgileri de destekler. Özelleştirilmiş bilgiler tek başına bir yöntem olarak uygulanabileceği gibi, üst düzey kategorilerle de uygulanabilir. Etki alanına özgü modellerin eklenmesiyle 86 kategori taksonomisini daha da geliştirmek için bir araç işlevi görür.

Şu anda, desteklenen özelleştirilmiş bilgiler yalnızca ünlü tanıma ve yer tanımadır. Bunlar, kişi ve kişi grubu kategorileri için etki alanına özgü geliştirmeler ve dünyanın her yanındaki yerlerdir.

Etki alanına özgü modelleri kullanmak için iki seçenek vardır:

### <a name="option-one---scoped-analysis"></a>Seçenek Bir - Kapsamı Belirlenmiş Analiz
Bir HTTP POST çağrısı yaparak yalnızca seçilen modeli analiz edin. Bu seçenek için, hangi modeli kullanmak istediğinizi biliyorsanız modelin adını belirtirsiniz ve yalnızca bu modelle ilgili bilgileri alırsınız. Örneğin, bu seçeneği kullanarak yalnızca ünlü tanıma için arama yapabilirsiniz. Sonuç, eşleşen olası ünlülerin listesiyle birlikte bunların güvenilirlik puanını da içerir.

### <a name="option-two---enhanced-analysis"></a>Seçeneği İki - Gelişmiş Analiz
86 kategori taksonomisindeki kategorilerle ilgili ek ayrıntılar sağlamak için analiz edin. Bu seçenek bir veya daha fazla etki alanına özgü modelde yer alan ayrıntılara ek olarak kullanıcının genel görüntü analizi de almak istediği uygulamalarda kullanılmaya yöneliktir. Bu yöntem çağrıldığında, önce 86 kategori taksonomisinin sınıflandırıcısı çağrılır. Herhangi bir kategori, bilinen/eşleşen modellerin kategorileriyle eşleşiyorsa, ardından ikinci bir sınıflandırıcı çağrısı geçişi yapılır. Örneğin, 'details=all' veya "details" 'celebrities' içeriyorsa, 86 kategori sınıflandırıcısı çağrıldıktan sonra yöntem ünlü sınıflandırıcısını çağırır. Sonuç, 'people_' ile başlayan etiketleri içerir.

## <a name="generating-descriptions"></a>Açıklamaları Oluşturma 
Görüntü İşleme API'sinin algoritmaları görüntünün içeriğini analiz eder. Bu analiz, tam tümceler halinde insan tarafından okunabilir dilde görüntülenen bir 'açıklamanın' temelini oluşturur. Açıklama, görüntüde nelerin bulunduğunu özetler. Görüntü İşleme API'sinin algoritmaları, görüntüde tanımlanan nesneleri temel alan çeşitli açıklamalar oluşturur. Açıklamaların her biri değerlendirilir ve bir güvenilirlik puanı oluşturulur. Ardından, güvenilirlik puanı için azalan düzende sıralı bir liste döndürülür. Resim yazıları oluşturmak üzere bu teknolojinin kullanıldığı bir bot örneğine [buradan](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/intelligence-ImageCaption) ulaşılabilir.  

### <a name="example-description-generation"></a>Örnek Açıklama Oluşturma
![Siyah Beyaz Binalar](./Images/bw_buildings.png) '
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

## <a name="perceiving-color-schemes"></a>Renk Düzenlerini Algılama
Görüntü İşleme algoritması, görüntüdeki renkleri ayıklar. Renkler üç farklı bağlamda analiz edilir: ön plan, arka plan ve bütün resim. Bunlar 12 adet baskın vurgu rengine ayrılarak gruplandırılır. Bu vurgu renkleri beyaz, deniz mavisi, gri, kahverengi, kırmızı, mavi, mor, pembe, sarı, siyah, turuncu ve yeşildir. Görüntüdeki renklere bağlı olarak, basit siyah ve beyaz veya vurgu renkleri onaltılık renk kodlarıyla döndürülebilir.

Görüntü                                                       | Ön plan |Arka plan| Renkler
----------------------------------------------------------- | --------- | ------- | ------
![Dış Mekanda Dağ](./Images/mountain_vista.png)            | Siyah     | Siyah   | Beyaz
![Görüntü Analizi Çiçek](./Images/flower.png)               | Siyah     | Beyaz   | Beyaz, Siyah, Yeşil
![Görüntü Analizi Tren İstasyonu](./Images/train_station.png) | Siyah     | Siyah   | Siyah

### <a name="accent-color"></a>Vurgu rengi
Baskın renkler ve doygunluğun bir bileşimiyle kullanıcılara en göz alıcı renkleri göstermek için tasarlanmış bir görüntüden ayıklanan renkler.

Görüntü                                                       | Yanıt
----------------------------------------------------------- | ----
![Dış Mekanda Dağ](./Images/mountain_vista.png)            | #BC6F0F
![Görüntü Analizi Çiçek](./Images/flower.png)               | #CAA501
![Görüntü Analizi Tren İstasyonu](./Images/train_station.png) | #484B83


### <a name="black--white"></a>Siyah Beyaz
Görüntünün siyah beyaz olup olmadığını gösteren Boole bayrağı.

Görüntü                                                      | Yanıt
---------------------------------------------------------- | ----
![Görüntü Analizi Bina](./Images/bw_buildings.png)      | True
![Görüntü Analizi Ev Bahçesi](./Images/house_yard.png)      | False

## <a name="flagging-adult-content"></a>Yetişkinlere Yönelik İçeriğe Bayrak Ekleme
Çeşitli görsel kategoriler arasında yetişkinlere yönelik ve müstehcen içerik grubu, yetişkinlere yönelik malzemeleri algılamaya olanak tanır ve cinsel içerikli görüntülerin gösterilmesini kısıtlar. Yetişkinlere yönelik veya müstehcen içeriklerin algılanmasına yönelik filtre, kullanıcının tercihleri doğrultusunda bir kaydırıcı ölçeği üzerinde ayarlanabilir.

## <a name="optical-character-recognition-ocr"></a>Optik Karakter Tanıma (OCR)
OCR teknolojisi bir görüntüdeki metin içeriğini algılar ve tanımlanan metni makine tarafından okunabilen bir karakter akışına ayıklar. Sonucu, arama yapmak için veya tıbbi kayıtlar, güvenlik ve bankacılık gibi çok çeşitli amaçlarla kullanabilirsiniz. Dili otomatik olarak algılar. OCR, zaman tasarrufu sağlar ve kullanıcılara, metni yazmak yerine bunların fotoğrafını çekme olanağı tanıyarak kolaylık sunar.

OCR, 25 dili destekler. Bu diller şunlardır: Arapça, Çince, Geleneksel Çince, Basitleştirilmiş Çekçe, Danca, Felemenkçe, İngilizce, Fince, Fransızca, Almanca, Yunanca, Macarca, İtalyanca, Japonca, Korece, Norveççe, Lehçe, Portekizce, Rumence, Rusça, Sırpça (Kiril ve Latin), Slovakya, İspanyolca, İsveççe ve Türkçe.

Gerekirse, OCR tanınan metnin yönünü yatay görüntü ekseninde derece olarak düzeltir. OCR, aşağıdaki çizimde gösterildiği gibi her sözcük için çerçeve koordinatlarını verir.

![OCR'ye Genel Bakış](./Images/vision-overview-ocr.png) OCR Gereksinimleri:
- Giriş görüntüsünün boyutu 40 x 40 ile 3200 x 3200 piksel arasında olmalıdır.
- Görüntü 10 megapikselden büyük olamaz.

Giriş görüntüsü 90 derecenin katlarında ve ayrıca 40 dereceye kadar küçük açılarda döndürülebilir.

Metin tanımanın doğruluğu görüntünün kalitesine bağlıdır. Aşağıdaki durumlar yanlış okumaya yol açabilir:
- Bulanık görüntüler.
- El yazısı veya bitişik el yazısı metin.
- Artistik yazı tipi stilleri.
- Küçük metin boyutu.
- Karmaşık arka planlar, gölgeler ya da metnin üzerinde parlama veya perspektif bozukluk.
- Sözcüklerin başındaki aşırı büyük veya eksik büyük harfler
- Alt simge, üst simge veya üstü çizili metin.

Sınırlamalar: Metin baskın olduğu fotoğraflar, hatalı pozitif sonuçları kısmen tanınan sözcükleri gelebilir. Bazı fotoğraflarda, özellikle de hiç metin bulunmayan fotoğraflarda görüntünün türüne bağlı olarak duyarlık çok değişebilir.

## <a name="recognize-handwritten-text"></a>El Yazısı Metinleri Tanıma
Bu teknoloji el yazısı not, mektup, ödev, tahta ve form gibi kaynaklardaki metinleri algılamanıza ve ayıklamanıza olanak tanır. Beyaz kağıt, sarı yapışkan notlar ve beyaz tahtalar gibi farklı yüzey ve arka planlarla çalışır.

El yazısı metinleri tanıma özelliği, hem zaman ve çabadan tasarruf etmenizi sağlar hem de metnin dökümünü almak yerine fotoğrafını çekme olanağı sunarak daha üretken olmanıza yardımcı olur. Notları dijital ortama geçirmeyi mümkün kılar. Bu dijitalleştirme hızlı ve kolay aramalar yapmanızı sağlar. Üstelik kağıt dağınıklığını da azaltır.

Giriş gereksinimleri:
- Resim biçimleri desteklenir: JPEG, PNG ve BMP.
- Görüntü dosyası boyutu 4 MB’tan az olmalıdır.
- Görüntü boyutları en az 40 x 40 ve en fazla 3200 x 3200 olmalıdır.

Not: Bu teknoloji şu an için önizleme aşamasındadır ve yalnızca İngilizce metinlerde kullanılabilir.

## <a name="generating-thumbnails"></a>Küçük Resimler Oluşturma
Küçük resim, tam boyutlu bir görüntünün küçük gösterimidir. Telefon, tablet ve PC gibi çeşitli cihazlar, farklı kullanıcı deneyimi (UX) düzenleri ve küçük resim boyutları gereğini ortaya çıkarmıştır. Akıllı kırpmayı kullanan bu Görüntü İşleme API'si özelliği, bu sorunu çözmeye yardımcı olur.

Görüntüyü karşıya yükledikten sonra, yüksek kaliteli bir küçük resim oluşturulur ve Görüntü İşleme API'sinin algoritması görüntüdeki nesneleri analiz eder. Ardından, 'ilgi' gereksinimlerini resmi kırpar. Çıkış, aşağıdaki resimde gösterildiği gibi özel bir çerçeve içinde görüntülenir. Oluşturulan küçük resim, kullanıcının ihtiyaçlarını karşılayacak şekilde, özgün görüntünün en boy oranından farklı bir en boy oranı kullanılarak sunulabilir.

Küçük resim algoritması şöyle çalışır:

1. Görüntüden dikkat dağıtıcı öğeleri kaldırır ve bir ana nesnesi, 'ilgi' tanır.
2. İlgilendiğiniz tanımlanan alana dayalı resmi kırpar.
3. Hedef küçük resmin boyutlarına uyması için en boy oranını değiştirir.

![Küçük Resimler](./Images/thumbnail-demo.png)
