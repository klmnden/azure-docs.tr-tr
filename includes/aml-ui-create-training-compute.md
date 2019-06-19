---
title: include dosyası
description: include dosyası
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 05/06/2019
ms.openlocfilehash: cf35651f7dd839e8792029851b9bfe278036624c
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188988"
---
Bir deney işlem hedefi, çalışma alanınıza bağlı bir işlem kaynağı çalışır.  İşlem hedefi oluşturduktan sonra sonraki çalıştırmalar için yeniden kullanabilirsiniz.

1. Seçin **çalıştırma** kısımdaki denemeyi çalıştırın.

     ![Denemeyi çalıştırma](./media/aml-ui-create-training-compute/run-experiment.png)

1. Zaman **Kurulum işlem hedefleri** iletişim kutusu açılır, çalışma alanınızı bir işlem kaynağı zaten varsa, seçebilirsiniz şimdi.  Aksi takdirde seçin **Yeni Oluştur**.

    > [!NOTE]
    > Görsel arabirim yalnızca Machine Learning işlem hedefler üzerinde denemeler çalıştırabilirsiniz. Diğer işlem hedefleri gösterilmez.

1. İşlem kaynağı için bir ad belirtin.

1. **Çalıştır**'ı seçin.

    ![Kurulum işlem hedefi](./media/aml-ui-create-training-compute/set-compute.png)

    İşlem kaynağı artık oluşturulur. Denemeyi sağ üst köşesinde bulunan durumunu görüntüleyin. 

    > [!NOTE]
    > Bir işlem kaynağı oluşturmak için yaklaşık 5 dakika sürer. Kaynak oluşturulduktan sonra yeniden kullanmak ve gelecekteki çalışmalarını bu bekleme süresi atlayın.
    >
    > Bilgi işlem maliyetinden tasarruf etmek boşta olduğunda 0 düğümlerini otomatik ölçeklendirme yapar.  Bir gecikmeden sonra yeniden kullandığınızda, yeniden eşitlenene kadar yeniden bekleme süresi yaklaşık 5 dakika karşılaşabilirsiniz.
