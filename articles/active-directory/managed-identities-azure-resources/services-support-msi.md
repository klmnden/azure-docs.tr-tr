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
manager: mtillman
ms.openlocfilehash: 3fdbac019849bc97e8d336b75f26a8fe0a05c449
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53713123"
---
# <a name="services-that-support-managed-identities-for-azure-resources"></a>Azure kaynakları için yönetilen kimlikleri destekleyen hizmetler

Azure kaynakları için yönetilen kimlikleri Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Yönetilen kimlik kullanarak kodunuzda kimlik bilgileri olmadan Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmeti doğrulayabilir. Azure kaynaklarını ve Azure AD kimlik doğrulaması için tümleştirme yönetilen kimlikleri sürecinde Azure genelinde duyuyoruz. Kontrol güncelleştirmeleri için sık sık.

> [!NOTE]
> Azure kaynakları için yönetilen kimlikler, Yönetilen Hizmet Kimliği (MSI) olarak bilinen hizmetin yeni adıdır.

## <a name="azure-services-that-support-managed-identities-for-azure-resources"></a>Azure kaynakları için yönetilen kimlikleri destekleyen Azure hizmetleri

Aşağıdaki Azure Hizmetleri, Azure kaynakları için yönetilen kimlikleri destekler:

| Hizmet | Sistem tarafından atanan durumu | kullanıcı tarafından atanan durumu| Yapılandırma | Bir belirteç Al |
| ------- | ------ | ---- | --------- | ----------- |
| Azure Sanal Makineler | Kullanılabilir | Önizleme | [Azure portal](qs-configure-portal-windows-vm.md)<br>[PowerShell](qs-configure-powershell-windows-vm.md)<br>[Azure CLI](qs-configure-cli-windows-vm.md)<br>[Azure Resource Manager şablonları](qs-configure-template-windows-vm.md)<br>[REST](qs-configure-rest-vm.md) | [REST](how-to-use-vm-token.md#get-a-token-using-http)<br>[.NET](how-to-use-vm-token.md#get-a-token-using-c)<br>[Bash/Curl](how-to-use-vm-token.md#get-a-token-using-curl)<br>[Go](how-to-use-vm-token.md#get-a-token-using-go)<br>[PowerShell](how-to-use-vm-token.md#get-a-token-using-azure-powershell) |
| Sanal Makine Ölçek Kümeleri | Kullanılabilir | Önizleme | [Azure Portal](qs-configure-portal-windows-vmss.md)<br>[PowerShell](qs-configure-powershell-windows-vmss.md)<br>[Azure CLI](qs-configure-cli-windows-vmss.md)<br>[Azure Resource Manager şablonları](qs-configure-template-windows-vmss.md)<br>[REST](qs-configure-rest-vmss.md) | [REST](how-to-use-vm-token.md#get-a-token-using-http)<br>[.NET](how-to-use-vm-token.md#get-a-token-using-c)<br>[Bash/Curl](how-to-use-vm-token.md#get-a-token-using-curl)<br>[Go](how-to-use-vm-token.md#get-a-token-using-go)<br>[PowerShell](how-to-use-vm-token.md#get-a-token-using-azure-powershell)
| Azure App Service | Windows: Kullanılabilir <br> Linux: Önizleme | Önizleme | [Azure portal](/azure/app-service/overview-managed-identity#using-the-azure-portal)<br>[Azure CLI](/azure/app-service/overview-managed-identity#using-the-azure-cli)<br>[Azure PowerShell](/azure/app-service/overview-managed-identity#using-azure-powershell)<br>[Azure Resource Manager şablonu](/azure/app-service/overview-managed-identity#using-an-azure-resource-manager-template) | [REST](/azure/app-service/overview-managed-identity#using-the-rest-protocol)<br>[.NET](/azure/app-service/overview-managed-identity#asal)<br>[JavaScript](/azure/app-service/overview-managed-identity#token-js)<br>[PowerShell](/azure/app-service/overview-managed-identity#token-powershell)  |
| Azure İşlevleri | Kullanılabilir | Önizleme | [Azure portal](/azure/app-service/overview-managed-identity#using-the-azure-portal)<br>[Azure CLI](/azure/app-service/overview-managed-identity#using-the-azure-cli)<br>[Azure PowerShell](/azure/app-service/overview-managed-identity#using-azure-powershell)<br>[Azure Resource Manager şablonu](/azure/app-service/overview-managed-identity#using-an-azure-resource-manager-template) | [REST](/azure/app-service/overview-managed-identity#using-the-rest-protocol)<br>[.NET](/azure/app-service/overview-managed-identity#asal)<br>[JavaScript](/azure/app-service/overview-managed-identity#token-js)<br>[PowerShell](/azure/app-service/overview-managed-identity#token-powershell) |
| Azure Logic Apps | Kullanılabilir | Kullanılamaz | [Azure portal](/azure/logic-apps/create-managed-service-identity#azure-portal)<br>[Azure Resource Manager şablonu](/azure/app-service/overview-managed-identity#deployment-template) |  |
| Azure Data Factory V2 | Kullanılabilir | Kullanılamaz | [Azure portal](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity)<br>[PowerShell](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-powershell)<br>[REST](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-rest-api)<br>[SDK](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-sdk) |
| Azure API Management | Kullanılabilir | Kullanılamaz | [Azure Resource Manager şablonu](/azure/api-management/api-management-howto-use-managed-service-identity) |
| Azure Container Instances | Linux: Önizleme<br>Windows: Kullanılamaz | Linux: Önizleme<br>Windows: Kullanılamaz | [Azure CLI](~/articles/container-instances/container-instances-managed-identity.md)<br>[Azure Resource Manager şablonu](~/articles/container-instances/container-instances-managed-identity.md#enable-managed-identity-using-resource-manager-template)<br>[YAML](~/articles/container-instances/container-instances-managed-identity.md#enable-managed-identity-using-yaml-file) |  |


## <a name="azure-services-that-support-azure-ad-authentication"></a>Söz konusu destek Azure AD kimlik doğrulamasını Azure Hizmetleri

Aşağıdaki hizmetler, Azure AD kimlik doğrulamasını desteklemek ve Azure kaynakları için yönetilen kimlikleri kullanan istemci Hizmetleri ile test edilmiştir.

| Hizmet | Kaynak kimliği | Durum | Tarih | Erişim atama |
| ------- | ----------- | ------ | ---- | ------------- |
| Azure Resource Manager | `https://management.azure.com/` | Kullanılabilir | Eylül 2017 | [Azure portal](howto-assign-access-portal.md) <br>[PowerShell](howto-assign-access-powershell.md) <br>[Azure CLI](howto-assign-access-CLI.md) <br>[Azure Resource Manager şablonu](../../role-based-access-control/role-assignments-template.md) |
| Azure Key Vault | `https://vault.azure.net` | Kullanılabilir | Eylül 2017 | |
| Azure Data Lake | `https://datalake.azure.net/` | Kullanılabilir | Eylül 2017 | |
| Azure SQL | `https://database.windows.net/` | Kullanılabilir | Ekim 2017 | |
| Azure Event Hubs | `https://eventhubs.azure.net` | Önizleme | Aralık 2017 | |
| Azure Service Bus | `https://servicebus.azure.net` | Önizleme | Aralık 2017 | |
| Azure Storage | `https://storage.azure.com/` | Önizleme | Mayıs 2018 | |