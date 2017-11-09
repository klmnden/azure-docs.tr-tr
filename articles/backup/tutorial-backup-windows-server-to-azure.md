---
title: "Azure için Windows Server Yedekleme | Microsoft Docs"
description: "Şirket içi Windows sunucularını bir kurtarma Hizmetleri kasasına yedekleme Bu öğretici ayrıntıları."
services: back up
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
keywords: "Windows server yedekleme; Windows server'ı Yedekle; Yedekleme ve olağanüstü durum kurtarma"
ms.assetid: 
ms.service: back up
ms.workload: storage-back up-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/23/2017
ms.author: saurabhsensharma;markgal;
ms.custom: 
ms.openlocfilehash: 7caf1dd3fa5ef295c2472cc11deb2895fc2a7111
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2017
---
# <a name="back-up-windows-server-to-azure"></a>Windows Server’ı Azure’da Yedekleme


Windows Server'ınızın bozulmaları, saldırıları ve olağanüstü korumak için Azure Backup kullanabilirsiniz. Azure Backup Microsoft Azure kurtarma Hizmetleri (MARS) aracısı olarak bilinen basit bir araç sağlar. MARS Aracısı'nı, dosya ve klasörleri ve Windows Server sistem durumu aracılığıyla sunucu yapılandırma bilgilerini korumak için Windows Server yüklenir. Bu öğretici, Azure için Windows Sunucunuzu Yedekleme için MARS Aracısı nasıl kullanabileceğiniz açıklanır. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz: 


> [!div class="checklist"]
> * Karşıdan yükleme ve MARS aracısını ayarlama
> * Geri süreleri yapılandırmak ve sunucunuzun yedeklemeler için bekletme zamanlaması
> * Bir geçici yedekleme gerçekleştirin


## <a name="log-in-to-azure"></a>Azure'da oturum açma

http://portal.azure.com sayfasından Azure portalda oturum açın.

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

Windows Server Yedekleme önce yedeklemeler için yer oluşturun veya depolanması için geri yükleme noktalarını gerekir. A [kurtarma Hizmetleri kasası](backup-azure-recovery-services-vault-overview.md) Windows Server'ınızın yedeklemelerden depolar azure'da bir kapsayıcıdır. Azure portalında bir kurtarma Hizmetleri kasası oluşturmak için aşağıdaki adımları izleyin. 

1. Sol taraftaki menüden seçin **daha fazla hizmet** ve Hizmetler listesinde yazın **kurtarma Hizmetleri**. Tıklatın **kurtarma Hizmetleri kasaları**.

   ![Açık kurtarma Hizmetleri kasası](./media/tutorial-backup-windows-server-to-azure/full-browser-open-rs-vault.png)

2.  **Kurtarma Hizmetleri kasaları** menüsünde **Ekle**'ye tıklayın.

   ![Kasa için bilgileri sağlayın](./media/tutorial-backup-windows-server-to-azure/provide-vault-detail-2.png)

3.  İçinde **kurtarma Hizmetleri kasası** menüsünde

    - Tür *myRecoveryServicesVault* içinde **adı**.
    - Geçerli abonelik kimliği görünür **abonelik**.
    - İçin **kaynak grubu**seçin **var olanı kullan** ve *myResourceGroup*. Varsa *myResourceGroup* mevcut değil, seçin **Yeni Oluştur** ve türü *myResourceGroup*. 
    - Gelen **konumu** açılır menü seçin *Batı Avrupa*.
    - Tıklatın **oluşturma** , Kurtarma Hizmetleri kasası oluşturmak için.
 
Kasanız oluşturulduktan sonra Kurtarma Hizmetleri kasaları listesinde görünür.

## <a name="download-recovery-services-agent"></a>Kurtarma Hizmetleri Aracısı'nı indirme

Microsoft Azure kurtarma Hizmetleri (MARS) Aracısı'nı Windows Server ve kurtarma Hizmetleri kasanız arasında bir ilişki oluşturur. Aşağıdaki yordam, sunucunuza Aracısı'nı indirme açıklanmaktadır.

1.  Kurtarma Hizmetleri kasalarının listesinden seçin **myRecoveryServicesVault** kendi panosunu açın.

   ![Kasa için bilgileri sağlayın](./media/tutorial-backup-windows-server-to-azure/open-vault-from-list.png)

2.  Kasa Panosu menüsünden tıklatın **yedekleme**.

3.  Üzerinde **yedekleme hedefi** menüsü:

    - için **, iş yükünü çalıştırdığı?**seçin**şirket içi**, 
    - için **neleri yedeklemek istiyorsunuz?**seçin **dosya ve klasörleri** ve **sistem durumu** 

    ![Kasa için bilgileri sağlayın](./media/tutorial-backup-windows-server-to-azure/backup-goal.png)
    
4.  Tıklatın **altyapıyı hazırlama** açmak için **altyapıyı hazırlama** menüsü.
5.  Üzerinde **altyapıyı hazırlama** menüsünde tıklatın **karşıdan aracısı için Windows Server veya Windows İstemcisi** indirmek için *MARSAgentInstaller.exe*. 

    ![altyapıyı hazırlama](./media/tutorial-backup-windows-server-to-azure/prepare-infrastructure.png)

    Yükleyici ayrı bir tarayıcı açar ve yüklemeleri **MARSAgentInstaller.exe**.
 
6.  İndirilen Dosya çalıştırmadan önce tıklatın **karşıdan** yükleyip kaydetmek için altyapıyı hazırlama dikey penceresinde düğmesinde **kasa kimlik bilgileri** dosya. Bu dosya, MARS Aracısı kurtarma Hizmetleri kasası ile bağlanmak için gereklidir.

    ![altyapıyı hazırlama](./media/tutorial-backup-windows-server-to-azure/download-vault-credentials.png)
 
## <a name="install-and-register-the-agent"></a>Aracıyı yükleme ve kaydetme

1. Bulun ve indirilen çift **MARSagentinstaller.exe**.
2. **Microsoft Azure kurtarma Hizmetleri Aracısı Kurulum Sihirbazı** görüntülenir. Sihirbazda ilerledikçe, istendiğinde aşağıdaki bilgileri sağlayın ve tıklayın **kaydetmek**.
    - Yükleme ve önbellek klasörünün konumu.
    - Internet'e bağlanmak üzere bir proxy sunucu kullanıyorsanız, proxy sunucu bilgileri.
    - Doğrulanmış bir proxy kullanıyorsanız kullanıcı adı ve parola bilgilerinizi.

    ![altyapıyı hazırlama](./media/tutorial-backup-windows-server-to-azure/mars-installer.png) 

3. Sihirbazın sonunda'i **devam etmek için kayıt** ve sağlamak **kasa kimlik bilgileri** önceki yordamda yüklediğiniz dosya.
 
4. İstendiğinde, Windows Server yedeklemeleri şifrelemek için bir şifreleme parolası sağlayın. Kayıp ise Microsoft parolayı kurtaramazsınız olarak parolayı güvenli bir konuma kaydedin.

5. **Son**'a tıklayın. 

## <a name="configure-backup-and-retention"></a>Yedekleme ve bekletme yapılandırma

Azure yedeklemeleri ortaya çıktığında Windows Server'da zamanlamak için Microsoft Azure kurtarma Hizmetleri Aracısı'nı kullanın. Aracı indirdiğiniz sunucusunda aşağıdaki adımları yürütün.

1. Microsoft Azure Kurtarma Hizmetleri aracısını açın. Bunu, makinenizde **Microsoft Azure Backup** aramasını yaparak bulabilirsiniz.

2.  Kurtarma Hizmetleri Aracısı konsolunda **yedekleme zamanlaması** altında **Eylemler bölmesi**.

    ![altyapıyı hazırlama](./media/tutorial-backup-windows-server-to-azure/mars-schedule-backup.png)

3. Tıklatın **sonraki** gitmek için **yukarı geri öğelerine seçin** sayfası.

4. Tıklatın **öğeleri Ekle** ve açılan iletişim kutusundan seçin **sistem durumu** ve dosya veya de yedeklemek istediğiniz klasörleri. Daha sonra, **Tamam**'a tıklayın.

5. **İleri**’ye tıklayın.

6. Üzerinde **yedekleme zamanlamasını belirtin (sistem durumu)** sayfasında, gün veya hafta yedeklemeleri gerektiğinde için sistem durumu tetiklenmesi tıklatıp süresini belirtin **sonraki** 

7.  Üzerinde **bekletme ilkesi seçin (sistem durumu)** sayfasında yedekleme kopyası için sistem durumu için bekletme ilkesi seçin ve tıklayın **sonraki**
8. Benzer şekilde, seçili dosya ve klasörler için yedekleme zamanlaması ve bekletme ilkesi seçin. 
8.  Üzerinde **seçin ilk yedekleme türünü** sayfasında, seçeneğini bırakın **otomatik olarak ağ üzerinden** seçili ve ardından **sonraki**.
9.  Üzerinde **onay** sayfasında, bilgileri gözden geçirin ve ardından **son**.
10. Sihirbaz yedekleme zamanlamasını oluşturduktan sonra **Kapat**'a tıklayın.

## <a name="perform-an-ad-hoc-back-up"></a>Bir geçici yedekleme gerçekleştirin

Yedekleme işleri çalıştırdığınızda, zamanlama oluşturulur. Ancak, siz sunucusunda yedeklediğiniz değil. Bu veri dayanıklılığı sunucunuz için emin olmak için bir isteğe bağlı yedekleme çalıştırmak için bir olağanüstü durum kurtarma en iyi uygulamadır.

1.  Microsoft Azure kurtarma Hizmetleri Aracısı konsolunda **Şimdi Yedekle**.

    ![altyapıyı hazırlama](./media/tutorial-backup-windows-server-to-azure/mars-schedule-backup.png)

2.  Üzerinde **Şimdi Yedekle** seçin makineden **dosya ve klasörleri** veya **sistem durumu** yedeklemek ve tıklatın istediğiniz **sonraki** 
3. Üzerinde **onay** sayfasında, ayarları gözden geçirin, **Şimdi Yedekle** Sunucunuzu Yedekleme için Sihirbazı'nı kullanır. Ardından **Yedekle**'ye tıklayın.
4.  Sihirbazı kapatmak için **Kapat**'a tıklayın. Sihirbaz yedekleme işlemi tamamlanmadan önce kapatırsanız, sihirbaz arka planda çalışmaya devam eder.
4.  İlk Yedekleme tamamlandıktan sonra **işi tamamlandı** durumu görünür **işleri** MARS Aracısı konsolunun bölmesinde.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide Azure portalına kullanılır: 
 
> [!div class="checklist"] 
> * Kurtarma Hizmetleri kasası oluşturma 
> * Microsoft Azure kurtarma Hizmetleri Aracısı'nı indirme 
> * Aracıyı yükleyin 
> * Windows Server için yedekleme yapılandırın 
> * Bir talep üzerine yedekleme gerçekleştirin 

Dosyalar, Windows Server için Azure yedeklemeden kurtarmak için sonraki öğretici devam

> [!div class="nextstepaction"] 
> [Dosyaları, Windows Server Azure'dan geri yükle](./tutorial-backup-restore-files-windows-server.md) 

