---
title: En iyi uygulamalar - Azure uygulama hizmeti
description: En iyi yöntemler ve sorun giderme için Azure App Service'ı öğrenin.
services: app-service
documentationcenter: ''
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: f3359464-fa44-4f4a-9ea6-7821060e8d0d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/01/2016
ms.author: dariagrigoriu
ms.custom: seodec18
ms.openlocfilehash: fc3749a9ebfbf0319a57b471b6fce9f62042ba27
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60852691"
---
# <a name="best-practices-for-azure-app-service"></a>Azure Uygulama Hizmeti için En İyi Uygulamalar
Bu makalede kullanmak için en iyi uygulamalar özetlenmektedir [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714). 

## <a name="colocation"></a>Birlikte bulundurma
Farklı bölgelerdeki Azure kaynakları bir web uygulaması ve bir veritabanı gibi bir çözüm oluşturma bulunduğunda, aşağıdaki etkileri sahip olabilir:

* Daha yüksek gecikme süresiyle kaynakları arasındaki iletişim
* Parasal ücretleri için giden veri aktarımı üzerinde belirtildiği gibi bölgeler arası [Azure fiyatlandırma sayfasını](https://azure.microsoft.com/pricing/details/data-transfers).

Birlikte bulundurma aynı bölgedeki bir web uygulaması ve içerik veya verileri tutmak için kullanılan bir veritabanı veya depolama hesabı gibi bir çözüm oluşturma Azure kaynakları için idealdir. Kaynakları oluşturulurken belirli iş ya da bunları olmayacak biçimde nedeni tasarım sürece, aynı Azure bölgesinde olduğundan emin olun. Kullanarak aynı bölgede veritabanı için bir App Service uygulaması taşıyabilirsiniz [App Service kopyalama özelliği](app-service-web-app-cloning.md) Premium App Service planı uygulamalar için kullanılabilir.   

## <a name="memoryresources"></a>Ne zaman beklenenden daha fazla bellek uygulamalarını kullanma
Belirtilen izleme veya hizmet önerileri, bir uygulama olarak beklenenden daha fazla bellek tüketir olduğunu fark, göz önünde bulundurun [App Service otomatik olarak onarma özelliği](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites). Otomatik düzeltmeyi özelliği için seçeneklerden birini Bellek Eşiği temel alarak özel eylemler sürüyor. Eylemler, e-posta bildirimleri yelpazesinden şirket--nokta risk azaltma için bellek dökümü aracılığıyla araştırma için çalışan işlemi geri dönüştürerek yayılır. Otomatik düzeltmeyi yapılandırılabilir web.config aracılığıyla ve kolay bir kullanıcı arabirimi aracılığıyla için bu blog gönderisinde açıklandığı [uygulama hizmeti destek Site uzantısı](https://azure.microsoft.com/blog/additional-updates-to-support-site-extension-for-azure-app-service-web-apps).   

## <a name="CPUresources"></a>Ne zaman beklenenden daha fazla CPU uygulamalarını kullanma
Fark, uygulama beklenenden daha fazla CPU kullanan veya artırma veya azaltma App Service planı kullanıma deneyimleri yinelenen CPU'daki ani değişikliklerin izleme veya hizmet önerileri belirtildiği gibi düşünün. Uygulamanızı durum bilgisi olan, uygulamanız varsa durum bilgisi olmayan, ölçeklendirme çıkış size daha fazla esneklik ve daha yüksek ölçek olası getirirken büyütme tek seçenek ise. 

"Durum bilgisi olan" vs "durum bilgisi olmayan" uygulamalar hakkında daha fazla bilgi için bu videoyu izleyebilirsiniz: [Ölçeklenebilir uçtan uca çok katmanlı uygulaması üzerinde Azure App Service'te planlama](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B414#fbid=?hashlink=fbid). App Service, ölçeklendirme ve otomatik ölçeklendirme seçenekleri hakkında daha fazla bilgi için bkz. [Web uygulamasını Azure App Service'te ölçeklendirme](web-sites-scale.md).  

## <a name="socketresources"></a>Ne zaman yuva kaynakları tükendi
Tükettiğini giden TCP bağlantılarına yaygın bir nedeni, istemci kitaplıklarının hangi TCP bağlantılarını yeniden uygulanmadı veya HTTP - tutma gibi daha üst düzey bir protokol değil kullanıldığında kullanılır. App Service yapılandırılmış veya verimli kullanılmasını giden bağlantılar için kodunuza erişir emin olmak için planınızı uygulamalar tarafından başvurulan kitaplıkların her birinde belgelerini gözden geçirin. Ayrıca uygun oluşturma ve yayın veya bağlantıları sızdırılmasını önlemek için temizleme kitaplığı belgeleri yönergeleri izleyin. Bu istemci kitaplıklarını araştırmalar devam ederken, etkisi birden fazla örneğe ölçek genişletilerek azaltılması.

### <a name="nodejs-and-outgoing-http-requests"></a>Node.js ve giden http istekleri
Birçok giden http isteklerini ve node.js ile çalışırken, HTTP - tutma uğraşmanızı önemlidir. Kullanabileceğiniz [agentkeepalive](https://www.npmjs.com/package/agentkeepalive) `npm` kodunuzda kolaylaştırmak için paket.

Her zaman işlemek `http` işleyici, hiçbir şey yapma olsa bile yanıt. Yanıt düzgün şekilde işlemez, hiçbir daha fazla yuva kullanılabilir olmadığından, uygulamanızın sonunda takılı.

Örneğin, ile çalışırken `http` veya `https` paket:

```javascript
const request = https.request(options, function(response) {
    response.on('data', function() { /* do nothing */ });
});
```

Linux üzerinde App Service'te birden çok çekirdeğe sahip bir makine üzerinde çalıştırıyorsanız, başka bir en iyi PM2 uygulamanızı yürütmek için birden çok Node.js işlemlerine başlamak için kullanmaktır. Kapsayıcı için bir başlangıç komutu belirterek bunu yapabilirsiniz.

Örneğin, dört örnek başlatmak için şunu yazın:

```
pm2 start /home/site/wwwroot/app.js --no-daemon -i 4
```

## <a name="appbackup"></a>Ne zaman uygulamanızı yedekleme başarısız başlatır
Uygulama yedekleme başarısız neden olan iki en yaygın nedenleri: Geçersiz depolama ayarlarını ve geçersiz veritabanı yapılandırması. Bu hatalar genellikle depolama veya veritabanı kaynaklarına değişiklikleri veya bu kaynaklara (örneğin, yedekleme ayarlarında seçili veritabanı için güncelleştirilmiş kimlik) öğrenmek için değişiklikler olduğunda gerçekleşir. Yedeklemeler, genellikle bir zamanlamaya göre çalıştırın ve (için kopyalama ve yedeklemeye dahil edilecek içeriği okunurken) depolama (Yedeklenen dosyaların çıktısı için) ve veritabanlarına erişim gerektirir. Ya da bu kaynakları erişmek başarısız sonucu tutarlı yedekleme başarısız olacaktır. 

Yedekleme hataları meydana geldiğinde, hangi tür hataları olup olmadığını anlamak için en son sonuçları gözden geçirin. Depolama erişim hataları için gözden geçirin ve yedekleme yapılandırmasında kullanılan depolama ayarlarını güncelleştirin. Veritabanı erişim hataları gözden geçirin ve uygulama ayarlarının bir parçası, bağlantı dizelerini güncelleştirin; ardından yedekleme yapılandırmanızı doğru gerekli veritabanlarını içerecek şekilde güncelleştirmek için devam edin. Uygulama yedeklemeleri hakkında daha fazla bilgi için bkz. [Azure App Service'te bir web uygulamasını yedekleme](manage-backup.md).

## <a name="nodejs"></a>Ne zaman yeni bir Node.js uygulamalarını Azure App Service'e dağıtılır
Node.js uygulamaları için Azure App Service varsayılan yapılandırma, en yaygın uygulamalarının ihtiyaçlarını en iyi uyacak şekilde yöneliktir. Node.js uygulamanız için yapılandırma kişiselleştirilmiş ayarlama gelen performansını veya CPU/bellek/ağ kaynakları için kaynak kullanımını en iyi duruma görmek için yararlı [en iyi yöntemler ve Azure uygulamasında node.js uygulamaları için sorun giderme kılavuzu Hizmet](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md). Bu makalede, Node.js uygulamanız için değiştirmeniz gerekebilir, çeşitli senaryolar açıklanmaktadır veya uygulamanızı karşılaşmış ve bu sorunları gidermeye yönelik gösterilmektedir sorunları iisnode ayarları açıklanır.

