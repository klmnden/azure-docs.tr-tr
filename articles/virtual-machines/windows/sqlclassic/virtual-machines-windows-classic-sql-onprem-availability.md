---
title: "Şirket içi Always On kullanılabilirlik grupları için Azure genişletmek | Microsoft Docs"
description: "Bu öğretici Klasik dağıtım modeliyle oluşturulan kaynakları kullanır ve Azure'da bir her zaman üzerindeki kullanılabilirlik grubu çoğaltması eklemek için SQL Server Management Studio (SSMS) çoğaltma Ekleme Sihirbazı'nı kullanmayı açıklar."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: craigg
editor: 
tags: azure-service-management
ms.assetid: 7ca7c423-8342-4175-a70b-d5101dfb7f23
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/31/2017
ms.author: mikeray
ms.openlocfilehash: d3e56f1741a9cfd3f2d9f786c2ce22eb6a946ef2
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="extend-on-premises-always-on-availability-groups-to-azure"></a>Azure için şirket içi Always On kullanılabilirlik grupları genişletme
Always On kullanılabilirlik grupları ikincil çoğaltmaları ekleyerek veritabanının grupları için yüksek kullanılabilirlik sağlar. Bu çoğaltmalar izin arıza durumunda veritabanlarını yük devrediliyor. Buna ek olarak, okuma iş yükleri veya yedekleme görevlerini boşaltmak için kullanılabilir.

Bir veya daha fazla Azure Vm'lerde SQL Server ile sağlama ve bunları şirket içi kullanılabilirlik gruplarına çoğaltmaları olarak ekleyerek Microsoft Azure için şirket içi kullanılabilirlik grupları genişletebilirsiniz.

Bu öğretici aşağıdakilere sahip varsayılır:

* Etkin bir Azure aboneliği. Yapabilecekleriniz [ücretsiz deneme için kaydolun](https://azure.microsoft.com/pricing/free-trial/).
* Varolan her zaman üzerinde kullanılabilirlik grubunu şirket içi. Kullanılabilirlik grupları hakkında daha fazla bilgi için bkz: [Always On kullanılabilirlik grupları](https://msdn.microsoft.com/library/hh510230.aspx).
* Şirket içi ağınız ve Azure sanal ağınız arasında bağlantı. Bu sanal ağ oluşturma hakkında daha fazla bilgi için bkz: [Azure Portalı'nı (Klasik) kullanarak siteden siteye bağlantı oluşturma](../../../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md).

> [!IMPORTANT] 
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.

## <a name="add-azure-replica-wizard"></a>Azure Yineleme Sihirbazı Ekle
Bu bölümde, nasıl kullanılacağını gösterir **Azure çoğaltma Ekleme Sihirbazı'nı** Azure çoğaltmaları dahil etmek için her zaman üzerindeki kullanılabilirlik grubu çözümünüzü genişletmek için.

> [!IMPORTANT]
> **Azure çoğaltma Ekleme Sihirbazı'nı** yalnızca klasik dağıtım modeliyle oluşturulan sanal makineleri destekler. Yeni VM dağıtımlarının yeni Resource Manager modeli kullanmanız gerekir. Resource Manager ile VM kullanıyorsanız, Transact-SQL commmands (burada gösterilmiyor) kullanarak ikincil Azure çoğaltmayı el ile eklemelisiniz. Bu sihirbaz Resource Manager senaryosunda çalışmaz.

1. Gelen SQL Server Management Studio içinde genişletin **her zaman üzerinde yüksek kullanılabilirlik** > **kullanılabilirlik grupları** > **[kullanılabilirlik grubu adı]**.
2. Sağ **kullanılabilirlik çoğaltmalarının**, ardından **eklemek çoğaltma**.
3. Varsayılan olarak, **eklemek çoğaltma Kullanılabilirlik Grubu Sihirbazı'nı** görüntülenir. **İleri**’ye tıklayın.  Seçtiyseniz **bu sayfayı bir daha gösterme** seçeneği sayfanın sonundaki önceki başlatma sırasında bu sihirbaz, bu ekranda görüntülenmez.
   
    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742861.png)
4. Var olan tüm ikincil çoğaltmalar için bağlanmak için gerekli. Tıklatabilirsiniz **Bağlan...** her yineleme veya tıklatabilirsiniz **bağlanmak tüm...** ekranın alt kısmında. Sonra kimlik doğrulaması **sonraki** sonraki ekrana ilerlemek için.
5. Üzerinde **çoğaltmaları belirle** sayfasında, birden fazla sekme üstte listelenir: **çoğaltmaları**, **uç noktaları**, **yedekleme tercihlerine**, ve **dinleyici**. Gelen **çoğaltmaları** sekmesini tıklatın, **Azure çoğaltma Ekle...** Azure çoğaltma Ekleme Sihirbazı'nı başlatmak için.
   
    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742863.png)
6. Önce bir yüklü değilse yerel Windows sertifika deposundan olan bir Azure yönetim sertifikasını seçin. Önce bir kullandıysanız, Azure aboneliği kimliğini girin veya seçin. Karşıdan yükle ve Azure yönetim sertifikasını yükleyin ve bir Azure hesabı kullanarak Aboneliklerin listesini indirmek için indirme tıklatabilirsiniz.
   
    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742864.png)
7. Azure sanal makine (çoğaltmasını barındıracak VM) oluşturmak için kullanılan değerlerle sayfasında her bir alan doldurulacaktır.
   
   | Ayar | Açıklama |
   | --- | --- |
   | **Görüntü** |İşletim sistemi ve SQL Server istenen birleşimi seçin |
   | VM boyutu |İş gereksinimlerinize en uygun VM boyutunu seçin |
   | **VM adı** |Yeni VM için benzersiz bir ad belirtin. Adı gerekir 3 ile 15 karakter arasında içeren, yalnızca harf, rakam ve tire içerebilir ve gerekir bir harfle başlamalı ve harf veya sayı ile bitmelidir. |
   | **VM kullanıcı adı** |VM üzerinde yönetici hesabı olacaktır bir kullanıcı adı belirtin |
   | **VM yönetici parolası** |Yeni hesap için bir parola belirtin |
   | **Parolayı onaylayın** |Yeni Parolayı Onayla |
   | **Sanal Ağ** |Yeni VM kullanması gereken Azure sanal ağı belirtin. Sanal ağlar hakkında daha fazla bilgi için bkz: [Virtual Network'e genel bakış](../../../virtual-network/virtual-networks-overview.md). |
   | **Sanal ağ alt ağı** |Yeni VM kullanması gereken sanal ağ alt ağı belirtin |
   | **Etki alanı** |Etki alanı için önceden doldurulmuş haldedir değerin doğru olduğunu onaylayın |
   | **Etki alanı kullanıcı adı** |Yerel küme düğümlerinde yerel Yöneticiler grubunda olan bir hesabı belirtin |
   | **Parola** |Etki alanı kullanıcı adı için parola belirtin |
8. Tıklatın **Tamam** dağıtım ayarlarını doğrulamak için.
9. Yasal koşullar sonraki görüntülenir. Okuma ve tıklatın **Tamam** bu koşulları kabul etmesi durumunda.
10. **Çoğaltmaları belirle** sayfası yeniden görüntülenir. Yeni Azure çoğaltma ayarları üzerinde doğrulama **çoğaltmaları**, **uç noktaları**, ve **yedekleme tercihlerine** sekmeleri. İş gereksinimlerinizi karşılamak üzere ayarlarını değiştirin.  Bu sekmelerde bulunan parametreleri hakkında daha fazla bilgi için bkz: [çoğaltmaları sayfası belirtin (yeni Kullanılabilirlik Grubu Sihirbazı'nı / Add çoğaltma Sihirbazı)](https://msdn.microsoft.com/library/hh213088.aspx). Not dinleyicileri dinleyicisi sekmesini Azure çoğaltmaları içeren kullanılabilirlik grupları kullanılarak oluşturulamaz. Ayrıca, bir dinleyici zaten Sihirbazı'nı başlatmadan önce oluşturulduysa, Azure'da desteklenmiyor belirten bir ileti alırsınız. İçinde dinleyicileri oluşturma ele alacağız **bir kullanılabilirlik grubu dinleyicisi oluşturma** bölümü.
    
     ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742865.png)
11. **İleri**’ye tıklayın.
12. Üzerinde kullanmak istediğiniz veri eşitleme yöntemini seçin **ilk veri eşitlemesi** sayfasında ve tıklayın **sonraki**. Çoğu senaryoda, seçin **tam veri eşitlemesi**. Veri Eşitleme yöntemleri hakkında daha fazla bilgi için bkz: [seçin ilk veri eşitlemesi sayfası (her zaman üzerinde kullanılabilirlik grubu sihirbazlar)](https://msdn.microsoft.com/library/hh231021.aspx).
13. Sonuçları gözden **doğrulama** sayfası. Bekleyen sorunları düzeltin ve gerekirse doğrulamayı yeniden çalıştırın. **İleri**’ye tıklayın.
    
     ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742866.png)
14. Ayarları gözden **Özet** sayfasında ve ardından **son**.
15. Sağlama işlemi başlar. Sihirbaz başarıyla tamamlandığında, tıklayın **Kapat** dışında sihirbazdan çıkmak için.

> [!NOTE]
> Azure çoğaltma Ekleme Sihirbazı'nı Users\User Name\AppData\Local\SQL Server\AddReplicaWizard içinde bir günlük dosyası oluşturur. Bu günlük dosyası başarısız Azure çoğaltma dağıtımlardaki sorunları çözmek için kullanılabilir. Sihirbazın herhangi bir eylem yürütme başarısız olursa, tüm önceki işlemleri sağlanan VM silme de dahil olmak üzere geri alınır.
> 
> 

## <a name="create-an-availability-group-listener"></a>Bir kullanılabilirlik grubu dinleyicisi oluşturma
Kullanılabilirlik Grubu oluşturulduktan sonra çoğaltmalar için bağlanmak için istemcilere yönelik bir dinleyici oluşturmanız gerekir. Dinleyicileri gelen bağlantıları birincil veya salt okunur bir ikincil çoğaltmaya yönlendirir. Dinleyicileri hakkında daha fazla bilgi için bkz: [Azure'da Always On kullanılabilirlik grupları için bir ILB dinleyicisi yapılandırın](../classic/ps-sql-int-listener.md).

## <a name="next-steps"></a>Sonraki adımlar
Kullanmanın yanı sıra **Azure çoğaltma Ekleme Sihirbazı'nı** , her zaman üzerindeki kullanılabilirlik grubu için Azure genişletmek için bazı SQL Server iş yüklerini tamamen Azure'a taşımak da. Başlamak için bkz: [Azure üzerinde bir SQL Server sanal makine sağlama](../sql/virtual-machines-windows-portal-sql-server-provision.md).

Azure Vm'lerde SQL Server çalıştırma ile ilgili diğer konular için bkz: [Azure Virtual Machines'de SQL Server](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

