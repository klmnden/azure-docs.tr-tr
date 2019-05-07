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
ms.openlocfilehash: 0206fd2b2ea0a7cfaf79aaf19052e0174645780b
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65143148"
---
# <a name="access-to-azure-virtual-network-resources-from-azure-logic-apps-by-using-integration-service-environments-ises"></a>Tümleştirme service ortamları (ISEs) kullanarak Azure sanal ağ kaynakları için Azure Logic Apps gelen erişimi

Bazı durumlarda, logic apps ve tümleştirme hesapları sanal makineleri (VM'ler) ve diğer sistemler veya hizmetleri gibi güvenli kaynaklara erişmeye ihtiyacınız bir [Azure sanal ağı](../virtual-network/virtual-networks-overview.md). Bu erişimi ayarlamak için [oluşturmak bir *tümleştirme hizmeti ortamı* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment.md) logic apps ve tümleştirme hesapları çalıştırma. Bir işe oluşturduğunuzda, Azure Logic Apps hizmetinin, Azure sanal ağına bir özel ve ayrılmış örnek dağıtır. Bu özel örnek depolama gibi ayrılmış kaynaklarını kullanır ve genel "Genel" Logic Apps hizmetten ayrı olarak çalışır. Yalıtılmış özel Örneğinize ve ortak genel örnek ayırma da yardımcı olur, diğer Azure kiracılarında olarak da bilinen uygulamalarınızın performans üzerinde olabilir etkisini azaltmak ["gürültülü komşu" etkili](https://en.wikipedia.org/wiki/Cloud_computing_issues#Performance_interference_and_noisy_neighbors).

Mantıksal uygulama veya tümleştirme hesabınızı oluşturmak için çalışmaya başladığınızda, işe oluşturduktan sonra işe, mantıksal uygulama veya tümleştirme hesabınızın konumu olarak seçebilirsiniz:

![Tümleştirme hizmeti ortamı seçin](./media/connect-virtual-network-vnet-isolated-environment-overview/select-logic-app-integration-service-environment.png)

Mantıksal uygulamanız artık doğrudan içinde olduğunda veya bu öğelerden herhangi birini kullanarak uygulamanızı sanal ağınıza bağlı sistemler erişebilirsiniz:

* Örneğin, SQL Server sistem için bir işe sürümlü Bağlayıcısı
* Yerleşik bir tetikleyici veya eylemi, HTTP tetikleyici veya eylemi gibi
* Özel bağlayıcı

Bir işe mantıksal uygulamalarınızı nasıl verir hakkında daha fazla ayrıntı bu genel bakış açıklar ve tümleştirme, Azure sanal ağa doğrudan erişim hesapları ve genel Logic Apps hizmeti ile bir işe arasındaki farklar karşılaştırır.
Bir sanal ağa bağlı olmayan veya işe sürüm bağlayıcılar yoksa şirket içi sistemler için bu sistemleri tarafından bağlanabilirsiniz [ayarlama ve şirket içi veri ağ geçidini kullanarak](../logic-apps/logic-apps-gateway-install.md).

> [!IMPORTANT]
> Logic apps, yerleşik Eylemler ve bağlayıcılar, ISE'de çalıştıran farklı bir fiyatlandırma planı, değil tüketim tabanlı fiyatlandırma planı kullanın. Daha fazla bilgi için [Logic Apps fiyatlandırma](../logic-apps/logic-apps-pricing.md).

<a name="difference"></a>

## <a name="isolated-versus-global"></a>Genel karşı yalıtılmış

Azure'da bir tümleşik service ortamı (ISE) oluşturduğunuzda, istediğiniz Azure sanal ağı seçebilirsiniz *ekleme* , işe. Azure ekler veya dağıtır, Logic Apps hizmetinin sanal ağınızda özel bir örneği. Bu eylem, oluşturduğunuz ve ayrılmış kaynakları mantıksal uygulamalarınızı çalıştırmak için yalıtılmış bir ortam oluşturur. Mantıksal uygulamanızı oluşturduğunuzda, sanal ağınız ile ilgili ağ kaynakları için mantıksal uygulama doğrudan erişim sağlayan, uygulamanızın konumu olarak, işe seçin.

Logic apps'te bir işe genel Logic Apps hizmet olarak aynı kullanıcı deneyimleri ve benzer özellikleri sağlar. Yalnızca genel Logic Apps hizmetinde aynı yerleşik Eylemler ve bağlayıcılar kullanabilirsiniz, ancak işe özel bağlayıcılar da kullanabilirsiniz. Örneğin, bir ISE'de çalıştıran sürümler sunduğu bazı standart bağlayıcılar şu şekildedir:

* Azure Blob Depolama, dosya depolama ve tablo depolama
* Azure kuyrukları, Azure Service Bus, Azure olay hub'ları ve IBM MQ
* FTP ve SFTP-SSH
* SQL Server, SQL veri ambarı, Azure Cosmos DB
* X 12 ve EDIFACT, AS2

ISE ve ISE olmayan bağlayıcıları arasındaki fark tetikleyiciler ve Eylemler çalıştırdığı konumlarda bulunur:

* ISE'de yerleşik tetikleyiciler ve Eylemler HTTP gibi her zaman aynı işe olarak mantıksal uygulamanızı çalıştırın.

* Başka bir sürüm genel Logic Apps hizmetinde çalışırken iki sürümü teklif bağlayıcıları, bir ISE'de bir sürümü çalışır.  

  Sahip bağlayıcıları **ISE** her zaman aynı işe olarak mantıksal uygulamanızı çalıştırma etiketleyin. Bağlayıcılar olmadan **ISE** etiket genel Logic Apps hizmetinde çalıştırın.

  ![ISE bağlayıcıları seçme](./media/connect-virtual-network-vnet-isolated-environment-overview/select-ise-connectors.png)

* Bir ISE'de çalışan bağlayıcılar ayrıca genel Logic Apps hizmetinde kullanılabilir.

<a name="create-integration-account-environment"></a>

## <a name="integration-accounts-with-ise"></a>ISE ile tümleştirme hesapları

Bir tümleştirme hizmeti ortamı (ISE) içinde logic apps ile tümleştirme hesapları kullanabilirsiniz. Ancak, bu tümleştirme hesapları kullanmanız gerekir *aynı işe* bağlı mantıksal uygulamalar. Logic apps'te bir işe aynı ISE'de olan tümleştirme hesapları başvurabilirsiniz. Tümleştirme hesabı oluşturduğunuzda, işe, tümleştirme hesabı için konumu olarak seçebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [yalıtılmış mantıksal uygulamalardan Azure sanal ağlarına bağlanma](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)
* Daha fazla bilgi edinin [Azure sanal ağ](../virtual-network/virtual-networks-overview.md)
* Hakkında bilgi edinin [Azure Hizmetleri için sanal ağ tümleştirmesi](../virtual-network/virtual-network-for-azure-services.md)
