---
title: "Azure’da bir zamanlamaya göre çalışan işlev oluşturma | Microsoft Docs"
description: "Azure’da nasıl tanımladığınız bir zamanlamaya göre çalışan bir işlev oluşturabileceğinizi öğrenin."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: ba50ee47-58e0-4972-b67b-828f2dc48701
ms.service: functions
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/02/2017
ms.author: glenga
ms.translationtype: Human Translation
ms.sourcegitcommit: fc4172b27b93a49c613eb915252895e845b96892
ms.openlocfilehash: 1478a2eedad496d3113fef28920d10859d11b1ce
ms.contentlocale: tr-tr
ms.lasthandoff: 05/12/2017


---
# <a name="create-a-function-in-azure-that-is-triggered-by-a-timer"></a>Azure’da bir zamanlayıcı tarafından tetiklenen bir işlev oluşturma

Azure İşlevleri’ni kullanarak nasıl tanımladığınız bir zamanlamaya göre çalışan bir işlev oluşturabileceğinizi öğrenin.

![Azure portalında işlev uygulaması oluşturma](./media/functions-create-scheduled-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Azure İşlev uygulaması oluşturma

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![İşlev uygulaması başarıyla oluşturuldu.](./media/functions-create-first-azure-function/function-app-create-success.png)

Ardından, yeni işlev uygulamasında bir işlev oluşturun.

<a name="create-function"></a>

## <a name="create-a-timer-triggered-function"></a>Zamanlayıcı ile tetiklenen işlev oluşturma

1. İşlev uygulamanızı genişletin, **İşlevler**'in yanındaki **+** düğmesine ve ardından tercih ettiğiniz dildeki **TimerTrigger** şablonuna tıklayın. Sonra, tabloda belirtilen ayarları kullanın ve **Oluştur**'a tıklayın:

| Ayar | Önerilen değer | Açıklama |
|---|---|---|
| **İşlevinizi adlandırın** | TimerTriggerCSharp1 | Zamanlayıcı ile tetiklenen işlevinizin adını tanımlar. |
| **[Zamanlama](http://en.wikipedia.org/wiki/Cron#CRON_expression)** | 0 \*/1 \* \* \* \* | İşlevinizi her dakika çalışacak şekilde zamanlayan altı haneli bir [CRON ifadesi](http://en.wikipedia.org/wiki/Cron#CRON_expression). |

Seçtiğiniz dilde her dakika çalışan bir işlev oluşturulur.

1. Günlüklere yazılan izleme bilgilerini görüntüleyerek yürütmeyi doğrulayın.

![Azure portalında İşlevler günlük görüntüleyicisi.](./media/functions-create-scheduled-function/functions-timer-trigger-view-logs2.png)

Şimdi, işlevin zamanlamasını saatte bir gibi daha az sıklıkta çalışacak şekilde değiştirebilirsiniz. 

## <a name="update-the-timer-schedule"></a>Zamanlayıcı zamanlamasını güncelleştirme

1. İşlevinizi genişletin ve **Tümleştir**’e tıklayın. Burada, işlevinizin giriş ve çıkış bağlamalarını tanımlamanın yanı sıra zamanlamayı da ayarlarsınız. 

2. `0 0 */1 * * *` şeklinde yeni bir **Zamanlama** değeri girin ve **Kaydet**’e tıklayın.  

![İşlevler Azure portalındaki zamanlayıcı zamanlamasını güncelleştirir.](./media/functions-create-scheduled-function/functions-timer-trigger-change-schedule.png)

Saatte bir çalışan bir işleviniz oldu. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bir zamanlamaya göre çalışan bir işlev oluşturdunuz.

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Zamanlayıcı tetikleyicileri hakkında daha fazla bilgi edinmek için bkz. [Azure İşlevleri ile kod yürütme zamanlama](functions-bindings-timer.md).
