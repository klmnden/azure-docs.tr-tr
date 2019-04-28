---
title: StorSimple 8000 serisi destek paketi oluştur | Microsoft Docs
description: StorSimple 8000 serisi Cihazınızı için bir destek paketi oluşturabilecek, şifresini çözmek ve öğrenin.
services: storsimple
documentationcenter: ''
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/09/2018
ms.author: alkohli
ms.openlocfilehash: dfc2d8d763a1eb64a37af73e03992f2d948a6856
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61481878"
---
# <a name="create-and-manage-a-support-package-for-storsimple-8000-series"></a>Oluşturmak ve yönetmek için StorSimple 8000 serisi bir destek paketi

## <a name="overview"></a>Genel Bakış

StorSimple destek paketi Microsoft Support StorSimple cihaz sorunları gidermeye yardımcı olmak için tüm ilgili günlükleri toplayan bir kullanımı kolay mekanizmasıdır. Toplanan günlükler şifrelenmiş ve sıkıştırılmış.

Bu öğretici, oluşturmak ve yönetmek için StorSimple 8000 serisi Cihazınızı destek paketi için adım adım yönergeler içerir. StorSimple Virtual Array ile çalışıyorsanız, Git [bir günlük paketi oluşturun](storsimple-ova-web-ui-admin.md#generate-a-log-package).

## <a name="create-a-support-package"></a>Destek paketi oluşturma

Bazı durumlarda, el ile StorSimple için Windows PowerShell üzerinden destek paketi oluşturmanız gerekir. Örneğin:

* Günlük dosyalarınızın Microsoft Support paylaşımı önce önemli bilgileri kaldırmak gerekiyorsa.
* Bağlantı sorunları nedeniyle paket karşıya yükleme ile ilgili sorunlar yaşıyorsanız.

E-posta Microsoft Support, el ile oluşturulan destek paketinin paylaşabilirsiniz. Destek Paketi StorSimple için Windows PowerShell'de oluşturmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-create-a-support-package-in-windows-powershell-for-storsimple"></a>Destek Paketi StorSimple için Windows PowerShell'de oluşturmak için

1. StorSimple Cihazınızı bağlamak için kullanılan uzak bilgisayarda yönetici olarak bir Windows PowerShell oturumu başlatmak için aşağıdaki komutu girin:
   
    `Start PowerShell`
2. Windows PowerShell oturumunda, cihazınızın SSAdmin konsoluna Bağlan:
   
   1. Komut isteminde şunu girin:
     
       `$MS = New-PSSession -ComputerName <IP address for DATA 0> -Credential SSAdmin -ConfigurationName "SSAdminConsole"`
   2. Açılan iletişim kutusunda, cihaz Yöneticisi parolanızı girin. Varsayılan parola _Password1_.
     
      ![PowerShell kimlik bilgisi iletişim kutusu](./media/storsimple-8000-create-manage-support-package/IC740962.png)
   3. **Tamam**’ı seçin.
   4. Komut isteminde şunu girin:
     
      `Enter-PSSession $MS`
3. Açılan oturumda uygun komutu girin.
   
   * Parola ile korunmayan ağ paylaşımları için girin:
     
       `Export-HcsSupportPackage -Path <\\IP address\location of the shared folder> -Include Default -Credential domainname\username`
     
       (Destek Paketi şifreli olduğundan) için bir parola ve şifreleme parolası istenir. Destek Paketi, ardından varsayılan klasörü (cihaz adı geçerli tarih ve saat ile eklenen) oluşturulur.
   * Parola ile korunmayan paylaşımları gerekmeyen `-Credential` parametresi. Aşağıdakileri girin:
     
       `Export-HcsSupportPackage`
     
       Destek Paketi, her iki denetleyicilerinin varsayılan klasörde oluşturulur. Sorun giderme için Microsoft Support gönderilebilecek bir şifrelenmiş ve sıkıştırılmış dosya paketidir. Daha fazla bilgi için [Microsoft Destek'e başvur](storsimple-8000-contact-microsoft-support.md).

### <a name="the-export-hcssupportpackage-cmdlet-parameters"></a>Export-hcssupportpackage komutunu cmdlet parametreleri

Export-hcssupportpackage komutunu cmdlet'i ile aşağıdaki parametreleri kullanabilirsiniz.

| Parametre | Gerekli/isteğe bağlı | Açıklama |
| --- | --- | --- |
| `-Path` |Gerekli |Destek Paketi yerleştirildiği ağda paylaşılan klasör konumunu belirtmek için kullanın. |
| `-EncryptionPassphrase` |Gerekli |Destek Paketi şifrelemek yardımcı olmak için bir parola sağlamak için kullanın. |
| `-Credential` |İsteğe bağlı |Paylaşılan ağ klasörüne erişim kimlik bilgileri sağlamak için kullanın. |
| `-Force` |İsteğe bağlı |Şifreleme parolası onayı adımı atlamak için bu seçeneği kullanın. |
| `-PackageTag` |İsteğe bağlı |Altında bir dizin belirtin kullanacağınız *yolu* destek paketi yerleştirildiği içinde. Varsayılandır [cihaz adı]-[geçerli tarih ve time:yyyy-MM-dd-HH-mm-ss]. |
| `-Scope` |İsteğe bağlı |Olarak belirttiğiniz **küme** hem denetleyicileri için bir destek paketi oluşturmak için (varsayılan). Yalnızca geçerli denetleyiciye ait bir paket oluşturmak istiyorsanız, belirtin **denetleyicisi**. |

## <a name="edit-a-support-package"></a>Destek paketini Düzenle

Destek Paketi oluşturduktan sonra önemli bilgileri kaldırmak için paketi düzenleyin gerekebilir. Bu, birim adları, cihaz IP adreslerini ve günlük dosyalarını yedekleme adlarından içerebilir.

> [!IMPORTANT]
> Yalnızca StorSimple için Windows PowerShell aracılığıyla oluşturulan bir destek paketi düzenleyebilirsiniz. StorSimple cihaz Yöneticisi hizmeti ile Azure portalında oluşturulan bir paket düzenleyemezsiniz.

Üzerinde Microsoft Support sitesini yüklemeden önce bir destek paketi düzenlemek için ilk destek paketi şifresini, dosyaları düzenleyin ve yeniden şifrele. Aşağıdaki adımları uygulayın.

#### <a name="to-edit-a-support-package-in-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell'de bir destek paketi düzenlemek için

1. Açıklandığı gibi daha önce bir destek paketi oluştur [StorSimple için Windows PowerShell'de bir destek paketi oluşturulacak](#to-create-a-support-package-in-windows-powershell-for-storsimple).
2. [Betiği indirin](https://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) istemciniz üzerinde yerel olarak.
3. Windows PowerShell modülünü içeri aktarın. Komut dosyasını indirdiğiniz yerel klasör yolunu belirtin. Modülü içeri aktarmak için şunu girin:
   
    `Import-module <Path to the folder that contains the Windows PowerShell script>`
4. Tüm dosyalar *.aes* sıkıştırılmış ve şifrelenmiş dosyalar. Sıkıştırmasını açın ve dosyaların şifresini çözmek için şunu girin:
   
    `Open-HcsSupportPackage <Path to the folder that contains support package files>`
   
    Gerçek dosya uzantıları için tüm dosyaları artık görüntülendiğini unutmayın.
   
    ![Destek paketini Düzenle](./media/storsimple-8000-create-manage-support-package/IC750706.png)
5. İçin şifreleme parolası istendiğinde destek paketi oluştururken kullandığınız parolayı girin.
   
        cmdlet Open-HcsSupportPackage at command pipeline position 1
   
        Supply values for the following parameters:EncryptionPassphrase: ****
6. Günlük dosyalarını içeren klasöre göz atın. Günlük dosyaları artık sıkıştırması açılmış ve şifresi olduğundan bu özgün dosya uzantıları olacaktır. Birim adları ve cihaz IP adresleri gibi herhangi bir müşteriye özgü bilgileri kaldırmak için bu dosyalarda değişiklik ve dosyaları kaydedin.
7. Gzip ile sıkıştırmak ve bunları AES-256 ile şifrelemek için dosyaları kapatın. Bu hız ve güvenlik, destek paketini bir ağ üzerinden aktarılması içindir. Sıkıştırma ve dosyaları şifrelemek için aşağıdakileri girin:
   
    `Close-HcsSupportPackage <Path to the folder that contains support package files>`
   
    ![Destek paketini Düzenle](./media/storsimple-8000-create-manage-support-package/IC750707.png)
8. İstendiğinde, değiştirilen destek paketi için bir şifreleme parolası sağlayın.
   
        cmdlet Close-HcsSupportPackage at command pipeline position 1
        Supply values for the following parameters:EncryptionPassphrase: ****
9. Yeni parolayı not yazın, böylece Microsoft istendiğinde Support paylaşabilirsiniz.

### <a name="example-editing-files-in-a-support-package-on-a-password-protected-share"></a>Örnek: Destek Paketi parola korumalı bir paylaşımı üzerindeki dosyaları düzenleme

Aşağıdaki örnek, şifresini çözmek için düzenlemek ve bir destek paketi yeniden şifrele gösterilmektedir.

        PS C:\WINDOWS\system32> Import-module C:\Users\Default\StorSimple\SupportPackage\HCSSupportPackageTools.psm1

        PS C:\WINDOWS\system32> Open-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Open-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32> Close-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Close-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32>

## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [destek paketi toplanan bilgiler](https://support.microsoft.com/help/3193606/storsimple-support-packages-and-device-logs)
* Bilgi nasıl [desteği paketleri ve cihaz günlükleri cihaz dağıtımınızın sorunlarını giderdikten kullanılacağını](storsimple-8000-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting).
* Bilgi edinmek için nasıl [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).

