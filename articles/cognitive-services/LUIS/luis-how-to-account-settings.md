---
title: Hesap ayarlarınızda LUIS yönetme | Microsoft Docs
description: Hesap ayarlarınızı yönetmek için LUIS Web sitesi kullanın.
titleSuffix: Azure
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/26/2018
ms.author: diberry
ms.openlocfilehash: bb41331228e700c55da21c627d617d16faa2dcb9
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52335409"
---
# <a name="manage-account-and-authoring-key"></a>Hesabı ve anahtarı yazma yönetme
İki anahtar bilgi HLUIShesabı için kullanıcı hesabı ve geliştirme anahtar parçalarıdır. Oturum açma bilgilerini, yönetilen [account.microsoft.com](https://account.microsoft.com). Yazma anahtarınızı yönetilen engelle [LUIS](luis-reference-regions.md) Web sitesi **ayarları** sayfası. 

## <a name="authoring-key"></a>Anahtar yazma

Bu tek ve bölgeye özgü yazma üzerinde anahtar **ayarları** sayfasında, tüm uygulamalarınızdan yazmanızı sağlar [LUIS](luis-reference-regions.md) Web sitesi hem de [yazma API'leri](https://aka.ms/luis-authoring-api). Kolaylık, geliştirme anahtar yapmak için izin verilen bir [sınırlı](luis-boundaries.md) uç nokta sayısı, her ay sorgular. 

[![LUIS Ayarları sayfası](./media/luis-how-to-account-settings/account-settings.png)](./media/luis-how-to-account-settings/account-settings.png#lightbox)

Yazma anahtar, sahip olduğunuz tüm uygulamaları ve bunun yanı sıra ortak çalışan listelenen tüm uygulamalar için kullanılır.

## <a name="authoring-key-regions"></a>Anahtar bölgeleri yazma
Yazma anahtar özeldir [yazma bölgesi](luis-reference-regions.md#publishing-regions). Anahtar, farklı bir bölgede çalışmaz. 

## <a name="reset-authoring-key"></a>Yazma anahtarını Sıfırla
Yazma anahtarınız açığa çıktıysa, anahtar sıfırlayın. Bu anahtar, tüm uygulamalarınızda sıfırlamak [LUIS](luis-reference-regions.md) Web sitesi. Uygulamalarınızı geliştirme API'leri aracılığıyla Yazar değerini değiştirmek gerekirse `Ocp-Apim-Subscription-Key` yeni anahtarı. 

## <a name="delete-account"></a>Hesabı sil
Bkz: [veri depolama ve Temizleme](luis-concept-data-storage.md#accounts) hesabınızı sildiğinizde, hangi verilerin silinir hakkında bilgi için. 

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin, [anahtar yazma](luis-concept-keys.md#authoring-key). 

