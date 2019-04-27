---
title: Azure Resource Manager şablonu örnekleri - Event Grid | Microsoft Docs
description: Event Grid için Azure Resource Manager şablonu örnekleri
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.date: 01/04/2019
ms.author: tomfitz
ms.openlocfilehash: 788d23c7bddd90c1e12a118742c651eb9759529c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60822696"
---
# <a name="azure-resource-manager-templates-for-event-grid"></a>Event Grid için Azure Resource Manager şablonları

JSON söz dizimi ve bir şablonunda kullanmak için özellikler için bkz: [Microsoft.EventGrid kaynak türleri](/azure/templates/microsoft.eventgrid/allversions). Aşağıdaki tablo, Event Grid için Azure Resource Manager şablonlarının bağlantılarını içerir.

| | |
|-|-|
|**Event Grid abonelikleri**||
| [Web Kancası uç noktası ile özel konu ve abonelik](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid)| Event Grid özel konusunu dağıtır. Web Kancası uç noktasını kullanan özel konuya yönelik bir abonelik oluşturur. |
| [EventHub uç noktası ile özel konu aboneliği](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-event-hubs-handler)| Bir özel konuya yönelik Event Grid aboneliği oluşturur. Abonelik, uç nokta için bir Olay Hub’ı kullanır. |
| [Azure aboneliği veya kaynak grubu aboneliği](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-resource-events-to-webhook)| Azure aboneliği veya bir kaynak grubu için olaylara abone olur. Dağıtım sırasında hedef olarak belirttiğiniz kaynak grubu, olayların kaynağıdır. Abonelik, uç nokta için bir Web Kancası kullanır. |
| [Blob depolama hesabı ve aboneliği](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-subscription-and-storage)| Bir Azure Blob depolama hesabı dağıtır ve o depolama hesabı için olaylara abone olur. |
| | |
