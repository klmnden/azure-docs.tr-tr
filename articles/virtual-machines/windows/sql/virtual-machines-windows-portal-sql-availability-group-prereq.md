---
title: SQL Server kullanılabilirlik gruplarını - Azure sanal makineleri - önkoşulları | Microsoft Docs
description: Bu öğreticide Azure Vm'lerinde SQL Server Always On kullanılabilirlik grubu oluşturmak için önkoşulların nasıl yapılandırılacağı gösterilmiştir.
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
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
ms.openlocfilehash: f2a0af65af068f3a78a08e46e0e42caefd87d7b1
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
ms.locfileid: "30322905"
---
# <a name="complete-the-prerequisites-for-creating-always-on-availability-groups-on-azure-virtual-machines"></a>Azure sanal makinelerde Always On kullanılabilirlik grupları oluşturmak için önkoşulları tamamlamanız

Bu öğretici oluşturmak için önkoşulları tamamlamanız gösterilmektedir bir [SQL Server Always On kullanılabilirlik grubu Azure sanal makinelerde (VM'ler)](virtual-machines-windows-portal-sql-availability-group-tutorial.md). Bir etki alanı denetleyicisi, iki SQL Server Vm'lerinin ve bir Tanık önkoşulları tamamladığınızda, tek bir kaynak grubu içinde vardır.

**Zaman tahmin**: birkaç önkoşulları tamamlamanız için saat sürebilir. Bu süre çoğunu sanal makinelerin oluşturulmasını harcanmaktadır.

Aşağıdaki diyagram, öğreticide yapı gösterir.

![Kullanılabilirlik grubu](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

## <a name="review-availability-group-documentation"></a>Kullanılabilirlik grubu belgelerini gözden geçirin

Bu öğretici, SQL Server Always On kullanılabilirlik grupları temel bilgilere sahip olduğunuzu varsayar. Bu teknolojiyle bilmiyorsanız bkz [genel bakış, Always On kullanılabilirlik grupları (SQL Server)](http://msdn.microsoft.com/library/ff877884.aspx).


## <a name="create-an-azure-account"></a>Bir Azure hesabı oluşturun
Bir Azure hesabınız olmalıdır. Yapabilecekleriniz [ücretsiz bir Azure hesabı açın](/pricing/free-trial/?WT.mc_id=A261C142F) veya [Visual Studio abone Avantajlarınızı etkinleştirebilir](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
1. [Azure Portal](http://portal.azure.com)’da oturum açın.
2. Tıklatın **+** Portalı'nda yeni bir nesne oluşturmak için.

   ![Yeni nesne](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/01-portalplus.png)

3. Tür **kaynak grubu** içinde **Market** arama penceresi.

   ![Kaynak grubu](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/01-resourcegroupsymbol.png)
4. Tıklatın **kaynak grubu**.
5. **Oluştur**’a tıklayın.
6. Altında **kaynak grubu adı**, kaynak grubu için bir ad yazın. Örneğin, **sql-ha-rg**.
7. Birden çok Azure aboneliğiniz varsa, aboneliğin kullanılabilirlik grubunda oluşturmak istediğiniz Azure aboneliği olduğundan emin olun.
8. Bir konum seçin. Kullanılabilirlik grubu oluşturmak istediğiniz Azure bölgesini konumdur. Bu makale bir Azure konumdaki tüm kaynaklar oluşturur.
9. Doğrulayın **panoya Sabitle** denetlenir. Bu isteğe bağlı ayar Azure portal panosunda kaynak grubu için bir kısayol yerleştirir.

   ![Kaynak grubu](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/01-resourcegroup.png)

10. Tıklatın **oluşturma** kaynak grubu oluşturun.

Azure kaynak grubu ve PIN Portalı'nda bir kısayol kaynak grubuna oluşturur.

## <a name="create-the-network-and-subnets"></a>Ağ ve alt ağlar oluşturun
Sonraki adım ağlar ve alt ağlar Azure kaynak grubu oluşturmaktır.

Çözüm, bir sanal ağı iki alt ağ ile kullanır. [Sanal ağa genel bakış](../../../virtual-network/virtual-networks-overview.md) Azure ağlar hakkında daha fazla bilgi sağlar.

Sanal ağ oluşturmak için:

1. Azure portalında, kaynak grubunuzun tıklatın **+ Ekle**. 

   ![Yeni öğe](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/02-newiteminrg.png)
2. Arama **sanal ağ**.

     ![Arama sanal ağ](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/04-findvirtualnetwork.png)
3. Tıklatın **sanal ağ**.
4. Üzerinde **sanal ağ**, tıklatın **Resource Manager** dağıtım modeli ve ardından **oluşturma**.

    Aşağıdaki tabloda sanal ağ ayarlarını gösterir:

   | **Alan** | Değer |
   | --- | --- |
   | **Ad** |autoHAVNET |
   | **Adres alanı** |10.33.0.0/24 |
   | **Alt ağ adı** |Yönetim Bölgesi |
   | **Alt ağ adres aralığı** |10.33.0.0/29 |
   | **Abonelik** |Kullanmak istediğiniz aboneliği belirtin. **Abonelik** yalnızca bir aboneliğiniz varsa boştur. |
   | **Kaynak grubu** |Seçin **var olanı kullan** ve kaynak grubunun adını seçin. |
   | **Konum** |Azure konumu belirtin. |

   Adres alanı ve alt ağ adres aralığınızı tablodan farklı olabilir. Aboneliğinize bağlı olarak, portal kullanılabilir adres alanı ve karşılık gelen alt ağ adres aralığı önerir. Hiçbir yeterli adres alanı varsa, farklı bir abonelik kullanın.

   Örnek bir alt ağ adı kullanır **yönetici**. Bu alt etki alanı denetleyicileri değil.

5. **Oluştur**’a tıklayın.

   ![Sanal ağ yapılandırma](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/06-configurevirtualnetwork.png)

Azure portal panosuna verir ve yeni bir ağ oluşturulduğunda bunu size bildirir.

### <a name="create-a-second-subnet"></a>İkinci alt ağ oluşturma
Yeni sanal ağ, bir alt ağ, sahip **yönetici**. Etki alanı denetleyicileri bu alt ağ kullanın. İkinci bir alt ağ adlı SQL Server Vm'lerinin kullanım **SQL**. Bu alt ağ yapılandırmak için:

1. Panonuzda, oluşturduğunuz kaynak grubunu tıklatın **SQL-HA-RG**. Kaynak grubu altında ağ bulun **kaynakları**.

    Varsa **SQL-HA-RG** görünmeyen, bunu bulmak **kaynak grupları** ve kaynak grubu adına göre filtreleme.
2. Tıklatın **autoHAVNET** kaynaklar listesi üzerinde. 
3. Üzerinde **autoHAVNET** sanal ağ, altında **ayarları** , tıklatın **alt ağlar**.

    Önceden oluşturulmuş alt ağ unutmayın.

   ![Sanal ağ yapılandırma](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/07-addsubnet.png)
5. İkinci bir alt ağ oluşturun. Tıklatın **+ alt**.
6. Üzerinde **alt ağ Ekle**, alt yazarak yapılandırın **sqlsubnet** altında **adı**. Azure otomatik olarak belirtir geçerli bir **adres aralığı**. Bu adres aralığı en az 10 adresleri içinde olduğunu doğrulayın. Bir üretim ortamında, daha fazla adresleri gerektirebilir.
7. **Tamam**’a tıklayın.

    ![Sanal ağ yapılandırma](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/08-configuresubnet.png)

Ağ yapılandırması ayarları aşağıdaki tabloda özetlenmiştir:

| **Alan** | Değer |
| --- | --- |
| **Ad** |**autoHAVNET** |
| **Adres alanı** |Bu değer, aboneliğinizdeki kullanılabilir adres alanları bağlıdır. Tipik bir değer 10.0.0.0/16 ' dir. |
| **Alt ağ adı** |**admin** |
| **Alt ağ adres aralığı** |Bu değer, aboneliğinizdeki kullanılabilir adres aralıklarını bağlıdır. Tipik bir değer 10.0.0.0/24 ' dir. |
| **Alt ağ adı** |**sqlsubnet** |
| **Alt ağ adres aralığı** |Bu değer, aboneliğinizdeki kullanılabilir adres aralıklarını bağlıdır. Tipik bir değer 10.0.1.0/24 ' dir. |
| **Abonelik** |Kullanmak istediğiniz aboneliği belirtin. |
| **Kaynak Grubu** |**SQL-HA-RG** |
| **Konum** |Kaynak grubu için seçtiğiniz aynı konumu belirtin. |

## <a name="create-availability-sets"></a>Kullanılabilirlik kümeleri oluşturma

Sanal makineleri oluşturmadan önce kullanılabilirlik kümelerini oluşturmanız gerekir. Kullanılabilirlik kümeleri, planlı veya plansız bir bakım olayları için kapalı kalma süresini kısaltabilir. Bir Azure kullanılabilirlik kümesi, fiziksel hata etki alanları ve güncelleme etki alanına Azure yerleştirir kaynakların mantıksal bir gruptur. Hata etki alanı kullanılabilirlik kümesi üyeleri ayrı güç ve ağ kaynaklarına sahip olmasını sağlar. Bir güncelleştirme etki alanı kullanılabilirlik kümesi üyeleri bakım için aynı anda duruma olmayan olmasını sağlar. Ek bilgi için bkz: [sanal makinelerin kullanılabilirliğini yönetme](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

İki kullanılabilirlik kümesi gerekir. Etki alanı denetleyicileri biridir. SQL Server VM'ler için saniyedir.

Bir kullanılabilirlik kümesi oluşturmak için kaynak grubuna gidin ve **Ekle**. Yazarak sonuçlara filtre **kullanılabilirlik kümesi**. Tıklatın **kullanılabilirlik kümesi** sonuçları ve ardından **oluşturma**.

Aşağıdaki tabloda iki kullanılabilirlik kümeleri parametreleri göre yapılandırın:

| **Alan** | Etki alanı denetleyicisi kullanılabilirlik kümesi | SQL Server kullanılabilirlik kümesi |
| --- | --- | --- |
| **Ad** |adavailabilityset |sqlavailabilityset |
| **Kaynak grubu** |SQL-HA-RG |SQL-HA-RG |
| **Hata etki alanları** |3 |3 |
| **Güncelleme etki alanları** |5 |3 |

Kullanılabilirlik kümesi oluşturduktan sonra Azure Portal'daki kaynak grubu dönün.

## <a name="create-domain-controllers"></a>Etki alanı denetleyicileri oluşturun
Ağ, alt ağlar, kullanılabilirlik kümeleri ve bir Internet'e yönelik Yük Dengeleyici oluşturduktan sonra etki alanı denetleyicileri sanal makineler oluşturmak hazırsınız.

### <a name="create-virtual-machines-for-the-domain-controllers"></a>Etki alanı denetleyicileri için sanal makineler oluşturun
Oluşturma ve etki alanı denetleyicilerini yapılandırmak için dönmek **SQL-HA-RG** kaynak grubu.

1. **Ekle**'ye tıklayın. 
2. Tür **Windows Server 2016 Datacenter**.
3. Tıklatın **Windows Server 2016 Datacenter**. İçinde **Windows Server 2016 Datacenter**, dağıtım modeli olduğunu doğrulayın **Resource Manager**ve ardından **oluşturma**. 

İki sanal makine oluşturmak için önceki adımları yineleyin. İki sanal makine adı:

* ad-primary-dc
* ad-secondary-dc

  > [!NOTE]
  > **Ad ikincil dc** sanal makineyi yüksek kullanılabilirlik sağlamak için Active Directory etki alanı Hizmetleri isteğe bağlıdır.
  >
  >

Aşağıdaki tabloda bu iki makine ayarlarını gösterir:

| **Alan** | Değer |
| --- | --- |
| **Ad** |İlk etki alanı denetleyicisi: *ad birincil dc*.</br>İkinci etki alanı denetleyicisi *ad ikincil dc*. |
| **VM disk türü** |SSD |
| **Kullanıcı adı** |Etki alanı yöneticisi |
| **Parola** |Contoso!0000 |
| **Abonelik** |*Aboneliğiniz* |
| **Kaynak grubu** |SQL-HA-RG |
| **Konum** |*Konumunuz* |
| **Boyut** |DS1_V2 |
| **Depolama** | **Yönetilen diskler kullanan** - **Evet** |
| **Sanal ağ** |autoHAVNET |
| **Alt ağ** |yönetici |
| **Genel IP adresi** |*VM adıyla aynı* |
| **Ağ güvenlik grubu** |*VM adıyla aynı* |
| **Kullanılabilirlik kümesi** |adavailabilityset </br>**Hata etki alanları**: 2</br>**Güncelleme etki alanları**: 2|
| **Tanılama** |Etkin |
| **Tanılama depolama hesabı** |*Otomatik olarak oluşturulur* |

   >[!IMPORTANT]
   >Bu gibi durumlarda, bir VM yalnızca kullanılabilirlik oluşturduğunuzda kümesi içinde yerleştirebilirsiniz. Kullanılabilirlik kümesini VM oluşturulduktan sonra değiştirilemez. Bkz: [sanal makinelerin kullanılabilirliğini yönetme](../manage-availability.md).

Azure sanal makineleri oluşturur.

Sanal makineler oluşturulduktan sonra etki alanı denetleyicisi yapılandırın.

### <a name="configure-the-domain-controller"></a>Etki alanı denetleyicisini Yapılandır
Aşağıdaki adımlarda yapılandırma **ad birincil dc** corp.contoso.com etki alanı denetleyicisi olarak makine.

1. Portalı'nda açmak **SQL-HA-RG** kaynak grubu ve select **ad birincil dc** makine. Üzerinde **ad birincil dc**, tıklatın **Bağlan** Uzak Masaüstü erişimi için RDP dosyasını açın.

    ![Bir sanal makineye bağlanma](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/20-connectrdp.png)
2. Yapılandırılmış Yönetici hesabınızla oturum açın (**\DomainAdmin**) ve parolayı (**Contoso! 0000**).
3. Varsayılan olarak, **Sunucu Yöneticisi'ni** Pano görüntülenmesi.
4. Tıklatın **rol ve Özellik Ekle** Panoda bağlantı.

    ![Sunucu Yöneticisi - rolleri Ekle](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/22-addfeatures.png)
5. Seçin **sonraki** için elde edene kadar **sunucu rolleri** bölümü.
6. Seçin **Active Directory etki alanı Hizmetleri** ve **DNS sunucusu** rolleri. İstendiğinde, bu roller için gerekli olan diğer ek özellikleri ekleyin.

   > [!NOTE]
   > Windows, statik IP adresi olduğunu sizi uyarır. Yapılandırmayı test ediyorsanız tıklatın **devam**. Üretim senaryoları için Azure portalında, statik IP adresi ayarlayın veya [etki alanı denetleyicisi makinenin statik IP adresi ayarlamak için PowerShell kullanın](../../../virtual-network/virtual-networks-reserved-private-ip.md).
   >
   >

    ![Rolleri iletişim ekleyin](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/23-addroles.png)
7. Tıklatın **sonraki** ulaşana kadar **onay** bölümü. Seçin **gerekirse hedef sunucuyu otomatik olarak yeniden** onay kutusu.
8. **Yükle**'ye tıklayın.
9. Özellikleri yükleme işlemini tamamladıktan sonra geri dönüp **Sunucu Yöneticisi'ni** Pano.
10. Yeni **AD DS** sol bölmesinden seçeneği.
11. Tıklatın **daha fazla** sarı uyarı çubuğundaki bağlantı.

    ![DNS sunucusu VM AD DS iletişim kutusu](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/24-addsmore.png)
12. İçinde **eylem** sütunu **tüm sunucu görev ayrıntıları** iletişim kutusunda, tıklatın **bu sunucu bir etki alanı denetleyicisine Yükselt**.
13. İçinde **Active Directory etki alanı Hizmetleri Yapılandırma Sihirbazı**, aşağıdaki değerleri kullanın:

    | **Sayfa** | Ayar |
    | --- | --- |
    | **Dağıtım Yapılandırması** |**Yeni orman Ekle**<br/> **Kök etki alanı adı** corp.contoso.com = |
    | **Etki alanı denetleyicisi seçenekleri** |**DSRM parolasını** Contoso =! 0000<br/>**Parolayı onaylayın** Contoso =! 0000 |
14. Tıklatın **sonraki** bir sihirbaz sayfalarında gitmesi. Üzerinde **Önkoşul denetimi** sayfasında, aşağıdaki iletiyi görürsünüz doğrulayın: **tüm önkoşul denetimlerinden başarıyla geçti**. Uygulanabilir tüm uyarı iletilerini gözden geçirebilirsiniz, ancak yükleme işlemine devam etmek mümkündür.
15. **Yükle**'ye tıklayın. **Ad birincil dc** sanal makine otomatik olarak yeniden başlatılır.

### <a name="note-the-ip-address-of-the-primary-domain-controller"></a>Birincil etki alanı denetleyicisinin IP adresini not edin

Birincil etki alanı denetleyicisi, DNS için kullanın. Birincil etki alanı denetleyicisi IP adresini not edin.

Birincil etki alanı denetleyicisi IP adresini almak için bir Azure portalı üzerinden yoludur.

1. Azure Portalı'nda, kaynak grubu açın.

2. Birincil etki alanı denetleyicisini tıklatın.

3. Birincil etki alanı denetleyicisinde **ağ arabirimleri**.

![Ağ arabirimleri](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/25-primarydcip.png)

Bu sunucu için özel IP adresini not edin.

### <a name="configure-the-virtual-network-dns"></a>Sanal ağ DNS yapılandırma
İlk etki alanı denetleyicisi oluşturmak ve DNS ilk sunucuda etkinleştirme sonra bu sunucu için DNS kullanmak üzere sanal ağ yapılandırın.

1. Azure portalında sanal ağ'ı tıklatın.

2. Altında **ayarları**, tıklatın **DNS sunucusu**.

3. Tıklatın **özel**ve birincil etki alanı denetleyicisi özel IP adresini yazın.

4. **Kaydet**’e tıklayın.

### <a name="configure-the-second-domain-controller"></a>İkinci etki alanı denetleyicisi yapılandırma
Birincil etki alanı denetleyicisi yeniden başlatıldıktan sonra ikinci etki alanı denetleyicisi yapılandırabilirsiniz. Bu isteğe bağlı yüksek kullanılabilirlik için bir adımdır. İkinci etki alanı denetleyicisi yapılandırmak için aşağıdaki adımları izleyin:

1. Portalı'nda açmak **SQL-HA-RG** kaynak grubu ve select **ad ikincil dc** makine. Üzerinde **ad ikincil dc**, tıklatın **Bağlan** Uzak Masaüstü erişimi için RDP dosyasını açın.
2. VM, yapılandırılmış yönetici hesabı kullanarak oturum açın (**BUILTIN\DomainAdmin**) ve parolayı (**Contoso! 0000**).
3. Tercih edilen DNS sunucusu adresi ve etki alanı denetleyicisinin adresine değiştirin.
4. İçinde **ağ ve Paylaşım Merkezi**, ağ arabirimi'ı tıklatın.
   ![Ağ arabirimi](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/26-networkinterface.png)

5. **Özellikler**'e tıklayın.
6. Seçin **Internet Protokolü sürüm 4 (TCP/IPv4)** tıklatıp **özellikleri**.
7. Seçin **aşağıdaki DNS sunucu adreslerini kullan** ve adres alanındaki birincil etki alanı denetleyicisi belirtin **tercih edilen DNS sunucusu**.
8. Tıklatın **Tamam**ve ardından **Kapat** değişiklikleri kaydedilemedi. Şimdi VM katılamayacak **corp.contoso.com**.

   >[!IMPORTANT]
   >DNS ayarı değiştirdikten sonra Uzak Masaüstü bağlantısı kesildiğinde, Azure Portalı'na gidin ve sanal makineyi yeniden başlatın.

9. İkincil etki alanı denetleyicisine uzak masaüstünden açmak **Sunucu Yöneticisi Panosu**.
10. Tıklatın **rol ve Özellik Ekle** Panoda bağlantı.

    ![Sunucu Yöneticisi - rolleri Ekle](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/22-addfeatures.png)
11. Seçin **sonraki** için elde edene kadar **sunucu rolleri** bölümü.
12. Seçin **Active Directory etki alanı Hizmetleri** ve **DNS sunucusu** rolleri. İstendiğinde, bu roller için gerekli olan diğer ek özellikleri ekleyin.
13. Özellikleri yükleme işlemini tamamladıktan sonra geri dönüp **Sunucu Yöneticisi'ni** Pano.
14. Yeni **AD DS** sol bölmesinden seçeneği.
15. Tıklatın **daha fazla** sarı uyarı çubuğundaki bağlantı.
16. İçinde **eylem** sütunu **tüm sunucu görev ayrıntıları** iletişim kutusunda, tıklatın **bu sunucu bir etki alanı denetleyicisine Yükselt**.
17. Altında **Dağıtım Yapılandırması**seçin **mevcut bir etki alanına bir etki alanı denetleyicisi eklemek**.
   ![Dağıtım Yapılandırması](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/28-deploymentconfig.png)
18. **Seç**'e tıklayın.
19. Yönetici hesabı kullanarak bağlanmak (**CORP. CONTOSO.COM\domainadmin**) ve parolayı (**Contoso! 0000**).
20. İçinde **ormandan bir etki alanı seçin**etki alanınızı tıklatın ve ardından **Tamam**.
21. İçinde **etki alanı denetleyicisi seçenekleri**, varsayılan değerleri kullanın ve DSRM parolasını ayarlayın.

   >[!NOTE]
   >**DNS seçenekleri** sayfa uyar, bu DNS sunucusu için bir temsilci oluşturulamıyor. Üretim dışı ortamlarında bu uyarıyı yoksayabilirsiniz.
22. Tıklatın **sonraki** iletişim kutusu erişene kadar **Önkoşullar** denetleyin. Ardından **Yükle**'ye tıklayın.

Yapılandırma değişiklikleri tamamlandıktan sonra sunucu, sunucuyu yeniden başlatın.

### <a name="add-the-private-ip-address-to-the-second-domain-controller-to-the-vpn-dns-server"></a>VPN DNS sunucusuna ikinci etki alanı denetleyicisine özel IP adresi Ekle

Azure portalında, sanal ağ, DNS sunucusu ikincil etki alanı denetleyicisinin IP adresi eklemek için değiştirin. Bu ayar, DNS hizmeti artıklık sağlar.

### <a name=DomainAccounts></a> Etki alanı hesaplarını yapılandırma

Sonraki adımlarda Active Directory hesaplarını yapılandırın. Aşağıdaki tabloda, hesapların gösterilmektedir:

| |Yükleme hesabı<br/> |sqlserver-0 <br/>SQL Server ve SQL aracısı hizmet hesabı |sqlserver-1<br/>SQL Server ve SQL aracısı hizmet hesabı
| --- | --- | --- | ---
|**İlk adı** |Yükleme |SQLSvc1 | SQLSvc2
|**Kullanıcı SamAccountName** |Yükleme |SQLSvc1 | SQLSvc2

Her hesap oluşturmak için aşağıdaki adımları kullanın.

1. Oturum **ad birincil dc** makine.
2. İçinde **Sunucu Yöneticisi'ni**seçin **Araçları**ve ardından **Active Directory Yönetim Merkezi'ni**.   
3. Seçin **corp (yerel)** sol bölmeden.
4. Sağdaki **görevleri** bölmesinde, **yeni**ve ardından **kullanıcı**.
   ![Active Directory Yönetim Merkezi](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/29-addcnewuser.png)

   >[!TIP]
   >Her hesap için karmaşık bir parola ayarlayın.<br/> Üretim dışı ortamlar için kullanıcı hesabını süresi dolmayacak şekilde ayarlayın.

5. Tıklatın **Tamam** kullanıcı oluşturmak için.
6. Her üç hesapları için önceki adımları yineleyin.

### <a name="grant-the-required-permissions-to-the-installation-account"></a>Yükleme hesabı için gerekli izinleri verin
1. İçinde **Active Directory Yönetim Merkezi'ni**seçin **corp (yerel)** sol bölmede. Ardından sağ **görevleri** bölmesinde tıklatın **özellikleri**.

    ![CORP kullanıcı özellikleri](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/31-addcproperties.png)
2. Seçin **uzantıları**ve ardından **Gelişmiş** düğmesini **güvenlik** sekmesi.
3. İçinde **corp için Gelişmiş güvenlik ayarları** iletişim kutusunda, tıklatın **Ekle**.
4. Tıklatın **sorumlu Seç**, arama **CORP\Install**ve ardından **Tamam**.
5. Seçin **tüm özellikleri oku** onay kutusu.

6. Seçin **bilgisayar nesneleri oluşturma** onay kutusu.

     ![Corp kullanıcı izinleri](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/33-addpermissions.png)
7. Tıklatın **Tamam**ve ardından **Tamam** yeniden. Kapat **corp** Özellikler penceresi.

Active Directory ve kullanıcı nesneleri yapılandırma bitirdikten sonra iki SQL Server VM'ler ve Tanık VM oluşturun. Ardından üç etki alanına katılın.

## <a name="create-sql-server-vms"></a>SQL Server Vm'lerinin oluşturma

Üç ek sanal makineler oluşturun. Çözüm, SQL Server örnekleri iki sanal makinelerle gerektirir. Üçüncü sanal makineye bir tanığı olarak çalışır. Windows Server 2016 kullanabileceğiniz bir [bulut Tanık](http://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness), ancak bu belgenin önceki işletim sistemleri ile tutarlılığını tanığı için bir sanal makine kullanır.  

Devam etmeden önce aşağıdaki tasarım kararlarını göz önünde bulundurun.

* **Depolama - Azure tarafından yönetilen disklerde**

   Sanal makine depolama için Azure yönetilen diskleri kullanın. Microsoft SQL Server sanal makineler için yönetilen diskleri önerir. Yönetilen Diskler, depolama alanını arka planda yönetir. Ayrıca, Yönetilen Disklere sahip sanal makineler aynı kullanılabilirlik kümesinde olduğunda Azure uygun artıklık düzeyini sağlamak için depolama kaynaklarını dağıtır. Daha fazla bilgi için bkz. [Azure Yönetilen Disklere Genel Bakış](../managed-disks-overview.md). Bir kullanılabilirlik kümesinde yönetilen diskler hakkında daha fazla ayrıntı için bkz: [bir kullanılabilirlik kümesindeki sanal makineleri için kullanım yönetilen diskleri](../manage-availability.md#use-managed-disks-for-vms-in-an-availability-set).

* **Ağ - üretimde özel IP adresleri**

   Sanal makineler için Bu öğretici ortak IP adreslerini kullanır. Bir ortak IP adresi internet üzerinden doğrudan sanal makineye uzak bağlantı sağlar - yapılandırma adımlarını kolaylaştırır. Üretim ortamlarında, Microsoft SQL Server örneğinin VM kaynak güvenlik açığı ayak izini azaltmak için yalnızca özel IP adresleri önerir.

### <a name="create-and-configure-the-sql-server-vms"></a>Oluşturma ve SQL Server Vm'lerinin yapılandırma
Ardından, üç sanal makineleri--iki SQL Server VM'ler ve ek bir küme düğümüne için bir VM oluşturun. VM'lerin her birinde oluşturmak için geri dönüp **SQL-HA-RG** kaynak grubu tıklatın **Ekle**, arama için uygun galeri öğesi,'ı tıklatın **sanal makine**ve 'ıtıklatın **Galeriden**. VM'ler oluşturmanıza yardımcı olması için aşağıdaki tablodaki bilgileri kullanın:


| Sayfa | VM1 | VM2 | VM3 |
| --- | --- | --- | --- |
| Uygun galeri öğesini seçin |**Windows Server 2016 Datacenter** |**Windows Server 2016 SQL Server 2016 SP1 Enterprise** |**Windows Server 2016 SQL Server 2016 SP1 Enterprise** |
| Sanal Makine Yapılandırması **temelleri** |**Ad** küme fsw =<br/>**Kullanıcı adı** DomainAdmin =<br/>**Parola** Contoso =! 0000<br/>**Abonelik** aboneliğinizi =<br/>**Kaynak grubu** SQL-HA-RG =<br/>**Konum** azure konumunuz = |**Ad** sqlserver-0 =<br/>**Kullanıcı adı** DomainAdmin =<br/>**Parola** Contoso =! 0000<br/>**Abonelik** aboneliğinizi =<br/>**Kaynak grubu** SQL-HA-RG =<br/>**Konum** azure konumunuz = |**Ad** sqlserver-1 =<br/>**Kullanıcı adı** DomainAdmin =<br/>**Parola** Contoso =! 0000<br/>**Abonelik** aboneliğinizi =<br/>**Kaynak grubu** SQL-HA-RG =<br/>**Konum** azure konumunuz = |
| Sanal Makine Yapılandırması **boyutu** |**SIZE** = DS1\_V2 (1 vCPU, 3.5 GB) |**SIZE** = DS2\_V2 (2 vCPUs, 7 GB)</br>Boyutu SSD depolama (Premium disk desteği. desteklemesi gerekir )) |**SIZE** = DS2\_V2 (2 vCPUs, 7 GB) |
| Sanal Makine Yapılandırması **ayarları** |**Depolama**: yönetilen diskleri kullanın.<br/>**Sanal ağ** autoHAVNET =<br/>**Alt ağ** sqlsubnet(10.1.1.0/24) =<br/>**Genel IP adresi** otomatik olarak oluşturulan.<br/>**Ağ güvenlik grubu** = yok<br/>**Tanılama izleme** = etkin<br/>**Tanılama depolama hesabı** otomatik olarak oluşturulan depolama hesabı kullan =<br/>**Kullanılabilirlik kümesi** sqlAvailabilitySet =<br/> |**Depolama**: yönetilen diskleri kullanın.<br/>**Sanal ağ** autoHAVNET =<br/>**Alt ağ** sqlsubnet(10.1.1.0/24) =<br/>**Genel IP adresi** otomatik olarak oluşturulan.<br/>**Ağ güvenlik grubu** = yok<br/>**Tanılama izleme** = etkin<br/>**Tanılama depolama hesabı** otomatik olarak oluşturulan depolama hesabı kullan =<br/>**Kullanılabilirlik kümesi** sqlAvailabilitySet =<br/> |**Depolama**: yönetilen diskleri kullanın.<br/>**Sanal ağ** autoHAVNET =<br/>**Alt ağ** sqlsubnet(10.1.1.0/24) =<br/>**Genel IP adresi** otomatik olarak oluşturulan.<br/>**Ağ güvenlik grubu** = yok<br/>**Tanılama izleme** = etkin<br/>**Tanılama depolama hesabı** otomatik olarak oluşturulan depolama hesabı kullan =<br/>**Kullanılabilirlik kümesi** sqlAvailabilitySet =<br/> |
| Sanal Makine Yapılandırması **SQL Server ayarları** |Uygulanamaz |**SQL Bağlantısı** özel (sanal ağ dahilinde) =<br/>**Bağlantı noktası** 1433 =<br/>**SQL kimlik doğrulaması** = devre dışı bırak<br/>**Depolama yapılandırması** genel =<br/>**Otomatik düzeltme eki uygulama** 2: 00'dan Pazar =<br/>**Otomatik yedekleme** = devre dışı</br>**Azure anahtar kasası tümleştirme** = devre dışı |**SQL Bağlantısı** özel (sanal ağ dahilinde) =<br/>**Bağlantı noktası** 1433 =<br/>**SQL kimlik doğrulaması** = devre dışı bırak<br/>**Depolama yapılandırması** genel =<br/>**Otomatik düzeltme eki uygulama** 2: 00'dan Pazar =<br/>**Otomatik yedekleme** = devre dışı</br>**Azure anahtar kasası tümleştirme** = devre dışı |

<br/>

> [!NOTE]
> Burada önerilen makine boyutlarını kullanılabilirlik grupları Azure Vm'lerde test etmek için yöneliktir. Üretim iş yükleri üzerinde en iyi performans için SQL Server makine boyutlarına ve yapılandırmada önerilerini görmek [performans Azure sanal makinelerde SQL Server için en iyi uygulamaları](virtual-machines-windows-sql-performance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
>
>

Bunlara katılmak gereken üç VM'ler tam olarak sağlandıktan sonra **corp.contoso.com** makinelere, CORP\Install yönetici hakları etki alanı ve verin.

### <a name="joinDomain"></a>Sunucusunu etki alanına katma

Şimdi Vm'lere katılamayacak **corp.contoso.com**. SQL Server Vm'lerinin ve dosya paylaşımı tanığı sunucusu için aşağıdaki adımları uygulayın:

1. Sanal makineyle uzaktan bağlanmak **BUILTIN\DomainAdmin**.
2. İçinde **Sunucu Yöneticisi'ni**, tıklatın **yerel sunucu**.
3. Tıklatın **çalışma grubu** bağlantı.
4. İçinde **bilgisayar adı** 'yi tıklatın **değişiklik**.
5. Seçin **etki alanı** onay kutusunu ve türü **corp.contoso.com** metin kutusuna. **Tamam**’a tıklayın.
6. İçinde **Windows Güvenliği** açılan iletişim kutusunda, varsayılan etki alanı yönetici hesabının kimlik bilgilerini belirtin (**CORP\DomainAdmin**) ve parolayı (**Contoso! 0000**) .
7. "Corp.contoso.com etki alanına Hoş Geldiniz" iletisini gördüğünüzde, tıklatın **Tamam**.
8. Tıklatın **Kapat**ve ardından **şimdi yeniden Başlat** açılan iletişim kutusunda.

### <a name="add-the-corpinstall-user-as-an-administrator-on-each-cluster-vm"></a>Her küme VM üzerinde bir yönetici olarak Corp\Install kullanıcı ekleme

Her bir sanal makine etki alanının bir üyesi olarak yeniden başlatıldıktan sonra add **CORP\Install** yerel Yöneticiler grubunun bir üyesi olarak.

1. VM yeniden başlatılana kadar bekleyin ve yeniden oturum açmak için birincil etki alanı denetleyicisinden RDP dosyasını başlatma **sqlserver-0** kullanarak **CORP\DomainAdmin** hesabı.
   >[!TIP]
   >Etki alanı yönetici hesabıyla oturum emin olun. Önceki adımlarda IN YERLEŞİK yönetici hesabını kullanarak. Sunucu etki alanında olduğundan, etki alanı hesabını kullanın. RDP oturumunuzda belirtin *etki alanı*\\*kullanıcıadı*.

2. İçinde **Sunucu Yöneticisi'ni**seçin **Araçları**ve ardından **Bilgisayar Yönetimi**.
3. İçinde **Bilgisayar Yönetimi** penceresinde genişletin **yerel kullanıcılar ve gruplar**ve ardından **grupları**.
4. Çift **Yöneticiler** grubu.
5. İçinde **Yöneticiler özellikleri** iletişim kutusunda, tıklatın **Ekle** düğmesi.
6. Kullanıcının girmesi **CORP\Install**ve ardından **Tamam**.
7. Tıklatın **Tamam** kapatmak için **yönetici özellikleri** iletişim.
8. Önceki adımları tekrarlayın **sqlserver-1** ve **küme fsw**.

### <a name="setServiceAccount"></a>SQL Server hizmet hesapları

Her SQL Server VM üzerinde SQL Server hizmet hesabı ayarlayın. Ne zaman oluşturulan hesapların, [etki alanı hesapları yapılandırılmış](#DomainAccounts).

1. Açık **SQL Server Configuration Manager**.
2. SQL Server hizmetini sağ tıklatın ve ardından **özellikleri**.
3. Hesap ve parola ayarlayın.
4. Bir SQL Server VM üzerinde bu adımları yineleyin.  

SQL Server kullanılabilirlik grupları için her SQL Server VM bir etki alanı hesabı olarak çalıştırılması gerekir.

### <a name="create-a-sign-in-on-each-sql-server-vm-for-the-installation-account"></a>Bir oturum açma yükleme hesabı için her bir SQL Server VM oluşturma

Kullanılabilirlik grubu yapılandırmak için yükleme hesabı'nı (CORP\install) kullanın. Bu hesap bir üyesi olması gerekiyor **sysadmin** her SQL Server VM üzerinde sabit sunucu rolü. Bir oturum açma yükleme hesabı için aşağıdaki adımları oluşturun:

1. Uzak Masaüstü Protokolü (RDP) aracılığıyla sunucuya kullanarak bağlanmak  *\<MachineName\>\DomainAdmin* hesabı.

1. SQL Server Management Studio'yu açın ve yerel SQL Server örneğine bağlanın.

1. İçinde **Object Explorer**, tıklatın **güvenlik**.

1. Sağ **oturumları**. Tıklatın **yeni oturum açma**.

1. İçinde **yeni oturum açma -**, tıklatın **arama**.

1. Tıklatın **konumları**.

1. Etki alanı yönetici ağ kimlik bilgilerini girin.

1. Yükleme hesabı'nı kullanın.

1. Oturum-üyesi olacak şekilde ayarlanan **sysadmin** sabit sunucu rolü.

1. **Tamam**’a tıklayın.

Bir SQL Server VM üzerinde önceki adımları yineleyin.

## <a name="add-failover-clustering-features-to-both-sql-server-vms"></a>Her iki SQL Server Vm'lerinin yük devretme kümeleme özellikleri ekleyin

Yük devretme kümeleme özellikleri eklemek için her iki SQL Server VM üzerinde aşağıdaki adımları uygulayın:

1. Kullanarak SQL Server sanal makineye Uzak Masaüstü Protokolü (RDP) üzerinden bağlanan *CORP\install* hesabı. Açık **Sunucu Yöneticisi Panosu**.
2. Tıklatın **rol ve Özellik Ekle** Panoda bağlantı.

    ![Sunucu Yöneticisi - rolleri Ekle](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/22-addfeatures.png)
3. Seçin **sonraki** için elde edene kadar **Server özellikleri** bölümü.
4. İçinde **özellikleri**seçin **Yük Devretme Kümelemesi**.
5. Gerekli diğer ek özellikleri ekleyin.
6. Tıklatın **yükleme** özellikleri eklemek için.

Bir SQL Server VM adımları yineleyin.

## <a name="a-nameendpoint-firewall-configure-the-firewall-on-each-sql-server-vm"></a><a name="endpoint-firewall"> Her SQL Server VM Güvenlik duvarını yapılandırma

Çözüm, Güvenlik Duvarı'nda açık olması için aşağıdaki TCP bağlantı noktalarını gerektirir:

- **SQL Server VM**:<br/>
   Bağlantı noktası 1433'ü SQL Server'ın varsayılan bir örnek için.
- **Azure yük dengeleyici araştırmasını:**<br/>
   Tüm kullanılabilir bağlantı noktası. Örnekler sık 59999 kullanın.
- **Veritabanı yansıtma uç noktası:** <br/>
   Tüm kullanılabilir bağlantı noktası. Örnekler sık 5022 kullanın.

Güvenlik Duvarı bağlantı noktalarının her iki SQL Server VM üzerinde açık olması gerekir.

Bağlantı noktaları açma yöntemi, güvenlik duvarı çözüm bağlıdır. Sonraki bölümde, Windows Güvenlik Duvarı'nda bağlantı noktalarını açmak açıklanmaktadır. Her SQL Server Vm'lerinin gerekli bağlantı noktalarını açın.

### <a name="open-a-tcp-port-in-the-firewall"></a>Güvenlik Duvarı'nda bir TCP bağlantı noktasını açın

1. İlk SQL Server'da **Başlat** ekranında, başlatma **Gelişmiş Güvenlik Özellikli Windows Güvenlik Duvarı**.
2. Sol bölmede seçin **gelen kuralları**. Sağ bölmede, tıklatın **yeni kural**.
3. İçin **kural türü**, seçin **bağlantı noktası**.
4. Bağlantı noktasını belirtin **TCP** ve uygun bağlantı noktası numaralarını yazın. Aşağıdaki örneğe bakın:

   ![SQL firewall](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/35-tcpports.png)

5. **İleri**’ye tıklayın.
6. Üzerinde **eylem** sayfasında, tutmak **bağlantıya izin** seçili ve ardından **sonraki**.
7. Üzerinde **profil** sayfasında, varsayılan ayarları kabul edin ve ardından **sonraki**.
8. Üzerinde **adı** sayfasında, bir kural adı belirtin (gibi **Azure LB araştırma**) içinde **adı** metin kutusuna ve ardından **son**.

İkinci SQL Server VM üzerinde bu adımları yineleyin.

## <a name="configure-system-account-permissions"></a>System hesabının izinlerini yapılandırma

Sistem hesabı için bir hesap oluşturun ve uygun izinleri vermek için her SQL Server örneği için aşağıdaki adımları tamamlayın:

1. İçin bir hesap oluşturma `[NT AUTHORITY\SYSTEM]` her SQL Server örneğinde. Aşağıdaki komut dosyası bu hesabı oluşturur:

   ```sql
   USE [master]
   GO
   CREATE LOGIN [NT AUTHORITY\SYSTEM] FROM WINDOWS WITH DEFAULT_DATABASE=[master]
   GO 
   ```

1. Aşağıdaki izinleri `[NT AUTHORITY\SYSTEM]` her SQL Server örneği:

   - `ALTER ANY AVAILABILITY GROUP`
   - `CONNECT SQL`
   - `VIEW SERVER STATE`

   Aşağıdaki komut dosyası bu izinleri verir:

   ```sql
   GRANT ALTER ANY AVAILABILITY GROUP TO [NT AUTHORITY\SYSTEM]
   GO
   GRANT CONNECT SQL TO [NT AUTHORITY\SYSTEM]
   GO
   GRANT VIEW SERVER STATE TO [NT AUTHORITY\SYSTEM]
   GO 
   ```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure sanal makinelerde bir SQL Server Always On kullanılabilirlik grubu oluşturma](virtual-machines-windows-portal-sql-availability-group-tutorial.md)
