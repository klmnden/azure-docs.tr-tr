---
title: 'Örnek: AdventureWorks stok veritabanı - Azure Search modeli'
description: Azure Search'te dizin oluşturma ve tam metin araması için düzleştirilmiş veri kümesine dönüştürme, ilişkisel bir veri modeli hakkında bilgi edinin.
author: cstone
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 01/25/2019
ms.author: chstone
ms.openlocfilehash: 6d5d01dfbbcfda56818f5c38b06117a87e021445
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61291918"
---
# <a name="example-model-the-adventureworks-inventory-database-for-azure-search"></a>Örnek: Azure arama için stok AdventureWorks veritabanı modeli

Modelleme yapılandırılmış veritabanı bir verimli search dizinine içerik nadiren basit bir uygulamadır. Zamanlama ve değişiklik Yönetimi CPU'nun, arama dostu varlıklara kaynak satırları tablo katılmış durumlarına uzağa normal durumdan çıkarmayı zorluklarıyla vardır. Bu makalede kullanılabilir AdventureWorks örnek verileri aramak için veritabanı geçiş ortak deneyimlerinde vurgulamak için çevrimiçi olarak kullanır. 

## <a name="about-adventureworks"></a>AdventureWorks hakkında

Bir SQL Server örneğiniz varsa, AdventureWorks örnek veritabanı ile aşina olabilirsiniz. Ürün bilgileri açığa beş tablo bu veritabanında bulunan tabloları arasındadır.

+ **ProductModel**: adı
+ **Ürün**: ad, renk, maliyet, boyut, ağırlık, görüntü, kategori (her satır için belirli bir ProductModel birleştirir)
+ **ProductDescription**: Açıklama
+ **ProductModelProductDescription**: yerel (her satır birleştiren bir ProductModel belirli bir ProductDescription belirli bir dil için)
+ **ProductCategory**: ad, üst kategori

Tüm bu verileri bir arama dizini alınan düzleştirilmiş satır içine birleştirme elinizdeki var. 

## <a name="considering-our-options"></a>Seçeneklerimiz de göz önünde bulundurur

Naïve yaklaşım (uygun yerlerde katılmış) ürün tablosundan tüm satırları beri Product tablosunda dizin için en belirli bilgilere sahip olacaktır. Ancak, bu yaklaşım bir sonuç kümesi içinde algılanan çoğaltmaları arama dizinine kullanıma. Örneğin, yol-650 modeli iki renkleri ve altı boyutlarını içinde kullanılabilir. "Yol bisiklet" için bir sorgu daha sonra aynı modelin yalnızca boyut ve renk tarafından ayrıştırılan on iki örnekleri tarafından direncin hakim olduğu. Diğer altı seyahat özgü modeller tüm arama nether'i dünyasına sahip: iki sayfa.

  ![Ürün listesini](./media/search-example-adventureworks/products-list.png "ürünler listesi")
 
Yol-650 modeli on iki seçenek olduğuna dikkat edin. Bire çok varlık satır en çok değer alanları veya öncesi aggregated değeri alanları arama dizini olarak temsil edilmektedir.

Bu sorunu çözme konusunda hedef dizin ProductModel tabloya hareket ettirmek kadar basit değil. Bunun yapılması, arama sonuçlarında gösterilen hala Product tablosunda önemli verileri yoksayın.

## <a name="use-a-collection-data-type"></a>Bir koleksiyon veri türü kullanın

Doğrudan bir paralel veritabanı modele sahip olmayan bir arama şeması özelliğini kullanmak için "doğru yaklaşım" şöyledir: **Collection(EDM.String)**. Bir koleksiyon veri türü bağımsız değişkenlerle yerine çok uzun bir listeniz kullanıldığında (tek) dizesi. Etiketleri veya anahtar sözcükler varsa, bu alan için bir koleksiyon veri türü kullanmanız gerekir.

Birden çok değerli dizin alanları tanımlayarak **Collection(Edm.String)** "renk", "boyutu" ve "image" bulunabilecek yinelenen girişler diziniyle kirletmesini olmadan tutulan için model oluşturma ve filtreleme bilgilerdir. Benzer şekilde, dizin oluşturma sayısal ürün alanları için toplama işlevleri uygulamak **minListPrice** her tek ürün yerine **listPrice**.

Bu yapılar ile dizin verildiğinde, "dağ bisikleti" için arama rengini, boyutunu ve en düşük fiyat gibi önemli meta verileri korurken ayrık bisiklet modelleri gösterebilir. Aşağıdaki ekran görüntüsünde, bir gösterim sağlar.

  ![Sıradağlar bisiklet araması örneği](./media/search-example-adventureworks/mountain-bikes-visual.png "Sıradağlar bisiklet arama örneği")

## <a name="use-script-for-data-manipulation"></a>Veri işleme için komut dosyası kullan

Ne yazık ki, bu tür bir modelleme tek başına SQL deyimleri kolayca elde edilemeyecek. Bunun yerine verileri yüklemek ve ardından arama dostu JSON varlıklara eşleme için basit bir NodeJS komut dosyası kullanın.

Son veritabanı arama eşlemesi şöyle görünür:

+ Model (Edm.String: aranabilir, filtrelenebilir, alınabilir) "ProductModel.Name" ndan
+ description_en (Edm.String: aranabilir) modeli için "ProductDescription" den burada kültür = 'tr'
+ Renk (Collection(Edm.String): aranabilir, filtrelenebilir, modellenebilir, alınabilir): "Product.Color" modeli için benzersiz değerler
+ Boyut (Collection(Edm.String): aranabilir, filtrelenebilir, modellenebilir, alınabilir): "Product.Size" modeli için benzersiz değerler
+ Görüntü (Collection(Edm.String): alınabilir): "Product.ThumbnailPhoto" modeli için benzersiz değerler
+ minStandardCost (Edm.Double: sıralanabilir şekilde alınabilir, filtrelenebilir, modellenebilir): modeli için tüm "Product.StandardCost" toplam değer en az
+ minListPrice (Edm.Double: sıralanabilir şekilde alınabilir, filtrelenebilir, modellenebilir): modeli için tüm "Product.ListPrice" toplam değer en az
+ minWeight (Edm.Double: sıralanabilir şekilde alınabilir, filtrelenebilir, modellenebilir): modeli için tüm "Product.Weight" toplam değer en az
+ Ürünler (Collection(Edm.String): aranabilir, filtrelenebilir, alınabilir): "Product.Name" modeli için benzersiz değerler

Ürün ve ProductDescription, kullanım ProductModel tabloyla katıldıktan sonra [lodash](https://lodash.com/) (veya LINQ C#) hızlı bir sonuç kümesi dönüştürmek için:

```javascript
var records = queryYourDatabase();
var models = _(records)
  .groupBy('ModelName')
  .values()
  .map(function(d) {
    return {
      model: _.first(d).ModelName,
      description: _.first(d).Description,
      colors: _(d).pluck('Color').uniq().compact().value(),
      products: _(d).pluck('ProductName').uniq().compact().value(),
      sizes: _(d).pluck('Size').uniq().compact().value(),
      images: _(d).pluck('ThumbnailPhotoFilename').uniq().compact().value(),
      minStandardCost: _(d).pluck('StandardCost').min(),
      maxStandardCost: _(d).pluck('StandardCost').max(),
      minListPrice: _(d).pluck('ListPrice').min(),
      maxListPrice: _(d).pluck('ListPrice').max(),
      minWeight: _(d).pluck('Weight').min(),
      maxWeight: _(d).pluck('Weight').max(),
    };
  })
  .value();
```

Sonuçta elde edilen JSON şöyle görünür:

```json
[
  {
    "model": "HL Road Frame",
    "colors": [
      "Black",
      "Red"
    ],
    "products": [
      "HL Road Frame - Black, 58",
      "HL Road Frame - Red, 58",
      "HL Road Frame - Red, 62",
      "HL Road Frame - Red, 44",
      "HL Road Frame - Red, 48",
      "HL Road Frame - Red, 52",
      "HL Road Frame - Red, 56",
      "HL Road Frame - Black, 62",
      "HL Road Frame - Black, 44",
      "HL Road Frame - Black, 48",
      "HL Road Frame - Black, 52"
    ],
    "sizes": [
      "58",
      "62",
      "44",
      "48",
      "52",
      "56"
    ],
    "images": [
      "no_image_available_small.gif"
    ],
    "minStandardCost": 868.6342,
    "maxStandardCost": 1059.31,
    "minListPrice": 1431.5,
    "maxListPrice": 1431.5,
    "minWeight": 961.61,
    "maxWeight": 1043.26
  }
]
```

Son olarak, ilk kayıt kümesini döndürmek için SQL sorgu aşağıdadır. Kullandım [mssql](https://www.npmjs.com/package/mssql) NodeJS uygulamamı verileri yüklemek için npm modülünü.

```T-SQL
SELECT
  m.Name as ModelName,
  d.Description,
  p.Name as ProductName,
  p.*
FROM 
  SalesLT.ProductModel m
INNER JOIN 
  SalesLT.ProductModelProductDescription md
  ON m.ProductModelId = md.ProductModelId
INNER JOIN 
  SalesLT.ProductDescription d
  ON md.ProductDescriptionId = d.ProductDescriptionId
LEFT JOIN 
  SalesLT.product p
  ON m.ProductModelId = p.ProductModelId
WHERE
  md.Culture='en'
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Örnek: Azure Arama'daki çok düzeyli model sınıflandırmaları](search-example-adventureworks-multilevel-faceting.md)


