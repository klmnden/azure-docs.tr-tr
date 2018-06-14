---
title: Microsoft Azure StorSimple veri Yöneticisi kullanıcı Arabirimi | Microsoft Docs
description: StorSimple veri Yöneticisi hizmetini UI kullanmayı açıklar
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 01/16/2018
ms.author: alkohli
ms.openlocfilehash: d704cf8e6840c6a7b0a637c404d421f9f1497c46
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2018
ms.locfileid: "27862266"
---
# <a name="manage-the-storsimple-data-manager-service-in-azure-portal"></a>Azure portalında StorSimple veri Yöneticisi hizmetini yönetme

Bu makalede, StorSimple 8000 serisi cihazlar üzerinde bulunan verileri dönüştürmek için StorSimple Data Manager kullanıcı arabirimini nasıl kullanabileceğiniz açıklanır. Dönüştürülen veriler, ardından Azure Media Services, Azure Hdınsight, Azure Machine Learning ve Azure Search gibi diğer Azure Hizmetleri tarafından kullanılabilecek.


## <a name="use-storsimple-data-transformation"></a>StorSimple veri dönüşümü kullanın

StorSimple Data Manager içinde hangi veri dönüştürme örneği kaynaktır. Veri dönüştürme hizmeti, BLOB veya Azure dosyaları yerel biçiminde StorSimple biçiminden veri dönüştürme olanak tanır. StorSimple yerel biçimini verileri dönüştürmek için StorSimple 8000 serisi cihazınız ve veri dönüştürmek istediğiniz ilgi ayrıntılarını belirtmeniz gerekir.

### <a name="create-a-storsimple-data-manager-service"></a>Bir StorSimple veri Yöneticisi hizmet oluşturma

Bir StorSimple veri Yöneticisi hizmet oluşturmak için aşağıdaki adımları gerçekleştirin.

1. [Azure portalda](https://portal.azure.com/) oturum açmak için Microsoft hesabınızın kimlik bilgilerini kullanın.

2. Tıklatın **+ kaynak oluşturma** ve StorSimple veri Yöneticisi arama.

    ![Bir StorSimple veri Yöneticisi hizmeti 1 oluşturun](./media/storsimple-data-manager-ui/create-service-1.png)

3. StorSimple Data Manager'ı tıklatın ve ardından **oluşturma**.
    
    ![Bir StorSimple veri Yöneticisi hizmeti 2 oluşturun](./media/storsimple-data-manager-ui/create-service-3.png)

3. Yeni hizmet için aşağıdakileri belirtin:

    1. Benzersiz bir sağlamak **hizmet adı** için StorSimple veri Yöneticisi. Hizmetinizi tanımlayabilmek için kullanılan kolay bir addır. Ad harf, rakam ve kısa çizgi olabilir 3-24 karakter uzunluğunda olabilir. Ad bir harf veya sayıyla başlamalı ve bitmelidir.

    2. Seçin bir **abonelik** aşağı açılan listeden. Abonelik fatura hesabınıza bağlıdır. Bu alan otomatik olarak doldurulan (ve seçilebilir değil) yalnızca bir aboneliğiniz varsa.

    3. Varolan bir kaynak grubu seçin veya yeni bir grup oluşturun. Daha fazla bilgi edinmek için bkz. [Azure kaynak grupları](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-infrastructure-resource-groups-guidelines/).

    4. Belirtin **konumu** depolama hesaplarınızı ve StorSimple veri Yöneticisi hizmetinize barındırıldığı hizmetiniz için. StorSimple cihaz Yöneticisi hizmetiniz, veri Yöneticisi hizmeti ve ilişkili depolama hesabı tüm desteklenen bölgelerde olmalıdır.
    
    5. Bu hizmet için bir bağlantı Panonuzda almak için seçin **panoya Sabitle**.
    
    6. **Oluştur**’a tıklayın.

    ![Bir StorSimple veri Yöneticisi hizmet 3 oluşturma](./media/storsimple-data-manager-ui/create-service-4.png)

Hizmetin oluşturulması birkaç dakika sürer. Hizmet sorunsuz oluşturulduktan sonra yeni hizmet görüntülenen bir bildirim görür.

### <a name="create-a-data-transformation-job-definition"></a>Veri dönüştürme iş tanımı oluştur

Bir StorSimple Data Manager Hizmeti'nde bir veri dönüştürme iş tanımı oluşturmanız gerekir. Bir iş tanımı bir depolama hesabı yerel biçiminde taşınmasını ilgilendiğiniz StorSimple veri ayrıntılarını belirtir. Bir iş tanımı oluşturduktan sonra daha sonra bu iş yeniden farklı çalışma zamanı ayarları ile çalıştırabilirsiniz.

Bir iş tanımı oluşturmak için aşağıdaki adımları gerçekleştirin.

1. Oluşturduğunuz hizmete gidin. Git **Yönetim > iş tanımları**.

2. Tıklatın **+ iş tanımı**.

    ![Tıklatın + iş tanımı](./media/storsimple-data-manager-ui/create-job-definition-1.png)

3. İş tanımı için bir ad sağlayın. Adı 3-63 karakter uzunluğunda olabilir. Ad, büyük ve küçük harfler, sayılar ve kısa çizgi içerebilir.

4. İşinizi çalıştığı bir konum belirtin. Bu konum, hizmetin dağıtıldığı konumundan farklı olabilir.

5. Tıklatın **kaynak** kaynak veri deposu belirtmek için.

    ![Kaynak veri deposu yapılandırma](./media/storsimple-data-manager-ui/create-job-definition-2.png)

6. Bu yeni bir veri Yöneticisi hizmeti olduğundan, hiçbir veri depoları yapılandırılır. İçinde **yapılandırma veri kaynağı**, StorSimple 8000 serisi Cihazınızı ayrıntılarını ve istediğiniz verileri belirtin.

   StorSimple Aygıt Yöneticisi'ni bir veri deposu olarak eklemek için tıklatın **yeni Ekle** veri deposu açılır ve ardından **eklemek veri deposu**.

    ![Yeni veri deposu ekleme](./media/storsimple-data-manager-ui/create-job-definition-3.png)
  
    1. Seçin **StorSimple 8000 serisi Yöneticisi** veri deposu türü olarak.
    
    2. Kaynak veri deposu için kolay bir ad girin.
    
    3. Açılır listeden StorSimple cihaz Yöneticisi hizmetiniz ile ilişkili bir abonelik seçin.
    
    4. İçin StorSimple cihaz Yöneticisi'nin ad **kaynak**.

    5. Girin **hizmet verileri şifreleme** StorSimple cihaz Yöneticisi hizmeti için anahtar. 

    ![Kaynak veri deposu 1 yapılandırın](./media/storsimple-data-manager-ui/create-job-definition-4.png)

    Tıklatın **Tamam** bittiğinde. Bu veri deponuz kaydeder. Bu parametreleri yeniden girmeye gerek kalmadan bu StorSimple Aygıt Yöneticisi'nde diğer iş tanımları yeniden. Tıklattıktan sonra birkaç saniye sürer **Tamam** açılır listede görünmesi yeni oluşturulan kaynak veri deposu için.

7. İçin aşağı açılan listeden **veri deposu**, oluşturduğunuz veri deposu seçin. 

    1. İstediğiniz verileri içeren StorSimple 8000 serisi cihaz adını girin.

    2. Verilerinizi ilgi StorSimple cihazında bulunan birimin adını belirtin.

    3. İçinde **filtre** alt bölüm, ilgi verilerinizi içeren kök dizini girin _\MyRootDirectory\Data_ biçimi. Sürücü harfleri gibi _\C:\Data_ desteklenmez. Dosya filtreleri burada da ekleyebilirsiniz.

    4. Veri dönüştürme hizmeti Azure anlık görüntüleri gönderilir veriler üzerinde çalışır. Bu işlemi çalıştırdığınızda, bu işi (en son veriler üzerinde çalışmak için) her çalıştırılışında yedekleyin seçebilirsiniz veya (bazı arşivlenmiş veriler üzerinde çalışıyorsanız) son varolan yedekleme bulutta kullanın.

    5. **Tamam**’a tıklayın.

    ![Kaynak veri deposu 2 yapılandırın](./media/storsimple-data-manager-ui/create-job-definition-8.png)

8. Ardından, hedef veri deposu yapılandırılması gerekiyor. Bu hesaptaki BLOB'lar içine dosyaları yerleştirmek için depolama hesapları'nı seçin. Açılır listede seçin **yeni Ekle** ve ardından **ayarlarını yapılandırma**.

9. Eklemek istediğiniz hedef depo ve deposuyla ilişkili diğer parametreler türünü seçin.

    Bir depolama hesabı türü hedef seçerseniz, abonelik kolay bir ad belirtebilirsiniz (seçin aynı hizmet veya diğer) ve bir depolama hesabı.
        ![Hedef veri deposuna 1 yapılandırın](./media/storsimple-data-manager-ui/create-job-definition-10.png)

    İş çalıştırıldığında bir depolama kuyruğu oluşturulur. Bu kuyruk, hazır olduğunda dönüştürülen bloblar hakkında iletilerle doldurulur. Bu kuyruğun adı, iş tanımının adıyla aynıdır.
    
10. Veri deposu ekledikten sonra birkaç dakika bekleyin.
    
    1. Aşağı açılan listeden hedefi olarak oluşturduğunuz havuzu seçin **hedef hesap adı**.

    2. BLOB'ları veya dosyaları olarak depolama türü seçin. Dönüştürülmüş verilerin bulunduğu depolama kapsayıcısının adı belirtin. **Tamam**’a tıklayın.

        ![Hedef veri deposuna depolama hesabı yapılandırma](./media/storsimple-data-manager-ui/create-job-definition-16.png)

11. Ayrıca iş çalıştırmadan önce iş süresi tahmini sunmak için seçenek kontrol edebilirsiniz. Tıklatın **Tamam** iş tanımı oluşturmak için. İş tanımı tamamlanmıştır. Bu iş tanımı birden çok kez kullanıcı Arabirimi aracılığıyla farklı çalışma zamanı ayarları ile kullanabilirsiniz.

    ![Tam iş tanımı](./media/storsimple-data-manager-ui/create-job-definition-13.png)

    Yeni oluşturulan iş tanımı, bu hizmet için iş tanımları listesine eklenir.

### <a name="run-the-job-definition"></a>İş tanımı çalıştırın

İş tanımında belirtilen depolama hesabı StorSimple verileri taşımak ihtiyaç duyduğunuzda çalıştırmak gerekir. Çalışma zamanında bazı parametreler farklı belirtilebilir. Adımlar aşağıdaki gibidir:

1. StorSimple Data Manager hizmetinizi seçin ve Git **Yönetim > iş tanımları**. Seçin ve çalıştırmak istediğiniz iş tanımı'nı tıklatın.
     
     ![İş 1 Başlat](./media/storsimple-data-manager-ui/start-job-run1.png)

2. Tıklatın **Şimdi Çalıştır**.
     
     ![İş 2 Başlat](./media/storsimple-data-manager-ui/start-job-run2.png)

3. Tıklatın **çalışma ayarları** değiştirmek için bu iş için isteyebilirsiniz herhangi bir ayarı değiştirmek için. Tıklatın **Tamam** ve ardından **çalıştırmak** işinizi başlatmak için.

    ![İş yürütme 3 Başlat](./media/storsimple-data-manager-ui/start-job-run3.png)

4. Bu işi izlemek için Git **işleri** , StorSimple veri Yöneticisi'nde. İzleme ek olarak **işleri** dikey penceresinde ayrıca dinleme burada bir ileti eklenen bir dosya depolama hesabına StorSimple taşınır her zaman depolama kuyruğu üzerinde.

    ![İş yürütme 4 Başlat](./media/storsimple-data-manager-ui/start-job-run4.png)


## <a name="next-steps"></a>Sonraki adımlar

[StorSimple veri Yöneticisi işleri başlatmak için .NET SDK'yı kullanma](storsimple-data-manager-dotnet-jobs.md).