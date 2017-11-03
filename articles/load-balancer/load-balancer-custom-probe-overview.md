---
title: "Yük Dengeleyici özel araştırmalar ve sistem durumu izleme | Microsoft Docs"
description: "Yük dengeleyicinin arkasındaki örnekleri izlemek için Azure yük dengeleyici için özel araştırmalara kullanmayı öğrenin"
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 102c07ff0994b3b411f2a13d7a43c5398d5dfd42
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="understand-load-balancer-probes"></a>Yük dengeleyici araştırmalarını anlama

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Azure yük dengeleyici araştırmalar kullanarak sunucu örneklerinin durumunu izleme yeteneği sağlar. Yanıt bir araştırma başarısız olduğunda, yük dengeleyici sağlıksız örneğine yeni bağlantılar gönderme durdurur. Var olan bağlantıların etkilenmez ve yeni bağlantılar sağlıklı örneklerine gönderilir.

Bulut hizmeti rollerinizi (çalışan rolleri ve web rolleri) bir konuk Aracısı araştırma izlemek için kullanın. Sanal makineler yük dengeleyici arkasında kullandığınızda, TCP veya HTTP özel araştırmalara yapılandırılması gerekir.

## <a name="understand-probe-count-and-timeout"></a>Araştırma sayısı ve zaman aşımı anlama

Araştırma davranışı bağlıdır:

* Bir örnek olarak etiketlenmiş izin başarılı araştırmalar sayısı olarak çalışır.
* Bir örnek olarak etiketlenmiş neden başarısız araştırmalar sayısı olarak aşağı.

Zaman aşımı ve sıklığı değeri SuccessFailCount ayarlanmış belirlemek örneğini çalıştıran veya çalışmıyor onaylanıp onaylanmadığını. Azure portalında zaman aşımı için sıklığı değerini iki kez ayarlanır.

Tüm örneklerin yük dengeli bir uç nokta (diğer bir deyişle, yük dengeli kümesi) için araştırma yapılandırması aynı olmalıdır. Başka bir deyişle, belirli bir uç nokta birleşimi için aynı barındırılan hizmetindeki her rol örneği veya sanal makine için farklı araştırma yapılandırmasını sahip olamaz. Örneğin, her bir örneği aynı yerel bağlantı noktaları ve zaman aşımlarına olması gerekir.

> [!IMPORTANT]
> Bir yük dengeleyici araştırmasını 168.63.129.16 IP adresini kullanır. Bu ortak IP adresini Getir-bilgisayarınızı-kendi-IP Azure sanal ağı senaryosu için iç platform kaynaklarına iletişimi kolaylaştırır. Genel sanal IP adresi 168.63.129.16 tüm bölgelerde kullanılır ve değişmez. Tüm yerel güvenlik duvarı ilkeleri bu IP adresine izin öneririz. Yalnızca dahili Azure platformu bu adresi bir iletiden kaynağı bir güvenlik riski düşünülmemelidir. Bunu yaparsanız, çeşitli senaryolarda 168.63.129.16 ve IP adreslerini yinelenen aynı IP adresi aralığı yapılandırma gibi beklenmeyen davranışlara olacaktır.

## <a name="learn-about-the-types-of-probes"></a>Araştırmalar türleri hakkında bilgi edinin

### <a name="guest-agent-probe"></a>Konuk aracı araştırması

Bu araştırma yalnızca Azure bulut Hizmetleri için kullanılabilir. Yük Dengeleyici sanal makine içinde Konuk Aracısı'nı kullanır ve dinler ve yalnızca örnek hazır durumda olduğunda bir HTTP 200 Tamam yanıt ile yanıt verir (diğer bir deyişle, içinde başka durum geri dönüştürme ya da durdurma meşgul gibi).

Daha fazla bilgi için bkz: [Hizmet tanım dosyası (csdef) için sistem durumu araştırmalarının yapılandırma](https://msdn.microsoft.com/library/azure/ee758710.aspx) veya [bulut Hizmetleri için bir Internet'e yönelik Yük Dengeleyici oluşturmaya başlamak](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services).

### <a name="what-makes-a-guest-agent-probe-mark-an-instance-as-unhealthy"></a>Bir örneği sağlıksız olarak işaretlemek Konuk aracı araştırması ne yapar?

Konuk Aracısı ile HTTP 200 Tamam yanıt vermiyorsa, yük dengeleyici örneği yanıt olarak işaretler ve trafiği için bu örneği göndermeye durdurur. Yük Dengeleyici örneği ping işlemi devam eder. Yük Dengeleyici trafiği için bu örneği ile bir HTTP 200 Konuk aracısı yanıt verirse, yeniden gönderir.

Web rolü kullandığınızda, Web sitesi kodu genellikle Azure tarafından izlenmeyen w3wp.exe çalışan doku veya konuk Aracısı. Bu w3wp.exe (örneğin, HTTP 500 yanıtları) hatalarına Konuk aracıya raporlanmayacaktır ve yük dengeleyici döndürme dışında bu örneği olmayacak anlamına gelir.

### <a name="http-custom-probe"></a>HTTP özel araştırma

Özel HTTP yük dengeleyici araştırmasını rol örneğinin sistem durumunu belirlemek için kendi özel mantık oluşturabileceğiniz anlamına gelir varsayılan Konuk aracı araştırması geçersiz kılar. Yük Dengeleyici uç noktanızı varsayılan olarak 15 dakikada araştırmaları. Örnek, bir HTTP 200 ile (varsayılan olarak 31 saniye) zaman aşımı süresi içinde yanıt verirse yük dengeleyici döndürme olarak kabul edilir.

Bu yük dengeleyici döndürme örneklerini kaldırmak için kendi mantığını uygulamak istediğinizde yararlı olabilir. Örneğin, % 90 CPU ise ve 200 olmayan durum döndürür örneği kaldırmaya karar. W3wp.exe kullanan web rolleri varsa, bu da otomatik al anlamına gelir, Web sitesini, Web sitesi kodunuzdaki hataları için yük dengeleyici araştırmasını 200 durum geri getireceğinden izleme.

> [!NOTE]
> HTTP özel araştırma göreli yollar ve yalnızca HTTP protokolünü destekler. HTTPS desteklenmiyor.

### <a name="what-makes-an-http-custom-probe-mark-an-instance-as-unhealthy"></a>Bir örneği sağlıksız olarak işaretlemek bir HTTP özel araştırma ne yapar?

* HTTP uygulama 200 (örneğin, 403, 404 veya 500) dışında bir HTTP yanıt kodunu döndürür. Uygulama örneğinin hizmet dışı hemen alınıp alınmayacağını pozitif bir bildirim budur.
* HTTP sunucusu zaman aşımı süresinden sonra hiç yanıt vermez. Belirlenen zaman aşımı değeri bağlı olarak, birden çok araştırma istekleri yoklama çalışmıyor olarak işaretlenmiş önce yanıtlanmadan Git bu anlamına gelebilir (diğer bir deyişle, önce SuccessFailCount araştırmalar gönderilir).
* Sunucu TCP sıfırlama aracılığıyla bağlantıyı kapatır.

### <a name="tcp-custom-probe"></a>TCP özel araştırma

TCP araştırmalar üç yönlü el sıkışma tanımlanan bağlantı noktası ile gerçekleştirerek bir bağlantı başlatır.

### <a name="what-makes-a-tcp-custom-probe-mark-an-instance-as-unhealthy"></a>Bir örneği sağlıksız olarak işaretlemek TCP özel bir araştırma yapan nedir?

* TCP sunucu zaman aşımı süresinden sonra hiç yanıt vermez. Ne zaman araştırma çalışmıyor olarak işaretlenmiş sayısı, araştırma çalışmıyor olarak işaretlemek önce yanıtlanmadan gitmek için yapılandırılmış olan başarısız araştırma isteklerinin bağlıdır.
* Rol örneğinden sıfırlama bir TCP araştırması alır.

Bir HTTP durum araştırması veya bir TCP araştırması yapılandırma hakkında daha fazla bilgi için bkz: [PowerShell kullanarak Kaynak Yöneticisi'nde bir Internet'e yönelik Yük Dengeleyici oluşturmaya başlamak](load-balancer-get-started-internet-arm-ps.md).

## <a name="add-healthy-instances-back-into-load-balancer-rotation"></a>Yük Dengeleyici döndürme uygulamasına geri sağlıklı örnekleri Ekle

TCP ve HTTP araştırmaları sağlıklı olarak kabul edilir ve rol örneği sağlıklı olduğunda olarak işaretleyin:

* Yük Dengeleyici VM ilk kez önyükleme pozitif bir araştırma alır.
* (Daha önce açıklanan) numara SuccessFailCount rol örneği sağlıklı olarak işaretlemek için gerekli başarılı araştırmalar değerini tanımlar. Rol örneği kaldırılmışsa, başarılı, art arda araştırmalar sayısı eşit veya rol örneği çalışıyor olarak işaretlemek için SuccessFailCount değerini aşıyor.

> [!NOTE]
> Rol örneği durumunu dalgalı, yük dengeleyici artık rol örneği sağlam durumda geçirmeden önce bekler. Bu, kullanıcı ve altyapıyı koruma ilke aracılığıyla gerçekleştirilir.

## <a name="use-log-analytics-for-load-balancer"></a>Yük Dengeleyici için günlük analizi kullanın

Kullanabileceğiniz [analytics yük dengeleyici için oturum](load-balancer-monitor-log.md) araştırma sistem durumunu ve araştırma sayısı denetlemek için. Günlüğe kaydetme, Power BI veya Azure Operational Insights ile yük dengeleyici sistem durumu hakkındaki istatistiklerdir sağlamak için kullanılabilir.
