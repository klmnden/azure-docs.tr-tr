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
ms.openlocfilehash: 2b9706241bd65ee75869d8b1fe58c6922d9e246a
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64951579"
---
## <a name="prepare-your-repository"></a>Deponuzu hazırlama

Azure App Service Kudu derleme sunucusundan otomatik derlemeler almak için depo kökünüzde projenize doğru dosya olduğundan emin olun.

| Çalışma Zamanı | Kök dizin dosyaları |
|-|-|
| ASP.NET (yalnızca Windows) | _*.sln_, _*.csproj_, veya _default.aspx_ |
| ASP.NET Çekirdeği | _*.sln_ veya _*.csproj_ |
| PHP | _index.php_ |
| Ruby (yalnızca Linux) | _Gemfile_ |
| Node.js | _Server.js_, _app.js_, veya _package.json_ bir başlangıç betiği ile |
| Python (yalnızca Windows) | _\*.PY_, _requirements.txt_, veya _runtime.txt_ |
| HTML | _default.htm_, _default.html_, _default.asp_, _index.htm_, _index.html_, veya  _iisstart.htm_ |
| WebJobs | _\<job_name > / çalıştırın. \<uzantısı >_ altında _uygulama\_veri/iş/continuous_ (için sürekli WebJobs) veya _uygulama\_veri/iş/triggered_ (tetiklenen için Web işleri). Daha fazla bilgi için [Kudu Web işleri belgeleri](https://github.com/projectkudu/kudu/wiki/WebJobs). |
| İşlevler | Bkz: [Azure işlevleri için sürekli dağıtım](../articles/azure-functions/functions-continuous-deployment.md#requirements-for-continuous-deployment). |

Dağıtımınızı özelleştirmek için dahil bir _.deployment_ depo köküne dosya. Daha fazla bilgi için [dağıtımlarını özelleştirme](https://github.com/projectkudu/kudu/wiki/Customizing-deployments) ve [özel dağıtım betiği](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script).

> [!NOTE]
> Visual Studio'da oluşturursanız, izin [Visual Studio sizin için bir depo oluşturma](/azure/devops/repos/git/creatingrepo?view=vsts&tabs=visual-studio). Projeyi hemen Git kullanarak dağıtılmaya hazırdır.
>
>

