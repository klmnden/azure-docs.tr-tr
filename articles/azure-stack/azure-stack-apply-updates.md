---
title: Azure stack'teki güncelleştirmeleri uygulamak | Microsoft Docs
description: İçeri aktarma ve Azure Stack tümleşik sistemi için Microsoft güncelleştirme paketlerini yükleme hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 449ae53e-b951-401a-b2c9-17fee2f491f1
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2017
ms.author: mabrigg
ms.openlocfilehash: da8261d27ae7fad3c5ff30e4e1cce3f1bca2b70a
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39035342"
---
# <a name="apply-updates-in-azure-stack"></a>Azure stack'teki güncelleştirmelerini uygulayın

*İçin geçerlidir: Azure Stack tümleşik sistemleri*

Azure Stack operatör olarak Microsoft uygulayabilir veya Yönetici portalı'nda Azure Stack için OEM güncelleştirme paketlerini güncelleştirmeyi kullanarak kutucuk. Güncelleştirme paketinin indirilebilmesi Azure Stack'e paket dosyalarını içeri ve ardından güncelleştirme paketini yüklemeniz gerekir. 

## <a name="download-the-update-package"></a>Güncelleştirme paketini indirme

Azure Stack için bir Microsoft veya OEM güncelleştirme paketi kullanılabilir olduğunda, Azure yığını tarafından erişilebilen bir konuma paketi indirebilir ve paket içeriğini gözden geçirin. Bir güncelleştirme paketi genellikle aşağıdaki dosyalardan oluşur:

- Bir kendiliğinden *PackageName*.exe dosyası. Bu dosya, güncelleştirme, örneğin en son toplu güncelleştirmeyi için Windows Server için yükü içerir.   
- Karşılık gelen *PackageName*.bin dosyaları. Bu dosyalar ilişkili olduğu yükü için sıkıştırma sağlar *PackageName*.exe dosyası. 
- Metadata.xml dosyası. Bu dosya, güncelleştirme, örneğin Yayımcı adı, önkoşul, boyutu ve Destek yol URL'si hakkında gerekli bilgileri içerir.

## <a name="import-and-install-updates"></a>İçeri aktarmak ve güncelleştirmeleri yükleme

Aşağıdaki yordam, içeri aktarma ve Yönetici portalı'nda güncelleştirme paketlerini yükleme gösterir.

> [!IMPORTANT]
> Herhangi bir bakım işlemi kullanıcılara bildirmek ve çalışma saatleri sırasında mümkün olduğunca normal bakım pencereleri zamanlamanızı öneririz. Bakım işlemleri, kullanıcı iş yükleri hem portal işlemlerini etkileyebilir.

1. Yönetici portalında **diğer hizmetler**. Ardından, altında **veri + depolama** kategorisi, select **depolama hesapları**. (Veya filtre kutusuna yazmaya başlayın **depolama hesapları**ve bu seçeneği belirleyin.)

    ![Depolama hesaplarını portalda nerede bulacağını gösterir](media/azure-stack-apply-updates/ApplyUpdates1.png)

2. Filtre kutusuna **güncelleştirme**seçip **updateadminaccount** depolama hesabı.

    ![Updateadminaccount için arama yapmak nasıl gösterir](media/azure-stack-apply-updates/ApplyUpdates2.png)

3. Depolama hesabı ayrıntıları altında **Hizmetleri**seçin **Blobları**.
 
    ![Depolama hesabı için BLOB'ları için nasıl gösterir](media/azure-stack-apply-updates/ApplyUpdates3.png) 
 
4. Altında **Blob hizmeti**seçin **+ kapsayıcı** bir kapsayıcı oluşturmak için. Bir ad girin (örneğin *güncelleştirme 1709*) ve ardından **Tamam**.
 
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

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack genel bakış güncelleştirmelerini yönetme](azure-stack-updates.md)
- [Azure Stack hizmet İlkesi](azure-stack-servicing-policy.md)
