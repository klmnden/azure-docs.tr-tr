---
title: Sorgu ifadesi söz dizimi - akademik bilgi API'si
titlesuffix: Azure Cognitive Services
description: Sorgu ifadesi söz dizimi içinde akademik bilgi API'sini kullanmayı öğrenin.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: 95c2e9d3f54f967b3ebb434c742f69251b80573e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61336765"
---
# <a name="query-expression-syntax"></a>Sorgu ifadesi söz dizimi

Gördük yanıt olarak bir **yorumlama** istek sorgu ifadesi içerir. Kullanıcının sorgu yorumlanan dilbilgisi bir sorgu ifadesi her yorumu için oluşturuldu. Bir sorgu ifadesinde, kullanılabilir verilecek bir **değerlendirmek** varlık arama sonuçları almak için isteği.

Ayrıca kendi sorgu ifadeleri oluşturmak ve bunları kullanmak bir **değerlendirmek** isteği. Bu, kendi kullanıcı arabirimi yanıt kullanıcının eylemleri için bir sorgu ifadesinde oluşturan oluşturuyorsanız yararlı olabilir. Bunu yapmak için sorgu ifade sözdizimi bilmeniz gerekir.  

Bir sorgu ifadesine dahil her varlık öznitelik, bir özel veri türü ve olası sorgu işleçleri kümesini sahiptir. Varlık öznitelikleri ve her bir öznitelik için desteklenen işleçleri kümesini belirtilen [varlık öznitelikleri](EntityAttributes.md). Destek için özniteliği tek değerli sorgu gerektirir *eşittir* işlemi. Bir ön ek sorgu desteklemek için öznitelik gerektirir *StartsWith* işlemi. Sayısal Aralık sorguları desteklemek için öznitelik gerektirir *IsBetween* işlemi.

Bazı varlık veri noktayla belirtildiği gibi bileşik öznitelikleri depolanır '.' öznitelik adı. Örneğin, yazar/bağlantı bilgileri, bileşik bir özniteliği olarak temsil edilir. 4 bileşenleri içerir: AuN, AuId, AfN, AfId. Bu bileşenleri tek bir varlık öznitelik değeri form verileri ayrı parçalarıdır.


**Dize özniteliği: Tek değer** (eş anlamlılar karşı eşleşmeleri içerir)  
Za 'görünmeyen bir anlam analizi tarafından dizin oluşturma' =  
Bileşik (AA. AuN 'dumais Oya' =)

**Dize özniteliği: Tek değer tam** (yalnızca kurallı değerlere eşleşir)  
Za 'görünmeyen bir anlam analizi tarafından dizin oluşturma' ==  
Bileşik (AA. AuN 'susan t dumais' ==)
     
**Dize özniteliği: Önek değeri**   
Za 'görünmeyen seman tarafından dizin oluşturma' =...  
Bileşik (AA. AuN 'du Oya' =...)

**Sayısal özniteliği: Tek değer**  
Y=2010
 
**Sayısal özniteliği: Aralık değeri**  
Y>2005  
Y &GT; 2005 =  
Y<2010  
Y &LT; 2010 =  
Y =\[2010, 2012\) (yalnızca sol sınır değeri içerir: 2010, 2011)  
Y =\[2010, 2012\] (her iki sınır değerleri içerir: 2010, 2011, 2012)
 
**Sayısal özniteliği: Önek değeri**  
Y = '19'... (19 ile başlayan herhangi bir sayısal değer) 
 
**Tarih özniteliği: Tek değer**  
D ='2010-02-04'

**Tarih özniteliği: Aralık değeri**  
D &GT;'2010-02-03'  
D = ['2010-02-03', ' 2010-02-05']

**Ve/veya sorgular:**  
Ve (Y = 1985, ZA = 'disordered elektronik sistemleri')  
Veya (Za 'disordered elektronik sistemleri' Ti = 'hata toleransı ilkeleri ve uygulama' =)  
And(OR(Y=1985,Y=2008), ZA = 'disordered elektronik sistemleri')
 
**Bileşik sorgular:**  
Bileşik bir öznitelik sorgu bileşenine Composite() işlevi bileşik özniteliğine başvuruyor sorgu ifadesinin parçası içine gerekir. 

Örneğin, incelemeler için yazar adına göre sorgulamak için aşağıdaki sorguyu kullanın:
```
Composite(AA.AuN='mike smith')
```
<br>Belirli bir yazar tarafından İnceleme yazar, belirli bir kurum ederken sorgulamak için aşağıdaki sorguyu kullanın:
```
Composite(And(AA.AuN='mike smith',AA.AfN='harvard university'))
```
<br>Bileşik öznitelik iki bölümden bağdaştırabilir Composite() işlevi. Bu, biz yalnızca incelemeler o Harvard ederken biri yazarlar, "Mike Smith" olduğu aldığını anlamına gelir. 

İlişkiler, belirli bir kuruluşa (diğer) yazarların ile belirli bir yazar tarafından incelemeler sorgulamak için aşağıdaki sorguyu kullanın:
```
And(Composite(AA.AuN='mike smith'),Composite(AA.AfN='harvard university'))
```
<br>Bu sürümde, yazar ve ayrı ayrı And() önce ilişkisi Composite() uygulandığından burada yazarlar, "Mike Smith" biridir ve yazarların ilişkiler biri "Harvard" olan tüm raporlar aldığımız. Bu sorgu önceki örneğe benzer görünüyor, ancak aynı şey değildir.

Genel olarak, aşağıdaki örneği göz önünde bulundurun: A ve b iki bileşenden oluşur bileşik bir öznitelik C sahibiz. Bir varlık birden çok değer C. olabilir. Bizim varlıkları şunlardır:
```
E1: C={A=1, B=1}  C={A=1,B=2}  C={A=2,B=3}
E2: C={A=1, B=3}  C={A=3,B=2}
```

<br>Sorgu 
```
Composite(And(C.A=1, C.B=2))
```

<br>burada C.A bileşen 1, 2 C.B bileşeni ise C için bir değere sahip varlıklar eşleşir. Bu sorgu yalnızca E1 eşleşir.

Sorgu 
```
And(Composite(C.A=1), Composite(C.B=2))
```
<br>bir değer için C C.A 1 olduğu ve burada C.B 2, C için de bir değere sahip varlıklar eşleşir. Bu sorgu hem E1 ve E2 eşleştirin.

Lütfen unutmayın:  
- Bileşik bir öznitelik Composite() işlevi dışında bir parçası başvuramaz.
- Aynı Composite() işlevin içindeki iki farklı bileşik öznitelikleri bölümlerini başvuramaz.
- Bileşik bir öznitelik Composite() işlevi içinde bir parçası olmayan bir öznitelik başvuramaz.
