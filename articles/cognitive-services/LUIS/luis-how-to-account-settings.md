---
title: Hesapları ve anahtarları yönetme
titleSuffix: Language Understanding - Azure Cognitive Services
description: İki anahtar bilgi HLUIShesabı için kullanıcı hesabı ve geliştirme anahtar parçalarıdır. Oturum açma bilgileriniz account.microsoft.com yönetilir. Yazma anahtarınızı LUIS portal ayarları sayfasından yönetilir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 03/11/2019
ms.author: diberry
ms.openlocfilehash: d7d63ad642ab2d3b336e15dcca606077762ceb9d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60195829"
---
# <a name="manage-account-and-authoring-key"></a>Hesabı ve anahtarı yazma yönetme

İki anahtar bilgi HLUIShesabı için kullanıcı hesabı ve geliştirme anahtar parçalarıdır. Oturum açma bilgilerini, yönetilen [account.microsoft.com](https://account.microsoft.com). Yazma anahtarınızı yönetilen engelle [LUIS](luis-reference-regions.md) portalı **ayarları** sayfası.

## <a name="authoring-key"></a>Anahtar yazma

Bu tek ve bölgeye özgü yazma üzerinde anahtar **ayarları** sayfasında, tüm uygulamalarınızdan yazmanızı sağlar [LUIS](luis-reference-regions.md) yanı portal [yazma API'leri](https://aka.ms/luis-authoring-api). Kolaylık, geliştirme anahtar yapmak için izin verilen bir [sınırlı](luis-boundaries.md) uç nokta sayısı, her ay sorgular.

[![LUIS Ayarları sayfası](./media/luis-how-to-account-settings/account-settings.png)](./media/luis-how-to-account-settings/account-settings.png#lightbox)

Yazma anahtar, sahip olduğunuz tüm uygulamaları ve bunun yanı sıra ortak çalışan listelenen tüm uygulamalar için kullanılır.

## <a name="authoring-key-regions"></a>Anahtar bölgeleri yazma

Yazma anahtar özeldir [yazma bölgesi](luis-reference-regions.md#publishing-regions). Anahtar, farklı bir bölgede çalışmaz.

## <a name="reset-authoring-key"></a>Yazma anahtarını Sıfırla

Yazma anahtarınız açığa çıktıysa, anahtar sıfırlayın. Anahtar, tüm uygulamalarınızda sıfırlamasına [LUIS](luis-reference-regions.md) portalı. Uygulamalarınızı geliştirme API'leri aracılığıyla Yazar değerini değiştirmek gerekirse `Ocp-Apim-Subscription-Key` yeni anahtarı.

## <a name="delete-account"></a>Hesabı sil

Bkz: [veri depolama ve Temizleme](luis-concept-data-storage.md#accounts) hesabınızı sildiğinizde, hangi verilerin silinir hakkında bilgi için.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin, [anahtar yazma](luis-concept-keys.md#authoring-key).

