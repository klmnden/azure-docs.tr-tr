---
title: Azure sanal makineler'de SQL Server kullanılabilirlik grubu dinleyicisi oluşturma | Microsoft Docs
description: Always On kullanılabilirlik grubu için bir dinleyici, Azure sanal makineler'de SQL Server için oluşturmak için adım adım yönergeler
services: virtual-machines
documentationcenter: na
author: MikeRayMSFT
manager: craigg
editor: monicar
ms.assetid: d1f291e9-9af2-41ba-9d29-9541e3adcfcf
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 02/16/2017
ms.author: mikeray
ms.openlocfilehash: 3b90ae3e9808b22b6d6c41e3ac11bec0293bd4bf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60326155"
---
# <a name="configure-a-load-balancer-for-an-always-on-availability-group-in-azure"></a>Azure'da bir Always On kullanılabilirlik grubu için bir yük dengeleyici yapılandırma
Bu makalede, Azure Resource Manager ile çalışan Azure sanal makineler'de SQL Server Always On kullanılabilirlik grubu için yük dengeleyici oluşturma açıklanmaktadır. Azure sanal makinelerinde SQL Server örnekleri olan bir kullanılabilirlik grubu yük dengeleyici gerektirir. Yük Dengeleyici IP adresi için kullanılabilirlik grubu dinleyicisi depolar. Bir kullanılabilirlik grubu birden çok bölgede kapsıyorsa, her bölge bir yük dengeleyicinin gerekir.

Bu görevi tamamlamak için Resource Manager ile çalışan Azure sanal makinelerinde dağıtılan bir SQL Server kullanılabilirlik grubu olması gerekir. Her iki SQL Server sanal makineleri aynı kullanılabilirlik kümesine ait olmalıdır. Kullanabileceğiniz [Microsoft şablon](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) otomatik olarak Kaynak Yöneticisi'nde kullanılabilirlik grubunu oluşturun. Bu şablon bir iç yük dengeleyici sizin için otomatik olarak oluşturur. 

İsterseniz, aşağıdakileri yapabilirsiniz [el ile bir kullanılabilirlik grubu yapılandırma](virtual-machines-windows-portal-sql-availability-group-tutorial.md).

Bu makalede, kullanılabilirlik gruplarını zaten yapılandırılmış olmasını gerektirir.  

İlgili Konular şunlardır:

* [Azure VM (GUI), Always On kullanılabilirlik grupları yapılandırma](virtual-machines-windows-portal-sql-availability-group-tutorial.md)   
* [Azure Resource Manager ve PowerShell kullanarak VNet-VNet bağlantı yapılandırma](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

Bu makalede walking tarafından oluşturun ve Azure portalında bir yük dengeleyici yapılandırın. İşlem tamamlandıktan sonra küme için kullanılabilirlik grubu dinleyici IP adresi yük dengeleyiciden kullanacak şekilde yapılandırın.

## <a name="create-and-configure-the-load-balancer-in-the-azure-portal"></a>Oluşturma ve Azure portalında Yük Dengeleyiciyi yapılandırma
Bu görev bölümünde aşağıdakileri yapın:

1. Azure portalında, Yük Dengeleyiciyi oluşturun ve IP adresini yapılandırın.
2. Arka uç havuzunu yapılandırın.
3. Araştırması oluşturun. 
4. Yük Dengeleme kuralları ayarlayın.

> [!NOTE]
> SQL Server örneklerini birden çok kaynak grupları ve bölgeler, her adım, iki kez kez her kaynak grubunda gerçekleştirin.
> 
> 

### <a name="step-1-create-the-load-balancer-and-configure-the-ip-address"></a>1. Adım: Yük Dengeleyiciyi oluşturun ve IP adresini yapılandırın
İlk olarak, Yük Dengeleyiciyi oluşturun. 

1. Azure portalında SQL Server sanal makineleri içeren kaynak grubunu açın. 

2. Kaynak grubunda tıklayın **Ekle**.

3. Arama **yük dengeleyici** ve ardından, arama sonuçlarında **Load Balancer**, tarafından yayımlanan **Microsoft**.

4. Üzerinde **yük dengeleyici** dikey penceresinde tıklayın **Oluştur**.

5. İçinde **yük dengeleyici Oluştur** iletişim kutusunda, Yük Dengeleyiciyi şu şekilde yapılandırın:

   | Ayar | Değer |
   | --- | --- |
   | **Ad** |Yük Dengeleyiciyi temsil eden bir metin adı. Örneğin, **sqlLB**. |
   | **Tür** |**İç**: Çoğu uygulamalarının kullanılabilirlik grubuna bağlanmak uygulamaların aynı sanal ağ içinde veren bir iç yük dengeleyici kullanın.  </br> **Dış**: Uygulamaların kullanılabilirlik grubuna ortak bir Internet bağlantısı üzerinden bağlanmasına izin verir. |
   | **Sanal ağ** |SQL Server örnekleri olan sanal ağı seçin. |
   | **Alt ağ** |SQL Server örnekleri olan bir alt ağ seçin. |
   | **IP adresi ataması** |**Statik** |
   | **Özel IP adresi** |Kullanılabilir bir IP adresi alt ağ belirtin. Küme üzerinde bir dinleyici oluşturmak için bu IP adresi kullanabilirsiniz. Bu adres için bu makalenin sonraki bölümlerinde bir PowerShell Betiği kullanmak `$ILBIP` değişkeni. |
   | **Abonelik** |Birden fazla aboneliğiniz varsa, bu alanda görüntülenir. Bu kaynak ile ilişkilendirmek istediğiniz aboneliği seçin. Normalde, kullanılabilirlik grubunun tüm kaynaklarını aynı abonelik olur. |
   | **Kaynak grubu** |SQL Server örnekleri olan kaynak grubunu seçin. |
   | **Konum** |SQL Server örnekleri olan Azure konumu seçin. |

6. **Oluştur**’a tıklayın. 

Azure yük dengeleyici oluşturur. Yük Dengeleyici, belirli ağ, alt ağ, kaynak grubunu ve konumu için aittir. Azure görev tamamlandıktan sonra Azure yük dengeleyici ayarlarını doğrulayın. 

### <a name="step-2-configure-the-back-end-pool"></a>2. Adım: Arka uç havuzunu yapılandırma
Azure arka uç adres havuzu çağırır *arka uç havuzu*. Bu durumda, arka uç havuzu, bir kullanılabilirlik grubunda iki SQL Server örneği adresi olduğu. 

1. Kaynak grubunuzda, oluşturduğunuz yük dengeleyiciye tıklayın. 

2. Üzerinde **ayarları**, tıklayın **arka uç havuzları**.

3. Üzerinde **arka uç havuzları**, tıklayın **Ekle** bir arka uç adres havuzu oluşturma. 

4. Üzerinde **arka uç havuzu Ekle**altında **adı**, arka uç havuzu için bir ad yazın.

5. Altında **sanal makineler**, tıklayın **bir sanal makine ekleme**. 

6. Altında **sanal makineleri seçin**, tıklayın **bir kullanılabilirlik kümesi seçin**ve ardından kullanılabilirlik kümesi için SQL Server sanal makineleri ait olduğunu belirtin.

7. Kullanılabilirlik kümesini seçtikten sonra tıklayın **sanal makineleri seçin**iki sanal makine barındıran SQL Server örneklerinin kullanılabilirlik grubundaki seçin ve ardından **seçin**. 

8. Tıklayın **Tamam** için dikey pencereleri kapatmak için **sanal makineleri seçin**, ve **arka uç havuzu Ekle**. 

Azure arka uç adres havuzu ayarlarını güncelleştirir. Artık iki SQL Server örneği bir havuzu kullanılabilirlik kümeniz vardır.

### <a name="step-3-create-a-probe"></a>3. Adım: Bir araştırma oluşturma
Araştırma, Azure'nın hangi SQL Server örneklerinin kullanılabilirlik grubu dinleyicisi şu anda sahip nasıl doğrular tanımlar. Azure, araştırma oluştururken tanımladığınız bir bağlantı noktasında IP adresini temel alarak hizmetin araştırmaları.

1. Yük dengeleyicideki **ayarları** dikey penceresinde tıklayın **sistem durumu araştırmalarının**. 

2. Üzerinde **sistem durumu araştırmalarının** dikey penceresinde tıklayın **Ekle**.

3. Araştırmayı yapılandırın **araştırma Ekle** dikey penceresi. Araştırmayı yapılandırmak için aşağıdaki değerleri kullanın:

   | Ayar | Değer |
   | --- | --- |
   | **Ad** |Araştırmayı temsil eden bir metin adı. Örneğin, **SQLAlwaysOnEndPointProbe**. |
   | **Protokol** |**TCP** |
   | **Bağlantı Noktası** |Tüm kullanılabilir bağlantı noktası kullanabilirsiniz. Örneğin, *59999*. |
   | **Aralık** |*5* |
   | **Sağlıksız durum eşiği** |*2* |

4.  **Tamam** düğmesine tıklayın. 

> [!NOTE]
> Belirttiğiniz bağlantı noktası hem de SQL Server örneklerinin güvenlik duvarının açık olduğundan emin olun. Her iki örnekleri kullandığınız TCP bağlantı noktası için bir gelen kuralı gerektirir. Daha fazla bilgi için [Ekle veya güvenlik duvarı kuralını Düzenle](https://technet.microsoft.com/library/cc753558.aspx). 
> 
> 

Azure araştırması oluşturur ve bunu test etmek için hangi SQL Server örneği için kullanılabilirlik grubu dinleyici sahip kullanır.

### <a name="step-4-set-the-load-balancing-rules"></a>4. Adım: Yük Dengeleme kuralları ayarlayın
Yük Dengeleme kuralları, yük dengeleyicinin trafiği için SQL Server örneklerini nasıl yönlendirdiğini yapılandırın. Bu yük dengeleyici için iki SQL Server örneği yalnızca bir kullanılabilirlik grubu dinleyicisi kaynağı aynı anda sahibi olduğu için doğrudan sunucu dönüşü sağlar.

1. Yük dengeleyicideki **ayarları** dikey penceresinde tıklayın **Yük Dengeleme kuralları**. 

2. Üzerinde **Yük Dengeleme kuralları** dikey penceresinde tıklayın **Ekle**.

3. Üzerinde **Ekle Yük Dengeleme kuralları** dikey penceresinde Yük Dengeleme kuralını yapılandırın. Aşağıdaki ayarları kullanın: 

   | Ayar | Değer |
   | --- | --- |
   | **Ad** |Yük Dengeleme kuralları temsil eden bir metin adı. Örneğin, **SQLAlwaysOnEndPointListener**. |
   | **Protokol** |**TCP** |
   | **Bağlantı Noktası** |*1433* |
   | **Arka uç bağlantı noktası** |*1433*. Bu kural kullandığından, bu değer yoksayılır **kayan IP (doğrudan sunucu dönüşü)**. |
   | **Araştırma** |Bu yük dengeleyici için oluşturduğunuz araştırmayı adını kullanın. |
   | **Oturum kalıcılığı** |**Yok.** |
   | **Boşta kalma zaman aşımı (dakika)** |*4* |
   | **Kayan IP (doğrudan sunucu dönüşü)** |**Etkin** |

   > [!NOTE]
   > Tüm ayarlar dikey kaydırmanız gerekebilir.
   > 

4. **Tamam** düğmesine tıklayın. 
5. Azure Yük Dengeleme kuralını yapılandırır. Artık yük dengeleyici, trafiği yönlendirmek için kullanılabilirlik grubu dinleyicisini barındıran SQL Server örneği için yapılandırılmıştır. 

Bu noktada, kaynak grubuna hem SQL Server makinelerine bağlanan bir yük dengeleyici sahip. İki makine, kullanılabilirlik grupları için isteklere yanıt vermesi Yük Dengeleyiciyi SQL Server Always On kullanılabilirlik grubu dinleyicisi için bir IP adresi de içerir.

> [!NOTE]
> SQL Server örneklerinizin iki ayrı bölgede ise başka bir bölgede adımlarını yineleyin. Her bölgede bir yük dengeleyici gerektirir. 
> 
> 

## <a name="configure-the-cluster-to-use-the-load-balancer-ip-address"></a>Yük Dengeleyici IP adresi kullanmak için küme yapılandırma
Sonraki adım, küme üzerinde dinleyiciyi yapılandırın ve dinleyiciyi çevrimiçi duruma getirmek sağlamaktır. Şunları yapın: 

1. Kullanılabilirlik grubu dinleyicisi, yük devretme kümesinde oluşturun. 

2. Dinleyici çevrimiçi duruma getirin.

### <a name="step-5-create-the-availability-group-listener-on-the-failover-cluster"></a>5. Adım: Yük devretme kümesinde kullanılabilirlik grubu dinleyicisi oluşturun
Bu adımda, el ile yük devretme kümesi Yöneticisi'ni ve SQL Server Management Studio kullanılabilirlik grubu dinleyicisi oluşturun.

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

### <a name="verify-the-configuration-of-the-listener"></a>Dinleyici yapılandırmasını doğrulayın

Küme kaynakları ve bağımlılıkları doğru yapılandırılıp yapılandırılmadığını dinleyici SQL Server Management Studio'da görüntüleyebilirsiniz. Dinleyici bağlantı noktasını belirlemek için aşağıdakileri yapın:

1. SQL Server Management Studio'yu başlatın ve sonra birincil kopyaya bağlanın.

2. Git **AlwaysOn yüksek kullanılabilirlik** > **kullanılabilirlik grupları** > **kullanılabilirlik grubu dinleyicisi**.  
    Yük Devretme Kümesi Yöneticisi'nde, oluşturduğunuz adlı dinleyiciyi görmelisiniz. 

3. Dinleyici adına sağ tıklayın ve ardından **özellikleri**.

4. İçinde **bağlantı noktası** kutusunda, daha önce kullandığınız $EndpointPort kullanarak için kullanılabilirlik grubu dinleyicisinin bağlantı noktası numarası belirtin (1433 varsayılan olduğu) ve ardından **Tamam**.

Resource Manager modunda çalıştıran Azure sanal makineler'de artık bir kullanılabilirlik grubuna sahipsiniz. 

## <a name="test-the-connection-to-the-listener"></a>Sınama bağlantısı dinleyicisi
Bağlantıyı test etmek için aşağıdakileri yapın:

1. RDP için bir SQL Server örneği aynı sanal ağda bulunan, ancak çoğaltma sahibi değildir. Bu sunucu, kümedeki diğer SQL Server örneği olabilir.

2. Kullanım **sqlcmd** yardımcı programını kullanarak bağlantıyı test edin. Örneğin, aşağıdaki komut dosyası oluşturur bir **sqlcmd** Windows kimlik doğrulaması ile dinleyicisi aracılığıyla birincil kopyanın bağlantısı:
   
        sqlcmd -S <listenerName> -E

SQLCMD bağlantı otomatik olarak birincil çoğaltmasını barındıran SQL Server örneğine bağlanır. 

## <a name="create-an-ip-address-for-an-additional-availability-group"></a>Ek kullanılabilirlik grubu için bir IP adresi oluşturma

Her kullanılabilirlik grubu ayrı bir dinleyici kullanır. Her dinleyici kendi IP adresine sahiptir. IP adresi için ek dinleyicileri tutmak için aynı yük dengeleyici kullanın. Bir kullanılabilirlik grubu oluşturduktan sonra Yük Dengeleyici için IP adresini ekleyin ve ardından dinleyiciyi yapılandırın.

Azure portalı ile bir yük dengeleyici için bir IP adresi eklemek için aşağıdakileri yapın:

1. Azure portalında, Yük Dengeleyiciyi içeren kaynak grubunu açın ve ardından yük dengeleyiciye tıklayın. 

2. Altında **ayarları**, tıklayın **ön uç IP havuzu**ve ardından **Ekle**. 

3. Altında **ön uç IP adresi Ekle**, ön uç için bir ad atayın. 

4. Doğrulayın **sanal ağ** ve **alt** SQL Server örnekleri ile aynıdır.

5. IP adresi için bir dinleyici ayarlayın. 
   
   >[!TIP]
   >IP adresi statik olarak ayarlayın ve alt ağdaki şu anda kullanılmayan bir adresi yazın. Alternatif olarak, IP adresini dinamik olarak ayarlayın ve yeni ön uç IP havuzunu kaydedin. Bunu yaptığınızda, Azure portalında kullanılabilir bir IP adresi havuzuna otomatik olarak atar. Sonra ön uç IP havuzu yeniden açın ve ataması statik olarak değiştirin. 

6. İçin dinleyici IP adresini kaydedin. 

7. Aşağıdaki ayarları kullanarak bir durum araştırması Ekle:

   |Ayar |Değer
   |:-----|:----
   |**Ad** |Araştırma tanımlamak için bir ad.
   |**Protokol** |TCP
   |**Bağlantı Noktası** |Tüm sanal makinelerde kullanılabilir olması gereken bir kullanılmayan TCP bağlantı noktası. Başka bir amaçla kullanılamaz. Hiçbir iki dinleyici aynı araştırma bağlantı noktasını kullanabilirsiniz. 
   |**Aralık** |Araştırma girişimleri arasındaki süre miktarı. Varsayılan (5) kullanın.
   |**Sağlıksız durum eşiği** |Önce sanal makineyi başarısız olması ardışık eşikleri sayısı kötü olarak değerlendirilir.

8. Tıklayın **Tamam** araştırma kaydetmek için. 

9. Yük Dengeleme kuralı oluşturun. Tıklayın **Yük Dengeleme kuralları**ve ardından **Ekle**.

10. Yeni Yük Dengeleme kuralını aşağıdaki ayarlarla yapılandırın:

    |Ayar |Değer
    |:-----|:----
    |**Ad** |Yük Dengeleme kuralını tanımlamak için bir ad. 
    |**Ön uç IP adresi** |Oluşturduğunuz IP adresi seçin. 
    |**Protokol** |TCP
    |**Bağlantı Noktası** |SQL Server örneklerini kullanarak bağlantı noktasını kullanın. Bunu değiştirmediğiniz sürece bağlantı noktası 1433 varsayılan bir örnek kullanır. 
    |**Arka uç bağlantı noktası** |Aynı değeri kullanın **bağlantı noktası**.
    |**Arka uç havuzu** |SQL Server örneklerini içeren sanal makineler içeren havuz. 
    |**Durum araştırması** |Oluşturduğunuz araştırmayı seçin.
    |**Oturum kalıcılığı** |None
    |**Boşta kalma zaman aşımı (dakika)** |Varsayılan (4)
    |**Kayan IP (doğrudan sunucu dönüşü)** | Enabled

### <a name="configure-the-availability-group-to-use-the-new-ip-address"></a>Yeni IP adresini kullanmak için kullanılabilirlik grubu yapılandırma

Küme yapılandırmayı tamamlamak için ilk kullanılabilirlik grubu yapıldığında, uyguladığınız adımları yineleyin. Diğer bir deyişle, yapılandırma [yeni IP adresini kullanmak için küme](#configure-the-cluster-to-use-the-load-balancer-ip-address). 

Dinleyici için bir IP adresi ekledikten sonra aşağıdakileri yaparak ek kullanılabilirlik grubu yapılandırın: 

1. Araştırma bağlantı noktasını yeni IP adresi için hem SQL Server sanal makinelerinde açık olduğunu doğrulayın. 

2. [Küme Yöneticisi'nde, istemci erişim noktası Ekle](#addcap).

3. [Kullanılabilirlik grubu için IP kaynağını yapılandırmak](#congroup).

   >[!IMPORTANT]
   >IP adresi oluşturduğunuzda yük dengeleyiciye eklediğiniz IP adresini kullanın.  

4. [SQL Server kullanılabilirlik grubu kaynağının istemci erişim noktasında bağımlı hale](#dependencyGroup).

5. [İstemci erişim noktası kaynak IP adresine bağımlı yapma](#listname).
 
6. [PowerShell'de küme parametrelerinin](#setparam).

Kullanılabilirlik grubu yeni IP adresini kullanacak şekilde yapılandırdıktan sonra dinleyici bağlantısını yapılandırın. 

## <a name="add-load-balancing-rule-for-distributed-availability-group"></a>Yük Dengeleme kuralı için Dağıtılmış Kullanılabilirlik Grubu Ekle

Bir kullanılabilirlik grubuna dağıtılmış kullanılabilirlik grubunda yer alıyorsa, yük dengeleyicinin başka bir kural gerekir. Bu kural, dağıtılmış bir kullanılabilirlik grubu dinleyicisi tarafından kullanılan bağlantı noktası depolar.

>[!IMPORTANT]
>Kullanılabilirlik grubu içinde yer alıyorsa, bu adım yalnızca geçerlidir bir [dağıtılmış kullanılabilirlik grubu](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/configure-distributed-availability-groups). 

1. Dağıtılmış bir kullanılabilirlik grubuna katılan her sunucu üzerinde dağıtılmış kullanılabilirlik grubu dinleyicisi TCP bağlantı noktası üzerinde bir gelen kuralı oluşturun. Birçok örneklerde belgeleri 5022 kullanır. 

1. Azure portalında, yük dengeleyicide tıklayarak **Yük Dengeleme kuralları**ve ardından **+ Ekle**. 

1. Yük Dengeleme kuralını aşağıdaki ayarlarla oluşturun:

   |Ayar |Değer
   |:-----|:----
   |**Ad** |Yük Dengeleme kuralını dağıtılmış kullanılabilirlik grubu için tanımlamak için bir ad. 
   |**Ön uç IP adresi** |Aynı ön uç IP adresi kullanılabilirlik grubu olarak kullanın.
   |**Protokol** |TCP
   |**Bağlantı Noktası** |5022 - bağlantı noktası için [dağıtılmış bir kullanılabilirlik grubunun uç noktası dinleyicisi](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/configure-distributed-availability-groups).</br> Tüm kullanılabilir bağlantı noktası olabilir.  
   |**Arka uç bağlantı noktası** | 5022 - kullanımı aynı değer olarak **bağlantı noktası**.
   |**Arka uç havuzu** |SQL Server örneklerini içeren sanal makineler içeren havuz. 
   |**Durum araştırması** |Oluşturduğunuz araştırmayı seçin.
   |**Oturum kalıcılığı** |None
   |**Boşta kalma zaman aşımı (dakika)** |Varsayılan (4)
   |**Kayan IP (doğrudan sunucu dönüşü)** | Enabled

Load balancer'dağıtılmış kullanılabilirlik grubunda yer alan diğer kullanılabilirlik grupları için bu adımları yineleyin.

Eğer bir Azure ağ güvenlik grubu ile erişimi kısıtlama olun arka uç SQL Server VM IP adreslerine izin verme kuralları içerir ve yük dengeleyici kayan IP adresleri /AG dinleyicisi ve küme çekirdek IP adresi için varsa.

## <a name="next-steps"></a>Sonraki adımlar

- [Farklı bölgelerdeki Azure sanal makineler'de SQL Server Always On kullanılabilirlik grubu yapılandırma](virtual-machines-windows-portal-sql-availability-group-dr.md)
