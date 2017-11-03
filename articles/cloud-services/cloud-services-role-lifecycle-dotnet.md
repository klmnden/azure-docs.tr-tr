---
title: "Bulut hizmeti yaşam döngüsü olayları işlemek | Microsoft Docs"
description: "Bir bulut hizmeti rolü yaşam döngüsü yöntemlerinin .NET içinde nasıl kullanılacağını öğrenin"
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 39b30acd-57b9-48b7-a7c4-40ea3430e451
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: eb78c05df3b3cdf3887334c11bdabd5cebb74747
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="customize-the-lifecycle-of-a-web-or-worker-role-in-net"></a>.NET’te Web veya Çalışan rolünün yaşam döngüsünü özelleştirme
Çalışan rolü oluşturduğunuzda, genişlettiğiniz [RoleEntryPoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx) olanak sağlayan yöntemleri geçersiz kılmanıza olanak sağlayan sınıf yaşam döngüsü olaylarına yanıt. Yaşam döngüsü olaylara yanıt vermek kullanmalısınız web rolleri için bu sınıf isteğe bağlıdır.

## <a name="extend-the-roleentrypoint-class"></a>RoleEntryPoint sınıfını genişletir
[RoleEntryPoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx) sınıfı, Azure tarafından çağrılan yöntemler içerir, bunu **başlatır**, **çalıştıran**, veya **durdurur** web veya çalışan rolü. İsteğe bağlı olarak rol başlatma, rol kapatma dizileri veya rolünün yürütme iş parçacığı yönetmek için bu yöntemleri geçersiz kılabilirsiniz. 

Genişletirken **RoleEntryPoint**, yöntemler aşağıdaki davranışlarını bilincinde olmanız gerekir:

* [OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) ve [OnStop](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) yöntemleri döndürmeyi mümkün olması için bir Boole değeri döndürür **false** bu yöntemlerden.
  
   Kodunuzu döndürürse **false**rol işlem beklenmedik şekilde sona erdirildi, tüm kapatma dizisi çalıştırmadan yerinde olabilir. Genel olarak, döndürme kaçınmalısınız **false** gelen **OnStart** yöntemi.
* Herhangi bir aşırı yüklemesini içinde özel durum yakalanmamış bir **RoleEntryPoint** yöntemi, işlenmeyen bir özel durum değerlendirilir.
  
   Yaşam döngüsü yöntemlerden birini içinde bir özel durum oluşursa, Azure oluşturacağı [UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx) olay ve sonra işlemi sonlandırılır. Rolünüze çevrimdışı alındıktan sonra Azure tarafından başlatılır. İşlenmeyen bir özel durum oluştuğunda [durdurma](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.stopping.aspx) olay başlatılmaz ve **OnStop** yöntemi çağrılmaz.

Rolünüze başlamıyor ya da, meşgul, başlatma ve durdurma durumları arasında geri dönüştürme, kodunuzun her rol yeniden başlatıldığında yaşam döngüsü olayları biri içinde işlenmeyen bir özel durum atma. Bu durumda, kullanarak [UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx) özel durumun nedeni belirlemek ve uygun şekilde işlemek için olay. Rolünüze de gelen döndürüyor olabilir [çalıştırmak](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) yeniden başlatmak rol neden yöntemi. Dağıtım durumları hakkında daha fazla bilgi için bkz: [ortak sorunları neden rolleri için Geri Dönüşüm](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md).

> [!NOTE]
> Kullanıyorsanız **Azure Araçları için Microsoft Visual Studio** uygulamanızı geliştirmek için rol proje şablonlarını otomatik olarak genişletmek **RoleEntryPoint** sınıfı, içinde  *WebRole.cs* ve *WorkerRole.cs* dosyaları.
> 
> 

## <a name="onstart-method"></a>OnStart yöntemi
**OnStart** rol örneğinizi Azure tarafından çevrimiçi duruma getirildiğinde yöntemi çağrılır. OnStart kod yürütülürken rol örneği olarak işaretlenmiş **meşgul** ve dış trafiği için yük dengeleyici tarafından yönlendirilir. Olay işleyicileri uygulama ve başlatma gibi başlatma işini yapmak için bu yöntemi geçersiz kılabilirsiniz [Azure tanılama](cloud-services-how-to-monitor.md).

Varsa **OnStart** döndürür **true**örneği başarıyla başlatıldı ve Azure çağırır **RoleEntryPoint.Run** yöntemi. Varsa **OnStart** döndürür **yanlış**, herhangi bir planlı kapatma dizisini çalıştırmadan rolü derhal sona erer.

Aşağıdaki kod örneğinde nasıl geçersiz kılınacağını gösterir **OnStart** yöntemi. Bu yöntem yapılandırır ve rol örneği başlatır ve günlük veri aktarımını depolama hesabı ayarlar tanı İzleyicisi başlatılır:

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
**OnStop** yöntemi, bir rol örneği Azure tarafından ve çıkılıyor önce çevrimdışı alındıktan sonra çağrılır. Düzgün bir şekilde kapatmak, rol örneği için gerekli kod çağırmak için bu yöntemin üzerine yazabilir.

> [!IMPORTANT]
> Çalışan kod **OnStop** yöntemi sahip bir kullanıcı tarafından başlatılan kapatma dışında nedenlerle çağrıldığında tamamlamak için sınırlı bir süre. Bu süre geçtikten sonra bu kodu emin olmanız gerekir böylece işlemin, sonlandırıldığından **OnStop** yöntemi tamamlanıncaya kadar çalıştırmayan göstereceği veya hızlı bir şekilde çalıştırabilirsiniz. **OnStop** yöntemi çağrıldıktan sonra **durdurma** olayı oluşturulur.
> 
> 

## <a name="run-method"></a>Run yöntemi
Geçersiz kılabilirsiniz **çalıştırmak** rol örneği için uzun süre çalışan iş parçacığı uygulamak için yöntem.

Geçersiz kılma **çalıştırmak** yöntemi gerekli değildir; sonsuza kadar uyku bir iş parçacığı varsayılan uygulamasını başlatır. Geçersiz kılarsanız **çalıştırmak** yöntemi, kodunuzu engelleme süresiz olarak. Varsa **çalıştırmak** yöntemi döndürür, rolü otomatik olarak düzgün bir şekilde geri dönüştürülmeden; diğer bir deyişle, Azure başlatır **durdurma** olay ve çağrıları **OnStop** yöntemi için rolü çevrimdışı duruma önce kapatma sıraları yürütülen.

### <a name="implementing-the-aspnet-lifecycle-methods-for-a-web-role"></a>Web rolü için ASP.NET yaşam döngüsü yöntemleri uygulama
Tarafından sağlanan ek olarak ASP.NET yaşam döngüsü yöntemlerini kullanabilirsiniz **RoleEntryPoint** web rolü için başlatma ve kapatma sıralarını yönetmek için sınıf. Mevcut bir ASP.NET uygulamasını Azure için bağlantı noktası oluşturma, bu uyumluluk amacıyla yararlı olabilir. ASP.NET yaşam döngüsü yöntemleri içinden adlı **RoleEntryPoint** yöntemleri. **Uygulama\_Başlat** yöntemi çağrıldıktan sonra **RoleEntryPoint.OnStart** yöntemini çağırır. **Uygulama\_son** yöntemi çağrılmadan önce **RoleEntryPoint.OnStop** yöntemi çağrılır.

## <a name="next-steps"></a>Sonraki adımlar
Bilgi edinmek için nasıl [bir bulut hizmet paketi oluşturma](cloud-services-model-and-package.md).

