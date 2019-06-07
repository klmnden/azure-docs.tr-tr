---
title: Windows sanal masaüstü Önizleme oturumu konakları - Azure otomatik ölçeklendirme
description: Windows sanal masaüstü Önizleme oturumu konakları için otomatik ölçeklendirme komut dosyasını ayarlama işlemi açıklanmaktadır.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: e9f500e3ab965b9dbfc5e395a6572497c85f6f8f
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66755139"
---
# <a name="automatically-scale-session-hosts"></a>Oturum ana bilgisayarlarını otomatik olarak ölçeklendirme

Azure'da çok sayıda Windows sanal masaüstü Önizleme dağıtımlar için sanal makine maliyetleri önemli toplam Windows Sanal Masaüstü Dağıtım maliyetini bölümünü temsil eder. Maliyetleri azaltmak için kapatma ve yoğun olmayan saatlerde oturum konak sanal makineleri (VM'ler) serbest bırakın ve ardından bunları yeniden kullanımı yoğun zamanlarda en iyisidir.

Bu makalede bir basit ölçeklendirme betik oturumu konak sanal makineler, Windows sanal masaüstü ortamında otomatik ölçeklendirme için kullanır. Ölçeklendirme betik nasıl çalıştığı hakkında daha fazla bilgi için bkz. [ölçeklendirme kodun nasıl çalıştığını](#how-the-scaling-script-works) bölümü.

## <a name="prerequisites"></a>Önkoşullar

Betiğin çalıştırıldığı ortam şunları içermelidir:

- Bir Windows sanal masaüstü Kiracı ve hesap veya bir hizmet sorumlusu (örneğin, RDS'yi katkıda bulunan) söz konusu kiracıyı sorgulamak için gerekli izinlere sahip.
- Oturum Ana havuzuna Vm'leri yapılandırılmış ve sanal masaüstü Windows hizmetine kayıtlı.
- Bir ek sanal zamanlanmış görev, Görev Zamanlayıcısı üzerinden çalışır ve oturumu konakları için ağ erişimi olan makine. Bunun için belgede daha sonra VM scaler adlandırılır.
- [Microsoft Azure Resource Manager PowerShell Modülü](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps) zamanlanmış görev çalışan sanal makinede yüklü.
- [Windows sanal masaüstü PowerShell Modülü](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) zamanlanmış görev çalışan sanal makinede yüklü.

## <a name="recommendations-and-limitations"></a>Öneriler ve sınırlamalar

Ölçeklendirme komut dosyası çalıştırılırken, şunları göz önünde bulundurun:

- Ölçeklendirme bu betik yalnızca ölçeklendirme betiğin çalıştığı zamanlanmış bir görev örneği başına bir konak havuz başa çıkabilir.
- Ölçeklendirme betikleri çalıştırma zamanlanmış görevleri, her zaman açık olan bir VM üzerinde olmalıdır.
- Her örneği ölçeklendirme betik ve yapılandırması için ayrı bir klasör oluşturun.
- Bu betik, çok faktörlü kimlik doğrulaması gerektiren bir Azure AD kullanıcı hesapları ile Windows sanal masaüstüne bir yönetici olarak oturum açmayı desteklemiyor. Azure ve Windows Sanal Masaüstü hizmetini erişmek için hizmet sorumluları kullanmanızı öneririz. İzleyin [Bu öğreticide](create-service-principal-role-powershell.md) PowerShell ile bir hizmet sorumlusu ve bir rol ataması oluşturmak için.
- Azure'nın SLA garantisi yalnızca bir kullanılabilirlik kümesindeki VM'ler için geçerlidir. Belge geçerli sürümü, kullanılabilirlik gereksinimlerini karşılamayabilir ölçeklendirme yapmak tek bir VM ile bir ortam açıklar.

## <a name="deploy-the-scaling-script"></a>Ölçeklendirme öncesinde bir betik dağıtın

Nasıl ölçekleme betiği dağıtmak aşağıdaki yordamları size bildirir.

### <a name="prepare-your-environment-for-the-scaling-script"></a>Ölçeklendirme betik için ortamınızı hazırlama

İlk olarak, ortamınızı ölçeklendirme betiği için hazırlayın:

1. Zamanlanmış görev, bir etki alanı yönetici hesabıyla çalışacak VM (scaler VM) için oturum açın.
2. Scaler ölçeklendirme betik ve yapılandırmasını tutmak için VM üzerinde bir klasör oluşturun (örneğin, **C:\\ölçeklendirme HostPool1**).
3. İndirme **basicScale.ps1**, **Config.xml**, ve **işlevleri PSStoredCredentials.ps1** dosyaları ve **PowershellModules** klasörden [betik deposu ölçeklendirme](https://github.com/Azure/RDS-Templates/tree/master/wvd-sh/WVD%20scaling%20script) ve 2. adımda oluşturduğunuz klasöre kopyalayın. Scaler VM kopyalamadan önce dosyaları almak için iki temel yol vardır:
    - Git deposunu yerel makinenize kopyalayın.
    - Görünüm **ham** her dosyanın sürümünü kopyalayın ve bir metin düzenleyicisi için her bir dosyanın içeriğini yapıştırın ve ardından dosyaları ilgili dosya adını ve dosya türü ile kaydedin. 

### <a name="create-securely-stored-credentials"></a>Güvenli şekilde depolanan kimlik bilgileri oluşturma

Ardından, güvenli şekilde depolanan kimlik bilgileri oluşturmanız gerekir:

1. Yönetici olarak PowerShell ISE'yi açın.
2. RDS PowerShell modülü, aşağıdaki cmdlet'i çalıştırarak içeri aktarın:

    ```powershell
    Install-Module Microsoft.RdInfra.RdPowershell
    ```
    
3. Düzen bölmesini açıp yük **işlevi PSStoredCredentials.ps1** dosya.
4. Aşağıdaki cmdlet'i çalıştırın:
    
    ```powershell
    Set-Variable -Name KeyPath -Scope Global -Value <LocalScalingScriptFolder>
    ```
    
    Örneğin, **Set-değişken - adı KeyPath-genel kapsam-değeri "c:\\ölçeklendirme HostPool1"**
5. Çalıştırma **yeni StoredCredential - KeyPath \$KeyPath** cmdlet'i. İstendiğinde, ana bilgisayar havuzu sorgulamak için gerekli izinlere sahip Windows sanal masaüstü kimlik bilgilerinizi girin (konak havuzu belirtilen **config.xml**).
    - Farklı hizmet sorumluları veya standart hesabı kullanıyorsanız çalıştırın **yeni StoredCredential - KeyPath \$KeyPath** yerel oluşturmak her hesap için depolanan kimlik bilgileri bir kez cmdlet'i.
6. Çalıştırma **StoredCredentials Get-liste** kimlik bilgilerini başarıyla oluşturulduğunu doğrulamak için.

### <a name="configure-the-configxml-file"></a>Config.cml dosyasını yapılandırın

Ölçeklendirme config.xml betik ayarları güncelleştirmek için aşağıdaki alanlara ilgili değerleri girin:

| Alan                     | Açıklama                    |
|-------------------------------|------------------------------------|
| AADTenantId                   | Oturum ana bilgisayarının Vm'leri çalıştırdığı abonelik ilişkilendirir azure AD Kiracı kimliği     |
| AADApplicationId              | Hizmet sorumlusu uygulama kimliği                                                       |
| AADServicePrincipalSecret     | Bu sınama aşamasında girilebilir ancak kimlik bilgileriyle oluşturduktan sonra boş tutulması **PSStoredCredentials.ps1 işlevleri**    |
| currentAzureSubscriptionId    | Oturum ana bilgisayarının Vm'leri çalıştırdığı Azure abonelik kimliği                        |
| tenantname değeri:                    | Windows sanal masaüstü Kiracı adı                                                    |
| hostPoolName                  | Windows sanal masaüstü ana makine havuzu adı                                                 |
| RDBroker                      | Varsayılan değer https WVD Hizmeti'ne URL:\//rdbroker.wvd.microsoft.com             |
| Kullanıcı adı                      | Hizmet sorumlusu uygulama kimliği (AADApplicationId gibi aynı hizmet sorumlusu olması olası) veya standart kullanıcı multi-Factor authentication olmaksızın |
| isServicePrincipal            | Kabul edilen değerler **true** veya **false**. İkinci bir dizi kimlik bilgisi kullanılan bir hizmet sorumlusu olup veya standart hesabı gösterir. |
| BeginPeakTime                 | En yüksek kullanım süresi ne zaman başlar                                                            |
| EndPeakTime                   | En yüksek kullanım süresi sona erdiğinde                                                              |
| TimeDifferenceInHours         | Saat içindeki yerel saati ile UTC arasındaki zaman farkı                                   |
| SessionThresholdPerCPU        | CPU eşiği yeni oturumu ana bilgisayarı VM yoğun saatler sırasında başlatılması gerektiğinde belirlemek için kullanılan başına oturum sayısı.  |
| MinimumNumberOfRDSH           | Konak havuzu Vm'leri kullanımın süre boyunca çalıştırmaya devam etmek için en düşük sayısı             |
| LimitSecondsToForceLogOffUser | Oturumu kapatmak için kullanıcıların zorlama önce beklenecek saniye sayısı. 0 olarak kullanıcıların oturumu kapatmak için zorunlu değildir  |
| LogOffMessageTitle            | Oturumu kapatmak zorunda önce kullanıcıya gönderilen ileti başlığı                  |
| LogOffMessageBody             | Bunlar oturumunuz önce kullanıcılara gönderilen uyarı iletisinin gövdesi. Örneğin, "Bu makinede kapatacak X dakika boyunca menü. "Lütfen çalışmanızı kaydedin ve oturumu kapatın." |

### <a name="configure-the-task-scheduler"></a>Görev Zamanlayıcı'yı yapılandırma

Yapılandırma .xml dosyasını yapılandırdıktan sonra normal bir aralıkta basicScaler.ps1 dosyasını çalıştırmak için Görev Zamanlayıcısı'nı yapılandırmak gerekir.

1. Başlangıç **Görev Zamanlayıcı**.
2. İçinde **Görev Zamanlayıcı** penceresinde **görev oluştur...**
3. İçinde **Görevi Oluştur** iletişim kutusunda **genel** sekmesinde bir **adı** seçin (örneğin, "Dinamik RDSH"), **kullanıcı veya oturum açmamışsa da Çalıştır** ve **en yüksek ayrıcalıklarla çalıştır**.
4. Git **Tetikleyicileri** sekmesine ve ardından seçin **yeni...**
5. İçinde **Yeni Tetikleyici** iletişim altında **Gelişmiş ayarlar**, kontrol **görevi yineleme her** ve uygun dönemi ve süre seçin (örneğin, **15 dakika** veya **süresiz olarak**).
6. Seçin **eylemleri** sekmesi ve **yeni...**
7. İçinde **yeni eylem** iletişim kutusunda girin **powershell.exe** içine **Program/betik** alan ve ardından girin **C:\\ölçeklendirme\\ RDSScaler.ps1** içine **bağımsız değişkenler (isteğe bağlı) ekleme** alan.
8. Git **koşullar** ve **ayarları** sekmeler ve select **Tamam** her biri için varsayılan ayarları kabul etmek için.
9. Ölçeklendirme komut dosyasını çalıştırmak planladığınız yönetici hesabının parolasını girin.

## <a name="how-the-scaling-script-works"></a>Betik ölçeklendirme nasıl çalışır?

Ölçeklendirme bu betik, başlangıç ve bitiş günde en yüksek kullanım süresi de dahil olmak üzere bir config.xml dosyası ayarı okur.

En yüksek kullanım süresi sırasında betik oturumları ve her konak havuzu için çalışan geçerli RDSH kapasite geçerli sayısını denetler. Bu, çalışan oturum ana bilgisayarının Vm'leri oturumlarına config.xml dosyasında tanımlanan SessionThresholdPerCPU parametresi göre desteklemek üzere yeterli kapasitesi varsa hesaplar. Aksi durumda, betik ek oturumu ana bilgisayarı Vm'leri konak havuzunda başlatır.

Kullanımın süre boyunca, betik config.xml dosyasında MinimumNumberOfRDSH parametresi temel VM'ler kapatılmalıdır oturumu ana bilgisayarı belirler. Betik oturum boşaltma modu için ana bilgisayarları bağlama yeni oturumlar önlemek için Vm'leri barındıracak ayarlar. Ayarlarsanız **LimitSecondsToForceLogOffUser** parametresi sıfır olmayan pozitif bir değer config.xml dosyasında betik bildirim gönderilir herhangi kaydedin, yapılandırılan süre bekleyin ve ardından zorlamak için kullanıcılara açmış oturumu kapatmak için kullanıcılar'ı tıklatın. Betik, tüm kullanıcı oturumlarını oturumu konakta VM imzalandığında, sunucuyu kapatın.

Ayarlarsanız **LimitSecondsToForceLogOffUser** parametre sıfıra config.xml dosyasında betik işlem sonrasında konak oturum yapılandırma ayarında kullanıcı oturumlarını imzalama işlemek havuz özellikleri. Bir oturum konakta VM tüm oturumları varsa, oturumu ana bilgisayarı VM bırakır çalışıyor. Tüm oturumları değilseniz, betik oturumu ana bilgisayarı VM kapanır.

Betik, scaler VM sunucusunda Görev Zamanlayıcı'yı kullanarak düzenli aralıklarla çalışacak şekilde tasarlanmıştır. Uzak Masaüstü Hizmetleri ortamınızın boyutuna göre uygun zaman aralığını seçin ve başlatma ve sanal makinelerin kapatılması biraz zaman alabileceğini unutmayın. Her 15 dakikada ölçeklendirme betik çalıştırmanızı öneririz.

## <a name="log-files"></a>Günlük dosyaları

Ölçeklendirme betiği iki günlük dosyası oluşturur **WVDTenantScale.log** ve **WVDTenantUsage.log**. **WVDTenantScale.log** dosya günlüğe olayları ve hataları (varsa) her ölçeklendirme betik yürütme sırasında.

**WVDTenantUsage.log** dosya kayıt etkin çekirdek sayısı ve etkin sanal makine sayısı her zaman ölçekleme betiği yürütün. Microsoft Azure Vm'leri ve maliyet gerçek kullanımını tahmin etmek için bu bilgileri kullanabilirsiniz. Dosya, aşağıdaki bilgileri içeren her bir öğeyle virgülle ayrılmış değerler biçimlendirilir:

>saat, konak havuzu, çekirdek, VM'ler

Bir .csv uzantısı, yüklü Microsoft Excel'e ve analiz için dosya adı da değiştirilebilir.
