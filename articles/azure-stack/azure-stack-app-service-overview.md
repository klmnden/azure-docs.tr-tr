---
title: App Service, Azure Stack genel bakış | Microsoft Docs
description: Azure Stack üzerinde App Service'e genel bakış
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2018
ms.author: sethm
ms.reviewer: anwestg
ms.openlocfilehash: 3015755d8046f72c03cd1a1485d90a7f720a8909
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49353039"
---
# <a name="app-service-on-azure-stack-overview"></a>Azure Stack’te App Service’e genel bakış

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack'te Azure App Service, Azure Stack için kullanılabilir olan Microsoft Azure platformu-bir hizmet olarak (PaaS) teklifidir. Web API ve Azure işlevleri'ni oluşturmak iç veya dış müşterilere hizmet sağlar herhangi bir platform veya cihaz için uygulamalar. Uygulamalarınızı şirket içi uygulamalarla tümleştirin ve iş süreçlerini otomatik hale getirin. Azure Stack bulut operatörlerinin müşteri uygulamaları, kendi seçtiğiniz paylaşılan VM kaynakları veya özel VM'ler tam olarak yönetilen sanal makineler (VM) çalıştırabilirsiniz.

Azure App Service, iş süreçlerini otomatikleştirmek ve bulut API'leri barındırmak sağlar. Tek bir tümleşik hizmet olarak Azure App Service, Web siteleri, REST API'leri ve tek bir çözüm, iş süreçleri gibi çeşitli bileşenleri birleştirmek sağlar.

## <a name="why-offer-azure-app-service-on-azure-stack"></a>Neden Azure Stack üzerinde Azure App Service'te sunuyor?

App Service’in temel özelliklerinden bazıları şunlardır:

- **Birden çok dil ve çerçeveleri**: App Service ASP.NET, Node.js, Java, PHP ve Python için birinci sınıf destek sunar. Windows PowerShell ve diğer betikleri veya yürütülebilirleri App Service sanal makineleri üzerinde çalıştırabilirsiniz.
- **DevOps iyileştirmesi**: sürekli tümleştirme ve dağıtım GitHub, yerel Git veya BitBucket ile ayarlayın. Test ve hazırlık ortamları aracılığıyla güncelleştirmeleri yükseltin ve Azure PowerShell veya platformlar arası komut satırı arabirimi (CLI) kullanarak App Service uygulamalarınızı yönetin.
- **Visual Studio tümleştirmesi**: Visual Studio'daki ayrılmış Araçlar oluşturma ve uygulamaları dağıtma işlemlerini kolaylaştırır.

## <a name="app-types-in-app-service"></a>App Service’deki uygulama türleri

App Service, her biri belirli bir iş yükü barındırması amaçlanan birkaç uygulama türlerini sunar:

- [Web uygulamaları](../app-service/app-service-web-overview.md) Web siteleri ve web uygulamalarını barındırmak için.
- [API Apps](../app-service/app-service-web-overview.md) REST API'leri barındırmak için.
- Olay temelli, sunucusuz iş yüklerini barındırmak için azure işlevleri.

Word *uygulama* bir iş yükünü çalıştırmaya ayrılmış barındırma kaynaklarını ifade eder. Alma *web uygulaması* örnek olarak, büyük olasılıkla işlem kaynakları ve uygulamayı bir tarayıcıya birlikte teslim işlevselliği kod gibi bir web uygulaması düşünmek için alışkın. App Service'te bir web uygulaması, uygulama kodunuzun barındırılması için Azure Stack sağlayan işlem kaynağıdır.

Uygulamanız farklı türlerde birden fazla App Service uygulamalarında oluşabilir. Örneğin, uygulamanız bir web ön ucu oluşur ve son geri bir REST API, şunları yapabilirsiniz:

- İkisini de (ön uç ve api) tek bir web uygulamasına dağıtma
- Ön uç kodunuzu bir web uygulamasına, arka uç kodunuzu bir API uygulamasına dağıtma.

   [ ![Veri izleme ile app Service'e genel bakış](media/azure-stack-app-service-overview/image01.png "izleme verileri ile App Service'e genel bakış") ](media/azure-stack-app-service-overview/image01.png#lightbox)

## <a name="what-is-an-app-service-plan"></a>App Service planı nedir?

App Service kaynak sağlayıcısı, Azure App Service kullanan aynı kodu kullanır ve bu nedenle bazı genel kavramlar paylaşır. App Service fiyatlandırma kapsayıcı uygulamaları için çağrılır *App Service planı*. Bu, uygulamalarınızın tutmak için kullanılan adanmış sanal makineler kümesini temsil eder. Belirli bir aboneliği içinde birden fazla App Service planları olabilir.

Azure'da, paylaşılan ve adanmış çalışanları vardır. Paylaşılan çalışan yüksek yoğunluklu çok kiracılı uygulama barındırma destekler ve yalnızca bir dizi paylaşılan çalışanları yoktur. Adanmış sunucusu yalnızca bir kiracı tarafından kullanılan ve üç boyutlarında gelir: küçük, Orta ve büyük. Şirket içi müşteri gereksinimlerini her zaman bu terimleri kullanarak açıklanan olamaz. Azure Stack üzerinde App Service'te kullanılabilir hale getirmek istedikleri çalışan katmanları kaynak sağlayıcısı Yöneticiler tanımlayabilirsiniz. Benzersiz barındırma gereksinimlerinize göre birden fazla paylaşılan çalışan veya adanmış çalışan farklı ayarlar tanımlayabilirsiniz. Bu çalışan katmanı tanımlarını kullanarak, bunlar daha sonra kendi fiyatlandırma SKU'ları tanımlayabilirsiniz.

## <a name="portal-features"></a>Portal özellikleri

Azure Stack'te App Service, Azure App Service kullanan aynı kullanıcı arabirimini kullanır. Aynı arka uç ile geçerlidir. Ancak, bazı özellikleri Azure Stack'te devre dışı bırakılır. Azure'a özel beklentileri ya da bu özellikleri gerektiren hizmetler Azure Stack'te şu anda kullanılabilir değil.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack üzerinde App Service ile çalışmaya başlamadan önce](azure-stack-app-service-before-you-get-started.md)
- [App Service kaynak Sağlayıcısı'nı yükleme](azure-stack-app-service-deploy.md)

Diğer de deneyebilirsiniz [platform olarak hizmet (PaaS) Hizmetleri](azure-stack-tools-paas-services.md), gibi [SQL Server Kaynak sağlayıcısı](azure-stack-sql-resource-provider-deploy.md) ve [MySQL kaynak sağlayıcısı](azure-stack-mysql-resource-provider-deploy.md).
