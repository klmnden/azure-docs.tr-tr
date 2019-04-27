---
title: Azure Application Insights Telemetri veri modeli - Telemetri bağlam | Microsoft Docs
description: Application Insights telemetri bağlam veri modeli
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/15/2017
ms.reviewer: sergkanz
ms.author: mbullwin
ms.openlocfilehash: 7c1f47c9b88bd68b326b3c8923ba5b81d425c3e4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60900719"
---
# <a name="telemetry-context-application-insights-data-model"></a>Telemetri Bağlam: Application Insights veri modeli

Her telemetri öğesine bir türü kesin belirlenmiş bağlam alanları olabilir. Her alan, belirli bir izleme senaryosu sağlar. Özel veya uygulamaya özgü bağlamsal bilgi depolamak için özel özellikler koleksiyonu kullanın.


## <a name="application-version"></a>Uygulama sürümü

Uygulama bağlamı alanları her zaman telemetri gönderdiği uygulama hakkındaki bilgilerdir. Uygulama sürümü, uygulama davranışını ve dağıtımları kendi bağıntı eğilim değişiklikleri analiz etmek için kullanılır.

En fazla uzunluk: 1024


## <a name="client-ip-address"></a>İstemci IP adresi

İstemci cihazının IP adresi. IPv4 ve IPv6 desteklenir. Bir hizmetten telemetri gönderildiğinde, konum bağlam işlemi hizmetinde başlatılan kullanıcı hakkındadır. Application Insights, istemci IP coğrafi konum bilgileri ayıklayın ve ardından kesecek. Bu nedenle istemci IP'si kendisi tarafından son kullanıcı olarak tanımlanabilir bilgiler kullanılamaz. 

En fazla uzunluk: 46


## <a name="device-type"></a>Cihaz türü

İlk olarak bu alan, uygulamanın son kullanıcı kullanarak cihaz türünü belirtmek için kullanıldı. Bugün öncelikle JavaScript telemetri cihaz türü ile ayırt etmek için kullanılan sunucu tarafı telemetri cihaz ' tarayıcıdan' 'PC' yazın.

En fazla uzunluk: 64


## <a name="operation-id"></a>İşlem kimliği

Kök işlemin benzersiz bir tanımlayıcı. Bu tanımlayıcı arasında birden çok bileşen grubu telemetri sağlar. Bkz: [telemetri bağıntısı](../../azure-monitor/app/correlation.md) Ayrıntılar için. İşlem kimliği, bir istek veya sayfa görünümü tarafından oluşturulur. Diğer tüm telemetri Bu alan içeren bir istek veya sayfa görüntüleme değerini ayarlar. 

En fazla uzunluk: 128


## <a name="parent-operation-id"></a>Üst işlem kimliği

İlk üst öğe telemetri öğesinin benzersiz tanımlayıcısı. Bkz: [telemetri bağıntısı](../../azure-monitor/app/correlation.md) Ayrıntılar için.

En fazla uzunluk: 128


## <a name="operation-name"></a>İşlem adı

(Grup) olan işlemin adı. İşlem adı, bir istek veya sayfa görünümü tarafından oluşturulur. Diğer tüm telemetri öğeleri bu alan içeren bir istek veya sayfa görüntüleme değerini ayarlayın. İşlem adı (örneğin ' GET Home/Index') işlem grubu için tüm telemetri öğeleri bulmak için kullanılır. Bu bağlam özelliği yanıtlamak için kullanılan "Bu sayfada oluşturulan tipik özel durumları nelerdir." gibi sorular sorun

En fazla uzunluk: 1024


## <a name="synthetic-source-of-the-operation"></a>Yapay işlem kaynağı

Yapay kaynağının adı. Yapay trafik uygulama alınan bazı telemetri temsil edebilir. Web sitesi, site kullanılabilirlik testleri veya Application Insights SDK kendisi gibi tanılama kitaplıkları izlemelerinden dizin web Gezgini olabilir.

En fazla uzunluk: 1024


## <a name="session-id"></a>Oturum kimliği

Oturum kimliği - kullanıcının uygulamayla etkileşimi örneği. Oturum bağlamı alanları her zaman son kullanıcı bilgilerdir. Oturum bağlamı, hizmetten telemetri gönderildiğinde, işlemi hizmetinde başlatılan kullanıcı hakkındadır.

En fazla uzunluk: 64


## <a name="anonymous-user-id"></a>Anonim kullanıcı kimliği

Anonim kullanıcı kimliği. Son kullanıcı uygulamasının temsil eder. Bir hizmetten telemetri gönderildiğinde, kullanıcı bağlamı hizmetinde işlemi başlatan kullanıcı hakkındadır.

[Örnekleme](../../azure-monitor/app/sampling.md) toplanan telemetri miktarını en aza indirmek için teknikleri biridir. Her iki örnek veya tüm ilişkili telemetri dışa örnekleme algoritması dener. Anonim kullanıcı kimliği, puan nesil örnekleme için kullanılır. Bu nedenle anonim kullanıcı kimliği yeterince rastgele bir değer olmalıdır. 

Anonim kullanıcı kimliği kullanıcı adı depolamak için bir alan b.internet kullanmaktır. Kimliği doğrulanmış kullanıcı kimliği kullanın.

En fazla uzunluk: 128


## <a name="authenticated-user-id"></a>Kimliği doğrulanmış kullanıcı kimliği

Kimliği doğrulanmış kullanıcı kimliği. Anonim kullanıcı kimliği tersine, bu alanın kolay adı ile kullanıcı temsil eder. PII bilgilerini beri çoğu SDK'sı tarafından varsayılan olarak toplanmaz.

En fazla uzunluk: 1024


## <a name="account-id"></a>Hesap kimliği

Çok kiracılı uygulamalarda hesap kimliği veya adı, kullanıcı ile çalışan budur. Örnekler, abonelik kimliği için Azure portalı ya da blogda adı blog oluşturma platformu olabilir.

En fazla uzunluk: 1024


## <a name="cloud-role"></a>Bulut rolü

Uygulama rolü adıyla bir parçasıdır. Azure rol adını doğrudan eşleşir. Ayrıca tek bir uygulamanın parçası olan mikro hizmetler ayırt etmek için kullanılabilir.

En fazla uzunluk: 256


## <a name="cloud-role-instance"></a>Bulut rol örneği

Uygulamanın çalıştığı örneğinin adı. Bilgisayar adı'için şirket içi, Azure için örnek adı.

En fazla uzunluk: 256


## <a name="internal-sdk-version"></a>İç: SDK sürümü

SDK sürümü. Bkz: https://github.com/Microsoft/ApplicationInsights-Home/blob/master/SDK-AUTHORING.md#sdk-version-specification bilgi.

En fazla uzunluk: 64


## <a name="internal-node-name"></a>İç: Düğüm adı

Bu alan, faturalandırma için kullanılan düğüm adı temsil eder. Standart algılama düğümlerinin geçersiz kılmak için kullanın.

En fazla uzunluk: 256


## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [genişletmek ve telemetri filtreleme](../../azure-monitor/app/api-filtering-sampling.md).
- Bkz: [veri modeli](data-model.md) için Application Insights türleri ve veri modeli.
- Standart bağlam özellikleri koleksiyonunu kontrol [yapılandırma](../../azure-monitor/app/configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet).
