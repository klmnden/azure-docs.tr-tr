---
title: Sistem durumunu izlemek için yük dengeleyici özel araştırmalar kullanın | Microsoft Docs
description: Yük dengeleyicinin arkasında izlemek için Azure Load Balancer için özel araştırmalar'ı kullanmayı öğrenin
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
ms.date: 07/13/2018
ms.author: kumud
ms.openlocfilehash: 69991a0b805b5502fc96fab4ce902b3d8bc77baf
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39056367"
---
# <a name="understand-load-balancer-probes"></a>Yük Dengeleyici araştırmalarını anlama

Azure Load Balancer, hangi arka uç havuzu örneğine yeni akışlar alması gereken belirlemek için sistem durumu araştırmaları kullanır.   Arka uç örnek bir uygulama hatasını algılamak için sistem durumu araştırmaları kullanabilirsiniz.  Uygulamanızın sistem durumu araştırma yanıtı, yük dengeleyici için yeni akışlar göndereceğini ya da yeni akışlar yük ya da planlanan kapalı kalma süresi yönetmek için bir arka uç örneğe gönderilmesini durdurmaya devam edilip edilmeyeceğini sinyal için de kullanabilirsiniz.

Yeni akışlar sağlam bir arka uç örneklerine oluşturulmuş olup olmadığını sistem durumu araştırmaları yönetir. Durum araştırması başarısız olduğunda, yük dengeleyici sağlıksız ilgili örneğine yeni akışlar gönderme durdurur.  Yerleşik TCP bağlantıları sistem durumu araştırma hatasından sonra devam edin.  Mevcut UDP akışları taşınır arka uç havuzundaki başka bir örneğine sağlıksız örneğinden.

Arka uç havuzu için tüm araştırmaları başarısız olursa, standart yük dengeleyici devam etmek için yerleşik TCP akışları izin verir ancak temel yük dengeleyici arka uç havuzu için tüm mevcut TCP akışları sona erer; Yeni akış arka uç havuzuna gönderilir.

Bulut hizmeti rolleri (çalışan rolleri ve web rolleri), Konuk Aracısı izleme yoklaması için kullanın. Yük dengeleyicinin arkasına Iaas Vm'leri ile bulut hizmetlerini kullandığınızda, TCP veya HTTP özel sistem durumu araştırmaları yapılandırılmalıdır.

## <a name="understand-probe-count-and-timeout"></a>Yoklama sayısı ve zaman aşımı anlayın

Araştırma davranışı bağlıdır:

* Etiketlenecek bir örnek izin başarılı bir araştırmalarla sayısı kadar yukarı.
* Bir örneği olarak etiketlenmiş neden başarısız yoklama sayısını kapalı olarak.

SuccessFailCount içinde ayarlanan zaman aşımı ve sıklık değerleri örneğini çalıştıran ya da çalışmıyor onaylanıp onaylanmadığını belirler. Azure portalında, zaman aşımı sıklığı değerini iki kez ayarlanır.

Tüm örneklerin Yük Dengelemesi yapılmış bir uç noktası (diğer bir deyişle, yük dengeli kümesi) için araştırma yapılandırması aynı olması gerekir. Belirli bir uç noktası birleşimi için aynı barındırılan hizmetindeki her bir rol örneği veya VM için bir farklı bir araştırma yapılandırması sahip olamaz. Örneğin, her örneği aynı yerel bağlantı noktaları ve zaman aşımları olmalıdır.

> [!IMPORTANT]
> Bir yük dengeleyici araştırması 168.63.129.16 IP adresini kullanır. Bu genel IP adresi, iç platform kaynaklara Getir your-kendi-IP Azure sanal ağ senaryosu için iletişimi kolaylaştırır. Tüm bölgelerde genel sanal IP adresi 168.63.129.16 kullanılır ve bunu değiştirmez. Tüm yerel güvenlik duvarı ilkeleri bu IP adreslerine izin verecek öneririz. Yalnızca iç Azure platformu bu adresi bir iletiden kaynağı bir güvenlik riski düşünülmemelidir. Bu IP adresini güvenlik duvarı ilkelerinizi izin verme, beklenmeyen davranış çeşitli senaryoları oluşur. Çoğaltma IP adresleri ve 168.63.129.16 aynı IP adresi aralığını yapılandırma davranışı içerir.

## <a name="learn-about-the-types-of-probes"></a>Araştırmalar türleri hakkında bilgi edinin

### <a name="guest-agent-probe"></a>Konuk aracı araştırması

Konuk aracı araştırması, yalnızca Azure bulut Hizmetleri için kullanılabilir. Konuk Aracısı VM içindeki yük dengeleyici kullanır. Dinler ve hazır durumda yalnızca örnektir ile bir HTTP 200 OK yanıtı yanıt verdiği. (Geri dönüştürme veya durduruluyor meşgul diğer durumlar vardır.)

Daha fazla bilgi için [Hizmet tanım dosyası (csdef) yapılandırmak için sistem durumu araştırmaları](https://msdn.microsoft.com/library/azure/ee758710.aspx) veya [bulut Hizmetleri için genel yük dengeleyici oluşturmaya başlama](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services).

### <a name="what-makes-a-guest-agent-probe-mark-an-instance-as-unhealthy"></a>Bir örnek sağlıksız olarak işaretlemek bir konuk aracı araştırması yapan nedir?

Konuk Aracısı HTTP 200 OK ile yanıt vermezse, yük dengeleyici örnek yanıt vermiyor olarak işaretler. Ardından, bu örneğe trafik gönderen durdurur. Yük Dengeleyici örneği ping işlemi devam ediyor. Yük dengeleyicinin trafiği bu örneğe Konuk Aracısı bir HTTP 200 yanıt verirse, yeniden gönderir.

Bir web rolü kullandığınızda, Web sitesi kodu genellikle Azure tarafından izlenen değil w3wp.exe çalışan yapı veya konuk Aracısı. W3wp.exe (örneğin, HTTP 500 yanıt) hataları Konuk Aracısı ile bildirilen değildir. Sonuç olarak, yük dengeleyici rotasyon dışında bu örneğe almaz.

### <a name="http-custom-probe"></a>HTTP özel araştırma

HTTP özel araştırma varsayılan Konuk aracı araştırması geçersiz kılar. Rol örneğinin durumunu belirlemek için kendi özel bir mantık oluşturabilirsiniz. Yük Dengeleyici uç noktanızı varsayılan olarak her 15 saniyede araştırmaları. Örnek zaman aşımı süresi içinde bir HTTP 200 yanıt verirse yük dengeleyici rotasyonuna olarak kabul edilir. Varsayılan zaman aşımı süresi 31 saniyedir.

Özel bir HTTP araştırması örneklerini yük dengeleyici rotasyonuna kaldırmak için kendi mantığını uygulamak istiyorsanız yararlı olabilir. Örneğin, 200 durum döndürür ve % 90 CPU üzerinde olduğu bir örneğini kaldırma karar verebilirsiniz. W3wp.exe kullanan web rolleri varsa, aynı zamanda otomatik alırsınız, Web sitesini izleme. Web sitesi kodunuzdaki hataları, yük dengeleyici araştırması için 200 durumu döndürür.

> [!NOTE]
> HTTP özel araştırma ve göreli yollar yalnızca HTTP protokolünü destekler. HTTPS desteklenmiyor.

### <a name="what-makes-an-http-custom-probe-mark-an-instance-as-unhealthy"></a>Bir örnek sağlıksız olarak işaretlemek bir HTTP özel araştırma yapan nedir?

* HTTP uygulama 200 (örneğin, 403, 404 veya 500) dışında bir HTTP yanıt kodunu döndürür. Pozitif bu bildirim, Uygulama örneğinin hizmet dışına hemen almak için sizi uyarır.
* HTTP sunucu zaman aşımı süresinden sonra hiç yanıt vermiyor. Araştırma çalışmıyor olarak işaretleneceğini önce ayarlanan zaman aşımı değeri bağlı olarak birden fazla araştırma isteklerine yanıtlanmamış geçebilir (diğer bir deyişle, önce SuccessFailCount araştırmaları gönderilir).
* Sunucu TCP sıfırlama yoluyla bağlantıyı kapatır.

### <a name="tcp-custom-probe"></a>TCP özel araştırma

TCP özel araştırmalar, tanımlı bir bağlantı üç yönlü sıkışmaya gerçekleştirerek bir bağlantıyı başlatırsınız.

### <a name="what-makes-a-tcp-custom-probe-mark-an-instance-as-unhealthy"></a>Bir örnek sağlıksız olarak işaretlemek bir TCP özel araştırma yapan nedir?

* Sunucu TCP zaman aşımı süresinden sonra hiç yanıt vermiyor. Ne zaman araştırma çalışmıyor olarak işaretlenmiş yanıtlanmamış gitmek araştırma çalışmıyor olarak işaretlemek için yapılandırılmış başarısız istek sayısı bağlıdır.
* Rol örneğinden sıfırlama bir TCP araştırması alır.

Bir HTTP durum araştırması veya bir TCP araştırması nasıl yapılandırılacağı hakkında daha fazla bilgi için bkz. [PowerShell kullanarak Resource Manager'da herkese açık yük dengeleyici oluşturmaya başlama](load-balancer-get-started-internet-arm-ps.md).

## <a name="add-healthy-instances-back-into-the-load-balancer-rotation"></a>Yük Dengeleyici rotasyonuna uygulamasına geri sağlıklı örnek ekleyin

TCP ve HTTP araştırmaları sağlıklı olarak kabul edilir ve rol örneği iyi durumda olduğunda olarak işaretle:

* Yük Dengeleyici, VM ilk kez önyükleme pozitif bir araştırma alır.
* (Daha önce açıklanan) SuccessFailCount için rol örneği sağlıklı olarak işaretlemek için gerekli başarılı bir araştırmalarla değerini tanımlar. Bir rol örneği kaldırıldıysa, başarılı, art arda gelen yoklama sayısını eşit veya rol örneği çalışıyor olarak işaretlemek için SuccessFailCount değerini aşıyor.

> [!NOTE]
> Bir rol örneğinin durumunu dalgalanma, iyi durumda rol örneği getirmeden önce yük dengeleyici artık bekler. Bu ek bekleme süresi, kullanıcı ve altyapı korur ve kasıtlı bir ilkedir.

## <a name="use-log-analytics-for-a-load-balancer"></a>Load balancer için log Analytics'i kullanma

Kullanabileceğiniz [günlük analizi](load-balancer-monitor-log.md) sayısı araştırma ve yük dengeleyici araştırması durumunu denetleyin. Günlüğe kaydetme, Power BI veya Azure operasyonel İçgörüler ile yük dengeleyici sistem durumu hakkındaki istatistiklerdir sağlamak için kullanılabilir.
