---
title: "Azure sanal makinelerinde bir SQL Server kullanılabilirlik grubu dinleyicisi oluşturma | Microsoft Docs"
description: "Always On kullanılabilirlik grubu için bir dinleyici SQL Server için Azure sanal makinelerinde oluşturmak için adım adım yönergeler"
services: virtual-machines
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: monicar
ms.assetid: d1f291e9-9af2-41ba-9d29-9541e3adcfcf
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/01/2017
ms.author: mikeray
ms.openlocfilehash: 09fed7e785708d4afe64905de973becc188181d7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configure-a-load-balancer-for-an-always-on-availability-group-in-azure"></a>Always On kullanılabilirlik grubu için yük dengeleyici Azure'da yapılandırın
Bu makalede, Azure Resource Manager ile çalışan Azure sanal makinelerde bir SQL Server Always On kullanılabilirlik grubu için yük dengeleyici oluşturma açıklanmaktadır. SQL Server örnekleri üzerinde Azure sanal makine olduğunda bir kullanılabilirlik grubu yük dengeleyici gerektirir. Yük Dengeleyici için kullanılabilirlik grubu dinleyici IP adresini depolar. Bir kullanılabilirlik grubu birden çok bölgeye yayılırsa, her bölge bir yük dengeleyicinin gerekir.

Bu görevi tamamlamak için Resource Manager ile çalışan Azure sanal makinelerinde dağıtılan bir SQL Server kullanılabilirlik grubu olması gerekir. Her iki SQL Server sanal makineleri aynı kullanılabilirlik kümesine ait olması gerekir. Kullanabileceğiniz [Microsoft şablonu](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) kullanılabilirlik grubu Kaynak Yöneticisi'nde otomatik olarak oluşturmak için. Bu şablon bir iç yük dengeleyici sizin için otomatik olarak oluşturur. 

İsterseniz, aşağıdakileri yapabilirsiniz [el ile bir kullanılabilirlik grubu yapılandırmak](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Bu makalede, kullanılabilirlik grupları zaten yapılandırılmış olmasını gerektirir.  

İlgili Konular şunlardır:

* [Azure VM (GUI), Always On kullanılabilirlik grupları yapılandırma](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   
* [Azure Resource Manager ve PowerShell kullanarak VNet-VNet bağlantı yapılandırma](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

Bu makalede taramasını tarafından oluşturabilir ve Azure portalında bir yük dengeleyici yapılandırabilirsiniz. İşlem tamamlandıktan sonra küme için kullanılabilirlik grubu dinleyici IP adresi yük dengeleyiciden kullanmak için yapılandırın.

## <a name="create-and-configure-the-load-balancer-in-the-azure-portal"></a>Oluşturma ve Azure portalında yük dengeleyici yapılandırma
Bu görev bölümünde aşağıdakileri yapın:

1. Azure portalında yük dengeleyicisi oluşturun ve IP adresini yapılandırın.
2. Arka uç havuzunu yapılandırın.
3. Araştırma oluşturun. 
4. Yük Dengeleme kuralları ayarlayın.

> [!NOTE]
> SQL Server örneklerini birden çok kaynak grupları ve bölgelerde varsa, her adım, iki kez kez her kaynak grubunda gerçekleştirin.
> 
> 

### <a name="step-1-create-the-load-balancer-and-configure-the-ip-address"></a>1. adım: yük dengeleyicisi oluşturun ve IP adresini yapılandırın
İlk olarak, yük dengeleyici oluşturun. 

1. Azure Portal'da SQL Server sanal makineleri içeren kaynak grubunu açın. 

2. Kaynak grubunda tıklatın **Ekle**.

3. Arama **yük dengeleyici** ve arama sonuçlarında ardından **yük dengeleyici**, tarafından yayımlanan **Microsoft**.

4. Üzerinde **yük dengeleyici** dikey penceresinde tıklatın **oluşturma**.

5. İçinde **oluşturma yük dengeleyici** iletişim kutusunda, yük dengeleyici gibi yapılandırın:

   | Ayar | Değer |
   | --- | --- |
   | **Ad** |Yük Dengeleyici temsil eden bir metin adı. Örneğin, **sqlLB**. |
   | **Tür** |**İç**: çoğu uygulamalarını uygulamalarının aynı sanal ağ içinde kullanılabilirlik grubuna bağlanmasına olanak tanıyan bir iç yük dengeleyici kullanın.  </br> **Dış**: uygulamaların kullanılabilirlik grubu genel bir Internet bağlantısı üzerinden bağlanmasına izin verir. |
   | **Sanal ağ** |SQL Server intances bulunan sanal ağı seçin. |
   | **Alt ağ** |SQL Server örnekleri bulunan alt ağ seçin. |
   | **IP adresi ataması** |**Statik** |
   | **Özel IP adresi** |Kullanılabilir bir IP adresi alt ağından belirtin. Küme üzerinde bir dinleyici oluşturduğunuzda, bu IP adresi kullanın. Bu adres için bu makalenin sonraki bölümlerinde bir PowerShell Betiği kullanmak `$ILBIP` değişkeni. |
   | **Abonelik** |Birden çok aboneliğiniz varsa, bu alan görünebilir. Bu kaynak ile ilişkilendirmek istediğiniz aboneliği seçin. Normalde, kullanılabilirlik grubu için tüm kaynakların aynı abonelik olur. |
   | **Kaynak grubu** |SQL Server örnekleri bulunan kaynak grubu seçin. |
   | **Konum** |SQL Server örnekleri bulunan Azure konumu seçin. |

6. **Oluştur**'a tıklayın. 

Azure yük dengeleyici oluşturur. Yük Dengeleyici belirli ağ, alt ağ, kaynak grubunu ve konumu için aittir. Azure görev tamamlandıktan sonra Azure yük dengeleyici ayarları doğrulayın. 

### <a name="step-2-configure-the-back-end-pool"></a>2. adım: arka uç havuzunu yapılandırma
Azure arka uç adres havuzu çağırır *arka uç havuzu*. Bu durumda, arka uç havuzu, kullanılabilirlik grubunuzun iki SQL Server örneği adresleri ' dir. 

1. Kaynak grubunda oluşturduğunuz yük dengeleyici'ı tıklatın. 

2. Üzerinde **ayarları**, tıklatın **arka uç havuzları**.

3. Üzerinde **arka uç havuzları**, tıklatın **Ekle** bir arka uç adres havuzu oluşturmak için. 

4. Üzerinde **arka uç havuzu ekleme**altında **adı**, arka uç havuzu için bir ad yazın.

5. Altında **sanal makineleri**, tıklatın **bir sanal makine Ekle**. 

6. Altında **sanal makineleri seçin**, tıklatın **bir kullanılabilirlik kümesi seçin**ve ardından kullanılabilirlik kümesi SQL Server sanal makineleri ait olduğunu belirtin.

7. Kullanılabilirlik kümesi seçtikten sonra tıklatın **sanal makineleri seçin**, iki sanal makine kullanılabilirlik grubunda barındıran SQL Server örneği seçin ve ardından **seçin**. 

8. Tıklatın **Tamam** için dikey pencereleri kapatmak için **sanal makineleri seçin**, ve **arka uç havuzu ekleme**. 

Azure arka uç adres havuzu ayarlarını güncelleştirir. Şimdi iki SQL Server örneklerinin bir havuzu kullanılabilirlik kümesi vardır.

### <a name="step-3-create-a-probe"></a>3. adım: bir araştırma oluşturma
Azure, SQL Server örneklerinin kullanılabilirlik grubu dinleyicisi şu anda sahibi nasıl doğrular araştırma tanımlar. Azure araştırma oluşturduğunuzda tanımlayan bir bağlantı noktasında IP adresine bağlı hizmet araştırmaları.

1. Yük dengeleyicideki **ayarları** dikey penceresinde tıklatın **sistem durumu araştırmalarının**. 

2. Üzerinde **sistem durumu araştırmalarının** dikey penceresinde tıklatın **Ekle**.

3. Araştırmayı yapılandırma **Ekle araştırma** dikey. Araştırmayı yapılandırmak için aşağıdaki değerleri kullanın:

   | Ayar | Değer |
   | --- | --- |
   | **Ad** |Araştırmayı temsil eden bir metin adı. Örneğin, **SQLAlwaysOnEndPointProbe**. |
   | **Protokol** |**TCP** |
   | **Bağlantı Noktası** |Tüm kullanılabilir bağlantı noktası kullanabilirsiniz. Örneğin, *59999*. |
   | **Aralığı** |*5* |
   | **Sağlıksız durum eşiği.** |*2* |

4.  **Tamam** düğmesine tıklayın. 

> [!NOTE]
> Belirttiğiniz bağlantı noktasının Güvenlik Duvarı'nı her iki SQL Server örneklerinin açık olduğundan emin olun. Her iki örnek bir gelen kuralı kullandığınız için TCP bağlantı noktası gerektirir. Daha fazla bilgi için bkz: [Ekle veya Düzenle güvenlik duvarı kuralı](http://technet.microsoft.com/library/cc753558.aspx). 
> 
> 

Azure araştırması oluşturur ve dinleyicisinde kullanılabilirlik grubu için hangi SQL Server örneği kullanılır, test etmek için kullanır.

### <a name="step-4-set-the-load-balancing-rules"></a>4. adım: Yük Dengeleme kuralları ayarlama
Yük Dengeleme kuralları nasıl yük dengeleyici için SQL Server örnekleri trafiğini yönlendiren yapılandırın. Bu yük dengeleyici için iki SQL Server örneği yalnızca biri aynı anda kullanılabilirlik grubu dinleyicisi kaynağının sahibi olduğundan doğrudan sunucu dönüşü etkinleştirin.

1. Yük dengeleyicideki **ayarları** dikey penceresinde tıklatın **Yük Dengeleme kuralları**. 

2. Üzerinde **Yük Dengeleme kuralları** dikey penceresinde tıklatın **Ekle**.

3. Üzerinde **Ekle Yük Dengeleme kuralları** dikey penceresinde, Yük Dengeleme kuralını yapılandırın. Aşağıdaki ayarları kullanın: 

   | Ayar | Değer |
   | --- | --- |
   | **Ad** |Yük Dengeleme kuralları temsil eden bir metin adı. Örneğin, **SQLAlwaysOnEndPointListener**. |
   | **Protokol** |**TCP** |
   | **Bağlantı Noktası** |*1433* |
   | **Arka uç bağlantı noktası** |*1433*. Bu kural kullandığından bu değer yoksayılır **kayan IP (doğrudan sunucu dönüşü)**. |
   | **Araştırma** |Bu yük dengeleyici için oluşturduğunuz araştırmayı adını kullanın. |
   | **Oturum kalıcılığı** |**Yok** |
   | **Boşta kalma zaman aşımı (dakika)** |*4* |
   | **Kayan IP (doğrudan sunucu dönüşü)** |**Etkin** |

   > [!NOTE]
   > Tüm ayarları görüntülemek için dikey penceresini kaydırmanız gerekebilir.
   > 

4. **Tamam** düğmesine tıklayın. 
5. Azure Yük Dengeleme kuralını yapılandırır. Yük Dengeleyici kullanılabilirlik grubu dinleyicisini barındıran SQL Server örneğine trafiğini yönlendirmek için yapılandırılmıştır. 

Bu noktada, kaynak grubu, her iki SQL Server makine bağlayan bir yük dengeleyiciye sahiptir. Her iki makine kullanılabilirlik grupları için isteklerine yanıt vermesini sağlayabilirsiniz yük dengeleyici ayrıca SQL Server Always On kullanılabilirlik grubu dinleyicisi için bir IP adresini içerir.

> [!NOTE]
> SQL Server örnekleri iki ayrı bölgelerde bulunuyorsa, diğer bölge adımlarını yineleyin. Her bölge bir yük dengeleyici gerektirir. 
> 
> 

## <a name="configure-the-cluster-to-use-the-load-balancer-ip-address"></a>Yük Dengeleyici IP adresi kullanmak için küme yapılandırma
Kümede dinleyicisini yapılandırın ve dinleyiciyi çevrimiçi hale getirmek için sonraki adım olacaktır. Şunları yapın: 

1. Kullanılabilirlik grubu dinleyicisi üzerinde yük devretme kümesi oluşturun. 

2. Dinleyici çevrimiçi duruma getirin.

### <a name="step-5-create-the-availability-group-listener-on-the-failover-cluster"></a>5. adım: yük devretme kümesinde kullanılabilirlik grubu dinleyicisi oluşturma
Bu adımda, yük devretme kümesi Yöneticisi'ni ve SQL Server Management Studio kullanılabilirlik grubu dinleyicisi el ile oluşturun.

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

### <a name="verify-the-configuration-of-the-listener"></a>Dinleyici yapılandırmasını doğrulama

Bağımlılıklar ve küme kaynaklarının doğru şekilde yapılandırılırsa, SQL Server Management Studio'da dinleyicisi görmeye olmalıdır. Dinleyici bağlantı noktası için aşağıdakileri yapın:

1. SQL Server Management Studio'yu açın ve ardından birincil kopyaya bağlanın.

2. Git **AlwaysOn yüksek kullanılabilirlik** > **kullanılabilirlik grupları** > **kullanılabilirlik grubu dinleyicileri**.  
    Yük Devretme Kümesi Yöneticisi'nde oluşturulan dinleyici adı görmelisiniz. 

3. Dinleyici adına sağ tıklayın ve ardından **özellikleri**.

4. İçinde **bağlantı noktası** kutusunda, daha önce kullandığınız $EndpointPort kullanarak için kullanılabilirlik grubu dinleyicisinin bağlantı noktası numarası belirtin (1433 olduğu varsayılan) ve ardından **Tamam**.

Artık Azure sanal makinelerde Kaynak Yöneticisi modunda çalışan bir kullanılabilirlik grubuna sahip. 

## <a name="test-the-connection-to-the-listener"></a>Dinleyici bağlantıyı sınayın
Aşağıdakileri yaparak sınama bağlantısı:

1. RDP SQL Server örneğine aynı sanal ağda, ancak çoğaltma kendisine ait değil. Bu sunucu kümedeki diğer SQL Server örneği olabilir.

2. Kullanım **sqlcmd** yardımcı programını kullanarak bağlantıyı sınayın. Örneğin, aşağıdaki komut dosyasını oluşturur. bir **sqlcmd** Windows kimlik doğrulaması dinleyicisiyle aracılığıyla birincil çoğaltma bağlantısı:
   
        sqlcmd -S <listenerName> -E

SQLCMD bağlantı birincil çoğaltmayı barındıran SQL Server örneği otomatik olarak bağlanır. 

## <a name="create-an-ip-address-for-an-additional-availability-group"></a>Bir ek kullanılabilirlik grubu için bir IP adresi oluştur

Her kullanılabilirlik grubu ayrı bir dinleyicisi kullanır. Her dinleyicisi kendi IP adresi vardır. IP adresi için ek dinleyicileri tutmak için aynı yük dengeleyici kullanın. Bir kullanılabilirlik grubu oluşturduktan sonra IP adresini yük dengeleyiciye ekleyin ve dinleyicisini yapılandırın.

Azure portal ile bir yük dengeleyici için bir IP adresi eklemek için aşağıdakileri yapın:

1. Azure portalında yük dengeleyici içeren kaynak grubunu açın ve yük dengeleyici'ye tıklayın. 

2. Altında **ayarları**, tıklatın **ön uç IP havuzu**ve ardından **Ekle**. 

3. Altında **ön uç IP adresi Ekle**, ön uç için bir ad atayın. 

4. Doğrulayın **sanal ağ** ve **alt** SQL Server örnekleri ile aynıdır.

5. IP adresi için dinleyici ayarlayın. 
   
   >[!TIP]
   >IP adresi statik olarak ayarlayın ve alt ağ içindeki şu anda kullanılmamaktadır bir adres yazın. Alternatif olarak, IP adresi dinamik olarak ayarlayın ve yeni ön uç IP havuzu kaydedin. Bunu yaptığınızda, Azure portalında kullanılabilir bir IP adresi havuzuna otomatik olarak atar. Ardından, ön uç IP havuzu yeniden açın ve atama statik olarak değiştirin. 

6. IP adresi için dinleyici kaydedin. 

7. Aşağıdaki ayarları kullanarak bir sistem durumu araştırması ekleyin:

   |Ayar |Değer
   |:-----|:----
   |**Ad** |Araştırma tanımlamak için bir ad.
   |**Protokol** |TCP
   |**Bağlantı Noktası** |Tüm sanal makinelerde kullanılabilir olması gereken bir kullanılmayan TCP bağlantı noktası. Başka bir amaçla kullanılamaz. Hiçbir iki dinleyicileri aynı sonda bağlantı noktası kullanabilirsiniz. 
   |**Aralığı** |Araştırma girişimleri arasındaki süre miktarı. Varsayılan değer (5) kullanın.
   |**Sağlıksız durum eşiği.** |Bir sanal makine sağlıksız kabul edilmeden önce başarısız olması ardışık eşikleri sayısı.

8. Tıklatın **Tamam** araştırma kaydetmek için. 

9. Bir Yük Dengeleme kuralı oluşturun. Tıklatın **Yük Dengeleme kuralları**ve ardından **Ekle**.

10. Yeni Yük Dengeleme kuralını aşağıdaki ayarlarla yapılandırın:

   |Ayar |Değer
   |:-----|:----
   |**Ad** |Yük Dengeleme kuralını tanımlamak için bir ad. 
   |**Ön uç IP adresi** |Oluşturduğunuz IP adresi seçin. 
   |**Protokol** |TCP
   |**Bağlantı Noktası** |SQL Server örneklerini kullanarak bağlantı noktasını kullanır. Bunu değiştirmediyse bağlantı noktası 1433 varsayılan bir örnek kullanır. 
   |**Arka uç bağlantı noktası** |Aynı değer olarak **bağlantı noktası**.
   |**Arka uç havuzu** |SQL Server örnekleri ile sanal makineleri içeren havuz. 
   |**Sistem durumu araştırması** |Oluşturduğunuz araştırmayı seçin.
   |**Oturum kalıcılığı** |None
   |**Boşta kalma zaman aşımı (dakika)** |Varsayılan (4)
   |**Kayan IP (doğrudan sunucu dönüşü)** | Etkin

### <a name="configure-the-availability-group-to-use-the-new-ip-address"></a>Yeni IP adresini kullanmak için kullanılabilirlik grubunu yapılandırma

Küme yapılandırmayı tamamlamak için ilk kullanılabilirlik grubu yapıldığında, uyguladığınız adımları yineleyin. Diğer bir deyişle, yapılandırma [yeni IP adresini kullanmak için küme](#configure-the-cluster-to-use-the-load-balancer-ip-address). 

Bir IP adresi için dinleyici ekledikten sonra aşağıdakileri yaparak ek kullanılabilirlik grubu yapılandırın: 

1. Sonda bağlantı noktası yeni IP adresi için hem SQL Server sanal makinelerde açık olduğunu doğrulayın. 

2. [Küme Yöneticisi'nde, istemci erişim noktası Ekle](#addcap).

3. [IP kaynağı kullanılabilirlik grubu için yapılandırma](#congroup).

   >[!IMPORTANT]
   >IP adresi oluşturduğunuzda, yük dengeleyici ile eklenen IP adresi kullanın.  

4. [SQL Server kullanılabilirlik grubu kaynağı istemci erişim noktasında bağımlı hale](#dependencyGroup).

5. [İstemci erişim noktası kaynak IP adresine bağımlı yapma](#listname).
 
6. [PowerShell'de küme parametrelerinin](#setparam).

Kullanılabilirlik grubu yeni IP adresini kullanacak şekilde yapılandırdıktan sonra dinleyiciyi bağlantısını yapılandırın. 

## <a name="next-steps"></a>Sonraki adımlar

- [Farklı bölgelerdeki Azure sanal makinelerde bir SQL Server Always On kullanılabilirlik grubu yapılandırma](virtual-machines-windows-portal-sql-availability-group-dr.md)
