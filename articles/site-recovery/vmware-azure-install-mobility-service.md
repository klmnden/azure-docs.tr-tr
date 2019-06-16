---
title: Kaynak makine, Mobility hizmeti göndererek yükleme VMware vm'lerinin olağanüstü durum kurtarma ve fiziksel sunucuları aracılığıyla Azure'a yüklemek için hazırlama | Microsoft Docs
description: VMware vm'lerinin olağanüstü durum kurtarma için göndererek yükleme yoluyla Mobility aracısı yüklemek için sunucu ve fiziksel sunucuları Azure Site Recovery hizmetini kullanarak azure'a hazırlamayı öğrenin.
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: ramamill
ms.openlocfilehash: 628be573d03d42ec62a358071074facfe228852d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60318198"
---
# <a name="prepare-source-machine-for-push-installation-of-mobility-agent"></a>Kaynak makineye mobility Aracısı gönderme yüklemesi için hazırlama

Ayarladığınızda olağanüstü durum kurtarma için VMware Vm'lerini ve fiziksel sunucuları kullanarak [Azure Site Recovery](site-recovery-overview.md), yüklediğiniz [Site Recovery Mobility hizmetini](vmware-physical-mobility-service-overview.md) her bir şirket içi VMware VM ve fiziksel sunucu.  Mobility hizmeti yakalar makinede veri yazar ve onları Site Recovery işlem sunucusuna gönderir.

## <a name="install-on-windows-machine"></a>Windows makinesine yükleyin

Korumak istediğiniz her bir Windows makinede aşağıdakileri yapın:

1. Makine ile işlem sunucusu arasında ağ bağlantısı olduğundan emin olun. Ardından ayrı işlem sunucusu ayarlamadıysanız ayarlamadıysanız varsayılan olarak, yapılandırma sunucusunda çalışıyor.
1. İşlem sunucusunun bilgisayara erişmek için kullanabileceği bir hesap oluşturun. Hesabı yerel veya etki alanı yönetici hakları olması gerekir. Bu hesap yalnızca gönderim yüklemesi için ve aracı güncelleştirmeleri için kullanın.
2. Bir etki alanı hesabı kullanmıyorsanız yerel bilgisayarda Uzak kullanıcı erişim denetimi gibi devre dışı bırakın:
    - Hkey_local_machıne\software\microsoft\windows\currentversion\policies\system kayıt defteri anahtarı altında yeni bir DWORD değeri ekleyin: **LocalAccountTokenFilterPolicy**. Değerine **1**.
    -  Bu komut isteminde aşağıdakini yapmak için aşağıdaki komutu çalıştırın:  
   `REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d
3. Korumak istediğiniz makinede Windows Güvenlik Duvarı'nda seçin **bir uygulama veya özelliğin güvenlik duvarı üzerinden izin**. Etkinleştirme **dosya ve Yazıcı Paylaşımı** ve **Windows Yönetim Araçları (WMI)** . Bir etki alanına ait bilgisayarlar için bir Grup İlkesi nesnesi (GPO) kullanarak güvenlik duvarı ayarlarını yapılandırabilirsiniz.

   ![Güvenlik duvarı ayarları](./media/vmware-azure-install-mobility-service/mobility1.png)

4. Oluşturduğunuz hesabı CSPSConfigtool içine ekleyin. Bunu yapmak için yapılandırma sunucunuzda oturum açın.
5. **cspsconfigtool.exe** dosyasını açın. %ProgramData%\home\svsystems\bin klasöründe ve masaüstünde kısayol olarak kullanılabilir.
6. Üzerinde **hesaplarını yönetme** sekmesinde **hesabı Ekle**.
7. Oluşturduğunuz hesabı ekleyin.
8. Bir bilgisayar için çoğaltmayı etkinleştirdiğinizde kullandığınız kimlik bilgilerini girin.

## <a name="install-on-linux-machine"></a>Linux makinesine yükleyin

Korumak istediğiniz her Linux makinesinde, aşağıdakileri yapın:

1. Linux makinesi ve işlem sunucusu arasında ağ bağlantısı olduğundan emin olun.
2. İşlem sunucusunun bilgisayara erişmek için kullanabileceği bir hesap oluşturun. Hesap, kaynak Linux sunucusu üzerindeki bir **kök** kullanıcı olmalıdır. Bu hesap yalnızca gönderim yüklemesi için ve güncelleştirmeler için kullanın.
3. Kaynak Linux sunucusundaki /etc/hosts dosyasında, yerel ana bilgisayar adını tüm ağ bağdaştırıcıları ile ilişkili IP adreslerine eşleyen girişler olduğunu denetleyin.
4. Çoğaltmak istediğiniz bilgisayara en son openssh, openssh-server, openssl paketlerini yükleyin.
5. Secure Shell’in (SSH) etkin olduğundan ve bağlantı noktası 22’de çalıştırıldığından emin olun.
4. SFTP alt sistemi ve parola kimlik doğrulamasını sshd_config dosyasında etkinleştirin. Bunu yapmak için olarak oturum **kök**.
5. İçinde **/etc/ssh/sshd_config** dosya, ile başlayan satırı bulun **PasswordAuthentication**.
6. Bu satırı açıklamadan çıkarın ve değerini değiştirin **Evet**.
7. İle başlayan satırı bulun **alt**, ve bu satırı açıklamadan çıkarın.

      ![Linux](./media/vmware-azure-install-mobility-service/mobility2.png)

8. **sshd** hizmetini yeniden başlatın.
9. Oluşturduğunuz hesabı CSPSConfigtool içine ekleyin. Bunu yapmak için yapılandırma sunucunuzda oturum açın.
10. **cspsconfigtool.exe** dosyasını açın. %ProgramData%\home\svsystems\bin klasöründe ve masaüstünde kısayol olarak kullanılabilir.
11. Üzerinde **hesaplarını yönetme** sekmesinde **hesabı Ekle**.
12. Oluşturduğunuz hesabı ekleyin.
13. Bir bilgisayar için çoğaltmayı etkinleştirdiğinizde kullandığınız kimlik bilgilerini girin.

## <a name="anti-virus-on-replicated-machines"></a>Çoğaltılan makinelerde virüsten koruma

Çoğaltmak istediğiniz makineleri çalışan active virüsten koruma yazılımı varsa, virüsten koruma işlemlerini Mobility hizmeti yükleme klasör dışlama emin olun (*C:\ProgramData\ASR\agent*). Bu, çoğaltmanın beklendiği gibi çalıştığını sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Mobility hizmeti, Azure portalında yükledikten sonra seçin **+ Çoğalt** bu Vm'leri korumaya başlamak için. Çoğaltma için etkinleştirme hakkında daha fazla bilgi edinin [VMware VMs(vmware-azure-enable-replication.md) ve [fiziksel sunucuları](physical-azure-disaster-recovery.md#enable-replication).


