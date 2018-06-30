---
title: Hdınsight, şirket içi ağınıza - Azure Hdınsight bağlanmak | Microsoft Docs
description: Bir Azure sanal ağında Hdınsight kümesi oluşturmak ve şirket içi ağınıza bağlayın öğrenin. Özel bir DNS sunucusu kullanarak Hdınsight ile şirket içi ağınız arasında ad çözümleme yapılandırmayı öğrenin.
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 02/23/2018
ms.author: larryfr
ms.openlocfilehash: bfb6515ba9b7f36e90783444fc474dc575b32f37
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37113631"
---
# <a name="connect-hdinsight-to-your-on-premises-network"></a>HDInsight’ı şirket içi ağınıza bağlama

Hdınsight Azure sanal ağlar ve VPN ağ geçidi kullanarak şirket ağınıza bağlanmak öğrenin. Bu belge planlama bilgilerini sağlar:

* Bir Azure sanal ağındaki şirket içi ağınıza bağlanan Hdınsight kullanma.

* Sanal ağ ve şirket içi ağınız arasında DNS ad çözümlemesini yapılandırma.

* Hdınsight için Internet erişimi kısıtlamak için ağ güvenlik gruplarını yapılandırma.

* Hdınsight tarafından sağlanan sanal ağ bağlantı noktaları.

## <a name="create-the-virtual-network-configuration"></a>Sanal ağ yapılandırması oluştur

Bir Azure sanal şirket içi ağınıza bağlı ağ oluşturmayı öğrenmek için aşağıdaki belgeleri kullanın:
    
* [Azure portalını kullanma](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)

* [Azure PowerShell’i kullanma](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

* [Azure CLI’yi kullanma](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md)

## <a name="configure-name-resolution"></a>Ad çözümlemesini yapılandırma

Hdınsight ve kaynakları adıyla iletişim kurması için birleştirilmiş ağdaki izin vermek için aşağıdaki eylemleri gerçekleştirmeniz gerekir:

* Özel bir DNS sunucusu, Azure sanal ağ oluşturun.

* Sanal ağ Azure özyinelemeli çözümleyici varsayılan yerine özel DNS sunucusunu kullanacak şekilde yapılandırın.

* Özel DNS sunucusu ve şirket içi DNS sunucunuz arasında iletim yapılandırın.

Bu yapılandırma aşağıdaki davranışı etkinleştirir:

* DNS son ekine sahip tam etki alanı adları için istekleri __sanal ağ için__ özel DNS sunucusuna gönderilir. Özel DNS sunucusu, daha sonra Azure özyinelemeli IP adresini döndüren çözümleyiciye, bu istekleri iletir.

* Tüm diğer isteklerden şirket DNS sunucusuna iletilir. Microsoft.com gibi ortak Internet kaynakların bile istekleri için ad çözümlemesi şirket DNS sunucusuna iletilir.

Aşağıdaki şemada çizgiler sanal ağın DNS soneki bitiş kaynaklara yönelik isteklerin ' dir. Mavi şirket içi ağ veya ortak Internet kaynakların isteklerdir.

![Bu belgede kullanılan yapılandırmasında DNS isteklerine nasıl çözüleceğini diyagramı](./media/connect-on-premises-network/on-premises-to-cloud-dns.png)

### <a name="create-a-custom-dns-server"></a>Özel bir DNS sunucusu oluştur

> [!IMPORTANT]
> Oluşturma ve Hdınsight sanal ağınıza yüklemeden önce DNS sunucusu yapılandırmanız gerekir.

Kullanan bir Linux VM oluşturmak için [bağlamak](https://www.isc.org/downloads/bind/) DNS yazılım, aşağıdaki adımları kullanın:

> [!NOTE]
> Aşağıdaki adımları kullanın [Azure portal](https://portal.azure.com) bir Azure sanal makine oluşturmak için. Bir sanal makine oluşturmak diğer yolları için aşağıdaki belgelere bakın:
>
> * [VM Oluşturma - Azure CLI](../virtual-machines/linux/quick-create-cli.md)
> * [Azure PowerShell VM - oluşturma](../virtual-machines/linux/quick-create-portal.md)

1. Gelen [Azure portal](https://portal.azure.com)seçin __+__, __işlem__, ve __Ubuntu Server 16.04 LTS__.

    ![Ubuntu sanal makine oluşturma](./media/connect-on-premises-network/create-ubuntu-vm.png)

2. __Temel bilgiler__ bölümünde aşağıdaki bilgileri girin:

    * __Ad__: Bu sanal makineyi tanımlayan bir kolay ad. Örneğin, __DNSProxy__.
    * __Kullanıcı adı__: SSH hesabının adı.
    * __SSH ortak anahtarını__ veya __parola__: SSH hesabı için kimlik doğrulama yöntemi. Daha güvenli olarak ortak anahtarları kullanmanızı öneririz. Daha fazla bilgi için bkz: [Linux VM'ler için SSH anahtarları oluşturma ve kullanma](../virtual-machines/linux/mac-create-ssh-keys.md) belge.
    * __Kaynak grubu__: seçin __var olanı kullan__ve ardından daha önce oluşturduğunuz sanal ağı içeren kaynak grubunu seçin.
    * __Konum__: sanal ağ olarak aynı konumu seçin.

    ![Sanal makine temel yapılandırma](./media/connect-on-premises-network/vm-basics.png)

    Diğer girişler varsayılan değerlerinde bırakın ve ardından __Tamam__.

3. Gelen __bir boyutu seçin__ bölümünde, VM boyutunu seçin. Bu öğretici için en küçük ve en düşük Maliyeti seçeneğini seçin. Devam etmek için kullanın __seçin__ düğmesi.

4. Gelen __ayarları__ bölümünde, aşağıdaki bilgileri girin:

    * __Sanal ağ__: daha önce oluşturduğunuz sanal ağı seçin.

    * __Alt ağ__: sanal ağ için varsayılan alt ağ seçin. Yapmak __değil__ VPN ağ geçidi tarafından kullanılan alt ağ seçin.

    * __Tanılama depolama hesabı__: mevcut bir depolama hesabını seçin veya yeni bir tane oluşturun.

    ![Sanal ağ ayarları](./media/connect-on-premises-network/virtual-network-settings.png)

    Varsayılan değer olarak diğer girişler bırakın ve ardından __Tamam__ devam etmek için.

5. Gelen __satın alma__ bölümünde, select __satın alma__ sanal makine oluşturmak için düğmesi.

6. Sanal makine oluşturulduktan sonra kendi __genel bakış__ bölümünde görüntülenir. Sol taraftaki listesinden __özellikleri__. Kaydet __genel IP adresi__ ve __özel IP adresi__ değerleri. Sonraki bölümde kullanılır.

    ![Ortak ve özel IP adresleri](./media/connect-on-premises-network/vm-ip-addresses.png)

### <a name="install-and-configure-bind-dns-software"></a>Yükleme ve bağlama (DNS yazılım) yapılandırma

1. SSH bağlanmak için kullandığı __genel IP adresi__ sanal makinenin. Aşağıdaki örnek bir sanal makinenin 40.68.254.142 bağlanır:

    ```bash
    ssh sshuser@40.68.254.142
    ```

    Değiştir `sshuser` küme oluştururken, belirtilen SSH kullanıcı hesabıyla.

    > [!NOTE]
    > Bir elde etmek için çeşitli yollar vardır `ssh` yardımcı programı. Linux, Unix ve macOS, işletim sisteminin bir parçası sağlanır. Windows kullanıyorsanız, aşağıdaki seçeneklerden birini göz önünde bulundurun:
    >
    > * [Azure Cloud Shell](../cloud-shell/quickstart.md)
    > * [Windows 10 Ubuntu bash](https://msdn.microsoft.com/commandline/wsl/about)
    > * [Git)https://git-scm.com/)](https://git-scm.com/)
    > * [OpenSSH)https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)

2. Bağ yüklemek için SSH oturumundan aşağıdaki komutları kullanın:

    ```bash
    sudo apt-get update -y
    sudo apt-get install bind9 -y
    ```

3. Şirket içi DNS sunucunuz için ad çözümleme isteklerini iletmek için bağlama yapılandırmak için aşağıdaki metni içeriğini kullanın `/etc/bind/named.conf.options` dosyası:

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
    > Değerleri değiştirin `goodclients` bölümünü IP adres aralığı ile şirket içi ağ ve sanal ağ. Bu bölümde, bu DNS sunucusu gelen istekleri kabul adreslerini tanımlar.
    >
    > Değiştir `192.168.0.1` girişi `forwarders` şirket içi DNS sunucunuzun IP adresi bölümü. Bu girdi, çözümleme için şirket DNS sunucusuna DNS isteklerini yönlendirir.

    Bu dosyayı düzenlemek için aşağıdaki komutu kullanın:

    ```bash
    sudo nano /etc/bind/named.conf.options
    ```

    Dosyayı kaydetmek için kullanın __Ctrl + X__, __Y__ve ardından __Enter__.

4. SSH oturumunda aşağıdaki komutu kullanın:

    ```bash
    hostname -f
    ```

    Bu komutu aşağıdaki metni benzer bir değer döndürür:

        dnsproxy.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net

    `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` Metin __DNS soneki__ bu sanal ağ için. Bu değeri kaydedin çünkü daha sonra kullanılacaktır.

5. Sanal ağ içindeki kaynaklar için DNS adlarını çözümlemek için bağ yapılandırmak için aşağıdaki metni içeriğini kullanın `/etc/bind/named.conf.local` dosyası:

        // Replace the following with the DNS suffix for your virtual network
        zone "icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net" {
            type forward;
            forwarders {168.63.129.16;}; # The Azure recursive resolver
        };

    > [!IMPORTANT]
    > Değiştirmeniz gereken `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` daha önce DNS soneki.

    Bu dosyayı düzenlemek için aşağıdaki komutu kullanın:

    ```bash
    sudo nano /etc/bind/named.conf.local
    ```

    Dosyayı kaydetmek için kullanın __Ctrl + X__, __Y__ve ardından __Enter__.

6. Bağ başlatmak için aşağıdaki komutu kullanın:

    ```bash
    sudo service bind9 restart
    ```

7. Bu bağlama şirket içi ağınızdaki kaynaklara adlarını çözümleyebildiğini doğrulamak için aşağıdaki komutları kullanın:

    ```bash
    sudo apt install dnsutils
    nslookup dns.mynetwork.net 10.0.0.4
    ```

    > [!IMPORTANT]
    > Değiştir `dns.mynetwork.net` , şirket içi ağınıza bir kaynak tam etki alanı adını (FQDN).
    >
    > Değiştir `10.0.0.4` ile __iç IP adresi__ özel DNS sunucunuzun sanal ağda.

    Yanıt aşağıdakine benzer görünür:

        Server:         10.0.0.4
        Address:        10.0.0.4#53

        Non-authoritative answer:
        Name:   dns.mynetwork.net
        Address: 192.168.0.4

### <a name="configure-the-virtual-network-to-use-the-custom-dns-server"></a>Sanal ağ özel DNS sunucusunu kullanacak şekilde yapılandırma

Sanal ağ yerine Azure özyinelemeli çözümleyici özel DNS sunucusunu kullanacak şekilde yapılandırmak için aşağıdaki adımları kullanın:

1. İçinde [Azure portal](https://portal.azure.com)sanal ağı seçin ve ardından __DNS sunucuları__.

2. Seçin __özel__ve girin __iç IP adresi__ özel DNS sunucusu. Son olarak, seçin __kaydetmek__.

    ![Ağ için özel DNS sunucusunu ayarlayın](./media/connect-on-premises-network/configure-custom-dns.png)

### <a name="configure-the-on-premises-dns-server"></a>Şirket içi DNS sunucusunu yapılandırma

Önceki bölümde özel DNS sunucusunu isteklerini şirket DNS sunucusuna iletmek üzere yapılandırılmış. Ardından, özel DNS sunucu isteklerini iletmek için şirket içi DNS sunucusu yapılandırmanız gerekir.

DNS sunucunuzu yapılandırma hakkında belirli adımlar için DNS sunucu yazılımınızın belgelerine bakın. Yapılandırma adımları arayın bir __koşullu ileticisi__.

Koşullu iletme, yalnızca belirli bir DNS soneki isteklerini iletir. Bu durumda, sanal ağın DNS soneki için bir iletici yapılandırmanız gerekir. Bu soneki isteklerinde özel DNS sunucusunun IP adresine iletilen. 

Aşağıdaki metni için bir koşullu ileticisi yapılandırma örneğidir **bağlamak** DNS yazılım:

    zone "icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net" {
        type forward;
        forwarders {10.0.0.4;}; # The custom DNS server's internal IP address
    };

DNS kullanma hakkında bilgi için **Windows Server 2016**, bkz: [Ekle DnsServerConditionalForwarderZone](https://technet.microsoft.com/itpro/powershell/windows/dnsserver/add-dnsserverconditionalforwarderzone) belgelerine...

Şirket içi DNS sunucusunu yapılandırdıktan sonra kullanabileceğiniz `nslookup` şirket ağından sanal ağda adlarını çözümleyebildiğini doğrulayın. Aşağıdaki örnek 

```bash
nslookup dnsproxy.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net 196.168.0.4
```

Bu örnek özel DNS sunucu adını çözümlemek için 196.168.0.4 şirket içi DNS sunucusunu kullanır. Bir şirket içi DNS sunucusunun IP adresini değiştirin. Değiştir `dnsproxy` özel DNS sunucusunun tam etki alanı adı ile adres.

## <a name="optional-control-network-traffic"></a>İsteğe bağlı: Denetim ağ trafiği

Ağ trafiğini denetlemek için ağ güvenlik grubu (NSG) veya kullanıcı tanımlı yolları (UDR) kullanabilirsiniz. Nsg'ler gelen ve giden trafiği filtrelemek ve izin veren veya trafiği reddeden olanak sağlar. Udr'ler, nasıl sanal ağ, internet ve şirket içi ağ kaynaklarına arasında trafiği akan denetlemenize olanak sağlar.

> [!WARNING]
> Hdınsight Azure bulutta belirli IP adreslerinden gelen erişim ve sınırsız giden erişim gerektirir. Trafiği denetlemek için Nsg'ler veya Udr'ler kullanırken, aşağıdaki adımları gerçekleştirmeniz gerekir:

1. IP adreslerini sanal ağınızı içeren konumunu bulur. Konuma göre gerekli IP'leri listesi için bkz: [gerekli IP adreslerini](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip).

2. 1. adımda tanımlanan IP adresleri için bu IP gelen trafiğe izin adresleri.

   * Kullanıyorsanız __NSG__: izin __gelen__ bağlantı noktasında trafik __443__ için IP adresleri.
   * Kullanıyorsanız __UDR__: ayarlamak __sonraki atlama__ yol türünü __Internet__ için IP adresleri.

Nsg'ler oluşturmak için Azure PowerShell veya Azure CLI kullanma örneği için bkz: [genişletmek Hdınsight Azure sanal ağlar ile](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-nsg) belge.

## <a name="create-the-hdinsight-cluster"></a>Hdınsight kümesi oluşturma

> [!WARNING]
> Sanal ağ Hdınsight yüklemeden önce özel DNS sunucusu yapılandırmanız gerekir.

İçindeki adımları kullanın [Azure portalını kullanarak bir Hdınsight kümesi oluşturmayı](./hdinsight-hadoop-create-linux-clusters-portal.md) Hdınsight kümesi oluşturmak için belge.

> [!WARNING]
> * Küme oluşturma sırasında sanal ağınızı içeren konuma seçmeniz gerekir.
>
> * İçinde __Gelişmiş ayarları__ bölümü yapılandırmasına, daha önce oluşturduğunuz alt ağ ve sanal ağ seçmelisiniz.

## <a name="connecting-to-hdinsight"></a>Hdınsight'a bağlanma

Hdınsight üzerinde çoğu belgeleri Internet üzerinden kümesine erişimi olduğunu varsayar. Örneğin, https://CLUSTERNAME.azurehdinsight.net konumundaki kümeye bağlanabildiğiniz kabul edilir. Bu adres Nsg'ler veya Udr'ler kullandıysanız internet'ten erişimi kısıtlamak için kullanılabilir olmayan ortak ağ geçidi, kullanır.

Ayrıca bazı belgelerde başvuran `headnodehost` kümeye bir SSH oturumunda bağlanırken. Bu adres yalnızca bir küme içindeki düğümler edinilebilir ve sanal ağ üzerinden bağlı istemciler üzerinde kullanılabilir değil.

Hdınsight için sanal ağ üzerinden doğrudan bağlanmak için aşağıdaki adımları kullanın:

1. Hdınsight küme düğümlerinin iç tam etki alanı adlarını bulmak için aşağıdaki yöntemlerden birini kullanın:

    ```powershell
    $resourceGroupName = "The resource group that contains the virtual network used with HDInsight"

    $clusterNICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName | where-object {$_.Name -like "*node*"}

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

2. Hizmet kullanılabilir bağlantı noktasını belirlemek için bkz: [hdınsight'ta Hadoop Hizmetleri tarafından kullanılan bağlantı noktaları](./hdinsight-hadoop-port-settings-for-services.md) belge.

    > [!IMPORTANT]
    > Baş düğümler üzerinde barındırılan bazı hizmetler aynı anda yalnızca bir düğümde etkin olur. Bir hizmet bir baş düğüm üzerindeki erişmeyi deneyin ve başarısız olursa, diğer baş düğüme geçiş yapar.
    >
    > Örneğin, Ambari yalnızca aynı anda bir baş düğüm üzerinde etkindir. Ambari bir baş düğüm üzerinde erişmeyi deneyin ve bir 404 hatası döndürür, diğer baş düğüm üzerinde çalışıyor.

## <a name="next-steps"></a>Sonraki adımlar

* Sanal bir ağa Hdınsight kullanma hakkında daha fazla bilgi için bkz: [genişletmek Azure sanal ağları kullanarak Hdınsight](./hdinsight-extend-hadoop-virtual-network.md).

* Azure sanal ağlar hakkında daha fazla bilgi için bkz: [Azure Virtual Network'e genel bakış](../virtual-network/virtual-networks-overview.md).

* Ağ güvenlik grupları hakkında daha fazla bilgi için bkz: [ağ güvenlik grupları](../virtual-network/security-overview.md).

* Kullanıcı tanımlı yollar hakkında daha fazla bilgi için bkz: [kullanıcı tanımlı yollar ve IP iletimini](../virtual-network/virtual-networks-udr-overview.md).
