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
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
---
Azure toplu işlemde çalışan bir görev çalıştırıldığında çıktı veri üretebilir. Görev çıktı verileri genellikle alımını işteki diğer görevler tarafından depolanması gereken, iş ya da her ikisini de yürütülen istemci uygulaması. Görev çıktı verileri bir toplu işlem düğümünde dosya sistemine yazma, ancak ne zaman başlatıldığı ya da düğüm havuza ayrıldığında düğümdeki tüm verileri kaybolur. Görevler bir dosya Bekletme dönemi geçmesi görevi tarafından oluşturulan dosyalar silinir da sahip olabilirsiniz. Bu nedenlerle, bir veri deposuna gibi daha sonra ihtiyacınız olacak görev çıktısını kalıcı hale getirmek önemli olan [Azure Storage](https://docs.microsoft.com/azure/storage/).

Toplu depolama hesabı seçenekleri için bkz: [Batch özelliklerine genel bakış](../articles/batch/batch-api-basics.md#azure-storage-account).
