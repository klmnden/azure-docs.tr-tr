---
title: Azure’da bir zamanlamaya göre çalışan işlev oluşturma | Microsoft Docs
description: Azure’da nasıl tanımladığınız bir zamanlamaya göre çalışan bir işlev oluşturabileceğinizi öğrenin.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: ''
tags: ''
ms.assetid: ba50ee47-58e0-4972-b67b-828f2dc48701
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/28/2018
ms.author: glenga
ms.custom: mvc, cc996988-fb4f-47
ms.openlocfilehash: 6dc5d494135fde3740d41453f3f484b49fcb3f80
ms.sourcegitcommit: cfff72e240193b5a802532de12651162c31778b6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39308669"
---
# <a name="create-a-function-in-azure-that-is-triggered-by-a-timer"></a>Azure’da bir zamanlayıcı tarafından tetiklenen bir işlev oluşturma

Azure İşlevleri’ni kullanarak, tanımladığınız bir zamanlamaya göre çalışan bir [sunucusuz](https://azure.microsoft.com/overview/serverless-computing/) işlev oluşturma hakkında bilgi edinin.

![Azure portalında işlev uygulaması oluşturma](./media/functions-create-scheduled-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

+ Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-an-azure-function-app"></a>Azure İşlev uygulaması oluşturma

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![İşlev uygulaması başarıyla oluşturuldu.](./media/functions-create-first-azure-function/function-app-create-success.png)

Ardından, yeni işlev uygulamasında bir işlev oluşturun.

<a name="create-function"></a>

## <a name="create-a-timer-triggered-function"></a>Zamanlayıcı ile tetiklenen işlev oluşturma

1. İşlev uygulamanızı genişletin ve **İşlevler**'in yanındaki **+** düğmesine tıklayın. Bu, işlev uygulamanızdaki ilk işlevse **Özel işlev**'i seçin. Böylece işlev şablonlarının tamamı görüntülenir.

    ![Azure portalındaki İşlevler hızlı başlangıç sayfası](./media/functions-create-scheduled-function/add-first-function.png)

2. Arama alanına `timer` yazıp zamanlayıcı tetikleyici şablonunuz için istediğiniz dili seçin. 

    ![Zamanlayıcı ile tetiklenen işlev şablonunu seçin.](./media/functions-create-scheduled-function/functions-create-timer-trigger.png)

3. Yeni tetikleyiciyi resmin altındaki tabloda belirtilen ayarlarla yapılandırın.

    ![Azure portalında zamanlayıcı tarafından tetiklenen bir işlev oluşturun.](./media/functions-create-scheduled-function/functions-create-timer-trigger-2.png)

    | Ayar | Önerilen değer | Açıklama |
    |---|---|---|
    | **Ad** | Varsayılan | Zamanlayıcı ile tetiklenen işlevinizin adını tanımlar. |
    | **Zamanlama** | 0 \*/1 \* \* \* \* | İşlevinizi her dakika çalışacak şekilde zamanlayan altı haneli bir [CRON ifadesi](functions-bindings-timer.md#cron-expressions). |

2. **Oluştur**’a tıklayın. Seçtiğiniz dilde her dakika çalışan bir işlev oluşturulur.

3. Günlüklere yazılan izleme bilgilerini görüntüleyerek yürütmeyi doğrulayın.

    ![Azure portalında İşlevler günlük görüntüleyicisi.](./media/functions-create-scheduled-function/functions-timer-trigger-view-logs2.png)

Artık, işlevin zamanlamasını dakikada bir yerine saatte bir çalışacak şekilde değiştirebilirsiniz. 

## <a name="update-the-timer-schedule"></a>Zamanlayıcı zamanlamasını güncelleştirme

1. İşlevinizi genişletin ve **Tümleştir**’e tıklayın. Burada, işlevinizin giriş ve çıkış bağlamalarını tanımlamanın yanı sıra zamanlamayı da ayarlarsınız. 

2. `0 0 */1 * * *` şeklinde yeni bir saatlik **Zamanlama** değeri girin ve **Kaydet**’e tıklayın.  

![İşlevler Azure portalındaki zamanlayıcı zamanlamasını güncelleştirir.](./media/functions-create-scheduled-function/functions-timer-trigger-change-schedule.png)

Saatte bir çalışan bir işleviniz oldu. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bir zamanlamaya göre çalışan bir işlev oluşturdunuz.

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Zamanlayıcı tetikleyicileri hakkında daha fazla bilgi edinmek için bkz. [Azure İşlevleri ile kod yürütme zamanlama](functions-bindings-timer.md).
