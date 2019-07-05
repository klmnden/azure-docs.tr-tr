---
title: Sanal ağ - Azure HDInsight'ı genişletin
description: HDInsight, diğer bulut kaynaklarını veya, veri merkezinizdeki kaynaklarına bağlanmak için Azure sanal ağ kullanmayı öğrenin
author: hrasheed-msft
ms.author: hrasheed
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/17/2019
ms.openlocfilehash: 61a208f3e84125acc2a3cb22d3abccf16587e581
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67543687"
---
# <a name="extend-azure-hdinsight-using-an-azure-virtual-network"></a>Azure HDInsight'ın bir Azure sanal ağı kullanarak genişletme

HDInsight ile kullanmayı öğrenin bir [Azure sanal ağı](../virtual-network/virtual-networks-overview.md). Bir Azure sanal ağı kullanarak aşağıdaki senaryolar sağlar:

* HDInsight için bir şirket içi ağdan doğrudan bağlanma.

* HDInsight verilerine bağlantı kurma, bir Azure sanal ağında depolar.

* Doğrudan erişim [Apache Hadoop](https://hadoop.apache.org/) genel internet üzerinden kullanılabilir olmayan hizmetler. Örneğin, [Apache Kafka](https://kafka.apache.org/) API'leri veya [Apache HBase](https://hbase.apache.org/) Java API'si.

> [!IMPORTANT]  
> 28 Şubat 2019'dan sonra bir sanal ağda oluşturulan yeni kümeleri için ağ kaynakları (örneğin, NIC, lb, vb.) aynı HDInsight küme kaynak grubunda sağlanır. Daha önce bu kaynaklar sanal ağ kaynağı grubunda sağlanan. Geçerli çalışan kümeler ve bu küme bir VNET oluşturduğunuzu değişiklik yoktur.

## <a name="prerequisites-for-code-samples-and-examples"></a>Kod örnekleri ve önkoşulları

* TCP/IP ağı olma. TCP/IP ağ bağlantısı ile ilgili bilgi sahibi değilseniz, üretim ağları için değişiklik yapmadan önce olan bir kullanıcıyla ortak.
* PowerShell kullanarak, ihtiyacınız olacak [AZ modül](https://docs.microsoft.com/powershell/azure/overview).
* Kullanmak isteyen eğitimcilere ve Azure CLI'yi henüz yüklemediyseniz, bkz: [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli).

> [!IMPORTANT]  
> HDInsight için şirket içi bağlanma konusunda adım adım yönergeler arıyorsanız, Azure sanal ağı kullanarak ağ, bkz [HDInsight'ı şirket içi ağınıza bağlama](connect-on-premises-network.md) belge.

## <a name="planning"></a>Planlama

Bir sanal ağda HDInsight'ı yüklemek planlama yaparken yanıt sorular şunlardır:

* Mevcut bir sanal ağa HDInsight'ı yüklemek ihtiyacınız var? Veya yeni bir ağ oluşturuyorsunuz?

    Bir sanal ağınız kullanıyorsanız, HDInsight'ı yüklemeden önce ağ yapılandırmasını değiştirmeniz gerekebilir. Daha fazla bilgi için [HDInsight mevcut bir sanal ağa ekleme](#existingvnet) bölümü.

* Başka bir sanal ağ veya şirket içi ağınıza HDInsight içeren sanal ağa bağlanmak istiyor musunuz?

    Kolayca ağlarda kaynak ile çalışmak için özel bir DNS oluşturun ve DNS iletme'yi yapılandırma gerekebilir. Daha fazla bilgi için [birden çok ağları bağlama](#multinet) bölümü.

* HDInsight için gelen ve giden trafiği kısıtlama/yeniden yönlendirme istiyor musunuz?

    HDInsight Azure veri merkezindeki belirli IP adresleri ile iletişim sınırsız gerekir. İstemci iletişimi için güvenlik duvarlarından izin verilmelidir çeşitli bağlantı noktaları vardır. Daha fazla bilgi için [ağ trafiğini denetleme](#networktraffic) bölümü.

## <a id="existingvnet"></a>HDInsight, mevcut bir sanal ağa ekleme

Nasıl yeni bir HDInsight mevcut bir Azure sanal ağına eklemek keşfetmek için bu bölümdeki adımları kullanın.

> [!NOTE]  
> Bir sanal ağa var olan bir HDInsight kümesine eklenemiyor.

1. Sanal ağ için bir Klasik veya Resource Manager dağıtım modelini kullanıyor musunuz?

    HDInsight 3.4 ve büyük bir Resource Manager sanal ağı gerekir. HDInsight'ın önceki sürümlerinde, bir Klasik sanal ağ gereklidir.

    Ardından mevcut ağınıza klasik bir sanal ağ, bir Resource Manager sanal ağı oluşturmak ve sonra iki bağlanma gerekir. [Klasik sanal ağları yeni sanal ağlara bağlama](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

    Birleştirilmiş sonra Resource Manager ağında yüklü HDInsight Klasik ağ içindeki kaynaklarla etkileşim kurabilir.

2. İçine veya dışına sanal ağ trafiği kısıtlamak için ağ güvenlik grupları, kullanıcı tanımlı yollar ve ağ sanal Gereçleri kullanıyorsunuz?

    Yönetilen bir hizmet olarak HDInsight, Azure veri merkezinde birden fazla IP adresi için sınırsız erişim gerektirir. Bu IP adresleri ile iletişime izin vermek için herhangi bir mevcut ağ güvenlik grupları veya kullanıcı tanımlı yollar güncelleştirin.
    
    HDInsight, çeşitli bağlantı noktaları kullanan birden çok hizmetleri barındırır. Bu bağlantı noktaları için trafiği engellemediğinizden. Sanal gereç güvenlik duvarlarından izin vermek için bağlantı noktalarının listesi için güvenlik bölümüne bakın.
    
    Mevcut güvenlik yapılandırmanızı bulmak için aşağıdaki Azure PowerShell veya Azure CLI komutları kullanın:

    * Ağ güvenlik grupları

        Değiştirin `RESOURCEGROUP` adıyla kaynak grubunun sanal ağı içeren ve ardından komutu girin:
    
        ```powershell
        Get-AzNetworkSecurityGroup -ResourceGroupName  "RESOURCEGROUP"
        ```
    
        ```azurecli
        az network nsg list --resource-group RESOURCEGROUP
        ```

        Daha fazla bilgi için [ağ güvenlik gruplarında sorun giderme](../virtual-network/diagnose-network-traffic-filter-problem.md) belge.

        > [!IMPORTANT]  
        > Ağ güvenlik grubu kuralları kural önceliği temelinde sırayla uygulanır. Trafik deseni ile eşleşen ilk kural uygulanır ve hiçbir diğerleri için o trafiğe uygulanır. Sipariş en esnek en katı kuralları. Daha fazla bilgi için [ağ güvenlik grupları ile ağ trafiğini filtreleme](../virtual-network/security-overview.md) belge.

    * Kullanıcı tanımlı yollar

        Değiştirin `RESOURCEGROUP` adıyla kaynak grubunun sanal ağı içeren ve ardından komutu girin:

        ```powershell
        Get-AzRouteTable -ResourceGroupName "RESOURCEGROUP"
        ```

        ```azurecli
        az network route-table list --resource-group RESOURCEGROUP
        ```

        Daha fazla bilgi için [yollarla ilgili sorunları giderme](../virtual-network/diagnose-network-routing-problem.md) belge.

3. Bir HDInsight kümesi oluşturma ve yapılandırma sırasında Azure sanal ağı seçin. Adımları, küme oluşturma işlemi anlamak için aşağıdaki belgeleri kullanın:

    * [Azure portalını kullanarak HDInsight oluşturma](hdinsight-hadoop-create-linux-clusters-portal.md)
    * [Azure PowerShell kullanarak HDInsight oluşturma](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
    * [Klasik Azure CLI kullanarak HDInsight oluşturma](hdinsight-hadoop-create-linux-clusters-azure-cli.md)
    * [Bir Azure Resource Manager şablonu kullanarak HDInsight oluşturma](hdinsight-hadoop-create-linux-clusters-arm-templates.md)

   > [!IMPORTANT]  
   > HDInsight'ı bir sanal ağa ekleyerek bir isteğe bağlı yapılandırma adımdır. Küme yapılandırma sırasında sanal ağ'ı seçtiğinizden emin olun.

## <a id="multinet"></a>Birden çok ağları bağlama

En büyük güçlük birden çok ağ yapılandırmasına sahip ağlar arasında ad çözümlemesine ' dir.

Azure sanal ağında yüklü olan Azure Hizmetleri için ad çözümleme sağlar. HDInsight, tam etki alanı adını (FQDN) kullanarak aşağıdaki kaynaklara bağlanmak bu yerleşik ad çözümlemesi sağlar:

* İnternet üzerinde kullanılabilir olan herhangi bir kaynaktır. Örneğin, microsoft.com, windowsupdate.com.

* Aynı Azure sanal ağında, kullanarak herhangi bir kaynağa __iç DNS ad__ kaynak. Örneğin, varsayılan ad çözümlemesi kullanırken, örnek HDInsight çalışan düğümlerine atanan iç DNS adları şunlardır:

  * wn0-hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net
  * wn2-hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net

    Hem bu düğümler doğrudan birbirine ve HDInsight, diğer düğümleri iç DNS adları kullanarak iletişim kurabilir.

Varsayılan ad çözümlemesini yapar __değil__ sanal ağa katılan ağlardaki kaynakların adlarını çözümlemek HDInsight izin. Örneğin, sanal ağa şirket içi ağınıza katılmak için yaygındır. Yalnızca varsayılan ad çözümlemesi ile HDInsight adına göre şirket ağındaki kaynaklara erişemez. Şirket içi ağınızdaki kaynaklara adına göre sanal ağ içindeki kaynaklarla erişilemiyor, bunun tersi de geçerlidir.

> [!WARNING]  
> Özel DNS sunucusu oluşturup HDInsight küme oluşturmadan önce kullanmak için sanal ağ yapılandırmanız gerekir.

Sanal ağ ve birleştirilmiş ağlardaki kaynaklar arasında ad çözümlemesine etkinleştirmek için aşağıdaki eylemleri gerçekleştirmeniz gerekir:

1. Azure sanal HDInsight yüklemeyi planladığınız ağ üzerinde özel bir DNS sunucusu oluşturun.

2. Sanal ağ özel DNS sunucusunu kullanacak şekilde yapılandırın.

3. Azure sanal ağınız için DNS son eki atanan bulun. Bu değer benzer `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net`. DNS son eki bulma hakkında daha fazla bilgi için bkz. [örneği: Özel DNS](#example-dns) bölümü.

4. DNS sunucuları arasında iletim yapılandırın. Yapılandırma, uzak ağ türüne bağlıdır.

   * Uzak ağ ile şirket içi ağ ise, DNS gibi yapılandırın:
        
     * __Özel DNS__ (sanal ağdaki):

         * Azure yinelemeli çözümleyici (168.63.129.16) sanal ağa ait DNS sonekini istekleri iletmek. Azure sanal ağdaki kaynaklar için istekleri işler

         * Şirket içi DNS sunucusuna tüm istekleri iletin. Şirket içi DNS diğer ad çözümleme isteklerinin Microsoft.com gibi Internet kaynaklarına yönelik bile istekleri işler.

     * __Şirket içi DNS__: Özel DNS sunucusu için sanal ağ DNS soneki için istekleri iletmek. Özel DNS sunucusu için Azure özyinelemeli çözümleyici ardından iletir.

       Bu yapılandırma yollar istekleri için sanal ağ özel DNS sunucusuna DNS sonekinde etki alanı adları tam. Diğer tüm istekler (hatta için genel internet adresleri), şirket içi DNS sunucusu tarafından işlenir.

   * Uzak ağ başka bir Azure sanal ağ değilse, DNS gibi yapılandırın:

     * __Özel DNS__ (içinde her sanal ağ için):

         * DNS soneki sanal ağlar için istekleri, özel DNS sunucularını iletilir. Her bir sanal ağ DNS, alt ağ içindeki kaynakları çözümleme için sorumludur.

         * Diğer tüm istekler için Azure özyinelemeli çözümleyici iletin. Özyinelemeli çözümleyici yerel çözme ve internet kaynakları sorumludur.

       DNS sunucusu DNS son ekini temel alınarak her ağ diğer istekleri iletir. Azure yinelemeli çözümleyici kullanarak diğer istekleri çözümlenir.

     Her yapılandırma örneği için bkz: [örneği: Özel DNS](#example-dns) bölümü.

Daha fazla bilgi için [VM'ler ve rol örnekleri için ad çözümlemesi](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) belge.

## <a name="directly-connect-to-apache-hadoop-services"></a>Apache Hadoop Hizmetleri için doğrudan bağlanma

Konumundaki kümeye bağlanabilirsiniz `https://CLUSTERNAME.azurehdinsight.net`. Bu adres, internet'ten gelen trafiği kısıtlamak için Nsg kullandıysanız erişilebilir olmayabilir genel bir IP kullanır. Bir sanal ağda küme dağıttığınızda ek olarak, özel uç nokta kullanarak erişebileceğiniz `https://CLUSTERNAME-int.azurehdinsight.net`. Bu uç nokta kümesi erişim için sanal ağ içinde bir özel IP çözümler.

Apache Ambari ve sanal ağ üzerinden diğer web sayfalarına bağlanmak için aşağıdaki adımları kullanın:

1. HDInsight küme düğümleri dahili tam etki alanı adlarını (FQDN) bulmak için aşağıdaki yöntemlerden birini kullanın:

    Değiştirin `RESOURCEGROUP` adıyla kaynak grubunun sanal ağı içeren ve ardından komutu girin:

    ```powershell
    $clusterNICs = Get-AzNetworkInterface -ResourceGroupName "RESOURCEGROUP" | where-object {$_.Name -like "*node*"}

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
    az network nic list --resource-group RESOURCEGROUP --output table --query "[?contains(name,'node')].{NICname:name,InternalIP:ipConfigurations[0].privateIpAddress,InternalFQDN:dnsSettings.internalFqdn}"
    ```

    Döndürülen düğümleri listesinde, FQDN için baş düğümlerine bulup, Ambari ve diğer web hizmetlerine bağlanmak için FQDN'leri kullanın. Örneğin, `http://<headnode-fqdn>:8080` Ambari erişmek için.

    > [!IMPORTANT]  
    > Baş düğümler üzerinde barındırılan bazı hizmetler aynı anda yalnızca bir düğümde etkin olan. Bir baş düğüm üzerinde bir hizmete erişim deneyin ve bir 404 hatası döndürürse diğer baş düğümüne geçin.

2. Düğüm ve kullanılabilir bir hizmet bağlantı noktasını belirlemek için bkz: [HDInsight üzerindeki Hadoop Hizmetleri tarafından kullanılan bağlantı noktaları](./hdinsight-hadoop-port-settings-for-services.md) belge.

## <a id="networktraffic"></a> Ağ trafiğini denetleme

### <a name="techniques-for-controlling-inbound-and-outbound-traffic-to-hdinsight-clusters"></a>HDInsight kümeleri gelen ve giden trafiği denetlemek için teknikleri

Aşağıdaki yöntemleri kullanarak bir Azure sanal ağlarda ağ trafiğini denetlenebilir:

* **Ağ güvenlik grupları** (NSG) ağa gelen ve giden trafiği filtrelemenize olanak tanır. Daha fazla bilgi için [ağ güvenlik grupları ile ağ trafiğini filtreleme](../virtual-network/security-overview.md) belge.

* **Ağ sanal Gereçleri** (NVA) yalnızca giden trafik ile kullanılabilir. Nva'ları güvenlik duvarları ve yönlendiriciler gibi işlevselliğini yineler.  Daha fazla bilgi için [ağ Gereçleri](https://azure.microsoft.com/solutions/network-appliances) belge.

Yönetilen bir hizmet olarak HDInsight, HDInsight sistem sınırsız erişim gerektirir ve sanal ağdan gelen ve giden trafiği için hem de Yönetim Hizmetleri. Nsg'leri kullanarak, bu hizmetler HDInsight kümesiyle iletişim kurabildiğinden emin olmanız gerekir.

![Oluşturulan özel Azure VNET'te HDInsight varlıklar diyagramı](./media/hdinsight-virtual-network-architecture/vnet-diagram.png)

### <a name="hdinsight-with-network-security-groups"></a>Ağ güvenlik grupları ile HDInsight

Kullanmayı planlıyorsanız **ağ güvenlik grupları** ağ trafiğinizi denetlemek için HDInsight'ı yüklemeden önce aşağıdaki eylemleri gerçekleştirin:

1. HDInsight için kullanmayı düşündüğünüz Azure bölgesinin belirleyin.

2. HDInsight tarafından gerekli IP adresleri belirleyin. Daha fazla bilgi için [HDInsight tarafından gerekli IP adresleri](#hdinsight-ip) bölümü.

3. Oluşturabilir veya HDInsight içine yüklemeyi planladığınız alt ağ için ağ güvenlik gruplarını değiştirebilirsiniz.

    * __Ağ güvenlik grupları__: izin __gelen__ bağlantı noktası üzerinde trafiğe __443__ IP adresleri. Böylece, HDInsight Yönetimi Hizmetleri sanal ağın dışında kümeden ulaşabildiğimizden emin olursunuz.

Ağ güvenlik grupları hakkında daha fazla bilgi için bkz. [ağ güvenlik gruplarına genel bakış](../virtual-network/security-overview.md).

### <a name="controlling-outbound-traffic-from-hdinsight-clusters"></a>HDInsight kümeleri giden trafiği denetleme

HDInsight kümeleri giden trafiği denetleme hakkında daha fazla bilgi için bkz: [Azure HDInsight kümeleri için giden ağ trafiği kısıtlama yapılandırma](hdinsight-restrict-outbound-traffic.md).

#### <a name="forced-tunneling-to-on-premise"></a>Şirket içi zorlamalı

Zorlamalı tünel bir kullanıcı tanımlı yönlendirme burada tüm trafiğin bir alt ağdan belirli ağ veya şirket içi ağınız gibi konuma zorlanır yapılandırmadır. HDInsight mu __değil__ destek zorlamalı tünel şirket içi ağlara trafiği. 

## <a id="hdinsight-ip"></a> Gerekli IP adresleri

Trafiği denetlemek için ağ güvenlik grupları veya kullanıcı tanımlı yollar kullanıyorsanız, HDInsight kümenizle iletişim kurabilmesi için Azure sistem durumu ve Yönetim Hizmetleri IP adreslerinden gelen trafiğe izin vermelidir. Bölge belirli IP adreslerinden bazıları ve bazıları tüm Azure bölgelerine uygulayın. Özel DNS kullanmıyorsanız, Azure DNS hizmeti gelen trafiğe izin gerekebilir. Alt ağ içindeki VM'ler arasında trafiği de izin vermeniz gerekir. İzin verilmiş olmalıdır IP adreslerini bulmak için aşağıdaki adımları kullanın:

> [!Note]  
> Bu bölümde, trafiği denetlemek için ağ güvenlik grupları veya kullanıcı tanımlı yollar kullanmazsanız, yoksayabilirsiniz.

1. Azure tarafından sağlanan DNS hizmeti kullanıyorsanız erişime izin verecek __168.63.129.16__ bağlantı noktası 53. Daha fazla bilgi için [VM'ler ve rol için ad çözümlemesi örnekleri](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) belge. Özel DNS kullanıyorsanız bu adımı atlayın.

2. Tüm Azure bölgeleri için geçerli Azure sistem durumu ve Yönetim Hizmetleri için aşağıdaki IP adreslerinden gelen trafiği izin ver:

    | Kaynak IP adresi | Hedef  | Direction |
    | ---- | ----- | ----- |
    | 168.61.49.99 | \*:443 | Gelen |
    | 23.99.5.239 | \*:443 | Gelen |
    | 168.61.48.131 | \*:443 | Gelen |
    | 138.91.141.162 | \*:443 | Gelen |

3. Kaynaklarınızı bulunduğu yere belirli bölgedeki Azure sistem durumu ve Yönetim Hizmetleri için listedeki IP adreslerinden gelen trafiğe izin ver:

    > [!IMPORTANT]  
    > Kullanmakta olduğunuz Azure bölgesi listede yoksa, yalnızca adım 1'deki dört IP adreslerini kullanır.

    | Country | Bölge | İzin verilen kaynak IP adresleri | İzin verilen hedef | Direction |
    | ---- | ---- | ---- | ---- | ----- |
    | Asya | Doğu Asya | 23.102.235.122</br>52.175.38.134 | \*:443 | Gelen |
    | &nbsp; | Güneydoğu Asya | 13.76.245.160</br>13.76.136.249 | \*:443 | Gelen |
    | Avustralya | Avustralya Orta | 20.36.36.33</br>20.36.36.196 | \*:443 | Gelen |
    | &nbsp; | Avustralya Doğu | 104.210.84.115</br>13.75.152.195 | \*:443 | Gelen |
    | &nbsp; | Avustralya Güneydoğu | 13.77.2.56</br>13.77.2.94 | \*:443 | Gelen |
    | Brezilya | Güney Brezilya | 191.235.84.104</br>191.235.87.113 | \*:443 | Gelen |
    | Kanada | Doğu Kanada | 52.229.127.96</br>52.229.123.172 | \*:443 | Gelen |
    | &nbsp; | Orta Kanada | 52.228.37.66</br>52.228.45.222 |\*: 443 | Gelen |
    | Çin | Çin Kuzey | 42.159.96.170</br>139.217.2.219</br></br>42.159.198.178</br>42.159.234.157 | \*:443 | Gelen |
    | &nbsp; | Çin Doğu | 42.159.198.178</br>42.159.234.157</br></br>42.159.96.170</br>139.217.2.219 | \*:443 | Gelen |
    | &nbsp; | Çin Kuzey 2 | 40.73.37.141</br>40.73.38.172 | \*:443 | Gelen |
    | &nbsp; | Çin Doğu 2 | 139.217.227.106</br>139.217.228.187 | \*:443 | Gelen |
    | Avrupa | Kuzey Avrupa | 52.164.210.96</br>13.74.153.132 | \*:443 | Gelen |
    | &nbsp; | Batı Avrupa| 52.166.243.90</br>52.174.36.244 | \*:443 | Gelen |
    | Fransa | Fransa Orta| 20.188.39.64</br>40.89.157.135 | \*:443 | Gelen |
    | Almanya | Almanya Orta | 51.4.146.68</br>51.4.146.80 | \*:443 | Gelen |
    | &nbsp; | Almanya Kuzeydoğu | 51.5.150.132</br>51.5.144.101 | \*:443 | Gelen |
    | Hindistan | Orta Hindistan | 52.172.153.209</br>52.172.152.49 | \*:443 | Gelen |
    | &nbsp; | Güney Hindistan | 104.211.223.67<br/>104.211.216.210 | \*:443 | Gelen |
    | Japonya | Japonya Doğu | 13.78.125.90</br>13.78.89.60 | \*:443 | Gelen |
    | &nbsp; | Japonya Batı | 40.74.125.69</br>138.91.29.150 | \*:443 | Gelen |
    | Güney Kore | Kore Orta | 52.231.39.142</br>52.231.36.209 | \*:443 | Gelen |
    | &nbsp; | Kore Güney | 52.231.203.16</br>52.231.205.214 | \*:443 | Gelen
    | Birleşik Krallık | Birleşik Krallık Batı | 51.141.13.110</br>51.141.7.20 | \*:443 | Gelen |
    | &nbsp; | Birleşik Krallık Güney | 51.140.47.39</br>51.140.52.16 | \*:443 | Gelen |
    | Amerika Birleşik Devletleri | Orta ABD | 13.89.171.122</br>13.89.171.124 | \*:443 | Gelen |
    | &nbsp; | East US | 13.82.225.233</br>40.71.175.99 | \*:443 | Gelen |
    | &nbsp; | Orta Kuzey ABD | 157.56.8.38</br>157.55.213.99 | \*:443 | Gelen |
    | &nbsp; | Batı Orta ABD | 52.161.23.15</br>52.161.10.167 | \*:443 | Gelen |
    | &nbsp; | Batı ABD | 13.64.254.98</br>23.101.196.19 | \*:443 | Gelen |
    | &nbsp; | Batı ABD 2 | 52.175.211.210</br>52.175.222.222 | \*:443 | Gelen |

    Azure kamu için kullanılacak IP adresleri hakkında daha fazla bilgi için bkz: [Azure kamu INTELLIGENCE + Analytıcs](https://docs.microsoft.com/azure/azure-government/documentation-government-services-intelligenceandanalytics) belge.

Daha fazla bilgi için [ağ trafiğini denetleme](#networktraffic) bölümü.

Kullanıcı tanımlı routes(UDRs) kullanıyorsanız, bir yol belirtin ve "Internet" sanal ağdan giden trafik sonraki atlama yukarıdaki IP'ler için ayarlanmış izin gerekir.
    
## <a id="hdinsight-ports"></a> Gerekli bağlantı noktaları

Kullanmayı planlıyorsanız bir **Güvenlik Duvarı** ve küme dışındaki belirli noktalarına erişmek için senaryonuz için gerekli olan bu bağlantı noktalarında trafiğe izin verecek şekilde gerekebilir. Önceki bölümde açıklanan azure yönetim trafiğini küme bağlantı noktası 443 üzerinden erişmek için izin verilen sürece varsayılan olarak, hiçbir özel beyaz listeye ekleme bağlantı noktası gereklidir.

Belirli hizmetlere yönelik bağlantı noktalarının listesi için bkz. [HDInsight üzerinde Apache Hadoop Hizmetleri tarafından kullanılan bağlantı noktaları](hdinsight-hadoop-port-settings-for-services.md) belge.

Sanal gereçler için güvenlik duvarı kuralları hakkında daha fazla bilgi için bkz. [sanal gereç senaryo](../virtual-network/virtual-network-scenario-udr-gw-nva.md) belge.

## <a id="hdinsight-nsg"></a>Örnek: ağ güvenlik grupları ile HDInsight

Bu bölümdeki örneklerde, ağ güvenliği HDInsight, Azure Yönetim Hizmetleri ile iletişim kurmasına izin ver Grup kuralları oluşturmak nasıl ekleyebileceğiniz gösterilmektedir. Örnekleri kullanmadan önce kullanmakta olduğunuz Azure bölgesi için olanlarla eşleşmesi için IP adreslerini ayarlayın. Bu bilgiler bulabilirsiniz [HDInsight ile ağ güvenlik gruplarını ve kullanıcı tanımlı yollar](#hdinsight-ip) bölümü.

### <a name="azure-resource-management-template"></a>Azure kaynak yönetimi şablonu

Aşağıdaki kaynak yönetimi şablonu, gelen trafiği kısıtlar ancak HDInsight tarafından gerekli IP adreslerinden gelen trafiğe izin veren bir sanal ağ oluşturur. Bu şablon, ayrıca sanal ağ içinde bir HDInsight kümesi oluşturur.

* [Güvenli bir Azure sanal ağ ve bir HDInsight Hadoop kümesi dağıtma](https://azure.microsoft.com/resources/templates/101-hdinsight-secure-vnet/)

### <a name="azure-powershell"></a>Azure PowerShell

Gelen trafiği sınırlayan ve Kuzey Avrupa bölgesinde için IP adreslerinden gelen trafiğe izin veren bir sanal ağ oluşturmak için aşağıdaki PowerShell betiğini kullanın.

> [!IMPORTANT]  
> IP adreslerini değiştirmek `hdirule1` ve `hdirule2` Bu örnekte, kullanmakta olduğunuz Azure bölgesi eşleştirilecek. Bu bilgiler bulabilirsiniz [HDInsight ile ağ güvenlik gruplarını ve kullanıcı tanımlı yollar](#hdinsight-ip) bölümü.

```powershell
$vnetName = "Replace with your virtual network name"
$resourceGroupName = "Replace with the resource group the virtual network is in"
$subnetName = "Replace with the name of the subnet that you plan to use for HDInsight"

# Get the Virtual Network object
$vnet = Get-AzVirtualNetwork `
    -Name $vnetName `
    -ResourceGroupName $resourceGroupName

# Get the region the Virtual network is in.
$location = $vnet.Location

# Get the subnet object
$subnet = $vnet.Subnets | Where-Object Name -eq $subnetName

# Create a Network Security Group.
# And add exemptions for the HDInsight health and management services.
$nsg = New-AzNetworkSecurityGroup `
    -Name "hdisecure" `
    -ResourceGroupName $resourceGroupName `
    -Location $location `
    | Add-AzNetworkSecurityRuleConfig `
        -name "hdirule1" `
        -Description "HDI health and management address 52.164.210.96" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "52.164.210.96" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 300 `
        -Direction Inbound `
    | Add-AzNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 13.74.153.132" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "13.74.153.132" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 301 `
        -Direction Inbound `
    | Add-AzNetworkSecurityRuleConfig `
        -Name "hdirule3" `
        -Description "HDI health and management 168.61.49.99" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "168.61.49.99" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 302 `
        -Direction Inbound `
    | Add-AzNetworkSecurityRuleConfig `
        -Name "hdirule4" `
        -Description "HDI health and management 23.99.5.239" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "23.99.5.239" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 303 `
        -Direction Inbound `
    | Add-AzNetworkSecurityRuleConfig `
        -Name "hdirule5" `
        -Description "HDI health and management 168.61.48.131" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "168.61.48.131" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 304 `
        -Direction Inbound `
    | Add-AzNetworkSecurityRuleConfig `
        -Name "hdirule6" `
        -Description "HDI health and management 138.91.141.162" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "138.91.141.162" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 305 `
        -Direction Inbound `

# Set the changes to the security group
Set-AzNetworkSecurityGroup -NetworkSecurityGroup $nsg

# Apply the NSG to the subnet
Set-AzVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name $subnetName `
    -AddressPrefix $subnet.AddressPrefix `
    -NetworkSecurityGroup $nsg
$vnet | Set-AzVirtualNetwork
```

Bu örnek, gerekli IP adreslerini gelen trafiğe izin vermek için kural eklemek üzere nasıl gösterir. Diğer kaynaklardan gelen erişimi kısıtlamak için bir kuralı içermiyor. Aşağıdaki kod, Internet'ten SSH erişimini etkinleştirmek gösterilmektedir:

```powershell
Get-AzNetworkSecurityGroup -Name hdisecure -ResourceGroupName RESOURCEGROUP |
Add-AzNetworkSecurityRuleConfig -Name "SSH" -Description "SSH" -Protocol "*" -SourcePortRange "*" -DestinationPortRange "22" -SourceAddressPrefix "*" -DestinationAddressPrefix "VirtualNetwork" -Access Allow -Priority 306 -Direction Inbound
```

### <a name="azure-cli"></a>Azure CLI

Gelen trafiği kısıtlar ancak HDInsight tarafından gerekli IP adreslerinden gelen trafiğe izin veren bir sanal ağ oluşturmak için aşağıdaki adımları kullanın.

1. Adlı yeni bir ağ güvenlik grubu oluşturmak için aşağıdaki komutu kullanın `hdisecure`. Değiştirin `RESOURCEGROUP` kaynak grubuyla Azure sanal ağı içerir. Değiştirin `LOCATION` grubunun oluşturulduğu konum (bölge).

    ```azurecli
    az network nsg create -g RESOURCEGROUP -n hdisecure -l LOCATION
    ```

    Grup oluşturulduktan sonra yeni grubu hakkında bilgi alırsınız.

2. Azure HDInsight sistem durumu ve yönetim hizmetinden 443 numaralı bağlantı noktasında gelen iletişime izin verecek yeni bir ağ güvenlik grubu kuralları eklemek için aşağıdakileri kullanın. Değiştirin `RESOURCEGROUP` ile Azure sanal ağı içeren kaynak grubunun adı.

    > [!IMPORTANT]  
    > IP adreslerini değiştirmek `hdirule1` ve `hdirule2` Bu örnekte, kullanmakta olduğunuz Azure bölgesi eşleştirilecek. Bu bilgiler bulabilirsiniz [HDInsight ile ağ güvenlik gruplarını ve kullanıcı tanımlı yollar](#hdinsight-ip) bölümü.

    ```azurecli
    az network nsg rule create -g RESOURCEGROUP --nsg-name hdisecure -n hdirule1 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "52.164.210.96" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 300 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUP --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "13.74.153.132" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 301 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUP --nsg-name hdisecure -n hdirule3 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "168.61.49.99" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 302 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUP --nsg-name hdisecure -n hdirule4 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "23.99.5.239" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 303 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUP --nsg-name hdisecure -n hdirule5 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "168.61.48.131" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 304 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUP --nsg-name hdisecure -n hdirule6 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "138.91.141.162" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 305 --direction "Inbound"
    ```

3. Bu ağ güvenlik grubu için benzersiz tanımlayıcı almak için aşağıdaki komutu kullanın:

    ```azurecli
    az network nsg show -g RESOURCEGROUP -n hdisecure --query "id"
    ```

    Bu komut, aşağıdaki metne benzer bir değer döndürür:

        "/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Network/networkSecurityGroups/hdisecure"

4. Bir alt ağ için ağ güvenlik grubunu uygulamak için aşağıdaki komutu kullanın. Değiştirin `GUID` ve `RESOURCEGROUP` fiyatlarla değerleri önceki adımdaki döndürdü. Değiştirin `VNETNAME` ve `SUBNETNAME` oluşturmak istediğiniz alt ağ adı ve sanal ağ adı.

    ```azurecli
    az network vnet subnet update -g RESOURCEGROUP --vnet-name VNETNAME --name SUBNETNAME --set networkSecurityGroup.id="/subscriptions/GUID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Network/networkSecurityGroups/hdisecure"
    ```

    Bu komut tamamlandığında, sanal ağa HDInsight yükleyebilirsiniz.


Bu adımlar, yalnızca Azure bulut üzerindeki HDInsight sistem durumu ve yönetim hizmetine erişim açın. HDInsight küme sanal ağ dışındaki diğer tüm erişim engellenir. Sanal Ağ dışından erişim etkinleştirmek için ek ağ güvenlik grubu kuralları eklemeniz gerekir.

Aşağıdaki kod, Internet'ten SSH erişimini etkinleştirmek gösterilmektedir:

```azurecli
az network nsg rule create -g RESOURCEGROUP --nsg-name hdisecure -n ssh --protocol "*" --source-port-range "*" --destination-port-range "22" --source-address-prefix "*" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 306 --direction "Inbound"
```

## <a id="example-dns"></a> Örnek: DNS yapılandırması

### <a name="name-resolution-between-a-virtual-network-and-a-connected-on-premises-network"></a>Bir sanal ağ ile bağlı şirket içi ağ arasında ad çözümleme

Bu örnek aşağıdaki varsayımların yapar:

* Bir Azure sanal bir VPN ağ geçidi kullanarak şirket ağına bağlı ağ var.

* Sanal ağda özel DNS sunucusu, Linux veya UNIX işletim sistemi olarak çalışıyor.

* [Bağlama](https://www.isc.org/downloads/bind/) özel DNS sunucusuna yüklenir.

Özel DNS sunucusunda sanal ağ:

1. Sanal ağın DNS soneki bulmak için Azure PowerShell veya Azure CLI'yı kullanın:

    Değiştirin `RESOURCEGROUP` adıyla kaynak grubunun sanal ağı içeren ve ardından komutu girin:

    ```powershell
    $NICs = Get-AzNetworkInterface -ResourceGroupName "RESOURCEGROUP"
    $NICs[0].DnsSettings.InternalDomainNameSuffix
    ```

    ```azurecli
    az network nic list --resource-group RESOURCEGROUP --query "[0].dnsSettings.internalDomainNameSuffix"
    ```

2. Sanal ağ için özel DNS sunucusunda aşağıdaki metni içeriğini kullanın. `/etc/bind/named.conf.local` dosyası:

    ```
    // Forward requests for the virtual network suffix to Azure recursive resolver
    zone "0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net" {
        type forward;
        forwarders {168.63.129.16;}; # Azure recursive resolver
    };
    ```

    Değiştirin `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` sanal ağınızın DNS soneki ile değeri.

    Bu yapılandırma sanal ağın DNS soneki için tüm DNS istekleri için Azure özyinelemeli çözümleyici yönlendirir.

2. Sanal ağ için özel DNS sunucusunda aşağıdaki metni içeriğini kullanın. `/etc/bind/named.conf.options` dosyası:

    ```
    // Clients to accept requests from
    // TODO: Add the IP range of the joined network to this list
    acl goodclients {
        10.0.0.0/16; # IP address range of the virtual network
        localhost;
        localnets;
    };

    options {
            directory "/var/cache/bind";

            recursion yes;

            allow-query { goodclients; };

            # All other requests are sent to the following
            forwarders {
                192.168.0.1; # Replace with the IP address of your on-premises DNS server
            };

            dnssec-validation auto;

            auth-nxdomain no;    # conform to RFC1035
            listen-on { any; };
    };
    ```
    
    * Değiştirin `10.0.0.0/16` değeri ile sanal ağ IP adresi aralığı. Bu giriş, bu aralıktaki ad çözümleme istekleri adreslerini sağlar.

    * Şirket içi ağ için IP adres aralığı Ekle `acl goodclients { ... }` bölümü.  Giriş şirket içi ağdaki kaynakları adı çözümlemesi isteklerinden sağlar.
    
    * Değeri Değiştir `192.168.0.1` ile şirket içi DNS sunucunuzun IP adresidir. Bu giriş, diğer tüm DNS istekleri şirket içi DNS sunucusuna yönlendirir.

3. Yapılandırmayı kullanmak için bağlama yeniden başlatın. Örneğin, `sudo service bind9 restart`.

4. Koşullu ileticisi şirket içi DNS sunucusuna ekleyin. Adım 1'den özel DNS sunucusuna DNS soneki için istekleri göndermeye koşullu ileticisi yapılandırın.

    > [!NOTE]  
    > Koşullu ileticisi ekleme hakkında daha fazla ayrıntı için DNS yazılımınızın belgelerine bakın.

Bu adımları tamamladıktan sonra tam etki alanı adlarını (FQDN) kullanarak ya da ağ içindeki kaynaklarla bağlanabilirsiniz. HDInsight, artık sanal ağa yükleyebilirsiniz.

### <a name="name-resolution-between-two-connected-virtual-networks"></a>İki bağlı sanal ağlar arasında ad çözümleme

Bu örnek aşağıdaki varsayımların yapar:

* İki Azure sanal bir VPN ağ geçidi kullanarak veya eşleme bağlı ağ var.

* Her iki ağ özel DNS sunucusu, Linux veya UNIX işletim sistemi olarak çalışıyor.

* [Bağlama](https://www.isc.org/downloads/bind/) özel DNS sunucularında yüklü.

1. Her iki sanal ağ DNS sonekini bulmak için Azure PowerShell veya Azure CLI'yı kullanın:

    Değiştirin `RESOURCEGROUP` adıyla kaynak grubunun sanal ağı içeren ve ardından komutu girin:

    ```powershell
    $NICs = Get-AzNetworkInterface -ResourceGroupName "RESOURCEGROUP"
    $NICs[0].DnsSettings.InternalDomainNameSuffix
    ```

    ```azurecli
    az network nic list --resource-group RESOURCEGROUP --query "[0].dnsSettings.internalDomainNameSuffix"
    ```

2. Aşağıdaki metni içeriğini kullanın `/etc/bind/named.config.local` özel DNS sunucusundaki dosya. Her iki sanal ağ özel DNS sunucusunda bu değişiklik yapın.

    ```
    // Forward requests for the virtual network suffix to Azure recursive resolver
    zone "0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net" {
        type forward;
        forwarders {10.0.0.4;}; # The IP address of the DNS server in the other virtual network
    };
    ```

    Değiştirin `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` DNS soneki değeriyle __diğer__ sanal ağ. Bu giriş, ağda özel DNS istekleri uzak ağ DNS soneki için yönlendirir.

3. Her iki sanal ağ içindeki özel DNS sunucularında aşağıdaki metni içeriğini kullanın. `/etc/bind/named.conf.options` dosyası:

    ```
    // Clients to accept requests from
    acl goodclients {
        10.1.0.0/16; # The IP address range of one virtual network
        10.0.0.0/16; # The IP address range of the other virtual network
        localhost;
        localnets;
    };

    options {
            directory "/var/cache/bind";

            recursion yes;

            allow-query { goodclients; };

            forwarders {
            168.63.129.16;   # Azure recursive resolver         
            };

            dnssec-validation auto;

            auth-nxdomain no;    # conform to RFC1035
            listen-on { any; };
    };
    ```
    
   Değiştirin `10.0.0.0/16` ve `10.1.0.0/16` değerleri ile IP adresi aralıkları, sanal ağların. Bu giriş, DNS sunucularının isteğinde bulunmak için her bir ağ kaynakları sağlar.

    Tüm istekler için DNS son eklerini sanal ağlar (örneğin, microsoft.com) olmayan Azure özyinelemeli çözümleyici tarafından gerçekleştirilir.

4. Yapılandırmayı kullanmak için bağlama yeniden başlatın. Örneğin, `sudo service bind9 restart` hem DNS sunucularında.

Bu adımları tamamladıktan sonra tam etki alanı adlarını (FQDN) bir sanal ağdaki kaynaklara bağlanabilir. HDInsight, artık sanal ağa yükleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Bir şirket içi ağa bağlanmak için HDInsight yapılandırma uçtan uca örneği için bkz: [bir şirket içi ağa bağlanma HDInsight](./connect-on-premises-network.md).
* Apache HBase kümeleri Azure sanal ağları yapılandırmak için bkz: [Apache HBase kümeleri oluşturma Azure sanal ağdaki HDInsight üzerinde](hbase/apache-hbase-provision-vnet.md).
* Apache HBase coğrafi çoğaltmayı yapılandırmak için bkz: [Azure sanal ağlarda bulunan Apache HBase kümesi çoğaltma ayarlama](hbase/apache-hbase-replication.md).
* Azure sanal ağları hakkında daha fazla bilgi için bkz. [Azure sanal ağına genel bakış](../virtual-network/virtual-networks-overview.md).

* Ağ güvenlik grupları hakkında daha fazla bilgi için bkz. [ağ güvenlik grupları](../virtual-network/security-overview.md).

* Kullanıcı tanımlı yollar hakkında daha fazla bilgi için bkz. [kullanıcı tanımlı yollar ve IP iletme](../virtual-network/virtual-networks-udr-overview.md).
