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
ms.openlocfilehash: 5c82d7ad3cf9c2318d3bf5d0157f00730a24a968
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57457783"
---
Visual Studio'daki Azure İşlevleri proje şablonu, Azure'daki bir işlev uygulamasında yayımlanabilen bir proje oluşturur. İşlev uygulaması, kaynakların yönetilmesi, dağıtılması ve paylaşılması için işlevleri bir mantıksal birim olarak gruplandırmanıza olanak tanır.

1. Visual Studio'da **Dosya** menüsünden **Yeni** > **Proje**’yi seçin.

2. **Yeni Proje** iletişim kutusunda **Yüklü**'yü seçin, **Visual C#** > **Bulut**'u genişletin, **Azure İşlevleri**'ni seçin, projeniz için bir **Ad** yazın ve **Tamam**'a tıklayın. İşlev uygulamasının adı, bir C# ad alanı olarak geçerli olmalıdır; bu nedenle alt çizgi, kısa çizgi veya alfasayısal olmayan herhangi bir karakter kullanmayın.

    ![Visual Studio'da işlev oluşturmaya yönelik yeni proje iletişim kutusu](./media/functions-vstools-create/functions-vs-new-project.png)

3. Resmin altındaki tabloda belirtilen ayarları kullanın.

    ![Visual Studio'da Yeni işlev iletişim kutusu](./media/functions-vstools-create/functions-vs-new-function.png) 

    | Ayar      | Önerilen değer  | Açıklama                      |
    | ------------ |  ------- |----------------------------------------- |
    | **Sürüm** | Azure İşlevleri 2.x <br />(.NET Core) | Bu, Azure İşlevleri'nin .NET Core destekleyen sürüm 2.x çalışma zamanını kullanan bir işlev projesi oluşturur. Azure İşlevleri 1.x, .NET Framework’ü destekler. Daha fazla bilgi için bkz. [Azure İşlevleri çalışma zamanı sürümünün hedefini belirleme](../articles/azure-functions/functions-versions.md).   |
    | **Şablon** | HTTP tetikleyicisi | Bu, HTTP isteği tarafından tetiklenen bir işlev oluşturur. |
    | **Depolama hesabı**  | Depolama Öykünücüsü | HTTP tetikleyicisi Depolama hesabı bağlantısını kullanmaz. Diğer tüm tetikleyici türleri için geçerli bir Depolama hesabı bağlantı dizesi gerekir. |
    | **Erişim hakları** | Anonim | Oluşturulan işlev, anahtar gerektirmeden herhangi bir istemci tarafından tetiklenebilir. Bu yetkilendirme ayarı yeni işlevinizi test etmenizi kolaylaştırır. Anahtarlar ve yetkilendirme hakkında daha fazla bilgi için, [HTTP ve web kancası bağlamaları](../articles/azure-functions/functions-bindings-http-webhook.md) altında [Yetkilendirme anahtarları](../articles/azure-functions/functions-bindings-http-webhook.md#authorization-keys) bölümüne bakın. |
    
    > [!NOTE]
    > Ayarladığınızdan emin olun **erişim hakları** için `Anonymous`. Varsayılan düzeyini seçtiğinizde `Function`, sunmak için gerekli [işlev anahtarı](../articles/azure-functions/functions-bindings-http-webhook.md#authorization-keys) işlevi uç noktanızı erişmek için istek.
    
4. İşlev projesini ve HTTP ile tetiklenen işlevi oluşturmak için **Tamam**'a tıklayın.
