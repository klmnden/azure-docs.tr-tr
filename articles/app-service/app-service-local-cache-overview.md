---
title: Azure uygulama hizmeti yerel önbelleğinden genel bakış | Microsoft Docs
description: Bu makalede, etkinleştirme, yeniden boyutlandırma ve Azure uygulama hizmeti yerel önbelleğinden özelliği durumunu sorgulamak açıklar
services: app-service
documentationcenter: app-service
author: SyntaxC4
manager: yochayk
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
ms.author: cfowler
ms.openlocfilehash: 75f2dcb80514105ed663ba1fe5f7adccc05af1fc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23836388"
---
# <a name="azure-app-service-local-cache-overview"></a>Azure uygulama hizmeti yerel önbelleğinden genel bakış
Azure web uygulaması içerik Azure depolama alanında depolanır ve yukarı içerik paylaşımı sağlam bir şekilde ortaya. Bu tasarım çeşitli uygulamaları ile kullanılmak üzere tasarlanmış ve aşağıdaki özniteliklere sahiptir:  

* İçeriği birden çok sanal makine (VM) örneği web uygulaması arasında paylaşılır.
* İçerik dayanıklı ve web uygulamaları çalıştırılarak değiştirilebilir.
* Günlük dosyaları ve tanılama verilerinin dosyaları, aynı paylaşılan içerik klasörü altında kullanılabilir.
* Yeni içerik doğrudan yayımlama içerik klasörünü güncelleştirir. SCM Web ve çalışan web uygulaması aracılığıyla aynı içerik hemen görüntüleyebilirsiniz (genellikle ASP.NET gibi bazı teknolojiler son içerik almak için bazı dosya değişiklikleri bir web uygulaması yeniden başlatmayı başlatmak).

Birçok web uygulamaları bir ya da bu özelliklerin tümü, kullansa da, bazı web uygulamaları, yüksek kullanılabilirlik ile çalıştırabilirsiniz bir yüksek performanslı, salt okunur içerik deposu yeterlidir. Bu uygulamalar, belirli bir yerel önbelleği VM örneğinden yararlı olabilir.

Azure uygulama hizmeti yerel önbelleğinden özelliği içeriğinizi web rolü görünümünü sağlar. Bu içerik, zaman uyumsuz olarak yerinde başlangıç oluşturulan depolama içeriğinizi yazma ancak atma önbelleğidir. Önbellek hazır olduğunda, önbelleğe alınan içeriğe karşı çalıştırmak için site çevrilir. Yerel önbelleğine çalışan web uygulamalarını aşağıdaki avantajlara sahiptir:

* Bunlar Azure Storage içeriğine eriştiğinde oluşan gecikme bilmez.
* Bunlar, planlanmış yükseltme veya Planlanmamış kapalı kalma süreleri ve Azure Storage ile içerik Paylaşımı hizmet sunucularda ortaya diğer kesintilere bilmez.
* Depolama paylaşımı değişiklikler nedeniyle daha az uygulama yeniden başlatma sahiptirler.

## <a name="how-local-cache-changes-the-behavior-of-app-service"></a>Nasıl yerel önbelleği uygulama hizmeti davranışını değiştirir
* Yerel önbellek, web uygulamasının /site ve /siteextensions klasörleri kopyasıdır. Web uygulaması başlangıç yerel VM örneğinde oluşturulur. Web uygulaması başına yerel önbellek boyutu varsayılan olarak 300 MB ile sınırlandırılmıştır ancak 2 GB artırabilir.
* Yerel önbellek okuma-yazma. Ancak, tüm değişiklikler atılır web uygulaması sanal makineleri taşıdığında veya yeniden. Yerel önbelleği kritik veri içerik deposunu uygulamalar için kullanmayın.
* Web uygulamaları, şu anda yaptıkları gibi günlük dosyaları ve tanılama veri yazmaya devam eder. Günlük dosyaları ve verileri, ancak yerel olarak VM üzerinde depolanır. Ardından üzerinden düzenli aralıklarla paylaşılan içerik deposu kopyalanırlar. Paylaşılan içerik deposu için iyi çaba--geri son ani bir VM örneği çökmesine kaybolabilir yazma kopyasıdır.
* Yerel önbelleği kullanan web uygulamaları için LogFiles ve veri klasörünün klasörü yapısı içinde bir değişiklik yoktur. "Benzersiz tanımlayıcı" + zaman damgası adlandırma desenini izler depolama LogFiles ve veri klasörleri içinde artık alt klasörler var. Her alt burada web uygulaması çalıştıran veya çalışan bir VM örneğine karşılık gelir.  
* Web uygulaması yayımlama mekanizmaları kanalıyla yayımlama değişiklikler paylaşılan içerik deposu yayımlar. Yayımlanan içerik dayanıklı olmasını istiyoruz tasarım gereği olmasıdır. Web uygulamasının yerel önbelleği yenilemek için yeniden başlatılması gerekir. Bu bir aşırı adım gibi görünüyor? Yaşam döngüsü sorunsuz yapmak için bu makalenin sonraki bölümlerinde bilgilerine bakın.
* D:\Home yerel önbelleğine işaret eder. D:\Local devam geçici VM özgü depolama birimini işaret eder.
* SCM sitenin varsayılan içerik görünümü, paylaşılan içerik deposu olmaya devam ediyor.

## <a name="enable-local-cache-in-app-service"></a>Uygulama hizmeti yerel önbellekteki etkinleştir
Yerel önbelleği ayrılmış uygulama ayarları bir bileşimini kullanarak yapılandırın. Aşağıdaki yöntemleri kullanarak bu uygulama ayarları yapılandırabilirsiniz:

* [Azure portal](#Configure-Local-Cache-Portal)
* [Azure Resource Manager](#Configure-Local-Cache-ARM)

### <a name="configure-local-cache-by-using-the-azure-portal"></a>Azure portalını kullanarak yerel önbelleği yapılandırma
<a name="Configure-Local-Cache-Portal"></a>

Bu uygulama ayarını kullanarak bir web-uygulama başına temelinde yerel önbelleği etkinleştir:`WEBSITE_LOCAL_CACHE_OPTION` = `Always`  

![Azure portal uygulaması ayarları: yerel önbelleği](media/app-service-local-cache-overview/app-service-local-cache-configure-portal.png)

### <a name="configure-local-cache-by-using-azure-resource-manager"></a>Azure Kaynak Yöneticisi'ni kullanarak yerel önbelleği yapılandırma
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

## <a name="change-the-size-setting-in-local-cache"></a>Yerel önbellek boyutu ayarına değiştirme
Varsayılan olarak, yerel önbellek boyutu olan **300 MB**. Bu, yerel olarak oluşturulan günlükleri ve veri klasörleri yanı sıra /site ve içerik deposundan kopyalanır /siteextensions klasörleri içerir. Bu sınırı artırmak için uygulama ayarı kullanın `WEBSITE_LOCAL_CACHE_SIZEINMB`. En fazla boyutunu artırabilir **2 GB** (2000 MB) web uygulaması başına.

## <a name="best-practices-for-using-app-service-local-cache"></a>Uygulama hizmeti yerel önbelleğinden kullanmak için en iyi uygulamalar
İle birlikte yerel önbelleği kullanmanızı öneririz [hazırlama ortamlarını](../app-service/web-sites-staged-publishing.md) özelliği.

* Ekleme *Yapışkan* uygulama ayarı `WEBSITE_LOCAL_CACHE_OPTION` değerle `Always` için **üretim** yuvası. Kullanıyorsanız, `WEBSITE_LOCAL_CACHE_SIZEINMB`, ayrıca, üretim yuvası için Yapışkan ayarı olarak ekleyin.
* Oluşturma bir **hazırlama** yuva ve hazırlama yuvası yayımlayın. Genellikle, üretim yuvası için yerel önbelleği yararları alırsanız hazırlama için sorunsuz bir derleme, dağıtma, test ömrü etkinleştirmek için yerel önbelleği kullanmak üzere hazırlama yuvası ayarlamanız gerekmez.
* Sitenizi hazırlama yuvası karşı test edin.  
* Hazır olduğunuzda, sorun bir [takas işlemi](../app-service/web-sites-staged-publishing.md#Swap) arasında hazırlama ve üretim yuvalarını.  
* Ad ve bir yuvaya Yapışkan Yapışkan ayarları içerir. Bu nedenle üretime hazırlama yuvası takas, yerel önbellek uygulama ayarlarını devralır. Yeni takas üretim yuvasına yerel önbelleğe karşı birkaç dakika sonra çalışacak ve yuva Isınma bir parçası olarak değiştirme işleminden sonra warmed. Bu nedenle yuva değiştirmenin tamamlandığında, üretim yuvası yerel önbelleğe karşı çalışıyor.

## <a name="frequently-asked-questions-faq"></a>Sık sorulan sorular (SSS)
### <a name="how-can-i-tell-if-local-cache-applies-to-my-web-app"></a>Yerel önbelleği my web uygulaması için geçerli olup olmadığını nasıl anlayabilirim?
Web uygulamanızı yüksek performanslı, güvenilir bir içerik deposu gereken, çalışma zamanında kritik veri yazmak için içerik deposu kullanmaz ve toplam boyutu 2 GB'tan daha az ise, yanıt "Evet" değil! /Site ve /siteextensions klasörlerinizi toplam boyutu almak için site uzantısı "Azure Web Apps Disk kullanımı." kullanabilirsiniz

### <a name="how-can-i-tell-if-my-site-has-switched-to-using-local-cache"></a>Sitem yerel önbelleği kullanmaya geçirdi nasıl anlayabilirim?
Hazırlama ortamlarını ile yerel önbelleği özelliğini kullanıyorsanız, yerel önbellek warmed kadar değiştirme işlemi tamamlayın değil. Siteniz yerel önbelleğe karşı çalışıp çalışmadığını denetlemek için çalışan işlem ortam değişkenini denetleyebilir `WEBSITE_LOCALCACHE_READY`. Yönergeleri kullanın [çalışan işlem ortam değişkeni](https://github.com/projectkudu/kudu/wiki/Process-Threads-list-and-minidump-gcdump-diagsession#process-environment-variable) birden çok örneği üzerinde çalışan işlem ortam değişkeni erişmek için sayfa.  

### <a name="i-just-published-new-changes-but-my-web-app-does-not-seem-to-have-them-why"></a>Henüz yeni değişiklikleri yayımladığınız, ancak web Uygulamam bunları görünmüyor. Neden?
Web uygulamanızı yerel önbelleği kullanıyorsa, sitenizi en son değişiklikleri almak için yeniden başlatmanız gerekir. Değişiklikleri üretim yayınlamanıza istemiyor musunuz? Önceki bölümdeki en iyi yöntemler yuvası seçeneklerine bakın.

### <a name="where-are-my-logs"></a>My günlükleri nerede?
Yerel önbelleği ile günlükleri ve veri klasörleri biraz farklı görünüyor. Ancak, biçimi "VM tanımlayıcısı" + zaman damgası bir alt klasörü altında alt klasörleri nestled ancak bu, alt yapısını aynı kalır.

### <a name="i-have-local-cache-enabled-but-my-web-app-still-gets-restarted-why-is-that-i-thought-local-cache-helped-with-frequent-app-restarts"></a>Yerel önbellek etkin olan, ancak web Uygulamam yeniden. Bunun nedeni nedir? I sık sık uygulama yeniden başlatma ile yerel önbelleği Yardım zorlayıcı.
Yerel önbelleği depolama ile ilgili web uygulaması yeniden önlemeye yardımcı olur. Ancak, web uygulamanızı yeniden başlatmalar VM planlanmış altyapısını yükseltmeler sırasında uygulanabilecek hala. Yerel önbelleği etkin deneyimi genel uygulama yeniden başlatılmadan daha az olmalıdır.

### <a name="does-local-cache-exclude-any-directories-from-being-copied-to-the-faster-local-drive"></a>Yerel önbelleği daha hızlı yerel sürücüye kopyalanan herhangi bir dizin hariç mu?
Depolama içeriği kopyalar adımının bir parçası, depo adlı herhangi bir klasör dışlandı. Bu web uygulamasının günlük işleminde gerekli olmayan bir kaynak denetimi deponuza sitenizin içeriğinin nerede içerebilir senaryolarında yardımcı olur. 
