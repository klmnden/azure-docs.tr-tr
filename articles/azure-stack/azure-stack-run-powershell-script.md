<properties
    pageTitle="Azure Stack POC'yi dağıtma | Microsoft Azure"
    description="Azure Stack POC'nin nasıl hazırlanacağını ve PowerShell betiği çalıştırılarak Azure Stack POC'nin nasıl dağıtılacağını öğrenin."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/29/2016"
    ms.author="erikje"/>


# Azure Stack POC'yi dağıtma
Azure Stack POC'yi dağıtmak için öncelikle [dağıtım makinesini hazırlamanız](#prepare-the-deployment-machine) ve ardından [PowerShell dağıtım betiğini çalıştırmanız](#run-the-powershell-deployment-script) gerekir.

## Dağıtım makinesini hazırlama

1. Dağıtım makinesine fiziksel olarak bağlanabildiğinizden veya fiziksel konsol erişiminizin (örneğin, KVM) olduğundan emin olun. Bu erişime daha sonra 9. adımda dağıtım makinesini yeniden başlattıktan sonra ihtiyacınız olacak.
 
2. Dağıtım makinesinin [minimum gereksinimleri](azure-stack-deploy.md) karşıladığından emin olun. Gereksinimlerinizi doğrulamak için [Deployment Checker for Azure Stack Technical Preview 1](https://gallery.technet.microsoft.com/Deployment-Checker-for-76d824e1) paketini kullanabilirsiniz.

3.  Windows Server 2016 Datacenter Edition Technical Preview 4 EN-US (Tam Sürüm) paketini [indirin](http://aka.ms/ReqOSforAzureStack) ve yükleyin.

4.  Azure Stack POC dağıtım paketini C sürücünüzdeki bir klasöre (örneğin, c:\\AzureStack) [indirin](https://azure.microsoft.com/overview/azure-stack/try/?v=try).

5.  **Microsoft Azure Stack POC .exe** dosyasını çalıştırın.

    Bu işlem sonrasında, şu öğeleri içeren \\Microsoft Azure Stack POC\\ klasörü oluşur:

    -   DeployAzureStack.ps1: Azure Stack POC kurulumu PowerShell betiği

    -   MicrosoftAzureStackPOC.vhdx: Azure Stack veri paketi

    -   SQLServer2014.vhdx: SQL Server VHD

    -   WindowsServer2012R2DatacenterEval.vhd

    -   WindowsServer2016Datacenter.vhdx: Windows Server 2016 Datacenter VHD (KB 3124262 içerir)

    > [AZURE.IMPORTANT] Fiziksel önyükleme biriminde en az 128 GB'lık boş alana sahip olmanız gerekir.

6. WindowsServer2016Datacenter.vhdx öğesini C:\ sürücüsüne kopyalayıp MicrosoftAzureStackPOCBoot.vhdx olarak yeniden adlandırın.

7. Dosya Gezgini'nde, MicrosoftAzureStackPOCBoot.vhdx öğesine sağ tıklayın ve ardından **Bağla**'ya tıklayın.

8. Yönetici olarak bir Komut İstemi penceresi açın ve aşağıdaki bcdboot komutunu çalıştırın. Bu komut, bir ikili önyükleme ortamı oluşturur. Bu noktadan sonra, daha üstteki bir önyükleme seçeneğine önyükleme yapmanız gerekir.

        bcdboot <mounted drive letter>:\windows

9. Makineyi yeniden başlatın. VHD sistemi hazırlandığında otomatik olarak Windows Kurulumunu çalıştırır. Ardından, sonraki adımları tamamlamak için dağıtım makinesine fiziksel olarak bağlanmanız veya fiziksel konsol erişimine sahip olmanız gerekir.

10. BIOS'unuzda böyle bir seçenek varsa UTC saati yerine yerel saati kullanması için BIOS'unuzu yapılandırmanız gerekir.

11. Sorulduğunda ülke, dil, klavye ve diğer tercihlerinizi belirtin. Bir ürün anahtarı sağlamanız istenirse anahtarı [Sistem Gereksinimleri ve Kurulum](https://technet.microsoft.com/library/mt126134.aspx) bölümünde bulabilirsiniz.

12. İstendiğinde, Yönetici hesabına ilişkin parolayı ayarlayın ve ardından bu hesap adı ve parola ile oturum açın.

13. Yeniden başlatıp oturum açtıktan sonra, DHCP'niz yoksa NIC üzerinde statik yapılandırmayı ayarlayın.

14. **Sistem Özellikleri**'ne gidip **Bu bilgisayara uzaktan bağlantıya izin ver** seçeneğini belirleyerek Uzak Masaüstü Bağlantılarını etkinleştirin.

15. Yönetici izinlerine sahip yerel bir hesap kullanarak oturum açın.

16. Azure Stack POC verilerine yönelik **tam olarak** dört sürücü için şunları doğrulayın:
  - Disk yönetiminde görünür olma
  - Kullanımda olmama
  - Çevrimiçi ve Ayrılmamış olarak görünme

17. Ana bilgisayarın bir etki alanına katılmadığını doğrulayın.

18. Internet Explorer'ı kullanarak Azure.com ile ağ bağlantınızı doğrulayın.

> [AZURE.IMPORTANT] TP1 POC dağıtımı, depolama özellikleri için tam olarak dört sürücüyü ve ağ işlemleri için yalnızca bir adet NIC'yi destekler.
>
> - **Depolama için** diğer tüm sürücüleri devre dışı bırakmak üzere cihaz yöneticisini veya WMI'yı kullanın.
>
> - **Ağ için**, birden çok NIC'niz varsa aşağıdaki dağıtım betiğini çalıştırmadan önce yalnızca bir tanesinin etkinleştirildiğinden (ve diğer tüm NIC'lerin devre dışı bırakıldığından) emin olun.
>
> Yukarıda belirtilen VHD önyükleme adımlarını uyguladıysanız VHD'ye önyüklemeden sonra ve dağıtım betiğini başlatmadan önce bu güncelleştirmeleri yapmanız gerekir.

## PowerShell dağıtım betiğini çalıştırma

1. Aşağıdaki dağıtım belgelerini, D:\Microsoft Azure Stack POC\ konumundan C:\Microsoft Azure Stack POC konumuna taşıyın\.
    - DeployAzureStack.ps1
    - MicrosoftAzureStackPOC.vhdx
    - SQLServer14.vhdx
    - WindowsServer2012R2DatacenterEval.vhd
    - WindowsServer2016Datacenter.vhdx

2.  PowerShell'i yönetici olarak açın.

3.  PowerShell'de Azure Stack klasör konumuna (varsayılanı kullandıysanız \\Microsoft Azure Stack POC\\'ye) gidin.

4.  Dağıtma komutunu çalıştırın:

        .\DeployAzureStack.ps1 –Verbose

    Çin'de, bunun yerine şu komutu kullanın:

        .\DeployAzureStack.ps1 –Verbose -UseAADChina $true

    Dağıtım başlar ve Azure Stack POC etki alanı adı, azurestack.local olarak kodlanır.

5.  **Yerleşik yönetici parolasını girin** isteminde bir parola girin ve parolayı doğrulayın. Bu parola, tüm sanal makineler için geçerlidir. Bu Hizmet Yöneticisi parolasını kaydettiğinizden emin olun.

6.  **Açılır Azure kimlik doğrulaması sayfasında lütfen Azure hesabınızda oturum açın** alanında, Microsoft Azure oturum açma iletişim kutusunu açmak için herhangi bir tuşa basın.

7.  Azure Active Directory hesabınızın kimlik bilgilerini girin. Bu kullanıcının, dizin kiracısında Genel Yönetici olması gerekir

8.  PowerShell' geri dönüp hesap seçimi doğrulama isteminde *y* değerini girin. Bu işlem sonrasında, söz konusu dizin kiracısında Azure Stack için şu şekilde iki kullanıcı ve üç uygulama oluşur: Azure Stack için bir yönetici kullanıcı, TiP testleri için bir kiracı kullanıcı ve Portal, API ve İzleme kaynak sağlayıcılarının her biri için bir uygulama. Buna ek olarak yükleyici, söz konusu Dizin Kiracısına; Azure PowerShell, XPlat CLI ve Visual Studio onayları ekler.

9.  **Microsoft Azure Stack POC dağıtılmaya hazır. Devam edilsin mi?** istemine *y* değerini girin.

10.  Dağıtım işlemi birkaç saat sürebilir. Bu süre boyunca sistem birkaç kez otomatik olarak yeniden başlatılır. Dağıtım esnasında oturum açılırsa otomatik olarak, dağıtım ilerlemesinin görüntülendiği bir PowerShell penceresi başlatılır. Dağıtım tamamlandıktan sonra PowerShell penceresi kapanır.

11. Azure Stack POC makinesinde, Azure Stack\yönetici olarak oturum açıp **Sunucu Yöneticisi**'ni açın ve hem yöneticiler hem de kullanıcılar için **IE Artırılmış Güvenlik Yapılandırması** seçeneğini kapatın.

Saat veya tarih hatası nedeniyle dağıtım işlemi başarısız olursa UTC yerine Yerel Saat kullanmak üzere BIOS'u yapılandırın. Ardından, yeniden dağıtım yapın.

Betik başarısız olursa yeniden başlatın. Başarısız olmaya devam ederse betiği silin ve yeniden başlatın.

Betik günlüklerini `C:\ProgramData\microsoft\azurestack` POC ana bilgisayarında bulabilirsiniz.

### DeployAzureStack.ps1 isteğe bağlı parametreleri

**AADCredential** (PSCredential) - Azure Active Directory kullanıcı adını ve parolasını ayarlar. Bu parametre sağlanmazsa betik, kullanıcı adı ve parola ister.

**AADTenant** (string) - Kiracı dizinini ayarlar. AAD hesabının birden çok dizini yönetme izni olduğunda, belirli bir dizini belirtmek için bu parametreyi kullanın. Bu parametre sağlanmazsa betik, dizin ister.

**AdminPassword** (SecureString) - Varsayılan yönetici parolasını ayarlar. Bu parametre sağlanmazsa betik, parola ister.

**Force** (Switch) - Cmdlet'i ayarlayarak doğrulama olmadan çalışmasını sağlar.

**NATVMStaticGateway** (String) - NATVM'ye yönelik statik IP adresinde kullanılan varsayılan ağ geçidini ayarlar. Bu parametreyi yalnızca DHCP'nin, İnternet erişimi için geçerli bir IP adresi atayamadığı durumlarda kullanın. Bu parametreyi kullanırsanız NATVMStaticIP parametresini de kullanmanız gerekir.
Örneğin, `.\DeployAzureStack.ps1 –Verbose -NATVMStaticIP 10.10.10.10/24 – NATVMStaticGateway 10.10.10.1`

**NATVMStaticIP** (string) - NATVM'ye yönelik ek bir statik IP adresi ayarlar. Bu parametreyi yalnızca DHCP'nin, İnternet erişimi için geçerli bir IP adresi atayamadığı durumlarda kullanın.
Örneğin, `.\DeployAzureStack.ps1 –Verbose -NATVMStaticIP 10.10.10.10/24`

**NoAutoReboot** (Switch) - Betiği ayarlayarak, otomatik yeniden başlatma olmadan çalışmasını sağlar.

**ProxyServer** (String) - Ara sunucu bilgilerini ayarlar. Bu parametreyi yalnızca ortamınızın, İnternet'e erişmek için bir ara sunucu kullanması gereken durumlarda kullanın. Kimlik bilgileri gerektiren ara sunucular desteklenmez.
Örneğin, `.\DeployAzureStack.ps1 -Verbose -ProxyServer 172.11.1.1:8080`

**PublicVLan** (String) - VLAN kimliğini ayarlar. Bu parametreyi yalnızca ana bilgisayar ve NATVM'nin, fiziksel ağa (ve İnternet'e) erişmek için VLAN kimliğini yapılandırması gereken durumlarda kullanın.
Örneğin, `.\DeployAzureStack.ps1 –Verbose –PublicVLan 305`

**TIPServiceAdminCredential** (PSCredential) - Mevcut bir hizmet yöneticisi Azure Active Directory hesabının kimlik bilgilerini ayarlar. Bu hesap, TiP (Üretim Sınaması) tarafından kullanılır. Bu parametre sağlanmazsa otomatik olarak bir hesap oluşturulur.

**TIPTenantAdminCredential** (PSCredential) - Mevcut kiracı yöneticisi Azure Active Directory hesabının kimlik bilgilerini ayarlar. Bu hesap, TiP (Üretim Sınaması) tarafından kullanılır. Bu parametre sağlanmazsa otomatik olarak bir hesap oluşturulur.

**UseAADChina**(Boolean) - Azure Çin (Mooncake) ile Microsoft Azure Stack POC'yi dağıtmak istiyorsanız bu Boolean parametresini $true olarak ayarlayın.

## Otomatik TiP testlerini kapatma (isteğe bağlı)

Microsoft Azure Stack Technical Preview 1 paketi, dağıtım işleminde yinelenen bir günlük zamanlamaya göre kullanılan doğrulama testlerinin bir kümesini içerir. Bu testler, Azure Stack kiracısı tarafından gerçekleştirilen eylemlerin benzetimini yapar ve testlerin çalıştırılması için Azure Active Directory'nizde Test-in-POC (TiP) kullanıcı hesapları oluşturulur. Başarılı bir dağıtımın ardından bu TiP testlerini kapatabilirsiniz. 

**Otomatik TiP testlerini kapatma**

  - ClientVM'de şu cmldet'i çalıştırın:

  `Disable-ScheduledTask -TaskName AzureStackSystemvalidationTask`

**Test sonuçlarını görüntüleme**

  - ClientVM'de şu cmldet'i çalıştırın:

  `Get-AzureStackTiPTestsResult`



## Microsoft Azure Stack POC telemetrisini kapatma (isteğe bağlı)


Microsoft Azure Stack POC'yi dağıtmadan önce, dağıtımın gerçekleştiği makinede Microsoft Azure Stack telemetrisini kapatabilirsiniz. Bu özelliği tek bir makinede kapatmak için lütfen [http://windows.microsoft.com/tr-TR/windows-10/feedback-diagnostics-privacy-faq](http://windows.microsoft.com/en-us/windows-10/feedback-diagnostics-privacy-faq) makalesine göz atıp **Tanılama ve kullanım verileri** ayarını **Temel** olarak değiştirin.



Microsoft Azure Stack POC'yi dağıttıktan sonra, Azure Stack etki alanına katılan sanal makinelerin tümünde telemetriyi kapatabilirsiniz. Bir grup ilkesi oluşturmak ve bu sanal makinelerde telemetri ayarlarınızı yönetmek için lütfen [https://technet.microsoft.com/library/mt577208(v=vs.85).aspx\#BKMK\_UTC](https://technet.microsoft.com/library/mt577208%28v=vs.85%29.aspx#BKMK_UTC) makalesine göz atıp **Telemetriye İzin Ver** grup ilkesi için **0** veya **1**'i seçin. Azure Stack etki alanına katılmayan iki sanal makine (bgpvm ve natvm) vardır. Bu sanal makinelerdeki Geri Bildirim ve Tanılama ayarlarını ayrı ayrı değiştirmek için lütfen bkz. [http://windows.microsoft.com/tr-TR/windows-10/feedback-diagnostics-privacy-faq](http://windows.microsoft.com/en-us/windows-10/feedback-diagnostics-privacy-faq).

## Sonraki adımlar

[Azure Stack'e Bağlanma](azure-stack-connect-azure-stack.md)



<!--HONumber=Sep16_HO3-->


