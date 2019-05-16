---
title: Azure kaynakları için yönetilen kimlikleri destekleyen azure Hizmetleri
description: Azure kaynaklarını ve Azure AD kimlik doğrulaması için yönetilen kimlikleri destekleyen hizmetlerin listesi
services: active-directory
author: MarkusVi
ms.author: priyamo
ms.date: 05/09/2019
ms.topic: conceptual
ms.service: active-directory
ms.subservice: msi
manager: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0809423472bde0240145d43f4c28d9594b517928
ms.sourcegitcommit: 17411cbf03c3fa3602e624e641099196769d718b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65518683"
---
# <a name="services-that-support-managed-identities-for-azure-resources"></a>Azure kaynakları için yönetilen kimlikleri destekleyen hizmetler

Azure kaynakları için yönetilen kimlikleri Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Yönetilen kimlik kullanarak kodunuzda kimlik bilgileri olmadan Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmeti doğrulayabilir. Azure kaynaklarını ve Azure AD kimlik doğrulaması için tümleştirme yönetilen kimlikleri sürecinde Azure genelinde duyuyoruz. Kontrol güncelleştirmeleri için sık sık.

> [!NOTE]
> Azure kaynakları için yönetilen kimlikler, Yönetilen Hizmet Kimliği (MSI) olarak bilinen hizmetin yeni adıdır.

## <a name="azure-services-that-support-managed-identities-for-azure-resources"></a>Azure kaynakları için yönetilen kimlikleri destekleyen Azure hizmetleri

Aşağıdaki Azure Hizmetleri, Azure kaynakları için yönetilen kimlikleri destekler:

### <a name="azure-virtual-machines"></a>Azure Sanal Makineler

| Yönetilen kimlik türü | Genel olarak kullanılabilir<br>Küresel Azure bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | --- | --- | --- | --- |
| Sistem tarafından atanmış | Kullanılabilir | Preview | Preview | Preview | 
| Kullanıcı tarafından atanmış | Preview | Preview | Preview | Preview |

Azure sanal makineler için yönetilen kimlik yapılandırmak için aşağıdaki listeye bakın (bölgede kullanılabiliyorsa):

- [Azure portal](qs-configure-portal-windows-vm.md)
- [PowerShell](qs-configure-powershell-windows-vm.md)
- [Azure CLI](qs-configure-cli-windows-vm.md)
- [Azure Resource Manager şablonları](qs-configure-template-windows-vm.md)
- [REST](qs-configure-rest-vm.md)

### <a name="azure-virtual-machine-scale-sets"></a>Azure sanal makine ölçek kümeleri

|Yönetilen kimlik türü | Genel olarak kullanılabilir<br>Küresel Azure bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | --- | --- | --- | --- |
| Sistem tarafından atanmış | Kullanılabilir | Preview | Preview | Preview |
| Kullanıcı tarafından atanmış | Preview | Preview | Preview | Preview |

Azure sanal makine ölçek kümeleri için yönetilen kimlik yapılandırmak için aşağıdaki listeye bakın (bölgede kullanılabiliyorsa):

- [Azure portal](qs-configure-portal-windows-vm.md)
- [PowerShell](qs-configure-powershell-windows-vm.md)
- [Azure CLI](qs-configure-cli-windows-vm.md)
- [Azure Resource Manager şablonları](qs-configure-template-windows-vm.md)
- [REST](qs-configure-rest-vm.md)

### <a name="azure-app-service"></a>Azure App Service

| Yönetilen kimlik türü | Genel olarak kullanılabilir<br>Küresel Azure bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | --- | --- | --- | --- |
| Sistem tarafından atanmış | Kullanılabilir | Kullanılabilir | Kullanılabilir | Kullanılabilir |
| Kullanıcı tarafından atanmış | Preview | Yok | Yok | Yok |

Azure App Service için yönetilen kimlik yapılandırmak için aşağıdaki listeye bakın (bölgede kullanılabiliyorsa):

- [Azure portal](/azure/app-service/overview-managed-identity#using-the-azure-portal)
- [Azure CLI](/azure/app-service/overview-managed-identity#using-the-azure-cli)
- [Azure PowerShell](/azure/app-service/overview-managed-identity#using-azure-powershell)
- [Azure Resource Manager şablonu](/azure/app-service/overview-managed-identity#using-an-azure-resource-manager-template)

### <a name="azure-blueprints"></a>Azure Blueprints

|Yönetilen kimlik türü | Genel olarak kullanılabilir<br>Küresel Azure bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | --- | --- | --- | --- |
| Sistem tarafından atanmış | Preview | Yok | Yok | Yok |
| Kullanıcı tarafından atanmış | Preview | Yok | Yok | Yok |

Yönetilen bir kimlik ile kullanmak için aşağıdaki listeye bakın [Azure şemaları](../../governance/blueprints/overview.md):

- [Azure portalı - şema atamasını](../../governance/blueprints/create-blueprint-portal.md#assign-a-blueprint)
- [REST API - şema atamasını](../../governance/blueprints/create-blueprint-rest-api.md#assign-a-blueprint)

### <a name="azure-functions"></a>Azure İşlevleri

Yönetilen kimlik türü |Genel olarak kullanılabilir<br>Küresel Azure bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | --- | --- | --- | --- |
| Sistem tarafından atanmış | Kullanılabilir | Kullanılabilir | Kullanılabilir | Kullanılabilir |
| Kullanıcı tarafından atanmış | Preview | Yok | Yok | Yok |

Azure işlevleri için yönetilen kimlik yapılandırmak için aşağıdaki listeye bakın (bölgede kullanılabiliyorsa):

- [Azure portal](/azure/app-service/overview-managed-identity#using-the-azure-portal)
- [Azure CLI](/azure/app-service/overview-managed-identity#using-the-azure-cli)
- [Azure PowerShell](/azure/app-service/overview-managed-identity#using-azure-powershell)
- [Azure Resource Manager şablonu](/azure/app-service/overview-managed-identity#using-an-azure-resource-manager-template)

### <a name="azure-logic-apps"></a>Azure Logic Apps

Yönetilen kimlik türü | Genel olarak kullanılabilir<br>Küresel Azure bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | --- | --- | --- | --- |
| Sistem tarafından atanmış | Preview | Preview | Yok | Preview |
| Kullanıcı tarafından atanmış | Yok | Yok | Yok | Yok |

Azure Logic Apps için yönetilen kimlik yapılandırmak için aşağıdaki listeye bakın (bölgede kullanılabiliyorsa):

- [Azure portal](/azure/logic-apps/create-managed-service-identity#azure-portal)
- [Azure Resource Manager şablonu](/azure/app-service/overview-managed-identity)

### <a name="azure-data-factory-v2"></a>Azure Data Factory V2

Yönetilen kimlik türü | Genel olarak kullanılabilir<br>Küresel Azure bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | --- | --- | --- | --- |
| Sistem tarafından atanmış | Kullanılabilir | Yok | Yok | Yok |
| Kullanıcı tarafından atanmış | Yok | Yok | Yok | Yok |

Azure Data Factory V2 için yönetilen kimlik yapılandırmak için aşağıdaki listeye bakın (bölgede kullanılabiliyorsa):

- [Azure portal](~/articles/data-factory/data-factory-service-identity.md#generate-managed-identity)
- [PowerShell](~/articles/data-factory/data-factory-service-identity.md#generate-managed-identity-using-powershell)
- [REST](~/articles/data-factory/data-factory-service-identity.md#generate-managed-identity-using-rest-api)
- [SDK](~/articles/data-factory/data-factory-service-identity.md#generate-managed-identity-using-sdk)

### <a name="azure-api-management"></a>Azure API Yönetimi

Yönetilen kimlik türü | Genel olarak kullanılabilir<br>Küresel Azure bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | --- | --- | --- | --- |
| Sistem tarafından atanmış | Kullanılabilir | Kullanılabilir | Yok | Yok |
| Kullanıcı tarafından atanmış | Yok | Yok | Yok | Yok |

Azure API Management için yönetilen kimlik yapılandırmak için aşağıdaki listeye bakın (bölgede kullanılabiliyorsa):

- [Azure Resource Manager şablonu](/azure/api-management/api-management-howto-use-managed-service-identity)

### <a name="azure-container-instances"></a>Azure Container Instances

Yönetilen kimlik türü | Genel olarak kullanılabilir<br>Küresel Azure bölgeleri | Azure Kamu | Azure Almanya | Azure Çin 21Vianet |
| --- | --- | --- | --- | --- |
| Sistem tarafından atanmış | Linux: Preview<br>Windows: Yok | Yok | Yok | Yok |
| Kullanıcı tarafından atanmış | Linux: Preview<br>Windows: Yok | Yok | Yok | Yok |

Azure Container Instances için yönetilen kimlik yapılandırmak için aşağıdaki listeye bakın (bölgede kullanılabiliyorsa):

- [Azure CLI](~/articles/container-instances/container-instances-managed-identity.md)
- [Azure Resource Manager şablonu](~/articles/container-instances/container-instances-managed-identity.md#enable-managed-identity-using-resource-manager-template)
- [YAML](~/articles/container-instances/container-instances-managed-identity.md#enable-managed-identity-using-yaml-file)

## <a name="azure-services-that-support-azure-ad-authentication"></a>Söz konusu destek Azure AD kimlik doğrulamasını Azure Hizmetleri

Aşağıdaki hizmetler, Azure AD kimlik doğrulamasını desteklemek ve Azure kaynakları için yönetilen kimlikleri kullanan istemci Hizmetleri ile test edilmiştir.

### <a name="azure-resource-manager"></a>Azure Resource Manager

Azure Resource Manager'a erişimi yapılandırmak için aşağıdaki listeye bakın:

- [Azure portal aracılığıyla erişim atama](howto-assign-access-portal.md)
- [Powershell aracılığıyla erişim atama](howto-assign-access-powershell.md)
- [Azure CLI aracılığıyla erişim atama](howto-assign-access-CLI.md)
- [Azure Resource Manager şablonu aracılığıyla erişim atama](../../role-based-access-control/role-assignments-template.md)

| Bulut | Kaynak Kimliği | Durum |
|--------|------------|--------|
| Azure genel | `https://management.azure.com/`| Kullanılabilir |
| Azure Kamu | `https://management.usgovcloudapi.net/` | Kullanılabilir |
| Azure Almanya | `https://management.microsoftazure.de/` | Kullanılabilir |
| Azure Çin 21Vianet | `https://management.chinacloudapi.cn` | Kullanılabilir |

### <a name="azure-key-vault"></a>Azure Key Vault

| Bulut | Kaynak Kimliği | Durum |
|--------|------------|--------|
| Azure genel | `https://vault.azure.net`| Kullanılabilir |
| Azure Kamu | `https://vault.usgovcloudapi.net` | Kullanılabilir |
| Azure Almanya |  `https://vault.microsoftazure.de` | Kullanılabilir |
| Azure Çin 21Vianet | `https://vault.azure.cn` | Kullanılabilir |

### <a name="azure-data-lake"></a>Azure Data Lake 

| Bulut | Kaynak Kimliği | Durum |
|--------|------------|--------|
| Azure genel | `https://datalake.azure.net/` | Kullanılabilir |
| Azure Kamu |  | Kullanılamaz |
| Azure Almanya |   | Kullanılamaz |
| Azure Çin 21Vianet |  | Kullanılamaz |

### <a name="azure-sql"></a>Azure SQL 

| Bulut | Kaynak Kimliği | Durum |
|--------|------------|--------|
| Azure genel | `https://database.windows.net/` | Kullanılabilir |
| Azure Kamu | `https://database.usgovcloudapi.net/` | Kullanılabilir |
| Azure Almanya | `https://database.cloudapi.de/` | Kullanılabilir |
| Azure Çin 21Vianet | `https://database.chinacloudapi.cn/` | Kullanılabilir |

### <a name="azure-event-hubs"></a>Azure Event Hubs

| Bulut | Kaynak Kimliği | Durum |
|--------|------------|--------|
| Azure genel | `https://eventhubs.azure.net` | Preview |
| Azure Kamu |  | Kullanılamaz |
| Azure Almanya |   | Kullanılamaz |
| Azure Çin 21Vianet |  | Kullanılamaz |

### <a name="azure-service-bus"></a>Azure Service Bus

| Bulut | Kaynak Kimliği | Durum |
|--------|------------|--------|
| Azure genel | `https://servicebus.azure.net`  | Preview |
| Azure Kamu |  | Kullanılamaz |
| Azure Almanya |   | Kullanılamaz |
| Azure Çin 21Vianet |  | Kullanılamaz |

### <a name="azure-storage-blobs-and-queues"></a>Azure depolama BLOB'ları ve Kuyruklar

| Bulut | Kaynak Kimliği | Durum |
|--------|------------|--------|
| Azure genel | `https://storage.azure.com/` | Kullanılabilir |
| Azure Kamu | `https://storage.azure.com/` | Kullanılabilir |
| Azure Almanya | `https://storage.azure.com/` | Kullanılabilir |
| Azure Çin 21Vianet | `https://storage.azure.com/` | Kullanılabilir |

### <a name="azure-analysis-services"></a>Azure Analysis Services

| Bulut | Kaynak Kimliği | Durum |
|--------|------------|--------|
| Azure genel | `https://*.asazure.windows.net` | Kullanılabilir |
| Azure Kamu | `https://*.asazure.usgovcloudapi.net` | Kullanılabilir |
| Azure Almanya | `https://*.asazure.cloudapi.de` | Kullanılabilir |
| Azure Çin 21Vianet | `https://*.asazure.chinacloudapi.cn` | Kullanılabilir |