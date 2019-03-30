---
title: .NET uygulamaları için Azure Application Insights Snapshot Debugger yükseltme | Microsoft Docs
description: Snapshot Debugger, Azure App Services'ta veya aracılığıyla Nuget paketlerinin en son sürüme yükseltme
services: application-insights
author: MarioHewardt
manager: carmonm
ms.service: application-insights
ms.topic: conceptual
ms.date: 03/28/2019
ms.author: Mario.Hewardt
ms.reviewer: mbullwin
ms.openlocfilehash: 54b79897ee378cda52106fe704e25c50a4325f38
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58632632"
---
# <a name="upgrading-the-snapshot-debugger"></a>Snapshot Debugger'ı yükseltme

Verileriniz için olası en iyi güvenliği sağlamak için belirlenen saldırganlara karşı savunmasız gösterilen TLS 1.0 ve TLS 1.1 verdikleri Microsoft'tur. Site uzantısı'nın eski bir sürümünü kullanıyorsanız, çalışmaya devam etmek için bir yükseltme gerektirir. Bu belge, anlık görüntü hata ayıklayıcısı en son sürüme yükseltmek için gerekli olan adımları açıklanmaktadır. Snapshot Debugger site uzantısını kullanarak etkinleştirilirse veya eklenen uygulamanıza bir SDK'sı / Nuget kullandıysanız bağlı olarak iki birincil yükseltme yolları vardır. Her iki yükseltme yolları aşağıda ele alınmıştır. 

## <a name="upgrading-the-site-extension"></a>Site uzantısı'nı yükseltme

Snapshot debugger site uzantısını kullanarak etkinleştirilirse, aşağıdaki yordamı kullanarak kolayca yükseltebilirsiniz:

1. Azure Portal’da oturum açın.
2. Application Insights ve anlık görüntü hata ayıklayıcısı etkin olan kaynağınıza gidin. Örneğin, bir Web uygulaması için App Service kaynağına gidin:

   ![Ayrı App Service kaynak görüntüsü DiagService01 adlı](./media/snapshot-debugger-upgrade/app-service-resource.png)

3. Kaynağınıza yaptığında, Application Insights genel bakış dikey penceresinde tıklayın:

   ![Üç düğme ekran görüntüsü. Adı Application Insights ile orta düğmesi seçili](./media/snapshot-debugger-upgrade/application-insights-button.png)

4. Geçerli ayarlarla yeni bir dikey pencere açılır. Ayarlarınızı değiştirmek için bu fırsattan yararlanarak istemediğiniz sürece, bunları olduğu gibi bırakabilirsiniz. **Uygula** varsayılan olarak dikey pencerenin alt kısmındaki düğmesi etkin değil ve bir düğmeyi etkinleştirmek için ayarları değiştirmek gerekir. Gerçek herhangi bir ayarı değiştirmek zorunda kalmadan, bunun yerine ayarı değiştirin ve ardından hemen geri değiştirin. Ayarlama ve sonra seçerek Profiler geçiş öneririz **Uygula**.

   ![Uygula düğmesini kırmızı renkte vurgulanmış ekran görüntüsü, Application Insights uygulama hizmetini yapılandırma sayfası](./media/snapshot-debugger-upgrade/view-application-insights-data.png)

5. ' A tıkladığınızda **Uygula**, değişiklikleri onaylamanız istenir.

    > [!NOTE]
    > Site yükseltme işleminin bir parçası yeniden başlatılır.

   ![Ekran görüntüsü, App Service'nın izleme istemi uygulayın. Metin kutusu iletiyi görüntüler: "Biz şimdi uygulama ayarlarınızda değişiklikler uygulanır ve web uygulaması için Application Insights kaynağınıza bağlanmak için araçlarımızı yükler. Bu sitenin yeniden başlatır. Devam etmek istiyor musunuz?"](./media/snapshot-debugger-upgrade/apply-monitoring-settings.png)

6. Tıklayın **Evet** değişiklikleri uygulamak için. İşlem sırasında değişiklikler uygulanıyor gösteren bir bildirim görüntülenir:

   ![Sağ üst köşedeki görüntülenen uzantıları mesajı güncelleştirme Değişiklikleri Uygula - ekran görüntüsü](./media/snapshot-debugger-upgrade/updating-extensions.png)

Tamamlandıktan sonra bir **"Değişiklikler uygulanır"** bildirimi görüntülenir.

   ![Ekran görüntüsü değişiklikleri bildiren bir ileti uygulanır](./media/snapshot-debugger-upgrade/changes-are-applied.png)

Sitenin artık yükseltildi ve kullanıma hazır.

## <a name="upgrading-snapshot-debugger-using-sdknuget"></a>Snapshot Debugger SDK/Nuget kullanarak yükseltme

Uygulamanın bir sürümünü kullanıyorsanız `Microsoft.ApplicationInsights.SnapshotCollector` sürüm 1.3.1 için yükseltilmesi gerekir bir [sürüme](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) çalışmaya devam etmek için.
