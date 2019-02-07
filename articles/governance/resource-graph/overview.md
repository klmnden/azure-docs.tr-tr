---
title: Azure Kaynak Grafiği'ne Genel Bakış
description: Azure Kaynak Grafiği, büyük ölçekteki kaynaklarda karmaşık sorgular çalıştırılmasını sağlayan bir Azure hizmetidir.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 02/06/2019
ms.topic: overview
ms.service: resource-graph
manager: carmonm
ms.openlocfilehash: 6b3bad4e4619f8909f5c6d71111b4fad9ddb3098
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55813288"
---
# <a name="what-is-azure-resource-graph"></a>Azure Kaynak Grafiği nedir

Azure Kaynak Grafiği, ortamınızı etkili bir biçimde idare edebilmeniz için tüm abonelik ve yönetim grupları ölçeğinde sorgu özelliğiyle verimli ve performanslı kaynak keşfi sağlayarak Azure Kaynak Yönetimi’ni genişletmek için tasarlanmış, Azure’deki bir hizmettir. Bu sorgular aşağıdaki özellikleri sağlar:

- Karmaşık filtreleme, gruplandırma ve kaynak özelliklerine göre sıralama ile kaynakları sorgulama özelliği.
- Kaynakları idare gereksinimlerine göre yinelemeli keşfetme ve ortaya çıkan ifadeyi bir ilke tanımına dönüştürme özelliği.
- Uygulanan ilkenin geniş bir bulut ortamındaki etkisini değerlendirme özelliği.

Bu belgede her özelliği ayrıntılı olarak inceleyeceksiniz.

> [!NOTE]
> Azure Kaynak Grafiği, Azure portalın yeni ‘Tüm kaynaklara’ gözat deneyimi tarafından kullanılır. Büyük ölçekli ortamları yönetme ihtiyacı olan müşterilere yardımcı olmak için tasarlanmıştır.

## <a name="how-does-resource-graph-complement-azure-resource-manager"></a>Kaynak Grafiği, Azure Resource Manager'ı nasıl tamamlar

Azure Resource Manager şu anda verileri, özellikle kaynak adı, Kimlik, Tür, Kaynak Grubu, Abonelikler ve Konum olmak üzere bazı kaynak alanlarını kullanıma sunan sınırlı bir kaynak önbelleğine gönderir. Eskiden farklı kaynak özellikleriyle çalışmak isteseydiniz, her kaynak sağlayıcısına çağrı yapmanız ve her kaynağın özellik ayrıntılarını istemeniz gerekirdi.

Azure Kaynak Grafiği ile, her kaynak sağlayıcısına tek tek çağrı yapmanıza gerek kalmadan, kaynak sağlayıcılarının geri döndürdüğü bu özelliklere erişebilirsiniz.

## <a name="the-query-language"></a>Sorgu dili

Artık Azure Kaynak Grafiği’nin ne olduğuna dair daha iyi bir anlayışa sahip olduğunuza göre, sorguların nasıl oluşturulacağına bakalım.

Azure Kaynak Grafiği’nin sorgu dilinin [Azure Veri Gezgini Sorgu Dili](../../data-explorer/data-explorer-overview.md)’ne dayalı olduğunu anlamak önemlidir.

Önce, Azure Kaynak Grafiği ile kullanılabilecek işlemler ve işlevler hakkında ayrıntılar için, bkz. [Kaynak Grafiği sorgu dili](./concepts/query-language.md). Kaynakları göz atmak için, bkz, [kaynakları keşfedin](./concepts/explore-resources.md).

## <a name="permissions-in-azure-resource-graph"></a>Azure Kaynak Grafiği’nde izinler

Kaynak Grafı’nı kullanmak için, sorgulamak istediğiniz kaynaklara en az okuma erişimi olan [Rol tabanlı erişim denetimi](../../role-based-access-control/overview.md) (RBAC) kapsamında uygun izinlere sahip olmanız gerekir. Sonuç döndürülmesi için Azure nesnesinde veya nesne grubunda en azından `read` iznine sahip olmanız gerekir.

## <a name="throttling"></a>Azaltma

Tüm müşteriler için en iyi deneyimi ve yanıt zamanı sağlamak için kaynak graf sorgularını kısıtlanmış. Lütfen kuruluşunuz büyük ölçekli ve sık kullanılan sorgular için kaynak Graph API'sini kullanmak isterse, Kaynak Grafiği sayfasından portal 'Geri' kullanın. İşinizin durumunu sağlayın ve sizinle iletişim kurmak takım için sırayla 'Microsoft, hakkındaki görüşlerinizi e-posta' onay kutusunu işaretleyin emin olun.

## <a name="running-your-first-query"></a>İlk sorgunuzu çalıştırma

Kaynak Grafiği hem Azure CLI’yi hem de Azure PowerShell’i destekler. Sorgu, iki dil için de aynı şekilde yapılandırılır. [Azure CLI](first-query-azurecli.md#add-the-resource-graph-extension) ve [Azure PowerShell](first-query-powershell.md#add-the-resource-graph-module)’de Kaynak Grafiği’ni nasıl etkinleştireceğinizi öğrenin.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure CLI](first-query-azurecli.md) ile ilk sorgunuzu çalıştırma
- [Azure PowerShell](first-query-powershell.md) ile ilk sorgunuzu çalıştırma
- [Başlangıç Sorguları](./samples/starter.md) ile başlama
- [Gelişmiş Sorgular](./samples/advanced.md) ile anlayışınızı geliştirme