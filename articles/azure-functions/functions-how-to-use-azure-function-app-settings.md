---
title: "Azure işlev uygulaması ayarlarını yapılandır | Microsoft Docs"
description: "Azure işlevi uygulama ayarlarını yapılandırmayı öğrenin."
services: 
documentationcenter: .net
author: ggailey777
manager: cfowler
editor: 
ms.assetid: 81eb04f8-9a27-45bb-bf24-9ab6c30d205c
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 04/23/2017
ms.author: glenga
ms.openlocfilehash: a6cfcd939cb0f21d01fe849ef04619ec9c1c972a
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="how-to-manage-a-function-app-in-the-azure-portal"></a>Azure portalında bir işlev uygulaması yönetme 

Azure işlevleri, bir işlev uygulaması, tek tek işlevleri için yürütme bağlamı sağlar. Verilen işlevi uygulama tarafından barındırılan tüm işlevler için işlev uygulama davranışları uygulayın. Bu konu, yapılandırmak ve işlev uygulamalarınızı Azure portalında yönetmek açıklar.

Başlamak için Git [Azure portal](http://portal.azure.com) ve Azure hesabınızda oturum açın. Portalın en üstündeki arama çubuğunda, işlev uygulamanızın adını yazın ve uygulamayı listeden seçin. İşlev uygulamanızı seçtikten sonra aşağıdaki sayfaya bakın:

![Azure portalında işlevi uygulama genel bakış](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-main.png)

## <a name="manage-app-service-settings"></a>İşlev uygulama ayarları sekmesi

![Azure portalında işlevi uygulama genel bakış.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-settings-tab.png)

**Ayarları** sekme kullanılabilir işlevi uygulamanız tarafından kullanılan işlevler çalışma zamanı sürümü burada güncelleştirebilirsiniz. Ayrıca, işlev uygulaması tarafından barındırılan tüm işlevleri HTTP erişimi kısıtlamak için kullanılan ana bilgisayar anahtarları yöneteceğiniz unutulmamalıdır.

İşlevler tüketim barındırma ve App Service planları barındırma destekler. Daha fazla bilgi için bkz: [Azure işlevleri için doğru hizmet planını seçin](functions-scale.md). Tüketim planında daha iyi öngörülebilirlik için işlevleri sağlar, günlük kullanım kotası gigabayt saniye olarak ayarlayarak platform kullanımını sınırla. Günlük kullanım kotası ulaşıldığında, işlev uygulaması durdurulur. Harcama kota ulaştıktan sonucunda durduruldu bir işlev uygulaması kota harcama günlük oluşturma olarak aynı bağlamından yeniden etkinleştirilebilir. Bkz: [Azure fiyatlandırma sayfası işlevleri](http://azure.microsoft.com/pricing/details/functions/) faturalama hakkında ayrıntılı bilgi için.   

## <a name="platform-features-tab"></a>Platform Özellikleri sekmesi

![İşlev uygulama platformu Özellikleri sekmesi.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-features-tab.png)

İşlev uygulamalarının çalıştırın ve Azure App Service platformu tarafından korunur. Bu nedenle, işlevi uygulamalarınızı Azure'nın çekirdek web barındırma platformu özelliklerin çoğunu erişimi. **Platform özellikleri** sekme kullanılır, işlevi uygulamalarınızda kullanabileceğiniz App Service platformu özelliklerinin çoğu eriştiği. 

> [!NOTE]
> Barındırma planı tüketimi bir işlev uygulaması çalıştığında, tüm uygulama hizmeti özellikleri kullanılabilir.

Bu konunun geri kalanında işlevleri için yararlı bir aşağıdaki uygulama hizmeti özellikleri Azure portalında odaklanır:

+ [Uygulama hizmeti Düzenleyicisi](#editor)
+ [Uygulama ayarları](#settings) 
+ [Console](#console)
+ [Gelişmiş araçlar (Kudu)](#kudu)
+ [Dağıtım seçenekleri](#deployment)
+ [CORS](#cors)
+ [Kimlik doğrulaması](#auth)
+ [API tanımı](#swagger)

Uygulama hizmeti ayarları ile çalışma hakkında daha fazla bilgi için bkz: [Azure App Service ayarlarını yapılandır](../app-service/web-sites-configure.md).

### <a name="editor"></a>Uygulama hizmeti Düzenleyicisi

| | |
|-|-|
| ![İşlev uygulaması App Service Düzenleyici.](./media/functions-how-to-use-azure-function-app-settings/function-app-appsvc-editor.png)  | Uygulama hizmeti Düzenleyici JSON yapılandırma dosyalarını ve kod dosyaları aynı şekilde değiştirmek için kullanabileceğiniz bir Gelişmiş bir portal düzenleyicisidir. Bu seçeneğin belirlenmesi temel bir düzenleyici olan ayrı bir tarayıcı sekmesi başlatır. Bu, Git deposu ile tümleştirme, çalıştırmak ve kodda hata ayıklama ve işlev uygulama ayarları değiştirmenize olanak sağlar. Bu düzenleyici varsayılan işlevi uygulaması dikey ile karşılaştırıldığında, İşlevler için bir Gelişmiş geliştirme ortamı sağlar.    |

![Uygulama hizmeti Düzenleyicisi](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-appservice-editor.png)

### <a name="settings"></a>Uygulama ayarları

| | |
|-|-|
| ![İşlev app uygulama ayarları.](./media/functions-how-to-use-azure-function-app-settings/function-app-application-settings.png) | Uygulama hizmeti **uygulama ayarları** dikey penceresinde, burada yapılandırın ve framework sürümleri, uzaktan hata ayıklama, uygulama ayarları ve bağlantı dizelerini Yönet. Diğer Azure ve üçüncü taraf hizmetleri ile işlevi uygulamanızı'i tümleştirdiğinizde burada bu ayarları değiştirebilirsiniz. |

![Uygulama ayarlarını yapılandırın](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-settings.png)

### <a name="console"></a>Konsol

| | |
|-|-|
| ![Azure portalında uygulama konsol işlevi](./media/functions-how-to-use-azure-function-app-settings/function-app-console.png) | Komut satırından işlevi uygulamanızı ile etkileşim kurmak tercih ettiğiniz zaman portala konsolu bir ideal Geliştirici aracıdır. Sık kullanılan komutlar dizin ve dosya oluşturma ve gezinti yanı toplu iş dosyaları ve komut dosyaları yürütme içerir. |

![Uygulama Konsolu işlevi](./media/functions-how-to-use-azure-function-app-settings/configure-function-console.png)

### <a name="kudu"></a>Gelişmiş araçlar (Kudu)

| | |
|-|-|
| ![Azure portalında işlev uygulaması Kudu](./media/functions-how-to-use-azure-function-app-settings/function-app-advanced-tools.png) | Uygulama Hizmeti (Kudu olarak da bilinir) için Gelişmiş araçlar işlevi uygulamanızın Gelişmiş yönetim özelliklerine erişim sağlar. Kudu sistem bilgileri, uygulama ayarları, ortam değişkenleri, site uzantıları, HTTP üstbilgileri ve sunucu değişkenlerine yönetin. Ayrıca başlatma **Kudu** gibi işlev uygulamanız için SCM uç noktasına giderek`https://<myfunctionapp>.scm.azurewebsites.net/` |

![Kudu yapılandırın](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-kudu.png)


### <a name="a-namedeploymentdeployment-options"></a><a name="deployment">Dağıtım seçenekleri

| | |
|-|-|
| ![Azure portalında işlevi uygulama dağıtım seçenekleri](./media/functions-how-to-use-azure-function-app-settings/function-app-deployment-source.png) | İşlevler işlevi kodunuzu yerel makinenizde geliştirmenize olanak sağlar. Azure için yerel bir işlev uygulaması projenize sonra karşıya yükleyebilirsiniz. Geleneksel FTP karşıya yükleme yanı sıra işlevler sağlar, GitHub, VSTS, Dropbox, Bitbucket ve diğerleri gibi popüler sürekli tümleştirme çözümleri kullanarak işlevi uygulamanızı dağıtma. Daha fazla bilgi için bkz: [Azure işlevleri için sürekli dağıtım](functions-continuous-deployment.md). FTP veya yerel Git kullanarak el ile karşıya yüklemek için ayrıca gerekir [dağıtım kimlik bilgilerinizi yapılandırma](functions-continuous-deployment.md#credentials). |


### <a name="cors"></a>CORS

| | |
|-|-|
| ![İşlev uygulaması CORS'yi Azure portalında](./media/functions-how-to-use-azure-function-app-settings/function-app-cors.png) | Kötü amaçlı kod yürütme hizmetlerinizde önlemek için dış kaynaklardan işlevi uygulamalarınızı çağrıları uygulama hizmeti engeller. Çıkış noktaları arası kaynak paylaşımı (CORS), izin verilen çıkış noktası işlevlerinizi Uzak istekleri kabul edebilir, "beyaz liste" tanımlamanıza olanak işlevlerini destekler.  |

![İşlev uygulamanın yapılandırma CORS](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-cors.png)

### <a name="auth"></a>Kimlik doğrulaması

| | |
|-|-|
| ![Azure portalında işlevi uygulama kimlik doğrulaması](./media/functions-how-to-use-azure-function-app-settings/function-app-authentication.png) | İşlevler bir HTTP tetikleyicisi kullandığınızda, ilk kimlik doğrulaması için çağrıları gerektirebilir. Uygulama hizmeti, Azure Active Directory kimlik doğrulaması ve oturum açın. Facebook, Microsoft ve Twitter gibi sosyal sağlayıcılardan destekler. Özel kimlik doğrulama sağlayıcılarını yapılandırma hakkında daha fazla bilgi için bkz: [Azure App Service kimlik doğrulamasına genel bakış](../app-service/app-service-authentication-overview.md). |

![Bir işlev uygulaması için kimlik doğrulamasını yapılandırma](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-authentication.png)


### <a name="swagger"></a>API tanımı

| | |
|-|-|
| ![İşlev uygulama API'si swagger tanımı Azure portalında](./media/functions-how-to-use-azure-function-app-settings/function-app-api-definition.png) | İstemcilerin HTTP tetiklemeli işlevleri daha kolay kullanmasına izin vermek için Swagger işlevlerini destekler. Swagger ile API tanımları oluşturma hakkında daha fazla bilgi için [API uygulamaları ve azure'da Swagger ile çalışmaya başlama](../app-service/app-service-web-tutorial-rest-api.md). İşlevler proxy'leri, birden çok işlevler için tek bir API yüzeyi tanımlamak için de kullanabilirsiniz. Daha fazla bilgi için bkz: [Azure işlevleri proxy'leri çalışma](functions-proxies.md). |

![İşlev uygulamanın API yapılandırın](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-apidef.png)



## <a name="next-steps"></a>Sonraki adımlar

+ [Azure App Service ayarlarını yapılandırma](../app-service/web-sites-configure.md)
+ [Azure İşlevleri için sürekli dağıtım](functions-continuous-deployment.md)



