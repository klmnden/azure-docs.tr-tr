---
title: Sistem durumunu izlemek için yük dengeleyici özel araştırmalara kullanın | Microsoft Docs
description: Yük dengeleyicinin arkasındaki örnekleri izlemek için Azure yük dengeleyici için özel araştırmalara kullanmayı öğrenin
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/8/2018
ms.author: kumud
ms.openlocfilehash: 0aab72fdf48589a72707ae87f90af11f65f35088
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="understand-load-balancer-probes"></a>Yük Dengeleyici araştırmalar anlama

Azure yük dengeleyici sistem durumu araştırmalarının hangi arka uç havuzu örneğine yeni akışları alması gereken belirlemek için kullanır. Bir sistem durumu araştırması başarısız olduğunda, yük dengeleyici yeni akışlar ilgili sağlıksız örneğine gönderme durdurur ve bu örneğinde mevcut akışları etkilenmez.  Aşağı tüm arka uç havuzu örnekleri araştırma, var olan tüm akışlar arka uç havuzundaki tüm örneklerinde zaman aşımına uğrar.

Bulut hizmeti rollerinizi (çalışan rolleri ve web rolleri) bir konuk Aracısı araştırma izlemek için kullanın. Yük dengeleyicinin arkasındaki VM'ler kullandığınızda, TCP veya HTTP özel durumu araştırmalarının yapılandırılması gerekir.

## <a name="understand-probe-count-and-timeout"></a>Araştırma sayısı ve zaman aşımı anlama

Araştırma davranışı bağlıdır:

* Bir örnek olarak etiketlenmiş izin başarılı araştırmalar sayısı olarak çalışır.
* Bir örnek olarak etiketlenmiş neden başarısız araştırmalar sayısı olarak aşağı.

İçinde SuccessFailCount ayarladığınız zaman aşımı ve sıklığı değerlerini örneğini çalıştıran veya çalışmıyor onaylanıp onaylanmadığını belirler. Azure portalında zaman aşımı için sıklığı değerini iki kez ayarlanır.

Tüm örneklerin yük dengeli bir uç nokta (diğer bir deyişle, yük dengeli kümesi) için araştırma yapılandırması aynı olmalıdır. Her rol örneği ya da VM için aynı barındırılan hizmet belirli uç noktası bileşimi için bir farklı araştırma yapılandırmaya sahip olamaz. Örneğin, her bir örneği aynı yerel bağlantı noktaları ve zaman aşımlarına olması gerekir.

> [!IMPORTANT]
> Yük Dengeleyici araştırmasını 168.63.129.16 IP adresini kullanır. Bu ortak IP adresini Getir-bilgisayarınızı-kendi-IP Azure sanal ağı senaryosu için iç platform kaynaklarına iletişimi kolaylaştırır. Genel sanal IP adresi 168.63.129.16 tüm bölgelerde kullanılır ve bu değiştirmez. Tüm yerel güvenlik duvarı ilkeleri bu IP adresine izin öneririz. Yalnızca dahili Azure platformu bu adresi bir iletiden kaynağı bir güvenlik riski düşünülmemelidir. Bu IP adresi, güvenlik duvarı ilkelerinizi izin verme, çeşitli senaryolarda beklenmeyen davranış oluşur. Çoğaltma IP adresleri ve 168.63.129.16 aynı IP adresi aralığını yapılandırma davranışı içerir.

## <a name="learn-about-the-types-of-probes"></a>Araştırmalar türleri hakkında bilgi edinin

### <a name="guest-agent-probe"></a>Konuk aracı araştırması

Konuk Aracısı araştırma yalnızca Azure bulut Hizmetleri için kullanılabilir. Yük Dengeleyici VM Konuk Aracısı'nı kullanır. Dinler ve yalnızca örnek hazır durumda olduğunda bir HTTP 200 Tamam yanıt ile yanıt verir. (Diğer geri dönüştürme ya da durdurma meşgul durumlarıdır.)

Daha fazla bilgi için bkz: [Hizmet tanım dosyası (csdef) yapılandırmak için sistem durumu araştırmalarının](https://msdn.microsoft.com/library/azure/ee758710.aspx) veya [bulut Hizmetleri için bir genel yük dengeleyiciye oluşturarak başlayın](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services).

### <a name="what-makes-a-guest-agent-probe-mark-an-instance-as-unhealthy"></a>Bir örneği sağlıksız olarak işaretlemek Konuk aracı araştırması ne yapar?

Konuk Aracısı ile HTTP 200 Tamam yanıt vermiyorsa, yük dengeleyici örneği yanıt olarak işaretler. Ardından, trafiği için bu örneği göndermeye durdurur. Yük Dengeleyici örneği ping işlemi devam eder. Yük Dengeleyici trafiği için bu örneği ile bir HTTP 200 Konuk aracısı yanıt verirse, yeniden gönderir.

Web rolü kullandığınızda, Web sitesi kodu genellikle Azure tarafından izlenen değil w3wp.exe çalışan doku veya konuk Aracısı. W3wp.exe (örneğin, HTTP 500 yanıtları) hatalar için konuk Aracısı bildirilen değil. Sonuç olarak, yük dengeleyici döndürme dışında bu örneği almaz.

### <a name="http-custom-probe"></a>HTTP özel araştırma

HTTP özel araştırma varsayılan Konuk aracı araştırması geçersiz kılar. Rol örneğinin sistem durumunu belirlemek için kendi özel mantık oluşturabilirsiniz. Yük Dengeleyici uç noktanızı varsayılan olarak 15 dakikada araştırmaları. Örnek, bir HTTP 200 ile zaman aşımı süresi içinde yanıt verirse yük dengeleyici döndürme olarak kabul edilir. Zaman aşımı süresi, varsayılan olarak 31 saniyedir.

HTTP özel araştırma yük dengeleyici döndürme örneklerini kaldırmak için kendi mantığını uygulamak istediğinizde yararlı olabilir. Örneğin, % 90 CPU ise ve 200 olmayan durum döndürür örneği kaldırmaya karar verebilirsiniz. W3wp.exe kullanan web rolleri varsa, aynı zamanda otomatik alırsınız, Web sitesini izleme. Web sitesi kodunuzdaki hataları için yük dengeleyici araştırmasını dönüş 200 olmayan durumu.

> [!NOTE]
> HTTP özel araştırma göreli yollar ve yalnızca HTTP protokolünü destekler. HTTPS desteklenmiyor.

### <a name="what-makes-an-http-custom-probe-mark-an-instance-as-unhealthy"></a>Bir örneği sağlıksız olarak işaretlemek bir HTTP özel araştırma ne yapar?

* HTTP uygulama 200 (örneğin, 403, 404 veya 500) dışında bir HTTP yanıt kodunu döndürür. Pozitif bu bildirim, uygulama örneği hizmet dışına hemen almak için sizi uyarır.
* HTTP sunucusu zaman aşımı süresinden sonra hiç yanıt vermiyor. Araştırma çalışmıyor olarak işaretlenmiş önce ayarlanır zaman aşımı değeri bağlı olarak, birden çok araştırma isteklerine yanıtlanmadan geçebilir (diğer bir deyişle, önce SuccessFailCount araştırmalar gönderilir).
* Sunucu TCP sıfırlama aracılığıyla bağlantıyı kapatır.

### <a name="tcp-custom-probe"></a>TCP özel araştırma

TCP özel araştırmalara üç yönlü el sıkışma tanımlanan bağlantı noktası ile gerçekleştirerek bir bağlantı başlatır.

### <a name="what-makes-a-tcp-custom-probe-mark-an-instance-as-unhealthy"></a>Bir örneği sağlıksız olarak işaretlemek TCP özel bir araştırma yapan nedir?

* TCP sunucu zaman aşımı süresinden sonra hiç yanıt vermiyor. Ne zaman araştırma çalışmıyor olarak işaretlenmiş sayısı, araştırma çalışmıyor olarak işaretlemek önce yanıtlanmadan gitmek için yapılandırılmış olan başarısız araştırma isteklerinin bağlıdır.
* Rol örneğinden sıfırlama bir TCP araştırması alır.

Bir HTTP durum araştırması veya bir TCP araştırması nasıl yapılandırılacağı hakkında daha fazla bilgi için bkz: [PowerShell kullanarak Kaynak Yöneticisi'nde bir genel yük dengeleyiciye oluşturmaya başlamak](load-balancer-get-started-internet-arm-ps.md).

## <a name="add-healthy-instances-back-into-the-load-balancer-rotation"></a>Yük Dengeleyici döndürme uygulamasına geri sağlıklı örnekleri Ekle

TCP ve HTTP araştırmaları sağlıklı olarak kabul edilir ve rol örneği sağlıklı olduğunda olarak işaretleyin:

* Yük Dengeleyici VM ilk kez önyükleme pozitif bir araştırma alır.
* (Daha önce açıklanan) SuccessFailCount numaralı rol örneği sağlıklı olarak işaretlemek için gerekli başarılı araştırmalar değerini tanımlar. Rol örneği kaldırılmışsa, başarılı, art arda araştırmalar sayısı eşit veya rol örneği çalışıyor olarak işaretlemek için SuccessFailCount değerini aşıyor.

> [!NOTE]
> Rol örneği durumunu dalgalanma, yük dengeleyici rol örneği geri sağlıklı duruma getirmeden önce artık bekler. Bu ek bekleme süresi kullanıcı ve altyapı korur ve kasıtlı bir ilkedir.

## <a name="use-log-analytics-for-a-load-balancer"></a>Bir yük dengeleyici için günlük analizi kullanın

Kullanabileceğiniz [oturum analytics](load-balancer-monitor-log.md) yük dengeleyici araştırması sağlık durumunu denetleyin ve araştırma sayısı. Günlüğe kaydetme, Power BI veya Azure Operational Insights ile yük dengeleyici sistem durumu hakkındaki istatistiklerdir sağlamak için kullanılabilir.
