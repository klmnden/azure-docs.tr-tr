---
title: "Portal üzerinden Azure Event Hubs Yakalama özelliğini etkinleştirme | Microsoft Docs"
description: "Azure portalını kullanarak Event Hubs Yakalama özelliğini etkinleştirin."
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/28/2017
ms.author: sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: 1f3d373944b909db290f6cf2da7bf12a8a00e1c5
ms.contentlocale: tr-tr
ms.lasthandoff: 06/28/2017


---

<a id="enable-event-hubs-capture-using-the-azure-portal" class="xliff"></a>

# Azure portalını kullanarak Event Hubs Yakalama özelliğini etkinleştirme

[Azure portalını](https://portal.azure.com) kullanarak olay hub'ı oluşturma sırasında Yakalama özelliğini yapılandırabilirsiniz. **Olay Hub'ı oluştur** portal dikey penceresindeki **Açık** düğmesine tıklayarak Yakalama özelliğini etkinleştirebilirsiniz. Dikey pencerenin **Kapsayıcı** bölümüne tıklayarak Depolama Hesabı ve kapsayıcı yapılandırabilirsiniz. Event Hubs Yakalama özelliği, depolama ile hizmetten hizmete kimlik doğrulama kullandığından depolama bağlantı dizesi belirtmenize gerek yoktur. Kaynak seçici, depolama hesabınız için kaynak URI'sini otomatik olarak seçer. Azure Resource Manager kullanıyorsanız bu URI'yi dize olarak açıkça belirtmeniz gerekir.

Zaman penceresi varsayılan olarak 5 dakikadır. En düşük değer 1, en yüksek değer ise 15'tir. **Boyut** penceresi 10-500 MB aralığındadır.

![][1]

<a id="adding-capture-to-an-existing-event-hub" class="xliff"></a>

## Mevcut bir olay hub'ına Yakalama özelliği ekleme

Yakalama özelliği, Event Hubs ad alanlarında mevcut olan olay hub'ları üzerinde yapılandırılabilir. Bu özellik daha eski olan **Mesajlaşma** veya **Karışık** türde ad alanları için kullanılamaz. Yakalama özelliğini mevcut bir olay hub'ında etkinleştirmek veya Yakalama ayarlarınızı değiştirmek için ad alanına tıklayarak **Temel Bileşenler** dikey penceresini yükleyin, ardından Yakalama ayarını etkinleştirmek veya değiştirmek istediğiniz olay hub'ına tıklayın. Son olarak, aşağıdaki şekilde gösterildiği gibi açık dikey pencerenin **Özellikler** bölümüne tıklayın:

![][2]

[1]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture1.png
[2]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture2.png

<a id="next-steps" class="xliff"></a>

## Sonraki adımlar

Dilerseniz Azure Resource Manager şablonlarını kullanarak da Event Hubs Yakalama özelliğini yapılandırabilirsiniz. Daha fazla bilgi için bkz. [Azure Resource Manager şablonu kullanarak Yakalama özelliğini etkinleştirme](event-hubs-resource-manager-namespace-event-hub-enable-capture.md).

