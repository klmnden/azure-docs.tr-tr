---
title: include dosyası
description: include dosyası
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 06/05/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 1c2a4f1e463fff278981de2297662a94cca8944e
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34850823"
---
## <a name="prepare-your-repository"></a>Deponuzda hazırlama

Uygulama hizmeti Kudu Yapı sunucusundan otomatik derlemeleri almak için depo kök projenizdeki doğru dosyalar sahip olduğundan emin olun.

| Çalışma Zamanı | Kök dizin dosyaları |
|-|-|
| ASP.NET (yalnızca Windows) | _*.sln_, _*.csproj_, veya _default.aspx_ |
| ASP.NET Çekirdeği | _*.sln_ veya _*.csproj_ |
| PHP | _PHP için index.php'dir_ |
| Ruby (yalnızca Linux) | _Gemfile_ |
| Node.js | _Server.js_, _app.js_, veya _package.json_ başlangıç komut dosyası |
| Python (yalnızca Windows) | _\*.PY_, _requirements.txt_, veya _runtime.txt_ |
| HTML | _default.htm_, _default.html_, _default.asp_, _index.htm_, _index.html_, veya  _iisstart.htm_ |
| WebJobs | _\<job_name > / çalıştırın. \<uzantısı >_ altında _uygulama\_veri/işleri/sürekli_ (için sürekli Webjob'lar) veya _uygulama\_veri/işleri/tetiklenen_ (tetiklenir için Web işleri). Daha fazla bilgi için bkz: [Kudu Web işleri belgeleri](https://github.com/projectkudu/kudu/wiki/WebJobs) |
| İşlevler | Bkz: [Azure işlevleri için sürekli dağıtım](../articles/azure-functions/functions-continuous-deployment.md#continuous-deployment-requirements). |

Dağıtımınızı özelleştirmek için dahil bir _.deployment_ depo kök dosyasında. Daha fazla bilgi için bkz: [dağıtımlarını özelleştirme](https://github.com/projectkudu/kudu/wiki/Customizing-deployments) ve [özel dağıtım betiği](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script).

> [!NOTE]
> Visual Studio'da geliştiriyorsanız izin [Visual Studio sizin için bir havuz oluşturma](/vsts/git/tutorial/creatingrepo?view=vsts&tabs=visual-studio). Proje Git kullanarak dağıtılacak hemen hazırdır.
>
>

