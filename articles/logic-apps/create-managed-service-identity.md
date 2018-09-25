---
title: Erişim ve Azure Logic Apps-açmanıza gerek kalmadan kimlik doğrulaması | Microsoft Docs
description: Mantıksal uygulamanız kimlik doğrulaması ve kimlik bilgilerinizi olmadan diğer Azure Active Directory (Azure AD) Kiracı kaynaklara erişmek için bir yönetilen kimlik oluşturma
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
services: logic-apps
ms.service: logic-apps
ms.suite: integration
ms.topic: article
ms.date: 09/24/2018
ms.openlocfilehash: fb1c31e6e7c075e20191a4e51d7b1a9323f3b979
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46973975"
---
# <a name="access-resources-and-authenticate-as-managed-identities-in-azure-logic-apps"></a>Kaynaklara erişebilir ve Azure Logic apps'te yönetilen kimlik olarak kimliğinizi doğrulayın

Diğer Azure Active Directory (Azure AD) Kiracı kaynaklara erişmek ve açmadan kimliğinizi doğrulamak için oluşturabileceğiniz bir [yönetilen kimliği](../active-directory/managed-identities-azure-resources/overview.md) , mantıksal uygulamanızı yerine kimlik bilgilerinizi kullanır. Bu kimlik, Azure yönetir ve sağlamak veya gizli dizileri döndürmek zorunda olmadığınız için kimlik bilgilerinizi yardımcı güvenli. Bu makalede, oluşturma ve mantıksal uygulamanız için bir yönetilen kimliği kullanma gösterilmektedir. Daha fazla bilgi için [Azure kaynakları için kimlikleri yönetme](../app-service/app-service-managed-service-identity.md).

> [!NOTE]
> Azure kaynakları için yönetilen kimlikleri olan daha önce yönetilen hizmet kimliği (MSI) olarak bilinen hizmetin yeni ad.

## <a name="prerequisites"></a>Önkoşullar

* Bir Azure aboneliğine veya abonelik yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

* Yönetilen kimlik olarak kullanmak istediğiniz mantıksal uygulaması. Mantıksal uygulama yoksa bkz [ilk mantıksal uygulama iş akışınızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="create-identity"></a>

## <a name="create-managed-identity"></a>Yönetilen kimlik oluşturma

Oluşturun veya Azure portalı, Azure Resource Manager şablonlarını veya Azure PowerShell aracılığıyla mantıksal uygulamanız için bir yönetilen kimlik etkinleştirin. 

### <a name="azure-portal"></a>Azure portal

Mantıksal uygulamanızın Azure portalı üzerinden yönetilen bir kimlik oluşturmak için açma **Azure Active Directory ile kayıt** mantıksal uygulamanızın iş akışı ayarlarını.

1. İçinde [Azure portalında](https://portal.azure.com), Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın.

1. Şu adımları uygulayın: 

   1. Mantıksal uygulama menüsünde altında **ayarları**seçin **iş akışı ayarları**. 

   1. Altında **yönetilen hizmet kimliği** > 
    **Azure Active Directory ile kayıt**, seçin **üzerinde**.

   1. İşiniz bittiğinde seçin **Kaydet** araç.

      ![Yönetilen kimlik ayarı](./media/create-managed-service-identity/turn-on-managed-service-identity.png)

      Azure, artık bu özellikleri ve mantıksal uygulamanızın yönetilen kimlik değerleri gösterir:

      ![Sorumlu Kimliği ve Kiracı kimliği için GUID](./media/create-managed-service-identity/principal-tenant-id.png)

      | Özellik | Değer | Açıklama | 
      |----------|-------|-------------| 
      | **Sorumlu Kimliği** | <*Sorumlu Kimliği GUID*> | Mantıksal uygulama Azure AD kiracısında temsil eden bir genel benzersiz tanıtıcısı (GUID) | 
      | **Kiracı kimliği** | <*Azure-AD-Kiracı--kimliği GUID*> | Bir genel benzersiz tanımlayıcı (mantıksal uygulamanız artık üyesi olduğu Azure AD kiracısı temsil eden GUID). Azure AD kiracısı içinde hizmet sorumlusu mantıksal uygulama örneği ile aynı ada sahiptir. | 
      ||| 

### <a name="deployment-template"></a>Dağıtım şablonu

Logic apps gibi Azure kaynaklarını oluşturma ve dağıtımı otomatikleştirmek için Azure Resource Manager şablonları ayarlayabilirsiniz. Daha fazla bilgi için [oluştur ve logic apps ile Azure Resource Manager şablonlarını dağıtma](../logic-apps/logic-apps-create-deploy-azure-resource-manager-templates.md). 

Mantıksal uygulamanızın bir şablon aracılığıyla yönetilen bir kimlik oluşturmak için Ekle **kimlik** öğesi ve **türü** dağıtım şablonunuzdaki mantıksal uygulama iş akışı tanımı özelliği. Bu ayarlar, Azure'nın oluşturur ve mantıksal uygulamanız için bu kimlik yönetir belirtin:

```json
"identity": {
    "type": "SystemAssigned"
}
```

Örneğin, mantıksal uygulamanız bu sürümünü gibi görünebilir:

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
    "principalId": "<principal-ID-GUID>",
    "tenantId": "<Azure-AD-tenant-ID>-GUID"
}
```

| Özellik | Değer | Açıklama | 
|----------|-------|-------------|
| **Principalıd** | <*Sorumlu Kimliği GUID*> | Mantıksal uygulama Azure AD kiracısında temsil eden bir genel benzersiz tanıtıcısı (GUID) | 
| **tenantId** | <*Azure-AD-Kiracı--kimliği GUID*> | Bir genel benzersiz tanımlayıcı (mantıksal uygulama şimdi üyesi olduğu Azure AD kiracısı temsil eden GUID). Azure AD kiracısı içinde hizmet sorumlusu mantıksal uygulama örneği ile aynı ada sahiptir. | 
||| 

<a name="access-other-resources"></a>

## <a name="access-resources-with-managed-identity"></a>Yönetilen kimlik kaynaklarla erişim

Mantıksal uygulamanız için bir yönetilen kimlik oluşturduktan sonra [o kimlik diğer kaynaklara erişmesini](../active-directory/managed-identities-azure-resources/howto-assign-access-portal.md). Ardından, yönetilen kimlik gibi diğer kimlik doğrulaması için kullanabilirsiniz [hizmet sorumlusu](../active-directory/develop/app-objects-and-service-principals.md). 

Örneğin, zaten başka bir kaynağa erişimi olan bir yönetilen kimlik ile bir mantıksal uygulama'kurmak ayarladığınızı düşünelim. Mantıksal uygulamanızı yapabilir veya bir HTTP isteği göndermek için bu kaynak çağrısı bir HTTP eylemi artık ekleyebilirsiniz. 

1. Mantıksal uygulamanızı eklemek **HTTP** eylem. 

1. Bu eylem, isteği gibi gerekli bilgileri sağlayın **yöntemi** ve **URI** çağırmak istediğiniz kaynak konumu.

1. HTTP eylemi seçin **Gelişmiş Seçenekleri Göster**. 

1. Gelen **kimlik doğrulaması** listesinden **yönetilen hizmet kimliği**, sonra hangi gösterir **İzleyici** özelliği ayarlamanız için:

   !["Yönetilen hizmet kimliği" seçin](./media/create-managed-service-identity/select-managed-service-identity.png)

1. İstediğiniz gibi mantıksal uygulama oluşturmaya devam edin.

<a name="remove-identity"></a>

## <a name="remove-managed-identity"></a>Yönetilen kimlik Kaldır

Mantıksal uygulamanız üzerinde yönetilen bir kimlik devre dışı bırakmak için Azure portalı, Azure Resource Manager dağıtım şablonlarını veya Azure PowerShell aracılığıyla kimlik nasıl oluşturduğunuz için benzer adımları izleyebilirsiniz. 

Mantıksal uygulamanızı silseniz bile Azure mantıksal uygulamanızın sistem tarafından atanan kimlik Azure AD'den otomatik olarak kaldırır.

### <a name="azure-portal"></a>Azure portal

1. Logic Apps Tasarımcısı'nda, mantıksal uygulamanızı açın.

1. Şu adımları uygulayın: 

   1. Mantıksal uygulama menüsünde altında **ayarları**seçin **iş akışı ayarları**. 
   
   1. Altında **yönetilen hizmet kimliği**, seçin **kapalı** için **Azure Active Directory ile kayıt** özelliği.

   1. İşiniz bittiğinde seçin **Kaydet** araç.

      ![Yönetilen kimlik ayarı devre dışı bırakmak](./media/create-managed-service-identity/turn-off-managed-service-identity.png)

### <a name="deployment-template"></a>Dağıtım şablonu

Mantıksal uygulamanın yönetilen kimlik ile bir Azure Resource Manager dağıtım şablonu oluşturduğunuz verilirse `"identity"` öğenin `"type"` özelliğini `"None"`. Bu eylem, sorumlu Kimliğini de Azure AD'den siler. 

```json
"identity": {
    "type": "None"
}
```

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](http://aka.ms/logicapps-wish) ziyaret edin.
