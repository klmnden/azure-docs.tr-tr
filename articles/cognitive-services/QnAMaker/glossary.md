---
title: Sözlük - soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: Sözlük
services: cognitive-services
author: nstulasi
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: nstulasi
ms.openlocfilehash: e4c0af68911ed25d4c07ef2050d0b74c435488ab
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45540807"
---
# <a name="glossary"></a>Sözlük

## <a name="qna-maker-service"></a>Soru-cevap Oluşturucu hizmeti
Soru-cevap Oluşturucu hizmetini soru-cevap Oluşturucu'ı kullanmaya başlamak için bir önkoşuldur. Soru-cevap Oluşturucu katman satın alma oluşturmak ve bilgi bankanızı yönetmek için Azure aboneliğinizdeki kaynaklar ayarlar. Soru-cevap Oluşturucu her kullanıcı hesabı, Azure aboneliğinde birden çok soru-cevap Oluşturucu hizmeti oluşturabilirsiniz.

## <a name="knowledge-base"></a>Bilgi Bankası
Bilgi Bankası, sorular deposudur ve soru-cevap Oluşturucu kullanılan oluşturulan ve tutulan yanıtlar. Her bir soru-cevap Oluşturucu katman birden çok bilgi bankaları için kullanılabilir.

## <a name="endpoint"></a>Uç Nokta
Uygulamanıza sık sohbet Robotu tümleştirilebilir, Bilgi Bankası içeriğinizi hizmet bir REST tabanlı bir HTTP uç noktası. 

## <a name="test-knowledge-base"></a>Bilgi Bankası test
Bilgi Bankası iki durumlu - Test sahiptir ve yayımladınız. Test Bilgi Bankası düzenlenen, kaydedilmiş ve için test edilmiş doğruluk ve yanıtları tamlığını yüklenmekte olan sürümüdür. Test Bilgi Bankası'na yapılan değişiklikler, uygulama/sohbet Robotu son kullanıcısına etkilemez.

## <a name="published-knowledge-base"></a>Yayımlanan Bilgi Bankası
Bilgi Bankası - Test ve yayımlanan iki durum vardır.  Yayımlanan Bilgi Bankası sohbet robotunuzun/uygulamanızın içinde kullanılan sürümüdür. Bilgi Bankası Yayımlama eylemi Bilgi Bankası yayımlanan sürümünü Test Bilgi Bankası içeriği yerleştirir. Yayımlanan Bilgi Bankası uç noktası aracılığıyla uygulamanın kullandığı sürüm olduğundan, içeriği doğru ve test edilmiş iyi olduğundan emin olmak için dikkatli olunması.

## <a name="query"></a>Sorgu
Son kullanıcı veya test edici soran soru Bilgi Bankası'nın bir kullanıcı sorgudur. Sorgu, doğal dil biçimi veya soru temsil eden birkaç anahtar sözcükleri genellikle gereklidir.

## <a name="response"></a>Yanıt
Yanıt, belirtilen kullanıcı sorgusu için en iyi eşleşmeyi temel Bilgi Bankası alınan yanıt.

## <a name="confidence-score"></a>Güvenilirlik Puanı
Güvenilirlik puanı yanıtın 0 ve 100, 100 kullanıcı sorgusu ve soru-yanıt sunulan Bilgi Bankası arasında bir tam sorgu eşleşme olan arasında sayısal bir değer belirtilen kullanıcı sorgusu için doğru uygun yanıt şeklindedir. Yanıtları genellikle güvenilirlik puanı göre sıralanır ve daha yüksek bir güven puanıyla birlikte bir tane varsayılan yanıt sunulur.
