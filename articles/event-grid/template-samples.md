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
ms.date: 03/27/2018
ms.author: tomfitz
ms.openlocfilehash: f4d6a663b4e0d2c2166028051e713668534b20bc
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
ms.locfileid: "30246279"
---
# <a name="azure-resource-manager-templates-for-event-grid"></a>Event Grid için Azure Resource Manager şablonları

Aşağıdaki tablo, Event Grid için Azure Resource Manager şablonlarının bağlantılarını içerir.

| | |
|-|-|
|**Event Grid abonelikleri**||
| [Web Kancası uç noktası ile özel konu ve abonelik](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid)| Event Grid özel konusunu dağıtır. Web Kancası uç noktasını kullanan özel konuya yönelik bir abonelik oluşturur. |
| [EventHub uç noktası ile özel konu aboneliği](https://github.com/Azure/azure-docs-json-samples/blob/master/event-grid/subscribeCustomTopicToEventHub.json)| Önceden var olan bir özel konuya yönelik Event Grid aboneliği oluşturur. Abonelik, uç nokta için bir Olay Hub’ı kullanır. |
| [Kaynak grubu aboneliği](https://github.com/Azure/azure-docs-json-samples/blob/master/event-grid/subscribeResourceGroupToWebHook.json)| Bir kaynak grubu için olaylara abone olur. Dağıtım sırasında hedef olarak belirttiğiniz kaynak grubu, olayların kaynağıdır. Abonelik, uç nokta için bir Olay Hub’ı kullanır. |
| [Blob depolama hesabı ve aboneliği](https://github.com/Azure/azure-docs-json-samples/blob/master/event-grid/createBlobAndSubscribe.json)| Bir Azure Blob depolama hesabı dağıtır ve o depolama hesabı için olaylara abone olur. |
| | |
