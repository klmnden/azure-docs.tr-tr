---
title: İşletim sistemi ve Azure App Service'te çalışma zamanı düzeltme eki uygulama | Microsoft Docs
description: Azure uygulama hizmeti güncelleştirmeler işletim sistemi ve çalışma zamanları ve nasıl erişebileceğini duyuruları güncelleştirme nasıl açıklar.
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
ms.openlocfilehash: 0626b958a9b822569f4d3b6d27f3395bed853174
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37030062"
---
# <a name="os-and-runtime-patching-in-azure-app-service"></a>İşletim sistemi ve Azure App Service'te çalışma zamanı düzeltme eki uygulama

Bu makalede, yazılım ve işletim sistemi ile ilgili belirli sürüm bilgilerini alma gösterilmektedir [uygulama hizmeti](app-service-web-overview.md). 

Uygulama hizmeti olan bir Platform-bir işletim sistemi ve uygulama yığın anlamına hizmet olarak, sizin için Azure tarafından; yönetilir yalnızca uygulamanız ve verileri yönetin. Daha fazla denetim yığını işletim sistemi ve uygulama kullanılabilir size [Azure sanal makineleri](https://docs.microsoft.com/azure/virtual-machines/). Unutmayın, ancak yine de daha fazla bilgi için aşağıdaki gibi öğrenmek için bir uygulama hizmeti kullanıcı olarak sizin için yararlıdır:

-   İşletim sistemi güncelleştirmelerini nasıl ve ne zaman uygulanır?
-   Nasıl (örneğin, sıfırıncı gün) önemli güvenlik açıklarına karşı uygulama hizmeti düzeltme?
-   Uygulamalarınızı hangi işletim sistemi ve çalışma zamanı sürümlerini çalıştırıyor?

Güvenlik nedenleriyle, belirli güvenlik bilgileri ayrıntılarını yayımlanmaz. Ancak, en yüksek düzeyde saydamlık işleme tarafından sorunları ve nasıl güvenlikle ilgili Duyurular veya çalışma zamanı güncelleştirmeleri güncel kalabileceği hafifletmek için makaleyi amaçlar.

## <a name="how-and-when-are-os-updates-applied"></a>İşletim sistemi güncelleştirmelerini nasıl ve ne zaman uygulanır?

Azure tarafından yönetilen iki düzey, fiziksel sunucuların ve Konuk App Service kaynakları çalışan sanal makineleri (VM'ler) işletim sistemi düzeltme eki uygulama. Her ikisi de, aylık hizalar aylık olarak güncelleştirilir [Patch Tuesday](https://technet.microsoft.com/security/bulletins.aspx) zamanlama. Bu güncelleştirmeler, otomatik olarak yüksek kullanılabilirlik SLA'sı, Azure Hizmetleri garanti bir biçimde uygulanır. 

Güncelleştirmeleri nasıl uygulandığını ile ilgili ayrıntılı bilgi için bkz: [App Service işletim sistemi güncelleştirmelerini arkasında Sihirli Demystifying](https://blogs.msdn.microsoft.com/appserviceteam/2018/01/18/demystifying-the-magic-behind-app-service-os-updates/).

## <a name="how-does-azure-deal-with-significant-vulnerabilities"></a>Azure önemli güvenlik açıklarıyla nasıl ilgilenir?

Ne zaman ciddi güvenlik açıkları gerektiren hemen, gibi düzeltme eki uygulama [sıfırıncı gün güvenlik açıkları](https://wikipedia.org/wiki/Zero-day_(computing)), yüksek öncelikli güncelleştirmeleri bir olay temelinde işlenir.

Kritik güvenlik duyuruları Azure ile ziyaret ederek güncel [Azure güvenlik blogu](https://azure.microsoft.com/blog/topics/security/). 

## <a name="when-are-supported-language-runtimes-updated-added-or-deprecated"></a>Ne zaman desteklenen dil çalışma zamanları güncelleştirildi, eklenen kullanım dışı veya?

Desteklenen dil çalışma zamanları yeni kararlı sürümleri (birincil, ikincil veya düzeltme eki) uygulama hizmeti örneklerine düzenli olarak eklenir. Başkalarının sürümlerini yan yana yüklenirken bazı güncelleştirmeler mevcut yüklemesi üzerine yazın. Uygulamanızın üzerinde güncelleştirilmiş çalışma zamanı modülü otomatik olarak çalıştırdığı bir üzerine yazma yükleme anlamına gelir. Yan yana yükleme el ile yeni bir çalışma zamanı sürümü yararlanmak için uygulamanızı geçirmelisiniz anlamına gelir. Daha fazla bilgi için alt bölümleri birine bakın.

Çalışma zamanı güncelleştirmeleri ve deprecations burada duyurulur:

- https://azure.microsoft.com/updates/?product=app-service 
- https://github.com/Azure/app-service-announcements/issues

> [!NOTE] 
> Buradaki bilgiler, bir uygulama hizmeti uygulamaya yerleşik dil çalışma zamanları uygular. App Service'e karşıya özel bir çalışma zamanı, örneğin, el ile yükseltme sürece değişmez.
>
>

### <a name="new-patch-updates"></a>Yeni düzeltme eki güncelleştirmeleri

Düzeltme eki güncelleştirmeleri .NET, PHP, Java SDK'sı ya da Tomcat/Jetty sürüme otomatik olarak yeni sürümü ile mevcut yüklemesi üzerine yazarak uygulanır. Node.js düzeltme eki güncelleştirmeler (sonraki bölümde birincil ve ikincil sürümleri benzer) sürümlerini yan yana yüklenir. Yeni Python düzeltme eki sürümleri el ile yüklenebilir [site uzantıları](https://www.siteextensions.net/packages?q=Tags%3A%22python%22)), yerleşik Python yükleme yan yana.

### <a name="new-major-and-minor-versions"></a>Yeni birincil ve ikincil sürümleri

Yeni bir birincil veya ikincil sürüm eklendiğinde sürümlerini yan yana yüklenir. Bu gibi durumlarda, uygulamanızı el ile yeni sürüme yükseltebilirsiniz. Bir yapılandırma dosyasında çalışma zamanı sürümü yapılandırılıp yapılandırılmadığını (gibi `web.config` ve `package.json`), aynı yöntemiyle yükseltmeniz gerekir. Bir uygulama hizmeti kullandıysanız ayarını çalışma zamanı sürümü yapılandırmak, içinde değiştirebilirsiniz [Azure portal](https://portal.azure.com) veya çalıştırarak bir [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) komutunu [bulut Kabuk](../cloud-shell/overview.md), olarak Aşağıdaki örneklerde gösterildiği:

```azurecli-interactive
az webapp config set --net-framework-version v4.7 --resource-group <groupname> --name <appname>
az webapp config set --php-version 7.0 --resource-group <groupname> --name <appname>
az webapp config appsettings set --settings WEBSITE_NODE_DEFAULT_VERSION=8.9.3 --resource-group <groupname> --name <appname>
az webapp config set --python-version 3.4 --resource-group <groupname> --name <appname>
az webapp config set --java-version 1.8 --java-container Tomcat --java-container-version 9.0 --resource-group <groupname> --name <appname>
```

### <a name="deprecated-versions"></a>Kullanım dışı sürümleri  

Eski bir sürümü kullanım dışıdır, böylece çalışma zamanı sürüm yükseltme uygun planı yapabilmesi kaldırma tarihi duyurdu. 

## <a name="how-can-i-query-os-and-runtime-update-status-on-my-instances"></a>Nasıl ı işletim sistemi ve çalışma zamanı güncelleştirme durumu my örneklerinde sorgulama yapabilirsiniz?  

Kritik işletim sistemi bilgileri aşağı erişimden kilitliyken (bkz [işletim sistemi işlevselliğini Azure App Service'te](web-sites-available-operating-system-functionality.md)), [Kudu konsol](https://github.com/projectkudu/kudu/wiki/Kudu-console) işletim sistemi ile ilgili uygulama hizmeti örneğinizi sorgu olanak tanır Sürüm ve çalışma zamanı sürümler. 

Aşağıdaki tabloda, uygulamalarınızı çalıştırmakta olduğunuz Windows ve sürümleri dil çalışma zamanı nasıl:

| Bilgi | Nerede bulacağını | 
|-|-|
| Windows sürümü | Bkz: `https://<appname>.scm.azurewebsites.net/Env.cshtml` (altında sistem bilgisi) |
| .NET sürüm | Konumundaki `https://<appname>.scm.azurewebsites.net/DebugConsole`, komut isteminde aşağıdaki komutu çalıştırın: <br>`powershell -command "gci 'Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Net Framework Setup\NDP\CDF'"` |
| .NET core sürümü | Konumundaki `https://<appname>.scm.azurewebsites.net/DebugConsole`, komut isteminde aşağıdaki komutu çalıştırın: <br> `dotnet --version` |
| PHP sürümü | Konumundaki `https://<appname>.scm.azurewebsites.net/DebugConsole`, komut isteminde aşağıdaki komutu çalıştırın: <br> `php --version` |
| Varsayılan Node.js sürümü | İçinde [bulut Kabuk](../cloud-shell/overview.md), aşağıdaki komutu çalıştırın: <br> `az webapp config appsettings list --resource-group <groupname> --name <appname> --query "[?name=='WEBSITE_NODE_DEFAULT_VERSION']"` |
| Python sürümü | Konumundaki `https://<appname>.scm.azurewebsites.net/DebugConsole`, komut isteminde aşağıdaki komutu çalıştırın: <br> `python --version` |  

> [!NOTE]  
> Kayıt defteri konuma erişim `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Component Based Servicing\Packages`, burada hakkında bilgi ["KB" düzeltme ekleri]((https://docs.microsoft.com/security-updates/SecurityBulletins/securitybulletins)) depolanır, kilitlenmiştir.
>
>

## <a name="more-resources"></a>Diğer kaynaklar

[Güven Merkezi: güvenlik](https://www.microsoft.com/en-us/trustcenter/security)  
[Azure App Service ASP.NET Core 64 bit](https://gist.github.com/glennc/e705cd85c9680d6a8f1bdb62099c7ac7)
