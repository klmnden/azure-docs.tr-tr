---
title: "İlk Azure İşlevinizi oluşturma | Microsoft Belgeleri"
description: "İki dakikadan kısa bir süre içinde sunucusuz bir uygulama olan ilk Azure İşlevinizi oluşturun."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 4a1669e7-233e-4ea2-9b83-b8624f2dbe59
ms.service: functions
ms.devlang: multiple
ms.topic: hero-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/14/2016
ms.author: glenga
translationtype: Human Translation
ms.sourcegitcommit: afe143848fae473d08dd33a3df4ab4ed92b731fa
ms.openlocfilehash: a797910e286cd2aacf5a8aa6c509e2b0f5f60276
ms.lasthandoff: 03/17/2017


---
# <a name="create-your-first-azure-function"></a>İlk Azure İşlevinizi oluşturma

Bu konu başlığında, bir HTTP isteği tarafından çağrılan basit bir "merhaba dünya" işlevi oluşturmak için portaldaki Azure İşlevleri hızlı başlangıcının nasıl kullanılacağı gösterilmektedir. Azure İşlevleri hakkında daha fazla bilgi edinmek için bkz. [Azure İşlevlerine Genel Bakış](functions-overview.md).

Başlamadan önce bir Azure hesabınızın olması gerekir. [Ücretsiz hesaplar](https://azure.microsoft.com/free/) mevcuttur. Ayrıca, Azure’a kaydolmadan [Azure İşlevleri’ni deneyebilirsiniz](https://azure.microsoft.com/try/app-service/functions/).

## <a name="create-a-function-from-the-portal-quickstart"></a>Portal hızlı başlangıcından işlev oluşturma

1. [Azure İşlevleri portalına](https://functions.azure.com/signin) gidin ve Azure hesabınız ile oturum açın. 
 
2. Yeni işlev uygulamanız için benzersiz bir **Ad** yazın veya otomatik olarak oluşturulan adı kabul edin, tercih ettiğiniz **Region (Bölge)** seçeneğini belirleyin ve ardından **Create + get started (Oluştur + başla)** düğmesine tıklayın. Geçerli adlar yalnızca küçük harf, sayı ve kısa çizgi içerebilir. Alt çizgi (**_**) izin verilen bir karakter değildir.

3. **Hızlı Başlangıç** sekmesinde, **Web Kancası + API**’ye tıklayıp işlevinizin dilini seçin ve **İşlev oluştur**'a tıklayın. Seçtiğiniz dilde önceden tanımlanmış yeni bir işlev oluşturulur. 
   
    ![](./media/functions-create-first-azure-function/function-app-quickstart-node-webhook.png)

4. (İsteğe bağlı) Hızlı başlangıcın bu noktasında portaldaki Azure İşlevleri özelliklerinin hızlı turuna bakmayı seçebilirsiniz. Turu tamamladıktan veya atladıktan sonra bir HTTP isteği göndererek yeni işlevinizi test edebilirsiniz.

## <a name="test-the-function"></a>İşlevi test etme
[!INCLUDE [Functions quickstart test](../../includes/functions-quickstart-test.md)]

## <a name="watch-the-video"></a>Videoyu izleme
Aşağıdaki video bu öğreticideki temel adımların nasıl gerçekleştirileceğini gösterir. 

> [!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-your-first-Azure-Function-simple/player]
> 


## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [Functions quickstart next steps](../../includes/functions-quickstart-next-steps.md)]

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]


