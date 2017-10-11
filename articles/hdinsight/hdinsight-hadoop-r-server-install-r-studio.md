---
title: "R Server hdınsight'ta - Azure ile Rstudio'dan yükleme | Microsoft Docs"
description: "Rstudio'dan hdınsight'ta R Server ile yükleme."
services: hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 918abb0d-8248-4bc5-98dc-089c0e007d49
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/19/2017
ms.author: bradsev
ms.openlocfilehash: 416420d855505508735ebd8526e93efdb230ad53
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="installing-rstudio-with-r-server-on-hdinsight"></a>Hdınsight'ta R Server ile Rstudio'dan yükleme

Bu makalede community (ücretsiz) sürümünü yüklemek nasıl [Rstudio'dan Server](https://www.rstudio.com/products/rstudio-server/) özel bir komut dosyası kullanarak bir küme kenar düğümü üzerinde. Rstudio'dan sunucusu, uzak istemciler tarafından kullanılacak tarayıcı tabanlı bir IDE sağlar ve Linux üzerinde yaygın olarak kullanılır. Dahil olmak üzere birden çok tümleşik geliştirme ortamı (IDE) R için kullanılabilir bugün vardır:

- Microsoft'un [R araçları Visual Studio için](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS) 
- [Rstudio'dan sunucu](https://www.rstudio.com/products/rstudio-server/) 
- Walware Eclipse tabanlı [StatET](http://www.walware.de/goto/statet).

Hdınsight kümesi kenar düğümüne Rstudio'dan sunucusu yükleme gibi bir avantajı, tam IDE Deneyimi Geliştirme ve R betiklerinin yürütülmesi için R sunucusuyla kümede sağlamasıdır. Bu yapılandırma önemli ölçüde daha üretken R konsol varsayılan kullanımını olabilir.

> [!NOTE]
> Bu makalede açıklanan yordam, yalnızca kümenizi sağlamada Rstudio'dan Server community sürümü yüklemek için seçmediyseniz geçerlidir. Sağlama işlemi sırasında eklenen sonra erişmek için tıklatılan **R Server panolar** kutucuğunu, kümeniz için Azure portal girişi sonra **R Studio Server** döşeme. 

Ticari olarak lisanslı Pro Server sürümü Rstudio'dan kullanmayı tercih ederseniz, yükleme yönergeleri izlemeniz gereken [Rstudio'dan Server](https://www.rstudio.com/products/rstudio/download-server/).

> [!NOTE]
> İçin R yüklenen kullanarak bir Hdınsight kümesi kullanıyorsanız [yükleme R betik eylemi](hdinsight-hadoop-r-scripts-linux.md), bu belgede yer alan adımlar çalışmaz doğru Hdınsight kümesinde bir R Server gerektirir.
>
> 

## <a name="prerequisites"></a>Ön koşullar

* Azure Hdınsight kümesi R Server yüklü. Yönergeler için bkz: [Hdınsight kümelerinde R Server ile çalışmaya başlama](hdinsight-hadoop-r-server-get-started.md).
* Bir SSH istemcisi. Linux ve UNIX dağıtımlarının veya Macintosh OS X için `ssh` komutu, işletim sistemiyle birlikte sağlanır. Windows için öneririz [Cygwin](http://www.redhat.com/services/custom/cygwin/) ile [OpenSSH seçeneği](https://www.youtube.com/watch?v=CwYSvvGaiWU), veya [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).  

## <a name="install-rstudio-on-the-cluster-using-a-custom-script"></a>Özel bir komut dosyası kullanarak kümede Rstudio'dan yüklemek

Adımlar aşağıdaki gibidir:

1. Kümenin kenar düğümüne tanımlayın. R Server ile bir Hdınsight kümesi için baş düğümü ve kenar düğümü için adlandırma kuralı aşağıda verilmiştir.
   * Baş düğüm`CLUSTERNAME-ssh.azurehdinsight.net`
   * Kenar düğümüne`CLUSTERNAME-ed-ssh.azurehdinsight.net` 

2. 1. adımda sağlanan adlandırma desenini kullanarak küme kenar düğümüne içine SSH. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

3. Bağlandıktan sonra küme üzerinde bir kök kullanıcı haline gelir. SSH oturumunda aşağıdaki komutu kullanın:

        sudo su -

4. Rstudio'dan yüklemek için özel bir komut dosyası yükleyin. Aşağıdaki komutu kullanın:

        wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/InstallRStudio.sh

5. Özel komut dosyası izinleri değiştirmek ve betiği çalıştırın. Aşağıdaki komutları kullanın:

        chmod 755 InstallRStudio.sh
        ./InstallRStudio.sh

6. R Server ile bir Hdınsight kümesi oluştururken bir SSH parola kullandıysanız, bu adımı atlayın ve sonraki devam edin. Bir SSH anahtarı yerine küme oluşturmak için kullandıysanız, SSH kullanıcı için bir parola ayarlamanız gerekir. Bu parola için Rstudio'dan bağlanırken gerekir. Aşağıdaki komutları çalıştırın:

        passwd USERNAME
        Current Kerberos password:
        New password:
        Retype new password:
        Current Kerberos password:


7. İstendiğinde **geçerli Kerberos Parola**, basın **ENTER**.  Değiştirmeniz gereken Not `USERNAME` Hdınsight kümenizle bir SSH kullanıcıyla. Parolanızı başarıyla ayarlarsanız, şu iletiyi görmeniz gerekir:

        passwd: password updated successfully

    SSH oturumu çıkın.

8. Eşleme tarafından kümeye SSH tüneli oluşturma `ssh -L localhost:8787:localhost:8787 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` istemci makineye Hdınsight kümesinde. Yeni bir tarayıcı oturum açmadan önce bir SSH tüneli oluşturmanız gerekir.

   * Linux istemci veya bir Windows İstemcisi ile [Cygwin](http://www.redhat.com/services/custom/cygwin/), bir terminal oturumu açın ve aşağıdaki komutu kullanın:

             ssh -L localhost:8787:localhost:8787 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

       Değiştir **kullanıcıadı** Hdınsight kümesi ve değiştirmek için bir SSH kullanıcıyla **CLUSTERNAME** Hdınsight kümenizin adıyla.
       Ayrıca bir parola yerine bir SSH anahtarı ekleyerek kullanabileceğiniz `-i id_rsa_key`.        
   * Windows İstemcisi ile PuTTY sonra kullanıyorsanız

     1. Putty'yi açın ve bağlantı bilgilerinizi girin.
     2. İçinde **kategori** iletişim kutusunun solundaki bölümünde, genişletmek **bağlantı**, genişletin **SSH**ve ardından **tüneller**.
     3. Üzerinde aşağıdaki bilgileri sağlayın **SSH denetleme seçenekleri bağlantı noktası iletme** form:

        * **Kaynak bağlantı noktası** - İletmek istediğiniz istemci üzerindeki bağlantı noktası. Örneğin, **8787**.
        * **Hedef** -yerel istemci makineye eşlenmelidir hedef. Örneğin, **localhost:8787**.

            ![SSH tüneli oluşturma](./media/hdinsight-hadoop-r-server-install-r-studio/createsshtunnel.png "SSH tüneli oluşturma")

     4. Tıklatın **Ekle** ayarları ekleyin ve ardından **açmak** bir SSH bağlantısı açılamıyor.
     5. İstendiğinde, bir SSH oturumu oluşturur ve tüneli etkinleştirmek için sunucuya oturum açın.

9. Bir web tarayıcısı açın ve için tünel girdiğiniz bağlantı noktası göre aşağıdaki URL'yi girin:

        http://localhost:8787/ 

10. SSH kullanıcı adı ve kümeye bağlanmak için parola girmeniz istenir. Küme oluştururken bir SSH anahtarı kullandıysanız, 5. adımda oluşturduğunuz parola girmeniz gerekir.

    ![R Studio'ya bağlanın](./media/hdinsight-hadoop-r-server-install-r-studio/connecttostudio.png "SSH tüneli oluşturma")

11. Rstudio'dan yüklemenin başarılı olup olmadığını sınamak için küme üzerinde R tabanlı MapReduce ve Spark işlerini yürütür test betiğini çalıştırabilirsiniz. Rstudio'dan içinde çalıştırmak için test komut dosyasını indirmek için SSH konsola geri dönün ve aşağıdaki komutları girin:

    *    R ile Hadoop kümesi oluşturduysanız, bu komutu kullanın:

            wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi.r
    *    R ile Spark kümesi oluşturduysanız, bu komutu kullanın:

            wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi_spark.r

12. Rstudio'dan içinde indirdiğiniz test komut dosyasına bakın. Açmak, dosyanın içeriğini seçin ve ardından dosyasına çift tıklayarak **çalıştırmak**. Bir çıktı görmeniz gerekir **konsol** bölmesi:

   ![Yükleme testi](./media/hdinsight-hadoop-r-server-install-r-studio/test-r-script.png "Test yüklemesi")

Türü için başka bir seçenek olabilir `source(testhdi.r)` veya `source(testhdi_spark.r)` komut dosyası yürütme.

## <a name="see-also"></a>Ayrıca bkz.

* [Hdınsight kümelerinde R Server için içerik seçeneklerini işlem](hdinsight-hadoop-r-server-compute-contexts.md)
* [HDInsight üzerinde R Server için Azure Depolama seçenekleri](hdinsight-hadoop-r-server-storage.md)

