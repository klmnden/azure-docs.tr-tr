---
title: "Bir iş tetiklemek için Azure Otomasyonu kullanın | Microsoft Docs"
description: "StorSimple veri Yöneticisi işleri (özel olarak incelenmektedir) tetiklemek için Azure Otomasyonu kullanmayı öğrenin"
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
ms.date: 03/16/2017
ms.author: vidarmsft
ms.openlocfilehash: 9691408bcd80afb6eba534f26749b76dd3bfe315
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-azure-automation-to-trigger-a-job-private-preview"></a>Azure Otomasyonu (özel Önizleme) bir iş tetiklemek için kullanın

Bu makaleler Azure Automation StorSimple Data Manager iş tetiklemek için nasıl kullanılacağını açıklar.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunları yapın:

*   Azure Powershell yüklü. [Azure Powershell indirme](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
*   Veri dönüştürme işlemini başlatmak için yapılandırma ayarlarını (Bu ayarları almak için yönergeleri dahil burada).
*   Bir kaynak grubu içinde karma veri kaynağındaki doğru şekilde yapılandırılmış bir iş tanımı.
*   Karşıdan `DataTransformationApp.zip` [zip](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/raw/master/Azure%20Automation%20For%20Data%20Manager/DataTransformationApp.zip) github deposuna dosyasından.
*   Karşıdan `Get-ConfigurationParams.ps1` [betik](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Get-ConfigurationParams.ps1) github'da depodan.
*   Karşıdan `Trigger-DataTransformation-Job.ps1` [betik](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Trigger-DataTransformation-Job.ps1) github'da depodan.

## <a name="step-by-step"></a>Adım adım

### <a name="get-azure-active-directory-permissions-for-the-automation-job-to-run-the-job-definition"></a>Azure Active Directory izinlerini iş tanımı çalıştırmak Otomasyonu işini alın

1. Active Directory için yapılandırma parametreleri almak için aşağıdaki adımları uygulayın:

    1. Windows PowerShell'i yerel makinenizde açın. Emin [Azure PowerShell](https://azure.microsoft.com/downloads/) yüklenir.
    1. Çalıştırma `Get-ConfigurationParams.ps1` komut dosyası (yukarıda indirdiğiniz klasörde). PowerShell penceresinde aşağıdaki komutu yazın:

        ```
        .\Get-ConfigurationParams.ps1 -SubscriptionName "AzureSubscriptionName" -ActiveDirectoryKey "AnyRandomPassword" -AppName "ApplicationName"
         ```

        ActiveDirectoryKey daha sonra kullandığınız paroladır. Tercih ettiğiniz bir parola girin. AppName herhangi bir dize olabilir.

2. Bu komut dosyası Otomasyon runbook'u tetikleyen yüklenirken kullanılması gereken aşağıdaki değerleri çıkarır. Bu değerleri not edin.

    - İstemci kimliği
    - Kiracı Kimliği
    - Active Directory anahtar (aynı Yukarıda girilen)
    - Abonelik Kimliği

### <a name="set-up-the-automation-account"></a>Otomasyon hesabı ayarlayın

1. Azure'da oturum açın ve Automation hesabınızı açın.
2. Tıklatın **varlıklar** döşeme varlıklar listesini açın.
3. Tıklatın **modülleri** modüllerin listesini açmak için kutucuğa.
4. Tıklatın **+ bir modül Ekle** düğmesi ve Ekle modülü dikey başlatıldığında.

    ![Otomasyon hesabı ayarları](./media/storsimple-data-manager-job-using-automation/add-module1m.png)

5. Seçtikten sonra `DataTransformationApp.zip` dosya yerel bilgisayarınızdan, tıklatın **Tamam** modülü içeri aktarmak için.

   Azure Automation hesabınız için bir modül aldığında modülüyle ilgili meta verileri ayıklar. Bu işlem birkaç dakika sürebilir.

   ![Otomasyon hesabı ayarları](./media/storsimple-data-manager-job-using-automation/add-module2m.png)

   

6. İşlem tamamlandığında, modül dağıtıldığı bir bildirim ve başka bir bildirim alırsınız.  Durum ayrıca kontrol edebilirsiniz **modülleri** döşeme.

### <a name="to-import-the-runbook-that-triggers-the-job-definition"></a>İş tanımı tetikler runbook'u içeri aktarma

1. Azure portalında, Otomasyon hesabınızı açın.
2. Tıklatın **Runbook'lar** listesini açmak için döşeme.
3. Tıklatın **+ runbook Ekle** ve ardından **mevcut bir runbook'u içeri aktar**.

   ![Mevcut bir runbook'u İçeri Aktar](./media/storsimple-data-manager-job-using-automation/import-a-runbook.png)

4. Tıklatın **Runbook dosyası** ve alınacak dosya seçin `Trigger-DataTransformation-Job.ps1`.
5. Tıklatın **oluşturma** runbook'u içeri aktarma. Yeni runbook Otomasyon hesabı için runbook'ları listesi görüntülenir.
7. Tıklatın **tetikleyici DataTransformation iş** runbook ve ardından **Düzenle**.
8. Tıklatın **Yayımla** ve ardından **Evet** onaylamanız istendiğinde.


### <a name="to-run-the-runbook"></a>Runbook'u çalıştırmak için:
1. Azure portalında, Otomasyon hesabınızı açın.
2. Runbook'ların listesini açmak için **Runbook'lar** kutucuğuna tıklayın.
3. Tıklatın **tetikleyici DataTransformation iş**.
4. Runbook'u başlatmak için **Başlat**’a tıklayın.

   ![Runbook’u başlatma](./media/storsimple-data-manager-job-using-automation/run-runbook1m.png)

5. İçinde **Başlat runbook** dikey penceresinde tüm parametreleri girin. Tıklatın **Tamam** veri dönüştürme işi göndermek için.

   ![Runbook’u başlatma](./media/storsimple-data-manager-job-using-automation/run-runbook2m.png)


## <a name="next-steps"></a>Sonraki adımlar

[StorSimple veri Yöneticisi'ni kullanın, verilerinizi dönüştürmek için kullanıcı Arabirimi](storsimple-data-manager-ui.md).