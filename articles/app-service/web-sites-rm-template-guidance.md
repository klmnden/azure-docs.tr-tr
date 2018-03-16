---
title: "Şablonları kullanarak Azure web uygulamaları dağıtma Kılavuzu | Microsoft Docs"
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
ms.openlocfilehash: dc816bb6e95d2800d79124dfac60b55e88eaa500
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="guidance-on-deploying-web-apps-by-using-azure-resource-manager-templates"></a>Azure Resource Manager şablonları kullanarak web uygulamaları dağıtma Kılavuzu

Bu makalede Azure Resource Manager şablonları oluşturmak için Azure App Service çözümlerini dağıtmak için öneriler sağlar. Bu öneriler, ortak sorunları önlemenize yardımcı olabilir.

## <a name="define-dependencies"></a>Bağımlıkları tanımlama

Web uygulamaları için bağımlılıkları tanımlayan bir web uygulaması içindeki kaynaklara nasıl etkileşim, bilinmesini gerektirir. Hatalı bir sırayla bağımlılıkları belirtirseniz, dağıtım hatalarına neden veya dağıtım dökümlerinin bir yarış durumu oluşturun.

> [!WARNING]
> Şablonunuzda MSDeploy site uzantısı dahil, herhangi bir yapılandırma kaynağı MSDeploy kaynağına bağımlı olarak ayarlamanız gerekir. Zaman uyumsuz olarak yeniden başlatmak site yapılandırma değişiklikleri neden. Yapılandırma kaynakları MSDeploy bağımlı hale getirerek site yeniden başlatılmadan önce geçecek MSDeploy tamamladığından emin olun. Bu bağımlılıklar site MSDeploy dağıtım işlemi sırasında yeniden başlatılabilir. Bir örnek için bkz [Web dağıtımı bağımlılığı WordPress şablonla](https://github.com/davidebbo/AzureWebsitesSamples/blob/master/ARMTemplates/WordpressTemplateWebDeployDependency.json).

Aşağıdaki resimde, çeşitli uygulama hizmeti kaynaklarına bağımlılık sırası gösterilmektedir:

![Web uygulama bağımlılıkları](media/web-sites-rm-template-guidance/web-dependencies.png)

Aşağıdaki sırayla kaynakları dağıtın:

**Katman 1**
* Uygulama hizmeti planı.
* Diğer ilgili kaynakları, veritabanları veya depolama hesapları gibi.

**Katman 2**
* Web uygulaması--uygulama hizmeti plan üzerinde bağlıdır.
* Sunucu grubu--hedefler azure Application Insights örneği uygulama hizmeti plan üzerinde bağlıdır.

**Katman 3**
* Kaynak denetimi--web uygulamasında bağlıdır.
* MSDeploy site uzantısı--web uygulamasında bağlıdır.
* Sunucu grubu--hedefleyen uygulama Öngörüler örneği web uygulamasında bağlıdır.

**Katman 4**
* Uygulama hizmeti sertifika--kaynak denetimi bağlıdır veya MSDeploy ya da varsa mevcut. Aksi takdirde, web uygulamasında bağlıdır.
* Yapılandırma ayarları (bağlantı dizeleri, web.config değerleri, uygulama ayarlarını)--kaynak denetimi ve MSDeploy göre değişir ya da mevcut ise. Aksi takdirde, web uygulamasında bağlıdır.

**Katman 5**
* Konak adı bağlamaları--varsa sertifikadaki bağlıdır. Aksi takdirde, üst düzey bir kaynağa bağlıdır.
* Site uzantıları--varsa yapılandırma ayarlarına bağlıdır. Aksi takdirde, üst düzey bir kaynağa bağlıdır.

Genellikle, çözümünüzün bu kaynakları ve katmanları yalnızca bazılarını içerir. Eksik katmanları için alt kaynakları İleri daha yüksek katman eşleyin.

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
2. Browse to the folder at D:\home\LogFiles\SiteExtensions\MSDeploy.
3. AppManagerStatus.xml ve appManagerLog.xml dosyaları arayın. İlk dosya durumunu kaydeder. İkinci dosya hata hakkındaki bilgileri kaydeder. Hata, açık değilse, forum ilgili Yardım isteyen zaman içerebilir.

## <a name="choose-a-unique-web-app-name"></a>Benzersiz web uygulaması adı seçin

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