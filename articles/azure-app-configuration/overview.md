---
title: Azure Uygulama Yapılandırması nedir | Microsoft Docs
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
ms.openlocfilehash: 0d78536173fd87e046a882314d6f94a92092fd9e
ms.sourcegitcommit: c712cb5c80bed4b5801be214788770b66bf7a009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57215889"
---
# <a name="what-is-azure-app-configuration"></a>Azure Uygulama Yapılandırması nedir

Azure uygulama yapılandırması, uygulama ayarlarını yönetmek için bir hizmet sağlar merkezi olarak. Özellikle de bir bulutta çalışan modern programlar genellikle doğası gereği dağıtılmış birçok bileşen sahiptir. Yapılandırma ayarlarını bu bileşenler genelinde yaymak bir uygulama dağıtımı sırasında sabit giderme hatalarına neden olabilir. Uygulama yapılandırması, uygulamanız için tüm ayarları depolar ve bunların erişim tek bir yerde güvenli olanak tanır.

Uygulama yapılandırma hizmetin içinde bulunduğu **özel Önizleme**. Önizleme dönemi boyunca ücretsiz olarak kullanılabilir. Lütfen [kaydetme](https://aka.ms/azconfig/register) denediğiniz yer ilgileniyorsanız Önizleme aşamasında.

## <a name="why-use-app-configuration"></a>Neden uygulama yapılandırmasını kullanma

Bulut tabanlı uygulamalar genellikle birden çok sanal makine ya da kapsayıcıları birden çok bölgede çalıştırın ve birden çok dış hizmetler kullanma. Dağıtılmış bir uygulama oluşturma, sağlam ve gerçek kolay ölçeklenebilir değildir. Bu uygulamalar oluşturmanın artan karmaşıklık ilgilenme geliştiricilerin yardımcı olmak için çeşitli programlama yöntemlerini ortaya çıkmıştır. Örneğin, birçok iyi sınanmış mimari desenleri ve bulut uygulamaları ile kullanmak için en iyi 12 Faktörlü uygulama ayrıntıları. Bu kılavuzda bir anahtar önerisi kod yapılandırmasından ayırmaktır. Bu uygulama yapılandırma ayarları gibi için çalıştırılabilir dışında tutulur ve kendi çalışma zamanı ortamı veya dış bir kaynaktan okunan anlamına gelir.

Herhangi bir uygulama yapabilirsiniz ancak bunu kullanmak, uygulama yapılandırmasını kullanmalıdır uygulama türleri, aşağıdakilere iyi örneklerdir:

* AKS, Service Fabric veya diğer kapsayıcı uygulamaları bir veya daha fazla coğrafi dağıtılmış mikro Hizmetleri temel.
* Azure işlevleri veya diğer durum bilgisi olmayan işlem olay temelli uygulamalar gibi sunucusuz uygulamalar.
* Sürekli dağıtım işlem hattı.

Uygulama yapılandırması aşağıdaki avantajları sunar:

* Dakikalar içinde ayarlanabilir tam olarak yönetilen bir hizmet.
* Esnek anahtar ifadeleri ve eşlemeleri.
* Etiketlerle etiketleme.
* Zaman içinde nokta yeniden yürütme ayarları.
* İki özel tanımlı boyut yapılandırmalarının karşılaştırması.
* Azure tarafından yönetilen kimlikleri ile gelişmiş güvenlik sağlar.
* Bekleyen veri veya Aktarımdaki verileri şifrelemeler tamamlayın.
* Popüler çerçeveleri ile yerel tümleştirme.

Uygulama yapılandırmasını tamamlar [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) uygulama parolalarını depolamak için kullanılır. Uygulama yapılandırması senaryolarda uygulama kolaylaştırır:

* Merkezi Yönetim ve dağıtım hiyerarşik yapılandırma verileri farklı ortamları ve coğrafyalar için.
* Bir uygulamayı yeniden başlatma veya yeniden dağıtmaya gerek olmadan dinamik yapılandırma değişiklikleri'ni kullanın.
* Özellik Yönetimi.

## <a name="how-to-use-app-configuration"></a>Uygulama yapılandırmasını kullanma hakkında

Bir uygulama yapılandırma deposu uygulamanız için bir istemci kitaplığı Microsoft eklemenin en kolay yolu sağlar. Programlama dili ve framework bağlı olarak, aşağıdakilere uygun olan en iyi yöntemler için Göster:

| Programlama dil ve çerçeve | Bağlanma |
|---|---|
| **.NET core** ve **ASP.NET Core** | .NET Core için uygulama yapılandırma yapılandırma sağlayıcısı. |
| **.NET** ve **ASP.NET** | .NET için uygulama yapılandırma yapılandırma Oluşturucu. |
| **Java Spring** | Spring Cloud için uygulama yapılandırma yapılandırma istemcisi. |
| Diğer | App Configuration REST API. |

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı Başlangıç: ASP.NET web uygulaması oluşturma](quickstart-aspnet-core-app.md)  
