---
title: "Microsoft Azure StorSimple veri Yöneticisi kullanıcı Arabirimi | Microsoft Docs"
description: "StorSimple veri Yöneticisi hizmetini UI (özel olarak incelenmektedir) kullanmayı açıklar"
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
ms.openlocfilehash: 53a8599df2c647613122cd791b680e2e658586b0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="manage-using-the-storsimple-data-manager-service-ui-private-preview"></a>StorSimple veri Yöneticisi hizmeti kullanıcı Arabirimi (özel olarak incelenmektedir) kullanarak yönetme

Bu makalede StorSimple 8000 serisi aygıtlarında bulunan veriler üzerindeki veri dönüştürme gerçekleştirmek için StorSimple Data Manager kullanıcı arabirimini nasıl kullanabileceğiniz açıklanır. Dönüştürülen veriler, ardından Azure Media Services, Azure Hdınsight, Azure Machine Learning ve Azure Search gibi diğer Azure Hizmetleri tarafından kullanılabilecek. 


## <a name="use-storsimple-data-transformation"></a>StorSimple veri dönüşümü kullanın

StorSimple Data Manager içinde veri dönüştürme oluşturulabilir kaynaktır. Veri dönüştürme hizmet verileri Azure depolama alanında BLOB'lar için StorSimple şirket içi aygıtınızdan taşımanıza olanak tanır. Bu nedenle, iş akışında StorSimple cihazınız ve veri depolama hesabına taşımak istediğiniz ilgi ayrıntılarını belirtmeniz gerekir.

### <a name="create-a-storsimple-data-manager-service"></a>Bir StorSimple veri Yöneticisi hizmet oluşturma

Bir StorSimple veri Yöneticisi hizmet oluşturmak için aşağıdaki adımları gerçekleştirin.

1. Bir StorSimple veri Yöneticisi hizmeti oluşturmak için şu adrese gidin [https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager)

2. Tıklatın  **+**  simgesi ve StorSimple veri Yöneticisi arama. StorSimple veri Yöneticisi hizmetinize tıklayın ve ardından **oluşturma**.

3. Bu hizmet oluşturmak için aboneliğinizi etkinse, aşağıdaki dikey penceresine bakın.

    ![StorSimple veri yöneticilerini kaynak oluşturma](./media/storsimple-data-manager-ui/create-new-data-manager-service.png)

4. Girdi girin ve tıklayın **oluşturma**. Belirtilen konum, depolama hesaplarınıza ve StorSimple Yöneticisi hizmetiniz barındıran bir olmalıdır. Şu anda yalnızca Batı ABD ve Batı Avrupa bölgeleri desteklenir. Bu nedenle, StorSimple Yöneticisi hizmetiniz, veri Yöneticisi hizmeti ve ilişkili depolama hesabı tüm önceki desteklenen bölgelerde olmalıdır. Bir hizmet oluşturmak için yaklaşık bir dakika sürer.

### <a name="create-a-data-transformation-job-definition"></a>Veri dönüştürme iş tanımı oluştur

Bir StorSimple Data Manager Hizmeti'nde bir veri dönüştürme iş tanımı oluşturmanız gerekir. Bir iş tanımı bir depolama hesabı yerel biçiminde taşınmasını ilgilendiğiniz veri ayrıntılarını belirtir. 

Yeni bir veri dönüştürme iş tanımı oluşturmak için aşağıdaki adımları gerçekleştirin.

1.  Oluşturduğunuz hizmete gidin. Tıklatın **+ iş tanımı**.

    ![Tıklatın + iş tanımı](./media/storsimple-data-manager-ui/click-add-job-definition.png)

2. Yeni iş tanımı dikey penceresi açılır. İş tanımı bir ad verin ve tıklatın **kaynak**. İçinde **yapılandırma veri kaynağı** dikey penceresinde, StorSimple Cihazınızı ayrıntılarını ve istediğiniz verileri belirtin.

    ![İş tanımı oluştur](./media/storsimple-data-manager-ui//create-new-job-deifnition.png)

3. Bu yeni bir veri Yöneticisi hizmeti olduğundan, hiçbir veri depoları yapılandırılır. StorSimple Yöneticisi bir veri deposu olarak eklemek için tıklatın **yeni Ekle** veri deposu açılır ve ardından **eklemek veri deposu**.

4. Seçin **StorSimple 8000 serisi Yöneticisi** deposu olarak yazın ve özelliklerini girin, **StorSimple Yöneticisi**. İçin **kaynak kimliği** alana gereksiniminiz önce numara girmek **:** kayıt anahtarı, StorSimple Yöneticisi.

    ![Veri kaynağı oluşturun](./media/storsimple-data-manager-ui/create-new-data-source.png)

5.  Tıklatın **Tamam** bittiğinde. Bu veri deponuz kaydeder ve bu StorSimple Yöneticisi diğer iş tanımları bu parametreler tekrar girmeden yeniden kullanılabilir. Tıklattıktan sonra birkaç saniye sürer **Tamam** için StorSimple açılır listede göstermeyi Yöneticisi.

6.  İçinde **yapılandırma veri kaynağı** dikey penceresinde, cihaz adını ve verilerinizi ilgi birim adı girin.

7.  İçinde **filtre** alt bölüm, verilerinizi ilgi içeren kök dizini girin (Bu alan ile başlaması gereken bir `\`). Dosya filtreleri burada da ekleyebilirsiniz.

8.  Veri dönüştürme hizmeti Azure anlık görüntüleri gönderilir veriler üzerinde çalışır. Bu iş çalışırken, bu işi (en son veriler üzerinde çalışmak için) her çalıştırılışında yedekleyin seçebilirsiniz veya (bazı arşivlenmiş veriler üzerinde çalışıyorsanız) son varolan yedekleme bulutta kullanılacak.

    ![Yeni veri kaynağı ayrıntıları](./media/storsimple-data-manager-ui/new-data-source-details.png)

9. Ardından, hedef ayarlarının yapılandırılmış olması gerekir. Desteklenen hedefleri – Azure Storage ve Azure Media Services hesapları 2 tür vardır. Bu hesaptaki BLOB'lar içine dosyaları yerleştirmek için depolama hesapları'nı seçin. Medya Hizmetleri hesabı dosyaları bu hesaptaki varlıklar yerleştirin seçin. Yeniden, biz depo eklemeniz gerekir. Açılır listede seçin **yeni Ekle** ve ardından **ayarlarını yapılandırma**.

    ![Veri havuzu oluşturma](./media/storsimple-data-manager-ui/create-new-data-sink.png)

10. Burada, eklemek istediğiniz depo ve deposuyla ilişkili diğer parametreler türünü seçebilirsiniz. İş çalıştırıldığında her iki durumda da, bir depolama kuyruğu oluşturulur. Bu kuyruk, hazır olduğunda dönüştürülen bloblar hakkında iletilerle doldurulur. Bu kuyruğun adı, iş tanımının adıyla aynıdır. Seçerseniz **Media Services** depo türü, sonra da depolama hesabının kimlik bilgilerini Sıranın oluşturulduğu girebilirsiniz.

    ![Yeni veri havuzu ayrıntıları](./media/storsimple-data-manager-ui/new-data-sink-details.png)

11. (Bu, birkaç saniye sürer) veri deposu eklendikten sonra açılır menüde, depoyu bulabilirsiniz **hedef hesap adı**.  Gereksinim duyduğunuz bir hedef seçin.

12. Tıklatın **Tamam** iş tanımı oluşturmak için. İş tanımı şimdi ayarlanır. Bu iş tanımı birden çok kez aracılığıyla kullanıcı Arabirimi kullanabilirsiniz.

    ![Yeni iş tanımı Ekle](./media/storsimple-data-manager-ui/add-new-job-definition.png)

### <a name="run-the-job-definition"></a>İş tanımı çalıştırın

İş tanımında belirtilen depolama hesabı StorSimple verileri taşımak gereksinim duyduğunuzda, çağrılacak gerekir. İş çağırma her zaman parametrelerinin değiştirilmesi biraz esneklik vardır. Adımlar aşağıdaki gibidir:

1. StorSimple Data Manager hizmetinizi seçin ve Git **izleme**. Tıklatın **Şimdi Çalıştır**.

    ![Tetikleyici iş tanımı](./media/storsimple-data-manager-ui/run-now.png)

2. Çalıştırmak istediğiniz iş tanımı seçin. Tıklatın **çalışma ayarları** değiştirmek için bu iş için isteyebilirsiniz herhangi bir ayarı değiştirmek için.

    ![Çalışma proje ayarları](./media/storsimple-data-manager-ui/run-settings.png)

3. Tıklatın **Tamam** ve ardından **çalıştırmak** işinizi başlatmak için. Bu işi izlemek için Git **işleri** sayfası, StorSimple veri Yöneticisi'nde.

    ![İşlerini listesi ve durumu](./media/storsimple-data-manager-ui/jobs-list-and-status.png)

4. İzleme ek olarak **işleri** dikey penceresinde ayrıca dinleme burada bir ileti eklenen bir dosya depolama hesabına StorSimple taşınır her zaman depolama kuyruğu üzerinde.


## <a name="next-steps"></a>Sonraki adımlar

[StorSimple veri Yöneticisi işleri başlatmak için .NET SDK'yı kullanma](storsimple-data-manager-dotnet-jobs.md).