---
title: " Azure Site Recovery ile VMware olağanüstü durum kurtarma için yapılandırma sunucusu yönetme | Microsoft Docs"
description: "Bu makalede, Azure Site Recovery hizmeti ile Azure, VMware olağanüstü durum kurtarma için mevcut bir yapılandırma sunucusunu yönetmek açıklar."
services: site-recovery
author: AnoopVasudavan
ms.service: site-recovery
ms.topic: article
ms.date: 01/15/2017
ms.author: anoopkv
ms.openlocfilehash: e9e4bfc86df2cae1facac62472c915d91fb8c84c
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2018
---
# <a name="manage-the-configuration-server"></a>Yapılandırma sunucusunu yönetme

Kullanırken bir şirket içi yapılandırma sunucusunu ayarlama [Azure Site Recovery](site-recovery-overview.md) VMware Vm'lerini ve fiziksel sunucuların azure'a olağanüstü durum kurtarma için hizmet. Yapılandırma sunucusu arasındaki iletişimi düzenler VMware ve Azure şirket içi ve veri çoğaltma yönetir. Bu makalede dağıtımı yapıldıktan sonra yapılandırma sunucusunu yönetmek için ortak görevler özetlenmektedir.

## <a name="modify-vmware-settings"></a>VMware ayarlarını değiştirme

Yapılandırma sunucusu bağlandığı VMware sunucu ayarlarını değiştirin.

1. Yapılandırma sunucusu çalıştıran makinede oturum açın.
2. Azure Site Recovery Configuration Manager Masaüstü kısayoldan başlatın. Ya da açmak **https://configuration-server-name/IP:44315**.
3. Tıklatın **Yönet vCenter sunucusu/vSPhere ESXi sunucusunda**:
    - Farklı bir VMware sunucusu yapılandırma sunucusuyla ilişkilendirmek için tıklatın **vCenter sunucusu/vSphere ESXi Sunucu Ekle**ve sunucu ayrıntıları belirtin.
    - VMware sanal makineleri otomatik olarak bulmayı VMware sunucusuna bağlanmak için kullanılan kimlik bilgilerini güncelleştirmek için **Düzenle**. Yeni kimlik bilgilerini belirtin ve ardından **Tamam**.

        ![VMware değiştirme](./media/site-recovery-vmware-to-azure-manage-configuration-server/modify-vmware-server.png)

## <a name="modify-credentials-for-mobility-service-installation"></a>Mobility hizmeti yüklemesi için kimlik bilgilerini değiştirme

Mobility hizmeti için çoğaltmayı etkinleştirme VMware vm'lerinde otomatik olarak yüklemek için kullanılan kimlik bilgilerini değiştirin.

1. Yapılandırma sunucusu çalıştıran makinede oturum açın.
2. Azure Site Recovery Configuration Manager Masaüstü kısayoldan başlatın. Ya da açmak **https://configuration-server-name/IP:44315**.
3. Tıklatın **sanal makine kimlik bilgilerini yöneten**ve yeni kimlik bilgilerini belirtin. Ardından **Tamam** ayarları güncelleştirmek için.

    ![Mobility hizmeti kimlik bilgilerini değiştirme](./media/site-recovery-vmware-to-azure-manage-configuration-server/modify-mobility-credentials.png)

## <a name="modify-proxy-settings"></a>Proxy ayarlarını değiştirme

Azure Internet erişimi için yapılandırma sunucu makinesi tarafından kullanılan proxy ayarlarını değiştirin. Yapılandırma sunucusu makinesinde çalışan varsayılan işlem sunucusuna ek olarak bir ek işlem sunucusu makineniz varsa her iki makinede ayarlarını değiştirin.

1. Yapılandırma sunucusu çalıştıran makinede oturum açın.
2. Azure Site Recovery Configuration Manager Masaüstü kısayoldan başlatın. Ya da açmak **https://configuration-server-name/IP:44315**.
3. Tıklatın **bağlantı yönetmek**ve proxy değerlerini güncelleştirin. Ardından **kaydetmek** ayarları güncelleştirmek için.

## <a name="add-a-network-adapter"></a>Bir ağ bağdaştırıcısı ekleyin

OVF şablon tek bir ağ bağdaştırıcısı yapılandırma sunucusuyla VM dağıtır. Yapabilecekleriniz [ek bağdaştırıcıyı VM'ye ekleyin)](how-to-deploy-configuration-server.md#add-an-additional-adapter), ancak yapılandırma sunucusunu kasaya kaydetmek önce bunu yapmanız gerekir.

Bir bağdaştırıcı kasaya yapılandırma sunucusu kaydedildikten sonra eklemeniz gerekiyorsa, VM Özellikleri'nde bağdaştırıcısı ekleyin ve kasada sunucusunu yeniden kaydettirin gerekir.


## <a name="reregister-a-configuration-server-in-the-same-vault"></a>Bir yapılandırma sunucusunda aynı kasaya yeniden kaydetme

Gerekirse aynı kasada yapılandırma sunucusunu yeniden kaydettirin. yapılandırma sunucusu makinesinde çalışan varsayılan işlem sunucusuna ek olarak bir ek işlem sunucusu makine sahip f her iki makine yeniden kaydettirin.

  1. Kasada açmak **Yönet** > **Site Recovery altyapısı** > **yapılandırma sunucularına**.
  2. İçinde **sunucuları**, tıklatın **indirme kayıt anahtarı**. Bu, kasa kimlik bilgileri dosyası indirir.
  3. Yapılandırma sunucusu makine oturum açın.
  4. İçinde **%ProgramData%\ASR\home\svagent\bin**, açık **cspsconfigtool.exe**.
  5. Üzerinde **kasa kayıt** sekmesinde, Gözat'ı tıklatın ve indirdiğiniz kasa kimlik bilgileri dosyası bulunur.
  6. Gerekliyse, proxy sunucusu ayrıntılarını belirtin. Ardından **kaydetmek**.
  7. Bir yönetici PowerShell komut penceresi açın ve aşağıdaki komutu çalıştırın:

      ```
      $pwd = ConvertTo-SecureString -String MyProxyUserPassword
      Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
      net stop obengine
      net start obengine
      ```

## <a name="delete-or-unregister-a-configuration-server"></a>Silme veya yapılandırma sunucusunun kaydı silinemedi

1. Devre dışı [korumayı devre dışı bırakın](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure) yapılandırma sunucusu altında yer alan tüm VM'ler için.
2. [İlişkisini](site-recovery-setup-replication-settings-vmware.md#dissociate-a-configuration-server-from-a-replication-policy) ve [silmek](site-recovery-setup-replication-settings-vmware.md#delete-a-replication-policy) yapılandırma sunucusundan tüm çoğaltma ilkeleri.
3. [Silme](site-recovery-vmware-to-azure-manage-vCenter.md#delete-a-vcenter-in-azure-site-recovery) yapılandırma sunucuyla ilişkili olan tüm Vcenter sunucularını/vSphere ana.
4. Kasa içinde açmak **Site Recovery altyapısı** > **yapılandırma sunucuları**
5. Kaldırmak istediğiniz yapılandırma sunucuya tıklayın. Ardından **ayrıntıları** sayfasında, **silmek**.

    ![Yapılandırma sunucusu Sil](./media/site-recovery-vmware-to-azure-manage-configuration-server/delete-configuration-server.png)
   

### <a name="delete-with-powershell"></a>PowerShell ile silme

PowerShell kullanarak yapılandırma sunucusu isteğe bağlı olarak silebilirsiniz:

1. [Yükleme](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.4.0) Azure PowerShell Modülü
2. Komutunu kullanarak Azure hesabınızda oturum açın:
    
    `Login-AzureRmAccount`
3. Kasa abonelik seçin:

     `Get-AzureRmSubscription –SubscriptionName <your subscription name> | Select-AzureRmSubscription`
3.  Kasa bağlamını ayarlayın:
    
    ```
    $vault = Get-AzureRmRecoveryServicesVault -Name <name of your vault>
    Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault
    ```
4. Yapılandırma sunucusu Al:

    `$fabric = Get-AzureRmSiteRecoveryFabric -FriendlyName <name of your configuration server>`
6. Yapılandırma sunucusu Sil:

    `Remove-AzureRmSiteRecoveryFabric -Fabric $fabric [-Force] `

> [!NOTE]
> Kullanabileceğiniz **-Force** Kaldır AzureRmSiteRecoveryFabric seçeneğinde zorla silinmesini yapılandırma sunucusu için.
 


## <a name="renew-ssl-certificates"></a>SSL sertifikaları yenile

Mobility hizmeti, işlemi sunucuları ve ona bağlı olan ana hedef sunucusu etkinliklerini düzenler bir yerleşik web sunucusu yapılandırma sunucusu vardır. Web sunucusu, istemcilerin kimliğini doğrulamak için bir SSL sertifikası kullanır. Sertifika üç yıl sonra dolar ve herhangi bir zamanda yenilenebilir.

### <a name="check-expiry"></a>Süre sonu denetleyin

Mayıs 2016 önce yapılandırma sunucu dağıtımları için bir yıl için sertifika süre sonu ayarlandı. Varsa bir sertifikanın süresi, aşağıdakiler gerçekleşir geçiyor:

- Ne zaman sona erme tarihi iki ay ya da daha düşük (Azure Site Recovery bildirimlerine abone değilse) bildirimleri Portalı'nda ve e-posta ile gönderme hizmetini başlatır.
- Bir bildirim başlığı kasası kaynak sayfasında görüntülenir. Daha fazla ayrıntı için başlığını tıklatın.
- Görürseniz bir **Şimdi Yükselt** düğmesi, bu gösterir 9.4.xxxx.x veya daha sonraki sürümler için yükseltilmemiş bazı bileşenler, ortamınızdaki vardır. Sertifikayı yenilemeden önce bileşenleri yükseltin. Eski sürümlerinde yenileyemezsiniz.

### <a name="renew-the-certificate"></a>Sertifikayı Yenile

1. Kasada açmak **Site Recovery altyapısı** > **yapılandırma sunucusu**ve gerekli yapılandırma server'ı tıklatın.
2. Sona erme tarihi altında görünür **yapılandırma sunucusu durumu**
3. Tıklatın **sertifikaları Yenile**. 


## <a name="next-steps"></a>Sonraki adımlar

Olağanüstü durum kurtarma ayarlamak için öğreticileri gözden [VMware Vm'lerini](tutorial-vmware-to-azure.md) ve Azure fiziksel servers(tutorial-physical-to-azure.md).
