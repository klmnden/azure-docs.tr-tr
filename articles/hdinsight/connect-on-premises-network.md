---
title: Azure HDInsight, şirket içi ağınıza bağlama
description: Azure sanal ağ'daki bir HDInsight kümesi oluşturma ve sonra şirket içi ağınıza bağlanma hakkında bilgi edinin. Özel bir DNS sunucusu kullanarak HDInsight ile şirket içi ağınız arasında ad çözümlemesine yapılandırmayı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/04/2019
ms.openlocfilehash: 52fe8c05101f9647549acec276f0bdb9fa52d1c7
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59256813"
---
# <a name="connect-hdinsight-to-your-on-premises-network"></a>HDInsight’ı şirket içi ağınıza bağlama

Azure sanal ağları ve VPN ağ geçidi kullanarak şirket içi ağınıza HDInsight bağlanmayı öğreneceksiniz. Bu belge planlama bilgileri sağlar:

* HDInsight, Azure sanal ağında şirket içi ağınıza bağlanan kullanma.
* Sanal ağ ve şirket içi ağınız arasında DNS ad çözümlemesi yapılandırma.
* HDInsight için internet erişimi kısıtlamak için ağ güvenlik gruplarını yapılandırma.
* Sanal ağda HDInsight tarafından sağlanan bağlantı noktaları.

## <a name="overview"></a>Genel Bakış

HDInsight ve kaynakları adıyla iletişim kurması için birleştirilmiş ağdaki izin vermek için aşağıdaki eylemleri gerçekleştirmeniz gerekir:

* Azure sanal ağı oluşturun.
* Özel bir DNS sunucusu, Azure sanal ağ oluşturun.
* Azure yinelemeli çözümleyici varsayılan yerine özel bir DNS sunucusu kullanmak için sanal ağ yapılandırın.
* Özel DNS sunucusu ve şirket içi DNS sunucunuz arasında iletim yapılandırın.

Bu yapılandırma, aşağıdaki davranış sağlar:

* DNS son ekine sahip tam etki alanı adları için istekleri __sanal ağın__ özel DNS sunucusuna gönderilir. Özel DNS sunucusu, bu istekler Azure özyinelemeli IP adresi döndüren çözümleyiciye, ardından iletir.
* Diğer tüm istekler için şirket içi DNS sunucusuna iletilir. Microsoft.com gibi ortak Internet kaynaklarına yönelik bile istekleri, ad çözümlemesi için şirket içi DNS sunucusuna iletilir.

Aşağıdaki diyagramda, yeşil satırlar sanal ağın DNS son eki ile biten kaynakların isteklerdir. Mavi çizgilerin, şirket içi ağ ya da genel internet'teki kaynakları isteklerdir.

![Bu belgede kullanılan yapılandırma DNS istekleri nasıl çözüleceğini diyagramı](./media/connect-on-premises-network/on-premises-to-cloud-dns.png)

## <a name="prerequisites"></a>Önkoşullar

* Bir SSH istemcisi. Daha fazla bilgi için [SSH kullanarak HDInsight (Apache Hadoop) bağlanma](./hdinsight-hadoop-linux-use-ssh-unix.md).
* PowerShell kullanarak, ihtiyacınız olacak [AZ modül](https://docs.microsoft.com/powershell/azure/overview).
* Kullanmak isteyen eğitimcilere ve Azure CLI'yi henüz yüklemediyseniz, bkz: [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="create-virtual-network-configuration"></a>Sanal ağ yapılandırması oluşturun

Bir Azure sanal şirket içi ağınıza bağlı ağ oluşturma hakkında bilgi edinmek için aşağıdaki belgeleri kullanın:

* [Azure Portalını kullanma](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
* [Azure PowerShell’i kullanma](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
* [Azure CLI’yi kullanma](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md)

## <a name="create-custom-dns-server"></a>Özel DNS sunucusu oluşturma

> [!IMPORTANT]  
> Oluşturun ve HDInsight sanal ağa yüklemeden önce DNS sunucusu yapılandırmanız gerekir.

Bu adımları kullanın [Azure portalında](https://portal.azure.com) bir Azure sanal makine oluşturmak için. Bir sanal makine oluşturmak diğer yolları için bkz. [VM oluşturma - Azure CLI](../virtual-machines/linux/quick-create-cli.md) ve [VM oluşturma - Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md).  Kullanan bir Linux VM oluşturmak için [bağlama](https://www.isc.org/downloads/bind/) DNS yazılım, aşağıdaki adımları kullanın:

1. [Azure Portal](https://portal.azure.com) oturum açın.
  
2. Sol menüden gidin **+ kaynak Oluştur** > **işlem** > **Ubuntu Server 18.04 LTS**.

    ![Bir Ubuntu sanal makinesi oluşturma](./media/connect-on-premises-network/create-ubuntu-vm.png)

3. Gelen __Temelleri__ sekmesinde, aşağıdaki bilgileri girin:  
  
    | Alan | Değer |
    | --- | --- |
    |Abonelik |Uygun aboneliğinizi seçin.|
    |Kaynak grubu |Daha önce oluşturduğunuz sanal ağı içeren kaynak grubunu seçin.|
    |Sanal makine adı | Bu sanal makine tanımlayan bir kolay ad girin. Bu örnekte **DNSProxy**.|
    |Bölge | Aynı bölgede daha önce oluşturduğunuz sanal ağı seçin.  Tüm VM boyutları, tüm bölgelerde kullanılabilir.  |
    |Kullanılabilirlik seçenekleri |  İstenen, kullanılabilirlik düzeyini seçin.  Azure, kullanılabilirlik ve dayanıklılık için uygulamalarınızı yönetmek için seçenekleri sunar.  Çoğaltılmış VM'lerin kullanılabilirlik bölgelerinde veya kullanılabilirlik kümeleri, veri merkezi kesintilerine ve Bakım olayları uygulamalarınızı ve verilerinizi korumak amacıyla kullanılacak çözümünüzü tasarlama. Bu örnekte **gerekli altyapı artıklık**. |
    |Görüntü | Bırakılacak **Ubuntu Server 18.04 LTS**. |
    |Kimlik doğrulaması türü | __Parola__ veya __SSH ortak anahtarı__: SSH hesabı için kimlik doğrulama yöntemi. Daha güvenli oldukları gibi ortak anahtarları kullanmanızı öneririz. Bu örnekte **parola**.  Daha fazla bilgi için [Linux VM'ler için SSH anahtarları oluşturma ve kullanma](../virtual-machines/linux/mac-create-ssh-keys.md) belge.|
    |Kullanıcı adı |VM için yönetici kullanıcı adı girin.  Bu örnekte **sshuser**.|
    |Parola veya SSH ortak anahtarı | Seçiminiz için kullanılabilir alanın belirlenir **kimlik doğrulama türü**.  Uygun değeri girin.|
    |Ortak gelen bağlantı noktası|Seçin **Seçili bağlantı noktalarına izin**. Ardından **SSH (22)** gelen **gelen bağlantı noktalarını seçin** aşağı açılan listesi.|

    ![Sanal makine temel yapılandırma](./media/connect-on-premises-network/vm-basics.png)

    Diğer girişler varsayılan değerlerinde bırakın ve ardından **ağ** sekmesi.

4. Gelen **ağ** sekmesinde, aşağıdaki bilgileri girin:

    | Alan | Değer |
    | --- | --- |
    |Sanal ağ | Daha önce oluşturduğunuz sanal ağı seçin.|
    |Alt ağ | Daha önce oluşturduğunuz sanal ağ için varsayılan alt ağ seçin. Yapmak __değil__ VPN ağ geçidi tarafından kullanılan alt ağ seçin.|
    |Genel IP | Paneldeki değerini kullanın.  |

    ![Sanal ağ ayarları](./media/connect-on-premises-network/virtual-network-settings.png)

    Diğer girişler varsayılan değerlerinde bırakın ve ardından **gözden geçir + Oluştur**.

5. Gelen **gözden geçir + Oluştur** sekmesinde **Oluştur** sanal makine oluşturmak için.

### <a name="review-ip-addresses"></a>IP adreslerini gözden geçirin
Sanal makine oluşturulduktan sonra alacak bir **dağıtım başarılı** bildirimi bir **kaynağa Git** düğmesi.  Seçin **kaynağa Git** yeni sanal makinenize gidin.  Yeni bir sanal makine için varsayılan görünümünden ilişkili IP adreslerini belirlemek için aşağıdaki adımları izleyin:

1. Gelen **ayarları**seçin **özellikleri**.

2. Değerlerini Not **genel IP adresi/DNS ad ETİKETİ** ve **özel IP adresi** daha sonra kullanmak için.

   ![Genel ve özel IP adresleri](./media/connect-on-premises-network/vm-ip-addresses.png)

### <a name="install-and-configure-bind-dns-software"></a>Yükleme ve bağlama (DNS yazılım) yapılandırma

1. Bağlanmak için SSH kullanın __genel IP adresi__ sanal makinenin. Değiştirin `sshuser` ile sanal makine oluştururken belirttiğiniz SSH kullanıcı hesabı. Aşağıdaki örnek bir sanal makinenin 40.68.254.142 bağlanır:

    ```bash
    ssh sshuser@40.68.254.142
    ```

2. Bağlama yüklemek için SSH oturumunda aşağıdaki komutları kullanın:

    ```bash
    sudo apt-get update -y
    sudo apt-get install bind9 -y
    ```

3. Şirket içi DNS sunucunuzun adı çözümlemesi isteklerini iletmek için bağlama yapılandırmak için aşağıdaki metni içeriğini kullanın `/etc/bind/named.conf.options` dosyası:

        acl goodclients {
            10.0.0.0/16; # Replace with the IP address range of the virtual network
            10.1.0.0/16; # Replace with the IP address range of the on-premises network
            localhost;
            localnets;
        };

        options {
                directory "/var/cache/bind";

                recursion yes;

                allow-query { goodclients; };

                forwarders {
                192.168.0.1; # Replace with the IP address of the on-premises DNS server
                };

                dnssec-validation auto;

                auth-nxdomain no;    # conform to RFC1035
                listen-on { any; };
        };

    > [!IMPORTANT]  
    > Değerleri değiştirin `goodclients` sanal ağ ve şirket içi ağ ile IP adresi aralığı bölümü. Bu bölümde, bu DNS sunucusu gelen istekleri kabul eder adresleri tanımlar.
    >
    > Değiştirin `192.168.0.1` girişi `forwarders` şirket içi DNS sunucunuzun IP adresi bölümü. Bu giriş, şirket içi DNS sunucusuna çözümleme için DNS istekleri yönlendirir.

    Bu dosyayı düzenlemek için aşağıdaki komutu kullanın:

    ```bash
    sudo nano /etc/bind/named.conf.options
    ```

    Dosyayı kaydetmek için kullanın __Ctrl + X__, __Y__, ardından __Enter__.

4. SSH oturumunda aşağıdaki komutu kullanın:

    ```bash
    hostname -f
    ```

    Bu komut, aşağıdaki metne benzer bir değer döndürür:

    ```output
    dnsproxy.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net
    ```

    `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` Metin __DNS soneki__ bu sanal ağ için. Bu değeri kaydedin çünkü daha sonra kullanılacaktır.

5. Sanal ağ içindeki kaynaklar için DNS adlarını çözümlemek için bağlama yapılandırmak için aşağıdaki metni içeriğini kullanın `/etc/bind/named.conf.local` dosyası:

        // Replace the following with the DNS suffix for your virtual network
        zone "icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net" {
            type forward;
            forwarders {168.63.129.16;}; # The Azure recursive resolver
        };

    > [!IMPORTANT]  
    > Değiştirmeniz gereken `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` daha önce aldığınız DNS soneki.

    Bu dosyayı düzenlemek için aşağıdaki komutu kullanın:

    ```bash
    sudo nano /etc/bind/named.conf.local
    ```

    Dosyayı kaydetmek için kullanın __Ctrl + X__, __Y__, ardından __Enter__.

6. Bağlama başlatmak için aşağıdaki komutu kullanın:

    ```bash
    sudo service bind9 restart
    ```

7. Bu bağlama, şirket içi ağınızdaki kaynakların adlarını çözümleyebilen doğrulamak için aşağıdaki komutları kullanın:

    ```bash
    sudo apt install dnsutils
    nslookup dns.mynetwork.net 10.0.0.4
    ```

    > [!IMPORTANT]  
    > Değiştirin `dns.mynetwork.net` şirket içi ağınızda bir kaynak tam etki alanı adını (FQDN).
    >
    > Değiştirin `10.0.0.4` ile __iç IP adresi__ sanal ağda özel DNS sunucunuzun.

    Yanıt aşağıdaki metne benzer görünür:

    ```output
    Server:         10.0.0.4
    Address:        10.0.0.4#53

    Non-authoritative answer:
    Name:   dns.mynetwork.net
    Address: 192.168.0.4
    ```

## <a name="configure-virtual-network-to-use-the-custom-dns-server"></a>Sanal ağ özel DNS sunucusunu kullanacak şekilde yapılandırma

Özel DNS sunucusu yerine Azure özyinelemeli çözümleyici kullanılacak sanal ağı yapılandırmak için aşağıdaki adımları kullanın. [Azure portalında](https://portal.azure.com):

1. Sol menüden gidin **tüm hizmetleri** > **ağ** > **sanal ağları**.

2. Sanal ağınıza, sanal ağınız için varsayılan görünüm açılır listesinden seçin.  

3. Varsayılan görünümünden altında **ayarları**seçin **DNS sunucuları**.  

4. Seçin __özel__girin **özel IP adresi** özel DNS sunucusu.   

5. __Kaydet__’i seçin.  <br />  

    ![Ağ için özel DNS sunucusunu ayarlayın](./media/connect-on-premises-network/configure-custom-dns.png)

## <a name="configure-on-premises-dns-server"></a>Şirket içi DNS sunucusunu yapılandırma

Önceki bölümde, özel DNS sunucusunu şirket içi DNS sunucusu, istekleri iletmek üzere yapılandırılmış. Ardından, özel DNS sunucu isteklerini iletmek için şirket içi DNS sunucusu yapılandırmanız gerekir.

DNS sunucunuzu yapılandırma hakkında belirli adımlar için DNS sunucusu yazılımınızın belgelerine bakın. Yapılandırma adımları için konum bir __koşullu ileticisi__.

Koşullu iletme, yalnızca belirli bir DNS soneki isteklerini iletir. Bu durumda, sanal ağın DNS soneki için bir yönlendirici yapılandırmanız gerekir. Bu son ek istekler için özel DNS sunucusunun IP adresi iletilmesi gereken. 

Aşağıdaki metni için bir koşullu ileticisi yapılandırma örneğidir **bağlama** DNS yazılım:

    zone "icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net" {
        type forward;
        forwarders {10.0.0.4;}; # The custom DNS server's internal IP address
    };

DNS kullanma hakkında bilgi **Windows Server 2016**, bkz: [Ekle DnsServerConditionalForwarderZone](https://technet.microsoft.com/itpro/powershell/windows/dnsserver/add-dnsserverconditionalforwarderzone) belgeleri...

Şirket içi DNS sunucusunu yapılandırdıktan sonra kullanabileceğiniz `nslookup` , sanal ağ adlarını çözümleyebilen doğrulamak için şirket içi ağdan. Aşağıdaki örnek 

```bash
nslookup dnsproxy.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net 196.168.0.4
```

Bu örnek, özel DNS sunucu adını çözümlemek için 196.168.0.4 şirket içi DNS sunucusunu kullanır. Bir şirket içi DNS sunucusunun IP adresini değiştirin. Değiştirin `dnsproxy` özel DNS sunucusunun tam etki alanı adı ile adres.

## <a name="optional-control-network-traffic"></a>İsteğe bağlı: Ağ trafiği denetleme

Ağ trafiğinizi denetlemek için ağ güvenlik grupları (NSG) veya kullanıcı tanımlı yollar (UDR) kullanabilirsiniz. Nsg'ler, gelen ve giden trafiği filtrelemek ve izin vermek veya trafiği reddetmeye olanak tanır. Udr, şirket içi ağ sanal ağ ve internet kaynakları arasındaki trafiğin nasıl akacağını denetlemenize olanak sağlar.

> [!WARNING]  
> HDInsight, Azure bulutunda belirli IP adreslerinden gelen erişim ve sınırsız giden erişim gerektirir. Trafiği denetlemek için Nsg'ler veya Udr kullanırken, aşağıdaki adımları gerçekleştirmeniz gerekir:

1. IP adresleri için sanal ağınızın bulunduğu konumu bulun. Konuma göre gerekli IP'ler listesi için bkz. [gerekli IP adresleri](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip).

2. 1. adımda belirlenen IP adresleri için bu IP gelen trafiğe izin adresleri.

   * Kullanıyorsanız __NSG__: İzin __gelen__ bağlantı noktası üzerinde trafiğe __443__ IP adresleri için.
   * Kullanıyorsanız __UDR__: Ayarlama __sonraki atlama__ yol türünü __Internet__ IP adresleri için.

Azure PowerShell veya Azure CLI kullanarak Nsg oluşturma örneği için bkz: [genişletmek HDInsight ile Azure sanal ağları](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-nsg) belge.

## <a name="create-the-hdinsight-cluster"></a>HDInsight kümesi oluşturma

> [!WARNING]  
> Sanal ağda HDInsight yüklemeden önce özel bir DNS sunucusu yapılandırmanız gerekir.

İçindeki adımları kullanın [Azure portalını kullanarak bir HDInsight kümesi oluşturma](./hdinsight-hadoop-create-linux-clusters-portal.md) bir HDInsight kümesi oluşturmak için belge.

> [!WARNING]  
> * Küme oluşturma sırasında sanal ağınızın bulunduğu konumu seçmeniz gerekir.
> * İçinde __Gelişmiş ayarlar__ bölümü yapılandırmasına, daha önce oluşturduğunuz alt ağ ve sanal ağ seçmelisiniz.

## <a name="connecting-to-hdinsight"></a>HDInsight için bağlanma

HDInsight üzerinde çoğu belgeleri, internet üzerinden kümeye erişim sahibi olduğunuzu varsayar. Örneğin, `https://CLUSTERNAME.azurehdinsight.net` konumundaki kümeye bağlanabildiğiniz kabul edilir. Bu adres, internet'ten erişimi kısıtlamak için Nsg veya Udr'ler kullandıysanız, kullanılabilir olmayan ortak ağ geçidi kullanır.

Ayrıca bazı belgelerde başvuran `headnodehost` bir SSH oturumundan kümeye bağlanırken. Bu adres, yalnızca bir küme içindeki düğümler kullanılabilir olduğundan ve sanal ağ üzerinden bağlanan istemciler üzerinde kullanılamaz.

HDInsight için sanal ağ üzerinden doğrudan bağlanmak için aşağıdaki adımları kullanın:

1. HDInsight küme düğümleri dahili tam etki alanı adlarını bulmak için aşağıdaki yöntemlerden birini kullanın:

    ```powershell
    $resourceGroupName = "The resource group that contains the virtual network used with HDInsight"

    $clusterNICs = Get-AzNetworkInterface -ResourceGroupName $resourceGroupName | where-object {$_.Name -like "*node*"}

    $nodes = @()
    foreach($nic in $clusterNICs) {
        $node = new-object System.Object
        $node | add-member -MemberType NoteProperty -name "Type" -value $nic.Name.Split('-')[1]
        $node | add-member -MemberType NoteProperty -name "InternalIP" -value $nic.IpConfigurations.PrivateIpAddress
        $node | add-member -MemberType NoteProperty -name "InternalFQDN" -value $nic.DnsSettings.InternalFqdn
        $nodes += $node
    }
    $nodes | sort-object Type
    ```

    ```azurecli
    az network nic list --resource-group <resourcegroupname> --output table --query "[?contains(name,'node')].{NICname:name,InternalIP:ipConfigurations[0].privateIpAddress,InternalFQDN:dnsSettings.internalFqdn}"
    ```

2. Bir hizmeti üzerinde kullanılabilir bağlantı noktasını belirlemek için bkz: [HDInsight üzerinde Apache Hadoop Hizmetleri tarafından kullanılan bağlantı noktaları](./hdinsight-hadoop-port-settings-for-services.md) belge.

    > [!IMPORTANT]  
    > Baş düğümler üzerinde barındırılan bazı hizmetler aynı anda yalnızca bir düğümde etkin olan. Bir baş düğüm üzerinde bir hizmete erişim deneyin ve yine başarısız olursanız diğer baş düğümüne geçin.
    >
    > Örneğin, Apache Ambari yalnızca aynı anda bir baş düğüm üzerinde etkin değil. Ambari bir baş düğüm üzerinde erişim deneyin ve bir 404 hatası döndürür, diğer baş düğüm üzerinde çalışıyor.

## <a name="next-steps"></a>Sonraki adımlar

* Bir sanal ağda HDInsight kullanma hakkında daha fazla bilgi için bkz. [Azure sanal ağlarını kullanarak HDInsight genişletmek](./hdinsight-extend-hadoop-virtual-network.md).

* Azure sanal ağları hakkında daha fazla bilgi için bkz. [Azure sanal ağına genel bakış](../virtual-network/virtual-networks-overview.md).

* Ağ güvenlik grupları hakkında daha fazla bilgi için bkz. [ağ güvenlik grupları](../virtual-network/security-overview.md).

* Kullanıcı tanımlı yollar hakkında daha fazla bilgi için bkz. [kullanıcı tanımlı yollar ve IP iletme](../virtual-network/virtual-networks-udr-overview.md).
