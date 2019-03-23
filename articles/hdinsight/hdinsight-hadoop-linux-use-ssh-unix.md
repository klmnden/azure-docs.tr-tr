---
title: SSH’yi Hadoop - Azure HDInsight ile Kullanma
description: Secure Shell (SSH) kullanarak HDInsight'a erişebilirsiniz. Bu belgede; Windows, Linux, Unix veya macOS istemcilerinden ssh ve scp komutlarını kullanarak HDInsight’a bağlanmaya ilişkin bilgi sağlanmıştır.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
keywords: linux’taki hadoop komutları,hadoop linux komutları,hadoop macos,ssh hadoop,ssh hadoop kümesi
ms.service: hdinsight
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: hrasheed
ms.custom: H1Hack27Feb2017,hdinsightactive,hdiseo17may2017
ms.openlocfilehash: 82f57701a2ba83d500747383d49bbefaa23877f2
ms.sourcegitcommit: 223604d8b6ef20a8c115ff877981ce22ada6155a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58361226"
---
# <a name="connect-to-hdinsight-apache-hadoop-using-ssh"></a>SSH kullanarak HDInsight için (Apache Hadoop) bağlanma

Nasıl kullanacağınızı öğrenin [güvenli Kabuk (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) Azure HDInsight üzerinde Apache hadoop'a güvenli bir şekilde bağlanmak için. 

HDInsight, Hadoop kümesi içindeki düğümler için işletim sistemi olarak Linux’ı (Ubuntu) kullanabilir. Aşağıdaki tablo, SSH istemcisi kullanılarak Linux tabanlı HDInsight’a bağlanılırken gereken adres ve bağlantı noktası bilgilerini içerir:

| Adres | Bağlantı noktası | Bağlandığı yer... |
| ----- | ----- | ----- |
| `<clustername>-ed-ssh.azurehdinsight.net` | 22 | Kenar düğümü (HDInsight üzerinde ML Services) |
| `<edgenodename>.<clustername>-ssh.azurehdinsight.net` | 22 | Kenar düğümü (bir kenar düğümü varsa diğer küme türleri) |
| `<clustername>-ssh.azurehdinsight.net` | 22 | Birincil baş düğüm |
| `<clustername>-ssh.azurehdinsight.net` | 23 | İkincil baş düğüm |

> [!NOTE]  
> `<edgenodename>` ifadesini kenar düğümünün adıyla değiştirin.
>
> `<clustername>` değerini kümenizin adıyla değiştirin.
>
> Kümeniz bir kenar düğümü içeriyorsa __kenar düğümüne her zaman SSH’yi kullanarak bağlanmanızı__ öneririz. Baş düğümler, Hadoop’un sistem durumu için kritik öneme sahip olan hizmetleri barındırır. Kenar düğümü yalnızca üzerine yerleştirdiğiniz öğeleri çalıştırır.
>
> Kenar düğümlerini kullanma hakkında daha fazla bilgi için bkz. [HDInsight’ta kenar düğümlerini kullanma](hdinsight-apps-use-edge-node.md#access-an-edge-node).

> [!TIP]  
> HDInsight'a ilk kez bağlandığınızda SSH istemciniz konağın orijinalliğinin belirlenemediği yönünde bir uyarı görüntüleyebilir. Konağı SSH istemcinizin güvenilen sunucular listesine eklemek isteyip istemediğiniz sorulduğunda "Evet"i seçin.
>
> Daha önceden aynı ada sahip bir sunucuya bağlandıysanız konak anahtarının sunucu konak anahtarıyla eşleşmediğine dair bir uyarı alabilirsiniz. Var olan sunucu adı girişini kaldırma talimatları için SSH istemcinizin belgelerine bakın.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="ssh-clients"></a>SSH istemcileri

Linux, Unix ve macOS sistemleri `ssh` ve `scp` komutlarını sağlar. `ssh` istemcisi, yaygın olarak Linux veya Unix tabanlı bir sistemle uzak komut satırı oturumu oluşturmak için kullanılır. `scp` istemcisi, dosyaları istemciniz ve uzak sistem arasında güvenli bir şekilde kopyalamak için kullanılır.

Microsoft Windows, hiçbir SSH istemcisini varsayılan olarak yüklemez. `ssh` ve `scp` istemcileri, aşağıdaki paketler aracılığıyla Windows için kullanılabilir:

* OpenSSH istemcisi (Beta): Fall Creators Update'te Git __ayarları__ > __uygulamalar ve Özellikler__ > __isteğe bağlı özellikleri Yönet__  >  __Özellik ekleme__ seçip __OpenSSH istemcisi__. 

    > [!NOTE]  
    > Bu özellik etkinleştirildikten sonra PowerShell'de `ssh` ve `scp` komutları kullanılamıyorsa, oturumu kapatın ve yeniden açın.

* [Windows 10 üzerinde ubuntu'da bash](https://msdn.microsoft.com/commandline/wsl/about): `ssh` Ve `scp` komutları, Windows komut satırında Bash aracılığıyla kullanılabilir.

* [OpenSSH istemcisi (beta)](https://blogs.msdn.microsoft.com/powershell/2017/12/15/using-the-openssh-beta-in-windows-10-fall-creators-update-and-windows-server-1709/): Bu, Windows 10 Fall Creators Update içinde sunulan isteğe bağlı bir özelliktir.

* [Azure Cloud Shell'i](../cloud-shell/quickstart.md): Cloud Shell, tarayıcınızda bir Bash ortamı sunar ve sağlar `ssh`, `scp`ve diğer sık kullanılan Linux komutlarını.

* [Git (https://git-scm.com/)](https://git-scm.com/): `ssh` Ve `scp` komutlarını GitBash komut satırıyla kullanılabilir.

Ayrıca [PuTTY (https://www.chiark.greenend.org.uk/~sgtatham/putty/)](https://www.chiark.greenend.org.uk/~sgtatham/putty/) ve [MobaXterm (https://mobaxterm.mobatek.net/)](https://mobaxterm.mobatek.net/) gibi birkaç grafik SSH istemcisi bulunur. Bu istemciler HDInsight’a bağlanmak için kullanılabilse de bağlanma işlemi `ssh` yardımcı programını kullanmaktan farklıdır. Daha fazla bilgi için, kullanmakta olduğunuz grafiksel istemcinin belgelerine bakın.

## <a id="sshkey"></a>Kimlik doğrulaması: SSH anahtarları

SSH anahtarları, SSH oturumlarının kimliğini doğrulamak için [Ortak anahtar şifrelemesi](https://en.wikipedia.org/wiki/Public-key_cryptography) kullanır. SSH anahtarları parolalara göre daha güvenlidir ve Hadoop kümenize erişimin güvenliğini sağlamak için kolay bir yol sağlar.

SSH hesabınızın güvenliği bir anahtar yardımıyla sağlanıyorsa, bağlantı kurduğunuzda istemci eşleşen özel anahtarı sağlamalıdır:

* Çoğu istemci, bir __varsayılan anahtar__ kullanmak üzere yapılandırılmıştır. Örneğin, `ssh` istemcisi Linux ve Unix ortamlarında `~/.ssh/id_rsa` üzerinde bir özel anahtar arar.

* __Özel anahtarın yolunu__ belirtebilirsiniz. `ssh` istemcisi ile özel anahtarın yolunu belirtmek için `-i` parametresi kullanılır. Örneğin, `ssh -i ~/.ssh/id_rsa sshuser@myedge.mycluster-ssh.azurehdinsight.net`.

* Farklı sunucularla kullanılacak __birden fazla özel anahtarınız__ varsa [ssh-agent (https://en.wikipedia.org/wiki/Ssh-agent)](https://en.wikipedia.org/wiki/Ssh-agent) gibi bir yardımcı program kullanmayı deneyin. `ssh-agent` yardımcı programı, SSH oturumu oluşturulurken kullanılacak anahtarı otomatik olarak seçmek için kullanılabilir.

> [!IMPORTANT]  
> Özel anahtarınızın güvenliğini şifre ile sağlıyorsanız, anahtarı kullanmak için şifreyi girmeniz gerekir. `ssh-agent` gibi yardımcı programlar, size kolaylık sağlamak için parolayı önbelleğe alabilir.

### <a name="create-an-ssh-key-pair"></a>SSH anahtar çifti oluşturma

Ortak ve özel anahtar dosyaları oluşturmak için `ssh-keygen` komutunu kullanın. Aşağıdaki komut, HDInsight ile kullanılabilecek bir 2048-bit RSA anahtar çifti oluşturur:

    ssh-keygen -t rsa -b 2048

Anahtar oluşturma işlemi sırasında sizden bilgiler istenir. Örneğin, anahtarların nerede depolanacağı veya şifre kullanılıp kullanılmayacağı. İşlem tamamlandıktan sonra biri ortak anahtar, diğeri özel anahtar olmak üzere iki dosya oluşturulur.

* __Ortak anahtar__ bir HDInsight kümesi oluşturmak için kullanılır. Ortak anahtar `.pub` uzantısına sahiptir.

* __Özel anahtar__, HDInsight kümesinde istemcinizin kimliğini doğrulamak için kullanılır.

> [!IMPORTANT]  
> Anahtarlarınızın güvenliğini şifre ile sağlayabilirsiniz. Parola, aslında özel anahtarınız üzerindeki bir şifredir. Özel anahtarınız başkası tarafından ele geçirilirse, anahtarın kullanılması için şifrenin girilmesi gerekir.

### <a name="create-hdinsight-using-the-public-key"></a>Ortak anahtar kullanarak HDInsight oluşturma

| Oluşturma yöntemi | Ortak anahtarı kullanma |
| ------- | ------- |
| **Azure portal** | __Küme oturumu açmak için kullanılan parolayı kullan__ seçeneğinin işaretini kaldırın ve ardından SSH kimlik doğrulama türü olarak __Ortak Anahtar__’ı seçin. Son olarak, ortak anahtar dosyasını seçin veya dosyanın metin içeriğini __SSH ortak anahtarı__ alanına yapıştırın.</br>![HDInsight küme oluşturma işleminde SSH ortak anahtarı iletişim kutusu](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-public-key.png) |
| **Azure PowerShell** | `New-AzHdinsightCluster` cmdlet'inin `-SshPublicKey` parametresini kullanarak, ortak anahtarın içeriğini dize olarak geçirin.|
| **Klasik Azure CLI** | `azure hdinsight cluster create` komutunun `--sshPublicKey` parametresini kullanarak, ortak anahtarın içeriğini dize olarak geçirin. |
| **Resource Manager Şablonu** | SSH anahtarlarını şablonla kullanma örneği için bkz. [HDInsight’ı SSH anahtarı ile Linux’a dağıtma](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-publickey/). [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-publickey/azuredeploy.json) dosyasında `publicKeys` öğesi, kümeyi oluştururken Azure’a anahtarları geçirmek için kullanılır. |

## <a id="sshpassword"></a>Kimlik doğrulaması: Parola

SSH hesaplarının güvenliği bir parola kullanılarak sağlanabilir. SSH kullanarak HDInsight’a bağlandığınızda, parola girmeniz istenir.

> [!WARNING]  
> Microsoft, SSH için parola kimlik doğrulamasının kullanılmasını önermez. Parolalar tahmin edilebilir ve deneme yanılma saldırılarına karşı savunmasızdır. Bunun yerine, [kimlik doğrulaması için SSH anahtarları](#sshkey) kullanmanız önerilir.

> [!IMPORTANT]  
> SSH hesabı parolasının, HDInsight kümesi oluşturulduktan 70 gün sonra süresi dolar. Parolanızın süresi dolarsa, [HDInsight’ı yönetme](hdinsight-administer-use-portal-linux.md#change-passwords) belgesindeki bilgileri kullanarak değiştirebilirsiniz.

### <a name="create-hdinsight-using-a-password"></a>Parola kullanarak HDInsight oluşturma

| Oluşturma yöntemi | Parola belirtme |
| --------------- | ---------------- |
| **Azure portal** | Varsayılan olarak, SSH kullanıcı hesabı ile küme oturum açma hesabı aynı parolaya sahiptir. Farklı bir parola kullanmak için __Küme oturumu açmak için kullanılan parolayı kullan__ seçeneğinin işaretini kaldırın ve __SSH parolası__ alanına parolayı girin.</br>![HDInsight küme oluşturma işleminde SSH parolası iletişim kutusu](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-password.png)|
| **Azure PowerShell** | `New-AzHdinsightCluster` cmdlet’inin `--SshCredential` parametresini kullanın ve SSH kullanıcı hesabı adı ile parolasını içeren bir `PSCredential` nesnesi geçirin. |
| **Klasik Azure CLI** | `azure hdinsight cluster create` komutunun `--sshPassword` parametresini kullanarak parola değerini belirtin. |
| **Resource Manager Şablonu** | Parolayı şablonla kullanma örneği için bkz. [HDInsight’ı SSH parolası ile Linux’a dağıtma](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-password/). [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-password/azuredeploy.json) dosyasındaki `linuxOperatingSystemProfile` öğesi, kümeyi oluştururken SSH hesabı adı ile parolasını Azure’a geçirmek için kullanılır.|

### <a name="change-the-ssh-password"></a>SSH parolasını değiştirme

SSH kullanıcı hesabı parolasını değiştirme hakkında bilgi için, [HDInsight’ı Yönetme](hdinsight-administer-use-portal-linux.md#change-passwords) belgesinin __Parolaları değiştirme__ bölümüne bakın.

## <a id="domainjoined"></a>Kimlik doğrulaması: Etki alanına katılmış HDInsight

__Etki alanına katılmış HDInsight kümesi__ kullanıyorsanız, SSH yerel kullanıcısı ile bağlantı kurduktan sonra `kinit` komutunu kullanmanız gerekir. Bu komut sizden bir etki alanı kullanıcı adı ile parolası ister ve kümenizle ilişkili Azure Active Directory etki alanını kullanarak oturumunuzun kimliğini doğrular.

Etki alanı hesabıyla SSH kullanmak için etki alanına katılmış olan tüm düğümlerde (baş düğüm, uç düğüm gibi) Kerberos Kimlik Doğrulamasını etkinleştirebilirsiniz. Bunu yapmak için sshd config dosyasını düzenleyin:
```bash
sudo vi /etc/ssh/sshd_config
```
açıklamayı kaldırın ve `KerberosAuthentication` değerini `yes` olarak değiştirin

```bash
sudo service sshd restart
```

Dilediğiniz zaman Kerberos kimlik doğrulamasının başarılı olup olmadığını doğrulamak için `klist` komutunu kullanabilirsiniz.

Daha fazla bilgi için bkz. [Etki alanına katılmış HDInsight yapılandırma](./domain-joined/apache-domain-joined-configure.md).

## <a name="connect-to-nodes"></a>Düğümlere bağlanma

Baş düğümlere ve (varsa) kenar düğümüne İnternet üzerinden 22 ve 23 numaralı bağlantı noktalarıyla erişilebilir.

* __Baş düğümlere__ bağlanırken, birincil baş düğüme bağlanmak için __22__, ikincil baş düğüme bağlanmak için __23__ numaralı bağlantı noktasını kullanın. Kullanılacak tam etki alanı adı `clustername-ssh.azurehdinsight.net`‘tir, burada `clustername` kümenizin adıdır.

    ```bash
    # Connect to primary head node
    # port not specified since 22 is the default
    ssh sshuser@clustername-ssh.azurehdinsight.net

    # Connect to secondary head node
    ssh -p 23 sshuser@clustername-ssh.azurehdinsight.net
    ```
    
* __Kenar düğümüne__ bağlanırken 22 numaralı bağlantı noktasını kullanın. Kullanılacak tam etki alanı adı `edgenodename.clustername-ssh.azurehdinsight.net`‘tir, burada `edgenodename` kenar düğümü oluştururken girdiğiniz addır. `clustername`, kümenin adıdır.

    ```bash
    # Connect to edge node
    ssh sshuser@edgnodename.clustername-ssh.azurehdinsight.net
    ```

> [!IMPORTANT]  
> Önceki örneklerde, parola ile kimlik doğrulaması kullandığınız veya sertifika kimlik doğrulamasının otomatik olarak yapıldığı varsayılır. Kimlik doğrulaması için bir SSH anahtar çifti kullanıyorsanız ve sertifika otomatik olarak kullanılmıyorsa, özel anahtarı belirtmek için `-i` parametresini kullanın. Örneğin, `ssh -i ~/.ssh/mykey sshuser@clustername-ssh.azurehdinsight.net`.

Bağlantı kurulduktan sonra istem SSH kullanıcı adını ve bağlandığınız düğüm belirtecek şekilde değiştir. Örneğin, `sshuser` olarak birincil baş düğüme bağlıyken komut istemi `sshuser@hn0-clustername:~$` değerini gösterir.

### <a name="connect-to-worker-and-apache-zookeeper-nodes"></a>Çalışan ve Apache Zookeeper düğümlerine bağlanma

Çalışan düğümlerine ve Zookeeper düğümlerine doğrudan internetten erişilemez. Bunlara, küme baş düğümleri veya kenar düğümlerinden erişilebilir. Diğer düğümlere bağlanmak için uygulamanız gereken genel adımlar şunlardır:

1. SSH kullanarak bir baş veya kenar düğümüne bağlanın:

        ssh sshuser@myedge.mycluster-ssh.azurehdinsight.net

2. Baş veya kenar düğümüne yaptığınız SSH bağlantısında `ssh` komutunu kullanarak kümedeki bir çalışan düğümüne bağlanın:

        ssh sshuser@wn0-myhdi

    Düğüm adlarının listesini almak için bkz: [yönetme Apache Ambari REST API'yi kullanarak HDInsight](hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-fqdn-of-cluster-nodes) belge.

SSH hesabının güvenliği bir __parola__ kullanılarak sağlanıyorsa bağlanırken parolayı girin.

SSH hesabının güvenliği __SSH anahtarları__ kullanılarak sağlanıyorsa istemciden SSH iletmenin etkinleştirildiğinden emin olun.

> [!NOTE]  
> Kümedeki tüm düğümlere doğrudan erişmenin başka bir yolu ise HDInsight’ın bir Azure Sanal Ağına bağlanmasıdır. Bundan sonra, uzak makinenizi aynı sanal ağa bağlayabilir ve kümedeki tüm düğümlere doğrudan erişebilirsiniz.
>
> Daha fazla bilgi için bkz. [HDInsight ile sanal ağ kullanma](hdinsight-extend-hadoop-virtual-network.md).

### <a name="configure-ssh-agent-forwarding"></a>SSH aracı iletmeyi yapılandırma

> [!IMPORTANT]  
> Aşağıdaki adımlar, Linux veya UNIX tabanlı bir sistem için geçerlidir ve Windows 10 üzerinde Bash ile birlikte çalışır. Bu adımlar sisteminizde çalışmazsa SSH istemcinizin belgelerine bakmanız gerekebilir.

1. Bir metin düzenleyicisiyle `~/.ssh/config` dosyasını açın. Bu dosya yoksa, komut satırında `touch ~/.ssh/config` girerek oluşturabilirsiniz.

2. Aşağıdakileri metni `config` dosyasına ekleyin.

        Host <edgenodename>.<clustername>-ssh.azurehdinsight.net
          ForwardAgent yes

    __Ana bilgisayar__ bilgisini, SSH kullanarak bağlandığınız düğümün adresiyle değiştirin. Önceki örnekte kenar düğümü kullanılmıştır. Bu giriş, belirtilen düğüm için SSH aracı iletmeyi yapılandırır.

3. Terminalde aşağıdaki komutu kullanarak, SSH aracı iletmeyi test edin:

        echo "$SSH_AUTH_SOCK"

    Bu komutun aşağıdaki metne benzer bilgiler döndürmesi gerekir:

        /tmp/ssh-rfSUL1ldCldQ/agent.1792

    Hiçbir şeyin döndürülmemesi, `ssh-agent` özelliğinin çalışmadığını gösterir. Daha fazla bilgi için [ssh ile ssh-agent kullanma (http://mah.everybody.org/docs/ssh)](http://mah.everybody.org/docs/ssh) sayfasındaki aracı başlatma betikleri bilgilerine bakın veya SSH istemcinizin belgelerine başvurun.

4. **ssh aracı**nın çalıştığını doğruladıktan sonra, SSH özel anahtarınızı aracıya eklemek için aşağıdakini kullanın:

        ssh-add ~/.ssh/id_rsa

    Özel anahtarınızı farklı bir dosyada saklanıyorsa, `~/.ssh/id_rsa` ile dosyanın yolunu değiştirin.

5. SSH kullanarak küme kenar düğümüne veya baş düğümlerine bağlanın. Ardından, SSH komutunu kullanarak bir çalışan veya zookeeper düğümüne bağlanın. İletilen anahtar kullanılarak bağlantı kurulur.

## <a name="copy-files"></a>Dosyaları kopyalama

`scp` yardımcı programı, kümedeki bireysel düğümlerde gelen ve giden dosyaları kopyalamak için kullanılabilir. Örneğin, aşağıdaki komut `test.txt` dizinini yerel sistemden birincil baş düğüme kopyalar.

```bash
scp test.txt sshuser@clustername-ssh.azurehdinsight.net:
```

`:` sonrasında yol sonra belirtilmezse dosya `sshuser` giriş dizinine yerleştirilir.

Aşağıdaki örnekte birincil baş düğümdeki `sshuser` giriş dizininden `test.txt` dosyası yerel sisteme kopyalanmaktadır:

```bash
scp sshuser@clustername-ssh.azurehdinsight.net:test.txt .
```

> [!IMPORTANT]  
> `scp`, yalnızca küme içindeki tek düğümlerin dosya sistemine erişebilir. Küme için HDFS uyumlu depolama biriminde bulunan verilere erişmek için kullanılamaz.
>
> Bir kaynağı SSH oturumundan kullanmak için karşıya yüklemeniz gerektiğinde `scp` kullanın. Örneğin, bir Python betiğini karşıya yükleyin ve bir SSH oturumundan çalıştırın.
>
> Verileri HDFS uyumlu depolama alanına doğrudan yükleme hakkında daha fazla bilgi için aşağıdaki belgelere bakın:
>
> * [Azure Depolama kullanarak HDInsight](hdinsight-hadoop-use-blob-storage.md)
>
> * [Azure Data Lake Store kullanarak HDInsight](hdinsight-hadoop-use-data-lake-store.md).

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight ile SSH tüneli kullanma](hdinsight-linux-ambari-ssh-tunnel.md)
* [HDInsight ile sanal ağ kullanma](hdinsight-extend-hadoop-virtual-network.md)
* [HDInsight’ta kenar düğümlerini kullanma](hdinsight-apps-use-edge-node.md#access-an-edge-node)
