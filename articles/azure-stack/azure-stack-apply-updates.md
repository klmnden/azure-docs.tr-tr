---
title: Azure stack'teki güncelleştirmeleri uygulamak | Microsoft Docs
description: İçeri aktarma ve Azure Stack tümleşik sistemi için Microsoft güncelleştirme paketlerini yükleme hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2019
ms.author: mabrigg
ms.reviewer: wfayed
ms.lastreviewed: 01/18/2019
ms.openlocfilehash: 585fc4f1bbddb08d881414b581120b7bc14232ab
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2019
ms.locfileid: "55729957"
---
# <a name="apply-updates-in-azure-stack"></a>Azure stack'teki güncelleştirmelerini uygulayın

*Uygulama hedefi: Azure Stack tümleşik sistemleri*

Kullanabileceğiniz **güncelleştirme** kutucuğunda Azure Stack için Microsoft veya OEM güncelleştirme paketleri uygulamak Yönetim Portalı'nda. Güncelleştirme paketinin indirilebilmesi Azure Stack'e paket dosyalarını içeri ve ardından güncelleştirme paketini yüklemeniz gerekir.

## <a name="download-the-update-package"></a>Güncelleştirme paketini indirme

Azure Stack için bir Microsoft veya OEM güncelleştirme paketi kullanılabilir olduğunda, Azure yığını tarafından erişilebilen bir konuma paketi indirebilir ve paket içeriğini gözden geçirin. Bir güncelleştirme paketi genellikle aşağıdaki dosyalardan oluşur:

- Bir kendiliğinden `<PackageName>.exe` dosya. Bu dosya, güncelleştirme, örneğin en son toplu güncelleştirmeyi için Windows Server için yükü içerir.

- Karşılık gelen `<PackageName>.bin` dosyaları. Bu dosyalar ilişkili olduğu yükü için sıkıştırma sağlar *PackageName*.exe dosyası.

- A `Metadata.xml` dosya. Bu dosya, güncelleştirme, örneğin Yayımcı adı, önkoşul, boyutu ve Destek yol URL'si hakkında gerekli bilgileri içerir.

> [!IMPORTANT]  
> Azure Stack 1901 güncelleştirme paketi uyguladıktan sonra Azure Stack güncelleştirme pacakges paketleme biçimini .exe, .bin(s) ve bir .zip(s) .xml biçiminde ve .xml biçiminde taşınır. Damgalar bağlı azure Stack operatörleri etkilenmiş olmaz. Aşağıda açıklanan sürecin aynısını kullanarak bağlı azure Stack operatörleri yalnızca .xml ve .zip dosyasını/dosyalarını aktarın.

## <a name="import-and-install-updates"></a>İçeri aktarmak ve güncelleştirmeleri yükleme

Aşağıdaki yordam, içeri aktarma ve Yönetici portalı'nda güncelleştirme paketlerini yükleme gösterir.

> [!IMPORTANT]  
> Herhangi bir bakım işlemi kullanıcılara bildirmek ve çalışma saatleri sırasında mümkün olduğunca normal bakım pencereleri zamanlamanızı öneririz. Bakım işlemleri, kullanıcı iş yükleri hem portal işlemlerini etkileyebilir.

1. Yönetici portalında **tüm hizmetleri**. Ardından, altında **veri + depolama** kategorisi, select **depolama hesapları**. (Veya filtre kutusuna yazmaya başlayın **depolama hesapları**ve bu seçeneği belirleyin.)

    ![Depolama hesaplarını portalda nerede bulacağını gösterir](media/azure-stack-apply-updates/ApplyUpdates1.png)

2. Filtre kutusuna **güncelleştirme**seçip **updateadminaccount** depolama hesabı.

3. Depolama hesabı ayrıntıları altında **Hizmetleri**seçin **Blobları**.
 
    ![Depolama hesabı için BLOB'ları için nasıl gösterir](media/azure-stack-apply-updates/ApplyUpdates3.png) 

4. Altında **Blob hizmeti**seçin **+ kapsayıcı** bir kapsayıcı oluşturmak için. Bir ad girin (örneğin *güncelleştirme 1811*) ve ardından **Tamam**.
 
     ![Depolama hesabında bir kapsayıcı ekleme işlemi açıklanır](media/azure-stack-apply-updates/ApplyUpdates4.png)

5. Kapsayıcı oluşturulduktan sonra kapsayıcı adına tıklayın ve ardından **karşıya** paket dosyalarını kapsayıcıya yüklemek için.
 
    ![Paket dosyaları karşıya yükleme işlemini gösterir](media/azure-stack-apply-updates/ApplyUpdates5.png)

6. Altında **blobu karşıya yükleme**klasör simgesine tıklayın, güncelleştirme paketi .exe dosyasına göz atın ve ardından **açık** dosya Gezgini penceresinde.
  
7. Altında **blobu karşıya yükleme**, tıklayın **karşıya**.
  
    ![Her bir paket dosyasını nerede gösterir](media/azure-stack-apply-updates/ApplyUpdates6.png)

8. 6 ve 7. adımları yineleyin *PackageName*.bin ve Metadata.xml dosyaları. Ek Notice.txt dosyası eklediyseniz içeri aktarılmaz.
9. İşiniz bittiğinde, (zil simgesi, portalın sağ üst köşedeki) bildirimleri gözden geçirebilirsiniz. Bildirimler, karşıya yükleme tamamlandığını belirtmelidir.
10. Güncelleştirme kutucuğu panoya dönün. Kutucuk güncelleştirmesi kullanıma sunuldu belirtmeniz gerekir. Yeni eklenen güncelleştirme paketini gözden geçirmek için kutucuğa tıklayın.
11. Güncelleştirmeyi yüklemek için olarak işaretlenmiş paketi seçin **hazır** ya da paketini sağ tıklatın ve seçin ve **Şimdi Güncelleştir**, veya **Şimdi Güncelleştir** üst eylemi .
12. Güncelleştirme paketi yüklenirken tıkladığınızda, durumu görüntüleyin **güncelleştirme çalıştırması ayrıntıları** alan. Burada da tıklayabilirsiniz **tam günlükleri indirmek** günlük dosyaları indirilemedi.
13. Güncelleştirme tamamlandığında, Azure Stack gireceği kutucuğu güncelleştir gösterir.

Azure Stack'te yüklendikten sonra depolama hesabından güncelleştirmeleri el ile silebilirsiniz. Azure Stack, düzenli aralıklarla için eski güncelleştirme paketleri denetler ve bunları Depolama'dan kaldırır. Azure Stack sürebilir iki hafta içinde eski paketleri kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack güncelleştirmelerini yönetmeye genel bakış](azure-stack-updates.md)
- [Azure Stack hizmet İlkesi](azure-stack-servicing-policy.md)
