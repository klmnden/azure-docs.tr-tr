---
title: StorSimple veri Yöneticisi'nde bir proje başlatmak için Azure Otomasyonu kullanma | Microsoft Docs
description: StorSimple veri Yöneticisi işleri tetiklemek için Azure Automation'ı kullanmayı öğrenin
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
ms.openlocfilehash: b837aab871827c468295a365727a282f6c8a1a4b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60634259"
---
# <a name="use-azure-automation-to-trigger-a-job"></a>Bir iş tetiklemek için Azure Otomasyonu kullanma

Bu makalede, StorSimple cihaz verileri dönüştürmek için veri dönüştürme özelliği StorSimple veri Yöneticisi hizmeti içinde nasıl kullanabileceğiniz açıklanmaktadır. Bir veri dönüşüm işi iki yolla başlatabilirsiniz: 

 - .NET SDK’yı kullanma
 - Azure Otomasyonu runbook'u kullanın
 
Bu makalede, Azure Otomasyonu runbook'u oluşturma ve bir veri dönüşüm işi başlatmak için kullanma işlemi açıklanmaktadır. .NET SDK'sı üzerinden veri dönüştürme başlatma hakkında daha fazla bilgi için şuraya gidin [kullanım .NET SDK'sı tetikleyici veri dönüştürme işleri](storsimple-data-manager-dotnet-jobs.md).

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce şunları yapın:

*   Azure PowerShell istemci bilgisayarda yüklü. [Azure PowerShell indirme](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps).
*   Bir kaynak grubu içindeki bir StorSimple veri Yöneticisi hizmeti doğru yapılandırılmış iş tanımında.
*   İndirme [ `DataTransformationApp.zip` ](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/raw/master/Azure%20Automation%20For%20Data%20Manager/DataTransformationApp.zip) GitHub deposundan dosya. 
*   İndirme [ `Trigger-DataTransformation-Job.ps1` ](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Trigger-DataTransformation-Job.ps1) GitHub deposundan bir betik.

## <a name="step-by-step-procedure"></a>Adım adım yordam

### <a name="set-up-the-automation-account"></a>Otomasyon hesabı ayarlayın

1. Azure portalında bir Azure farklı çalıştır Otomasyon hesabı oluşturun. Bunu yapmak için Git **Azure Market > her şeyi** bulun **Otomasyon**. Seçin **Otomasyon hesapları**.

    ![Farklı Çalıştır Otomasyon hesabı oluşturma](./media/storsimple-data-manager-job-using-automation/search-automation-account1.png)

2. Yeni bir Otomasyon hesabı eklemek için tıklatın **+ Ekle**.

    ![Farklı Çalıştır Otomasyon hesabı oluşturma](./media/storsimple-data-manager-job-using-automation/add-automation-account1.png)

3. İçinde **Otomasyon ekleme**:

   1. Tedarik **adı** Otomasyon hesabınızın.
   2. Seçin **abonelik** StorSimple veri Yöneticisi hizmetinize bağlı.
   3. Yeni bir kaynak grubu oluşturun veya mevcut bir kaynak grubundan'ı seçin.
   4. Bir **Konum** seçin.
   5. Varsayılan değeri bırakın **Çalıştır hesabı oluştur** seçeneği belirlenmiş.
   6. Panoda hızlı erişim için bir bağlantı almak için denetleyin **panoya Sabitle**. **Oluştur**’a tıklayın.

      ![Farklı Çalıştır Otomasyon hesabı oluşturma](./media/storsimple-data-manager-job-using-automation/create-automation-run-as-account.png)
    
      Otomasyon hesabı başarıyla oluşturulduktan sonra size bildirilir.
    
      ![Otomasyon hesabı dağıtımını bildirimi](./media/storsimple-data-manager-job-using-automation/deployment-automation-account-notification1.png)

      Daha fazla bilgi için Git [bir farklı çalıştır hesabı oluşturma](../automation/automation-create-runas-account.md).

3. Yeni oluşturulan hesabın Git **paylaşılan kaynaklar > modülleri** tıklatıp **+ Ekle Modülü**.

    ![1 modülünü içeri aktarın](./media/storsimple-data-manager-job-using-automation/import-module-1.png)

4. Konumuna gözatın `DataTransformationApp.zip` kullanarak yerel bilgisayarınızdan dosyasını seçin ve modülün açın. Tıklayın **Tamam** modülü içeri aktarın.

    ![İçeri aktarma modülü 2](./media/storsimple-data-manager-job-using-automation/import-module-2.png)

   Azure Automation hesabınız için bir modül aktardığında, modül hakkındaki meta verileri ayıklar. Bu işlem birkaç dakika sürebilir.

   ![4 modülünü içeri aktarın](./media/storsimple-data-manager-job-using-automation/import-module-4.png)

5. İşlem tamamlandığında, modül dağıtılmakta bir bildirim ve başka bir bildirim alırsınız.  Durum **modülleri** değişikliklerini **kullanılabilir**.

    ![5 modülünü içeri aktarın](./media/storsimple-data-manager-job-using-automation/import-module-5.png)

### <a name="import-publish-and-run-automation-runbook"></a>İçeri aktarma, yayınlama ve Otomasyon runbook'u çalıştırma

İçeri aktarma, yayınlama ve iş tanımını tetikleyeceğini runbook'u çalıştırmak için aşağıdaki adımları gerçekleştirin.

1. Azure portalında, Otomasyon hesabınızı açın. Git **süreç otomasyonu > runbook'ları** tıklatıp **+ runbook Ekle**.

    ![Runbook 1 Ekle](./media/storsimple-data-manager-job-using-automation/add-runbook-1.png)

2. İçinde **runbook Ekle**, tıklayın **mevcut bir runbook'u içeri aktar**.

3. Azure PowerShell betik dosyasına işaret `Trigger-DataTransformation-Job.ps1` için **Runbook dosyası**. Runbook türü otomatik olarak seçilir. Bir ad ve runbook için isteğe bağlı bir açıklama sağlayın. **Oluştur**’a tıklayın.

    ![Runbook 2 Ekle](./media/storsimple-data-manager-job-using-automation/add-runbook-2.png)

4. Yeni runbook Otomasyon hesabı için runbook'ların listesi görüntülenir. Seçin ve bu runbook'ı tıklatın.

    ![Runbook 3 Ekle](./media/storsimple-data-manager-job-using-automation/add-runbook-3.png)

5. Runbook'u düzenleyebilir ve **Test** bölmesi.

    ![Runbook 4 Ekle](./media/storsimple-data-manager-job-using-automation/add-runbook-4.png)

6. StorSimple veri Yöneticisi hizmetiniz, ilişkili kaynak grubunu ve iş tanımı adının adı gibi parametreler belirtin. **Başlangıç** test. Çalıştırma tamamlandığında rapor oluşturulur. Nasıl daha fazla bilgi için Git [bir runbook'u test](../automation/automation-first-runbook-textual-powershell.md#step-3---test-the-runbook).

    ![Runbook 8 Ekle](./media/storsimple-data-manager-job-using-automation/add-runbook-8.png)    

7. Runbook'u test bölmesi çıktısı inceleyin. Memnun bölmesini kapatın. Tıklayın **Yayımla** ve onaylamanız istendiğinde, onaylayın ve runbook'u yayımlayamadı.

    ![Runbook 6 Ekle](./media/storsimple-data-manager-job-using-automation/add-runbook-6.png)

8. Geri Git **runbook'ları** ve yeni oluşturulan runbook seçin.

    ![Runbook 7 Ekle](./media/storsimple-data-manager-job-using-automation/add-runbook-7.png)

9. **Başlangıç** runbook. İçinde **Başlat runbook**, tüm parametreleri girin. Tıklayın **Tamam** gönderin ve veri dönüşüm işi başlatın.

10. Azure portalında iş ilerleme durumunu izlemek için Git **işleri** StorSimple veri Yöneticisi hizmetinizde. Seçin ve işi iş ayrıntılarını görüntülemek için tıklayın.

    ![Runbook 10 Ekle](./media/storsimple-data-manager-job-using-automation/add-runbook-10.png)

## <a name="next-steps"></a>Sonraki adımlar

[StorSimple veri Yöneticisi'ni kullanın, verilerinizi dönüştürmek için kullanıcı Arabirimi](storsimple-data-manager-ui.md).