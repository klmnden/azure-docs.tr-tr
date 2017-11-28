---
title: "Azure geçirmek, bağımlılık Görselleştirme | Microsoft Docs"
description: "Değerlendirme hesaplamalar Azure geçirmek hizmetindeki genel bir bakış sağlar."
services: migrate
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 78e52157-edfd-4b09-923f-f0df0880e0e0
ms.service: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/22/2017
ms.author: raynew
ms.openlocfilehash: e787cd6746b53c28fc1495b9a3ac62f2aa08f639
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2017
---
# <a name="dependency-visualization"></a>Bağımlılık Görselleştirme

[Azure geçirmek](migrate-overview.md) Hizmetleri geçiş Azure için şirket içi makine grupları değerlendirir. Makineleri birlikte gruplamak için bağımlılık görselleştirme kullanabilirsiniz. Bu makalede, bu özellik hakkında bilgi sağlar.


## <a name="overview"></a>Genel Bakış

Azure geçirmek bağımlılık görsel öğeyi geçiş değerlendirmek için artan güvenle gruplar oluşturmanıza olanak sağlar. Bağımlılık görselleştirme kullanarak belirli makinelerin veya makine grubu arasında ağ bağımlılıkları görüntüleyebilirsiniz. Bu işlevselliği emin olmak kullanışlı veya kayıp (veya unutulursa makineler) uygulamaları ve iş yüklerini birden fazla makine arasında çalıştırdığınızda geçiş işleminin.  

## <a name="how-does-it-work"></a>Nasıl çalışır?

Azure geçirme kullanır [hizmet Haritası](../operations-management-suite/operations-management-suite-service-map.md) çözümde [günlük analizi](../log-analytics/log-analytics-overview.md) bağımlılık görselleştirme için.
- Bir Azure geçiş projesi oluşturduğunuzda, aboneliğinizde bir OMS günlük analizi çalışma alanı oluşturulur.
- Çalışma alanı adı önekine sahip geçiş proje için belirttiğiniz addır **geçirmek-**ve isteğe bağlı olarak sonekine olan bir sayı. 
- Günlük analizi çalışma alanından gidin **Essentials** proje bölümünü **genel bakış** sayfası.
- Oluşturulan çalışma anahtarla etiketli **MigrateProject**ve değeri **proje adı**. Azure portalında aramak için bunları kullanabilirsiniz.  

    ![Günlük analizi çalışma alanı](./media/concepts-dependency-visualization/oms-workspace.png)

Bağımlılık görselleştirme kullanmak için aracıları analiz etmek istediğiniz her şirket içi makinede yükleyip gerekir.  

## <a name="do-i-need-to-pay-for-it"></a>Bunun için ödeme gerekiyor mu?

Evet. Günlük analizi çalışma alanı varsayılan olarak oluşturulur, ancak Azure geçirmek bağımlılık görselleştirme kullanmadığınız sürece kullanılmaz. Bağımlılık görselleştirme kullanın (veya çalışma alanı Azure geçirme dışında kullanıyorsanız), çalışma alanı kullanımı için sizden ücret kesilir.  [Daha fazla bilgi edinin](https://www.microsoft.com/cloud-platform/operations-management-suite) hizmet Haritası çözüm fiyatlandırma hakkında. 

## <a name="how-do-i-manage-the-workspace"></a>Çalışma alanı nasıl yönetebilirim?

Azure geçirme dışında günlük analizi çalışma alanını kullanabilirsiniz. İçinde oluşturulduğu geçiş proje silerseniz silinmez. Çalışma alanı artık ihtiyacınız varsa [silmeden](../log-analytics/log-analytics-manage-access.md) el ile.

Geçiş proje silmediğiniz sürece Azure geçirmek tarafından oluşturulan bir çalışma alanı silmeyin. Bunu yaparsanız, bağımlılıkları beklendiği gibi çalışmıyor.

## <a name="next-steps"></a>Sonraki adımlar

[Grup makineler makine bağımlılıklarını kullanma](how-to-create-group-machine-dependencies.md)