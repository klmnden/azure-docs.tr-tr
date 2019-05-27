---
title: Azure işlev uygulaması ayarlarını yapılandırma | Microsoft Docs
description: Azure işlev uygulaması ayarlarını yapılandırmayı öğrenin.
services: ''
documentationcenter: .net
author: ggailey777
manager: jeconnoc
ms.assetid: 81eb04f8-9a27-45bb-bf24-9ab6c30d205c
ms.service: azure-functions
ms.topic: conceptual
ms.date: 03/28/2018
ms.author: glenga
ms.custom: cc996988-fb4f-47
ms.openlocfilehash: 92ca09040836dfc55a9d709b12a0ee01192d6bac
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65957397"
---
# <a name="how-to-manage-a-function-app-in-the-azure-portal"></a>Azure portalında işlev uygulaması yönetme 

Azure işlevleri'nde bir işlev uygulaması, tek tek işlevleri için yürütme bağlamı sağlar. Belirtilen işlev uygulaması tarafından barındırılan tüm işlevler için işlev uygulama davranışları uygulayın. Bu konu başlığı altında yapılandırmak ve işlev uygulamalarınızı Azure portalında yönetmek açıklar.

Başlamak için Git [Azure portalında](https://portal.azure.com) ve Azure hesabınızda oturum açın. Portalın en üstündeki arama çubuğunda, işlev uygulamanızın adını yazın ve uygulamayı listeden seçin. İşlev uygulamanızı seçtikten sonra aşağıdaki sayfaya bakın:

![Azure portalında işlev uygulama genel bakış](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-main.png)

İşlev uygulamanızı genel bakış sayfasından özellikle yönetmek için gereken her şey için gidebilirsiniz **[uygulama ayarları](#settings)** ve **[Platformözellikleri](#platform-features)**.

## <a name="settings"></a>Uygulama ayarları

**Uygulama ayarları** sekmesini işlev uygulamanız tarafından kullanılan ayarları bulundurur.

![Azure portalında işlev uygulaması ayarları.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-settings-tab.png)

Bu ayarları şifrelenmiş olarak depolanır ve seçmelisiniz **değerleri gösterir** portalında değerleri görmek için.

Bir ayarı eklemek için seçin **yeni uygulama ayarı** ve yeni bir anahtar-değer çifti ekleyin.

[!INCLUDE [functions-environment-variables](../../includes/functions-environment-variables.md)]

Yerel bir işlev uygulaması geliştirdiğinizde, bu değerleri local.settings.json proje dosyasında saklanır.

## <a name="platform-features"></a>Platform özellikleri

![İşlev uygulaması platformu Özellikleri sekmesi.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-features-tab.png)

İşlev uygulamaları, çalıştırmak ve Azure App Service platformu tarafından korunur. Bu nedenle, işlev uygulamalarınızı Azure'nın temel web barındırma platformu özelliklerinin çoğunu erişebilir. **Platform özellikleri** sekmedir işlevi uygulamalarınızda kullanabileceğiniz bir App Service platformu özelliklerinin çoğu eriştiği. 

> [!NOTE]
> Tüm App Service özellikleri, bir işlev uygulaması tüketim barındırma planı çalıştırdığında kullanılabilir.

Bu konunun geri kalanında Azure portalında işlevler için yararlı olan aşağıdaki bir App Service özelliklere odaklanır:

+ [App Service Düzenleyicisi](#editor)
+ [Console](#console)
+ [Gelişmiş araçlar (Kudu)](#kudu)
+ [Dağıtım seçenekleri](#deployment)
+ [CORS](#cors)
+ [Kimlik Doğrulaması](#auth)
+ [API tanımı](#swagger)

App Service ayarları ile çalışma hakkında daha fazla bilgi için bkz. [Azure App Service ayarlarını yapılandırma](../app-service/configure-common.md).

### <a name="editor"></a>App Service Düzenleyicisi

| | |
|-|-|
| ![İşlev uygulaması App Service Düzenleyicisi.](./media/functions-how-to-use-azure-function-app-settings/function-app-appsvc-editor.png)  | App Service Düzenleyicisi, JSON yapılandırma dosyaları ve kod dosyaları aynı şekilde değiştirmek için kullanabileceğiniz Gelişmiş bir portal düzenleyicisidir. Bu seçeneğin belirlenmesi, ayrı bir tarayıcı sekmesinde temel düzenleyici ile başlatır. Bu, Git deposu ile tümleştirme, çalıştırmak ve kod hatalarını ayıklamak ve işlev uygulaması ayarlarını değiştirmek sağlar. Bu düzenleyici varsayılan işlevi uygulama dikey ile karşılaştırıldığında işlevleriniz için bir Gelişmiş geliştirme ortamı sağlar.    |

![App Service Düzenleyicisi](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-appservice-editor.png)

### <a name="console"></a>Konsolu

| | |
|-|-|
| ![Azure portalında uygulama konsolunda işlevi](./media/functions-how-to-use-azure-function-app-settings/function-app-console.png) | İşlev uygulamanızı komut satırından etkileşim kurmayı olduğunda portal konsolu bir ideal Geliştirici aracıdır. Dizin ve dosya oluşturma ve gezinti, hem de toplu iş dosyaları ve betikler yürütülürken genel komutları içerir. |

![Uygulama konsolunda işlevi](./media/functions-how-to-use-azure-function-app-settings/configure-function-console.png)

### <a name="kudu"></a>Gelişmiş araçlar (Kudu)

| | |
|-|-|
| ![Azure Portal'da, Kudu işlev uygulaması](./media/functions-how-to-use-azure-function-app-settings/function-app-advanced-tools.png) | Gelişmiş araçlar (Kudu olarak da bilinir) App Service için işlev uygulamanızın Gelişmiş yönetim özelliklerine erişim sağlar. Kudu sistem bilgileri, uygulama ayarları, ortam değişkenleri, site uzantıları, HTTP üstbilgileri ve sunucu değişkenlerine yönetin. Da başlatabilirsiniz **Kudu** işlev uygulamanız için SCM uç noktasına gibi gözatarak `https://<myfunctionapp>.scm.azurewebsites.net/` |

![Kudu yapılandırın](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-kudu.png)


### <a name="a-namedeploymentdeployment-options"></a><a name="deployment">Dağıtım seçenekleri

| | |
|-|-|
| ![Azure portalında işlev uygulaması dağıtım seçenekleri](./media/functions-how-to-use-azure-function-app-settings/function-app-deployment-source.png) | İşlevler, işlev kodunuzun yerel makinenizde geliştirmenize olanak tanır. Ardından, Azure'da yerel bir işlev uygulaması projenizi karşıya yükleyebilirsiniz. Yanı sıra geleneksel FTP karşıya yükleme, işlevler sağlar, GitHub, Azure DevOps, Dropbox, Bitbucket ve diğerleri gibi popüler sürekli tümleştirme çözümlerini kullanarak işlev uygulamanızı dağıtın. Daha fazla bilgi için [Azure işlevleri için sürekli dağıtım](functions-continuous-deployment.md). FTP veya yerel Git kullanarak el ile karşıya yüklemek için ayrıca gerekir [dağıtım kimlik bilgilerinizi yapılandırın](functions-continuous-deployment.md#credentials). |


### <a name="cors"></a>CORS

| | |
|-|-|
| ![Azure portalında işlev uygulaması CORS](./media/functions-how-to-use-azure-function-app-settings/function-app-cors.png) | Kötü amaçlı kod yürütme hizmetlerinizdeki önlemek için dış kaynaklardan gelen çağrıları işlev uygulamalarınızı App Service engeller. Çıkış noktaları arası kaynak paylaşımı (CORS) izin verilen çıkış noktaları işlevlerinizi uzak istekler kabul edebilir, "beyaz liste" tanımlamanıza olanak işlevlerini destekler.  |

![İşlev uygulaması'nın yapılandırma CORS](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-cors.png)

### <a name="auth"></a>Kimlik doğrulaması

| | |
|-|-|
| ![Azure portalında işlev uygulaması kimlik](./media/functions-how-to-use-azure-function-app-settings/function-app-authentication.png) | İşlevleri HTTP tetikleyicisi kullandığınızda, ilk kimlik doğrulaması için çağrılar gerektirebilir. App Service, Azure Active Directory kimlik doğrulaması ve oturum açın. Facebook, Microsoft ve Twitter gibi sosyal sağlayıcılardan destekler. Özel kimlik doğrulama sağlayıcılarını yapılandırma hakkında daha fazla bilgi için bkz [Azure App Service kimlik doğrulamasına genel bakış](../app-service/overview-authentication-authorization.md). |

![Bir işlev uygulaması için kimlik doğrulamasını yapılandırma](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-authentication.png)


### <a name="swagger"></a>API tanımı

| | |
|-|-|
| ![İşlev uygulaması API'si swagger tanımı Azure portalında](./media/functions-how-to-use-azure-function-app-settings/function-app-api-definition.png) | İşlevler, istemcilerin HTTP ile tetiklenen işlevlerinizi daha kolay kullanmasına izin vermek için Swagger destekler. Swagger API tanımlarını oluşturma hakkında daha fazla bilgi için ziyaret [Azure App Service'te CORS ile RESTful API barındırma](../app-service/app-service-web-tutorial-rest-api.md). İşlev proxy'leri, birden çok işlevler için tek bir API yüzeyi tanımlamak için de kullanabilirsiniz. Daha fazla bilgi için [Azure işlev proxy'lerini ile çalışma](functions-proxies.md). |

![İşlev uygulaması'nın API'yi yapılandırma](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-apidef.png)



## <a name="next-steps"></a>Sonraki adımlar

+ [Azure App Service ayarlarını yapılandırma](../app-service/configure-common.md)
+ [Azure İşlevleri için sürekli dağıtım](functions-continuous-deployment.md)



