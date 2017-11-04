---
title: "Azure yığın VPN kullanarak Azure'a bağlanma"
description: "Sanal ağlar Azure yığınında VPN kullanarak azure'daki sanal ağlara bağlanma."
services: azure-stack
documentationcenter: 
author: ScottNapolitan
manager: 
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 9/25/2017
ms.author: victorh
ms.openlocfilehash: c06eb0bb44bdfeab956e9b5051786b5bc631acf5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="connect-azure-stack-to-azure-using-vpn"></a>Azure yığın VPN kullanarak Azure'a bağlanma

*Uygulandığı öğe: Azure yığın tümleşik sistemleri*

Bu makalede Azure sanal ağında Azure yığın sanal bir ağa bağlanmak için siteden siteye VPN oluşturulacağını gösterir.

### <a name="connection-diagram"></a>Bağlantı diyagramı
İşiniz bittiğinde bağlantı yapılandırması gibi görünmelidir Aşağıdaki diyagramda gösterilmiştir:

![Siteden siteye VPN bağlantısı yapılandırma](media/azure-stack-connect-vpn/image2.png)

### <a name="before-you-begin"></a>Başlamadan önce
Bağlantı yapılandırmasını tamamlamak için başlamadan önce aşağıdaki öğelerin bulunduğundan emin olun:

* Bir Azure yığın doğrudan Internet'e bağlı systems (çok düğümlü) dağıtımı tümleşiktir. Bu dış ortak IP adresi aralığınızı genel Internet'ten doğrudan erişilebilir olması gerektiği anlamına gelir.
* Geçerli bir Azure aboneliği.  Bir Azure aboneliğiniz yoksa, oluşturabileceğiniz bir [ücretsiz Azure hesabı burada](https://azure.microsoft.com/free/?b=17.06).

## <a name="network-example-values-table"></a>Ağ örnek değerler tablosu
Ağ örnek değerler tablosu, bu makalede kullanılan örnek değerleri gösterir. Bu makaledeki örneklerde daha iyi anlamak için bunları başvurabilirsiniz veya bu değerleri kullanabilirsiniz.

**Ağ örnek değerler tablosu**
|   |Azure Stack|Azure|
|---------|---------|---------|
|Sanal ağ adı     |Azs-VNet|AzureVNet |
|Sanal ağ adres alanı |10.1.0.0/16|10.100.0.0/16|
|Alt ağ adı     |FrontEnd|FrontEnd|
|Alt ağ adres aralığı|10.1.0.0/24 |10.100.0.0/24 |
|Ağ geçidi alt ağı     |10.1.1.0/24|10.100.1.0/24|

## <a name="create-the-network-resources-in-azure"></a>Ağ kaynakları oluşturma

İlk Azure için ağ kaynaklarına oluşturun. Aşağıdaki yönergeleri kullanarak kaynak oluşturmak nasıl Göster [Azure portal](http://portal.azure.com/).

### <a name="create-the-virtual-network-and-vm-subnet"></a>Sanal ağ ve VM alt ağı oluşturma

1. Oturum [Azure portal](http://portal.azure.com/) Azure hesabınızı kullanarak.
2. Kullanıcı Portalı'nda seçin **yeni**.
3. Git **Market**ve ardından **ağ**.
4. Seçin **sanal ağ**.
5. Azure için değerleri belirlemek için ağ yapılandırması tabloda bulunan bilgileri kullanın **adı**, **adres alanı**, **alt ağ adı**, ve **alt ağ adresi Aralık**.
6. İçin **kaynak grubu**, yeni bir kaynak grubu oluşturun veya zaten varsa, seçin **var olanı kullan**.
7. Seçin **konumu** Vnet'iniz olarak.  Örnek değerler kullanıyorsanız seçin **Doğu ABD** veya tercih ederseniz başka bir konum kullanın.
8. **Panoya sabitle**’yi seçin.
9. **Oluştur**’u seçin.

### <a name="create-the-gateway-subnet"></a>Ağ Geçidi Alt Ağı oluşturma
1. Oluşturduğunuz sanal ağ kaynağı açın (**AzureVNet**) panodan.
2. Üzerinde **ayarları** bölümünde, select **alt ağlar**.
3. Seçin **ağ geçidi alt ağı** sanal ağa bir ağ geçidi alt ağı eklemek için.
4. Alt ağın adı varsayılan olarak **GatewaySubnet** şeklinde ayarlanır.
   Ağ geçidi alt ağları özeldir ve düzgün şekilde çalışabilmesi için bu ada sahip olmalıdır.
5. İçinde **adres aralığı** alan, adresi doğrulayın **10.100.0.0/24**.
6. Seçin **Tamam** ağ geçidi alt ağı oluşturmak için.

### <a name="create-the-virtual-network-gateway"></a>Sanal ağ geçidini oluşturma
1. Azure portalında seçin **yeni**.  
2. Git **Market**ve ardından **ağ**.
3. Ağ kaynakları listesinden seçin **sanal ağ geçidi**.
4. İçinde **adı**, türü **Azure-GW**.
5. Bir sanal ağ seçmek için Seç **sanal ağ**. Ardından **AzureVnet** listeden.
6. Seçin **genel IP adresi**. Zaman **genel IP adresi seçin** bölümü açılır, select **Yeni Oluştur**.
7. İçinde **adı**, türü **Azure GW PIP**ve ardından **Tamam**.
8. Varsayılan olarak, için **VPN türü**, **rota tabanlı** seçilir.
    Tutmak **rota tabanlı** VPN türü.
9. **Abonelik** ve **Konum** seçeneklerinin doğruluğunu onaylayın. Kaynak panoya sabitleyebilirsiniz. **Oluştur**’u seçin.

### <a name="create-the-local-network-gateway-resource"></a>Yerel ağ geçidi kaynağı oluşturma

1. Azure portalında seçin **yeni**. 
4. Git **Market**ve ardından **ağ**.
5. Kaynakların listesinden **yerel ağ geçidi**.
6. İçinde **adı**, türü **Azs-GW**.
7. İçinde **IP adresi**, Azure yığın sanal ağ, daha önce ağ yapılandırması tabloda listelenen geçidinizin genel IP adresini yazın.
8. İçinde **adres alanı**, Azure yığınından yazın **10.0.10.0/23** adres alanı için **AzureVNet**.
9. Doğrulayın, **abonelik**, **kaynak grubu**, ve **konumu** doğru olduğunu onaylayın ve ardından **oluşturma**.

## <a name="create-the-connection"></a>Bağlantı oluşturma
1. Kullanıcı Portalı'nda seçin **yeni**. 
2. Git **Market**ve ardından **ağ**.
3. Kaynakların listesinden **bağlantı**.
4. Üzerinde **temel** Ayarlar bölümünde için **bağlantı türü**, seçin **siteden siteye (IPSec)**.
5. Seçin **abonelik**, **kaynak grubu**, ve **konumu**ve ardından **Tamam**.
6. Üzerinde **ayarları** bölümünde, select **sanal ağ geçidi**ve ardından **Azure-GW**.
7. Seçin **yerel ağ geçidi**ve ardından **Azs-GW**.
8. İçinde **bağlantı adı**, türü **Azure Azs**.
9. İçinde **paylaşılan anahtar (PSK)**, türü **12345**. Farklı bir değer seçerseniz, bu BT unutmayın *gerekir* bağlantı diğer ucundaki oluşturduğunuz paylaşılan anahtarı için değer ile eşleşmelidir. **Tamam**’ı seçin.
10. Gözden geçirme **Özet** bölümünde ve ardından **Tamam**.

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma
Bir sanal makine Azure'da şimdi oluşturmak ve sanal ağınızda VM alt ağınız üzerinde yerleştirin.

1. Azure portalında seçin **yeni**.
2. Git **Market**ve ardından **işlem**.
3. Sanal makine görüntülerini listesinde seçin **Windows Server 2016 Datacenter Eval** görüntü.
4. Üzerinde **Temelleri** bölüm için **adı**, türü **AzureVM**.
5. Geçerli kullanıcı adı ve parola yazın. Oluşturulduktan sonra sanal makinede oturum açmak için bu hesabı kullanın.
6. Sağlayan bir **abonelik**, **kaynak grubu**, ve **konumu**ve ardından **Tamam**.
7. Üzerinde **boyutu** bölümünde, bu örnek için bir sanal makine boyutu seçin ve ardından **seçin**.
8. Üzerinde **ayarları** bölümünde, Varsayılanları kabul edebilir. Olduğundan emin olun **AzureVnet** sanal ağ seçilir ve alt ağ değerine ayarlandığını doğrulayın **10.0.20.0/24**. **Tamam**’ı seçin.
9. Ayarları gözden **Özet** bölümünde ve ardından **Tamam**.

## <a name="create-the-network-resources-in-azure-stack"></a>Ağ kaynakları Azure yığınında oluşturun
Ardından Azure yığınında ağ kaynaklarını oluşturun.

### <a name="sign-in-as-a-user"></a>Bir kullanıcı olarak oturum aç
Hizmet Yöneticisi kullanıcı planları, sunar ve bunların kullanıcıların kullanabileceği abonelikleri test etmek için oturum açabilirsiniz. Zaten yoksa, [bir kullanıcı hesabı oluşturma](azure-stack-add-new-user-aad.md) oturum açmadan önce.

### <a name="create-the-virtual-network-and-vm-subnet"></a>Sanal ağ ve VM alt ağı oluşturma
1. Kullanıcı portalında oturum açmak için bir kullanıcı hesabı kullanın.
2. Kullanıcı Portalı'nda seçin **yeni**.

    ![Yeni sanal ağ oluştur](media/azure-stack-create-vpn-connection-one-node-tp2/image3.png)

3. Git **Market**ve ardından **ağ**.
4. Seçin **sanal ağ**.
5. İçin **adı**, **adres alanı**, **alt ağ adı**, ve **alt ağ adres aralığı**, ağ yapılandırması tablosundan değerleri kullanın.
6. İçinde **abonelik**, daha önce oluşturduğunuz aboneliği görünüyor.
7. İçin **kaynak grubu**, bir kaynak grubu oluşturabilir veya zaten bir varsa seçin **var olanı kullan**.
8. Varsayılan konumu doğrulayın.
9. **Panoya sabitle**’yi seçin.
10. **Oluştur**’u seçin.

### <a name="create-the-gateway-subnet"></a>Ağ geçidi alt ağını oluşturma
1. Panoda oluşturduğunuz Azs VNet sanal ağ kaynağı açın.
2. Üzerinde **ayarları** bölümünde, select **alt ağlar**.
3. Sanal ağ için ağ geçidi alt ağı eklemek için seçin **ağ geçidi alt ağı**.
   
    ![Ağ geçidi alt ağı Ekle](media/azure-stack-create-vpn-connection-one-node-tp2/image4.png)

4. Varsayılan olarak, alt ağ adı ayarlamak **GatewaySubnet**.
   Ağ geçidi alt ağları özel. Düzgün çalışması için bunlar kullanmalısınız *GatewaySubnet* adı.
5. İçinde **adres aralığı**, adresi olduğunu doğrulayın **10.1.1.0/24**.
6. Seçin **Tamam** ağ geçidi alt ağı oluşturmak için.

### <a name="create-the-virtual-network-gateway"></a>Sanal ağ geçidini oluşturma
1. Azure yığın Portalı'nda seçin **yeni**. 
2. Git **Market**ve ardından **ağ**.
3. Ağ kaynakları listesinden seçin **sanal ağ geçidi**.
4. İçinde **adı**, türü **Azs-GW**.
5. Seçin **sanal ağ** öğesi bir sanal ağ seçin.
   Seçin **Azs VNet** listeden.
6. Seçin **genel IP adresi** menü öğesi. Zaman **genel IP adresi seçin** bölümü açılır, select **Yeni Oluştur**.
7. İçinde **adı**, türü **Azs GW PIP**ve ardından **Tamam**.
8.  Varsayılan olarak, için **VPN türü**, **rota tabanlı** seçilir.
    Tutmak **rota tabanlı** VPN türü.
9. **Abonelik** ve **Konum** seçeneklerinin doğruluğunu onaylayın. Kaynak panoya sabitleyebilirsiniz. **Oluştur**’u seçin.

### <a name="create-the-local-network-gateway"></a>Yerel ağ geçidini oluşturma
Kavramı bir *yerel ağ geçidi* Azure yığınında bir Azure dağıtımında biraz farklıdır.

Bir Azure dağıtımında azure'da bir sanal ağ geçidi bağlanmak için kullandığı bir şirket içi (kullanıcı konumda) fiziksel aygıt, bir yerel ağ geçidi temsil eder. Azure yığınında bağlantının her iki ucuna sanal ağ geçitlerini demektir!

Bu konuda daha genel düşünmek için bir yerel ağ geçidi kaynağı her zaman bağlantısının diğer ucundaki uzak ağ geçidi belirten yoludur. 

### <a name="create-the-local-network-gateway-resource"></a>Yerel ağ geçidi kaynağı oluşturma
1. Azure yığın portalında oturum açın.
2. Kullanıcı Portalı'nda seçin **yeni**.
3. Git **Market**ve ardından **ağ**.
4. Kaynakların listesinden **yerel ağ geçidi**.
5. İçinde **adı**, türü **Azure-GW**.
6. İçinde **IP adresi**, Azure'da sanal ağ geçidi için genel IP adresi yazın **Azure GW PIP**. Bu adres, daha önce ağ yapılandırması tablosunda görünür.
7. İçinde **adres alanı**, oluşturduğunuz Azure VNET adres alanı için yazın **10.0.20.0/23**.
8. Doğrulayın, **abonelik**, **kaynak grubu**, ve **konumu** doğru olduğunu onaylayın ve ardından **oluşturma**.

### <a name="create-the-connection"></a>Bağlantı oluşturma
1. Kullanıcı Portalı'nda seçin **yeni**.
2. Git **Market**ve ardından **ağ**.
3. Kaynakların listesinden **bağlantı**.
4. Üzerinde **Temelleri** Ayarlar bölümünde için **bağlantı türü**seçin **siteden siteye (IPSec)**.
5. Seçin **abonelik**, **kaynak grubu**, ve **konumu**ve ardından **Tamam**.
6. Üzerinde **ayarları** bölümünde, select **sanal ağ geçidi**ve ardından **Azs-GW**.
7. Seçin **yerel ağ geçidi**ve ardından **Azure-GW**.
8. İçinde **bağlantı adı**, türü **Azs Azure**.
9. İçinde **paylaşılan anahtar (PSK)**, türü **12345**ve ardından **Tamam**.
10. Üzerinde **Özet** bölümünde, select **Tamam**.

### <a name="create-a-vm"></a>VM oluşturma
VPN bağlantısı üzerinden geçen verileri doğrulamak için VPN tüneli üzerinden veri göndermek ve almak için her ucunda sanal makineleri oluşturmanız gerekir. 

1. Azure portalında seçin **yeni**.
2. Git **Market**ve ardından **işlem**.
3. Sanal makine görüntülerini listesinde seçin **Windows Server 2016 Datacenter Eval** görüntü.
4. Üzerinde **Temelleri** bölümünde **adı**, türü **Azs VM**.
5. Geçerli kullanıcı adı ve parola yazın. Oluşturulduktan sonra VM oturum açmak için bu hesabı kullanın.
6. Sağlayan bir **abonelik**, **kaynak grubu**, ve **konumu**ve ardından **Tamam**.
7. Üzerinde **boyutu** bölümünde, bu örnek için select bir sanal makine boyutu ve ardından **seçin**.
8. Üzerinde **ayarları** bölümünde, Varsayılanları kabul edin. Olduğundan emin olun **Azs VNet** sanal ağın seçili. Alt ağ değerine ayarlandığını doğrulayın **10.1.0.0/24**. Ardından **Tamam**.
9. Üzerinde **Özet** bölümünde, ayarları gözden geçirin ve ardından **Tamam**.


## <a name="test-the-connection"></a>Bağlantıyı sınama
Siteden siteye bağlantı kuran, üzerinden akan trafik alabilir doğrulamalıdır. Doğrulamak için Azure yığınında oluşturulan sanal makinelerden biri için oturum açın. Ardından, Azure üzerinde oluşturulan sanal makine ping işlemi uygulayın. 

Siteden siteye bağlantı üzerinden trafiği gönderme emin olmak için uzak alt ağ, VIP sanal makineye doğrudan IP'yi (DIP) adresini ping işlemi yapın. Bunu yapmak için diğer uçtaki bağlantının DIP adresi bulun. Daha sonra kullanmak için adresi kaydedin.

### <a name="sign-in-to-the-user-vm-in-azure-stack"></a>VM Azure yığınında kullanıcı için oturum açın
1. Azure yığın portalında oturum açın.
2. Sol gezinti çubuğunda seçin **sanal makineleri**.
3. Sanal makineleri listesinde bulun **Azs VM** daha önce oluşturduğunuz ve ardından seçin.
4. Sanal makine için bölümünü tıklatın **Bağlan**ve Azs VM.rdp dosyasını açın.
   
     ![Düğme Bağlan](media/azure-stack-create-vpn-connection-one-node-tp2/image17.png)
5. Sanal makine oluşturduğunuzda, yapılandırdığınız hesabıyla oturum açın.
6. Yükseltilmiş bir açık **Windows PowerShell** penceresi.
7. **ipconfig /all** yazın.
8. Çıktıda Bul **IPv4 adresi**ve daha sonra kullanmak için adresi kaydedin. Bu, Azure ping gönderecek adresidir. Örnek ortamda adres **10.0.10.4** şeklindedir, ancak sizin ortamınızda farklı olabilir. İçinde girecektir **10.0.10.0/24** daha önce oluşturduğunuz alt ağ.
9. Sanal makinenin ping komutuna yanıt veren bir güvenlik duvarı kuralı oluşturmak için aşağıdaki PowerShell komutunu çalıştırın:

   ```powershell
   New-NetFirewallRule `
    –DisplayName “Allow ICMPv4-In” `
    –Protocol ICMPv4
   ```

### <a name="sign-in-to-the-tenant-vm-in-azure"></a>Kiracı Azure VM'de oturum açın
1. Azure Portal’da oturum açın.
2. Sol gezinti çubuğunda **sanal makineleri**.
3. Sanal makineler listesinden Bul **Azure VM** daha önce oluşturduğunuz ve ardından seçin.
4. Sanal makine için bölümünü tıklatın **Bağlan**.
5. Sanal makine oluşturduğunuzda, yapılandırdığınız hesabıyla oturum açın.
6. Yükseltilmiş bir açık **Windows PowerShell** penceresi.
7. **ipconfig /all** yazın.
8. İçine düşerse bir IPv4 adresi görmeniz gerekir **10.0.20.0/24**. Örnek ortamında adresidir **10.0.20.4**, ancak adresinizi farklı olabilir.
9. Sanal makinenin ping komutuna yanıt veren bir güvenlik duvarı kuralı oluşturmak için aşağıdaki PowerShell komutunu çalıştırın:

   ```powershell
   New-NetFirewallRule `
    –DisplayName “Allow ICMPv4-In” `
    –Protocol ICMPv4
   ```

10. Azure'da sanal makineden Azure yığınında, sanal makine tüneli üzerinden ping işlemi yapın. Bunu yapmak için Azs VM'den kaydettiğiniz DIP ping atın.
   Örnek ortamında budur **10.0.10.4**, ancak laboratuvarınızda ettiğiniz adresi ping emin olun. Aşağıdaki ekran görüntüsü gibi görünen bir sonuç görmeniz gerekir:
   
    ![PING başarılı](media/azure-stack-create-vpn-connection-one-node-tp2/image19b.png)
11. Uzak sanal makineden bir yanıt başarılı bir test gösteriyor! Sanal makine penceresinin kapatabilirsiniz. Bağlantınızı test veri aktarımları bir dosya kopyalama gibi diğer tür deneyebilirsiniz.

### <a name="viewing-data-transfer-statistics-through-the-gateway-connection"></a>Ağ geçidi bağlantısı üzerinden veri aktarımı istatistiklerini görüntüleme
Siteden siteye bağlantınızı ne kadar veri geçirmeden bilmek istiyorsanız, bu bilgi edinilebilir **bağlantı** bölümü. Bu test ayrıca yalnızca gönderilen ping gerçekte VPN bağlantısı üzerinden geçmeden doğrulamak için başka bir yoludur.

1. Azure yığın içindeki kullanıcı sanal makineye oturum açtınız yaparken, kullanıcı portalında oturum açmak için kullanıcı hesabınızı kullanın.
2. Git **tüm kaynakları**ve ardından **Azs Azure** bağlantı. **Bağlantıları** görüntülenir.
4. Üzerinde **bağlantı** bölümünde, istatistikleri **verileri** ve **verileri** görünür. Aşağıdaki ekran görüntüsünde, çok sayıda ek dosya aktarımı öznitelikli. Bazı sıfır olmayan değerler var. görmeniz gerekir.
   
    ![Veri giriş ve çıkış](media/azure-stack-connect-vpn/Connection.png)

## <a name="next-steps"></a>Sonraki adımlar

[Azure ve Azure uygulamaları dağıtmak yığını](azure-stack-solution-pipeline.md)