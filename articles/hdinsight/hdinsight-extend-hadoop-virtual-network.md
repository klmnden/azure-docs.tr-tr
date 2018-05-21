---
title: Hdınsight sanal ağ - Azure ile genişletmek | Microsoft Docs
description: Hdınsight diğer bulut kaynaklarına veya veri merkezinizdeki kaynaklarına bağlanmak için Azure sanal ağı kullanmayı öğrenin
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
ms.assetid: 37b9b600-d7f8-4cb1-a04a-0b3a827c6dcc
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/21/2018
ms.author: larryfr
ms.openlocfilehash: 3df32c39152c8dda24fd5d0796f8074af8ce8a1a
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="extend-azure-hdinsight-using-an-azure-virtual-network"></a>Bir Azure sanal ağı kullanarak Azure Hdınsight genişletme

Hdınsight ile kullanmayı öğrenin bir [Azure Virtual Network](../virtual-network/virtual-networks-overview.md). Bir Azure sanal ağı kullanarak aşağıdaki senaryolar sağlar:

* Hdınsight için doğrudan bir şirket içi ağ üzerinden bağlanılıyor.

* Hdınsight verilere bağlanma bir Azure sanal ağında depolar.

* Doğrudan genel olarak internet üzerinden kullanılabilir değil Hadoop hizmetlerine erişme. Örneğin, Kafka API'leri veya HBase Java API.

> [!WARNING]
> Bu belgedeki bilgiler TCP/IP ağı bilinmesini gerektirir. TCP/IP ağ ile bilmiyorsanız, üretim ağları değişiklikler yapmadan önce olan bir kullanıcıyla ortak.

> [!IMPORTANT]
> Bağlama hakkında adım adım kılavuz arıyorsanız, şirket içi Hdınsight'a bir Azure sanal ağı kullanarak ağa, bkz: [şirket içi ağınıza bağlanmak Hdınsight](connect-on-premises-network.md) belge.

## <a name="planning"></a>Planlama

Sanal bir ağa Hdınsight yüklemek planlama yaparken yanıt sorular şunlardır:

* Mevcut bir sanal ağı Hdınsight yükleme gerekiyor mu? Veya yeni bir ağ oluşturmak?

    Varolan bir sanal ağı kullanıyorsanız, Hdınsight yükleyebilmek için önce ağ yapılandırmasını değiştirmeniz gerekebilir. Daha fazla bilgi için bkz: [Hdınsight mevcut bir sanal ağa eklemek](#existingvnet) bölümü.

* Başka bir sanal ağ veya şirket içi ağınız için Hdınsight içeren sanal ağa bağlanmak istiyor musunuz?

    Kolayca ağlarda kaynakları ile çalışmak için bir özel DNS oluşturma ve DNS iletmeyi yapılandırma gerekebilir. Daha fazla bilgi için bkz: [birden çok ağları bağlama](#multinet) bölümü.

* Hdınsight gelen veya giden trafiği kısıtlama/yeniden yönlendirme etmek istiyor musunuz?

    Hdınsight Azure veri merkezindeki belirli IP adresleri ile iletişim sınırsız gerekir. Güvenlik duvarları üzerinden istemci iletişimi için izin verilmelidir çeşitli bağlantı noktaları da vardır. Daha fazla bilgi için bkz: [ağ trafiğini denetleme](#networktraffic) bölümü.

## <a id="existingvnet"></a>Mevcut bir sanal ağa Hdınsight Ekle

Yeni Hdınsight var olan bir Azure sanal ağı eklemek nasıl keşfetmek için bu bölümdeki adımları kullanın.

> [!NOTE]
> Bir sanal ağ var olan bir Hdınsight kümesine eklenemiyor.

1. Sanal ağ için bir Klasik veya Resource Manager dağıtım modeli kullanıyor musunuz?

    Hdınsight 3.4 ve büyük bir Resource Manager sanal ağ gerektirir. Hdınsight'ın önceki sürümlerini Klasik sanal ağ gereklidir.

    Ardından, varolan ağınızda bir Klasik sanal ağı ise, Resource Manager sanal ağ oluşturma ve iki bağlantı gerekir. [Klasik sanal ağlar için yeni sanal ağlara bağlanma](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

    Birleştirilmiş sonra kaynak yöneticisi ağ yüklü Hdınsight Klasik ağ kaynakları ile etkileşim kurabilir.

2. Zorlamalı tünel kullanıyor musunuz? Zorlamalı tünel, giden Internet trafiğini incelemesi için bir cihaz için zorlar bir alt ağ ayarı ve günlüğe kaydetme olur. Hdınsight zorlamalı tünel desteklemez. Bir alt ağ Hdınsight yüklemeden önce zorlamalı tünel kaldırmak veya Hdınsight için yeni bir alt ağ oluşturun.

3. İçine veya dışına sanal ağ trafiği kısıtlamak için ağ güvenlik grupları, kullanıcı tanımlı yollar ya da sanal ağ cihazları kullanıyorsunuz?

    Yönetilen bir hizmet olarak Hdınsight Azure veri merkezinde birden fazla IP adresi Kısıtlanmamış erişim gerektirir. Bu IP adresleri ile iletişime izin verecek şekilde herhangi bir mevcut ağ güvenlik grupları veya kullanıcı tanımlı yollar güncelleştirin.

    Hdınsight çeşitli bağlantı noktaları kullanan birden çok hizmetleri barındırır. Bu bağlantı noktaları için trafiği engellemez. Sanal gereç güvenlik duvarlarından izin vermek için bağlantı noktalarının listesi için bkz [güvenlik](#security) bölümü.

    Var olan güvenlik yapılandırmanızı bulmak için aşağıdaki Azure PowerShell veya Azure CLI komutları kullanın:

    * Ağ güvenlik grupları

        ```powershell
        $resourceGroupName = Read-Input -Prompt "Enter the resource group that contains the virtual network used with HDInsight"
        get-azurermnetworksecuritygroup -resourcegroupname $resourceGroupName
        ```

        ```azurecli-interactive
        read -p "Enter the name of the resource group that contains the virtual network: " RESOURCEGROUP
        az network nsg list --resource-group $RESOURCEGROUP
        ```

        Daha fazla bilgi için bkz: [ağ güvenlik grupları sorun giderme](../virtual-network/virtual-network-nsg-troubleshoot-portal.md) belge.

        > [!IMPORTANT]
        > Ağ güvenlik grubu kural kuralı önceliği temelinde sırayla uygulanır. Trafik desenle eşleşen ilk kural uygulanır ve hiçbir diğerlerinin bu trafiği için uygulanır. Sipariş kurallardan en fazla izne sahip az izin veren için. Daha fazla bilgi için bkz: [filtre ağ güvenlik grupları ile ağ trafiği](../virtual-network/security-overview.md) belge.

    * Kullanıcı tanımlı yollar

        ```powershell
        $resourceGroupName = Read-Input -Prompt "Enter the resource group that contains the virtual network used with HDInsight"
        get-azurermroutetable -resourcegroupname $resourceGroupName
        ```

        ```azurecli-interactive
        read -p "Enter the name of the resource group that contains the virtual network: " RESOURCEGROUP
        az network route-table list --resource-group $RESOURCEGROUP
        ```

        Daha fazla bilgi için bkz: [sorun giderme yolları](../virtual-network/virtual-network-routes-troubleshoot-portal.md) belge.

4. Hdınsight kümesi oluşturma ve yapılandırma sırasında Azure sanal ağı seçin. Küme oluşturma işlemi anlamak için aşağıdaki belgelerde adımları kullanın:

    * [Azure portalını kullanarak HDInsight oluşturma](hdinsight-hadoop-create-linux-clusters-portal.md)
    * [Azure PowerShell kullanarak HDInsight oluşturma](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
    * [Azure CLI 1.0 kullanarak Hdınsight oluşturma](hdinsight-hadoop-create-linux-clusters-azure-cli.md)
    * [Bir Azure Resource Manager şablonu kullanarak Hdınsight oluşturma](hdinsight-hadoop-create-linux-clusters-arm-templates.md)

  > [!IMPORTANT]
  > Bir sanal ağa Hdınsight ekleyerek bir isteğe bağlı yapılandırma adımdır. Küme yapılandırırken sanal ağı seçtiğinizden emin olun.

## <a id="multinet"></a>Birden çok ağları bağlama

Büyük çok ağ yapılandırması ile ağlar arasında ad çözümleme iştir.

Azure ad çözümlemesi için sanal bir ağa yüklü olan Azure hizmetleri sağlar. Bu yerleşik ad çözümlemesi bir tam etki alanı adı (FQDN) kullanarak aşağıdaki kaynaklara bağlanmak Hdınsight sağlar:

* İnternet'te kullanılabilir herhangi bir kaynağa. Örneğin, microsoft.com, google.com.

* Kullanarak aynı Azure sanal ağında, olan herhangi bir kaynağa __iç DNS ad__ kaynağının. Örneğin, varsayılan ad çözümlemesi kullanırken, Hdınsight çalışan düğümlerine atanan örnek iç DNS adları şunlardır:

    * wn0-hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net
    * wn2-hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net

    Bu her iki düğüm iç DNS adları kullanarak birbirine ve diğer düğümlere, hdınsight'ta ile doğrudan iletişim kurabilir.

Varsayılan ad çözümlemesi mu __değil__ ağlarda, sanal ağa katılan kaynakların adları çözümlemek Hdınsight izin verin. Örneğin, sanal ağa şirket içi ağınıza katılmak için yaygındır. Yalnızca varsayılan ad çözümlemesi ile Hdınsight, ada göre şirket içi ağ kaynaklarına erişemez. Şirket içi ağınızdaki kaynaklara ada göre sanal ağ kaynaklarına erişemez, bunun tersi de geçerlidir.

> [!WARNING]
> Özel DNS sunucusu oluşturmak ve Hdınsight küme oluşturmadan önce kullanılacak sanal ağ yapılandırmanız gerekir.

Sanal ağ ve birleştirilmiş ağlarda bulunan kaynaklar arasındaki ad çözümlemesi etkinleştirmek için aşağıdaki eylemleri gerçekleştirmeniz gerekir:

1. Özel bir DNS sunucusu, Azure sanal Hdınsight yüklemeyi planladığınız Ağ oluşturun.

2. Sanal ağ özel DNS sunucusunu kullanacak şekilde yapılandırın.

3. Azure sanal ağınızın DNS soneki atanan bulun. Bu değer benzer `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net`. DNS son eki bulma hakkında daha fazla bilgi için bkz: [örnek: özel DNS](#example-dns) bölümü.

4. DNS sunucuları arasında iletme yapılandırın. Yapılandırma uzak ağ türüne bağlıdır.

    * Uzak ağ bir şirket içi ağ ise, DNS aşağıdaki gibi yapılandırın:
        
        * __Özel DNS__ (sanal ağındaki):

            * Sanal ağın Azure özyinelemeli çözümleyici (168.63.129.16) için DNS soneki için istekleri ilet. Azure sanal ağındaki kaynaklara yönelik isteklerin işleme

            * Şirket içi DNS sunucusuna tüm diğer isteklerden iletin. Şirket içi DNS bile istekleri Internet kaynakların Microsoft.com gibi diğer tüm ad çözümleme isteklerini işler.

        * __Şirket içi DNS__: iletmek sanal ağ DNS soneki özel DNS sunucusu için istek. Özel DNS sunucusu, daha sonra Azure özyinelemeli çözümleyici iletir.

        Bu yapılandırma yolları istekleri için sanal ağ özel DNS sunucusuna DNS sonekini içeren etki alanı adları tam. Tüm diğer isteklerden (hatta genel internet adresleri için) şirket içi DNS sunucusu tarafından işlenir.

    * Uzak ağ başka bir Azure sanal ağ ise, DNS aşağıdaki gibi yapılandırın:

        * __Özel DNS__ (her sanal ağındaki):

            * DNS soneki sanal ağlar için istekleri özel DNS sunucularına iletilir. Her sanal ağındaki DNS, alt ağ içindeki kaynakların çözümlemek için sorumludur.

            * Tüm diğer isteklerden Azure özyinelemeli çözümleyici iletin. Özyinelemeli çözümleyici yerel çözme ve Internet kaynakların sorumludur.

        Her ağ diğerini isteklerini iletir için DNS sunucusu DNS son ekini temel alarak. Diğer istekleri Azure özyinelemeli çözümleyici kullanarak çözümlenir.

    Her yapılandırma örneği için bkz: [örnek: özel DNS](#example-dns) bölümü.

Daha fazla bilgi için bkz: [VM'ler ve rol örnekleri için ad çözümlemesi](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) belge.

## <a name="directly-connect-to-hadoop-services"></a>Hadoop Services'e doğrudan bağlanmak

Hdınsight üzerinde çoğu belgeleri Internet üzerinden kümesine erişimi olduğunu varsayar. Örneğin, https://CLUSTERNAME.azurehdinsight.net konumundaki kümeye bağlanabildiğiniz kabul edilir. Bu adres Nsg'ler veya Udr'ler kullandıysanız internet'ten erişimi kısıtlamak için kullanılabilir olmayan ortak ağ geçidi, kullanır.

Ambari ve sanal ağ üzerinden diğer web sayfalarına bağlanmak için aşağıdaki adımları kullanın:

1. Hdınsight küme düğümlerinin iç tam etki alanı adları (FQDN) bulmak için aşağıdaki yöntemlerden birini kullanın:

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

    Verilen düğüm listesi, baş düğümler için FQDN bulun ve Ambari ve diğer web hizmetlerine bağlanmak için FQDN'leri kullanın. Örneğin, `http://<headnode-fqdn>:8080` Ambari erişmek için.

    > [!IMPORTANT]
    > Baş düğümler üzerinde barındırılan bazı hizmetler aynı anda yalnızca bir düğümde etkin olur. Bir hizmet bir baş düğüm üzerindeki erişmeyi deneyin ve bir 404 hatası döndürür, diğer baş düğüme geçiş yapar.

2. Düğüm ve bir hizmet kullanılabilir bağlantı noktasını belirlemek için bkz: [hdınsight'ta Hadoop Hizmetleri tarafından kullanılan bağlantı noktaları](./hdinsight-hadoop-port-settings-for-services.md) belge.

## <a id="networktraffic"></a> Ağ trafiğini denetleme

Bir Azure sanal ağlarda ağ trafiğini aşağıdaki yöntemler kullanılarak denetlenebilir:

* **Ağ güvenlik grupları** (NSG) ağa gelen ve giden trafiği filtrelemek olanak sağlar. Daha fazla bilgi için bkz: [filtre ağ güvenlik grupları ile ağ trafiği](../virtual-network/security-overview.md) belge.

    > [!WARNING]
    > Hdınsight giden trafiği kısıtlama desteklemez.

* **Kullanıcı tanımlı yollar** (UDR) nasıl trafiği ağ kaynakları arasında akan tanımlayın. Daha fazla bilgi için bkz: [kullanıcı tanımlı yollar ve IP iletimini](../virtual-network/virtual-networks-udr-overview.md) belge.

* **Ağ sanal Gereçleri** güvenlik duvarları ve yönlendiriciler gibi cihazların işlevselliğiyle Çoğalt. Daha fazla bilgi için bkz: [ağ uygulamaları](https://azure.microsoft.com/solutions/network-appliances) belge.

Yönetilen bir hizmet olarak Hdınsight Azure bulutta Azure sistem durumu ve Yönetim hizmetlerine Kısıtlanmamış erişim gerektirir. Nsg'ler ve Udr'ler kullanırken Hdınsight hizmetlerin hala Hdınsight ile iletişim kurabildiğinden emin olmalısınız.

Hdınsight, çeşitli bağlantı noktaları üzerinde hizmetleri sunar. Bir sanal gereç Güvenlik Duvarı'nı kullanırken, bu hizmetler için kullanılan bağlantı noktaları üzerinde trafiğe izin vermelidir. Daha fazla bilgi için [gerekli bağlantı noktalarını] bölümüne bakın.

### <a id="hdinsight-ip"></a> Hdınsight ile ağ güvenlik gruplarını ve kullanıcı tanımlı yollar

Kullanmayı planlıyorsanız, **ağ güvenlik grubu** veya **kullanıcı tanımlı yollar** ağ trafiğini denetlemek için Hdınsight'ı yüklemeden önce aşağıdaki eylemleri gerçekleştirin:

1. Hdınsight için kullanmayı planladığınız Azure bölgesi tanımlayın.

2. Hdınsight tarafından gerekli IP adreslerini belirleyin. Daha fazla bilgi için bkz: [Hdınsight tarafından gerekli IP adreslerini](#hdinsight-ip) bölümü.

3. Oluşturun veya ağ güvenlik grupları veya kullanıcı tanımlı yollar Hdınsight'a yüklemeyi planladığınız alt ağ için değiştirin.

    * __Ağ güvenlik grupları__: izin __gelen__ bağlantı noktasında trafik __443__ IP adresleri.
    * __Kullanıcı tanımlı yollar__: her IP adresi için bir yol oluşturun ve ayarlayın __sonraki atlama türü__ için __Internet__.

Ağ güvenlik grupları veya kullanıcı tanımlı yollar hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [Ağ güvenlik grubu](../virtual-network/security-overview.md)

* [Kullanıcı tanımlı yollar](../virtual-network/virtual-networks-udr-overview.md)

#### <a name="forced-tunneling"></a>Zorlamalı tünel oluşturma

Zorlamalı tünel bir kullanıcı tanımlı yönlendirme burada bir alt ağdaki tüm trafiği belirli ağ veya şirket içi ağınız gibi konuma zorlanır yapılandırmadır. Hdınsight mu __değil__ destek zorlamalı tünel.

## <a id="hdinsight-ip"></a> Gerekli IP adresi

> [!IMPORTANT]
> Azure sistem durumu ve Yönetim Hizmetleri Hdınsight ile iletişim kurabilmesi gerekir. Ağ güvenlik grupları veya kullanıcı tanımlı yollar kullanıyorsanız, Hdınsight ulaşmak bu hizmetler için IP adreslerinden gelen trafiğe izin verecek.
>
> Trafiği denetlemek için ağ güvenlik grupları veya kullanıcı tanımlı yollar kullanmıyorsanız, bu bölüm göz ardı edebilirsiniz.

Ağ güvenlik grupları veya kullanıcı tanımlı yollar kullanıyorsanız, Hdınsight erişmek için Azure sistem durumu ve Yönetim hizmetlerinden trafiğe izin vermelidir. İzin verilmiş olmalıdır IP adresleri bulmak için aşağıdaki adımları kullanın:

1. Her zaman aşağıdaki IP adreslerinden gelen trafiğe izin vermeniz gerekir:

    | IP adresi | İzin verilen bağlantı noktası | Yön |
    | ---- | ----- | ----- |
    | 168.61.49.99 | 443 | Gelen |
    | 23.99.5.239 | 443 | Gelen |
    | 168.61.48.131 | 443 | Gelen |
    | 138.91.141.162 | 443 | Gelen |

2. Hdınsight kümenizi aşağıdaki bölgeler birinde ise, bölge için listelenen IP adreslerinden gelen trafiğe izin vermelidir:

    > [!IMPORTANT]
    > Kullanmakta olduğunuz Azure bölgesi listede yoksa, yalnızca 1. adım dört IP adreslerinden kullanın.

    | Ülke | Bölge | İzin verilen IP adresi | İzin verilen bağlantı noktası | Yön |
    | ---- | ---- | ---- | ---- | ----- |
    | Asya | Doğu Asya | 23.102.235.122</br>52.175.38.134 | 443 | Gelen |
    | &nbsp; | Güneydoğu Asya | 13.76.245.160</br>13.76.136.249 | 443 | Gelen |
    | Avustralya | Avustralya Doğu | 104.210.84.115</br>13.75.152.195 | 443 | Gelen |
    | &nbsp; | Avustralya Güneydoğu | 13.77.2.56</br>13.77.2.94 | 443 | Gelen |
    | Brezilya | Güney Brezilya | 191.235.84.104</br>191.235.87.113 | 443 | Gelen |
    | Kanada | Doğu Kanada | 52.229.127.96</br>52.229.123.172 | 443 | Gelen |
    | &nbsp; | Orta Kanada | 52.228.37.66</br>52.228.45.222 | 443 | Gelen |
    | Çin | Çin Kuzey | 42.159.96.170</br>139.217.2.219 | 443 | Gelen |
    | &nbsp; | Çin Doğu | 42.159.198.178</br>42.159.234.157 | 443 | Gelen |
    | Avrupa | Kuzey Avrupa | 52.164.210.96</br>13.74.153.132 | 443 | Gelen |
    | &nbsp; | Batı Avrupa| 52.166.243.90</br>52.174.36.244 | 443 | Gelen |
    | Almanya | Almanya Orta | 51.4.146.68</br>51.4.146.80 | 443 | Gelen |
    | &nbsp; | Almanya Kuzeydoğu | 51.5.150.132</br>51.5.144.101 | 443 | Gelen |
    | Hindistan | Orta Hindistan | 52.172.153.209</br>52.172.152.49 | 443 | Gelen |
    | Japonya | Japonya Doğu | 13.78.125.90</br>13.78.89.60 | 443 | Gelen |
    | &nbsp; | Japonya Batı | 40.74.125.69</br>138.91.29.150 | 443 | Gelen |
    | Kore | Kore Orta | 52.231.39.142</br>52.231.36.209 | 433 | Gelen |
    | &nbsp; | Kore Güney | 52.231.203.16</br>52.231.205.214 | 443 | Gelen
    | Birleşik Krallık | Birleşik Krallık Batı | 51.141.13.110</br>51.141.7.20 | 443 | Gelen |
    | &nbsp; | Birleşik Krallık Güney | 51.140.47.39</br>51.140.52.16 | 443 | Gelen |
    | Amerika Birleşik Devletleri | Orta ABD | 13.67.223.215</br>40.86.83.253 | 443 | Gelen |
    | &nbsp; | Doğu ABD | 13.82.225.233</br>40.71.175.99 | 443 | Gelen |
    | &nbsp; | Orta Kuzey ABD | 157.56.8.38</br>157.55.213.99 | 443 | Gelen |
    | &nbsp; | Batı Orta ABD | 52.161.23.15</br>52.161.10.167 | 443 | Gelen |
    | &nbsp; | Batı ABD | 13.64.254.98</br>23.101.196.19 | 443 | Gelen |
    | &nbsp; | Batı ABD 2 | 52.175.211.210</br>52.175.222.222 | 443 | Gelen |

    Azure kamu için kullanılacak IP adresleri hakkında daha fazla bilgi için bkz: [Azure Kamu Intelligence + analiz](https://docs.microsoft.com/azure/azure-government/documentation-government-services-intelligenceandanalytics) belge.

3. Sanal ağınız ile özel bir DNS sunucusu kullanıyorsanız, erişimden de izin vermeniz gerekir __168.63.129.16__. Azure'nın özyinelemeli çözümleyici adresidir. Daha fazla bilgi için bkz: [VM'ler ve rol için ad çözümlemesi örnekleri](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) belge.

Daha fazla bilgi için bkz: [ağ trafiğini denetleme](#networktraffic) bölümü.

## <a id="hdinsight-ports"></a> Gerekli bağlantı noktaları

Bir ağı kullanmayı planlıyorsanız, **sanal gereç Güvenlik Duvarı** sanal ağ güvenliğini sağlamak için aşağıdaki bağlantı noktaları üzerinde giden trafiğe izin vermesi gerekir:

* 53
* 443
* 1433
* 11000-11999
* 14000-14999

Belirli hizmetleri için bağlantı noktalarını bir listesi için bkz: [hdınsight'ta Hadoop Hizmetleri tarafından kullanılan bağlantı noktaları](hdinsight-hadoop-port-settings-for-services.md) belge.

Sanal gereçler için güvenlik duvarı kuralları hakkında daha fazla bilgi için bkz: [sanal gereç senaryo](../virtual-network/virtual-network-scenario-udr-gw-nva.md) belge.

## <a id="hdinsight-nsg"></a>Örnek: ağ güvenlik grupları Hdınsight ile

Bu bölümdeki örnekleri ağ güvenliği Hdınsight Azure Yönetim Hizmetleri ile iletişim kurmasına izin ver Grup kuralları oluşturmak nasıl ekleyebileceğiniz gösterilmektedir. Örnekler kullanmadan önce kullanmakta olduğunuz Azure bölgesinin olanlarla eşleşmesi için IP adreslerini ayarlayın. Bu bilgiler bulabilirsiniz [Hdınsight ağ güvenlik gruplarını ve kullanıcı tanımlı yollar ile](#hdinsight-ip) bölümü.

### <a name="azure-resource-management-template"></a>Azure kaynak yönetimi şablonu

Aşağıdaki kaynak yönetimi şablon gelen trafiği sınırlar, ancak Hdınsight tarafından gerekli IP adreslerinden gelen trafiğe izin veren bir sanal ağ oluşturur. Bu şablon, ayrıca bir Hdınsight kümesi sanal ağ oluşturur.

* [Güvenli bir Azure sanal ağ ve bir Hdınsight Hadoop kümesi dağıtma](https://azure.microsoft.com/resources/templates/101-hdinsight-secure-vnet/)

> [!IMPORTANT]
> Bu örnekte, kullanmakta olduğunuz Azure bölgesi eşleştirmek için kullanılan IP adreslerini değiştirin. Bu bilgiler bulabilirsiniz [Hdınsight ağ güvenlik gruplarını ve kullanıcı tanımlı yollar ile](#hdinsight-ip) bölümü.

### <a name="azure-powershell"></a>Azure PowerShell

Gelen trafik kısıtlar ve Kuzey Avrupa bölgesinin IP adreslerinden gelen trafiğe izin veren bir sanal ağ oluşturmak için aşağıdaki PowerShell betiğini kullanın.

> [!IMPORTANT]
> Bu örnekte, kullanmakta olduğunuz Azure bölgesi eşleştirmek için kullanılan IP adreslerini değiştirin. Bu bilgiler bulabilirsiniz [Hdınsight ağ güvenlik gruplarını ve kullanıcı tanımlı yollar ile](#hdinsight-ip) bölümü.

```powershell
$vnetName = "Replace with your virtual network name"
$resourceGroupName = "Replace with the resource group the virtual network is in"
$subnetName = "Replace with the name of the subnet that you plan to use for HDInsight"
# Get the Virtual Network object
$vnet = Get-AzureRmVirtualNetwork `
    -Name $vnetName `
    -ResourceGroupName $resourceGroupName
# Get the region the Virtual network is in.
$location = $vnet.Location
# Get the subnet object
$subnet = $vnet.Subnets | Where-Object Name -eq $subnetName
# Create a Network Security Group.
# And add exemptions for the HDInsight health and management services.
$nsg = New-AzureRmNetworkSecurityGroup `
    -Name "hdisecure" `
    -ResourceGroupName $resourceGroupName `
    -Location $location `
    | Add-AzureRmNetworkSecurityRuleConfig `
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
    | Add-AzureRmNetworkSecurityRuleConfig `
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
    | Add-AzureRmNetworkSecurityRuleConfig `
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
    | Add-AzureRmNetworkSecurityRuleConfig `
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
    | Add-AzureRmNetworkSecurityRuleConfig `
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
    | Add-AzureRmNetworkSecurityRuleConfig `
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
Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
# Apply the NSG to the subnet
Set-AzureRmVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name $subnetName `
    -AddressPrefix $subnet.AddressPrefix `
    -NetworkSecurityGroup $nsg
$vnet | Set-AzureRmVirtualNetwork
```

> [!IMPORTANT]
> Bu örnek gerekli IP adreslerini gelen trafiğe izin verme kuralları ekleneceği gösterilmektedir. Diğer kaynaklardan gelen erişimi kısıtlamak için bir kural içermiyor.
>
> Aşağıdaki örnek, Internet'ten SSH erişimini etkinleştirmek gösterilmiştir:
>
> ```powershell
> Add-AzureRmNetworkSecurityRuleConfig -Name "SSH" -Description "SSH" -Protocol "*" -SourcePortRange "*" -DestinationPortRange "22" -SourceAddressPrefix "*" -DestinationAddressPrefix "VirtualNetwork" -Access Allow -Priority 306 -Direction Inbound
> ```

### <a name="azure-cli"></a>Azure CLI

Gelen trafik sınırlar, ancak Hdınsight tarafından gerekli IP adreslerinden gelen trafiğe izin veren bir sanal ağ oluşturmak için aşağıdaki adımları kullanın.

1. Adlı yeni bir ağ güvenlik grubu oluşturmak için aşağıdaki komutu kullanın `hdisecure`. Değiştir **RESOURCEGROUPNAME** Azure sanal ağı içeren kaynak grubunu ile. Değiştir **konumu** grubunun oluşturulduğu konum (bölge).

    ```azurecli
    az network nsg create -g RESOURCEGROUPNAME -n hdisecure -l LOCATION
    ```

    Grup oluşturulduktan sonra yeni grubu hakkında bilgi alabilir.

2. Bağlantı noktası 443 üzerinden Azure Hdınsight sistem durumu ve yönetim hizmetinden gelen iletişime izin verecek yeni bir ağ güvenlik grubu kural eklemek için aşağıdakileri kullanın. Değiştir **RESOURCEGROUPNAME** Azure sanal ağı içeren kaynak grubu adı.

    > [!IMPORTANT]
    > Bu örnekte, kullanmakta olduğunuz Azure bölgesi eşleştirmek için kullanılan IP adreslerini değiştirin. Bu bilgiler bulabilirsiniz [Hdınsight ağ güvenlik gruplarını ve kullanıcı tanımlı yollar ile](#hdinsight-ip) bölümü.

    ```azurecli
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule1 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "52.164.210.96" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 300 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "13.74.153.132" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 301 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "168.61.49.99" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 302 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "23.99.5.239" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 303 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "168.61.48.131" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 304 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "138.91.141.162" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 305 --direction "Inbound"
    ```

3. Bu ağ güvenlik grubu için benzersiz tanımlayıcı almak için aşağıdaki komutu kullanın:

    ```azurecli
    az network nsg show -g RESOURCEGROUPNAME -n hdisecure --query 'id'
    ```

    Bu komutu aşağıdaki metni benzer bir değer döndürür:

        "/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUPNAME/providers/Microsoft.Network/networkSecurityGroups/hdisecure"

    Beklenen sonuç alamazsanız kimliği etrafında çift tırnak komutunu kullanın.

4. Bir alt ağ için ağ güvenlik grubu uygulamak için aşağıdaki komutu kullanın. Değiştir __GUID__ ve __RESOURCEGROUPNAME__ olanları değerlerle önceki adımda döndürülen. Değiştir __vnetname ADLI__ ve __SUBNETNAME__ oluşturmak istediğiniz alt ağ adı ve sanal ağ adı.

    ```azurecli
    az network vnet subnet update -g RESOURCEGROUPNAME --vnet-name VNETNAME --name SUBNETNAME --set networkSecurityGroup.id="/subscriptions/GUID/resourceGroups/RESOURCEGROUPNAME/providers/Microsoft.Network/networkSecurityGroups/hdisecure"
    ```

    Bu komut tamamlandıktan sonra sanal ağa Hdınsight yükleyebilirsiniz.

> [!IMPORTANT]
> Bu adımları yalnızca Azure Bulutu üzerinde Hdınsight sistem durumu ve yönetim hizmetine erişim açın. Hdınsight küme sanal ağ dışındaki diğer tüm erişimi engellenir. Sanal Ağ dışından erişim etkinleştirmek için ek ağ güvenlik grubu kuralları eklemeniz gerekir.
>
> Aşağıdaki örnek, Internet'ten SSH erişimini etkinleştirmek gösterilmiştir:
>
> ```azurecli
> az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule5 --protocol "*" --source-port-range "*" --destination-port-range "22" --source-address-prefix "*" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 306 --direction "Inbound"
> ```

## <a id="example-dns"></a> Örnek: DNS yapılandırması

### <a name="name-resolution-between-a-virtual-network-and-a-connected-on-premises-network"></a>Sanal bir ağa bağlı şirket içi ağ arasındaki ad çözümlemesi

Bu örnekte aşağıdaki varsayımlar yapar:

* Bir Azure sanal bir VPN ağ geçidi kullanarak bir şirket ağına bağlı ağ var.

* Özel sanal ağın DNS sunucusu, Linux veya UNIX işletim sistemi olarak çalışıyor.

* [Bağlama](https://www.isc.org/downloads/bind/) özel DNS sunucusuna yüklenir.

Özel DNS sunucusunda sanal ağda:

1. Sanal ağın DNS soneki bulmak için Azure PowerShell veya Azure CLI kullanın:

    ```powershell
    $resourceGroupName = Read-Input -Prompt "Enter the resource group that contains the virtual network used with HDInsight"
    $NICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName
    $NICs[0].DnsSettings.InternalDomainNameSuffix
    ```

    ```azurecli-interactive
    read -p "Enter the name of the resource group that contains the virtual network: " RESOURCEGROUP
    az network nic list --resource-group $RESOURCEGROUP --query "[0].dnsSettings.internalDomainNameSuffix"
    ```

2. Sanal ağ için özel DNS sunucusunda aşağıdaki metni içeriğini kullanmak `/etc/bind/named.conf.local` dosyası:

    ```
    // Forward requests for the virtual network suffix to Azure recursive resolver
    zone "0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net" {
        type forward;
        forwarders {168.63.129.16;}; # Azure recursive resolver
    };
    ```

    Değiştir `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` sanal ağınızın DNS soneki ile değer.

    Bu yapılandırma sanal ağın DNS soneki için tüm DNS isteklerine Azure özyinelemeli çözümleyici yönlendirir.

2. Sanal ağ için özel DNS sunucusunda aşağıdaki metni içeriğini kullanmak `/etc/bind/named.conf.options` dosyası:

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
    
    * Değiştir `10.0.0.0/16` sanal ağınızdaki IP adres aralığı ile değer. Bu giriş, ad çözümleme istekleri adreslerini bu aralıkta sağlar.

    * Şirket içi ağ için IP adres aralığı Ekle `acl goodclients { ... }` bölümü.  Giriş ad çözümleme isteklerinin kaynaklardan şirket içi ağ sağlar.
    
    * Değeri değiştirme `192.168.0.1` şirket içi DNS sunucunuzun IP adresine sahip. Bu giriş diğer tüm DNS isteklerine şirket DNS sunucusuna yönlendirir.

3. Yapılandırmayı kullanmak için bağlama yeniden başlatın. Örneğin, `sudo service bind9 restart`.

4. Koşullu ileticisi şirket DNS sunucusuna ekleyin. Adım 1'den özel DNS sunucusuna DNS soneki için istekleri göndermesine koşullu ileticisi yapılandırın.

    > [!NOTE]
    > Koşullu ileticisi ekleme özellikleri için DNS yazılımınızın belgelerine bakın.

Bu adımları tamamladıktan sonra tam etki alanı adları (FQDN) kullanarak ya da ağdaki kaynaklara bağlanabilir. Bu gibi durumlarda, Hdınsight artık sanal ağınıza yükleyebilirsiniz.

### <a name="name-resolution-between-two-connected-virtual-networks"></a>İki bağlı sanal ağlar arasındaki ad çözümlemesi

Bu örnekte aşağıdaki varsayımlar yapar:

* İki Azure sanal veya eşliği ya da, bir VPN ağ geçidi kullanarak bağlanan ağ var.

* Her iki ağlarda özel DNS sunucusu, Linux veya UNIX işletim sistemi olarak çalışıyor.

* [Bağlama](https://www.isc.org/downloads/bind/) özel DNS sunucularında yüklü.

1. Her iki sanal ağ DNS sonekini bulmak için Azure PowerShell veya Azure CLI kullanın:

    ```powershell
    $resourceGroupName = Read-Input -Prompt "Enter the resource group that contains the virtual network used with HDInsight"
    $NICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName
    $NICs[0].DnsSettings.InternalDomainNameSuffix
    ```

    ```azurecli-interactive
    read -p "Enter the name of the resource group that contains the virtual network: " RESOURCEGROUP
    az network nic list --resource-group $RESOURCEGROUP --query "[0].dnsSettings.internalDomainNameSuffix"
    ```

2. Aşağıdaki metni içeriğini kullanmak `/etc/bind/named.config.local` özel DNS sunucusunda dosya. Özel DNS sunucusunda hem de sanal ağlarda bu değişikliği yapın.

    ```
    // Forward requests for the virtual network suffix to Azure recursive resolver
    zone "0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net" {
        type forward;
        forwarders {10.0.0.4;}; # The IP address of the DNS server in the other virtual network
    };
    ```

    Değiştir `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` DNS soneki değeri __diğer__ sanal ağ. Bu giriş özel DNS, ağ istekleri uzak ağın DNS soneki için yönlendirir.

3. Her iki sanal ağlarda özel DNS sunucularında aşağıdaki metni içeriğini kullanın `/etc/bind/named.conf.options` dosyası:

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
    
    * Değiştir `10.0.0.0/16` ve `10.1.0.0/16` değerleri ile IP adresi aralıkları, sanal ağların. Bu girdi kaynakları her ağın DNS sunucularını istekler yapmasını sağlar.

    Sanal ağlar (örneğin, microsoft.com) DNS sonekleri için değil tüm istekleri Azure özyinelemeli çözümleyici tarafından işlenir.

4. Yapılandırmayı kullanmak için bağlama yeniden başlatın. Örneğin, `sudo service bind9 restart` hem DNS sunucularında.

Bu adımları tamamladıktan sonra tam etki alanı adları (FQDN) kullanarak sanal ağınızdaki kaynaklara bağlanabilir. Bu gibi durumlarda, Hdınsight artık sanal ağınıza yükleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Bir şirket ağına bağlanmak için Hdınsight yapılandırma uçtan uca örneği için bkz: [bir şirket içi ağınıza bağlanmak Hdınsight](./connect-on-premises-network.md).
* Hbase kümeleri Azure sanal ağları yapılandırmak için bkz: [oluşturma HBase kümeleri Azure sanal ağındaki hdınsight'ta](hbase/apache-hbase-provision-vnet.md).
* HBase coğrafi çoğaltma yapılandırmak için bkz: [HBase kümesi çoğaltma Azure sanal ağlarda ayarlama](hbase/apache-hbase-replication.md).
* Azure sanal ağlar hakkında daha fazla bilgi için bkz: [Azure Virtual Network'e genel bakış](../virtual-network/virtual-networks-overview.md).

* Ağ güvenlik grupları hakkında daha fazla bilgi için bkz: [ağ güvenlik grupları](../virtual-network/security-overview.md).

* Kullanıcı tanımlı yollar hakkında daha fazla bilgi için bkz: [kullanıcı tanımlı yollar ve IP iletimini](../virtual-network/virtual-networks-udr-overview.md).
