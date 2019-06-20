---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: 5ba55e339db4c33d1b0d759e4682481e20318938
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67203739"
---
### <a name="prepare-for-a-push-installation-on-a-linux-server"></a>Bir Linux sunucusu üzerinde göndererek yüklemeye hazırlanma

1. Linux bilgisayarı ile işlem sunucusu arasında ağ bağlantısı olduğundan emin olun.
1. İşlem sunucusunun bilgisayara erişmek için kullanabileceği bir hesap oluşturun. Hesap, kaynak Linux sunucusu üzerindeki bir **kök** kullanıcı olmalıdır. Bu hesap yalnızca gönderim yüklemesi için ve güncelleştirmeler için kullanın.
1. Kaynak Linux sunucusundaki /etc/hosts dosyasında, yerel ana bilgisayar adını tüm ağ bağdaştırıcıları ile ilişkili IP adreslerine eşleyen girişler olduğunu denetleyin.
1. Çoğaltmak istediğiniz bilgisayara en son openssh, openssh-server, openssl paketlerini yükleyin.
1. Secure Shell’in (SSH) etkin olduğundan ve bağlantı noktası 22’de çalıştırıldığından emin olun.
1. SFTP alt sistemi ve parola kimlik doğrulamasını sshd_config dosyasında etkinleştirin. Şu adımları uygulayın:

    a. **Kök** kullanıcı olarak oturum açın.

    b. İçinde **/etc/ssh/sshd_config** dosya, ile başlayan satırı bulun **PasswordAuthentication**.

    c. Bu satırı açıklamadan çıkarın ve değerini değiştirin **Evet**.

    d. İle başlayan satırı bulun **alt**, ve bu satırı açıklamadan çıkarın.

      ![Linux](./media/site-recovery-prepare-push-install-mob-svc-lin/mobility2.png)

    e. **sshd** hizmetini yeniden başlatın.

1. Oluşturduğunuz hesabı CSPSConfigtool içine ekleyin. Şu adımları uygulayın:

    a. Yapılandırma sunucunuzda oturum açın.

    b. **cspsconfigtool.exe** dosyasını açın. %ProgramData%\home\svsystems\bin klasöründe ve masaüstünde kısayol olarak kullanılabilir.

    c. Üzerinde **hesaplarını yönetme** sekmesinde **hesabı Ekle**.

    d. Oluşturduğunuz hesabı ekleyin.

    d. Bir bilgisayar için çoğaltmayı etkinleştirdiğinizde kullandığınız kimlik bilgilerini girin.
