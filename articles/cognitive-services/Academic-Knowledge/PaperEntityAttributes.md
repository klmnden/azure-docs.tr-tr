---
title: Kağıt varlık öznitelikleri - akademik bilgi API'si
titlesuffix: Azure Cognitive Services
description: Akademik bilgi API'si kağıt varlıkta ile kullanabileceğiniz öznitelikleri hakkında bilgi edinin.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/31/2017
ms.author: alch
ms.openlocfilehash: 92844b5faf691b67617c9f3424a1322aa05429bb
ms.sourcegitcommit: 2c09af866f6cc3b2169e84100daea0aac9fc7fd0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64875746"
---
# <a name="paper-entity"></a>Kağıt varlık

<sub> * Aşağıdaki öznitelikleri kağıt varlığa özgüdür. (Ty = '0') </sub>


Ad    |Açıklama                                        |Tür       | İşlemler
------- | ------------------------------------------------- | --------- | ----------------------------
Kimlik      |Varlık Kimliği                                          |Int64      |Eşittir
Za      |Kağıt başlığı                                        |String     |Eşittir<br/>StartsWith
L       |Kağıt dil kodu ayırarak "\@\@\@"          |String     |Eşittir
E       |Kağıt yıl                                         |Int32      |Eşittir<br/>IsBetween
D       |İnceleme Tarihi                                         |Tarih       |Eşittir<br/>IsBetween
BİLGİ      |Alıntı sayısı                                     |Int32      |yok  
ECC     |Tahmini alıntı sayısı                           |Int32      |yok
AA.AuN  |Yazar adı                                        |String     |Eşittir<br/>StartsWith
AA.AuId |Yazar Kimliği                                          |Int64      |Eşittir
AA.AfN  |Yazar ilişkisi adı                            |String     |Eşittir<br/>StartsWith
AA.AfId |Yazar bağlantı kimliği                              |Int64      |Eşittir
AA. S    |Yazar sıra kağıt                         |Int32      |Eşittir
F.FN    |Örnek olay incelemesini ad alanı                                |String     |Eşittir<br/>StartsWith
F.FId   |Örnek olay incelemesini Kimliği alanı                                  |Int64      |Eşittir
J.JN    |Günlük adı                                       |String     |Eşittir<br/>StartsWith
J.JId   |Günlük kimliği                                         |Int64      |Eşittir
C.CN    |Konferans serisi adı                             |String     |Eşittir<br/>StartsWith
C.CId   |Konferans serisi kimliği                               |Int64      |Eşittir
RId     |Başvurulan incelemeler kimliği                              |Int64[]    |Eşittir
W       |Kağıt başlık ve Özet sözcük                |String[]   |Eşittir
E       |Genişletilmiş meta verileri (aşağıdaki tabloya bakın)                |String     |yok  
        


## <a name="extended-metadata-attributes"></a>Genişletilmiş meta veri öznitelikleri ##

Ad    | Açıklama               
--------|---------------------------    
DN      | Kağıt görünen adı 
S       | Kaynaklar - statik sırasına göre sıralanmış kağıdın, web kaynaklarının listesi
S.Ty    | Kaynak türü (1:HTML 2:Text, 3:PDF, 4:DOC, 5:PPT, 6:XLS, 7:PS)
S.U     | Kaynak URL
VFN     | Mekan tam adı - günlük veya konferans tam adı
VSN     | Kısa ad mekan - günlük veya konferans kısa adı
V       | Toplu - günlük birim
BV      | Günlük adı
BT      | 
PB      | Günlük kısaltmaları
I       | Sorun - günlük sorun
FP      | FirstPage - ilk sayfayı
LP      | LastPage - kağıt son sayfa
DOI     | Dijital nesne tanımlayıcısı
BİLGİ      | Alıntı bağlamları – başvurulan kağıt listesi kimliğin ve karşılık gelen bağlamı yazıda (örn: [{123: ["kahverengi foxes bilinir kağıt 123 belirtildiği gibi atlamak için", "yavaş köpekler geçmiş misnomer kağıt 123 gösterildiği gibi"]})
IA      | Ters soyut
IA. IndexLength| (Soyut'ın sözcük sayısı) dizin içindeki öğelerin sayısı
IA. InvertedIndex| Soyut sözcükler ve karşılık gelen konumlarına özgün Özet listesi (örn: [{"": [0, 15, 30]}, {"kahverengi": [1]}, {"fox":[2]}])
