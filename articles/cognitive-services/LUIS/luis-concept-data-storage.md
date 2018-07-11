---
title: LUIS - Azure Depolama'da veri anlama | Microsoft Docs
description: Language Understanding (LUIS), verilerin depolanma şeklini öğrenin
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/08/2018
ms.author: v-geberr
ms.openlocfilehash: 515bcf4e06de562d1c83082824daa4dca0b54226
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37950524"
---
# <a name="data-storage-and-removal"></a>Veri depolama ve kaldırma
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