---
title: "Azure’da GitHub web kancası tarafından tetiklenen bir işlev oluşturma | Microsoft Docs"
description: "GitHub web kancası ile çağrılan sunucusuz işlev oluşturmak için Azure İşlevlerini kullanın."
services: azure-functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 36ef34b8-3729-4940-86d2-cb8e176fcc06
ms.service: functions
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/02/2017
ms.author: glenga
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 34988ef05a27062ca109a1640e39695b52b8773f
ms.contentlocale: tr-tr
ms.lasthandoff: 05/10/2017


---
# <a name="create-a-function-triggered-by-a-github-webhook"></a>GitHub web kancası tarafından tetiklenen bir işlev oluşturma

GitHub’a özel bir yük kullanarak, HTTP web kancası isteğiyle tetiklenmiş bir işlev oluşturma hakkında bilgi edinin. 

![Azure portalında GitHub Web Kancası ile tetiklenen işlev](./media/functions-create-github-webhook-triggered-function/function-app-in-portal-editor.png)

Bu konu başlığı altındaki adımların tümünü beş dakikadan kısa bir sürede tamamlamalısınız.

## <a name="prerequisites"></a>Ön koşullar 

[!INCLUDE [Previous quickstart note](../../includes/functions-quickstart-previous-topics.md)]

Ayrıca en az bir proje içeren bir GitHub hesabı gerekir. Henüz yoksa [ücretsiz GitHub hesabına kaydolabilirsiniz](https://github.com/join).

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)] 

## <a name="create-function"></a>GitHub web kancası ile tetiklenen bir işlev oluşturma

1. İşlev uygulamanızı genişletin, **İşlevler**’in yanındaki **+** düğmesine tıklayın, tercih ettiğiniz dildeki **GitHubWebHook** şablonuna tıklayın. **İşlevinizi adlandırın**, ardından **Oluştur**’a tıklayın. 

2. Yeni işlevinizde, **</> İşlev URL’sini al**’a tıklayın, sonra da değerleri kopyalayın ve kaydedin. **</> GitHub parolasını al** için de aynı işlemi yapın. GitHub’da web kancasını yapılandırırken bu değerleri kullanırsınız. 

    ![İşlev kodunu gözden geçirme](./media/functions-create-github-webhook-triggered-function/functions-copy-function-url-github-secret.png) 
         
Ardından, GitHub deponuzda web kancasını oluşturursunuz. 

## <a name="configure-the-webhook"></a>Web kancası yapılandırma
1. GitHub’da sahip olduğunuz bir depoya gidin. Varsa çatallandırdığınız depoları da kullanabilirsiniz. Bir depoyu çatallaştırmanız gerekirse, <https://github.com/Azure-Samples/functions-quickstart> sayfasını kullanın. 
 
2. **Ayarlar**’a tıklayın, sonra da **Web Kancaları**’na ve **Web kancası ekle**’ye tıklayın.
   
    ![GitHub web kancası ekleme](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-2.png)

3. Tabloda belirtilen ayarları kullanın, ardından **Web kancası ekle**'ye tıklayın.
 
    ![Web kancası URL'sini ve parolasını ayarlama](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-3.png)

    | Ayar      |  Önerilen değer   | Açıklama                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Yük URL'si** | Kopyalanan değer | **</> İşlev URL’sini Al** tarafından döndürülen değeri kullanın. |
    | **Gizli dizi**   | Kopyalanan değer | **</> GitHub gizli dizisini al** tarafından döndürülen değeri kullanın. |
    | **İçerik türü** | uygulama/json | İşlev bir JSON yükü bekler. |
    | Olay tetikleyicileri | Olayları ayrı ayrı seçmeme izin ver | Yalnızca sorun yorum olaylarını tetiklemek istiyoruz.  |
    |                | Sorun açıklaması                    |  |

Şimdi, web kancası yeni bir sorun açıklaması eklendiğinde işlevinizi tetikleyecek şekilde yapılandırılır. 

## <a name="test-the-function"></a>İşlevi test etme
1. GitHub deposunda **Sorunlar** sekmesini yeni bir tarayıcı penceresinde açın.

2. Yeni pencerede **Yeni Sorun**’a tıklayın, bir başlık yazın ve ardından **Yeni sorunu gönder**’e tıklayın. 

2. Sorunda bir açıklama yazın ve **Açıklama**’ya tıklayın. 

    ![GitHub sorun açıklaması ekleyin.](./media/functions-create-github-webhook-triggered-function/functions-github-webhook-add-comment.png) 

3. Portala geri dönün ve günlükleri görüntüleyin. Yeni açıklama metnini içeren bir izleme girişi görmeniz gerekir. 
    
     ![Günlüklerde açıklama metnini görüntüleyin.](./media/functions-create-github-webhook-triggered-function/function-app-view-logs.png)
 

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

GitHub web kancasından istek alındığında çalıştırılan bir işlev oluşturdunuz. 
[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)] Web kancası bağlamaları hakkında daha fazla bilgi için bkz. [Azure İşlevleri HTTP ve web kancası bağlamaları](functions-bindings-http-webhook.md). 




