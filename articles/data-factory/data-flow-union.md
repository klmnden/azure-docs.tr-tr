---
title: Yeni dal dönüştürme Azure veri fabrikası eşleme veri akışı
description: Yeni dal dönüştürme Azure veri fabrikası eşleme veri akışı
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: 1eebc879ad56ba4f35e6a8a1b857ae877a6a2f01
ms.sourcegitcommit: 235cd1c4f003a7f8459b9761a623f000dd9e50ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2019
ms.locfileid: "57726269"
---
# <a name="mapping-data-flow-union-transformation"></a>Veri akışı birleşim dönüştürme eşlemesi

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

UNION, tek SQL birleşim bu akışları birleşim dönüşümü yeni çıktısı olarak ile birden çok veri akışı birleştirecek. Tüm şema her giriş akışından bir birleştirme anahtarı gerek kalmadan, veri akışı içinde birleştirilebilir.

N-akış ayarları tablo sayısını, yapılandırılmış her satırın yanındaki "+" simgesini seçerek birleştirebilirsiniz.

![Birleşim dönüştürme](media/data-flow/union.png "birleşim")

Bu durumda, (Bu örnekte, üç farklı kaynak dosyaları) birden çok kaynaktan birbirinden farklı meta verileri birleştirmek ve bunları tek bir akışa birleştirir:

![Birleşim dönüşümüne genel bakış](media/data-flow/union111.png "birleşim 1")

Bunu başarmak için eklemek istediğiniz tüm kaynak dahil ederek birleşim ayarlarında ek satırlar ekleyin. Ortak bir arama ya da birleştirme anahtarı için gerek yoktur:

![Birleşim dönüştürme ayarlarını](media/data-flow/unionsettings.png "birleşim ayarları")

Sonra birleşim Select dönüştürme ayarlarsanız, çakışan alanlarını veya headerless kaynaklardan adlı olmayan alanları yeniden adlandırma mümkün olacaktır. Bu örnekte üç farklı kaynaklardan 132 toplam sütunlarla birleştirme meta verileri görmek için "inceleyin" tıklayın:

![Birleşim dönüştürme son](media/data-flow/union333.png "birleşim 3")
