---
title: Azure kaynakları için yönetilen kimlikleri destekleyen azure Hizmetleri
description: Azure kaynaklarını ve Azure AD kimlik doğrulaması için yönetilen kimlikleri destekleyen hizmetlerin listesi
services: active-directory
author: daveba
ms.author: daveba
ms.date: 11/28/2018
ms.topic: conceptual
ms.service: active-directory
ms.component: msi
manager: daveba
ms.openlocfilehash: 7591860d36a3d6472411321c60c608411ab4eb8b
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54437683"
---
# <a name="services-that-support-managed-identities-for-azure-resources"></a>Azure kaynakları için yönetilen kimlikleri destekleyen hizmetler

Azure kaynakları için yönetilen kimlikleri Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Yönetilen kimlik kullanarak kodunuzda kimlik bilgileri olmadan Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmeti doğrulayabilir. Azure kaynaklarını ve Azure AD kimlik doğrulaması için tümleştirme yönetilen kimlikleri sürecinde Azure genelinde duyuyoruz. Kontrol güncelleştirmeleri için sık sık.

> [!NOTE]
> Azure kaynakları için yönetilen kimlikler, Yönetilen Hizmet Kimliği (MSI) olarak bilinen hizmetin yeni adıdır.

## <a name="azure-services-that-support-managed-identities-for-azure-resources"></a>Azure kaynakları için yönetilen kimlikleri destekleyen Azure hizmetleri

Aşağıdaki Azure Hizmetleri, Azure kaynakları için yönetilen kimlikleri destekler:

### <a name="azure-virtual-machines"></a>Azure Sanal Makineler

|Yönetilen kimlik türü |  Genel olarak kullanılabilir<br>Küresel Azure bölgeleri | Azure Kamu|Azure Almanya|Azure Çin 21Vianet|
| --- | --- | --- | --- | --- |
| Sistem tarafından atanmış | Kullanılabilir | Önizleme | Önizleme | Önizleme | Önizleme |
| Kullanıcı tarafından atanmış | Önizleme | Önizleme | Önizleme | Önizleme | Önizleme

Azure sanal makineler için yönetilen kimlik yapılandırmak için aşağıdaki listeye bakın (bölgede kullanılabiliyorsa):

- [Azure portal](qs-configure-portal-windows-vm.md)
- [PowerShell](qs-configure-powershell-windows-vm.md)
- [Azure CLI](qs-configure-cli-windows-vm.md)
- [Azure Resource Manager şablonları](qs-configure-template-windows-vm.md)
- [REST](qs-configure-rest-vm.md)

### <a name="azure-virtual-machine-scale-sets"></a>Azure sanal makine ölçek kümeleri

|Yönetilen kimlik türü |  Genel olarak kullanılabilir<br>Küresel Azure bölgeleri | Azure Kamu|Azure Almanya|Azure Çin 21Vianet|
| --- | --- | --- | --- | --- |
| Sistem tarafından atanmış | Kullanılabilir | Önizleme | Önizleme | Önizleme |
| Kullanıcı tarafından atanmış | Önizleme | Önizleme | Önizleme | Önizleme

Azure sanal makine ölçek kümeleri için yönetilen kimlik yapılandırmak için aşağıdaki listeye bakın (bölgede kullanılabiliyorsa):

- [Azure portal](qs-configure-portal-windows-vm.md)
- [PowerShell](qs-configure-powershell-windows-vm.md)
- [Azure CLI](qs-configure-cli-windows-vm.md)
- [Azure Resource Manager şablonları](qs-configure-template-windows-vm.md)
- [REST](qs-configure-rest-vm.md)

### <a name="azure-app-service"></a>Azure App Service

|Yönetilen kimlik türü |  Genel olarak kullanılabilir<br>Küresel Azure bölgeleri | Azure Kamu|Azure Almanya|Azure Çin 21Vianet|
| --- | --- | --- | --- | --- |
| Sistem tarafından atanmış | Kullanılabilir | Kullanılabilir | Kullanılabilir | Kullanılabilir |
| Kullanıcı tarafından atanmış | Önizleme | Kullanılamaz | Kullanılamaz | Kullanılamaz

Azure App Service için yönetilen kimlik yapılandırmak için aşağıdaki listeye bakın (bölgede kullanılabiliyorsa):

- [Azure portal](/azure/app-service/overview-managed-identity#using-the-azure-portal)
- [Azure CLI](/azure/app-service/overview-managed-identity#using-the-azure-cli)
- [Azure PowerShell](/azure/app-service/overview-managed-identity#using-azure-powershell)
- [Azure Resource Manager şablonu](/azure/app-service/overview-managed-identity#using-an-azure-resource-manager-template)

### <a name="azure-functions"></a>Azure İşlevleri

Yönetilen kimlik türü |  Genel olarak kullanılabilir<br>Küresel Azure bölgeleri | Azure Kamu|Azure Almanya|Azure Çin 21Vianet|
| --- | --- | --- | --- | --- |
| Sistem tarafından atanmış | Kullanılabilir | Kullanılabilir | Kullanılabilir | Kullanılabilir |
| Kullanıcı tarafından atanmış | Önizleme | Kullanılamaz | Kullanılamaz | Kullanılamaz

Azure işlevleri için yönetilen kimlik yapılandırmak için aşağıdaki listeye bakın (bölgede kullanılabiliyorsa):

- [Azure portal](/azure/app-service/overview-managed-identity#using-the-azure-portal)
- [Azure CLI](/azure/app-service/overview-managed-identity#using-the-azure-cli)
- [Azure PowerShell](/azure/app-service/overview-managed-identity#using-azure-powershell)
- [Azure Resource Manager şablonu](/azure/app-service/overview-managed-identity#using-an-azure-resource-manager-template)

### <a name="azure-logic-apps"></a>Azure Logic Apps

Yönetilen kimlik türü |  Genel olarak kullanılabilir<br>Küresel Azure bölgeleri | Azure Kamu|Azure Almanya|Azure Çin 21Vianet|
| --- | --- | --- | --- | --- |
| Sistem tarafından atanmış | Kullanılabilir | Kullanılabilir | Kullanılabilir | Kullanılabilir |
| Kullanıcı tarafından atanmış | Kullanılamaz | Kullanılamaz | Kullanılamaz | Kullanılamaz

Azure Logic Apps için yönetilen kimlik yapılandırmak için aşağıdaki listeye bakın (bölgede kullanılabiliyorsa):

- [Azure portal](/azure/logic-apps/create-managed-service-identity#azure-portal)
- [Azure Resource Manager şablonu](/azure/app-service/overview-managed-identity#deployment-template)

### <a name="azure-data-factory-v2"></a>Azure Data Factory V2

Yönetilen kimlik türü |  Genel olarak kullanılabilir<br>Küresel Azure bölgeleri | Azure Kamu|Azure Almanya|Azure Çin 21Vianet|
| --- | --- | --- | --- | --- |
| Sistem tarafından atanmış | Kullanılabilir | Kullanılamaz | Kullanılamaz | Kullanılamaz |
| Kullanıcı tarafından atanmış | Kullanılamaz | Kullanılamaz | Kullanılamaz | Kullanılamaz

Azure Data Factory V2 için yönetilen kimlik yapılandırmak için aşağıdaki listeye bakın (bölgede kullanılabiliyorsa):

- [Azure portal](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity)
- [PowerShell](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-powershell)
- [REST](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-rest-api)
- [SDK](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-sdk)

### <a name="azure-api-management"></a>Azure API Management

Yönetilen kimlik türü |  Genel olarak kullanılabilir<br>Küresel Azure bölgeleri | Azure Kamu|Azure Almanya|Azure Çin 21Vianet|
| --- | --- | --- | --- | --- |
| Sistem tarafından atanmış | Kullanılabilir | Kullanılabilir | Kullanılamaz | Kullanılamaz |
| Kullanıcı tarafından atanmış | Kullanılamaz | Kullanılamaz | Kullanılamaz | Kullanılamaz

Azure API Management için yönetilen kimlik yapılandırmak için aşağıdaki listeye bakın (bölgede kullanılabiliyorsa):

- [Azure Resource Manager şablonu](/azure/api-management/api-management-howto-use-managed-service-identity)

### <a name="azure-container-instances"></a>Azure Container Instances

Yönetilen kimlik türü |  Genel olarak kullanılabilir<br>Küresel Azure bölgeleri | Azure Kamu|Azure Almanya|Azure Çin 21Vianet|
| --- | --- | --- | --- | --- |
| Sistem tarafından atanmış | Linux: Önizleme<br>Windows: Kullanılamaz | Kullanılamaz | Kullanılamaz | Kullanılamaz |
| Kullanıcı tarafından atanmış | Linux: Önizleme<br>Windows: Kullanılamaz | Kullanılamaz | Kullanılamaz | Kullanılamaz

Azure Container Instances için yönetilen kimlik yapılandırmak için aşağıdaki listeye bakın (bölgede kullanılabiliyorsa):

- [Azure CLI](~/articles/container-instances/container-instances-managed-identity.md)
- [Azure Resource Manager şablonu](~/articles/container-instances/container-instances-managed-identity.md#enable-managed-identity-using-resource-manager-template)
- [YAML](~/articles/container-instances/container-instances-managed-identity.md#enable-managed-identity-using-yaml-file)


## <a name="azure-services-that-support-azure-ad-authentication"></a>Söz konusu destek Azure AD kimlik doğrulamasını Azure Hizmetleri

Aşağıdaki hizmetler, Azure AD kimlik doğrulamasını desteklemek ve Azure kaynakları için yönetilen kimlikleri kullanan istemci Hizmetleri ile test edilmiştir.

| Hizmet | Kaynak kimliği | Durum | Erişim atama |
| ------- | ----------- | ------ | ---- | ------------- |
| Azure Resource Manager | `https://management.azure.com/` | Kullanılabilir | [Azure portal](howto-assign-access-portal.md) <br>[PowerShell](howto-assign-access-powershell.md) <br>[Azure CLI](howto-assign-access-CLI.md) <br>[Azure Resource Manager şablonu](../../role-based-access-control/role-assignments-template.md) |
| Azure Key Vault | `https://vault.azure.net` | Kullanılabilir |  
| Azure Data Lake | `https://datalake.azure.net/` | Kullanılabilir |
| Azure SQL | `https://database.windows.net/` | Kullanılabilir |
| Azure Event Hubs | `https://eventhubs.azure.net` | Önizleme |
| Azure Service Bus | `https://servicebus.azure.net` | Önizleme |
| Azure Storage | `https://storage.azure.com/` | Önizleme |