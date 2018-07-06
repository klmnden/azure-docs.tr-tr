---
title: Azure Site Recovery ile VMware olağanüstü durum kurtarması için yapılandırma sunucusunu yönetme | Microsoft Docs
description: Bu makale, Vmware'den azure'a Azure Site RecoveryS ile olağanüstü durum kurtarma için var olan bir yapılandırma sunucusunu yönetme açıklamaktadır.
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: raynew
ms.openlocfilehash: 29fa232e328ec0b16cb4e00fb16e3be458936cd7
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37857265"
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
      net stop obengine
      net start obengine
      ```

## <a name="upgrade-the-configuration-server"></a>Yapılandırma sunucusunu yükseltme

Yapılandırma sunucusunu güncelleştirmek için güncelleştirme paketleri çalıştırdığınız. Güncelleştirmeleri kadar N-4 sürümleri için uygulanabilir. Örneğin:

- 9.7, 9.8, 9.9 veya 9.10 çalıştırırsanız, doğrudan 9.11 için yükseltebilirsiniz.
- 9.6 veya önceki bir sürümünü çalıştırın ve 9.11 için yükseltmek istiyorsanız 9.7 sürümüne yükseltmeniz gerekir. 9.11 önce.

Güncelleştirme paketleri yapılandırma sunucusundaki tüm sürümlerine yükseltme için bağlantılar kullanılabilir [wiki güncelleştirmeleri sayfası](https://social.technet.microsoft.com/wiki/contents/articles/38544.azure-site-recovery-service-updates.aspx).

Sunucuyu aşağıdaki gibi yükseltin:

1. Kasaya Git **Yönet** > **Site Recovery altyapısı** > **Configuration Servers**.
2. Bir güncelleştirme varsa, bir bağlantı görünür **aracı sürümü** > sütun.
    ![Güncelleştirme](./media/vmware-azure-manage-configuration-server/update2.png)
3. Güncelleştirme yükleyicisi dosya yapılandırma sunucusuna indirin.

    ![Güncelleştirme](./media/vmware-azure-manage-configuration-server/update1.png)

4. Yükleyiciyi çalıştırmak için çift tıklayın.
5. Yükleyici, makine üzerinde çalışan geçerli sürümünü algılar. Tıklayın **Evet** yükseltmeyi başlatmak için.
6. Yükseltme tamamlandığında sunucu yapılandırmasını doğrular.

    ![Güncelleştirme](./media/vmware-azure-manage-configuration-server/update3.png)
    
7. Tıklayın **son** yükleyici kapatın.

## <a name="delete-or-unregister-a-configuration-server"></a>Silme veya kaydını iptal yapılandırma sunucusu

1. [Korumayı devre dışı](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure) yapılandırma sunucusu altında yer alan tüm VM'ler için.
2. [İlişkisini](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) ve [Sil](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) yapılandırma sunucusundan tüm çoğaltma ilkeleri.
3. [Silme](vmware-azure-manage-vcenter.md#delete-a-vcenter-server) yapılandırma sunucusuyla ilişkili tüm vCenter sunucuları/vSphere konakları.
4. Kasası'nda açın **Site Recovery altyapısı** > **Configuration Servers**.
5. Kaldırmak istediğiniz yapılandırma sunucusunu seçin. Ardından **ayrıntıları** sayfasında **Sil**.

    ![Yapılandırma sunucusunu silme](./media/vmware-azure-manage-configuration-server/delete-configuration-server.png)
   

### <a name="delete-with-powershell"></a>PowerShell ile silme

İsteğe bağlı olarak, PowerShell kullanarak yapılandırma sunucusunu silebilirsiniz.

1. [Yükleme](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.4.0) Azure PowerShell modülü.
2. Bu komutu kullanarak Azure hesabınızda oturum açın:
    
    `Connect-AzureRmAccount`
3. Kasa aboneliğini seçin.

     `Get-AzureRmSubscription –SubscriptionName <your subscription name> | Select-AzureRmSubscription`
3.  Kasa bağlamını ayarlayın.
    
    ```
    $vault = Get-AzureRmRecoveryServicesVault -Name <name of your vault>
    Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault
    ```
4. Yapılandırma sunucusunu alır.

    `$fabric = Get-AzureRmSiteRecoveryFabric -FriendlyName <name of your configuration server>`
6. Yapılandırma sunucusu silme.

    `Remove-AzureRmSiteRecoveryFabric -Fabric $fabric [-Force] `

> [!NOTE]
> Kullanabileceğiniz **-Force** AzureRmSiteRecoveryFabric Kaldır seçeneği için yapılandırma sunucusunu zorla silme işlemi.
 
## <a name="generate-configuration-server-passphrase"></a>Yapılandırma sunucusu parolası oluşturma

1. Yapılandırma sunucunuzda oturum açın ve ardından yönetici olarak bir komut istemi penceresi açın.
2. Bin klasörüne erişmeye dizini değiştirmek için komutu yürütün **cd %ProgramData%\ASR\home\svsystems\bin**
3. Parola dosyası oluşturmak için yürütme **genpassphrase.exe - v > MobSvc.passphrase**.
4. Parolanız konumundaki dosyasında depolanan **%ProgramData%\ASR\home\svsystems\bin\MobSvc.passphrase**.

## <a name="renew-ssl-certificates"></a>SSL sertifikalarını yenileme

Mobility hizmeti, işlem sunucusu ve ona bağlı olan ana hedef sunucusu etkinliklerini düzenleyen bir yerleşik web sunucusuna yapılandırma sunucusu vardır. Web sunucusu istemcilerin kimliğini doğrulamak için bir SSL sertifikası kullanır. Sertifika üç yıl sonra süresi dolar ve herhangi bir zamanda yenilenebilir.

### <a name="check-expiry"></a>Süre sonu denetleyin

Mayıs 2016 önce yapılandırma sunucusu dağıtımları için sertifika süre sonu bir yıla ayarlanır. Sona erecek bir sertifika varsa, aşağıdakiler gerçekleşir:

- Ne zaman sona erme tarihi olan iki ay veya daha az (Site Recovery bildirimlerine abone olursa) portalında ve e-posta bildirimleri gönderme hizmetini başlatır.
- Kasa kaynak sayfasında bir bildirim başlığı görüntülenir. Daha fazla bilgi için başlığı seçin.
- Görürseniz bir **Şimdi Yükselt** düğmesini gösteren 9.4.xxxx.x veya üzeri sürümler için bazı bileşenler, ortamınızdaki yükseltilmemiş. Sertifikayı yenilemek önce bileşenleri yükseltin. Eski sürümlerine yeniden yenilenemiyor.

### <a name="renew-the-certificate"></a>Sertifikayı Yenile

1. Kasası'nda açın **Site Recovery altyapısı** > **yapılandırma sunucusu**. Gerekli yapılandırma sunucusunu seçin.
2. Sona erme tarihi altında görünür **Configuration Server sistem durumu**.
3. Seçin **sertifikalarını yenilemesi**. 


## <a name="next-steps"></a>Sonraki adımlar

Olağanüstü durum kurtarma işlemini ayarlama öğreticileri gözden [VMware Vm'lerini](vmware-azure-tutorial.md) azure'a.
