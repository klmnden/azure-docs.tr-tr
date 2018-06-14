---
title: Microsoft Azure StorSimple ve bulut çözümleri sağlayıcısı programı genel bakış | Microsoft Docs
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
ms.openlocfilehash: c8cb51093142146fc7d43b51a62d949f6cc38988
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23875658"
---
# <a name="deploy-storsimple-virtual-array-for-cloud-solution-provider-program"></a>StorSimple sanal dizinin bulut çözümü sağlayıcısı programı için dağıtma

## <a name="overview"></a>Genel Bakış

StorSimple sanal dizinin müşterileri için bulut çözümü sağlayıcısı (CSP) iş ortakları tarafından dağıtılabilir. Bir CSP ortak bir StorSimple cihaz Yöneticisi hizmeti oluşturabilirsiniz. Bu hizmet, daha sonra StorSimple sanal dizinin ve ilişkili paylaşımları, birimler ve yedekleme dağıtmak ve yönetmek için de kullanılabilir.

Bu makalede, nasıl bir CSP ortak bir müşteri ya da yeni bir abonelik varolan bir müşteri ekleyin ve CSP StorSimple sanal dizisinde dağıtmak için bir hizmet oluşturma açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce emin olun:

- CSP programın altında kaydedilir.
- Geçerli sahip [ortağı Merkezi'nde](http://partnercenter.microsoft.com/) oturum açma kimlik bilgileri. Kimlik bilgileri yeni müşteriler eklemek, müşteriler için arama veya iş ortağı panodan bir müşteri hesabına gezinmek için iş ortağı portalında oturum açmanızı sağlar. CSP, Azure portalında müşteri adına bir StorSimple Yöneticisi olarak işlev görebilir.
                             
## <a name="add-a-customer"></a>Bir müşteri ekleyin

Bir müşteri eklerseniz, bir abonelik otomatik olarak oluşturulur. Bir müşteri ekleyin (ve otomatik olarak bir aboneliği oluşturmak için), iş ortağı Portalı'nda aşağıdaki adımları gerçekleştirin.

1. Git [ortağı Merkezi'nde](http://partnercenter.microsoft.com/) ve CSP kimlik bilgilerinizi kullanarak oturum açın. Tıklatın **Pano**.

     ![İş ortağı Merkezi'nde Panosu](./media/storsimple-partner-csp-deploy/image1.png)
                              
2. Sol bölmesinde, **müşteriler**. Sağ bölmesinde, **müşteriler eklemek**. Müşteri ayrıntılarını girin. Tıklatın **sonraki: abonelikleri** bir müşteri aboneliği oluşturmak için.

    ![Müşteri ekleme](./media/storsimple-partner-csp-deploy/image2.png)

3.  Seçin **Microsoft Azure** sunar. Sayfanın alt kısmına kaydırın ve tıklatın **gözden**.

    ![Abonelik bilgileri gözden geçirin](./media/storsimple-partner-csp-deploy/image3.png)
                              
4. Bilgileri gözden geçirin ve tıklayın **gönderme**.

    ![Abonelik gönderme](./media/storsimple-partner-csp-deploy/image4.png)

5. Gelecekte başvurmak için onay bilgilerini kaydedin.

    ![Kaydetme onayı](./media/storsimple-partner-csp-deploy/image5.png)

6. Bulma veya müşteriye gidin, yeni eklediğiniz. Tıklatın **şirket adı** ayrıntılara detaya gitmek için.

    ![Müşteri arayın](./media/storsimple-partner-csp-deploy/image6.png)  

7. Sol bölmede seçin **Hizmet Yönetimi**. Sağ bölmede altında **yönetmek Hizmetleri**, tıklatın **Microsoft Azure Yönetim Portalı** müşteri için Azure yönetici olarak oturum açmak için.

    ![Azure portalında oturum açın](./media/storsimple-partner-csp-deploy/image9.png)

8. StorSimple cihaz Yöneticisi oluşturmak için tıklatın **+ yeni** aramak ve gidin **StorSimple sanal cihaz seri**. Daha fazla bilgi için Git [StorSimple Aygıt Yöneticisi'ni hizmet dağıtma](storsimple-virtual-array-manage-service.md).

    ![StorSimple cihaz Yöneticisi hizmeti oluşturma](./media/storsimple-partner-csp-deploy/image8.png)


## <a name="add-a-subscription"></a>Bir abonelik Ekle

Bazı durumlarda, varolan bir müşteri olabilir ve bir abonelik eklemeniz gerekir. Varolan bir müşteri için bir abonelik eklemek için iş ortağı Portalı'nda aşağıdaki adımları gerçekleştirin.

1. Git [ortağı Merkezi'nde](http://partnercenter.microsoft.com/) ve CSP kimlik bilgilerinizi kullanarak oturum açın. Tıklatın **Pano**.

     ![İş ortağı Merkezi'nde Panosu](./media/storsimple-partner-csp-deploy/image1.png)
                              
2. Sol bölmesinde, **müşteriler**. Bulma veya gidin, bir aboneliğe eklemek istediğiniz müşteriye. Tıklatın ![genişletme onay simgesi](./media/storsimple-partner-csp-deploy/expand_pane_icon.png) şirket adı, müşteriniz için satırı genişletmek için simge. Ayrıntılar tıklatın **abonelik ekleme**.

    ![Müşteriler](./media/storsimple-partner-csp-deploy/image10.png)

3. Denetleyin **Microsoft Azure** için **üst teklifleri** abonelik ve tıklatın **gönderme**. Bu, yeni bir abonelik oluşturur.

    ![Yeni Abonelik Ekle](./media/storsimple-partner-csp-deploy/image11.png)

6. Yeni bir abonelik oluşturulduktan sonra tıklatın **<--müşteriler** geri dönmek için sol bölmesinde **müşteriler** sayfası. Kendisi için bir abonelik oluşturduğunuz müşteri arayın. Tıklatın **şirket adı** ayrıntılara detaya gitmek için.

    ![Müşteri arayın](./media/storsimple-partner-csp-deploy/image6.png)  

7. Sol bölmede seçin **Hizmet Yönetimi**. Sağ bölmede altında **yönetmek Hizmetleri**, tıklatın **Microsoft Azure Yönetim Portalı** müşteri için Azure yönetici olarak oturum açmak için.

    ![Azure portalında oturum açın](./media/storsimple-partner-csp-deploy/image9.png)

8. StorSimple cihaz Yöneticisi oluşturmak için tıklatın **+ yeni** aramak ve gidin **StorSimple sanal cihaz seri**. Daha fazla bilgi için Git [StorSimple Aygıt Yöneticisi'ni hizmet dağıtma](storsimple-virtual-array-manage-service.md).

    ![StorSimple cihaz Yöneticisi hizmeti oluşturma](./media/storsimple-partner-csp-deploy/image8.png)

## <a name="next-steps"></a>Sonraki adımlar

- CSP StorSimple ilgili daha fazla sorularınız varsa, Git [CSP StorSimple: sık sorulan sorular](storsimple-partner-csp-faq.md).
- StorSimple dağıtmak hazırsanız Git [, StorSimple CSP içinde dağıtmak](storsimple-partner-csp-deploy.md).
