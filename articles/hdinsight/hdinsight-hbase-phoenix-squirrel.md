---
title: "Apache kullanın ve Windows tabanlı Azure Hdınsight ile SQuirreL | Microsoft Docs"
description: "Hdınsight'ta Apache Phoenix kullanmayı ve yüklemek ve hdınsight'ta HBase kümesi bağlanmak için istasyonunuzda SQuirreL yapılandırmak nasıl öğrenin."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 1a756e98-75c1-44cd-a178-c5119683b7b7
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 04392b535965edd785bbb66a52eb6b41b768553e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-apache-phoenix-and-squirrel-with-windows-based-hbase-clusters-in-hdinsight"></a>Hdınsight'ta Windows tabanlı HBase kümeleriyle Apache ve SQuirreL kullanma
Nasıl kullanacağınızı öğrenin [Apache Phoenix](http://phoenix.apache.org/) ve yükleme ve SQuirreL istasyonunuzu hdınsight'ta HBase kümesi bağlanmak için yapılandırın. Phoenix hakkında daha fazla bilgi için bkz: [Phoenix 15 dakika veya daha az](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html). Phoenix dilbilgisi için bkz: [Phoenix Dilbilgisi](http://phoenix.apache.org/language/index.html).

> [!NOTE]
> Phoenix sürümünü hdınsight'ta bilgi için [Hdınsight tarafından sağlanan Hadoop küme sürümlerindeki yenilikler nelerdir?](hdinsight-component-versioning.md).
>

> [!IMPORTANT]
> Bu belgede yer alan adımlar, yalnızca Windows tabanlı Hdınsight kümeleri için geçerlidir. Hdınsight yalnızca Windows'da Hdınsight 3.4 ' düşük sürümleri için kullanılabilir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement). Phoenix Linux tabanlı Hdınsight kullanma hakkında daha fazla bilgi için bkz: [Hdınsight'ta Linux tabanlı HBase ile kullanım Apache Phoenix kümeleri](hdinsight-hbase-phoenix-squirrel-linux.md).
>



## <a name="use-sqlline"></a>SQLLine kullanın
[SQLLine](http://sqlline.sourceforge.net/) SQL yürütülecek bir komut satırı yardımcı programıdır.

### <a name="prerequisites"></a>Ön koşullar
SQLLine kullanmadan önce aşağıdakilere sahip olmanız gerekir:

* **Hdınsight'ta HBase kümesi**. Küme hazırlama HBase hakkında bilgi için bkz: [hdınsight'ta Apache HBase kullanmaya başlama][hdinsight-hbase-get-started].
* **Uzak Masaüstü Protokolü aracılığıyla HBase kümesi bağlanmak**. Yönergeler için bkz: [yönetmek Hdınsight'ta Hadoop kümeleri Azure Klasik Portalı'nı kullanarak][hdinsight-manage-portal].

**Ana bilgisayar adı bulunamıyor**

1. Açık **Hadoop komut satırı** masaüstünden.
2. DNS soneki almak için aşağıdaki komutu çalıştırın:

        ipconfig

    Yazma **bağlantıya özgü DNS soneki**. Örneğin, *myhbasecluster.f5.internal.cloudapp.net*. Bir HBase kümesi bağlandığınızda, FQDN kullanarak Zookeepers birine bağlanabilmeleri gerekir. Her Hdınsight kümesi 3 Zookeepers sahiptir. Bunlar *zookeeper0*, *zookeeper1*, ve *zookeeper2*. FQDN şöyle olacaktır *zookeeper2.myhbasecluster.f5.internal.cloudapp.net*.

**SQLLine kullanmak için**

1. Açık **Hadoop komut satırı** masaüstünden.
2. SQLLine açmak için aşağıdaki komutları çalıştırın:

        cd %phoenix_home%\bin
        sqlline.py [The FQDN of one of the Zookeepers]

    ![Hdınsight hbase phoenix sqlline][hdinsight-hbase-phoenix-sqlline]

    Aşağıdaki örnekte kullanılan komutlar:

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));

        !tables

        UPSERT INTO Company VALUES(1, 'Microsoft');

        SELECT * FROM Company;

Daha fazla bilgi için bkz: [SQLLine el ile](http://sqlline.sourceforge.net/#manual) ve [Phoenix Dilbilgisi](http://phoenix.apache.org/language/index.html).

## <a name="use-squirrel"></a>SQuirreL kullanma
[SQL istemci sQuirreL](http://squirrel-sql.sourceforge.net/) JDBC uyumlu bir veritabanına yapısını görüntüleme, tablolardaki verileri göz atın, SQL komutlarını vb. sorun sağlayacak bir grafik Java programdır. Hdınsight üzerinde Apache Phoenix bağlanmak için kullanılabilir.

Bu bölümde yükleme ve hdınsight'ta HBase kümesi VPN aracılığıyla bağlanmak için istasyonunuzda SQuirreL yapılandırma gösterilmektedir.

### <a name="prerequisites"></a>Ön koşullar
Yordamları izlemeden önce aşağıdakilerin olması gerekir:

* Azure sanal ağını DNS sanal makine ile dağıtılan bir HBase kümesi.  Yönergeler için bkz: [oluşturma HBase kümeleri Azure sanal ağı][hdinsight-hbase-provision-vnet].

* HBase kümesi küme bağlantıya özgü DNS soneki alın. Bu, küme halinde RDP almak ve ipconfig komutunu çalıştırmak için.  DNS soneki benzer:

        myhbase.b7.internal.cloudapp.net
* İndirme ve yükleme [Microsoft Visual Studio Express için Windows Masaüstü](https://www.visualstudio.com/products/visual-studio-express-vs.aspx) istasyonunuzda. Makecert, bir sertifika oluşturmak için paketten gerekir.  
* İndirme ve yükleme [Java Çalışma zamanı ortamı](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html) istasyonunuzda.  SQL istemci sürüm 3.0 ve daha yüksek sQuirreL JRE 1.6 veya sonraki sürümünü gerektirir.  

### <a name="configure-a-point-to-site-vpn-connection-to-the-azure-virtual-network"></a>Azure sanal ağa noktadan siteye VPN bağlantısı yapılandırma
Bir noktadan siteye VPN bağlantısı yapılandırma söz konusu 3 adım vardır:

1. [Bir sanal ağ ve dinamik yönlendirme ağ geçidi yapılandırma](#Configure-a-virtual-network-and-a-dynamic-routing-gateway)
2. [Sertifikalarınızı oluşturma](#Create-your-certificates)
3. [VPN istemcinizi yapılandırma](#Configure-your-VPN-client)

Bkz: [bir Azure sanal ağa noktadan siteye VPN bağlantısı yapılandırma](../vpn-gateway/vpn-gateway-point-to-site-create.md) daha fazla bilgi için.

#### <a name="configure-a-virtual-network-and-a-dynamic-routing-gateway"></a>Bir sanal ağ ve dinamik yönlendirme ağ geçidi yapılandırma
Bir Azure sanal ağındaki bir HBase kümesi sağlanan güvence altına almak (Bu bölümün önkoşullara bakın). Sonraki adım, bir noktadan siteye bağlantı yapılandırmaktır.

**Noktadan siteye bağlantı yapılandırmak için**

1. Oturum [Klasik Azure portalı][azure-portal].
2. Sol bölmede, tıklatın **ağlar**.
3. Oluşturduğunuz sanal ağa tıklayın (bkz [sağlama HBase kümeleri Azure sanal ağı][hdinsight-hbase-provision-vnet]).
4. Tıklatın **yapılandırma** üstten.
5. İçinde **noktadan siteye bağlantı** bölümünde, select **noktadan siteye bağlantı yapılandırma**.
6. Yapılandırma **başlangıç IP** ve **CIDR** içinden VPN istemcilerinizin bağlıyken bir IP adresi alacağı IP adresi aralığı belirtmek için. Aralığın, şirket içi ağınız ve Azure sanal ağı için bağlanırsınız bulunan aralıklardan herhangi biriyle çakışamaz. Örneğin. sanal ağ için 10.0.0.0/20 seçtiyseniz, istemci adres alanı için 10.1.0.0/24 seçebilirsiniz. Bkz: [noktadan siteye bağlantı] [ vnet-point-to-site-connectivity] daha fazla bilgi için.
7. Sanal ağ adres alanları bölümünde tıklayın **ağ geçidi alt ağı eklemek**.
8. Tıklatın **KAYDETMEK** sayfanın üzerinde.
9. Tıklatın **Evet** Değişikliği onaylamak için. Sistem sonraki yordama devam etmeden önce değişikliği yapmadan tamamlanana kadar bekleyin.

**Dinamik yönlendirme ağ geçidi oluşturmak için**

1. Azure Klasik portalından tıklatın **PANO** sayfasının üstten.
2. Tıklatın **ağ geçidi Oluştur** sayfanın en altındaki.
3. Tıklatın **Evet** onaylamak için. Ağ geçidi oluşturulana kadar bekleyin.
4. Tıklatın **PANO** üstten.  Sanal ağ visual diyagramı görürsünüz:

    ![Azure sanal ağı noktadan siteye sanal diyagramı][img-vnet-diagram]

    Diyagram 0 istemci bağlantılarını gösterir. Sanal ağa bir bağlantı yaptıktan sonra bir sayı güncelleştirilir.

#### <a name="create-your-certificates"></a>Sertifikalarınızı oluşturma
Bir X.509 sertifikası oluşturmanın yollarından biri olan sertifika oluşturma ile birlikte gelen Aracı (makecert.exe) kullanarak [Microsoft Visual Studio Express için Windows Masaüstü](https://www.visualstudio.com/products/visual-studio-express-vs.aspx).

**Bir otomatik olarak imzalanan sertifika oluşturmak için**

1. İstasyonunuzdan, bir komut istemi penceresi açın.
2. Visual Studio Araçları klasörüne gidin.
3. Aşağıdaki komutu aşağıdaki örnekte oluşturur ve istasyonunuzda kişisel sertifika deposunda kök sertifikasını yükleyin ve daha sonra Azure Klasik Portalı'na yükleyeceksiniz karşılık gelen bir .cer dosyası oluşturabilir.

        makecert -sky exchange -r -n "CN=HBaseVnetVPNRootCertificate" -pe -a sha1 -len 2048 -ss My "C:\Users\JohnDole\Desktop\HBaseVNetVPNRootCertificate.cer"

    .Cer dosyasının, HBaseVnetVPNRootCertificate sertifika için kullanmak istediğiniz adı olduğu yer istediğiniz dizine geçin.

    Komut istemi kapatmayın.  Sonraki yordamda gerekir.

   > [!NOTE]
   > Kendisinden istemci sertifikaları oluşturulacak bir kök sertifikası oluşturduğunuzdan, bu sertifikayı ve özel anahtarını dışarı aktarıp ileride kurtarma amaçlı kullanabileceğiniz güvenli bir konuma kaydetmek isteyebilirsiniz.
   >
   >

**Bir istemci sertifikası oluşturmak için**

* Aynı komut istemini (Bu bilgisayarın kök sertifikanın oluşturulduğu bilgisayarda olması gerekir. İstemci sertifikası oluşturulmalıdır kök sertifikası), aşağıdaki komutu çalıştırın:

          makecert.exe -n "CN=HBaseVnetVPNClientCertificate" -pe -sky exchange -m 96 -ss My -in "HBaseVnetVPNRootCertificate" -is my -a sha1

    HBaseVnetVPNRootCertificate kök sertifika addır.  Bu kök sertifika adı eşleşmelidir.  

    Kök sertifikayı ve istemci sertifikası, bilgisayarınızdaki kişisel sertifika deposunda depolanır. Certmgr.msc doğrulamak için kullanın.

    ![Azure sanal ağı noktadan siteye VPN sertifikası][img-certificate]

    Sanal ağa bağlamak istediğiniz her bilgisayara bir istemci sertifikası yüklenmelidir. Sanal ağa bağlamak istediğiniz her bilgisayar için benzersiz istemci sertifikaları oluşturmanızı öneririz. İstemci sertifikaları vermek için certmgr.msc kullanın.

**Klasik Azure portalı kök sertifikayı karşıya yüklemek için**

1. Azure Klasik portalından tıklatın **ağ** soldaki.
2. HBase kümesi dağıtıldığı sanal ağ'ı tıklatın.
3. Tıklatın **SERTİFİKALARI** üstten.
4. Tıklatın **karşıya** Alttan ve son önce yordamda oluşturduğunuz kök sertifika dosyasını belirtin. Sertifika içeri kadar bekleyin.
5. Tıklatın **PANO** üstte.  Sanal diyagramı durumu gösterilir.

#### <a name="configure-your-vpn-client"></a>VPN istemcinizi yapılandırma
**İndirmek ve istemci VPN paketini yüklemek için**

1. Hızlı Bakış bölümünde, sanal ağ PANOSU sayfasından tıklayın **64-bit istemci VPN paketini indir** veya **32 bit istemci VPN paketini indir** , iş istasyonunuzu işletim sistemi tabanlı Sürüm.
2. Tıklatın **çalıştırmak** paketi yüklemek için.
3. Güvenlik isteminde tıklatın **daha fazla bilgi**ve ardından **yine de çalıştırmaya**.
4. Tıklatın **Evet** iki kez.

**VPN'ye bağlanmak için**

1. İş istasyonunuzu masaüstünde, görev çubuğunda ağ simgesine tıklayın. Bir VPN bağlantısı, sanal ağ adıyla göreceksiniz.
2. VPN bağlantısı adını tıklatın.
3. **Bağlan**'a tıklayın.

**VPN bağlantısı ve etki alanı ad çözümlemesini test etmek için**

* İş istasyonundan, bir komut istemi açın ve ping HBase kümenin DNS soneki verilen aşağıdaki adlarından birini myhbase.b7.internal.cloudapp.net ise:

        zookeeper0.myhbase.b7.internal.cloudapp.net
        zookeeper0.myhbase.b7.internal.cloudapp.net
        zookeeper0.myhbase.b7.internal.cloudapp.net
        headnode0.myhbase.b7.internal.cloudapp.net
        headnode1.myhbase.b7.internal.cloudapp.net
        workernode0.myhbase.b7.internal.cloudapp.net

### <a name="install-and-configure-squirrel-on-your-workstation"></a>Yükleme ve SQuirreL iş istasyonunuza yapılandırma
**SQuirreL yüklemek için**

1. SQuirreL SQL istemci jar dosyasını indirin [http://squirrel-sql.sourceforge.net/#installation](http://squirrel-sql.sourceforge.net/#installation).
2. Jar dosyasını açma veya çalıştırma. Gerektirdiği [Java Çalışma zamanı ortamı](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html).
3. Tıklatın **sonraki** iki kez.
4. Burada, yazma izni ve ardından bir yol belirtin **sonraki**.

  > [!NOTE]
  > Varsayılan yükleme klasörü C:\Program Files\squirrel-sql-3.6 klasöründe bulunur.  Bu yolu yazmak için yükleyici yönetici ayrıcalığı verilmelidir. Yönetici olarak bir komut istemi açın, Java'nın depo klasörüne gidin ve ardından çalıştırın:
  >
  >     Java.exe-jar [SQuirreL jar dosyasının yolu]
5. Tıklatın **Tamam** hedef dizin oluşturma onaylamak için.
6. Temel ve standart paketleri yüklemek için varsayılan ayardır.  **İleri**’ye tıklayın.
7. Tıklatın **sonraki** iki kez tıkladıktan sonra **Bitti**.

**Phoenix sürücüyü yüklemek için**

Phoenix sürücü jar dosyasını HBase kümesi bulunur. Yolu sürümlerine bağlı aşağıdakine benzer:

    C:\apps\dist\phoenix-4.0.0.2.1.11.0-2316\phoenix-4.0.0.2.1.11.0-2316-client.jar
İhtiyacınız olan [SQuirreL yükleme klasörü] altında iş istasyonunuzu kopyalayın / lib yolu.  En kolay yolu için RDP kümesine olduğundan ve dosya kopyalayıp yapıştırın (CTRL + C ve CTRL + V) istasyonunuza kopyalamak için kullanın.

**Phoenix sürücüsü için SQuirreL eklemek için**

1. SQuirreL SQL istemci istasyonunuzdan açın.
2. Tıklatın **sürücü** sol sekmesinde.
3. Gelen **sürücüleri** menüsünde tıklatın **yeni sürücü**.
4. Aşağıdaki bilgileri girin:

   * **Ad**: Phoenix
   * **Örnek URL**: jdbc:phoenix:zookeeper2.contoso hbase eu.f5.internal.cloudapp.net
   * **Sınıf adı**: org.apache.phoenix.jdbc.PhoenixDriver

     > [!WARNING]
     > Kullanıcı örnek URL tüm alt durumda. Kullanabileceğiniz bunlar tam zookeeper çekirdek bunlardan birini çalışmıyor durumda.  Konak zookeeper0, zookeeper1 ve zookeeper2 adları.
     >
     >

     ![Hdınsight HBase Phoenix SQuirreL sürücüsü][img-squirrel-driver]
5. **Tamam** düğmesine tıklayın.

**HBase kümesi için bir diğer adı oluşturmak için**

1. SQuirreL tıklatın **diğer adlar** sol sekmesinde.
2. Gelen **diğer adlar** menüsünde tıklatın **yeni diğer**.
3. Aşağıdaki bilgileri girin:

   * **Ad**: HBase kümesi veya tercih ettiğiniz herhangi bir ad adı.
   * **Sürücü**: Phoenix.  Bu son yordamda oluşturduğunuz sürücü adı eşleşmelidir.
   * **URL**: URL, sürücü yapılandırmasından kopyalanır. Kullanıcı için tüm küçük emin olun.
   * **Kullanıcı adı**: herhangi bir metin olabilir.  VPN bağlantısı burada kullandığından, kullanıcı adı hiç kullanılmaz.
   * **Parola**: herhangi bir metin olabilir.

     ![Hdınsight HBase Phoenix SQuirreL sürücüsü][img-squirrel-alias]
4. Tıklatın **Test**.
5. **Bağlan**'a tıklayın. Bağlantı yaptığında, SQuirreL şuna benzer:

    ![HBase Phoenix SQuirreL][img-squirrel]

**Bir test çalıştırmak için**

1. Tıklatın **SQL** sekmesinde İleri sağa **nesneleri** sekmesi.
2. Aşağıdaki kodu kopyalayıp yapıştırın:

        CREATE TABLE IF NOT EXISTS us_population (state CHAR(2) NOT NULL, city VARCHAR NOT NULL, population BIGINT  CONSTRAINT my_pk PRIMARY KEY (state, city))
3. Çalıştırma düğmesine tıklayın.

    ![HBase Phoenix SQuirreL][img-squirrel-sql]
4. Dönmek **nesneleri** sekmesi.
5. Diğer adı'nı genişletin ve ardından genişletin **tablo**.  Altında listelenen yeni tablo görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Hdınsight'ta Apache Phoenix kullanmayı öğrendiniz.  Daha fazla bilgi için bkz:

* [HDInsight HBase'e genel bakış][hdinsight-hbase-overview]: HBase büyük miktarda yapılandırılmamış ve yarı yapılandırılmış veri için rastgele erişim ve güçlü tutarlılık sağlayan, Hadoop'ta yerleşik bir Apache, açık kaynak, NoSQL veritabanıdır.
* [Azure Virtual Network HBase kümelerine sağlamak][hdinsight-hbase-provision-vnet]: uygulamalar HBase ile iletişim kurabilmesi için sanal ağ tümleştirmesinin ile HBase kümeleri aynı sanal ağ, uygulamalarınızın dağıtılabilir doğrudan.
* [Hdınsight'ta HBase çoğaltmayı yapılandırma](hdinsight-hbase-replication.md): HBase çoğaltmayı iki Azure veri merkezi arasında yapılandırmayı öğrenin.


[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md

[hdinsight-hbase-phoenix-sqlline]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-phoenix-sqlline.png
[img-certificate]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vpn-certificate.png
[img-vnet-diagram]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vnet-point-to-site.png
[img-squirrel-driver]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-driver.png
[img-squirrel-alias]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-alias.png
[img-squirrel]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel.png
[img-squirrel-sql]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-sql.png
