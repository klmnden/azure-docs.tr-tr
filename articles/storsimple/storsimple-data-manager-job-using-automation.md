---
title: "Azure Otomasyonu işi StorSimple veri Yöneticisi'ni başlatmak için kullanın | Microsoft Docs"
description: "StorSimple veri Yöneticisi işleri tetiklemek için Azure Otomasyonu kullanmayı öğrenin"
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 01/16/2018
ms.author: alkohli
ms.openlocfilehash: 1e5fcbee664271058ac1c7fa80bb285e09b8579a
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2018
---
# <a name="use-azure-automation-to-trigger-a-job"></a>Bir iş tetiklemek için Azure Otomasyonu kullanın

Bu makalede, StorSimple cihaz verileri dönüştürmek için StorSimple veri Yöneticisi hizmeti içindeki veri dönüştürme özelliği nasıl kullanabileceğiniz açıklanır. İki yolla veri dönüştürme işi başlatabilirsiniz: 

 - .NET SDK’yı kullanma
 - Azure Otomasyonu runbook kullanın
 
Bu makale bir Azure Otomasyonu runbook'u oluşturma ve veri dönüştürme işi başlatmak için kullanmak ayrıntılarını verir. .NET SDK'sı üzerinden veri dönüştürme başlatmak hakkında daha fazla bilgi için şuraya gidin [kullanım .NET SDK'sı tetikleyici veri dönüştürme işleri](storsimple-data-manager-dotnet-jobs.md).

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce şunları yapın:

*   Azure PowerShell istemci bilgisayarda yüklü. [Azure PowerShell indirme](https://docs.microsoft.com/powershell/azure/install-azurerm-ps).
*   Bir kaynak grubu içinde bir StorSimple veri Yöneticisi hizmeti düzgün yapılandırılmış iş tanımında.
*   Karşıdan [ `DataTransformationApp.zip` ](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/raw/master/Azure%20Automation%20For%20Data%20Manager/DataTransformationApp.zip) GitHub deposuna dosyasından. 
*   Karşıdan [ `Trigger-DataTransformation-Job.ps1` ](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Trigger-DataTransformation-Job.ps1) GitHub deposuna betikten.

## <a name="step-by-step-procedure"></a>Adım adım yordam

### <a name="set-up-the-automation-account"></a>Otomasyon hesabı ayarlayın

1. Azure portalında bir Azure farklı çalıştır Otomasyon hesabı oluşturun. Bunu yapmak için şu adrese gidin **Azure Market > her şeyi** arayın ve sonra **Otomasyon**. Seçin **Automation hesapları**.

    ![Automation farklı çalıştır hesabı oluşturma](./media/storsimple-data-manager-job-using-automation/search-automation-account1.png)

2. Yeni bir Otomasyon hesabı eklemek için tıklatın **+ Ekle**.

    ![Automation farklı çalıştır hesabı oluşturma](./media/storsimple-data-manager-job-using-automation/add-automation-account1.png)

3. İçinde **Otomasyonu ekleyin**:

    1. Tedarik **adı** Otomasyon hesabınızın.
    2. Seçin **abonelik** StorSimple veri Yöneticisi hizmetinize bağlı.
    3. Yeni bir kaynak grubu oluşturun veya varolan bir kaynak grubu seçin.
    4. Bir **Konum** seçin.
    5. Varsayılan adı bırakın **Çalıştır hesabı oluştur** seçeneği belirlenmiş.
    6. Panoda hızlı erişim için bir bağlantı almak üzere giriş **panoya Sabitle**. **Oluştur**’a tıklayın.

    ![Automation farklı çalıştır hesabı oluşturma](./media/storsimple-data-manager-job-using-automation/create-automation-run-as-account.png)
    
    Otomasyon hesabı başarıyla oluşturulduktan sonra size bildirilir.
    
    ![Otomasyon hesabı dağıtımını bildirimi](./media/storsimple-data-manager-job-using-automation/deployment-automation-account-notification1.png)

    Daha fazla bilgi için Git [bir farklı çalıştır hesabı oluşturma](../automation/automation-create-runas-account.md).

3. Yeni oluşturulan hesabında Git **paylaşılan kaynakları > modülleri** tıklatıp **+ Ekle Modülü**.

    ![İçeri aktarma Modül 1](./media/storsimple-data-manager-job-using-automation/import-module-1.png)

4. Konumuna göz atın `DataTransformationApp.zip` dosya yerel bilgisayarınızdan seçin ve modülü açın. Tıklatın **Tamam** modülü içeri aktarmak için.

    ![İçeri aktarma modülü 2](./media/storsimple-data-manager-job-using-automation/import-module-2.png)

   Azure Automation hesabınız için bir modül aldığında modülüyle ilgili meta verileri ayıklar. Bu işlem birkaç dakika sürebilir.

   ![İçeri aktarma modülü 4](./media/storsimple-data-manager-job-using-automation/import-module-4.png)

5. İşlem tamamlandığında, modül dağıtıldığı bir bildirim ve başka bir bildirim alırsınız.  Durum **modülleri** değişikliklerini **kullanılabilir**.

    ![İçeri aktarma modül 5](./media/storsimple-data-manager-job-using-automation/import-module-5.png)

### <a name="import-publish-and-run-automation-runbook"></a>İçeri aktarma, yayımlama ve Otomasyon runbook çalıştırma

İçeri aktarma, yayımlama ve iş tanımı tetiklemek için runbook'u çalıştırmak için aşağıdaki adımları gerçekleştirin.

1. Azure portalında, Otomasyon hesabınızı açın. Git **işlem Otomasyonu > Runbook'lar** tıklatıp **+ runbook Ekle**.

    ![Runbook 1 Ekle](./media/storsimple-data-manager-job-using-automation/add-runbook-1.png)

2. İçinde **runbook Ekle**, tıklatın **mevcut bir runbook'u içeri aktar**.

3. Azure PowerShell Betiği dosyanın üzerine `Trigger-DataTransformation-Job.ps1` için **Runbook dosyası**. Runbook türü otomatik olarak seçilir. Bir ad ve runbook için isteğe bağlı bir açıklama sağlayın. **Oluştur**’a tıklayın.

    ![Runbook 2 Ekle](./media/storsimple-data-manager-job-using-automation/add-runbook-2.png)

4. Yeni runbook Otomasyon hesabı için runbook'ları listesi görüntülenir. Seçin ve bu runbook'ı tıklatın.

    ![Runbook 3 Ekle](./media/storsimple-data-manager-job-using-automation/add-runbook-3.png)

5. Runbook'u düzenlemek ve tıklayın **Test** bölmesi.

    ![Runbook 4 Ekle](./media/storsimple-data-manager-job-using-automation/add-runbook-4.png)

6. StorSimple veri Yöneticisi hizmetiniz, ilişkili kaynağı grubu ve iş tanımı adını adı gibi parametreler belirtin. **Başlat** test. Rapor Çalıştır tamamlandığında oluşturulur. Nasıl daha fazla bilgi için Git [bir runbook'u test](../automation/automation-first-runbook-textual-powershell.md#step-3---test-the-runbook).

    ![Runbook 8 Ekle](./media/storsimple-data-manager-job-using-automation/add-runbook-8.png)    

7. Test Bölmesi'nda runbook'un çıkışı inceleyin. Karşılanan bölmesini kapatın. Tıklatın **Yayımla** ve onaylamanız istendiğinde, onaylayın ve runbook yayımlayabilirsiniz.

    ![Runbook 6 Ekle](./media/storsimple-data-manager-job-using-automation/add-runbook-6.png)

8. Geri dönerek **Runbook'lar** ve yeni oluşturulan runbook'u seçin.

    ![Add runbook 7](./media/storsimple-data-manager-job-using-automation/add-runbook-7.png)

9. **Başlat** runbook. İçinde **Başlat runbook**, tüm parametreleri girin. Tıklatın **Tamam** göndermek ve veri dönüştürme işlemi başlatmak için.

10. Azure portalında iş ilerleme durumunu izlemek için Git **işleri** StorSimple Data Manager hizmetinizde. Seçin ve iş ayrıntılarını görüntülemek için iş'i tıklatın.

    ![Runbook 10 Ekle](./media/storsimple-data-manager-job-using-automation/add-runbook-10.png)

## <a name="next-steps"></a>Sonraki adımlar

[StorSimple veri Yöneticisi'ni kullanın, verilerinizi dönüştürmek için kullanıcı Arabirimi](storsimple-data-manager-ui.md).