---
title: "Azure Yedekleme aracısı (MARS Aracısı) sorunlarını giderme | Microsoft Docs"
description: "Yükleme & Azure yedekleme Aracısı'nın kayıt sorunlarını giderme"
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
ms.openlocfilehash: da297177ca56822f7d50dbe06ec022640bf5d0cb
ms.sourcegitcommit: 7f1ce8be5367d492f4c8bb889ad50a99d85d9a89
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2017
---
# <a name="troubleshooting-azure-backup-agent-configurationregistration-issues"></a>Azure yedekleme Aracısı Yapılandırma/kayıt sorunlarını giderme
## <a name="recommended-steps"></a>Önerilen adımlar
Yapılandırma veya Azure yedekleme Aracısı'nın kayıt sırasında görülen ilgili hataları gidermek için aşağıda belirtilen önerilen eylemleri bakın.

## <a name="error-invalid-vault-credentials-provided-the-file-is-either-corrupted-or-does-not-have-the-latest-credentials-associated-with-recovery-service"></a>Hata: Geçersiz kasa kimlik bilgileri sağlandı. Dosya bozuk veya mu sahip en yeni kimlik ilişkilendirilmemiş kurtarma hizmeti ile.
| Hata ayrıntıları | Olası nedenler | Önerilen eylemleri |
| ---     | ---     | ---    |
| **Hata** </br> *Geçersiz kasa kimlik bilgileri sağlandı. Dosya bozuk veya mu sahip en yeni kimlik ilişkilendirilmemiş kurtarma hizmeti ile. (KİMLİK: 34513)* | <ol><li> Kasa kimlik bilgilerini (yani. geçerli değil Bunlar 48 saatten kayıt süreden önce indirildi).<li>   Azure Yedekleme aracısı Windows Temp klasörüne geçici dosya indirme mümkün değil. <li>Kasa kimlik bilgileri bir ağ konumuna bağlıdır. <li>TLS 1.0 devre dışı<li> Yapılandırılan proxy sunucusu bağlantıyı engelliyor <br> |  <ol><li>Yeni kasa kimlik bilgilerini indirme<li>Internet Seçenekleri'na > Güvenlik > Internet > Özel düzey > Dosya Yükleme bölümüne bakın ve Etkinleştir'i seçin kadar kaydırın.<li>Ayrıca siteye eklemek zorunda kalabilirsiniz [Güvenilen siteler](https://docs.microsoft.com/en-us/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements).<li>Proxy sunucu kullan ve Ara sunucu bilgilerini vermek için ayarlarını değiştirme <li> Tarih ve saat (UTC) makinenizle eşleşmesi<li>C:/Windows/Temp gidin ve birden fazla 60.000 veya 65.000 dosyaları (.tmp uzantılı) olup olmadığını denetleyin ve bu dosyaları silin.<li>Bu sunucu üzerinde SDP paket çalıştırarak test edebilirsiniz. Belirten dosya yüklemeleri hata alırsanız, çok sayıda dosya C:/Windows/Temp dizininde hakkında makul şekilde emin olabilirsiniz izin verilmez.<li>.NET framework 4.6.2 yüklü olduğundan emin olun <li>PCI uyumluluğunu nedeniyle TLS 1.0 devre dışıysa için başvuruda [bağlantı sorunlarını gidermek](https://support.microsoft.com/help/4022913). <li>Sunucuda yüklü bir virüsten koruma varsa, aşağıdaki dosyaların virüsten koruma tarama dışında tut. <ol><li>CBengine.exe<li>CSC.exe (.NET Framework ile ilgili. Sunucuda yüklü .NET sürüm başına CSC.exe yoktur. .NET framework etkilenen sunucudaki tüm sürümleri bağlı tüm CSC.exe dosyalarını hariç tutun.) <li>Klasör veya önbellek konumu boş. <br>*Geçici klasör veya önbellek konumu yolu için varsayılan konumu C:\Program Files\Microsoft Azure kurtarma Hizmetleri Agent\Scratch:*

## <a name="error-the-microsoft-azure-recovery-service-agent-was-unable-to-connect-to-microsoft-azure-backup"></a>Hata: Microsoft Azure kurtarma hizmeti Aracısı Microsoft Azure Yedekleme'ye bağlanamadı

| Hata ayrıntıları | Olası nedenler | Önerilen eylemleri |
| ---     | ---     | ---    |
| **Hata** </br><ol><li>*Microsoft Azure kurtarma hizmeti Aracısı Microsoft Azure Yedekleme'ye bağlanamadı. (KİMLİK: 100050) Ağ ayarlarınızı denetleyin ve Internet'e bağlanabildiğinizden emin olun*<li>*(407) proxy kimlik doğrulaması gerekli* |Proxy bağlantı * engelleme |  <ol><li>Internet Seçenekleri'na > Güvenlik > Internet > Özel düzey > Dosya Yükleme bölümüne bakın ve Etkinleştir'i seçin kadar kaydırın.<li>Ayrıca siteye eklemek zorunda kalabilirsiniz [Güvenilen siteler](https://docs.microsoft.com/en-us/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements).<li>Proxy sunucu kullan ve Ara sunucu bilgilerini vermek için ayarlarını değiştirme <li>Sunucuda yüklü bir virüsten koruma varsa, aşağıdaki dosyaların virüsten koruma tarama dışında tut. <ol><li>CBengine.exe<li>(dpmra.exe yerine)<li>CSC.exe (.NET Framework ile ilgili. Sunucuda yüklü .NET sürüm başına CSC.exe yoktur. .NET framework etkilenen sunucudaki tüm sürümleri bağlı tüm CSC.exe dosyalarını hariç tutun.) <li>Klasör veya önbellek konumu boş <br>*Geçici klasör veya önbellek konumu yolu için varsayılan konumu C:\Program Files\Microsoft Azure kurtarma Hizmetleri Agent\Scratch:*

## <a name="error-failed-to-set-the-encryption-key-for-secure-backups"></a>Hata: Güvenli yedeklemeler için şifreleme anahtarı ayarlanamadı

| Hata ayrıntıları | Olası nedenler | Önerilen eylemleri |
| ---     | ---     | ---    |      
| **Hata** </br>*Güvenli yedeklemeler için şifreleme anahtarı ayarlanamadı. Geçerli işlem bir iç hizmet hatası 'geçersiz giriş hatası nedeniyle' başarısız oldu. Lütfen bir süre sonra işlemi yeniden deneyin. Sorun devam ederse, lütfen Microsoft Destek'e başvurun* |Sunucu zaten başka bir kasaya kayıtlı| Kaydı sunucu kasası ve yeniden kaydedin.

## <a name="error-the-activation-did-not-complete-successfully-the-current-operation-failed-due-to-an-internal-service-error-0x1fc07"></a>Hata: Etkinleştirme başarıyla tamamlanamadı. Geçerli işlem bir iç hata nedeniyle [0x1FC07] başarısız oldu

| Hata ayrıntıları | Olası nedenler | Önerilen eylemleri |
| ---     | ---     | ---    |          
| **Hata** </br><ol><li>*Etkinleştirme başarıyla tamamlanamadı. Geçerli işlem bir iç hata nedeniyle [0x1FC07] başarısız oldu. Lütfen bir süre sonra işlemi yeniden deneyin. Sorun devam ederse, lütfen Microsoft Destek'e başvurun* <li>*Hata 34506. Bu bilgisayarda depolanan şifreleme parolası düzgün yapılandırılmamış* | <li> Geçici klasör yeterli alana sahip bir birimde bulunuyor <li> Geçici klasör yanlış başka bir konuma taşındı <li> OnlineBackup.KEK dosyası eksik | <li>Geçici klasör veya önbellek konumu yedek verilerin toplam boyutu 5-%10 eşdeğer boş alanı olan bir birim taşıyın. Belirtilen adımları başvurmak [bu makalede](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup) doğru önbellek konumunu taşımak için.<li> OnlineBackup.KEK dosyasının var olduğundan emin olun <br>*Geçici klasör veya önbellek konumu yolu için varsayılan konumu C:\Program Files\Microsoft Azure kurtarma Hizmetleri Agent\Scratch:*

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun
Hala yardıma gereksiniminiz varsa [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) hızla çözümlenen sorunu almak için.

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla ayrıntı elde [Azure Yedekleme aracısı, Windows Server Yedekleme](tutorial-backup-windows-server-to-azure.md).
* Bir yedeklemeyi geri yüklemeniz gerekirse [dosyaları bir Windows makinesine geri yüklemek](backup-azure-restore-windows-server.md) için bu makaleyi kullanın.
