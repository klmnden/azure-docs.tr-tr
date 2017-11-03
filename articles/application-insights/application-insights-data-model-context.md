---
title: "Azure uygulama Insights Telemetri veri modeli - Telemetri bağlamı | Microsoft Docs"
description: "Uygulama Insights telemetri bağlamı veri modeli"
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 05/15/2017
ms.author: sergkanz
ms.openlocfilehash: d6a0cad8bda6ca68aa691867e84f540c5ac9f6f3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="telemetry-context-application-insights-data-model"></a>Telemetri bağlamı: Application Insights veri modeli

Her telemetri öğeyi kesin türü belirtilmiş bağlam alanları olabilir. Her alan belirli bir izleme senaryosu sağlar. Özel özellikler koleksiyonu veya uygulamaya özgü özel bağlamsal bilgileri depolamak için kullanın.


##<a name="application-version"></a>Uygulama sürümü

Uygulama bağlamı alanları her zaman telemetri gönderiyor uygulama hakkındaki bilgilerdir. Uygulama sürümü, uygulama davranışına ve dağıtımları için kendi bağıntı eğilimi değişiklikleri analiz etmek için kullanılır.

En fazla uzunluk: 1024


##<a name="client-ip-address"></a>İstemci IP adresi

İstemci cihazının IP adresi. IPv4 ve IPv6 desteklenir. Telemetri bir hizmetten gönderildiğinde, konum bağlam işlemi hizmetinde başlatılan kullanıcı hakkındadır. Application Insights coğrafi konum bilgilerini istemci IP ayıklayın ve ardından kesme. Bu nedenle istemci IP kendisi tarafından son kullanıcı bilgilerinizle kullanılamaz. 

En fazla uzunluk: 46


##<a name="device-type"></a>Aygıt türü

İlk olarak bu alan, uygulamanın son kullanıcı kullanarak aygıt türünü belirtmek için kullanıldı. Bugün öncelikle cihaz türü ile JavaScript telemetri ayırt etmek için kullanılan sunucu tarafı telemetri aygıtla ' tarayıcıdan' 'PC' yazın.

En fazla uzunluk: 64


##<a name="operation-id"></a>İşlem kimliği

Kök işlemi benzersiz tanıtıcısı. Bu tanımlayıcı arasında birden çok bileşen grubu telemetri sağlar. Bkz: [telemetri bağıntı](application-insights-correlation.md) Ayrıntılar için. İşlem kimliği, bir istek veya bir sayfa görünümü tarafından oluşturulur. Diğer tüm telemetri bu alanı içeren istek veya sayfa görünümü değerine ayarlar. 

En fazla uzunluk: 128


##<a name="parent-operation-id"></a>Üst işlem kimliği

Telemetri öğesi'nin en yakın üst benzersiz tanımlayıcısı. Bkz: [telemetri bağıntı](application-insights-correlation.md) Ayrıntılar için.

En fazla uzunluk: 128


##<a name="operation-name"></a>İşlem adı

(Grup) işlemin adı. İşlem adı, bir istek veya bir sayfa görünümü tarafından oluşturulur. Diğer tüm telemetri öğeleri bu alanını içeren istek veya sayfa görünümü için değer ayarlayın. İşlem adı işlemlerini (örneğin ' GET Home/Index') grubu için tüm telemetri öğeleri bulmak için kullanılır. Bu bağlam özellik yanıtlamak için kullanılır gibi soruları "Bu sayfada oluşturulan tipik özel durumları nelerdir."

En fazla uzunluk: 1024


##<a name="synthetic-source-of-the-operation"></a>Yapay işlem kaynağı

Yapay kaynağının adı. Birkaç telemetri uygulamadan yapay trafiği temsil edebilir. Web sitesi, site kullanılabilirlik testleri veya Application Insights SDK kendisini gibi tanılama kitaplıklarından izlemeleri dizinini web Gezgini olabilir.

En fazla uzunluk: 1024


##<a name="session-id"></a>Oturum kimliği

Oturum kimliği - kullanıcının uygulamayla etkileşimi örneği. Oturum bağlamı alanları her zaman son kullanıcı bilgilerdir. Telemetri bir hizmetten gönderildiğinde, hizmet işlemi başlatan kullanıcı oturum bağlamı hakkındadır.

En fazla uzunluk: 64


##<a name="anonymous-user-id"></a>Anonim kullanıcı kimliği

Anonim kullanıcı kimliği. Son kullanıcı uygulamanın temsil eder. Telemetri bir hizmetten gönderildiğinde, kullanıcı bağlamı hizmet işlemi başlatan kullanıcı hakkındadır.

[Örnekleme](app-insights-sampling.md) toplanan telemetri en aza indirmek için kullanılan teknikleri biridir. Her iki örnek veya tüm ilişkili telemetri uzaklaştırma örnekleme algoritması dener. Anonim kullanıcı kimliği, puan nesil örnekleme için kullanılır. Bu nedenle anonim kullanıcı kimliği yeterince rastgele bir değeri olmalıdır. 

Kullanıcı adı depolamak için anonim kullanıcı kimliği'ni kullanarak kötüye alanının değildir. Kimliği doğrulanmış kullanıcı kimliği kullanın.

En fazla uzunluk: 128


##<a name="authenticated-user-id"></a>Kimliği doğrulanmış kullanıcı kimliği

Kimliği doğrulanmış kullanıcı kimliği. Anonim kullanıcı kimliği karşıtı, bu alan bir kolay ad kullanıcıyla temsil eder. PII bilgilerini bu yana çoğu SDK'sı tarafından varsayılan olarak toplanmaz.

En fazla uzunluk: 1024


##<a name="account-id"></a>Hesap Kimliği

Çok kiracılı uygulamalara hesap kimliği veya kullanıcı ile hareket adı budur. Örnekler, abonelik kimliği için Azure portal veya blog adı blog platformu olabilir.

En fazla uzunluk: 1024


##<a name="cloud-role"></a>Bulut rolü

Uygulama rolü adı bir parçasıdır. Azure rol adını doğrudan eşleşir. Ayrıca tek bir uygulamanın parçası olan mikro hizmetler ayırt etmek için kullanılabilir.

En fazla uzunluk: 256


##<a name="cloud-role-instance"></a>Bulut rol örneği

Uygulamanın çalıştığı örneğinin adı. Bilgisayar adı şirket içi, Azure için örnek adı.

En fazla uzunluk: 256


##<a name="internal-sdk-version"></a>Dahili: SDK sürümü

SDK sürümü. Https://github.com/Microsoft/ApplicationInsights-Home/BLOB/master/SDK-AUTHORING.MD#SDK-Version-Specification bilgi için bkz.

En fazla uzunluk: 64


##<a name="internal-node-name"></a>Dahili: Düğüm adı

Bu alan, fatura amacıyla kullanılan düğüm adı temsil eder. Standart algılama düğümlerinin geçersiz kılmak için kullanın.

En fazla uzunluk: 256


## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [genişletmek ve filtre telemetri](app-insights-api-filtering-sampling.md).
- Bkz: [veri modeli](application-insights-data-model.md) Application Insights türleri ve veri modeli için.
- Standart bağlam özellikleri koleksiyonunu denetleyin [yapılandırma](app-insights-configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet).
