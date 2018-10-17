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
ms.date: 10/09/2018
ms.author: alkohli
ms.openlocfilehash: 9b2c03c13cf687af7cdebc9c4d2624a6a7c5907f
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "49069208"
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

## <a name="data-box-disk-split-copy-tool-errors"></a>Data Box Disk Split Copy aracı hataları

|Hata iletisi/Uyarılar  |Öneriler |
|---------|---------|
|[Bilgi] m: birimi için BitLocker parolası alınıyor <br>[Hata] m: birimi için BitLocker anahtarı alınırken özel durum oluştu<br> Sıra hiçbir öğe içermiyor.|Bu hata hedef Data Box Disk çevrimdışı olduğunda gösterilir. <br> Diskleri çevrimiçi duruma getirmek için `diskmgmt.msc` aracını kullanın.|
|[Hata] Özel durum oluştu: WMI işlemi başarısız oldu:<br> Method=UnlockWithNumericalPassword, ReturnValue=2150694965, <br>Win32Message=Girilen kurtarma parolası biçimi geçersiz. <br>BitLocker kurtarma parolaları 48 hanelidir. <br>Kurtarma parolasının doğru biçimde olduğunu doğrulayıp yeniden deneyin.|Data Box Disk kilit açma aracını kullanarak disklerin kilidini açın ve komutu yeniden deneyin. Daha fazla bilgi için bkz. <li> [Windows istemcileri için Data Box Disk kilidini açma](data-box-disk-deploy-set-up.md#unlock-disks-on-windows-client). </li><li> [Linux istemcileri için Data Box Disk kilidini açma](data-box-disk-deploy-set-up.md#unlock-disks-on-linux-client). </li>|
|[Hata] Özel durum oluştu: Hedef sürücüde DriveManifest.xml dosyası var. <br> Bu durum hedef sürücünün farklı bir günlük dosyasıyla hazırlanmış olabileceğini gösterir. <br>Aynı sürücüye daha fazla veri eklemek için önceki günlük dosyasını kullanın. Var olan verileri silmek ve hedef sürücüyü yeni bir içeri aktarma işi için kullanmak istiyorsanız sürücüdeki DriveManifest.xml dosyasını silin. Bu komutu yeni bir günlük dosyasıyla yeniden çalıştırın.| Bu hata aynı sürücü kümesini birden fazla içeri aktarma oturumunda kullanmaya çalıştığınızda ortaya çıkar. <br> Bir sürücü kümesini yalnızca bir bölme ve kopyalama oturumu için kullanın.|
|[Hata] Özel durum oluştu: CopySessionId importdata-sept-test-1 eski bir kopyalama oturumunu gösteriyor ve yeni bir kopyalama oturumu için yeniden kullanılamaz.|Bu hata yeni bir işe önceden başarıyla tamamlanan bir işin adının verilmeye çalışılması durumunda bildirilir.<br> Yeni işiniz için benzersiz bir ad atayın.|
|[Bilgi] Hedef dosya veya dizin adı NTFS uzunluk sınırını aşıyor. |Bu ileti hedef dosya uzun dosya yolu nedeniyle yeniden adlandırıldığında bildirilir.<br> Bu davranışı denetlemek için `config.json` dosyasındaki değerlendirme seçeneğini değiştirin.|
|[Hata] Özel durum oluştu: Bozuk JSON kaçış dizisi. |Bu ileti config.json dosyasında geçersiz biçim olduğunda bildirilir. <br> Dosyayı kaydetmeden önce [JSONlint](https://jsonlint.com/) kullanarak `config.json` için doğrulama gerçekleştirin.|



## <a name="next-steps"></a>Sonraki adımlar

- [Azure portal aracılığıyla Data Box Disk'i yönetme](data-box-portal-ui-admin.md) hakkında bilgi edinin.
