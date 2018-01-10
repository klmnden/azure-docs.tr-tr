---
title: "Azure’da GitHub web kancası tarafından tetiklenen bir işlev oluşturma | Microsoft Docs"
description: "GitHub web kancası ile çağrılan sunucusuz işlev oluşturmak için Azure İşlevlerini kullanın."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.assetid: 36ef34b8-3729-4940-86d2-cb8e176fcc06
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/31/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: cdfb5db7b304a18d6945328abc0ca7ebf2f9ec6a
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="create-a-function-triggered-by-a-github-webhook"></a>GitHub web kancası tarafından tetiklenen bir işlev oluşturma

GitHub’a özel bir yük kullanarak, HTTP web kancası isteğiyle tetiklenmiş bir işlev oluşturma hakkında bilgi edinin.

![Azure portalında GitHub Web Kancası ile tetiklenen işlev](./media/functions-create-github-webhook-triggered-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a>Ön koşullar

+ En az bir proje içeren bir GitHub hesabı.
+ Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Azure İşlev uygulaması oluşturma

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![İşlev uygulaması başarıyla oluşturuldu.](./media/functions-create-first-azure-function/function-app-create-success.png)

Ardından, yeni işlev uygulamasında bir işlev oluşturun.

<a name="create-function"></a>

## <a name="create-a-github-webhook-triggered-function"></a>GitHub web kancası ile tetiklenen bir işlev oluşturma

1. İşlev uygulamanızı genişletin ve **İşlevler**'in yanındaki **+** düğmesine tıklayın. Bu, işlev uygulamanızdaki ilk işlevse **Özel işlev**'i seçin. Böylece işlev şablonlarının tamamı görüntülenir.

    ![Azure portalındaki İşlevler hızlı başlangıç sayfası](./media/functions-create-github-webhook-triggered-function/add-first-function.png)

2. Arama alanına `github` yazıp GitHub web kancası tetikleyici şablonunuz için istediğiniz dili seçin. 

     ![GitHub web kancası tetikleyici şablonunu seçin](./media/functions-create-github-webhook-triggered-function/functions-create-github-webhook-trigger.png) 

2. İşleviniz için bir **Ad** yazın ve **Oluştur**'u seçin. 

     ![Azure portalında GitHub web kancası ile tetiklenen işlevi yapılandırma](./media/functions-create-github-webhook-triggered-function/functions-create-github-webhook-trigger-2.png) 

3. Yeni işlevinizde, **</> İşlev URL’sini al**’a tıklayın, sonra da değerleri kopyalayın ve kaydedin. **</> GitHub parolasını al** için de aynı işlemi yapın. GitHub’da web kancasını yapılandırırken bu değerleri kullanırsınız.

    ![İşlev kodunu gözden geçirme](./media/functions-create-github-webhook-triggered-function/functions-copy-function-url-github-secret.png)

Ardından, GitHub deponuzda web kancasını oluşturursunuz.

## <a name="configure-the-webhook"></a>Web kancası yapılandırma

1. GitHub’da sahip olduğunuz bir depoya gidin. Varsa çatallandırdığınız depoları da kullanabilirsiniz. Bir depoyu çatallaştırmanız gerekirse, <https://github.com/Azure-Samples/functions-quickstart> sayfasını kullanın.

1. **Ayarlar**’a tıklayın, sonra da **Web Kancaları**’na ve **Web kancası ekle**’ye tıklayın.

    ![GitHub web kancası ekleme](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-2.png)

1. Tabloda belirtilen ayarları kullanın, ardından **Web kancası ekle**'ye tıklayın.

    ![Web kancası URL'sini ve parolasını ayarlama](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-3.png)

| Ayar | Önerilen değer | Açıklama |
|---|---|---|
| **Yük URL'si** | Kopyalanan değer | **</> İşlev URL’sini Al** tarafından döndürülen değeri kullanın. |
| **Gizli dizi**   | Kopyalanan değer | **</> GitHub gizli dizisini al** tarafından döndürülen değeri kullanın. |
| **İçerik türü** | uygulama/json | İşlev bir JSON yükü bekler. |
| Olay tetikleyicileri | Olayları ayrı ayrı seçmeme izin ver | Yalnızca sorun yorum olaylarını tetiklemek istiyoruz.  |
| | Sorun açıklaması |  |

Şimdi, web kancası yeni bir sorun açıklaması eklendiğinde işlevinizi tetikleyecek şekilde yapılandırılır.

## <a name="test-the-function"></a>İşlevi test etme

1. GitHub deposunda **Sorunlar** sekmesini yeni bir tarayıcı penceresinde açın.

1. Yeni pencerede **Yeni Sorun**’a tıklayın, bir başlık yazın ve ardından **Yeni sorunu gönder**’e tıklayın.

1. Sorunda bir açıklama yazın ve **Açıklama**’ya tıklayın.

    ![GitHub sorun açıklaması ekleyin.](./media/functions-create-github-webhook-triggered-function/functions-github-webhook-add-comment.png)

1. Portala geri dönün ve günlükleri görüntüleyin. Yeni açıklama metnini içeren bir izleme girişi görmeniz gerekir.

     ![Günlüklerde açıklama metnini görüntüleyin.](./media/functions-create-github-webhook-triggered-function/function-app-view-logs.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

GitHub web kancasından istek alındığında çalıştırılan bir işlev oluşturdunuz.

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Web kancası bağlamaları hakkında daha fazla bilgi için bkz. [Azure İşlevleri HTTP ve web kancası bağlamaları](functions-bindings-http-webhook.md).
