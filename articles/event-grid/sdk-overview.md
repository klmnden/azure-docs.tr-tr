---
title: Azure Event kılavuz SDK'ları
description: SDK'ları için Azure olay kılavuz açıklar. Bu SDK, yönetim, yayımlama ve tüketimi sağlar.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 05/04/2018
ms.author: tomfitz
ms.openlocfilehash: d9bb4b3b161060f20fca34760872a24cbfcabf30
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="event-grid-sdks-for-management-and-publishing"></a>Yönetim ve yayımlama için olay kılavuz SDK'ları

Program aracılığıyla kaynaklarınızı yönetmek ve olayları sonrası olanak SDK olay kılavuz sağlar.

## <a name="management-sdks"></a>Yönetim SDK'ları

Yönetim SDK'ları oluşturmak, güncelleştirmek ve olay kılavuz konuları ve abonelikleri silme olanak tanır. Şu anda aşağıdaki SDK'ları bulunmaktadır:

* [.NET](https://www.nuget.org/packages/Microsoft.Azure.Management.EventGrid)
* [Go](https://github.com/Azure/azure-sdk-for-go)
* [Java](https://mvnrepository.com/artifact/com.microsoft.azure.eventgrid-2018-01-01/azure-mgmt-eventgrid)
* [Node](https://www.npmjs.com/package/azure-arm-eventgrid)
* [Python](https://pypi.python.org/pypi/azure-mgmt-eventgrid)
* [Ruby](https://rubygems.org/gems/azure_mgmt_event_grid)

## <a name="data-plane-sdks"></a>Veri düzlemi SDK'ları

Veri düzlemi SDK'ları alma kimlik doğrulaması, olayı oluşturan ve zaman uyumsuz olarak belirtilen bitiş noktasına nakil verdiğiniz tarafından konulara olayları göndermek etkinleştirin. Bunlar ayrıca birinci taraf olayları kullanmasına olanak sağlar. Şu anda aşağıdaki SDK'ları bulunmaktadır:

* [.NET](https://www.nuget.org/packages/Microsoft.Azure.EventGrid)
* [Go](https://github.com/Azure/azure-sdk-for-go)
* [Java](https://mvnrepository.com/artifact/com.microsoft.azure/azure-eventgrid)
* [Node](https://www.npmjs.com/package/azure-eventgrid)
* [Python](https://pypi.python.org/pypi/azure-eventgrid)
* [Ruby](https://rubygems.org/gems/azure_event_grid)

## <a name="next-steps"></a>Sonraki adımlar

* Olay kılavuz giriş için bkz: [olay kılavuz nedir?](overview.md)
* Azure CLI olay kılavuz komutlar için bkz: [Azure CLI](/cli/azure/eventgrid).
* PowerShell'de olay kılavuz komutları için bkz: [PowerShell](/powershell/module/azurerm.eventgrid).