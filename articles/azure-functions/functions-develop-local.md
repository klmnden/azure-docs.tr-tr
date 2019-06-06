---
title: Geliştirme ve Azure işlevleri yerel olarak çalıştırma | Microsoft Docs
description: Kod ve Azure işlevleri üzerinde çalıştırmadan önce yerel bilgisayarınızda Azure işlevlerini test etme hakkında bilgi edinin.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 1f430454bf994f9aac4ad6c113937f3798392319
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66492851"
---
# <a name="code-and-test-azure-functions-locally"></a>Kod ve Azure işlevleri yerel olarak test etme

Geliştirme ve Azure işlevleri'nde test ederken [Azure portal], birçok geliştiricinin tercih ettiğiniz bir yerel geliştirme deneyimi. İşlevler oluşturun ve işlevleri, yerel bilgisayarınızda test etmek için sık kullandığınız Kod Düzenleyicisi ve geliştirme araçlarınızı kullanın kolaylaştırır. Yerel işlevlerinizi Canlı Azure hizmetlerine bağlanabilir ve tam işlevler çalışma zamanı'nı kullanarak yerel bilgisayarınızda ayıklayabilirsiniz.

## <a name="local-development-environments"></a>Yerel geliştirme ortamları

İçinde geliştirdiğiniz işlevleri yerel bilgisayarınızda bir şekilde bağlıdır, [dil](supported-languages.md) ve araç tercihleri. Aşağıdaki tabloda ortamlarında yerel geliştirme destekler:

|Ortam                              |Languages         |Açıklama|
|-----------------------------------------|------------|---|
| [Komut istemi veya terminal](functions-run-local.md) | [C# (sınıf kütüphanesi)](functions-dotnet-class-library.md), [C# betiği (.csx)](functions-reference-csharp.md), [JavaScript](functions-reference-node.md) | [Azure işlevleri temel araçları] yerel geliştirme etkinleştiren işlevler oluşturmak için temel çalışma zamanı ve şablonlar sağlar. Sürüm 2.x Linux, MacOS ve Windows üzerinde geliştirme destekler. Tüm ortamlar için yerel işlevler çalışma zamanı Core araçları kullanır. |
|[Visual Studio Code](functions-create-first-function-vs-code.md)| [C# (sınıf kütüphanesi)](functions-dotnet-class-library.md), [C# betiği (.csx)](functions-reference-csharp.md), [JavaScript](functions-reference-node.md) | [VS Code için Azure işlevleri uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) işlevleri için VS Code desteği ekler. Core araçlarının yüklenmesini gerektirir. Geliştirme sürümü kullanırken, Linux, MacOS ve Windows, destekleyen 2.x çekirdek araçlar. Daha fazla bilgi için bkz. [Visual Studio Code kullanarak ilk işlevinizi oluşturma](functions-create-first-function-vs-code.md). |
| [Visual Studio 2019](functions-develop-vs.md) | [C# (sınıf kitaplığı)](functions-dotnet-class-library.md) | Azure işlevleri araçları dahil **Azure geliştirme** iş yükünü [Visual Studio 2019](https://www.visualstudio.com/vs/) ve sonraki sürümler. Bir sınıf kitaplığı'nda işlevler derlemek ve DLL'i Azure'a yayımlama sağlar. Yerel test etmek için temel araçlar içerir. Daha fazla bilgi için bkz. [geliştirme Visual Studio kullanarak Azure işlevleri](functions-develop-vs.md). |
| [Maven](functions-create-first-java-maven.md) (çeşitli) | [Java](functions-reference-java.md) | Java işlevleri geliştirme etkinleştirmek için çekirdek Araçlar ile tümleştirilir. Sürüm 2.x Linux, MacOS ve Windows üzerinde geliştirme destekler. Daha fazla bilgi için bkz. [Java ve Maven ile ilk işlevinizi oluşturma](functions-create-first-java-maven.md). Geliştirme kullanarak da destekler [Eclipse](functions-create-maven-eclipse.md) ve [Intellij Idea](functions-create-maven-intellij.md) |

[!INCLUDE [Don't mix development environments](../../includes/functions-mixed-dev-environments.md)]

Bu yerel geliştirme ortamların her birinde işlevi uygulaması projeleri oluşturmak ve yeni işlevler oluşturmak için önceden tanımlanmış işlevleri şablonları kullanmanıza olanak tanır. Her temel araçları kullanır, böylece test edebilir ve diğer uygulamalarda olduğu gibi işlevlerinizi gerçek işlevler çalışma zamanı karşı kendi makinenizde hata ayıklayın. Ayrıca yayımladığınız herhangi Bu ortamların Azure işlev uygulaması projesi.  

## <a name="next-steps"></a>Sonraki adımlar

+ Yerel geliştirme hakkında daha fazla bilgi edinmek için derlenmiş C# işlevleri kullanarak Visual Studio 2019 [geliştirme Visual Studio kullanarak Azure işlevleri](functions-develop-vs.md).
+ VS Code kullanarak Mac, Linux veya Windows bilgisayarda işlevlerinin yerel geliştirme hakkında daha fazla bilgi için bkz: [Azure işlevleri için VS Code belgeleri](https://docs.microsoft.com/azure/azure-functions/tutorial-javascript-vscode-get-started).
+ Komut istemi veya terminal işlevleri geliştirme hakkında daha fazla bilgi için bkz. [iş ile Azure işlevleri çekirdek Araçları](functions-run-local.md).

<!-- LINKS -->

[Azure işlevleri temel araçları]: https://www.npmjs.com/package/azure-functions-core-tools
[Azure portal]: https://portal.azure.com 
[Node.js]: https://docs.npmjs.com/getting-started/installing-node#osx-or-windows
