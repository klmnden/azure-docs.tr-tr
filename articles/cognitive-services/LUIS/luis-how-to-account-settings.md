---
title: Ayarları yönetme
titleSuffix: Language Understanding - Azure Cognitive Services
description: LUIS Web sitesi kullanıcı hesap ayarlarınızı ve tüm uygulamalarınız arasında kullanılan yazma anahtarını yönetmek için kullanın.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 12/07/2018
ms.author: diberry
ms.openlocfilehash: bd6ae88834b45e9e154eb1e5e3ba921f403c7eaa
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53138764"
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

