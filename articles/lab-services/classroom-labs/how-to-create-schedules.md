---
title: Azure Lab Services içinde sınıf Laboratuvarları için zamanlama oluşturma | Microsoft Docs
description: Sınıf laboratuvarlarını zamanlamalarını Azure Lab Services içinde oluşturabilir ve böylece labs'teki sanal makineleri başlatın ve belirli bir zamanda kapatma öğrenin.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2019
ms.author: spelluru
ms.openlocfilehash: 34bc8263053cd4a701c16ee1832cf1b27340a345
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60696039"
---
# <a name="create-and-manage-schedules-for-classroom-labs-in-azure-lab-services"></a>Oluşturma ve zamanlamalarını Azure Lab Services içinde sınıf laboratuvarlarını yönetme 
Zamanlamaları bir sınıf laboratuvarına Vm'leri Laboratuvardaki otomatik olarak başlatın ve belirli bir zamanda kapatma şekilde yapılandırmanıza olanak sağlar. Tek seferlik zamanlama veya yinelenen bir zamanlama tanımlayabilirsiniz. Aşağıdaki yordamlar oluşturmak ve yönetmek için bir sınıf laboratuvarına zamanlamaları için adımları şunları sağlar: 

> [!IMPORTANT]
> Vm'leri zamanlanmış çalışma süresi karşı sayılmaz [bir kullanıcı için ayrılan kota](how-to-configure-student-usage.md#set-quotas-per-user). Bir öğrenci Vm'lerde geçirdiği zamanlama saatleri dışında saat kota içindir. 

## <a name="add-a-schedule-once"></a>Bir zamanlama (kez) Ekle

1. Geçiş **zamanlamaları** sayfasında ve seçin **Ekle zamanlama** araç. 

    ![Zamanlama sayfasında zamanlama düğmesi ekleme](../media/how-to-create-schedules/add-schedule-button.png)
2. Üzerinde **Ekle zamanlama** sayfasında, onaylayın **kez** seçeneği en üstünde. Yüklü değilse, seçin **kez**. 
3. İçin **zamanlama (gerekli) tarih**, tarih girin veya bir tarih seçmek üzere takvim simgesini seçin. 
4. İçin **başlangıç zamanı**, başlatılacak Vm'leri istediğiniz zaman saati seçin. Başlangıç zamanı, bitiş zamanı ayarlanmamışsa gereklidir. Seçin **kaldırma olay başlangıç** durdurma saati belirtmek istiyorsanız. varsa **başlangıç zamanı** olduğundan devre dışı seçin **Ekle başlangıç olayı** etkinleştirmek için açılır listenin yanındaki. 
5. İçin **durdurma saati**, sanal makinelerin kapatılması için istediğiniz zaman saati seçin. Bitiş zamanı başlangıç zamanından ayarlanmamışsa gereklidir. Seçin **Kaldır durdurma olayını** yalnızca başlangıç saatini belirtmek istiyorsanız. varsa **durdurma saati** olduğundan devre dışı seçin **durdurma olay Ekle** etkinleştirmek için açılır listenin yanındaki.
6. İçin **saat dilimi (gerekli)**, başlangıç için saat dilimini seçin ve bitiş zamanlarını belirttiğiniz. 
7. İçin **notları**, zamanlama için herhangi bir açıklama veya notları girin. 
8. **Kaydet**’i seçin. 

    ![Tek seferlik zamanlama](../media/how-to-create-schedules/add-schedule-page.png)

## <a name="add-a-recurring-schedule-weekly"></a>Yinelenen bir zamanlama (haftalık) Ekle

1. Geçiş **zamanlamaları** sayfasında ve seçin **Ekle zamanlama** araç. 

    ![Zamanlama sayfasında zamanlama düğmesi ekleme](../media/how-to-create-schedules/add-schedule-button.png)
2. Üzerinde **Ekle zamanlama** sayfasında, geçiş **haftalık** en üstünde. 
3. İçin **zamanlama gün (gereklidir)**, zamanlama etkili olmasını istediğiniz günleri seçin. Aşağıdaki örnekte, Pazartesi-Cuma seçilir. 
4. İçin **gelen** alanına **zamanlama başlangıç tarihi** ya da bir tarih seçerek çekme **Takvim** düğmesi. Bu alan zorunludur. 
5. İçin **zamanlama bitiş tarihi**kapatma için VM'ler üzerinde olan bir bitiş tarihi seçin veya girin. 
6. İçin **başlangıç zamanı**, başlatılacak Vm'leri istediğiniz saati seçin. Başlangıç zamanı, bitiş zamanı ayarlanmamışsa gereklidir. Seçin **kaldırma olay başlangıç** durdurma saati belirtmek istiyorsanız. varsa **başlangıç zamanı** olduğundan devre dışı seçin **Ekle başlangıç olayı** etkinleştirmek için açılır listenin yanındaki. 
7. İçin **durdurma saati**, kapatma için Vm'leri istediğiniz saati seçin. Bitiş zamanı başlangıç zamanından ayarlanmamışsa gereklidir. Seçin **Kaldır durdurma olayını** yalnızca başlangıç saatini belirtmek istiyorsanız. varsa **durdurma saati** olduğundan devre dışı seçin **durdurma olay Ekle** etkinleştirmek için açılır listenin yanındaki.
8. İçin **saat dilimi (gerekli)**, başlangıç için saat dilimini seçin ve bitiş zamanlarını belirttiğiniz.  
9. İçin **notları**, zamanlama için herhangi bir açıklama veya notları girin. 
10. **Kaydet**’i seçin. 

    ![Haftalık Zamanlama](../media/how-to-create-schedules/add-schedule-page-weekly.png)

## <a name="view-schedules-in-calendar"></a>Takvimdeki zamanlamaları görüntüle
Zamanlanan tarih ve saatleri aşağıdaki görüntüde gösterildiği gibi bir takvim görünümünde vurgulanmış görebilirsiniz:

![Takvim görünümünde zamanlamaları](../media/how-to-create-schedules/schedules-in-calendar.png)

Seçin **Bugün** geçerli takvim tarihi geçmek için sağ üst köşedeki düğmesi. Seçin **sol ok** için önceki hafta geçmek ve **sağ ok** sonraki hafta içinde Takvim geçmek için. 

## <a name="edit-a-schedule"></a>Bir zamanlamayı Düzenle
Çift tıkladığınızda vurgulanan bir zamanlamaya göre Takvim ya da seçin **kalem** gördüğünüz araç çubuğunda düğme **zamanlamayı Düzenle** sayfası. Bu sayfadaki ayarlarını güncelleştirme işlemi aynı ayarları güncelleştirme olarak **Ekle zamanlama** sayfasında açıklandığı gibi [yinelenen bir zamanlama Ekle](#add-a-recurring-schedule-weekly) bölümü. 

![Zamanlama sayfayı Düzenle](../media/how-to-create-schedules/edit-schedule-page.png)

## <a name="delete-a-schedule"></a>Zamanlamayı silme

1. Bir zamanlama silmek için çöp seçin (aşağıdaki görüntüde gösterildiği gibi araç çubuğunda silebilirsiniz):

    ![Araç çubuğu düğmesi silme](../media/how-to-create-schedules/delete-schedule-button.png)

    Zamanlanan tarih ve saatler takvimde hiçbiri için Sil düğmesini kullanın ve seçin **Sil**. 
2. Üzerinde **zamanlamaları silmek** sayfasında **Evet**.

    ![Zamanlamalar onayını Sil](../media/how-to-create-schedules/delete-schedules-confirmation.png)




## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Bir yönetici olarak oluşturun ve Laboratuvar hesaplarını yönetme](how-to-manage-lab-accounts.md)
- [Laboratuvar sahibi olarak oluşturun ve Laboratuvarları yönetin](how-to-manage-classroom-labs.md)
- [Laboratuvar sahibi olarak yapılandırın ve Laboratuvar kullanımını denetleme](how-to-configure-student-usage.md)
- [Bir laboratuvar kullanıcı olarak sınıf laboratuvarlarına erişim](how-to-use-classroom-lab.md)
