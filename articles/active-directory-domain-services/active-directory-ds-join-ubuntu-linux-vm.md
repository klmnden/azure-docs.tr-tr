---
title: "Azure Active Directory etki alanı Hizmetleri: bir Ubuntu VM yönetilen bir etki alanına katılın. | Microsoft Docs"
description: "Azure AD Etki Alanı Hizmetleri'ne bir Ubuntu Linux sanal makine birleştirme"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 804438c4-51a1-497d-8ccc-5be775980203
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2017
ms.author: maheshu
ms.openlocfilehash: a8a3610707ca7d00694779c4b3631e1483d6bbdd
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="join-an-ubuntu-virtual-machine-in-azure-to-a-managed-domain"></a>Ubuntu sanal makine Azure'da yönetilen bir etki alanına katılın.
Bu makalede bir Ubuntu Linux sanal makine bir Azure AD etki alanı Hizmetleri yönetilen etki alanına katılma kullanmayı gösterir.


## <a name="before-you-begin"></a>Başlamadan önce
Bu makalede listelenen görevleri gerçekleştirmek için gerekir:  
1. Geçerli bir **Azure aboneliği**.
2. Bir **Azure AD dizini** -ya da bir şirket içi dizin veya bir yalnızca bulut dizini ile eşitlenir.
3. **Azure AD etki alanı Hizmetleri** Azure AD dizini için etkinleştirilmesi gerekir. Bunu yapmadıysanız, özetlenen tüm görevleri izleyin [Getting Started guide](active-directory-ds-getting-started.md).
4. Sanal ağ için DNS sunucuları olarak yönetilen etki alanı IP adreslerini yapılandırdığınızdan emin olun. Daha fazla bilgi için bkz: [Azure sanal ağı için DNS ayarlarını güncelleştirme](active-directory-ds-getting-started-dns.md)
5. İçin gerekli adımları [Azure AD etki alanı Hizmetleri yönetilen etki alanınız için parola eşitleme](active-directory-ds-getting-started-password-sync.md).


## <a name="provision-an-ubuntu-linux-virtual-machine"></a>Ubuntu Linux sanal makine sağlama
Aşağıdaki yöntemlerden birini kullanarak azure'da bir Ubuntu Linux sanal makine sağlayın:
* [Azure portal](../virtual-machines/linux/quick-create-portal.md)
* [Azure CLI](../virtual-machines/linux/quick-create-cli.md)
* [Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md)

> [!IMPORTANT]
> * Sanal makineye dağıtmak **Azure AD etki alanı Hizmetleri içinde etkinleştirdiğiniz aynı sanal ağ**.
> * Çekme bir **farklı alt** Azure AD etki alanı Hizmetleri içinde etkinleştirdiğiniz olandan.
>


## <a name="connect-remotely-to-the-ubuntu-linux-virtual-machine"></a>Ubuntu Linux sanal makineye uzaktan bağlanın
Ubuntu sanal makine Azure'da sağlandı. Sonraki uzaktan VM sağlarken oluşturulmuş yerel yönetici hesabı kullanarak sanal makineye bağlanmak için bir görevdir.

Makalesindeki yönergeleri izleyin [Linux çalıştıran bir sanal makine için oturum açma](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="configure-the-hosts-file-on-the-linux-virtual-machine"></a>Linux sanal makine üzerinde konak dosyasını yapılandırma
SSH terminalinizde/etc/hosts dosyasını düzenleyin ve makinenizin IP adresi ve ana bilgisayar adını güncelleştirin.

```
sudo vi /etc/hosts
```

Hosts dosyasında şu değeri girin:

```
127.0.0.1 contoso-ubuntu.contoso100.com contoso-ubuntu
```
Burada, 'contoso100.com' DNS etki alanı, yönetilen etki alanı adıdır. 'contoso-ubuntu' Ubuntu sanal makineyi yönetilen bir etki alanına katılma barındırıcı adıdır.


## <a name="install-required-packages-on-the-linux-virtual-machine"></a>Gerekli paketler Linux sanal makineye yükleme
Ardından, sanal makinenin etki alanına katılma için gerekli paketleri yüklemek. Aşağıdaki adımları gerçekleştirin:

1.  SSH terminalinizde havuzların paket listelerini indirmek için aşağıdaki komutu yazın. Bu komut, paketleri ve bunların bağımlılıklarını en son sürümleri hakkında bilgi almak için paket listeleri güncelleştirir.

    ```
    sudo apt-get update
    ```

2. Gereken paketleri yüklemek için aşağıdaki komutu yazın.
    ```
      sudo apt-get install krb5-user samba sssd sssd-tools libnss-sss libpam-sss ntp ntpdate realmd adcli
    ```

3. Kerberos yükleme sırasında pembe bir ekran görürsünüz. 'Kullanıcı krb5' paketin yüklenmesi bölge adı (tüm ve büyük harf) ister. Yükleme [Bölge] yazar ve /etc/krb5.conf [domain_realm] bölümler.

    > [!TIP]
    > Yönetilen etki alanınızın adını contoso100.com ise, CONTOSO100.COM bölge olarak girin. Unutmayın, bölge adını büyük harfli belirtilmesi gerekir.
    >
    >


## <a name="configure-the-ntp-network-time-protocol-settings-on-the-linux-virtual-machine"></a>Linux sanal makinede NTP (Ağ Zaman Protokolü) ayarlarını yapılandırın
Tarih ve saat Ubuntu vm'nizin yönetilen etki alanı ile eşitlemeniz gerekir. Yönetilen etki alanınızın NTP hostname /etc/ntp.conf dosyasına ekleyin.

```
sudo vi /etc/ntp.conf
```

Ntp.conf dosyasındaki şu değeri girin ve dosyayı kaydedin:

```
server contoso100.com
```
Burada, 'contoso100.com' DNS etki alanı, yönetilen etki alanı adıdır.

Ubuntu VM tarih ve saat NTP sunucusu ile Şimdi Eşitle ve NTP hizmetini başlatın:

```
sudo systemctl stop ntp
sudo ntpdate contoso100.com
sudo systemctl start ntp
```


## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a>Linux sanal makinesini yönetilen bir etki alanına katılma
Gerekli paketleri Linux sanal makinede yüklü olan, sonraki görev sanal makineyi yönetilen etki alanına belirlemektir.

1. AAD etki alanı Hizmetleri yönetilen etki bulur. SSH terminalinizde aşağıdaki komutu yazın:

    ```
    sudo realm discover CONTOSO100.COM
    ```

   > [!NOTE] 
   > **Sorun giderme:** varsa *bölge Bul* yönetilen etki alanınızı bulamıyor:
     * Etki alanı sanal makine (try ping) erişilebilir olduğundan emin olun.
     * Sanal makine yönetilen etki alanı kullanılamıyor aynı sanal ağa gerçekten dağıtıldıktan denetleyin.
     * Sanal ağın DNS sunucusu ayarlarını yönetilen etki alanının etki alanı denetleyicilerine işaret edecek şekilde güncelleştirdiyseniz denetleyin.
   >

2. Kerberos başlatır. SSH terminalinizde aşağıdaki komutu yazın: 

    > [!TIP] 
    > * 'AAD DC Yöneticiler' grubuna ait bir kullanıcı belirttiğinizden emin olun. 
    > * Büyük harflerle etki alanı adı belirtin, başka kinit başarısız olur.
    >

    ```
    kinit bob@CONTOSO100.COM
    ```

3. Makine etki alanına katılın. SSH terminalinizde aşağıdaki komutu yazın: 

    > [!TIP] 
    > Önceki adımda ('kinit') belirtilen aynı kullanıcı hesabı kullanın.
    >

    ```
    sudo realm join --verbose CONTOSO100.COM -U 'bob@CONTOSO100.COM' --install=/
    ```

("Başarılı bir şekilde kaydedilen makine bölgedeki") bir ileti almalısınız zaman makine başarıyla katılırsa yönetilen etki.


## <a name="update-the-sssd-configuration-and-restart-the-service"></a>SSSD yapılandırmasını güncelleştirin ve hizmeti yeniden başlatın
1. SSH terminalinizde aşağıdaki komutu yazın. Sssd.conf dosyasını açın ve aşağıdaki değişiklik yapın
    ```
    sudo vi /etc/sssd/sssd.conf
    ```

2. Açıklama satırı çıkışı **use_fully_qualified_names = True** ve dosyayı kaydedin.
    ```
    # use_fully_qualified_names = True
    ```

3. SSSD hizmetini yeniden başlatın.
    ```
    sudo service sssd restart
    ```


## <a name="configure-automatic-home-directory-creation"></a>Otomatik giriş dizini oluşturmayı yapılandır
Kullanıcılar oturum sonra otomatik olarak oluşturulmasını giriş dizini etkinleştirmek için PuTTY terminalinizde aşağıdaki komutları yazın:
```
sudo vi /etc/pam.d/common-session
```
    
Satırın 'oturumu isteğe bağlı pam_sss.so' aşağıda bu dosyada aşağıdaki satırı ekleyin ve dosyayı kaydedin:
```
session required pam_mkhomedir.so skel=/etc/skel/ umask=0077
```


## <a name="verify-domain-join"></a>Etki alanına katılma doğrulayın
Makine için yönetilen etki alanı başarıyla katıldı olup olmadığını doğrulayın. Bağlanmak Ubuntu VM farklı bir SSH bağlantısı kullanarak etki alanına katıldı. Bir etki alanı kullanıcı hesabı kullanmanızı ve kullanıcı hesabının doğru çözülmüş olup olmadığını denetleyin.

1. Terminal, SSH SSH kullanarak Ubuntu sanal makine etki alanına bağlanmak için aşağıdaki komutu yazın katıldı. Yönetilen etki alanına ait bir etki alanı hesabı kullanın (örneğin, 'bob@CONTOSO100.COM' Bu durumda.)
    ```
    ssh -l bob@CONTOSO100.COM contoso-ubuntu.contoso100.com
    ```

2. SSH terminalinizde giriş dizini doğru başlatılmadı görmek için aşağıdaki komutu yazın.
    ```
    pwd
    ```

3. SSH terminalinizde grup üyeliklerini doğru çözüldüğünü olmadığını görmek için aşağıdaki komutu yazın.
    ```
    id
    ```


## <a name="grant-the-aad-dc-administrators-group-sudo-privileges"></a>'AAD DC Yöneticiler' grup sudo ayrıcalıkları
Ubuntu VM üzerinde 'AAD DC Yöneticiler' grup yönetim ayrıcalıkları üyeleri verebilirsiniz. Sudo dosyası /etc/sudoers bulunur. İçinde sudoers eklenen AD gruplarının üyeleri sudo gerçekleştirebilir.

1. Terminal, SSH süper kullanıcı ayrıcalıkları ile oturum açtığınızdan emin olun. VM oluşturulurken belirtilen yerel yönetici hesabı kullanabilirsiniz. Aşağıdaki komutu yürütün:
    ```
    sudo vi /etc/sudoers
    ```

2. /Etc/sudoers dosyasına aşağıdaki girişi ekleyin ve dosyayı kaydedin:
    ```
    # Add 'AAD DC Administrators' group members as admins.
    %AAD\ DC\ Administrators ALL=(ALL) NOPASSWD:ALL
    ```

3. Şimdi 'AAD DC Yöneticiler' grubunun bir üyesi olarak oturum açabilir ve VM üzerinde yönetici ayrıcalıklarına sahip olmalıdır.


## <a name="troubleshooting-domain-join"></a>Sorun giderme etki alanına katılma
Başvurmak [sorun giderme etki alanına katılma](active-directory-ds-admin-guide-join-windows-vm-portal.md#troubleshooting-domain-join) makalesi.


## <a name="related-content"></a>İlgili İçerik
* [Azure AD etki alanı Hizmetleri - başlangıç kılavuzu](active-directory-ds-getting-started.md)
* [Bir Windows Server sanal makine bir Azure AD etki alanı Hizmetleri yönetilen etki alanına katılma](active-directory-ds-admin-guide-join-windows-vm.md)
* [Linux çalıştıran bir sanal makine için oturum açma](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
