---
title: Veri depolama
titleSuffix: Language Understanding - Azure Cognitive Services
description: LUIS anahtarı ile belirtilen bölgeyi karşılık gelen bir Azure veri deposunda şifrelenmiş verileri depolar.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: diberry
ms.openlocfilehash: 05a2bd5334fe2836a96bd9437121252fe6ef6eff
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55882327"
---
# <a name="data-storage-and-removal-in-language-understanding-luis-cognitive-services"></a>Veri depolama ve Bilişsel hizmetler Language Understanding (LUIS) kaldırılması
LUIS anahtarı ile belirtilen bölgeyi karşılık gelen bir Azure veri deposunda şifrelenmiş verileri depolar. Bu veriler 30 gün boyunca saklanır. 

## <a name="export-and-delete-app"></a>Dışarı aktarma ve uygulamayı Sil
Kullanıcılar üzerinde tam denetime sahip [verme](luis-how-to-start-new-app.md#export-app) ve [silme](luis-how-to-start-new-app.md#delete-app) uygulama. 

## <a name="utterances-in-an-intent"></a>Bir hedefi olarak konuşma
Eğitim için kullanılan örnek konuşma Sil [LUIS](luis-reference-regions.md). LUIS uygulamanızdan bir örnek utterance silerseniz, LUIS web hizmetinden kaldırılır ve dışarı aktarma için kullanılamaz.

## <a name="utterances-in-review"></a>Konuşma incelemesindeki
Konuşma içinde LUIS önerir kullanıcı konuşma listesinden silebilirsiniz  **[gözden geçirme endpoint konuşma sayfası](luis-how-to-review-endoint-utt.md)**. Konuşma bu listeden silme önerilmesini engelliyor, ancak bunları günlüklerinden silmez.

## <a name="accounts"></a>Hesaplar
Bir hesabı silerseniz, tüm uygulamalar, kendi örnek konuşma ve günlükleri birlikte silinir. Hesabın önce 60 gün boyunca veriler korunur ve veriler kalıcı olarak silinir.

Hesap silme, kullanılabilir **ayarları** sayfası. Hesabınızın adını almak için sağ üst gezinti çubuğunda seçin **ayarları** sayfası.

## <a name="data-inactivity-as-an-expired-subscription"></a>Veri etkin olmama süresi dolmuş bir Aboneliğim olarak
Veri saklama ve silme amacı doğrultusunda, etkin olmayan bir LUIS uygulaması olabilir. _Microsoft'un takdirine bağlı olarak_ süresi dolmuş bir Aboneliğim kabul edilir. Uygulama Son 90 gün boyunca aşağıdaki ölçütleri karşılıyorsa etkin olmadığı varsayılır: 

* Oluşmuş **hiçbir** kendisine yapılan çağrılar.
* Değiştirilmedi.
* Değil sahip geçerli bir anahtar atanır.
* Oturum açma yapılmamış.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dışarı aktarma ve uygulama silme hakkında bilgi edinin](luis-how-to-start-new-app.md)
