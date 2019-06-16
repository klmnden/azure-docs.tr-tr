---
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 11/09/2018
ms.author: sngun
ms.openlocfilehash: 99dddd86c9348c9791d3012b382298bb020e63c9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66154583"
---
**1. Müşteriler, retiring SDK'sının nasıl bildirilecek?**

Microsoft, desteklenen bir SDK'sı için sorunsuz bir geçiş kolaylaştırmak için 12 aylık sağladığımız ön bildirimi desteğin sona ermesi için retiring SDK'sının sağlayacaktır. Ayrıca, müşteriler çeşitli iletişim kanalları – Azure Yönetim Portalı, Geliştirici Merkezi aracılığıyla Web günlüğü gönderisinde bildirilir ve doğrudan iletişim için atanan hizmet yöneticileri.

**2. Müşterilere 12 aylık dönem boyunca bir "yüklenecek" devre dışı bırakılan Azure Cosmos DB SDK'sını kullanarak uygulamaları yazabilirsiniz?** 

Evet, müşteriler yazar, dağıtmak ve 12 aylık yetkisiz kullanım süresi sırasında "yüklenecek" devre dışı bırakılan Azure Cosmos DB SDK'sını kullanan uygulamaları değiştirmek için tam erişim gerekir. Yetkisiz kullanım süresi 12 ay, müşteriler desteklenen Azure Cosmos DB SDK sürümü daha yeni uygun şekilde geçirmek için önerilir.

**3. Müşteriler yazabilir ve 12 aylık bildirim sonunda devre dışı bırakılan bir Azure Cosmos DB SDK kullanan uygulamaları değiştirmek mi?**

12 aylık bildirim süresinden sonra SDK'sı kullanımdan kaldırılacaktır. Azure Cosmos DB platformu tarafından herhangi bir Azure Cosmos DB erişimi devre dışı bırakılan bir SDK'sını kullanarak bir uygulama tarafından izin verilmez. Ayrıca, Microsoft müşteri desteği devre dışı bırakılan SDK'sı üzerinde sağlamaz.

**4. Müşterinin ne desteklenmeyen bir Azure Cosmos DB SDK sürümü kullanan uygulamaları çalıştıran?**

Devre dışı bırakılan bir SDK sürümü ile Azure Cosmos DB hizmetine bağlanmak için yapılan tüm denemeleri reddedilir. 

**5. Yeni özellikler ve İşlevler için tüm olmayan kullanımdan kaldırılacak SDKs uygulanır?**

Yeni özellikler ve işlevler yalnızca yeni sürümlere eklenecektir. SDK'ın eski, olmayan-kullanımdan kaldırıldı, bir sürümünü kullanıyorsanız, isteklerinizi Azure Cosmos DB için önceki gibi çalışmaya devam eder ancak tüm yeni özelliklere erişebilir değil.  

**6. Uygulamamı bir sonlandırma tarihinden önce güncelleştiremiyorsanız ne yapmalıyım?**

Son SDK'ın mümkün olduğunca erken yükseltmenizi öneririz. Bir SDK'yı devre dışı bırakılması için etiketlendikten 12 ay uygulamanızı güncelleştirmeniz gerekir. Herhangi bir nedenle, olamaz, bu zaman çerçevesi içinde uygulama güncelleştirmesi tamamlamanız ve ardından iletişim kurun, [Cosmos DB takım](mailto:askcosmosdb@microsoft.com) ve kesme tarihinden önce bunların Yardım isteyin.

