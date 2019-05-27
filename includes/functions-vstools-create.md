---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/05/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 6c430f22a9d4fa0fad95bcaa41675545fffd91ec
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66131826"
---
Visual Studio'daki Azure İşlevleri proje şablonu, Azure'daki bir işlev uygulamasında yayımlanabilen bir proje oluşturur. Bir işlev uygulaması grubun işlevleri için bir mantıksal birim olarak yönetim, dağıtım ve kaynakların paylaşımı için kullanabilirsiniz.

1. Visual Studio'da üzerinde **dosya** menüsünde **yeni** > **proje**.

2. İçinde **yeni proje** iletişim kutusunda **yüklü** > **Visual C#**   >  **bulut**  >  **Azure işlevleri**. Projeniz için bir ad girin ve seçin **Tamam**. İşlev uygulamasının adı, bir C# ad alanı olarak geçerli olmalıdır; bu nedenle alt çizgi, kısa çizgi veya alfasayısal olmayan herhangi bir karakter kullanmayın.

    ![Visual Studio'da bir işlev oluşturmak için yeni proje iletişim kutusu](./media/functions-vstools-create/functions-vs-new-project.png)

3. Resmin altındaki tabloda belirtilen ayarları kullanın.

    ![Visual Studio'da yeni işlev iletişim kutusu](./media/functions-vstools-create/functions-vs-new-function.png) 

    | Ayar      | Önerilen değer  | Açıklama                      |
    | ------------ |  ------- |----------------------------------------- |
    | **Sürüm** | Azure İşlevleri 2.x <br />(.NET Core) | Bu ayar, .NET Core destekleyen Azure işlevleri'nin sürüm 2.x çalışma zamanı kullanan bir işlev projesi oluşturur. Azure İşlevleri 1.x, .NET Framework’ü destekler. Daha fazla bilgi için [hedef Azure işlevleri çalışma zamanı sürümü](../articles/azure-functions/functions-versions.md).   |
    | **Şablon** | HTTP tetikleyicisi | Bu ayar, bir HTTP isteği tarafından tetiklenen bir işlev oluşturur. |
    | **Depolama hesabı**  | Depolama öykünücüsü | HTTP tetikleyicisi, Azure depolama hesabı bağlantısı kullanmaz. Diğer tüm tetikleyici türleri için geçerli bir Depolama hesabı bağlantı dizesi gerekir. |
    | **Erişim hakları** | Anonim | Oluşturulan işlev, anahtar gerektirmeden herhangi bir istemci tarafından tetiklenebilir. Bu yetkilendirme ayarı yeni işlevinizi test etmenizi kolaylaştırır. Anahtarlar ve yetkilendirme hakkında daha fazla bilgi için, [HTTP ve web kancası bağlamaları](../articles/azure-functions/functions-bindings-http-webhook.md) altında [Yetkilendirme anahtarları](../articles/azure-functions/functions-bindings-http-webhook.md#authorization-keys) bölümüne bakın. |
    
    > [!NOTE]
    > Ayarladığınızdan emin olun **erişim hakları** için `Anonymous`. Varsayılan düzeyini seçerseniz `Function`, sunmak için gerekmesinden [işlev anahtarı](../articles/azure-functions/functions-bindings-http-webhook.md#authorization-keys) işlevi uç noktanızı erişmek için istek.
    
4. Seçin **Tamam** HTTP ile tetiklenen bir işlev ve işlev projesi oluşturmak için.
