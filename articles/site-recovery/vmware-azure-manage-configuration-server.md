---
title: Azure Site Recovery ile VMware olağanüstü durum kurtarması için yapılandırma sunucusunu yönetme | Microsoft Docs
description: Bu makale, Vmware'den azure'a Azure Site RecoveryS ile olağanüstü durum kurtarma için var olan bir yapılandırma sunucusunu yönetme açıklamaktadır.
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: raynew
ms.openlocfilehash: df5b2ecce2a5c9d7c263ee0acc3a49b859b93f7f
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39346129"
---
# <a name="manage-the-configuration-server-for-vmware-vms"></a>VMware Vm'leri için yapılandırma sunucusunu yönetme

Kullanırken bir şirket içi yapılandırma sunucusu ayarlama [Azure Site Recovery](site-recovery-overview.md) VMware Vm'lerini ve fiziksel sunucuları azure'a olağanüstü durum kurtarma için. Yapılandırma sunucusu arasındaki iletişimi düzenler şirket içi VMware ve Azure ve veri çoğaltma işlemlerini yönetir. Bu makalede dağıtıldıktan sonra yapılandırma sunucusunu yönetmek için ortak görevler özetlenir.



## <a name="modify-vmware-settings"></a>VMware ayarlarını değiştirme

Yapılandırma sunucusu gibi erişebilirsiniz:
    - Sanal Makinenin üzerinde dağıtıldığı ve Azure Site Recovery Yapılandırma Yöneticisi başlatmak için Masaüstü kısayoldan oturum açın.
    - Alternatif olarak, yapılandırma sunucusundan uzaktan erişebileceğiniz **https://*ConfigurationServerName*/:44315 /**. Yönetici kimlik bilgilerinizle oturum açın.
   
### <a name="modify-vmware-server-settings"></a>VMware sunucu ayarlarını değiştirin

1. Oturum açma işleminden sonra farklı bir VMware sunucusunu yapılandırma sunucusu ile ilişkilendirmek için işaretleyin **vCenter sunucusu/vSphere ESXi Sunucusu Ekle**.
2. Ayrıntıları girin ve ardından **Tamam**.


### <a name="modify-credentials-for-automatic-discovery"></a>Otomatik bulma için kimlik bilgilerini değiştirme

1. Oturum açma işleminden sonra VMware vm'lerinin otomatik bulma için VMware sunucusuna bağlanmak için kullanılan kimlik bilgilerini güncelleştirmek için seçin **Düzenle**.
2. Yeni kimlik bilgilerini girin ve ardından **Tamam**.

    ![VMware değiştirme](./media/vmware-azure-manage-configuration-server/modify-vmware-server.png)


## <a name="modify-credentials-for-mobility-service-installation"></a>Mobility hizmeti yüklemesi için kimlik bilgilerini değiştirme

VMware Vm'leri için çoğaltmayı etkinleştirme Mobility hizmetini otomatik olarak yüklemek için kullanılan kimlik bilgilerini değiştirin.

1. Oturum açma işleminden sonra seçin **sanal makine kimlik bilgilerini yönetme**
2. Yeni kimlik bilgilerini girin ve ardından **Tamam**.

    ![Mobility hizmet kimlik bilgilerini değiştirme](./media/vmware-azure-manage-configuration-server/modify-mobility-credentials.png)

## <a name="modify-proxy-settings"></a>Proxy ayarlarını değiştirme

Azure'da internet erişimi için yapılandırma sunucusu makine tarafından kullanılan proxy ayarları değiştirin. Varsayılan işlem sunucusunun yapılandırma sunucusu makinesinde çalışan yanı sıra işlem sunucunuz varsa, her iki makinede ayarlarını değiştirin.

1. Oturum açtıktan sonra yapılandırma sunucusuna seçin **bağlantıları Yönet**.
2. Proxy değerlerini güncelleştirin. Ardından **Kaydet** ayarları güncelleştirilemiyor.

## <a name="add-a-network-adapter"></a>Bir ağ bağdaştırıcısı ekleyin

Open Virtualization Format (OVF) şablonu, tek bir ağ bağdaştırıcısı ile VM yapılandırma sunucusu dağıtır.

- Yapabilecekleriniz [VM'ye ek bağdaştırıcı ekleme](vmware-azure-deploy-configuration-server.md#add-an-additional-adapter), ancak yapılandırma sunucusunu kasaya kaydetmeden önce eklemeniz gerekir.
- Yapılandırma sunucusunu kasaya kaydetmek sonra bir bağdaştırıcı eklemek için VM özelliklerinde bağdaştırıcı ekleyin. Ardından sunucuyu kasaya kaydetmeden gerekir.


## <a name="reregister-a-configuration-server-in-the-same-vault"></a>Bir yapılandırma sunucusunda aynı kasaya yeniden kaydettirin

Gerekirse yapılandırma sunucusunu aynı kasaya yeniden kaydettirin. Varsayılan işlem sunucusunun yapılandırma sunucusu makinesinde çalışan yanı sıra bir ek işlem sunucusu makinesi varsa, her iki makine yeniden kaydettirin.


  1. Kasası'nda açın **Yönet** > **Site Recovery altyapısı** > **Configuration Servers**.
  2. İçinde **sunucuları**seçin **indirme kayıt anahtarı** kasa kimlik bilgileri dosyası indirilemedi.
  3. Configuration server makinesinde oturum açın.
  4. İçinde **%ProgramData%\ASR\home\svsystems\bin**açın **cspsconfigtool.exe**.
  5. Üzerinde **kasa kaydı** sekmesinde **Gözat**, indirdiğiniz kasa kimlik bilgileri dosyası bulun.
  6. Gerekiyorsa, proxy sunucusu ayrıntıları sağlayın. Ardından **Kaydet**’i seçin.
  7. Bir yönetici PowerShell komut penceresi açın ve aşağıdaki komutu çalıştırın:
   ```
      $pwd = ConvertTo-SecureString -String MyProxyUserPassword
      Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
   ```

      >[!NOTE] 
      >Şunları **son sertifikaları çekme** genişleme işlem sunucusu yapılandırma sunucusundan komutu yürütün *"< yükleme Drive\Microsoft Azure Site Recovery\agent\cdpcli.exe >"--registermt'yi*

  8. Son olarak, aşağıdaki komutu yürüterek obengine yeniden başlatın.
  ```
          net stop obengine
          net start obengine

## Upgrade the configuration server

You run update rollups to update the configuration server. Updates can be applied for up to N-4 versions. For example:

- If you run 9.7, 9.8, 9.9, or 9.10, you can upgrade directly to 9.11.
- If you run 9.6 or earlier and you want to upgrade to 9.11, you must first upgrade to version 9.7. before 9.11.

Links to update rollups for upgrading to all versions of the configuration server are available in the [wiki updates page](https://social.technet.microsoft.com/wiki/contents/articles/38544.azure-site-recovery-service-updates.aspx).

Upgrade the server as follows:

1. In the vault, go to **Manage** > **Site Recovery Infrastructure** > **Configuration Servers**.
2. If an update is available, a link appears in the **Agent Version** > column.
    ![Update](./media/vmware-azure-manage-configuration-server/update2.png)
3. Download the update installer file to the configuration server.

    ![Update](./media/vmware-azure-manage-configuration-server/update1.png)

4. Double-click to run the installer.
5. The installer detects the current version running on the machine. Click **Yes** to start the upgrade.
6. When the upgrade completes the server configuration validates.

    ![Update](./media/vmware-azure-manage-configuration-server/update3.png)
    
7. Click **Finish** to close the installer.

## Delete or unregister a configuration server

1. [Disable protection](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure) for all VMs under the configuration server.
2. [Disassociate](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) and [delete](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) all replication policies from the configuration server.
3. [Delete](vmware-azure-manage-vcenter.md#delete-a-vcenter-server) all vCenter servers/vSphere hosts that are associated with the configuration server.
4. In the vault, open **Site Recovery Infrastructure** > **Configuration Servers**.
5. Select the configuration server that you want to remove. Then, on the **Details** page, select **Delete**.

    ![Delete configuration server](./media/vmware-azure-manage-configuration-server/delete-configuration-server.png)
   

### Delete with PowerShell

You can optionally delete the configuration server by using PowerShell.

1. [Install](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.4.0) the Azure PowerShell module.
2. Sign in to your Azure account by using this command:
    
    `Connect-AzureRmAccount`
3. Select the vault subscription.

     `Get-AzureRmSubscription –SubscriptionName <your subscription name> | Select-AzureRmSubscription`
3.  Set the vault context.
    
    ```
    $vault = get-AzureRmRecoveryServicesVault-adı <name of your vault> AzureRmSiteRecoveryVaultSettings Set - ARSVault $vault
    ```
4. Retrieve the configuration server.

    `$fabric = Get-AzureRmSiteRecoveryFabric -FriendlyName <name of your configuration server>`
6. Delete the configuration server.

    `Remove-AzureRmSiteRecoveryFabric -Fabric $fabric [-Force] `

> [!NOTE]
> You can use the **-Force** option in Remove-AzureRmSiteRecoveryFabric for forced deletion of the configuration server.
 
## Generate configuration server Passphrase

1. Sign in to your configuration server, and then open a command prompt window as an administrator.
2. To change the directory to the bin folder, execute the command **cd %ProgramData%\ASR\home\svsystems\bin**
3. To generate the passphrase file, execute **genpassphrase.exe -v > MobSvc.passphrase**.
4. Your passphrase will be stored in the file located at **%ProgramData%\ASR\home\svsystems\bin\MobSvc.passphrase**.

## Renew SSL certificates

The configuration server has an inbuilt web server, which orchestrates activities of the Mobility Service, process servers, and master target servers connected to it. The web server uses an SSL certificate to authenticate clients. The certificate expires after three years and can be renewed at any time.

### Check expiry

For configuration server deployments before May 2016, certificate expiry was set to one year. If you have a certificate that is going to expire, the following occurs:

- When the expiry date is two months or less, the service starts sending notifications in the portal, and by email (if you subscribed to Site Recovery notifications).
- A notification banner appears on the vault resource page. For more information, select the banner.
- If you see an **Upgrade Now** button, it indicates that some components in your environment haven't been upgraded to 9.4.xxxx.x or higher versions. Upgrade the components before you renew the certificate. You can't renew on older versions.

### Renew the certificate

1. In the vault, open **Site Recovery Infrastructure** > **Configuration Server**. Select the required configuration server.
2. The expiry date appears under **Configuration Server health**.
3. Select **Renew Certificates**. 


## Next steps

Review the tutorials for setting up disaster recovery of [VMware VMs](vmware-azure-tutorial.md) to Azure.
