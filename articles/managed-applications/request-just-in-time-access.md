---
title: Azure yönetilen uygulamalar için etkinleştirme ve istek tam zamanında erişim
description: Yayımcılar Azure yönetilen uygulamalar tam zamanında erişim yönetilen bir uygulamaya nasıl istek açıklar.
author: MSEvanhi
ms.service: managed-applications
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: evanhi
ms.openlocfilehash: ea933f5382cb42c01de523326b094c1813401132
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66481782"
---
# <a name="enable-and-request-just-in-time-access-for-azure-managed-applications"></a>Azure yönetilen uygulamalar için etkinleştirme ve istek tam zamanında erişim

Yönetilen uygulama tüketicileri yönetilen kaynak grubuna, kalıcı erişim vermek istemez olabilir. Yöneticisi uygulama yayımcısı tüketicilerinin tam olarak yönetilen kaynaklara erişmek gerektiğinde isteneceğini tercih edebilirsiniz. Tüketiciler vermek için şu anda Önizleme aşamasında olan, just-ın-time (JIT) erişim denilen bir özelliği Azure yönetilen uygulamalar, yönetilen kaynaklara erişim üzerinde daha fazla denetim sağlar.

JIT erişim sorun giderme ve bakım için yönetilen bir uygulamanın kaynaklarını yükseltilmiş erişim istemesine olanak tanır. Her zaman kaynakları salt okunur erişimine sahip, ancak belirli bir süre için daha fazla erişim sağlayabilirsiniz.

Erişim izni verme için iş akışı şöyledir:

1. Market'te bir yönetilen uygulama eklediğinizde ve JIT erişim kullanılabilir olduğunu belirtin.

1. Dağıtım sırasında tüketici yönetilen bir uygulamanın bu örneği için JIT erişim sağlar.

1. Dağıtımdan sonra tüketici JIT erişim ayarlarını değiştirebilirsiniz.

1. Sorun giderme veya yönetilen kaynakları güncelleştirme gerektiğinde erişim için bir istek gönderin.

1. Tüketici isteğiniz onaylar.

Bu makalede yayımcılardan JIT erişime izin verin ve istekleri göndermek için eylemleri üzerinde odaklanır. JIT erişim isteklerini onaylama hakkında bilgi edinmek için [Azure yönetilen uygulamalar tam zamanında erişim onaylama](approve-just-in-time-access.md).

## <a name="add-jit-access-step-to-ui"></a>Kullanıcı Arabirimi için JIT erişim adım ekleme

Tüketiciler JIT erişmesini sağlayan bir adımı içermelidir dışında CreateUiDefinition.json dosyanızı tam olarak oluşturduğunuz UI dosya kalıcı erişim gibidir. Azure Marketi'nde sunan ilk yönetilen uygulamanızı yayımlama hakkında daha fazla bilgi için bkz: [Azure yönetilen uygulamalar pazarında](publish-marketplace-app.md).

Teklifiniz için JIT özelliği desteklemek için aşağıdaki içeriği CreateUiDefinition.json dosyanıza ekleyin:

"Adımları":

```json
{
    "name": "jitConfiguration",
    "label": "JIT Configuration",
    "subLabel": {
        "preValidation": "Configure JIT settings for your application",
        "postValidation": "Done"
    },
    "bladeTitle": "JIT Configuration",
    "elements": [
        {
          "name": "jitConfigurationControl",
          "type": "Microsoft.Solutions.JitConfigurator",
          "label": "JIT Configuration"
        }
    ]
}
```
 
"Çıkışlarına":

```json
"jitAccessPolicy": "[parse(concat('{\"jitAccessEnabled\":', string(steps('jitConfiguration').jitConfigurationControl.jitEnabled), ',\"jitApprovalMode\":\"', steps('jitConfiguration').jitConfigurationControl.jitApprovalMode, '\",\"maximumJitAccessDuration\":\"', steps('jitConfiguration').jitConfigurationControl.maxAccessDuration, '\",\"jitApprovers\":', string(steps('jitConfiguration').jitConfigurationControl.approvers), '}'))]"
```

> [!NOTE]
> JIT erişimi Önizleme sürümündedir. JIT configuration için şema yinelemeler gelecekte değişebilir.

## <a name="enable-jit-access"></a>JIT ağ erişimi etkinleştir

Teklifiniz Market'te tanımlarken, JIT erişim etkinleştirdiğinizden emin olun.

1. Oturum [bulut iş ortağı yayımlama portalı](https://cloudpartner.azure.com).

1. Market'te yönetilen uygulamanızı yayımlamak için değerler sağlayın. Seçin **Evet** için **JIT erişimi etkinleştirme?**

   ![Tam zamanında erişim etkinleştir](./media/request-just-in-time-access/marketplace-enable.png)

JIT yapılandırma adımı UI ekledik ve JIT Access'te Market teklifini etkinleştirdiyseniz. Tüketicileri, yönetilen bir uygulama dağıttığınızda, yapabilirler [kendi örneği için JIT erişimi açın](approve-just-in-time-access.md#enable-during-deployment).

## <a name="request-access"></a>Erişim izni iste

Tüketicinin yönetilen kaynaklara erişmek gerektiğinde belirli bir rol, saati ve süre için bir istek gönderin. Tüketici sonra isteği onaylamanız gerekir.

JIT erişim isteği göndermek için:

1. Seçin **JIT erişim** erişmek için yönetilen uygulama için.

1. Seçin **uygun roller**seçip **etkinleştirme** Eylem sütununda, istediğiniz rolü için.

   ![Erişim isteği etkinleştir](./media/request-just-in-time-access/send-request.png)

1. Üzerinde **rol etkinleştirme** formunda, başlangıç zamanı ve etkin olarak rolünüz için süre seçin. Seçin **etkinleştirme** isteği göndermek için.

   ![Erişimi etkinleştir](./media/request-just-in-time-access/activate-access.png) 

1. Yeni JIT isteği başarıyla tüketiciye gönderilen görmek için bildirimleri görüntüleyin.

   ![Bildirim](./media/request-just-in-time-access/in-progress.png)

   Şimdi, tüketiciye beklemelisiniz [isteğinizi onaylamanız](approve-just-in-time-access.md#approve-requests).

1. Yönetilen bir uygulamaya ait tüm JIT istekleri durumunu görüntülemek için seçin **JIT erişim** ve **istek geçmişi**.

   ![Durumu görüntüle](./media/request-just-in-time-access/view-status.png)

## <a name="known-issues"></a>Bilinen sorunlar

JIT erişim isteğinde hesap asıl kimliği yönetilen uygulama tanımı içinde açıkça eklenmesi gerekir. Hesabı yalnızca pakette belirtilen bir grubu ile birlikte olamaz. Bu sınırlama, gelecekteki bir sürümde düzeltilecektir.

## <a name="next-steps"></a>Sonraki adımlar

JIT erişim isteklerini onaylama hakkında bilgi edinmek için [Azure yönetilen uygulamalar tam zamanında erişim onaylama](approve-just-in-time-access.md).
