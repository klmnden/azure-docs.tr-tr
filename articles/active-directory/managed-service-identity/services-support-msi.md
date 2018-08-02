---
title: Yönetilen hizmet kimliği destekleyen azure Hizmetleri
description: Yönetilen hizmet kimliği ve Azure AD kimlik doğrulamasını destekleyen hizmetlerin listesi
services: active-directory
author: daveba
ms.author: daveba
ms.date: 06/27/2018
ms.topic: conceptual
ms.service: active-directory
ms.component: msi
manager: mtillman
ms.openlocfilehash: 74fb9e784122dadd1ad2f6f29a497398eacf7464
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39412891"
---
# <a name="services-that-support-managed-service-identity"></a>Yönetilen hizmet kimliği destekleyen hizmetler 

Yönetilen hizmet kimliği Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Yönetilen kimlik kullanarak kodunuzda kimlik bilgileri olmadan Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmeti doğrulayabilir. Yönetilen hizmet kimliği ve Azure AD kimlik doğrulaması Azure'da tümleştirme sürecinde duyuyoruz. Kontrol güncelleştirmeleri için sık sık.

## <a name="azure-services-that-support-managed-service-identity"></a>Yönetilen hizmet kimliği destekleyen azure Hizmetleri

Aşağıdaki Azure Hizmetleri, yönetilen hizmet kimliği destekler.

| Hizmet | Sistem durumu atandı | Durum atanan kullanıcı| Yapılandırma | Bir belirteç Al |
| ------- | ------ | ---- | --------- | ----------- |
| Azure Sanal Makineler | Önizleme | Önizleme | [Azure portal](qs-configure-portal-windows-vm.md)<br>[PowerShell](qs-configure-powershell-windows-vm.md)<br>[Azure CLI](qs-configure-cli-windows-vm.md)<br>[Azure Resource Manager şablonları](qs-configure-template-windows-vm.md)<br>[REST](qs-configure-rest-vm.md) | [REST](how-to-use-vm-token.md#get-a-token-using-http)<br>[.NET](how-to-use-vm-token.md#get-a-token-using-c)<br>[Bash/Curl](how-to-use-vm-token.md#get-a-token-using-curl)<br>[Go](how-to-use-vm-token.md#get-a-token-using-go)<br>[PowerShell](how-to-use-vm-token.md#get-a-token-using-azure-powershell) |
| Sanal Makine Ölçek Kümeleri | Önizleme | Önizleme | [Azure Portal](qs-configure-portal-windows-vmss.md)<br>[PowerShell](qs-configure-powershell-windows-vmss.md)<br>[Azure CLI](qs-configure-cli-windows-vmss.md)<br>[Azure Resource Manager şablonları](qs-configure-template-windows-vmss.md)<br>[REST](qs-configure-rest-vmss.md) | [REST](how-to-use-vm-token.md#get-a-token-using-http)<br>[.NET](how-to-use-vm-token.md#get-a-token-using-c)<br>[Bash/Curl](how-to-use-vm-token.md#get-a-token-using-curl)<br>[Go](how-to-use-vm-token.md#get-a-token-using-go)<br>[PowerShell](how-to-use-vm-token.md#get-a-token-using-azure-powershell)
| Azure App Service | Kullanılabilir | Kullanılamaz | [Azure portal](/azure/app-service/app-service-managed-service-identity#using-the-azure-portal)<br>[Azure CLI](/azure/app-service/app-service-managed-service-identity#using-the-azure-cli)<br>[Azure PowerShell](/azure/app-service/app-service-managed-service-identity#using-azure-powershell)<br>[Azure Resource Manager şablonu](/azure/app-service/app-service-managed-service-identity#using-an-azure-resource-manager-template) | [REST](/azure/app-service/app-service-managed-service-identity#using-the-rest-protocol)<br>[.NET](/azure/app-service/app-service-managed-service-identity#asal)<br>[JavaScript](/azure/app-service/app-service-managed-service-identity#token-js)<br>[PowerShell](/azure/app-service/app-service-managed-service-identity#token-powershell)  |
| Azure İşlevleri | Kullanılabilir | Kullanılamaz | [Azure portal](/azure/app-service/app-service-managed-service-identity#using-the-azure-portal)<br>[Azure CLI](/azure/app-service/app-service-managed-service-identity#using-the-azure-cli)<br>[Azure PowerShell](/azure/app-service/app-service-managed-service-identity#using-azure-powershell)<br>[Azure Resource Manager şablonu](/azure/app-service/app-service-managed-service-identity#using-an-azure-resource-manager-template) | [REST](/azure/app-service/app-service-managed-service-identity#using-the-rest-protocol)<br>[.NET](/azure/app-service/app-service-managed-service-identity#asal)<br>[JavaScript](/azure/app-service/app-service-managed-service-identity#token-js)<br>[PowerShell](/azure/app-service/app-service-managed-service-identity#token-powershell) |
| Azure Data Factory V2 | Önizleme | Kullanılamaz | [Azure portal](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity)<br>[PowerShell](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-powershell)<br>[REST](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-rest-api)<br>[SDK](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-sdk) |
| Azure API Management | Önizleme | Kullanılamaz | [Azure Resource Manager şablonu](/azure/api-management/api-management-howto-use-managed-service-identity) | 


## <a name="azure-services-that-support-azure-ad-authentication"></a>Söz konusu destek Azure AD kimlik doğrulamasını Azure Hizmetleri

Aşağıdaki hizmetler, Azure AD kimlik doğrulamasını desteklemek ve yönetilen hizmet kimliği kullanan istemci Hizmetleri ile test edilmiştir.

| Hizmet | Kaynak kimliği | Durum | Tarih | Erişim atama |
| ------- | ----------- | ------ | ---- | ------------- |
| Azure Resource Manager | https://management.azure.com/ | Kullanılabilir | Eylül 2017 | [Azure portal](howto-assign-access-portal.md) <br>[PowerShell](howto-assign-access-powershell.md) <br>[Azure CLI](howto-assign-access-CLI.md) |
| Azure Key Vault | https://vault.azure.net | Kullanılabilir | Eylül 2017 | |
| Azure Data Lake | https://datalake.azure.net/ | Kullanılabilir | Eylül 2017 | |
| Azure SQL | https://database.windows.net/ | Kullanılabilir | Ekim 2017 | |
| Azure Event Hubs | https://eventhubs.azure.net | Kullanılabilir | Aralık 2017 | |
| Azure Service Bus | https://servicebus.azure.net | Kullanılabilir | Aralık 2017 | |
| Azure Storage | https://storage.azure.com/ | Önizleme | Mayıs 2018 | |
