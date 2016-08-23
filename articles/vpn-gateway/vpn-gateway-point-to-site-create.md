<properties
   pageTitle="Klasik portalı kullanarak bir Azure Sanal Ağa Noktadan Siteye bir VPN bağlantısı yapılandırma | Microsoft Azure"
   description="Noktadan Siteye bir VPN bağlantısı oluşturarak Azure Sanal Ağınıza güvenli bir şekilde bağlanın. Hizmet Yönetimi (klasik) dağıtım modeli kullanılarak oluşturulan sanal ağlara yönelik yönergeler."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/18/2016"
   ms.author="cherylmc"/>

# Klasik portalı kullanarak bir Sanal Ağa Noktadan Siteye bir VPN bağlantısı yapılandırma

> [AZURE.SELECTOR]
- [PowerShell - Resource Manager](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Portal - Klasik](vpn-gateway-point-to-site-create.md)

Noktadan Siteye yapılandırma, bir istemci bilgisayardan sanal ağınıza tek başına güvenli bir bağlantı oluşturmanıza olanak sağlar. VPN bağlantısı, bağlantının istemci bilgisayardan başlatılmasıyla oluşturulur. Sanal ağınıza uzak bir konumdan (örneğin, evden veya bir konferanstan) bağlanmak istediğinizde ya da sanal bir ağa bağlanması gereken yalnızca birkaç istemciniz bulunduğunda Noktadan Siteye ideal bir çözümdür. 

Noktadan Siteye bağlantıların çalışması için bir VPN cihazına veya genel kullanıma yönelik bir IP adresine gerek yoktur. Noktadan Siteye bağlantılar hakkında daha fazla bilgi için [VPN Gateway Hakkında SSS](vpn-gateway-vpn-faq.md#point-to-site-connections) ve [Şirket içi ve dışı karışık bağlantılar hakkında](vpn-gateway-cross-premises-options.md) makalelerine bakın.

Bu makale, **klasik dağıtım modeli** (Hizmet Yönetimi) ve klasik portal kullanılarak oluşturulan bir sanal ağa Noktadan Siteye VPN Gateway bağlantıları için geçerlidir. Azure portalına yönelik yönergeler hazır olduğunda ilgili makaleye bu sayfadan bir bağlantı sağlanacaktır.

**Noktadan Siteye bağlantılar için dağıtım modelleri ve araçları**

[AZURE.INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)] 


**Azure dağıtım modelleri hakkında**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

![Noktadan Siteye diyagramı](./media/vpn-gateway-point-to-site-create/point2site.png "point-to-site")



## Noktadan Siteye bağlantı oluşturma hakkında
 
Sanal bir ağa güvenli bir Noktadan Siteye bağlantı oluşturulması aşağıda adım adım açıklanmaktadır. Yapılandırılması için birçok adımı gerçekleştirmek gerekse de Noktadan Siteye bir bağlantı, bir VPN cihazı edinip yapılandırmaya gerek kalmadan bilgisayarınızdan sanal ağınıza güvenli bir bağlantı kurmak için harika bir yoldur. 

Noktadan Siteye bağlantının yapılandırılması 4 bölüme ayrılmıştır. Bu bölümlerin yapılandırılma sırası önemlidir. Bu nedenle, hiçbir adımı yapmadan geçmeyin veya ileri atlamayın.


- **1. Bölüm**, bir sanal ağ ve VPN ağ geçidi oluşturmada size yol gösterir.
- **2. Bölüm**, kimlik doğrulamak için kullanılan sertifikaları oluşturmanıza ve bunları yüklemenize yardımcı olur.
- **3. Bölüm**, istemci sertifikalarınızı dışarı aktarma ve yüklemede size yol gösterir.
- **4. Bölüm**, VPN istemcinizi yapılandırmada size yol gösterir.

## 1. Bölüm - Sanal ağ ve VPN ağ geçidi oluşturma


### 1. Kısım: Sanal ağ oluşturma

1. [Klasik Azure portalında](https://manage.windowsazure.com/) oturum açın. Buradaki adımlarda Azure portalın değil klasik portalın kullanıldığını unutmayın.

2. Ekranın sol alt köşesinde **Yeni**’ye tıklayın. Gezinme bölmesinde **Ağ Hizmetleri**’ne, sonra da **Virtual Network**’a tıklayın. Yapılandırma sihirbazını başlatmak için **Özel Oluştur**’a tıklayın.

3. **Sanal Ağ Ayrıntıları** sayfasında aşağıdaki bilgileri girin ve sağ alt köşedeki ileri okuna tıklayın.
    - **Ad**: Sanal ağınıza bir ad verin. Örneğin, "VNetEast". Bu ad, bu sanal ağa VM’ler ve PaaS örnekleri dağıtırken başvuracağınız ad olacaktır.
    - **Konum**: Konum, kaynaklarınızın (VM’ler) bulunmasını istediğiniz fiziksel konum (bölge) ile doğrudan ilişkilidir. Örneğin, bu sanal ağa dağıttığınız VM’lerin fiziksel olarak Doğu ABD’de bulunmasını istiyorsanız, o konumu seçin. Sanal ağınızı oluşturduktan sonra sanal ağınızla ilişkili bölgeyi değiştiremezsiniz.

4. **DNS Sunucuları ve VPN Bağlantısı** sayfasında aşağıdaki bilgileri girin ve sağ alt köşedeki ileri okuna tıklayın.
    - **DNS Sunucuları**: DNS sunucusunun adını ve IP adresini girin veya kısayol menüsünden, önceden kaydedilmiş bir DNS sunucusu seçin. Bu ayar, DNS sunucusu oluşturmaz; bu sanal ağ için ad çözümlemede kullanmak istediğiniz DNS sunucularını belirtmenizi sağlar. Azure varsayılan ad çözümleme hizmetini kullanmak istiyorsanız bu bölümü boş bırakın.
    - **Noktadan Siteye VPN’yi Yapılandır**: Onay kutusunu işaretleyin.

5. **Noktadan Siteye Bağlantı** sayfasında VPN istemcilerinizin bağlandıklarında bir IP adresi alacağı IP adresi aralığını belirleyin. Belirleyebileceğiniz adres aralıklarına ilişkin bazı kurallar vardır. Belirlediğiniz aralığın, şirket içi ağınızda yer alan aralıkların hiçbiriyle çakışmadığını doğrulamanız çok önemlidir.

6. Aşağıdaki bilgileri girin ve ileri okuna tıklayın.
 - **Adres Alanı**: Başlangıç IP’sini ve CIDR’yi (Adres Sayısı) ekleyin.
 - **Adres alanı ekle**: Yalnızca ağ tasarımınız için gerekiyorsa adres alanı ekleyin.

7. **Sanal Ağ Adres Alanları** sayfasında sanal ağınız için kullanmak istediğiniz adres aralığını belirtin. Belirlediğiniz adresler, bu sanal ağa dağıtacağınız VM’ler ve diğer rol örneklerine atanacak olan dinamik IP adresleridir (DIPS). Şirket içi ağınız için kullanılan aralıklardan herhangi biriyle çakışmayan bir aralık seçmeniz çok önemlidir. Bu konuda ağ yöneticinizle birlikte çalışmalısınız. Ağ yöneticinizin, şirket içi ağ adresi alanından, sanal ağınızda kullanabilmeniz için bir IP adresi aralığı ayırması gerekebilir.

8. Aşağıdaki bilgileri girin ve onay işaretine tıklayarak sanal ağınızı oluşturmaya başlayın.
 - **Adres Alanı**: Başlangıç IP’si ve Sayı dahil olmak üzere, bu sanal ağ için kullanmak istediğiniz iç IP adresi aralığını ekleyin. Şirket içi ağınız için kullanılan aralıklardan herhangi biriyle çakışmayan bir aralık seçmeniz önemlidir. Bu konuda ağ yöneticinizle birlikte çalışmalısınız. Ağ yöneticinizin, şirket içi ağ adresi alanından, sanal ağınızda kullanabilmeniz için bir IP adresi aralığı ayırması gerekebilir.
 - **Alt ağ ekle**: Ek alt ağlar şart değildir, ancak statik DIPS’leri olacak VM’ler için ayrı bir alt ağ oluşturmak isteyebilirsiniz. Ayrıca VM’lerinizi diğer rol örneklerinizden ayrı bir alt ağda tutmak da isteyebilirsiniz.
 - **Ağ geçidi alt ağı ekle**: Noktadan Siteye VPN için ağ geçidi alt ağı gereklidir. Ağ geçidi alt ağı eklemek için tıklayın. Ağ geçidi alt ağı, yalnızca sanal ağ geçidi için kullanılır.

9. Sanal ağınız oluşturulduğunda, Klasik Azure Portalı’nın ağlar sayfasındaki **Durum**’un altında, **Oluşturuldu** yazısını görürsünüz. Sanal ağınız oluşturulduktan sonra, dinamik yönlendirme ağ geçidinizi oluşturabilirsiniz.

### 2. Kısım: Dinamik yönlendirme ağ geçidi oluşturma

Ağ geçidi türü, dinamik şeklinde yapılandırılmalıdır. Statik yönlendirme ağ geçitleri bu özellik ile çalışmaz.

1. Klasik Azure Portalı’ndaki **Ağlar** sayfasında, yeni oluşturduğunuz sanal ağa tıklayın ve **Pano** sayfasına gidin.

2. **Pano** sayfasının alt kısmında yer alan **Ağ Geçidi Oluştur**’a tıklayın. **Do you want to create a gateway for virtual network "yournetwork"** ("Ağınız" sanal ağı için bir ağ geçidi oluşturmak istiyor musunuz?) diye soran bir ileti görünür. Ağ geçidini oluşturmaya başlamak için **Evet**’e tıklayın. Ağ geçidinin oluşturulması yaklaşık 15 dakika sürebilir.

## 2. Bölüm - Sertifikaları oluşturma ve yükleme

Noktadan Siteye VPN’lerde VPN istemcilerinin kimlik doğrulamasını yapmak için sertifikalar kullanılır. Kurumsal bir sertifika çözümü tarafından oluşturulan veya otomatik olarak imzalanan sertifikalar kullanabilirsiniz. Azure’a en çok 20 kök sertifikası yükleyebilirsiniz.

Otomatik olarak imzalanan bir sertifika kullanmak istiyorsanız, ne yapmanız gerektiği adım adım aşağıda anlatılmaktadır. Kurumsal bir sertifika çözümü kullanmayı planlıyorsanız her bölüm içerisindeki adımlar farklı olacaktır, ancak aşağıdaki adımları yine de uygulamanız gerekir:

### 1. Kısım: Kök sertifikası tanımlama veya oluşturma

Kurumsal bir sertifika çözümü kullanmıyorsanız, otomatik olarak imzalanan bir sertifika oluşturmanız gerekir. Bu bölümdeki adımlar Windows 8 düşünülerek yazılmıştır. Windows 10’a yönelik adımlar için [Working with self-signed root certificates for Point-to-Site configurations](vpn-gateway-certificates-point-to-site.md) (Noktadan Siteye yapılandırmaları için otomatik olarak imzalanan kök sertifikalar ile çalışma) makalesine başvurabilirsiniz.

X.509 sertifikası oluşturmanın yollarından biri Sertifika Oluşturma Aracı’nı (makecert.exe) kullanmaktır. Makecert kullanmak için, ücretsiz bir uygulama olan [Microsoft Visual Studio Express](https://www.visualstudio.com/products/visual-studio-express-vs.aspx)’i indirip yükleyin.

1. Visual Studio Araçları klasörüne gidin ve komut istemini Yönetici olarak başlatın.

2. Aşağıdaki örnekte yer alan komut, bir kök sertifikası oluşturup bilgisayarınızın Kişisel sertifika depolama alanına yükler. Komut ayrıca, bu sertifikaya karşılık gelen, daha sonra Klasik Azure Portalı’na yükleyeceğiniz *.cer* dosyasını da oluşturur.

3. .cer dosyasını yerleştirmek istediğiniz dizine giderek aşağıdaki komutu çalıştırın. Burada *RootCertificateName* sertifika için kullanmak istediğiniz addır. Herhangi bir değişiklik yapmadan aşağıdaki örneği çalıştırırsanız, sonuç olarak bir kök sertifikası ve karşılık gelen *RootCertificateName.cer* dosyasını elde edersiniz.

>[AZURE.NOTE] Kendisinden istemci sertifikaları oluşturulacak bir kök sertifikası oluşturduğunuzdan, bu sertifikayı ve özel anahtarını dışarı aktarıp ileride kurtarma amaçlı kullanabileceğiniz güvenli bir konuma kaydetmek isteyebilirsiniz.

    makecert -sky exchange -r -n "CN=RootCertificateName" -pe -a sha1 -len 2048 -ss My "RootCertificateName.cer"

### 2. Kısım: Kök sertifika .cer dosyasını Klasik Azure Portalı’na yükleme

Her kök sertifikasının karşılık gelen .cer dosyasını Azure'a yüklemeniz gerekir. 20 adede kadar sertifika yükleyebilirsiniz.

1. Önceki yordamda, bir kök sertifikası oluştururken aynı zamanda bir de *.cer* dosyası oluşturdunuz. Bu dosyayı şimdi Klasik Azure Portalı’na yükleyeceksiniz. .cer dosyasının, kök sertifikasının özel anahtarını içermediğini unutmayın. 20 adede kadar kök sertifikası yükleyebilirsiniz.

2. Klasik Azure Portalı’nda, sanal ağınıza ait **Sertifikalar** sayfasında **Upload a root certificate** (Kök sertifikası yükle) seçeneğine tıklayın.

3. **Upload Certificate** (Sertifika Yükle) sayfasında .cer kök sertifikasını bulun ve onay işaretine tıklayın.

### 3. Kısım: İstemci sertifikası oluşturma

Aşağıdaki adımlarda, otomatik olarak imzalanan kök sertifikasından bir istemci sertifikası oluşturulması açıklanmaktadır. Bu bölümdeki adımlar Windows 8 düşünülerek yazılmıştır. Windows 10’a yönelik adımlar için [Working with self-signed root certificates for Point-to-Site configurations](vpn-gateway-certificates-point-to-site.md) (Noktadan Siteye yapılandırmaları için otomatik olarak imzalanan kök sertifikalar ile çalışma) makalesine başvurabilirsiniz. Kurumsal bir sertifika çözümü kullanıyorsanız, kullandığınız çözüme yönelik yönergeleri izleyin. 

1. Otomatik olarak imzalanan sertifikayı oluşturmak için kullandığınız bilgisayarda yönetici olarak bir Visual Studio komut istemi penceresi açın.

2. İstemci sertifikası dosyasını kaydetmek istediğiniz konuma gidin. *RootCertificateName* oluşturduğunuz otomatik olarak imzalanan kök sertifikasıdır. Aşağıdaki örneği çalıştırırsanız (RootCertificateName yerine kendi kök sertifikanızın adını koyarak) sonuçta Kişisel sertifika depolama alanınızda "ClientCertificateName" adlı bir istemci sertifikası elde edersiniz.

3. Aşağıdaki komutu yazın:

        makecert.exe -n "CN=ClientCertificateName" -pe -sky exchange -m 96 -ss My -in "RootCertificateName" -is my -a sha1

4. Tüm sertifikalar, bilgisayarınızdaki Kişisel sertifika depolama alanınızda depolanır. Doğrulamak için *certmgr* ile denetleyin. Bu yordamı kullanarak gereken sayıda istemci sertifikası oluşturabilirsiniz. Sanal ağa bağlamak istediğiniz her bilgisayar için benzersiz istemci sertifikaları oluşturmanızı öneririz.

## 3. Bölüm - İstemci sertifikasını dışarı aktarma ve yükleme

Sanal ağa bağlamak istediğiniz her bilgisayara bir istemci sertifikası yüklemek zorunlu bir adımdır. Aşağıdaki adımlar, istemci sertifikasının el ile yüklenmesinde size yol gösterecektir.

1. Sanal ağa bağlamak istediğiniz her bilgisayara bir istemci sertifikası yüklenmelidir. Bu nedenle, büyük olasılıkla birden çok istemci sertifikası oluşturup ardından bunları dışarı aktarmanız gerekecektir. İstemci sertifikalarını dışarı aktarmak için *certmgr.msc* aracını kullanın. Dışarı aktarmak istediğiniz istemci sertifikasına sağ tıklayın, **tüm görevler**’e ve ardından **dışarı aktar**’a tıklayın.
2. Özel anahtara sahip *istemci sertifikasını* dışarı aktarın. Bu bir *.pfx* dosyasıdır. Bu sertifika için ayarladığınız parolayı (anahtar) kaydettiğinizden ya da unutmayacağınızdan emin olun.
3. *.pfx* dosyasını istemci bilgisayara kopyalayın. İstemci bilgisayarda, üzerine çift tıklayarak *.pfx* dosyasını yükleyin. İstendiğinde parolayı girin. Yükleme konumunu değiştirmeyin.

## 4. Bölüm - VPN istemcinizi yapılandırma

Sanal ağa bağlanmak için VPN istemcinizi de yapılandırmanız gerekir. İstemcinin bağlanması için bir istemci sertifikasına ve uygun VPN istemcisi yapılandırmasına gerek vardır. VPN istemcinizi yapılandırmak için sırasıyla aşağıdaki adımları gerçekleştirin.

### 1. Kısım: VPN istemcisi yapılandırma paketini oluşturma

1. Klasik Azure Portalı’nda, sanal ağınıza ait **Pano** sayfasında, sağ köşedeki hızlı bakış menüsüne gidin ve sanal ağınıza bağlamak istediğiniz istemciyle ilgili VPN paketine tıklayın.

2. 
Aşağıdaki istemci işletim sistemleri desteklenmektedir:
 - Windows 7 (32 bit ve 64 bit)
 - Windows Server 2008 R2 (yalnızca 64 bit)
 - Windows 8 (32 bit ve 64 bit)
 - Windows 8.1 (32 bit ve 64 bit)
 - Windows Server 2012 (yalnızca 64 bit)
 - Windows Server 2012 R2 (yalnızca 64 bit)
 - Windows 10

3. Yükleneceği istemci işletim sistemine karşılık gelen indirme paketini seçin:
 - 32 bit istemciler için **32 bit İstemci VPN Paketini İndir**’i seçin.
 - 64 bit istemciler için **64 bit İstemci VPN Paketini İndir**’i seçin.

4. İstemci paketinizi oluşturmak birkaç dakika sürebilir. Paket tamamlandığında dosyayı indirebilirsiniz. İndirdiğiniz *.exe* dosyası yerel bilgisayarınızda güvenle depolanabilir.

5. Klasik Azure Portalı’nda VPN istemci paketini oluşturup indirdikten sonra, sanal ağınıza bağlanmak istediğiniz istemci bilgisayara istemci paketini yükleyebilirsiniz. VPN istemci paketini birden çok istemci bilgisayara yüklemeyi planlıyorsanız, bu bilgisayarların her birine bir istemci sertifikası da yüklü olduğundan emin olun. VPN istemci paketi, Windows'da yerleşik bulunan VPN istemci yazılımını yapılandırmaya yönelik yapılandırma bilgileri içerir. Paket, ek yazılım yüklemez.

### 2. Kısım: VPN yapılandırma paketini istemciye yükleme ve bağlantıyı başlatma 

1. Yapılandırma dosyasını, sanal ağınıza bağlamak istediğiniz bilgisayara yerel olarak kopyalayın ve .exe dosyasına çift tıklayın. Paket yüklendikten sonra VPN bağlantısını başlatabilirsiniz.
Yapılandırma paketinin Microsoft tarafından imzalanmadığını unutmayın. Paketi, kuruluşunuzun imzalama hizmetini kullanarak veya kendiniz [SignTool]( http://go.microsoft.com/fwlink/p/?LinkId=699327) ile imzalamak isteyebilirsiniz. Paketin imzasız olarak kullanılmasında bir sorun yoktur. Ancak, paket imzalanmamışsa yükleme sırasında bir uyarı gösterilir.
2. İstemci bilgisayarda VPN bağlantılarına gidin ve yeni oluşturduğunuz VPN bağlantısını bulun. Sanal ağınızla aynı ada sahip olacaktır. **Bağlan**'a tıklayın.
3. Ağ Geçidi uç noktası için otomatik olarak imzalanan bir sertifika oluşturmak amacıyla kullanılan bir açılır ileti görüntülenir. Yükseltilmiş ayrıcalıklar kullanmak için **Devam**’a tıklayın.
4. **Bağlantı** durum sayfasında **Bağlan**’a tıklayarak bağlantıyı başlatın.
5. Bir **Sertifika Seç** ekranı çıkarsa, gösterilen istemci sertifikasının bağlanmak için kullanmak istediğiniz sertifika olduğunu doğrulayın. Başka bir sertifika gösteriliyorsa, açılan liste okunu kullanarak doğru sertifikayı seçin ve **Tamam**’a tıklayın.
6. Artık sanal ağınıza bağlısınız ve sanal ağınızda barındırılan tüm hizmetlere ve sanal makinelere tam erişiminiz var.

### 3. Kısım: VPN bağlantısını doğrulama

1. VPN bağlantınızın etkin olduğunu doğrulamak için, yükseltilmiş bir komut istemi açın ve *ipconfig/all* komutunu çalıştırın.
2. Sonuçlara bakın. Aldığınız IP adresinin, sanal ağınızı oluştururken belirlediğiniz Noktadan Siteye bağlantı adres aralığı içerisinden bir adres olduğuna dikkat edin. Sonuçlar aşağıdakine benzer olmalıdır:

Örnek:



    PPP adapter VNetEast:
        Connection-specific DNS Suffix .:
        Description.....................: VNetEast
        Physical Address................:
        DHCP Enabled....................: No
        Autoconfiguration Enabled.......: Yes
        IPv4 Address....................: 192.168.130.2(Preferred)
        Subnet Mask.....................: 255.255.255.255
        Default Gateway.................:
        NetBIOS over Tcpip..............: Enabled

## Sonraki adımlar

Sanal ağınıza, sanal makineler ekleyebilirsiniz. Bkz. [Özel bir sanal makine oluşturma](../virtual-machines/virtual-machines-windows-classic-createportal.md).

Sanal Ağlar hakkında daha fazla bilgi edinmek için [Sanal Ağ Belgeleri](https://azure.microsoft.com/documentation/services/virtual-network/) sayfasına bakın.



<!--HONumber=Aug16_HO1-->


