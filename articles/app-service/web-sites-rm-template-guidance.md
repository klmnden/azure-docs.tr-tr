---
title: "Azure web uygulamaları şablonları ile Dağıtma Kılavuzu | Microsoft Docs"
description: "Web uygulamalarını dağıtmak için Azure Resource Manager şablonları oluşturmak için öneriler sunar."
services: app-service
documentationcenter: app-service
author: tfitzmac
manager: timlt
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2018
ms.author: tomfitz
ms.openlocfilehash: eff0b0e6258217fa8fbf7f15606f98d70fecd7c5
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="guidance-on-deploying-web-apps-with-azure-resource-manager-templates"></a>Azure Resource Manager şablonları ile web uygulamaları dağıtma Kılavuzu

Bu makalede Azure Resource Manager şablonları oluşturmak için uygulama hizmeti çözümlerini dağıtmak için öneriler sağlar. Bu öneriler, ortak sorunları önlemenize yardımcı olabilir.

## <a name="define-dependencies"></a>Bağımlıkları tanımlama

Web uygulamaları için bağımlılıklar tanımlayarak, bir Web uygulaması içindeki kaynaklara nasıl etkileşim bilinmesini gerektirir. Hatalı bir sırayla bağımlılıkları belirtirseniz, dağıtım hatalarına neden veya dağıtım dökümlerinin bir yarış durumu oluşturun.

> [!WARNING]
> Şablonunuzda MS Deploy site uzantısı dahil ederseniz, herhangi bir yapılandırma kaynağı MS Deploy kaynağına bağımlı olarak ayarlamanız gerekir. Zaman uyumsuz olarak yeniden başlatmak site yapılandırma değişiklikleri neden. Yapılandırma kaynakları MS Deploy bağımlı hale getirerek site yeniden başlatılmadan önce geçecek MS Deploy bittikten emin olun. Bu bağımlılıklar site MS Deploy'nın dağıtım işlemi sırasında yeniden başlatılabilir. Bir örnek için bkz [Web dağıtımı bağımlılığı Wordpress şablonla](https://github.com/davidebbo/AzureWebsitesSamples/blob/master/ARMTemplates/WordpressTemplateWebDeployDependency.json).

Aşağıdaki resimde, çeşitli uygulama hizmeti kaynaklar için bağımlılık sırayı gösterir.

![Web uygulama bağımlılıkları](media/web-sites-rm-template-guidance/web-dependencies.png)

Aşağıdaki sırayla kaynakları dağıtın:

**Katman 1**
* App Service Planı
* Veritabanları veya depolama hesapları gibi ilgili diğer kaynaklar

**Katman 2**
* Web uygulaması - uygulama hizmeti planı bağlıdır
* Sunucu grubu - hedef application Insights uygulama hizmeti planı bağlıdır

**Katman 3**
* Kaynak denetimi - web uygulamasında bağlıdır
* MSDeploy site uzantısı - web uygulamasında bağlıdır
* Sunucu grubu - hedef application Insights web uygulamasında bağlıdır

**Katman 4**
* Uygulama hizmeti sertifikası - kaynak denetimi ve MSDeploy göre değişir ya da mevcut ise; Aksi takdirde, web uygulamasında bağlıdır
* Yapılandırma ayarları (bağlantı dizeleri, web yapılandırma değerleri, uygulama ayarlarını) - kaynak denetimi ve MSDeploy göre değişir ya da mevcut ise; Aksi takdirde, web uygulamasında bağlıdır

**Katman 5**
* Konak adı bağlamaları - sertifika varsa; bağlıdır Aksi takdirde, üst düzey bir kaynak
* Site uzantıları - yapılandırma ayarlarına varsa; bağlıdır Aksi takdirde, üst düzey bir kaynak

Genellikle, çözümünüzün bu kaynakları ve katmanları yalnızca bazılarını içerir. Eksik katmanları için sonraki daha yüksek katman alt kaynakları eşleyin.

Aşağıdaki örnek, bir şablon parçası gösterir. Bağlantı dizesi yapılandırma değeri MSDeploy uzantısına göre değişir. MSDeploy uzantısı, web uygulaması ve veritabanı bağlıdır.

```json
{
    "name": "[parameters('name')]",
    "type": "Microsoft.Web/sites",
    "resources": [
      {
          "name": "MSDeploy",
          "type": "Extensions",
          "dependsOn": [
            "[concat('Microsoft.Web/Sites/', parameters('name'))]",
            "[concat('SuccessBricks.ClearDB/databases/', parameters('databaseName'))]"
          ],
          ...
      },
      {
          "name": "connectionstrings",
          "type": "config",
          "dependsOn": [
            "[concat('Microsoft.Web/Sites/', parameters('name'), '/Extensions/MSDeploy')]"
          ],
          ...
      }
    ]
}
```

## <a name="find-information-about-msdeploy-errors"></a>MSDeploy hatalar hakkında bilgi

Resource Manager şablonu MSDeploy kullanıyorsa, dağıtım hata iletileri anlaşılması zor olabilir. Başarısız bir dağıtımdan sonra daha fazla bilgi edinmek için aşağıdaki adımları deneyin:

1. Sitenin gidin [Kudu konsol](https://github.com/projectkudu/kudu/wiki/Kudu-console).
2. Klasöre gidin `D:\home\LogFiles\SiteExtensions\MSDeploy`.
3. Ara *appManagerStatus.xml* ve *appManagerLog.xml* dosyaları. İlk dosya durumunu kaydeder. İkinci dosya hata hakkındaki bilgileri kaydeder. Hata, açık değilse, forum hakkında Yardım için isterken içerebilir.

## <a name="unique-web-app-name"></a>Benzersiz web uygulaması adı

Web uygulamanız için ad genel olarak benzersiz olmalıdır. Bir adlandırma kuralını kullanabilirsiniz, büyük olasılıkla benzersiz veya kullanabileceğiniz [uniqueString işlevi](../azure-resource-manager/resource-group-template-functions-string.md#uniquestring) benzersiz bir ad oluşturma ile yardımcı olmak için.

```json
{
  "apiVersion": "2016-08-01",
  "name": "[concat(parameters('siteNamePrefix'), uniqueString(resourceGroup().id))]",
  "type": "Microsoft.Web/sites",
  ...
}
```

## <a name="next-steps"></a>Sonraki adımlar

* Bir şablon ile web uygulamaları dağıtma, Öğretici için bkz: [sağlamak ve mikro beklendiği azure'da dağıtmak](app-service-deploy-complex-application-predictably.md).