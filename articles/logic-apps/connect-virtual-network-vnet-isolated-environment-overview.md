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
ms.date: 02/12/2019
ms.openlocfilehash: 204138e7b8b3846e2d50607b3c5ec0836abefe24
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56162382"
---
# <a name="access-to-azure-virtual-network-resources-from-azure-logic-apps-by-using-integration-service-environments-ises"></a>Tümleştirme service ortamları (ISEs) kullanarak Azure sanal ağ kaynakları için Azure Logic Apps gelen erişimi

> [!NOTE]
> Bu özellik bulunduğu *özel Önizleme*. Özel önizlemeye katılmak için [isteğiniz buraya oluşturma](https://aka.ms/iseprivatepreview).

Bazı durumlarda, logic apps ve tümleştirme hesapları sanal makineleri (VM'ler) ve diğer sistemler veya hizmetleri gibi güvenli kaynaklara erişmeye ihtiyacınız bir [Azure sanal ağı](../virtual-network/virtual-networks-overview.md). Bu erişimi ayarlamak için [oluşturmak bir *tümleştirme hizmeti ortamı* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment.md) logic apps ve tümleştirme hesapları çalıştırma.

![Tümleştirme hizmeti ortamı seçin](./media/connect-virtual-network-vnet-isolated-environment-overview/select-logic-app-integration-service-environment.png)

Bir işe oluşturma, Azure sanal ağına özel ve yalıtılmış bir Logic Apps örneği dağıtır. Bu özel örnek depolama gibi ayrılmış kaynaklarını kullanır ve genel "Genel" Logic Apps hizmetten ayrı olarak çalışır. Bu ayrım uygulamalarınızın performans, diğer Azure kiracılarında olabilecek etkisini azaltmak da yardımcı olur veya ["gürültülü komşu" etkili](https://en.wikipedia.org/wiki/Cloud_computing_issues#Performance_interference_and_noisy_neighbors). 

Bu genel bakış, nasıl bir işe logic apps sağlar ve tümleştirme hesapları doğrudan Azure sanal ağınız için erişim açıklar ve genel Logic Apps hizmeti ile bir işe arasındaki farklar karşılaştırır.

<a name="difference"></a>

## <a name="isolated-versus-global"></a>Genel karşı yalıtılmış

Azure'da bir tümleşik service ortamı (ISE) oluşturduğunuzda, istediğiniz Azure sanal ağı seçin *ekleme* , işe. Azure Logic Apps hizmetinin sanal ağınızda özel bir örneği dağıtır. Bu eylem, oluşturduğunuz ve ayrılmış kaynakları mantıksal uygulamalarınızı çalıştırmak için yalıtılmış bir ortam oluşturur. Mantıksal uygulama oluşturduğunuzda, mantıksal uygulama doğrudan erişim sağlayan, sanal ağınızda bulunan kaynaklar için uygulamanızın konumu olarak bu ortamı seçin.

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

> [!IMPORTANT]
> Logic apps, yerleşik Eylemler ve bağlayıcılar, ISE'de çalıştıran farklı bir fiyatlandırma planı, değil tüketim tabanlı fiyatlandırma planı kullanın. Daha fazla bilgi için [Logic Apps fiyatlandırma](../logic-apps/logic-apps-pricing.md).

<a name="vnet-access"></a>

## <a name="permissions-for-virtual-network-access"></a>Sanal ağ erişim izinleri

Bir tümleştirme hizmeti ortamı (ISE) oluşturduğunuzda, yeri bir Azure sanal ağı seçin, *ekleme* ortamınızı. Ekleme, sanal ağınızda özel bir Logic Apps hizmetinin örneğini dağıtır. Bu eylem, oluşturduğunuz ve ayrılmış kaynakları mantıksal uygulamalarınızı çalıştırmak için yalıtılmış bir ortam sonuçlanır. Oluşturduğunuz logic apps, işe uygulamalarınızın konum olarak seçin. Bu mantıksal uygulamalar daha sonra doğrudan sanal ağa erişmek ve bu ağdaki kaynaklara da bağlayın.

Bir sanal ağa bağlı sistemler için logic apps doğrudan bu sistemleri bu öğelerden herhangi birini kullanarak erişebilmesi için bu sanal ağa bir işe ekleyemezsiniz:

* Örneğin, SQL Server sistem için işe Bağlayıcısı

* HTTP eylemi

* Özel bağlayıcı

Bir sanal ağa bağlı olmayan veya işe bağlayıcılar yoksa şirket içi sistemler için bu sistemleri tarafından bağlanabilirsiniz [ayarlama ve şirket içi veri ağ geçidini kullanarak](../logic-apps/logic-apps-gateway-install.md).

Ortamınızı ekleme için bir Azure sanal ağı seçebilmek için sanal ağınızda Azure Logic Apps hizmeti için rol tabanlı erişim denetimi (RBAC) izinlerini ayarlamanız gerekir. Bu görev, sizin atamanızı gerektirir **ağ Katılımcısı** ve **Klasik Katılımcısı** Azure Logic Apps hizmetine rolleri.
Bu izinleri ayarlama hakkında bilgi için bkz: [mantıksal uygulamalar'ten Azure sanal ağlarına bağlanma](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#vnet-access)

<a name="create-integration-account-environment"></a>

## <a name="integration-accounts-with-ise"></a>ISE ile tümleştirme hesapları

Bir tümleştirme hizmeti ortamı (ISE) içinde logic apps ile tümleştirme hesapları kullanabilirsiniz. Ancak, bu tümleştirme hesapları kullanmanız gerekir *aynı işe* bağlı mantıksal uygulamalar. Logic apps'te bir işe aynı ISE'de olan tümleştirme hesapları başvurabilirsiniz. Tümleştirme hesabı oluşturduğunuzda, işe, tümleştirme hesabı için konumu olarak seçebilirsiniz.

## <a name="get-support"></a>Destek alın

* Sorularınız için <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps" target="_blank">Azure Logic Apps forumunu</a> ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için <a href="https://aka.ms/logicapps-wish" target="_blank">Logic Apps kullanıcı geri bildirimi sitesini</a> ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [yalıtılmış mantıksal uygulamalardan Azure sanal ağlarına bağlanma](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)
* Daha fazla bilgi edinin [Azure sanal ağ](../virtual-network/virtual-networks-overview.md)
* Hakkında bilgi edinin [Azure Hizmetleri için sanal ağ tümleştirmesi](../virtual-network/virtual-network-for-azure-services.md)
