---
title: İşletim sistemi ve uyumu - Azure App Service düzeltme çalışma zamanı | Microsoft Docs
description: Azure App Service güncelleştirmeler işletim sistemi ve çalışma zamanları ve nasıl alabileceğiniz duyuruları güncelleştirme nasıl açıklar.
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 576627c96b19dd3563ab21a5d478b779e4a3ed64
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60839003"
---
# <a name="os-and-runtime-patching-in-azure-app-service"></a>İşletim sistemi ve Azure App Service'te çalışma zamanı düzeltme eki uygulama

Bu makalede, yazılım ve işletim sistemi ile ilgili belirli sürüm bilgilerini alma gösterir [App Service](overview.md). 

App Service, bir hizmet olarak Platform-a-işletim sistemi ve uygulama yığın anlamına, sizin için Azure tarafından yönetilir yalnızca, uygulama ve verileri yönetin. Daha fazla denetim işletim sistemi ve uygulama stack kullanılabilir size [Azure sanal makineler](https://docs.microsoft.com/azure/virtual-machines/). Unutmayın, yine de daha fazla bilgi için aşağıdaki gibi bilmek bir App Service kullanıcı olarak sizin için yararlıdır:

-   İşletim sistemi güncelleştirmelerini nasıl ve ne zaman uygulanır?
-   App Service nasıl (örneğin, sıfır gün) önemli güvenlik açıklarına karşı yama?
-   İşletim sistemi ve çalışma zamanının hangi sürümlerinin uygulamalarınızı çalıştırıyorsunuz?

Güvenlik nedenleriyle, belirli güvenlik bilgilerinin ayrıntıları yayımlanmaz. Ancak, sorunları tarafından süreci hakkında en yüksek düzeyde saydamlık ve nasıl güvenlikle ilgili Duyurular veya çalışma zamanı güncelleştirmeler hakkında güncel kalmanız hafifletmek için makaleyi amaçlar.

## <a name="how-and-when-are-os-updates-applied"></a>İşletim sistemi güncelleştirmelerini nasıl ve ne zaman uygulanır?

Azure, iki düzeyi, fiziksel sunucuların ve Konuk App Service kaynaklarını çalışan sanal makineleri (VM'ler) işletim sistemi düzeltme eki uygulama yönetir. Her ikisi de, aylık olarak hizalar aylık olarak güncelleştirilir [Patch Tuesday](https://technet.microsoft.com/security/bulletins.aspx) zamanlama. Bu güncelleştirmeler, yüksek kullanılabilirlik SLA'sı, Azure hizmetlerini garanti eden bir şekilde otomatik olarak uygulanır. 

Güncelleştirmelerinin nasıl uygulanacağı hakkında ayrıntılı bilgi için bkz: [App Service işletim sistemi güncelleştirmelerini arkasında Sihirli Demystifying](https://blogs.msdn.microsoft.com/appserviceteam/2018/01/18/demystifying-the-magic-behind-app-service-os-updates/).

## <a name="how-does-azure-deal-with-significant-vulnerabilities"></a>Azure, önemli güvenlik açıkları ile nasıl dağıtılsın mı?

Önemli güvenlik açıkları ne zaman gerekli anında, gibi düzeltme eki uygulama [sıfır gün güvenlik açıklarını](https://wikipedia.org/wiki/Zero-day_(computing)), yüksek öncelikli güncelleştirmeler--olay bazında ele alınır.

Ziyaret ederek azure'da kritik güvenlik duyuruları ile yeniliklerden haberdar olun [Azure güvenlik blogu](https://azure.microsoft.com/blog/topics/security/). 

## <a name="when-are-supported-language-runtimes-updated-added-or-deprecated"></a>Ne zaman desteklenen dil çalışma zamanlarını güncelleştirildi, eklenen kullanım dışı mı?

Desteklenen dil çalışma zamanlarını kararlı yeni sürümlerini (birincil, ikincil veya düzeltme eki) App Service örneği için düzenli olarak eklenir. Diğer sürümlerini yan yana yüklenir ancak bazı güncelleştirmeler yüklemelerin üzerine yazın. Uygulamanızı otomatik olarak güncelleştirilen çalışma zamanında çalışan bir üzerine yazma yükleme anlamına gelir. Bir yan yana yüklemesi yeni bir çalışma zamanı sürümü avantajlarından faydalanmak için uygulamanızı el ile taşımanız gerektiğini anlamına gelir. Daha fazla bilgi için bir alt bölümlere bakın.

Çalıştırma zamanı güncelleştirmeleri ve bırakılanların burada bildirilir:

- https://azure.microsoft.com/updates/?product=app-service 
- https://github.com/Azure/app-service-announcements/issues

> [!NOTE] 
> Buradaki bilgiler, bir App Service uygulamasında yerleşik dil çalışma zamanlarını uygular. El ile yükseltmediğiniz sürece özel çalışma zamanı için App Service, karşıya yüklediğiniz bir örneğin değişmeden kalır.
>
>

### <a name="new-patch-updates"></a>Yeni bir düzeltme eki güncelleştirmeleri

.NET, PHP, Java SDK'sı veya Tomcat/Jetty sürüm için düzeltme eki güncelleştirmeleri, yeni sürümle yüklemelerin üzerine yazarak otomatik olarak uygulanır. Node.js düzeltme eki güncelleştirmeler (sonraki bölümde birincil ve ikincil sürüme benzer) sürümlerini yan yana yüklenir. Yeni Python düzeltme eki sürümleri el ile yüklenebilir [site uzantıları](https://www.siteextensions.net/packages?q=Tags%3A%22python%22)), yerleşik Python yüklemeleri ile yan yana.

### <a name="new-major-and-minor-versions"></a>Yeni birincil ve ikincil sürüme

Yeni bir birincil veya ikincil sürüm eklendiğinde, mevcut sürümleri ile yan yana yüklenir. Bu gibi durumlarda, uygulamanızı el ile yeni sürüme yükseltebilirsiniz. Çalışma zamanı sürümü bir yapılandırma dosyasında yapılandırılıp yapılandırılmadığını (gibi `web.config` ve `package.json`), aynı yöntemle yükseltmeniz gerekir. Bir App Service'ı kullandıysanız ayarını yapılandırmak, çalışma zamanı sürümü, bunu değiştirebilirsiniz [Azure portalında](https://portal.azure.com) veya çalıştırarak bir [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) komutunu [Cloud Shell](../cloud-shell/overview.md), olarak Aşağıdaki örneklerde gösterildiği:

```azurecli-interactive
az webapp config set --net-framework-version v4.7 --resource-group <groupname> --name <appname>
az webapp config set --php-version 7.0 --resource-group <groupname> --name <appname>
az webapp config appsettings set --settings WEBSITE_NODE_DEFAULT_VERSION=8.9.3 --resource-group <groupname> --name <appname>
az webapp config set --python-version 3.4 --resource-group <groupname> --name <appname>
az webapp config set --java-version 1.8 --java-container Tomcat --java-container-version 9.0 --resource-group <groupname> --name <appname>
```

### <a name="deprecated-versions"></a>Kullanım dışı sürümler  

Eski bir sürümü kullanım dışıdır, böylece, çalışma zamanı sürümü yükseltmesine uygun planı kaldırma tarihi duyurulur. 

## <a name="how-can-i-query-os-and-runtime-update-status-on-my-instances"></a>Nasıl miyim işletim sistemi ve çalışma zamanı güncelleştirme durumu Örneklerim üzerinde sorgulama yapabilirsiniz?  

Kritik işletim sistemi bilgileri aşağı erişimden kilitliyken (bkz [Azure App Service üzerindeki işletim sistemi işlevi](operating-system-functionality.md)), [Kudu konsolunu](https://github.com/projectkudu/kudu/wiki/Kudu-console) App Service örneğinizin işletim sistemi ile ilgili sorgu sağlar Sürüm ve çalışma zamanı sürümleri. 

Aşağıdaki tabloda nasıl uygulamalarınızı çalıştıran ve sürümleri için Windows dil çalışma zamanı:

| Bilgi | Nerede bulacağını | 
|-|-|
| Windows sürümü | Bkz: `https://<appname>.scm.azurewebsites.net/Env.cshtml` (altında sistem bilgisi) |
| .NET sürüm | Adresindeki `https://<appname>.scm.azurewebsites.net/DebugConsole`, komut isteminde aşağıdaki komutu çalıştırın: <br>`powershell -command "gci 'Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Net Framework Setup\NDP\CDF'"` |
| .NET core sürümü | Adresindeki `https://<appname>.scm.azurewebsites.net/DebugConsole`, komut isteminde aşağıdaki komutu çalıştırın: <br> `dotnet --version` |
| PHP sürümü | Adresindeki `https://<appname>.scm.azurewebsites.net/DebugConsole`, komut isteminde aşağıdaki komutu çalıştırın: <br> `php --version` |
| Varsayılan node.js sürümü | İçinde [Cloud Shell](../cloud-shell/overview.md), aşağıdaki komutu çalıştırın: <br> `az webapp config appsettings list --resource-group <groupname> --name <appname> --query "[?name=='WEBSITE_NODE_DEFAULT_VERSION']"` |
| Python sürümü | Adresindeki `https://<appname>.scm.azurewebsites.net/DebugConsole`, komut isteminde aşağıdaki komutu çalıştırın: <br> `python --version` |  

> [!NOTE]  
> Kayıt defteri konumuna erişim `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Component Based Servicing\Packages`burada bilgi ["Bb" düzeltme ekleri](https://docs.microsoft.com/security-updates/SecurityBulletins/securitybulletins) depolanır, kilitli.
>
>

## <a name="more-resources"></a>Diğer kaynaklar

[Güven Merkezi: Güvenlik](https://www.microsoft.com/en-us/trustcenter/security)  
[64-Azure App Service'te ASP.NET Core bit](https://gist.github.com/glennc/e705cd85c9680d6a8f1bdb62099c7ac7)
