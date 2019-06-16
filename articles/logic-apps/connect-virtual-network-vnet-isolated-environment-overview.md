---
title: Erişimden Azure sanal ağlarına Azure Logic Apps ile tümleştirme service ortamları (ISEs)
description: Bu genel bakış, tümleştirme service ortamları (ISEs) mantıksal uygulamaları Azure sanal ağları (Vnet) erişimi nasıl yardımcı açıklar.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 05/06/2019
ms.openlocfilehash: 1ef8c8eec3865f2a6e363e7da1dbda9504b81c05
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65546426"
---
# <a name="access-to-azure-virtual-network-resources-from-azure-logic-apps-by-using-integration-service-environments-ises"></a>Tümleştirme service ortamları (ISEs) kullanarak Azure sanal ağ kaynakları için Azure Logic Apps gelen erişimi

Bazı durumlarda, logic apps ve tümleştirme hesapları, sanal makineler (VM) gibi güvenli kaynaklara ve diğer sistemler veya içinde olan hizmetleri erişmesi gereken bir [Azure sanal ağı](../virtual-network/virtual-networks-overview.md). Bu erişimi ayarlamak için [oluşturmak bir *tümleştirme hizmeti ortamı* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment.md) , logic apps çalıştırın ve tümleştirmenizi oluşturmak hesaplar.

Bir işe oluşturduğunuzda, Azure Logic Apps hizmetinin, Azure sanal ağına bir özel ve ayrılmış örnek dağıtır. Bu özel örnek depolama gibi ayrılmış kaynaklarını kullanır ve genel "Genel" Logic Apps hizmetten ayrı olarak çalışır. Yalıtılmış özel Örneğinize ve ortak genel örnek ayırma da yardımcı olur, diğer Azure kiracılarında olarak da bilinen uygulamalarınızın performans üzerinde olabilir etkisini azaltmak ["gürültülü komşu" etkili](https://en.wikipedia.org/wiki/Cloud_computing_issues#Performance_interference_and_noisy_neighbors).

Mantıksal uygulama veya tümleştirme hesabınızı oluşturmak için çalışmaya başladığınızda, işe oluşturduktan sonra işe, mantıksal uygulama veya tümleştirme hesabınızın konumu olarak seçebilirsiniz:

![Tümleştirme hizmeti ortamı seçin](./media/connect-virtual-network-vnet-isolated-environment-overview/select-logic-app-integration-service-environment.png)

Mantıksal uygulamanız artık doğrudan içinde olduğunda veya bu öğelerden herhangi birini kullanarak uygulamanızı sanal ağınıza bağlı sistemler erişebilirsiniz:

* Bir **ISE**-Bağlayıcısı için SQL Server gibi sistem etiketli
* A **çekirdek**-yerleşik bir tetikleyici veya eylemi, HTTP tetikleyici veya eylemi gibi etiketli
* Özel bağlayıcı

Bir işe mantıksal uygulamalarınızı nasıl verir hakkında daha fazla ayrıntı bu genel bakış açıklar ve tümleştirme, Azure sanal ağa doğrudan erişim hesapları ve genel Logic Apps hizmeti ile bir işe arasındaki farklar karşılaştırır.

> [!NOTE]
> Logic apps, yerleşik tetikleyicileri, yerleşik Eylemler ve bağlayıcılar ISE kullanımınız fiyatlandırma planı tüketim tabanlı fiyatlandırma planından farklı çalıştır. Daha fazla bilgi için [Logic Apps fiyatlandırma](../logic-apps/logic-apps-pricing.md).

<a name="difference"></a>

## <a name="isolated-versus-global"></a>Genel karşı yalıtılmış

Azure'da bir tümleşik service ortamı (ISE) oluşturduğunuzda, istediğiniz Azure sanal ağı seçebilirsiniz *ekleme* , işe. Azure ekler veya dağıtır, Logic Apps hizmetinin sanal ağınızda özel bir örneği. Bu eylem, oluşturduğunuz ve ayrılmış kaynakları mantıksal uygulamalarınızı çalıştırmak için yalıtılmış bir ortam oluşturur. Mantıksal uygulamanızı oluşturduğunuzda, sanal ağınız ile ilgili ağ kaynakları için mantıksal uygulama doğrudan erişim sağlayan, uygulamanızın konumu olarak, işe seçin.

Logic apps'te bir işe genel Logic Apps hizmet olarak aynı kullanıcı deneyimleri ve benzer özellikleri sağlar. Yalnızca aynı yerleşik tetikleyicileri, yerleşik Eylemler ve bağlayıcılarından genel Logic Apps hizmetinin kullanabilirsiniz, ancak işe özel bağlayıcılar da kullanabilirsiniz. Örneğin, bir ISE'de çalıştıran sürümler sunduğu bazı standart bağlayıcılar şu şekildedir:

* Azure Blob Depolama, dosya depolama ve tablo depolama
* Azure kuyrukları, Azure Service Bus, Azure olay hub'ları ve IBM MQ
* FTP ve SFTP-SSH
* SQL Server, SQL veri ambarı, Azure Cosmos DB
* X 12 ve EDIFACT, AS2

ISE ve ISE olmayan bağlayıcıları arasındaki fark tetikleyiciler ve Eylemler çalıştırdığı konumlarda bulunur:

* ISE'de yerleşik tetikleyiciler ve HTTP gibi eylemler her zaman aynı işe mantıksal uygulamanızı ve görüntü olarak çalıştır **çekirdek** etiketi.

  !["Çekirdek" yerleşik Tetikleyicileri ve eylemleri seçin](./media/connect-virtual-network-vnet-isolated-environment-overview/select-core-built-in-actions-triggers.png)

* Herkese açık şekilde bir ISE'de çalışan bağlayıcılar sürümler genel Logic Apps hizmetinde kullanılabilir barındırılan. İki sürümü, Bağlayıcılarla teklif bağlayıcıların **ISE** her zaman aynı işe olarak mantıksal uygulamanızı çalıştırma etiketleyin. Bağlayıcılar olmadan **ISE** etiket genel Logic Apps hizmetinde çalıştırın.

  ![ISE bağlayıcıları seçme](./media/connect-virtual-network-vnet-isolated-environment-overview/select-ise-connectors.png)

### <a name="access-to-on-premises-data-sources"></a>Şirket içi veri kaynaklarına erişim

Logic apps bu öğelerden herhangi birini kullanarak bu sistemlerin doğrudan erişerek Azure sanal ağına bağlı şirket içi sistemler için bu ağa bir işe ekleme:

* Örneğin, SQL Server sistem için işe sürüm Bağlayıcısı
  
* HTTP eylemi
  
* Özel bağlayıcı

  * Şirket içi veri ağ geçidi gerektiren özel bağlayıcılara sahiptir ve söz konusu bağlayıcıların bir işe dışında oluşturduğunuz, bir işe logic apps'te de bu bağlayıcıları kullanabilirsiniz.
  
  * Bir ISE'de oluşturulan özel bağlayıcıları, şirket içi veri ağ geçidi ile çalışmaz. Ancak, bu bağlayıcıları doğrudan ISE barındırma sanal ağa bağlı şirket içi veri kaynaklarına erişebilir. Bu nedenle, logic apps'te bir işe büyük olasılıkla veri ağ geçidi bu kaynaklarla iletişim kurarken gerekmez.

Bir sanal ağa bağlı olmayan veya işe sürüm bağlayıcılar yoksa şirket içi sistemler için önce [şirket içi veri ağ geçidi ayarlama](../logic-apps/logic-apps-gateway-install.md) önce mantıksal uygulamalar bu sisteme bağlanabilirsiniz.

<a name="create-integration-account-environment"></a>

## <a name="integration-accounts-with-ise"></a>ISE ile tümleştirme hesapları

Bir tümleştirme hizmeti ortamı (ISE) içinde logic apps ile tümleştirme hesapları kullanabilirsiniz. Ancak, bu tümleştirme hesapları kullanmanız gerekir *aynı işe* bağlı mantıksal uygulamalar. Logic apps'te bir işe aynı ISE'de olan tümleştirme hesapları başvurabilirsiniz. Tümleştirme hesabı oluşturduğunuzda, işe, tümleştirme hesabı için konumu olarak seçebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [yalıtılmış mantıksal uygulamalardan Azure sanal ağlarına bağlanma](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)
* Daha fazla bilgi edinin [Azure sanal ağ](../virtual-network/virtual-networks-overview.md)
* Hakkında bilgi edinin [Azure Hizmetleri için sanal ağ tümleştirmesi](../virtual-network/virtual-network-for-azure-services.md)
