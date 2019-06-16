---
title: Lambda arama söz dizimi - akademik bilgi API'si
titlesuffix: Azure Cognitive Services
description: Akademik bilgi API'si kullanabilirsiniz Lambda arama söz dizimi hakkında bilgi edinin.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 4d4c540e00794bfdf1df265457798cc13530c828
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61337797"
---
# <a name="lambda-search-syntax"></a>Lambda arama söz dizimi

Her *lambda* dizeyi tanımlayan bir grafik desenini arama sorgusu. Bir sorgu grafik düğüm biz geçişi Başlat belirterek en az bir başlangıç düğümü olmalıdır. Başlangıç düğümünün belirtmek için çağrı *MAG. StartFrom()* nesne yöntemi ve bir veya daha fazla düğüm veya bir sorgu kimlikleri geçişinde arama kısıtlamaları belirtir. *StartFrom()* yöntemi üç aşırı yüklemeleri vardır. Bunların tümünde ikinci olan ile iki bağımsız değişken isteğe bağlı almaz. İlk bağımsız değişken büyük bir tamsayı, büyük tamsayı, numaralandırılabilir bir koleksiyonunu olabilir veya olarak aynı semantiğe sahip bir JSON temsil eden bir dize nesnesi *json* arama:
```
StartFrom(long cellid, IEnumerable<string> select = null)
StartFrom(IEnumerable<long> cellid, IEnumerable<string> select = null)
StartFrom(string queryObject, IEnumerable<string> select = null)
```

Lambda arama sorgusu yazma işleminin bir düğümden başka bir yol sağlamaktır. İzlenecek yol için edge türünü belirtmek için kullanın *FollowEdge()* ve istenen edge türlerinde geçirin. *FollowEdge()* tercihe bağlı sayıda dize bağımsız değişkeni alır:
```
FollowEdge(params string[] edgeTypes)
```
> [!NOTE]
> Sadece biz izlemenizi edge(s) türlerini hakkında önemsemiyorsanız atlamak *FollowEdge()* iki düğüm arasındaki: Sorgu bu iki düğüm arasında tüm olası kenarları boyunca size yol gösterir.

Bir düğüme uygulanacak geçişi eylemleri belirttiğimiz *VisitNode()* , diğer bir deyişle, bu düğümde durdurun ve sonucu olarak geçerli bir yol döndürür veya graph incelemeye devam edin.  Enum türü *eylem* iki tür eylem tanımlar: *Action.Return* ve *Action.Continue*. Biz bu tür bir sabit listesi değeri doğrudan geçirebilirsiniz *VisitNode()* , veya bit düzeyinde ile birleştirip- and işleci '&'. İki eylem birleştirilir, hem eylemler gerçekleştirilecek anlamına gelir. Not: bit düzeyinde kullanmayın- or işleci ' |' eylemleri. Bunun yapılması, hiçbir şey dönmeden sonlandırmak sorgu neden olur. Atlama *VisitNode()* iki *FollowEdge()* koşulsuz olarak bir düğümde geldikten sonra graf keşfetmek sorgu çağrıları neden olur.

```
VisitNode(Action action, IEnumerable<string> select = null)
```

İçin *VisitNode()* , tür lambda ifadesinde biz de geçirebilirsiniz *ifade\<Func\<Inode, eylem\>\>* , bir aldığı*Inode* ve geçişi eylem döndürür:

```
VisitNode(Expression<Func<INode, Action>> action, IEnumerable<string> select = null)
```

## <a name="inode"></a>*Inode* 

*Inode* sağlar *salt okunur* verilere arabirimleri ve bir düğüm üzerinde birkaç yerleşik yardımcı işlevleri. 

### <a name="basic-data-access-interfaces"></a>Temel veri erişim arabirimleri

##### <a name="long-cellid"></a>uzun CellID

Düğümün 64-bit kimliği. 

##### <a name="t-getfieldtstring-fieldname"></a>T GetField\<T\>(fieldName string)

Belirtilen özelliğin değerini alır. *T* alan olarak yorumlanması için beklenen istenen türü. İstenen türü örtük olarak alan türünden döndürülemezse otomatik tür atama denenecek. Not: özelliği olduğunda birden çok değerli *GetField\<dize\>*  listenin bir Json dizesi ["değ1", "değ2",...] serileştirilecek neden olur. Özellik mevcut değilse bir özel durum ve geçerli grafik araştırmayı durdur.

##### <a name="bool-containsfieldstring-fieldname"></a>bool ContainsField (dize fieldName)

Belirtilen ada sahip bir alan geçerli düğüm olup olmadığını söyler.

##### <a name="string-getstring-fieldname"></a>dize (dize fieldName) alın

Gibi çalışır *GetField\<dize\>(fieldName)* . Ancak, alanı bulunamazsa özel durum oluşturmaz, bunun yerine boş bir string("") döndürür.

##### <a name="bool-hasstring-fieldname"></a>bool (dize fieldName) sahiptir.

Sağlanan özellik geçerli düğüm olup olmadığını söyler. Aynı *ContainsField(fieldName)* .

##### <a name="bool-hasstring-fieldname-string-value"></a>bool (fieldName dize, dize değeri) sahip.

Özelliği, verilen değer olup olmadığını söyler. 

##### <a name="int-countstring-fieldname"></a>int sayısı (dize fieldname)

Verilen özellik değerlerinin sayısı alın. Özellik yok, 0 döndürür.

### <a name="built-in-helper-functions"></a>Yerleşik yardımcı işlevleri

##### <a name="action-returnifbool-condition"></a>Eylem return_if (bool koşul)

Döndürür *Action.Return* koşul ise *true*. Koşul ise *false* ve bu ifadeyi başka eylemler tarafından bit düzeyinde katılmamışsa- and işleci, grafik keşfi iptal edilecek.

##### <a name="action-continueifbool-condition"></a>Eylem continue_if (bool koşul)

Döndürür *Action.Continue* koşul ise *true*. Koşul ise *false* ve bu ifadeyi başka eylemler tarafından bit düzeyinde katılmamışsa- and işleci, grafik keşfi iptal edilecek.

##### <a name="bool-dicedouble-p"></a>bool dice (çift p)

Büyüktür veya eşittir 0,0 ile 1,0'den küçük rastgele bir sayı oluşturur. Bu işlev döndürür *true* sayı ya da eşit ise *p*.

İle karşılaştırıldığında *json* arama *lambda* arama daha ifadesel: C#Lambda ifadeleri, doğrudan sorgu desenleri belirtmek üzere kullanılabilir. İki örnek aşağıda verilmiştir.

```
MAG.StartFrom(@"{
    type  : ""ConferenceSeries"",
    match : {
        FullName : ""graph""
    }
}", new List<string>{ "FullName", "ShortName" })
.FollowEdge("ConferenceInstanceIDs")
.VisitNode(v => v.return_if(v.GetField<DateTime>("StartDate").ToString().Contains("2014")),
        new List<string>{ "FullName", "StartDate" })
```

```
MAG.StartFrom(@"{
    type  : ""Affiliation"",
    match : {
        Name : ""microsoft""
    }
}").FollowEdge("PaperIDs")
.VisitNode(v => v.return_if(v.get("NormalizedTitle").Contains("graph") || v.GetField<int>("CitationCount") > 100),
        new List<string>{ "OriginalTitle", "CitationCount" })
```
