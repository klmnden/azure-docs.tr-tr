---
title: Sorun giderme için Azure Data Box Disk disk kilidini açma sorunları | Microsoft Docs
description: Azure Data Box Disk sorunlarının nasıl giderileceği anlatılmaktadır.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 06/14/2019
ms.author: alkohli
ms.openlocfilehash: 02cbf64261bbfbf50561e1b7466b46b27b688e0a
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2019
ms.locfileid: "67148289"
---
# <a name="troubleshoot-disk-unlocking-issues-in-azure-data-box-disk"></a>Azure Data Box Disk Disk kilidini açma sorunları

Bu makale, Microsoft Azure Data Box Disk için geçerlidir ve kilidini aracı kullanırken sorunları gidermek için kullanılan iş akışları açıklanır. 


<!--## Query activity logs

Use the activity logs to find who unlocked and accessed the disks. Your Data Box Disk arrive on your premises in a locked state. You can use the device credentials available in the Azure portal for your order to unlock them.  

To figure out who accessed the **Device credentials** blade, you can query the Activity logs.  Any action that involves accessing **Device details > Credentials** blade is logged into the activity logs as `ListCredentials` action.

![Query Activity logs](media/data-box-logs/query-activity-log-1.png)-->


## <a name="data-box-disk-unlock-tool-errors"></a>Data Box Disk kilit açma aracı hataları


| Hata iletisi/Aracın davranışı      | Öneriler                                                                             |
|-------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------|
| Güncel .NET Framework desteklenmez. 4\.5 ve sonraki sürümler desteklenir.<br><br>Araç kapanıyor ve bir ileti açılıyor.  | .NET 4.5 yüklenmedi. Data Box Disk kilit açma aracının çalıştığı ana bilgisayara .NET 4.5 veya üzerini yükleyin.                                                                            |
| Birimlerin kilidi açılamadı veya birimler doğrulanamadı. Microsoft Desteği'ne başvurun.  <br><br>Araç kilitli sürücülerin kilidini açamıyor veya bu sürücüleri doğrulayamıyor. | Araç verilen destek anahtarıyla kilitlenen sürücülerin kilidini açamıyor. Sonraki adımlar için Microsoft Desteği'ne başvurun.                                                |
| Aşağıdaki birimlerin kilidi açıldı ve bu birimler doğrulandı. <br>Birimin sürücü harflerini: E:<br>Şu destek anahtarlarıyla birimlerin kilidi açılamadı: werwerqomnf, qwerwerqwdfda <br><br>Araç bazı sürücülerin kilidini açar, başarılı ve başarısız olan sürücü harflerini listeler.| Kısmen başarıldı. Kullanılan destek anahtarıyla bazı sürücülerin kilidi açılamadı. Sonraki adımlar için Microsoft Desteği'ne başvurun. |
| Kilitli birimler bulunamadı. Microsoft'tan alınan diskin düzgün bağlandığından ve kilitli durumda olduğundan emin olun.          | Araç kilitli sürücüleri bulamıyor. Sürücülerden birinin kilidi açılmış veya sürücü bulunamıyor. Sürücülerin bağlı ve kilitli olduğundan emin olun.                                                           |
| Önemli hata: Geçersiz parametre<br>Parametre adı: invalid_arg<br>KULLANIM:<br>DataBoxDiskUnlock /PassKeys:<noktalı_virgülle_ayrılmış_destek_anahtarı_listesi><br><br>Örnek: DataBoxDiskUnlock /PassKeys:passkey1;passkey2;passkey3<br>Örnek: DataBoxDiskUnlock /SystemCheck<br>Örnek: DataBoxDiskUnlock/Help<br><br>/ Parolalı:       Bu geçiş, Azure Data Box Disk Siparişiniz alın. Bu destek anahtarı disklerinizin kilidini açar.<br>/ Help:           Bu seçenek, cmdlet kullanım ve örnekleri Yardım sağlar.<br>/ SystemCheck:    Bu seçenek, sisteminizin aracı çalıştırmak için gereksinimleri karşılayıp karşılamadığını denetler.<br><br>Çıkmak için bir tuşa basın. | Geçersiz parametre girildi. Yalnızca izin verilen /SystemCheck /PassKey ve/Help parametrelerdir.|


## <a name="unlock-issues-for-disks-when-using-a-windows-client"></a>Windows İstemcisi kullanılırken diskler için sorunları kilidini aç

Bu bölümde Data Box Disk dağıtımı sırasında bir Windows istemci için veri kopyalama kullanırken karşılaşılan en önemli sorunlardan bazıları açıklanmaktadır.

### <a name="issue-could-not-unlock-drive-from-bitlocker"></a>Sorun: BitLocker sürücüsünden kilidi açılamadı
 
**Bunun nedeni** 

BitLocker'ı iletişim kutusunda veya kullanılan parolayı ve BitLocker'ı aracılığıyla disk kilidi açılmaya çalışılırken sürücüler iletişim kilidini açın. Bu çalışmaz.

**Çözümleme**

Veri kutusu disk kilidini açmak için veri kutusu Disk kilidini aracını kullanın ve Azure portalından parolayı girmeniz gerekir. Daha fazla bilgi için Git [Öğreticisi: Cihazınızı kutusundan çıkarma, bağlama ve Azure Data Box Disk kilidini](data-box-disk-deploy-set-up.md#connect-to-disks-and-get-the-passkey).
 
### <a name="issue-could-not-unlock-or-verify-some-volumes-contact-microsoft-support"></a>Sorun: Yüklenemedi kilidini veya bazı birimler doğrulayın. Microsoft Desteği'ne başvurun.
 
**Bunun nedeni**

Hata günlüğünde yer alan aşağıdaki hatayı görebilirsiniz ve kilidini ya da bazı birimler doğrulamak mümkün değildir.

`Exception System.IO.FileNotFoundException: Could not load file or assembly 'Microsoft.Management.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies. The system cannot find the file specified.`
 
Bu, büyük olasılıkla uygun Windows PowerShell sürümünü Windows istemciniz üzerinde eksik olduğunu gösterir.

**Çözümleme**

Yükleyebileceğiniz [Windows PowerShell v 5.0](https://www.microsoft.com/download/details.aspx?id=54616) ve işlemi yeniden deneyin.
 
Birimlerin kilidini açması hala kaldıramıyorsanız, günlükleri veri kutusu Disk kilidini aracın klasörüne kopyalayın ve [Microsoft Support başvurun](data-box-disk-contact-microsoft-support.md).

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [doğrulama sorunlarını giderme](data-box-disk-troubleshoot.md).
