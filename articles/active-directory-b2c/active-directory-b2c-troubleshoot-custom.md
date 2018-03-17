---
title: "Azure AD B2C özel ilkeleri - gidermek için application Insights | Microsoft Docs"
description: "Kurulum özel ilkeler yürütülmesini izlemek için Application Insights nasıl"
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: mtillman
editor: parakhj
ms.assetid: 658c597e-3787-465e-b377-26aebc94e46d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 08/04/2017
ms.author: saeda
ms.openlocfilehash: 4f71380917a5a29497da9831791cd9f86ec4c8ca
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="azure-active-directory-b2c-collecting-logs"></a>Azure Active Directory B2C: Günlükleri toplama

Bu makalede, böylece özel ilkelerinizi sorunları tanılama günlüklerini Azure AD B2C ' toplanması için adımları sağlar.

>[!NOTE]
>Burada açıklanan ayrıntılı etkinlik günlükleri şu anda tasarlanmış **yalnızca** özel ilkeler geliştirmeye yardımcı olmak için. Geliştirme modunu üretimde kullanmayın.  Geliştirme sırasında gönderilen ve kimlik sağlayıcılardan gelen tüm talepler günlüklerini toplayın.  Üretimde kullandıysanız, geliştirici PII (özel olarak tanımlanabilen bilgiler) sahip oldukları App Insights günlüğünde toplanan sorumluluğunu varsayar.  Bu ayrıntılı günlükler yalnızca ilke yerleştirildiği olduğunda toplanır **geliştirme MODUNU**.


## <a name="use-application-insights"></a>Application Insights kullanın

Azure AD B2C, Application Insights'a veri göndermek için bir özelliği destekler.  Application Insights özel durumlar tanılama ve uygulama performans sorunlarını görselleştirmek için bir yol sağlar.

### <a name="setup-application-insights"></a>Kurulum Application Insights

1. [Azure Portal](https://portal.azure.com) gidin. Kiracısı'nda, Azure aboneliğinizle (Azure AD B2C kiracısına değil) olduğundan emin olun.
1. Tıklatın **+ yeni** sol taraftaki gezinti menüsünde.
1. Aramak ve seçmek **Application Insights**, ardından **oluşturma**.
1. Formu tamamlayıp tıklatın **oluşturma**. Seçin **genel** için **uygulama türü**.
1. Kaynak oluşturulduktan sonra Application Insights kaynağı açın.
1. Bul **özellikleri** sol menü ve onu tıklayın.
1. Kopya **izleme anahtarını** ve için sonraki bölüme kaydedin.

### <a name="set-up-the-custom-policy"></a>Özel ilke ayarla

1. RP dosyasını (örneğin, SignUpOrSignin.xml) açın.
1. Aşağıdaki öznitelikler eklemek `<TrustFrameworkPolicy>` öğe:

  ```XML
  DeploymentMode="Development"
  UserJourneyRecorderEndpoint="urn:journeyrecorder:applicationinsights"
  ```

1. Zaten yoksa, bir alt düğüm eklemek `<UserJourneyBehaviors>` için `<RelyingParty>` düğümü. Hemen sonra bulunmalıdır `<DefaultUserJourney ReferenceId="UserJourney Id from your extensions policy, or equivalent (for example:SignUpOrSigninWithAAD" />`
2. Bir alt öğesi olarak aşağıdaki düğüm eklemek `<UserJourneyBehaviors>` öğesi. Değiştirdiğinizden emin olun `{Your Application Insights Key}` ile **izleme anahtarını** gelen Application Insights önceki bölümde edindiğiniz.

  ```XML
  <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="{Your Application Insights Key}" DeveloperMode="true" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
  ```

  * `DeveloperMode="true"` en yüksek hacim rakamlarına kısıtlanmış ancak geliştirme için işleme ardışık düzeninden telemetri iyi hızlandırmak için Applicationınsights söyler.
  * `ClientEnabled="true"` Sayfa görünümü ve istemci tarafı hataları (gerekli) izlemek için Applicationınsights istemci tarafı komut dosyası gönderir.
  * `ServerEnabled="true"` Varolan UserJourneyRecorder JSON özel bir olay Application Insights'a gönderir.
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

### <a name="see-the-logs-in-application-insights"></a>Application Insights günlüklerine bakın

>[!NOTE]
> Olduğundan yeni günlükler Application ınsights'ta görmek için kısa bir gecikme (beş dakikadan daha az).

1. Oluşturduğunuz Application Insights kaynağını açma [Azure portal](https://portal.azure.com).
1. İçinde **genel bakış** menüsünde tıklatın **Analytics**.
1. Application Insights'ta yeni bir sekme açın.
1. Burada bir listesidir günlükleri görmek için kullanabileceğiniz sorgular

| Sorgu | Açıklama |
|---------------------|--------------------|
İzlemeler | Tüm Azure AD B2C tarafından oluşturulan günlükler bakın |
İzlemeler \| olduğu zaman damgası > ago(1d) | Tüm son gündür Azure AD B2C tarafından oluşturulan günlükler bakın

Girişleri uzun olabilir.  Daha ayrıntılı bir bakış için CSV'ye aktarın.

Analiz aracı hakkında daha fazla bilgiyi [burada](https://docs.microsoft.com/azure/application-insights/app-insights-analytics).

>[!NOTE]
>Topluluk kimlik geliştiricilere yardımcı olmak için bir kullanıcı gezisine Görüntüleyici geliştirmiştir.  Bu değil Microsoft tarafından desteklenir ve kesin olarak kullanılabilir hale-değil.  Application Insights örneğinden okur ve gezisine olayları kullanıcı iyi yapısı görünümünü sağlar.  Kaynak kodu alın ve kendi çözümde dağıtın.

Application Insights olayları okur Görüntüleyicisi sürümü bulunduğu [burada](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/wingtipgamesb2c/src/WingTipUserJourneyPlayerWebApplication)

>[!NOTE]
>Burada açıklanan ayrıntılı etkinlik günlükleri şu anda tasarlanmış **yalnızca** özel ilkeler geliştirmeye yardımcı olmak için. Geliştirme modunu üretimde kullanmayın.  Geliştirme sırasında gönderilen ve kimlik sağlayıcılardan gelen tüm talepler günlüklerini toplayın.  Üretimde kullandıysanız, geliştirici PII (özel olarak tanımlanabilen bilgiler) sahip oldukları App Insights günlüğünde toplanan sorumluluğunu varsayar.  Bu ayrıntılı günlükler yalnızca ilke yerleştirildiği olduğunda toplanır **geliştirme MODUNU**.

[Desteklenmeyen özel ilkesi örnekleri ve ilgili araçlar için Github depo](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies)



## <a name="next-steps"></a>Sonraki Adımlar

Application Insights'ı nasıl kimlik deneyimi kendi kimlik sunmak için temel alınan B2C çalışır Framework deneyimlerini anlamanıza yardımcı olması için verileri keşfedin.
