---
title: " Fiziksel sunucu olağanüstü durum kurtarma Azure Site Recovery ile yapılandırma sunucusu yönetme | Microsoft Docs"
description: Bu makalede, Azure Site Recovery hizmeti ile Azure, fiziksel sunucu olağanüstü durum kurtarma için mevcut bir yapılandırma sunucusunu yönetmek açıklar.
services: site-recovery
author: AnoopVasudavan
ms.service: site-recovery
ms.topic: article
ms.date: 04/11/2018
ms.author: anoopkv
ms.openlocfilehash: 580d32a51f6b38916ddccd46784b80b1179c29c4
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="manage-the-configuration-server-for-physical-server-disaster-recovery"></a>Fiziksel sunucu olağanüstü durum kurtarma için yapılandırma sunucusunu yönetme

Kullanırken bir şirket içi yapılandırma sunucusunu ayarlama [Azure Site Recovery](site-recovery-overview.md) fiziksel sunucuların azure'a olağanüstü durum kurtarma için hizmet. Yapılandırma sunucusu şirket içi makineler ve Azure arasındaki iletişimi düzenler ve veri çoğaltma işlemlerini yönetir. Bu makalede dağıtımı yapıldıktan sonra yapılandırma sunucusunu yönetmek için ortak görevler özetlenmektedir.

## <a name="prerequisites"></a>Önkoşullar

Tablo, şirket içi yapılandırma sunucusu makine dağıtmak için prerequistes özetler.

| **Bileşen** | **Gereksinim** |
| --- |---|
| CPU çekirdekleri| 8 |
| RAM | 16 GB|
| Disk sayısı | işletim sistemi diski, işlem sunucusu önbellek disk ve yeniden çalışma için bekletme sürücüsü dahil olmak üzere 3 |
| Boş disk alanı (işlem sunucusu önbelleği) | 600 GB
| Boş disk alanı (bekletme diski) | 600 GB|
| İşletim sistemi  | Windows Server 2012 R2 <br> Windows Server 2016 |
| İşletim sistemi yerel ayarı | English (US)|
| VMware vSphere PowerCLI sürümü | [PowerCLI 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1 "PowerCLI 6.0")|
| Windows Server rolleri | Bu rolleri etkinleştirme: <br> - Active Directory Domain Services <br>- İnternet Bilgi Hizmetleri <br> - Hyper-V |
| Grup İlkeleri| Bu grup ilkeleri etkinleştirme: <br> -Komut istemi erişimi engelle <br> -Kayıt defteri düzenleme araçları erişimi engelle <br> -Dosya ekleri için mantığı güven <br> -Komut dosyası yürütme Aç <br> [Daha fazla bilgi](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)|
| IIS | -Önceden var olan varsayılan Web sitesi <br> -Etkinleştirin [anonim kimlik doğrulaması](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) <br> -Etkinleştirin [Fastcgı](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) ayarı  <br> -Önceden varolan Web sitesi/443 numaralı bağlantı noktasını dinlemeye uygulama<br>|
| NIC türü | (VMware VM olarak dağıtıldığında) VMXNET3 |
| IP adresi türü | Statik |
| İnternet erişimi | Sunucunun aşağıdaki URL'lere erişim gerekir: <br> - \*.accesscontrol.windows.net<br> - \*.backup.windowsazure.com <br>- \*.store.core.windows.net<br> - \*.blob.core.windows.net<br> - \*.hypervrecoverymanager.windowsazure.com <br> - dc.services.visualstudio.com <br> - https://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi (genişleme işlem sunucuları için gerekli değildir) <br> - time.nist.gov <br> - time.windows.com |
| Bağlantı Noktaları | 443 (Denetim kanalı düzenleme)<br>9443 (Veri aktarımı)|

## <a name="download-the-latest-installation-file"></a>En son yükleme dosyasını indirin

Yapılandırma sunucusu yükleme dosyasının en son sürümünü Site kurtarma Portalı'nda kullanılabilir. Ayrıca, bunu doğrudan indirilebilir [Microsoft Download Center](http://aka.ms/unifiedsetup).

1. Azure portalında oturum açın ve kurtarma Hizmetleri Kasası'na göz atın.
2. Gözat **Site Recovery altyapısı** > **yapılandırma sunucularına** (altında VMware ve fiziksel makineler için).
3. Tıklatın **+ sunucuları** düğmesi.
4. Üzerinde **Sunucu Ekle** sayfasında, kayıt anahtarını indirin için karşıdan yükleme düğmesini tıklatın. Azure Site Recovery hizmeti ile kaydetmek için yapılandırma sunucusu yüklemesi sırasında bu anahtar gerekir.
5. Tıklatın **Microsoft Azure Site Recovery birleşik Kurulumu karşıdan** yapılandırma sunucusunun en son sürümünü indirmek için bağlantı.

  ![Sayfayı Yükle](./media/physical-manage-configuration-server/downloadcs.png)


## <a name="install-and-register-the-server"></a>Yükleyin ve sunucuyu kaydetmek

1. Birleşik Kurulum yükleme dosyasını çalıştırın.
2. İçinde **başlamadan önce**seçin **işlem sunucusu ve yapılandırma sunucusu yüklemek**.

    ![Başlamadan önce](./media/physical-manage-configuration-server/combined-wiz1.png)

3. MySQL indirip yüklemek için **Üçüncü Taraf Yazılım Lisansı** bölümünde **Kabul Ediyorum**’a tıklayın.
4. **İnternet Ayarları** alanında, yapılandırma sunucusunda çalışan Sağlayıcının Azure Site Recovery'ye İnternet üzerinden nasıl bağlanacağını belirtin. Gerekli URL'lere izin verdiğiniz emin olun.

    - Şu anda makinede select ayarlanıp proxy ile bağlanmak isterseniz **Azure Site Recovery proxy sunucu kullanma Bağlan**.
    - Sağlayıcı doğrudan bağlanmasını istiyorsanız seçin **Azure Site Recovery bir proxy sunucu olmadan doğrudan bağlan**.
    - Mevcut proxy kimlik doğrulaması gerektiriyorsa veya özel bir ara sunucu sağlayıcısı bağlantılarında kullanmak istiyorsanız, **özel proxy ayarlarıyla Bağlan**, adresini, bağlantı noktası ve kimlik bilgilerini belirtin.
     ![Güvenlik duvarı](./media/physical-manage-configuration-server/combined-wiz4.png)
6. **Önkoşul Denetimi** menüsünde Kurulum, yüklemenin çalışabildiğinden emin olmak üzere bir denetim gerçekleştirir. **Genel saat eşitleme denetimi** hakkında bir uyarı görünürse, sistem saatindeki zamanın (**Tarih ve Saat** ayarları) saat dilimiyle aynı olduğunu doğrulayın.

    ![Önkoşullar](./media/physical-manage-configuration-server/combined-wiz5.png)
7. **MySQL Yapılandırması** menüsünde, yüklü MySQL sunucu örneğinde oturum açmak için kimlik bilgileri oluşturun.

    ![MySQL](./media/physical-manage-configuration-server/combined-wiz6.png)
8. **Ortam Ayrıntıları**’nda VMware sanal makinelerini çoğaltıp çoğaltmayacağınızı seçin. Varsa, Kurulum Powerclı 6.0 yüklü olduğunu denetler.
9. **Yükleme Konumu** alanında ikili dosyaları yüklemek ve önbelleği depolamak istediğiniz konumu seçin. Seçtiğiniz sürücü en az 5 GB kullanılabilir disk alanına sahip olmalıdır, ancak en az 600 GB boş alanı olan bir önbellek sürücüsü seçmeniz önerilir.

    ![Yükleme konumu](./media/physical-manage-configuration-server/combined-wiz8.png)
10. **Ağ Seçimi** menüsünde, yapılandırma sunucusunun çoğaltma verilerini gönderip aldığı dinleyiciyi (ağ bağdaştırıcısı ve SSL bağlantı noktası) seçin. Bağlantı noktası 9443, çoğaltma trafiğini gönderip almak için kullanılan varsayılan bağlantı noktasıdır, ancak bu bağlantı noktası numarasını ortamınızın gereksinimlerine uyacak şekilde değiştirebilirsiniz. Bağlantı noktası 9443’e ek olarak, çoğaltma işlemlerini düzenlemek için web sunucusu tarafından kullanılan bağlantı noktası 443 de açılır. Bağlantı noktası 443 gönderirken ya da çoğaltma trafiğini alırken için kullanmayın.

    ![Ağ seçimi](./media/physical-manage-configuration-server/combined-wiz9.png)


11. **Özet** alanındaki bilgileri gözden geçirin ve **Yükle**’ye tıklayın. Yükleme tamamlandığında bir parola oluşturulur. Çoğaltmayı etkinleştirdiğinizde bu parola gerekli olacaktır; bu yüzden kopyalayıp güvenli bir yerde saklayın.


Kayıt tamamlandıktan sonra, sunucu kasadaki **Ayarlar** > **Sunucular** dikey penceresinde görüntülenir.


## <a name="install-from-the-command-line"></a>Komut satırından yükleyin

Yükleme dosyasını aşağıdaki gibi çalıştırın:

  ```
  UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]
  ```

### <a name="sample-usage"></a>Örnek Kullanım
  ```
  MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /xC:\Temp\Extracted
  cd C:\Temp\Extracted
  UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "CS" /InstallLocation "D:\" /MySQLCredsFilePath "C:\Temp\MySQLCredentialsfile.txt" /VaultCredsFilePath "C:\Temp\MyVault.vaultcredentials" /EnvType "VMWare"
  ```


### <a name="parameters"></a>Parametreler

|Parametre Adı| Tür | Açıklama| Değerler|
|-|-|-|-|
| /ServerMode|Gerekli|Hem yapılandırma hem de işlem sunucusunun mu yoksa yalnızca işlem sunucusunun mu yükleneceğini belirtir|CS<br>PS|
|/InstallLocation|Gerekli|Bileşenlerin yüklendiği klasör| Bilgisayardaki herhangi bir klasör|
|/MySQLCredsFilePath|Gerekli|MySQL sunucusu kimlik bilgilerinin depolandığı dosya yolu|Dosya aşağıda belirtilen biçimde olmalıdır|
|/VaultCredsFilePath|Gerekli|Kasa kimlik bilgileri dosyasının yolu|Geçerli dosya yolu|
|/EnvType|Gerekli|Korumak istediğiniz ortam türü |VMware<br>NonVMware|
|/PSIP|Gerekli|Çoğaltma veri aktarımı için kullanılacak NIC’nin IP adresi| Herhangi bir geçerli IP adresi|
|/CSIP|Gerekli|Yapılandırma sunucusunun dinleme yaptığı NIC’nin IP adresi| Herhangi bir geçerli IP adresi|
|/PassphraseFilePath|Gerekli|Parola dosyası konumunun tam yolu|Geçerli dosya yolu|
|/BypassProxy|İsteğe bağlı|Yapılandırma sunucusunun Azure'a bir ara sunucu olmadan bağlandığını belirtir|Yapmak için bu değeri Venu’den alın|
|/ProxySettingsFilePath|İsteğe bağlı|Ara sunucu ayarları (Varsayılan ara sunucu kimlik doğrulaması gerektirir ya da özel bir ara sunucu kullanılır)|Dosya aşağıda belirtilen biçimde olmalıdır|
|DataTransferSecurePort|İsteğe bağlı|Çoğaltma verileri için kullanılacak PSIP’deki bağlantı noktası numarası| Geçerli Bağlantı Noktası Numarası (varsayılan değer: 9433)|
|/SkipSpaceCheck|İsteğe bağlı|Önbellek diski için alan denetimini atlama| |
|/AcceptThirdpartyEULA|Gerekli|Bayrak, üçüncü taraf EULA'nın kabul edildiğini gösterir| |
|/ShowThirdpartyEULA|İsteğe bağlı|Üçüncü taraf EULA belgesini görüntüler. Giriş olarak sağlanırsa, diğer tüm parametreler yoksayılır| |



### <a name="create-file-input-for-mysqlcredsfilepath"></a>Dosya giriş MYSQLCredsFilePath için oluşturma

MySQLCredsFilePath parametresi bir dosya girdi olarak alır. Aşağıdaki biçimi kullanarak dosya oluşturma ve giriş MySQLCredsFilePath parametresi olarak geçirin.
```
[MySQLCredentials]
MySQLRootPassword = "Password>"
MySQLUserPassword = "Password"
```
### <a name="create-file-input-for-proxysettingsfilepath"></a>Dosya giriş ProxySettingsFilePath için oluşturma
ProxySettingsFilePath parametresi bir dosya girdi olarak alır. Aşağıdaki biçimi kullanarak dosya oluşturma ve giriş ProxySettingsFilePath parametresi olarak geçirin.

```
[ProxySettings]
ProxyAuthentication = "Yes/No"
Proxy IP = "IP Address"
ProxyPort = "Port"
ProxyUserName="UserName"
ProxyPassword="Password"
```
## <a name="modify-proxy-settings"></a>Proxy ayarlarını değiştirme

Yapılandırma sunucusu makine için proxy ayarlarını aşağıdaki gibi değiştirebilirsiniz:

1. Yapılandırma sunucusunda oturum açın.
2. Kısayol kullanarak cspsconfigtool.exe başlatın.
3. Tıklatın **kasa kayıt** sekmesi.
4. Yeni bir kasa kayıt dosyası portaldan indirmenizi ve aracı giriş olarak sağlayın.

  ![YAZMAÇ yapılandırma sunucusu](./media/physical-manage-configuration-server/register-csconfiguration-server.png)
5. Yeni proxy ayrıntılarını girin ve tıklayın **kaydetmek** düğmesi.
6. Bir yönetici PowerShell komut penceresi açın.
7. Şu komutu çalıştırın:
  ```
  $pwd = ConvertTo-SecureString -String MyProxyUserPassword
  Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
  net stop obengine
  net start obengine
  ```

  >[!WARNING]
  Yapılandırma sunucusuna bağlı ek işlem sunucularınız varsa, gerek [proxy ayarları tüm genişleme işlem sunucularındaki düzeltme](vmware-azure-manage-process-server.md#modify-proxy-settings-for-an-on-premises-process-server) dağıtımınızdaki.

## <a name="reregister-a-configuration-server-with-the-same-vault"></a>Yapılandırma sunucusunu aynı Kasayla birlikte yeniden kaydetme
  1. Yapılandırma sunucunuza oturum açın.
  2. Masaüstünde kısayol kullanarak cspsconfigtool.exe başlatın.
  3. Tıklatın **kasa kayıt** sekmesi.
  4. Yeni bir kayıt dosyası portaldan indirmenizi ve aracı giriş olarak sağlayın.
        ![register-configuration-server](./media/physical-manage-configuration-server/register-csconfiguration-server.png)
  5. Proxy sunucu bilgileri sağlayın ve tıklatın **kaydetmek** düğmesi.  
  6. Bir yönetici PowerShell komut penceresi açın.
  7. Aşağıdaki komutu çalıştırın

      ```
      $pwd = ConvertTo-SecureString -String MyProxyUserPassword
      Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
      net stop obengine
      net start obengine
      ```

  >[!WARNING]
  Birden çok işlem sunucusu varsa, gerek [bunları yeniden kaydettirin](vmware-azure-manage-process-server.md#reregister-a-process-server).

## <a name="register-a-configuration-server-with-a-different-vault"></a>Yapılandırma sunucusu farklı bir kasayla kaydedin

> [!WARNING]
> Geçerli kasa yapılandırma sunucusundan aşağıdaki adımı keser ve yapılandırma sunucusunun altındaki tüm korumalı sanal makineleri çoğaltma durdurulur.

1. Yapılandırma sunucuya oturum açın
2. bir yönetici komut isteminden komutu çalıştırın:

    ```
    reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
    net stop dra
    ```
3. Masaüstünde kısayol kullanarak cspsconfigtool.exe başlatın.
4. Tıklatın **kasa kayıt** sekmesi.
5. Yeni bir kayıt dosyası portaldan indirmenizi ve aracı giriş olarak sağlayın.
6. Proxy sunucu bilgileri sağlayın ve tıklatın **kaydetmek** düğmesi.  
7. Bir yönetici PowerShell komut penceresi açın.
8. Aşağıdaki komutu çalıştırın
    ```
    $pwd = ConvertTo-SecureString -String MyProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
    net stop obengine
    net start obengine
    ```

## <a name="upgrade-a-configuration-server"></a>Bir yapılandırma sunucusunu yükseltme

Yapılandırma sunucusu güncelleştirmek için güncelleştirme paketleri çalıştırın. Güncelleştirmeleri kadar N-4 sürümleri için uygulanabilir. Örneğin:

- 9.7, 9.8, 9.9 veya 9.10 - çalıştırıyorsanız, doğrudan 9.11 yükseltebilirsiniz.
- 9.6 veya önceki bir sürümünü çalıştırıyorsanız ve 9.11 için yükseltme yapmak isterseniz, 9.7 sürümüne yükseltmeniz gerekir. 9.11 önce.

Güncelleştirme paketleri tüm yapılandırma sunucusu sürümlerine yükseltme için bağlantıları kullanılabilir [wiki güncelleştirmeleri sayfası](https://social.technet.microsoft.com/wiki/contents/articles/38544.azure-site-recovery-service-updates.aspx).

Sunucu gibi yükseltin:

1. Güncelleştirme yükleyicisi dosya yapılandırma sunucusuna yükleyin.
2. Yükleyiciyi çalıştırmak için çift tıklayın.
3. Yükleyici, makinede çalışan geçerli sürümü algılar.
4. Tıklatın **Tamam** doğrulayın ve yükseltmeyi çalıştırın. 


## <a name="delete-or-unregister-a-configuration-server"></a>Silme veya yapılandırma sunucusunun kaydı silinemedi

> [!WARNING]
> Yapılandırma sunucusu yetkisini başlamadan önce aşağıdakilerden emin olun.
> 1. [Korumayı devre dışı](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure) bu yapılandırma sunucu altındaki tüm sanal makineler için.
> 2. [İlişkisini](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) ve [silmek](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) yapılandırma sunucusundan tüm çoğaltma ilkeleri.
> 3. [Silme](vmware-azure-manage-vcenter.md#delete-a-vcenter-server) yapılandırma sunucusuna ilişkili tüm Vcenter sunucularını/vSphere ana.


### <a name="delete-the-configuration-server-from-azure-portal"></a>Yapılandırma sunucusu Azure portalından Sil
1. Azure Portal'da Gözat **Site Recovery altyapısı** > **yapılandırma sunucularına** kasa menüsünden.
2. Yetkisini istediğiniz yapılandırma sunucuya tıklayın.
3. Yapılandırma sunucusunun Ayrıntıları sayfasında, tıklatın **silmek** düğmesi.
4. Tıklatın **Evet** sunucu silme işlemini onaylamak için.

### <a name="uninstall-the-configuration-server-and-its-dependencies"></a>Yapılandırma sunucusu ve onun bağımlılıklarını kaldırma
  > [!TIP]
  Yapılandırma sunucusu Azure Site Recovery ile yeniden yeniden kullanmayı planlıyorsanız, daha sonra adım 4 doğrudan atlayabilirsiniz.

1. Yapılandırma sunucusunda bir yönetici olarak oturum açın.
2. Denetim Masası açın > Program > programları Kaldır
3. Aşağıdaki sırayla programları kaldırın:
  * Microsoft Azure Kurtarma Hizmetleri Aracısı
  * Microsoft Azure Site Recovery Mobility hizmeti/ana hedef sunucu
  * Microsoft Azure Site Recovery sağlayıcısı
  * Microsoft Azure Site kurtarma yapılandırması sunucu/işlem sunucusu
  * Microsoft Azure Site kurtarma yapılandırması sunucu bağımlılıkları
  * MySQL Server 5.5
4. Yönetici komut istemi ve aşağıdaki komutu çalıştırın.
  ```
  reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
  ```

## <a name="delete-or-unregister-a-configuration-server-powershell"></a>Silme veya yapılandırma sunucusu (PowerShell) kaydı silinemedi

1. [Yükleme](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.4.0) Azure PowerShell Modülü
2. İçine komutunu kullanarak Azure hesabınızda oturum açın
    
    `Connect-AzureRmAccount`
3. Kasanın var olduğu altında aboneliği seçin

     `Get-AzureRmSubscription –SubscriptionName <your subscription name> | Select-AzureRmSubscription`
3.  Şimdi, kasa bağlamı Ayarla
    
    ```
    $vault = Get-AzureRmRecoveryServicesVault -Name <name of your vault>
    Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault
    ```
4. Yapılandırma sunucusu seçin Al

    `$fabric = Get-AzureRmSiteRecoveryFabric -FriendlyName <name of your configuration server>`
6. Yapılandırma sunucusu Sil

    `Remove-AzureRmSiteRecoveryFabric -Fabric $fabric [-Force] `

> [!NOTE]
> **-Force** Kaldır AzureRmSiteRecoveryFabric seçeneğinde, yapılandırma sunucusu temizleme/silinmesini zorlamak için kullanılabilir.

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




## <a name="common-issues"></a>Genel sorunlar
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]

## <a name="next-steps"></a>Sonraki adımlar

Olağanüstü durum kurtarma ayarlamak için öğreticileri gözden [fiziksel sunucuları](tutorial-physical-to-azure.md) Azure.

