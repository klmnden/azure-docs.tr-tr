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
ms.date: 02/11/2019
ms.author: mabrigg
ms.reviewer: justini
ms.lastreviewed: 02/11/2019
ms.openlocfilehash: 0c3f52c78bbfd3094324b74f3b66610fcebfa2f4
ms.sourcegitcommit: 39397603c8534d3d0623ae4efbeca153df8ed791
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56099301"
---
# <a name="apply-updates-in-azure-stack"></a>Azure stack'teki güncelleştirmelerini uygulayın

*Uygulama hedefi: Azure Stack tümleşik sistemleri*

Kullanabileceğiniz **güncelleştirme** kutucuğunda Azure Stack için Microsoft veya OEM güncelleştirme paketleri uygulamak Yönetim Portalı'nda.

Bir tümleşik sistemler sürümü 1807 kullandığınız ya da daha önce güncelleştirme paketini indirin Azure Stack'e paket dosyalarını içeri ve ardından güncelleştirme paketini yükleyin. Yönergeler için [paketini yükleyerek güncelleştirme Azure Stack](#update-azure-stack-by-downloading-the-package)

Bu yönergeler iş ile Azure Stack tümleşik sistemleri yükseltin. Azure Stack geliştirme sistemi kullanıyorsanız, güncel sürümü için yükleme paketi indirmeniz gerekir. Yönergeler için [Azure Stack geliştirme Seti'ni yükleme](.\asdk\asdk-install.md)

## <a name="update-azure-stack"></a>Azure Stack güncelleştir

### <a name="select-and-apply-an-update-package"></a>Seçin ve bir güncelleştirme paketini Uygula

1. Yönetim Portalı'nı açın.

2. Seçin **Pano**. Seçin **güncelleştirme** Döşe.

    ![Azure Stack güncelleştirmesi mevcut](media/azure-stack-apply-updates/azure-stack-updates-1901-dashboard.png)

3. Azure Stack'ın geçerli sürümü not edin. Sonraki tam sürümüne güncelleştirebilirsiniz. Örneğin, Azure Stack 1811 çalıştıran, sonraki sürüm yayın tarihi 1901 ise için.

    ![Azure yığını güncelleştirmesi uygulama](media/azure-stack-apply-updates/azure-stack-updates-1901-updateavailable.png)

4. Güncelleştirmeler listesinde kullanılabilir bir sonraki sürümü seçin. Seçebileceğiniz **görünümü** sürümde, sürümü için sürüm notları konusunda açmak için Notlar sütununu sürüm değişikliklerini gözden geçirmek istediğiniz.

5. Şimdi, güncelleştirmeyi seçin. Güncelleştirme başlar.

### <a name="review-update-history"></a>Güncelleştirme geçmişini gözden geçirme

1. Yönetim Portalı'nı açın.

2. Seçin **Pano**. Seçin **güncelleştirme** Döşe.

3. Seçin **güncelleştirme geçmişi**.

![Azure Stack güncelleştirme geçmişi](media/azure-stack-apply-updates/azure-stack-update-history.PNG)

## <a name="update-azure-stack-by-downloading-the-package"></a>Paketini yükleyerek Azure Stack güncelleştir

Bir tümleşik sistemler sürümü 1807 kullandığınız ya da daha önce güncelleştirme paketini indirin Azure Stack'e paket dosyalarını içeri ve ardından güncelleştirme paketini yükleyin.

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
