---
title: SQL Server kullanılabilirlik gruplarını - Azure sanal makineler - önkoşulları | Microsoft Docs
description: Bu öğreticide, Azure Vm'lerinde SQL Server Always On kullanılabilirlik grubu oluşturmak için önkoşulların nasıl yapılandırılacağı gösterilmektedir.
services: virtual-machines
documentationCenter: na
author: MikeRayMSFT
manager: craigg
editor: monicar
tags: azure-service-management
ms.assetid: c492db4c-3faa-4645-849f-5a1a663be55a
ms.service: virtual-machines-sql
ms.devlang: na
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/29/2018
ms.author: mikeray
ms.openlocfilehash: 1d0f3bfa03eb4bafdd10222e28782c318848b7f7
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60592163"
---
# <a name="complete-the-prerequisites-for-creating-always-on-availability-groups-on-azure-virtual-machines"></a>Azure sanal makinelerinde Always On kullanılabilirlik grupları oluşturmak için önkoşulları tamamlayın

Bu öğreticide, oluşturma önkoşullarını tamamlamak gösterilir bir [SQL Server Always On kullanılabilirlik grubu'Azure sanal makinelerinde (VM'ler)](virtual-machines-windows-portal-sql-availability-group-tutorial.md). Bir etki alanı denetleyicisi, iki SQL Server Vm'leri ve bir Tanık önkoşulları bitirdiğinizde, tek bir kaynak grubu içinde sahiptir.

**Tahmini Süre**: Bu, birkaç önkoşullarını tamamlamak için saat sürebilir. Bu süre çoğu sanal makine oluşturma harcanır.

Aşağıdaki diyagram, öğreticide yapı gösterir.

![Kullanılabilirlik grubu](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

## <a name="review-availability-group-documentation"></a>Kullanılabilirlik grubu belgelerini gözden geçirin

Bu öğreticide, SQL Server Always On kullanılabilirlik grupları hakkında bilgi sahibi olduğunuzu varsayar. Bu teknolojiyle ilgili bilgi sahibi değilseniz bkz [genel bakış, Always On kullanılabilirlik grupları (SQL Server)](https://msdn.microsoft.com/library/ff877884.aspx).


## <a name="create-an-azure-account"></a>Bir Azure hesabı oluşturun
Bir Azure hesabınız olmalıdır. Yapabilecekleriniz [ücretsiz bir Azure hesabı açın](https://signup.azure.com/signup?offer=ms-azr-0044p&appId=102&ref=azureplat-generic&redirectURL=https:%2F%2Fazure.microsoft.com%2Fget-started%2Fwelcome-to-azure%2F&correlationId=24f9d452-1909-40d7-b609-2245aa7351a6&l=en-US) veya [Visual Studio abone Avantajlarınızı etkinleştirebilir](https://docs.microsoft.com/visualstudio/subscriptions/subscriber-benefits).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Tıklayın **+** portalda yeni bir nesne oluşturmak için.

   ![Yeni nesne](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/01-portalplus.png)

3. Tür **kaynak grubu** içinde **Market** arama penceresi.

   ![Kaynak grubu](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/01-resourcegroupsymbol.png)
4. Tıklayın **kaynak grubu**.
5. **Oluştur**’a tıklayın.
6. Altında **kaynak grubu adı**, kaynak grubu için bir ad yazın. Örneğin **sql-ha-rg**.
7. Birden çok Azure aboneliğiniz varsa, abonelik, kullanılabilirlik grubundaki oluşturmak istediğiniz Azure aboneliğini olduğunu doğrulayın.
8. Bir konum seçin. Kullanılabilirlik grubu oluşturmak istediğiniz Azure bölgesi konumdur. Bu makalede bir Azure konumundaki tüm kaynakları oluşturur.
9. Doğrulayın **panoya Sabitle** denetlenir. Bu isteğe bağlı ayar, kaynak grubu için bir kısayol Azure portal panosunda yerleştirir.

   ![Kaynak grubu](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/01-resourcegroup.png)

10. Tıklayın **Oluştur** kaynak grubu oluşturun.

Azure kaynak grubu ve PIN portalda kaynak grubu için bir kısayol oluşturur.

## <a name="create-the-network-and-subnets"></a>Ağ ve alt ağlar oluşturun
Sonraki adım ağlar ve alt ağlar Azure kaynak grubunda oluşturmaktır.

Çözüm iki alt ağa sahip bir sanal ağ'ı kullanır. [Sanal ağa genel bakış](../../../virtual-network/virtual-networks-overview.md) ağları Azure hakkında daha fazla bilgi sağlar.

Sanal ağ oluşturmak için:

1. Azure portalında kaynak grubunuzu tıklayın **+ Ekle**. 

   ![Yeni öğe](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/02-newiteminrg.png)
2. Arama **sanal ağ**.

     ![Sanal ağ Ara](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/04-findvirtualnetwork.png)
3. Tıklayın **sanal ağ**.
4. Üzerinde **sanal ağ**, tıklayın **Resource Manager** dağıtım modeli ve ardından **Oluştur**.

    Aşağıdaki tabloda, sanal ağ ayarlarını gösterilmektedir:

   | **Alan** | Değer |
   | --- | --- |
   | **Ad** |autoHAVNET |
   | **Adres alanı** |10.33.0.0/24 |
   | **Alt ağ adı** |Yönetici |
   | **Alt ağ adres aralığı** |10.33.0.0/29 |
   | **Abonelik** |Kullanmak istediğiniz aboneliği belirtin. **Abonelik** yalnızca bir aboneliğiniz varsa boştur. |
   | **Kaynak grubu** |Seçin **var olanı kullan** ve kaynak grubu adını seçin. |
   | **Konum** |Azure konumu belirtin. |

   Adres alanı ve alt ağ adres aralığınızı tablodan farklı olabilir. Aboneliğinize bağlı olarak, portal bir kullanılabilir adres alanı ve ilgili alt ağ adres aralığı önerir. Hiçbir yeterli adres alanı varsa, farklı bir abonelik kullanın.

   Alt ağ adı örnekte **yönetici**. Bu alt etki alanı denetleyicilerinin ' dir.

5. **Oluştur**’a tıklayın.

   ![Sanal ağ yapılandırma](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/06-configurevirtualnetwork.png)

Azure portal panosuna döndürür ve yeni bir ağ oluşturulduğunda size bildirir.

### <a name="create-a-second-subnet"></a>İkinci bir alt ağ oluşturma
Adlı bir alt ağ, yeni bir sanal ağ olan **yönetici**. Etki alanı denetleyicileri bu alt ağ kullanın. Adlı ikinci bir alt ağ SQL Server sanal makinelerinin kullanım **SQL**. Bu alt ağı yapılandırmak için:

1. Panonuzda oluşturduğunuz kaynak grubuna tıklayın **SQL-HA-RG**. Kaynak grubu altında ağ bulun **kaynakları**.

    Varsa **SQL-HA-RG** görünmeyen, sabitlemediyseniz bunu bulmak **kaynak grupları** ve kaynak grubu adına göre filtreleme.
2. Tıklayın **autoHAVNET** kaynaklar listesi. 
3. Üzerinde **autoHAVNET** sanal ağ, altında **ayarları** seçin **alt ağlar**.

    Zaten oluşturduğunuz alt ağ unutmayın.

   ![Sanal ağ yapılandırma](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/07-addsubnet.png)
5. İkinci bir alt ağ oluşturun. Tıklayın **+ alt ağ**.
6. Üzerinde **alt ağ Ekle**, yazarak alt ağ yapılandırma **sqlsubnet** altında **adı**. Azure otomatik olarak belirten bir geçerli **adres aralığı**. Bu adres aralığı en az 10 adresleri içerdiğinden emin olun. Bir üretim ortamında, daha fazla adres gerektirebilir.
7. **Tamam**'ı tıklatın.

    ![Sanal ağ yapılandırma](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/08-configuresubnet.png)

Ağ yapılandırması ayarları aşağıdaki tabloda özetlenmiştir:

| **Alan** | Değer |
| --- | --- |
| **Ad** |**autoHAVNET** |
| **Adres alanı** |Bu değer, aboneliğinizdeki kullanılabilir adres alanları bağlıdır. 10\.0.0.0/16 tipik değeridir. |
| **Alt ağ adı** |**Yönetici** |
| **Alt ağ adres aralığı** |Bu değer, aboneliğinizdeki kullanılabilir adres aralıklarını bağlıdır. 10\.0.0.0/24 buna tipik bir değerdir. |
| **Alt ağ adı** |**sqlsubnet** |
| **Alt ağ adres aralığı** |Bu değer, aboneliğinizdeki kullanılabilir adres aralıklarını bağlıdır. 10\.0.1.0/24 buna tipik bir değerdir. |
| **Abonelik** |Kullanmak istediğiniz aboneliği belirtin. |
| **Kaynak Grubu** |**SQL-HA-RG** |
| **Konum** |Kaynak grubu için seçtiğiniz aynı konumu belirtin. |

## <a name="create-availability-sets"></a>Kullanılabilirlik kümeleri oluşturma

Sanal makine oluşturmadan önce kullanılabilirlik kümeleri oluşturmak gerekir. Kullanılabilirlik kümeleri, planlı veya Plansız bakım olayları için kapalı kalma süresini azaltın. Azure kullanılabilirlik kümesi, Azure üzerinde fiziksel hata etki alanları ve güncelleme etki alanları yerleştirir kaynakların mantıksal bir gruptur. Hata etki alanı, kullanılabilirlik kümesi üyesi ayrı gücü ve ağ kaynaklarına sahip olmasını sağlar. Kullanılabilirlik kümesi üyeleri bakım için aynı anda getirildiği olmayan, bir güncelleştirme etki alanı sağlar. Daha fazla bilgi için [sanal makinelerin kullanılabilirliğini yönetme](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

İki kullanılabilirlik kümesi gerekir. Etki alanı denetleyicilerinin biridir. SQL Server Vm'leri için saniyedir.

Bir kullanılabilirlik kümesi oluşturmak için kaynak grubuna gidin ve **Ekle**. Yazarak sonuçlara filtre **kullanılabilirlik kümesi**. Tıklayın **kullanılabilirlik kümesi** sonuçları ve ardından **Oluştur**.

Aşağıdaki tabloda iki kullanılabilirlik kümesi parametreleri göre yapılandırın:

| **Alan** | Etki alanı denetleyicisi kullanılabilirlik kümesi | SQL Server kullanılabilirlik kümesi |
| --- | --- | --- |
| **Ad** |adavailabilityset |sqlavailabilityset |
| **Kaynak grubu** |SQL-HA-RG |SQL-HA-RG |
| **Hata etki alanları** |3 |3 |
| **Güncelleme etki alanları** |5 |3 |

Azure portalında kaynak grubu için kullanılabilirlik kümeleri oluşturduktan sonra döndürür.

## <a name="create-domain-controllers"></a>Etki alanı denetleyicileri oluşturma
Ağ, alt ağlar, kullanılabilirlik kümeleri ve bir Internet'e yönelik Yük Dengeleyiciyi oluşturduktan sonra etki alanı denetleyicileri için sanal makineler oluşturmak hazırsınız.

### <a name="create-virtual-machines-for-the-domain-controllers"></a>Etki alanı denetleyicileri için sanal makineler oluşturun
Oluşturma ve etki alanı denetleyicilerini yapılandırmak için iade **SQL-HA-RG** kaynak grubu.

1. **Ekle**'yi tıklatın. 
2. Tür **Windows Server 2016 Datacenter**.
3. Tıklayın **Windows Server 2016 Datacenter**. İçinde **Windows Server 2016 Datacenter**, dağıtım modeli olduğundan emin olun **Resource Manager**ve ardından **Oluştur**. 

İki sanal makine oluşturmak için önceki adımları yineleyin. İki sanal makine adı:

* ad birincil dc
* ad ikincil dc

  > [!NOTE]
  > **Ad ikincil dc** sanal makine yüksek kullanılabilirlik sağlamak için Active Directory Domain Services isteğe bağlıdır.
  >
  >

Aşağıdaki tabloda, bu iki makine ayarlarını gösterilmektedir:

| **Alan** | Değer |
| --- | --- |
| **Ad** |İlk etki alanı denetleyicisi: *ad birincil dc*.</br>İkinci etki alanı denetleyicisi *ad ikincil dc*. |
| **VM disk türü** |SSD |
| **Kullanıcı adı** |DomainAdmin |
| **Parola** |Contoso! 0000 |
| **Abonelik** |*Aboneliğiniz* |
| **Kaynak grubu** |SQL-HA-RG |
| **Konum** |*Konumunuz* |
| **Boyut** |DS1_V2 |
| **Depolama** | **Yönetilen diskleri kullanma** - **Evet** |
| **Sanal ağ** |autoHAVNET |
| **Alt ağ** |Yönetici |
| **Genel IP adresi** |*VM adıyla aynı* |
| **Ağ güvenlik grubu** |*VM adıyla aynı* |
| **Kullanılabilirlik kümesi** |adavailabilityset </br>**Hata etki alanları**: 2 </br>**Güncelleme etki alanları**: 2|
| **Tanılama** |Enabled |
| **Tanılama depolama hesabı** |*Otomatik olarak oluşturulur* |

   >[!IMPORTANT]
   >Yalnızca, bir VM oluşturduğunuzda bir kullanılabilirlik içinde yerleştirebilirsiniz. VM oluşturulduktan sonra kullanılabilirlik değiştiremezsiniz. Bkz: [sanal makinelerin kullanılabilirliğini yönetme](../manage-availability.md).

Azure sanal makineleri oluşturur.

Sanal makine oluşturulduktan sonra etki alanı denetleyicisi yapılandırın.

### <a name="configure-the-domain-controller"></a>Etki alanı denetleyicisi yapılandırma
Aşağıdaki adımlarda, yapılandırma **ad birincil dc** corp.contoso.com etki alanı denetleyicisi olarak makine.

1. Portalda açın **SQL-HA-RG** kaynak grubu ve select **ad birincil dc** makine. Üzerinde **ad birincil dc**, tıklayın **Connect** Uzak Masaüstü erişimi için RDP dosyasını açın.

    ![Sanal makineye bağlanma](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/20-connectrdp.png)
2. Yapılandırılmış Yönetici hesabınızla oturum açın ( **\DomainAdmin**) ve parolayı (**Contoso! 0000**).
3. Varsayılan olarak, **Sunucu Yöneticisi'ni** Pano görüntülenmesi gerekir.
4. Tıklayın **rol ve Özellik Ekle** Panoda bağlantı.

    ![Sunucu Yöneticisi - rolleri ekleme](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/22-addfeatures.png)
5. Seçin **sonraki** gelene kadar **sunucu rolleri** bölümü.
6. Seçin **Active Directory Domain Services** ve **DNS sunucusu** rolleri. İstendiğinde, bu roller için gerekli olan diğer ek özellikleri ekleyin.

   > [!NOTE]
   > Windows, statik bir IP adresi olduğunu sizi uyarır. Yapılandırmayı test ediyorsanız tıklayın **devam**. Üretim senaryoları için Azure portalında, statik IP adresi ayarlayın veya [etki alanı denetleyicisi makinesine statik IP adresini ayarlamak için PowerShell kullanma](../../../virtual-network/virtual-networks-reserved-private-ip.md).
   >
   >

    ![Rol iletişim kutusu Ekle](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/23-addroles.png)
7. Tıklayın **sonraki** ulaşana kadar **onay** bölümü. Seçin **gerekirse hedef sunucuyu otomatik olarak yeniden** onay kutusu.
8. **Yükle**'ye tıklatın.
9. Özellikleri yükleme işlemini tamamladıktan sonra dönmek **Sunucu Yöneticisi'ni** Pano.
10. Yeni **AD DS** sol taraftaki bölmede seçeneği.
11. Tıklayın **daha fazla** sarı uyarı çubuğundaki bağlantı.

    ![DNS sunucusu VM AD DS iletişim kutusu](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/24-addsmore.png)
12. İçinde **eylem** sütununun **tüm sunucu görev ayrıntıları** iletişim kutusunda, tıklayın **bu sunucuyu bir etki alanı denetleyicisi yükseltme**.
13. İçinde **Active Directory etki alanı Hizmetleri Yapılandırma Sihirbazı**, aşağıdaki değerleri kullanın:

    | **Sayfa** | Ayar |
    | --- | --- |
    | **Dağıtım Yapılandırması** |**Yeni orman Ekle**<br/> **Kök etki alanı adı** corp.contoso.com = |
    | **Etki alanı denetleyicisi seçenekleri** |**DSRM parolasını** Contoso =! 0000<br/>**Parolayı onaylayın** Contoso =! 0000 |
14. Tıklayın **sonraki** sihirbazdaki diğer sayfalarına gitmek için. Üzerinde **Önkoşul denetimi** sayfasında, aşağıdaki iletisini gördüğünüzü doğrulayın: **Tüm önkoşul denetimlerinden başarıyla geçti**. Uygulanabilir tüm uyarı iletilerini gözden geçirebilirsiniz, ancak yükleme işlemine devam etmek mümkündür.
15. **Yükle**'ye tıklatın. **Ad birincil dc** sanal makine otomatik olarak yeniden başlatılır.

### <a name="note-the-ip-address-of-the-primary-domain-controller"></a>Birincil etki alanı denetleyicisinin IP adresini not alın

Birincil etki alanı denetleyicisi, DNS için kullanın. Birincil etki alanı denetleyicisi IP adresini not edin.

Birincil etki alanı denetleyicisi IP adresini almak için bir Azure portalı üzerinden yoludur.

1. Azure portalında, kaynak grubunu açın.

2. Birincil etki alanı denetleyicisi tıklayın.

3. Birincil etki alanı denetleyicisinde, **ağ arabirimleri**.

![Ağ arabirimleri](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/25-primarydcip.png)

Bu sunucu için özel IP adresini not edin.

### <a name="configure-the-virtual-network-dns"></a>Sanal ağ DNS yapılandırma
İlk etki alanı denetleyicisi oluşturmak ve DNS ilk sunucuda etkinleştirme sonra bu sunucunun DNS için kullanılacak sanal ağ yapılandırın.

1. Azure portalında sanal ağ'a tıklayın.

2. Altında **ayarları**, tıklayın **DNS sunucusu**.

3. Tıklayın **özel**, birincil etki alanı denetleyicisinin özel IP adresini yazın.

4. **Kaydet**’e tıklayın.

### <a name="configure-the-second-domain-controller"></a>İkinci etki alanı denetleyicisi yapılandırın
Birincil etki alanı denetleyicisi yeniden başlatıldıktan sonra ikinci etki alanı denetleyicisini yapılandırabilirsiniz. Bu isteğe bağlı bir adım, yüksek kullanılabilirlik içindir. İkinci etki alanı denetleyicisi yapılandırmak için aşağıdaki adımları izleyin:

1. Portalda açın **SQL-HA-RG** kaynak grubu ve select **ad ikincil dc** makine. Üzerinde **ad ikincil dc**, tıklayın **Connect** Uzak Masaüstü erişimi için RDP dosyasını açın.
2. VM için yapılandırılmış yönetici hesabınızı kullanarak oturum açın (**BUILTIN\DomainAdmin**) ve parolayı (**Contoso! 0000**).
3. Tercih edilen DNS sunucusu adresi, etki alanı denetleyicisinin adresine değiştirin.
4. İçinde **ağ ve Paylaşım Merkezi**, ağ arabirimi'a tıklayın.
   ![Ağ arabirimi](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/26-networkinterface.png)

5. **Özellikler**'e tıklayın.
6. Seçin **Internet Protokolü sürüm 4 (TCP/IPv4)** tıklatıp **özellikleri**.
7. Seçin **aşağıdaki DNS sunucu adreslerini kullan** ve birincil etki alanı denetleyicisini adresini belirtin **tercih edilen DNS sunucusu**.
8. Tıklayın **Tamam**, ardından **Kapat** değişiklikleri kaydetmek için. Artık VM katılamayacak **corp.contoso.com**.

   >[!IMPORTANT]
   >DNS ayarını değiştirdikten sonra Uzak Masaüstü bağlantısı kesildiğinde, Azure portalına gidin ve sanal makineyi yeniden başlatın.

9. İkincil etki alanı denetleyicisine uzak masaüstünden açmak **Sunucu Yöneticisi Panosu**.
10. Tıklayın **rol ve Özellik Ekle** Panoda bağlantı.

    ![Sunucu Yöneticisi - rolleri ekleme](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/22-addfeatures.png)
11. Seçin **sonraki** gelene kadar **sunucu rolleri** bölümü.
12. Seçin **Active Directory Domain Services** ve **DNS sunucusu** rolleri. İstendiğinde, bu roller için gerekli olan diğer ek özellikleri ekleyin.
13. Özellikleri yükleme işlemini tamamladıktan sonra dönmek **Sunucu Yöneticisi'ni** Pano.
14. Yeni **AD DS** sol taraftaki bölmede seçeneği.
15. Tıklayın **daha fazla** sarı uyarı çubuğundaki bağlantı.
16. İçinde **eylem** sütununun **tüm sunucu görev ayrıntıları** iletişim kutusunda, tıklayın **bu sunucuyu bir etki alanı denetleyicisi yükseltme**.
17. Altında **Dağıtım Yapılandırması**seçin **mevcut bir etki alanına bir etki alanı denetleyicisi eklemek**.
    ![Dağıtım Yapılandırması](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/28-deploymentconfig.png)
18. **Seç**'e tıklayın.
19. Yönetici hesabını kullanarak bağlanın (**CORP. CONTOSO.COM\domainadmin**) ve parolayı (**Contoso! 0000**).
20. İçinde **ormandan etki alanı seç**, etki alanına tıklayın ve ardından **Tamam**.
21. İçinde **etki alanı denetleyicisi seçenekleri**, varsayılan değerleri kullanın ve DSRM parolasını ayarlayın.

    >[!NOTE]
    >**DNS seçenekleri** sayfası uyar, bu DNS sunucusu için bir temsilci oluşturulamıyor. Üretim dışı ortamlarda bu uyarıyı yoksayabilirsiniz.
22. Tıklayın **sonraki** iletişim ulaşana kadar **önkoşulları** denetleyin. Ardından **Yükle**'ye tıklayın.

Sunucu yapılandırma değişikliklerini tamamlandıktan sonra sunucuyu yeniden başlatın.

### <a name="add-the-private-ip-address-to-the-second-domain-controller-to-the-vpn-dns-server"></a>İkinci etki alanı denetleyicisine VPN DNS sunucusu için özel IP adresini ekleyin

Azure portalında, sanal ağ DNS sunucusu, ikincil etki alanı denetleyicisinin IP adresini içerecek şekilde değiştirin. Bu ayar, DNS hizmeti yedeklilik sağlar.

### <a name="DomainAccounts"></a> Etki alanı hesaplarını yapılandırma

Sonraki adımlarda Active Directory hesaplarını yapılandırın. Aşağıdaki tabloda, hesapların gösterilmektedir:

| |Yükleme hesabı<br/> |SQLServer-0 <br/>SQL Server ve SQL Aracısı hizmeti hesabı |SQLServer-1<br/>SQL Server ve SQL Aracısı hizmeti hesabı
| --- | --- | --- | ---
|**Ad** |Yükleme |SQLSvc1 | SQLSvc2
|**Kullanıcı SamAccountName** |Yükleme |SQLSvc1 | SQLSvc2

Her hesap oluşturmak için aşağıdaki adımları kullanın.

1. Oturum **ad birincil dc** makine.
2. İçinde **Sunucu Yöneticisi'ni**seçin **Araçları**ve ardından **Active Directory Yönetim Merkezi**.   
3. Seçin **corp (yerel)** sol bölmeden.
4. Sağ taraftaki **görevleri** bölmesinde **yeni**ve ardından **kullanıcı**.
   ![Active Directory Yönetim Merkezi](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/29-addcnewuser.png)

   >[!TIP]
   >Her hesap için karmaşık bir parola ayarlayın.<br/> Üretim dışı ortamlar için kullanıcı hesabının süresi dolmayacak şekilde ayarlayın.

5. Tıklayın **Tamam** kullanıcı oluşturmak için.
6. Her üç hesapları için önceki adımları yineleyin.

### <a name="grant-the-required-permissions-to-the-installation-account"></a>Yükleme hesabı için gerekli izinleri verin
1. İçinde **Active Directory Yönetim Merkezi**seçin **corp (yerel)** sol bölmesinde. Ardından sağdaki **görevleri** bölmesinde tıklayın **özellikleri**.

    ![CORP kullanıcı özelliklerini](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/31-addcproperties.png)
2. Seçin **uzantıları**ve ardından **Gelişmiş** düğmesini **güvenlik** sekmesi.
3. İçinde **corp için Gelişmiş güvenlik ayarları** iletişim kutusunda, tıklayın **Ekle**.
4. Tıklayın **sorumlu Seç**, arama **CORP\Install**ve ardından **Tamam**.
5. Seçin **tüm özellikleri okumasını** onay kutusu.

6. Seçin **bilgisayar nesneleri oluşturma** onay kutusu.

     ![Corp kullanıcı izinleri](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/33-addpermissions.png)
7. Tıklayın **Tamam**ve ardından **Tamam** yeniden. Kapat **corp** Özellikler penceresi.

Active Directory ve kullanıcı nesnelerinin yapılandırma bitirdikten sonra iki SQL Server Vm'leri ve Tanık VM oluşturun. Ardından üç etki alanına katılın.

## <a name="create-sql-server-vms"></a>SQL Server Vm'leri oluşturma

Üç ek sanal makine oluşturun. Çözüm, SQL Server örneklerini içeren iki sanal makine gerektirir. Üçüncü bir sanal makine bir Tanık olarak çalışmaya devam edecek. Windows Server 2016 kullanabileceğiniz bir [bulut tanığı](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness), ancak bu belgenin önceki işletim sistemlerinde tutarlılık için bir Tanık sanal makine kullanır.  

Devam etmeden önce aşağıdaki tasarım kararlarını göz önünde bulundurun.

* **Depolama - Azure yönetilen diskler**

   Azure yönetilen diskler için sanal makine depolama alanını kullanın. Microsoft SQL Server sanal makineleri için yönetilen diskleri önerir. Yönetilen Diskler, depolama alanını arka planda yönetir. Ayrıca, Yönetilen Disklere sahip sanal makineler aynı kullanılabilirlik kümesinde olduğunda Azure uygun artıklık düzeyini sağlamak için depolama kaynaklarını dağıtır. Daha fazla bilgi için bkz. [Azure Yönetilen Disklere Genel Bakış](../managed-disks-overview.md). Bir kullanılabilirlik kümesindeki yönetilen diskler hakkında daha fazla ayrıntı için bkz. [bir kullanılabilirlik kümesindeki VM'ler için yönetilen diskleri kullan](../manage-availability.md#use-managed-disks-for-vms-in-an-availability-set).

* **Ağ - üretim ortamında özel IP adresleri**

   Bu öğreticide, sanal makineler için genel IP adresleri kullanılır. Genel bir IP adresi, internet üzerinden doğrudan sanal makineye uzaktan bağlantı sağlar - yapılandırma adımları kolaylaştırır. Microsoft, üretim ortamlarında, VM kaynak SQL Server örneğinin güvenlik açığı ayak izini azaltmak için özel IP adresleri önerir.

### <a name="create-and-configure-the-sql-server-vms"></a>Oluşturma ve SQL Server Vm'leri yapılandırma
Ardından, üç VM--iki SQL Server Vm'leri ve ek bir düğümü için bir VM oluşturun. Her bir VM oluşturmak için geri gidin **SQL-HA-RG** kaynak grubu, tıklayın **Ekle**, arama için uygun galeri öğesi, tıklayın **sanal makine**ve 'yetıklayın **Galeriden**. Bilgileri aşağıdaki tabloda sanal makineler oluşturmanıza yardımcı olması için kullanın:


| Sayfa | VM1 | VM2 | VM3 |
| --- | --- | --- | --- |
| Uygun galeri öğesi seçin |**Windows Server 2016 Datacenter** |**Windows Server 2016 üzerinde SQL Server 2016 SP1 Enterprise** |**Windows Server 2016 üzerinde SQL Server 2016 SP1 Enterprise** |
| Sanal Makine Yapılandırması **temelleri** |**Adı** küme fsw =<br/>**Kullanıcı adı** DomainAdmin =<br/>**Parola** Contoso =! 0000<br/>**Abonelik** aboneliğinizi =<br/>**Kaynak grubu** SQL-HA-RG =<br/>**Konum** azure konumunuz = |**Adı** sqlserver-0 =<br/>**Kullanıcı adı** DomainAdmin =<br/>**Parola** Contoso =! 0000<br/>**Abonelik** aboneliğinizi =<br/>**Kaynak grubu** SQL-HA-RG =<br/>**Konum** azure konumunuz = |**Adı** sqlserver-1 =<br/>**Kullanıcı adı** DomainAdmin =<br/>**Parola** Contoso =! 0000<br/>**Abonelik** aboneliğinizi =<br/>**Kaynak grubu** SQL-HA-RG =<br/>**Konum** azure konumunuz = |
| Sanal Makine Yapılandırması **boyutu** |**BOYUTU** DS1 =\_V2 (1 vCPU, 3,5 GB) |**BOYUTU** DS2 =\_V2 (2 Vcpu, 7 GB)</br>SSD depolama (Premium disk desteği. boyutu desteklemelidir )) |**BOYUTU** DS2 =\_V2 (2 Vcpu, 7 GB) |
| Sanal Makine Yapılandırması **ayarları** |**Depolama**: Yönetilen diskler kullanın.<br/>**Sanal ağ** autoHAVNET =<br/>**Alt ağ** sqlsubnet(10.1.1.0/24) =<br/>**Genel IP adresi** otomatik olarak oluşturulmuş.<br/>**Ağ güvenlik grubu** = yok<br/>**Tanılama izleme** = etkin<br/>**Tanılama depolama hesabı** = otomatik olarak oluşturulan depolama hesabı kullanın<br/>**Kullanılabilirlik kümesi** sqlAvailabilitySet =<br/> |**Depolama**: Yönetilen diskler kullanın.<br/>**Sanal ağ** autoHAVNET =<br/>**Alt ağ** sqlsubnet(10.1.1.0/24) =<br/>**Genel IP adresi** otomatik olarak oluşturulmuş.<br/>**Ağ güvenlik grubu** = yok<br/>**Tanılama izleme** = etkin<br/>**Tanılama depolama hesabı** = otomatik olarak oluşturulan depolama hesabı kullanın<br/>**Kullanılabilirlik kümesi** sqlAvailabilitySet =<br/> |**Depolama**: Yönetilen diskler kullanın.<br/>**Sanal ağ** autoHAVNET =<br/>**Alt ağ** sqlsubnet(10.1.1.0/24) =<br/>**Genel IP adresi** otomatik olarak oluşturulmuş.<br/>**Ağ güvenlik grubu** = yok<br/>**Tanılama izleme** = etkin<br/>**Tanılama depolama hesabı** = otomatik olarak oluşturulan depolama hesabı kullanın<br/>**Kullanılabilirlik kümesi** sqlAvailabilitySet =<br/> |
| Sanal Makine Yapılandırması **SQL Server ayarları** |Geçerli değil |**SQL Bağlantısı** özel (sanal ağ içinde) =<br/>**Bağlantı noktası** 1433 =<br/>**SQL kimlik doğrulaması** = devre dışı bırak<br/>**Depolama yapılandırması** genel =<br/>**Otomatik düzeltme eki uygulama** 2: 00'da Pazar =<br/>**Otomatik yedekleme** = devre dışı</br>**Azure Key Vault tümleştirmesi** = devre dışı |**SQL Bağlantısı** özel (sanal ağ içinde) =<br/>**Bağlantı noktası** 1433 =<br/>**SQL kimlik doğrulaması** = devre dışı bırak<br/>**Depolama yapılandırması** genel =<br/>**Otomatik düzeltme eki uygulama** 2: 00'da Pazar =<br/>**Otomatik yedekleme** = devre dışı</br>**Azure Key Vault tümleştirmesi** = devre dışı |

<br/>

> [!NOTE]
> Burada önerilen makine boyutlarını, kullanılabilirlik gruplarının Azure Vm'lerinde test etmek için yöneliktir. Üretim iş yüklerinde en iyi performans için SQL Server makine boyutlarına ve yapılandırma önerileri görmek [performans Azure sanal makineler'de SQL Server için en iyi uygulamaları](virtual-machines-windows-sql-performance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
>
>

Üç sanal makinelerin tam olarak sağlandıktan sonra bunlara katılmak gereken **corp.contoso.com** makinelere, CORP\Install yönetici hakları etki alanı ve verin.

### <a name="joinDomain"></a>Sunucular etki alanına ekleme

Artık Vm'lere katılamayacak **corp.contoso.com**. SQL Server Vm'leri ve dosya paylaşımı tanığı sunucusu için aşağıdaki adımları uygulayın:

1. Sanal makineyle uzaktan bağlanmak **BUILTIN\DomainAdmin**.
2. İçinde **Sunucu Yöneticisi'ni**, tıklayın **yerel sunucu**.
3. Tıklayın **çalışma grubu** bağlantı.
4. İçinde **bilgisayar adı** bölümünde **değişiklik**.
5. Seçin **etki alanı** onay kutusunu ve türü **corp.contoso.com** metin kutusuna. **Tamam**'ı tıklatın.
6. İçinde **Windows Güvenlik** açılan iletişim kutusunda, varsayılan etki alanı yönetici hesabı için kimlik bilgilerini belirtin (**CORP\DomainAdmin**) ve parolayı (**Contoso! 0000**) .
7. "Corp.contoso.com etki alanına Hoş Geldiniz" ileti gördüğünüzde tıklayın **Tamam**.
8. Tıklayın **Kapat**ve ardından **şimdi yeniden Başlat** açılan iletişim kutusunda.

### <a name="add-the-corpinstall-user-as-an-administrator-on-each-cluster-vm"></a>Corp\Install kullanıcı kümesindeki her VM bir yönetici olarak Ekle

Her bir sanal makine etki alanının bir üyesi yeniden başlatıldıktan sonra Ekle **CORP\Install** yerel Yöneticiler grubunun bir üyesi olarak.

1. VM yeniden başlatılana kadar bekleyin ve yeniden oturum açmak için birincil etki alanı denetleyicisi RDP dosyasını başlatın **sqlserver-0** kullanarak **CORP\DomainAdmin** hesabı.
   >[!TIP]
   >Etki alanı yönetici hesabıyla oturum emin olun. IN YERLEŞİK yönetici hesabı önceki adımlarda kullandığınız. Sunucu etki alanında olduğuna göre etki alanı hesabı kullanın. RDP oturumunuzda belirtin *etki alanı*\\*username*.

2. İçinde **Sunucu Yöneticisi'ni**seçin **Araçları**ve ardından **Bilgisayar Yönetimi**.
3. İçinde **Bilgisayar Yönetimi** penceresini genişletin **yerel kullanıcılar ve gruplar**ve ardından **grupları**.
4. Çift **Yöneticiler** grubu.
5. İçinde **Yöneticiler özellikleri** iletişim kutusunda, tıklayın **Ekle** düğmesi.
6. Kullanıcının girmesi **CORP\Install**ve ardından **Tamam**.
7. Tıklayın **Tamam** kapatmak için **yönetici özellikleri** iletişim.
8. Önceki adımları tekrar uygulayın **sqlserver-1** ve **küme fsw**.

### <a name="setServiceAccount"></a>SQL Server hizmet hesapları

Her SQL Server sanal makinesinde SQL Server hizmet hesabını ayarlayın. Etki alanı hesapları yapılandırıldığında oluşturduğunuz hesapları kullanın.

1. Açık **SQL Server Yapılandırma Yöneticisi**.
2. SQL Server hizmetini sağ tıklatın ve ardından **özellikleri**.
3. Hesap ve parola ayarlayın.
4. Bir SQL Server VM üzerinde bu adımları yineleyin.  

SQL Server kullanılabilirlik grupları için her SQL Server VM bir etki alanı hesabı olarak çalışması gerekir.

### <a name="create-a-sign-in-on-each-sql-server-vm-for-the-installation-account"></a>Her SQL Server VM yükleme hesabı için bir oturum açma oluşturun

Kullanılabilirlik grubunu yapılandırmak için (CORP\install) yükleme hesabı'nı kullanın. Bu hesabın üyesi olması gerekir **sysadmin** her SQL Server VM sabit sunucu rolü. Aşağıdaki adımlar, bir oturum açma için yükleme hesabı oluşturun:

1. Kullanarak Uzak Masaüstü Protokolü (RDP) ile sunucuya bağlanma  *\<MachineName\>\DomainAdmin* hesabı.

1. SQL Server Management Studio'yu açın ve yerel SQL Server örneğine bağlanın.

1. İçinde **Nesne Gezgini**, tıklayın **güvenlik**.

1. Sağ **oturumları**. Tıklayın **yeni oturum açma**.

1. İçinde **oturum açma - yeni**, tıklayın **arama**.

1. Tıklayın **konumları**.

1. Etki alanı yönetici ağ kimlik bilgilerini girin.

1. Yükleme hesabı'nı kullanın.

1. Bir üyesi olarak oturum ayarlanmış **sysadmin** sabit sunucu rolü.

1. **Tamam**'ı tıklatın.

Bir SQL Server VM önceki adımları tekrarlayın.

## <a name="add-failover-clustering-features-to-both-sql-server-vms"></a>Her iki SQL Server Vm'leri için yük devretme kümeleme özellikleri ekleyin

Yük devretme kümeleme özellikleri eklemek, hem SQL Server Vm'leri için aşağıdaki adımları yapın:

1. SQL Server sanal makinesine Uzak Masaüstü Protokolü (RDP) aracılığıyla kullanarak bağlanma *CORP\install* hesabı. Açık **Sunucu Yöneticisi Panosu**.
2. Tıklayın **rol ve Özellik Ekle** Panoda bağlantı.

    ![Sunucu Yöneticisi - rolleri ekleme](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/22-addfeatures.png)
3. Seçin **sonraki** gelene kadar **Server özellikleri** bölümü.
4. İçinde **özellikleri**seçin **Yük Devretme Kümelemesi**.
5. Gerekli diğer ek özellikleri ekleyin.
6. Tıklayın **yükleme** özellikleri eklemek için.

Bir SQL Server VM adımları tekrarlayın.

  >[!NOTE]
  > SQL Server Vm'leri için yük devretme kümesi, aslında katılma yanı sıra, bu adımı şimdi ile otomatikleştirilebilir [Azure SQL VM CLI](virtual-machines-windows-sql-availability-group-cli.md) ve [Azure hızlı başlangıç şablonları](virtual-machines-windows-sql-availability-group-quickstart-template.md).


## <a name="a-nameendpoint-firewall-configure-the-firewall-on-each-sql-server-vm"></a><a name="endpoint-firewall"> Her SQL Server VM Güvenlik duvarını yapılandırma

Çözüm, Güvenlik Duvarı'nda açık olması için aşağıdaki TCP bağlantı noktalarını gerektirir:

- **SQL Server sanal makinesi**:<br/>
   SQL Server'ın varsayılan bir örnek için 1433 numaralı bağlantı noktası.
- **Azure yük dengeleyici araştırması:**<br/>
   Tüm kullanılabilir bağlantı noktası. Örnekler, sık 59999 kullanın.
- **Veritabanı yansıtma uç noktası:** <br/>
   Tüm kullanılabilir bağlantı noktası. Örnekler, sık 5022 kullanın.

Güvenlik Duvarı bağlantı noktalarının hem SQL Server Vm'leri hakkında açık olması gerekir.

Bağlantı noktalarını açma yöntemi, güvenlik duvarı çözüm bağlıdır. Sonraki bölümde, Windows Güvenlik Duvarı bağlantı noktalarını açmak açıklanmaktadır. SQL Server sanal makinelerinizin tümünde gerekli bağlantı noktalarını açın.

### <a name="open-a-tcp-port-in-the-firewall"></a>Güvenlik Duvarı'nda bir TCP bağlantı noktasını açın

1. İlk SQL Server'da **Başlat** ekranında, başlatma **Gelişmiş Güvenlik Özellikli Windows Güvenlik Duvarı**.
2. Sol bölmeden **gelen kuralları**. Sağ bölmede, tıklayın **yeni kural**.
3. İçin **kural türü**, seçin **bağlantı noktası**.
4. Bağlantı noktasını belirtin **TCP** ve uygun bağlantı noktası numaralarını yazın. Aşağıdaki örneğe bakın:

   ![SQL güvenlik duvarı](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/35-tcpports.png)

5. **İleri**’ye tıklayın.
6. Üzerinde **eylem** sayfasında, korumak **bağlantıya izin** seçili ve ardından **sonraki**.
7. Üzerinde **profili** sayfasında varsayılan ayarları kabul edin ve ardından **sonraki**.
8. Üzerinde **adı** sayfasında, kural adını belirtin (gibi **Azure LB araştırma**) içinde **adı** metin kutusuna ve ardından **son**.

İkinci bir SQL Server sanal makinede bu adımları yineleyin.

## <a name="configure-system-account-permissions"></a>System hesabının izinlerini yapılandırma

Sistem hesabı için bir hesap oluşturun ve uygun izinleri vermek için her SQL Server örneği için aşağıdaki adımları tamamlayın:

1. İçin bir hesap `[NT AUTHORITY\SYSTEM]` her SQL Server örneğinde. Aşağıdaki betik bu hesabı oluşturur:

   ```sql
   USE [master]
   GO
   CREATE LOGIN [NT AUTHORITY\SYSTEM] FROM WINDOWS WITH DEFAULT_DATABASE=[master]
   GO 
   ```

1. Aşağıdaki izinleri vermek `[NT AUTHORITY\SYSTEM]` her SQL Server örneğinde:

   - `ALTER ANY AVAILABILITY GROUP`
   - `CONNECT SQL`
   - `VIEW SERVER STATE`

   Aşağıdaki betiği bu izinleri verir:

   ```sql
   GRANT ALTER ANY AVAILABILITY GROUP TO [NT AUTHORITY\SYSTEM]
   GO
   GRANT CONNECT SQL TO [NT AUTHORITY\SYSTEM]
   GO
   GRANT VIEW SERVER STATE TO [NT AUTHORITY\SYSTEM]
   GO 
   ```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure sanal makinelerde SQL Server Always On kullanılabilirlik grubu oluşturma](virtual-machines-windows-portal-sql-availability-group-tutorial.md)
