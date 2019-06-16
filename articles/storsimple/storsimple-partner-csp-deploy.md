---
title: Microsoft Azure StorSimple ve bulut çözüm sağlayıcısı programı genel bakış | Microsoft Docs
description: StorSimple iş ortakları için bir genel bakış StorSimple ve CSP hakkında.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/08/2017
ms.author: alkohli
ms.openlocfilehash: 0dac86a696599a391cb243ad11e16931e00b8921
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60630011"
---
# <a name="deploy-storsimple-virtual-array-for-cloud-solution-provider-program"></a>Bulut çözümü sağlayıcısı programı için StorSimple sanal Dizini'ni dağıtma

## <a name="overview"></a>Genel Bakış

StorSimple sanal dizisi, bulut çözümü sağlayıcısı (CSP) iş ortakları, müşteriler tarafından dağıtılabilir. CSP iş ortağı, StorSimple cihaz Yöneticisi hizmeti oluşturabilirsiniz. Bu hizmet, dağıtma ve StorSimple sanal dizisi ve ilişkili paylaşımları, birimleri ve yedekleri yönetme sonra kullanılabilir.

Bu makalede, nasıl bir CSP iş ortağı mevcut bir müşteri için bir müşteri ya da yeni bir abonelik ekleyin ve ardından bir StorSimple Virtual Array CSP'de dağıtmak için bir hizmet oluşturma açıklanır.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce şunlardan emin olun:

- CSP programı kapsamında kaydedilir.
- Geçerli sahip [iş ortağı Merkezi](https://partnercenter.microsoft.com/) oturum açma kimlik bilgileri. Kimlik bilgilerini, yeni müşteriler eklemek, müşterilerin arama veya iş ortağı panodan bir müşteri hesabına gidin için iş ortağı portalında oturum açmak etkinleştirin. CSP, Azure portalında müşteri adına bir StorSimple Yöneticisi olarak işlev görebilir.
                             
## <a name="add-a-customer"></a>Bir müşteri ekleyin

Bir müşteri eklerseniz, bir aboneliği otomatik olarak oluşturulur. Bir müşteri ekleyin (ve otomatik olarak bir abonelik oluşturmak için), iş ortağı Portalı'nda aşağıdaki adımları gerçekleştirin.

1. Git [iş ortağı Merkezi](https://partnercenter.microsoft.com/) CSP kimlik bilgilerinizi kullanarak oturum açın. Tıklayın **Pano**.

     ![İş ortağı merkezi bir Panoda](./media/storsimple-partner-csp-deploy/image1.png)
                              
2. Sol bölmedeki **müşteriler**. Sağ bölmedeki **müşteriler ekleme**. Müşterinin ayrıntılarını girin. Tıklayın **sonraki: Abonelikleri** müşteri aboneliği oluşturmak için.

    ![Müşteri Ekle](./media/storsimple-partner-csp-deploy/image2.png)

3.  Seçin **Microsoft Azure** sunar. Sayfanın alt kısmına kaydırın ve tıklayın **gözden geçirme**.

    ![Abonelik bilgilerini gözden geçirin](./media/storsimple-partner-csp-deploy/image3.png)
                              
4. Bilgileri gözden geçirin ve tıklayın **Gönder**.

    ![Aboneliği Gönder](./media/storsimple-partner-csp-deploy/image4.png)

5. Gelecek başvurular için onay bilgileri kaydedin.

    ![Onay Kaydet](./media/storsimple-partner-csp-deploy/image5.png)

6. Bulmak veya müşteriye gidin, yeni eklediğiniz. Tıklayın **şirket adı** ayrıntılarına detaya gitmek için.

    ![Müşteri arama](./media/storsimple-partner-csp-deploy/image6.png)  

7. Sol bölmede seçin **Hizmet Yönetimi**. Sağ bölmede altında **yönetmek Hizmetleri**, tıklayın **Microsoft Azure Yönetim Portalı** müşterinizin Azure yönetici olarak oturum açmak için.

    ![Azure portalında oturum açın](./media/storsimple-partner-csp-deploy/image9.png)

8. StorSimple cihaz Yöneticisi'ni oluşturmak için tıklayın **+ yeni** ve arayın veya gidin **StorSimple sanal cihaz serisi**. Daha fazla bilgi için Git [StorSimple cihaz Yöneticisi hizmetini dağıtma](storsimple-virtual-array-manage-service.md).

    ![StorSimple cihaz Yöneticisi hizmeti oluşturma](./media/storsimple-partner-csp-deploy/image8.png)


## <a name="add-a-subscription"></a>Bir abonelik ekleyin

Bazı durumlarda, varolan bir müşteri olabilir ve bir abonelik eklemeniz gerekir. Mevcut bir müşteri için bir abonelik eklemek, iş ortağı Portalı'nda aşağıdaki adımları gerçekleştirin.

1. Git [iş ortağı Merkezi](https://partnercenter.microsoft.com/) CSP kimlik bilgilerinizi kullanarak oturum açın. Tıklayın **Pano**.

     ![İş ortağı merkezi bir Panoda](./media/storsimple-partner-csp-deploy/image1.png)
                              
2. Sol bölmedeki **müşteriler**. Bulmak veya gitmek için bir abonelik eklemek istediğiniz müşteriye. Tıklayın ![genişletme onay simgesi](./media/storsimple-partner-csp-deploy/expand_pane_icon.png) müşteriniz için şirket adı için satırı genişletmek için simge. Ayrıntılar, tıklatın **abonelik ekleme**.

    ![Müşteriler](./media/storsimple-partner-csp-deploy/image10.png)

3. Denetleme **Microsoft Azure** için **üst teklifler** abonelik tıklayıp **Gönder**. Bu, yeni bir abonelik oluşturur.

    ![Yeni Abonelik Ekle](./media/storsimple-partner-csp-deploy/image11.png)

6. Yeni bir abonelik oluşturduktan sonra tıklayın **<--müşteriler** dönmek için sol bölmesinde **müşteriler** sayfası. Kendisi için bir abonelik oluşturduğunuz müşteri arayın. Tıklayın **şirket adı** ayrıntılarına detaya gitmek için.

    ![Müşteri arama](./media/storsimple-partner-csp-deploy/image6.png)  

7. Sol bölmede seçin **Hizmet Yönetimi**. Sağ bölmede altında **yönetmek Hizmetleri**, tıklayın **Microsoft Azure Yönetim Portalı** müşterinizin Azure yönetici olarak oturum açmak için.

    ![Azure portalında oturum açın](./media/storsimple-partner-csp-deploy/image9.png)

8. StorSimple cihaz Yöneticisi'ni oluşturmak için tıklayın **+ yeni** ve arayın veya gidin **StorSimple sanal cihaz serisi**. Daha fazla bilgi için Git [StorSimple cihaz Yöneticisi hizmetini dağıtma](storsimple-virtual-array-manage-service.md).

    ![StorSimple cihaz Yöneticisi hizmeti oluşturma](./media/storsimple-partner-csp-deploy/image8.png)

## <a name="next-steps"></a>Sonraki adımlar

- CSP, StorSimple ile ilgili başka sorularım varsa, Git [CSP'de StorSimple: Sık sorulan sorular](storsimple-partner-csp-faq.md).
- StorSimple'ınızı dağıtmak hazırsanız, Git [CSP'de StorSimple'ınızı dağıtın](storsimple-partner-csp-deploy.md).
