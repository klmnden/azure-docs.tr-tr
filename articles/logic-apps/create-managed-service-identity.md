---
title: Yönetilen kimlik - Azure Logic Apps ile kimlik doğrulaması | Microsoft Docs
description: Oturum açma olmadan kimlik doğrulaması için (yönetilen hizmet kimliği veya MSI eski adıyla) yönetilen bir kimlik oluşturabilirsiniz mantıksal uygulamanızı diğer kaynaklara erişebilmeniz için kimlik bilgileri veya gizli dizileri Azure Active Directory (Azure AD) Kiracı
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
services: logic-apps
ms.service: logic-apps
ms.suite: integration
ms.topic: article
ms.date: 03/29/2019
ms.openlocfilehash: 8445b67fa049116d93f3710ff108f904ca7ecd77
ms.sourcegitcommit: e43ea344c52b3a99235660960c1e747b9d6c990e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "59010558"
---
# <a name="authenticate-and-access-resources-with-managed-identities-in-azure-logic-apps"></a>Kimlik doğrulaması ve Azure Logic apps'te yönetilen kimliklerle kaynaklara erişin

Diğer Azure Active Directory (Azure AD) Kiracı kaynaklara erişmek ve açmadan kimliğinizi doğrulamak için mantıksal uygulamanızı kullanabilirsiniz bir [yönetilen kimliği](../active-directory/managed-identities-azure-resources/overview.md) (eski adıyla yönetilen hizmet kimliği veya MSI olarak bilinir), yerine kimlik bilgileri veya gizli dizileri. Bu kimlik, Azure yönetir ve sağlamak veya gizli dizileri döndürmek zorunda olmadığınız için yardımcı kimlik bilgilerinizi koruyun. Bu makalede nasıl ayarlayabilir ve mantıksal uygulamanız için bir sistem tarafından atanan bir yönetilen kimliği kullanma gösterilmektedir. Yönetilen kimlikler hakkında daha fazla bilgi için bkz. [Azure kaynakları için yönetilen kimlikleri nedir?](../active-directory/managed-identities-azure-resources/overview.md)

> [!NOTE]
> Mantıksal uygulamanızı yönetilen kimlikleri yönetilen kimlikleri destekleyen bağlayıcıları kullanabilirsiniz. Şu anda yalnızca HTTP Bağlayıcısı yönetilen kimlikleri destekler.
>
> Şu anda sistem tarafından atanan ile 10 mantıksal uygulama iş akışlarını kadar her bir Azure aboneliği kimliklerini yönetilen.

## <a name="prerequisites"></a>Önkoşullar

* Bir Azure aboneliğine veya abonelik yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

* Sistem tarafından atanan kullanmak istediğiniz mantıksal uygulamayı yönetilen kimliği. Mantıksal uygulama yoksa bkz [ilk mantıksal uygulama iş akışınızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="enable-identity"></a>

## <a name="enable-managed-identity"></a>Yönetilen kimlik etkinleştir

Sistem tarafından atanan yönetilen kimlikleri için bu kimlik el ile oluşturmanız gerekmez. Sistem tarafından atanan yönetilen bir kimlik mantıksal uygulamanızın ayarlamak için bu şekilde kullanabilirsiniz: 

* [Azure portal](#azure-portal) 
* [Azure Resource Manager şablonları](#template) 
* [Azure PowerShell](../active-directory/managed-identities-azure-resources/howto-assign-access-powershell.md) 

<a name="azure-portal"></a>

### <a name="azure-portal"></a>Azure portal

Sistem tarafından atanan yönetilen bir kimlik mantıksal uygulamanızın Azure portalından etkinleştirmek için açma **sistem tarafından atanan** mantıksal uygulamanızın kimlik ayarlarında ayarlama.

1. İçinde [Azure portalında](https://portal.azure.com), Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın.

1. Mantıksal uygulama menüsünde altında **ayarları**seçin **kimlik**. 

1. Altında **sistem tarafından atanan** > **durumu**, seçin **üzerinde**. Ardından, **Kaydet** > **Evet**.

   ![Yönetilen kimlik ayarı](./media/create-managed-service-identity/turn-on-managed-service-identity.png)

   Mantıksal uygulamanız artık Azure Active Directory'de kayıtlı sistem tarafından atanan yönetilen bir kimlik vardır:

   ![Nesne Kimliği GUID'leri](./media/create-managed-service-identity/object-id.png)

   | Özellik | Değer | Açıklama | 
   |----------|-------|-------------| 
   | **Nesne Kimliği** | <*kimlik kaynak kimliği*> | Temsil eden sistem tarafından atanan bir genel benzersiz tanıtıcısı (GUID) yönetilen bir Azure AD kiracısında mantıksal uygulamanızın kimliği | 
   ||| 

<a name="template"></a>

### <a name="azure-resource-manager-template"></a>Azure Resource Manager şablonu

Logic apps gibi Azure kaynaklarını oluşturma ve dağıtımı otomatik hale getirmek istediğinizde, kullanabileceğiniz [Azure Resource Manager şablonları](../logic-apps/logic-apps-create-deploy-azure-resource-manager-templates.md). Sistem tarafından atanan yönetilen bir kimlik için bir şablon aracılığıyla mantıksal uygulamanızı oluşturmak için Ekle `"identity"` öğesi ve `"type"` dağıtım şablonunuzdaki mantıksal uygulama iş akışı tanımı özelliği: 

```json
"identity": {
   "type": "SystemAssigned"
}
```

Örneğin:

```json
{
   "apiVersion": "2016-06-01", 
   "type": "Microsoft.logic/workflows", 
   "name": "[variables('logicappName')]", 
   "location": "[resourceGroup().location]", 
   "identity": { 
      "type": "SystemAssigned" 
   }, 
   "properties": { 
      "definition": { 
         "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#", 
         "actions": {}, 
         "parameters": {}, 
         "triggers": {}, 
         "contentVersion": "1.0.0.0", 
         "outputs": {} 
   }, 
   "parameters": {}, 
   "dependsOn": [] 
}
```

Azure mantıksal uygulamanızı oluşturduğunda, bu mantıksal uygulamanın iş akışı tanımı bu ek özellikler içerir:

```json
"identity": {
   "type": "SystemAssigned",
   "principalId": "<principal-ID>",
   "tenantId": "<Azure-AD-tenant-ID>"
}
```

| Özellik | Değer | Açıklama | 
|----------|-------|-------------|
| **Principalıd** | <*Sorumlu Kimliği*> | Mantıksal uygulama Azure AD kiracısını ve bazen temsil eden bir genel benzersiz tanıtıcısı (GUID), bir "nesne kimliği" görünür veya `objectID` | 
| **Kiracı kimliği** | <*Azure AD Kiracı kimliği*> | Bir genel benzersiz tanımlayıcı (mantıksal uygulama şimdi üyesi olduğu Azure AD kiracısı temsil eden GUID). Azure AD kiracısı içinde hizmet sorumlusu mantıksal uygulama örneği ile aynı ada sahiptir. | 
||| 

<a name="access-other-resources"></a>

## <a name="access-resources-with-managed-identity"></a>Yönetilen kimlik kaynaklarla erişim

Mantıksal uygulamanız için sistem tarafından atanan bir yönetilen kimlik oluşturduktan sonra [diğer Azure kaynakları için bu kimlik erişmesini](../active-directory/managed-identities-azure-resources/howto-assign-access-portal.md). Daha sonra bu kimlik gibi diğer kimlik doğrulaması için kullanabilirsiniz [hizmet sorumlusu](../active-directory/develop/app-objects-and-service-principals.md). 

> [!NOTE]
> Hem sistem tarafından atanan bir yönetilen kimlik hem de, erişim atamak istediğiniz kaynak aynı Azure aboneliğinde olması gerekir.

### <a name="assign-access-to-managed-identity"></a>Yönetilen kimliğe erişim atama

Mantıksal uygulamanızın sistem tarafından atanan yönetilen kimlik için başka bir Azure kaynağına erişim vermek için bu adımları izleyin:

1. Azure portalında, yönetilen kimliğiniz için erişim atamak istediğiniz Azure kaynağına gidin. 

1. Kaynağın menüden **erişim denetimi (IAM)** ve **rol ataması Ekle**. 

   ![Rol ataması ekle](./media/create-managed-service-identity/add-permissions-logic-app.png)

1. Altında **rol ataması Ekle**seçin **rol** kimliği için istediğiniz. 

1. İçinde **erişim Ata** özelliği, select **Azure AD kullanıcı, Grup veya hizmet sorumlusu**, henüz seçili değilse.

1. İçinde **seçin** kutusunda, mantıksal uygulamanızın adının ilk karakteri ile başlayarak, mantıksal uygulamanızın adı girin. Mantıksal uygulamanızı göründüğünde, mantıksal uygulama seçin.

   ![Yönetilen kimlik ile mantıksal uygulama seçin](./media/create-managed-service-identity/add-permissions-select-logic-app.png)

1. İşiniz bittiğinde **Kaydet**’i seçin.

### <a name="authenticate-with-managed-identity-in-logic-app"></a>Mantıksal uygulama içinde yönetilen kimlik ile kimlik doğrulaması

Mantıksal uygulamanız ile ayarladıktan sonra sistem tarafından atanan kimliği yönetilen ve söz konusu kimlik için istediğiniz kaynağa erişim atanan, artık bu kimlik için kimlik doğrulaması kullanabilirsiniz. Örneğin, mantıksal uygulamanızı yapabilir veya bir HTTP isteği göndermek için bu kaynak çağrısı, bir HTTP eylemi kullanabilirsiniz. 

1. Mantıksal uygulamanızı eklemek **HTTP** eylem.

1. Bu eylem, isteği gibi gerekli bilgileri sağlayın **yöntemi** ve **URI** çağırmak istediğiniz kaynak konumu.

   Örneğin, Azure Active Directory (Azure AD) kimlik doğrulaması ile kullandığınız varsayalım [Azure AD'ye destekleyen Azure Bu hizmetlerden biri](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication). 
   İçinde **URI** kutusunda, ilgili Azure hizmeti için uç nokta URL'sini girin. 
   Azure Resource Manager kullanıyorsanız, bu nedenle, bu değer girin **URI** özelliği:

   `https://management.azure.com/subscriptions/<Azure-subscription-ID>?api-version=2016-06-01`

1. HTTP eylemi seçin **Gelişmiş Seçenekleri Göster**.

1. Gelen **kimlik doğrulaması** listesinden **yönetilen kimliği**. Bu kimlik doğrulaması'nı seçtikten sonra **İzleyici** özelliği varsayılan kaynak kimliği değeri ile görünür:

   !["Yönetilen kimliği" seçin](./media/create-managed-service-identity/select-managed-service-identity.png)

   > [!IMPORTANT]
   > 
   > İçinde **İzleyici** özelliği, hangi Azure AD bekliyor kaynağı kimliği değeri tam olarak eşleşmelidir, gerekli sondaki eğik çizgi dahil. 
   > Bu kaynak kimliği değerleri bu bulabilirsiniz [Azure açıklayan tablo destekleyen Azure AD Hizmetleri](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication). 
   > Örneğin, Azure kaynak yöneticisi kaynak Kimliğini kullanıyorsanız, URI sonunda eğik çizgi olduğundan emin olun.

1. İstediğiniz gibi mantıksal uygulama oluşturmaya devam edin.

<a name="remove-identity"></a>

## <a name="remove-managed-identity"></a>Yönetilen kimlik Kaldır

Sistem tarafından atanan yönetilen bir kimlik mantıksal uygulamanız üzerinde devre dışı bırakmak için nasıl Azure portalı, Azure Resource Manager dağıtım şablonlarını veya Azure PowerShell aracılığıyla kimlik ayarlamanız benzer adımları izleyebilirsiniz.

Mantıksal uygulamanızı silseniz bile Azure mantıksal uygulamanızın sistem tarafından atanan kimlik Azure AD'den otomatik olarak kaldırır.

### <a name="azure-portal"></a>Azure portal

Sistem tarafından atanan yönetilen bir kimlik mantıksal uygulamanızın Azure Portalından kaldırmak için devre dışı **sistem tarafından atanan** mantıksal uygulamanızın kimlik ayarlarında ayarlama.

1. İçinde [Azure portalında](https://portal.azure.com), Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın.

1. Mantıksal uygulama menüsünde altında **ayarları**seçin **kimlik**. 

1. Altında **sistem tarafından atanan** > **durumu**, seçin **kapalı**. Ardından, **Kaydet** > **Evet**.

   ![Yönetilen kimlik ayarı devre dışı bırakmak](./media/create-managed-service-identity/turn-off-managed-service-identity.png)

### <a name="deployment-template"></a>Dağıtım şablonu

Mantıksal uygulamanın yönetilen kimlik sistem tarafından atanan bir Azure Resource Manager dağıtım şablonu ile oluşturulan verilirse `"identity"` öğenin `"type"` özelliğini `"None"`. Bu eylem, sorumlu Kimliğini de Azure AD'den siler.

```json
"identity": {
   "type": "None"
}
```

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.
