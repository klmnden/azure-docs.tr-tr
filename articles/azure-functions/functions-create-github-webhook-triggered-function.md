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
ms.date: 04/18/2017
ms.author: glenga
translationtype: Human Translation
ms.sourcegitcommit: 9eafbc2ffc3319cbca9d8933235f87964a98f588
ms.openlocfilehash: d4354546f3342d65353a86a4cec7d02547ab92e7
ms.lasthandoff: 04/22/2017


---
# <a name="create-a-function-triggered-by-a-github-webhook"></a>GitHub web kancası tarafından tetiklenen bir işlev oluşturma

GitHub web kancası tarafından tetiklenen bir işlev oluşturmayı öğrenin. 

![Azure portalında işlev uygulaması oluşturma](./media/functions-create-github-webhook-triggered-function/function-app-in-portal-editor.png)

Bu konu için, [Azure Portal’dan ilk işlevinizi oluşturma](functions-create-first-azure-function.md) konusunda oluşturulan kaynaklar gereklidir.

Ayrıca bir de GitHub hesabı gerekir. Henüz yoksa [ücretsiz GitHub hesabına kaydolabilirsiniz](https://github.com/join). 

Bu konu başlığı altındaki adımların tümünü beş dakikadan kısa bir sürede tamamlamalısınız.

## <a name="find-your-function-app"></a>İşlev uygulamanızı bulma    

1. [Azure Portal](https://portal.azure.com/)’da oturum açın. 

2. Portalın en üstündeki arama çubuğunda, işlev uygulamanızın adını yazın ve uygulamayı listeden seçin.

## <a name="create-function"></a>GitHub web kancası ile tetiklenen bir işlev oluşturma

1. İşlev uygulamanızda, **İşlevler**’in yanındaki **+** düğmesine tıklayın, tercih ettiğiniz dildeki **GitHubWebHook** şablonuna tıklayın ve sonra da **Oluştur**’a tıklayın.
   
    ![Azure Portal’da GitHub web kancası ile tetiklenen bir işlev oluşturun.](./media/functions-create-github-webhook-triggered-function/functions-create-github-webhook-trigger.png) 

2. **</> İşlev URL’sini al**’a tıklayın, sonra da değerleri kopyalayın ve kaydedin. **</> GitHub parolasını al** için de aynı işlemi yapın. GitHub’da web kancasını yapılandırırken bu değerleri kullanırsınız. 

    ![İşlev kodunu gözden geçirme](./media/functions-create-github-webhook-triggered-function/functions-copy-function-url-github-secret.png) 
         
Ardından, GitHub deponuzda web kancasını oluşturursunuz. 

## <a name="configure-the-webhook"></a>Web kancası yapılandırma
1. GitHub’da sahip olduğunuz bir depoya gidin. Varsa çatallandırdığınız depoları da kullanabilirsiniz.
 
2. **Ayarlar**’a tıklayın, sonra da **Web Kancaları**’na ve **Web kancası ekle**’ye tıklayın.
   
    ![GitHub web kancası ekleme](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-2.png)

3. İşlevinizin URL’sini ve gizli anahtarını **Yük URL’si** ve **Gizli Anahtar** alanına yapıştırıp **İçerik türü** için **uygulama/json** öğesini seçin.

4. **Olayları tek tek seçmeme izin ver**’e tıklayın, **Sorun Açıklaması**’nı seçin ve **Web kancası ekle**’ye tıklayın.
   
    ![Web kancası URL'sini ve parolasını ayarlama](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-3.png)

Şimdi, web kancası yeni bir sorun açıklaması eklendiğinde işlevinizi tetikleyecek şekilde yapılandırılır. 

## <a name="test-the-function"></a>İşlevi test etme
1. GitHub deposunda **Sorunlar** sekmesini yeni bir tarayıcı penceresinde açın.

2. Yeni pencerede **Yeni Sorun**’a tıklayın, bir başlık yazın ve ardından **Yeni sorunu gönder**’e tıklayın. 

2. Sorunda bir açıklama yazın ve **Açıklama**’ya tıklayın. 

3. Diğer GitHub penceresinde, yeni web kancanızın yanındaki **Düzenle** seçeneğine tıklayın, ekranı aşağı kaydırarak **Son Teslimler**’e gelin, ardından bir web kancası isteğinin işleviniz tarafından işlendiğini doğrulayın. 
 
    ![Web kancası URL'sini ve parolasını ayarlama](./media/functions-create-github-webhook-triggered-function/functions-github-webhook-triggered.png)

   İşlevinizden gelen yanıt `New GitHub comment: <Your issue comment text>` içermelidir.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]


