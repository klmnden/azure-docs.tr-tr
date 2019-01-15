---
title: Bir Linux sanal makinesine Apache Tomcat ' ayarlama | Microsoft Docs
description: Linux çalıştıran Azure sanal makineleri kullanarak Apache tomcat7'yi ayarlama konusunda bilgi edinin.
services: virtual-machines-linux
documentationcenter: ''
author: NingKuang
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: 45ecc89c-1cb0-4e80-8944-bd0d0bbedfdc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/15/2015
ms.author: ningk
ms.openlocfilehash: 5a5d052052be447ea2ccbd9231d3b03d38c7615c
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2019
ms.locfileid: "54266952"
---
# <a name="set-up-tomcat7-on-a-linux-virtual-machine-with-azure"></a>Azure ile bir Linux sanal makinesine tomcat7'yi ayarlayın
Apache Tomcat (veya yalnızca Cakarta Tomcat adıysa ayrıca Tomcat) bir açık kaynak web sunucusu ve Apache Software Foundation (ASF) tarafından geliştirilen servlet kapsayıcısıdır. Tomcat, Java Servlet ve JavaServer sayfaları (JSP) belirtimlerine Sun Microsystems uygular. Tomcat, Java kodu çalıştırmak için saf Java HTTP web sunucusu ortamı sağlar. En basit yapılandırmadır, Tomcat, bir tek işletim sistemi işlemde çalıştırır. Bu işlem, Java sanal makinesi (JVM) çalışır. Her HTTP isteği bir tarayıcıdan Tomcat Tomcat işlemi ayrı bir iş parçacığı olarak işlenir.  

> [!IMPORTANT]
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Azure Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modelini kullanan ele alınmaktadır. En yeni dağıtımların Resource Manager modelini kullanmasını öneririz. Open JDK ve Tomcat bir Ubuntu VM dağıtmak için Resource Manager şablonu kullanmak için bkz: [bu makalede](https://azure.microsoft.com/documentation/templates/openjdk-tomcat-ubuntu-vm/).
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

Bu makalede, bir Linux görüntüsü üzerinde tomcat7'yi yükleyin ve Azure'da dağıtın.  

Şunları öğreneceksiniz:  

* Azure'da bir sanal makine oluşturma
* Sanal makine için tomcat7'yi hazırlamayı öğrenin.
* Tomcat7'yi yükleme.

Azure aboneliğiniz zaten sahip olduğunuz varsayılır.  Ücretsiz bir deneme sürümü için kaydolabilirsiniz değil, varsa [Azure Web sitesinde](https://azure.microsoft.com/). Bir MSDN aboneliğine sahip değilse [Microsoft Azure özel fiyatlandırması: MSDN, MPN ve BizSpark avantajları](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/?c=14-39). Azure hakkında daha fazla bilgi için bkz: [Azure nedir?](https://azure.microsoft.com/overview/what-is-azure/).

Bu makalede, Tomcat ve Linux temel bilgiye sahip olduğunu varsayar.  

## <a name="phase-1-create-an-image"></a>1. Aşama: Görüntü oluştur
Bu aşamada, Azure'da bir Linux görüntüsü kullanarak bir sanal makine oluşturacaksınız.  

### <a name="step-1-generate-an-ssh-authentication-key"></a>1. Adım: Bir SSH kimlik doğrulama anahtarı oluştur
SSH, sistem yöneticileri için önemli bir araçtır. Ancak, insan tarafından belirlenen bir parola temelinde erişim güvenliğini yapılandırma önerilmez. Kötü niyetli kullanıcılar, bir kullanıcı adı ve zayıf bir parolaya göre sisteme bozabilir.

Güzel bir haberimiz var uzaktan erişim açık bırakın ve parolaları hakkında endişe duymamanızı bir yolu yoktur. Bu yöntem, asimetrik şifreleme ile kimlik doğrulaması oluşur. Kullanıcının özel kimlik doğrulama veren bir anahtardır. Kullanıcı hesabının parola kimlik doğrulaması izin vermeyecek şekilde bile kilitleyebilirsiniz.

Bu yöntem başka bir avantajı, farklı sunuculara oturum açmak için farklı parolalarınızın gerekmeyen sağlamasıdır. Kişisel özel anahtarı tüm sunucularda, birkaç hatırlamak zorunda kalmasını önler kullanarak kimlik doğrulaması yapabilir.



SSH kimlik doğrulama anahtarını oluşturmak için aşağıdaki adımları izleyin.

1. İndirip PuTTYgen şu konumdan yükleyin: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
2. Run Puttygen.exe.
3. Tıklayın **Oluştur** anahtarları oluşturmak için. İşlem sırasında penceresinde boş alanı üzerinde fareyi hareket ettirerek doğrulukla artırabilirsiniz.  
   ![Generate yeni anahtar düğmesini gösteren puTTY anahtar Oluşturucu ekran görüntüsü][1]
4. Oluşturma işleminden sonra yeni bir ortak anahtar sertifikanızı Puttygen.exe gösterilir.  
   ![Yeni ortak anahtar ile Kaydet gösteren puTTY anahtar Oluşturucu ekran görüntüsü özel anahtar düğmesi][2]
5. Seçin ve ortak anahtarı kopyalayın ve publicKey.pem adlı bir dosyaya kaydedin. Tıklamayın **ortak anahtarı Kaydet**kaydedilmiş ortak anahtarın dosya biçimi istiyoruz ortak anahtardan farklı olduğu için.
6. Tıklayın **özel anahtarı Kaydet**ve privateKey.ppk adlı bir dosyaya kaydedin.

### <a name="step-2-create-the-image-in-the-azure-portal"></a>2. Adım: Azure portalında görüntüsü oluşturma
1. İçinde [portalı](https://portal.azure.com/), tıklayın **kaynak Oluştur** bir görüntü oluşturmak için görev çubuğunda. Ardından ihtiyaçlarınıza göre Linux görüntüsünü seçin. Aşağıdaki örnekte, Ubuntu 14.04 görüntü kullanır.
![Portal yeni düğmesini gösteren ekran görüntüsü][3]

2. İçin **ana bilgisayar adı**, siz ve Internet istemcileri bu sanal makineye erişmek için kullanacağı URL'yi adını belirtin. DNS adı, örneğin, tomcatdemo son kısmını tanımlayın. Azure, ardından URL'yi tomcatdemo.cloudapp.net oluşturur.  

3. İçin **SSH kimlik doğrulama anahtarı**, ortak anahtarı PuTTYgen tarafından oluşturulan içeren publicKey.pem dosyasından anahtar değerini kopyalayın.  
![SSH kimlik doğrulama anahtarı kutusu portalında][4]

4. Gerekirse diğer ayarları yapılandırın ve ardından **Oluştur**.  

## <a name="phase-2-prepare-your-virtual-machine-for-tomcat7"></a>2. Aşama: Sanal makinenizi tomcat7'yi için hazırlama
Bu aşamada, Tomcat trafiği bir uç noktasını yapılandırın ve ardından yeni sanal makinenize bağlanın.

### <a name="step-1-open-the-http-port-to-allow-web-access"></a>1. Adım: Web erişime izin vermek için HTTP bağlantı noktasını açın
Uç noktalar Azure genel ve özel bir bağlantı noktalarının yanı sıra bir TCP veya UDP protokolünü oluşur. Özel bağlantı noktası, hizmet sanal makine üzerinde dinleme yaptığı bağlantı noktasıdır. Genel bağlantı noktası, Azure bulut hizmeti için dışarıdan, Internet üzerinden gelen trafiği dinlediği bağlantı noktasıdır.  

TCP bağlantı noktası 8080 Tomcat dinlemek için kullandığı varsayılan bağlantı noktası numarasıdır. Bu bağlantı noktası bir Azure uç noktası ile açtıysanız, siz ve diğer Internet istemcilerinin Tomcat sayfalarına erişebilirsiniz.  

1. Portalında **Gözat** > **sanal makineler**ve ardından, oluşturduğunuz sanal makineye tıklayın.  
   ![Virtual machines dizininde ekran görüntüsü][5]
2. Sanal makinenize bir uç nokta eklemek için tıklatın **uç noktaları** kutusu.
   ![Uç noktaları kutusunu gösteren ekran görüntüsü][6]
3. **Ekle**'ye tıklayın.  

   1. Uç nokta için uç noktası için bir ad girin **uç nokta**ve 80 numaralı enter **genel bağlantı noktası**.  

      Değerinin 80'e ayarlarsanız, bağlantı noktası numarasını Tomcat erişmek için kullanılan URL'yi eklemeniz gerekmez. Örneğin, http://tomcatdemo.cloudapp.net.    

      81 gibi başka bir değere ayarlarsanız, için URL Tomcat erişmek için bağlantı noktası numarasını eklemeniz gerekir. Örneğin, http://tomcatdemo.cloudapp.net:81/.
   2. İçinde 8080 girin **özel bağlantı noktası**. Varsayılan olarak, Tomcat 8080 numaralı TCP bağlantı noktasını dinler. Varsayılan değer değiştiyse dinleme bağlantı noktası, Tomcat, güncelleştirmeniz gerekir **özel bağlantı noktası** Tomcat aynı dinleme bağlantı noktası olacak.  
      ![Ekran görüntüsü, Ekle komutunu, genel bağlantı noktası ve özel bağlantı noktası gösteren kullanıcı Arabirimi][7]
4. Tıklayın **Tamam** sanal makineniz için uç nokta ekleme.

### <a name="step-2-connect-to-the-image-you-created"></a>2. Adım: Oluşturduğunuz görüntüye bağlan
Sanal makinenize bağlanmak için herhangi bir SSH aracını seçebilirsiniz. Bu örnekte, PuTTY kullanırız.  

1. Portalda, sanal makinenin DNS adını alın.
    1. Tıklayın **Gözat** > **sanal makineler**.
    2. Sanal makinenizin adını seçin ve ardından **özellikleri**.
    3. İçinde **özellikleri** kutucuğunda, konum **etki alanı adı** DNS adını almak için kutusu.  

2. Gelen SSH bağlantıları için bağlantı noktası numarasını alma **SSH** kutusu.  
![SSH bağlantı bağlantı noktası numarasını gösteren ekran görüntüsü][8]

3. İndirme [PuTTY](http://www.putty.org/).  

4. İndirdikten sonra yürütülebilir dosya Putty.exe tıklayın. PuTTY yapılandırması, ana bilgisayar adıyla temel seçenekleri yapılandırın ve bağlantı noktası sanal makineniz özelliklerinden elde edilen numarası.   
![PuTTY yapılandırması ana bilgisayar adını ve bağlantı noktası seçeneklerini gösteren ekran görüntüsü][9]

5. Sol bölmede **bağlantı** > **SSH** > **Auth**ve ardından **Gözat** belirtmek için privateKey.ppk dosyasının konumu. PrivateKey.ppk dosyayı PuTTYgen tarafından daha önce içinde oluşturulan özel anahtarı içeren "1. Aşama: Bu makalede bir görüntü oluşturma"bölümü.  
![Bağlantı dizin hiyerarşisi ve Gözat düğmesini gösteren ekran görüntüsü][10]

6. **Aç**'a tıklayın. Bir ileti kutusu tarafından uyarı. DNS adı yapılandırdıysanız ve bağlantı noktası numarası doğru değilse tıklayın **Evet**.
![Bildirim gösteren ekran görüntüsü][11]

7. Kullanıcı adınızı girmeniz istenir.  
![Kullanıcı adı girin nerede gösteren ekran görüntüsü][12]

8. Sanal makineyi oluşturmak için kullanılan kullanıcı adı girin "1. Aşama: Görüntü oluşturma"bölümünde bu makalenin önceki kısımlarında. Aşağıdaki gibi bir şey görürsünüz:  
![Kimlik doğrulama gösteren ekran görüntüsü][13]

## <a name="phase-3-install-software"></a>3. Aşama: Yazılım yükleme
Bu aşamada Java Çalışma zamanı ortamı, tomcat7'yi ve diğer tomcat7'yi bileşenlerini yükleyin.  

### <a name="java-runtime-environment"></a>Java Çalışma zamanı ortamı
Tomcat, Java dilinde yazılır. Bkz: [Azure desteklenen jdk](https://aka.ms/azure-jdks) desteklenen Java Çalışma zamanları tam alma hakkında daha fazla bilgi için. Ayrıca kendi getirebilirsiniz, ancak bu makalenin geri kalanında Azure tarafından desteklenen sürümler kullanır.


#### <a name="install-azure-supported-jdk"></a>Azure desteklenen JDK yüklemek

İzleyin `apt-get` yükleme yönergeleri öğesinde belgelendirilen [Azul Zulu Enterprise Azure](https://www.azul.com/downloads/azure-only/zulu/#apt-repo) Web sitesi.

#### <a name="confirm-that-java-installation-is-successful"></a>Java yüklemesinin başarılı olduğunu onaylayın
Java Çalışma zamanı ortamı doğru yüklenip yüklenmediğini test etmek için aşağıdakine benzer bir komut kullanabilirsiniz:  
    Java-Sürüm  

Aşağıdaki gibi bir ileti görürsünüz: ![Başarılı OpenJDK yükleme iletisi][14]


### <a name="install-tomcat7"></a>Tomcat7'yi yükleme
Tomcat7'yi yüklemek için aşağıdaki komutu kullanın.  

    sudo apt-get install tomcat7  

Tomcat7'yi kullanmıyorsanız, bu komutun uygun varyasyonunu kullanın.  

#### <a name="confirm-that-tomcat7-installation-is-successful"></a>Tomcat7'yi yükleme işleminin başarılı olduğunu doğrulayın
Tomcat7'yi başarıyla yüklendiyse denetlemek için Tomcat sunucusunun DNS adına gidin. Bu makalede, örnek URL'dir http://tomcatexample.cloudapp.net/. Tomcat7'yi aşağıdaki gibi bir ileti görürseniz, doğru şekilde yüklendiğinden.
![Başarılı tomcat7'yi yükleme iletisi][16]

### <a name="install-other-tomcat7-components"></a>Diğer tomcat7'yi bileşenlerini yükleme
Yükleyebileceğiniz diğer isteğe bağlı Tomcat bileşenler vardır.  

Kullanım **sudo apt-cache arama tomcat7'yi** komut tüm kullanılabilir bileşenleri görün. Bazı yararlı bileşenlerini yüklemek için aşağıdaki komutları kullanın.  

    sudo apt-get install tomcat7-admin      #admin web applications

    sudo apt-get install tomcat7-user         #tools to create user instances  

## <a name="phase-4-configure-tomcat7"></a>4. Aşama: Configure Tomcat7
Bu aşamada, Tomcat yönetebilirsiniz.

### <a name="start-and-stop-tomcat7"></a>Başlatma ve durdurma tomcat7'yi
Yüklendiğinde tomcat7'yi sunucu otomatik olarak başlatılır. Ayrıca şu komutla başlayabilirsiniz:   

    sudo /etc/init.d/tomcat7 start

Tomcat7'yi durdurmak için:

    sudo /etc/init.d/tomcat7 stop

Tomcat7'yi durumunu görüntülemek için:

    sudo /etc/init.d/tomcat7 status

Tomcat hizmetleri yeniden başlatmak için: 

    sudo /etc/init.d/tomcat7 restart

### <a name="tomcat7-administration"></a>Tomcat7'yi Yönetim
Yönetici kimlik bilgilerinizi ayarlamak için Tomcat kullanıcı yapılandırma dosyasını düzenleyebilirsiniz. Aşağıdaki komutu kullanın:  

    sudo vi  /etc/tomcat7/tomcat-users.xml   

Örnek aşağıda verilmiştir:  
![Sudo VI komut çıktısını gösteren ekran görüntüsü][17]  

> [!NOTE]
> Yönetici kullanıcı adı için güçlü bir parola oluşturun.  

Bu dosyayı düzenledikten sonra değişiklikler etkili olmasını sağlamak için aşağıdaki komutu hizmetleriyle tomcat7'yi yeniden başlatmanız gerekir:  

    sudo /etc/init.d/tomcat7 restart  

Tarayıcınızı açın ve girin **http://<your tomcat server DNS name>/manager/html** URL. Bu makaledeki örnekte URL, http://tomcatexample.cloudapp.net/manager/html.  

Bağlandıktan sonra aşağıdakine benzer bir şey görmeniz gerekir:  
![Tomcat Web uygulaması Yöneticisi'nin ekran görüntüsü][18]

## <a name="common-issues"></a>Genel sorunlar
### <a name="cant-access-the-virtual-machine-with-tomcat-and-moodle-from-the-internet"></a>Sanal makine Tomcat ve Moodle ile Internet'ten erişemiyor.
#### <a name="symptom"></a>Belirti  
  Tomcat çalışıyor ancak tarayıcınızla Tomcat varsayılan sayfasını göremezsiniz.
#### <a name="possible-root-cause"></a>Olası kök nedeni   

  * Tomcat dinleme bağlantı noktası, sanal makinenin uç Tomcat trafiği için özel bağlantı noktası ile aynı değil.  

     Genel bağlantı noktası ve özel bağlantı noktası uç nokta ayarları denetleyin ve Tomcat aynı bağlantı noktasını dinleme özel bağlantı noktası olduğundan emin olun. Bkz: "1. Aşama: Bu makalede uç noktaları sanal makineniz için yapılandırmaya ilişkin yönergeler için görüntü oluşturma"bölümü.  

     Tomcat dinleme bağlantı noktasını belirlemek için /etc/httpd/conf/httpd.conf (Red Hat sürüm) veya /etc/tomcat7/server.xml (Debian sürüm) açın. Varsayılan olarak, Tomcat dinleme bağlantı noktası 8080'dir. Örnek aşağıda verilmiştir:  

        <Connector port="8080" protocol="HTTP/1.1"  connectionTimeout="20000"   URIEncoding="UTF-8"            redirectPort="8443" />  

     Varsayılan bağlantı noktası, Tomcat dinleme (örneğin, 8081) değiştirileceğini, Debian ve Ubuntu ve istediğiniz gibi bir sanal makine kullanıyorsanız, işletim sistemi için bağlantı noktasını da açmanız gerekir. İlk olarak, profili açın:  

        sudo vi /etc/default/tomcat7  

     Ardından son satırı açıklamadan çıkarın ve "no" "Evet" değiştirin.  

        AUTHBIND=yes
  2. Güvenlik Duvarı, dinleme bağlantı noktası Tomcat devre dışı bıraktı.

     Yalnızca yerel ana Tomcat varsayılan sayfasından da görebilirsiniz. Sorun büyük olasılıkla çok tarafından Tomcat dinlenen, bağlantı noktası güvenlik duvarı tarafından engellenir. Web sayfasına gidin, w3m Aracı'nı kullanabilirsiniz. Aşağıdaki komutlar w3m yükleyin ve Tomcat varsayılan sayfasına göz atın:  


        sudo yum yüklemesi w3m w3m-img


        w3m http://localhost:8080  
#### <a name="solution"></a>Çözüm

  * Tomcat dinleme bağlantı noktası sanal makineye trafiği uç noktasının özel bağlantı noktası ile aynı değil, Tomcat aynı bağlantı noktasını dinleme olması için özel bağlantı noktası değiştirmeniz.   
  2. Güvenlik Duvarı/iptables sorunu neden oluyorsa, /etc/sysconfig/iptables için aşağıdaki satırları ekleyin. İkinci satır yalnızca https trafiği için gereklidir:  

      -A -p tcp -m tcp--dport 80 -j kabul giriş

      -A -p tcp -m tcp 443--dport -j kabul giriş  

     > [!IMPORTANT]
     > Önceki satırları, genel olarak, aşağıdaki gibi erişimi kısıtlar satırları yukarıda konumlandırıldığı emin olun: INPUT -j--Red-with ICMP konak yasaklanmış REDDET



İptables yeniden yüklemek için aşağıdaki komutu çalıştırın:

    service iptables restart

Bu CentOS 6.3 üzerinde test edilmiştir.

### <a name="permission-denied-when-you-upload-project-files-to-varlibtomcat7webapps"></a>İzin reddedildi; /var/lib/tomcat7/webapps için proje dosyaları karşıya yüklediğinizde /
#### <a name="symptom"></a>Belirti
  Bir SFTP istemcisi (örneğin, FileZilla) kullandığınızda, sanal makinenize bağlanın ve /var/lib/tomcat7/webapps için gitmek için/sitenizi yayımlamak için bir hata iletisi aşağıdakine benzer olursunuz:  

     status:    Listing directory /var/lib/tomcat7/webapps
     Command:    put "C:\Users\liang\Desktop\info.jsp" "info.jsp"
     Error:    /var/lib/tomcat7/webapps/info.jsp: open for write: permission denied
     Error:    File transfer failed
#### <a name="possible-root-cause"></a>Olası kök nedeni
  /Var/lib/tomcat7/webapps klasöre erişim izni var.  
#### <a name="solution"></a>Çözüm  
  Kök hesabı izni almak gerekir. Makine sağladığında, kullandığınız kullanıcı adı için kök klasörün sahipliği değiştirebilirsiniz. Azureuser hesap adına sahip bir örnek aşağıda verilmiştir:  

     sudo chown azureuser -R /var/lib/tomcat7/webapps

  Bir dizin içinde tüm dosyalar için izinleri çok uygulamak için -R seçeneğini kullanın.  

  Bu komut ayrıca dizinler için çalışır. -R seçeneği, tüm dosya ve dizin içindeki dizinlerin izinlerini değiştirir. Örnek aşağıda verilmiştir:  

     sudo chown -R username:group directory  

  Bu komut tüm dosya ve dizin içinde olan dizinleri sahipliği (kullanıcı ve grup) değiştirir.  

  Aşağıdaki komut, yalnızca klasörü dizin izni değiştirir. Dosya ve klasörleri dizini içinde değiştirilmez.  

     sudo chown username:group directory

[1]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-01.png
[2]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-02.png
[3]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-03.png
[4]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-04.png
[5]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-05.png
[6]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-06.png
[7]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-07.png
[8]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-08.png
[9]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-09.png
[10]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-10.png
[11]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-11.png
[12]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-12.png
[13]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-13.png
[14]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-14.png
[15]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-15.png
[16]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-16.png
[17]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-17.png
[18]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-18.png
