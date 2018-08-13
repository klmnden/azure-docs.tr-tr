---
title: Azure Data Box Disk sorunlarını giderme | Microsoft Docs
description: Azure Data Box Disk sorunlarının nasıl giderileceği anlatılmaktadır.
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: overview
ms.custom: mvc
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/03/2018
ms.author: alkohli
ms.openlocfilehash: e1a5cb33bb473daf5b9e45e7c64bcb297eca7733
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39595554"
---
# <a name="troubleshoot-issues-in-azure-data-box-disk-preview"></a>Azure Data Box Disk (Önizleme) sorunlarını giderme

Bu makale Microsoft Azure Data Box Önizleme sürümü için geçerlidir. Bu makalede Data Box ve Data Box Disk ile gerçekleştirilebilen bazı karmaşık iş akışları ve yönetim görevleri anlatılmaktadır. 

Data Box Disk'i Azure portaldan yönetebilirsiniz. Bu makale, Azure portalı kullanarak gerçekleştirebileceğiniz görevlere odaklanmaktadır. Azure portalı kullanarak siparişleri yönetebilir, cihazları yönetebilir ve siparişin durumunu tamamlanana kadar takip edebilirsiniz.

Bu makale aşağıdaki öğreticileri içerir:

- Tanılama günlüklerini indirme
- Etkinlik günlüklerini sorgulama


> [!IMPORTANT]
> Data Box önizleme aşamasındadır. Bu çözümü dağıtmadan önce [Önizleme için Azure hizmet şartlarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) gözden geçirin.

## <a name="download-diagnostic-logs"></a>Tanılama günlüklerini indirme

Veri kopyalama işlemi sırasında hata oluştursa portalda tanılama günlüklerinin bulunduğu klasörün yolu görüntülenir. 

Tanılama günlükleri şunlardan oluşabilir:
- Hata günlükleri
- Ayrıntılı günlükler  

Kopyalama günlüğünün yoluna gitmek için Data Box siparişinizle ilişkilendirilmiş olan depolama hesabına gidin. 

1.  **Genel > Sipariş ayrıntıları** sayfasına gidin ve siparişinizle ilişkilendirilmiş olan depolama hesabını not edin.
 

2.  **Tüm kaynaklar**'a gidin ve önceki adımda tanımlana depolama hesabını arayın. Depolama hesabını seçin ve üzerine tıklayın.

    ![Günlükleri kopyalama 1](./media/data-box-disk-troubleshoot/data-box-disk-copy-logs1.png)

3.  **Blob hizmeti > Blob'lara göz atın** sayfasına gidin ve depolama hesabına karşılık gelen blobu bulun. **diagnosticslogcontainer > waies** sayfasına gidin. 

    ![Günlükleri kopyalama 2](./media/data-box-disk-troubleshoot/data-box-disk-copy-logs2.png)

    Veri kopyalama işlemi için hem hata günlüklerini hem de ayrıntılı günlükleri görmeniz gerekir. Dosyaları seçip üzerine tıklayarak yerel kopyalarını indirin.

## <a name="query-activity-logs"></a>Etkinlik günlüklerini sorgulama

Sorun giderme sırasında bir hata bulmak veya kuruluşunuzdaki kullanıcının bir kaynağı nasıl değiştirdiğini izlemek için etkinlik günlüklerini kullanın. Etkinlik günlükleri ile aşağıdakileri belirleyebilirsiniz:

- Aboneliğinizdeki kaynaklarda gerçekleştirilen işlem.
- İşlemi başlatandır. 
- İşlemin ne zaman oluştuğu.
- İşlemin durumu.
- İşlemi araştırmanıza yardımcı olabilecek diğer özelliklerin değerleri.

Etkinlik günlüğü, kaynaklarınız üzerinde gerçekleştirilen tüm yazma işlemlerini (PUT, POST, DELETE gibi) içerir, ancak okuma işlemlerini (GET gibi) içermez. 

Etkinlik günlükleri 90 gün boyunca saklanır. Başlangıç tarihi 90 günden eski olmamak şartıyla istediğiniz tarih aralığını sorgulayabilirsiniz. Yerleşik Insights sorgularını kullanarak da filtreleme yapabilirsiniz. Örneğin hataya tıklayıp belirli alt hataları seçerek kök nedeni anlayabilirsiniz.

## <a name="data-box-disk-unlock-tool-errors"></a>Data Box Disk kilit açma aracı hataları


| Hata iletisi/Aracın davranışı      | Öneriler                                                                                               |
|-------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------|
| None<br><br>Data Box Disk kilit açma aracı kilitleniyor.                                                                            | Bitlocker yüklü değil. Data Box Disk kilit açma aracının çalıştığı ana bilgisayarda BitLocker uygulamasının yüklü olduğundan emin olun.                                                                            |
| Güncel .NET Framework desteklenmez. 4.5 ve sonraki sürümler desteklenir.<br><br>Araç kapanıyor ve bir ileti açılıyor.  | .NET 4.5 yüklenmedi. Data Box Disk kilit açma aracının çalıştığı ana bilgisayara .NET 4.5 veya üzerini yükleyin.                                                                            |
| Birimlerin kilidi açılamadı veya birimler doğrulanamadı. Microsoft Desteği'ne başvurun.  <br><br>Araç kilitli sürücülerin kilidini açamıyor veya bu sürücüleri doğrulayamıyor. | Araç verilen destek anahtarıyla kilitlenen sürücülerin kilidini açamıyor. Sonraki adımlar için Microsoft Desteği'ne başvurun.                                                |
| Aşağıdaki birimlerin kilidi açıldı ve bu birimler doğrulandı. <br>Birim sürücü harfleri: E:<br>Şu destek anahtarlarıyla birimlerin kilidi açılamadı: werwerqomnf, qwerwerqwdfda <br><br>Araç bazı sürücülerin kilidini açar, başarılı ve başarısız olan sürücü harflerini listeler.| Kısmen başarıldı. Kullanılan destek anahtarıyla bazı sürücülerin kilidi açılamadı. Sonraki adımlar için Microsoft Desteği'ne başvurun. |
| Kilitli birimler bulunamadı. Microsoft'tan alınan diskin düzgün bağlandığından ve kilitli durumda olduğundan emin olun.          | Araç kilitli sürücüleri bulamıyor. Sürücülerden birinin kilidi açılmış veya sürücü bulunamıyor. Sürücülerin bağlı ve kilitli olduğundan emin olun.                                                           |
| Önemli hata: Parametre geçersiz<br>Parametre adı: invalid_arg<br>KULLANIM:<br>DataBoxDiskUnlock /PassKeys:<noktalı_virgülle_ayrılmış_destek_anahtarı_listesi><br><br>Örnek: DataBoxDiskUnlock /PassKeys:passkey1;passkey2;passkey3<br>Örnek: DataBoxDiskUnlock /SystemCheck<br>Örnek: DataBoxDiskUnlock /Help<br><br>/PassKeys:       Bu destek anahtarını Azure DataBox Disk siparişinden alın. Bu destek anahtarı disklerinizin kilidini açar.<br>/Help:           Bu seçenek, cmdlet kullanımı konusunda yardım bilgileri ve örnekler sunar.<br>/SystemCheck:    Bu seçenek, sisteminizin aracı çalıştırmak için istenen gereksinimlere uygun olup olmadığını denetler.<br><br>Çıkmak için bir tuşa basın. | Geçersiz parametre girildi. İzin verilen parametreler: /SystemCheck, /PassKey ve /Help.                                                                            |
## <a name="next-steps"></a>Sonraki adımlar

- [Azure portal aracılığıyla Data Box Disk'i yönetme](data-box-portal-ui-admin.md) hakkında bilgi edinin.
