---
title: "Azure Active Directory etki alanı Hizmetleri: RHEL VM yönetilen bir etki alanına katılın. | Microsoft Docs"
description: "Red Hat Enterprise Linux sanal makinesini Azure AD Etki Alanı Hizmetleri'ne ekleme"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: d76ae997-2279-46dd-bfc5-c0ee29718096
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2017
ms.author: maheshu
ms.openlocfilehash: 9046bdb5bd8ff21429c951cbe7120334bd000621
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="join-a-red-hat-enterprise-linux-7-virtual-machine-to-a-managed-domain"></a>Red Hat Enterprise Linux 7 sanal makinesini yönetilen bir etki alanına ekleme
Bu makalede bir Red Hat Enterprise Linux (RHEL) 7 sanal makine bir Azure AD etki alanı Hizmetleri yönetilen etki alanına katılma kullanmayı gösterir.

## <a name="before-you-begin"></a>Başlamadan önce
Bu makalede listelenen görevleri gerçekleştirmek için gerekir:  
1. Geçerli bir **Azure aboneliği**.
2. Bir **Azure AD dizini** -ya da bir şirket içi dizin veya bir yalnızca bulut dizini ile eşitlenir.
3. **Azure AD etki alanı Hizmetleri** Azure AD dizini için etkinleştirilmesi gerekir. Bunu yapmadıysanız, özetlenen tüm görevleri izleyin [Getting Started guide](active-directory-ds-getting-started.md).
4. Sanal ağ için DNS sunucuları olarak yönetilen etki alanı IP adreslerini yapılandırdığınızdan emin olun. Daha fazla bilgi için bkz: [Azure sanal ağı için DNS ayarlarını güncelleştirme](active-directory-ds-getting-started-dns.md)
5. İçin gerekli adımları [Azure AD etki alanı Hizmetleri yönetilen etki alanınız için parola eşitleme](active-directory-ds-getting-started-password-sync.md).


## <a name="provision-a-red-hat-enterprise-linux-virtual-machine"></a>Red Hat Enterprise Linux sanal makine sağlama
Aşağıdaki yöntemlerden birini kullanarak azure'da bir RHEL 7 sanal makine sağlama:
* [Azure portalı](../virtual-machines/linux/quick-create-portal.md)
* [Azure CLI](../virtual-machines/linux/quick-create-cli.md)
* [Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md)

> [!IMPORTANT]
> * Sanal makineye dağıtmak **Azure AD etki alanı Hizmetleri içinde etkinleştirdiğiniz aynı sanal ağ**.
> * Çekme bir **farklı alt** Azure AD etki alanı Hizmetleri içinde etkinleştirdiğiniz olandan.
>


## <a name="connect-remotely-to-the-newly-provisioned-linux-virtual-machine"></a>Uzaktan yeni sağlandı Linux sanal makineye bağlanma
RHEL 7.2 sanal makine Azure'da sağlandı. Sonraki uzaktan VM sağlarken oluşturulmuş yerel yönetici hesabı kullanarak sanal makineye bağlanmak için bir görevdir.

Makalesindeki yönergeleri izleyin [Linux çalıştıran bir sanal makine için oturum açma](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="configure-the-hosts-file-on-the-linux-virtual-machine"></a>Linux sanal makine üzerinde konak dosyasını yapılandırma
SSH terminalinizde/etc/hosts dosyasını düzenleyin ve makinenizin IP adresi ve ana bilgisayar adını güncelleştirin.

```
sudo vi /etc/hosts
```

Hosts dosyasında şu değeri girin:

```
127.0.0.1 contoso-rhel.contoso100.com contoso-rhel
```
Burada, 'contoso100.com' DNS etki alanı, yönetilen etki alanı adıdır. 'contoso-rhel' RHEL sanal makineyi yönetilen bir etki alanına katılma barındırıcı adıdır.


## <a name="install-required-packages-on-the-linux-virtual-machine"></a>Gerekli paketler Linux sanal makineye yükleme
Ardından, sanal makinenin etki alanına katılma için gerekli paketleri yüklemek. SSH terminalinizde gereken paketleri yüklemek için aşağıdaki komutu yazın:

    ```
    sudo yum install realmd sssd krb5-workstation krb5-libs samba-common-tools
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

2. Initialize Kerberos. SSH terminalinizde aşağıdaki komutu yazın: 

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
    sudo realm join --verbose CONTOSO100.COM -U 'bob@CONTOSO100.COM'
    ```

("Başarılı bir şekilde kaydedilen makine bölgedeki") bir ileti almalısınız zaman makine başarıyla katılırsa yönetilen etki.


## <a name="verify-domain-join"></a>Etki alanına katılma doğrulayın
Makine için yönetilen etki alanı başarıyla katıldı olup olmadığını doğrulayın. Bağlanmak RHEL farklı bir SSH bağlantısı kullanarak VM etki alanına katıldı. Bir etki alanı kullanıcı hesabı kullanmanızı ve kullanıcı hesabının doğru çözülmüş olup olmadığını denetleyin.

1. Terminal, SSH SSH kullanarak RHEL sanal makine etki alanına bağlanmak için aşağıdaki komutu yazın katıldı. Yönetilen etki alanına ait bir etki alanı hesabı kullanın (örneğin, 'bob@CONTOSO100.COM' Bu durumda.)
    ```
    ssh -l bob@CONTOSO100.COM contoso-rhel.contoso100.com
    ```

2. SSH terminalinizde giriş dizini doğru başlatılmadı görmek için aşağıdaki komutu yazın.
    ```
    pwd
    ```

3. SSH terminalinizde grup üyeliklerini doğru çözüldüğünü olmadığını görmek için aşağıdaki komutu yazın.
    ```
    id
    ```


## <a name="troubleshooting-domain-join"></a>Sorun giderme etki alanına katılma
Başvurmak [sorun giderme etki alanına katılma](active-directory-ds-admin-guide-join-windows-vm-portal.md#troubleshooting-domain-join) makalesi.

## <a name="related-content"></a>İlgili İçerik
* [Azure AD etki alanı Hizmetleri - başlangıç kılavuzu](active-directory-ds-getting-started.md)
* [Bir Windows Server sanal makine bir Azure AD etki alanı Hizmetleri yönetilen etki alanına katılma](active-directory-ds-admin-guide-join-windows-vm.md)
* [Linux çalıştıran bir sanal makine için oturum açma](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* [Installing Kerberos](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Managing_Smart_Cards/installing-kerberos.html)
* [Red Hat Enterprise Linux 7 - Windows tümleştirme Kılavuzu](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/index.html)
