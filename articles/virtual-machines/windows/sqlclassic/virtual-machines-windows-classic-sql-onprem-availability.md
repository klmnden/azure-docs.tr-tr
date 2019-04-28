---
title: Şirket içi Always On kullanılabilirlik grupları Azure'a genişletme | Microsoft Docs
description: Bu öğretici, Klasik dağıtım modeliyle oluşturulan kaynaklar kullanılmaktadır ve Azure'da bir Always On kullanılabilirlik grubu çoğaltması eklemek için SQL Server Management Studio (SSMS) çoğaltma Ekleme Sihirbazı'nı kullanmayı açıklar.
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: craigg
editor: ''
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
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61481618"
---
# <a name="extend-on-premises-always-on-availability-groups-to-azure"></a>Şirket içi Always On kullanılabilirlik grupları Azure'a genişletme
Always On kullanılabilirlik grupları, ikincil çoğaltmalar ekleyerek veritabanı grupları için yüksek kullanılabilirlik sağlar. Bu çoğaltmaların izin arıza durumunda veritabanlarını devretmek. Ayrıca, okuma iş yükleri veya yedekleme görevi yük boşaltması için kullanılabilir.

Şirket içi kullanılabilirlik grupları, SQL Server Azure Vm'leri bir veya daha fazla sağlama ve bunları, şirket içi kullanılabilirlik grupları için çoğaltma olarak ekleme Microsoft Azure'a genişletebilirsiniz.

Bu öğretici aşağıdakilere sahip varsayar:

* Etkin bir Azure aboneliği. Yapabilecekleriniz [ücretsiz deneme için kaydolun](https://azure.microsoft.com/pricing/free-trial/).
* Bir mevcut Always On kullanılabilirlik grubu şirket içi. Kullanılabilirlik grupları hakkında daha fazla bilgi için bkz. [Always On kullanılabilirlik grupları](https://msdn.microsoft.com/library/hh510230.aspx).
* Azure sanal ağınız ile şirket içi ağ arasında bağlantı. Bu sanal ağ oluşturma hakkında daha fazla bilgi için bkz. [Azure portalını (Klasik) kullanarak siteden siteye bağlantı oluşturma](../../../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md).

> [!IMPORTANT] 
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.

## <a name="add-azure-replica-wizard"></a>Azure çoğaltma Sihirbazı Ekle
Bu bölümde, nasıl kullanılacağını gösterir **Azure çoğaltması Ekleme Sihirbazı'nı** Always On kullanılabilirlik grubu çözümünüzü Azure çoğaltmaları içerecek şekilde genişletmek için.

> [!IMPORTANT]
> **Azure çoğaltması Ekleme Sihirbazı'nı** yalnızca klasik dağıtım modeliyle oluşturulan sanal makineleri destekler. Yeni VM dağıtımları daha yeni Resource Manager modelini kullanmanız gerekir. Resource Manager ile Vm'leri kullanıyorsanız, Transact-SQL commmands (burada gösterilmiyor) kullanarak Azure ikincil çoğaltmayı el ile eklemelisiniz. Bu sihirbaz, Resource Manager senaryoda çalışmaz.

1. SQL Server Management Studio gelen genişletin **Always On yüksek kullanılabilirlik** > **kullanılabilirlik grupları** > **[kullanılabilirlik grubunuzun adı]**.
2. Sağ **kullanılabilirlik çoğaltmalarının**, ardından **ekleme çoğaltma**.
3. Varsayılan olarak, **Kullanılabilirlik Grubu Sihirbazı Ekle çoğaltmaya** görüntülenir. **İleri**’ye tıklayın.  Seçtiyseniz **bu sayfayı bir daha gösterme** sayfanın alt kısmındaki Bu sihirbaz, bu ekran seçeneğini bir önceki başlatma sırasında görüntülenmez.
   
    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742861.png)
4. Var olan tüm ikincil çoğaltmalara bağlanmak için gerekli olacaktır. Tıklayabilirsiniz **Bağlan...** her yineleme veya tıklayabilirsiniz **tüm Bağlan...** ekranın alt kısmında. Sonra kimlik doğrulaması **sonraki** sonraki ekrana ilerletmek için.
5. Üzerinde **çoğaltmaları belirle** sayfasında birden fazla sekme üstte listelenir: **Çoğaltmaları**, **uç noktaları**, **yedekleme tercihlerine**, ve **dinleyici**. Gelen **çoğaltmaları** sekmesinde **Azure çoğaltma Ekle...** Azure çoğaltması Ekleme Sihirbazı'nı başlatmak için.
   
    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742863.png)
6. Önce yüklediyseniz yerel Windows sertifika deposundan var olan bir Azure yönetim sertifikasını seçin. Önce bir kullandıysanız bir Azure aboneliği kimliği girin veya seçin. İndirip bir Azure yönetim sertifikası yüklemeniz ve bir Azure hesabı kullanarak Aboneliklerin listesini indirmek için indir'ye tıklayabilirsiniz.
   
    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742864.png)
7. Her bir alan sayfasında Azure sanal makinesi (çoğaltmasını barındıracak VM) oluşturmak için kullanılan değerlerle doldurulur.
   
   | Ayar | Açıklama |
   | --- | --- |
   | **Görüntü** |İstenen birleşimi işletim sistemi ve SQL Server'ı seçin |
   | **VM boyutu** |İş gereksinimlerinize uygun VM boyutunu seçin |
   | **VM adı** |Yeni sanal makine için benzersiz bir ad belirtin. Adı gerekir 3 ile 15 arasında karakter içermesi, yalnızca harf, rakam ve tire içeren ve bir harf ile başlamalı ve harf veya sayı ile bitmelidir. |
   | **VM kullanıcı adı** |Sanal makinedeki yönetici hesabı olacak bir kullanıcı adı belirtin |
   | **Sanal makine yönetici parolası** |Yeni hesap için bir parola belirtin |
   | **Parolayı onaylayın** |Yeni hesap parolasını onaylayın |
   | **Sanal Ağ** |Yeni VM kullanması gereken Azure sanal ağı belirtin. Sanal ağlar hakkında daha fazla bilgi için bkz. [sanal ağa genel bakış](../../../virtual-network/virtual-networks-overview.md). |
   | **Sanal ağ alt ağı** |Yeni VM kullanması gereken sanal ağ alt ağı belirtin |
   | **Etki alanı** |Etki alanı için önceden doldurulmuş değerin doğru olduğunu onaylayın |
   | **Etki alanı kullanıcı adı** |Yerel küme düğümlerinde yerel Yöneticiler grubunda olan bir hesap belirtin |
   | **Parola** |Etki alanı kullanıcı adı için bir parola belirleyin |
8. Tıklayın **Tamam** dağıtım ayarlarını doğrulamak için.
9. Yasal koşullar sonraki görüntülenir. Okuma ve tıklayın **Tamam** bu koşulları kabul ediyorsanız.
10. **Çoğaltmaları belirle** sayfası yeniden görüntülenir. Yeni Azure çoğaltma ayarlarını doğrulayın **çoğaltmaları**, **uç noktaları**, ve **yedekleme tercihlerine** sekmeler. İş gereksinimlerinizi karşılamak için ayarları değiştirin.  Bu sekmelerde yer alan parametreler hakkında daha fazla bilgi için bkz. [çoğaltmaları sayfası belirtin (yeni Kullanılabilirlik Grubu Sihirbazı'nı / Add çoğaltma Sihirbazı)](https://msdn.microsoft.com/library/hh213088.aspx). Not dinleyicileri Azure çoğaltmalarını içeren kullanılabilirlik grupları için dinleyici sekmesi kullanılarak oluşturulamaz. Ayrıca, bir dinleyici zaten Sihirbazı'nı başlatmadan önce oluşturulduysa, Azure'da desteklenmediğini belirten bir ileti alırsınız. İçinde dinleyicileri oluşturma atacağız **bir kullanılabilirlik grubu dinleyicisi oluşturma** bölümü.
    
     ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742865.png)
11. **İleri**’ye tıklayın.
12. Kullanmak istediğiniz veri eşitleme yöntemi seçin **ilk veri eşitlemesini Seç** sayfasında ve tıklayın **sonraki**. Çoğu senaryo için seçin **tam veri eşitlemesi**. Veri Eşitleme yöntemleri hakkında daha fazla bilgi için bkz. [seçin ilk veri eşitlemeyi sayfası (her zaman üzerinde kullanılabilirlik grubu sihirbazlar)](https://msdn.microsoft.com/library/hh231021.aspx).
13. Sonuçları gözden **doğrulama** sayfası. Bekleyen sorunları düzeltin ve gerekirse doğrulamayı yeniden çalıştırın. **İleri**’ye tıklayın.
    
     ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742866.png)
14. Ayarları gözden **özeti** sayfasında'a tıklayın **son**.
15. Sağlama işlemi başlar. Sihirbaz başarıyla tamamlandığında, tıklayın **Kapat** Sihirbazı'ndan çıkmak için.

> [!NOTE]
> Azure çoğaltması Ekleme Sihirbazı'nı Users\User Name\AppData\Local\SQL Server\AddReplicaWizard içinde bir günlük dosyası oluşturur. Bu günlük dosyası başarısız Azure çoğaltma dağıtımlardaki sorunları çözmek için kullanılabilir. Sihirbazın herhangi bir eylemi yürütmede başarısız olursa, sağlanan sanal Makineyi silme de dahil olmak üzere önceki tüm işlemler geri alınır.
> 
> 

## <a name="create-an-availability-group-listener"></a>Bir kullanılabilirlik grubu dinleyicisi oluşturun
Kullanılabilirlik Grubu oluşturulduktan sonra yinelemeler için bağlanmak için istemcilere yönelik bir dinleyici oluşturmanız gerekir. Dinleyicileri gelen bağlantıları birincil ya da salt okunur bir ikincil çoğaltmaya yönlendirir. Dinleyicileri hakkında daha fazla bilgi için bkz. [Azure'da Always On kullanılabilirlik grupları için ILB dinleyicisi yapılandırma](../classic/ps-sql-int-listener.md).

## <a name="next-steps"></a>Sonraki adımlar
Kullanmanın yanı sıra **Azure çoğaltması Ekleme Sihirbazı'nı** , Always On kullanılabilirlik grubu Azure'a genişletmek için de bazı SQL Server iş yüklerini tamamen Azure'da taşımanız. Başlamak için bkz: [azure'da bir SQL Server sanal makinesi sağlama](../sql/virtual-machines-windows-portal-sql-server-provision.md).

Azure Vm'lerinde SQL Server çalıştırma ile ilgili diğer konular için bkz [Azure Virtual Machines'de SQL Server](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

