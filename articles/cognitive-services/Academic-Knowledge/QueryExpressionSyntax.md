---
title: Sorgu ifade sözdizimi akademik bilgi API'sindeki | Microsoft Docs
description: Akademik bilgi API'si Microsoft Bilişsel hizmetler için sorgu ifade sözdizimi kullanmayı öğrenin.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: 6ec338fff09954e2f14066ce2b83bc1228794af8
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351436"
---
# <a name="query-expression-syntax"></a>Sorgu ifade sözdizimi

Biz gördünüz yanıta bir **yorumlama** istek sorgu ifadesi içerir. Kullanıcının sorgu yorumlanan dilbilgisi her yorumlama için sorgu ifadesi oluşturulur. Bir sorgu ifadesi sonra kullanılabilir sorun için bir **değerlendirmek** varlık arama sonuçları almak için isteği.

Ayrıca kendi sorgu ifadeleri oluşturmak ve bunları kullanan bir **değerlendirmek** isteği. Bu, kullanıcının eylemlerine yanıt olarak bir sorgu ifadesi oluşturur, kendi kullanıcı arabirimi oluşturuluyorsa yararlı olabilir. Bunu yapmak için sorgu ifadeleri için söz dizimi bilmeniz gerekir.  

Sorgu ifadesinde bulunan her varlık özniteliği, belirli bir veri türüne ve olası sorgu işleçleri kümesi vardır. Varlık öznitelikleri ve her özniteliği için desteklenen işleçleri kümesi içinde belirtilen [varlık öznitelikleri](EntityAttributes.md). Tek değerli sorgu desteklemek için öznitelik gerektiriyor *eşittir* işlemi. Bir önek sorgu desteklemek için öznitelik gerektirir *StartsWith* işlemi. Sayısal aralığın sorguları gerektirir desteklemek için öznitelik *IsBetween* işlemi.

Bir nokta tarafından belirtildiği gibi bazı varlık veri bileşik öznitelikleri olarak depolanır '.' özniteliğinin adı. Örneğin, yazar/ilişkisi bilgileri bileşik bir öznitelik olarak temsil edilir. 4 bileşenleri içerir: AuN, AuId, AfN, AfId. Bu bileşenleri tek bir varlık öznitelik değeri form verileri ayrı parçalarıdır.


**Dize özniteliği: Tek bir değer** (eş anlamlıları karşı eşleşmeleri içerir)  
TI 'göre görünmeyen anlamsal analizi dizinini' =  
Bileşik (AA. AuN 'dumais yeniden' =)

**Dize özniteliği: Tam tek değer** (yalnızca kurallı değerlere eşleşir)  
TI 'göre görünmeyen anlamsal analizi dizinini' ==  
Bileşik (AA. AuN 'Çiğdem t dumais' ==)
     
**Dize özniteliği: Önek değeri**   
TI 'tarafından görünmeyen seman dizin' =...  
Bileşik (AA. AuN 'du yeniden' =...)

**Sayısal öznitelik: Tek değer**  
Y = 2010
 
**Sayısal öznitelik: Aralık değeri**  
Y &GT; 2005  
Y &GT; 2005 =  
Y &LT; 2010  
Y &LT; 2010 =  
Y =\[2010, 2012\) (yalnızca sol sınır değeri içerir: 2010, 2011)  
Y =\[2010, 2012\] (her iki sınır değerleri içerir: 2010, 2011, 2012)
 
**Sayısal öznitelik: Önek değeri**  
Y '19' =... (19 ile başlayan bir sayısal değer) 
 
**Tarihi özniteliği: Tek değer**  
D ='2010-02-04'

**Tarih öznitelik: Aralık değeri**  
D &GT;'2010-02-03'  
D = ['2010-02-03', ' 2010-02-05']

**Ve/veya sorgular:**  
Ve (Y = 1985, tı 'disordered elektronik sistemleri' =)  
Veya (tı 'disordered elektronik sistemleri' tı = 'hataya dayanıklılık ilkeleri ve uygulama' =)  
And(OR(Y=1985,Y=2008), tı 'disordered elektronik sistemleri' =)
 
**Bileşik sorgular:**  
Bileşik özniteliği sorgu bileşenine Composite() işlevi bileşik özniteliğine başvuruyor sorgu ifadesi parçası içine gerekir. 

Örneğin, raporlar için yazar adıyla sorgulamak için aşağıdaki sorguyu kullanın:
```
Composite(AA.AuN='mike smith')
```
<br>Belirli bir yazar tarafından yazıları için yazar belirli bir kurum etkinken sorgulamak için aşağıdaki sorguyu kullanın:
```
Composite(And(AA.AuN='mike smith',AA.AfN='harvard university'))
```
<br>Composite() işlevi bileşik öznitelik iki bölümden birbirine bağlar. Bu, biz yalnızca yazıları kendisinin Harvard etkinken yazarlar birini "Can Etikan" olduğu aldığını anlamına gelir. 

Yazıları belirli Yazar ilişkiler için belirli bir kurum (diğer) yazarların ile tarafından sorgulamak için aşağıdaki sorguyu kullanın:
```
And(Composite(AA.AuN='mike smith'),Composite(AA.AfN='harvard university'))
```
<br>Composite() yazar ve tek tek And() önce ilişkisi uygulandığından bu sürümde, biz burada yazarlar birini "Can Etikan" ve "Harvard" yazarların üyelikler, biri tüm incelemeler alın. Bu önceki sorgu örnektekine benzer sesleri, ancak aynı değil.

Genel olarak, aşağıdaki örnekte göz önünde bulundurun: A ve b iki bileşenden oluşur bileşik bir öznitelik C sahibiz Bir varlık birden çok değer C. olabilir Bizim varlıklar şunlardır:
```
E1: C={A=1, B=1}  C={A=1,B=2}  C={A=2,B=3}
E2: C={A=1, B=3}  C={A=3,B=2}
```

<br>Sorgu 
```
Composite(And(C.A=1, C.B=2))
```

<br>burada C.A bileşen 1 ve C.B bileşen 2 C için bir değere sahip varlıklar eşleşir. Bu sorgu yalnızca E1 eşleşir.

Sorgu 
```
And(Composite(C.A=1), Composite(C.B=2))
```
<br>C.A 1 olduğu için C bir değere sahip ve ayrıca bir değer için C C.B 2 olduğu varlıklar eşleşir. Bu sorgu E1 ve E2 eşleşmesi.

Lütfen unutmayın:  
- Bileşik bir öznitelik Composite() işlevi dışında bir parçası başvuramaz.
- Aynı Composite() işlevi içinde iki farklı bileşik öznitelikler bölümlerini başvuramaz.
- Bileşik bir öznitelik Composite() işlevi içine parçası olmayan bir öznitelik başvuramaz.
