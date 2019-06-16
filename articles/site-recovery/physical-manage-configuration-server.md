---
title: Şirket içi fiziksel sunucuları Azure Site Recovery ile azure'a olağanüstü durum kurtarması için yapılandırma sunucusunu yönetme | Microsoft Docs
description: Bu makalede, Azure'da fiziksel sunucu olağanüstü durum kurtarma için Azure Site Recovery yapılandırma sunucusunu yönetme konusunda açıklanır.
services: site-recovery
author: mayurigupta13
ms.service: site-recovery
ms.topic: article
ms.date: 02/28/2019
ms.author: mayg
ms.openlocfilehash: 10bec01a3b90776c8dd8c32a74ba7754264da131
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62119747"
---
# <a name="manage-the-configuration-server-for-physical-server-disaster-recovery"></a>Fiziksel sunucu olağanüstü durum kurtarma için yapılandırma sunucusunu yönetme

Kullanırken bir şirket içi yapılandırma sunucusu ayarlama [Azure Site Recovery](site-recovery-overview.md) fiziksel sunucularını azure'a olağanüstü durum kurtarma hizmeti. Yapılandırma sunucusu, şirket içi makinelerin ve Azure arasındaki iletişimi düzenler ve veri çoğaltma işlemlerini yönetir. Bu makalede dağıtıldıktan sonra yapılandırma sunucusunu yönetmek için ortak görevler özetlenir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

Tablo, şirket içi yapılandırma sunucusu makine dağıtmak için gereken önkoşulları özetler.

| **Bileşen** | **Gereksinim** |
| --- |---|
| CPU çekirdekleri| 8 |
| RAM | 16 GB|
| Disk sayısı | işletim sistemi diski, işlem sunucusu önbellek diski ve yeniden çalışma için bekletme sürücüsü dahil olmak üzere, 3 |
| Boş disk alanı (işlem sunucusu önbelleği) | 600 GB
| Boş disk alanı (bekletme diski) | 600 GB|
| İşletim sistemi  | Windows Server 2012 R2 <br> Windows Server 2016 |
| İşletim sistemi yerel ayarı | English (US)|
| VMware vSphere PowerCLI sürümü | [PowerCLI 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1 "PowerCLI 6.0")|
| Windows Server rolleri | Bu rolleri etkinleştirmeyin: <br> - Active Directory Domain Services <br>- İnternet Bilgi Hizmetleri <br> - Hyper-V |
| Grup İlkeleri| Bu grup ilkeleri etkinleştirme: <br> -Komut istemine erişimi engelle <br> -Kayıt defteri düzenleme araçlarına erişimi engelleme <br> -Mantıksal dosya ekleri için güven <br> -Betik yürütmeyi açma <br> [Daha fazla bilgi edinin](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)|
| IIS | -Önceden mevcut olan varsayılan Web sitesi <br> -Etkinleştir [anonim kimlik doğrulaması](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) <br> -Etkinleştir [Fastcgı](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) ayarı  <br> -Önceden varolan Web sitesi/443 numaralı bağlantı noktasını dinlemeye uygulama<br>|
| NIC türü | VMXNET3 (VMware VM olarak dağıtıldığında) |
| IP adresi türü | Statik |
| İnternet erişimi | Sunucunun şu URL'lere erişimi gerekir: <br> - \*.accesscontrol.windows.net<br> - \*.backup.windowsazure.com <br>- \*.store.core.windows.net<br> - \*.blob.core.windows.net<br> - \*.hypervrecoverymanager.windowsazure.com <br> - https://management.azure.com <br> - *.services.visualstudio.com <br> - https://dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-community-5.7.20.0.msi (genişleme işlem sunucusu için gerekli değildir) <br> - time.nist.gov <br> - time.windows.com |
| Bağlantı Noktaları | 443 (Denetim kanalı düzenleme)<br>9443 (Veri aktarımı)|

## <a name="download-the-latest-installation-file"></a>Son yükleme dosyasını indirin

Site Recovery portalında yapılandırma sunucusu yükleme dosyasının en son sürümü kullanılabilir. Ayrıca, bunu doğrudan indirilebilir [Microsoft Download Center](https://aka.ms/unifiedsetup).

1. Azure portalında oturum açın ve kurtarma Hizmetleri kasanıza göz atın.
2. Gözat **Site Recovery altyapısı** > **yapılandırma sunucusu** (altında VMware ve fiziksel makineler için).
3. Tıklayın **+ sunucuları** düğmesi.
4. Üzerinde **Sunucusu Ekle** sayfasında, kayıt anahtarını indirmek için indir düğmesine tıklayın. Azure Site Recovery hizmetiyle kaydetmek için yapılandırma sunucusu yüklemesi sırasında bu anahtar gerekir.
5. Tıklayın **Microsoft Azure Site Recovery birleşik Kurulumu indirme** yapılandırma sunucusunu en son sürümünü indirmek için bağlantı.

   ![İndirme sayfası](./media/physical-manage-configuration-server/downloadcs.png)


## <a name="install-and-register-the-server"></a>Yükleme ve sunucuyu kaydetme

1. Birleşik Kurulum yükleme dosyasını çalıştırın.
2. İçinde **başlamadan önce**seçin **yapılandırma sunucusu ve işlem sunucusunu yükle**.

    ![Başlamadan önce](./media/physical-manage-configuration-server/combined-wiz1.png)

3. MySQL indirip yüklemek için **Üçüncü Taraf Yazılım Lisansı** bölümünde **Kabul Ediyorum**’a tıklayın.
4. **İnternet Ayarları** alanında, yapılandırma sunucusunda çalışan Sağlayıcının Azure Site Recovery'ye İnternet üzerinden nasıl bağlanacağını belirtin. Gerekli URL izin verdik emin olun.

    - Şu anda makinede select ayarlanır proxy ile bağlanmak isterseniz **proxy sunucusu kullanarak Azure Site Recovery hizmetine bağlan**.
    - Sağlayıcı doğrudan bağlanmasını istiyorsanız belirleyin **Azure Site Recovery Ara sunucu olmadan doğrudan bağlan**.
    - Var olan ara sunucu kimlik doğrulaması gerektiriyorsa veya sağlayıcı bağlantısı için seçim özel bir ara sunucu kullanmak istiyorsanız **özel ara sunucu ayarlarıyla Bağlan**, adresi, bağlantı noktasını ve kimlik bilgilerini belirtin.
     ![Güvenlik duvarı](./media/physical-manage-configuration-server/combined-wiz4.png)
6. **Önkoşul Denetimi** menüsünde Kurulum, yüklemenin çalışabildiğinden emin olmak üzere bir denetim gerçekleştirir. **Genel saat eşitleme denetimi** hakkında bir uyarı görünürse, sistem saatindeki zamanın (**Tarih ve Saat** ayarları) saat dilimiyle aynı olduğunu doğrulayın.

    ![Önkoşullar](./media/physical-manage-configuration-server/combined-wiz5.png)
7. **MySQL Yapılandırması** menüsünde, yüklü MySQL sunucu örneğinde oturum açmak için kimlik bilgileri oluşturun.

    ![MySQL](./media/physical-manage-configuration-server/combined-wiz6.png)
8. **Ortam Ayrıntıları**’nda VMware sanal makinelerini çoğaltıp çoğaltmayacağınızı seçin. Varsa, Kurulum Powerclı 6.0 yüklü olduğunu denetler.
9. **Yükleme Konumu** alanında ikili dosyaları yüklemek ve önbelleği depolamak istediğiniz konumu seçin. Seçtiğiniz sürücü en az 5 GB kullanılabilir disk alanına sahip olmalıdır, ancak en az 600 GB boş alanı olan bir önbellek sürücüsü seçmeniz önerilir.

    ![Yükleme konumu](./media/physical-manage-configuration-server/combined-wiz8.png)
10. İçinde **Ağ Seçimi**ilk Keşif ve anında iletme kaynak makinede mobility hizmeti yüklemesi için yerleşik bir işlem sunucusunu kullanan NIC'yi seçin ve ardından yapılandırma sunucusu için bağlantı kullanan NIC'yi seçin Azure ile. Bağlantı noktası 9443, çoğaltma trafiğini gönderip almak için kullanılan varsayılan bağlantı noktasıdır, ancak bu bağlantı noktası numarasını ortamınızın gereksinimlerine uyacak şekilde değiştirebilirsiniz. Bağlantı noktası 9443’e ek olarak, çoğaltma işlemlerini düzenlemek için web sunucusu tarafından kullanılan bağlantı noktası 443 de açılır. Bağlantı noktası 443, gönderme veya çoğaltma trafiğini gönderip almak için kullanmayın.

    ![Ağ seçimi](./media/physical-manage-configuration-server/combined-wiz9.png)


11. **Özet** alanındaki bilgileri gözden geçirin ve **Yükle**’ye tıklayın. Yükleme tamamlandığında bir parola oluşturulur. Çoğaltmayı etkinleştirdiğinizde bu parola gerekli olacaktır; bu yüzden kopyalayıp güvenli bir yerde saklayın.


Kayıt tamamlandıktan sonra, sunucu kasadaki **Ayarlar** > **Sunucular** dikey penceresinde görüntülenir.


## <a name="install-from-the-command-line"></a>Komut satırından yükleme

Yükleme dosyasını aşağıdaki gibi çalıştırın:

  ```
  UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]
  ```

### <a name="sample-usage"></a>Örnek kullanımı
  ```
  MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /x:C:\Temp\Extracted
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



### <a name="create-file-input-for-mysqlcredsfilepath"></a>MYSQLCredsFilePath giriş dosyası oluşturma

MySQLCredsFilePath parametresi bir dosya, girdi olarak alır. Aşağıdaki biçimi kullanarak bir dosya oluşturun ve giriş MySQLCredsFilePath parametresi olarak geçirin.
```ini
[MySQLCredentials]
MySQLRootPassword = "Password"
MySQLUserPassword = "Password"
```
### <a name="create-file-input-for-proxysettingsfilepath"></a>ProxySettingsFilePath giriş dosyası oluşturma
ProxySettingsFilePath parametre bir dosya, girdi olarak alır. Aşağıdaki biçimi kullanarak bir dosya oluşturun ve giriş ProxySettingsFilePath parametresi olarak geçirin.

```ini
[ProxySettings]
ProxyAuthentication = "Yes/No"
Proxy IP = "IP Address"
ProxyPort = "Port"
ProxyUserName="UserName"
ProxyPassword="Password"
```
## <a name="modify-proxy-settings"></a>Proxy ayarlarını değiştirme

Makinede yapılandırma sunucusu için proxy ayarlarını aşağıdaki gibi değiştirebilirsiniz:

1. Yapılandırma sunucusuna oturum açın.
2. Masaüstü kısayolunu kullanarak cspsconfigtool.exe'yi başlatın.
3. Tıklayın **kasa kaydı** sekmesi.
4. Portaldan yeni bir kasa kayıt dosyası indirin ve aracı için giriş olarak sağlayın.

   ![kayıt yapılandırma sunucusu](./media/physical-manage-configuration-server/register-csconfiguration-server.png)
5. Yeni proxy ayrıntılarını sağlayın ve tıklayın **kaydetme** düğmesi.
6. Bir yönetici PowerShell komut penceresi açın.
7. Şu komutu çalıştırın:

   ```powershell
   $Pwd = ConvertTo-SecureString -String MyProxyUserPassword
   Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber –ProxyUserName domain\username -ProxyPassword $Pwd
   net stop obengine
   net start obengine
   ```

   > [!WARNING]
   > Yapılandırma sunucusuna bağlı ek işlem sunucularının varsa yapmanız [proxy ayarları tüm genişleme işlem sunucularındaki düzeltme](vmware-azure-manage-process-server.md#modify-proxy-settings-for-an-on-premises-process-server) dağıtımınızdaki.

## <a name="reregister-a-configuration-server-with-the-same-vault"></a>Bir yapılandırma sunucusu ile aynı kasaya yeniden kaydettirin
1. Yapılandırma sunucunuzda oturum açın.
2. Masaüstü kısayolunu kullanarak cspsconfigtool.exe'yi başlatın.
3. Tıklayın **kasa kaydı** sekmesi.
4. Portaldan yeni bir kayıt dosyası indirin ve aracı için giriş olarak sağlayın.
      ![register-configuration-server](./media/physical-manage-configuration-server/register-csconfiguration-server.png)
5. Proxy sunucusu ayrıntıları sağlayın ve tıklayın **kaydetme** düğmesi.  
6. Bir yönetici PowerShell komut penceresi açın.
7. Aşağıdaki komutu çalıştırın

    ```powershell
    $Pwd = ConvertTo-SecureString -String MyProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber –ProxyUserName domain\username -ProxyPassword $Pwd
    net stop obengine
    net start obengine
    ```

   > [!WARNING]
   > Birden çok işlem sunucusu varsa, yapmanız [bunları yeniden kaydettirin](vmware-azure-manage-process-server.md#reregister-a-process-server).

## <a name="register-a-configuration-server-with-a-different-vault"></a>Yapılandırma sunucusunu farklı bir kasaya kaydetme

> [!WARNING]
> Geçerli kasa yapılandırma sunucusundan aşağıdaki adımı ayırır ve yapılandırma sunucusu altında tüm korumalı sanal makinelerin çoğaltma durdurulur.

1. Yapılandırma sunucuya oturum açın
2. bir yönetici komut isteminden komutu çalıştırın:

    ```
    reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
    net stop dra
    ```
3. Masaüstü kısayolunu kullanarak cspsconfigtool.exe'yi başlatın.
4. Tıklayın **kasa kaydı** sekmesi.
5. Portaldan yeni bir kayıt dosyası indirin ve aracı için giriş olarak sağlayın.
6. Proxy sunucusu ayrıntıları sağlayın ve tıklayın **kaydetme** düğmesi.  
7. Bir yönetici PowerShell komut penceresi açın.
8. Aşağıdaki komutu çalıştırın
    ```powershell
    $pwd = ConvertTo-SecureString -String MyProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber –ProxyUserName domain\username -ProxyPassword $pwd
    net stop obengine
    net start obengine
    ```

## <a name="upgrade-a-configuration-server"></a>Yapılandırma sunucusunu yükseltme

Yapılandırma sunucusunu güncelleştirmek için güncelleştirme paketleri çalıştırdığınız. Güncelleştirmeleri kadar N-4 sürümleri için uygulanabilir. Örneğin:

- 9\.7, 9.8, 9.9 veya 9.10 - çalıştırıyorsanız, doğrudan 9.11 için yükseltebilirsiniz.
- 9\.6 veya önceki bir sürümü çalıştırıyorsanız ve 9.11 için yükseltmek istiyorsanız 9.7 sürümüne yükseltmeniz gerekir. 9\.11 önce.

Güncelleştirme paketleri yapılandırma sunucusundaki tüm sürümlerine yükseltme için bağlantılar kullanılabilir [wiki güncelleştirmeleri sayfası](https://social.technet.microsoft.com/wiki/contents/articles/38544.azure-site-recovery-service-updates.aspx).

Sunucuyu aşağıdaki gibi yükseltin:

1. Güncelleştirme yükleyicisi dosya yapılandırma sunucusuna indirin.
2. Yükleyiciyi çalıştırmak için çift tıklayın.
3. Yükleyici, makine üzerinde çalışan geçerli sürümünü algılar.
4. Tıklayın **Tamam** doğrulayın ve yükseltmeyi çalıştırın. 


## <a name="delete-or-unregister-a-configuration-server"></a>Silme veya kaydını iptal yapılandırma sunucusu

> [!WARNING]
> Yapılandırma Sunucusu'nun yetkisini alma işlemine başlamadan önce aşağıdakilerden emin olun.
> 1. [Korumayı devre dışı](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure) yapılandırma bu sunucu kapsamındaki tüm sanal makineler için.
> 2. [İlişkisini](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) ve [Sil](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) yapılandırma sunucusundan tüm çoğaltma ilkeleri.
> 3. [Silme](vmware-azure-manage-vcenter.md#delete-a-vcenter-server) yapılandırma sunucusuna ilişkili olan tüm vCenters sunucularını/vSphere konakları.


### <a name="delete-the-configuration-server-from-azure-portal"></a>Azure portalında yapılandırma sunucusu silme
1. Azure portalında göz **Site Recovery altyapısı** > **Configuration Servers** kasa menüsünden.
2. Yetkisini almak için istediğiniz yapılandırmayı sunucuya tıklayın.
3. Yapılandırma sunucusunun Ayrıntıları sayfasında **Sil** düğmesi.
4. Tıklayın **Evet** sunucu silme işlemini onaylamak için.

### <a name="uninstall-the-configuration-server-and-its-dependencies"></a>Yapılandırma sunucusunu ve bağımlılıklarını Kaldır
> [!TIP]
>   Azure Site Recovery ile Configuration Server'ı yeniden kullanmayı planlıyorsanız, daha sonra adım 4 doğrudan atlayabilirsiniz

1. Yapılandırma sunucusunda yönetici olarak oturum açın.
2. Denetim Masası'nı açın > Program > programları Kaldır
3. Aşağıdaki sırayla Program Kaldır:
   * Microsoft Azure Kurtarma Hizmetleri Aracısı
   * Microsoft Azure Site Recovery Mobility hizmeti/ana hedef sunucusu
   * Microsoft Azure Site Recovery sağlayıcısı
   * Microsoft Azure Site Recovery yapılandırma sunucusu/işlem sunucusu
   * Microsoft Azure Site Recovery yapılandırma sunucusu bağımlılıkları
   * MySQL Server 5.5
4. Yönetici komut istemi ve aşağıdaki komutu çalıştırın.
   ```
   reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
   ```

## <a name="delete-or-unregister-a-configuration-server-powershell"></a>Silme veya kaydını iptal yapılandırma sunucusunu (PowerShell)

1. [Yükleme](https://docs.microsoft.com/powershell/azure/install-Az-ps) Azure PowerShell Modülü
2. Komutunu kullanarak Azure hesabınızda oturum açın
    
    `Connect-AzAccount`
3. Kasa altında mevcut olan aboneliği seçin

     `Get-AzSubscription –SubscriptionName <your subscription name> | Select-AzSubscription`
3.  Artık, kasa bağlamını ayarlayın
    
    ```powershell
    $Vault = Get-AzRecoveryServicesVault -Name <name of your vault>
    Set-AzSiteRecoveryVaultSettings -ARSVault $Vault
    ```
4. Yapılandırma sunucunuzu seçin Al

    `$Fabric = Get-AzSiteRecoveryFabric -FriendlyName <name of your configuration server>`
6. Yapılandırma sunucusunu silme

    `Remove-AzSiteRecoveryFabric -Fabric $Fabric [-Force]`

> [!NOTE]
> **-Force** Remove-AzSiteRecoveryFabric seçeneğinde, yapılandırma sunucusunun kaldırılması/silinmesini zorlamak için kullanılabilir.

## <a name="renew-ssl-certificates"></a>SSL sertifikalarını yenileme
Mobility hizmeti, işlem sunucusu ve ona bağlı olan ana hedef sunucusu etkinliklerini düzenleyen bir yerleşik web sunucusuna yapılandırma sunucusu vardır. Web sunucusu istemcilerin kimliğini doğrulamak için bir SSL sertifikası kullanır. Sertifika, üç yıl sonra süresi dolar ve herhangi bir zamanda yenilenebilir.

### <a name="check-expiry"></a>Süre sonu denetleyin

Mayıs 2016 önce yapılandırma sunucusu dağıtımları için sertifika süre sonu bir yıla ayarlanır. Varsa bir sertifika, aşağıdaki olaylar gerçekleşir'de sona erecek:

- Ne zaman sona erme tarihi olan iki ay veya daha az (Azure Site Recovery bildirimlerine abone olursa) portalında ve e-posta bildirimleri gönderme hizmetini başlatır.
- Kasa kaynak sayfasında bir bildirim başlığı görüntülenir. Daha fazla ayrıntı için başlığa tıklayın.
- Görürseniz bir **Şimdi Yükselt** düğmesi, bu gösterir 9.4.xxxx.x veya üzeri sürümler için yükseltilmemiş bazı bileşenler, ortamınızdaki vardır. Sertifikayı yenilemek önce bileşenleri yükseltin. Eski sürümlerine yeniden yenilenemiyor.

### <a name="renew-the-certificate"></a>Sertifikayı Yenile

1. Kasası'nda açın **Site Recovery altyapısı** > **yapılandırma sunucusu**, gerekli yapılandırma sunucusuna tıklayın.
2. Sona erme tarihi altında görünür **Configuration Server sistem durumu**
3. Tıklayın **sertifikalarını yenilemesi**. 




## <a name="common-issues"></a>Genel sorunlar
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]

## <a name="next-steps"></a>Sonraki adımlar

Olağanüstü durum kurtarma işlemini ayarlama öğreticileri gözden [fiziksel sunucuları](tutorial-physical-to-azure.md) azure'a.

