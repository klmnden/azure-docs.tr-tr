---
title: Yönetilen hizmet kimliği destekleyen azure Hizmetleri
description: Yönetilen hizmet kimliği ve Azure AD kimlik doğrulama desteği hizmetlerin listesi
services: active-directory
author: daveba
ms.author: daveba
ms.date: 03/28/2018
ms.topic: reference
ms.service: active-directory
ms.component: msi
manager: mtillman
ms.openlocfilehash: 2d896b11ae94355eb2bdcfa8bc3a647f96fd8caf
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34723771"
---
# <a name="services-that-support-managed-service-identity"></a>Yönetilen hizmet kimliği Destek Hizmetleri 

Yönetilen hizmet kimliği Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Yönetilen bir kimliği'ni kullanarak Azure AD kimlik doğrulama kimlik bilgileri, kodunuzda gerek kalmadan destekleyen herhangi bir hizmeti doğrulayabilir. MSI ve Azure AD kimlik doğrulaması Azure arasında tümleştirme sürecinde duyuyoruz. Denetleme Güncelleştirmeleri için sık sık.

## <a name="azure-services-that-support-managed-service-identity"></a>Yönetilen hizmet kimliği destekleyen azure Hizmetleri

Aşağıdaki Azure hizmetlerini yönetilen hizmet kimliği destekler.

| Hizmet | Durum | Tarih | Yapılandırma | Belirteç alın |
| ------- | ------ | ---- | --------- | ----------- |
| Azure Sanal Makineler | Önizleme | Eylül 2017 | [Azure Portal](qs-configure-portal-windows-vm.md)<br>[PowerShell](qs-configure-powershell-windows-vm.md)<br>[Azure CLI](qs-configure-cli-windows-vm.md)<br>[Azure Resource Manager şablonları](qs-configure-template-windows-vm.md) | [REST](how-to-use-vm-token.md#get-a-token-using-http)<br>[.NET](how-to-use-vm-token.md#get-a-token-using-c)<br>[Bash/Curl](how-to-use-vm-token.md#get-a-token-using-curl)<br>[Go](how-to-use-vm-token.md#get-a-token-using-go)<br>[PowerShell](how-to-use-vm-token.md#get-a-token-using-azure-powershell) |
| Azure App Service | Önizleme | Eylül 2017 | [Azure Portal](/azure/app-service/app-service-managed-service-identity#using-the-azure-portal)<br>[Azure Resource Manager şablonu](/azure/app-service/app-service-managed-service-identity#using-an-azure-resource-manager-template) | [.NET](/azure/app-service/app-service-managed-service-identity#asal)<br>[REST](/azure/app-service/app-service-managed-service-identity#using-the-rest-protocol) |
| Azure İşlevleri | Önizleme | Eylül 2017 | [Azure Portal](/azure/app-service/app-service-managed-service-identity#using-the-azure-portal)<br>[Azure Resource Manager şablonu](/azure/app-service/app-service-managed-service-identity#using-an-azure-resource-manager-template) | [.NET](/azure/app-service/app-service-managed-service-identity#asal)<br>[REST](/azure/app-service/app-service-managed-service-identity#using-the-rest-protocol) |
| Azure Data Factory V2 | Önizleme | Kasım 2017 | [Azure Portal](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity)<br>[PowerShell](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-powershell)<br>[REST](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-rest-api)<br>[SDK](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-sdk) |


## <a name="azure-services-that-support-azure-ad-authentication"></a>Bu destek Azure AD kimlik doğrulaması Azure Hizmetleri

Aşağıdaki hizmetler Azure AD kimlik doğrulamayı desteklemek ve yönetilen hizmet kimliği kullanan istemci Hizmetleri ile test edilmiştir.

| Hizmet | Kaynak kimliği | Durum | Tarih | Erişimi atayın |
| ------- | ----------- | ------ | ---- | ------------- |
| Azure Resource Manager | https://management.azure.com/ | Kullanılabilir | Eylül 2017 | [Azure Portal](howto-assign-access-portal.md) <br>[PowerShell](howto-assign-access-powershell.md) <br>[Azure CLI](howto-assign-access-CLI.md) |
| Azure Key Vault | https://vault.azure.net | Kullanılabilir | Eylül 2017 | |
| Azure Data Lake | https://datalake.azure.net/ | Kullanılabilir | Eylül 2017 | |
| Azure SQL | https://database.windows.net/ | Kullanılabilir | Ekim 2017 | |
| Azure Event Hubs | https://eventhubs.azure.net | Kullanılabilir | Aralık 2017 | |
| Azure Service Bus | https://servicebus.azure.net | Kullanılabilir | Aralık 2017 | |
| Azure Storage | https://storage.azure.com/ | Önizleme | Mayıs 2018 | |
