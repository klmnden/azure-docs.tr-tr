---
title: Azure Stack VPN kullanarak Azure'a bağlanma
description: Azure stack'teki sanal ağlara VPN kullanarak azure'daki sanal ağlara bağlanma.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: sethm
ms.reviewer: scottnap
ms.lastreviewed: 10/24/2018
ms.openlocfilehash: d35ab3f477f327cb85cd8dfebd255542489debdc
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58286151"
---
# <a name="connect-azure-stack-to-azure-using-vpn"></a>Azure Stack VPN kullanarak Azure'a bağlanma

*Uygulama hedefi: Azure Stack tümleşik sistemleri*

Bu makalede azure'da bir sanal ağa bir sanal ağda Azure Stack bağlanmak için siteden siteye VPN oluşturma işlemini gösterir.

## <a name="before-you-begin"></a>Başlamadan önce

Bağlantı yapılandırmasını tamamlamak için başlamadan önce aşağıdaki öğelerin bulunduğundan emin olun:

* Doğrudan internet'e bağlı systems (çok düğümlü) dağıtımı bir Azure Stack tümleşik. Dış ortak IP adresi aralığınız genel internet'ten doğrudan erişilebilir olmalıdır.
* Geçerli bir Azure aboneliği. Azure aboneliğiniz yoksa, oluşturabileceğiniz bir [ücretsiz Azure hesabı](https://azure.microsoft.com/free/?b=17.06).

### <a name="vpn-connection-diagram"></a>VPN bağlantı diyagramı

İşiniz bittiğinde bağlantı yapılandırması gibi görünmelidir aşağıdaki şekilde gösterilmiştir:

![Siteden siteye VPN bağlantısı yapılandırma](media/azure-stack-connect-vpn/image2.png)

### <a name="network-configuration-example-values"></a>Ağ yapılandırma örnek değerler

Ağ Yapılandırma örnekleri tablo, bu makaledeki örnekler için kullanılan değerleri gösterir. Bu değerleri kullanabilirsiniz veya bu makaledeki örnekleri daha iyi anlamak için bunlara bakabilirsiniz:

|   |Azure Stack|Azure|
|---------|---------|---------|
|Sanal ağ adı     |Azs-VNet|AzureVNet |
|Sanal ağ adres alanı |10.1.0.0/16|10.100.0.0/16|
|Alt ağ adı     |FrontEnd|FrontEnd|
|Alt ağ adres aralığı|10.1.0.0/24 |10.100.0.0/24 |
|Ağ geçidi alt ağı     |10.1.1.0/24|10.100.1.0/24|

## <a name="create-the-network-resources-in-azure"></a>Azure'da ağ kaynakları oluşturma

İlk olarak Azure için ağ kaynaklarını oluşturun. Aşağıdaki yönergeler kullanarak kaynak oluşturma işlemini göstermektedir [Azure portalında](https://portal.azure.com/).

### <a name="create-the-virtual-network-and-virtual-machine-vm-subnet"></a>Sanal ağ ve sanal makine (VM) alt ağı oluşturma

1. Oturum [Azure portalında](https://portal.azure.com/) Azure hesabınızı kullanarak.
2. Kullanıcı Portalı'nda seçin **+ kaynak Oluştur**.
3. Git **Market**ve ardından **ağ**.
4. **Sanal ağ**'ı seçin.
5. Azure için değerleri belirlemek için ağ yapılandırma tabloda yer alan bilgileri kullanın. **adı**, **adres alanı**, **alt ağ adı**, ve **alt ağ adresi Aralık**.
6. İçin **kaynak grubu**, yeni bir kaynak grubu oluşturun veya zaten bir hesabınız varsa seçin **var olanı kullan**.
7. Seçin **konumu** ağınızın.  Örnek değerleri kullanıyorsanız seçin **Doğu ABD** veya başka bir konum kullanın.
8. **Panoya sabitle**’yi seçin.
9. **Oluştur**’u seçin.

### <a name="create-the-gateway-subnet"></a>Ağ geçidi alt ağı oluşturma

1. Oluşturduğunuz sanal ağ kaynağı açın (**AzureVNet**) panodan.
2. Üzerinde **ayarları** bölümünden **alt ağlar**.
3. Seçin **ağ geçidi alt ağı** sanal ağa bir ağ geçidi alt ağı eklemek için.
4. Alt ağın adı varsayılan olarak **GatewaySubnet** şeklinde ayarlanır.

   >[!IMPORTANT]
   >Ağ geçidi alt ağları özeldir ve düzgün şekilde çalışabilmesi için bu ada sahip olmalıdır.

5. İçinde **adres aralığı** adresi olduğunu doğrulayın, alan **10.100.1.0/24**.
6. Seçin **Tamam** ağ geçidi alt ağı oluşturmak için.

### <a name="create-the-virtual-network-gateway"></a>Sanal ağ geçidini oluşturma

1. Azure portalında **+ kaynak Oluştur**.  
2. Git **Market**ve ardından **ağ**.
3. Ağ kaynakları listesinden **sanal ağ geçidi**.
4. İçinde **adı**, türü **Azure-GW**.
5. Bir sanal ağ seçmek için Seç **sanal ağ**. Ardından **AzureVnet** listeden.
6. **Genel IP adresi**'ni seçin. Zaman **genel IP adresi seçin** bölümü açılır, select **Yeni Oluştur**.
7. İçinde **adı**, türü **Azure GW PiP**ve ardından **Tamam**.
8. **Abonelik** ve **Konum** seçeneklerinin doğruluğunu onaylayın. Kaynağı panoya sabitleyebilirsiniz. **Oluştur**’u seçin.

### <a name="create-the-local-network-gateway-resource"></a>Yerel ağ geçidi kaynağı oluşturma

1. Azure portalında **+ kaynak Oluştur**.
2. Git **Market**ve ardından **ağ**.
3. Kaynak listesinden **yerel ağ geçidi**.
4. İçinde **adı**, türü **Azs-GW**.
5. İçinde **IP adresi**, Azure Stack sanal ağ ağ yapılandırma tabloda daha önce listelenen geçidinizin genel IP adresini yazın.
6. İçinde **adres alanı**, Azure yığını, yazın **10.1.0.0/24** ve **10.1.1.0/24** adres alanı için **AzureVNet**.
7. Doğrulayın, **abonelik**, **kaynak grubu**, ve **konumu** doğru olduğundan ve ardından **Oluştur**.

## <a name="create-the-connection"></a>Bağlantı oluşturma

1. Kullanıcı Portalı'nda seçin **+ kaynak Oluştur**.
2. Git **Market**ve ardından **ağ**.
3. Kaynak listesinden **bağlantı**.
4. Üzerinde **temel** ayarları bölümü için **bağlantı türü**, seçin **siteden siteye (IPSec)**.
5. Seçin **abonelik**, **kaynak grubu**, ve **konumu**ve ardından **Tamam**.
6. Üzerinde **ayarları** bölümünden **sanal ağ geçidi**ve ardından **Azure-GW**.
7. Seçin **yerel ağ geçidi**ve ardından **Azs-GW**.
8. İçinde **bağlantı adı**, türü **Azure Azs**.
9. İçinde **paylaşılan anahtar (PSK)**, türü **12345**, ardından **Tamam**.

   >[!NOTE]
   >Paylaşılan anahtar için farklı bir değer kullanıyorsanız, bu bağlantının diğer ucundaki oluşturduğunuz paylaşılan anahtar değeri eşleşmesi gerektiğini unutmayın.

10. Gözden geçirme **özeti** bölümüne ve ardından **Tamam**.

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Artık Azure'da bir sanal makine oluşturma ve sanal ağınızdaki VM alt yerleştirin.

1. Azure portalında **+ kaynak Oluştur**.
2. Git **Market**ve ardından **işlem**.
3. Sanal makine görüntüleri listesinde seçin **Windows Server 2016 Datacenter değerlendirme** görüntü.
4. Üzerinde **Temelleri** bölüm için **adı**, türü **AzureVM**.
5. Geçerli kullanıcı adı ve parola yazın. Oluşturulduktan sonra sanal makineye oturum açmak için bu hesabı kullanın.
6. Sağlayan bir **abonelik**, **kaynak grubu**, ve **konumu**ve ardından **Tamam**.
7. Üzerinde **boyutu** bölümünde bu örneği için bir sanal makine boyutu seçin ve ardından **seçin**.
8. İçinde **ayarları** bölümünde varsayılan ayarları kullanabilirsiniz. Seçtiğiniz önce **Tamam**, onaylayın:

   * **AzureVnet** sanal ağın seçili.
   * Alt kümesine **10.100.0.0/24**.

   **Tamam**’ı seçin.

9. Ayarları gözden **özeti** bölümüne ve ardından **Tamam**.

## <a name="create-the-network-resources-in-azure-stack"></a>Azure Stack'te ağ kaynakları oluşturma

Ardından, ağ kaynakları Azure Stack'te oluşturun.

### <a name="sign-in-as-a-user"></a>Bir kullanıcı olarak oturum açın

Hizmet Yöneticisi kullanıcı planları, teklifleri ve kendi kullanıcıların kullanabileceği abonelikleri test etmek için oturum açabilir. Zaten yoksa, [bir kullanıcı hesabı oluşturma](../azure-stack-add-new-user-aad.md) oturum açmadan önce.

### <a name="create-the-virtual-network-and-a-vm-subnet"></a>Sanal ağ ve VM alt ağı oluşturma

1. Kullanıcı portalında oturum açmak için bir kullanıcı hesabı kullanın.
2. Kullanıcı Portalı'nda seçin **+ kaynak Oluştur**.

    ![Yeni sanal ağ oluştur](media/azure-stack-connect-vpn/image3.png)

3. Git **Market**ve ardından **ağ**.
4. **Sanal ağ**'ı seçin.
5. İçin **adı**, **adres alanı**, **alt ağ adı**, ve **alt ağ adres aralığı**, ağ yapılandırma tablodaki değerleri kullanın.
6. İçinde **abonelik**, daha önce oluşturduğunuz aboneliği görüntülenir.
7. İçin **kaynak grubu**, bir kaynak grubu oluşturabilir veya zaten bir varsa seçin **var olanı kullan**.
8. Varsayılan konumu doğrulayın.
9. **Panoya sabitle**’yi seçin.
10. **Oluştur**’u seçin.

### <a name="create-the-gateway-subnet"></a>Ağ geçidi alt ağını oluşturma

1. Panoda, oluşturduğunuz Azs-VNet sanal ağ kaynağını açın.
2. Üzerinde **ayarları** bölümünden **alt ağlar**.
3. Sanal ağa bir ağ geçidi alt ağı eklemek için seçin **ağ geçidi alt ağı**.

    ![Ağ geçidi alt ağı ekleme](media/azure-stack-connect-vpn/image4.png)

4. Varsayılan olarak, alt ağ adı kümesine **GatewaySubnet**. Ağ geçidi alt düzgün çalışması kullandıkları gerekir **GatewaySubnet** adı.
5. İçinde **adres aralığı**, adresi olduğunu doğrulayın **10.1.1.0/24**.
6. Seçin **Tamam** ağ geçidi alt ağı oluşturmak için.

### <a name="create-the-virtual-network-gateway"></a>Sanal ağ geçidini oluşturma

1. Azure Stack portalında **+ kaynak Oluştur**.
2. Git **Market**ve ardından **ağ**.
3. Ağ kaynakları listesinden **sanal ağ geçidi**.
4. İçinde **adı**, türü **Azs-GW**.
5. Seçin **sanal ağ** öğesini bir sanal ağ seçin. Seçin **Azs-VNet** listeden.
6. Seçin **genel IP adresi** menü öğesi. Zaman **genel IP adresi seçin** bölümü açılır, select **Yeni Oluştur**.
7. İçinde **adı**, türü **Azs-GW-PiP**ve ardından **Tamam**.
8. Varsayılan olarak, **rota tabanlı** seçildiğini **VPN türü**. Tutun **rota tabanlı** VPN türü.
9. **Abonelik** ve **Konum** seçeneklerinin doğruluğunu onaylayın. Kaynağı panoya sabitleyebilirsiniz. **Oluştur**’u seçin.

### <a name="create-the-local-network-gateway"></a>Yerel ağ geçidini oluşturma

Kavramı bir *yerel ağ geçidi* Azure Stack'te Azure dağıtımında biraz farklıdır.

Bir Azure dağıtımında yerel ağ geçidi, azure'daki bir sanal ağ geçidine bağlanmak bir şirket içi (kullanıcı konumda) fiziksel cihazı temsil eder. Ancak, Azure Stack'te her iki ucunda da bağlantı sanal ağ geçitleri uygulanır.

Daha genel bir açıklama, yerel ağ geçidi kaynağı her zaman bağlantının diğer ucundaki uzak ağ geçidini gösterir bağlıdır.

### <a name="create-the-local-network-gateway-resource"></a>Yerel ağ geçidi kaynağı oluşturma

1. Azure Stack portalında oturum açın.
2. Kullanıcı Portalı'nda seçin **+ kaynak Oluştur**.
3. Git **Market**ve ardından **ağ**.
4. Kaynak listesinden **yerel ağ geçidi**.
5. İçinde **adı**, türü **Azure-GW**.
6. İçinde **IP adresi**, Azure'da sanal ağ geçidi için genel IP adresini yazın **Azure GW PiP**. Bu adres, daha önce ağ yapılandırma tablosunda görüntülenir.
7. İçinde **adres alanı**, oluşturduğunuz Azure sanal ağı için kullanılan adres alanını yazın **10.100.0.0/24** ve **10.100.1.0/24**.
8. Doğrulayın, **abonelik**, **kaynak grubu**, ve **konumu** değerlerin doğru olduğundan ve ardından **Oluştur**.

### <a name="create-the-connection"></a>Bağlantı oluşturma

1. Kullanıcı Portalı'nda seçin **+ kaynak Oluştur**.
2. Git **Market**ve ardından **ağ**.
3. Kaynak listesinden **bağlantı**.
4. Üzerinde **Temelleri** ayarları bölümü için **bağlantı türü**seçin **siteden siteye (IPSec)**.
5. Seçin **abonelik**, **kaynak grubu**, ve **konumu**ve ardından **Tamam**.
6. Üzerinde **ayarları** bölümünden **sanal ağ geçidi**ve ardından **Azs-GW**.
7. Seçin **yerel ağ geçidi**ve ardından **Azure-GW**.
8. İçinde **bağlantı adı**, türü **Azs-Azure**.
9. İçinde **paylaşılan anahtar (PSK)**, türü **12345**ve ardından **Tamam**.
10. Üzerinde **özeti** bölümünden **Tamam**.

### <a name="create-a-virtual-machine-vm"></a>Sanal makine (VM) oluşturma

VPN bağlantısını denetlemek için iki VM oluşturun: biri azure'da ve Azure Stack'te biri. Bu sanal makineler oluşturduktan sonra VPN tüneli aracılığıyla veri göndermek ve almak için kullanabilirsiniz.

1. Azure portalında **+ kaynak Oluştur**.
2. Git **Market**ve ardından **işlem**.
3. Sanal makine görüntüleri listesinde seçin **Windows Server 2016 Datacenter değerlendirme** görüntü.
4. Üzerinde **Temelleri** bölümünde **adı**, türü **Azs-VM**.
5. Geçerli kullanıcı adı ve parola yazın. Oluşturulduktan sonra VM'de oturum açmak için bu hesabı kullanın.
6. Sağlayan bir **abonelik**, **kaynak grubu**, ve **konumu**ve ardından **Tamam**.
7. Üzerinde **boyutu** bölümünde, bu örnek için seçim bir sanal makine boyutu ve ardından **seçin**.
8. Üzerinde **ayarları** bölümünde, Varsayılanları kabul edin. Emin olun **Azs-VNet** sanal ağın seçili. Alt ağ değerine ayarlandığını doğrulayın **10.1.0.0/24**. Sonra **Tamam**’ı seçin.
9. Üzerinde **özeti** bölümünde ayarları gözden geçirin ve ardından * Tamam **.

## <a name="test-the-connection"></a>Bağlantıyı sınama

Siteden siteye bağlantı kurulduktan sonra her iki yönde akmasını veri alabilirsiniz doğrulamanız gerekir. Bağlantıyı test etmek için en kolay yolu, ping testi yaparak şöyledir:

* Azure Stack ve Azure sanal makineler'de ping oluşturduğunuz sanal makinede oturum açın.
* Azure'da oluşturduğunuz sanal makinede oturum açın ve Azure stack'teki sanal makineye ping.

>[!NOTE]
>Siteden siteye bağlantı üzerinden geçen trafik gönderiyorsanız emin olmak için VIP uzak alt ağda bulunan sanal makinenin doğrudan IP (DIP) adresine ping yapın.

### <a name="sign-in-to-the-user-vm-in-azure-stack"></a>Azure Stack'te VM kullanıcı için oturum açın

1. Azure Stack portalında oturum açın.
2. Sol gezinti çubuğunda **sanal makineler**.
3. VM listesinde, bulma **Azs-VM** daha önce oluşturduğunuz ve ardından bu seçeneği belirleyin.
4. Sanal makine için bölümünü seçin **Connect**ve ardından Azs-VM.rdp dosyasını açın.

     ![Bağlanma düğmesi](media/azure-stack-connect-vpn/image17.png)

5. Sanal makine oluştururken yapılandırdığınız hesabıyla oturum açın.
6. Yükseltilmiş bir Windows PowerShell istemi açın.
7. **ipconfig /all** yazın.
8. Çıktıda Bul **IPv4 adresi**ve daha sonra kullanmak için adresini kaydedin. Bu, Azure'dan ping adresidir. Örnek ortamda adres olduğu **10.1.0.4**, ancak ortamınızda farklı olabilir. İçinde içerilmesi gerekir **10.1.0.0/24** daha önce oluşturduğunuz alt ağ.
9. Sanal makine ping komutuna yanıt veren bir güvenlik duvarı kuralı oluşturmak için aşağıdaki PowerShell komutunu çalıştırın:

   ```powershell
   New-NetFirewallRule `
    –DisplayName “Allow ICMPv4-In” `
    –Protocol ICMPv4
   ```

### <a name="sign-in-to-the-tenant-vm-in-azure"></a>Azure'deki Kiracı VM'de oturum açın

1. Azure Portal’da oturum açın.
2. Sol gezinti çubuğunda **sanal makineler**.
3. Sanal makineler listesinden Bul **Azure VM** daha önce oluşturduğunuz ve ardından bu seçeneği belirleyin.
4. Sanal makine için bölümünü seçin **Connect**.
5. Sanal makine oluştururken yapılandırdığınız hesabıyla oturum açın.
6. Yükseltilmiş açın **Windows PowerShell** penceresi.
7. **ipconfig /all** yazın.
8. İçinde denk gelen bir IPv4 adresi görmeniz gerekir **10.100.0.0/24**. Örnek ortamda adres olduğu **10.100.0.4**, ancak adresinizi farklı olabilir.
9. Sanal makine ping komutuna yanıt veren bir güvenlik duvarı kuralı oluşturmak için aşağıdaki PowerShell komutunu çalıştırın:

   ```powershell
   New-NetFirewallRule `
    –DisplayName “Allow ICMPv4-In” `
    –Protocol ICMPv4
   ```

10. Azure'da sanal makineden Azure Stack'te, sanal makine tüneli üzerinden ping atın. Bunu yapmak için Azs-sanal makineden kaydettiğiniz DIP'ye ping gönderin. Örnek ortamda budur **10.1.0.4**, ancak laboratuvarınızda not aldığınız adrese ping gönderdiğinizden emin olun. Aşağıdaki ekran görüntüsü yakalamayı gibi görünen bir sonuç görmeniz gerekir:

    ![PING başarılı](media/azure-stack-connect-vpn/image19b.png)

11. Uzak sanal makineden bir yanıt başarılı bir sınama gösterir. Sanal makine pencereyi kapatabilirsiniz.

Ayrıca daha ayrıntılı veri aktarımı test yapmanız gerekir; Örneğin, farklı kopyalama her iki yönde de dosya boyutu.

### <a name="viewing-data-transfer-statistics-through-the-gateway-connection"></a>Ağ geçidi bağlantısı üzerinden veri aktarımı istatistiklerini görüntüleme

Ne kadar veri, siteden siteye bağlantı geçirir bilmek istiyorsanız, bu bilgi kullanılabilir **bağlantı** bölümü. Bu test ayrıca yeni gönderdiğiniz ping gerçekten VPN bağlantısı üzerinden geçip geçmediğini doğrulamanın başka bir yoludur.

1. Azure stack'teki kullanıcı sanal makineye oturum açmış durumdayken, kullanıcı portalında oturum açmak için kullanıcı hesabınızı kullanın.
2. Git **tüm kaynakları**ve ardından **Azs-Azure** bağlantı. **Bağlantıları** görünür.
3. Üzerinde **bağlantı** bölümü istatistikleri **verilerinde** ve **verileri** görünür. Aşağıdaki ekran görüntüsünde, ek dosya aktarımı için çok sayıda atanmıştır. Sıfır olmayan bazı değerler görürsünüz.

    ![Giren ve çıkan veriler](media/azure-stack-connect-vpn/Connection.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure ve Azure uygulama dağıtma yığını](azure-stack-solution-pipeline.md)
