---
title: "Azure Yedekleme aracısı sorunlarını giderme | Microsoft Docs"
description: "Yükleme ve Azure yedekleme Aracısı'nın kayıt sorunlarını giderme"
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shreeshd
editor: 
ms.assetid: 778c6ccf-3e57-4103-a022-367cc60c411a
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/4/2017
ms.author: saurse;markgal;
<<<<<<< HEAD
ms.openlocfilehash: da297177ca56822f7d50dbe06ec022640bf5d0cb
ms.sourcegitcommit: 7f1ce8be5367d492f4c8bb889ad50a99d85d9a89
ms.translationtype: HT
=======
ms.openlocfilehash: 52a32d61dd69110ace560fd1e52389140f322678
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: MT
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2017
---
# <a name="troubleshoot-azure-backup-agent-configuration-and-registration-issues"></a>Azure Yedekleme aracısı yapılandırma ve kayıt sorunlarını giderme
## <a name="recommended-steps"></a>Önerilen adımlar
Aşağıdaki tablolarda yapılandırma ve Azure yedekleme Aracısı'nın kayıt sırasında karşılaşabileceğiniz hataları gidermek için önerilen eylemleri bakın.

## <a name="invalid-vault-credentials-provided-the-file-is-either-corrupted-or-does-not-have-the-latest-credentials-associated-with-recovery-service"></a>Geçersiz kasa kimlik bilgileri sağlandı. Dosya bozuk veya mu sahip en yeni kimlik ilişkilendirilmemiş kurtarma hizmeti ile.
| Hata ayrıntıları | Olası nedenler | Önerilen eylemler |
| ---     | ---     | ---    |
| **Hata** </br> *Geçersiz kasa kimlik bilgileri sağlandı. Dosya bozuk veya mu sahip en yeni kimlik ilişkilendirilmemiş kurtarma hizmeti ile. (KİMLİK: 34513)* | <ul><li> Kasa kimlik bilgileri geçersiz (diğer bir deyişle, bunlar 48 saatten kayıt tarihten önce yüklenen).<li>Azure Yedekleme aracısı Windows Temp klasörüne geçici dosya indirme mümkün değil. <li>Kasa kimlik bilgileri bir ağ konumuna bağlıdır. <li>TLS 1.0 devre dışı<li> Yapılandırılan proxy sunucusu bağlantıyı engelliyor. <br> |  <ul><li>Yeni kasa kimlik bilgilerini indirin.<li>Git **Internet Seçenekleri** > **güvenlik** > **Internet**. Ardından, **Özel düzey**ve bölüm karşıdan dosya görene kadar kaydırın. Ardından **etkinleştirmek**.<li>Siteye eklemek gerekebilir [Güvenilen siteler](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements).<li>Bir proxy sunucusu kullanmak üzere ayarları değiştirin. Proxy sunucu ayrıntıları sağlayın. <li> Tarih ve saat makinenizle eşleşmesi.<li>C:/Windows/Temp gidin ve birden fazla 60.000 veya 65.000 dosyaları .tmp uzantısına sahip olup olmadığını denetleyin. Varsa, bu dosyaları silin.<li>Bu sunucu üzerinde SDP paket çalıştırarak test edebilirsiniz. Dosya yüklemeleri izin verilmeyen bildiren bir hata alırsanız, çok sayıda dosya C:/Windows/Temp dizininde olduğunu olasılığı yüksektir.<li>.NET framework 4.6.2 yüklü olduğundan emin olun. <li>PCI uyumluluğunu nedeniyle TLS 1.0 devre dışıysa için başvuruda [sorun giderme sayfası](https://support.microsoft.com/help/4022913). <li>Virüsten koruma yazılımı sunucuda yüklü varsa, aşağıdaki dosyaların virüsten koruma tarama dışında tut: <ul><li>CBengine.exe<li>.NET Framework ile ilgili CSC.exe. Sunucuda yüklü her .NET sürüm için CSC.exe yoktur. .NET framework etkilenen sunucudaki tüm sürümleri bağlıdır CSC.exe dosyaları dışlayın. <li>Klasör veya önbellek konumu boş. <br>*Geçici klasör veya önbellek konumu yolu için varsayılan konumu C:\Program Files\Microsoft Azure kurtarma Hizmetleri Agent\Scratch:*.

## <a name="the-microsoft-azure-recovery-service-agent-was-unable-to-connect-to-microsoft-azure-backup"></a>Microsoft Azure kurtarma hizmeti Aracısı Microsoft Azure Yedekleme'ye bağlanamadı

| Hata ayrıntıları | Olası nedenler | Önerilen eylemler |
| ---     | ---     | ---    |
| **Hata** </br><ol><li>*Microsoft Azure kurtarma hizmeti Aracısı Microsoft Azure Yedekleme'ye bağlanamadı. (KİMLİK: 100050) Ağ ayarlarınızı denetleyin ve Internet'e bağlanabildiğinizden emin olun*<li>*(407) proxy kimlik doğrulaması gerekli* |Proxy bağlantıyı engelliyor. |  <ul><li>Na **Internet Seçenekleri** > **güvenlik** > **Internet**. Ardından **Özel düzey** ve bölüm karşıdan dosya görene kadar kaydırın. **Etkinleştir**’i seçin.<li>Siteye eklemek gerekebilir [Güvenilen siteler](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements).<li>Bir proxy sunucusu kullanmak üzere ayarları değiştirin. Proxy sunucu ayrıntıları sağlayın. <li>Virüsten koruma yazılımı sunucuda yüklü varsa, aşağıdaki dosyaların virüsten koruma tarama dışında tut. <ul><li>CBEngine.exe (yerine dpmra.exe).<li>CSC.exe (.NET Framework ile ilgili). Sunucuda yüklü her .NET sürüm için CSC.exe yoktur. .NET framework etkilenen sunucudaki tüm sürümleri bağlıdır CSC.exe dosyaları dışlayın. <li>Klasör veya önbellek konumu boş. <br>*Geçici klasör veya önbellek konumu yolu için varsayılan konumu C:\Program Files\Microsoft Azure kurtarma Hizmetleri Agent\Scratch:*.

## <a name="failed-to-set-the-encryption-key-for-secure-backups"></a>Güvenli yedeklemeler için şifreleme anahtarı ayarlanamadı

| Hata ayrıntıları | Olası nedenler | Önerilen eylemler |
| ---     | ---     | ---    |      
| **Hata** </br>*Güvenli yedeklemeler için şifreleme anahtarı ayarlanamadı. Geçerli işlem bir iç hizmet hatası 'geçersiz giriş hatası nedeniyle' başarısız oldu. Lütfen bir süre sonra işlemi yeniden deneyin. Sorun devam ederse, lütfen Microsoft desteğine başvurun*. |Sunucu, başka bir kasasına zaten kayıtlı.| Kasa sunucusundan kaydını silin ve yeniden kaydedin.

## <a name="the-activation-did-not-complete-successfully-the-current-operation-failed-due-to-an-internal-service-error-0x1fc07"></a>Etkinleştirme başarıyla tamamlanamadı. Geçerli işlem bir iç hata nedeniyle [0x1FC07] başarısız oldu

| Hata ayrıntıları | Olası nedenler | Önerilen eylemler |
| ---     | ---     | ---    |          
| **Hata** </br><ol><li>*Etkinleştirme başarıyla tamamlanamadı. Geçerli işlem bir iç hata nedeniyle [0x1FC07] başarısız oldu. Lütfen bir süre sonra işlemi yeniden deneyin. Sorun devam ederse, lütfen Microsoft Destek'e başvurun* <li>*Hata 34506. Bu bilgisayarda depolanan şifreleme parolası düzgün yapılandırılmamış*. | <li> Geçici klasör yeterli alana sahip bir birimde bulunur. <li> Geçici klasör yanlış başka bir konuma taşındı. <li> OnlineBackup.KEK dosyası eksik. | <li>Geçici klasör veya önbellek konumu yedek verilerin toplam boyutu 5-%10 eşdeğer boş alanı olan bir birim taşıyın. Önbellek konumunu doğru taşımak için adımlarda bakın [Azure Yedekleme aracısı hakkında sorular](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup).<li> OnlineBackup.KEK dosyasının var olduğundan emin olun. <br>*Geçici klasör veya önbellek konumu yolu için varsayılan konumu C:\Program Files\Microsoft Azure kurtarma Hizmetleri Agent\Scratch:*.

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun
Hala yardıma gereksiniminiz varsa [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) hızla çözümlenen sorunu almak için.

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla ayrıntı elde [Azure yedekleme Aracısı'nı Windows Server Yedekleme](tutorial-backup-windows-server-to-azure.md).
* Bir yedeklemeyi geri yüklemeniz gerekirse [dosyaları bir Windows makinesine geri yüklemek](backup-azure-restore-windows-server.md) için bu makaleyi kullanın.
