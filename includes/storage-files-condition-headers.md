---
title: include dosyası
description: include dosyası
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 05/17/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 0486b595bffd18b06d54e8377b24deab04e2aa93
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66159861"
---
## <a name="error-conditionheadersnotsupported-from-a-web-application-using-azure-files-from-browser"></a>Tarayıcıdan Azure dosyaları'nı kullanarak bir Web uygulamasında hata ConditionHeadersNotSupported

Bir web tarayıcısı gibi koşullu üst bilgi sağlayan bir uygulama ile Azure dosyaları ' barındırılan içeriğe erişmesini kullandığınızda, ConditionHeadersNotSupported hata görüntüleniyor erişim başarısız.

![ConditionHeaderNotSupported hata](media/storage-files-condition-headers/conditionalerror.png)

### <a name="cause"></a>Nedeni

Koşullu üstbilgiler henüz desteklenmemektedir. Uygulamaları uygularken dosyaya her erişildiğinde tam dosya istemeniz gerekir.

### <a name="workaround"></a>Geçici Çözüm

Yeni bir dosya yüklendiğinde, cache-control özellik varsayılan olarak "no-cache" olur. Dosya istek uygulamaya zorlamak için her zaman dosyanın cache-control özelliği "no-cache" "no-cache, no-store gerekir-revalidate için" güncelleştirilmesi gerekiyor. Bunu kullanarak gerçekleştirilebilir [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/).

![Depolama Gezgini içerik önbelleği değiştirme](media/storage-files-condition-headers/storage-explorer-cache.png)