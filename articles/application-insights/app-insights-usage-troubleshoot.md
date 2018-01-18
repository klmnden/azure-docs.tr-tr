---
title: "Azure Application ınsights'ta kullanım analizi sorunlarını giderme"
description: "Sorun giderme kılavuzu - Application Insights ile site ve uygulama kullanımını analiz etme."
services: application-insights
documentationcenter: 
author: numberbycolors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 01/16/2018
ms.author: mbullwin
ms.openlocfilehash: cb5f3052301b23eb10cd6b84ab6fae98bcc7ea18
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="troubleshoot-usage-analytics-in-application-insights"></a>Application ınsights'ta kullanım analizi sorunlarını giderme
Hakkında sorularınız [kullanım analiz araçları Application ınsights'ta](app-insights-usage-overview.md): [kullanıcıları, oturumlar, olayları](app-insights-usage-segmentation.md), [Funnels](usage-funnels.md), [kullanıcı akar](app-insights-usage-flows.md), [Bekletme](app-insights-usage-retention.md), veya Cohorts? Burada bazı yanıtlar bulunmaktadır.

## <a name="counting-users"></a>Kullanıcıların sayım
**Bir kullanıcı oturum Uygulamam var, ancak Uygulamam bilmeniz araçlarını Göster kullanım analizi birçok kullanıcı oturumları sahiptir. Bu yanlış sayıları nasıl çözebilir mi?**

Application ınsights'ta tüm telemetri olaylarını sahip bir [anonim kullanıcı kimliği](application-insights-data-model-context.md) ve [oturum kimliği](application-insights-data-model-context.md) iki standart özellikleri olarak. Varsayılan olarak, tüm kullanım analizi araçları kullanıcı ve bu kimliklerine göre göre oturum sayısı. Bu standart özellikleri her kullanıcı ve uygulamanızın oturum için benzersiz kimlikler girilmiş olmayan sayımının yanlış kullanıcı ve kullanım analizi Araçları'nda oturum görürsünüz.

Bir web uygulaması izliyorsanız, en kolay çözüm eklemektir [Application Insights JavaScript SDK'sı](app-insights-javascript.md) , uygulama ve emin olun için kod parçacığını izlemek istediğiniz her sayfada yüklenir. JavaScript SDK'sı otomatik olarak anonim kullanıcı ve oturum kimliklerini oluşturur, ardından bu kimlikleri telemetri olayları uygulamanızdan gönderilen olarak doldurur.

Bir web hizmeti (hiçbir kullanıcı arabirimi) izliyorsanız [anonim kullanıcı kimliği ve oturum kimliği özelliklerini dolduran bir telemetri Başlatıcı oluşturmak](app-insights-usage-send-user-context.md) benzersiz kullanıcı ve oturum hizmetinizin kavramları göre.

Uygulamanızı gönderiyorsa [kullanıcı kimlikleri kimliği doğrulanmış](app-insights-api-custom-events-metrics.md#authenticated-users), temel alınarak kimliği doğrulanmış kullanıcı kimlikleri kullanıcılar aracında sayabilirsiniz. "Göster" açılır listede "Kimliği doğrulanan kullanıcılar" seçin

Kullanım analiz araçları sayım kullanıcılar veya anonim kullanıcı kimliği, kimliği doğrulanmış kullanıcı kimliği veya oturum kimliği dışındaki özellikleri bağlı oturumlar şu anda desteklenmiyor

## <a name="naming-events"></a>Adlandırma olayları
**Uygulamam binlerce farklı sayfa görünümü ve özel olay adları var. Bunlar arasında ayrım yapmak zor ve kullanım analiz araçları genellikle yanıt veremez duruma gelebilir. Bu adlandırma sorunları nasıl çözebilir mi?**

Sayfa görünümü ve özel olay adları kullanım analiz araçları kullanılır. Olayları adlandırması da bu Araçları'ndan değer almak için önemlidir. Arasında bir denge hedeftir sahip çok az, aşırı genel adları ("tıklattınız Button") ve çok sayıda, aşırı belirli adları ("http://www.contoso.com/index üzerinde tıkladığınız Düzenle düğmesi") sahip.

Değişiklik sayfa görünümü ve uygulamanızı gönderme özel olay adları için uygulamanızın kaynak kodunu ve yeniden dağıtın değiştirmeniz gerekir. **Application Insights verilerde 90 gün süreyle depolanır ve silinemez tüm telemetri**, olay adları için yaptığınız değişiklikler tam listesi için 90 gün sürer. 90 ad değişiklikleri yaptıktan sonra gün için eski ve yeni olay adlarını, telemetri görünmesini, böylece sorguları ayarlamak ve ekipleriniz içinde uygun şekilde iletişim.

Uygulamanız çok sayıda sayfa görünümü adları gönderiyorsa, bu sayfa görünümü adları kodunda el ile belirtilen olup olmadığını veya bunlar otomatik olarak Application Insights JavaScript SDK tarafından gönderilen olmadığını denetleyin:

* Sayfa görünümü adlarını el ile bir kodun kullanıldığı belirtilmezse, [ `trackPageView` API](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md), daha az belirli olmasını adını değiştirin. Sayfa görünümü adına URL koyma gibi yaygın hataları kaçının. Bunun yerine, URL parametresini kullanın `trackPageView` API. Diğer ayrıntılar sayfa görünümü adından özel özelliklerini taşıyın.

* Application Insights JavaScript SDK'sı otomatik olarak sayfa görünümü adları gönderiyorsa, sayfalarınızı başlıklarını değiştirin veya sayfa görünümü adlarını el ile gönderme için geçiş. SDK gönderir [başlık](https://developer.mozilla.org/docs/Web/HTML/Element/title) sayfası Görünüm adı, varsayılan olarak her sayfanın. Daha fazla genel ancak SEO ve bu değişiklik olabilir başka etkileri oluşturduğunu başlıklarınızın değiştirebilirsiniz. Sayfa görünümü el ile belirtme adları ile `trackPageView` API sayfa başlıklarını değiştirmeden daha genel adları telemetri gönderebilir şekilde otomatik olarak toplanan adları geçersiz kılar.   

Uygulamanız çok fazla özel olay adları gönderiyorsa, daha az belirli olmasını kodda adını değiştirin. Yeniden, URL ve diğer sayfa başına veya dinamik bilgilerini özel olay adlarında doğrudan yerleştirmeyin. Bunun yerine, bu ayrıntıları özel olayla özel özelliklerini taşınıp `trackEvent` API. Örneğin, yerine, `appInsights.trackEvent("Edit button clicked on http://www.contoso.com/index")`, aşağıdakine benzer önerdiğimiz `appInsights.trackEvent("Edit button clicked", { "Source URL": "http://www.contoso.com/index" })`.

## <a name="next-steps"></a>Sonraki adımlar

* [Kullanım analizi genel bakış](app-insights-usage-overview.md)

## <a name="get-help"></a>Yardım alın
* [Stack Overflow](http://stackoverflow.com/questions/tagged/ms-application-insights)

