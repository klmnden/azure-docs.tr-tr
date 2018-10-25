---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: brandwe
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: ios
ms.workload: identity
ms.date: 09/19/2018
ms.author: brandwe
ms.custom: include file
ms.openlocfilehash: 1604b7c9ee9888375e65aa679803c6e996e13b14
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49988291"
---
## <a name="register-your-application"></a>Uygulamanızı kaydetme

Uygulamanızı iki yoldan biriyle sonraki iki bölümde açıklandığı gibi kaydedebilirsiniz.

### <a name="option-1-express-mode"></a>1. seçenek: Hızlı mod

Artık uygulamanızı kaydetmek gerekiyor *Microsoft uygulama kayıt portalı*:

1. Uygulamanızı kaydetmek [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=ios&step=configure).
2. Uygulamanız ve e-posta adresiniz için bir ad girin.
3. Destekli kurulum seçeneğinin işaretli olduğundan emin olun.
4. Uygulama Kimliğini almak ve kodunuza yapıştırmak için yönergeleri izleyin.

### <a name="option-2-advanced-mode"></a>2. seçenek: Gelişmiş mod

1. Git [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/portal/register-app).
2. Uygulamanız için bir ad girin.
3. Destekli kurulum seçeneğini işaretli olduğundan emin olun.
4. Seçin `Add Platform` seçip `Native Application`.
5. `Save` öğesini seçin.
6. Xcode için geri dönün. İçinde `ViewController.swift`, ile başlayan satırı değiştirin '`let kClientID`' yalnızca kayıtlı uygulama Kimliğine sahip:

```swift
let kClientID = "Your_Application_Id_Here"
```

<!-- Workaround for Docs conversion bug -->
<ol start="6">
<li>
Control + tıklayın <code>Info.plist</code> bağlamsal menüyü getirin ve ardından: <code>Open As</code> > <code>Source Code</code>
</li>
<li>
Altında <code>dict</code> kök düğümü, aşağıdakileri ekleyin:
</li>
</ol>

```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>$(PRODUCT_BUNDLE_IDENTIFIER)</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>msal[Your_Application_Id_Here]</string>
        </array>
    </dict>
</array>
```
<ol start="8">
<li>
Değiştirin <i> <code>[Your_Application_Id_Here]</code> </i> yalnızca kayıtlı uygulama kimliğine sahip
</li>
</ol>
