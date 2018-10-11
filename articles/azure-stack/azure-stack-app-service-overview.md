---
title: "App Service'e genel bakış: Azure Stack | Microsoft Docs"
description: Azure Stack üzerinde App Service'e genel bakış
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2018
ms.author: sethm
ms.reviewer: anwestg
ms.openlocfilehash: cf2d65e7e2927aee99e533ea0bca0818f3ab98f6
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "49079166"
---
# <a name="app-service-on-azure-stack-overview"></a>Azure Stack’te App Service’e genel bakış

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack'te Azure App Service, Azure Stack için kullanılabilir olan Microsoft Azure platformu-bir hizmet olarak (PaaS) teklifidir. Müşterilerinizin - iç veya dış hizmet sağlar - web API ve Azure işlevleri oluşturmak istediğiniz platform veya cihaz için uygulamalar. Uygulamalarınızı şirket içi uygulamalarla tümleştirin ve iş süreçlerini otomatik hale getirin. Azure Stack bulut operatörlerinin müşteri uygulamaları, kendi seçtiğiniz paylaşılan VM kaynakları veya özel VM'ler tam olarak yönetilen sanal makineler (VM) çalıştırabilirsiniz.

Azure App Service, iş süreçleri ve API'leri barındırma Bulutu otomatik hale getirmenizi sağlar. Tek bir tümleşik hizmet olarak Azure App Service – Web siteleri, RESTful API'leri ve iş süreçlerini--çeşitli bileşenlerini birleştirerek, tek bir çözümde sağlar.

## <a name="why-offer-azure-app-service-on-azure-stack"></a>Neden Azure Stack üzerinde Azure App Service'te sunuyor?

App Service’in temel özelliklerinden bazıları şunlardır:

- **Birden çok dil ve çerçeveleri**: App Service ASP.NET, Node.js, Java, PHP ve Python için birinci sınıf destek sunar. Windows PowerShell ve diğer betikleri veya yürütülebilirleri App Service sanal makineleri üzerinde çalıştırabilirsiniz.
- **DevOps iyileştirmesi**: sürekli tümleştirme ve dağıtım GitHub, yerel Git veya BitBucket ile ayarlayın. Test ve hazırlık ortamları aracılığıyla güncelleştirmeleri yükseltin. Azure PowerShell veya platformlar arası komut satırı arabirimi (CLI) kullanarak App Service uygulamalarınızı yönetin.
- **Visual Studio tümleştirmesi**: Visual Studio'daki ayrılmış Araçlar oluşturma ve uygulamaları dağıtma işlemlerini kolaylaştırır.

## <a name="app-types-in-app-service"></a>App Service’deki uygulama türleri

App Service, her biri belirli bir iş yükü barındırması amaçlanan birkaç uygulama türlerini sunar:

- [Web uygulamaları](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) Web siteleri ve web uygulamalarını barındırmak için.
- [API Apps](https://docs.microsoft.com/azure/app-service-api/app-service-api-apps-why-best-platform) RESTful API'lerini barındırmaya yöneliktir.
- Olay temelli, sunucusuz iş yüklerini barındırmak için azure işlevleri.

Word uygulaması, bir iş yükünü çalıştırmaya ayrılmış barındırma kaynaklarını ifade eder. “Web uygulaması” örnek olarak alındığında, web uygulamasını tarayıcıya işlev kazandıran işlem kaynaklar ve uygulama kodu bütünü olarak düşünmeye büyük olasılıkla alışkınsınız. Ancak, App Service'te bir uygulama kodunuzun barındırılması için Azure Stack sağlayan bir işlem kaynak web uygulamasıdır.

Uygulamanız farklı türlerde birden fazla App Service uygulamalarında oluşabilir. Örneğin, uygulamanız bir web ön ucu oluşur ve bir RESTful API'si geri bitiş, şunları yapabilirsiniz:

- İkisini de (ön uç ve api) tek bir web uygulamasına dağıtma
- Ön uç kodunuzu bir web uygulamasına, arka uç kodunuzu bir API uygulamasına dağıtma.

   ![Veri izleme ile app Service'e genel bakış](media/azure-stack-app-service-overview/image01.png)

## <a name="what-is-an-app-service-plan"></a>App Service planı nedir?

App Service kaynak sağlayıcısı, Azure App Service kullanan aynı kodu kullanır. Sonuç olarak, bazı ortak tanımlayan değer kavramlardır. App Service fiyatlandırma kapsayıcı uygulamaları için App Service planının adı verilir. Bu, uygulamalarınızın tutmak için kullanılan adanmış sanal makineler kümesini temsil eder. Belirli bir aboneliği içinde birden fazla App Service planları olabilir.

Azure'da, paylaşılan ve adanmış çalışanları vardır. Paylaşılan çalışan yüksek yoğunluklu çok kiracılı uygulama barındırma destekler ve yalnızca bir dizi paylaşılan çalışanları yoktur. Adanmış sunucusu yalnızca bir kiracı tarafından kullanılan ve üç boyutlarında gelir: küçük, Orta ve büyük. Şirket içi müşteri gereksinimlerini her zaman bu terimleri kullanarak açıklanan olamaz. Azure Stack üzerinde App Service'te kullanılabilir hale getirmek istedikleri çalışan katmanları kaynak sağlayıcısı Yöneticiler tanımlayabilirsiniz. Benzersiz barındırma gereksinimlerinize göre birden fazla paylaşılan çalışan veya adanmış çalışan farklı ayarlar tanımlayabilirsiniz. Bu çalışan katmanı tanımlarını kullanarak, bunlar daha sonra kendi fiyatlandırma SKU'ları tanımlayabilirsiniz.

## <a name="portal-features"></a>Portal özellikleri

Azure Stack üzerinde App Service, Azure App Service kullanan aynı Arabirimi kullanan, aynı arka uç ile geçerlidir. Ancak, bazı özellikler devre dışı bırakılır ve Azure Stack'te işlevsel değildir. Azure'a özel beklentileri ya da bu özellikleri gerektiren hizmetler Azure Stack'te şu anda mevcut değildir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack üzerinde App Service ile çalışmaya başlamadan önce](azure-stack-app-service-before-you-get-started.md)
- [App Service kaynak Sağlayıcısı'nı yükleme](azure-stack-app-service-deploy.md)

Diğer de deneyebilirsiniz [platform olarak hizmet (PaaS) Hizmetleri](azure-stack-tools-paas-services.md), gibi [SQL Server Kaynak sağlayıcısı](azure-stack-sql-resource-provider-deploy.md) ve [MySQL kaynak sağlayıcısı](azure-stack-mysql-resource-provider-deploy.md).
