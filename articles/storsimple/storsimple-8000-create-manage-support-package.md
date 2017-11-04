---
title: "StorSimple 8000 serisi destek paketi oluştur | Microsoft Docs"
description: "Oluşturma, şifresini ve StorSimple 8000 serisi cihazınız için bir destek paketi düzenleme öğrenin."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/05/2017
ms.author: alkohli
ms.openlocfilehash: 92abbb96b2117e10800de61b5c405a784453265b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-manage-a-support-package-for-storsimple-8000-series"></a>Oluşturma ve StorSimple 8000 serisi için bir destek paketi yönetme

## <a name="overview"></a>Genel Bakış

Bir StorSimple destek paketi Microsoft Support StorSimple cihaz sorunları gidermenize yardımcı olacak tüm ilgili günlükleri toplayan bir kullanımı kolay mekanizmasıdır. Toplanan günlükleri şifrelenmiş ve sıkıştırılmış.

Bu öğretici oluşturmak ve StorSimple 8000 serisi cihazınız için destek paketi yönetmek için adım adım yönergeler içerir. Bir StorSimple sanal dizisi ile çalışıyorsanız, Git [günlük paketi oluşturmak](storsimple-ova-web-ui-admin.md#generate-a-log-package).

## <a name="create-a-support-package"></a>Bir destek paketi oluştur

Bazı durumlarda, StorSimple için Windows PowerShell aracılığıyla destek paketi el ile oluşturmanız gerekir. Örneğin:

* Microsoft Support paylaşımı önce günlük dosyalarınızı hassas bilgileri kaldırmak gerekiyorsa.
* Bağlantı sorunları nedeniyle paketi karşıya zorluk yaşıyorsanız.

E-posta Microsoft Support el ile oluşturulan destek paketinizi paylaşabilirsiniz. StorSimple için Windows PowerShell içinde bir destek paketi oluşturmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-create-a-support-package-in-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell içinde bir destek paketi oluşturmak için

1. StorSimple Cihazınızı bağlamak için kullanılan uzak bilgisayarda yönetici olarak bir Windows PowerShell oturumu başlatmak için aşağıdaki komutu girin:
   
    `Start PowerShell`
2. Windows PowerShell oturumunda Cihazınızı SSAdmin Konsola bağlan:
   
   1. Komut isteminde girin:
     
       `$MS = New-PSSession -ComputerName <IP address for DATA 0> -Credential SSAdmin -ConfigurationName "SSAdminConsole"`
   2. Açılan iletişim kutusunda, cihaz Yöneticisi parolasını girin. Varsayılan parola _Parola1_.
     
      ![PowerShell kimlik bilgisi iletişim kutusu](./media/storsimple-8000-create-manage-support-package/IC740962.png)
   3. **Tamam**’ı seçin.
   4. Komut isteminde girin:
     
      `Enter-PSSession $MS`
3. Açılan oturumda uygun komutunu girin.
   
   * Parola korumalı ağ paylaşımları için girin:
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" –Credential "Username" -Force`
     
       (Destek Paketi şifrelendiğinden) bir ağ paylaşılan klasörüne ve bir şifreleme parolası yoluna bir parola istenir. Bir destek paketi sonra belirtilen klasörde oluşturulur.
   * Parola korumalı olmayan paylaşımlar için ihtiyacınız olmayan `-Credential` parametresi. Aşağıdakileri girin:
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" -Force`
     
       Destek paketini hem denetleyicileri belirtilen ağ paylaşılan klasöründe oluşturulur. Bu sorun giderme için Microsoft Support gönderilebilecek bir şifrelenmiş ve sıkıştırılmış dosyasıdır. Daha fazla bilgi için bkz: [Microsoft Destek birimine başvurun](storsimple-8000-contact-microsoft-support.md).

### <a name="the-export-hcssupportpackage-cmdlet-parameters"></a>Dışarı aktarma HcsSupportPackage cmdlet parametreleri

Dışarı aktarma HcsSupportPackage cmdlet'iyle şu parametreleri kullanabilirsiniz.

| Parametre | Gerekli/isteğe bağlı | Açıklama |
| --- | --- | --- |
| `-Path` |Gerekli |Destek paketini yerleştirildiği ağ paylaşılan klasörün konumunu belirtmek için kullanın. |
| `-EncryptionPassphrase` |Gerekli |Destek Paketi şifrelemek yardımcı olmak için bir parola sağlamak için kullanın. |
| `-Credential` |İsteğe bağlı |Ağ paylaşılan klasörüne erişim kimlik bilgileri sağlamak için kullanın. |
| `-Force` |İsteğe bağlı |Şifreleme parolası onayı adımı atlamak için kullanın. |
| `-PackageTag` |İsteğe bağlı |Bir dizini altında belirtmek için kullanın *yolu* destek paketi yerleştirildiği içinde. Varsayılan değer [aygıt adı]-[geçerli tarih ve time:yyyy-MM-dd-HH-mm-ss]. |
| `-Scope` |İsteğe bağlı |Olarak belirttiğiniz **küme** hem denetleyicileri için bir destek paketi oluşturmak için (varsayılan). Geçerli denetleyici için yalnızca bir paketi oluşturmak istiyorsanız, belirtin **denetleyicisi**. |

## <a name="edit-a-support-package"></a>Destek paketini Düzenle

Bir destek paketi oluşturduktan sonra hassas bilgileri kaldırmak için paketi düzenleyin gerekebilir. Bu, birim adları, cihaz IP adresleri ve günlük dosyalarını yedekleme adlarından içerebilir.

> [!IMPORTANT]
> Yalnızca StorSimple için Windows PowerShell aracılığıyla oluşturulan bir destek paketi düzenleyebilirsiniz. StorSimple cihaz Yöneticisi hizmeti ile Azure portalında oluşturulan bir paket düzenleyemezsiniz.

Microsoft Support sitesinde karşıya yüklemeden önce bir destek paketi düzenlemek, ilk destek paketi şifresini, dosyaları düzenleyin ve yeniden şifrelemek için. Aşağıdaki adımları gerçekleştirin.

#### <a name="to-edit-a-support-package-in-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell desteği paketinde düzenlemek için

1. Açıklandığı gibi daha önce bir destek paketi oluştur [StorSimple için Windows PowerShell içinde bir destek paketi oluşturmak için](#to-create-a-support-package-in-windows-powershell-for-storsimple).
2. [Komut dosyasını karşıdan](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) istemciniz yerel olarak.
3. Windows PowerShell modülünü içeri aktarın. Komut dosyası karşıdan yerel klasörün yolunu belirtin. Modülü içeri aktarmak için şunu girin:
   
    `Import-module <Path to the folder that contains the Windows PowerShell script>`
4. Tüm dosyalar *.aes* sıkıştırılmış ve şifrelenmiş dosyalar. Sıkıştırmasını açın ve dosyaların şifresini çözmek için şunu girin:
   
    `Open-HcsSupportPackage <Path to the folder that contains support package files>`
   
    Gerçek dosya uzantıları için tüm dosyalar artık görüntülendiğini unutmayın.
   
    ![Destek paketini Düzenle](./media/storsimple-8000-create-manage-support-package/IC750706.png)
5. Şifreleme parolası istendiğinde, destek paketi oluştururken kullandığınız parolayı girin.
   
        cmdlet Open-HcsSupportPackage at command pipeline position 1
   
        Supply values for the following parameters:EncryptionPassphrase: ****
6. Günlük dosyalarını içeren klasöre gidin. Günlük dosyalarının şimdi sıkıştırması açılmış ve şifresi çünkü bunlar özgün dosya uzantılarını sahip olur. Birim adları ve cihaz IP adresleri gibi herhangi bir müşteriye özgü bilgileri kaldırmak için bu dosyalarda değişiklik ve dosyaları kaydedebilir.
7. Gzip ile sıkıştırmak ve AES 256 ile şifrelemek için dosyaları kapatın. Bu hız ve Destek paketi bir ağ üzerinden aktarılması güvenliği içindir. Sıkıştır ve dosyaları şifrelemek için aşağıdakileri girin:
   
    `Close-HcsSupportPackage <Path to the folder that contains support package files>`
   
    ![Destek paketini Düzenle](./media/storsimple-8000-create-manage-support-package/IC750707.png)
8. İstendiğinde, bir şifreleme parolası değiştirilmiş destek paketi için belirtin.
   
        cmdlet Close-HcsSupportPackage at command pipeline position 1
        Supply values for the following parameters:EncryptionPassphrase: ****
9. Microsoft istendiğinde Support paylaşmanızı sağlayan yeni parolayı not yazın.

### <a name="example-editing-files-in-a-support-package-on-a-password-protected-share"></a>Örnek: parola korumalı bir paylaşılan bir destek paketi dosyalarını düzenleme

Aşağıdaki örnek, şifre çözme, düzenleme ve Destek paketi yeniden şifrele gösterilmektedir.

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
* Bilgi nasıl [aygıt dağıtımınızı gidermek için destek paketler ve cihaz günlükleri kullanan](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting).
* Bilgi edinmek için nasıl [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).

