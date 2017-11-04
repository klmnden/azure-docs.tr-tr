---
title: "Devre dışı bırakın ve bir StorSimple cihazı silmek | Microsoft Docs"
description: "StorSimple cihazı hizmetinden önce devre dışı bırakma ve silme kaldırmayı açıklar."
services: storsimple
documentationcenter: 
author: SharS
manager: timlt
editor: 
ms.assetid: 155cda38-c5ae-45dc-b7e8-6444494afc9e
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/17/2017
ms.author: anbacker
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c000a642aa088ac80cc7077453b87e9a47f96900
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="deactivate-and-delete-a-storsimple-8000-series-device-via-storsimple-manager-service"></a>Devre dışı bırakın ve StorSimple Yöneticisi hizmeti üzerinden bir StorSimple 8000 serisi aygıtı silme
## <a name="overview"></a>Genel Bakış
Hizmet dışına StorSimple cihazı (örneğin, değiştirme veya Cihazınızı yükseltmek veya StorSimple artık kullanıyorsanız) almak isteyebilirsiniz. Bu durumda, silebilmek için önce cihazın devre dışı bırakılması gerekir. Devre dışı bırakma, aygıt ve karşılık gelen StorSimple Yöneticisi hizmeti arasındaki bağlantı sunucularının. Bu öğretici, bir StorSimple cihazı ilk onu devre dışı bırakma ve silme hizmetten kaldırmak açıklanmaktadır. 

Bir aygıtı devre dışı bıraktığınızda, cihazda yerel olarak depolanan tüm verileri artık erişilebilir olacaktır. Yalnızca bulutta depolanan cihaz ile ilişkili verileri kurtarılabilir.  

> [!WARNING]
> Devre dışı bırakma kalıcı bir işlemdir ve geri alınamaz. Varsayılan fabrika ayarlarına sıfırlama işlemi ilk sürece StorSimple Yöneticisi hizmetiyle devre dışı bırakılan bir cihaz kaydedilemez. 
> 
> İşlem siler, cihazda yerel olarak depolanan tüm verileri fabrika ayarlarına sıfırlayamaz. Bu nedenle, bir cihazın devre dışı bırakmadan önce tüm verilerinizi bir bulut anlık görüntüsünü gereklidir. Bu, daha sonraki bir aşamada tüm verileri kurtarmak olanak tanır.
> 
> 

Bu öğretici açıklar nasıl yapılır:

* Bir aygıtı devre dışı bırakın ve verileri silme
* Bir aygıtı devre dışı bırakın ve verileri tut

Ayrıca açıklar nasıl devre dışı bırakma ve silme StorSimple sanal cihazlar üzerinde çalışır.

> [!NOTE]
> StorSimple fiziksel veya sanal aygıt devre dışı bırakmadan önce istemciler ve bu cihazda bağımlı konakları silin veya durdurmak emin olun.
> 
> 

## <a name="deactivate-and-delete-data"></a>Devre dışı bırakın ve verileri silme
Cihaz tamamen silme ilgilendiğiniz ve cihazdaki verilerin korumak istemiyorsanız, aşağıdaki adımları tamamlayın.

#### <a name="to-deactivate-the-device-and-delete-the-data"></a>Aygıt devre dışı bırak ve veri silmek için
1. Bir aygıtı devre dışı bırakma önce tüm birim silmeniz gerekir aygıtla ilişkili kapsayıcıları (ve birimleri). Birim kapsayıcıları, ilişkili yedeklemelerinizi yalnızca sildikten sonra silebilirsiniz.
2. Cihaz şu şekilde devre dışı bırakın:
   
   1. StorSimple Yöneticisi hizmetine **aygıtları** sayfasında, sayfanın alt kısmındaki devre dışı bırakın ve istediğiniz cihazı seçin **etkinliğini**.
   2. Bir onay iletisi görüntülenir. Tıklatın **Evet** devam etmek için. Devre dışı bırakma işlemi başlatmak ve tamamlanması birkaç dakika sürebilir.
3. Devre dışı bırakma sonra cihaz tamamen silebilirsiniz. Bir cihazı silme hizmete bağlı aygıtlar listesinden kaldırır. Hizmeti, silinen cihaz artık yönetebilirsiniz. Cihazı silmek için aşağıdaki adımları kullanın:
   
   1. StorSimple Yöneticisi hizmetine **aygıtları** sayfasında, silmek istediğiniz devre dışı bırakılan bir cihaz seçin.
   2. Sayfada alta tıklatın **silmek**.
   3. Onayınız istenir. Tıklatın **Evet** devam etmek için.
      
      Cihazın silinmesi birkaç dakika sürebilir.

## <a name="deactivate-and-retain-data"></a>Devre dışı bırakın ve verileri tut
Cihaz silme ilginizi çekiyor mu, ancak verileri korumak istiyorsanız, aşağıdaki adımları tamamlayın.

#### <a name="to-deactivate-a-device-and-retain-the-data"></a>Bir cihazı devre dışı bırakmak ve verileri korumak için
1. Aygıt devre dışı bırakın. Tüm birim kapsayıcıları ve anlık görüntüleri cihazın kalır.
   
   1. StorSimple Yöneticisi hizmetine **aygıtları** sayfasında, sayfanın alt kısmındaki devre dışı bırakın ve istediğiniz cihazı seçin **etkinliğini**.
   2. Bir onay iletisi görüntülenir. Tıklatın **Evet** devam etmek için. Devre dışı bırakma işlemi başlatmak ve tamamlanması birkaç dakika sürebilir.
2. Şimdi birim kapsayıcıları ve ilişkili anlık görüntüleri üzerinden başarısız olabilir. Yordamlar için Git [StorSimple cihazınız için yük devretme ve olağanüstü durum kurtarma](storsimple-device-failover-disaster-recovery.md).
3. Devre dışı bırakma ve yük devretme aygıtı tamamen silebilirsiniz. Bir cihazı silme hizmete bağlı aygıtlar listesinden kaldırır. Hizmeti, silinen cihaz artık yönetebilirsiniz. Cihazı silmek için aşağıdaki adımları tamamlayın:
   
   1. StorSimple Yöneticisi hizmetine **aygıtları** sayfasında, silmek istediğiniz devre dışı bırakılan bir cihaz seçin.
   2. Sayfada alta tıklatın **silmek**.
   3. Onayınız istenir. Tıklatın **Evet** devam etmek için.
      
      Cihazın silinmesi birkaç dakika sürebilir.

## <a name="deactivate-and-delete-a-virtual-device"></a>Devre dışı bırakın ve sanal cihazı Sil
StorSimple sanal cihaz için devre dışı bırakma sanal makineyi kaldırır. Ardından, sanal makine ve hazırlandığında oluşturulan kaynakları silebilirsiniz. Sanal cihaz devre dışı bırakıldıktan sonra, önceki durumuna geri yüklenemez. 

Aşağıdaki eylemleri sonuçlarında devre dışı bırakma:

* StorSimple sanal cihaz kaldırılır.
* OSDisk ve StorSimple sanal cihaz için oluşturulan veri diskleri kaldırılır.
* Barındırılan hizmet ve sağlama işlemi sırasında oluşturulan sanal ağ korunur. Bu varlıklar kullanmıyorsanız, bunları el ile silmeniz gerekir.
* StorSimple sanal cihaz tarafından oluşturulan bulut anlık görüntüleri korunur.

## <a name="next-steps"></a>Sonraki adımlar
* Devre dışı bırakılan cihazı fabrika varsayılan ayarlarına geri yüklemek için şu adrese gidin [cihazı fabrika varsayılan ayarlarına sıfırlama](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).
* Teknik destek için [Microsoft Support başvurun](storsimple-contact-microsoft-support.md).
* StorSimple Yöneticisi hizmetini kullanma hakkında daha fazla bilgi için şuraya gidin [StorSimple Cihazınızı yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-manager-service-administration.md). 

