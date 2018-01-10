---
title: "Azure Uygulama Hizmeti için En İyi Uygulamalar"
description: "En iyi yöntemler ve Azure App Service için sorun giderme öğrenin."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: f3359464-fa44-4f4a-9ea6-7821060e8d0d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2016
ms.author: dariagrigoriu
ms.openlocfilehash: 1a36c11fcce33c0148fa7d0a4e947a9cc37cd276
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2018
---
# <a name="best-practices-for-azure-app-service"></a>Azure Uygulama Hizmeti için En İyi Uygulamalar
Bu makalede kullanmak için en iyi uygulamalar özetlenmektedir [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714). 

## <a name="colocation"></a>Birlikte bulundurma
Bir web uygulaması ve bir veritabanı gibi bir çözüm oluşturma Azure kaynaklarını farklı bölgelerde bulunduğunda etkilerini şunları içerebilir:

* Kaynaklar arasındaki iletişimde daha yüksek gecikme süresi
* Para ücretleri için giden veri aktarımı üzerinde belirtildiği gibi bölgeler arası [Azure fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/data-transfers).

Birlikte bulundurma aynı bölgede bir web uygulaması ve içerik veya verileri tutmak için kullanılan bir veritabanı veya depolama hesabı gibi bir çözüm oluşturma Azure kaynakları için en iyisidir. Belirli iş ya da bunları olmayacak biçimde nedeni tasarım sürece emin olmalısınız kaynakları oluştururken, aynı Azure bölgesinde oldukları. Yararlanarak veritabanınızla aynı bölge için bir App Service uygulaması taşıyabilirsiniz [uygulama kopyalama özelliği hizmeti](app-service-web-app-cloning.md) Premium App Service planı uygulamalar için kullanılabilir.   

## <a name="memoryresources"></a>Ne zaman beklenenden daha fazla bellek uygulamaları kullanma
Ne zaman fark uygulama izleme aracılığıyla belirtildiği gibi beklenenden daha fazla bellek kullanır veya hizmet önerileri göz önünde bulundurun [uygulama hizmeti otomatik düzeltme özelliği](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites). Otomatik Düzeltme özelliği için seçeneklerden birini bir bellek eşiğine dayalı özel eylemler sürüyor. Eylemler, çalışan işlem geri dönüştürme üzerindeki--nokta azaltma için bellek dökümü aracılığıyla araştırma için gelen e-posta bildirimleri spektrumun span. Otomatik Düzeltme yapılandırılabilir web.config aracılığıyla ve kolay bir kullanıcı arabirimi aracılığıyla için bu Web günlüğü gönderisinde bölümünde anlatıldığı gibi [App Service destek Site uzantısı](https://azure.microsoft.com/blog/additional-updates-to-support-site-extension-for-azure-app-service-web-apps).   

## <a name="CPUresources"></a>Ne zaman uygulamaları beklenenden daha fazla CPU kullanır
Ne zaman bir uygulama beklenen veya karşılaştığında olandan daha fazla CPU kullanan fark CPU ani izleme aracılığıyla belirtildiği şekilde yinelenen veya ölçeklendirmeyi veya uygulama hizmeti planı kullanıma ölçeklendirme hizmet önerileri göz önünde bulundurun. Uygulamanızı durum bilgisi olan, uygulamanız ise durum bilgisiz, ölçekleme çıkış size daha fazla esneklik ve daha yüksek ölçek olası sunar ancak ölçeklendirmeyi tek seçenek ise. 

"Durum bilgisi olan" vs "durum bilgisiz" uygulamalar hakkında daha fazla bilgi için bu videoyu izleyebilirsiniz: [ölçeklenebilir baştan sona çok katmanlı bir uygulama Microsoft Azure Web uygulaması üzerinde planlama](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B414#fbid=?hashlink=fbid). Uygulama hizmeti ölçekleme ve otomatik ölçeklendirme seçenekleri hakkında daha fazla bilgi için okumaya devam edin: [bir Web uygulamasını Azure App Service'te ölçeklendirme](web-sites-scale.md).  

## <a name="socketresources"></a>Ne zaman yuva kaynakları tükendi
Tükettiğini giden TCP bağlantıları için ortak bir nedeni TCP bağlantılarını yeniden uygulanmadı istemci kitaplıklarının ya da daha yüksek bir düzeyinde Protokolü HTTP - değil işlevden tutma gibi söz konusu olduğunda kullanılır. Lütfen her bir uygulama hizmeti yapılandırılmış veya kodunuz verimli kullanılmasını giden bağlantılar için erişilebilir emin olmak için planınız uygulamalar tarafından başvurulan kitaplıkları için belgelerini gözden geçirin. Ayrıca uygun oluşturma ve yayın veya bağlantıları sızmasını önlemek için temizleme kitaplığı belgeleri yönergeleri izleyin. Bu tür istemci kitaplıkları araştırmalar işlenirken ilerleme etkisi birden çok örneği ölçeğini tarafından azaltıldığından.

### <a name="nodejs-and-outgoing-http-requests"></a>Node.js ve giden http istekleri
Node.js ve birçok giden http istekleri ile çalışırken, HTTP - tutma postalarla gerçekten önemlidir. Kullanabileceğiniz [agentkeepalive](https://www.npmjs.com/package/agentkeepalive) `npm` kodunuzda kolaylaştırmak için paket.

Her zaman işlemelidir `http` işleyicisinde yapmıyor olsa bile yanıt. Yanıt düzgün bir şekilde işlemek yok, daha fazla hiçbir yuva kullanılabilir olmadığından, uygulamanızın sonunda takılıyor.

Örneğin, ile çalışırken `http` veya `https` paketi:

```
var request = https.request(options, function(response) {
    response.on('data', function() { /* do nothing */ });
});
```

Birden çok çekirdeğe sahip bir makinede Linux uygulama hizmeti üzerinde çalıştırıyorsanız, başka bir en iyi uygulama PM2 uygulamanızı yürütmek için birden çok Node.js işlemi başlatmak için kullanmaktır. Bu, kapsayıcı için bir başlangıç komut belirterek yapabilirsiniz.

Örneğin, dört örnekleri başlatmak için şunu yazın:

`pm2 start /home/site/wwwroot/app.js --no-daemon -i 4`

## <a name="appbackup"></a>Ne zaman uygulamanızı yedekleme başarısız başlatır
Uygulama yedekleme başarısız neden olan iki en yaygın nedenleri: Geçersiz depolama ayarları ve geçersiz veritabanı yapılandırması. Değişiklikleri depolama veya veritabanı kaynaklarına veya bu kaynakları (örneğin yedekleme ayarlarında seçilen veritabanı için güncelleştirilmiş kimlik) erişmek nasıl değişiklikleri olduğunda bu hatalar genellikle gerçekleşir. Yedeklemeleri genellikle bir zamanlamaya göre çalışmasını ve (için kopyalama ve yedeklemeye dahil edilecek içeriği okunurken) (Yedeklenen dosyaları kayıt çıkarma) depolama alanı ve veritabanı erişimi gerektirir. Ya da bu kaynaklara erişmek başarısız sonucu tutarlı yedekleme hatası olacaktır. 

Lütfen yedekleme hatası oluştuğunda, hangi tür hatanın gerçekleştiği anlamak için en son sonuçlarını gözden geçirin. Depolama erişim hatası durumunda gözden geçirin ve yedekleme yapılandırmada kullanılan depolama ayarlarını güncelleştirin. Veritabanına erişim hatası durumunda gözden geçirin ve uygulama ayarlarının bir parçası, bağlantı dizelerini güncelleştirmek; Yedekleme yapılandırmanızı düzgün gerekli veritabanlarını içerecek şekilde güncelleştirmek için sonra devam edin. Uygulama yedekleme hakkında daha fazla bilgi için lütfen bkz [Azure App Service'te bir web uygulaması yedekleme](web-sites-backup.md) belgeleri.

## <a name="nodejs"></a>Yeni Node.js uygulamaları Azure App Service'e zaman dağıtılır
Node.js uygulamaları için Azure uygulama hizmeti varsayılan yapılandırma, en yaygın uygulamaları gereksinimlerine uyacak şekilde tasarlanmıştır. Yapılandırma, Node.js uygulamanız için kişiselleştirilmiş performansı veya kaynak kullanımı CPU/bellek/ağ kaynakları için en iyi duruma getirmek için ayarlama gelen yararlanabileceğini, bizim en iyi yöntemler ve sorun giderme adımlarını gözden. Bu belge makalede Node.js uygulamanız için değiştirmeniz gerekebilir, çeşitli senaryolar açıklanmaktadır veya uygulamanızın karşılıklı ve bu sorunları gidermek nasıl gösterir sorunları iisnode ayarları açıklanır: [en iyi uygulamalar ve Azure App Service'te düğümü uygulamalar için sorun giderme kılavuzu](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md).   

