---
title: Geliştirme ve Azure işlevleri yerel olarak çalıştırma | Microsoft Docs
description: Kod ve Azure işlevleri üzerinde çalıştırmadan önce yerel bilgisayarınızda Azure işlevlerini test etme hakkında bilgi edinin.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: ''
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: article
ms.date: 06/27/2018
ms.author: glenga
ms.openlocfilehash: e82981471306c9a9d09751da15256e85b770eb1a
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39125048"
---
# <a name="code-and-test-azure-functions-locally"></a>Kod ve Azure işlevleri yerel olarak test etme

Geliştirme ve Azure işlevleri'nde test ederken [Azure portal], birçok geliştiricinin tercih ettiğiniz bir yerel geliştirme deneyimi. İşlevler oluşturun ve işlevleri, yerel bilgisayarınızda test etmek için sık kullandığınız Kod Düzenleyicisi ve geliştirme araçlarınızı kullanın kolaylaştırır. Yerel işlevlerinizi Canlı Azure hizmetlerine bağlanabilir ve tam işlevler çalışma zamanı'nı kullanarak yerel bilgisayarınızda ayıklayabilirsiniz.

## <a name="local-development-environments"></a>Yerel geliştirme ortamları

İçinde geliştirdiğiniz işlevleri yerel bilgisayarınızda bir şekilde bağlıdır, [dil](supported-languages.md) ve araç tercihleri. Aşağıdaki tabloda ortamlarında yerel geliştirme destekler:

|Ortam                              |Diller         |Açıklama|
|-----------------------------------------|------------|---|
| [Komut istemi veya terminal](functions-run-local.md) | [C# (sınıf kütüphanesi)](functions-dotnet-class-library.md), [C# betiği (.csx)](functions-reference-csharp.md), [JavaScript](functions-reference-node.md), [Java](functions-reference-java.md) | [Azure işlevleri temel araçları] İşlevler, yerel geliştirme sağlayan oluşturmaya yönelik temel çalışma zamanı ve şablonlar sağlar. Sürüm 2.x Linux, MacOS ve Windows üzerinde geliştirme destekler. Tüm ortamlar için yerel işlevler çalışma zamanı Core araçları kullanır. |
|[Visual Studio Code](https://code.visualstudio.com/tutorials/functions-extension/getting-started)| [C# betiği (.csx)](functions-reference-csharp.md), [JavaScript](functions-reference-node.md) | [VS Code için Azure işlevleri uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) işlevleri için VS Code desteği ekler. Core araçlarının yüklenmesini gerektirir. Geliştirme sürümü kullanırken, Linux, MacOS ve Windows, destekleyen 2.x çekirdek araçlar. Daha fazla bilgi için bkz. [Azure işlevleri'ni kullanarak azure'a Dağıt](https://code.visualstudio.com/tutorials/functions-extension/getting-started).  |
| [Visual Studio 2017](functions-develop-vs.md) | [C# (sınıf kitaplığı)](functions-dotnet-class-library.md) | Azure işlevleri araçları dahil **Azure geliştirme** iş yükünü [Visual Studio 2017 sürüm 15.5](https://www.visualstudio.com/vs/) ve sonraki sürümler. Bir sınıf kitaplığı'nda işlevler derlemek ve DLL'i Azure'a yayımlama sağlar. Yerel test etmek için temel araçlar içerir. Daha fazla bilgi için bkz. [geliştirme Visual Studio kullanarak Azure işlevleri](functions-develop-vs.md). |
| [Maven](functions-create-first-java-maven.md) | [Java](functions-reference-java.md) | Java işlevleri geliştirme etkinleştirmek için çekirdek Araçlar ile tümleştirilir. Sürüm 2.x Linux, MacOS ve Windows üzerinde geliştirme destekler. Daha fazla bilgi için bkz. [Java ve Maven ile ilk işlevinizi oluşturma](functions-create-first-java-maven.md).|

[!INCLUDE [Don't mix development environments](../../includes/functions-mixed-dev-environments.md)]

Bu yerel geliştirme ortamların her birinde işlevi uygulaması projeleri oluşturmak ve yeni işlevler oluşturmak için önceden tanımlanmış işlevleri şablonları kullanmanıza olanak tanır. Her temel araçları kullanır, böylece test edebilir ve diğer uygulamalarda olduğu gibi işlevlerinizi gerçek işlevler çalışma zamanı karşı kendi makinenizde hata ayıklayın. Ayrıca yayımladığınız herhangi Bu ortamların Azure işlev uygulaması projesi.  

## <a name="next-steps"></a>Sonraki adımlar

+ Visual Studio 2017 kullanarak olan derlenmiş C# işlevlerinin yerel geliştirme hakkında daha fazla bilgi için bkz: [geliştirme Visual Studio kullanarak Azure işlevleri](functions-develop-vs.md).
+ VS Code kullanarak Mac, Linux veya Windows bilgisayarda işlevlerinin yerel geliştirme hakkında daha fazla bilgi için bkz: [Azure işlevleri için VS Code belgeleri](https://code.visualstudio.com/tutorials/functions-extension/getting-started).
+ Komut istemi veya terminal işlevleri geliştirme hakkında daha fazla bilgi için bkz. [iş ile Azure işlevleri çekirdek Araçları](functions-run-local.md).

<!-- LINKS -->

[Azure işlevleri temel araçları]: https://www.npmjs.com/package/azure-functions-core-tools
[Azure portal]: https://portal.azure.com 
[Node.js]: https://docs.npmjs.com/getting-started/installing-node#osx-or-windows
