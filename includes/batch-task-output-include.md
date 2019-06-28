---
title: include dosyası
description: include dosyası
services: batch
author: laurenhughes
ms.service: batch
ms.topic: include
ms.date: 04/06/2018
ms.author: lahugh
ms.custom: include file
ms.openlocfilehash: 7ba4c90811bd8051ed9c307d9f9fa33e08e69dc7
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188724"
---
Azure Batch hizmetinde çalışan bir görev, çalıştırıldığında çıkış verileri üretmek. Görev çıktı verileri genellikle alma işteki diğer görevler tarafından depolanmış olması gerekir, iş veya her ikisi de yürütülen istemci uygulaması. Batch işlem düğümü dosya sistemine çıkış veri yazma görevi, ancak zaman başlatıldığı ya da düğüm havuzu ayrıldığında düğümdeki tüm verileri kaybolur. Görevler, bir dosya Bekletme dönemi sonra görev tarafından oluşturulan dosyalar silinir da olabilir. Bu nedenlerden dolayı bir veri deposuna gibi daha sonra ihtiyacınız olacak görev çıktısını kalıcı hale getirmek önemli olan [Azure depolama](https://docs.microsoft.com/azure/storage/).

Batch’teki depolama hesabı seçenekleri için bkz. [Batch özelliğine genel bakış](../articles/batch/batch-api-basics.md#azure-storage-account).
