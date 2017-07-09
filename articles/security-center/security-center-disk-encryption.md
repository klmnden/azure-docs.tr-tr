---
title: "Bir Azure Sanal Makinesi&quot;ni şifreleme | Microsoft Docs"
description: "Bu belge, Azure Güvenlik Merkezi&quot;nden uyarı aldıktan sonra Azure Sanal Makine&quot;yi şifrelemenize yardımcı olur."
services: security, security-center
documentationcenter: na
author: TomShinder
manager: swadhwa
editor: 
ms.assetid: f6c28bc4-1f79-4352-89d0-03659b2fa2f5
ms.service: security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/15/2017
ms.author: tomsh
ms.translationtype: Human Translation
ms.sourcegitcommit: 44eac1ae8676912bc0eb461e7e38569432ad3393
ms.openlocfilehash: d3817d44bb21162afe799fd13368fbf902521416
ms.contentlocale: tr-tr
ms.lasthandoff: 05/17/2017


---
# <a name="encrypt-an-azure-virtual-machine"></a>Azure Sanal Makine'yi şifreleme
Şifrelenmemiş sanal makineleriniz varsa Azure Güvenlik Merkezi sizi uyarır. Bu uyarılar Yüksek Önem Derecesine Sahip olarak gösterilir ve bu sanal makineleri şifrelemeniz önerilir.

![Disk şifreleme önerisi](./media/security-center-disk-encryption/security-center-disk-encryption-fig1.png)

> [!NOTE]
> Bu belgedeki bilgiler bir Anahtar Şifreleme Anahtarı (Azure Backup kullanarak sanal makineleri yedeklemek için gereklidir) kullanmadan sanal makineleri şifreleme işlemiyle ilgilidir. Şifrelenmiş Azure Sanal Makineler için Azure Backup’ı desteklemek üzere Anahtar Şifreleme Anahtarı’nı kullanma hakkında bilgi için lütfen [Windows ve Linux Azure Sanal Makineleri için Azure Disk Şifrelemesi](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption) makalesine bakın.
>
>

Azure Güvenlik Merkezi tarafından şifreleme gerektiği belirlenen Azure Virtual Machines'i şifrelemek için aşağıdaki adımları öneririz:

* Azure PowerShell'i yükleyip yapılandırın. Böylece Azure Virtual Machines şifreleme için gereken önkoşulları ayarlamaya yönelik gerekli PowerShell komutlarını çalıştırmanıza olanak sağlanır.
* Azure Disk Şifrelemesi Önkoşulları Azure PowerShell betiğini elde edip çalıştırın
* Sanal makinelerinizi şifreleyin

Bu belgenin amacı, Azure PowerShell ile ilgili çok az arka plana sahip olsanız veya hiç arka plana sahip olmasanız bile sanal makinelerinizi şifrelemenizi sağlamaktır.
Bu belge, Azure Disk Şifrelemesi'ni yapılandıracağınız istemci makine olarak Windows 10 kullandığınızı varsayar.

Azure Virtual Machines için önkoşulları ayarlamak ve şifrelemeyi yapılandırmak üzere birçok yaklaşım vardır. Azure PowerShell veya Azure CLI konusunda zaten bilgiliyseniz alternatif yaklaşımlar kullanmayı tercih edebilirsiniz.

> [!NOTE]
> Azure Virtual Machines için şifreleme yapılandırmaya yönelik alternatif yaklaşımlar hakkında daha fazla bilgi edinmek için bkz. [Windows ve Linux Azure Virtual Machines için Azure Disk Şifrelemesi](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0).
>
>

## <a name="install-and-configure-azure-powershell"></a>Azure PowerShell'i yükleyip yapılandırma
Bilgisayarınızda Azure PowerShell 1.2.1 sürümü veya üstünün yüklü olması gerekir. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview) makalesi, bilgisayarınızın Azure PowerShell ile çalışmasını sağlamak için ihtiyacınız olan tüm adımları içerir. En kolay yaklaşım, bu makalede değinilen Web PI kurulumu yaklaşımını kullanmaktır. Azure PowerShell önceden yüklü olsa bile, Azure PowerShell'in en son sürümüne sahip olmak için Web PI yaklaşımını kullanarak yeniden yükleyin.

## <a name="obtain-and-run-the-azure-disk-encryption-prerequisites-configuration-script"></a>Azure disk şifrelemesi önkoşulları yapılandırma betiğini elde edip çalıştırma
Azure Disk Şifrelemesi Önkoşulları Yapılandırma Betiği, Azure Virtual Machines'inizi şifrelemek için gereken tüm önkoşulları ayarlar.

1. [Azure Disk Şifrelemesi Önkoşulları Kurulum Betiği](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1)'ne sahip olan GitHub sayfasına gidin.
2. GibHub sayfasında **Ham** düğmesine tıklayın.
3. Sayfadaki tüm metni seçmek için **CTRL-A**'yı kullanın ve ardından sayfadaki tüm metni panoya kopyalamak için **CTRL-C**'yi kullanın.
4. **Not Defteri**'ni açın ve kopyalanan metni Not Defteri'ne yapıştırın.
5. C: sürücünüzde **AzureADEScript** adlı yeni bir klasör oluşturun.
6. Not Defteri dosyasını kaydedin; **Dosya**'ya tıklayın, ardından **Farklı Kaydet**'e tıklayın. Dosya adı metin kutusunda **"ADEPrereqScript.ps1"** girin ve **Kaydet**'e tıklayın. (adın etrafına tırnak işaretleri koyduğunuzdan emin olun; yoksa dosya .txt dosya uzantısıyla kaydedilir).

Artık betik içeriği kaydedildiğine göre, PowerShell ISE'deki betiği açın:

1. Başlat Menüsünde **Cortana**'ya tıklayın. Cortana arama metni kutusuna **PowerShell** yazarak **Cortana** "PowerShell" sorusunu sorun.
2. **Windows PowerShell ISE**'ye sağ tıklayın ve **Yönetici olarak çalıştır**'a tıklayın.
3. **Yönetici: Windows PowerShell ISE** penceresinde **Görünüm**'e tıklayın ve ardından **Betik Bölmesini Göster**'e tıklayın.
4. Pencerenin sağ tarafında **Komutlar** bölmesini görürseniz kapatmak için bölmenin sağ üst köşesindeki **"x"** simgesine tıklayın. Metin göremeyeceğiniz kadar küçükse **CTRL+Ekle**'yi ("Ekle", "+" işaretidir) kullanın. Metin çok büyükse **CTRL+Çıkar** (Çıkar "-" işaretidir).
5. **Dosya**'ya tıklayın ve ardından **Aç**'a tıklayın. **C:\AzureADEScript** klasörüne gidin ve **ADEPrereqScript**'e çift tıklayın.
6. **ADEPrereqScript** içeriği PowerShell ISE'de artık görünür olmalıdır ve komutlar, parametreler ve değişkenler gibi çeşitli bileşenleri daha kolayca görmenize yardımcı olmak amacıyla renk kodludur.

Şimdi aşağıdaki gibi bir şekil görüyor olmanız gerekir.

![PowerShell ISE penceresi](./media/security-center-disk-encryption/security-center-disk-encryption-fig2.png)

Üst bölme "betik bölmesi" olarak adlandırılır ve alt bölme de "konsol" olarak adlandırılır. Daha sonra bu makalede bu terimleri kullanacağız.

## <a name="run-the-azure-disk-encryption-prerequisites-powershell-command"></a>Azure disk şifrelemesi önkoşulları PowerShell komutunu çalıştırma
Azure Disk Şifrelemesi Önkoşulları betiği, başlatıldıktan sonra sizden aşağıdaki bilgileri ister:

* **Kaynak Grubu Adı** - Anahtar Kasasını yerleştirmek istediğiniz Kaynak Grubunun adı.  Bu ada sahip önceden oluşturulmuş bir kaynak grubu yoksa girdiğiniz ad ile yeni bir Kaynak Grubu oluşturulur. Bu abonelikte kullanmak istediğiniz bir Kaynak Grubu zaten varsa bu Kaynak Grubunun adını girin.
* **Anahtar Kasasının Adı** - Şifreleme anahtarlarının yerleştirileceği Anahtar Kasasının adı. Bu ada sahip bir Anahtar Kasanız zaten yoksa bu ad ile yeni bir Anahtar Kasası oluşturulur. Kullanmak istediğiniz bir Anahtar Kasası zaten varsa var olan Anahtar Kasasının adını girin.
* **Konum** - Anahtar Kasasının Konumu. Şifrelenecek Anahtar Kasası ve VM'lerin aynı konumda olduğundan emin olun. Konumu bilmiyorsanız bunun nasıl öğrenileceğine dair adımlar bu makalenin devamında gösterilir.
* **Azure Active Directory Uygulamasının Adı** - Anahtar Kasasına gizli anahtarları yazmak için kullanılacak Azure Active Directory uygulamasının adı. Bu ada sahip bir uygulama yoksa yeni bir uygulama oluşturulur. Kullanmak istediğiniz bir Azure Active Directory uygulaması zaten varsa bu Azure Active Directory uygulamasının adını girin.

> [!NOTE]
> Neden bir Azure Active Directory uygulaması oluşturmanız gerektiğini merak ediyorsanız lütfen [Azure Anahtar Kasası ile Çalışmaya Başlama](../key-vault/key-vault-get-started.md) makalesinin *Bir uygulamayı Azure Active Directory'ye kaydetme* bölümüne bakın.
>
>

Bir Azure Sanal Makinesini şifrelemek için aşağıdaki adımları gerçekleştirin:

1. PowerShell ISE'yi kapattıysanız yükseltilmiş bir PowerShell ISE örneği açın. PowerShell ISE zaten açık değilse bu makalede önceden geçen yönergeleri izleyin. Betiği kapattıysanız **Dosya**'ya ve ardından **Aç**'a tıklayarak ve **c:\AzureADEScript** klasöründen betiği seçerek **ADEPrereqScript.ps1**'i açın. Makaleyi baştan beri izlediyseniz sonraki adıma geçmeniz yeterlidir.
2. PowerShell ISE konsolunda (PowerShell ISE'nin en alt bölmesi) **cd c:\AzureADEScript** yazıp **ENTER**'a basarak betiğin odağını yerel olarak değiştirin.
3. Makinenizdeki yürütme ilkesini, betiği çalıştırabilecek şekilde ayarlayın. Konsolda **Set-ExecutionPolicy Unrestricted** yazın ve ardından ENTER'a basın. Yürütme ilkesindeki değişikliğin etkilerini anlatan bir iletişim kutusu görürseniz **Tümüne evet** veya **Evet**'e tıklayın (**Tümüne evet**'i görürseniz bu seçeneği belirleyin; **Tümüne evet**'i görmezseniz **Evet**'e tıklayın).
4. Azure hesabınızda oturum açın. Konsolda **Login-AzureRmAccount** yazın ve **ENTER**'a basın. Kimlik bilgilerinizi gireceğiniz bir iletişim kutusu görünür (sanal makineleri değiştirme haklarına sahip olduğunuzdan emin olun; bu haklara sahip değilseniz şifreleme yapamazsınız. Emin değilseniz aboneliğinizin sahibine veya yöneticinize sorun). **Environment**, **Account**, **TenantId**, **SubscriptionId** ve **CurrentStorageAccount**'ınız hakkında bilgiler görmeniz gerekir. **SubscriptionId**'yi Not Defteri'ne kopyalayın. Bunu 6. adımda kullanmanız gerekir.
5. Sanal makinenizin hangi aboneliğe ait olduğunu ve konumunu bulun. [https://portal.azure.com](ttps://portal.azure.com)'a gidin ve oturum açın.  Sayfanın sol tarafında **Virtual Machines**'e tıklayın. Sanal makinelerinizi ve ait oldukları aboneliklerin listesini görürsünüz.

   ![Virtual Machines](./media/security-center-disk-encryption/security-center-disk-encryption-fig3.png)
6. PowerShell ISE'ye dönün. Betiğin çalıştırılacağı abonelik bağlamını ayarlayın. Konsolda **Select-AzureRmSubscription –SubscriptionId <your_subscription_Id>** yazın (**< your_subscription_Id >** öğesini gerçek Abonelik Kimliğinizle değiştirin) ve **ENTER**'a basın. Environment, **Account**, **TenantId**, **SubscriptionId** ve **CurrentStorageAccount** hakkında bilgiler görürsünüz.
7. Şimdi betiği çalıştırmaya hazırsınız. **Betiği Çalıştır** düğmesine tıklayın veya klavyede **F5**'e basın.

   ![PowerShell Betiğini Yürütme](./media/security-center-disk-encryption/security-center-disk-encryption-fig4.png)
8. Betik **resourceGroupName:** öğesini ister; kullanmak istediğiniz *Kaynak Grubu*'nun adını girin, ardından **ENTER**'a basın. Ad yoksa yenisi için kullanmak istediğiniz adı girin. Kullanmak istediğiniz bir *Kaynak Grubu* zaten varsa (sanal makinenizin içinde olduğu grup gibi) var olan Kaynak Grubu'nun adını girin.
9. Betik **keyVaultName:** öğesini ister; kullanmak istediğiniz *Anahtar Kasası*'nın adını girin, ardından ENTER'a basın. Ad yoksa yenisi için kullanmak istediğiniz adı girin. Kullanmak istediğiniz bir Anahtar Kasası zaten varsa var olan *Anahtar Kasası*'nın adını girin.
10. Betik **konum** ister; şifrelemek istediğiniz VM'nin bulunduğu konumun adını girin, ardından **ENTER**'a basın. Konumu hatırlamıyorsanız 5. adıma geri dönün.
11. Betik **aadAppName:** ister; kullanmak istediğiniz *Azure Active Directory* uygulamasının adını girin, ardından **ENTER**'a basın. Ad yoksa yenisi için kullanmak istediğiniz adı girin. Kullanmak istediğiniz bir *Azure Active Directory uygulaması* zaten varsa var olan *Azure Active Directory uygulamasının* adını girin.
12. İletişim kutusunda bir günlük görüntülenir. Kimlik bilgilerinizi sağlayın (evet, bir kere oturum açtınız ancak şimdi yeniden yapmanız gerekiyor).
13. Betik çalışır ve tamamlandığında **aadClientID**, **aadClientSecret**, **diskEncryptionKeyVaultUrl** ve **keyVaultResourceId** değerlerini kopyalamayı sorar. Bu değerlerin her birini panoya kopyalayın ve Not Defteri'ne yapıştırın.
14. PowerShell ISE'ye dönün ve imleci son satırın sonuna yerleştirip **ENTER**'a basın.

Betiğin çıkışı aşağıdaki ekrana benzer şekilde görünmelidir:

![PowerShell çıkışı](./media/security-center-disk-encryption/security-center-disk-encryption-fig5.png)

## <a name="encrypt-the-azure-virtual-machine"></a>Azure sanal makineyi şifreleme
Şimdi sanal makinenizi şifrelemeye hazırsınız. Sanal makineniz, Anahtar Kasanız ile aynı Kaynak Grubunda bulunuyorsa şifreleme adımları bölümüne geçebilirsiniz. Ancak sanal makineniz Anahtar Kasanız ile aynı Kaynak Grubu'nda değilse PowerShell ISE konsolunda aşağıdakini girmeniz gerekir:

**$resourceGroupName = <’Virtual_Machine_RG’>**

**< Virtual_Machine_RG >** öğesini, sanal makinelerinizin içinde bulunduğu Kaynak Grubunun adıyla tek bir tırnakla çevrelenmiş şekilde değiştirin. Ardından **ENTER**'a basın.
Doğru Kaynak Grubu adının girildiğini onaylamak için PowerShell ISE konsolunda aşağıdakini yazın:

**$resourceGroupName**

**ENTER**'a basın. Sanal makinelerinizin içinde bulunduğu Kaynak Grubunun adını görmeniz gerekir. Örneğin:

![PowerShell çıkışı](./media/security-center-disk-encryption/security-center-disk-encryption-fig6.png)

### <a name="encryption-steps"></a>Şifreleme adımları
Öncelikle, şifrelemek istediğiniz sanal makinenin adını PowerShell'e bildirmeniz gerekir. Konsolda şunu yazın:

**$vmName = <’your_vm_name’>**

**<’your_vm_name’>** öğesini, VM'nizin adıyla değiştirin (adın tek bir tırnakla çevrelenmiş olduğundan emin olun) ve ardından **ENTER**'a basın.

Doğru VM adının girildiğini onaylamak için şunu yazın:

**$vmName**

**ENTER**'a basın. Şifrelemek istediğiniz sanal makinenin adını görmeniz gerekir. Örnek:

![PowerShell çıkışı](./media/security-center-disk-encryption/security-center-disk-encryption-fig7.png)

Sanal makinedeki tüm sürücüleri şifrelemek için şifreleme komutunu çalıştırabileceğiniz iki yöntem vardır. İlk yöntem, PowerShell ISE konsolunda aşağıdaki komutu yazmaktır:

~~~
Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $resourceGroupName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $keyVaultResourceId -VolumeType All
~~~

Bu komutu yazdıktan sonra **ENTER**'a basın.

İkinci yöntem, betik bölmesine tıklamak (PowerShell ISE'nin en üst bölmesi) ve sayfayı betiğin en altına doğru aşağı kaydırmaktır. Yukarıda listelenen komutu vurgulayın, ardından buna sağ tıklayın ve sonra **Seçimi Çalıştır**'a tıklayın veya klavyede **F8**'e basın.

![PowerShell ISE](./media/security-center-disk-encryption/security-center-disk-encryption-fig8.png)

Kullandığınız yöntemden bağımsız olarak, işlemin tamamlanmasının 10-15 dakika süreceğini bildiren bir iletişim kutusu görünür. **Evet**'e tıklayın.

Şifreleme işlemi gerçekleşirken, Azure Portal'a dönebilir ve sanal makinenin durumunu görebilirsiniz. Sayfanın sol tarafında **Virtual Machines**'e tıklayın, ardından **Virtual Machines** dikey penceresinde şifrelediğiniz sanal makinenin adına tıklayın. Açılan dikey pencerede **Durum**'un **Güncelleştiriliyor** olduğunu fark edeceksiniz. Bu, şifreleme işleminin gerçekleştiğini gösterir.

![VM hakkında daha fazla ayrıntı](./media/security-center-disk-encryption/security-center-disk-encryption-fig9.png)

PowerShell ISE'ye dönün. Betik tamamlandığında aşağıdaki şekilde görünenleri görürsünüz.

![PowerShell çıkışı](./media/security-center-disk-encryption/security-center-disk-encryption-fig10.png)

Sanal makinenin artık şifrelenmiş olduğunu göstermek için Azure Portal'a dönün ve sayfanın sol tarafında **Virtual Machines**'e tıklayın. Şifrelediğiniz sanal makinenin adına tıklayın. **Ayarlar** dikey penceresinde **Diskler**'e tıklayın.

![Ayarlar seçenekleri](./media/security-center-disk-encryption/security-center-disk-encryption-fig11.png)

**Diskler** dikey penceresinde **Şifreleme**'nin **Etkin** olduğunu görürsünüz.

![Diskler özellikleri](./media/security-center-disk-encryption/security-center-disk-encryption-fig12.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede bir Azure Sanal Makine'nin nasıl şifreleneceğini öğrendiniz. Azure Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu izleme hakkında bilgi edinin
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) - Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanma hakkında sık sorulan soruları bulun
* [Azure Güvenlik Blogu](http://blogs.msdn.com/b/azuresecurity/) - Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun

