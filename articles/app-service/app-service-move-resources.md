---
title: "Web uygulama kaynakları başka bir kaynak grubuna taşıma"
description: "Burada, Web uygulamaları ve uygulama hizmetleri bir kaynak grubundan diğerine taşıyabilirsiniz senaryoları açıklanmıştır."
services: app-service
documentationcenter: 
author: ZainRizvi
manager: erikre
editor: 
ms.assetid: 22f97607-072e-4d1f-a46f-8d500420c33c
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/21/2016
ms.author: zarizvi
ms.openlocfilehash: 1b5059dc052005b6079f70ecf6771a3771df8d87
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="supported-move-configurations"></a>Desteklenen taşıma yapılandırmaları
Azure Web uygulaması kaynakları kullanarak taşıyabilirsiniz [Kaynak Yöneticisi taşıma kaynakları API](../azure-resource-manager/resource-group-move-resources.md).

Azure Web uygulamaları, şu anda aşağıdaki taşıma senaryoları destekler:

* Bir kaynak grubu (web uygulamaları, uygulama hizmeti planları ve sertifikaları) tüm içeriğini başka bir kaynak grubuna taşınır. 
   > [!Note]
   > Hedef kaynak grubu, bu senaryoda tüm Microsoft.Web kaynakları içeremez.

* Tek tek web uygulamaları, hala bunları (uygulama hizmeti planı eski kaynak grubunda kalır), geçerli uygulama hizmeti planında barındırma sırasında farklı bir kaynak grubuna taşıyın.


