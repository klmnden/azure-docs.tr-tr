---
title: "Azure Automation’da rol tabanlı erişim denetimi | Microsoft Belgeleri"
description: "Rol tabanlı erişim denetimi (RBAC), Azure kaynakları için erişim yönetimi sağlar. Bu makalede, Azure Automation’da RBAC’nin nasıl ayarlanacağı açıklanmaktadır."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
keywords: "otomasyon rbac, rol tabanlı erişim denetimi, azure rbac"
ms.assetid: 04b5625e-0ee8-4b5b-85cd-7734c1b3d4a3
ms.service: automation
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/12/2016
ms.author: magoedte;sngun
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 9fff24dfd2b20a785c6046b6c9700b583c309de4


---
# <a name="rolebased-access-control-in-azure-automation"></a>Azure Automation’da Rol Tabanlı Erişim Denetimi
## <a name="rolebased-access-control"></a>Rol tabanlı erişim denetimi
Rol tabanlı erişim denetimi (RBAC), Azure kaynakları için erişim yönetimi sağlar. [RBAC](../active-directory/role-based-access-control-configure.md) kullanarak, ekibiniz içinde görevleri ayırabilir, bu işlere gerek duyan kişilere, gruplara ve uygulamalara da sadece erişim miktarını verebilirsiniz. Kullanıcılara rol tabanlı erişim Azure portalı, Azure Komut Satırı araçları ve Azure Management API’leri kullanılarak verilebilir.

## <a name="rbac-in-automation-accounts"></a>Automation Hesaplarında RBAC
Azure Automation’da, otomasyon hesabı kapsamında kullanıcılara, gruplara ve uygulamalara uygun RBAC rolü atanarak erişim verilir. Aşağıda Automation hesabının desteklediği yerleşik roller bulunmaktadır:  

| **Rol** | **Açıklama** |
|:--- |:--- |
| Sahip |Sahip rolü, Otomasyon hesabını yönetmek için diğer kullanıcılar, gruplara ve uygulamalara erişim sağlamak da dahil, Otomasyon hesabı içindeki tüm kaynaklara ve işlemlere erişim sağlar. |
| Katılımcı |Katılımcı rolü, başka kullanıcının Otomasyon hesabına erişim izinlerini değiştirme dışında her şeyi yönetmenizi sağlar. |
| Okuyucu |Okuyucu rolü, Otomasyon hesabında tüm kaynakları görmenizi sağlar; ancak değişiklik yapamazsınız. |
| Otomasyon Operatörü |Otomasyon Operatörü rolü, başlatma, durdurma, askıya alma, sürdürme ve zamanlama işleri gibi işletimsel görevleri gerçekleştirmenizi sağlar. Bu rol, kimlik bilgileri varlıkları ve runbook'ları gibi Automation hesabı kaynaklarınızın görüntülenmesini veya değiştirilmesini engellemek, ancak yine de kuruluş üyelerinin bu runbook’ları yürütmesine izin vermek istiyorsanız yararlıdır. |
| Kullanıcı Erişimi Yöneticisi |Kullanıcı Erişimi Yöneticisi rolü, Azure Otomasyonu hesaplarına kullanıcı erişimini yönetmenizi sağlar. |

> [!NOTE]
> Belirli bir runbook’a veya runbook’lara erişim hakları veremezsiniz, yalnızca Otomasyon hesabındaki kaynaklara ve eylemlere verebilirsiniz.  
> 
> 

Bu makalede, Azure Automation’da RBAC ayarlamanıza yardımcı olacağız. Ancak, herhangi bir kişiye Otomasyon hesabı haklarını vermeden önce iyi anlamak amacıyla ilk olarak Katkı Yapan, Okuyucu, Otomasyon Operatörü ve Kullanıcı Erişimi Yöneticisi’ne verilen ayrı izinlere yakından bakalım.  Aksi takdirde istenmeyen sonuçlara neden olabilir.     

## <a name="contributor-role-permissions"></a>Katkıda Bulunan rol izinleri
Aşağıdaki tabloda Otomasyon’daki Katkıda Bulunan rolü tarafından gerçekleştirilebilecek belirli eylemler sunulmaktadır.

| **Kaynak Türü** | **Okuma** | **Yazma** | **Silme** | **Diğer Eylemler** |
|:--- |:--- |:--- |:--- |:--- |
| Azure Otomasyonu Hesabı |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | |
| Otomasyon Sertifika Varlığı |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | |
| Otomasyon Bağlantı Varlığı |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | |
| Otomasyon Bağlantı Türü Varlığı |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | |
| Otomasyon Kimlik Bilgisi Varlığı |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | |
| Otomasyon Zamanlama Varlığı |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | |
| Otomasyon Değişken Varlığı |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | |
| Otomasyon İstenen Durum Yapılandırması | | | |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) |
| Karma Runbook Çalışanı Kaynak Türü |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | |
| Azure Otomasyonu İşi |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) |
| Otomasyon İş Akışı |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Otomasyon İş Zamanlaması |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | |
| Otomasyon Modülü |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | |
| Azure Otomasyonu Runbook |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) |
| Otomasyon Runbook Taslağı |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | | |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) |
| Otomasyon Runbook Taslağı Test İşi |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) |
| Otomasyon Web Kancası |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) |

## <a name="reader-role-permissions"></a>Okuyucu rol izinleri
Aşağıdaki tabloda Otomasyon’daki Okuyucu rolü tarafından gerçekleştirilebilecek belirli eylemler sunulmaktadır.

| **Kaynak Türü** | **Okuma** | **Yazma** | **Silme** | **Diğer Eylemler** |
|:--- |:--- |:--- |:--- |:--- |
| Klasik abonelik yöneticisi |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Yönetim kilidi |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | | | |
| İzin |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Sağlayıcı işlemleri |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Rol ataması |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Rol tanımı |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | | | |

## <a name="automation-operator-role-permissions"></a>Otomasyon Operatörü rol izinleri
Aşağıdaki tabloda Otomasyon’daki Otomasyon Operatörü rolü tarafından gerçekleştirilebilecek belirli eylemler sunulmaktadır.

| **Kaynak Türü** | **Okuma** | **Yazma** | **Silme** | **Diğer Eylemler** |
|:--- |:--- |:--- |:--- |:--- |
| Azure Otomasyonu Hesabı |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Otomasyon Sertifika Varlığı | | | | |
| Otomasyon Bağlantı Varlığı | | | | |
| Otomasyon Bağlantı Türü Varlığı | | | | |
| Otomasyon Kimlik Bilgisi Varlığı | | | | |
| Otomasyon Zamanlama Varlığı |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | | |
| Otomasyon Değişken Varlığı | | | | |
| Otomasyon İstenen Durum Yapılandırması | | | | |
| Karma Runbook Çalışanı Kaynak Türü | | | | |
| Azure Otomasyonu İşi |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) |
| Otomasyon İş Akışı |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Otomasyon İş Zamanlaması |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | | |
| Otomasyon Modülü | | | | |
| Azure Otomasyonu Runbook |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Otomasyon Runbook Taslağı | | | | |
| Otomasyon Runbook Taslağı Test İşi | | | | |
| Otomasyon Web Kancası | | | | |

Daha fazla bilgi için [Otomasyon operatörü eylemleri](../active-directory/role-based-access-built-in-roles.md#automation-operator), Otomasyon hesabı ve kaynaklarındaki Otomasyon operatörü rolünün desteklediği eylemleri listelemektedir.

## <a name="user-access-administrator-role-permissions"></a>Kullanıcı Erişimi Yöneticisi rol izinleri
Aşağıdaki tabloda Otomasyon’daki Kullanıcı Erişimi Yöneticisi rolü tarafından gerçekleştirilebilecek belirli eylemler sunulmaktadır.

| **Kaynak Türü** | **Okuma** | **Yazma** | **Silme** | **Diğer Eylemler** |
|:--- |:--- |:--- |:--- |:--- |
| Azure Otomasyonu Hesabı |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Otomasyon Sertifika Varlığı |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Otomasyon Bağlantı Varlığı |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Otomasyon Bağlantı Türü Varlığı |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Otomasyon Kimlik Bilgisi Varlığı |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Otomasyon Zamanlama Varlığı |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Otomasyon Değişken Varlığı |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Otomasyon İstenen Durum Yapılandırması | | | | |
| Karma Runbook Çalışanı Kaynak Türü |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Azure Otomasyonu İşi |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Otomasyon İş Akışı |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Otomasyon İş Zamanlaması |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Otomasyon Modülü |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Azure Otomasyonu Runbook |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Otomasyon Runbook Taslağı |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Otomasyon Runbook Taslağı Test İşi |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Otomasyon Web Kancası |![Yeşil Durum](media/automation-role-based-access-control/green-checkmark.png) | | | |

## <a name="configure-rbac-for-your-automation-account-using-azure-portal"></a>Azure Portal kullanarak Automation Hesabınız için RBAC yapılandırma
1. [Azure Portal](https://portal.azure.com/)’da oturum açın Automation Hesapları dikey penceresinden Automation hesabınızı açın.  
2. Sağ üst köşedeki **Erişim** denetimine tıklayın. Bu işlem, Automation hesabınızı yönetmek için yeni kullanıcı, grup ve uygulama ekleyebileceğiniz, Automation Hesabı için yapılandırılabilen mevcut rolleri görüntüleyebildiğiniz **Kullanıcılar** dikey penceresini açar.  
   
   ![Erişim düğmesi](media/automation-role-based-access-control/automation-01-access-button.png)  

> [!NOTE]
> **Abonelik yöneticileri** zaten varsayılan kullanıcı olarak vardır. Abonelik yöneticileri Active Directory grubu, Azure aboneliğinizin hizmet yöneticileri ve ortak yöneticilerini de kapsar. Hizmet yöneticisi Azure aboneliğinizi ve kaynaklarının sahibidir; otomasyon hesapları için sahip rolünü de devralacaktır. Bu da, erişimin bir aboneliğe ait **hizmet yöneticileri ve ortak yöneticiler** için **Devralınmış** olduğu, tüm diğer kullanıcılar için de **Atanmış** olduğu anlamına gelir. İzinleri hakkında daha ayrıntılı bilgi görmek için **Abonelik yöneticileri**’ne tıklayın.  
> 
> 

### <a name="add-a-new-user-and-assign-a-role"></a>Yeni kullanıcı ekleme ve rol atama
1. Kullanıcılar dikey penceresinden, kullanıcı, grup veya uygulama ekleyip bunları bir role atayabildiğiniz **Erişim ekle** dikey penceresini açmak için **Ekle**’ye tıklayın.  
   
   ![Kullanıcı ekle](media/automation-role-based-access-control/automation-02-add-user.png)  
2. Kullanılabilir roller listesinden bir rol seçin. **Okuyucu** rolünü seçeceğiz, ancak Automation Hesabı’nın desteklediği herhangi bir rolü veya tanımlamış olduğunuz özel bir rolü de seçebilirsiniz.  
   
   ![Rol seç](media/automation-role-based-access-control/automation-03-select-role.png)  
3. **Kullanıcı ekle** dikey penceresini açmak için **Kullanıcı Ekle**’ye tıklayın. Aboneliğinizi yönetmek için herhangi bir kullanıcı, grup veya uygulama eklediyseniz, bu kullanıcılar listelenir ve erişim eklemek üzere bunları seçebilirsiniz. Listede hiç kullanıcı yoksa veya eklemekle ilgilendiğiniz kullanıcı listede yoksa, Outlook.com, OneDrive veya Xbox Live kimlikleri gibi geçerli bir Microsoft hesabı e-posta adresiyle kullanıcı davet edebildiğiniz **Konuk davet et** dikey penceresini açmak için **davet**’e tıklayın. Kullanıcının e-posta adresini girdikten sonra kullanıcı eklemek için **Seç**’e, ardından da **Tamam**’a tıklayın. 
   
   ![Kullanıcı ekle](media/automation-role-based-access-control/automation-04-add-users.png)  
   
   Atanan **Okuyucu** rolüyle, **Kullanıcılar** dikey penceresine eklenen kullanıcıları artık görmelisiniz.  
   
   ![Kullanıcıları listele](media/automation-role-based-access-control/automation-05-list-users.png)  
   
   Kullanıcıya **Roller** dikey penceresinden rol atayabilirsiniz. 
4. Kullanıcılar dikey penceresinden, **Roller** dikey penceresini açmak için **Roller**’e tıklayın. Bu dikey pencereden rolün adını, bu role atanan kullanıcıların ve grupların sayısını görebilirsiniz.
   
    ![Kullanıcılar dikey penceresinden rol ata](media/automation-role-based-access-control/automation-06-assign-role-from-users-blade.png)  
   
   > [!NOTE]
   > Rol tabanlı erişim denetimi yalnızca Automation Hesabı düzeyinde ayarlanabilir; Automation Hesabı altındaki kaynaklarda ayarlanamaz.
   > 
   > 
   
    Bir kullanıcı, grup veya uygulamaya birden çok rol atayabilirsiniz. Örneğin, kullanıcıya **Automation Operatörü** rolünü **Okuyucu** rolüyle birlikte eklersek hem tüm Automation kaynaklarını görüntüleyebilir, hem de runbook işlerini yürütebilirler. Kullanıcıya atanan rollerin listesini görüntülemek için açılan listeyi genişletebilirsiniz.  
   
    ![Birden çok rol görüntüle](media/automation-role-based-access-control/automation-07-view-multiple-roles.png)  

### <a name="remove-a-user"></a>Kullanıcıyı kaldırma
Automation hesabı yönetmeyen veya artık kuruluş için çalışmayan kullanıcının erişim iznini kaldırabilirsiniz. Aşağıdaki adımları kullanarak kullanıcı kaldırabilirsiniz: 

1. **Kullanıcılar** dikey penceresinde kaldırmak istediğiniz rol atamasını seçin.
2. Atama ayrıntıları dikey penceresinde **Kaldır** düğmesine tıklayın.
3. Kaldırmayı onaylamak için **Evet**'e tıklayın. 
   
   ![Kullanıcıları kaldır](media/automation-role-based-access-control/automation-08-remove-users.png)  

## <a name="role-assigned-user"></a>Rol Atanmış Kullanıcı
Role atanmış kullanıcı kendi Automation hesabında oturum açtığında, artık **Varsayılan Dizinler** listesinde listelenen sahibin hesabını görebilir. Eklenen Automation hesabını görüntülemek amacıyla varsayılan dizini sahibin varsayılan dizinine geçirmelidir.  

![Varsayılan dizin](media/automation-role-based-access-control/automation-09-default-directory-in-role-assigned-user.png)  

### <a name="user-experience-for-automation-operator-role"></a>Automation operatörü rolü için kullanıcı deneyimi
Otomasyon operatörü rolüne atanan kullanıcı atandığı Otomasyon hesabını görüntülediğinde, yalnızca runbook'lar listesini, runbook işlerini ve Otomasyon hesabında oluşturulan zamanlamalar listesini görüntüleyebilse de, tanımlarını görüntüleyemez. Runbook işini başlatabilir, durdurabilir, askıya alabilir, sürdürebilir veya zamanlayabilirler. Yapılandırmalar, karma çalışan grupları veya DSC düğümleri gibi diğer Automation kaynaklarına kullanıcının erişimi yoktur.  

![Kaynaklara erişim yok](media/automation-role-based-access-control/automation-10-no-access-to-resources.png)  

Kullanıcı runbook'a tıkladığında, Automation operatörü rolü erişilmelerini izin vermediğinden kaynağı görüntüleyecek veya runbook’u düzenleyecek komutlar sağlanmaz.  

![Runbook'u düzenleyecek erişim yok](media/automation-role-based-access-control/automation-11-no-access-to-edit-runbook.png)  

Kullanıcının zamanlama görüntülemek ve oluşturmak için erişimi olsa da, başka türden varlık türlerine erişimi olmayacaktır.  

![Varlıklara erişim yok](media/automation-role-based-access-control/automation-12-no-access-to-assets.png)  

Bu kullanıcının runbook’la ilişkili web kancalarını da görüntüleme erişimi yoktur

![Web kancalarına erişim yok](media/automation-role-based-access-control/automation-13-no-access-to-webhooks.png)  

## <a name="configure-rbac-for-your-automation-account-using-azure-powershell"></a>Azure PowerShell kullanarak Automation Hesabınız için RBAC yapılandırma
Rol tabanlı erişim, aşağıdaki [Azure PowerShell cmdlet'leri](../active-directory/role-based-access-control-manage-access-powershell.md) kullanılarak Automation hesabında yapılandırılabilir.

• [Get-AzureRmRoleDefinition](https://msdn.microsoft.com/library/mt603792.aspx), Azure Active Directory’de kullanılabilen tüm RBAC rollerini listeler. Belirli bir rol tarafından gerçekleştirilebilen tüm eylemleri listelemek için bu komutu **Ad** özelliğiyle birlikte bu komutu kullanabilirsiniz.  
    **Örnek:**  
    ![Rol tanımı al](media/automation-role-based-access-control/automation-14-get-azurerm-role-definition.png)  

• [Get-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt619413.aspx), Azure AD RBAC rolü atamalarını belirtilen kapsamda listeler. Hiçbir parametre olmadan, bu komut abonelik altında yapılan tüm rol atamalarını döndürür. Belirli kullanıcıların yanı sıra bu kullanıcıların üyesi olduğu gruplara da erişim atamalarını listelemek için **ExpandPrincipalGroups** parametresini kullanın.  
    **Örnek:** Otomasyon hesabı içinde tüm kullanıcıları ve rollerini listelemek için aşağıdaki komutu kullanın.

    Get-AzureRMRoleAssignment -scope “/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation Account Name>” 

![Rol ataması al](media/automation-role-based-access-control/automation-15-get-azurerm-role-assignment.png)

• [New-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt603580.aspx), belirli bir kapsamda kullanıcılara, gruplara veya uygulamalara erişim atamak için.  
    **Örnek:** Otomasyon Hesabı kapsamındaki bir kullanıcı için yeni "Otomasyon Operatörü" rolünü atamak için aşağıdaki komutu kullanın.

    New-AzureRmRoleAssignment -SignInName <sign-in Id of a user you wish to grant access> -RoleDefinitionName "Automation operator" -Scope “/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation Account Name>”  

![Yeni rol ataması](media/automation-role-based-access-control/automation-16-new-azurerm-role-assignment.png)

• [Remove-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt603781.aspx); belirli bir kapsamda belirtilen kullanıcıya, gruba veya uygulamaya olan erişimi kaldırmak için.  
    **Örnek:** Kullanıcıyı Otomasyon Hesabı kapsamındaki “Otomasyon Operatörü” rolünden kaldırmak için aşağıdaki komutu kullanın.

    Remove-AzureRmRoleAssignment -SignInName <sign-in Id of a user you wish to remove> -RoleDefinitionName "Automation Operator" -Scope “/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation Account Name>”

Yukarıdaki örneklerde **oturum açma adını**, **abonelik kimliğini**, **kaynak grubu adını** ve **Otomasyon hesabı adını** hesap ayrıntılarınızla değiştirin. Kullanıcı rolü atamasını kaldırmak için devam etmeden önce onaylamanız istendiğin **Evet**’i seçin.   

## <a name="next-steps"></a>Sonraki Adımlar
* Azure Otomasyonu’nda RBAC yapılandırmak için çeşitli yollar hakkında daha fazla bilgi için bkz. [Azure PowerShell ile RBAC yönetme](../active-directory/role-based-access-control-manage-access-powershell.md).
* Runbook başlatmak için çeşitli yollar hakkında daha fazla ayrıntı için bkz. [runbook başlatma](automation-starting-a-runbook.md)
* Farklı runbook türleri hakkında daha fazla bilgi için bkz. [Azure Otomasyonu runbook türleri](automation-runbook-types.md)




<!--HONumber=Nov16_HO2-->


