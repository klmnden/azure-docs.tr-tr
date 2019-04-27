---
title: Microsoft Azure StorSimple veri Yöneticisi kullanıcı Arabirimi | Microsoft Docs
description: StorSimple veri Yöneticisi hizmeti UI'si kullanmayı açıklar
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
ms.openlocfilehash: fa897b4b77f7f5869eab2ba2e7db9afbd84febfa
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60631503"
---
# <a name="manage-the-storsimple-data-manager-service-in-azure-portal"></a>Azure portalında StorSimple veri Yöneticisi hizmeti yönetme

Bu makalede, StorSimple 8000 serisi cihazlarda bulunan veriler dönüştürmek için StorSimple Data Manager kullanıcı arabirimini nasıl kullanabileceğinizi açıklar. Dönüştürülmüş verileri Azure Media Services, Azure HDInsight, Azure Machine Learning ve Azure Search gibi diğer Azure Hizmetleri tarafından ardından tarafından kullanılabilir.


## <a name="use-storsimple-data-transformation"></a>StorSimple veri dönüştürme kullanın

StorSimple veri Yöneticisi'ni hangi verileri dönüştürme örneği bir kaynaktır. Veri dönüştürme hizmeti, StorSimple biçiminden verileri yerel biçiminde blobları veya Azure dosyaları için dönüştürme sağlar. StorSimple yerel biçim verileri dönüştürmek için StorSimple 8000 serisi Cihazınızı ve veri dönüştürmek istediğiniz ilgi ayrıntılarını belirtmeniz gerekir.

### <a name="create-a-storsimple-data-manager-service"></a>StorSimple veri Yöneticisi hizmeti oluşturma

Bir StorSimple veri Yöneticisi hizmeti oluşturmak için aşağıdaki adımları gerçekleştirin.

1. [Azure portalda](https://portal.azure.com/) oturum açmak için Microsoft hesabınızın kimlik bilgilerini kullanın.

2. Tıklayın **+ kaynak Oluştur** ve arama için StorSimple veri Yöneticisi.

    ![1 bir StorSimple veri Yöneticisi hizmeti oluşturma](./media/storsimple-data-manager-ui/create-service-1.png)

3. StorSimple veri Yöneticisi'ni tıklatın ve ardından **Oluştur**.
    
    ![2 bir StorSimple veri Yöneticisi hizmeti oluşturma](./media/storsimple-data-manager-ui/create-service-3.png)

3. İçin yeni hizmet, aşağıdakileri belirtin:

   1. Benzersiz bir sağlamak **hizmet adı** için StorSimple veri Yöneticisi. Hizmetinizi tanımlayabilmek için kullanılan kolay bir addır. Ad harf, rakam ve tirelerden oluşan 3-24 karakter arası uzunlukta olabilir. Ad bir harf veya sayıyla başlamalı ve bitmelidir.

   2. Seçin bir **abonelik** aşağı açılan listeden. Abonelik fatura hesabınıza bağlıdır. Bu alan otomatik olarak doldurulmuş (ve seçilebilir değil) yalnızca bir aboneliğiniz varsa.

   3. Mevcut bir kaynak grubunu seçin veya yeni bir grup oluşturun. Daha fazla bilgi edinmek için bkz. [Azure kaynak grupları](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-infrastructure-resource-groups-guidelines/).

   4. Belirtin **konumu** depolama hesaplarınızı ve StorSimple veri Yöneticisi hizmetinizin barındırıldığı hizmetiniz için. StorSimple cihaz Yöneticisi hizmetine veri Yöneticisi hizmeti ve ilişkili depolama hesabı tüm desteklenen bölgelerde olmalıdır.
    
   5. Panonuzda bu hizmetin bağlantısını ulaşmak için **panoya Sabitle**.
    
   6. **Oluştur**’a tıklayın.

      ![StorSimple veri Yöneticisi hizmeti 3 oluşturma](./media/storsimple-data-manager-ui/create-service-4.png)

Hizmetin oluşturulması birkaç dakika sürer. Hizmet başarıyla oluşturulduktan sonra yeni hizmet görüntülenen bir bildirim görür.

### <a name="create-a-data-transformation-job-definition"></a>Bir veri dönüşüm işi tanımı oluşturun

Bir StorSimple veri Yöneticisi hizmetinde bir veri dönüşüm işi tanımını oluşturmanız gerekir. Bir iş tanımı yerel biçiminde bir depolama hesabına taşıma ilgilendiğiniz StorSimple veri ayrıntılarını belirtir. Bir iş tanımı oluşturduğunuzda, daha sonra bu iş yeniden farklı çalışma zamanı ayarları ile çalıştırabilirsiniz.

Bir iş tanımı oluşturmak için aşağıdaki adımları gerçekleştirin.

1. Oluşturduğunuz hizmetine gidin. Git **Yönetim > iş tanımları**.

2. Tıklayın **+ iş tanımı**.

    ![Tıklama + iş tanımı](./media/storsimple-data-manager-ui/create-job-definition-1.png)

3. İş tanımınızın için bir ad belirtin. Ad 3 ila 63 karakter uzunluğunda olabilir. Ad, büyük ve küçük harfler, sayılar ve kısa çizgi içerebilir.

4. İşinizin çalıştığı bir konum belirtin. Bu konum hizmeti dağıtıldığı konumdan farklı olabilir.

5. Tıklayın **kaynak** kaynak veri deposu belirtmek için.

    ![Kaynak veri deposunu Yapılandır](./media/storsimple-data-manager-ui/create-job-definition-2.png)

6. Bu yeni bir veri Yöneticisi hizmeti olduğundan, hiçbir veri depoları yapılandırılır. İçinde **yapılandırma veri kaynağı**, StorSimple 8000 serisi Cihazınızı ayrıntılarını ve istediğiniz verileri belirtin.

   Bir veri deposu olarak StorSimple cihaz Yöneticisi eklemek için tıklatın **yeni Ekle** veri deposu açılır ve ardından **veri deposu ekleme**.

    ![Yeni veri deposu ekleme](./media/storsimple-data-manager-ui/create-job-definition-3.png)
  
   1. Seçin **StorSimple 8000 serisi Manager** veri deposu türü.
    
   2. Kaynak veri deponuz için bir kolay ad girin.
    
   3. Açılan listeden, StorSimple cihaz Yöneticisi hizmetiniz ile ilişkili bir abonelik seçin.
    
   4. ' % S'adı için StorSimple cihaz Yöneticisi'nin sağlamak **kaynak**.

   5. Girin **hizmet verileri şifreleme** StorSimple cihaz Yöneticisi hizmeti için anahtar. 

      ![Kaynak veri deposu 1 yapılandırın](./media/storsimple-data-manager-ui/create-job-definition-4.png)

      Tıklayın **Tamam** işiniz bittiğinde. Bu, veri deposuna kaydeder. Bu StorSimple cihaz Yöneticisi'nde diğer iş tanımlarını, bu parametreleri yeniden girmeye gerek kalmadan yeniden kullanın. Tıkladıktan sonra birkaç saniye sürer **Tamam** açılan listede gösterilecek yeni oluşturulan kaynak veri deposu için.

7. İçin aşağı açılan listeden **veri deposu**, oluşturduğunuz veri deposu seçin. 

   1. İstediğiniz verileri içeren StorSimple 8000 serisi cihaz adını girin.

   2. Verilerinizin ilgi StorSimple cihazında bulunan birim adını belirtin.

   3. İçinde **filtre** ayrıntılarının, ilgilendiğiniz verilerinizi içeren kök dizinini girin _\MyRootDirectory\Data_ biçimi. Sürücü harflerini gibi _\C:\Data_ desteklenmez. Tüm dosya filtreleri de ekleyebilirsiniz.

   4. Veri dönüşümü hizmetini, anlık görüntüler Azure'a gönderilir veriler üzerinde çalışır. Bu işlemi çalıştırdığınızda (iş üzerindeki en son veriler için) bu işin komutunu her çalıştırdığınızda yedekleyin seçebilirsiniz veya (bazı arşivlenmiş veriler üzerinde çalışıyorsanız), bulutta son mevcut yedekleme kullanın.

   5. **Tamam** düğmesine tıklayın.

      ![Kaynak veri deposu 2 yapılandırın](./media/storsimple-data-manager-ui/create-job-definition-8.png)

8. Ardından, hedef veri deposu yapılandırılması gerekir. Depolama hesapları, hesaptaki bloblar dosyaları yerleştirmenin seçin. Açılır menüden seçin **yeni Ekle** ardından **ayarlarını yapılandırma**.

9. Eklemek istediğiniz hedef depo ve depoyla ilişkili diğer parametreler türünü seçin.

    Bir depolama hesabı türü hedef seçerseniz, abonelik bir kolay ad belirtebilirsiniz (belirleyin, hizmet veya diğer aynı) ve depolama hesabı.
        ![Hedef veri deposu 1 yapılandırın](./media/storsimple-data-manager-ui/create-job-definition-10.png)

    İş çalıştırıldığında bir depolama kuyruğu oluşturulur. Bu kuyruk, hazır olduğunda dönüştürülen bloblar hakkında iletilerle doldurulur. Bu kuyruğun adı, iş tanımının adıyla aynıdır.
    
10. Veri deposu ekledikten sonra birkaç dakika bekleyin.
    
    1. Açılan listeden hedefi olarak oluşturduğunuz depoyu seçin **hedef hesap adı**.

    2. Depolama türü, BLOB veya dosyalar seçin. Dönüştürülen verilerin bulunduğu depolama kapsayıcısının adını belirtin. **Tamam** düğmesine tıklayın.

        ![Hedef veri deposu depolama hesabı yapılandırın](./media/storsimple-data-manager-ui/create-job-definition-16.png)

11. Ayrıca, bir işi çalıştırmadan önce iş süresi tahmini sunmak için seçeneği de denetleyebilirsiniz. Tıklayın **Tamam** iş tanımı oluşturmak için. İş tanımınızın tamamlanmıştır. Bu iş tanımı birden çok kez aracılığıyla kullanıcı Arabirimi ile farklı çalışma zamanı ayarlarını kullanabilirsiniz.

    ![Tam iş tanımı](./media/storsimple-data-manager-ui/create-job-definition-13.png)

    Yeni oluşturulan iş tanımını, bu hizmet için iş tanımlarını listesine eklenir.

### <a name="run-the-job-definition"></a>İş tanımını Çalıştır

İş tanımında belirtilen depolama hesabına Storsimple'dan verileri taşımak ihtiyaç duyduğunuzda çalıştırmanız gerekir. Çalışma zamanında, bazı parametreler farklı belirtilebilir. Adımlar aşağıdaki gibidir:

1. StorSimple veri Yöneticisi'ni hizmetinizi seçin ve Git **Yönetim > iş tanımları**. Çalıştırmak istediğiniz iş tanımı tıklatıp seçin.
     
     ![Başlangıç işi 1 çalıştırın](./media/storsimple-data-manager-ui/start-job-run1.png)

2. Tıklayın **Şimdi Çalıştır**.
     
     ![Başlangıç işi 2 çalıştırma](./media/storsimple-data-manager-ui/start-job-run2.png)

3. Tıklayın **çalıştırma ayarları** bu proje için değiştirmek isteyebileceğiniz herhangi bir ayarı değiştirmek için. Tıklayın **Tamam** ve ardından **çalıştırma** işinizi başlatmak için.

    ![Başlangıç işi 3 çalıştırın](./media/storsimple-data-manager-ui/start-job-run3.png)

4. Bu işi izlemek için Git **işleri** , StorSimple veri Yöneticisi'nde. İzleme yanı sıra **işleri** dikey penceresinde, de dinlemesi burada bir ileti eklendiğinde bir dosya depolama hesabına Storsimple'dan taşınır her zaman depolama kuyruğu.

    ![Başlangıç işi 4 çalıştırın](./media/storsimple-data-manager-ui/start-job-run4.png)


## <a name="next-steps"></a>Sonraki adımlar

[StorSimple veri Yöneticisi işleri başlatmak için .NET SDK'sını kullanma](storsimple-data-manager-dotnet-jobs.md).