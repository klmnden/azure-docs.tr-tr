---
title: "Azure portalında ilk işlevinizi oluşturma | Microsoft Docs"
description: "Azure portalını kullanarak sunucusuz yürütme için ilk Azure İşlevinizi oluşturma hakkında bilgi edinin."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 96cf87b9-8db6-41a8-863a-abb828e3d06d
ms.service: functions
ms.devlang: multiple
ms.topic: hero-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/10/2017
ms.author: glenga
ms.translationtype: Human Translation
ms.sourcegitcommit: 5e92b1b234e4ceea5e0dd5d09ab3203c4a86f633
ms.openlocfilehash: abd508631787ba5f839a4ae2ea82e76c4bfab425
ms.contentlocale: tr-tr
ms.lasthandoff: 05/10/2017


---
# <a name="create-your-first-function-in-the-azure-portal"></a>Azure portalında ilk işlevinizi oluşturma

Azure İşlevleri, öncelikle bir VM oluşturmak veya bir web uygulaması yayımlamak zorunda kalmadan kodunuzu sunucusuz bir ortamda yürütmenize olanak tanır. Bu konu başlığında, Azure portalında İşlevler’i kullanarak bir "hello world" işlevi oluşturmayı öğrenebilirsiniz.

![Azure portalında işlev uygulaması oluşturma](./media/functions-create-first-azure-function/function-app-in-portal-editor.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

Bu konu başlığı altındaki adımların tümünü beş dakikadan kısa bir sürede tamamlamalısınız.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

[Azure Portal](https://portal.azure.com/)’da oturum açın.

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

İşlevlerinizin yürütülmesini barındıran bir işlev uygulamasına sahip olmanız gerekir. İşlev uygulaması, kaynakların daha kolay yönetilmesi, dağıtılması ve paylaşılması için işlevleri bir mantıksal birim olarak gruplandırmanıza olanak tanır. 

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![İşlev uygulaması başarıyla oluşturuldu.](./media/functions-create-first-azure-function/function-app-create-success.png)

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

Ardından, yeni işlev uygulamasında bir işlev oluşturun.

## <a name="create-function"></a>HTTP ile tetiklenen bir işlev oluşturma

1. Yeni işlev uygulamanızı genişletin, ardından **İşlevler**’in yanındaki **+** düğmesine tıklayın.

2.  **Hızlıca çalışmaya başlayın** sayfasında **Web Kancası ve API**’ye tıklayın, işleviniz için bir dil seçin ve **Bu işlevi oluştur**’a tıklayın. 
   
    ![Azure portalındaki İşlevler hızlı başlangıcı.](./media/functions-create-first-azure-function/function-app-quickstart-node-webhook.png)

HTTP ile tetiklenen işlevin şablonu kullanılarak, seçtiğiniz dilde bir işlev oluşturulur. Bir HTTP isteği göndererek yeni işlevi çalıştırabilirsiniz.

## <a name="test-the-function"></a>İşlevi test etme

1. Yeni işlevinizde **</> İşlev URL’sini al**’a tıklayıp **İşlev URL’si** değerini kopyalayın. 

    ![Azure portalından işlev URL’sini kopyalama](./media/functions-create-first-azure-function/function-app-develop-tab-testing.png)

2. HTTP isteğinin URL’sini tarayıcınızın adres çubuğuna yapıştırın. `&name=<yourname>` sorgu dizesini bu URL’ye ekleyip isteği yürütün. İşlevin döndürdüğü GET isteğine tarayıcıda verilen yanıt aşağıda gösterilmiştir:

    ![Tarayıcıdaki işlev yanıtı.](./media/functions-create-first-azure-function/function-app-browser-testing.png)

    İstek URL’si, işlevinize HTTP üzerinden erişmek için varsayılan olarak gerekli olan bir anahtar içerir.   

## <a name="view-the-function-logs"></a>İşlev günlüklerini görüntüleme 

İşleviniz çalıştığında, izleme bilgileri günlüklere yazılır. Önceki yürütme işleminden alınan izleme çıktısını görmek için, portalda işlevinize geri dönün ve ekranın altındaki yukarı okuna tıklayarak **Günlükler**’i genişletin. 

![Azure portalında İşlevler günlük görüntüleyicisi.](./media/functions-create-first-azure-function/function-view-logs.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [Clean up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

HTTP ile tetiklenen basit bir işlevi kullanarak işlev uygulaması oluşturdunuz.  

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Daha fazla bilgi için bkz. [Azure İşlevleri HTTP ve web kancası bağlamaları](functions-bindings-http-webhook.md).




