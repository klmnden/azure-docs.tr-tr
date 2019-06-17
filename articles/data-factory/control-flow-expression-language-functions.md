---
title: İfade ve işlevleri Azure Data factory'de | Microsoft Docs
description: Bu makalede, ifadeler ve data factory varlıklarını oluştururken kullanabileceğiniz işlevler hakkında bilgi sağlar.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
ms.openlocfilehash: 4c51974498539a0305312d6501bcfa9ebc3b2e88
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64573557"
---
# <a name="expressions-and-functions-in-azure-data-factory"></a>Azure Data Factory’deki ifadeler ve işlevler
> [!div class="op_single_selector" title1="Data Factory hizmetinin kullandığınız sürümü seçin:"]
> * [Sürüm 1](v1/data-factory-functions-variables.md)
> * [Geçerli sürüm](control-flow-expression-language-functions.md)

Bu makalede, ifadeler ve Azure Data Factory tarafından desteklenen işlevler hakkında ayrıntılar sağlar. 

## <a name="introduction"></a>Giriş
Tanımındaki JSON değerlerinin değişmez değeri ya da çalışma zamanında değerlendirilen bir ifade olabilir. Örneğin:  
  
```json
"name": "value"
```

 (veya)  
  
```json
"name": "@pipeline().parameters.password"
```

## <a name="expressions"></a>İfadeler  
İfadeler, herhangi bir yerde bir JSON dizesi değerinin görünür ve her zaman başka bir JSON değeri neden. Bir JSON değeri bir ifade ise, deyim gövdesi oturum sırasında kaldırarak ayıklanan (\@). Bir sabit dizesi ile başlayan gerekirse \@, kullanılarak atlanması gereken \@ \@. Aşağıdaki örnekler, ifadelerin nasıl değerlendirilir gösterir.  
  
|JSON değeri|Sonuç|  
|----------------|------------|  
|"parametre"|'Parameters' karakterleri döndürülür.|  
|"parametreleri [1]"|'Parameters [1]' karakterlerini döndürülür.|  
|"\@\@"|1 karakter içeren dize '\@' döndürülür.|  
|" \@"|İçeren bir 2 karakter dizesi ' \@' döndürülür.|  
  
 İfadeleri de yer alabilir dizeler bir özelliği kullanılarak çağrılır *dize ilişkilendirme* nerede ifadeleri sarılır içinde `@{ ... }`. Örneğin, `"name" : "First Name: @{pipeline().parameters.firstName} Last Name: @{pipeline().parameters.lastName}"`  
  
 Dize ilişkilendirme kullanarak sonucu her zaman bir dizedir. Ben tanımladığınızdan söyleyin `myNumber` olarak `42` ve `myString` olarak `foo`:  
  
|JSON değeri|Sonuç|  
|----------------|------------|  
|"\@().parameters.myString işlem hattı"| Döndürür `foo` dize olarak.|  
|"\@{().parameters.myString işlem hattı}"| Döndürür `foo` dize olarak.|  
|"\@().parameters.myNumber işlem hattı"| Döndürür `42` olarak bir *numarası*.|  
|"\@{().parameters.myNumber işlem hattı}"| Döndürür `42` olarak bir *dize*.|  
|"Yanıt: @{().parameters.myNumber işlem hattı}"| Dizeyi döndürür `Answer is: 42`.|  
|"\@concat (' yanıt: ', string(pipeline().parameters.myNumber))"| Bir dize döndürür `Answer is: 42`|  
|"Yanıt: \@ \@{().parameters.myNumber işlem hattı}"| Dizeyi döndürür `Answer is: @{pipeline().parameters.myNumber}`.|  
  
### <a name="examples"></a>Örnekler

#### <a name="a-dataset-with-a-parameter"></a>Bir parametre ile bir veri kümesi
Aşağıdaki örnekte, BlobDataset adlı bir parametre alan **yolu**. Değeri için bir değer ayarlamak için kullanılan **folderPath** ifade kullanarak özellik: `dataset().path`. 

```json
{
    "name": "BlobDataset",
    "properties": {
        "type": "AzureBlob",
        "typeProperties": {
            "folderPath": "@dataset().path"
        },
        "linkedServiceName": {
            "referenceName": "AzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "parameters": {
            "path": {
                "type": "String"
            }
        }
    }
}
```

#### <a name="a-pipeline-with-a-parameter"></a>Bir parametre ile bir işlem hattı
Aşağıdaki örnekte, işlem hattı alır **inputPath** ve **outputPath** parametreleri. **Yolu** için parametreli blob veri kümesi bu parametrelerin değerlerini kullanarak ayarlayın. Burada kullanılan söz dizimi şu şekildedir: `pipeline().parameters.parametername`. 

```json
{
    "name": "Adfv2QuickStartPipeline",
    "properties": {
        "activities": [
            {
                "name": "CopyFromBlobToBlob",
                "type": "Copy",
                "inputs": [
                    {
                        "referenceName": "BlobDataset",
                        "parameters": {
                            "path": "@pipeline().parameters.inputPath"
                        },
                        "type": "DatasetReference"
                    }
                ],
                "outputs": [
                    {
                        "referenceName": "BlobDataset",
                        "parameters": {
                            "path": "@pipeline().parameters.outputPath"
                        },
                        "type": "DatasetReference"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                }
            }
        ],
        "parameters": {
            "inputPath": {
                "type": "String"
            },
            "outputPath": {
                "type": "String"
            }
        }
    }
}
```
  
## <a name="functions"></a>İşlevler  
 İfadeler içinde işlevleri çağırabilir. Aşağıdaki bölümlerde, bir ifadede kullanılabilen işlevleri hakkında bilgi sağlar.  

## <a name="string-functions"></a>Dize işlevleri  
 Aşağıdaki işlevleri yalnızca, dizeleri için de geçerlidir. Dizeleri bir dizi toplama işlevleri de kullanabilirsiniz.  
  
|İşlev adı|Açıklama|  
|-------------------|-----------------|  
|concat|Herhangi bir sayıda dizeyi birleştirir. Örneğin, parametre1 ise `foo,` aşağıdaki ifade döndürür `somevalue-foo-somevalue`:  `concat('somevalue-',pipeline().parameters.parameter1,'-somevalue')`<br /><br /> **Parametre numarası**: 1 ... *n*<br /><br /> **Ad**: Dize *n*<br /><br /> **Açıklama**: Gereklidir. Tek bir dize olarak birleştirilecek dize.|  
|alt dize|Bir dizedeki karakterlerin bir alt kümesini döndürür. Örneğin, aşağıdaki ifade:<br /><br /> `substring('somevalue-foo-somevalue',10,3)`<br /><br /> döndürür:<br /><br /> `foo`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: String<br /><br /> **Açıklama**: Gereklidir. Alt dizenin alındığı dize.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Başlangıç dizini<br /><br /> **Açıklama**: Gereklidir. Parametre 1'de alt dizenin başladığı dizin.<br /><br /> **Parametre numarası**: 3<br /><br /> **Ad**: Uzunluk<br /><br /> **Açıklama**: Gereklidir. Alt dizenin uzunluğu.|  
|Değiştir|Bir dizeyi belirli bir dize ile değiştirir. Örneğin, ifade:<br /><br /> `replace('the old string', 'old', 'new')`<br /><br /> döndürür:<br /><br /> `the new string`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: dize<br /><br /> **Açıklama**: Gereklidir.  Parametre 2 parametresi 1, 2 parametre için Aranan ve parametre 3 ile güncelleştirilen dize bulunursa.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Eski dize<br /><br /> **Açıklama**: Gereklidir. Parametre 1'de bir eşleşme bulunduğunda parametre 3 ile değiştirilecek dize<br /><br /> **Parametre numarası**: 3<br /><br /> **Ad**: Yeni dize<br /><br /> **Açıklama**: Gereklidir. Parametre 1'de bir eşleşme bulunduğunda parametre 2 değiştirmek için kullanılan dize.|  
|GUID| (Diğer adıyla. genel olarak benzersiz bir dize oluşturur GUID). Örneğin, aşağıdaki çıktıyı üretilemedi `c2ecc88d-88c8-4096-912c-d6f2e2b138ce`:<br /><br /> `guid()`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: Biçimi<br /><br /> **Açıklama**: İsteğe bağlı. Bildiren bir tek biçim belirticisi [bu GUID değerini biçimlendirmek nasıl](https://msdn.microsoft.com/library/97af8hh4%28v=vs.110%29.aspx). "N", "D", "B", "P" veya "X" biçim parametresini olabilir. "D" biçim sağlanmazsa kullanılır.|  
|toLower|Bir dizeyi küçük harfe dönüştürür. Örneğin, aşağıdaki döndürür `two by two is four`:  `toLower('Two by Two is Four')`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: String<br /><br /> **Açıklama**: Gereklidir. Daha düşük büyük/küçük harf için dönüştürülecek dize. Dizesindeki bir karakter, bir küçük harf eşdeğeri yoksa döndürülen dizeye değiştirilmeden dahil edilmiştir.|  
|toUpper|Bir dizeyi büyük harfe dönüştürür. Örneğin, aşağıdaki ifade döndürür `TWO BY TWO IS FOUR`:  `toUpper('Two by Two is Four')`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: String<br /><br /> **Açıklama**: Gereklidir. Üst büyük/küçük harf için dönüştürülecek dize. Dizesindeki bir karakter, bir büyük harf eşdeğeri yoksa, bu döndürülen dizeye değiştirilmeden eklenir.|  
|indexOf|Dizin bir dize vaka içindeki bir değerin insensitively bulun. Örneğin, aşağıdaki ifade döndürür `7`: `indexof('hello, world.', 'world')`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: String<br /><br /> **Açıklama**: Gereklidir. Değeri içeriyor olabilecek dize.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: String<br /><br /> **Açıklama**: Gereklidir. Dizini aranacak değer.|  
|lastIndexOf|Bir dize vaka içindeki bir değerin son dizinini insensitively bulun. Örneğin, aşağıdaki ifade döndürür `3`: `lastindexof('foofoo', 'foo')`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: String<br /><br /> **Açıklama**: Gereklidir. Değeri içeriyor olabilecek dize.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: String<br /><br /> **Açıklama**: Gereklidir. Dizini aranacak değer.|  
|startswith|Dize değeri çalışmasıyla insensitively başlayıp başlamadığını denetler. Örneğin, aşağıdaki ifade döndürür `true`: `startswith('hello, world', 'hello')`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: String<br /><br /> **Açıklama**: Gereklidir. Değeri içeriyor olabilecek dize.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: String<br /><br /> **Açıklama**: Gereklidir. Dize değeri ile başlatılabilir.|  
|endswith|Dize değeri çalışmasıyla insensitively bitip bitmediğini denetler. Örneğin, aşağıdaki ifade döndürür `true`: `endswith('hello, world', 'world')`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: String<br /><br /> **Açıklama**: Gereklidir. Değeri içeriyor olabilecek dize.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: String<br /><br /> **Açıklama**: Gereklidir. Dize değeri ile sonlandırabiliriz.|  
|split|Ayırıcı kullanarak dizeyi böler. Örneğin, aşağıdaki ifade döndürür `["a", "b", "c"]`: `split('a;b;c',';')`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: String<br /><br /> **Açıklama**: Gereklidir. Bölünen dize.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: String<br /><br /> **Açıklama**: Gereklidir. Ayırıcı.|  
  
  
## <a name="collection-functions"></a>Toplama işlevleri  
 Bu işlevler, diziler, dizeleri ve bazen sözlükleri gibi koleksiyonlar üzerinde çalışır.  
  
|İşlev adı|Açıklama|  
|-------------------|-----------------|  
|içerir|Sözlük anahtarı, bir listesi içeriyorsa true döndürür değeri veya dize alt dizeyi içeren. Örneğin, aşağıdaki ifade döndürür. `true:``contains('abacaba','aca')`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: Koleksiyonu içinde<br /><br /> **Açıklama**: Gereklidir. İçinde arama yapılacak koleksiyon.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Nesne bulma<br /><br /> **Açıklama**: Gereklidir. İçinde bulunacak nesne **koleksiyonundaki**.|  
|length|Bir dizi veya dizedeki öğelerin sayısını döndürür. Örneğin, aşağıdaki ifade döndürür `3`:  `length('abc')`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: Koleksiyon<br /><br /> **Açıklama**: Gereklidir. Uzunluğu alma koleksiyonu.|  
|boş|Nesne, dizi veya dize boşsa true döndürür. Örneğin, aşağıdaki ifade döndürür `true`:<br /><br /> `empty('')`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: Koleksiyon<br /><br /> **Açıklama**: Gereklidir. Boş olup olmadığı denetlenecek koleksiyon.|  
|kesişimi|Tek bir dizi veya nesne ortak öğeleri olan diziler veya geçirilen nesneler arasında döndürür. Örneğin, bu işlevi döndürür `[1, 2]`:<br /><br /> `intersection([1, 2, 3], [101, 2, 1, 10],[6, 8, 1, 2])`<br /><br /> İşlev parametrelerini ya da bir nesne veya dizi (ikisinin karışımı olamaz) kümesi olabilir. Aynı ada sahip iki nesne varsa, son nesnede bu ada sahip son nesne görünür.<br /><br /> **Parametre numarası**: 1 ... *n*<br /><br /> **Ad**: Koleksiyon *n*<br /><br /> **Açıklama**: Gereklidir. Değerlendirilecek koleksiyonlar. Bir nesne, sonuçta görüntülenmek üzere geçirilen tüm koleksiyonlarda olmalıdır.|  
|birleşim|Tek bir dizi veya nesne, dizi veya nesne kendisine geçirilen tüm öğeleri döndürür. Örneğin, bu işlev döndürür. `[1, 2, 3, 10, 101]:`<br /><br /> :  `union([1, 2, 3], [101, 2, 1, 10])`<br /><br /> İşlev parametrelerini ya da bir nesne veya dizi (ikisinin karışımı olamaz) kümesi olabilir. Son Çıkışta aynı ada sahip iki nesne varsa, son nesnede bu ada sahip son nesne görünür.<br /><br /> **Parametre numarası**: 1 ... *n*<br /><br /> **Ad**: Koleksiyon *n*<br /><br /> **Açıklama**: Gereklidir. Değerlendirilecek koleksiyonlar. Koleksiyonları hiçbirinde görünen bir nesne sonuçta da görüntülenir.|  
|ilk|Dizi veya dize geçirilen ilk öğeyi döndürür. Örneğin, bu işlevi döndürür `0`:<br /><br /> `first([0,2,3])`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: Koleksiyon<br /><br /> **Açıklama**: Gereklidir. İlk nesnenin alınacağı koleksiyon.|  
|Son|Dizi veya dize geçirilen son öğeyi döndürür. Örneğin, bu işlevi döndürür `3`:<br /><br /> `last('0123')`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: Koleksiyon<br /><br /> **Açıklama**: Gereklidir. Son nesnenin alınacağı koleksiyon.|  
|sınav zamanı|İlk döndürür **sayısı** geçirilen dizi veya dizedeki öğeleri, bu işlev, örneğin döndürür `[1, 2]`:  `take([1, 2, 3, 4], 2)`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: Koleksiyon<br /><br /> **Açıklama**: Gereklidir. İlk alınacağı koleksiyon **sayısı** nesneleri.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Count<br /><br /> **Açıklama**: Gereklidir. Gelen alınacak nesne sayısı **koleksiyon**. Pozitif bir tamsayı olmalıdır.|  
|Atla|Öğeleri dizi dizininden başlayarak döndürür **sayısı**, bu işlev, örneğin döndürür `[3, 4]`:<br /><br /> `skip([1, 2 ,3 ,4], 2)`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: Koleksiyon<br /><br /> **Açıklama**: Gereklidir. İlk atlanacağı koleksiyon **sayısı** nesneleri.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Count<br /><br /> **Açıklama**: Gereklidir. Önünden kaldırılacak nesne sayısı **koleksiyon**. Pozitif bir tamsayı olmalıdır.|  
  
## <a name="logical-functions"></a>Mantıksal işlevler  
 Bu işlevler içinde koşullar yararlıdır, herhangi bir türde mantıksal değerlendirmek için kullanılabilir.  
  
|İşlev adı|Açıklama|  
|-------------------|-----------------|  
|eşittir|İki değer eşitse true döndürür. Örneğin, parametre1'foo olduğunda, aşağıdaki ifade döndürür `true`: `equals(pipeline().parameters.parameter1), 'foo')`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: nesnesi 1<br /><br /> **Açıklama**: Gereklidir. Karşılaştırma yapılacak nesne **nesne 2**.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: 2 nesnesi<br /><br /> **Açıklama**: Gereklidir. Karşılaştırma yapılacak nesne **nesne 1**.|  
|daha az|İlk bağımsız değişken ikinciden küçükse true döndürür ikinciden. Not: değerleri yalnızca tamsayı, kayan noktalı sayı veya dize olabilir. Örneğin, aşağıdaki ifade döndürür `true`:  `less(10,100)`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: nesnesi 1<br /><br /> **Açıklama**: Gereklidir. Olup olmadığı denetlenecek nesne küçüktür **nesne 2**.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: 2 nesnesi<br /><br /> **Açıklama**: Gereklidir. Daha büyük olup olmadığı denetlenecek nesne **nesne 1**.|  
|lessOrEquals|İlk bağımsız değişken küçük veya ikinciye eşitse true döndürür. Not: değerleri yalnızca tamsayı, kayan noktalı sayı veya dize olabilir. Örneğin, aşağıdaki ifade döndürür `true`:  `lessOrEquals(10,10)`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: nesnesi 1<br /><br /> **Açıklama**: Gereklidir. Küçük olup olmadığı kontrol veya ona eşit nesne **nesne 2**.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: 2 nesnesi<br /><br /> **Açıklama**: Gereklidir. Büyük veya ona eşit olup olmadığı denetlenecek nesne **nesne 1**.|  
|daha büyük|İlk bağımsız değişken ikinciden büyükse true döndürür. Not: değerleri yalnızca tamsayı, kayan noktalı sayı veya dize olabilir. Örneğin, aşağıdaki ifade döndürür `false`:  `greater(10,10)`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: nesnesi 1<br /><br /> **Açıklama**: Gereklidir. Daha büyük olup olmadığı denetlenecek nesne **nesne 2**.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: 2 nesnesi<br /><br /> **Açıklama**: Gereklidir. Olup olmadığı denetlenecek nesne küçüktür **nesne 1**.|  
|greaterOrEquals|İlk bağımsız değişken büyük veya ikinciye eşitse true döndürür. Not: değerleri yalnızca tamsayı, kayan noktalı sayı veya dize olabilir. Örneğin, aşağıdaki ifade döndürür `false`:  `greaterOrEquals(10,100)`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: nesnesi 1<br /><br /> **Açıklama**: Gereklidir. Büyük veya ona eşit olup olmadığı denetlenecek nesne **nesne 2**.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: 2 nesnesi<br /><br /> **Açıklama**: Gereklidir. Küçük veya buna eşit olup olmadığı denetlenecek nesne **nesne 1**.|  
|ve|Parametrelerinin her ikisi de doğruysa true döndürür. Her iki değişken Boolean olması gerekir. Aşağıdaki döndürür `false`:  `and(greater(1,10),equals(0,0))`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: Boole 1<br /><br /> **Açıklama**: Gereklidir. Gereken ilk bağımsız değişken `true`.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Boole 2<br /><br /> **Açıklama**: Gereklidir. İkinci bağımsız değişkeni olmalıdır `true`.|  
|or|Parametrelerden biri doğru olduğunda true döndürür. Her iki değişken Boolean olması gerekir. Aşağıdaki döndürür `true`:  `or(greater(1,10),equals(0,0))`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: Boole 1<br /><br /> **Açıklama**: Gereklidir. İlk bağımsız değişken olabilir `true`.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Boole 2<br /><br /> **Açıklama**: Gereklidir. İkinci bağımsız değişken olabilir `true`.|  
|değil|Parametre ise true değeri döndürür `false`. Her iki değişken Boolean olması gerekir. Aşağıdaki döndürür `true`:  `not(contains('200 Success','Fail'))`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: Boolean<br /><br /> **Açıklama**: Parametre ise true değeri döndürür `false`. Her iki değişken Boolean olması gerekir. Aşağıdaki döndürür `true`:  `not(contains('200 Success','Fail'))`|  
|if|Belirtilen bir değeri temel halinde sağlanan ifade sonuçları döndürür `true` veya `false`.  Örneğin, aşağıdaki döndürür `"yes"`: `if(equals(1, 1), 'yes', 'no')`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: İfade<br /><br /> **Açıklama**: Gereklidir. İfade tarafından hangi değeri belirleyen bir Boole değeri döndürülür.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: True<br /><br /> **Açıklama**: Gereklidir. İfadenin olması durumunda döndürülecek değer `true`.<br /><br /> **Parametre numarası**: 3<br /><br /> **Ad**: False<br /><br /> **Açıklama**: Gereklidir. İfadenin olması durumunda döndürülecek değer `false`.|  
  
## <a name="conversion-functions"></a>Dönüştürme işlevleri  
 Bu işlevlerin her dilde yerel türler arasında dönüştürmek için kullanılır:  
  
-   string  
  
-   integer  
  
-   float  
  
-   boole  
  
-   Diziler  
  
-   sözlükler  
  
|İşlev adı|Açıklama|  
|-------------------|-----------------|  
|int|Parametreyi bir tamsayıya dönüştürür. Örneğin, aşağıdaki ifade bir dize yerine bir sayı olarak 100 değerini döndürür:  `int('100')`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: Değer<br /><br /> **Açıklama**: Gereklidir. Tamsayıya dönüştürülen değer.|  
|string|Parametreyi bir dizeye dönüştürür. Örneğin, aşağıdaki ifade döndürür `'10'`:  `string(10)` Ayrıca bir nesne örneği için bir dize durumunda dönüştürebilirsiniz **foo** parametresi, bir özelliği olan bir nesne `bar : baz`, sonra da aşağıdaki döndürür `{"bar" : "baz"}` `string(pipeline().parameters.foo)`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: Değer<br /><br /> **Açıklama**: Gereklidir. Bir dizeye dönüştürülen değer.|  
|json|Parametreyi bir JSON türü değere dönüştürür. String() tersidir. Örneğin, aşağıdaki ifade döndürür `[1,2,3]` bir dize yerine bir dizi olarak:<br /><br /> `json('[1,2,3]')`<br /><br /> Benzer şekilde, bir dize bir nesneye dönüştürebilir. Örneğin, `json('{"bar" : "baz"}')` döndürür:<br /><br /> `{ "bar" : "baz" }`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: String<br /><br /> **Açıklama**: Gereklidir. Bir yerel tür değerine dönüştürülen değer bir dize.<br /><br /> Xml giriş json işlevi destekler. Örneğin, parametre değeri:<br /><br /> `<?xml version="1.0"?> <root>   <person id='1'>     <name>Alan</name>     <occupation>Engineer</occupation>   </person> </root>`<br /><br /> Aşağıdaki json dönüştürülür:<br /><br /> `{ "?xml": { "@version": "1.0" },   "root": {     "person": [     {       "@id": "1",       "name": "Alan",       "occupation": "Engineer"     }   ]   } }`|  
|float|Parametre bağımsız değişkenini bir kayan noktalı sayıya dönüştürün. Örneğin, aşağıdaki ifade döndürür `10.333`:  `float('10.333')`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: Değer<br /><br /> **Açıklama**: Gereklidir. Kayan noktalı sayıya dönüştürülen değer.|  
|bool|Parametreyi bir Boole değerine dönüştürür. Örneğin, aşağıdaki ifade döndürür `false`:  `bool(0)`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: Değer<br /><br /> **Açıklama**: Gereklidir. Değer bir boolean değerine dönüştürülür.|  
|birleşim|Geçirilen bağımsız değişkenlerdeki null olmayan ilk nesneyi döndürür. Not: boş bir dize null değil. Bu örneğin, 1 ve 2 parametreleri tanımlanmamışsa, döndürür `fallback`:  `coalesce(pipeline().parameters.parameter1', pipeline().parameters.parameter2 ,'fallback')`<br /><br /> **Parametre numarası**: 1 ... *n*<br /><br /> **Ad**: Nesne*n*<br /><br /> **Açıklama**: Gereklidir. Denetlenecek nesneleri `null`.|  
|Base64|Giriş dizesinin base64 gösterimini döndürür. Örneğin, aşağıdaki ifade döndürür `c29tZSBzdHJpbmc=`:  `base64('some string')`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: Dize 1<br /><br /> **Açıklama**: Gereklidir. Base64 gösterimine kodlanacak dize.|  
|base64ToBinary|Bir base64 ile kodlanmış dize ikili gösterimini döndürür. Örneğin, aşağıdaki ifade bazı dizenin ikili gösterimini döndürür: `base64ToBinary('c29tZSBzdHJpbmc=')`.<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: String<br /><br /> **Açıklama**: Gereklidir. Base64 kodlamalı dize.|  
|base64ToString|Based64 kodlanmış bir dizenin dize gösterimini döndürür. Örneğin, aşağıdaki ifade bazı dize döndürür: `base64ToString('c29tZSBzdHJpbmc=')`.<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: String<br /><br /> **Açıklama**: Gereklidir. Base64 kodlamalı dize.|  
|binary|Bir değerin ikili gösterimini döndürür.  Örneğin, aşağıdaki ifade, ikili bazı dize gösterimini döndürür: `binary(‘some string’).`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: Değer<br /><br /> **Açıklama**: Gereklidir. İkiliye dönüştürülen değer.|  
|dataUriToBinary|Bir veri URI'SİNİN ikili gösterimini döndürür. Örneğin, aşağıdaki ifade, bazı dizenin ikili gösterimini döndürür: `dataUriToBinary('data:;base64,c29tZSBzdHJpbmc=')`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: String<br /><br /> **Açıklama**: Gereklidir. İkili gösterime dönüştürülecek veri URI'si.|  
|dataUriToString|Bir veri URI'SİNİN dize gösterimini döndürür. Örneğin, aşağıdaki ifade, bazı dizesini döndürür: `dataUriToString('data:;base64,c29tZSBzdHJpbmc=')`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: String<br /><br />**Açıklama**: Gereklidir. Dize gösterimine dönüştürülecek veri URI'si.|  
|dataUri|Bir veri URI'SİNİN bir değer döndürür. Örneğin, aşağıdaki ifade veri döndürür: `text/plain;charset=utf8;base64,c29tZSBzdHJpbmc=: dataUri('some string')`<br /><br /> **Parametre numarası**: 1<br /><br />**Ad**: Değer<br /><br />**Açıklama**: Gereklidir. Veri URI'sine dönüştürülecek değer.|  
|decodeBase64|Bir giriş based64 dizenin dize gösterimini döndürür. Örneğin, aşağıdaki ifade döndürür `some string`:  `decodeBase64('c29tZSBzdHJpbmc=')`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: String<br /><br /> **Açıklama**: Bir giriş based64 dizenin dize gösterimini döndürür.|  
|Encodeurıcomponent|URL çıkışları geçirilen bir dize. Örneğin, aşağıdaki ifade döndürür `You+Are%3ACool%2FAwesome`:  `encodeUriComponent('You Are:Cool/Awesome')`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: String<br /><br /> **Açıklama**: Gereklidir. URL güvenli olmayan karakterleri kaçış için dize.|  
|Decodeurıcomponent|Kaldırma URL çıkışları geçirilen bir dize. Örneğin, aşağıdaki ifade döndürür `You Are:Cool/Awesome`:  `encodeUriComponent('You+Are%3ACool%2FAwesome')`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: String<br /><br /> **Açıklama**: Gereklidir. URL güvenli olmayan karakterleri kodu çözülecek dize.|  
|decodeDataUri|Bir giriş verisi URI dizesinin ikili gösterimini döndürür. Örneğin, aşağıdaki ifade, ikili gösterimini döndürür `some string`:  `decodeDataUri('data:;base64,c29tZSBzdHJpbmc=')`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: String<br /><br /> **Açıklama**: Gereklidir. Bir ikili gösterim kodu çözülecek dataURI.|  
|uriComponent|Döndürür bir URI ile kodlanmış bir değer gösterimini. Örneğin, aşağıdaki ifade döndürür. `You+Are%3ACool%2FAwesome: uriComponent('You Are:Cool/Awesome ')`<br /><br /> Parametre ayrıntıları: Sayı: 1, adı: Dize, Açıklama: Gereklidir. URI ile kodlanacak dize.|  
|uriComponentToBinary|Kodlamalı dize URI ikili gösterimini döndürür. Örneğin, aşağıdaki ifade, ikili gösterimini döndürür `You Are:Cool/Awesome`: `uriComponentToBinary('You+Are%3ACool%2FAwesome')`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: String<br /><br />**Açıklama**: Gereklidir. Kodlanmış URI dizesi.|  
|uriComponentToString|Bir dize gösterimini bir URI kodlamalı dize döndürür. Örneğin, aşağıdaki ifade döndürür `You Are:Cool/Awesome`: `uriComponentToString('You+Are%3ACool%2FAwesome')`<br /><br /> **Parametre numarası**: 1<br /><br />**Ad**: String<br /><br />**Açıklama**: Gereklidir. Kodlanmış URI dizesi.|  
|xml|Değeri bir xml temsilini döndürün. Örneğin, aşağıdaki ifade tarafından temsil edilen bir xml içeriğini döndürür `'\<name>Alan\</name>'`: `xml('\<name>Alan\</name>')`. JSON nesnesi de girişi xml işlevi destekler. Örneğin, parametre `{ "abc": "xyz" }` bir xml içeriği dönüştürülür `\<abc>xyz\</abc>`<br /><br /> **Parametre numarası**: 1<br /><br />**Ad**: Değer<br /><br />**Açıklama**: Gereklidir. XML'ye dönüştürülecek değer.|  
|XPath|Xml düğüm xpath ifadesi olarak değerlendirilen bir değerin xpath ifadesi ile eşleşen bir dizi döndürür.<br /><br />  **Örnek 1**<br /><br /> Aşağıdaki XML'i bir dize gösterimi 'p1' parametre değeri kabul edin:<br /><br /> `<?xml version="1.0"?> <lab>   <robot>     <parts>5</parts>     <name>R1</name>   </robot>   <robot>     <parts>8</parts>     <name>R2</name>   </robot> </lab>`<br /><br /> 1. Bu kod: `xpath(xml(pipeline().parameters.p1), '/lab/robot/name')`<br /><br /> döndürür<br /><br /> `[ <name>R1</name>, <name>R2</name> ]`<br /><br /> Oysa<br /><br /> 2. Bu kod: `xpath(xml(pipeline().parameters.p1, ' sum(/lab/robot/parts)')`<br /><br /> döndürür<br /><br /> `13`<br /><br /> <br /><br /> **Örnek 2**<br /><br /> Aşağıdaki XML içeriği verilen:<br /><br /> `<?xml version="1.0"?> <File xmlns="http://foo.com">   <Location>bar</Location> </File>`<br /><br /> 1.  Bu kod: `@xpath(xml(body('Http')), '/*[name()=\"File\"]/*[name()=\"Location\"]')`<br /><br /> or<br /><br /> 2. Bu kod: `@xpath(xml(body('Http')), '/*[local-name()=\"File\" and namespace-uri()=\"http://foo.com\"]/*[local-name()=\"Location\" and namespace-uri()=\"\"]')`<br /><br /> döndürür<br /><br /> `<Location xmlns="http://foo.com">bar</Location>`<br /><br /> ve<br /><br /> 3. Bu kod: `@xpath(xml(body('Http')), 'string(/*[name()=\"File\"]/*[name()=\"Location\"])')`<br /><br /> döndürür<br /><br /> ``bar``<br /><br /> **Parametre numarası**: 1<br /><br />**Ad**: Xml<br /><br />**Açıklama**: Gereklidir. XPath ifadesinin değerlendirileceği XML.<br /><br /> **Parametre numarası**: 2<br /><br />**Ad**: XPath<br /><br />**Açıklama**: Gereklidir. Değerlendirilecek XPath ifadesi.|  
|array|Parametreyi bir diziye dönüştürür.  Örneğin, aşağıdaki ifade döndürür `["abc"]`: `array('abc')`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: Değer<br /><br /> **Açıklama**: Gereklidir. Diziye dönüştürülen değer.|
|createArray|Parametrelerden bir dizi oluşturur.  Örneğin, aşağıdaki ifade döndürür `["a", "c"]`: `createArray('a', 'c')`<br /><br /> **Parametre numarası**: 1 ... n<br /><br /> **Ad**: Herhangi bir n<br /><br /> **Açıklama**: Gereklidir. Bir dizi olarak birleştirilecek değerler.|

## <a name="math-functions"></a>Matematik işlevleri  
 Bu işlevlerin her iki tür sayı için kullanılabilir: **tamsayılar** ve **gezinen**.  
  
|İşlev adı|Açıklama|  
|-------------------|-----------------|  
|add|Ayrıca iki sayının sonucunu döndürür. Örneğin, bu işlevi döndürür `20.333`:  `add(10,10.333)`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: Toplanan 1<br /><br /> **Açıklama**: Gereklidir. Eklenecek sayı **toplanan 2**.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Toplanan 2 '<br /><br /> **Açıklama**: Gereklidir. Eklenecek sayı **toplanan 1**.|  
|sub|İki sayının çıkarma işleminin sonucunu döndürür. Örneğin, bu işlevi döndürür: `-0.333`:<br /><br /> `sub(10,10.333)`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: Eksilen<br /><br /> **Açıklama**: Gereklidir. Sayı, **çıkarılan** kaldırılır.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: çıkarılan<br /><br /> **Açıklama**: Gereklidir. Kaldırmak üzere numarasını **Eksilen**.|  
|mul|İki sayının Çarpım sonucunu döndürür. Örneğin, aşağıdaki döndürür `103.33`:<br /><br /> `mul(10,10.333)`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: Çarpan 1<br /><br /> **Açıklama**: Gereklidir. Çarpılacağı sayı **çarpan 2** ile.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Çarpan 2<br /><br /> **Açıklama**: Gereklidir. Çarpılacağı sayı **çarpan 1** ile.|  
|div|Bölme işlemi iki sayının sonucunu döndürür. Örneğin, aşağıdaki döndürür `1.0333`:<br /><br /> `div(10.333,10)`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: Bölünen<br /><br /> **Açıklama**: Gereklidir. Bölecek sayı **bölen**.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: bölen<br /><br /> **Açıklama**: Gereklidir. Bölünecek sayı **bölünen** tarafından.|  
|mod|Sonra (mod) iki sayının bölümünün geri kalanı sonucu döndürür. Örneğin, aşağıdaki ifade döndürür `2`:<br /><br /> `mod(10,4)`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: Bölünen<br /><br /> **Açıklama**: Gereklidir. Bölecek sayı **bölen**.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: bölen<br /><br /> **Açıklama**: Gereklidir. Bölünecek sayı **bölünen** tarafından. Bölme işleminden sonra kalan alınır.|  
|dk|Bu işlevi çağırmak için iki farklı deseni vardır: `min([0,1,2])` Burada en az bir dizi alır. Bu ifade döndürür `0`. Alternatif olarak, bu işlev, değerleri virgülle ayrılmış bir listesini alabilir:  `min(0,1,2)` Bu işlev, aynı zamanda 0 döndürür. Unutmayın, parametresinin bir dizi ise yalnızca sayılar bulunması sahip olması tüm değerleri sayılar olmalıdır.<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: Koleksiyon veya değeri<br /><br /> **Açıklama**: Gereklidir. Ayrıca, en düşük değer veya bir kümenin ilk değeri bulunacak değerler dizisi ya da olabilir.<br /><br /> **Parametre numarası**: 2 ... *n*<br /><br /> **Ad**: Değer *n*<br /><br /> **Açıklama**: İsteğe bağlı. İlk parametre bir değer ise, ardından ek değerler geçirebilirsiniz ve geçirilen tüm değerlerin minimum döndürülür.|  
|en fazla|Bu işlevi çağırmak için iki farklı deseni vardır:  `max([0,1,2])`<br /><br /> Burada en fazla bir dizi alır. Bu ifade döndürür `2`. Alternatif olarak, bu işlev, değerleri virgülle ayrılmış bir listesini alabilir:  `max(0,1,2)` Bu işlev, 2 döndürür. Unutmayın, parametresinin bir dizi ise yalnızca sayılar bulunması sahip olması tüm değerleri sayılar olmalıdır.<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: Koleksiyon veya değeri<br /><br /> **Açıklama**: Gereklidir. Ayrıca, en yüksek değer veya bir kümenin ilk değeri bulunacak değerler dizisi ya da olabilir.<br /><br /> **Parametre numarası**: 2 ... *n*<br /><br /> **Ad**: Değer *n*<br /><br /> **Açıklama**: İsteğe bağlı. İlk parametre bir değer ise, ardından ek değerler geçirebilirsiniz ve geçirilen tüm değerlerin maksimum döndürülür.|  
|Aralığı| Belirli bir sayıdan başlayan bir tamsayı dizisi oluşturur ve döndürülen dizinin uzunluğu tanımlayın. Örneğin, bu işlevi döndürür `[3,4,5,6]`:<br /><br /> `range(3,4)`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: Başlangıç dizini<br /><br /> **Açıklama**: Gereklidir. Dizideki ilk tamsayıdır.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Count<br /><br /> **Açıklama**: Gereklidir. Dizideki tamsayıların sayısı.|  
|rand| (İki ucu da dahildir. belirtilen aralıkta rastgele bir tamsayı oluşturur Örneğin, döndürebilir `42`:<br /><br /> `rand(-1000,1000)`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: Minimum<br /><br /> **Açıklama**: Gereklidir. Döndürülebilen en küçük tamsayı.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Maksimum<br /><br /> **Açıklama**: Gereklidir. Döndürülebilecek en yüksek tamsayı.|  
  
## <a name="date-functions"></a>Tarih işlevleri  
  
|İşlev adı|Açıklama|  
|-------------------|-----------------|  
|UtcNow|Geçerli zaman damgasını dize olarak döndürür. Örneğin `2015-03-15T13:27:36Z`:<br /><br /> `utcnow()`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: Biçimi<br /><br /> **Açıklama**: İsteğe bağlı. Ya da bir [tek biçim tanımlayıcı karakteri](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) veya [özel biçim deseni](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) değeri bu zaman damgası, Biçimlendirilecek nasıl gösterir. Biçim sağlanmazsa ISO 8601 biçimi ("o") kullanılır.|  
|saniyeEkle|Geçirilen dize zaman damgasına tamsayı saniye ekler. Saniye sayısı pozitif veya negatif olabilir. Bir biçim belirtici sağlanmazsa ISO 8601 biçiminde ("o") varsayılan olarak, bir dize olarak sonucudur. Örneğin `2015-03-15T13:27:00Z`:<br /><br /> `addseconds('2015-03-15T13:27:36Z', -36)`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: Zaman damgası<br /><br /> **Açıklama**: Gereklidir. Saat içeren bir dize.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Saniye<br /><br /> **Açıklama**: Gereklidir. Eklenecek saniye sayısı. Saniye çıkarılması için negatif olabilir.<br /><br /> **Parametre numarası**: 3<br /><br /> **Ad**: Biçimi<br /><br /> **Açıklama**: İsteğe bağlı. Ya da bir [tek biçim tanımlayıcı karakteri](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) veya [özel biçim deseni](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) değeri bu zaman damgası, Biçimlendirilecek nasıl gösterir. Biçim sağlanmazsa ISO 8601 biçimi ("o") kullanılır.|  
|AddMinutes|Geçirilen dize zaman damgasına tamsayı dakika ekler. Dakika sayısı pozitif veya negatif olabilir. Bir biçim belirtici sağlanmazsa ISO 8601 biçiminde ("o") varsayılan olarak, bir dize olarak sonucudur. Örneğin, `2015-03-15T14:00:36Z`:<br /><br /> `addminutes('2015-03-15T13:27:36Z', 33)`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: Zaman damgası<br /><br /> **Açıklama**: Gereklidir. Saat içeren bir dize.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Dakika<br /><br /> **Açıklama**: Gereklidir. Eklenecek dakika sayısı. Dakika çıkarılması için negatif olabilir.<br /><br /> **Parametre numarası**: 3<br /><br /> **Ad**: Biçimi<br /><br /> **Açıklama**: İsteğe bağlı. Ya da bir [tek biçim tanımlayıcı karakteri](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) veya [özel biçim deseni](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) değeri bu zaman damgası, Biçimlendirilecek nasıl gösterir. Biçim sağlanmazsa ISO 8601 biçimi ("o") kullanılır.|  
|addhours|Geçirilen dize zaman damgasına tamsayı saat ekler. Saat sayısı pozitif veya negatif olabilir. Bir biçim belirtici sağlanmazsa ISO 8601 biçiminde ("o") varsayılan olarak, bir dize olarak sonucudur. Örneğin `2015-03-16T01:27:36Z`:<br /><br /> `addhours('2015-03-15T13:27:36Z', 12)`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: Zaman damgası<br /><br /> **Açıklama**: Gereklidir. Saat içeren bir dize.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Saat<br /><br /> **Açıklama**: Gereklidir. Eklenecek saat sayısı. Saat çıkarılması için negatif olabilir.<br /><br /> **Parametre numarası**: 3<br /><br /> **Ad**: Biçimi<br /><br /> **Açıklama**: İsteğe bağlı. Ya da bir [tek biçim tanımlayıcı karakteri](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) veya [özel biçim deseni](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) değeri bu zaman damgası, Biçimlendirilecek nasıl gösterir. Biçim sağlanmazsa ISO 8601 biçimi ("o") kullanılır.|  
|adddays|Geçirilen dize zaman damgasına tamsayı gün ekler. Gün sayısı pozitif veya negatif olabilir. Bir biçim belirtici sağlanmazsa ISO 8601 biçiminde ("o") varsayılan olarak, bir dize olarak sonucudur. Örneğin `2015-02-23T13:27:36Z`:<br /><br /> `adddays('2015-03-15T13:27:36Z', -20)`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: Zaman damgası<br /><br /> **Açıklama**: Gereklidir. Saat içeren bir dize.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Gün<br /><br /> **Açıklama**: Gereklidir. Eklenecek gün sayısı. Gün çıkarılması için negatif olabilir.<br /><br /> **Parametre numarası**: 3<br /><br /> **Ad**: Biçimi<br /><br /> **Açıklama**: İsteğe bağlı. Ya da bir [tek biçim tanımlayıcı karakteri](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) veya [özel biçim deseni](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) değeri bu zaman damgası, Biçimlendirilecek nasıl gösterir. Biçim sağlanmazsa ISO 8601 biçimi ("o") kullanılır.|  
|formatDateTime|Tarih biçiminde bir dize döndürür. Bir biçim belirtici sağlanmazsa ISO 8601 biçiminde ("o") varsayılan olarak, bir dize olarak sonucudur. Örneğin `2015-02-23T13:27:36Z`:<br /><br /> `formatDateTime('2015-03-15T13:27:36Z', 'o')`<br /><br /> **Parametre numarası**: 1<br /><br /> **Ad**: Tarih<br /><br /> **Açıklama**: Gereklidir. Tarih içeren bir dize.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Biçimi<br /><br /> **Açıklama**: İsteğe bağlı. Ya da bir [tek biçim tanımlayıcı karakteri](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) veya [özel biçim deseni](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) değeri bu zaman damgası, Biçimlendirilecek nasıl gösterir. Biçim sağlanmazsa ISO 8601 biçimi ("o") kullanılır.|  

## <a name="next-steps"></a>Sonraki adımlar
Sistem değişkenleri ifadelerinde kullanabileceğiniz bir listesi için bkz. [sistem değişkenlerini](control-flow-system-variables.md).
