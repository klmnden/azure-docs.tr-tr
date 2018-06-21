---
title: Azure Site Recovery ile VMware olağanüstü durum kurtarma için yapılandırma sunucusu yönetme | Microsoft Docs
description: Bu makalede, Azure Site RecoveryS ile azure'a VMware olağanüstü durum kurtarma için mevcut bir yapılandırma sunucusunu yönetmek açıklar.
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: raynew
ms.openlocfilehash: 753e123c660b1aacea1157157f0e580e15c47536
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36287414"
---
# <a name="manage-the-configuration-server-for-vmware-vms"></a>VMware Vm'leri için yapılandırma sunucusu yönetme

Kullanırken bir şirket içi yapılandırma sunucusunu ayarlama [Azure Site Recovery](site-recovery-overview.md) VMware Vm'lerini ve fiziksel sunucuların azure'a olağanüstü durum kurtarma. Yapılandırma sunucusu arasındaki iletişimi düzenler VMware ve Azure şirket içi ve veri çoğaltma yönetir. Bu makalede dağıtıldıktan sonra yapılandırma sunucusunu yönetmek için ortak görevler özetlenmektedir.



## <a name="modify-vmware-settings"></a>VMware ayarlarını değiştirme

Yapılandırma sunucusu gibi erişebilirsiniz:
    - Masaüstü kısayoldan, dağıtılan VM ve Azure Site Recovery Configuration Manager başlatmak için oturum açın.
    - Alternatif olarak, yapılandırma sunucusundan uzaktan erişebilir **https://*ConfigurationServerName*/:44315 /**. Yönetici kimlik bilgilerinizle oturum açın.
   
### <a name="modify-vmware-server-settings"></a>VMware server ayarlarını değiştir

1. Oturum açma sonra farklı bir VMware sunucusu yapılandırma sunucusuyla ilişkilendirmek için seçin **vCenter sunucusu/vSphere ESXi Sunucu Ekle**.
2. Ayrıntılarını girin ve ardından **Tamam**.


### <a name="modify-credentials-for-automatic-discovery"></a>Otomatik bulma için kimlik bilgilerini değiştirme

1. Oturum açma işleminden sonra VMware vm'lerinin otomatik bulma için VMware sunucuya bağlanmak için kullanılan kimlik bilgilerini güncellemek için seçin **Düzenle**.
2. Yeni kimlik bilgilerini girin ve ardından **Tamam**.

    ![VMware değiştirme](./media/vmware-azure-manage-configuration-server/modify-vmware-server.png)


## <a name="modify-credentials-for-mobility-service-installation"></a>Mobility hizmeti yüklemesi için kimlik bilgilerini değiştirme

Mobility hizmeti için çoğaltmayı etkinleştirme VMware Vm'lerini otomatik olarak yüklemek için kullanılan kimlik bilgilerini değiştirin.

1. Oturum açma işleminden sonra Seç **sanal makine kimlik bilgileri yönetme**
2. Yeni kimlik bilgilerini girin ve ardından **Tamam**.

    ![Mobility hizmeti kimlik bilgilerini değiştirme](./media/vmware-azure-manage-configuration-server/modify-mobility-credentials.png)

## <a name="modify-proxy-settings"></a>Proxy ayarlarını değiştirme

Azure Internet erişimi için yapılandırma sunucu makinesi tarafından kullanılan proxy ayarlarını değiştirin. Yapılandırma sunucusu makinesinde çalışan varsayılan işlem sunucusuna ek olarak bir işlem sunucusu makine varsa, her iki makinede ayarlarını değiştirin.

1. Oturum açma işleminden sonra yapılandırma sunucusuna, seçin **bağlantı yönetmek**.
2. Proxy değerlerini güncelleştirin. Ardından **kaydetmek** ayarları güncelleştirmek için.

## <a name="add-a-network-adapter"></a>Bir ağ bağdaştırıcısı ekleyin

Açık sanallaştırma biçimi (OVF) şablonu tek bir ağ bağdaştırıcısı yapılandırma sunucusuyla VM dağıtır.

- Yapabilecekleriniz [ek bağdaştırıcıyı VM'ye ekleyin)](vmware-azure-deploy-configuration-server.md#add-an-additional-adapter), ancak yapılandırma sunucusunu kasaya kaydetmek önce eklemeniz gerekir.
- Bir yapılandırma sunucusunu kasaya kaydetmek sonra eklemek için VM Özellikleri'nde bağdaştırıcı ekleyin. Sonra sunucu kasadaki yeniden kaydettirin gerekir.


## <a name="reregister-a-configuration-server-in-the-same-vault"></a>Bir yapılandırma sunucusunda aynı kasaya yeniden kaydetme

Gerekirse aynı kasada yapılandırma sunucusunu yeniden kaydettirin. Yapılandırma sunucusu makinesinde çalışan varsayılan işlem sunucusuna ek olarak bir ek işlem sunucusu makineniz varsa her iki makine yeniden kaydettirin.


  1. Kasada açmak **Yönet** > **Site Recovery altyapısı** > **yapılandırma sunucularına**.
  2. İçinde **sunucuları**seçin **indirme kayıt anahtarı** kasa kimlik bilgileri dosyası indirilemedi.
  3. Yapılandırma sunucusu makinede oturum açın.
  4. İçinde **%ProgramData%\ASR\home\svsystems\bin**, açık **cspsconfigtool.exe**.
  5. Üzerinde **kasa kayıt** sekmesine **Gözat**, indirdiğiniz kasa kimlik bilgileri dosyasını bulun.
  6. Gerekliyse, proxy sunucusu ayrıntılarını belirtin. Ardından **Kaydet**’i seçin.
  7. Bir yönetici PowerShell komut penceresi açın ve aşağıdaki komutu çalıştırın:

      ```
      $pwd = ConvertTo-SecureString -String MyProxyUserPassword
      Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
      net stop obengine
      net start obengine
      ```

## <a name="upgrade-the-configuration-server"></a>Yapılandırma sunucusu yükseltme

Yapılandırma sunucusu güncelleştirmek için güncelleştirme paketleri çalıştırın. Güncelleştirmeleri kadar N-4 sürümleri için uygulanabilir. Örneğin:

- 9.7, 9.8, 9.9 veya 9.10 çalıştırırsanız, doğrudan 9.11 yükseltebilirsiniz.
- 9.6 veya önceki bir sürümünü çalıştıran ve 9.11 için yükseltme yapmak isterseniz, 9.7 sürümüne yükseltmeniz gerekir. 9.11 önce.

Güncelleştirme paketleri tüm yapılandırma sunucusu sürümlerine yükseltme için bağlantıları kullanılabilir [wiki güncelleştirmeleri sayfası](https://social.technet.microsoft.com/wiki/contents/articles/38544.azure-site-recovery-service-updates.aspx).

Sunucu gibi yükseltin:

1. Kasasına gidin **Yönet** > **Site Recovery altyapısı** > **yapılandırma sunucularına**.
2. Bir güncelleştirme olup olmadığını bağlantısının görüntülenip **aracı sürümü** > sütun.

    ![Güncelleştirme](./media/vmware-azure-manage-configuration-server/update2.png)

1. Güncelleştirme yükleyicisi dosya yapılandırma sunucusuna yükleyin.

    ![Güncelleştirme](./media/vmware-azure-manage-configuration-server/update1.png)

4. Yükleyiciyi çalıştırmak için çift tıklayın.
2. Yükleyici, makinede çalışan geçerli sürümü algılar. Tıklatın **Evet** yükseltme işlemini başlatmak için. 
3. Yükseltme tamamlandığında sunucu yapılandırmasını doğrular.

    ![Güncelleştirme](./media/vmware-azure-manage-configuration-server/update3.png)

4. Tıklatın **son** yükleyici kapatın.


## <a name="delete-or-unregister-a-configuration-server"></a>Silme veya yapılandırma sunucusunun kaydı silinemedi

1. [Korumayı devre dışı](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure) yapılandırma sunucusu altında yer alan tüm VM'ler için.
2. [İlişkisini](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) ve [silmek](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) yapılandırma sunucusundan tüm çoğaltma ilkeleri.
3. [Silme](vmware-azure-manage-vcenter.md#delete-a-vcenter-server) yapılandırma sunucuyla ilişkili olan tüm vCenter sunucularını/vSphere ana.
4. Kasada açmak **Site Recovery altyapısı** > **yapılandırma sunucularına**.
5. Kaldırmak istediğiniz yapılandırma sunucusu seçin. Ardından **ayrıntıları** sayfasında, **silmek**.

    ![Yapılandırma sunucusu Sil](./media/vmware-azure-manage-configuration-server/delete-configuration-server.png)
   

### <a name="delete-with-powershell"></a>PowerShell ile silme

PowerShell kullanarak, yapılandırma sunucusu isteğe bağlı olarak silebilirsiniz.

1. [Yükleme](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.4.0) Azure PowerShell modülü.
2. Bu komutu kullanarak Azure hesabınızda oturum açın:
    
    `Connect-AzureRmAccount`
3. Kasa abonelik seçin.

     `Get-AzureRmSubscription –SubscriptionName <your subscription name> | Select-AzureRmSubscription`
3.  Kasa bağlamını ayarlayın.
    
    ```
    $vault = Get-AzureRmRecoveryServicesVault -Name <name of your vault>
    Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault
    ```
4. Yapılandırma sunucusu alın.

    `$fabric = Get-AzureRmSiteRecoveryFabric -FriendlyName <name of your configuration server>`
6. Yapılandırma sunucusu silin.

    `Remove-AzureRmSiteRecoveryFabric -Fabric $fabric [-Force] `

> [!NOTE]
> Kullanabileceğiniz **-Force** Kaldır AzureRmSiteRecoveryFabric seçeneğinde zorla silinmesini yapılandırma sunucusu için.
 


## <a name="renew-ssl-certificates"></a>SSL sertifikaları yenile

Mobility hizmeti, işlemi sunucuları ve ona bağlı olan ana hedef sunucusu etkinliklerini düzenler bir yerleşik web sunucusu yapılandırma sunucusu vardır. Web sunucusu, istemcilerin kimliğini doğrulamak için bir SSL sertifikası kullanır. Sertifika üç yıl sonra dolar ve herhangi bir zamanda yenilenebilir.

### <a name="check-expiry"></a>Süre sonu denetleyin

Mayıs 2016 önce yapılandırma sunucu dağıtımları için bir yıl için sertifika süre sonu ayarlandı. Süresi dolacak şekilde giderek bir sertifikanız varsa, aşağıdakiler gerçekleşir:

- Ne zaman sona erme tarihi iki ay ya da daha düşük (Site Recovery bildirimlerine abone değilse) bildirimleri Portalı'nda ve e-posta ile gönderme hizmetini başlatır.
- Bir bildirim başlığı kasası kaynak sayfasında görüntülenir. Daha fazla bilgi için başlık seçin.
- Görürseniz bir **Şimdi Yükselt** düğmesini gösteren 9.4.xxxx.x veya daha sonraki sürümler için bazı bileşenler, ortamınızdaki yükseltilmemiş. Sertifikayı yenilemeden önce bileşenleri yükseltin. Eski sürümlerinde yenileyemezsiniz.

### <a name="renew-the-certificate"></a>Sertifikayı Yenile

1. Kasada açmak **Site Recovery altyapısı** > **yapılandırma sunucusu**. Gerekli yapılandırma sunucusunu seçin.
2. Sona erme tarihi altında görünür **yapılandırma sunucusu durumu**.
3. Seçin **sertifikaları Yenile**. 


## <a name="next-steps"></a>Sonraki adımlar

Olağanüstü durum kurtarma ayarlamak için öğreticileri gözden [VMware Vm'lerini](vmware-azure-tutorial.md) azure'a.
