---
title: "Azure yığın Geliştirme Seti dağıtma | Microsoft Docs"
description: "Azure yığın Geliştirme Seti hazırlamak ve bunu dağıtmak için PowerShell betiğini çalıştırmak öğrenin."
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 0ad02941-ed14-4888-8695-b82ad6e43c66
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 7/17/2017
ms.author: erikje
ms.openlocfilehash: b67cabf0ecdb48f137bfcfbce95eee568a1c298d
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="deploy-the-azure-stack-development-kit"></a>Azure yığın Geliştirme Seti dağıtma

*Uygulandığı öğe: Azure yığın Geliştirme Seti*

Dağıtmak için [Azure yığın Geliştirme Seti](azure-stack-poc.md), aşağıdaki adımları tamamlamanız gerekir:

1. [Dağıtım paketini karşıdan](https://azure.microsoft.com/overview/azure-stack/try/?v=try) Cloudbuilder.vhdx alınamıyor.
2. [Cloudbuilder.vhdx hazırlama](#prepare-the-development-kit-host) üzerinde Geliştirme Seti yüklemek istediğiniz bilgisayarı (Geliştirme Seti ana bilgisayarı) yapılandırmak için asdk installer.ps1 komut dosyası çalıştırarak. Bu adımdan sonra Geliştirme Seti konak Cloudbuilder.vhdx önyükleme yapmaz.
3. [Geliştirme Seti dağıtmak](#deploy-the-development-kit) Geliştirme Seti konaktaki.

> [!NOTE]
> Bağlantısı kesilmiş bir Azure yığın ortam kullanmak istiyorsanız bile, en iyi sonuçlar için internet'e bağlı değilken dağıtmak en iyisidir. Bu şekilde, Windows Server 2016 Değerlendirme sürümü, dağıtım sırasında etkinleştirilebilir.
> 
> 

## <a name="download-and-extract-the-development-kit"></a>İndirmeyi ve ayıklamayı Geliştirme Seti
1. Yükleme başlamadan önce bilgisayarınızın aşağıdaki önkoşulları karşıladığından emin olun:

   * Bilgisayarda en az 60 GB boş disk alanı olmalıdır.
   * [.NET framework 4.6 (veya sonraki bir sürümünü)](https://aka.ms/r6mkiy) yüklü olması gerekir.

2. [Gidilecek Başlarken sayfa](https://azure.microsoft.com/overview/azure-stack/try/?v=try)bilgilerinizi verin ve tıklatın **gönderme**.
3. Altında **yazılımı yüklemek**, tıklatın **Azure yığın Geliştirme Seti**.
4. İndirilen AzureStackDownloader.exe dosyasını çalıştırın.
5. İçinde **Azure yığın Geliştirme Seti yükleyici** penceresinde, 5 1 adımlarını izleyin.
6. Yükleme tamamlandıktan sonra **çalıştırmak** MicrosoftAzureStackPOC.exe başlatmak için.
7. Lisans Sözleşmesi ekranında ve ayıklayıcısı Sihirbazı'nın bilgileri gözden geçirin ve ardından **sonraki**.
8. Gizlilik bildirimi ekran ve ayıklayıcısı Sihirbazı'nın bilgileri gözden geçirin ve ardından **sonraki**.
9. Ayıklanması için dosyaları için hedef seçin **sonraki**.
   * Varsayılan değer: <drive letter>:\<geçerli klasörde > \Microsoft Azure yığını
10. Hedef konumu ekran ve ayıklayıcısı Sihirbazı'nın bilgileri gözden geçirin ve ardından **ayıklamak** CloudBuilder.vhdx (~ 25 GB) ve ThirdPartyLicenses.rtf dosyaları ayıklayın. Bu işlemin tamamlanması biraz zaman alır.

> [!NOTE]
> Dosyaları ayıkla sonra makinedeki alanını kurtarmak için exe ve depo dosyaları silebilirsiniz. Veya bu dosyaları olması durumunda, yeniden dağıtmak gereken şekilde başka bir konuma dosyaları yeniden karşıdan yüklemeniz gerekmez taşıyabilirsiniz.
> 
> 

## <a name="prepare-the-development-kit-host"></a>Geliştirme Seti konak hazırlama
1. Fiziksel olarak Geliştirme Seti ana bilgisayarına bağlanmak veya kullanabilirsiniz (KVM gibi) fiziksel konsol erişimi olduğundan emin olun. 13. adım Geliştirme Seti konak yeniden başlatıldıktan sonra bu tür erişimi olması gerekir.
2. Geliştirme Seti konak karşıladığından emin olun [minimum gereksinimleri](azure-stack-deploy.md). Kullanabileceğiniz [Azure yığını için dağıtım denetleyicisi](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) gereksinimlerinizi onaylamak için.
3. Geliştirme Seti barındırmak için yerel yönetici olarak oturum açın.
4. Kopyalama veya CloudBuilder.vhdx dosyayı C:\ sürücüsü (C:\CloudBuilder.vhdx) kök dizinine taşıyın.
5. Geliştirme Seti yükleyici dosyasını (asdk-installer.ps1) karşıdan yüklemek için aşağıdaki betiği c:\AzureStack_Installer klasörüne Geliştirme Seti konağınız üzerinde çalıştırın.
    ```powershell
    # Variables
    $Uri = 'https://raw.githubusercontent.com/Azure/AzureStack-Tools/master/Deployment/asdk-installer.ps1'
    $LocalPath = 'c:\AzureStack_Installer'

    # Create folder
    New-Item $LocalPath -Type directory

    # Download file
    Invoke-WebRequest $uri -OutFile ($LocalPath + '\' + 'asdk-installer.ps1')
    ```
6. Yükseltilmiş bir PowerShell Konsolu > C:\AzureStack_Installer\asdk-installer.ps1 komut dosyasını çalıştırmak >'ı tıklatın **hazırlama ortamı**.
7. Üzerinde **seçin Cloudbuilder vhdx** sayfasında yükleyici, göz atın ve önceki adımda indirdiğiniz cloudbuilder.vhdx dosyasını seçin.
8. İsteğe bağlı: Denetleyin **sürücüleri ekleme** kutusunu konakta istediğiniz ek sürücülerini içeren klasörü belirtin.
9. Üzerinde **isteğe bağlı ayarlar** sayfasında, Geliştirme Seti ana bilgisayar için yerel yönetici hesabı sağlayın. Bu kimlik bilgileri sağlamazsanız, aşağıdaki yükleme işlemi sırasında konağa KVM erişim gerekir.
10. Ayrıca **isteğe bağlı ayarlar** sayfasında, aşağıdakileri ayarlayın seçeneğiniz vardır:
    - **ComputerName**: Bu seçenek Geliştirme Seti ana bilgisayar adını ayarlar. Ad FQDN gereksinimlere uygun olmalıdır ve 15 karakter veya daha az olmalıdır. Varsayılan Windows tarafından oluşturulan bir rastgele bilgisayar adıdır.
    - **Saat dilimi**: Geliştirme Seti ana bilgisayar için saat dilimini ayarlar. Varsayılan değer (UTC-8:00) Pasifik Saati (ABD ve Kanada).
    - **Statik IP yapılandırması**: dağıtımınızı statik bir IP adresi kullanacak şekilde ayarlar. Aksi takdirde, yükleyici cloudbuilder.vhx yeniden başlatıldığında, ağ arabirimleri DHCP ile yapılandırılır.
11. **İleri**’ye tıklayın.
12. Bir statik IP yapılandırması önceki adımda seçtiğiniz istiyorsanız, artık gerekir:
    - Bir ağ bağdaştırıcısı seçin. Tıklamadan önce bağdaştırıcıya bağlanabilir emin olun **sonraki**.
    - Olduğundan emin olun **IP adresi**, **ağ geçidi**, ve **DNS** değerleri doğru olduğundan ve ardından **sonraki**.
13. Tıklatın **sonraki** hazırlama işlemini başlatmak için.
14. Zaman hazırlık gösterdiği **tamamlandı**, tıklatın **sonraki**.
15. ' I tıklatın **artık yeniden** cloudbuilder.vhdx önyükleme ve dağıtım işlemine devam edin.

## <a name="deploy-the-development-kit"></a>Geliştirme Seti dağıtma
1. Geliştirme Seti barındırmak için yerel yönetici olarak oturum açın. Önceki adımlarda belirtilen kimlik bilgilerini kullanın.

    > [!IMPORTANT]
    > Azure Active Directory dağıtımları için Azure yığın doğrudan ya da saydam bir proxy üzerinden Internet erişimi gerektirir. Dağıtımı tam olarak bir NIC için ağ iletişimi destekler. Birden çok NIC varsa, yalnızca bir etkin (ve diğer tüm kullanıcılar devre dışı bırakılır) bir sonraki bölümde dağıtım betiği çalıştırmadan önce emin olun.
    
2. Yükseltilmiş bir PowerShell Konsolu > (olabilen Cloudbuilder.vhdx farklı bir sürücüde) \AzureStack_Installer\asdk-installer.ps1 komut dosyasını çalıştırmak > tıklatın **yüklemek**.
3. İçinde **türü** kutusunda **Azure bulut** veya **ADFS**.
    - **Azure bulut**: Azure Active Directory kimlik sağlayıcısı değil. AAD hesap genel yönetici izinlerine sahip olduğu belirli bir dizin belirtmek için bu parametreyi kullanın. Bir AAD dizini Kiracı tam adı. Örneğin,. onmicrosoft.com. 
    - **ADFS**: dizin hizmeti varsayılan damga kimlik sağlayıcısı, oturum açmak için varsayılan hesap azurestackadmin@azurestack.local, ve kullanmak için kurulumunun bir parçası sağlanan bir paroladır.
4. Altında **yerel yönetici parolasını**, **parola** kutusuna (hangi geçerli yapılandırılmış yerel yönetici parolası eşleşmelidir) yerel yönetici parolasını yazın ve ardından**Sonraki**.
5. Geliştirme Seti için kullanın ve ardından bir ağ bağdaştırıcısı seçin **sonraki**.
6. DHCP veya BGPNAT01 sanal makine için statik ağ yapılandırması seçin.
    - **DHCP** (varsayılan): sanal makine IP ağ yapılandırması DHCP sunucusundan alır.
    - **Statik**: DHCP Internet'e erişmek için Azure yığın için geçerli bir IP adresi atanamıyor, yalnızca bu seçeneği kullanın. Statik bir IP adresi alt ağ maskesi uzunluğunu (örneğin, 10.0.0.5/24) belirtilmelidir.
7. İsteğe bağlı olarak, aşağıdaki değerleri ayarlayın:
    - **VLAN kimliği**: VLAN kimliğini ayarlar Yalnızca AzS BGPNAT01 ve konak fiziksel ağ (ve Internet) erişmek için VLAN kimliği yapılandırmanız gerekir, bu seçeneği kullanın. 
    - **DNS ileticisi**: bir DNS sunucusu, Azure yığın dağıtımının bir parçası oluşturulur. Damga dışında adları çözümlemek için çözüm içinde izin vermek için mevcut altyapısını DNS sunucunuzun sağlar. Damga DNS sunucusu Bilinmeyen ad çözümleme isteklerinin bu sunucusuna iletir.
    - **Saat sunucusu**: Bu gerekli alan saat sunucusu ayarlar ve bir IP adresi olmalıdır. Bir saat sunucusu IP adresini bulmak için ziyaret [pool.ntp.org](http:\\pool.ntp.org) veya time.windows.com ping işlemi yapın. 
8. **İleri**’ye tıklayın. 
9. Üzerinde **ağ arabirimi kartı özelliklerini doğrulama** sayfasında, bir ilerleme çubuğu görürsünüz. 
    - Bunu diyorsa **bir güncelleştirme karşıdan**, sayfasındaki yönergeleri izleyin.
    - Ne zaman yazacaktır **tamamlandı**, tıklatın **sonraki**.
10. Üzerinde **Özet** sayfasında, **dağıtma**.
11. Bir Azure Active Directory dağıtımı kullanıyorsanız, Azure Active Directory genel yönetici hesabı kimlik bilgilerinizi girmeniz istenir.
12. Dağıtım işlemi sırasında sistem otomatik olarak bir kez yeniden birkaç saat sürebilir.
   
   > [!IMPORTANT]
   > Dağıtımının ilerleme durumunu izlemek isterseniz, azurestack\AzureStackAdmin oturum açın. Makine etki alanına katılan sonra yerel yönetici oturum açarsanız, dağıtımının ilerleme durumunu göremezsiniz. Dağıtımı yeniden çalıştır bulunamadı, bunun yerine azurestack\AzureStackAdmin çalıştığını doğrulamak oturum açın.
   > 
   > 
   
    Dağıtım başarılı olduğunda, PowerShell konsolunda görüntüler: **tamamlandı: eylem 'Dağıtım'**.
   
Dağıtım başarısız olursa aynı yükseltilmiş PowerShell penceresinden aşağıdaki yeniden PowerShell betiğini kullanabilirsiniz:

```powershell
cd c:\CloudDeployment\Setup
.\InstallAzureStackPOC.ps1 -Rerun
```

Bu komut dosyasını başarıyla son adımı dağıtımından yeniden başlatılır.

Veya, [dağıtmanız](azure-stack-redeploy.md) sıfırdan.


## <a name="reset-the-password-expiration-to-180-days"></a>180 gün olarak parola geçerlilik süresi Sıfırla

Geliştirme Seti konak parolasını çok yakında sona değil emin olmak için dağıttıktan sonra aşağıdaki adımları izleyin:

Parola süre sonu ilkesi Powershell'den değiştirmek için:
1. Powershell penceresinden komutunu çalıştırın; Set-ADDefaultDomainPasswordPolicy - MaxPasswordAge 180.00:00:00-kimlik azurestack.local

Parola süre sonu ilkesi el ile değiştirmek için:
1. Geliştirme Seti konakta açmak **Grup İlkesi Yönetimi** gidin **Grup İlkesi Yönetimi** – **orman: azurestack.local** – **etkialanları** – **azurestack.local**.
2. Sağ tıklayın **varsayılan etki alanı ilkesi** tıklatıp **Düzenle**.
3. Grup İlkesi Yönetimi Düzenleyicisi'nde gidin **Bilgisayar Yapılandırması** – **ilkeleri** – **Windows ayarları** – **güvenlik ayarları**– **Hesap ilkeleri** – **parola ilkesi**.
4. Sağ bölmede, çift **en uzun parola geçerlilik süresi**.
5. İçinde **en uzun parola geçerlilik süresi özellikleri** iletişim kutusu, değişiklik **parola içinde dolacak** değeri için 180 ve ardından **Tamam**.


## <a name="next-steps"></a>Sonraki adımlar

[PowerShell yükleme](azure-stack-powershell-configure-quickstart.md)

[Azure yığını, Azure aboneliğiniz ile kaydetme](azure-stack-register.md)

[Azure Stack’e bağlanma](azure-stack-connect-azure-stack.md)

