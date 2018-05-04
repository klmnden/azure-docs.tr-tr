---
title: Azure Search .NET Yönetim SDK sürüm 2 için yükseltme | Microsoft Docs
description: Azure Search .NET Yönetim SDK sürüm 2 için yükseltme
author: brjohnstmsft
manager: jlembicz
ms.author: brjohnst
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 01/15/2018
ms.openlocfilehash: a6b6c01f0cc811abca90139e4c26c27b7bd7119f
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="upgrading-to-the-azure-search-net-management-sdk-version-2"></a>Azure Search .NET Yönetim SDK sürüm 2 için yükseltme
Sürüm 1.0.2 kullanıyorsanız, eski veya [Azure Search .NET Yönetim SDK](https://aka.ms/search-mgmt-sdk), bu makalede, sürüm 2 kullanmak için uygulamanızı yükseltmenize yardımcı olur.

Azure Search .NET Yönetim SDK sürüm 2 önceki sürümlerinden bazı değişiklikler içerir. Kodunuzu değiştirmeden yalnızca en az çaba gerektirmelidir şekilde çoğunlukla ikincil, bunlar. Bkz: [yükseltme adımları](#UpgradeSteps) yeni SDK sürümü kullanmak kodunuzu değiştirmek konusunda yönergeler için.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-2"></a>Sürüm 2 yenilikler nelerdir?
Azure Search .NET Yönetim SDK sürüm 2 önceki SDK sürümleri olarak Azure arama yönetimi REST API aynı genel olarak kullanılabilir sürümünü özellikle 2015-08-19 hedefler. SDK SDK'ın kullanılabilirliğini artırmak için kesinlikle istemci-tarafı değişiklikleri değişir. Bu değişiklikler şunları içerir:

* `Services.CreateOrUpdate` ve zaman uyumsuz sürümleri artık otomatik olarak sağlama yoklaması `SearchService` ve hizmet sağlama işlemi tamamlanana kadar döndürmüyor. Bu tür yoklama kodu kendiniz yazmak zorunda kalmaktan kaydeder.
* Hizmeti el ile sağlama sorgulamak istiyorsanız, yeni kullanabilirsiniz `Services.BeginCreateOrUpdate` yöntemi veya zaman uyumsuz sürümlerinden biri.
* Yeni yöntemler `Services.Update` ve zaman uyumsuz sürümlere SDK eklenmiştir. Bu yöntemler, artımlı bir hizmeti güncelleştirme desteklemek için HTTP PATCH kullanın. Örneğin, artık bir hizmet geçirerek ölçeklendirmek bir `SearchService` yalnızca istenen içeren örneği için bu yöntemlere `partitionCount` ve `replicaCount` özellikleri. Eski yolunu çağırma `Services.Get`, döndürülen değiştirme `SearchService`ve onu için `Services.CreateOrUpdate` hala desteklenmektedir, ancak artık gerekli değildir. 

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Yükseltme adımları
İlk olarak, NuGet başvuru için güncelleştirme `Microsoft.Azure.Management.Search` NuGet Paket Yöneticisi konsolu kullanılarak veya göre proje başvuruları sağ tıklayıp Visual Studio'da "NuGet paketleri...'ı Yönet" seçerek.

NuGet yeni paketleri ve bunların bağımlılıklarını indirdikten sonra projenizi yeniden derleyin. Kodunuzu nasıl yapılandırıldığını bağlı olarak başarılı bir şekilde yeniden oluşturmak. Bu durumda, başlamaya hazırsınız!

Derleme başarısız olursa, değişmiş SDK arabirimleri (örneğin, amacı birim testi), bazıları uyguladık nedeni olabilir. Bu sorunu çözmek için yeni yöntemler gibi uygulama gerekecektir `BeginCreateOrUpdateWithHttpMessagesAsync`.

Derleme hataları düzelttik sonra isterseniz yeni işlevsellikten yararlanmak için uygulamanızı değişiklik yapabilirsiniz. SDK'sındaki yeni özellikleri ayrıntılı olarak [sürüm 2 yenilikler](#WhatsNew).

## <a name="conclusion"></a>Sonuç
SDK'bildiriminiz bizim. Sorunlarla karşılaşırsanız, bize yardım isteyin çekinmeyin [Azure arama MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch). Bir hata bulursanız, bir sorunun dosya [Azure .NET SDK GitHub deposunu](https://github.com/Azure/azure-sdk-for-net/issues). "[Azure Search]", sorunu başlık önek emin olun.

Azure Search kullandığınız için teşekkür ederiz!
