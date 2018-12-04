---
title: Azure'a VMware Vm'lerini ve fiziksel sunucuları olağanüstü durum kurtarma için Mobility hizmetini yükleme | Microsoft Docs
description: VMware Vm'lerini ve fiziksel sunucuları olağanüstü durum kurtarma için Mobility Hizmeti Aracısı Azure Site Recovery hizmetini kullanarak Azure'a yüklemeyi öğrenin.
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: ramamill
ms.openlocfilehash: 30b177578464653499cdcde8cacf65defa5548ef
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52846921"
---
# <a name="install-the-mobility-service-for-disaster-recovery-of-vmware-vms-and-physical-servers"></a>VMware Vm'lerini ve fiziksel sunucuları olağanüstü durum kurtarma için Mobility hizmetini yükleme

Ayarladığınızda olağanüstü durum kurtarma için VMware Vm'lerini ve fiziksel sunucuları kullanarak [Azure Site Recovery](site-recovery-overview.md), yüklediğiniz [Site Recovery Mobility hizmetini](vmware-physical-mobility-service-overview.md) her bir şirket içi VMware VM ve fiziksel sunucu.  Mobility hizmeti yakalar makinede veri yazar ve onları Site Recovery işlem sunucusuna gönderir.

## <a name="install-on-windows-machine"></a>Windows makinesine yükleyin

Korumak istediğiniz her bir Windows makinede aşağıdakileri yapın:

1. Makine ile işlem sunucusu arasında ağ bağlantısı olduğundan emin olun. Ardından ayrı işlem sunucusu ayarlamadıysanız ayarlamadıysanız varsayılan olarak, yapılandırma sunucusunda çalışıyor.
1. İşlem sunucusunun bilgisayara erişmek için kullanabileceği bir hesap oluşturun. Hesabı yerel veya etki alanı yönetici hakları olması gerekir. Bu hesap yalnızca gönderim yüklemesi için ve aracı güncelleştirmeleri için kullanın.
2. Bir etki alanı hesabı kullanmıyorsanız yerel bilgisayarda Uzak kullanıcı erişim denetimi gibi devre dışı bırakın:
    - Hkey_local_machıne\software\microsoft\windows\currentversion\policies\system kayıt defteri anahtarı altında yeni bir DWORD değeri ekleyin: **LocalAccountTokenFilterPolicy**. Değerine **1**.
    -  Bu komut isteminde aşağıdakini yapmak için aşağıdaki komutu çalıştırın:  
   ' REG ADD hkey_local_machıne\software\microsoft\windows\currentversion\policies\system /v LocalAccountTokenFilterPolicy /t REG_DWORD /d
3. Korumak istediğiniz makinede Windows Güvenlik Duvarı'nda seçin **bir uygulama veya özelliğin güvenlik duvarı üzerinden izin**. Etkinleştirme **dosya ve Yazıcı Paylaşımı** ve **Windows Yönetim Araçları (WMI)**. Bir etki alanına ait bilgisayarlar için bir Grup İlkesi nesnesi (GPO) kullanarak güvenlik duvarı ayarlarını yapılandırabilirsiniz.

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

## <a name="next-steps"></a>Sonraki adımlar

Mobility hizmeti, Azure portalında yükledikten sonra seçin **+ Çoğalt** bu Vm'leri korumaya başlamak için. Çoğaltma için etkinleştirme hakkında daha fazla bilgi edinin [VMware VMs(vmware-azure-enable-replication.md) ve [fiziksel sunucuları](physical-azure-disaster-recovery.md#enable-replication).


