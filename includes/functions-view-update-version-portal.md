---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 11/26/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: d06bda1826964b019edb156375885c7f389ca6ec
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66131275"
---
Görüntülemek ve şu anda bir işlev uygulaması tarafından kullanılan çalışma zamanı sürümünü güncelleştirmek için aşağıdaki yordamı kullanın.

1. [Azure portal'da](https://portal.azure.com) işlev uygulamanızı bulun.

1. Altında **yapılandırılan özellikler**, seçin **işlev uygulaması ayarları**.

    ![İşlev uygulaması ayarlarını seçin](./media/functions-view-update-version-portal/add-update-app-setting.png)

1. **İşlev uygulaması ayarları** sekmesinde **Çalışma zamanı sürümü** bölümünü bulun. Burada ilgili çalışma zamanı sürümünü ve istenen ana sürümü görebilirsiniz. Aşağıdaki örnekte sürüm `~2` olarak ayarlanmıştır.

   ![İşlev uygulaması ayarlarını seçin](./media/functions-view-update-version-portal/function-app-view-version.png)

1. İşlev uygulamanızı 1.x çalışma zamanı sürümüne sabitlemek için **Çalışma zamanı sürümü** bölümünde **~1** seçeneğini belirleyin. Uygulamanızda işlevler varsa bu anahtar devre dışı bırakılır.

1. Çalışma zamanı sürümünü değiştirdiğinizde, geri dönüp **genel bakış** sekmesini **yeniden** uygulamayı yeniden başlatmanız.  İşlev uygulaması, 1.x çalışma zamanı sürümü üzerinde çalıştırılarak yeniden başlatılır ve işlev oluşturduğunuzda 1.x sürümüne ilişkin şablonlar kullanılır.