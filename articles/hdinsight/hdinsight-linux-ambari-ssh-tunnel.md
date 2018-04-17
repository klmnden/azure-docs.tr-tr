---
title: Azure Hdınsight erişmek için tünel SSH kullanma | Microsoft Docs
description: Linux tabanlı Hdınsight düğümler üzerinde barındırılan web kaynaklarına güvenli bir şekilde göz atmak için SSH tüneli kullanmayı öğrenin.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 879834a4-52d0-499c-a3ae-8d28863abf65
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 02/07/2018
ms.author: larryfr
ms.openlocfilehash: 05e06d6ed8c2a3bec0d12f81aae6f7022a56b942
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="use-ssh-tunneling-to-access-ambari-web-ui-jobhistory-namenode-oozie-and-other-web-uis"></a>Ambari web kullanıcı Arabirimi, kaynak, iş, Oozie ve diğer web Uı'lar erişmek için SSH tünel kullan

Linux tabanlı Hdınsight kümeleri Internet üzerinden erişebilmesi için Ambari web kullanıcı Arabirimi, ancak bazı özellikler UI değildir. Örneğin, web kullanıcı Arabirimi Ambari ortaya çıkmış diğer hizmetler için. Ambari web kullanıcı Arabirimi tam işlevsellik için küme head için SSH tüneli kullanmanız gerekir.

## <a name="why-use-an-ssh-tunnel"></a>SSH tüneli neden kullanılır?

Ambari menülerde çeşitli yalnızca SSH tüneli çalışır. Bu menüler web siteleri ve alt düğümleri gibi diğer düğüm türleri üzerinde çalışan hizmetleri kullanır. Doğrudan internet'te kullanıma sunmak güvenli değildir genellikle, bu web sitelerinin, sağlanmaz.

Aşağıdaki Web kullanıcı arabirimleri SSH tüneli gerektirir:

* Kaynak
* İş
* İş parçacığı yığınları
* Oozie web kullanıcı Arabirimi
* HBase ana ve günlükleri kullanıcı Arabirimi

Kümenizi özelleştirmek için betik eylemleri kullanın, herhangi bir hizmet veya bir web kullanıcı arabirimini kullanıma yüklediğiniz utilities SSH tüneli gerektirir. Örneğin, ton betik eylemini kullanarak yüklerseniz, Hue web kullanıcı arabirimini erişmek için bir SSH tüneli kullanmanız gerekir.

> [!IMPORTANT]
> Bir sanal ağ üzerinden hdınsight'a doğrudan erişimi varsa, SSH tünelleri kullanmak gerekmez. Hdınsight aracılığıyla bir sanal ağa doğrudan erişimini ilişkin bir örnek için bkz: [şirket içi ağınıza bağlanmak Hdınsight](connect-on-premises-network.md) belge.

## <a name="what-is-an-ssh-tunnel"></a>SSH tüneli nedir

[Güvenli Kabuk (SSH) tüneli](https://en.wikipedia.org/wiki/Tunneling_protocol#Secure_Shell_tunneling) yerel istasyonunuzda bir bağlantı noktasına gönderilen trafiğin yönlendirir. Trafik, Hdınsight küme baş düğümüne bir SSH bağlantısı üzerinden yönlendirilir. İstek baş düğümünde geldiyse olarak çözümlenir. Yanıt, sonra iş istasyonunuzu için tünelinden yönlendirilir.

## <a name="prerequisites"></a>Önkoşullar

* Bir SSH istemcisi. Çoğu işletim sistemi üzerinden bir SSH istemcisi sağlar `ssh` komutu. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

* SOCKS5 proxy kullanmak üzere yapılandırılmış bir web tarayıcısı.

    > [!WARNING]
    > Windows'da yerleşik SOCKS proxy desteği SOCKS5 desteklemez ve bu belgede yer alan adımlar ile çalışmaz. Aşağıdaki tarayıcılar Windows proxy ayarlarını kullanır ve bu belgedeki adımları şu anda çalışmıyor:
    >
    > * Microsoft Edge
    > * Microsoft Internet Explorer
    >
    > Google Chrome, aynı zamanda Windows proxy ayarlarını kullanır. Ancak, SOCKS5 Destek Uzantıları yükleyebilirsiniz. Öneririz [FoxyProxy standart](https://chrome.google.com/webstore/detail/foxyproxy-standard/gcknhkkoolaabfmlnjonogaaifnjlfnp).

## <a name="usessh"></a>SSH komutunu kullanarak bir tünel oluşturma

Bir SSH oluşturmak için aşağıdaki komutu kullanarak tünel kullanımı `ssh` komutu. Değiştir **kullanıcıadı** Hdınsight kümesi ve değiştirmek için bir SSH kullanıcıyla **CLUSTERNAME** Hdınsight kümenizin adıyla:

```bash
ssh -C2qTnNf -D 9876 USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
```

Bu komut, bağlantı noktasına yerel 9876 kümeye SSH üzerinden trafiğini yönlendiren bir bağlantı oluşturur. Seçenekler şunlardır:

* **D 9876** -Tünel üzerinden trafiğini yönlendiren yerel bağlantı noktası.
* **C** -web trafiği çoğunlukla metin olduğundan tüm verileri sıkıştırmak.
* **2** -force Protokolü sürüm 2 yalnızca denemek için SSH.
* **q** -Sessiz mod.
* **T** -yalnızca bir bağlantı noktası iletme bu yana sözde tty ayırma, devre dışı.
* **n** -yalnızca bir bağlantı noktası iletme bu yana STDIN, okunması engelleme.
* **N** -yalnızca bir bağlantı noktası iletme bu yana bir uzak komutu yürütülmez.
* **f** -arka planda çalıştırın.

Komut bittikten sonra yerel bilgisayarda 9876 numaralı bağlantı noktasına gönderilen trafiğin küme baş düğümüne yönlendirilir.

## <a name="useputty"></a>PuTTY kullanarak bir tünel oluşturma

[PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty) Windows için grafik SSH istemcidir. PuTTY kullanarak SSH tüneli oluşturmak için aşağıdaki adımları kullanın:

1. Putty'yi açın ve bağlantı bilgilerinizi girin. PuTTY ile bilmiyorsanız bkz [PuTTY belgeleri (http://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html)](http://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html).

2. İçinde **kategori** iletişim kutusunun solundaki bölümünde, genişletmek **bağlantı**, genişletin **SSH**ve ardından **tüneller**.

3. Üzerinde aşağıdaki bilgileri sağlayın **SSH denetleme seçenekleri bağlantı noktası iletme** form:
   
   * **Kaynak bağlantı noktası** - İletmek istediğiniz istemci üzerindeki bağlantı noktası. Örneğin, **9876**.

   * **Hedef** -Linux tabanlı Hdınsight kümesi için SSH adresi. Örneğin, **mycluster-ssh.azurehdinsight.net**.

   * **Dinamik** - Dinamik SOCKS proxy yönlendirmesini etkinleştirir.
     
     ![Görüntü seçenekleri tünel](./media/hdinsight-linux-ambari-ssh-tunnel/puttytunnel.png)

4. Tıklatın **Ekle** ayarları ekleyin ve ardından **açmak** bir SSH bağlantısı açılamıyor.

5. İstendiğinde, sunucuya oturum açın.

## <a name="use-the-tunnel-from-your-browser"></a>Tarayıcınızdan tünel kullanın

> [!IMPORTANT]
> Aynı proxy ayarları tüm platformlarda sağladığı gibi bu bölümdeki adımları Mozilla FireFox tarayıcı kullanın. Google Chrome gibi modern tarayıcılar uzantı tünel ile çalışmak için FoxyProxy gibi gerektirebilir.

1. Kullanılacak tarayıcı yapılandırma **localhost** ve ne zaman kullanılan bağlantı noktası olarak tünel oluşturma bir **SOCKS v5** proxy. Firefox ayarlarının nasıl göründüğünü aşağıda verilmiştir. Farklı bir bağlantı noktasıyla 9876 kullandıysanız, kullandığınız bir bağlantı noktasını değiştirin:
   
    ![Firefox ayarlarının görüntüsü](./media/hdinsight-linux-ambari-ssh-tunnel/firefoxproxy.png)
   
   > [!NOTE]
   > Seçme **uzak DNS** Hdınsight kümesi kullanarak etki alanı adı sistemi (DNS) isteklerinin giderir. Bu ayar kümenin baş düğümüne kullanarak DNS çözümler.

2. Tünel bir siteyi ziyaret ederek gibi çalıştığını doğrulamak [ http://www.whatismyip.com/ ](http://www.whatismyip.com/). Döndürülen IP bir Microsoft Azure veri merkezi tarafından kullanılan olması.

## <a name="verify-with-ambari-web-ui"></a>Ambari web kullanıcı Arabirimi ile doğrulayın

Küme oluşturulduktan sonra hizmet web Uı'lar Ambari Web üzerinden erişebilirsiniz doğrulamak için aşağıdaki adımları kullanın:

1. Tarayıcınızda, Git http://headnodehost:8080. `headnodehost` Adresi ambarı üzerinde çalıştığı headnode çözümleyip ve küme için tüneli üzerinden gönderilir. İstendiğinde, kümeniz için yönetici kullanıcı adını (Yönetici) ve parolasını girin. Ambari web kullanıcı arabirimini ikinci kez istenebilir. Öyleyse, bilgileri yeniden girin.

   > [!NOTE]
   > Kullanırken http://headnodehost:8080 kümeye bağlanmak için adres, tünel üzerinden bağlanan. İletişim HTTPS yerine SSH tüneli kullanılarak güvenli hale getirilir. HTTPS kullanarak Internet üzerinden bağlanırken kullanacağı https://CLUSTERNAME.azurehdinsight.net, burada **CLUSTERNAME** kümesinin adı.

2. Ambari Web kullanıcı arabirimini HDFS sayfanın sol taraftaki listeden seçin.

    ![Seçili HDFS görüntüsüyle](./media/hdinsight-linux-ambari-ssh-tunnel/hdfsservice.png)

3. HDFS hizmet bilgileri görüntülendiğinde seçin **hızlı bağlantılar**. Küme baş düğümler listesi görüntülenir. Baş düğümlerden birini seçin ve ardından **iş UI**.

    ![Görüntü QuickLinks menüsü ile genişletilmiş](./media/hdinsight-linux-ambari-ssh-tunnel/namenodedropdown.png)

   > [!NOTE]
   > Seçtiğinizde, __hızlı bağlantılar__, bekleme göstergesi alabilirsiniz. Yavaş bir internet bağlantınız varsa, bu durum oluşabilir. Bir veya iki sunucudan alınacak veri için dakika bekleyin ve sonra listenin tekrar deneyin.
   >
   > Bazı girişler **hızlı bağlantılar** menü ekranın sağ tarafındaki tarafından kesilmiş. Bu durumda, farenizi kullanarak menüsünü genişletin ve ekran menü geri kalanını görmek için sağa kaydırma için sağ ok tuşunu kullanın.

4. Aşağıdaki görüntüye benzer bir sayfa görüntülenir:

    ![İş UI görüntüsü](./media/hdinsight-linux-ambari-ssh-tunnel/namenode.png)

   > [!NOTE]
   > Bu sayfanın URL'sini dikkat edin. benzer olmalıdır **http://hn1-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8088/cluster**. Bu URI düğümünün iç tam etki alanı adı (FQDN) kullanıyor ve SSH tüneli kullanırken, yalnızca erişilebilir.

## <a name="next-steps"></a>Sonraki adımlar

SSH tüneli oluşturma ve öğrendiniz, Ambari kullanmak diğer yolları için aşağıdaki belgelere bakın:

* [Ambari kullanarak Hdınsight kümelerini yönetme](hdinsight-hadoop-manage-ambari.md)

Hdınsight ile SSH kullanma hakkında daha fazla bilgi için bkz: [Hdınsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

