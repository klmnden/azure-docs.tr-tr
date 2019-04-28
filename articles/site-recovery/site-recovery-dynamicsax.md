---
title: Azure Site Recovery kullanarak çok katmanlı bir Dynamics AX dağıtımı için olağanüstü durum kurtarma ayarlama | Microsoft Docs
description: Bu makalede, Azure Site Recovery ile Dynamics AX için olağanüstü durum kurtarma ayarlama işlemi açıklanmaktadır
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: asgang
ms.openlocfilehash: b97bf56c23dfa96acf7cb5af5ac28b4270de117d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61281480"
---
# <a name="set-up-disaster-recovery-for-a-multitier-dynamics-ax-application"></a>Çok katmanlı bir Dynamics AX uygulamasının olağanüstü durum kurtarmayı ayarlama   




 Dynamics AX, kurumların uyumluluk basitleştirmek konumlar arasında işlemleri standartlaştırmak ve kaynakları yönetmek için kullanılan en popüler ERP çözümleri biridir. Uygulama bir olağanüstü durum olması halinde bir kuruluş için kritik olduğundan uygulama en az sürede çalışır duruma olmalıdır.

Bugün, Dynamics AX herhangi bir kullanıma hazır olağanüstü durum kurtarma özellikleri sağlamaz. Windows uygulama nesnesi sunucusu, Azure Active Directory, Azure SQL veritabanı, SharePoint sunucusu ve Reporting Services gibi birçok sunucu bileşenlerinin Dynamics AX oluşur. Olağanüstü durum yönetmek için bu bileşenlerin her birini el ile yalnızca da hataya pahalı değildir.

Bu makalede, Dynamics AX uygulamanız için kullanarak bir olağanüstü durum kurtarma çözümü nasıl oluşturacağınızı açıklar [Azure Site Recovery](site-recovery-overview.md). Ayrıca, tek tıklamayla kurtarma planı, desteklenen yapılandırmalar ve önkoşullar kullanarak planlanan/planlanmamış yük devretme testleri da kapsar.



## <a name="prerequisites"></a>Önkoşullar

Site Recovery kullanarak Dynamics AX uygulamasının olağanüstü durum kurtarma uygulama aşağıdaki önkoşulları gerektirir:

• Bir şirket içi Dynamics AX dağıtım ayarlayın.

• Bir Azure aboneliğinde bir Site kurtarma kasası oluşturun.

Azure kurtarma siteniz olarak ise • Vm'lerinde Azure sanal makine hazır olma durumu değerlendirmesi aracını çalıştırın. Azure sanal makineler ve Site Recovery services ile uyumlu olmaları gerekir.

## <a name="site-recovery-support"></a>Site Recovery desteği

Bu makalede oluşturmak amacıyla, VMware sanal makineleri ile Windows Server 2012 R2 Enterprise üzerinde Dynamics AX 2012 R3 kullandık. Site recovery çoğaltma uygulamadan bağımsız olduğundan, burada aşağıdaki senaryolar için tutmak için sağlanan önerileri bekliyoruz.

### <a name="source-and-target"></a>Kaynak ve hedef

**Senaryo** | **İkincil bir siteye** | **Azure’a**
--- | --- | ---
**Hyper-V** | Evet | Evet
**VMware** | Evet | Evet
**Fiziksel sunucu** | Evet | Evet

## <a name="enable-disaster-recovery-of-the-dynamics-ax-application-by-using-site-recovery"></a>Site RECOVERY'yi kullanarak Dynamics AX uygulamasının olağanüstü durum kurtarmayı etkinleştir
### <a name="protect-your-dynamics-ax-application"></a>Dynamics AX uygulamanızı koruyun
Tam uygulama çoğaltma ve kurtarma etkinleştirmek için her bir bileşeninin Dynamics AX korunması gerekir.

### <a name="1-set-up-active-directory-and-dns-replication"></a>1. Active Directory ve DNS çoğaltmayı ayarlama

Active Directory işlev Dynamics AX uygulamasının olağanüstü durum kurtarma sitesinde gereklidir. Bir müşterinin şirket içi ortamda karmaşıklığı aşağıdaki iki seçenek öneririz.

**Seçenek 1**

Az sayıda uygulamayı müşterinin ve tamamını tek bir etki alanı denetleyicisi şirket içinde site ve tüm site üzerinde birlikte yük planlamaktadır. Etki alanı denetleyicisi makinesine (hem siteden siteye hem de site Azure'a senaryoları için geçerlidir) ikincil bir siteye çoğaltmak için Site Recovery çoğaltma kullanmanızı öneririz.

**Seçenek 2**

Müşteri çok sayıda uygulamaları olan ve Active Directory ormanını ve bazı uygulamalar üzerinde aynı anda başarısız planlamaktadır çalışıyor. Olağanüstü durum kurtarma sitesinde bir ek etki alanı denetleyicisi ayarlamak öneririz (ikincil bir siteye veya Azure).

 Daha fazla bilgi için [bir etki alanı denetleyicisi bir olağanüstü durum kurtarma sitesinde kullanılabilir hale getirmek](site-recovery-active-directory.md). Bu belgenin geri kalanında için bir etki alanı denetleyicisi olağanüstü durum kurtarma sitesinde kullanılabilir olduğunu varsayıyoruz.

### <a name="2-set-up-sql-server-replication"></a>2. SQL Server çoğaltmayı ayarlama
SQL katmanı korumak için önerilen seçenek ile ilgili teknik rehberlik için bkz. [çoğaltma SQL Server ve Azure Site Recovery ile uygulamaları](site-recovery-sql.md).

### <a name="3-enable-protection-for-the-dynamics-ax-client-and-application-object-server-vms"></a>3. Dynamics AX istemci ve uygulama nesnesi Server VM'ler için korumayı etkinleştirin
Bağlı olup olmadığını VM'ler üzerinde dağıtılır üzerinde ilgili Site Recovery yapılandırması gerçekleştirmek [Hyper-V](site-recovery-hyper-v-site-to-azure.md) veya [VMware](site-recovery-vmware-to-azure.md).

> [!TIP]
> Kilitlenmeyle tutarlı sıklığını 15 dakika yapılandırmanızı öneririz.
>

Şu anlık görüntü, bir VMware sitesi Azure'a koruma senaryosunda Dynamics bileşeni Vm'leri koruma durumunu gösterir.

![Korunan öğeler](./media/site-recovery-dynamics-ax/protecteditems.png)

### <a name="4-configure-networking"></a>4. Ağ yapılandırması
**VM işlem yapılandırma ve ağ ayarları**

Böylece VM ağları yük devretme sonrasında doğru olağanüstü durum kurtarma ağa bağlı Dynamics AX İstemcisi ve uygulama nesnesi Server Vm'leri için ağ ayarlarını Site Recovery'de yapılandırın. Bu katmanlar için olağanüstü durum kurtarma ağı SQL katmana yönlendirilebilir olduğundan emin olun.

Aşağıdaki anlık görüntüde gösterildiği gibi sanal ağ ayarlarını yapılandırmak için çoğaltılan öğeler seçebilirsiniz:

* Uygulama nesnesi sunucusu sunucular için doğru kullanılabilirlik kümesi seçin.

* Statik IP kullanıyorsanız, VM yapılacağını istediğiniz IP belirtin **hedef IP** metin kutusu.

    ![Ağ ayarları](./media/site-recovery-dynamics-ax/vmpropertiesaos1.png)


### <a name="5-create-a-recovery-plan"></a>5. Kurtarma planı oluşturma

Site Recovery, yük devretme işlemini otomatik hale getirmek için bir kurtarma planı oluşturabilirsiniz. Kurtarma planında bir uygulama katmanı ve web katmanı ekleyin. Ön uç uygulama katmanı önce kapanır, bunları farklı gruplardaki sıralayın.

1. Site Recovery kasası aboneliğinizde seçip **kurtarma planları** Döşe.

2. Seçin **+ kurtarma planı**ve bir ad belirtin.

3. Seçin **kaynak** ve **hedef**. Hedef Azure veya ikincil bir site olabilir. Azure'ı seçerseniz, dağıtım modeline belirtmeniz gerekir.

    ![Kurtarma planı oluşturma](./media/site-recovery-dynamics-ax/recoveryplancreation1.png)

4. Uygulama nesnesi sunucu ve istemci sanal makinelerine kurtarma planı için seçin ve ✓ seçin.

    ![Öğe seçin](./media/site-recovery-dynamics-ax/selectvms.png)

    Kurtarma planı örneği:

    ![Kurtarma planı ayrıntıları](./media/site-recovery-dynamics-ax/recoveryplan.png)

Dynamics AX uygulamasının kurtarma planı, aşağıdaki adımları ekleyerek özelleştirebilirsiniz. Tüm adımları ekledikten sonra önceki anlık görüntüye tam kurtarma planı gösterir.


* **SQL Server Yük devretme adımları**: Kurtarma adımları belirli SQL Server hakkında daha fazla bilgi için bkz. [SQL Server ve Azure Site Recovery çoğaltma uygulamalarla](site-recovery-sql.md).

* **Yük devretme grubu 1**: Uygulama nesnesi sunucu VM'in devredilmesini düzenleyebilirsiniz.
Seçilen kurtarma noktasının veritabanına PIT, olabildiğince yakın ancak gibi olduğundan emin olun, önceden.

* **betik**: Yük Dengeleyici Ekle (yalnızca E-A).
Bir yük dengeleyici eklemek için bir betik uygulama nesnesi Server VM grubu (aracılığıyla Azure Otomasyonu) gelir ekleyin. Bu görevi gerçekleştirmek için bir komut dosyası kullanabilirsiniz. Daha fazla bilgi için [çok katmanlı uygulama olağanüstü durum kurtarma için bir yük dengeleyici ekleme](https://azure.microsoft.com/blog/cloud-migration-and-disaster-recovery-of-load-balanced-multi-tier-applications-using-azure-site-recovery/).

* **Yük devretme grubu 2**: Dynamics AX İstemci sanal makinelerine başarısız. Kurtarma planının bir parçası olarak web katmanı Vm'leri yük devredebilirsiniz.


### <a name="perform-a-test-failover"></a>Yük devretme testi gerçekleştirme

Daha fazla bilgi için Active Directory'ye belirli yük devretme testi sırasında "Active Directory Olağanüstü durum kurtarma çözümü" Yardımcısı kılavuzuna bakın.

Daha fazla bilgi için SQL Server'a özgü yük devretme testi sırasında [çoğaltma SQL Server ve Azure Site Recovery ile uygulamaları](site-recovery-sql.md).

1. Azure portalına gidin ve Site Recovery kasanızı seçin.

2. Dynamics AX için oluşturduğunuz kurtarma planı seçin.

3. **Yük Devretme Testi**'ni seçin.

4. Test yük devretme işlemini başlatmak için sanal ağ seçin.

5. İkincil ortamı kurma olduktan sonra doğrulamaları gerçekleştirebilirsiniz.

6. Doğrulama tamamlandıktan sonra seçin **doğrulamaların tamamlanması** ve yük devretme test ortamı temizlenir.

Yük devretme testi gerçekleştirme hakkında daha fazla bilgi için bkz: [Site Recovery'de azure'a yük devretme testi](site-recovery-test-failover-to-azure.md).

### <a name="perform-a-failover"></a>Bir yük devretme gerçekleştirme

1. Azure portalına gidin ve Site Recovery kasanızı seçin.

2. Dynamics AX için oluşturduğunuz kurtarma planı seçin.

3. Seçin **yük devretme**seçip **yük devretme**.

4. Hedef ağ seçip **✓** yük devretme işlemini başlatmak için.

Bir yük devretme yaptığınızda daha fazla bilgi için bkz: [Site recovery'de yük devretme](site-recovery-failover.md).

### <a name="perform-a-failback"></a>Yeniden çalışma gerçekleştirme

Konuları için belirli SQL Server için yeniden çalışma sırasında bkz [çoğaltma SQL Server ve Azure Site Recovery ile uygulamaları](site-recovery-sql.md).

1. Azure portalına gidin ve Site Recovery kasanızı seçin.

2. Dynamics AX için oluşturduğunuz kurtarma planı seçin.

3. Seçin **yük devretme**seçip **yük devretme**.

4. Seçin **yönünü değiştirme**.

5. Uygun seçenekleri belirtin: veri eşitleme ve VM oluşturma.

6. Seçin **✓** yeniden çalışma işlemini başlatmak için.


Bir yeniden çalışma yapmak daha fazla bilgi için bkz: [azure'dan şirket içine yeniden çalışma VMware Vm'lerinde](site-recovery-failback-azure-to-vmware.md).

## <a name="summary"></a>Özet
Site RECOVERY'yi kullanarak, Dynamics AX uygulamanız için bir tam otomatik bir olağanüstü durum kurtarma planı oluşturabilirsiniz. Bir kesinti durumunda her yerden saniyeler içinde yük devretme başlatın ve uygulamayı dakikalar içinde çalışmaya başlayın.

## <a name="next-steps"></a>Sonraki adımlar
Site Recovery ile kurumsal iş yüklerini koruma hakkında daha fazla bilgi edinmek için [hangi iş yüklerini koruyabilirim?](site-recovery-workload.md).
