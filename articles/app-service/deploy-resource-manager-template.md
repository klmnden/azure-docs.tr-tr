---
title: Şablonlar - Azure App Service ile Uygulamaları Dağıtma Kılavuzu | Microsoft Docs
description: Web uygulamalarını dağıtmak için Azure Resource Manager şablonları oluşturmaya yönelik öneriler.
services: app-service
documentationcenter: app-service
author: tfitzmac
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/03/2019
ms.author: tomfitz
ms.custom: seodec18
ms.openlocfilehash: d8fa8b216ca6044adefe1398b58f5d14630540e0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66137182"
---
# <a name="guidance-on-deploying-web-apps-by-using-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarını kullanarak web uygulaması Dağıtma Kılavuzu

Bu makalede, Azure App Service çözümlerini dağıtmak için Azure Resource Manager şablonları oluşturmaya yönelik öneriler sağlar. Bu öneriler, ortak sorunları önlemenize yardımcı olabilir.

## <a name="define-dependencies"></a>Bağımlıkları tanımlama

Web uygulamaları için bağımlılıklar tanımlayarak bir web uygulaması içindeki kaynaklara nasıl etkileşimde bulunduğunu anlamak gerekir. Hatalı bir sırayla bağımlılıkları belirtirseniz, dağıtım hatalarına neden veya dağıtım dökümlerinin bir yarış durumu oluşturun.

> [!WARNING]
> Şablonunuzda bir MSDeploy site uzantısı dahil, herhangi bir yapılandırma kaynağını MSDeploy kaynağına bağlı olarak ayarlamanız gerekir. Zaman uyumsuz olarak yeniden başlatmak site yapılandırma değişiklikleri neden. Yapılandırma kaynaklarını MSDeploy bağımlı hale getirerek site yeniden başlatılmadan önce MSDeploy bittiğini emin olun. Bu bağımlılıkları MSDeploy dağıtım işlemi sırasında site yeniden başlatılabilir. Örnek şablon için bkz: [WordPress Web dağıtma bağımlılık şablonuyla](https://github.com/davidebbo/AzureWebsitesSamples/blob/master/ARMTemplates/WordpressTemplateWebDeployDependency.json).

Aşağıdaki görüntüde, bağımlılık sırası çeşitli App Service kaynaklarını gösterilmektedir:

![Web uygulama bağımlılıkları](media/web-sites-rm-template-guidance/web-dependencies.png)

Aşağıdaki sırayla kaynakları dağıtma:

**Katman 1**
* App Service planı.
* Diğer tüm ilgili kaynakları, veritabanları veya depolama hesapları gibi.

**Katman 2**
* Web uygulaması, App Service planı üzerinde bağlıdır.
* Sunucu grubu--hedefleyen azure Application Insights örneği, App Service planı üzerinde bağlıdır.

**Katman 3**
* Kaynak denetimi--web uygulamasında bağlıdır.
* MSDeploy site uzantısı--web uygulamasında bağlıdır.
* Sunucu grubu--hedef application Insights örneği üzerinde web uygulaması bağlıdır.

**Katman 4**
* App Service sertifikası--bağımlı kaynak denetimi veya MSDeploy ya da mevcut değil. Aksi takdirde, web uygulamasında bağlıdır.
* Yapılandırma ayarları (bağlantı dizeleri, web.config değerleri, uygulama ayarları)--bağımlı kaynak denetimi veya MSDeploy ya da mevcut değilse. Aksi takdirde, web uygulamasında bağlıdır.

**Katman 5**
* Konak adı bağlamaları--varsa sertifikayı bağlıdır. Aksi takdirde, bir üst düzey kaynağa bağlıdır.
* Site uzantıları--varsa yapılandırma ayarlarına bağlıdır. Aksi takdirde, bir üst düzey kaynağa bağlıdır.

Genellikle, çözümünüz bu kaynakları ve katmanları yalnızca bazılarını içerir. Eksik katmanları için sonraki daha yüksek bir katman için alt kaynakları eşleyin.

Aşağıdaki örnek, bir şablonun parçası gösterir. Bağlantı dizesi yapılandırma değerini MSDeploy uzantısını üzerinde bağlıdır. Web uygulaması ve veritabanı MSDeploy uzantısı bağlıdır. 

```json
{
    "name": "[parameters('appName')]",
    "type": "Microsoft.Web/Sites",
    ...
    "resources": [
      {
          "name": "MSDeploy",
          "type": "Extensions",
          "dependsOn": [
            "[concat('Microsoft.Web/Sites/', parameters('appName'))]",
            "[concat('Microsoft.Sql/servers/', parameters('dbServerName'), '/databases/', parameters('dbName'))]",
          ],
          ...
      },
      {
          "name": "connectionstrings",
          "type": "config",
          "dependsOn": [
            "[concat('Microsoft.Web/Sites/', parameters('appName'), '/Extensions/MSDeploy')]"
          ],
          ...
      }
    ]
}
```

Yukarıdaki kod çalıştırılmaya hazır örnek için bkz [şablonu: Basit bir Umbraco Web uygulaması derleme](https://github.com/Azure/azure-quickstart-templates/tree/master/umbraco-webapp-simple).

## <a name="find-information-about-msdeploy-errors"></a>MSDeploy hatalar hakkında bilgi

Resource Manager şablonunuzu MSDeploy kullanıyorsa, dağıtım hata iletileri anlamak zor olabilir. Başarısız bir dağıtımdan sonra daha fazla bilgi edinmek için aşağıdaki adımları deneyin:

1. Sitenin Git [Kudu konsolunu](https://github.com/projectkudu/kudu/wiki/Kudu-console).
2. Browse to the folder at D:\home\LogFiles\SiteExtensions\MSDeploy.
3. AppManagerStatus.xml ve appManagerLog.xml dosyaları arayın. İlk dosya durumunu kaydeder. Dosyanın ikinci hata hakkındaki bilgileri kaydeder. Hata, açık değilse, forum hakkında Yardım almak için soran olduğunda içerebilir.

## <a name="choose-a-unique-web-app-name"></a>Benzersiz web uygulaması adı seçin

Web uygulamanız için bir ad genel olarak benzersiz olmalıdır. Bir adlandırma kuralını kullanabilirsiniz, büyük olasılıkla benzersiz olan veya kullanabileceğiniz [uniqueString işlevi](../azure-resource-manager/resource-group-template-functions-string.md#uniquestring) benzersiz bir ad oluşturma ile yardımcı olmak için.

```json
{
  "apiVersion": "2016-08-01",
  "name": "[concat(parameters('siteNamePrefix'), uniqueString(resourceGroup().id))]",
  "type": "Microsoft.Web/sites",
  ...
}
```

## <a name="deploy-web-app-certificate-from-key-vault"></a>Key vault'tan Web uygulaması sertifikası dağıtma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Şablonunuzu içeriyorsa bir [Microsoft.Web/certificates](/azure/templates/microsoft.web/certificates) kaynak SSL bağlaması ve sertifika için anahtar Kasası'nda depolanır, App Service kimlik sertifikası erişebilir emin olmanız gerekir.

Genel Azure'da App Service hizmet sorumlusu kimliği vardır. **abfa0a7c-a6b6-4736-8310-5855508787cd**. App Service hizmet sorumlusu için Key Vault'a erişim vermek için kullanın:

```azurepowershell-interactive
Set-AzKeyVaultAccessPolicy `
  -VaultName KEY_VAULT_NAME `
  -ServicePrincipalName abfa0a7c-a6b6-4736-8310-5855508787cd `
  -PermissionsToSecrets get `
  -PermissionsToCertificates get
```

Azure devlet kurumları, App Service hizmet sorumlusu kimliği olan **6a02c803-dafd-4136-b4c3-5a6f318b4714**. Önceki örnekte bu kimliği kullanın.

Anahtar kasanızı seçin **sertifikaları** ve **Oluştur/içeri aktarma** sertifikayı karşıya yüklemek için.

![Sertifikayı içeri aktar](media/web-sites-rm-template-guidance/import-certificate.png)

Şablonunuzda, sertifikanın adını sağlayın `keyVaultSecretName`.

Örnek şablon için bkz: [Key Vault gizli dizilerinden bir Web uygulaması sertifikası dağıtın ve SSL bağlaması oluşturmak için kullanmak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-certificate-from-key-vault).

## <a name="next-steps"></a>Sonraki adımlar

* Bir şablon ile web uygulaması dağıtma ilişkin bir öğretici için bkz. [sağlayın ve tahmin edilebilir bir biçimde azure'da mikro hizmetlerin dağıtımı](deploy-complex-application-predictably.md).
* JSON söz dizimi ve özellikleri şablonlarında kaynak türleri hakkında bilgi edinmek için bkz. [Azure Resource Manager şablon başvurusu](/azure/templates/).
