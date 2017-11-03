---
title: "Azure yığınında güncelleştirmeleri uygulamak | Microsoft Docs"
description: "İçeri aktarma ve Azure tümleşik yığını sistemi için Microsoft güncelleştirme paketlerini yüklemek hakkında bilgi edinin."
services: azure-stack
documentationcenter: 
author: twooley
manager: byronr
editor: 
ms.assetid: 449ae53e-b951-401a-b2c9-17fee2f491f1
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2017
ms.author: twooley
ms.openlocfilehash: b00bd606faaffaad30ff6cea3bcf47dc85282f69
ms.sourcegitcommit: cf4c0ad6a628dfcbf5b841896ab3c78b97d4eafd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2017
---
# <a name="apply-updates-in-azure-stack"></a>Azure yığınında güncelleştirmeleri uygulama

*Uygulandığı öğe: Azure yığın tümleşik sistemleri*

Bir Azure yığın operatör olarak, Microsoft update Yönetici portalı'nda güncelleştirme kullanarak Azure yığın paketleri döşeme uygulayabilirsiniz. Microsoft güncelleştirme paketini indirin, Azure yığınına paketi dosyalarını içe ve ardından güncelleştirme paketini yüklemek gerekir. 

## <a name="download-the-update-package"></a>Güncelleştirme paketini indirin

Azure yığını için bir Microsoft güncelleştirme paketi kullanılabilir olduğunda, Azure yığınından erişilebilen bir konuma paketini indirin ve paket içeriğini gözden geçirin. Bir güncelleştirme paketi genellikle aşağıdaki dosyaları içerir:

- Kendi kendini ayıklayan *PackageName*.exe dosyası. Bu dosya güncelleştirmesi, örneğin son için toplu güncelleştirme Windows Server için yükü içerir.   
- Karşılık gelen *PackageName*.bin dosyaları. Bu dosyaları ile ilişkili yük sıkıştırma sağlamak *PackageName*.exe dosyası. 
- Metadata.xml dosyası. Bu dosya, örneğin Yayımcı adı, önkoşul, boyutu ve Destek yol URL'si güncelleştirme hakkında önemli bilgiler içerir.

## <a name="import-and-install-updates"></a>İçeri aktarmak ve güncelleştirmeleri yükleme

Aşağıdaki yordam, içeri aktarma ve güncelleştirme paketlerini yüklemek için Yönetici portalı'nda gösterilmektedir.

> [!IMPORTANT]
> Bakım işlemleri kullanıcılara bildirmek ve iş saatleri sırasında mümkün olduğunca normal bakım pencereleri zamanlama öneririz. Bakım işlemleri, kullanıcı iş yükleri ve portal işlemlerini etkileyebilir.

1. Yönetici portalı'nda seçin **daha fazla hizmet**. Ardından, altında **veri + depolama** kategorisi, select **depolama hesapları**. (Veya filtre kutusuna yazmaya başlayın **depolama hesapları**ve seçin.)

    ![Depolama hesapları portalda nerede bulacağını gösterir](media/azure-stack-apply-updates/ApplyUpdates1.png)

2. Filtre kutusuna **güncelleştirme**seçip **updateadminaccount** depolama hesabı.

    ![Updateadminaccount için arama yapma gösterir](media/azure-stack-apply-updates/ApplyUpdates2.png)

3. Depolama hesabı ayrıntıları altında **Hizmetleri**seçin **BLOB'lar**.
 
    ![Depolama hesabı için BLOB'larını alma gösterir](media/azure-stack-apply-updates/ApplyUpdates3.png) 
 
4. Altında **Blob hizmeti**seçin **+ kapsayıcı** bir kapsayıcı oluşturmak için. Bir ad girin (örneğin *güncelleştirme 1709*) ve ardından **Tamam**.
 
     ![Depolama hesabında bir kapsayıcı eklemeyi gösterir](media/azure-stack-apply-updates/ApplyUpdates4.png)

5. Kapsayıcı oluşturulduktan sonra kapsayıcı adına tıklayın ve ardından **karşıya** kapsayıcıya paket dosyalarını karşıya yüklemek için.
 
    ![Paket dosyaları karşıya nasıl yükleneceğini gösterir](media/azure-stack-apply-updates/ApplyUpdates5.png)

6. Altında **karşıya yükleme blob**klasör simgesine tıklayın, güncelleştirme paketin .exe dosyasına gözatın ve ardından **açık** dosya Gezgini penceresinde.
  
7. Altında **karşıya yükleme blob**, tıklatın **karşıya**. 
 
    ![Her bir paket dosyası karşıya yüklemek nereye gösterir](media/azure-stack-apply-updates/ApplyUpdates6.png)

8. 6 ve 7. adımları tekrarlayarak *PackageName*.bin ve Metadata.xml dosyaları. 
9. İşiniz bittiğinde, (portalın sağ üst köşedeki zil simgesine) bildirimleri gözden geçirebilirsiniz. Bildirimler, karşıya yükleme tamamlandı belirtmeniz gerekir. 
10. Geri panosundaki güncelleştirme bölümünden gidin. Döşeme bir güncelleştirme kullanıma hazır olduğunu gösterebilir. Yeni eklenen güncelleştirme paketi gözden geçirmek için kutucuğa tıklayın.
11. Güncelleştirmeyi yüklemek için işaretlenmiş paketi seçin **hazır** ya da paketini sağ tıklatın ve seçin ve **şimdi güncelleştirmek**, veya **şimdi güncelleştirmek** ilk eylemi .
12. Yüklenen güncelleştirme paketi tıklattığınızda durumu görüntüleyebilirsiniz **güncelleştirme çalışma ayrıntıları** alanı. Buradan, tıklatarak **karşıdan tam günlükleri** günlük dosyalarını indirmek için.
13. Güncelleştirme tamamlandığında güncelleştirme döşeme güncelleştirilmiş Azure yığın sürümünü gösterir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure yığın genel bakış güncelleştirmelerini yönetme](azure-stack-updates.md)
- [İlke bakım azure yığını](azure-stack-servicing-policy.md)
