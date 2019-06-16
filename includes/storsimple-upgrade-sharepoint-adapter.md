---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: b2dec95e0258933b50d4437f1cb317639b62883d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66155826"
---
### <a name="upgrade-sharepoint-2010-to-sharepoint-2013-and-then-install-the-storsomple-adapter-for-sharepoint"></a>SharePoint 2010, SharePoint 2013'e yükseltin ve ardından StorSomple bağdaştırıcısı SharePoint için yükleyin
> [!IMPORTANT]
> Daha önce KKY dış depolama birimine taşınan dosyalar yükseltme tamamlandı ve KKY özelliği yeniden etkinleştirilene kadar kullanılamaz. Kullanıcı etkisini sınırlamak için herhangi bir yükseltme veya planlanan bir bakım penceresi sırasında yeniden gerçekleştirin.
> 
> 

#### <a name="to-upgrade-sharepoint-2010-to-sharepoint-2013-and-then-install-the-adapter"></a>SharePoint 2010, SharePoint 2013'e yükseltin ve ardından bağdaştırıcısı yüklemek için
1. SharePoint 2010 grupta te dış BLOB'ları ve KKY etkinleştirildiği içerik veritabanları için BLOB deposu yolu unutmayın. 
2. Yükleyin ve yeni SharePoint 2013 grubuna yapılandırın. 
3. Veritabanları, uygulamalar ve site koleksiyonları, SharePoint 2010 gruptan yeni SharePoint 2013 grubuna taşıyın. Yönergeler için Git [SharePoint 2013'e yükseltme işlemine genel bakış](https://technet.microsoft.com/library/cc262483.aspx).
4. Yeni gruptaki SharePoint için StorSimple bağdaştırıcısını yükleyin. Git [SharePoint için StorSimple bağdaştırıcısı](#install-the-storsimple-adapter-for-sharepoint) yordamlar.
5. 1\. adımda not ettiğiniz bilgileri kullanarak, içerik veritabanları için aynı kümesi KKY etkinleştirmek ve SharePoint 2010 yüklemesinde kullanılan BLOB deposu yolu sağlayın. Git [yapılandırma KKY](#configure-rbs) yordamlar. Bu adımı tamamladıktan sonra daha önce te dış dosyaları yeni grubundan erişilebilir olması gerekir. 

### <a name="upgrade-the-storsimple-adapter-for-sharepoint"></a>SharePoint için StorSimple bağdaştırıcısını yükseltme
> [!IMPORTANT]
> Bu yükseltme, aşağıdaki nedenlerle planlı bakım penceresi sırasında gerçekleşecek şekilde zamanlamanız gerekir:
> 
> * Daha önce te dış içerik bağdaştırıcısı yeniden kadar kullanılamaz.
> * SharePoint için StorSimple bağdaştırıcısını önceki sürümünü kaldırdıktan sonra ancak yeni sürümü yüklemeden önce siteye yüklenen herhangi bir içeriği içerik veritabanında depolanır. Yeni bağdaştırıcı yükledikten sonra bu içeriği StorSimple cihazına geçmeniz gerekecektir. Microsoft kullanabileceğiniz `RBS Migrate()` içeriği taşımak için SharePoint ile dahil edilen PowerShell cmdlet'i. Daha fazla bilgi için [içine veya dışına KKY İçerik Geçişi](https://technet.microsoft.com/library/ff628255.aspx). 
> 
> 

#### <a name="to-upgrade-the-storsimple-adapter-for-sharepoint"></a>SharePoint için StorSimple bağdaştırıcısını yükseltme
1. SharePoint için StorSimple Bağdaştırıcısı'nın önceki sürümünü kaldırın.
   
   > [!NOTE]
   > Bu otomatik olarak KKY içerik veritabanlarında devre dışı bırakır. Bununla birlikte, mevcut Blobları StorSimple cihazında kalır. KKY devre dışı bırakılır ve Bloblara içerik veritabanlarına taşınmamış çünkü bu Bloblar için tüm istekler başarısız olur. 
   > 
   > 
2. SharePoint için StorSimple bağdaştırıcısını yeni yükleyin. Yeni bağdaştırıcı, daha önce etkin veya devre dışı KKY için içerik veritabanları otomatik olarak algılar ve önceki ayarları kullanır.

