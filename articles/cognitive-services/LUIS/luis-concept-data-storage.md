---
title: LUIS - Language Understanding veri depolama
titleSuffix: Azure Cognitive Services
description: Language Understanding (LUIS), verilerin depolanma şeklini öğrenin. LUIS anahtarı ile belirtilen bölgeyi karşılık gelen bir Azure veri deposunda şifrelenmiş verileri depolar.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: f876c4e279e723120794c550392512f5672ef91e
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2018
ms.locfileid: "49637654"
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