---
title: Azure Logic Apps için Azure sanal ağ (VNET) erişim
description: Bu genel bakışta nasıl yalıtılmış mantıksal uygulamaları Azure sanal ağları (Vnet) özel ve ayrılmış kaynakları kullanan tümleştirme service ortamları (ISEs) bağlantı kurabilir gösterir
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 09/24/2018
ms.openlocfilehash: b012f1ac9c5f08a7e74871ca215299904f6b1deb
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46958654"
---
# <a name="access-to-azure-virtual-network-vnet-resources-from-isolated-azure-logic-apps"></a>Azure sanal ağ (VNET) kaynaklara erişim yalıtılmış Azure Logic Apps

> [!NOTE]
> Bu özellik bulunduğu *özel Önizleme*. Erişim talep etmek [burada katılma isteğiniz oluşturma](https://aka.ms/iseprivatepreview).

Bazı durumlarda, logic apps ve tümleştirme hesapları sanal makineler (VM) gibi güvenli kaynaklara ve diğer sistemler veya Hizmetleri içinde erişmesi gereken bir [Azure sanal ağ (VNET)](../virtual-network/virtual-networks-overview.md). Bu erişim sağlamak için [oluşturmak bir *tümleştirme hizmeti ortamı* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment.md) olarak logic apps ve tümleştirme hesapları oluşturmak için konum. 

![Tümleştirme hizmeti ortamı seçin](./media/connect-virtual-network-vnet-isolated-environment-overview/select-logic-app-integration-service-environment.png)

Bir işe oluşturma, sanal ağınızı bir özel ve yalıtılmış Logic Apps örneğinde dağıtır. Özel örnek depolama gibi ayrılmış kaynaklarını kullanır ve genel "Genel" Logic Apps hizmetten ayrı olarak çalışır. Bu ayrım uygulamalarınızın performans, diğer Azure kiracılarında olabilecek etkisini azaltmak da yardımcı olur veya ["gürültülü komşu" etkili](https://en.wikipedia.org/wiki/Cloud_computing_issues#Performance_interference_and_noisy_neighbors). 

Bu genel bakış nasıl bir işe oluşturma logic apps ve Azure VNET'İNİZE içindeki kaynaklara doğrudan tümleştirme hesapları yardımcı olduğunu açıklar ve genel Logic Apps hizmeti ile bir işe arasındaki farkları karşılaştırır.

<a name="difference"></a>

## <a name="isolated-versus-global"></a>Genel karşı yalıtılmış

Azure'da bir tümleşik service ortamı (ISE) oluşturduğunuzda, bir Azure sanal ağı olarak seçebileceğiniz bir *eş* ortamınız için. Azure Logic Apps hizmetinin özel bir örneğini oluşturduğunuz ve ayrılmış kaynakları mantıksal uygulamalarınızı çalıştırmak için yalıtılmış bir ortamda elde edilen sanal Ağınızdan uygulamasına dağıtır. Mantıksal uygulama oluşturduğunuzda, sanal ağınızdaki kaynaklara da mantıksal uygulama doğrudan erişim sağlayan, uygulamanızın konumu olarak bu ortamı seçebilirsiniz.  

Logic apps'te bir işe genel Logic Apps hizmet olarak aynı kullanıcı deneyimleri ve benzer özellikleri sağlar. Yalnızca aynı yerleşik olanları ve genel mantıksal uygulamalar hizmet tarafından sağlanan bağlayıcıları kullanabilirsiniz, ancak işe sürümleri sağlayan bağlayıcılar arasından seçim yapabilirsiniz. Örneğin, bir ISE'de çalıştıran sürümler sunduğu bazı standart bağlayıcılar şu şekildedir:
 
* Azure Blob Depolama, dosya depolama ve tablo depolama
* Azure Kuyrukları
* Azure Service Bus
* FTP
* SSH (SFTP) FTP
* SQL Server
* X 12 ve EDIFACT, AS2

ISE ve ISE olmayan bağlayıcıları arasındaki fark tetikleyiciler ve Eylemler çalıştırdığı konumlarda bulunur:

* Yerleşik tetikleyiciler ve HTTP gibi eylemleri, ISE'de kullanırsanız, bu tetikleyiciler ve Eylemler her zaman aynı işe olarak mantıksal uygulamanızı çalıştırın. 

* İki sürümü teklif bağlayıcıların: bir sürümünü çalıştıran bir ISE'de başka bir sürüm genel Logic Apps hizmetinde çalışırken.  

  Sahip bağlayıcıları **ISE** her zaman aynı işe olarak mantıksal uygulamanızı çalıştırma etiketleyin. Bağlayıcılar olmadan **ISE** etiket genel Logic Apps hizmetinde çalıştırın. 

  ![ISE bağlayıcıları seçme](./media/connect-virtual-network-vnet-isolated-environment-overview/select-ise-connectors.png)

* ISE'de yapılandırdığınız bağlayıcılar ayrıca genel Logic Apps hizmetinde kullanılabilir. 

> [!IMPORTANT]
> Logic apps, yerleşik Eylemler ve bağlayıcılar, ISE'de çalıştıran farklı bir fiyatlandırma planı, değil tüketim tabanlı fiyatlandırma planı kullanın. Daha fazla bilgi için [Logic Apps fiyatlandırma](../logic-apps/logic-apps-pricing.md).

<a name="vnet-access"></a>

## <a name="permissions-for-vnet-access"></a>Sanal ağ erişim izinleri

Bir tümleştirme hizmeti ortamı (ISE) oluşturduğunuzda, bir Azure sanal ağı (VNET) olarak seçebileceğiniz bir *eş* ortamınız için. Ancak, *yalnızca* bu ilişki oluşturma veya *eşlemesi*, işe oluşturduğunuzda. Bu ilişki işe bağlan, doğrudan sanal ağınızdaki kaynaklara ardından logic apps imkan tanıyan sanal ağınızdaki kaynakların, işe erişim sağlar. Bir işe için bağlı bir sanal ağ içindeki şirket içi sistemler için logic apps doğrudan bu sistemlerin bu öğelerden herhangi birini kullanarak erişebilir: 

* Örneğin, SQL Server sistem için işe Bağlayıcısı

* HTTP eylemi 

* Özel bağlayıcı

Sanal ağ içinde olmayan veya işe bağlayıcılar yoksa şirket içi sistemler için sonra bağlanmaya devam edebilirler [ayarlama ve şirket içi veri ağ geçidi kullanma](../logic-apps/logic-apps-gateway-install.md).

Ortamınız için bir Azure sanal ağı bir eş seçmeden önce Azure Logic Apps hizmeti için bir Azure sanal ağınızdaki rol tabanlı erişim denetimi (RBAC) izinlerini ayarlamanız gerekir. Bu görev, sizin atamanızı gerektirir **ağ Katılımcısı** ve **Klasik Katılımcısı** Azure Logic Apps hizmetine rolleri. Eşdüzey hizmet sağlama için gerekli rol izinleri hakkında daha fazla bilgi için bkz: [İzinler bölümünde oluşturma, değiştirme veya bir sanal ağ eşlemesini Sil](../virtual-network/virtual-network-manage-peering.md#permissions).

<a name="create-integration-account-environment"></a>

## <a name="integration-accounts-with-ise"></a>ISE ile tümleştirme hesapları

Bir tümleştirme hizmeti ortamı (ISE) çalıştıran logic apps ile tümleştirme hesaplarını kullanabilirsiniz, ancak bu tümleştirme hesapları da kullanmanız gerekir *aynı işe* bağlı mantıksal uygulamalar. Logic apps'te bir işe aynı ISE'de olan tümleştirme hesapları başvurabilirsiniz. Tümleştirme hesabı oluşturduğunuzda, işe, tümleştirme hesabı için konumu olarak seçebilirsiniz.

## <a name="get-support"></a>Destek alın

* Sorularınız için <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps" target="_blank">Azure Logic Apps forumunu</a> ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için <a href="http://aka.ms/logicapps-wish" target="_blank">Logic Apps kullanıcı geri bildirimi sitesini</a> ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi nasıl [yalıtılmış logic apps'ten sanal ağlar (Vnet'ler) bağlanma](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)
* Daha fazla bilgi edinin [Azure sanal ağ](../virtual-network/virtual-networks-overview.md)
* Hakkında bilgi edinin [Azure Hizmetleri için sanal ağ tümleştirmesi](../virtual-network/virtual-network-for-azure-services.md)
