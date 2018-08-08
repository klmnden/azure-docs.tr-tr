---
title: Azure App Service yerel önbelleğe genel bakış | Microsoft Docs
description: Bu makalede, etkinleştirme, yeniden boyutlandırma ve Azure uygulama hizmeti yerel önbelleğinden özellik durumunu sorgulamak açıklanır
services: app-service
documentationcenter: app-service
author: cephalin
manager: jpconnock
editor: ''
tags: optional
keywords: ''
ms.assetid: e34d405e-c5d4-46ad-9b26-2a1eda86ce80
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/04/2016
ms.author: cephalin
ms.openlocfilehash: 59fe70e4d2a710160751ab8e7a83c9f86310dc24
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39597739"
---
# <a name="azure-app-service-local-cache-overview"></a>Azure App Service yerel önbelleğe genel bakış

> [!NOTE]
> Yerel önbellek desteklenmez kapsayıcı App Service uygulamaları gibi [Linux üzerinde App Service'te](containers/app-service-linux-intro.md).

Azure web uygulaması içeriğini Azure depolama alanında depolanır ve yukarı dayanıklı bir şekilde içerik paylaşımı olarak kullanıma sunulur. Bu tasarım, çeşitli uygulamaları ile kullanılmak üzere tasarlanmış ve öznitelikleri şunlardır:  

* İçerik, web uygulamasının birden çok sanal makine (VM) örneklerinde paylaşılır.
* İçerik dayanıklı ve web uygulamaları çalıştırılarak değiştirilebilir.
* Günlük dosyaları ve tanılama verilerinin dosyaları, aynı paylaşılan içerik klasörü altında kullanılabilir.
* Yeni içerik doğrudan yayımlama içerik klasörünü güncelleştirir. Aynı içeriği SCM Web ve çalışan web uygulaması aracılığıyla hemen görüntüleyebilirsiniz (genellikle bazı teknolojiler ASP.NET gibi en son içeriğini almak için bazı dosya değişiklikleri bir web uygulamasını yeniden başlatın).

Bir ya da bu özelliklerin tümü, birçok web apps kullanıyoruz, ancak bazı web uygulamaları, yüksek kullanılabilirlik ile çalıştırılabilir bir yüksek performanslı, salt okunur içerik deposu yeterlidir. Bu uygulamalar, belirli bir yerel önbellek VM örneğinden yararlanabilirsiniz.

Azure uygulama hizmeti yerel önbelleğinden özelliği bir web rolü içeriğinizi görünümünü sağlar. Bu içerik zaman uyumsuz olarak yerinde başlangıç oluşturulan depolama içeriğinize yazma ancak atma önbellektir. Önbellek hazır olduğunda, önbelleğe alınan içeriğe karşı çalıştırmak için site anahtarlanır. Yerel önbellek üzerinde çalışan web uygulamaları aşağıdaki avantajlara sahiptir:

* Azure Depolamasındaki içerik her eriştiklerinde oluşan gecikme süreleri için ayrıcalıklı değildirler.
* Planlanmış yükseltme veya plansız kapalı kalma süreleri ve Azure depolama ile içerik Paylaşımı hizmet sunucularda ortaya diğer kesintileri bağışıklığı değildirler.
* Depolama paylaşımı değişiklikleri nedeniyle daha az uygulama yeniden başlatma sahiptirler.

## <a name="how-local-cache-changes-the-behavior-of-app-service"></a>Yerel önbellek App Service'in davranışını nasıl değiştirir
* Yerel önbellek, web uygulamasının /site ve /siteextensions klasörleri bir kopyasıdır. Web uygulaması başlangıç yerel sanal makine örneğinde oluşturulur. Web uygulaması başına yerel önbellek boyutu varsayılan olarak 300 MB ile sınırlıdır, ancak en fazla 2 GB artırın.
* Yerel önbellek, okunur-yazılır olmuştur. Web uygulaması sanal makineleri taşır veya yeniden ancak değişiklikler atılır. Yerel önbellek, görev açısından kritik veri içerik deposuna uygulamalar için kullanmayın.
* Web uygulamaları, şu anda yaptığınız gibi günlük dosyaları ve tanılama verilerini yazmak devam edebilirsiniz. Günlük dosyaları ve verileri, ancak yerel olarak VM üzerinde depolanır. Ardından bunlar üzerinden düzenli aralıklarla paylaşılan içerik deposuna kopyalanır. Paylaşılan içerik deposu için best-case bir çaba--telefonla geri nedeniyle sanal makine örneği ani bir kilitlenme için kaybolabileceğini yazma kopyasıdır.
* Yerel önbellek kullanan web uygulamaları için LogFiles ve veri klasörlerin klasörü yapısı içinde bir değişiklik yoktur. "Benzersiz tanımlayıcı" + zaman damgası adlandırma desenini izler depolama LogFiles ve veri klasörler, artık alt klasörler var. Her alt burada web uygulamasını çalıştıran veya çalışan bir VM örneğine karşılık gelir.  
* Herhangi bir yayımlama mekanizmalar aracılığıyla web uygulaması için yayımlama değişiklikleri kalıcı paylaşılan içerik deposu yayımlar. Web uygulamasının yerel önbelleği yenilemek için yeniden başlatılması gerekir. Yaşam döngüsü sorunsuz hale getirmek için bu makaledeki bilgileri bakın.
* D:\Home yerel önbelleğe işaret eder. D:\Local geçici sanal Makineye özgü depolama alanına işaret edecek şekilde devam eder.
* SCM sitesine varsayılan içerik görünümü, paylaşılan içerik deposu olmaya devam eder.

## <a name="enable-local-cache-in-app-service"></a>App Service yerel önbellek etkinleştir
Yerel önbellek, ayrılmış uygulama ayarları bir bileşimini kullanarak yapılandırın. Aşağıdaki yöntemleri kullanarak bu uygulama ayarları yapılandırabilirsiniz:

* [Azure portal](#Configure-Local-Cache-Portal)
* [Azure Resource Manager](#Configure-Local-Cache-ARM)

### <a name="configure-local-cache-by-using-the-azure-portal"></a>Yerel önbellek Azure portalını kullanarak yapılandırma
<a name="Configure-Local-Cache-Portal"></a>

Bu uygulama ayarını kullanarak tek başına-web-app temelinde yerel önbelleği etkinleştir: `WEBSITE_LOCAL_CACHE_OPTION` = `Always`  

![Azure Portalı Uygulama ayarları: yerel önbellek](media/app-service-local-cache-overview/app-service-local-cache-configure-portal.png)

### <a name="configure-local-cache-by-using-azure-resource-manager"></a>Azure Resource Manager kullanarak yerel önbelleğini yapılandırma
<a name="Configure-Local-Cache-ARM"></a>

```

...

{
    "apiVersion": "2015-08-01",
    "type": "config",
    "name": "appsettings",
    "dependsOn": [
        "[resourceId('Microsoft.Web/sites/', variables('siteName'))]"
    ],

"properties": {
        "WEBSITE_LOCAL_CACHE_OPTION": "Always",
        "WEBSITE_LOCAL_CACHE_SIZEINMB": "300"
    }
}

...
```

## <a name="change-the-size-setting-in-local-cache"></a>Yerel önbellek boyut ayarlarını değiştirmek
Varsayılan olarak, yerel önbellek boyutu olan **300 MB**. Bu, yerel olarak oluşturulmuş günlükleri ve veri klasörleri yanı sıra /site ve içerik deposundan kopyalanan /siteextensions klasörleri içerir. Bu sınırı artırmak için uygulama ayarını kullanan `WEBSITE_LOCAL_CACHE_SIZEINMB`. En fazla boyutunu artırabilir **2 GB** (2000 MB) web uygulaması başına.

## <a name="best-practices-for-using-app-service-local-cache"></a>App Service yerel önbellek kullanmak için en iyi uygulamalar
Yerel önbellek ile birlikte kullanmanızı öneririz [hazırlama ortamları](../app-service/web-sites-staged-publishing.md) özelliği.

* Ekleme *Yapışkan* uygulama ayarı `WEBSITE_LOCAL_CACHE_OPTION` değerle `Always` için **üretim** yuvası. Kullanıyorsanız `WEBSITE_LOCAL_CACHE_SIZEINMB`, ayrıca, üretim yuvasına Yapışkan ayarı olarak ekleyin.
* Oluşturma bir **hazırlama** yuva ve hazırlama yuvanızı yayımlayın. Genellikle, üretim yuvası için yerel önbellek avantajlarından yararlanın, hazırlama için sorunsuz bir yapı-dağıtma-test yaşam etkinleştirmek üzere yerel önbelleği kullanılacak hazırlama yuvasına ayarlamanız gerekmez.
* Hazırlama yuvanızı karşı sitenizi test edin.  
* Hazır olduğunuzda, sorun bir [takas işlemi](../app-service/web-sites-staged-publishing.md#Swap) , hazırlık ve üretim arasında yuvası.  
* Ad ve bir yuvaya Yapışkan Yapışkan ayarları içerir. Bu nedenle üretime hazırlama yuvası takas, yerel önbellek uygulama ayarlarını devralır. Yeni takas üretim yuvasına yerel önbelleğe karşı birkaç dakika sonra çalışacak ve yuva Isınma bir parçası olarak değiştirme işleminden sonra warmed. Bu nedenle, üretim yuvası yuvası takas tamamlandığında yerel önbelleğe karşı çalışıyor.

## <a name="frequently-asked-questions-faq"></a>Sık sorulan sorular (SSS)
### <a name="how-can-i-tell-if-local-cache-applies-to-my-web-app"></a>Yerel önbellek, web Uygulamam için geçerli olup olmadığını nasıl anlayabilirim?
Web uygulamanızı yüksek performanslı, güvenilir içerik deposuna ihtiyacı, kritik verileri çalışma zamanında yazılacak içerik deposuna kullanmaz ve toplam boyutu 2 GB'den küçük olduğundan, yanıt "Evet" demektir! /Site ve /siteextensions klasörlerinizi toplam boyutunu almak için site uzantısı "Azure Web Apps Disk kullanımı." kullanabilirsiniz.

### <a name="how-can-i-tell-if-my-site-has-switched-to-using-local-cache"></a>Sitem yerel önbelleği kullanmaya geçti nasıl anlayabilirim?
Hazırlama ortamları ile yerel önbellek özelliğini kullanıyorsanız, yerel önbellek warmed kadar değiştirme işlemi tamamlamak değil. Siteniz yerel önbelleğe karşı çalışıp çalışmadığını denetlemek için çalışan işlem ortam değişkenini denetleyebilir `WEBSITE_LOCALCACHE_READY`. Şirket yönergeleri kullanın [çalışan işlem ortam değişkeni](https://github.com/projectkudu/kudu/wiki/Process-Threads-list-and-minidump-gcdump-diagsession#process-environment-variable) birden fazla çalışan işlem ortam değişkeninde erişmek için sayfa.  

### <a name="i-just-published-new-changes-but-my-web-app-does-not-seem-to-have-them-why"></a>Yeni değişiklikler yayımlamış ancak web uygulamamı bunları görünmüyor. Neden?
Web uygulamanızı yerel önbelleği kullanıyorsa, siteniz en son değişiklikleri almak için yeniden başlatmanız gerekir. Bir üretim sitesini değişiklikleri yayımlamak istemiyor musunuz? En iyi yöntemler önceki bölümde yuvası seçeneklere bakın.

### <a name="where-are-my-logs"></a>My günlükleri nerededir?
Yerel önbellek ile günlükleri ve veri klasörleri biraz farklı görünür. Ancak, biçimi "VM tanımlayıcısı" + zaman damgası ile bir alt klasörü altında alt klasörleri nestled dışında kendi alt yapısını aynı kalır.

### <a name="i-have-local-cache-enabled-but-my-web-app-still-gets-restarted-why-is-that-i-thought-local-cache-helped-with-frequent-app-restarts"></a>Yerel önbellek etkin sahibim, ancak web uygulamamı yeniden. Bunun nedeni nedir? Sık sık uygulama yeniden başlatma ile yerel önbelleği Yardım düşündüm.
Yerel önbellek depolama bağlantılı web uygulaması yeniden önlemeye yardımcı olur. Ancak, web uygulamanızı yeniden sanal makinenin planlanmış altyapısını yükseltmeler sırasında geçeriz yine de. Yerel önbellek etkin deneyimi genel uygulama yeniden başlatma, daha az olmalıdır.

### <a name="does-local-cache-exclude-any-directories-from-being-copied-to-the-faster-local-drive"></a>Yerel önbellek, daha hızlı yerel sürücüye kopyalanan herhangi bir dizin dışarıda tuttuğunu?
Depolama içeriği kopyalar adımının bir parçası, depo adlı herhangi bir klasörü dahil değildir. Bu web uygulamasının günlük işlemlerinde gerekli olmayan bir kaynak denetim deposu site içeriğiniz burada içerebilir senaryoları ile yardımcı olur. 
