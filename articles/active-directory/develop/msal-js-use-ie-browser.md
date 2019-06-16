---
title: Internet Explorer (JavaScript için Microsoft kimlik doğrulama kitaplığı) | Azure
description: Microsoft kimlik doğrulama kitaplığı (MSAL.js) JavaScript için Internet Explorer tarayıcı ile kullanma hakkında bilgi edinin.
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/16/2019
ms.author: nacanuma
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8cf8c84120f4c90d3943cfc31ffbf9aafcec0ba3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65873923"
---
# <a name="known-issues-on-internet-explorer-and-microsoft-edge-browsers-with-msaljs"></a>Internet Explorer ve Microsoft Edge tarayıcılarda MSAL.js bilinen sorunlar

JavaScript (MSAL.js) için Microsoft kimlik doğrulama kitaplığı için oluşturulan [JavaScript ES5](https://fr.wikipedia.org/wiki/ECMAScript#ECMAScript_Edition_5_.28ES5.29) Internet Explorer'da çalıştırabilirsiniz. Ancak, bilmeniz gereken birkaç şey vardır.

## <a name="run-an-app-in-internet-explorer"></a>Internet Explorer'da uygulama çalıştırma
Internet Explorer'da çalışan uygulamaları MSAL.js kullanmak istiyorsanız, MSAL.js betik başvurmadan önce promise polyfill bir başvuru eklemeniz gerekir.

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/bluebird/3.3.4/bluebird.min.js" class="pre"></script>
```

Internet Explorer JavaScript gösterir yerel olarak desteklemiyor olmasıdır.

## <a name="debugging-an-application-running-in-internet-explorer"></a>Internet Explorer'da çalışan bir uygulamada hata ayıklama

### <a name="running-in-production"></a>Üretimde çalışan
Uygulamanızı üretim ortamında (örneğin, Azure Web apps) normalde dağıtma düzgün çalışır, sağlanan son kullanıcının açılan pencereler kabul etti. Internet Explorer 11 ile bu test.

### <a name="running-locally"></a>Yerel olarak çalıştırma
Çalıştırın ve yerel olarak da Internet Explorer'ın çalışan uygulamanızda hata ayıklamak istiyorsanız, aşağıdaki noktaları göz önünde bulundurmanız gerekir (uygulamanızı olarak çalıştırmak istediğini varsayın *http://localhost:1234* ):

- Internet Explorer "MSAL.js düzgün çalışmasını engeller. korumalı modunu" adlı bir güvenlik mekanizması vardır. Oturum açarken, sonra Belirtiler arasında sayfanın yönlendirilebilir http://localhost:1234/null.

- Çalıştırın ve uygulamanızı yerel olarak hata ayıklama için bu "korumalı modu" devre dışı bırakmak gerekir. Bunun için:

    1. Internet Explorer'ı **Araçları** (dişli simgesi).
    1. Seçin **Internet Seçenekleri** ardından **güvenlik** sekmesi.
    1. Tıklayarak **Internet** bölge ve işaretini kaldırın **korumalı modu etkinleştir (Internet Explorer'ı yeniden başlatma gerektirir)** . Internet Explorer, bilgisayar artık korumalı konusunda sizi uyarır. **Tamam**'ı tıklatın.
    1. Internet Explorer'ı yeniden başlatın.
    1. Uygulamanızda hata ayıklamak ve çalıştırın.

İşiniz bittiğinde, Internet Explorer güvenlik ayarları geri yükleyin.  Seçin **ayarları** -> **Internet Seçenekleri** -> **güvenlik** -> **tüm bölgeleri varsayılan düzeyiniSıfırla**.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [bilinen sorunlar MSAL.js Internet Explorer'da kullanırken](msal-js-use-ie-browser.md).