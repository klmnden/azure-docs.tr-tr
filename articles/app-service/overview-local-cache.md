---
title: Yerel önbelleğe genel bakış - Azure App Service | Microsoft Docs
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
ms.custom: seodec18
ms.openlocfilehash: 1d6e233509b50f0b03678f2e62267169d02133a1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60839047"
---
# <a name="azure-app-service-local-cache-overview"></a>Azure App Service yerel önbelleğe genel bakış

> [!NOTE]
> Yerel önbellek desteklenmez işlev uygulamaları veya kapsayıcı App Service uygulamaları gibi [Linux üzerinde App Service'te](containers/app-service-linux-intro.md).


Azure App Service içeriği, Azure depolama alanında depolanır ve yukarı dayanıklı bir şekilde içerik paylaşımı olarak kullanıma sunulur. Bu tasarım, çeşitli uygulamaları ile kullanılmak üzere tasarlanmış ve öznitelikleri şunlardır:  

* İçeriği, uygulama birden çok sanal makine (VM) örneklerinde paylaşılır.
* İçerik dayanıklı ve çalışan uygulamalar tarafından değiştirilebilir.
* Günlük dosyaları ve tanılama verilerinin dosyaları, aynı paylaşılan içerik klasörü altında kullanılabilir.
* Yeni içerik doğrudan yayımlama içerik klasörünü güncelleştirir. SCM Web ve çalışan aynı içeriğinden hemen görüntüleyebilirsiniz (genellikle ASP.NET gibi bazı teknolojiler başlatan son içeriğini almak için bazı dosya değişiklikleri uygulamanın yeniden) uygulama.

Birçok uygulama, bir ya da bu özelliklerin tümü, kullanırken, bazı uygulamalar, yüksek kullanılabilirlik ile çalıştırılabilir bir yüksek performanslı, salt okunur içerik deposu yeterlidir. Bu uygulamalar, belirli bir yerel önbellek VM örneğinden yararlanabilirsiniz.

Azure uygulama hizmeti yerel önbelleğinden özelliği bir web rolü içeriğinizi görünümünü sağlar. Bu içerik zaman uyumsuz olarak yerinde başlangıç oluşturulan depolama içeriğinize yazma ancak atma önbellektir. Önbellek hazır olduğunda, önbelleğe alınan içeriğe karşı çalıştırmak için site anahtarlanır. Yerel önbellek üzerinde çalışan uygulamalar, aşağıdaki avantajlara sahiptir:

* Azure Depolamasındaki içerik her eriştiklerinde oluşan gecikme süreleri için ayrıcalıklı değildirler.
* Planlanmış yükseltme veya plansız kapalı kalma süreleri ve Azure depolama ile içerik Paylaşımı hizmet sunucularda ortaya diğer kesintileri bağışıklığı değildirler.
* Depolama paylaşımı değişiklikleri nedeniyle daha az uygulama yeniden başlatma sahiptirler.

## <a name="how-the-local-cache-changes-the-behavior-of-app-service"></a>Yerel önbellek App Service'in davranışını nasıl değiştirir
* _D:\home_ uygulama başlatıldığında VM örneğine göre oluşturulan yerel önbellek işaret eder. _D:\Local_ geçici sanal Makineye özgü depolama alanına işaret edecek şekilde devam eder.
* Tek seferlik bir kopyasını yerel önbellek içeren _/site_ ve _/siteextensions_ paylaşılan içerik deposu, klasör, _D:\home\site_ ve _D:\home\ siteextensions_sırasıyla. Uygulama başlatıldığında dosyalar yerel önbelleğe kopyalanır. Her uygulama için iki klasör boyutu varsayılan olarak 300 MB ile sınırlıdır, ancak en fazla 2 GB artırın.
* Yerel önbellek, okunur-yazılır olmuştur. Uygulama, sanal makineleri taşır veya yeniden ancak değişiklikler atılır. Yerel önbellek, görev açısından kritik veri içerik deposuna uygulamalar için kullanmayın.
* _D:\home\LogFiles_ ve _D:\home\Data_ günlük dosyaları ve uygulama verilerini içerir. İki alt klasör VM örneğine göre yerel olarak depolanır ve paylaşılan içerik deposu için düzenli olarak kopyalanır. Uygulamaları günlük dosyaları ve verileri, bu klasörlere yazarak devam edebilir. Ancak, günlük dosyalarını ve sanal makine örneği ani bir kilitlenme nedeniyle kaybolacak veri mümkündür paylaşılan içerik deposu için kopyalama en yüksek çaba olduğundan.
* [Günlük akışını](troubleshoot-diagnostic-logs.md#streamlogs) en yüksek çaba kopyaya göre etkilenir. Akış günlüklerindeki bir dakikalık gecikme kadar mümkün.
* Paylaşılan içerik deposu klasörü yapısı içinde bir değişiklik olduğunda _LogFiles_ ve _veri_ klasörler için yerel önbellek kullanan uygulamalar. Var. Şimdi alt klasörler "benzersiz tanımlayıcı" + zaman damgası adlandırma desenini izler, bunların Her alt burada uygulama çalıştıran veya çalışan bir VM örneğine karşılık gelir.
* Diğer klasörler üzerinde _D:\home_ yerel önbellekte kalmasını ve paylaşılan içerik deposu için kopyalanmaz.
* Uygulama dağıtımı desteklenen herhangi bir yöntem aracılığıyla doğrudan kalıcı paylaşılan içerik deposu için yayımlar. Yenilemek için _D:\home\site_ ve _D:\home\siteextensions_ klasörler yerel önbellekte uygulamanın yeniden başlatılması gerekir. Yaşam döngüsü sorunsuz hale getirmek için bu makaledeki bilgileri bakın.
* SCM sitesine varsayılan içerik görünümü, paylaşılan içerik deposu olmaya devam eder.

## <a name="enable-local-cache-in-app-service"></a>App Service yerel önbellek etkinleştir
Yerel önbellek, ayrılmış uygulama ayarları bir bileşimini kullanarak yapılandırın. Aşağıdaki yöntemleri kullanarak bu uygulama ayarları yapılandırabilirsiniz:

* [Azure portal](#Configure-Local-Cache-Portal)
* [Azure Resource Manager](#Configure-Local-Cache-ARM)

### <a name="configure-local-cache-by-using-the-azure-portal"></a>Yerel önbellek Azure portalını kullanarak yapılandırma
<a name="Configure-Local-Cache-Portal"></a>

Bu uygulama ayarını kullanarak tek başına-web-app temelinde yerel önbelleği etkinleştir: `WEBSITE_LOCAL_CACHE_OPTION` = `Always`  

![Azure Portalı Uygulama ayarları: Yerel Önbellek](media/app-service-local-cache-overview/app-service-local-cache-configure-portal.png)

### <a name="configure-local-cache-by-using-azure-resource-manager"></a>Azure Resource Manager kullanarak yerel önbelleğini yapılandırma
<a name="Configure-Local-Cache-ARM"></a>

```json

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
Varsayılan olarak, yerel önbellek boyutu olan **300 MB**. Bu, yerel olarak oluşturulmuş günlükleri ve veri klasörleri yanı sıra /site ve içerik deposundan kopyalanan /siteextensions klasörleri içerir. Bu sınırı artırmak için uygulama ayarını kullanan `WEBSITE_LOCAL_CACHE_SIZEINMB`. En fazla boyutunu artırabilir **2 GB** (2000 MB) uygulama başına.

## <a name="best-practices-for-using-app-service-local-cache"></a>App Service yerel önbellek kullanmak için en iyi uygulamalar
Yerel önbellek ile birlikte kullanmanızı öneririz [hazırlama ortamları](../app-service/deploy-staging-slots.md) özelliği.

* Ekleme *Yapışkan* uygulama ayarı `WEBSITE_LOCAL_CACHE_OPTION` değerle `Always` için **üretim** yuvası. Kullanıyorsanız `WEBSITE_LOCAL_CACHE_SIZEINMB`, ayrıca, üretim yuvasına Yapışkan ayarı olarak ekleyin.
* Oluşturma bir **hazırlama** yuva ve hazırlama yuvanızı yayımlayın. Genellikle, üretim yuvası için yerel önbellek avantajlarından yararlanın, hazırlama için sorunsuz bir yapı-dağıtma-test yaşam etkinleştirmek üzere yerel önbelleği kullanılacak hazırlama yuvasına ayarlamanız gerekmez.
* Hazırlama yuvanızı karşı sitenizi test edin.  
* Hazır olduğunuzda, sorun bir [takas işlemi](../app-service/deploy-staging-slots.md#Swap) , hazırlık ve üretim arasında yuvası.  
* Ad ve bir yuvaya Yapışkan Yapışkan ayarları içerir. Bu nedenle üretime hazırlama yuvası takas, yerel önbellek uygulama ayarlarını devralır. Yeni takas üretim yuvasına yerel önbelleğe karşı birkaç dakika sonra çalışacak ve yuva Isınma bir parçası olarak değiştirme işleminden sonra warmed. Bu nedenle, üretim yuvası yuvası takas tamamlandığında yerel önbelleğe karşı çalışıyor.

## <a name="frequently-asked-questions-faq"></a>Sık sorulan sorular (SSS)
### <a name="how-can-i-tell-if-local-cache-applies-to-my-app"></a>Yerel önbellek uygulamama geçerli olup olmadığını nasıl anlayabilirim?
Uygulamanızı yüksek performanslı, güvenilir içerik deposuna ihtiyacı, kritik verileri çalışma zamanında yazılacak içerik deposuna kullanmaz ve toplam boyutu 2 GB'den küçük olduğundan, yanıt "Evet" demektir! /Site ve /siteextensions klasörlerinizi toplam boyutunu almak için site uzantısı "Azure Web Apps Disk kullanımı." kullanabilirsiniz.

### <a name="how-can-i-tell-if-my-site-has-switched-to-using-local-cache"></a>Sitem yerel önbelleği kullanmaya geçti nasıl anlayabilirim?
Hazırlama ortamları ile yerel önbellek özelliğini kullanıyorsanız, yerel önbellek warmed kadar değiştirme işlemi tamamlamak değil. Siteniz yerel önbelleğe karşı çalışıp çalışmadığını denetlemek için çalışan işlem ortam değişkenini denetleyebilir `WEBSITE_LOCALCACHE_READY`. Şirket yönergeleri kullanın [çalışan işlem ortam değişkeni](https://github.com/projectkudu/kudu/wiki/Process-Threads-list-and-minidump-gcdump-diagsession#process-environment-variable) birden fazla çalışan işlem ortam değişkeninde erişmek için sayfa.  

### <a name="i-just-published-new-changes-but-my-app-does-not-seem-to-have-them-why"></a>Yeni değişiklikler yayımlamış ancak uygulamamı bunları görünmüyor. Neden?
Uygulamanızı yerel önbelleği kullanıyorsa, siteniz en son değişiklikleri almak için yeniden başlatmanız gerekir. Bir üretim sitesini değişiklikleri yayımlamak istemiyor musunuz? En iyi yöntemler önceki bölümde yuvası seçeneklere bakın.

### <a name="where-are-my-logs"></a>My günlükleri nerededir?
Yerel önbellek ile günlükleri ve veri klasörleri biraz farklı görünür. Ancak, biçimi "VM tanımlayıcısı" + zaman damgası ile bir alt klasörü altında alt klasörleri nestled dışında kendi alt yapısını aynı kalır.

### <a name="i-have-local-cache-enabled-but-my--app-still-gets-restarted-why-is-that-i-thought-local-cache-helped-with-frequent-app-restarts"></a>Yerel önbellek etkin sahibim ancak uygulamamı yeniden. Bunun nedeni nedir? Sık sık uygulama yeniden başlatma ile yerel önbelleği Yardım düşündüm.
Yerel önbellek depolama ile ilgili uygulamalar yeniden önlemeye yardımcı olur. Ancak, uygulamanızı yeniden sanal makinenin planlanmış altyapısını yükseltmeler sırasında geçeriz yine de. Yerel önbellek etkin deneyimi genel uygulama yeniden başlatma, daha az olmalıdır.

### <a name="does-local-cache-exclude-any-directories-from-being-copied-to-the-faster-local-drive"></a>Yerel önbellek, daha hızlı yerel sürücüye kopyalanan herhangi bir dizin dışarıda tuttuğunu?
Depolama içeriği kopyalar adımının bir parçası, depo adlı herhangi bir klasörü dahil değildir. Bu uygulamanın günlük işlemlerinde gerekli değil bir kaynak denetim deposu site içeriğiniz burada içerebilir senaryoları ile yardımcı olur. 
