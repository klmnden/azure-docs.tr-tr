---
title: Azure hızlı başlangıç şablonları, Azure VM'deki SQL Server Always On kullanılabilirlik grubu yapılandırmak için kullanın
description: Azure hızlı başlangıç şablonları, Windows Yük devretme kümesi oluşturma, SQL Server Vm'leri kümeye, dinleyiciyi oluşturun ve Azure'da iç Load Balancer yapılandırma için kullanın.
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
ms.date: 01/04/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: fb09d91bb3204a1ab3dc4f9df71eabd2ee7d2bd1
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58487541"
---
# <a name="use-azure-quickstart-templates-to-configure-always-on-availability-group-for-sql-server-on-an-azure-vm"></a>Azure hızlı başlangıç şablonları, Azure VM'deki SQL Server Always On kullanılabilirlik grubu yapılandırmak için kullanın
Bu makalede, Azure hızlı başlangıç şablonları kısmen SQL Server sanal makineleri için azure'da bir Always On kullanılabilirlik grubu yapılandırmasının dağıtımını otomatikleştirmek için kullanmayı açıklar. Bu işlemde kullanılan iki Azure hızlı başlangıç şablonları vardır. 

   | Şablon | Açıklama |
   | --- | --- |
   | [101-sql-vm-ag-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-ag-setup) | Windows Yük devretme kümesi oluşturur ve SQL Server Vm'leri için birleştirir. |
   | [101-sql-vm-aglistener-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-aglistener-setup) | Kullanılabilirlik grubu dinleyicisi oluşturur ve iç Yük Dengeleyiciyi yapılandırır. Bu şablon, Windows Yük devretme kümesi ile oluşturulmuş olsa bile yalnızca kullanılabilir **101-sql-vm-ag-setup** şablonu. |
   | &nbsp; | &nbsp; |

Kullanılabilirlik grubu yapılandırmasının diğer bölümlerini kullanılabilirlik grubunu oluşturma ve iç Load Balancer oluşturma gibi el ile yapmanız gerekir. Bu makalede, otomatik ve el ile adımlar dizisi sağlanır.
 

## <a name="prerequisites"></a>Önkoşullar 
Hızlı Başlangıç şablonlarını kullanarak bir Always On kullanılabilirlik grubunun Kurulum otomatikleştirmek için aşağıdaki önkoşulları zaten olması gerekir: 
- Bir [Azure aboneliği](https://azure.microsoft.com/free/).
- Bir etki alanı denetleyicisi ile bir kaynak grubu. 
- Bir veya daha fazla etki alanına katılmış [Vm'leri Azure çalışan SQL Server 2016 (veya üzeri) Enterprise Edition'da](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision) olan aynı kullanılabilirlik kümesi veya kullanılabilirlik bölgesinde [SQL VM kaynak sağlayıcısınakayıtlı](virtual-machines-windows-sql-ahb.md#register-sql-server-vm-with-sql-resource-provider).  
- (Herhangi bir varlık tarafından kullanılmaz) iki kullanılabilir IP adresleri, bir iç Load Balancer ve kullanılabilirlik Grup dinleyicisinin kullanılabilirlik grubu olarak aynı alt ağ içinde bir. Ardından var olan bir yük dengeleyici kullanılıyorsa yalnızca bir kullanılabilir IP adresi gereklidir.  

## <a name="permissions"></a>İzinler
Azure hızlı başlangıç şablonlarını kullanarak Always On kullanılabilirlik grubu yapılandırmak aşağıdaki izinler gereklidir: 

- Etki alanında 'Bilgisayar nesnesi oluşturma' iznine sahip bir varolan etki alanı kullanıcı hesabı.  Örneğin, bir etki alanı yönetici hesabı genellikle yeterli izni (örn: account@domain.com). _Bu hesap, kümeyi oluşturmak için her VM'deki yerel yönetici grubunun bir parçası olarak da olmalıdır._
- SQL Server hizmetini denetler etki alanı kullanıcı hesabı. 


## <a name="step-1---create-the-wsfc-and-join-sql-server-vms-to-the-cluster-using-quickstart-template"></a>1. adım - WSFC oluşturma ve SQL Server Vm'leri Hızlı Başlangıç şablonu kullanarak kümeye ekleyin. 
SQL VM yeni kaynak sağlayıcısı ile SQL Server sanal makineleriniz üzere kaydettikten sonra SQL Server Vm'leri içine katılabilir *SqlVirtualMachineGroups*. Meta veri sürümü, sürüm, tam etki alanı adı, hem küme hem de SQL Hizmeti yönetmek için AD hesapları ve bulut depolama hesabı dahil olmak üzere Windows Yük devretme kümesi bu kaynak tanımlar Tanık. SQL Server Vm'leri için ekleme *SqlVirtualMachineGroups* kaynak grubu kümeyi oluşturmak için Windows Yük devretme Küme hizmetinin bootstraps ve ardından bu SQL Server Vm'leri bu kümeye birleştirir. Bu adım ile otomatik olarak **101-sql-vm-ag-setup** Hızlı Başlangıç şablonu ve aşağıdaki adımlarla uygulanabilir:

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
   | **Mevcut etki alanı hesabı** | Etki alanında 'Bilgisayar nesnesi oluşturma' iznine sahip mevcut bir etki alanı kullanıcı hesabı [CNO](/windows-server/failover-clustering/prestage-cluster-adds) şablon dağıtımı sırasında oluşturulur. Örneğin, bir etki alanı yönetici hesabı genellikle yeterli izni (örn: account@domain.com). *Bu hesap, kümeyi oluşturmak için her VM'deki yerel yönetici grubunun bir parçası olarak da olmalıdır.*| 
   | **Etki alanı hesabı parolası** | Yukarıda belirtilen etki alanı kullanıcı hesabının parolası. | 
   | **Mevcut Sql Hizmeti hesabı** | Denetimleri etki alanı kullanıcı hesabı [SQL Server hizmeti](/sql/database-engine/configure-windows/configure-windows-service-accounts-and-permissions) kullanılabilirlik grubu dağıtımı sırasında (örn: account@domain.com). |
   | **SQL Hizmeti parola** | SQL Server hizmetini denetleyen etki alanı kullanıcı hesabı tarafından kullanılan parola. |
   | **Bulut tanığı adı** | Oluşturulan ve bulut tanığı için kullanılan yeni bir Azure depolama hesabı budur. Bu adı değiştirilmiş. |
   | **\_yapıtları konumu** | Bu alan, varsayılan olarak ayarlanır ve değiştirilmemelidir. |
   | **\_yapıtları konumu Sas belirteci** | Bu alan, bilerek boş bırakılır. |
   | &nbsp; | &nbsp; |

1. Hüküm ve koşulları kabul ediyorsanız, yanındaki onay kutusunu işaretleyin **hüküm ve koşulları yukarıda belirtilen kabul ediyorum** seçip **satın alma** hızlı başlangıç şablon dağıtımı sonlandırmak için. 
1. Dağıtımınızı izlemek için dağıtımdan seçin **bildirimleri** çan simgesine, üst gezinti başlığı veya gidin, **kaynak grubu** Azure portalında  **Dağıtımları** içinde **ayarları** alanı ve 'Microsoft.Template' dağıtımı'nı seçin. 

   >[!NOTE]
   > Şablon dağıtımı sırasında sağlanan kimlik bilgileri yalnızca dağıtım uzunluğu için depolanır. Dağıtım tamamlandıktan sonra bu parolaları kaldırılır ve kümeye daha fazla SQL Server Vm'leri eklemelisiniz sağlayacağını yeniden istenir. 


## <a name="step-2---manually-create-the-availability-group"></a>2. adım: kullanılabilirlik grubunu el ile oluşturma 
Normalde, kullanarak yaptığınız gibi el ile kullanılabilirlik grubunu oluşturma [SQL Server Management Studio](/sql/database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio), [PowerShell](/sql/database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell), veya [Transact-SQL](/sql/database-engine/availability-groups/windows/create-an-availability-group-transact-sql). 

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
   | **Sanal ağ** | SQL Server örnekleri olan sanal ağı seçin. |
   | **Alt ağ** | SQL Server örnekleri olan bir alt ağ seçin. |
   | **IP adresi ataması** |**Statik** |
   | **Özel IP adresi** | Kullanılabilir bir IP adresi alt ağ belirtin. |
   | **Abonelik** |Birden fazla aboneliğiniz varsa, bu alanda görüntülenir. Bu kaynak ile ilişkilendirmek istediğiniz aboneliği seçin. Normalde, kullanılabilirlik grubunun tüm kaynaklarını aynı abonelik olur. |
   | **Kaynak grubu** |SQL Server örnekleri olan kaynak grubunu seçin. |
   | **Konum** |SQL Server örnekleri olan Azure konumu seçin. |
   | &nbsp; | &nbsp; |

6. **Oluştur**’u seçin. 


  >[!IMPORTANT]
  > Standart Load Balancer ile uyumlu olacak şekilde standart bir SKU her SQL Server VM için genel IP kaynağına sahip olmalıdır. Sanal makinenizin genel IP kaynağı SKU'su belirlemek için gidin, **kaynak grubu**seçin, **genel IP adresi** istenen SQL Server VM, kaynak ve değerin altında bulun **SKU**  , **genel bakış** bölmesi. 

## <a name="step-4---create-the-ag-listener-and-configure-the-ilb-with-the-quickstart-template"></a>4. adım - AG dinleyiciyi oluşturun ve ILB ile Hızlı Başlangıç şablonu yapılandırma

Kullanılabilirlik grubu dinleyicisi oluşturun ve iç yük dengeleyici (ILB) ile otomatik olarak yapılandırma **101-sql-vm-aglistener-setup** Hızlı Başlangıç şablonu olarak Microsoft.SqlVirtualMachine/ sağlar SqlVirtualMachineGroups/AvailabilityGroupListener kaynak. **101-sql-vm-aglistener-setup** SQL VM kaynak sağlayıcısı aracılığıyla Hızlı Başlangıç şablonu aşağıdaki eylemleri yapar:

 - Dinleyici için (dağıtım sırasında sağlanan IP adresi değere göre) yeni bir ön uç IP kaynağı oluşturur. 
 - Ağ ayarları küme ve ILB için yapılandırır. 
 - ILB arka uç havuzu, sistem durumu araştırması ve yük dengelemeyi yapılandırır kuralları.
 - Ağ dinleyicisi adını ve belirtilen IP adresi ile oluşturur.
 
   >[!NOTE]
   > **101-sql-vm-aglistener-setup** yalnızca Windows Yük devretme kümesi ile oluşturulmuşsa kullanılabilir **101-sql-vm-ag-setup** şablonu.
   
   
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
   | **Dinleyici** | DNS adı dinleyiciye atamak istersiniz. Varsayılan olarak, bu şablon adı 'aglistener' belirtiyor, ancak değiştirilebilir. Ad 15 karakteri aşmamalıdır. |
   | **Dinleyici bağlantı noktası** | Dinleyici kullanmak istediğiniz bağlantı noktası. Genellikle, bu bağlantı noktası 1433 varsayılan bağlantı noktası olmalıdır ve bu nedenle, bu şablon tarafından belirtilen bağlantı noktası numarası budur. Varsayılan bağlantı noktasını değiştirdiyseniz ancak ardından dinleyicisi bağlantı noktası değeri yerine kullanmanız gerekir. | 
   | **Dinleyici IP** | IP kullanmak için dinleyici istersiniz.  Bu IP adresi, şablon dağıtımı sırasında oluşturulur, bu nedenle zaten kullanılmayan bir IP adresi sağlayın.  |
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

```powershell
# Remove the AG listener
# example: Remove-AzResource -ResourceId '/subscriptions/a1a11a11-1a1a-aa11-aa11-1aa1a11aa11a/resourceGroups/SQLAG-RG/providers/Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups/Cluster/availabilitygrouplisteners/aglistener' -Force
Remove-AzResource -ResourceId '/subscriptions/<SubscriptionID>/resourceGroups/<resource-group-name>/providers/Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups/<cluster-name>/availabilitygrouplisteners/<listener-name>' -Force
```
 
## <a name="common-errors"></a>Sık karşılaşılan hatalar
Bu bölümde, bazı bilinen sorunlar ve olası bunların çözümü açıklanmaktadır. 

### <a name="availability-group-listener-for-availability-group-ag-name-already-exists"></a>Kullanılabilirlik grubu için kullanılabilirlik grubu dinleyicisi '\<AG-Name >' zaten var.
Ağ dinleyicisi Azure Hızlı Başlangıç şablonu zaten kullanılan seçilmiş olan kullanılabilirlik grubu dinleyicisi, kullanılabilirlik grubu içinde fiziksel olarak veya meta verileri içindeki SQL VM kaynak sağlayıcısı olarak içerir.  Kullanarak dinleyiciyi kaldırmak [PowerShell](#remove-availability-group-listener) dağıtarak önce **101-sql-vm-aglistener-setup** Hızlı Başlangıç şablonu. 

### <a name="connection-only-works-from-primary-replica"></a>Bağlantı yalnızca birincil çoğaltmadan çalışır
Bu büyük olasılıkla başarısız, davranıştır **101-sql-vm-aglistener-setup** şablon dağıtımı ILB yapılandırması tutarsız bir durumda bırakır. Arka uç havuzu kullanılabilirlik kümesi listeler ve kuralları için durum araştırması ve Yük Dengeleme kuralları için mevcut olduğunu doğrulayın. Herhangi bir şey eksikse, daha sonra ILB tutarsız bir duruma yapılandırmadır. 

Bu davranışı düzeltmek için kullanarak dinleyiciyi kaldırmak [PowerShell](#remove-availability-group-listener), Azure portalından iç Yük Dengeleyiciyi silmek ve adım 3'ı yeniden başlatın. 

### <a name="badrequest---only-sql-virtual-machine-list-can-be-updated"></a>BadRequest - yalnızca SQL sanal makine listesi güncelleştirilebilir
Dağıtırken bu hata oluşabilir **101-sql-vm-aglistener-setup** şablon dinleyicisi, SQL Server Management Studio (SSMS) aracılığıyla silindi ancak SQL VM kaynak Sağlayıcısı'ndan silinmedi. SSMS ile bir dinleyici silinmesi, meta verileri dinleyicisinin SQL VM kaynak Sağlayıcısı'ndan kaldırmaz; Dinleyici kullanarak kaynak Sağlayıcısı'ndan silinmelidir [PowerShell](#remove-availability-group-listener). 

### <a name="domain-account-does-not-exist"></a>Etki alanı hesabı yok
Bu hata iki nedenden biri tarafından neden olabilir. Belirtilen etki alanı hesabı gerçekten yok veya eksik olduğu [kullanıcı asıl adı (UPN)](/windows/desktop/ad/naming-properties#userprincipalname) veri. **101-sql-vm-ag-setup** şablon UPN biçiminde bir etki alanı hesabı bekliyor (yani user@domain.com), ancak bazı etki alanı hesapları, eksik olabilir. Bu genellikle sunucunun bir etki alanı denetleyicisine yükseltildiğinde ilk etki alanı yöneticisi hesabı olması bir yerel kullanıcı geçişi veya PowerShell aracılığıyla bir kullanıcının oluşturulduğu oluşabilir. 

 Hesap mevcut olduğunu doğrulayın. Aksi halde ikinci durum çalışıyor olabilir. Bu sorunu gidermek için aşağıdakileri yapın:

1. Etki alanı denetleyicisinde açın **Active Directory Kullanıcıları ve Bilgisayarları** penceresinden **Araçları** seçeneğini **Sunucu Yöneticisi**. 
2. Seçerek hesabına gidin **kullanıcılar** sol bölmede.
3. İstenen hesabını sağ tıklatın ve seçin **özellikleri**.
4. Seçin **hesabı** doğrulayın ve sekme **kullanıcı oturum açma adı** boştur. Eğer öyleyse, hatanın nedeni budur. 

    ![Boş bir kullanıcı hesabı eksik UPN gösterir](media/virtual-machines-windows-sql-availability-group-quickstart-template/account-missing-upn.png)

5. Doldurun **kullanıcı oturum açma adı** kullanıcı adını eşleştirmek ve aşağı açılan listeden uygun etki alanını seçin. 
6. Seçin **Uygula** değişikliklerinizi kaydedip seçerek iletişim kutusunu kapatmak için **Tamam**. 

   Bu değişiklikler yapıldıktan sonra bir kez daha fazla Azure Hızlı Başlangıç şablonu dağıtmak çalışır. 



## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makalelere bakın: 

* [SQL Server sanal makinesi genel bakış](virtual-machines-windows-sql-server-iaas-overview.md)
* [SQL Server VM SSS](virtual-machines-windows-sql-server-iaas-faq.md)
* [SQL Server VM fiyatlandırma Kılavuzu](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server VM sürüm notları](virtual-machines-windows-sql-server-iaas-release-notes.md)
* [SQL Server sanal makinesi için lisanslama modelleri değiştirme](virtual-machines-windows-sql-ahb.md)



