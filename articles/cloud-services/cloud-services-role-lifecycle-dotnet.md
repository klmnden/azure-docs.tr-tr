---
title: Bulut hizmeti yaşam döngüsü olaylarını işleme | Microsoft Docs
description: . NET'te bir bulut Hizmeti rol yaşam döngüsü yöntemlerinin nasıl kullanılabileceğini öğrenin
services: cloud-services
documentationcenter: .net
author: jpconnock
manager: timlt
editor: ''
ms.assetid: 39b30acd-57b9-48b7-a7c4-40ea3430e451
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: jeconnoc
ms.openlocfilehash: 13f500b32bb85bdc0f84b812ef4ef9188a257771
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59798032"
---
# <a name="customize-the-lifecycle-of-a-web-or-worker-role-in-net"></a>.NET’te Web veya Çalışan rolünün yaşam döngüsünü özelleştirme
Bir çalışan rolü oluşturduğunuzda, genişletilen [RoleEntryPoint](/previous-versions/azure/reference/ee758619(v=azure.100)) , geçersiz kılmak olanak tanıyan yöntemler sağlar sınıfını yaşam döngüsü olaylarını yanıtlama. Web rolleri için bu sınıf, isteğe bağlı yaşam döngüsü olaylarını yanıtlamak için kullanmalısınız.

## <a name="extend-the-roleentrypoint-class"></a>RoleEntryPoint sınıfını genişletir
[RoleEntryPoint](/previous-versions/azure/reference/ee758619(v=azure.100)) sınıfı, Azure tarafından çağrılan yöntemler içerir, bu **başlar**, **çalıştıran**, veya **durdurur** bir web veya çalışan rolü. İsteğe bağlı olarak rol başlatma, yürütme iş parçacığını rolün veya rol kapatma dizileriyle yönetmek için bu yöntemleri geçersiz kılabilirsiniz. 

Genişletirken **RoleEntryPoint**, aşağıdaki yöntemlerden birini davranışları farkında olmalıdır:

* [OnStart](/previous-versions/azure/reference/ee772851(v=azure.100)) ve [OnStop](/previous-versions/azure/reference/ee772844(v=azure.100)) yöntemleri dönmek mümkündür bir Boole değeri döndürür **false** bu yöntemlerden.
  
   Kodunuzu döndürürse **false**rolü işlem beklenmedik şekilde sonlandırıldı, hiçbir kapatma dizisi çalıştırmadan yerinde olabilir. Genel olarak, geri dönmekten kaçının **false** gelen **OnStart** yöntemi.
* Tüm Yakalanmayan Özel durum bir aşırı yüklemesini içinde bir **RoleEntryPoint** yöntemi işlenmeyen bir özel durum kabul edilir.
  
   Yaşam döngüsü yöntemlerinden biri içinde bir özel durum oluşursa, Azure oluşturacağı [UnhandledException](/dotnet/api/system.appdomain.unhandledexception) olay ve işlem sonlandırılır. Rolünüz çevrimdışı alındıktan sonra Azure tarafından başlatılır. İşlenmeyen özel durum oluştuğunda [durdurma](/previous-versions/azure/reference/ee758136(v=azure.100)) değil olayı oluşturulur ve **OnStop** yöntemi çağrılmadı.

Rolünüz başlamıyor veya başlatılıyor, meşgul ve durdurma durumları arasında geri dönüştürme, kodunuzu rolü her zaman yeniden yaşam döngüsü olaylarını biri içinde işlenmeyen bir özel durum atma. Bu durumda [UnhandledException](/dotnet/api/system.appdomain.unhandledexception) özel durumun nedenini belirlemek ve uygun şekilde işlemesi için olay. Rolünüz de gelen döndürüyor olabilir [çalıştırma](/previous-versions/azure/reference/ee772746(v=azure.100)) yöntemi rolün yeniden neden olur. Dağıtım durumları hakkında daha fazla bilgi için bkz: [yaygın sorunlar, neden rollerine geri dönüşüm](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md).

> [!NOTE]
> Kullanıyorsanız **Microsoft Visual Studio için Azure Araçları** genişletmek otomatik olarak rolü proje şablonları, uygulama geliştirmek için **RoleEntryPoint** sınıfı, içinde  *WebRole.cs* ve *WorkerRole.cs* dosyaları.
> 
> 

## <a name="onstart-method"></a>OnStart yöntemi
**OnStart** rol örneğiniz tarafından Azure çevrimiçi duruma getirildiğinde yöntemi çağrılır. OnStart kod yürütülürken, rol örneği olarak işaretlenmiş **meşgul** ve dış trafiği için yük dengeleyici tarafından yönlendirilir. Olay işleyicilerini uygulayan ve başlatma gibi başlatma işi yapmak için bu yöntemi geçersiz kılabilirsiniz [Azure tanılama](cloud-services-how-to-monitor.md).

Varsa **OnStart** döndürür **true**örneği başarıyla başlatıldı ve Azure'ı çağırır **RoleEntryPoint.Run** yöntemi. Varsa **OnStart** döndürür **false**, rol herhangi planlanmış kapatma Dizileri çalıştırmadan hemen sonlandırır.

Aşağıdaki kod örneğinde nasıl geçersiz kılınacağını gösterir **OnStart** yöntemi. Bu yöntem, yapılandırır ve rol örneği başlatır ve bir günlük veri aktarımı bir depolama hesabına ayarlar tanı İzleyicisi başlatılır:

```csharp
public override bool OnStart()
{
    var config = DiagnosticMonitor.GetDefaultInitialConfiguration();

    config.DiagnosticInfrastructureLogs.ScheduledTransferLogLevelFilter = LogLevel.Error;
    config.DiagnosticInfrastructureLogs.ScheduledTransferPeriod = TimeSpan.FromMinutes(5);

    DiagnosticMonitor.Start("DiagnosticsConnectionString", config);

    return true;
}
```

## <a name="onstop-method"></a>OnStop yöntemi
**OnStop** yöntemi, bir rol örneği işlemi önce ve Azure tarafından çevrimdışı alındıktan sonra çağrılır. Düzgün bir şekilde kapatmak, rol örneği için gereken kodu çağırmak için bu yöntemi geçersiz kılabilirsiniz.

> [!IMPORTANT]
> Çalışan kod **OnStop** yöntemi nedeniyle bir kullanıcı tarafından başlatılan kapatma dışında çağrıldığında tamamlamak için sınırlı bir süre vardır. Bu süre geçtikten sonra bu kodu emin olmanız gerekir, böylece işlem, sonlandırılana **OnStop** yöntemi tamamlanana kadar çalışmıyor göstereceği veya hızlı bir şekilde çalıştırabilirsiniz. **OnStop** yöntemi çağrıldıktan sonra **durdurma** olayı oluşturulur.
> 
> 

## <a name="run-method"></a>Run yöntemi
Geçersiz kılabilirsiniz **çalıştırma** rol Örneğiniz için bir uzun süre çalışan iş parçacığı uygulamak için yöntemi.

Geçersiz kılma **çalıştırma** yöntemi gerekli değildir; varsayılan uygulama uyku sonsuza kadar bir iş parçacığı başlatılır. Geçersiz kılarsanız **çalıştırma** yöntemi, kodunuzu block süresiz olarak. Varsa **çalıştırın** yöntemi döndürür, rolü otomatik olarak düzgün bir şekilde geri dönüştürülmeden; diğer bir deyişle, Azure'ı başlatır **durduruluyor** olay ve çağrıları **OnStop** yöntemi için Rol çevrimdışına önce kapatma dizileri çalıştırılabilir.

### <a name="implementing-the-aspnet-lifecycle-methods-for-a-web-role"></a>Bir web rolü için ASP.NET yaşam döngüsü yöntem uygulama
ASP.NET yaşam döngüsü yöntemleri tarafından sağlanan ek olarak kullanabileceğiniz **RoleEntryPoint** sınıfı, bir web rolü için başlatma ve kapatma dizilerini yönetmek için. Mevcut bir ASP.NET uygulamasını azure'a bağlantı noktası oluşturma, bu uyumluluk amacıyla yararlı olabilir. ASP.NET yaşam döngüsü yöntemleri içinden adlı **RoleEntryPoint** yöntemleri. **Uygulama\_Başlat** yöntemi çağrıldıktan sonra **RoleEntryPoint.OnStart** yöntemi biter. **Uygulama\_son** önce yöntemi çağrıldığında **RoleEntryPoint.OnStop** yöntemi çağrılır.

## <a name="next-steps"></a>Sonraki adımlar
Bilgi edinmek için nasıl [bir bulut hizmeti paketi oluşturma](cloud-services-model-and-package.md).

