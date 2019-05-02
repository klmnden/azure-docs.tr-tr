---
title: Azure Active Directory B2C özel ilkelerinde sorunlarını gidermek için application Insights | Microsoft Docs
description: Kurulum özel ilkeler yürütülmesini izlemek için Application ınsights'ı öğrenin.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/04/2017
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 9b25e5dc5d090ad7aab3d61e2c303a465b5d7443
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64703927"
---
# <a name="azure-active-directory-b2c-collecting-logs"></a>Azure Active Directory B2C: Günlükleri toplama

Bu makalede, böylece özel ilkeleriniz ile sorunları tanılamak günlükleri Azure AD B2C'den toplanması için adımları sağlar.

>[!NOTE]
>Burada açıklanan ayrıntılı etkinlik günlükleri şu anda tasarlanmıştır **yalnızca** özel ilkeler geliştirmede yardımcı olacak. Geliştirme modunda, üretim ortamında kullanmayın.  Geliştirme sırasında gönderilen ve kimlik sağlayıcılardan gelen tüm talepler günlükleri toplayın.  Üretim ortamında kullandıysanız, PII (özel olarak tanımlanabilir bilgiler) sahip oldukları App Insights günlüğünde toplanan sorumluluğunu Geliştirici varsayar.  Şirket ilkesi yerleştirildiğinde bu ayrıntılı günlükleri yalnızca toplanır **geliştirme modu**.


## <a name="use-application-insights"></a>Application Insights kullanın

Azure AD B2C'yi, Application Insights'a veri göndermek için bir özelliği destekler.  Application Insights özel durumları tanılama ve uygulama performası sorunlarını görselleştirmek için bir yol sağlar.

### <a name="setup-application-insights"></a>Application Insights Kurulumu

1. [Azure Portal](https://portal.azure.com) gidin. Kiracıda (Azure AD B2C kiracınızı değil) Azure aboneliğinizle olduğundan emin olun.
1. Tıklayın **+ yeni** sol taraftaki gezinti menüsünde.
1. Arayın ve seçin **Application Insights**, ardından **Oluştur**.
1. Formu tamamlayıp tıklayın **Oluştur**. Seçin **genel** için **uygulama türü**.
1. Kaynak oluşturulduktan sonra Application Insights kaynağını açın.
1. Bulma **özellikleri** sol menü ve onu tıklayın.
1. Kopyalama **izleme anahtarını** ve için sonraki bölüme kaydedin.

### <a name="set-up-the-custom-policy"></a>Özel İlkesi ayarlama

1. RP dosyasını (örneğin, SignUpOrSignin.xml) açın.
1. Aşağıdaki öznitelikleri eklemek `<TrustFrameworkPolicy>` öğesi:

   ```XML
   DeploymentMode="Development"
   UserJourneyRecorderEndpoint="urn:journeyrecorder:applicationinsights"
   ```

1. Zaten yoksa, bir alt düğüm ekleyin `<UserJourneyBehaviors>` için `<RelyingParty>` düğümü. Hemen sonra yer almalıdır `<DefaultUserJourney ReferenceId="UserJourney Id from your extensions policy, or equivalent (for example:SignUpOrSigninWithAAD" />`
2. Bir alt öğesi olarak aşağıdaki düğüm ekleme `<UserJourneyBehaviors>` öğesi. Değiştirdiğinizden emin olun `{Your Application Insights Key}` ile **izleme anahtarını** Application ınsights'ı önceki bölümde edindiğiniz.

   ```XML
   <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="{Your Application Insights Key}" DeveloperMode="true" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
   ```

   * `DeveloperMode="true"` en yüksek miktarlarda kısıtlanmış ancak geliştirme için telemetri işleme ardışık düzeninden iyi hızlandırmak için Applicationınsights söyler.
   * `ClientEnabled="true"` Sayfa görünümü ve istemci tarafı hataları (gerekli) izleme Applicationınsights istemci tarafı komut gönderir.
   * `ServerEnabled="true"` Mevcut UserJourneyRecorder JSON özel olay Application Insights'a gönderir.
   Örnek:

   ```XML
   <TrustFrameworkPolicy
    ...
    TenantId="fabrikamb2c.onmicrosoft.com"
    PolicyId="SignUpOrSignInWithAAD"
    DeploymentMode="Development"
    UserJourneyRecorderEndpoint="urn:journeyrecorder:applicationinsights"
   >
    ...
    <RelyingParty>
      <DefaultUserJourney ReferenceId="UserJourney ID from your extensions policy, or equivalent (for example: SignUpOrSigninWithAzureAD)" />
      <UserJourneyBehaviors>
        <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="{Your Application Insights Key}" DeveloperMode="true" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
      </UserJourneyBehaviors>
      ...
   </TrustFrameworkPolicy>
   ```

3. İlke karşıya yükleyin.

### <a name="see-the-logs-in-application-insights"></a>Application ınsights günlüklerine bakın

>[!NOTE]
> Yeni Application Insights günlüklerine görebilmek için önce kısa bir gecikme (beş dakikadan daha kısa) yoktur.

1. Oluşturduğunuz Application Insights kaynağını açma [Azure portalında](https://portal.azure.com).
1. İçinde **genel bakış** menüsünde tıklatın **Analytics**.
1. Application Insights'ta yeni bir sekme açın.
1. İşte bir listesi sorguların günlükleri görmek için kullanabilirsiniz

| Sorgu | Açıklama |
|---------------------|--------------------|
izlemeleri | Tüm Azure AD B2C tarafından oluşturulan günlükler |
izlemeleri \| nerede zaman damgası > ago(1d) | Tüm son günü için Azure AD B2C tarafından oluşturulan günlükler

Girişleri uzun olabilir.  CSV'ye dışarı aktarmak için daha yakından bakın.

Analiz aracı hakkında daha fazla bilgi [burada](https://docs.microsoft.com/azure/application-insights/app-insights-analytics).

>[!NOTE]
>Topluluk kimlik geliştiricilerin yardımcı olmak için bir kullanıcı yolculuğu Görüntüleyici geliştirmiştir.  Bu değil Microsoft tarafından desteklenen ve kesin olarak kullanıma sunulan-olduğu.  Application Insights örneğinizin okur ve yolculuğu olayları kullanıcı iyi yapılandırılmış bir görünümünü sağlar.  Kaynak kodu edinmenizi ve kendi çözümünüzü dağıtabilirsiniz.

Uygulama anlayışları'ndan olayları okur Görüntüleyicisi sürümünü bulunduğu [burada](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/wingtipgamesb2c/src/WingTipUserJourneyPlayerWebApplication)

>[!NOTE]
>Burada açıklanan ayrıntılı etkinlik günlükleri şu anda tasarlanmıştır **yalnızca** özel ilkeler geliştirmede yardımcı olacak. Geliştirme modunda, üretim ortamında kullanmayın.  Geliştirme sırasında gönderilen ve kimlik sağlayıcılardan gelen tüm talepler günlükleri toplayın.  Üretim ortamında kullandıysanız, PII (özel olarak tanımlanabilir bilgiler) sahip oldukları App Insights günlüğünde toplanan sorumluluğunu Geliştirici varsayar.  Şirket ilkesi yerleştirildiğinde bu ayrıntılı günlükleri yalnızca toplanır **geliştirme modu**.

[Desteklenmeyen özel ilkesi örnekleri ve ilgili araçlar için GitHub deposu](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies)



## <a name="next-steps"></a>Sonraki Adımlar

Application Insights'ı nasıl kendi kimlik sunmak için temel alınan B2C çalışır kimlik deneyimi çerçevesi deneyimlerini anlamanıza yardımcı olması için verileri araştırın.
