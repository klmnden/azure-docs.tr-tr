---
title: Azure Uygulama Yapılandırması nedir? | Microsoft Docs
description: Azure uygulama hizmetine genel bakış.
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 02/24/2019
ms.author: yegu
ms.openlocfilehash: 985845197f8a1ece76fe0a620f05194109f51bd6
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65408674"
---
# <a name="what-is-azure-app-configuration"></a>Azure Uygulama Yapılandırması nedir?

Azure uygulama yapılandırma merkezi olarak uygulama ayarlarını yönetme ve özellik bayrakları için bir hizmet sağlar. Özellikle bir bulutta çalışan programları modern programlar genellikle doğası gereği dağıtılmış birçok bileşen sahiptir. Yapılandırma ayarlarını bu bileşenler genelinde yaymak bir uygulama dağıtımı sırasında sabit giderme hatalarına neden olabilir. Uygulama yapılandırması, uygulamanız için tüm ayarları depolar ve tek bir yerde kendi erişim güvenliğini sağlamak için kullanın.

Uygulama yapılandırması, şu anda genel Önizleme aşamasındadır. Önizleme dönemi boyunca ücretsiz olarak kullanılabilir. İçinde için kaydolabilirsiniz [Azure portalında](https://portal.azure.com).

## <a name="why-use-app-configuration"></a>Uygulama yapılandırması neden kullanmalısınız?

Bulut tabanlı uygulamalar genellikle birden çok sanal makine ya da kapsayıcıları birden çok bölgede çalıştırın ve birden çok dış hizmetler kullanma. Dağıtılmış bir uygulama oluşturma, güçlü ve kolay ölçeklenebilir değildir.

Çeşitli programlama yöntemlerini uygulamaları oluşturma artan karmaşıklık baş geliştiriciler yardımcı olur. Örneğin, 12 Faktörlü uygulama birçok iyi sınanmış mimari desenleri ve bulut uygulamaları ile kullanmak için en iyi uygulamaları açıklar. Bu kılavuzda bir anahtar önerisi kod yapılandırmasından ayırmaktır. Bu durumda, uygulamanın yapılandırma ayarlarını, yürütülebilir dosyaya dışında tutulur ve kendi çalışma zamanı ortamı veya dış bir kaynaktan okunan.

Herhangi bir uygulama uygulama yapılandırmasını kullanma yapabilirsiniz, ancak aşağıdaki örnekler, kullanımından yararlanan uygulama türleri şunlardır:

* Azure Kubernetes hizmeti, Azure Service Fabric veya diğer kapsayıcı uygulamaları bir veya daha fazla coğrafi bölgelerde dağıtılan göre mikro hizmetler
* Azure işlevleri veya diğer durum bilgisi olmayan işlem olay temelli uygulamalar dahil sunucusuz uygulamalar
* Sürekli dağıtım işlem hattı

Uygulama yapılandırması aşağıdaki avantajları sunar:

* Dakikalar içinde ayarlanabilir tam olarak yönetilen bir hizmet
* Esnek anahtar ifadeleri ve eşlemeleri
* Etiketlerle etiketleme
* Zaman içinde nokta yeniden yürütme ayarları
* Özellik bayrağı yönetimi için ayrılmış kullanıcı Arabirimi
* İki özel tanımlı boyut yapılandırmalarının karşılaştırması
* Azure tarafından yönetilen kimlikleri ile Gelişmiş Güvenlik
* Tam veri şifrelemeleri, bekleyen veya aktarım
* Popüler çerçeveleri ile yerel tümleştirme

Uygulama yapılandırmasını tamamlar [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/), uygulama parolalarını depolamak için kullanılır. Uygulama yapılandırması aşağıdaki senaryolar uygulamak daha kolay hale getirir:

* Yönetim ve dağıtım hiyerarşik yapılandırma verileri farklı ortamları ve coğrafyalar için tek bir merkezden yönetin
* Dinamik olarak yeniden dağıtın veya bir uygulamayı yeniden başlatmanız gerek kalmadan uygulama ayarları değiştirme
* Gerçek zamanlı Denetim Özellik kullanılabilirliği

## <a name="use-app-configuration"></a>Uygulama yapılandırmasını kullanma

Bir uygulama yapılandırma deposu uygulamanız için bir istemci kitaplığı Microsoft eklemenin en kolay yolu sağlar. Programlama dili ve framework bağlı olarak, aşağıdaki en iyi yöntemleri kullanabilirsiniz.

| Programlama dil ve çerçeve | Bağlanma |
|---|---|
| .NET core ve ASP.NET Core | .NET Core için uygulama yapılandırma sağlayıcısı |
| .NET ve ASP.NET | .NET için uygulama yapılandırma Oluşturucu |
| Java Spring | Spring Cloud için uygulama yapılandırma istemcisi |
| Diğerleri | Uygulama yapılandırması REST API |

## <a name="next-steps"></a>Sonraki adımlar

* [ASP.NET Core hızlı başlangıç](./quickstart-aspnet-core-app.md)
* [.NET core hızlı başlangıç](./quickstart-dotnet-core-app.md)
* [.NET framework hızlı başlangıç](./quickstart-dotnet-app.md)
* [Azure işlevi hızlı başlangıç](./quickstart-azure-function-csharp.md)
* [Java Spring hızlı başlangıç](./quickstart-java-spring-app.md)
* [ASP.NET Core özellik bayrağı hızlı başlangıç](./quickstart-feature-flag-aspnet-core.md)
