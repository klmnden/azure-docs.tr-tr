<properties
   pageTitle="Windows’da Linux tabanlı kümelerde Hadoop’ta SSH anahtarları kullanma | Microsoft Azure"
   description="Linux tabanlı HDInsight kümelerinin kimlik doğrulaması için SSH anahtarları oluşturmayı ve kullanmayı öğrenin. PuTTY SSH istemcisini kullanarak Windows tabanlı istemcilerden kümelere bağlanın."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/30/2016"
   ms.author="larryfr"/>


#Windows’da HDInsight’ta Linux tabanlı Hadoop ile SSH kullanma

> [AZURE.SELECTOR]
- [Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
- [Linux, Unix, OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

[Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) Bir komut satırı arabirimi kullanarak, Linux tabanlı HDInsight kümelerinizde işlemleri uzaktan gerçekleştirmenizi sağlar. Bu belge PuTTY SSH istemcisini kullanarak Windows tabanlı istemcilerden HDInsight’a bağlanmaya ilişkin bilgiler sağlar

> [AZURE.NOTE] Bu makaledeki adımlarda Windows tabanlı istemci kullandığınız varsayılır. Linux, Unix ya da OS X kullanıyorsanız, bkz. [Linux, Unix ya da OS X’te HDInsight’ta Linux tabanlı Hadoop ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).
>
> Windows 10’unuz varsa ve [Bash on Ubuntu on Windows](https://msdn.microsoft.com/commandline/wsl/about) kullanıyorsanız [Linux, Unix ya da OS X üzerinde HDInsight’ta Linux tabanlı Hadoop ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md) belgesindeki adımları kullanabilirsiniz.

##Ön koşullar

* Windows tabanlı istemciler için **PuTTY** ve **PuTTYGen**. Yardımcı programlar [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) sayfasında bulunabilir.

* HTML5'i destekleyen modern bir web tarayıcısı.

OR

* [Azure CLI](../xplat-cli-install.md).

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 

##SSH nedir?

SSH, uzak bir sunucuda oturum açma ve komutları uzaktan yürütme yardımcı programdır. Linux tabanlı HDInsight ile, SSH küme baş düğümüne şifreli bir bağlantı kurar ve komutları yazmak için kullandığınız bir komut satırı sağlar. Komutlar böylece doğrudan sunucuda yürütülür.

###SSH kullanıcı adı

SSH kullanıcı adı, HDInsight kümesi için kimlik doğrulamasında kullandığınız addır. Küme oluşturma sırasında bir SSH kullanıcı adı belirttiğinizde, bu kullanıcı kümedeki tüm düğümlerde oluşturulur. Küme oluşturulduktan sonra, HDInsight kümesi baş düğümlerine bağlanmak için bu kullanıcı adını kullanabilirsiniz. Baş düğümlerden de ayrı ayrı çalışan düğümlerine bağlanabilirsiniz.

###SSH parolası veya Ortak anahtar

Bir SSH kullanıcısı kimlik doğrulaması için bir parola veya ortak anahtar kullanabilir. Parola sizin oluşturduğunuz bir metin dizesi iken, ortak anahtar sizi benzersiz olarak tanımlamak üzere oluşturulan şifreleme anahtar çiftinin bir parçasıdır.

Anahtar paroladan daha güvenlidir, ancak anahtar oluşturmak ek adımlar gerektirir ve anahtarı içeren dosyaları güvenli bir yerde saklamalısınız. Biri anahtar dosyalarına erişim kazanırsa, hesabınıza da erişim kazanır. Veya anahtar dosyalarını kaybederseniz, hesabınızda oturum açamazsınız.

Bir anahtar çifti, ortak anahtar (HDInsight sunucusuna gönderilir) ve özel anahtardan (istemci makinenizde tutulur) oluşur SSH kullanarak HDInsight sunucusuna bağlandığınızda, SSH istemcisi sunucuda kimlik doğrulaması için makinenizdeki özel anahtarı kullanır.

##SSH anahtarı oluşturma

Kümenizle bir SSH anahtarları kullanmayı planlıyorsanız, aşağıdaki bilgileri kullanın. Bir parola kullanmayı planlıyorsanız, bu bölümü atlayabilirsiniz.

1. PuTTYGen’i açın.

2. **Oluşturulacak anahtar türü için**, **SSH-2 RSA**’yı seçin ve ardından **Oluştur**’a tıklayın.

    ![PuTTYGen arabirimi](./media/hdinsight-hadoop-linux-use-ssh-windows/puttygen.png)

3. Çubuk doluncaya kadar, fareyi ilerleme çubuğunun altındaki alanda hareket ettirin. Fareyi hareket ettirmek, anahtarı oluşturmak için kullanılan rastgele veriler üretir.

    ![moving the mouse around](./media/hdinsight-hadoop-linux-use-ssh-windows/movingmouse.png)

    Anahtar oluşturulduktan sonra, ortak anahtar görüntülenir.

4. Ek güvenlik için **Anahtarı parolası** alanına bir parola girin ve ardından aynı değeri **Parolayı onayla** alanına yazın.

    ![passphrase](./media/hdinsight-hadoop-linux-use-ssh-windows/key.png)

    > [AZURE.NOTE] Anahtar için güvenli bir parola kullanmanızı öneriyoruz. Ancak, parolayı unutursanız, bunu kurtarma yolu yoktur.

5. **Özel anahtarı kaydet**’e tıklayarak anahtarı bir **.ppk** dosyasına kaydedin. Bu anahtar, Linux tabanlı HDInsight kümenize kimlik doğrulaması için kullanılır.

    > [AZURE.NOTE] Linux tabanlı HDInsight kümenize erişmek için kullanılabileceğinden, bu anahtarı güvenli bir konumda saklamalısınız.

6. Anahtarı **.txt** dosyası olarak kaydetmek için **Ortak anahtarı kaydet**’e tıklayın. Bu, ek Linux tabanlı HDInsight kümeleri oluşturduğunuzda ortak anahtarı gelecekte tekrar kullanmanıza olanak sağlar.

    > [AZURE.NOTE] Ortak anahtar de PuTTYGen’in üst kısmında da görüntülenir. Azure Portal kullanarak bir küme oluştururken, bu alana sağ tıklayabilir, değeri kopyalayabilir ve sonra forma yapıştırabilirsiniz.

##Linux tabanlı HDInsight kümesi oluşturma

Linux tabanlı HDInsight kümesi oluştururken, önceden oluşturduğunuz ortak anahtarı sağlamanız gerekir. Windows tabanlı istemcilerde, Linux tabanlı HDInsight kümesi oluşturmanın iki yolu vardır:

* **Azure Portal** -Küme oluşturmak için web tabanlı portal kullanır.

* **Mac, Linux ve Windows için Azure CLI** -Küme oluşturmak için komut satırı komutlarını kullanır.

Bu yöntemlerin her biri ortak anahtarı gerektirir. Linux tabanlı HDInsight kümesi oluşturma hakkında tam bilgi için bkz. [Linux tabanlı HDInsight kümeleri hazırlama](hdinsight-hadoop-provision-linux-clusters.md).

###Azure Portal

[Azure Portal][preview-portal]’ı kullanırken, Linux tabanlı HDInsight kümesi oluşturmak için girmelisiniz bir **SSH Kullanıcı Adı** girmeli ve bir **PAROLA** ya da **SSH ORTAK ANAHTARI** girmeyi seçmelisiniz.

**SSH ORTAK ANAHTARI**’nı seçerseniz, ortak anahtarı (PuttyGen’de __Ortak anahtarı OpenSSH yetkili\_anahtarları dosyasına__ yapıştırma alanında görüntülenir) __SSH PublicKey__ alanına yapıştırabilirsiniz ya da ortak anahtarı içeren dosyayı bulmak ve seçmek için __Dosya seç__’i seçebilirsiniz.

![Ortak anahtar isteyen form görüntüsü](./media/hdinsight-hadoop-linux-use-ssh-windows/ssh-key.png)

Bu belirtilen kullanıcı için bir oturum oluşturur ve parola kimlik doğrulaması veya SSH anahtar kimlik doğrulaması sağlar.

###Mac, Linux ve Windows için Azure Komut Satırı Arabirimi

`azure hdinsight cluster create` komutunu kullanarak yeni bir küme oluşturmak için [Mac, Linux ve Windows için Azure CLI](../xplat-cli-install.md)’yı kullanabilirsiniz.

Bu komutu kullanma hakkında daha fazla bilgi için bkz. [Özel seçenekleri kullanarak HDInsight’ta Hadoop Linux kümeleri hazırlama](hdinsight-hadoop-provision-linux-clusters.md).

##Linux tabanlı HDInsight kümesine bağlanma

1. PuTTY’yi açın.

    ![putty arabirimi](./media/hdinsight-hadoop-linux-use-ssh-windows/putty.png)

2. Kullanıcı hesabınızı oluştururken bir SSH anahtarı sağladıysanız, küme kimlik doğrulaması yapılırken özel anahtarı seçmek amacıyla aşağıdaki adımı gerçekleştirmelisiniz.

    **Kategori**’de **Bağlantı**’yı genişletin, **SSH**’yi genişletin ve **Kimlik Doğrulaması**’nı seçin. Son olarak, **Gözat**’a tıklayın ve özel anahtarınızı içeren .ppk dosyasını seçin.

    ![putty arabirimi, özel anahtarı seçme](./media/hdinsight-hadoop-linux-use-ssh-windows/puttykey.png)

3. **Kategori**’de **Oturum**’u seçin. **PuTTY oturumunuz için temel seçenekler** ekranında, **Ana bilgisayar adı (veya IP adresi)** alanına HDInsight sunucunuzun SSH adresini girin. Bir kümeye bağlanırken kullanabileceğiniz iki olası SSH adresi vardır:

    * __Baş düğüm adresi__: Kümenin baş düğümüne bağlanmak için küme adınızı ve ardından **-ssh.azurehdinsight.net**'i kullanın. Örneğin, **mycluster-ssh.azurehdinsight.net**.
    
    * __Edge düğüm adresi__: HDInsight kümesinde bir R Server’a bağlanıyorsanız, CLUSTERNAME’in küme adınız olduğu __RServer.CLUSTERNAME.ssh.azurehdinsight.net__ adresini kullanarak R Server edge düğümüne bağlanabilirsiniz. Örneğin, __RServer.mycluster.ssh.azurehdinsight.net__.

    ![putty interface with ssh address entered](./media/hdinsight-hadoop-linux-use-ssh-windows/puttyaddress.png)

4. Gelecekte kullanılmak üzere bağlantı bilgilerini kaydetmek için, **Kayıtlı Oturumlar** altında bu bağlantının adını girin ve **Kaydet**’e tıklayın. Bağlantı kayıtlı oturumlar listesine eklenir.

5. Kümeye bağlanmak için **Aç**’a tıklayın.

    > [AZURE.NOTE] Bu, kümeye ilk bağlanışınız ise, bir güvenlik uyarısı alırsınız. Bu normaldir. Devam etmek üzere sunucunun RSA2 anahtarını önbelleğe kaydetmek için **Evet**’i seçin.

6. İstendiğinde, kümeyi oluşturduğunuzda girdiğiniz kullanıcıyı girin. Kullanıcı için parola sağladıysanız, parolayı girmeniz de istenir.

> [AZURE.NOTE] Yukarıdaki adımlarda, HDInsight kümesindeki birincil baş düğüme bağlanacak olan bağlantı noktası 22'yi kullandığınız varsayılmıştır. Bağlantı noktası 23'ü kullanırsanız ikincil baş düğüme bağlanırsınız. Baş düğümler hakkında daha fazla bilgi için bkz. [HDInsight’ta Hadoop kümelerinin kullanılabilirliği ve güvenilirliği](hdinsight-high-availability-linux.md).

###Çalışan düğümüne bağlanma

Çalışan düğümüne Azure veri merkezi dışında doğrudan erişilemez, ancak küme baş düğümünden SSH aracılığıyla erişilebilir.

Kullanıcı hesabınızı oluştururken bir SSH anahtarı sağladıysanız, çalışan düğümlerine bağlanmak istiyorsanız kimlik doğrularken özel anahtarı kullanmak üzere aşağıdaki adımları gerçekleştirmelisiniz.

1. Pageant’ı [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) sayfasından yükleyin. Bu yardımcı program PuTTY için SSH anahtarlarını önbelleğe almak için kullanılır.

2. Pageant’ı çalıştırın. Durum tepsisinde bir simge durumuna küçülür. Simgeye sağ tıklayın ve **Anahtar Ekle**’yi seçin.

    ![adding key](./media/hdinsight-hadoop-linux-use-ssh-windows/addkey.png)

3. Gözatma iletişim kutusu göründüğünde, anahtarı içeren .ppk dosyasını seçin ve ardından **Aç**’a tıklayın. Bu, anahtarı kümeye bağlanırken onu PuTTY’ye sağlayacak olan Pageant’a ekler.

    > [AZURE.IMPORTANT] Hesabınızı korumak için bir SSH anahtarı kullandıysanız, çalışan düğümlerine bağlanabilmek için önceki adımları tamamlamanız gerekir.

4. PuTTY’yi açın.

5. Kimlik doğrulaması için SSH anahtarı kullanıyorsanız, **Kategori** bölümünde **Bağlantı**’yı genişletin **SSH**’yi genişletin ve ardından **Kimlik Doğrulaması**’nı seçin.

    **Kimlik doğrulama parametreleri** bölümünde, **Aracı iletmeye izin ver**’i etkinleştirin. Bu, PuTTY’nin çalışan düğümlerine bağlanırken, küme baş düğümüne sertifika kimlik doğrulamasını otomatik olarak geçirmesini sağlar.

    ![allow agent forwarding](./media/hdinsight-hadoop-linux-use-ssh-windows/allowforwarding.png)

6. Daha önce belirtildiği gibi kümeye bağlanın. Kimlik doğrulaması için SSH anahtarı kullanıyorsanız, anahtarı seçmeniz gerekmez; Pageant’a eklenen anahtar küme kimlik doğrulaması için kullanılır.

7. Bağlantı kurulduktan sonra, kümenizdeki düğümlerin listesini almak için aşağıdakini kullanın. *ADMINPASSWORD* değerin küme yönetici hesabınızın parolası ile değiştirin *CLUSTERNAME* değerini kümenizin adıyla değiştirin.

        curl --user admin:ADMINPASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/hosts

    Bu, her bir düğüm için uygun etki alanı adını (FQDN) içeren `host_name` dahil, kümedeki düğümler için bilgileri JSON biçiminde döndürür. Aşağıda **curl** komutuyla döndürülen bir `host_name` girişi örneği gösterilmektedir:

        "host_name" : "workernode0.workernode-0-e2f35e63355b4f15a31c460b6d4e1230.j1.internal.cloudapp.net"

8. Bağlanmak istediğiniz tüm çalışan düğümlerinin listesine sahip olduktan sonra, çalışan düğüme bir bağlantı açmak için PuTTY oturumunda aşağıdaki komutu kullanın:

        ssh USERNAME@FQDN

    *USERNAME* değerini kendi SSH kullanıcı adınızla ve *FQDN* değerini çalışan düğümünüz için FQDN ile değiştirin. Örneğin, `workernode0.workernode-0-e2f35e63355b4f15a31c460b6d4e1230.j1.internal.cloudapp.net`.

    > [AZURE.NOTE] SSH oturumunuzun kimliğini doğrulamak için parola kullanıyorsanız, parolayı tekrar girmeniz istenir. SSH anahtarı kullanıyorsanız, bağlantı herhangi bir soru olmadan tamamlanmalıdır.

9. Oturum kurulduktan sonra, çalışan düğümüne bağlandığınızı belirtmek için PuTTY oturumu isteminiz `username@hn#-clustername` iken `username@wn#-clustername` olarak değişir. Bu noktada çalıştırdığını tüm çalışan düğümünde çalışır.

10. Çalışan düğümünde eylemler gerçekleştirmeyi tamamladıktan sonra, çalışan düğümüne olan oturumu kapatmak için `exit` komutunu kullanın. Bu, size `username@hn#-clustername` istemini döndürür.

##Daha fazla hesap ekleme

Kümenize daha fazla hesap eklemeniz gerekiyorsa, aşağıdaki adımları gerçekleştirin:

1. Daha önce açıklandığı şekilde, yeni kullanıcı hesabı için yeni bir ortak anahtar ve özel anahtar oluşturun.

2. Küme için bir SSH oturumunda, aşağıdaki komutla yeni kullanıcıyı ekleyin:

        sudo adduser --disabled-password <username>

    Bu, yeni bir kullanıcı hesabı oluşturur, ancak parola kimlik doğrulamasını iptal eder.

3. Aşağıdaki komutları kullanarak anahtarı tutmak için dizin ve dosyaları oluşturun:

        sudo mkdir -p /home/<username>/.ssh
        sudo touch /home/<username>/.ssh/authorized_keys
        sudo nano /home/<username>/.ssh/authorized_keys

4. Nano düzenleyici açıldığında, yeni kullanıcı hesabının ortak anahtar içeriğini kopyalayıp yapıştırın. Son olarak, **Ctrl-X**’i kullanarak dosyayı kaydedin ve düzenleyiciden çıkın.

    ![image of nano editor with example key](./media/hdinsight-hadoop-linux-use-ssh-windows/nano.png)

5. .ssh klasörü sahipliğini ve içeriğini yeni kullanıcı hesabıyla değiştirmek için aşağıdaki komutu kullanın:

        sudo chown -hR <username>:<username> /home/<username>/.ssh

6. Artık yeni kullanıcı hesabı ve özel anahtarla sunucuda kimlik doğrulaması yapabiliyor olmanız gerekir.

##<a id="tunnel"></a>SSH tünel oluşturma

SSH, web istekleri gibi yerel istekler için HDInsight kümesine tünel oluşturmak üzere kullanılabilir. Daha sonra, istek HDInsight kümesi baş düğümünde oluşturulmuş gibi istenen kaynağa iletilir.

> [AZURE.IMPORTANT] SSH tüneli bazı Hadoop hizmetleri için web kullanıcı arabirimine erişmek üzere bir gereksinimdir. Örneğin, İş Geçmişi kullanıcı arabirimi veya Kaynak Yöneticisi kullanıcı arabirimine yalnızca SSH tüneli kullanılarak erişilebilir.

SSH tüneli oluşturma ve kullanma hakkında daha fazla bilgi için bkz. [Ambari web kullanıcı arabirimi, Kaynak Yöneticisi, İş Geçmişi, Ad Düğümü, Oozie ve diğer web kullanıcı arabirimlerine erişim için SSH tünel oluşturmayı kullanma](hdinsight-linux-ambari-ssh-tunnel.md).

##Sonraki adımlar

Artık bir SSH anahtarı kullanarak kimlik doğrulaması yapacağınızı anladığınıza göre, HDInsight’ta Hadoop ile MapReduce kullanmayı öğrenin.

* [HDInsight ile Hive kullanma](hdinsight-use-hive.md)

* [HDInsight ile Pig kullanma](hdinsight-use-pig.md)

* [HDInsight ile MapReduce işleri kullanma](hdinsight-use-mapreduce.md)

[preview-portal]: https://portal.azure.com/



<!--HONumber=Sep16_HO3-->


