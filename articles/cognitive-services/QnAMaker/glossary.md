---
title: Sözlük - soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu hizmetini, makine öğreniminin birçok yeni hüküm ve doğal dil işleme yanı sıra hizmete özgü şartları vardır. Bu liste, bu terimleri anlamanıza yardımcı olur.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 01/14/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: e86ea22208ccd08c0268fda0cc47d5127b4568a1
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55226477"
---
# <a name="glossary-for-qna-maker-knowledge-base-and-service"></a>Soru-cevap Oluşturucu Bilgi Bankası ve hizmet için terimler sözlüğü

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
