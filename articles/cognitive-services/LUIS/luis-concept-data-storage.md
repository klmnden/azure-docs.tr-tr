---
title: HALUK - Azure veri depolama anlama | Microsoft Docs
description: Veri dili anlama (HALUK) içinde nasıl depolandığını öğrenin
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/08/2018
ms.author: v-geberr
ms.openlocfilehash: f235c787e7d2064696e5421219a297d914b5882d
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266014"
---
# <a name="data-storage-and-removal"></a>Veri depolama ve kaldırma
HALUK anahtarı tarafından belirtilen bölgeyi karşılık gelen bir Azure veri deposunda şifrelenmiş verileri depolar. Bu veriler 30 gün süreyle depolanır. 

## <a name="export-and-delete-app"></a>Dışarı aktarma ve uygulamayı Sil
Kullanıcıların üzerinde tam denetime sahip [dışarı aktarma](create-new-app.md#export-app) ve [silme](create-new-app.md#delete-app) uygulama. 

## <a name="utterances-in-an-intent"></a>Amacına utterances
Eğitim için kullanılan örnek utterances silmek [HALUK][LUIS]. HALUK uygulamanızdan bir örnek utterance silerseniz, HALUK web hizmetinden kaldırılır ve dışarı aktarma için kullanılamıyor.

## <a name="utterances-in-review"></a>Gözden geçirme utterances
İçinde HALUK önerir kullanıcı utterances listesinden utterances silebilirsiniz  **[İnceleme uç nokta utterances sayfasında](label-suggested-utterances.md)**. Bu listeden utterances silme önerilmesini engeller, ancak bunları günlüklerinden silmez.

## <a name="accounts"></a>Hesaplar
Bir hesap silerseniz, tüm uygulamaları, kendi örnek utterances ve günlükleri birlikte silinir. Hesap önce 60 gün için veriler korunur ve veriler kalıcı olarak silinir.

Hesap silme edinilebilir **ayarları** sayfası. Hesabınızın adını almak için sağ üst gezinti çubuğundaki seçin **ayarları** sayfası.

## <a name="data-inactivity-as-an-expired-subscription"></a>Veri etkin olmama süresi dolmuş bir abonelik olarak
Veri saklama ve silinmesini amaçları doğrultusunda, en etkin olmayan HALUK uygulama olabilir. _Microsoft'un tedbirli_ kabul süresi dolmuş bir abonelik. Bir uygulama için Son 90 gündeki aşağıdaki ölçütleri karşılaması durumunda etkin değil olarak kabul edilir: 

* Dolmadığı **hiçbir** üzerinde yapılan çağrıları.
* Değiştirilmedi.
* Değil sahip geçerli bir anahtar atanır.
* Oturum açma yapılmamış.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dışarı aktarma ve uygulama silme hakkında bilgi edinin](create-new-app.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions