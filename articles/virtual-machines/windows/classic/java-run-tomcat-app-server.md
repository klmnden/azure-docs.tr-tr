---
title: Java uygulama sunucusu bir Klasik Azure VM üzerinde çalışan
description: Bu öğretici, Klasik dağıtım modeli kullanılarak oluşturulmuş kaynaklarını kullanır ve bir Windows sanal makine oluşturma ve Apache Tomcat uygulama sunucusu çalıştırmak için yapılandırın gösterir.
services: virtual-machines-windows
documentationcenter: java
author: rmcmurray
manager: mbaldwin
tags: azure-service-management
ms.assetid: d627aa09-f7d6-4239-8110-f8fc5111b939
ms.service: virtual-machines-windows
ms.workload: web
ms.tgt_pltfrm: vm-windows
ms.devlang: Java
ms.topic: article
ms.date: 04/11/2018
ms.author: robmcm
ms.openlocfilehash: e13228a707e7dae4a4c2505154d01215c40b4716
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
ms.locfileid: "31528037"
---
# <a name="how-to-run-a-java-application-server-on-a-virtual-machine-created-with-the-classic-deployment-model"></a>Klasik dağıtım modeliyle oluşturulan bir sanal makinede Java uygulama sunucusu çalıştırma
> [!IMPORTANT]
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Resource Manager şablonu webapp Java 8 ve Tomcat ile dağıtmak bkz: [burada](https://azure.microsoft.com/documentation/templates/201-web-app-java-tomcat/).
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

Azure ile sunucu yetenekleri sağlamak için bir sanal makine kullanabilirsiniz. Örnek olarak, Azure üzerinde çalışan bir sanal makine Apache Tomcat gibi bir Java uygulama sunucusu barındırmak için yapılandırılabilir.

Bu kılavuzu tamamladıktan sonra Azure üzerinde çalışan bir sanal makine oluşturun ve bunu bir Java uygulama sunucusu çalıştırmak için yapılandırmak üzere nasıl anlaşılması gerekir. Bilgi işlem ve aşağıdaki görevleri gerçekleştirin:

* Bir Java Geliştirme Seti (zaten yüklü JDK) sahip olan bir sanal makine oluşturma
* Sanal makinenize uzaktan oturum açma.
* Bir Java uygulama sunucusu--Apache Tomcat--sanal makinenizde nasıl yüklenir.
* Sanal makineniz için bir uç nokta oluşturma
* Güvenlik Duvarı'nda, uygulama sunucusu için bir bağlantı noktasını açmak nasıl.

Bir sanal makinede çalışan Tomcat yükleme tamamlandı sonuçlanır.

![Apache Tomcat çalıştıran sanal makine][virtual_machine_tomcat]

[!INCLUDE [create-account-and-vms-note](../../../../includes/create-account-and-vms-note.md)]

## <a name="to-create-a-virtual-machine"></a>Sanal makine oluşturmak için
1. [Azure Portal](https://portal.azure.com) oturum açın.  
2. ' I tıklatın **kaynak oluşturma**, tıklatın **işlem**, ardından **tümünü görmek** içinde **öne çıkan uygulamalar**.
3. Tıklatın **JDK**, tıklatın **JDK 8** içinde **JDK** bölmesi.  
   Sanal makine görüntüleri destekleyen **JDK 6** ve **JDK 7** JDK 8'de çalıştırmak hazır olmayan eski uygulamalarınız varsa kullanılabilir.
4. JDK 8 bölmesinde seçin **Klasik**, ardından **oluşturma**.
5. İçinde **Temelleri** dikey penceresinde:
   1. Sanal makine için bir ad belirtin.
   2. Bir yönetici adı **kullanıcı adı** alan. Bu ad ve sonraki alanda izleyen ilişkili parolayı unutmayın. Sanal makineye uzaktan oturum açtığınızda, bunları gerekir.
   3. Bir parolayı girin **yeni parola** alan ve girin **parolayı onayla** alan. Bu parola yönetici hesabıdır.
   4. Uygun seçin **abonelik**.
   5. İçin **kaynak grubu**, tıklatın **Yeni Oluştur** ve yeni bir kaynak grubu adını girin. Veya tıklatın **var olanı kullan** ve kullanılabilir kaynak gruplarını birini seçin.
   6. Sanal makinenin bulunduğu, gibi bir konum seçin **Orta Güney ABD**.
6. **İleri**’ye tıklayın.
7. İçinde **sanal makine görüntü boyutu** dikey penceresinde, select **A1 standart** veya başka bir uygun görüntü.
8. **Seç**'e tıklayın.

9. İçinde **ayarları** dikey penceresinde tıklatın **Tamam**. Azure tarafından sağlanan varsayılan değerleri kullanabilirsiniz.  
10. İçinde **Özet** dikey penceresinde tıklatın **Tamam**.

## <a name="to-remotely-sign-in-to-your-virtual-machine"></a>Sanal makinenize uzaktan oturum açmak için
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Tıklatın **sanal makineleri (Klasik)**. Gerekirse, tıklatın **daha fazla hizmet** hizmeti kategoriler altında sol alt köşedeki adresindeki. **Sanal makineleri (Klasik)** girişi listelenir **işlem** grubu.
3. Oturum açmak için istediğiniz sanal makinenin adına tıklayın.
4. Sanal makine başlatıldıktan sonra bölmenin üstündeki menü bağlantılara izin verir.
5. **Bağlan**'a tıklayın.
6. Sanal makineye bağlanmak için gereken şekilde yanıtlamasına. Genellikle, kaydedin veya bağlantı ayrıntılarını içeren .rdp dosyasını açın. Url: bağlantı noktası .rdp dosyasının ilk satırı son parçası olarak kopyalayın ve bir uzak oturum açma uygulamasında yapıştırmak zorunda kalabilirsiniz.

## <a name="to-install-a-java-application-server-on-your-virtual-machine"></a>Sanal makinenizde bir Java uygulama sunucusu yüklemek için
Java uygulama sunucusu sanal makinenize kopyalayabilirsiniz veya bir yükleyici aracılığıyla bir Java uygulama sunucusu yükleyebilirsiniz.

Bu öğretici Tomcat Java uygulama sunucusu olarak yüklemek için kullanır.

1. Sanal makinenize oturum açtığında, bir tarayıcı oturumu açın [Apache Tomcat](http://tomcat.apache.org/download-80.cgi).
2. Bağlantı için çift **32-bit/64-bit Windows hizmeti yükleyicisini**. Bu yöntemi kullanarak, Tomcat bir Windows hizmeti olarak yükler.
3. İstendiğinde, yükleyiciyi çalıştırmak seçin.
4. İçinde **Apache Tomcat Kurulum** Sihirbazı, Tomcat yüklemek için istemleri izleyin. Bu öğreticinin amaçları doğrultusunda, varsayılan değerleri kabul ederek uygundur. Ulaştığınızda **Apache Tomcat Kurulum Sihirbazı Tamamlanıyor** iletişim kutusu, isteğe bağlı olarak kontrol edebilirsiniz **çalıştırmak Apache Tomcat** Tomcat şimdi başlatmak için. Tıklatın **son** Tomcat Kurulum işlemini tamamlamak için.

## <a name="to-start-tomcat"></a>Tomcat başlatmak için

Sanal makinenizde bir komut istemi açmak ve komutunu çalıştırarak Tomcat el ile başlatabilirsiniz **net&nbsp;Başlat&nbsp;Tomcat8**.

Tomcat çalışmaya başladıktan sonra URL'yi girerek Tomcat erişebilirsiniz <http://localhost:8080> sanal makinenin tarayıcıda.

Dış makinelerden çalıştıran Tomcat görmek için bir uç noktası oluşturma ve bir bağlantı noktası açmanız gerekir.

## <a name="to-create-an-endpoint-for-your-virtual-machine"></a>Sanal makineniz için bir uç nokta oluşturmak için
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Tıklatın **sanal makineleri (Klasik)**.
3. Java uygulama sunucusu çalıştıran sanal makine adına tıklayın.
4. **Uç Noktalar**’a tıklayın.
5. **Ekle**'ye tıklayın.
6. İçinde **uç nokta ekleme** iletişim kutusunda:
   1. Uç nokta için bir ad belirtin; Örneğin, **HttpIn**.
   2. Seçin **TCP** protokolü için.
   3. Belirtin **80** genel bağlantı noktası için.
   4. Belirtin **8080** özel bağlantı noktası için.
   5. Seçin **devre dışı** kayan IP adresi.
   6. Erişim denetimi listesi olduğu gibi bırakın.
   7. Tıklatın **Tamam** düğmesine iletişim kutusunu kapatmak ve uç noktası oluşturur.

## <a name="to-open-a-port-in-the-firewall-for-your-virtual-machine"></a>Güvenlik Duvarı'nda, sanal makine için bir bağlantı noktasını açmak için
1. Sanal makinenize oturum açın.
2. Tıklatın **Windows Başlangıç**.
3. Tıklatın **Denetim Masası**.
4. Tıklatın **sistem ve güvenlik**, tıklatın **Windows Güvenlik Duvarı**ve ardından **Gelişmiş ayarları**.
5. Tıklatın **gelen kuralları**ve ardından **yeni kural**.  
   ![Yeni gelen kuralı][NewIBRule]
6. İçin **kural türü**seçin **bağlantı noktası**ve ardından **sonraki**.  
   ![Yeni gelen kuralı bağlantı noktası][NewRulePort]
7. Üzerinde **protokol ve bağlantı noktaları** ekran, select **TCP**, belirtin **8080** olarak **belirli yerel bağlantı noktası**ve ardından **sonraki**.  
  ![Yeni gelen kuralı ][NewRuleProtocol]
8. Üzerinde **eylem** ekran, select **bağlantıya izin**ve ardından **sonraki**.
   ![Yeni gelen kuralı eylemi][NewRuleAction]
9. Üzerinde **profil** ekranında, emin **etki alanı**, **özel**, ve **ortak** seçilir ve ardından **sonraki**.
   ![Yeni gelen kuralı profili][NewRuleProfile]
10. Üzerinde **adı** ekranında, kural için bir ad belirtin **HttpIn** (Kural adı gerekli değildir ancak uç nokta adı eşleşecek şekilde) ve ardından **son**.  
    ![Yeni gelen kuralı adı][NewRuleName]

Bu noktada, Tomcat Web sitenizi dış bir tarayıcıdan görüntülenebilir olması gerekir. Tarayıcının adres penceresinde bir URL biçiminde yazın **http://*,\_DNS\_adı*. cloudapp.net**, burada ***,\_DNS\_adı*** sanal makineyi oluşturduğunuzda belirttiğiniz DNS adı.

## <a name="application-lifecycle-considerations"></a>Uygulama yaşam döngüsü hakkında dikkat edilecek noktalar
* Kendi web uygulama Arşivi (WAR) oluşturun ve ona ekleyen **webapps** klasör. Örneğin, bir temel Java hizmet sayfa (JSP) dinamik web projesi oluşturun ve bir WAR dosyası olarak dışarı aktarma. Ardından, Apache Tomcat WAR kopyalama **webapps** sanal makinede bir tarayıcıda ardından çalıştırın, klasör.
* Tomcat hizmeti yüklü olduğunda, varsayılan olarak el ile başlatmak için ayarlanır. Hizmetler ek bileşenini kullanarak otomatik olarak başlayacak şekilde geçiş yapabilirsiniz. Hizmetler ek bileşenini başlatın tıklayarak **Windows Başlat**, **Yönetimsel Araçlar**ve ardından **Hizmetleri**. Çift **Apache Tomcat** ayarlayın ve hizmeti **başlangıç türü** için **otomatik**.

    ![Hizmeti otomatik olarak başlatılacak şekilde ayarlama][service_automatic_startup]

    Otomatik olarak Başlat Tomcat sahip olmanın avantaj (örneğin, yeniden başlatma gerektiren yazılım güncelleştirmeleri yüklendikten sonra) sanal makine yeniden başlatıldığında çalışmaya başladıktan emin olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar
Java uygulamalarınızla dahil etmek istediğiniz diğer hizmetler (örneğin, Azure Storage, hizmet veri yolu ve SQL veritabanı) hakkında bilgi edinebilirsiniz. Kullanılabilir bilgi görüntülemek [Java Geliştirici Merkezi](https://azure.microsoft.com/develop/java/).

[virtual_machine_tomcat]:media/java-run-tomcat-app-server/WA_VirtualMachineRunningApacheTomcat.png

[service_automatic_startup]:media/java-run-tomcat-app-server/WA_TomcatServiceAutomaticStart.png









[NewIBRule]:media/java-run-tomcat-app-server/NewInboundRule.png
[NewRulePort]:media/java-run-tomcat-app-server/NewRulePort.png
[NewRuleProtocol]:media/java-run-tomcat-app-server/NewRuleProtocol.png
[NewRuleAction]:media/java-run-tomcat-app-server/NewRuleAction.png
[NewRuleName]:media/java-run-tomcat-app-server/NewRuleName.png
[NewRuleProfile]:media/java-run-tomcat-app-server/NewRuleProfile.png


<!-- Deleted from the "To create an ednpoint for your virtual mache" 3/17/2017,
     to use the new portal.
6. In the **Add endpoint** dialog box, ensure **Add standalone endpoint** is selected, and then click **Next**.
7. In the **New endpoint details** dialog box:
-->
