<properties 
   pageTitle="Azure Automation’da Rol Tabanlı Erişim Denetimi | Microsoft Azure"
   description="Rol tabanlı erişim denetimi (RBAC), Azure kaynakları için erişim yönetimi sağlar. Bu makalede, Azure Automation’da RBAC’nin nasıl ayarlanacağı açıklanmaktadır."
   services="automation"
   documentationCenter=""
   authors="SnehaGunda"
   manager="stevenka"
   editor="tysonn"
   keywords="automation rbac, role based access control, azure rbac" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/10/2016"
   ms.author="magoedte;sngun"/>

# Azure Automation’da Rol Tabanlı Erişim Denetimi

## Rol tabanlı erişim denetimi

Rol tabanlı erişim denetimi (RBAC), Azure kaynakları için erişim yönetimi sağlar. [RBAC](../active-directory/role-based-access-control-configure.md) kullanarak, ekibiniz içinde görevleri ayırabilir, bu işlere gerek duyan kişilere, gruplara ve uygulamalara da sadece erişim miktarını verebilirsiniz. Kullanıcılara rol tabanlı erişim Azure portalı, Azure Komut Satırı araçları ve Azure Management API’leri kullanılarak verilebilir.

## Automation Hesaplarında RBAC

Azure Automation’da, otomasyon hesabı kapsamında kullanıcılara, gruplara ve uygulamalara uygun RBAC rolü atanarak erişim verilir. Aşağıda Automation hesabının desteklediği yerleşik roller bulunmaktadır:  

|**Rol** | **Açıklama** |
|:--- |:---|
| Sahip | Sahip rolü, Automation hesabını yönetmek için diğer kullanıcılar, gruplara ve uygulamalara erişim sağlamak da dahil, Automation hesabı içindeki tüm kaynaklara ve işlemlere erişim sağlar. |
| Katılımcı | Katılımcı rolü, başka kullanıcının Automation hesabına erişim izinlerini değiştirme dışında her şeyi yönetmenizi sağlar. |
| Okuyucu | Okuyucu rolü, Automation hesabında tüm kaynakları görmenizi sağlar; ancak değişiklik yapamazsınız. |
| Automation operatörü | Automation operatörü rolü, başlatma, durdurma, askıya alma, sürdürme ve zamanlama işleri gibi işletimsel görevleri gerçekleştirmenizi sağlar. Bu rol, kimlik bilgileri varlıkları ve runbook'ları gibi Automation hesabı kaynaklarınızın görüntülenmesini veya değiştirilmesini engellemek, ancak yine de kuruluş üyelerinin bu runbook’ları yürütmesine izin vermek istiyorsanız yararlıdır. [Automation operatörü eylemleri](../active-directory/role-based-access-built-in-roles.md#automation-operator), Automation hesabı ve kaynaklarındaki Automation operatörü rolünün desteklediği eylemlerin listesidir. |
| Kullanıcı erişimi yöneticisi | Kullanıcı erişimi yöneticisi rolü, Azure Automation hesaplarına kullanıcı erişimini yönetmenizi sağlar. |

Bu makalede, Azure Automation’da RBAC ayarlamanıza yardımcı olacağız. 

## Azure Portal kullanarak Automation Hesabınız için RBAC yapılandırma

1.  [Azure Portal](https://portal.azure.com/)’da oturum açın Automation Hesapları dikey penceresinden Automation hesabınızı açın.  

2.  Sağ üst köşedeki **Erişim** denetimine tıklayın. Bu işlem, Automation hesabınızı yönetmek için yeni kullanıcı, grup ve uygulama ekleyebileceğiniz, Automation Hesabı için yapılandırılabilen mevcut rolleri görüntüleyebildiğiniz **Kullanıcılar** dikey penceresini açar.  

    ![Erişim düğmesi](media/automation-role-based-access-control/automation-01-access-button.png)  

>[AZURE.NOTE]  **Abonelik yöneticileri** zaten varsayılan kullanıcı olarak vardır. Abonelik yöneticileri Active Directory grubu, Azure aboneliğinizin hizmet yöneticileri ve ortak yöneticilerini de kapsar. Hizmet yöneticisi Azure aboneliğinizi ve kaynaklarının sahibidir; otomasyon hesapları için sahip rolünü de devralacaktır. Bu da, erişimin bir aboneliğe ait **hizmet yöneticileri ve ortak yöneticiler** için **Devralınmış** olduğu, tüm diğer kullanıcılar için de **Atanmış** olduğu anlamına gelir. İzinleri hakkında daha ayrıntılı bilgi görmek için **Abonelik yöneticileri**’ne tıklayın.  

### Yeni kullanıcı ekleme ve rol atama

1.  Kullanıcılar dikey penceresinden, kullanıcı, grup veya uygulama ekleyip bunları bir role atayabildiğiniz **Erişim ekle** dikey penceresini açmak için **Ekle**’ye tıklayın.  

    ![Kullanıcı ekle](media/automation-role-based-access-control/automation-02-add-user.png)  

2.  Kullanılabilir roller listesinden bir rol seçin. **Okuyucu** rolünü seçeceğiz, ancak Automation Hesabı’nın desteklediği herhangi bir rolü veya tanımlamış olduğunuz özel bir rolü de seçebilirsiniz.  

    ![Rol seç](media/automation-role-based-access-control/automation-03-select-role.png)  

3.  **Kullanıcı ekle** dikey penceresini açmak için **Kullanıcı Ekle**’ye tıklayın. Aboneliğinizi yönetmek için herhangi bir kullanıcı, grup veya uygulama eklediyseniz, bu kullanıcılar listelenir ve erişim eklemek üzere bunları seçebilirsiniz. Listede hiç kullanıcı yoksa veya eklemekle ilgilendiğiniz kullanıcı listede yoksa, Outlook.com, OneDrive veya Xbox Live kimlikleri gibi geçerli bir Microsoft hesabı e-posta adresiyle kullanıcı davet edebildiğiniz **Konuk davet et** dikey penceresini açmak için **davet**’e tıklayın. Kullanıcının e-posta adresini girdikten sonra kullanıcı eklemek için **Seç**’e, ardından da **Tamam**’a tıklayın. 

    ![Kullanıcı ekle](media/automation-role-based-access-control/automation-04-add-users.png)  
 
Atanan **Okuyucu** rolüyle, **Kullanıcılar** dikey penceresine eklenen kullanıcıları artık görmelisiniz.  

![Kullanıcıları listele](media/automation-role-based-access-control/automation-05-list-users.png)  

Kullanıcıya **Roller** dikey penceresinden rol atayabilirsiniz. Kullanıcılar dikey penceresinden, **Roller** dikey penceresini açmak için **Roller**’e tıklayın. Bu dikey pencereden rolün adını, bu role atanan kullanıcıların ve grupların sayısını görebilirsiniz.

![Kullanıcılar dikey penceresinden rol ata](media/automation-role-based-access-control/automation-06-assign-role-from-users-blade.png)  
   
>[AZURE.NOTE] Rol tabanlı erişim denetimi yalnızca Automation Hesabı düzeyinde ayarlanabilir; Automation Hesabı altındaki kaynaklarda ayarlanamaz.

Bir kullanıcı, grup veya uygulamaya birden çok rol atayabilirsiniz. Örneğin, kullanıcıya **Automation Operatörü** rolünü **Okuyucu** rolüyle birlikte eklersek hem tüm Automation kaynaklarını görüntüleyebilir, hem de runbook işlerini yürütebilirler. Kullanıcıya atanan rollerin listesini görüntülemek için açılan listeyi genişletebilirsiniz.  

![Birden çok rol görüntüle](media/automation-role-based-access-control/automation-07-view-multiple-roles.png)  
 
### Kullanıcıyı kaldırma

Automation hesabı yönetmeyen veya artık kuruluş için çalışmayan kullanıcının erişim iznini kaldırabilirsiniz. Aşağıdaki adımları kullanarak kullanıcı kaldırabilirsiniz: 

1.  **Kullanıcılar** dikey penceresinde kaldırmak istediğiniz rol atamasını seçin.

2.  Atama ayrıntıları dikey penceresinde **Kaldır** düğmesine tıklayın.

3.  Kaldırmayı onaylamak için **Evet**'e tıklayın. 

    ![Kullanıcıları kaldır](media/automation-role-based-access-control/automation-08-remove-users.png)  

## Rol Atanmış Kullanıcı

Role atanmış kullanıcı kendi Automation hesabında oturum açtığında, artık **Varsayılan Dizinler** listesinde listelenen sahibin hesabını görebilir. Eklenen Automation hesabını görüntülemek amacıyla varsayılan dizini sahibin varsayılan dizinine geçirmelidir.  

![Varsayılan dizin](media/automation-role-based-access-control/automation-09-default-directory-in-role-assigned-user.png)  

### Automation operatörü rolü için kullanıcı deneyimi

Automation operatörü rolüne atanan kullanıcı atandığı Automation hesabını görüntülediğinde, yalnızca runbook'lar listesini, runbook işlerini ve Automation hesabında oluşturulan zamanlamalar listesini görüntüleyebilse de, tanımlarını görüntüleyemez. Runbook işini başlatabilir, durdurabilir, askıya alabilir, sürdürebilir veya zamanlayabilirler. Yapılandırmalar, karma çalışan grupları veya DSC düğümleri gibi diğer Automation kaynaklarına kullanıcının erişimi yoktur.  

![Kaynaklara erişim yok](media/automation-role-based-access-control/automation-10-no-access-to-resources.png)  

Kullanıcı runbook'a tıkladığında, Automation operatörü rolü erişilmelerini izin vermediğinden kaynağı görüntüleyecek veya runbook’u düzenleyecek komutlar sağlanmaz.  

![Runbook'u düzenleyecek erişim yok](media/automation-role-based-access-control/automation-11-no-access-to-edit-runbook.png)  

Kullanıcının zamanlama görüntülemek ve oluşturmak için erişimi olsa da, başka türden varlık türlerine erişimi olmayacaktır.  

![Varlıklara erişim yok](media/automation-role-based-access-control/automation-12-no-access-to-assets.png)  

Bu kullanıcının runbook’la ilişkili web kancalarını da görüntüleme erişimi yoktur

![Web kancalarına erişim yok](media/automation-role-based-access-control/automation-13-no-access-to-webhooks.png)  

## Azure PowerShell kullanarak Automation Hesabınız için RBAC yapılandırma

Rol tabanlı erişim, aşağıdaki [Azure PowerShell cmdlet'leri](../active-directory/role-based-access-control-manage-access-powershell.md) kullanılarak Automation hesabında yapılandırılabilir.

• [Get-AzureRmRoleDefinition](https://msdn.microsoft.com/library/mt603792.aspx), Azure Active Directory RBAC’de kullanılabilen tüm rolleri listeler. Tüm kullanıcıları belirli bir rolle birlikte listelemek için bu komutu **Ad** özelliğiyle birlikte kullanabilirsiniz.  
    **Örnek:**  
    ![Rol tanımı al](media/automation-role-based-access-control/automation-14-get-azurerm-role-definition.png)  

• [Get-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt619413.aspx), Azure RBAC rolü atamalarını belirtilen kapsamda listeler. Hiçbir parametre olmadan, bu komut abonelik altında yapılan tüm rol atamalarını döndürür. Belirli kullanıcıların yanı sıra bu kullanıcıların üyesi olduğu gruplara da erişim atamalarını listelemek için **ExpandPrincipalGroups** parametresini kullanın.  
    **Örnek:** Otomasyon hesabı içinde tüm kullanıcıları ve rollerini listelemek için aşağıdaki komutu kullanın.

    Get-AzureRMRoleAssignment -scope “/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation Account Name>” 

![Rol ataması al](media/automation-role-based-access-control/automation-15-get-azurerm-role-assignment.png)

• [New-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt603580.aspx), belirli bir kapsamda kullanıcılara, gruplara veya uygulamalara erişim vermek için.  
    **Örnek:** Automation hesabı kapsamındaki bir kullanıcı için yeni "Automation Operatör" rolü oluşturmak için aşağıdaki komutu kullanın.

    New-AzureRmRoleAssignment -SignInName <sign-in Id of a user you wish to grant access> -RoleDefinitionName "Automation operator" -Scope “/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation Account Name>”  

![Yeni rol ataması](media/automation-role-based-access-control/automation-16-new-azurerm-role-assignment.png)

• [Remove-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt603781.aspx); belirli bir kapsamda belirtilen kullanıcıya, gruba veya uygulamaya olan erişimi kaldırmak için.
    **Örnek:** Automation hesabı kapsamındaki bir kullanıcı için yeni "Automation Operatör" rolü oluşturmak için aşağıdaki komutu kullanın.

    Remove-AzureRmRoleAssignment -SignInName "<sign-in Id of a user you wish to remove>" -RoleDefinitionName "Automation Operator" -Scope “/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation Account Name>”

Yukarıdaki cmdlet'lerde, oturum açma adını, abonelik kimliğini, kaynak grubu adını ve Automation hesabı adını hesap ayrıntılarınızla değiştirin. Rol atamasını silmek için devam etmeniz istendiğinde **evet**’i seçin.   


## Sonraki Adımlar
-  Azure Automation’da RBAC yapılandırmak için çeşitli yollar hakkında daha fazla bilgi için bkz. [Azure PowerShell ile RBAC yönetme](../active-directory/role-based-access-control-manage-access-powershell.md).
- Runbook başlatmak için çeşitli yollar hakkında daha fazla ayrıntı için bkz. [runbook başlatma](automation-starting-a-runbook.md)
- Farklı türler hakkında daha fazla bilgi için bkz. [Azure Automation runbook türleri](automation-runbook-types.md)




<!----HONumber=Jun16_HO2-->


