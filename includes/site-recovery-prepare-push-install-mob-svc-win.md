---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: ffc9b09c72ef1bf5180a0d626908d09b6fdd41ca
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50165263"
---
### <a name="prepare-for-a-push-installation-on-a-windows-computer"></a>Bir Windows bilgisayar üzerinde göndererek yüklemeye hazırlanma

1. Windows bilgisayar ve işlem sunucusu arasında ağ bağlantısı olduğundan emin olun.
1. İşlem sunucusunun bilgisayara erişmek için kullanabileceği bir hesap oluşturun. Hesabı yerel veya etki alanı yönetici hakları olması gerekir. Bu hesap yalnızca gönderim yüklemesi için ve aracı güncelleştirmeleri için kullanın.

   > [!NOTE]
   > Bir etki alanı hesabı kullanmıyorsanız yerel bilgisayarda Uzak kullanıcı erişim denetimini devre dışı bırakın. Yeni bir DWORD kayıt defteri anahtarının hkey_local_machıne\software\microsoft\windows\currentversion\policies\system altında uzak kullanıcı erişim denetimini devre dışı bırakma eklemek için: **LocalAccountTokenFilterPolicy**. Değerine **1**. Bir komut isteminde bu görevi gerçekleştirmek için aşağıdaki komutu çalıştırın:  
   `REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`
   >
   >
1. Korumak istediğiniz bilgisayarda Windows Güvenlik Duvarı'nda seçin **bir uygulama veya özelliğin güvenlik duvarı üzerinden izin**. Etkinleştirme **dosya ve Yazıcı Paylaşımı** ve **Windows Yönetim Araçları (WMI)**. Bir etki alanına ait bilgisayarlar için bir Grup İlkesi nesnesi (GPO) kullanarak güvenlik duvarı ayarlarını yapılandırabilirsiniz.

   ![Güvenlik duvarı ayarları](./media/site-recovery-prepare-push-install-mob-svc-win/mobility1.png)

1. Oluşturduğunuz hesabı CSPSConfigtool içine ekleyin. Şu adımları uygulayın:

    a. Yapılandırma sunucunuzda oturum açın.

    b. **cspsconfigtool.exe** dosyasını açın. %ProgramData%\home\svsystems\bin klasöründe ve masaüstünde kısayol olarak kullanılabilir.

    c. Üzerinde **hesaplarını yönetme** sekmesinde **hesabı Ekle**.

    d. Oluşturduğunuz hesabı ekleyin.

    e. Bir bilgisayar için çoğaltmayı etkinleştirdiğinizde kullandığınız kimlik bilgilerini girin.
