---
title: "Microsoft Azure StorSimple veri Yöneticisi'ne genel bakış | Microsoft Docs"
description: "StorSimple veri Yöneticisi hizmetini (özel olarak incelenmektedir) genel bir bakış sağlar"
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/22/2016
ms.author: vidarmsft
ms.openlocfilehash: aedb44610fe57055851538b9dbdb810e66e58d73
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="storsimple-data-manager-overview-private-preview"></a>StorSimple Data Manager genel bakış (özel olarak incelenmektedir)

## <a name="overview"></a>Genel Bakış

Microsoft Azure StorSimple yaygın olarak dosya paylaşımları ile ilgili yapılandırılmamış veriler karmaşıklığını adresleri bir karma bulut depolama çözümüdür. StorSimple bulut depolama bulut depolama ve şirket içi depolama şirket içi çözüm ve otomatik olarak katmanları verilerinin bir uzantısı olarak kullanır. Veri koruma, yerel ile tümleşik ve bulut anlık görüntüleri, sprawling bir depolama altyapısını ortadan kaldırır. Arşiv ve olağanüstü durum kurtarma ayrıca bir site dışı konumu olarak işlev gören bulut ile sorunsuz olarak gerçekleştirilir.

Bu belgede, sunuyoruz veri dönüştürme hizmet sorunsuz bir şekilde bulutta StorSimple veri erişim sağlar. Bu hizmet, StorSimple verileri ayıklamak ve bunu diğer Azure hizmetlerine biçimlerde, sunmak için API'ları kolayca kullanılabilecek sağlar. Bu Önizleme'de desteklenen biçimler şunlardır: Azure BLOB'ları ve Azure Media Services varlıklar. Bu dönüşüm hizmetlerini Azure Media Services, Azure Hdınsight, Azure Machine Learning ve Azure Search verileri StorSimple 8000 serisi şirket içi cihazda çalışmaya gibi kolayca wire sağlar.

Bu gösteren üst düzey blok diyagramını aşağıda gösterilmiştir.

![Üst düzey diyagramı](./media//storsimple-data-manager-overview/high-level-diagram.png)

Bu belgede, bu hizmet için bir özel Önizleme ne kaydolabilirsiniz açıklanmaktadır. Ayrıca, bulutta nasıl StorSimple veri kullanan uygulamaları yazmak için bu hizmet ve diğer Azure hizmetleriyle kullanabileceğinizi açıklar.

## <a name="sign-up-for-data-manager-preview"></a>Veri Yöneticisi Önizleme için kaydolma
Veri Yöneticisi hizmeti için kaydolmadan önce aşağıdaki önkoşulları gözden geçirin.

### <a name="prerequisites"></a>Ön koşullar

Bu alıştırmada, sahibi olduğunuzu varsayar.
* Etkin bir Azure aboneliği.
* kayıtlı bir StorSimple 8000 serisi aygıt erişim
* StorSimple 8000 serisi cihaz ile ilişkili tüm anahtarları.

### <a name="sign-up"></a>Kaydolma

StorSimple veri Yöneticisi'ni özel önizlemede değil. Bu hizmetin özel Önizleme için kaydolmak için aşağıdaki adımları gerçekleştirin:

1.  StorSimple veri Yöneticisi uzantısına Azure portalıyla günlüğüne: [https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager). Oturum açmak için Azure kimlik bilgilerinizi kullanın.

2.  Tıklatın  **+**  bir hizmet oluşturmak için simge. Tıklatın **depolama** ve ardından **bkz tüm** dikey penceresinde açılır.

    ![Arama StorSimple veri Yöneticisi simgesi](./media/storsimple-data-manager-overview/search-data-manager-icon.png)

3. StorSimple Data Manager simgesini görürsünüz.

    ![StorSimple veri Yöneticisi simgesini seçin](./media/storsimple-data-manager-overview/select-data-manager-icon.png)

4. StorSimple veri Yöneticisi simgesine tıklayın ve ardından **oluşturma**. Özel önizleme için etkinleştirin ve ardından istediğiniz aboneliği seçin **bana kaydolun!**

    ![Bana kaydolun](./media/storsimple-data-manager-overview/sign-me-up.png)

5. Bu yerleşik bir isteği gönderir. Biz yerleşik mümkün olan en kısa sürede çalışacaksınız. Aboneliğinizi etkinleştirildikten sonra StorSimple veri Yöneticisi hizmeti oluşturabilirsiniz.

6. Kolayca StorSimple veri Yöneticisi hizmetine erişim için Sık Kullanılanlara sabitlemek için yıldız simgesine tıklayın.

    ![Erişim StorSimple veri yöneticileri](./media/storsimple-data-manager-overview/access-data-managers.png)


## <a name="next-steps"></a>Sonraki adımlar

[StorSimple veri Yöneticisi'ni kullanın, verilerinizi dönüştürmek için kullanıcı Arabirimi](storsimple-data-manager-ui.md).