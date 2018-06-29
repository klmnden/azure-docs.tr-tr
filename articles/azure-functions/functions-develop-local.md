---
title: Geliştirme ve Azure işlevleri yerel olarak çalıştırma | Microsoft Docs
description: Kod ve Azure işlevlerini çalıştırmadan önce yerel bilgisayarınızda Azure işlevlerini test öğrenin.
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
ms.openlocfilehash: 89f94be4cf624914183480362ae23c62c363622f
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37088669"
---
# <a name="code-and-test-azure-functions-locally"></a>Yerel kod ve test Azure işlevleri

Geliştirmek ve Azure işlevlerini test ederken [Azure portal], çoğu geliştiricinin yerel geliştirme deneyimi tercih eder. İşlevler oluşturmak ve İşlevler, yerel bilgisayarınızda sınamak için sık kullanılan kod düzenleyicisi ve geliştirme araçları kullanmak kolaylaştırır. Yerel işlevlerinizi Azure Hizmetleri Canlı bağlanabilir ve tam işlevleri çalışma zamanı kullanarak, yerel bilgisayarınızda ayıklayabilirsiniz.

## <a name="local-development-environments"></a>Yerel geliştirme ortamları

Hangi geliştirme işlevleri, yerel bilgisayarınızda yolu bağlıdır, [dil](supported-languages.md) ve tercihleri araçları. Aşağıdaki tabloda ortamlarında yerel geliştirme destekler:

|Ortam                              |Diller         |Açıklama|
|-----------------------------------------|------------|---|
| [Komut istemi veya terminal](functions-run-local.md) | [C# betik (.csx)](functions-reference-csharp.md), [JavaScript](functions-reference-node.md) | [Azure işlevleri çekirdek Araçları] İşlevler, yerel geliştirme sağlayan oluşturmak için çekirdeği çalışma zamanı ve şablonlar sağlar. Linux, MacOS ve Windows sürümü 2.x destek geliştirme. Tüm ortamlar için yerel işlevler çalışma zamanı çekirdek araçları kullanır.|
|[Visual Studio Code](https://code.visualstudio.com/tutorials/functions-extension/getting-started)| [C# betik (.csx)](functions-reference-csharp.md), [JavaScript](functions-reference-node.md) | [VS Code için Azure işlevleri uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) işlevleri desteklemek için VS Code ekler. Çekirdek araçları gerektirir. Geliştirme sürümünü kullanırken, Linux, MacOS ve Windows, destekler çekirdek araçların 2.x. Daha fazla bilgi için bkz: [Azure işlevleri kullanarak Azure'a Dağıt](https://code.visualstudio.com/tutorials/functions-extension/getting-started).  |
| [Visual Studio 2017](functions-develop-vs.md) | [C# (sınıf kitaplığı)](functions-dotnet-class-library.md) | Azure işlevleri araçları içinde yer alan **Azure geliştirme** iş yükünü [Visual Studio 2017 sürüm 15,5](https://www.visualstudio.com/vs/) ve sonraki sürümler. Sınıf Kitaplığı'nda işlevleri derlemek ve .dll için Azure yayımlama sağlar. Yerel test etmek için çekirdek araçlarını içerir. Daha fazla bilgi için bkz: [Azure Visual Studio kullanarak işlevleri geliştirmek](functions-develop-vs.md). |
| [Maven](functions-create-first-java-maven.md) | [Java](functions-reference-java.md) | Geliştirme Java işlevlerini etkinleştirmek için çekirdek araçlarla tümleşir. Sürüm 2.x Linux, MacOS ve Windows geliştirme destekler. Daha fazla bilgi için bkz: [Java ve Maven ilk işlevinizi oluşturma](functions-create-first-java-maven.md).|

[!INCLUDE [Don't mix development environments](../../includes/functions-mixed-dev-environments.md)]

Bu yerel geliştirme ortamların her birinde, işlev uygulaması projeleri oluşturmak ve yeni işlevler oluşturmak için önceden tanımlanmış işlevleri şablonlarını kullanma olanak tanır. Böylece test edin ve başka bir uygulama gibi kendi makinede işlevlerinizi karşı gerçek işlevleri çalışma zamanı hata ayıklama her çekirdek araçları kullanır. Ayrıca yayımladığınız Azure herhangi birinden bu ortamları işlevi uygulama projesi.  

## <a name="next-steps"></a>Sonraki adımlar

+ Visual Studio 2017 kullanarak olan derlenmiş C# işlevlerin yerel geliştirme hakkında daha fazla bilgi için bkz: [Azure Visual Studio kullanarak işlevleri geliştirmek](functions-develop-vs.md).
+ Mac, Linux veya Windows bilgisayarda VS Code kullanma işlevlerin yerel geliştirme hakkında daha fazla bilgi için bkz: [Azure işlevleri için VS Code belgelere](https://code.visualstudio.com/tutorials/functions-extension/getting-started).
+ Komut istemi veya terminal işlevlerden geliştirme hakkında daha fazla bilgi için bkz: [Azure işlevleri çekirdek araçları ile çalışma](functions-run-local.md).

<!-- LINKS -->

[Azure işlevleri çekirdek araçları]: https://www.npmjs.com/package/azure-functions-core-tools
[Azure portal]: https://portal.azure.com 
[Node.js]: https://docs.npmjs.com/getting-started/installing-node#osx-or-windows
