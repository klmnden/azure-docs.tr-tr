---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: fb521fc9d4927a953cdd66948101969dfc102de4
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48843509"
---
## <a name="register-your-application"></a>Uygulamanızı kaydetme
Uygulamanızı iki yoldan biriyle kaydedebilirsiniz.

### <a name="option-1-express-mode"></a>1. seçenek: Hızlı mod
Aşağıdakileri yaparak, uygulamanızı hızlı bir şekilde kaydedebilirsiniz:
1. [Microsoft Uygulama Kayıt Portalı](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=windowsDesktop&step=configure)'na gidin.

2. Seçin **uygulama ekleme**.

3. **Uygulama Adı** kutusuna uygulamanız için bir ad girin.

4. Emin **destekli Kurulum** onay kutusunu seçili ve ardından **Oluştur**.

5. Uygulama Kimliği alma yönergeleri izleyin ve kodunuzun yapıştırın.

### <a name="option-2-advanced-mode"></a>2. seçenek: Gelişmiş mod
Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze eklemek için aşağıdakileri yapın:
1. Uygulamanızı henüz kaydolmadıysanız, Git [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/portal/register-app).

2. Seçin **uygulama ekleme**.

3. **Uygulama Adı** kutusuna uygulamanız için bir ad girin. 

4. **Destekli Kurulum** onay kutusunun işaretli olmadığından emin olun ve **Oluştur**’u seçin.

5. **Platform Ekle**’yi, **Yerel Uygulama**’yı ve **Kaydet**’i seçin.

6. İçinde **uygulama kimliği** kutusunda, GUID kopyalayın.

7. Visual Studio, açık Git *App.xaml.cs* dosya ve sonra değiştirmek `your_client_id_here` yalnızca kayıtlı ve kopyaladığınız uygulama kimliği.

    ```csharp
    private static string ClientId = "your_application_id_here";
    ```
