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
ms.openlocfilehash: 7cee3f534e4ab4973a87a9c373a5b83aa2ba9ce2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60549954"
---
Azure Batch hizmetinde çalışan bir görev, çalıştırıldığında çıkış verileri üretmek. Görev çıktı verileri genellikle alma işteki diğer görevler tarafından depolanmış olması gerekir, iş veya her ikisi de yürütülen istemci uygulaması. Batch işlem düğümü dosya sistemine çıkış veri yazma görevi, ancak zaman başlatıldığı ya da düğüm havuzu ayrıldığında düğümdeki tüm verileri kaybolur. Görevler, bir dosya Bekletme dönemi sonra görev tarafından oluşturulan dosyalar silinir da olabilir. Bu nedenlerden dolayı bir veri deposuna gibi daha sonra ihtiyacınız olacak görev çıktısını kalıcı hale getirmek önemli olan [Azure depolama](https://docs.microsoft.com/azure/storage/).

Batch’teki depolama hesabı seçenekleri için bkz. [Batch özelliğine genel bakış](../articles/batch/batch-api-basics.md#azure-storage-account).
