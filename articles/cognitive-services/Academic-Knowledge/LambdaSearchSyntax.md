---
title: Akademik bilgi API'si lambda arama söz dizimi | Microsoft Docs
description: Akademik bilgi API'si Microsoft Bilişsel Hizmetleri'ndeki kullanabileceğiniz Lambda arama söz dizimi hakkında bilgi edinin.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: f486368e1d0258087091acb846a84b125712db40
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351479"
---
# <a name="lambda-search-syntax"></a>Lambda arama söz dizimi

Her *lambda* arama sorgu dizesi bir grafik deseni açıklar. Bir sorgu hangi grafik düğümden biz geçişi başlatmak belirtme en az bir başlangıç düğümü olmalıdır. Bir başlangıç düğümü belirtmek için arama *MANYETİK. StartFrom()* yöntemi ve bir veya daha fazla düğümde veya bir sorgu Kimlikleri'ni geçişinde nesnesi arama kısıtlamaları belirtir. *StartFrom()* yöntemi üç aşırı sahiptir. Bunların tümünün ikinci olan ile iki bağımsız değişken isteğe bağlı olur. İlk bağımsız değişken uzun tamsayı, uzun tamsayı numaralandırılabilir bir koleksiyonunu olabilir veya nesne JSON temsil eden bir dize olarak aynı semantiği ile *json* ara:
```
StartFrom(long cellid, IEnumerable<string> select = null)
StartFrom(IEnumerable<long> cellid, IEnumerable<string> select = null)
StartFrom(string queryObject, IEnumerable<string> select = null)
```

Bir düğümden diğerine yol için bir lambda arama sorgusu yazma işlemi gerçekleşir. Kenarın size yol türünü belirtmek için kullanın *FollowEdge()* ve istenen kenara türlerinde geçirin. *FollowEdge()* rastgele sayıda dize bağımsız değişkeni alır:
```
FollowEdge(params string[] edgeTypes)
```
> [!NOTE]
> Biz izlemek için edge(s) türleri hakkında önemli değil, yalnızca sütunsa *FollowEdge()* iki düğüm arasında: Sorgu bu iki düğüm arasında olası tüm kenarları boyunca size yol gösterir.

Aracılığıyla bir düğüme üzerinde gerçekleştirilecek geçişi eylemleri belirttiğimiz *VisitNode()*, diğer bir deyişle, bu düğümde durdurmak ve sonuç olarak geçerli yolda dönmek için mi grafiği keşfetmeye devam etmek için.  Enum türü *eylem* iki tür eylem tanımlar: *Action.Return* ve *Action.Continue*. Bu tür bir enum değeri doğrudan geçiş yapabilir *VisitNode()*, bit düzeyinde ile birleştirerek- and işleci '&'. İki eylem birleştirilir, her iki eylemlerin gerçekleştirilmesi anlamına gelir. Not: Bitsel kullanmayın- or işleci ' |' eylemleri. Bunun yapılması, herhangi bir şey dönmeden sonlandırmak sorgu neden olur. Atlanıyor *VisitNode()* iki arasında *FollowEdge()* çağrıları koşulsuz olarak grafiği bir düğümde ulaşan sonra keşfetmek sorgu neden olur.

```
VisitNode(Action action, IEnumerable<string> select = null)
```

İçin *VisitNode()*, biz de türünde lambda ifadesinde geçirebilirsiniz *ifade\<Func\<Inode, eylemi\>\>*, bir aldığı*Inode* ve çapraz geçişi eylem döndürür:

```
VisitNode(Expression<Func<INode, Action>> action, IEnumerable<string> select = null)
```

## <a name="inode"></a>*Inode* 

*Inode* sağlar *salt okunur* veri erişim arabirimleri ve bir düğüm üzerindeki birkaç yerleşik yardımcı işlevleri. 

### <a name="basic-data-access-interfaces"></a>Temel veri erişim arabirimleri

##### <a name="long-cellid"></a>uzun CellID

Düğümün 64-bit kimliği. 

##### <a name="t-getfieldtstring-fieldname"></a>T GetField\<T\>(fieldName dize)

Belirtilen özellik değerini alır. *T* alan yorumlanan beklenir istenen tür. İstenen türü örtük olarak alan türünden dönüştürülemiyorsa otomatik tür atama denenir. Not: özellik olduğunda birden çok değerli *GetField\<dize\>*  bir Json dizesinde ["değer1", "değer2",...] serileştirilmesi listeye neden olur. Özellik mevcut değilse, bir özel durum ve geçerli grafik araştırması iptal.

##### <a name="bool-containsfieldstring-fieldname"></a>bool ContainsField (dize fieldName)

Verilen ada sahip bir alan geçerli düğümdeki var olup olmadığını bildirir.

##### <a name="string-getstring-fieldname"></a>dize (dize fieldName) Al

Gibi çalışır *GetField\<dize\>(fieldName)*. Ancak, alanın bulunamadığında özel durumlar oluşturmadığını, bunun yerine boş bir string("") döndürür.

##### <a name="bool-hasstring-fieldname"></a>bool (dize fieldName) sahip

Sağlanan özellik geçerli düğümdeki var olup olmadığını bildirir. Aynı *ContainsField(fieldName)*.

##### <a name="bool-hasstring-fieldname-string-value"></a>bool (dize fieldName, dize değeri) sahip

Özellik verilen değer olup olmadığını bildirir. 

##### <a name="int-countstring-fieldname"></a>int sayısı (dize fieldname)

Belirli bir özellik değerlerini sayısını alın. Özellik yok, 0 döndürür.

### <a name="built-in-helper-functions"></a>Yerleşik yardımcı işlevleri

##### <a name="action-returnifbool-condition"></a>Eylem return_if (bool koşul)

Döndürür *Action.Return* koşul ise *doğru*. Koşul ise *false* ve bu ifade başka eylemler bit tarafından katılmamışsa- and işleci, grafik araştırması iptal edilecek.

##### <a name="action-continueifbool-condition"></a>Eylem continue_if (bool koşul)

Döndürür *Action.Continue* koşul ise *doğru*. Koşul ise *false* ve bu ifade başka eylemler bit tarafından katılmamışsa- and işleci, grafik araştırması iptal edilecek.

##### <a name="bool-dicedouble-p"></a>bool bölmek (çift p)

Büyük veya eşit 0,0 ile 1,0'den küçük rastgele bir sayı oluşturur. Bu işlev, döndürür *true* sayı küçük veya eşit ise *p*.

İle karşılaştırılan *json* arama *lambda* arama daha açıklayıcı: C# lambda ifadeleri doğrudan sorgu desenlerine belirtmek için kullanılabilir. Aşağıda, iki örnek verilmiştir.

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
