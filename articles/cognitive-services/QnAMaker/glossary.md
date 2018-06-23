---
title: Sözlük - Microsoft Bilişsel hizmetler | Microsoft Docs
titleSuffix: Azure
description: Sözlük
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 04/21/2018
ms.author: saneppal
ms.openlocfilehash: e28cddec005cb6ba99b9f60d8b03a11f1bc97062
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354107"
---
# <a name="glossary"></a>Sözlük

## <a name="qna-maker-service"></a>QnA Maker hizmeti
QnA Maker'ı kullanmaya başlamak için bir önkoşul QnA Maker hizmetidir. QnA Maker katmanı satın oluşturmak ve Bilgi Bankası yönetmek için Azure aboneliğinizin kaynaklarında ayarlar. Her QnA Maker kullanıcı hesabı birden fazla QnA Maker hizmet Azure aboneliğini oluşturabilirsiniz.

## <a name="knowledge-base"></a>Bilgi Bankası
Bilgi Bankası soruları deposudur ve oluşturulan, saklanır ve QnA Maker kullanılan yanıtlar. Her QnA Maker katmanı birden çok Bilgi Bankası için kullanılabilir.

## <a name="endpoint"></a>Uç Nokta
Bilgi Bankası içeriğinizi uygulamanıza sık sohbet bot tümleştirilebilir bakım REST tabanlı HTTP uç noktası. 

## <a name="test-knowledge-base"></a>Bilgi Bankası test
Bilgi Bankası iki durumlu - Test sahiptir ve yayımlanır. Test Bilgi Bankası düzenlenen, kaydedilmiş ve için test edilmiş doğruluğunu ve yanıtları bütünlüğü yüklenmekte olan sürümüdür. Test Bilgi Bankası'na yapılan değişiklikler, uygulama/sohbet bot, son kullanıcı etkilemez.

## <a name="published-knowledge-base"></a>Yayımlanan Bilgi Bankası
Bilgi Bankası iki durumlu - Test ve yayımlanan sahiptir.  Yayımlanan bilgi bankası, sohbet bot/uygulamanızda kullanılan sürümüdür. Bilgi Bankası yayımlama işlemi içeriği Test Bilgi Bankası, Bilgi Bankası yayımlanan sürümünü koyar. Yayımlanan Bilgi Bankası uygulama uç noktası aracılığıyla kullanır sürüm olduğundan, içeriği doğru olduğunu ve iyi test edilmiş olduğundan emin olmak için dikkatli olunmalıdır.

## <a name="query"></a>Sorgu
Kullanıcı sorgusu son kullanıcı veya tester ister soru Bilgi Bankası ' dir. Sorgu bir doğal dil veya soru temsil eden birkaç anahtar sözcükler biçiminde görülür.

## <a name="response"></a>Yanıt
Verilen kullanıcı sorgu için en iyi eşleşmeyi göre Bilgi Bankası kaynağından alınan yanıt cevaptır.

## <a name="confidence-score"></a>Güvenilirlik Puanı
Yanıtın güvenirlik puanı 0 ve 100, kullanıcı sorgu ve yanıt sunulan Bilgi Bankası'nda bir soru arasında bir tam sorgu eşleşme olan 100 arasında bir sayısal değer verilen kullanıcı sorgu için doğru uygun yanıtı numarasıdır. Yanıtları genellikle güvenirlik puana göre sıralanır ve yüksek güven puanı ile bir varsayılan yanıt olarak sunulur.
