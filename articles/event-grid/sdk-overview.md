---
title: Azure Event kılavuz SDK'ları
description: SDK'ları için Azure olay kılavuz açıklar. Bu SDK, yönetim, yayımlama ve tüketimi sağlar.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: reference
ms.date: 05/04/2018
ms.author: tomfitz
ms.openlocfilehash: ebba28e72b3b27729318956bad07b6b6df3f14ac
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34823555"
---
# <a name="event-grid-sdks-for-management-and-publishing"></a>Yönetim ve yayımlama için olay kılavuz SDK'ları

Program aracılığıyla kaynaklarınızı yönetmek ve olayları sonrası olanak SDK olay kılavuz sağlar.

## <a name="management-sdks"></a>Yönetim SDK'ları

Yönetim SDK'ları oluşturmak, güncelleştirmek ve olay kılavuz konuları ve abonelikleri silme olanak tanır. Şu anda aşağıdaki SDK'ları bulunmaktadır:

* [.NET](https://www.nuget.org/packages/Microsoft.Azure.Management.EventGrid)
* [Go](https://github.com/Azure/azure-sdk-for-go)
* [Java](https://search.maven.org/#search%7Cga%7C1%7Cazure-mgmt-eventgrid)
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
