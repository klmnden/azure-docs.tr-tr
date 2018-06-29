---
title: İfade ve Azure Data Factory işlevleri | Microsoft Docs
description: Bu makalede, ifadeler ve data factory varlıklarını oluşturmada kullanabileceğiniz işlevleri hakkında bilgi sağlar.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
ms.openlocfilehash: 81392cc8b6225302d6835cdb3d23e9bab7d9c930
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37059203"
---
# <a name="expressions-and-functions-in-azure-data-factory"></a>İfadeler ve Azure Data Factory işlevleri
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-functions-variables.md)
> * [Geçerli sürüm](control-flow-expression-language-functions.md)

Bu makalede, ifadeler ve Azure Data Factory ile desteklenen işlevler hakkında ayrıntılar sağlar. 

## <a name="introduction"></a>Giriş
JSON değerler tanımında sabit değer veya çalışma zamanında değerlendirilen bir ifade olabilir. Örneğin:  
  
```json
"name": "value"
```

 (veya)  
  
```json
"name": "@pipeline().parameters.password"
```

## <a name="expressions"></a>İfadeler  
İfadeler bir JSON dizesi değerindeki herhangi bir yerde görünür ve her zaman başka bir JSON değeri neden. Bir JSON değerini bir deyim ise ifadesinin gövdesi oturum sırasında kaldırarak ayıklanan (\@). Sabit değerli bir dize ile başlayan gerekliyse, @, @ kullanarak kaçış uygulanmalıdır@. Aşağıdaki örnekler, ifadeler nasıl değerlendirilir gösterir.  
  
|JSON değeri|Sonuç|  
|----------------|------------|  
|"parametreleri"|Karakter 'parameters' döndürülür.|  
|"parametreleri [1]"|Karakter 'parametreleri [1]' döndürülür.|  
|"\@@"|İçeren bir 1 karakter dizesi ' @' döndürülür.|  
|" \@"|İçeren bir 2 karakter dizesi ' @' döndürülür.|  
  
 İfadeler de görüntülenebilir dizeler bir özelliği kullanılarak çağrılır *dize ilişkilendirme* burada ifadeleri sarılır içinde `@{ ... }`. Örneğin, `"name" : "First Name: @{pipeline().parameters.firstName} Last Name: @{pipeline().parameters.lastName}"`  
  
 Dize ilişkilendirme kullanarak, sonuç her zaman bir dizedir. I tanımlı sahip söyleyin `myNumber` olarak `42` ve `myString` olarak `foo`:  
  
|JSON değeri|Sonuç|  
|----------------|------------|  
|"\@().parameters.myString kanal"| Döndürür `foo` dize olarak.|  
|"\@{().parameters.myString kanal}"| Döndürür `foo` dize olarak.|  
|"\@().parameters.myNumber kanal"| Döndürür `42` olarak bir *numarası*.|  
|"\@{().parameters.myNumber kanal}"| Döndürür `42` olarak bir *dize*.|  
|"Yanıt: @{().parameters.myNumber kanal}"| Bir dize döndürür `Answer is: 42`.|  
|"\@concat (' yanıt: ', string(pipeline().parameters.myNumber))"| Bir dize döndürür `Answer is: 42`|  
|"Yanıt: \@@{().parameters.myNumber kanal}"| Bir dize döndürür `Answer is: @{pipeline().parameters.myNumber}`.|  
  
### <a name="examples"></a>Örnekler

#### <a name="a-dataset-with-a-parameter"></a>Bir parametre içeren bir veri kümesi
Aşağıdaki örnekte, BlobDataset adlı bir parametre alan **yolu**. Değeri için bir değer ayarlamak için kullanılan **folderPath** aşağıdaki ifadeler kullanarak özellik: `@{dataset().path}`. 

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

#### <a name="a-pipeline-with-a-parameter"></a>Bir parametreye sahip işlem hattı
Aşağıdaki örnekte, ardışık düzen alır **InputPath** ve **outputPath** parametreleri. **Yolu** için parametreli blob dataset bu parametrelerinin değerleri kullanarak ayarlayın. Burada kullanılan sözdizimi şöyledir: `pipeline().parameters.parametername`. 

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
 İfadeler içinde işlevleri çağırabilir. Aşağıdaki bölümler bir ifadede kullanılan işlevler hakkında bilgi sağlar.  

## <a name="string-functions"></a>Dize işlevleri  
 Aşağıdaki işlevleri yalnızca dizeleri için geçerlidir. Dizeleri toplama işlevleri de kullanabilirsiniz.  
  
|İşlev adı|Açıklama|  
|-------------------|-----------------|  
|concat|Dizeleri herhangi bir sayıda birlikte birleştirir. Örneğin, parametre1 ise `foo,` ifadesini döndürecektir `somevalue-foo-somevalue`:  `concat('somevalue-',pipeline().parameters.parameter1,'-somevalue')`<br /><br /> **Numaralı parametre**: 1... *n*<br /><br /> **Ad**: dize *n*<br /><br /> **Açıklama**: gerekli. Tek bir dize halinde birleştirmek için dizeleri.|  
|substring|Bir dizeden karakterleri kümesini döndürür. Örneğin, aşağıdaki ifade:<br /><br /> `substring('somevalue-foo-somevalue',10,3)`<br /><br /> döndürür:<br /><br /> `foo`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: dize<br /><br /> **Açıklama**: gerekli. Alt dizeyi alındığı dizesi.<br /><br /> **Numaralı parametre**: 2<br /><br /> **Ad**: Başlangıç dizini<br /><br /> **Açıklama**: gerekli. Alt dizeyi parametresi 1 başladığı dizin.<br /><br /> **Numaralı parametre**: 3<br /><br /> **Ad**: uzunluğu<br /><br /> **Açıklama**: gerekli. Dizenin uzunluğu.|  
|Değiştir|Bir dizeyi belirli bir dizeyle değiştirir. Örneğin, ifade:<br /><br /> `replace('the old string', 'old', 'new')`<br /><br /> döndürür:<br /><br /> `the new string`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: dize<br /><br /> **Açıklama**: gerekli.  Parametre 2 parametresi 1, 2 parametresi için arama ve 3 parametreyle güncelleştirilen dize bulunursa.<br /><br /> **Numaralı parametre**: 2<br /><br /> **Ad**: eski dizesi<br /><br /> **Açıklama**: gerekli. Dize parametresi 1 bir eşleşme bulunduğunda 3 parametreyle değiştirmek için<br /><br /> **Numaralı parametre**: 3<br /><br /> **Ad**: yeni bir dize<br /><br /> **Açıklama**: gerekli. 1 parametresinde bir eşleşme bulunduğunda parametresi 2 dizesini değiştirmek için kullanılan dize.|  
|GUID| (Diğer adıyla. genel olarak benzersiz bir dize oluşturur GUID). Örneğin, aşağıdaki çıkış üretilemedi `c2ecc88d-88c8-4096-912c-d6f2e2b138ce`:<br /><br /> `guid()`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: biçimi<br /><br /> **Açıklama**: isteğe bağlı. Gösteren bir tek biçim belirticisi [bu GUID değeri biçimine](https://msdn.microsoft.com/library/97af8hh4%28v=vs.110%29.aspx). Format parametresi "N", "D", "B", "P" veya "X" olabilir. Biçim sağlanmazsa, "D" kullanılır.|  
|toLower|Bir dizeyi küçük harflere dönüştürür. Örneğin, aşağıdaki döndürür `two by two is four`:  `toLower('Two by Two is Four')`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: dize<br /><br /> **Açıklama**: gerekli. Alt kasasını dönüştürmek için dize. Dizedeki karakter küçük eşdeğeri yoksa, döndürülen dize değişmeden bulunur.|  
|toUpper|Bir dizeyi büyük harfe dönüştürür. Örneğin, aşağıdaki deyim döndürür `TWO BY TWO IS FOUR`:  `toUpper('Two by Two is Four')`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: dize<br /><br /> **Açıklama**: gerekli. Üst kasasını dönüştürmek için dize. Büyük harf eşdeğer bir dizedeki karakter sahip değilse, döndürülen dize değişmeden bulunur.|  
|IndexOf|Bir dize çalışması içinde bir değerle dizinini insensitively bulun. Örneğin, aşağıdaki deyim döndürür `7`: `indexof('hello, world.', 'world')`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: dize<br /><br /> **Açıklama**: gerekli. Değer içerebilir dize.<br /><br /> **Numaralı parametre**: 2<br /><br /> **Ad**: dize<br /><br /> **Açıklama**: gerekli. Dizini arama için değer.|  
|lastIndexOf|Bir dize çalışması arasında bir değer son dizinini insensitively bulun. Örneğin, aşağıdaki deyim döndürür `3`: `lastindexof('foofoo', 'foo')`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: dize<br /><br /> **Açıklama**: gerekli. Değer içerebilir dize.<br /><br /> **Numaralı parametre**: 2<br /><br /> **Ad**: dize<br /><br /> **Açıklama**: gerekli. Dizini arama için değer.|  
|startswith|Dize değeri durumuyla insensitively başlayıp başlamadığını denetler. Örneğin, aşağıdaki deyim döndürür `true`: `lastindexof('hello, world', 'hello')`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: dize<br /><br /> **Açıklama**: gerekli. Değer içerebilir dize.<br /><br /> **Numaralı parametre**: 2<br /><br /> **Ad**: dize<br /><br /> **Açıklama**: gerekli. Dize değeri ile başlayabilir.|  
|endswith|Dize değeri durumuyla insensitively bitip bitmediğini denetler. Örneğin, aşağıdaki deyim döndürür `true`: `lastindexof('hello, world', 'world')`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: dize<br /><br /> **Açıklama**: gerekli. Değer içerebilir dize.<br /><br /> **Numaralı parametre**: 2<br /><br /> **Ad**: dize<br /><br /> **Açıklama**: gerekli. Dize değeri ile bitebilir.|  
|split|Ayırıcı kullanarak dize böler. Örneğin, aşağıdaki deyim döndürür `["a", "b", "c"]`: `split('a;b;c',';')`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: dize<br /><br /> **Açıklama**: gerekli. Bölünen dize.<br /><br /> **Numaralı parametre**: 2<br /><br /> **Ad**: dize<br /><br /> **Açıklama**: gerekli. Ayırıcı.|  
  
  
## <a name="collection-functions"></a>Toplama işlevleri  
 Diziler, dizeleri ve bazen sözlükler gibi koleksiyonlar üzerinden bu işlevler çalışır.  
  
|İşlev adı|Açıklama|  
|-------------------|-----------------|  
|içerir|Dize alt dizeyi içeren veya sözlük bir anahtar, listesi içeriyorsa true döndürür değeri içeriyor. Örneğin, aşağıdaki ifade döndürür `true:``contains('abacaba','aca')`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: koleksiyonundaki<br /><br /> **Açıklama**: gerekli. Aranacak koleksiyonu.<br /><br /> **Numaralı parametre**: 2<br /><br /> **Ad**: Bul nesnesi<br /><br /> **Açıklama**: gerekli. İçinde bulunacak nesne **koleksiyonundaki**.|  
|uzunluğu|Bir dizi veya dize öğe sayısını döndürür. Örneğin, aşağıdaki deyim döndürür `3`:  `length('abc')`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: koleksiyonu<br /><br /> **Açıklama**: gerekli. Uzunluğu alma koleksiyonu.|  
|Boş|Nesne, dizi veya dize boşsa, true döndürür. Örneğin, aşağıdaki deyim döndürür `true`:<br /><br /> `empty('')`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: koleksiyonu<br /><br /> **Açıklama**: gerekli. Boş olup olmadığını denetlemek için koleksiyonu.|  
|Kesişim|Kendisine geçirilen nesneler ve diziler arasında bir tek bir dizi veya ortak öğeler nesneyi döndürür. Örneğin, bu işlevi döndürür `[1, 2]`:<br /><br /> `intersection([1, 2, 3], [101, 2, 1, 10],[6, 8, 1, 2])`<br /><br /> İşlev parametrelerini ya da nesne veya dizi (olmayan bir karışımını bunların) kümesi olabilir. Aynı ada sahip iki nesne, bu adı taşıyan son nesne son nesnesinde görüntülenir.<br /><br /> **Numaralı parametre**: 1... *n*<br /><br /> **Ad**: koleksiyon *n*<br /><br /> **Açıklama**: gerekli. Değerlendirilecek koleksiyonları. Bir nesne sonuçta görünmesini geçirilen tüm koleksiyonlar olması gerekir.|  
|birleşim|Bir tek bir dizi veya dizi veya nesne kendisine geçirilen tüm öğeleri nesneyi döndürür. Örneğin, bu işlevi döndürür `[1, 2, 3, 10, 101]:`<br /><br /> :  `union([1, 2, 3], [101, 2, 1, 10])`<br /><br /> İşlev parametrelerini ya da nesne veya dizi (olmayan bir karışımını bunların) kümesi olabilir. Son çıktıda aynı ada sahip iki nesne, bu adı taşıyan son nesne son nesnesinde görüntülenir.<br /><br /> **Numaralı parametre**: 1... *n*<br /><br /> **Ad**: koleksiyon *n*<br /><br /> **Açıklama**: gerekli. Değerlendirilecek koleksiyonları. Koleksiyonları hiçbirinde görünen bir nesnenin sonucunda görünür.|  
|ilk|Dizi ya da geçirilen dize ilk öğeyi döndürür. Örneğin, bu işlevi döndürür `0`:<br /><br /> `first([0,2,3])`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: koleksiyonu<br /><br /> **Açıklama**: gerekli. İlk nesneyi yapılacak koleksiyonu.|  
|En son|Dizi ya da geçirilen dize son öğeyi döndürür. Örneğin, bu işlevi döndürür `3`:<br /><br /> `last('0123')`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: koleksiyonu<br /><br /> **Açıklama**: gerekli. Son nesnesinden yapılacak koleksiyonu.|  
|Al|İlk döndürür **sayısı** dizisi veya dize öğelerinden geçirilen, örneğin bu işlevi döndürür `[1, 2]`:  `take([1, 2, 3, 4], 2)`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: koleksiyonu<br /><br /> **Açıklama**: gerekli. İlk yapılacak koleksiyonu **sayısı** nesnelerin.<br /><br /> **Numaralı parametre**: 2<br /><br /> **Ad**: sayısı<br /><br /> **Açıklama**: gerekli. Gelen yapılacak nesne sayısı **koleksiyonu**. Pozitif bir tamsayı olmalıdır.|  
|Atla|Öğeleri dizinden başlayarak bir diziye döndürür **sayısı**, örneğin bu işlevi döndürür `[3, 4]`:<br /><br /> `skip([1, 2 ,3 ,4], 2)`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: koleksiyonu<br /><br /> **Açıklama**: gerekli. İlk atlamak için koleksiyon **sayısı** nesnelerin.<br /><br /> **Numaralı parametre**: 2<br /><br /> **Ad**: sayısı<br /><br /> **Açıklama**: gerekli. Önüne kaldırmak için nesne sayısı **koleksiyonu**. Pozitif bir tamsayı olmalıdır.|  
  
## <a name="logical-functions"></a>Mantıksal işlevler  
 Bu işlevler içinde koşullar faydalıdır, herhangi bir türde mantığı değerlendirmek için kullanılabilir.  
  
|İşlev adı|Açıklama|  
|-------------------|-----------------|  
|şuna eşittir:|İki değer eşitse true değerini döndürür. Örneğin, parametre1 foo ise, aşağıdaki deyim dönüş `true`: `equals(pipeline().parameters.parameter1), 'foo')`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: 1 nesne<br /><br /> **Açıklama**: gerekli. Karşılaştırma yapılacak nesne **nesne 2**.<br /><br /> **Numaralı parametre**: 2<br /><br /> **Ad**: 2 nesnesi<br /><br /> **Açıklama**: gerekli. Karşılaştırma yapılacak nesne **nesne 1**.|  
|daha az|İlk bağımsız değişken daha az ise true değeri döndürür ikinciden. Not, değerler yalnızca türü tamsayı, kayan noktalı sayı veya dize olabilir. Örneğin, aşağıdaki deyim döndürür `true`:  `less(10,100)`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: 1 nesne<br /><br /> **Açıklama**: gerekli. Olup olmadığını denetlemek için nesne değerinden **nesne 2**.<br /><br /> **Numaralı parametre**: 2<br /><br /> **Ad**: 2 nesnesi<br /><br /> **Açıklama**: gerekli. Büyük olup olmadığını denetlemek için nesne **nesne 1**.|  
|lessOrEquals|İlk bağımsız değişken ikinci eşit veya daha az ise true, aksi durumda değeri döndürür. Not, değerler yalnızca türü tamsayı, kayan noktalı sayı veya dize olabilir. Örneğin, aşağıdaki deyim döndürür `true`:  `lessOrEquals(10,10)`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: 1 nesne<br /><br /> **Açıklama**: gerekli. Bu daha az olup olmadığını denetleyin veya eşit nesnesine **nesne 2**.<br /><br /> **Numaralı parametre**: 2<br /><br /> **Ad**: 2 nesnesi<br /><br /> **Açıklama**: gerekli. Büyük veya eşit olup olmadığını denetlemek için nesne **nesne 1**.|  
|büyük|İlk bağımsız değişken saniyeden büyükse, true döndürür. Not, değerler yalnızca türü tamsayı, kayan noktalı sayı veya dize olabilir. Örneğin, aşağıdaki deyim döndürür `false`:  `greater(10,10)`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: 1 nesne<br /><br /> **Açıklama**: gerekli. Büyük olup olmadığını denetlemek için nesne **nesne 2**.<br /><br /> **Numaralı parametre**: 2<br /><br /> **Ad**: 2 nesnesi<br /><br /> **Açıklama**: gerekli. Olup olmadığını denetlemek için nesne değerinden **nesne 1**.|  
|greaterOrEquals|İlk bağımsız değişkeni sıfırdan büyük veya ona eşit olduğunda true değerini döndürür. Not, değerler yalnızca türü tamsayı, kayan noktalı sayı veya dize olabilir. Örneğin, aşağıdaki deyim döndürür `false`:  `greaterOrEquals(10,100)`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: 1 nesne<br /><br /> **Açıklama**: gerekli. Büyük veya eşit olup olmadığını denetlemek için nesne **nesne 2**.<br /><br /> **Numaralı parametre**: 2<br /><br /> **Ad**: 2 nesnesi<br /><br /> **Açıklama**: gerekli. Bu küçük veya eşit olup olmadığını denetlemek için nesne **nesne 1**.|  
|ve|Parametrelerinin her ikisi de doğruysa true döndürür. Her iki değişken Boole değerlerini olması gerekir. Aşağıdaki döndürür `false`:  `and(greater(1,10),equals(0,0))`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: Boolean 1<br /><br /> **Açıklama**: gerekli. Gereken ilk bağımsız değişken `true`.<br /><br /> **Numaralı parametre**: 2<br /><br /> **Ad**: Boolean 2<br /><br /> **Açıklama**: gerekli. İkinci bağımsız değişkeni olmalıdır `true`.|  
|or|Parametrelerden biri doğru olduğunda true döndürür. Her iki değişken Boole değerlerini olması gerekir. Aşağıdaki döndürür `true`:  `or(greater(1,10),equals(0,0))`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: Boolean 1<br /><br /> **Açıklama**: gerekli. Olabilir ilk bağımsız değişken `true`.<br /><br /> **Numaralı parametre**: 2<br /><br /> **Ad**: Boolean 2<br /><br /> **Açıklama**: gerekli. İkinci bağımsız değişkeni olabilir `true`.|  
|Değil|Parametre ise true değeri döndürür `false`. Her iki değişken Boole değerlerini olması gerekir. Aşağıdaki döndürür `true`:  `not(contains('200 Success','Fail'))`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: Boolean<br /><br /> **Açıklama**: parametre ise true değerini döndürür `false`. Her iki değişken Boole değerlerini olması gerekir. Aşağıdaki döndürür `true`:  `not(contains('200 Success','Fail'))`|  
|Eğer|Belirtilen bir değeri temel açıksa sağlanan ifade sonuçları döndürür `true` veya `false`.  Örneğin, aşağıdaki döndürür `"yes"`: `if(equals(1, 1), 'yes', 'no')`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: ifade<br /><br /> **Açıklama**: gerekli. Hangi değer ifadesi tarafından döndürülen belirleyen bir boolean değeri.<br /><br /> **Numaralı parametre**: 2<br /><br /> **Ad**: True<br /><br /> **Açıklama**: gerekli. İfade ise döndürülecek değer `true`.<br /><br /> **Numaralı parametre**: 3<br /><br /> **Ad**: yanlış<br /><br /> **Açıklama**: gerekli. İfade ise döndürülecek değer `false`.|  
  
## <a name="conversion-functions"></a>Dönüşüm işlevleri  
 Bu işlevlerin her dil içindeki yerel türler arasında dönüştürme için kullanılır:  
  
-   dize  
  
-   integer  
  
-   float  
  
-   boole  
  
-   Diziler  
  
-   Sözlük  
  
|İşlev adı|Açıklama|  
|-------------------|-----------------|  
|int|Parametresi, bir tamsayıya dönüştürür. Örneğin, aşağıdaki ifade bir dize yerine bir sayı olarak 100 döndürür:  `int('100')`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: değer<br /><br /> **Açıklama**: gerekli. Bir tamsayıya dönüştürülüp değeri.|  
|dize|Parametresi bir dizeye dönüştürün. Örneğin, aşağıdaki deyim döndürür `'10'`: `string(10)` , ayrıca bir nesne örneği için bir dizeye dönüştürebilirsiniz **foo** parametredir bir özelliği olan bir nesne `bar : baz`, sonra da aşağıdaki gerekir Döndür `{"bar" : "baz"}` `string(pipeline().parameters.foo)`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: değer<br /><br /> **Açıklama**: gerekli. Bir dizeye dönüştürülen değer.|  
|json|Parametresi bir JSON türü değerine dönüştürür. String() tersidir. Örneğin, aşağıdaki deyim döndürür `[1,2,3]` bir dize yerine bir dizi olarak:<br /><br /> `json('[1,2,3]')`<br /><br /> Benzer şekilde, bir nesneye bir dize dönüştürebilirsiniz. Örneğin, `json('{"bar" : "baz"}')` döndürür:<br /><br /> `{ "bar" : "baz" }`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: dize<br /><br /> **Açıklama**: gerekli. Yerel tür değerine dönüştürülüp dize.<br /><br /> Json işlevi xml girişi destekler. Örneğin, parametre değeri:<br /><br /> `<?xml version="1.0"?> <root>   <person id='1'>     <name>Alan</name>     <occupation>Engineer</occupation>   </person> </root>`<br /><br /> Aşağıdaki json dönüştürülür:<br /><br /> `{ "?xml": { "@version": "1.0" },   "root": {     "person": [     {       "@id": "1",       "name": "Alan",       "occupation": "Engineer"     }   ]   } }`|  
|float|Parametre bağımsız değişkeni bir kayan nokta sayıya dönüştürün. Örneğin, aşağıdaki deyim döndürür `10.333`:  `float('10.333')`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: değer<br /><br /> **Açıklama**: gerekli. Kayan nokta sayıya dönüştürülmüş değeri.|  
|bool|Parametre bir Boole değeri dönüştürün. Örneğin, aşağıdaki deyim döndürür `false`:  `bool(0)`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: değer<br /><br /> **Açıklama**: gerekli. Bir Boole değeri dönüştürülen değer.|  
|birleşim|Geçirilen bağımsız değişkenleri ilk null olmayan nesne döndürür. Not: boş bir dize null değil. Örneğin, 1 ve 2 parametreleri tanımlı değil, bu verir `fallback`:  `coalesce(pipeline().parameters.parameter1', pipeline().parameters.parameter2 ,'fallback')`<br /><br /> **Numaralı parametre**: 1... *n*<br /><br /> **Ad**: nesne*n*<br /><br /> **Açıklama**: gerekli. Denetlenecek nesneleri `null`.|  
|Base64|Giriş dizesi base64 gösterimini döndürür. Örneğin, aşağıdaki deyim döndürür `c29tZSBzdHJpbmc=`:  `base64('some string')`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: Dize 1<br /><br /> **Açıklama**: gerekli. Base64 gösterimine kodlanacak dize.|  
|base64ToBinary|Bir base64 kodlu dize ikili bir gösterimini döndürür. Örneğin, aşağıdaki ifade ikili bazı dize gösterimini döndürür: `base64ToBinary('c29tZSBzdHJpbmc=')`.<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: dize<br /><br /> **Açıklama**: gerekli. Base64 ile kodlanmış dize.|  
|base64ToString|Kodlanmış based64 dize dize gösterimini döndürür. Örneğin, aşağıdaki ifade bazı dizesini döndürür: `base64ToString('c29tZSBzdHJpbmc=')`.<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: dize<br /><br /> **Açıklama**: gerekli. Base64 ile kodlanmış dize.|  
|İkili|Değer ikili bir gösterimini döndürür.  Örneğin, aşağıdaki ifade, bazı dize ikili bir gösterimini döndürür: `binary(‘some string’).`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: değer<br /><br /> **Açıklama**: gerekli. İkili dosya dönüştürülen değer.|  
|dataUriToBinary|Bir ikili veri URI'si gösterimini döndürür. Örneğin, aşağıdaki ifade ikili bazı dize gösterimini döndürür: `dataUriToBinary('data:;base64,c29tZSBzdHJpbmc=')`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: dize<br /><br /> **Açıklama**: gerekli. Veriler ikili gösterimine dönüştürmek için URI.|  
|dataUriToString|Bir veri URI dize gösterimini döndürür. Örneğin, aşağıdaki ifade, bazı dizesini döndürür: `dataUriToString('data:;base64,c29tZSBzdHJpbmc=')`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: dize<br /><br />**Açıklama**: gerekli. Verileri dize gösterimi dönüştürmek için URI.|  
|dataUri|Bir veri URI değeri döndürür. Örneğin, aşağıdaki ifade verileri döndürür: `text/plain;charset=utf8;base64,c29tZSBzdHJpbmc=: dataUri('some string')`<br /><br /> **Numaralı parametre**: 1<br /><br />**Ad**: değer<br /><br />**Açıklama**: gerekli. Veri URI'si dönüştürülecek değer.|  
|decodeBase64|Bir giriş based64 dize dize gösterimini döndürür. Örneğin, aşağıdaki deyim döndürür `some string`:  `decodeBase64('c29tZSBzdHJpbmc=')`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: dize<br /><br /> **Açıklama**: bir giriş based64 dize dize gösterimini döndürür.|  
|Encodeurıcomponent|URL çıkışları geçirilen dize. Örneğin, aşağıdaki deyim döndürür `You+Are%3ACool%2FAwesome`:  `encodeUriComponent('You Are:Cool/Awesome')`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: dize<br /><br /> **Açıklama**: gerekli. URL güvenli olmayan karakterler kaçınmak için dize.|  
|Decodeurıcomponent|Kaydını URL çıkışları geçirilen dize. Örneğin, aşağıdaki deyim döndürür `You Are:Cool/Awesome`:  `encodeUriComponent('You+Are%3ACool%2FAwesome')`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: dize<br /><br /> **Açıklama**: gerekli. URL güvenli olmayan karakterler çözecek dizesi.|  
|decodeDataUri|İkili bir giriş verileri URI dize gösterimini döndürür. Örneğin, aşağıdaki ifade ikili gösterimini döndürür `some string`:  `decodeDataUri('data:;base64,c29tZSBzdHJpbmc=')`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: dize<br /><br /> **Açıklama**: gerekli. İkili gösterimine çözecek dataURI.|  
|uriComponent|Bir URI döndürür değerinin gösterimiyle kodlanmış. Örneğin, aşağıdaki ifade döndürür `You+Are%3ACool%2FAwesome: uriComponent('You Are:Cool/Awesome ')`<br /><br /> Parametre ayrıntıları: Numara: 1, ad: String, Açıklama: gerekli. URI ile kodlanan olmasını dizesi.|  
|uriComponentToBinary|Bir URI ikili gösterimidir kodlanmış dizesi döndürür. Örneğin, aşağıdaki ifade bir ikili biçimi döndürür `You Are:Cool/Awesome`: `uriComponentToBinary('You+Are%3ACool%2FAwesome')`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: dize<br /><br />**Açıklama**: gerekli. URI kodlanmış dize.|  
|uriComponentToString|Bir URI dize gösterimini kodlanmış dizesi döndürür. Örneğin, aşağıdaki deyim döndürür `You Are:Cool/Awesome`: `uriComponentToBinary('You+Are%3ACool%2FAwesome')`<br /><br /> **Numaralı parametre**: 1<br /><br />**Ad**: dize<br /><br />**Açıklama**: gerekli. URI kodlanmış dize.|  
|xml|Değerinin xml gösterimini döndürür. Örneğin, aşağıdaki ifade tarafından temsil edilen bir xml içeriğini döndürür `'\<name>Alan\</name>'`: `xml('\<name>Alan\</name>')`. JSON nesnesi de Giriş xml işlevi destekler. Örneğin, parametre `{ "abc": "xyz" }` bir xml içeriği dönüştürülür `\<abc>xyz\</abc>`<br /><br /> **Numaralı parametre**: 1<br /><br />**Ad**: değer<br /><br />**Açıklama**: gerekli. XML biçimine dönüştürülecek değer.|  
|XPath|Xpath ifadesi xpath ifadesi değerlendiren bir değeri ile eşleşen xml düğümleri bir dizi döndürür.<br /><br />  **Örnek 1**<br /><br /> Aşağıdaki XML dize gösterimi 'p1' parametresinin değeri olduğunu varsayın:<br /><br /> `<?xml version="1.0"?> <lab>   <robot>     <parts>5</parts>     <name>R1</name>   </robot>   <robot>     <parts>8</parts>     <name>R2</name>   </robot> </lab>`<br /><br /> 1. Bu kod: `xpath(xml(pipeline().parameters.p1), '/lab/robot/name')`<br /><br /> döndürür<br /><br /> `[ <name>R1</name>, <name>R2</name> ]`<br /><br /> Oysa<br /><br /> 2. Bu kod: `xpath(xml(pipeline().parameters.p1, ' sum(/lab/robot/parts)')`<br /><br /> döndürür<br /><br /> `13`<br /><br /> <br /><br /> **Örnek 2**<br /><br /> Aşağıdaki XML içeriğini verilen:<br /><br /> `<?xml version="1.0"?> <File xmlns="http://foo.com">   <Location>bar</Location> </File>`<br /><br /> 1.  Bu kod: `@xpath(xml(body('Http')), '/*[name()=\"File\"]/*[name()=\"Location\"]')`<br /><br /> or<br /><br /> 2. Bu kod: `@xpath(xml(body('Http')), '/*[local-name()=\"File\" and namespace-uri()=\"http://foo.com\"]/*[local-name()=\"Location\" and namespace-uri()=\"\"]')`<br /><br /> Döndürür<br /><br /> `<Location xmlns="http://foo.com">bar</Location>`<br /><br /> ve<br /><br /> 3. Bu kod: `@xpath(xml(body('Http')), 'string(/*[name()=\"File\"]/*[name()=\"Location\"])')`<br /><br /> Döndürür<br /><br /> ``bar``<br /><br /> **Numaralı parametre**: 1<br /><br />**Ad**: Xml<br /><br />**Açıklama**: gerekli. XPath ifadesi değerlendirileceği XML.<br /><br /> **Numaralı parametre**: 2<br /><br />**Ad**: XPath<br /><br />**Açıklama**: gerekli. Değerlendirilecek XPath ifadesi.|  
|array|Parametresi bir diziye dönüştürür.  Örneğin, aşağıdaki deyim döndürür `["abc"]`: `array('abc')`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: değer<br /><br /> **Açıklama**: gerekli. Bir dizi dönüştürülen değer.|
|createArray|Bir dizi parametrelerinden oluşturur.  Örneğin, aşağıdaki deyim döndürür `["a", "c"]`: `createArray('a', 'c')`<br /><br /> **Numaralı parametre**: 1... n<br /><br /> **Ad**: tüm n<br /><br /> **Açıklama**: gerekli. Bir diziye birleştirmek için kullanılan değerler.|

## <a name="math-functions"></a>Matematiksel işlevler  
 Bu işlevlerin her iki tür numarası için kullanılabilir: **tamsayılar** ve **gezinen**.  
  
|İşlev adı|Açıklama|  
|-------------------|-----------------|  
|ekle|Ayrıca iki sayının sonucunu döndürür. Örneğin, bu işlevi döndürür `20.333`:  `add(10,10.333)`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: Summand 1<br /><br /> **Açıklama**: gerekli. Eklemek üzere numarasını **Summand 2**.<br /><br /> **Numaralı parametre**: 2<br /><br /> **Ad**: Summand 2<br /><br /> **Açıklama**: gerekli. Eklemek üzere numarasını **Summand 1**.|  
|Sub|Çıkarma iki sayının sonucunu döndürür. Örneğin, bu işlevi döndürür: `-0.333`:<br /><br /> `sub(10,10.333)`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: Minuend<br /><br /> **Açıklama**: gerekli. Sayı, **Subtrahend** kaldırılır.<br /><br /> **Numaralı parametre**: 2<br /><br /> **Ad**: Subtrahend<br /><br /> **Açıklama**: gerekli. Kaldırmak üzere numarasını **Minuend**.|  
|mul|İki sayının Çarpım sonucunu döndürür. Örneğin, aşağıdaki döndürür `103.33`:<br /><br /> `mul(10,10.333)`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: Multiplicand 1<br /><br /> **Açıklama**: gerekli. Çarp üzere numarasını **Multiplicand 2** ile.<br /><br /> **Numaralı parametre**: 2<br /><br /> **Ad**: Multiplicand 2<br /><br /> **Açıklama**: gerekli. Çarp üzere numarasını **Multiplicand 1** ile.|  
|div|İki sayının bölümünün sonucunu döndürür. Örneğin, aşağıdaki döndürür `1.0333`:<br /><br /> `div(10.333,10)`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: bölünen<br /><br /> **Açıklama**: gerekli. Sayı bölün **bölen**.<br /><br /> **Numaralı parametre**: 2<br /><br /> **Ad**: bölen<br /><br /> **Açıklama**: gerekli. Bölmek için numarası **bölünen** tarafından.|  
|mod|(Modül) iki sayının bölümünün sonra geri kalan sonucunu döndürür. Örneğin, aşağıdaki deyim döndürür `2`:<br /><br /> `mod(10,4)`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: bölünen<br /><br /> **Açıklama**: gerekli. Sayı bölün **bölen**.<br /><br /> **Numaralı parametre**: 2<br /><br /> **Ad**: bölen<br /><br /> **Açıklama**: gerekli. Bölmek için numarası **bölünen** tarafından. Ayırmadan sonra kalan alınır.|  
|dk|Bu işlevi çağırmak için iki farklı modeli vardır: `min([0,1,2])` burada en az bir dizi alır. Bu ifade döndürür `0`. Alternatif olarak, bu işlev virgülle ayrılmış bir liste değerleri alabilir: `min(0,1,2)` bu işlev, aynı zamanda 0 döndürür. Not: parametre dizisi ise, yalnızca rakam içinde iznine sahip olması gerekir böylece tüm değerleri sayı olması gerekir.<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: koleksiyon veya değeri<br /><br /> **Açıklama**: gerekli. Ayrıca, en düşük değer veya bir küme ilk değerini bulmak için değerler dizisi ya da olabilir.<br /><br /> **Numaralı parametre**: 2... *n*<br /><br /> **Ad**: değer *n*<br /><br /> **Açıklama**: isteğe bağlı. İlk parametre bir değer ise, ardından ek değerler geçirebilir ve geçirilen tüm değerlerin minimum döndürülür.|  
|en çok|Bu işlevi çağırmak için iki farklı modeli vardır:  `max([0,1,2])`<br /><br /> Burada en fazla bir dizi alır. Bu ifade döndürür `2`. Alternatif olarak, bu işlev virgülle ayrılmış bir liste değerleri alabilir: `max(0,1,2)` bu işlev, 2 döndürür. Not: parametre dizisi ise, yalnızca rakam içinde iznine sahip olması gerekir böylece tüm değerleri sayı olması gerekir.<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: koleksiyon veya değeri<br /><br /> **Açıklama**: gerekli. Ayrıca, en büyük değer veya bir küme ilk değerini bulmak için değerler dizisi ya da olabilir.<br /><br /> **Numaralı parametre**: 2... *n*<br /><br /> **Ad**: değer *n*<br /><br /> **Açıklama**: isteğe bağlı. İlk parametre bir değer ise, ardından ek değerler geçirebilir ve maksimum tüm geçirilen değeri döndürülür.|  
|Aralık| Belirli arasında bir sayı, başlangıç dizisi oluşturur ve döndürülen dizi uzunluğu, tanımlayın. Örneğin, bu işlevi döndürür `[3,4,5,6]`:<br /><br /> `range(3,4)`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: Başlangıç dizini<br /><br /> **Açıklama**: gerekli. Dizideki ilk tamsayıdır.<br /><br /> **Numaralı parametre**: 2<br /><br /> **Ad**: sayısı<br /><br /> **Açıklama**: gerekli. Dizesindedir tamsayılar sayısı.|  
|rand| (Her iki uçtaki. belirtilen aralıkta rasgele bir tamsayı oluşturur Örneğin, bu döndürebilirsiniz `42`:<br /><br /> `rand(-1000,1000)`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: en az<br /><br /> **Açıklama**: gerekli. Döndürülebilen en küçük tamsayı.<br /><br /> **Numaralı parametre**: 2<br /><br /> **Ad**: en fazla<br /><br /> **Açıklama**: gerekli. Döndürülebilen en büyük tamsayıyı.|  
  
## <a name="date-functions"></a>Date işlevleri  
  
|İşlev adı|Açıklama|  
|-------------------|-----------------|  
|UtcNow|Geçerli zaman damgası dize olarak döndürür. Örneğin `2015-03-15T13:27:36Z`:<br /><br /> `utcnow()`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: biçimi<br /><br /> **Açıklama**: isteğe bağlı. Ya da bir [tek bir biçim belirticisi karakter](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) veya [özel biçim deseni](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) bu zaman damgası değeri biçimine gösterir. Biçim sağlanmazsa, ISO 8601 biçiminde ("o") kullanılır.|  
|saniyeEkle|Bir tamsayı saniyeyi geçirilen dize zaman damgası ekler. Saniye sayısı pozitif veya negatif olabilir. Bir biçim belirticisi sağlanmadığı sürece varsayılan olarak, ("o") ISO 8601 biçiminde bir dize sonucudur. Örneğin `2015-03-15T13:27:00Z`:<br /><br /> `addseconds('2015-03-15T13:27:36Z', -36)`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: zaman damgası<br /><br /> **Açıklama**: gerekli. Saati içeren bir dize.<br /><br /> **Numaralı parametre**: 2<br /><br /> **Ad**: saniye<br /><br /> **Açıklama**: gerekli. Eklemek için saniye sayısı. Saniye çıkartılacak negatif olabilir.<br /><br /> **Numaralı parametre**: 3<br /><br /> **Ad**: biçimi<br /><br /> **Açıklama**: isteğe bağlı. Ya da bir [tek bir biçim belirticisi karakter](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) veya [özel biçim deseni](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) bu zaman damgası değeri biçimine gösterir. Biçim sağlanmazsa, ISO 8601 biçiminde ("o") kullanılır.|  
|AddMinutes|Bir dakika sayısıyla geçirilen dize zaman damgası ekler. Dakika sayısını olumlu veya olumsuz olabilir. Bir biçim belirticisi sağlanmadığı sürece varsayılan olarak, ("o") ISO 8601 biçiminde bir dize sonucudur. Örneğin, `2015-03-15T14:00:36Z`:<br /><br /> `addminutes('2015-03-15T13:27:36Z', 33)`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: zaman damgası<br /><br /> **Açıklama**: gerekli. Saati içeren bir dize.<br /><br /> **Numaralı parametre**: 2<br /><br /> **Ad**: dakika<br /><br /> **Açıklama**: gerekli. Eklemek için dakika sayısı. Dakika çıkartılacak negatif olabilir.<br /><br /> **Numaralı parametre**: 3<br /><br /> **Ad**: biçimi<br /><br /> **Açıklama**: isteğe bağlı. Ya da bir [tek bir biçim belirticisi karakter](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) veya [özel biçim deseni](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) bu zaman damgası değeri biçimine gösterir. Biçim sağlanmazsa, ISO 8601 biçiminde ("o") kullanılır.|  
|addhours|Bir tamsayı saat geçirilen dize zaman damgası ekler. Saat sayısı pozitif veya negatif olabilir. Bir biçim belirticisi sağlanmadığı sürece varsayılan olarak, ("o") ISO 8601 biçiminde bir dize sonucudur. Örneğin `2015-03-16T01:27:36Z`:<br /><br /> `addhours('2015-03-15T13:27:36Z', 12)`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: zaman damgası<br /><br /> **Açıklama**: gerekli. Saati içeren bir dize.<br /><br /> **Numaralı parametre**: 2<br /><br /> **Ad**: saat<br /><br /> **Açıklama**: gerekli. Eklemek için saat sayısı. Saat çıkartılacak negatif olabilir.<br /><br /> **Numaralı parametre**: 3<br /><br /> **Ad**: biçimi<br /><br /> **Açıklama**: isteğe bağlı. Ya da bir [tek bir biçim belirticisi karakter](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) veya [özel biçim deseni](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) bu zaman damgası değeri biçimine gösterir. Biçim sağlanmazsa, ISO 8601 biçiminde ("o") kullanılır.|  
|adddays|Geçirilen bir dize zaman damgası gün tamsayı sayısı ekler. Gün sayısı pozitif veya negatif olabilir. Bir biçim belirticisi sağlanmadığı sürece varsayılan olarak, ("o") ISO 8601 biçiminde bir dize sonucudur. Örneğin `2015-02-23T13:27:36Z`:<br /><br /> `adddays('2015-03-15T13:27:36Z', -20)`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: zaman damgası<br /><br /> **Açıklama**: gerekli. Saati içeren bir dize.<br /><br /> **Numaralı parametre**: 2<br /><br /> **Ad**: gün<br /><br /> **Açıklama**: gerekli. Eklenecek gün sayısı. Gün çıkartılacak negatif olabilir.<br /><br /> **Numaralı parametre**: 3<br /><br /> **Ad**: biçimi<br /><br /> **Açıklama**: isteğe bağlı. Ya da bir [tek bir biçim belirticisi karakter](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) veya [özel biçim deseni](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) bu zaman damgası değeri biçimine gösterir. Biçim sağlanmazsa, ISO 8601 biçiminde ("o") kullanılır.|  
|FormatDateTime|Tarih biçiminde bir dize döndürür. Bir biçim belirticisi sağlanmadığı sürece varsayılan olarak, ("o") ISO 8601 biçiminde bir dize sonucudur. Örneğin `2015-02-23T13:27:36Z`:<br /><br /> `formatDateTime('2015-03-15T13:27:36Z', 'o')`<br /><br /> **Numaralı parametre**: 1<br /><br /> **Ad**: tarih<br /><br /> **Açıklama**: gerekli. Tarih içeren bir dize.<br /><br /> **Numaralı parametre**: 2<br /><br /> **Ad**: biçimi<br /><br /> **Açıklama**: isteğe bağlı. Ya da bir [tek bir biçim belirticisi karakter](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) veya [özel biçim deseni](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) bu zaman damgası değeri biçimine gösterir. Biçim sağlanmazsa, ISO 8601 biçiminde ("o") kullanılır.|  

## <a name="next-steps"></a>Sonraki adımlar
İfadelerde kullanabileceğiniz sistem değişkenleri listesi için bkz: [sistem değişkenleri](control-flow-system-variables.md).
