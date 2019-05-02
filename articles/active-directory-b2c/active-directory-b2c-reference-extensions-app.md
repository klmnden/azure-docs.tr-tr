---
title: Azure Active Directory B2C uygulamasında uzantıları | Microsoft Docs
description: B2c-extensions-app geri yükleniyor.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 9/06/2017
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: aee21e6ea3c598818091aa11c3d13b9d6172743c
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64703449"
---
# <a name="azure-ad-b2c-extensions-app"></a>Azure AD B2C: Uzantılar uygulaması

Azure AD B2C dizini oluşturulurken bir uygulama olarak adlandırılan `b2c-extensions-app. Do not modify. Used by AADB2C for storing user data.` yeni dizinin içinde otomatik olarak oluşturulur. Bu uygulama denir **b2c-extensions-app**, görünür olan *uygulama kayıtları*. Azure AD B2C hizmeti tarafından kullanıcılar ve özel öznitelikler hakkında bilgi depolamak için kullanılır. Uygulama silinirse, Azure AD B2C düzgün çalışmaz ve üretim ortamınızı etkilenecek.

> [!IMPORTANT]
> Kiracınızın hemen silinecek planlama sürece b2c-extensions-app silmeyin. Uygulama 30 günden fazla bir süre için silinen kalırsa, kullanıcı bilgilerini kalıcı olarak kaybolur.

## <a name="verifying-that-the-extensions-app-is-present"></a>Uzantılar uygulaması mevcut olduğu doğrulanıyor

B2c-extensions-app mevcut olduğunu doğrulamak için:

1. Azure AD B2C kiracınızı tıklayarak **tüm hizmetleri** sol taraftaki gezinti menüsünde.
1. İçin arama yapın ve açık **uygulama kayıtları**.
1. İle başlayan uygulama Ara **b2c-extensions-app**

## <a name="recover-the-extensions-app"></a>Uzantılar uygulaması Kurtar

B2c-extensions-app yanlışlıkla sildiyseniz, kurtarma için 30 gününüz vardır. Graph API'sini kullanarak uygulama geri yükleyebilirsiniz:

1. Gözat [ https://graphexplorer.azurewebsites.net/ ](https://graphexplorer.azurewebsites.net/).
1. Siteye silinen uygulamayı geri yüklemek istediğiniz Azure AD B2C dizini için genel yönetici olarak oturum açın. Bu genel yönetici, aşağıdakine benzer bir e-posta adresi olmalıdır: `username@{yourTenant}.onmicrosoft.com`.
1. Bir HTTP GET URL'sinde sorun `https://graph.windows.net/myorganization/deletedApplications` api-version ile 1.6 =. Bu işlem, tüm son 30 gün içinde silinmiş olan uygulamaları listeler.
1. 'B2c-uzantısı-app' ve kopya ile adı başladığı listesinde uygulamanın bulur, `objectid` özellik değeri.
1. Bir HTTP POST URL'sinde sorun `https://graph.windows.net/myorganization/deletedApplications/{OBJECTID}/restore`. Değiştirin `{OBJECTID}` URL'si ile bir kısmı `objectid` önceki adımdan. 

Artık yapabilmelisiniz [geri yüklenen uygulama görmek](#verifying-that-the-extensions-app-is-present) Azure portalında.

> [!NOTE]
> Son 30 gün içinde silinmiş olması durumunda bir uygulamayı yalnızca geri yüklenebilir. 30 günden fazla süredir, verilerin kalıcı olarak kaybolur. Daha fazla yardım almak için bir destek bileti oluşturun.
