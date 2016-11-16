---
title: "Linux, Unix ya da OS X’te Linux tabanlı Hadoop ile SSH anahtarlarını kullanma | Microsoft Belgeleri"
description: " Secure Shell (SSH) kullanarak Linux tabanlı HDInsight’a erişebilirsiniz. Bu belge Linux, Unix ya da OS X istemcilerinde HDInsight ile SSH kullanma hakkında bilgi sağlar."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: a6a16405-a4a7-4151-9bbf-ab26972216c5
ms.service: hdinsight
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/13/2016
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 476d9ce8b64f3442031310bd9170c682a9940b2b


---
# <a name="use-ssh-with-linuxbased-hadoop-on-hdinsight-from-linux-unix-or-os-x"></a>Linux, Unix ya da OS X’te HDInsight’ta Linux tabanlı Hadoop ile SSH’yi kullanma
> [!div class="op_single_selector"]
> * [Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
> * [Linux, Unix, OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
> 
> 

[Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) Bir komut satırı arabirimi kullanarak, Linux tabanlı HDInsight kümelerinizde işlemleri uzaktan gerçekleştirmenizi sağlar. Bu belge Linux, Unix ya da OS X istemcilerinde HDInsight ile SSH kullanma hakkında bilgi sağlar.

> [!NOTE]
> Bu makaledeki adımlarda Linux, Unix veya OS X istemci kullandığınız varsayılır. [Bash on Ubuntu on Windows](https://msdn.microsoft.com/commandline/wsl/about) gibi `ssh` ve `ssh-keygen` sağlayan bir paket yüklediyseniz bu adımlar Windows tabanlı bir istemcide gerçekleştirilebilir.
> 
> Windows tabanlı istemcinizde SSH yüklü değilse PuTTY yükleme ve kullanma hakkında bilgi için bkz. [Windows’tan Linux tabanlı HDInsight (Hadoop) kullanmak için SSH kullanma](hdinsight-hadoop-linux-use-ssh-windows.md).
> 
> 

## <a name="prerequisites"></a>Ön koşullar
* OS X Linux ve Unix istemcileri için **SSH-keygen** ve **ssh**. Bu yardımcı programlar genellikle işletim sisteminizle birlikte verilir veya paket yönetim sistemi aracılığıyla sağlanır.
* HTML5'i destekleyen modern bir web tarayıcısı.

OR

* [Azure CLI](../xplat-cli-install.md).
  
    [!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 

## <a name="what-is-ssh"></a>SSH nedir?
SSH, uzak bir sunucuda oturum açma ve komutları uzaktan yürütme yardımcı programdır. SSH, Linux tabanlı HDInsight ile küme baş düğümüne şifreli bir bağlantı kurar ve komutları yazmak için kullandığınız bir komut satırı sağlar. Komutlar böylece doğrudan sunucuda yürütülür.

### <a name="ssh-user-name"></a>SSH kullanıcı adı
SSH kullanıcı adı, HDInsight kümesi için kimlik doğrulamasında kullandığınız addır. Küme oluşturma sırasında bir SSH kullanıcı adı belirttiğinizde, bu kullanıcı kümedeki tüm düğümlerde oluşturulur. Kime oluşturulduğunda, bu kullanıcı adını HDInsight küme baş düğümüne bağlanmak için kullanabilirsiniz. Baş düğümlerden de ayrı ayrı çalışan düğümlerine bağlanabilirsiniz.

### <a name="ssh-password-or-public-key"></a>SSH parolası veya Ortak anahtar
Bir SSH kullanıcısı kimlik doğrulaması için bir parola veya ortak anahtar kullanabilir. Parola sizin oluşturduğunuz bir metin dizesi iken, ortak anahtar sizi benzersiz olarak tanımlamak üzere oluşturulan şifreleme anahtar çiftinin bir parçasıdır.

Anahtar paroladan daha güvenlidir, ancak anahtar oluşturmak ek adımlar gerektirir ve anahtarı içeren dosyaları güvenli bir yerde saklamalısınız. Biri anahtar dosyalarına erişim kazanırsa, hesabınıza da erişim kazanır. Veya anahtar dosyalarını kaybederseniz, hesabınızda oturum açamazsınız.

Bir anahtar çifti, ortak anahtar (HDInsight sunucusuna gönderilir) ve özel anahtardan (istemci makinenizde tutulur) oluşur SSH kullanarak HDInsight sunucusuna bağlandığınızda, SSH istemcisi sunucuda kimlik doğrulaması için makinenizdeki özel anahtarı kullanır.

## <a name="create-an-ssh-key"></a>SSH anahtarı oluşturma
Kümenizle bir SSH anahtarları kullanmayı planlıyorsanız, aşağıdaki bilgileri kullanın. Bir parola kullanmayı planlıyorsanız, bu bölümü atlayabilirsiniz.

1. Terminal oturumunu açın ve mevcut bir SSH anahtarınız olup olmadığını görmek için aşağıdaki komutu kullanın:
   
        ls -al ~/.ssh
   
    Dizin listesinde aşağıdaki dosyaları arayın. Bunlar ortak SSH anahtarları için yaygın kullanılan adlardır.
   
   * id\_dsa.pub
   * id\_ecdsa.pub
   * id\_ed25519.pub
   * id\_rsa.pub
2. Mevcut bir dosyayı kullanmak istemiyorsanız veya mevcut bir SSH anahtarınız yoksa, yeni bir dosya oluşturmak için aşağıdakileri kullanın:
   
        ssh-keygen -t rsa
   
    Sizden aşağıdaki bilgiler istenir:
   
   * Dosya konumu - Konum ~/.ssh/id\_rsa olarak varsayılan ayarlıdır.
   * Bir parola - Bunu yeniden girmeniz istenir.
     
     > [!NOTE]
     > Anahtar için güvenli bir parola kullanmanızı öneriyoruz. Ancak, parolayı unutursanız, bunu kurtarma yolu yoktur.
     > 
     > 
     
     Komut bittikten sonra, özel anahtar (örneğin, **id\_rsa**) ve ortak anahtar (örneğin, **id\_rsa.pub**) olmak üzere iki yeni dosyanız olur. 

## <a name="create-a-linuxbased-hdinsight-cluster"></a>Linux tabanlı HDInsight kümesi oluşturma
Linux tabanlı HDInsight kümesi oluştururken, önceden oluşturduğunuz ortak anahtarı sağlamanız gerekir. Linux, Unix ya da OS X istemcilerde HDInsight kümesi oluşturmanın iki yolu vardır:

* **Azure Portal** -Küme oluşturmak için web tabanlı portal kullanır.
* **Mac, Linux ve Windows için Azure CLI** -Küme oluşturmak için komut satırı komutlarını kullanır.

Bu yöntemlerin her biri bir parola veya ortak anahtar gerektirir. Linux tabanlı HDInsight kümesi oluşturma hakkında tam bilgi için bkz. [Linux tabanlı HDInsight kümeleri hazırlama](hdinsight-hadoop-provision-linux-clusters.md).

### <a name="azure-portal"></a>Azure Portal
[Azure Portal][preview-portal]’ı kullanırken, Linux tabanlı HDInsight kümesi oluşturmak için girmelisiniz bir **SSHKULLANICI ADI** girmeli ve bir **PAROLA** ya da **SSH ORTAK ANAHTARI** girmeyi seçmelisiniz.

**SSH ORTAK ANAHTARI**’nı seçerseniz, ortak anahtarı (**.pub** uzantılı dosyada yer alır) **SSH PublicKey** alanına yapıştırabilirsiniz ya da ortak anahtar dosyasını bulmak ve seçmek için **Dosya seç**’i seçebilirsiniz.

![Ortak anahtar isteyen form görüntüsü](./media/hdinsight-hadoop-linux-use-ssh-unix/ssh-key.png)

> [!NOTE]
> Anahtar dosyası yalnızca bir metin dosyasıdır. İçeriği aşağıdakine benzer görünmelidir:
> 
> ```
> ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCelfkjrpYHYiks4TM+r1LVsTYQ4jAXXGeOAF9Vv/KGz90pgMk3VRJk4PEUSELfXKxP3NtsVwLVPN1l09utI/tKHQ6WL3qy89WVVVLiwzL7tfJ2B08Gmcw8mC/YoieT/YG+4I4oAgPEmim+6/F9S0lU2I2CuFBX9JzauX8n1Y9kWzTARST+ERx2hysyA5ObLv97Xe4C2CQvGE01LGAXkw2ffP9vI+emUM+VeYrf0q3w/b1o/COKbFVZ2IpEcJ8G2SLlNsHWXofWhOKQRi64TMxT7LLoohD61q2aWNKdaE4oQdiuo8TGnt4zWLEPjzjIYIEIZGk00HiQD+KCB5pxoVtp user@system
> ```
> 
> 

Bu, verdiğiniz parolayı veya ortak anahtarı kullanarak, belirtilen kullanıcı için bir oturum oluşturur.

### <a name="azure-commandline-interface-for-mac-linux-and-windows"></a>Mac, Linux ve Windows için Azure komut satırı arabirimi
`azure hdinsight cluster create` komutunu kullanarak yeni bir küme oluşturmak için [Mac, Linux ve Windows için Azure CLI](../xplat-cli-install.md)’yı kullanabilirsiniz.

Bu komutu kullanma hakkında daha fazla bilgi için bkz. [Özel seçenekleri kullanarak HDInsight’ta Hadoop Linux kümeleri hazırlama](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="connect-to-a-linuxbased-hdinsight-cluster"></a>Linux tabanlı HDInsight kümesine bağlanma
Terminal oturumunda, şu adresi ve kullanıcı adını sağlayarak küme baş düğümüne bağlanmak için SSH komutunu kullanın:

* **SSH adresi** -SSH kullanarak bir kümeye bağlanmak için kullanılabilecek iki adres vardır:
  
  * **Baş düğüme bağlanma**: Küme adını **-ssh.azurehdinsight.net** ifadesi izler. Örneğin, **mycluster-ssh.azurehdinsight.net**.
  * **Edge düğümüne bağlanma**: Kümeniz HDInsight’ta R Server ise, küme kümeye **CLUSTERNAME**’in küme adı olduğu **RServer.CLUSTERNAME.ssh.azurehdinsight.net** kullanılarak bağlanılabilen bir edge düğümünü de içerir.
* **Kullanıcı adı** -Küme oluştururken verdiğiniz SSH kullanıcı adı.

Şu örnekte **me** kullanıcısı olarak **mycluster** birincil baş düğümüne bağlanılmaktadır:

    ssh me@mycluster-ssh.azurehdinsight.net

Kullanıcı hesabı için parola kullandıysanız, parolayı girmeniz istenir.

Parola korumalı bir SSH anahtarı kullandıysanız, parola girmeniz istenir. Aksi takdirde, SSH istemcinizdeki yerel özel anahtarlardan birini kullanarak otomatik olarak kimliği doğrulamaya çalışır.

> [!NOTE]
> SSH doğru özel anahtarla otomatik olarak kimliği doğrulamaz ise, **-i** parametresini kullanarak özel anahtarın yolunu belirtin. Aşağıdaki örnek, `~/.ssh/id_rsa` konumdan özel anahtarı yükler:
> 
> `ssh -i ~/.ssh/id_rsa me@mycluster-ssh.azurehdinsight.net`
> 
> 

Baş düğüme ilişkin adresi kullanarak bağlanıyorsanız ve bağlantı noktası belirtilmediyse SSH, HDInsight kümesinde birincil baş düğüme bağlanacak olan bağlantı noktası 22'yi varsayılan olarak atar. Bağlantı noktası 23'ü kullanırsanız ikincil baş düğüme bağlanırsınız. Baş düğümler hakkında daha fazla bilgi için bkz. [HDInsight'ta Hadoop kümelerinin kullanılabilirliği ve güvenilirliği](hdinsight-high-availability-linux.md).

### <a name="connect-to-worker-nodes"></a>Çalışan düğümüne bağlanma
Çalışan düğümlerine Azure veri merkezi dışında doğrudan erişilemez ancak SSH aracılığıyla küme baş düğümünden erişilebilir.

Kullanıcı hesabınızın kimlik doğrulaması için bir SSH anahtarı kullanıyorsanız, istemcide aşağıdaki adımları tamamlamanız gerekir:

1. Bir metin düzenleyicisiyle `~/.ssh/config` dosyasını açın. Bu dosya yoksa, terminalde `touch ~/.ssh/config` girerek oluşturabilirsiniz.
2. Aşağıdakileri dosyaya ekleyin. *CLUSTERNAME* değerini HDInsight kümenizin adıyla değiştirin.
   
        Host CLUSTERNAME-ssh.azurehdinsight.net
          ForwardAgent yes
   
    Bu, SSH aracı iletmeyi HDInsight kümeniz için yapılandırır.
3. Terminalde aşağıdaki komutu kullanarak, SSH aracı iletmeyi test edin:
   
        echo "$SSH_AUTH_SOCK"
   
    Bunun aşağıdakine benzer bilgiler döndürmesi gerekir:
   
        /tmp/ssh-rfSUL1ldCldQ/agent.1792
   
    Hiçbir şey döndürülmezse, **ssh-aracının** çalışmadığını gösterir. **ssh aracı** yükleme ve yapılandırma hakkında belirli adımlar için işletim sistemi belgelerinize başvurun veya [ssh ile ssh-aracı kullanma](http://mah.everybody.org/docs/ssh)’ya bakın.
4. **ssh aracı**nın çalıştığını doğruladıktan sonra, SSH özel anahtarınızı aracıya eklemek için aşağıdakini kullanın:
   
        ssh-add ~/.ssh/id_rsa
   
    Özel anahtarınızı farklı bir dosyada saklanıyorsa, `~/.ssh/id_rsa` ile dosyanın yolunu değiştirin.

Kümeniz için çalışan düğümüne bağlanmak üzere aşağıdaki adımları kullanın.

> [!IMPORTANT]
> Hesabınızın kimliğini doğrulamak için bir SSH anahtarı kullanıyorsanız, aracı iletmenin çalışıp çalışmadığını denetlemek için önceki adımları tamamlamanız gerekir.
> 
> 

1. Daha önce açıklandığı şekilde, SSH’yi kullanarak HDInsight kümesine bağlanın.
2. Bağlandıktan sonra, kümenizdeki düğümlerin listesini almak için aşağıdakini kullanın. *ADMINPASSWORD* değerin küme yönetici hesabınızın parolası ile değiştirin *CLUSTERNAME* değerini kümenizin adıyla değiştirin.
   
        curl --user admin:ADMINPASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/hosts
   
    Bu, her bir düğüm için uygun etki alanı adını (FQDN) içeren `host_name` dahil, kümedeki düğümler için bilgileri JSON biçiminde döndürür. Aşağıda **curl** komutuyla döndürülen bir `host_name` girişi örneği gösterilmektedir:
   
        "host_name" : "workernode0.workernode-0-e2f35e63355b4f15a31c460b6d4e1230.j1.internal.cloudapp.net"
3. Bağlanmak istediğiniz tüm çalışan düğümlerinin listesine sahip olduktan sonra, çalışan düğüme bir bağlantı açmak için SSH oturumundan sunucuya aşağıdaki komutu kullanın:
   
        ssh USERNAME@FQDN
   
    *USERNAME* değerini kendi SSH kullanıcı adınızla ve *FQDN* değerini çalışan düğümünüz için FQDN ile değiştirin. Örneğin, `workernode0.workernode-0-e2f35e63355b4f15a31c460b6d4e1230.j1.internal.cloudapp.net`.
   
   > [!NOTE]
   > SSH oturumunuzun kimliğini doğrulamak için parola kullanıyorsanız, parolayı tekrar girmeniz istenir. SSH anahtarı kullanıyorsanız, bağlantı herhangi bir soru olmadan tamamlanmalıdır.
   > 
   > 
4. Oturum kurulduktan sonra, çalışan düğümüne bağlandığınızı belirtmek için terminal istemi `username@hn#-clustername` iken `username@wk#-clustername` olarak değişir. Bu noktada çalıştırdığını tüm çalışan düğümünde çalışır.
5. Çalışan düğümünde eylemler gerçekleştirmeyi tamamladıktan sonra, çalışan düğümüne olan oturumu kapatmak için `exit` komutunu kullanın. Bu, size `username@hn#-clustername` istemini döndürür.

## <a name="connect-to-a-domainjoined-hdinsight-cluster"></a>Etki alanına katılmış HDInsight kümesine bağlanma
[Etki alanına katılmış HDInsight](hdinsight-domain-joined-introduction.md), Kerberos ile HDInsight’ta Hadoop arasında entegrasyon sağlar. SSH kullanıcısı Active Directory etki alanı kullanıcısı olmadığından bu kullanıcı hesabı etki alanına katılmış kümedeki SSH kabuğundan doğrudan Hadoop komutlarını çalıştıramaz. Önce *kinit* aracını çalıştırmanız gerekir. 

**Etki alanına katılmış HDInsight kümesinde SSH kullanarak Hive sorgularını çalıştırmak için**

1. Etki alanına katılmış bir HDInsight kümesine SSH kullanarak bağlanın.  Talimatlar için bkz. [Linux tabanlı HDInsight kümesine bağlanma](#connect-to-a-linux-based-hdinsight-cluster).
2. kinit aracını çalıştırın. Etki alanı kullanıcı adı ve etki alanı kullanıcısı parolası girmeniz istenecektir. Etki alanına katılmış HDInsight kümeleri için etki alanı kullanıcılarını yapılandırma hakkında daha fazla bilgi için bkz. [Etki alanın katılmış HDInisight kümelerini yapılandırma](hdinsight-domain-joined-configure.md).
   
    ![HDInsight Hadoop Etki alanına katılmış kinit](./media/hdinsight-hadoop-linux-use-ssh-unix/hdinsight-domain-joined-hadoop-kinit.png)
3. Şu komutları girerek Hive konsolunu açın:
   
        hive
   
    Ardından Hive komutlarını çalıştırabilirsiniz.

## <a name="add-more-accounts"></a>Daha fazla hesap ekleme
1. [SSH anahtarı oluşturma](#create-an-ssh-key-optional) bölümünde açıklandığı şekilde, yeni kullanıcı hesabı için yeni bir ortak anahtar ve özel anahtar oluşturun.
   
   > [!NOTE]
   > Özel anahtar kullanıcının kümeye bağlanmak için kullanacağı istemcide oluşturulmalı veya oluşturulduktan sonra buraya güvenle aktarılmalıdır.
   > 
   > 
2. Küme için bir SSH oturumunda, aşağıdaki komutla yeni kullanıcıyı ekleyin:
   
        sudo adduser --disabled-password <username>
   
    Bu, yeni bir kullanıcı hesabı oluşturur, ancak parola kimlik doğrulamasını iptal eder.
3. Aşağıdaki komutları kullanarak anahtarı tutmak için dizin ve dosyaları oluşturun:
   
        sudo mkdir -p /home/<username>/.ssh
        sudo touch /home/<username>/.ssh/authorized_keys
        sudo nano /home/<username>/.ssh/authorized_keys
4. Nano düzenleyici açıldığında, yeni kullanıcı hesabının ortak anahtar içeriğini kopyalayıp yapıştırın. Son olarak, **Ctrl-X**’i kullanarak dosyayı kaydedin ve düzenleyiciden çıkın.
   
    ![image of nano editor with example key](./media/hdinsight-hadoop-linux-use-ssh-unix/nano.png)
5. .ssh klasörü sahipliğini ve içeriğini yeni kullanıcı hesabıyla değiştirmek için aşağıdaki komutu kullanın:
   
        sudo chown -hR <username>:<username> /home/<username>/.ssh
6. Artık yeni kullanıcı hesabı ve özel anahtarla sunucuda kimlik doğrulaması yapabiliyor olmanız gerekir.

## <a name="a-idtunnelassh-tunneling"></a><a id="tunnel"></a>SSH tünel oluşturma
SSH, web istekleri gibi yerel istekler için HDInsight kümesine tünel oluşturmak üzere kullanılabilir. Daha sonra istek, HDInsight kümesi baş düğümünde oluşturulmuş gibi istenen kaynağa iletilir.

> [!IMPORTANT]
> SSH tüneli bazı Hadoop hizmetleri için web kullanıcı arabirimine erişmek üzere bir gereksinimdir. Örneğin, İş Geçmişi kullanıcı arabirimi veya Kaynak Yöneticisi kullanıcı arabirimine yalnızca SSH tüneli kullanılarak erişilebilir.
> 
> 

SSH tüneli oluşturma ve kullanma hakkında daha fazla bilgi için bkz. [Ambari web kullanıcı arabirimi, Kaynak Yöneticisi, İş Geçmişi, Ad Düğümü, Oozie ve diğer web kullanıcı arabirimlerine erişim için SSH tünel oluşturmayı kullanma](hdinsight-linux-ambari-ssh-tunnel.md).

## <a name="next-steps"></a>Sonraki adımlar
Artık bir SSH anahtarı kullanarak kimlik doğrulaması yapacağınızı anladığınıza göre, HDInsight’ta Hadoop ile MapReduce kullanmayı öğrenin.

* [HDInsight ile Hive kullanma](hdinsight-use-hive.md)
* [HDInsight ile Pig kullanma](hdinsight-use-pig.md)
* [HDInsight ile MapReduce işleri kullanma](hdinsight-use-mapreduce.md)

[preview-portal]: https://portal.azure.com/



<!--HONumber=Nov16_HO2-->


