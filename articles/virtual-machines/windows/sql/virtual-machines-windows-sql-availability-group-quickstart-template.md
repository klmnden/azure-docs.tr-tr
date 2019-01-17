---
title: WSFC, oluşturma, dinleyici ve ILB Azure Hızlı Başlangıç şablonu ile bir SQL Server VM'de Always On kullanılabilirlik grubu için yapılandırma
description: Azure hızlı başlangıç şablonları, kümeyi oluşturmak için bir şablon kullanarak azure'da SQL Server Vm'leri için kullanılabilirlik grupları oluşturma işlemini basitleştirmek için kullanım SQL Vm'leri kümeye kattığınıxz, dinleyiciyi oluşturun ve ILB yapılandırın.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.assetid: aa5bf144-37a3-4781-892d-e0e300913d03
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/04/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 9be8717bc9b1d15a59486edf206dd0657a711c06
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2019
ms.locfileid: "54360373"
---
# <a name="create-wsfc-listener-and-configure-ilb-for-an-always-on-availability-group-on-a-sql-server-vm-with-azure-quickstart-template"></a>WSFC, oluşturma, dinleyici ve ILB Azure Hızlı Başlangıç şablonu ile bir SQL Server VM'de Always On kullanılabilirlik grubu için yapılandırma
Bu makalede, Azure hızlı başlangıç şablonları kısmen SQL Server sanal makineleri için azure'da bir Always On kullanılabilirlik grubu yapılandırmasının dağıtımını otomatikleştirmek için kullanmayı açıklar. Bu işlemde kullanılan iki Azure hızlı başlangıç şablonları vardır. 

   | Şablon | Açıklama |
   | --- | --- |
   | [101-sql-vm-ag-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-ag-setup) | Windows Yük devretme kümesi oluşturur ve SQL Server Vm'leri için birleştirir. |
   | [101-sql-vm-aglistener-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-aglistener-setup) | Kullanılabilirlik grubu dinleyicisi oluşturur ve iç Yük Dengeleyiciyi yapılandırır. |
   | &nbsp; | &nbsp; |

Kullanılabilirlik grubu yapılandırmasının diğer bölümlerini kullanılabilirlik grubunu oluşturma ve iç Load Balancer oluşturma gibi el ile yapmanız gerekir. Bu makalede, otomatik ve el ile adımlar dizisi sağlanır.
 

## <a name="prerequisites"></a>Önkoşullar 
Hızlı Başlangıç şablonlarını kullanarak bir Always On kullanılabilirlik grubunun Kurulum otomatikleştirmek için aşağıdaki önkoşulları zaten olması gerekir: 
- Bir [Azure aboneliği](https://azure.microsoft.com/free/).
- Bir kaynak grubu ile bir [etki alanı denetleyicisi](https://docs.microsoft.com/azure/architecture/reference-architectures/identity/adds-forest). 
- Bir veya daha fazla etki alanına katılmış [Vm'leri Azure çalışan SQL Server 2016 (veya üzeri) Enterprise Edition'da](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision) olan aynı kullanılabilirlik kümesi veya kullanılabilirlik bölgesinde [SQL VM kaynak sağlayıcısınakayıtlı](#register-existing-sql-vm-with-new-resource-provider).  

## <a name="register-existing-sql-vm-with-new-resource-provider"></a>Mevcut bir SQL VM ile yeni kaynak sağlayıcısını kaydetme
Bu kullanılabilirlik grubu bu yana Azure hızlı başlangıç şablonları SQL VM kaynak sağlayıcısı (Microsoft.SqlVirtualMachine) kullanan mevcut SQL Server Vm'leri SQL VM kaynak sağlayıcısı ile kayıtlı olması gerekir. Aralık 2018'den sonra bu tarih otomatik olarak kayıtlı sonra oluşturulan tüm SQL Server Vm'leri olarak SQL Server VM'nize oluşturduysanız bu adımı atlayın. Bu bölümde Azure portalını kullanarak sağlayıcıyı kaydetmek için adımları sağlar, ancak ayrıca [PowerShell](virtual-machines-windows-sql-ahb.md#powershell). 

  >[!IMPORTANT]
  > SQL Server VM kaynağınızı sürüklerseniz, görüntü sabit kodlanmış lisans ayarına geri geçer. 

1. Azure portalını açın ve gidin **tüm hizmetleri**. 
1. Gidin **abonelikleri** ve ilgi aboneliği seçin.  
1. İçinde **abonelikleri** dikey penceresinde gidin **kaynak sağlayıcısı**. 
1. Tür `sql` filtresindeki SQL ile ilgili kaynak sağlayıcıları taşıyın. 
1. Şunlardan birini seçin *kaydetme*, *yeniden kaydettirin*, veya *Unregister* için **Microsoft.SqlVirtualMachine** sağlayıcısına bağlı olarak, İstenen eylem. 

  ![Sağlayıcı değiştirme](media/virtual-machines-windows-sql-ahb/select-resource-provider-sql.png)

## <a name="step-1---create-the-wsfc-and-join-sql-server-vms-to-the-cluster-using-quickstart-template"></a>1. adım - WSFC oluşturma ve SQL Server Vm'leri Hızlı Başlangıç şablonu kullanarak kümeye ekleyin. 
SQL VM yeni kaynak sağlayıcısı ile SQL Server sanal makineleriniz üzere kaydettikten sonra SQL Server Vm'leri içine katılabilir *SqlVirtualMachineGroup*. Bu kaynak sürümü, sürüm, tam etki alanı adı, kümeyi yönetmek için AD hesapları ve bulut tanığı olarak depolama hesabı dahil olmak üzere Windows Yük devretme kümesi meta verileri tanımlar. SQL Server VM ekleme *SqlVirtualMachineGroup* Windows yük Devreme Küme Hizmeti'ni bootstraps ve SQL Server Vm'leri kümeye ekler. Bu adım ile otomatik olarak **101-sql-vm-ag-setup** Hızlı Başlangıç şablonu ve aşağıdaki adımlarla uygulanabilir:

1. Gidin [ **101-sql-vm-ag-setup** ](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-ag-setup) Hızlı Başlangıç şablonu seçip **azure'a Dağıt** Hızlı Başlangıç şablonu Azure portalındaki başlatmak için.
1. Windows Yük devretme kümesi meta verileri yapılandırmak için gerekli alanları doldurun. İsteğe bağlı alanları boş bırakılabilir.

    Aşağıdaki tabloda, şablon için gerekli değerleri gösterir: 

   | **Alan** | Değer |
   | --- | --- |
   | **Abonelik** |  SQL Server sanal makineleriniz bulunduğu abonelik. |
   |**Kaynak grubu** | SQL Server sanal makineleriniz bulunduğu kaynak grubu. | 
   |**Yük devretme kümesi adı** | Yeni Windows Yük devretme kümeniz için istenen ad. |
   | **Var olan Vm listesi** | SQL Server, kullanılabilirlik grubundaki ve bu nedenle, katılmak istediğiniz, bu yeni kümesinin parçası olacak Vm'leri. Bu değerleri boşluk ve virgül ile ayırın. (örn: SQLVM1, SQLVM2). |
   | **SQL Server sürümü** | Açılan listeden SQL Server sanal makineleriniz SQL Server sürümünü seçin. Şu anda yalnızca SQL 2016 ve 2017 SQL resimler desteklenir. |
   | **Var olan tam etki alanı adı** | Mevcut SQL Server sanal makinelerinizin içinde bulunduğu etki alanının FQDN'si. |
   | **Mevcut etki alanı hesabı** | SQL Server sysadmin erişimi olan mevcut bir etki alanı hesabı. | 
   | **Etki alanı hesabı parolası** | Yukarıda belirtilen etki alanı hesabının parolası. | 
   | **Mevcut Sql Hizmeti hesabı** | SQL Server hizmeti denetlemek için kullanılan etki alanı kullanıcı hesabı. Bu bilgileri kullanarak bulunabilir [ **SQL Server Configuration Manager**](https://docs.microsoft.com/sql/relational-databases/sql-server-configuration-manager?view=sql-server-2017). |
   | **SQL Hizmeti parola** | SQL Server hizmetini denetleyen etki alanı kullanıcı hesabı tarafından kullanılan parola. |
   | &nbsp; | &nbsp; |

1. Hüküm ve koşulları kabul ediyorsanız, yanındaki onay kutusunu işaretleyin **hüküm ve koşulları yukarıda belirtilen kabul ediyorum** seçip **satın alma** hızlı başlangıç şablon dağıtımı sonlandırmak için. 
1. Dağıtımınızı izlemek için dağıtımdan seçin **bildirimleri** çan simgesine, üst gezinti başlığı veya gidin, **kaynak grubu** Azure portalında  **Dağıtımları** içinde **ayarları** alanı ve 'Microsoft.Template' dağıtımı'nı seçin. 

## <a name="step-2---manually-create-the-availability-group"></a>2. adım: kullanılabilirlik grubunu el ile oluşturma 
Normalde, kullanarak yaptığınız gibi el ile kullanılabilirlik grubunu oluşturma [PowerShell](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell?view=sql-server-2017), [SQL Server Management Studio](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio?view=sql-server-2017) veya [Transact-SQL](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/create-an-availability-group-transact-sql?view=sql-server-2017). 

  >[!IMPORTANT]
  > Yapmak **değil** bir dinleyicisi bu tarafından otomatik olarak şu anda oluşturulamıyor **101-sql-vm-aglistener-setup** adım 4'te Hızlı Başlangıç şablonu. 

## <a name="step-3---manually-create-the-internal-load-balancer-ilb"></a>3. adım - iç yük dengeleyici (ILB) el ile oluşturma
Always On kullanılabilirlik grubu (ağ) dinleyicisi, iç Azure yük dengeleyici (ILB) gerektirir. ILB daha hızlı yük devretme ve yeniden bağlanmayı sağlayan ağ dinleyicisi "kayan" IP adresi sunar. SQL Server Vm'leri bir kullanılabilirlik grubuna varsa aynı kullanılabilirlik kümesinin parçası ve ardından bir temel yük dengeleyici kullanabilirsiniz; Aksi takdirde, bir Standard Load Balancer'ı kullanmanız gerekir.  **ILB, SQL Server VM örnekleri ile aynı sanal ağda olmalıdır.** ILB yalnızca oluşturulabilir, yapılandırmayı geri kalanını gerekir (gibi bir arka uç havuzu, sistem durumu araştırması ve Yük Dengeleme kuralları) tarafından işlenen **101-sql-vm-aglistener-setup** adım 4'te Hızlı Başlangıç şablonu. 

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
   | **Özel IP adresi** |Kullanılabilir bir IP adresi alt ağ belirtin. Küme üzerinde bir dinleyici oluşturmak için bu IP adresi kullanabilirsiniz.|
   | **Abonelik** |Birden fazla aboneliğiniz varsa, bu alanda görüntülenir. Bu kaynak ile ilişkilendirmek istediğiniz aboneliği seçin. Normalde, kullanılabilirlik grubunun tüm kaynaklarını aynı abonelik olur. |
   | **Kaynak grubu** |SQL Server örnekleri olan kaynak grubunu seçin. |
   | **Konum** |SQL Server örnekleri olan Azure konumu seçin. |
   | &nbsp; | &nbsp; |

6. **Oluştur**’u seçin. 


  >[!NOTE]
  > Standart Load Balancer ile uyumlu olacak şekilde standart bir SKU her SQL Server VM için genel IP kaynağına sahip olmalıdır. Sanal makinenizin genel IP kaynağı SKU'su belirlemek için gidin, **kaynak grubu**seçin, **genel IP adresi** istenen SQL Server VM, kaynak ve değerin altında bulun **SKU**  , **genel bakış** bölmesi. 

## <a name="step-4---create-the-ag-listener-and-configure-the-ilb-with-the-quickstart-template"></a>4. adım - AG dinleyiciyi oluşturun ve ILB ile Hızlı Başlangıç şablonu yapılandırma

Kullanılabilirlik grubu dinleyicisi oluşturun ve iç yük dengeleyici (ILB) ile otomatik olarak yapılandırma **101-sql-vm-aglistener-setup** Microsoft.SqlVirtualMachine/Sql sanal olarak hızlı başlangıç şablonu sağlar Makine grupları/kullanılabilirlik grubu dinleyicisi kaynağı. **101-sql-vm-aglistener-setup** SQL VM kaynak sağlayıcısı aracılığıyla Hızlı Başlangıç şablonu aşağıdaki eylemleri yapar:

 - Ağ ayarları küme ve ILB için yapılandırır. 
 - ILB arka uç havuzu, sistem durumu araştırması ve yük dengelemeyi yapılandırır kuralları.
 - Ağ dinleyicisi adını ve belirtilen IP adresi ile oluşturur.

ILB yapılandırmak ve /AG dinleyicisi oluşturmak için aşağıdakileri yapın:
1. Gidin [ **101-sql-vm-aglistener-setup** ](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-aglistener-setup) Hızlı Başlangıç şablonu seçip **azure'a Dağıt** Hızlı Başlangıç şablonu Azure portalındaki başlatmak için.
1. ILB yapılandırmak için gerekli alanları doldurun ve kullanılabilirlik grubu dinleyicisi oluşturun. İsteğe bağlı alanları boş bırakılabilir. 

    Aşağıdaki tabloda, şablon için gerekli değerleri gösterir: 

   | **Alan** | Değer |
   | --- | --- |
   |**Kaynak grubu** | SQL Server Vm'leri ve kullanılabilirlik grubunun bulunduğu kaynak grubu. | 
   |**Mevcut yük devretme kümesi adı** | SQL Server Vm'leri için katılan kümesinin adı. |
   | **Mevcut Sql kullanılabilirlik grubu**| SQL Server sanal makinelerinizin bir parçası olan kullanılabilirlik grubu adı. |
   | **Var olan Vm listesi** | Yukarıda belirtilen kullanılabilirlik grubunun parçası olan SQL Server Vm'leri adları. Adları boşluk ve virgül ile ayrılmalıdır (örn: SQLVM1, SQLVM2). |
   | **Var olan tam etki alanı adı** | Mevcut SQL Server sanal makinelerinizin içinde bulunduğu etki alanının FQDN'si. |
   | **Dinleyici** | DNS adı dinleyiciye atamak istersiniz. Varsayılan olarak, bu şablon adı 'aglistener' belirtiyor, ancak değiştirilebilir. |
   | **Dinleyici bağlantı noktası** | Dinleyici kullanmak istediğiniz bağlantı noktası. Genellikle, bu bağlantı noktası 1433 varsayılan bağlantı noktası olmalıdır ve bu nedenle, bu şablon tarafından belirtilen bağlantı noktası numarası budur. Varsayılan bağlantı noktasını değiştirdiyseniz ancak ardından dinleyicisi bağlantı noktası değeri yerine kullanmanız gerekir. | 
   | **Mevcut Vnet** | SQL Server Vm'leri ve ILB bulunduğu Vnet'in adı. |
   | **Mevcut alt ağı** | *Adı* iç alt ağın SQL Server sanal makinelerinizin (örn: varsayılan). Giderek bu değeri belirlenebilir, **kaynak grubu**, seçme, **vNet**u seçerek **alt ağlar** altında **ayarları**bölmesi ve değerin altında kopyalama **adı**. |
   | **Var olan bir iç yük dengeleyici** | 3. adımda oluşturulan ILB adı. |
   | **Araştırma bağlantı noktası** | ILB kullanmak istediğiniz araştırma bağlantı noktası. Şablon, varsayılan olarak 59999 kullanır ancak bu değeri değiştirilebilir. |
   | &nbsp; | &nbsp; |

1. Hüküm ve koşulları kabul ediyorsanız, yanındaki onay kutusunu işaretleyin **hüküm ve koşulları yukarıda belirtilen kabul ediyorum** seçip **satın alma** hızlı başlangıç şablon dağıtımı sonlandırmak için. 
1. Dağıtımınızı izlemek için dağıtımdan seçin **bildirimleri** çan simgesine, üst gezinti başlığı veya gidin, **kaynak grubu** Azure portalında  **Dağıtımları** içinde **ayarları** alanı ve 'Microsoft.Template' dağıtımı'nı seçin. 

  >[!NOTE]
  >Yarı aşamalardaki, dağıtım başarısız olursa, el ile yapmanız gerekir [yeni oluşturulan dinleyiciyi kaldırmak](#remove-availability-group-listener) yeniden dağıtmadan önce PowerShell kullanarak **101-sql-vm-aglistener-setup** Hızlı Başlangıç şablonu. 

## <a name="remove-availability-group-listener"></a>Kullanılabilirlik grubu dinleyicisi Kaldır
Daha sonra şablon tarafından yapılandırılan kullanılabilirlik grubu dinleyicisi kaldırmanız gerekirse, SQL VM kaynak sağlayıcısı aracılığıyla gitmeniz gerekir. Dinleyicisi SQL VM kaynak sağlayıcısı kayıtlı olduğundan, SQL Server Management Studio silmeden yeterli değildir. PowerShell kullanarak SQL VM kaynak sağlayıcısı gerçekten silinmesi gerekir. Bunun yapılması, AG dinleyici meta veriler SQL VM kaynak Sağlayıcısı'ndan kaldırır ve fiziksel kullanılabilirlik grubu dinleyicisi siler. 

Aşağıdaki kod parçacığı, hem SQL kaynak sağlayıcısı ve kullanılabilirlik grubunuzun SQL kullanılabilirlik grubu dinleyicisini siler: 

```PowerShell
# Remove the AG listener
# example: Remove-AzureRmResource -ResourceId '/subscriptions/a1a11a11-1a1a-aa11-aa11-1aa1a11aa11a/resourceGroups/SQLAG-RG/providers/Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups/Cluster/availabilitygrouplisteners/aglistener' -Force
Remove-AzureRmResource -ResourceId '/subscriptions/<SubscriptionID>/resourceGroups/<resource-group-name>/providers/Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups/<cluster-name>/availabilitygrouplisteners/<listener-name>' -Force
```
 
## <a name="known-issues-and-errors"></a>Bilinen sorunlar ve hatalar
Bu bölümde, bazı bilinen sorunlar ve olası bunların çözümü açıklanmaktadır. 

### <a name="availability-group-listener-for-availability-group-ag-name-already-exists"></a>Kullanılabilirlik grubu için kullanılabilirlik grubu dinleyicisi '\<AG-Name >' zaten var.
Ağ dinleyicisi Azure Hızlı Başlangıç şablonu zaten kullanılan seçilmiş olan kullanılabilirlik grubu dinleyicisi, kullanılabilirlik grubu içinde fiziksel olarak veya meta verileri içindeki SQL VM kaynak sağlayıcısı olarak içerir.  Kullanarak dinleyiciyi kaldırmak [PowerShell](#remove-availability-group-listener) dağıtarak önce **101-sql-vm-aglistener-setup** Hızlı Başlangıç şablonu. 

### <a name="connection-only-works-from-primary-replica"></a>Bağlantı yalnızca birincil çoğaltmadan çalışır
Bu büyük olasılıkla başarısız, davranıştır **101-sql-vm-aglistener-setup** şablon dağıtımı ILB yapılandırması tutarsız bir durumda bırakır. Arka uç havuzu kullanılabilirlik kümesi listeler ve kuralları için durum araştırması ve Yük Dengeleme kuralları için mevcut olduğunu doğrulayın. Herhangi bir şey eksikse, daha sonra ILB tutarsız bir duruma yapılandırmadır. 

Bu davranışı düzeltmek için kullanarak dinleyiciyi kaldırmak [PowerShell](#remove-availability-group-listener), Azure portalından iç Yük Dengeleyiciyi silmek ve tekrar başlangıç [3. adım](#step-3---manually-create-the-internal-load-balanced-ilb). 

### <a name="badrequest---only-sql-virtual-machine-list-can-be-updated"></a>BadRequest - yalnızca SQL sanal makine listesi güncelleştirilebilir
Dağıtırken bu hata oluşabilir **101-sql-vm-aglistener-setup** şablon dinleyicisi, SQL Server Management Studio (SSMS) aracılığıyla silindi ancak SQL VM kaynak Sağlayıcısı'ndan silinmedi. SSMS ile bir dinleyici silinmesi, meta verileri dinleyicisinin SQL VM kaynak Sağlayıcısı'ndan kaldırmaz; Dinleyici kullanarak kaynak Sağlayıcısı'ndan silinmelidir [PowerShell](#remove-availability-group-listener). 


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makalelere bakın: 

* [SQL Server sanal makinesi genel bakış](virtual-machines-windows-sql-server-iaas-overview.md)
* [SQL Server VM SSS](virtual-machines-windows-sql-server-iaas-faq.md)
* [SQL Server VM fiyatlandırma Kılavuzu](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server VM sürüm notları](virtual-machines-windows-sql-server-iaas-release-notes.md)
* [SQL Server sanal makinesi için lisanslama modelleri değiştirme](virtual-machines-windows-sql-ahb.md)



