---
title: "Bir StorSimple destek paketi oluştur | Microsoft Docs"
description: "Oluşturma, şifresini ve StorSimple cihazınız için bir destek paketi düzenleme öğrenin."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: eac76f5f-5db1-4c92-af8c-54053b91e66c
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/03/2017
ms.author: alkohli
ms.openlocfilehash: addfb01998271ee3a431d92bf2a6fec70f0577a6
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
---
# <a name="create-and-manage-a-storsimple-support-package"></a>Oluşturma ve StorSimple destek paketi yönetme
> [!NOTE]
> StorSimple için Klasik portalı kullanım dışıdır. StorSimple cihaz yöneticileri yeni Azure portalına kullanımdan zamanlamaya göre otomatik olarak taşır. Bir e-posta ve bu taşıma için portal bir bildirim alırsınız. Bu belgede ayrıca yakında kullanımdan kaldırılacaktır. Bu makalede yeni Azure portalına için sürümünü görüntülemek için şu adrese gidin [oluşturma ve StorSimple destek paketi yönetmek](storsimple-8000-create-manage-support-package.md). Taşıma hakkında herhangi bir sorunuz için bkz: [SSS: Azure portalına taşıma](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Genel Bakış
Bir StorSimple destek paketi Microsoft Support StorSimple cihaz sorunları gidermenize yardımcı olacak tüm ilgili günlükleri toplayan bir kullanımı kolay mekanizmasıdır. Toplanan günlükleri şifrelenmiş ve sıkıştırılmış.

Bu öğretici oluşturmak ve Destek Paketi yönetmek için adım adım yönergeler içerir.

## <a name="create-and-upload-a-support-package-in-the-azure-classic-portal"></a>Oluşturma ve Azure Klasik portalında bir destek paketi yükleme
Oluşturma ve Destek paketi Microsoft Support sitesini karşıya yükleme **Bakım** Klasik Azure portalı hizmetinde sayfası.

> [!NOTE]
> Karşıya yükleme desteği geçiş gerektirir. Destek mühendisinize Bu size bir e-postayla sağlamalıdır.
> 
> 

Şifrelenmiş ve sıkıştırılmış Destek Paketi (.cab dosyası) oluşturulur ve Destek siteye yüklenir. Destek mühendisine bu paket sorun giderme desteği sitesinden sonra alabilirsiniz.

Bir destek paketi oluşturmak için Klasik portalında aşağıdaki adımları gerçekleştirin.

#### <a name="to-create-a-support-package-in-the-azure-classic-portal"></a>Klasik Azure portalında bir destek paketi oluşturmak için
1. Seçin **aygıtları** > **Bakım**.
2. İçinde **destek paketi** bölümünde, select **oluşturma ve karşıya yükleme destek paketi**.
3. İçinde **oluşturma ve karşıya yükleme destek paketi** iletişim kutusunda, aşağıdakileri yapın:
   
    ![Destek Paketi Oluştur](./media/storsimple-create-manage-support-package/IC740923.png)
   
   * İçinde **destek geçiş** metin kutusuna, geçiş anahtarını girin. Microsoft destek mühendisinize bu geçiş, e-posta göndermesi gerekir.
   * Onay sağlamak için bu onay kutusunu işaretleyin destek paketi Microsoft Support sitesini otomatik olarak yüklenecek.
   * Onay simgesine tıklayarak ![Onay simgesi](./media/storsimple-create-manage-support-package/IC740895.png).

## <a name="manually-create-a-support-package"></a>El ile bir destek paketi oluştur
Bazı durumlarda, StorSimple için Windows PowerShell aracılığıyla destek paketi el ile oluşturmanız gerekir. Örneğin:

* Microsoft Support paylaşımı önce günlük dosyalarınızı hassas bilgileri kaldırmak gerekiyorsa.
* Bağlantı sorunları nedeniyle paketi karşıya zorluk yaşıyorsanız.

E-posta Microsoft Support el ile oluşturulan destek paketinizi paylaşabilirsiniz. StorSimple için Windows PowerShell içinde bir destek paketi oluşturmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-create-a-support-package-in-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell içinde bir destek paketi oluşturmak için
1. StorSimple Cihazınızı bağlamak için kullanılan uzak bilgisayarda yönetici olarak bir Windows PowerShell oturumu başlatmak için aşağıdaki komutu girin:
   
    `Start PowerShell`
2. Windows PowerShell oturumunda Cihazınızı SSAdmin Konsola bağlan:
   
   * Komut isteminde girin:
     
       `$MS = New-PSSession -ComputerName <IP address for DATA 0> -Credential SSAdmin -ConfigurationName "SSAdminConsole"`
   * Açılan iletişim kutusunda, cihaz Yöneticisi parolasını girin. Varsayılan paroladır:
     
      `Password1`
     
      ![PowerShell kimlik bilgisi iletişim kutusu](./media/storsimple-create-manage-support-package/IC740962.png)
   * **Tamam**’ı seçin.
   * Komut isteminde girin:
     
      `Enter-PSSession $MS`
3. Açılan oturumda uygun komutunu girin.
   
   * Parola korumalı ağ paylaşımları için girin:
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" –Credential "Username" -Force`
     
       (Destek Paketi şifrelendiğinden) bir ağ paylaşılan klasörüne ve bir şifreleme parolası yoluna bir parola istenir. Bir destek paketi sonra belirtilen klasörde oluşturulur.
   * Parola korumalı olmayan paylaşımlar için ihtiyacınız olmayan `-Credential` parametresi. Aşağıdakileri girin:
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" -Force`
     
       Destek paketini hem denetleyicileri belirtilen ağ paylaşılan klasöründe oluşturulur. Bu sorun giderme için Microsoft Support gönderilebilecek bir şifrelenmiş ve sıkıştırılmış dosyasıdır. Daha fazla bilgi için bkz: [Microsoft Destek birimine başvurun](storsimple-contact-microsoft-support.md).

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
> Yalnızca StorSimple için Windows PowerShell aracılığıyla oluşturulan bir destek paketi düzenleyebilirsiniz. StorSimple Yöneticisi hizmetiyle Azure Klasik portalında oluşturulan bir paket düzenleyemezsiniz.
> 
> 

Microsoft Support sitesinde karşıya yüklemeden önce bir destek paketi düzenlemek, ilk destek paketi şifresini, dosyaları düzenleyin ve yeniden şifrelemek için. Aşağıdaki adımları gerçekleştirin.

#### <a name="to-edit-a-support-package-in-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell desteği paketinde düzenlemek için
1. Açıklandığı gibi daha önce bir destek paketi oluştur [StorSimple için Windows PowerShell içinde bir destek paketi oluşturmak için](#to-create-a-support-package-in-windows-powershell-for-storsimple).
2. [Komut dosyasını karşıdan](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) istemciniz yerel olarak.
3. Windows PowerShell modülünü içeri aktarın. Komut dosyası karşıdan yerel klasörün yolunu belirtin. Modülü içeri aktarmak için şunu girin:
   
    `Import-module <Path to the folder that contains the Windows PowerShell script>`
4. Tüm dosyalar *.aes* sıkıştırılmış ve şifrelenmiş dosyalar. Sıkıştırmasını açın ve dosyaların şifresini çözmek için şunu girin:
   
    `Open-HcsSupportPackage <Path to the folder that contains support package files>`
   
    Gerçek dosya uzantıları için tüm dosyalar artık görüntülendiğini unutmayın.
   
    ![Destek paketini Düzenle](./media/storsimple-create-manage-support-package/IC750706.png)
5. Şifreleme parolası istendiğinde, destek paketi oluştururken kullandığınız parolayı girin.
   
        cmdlet Open-HcsSupportPackage at command pipeline position 1
   
        Supply values for the following parameters:EncryptionPassphrase: ****
6. Günlük dosyalarını içeren klasöre gidin. Günlük dosyalarının şimdi sıkıştırması açılmış ve şifresi çünkü bunlar özgün dosya uzantılarını sahip olur. Birim adları ve cihaz IP adresleri gibi herhangi bir müşteriye özgü bilgileri kaldırmak için bu dosyalarda değişiklik ve dosyaları kaydedebilir.
7. Gzip ile sıkıştırmak ve AES 256 ile şifrelemek için dosyaları kapatın. Bu hız ve Destek paketi bir ağ üzerinden aktarılması güvenliği içindir. Sıkıştır ve dosyaları şifrelemek için aşağıdakileri girin:
   
    `Close-HcsSupportPackage <Path to the folder that contains support package files>`
   
    ![Destek paketini Düzenle](./media/storsimple-create-manage-support-package/IC750707.png)
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
* Bilgi nasıl [aygıt dağıtımınızı gidermek için destek paketler ve cihaz günlükleri kullanan](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting).
* Bilgi edinmek için nasıl [StorSimple Cihazınızı yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-manager-service-administration.md).

