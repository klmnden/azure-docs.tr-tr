---
title: Azure Kaynak Grafiği'ne Genel Bakış
description: Uygun ölçekte kaynakların karmaşık sorgulama Azure kaynak Graph hizmeti nasıl sağladığını öğrenin.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/30/2019
ms.topic: overview
ms.service: resource-graph
manager: carmonm
ms.openlocfilehash: d76a5b32403bd14f18181580f891925130808922
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60622805"
---
# <a name="overview-of-the-azure-resource-graph-service"></a>Azure kaynak Graph hizmetine genel bakış

Azure Kaynak Grafiği, ortamınızı etkili bir biçimde idare edebilmeniz için tüm abonelik ve yönetim grupları ölçeğinde sorgu özelliğiyle verimli ve performanslı kaynak keşfi sağlayarak Azure Kaynak Yönetimi’ni genişletmek için tasarlanmış, Azure’deki bir hizmettir. Bu sorgular aşağıdaki özellikleri sağlar:

- Karmaşık filtreleme, gruplandırma ve kaynak özelliklerine göre sıralama ile kaynakları sorgulama özelliği.
- Kaynakları idare gereksinimlerine göre yinelemeli keşfetme ve ortaya çıkan ifadeyi bir ilke tanımına dönüştürme özelliği.
- Uygulanan ilkenin geniş bir bulut ortamındaki etkisini değerlendirme özelliği.
- Olanağı [ayrıntı kaynak özelliklerine yapılan değişiklikler](./how-to/get-resource-changes.md) (Önizleme).

Bu belgede her özelliği ayrıntılı olarak inceleyeceksiniz.

> [!NOTE]
> Azure kaynak Graph, Azure portal'ın 'Tüm kaynaklar' deneyimi yeni göz atma ve Azure İlkesi'nin tarafından kullanılır [değişiklik geçmişini](../policy/how-to/determine-non-compliance.md#change-history-preview).
> _görsel fark_. Bu, müşterilerin büyük ölçekli ortamlarda yönetmesine yardımcı olmak amacıyla tasarlanmıştır.

## <a name="how-does-resource-graph-complement-azure-resource-manager"></a>Kaynak Grafiği, Azure Resource Manager'ı nasıl tamamlar

Azure Resource Manager şu anda verileri, özellikle kaynak adı, Kimlik, Tür, Kaynak Grubu, Abonelikler ve Konum olmak üzere bazı kaynak alanlarını kullanıma sunan sınırlı bir kaynak önbelleğine gönderir. Eskiden farklı kaynak özellikleriyle çalışmak isteseydiniz, her kaynak sağlayıcısına çağrı yapmanız ve her kaynağın özellik ayrıntılarını istemeniz gerekirdi.

Azure Kaynak Grafiği ile, her kaynak sağlayıcısına tek tek çağrı yapmanıza gerek kalmadan, kaynak sağlayıcılarının geri döndürdüğü bu özelliklere erişebilirsiniz. Desteklenen kaynak türleri listesi için Ara bir **Evet** içinde [tam modda dağıtımlar için kaynakları](../../azure-resource-manager/complete-mode-deletion.md) tablo.

Azure kaynak grafiği ile şunları yapabilirsiniz:

- Her kaynak sağlayıcısı için çağrıları tek tek yapmaya gerek kalmadan kaynak sağlayıcıları tarafından döndürülen özelliklerine erişin.
- Son 14 gün özellikleri nelerin değiştiğini görmek için bu kaynağa yapılan değişiklik geçmişini görüntüleme ve ne zaman. (önizleme)

## <a name="the-query-language"></a>Sorgu dili

Artık Azure Kaynak Grafiği’nin ne olduğuna dair daha iyi bir anlayışa sahip olduğunuza göre, sorguların nasıl oluşturulacağına bakalım.

Azure Kaynak grafiğin sorgu diline dayalı anlaşılması önemlidir [Kusto sorgu dili](../../data-explorer/data-explorer-overview.md) Azure Veri Gezgini tarafından kullanılır.

Önce, Azure Kaynak Grafiği ile kullanılabilecek işlemler ve işlevler hakkında ayrıntılar için, bkz. [Kaynak Grafiği sorgu dili](./concepts/query-language.md).
Kaynakları göz atmak için, bkz, [kaynakları keşfedin](./concepts/explore-resources.md).

## <a name="permissions-in-azure-resource-graph"></a>Azure Kaynak Grafiği’nde izinler

Kaynak Grafı’nı kullanmak için, sorgulamak istediğiniz kaynaklara en az okuma erişimi olan [Rol tabanlı erişim denetimi](../../role-based-access-control/overview.md) (RBAC) kapsamında uygun izinlere sahip olmanız gerekir. Sonuç döndürülmesi için Azure nesnesinde veya nesne grubunda en azından `read` iznine sahip olmanız gerekir.

> [!NOTE]
> Kaynak Grafiği, oturum açma sırasında bir asıl kullanılabilir abonelikleri kullanır. Etkin bir oturum sırasında eklenen yeni bir abonelik kaynaklarını görmek için asıl bağlam yenilemeniz gerekir. Bu eylem otomatik olarak oturumu kapatmak ve geri olur.

## <a name="throttling"></a>Azaltma

Tüm müşteriler için en iyi deneyimi ve yanıt zamanı sağlamak için kaynak graf sorgularını kısıtlanmış. Lütfen kuruluşunuz büyük ölçekli ve sık kullanılan sorgular için kaynak Graph API'sini kullanmak isterse, Kaynak Grafiği sayfasından portal 'Geri' kullanın. İşinizin durumunu sağlayın ve sizinle iletişim kurmak takım için sırayla 'Microsoft, hakkındaki görüşlerinizi e-posta' onay kutusunu işaretleyin emin olun.

## <a name="running-your-first-query"></a>İlk sorgunuzu çalıştırma

Kaynak Grafiği, .NET için Azure CLI, Azure PowerShell ve Azure SDK'yı destekler. Sorgu yapılandırılmış her dil için aynı. [Azure CLI](first-query-azurecli.md#add-the-resource-graph-extension) ve [Azure PowerShell](first-query-powershell.md#add-the-resource-graph-module)’de Kaynak Grafiği’ni nasıl etkinleştireceğinizi öğrenin.

## <a name="next-steps"></a>Sonraki adımlar

- İlk Sorgunuzu çalıştırmak [Azure CLI](first-query-azurecli.md).
- İlk Sorgunuzu çalıştırmak [Azure PowerShell](first-query-powershell.md).
- İle başlayan [başlangıç sorguları](./samples/starter.md).
- Anlayışınızı ile geliştirmek [Gelişmiş sorgular](./samples/advanced.md).