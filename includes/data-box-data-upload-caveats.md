---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 05/21/2019
ms.author: alkohli
ms.openlocfilehash: b159ec8620fa8c93e4917f73be9b9898e1b4fbcc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66244667"
---
- Dosyaları doğrudan precreated paylaşımları birine bulundurmanıza gerek yoktur. Paylaşım altında bir klasör oluşturun ve bu klasöre kopyalayın gerekir.
- Bir klasörü altında *StorageAccount_BlockBlob* ve *StorageAccount_PageBlob* bir kapsayıcıdır. Örneğin, kapsayıcıları olarak oluşturulur *StorageAccount_BlockBlob/kapsayıcı* ve *StorageAccount_PageBlob/kapsayıcı*.
- Doğrudan altında oluşturulan her klasörün *StorageAccount_AzureFiles* bir Azure dosya paylaşımına çevrilir.
- Data Box, kopyalanan nesne olarak aynı ada sahip bir bulutta (örneğin, bir bloba veya bir dosya) var olan bir Azure nesne varsa, bulutta dosyanın üzerine yazar.
- Her dosyanın içine yazılmış *StorageAccount_BlockBlob* ve *StorageAccount_PageBlob* paylaşımları yüklendiği bir blok blobu ve sayfa blobu olarak sırasıyla.
- Azure blob Depolama dizinleri desteklememektedir. Bir klasörü altında oluşturursanız *StorageAccount_BlockBlob* klasörünü ve ardından sanal klasörler blob adı oluşturulur. Azure dosyaları için gerçek dizin yapısı korunur.
- Herhangi bir boş (olmadan tüm dosyaları) dizin hiyerarşisi altında oluşturulan *StorageAccount_BlockBlob* ve *StorageAccount_PageBlob* değil klasörleri karşıya yüklendi.
- Herhangi bir hata varsa verileri Azure'a karşıya yüklenirken bir hata günlüğü hedef depolama hesabında oluşturulur. Bu hata günlük yolunu karşıya yükleme tamamlandıktan ve düzeltme eylemi için günlüğü gözden geçirebilirsiniz kullanılabilir. Veri kaynağından karşıya yüklenen veriler doğrulamadan silmeyin.
