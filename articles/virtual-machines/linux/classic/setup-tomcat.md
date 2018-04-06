---
title: Linux sanal makine üzerinde Apache Tomcat ayarlama | Microsoft Docs
description: Linux çalıştıran Azure sanal makineler kullanarak Apache tomcat7'yi ayarlayın öğrenin.
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
ms.openlocfilehash: 161a56a019f8c2c8ce5e3890e73ad5c5710e7b82
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="set-up-tomcat7-on-a-linux-virtual-machine-with-azure"></a>Azure ile Linux sanal makine tomcat7'yi ayarlayın
Apache Tomcat (veya yalnızca Cakarta Tomcat adıysa ayrıca Tomcat) bir açık kaynak web sunucusu ve Apache yazılım Foundation (ASF) tarafından geliştirilmiş servlet kapsayıcı değil. Tomcat Java Servlet'i ve Sun Microsystems JavaServer sayfaları (JSP) belirtimlerinden uygular. Tomcat Java kodu çalıştırmak için saf Java HTTP web sunucusu ortamı sağlar. En basit yapılandırmada, Tomcat tek işletim sistemi işleminde çalışır. Bu işlem, Java sanal makinesi (JVM) çalışır. Her HTTP isteğine bir tarayıcıdan Tomcat Tomcat işleminde ayrı bir iş parçacığı olarak işlenir.  

> [!IMPORTANT]
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Azure Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede Klasik dağıtım modeli kullanmayı kapsar. En yeni dağıtımların Resource Manager modelini kullanmasını öneririz. Açık JDK ve Tomcat bir Ubuntu VM dağıtmak için bir Resource Manager şablonu kullanmak için bkz: [bu makalede](https://azure.microsoft.com/documentation/templates/openjdk-tomcat-ubuntu-vm/).
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

Bu makalede, Linux görüntüde tomcat7'yi yükleyin ve Azure'da dağıtın.  

Şunları öğreneceksiniz:  

* Azure'da bir sanal makine oluşturma
* Sanal makine için tomcat7'yi hazırlamak nasıl.
* Tomcat7'yi yükleme.

Bir Azure aboneliği zaten sahip olduğunuz varsayılır.  Ücretsiz deneme için kaydolabilirsiniz değil, varsa [Azure Web sitesinde](https://azure.microsoft.com/). Bir MSDN aboneliğiniz varsa, bkz: [Microsoft Azure özel fiyatlandırma: MSDN, MPN ve BizSpark avantajları](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/?c=14-39). Azure hakkında daha fazla bilgi için bkz: [Azure nedir?](https://azure.microsoft.com/overview/what-is-azure/).

Bu makalede, Tomcat ve Linux temel bilgiye sahip olduğunuz varsayılmaktadır.  

## <a name="phase-1-create-an-image"></a>1. Aşama: görüntü oluşturma
Bu aşamada, Azure'da bir Linux görüntüsü kullanarak bir sanal makine oluşturur.  

### <a name="step-1-generate-an-ssh-authentication-key"></a>1. adım: bir SSH kimlik doğrulama anahtarı oluştur
SSH, sistem yöneticileri için önemli bir araçtır. Ancak, insan tarafından belirlenen bir parola tabanlı erişim güvenlik yapılandırma önerilmez. Kötü niyetli kullanıcılar, bir kullanıcı adı ve zayıf bir parolaya göre sisteminizi içine bozulabilir.

İyi haber uzaktan erişim açık bırakın ve parolalar hakkında endişelenmeniz olmayan bir yolu yoktur. Bu yöntem, asimetrik şifreleme ile kimlik doğrulaması oluşur. Kullanıcının özel anahtar kimlik doğrulaması veren adrestir. Kullanıcı hesabının parola kimlik doğrulaması izin vermeyecek şekilde bile kilitleyebilirsiniz.

Bu yöntemin başka bir avantajı, farklı sunuculara oturum açmak için farklı parolaları gerekmez ' dir. Kişisel özel anahtarı tüm sunucularda, birden fazla parolayı hatırlamak zorunda kalmaktan önleyen kullanılarak doğrulanabilir.



SSH kimlik doğrulama anahtarı oluşturmak için aşağıdaki adımları izleyin.

1. Karşıdan yükle ve PuTTYgen şu konumdan yükleyin: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
2. Puttygen.exe çalıştırın.
3. Tıklatın **Generate** anahtarları oluşturmak için. İşlem sırasında penceresinde boş alanı üzerinden fareyi hareket ettirerek rastgele artırabilir.  
   ![Generate yeni anahtar düğmesini gösteren puTTY anahtar Oluşturucu ekran görüntüsü][1]
4. Oluşturma işleminden sonra yeni bir ortak anahtar Puttygen.exe gösterir.  
   ![Yeni bir ortak anahtar ve kaydetme gösteren puTTY anahtar Oluşturucu ekran görüntüsü özel anahtar düğmesi][2]
5. Seçin ve ortak anahtarı kopyalayın ve publicKey.pem adlı bir dosyaya kaydedin. Tıklamayın **ortak anahtarı Kaydet**, kaydedilen ortak anahtarın dosya biçimi istiyoruz ortak anahtarı ile farklı olduğu için.
6. Tıklatın **özel anahtarı Kaydet**ve privateKey.ppk adlı bir dosyaya kaydedin.

### <a name="step-2-create-the-image-in-the-azure-portal"></a>2. adım: Azure portalında görüntü oluşturma
1. İçinde [portal](https://portal.azure.com/), tıklatın **kaynak oluşturma** bir görüntü oluşturmak için görev çubuğunda. Ardından, gereksinimlerinize göre Linux görüntü seçin. Aşağıdaki örnek, Ubuntu 14.04 görüntü kullanır.
![Yeni düğmesini gösteren portalı ekran görüntüsü][3]

2. İçin **ana bilgisayar adı**, siz ve Internet istemcileri bu sanal makine erişmek için kullanacağı URL adını belirtin. DNS adı, örneğin, tomcatdemo son bölümü tanımlayın. Azure tomcatdemo.cloudapp.net sonra URL oluşturur.  

3. İçin **SSH kimlik doğrulama anahtarı**, PuTTYgen tarafından oluşturulan ortak anahtarı içeren publicKey.pem dosyasından anahtar değerini kopyalayın.  
![SSH kimlik doğrulama anahtarı kutusunda portalı][4]

4. Diğer ayarları gerektiği gibi yapılandırın ve ardından **oluşturma**.  

## <a name="phase-2-prepare-your-virtual-machine-for-tomcat7"></a>2. Aşama: sanal makineniz tomcat7'yi için hazırlayın.
Bu aşamada, Tomcat trafiği için bir uç nokta yapılandırın ve ardından, yeni bir sanal makineye bağlanın.

### <a name="step-1-open-the-http-port-to-allow-web-access"></a>1. adım: web erişime izin vermek için HTTP bağlantı noktasını açın
Azure uç noktaları ile birlikte bir ortak ve özel bağlantı noktası TCP veya UDP protokolünü oluşur. Özel bağlantı noktası, hizmet sanal makine üzerinde dinleme bağlantı noktasıdır. Genel bağlantı noktası, Azure bulut hizmeti için dışarıdan gelen, Internet tabanlı trafiğini dinlemediğinden bağlantı noktasıdır.  

TCP bağlantı noktası 8080 dinlemek için Tomcat kullanan varsayılan bağlantı noktası numarasıdır. Bu bağlantı noktası ile Azure bir uç nokta açıldıysa, sizin ve diğer Internet istemcileri Tomcat sayfalarına erişebilirsiniz.  

1. Portalı'nda tıklatın **Gözat** > **sanal makineleri**ve ardından, oluşturduğunuz sanal makineye tıklayın.  
   ![Sanal makineler dizininin ekran görüntüsü][5]
2. Sanal makinenize bir uç nokta eklemek için tıklatın **uç noktaları** kutusu.
   ![Uç noktaları kutusunu gösteren ekran görüntüsü][6]
3. **Ekle**'ye tıklayın.  

   1. Uç nokta için bir uç adı **Endpoint**ve 80 enter **genel bağlantı noktası**.  

      80 ayarlarsanız bağlantı noktası numarasını Tomcat erişmek için kullanılan URL'yi içerecek şekilde gerekmez. Örneğin, http://tomcatdemo.cloudapp.net.    

      81 gibi başka bir değere ayarlarsanız URL'ye Tomcat erişmek için bağlantı noktası numarası eklemeniz gerekir. Örneğin, http://tomcatdemo.cloudapp.net:81/.
   2. İçinde 8080 girin **özel bağlantı noktası**. Varsayılan olarak, TCP bağlantı noktası 8080 Tomcat dinler. Varsayılan değiştirdiyseniz dinleme bağlantı noktası, Tomcat, güncelleştirmeniz gerekir **özel bağlantı noktası** Tomcat aynı dinleme bağlantı noktası olmalıdır.  
      ![Ekran görüntüsü, Ekle komutu, genel bağlantı noktası ve özel bağlantı noktası gösteren kullanıcı Arabirimi][7]
4. Tıklatın **Tamam** , sanal makine uç noktası eklemek için.

### <a name="step-2-connect-to-the-image-you-created"></a>2. adım: oluşturduğunuz görüntünün Bağlan
Sanal makinenize bağlanmak için herhangi bir SSH aracı seçebilirsiniz. Bu örnekte, PuTTY kullanın.  

1. Sanal makineniz DNS adını portaldan alın.
    1. Tıklatın **Gözat** > **sanal makineleri**.
    2. Sanal makinenin adını seçin ve ardından **özellikleri**.
    3. İçinde **özellikleri** döşeme, konum **etki alanı adı** DNS adını almak için kutusu.  

2. SSH bağlantılarından için bağlantı noktası numarasını almak **SSH** kutusu.  
![SSH bağlantı bağlantı noktası numarasını gösteren ekran görüntüsü][8]

3. Karşıdan [PuTTY](http://www.putty.org/).  

4. İndirdikten sonra yürütülebilir dosya Putty.exe'ı tıklatın. PuTTY yapılandırması, ana bilgisayar adıyla temel seçenekleri yapılandırın ve bağlantı noktası, sanal makinenize özelliklerinden elde sayısını.   
![PuTTY yapılandırması ana bilgisayar adı ve bağlantı noktası seçeneklerini gösteren ekran görüntüsü][9]

5. Sol bölmede **bağlantı** > **SSH** > **Auth**ve ardından **Gözat** privateKey.ppk dosyanın konumunu belirtmek için. PrivateKey.ppk dosya PuTTYgen tarafından daha önce içinde oluşturulan özel anahtarı içeren "1. Aşama: görüntü oluşturma" bölümünde bu makalenin.  
![Bağlantı dizini hiyerarşi ve Gözat düğmesini gösteren ekran görüntüsü][10]

6. Tıklatın **açık**. Bir ileti kutusu tarafından uyarı. DNS adı yapılandırdıysanız ve bağlantı noktası numarası doğru değilse, tıklatın **Evet**.
![Bildirim gösteren ekran görüntüsü][11]

7. Kullanıcı adınızı girmeniz istenir.  
![Kullanıcı adı girin yeri gösteren ekran görüntüsü][12]

8. Sanal makine oluşturmak için kullanılan kullanıcı adı girin "1. Aşama: görüntü oluşturma" bölümünde bu makalenin. Aşağıdakine benzer bir şey görürsünüz:  
![Kimlik doğrulama onay gösteren ekran görüntüsü][13]

## <a name="phase-3-install-software"></a>3. Aşama: Yazılımı yükleme
Bu aşamada, Java Çalışma zamanı ortamı, tomcat7'yi ve diğer tomcat7'yi bileşenlerini yükler.  

### <a name="java-runtime-environment"></a>Java Çalışma zamanı ortamı
Tomcat Java'da yazılmış olmalıdır. Java geliştirme setleri (JDKs), OpenJDK ve Oracle JDK iki tür vardır. İstediğinizi seçebilirsiniz.  

> [!NOTE]
> Her iki JDKs neredeyse aynı kodu sınıfları için Java API sahip, ancak sanal makine için kod farklıdır. OpenJDK Oracle JDK kapalı yorumlar kullanmak eğilimindedir açık kitaplıkları, kullanım eğilimindedir. Oracle JDK sahip daha sınıfları ve bazı düzeltilen hatalar ve Oracle JDK OpenJDK daha fazla kararlı.

#### <a name="install-openjdk"></a>Install OpenJDK  

OpenJDK indirmek için aşağıdaki komutu kullanın.   

    sudo apt-get update  
    sudo apt-get install openjdk-7-jre  


* JDK dosyaları içerecek bir dizin oluşturmak için:  

        sudo mkdir /usr/lib/jvm  
* / Usr/lib/jvm/dizine JDK dosyalarını ayıklamak için:  

        sudo tar -zxf jdk-8u5-linux-x64.tar.gz  -C /usr/lib/jvm/

#### <a name="install-oracle-jdk"></a>Install Oracle JDK


Oracle JDK Oracle Web sitesinden karşıdan yüklemek için aşağıdaki komutu kullanın.  

     wget --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u5-b13/jdk-8u5-linux-x64.tar.gz  
* JDK dosyaları içerecek bir dizin oluşturmak için:  

        sudo mkdir /usr/lib/jvm  
* / Usr/lib/jvm/dizine JDK dosyalarını ayıklamak için:  

        sudo tar -zxf jdk-8u5-linux-x64.tar.gz  -C /usr/lib/jvm/  
* Oracle JDK varsayılan Java sanal makinesi olarak ayarlamak için:  

        sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.8.0_05/bin/java 100  

        sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.8.0_05/bin/javac 100  

#### <a name="confirm-that-java-installation-is-successful"></a>Java yüklemenin başarılı olduğunu onaylayın
Java Çalışma zamanı ortamı düzgün yüklenmediyse test etmek için aşağıdaki gibi bir komutu kullanabilirsiniz:  

    java -version  

OpenJDK yüklediyseniz, aşağıdaki gibi bir ileti görürsünüz: ![başarılı OpenJDK yükleme iletisi][14]

Oracle JDK yüklediyseniz, aşağıdaki gibi bir ileti görürsünüz: ![başarılı Oracle JDK yükleme iletisi][15]

### <a name="install-tomcat7"></a>Tomcat7'yi yükleme
Tomcat7'yi yüklemek için aşağıdaki komutu kullanın.  

    sudo apt-get install tomcat7  

Tomcat7'yi kullanmıyorsanız, bu komutun uygun varyasyonunu kullanın.  

#### <a name="confirm-that-tomcat7-installation-is-successful"></a>Tomcat7'yi yükleme işleminin başarılı olduğunu doğrulayın
Tomcat7'yi başarılı olup olmadığını denetleyin, Tomcat sunucunuzun DNS adına göz atın. Bu makaledeki örnek URL'dir http://tomcatexample.cloudapp.net/. Aşağıdaki gibi bir ileti görürseniz, tomcat7'yi yüklendiğinden emin olun.
![Başarılı tomcat7'yi yükleme iletisi][16]

### <a name="install-other-tomcat7-components"></a>Diğer tomcat7'yi bileşenlerini yükle
Yükleyebileceğiniz isteğe bağlı diğer Tomcat bileşenleri vardır.  

Kullanım **sudo önbellek apt arama tomcat7'yi** kullanılabilir bileşenlerinin tümünü görmek için komutu. Bazı yararlı bileşenleri yüklemek için aşağıdaki komutları kullanın.  

    sudo apt-get install tomcat7-admin      #admin web applications

    sudo apt-get install tomcat7-user         #tools to create user instances  

## <a name="phase-4-configure-tomcat7"></a>4. Aşama: Tomcat7'yi yapılandırma
Bu aşamada, Tomcat yönetin.

### <a name="start-and-stop-tomcat7"></a>Tomcat7'yi durdurup başlatın
Tomcat7'yi sunucusu yüklediğinizde otomatik olarak başlar. Ayrıca aşağıdaki komutla başlatabilirsiniz:   

    sudo /etc/init.d/tomcat7 start

Tomcat7'yi durdurmak için:

    sudo /etc/init.d/tomcat7 stop

Tomcat7'yi durumunu görüntülemek için:

    sudo /etc/init.d/tomcat7 status

Tomcat hizmetlerini yeniden başlatmak için: 

    sudo /etc/init.d/tomcat7 restart

### <a name="tomcat7-administration"></a>Tomcat7'yi Yönetim
Yönetici kimlik bilgilerinizi ayarlamak için Tomcat kullanıcı yapılandırma dosyası düzenleyebilirsiniz. Aşağıdaki komutu kullanın:  

    sudo vi  /etc/tomcat7/tomcat-users.xml   

Örnek aşağıda verilmiştir:  
![Sudo VI komut çıktısı gösteren ekran görüntüsü][17]  

> [!NOTE]
> Yönetici kullanıcı adı için güçlü bir parola oluşturun.  

Bu dosyasını düzenledikten sonra değişikliklerin etkili olmasını sağlamak için aşağıdaki komutu hizmetleriyle tomcat7'yi yeniden başlatmanız gerekir:  

    sudo /etc/init.d/tomcat7 restart  

Tarayıcınızı açın ve girin **http://<your tomcat server DNS name>/Yöneticisi/html** URL. Bu makaledeki örnek için URL'nin olduğundan http://tomcatexample.cloudapp.net/manager/html.  

Bağlandıktan sonra aşağıdakine benzer bir şey görmeniz gerekir:  
![Tomcat Web Uygulama Yöneticisi'nin ekran görüntüsü][18]

## <a name="common-issues"></a>Genel sorunlar
### <a name="cant-access-the-virtual-machine-with-tomcat-and-moodle-from-the-internet"></a>Sanal makine Tomcat ve Moodle ile Internet'ten erişemiyor.
#### <a name="symptom"></a>Belirti  
  Tomcat çalışıyor ancak tarayıcınızla Tomcat varsayılan sayfa göremez.
#### <a name="possible-root-cause"></a>Olası temel nedeni   

  * Tomcat dinleme bağlantı noktası, sanal makinenin uç noktasının Tomcat trafiği için özel bağlantı noktası ile aynı değil.  

     Genel bağlantı noktası ve özel bağlantı noktası bitiş noktası ayarlarını denetleyin ve Tomcat aynı bağlantı noktasını dinlemek özel bağlantı noktası olduğundan emin olun. Bkz: "1. Aşama: görüntü oluşturma" uç noktaları, sanal makine için yapılandırma hakkında yönergeler için bu makalenin.  

     Tomcat dinleme bağlantı noktasını belirlemek için /etc/httpd/conf/httpd.conf (Red Hat sürüm) veya /etc/tomcat7/server.xml (Debian sürüm) açın. Varsayılan olarak, Tomcat dinleme bağlantı noktası 8080'dir. Örnek aşağıda verilmiştir:  

        <Connector port="8080" protocol="HTTP/1.1"  connectionTimeout="20000"   URIEncoding="UTF-8"            redirectPort="8443" />  

     Debian ve Ubuntu ve varsayılan bağlantı noktası, Tomcat dinleme (örneğin 8081) değiştirmek istediğiniz gibi bir sanal makine kullanıyorsanız, işletim sistemi için bağlantı noktası açmanız gerekir. İlk olarak, profil açın:  

        sudo vi /etc/default/tomcat7  

     Ardından son satırı açıklamadan çıkarın ve "Evet" için "Hayır" değiştirin.  

        AUTHBIND=yes
  2. Güvenlik Duvarı, dinleme bağlantı noktası Tomcat devre dışı bıraktı.

     Yalnızca yerel ana Tomcat varsayılan sayfasından da görebilirsiniz. Sorunu en çok Tomcat tarafından dinlenen, bağlantı noktası güvenlik duvarı tarafından engellenir olasıdır. Web sayfasına göz atmak için w3m aracını kullanabilirsiniz. Aşağıdaki komutları w3m yükleyin ve Tomcat varsayılan sayfasına göz atın:  


        sudo yum yükleme w3m w3m-img


        w3m http://localhost:8080  
#### <a name="solution"></a>Çözüm

  * Tomcat dinleme bağlantı noktası bitiş noktasının trafiği sanal makine için özel bağlantı noktası ile aynı değil, Tomcat aynı bağlantı noktasını dinlemek olması için özel bağlantı noktası değiştirmeniz.   
  2. Güvenlik Duvarı/iptables tarafından sorunu neden olursa, /etc/sysconfig/iptables için aşağıdaki satırları ekleyin. İkinci satır yalnızca https trafiği için gereklidir:  

      -A -p tcp -m tcp--dport 80 -j kabul giriş

      -A -p tcp -m tcp--dport 443 -j kabul giriş  

     > [!IMPORTANT]
     > Önceki satırları genel olarak, aşağıdaki gibi erişimi kısıtlar satırları yukarıda konumlandırıldığı emin olun: - bir giriş -j--Reddet-with ICMP konak yasaklanmış REDDET



İptables yeniden yüklemek için aşağıdaki komutu çalıştırın:

    service iptables restart

Bu CentOS 6.3 test edilmiştir.

### <a name="permission-denied-when-you-upload-project-files-to-varlibtomcat7webapps"></a>/Var/lib/tomcat7/webapps için proje dosyalarını karşıya yüklediğinizde reddedildi /
#### <a name="symptom"></a>Belirti
  SFTP istemci (örneğin, FileZilla) kullandığınızda, sanal makineye bağlanın ve /var/lib/tomcat7/webapps için gezinmek için/sitenizi yayımlamak için size bir hata iletisi aşağıdakine benzer alın:  

     status:    Listing directory /var/lib/tomcat7/webapps
     Command:    put "C:\Users\liang\Desktop\info.jsp" "info.jsp"
     Error:    /var/lib/tomcat7/webapps/info.jsp: open for write: permission denied
     Error:    File transfer failed
#### <a name="possible-root-cause"></a>Olası temel nedeni
  /Var/lib/tomcat7/webapps klasöre erişim izni var.  
#### <a name="solution"></a>Çözüm  
  Kök hesabından izin edinmeniz gerekir. Bu klasörü sahipliğini kökünden makine sağladığında kullandığınız kullanıcı adı için değiştirebilirsiniz. Azureuser hesap adı ile örnek aşağıda verilmiştir:  

     sudo chown azureuser -R /var/lib/tomcat7/webapps

  Bir dizin içinde tüm dosyalar için izinler çok uygulamak için -R seçeneğini kullanın.  

  Bu komut, dizinler için de çalışır. -R seçeneği tüm dosyaları ve dizinleri dizini içinde izinlerini değiştirir. Örnek aşağıda verilmiştir:  

     sudo chown -R username:group directory  

  Bu komut tüm dosya ve dizin olan dizinleri için sahiplik (kullanıcı ve grup) değiştirir.  

  Aşağıdaki komut yalnızca klasörü dizin izni değiştirir. Dosya ve klasörleri dizini içinde değiştirilmez.  

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
