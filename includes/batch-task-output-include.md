---
title: include dosyası
description: include dosyası
services: batch
author: dlepow
ms.service: batch
ms.topic: include
ms.date: 04/06/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 0dcb608553d4455403c073e34e83bccb81cc64db
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38750100"
---
Azure Batch hizmetinde çalışan bir görev, çalıştırıldığında çıkış verileri üretmek. Görev çıktı verileri genellikle alma işteki diğer görevler tarafından depolanmış olması gerekir, iş veya her ikisi de yürütülen istemci uygulaması. Batch işlem düğümü dosya sistemine çıkış veri yazma görevi, ancak zaman başlatıldığı ya da düğüm havuzu ayrıldığında düğümdeki tüm verileri kaybolur. Görevler, bir dosya Bekletme dönemi sonra görev tarafından oluşturulan dosyalar silinir da olabilir. Bu nedenlerden dolayı bir veri deposuna gibi daha sonra ihtiyacınız olacak görev çıktısını kalıcı hale getirmek önemli olan [Azure depolama](https://docs.microsoft.com/azure/storage/).

Batch’teki depolama hesabı seçenekleri için bkz. [Batch özelliğine genel bakış](../articles/batch/batch-api-basics.md#azure-storage-account).
