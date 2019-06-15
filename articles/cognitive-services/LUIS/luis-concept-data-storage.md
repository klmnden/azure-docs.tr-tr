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
ms.date: 03/28/2019
ms.author: diberry
ms.openlocfilehash: a1093c2a6303b453a17a52058303913de5ecfa8d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60812944"
---
# <a name="data-storage-and-removal-in-language-understanding-luis-cognitive-services"></a>Veri depolama ve Bilişsel hizmetler Language Understanding (LUIS) kaldırılması
LUIS anahtarı ile belirtilen bölgeyi karşılık gelen bir Azure veri deposunda şifrelenmiş verileri depolar. Bu veriler 30 gün boyunca saklanır. 

## <a name="export-and-delete-app"></a>Dışarı aktarma ve uygulamayı Sil
Kullanıcılar üzerinde tam denetime sahip [verme](luis-how-to-start-new-app.md#export-app) ve [silme](luis-how-to-start-new-app.md#delete-app) uygulama. 

## <a name="utterances"></a>Konuşmalar

Konuşma iki farklı yerde depolanabilir. 

* Sırasında **yazma işlemi**, konuşma oluşturulur ve amaca depolanır. Konuşma amacı, başarılı bir LUIS uygulaması için gereklidir. Uygulama yayımlanır ve son nokta isteğinin sorgu dizesi, uç noktasında sorguları alır sonra `log=false`, uç nokta utterance depolanıp depolanmayacağını belirler. Uç nokta depolanırsa, bulunan etkin olarak öğrenmeye konuşma bir parçası haline gelir **derleme** Portalı'nın bölümü, **gözden geçirin, konuşma uç noktası** bölümü. 
* Olduğunda, **konuşma uç noktası gözden**ve bir amaç için bir utterance ekleyin, utterance artık gözden geçirilmesi için uç nokta konuşma bir parçası olarak depolanır. Bu uygulamanın amaçları için eklenir. 

<a name="utterances-in-an-intent"></a>

### <a name="delete-example-utterances-from-an-intent"></a>Örnek konuşma bir amacından Sil
Eğitim için kullanılan örnek konuşma Sil [LUIS](luis-reference-regions.md). LUIS uygulamanızdan bir örnek utterance silerseniz, LUIS web hizmetinden kaldırılır ve dışarı aktarma için kullanılamaz.

<a name="utterances-in-review"></a>

### <a name="delete-utterances-in-review-from-active-learning"></a>Konuşma incelemesindeki gelen etkin olarak öğrenmeye Sil

Konuşma içinde LUIS önerir kullanıcı konuşma listesinden silebilirsiniz  **[gözden geçirme endpoint konuşma sayfası](luis-how-to-review-endpoint-utterances.md)** . Konuşma bu listeden silme önerilmesini engelliyor, ancak bunları günlüklerinden silmez.

Etkin öğrenme konuşma istemiyorsanız yapabilecekleriniz [etkin olarak öğrenmeye devre dışı](luis-how-to-review-endpoint-utterances.md#disable-active-learning). Etkin öğrenme devre dışı bırakma günlüğünü de devre dışı bırakır.

### <a name="disable-logging-utterances"></a>Günlüğe kaydetme konuşma devre dışı bırak
[Etkin öğrenme devre dışı bırakma](luis-how-to-review-endpoint-utterances.md#disable-active-learning) olan günlüğü devre dışı bırakır.


<a name="accounts"></a>

## <a name="delete-an-account"></a>Hesap silme
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
