---
title: "Always On kullanılabilirlik grubu Azure sanal makineleri (Klasik) yapılandırma | Microsoft Docs"
description: "Always On kullanılabilirlik grubu ile Azure sanal makine oluşturun. Bu öğretici öncelikle kullanıcı arabirimi ve araçları kullanır komut dosyası yerine."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 8d48a3d2-79f8-4ab0-9327-2f30ee0b2ff1
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/17/2017
ms.author: mikeray
ms.openlocfilehash: b360fe9f28eeb9b10c82fce729165b1b572ac3c6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configure-always-on-availability-group-in-azure-virtual-machines-classic"></a>Always On kullanılabilirlik grubu Azure sanal makineleri (Klasik) yapılandırma
> [!div class="op_single_selector"]
> * [Klasik: kullanıcı Arabirimi](../classic/portal-sql-alwayson-availability-groups.md)
> * [Klasik: PowerShell](../classic/ps-sql-alwayson-availability-groups.md)
<br/>

Başlamadan önce Azure Resource Manager modelinde bu görevi tamamlamak olduğunu göz önünde bulundurun. Yeni dağıtımlar için Azure Resource Manager modeli öneririz. Bkz: [SQL Server Always On kullanılabilirlik grupları'Azure sanal makinelerinde](../sql/virtual-machines-windows-portal-sql-availability-group-overview.md).

> [!IMPORTANT] 
> Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Azure oluşturmak ve bu kaynaklarla çalışmak için iki farklı dağıtım modeline sahiptir: [Resource Manager ve klasik](../../../azure-resource-manager/resource-manager-deployment-model.md). Bu makale, Klasik dağıtım modeli kullanımı açıklanmaktadır. 

Azure Resource Manager modeli Bu görevi tamamlamak için bkz: [SQL Server Always On kullanılabilirlik grupları'Azure sanal makinelerinde](../sql/virtual-machines-windows-portal-sql-availability-group-overview.md).

Bu uçtan uca öğretici, SQL Server her zaman Azure sanal makinelerde çalışan kullanarak kullanılabilirlik grupları uygulama gösterir.

Öğreticinin sonunda Azure, SQL Server Always On çözümünüz aşağıdaki öğelerden oluşur:

* Birden çok alt kümeler içeriyor ve bir ön uç ve arka uç alt ağ içeren bir sanal ağ
* Bir Active Directory (Azure AD) etki alanı ile bir etki alanı denetleyicisi
* SQL Server çalıştıran ve arka uç alt ağ için dağıtılmış ve Azure AD etki alanına katılmış iki sanal makineler
* Düğüm çoğunluğu çekirdek modeli ile üç düğümlü yük devretme kümesi
* Bir kullanılabilirlik veritabanının iki synchronous-commit çoğaltmalarından sahip bir kullanılabilirlik grubu

Aşağıdaki çizimde, çözüm grafik gösterimidir.

![Test laboratuvarı mimarisi Azure içindeki kullanılabilirlik grupları](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC791912.png)

Bu bir olası yapılandırma olduğuna dikkat edin. Örneğin, iki çoğaltma kullanılabilirlik grubu için sanal makine sayısını en aza indirebilirsiniz. İki düğümlü bir küme içinde çekirdek dosya paylaşım tanığı olarak etki alanı denetleyicisini kullanarak Azure işlem saatlerinde bu yapılandırmayı kaydeder. Bu yöntem, tek Resimli yapılandırmasından sanal makine sayısı azaltır.

Bu öğretici aşağıdaki varsayılır:

* Bir Azure hesabı zaten var.
* SQL Server çalıştıran bir Klasik sanal makine sağlamak için sanal makine Galerisi'nde GUI'yi kullanmak nasıl zaten biliyor.
* Always On kullanılabilirlik grupları düz bir anlayış zaten var. Daha fazla bilgi için bkz: [Always On kullanılabilirlik grupları (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).

> [!NOTE]
> SharePoint ile Always On kullanılabilirlik grupları kullanarak ilgileniyorsanız, ayrıca bkz. [yapılandırmak SQL Server 2012 Always On kullanılabilirlik grupları SharePoint 2013 için](https://technet.microsoft.com/library/jj715261.aspx).
> 
> 

## <a name="create-the-virtual-network-and-domain-controller-server"></a>Sanal ağ ve etki alanı denetleyicisi sunucu oluşturma
Yeni bir Azure deneme sürümü hesabı ile başlar. Hesabınızı ayarladıktan sonra Klasik Azure portalı giriş ekranında olması gerekir.

1. Tıklatın **yeni** aşağıdaki ekran görüntüsünde gösterildiği gibi sayfanın sol köşesinde düğmesine tıklayın.
   
    ![Portal'da Yeni'ye tıklayın](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665511.gif)
2. Tıklatın **Ağ Hizmetleri** > **sanal ağ** > **özel Oluştur**aşağıdaki ekran görüntüsünde gösterildiği gibi.
   
    ![Sanal ağ oluşturma](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665512.gif)
3. İçinde **bir sanal ağ oluştur** iletişim kutusunda, sayfalar arasında atlama ve aşağıdaki tabloda ayarları kullanarak yeni bir sanal ağ oluşturun. 
   
   | Sayfa | Ayarlar |
   | --- | --- |
   | Sanal ağ ayrıntıları |**ADI ContosoNET =**<br/>**Bölge Batı ABD =** |
   | DNS sunucuları ve VPN bağlantısı |None |
   | Sanal ağ adres alanları |Ayarları aşağıdaki ekran görüntüsünde gösterilmiştir: ![Sanal ağ oluşturma](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784620.png) |
4. Etki alanı denetleyicisi (DC) olarak kullanacağınız sanal makine oluşturun. Tıklatın **yeni** > **işlem** > **sanal makine** > **Galeri'den**aşağıdaki ekran görüntüsünde gösterildiği gibi.
   
    ![Sanal makine oluşturma](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784621.png)
5. İçinde **bir sanal makine oluşturma** iletişim kutusunda, yeni bir sanal makine sayfalarıyla atlama ve ayarları aşağıdaki tabloda kullanarak yapılandırın. 
   
   | Sayfa | Ayarlar |
   | --- | --- |
   | Sanal makine işletim sistemini seçin |Windows Server 2012 R2 Datacenter |
   | Sanal Makine Yapılandırması |**Sürüm yayın tarihi** (son) =<br/>**SANAL makine adı** ContosoDC =<br/>**KATMAN** STANDART =<br/>**BOYUTU** A2 = (2 Çekirdek)<br/>**Yeni bir kullanıcı adı** AzureAdmin =<br/>**Yeni parola** Contoso =! 000<br/>**Onayla** Contoso =! 000 |
   | Sanal Makine Yapılandırması |**Bulut hizmeti** = yeni bir bulut hizmeti oluşturun<br/>**Bulut hizmeti DNS adı** benzersiz bulut hizmeti adı =<br/>**DNS adı** benzersiz bir ad = (örn: ContosoDC123)<br/>**Bölge/BENZEŞİM grubu/sanal ağ** ContosoNET =<br/>**SANAL ağ alt ağları** Back(10.10.2.0/24) =<br/>**Depolama hesabı** otomatik olarak oluşturulan depolama hesabı kullan =<br/>**Kullanılabilirlik KÜMESİ** = (yok) |
   | Sanal makine seçenekleri |Varsayılanları kullanın |

Yeni bir sanal makine yapılandırdıktan sonra sanal makine provsioned olmasını bekleyin. Bu işlemin tamamlanması biraz zaman alır. Tıklatırsanız **sanal makine** sekmesini Klasik Azure portalında ContosoDC dönüşüm durumlar görebilirsiniz **başlangıç (hazırlama)** için **durduruldu**, **başlangıç**, **çalışan (hazırlama)**ve son olarak **çalıştıran**.

DC sunucusuna şimdi başarıyla kaynak sağlandı. Ardından, bu DC sunucu üzerinde Active Directory etki alanı yapılandırır.

## <a name="configure-the-domain-controller"></a>Etki alanı denetleyicisini Yapılandır
Aşağıdaki adımlarda, corp.contoso.com etki alanı denetleyicisi olarak ContosoDC makine yapılandırın.

1. Portalı'nda seçin **ContosoDC** makine. Üzerinde **Pano** sekmesini tıklatın, **Bağlan** Uzak Masaüstü erişimi için bir Uzak Masaüstü (RDP) dosyası açılamıyor.
   
    ![Vritual makineye bağlanma](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784622.png)
2. Yapılandırılmış Yönetici hesabınızla oturum açın (**\AzureAdmin**) ve parolayı (**Contoso! 000**).
3. Varsayılan olarak, **Sunucu Yöneticisi'ni** Pano görüntülenmesi.
4. Tıklatın **rol ve Özellik Ekle** Panoda bağlantı.
   
    ![Sunucu Gezgini Rol Ekle](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784623.png)
5. Tıklatın **sonraki** için elde edene kadar **sunucu rolleri** bölümü.
6. Seçin **Active Directory etki alanı Hizmetleri** ve **DNS sunucusu** rolleri. İstendiğinde, bu rolleri gerektiren daha fazla özellikleri ekleyin.
   
   > [!NOTE]
   > Statik bir IP adresi olduğunu uyarı doğrulama alırsınız. Yapılandırmayı test ediyorsanız, tıklatın **devam**. Üretim senaryoları için [etki alanı denetleyicisi makinenin statik IP adresi ayarlamak için PowerShell kullanın](../../../virtual-network/virtual-networks-reserved-private-ip.md).
   > 
   > 
   
    ![Rolleri iletişim ekleyin](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784624.png)
7. Tıklatın **sonraki** ulaşana kadar **onay** bölümü. Seçin **gerekirse hedef sunucuyu otomatik olarak yeniden** onay kutusu.
8. **Yükle**'ye tıklayın.
9. Özellik yüklendikten sonra geri dönüp **Sunucu Yöneticisi'ni** Pano.
10. Yeni **AD DS** sol bölmede seçeneği.
11. Tıklatın **daha fazla** sarı uyarı çubuğundaki bağlantı.
    
     ![DNS sunucusu sanal makinesinde AD DS iletişim kutusu](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784625.png)
12. İçinde **eylem** sütunu **tüm sunucu görev ayrıntıları** iletişim kutusu, tıklatın **bu sunucu bir etki alanı denetleyicisine Yükselt**.
13. İçinde **Active Directory etki alanı Hizmetleri Yapılandırma Sihirbazı**, aşağıdaki değerleri kullanın:
    
    | Sayfa | Ayar |
    | --- | --- |
    | Dağıtım Yapılandırması |**Yeni orman Ekle** seçili =<br/>**Kök etki alanı adı** corp.contoso.com = |
    | Etki alanı denetleyicisi seçenekleri |**Parola** Contoso =! 000<br/>**Parolayı onaylayın** Contoso =! 000 |
14. Tıklatın **sonraki** bir sihirbaz sayfalarında gitmesi. Üzerinde **Önkoşul denetimi** sayfasında, aşağıdaki iletiyi görürsünüz doğrulayın: **tüm önkoşul denetimlerinden başarıyla geçti**. Tüm ilgili uyarı iletileri gözden geçirmeniz gerekir, ancak yükleme işlemine devam etmek mümkündür unutmayın.
15. **Yükle**'ye tıklayın. **ContosoDC** sanal makine otomatik olarak yeniden.

## <a name="configure-domain-accounts"></a>Etki alanı hesaplarını yapılandırma
Sonraki adımlar, daha sonra kullanmak için Active Directory hesaplarını yapılandırın.

1. İçinde oturum dön **ContosoDC** makine.
2. İçinde **Sunucu Yöneticisi'ni**, tıklatın **Araçları** > **Active Directory Yönetim Merkezi'ni**.
   
    ![Active Directory Yönetim Merkezi](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784626.png)
3. İçinde **Active Directory Yönetim Merkezi'ni**seçin **corp (yerel)** sol bölmede.
4. Sağdaki **görevleri** bölmesinde tıklatın **yeni** > **kullanıcı**. Aşağıdaki ayarları kullanın:
   
   | Ayar | Değer |
   | --- | --- |
   | **İlk adı** |Yükleme |
   | **Kullanıcı SamAccountName** |Yükleme |
   | **Parola** |Contoso! 000 |
   | **Parolayı onayla** |Contoso! 000 |
   | **Diğer parola seçenekleri** |Seçildi |
   | **Parola her zaman geçerli olsun** |İşaretli |
5. Tıklatın **Tamam** oluşturmak için **yükleme** kullanıcı. Bu hesap, yük devretme kümesinin ve kullanılabilirlik grubu yapılandırmak için kullanılır.
6. İki ek kullanıcılar oluşturma **CORP\SQLSvc1** ve **CORP\SQLSvc2**, aynı adımları. Bu hesaplar için SQL Server örnekleri kullanılır. Ardından, vermeniz gerekir **CORP\Install** Windows Yük Devretme Kümelemesi yapılandırmak için gerekli izinleri.
7. İçinde **Active Directory Yönetim Merkezi'ni**, tıklatın **corp (yerel)** sol bölmede. İçinde **görevleri** bölmesinde tıklatın **özellikleri**.
   
    ![CORP kullanıcı özellikleri](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784627.png)
8. Seçin **uzantıları**ve ardından **Gelişmiş** düğmesini **güvenlik** sekmesi.
9. İçinde **corp için Gelişmiş güvenlik ayarları** iletişim kutusu, tıklatın **Ekle**.
10. Tıklatın **sorumlu Seç**, arama **CORP\Install**ve ardından **Tamam**.
11. Seçin **tüm özellikleri oku** ve **bilgisayar nesneleri oluşturma** izinleri.
    
     ![Corp kullanıcı izinleri](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784628.png)
12. Tıklatın **Tamam**ve ardından **Tamam** yeniden. Corp Özellikler penceresini kapatın.

Active Directory ve kullanıcı nesnelerini yapılandırılmış, üç SQL Server sanal makine oluşturacak ve bu etki alanına ekleyin.

## <a name="create-the-sql-server-virtual-machines"></a>SQL Server sanal makine oluşturma
Üç sanal makine oluşturun. Bir küme düğümü için biridir ve SQL Server için ikisidir. Her sanal makine oluşturmak için Klasik Azure portalına geri dönün, **yeni** > **işlem** > **sanal makine** > **Galeri'den**. Ardından, şablonları aşağıdaki tabloda sanal makineler oluşturmanıza yardımcı olması için kullanın.

| Sayfa | VM1 | VM2 | VM3 |
| --- | --- | --- | --- |
| Sanal makine işletim sistemini seçin |**Windows Server 2012 R2 Datacenter** |**SQL Server 2014 RTM Enterprise** |**SQL Server 2014 RTM Enterprise** |
| Sanal Makine Yapılandırması |**Sürüm yayın tarihi** (son) =<br/>**SANAL makine adı** ContosoWSFCNode =<br/>**KATMAN** STANDART =<br/>**BOYUTU** A2 = (2 Çekirdek)<br/>**Yeni bir kullanıcı adı** AzureAdmin =<br/>**Yeni parola** Contoso =! 000<br/>**Onayla** Contoso =! 000 |**Sürüm yayın tarihi** (son) =<br/>**SANAL makine adı** ContosoSQL1 =<br/>**KATMAN** STANDART =<br/>**BOYUTU** A3 = (4 çekirdek)<br/>**Yeni bir kullanıcı adı** AzureAdmin =<br/>**Yeni parola** Contoso =! 000<br/>**Onayla** Contoso =! 000 |**Sürüm yayın tarihi** (son) =<br/>**SANAL makine adı** ContosoSQL2 =<br/>**KATMAN** STANDART =<br/>**BOYUTU** A3 = (4 çekirdek)<br/>**Yeni bir kullanıcı adı** AzureAdmin =<br/>**Yeni parola** Contoso =! 000<br/>**Onayla** Contoso =! 000 |
| Sanal Makine Yapılandırması |**Bulut hizmeti** önceden oluşturulmuş benzersiz bulut hizmeti DNS adı = (örn: ContosoDC123)<br/>**Bölge/BENZEŞİM grubu/sanal ağ** ContosoNET =<br/>**SANAL ağ alt ağları** Back(10.10.2.0/24) =<br/>**Depolama hesabı** otomatik olarak oluşturulan depolama hesabı kullan =<br/>**Kullanılabilirlik KÜMESİ** = oluşturun bir kullanılabilirlik kümesi<br/>**KULLANILABİLİRLİK KÜMESİ ADI** SQLHADR = |**Bulut hizmeti** önceden oluşturulmuş benzersiz bulut hizmeti DNS adı = (örn: ContosoDC123)<br/>**Bölge/BENZEŞİM grubu/sanal ağ** ContosoNET =<br/>**SANAL ağ alt ağları** Back(10.10.2.0/24) =<br/>**Depolama hesabı** otomatik olarak oluşturulan depolama hesabı kullan =<br/>**Kullanılabilirlik KÜMESİ** (de yapılandırabilirsiniz kullanılabilirlik makine oluşturulduktan sonra kümesini. SQLHADR = Üç makinenin SQLHADR kullanılabilirlik kümesine atanması gerekir.) |**Bulut hizmeti** önceden oluşturulmuş benzersiz bulut hizmeti DNS adı = (örn: ContosoDC123)<br/>**Bölge/BENZEŞİM grubu/sanal ağ** ContosoNET =<br/>**SANAL ağ alt ağları** Back(10.10.2.0/24) =<br/>**Depolama hesabı** otomatik olarak oluşturulan depolama hesabı kullan =<br/>**Kullanılabilirlik KÜMESİ** (de yapılandırabilirsiniz kullanılabilirlik makine oluşturulduktan sonra kümesini. SQLHADR = Üç makinenin SQLHADR kullanılabilirlik kümesine atanması gerekir.) |
| Sanal makine seçenekleri |Varsayılanları kullanın |Varsayılanları kullanın |Varsayılanları kullanın |

<br/>

> [!NOTE]
> Temel katman makinelerde yük dengeli uç noktalar desteklemediği için standart katman sanal makinelerde, önceki yapılandırmayı önerir. Daha sonra bir kullanılabilirlik grubu dinleyicisi oluşturmak için yük dengeli uç noktalar gerekir. Ayrıca, Azure sanal makinelerinde kullanılabilirlik grupları test etmek için burada önerilen makine boyutlarını yöneliktir. Üretim iş yükleri üzerinde en iyi performans için SQL Server makine boyutlarına ve yapılandırmada önerilerini görmek [Azure Virtual Machines'de SQL Server için performans en iyi uygulamaları](../sql/virtual-machines-windows-sql-performance.md).
> 
> 

Bunlara katılmak gereken üç sanal makine tam olarak sağlandıktan sonra **corp.contoso.com** makinelere, CORP\Install yönetici hakları etki alanı ve verin. Bunu yapmak için her üç sanal makine için aşağıdaki adımları kullanın.

1. İlk olarak, tercih edilen DNS sunucusu adresi değiştirin. Sanal makine listesinde seçerek ve tıklatarak yerel dizininize her sanal makinenin RDP dosyasını karşıdan **Bağlan** düğmesi. Bir sanal makine seçmek için herhangi bir yere satırda, ilk hücrenin ancak aşağıdaki ekran görüntüsünde gösterildiği gibi tıklayın.
   
    ![RDP dosyasını indir](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC664953.jpg)
2. Yüklediğiniz ve sanal makine için yapılandırılmış yönetici hesabınızı kullanarak oturum RDP dosyasını açın (**BUILTIN\AzureAdmin**) ve parolayı (**Contoso! 000**).
3. Oturum açtıktan sonra görmelisiniz **Sunucu Yöneticisi'ni** Pano. Tıklatın **yerel sunucu** sol bölmede.
4. Tıklatın **etkin IPv6 DHCP tarafından atanan bir IPv4 adresi** bağlantı.
5. İçinde **ağ bağlantıları** iletişim kutusunda, ağ simgesine tıklayın.
   
    ![Sanal makinenin tercih edilen DNS sunucusunu değiştirin](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784629.png)
6. Komut çubuğunda **bu bağlantının ayarlarını değiştir**. (Pencerenizin boyutuna bağlı olarak, bu komut görmek için çift sağ oka tıklayın gerekebilir).
7. Seçin **Internet Protokolü sürüm 4 (TCP/IPv4)**ve ardından **özellikleri**.
8. Seçin **aşağıdaki DNS sunucu adreslerini kullan** ve ardından belirtin **10.10.2.4** içinde **tercih edilen DNS sunucusu**.
9. **10.10.2.4** 10.10.2.0/24 alt ağdaki bir Azure sanal ağındaki bir sanal makineye atanan adresi adresidir. Bu sanal makine **ContosoDC**. Doğrulamak için **ContosoDC**bilgisayarın IP adresi, kullanım **nslookup contosodc** komut istemi penceresinde aşağıdaki ekran görüntüsünde gösterildiği gibi.
   
    ![Etki alanı denetleyicisi için IP adresini bulmak için NSLOOKUP kullanın](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC664954.jpg)
10. Tıklatın **Tamam** > **Kapat** değişiklikleri kaydedilemedi. Şimdi sanal makineye Katıl **corp.contoso.com**.
11. Geri **yerel sunucu** penceresinde tıklatın **çalışma grubu** bağlantı.
12. İçinde **bilgisayar adı** 'yi tıklatın **değişiklik**.
13. Seçin **etki alanı** onay kutusuna **corp.contoso.com** metin kutusuna ve ardından **Tamam**.
14. İçinde **Windows Güvenliği** iletişim kutusunda, varsayılan etki alanı yönetici hesabının kimlik bilgilerini belirtin (**CORP\AzureAdmin**) ve parolayı (**Contoso! 000**).
15. "Corp.contoso.com etki alanına Hoş Geldiniz" iletisini gördüğünüzde, tıklatın **Tamam**.
16. Tıklatın **Kapat** > **şimdi yeniden Başlat** iletişim kutusunda.

### <a name="add-the-corpinstall-user-as-an-administrator-on-each-virtual-machine"></a>Her sanal makinede yönetici olarak Corp\Install kullanıcı ekleyin
1. Sanal makine yeniden başlatmaları kadar ve ardından açık bekleme RDP dosyasını yeniden kullanarak sanal makinede oturum açmak için **BUILTIN\AzureAdmin** hesabı.
2. İçinde **Sunucu Yöneticisi'ni** tıklatın **Araçları** > **Bilgisayar Yönetimi**.
   
    ![Bilgisayar Yönetimi](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784630.png)
3. İçinde **Bilgisayar Yönetimi** iletişim kutusunda, genişletin **yerel kullanıcılar ve gruplar**ve ardından **grupları**.
4. Çift **Yöneticiler** grubu.
5. İçinde **Yöneticiler özellikleri** iletişim kutusu, tıklatın **Ekle** düğmesi.
6. Kullanıcının girmesi **CORP\Install**ve ardından **Tamam**. Kimlik bilgileri istendiğinde kullanmak **AzureAdmin** ile hesap **Contoso! 000** parola.
7. Tıklatın **Tamam** kapatmak için **yönetici özellikleri** iletişim kutusu.

### <a name="add-the-failover-clustering-feature-to-each-virtual-machine"></a>Her bir sanal makine için Yük Devretme Kümelemesi özelliği Ekle
1. İçinde **Sunucu Yöneticisi'ni** panoyu tıklatın **rol ve Özellik Ekle**.
2. İçinde **Ekle roller ve Özellikler Sihirbazı**, tıklatın **sonraki** için elde edene kadar **özellikleri** sayfası.
3. Seçin **Yük Devretme Kümelemesi**. İstendiğinde, bağımlı diğer özellikleri ekleyin.
   
    ![Sanal makine için Yük Devretme Kümelemesi özelliği Ekle](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784631.png)
4. Tıklatın **sonraki**ve ardından **yükleme** üzerinde **onay** sayfası.
5. Zaman **Yük Devretme Kümelemesi** özellik yükleme tamamlandığında, tıklatın **Kapat**.
6. Sanal makineyi oturum açın.
7. Üç sunucu için bu bölümdeki adımları yineleyin: **ContosoWSFCNode**, **ContosoSQL1**, ve **ContosoSQL2**.

Sanal makineler artık sağlanan SQL Server ve çalışıyor, ancak her SQL Server için varsayılan seçenekleri vardır.

## <a name="create-the-failover-cluster"></a>Yük devretme kümesi oluşturma
Bu bölümde daha sonra oluşturacağınız kullanılabilirlik grubu barındıracak yük devretme kümesi oluşturun. Artık, aşağıdaki her üç sanal makinelerin yük devretme kümesinde kullanacağı yaptığınız:

* Azure'da sanal makine tam olarak sağlanan
* Sanal Makine etki alanına katılmış
* Eklenen **CORP\Install** yerel Yöneticiler grubuna
* Yük Devretme Kümelemesi özelliği eklendi

Yük devretme kümesine katılabilmesi için önce bu tüm sanal makinelerin her ön koşullar verilmiştir.

Ayrıca, Azure sanal ağı şirket içi ağ aynı şekilde davranır değil olduğunu unutmayın. Küme şu sırayla oluşturmanız gerekir:

1. Bir düğümde tek düğümlü bir küme oluşturun (**ContosoSQL1**).
2. Kullanılmayan bir IP adresi küme IP adresine değiştirin (**10.10.2.101**).
3. Küme adı çevrimiçi duruma getirin.
4. Diğer düğümler eklemek (**ContosoSQL2** ve **ContosoWSFCNode**).

Tam küme yapılandırma görevleri tamamlamak için aşağıdaki adımları kullanın.

1. RDP dosyasını açın **ContosoSQL1**ve etki alanı hesabı kullanarak oturum **CORP\Install**.
2. İçinde **Sunucu Yöneticisi'ni** panoyu tıklatın **Araçları** > **yük devretme kümesi Yöneticisi**.
3. Sol bölmesinde, **yük devretme kümesi Yöneticisi**ve ardından **bir küme oluşturmak**aşağıdaki ekran görüntüsünde gösterildiği gibi.
   
    ![Küme oluşturma](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784632.png)
4. Küme oluşturma Sihirbazı'nda sayfalarıyla atlama ve ayarları kullanarak aşağıdaki tabloda tek düğümlü bir küme oluşturun:
   
   | Sayfa | Ayarlar |
   | --- | --- |
   | Başlamadan önce |Varsayılanları kullanın |
   | Sunucuları seçin |Tür **ContosoSQL1** içinde **sunucu adını girin** tıklatıp **Ekle** |
   | Doğrulama uyarısı |Seçin **ı bu küme için Microsoft desteğine gereksiniminiz ve bu nedenle doğrulama testlerini çalıştırmak istemiyorsanız Hayır. Sonraki tıkladığınızda, kümeyi oluşturmaya devam**. |
   | Kümeyi yönetmek için erişim noktası |Tür **Cluster1** içinde **küme adı** |
   | Onay |Depolama alanları kullanmadığınız sürece varsayılan ayarları kullanın. Bu tablodan sonraki uyarı bakın. |
   
   > [!WARNING]
   > Kullanıyorsanız [depolama alanları](https://technet.microsoft.com/library/hh831739), depolama havuzu halinde birden çok disk grupları, temizlemeniz gerekir **tüm uygun depolamayı kümeye eklemek** onay kutusunu **onay** sayfası. Bu seçeneğin işaretini kaldırmayın, kümeleme işlemi sırasında sanal diskleri ayrılacaktır. Depolama alanları kümeden kaldırılmış ve PowerShell kullanarak yeniden kadar sonuç olarak, bunlar ayrıca Disk Yöneticisi'nde veya Explorer görünmez.
   > 
   > 
5. Sol bölmede **yük devretme kümesi Yöneticisi**ve ardından **Cluster1.corp.contoso.com**.
6. Orta bölmede, ekranı aşağı kaydırarak **küme çekirdek kaynakları** bölümünde ve genişletin **Name: Clutser1** ayrıntıları. Her ikisi de görmelisiniz **adı** ve **IP adresi** kaynaklarında **başarısız** durumu. Küme yinelenen bir adresi olan makine kendisi, aynı IP adresine atandığından IP adresi kaynağı çevrimiçi duruma getirilemiyor.
7. Başarısız sağ **IP adresi** kaynak ve ardından **özellikleri**.
   
    ![Küme Özellikleri](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784633.png)
8. Seçin **statik IP adresi**, belirtin **10.10.2.101** içinde **adresi** metin kutusuna ve ardından **Tamam**.
9. İçinde **küme çekirdek kaynakları** bölümünde, sağ **Name: Cluster1**ve ardından **çevrimiçine**. Her iki kaynağın çevrimiçi olana kadar bekleyin. Küme Adı kaynağını çevrimiçi olduğunda, DC sunucusuna sahip yeni bir Active Directory bilgisayar hesabı güncelleştirilir. Bu Active Directory hesabının, kullanılabilirlik grubu kümelenmiş hizmet daha sonra çalıştırmak için kullanılır.
10. Kalan düğümleri kümeye ekleyin. Tarayıcı ağacında sağ **Cluster1.corp.contoso.com**ve ardından **düğüm Ekle**aşağıdaki ekran görüntüsünde gösterildiği gibi.
    
     ![Düğümü kümeye ekleme](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784634.png)
11. İçinde **Düğüm Ekleme Sihirbazı'nı**, tıklatın **sonraki** üzerinde **sunucuları Seç** sayfasında, eklemek **ContosoSQL2** ve **ContosoWSFCNode** sunucu adını yazarak listesine **sunucu adını girin** ve ardından **Ekle**. İşiniz bittiğinde tıklatın **sonraki**.
12. Üzerinde **doğrulama uyarısı** sayfasında, **Hayır**, ancak bir üretim senaryosunda, doğrulama testleri gerçekleştirmeniz gerekir. Ardından **İleri**'ye tıklayın.
13. Üzerinde **onay** sayfasında, **sonraki** düğümleri eklemek için.
    
    > [!WARNING]
    > Kullanıyorsanız [depolama alanları](https://technet.microsoft.com/library/hh831739), depolama havuzu halinde birden çok disk grupları, temizlemeniz gerekir **tüm uygun depolamayı kümeye eklemek** onay kutusu. Bu seçeneğin işaretini kaldırmayın, kümeleme işlemi sırasında sanal diskleri ayrılacaktır. Sonuç olarak, depolama alanları kümeden kaldırılana kadar ayrıca Disk Yöneticisi'nde veya Explorer görünmez ve PowerShell kullanarak yeniden.
    > 
    > 
14. Düğümler kümeye eklendikten sonra tıklatın **son**. Yük Devretme Kümesi Yöneticisi şimdi Göster kümenizi üç düğümü sahiptir ve bunları listesi **düğümleri** kapsayıcı.
15. Uzak Masaüstü oturumu dışında oturum açın.

## <a name="prepare-the-sql-server-instances-for-availability-groups"></a>SQL Server örneklerinin kullanılabilirlik grupları için hazırlama
Bu bölümde, her iki aşağıdakileri **ContosoSQL1** ve **contosoSQL2**:

* Oturum açma ekleme **NT AUTHORITY\SYSTEM** varsayılan SQL Server örneği için gerekli izinlere sahip.
* Ekleme **CORP\Install** varsayılan SQL Server örneğinde sysadmin rolüne olarak.
* SQL Server'ın uzaktan erişim için Güvenlik Duvarı'nı açın.
* Always On kullanılabilirlik grupları özelliğini etkinleştirin.
* SQL Server hizmet hesabını değiştirmek **CORP\SQLSvc1** ve **CORP\SQLSvc2**sırasıyla.

Bunlardan herhangi bir sırada gerçekleştirilebilir. Bununla birlikte, aşağıdaki adımları aralarında sırayla anlatılmaktadır. Her ikisi için adımları **ContosoSQL1** ve **ContosoSQL2**:

1. Sanal makine için Uzak Masaüstü oturumu dışında oturum değil, bunu şimdi yapın.
2. RDP dosyalarını açmak **ContosoSQL1** ve **ContosoSQL2**, olarak oturum açın ve **BUILTIN\AzureAdmin**.
3. Ekleme **NT AUTHORITY\SYSTEM** gerekli izinlere sahip SQL Server oturumları için. **SQL Server Management Studio**’yu açın.
4. Tıklatın **Bağlan** varsayılan SQL Server örneğine bağlanmak için.
5. İçinde **Object Explorer**, genişletin **güvenlik**, genişletin ve ardından **oturumları**.
6. Sağ **NT AUTHORITY\SYSTEM** oturum açma ve ardından **özellikleri**.
7. Üzerinde **güvenliği sağlanabilir öğelerle** sayfası, yerel sunucunun, select **Grant** aşağıdaki izinleri ve ardından **Tamam**.
   
   * Herhangi bir kullanılabilirlik grubu Değiştir
   * SQL bağlantı
   * VIEW server state
8. Ekleme **CORP\Install** olarak bir **sysadmin** rol varsayılan SQL Server örneği. İçinde **Object Explorer**, sağ **oturumları**ve ardından **yeni oturum açma**.
9. Tür **CORP\Install** içinde **oturum açma adı**.
10. Üzerinde **sunucu rolleri** sayfasında, **sysadmin**ve ardından **Tamam**. Oturum açma oluşturduktan sonra genişleterek görebileceğiniz **oturumları** içinde **Object Explorer**.
11. SQL Server için bir güvenlik duvarı kuralı oluşturmak için **Başlat** ekran, açık **Gelişmiş Güvenlik Özellikli Windows Güvenlik Duvarı**.
12. Sol bölmede seçin **gelen kuralları**. Sağ bölmede **yeni kural**.
13. Üzerinde **kural türü** sayfasında, **Program** > **sonraki**.
14. Üzerinde **Program** sayfasında, **bu programın yolu**, türü **%ProgramFiles%\Microsoft SQL Server\MSSQL12. MSSQLSERVER\MSSQL\Binn\sqlservr.exe** metin kutusuna ve ardından **sonraki**. Bu yönergeleri izleyerek ancak SQL Server 2012 kullanarak, SQL Server dizindir **MSSQL11. MSSQLSERVER**.
15. Üzerinde **eylem** sayfasında, tutmak **bağlantıya izin** seçili ve ardından **sonraki**.
16. Üzerinde **profil** sayfasında, varsayılan ayarları kabul edin ve ardından **sonraki**.
17. Üzerinde **adı** sayfasında, bir kural adı gibi belirtin **SQL Server (Program kuralı)**, **adı** metin kutusuna ve ardından **son**.
18. Etkinleştirmek için **Always On kullanılabilirlik grupları** özelliği, üzerinde **Başlat** ekran, açık **SQL Server Configuration Manager**.
19. Tarayıcı ağacında tıklayın **SQL Server Hizmetleri**, sağ **SQL Server (MSSQLSERVER)** hizmet ve ardından **özellikleri**.
20. Tıklatın **her zaman üzerinde yüksek kullanılabilirlik** sekmesine **etkinleştirmek Always On kullanılabilirlik grupları**gösterildiği aşağıdaki ekran görüntüsünde ve ardından **Uygula**. Tıklatın **Tamam** iletişim kutusu ve kapatmayın yapın **özellikleri** henüz iletişim kutusu. Hizmet hesabı değiştirdikten sonra SQL Server hizmetini yeniden başlatılır.
    
     ![Always On kullanılabilirlik grupları etkinleştir](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665520.gif)
21. SQL Server hizmet hesabı değiştirmek için tıklatın. **oturum açma** sekmesinde, yazın **CORP\SQLSvc1** (için **ContosoSQL1**) veya **CORP\SQLSvc2** (için **ContosoSQL2**) içinde **hesap adı**doldurun ve parolayı onaylayın ve ardından **Tamam**.
22. Açılan iletişim kutusunda tıklatın **Evet** SQL Server hizmetini yeniden başlatmak için. SQL Server hizmeti yeniden başlatıldıktan sonra yapılan değişiklikleri **özellikleri** iletişim kutusu etkin.
23. Sanal makineler dışında oturum açın.

## <a name="create-the-availability-group"></a>Kullanılabilirlik grubu oluşturma
Artık bir kullanılabilirlik grubu yapılandırmak hazırsınız. Ne yapacağını, ana hattı aşağıdadır:

* Yeni veritabanı oluştur (**MyDB1**) üzerinde **ContosoSQL1**.
* Tam yedekleme ve veritabanının işlem günlüğü yedeklemesi gerçekleştirin.
* Tam geri yükleme ve günlük yedeklemeler **ContosoSQL2** ile **NORECOVERY** seçeneği.
* Kullanılabilirlik grubu oluşturun (**AG1**) zaman uyumlu tamamlama, otomatik yük devretme ve okunabilir ikincil kopya.

### <a name="create-the-mydb1-database-on-contososql1"></a>Üzerinde ContosoSQL1 MyDB1 veritabanı oluşturma
1. Uzak Masaüstü oturumları için dışında oturum açmış değil, **ContosoSQL1** ve **ContosoSQL2**, bunu şimdi yapın.
2. RDP dosyasını açın **ContosoSQL1**, olarak oturum açın ve **CORP\Install**.
3. İçinde **dosya Gezgini**altında **C:\\**, adlı bir dizin oluşturun **yedekleme**. Bu dizin ve veritabanı geri yükleme için kullanır.
4. Yeni dizin sağ tıklayın, fareyle **paylaşmak**ve ardından **belirli kişiler**aşağıdaki ekran görüntüsünde gösterildiği gibi.
   
    ![Bir yedekleme klasörü oluşturma](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665521.gif)
5. Ekleme **CORP\SQLSvc1**ve ardından verin **okuma/yazma** izni. Ekleme **CORP\SQLSvc2**ve ardından verin **okuma** 'ye tıklayın ve aşağıdaki ekran görüntüsü gösterildiği gibi izin **paylaşımı**. Dosya Paylaşımı işlemi tamamlandıktan sonra tıklatın **Bitti**.
   
    ![Yedekleme klasörü için izin ver](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665522.gif)
6. Veritabanı oluşturmak için **Başlat** menüsünde, açık **SQL Server Management Studio**ve ardından **Bağlan** varsayılan SQL Server örneğine bağlanmak için.
7. İçinde **Object Explorer**, sağ **veritabanları**ve ardından **yeni veritabanı**.
8. İçinde **veritabanı adı**, türü **MyDB1**ve ardından **Tamam**.

### <a name="make-a-full-backup-of-mydb1-and-restore-it-on-contososql2"></a>Tam MyDB1 yedeklemesini yapın ve ContosoSQL2 üzerinde geri yükleme
1. Veritabanının tam yedeklemesini yapmalarına **Nesne Gezgini**, genişletin **veritabanları**, sağ **MyDB1**, işaret **görevleri**ve ardından **yedekleme**.
2. İçinde **kaynak** bölümünde, tutmak **yedekleme türü** kümesine **tam**. İçinde **hedef** 'yi tıklatın **kaldırmak** yedekleme dosyaları için varsayılan dosya yolu kaldırmak için.
3. İçinde **hedef** 'yi tıklatın **Ekle**.
4. İçinde **dosya adı** metin kutusunda,  **\\ContosoSQL1\backup\MyDB1.bak**, tıklatın **Tamam**ve ardından **Tamam** yeniden veritabanını yedeklemek için. Yedekleme işlemi bittiğinde, bilgisayarınızı **Tamam** iletişim kutusunu kapatın.
5. Bir işlem günlüğü yedeklemesi veritabanının içindeki haline getirmek için **Object Explorer**, genişletin **veritabanları**, sağ tıklatın **MyDB1**, işaret **görevleri**ve ardından **yedekleme**.
6. İçinde **yedekleme türü**seçin **işlem günlüğü**. Tutmak **hedef** dosya yolu olarak ayarlandığında bir daha önce belirttiğiniz ve ardından **Tamam**. Yedekleme işlemini tamamladıktan sonra **Tamam** yeniden.
7. Geri yüklemek için tam ve işlem günlük yedeklemelerine **ContosoSQL2**, RDP dosyasını açın **ContosoSQL2**, olarak oturum açın ve **CORP\Install**. İçin Uzak Masaüstü oturumu bırakın **ContosoSQL1** açın.
8. Gelen **Başlat** menüsünde, açık **SQL Server Management Studio**ve ardından **Bağlan** varsayılan SQL Server örneğine bağlanmak için.
9. İçinde **Object Explorer**, sağ **veritabanları**ve ardından **Restore Database**.
10. İçinde **kaynak** bölümünde, select **aygıt**ve üç nokta düğmesine **...** tıklayın.
11. İçinde **yedekleme cihazları Seç**, tıklatın **Ekle**.
12. İçinde **yedekleme dosyası konumu**, türü  **\\ContosoSQL1\backup**, tıklatın **yenileme**seçin **MyDB1.bak**, tıklatın **Tamam**ve ardından **Tamam** yeniden. Tam yedekleme ve günlük yedekleme görmelisiniz **yedekleme kümelerinin geri yüklemek için** bölmesi.
13. Git **seçenekleri** sayfasında, **RESTORE WITH NORECOVERY** içinde **Kurtarma durumu**ve ardından **Tamam** veritabanını geri yüklemek için. Geri yükleme işlemini tamamladıktan sonra **Tamam**.

### <a name="create-the-availability-group"></a>Kullanılabilirlik grubu oluşturma
1. Uzak Masaüstü oturumu için geri gidin **ContosoSQL1**. İçinde **Object Explorer** SQL Server Management Studio'da sağ **her zaman üzerinde yüksek kullanılabilirlik**ve ardından **yeni Kullanılabilirlik Grubu Sihirbazı'nı**, aşağıdaki ekran görüntüsünde gösterildiği gibi.
   
    ![Yeni Kullanılabilirlik Grubu Sihirbazı'nı başlatın](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665523.gif)
2. Üzerinde **giriş** sayfasında, **sonraki**. Üzerinde **kullanılabilirlik grubu adı belirtin** sayfasında, **AG1** içinde **kullanılabilirlik grubu adının**, ardından **sonraki** yeniden.
   
    ![Yeni Kullanılabilirlik Grubu Sihirbazı'nı, kullanılabilirlik grubu adını belirtin](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665524.gif)
3. Üzerinde **seçin veritabanları** sayfasında **MyDB1**ve ardından **sonraki**. En az bir tam yedekleme hedeflenen birincil Çoğaltmada uyguladığınız için veritabanı bir kullanılabilirlik grubu için Önkoşullar karşılıyor.
   
    ![Yeni Availabilty Grubu Sihirbazı'nı seçin veritabanları](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665525.gif)
4. üzerinde **çoğaltmaları belirle** sayfasında, **eklemek çoğaltma**.
   
    ![Yeni Availabilty Grubu Sihirbazı, çoğaltmaları belirtin](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665526.gif)
5. İçinde **sunucuya Bağlan** iletişim kutusuna **ContosoSQL2** içinde **sunucu adı**ve ardından **Bağlan**.
   
    ![Yeni Availabilty Grubu Sihirbazı'nı, sunucuya Bağlan](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665527.gif)
6. Geri **çoğaltmaları belirle** sayfasında, görmelisiniz **ContosoSQL2** listelenen **kullanılabilir çoğaltmaların**. Çoğaltmalar, aşağıdaki ekran görüntüsünde gösterildiği gibi yapılandırın. İşiniz bittiğinde, tıklatın **sonraki**.
   
    ![Yeni Availabilty Grubu Sihirbazı'nı belirtin çoğaltmaları (tam)](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665528.gif)
7. Üzerinde **ilk veri eşitlemesi** sayfasında **yalnızca katılma**ve ardından **sonraki**. Tam ve işlem yedekleme yapıldığında, zaten veri eşitleme el ile gerçekleştirilen **ContosoSQL1** ve bunları geri **ContosoSQL2**. Olmayan yedekleme ve geri yükleme işlemleri, veritabanınızdaki ve bunun yerine seçmek seçebileceğiniz **tam** yeni Kullanılabilirlik Grubu Sihirbazı veri eşitleme gerçekleştirdiğiniz izin vermek için. Ancak, bazı şirketler bulunan çok büyük veritabanları için bu seçeneği önermiyoruz.
   
    ![Yeni Availabilty Grubu Sihirbazı'nı, Select ilk veri eşitlemesi](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665529.gif)
8. Üzerinde **doğrulama** sayfasında, **sonraki**. Bu sayfa aşağıdaki ekran görüntüsüne benzer görünmelidir. Bir kullanılabilirlik grubu dinleyicisi yapılandırılmadığı için dinleyici yapılandırması için bir uyarı yok. Bu öğretici bir dinleyici yapılandırmaz çünkü bu uyarıyı yoksayabilirsiniz. Bu öğreticiyi tamamladıktan sonra dinleyicisini yapılandırmak için bkz: [Azure'da Always On kullanılabilirlik grupları için bir ILB dinleyicisi yapılandırın](../classic/ps-sql-int-listener.md).
   
    ![Yeni Availabilty Grubu Sihirbazı, doğrulama](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665530.gif)
9. Üzerinde **Özet** sayfasında, **son**ve ardından yeni Kullanılabilirlik Grubu Sihirbazı'nı yapılandırırken bekleyin. Üzerinde **ilerleme** tıklayabilirsiniz sayfasında **daha fazla ayrıntı** ayrıntılı ilerleme durumunu görüntülemek için. Sihirbaz sona erdikten sonra incelemek **sonuçları** sayfasına aşağıdaki ekran görüntüsünde gösterildiği gibi kullanılabilirlik grubu başarıyla oluşturulduğunu doğrulayın ve ardından **Kapat** sihirbazdan çıkmak için.
   
    ![Yeni Availabilty Grubu Sihirbazı'nı sonuçları](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665531.gif)
10. İçinde **Object Explorer**, genişletin **her zaman üzerinde yüksek kullanılabilirlik**, genişletin ve ardından **kullanılabilirlik grupları**. Bu kapsayıcıda yeni kullanılabilirlik grubu görmelisiniz. Sağ **AG1 (birincil)**ve ardından **Göster Pano**.
    
     ![Göster kullanılabilirlik grubu Panosu](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665532.gif)
11. **Üzerinde her zaman Pano** aşağıdaki ekran görüntüsünde gösterilene benzer görünmelidir. Çoğaltmalar, yük devretme modu her çoğaltma ve eşitleme durumu görebilirsiniz.
    
     ![Kullanılabilirlik grubu Panosu](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665533.gif)
12. Dönün **Sunucu Yöneticisi'ni**, tıklatın **Araçları**ve ardından açın **yük devretme kümesi Yöneticisi**.
13. Genişletme **Cluster1.corp.contoso.com**, genişletin ve ardından **hizmetler ve uygulamalar**. Seçin **rolleri** ve unutmayın **AG1** kullanılabilirlik grubu rolü oluşturuldu. Dinleyici yapılandırmadı çünkü AG1 tarafından hangi veritabanı kullanılabilirlik grubuna istemcilerin bağlanabileceği bir IP adresi sahip olup olmadığını unutmayın. Okuma-yazma işlemleri için birincil düğüm ve salt okunur sorgular için ikincil düğüm için doğrudan bağlanabilir.
    
     ![Yük Devretme Kümesi Yöneticisi'nde AG](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665534.gif)

> [!WARNING]
> Kullanılabilirlik grubu yük devretme kümesi Yöneticisi'nden üzerinden vermesine çalışmayın. Tüm yük devretme işlemlerini içinden gerçekleştirilmelidir **üzerinde her zaman Pano** SQL Server Management Studio'da. Daha fazla bilgi için bkz: [kısıtlamaları üzerinde kullanarak yük devretme kümesi Yöneticisi kullanılabilirlik grupları ile](https://msdn.microsoft.com/library/ff929171.aspx).
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Artık başarıyla SQL Server Always On Azure'da bir kullanılabilirlik grubu oluşturarak uyguladık. Bu kullanılabilirlik grubu için bir dinleyici yapılandırmak için bkz: [Azure'da Always On kullanılabilirlik grupları için bir ILB dinleyicisi yapılandırın](../classic/ps-sql-int-listener.md).

Azure'da SQL Server kullanma hakkında diğer bilgi için bkz: [Azure Virtual Machines'de SQL Server](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

