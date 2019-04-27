---
title: 'Azure Active Directory etki alanı Hizmetleri: Ubuntu sanal makinesi için yönetilen etki alanına Katıl | Microsoft Docs'
description: Bir Ubuntu Linux sanal makinesini Azure AD Domain Services için katılın
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: 804438c4-51a1-497d-8ccc-5be775980203
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: ergreenl
ms.openlocfilehash: 8699585a7f8e5cdfc81a40b94fbe10fa677a0030
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60417308"
---
# <a name="join-an-ubuntu-virtual-machine-in-azure-to-a-managed-domain"></a>Bir Ubuntu sanal makinesi, Azure'da yönetilen bir etki alanına katılın.
Bu makalede, bir Ubuntu Linux sanal makinesi bir Azure AD Domain Services yönetilen etki alanına ekleme işlemini göstermektedir.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Başlamadan önce
Bu makalede listelenen görevleri gerçekleştirmek için gerekir:  
1. Geçerli bir **Azure aboneliği**.
2. Bir **Azure AD dizini** -ya da şirket içi dizin veya bir yalnızca bulut dizini ile eşitlenir.
3. **Azure AD etki alanı Hizmetleri** Azure AD dizini için etkinleştirilmesi gerekir. Bunu yapmadıysanız, bölümünde açıklanan tüm görevleri izleyin [Başlarken kılavuzunda](active-directory-ds-getting-started.md).
4. Sanal ağın DNS sunucuları olarak yönetilen etki alanı IP adreslerini yapılandırdığınızdan emin olun. Daha fazla bilgi için [Azure sanal ağı için DNS ayarlarını güncelleştirme](active-directory-ds-getting-started-dns.md)
5. Tamamlamak için gereken adımlar [Azure AD Domain Services yönetilen etki alanınıza parolalarını eşitleyin](active-directory-ds-getting-started-password-sync.md).


## <a name="provision-an-ubuntu-linux-virtual-machine"></a>Bir Ubuntu Linux sanal makinesi sağlama
Aşağıdaki yöntemlerden birini kullanarak azure'daki bir Ubuntu Linux sanal makinesi sağlayın:
* [Azure portal](../virtual-machines/linux/quick-create-portal.md)
* [Azure CLI](../virtual-machines/linux/quick-create-cli.md)
* [Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md)

> [!IMPORTANT]
> * Sanal makineye dağıtma **aynı sanal ağda Azure AD Domain Services, etkinleştirdiğiniz**.
> * Çekme bir **farklı bir alt** Azure AD Domain Services, etkinleştirdiğiniz bir.
>


## <a name="connect-remotely-to-the-ubuntu-linux-virtual-machine"></a>Ubuntu Linux sanal makineye uzaktan bağlanın
Azure'da Ubuntu sanal makinesi sağlandı. VM sağlama sırasında oluşturulan yerel yönetici hesabını kullanarak sanal makineye uzaktan bağlanmak için sonraki görevdir bakın.

Makaledeki yönergeleri [Linux çalıştıran bir sanal makine için oturum açma](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="configure-the-hosts-file-on-the-linux-virtual-machine"></a>Linux sanal makine konak dosyasını yapılandırma
SSH terminalinizi/etc/hosts dosyasını düzenleyin ve makinenizin IP adresi ve ana bilgisayar adını güncelleştirin.

```
sudo vi /etc/hosts
```

Hosts dosyasında şu değeri girin:

```
127.0.0.1 contoso-ubuntu.contoso100.com contoso-ubuntu
```
Burada, 'contoso100.com' yönetilen etki alanınızın DNS etki alanı adı ' dir. 'contoso-ubuntu' yönetilen etki alanına katıldığınız Ubuntu sanal makinenin adıdır.


## <a name="install-required-packages-on-the-linux-virtual-machine"></a>Linux sanal makinesinde gerekli paketleri yükleyin
Ardından, sanal makinede etki alanına katılım için gerekli paketleri yükleyin. Aşağıdaki adımları gerçekleştirin:

1.  SSH terminalinizde depolarından paket listeleri indirmek için aşağıdaki komutu yazın. Bu komut, paketleri ve bunların bağımlılıklarını en yeni sürümleri hakkında bilgi almak için paket listeleri güncelleştirir.

    ```
    sudo apt-get update
    ```

2. Gerekli paketleri yüklemek için aşağıdaki komutu yazın.
    ```
      sudo apt-get install krb5-user samba sssd sssd-tools libnss-sss libpam-sss ntp ntpdate realmd adcli
    ```

3. Kerberos yüklenmesi sırasında pembe bir ekran görürsünüz. 'Kullanıcı krb5' paketi yüklemesi (içindeki tüm ve büyük harf) bölge adı ister. Yükleme [Bölge] yazar ve /etc/krb5.conf [domain_realm] bölümler.

    > [!TIP]
    > Yönetilen etki alanınızın adını contoso100.com ise CONTOSO100.COM bölge olarak girin. Unutmayın, bölge adını büyük harfli belirtilmelidir.
    >
    >


## <a name="configure-the-ntp-network-time-protocol-settings-on-the-linux-virtual-machine"></a>Linux sanal makinesinde NTP (Ağ Zaman Protokolü) ayarlarını yapılandırın
Tarih ve saat Ubuntu vm'nizin yönetilen etki alanı ile eşitlemeniz gerekir. Yönetilen etki alanınızın NTP hostname /etc/ntp.conf dosyasına ekleyin.

```
sudo vi /etc/ntp.conf
```

Ntp.conf dosyasındaki şu değeri girin ve dosyayı kaydedin:

```
server contoso100.com
```
Burada, 'contoso100.com' yönetilen etki alanınızın DNS etki alanı adı ' dir.

Ubuntu sanal makinenin tarih ve NTP sunucusuyla zaman Şimdi Eşitle ve NTP hizmetini başlatın:

```
sudo systemctl stop ntp
sudo ntpdate contoso100.com
sudo systemctl start ntp
```


## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a>Linux sanal makinesini yönetilen etki alanına
Gerekli paketleri, Linux sanal makinesinde yüklü olan, sonraki görev sanal makinenin yönetilen etki alanına sağlamaktır.

1. AAD Domain Services yönetilen etki alanında bulur. SSH terminalinizde şu komutu yazın:

    ```
    sudo realm discover CONTOSO100.COM
    ```

   > [!NOTE]
   > **Sorun giderme:** Varsa *bölge bulma* yönetilen etki alanınıza bulamıyor:
   >   * Etki alanı (try ping) sanal makineden erişilebilir olduğundan emin olun.
   >   * Sanal makinenin yönetilen etki alanında kullanılabilir olduğu aynı sanal ağa gerçekten dağıtılmış olduğunu kontrol edin.
   >   * Sanal ağın DNS sunucusu ayarlarını yönetilen etki alanının etki alanı denetleyicilerine işaret edecek şekilde güncelleştirdiyseniz denetleyin.

2. Kerberos başlatın. SSH terminalinizde şu komutu yazın:

    > [!TIP]
    > * 'AAD DC Administrators' grubuna ait bir kullanıcıyla belirttiğinizden emin olun.
    > * Büyük harf etki alanı adı belirtin, başka kinit başarısız olur.
    >

    ```
    kinit bob@CONTOSO100.COM
    ```

3. Makine etki alanına ekleyin. SSH terminalinizde şu komutu yazın:

    > [!TIP]
    > Önceki adımda ('kinit') belirtilen aynı kullanıcı hesabı kullanın.
    >

    ```
    sudo realm join --verbose CONTOSO100.COM -U 'bob@CONTOSO100.COM' --install=/
    ```

("Başarıyla kaydoldu makine bölgedeki") bir ileti almalısınız zaman makine başarıyla yönetilen etki alanına katılmış olmalıdır.


## <a name="update-the-sssd-configuration-and-restart-the-service"></a>SSSD yapılandırmasını güncelleştirin ve hizmeti yeniden başlatın
1. SSH terminalinizde şu komutu yazın. Sssd.conf dosyasını açın ve aşağıdaki değişikliği yapın
    ```
    sudo vi /etc/sssd/sssd.conf
    ```

2. Açıklama satırı haline **use_fully_qualified_names = True** ve dosyayı kaydedin.
    ```
    # use_fully_qualified_names = True
    ```

3. SSSD hizmetini yeniden başlatın.
    ```
    sudo service sssd restart
    ```


## <a name="configure-automatic-home-directory-creation"></a>Otomatik giriş dizini oluşturmayı yapılandır
Kullanıcıların açtıktan sonra otomatik olarak oluşturulmasını giriş dizini etkinleştirmek için PuTTY, terminalde aşağıdaki komutları yazın:
```
sudo vi /etc/pam.d/common-session
```

Bu dosyadaki 'isteğe bağlı oturum pam_sss.so' satırın altına aşağıdaki satırı ekleyin ve kaydedin:
```
session required pam_mkhomedir.so skel=/etc/skel/ umask=0077
```


## <a name="verify-domain-join"></a>Etki alanına katılım doğrulayın
Makine için yönetilen etki alanı başarıyla katıldı olup olmadığını doğrulayın. Bağlanma Ubuntu VM'yi farklı bir SSH bağlantısı kullanarak etki alanına katıldı. Bir etki alanı kullanıcı hesabı kullanın ve kullanıcı hesabının doğru şekilde çözülmüş olup olmadığını denetleyin.

1. Etki alanına bağlanmak için aşağıdaki komutu yazın, terminal, SSH SSH kullanarak Ubuntu sanal makinesi katıldı. Yönetilen etki alanına ait bir etki alanı hesabını kullanın (örneğin, 'bob@CONTOSO100.COM' Bu durumda.)
    ```
    ssh -l bob@CONTOSO100.COM contoso-ubuntu.contoso100.com
    ```

2. SSH terminalinizde giriş dizini doğru başlatılmadı görmek için aşağıdaki komutu yazın.
    ```
    pwd
    ```

3. SSH terminalinizde grup üyeliklerini doğru şekilde çözümlenmesine olmadığını görmek için aşağıdaki komutu yazın.
    ```
    id
    ```


## <a name="grant-the-aad-dc-administrators-group-sudo-privileges"></a>'AAD DC Administrators' grubunun sudo ayrıcalıkları verme
Ubuntu sanal makinesi üzerinde 'AAD DC Administrators' grubunun yönetici ayrıcalıkları üyesi verebilir. Sudo dosya /etc/sudoers bulunur. Sudoers içinde eklenen AD gruplarının üyeleri, sudo gerçekleştirebilirsiniz.

1. Terminal, SSH süper kullanıcı ayrıcalıklarıyla oturum açmış emin olun. VM oluşturulurken belirtilen yerel yönetici hesabını kullanabilirsiniz. Aşağıdaki komutu yürütün:
    ```
    sudo vi /etc/sudoers
    ```

2. Şu girişi /etc/sudoers dosyasına ekleyin ve kaydedin:
    ```
    # Add 'AAD DC Administrators' group members as admins.
    %AAD\ DC\ Administrators ALL=(ALL) NOPASSWD:ALL
    ```

3. Artık 'AAD DC Administrators' grubunun bir üyesi olarak oturum açabilirsiniz ve VM üzerinde yönetici ayrıcalıklarına sahip olmalıdır.


## <a name="troubleshooting-domain-join"></a>Etki alanına katılım sorunlarını giderme
Başvurmak [sorun giderme etki alanına katılma](active-directory-ds-admin-guide-join-windows-vm-portal.md#troubleshoot-joining-a-domain) makalesi.


## <a name="related-content"></a>İlgili İçerik
* [Azure AD etki alanı Hizmetleri - başlangıç kılavuzu](active-directory-ds-getting-started.md)
* [Bir Windows Server sanal makinesi bir Azure AD Domain Services yönetilen etki alanına ekleyin](active-directory-ds-admin-guide-join-windows-vm.md)
* [Linux çalıştıran bir sanal makine için oturum açma](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
