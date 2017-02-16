---
title: Windows, Linux, Unix ya da OS X&quot;te HDInsight (Hadoop) ile SSH kullanma | Microsoft Belgeleri
description: " Secure Shell (SSH) kullanarak HDInsight&quot;a erişebilirsiniz. Bu belge Windows, Linux, Unix ya da OS X istemcilerinde HDInsight ile SSH kullanma hakkında bilgi sağlar."
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
ms.date: 01/12/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 279990a67ae260b09d056fd84a12160150eb4539
ms.openlocfilehash: 37409ad3f50cdd4a7a384c96a57a35ef8c83fb8f


---
# <a name="use-ssh-with-hdinsight-hadoop-from-windows-linux-unix-or-os-x"></a>Windows, Linux, Unix ya da OS X'te HDInsight (Hadoop) ile SSH kullanma

> [!div class="op_single_selector"]
> * [PuTTY (Windows)](hdinsight-hadoop-linux-use-ssh-windows.md)
> * [SSH (Windows, Linux, Unix, OS X)](hdinsight-hadoop-linux-use-ssh-unix.md)

[Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell), bir komut satırı arabirimi kullanarak Linux tabanlı HDInsight kümesinde oturum açmanızı ve komut çalıştırmanızı sağlar. Bu belge, SSH ile ilgili temel bilgilerin yanı sıra HDInsight ile SSH kullanımı hakkında ayrıntılı bilgi sağlar.

## <a name="what-is-ssh"></a>SSH nedir?

SSH, güvenli olmayan bir ağ üzerinden uzak bir sunucu ile güvenli bir şekilde iletişim kurmanızı sağlayan şifreli ağ protokolüdür. SSH, uzak bir sunucuyla güvenli komut satırı oturumu kurmak için kullanılır. Bu durumda uzak sunucu, bir HDInsight kümesinin baş düğümleri veya kenar düğümüdür.

SSH’ı istemcinizle HDInsight kümesi arasındaki ağ trafiği için bir tünel oluşturmak amacıyla da kullanabilirsiniz. Tünek kullanarak HDInsight kümesinde mevcut olan ancak internete açık olmayan hizmetlere erişebilirsiniz. HDInsight ile SSH tüneli oluşturma hakkında daha fazla bilgi için bkz. [HDInsight ile SSH tüneli oluşturma](hdinsight-linux-ambari-ssh-tunnel.md).

## <a name="ssh-clients"></a>SSH istemcileri

Birçok işletim sistemi `ssh` ve `scp` komut satırı yardımcı programları aracılığıyla SSH istemcisi işlevini sunmaktadır.

* __ssh__: Uzak komut satırı oturumu açmak ve tünel oluşturmak için kullanılabilen genel SSH istemcisi.
* __scp__: SSH protokolünü kullanarak yerel sistemlerle uzak sistemler arasında dosya kopyalama imkanı sunan yardımcı program.

Windows, Windows 10 Yıldönümü Sürümü’ne kadar SSH istemcisine sahip değildi. Windows’un bu sürümünde yer alan Bash on Windows 10 özelliği, geliştiricilere `ssh`, `scp` ve diğer Linux komutlarını sunmaktadır. Bash on Windows 10’u kullanma hakkında daha fazla bilgi için bkz. [Windows’da Ubuntu Bash](https://msdn.microsoft.com/commandline/wsl/about).

Windows kullanıyorsanız ancak Bash on Windows 10’a erişiminiz yoksa aşağıdaki SSH istemcisi önerilerinden faydalanabilirsiniz:

* [Git For Windows](https://git-for-windows.github.io/): `ssh` ve `scp` komut satırı yardımcı programlarını sunar.
* [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/): Grafik arayüzlü SSH istemcisi sunar.
* [MobaXterm](http://mobaxterm.mobatek.net/): Grafik arayüzlü SSH istemcisi sunar.
* [Cygwin](https://cygwin.com/): `ssh` ve `scp` komut satırı yardımcı programlarını sunar.

> [!NOTE]
> Bu belgedeki adımlarda `ssh` komutuna erişiminiz olduğu kabul edilmiştir. puTTY veya MobaXterm gibi bir istemci kullanıyorsanız, eşdeğer komutlar ve parametreler için ilgili ürünün belgelerini inceleyin.

## <a name="ssh-authentication"></a>SSH Kimlik Doğrulaması

SSH bağlantılarında kimlik doğrulaması parola veya [ortak anahtar şifrelemesi (https://en.wikipedia.org/wiki/Public-key_cryptography)](https://en.wikipedia.org/wiki/Public-key_cryptography) ile gerçekleştirilebilir. Parolaların maruz kaldığı birçok saldırı türüne karşı korumalı olduğu için anahtar en güvenli seçenektir. Ancak anahtar oluşturmak ve yönetmek, parola kullanmaktan daha karmaşık bir işlemdir.

Ortak anahtar şifrelemesini kullanmak için _ortak_ ve _özel_ anahtar çifti oluşturmanız gerekir.

* **Ortak anahtar**, HDInsight kümenizin düğümlerine veya ortak anahtar şifrelemesi ile kullanmak istediğiniz diğer hizmetlere yüklenir.

* **Özel anahtar**, SSH istemcisi kullanarak HDInsight kümesinde oturum açtığınızda kimliğinizi doğrulamak için sunduğunuz bilgidir. Bu özel anahtarı koruyun. Özel anahtarı paylaşmayın.

    Özel anahtar için bir parola oluşturarak ek güvenlik katmanı ekleyebilirsiniz. Anahtarın kullanılabilmesi için bu parolanın sağlanması gerekir.

### <a name="create-a-public-and-private-key"></a>Ortak ve özel anahtar oluşturma

`ssh-keygen` yardımcı programı, HDInsight ile kullanmak üzere ortak ve özel anahtar çifti oluşturmanın en kolay yoludur. HDInsight ile kullanmak üzere yeni bir anahtar çifti oluşturmak için komut satırından aşağıdaki komutu kullanın:

> [!NOTE]
> MobaXTerm veya puTTY gibi grafik arayüze sahip bir GUI SSH istemcisi kullanıyorsanız, anahtar oluşturma talimatları için istemcinizin belgelerine bakın.

    ssh-keygen -t rsa -b 2048

Şu bilgiler istenecektir:

* Dosya konumu: Varsayılan değer `~/.ssh/id_rsa` olarak belirlenmiştir.

* İsteğe bağlı parola: Buraya parola girerseniz HDInsight kümenizdeki kimlik doğrulaması sırasında aynı parolayı tekrar girmeniz gerekir.

> [!IMPORTANT]
> Parola, özel anahtarın şifresidir. Kimlik doğrulaması için özel anahtarı kullanmadan önce bu parolayı girmeniz gerekir. Özel anahtarınızın ele geçirilmesi durumunda parola olmadan kullanılması mümkün olmayacaktır.
>
> Ancak parolayı unutursanız sıfırlama veya kurtarma imkanınız olmayacaktır.

Komut bittikten sonra iki yeni dosya oluşturulur:

* __id\_rsa__: Bu dosya özel anahtarı içerir.

    > [!WARNING]
    > Güvenliği ortak anahtarla sağlanan hizmetlere yetkisiz erişimi önlemek için bu dosyaya erişimi kısıtlamanız gerekir.

* __id\_rsa.pub__: Bu dosya ortak anahtarı içerir. HDInsight kümesi oluştururken kullanmanız gereken dosya budur.

    > [!NOTE]
    > _Ortak_ anahtara başkaları tarafından erişim sağlanması önemli değildir. Ortak anahtarın yapabileceği tek şey, özel anahtarı doğrulamaktır. SSH sunucusu gibi hizmetler, özel anahtarla kimlik doğrulaması gerçekleştirdiğinizde kimliğinizi doğrulamak için ortak anahtarı kullanır.

## <a name="configure-ssh-on-hdinsight"></a>HDInsight üzerinde SSH’ı yapılandırma

Linux tabanlı HDInsight kümesi oluşturduğunuzda _SSH kullanıcı adı_ ile _parola_ veya _ortak anahtar_ girmeniz gerekir. Bu bilgiler, küme oluşturma işlemi sırasında HDInsight küme düğümleri üzerinde oturum açma bilgileri oluşturmak için kullanılır. Kullanıcı hesabının güvenliğini sağlamak için parola veya ortak anahtar kullanılır.

Küme oluşturma sırasında SSH yapılandırma hakkında daha fazla bilgi için aşağıdaki belgeleri inceleyin:

* [Azure portalını kullanarak HDInsight oluşturma](hdinsight-hadoop-create-linux-clusters-portal.md)
* [Azure CLI kullanarak HDInsight oluşturma](hdinsight-hadoop-create-linux-clusters-azure-cli.md)
* [Azure PowerShell kullanarak HDInsight oluşturma](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
* [Azure Resource Manager şablonlarını kullanarak HDInsight oluşturma](hdinsight-hadoop-create-linux-clusters-arm-templates.md)
* [.NET SDK kullanarak HDInsight oluşturma](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)
* [REST kullanarak HDInsight oluşturma](hdinsight-hadoop-create-linux-clusters-curl-rest.md)

### <a name="additional-ssh-users"></a>Ek SSH kullanıcıları

Küme oluşturulduktan sonra SSH kullanıcıları eklenebilir ancak bu işlem önerilmez.

* Yeni SSH kullanıcılarını kümedeki her düğüme el ile eklemeniz gerekir.

* Yeni SSH kullanıcıları, varsayılan kullanıcıyla aynı HDInsight erişimine sahip olur. HDInsight üzerindeki verilere veya işlere erişimi SSH kullanıcı hesabına göre kısıtlama imkanı yoktur.

Erişimi kullanıcı tabanlı olarak kısıtlamak için etki alanına katılmış HDInsight kümesi kullanmanız gerekir. Etki alanına katılmış HDInsight, küme kaynaklarında erişim denetimi gerçekleştirmek için Active Directory hizmetini kullanır.

Etki alanına katılmış HDInsight kümesi kullandığınızda, SSH ile bağlantı kurduktan sonra Active Directory ile kimlik doğrulaması yapabilirsiniz. Birden fazla kullanıcı SSH kullanarak bağlandıktan sonra Active Directory hizmetiyle kimlik doğrulaması gerçekleştirebilir. Daha fazla bilgi için [Etki alanına katılmış HDInsight](#domainjoined) bölümüne bakın.

##<a name="a-idconnecta-connect-to-hdinsight"></a><a id="connect"></a> HDInsight’a bağlanma

Bir HDInsight kümesindeki tüm düğümler SSH sunucusunu çalıştırsa da genel internet üzerinden yalnızca baş veya kenar düğümlerine bağlanabilirsiniz.

* _Baş düğümlerine_ bağlanmak için `CLUSTERNAME-ssh.azurehdinsight.net` komutunu kullanın. Burada __CLUSTERNAME__, HDInsight kümesinin adıdır. 22 numaralı bağlantı noktasından (SSH için varsayılan ayar) bağlandığınızda birincil baş düğümüne bağlanmış olursunuz. 23 numaralı bağlantı noktası, ikincil baş düğümüne bağlanır.

* _Kenar düğümüne_ bağlanmak için `EDGENAME.CLUSTERNAME-ssh.azurehdinsight.net` komutunu kullanın. Burada __EDGENAME__ kenar düğümünün adı, __CLUSTERNAME__ ise HDInsight kümesinin adıdır. Kenar düğümüne bağlanırken 22 numaralı bağlantı noktasını kullanın.

Aşağıdaki örneklerde, __myhdi__ adlı bir kümenin baş düğümlerine ve kenar düğümüne __sshuser__ adlı SSH kullanıcısı ile bağlanma adımları gösterilmektedir. Kenar düğümün adı __myedge__ olarak belirlenmiştir.

| Bunu yapmak için... | Bunu kullanın... |
| ----- | ----- |
| Birincil baş düğümüne bağlanma | `ssh sshuser@myhdi-ssh.azurehdinsight.net` |
| İkincil baş düğümüne bağlanma | `ssh -p 23 sshuser@myhdi-ssh.azurehdinsight.net` |
| Kenar düğümüne bağlanma | `ssh sshuser@edge.myhdi-ssh.azurehdinsight.net` |

SSH hesabının güvenliğini sağlamak için parola kullanıyorsanız bu bilgiyi girmeniz istenir.

SSH hesabının güvenliğini sağlamak için ortak anahtar kullanıyorsanız, `-i` anahtarını kullanarak eşleşen özel anahtarın yolunu belirtmeniz gerekebilir. Aşağıdaki örnekte `-i` anahtarının kullanım şekli gösterilmiştir:

    ssh -i /path/to/public.key sshuser@myhdi-ssh.azurehdinsight.net

### <a name="connect-to-other-nodes"></a>Diğer düğümlere bağlanma

Çalışan düğümlerine ve Zookeeper düğümlerine kümenin dışından doğrudan erişim sağlamak mümkün değildir. Ancak bu düğümlere kümenin baş düğümlerinden veya kenar düğümlerinden erişebilirsiniz. Bunu yapmak için uygulamanız gereken genel adımlar şunlardır:

1. SSH kullanarak bir baş veya kenar düğümüne bağlanın:

        ssh sshuser@myhdi-ssh.azurehdinsight.net

2. Baş veya kenar düğümüne yaptığınız SSH bağlantısında `ssh` komutunu kullanarak kümedeki bir çalışan düğümüne bağlanın:

        ssh sshuser@wn0-myhdi

    Kümedeki çalışan düğümlerinin listesini almak için [Ambari REST API’yi kullanarak HDInsight yönetimi yapma](hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-fqdn-of-cluster-nodes) belgesindeki küme adlarının tam etki alanı adını alma örneğini inceleyin.

SSH hesabı parolayla korunuyorsa, ilgili parolayı girdikten sonra bağlantı kurulur.

Hesabınızda kimlik doğrulaması için SSH anahtarı kullanıyorsanız, yerel ortamınızda SSH aracısı iletme yapılandırması gerçekleştirildiğinden emin olun.

> [!IMPORTANT]
> Aşağıdaki adımlar, Linux/UNIX tabanlı sistem için geçerlidir ve Bash on Windows 10 ile birlikte çalışır. Bu adımlar sisteminizde çalışmazsa SSH istemcinizin belgelerine bakmanız gerekebilir.

1. Bir metin düzenleyicisiyle `~/.ssh/config` dosyasını açın. Bu dosya yoksa, komut satırında `touch ~/.ssh/config` girerek oluşturabilirsiniz.

2. Aşağıdakileri dosyaya ekleyin. *CLUSTERNAME* değerini HDInsight kümenizin adıyla değiştirin.

        Host CLUSTERNAME-ssh.azurehdinsight.net
          ForwardAgent yes

    Bu giriş, SSH aracı iletmeyi HDInsight kümeniz için yapılandırır.

3. Terminalde aşağıdaki komutu kullanarak, SSH aracı iletmeyi test edin:

        echo "$SSH_AUTH_SOCK"

    Bu komutun aşağıdaki metne benzer bilgiler döndürmesi gerekir:

        /tmp/ssh-rfSUL1ldCldQ/agent.1792

    Hiçbir şeyin döndürülmemesi, `ssh-agent` özelliğinin çalışmadığını gösterir. Aracı başlatma komut dosyası bilgileri için [ssh ile ssh-agent kullanma (http://mah.everybody.org/docs/ssh)](http://mah.everybody.org/docs/ssh) sayfasını inceleyin veya SSH istemcinizin `ssh-agent` yükleme ve yapılandırma adımlarına başvurun.

4. **ssh aracı**nın çalıştığını doğruladıktan sonra, SSH özel anahtarınızı aracıya eklemek için aşağıdakini kullanın:

        ssh-add ~/.ssh/id_rsa

    Özel anahtarınızı farklı bir dosyada saklanıyorsa, `~/.ssh/id_rsa` ile dosyanın yolunu değiştirin.

<a id="domainjoined"></a>
### <a name="domain-joined-hdinsight"></a>Etki alanına katılmış HDInsight

[Etki alanına katılmış HDInsight](hdinsight-domain-joined-introduction.md), Kerberos ile HDInsight’ta Hadoop arasında entegrasyon sağlar. SSH kullanıcısı bir Active Directory etki alanı kullanıcısı olmadığı için Active Directory kimlik doğrulamasından geçene kadar Hadoop komutlarını çalıştıramazsınız. SSH oturumunuzda Active Directory ile kimlik doğrulaması yapmak için aşağıdaki adımları kullanın:

1. SSH kullanarak etki alanına katılmış HDInsight kümesine bağlanmak için [HDInsight bağlantısı yapma](#connect) bölümündeki adımları uygulayın. Örneğin, aşağıdaki komut __myhdi__ adlı HDInsight kümesine __sshuser__ adlı SSH hesabını kullanarak bağlanmanızı sağlar.

        ssh sshuser@myhdi-ssh.azurehdinsight.net

2. Etki alanı kullanıcısı ve parolası ile kimlik doğrulaması yapmak için aşağıdaki komutu kullanın:

        kinit

     İstendiğinde etki alanı kullanıcısı için etki alanı kullanıcı adını ve parolasını girin.

    Etki alanına katılmış HDInsight kümeleri için etki alanı kullanıcılarını yapılandırma hakkında daha fazla bilgi için bkz. [Etki alanın katılmış HDInisight kümelerini yapılandırma](hdinsight-domain-joined-configure.md).

`kinit` komutunu kullanarak kimlik doğrulaması gerçekleştirdikten sonra `hdfs dfs -ls /` veya `hive` gibi Hadoop komutlarını kullanabilirsiniz.

## <a name="a-idtunnelassh-tunneling"></a><a id="tunnel"></a>SSH tünel oluşturma

SSH, web istekleri gibi yerel istekler için HDInsight kümesine tünel oluşturmak üzere kullanılabilir. Daha sonra istek, HDInsight kümesi baş düğümünde oluşturulmuş gibi istenen kaynağa iletilir.

> [!IMPORTANT]
> SSH tüneli bazı Hadoop hizmetleri için web kullanıcı arabirimine erişmek üzere bir gereksinimdir. Örneğin, İş Geçmişi kullanıcı arabirimi veya Kaynak Yöneticisi kullanıcı arabirimine yalnızca SSH tüneli kullanılarak erişilebilir.

SSH tüneli oluşturma ve kullanma hakkında daha fazla bilgi için bkz. [Ambari web kullanıcı arabirimi, JobHistory, NameNode, Oozie ve diğer web kullanıcı arabirimlerine erişim için SSH tünel oluşturmayı kullanma](hdinsight-linux-ambari-ssh-tunnel.md).

## <a name="next-steps"></a>Sonraki adımlar

Artık bir SSH anahtarı kullanarak kimlik doğrulaması yapacağınızı anladığınıza göre, HDInsight’ta Hadoop ile MapReduce kullanmayı öğrenin.

* [HDInsight ile Hive kullanma](hdinsight-use-hive.md)
* [HDInsight ile Pig kullanma](hdinsight-use-pig.md)
* [HDInsight ile MapReduce işleri kullanma](hdinsight-use-mapreduce.md)

[preview-portal]: https://portal.azure.com/



<!--HONumber=Jan17_HO3-->


