---
title: "Azure Site Recovery kullanarak çok katmanlı bir Dynamics AX dağıtım çoğaltmak | Microsoft Docs"
description: "Bu makalede, çoğaltma ve Azure Site Recovery kullanarak Dynamics AX korumak açıklar"
services: site-recovery
documentationcenter: 
author: asgang
manager: rochakm
editor: 
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/24/2017
ms.author: asgang
ms.openlocfilehash: c235102a60b6d11c8b77203121352bd1400f4325
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="replicate-a-multitier-dynamics-ax-application-by-using-azure-site-recovery"></a>Azure Site Recovery kullanarak çok katmanlı bir Dynamics AX uygulamayı çoğaltma

## <a name="overview"></a>Genel Bakış


 Dynamics AX kurumların konumlar arasında işlemleri standartlaştırmak, kaynakları yönetmek ve uyumluluk kolaylaştırmak için kullanılan en popüler ERP çözümleri biridir. Uygulama bir olağanüstü durum durumunda bir kuruluş için kritik olduğundan uygulama en az sürede hazır ve çalışır olmalıdır.

Bugün, Dynamics AX tüm giden kutusu olağanüstü durum kurtarma özellikleri sağlamaz. Windows Uygulama Nesne Sunucusu, Azure Active Directory, Azure SQL veritabanı, SharePoint Server ve Reporting Services gibi pek çok sunucu bileşenleri, Dynamics AX oluşur. Olağanüstü durum yönetmek için bu bileşenlerin her birini el ile yalnızca aynı zamanda hataya pahalı değildir.

Bu makalede, Dynamics AX uygulamanız için kullanarak bir olağanüstü durum kurtarma çözümü nasıl oluşturabileceğiniz açıklanmaktadır [Azure Site Recovery](site-recovery-overview.md). Ayrıca, bir tek tıklamayla kurtarma planı, desteklenen yapılandırmalar ve önkoşullar kullanarak planlanan/planlanmamış yük devretme testlerini da kapsar.

Site Recovery temelli olağanüstü durum kurtarma çözümü tam olarak test sertifikalı ve Dynamics AX tarafından önerilir.


## <a name="prerequisites"></a>Ön koşullar

Site RECOVERY'yi kullanarak Dynamics AX uygulamasının olağanüstü durum kurtarma uygulama aşağıdaki önkoşulları gerektirir:

• Bir şirket içi Dynamics AX dağıtım ayarlayın.

• Bir Azure aboneliği Site Recovery kasası oluşturun.

Azure kurtarma sitenizi ise • sanal makinelerin Azure sanal makine hazırlık değerlendirme aracı çalıştırın. Azure sanal makineleri ve Site Recovery services ile uyumlu olmaları gerekir.

## <a name="site-recovery-support"></a>Site kurtarma desteği

Bu makale oluşturmak amacıyla VMware sanal makineler üzerinde Windows Server 2012 R2 Enterprise Dynamics AX 2012 R3 ile kullandık. Site recovery çoğaltma uygulama belirsiz olduğundan, burada aşağıdaki senaryolar için tutmak için sağlanan öneriler bekliyoruz.

### <a name="source-and-target"></a>Kaynak ve hedef

**Senaryo** | **İkincil bir siteye** | **Azure'a**
--- | --- | ---
**Hyper-V** | Evet | Evet
**VMware** | Evet | Evet
**Fiziksel sunucu** | Evet | Evet

## <a name="enable-disaster-recovery-of-the-dynamics-ax-application-by-using-site-recovery"></a>Site RECOVERY'yi kullanarak Dynamics AX uygulamasının olağanüstü durum kurtarmayı etkinleştir
### <a name="protect-your-dynamics-ax-application"></a>Dynamics AX uygulamanızı koruma
Tam uygulama çoğaltma ve kurtarma etkinleştirmek için her bileşen Dynamics AX korunması gerekir. 

### <a name="1-set-up-active-directory-and-dns-replication"></a>1. Active Directory ve DNS çoğaltmayı ayarlama

Active Directory işlevi Dynamics AX uygulamasının olağanüstü durum kurtarma sitesinde gereklidir. Bir müşterinin şirket içi ortamına karmaşıklığı aşağıdaki iki seçenek öneririz.

**Seçenek 1**

Az sayıda uygulamayı müşteri sahiptir ve tüm tek etki alanı denetleyicisi şirket içi site ve birlikte tüm site vermesine planları. Etki alanı denetleyicisi makinesi (siteden siteye ve site Azure senaryoları için geçerlidir) ikincil bir siteye çoğaltmak için Site Recovery çoğaltma kullanmanızı öneririz.

**Seçenek 2**

Müşteri, çok sayıda uygulamalar vardır ve Active Directory ormanını ve aynı anda birkaç uygulamaları vermesine planları çalışıyor. Olağanüstü durum kurtarma sitesinde bir ek etki alanı denetleyicisi ayarlamanızı öneririz (ikincil bir site veya Azure).

 Daha fazla bilgi için bkz: [bir olağanüstü durum kurtarma sitesinde bir etki alanı denetleyicisi kullanılabilir hale](site-recovery-active-directory.md). Bu belgenin geri kalanında için bir etki alanı denetleyicisi olağanüstü durum kurtarma sitesinde kullanılabilir olduğunu varsayalım.

### <a name="2-set-up-sql-server-replication"></a>2. SQL Server çoğaltmayı ayarlama
SQL katmanı korunması için önerilen seçenek hakkında teknik yönergeler için bkz [çoğaltma SQL Server ve Azure Site Recovery uygulamalarla](site-recovery-sql.md).

### <a name="3-enable-protection-for-the-dynamics-ax-client-and-application-object-server-vms"></a>3. Dynamics AX İstemcisi ve uygulama nesnesi Server VM'ler için korumayı etkinleştirin
Olup VM'ler dağıtılan üzerinde temel ilgili Site kurtarma yapılandırması gerçekleştirmek [Hyper-V](site-recovery-hyper-v-site-to-azure.md) veya [VMware](site-recovery-vmware-to-azure.md).

> [!TIP]
> Kilitlenme tutarlı sıklığı 15 dakikaya kadar yapılandırmanızı öneririz.
>

Aşağıdaki anlık bir VMware sitesi Azure koruması senaryosunda Dynamics bileşeni Vm'leri koruma durumunu gösterir.

![Korumalı öğeler](./media/site-recovery-dynamics-ax/protecteditems.png)

### <a name="4-configure-networking"></a>4. Ağı yapılandırma
**VM işlem yapılandırmak ve ağ ayarlarını**

Böylece VM ağları yük devretme sonrasında doğru olağanüstü durum kurtarma ağa bağlı Dynamics AX İstemcisi ve uygulama nesnesi Server VM'ler için Site Recovery ağ ayarlarını yapılandırın. Bu katmanlar için olağanüstü durum kurtarma ağ SQL katmanına yönlendirilebilir olduğundan emin olun.

Aşağıdaki anlık görüntüde gösterildiği gibi VM ağ ayarlarını yapılandırmak için çoğaltılan öğe seçebilirsiniz:

* Uygulama Nesne Sunucusu sunucuları için doğru kullanılabilirlik kümesi seçin.

* Bir statik IP kullanıyorsanız, yapılacağını VM istediğiniz IP belirtin **hedef IP** metin kutusu.

    ![Ağ ayarları ](./media/site-recovery-dynamics-ax/vmpropertiesaos1.png).


### <a name="5-create-a-recovery-plan"></a>5. Kurtarma planı oluşturma

Bir kurtarma planı yük devretme işlemini otomatikleştirmek için Site Recovery oluşturabilirsiniz. Bir uygulama katmanı ve bir web katmanı kurtarma planında ekleyin. Ön uç önce uygulama katmanı kapandıktan böylece bunları farklı gruplarda sıralayın.

1. Site Recovery kasası aboneliğinizde ve seçin **kurtarma planları** döşeme.

2. Seçin **+ kurtarma planı**ve bir ad belirtin.

3. Seçin **kaynak** ve **hedef**. Hedef Azure veya ikincil bir site olabilir. Azure seçerseniz, dağıtım modeli belirtmeniz gerekir.

    ![Kurtarma planı oluşturma](./media/site-recovery-dynamics-ax/recoveryplancreation1.png)

4. Uygulama Nesne Sunucusu ve istemci sanal makineleri kurtarma planı için seçme ve ✓ seçin.

    ![Öğeleri seçin](./media/site-recovery-dynamics-ax/selectvms.png)

    Kurtarma planı örneği:

    ![Kurtarma planı ayrıntıları](./media/site-recovery-dynamics-ax/recoveryplan.png)

Kurtarma planı Dynamics AX uygulama için aşağıdaki adımları ekleyerek özelleştirebilirsiniz. Tüm adımları ekledikten sonra önceki anlık tam kurtarma planı gösterir.


* **SQL Server Yük devretme adımları**: SQL server belirli kurtarma adımları hakkında daha fazla bilgi için bkz: [SQL Server ve Azure Site Recovery ile çoğaltma uygulamaları](site-recovery-sql.md).

* **Yük devretme grubu 1**: uygulama nesnesi Server Vm'lerinin başarısız.
Seçilen kurtarma noktası PIT, veritabanı olabildiğince yakın ancak gibi olduğundan emin olun, öncesinde.

* **Komut dosyası**: ekleme yük dengeleyici (yalnızca E-A).
Bir yük dengeleyici eklemek için (aracılığıyla Azure Otomasyonu) bir betik uygulama nesnesi Server VM gruptan sonra gelir ekleyin. Bu görevi gerçekleştirmek için bir komut dosyası kullanabilirsiniz. Daha fazla bilgi için bkz: [çok katmanlı uygulama olağanüstü durum kurtarma için bir yük dengeleyici eklemek nasıl](https://azure.microsoft.com/blog/cloud-migration-and-disaster-recovery-of-load-balanced-multi-tier-applications-using-azure-site-recovery/).

* **Yük devretme Grup 2**: Dynamics AX İstemci VM'ler başarısız. Kurtarma planının bir parçası olarak VM'ler web katmanı yük devri.


### <a name="perform-a-test-failover"></a>Bir sınama yük devretmesi gerçekleştirme

Daha fazla bilgi için Active Directory'ye belirli yük devretme testi sırasında "Active Directory Olağanüstü durum kurtarma çözümü" Yardımcısı Kılavuzu'na bakın. 

Daha fazla bilgi için SQL Server'a özgü yük devretme testi sırasında [çoğaltma SQL Server ve Azure Site Recovery uygulamalarla](site-recovery-sql.md).

1. Azure Portalı'na gidin ve Site Recovery kasanızı seçin.

2. Dynamics AX için oluşturduğunuz kurtarma planı seçin.

3. Seçin **yük devretme testi**.

4. Test yük devretme işlemini başlatmak için sanal ağ seçin.

5. İkincil ortamı kurma olduktan sonra doğrulama gerçekleştirebilirsiniz.

6. Doğrulama tamamlandıktan sonra Seç **doğrulamaları tamamlamak** ve yük devretme sınama ortamı temizlendi.

Yük devretme testi gerçekleştirme hakkında daha fazla bilgi için bkz: [Test yük devretme Azure Site Recovery](site-recovery-test-failover-to-azure.md).

### <a name="perform-a-failover"></a>Bir yük devretme gerçekleştirin.

1. Azure Portalı'na gidin ve Site Recovery kasanızı seçin.

2. Dynamics AX için oluşturduğunuz kurtarma planı seçin.

3. Seçin **yük devretme**seçip **yük devretme**.

4. Hedef ağ seçin ve Seç **✓** yük devretme işlemini başlatmak üzere.

Bir yük devretme işleminden daha fazla bilgi için bkz: [Site Recovery yük](site-recovery-failover.md).

### <a name="perform-a-failback"></a>Bir yeniden çalışma gerçekleştirme

Dikkat edilecek noktalar için belirli SQL Server için yeniden çalışma sırasında bkz: [çoğaltma SQL Server ve Azure Site Recovery uygulamalarla](site-recovery-sql.md).

1. Azure Portalı'na gidin ve Site Recovery kasanızı seçin.

2. Dynamics AX için oluşturduğunuz kurtarma planı seçin.

3. Seçin **yük devretme**seçip **yük devretme**.

4. Seçin **değiştirme yönü**.

5. Uygun seçeneği belirleyin: veri eşitleme ve VM oluşturma.

6. Seçin **✓** yeniden çalışma işlemini başlatmak üzere.


Bir yeniden çalışma yapılması ile ilgili daha fazla bilgi için bkz: [azure'dan şirket içi yeniden çalışma VMware Vm'lerini](site-recovery-failback-azure-to-vmware.md).

## <a name="summary"></a>Özet
Site Recovery kullanarak, Dynamics AX uygulamanız için bir tam otomatik olağanüstü durum kurtarma planı oluşturabilirsiniz. Bir kesinti durumunda yerden saniye içinde yük devretme başlatın ve uygulamayı dakika içinde hazır ve çalışır alın.

## <a name="next-steps"></a>Sonraki adımlar
Site Recovery ile kurumsal iş yüklerinin korunması hakkında daha fazla bilgi için bkz: [hangi iş yüklerini koruyabilirim?](site-recovery-workload.md).
