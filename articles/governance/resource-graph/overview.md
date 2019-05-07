---
title: Azure Kaynak Grafiği'ne Genel Bakış
description: Uygun ölçekte kaynakların karmaşık sorgulama Azure kaynak Graph hizmeti nasıl sağladığını öğrenin.
author: DCtheGeek
ms.author: dacoulte
ms.date: 05/06/2019
ms.topic: overview
ms.service: resource-graph
manager: carmonm
ms.openlocfilehash: 45d5cf7c4235d10e136cc96364d52aa4319bbf79
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65137774"
---
# <a name="overview-of-the-azure-resource-graph-service"></a>Azure kaynak Graph hizmetine genel bakış

Azure Kaynak Grafiği etkili bir şekilde yönetmek için Azure kaynak sağlayarak Yönetimi verimli ve yüksek performanslı kaynak araştırma uygun ölçekte sorgu ile abonelikler arasında belirli bir kümesi genişletmek için tasarlanmış bir hizmet olduğundan, ortam. Bu sorgular aşağıdaki özellikleri sağlar:

- Karmaşık filtreleme, gruplandırma ve kaynak özelliklerine göre sıralama ile kaynakları sorgulama özelliği.
- Yinelemeli olarak idare gereksinimleri temel alarak kaynakları keşfetme olanağı.
- Uygulanan ilkenin geniş bir bulut ortamındaki etkisini değerlendirme özelliği.
- Olanağı [ayrıntı kaynak özelliklerine yapılan değişiklikler](./how-to/get-resource-changes.md) (Önizleme).

Bu belgede her özelliği ayrıntılı olarak inceleyeceksiniz.

> [!NOTE]
> Azure Kaynak Grafiği Azure portalın arama çubuğunda, 'Tüm kaynaklar' deneyimi yeni göz atma ve Azure İlkesi'nin güç katan [değişiklik geçmişini](../policy/how-to/determine-non-compliance.md#change-history-preview)
> _visual fark_. Bu, müşterilerin büyük ölçekli ortamlarda yönetmesine yardımcı olmak amacıyla tasarlanmıştır.

## <a name="how-does-resource-graph-complement-azure-resource-manager"></a>Kaynak Grafiği, Azure Resource Manager'ı nasıl tamamlar

Azure Resource Manager şu anda temel kaynak alanlarına yapılan sorguları destekleyen özellikle - kaynak adı kimliği, türü, kaynak grubu, abonelik ve konum. Resource Manager tesisleri ayrı ayrı kaynak sağlayıcıları, aynı anda ayrıntılı özellikler için bir kaynak çağırmak için de sunar.

Azure Kaynak Grafiği ile, her kaynak sağlayıcısına tek tek çağrı yapmanıza gerek kalmadan, kaynak sağlayıcılarının geri döndürdüğü bu özelliklere erişebilirsiniz. Desteklenen kaynak türleri listesi için Ara bir **Evet** içinde [tam modda dağıtımlar için kaynakları](../../azure-resource-manager/complete-mode-deletion.md) tablo.

Azure kaynak grafiği ile şunları yapabilirsiniz:

- Her kaynak sağlayıcısı için çağrıları tek tek yapmaya gerek kalmadan kaynak sağlayıcıları tarafından döndürülen özelliklerine erişin.
- Son 14 gün özellikleri nelerin değiştiğini görmek için bu kaynağa yapılan değişiklik geçmişini görüntüleme ve ne zaman. (önizleme)

## <a name="how-resource-graph-is-kept-current"></a>Nasıl Kaynak Grafiği geçerli tutulur

Bir Azure kaynak güncelleştirildiğinde Kaynak Grafiği değişikliği kaynak yöneticisi tarafından bildirilir.
Kaynak Grafiği, sonra veritabanını güncelleştirir. Kaynak Grafiği de yapar normal _tam tarama_. Bu tarama kaynak graf verileri eksik bildirimler veya bir kaynak Resource Manager dışında güncelleştirildiğinde durumunda geçerli olmasını sağlar.

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

Ücretsiz bir hizmet tüm müşteriler için en iyi deneyimi ve yanıt zamanı sağlamak için kaynak graf sorgularını azaltılır. Kuruluşunuz için büyük ölçekli ve sık kullanılan sorgular kaynak Graph API'sini kullanmak istiyorsa, Kaynak Grafiği sayfasından portal 'Geri' kullanın. İşinizin durumunu sağlayın ve sizinle iletişim kurmak takım için sırayla 'Microsoft, hakkındaki görüşlerinizi e-posta' onay kutusunu işaretleyin emin olun.

Kaynak Grafiği Kiracı düzeyinde kısıtlar. Hizmet geçersiz kılar ve ayarlar `x-ms-ratelimit-remaining-tenant-reads` kalan belirtmek için yanıt üst bilgisi sorgular kullanılabilir kiracıda bir kullanıcı tarafından. Kaynak Grafiği, kota her 5 saniyede bir yerine saatte sıfırlar. Daha fazla bilgi için [istekleri azaltma Resource Manager](../../azure-resource-manager/resource-manager-request-limits.md).

## <a name="running-your-first-query"></a>İlk sorgunuzu çalıştırma

Kaynak Grafiği, .NET için Azure CLI, Azure PowerShell ve Azure SDK'yı destekler. Sorgu yapılandırılmış her dil için aynı. [Azure CLI](first-query-azurecli.md#add-the-resource-graph-extension) ve [Azure PowerShell](first-query-powershell.md#add-the-resource-graph-module)’de Kaynak Grafiği’ni nasıl etkinleştireceğinizi öğrenin.

## <a name="next-steps"></a>Sonraki adımlar

- İlk Sorgunuzu çalıştırmak [Azure CLI](first-query-azurecli.md).
- İlk Sorgunuzu çalıştırmak [Azure PowerShell](first-query-powershell.md).
- İle başlayan [başlangıç sorguları](./samples/starter.md).
- Anlayışınızı ile geliştirmek [Gelişmiş sorgular](./samples/advanced.md).