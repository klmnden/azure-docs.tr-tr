---
title: Azure Event Grid SDK'ları
description: Azure Event Grid için SDK'ları açıklar. Bu SDK'ları, yönetim, yayımlama ve tüketimi sağlar.
services: event-grid
author: spelluru
manager: timlt
ms.service: event-grid
ms.topic: reference
ms.date: 01/19/2019
ms.author: spelluru
ms.openlocfilehash: 7f05665f4bcc5449c1a81fa24582b333b0a944e0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60822839"
---
# <a name="event-grid-sdks-for-management-and-publishing"></a>Event Grid SDK'lar yönetim ve yayımlama

Event Grid, program aracılığıyla kaynaklarınızı yönetmek ve olayları sağlayan SDK'lar sağlar.

## <a name="management-sdks"></a>Yönetim SDK'ları

Yönetim SDK'ları oluşturmak, güncelleştirmek ve event grid konuları ve abonelikleri silme etkinleştirin. Şu anda aşağıdaki Sdk'lardan bulunmaktadır:

* [.NET](https://www.nuget.org/packages/Microsoft.Azure.Management.EventGrid)
* [Go](https://github.com/Azure/azure-sdk-for-go)
* [Java](https://search.maven.org/#search%7Cga%7C1%7Cazure-mgmt-eventgrid)
* [Node](https://www.npmjs.com/package/azure-arm-eventgrid)
* [Python](https://pypi.python.org/pypi/azure-mgmt-eventgrid)
* [Ruby](https://rubygems.org/gems/azure_mgmt_event_grid)

## <a name="data-plane-sdks"></a>Veri düzlemi SDK'ları

Veri düzlemi SDK'ları alma kimlik doğrulaması, olay oluşturan ve zaman uyumsuz olarak belirtilen uç noktaya gönderme care tarafından konulara olayları göndermek etkinleştirin. Birinci taraf olayları kullanma da sağlıyor. Şu anda aşağıdaki Sdk'lardan bulunmaktadır:

* [.NET](https://www.nuget.org/packages/Microsoft.Azure.EventGrid)
* [Go](https://github.com/Azure/azure-sdk-for-go)
* [Java](https://mvnrepository.com/artifact/com.microsoft.azure/azure-eventgrid)
* [Node](https://www.npmjs.com/package/azure-eventgrid)
* [Python](https://pypi.python.org/pypi/azure-eventgrid)
* [Ruby](https://rubygems.org/gems/azure_event_grid)

## <a name="next-steps"></a>Sonraki adımlar

* Örneğin bkz. uygulamalar, [Event Grid kod örnekleri](https://azure.microsoft.com/resources/samples/?sort=0&service=event-grid).
* Event grid'e giriş için bkz [Event Grid nedir?](overview.md)
* Azure CLI, Event Grid komutlar için bkz [Azure CLI](/cli/azure/eventgrid).
* PowerShell'de Event Grid komutlar için bkz [PowerShell](/powershell/module/az.eventgrid).
