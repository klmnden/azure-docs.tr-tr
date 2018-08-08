---
title: Azure işlevleri çalışma zamanı sürümleri genel bakış
description: Azure işlevleri çalışma zamanı birden çok sürümünü destekler. Bunları sizin için doğru olanı seçme arasındaki farkları öğrenin.
services: functions
documentationcenter: ''
author: ggailey777
manager: cfowler
editor: ''
ms.service: functions
ms.workload: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2018
ms.author: glenga
ms.openlocfilehash: 6bf6621d650ad590cd1134bc79fcdecdc3fd0963
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39622645"
---
# <a name="azure-functions-runtime-versions-overview"></a>Azure işlevleri çalışma zamanı sürümleri genel bakış

 Azure işlevleri çalışma zamanı ana iki sürümü vardır: 1.x ve 2.x'i. Yalnızca 1.x üretim kullanımı için onaylanmış olması gerekir. Bu makalede, Önizleme aşamasında olan 2.x yenilikler açıklanmaktadır.

| Çalışma Zamanı | Durum |
|---------|---------|
|1.x|Genel kullanıma (GA)|
|2.x|Önizleme<sup>*</sup>|

<sup>*</sup>Sürüm önemli güncelleştirmeleri almak için Duyurular, izleme 2.x bozucu dahil olmak üzere, değişiklikleri [Azure App Service duyuruları](https://github.com/Azure/app-service-announcements/issues) depo.

> [!NOTE] 
> Bu makalede, Azure işlevleri bulut hizmetine başvurur. Azure işlevleri şirket içi çalıştırmanıza olanak tanıyan ürün hakkında daha fazla bilgi için bkz: [Azure işlevleri çalışma zamanına genel bakış](functions-runtime-overview.md).

## <a name="cross-platform-development"></a>Platformlar arası geliştirme

Çalışma zamanının 1.x destekler geliştirme ve yalnızca portalı veya Windows üzerinde barındırma. .NET Core, macOS ve Linux gibi .NET Core tarafından desteklenen tüm platformlarda çalıştırabilirsiniz anlamına çalışma zamanı 2.x çalışır. Bu, platformlar arası geliştirme ve 1.x ile mümkün olmayan barındırma senaryolarını etkinleştirir.

## <a name="languages"></a>Diller

Çalışma zamanı 2.x yeni bir dil genişletilebilirlik modeli kullanır. Başlangıçta, JavaScript ve Java bu yeni modelin avantajlarından edilmektedir. Azure işlevleri 1.x Deneysel dillerden 2.x'i desteklenmez bu nedenle yeni modeli kullanmak için güncelleştirilmedi. Aşağıdaki tablo, her çalışma zamanı sürümünde desteklenen programlama dilleri belirtir.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

Daha fazla bilgi için [desteklenen diller](supported-languages.md).

## <a name="bindings"></a>Bağlamalar 

Çalışma zamanı 2.x kullanan yeni bir [bağlama genişletilebilirlik modeli](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) , aşağıdaki avantajları sunar:

* Üçüncü taraf bağlama uzantıları için destek.
* Çalışma zamanı bağlamaları ve ayırma. Bu bağlama uzantıları tutulan ve bağımsız olarak yayımlanan olmasını sağlar. Örneğin, temel alınan bir SDK'ın daha yeni bir sürümüne bağımlı bir uzantı sürümünü yükseltmek için iyileştirilmiş olabilir.
* Bağlamaları kullanımda olduğu bilinen ve çalışma zamanı tarafından yüklenen bir açık yürütme ortamı.

Tüm yerleşik Azure işlevleri bağlamaları bu modeli benimseyen ve artık varsayılan olarak, Zamanlayıcı tetikleyicisi ve HTTP tetikleyicisi dışında yer alır. Bu uzantıları, Visual Studio gibi Araçlar üzerinden veya portal aracılığıyla işlevleri oluşturduğunuzda otomatik olarak yüklenir.

Aşağıdaki tabloda, hangi bağlamaları her çalışma zamanı sürümünde desteklendiğini gösterir.

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

## <a name="known-issues-in-2x"></a>2.x'de bilinen sorunlar

Bağlama desteğini ve diğer işlev eksiklikleri 2.x hakkında daha fazla bilgi için bkz. [çalışma zamanı bilinen sorunlar 2.0](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Azure-Functions-runtime-2.0-known-issues).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure İşlevleri’ni yerel olarak kodlama ve test etme](functions-run-local.md)
* [Azure işlevleri çalışma zamanı sürümlerini hedeflemek nasıl](set-runtime-version.md)
* [Sürüm notları](https://github.com/Azure/azure-functions-host/releases)