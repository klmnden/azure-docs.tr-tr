---
title: Kağıt varlık öznitelikleri - akademik bilgi API'si
titlesuffix: Azure Cognitive Services
description: Akademik bilgi API'si kağıt varlıkta ile kullanabileceğiniz öznitelikleri hakkında bilgi edinin.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 03/31/2017
ms.author: alch
ms.openlocfilehash: c1f97896a8c3264fca0e76a0800731b8c6c85267
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48901610"
---
# <a name="paper-entity"></a>Kağıt varlık

<sub> * Aşağıdaki öznitelikleri kağıt varlığa özgüdür. (Ty = '0') </sub>


Ad    |Açıklama                                        |Tür       | İşlemler
------- | ------------------------------------------------- | --------- | ----------------------------
Kimlik      |Varlık Kimliği                                          |Int64      |Eşittir
Za      |Kağıt başlığı                                        |Dize     |Eşittir<br/>StartsWith
L       |Kağıt dil kodu çizgilerle "\@@@\"            |Dize     |Eşittir
E       |Kağıt yıl                                         |Int32      |Eşittir<br/>IsBetween
D       |İnceleme Tarihi                                         |Tarih       |Eşittir<br/>IsBetween
BİLGİ      |Alıntı sayısı                                     |Int32      |yok  
ECC     |Tahmini alıntı sayısı                           |Int32      |yok
AA. AuN  |Yazar adı                                        |Dize     |Eşittir<br/>StartsWith
AA. AuId |Yazar Kimliği                                          |Int64      |Eşittir
AA. AfN  |Yazar ilişkisi adı                            |Dize     |Eşittir<br/>StartsWith
AA. AfId |Yazar bağlantı kimliği                              |Int64      |Eşittir
AA. S    |Yazar sıra kağıt                         |Int32      |Eşittir
F.FN    |Örnek olay incelemesini ad alanı                                |Dize     |Eşittir<br/>StartsWith
F.FId   |Örnek olay incelemesini Kimliği alanı                                  |Int64      |Eşittir
J.JN    |Günlük adı                                       |Dize     |Eşittir<br/>StartsWith
J.JId   |Günlük kimliği                                         |Int64      |Eşittir
C.CN    |Konferans serisi adı                             |Dize     |Eşittir<br/>StartsWith
C.CId   |Konferans serisi kimliği                               |Int64      |Eşittir
RID     |Başvurulan incelemeler kimliği                              |Int64]    |Eşittir
W       |Kağıt başlık ve Özet sözcük                |String[]   |Eşittir
E       |Genişletilmiş meta verileri (aşağıdaki tabloya bakın)                |Dize     |yok  
        


## <a name="extended-metadata-attributes"></a>Genişletilmiş meta veri öznitelikleri ##

Ad    | Açıklama               
--------|---------------------------    
DN      | Kağıt görünen adı 
S       | Kaynaklar - statik sırasına göre sıralanmış kağıdın, web kaynaklarının listesi
S.Ty    | Kaynak türü (1:HTML 2:Text, 3:PDF, 4:DOC, 5:PPT, 6:XLS, 7:PS)
S.U     | Kaynak URL
VFN     | Mekan tam adı - günlük veya konferans tam adı
VSN'NİN     | Kısa ad mekan - günlük veya konferans kısa adı
V       | Toplu - günlük birim
BV      | Günlük adı
BT      | 
PB      | Günlük Abreviations
I       | Sorun - günlük sorun
FP      | FirstPage - ilk sayfayı
LP      | LastPage - kağıt son sayfa
DOI     | Dijital nesne tanımlayıcısı
BİLGİ      | Alıntı bağlamları – başvurulan kağıt listesi kimliğin ve karşılık gelen bağlamı yazıda (örn: [{123: ["kahverengi foxes bilinir kağıt 123 belirtildiği gibi atlamak için", "yavaş köpekler geçmiş misnomer kağıt 123 gösterildiği gibi"]})
IA      | Ters soyut
IA. IndexLength| (Soyut'ın sözcük sayısı) dizin içindeki öğelerin sayısı
IA. InvertedIndex| Soyut sözcükler ve karşılık gelen konumlarına özgün Özet listesi (örn: [{"": [0, 15, 30]}, {"kahverengi": [1]}, {"fox":[2]}])
