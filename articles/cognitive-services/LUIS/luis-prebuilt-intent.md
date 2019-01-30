---
title: Önceden oluşturulmuş hedefleri
titleSuffix: Azure Cognitive Services
description: LUIS, bir dizi ortak, konuşma kullanıcı senaryoları hızlı bir şekilde eklemek için önceden oluşturulmuş hedefleri içerir.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 01/24/2019
ms.author: diberry
ms.openlocfilehash: abdbd0672fbb29f3936e4bda6f71e41c0f287329
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55221234"
---
# <a name="add-prebuilt-intents-for-common-intents"></a>Ortak hedefleri için önceden oluşturulmuş hedef ekleme 

LUIS, bir dizi ortak hedefleri ve konuşma hızlı bir şekilde eklemek için önceden oluşturulmuş etki alanlarının önceden oluşturulmuş hedefleri içerir. Bu yetenekler modellerini tasarlamak zorunda kalmadan yeteneklerini damıtarak konuşma bağlamında kullanılabilen istemci uygulamanıza eklemek için hızlı ve kolay bir yolu budur. 

## <a name="add-a-prebuilt-intent"></a>Önceden oluşturulmuş bir hedefi ekleyin

1. Üzerinde **uygulamalarım** sayfasında, uygulamanızı seçin. Uygulamanıza açılır **derleme** uygulama bölümü. 

1. Üzerinde **hedefleri** sayfasında **önceden oluşturulmuş hedefi ekleme** ıntents listenin üstündeki araç çubuğundan. 

1. Seçin **Utilities.Cancel** açılan iletişim kutusundan hedefi. 

    ![Önceden oluşturulmuş hedefi ekleyin](./media/luis-prebuilt-intents/prebuilt-intents-ddl.png)

1. Seçin **Bitti** düğmesi.

## <a name="train-and-test"></a>Eğitme ve test

1. Amaç eklendikten sonra uygulamayı seçerek eğitme **eğitme** üst, sağ araç çubuğu. 

1. Yeni hedefi seçerek test **Test** sağ araç çubuğunda. 

1. İptal etmek için konuşma metin kutusuna şunu girin:

    |Test utterance|Tahmin puanı|
    |--|:--|
    |My uçuş iptal etmek istiyorsunuz.|0.67|
    |Satın alma işlemini iptal edin.|0.52|
    |Toplantı iptal edin.|0.56|

    ![Önceden oluşturulmuş amacı test](./media/luis-prebuilt-intents/test.png)

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Önceden oluşturulmuş varlıklar](./luis-prebuilt-entities.md)
