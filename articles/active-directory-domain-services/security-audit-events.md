---
title: Azure AD etki alanı Hizmetleri için güvenlik denetimlerini etkinleştir | Microsoft Docs
description: Azure AD Domain Services için güvenlik denetimlerini etkinleştir
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 662362c3-1a5e-4e94-ae09-8e4254443697
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: iainfou
ms.openlocfilehash: 3105296b3c670d3d44789c93878fa1fc6076973b
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67566501"
---
# <a name="enable-security-audits-for-azure-ad-domain-services-preview"></a>Azure AD etki alanı Hizmetleri (Önizleme) güvenlik denetimlerini etkinleştir
Azure AD etki alanı hizmeti güvenlik denetim akışı güvenlik denetim olaylarını hedeflenen kaynaklar için Azure AD etki alanı hizmeti portalını kullanarak müşterilerin sağlar. Bu olayları alıp kaynaklar, Azure depolama, Azure Log Analytics çalışma alanına veya Azure olay hub'ı içerir. Kısa bir süre sonra güvenlik denetim olaylarını etkinleştirdikten sonra Azure AD etki alanı hizmeti Denetlenen olayları için seçilen kategori hedeflenen kaynağa gönderir. Güvenlik denetim olaylarını arşiv Denetlenen olayları müşterilere Azure depolamada etkinleştirmek. Ayrıca, müşteriler, güvenlik bilgileri ve Olay yönetimi (SIEM) yazılımını (veya eşdeğeri) event hubs kullanarak olay akışı veya kendi analiz ve Öngörüler Azure portalından Azure Log Analytics kullanarak yapın. 

> [!IMPORTANT]
> Azure AD Domain Services güvenlik denetimi, yalnızca Azure Resource Manager tabanlı örnekler için Azure AD Domain Services üzerinde kullanılabilir.
>
>

## <a name="auditing-event-categories"></a>Denetim olayı kategorileri
Azure AD Domain Services güvenlik denetimi, geleneksel için Active Directory Domain Services etki alanı denetleyicilerine gelen denetimi ile hizalar. Mevcut denetim desenleri yeniden aynı mantığı olayları analiz edilirken kullanılabilir sağlar. Azure AD Domain Services güvenlik denetimi, aşağıdaki olay kategorilerini içerir.

| Denetim kategori adı | Açıklama |
|:---|:---|
| Hesap oturumu açma|Hesap verileri bir etki alanı denetleyicisinde veya bir yerel Güvenlik Hesapları Yöneticisi (SAM) üzerinde kimlik doğrulaması yapma girişimleri denetler.</p>Belirli bir bilgisayar erişim girişimlerini, oturum açma ve kapatma ilke ayarları ve olayları izleyin. Ayarlar ve bu kategorideki olayları kullanılan hesabı veritabanı üzerinde odaklanın. Bu kategori, aşağıdaki alt kategorileri içerir:<ul><li>[Kimlik bilgisi doğrulamayı denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-credential-validation)</li><li>[Kerberos kimlik doğrulaması hizmetini denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-kerberos-authentication-service)</li><li>[Kerberos hizmet bileti işlemlerini denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-kerberos-service-ticket-operations)</li><li>[Diğer oturum açma ve kapatma olaylarını denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-logonlogoff-events)</li></ul>|
| Hesap Yönetimi|Denetimleri, kullanıcı ve bilgisayar hesapları ve grupları için değiştirir. Bu kategori, aşağıdaki alt kategorileri içerir:<ul><li>[Uygulama grubu yönetimini denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-application-group-management)</li><li>[Bilgisayar hesabı yönetimini denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-computer-account-management)</li><li>[Dağıtım grubu yönetimini denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-distribution-group-management)</li><li>[Diğer hesabı yönetimini denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-account-management-events)</li><li>[Güvenlik grubu yönetimini denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-security-group-management)</li><li>[Kullanıcı hesabı yönetimini denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-user-account-management)</li></ul>|
| İzleme Ayrıntıları|Bireysel uygulamaların ve kullanıcıların bu bilgisayarda ve bir bilgisayara nasıl kullanıldığını anlamak için etkinlikleri denetler. Bu kategori, aşağıdaki alt kategorileri içerir:<ul><li>[DPAPI etkinliğini denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-dpapi-activity)</li><li>[PNP etkinliğini denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-pnp-activity)</li><li>[İşlem oluşturmayı denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-process-creation)</li><li>[İşlem sonlandırmayı denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-process-termination)</li><li>[RPC olaylarını denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-rpc-events)</li></ul>|
| Dizin Hizmetleri erişim|Denetimleri erişmek ve Active Directory Etki Alanı Hizmetleri'nde (AD DS) nesneleri değiştirmek çalışır. Bu denetim yalnızca etki alanı denetleyicilerinde olayları günlüğe kaydedilir. Bu kategori, aşağıdaki alt kategorileri içerir:<ul><li>[Ayrıntılı dizin hizmeti çoğaltmayı denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-detailed-directory-service-replication)</li><li>[Dizin hizmeti erişimini denetle](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-directory-service-access)</li><li>[Dizin hizmeti değişikliklerini denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-directory-service-changes)</li><li>[Dizin hizmeti çoğaltmayı denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-directory-service-replication)</li></ul>|
| Oturum açma ve kapatma|Etkileşimli olarak veya bir ağ üzerinden bir bilgisayara oturum açma girişimi denetler. Bu olaylar, kullanıcı etkinliğini izleme ve ağ kaynaklarını olası saldırıları tanımlamak için yararlıdır. Bu kategori, aşağıdaki alt kategorileri içerir:<ul><li>[Hesap kilitlemeyi denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-account-lockout)</li><li>[Denetim kullanıcı/cihaz talepleri](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-user-device-claims)</li><li>[IPSec genişletilmiş modunu denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-ipsec-extended-mode)</li><li>[Denetim grubu üyeliği](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-group-membership)</li><li>[IPSec ana modunu denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-ipsec-main-mode)</li><li>[IPSec hızlı modunu denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-ipsec-quick-mode)</li><li>[Oturum kapatmayı denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-logoff)</li><li>[Oturum açmayı denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-logon)</li><li>[Ağ İlkesi sunucusunu denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-network-policy-server)</li><li>[Diğer oturum açma ve kapatma olaylarını denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-logonlogoff-events)</li><li>[Özel oturum açmayı denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-special-logon)</li></ul>|
|Nesne erişimi| Özel nesneler veya bir ağ veya bilgisayarı nesne türlerini erişmeye çalışır denetler. Bu kategori, aşağıdaki alt kategorileri içerir:<ul><li>[Oluşturulan uygulamayı denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-application-generated)</li><li>[Sertifika Hizmetlerini denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-certification-services)</li><li>[Ayrıntılı dosya paylaşımını denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-detailed-file-share)</li><li>[Dosya paylaşımını denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-file-share)</li><li>[Dosya sistemini denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-file-system)</li><li>[Platform bağlantısı filtrelemeyi denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-filtering-platform-connection)</li><li>[Platform paketi bırakma filtrelemesini denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-filtering-platform-packet-drop)</li><li>[Tanıtıcı değiştirmeyi denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-handle-manipulation)</li><li>[Çekirdek nesnesini denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-kernel-object)</li><li>[Diğer nesne erişim olaylarını denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-object-access-events)</li><li>[Kayıt defterini denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-registry)</li><li>[Çıkarılabilir Depolama Birimi denetimi](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-removable-storage)</li><li>[SAM denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-sam)</li><li>[Denetim Merkezi erişim ilkesi hazırlama](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-central-access-policy-staging)</li></ul>|
|İlke değişikliği|Önemli güvenlik ilkeleri, yerel sistem veya ağ denetimleri değişir. İlkeleri genellikle güvenli ağ kaynaklarını yardımcı olmak için yöneticiler tarafından oluşturulur. Değişiklikleri veya bu ilkeleri değiştirme girişimleri izlemek için bir ağ güvenlik yönetimi için önemli bir yönüdür olabilir. Bu kategori, aşağıdaki alt kategorileri içerir:<ul><li>[İlke değişimini denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-audit-policy-change)</li><li>[Kimlik doğrulama İlkesi değişikliğini denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-authentication-policy-change)</li><li>[Yetkilendirme İlkesi değişikliğini denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-authorization-policy-change)</li><li>[Filtre Platformu ilke değişikliğini denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-filtering-platform-policy-change)</li><li>[MPSSVC kural düzeyi ilke değişikliğini denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-mpssvc-rule-level-policy-change)</li><li>[Diğer İlke değişikliğini denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-policy-change-events)</li></ul>|
|Ayrıcalık kullanımını| Belirli bir veya daha fazla sistem izinleri kullanımını denetler. Bu kategori, aşağıdaki alt kategorileri içerir:<ul><li>[Hassas olmayan Ayrıcalık kullanımını denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-non-sensitive-privilege-use)</li><li>[Hassas Ayrıcalık kullanımını denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-sensitive-privilege-use)</li><li>[Diğer ayrıcalık kullanım olaylarını denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-privilege-use-events)</li></ul>|
|Sistem| Denetimleri sistem düzeyindeki değişiklikler diğer kategorileri ve bulunmayan bir bilgisayara, olası güvenlik uygulamalarını içerir. Bu kategori, aşağıdaki alt kategorileri içerir:<ul><li>[IPSec sürücüsünü denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-ipsec-driver)</li><li>[Diğer sistem olaylarını denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-system-events)</li><li>[Güvenlik durumu değişikliğini denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-security-state-change)</li><li>[Güvenlik sistemi uzantısını denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-security-system-extension)</li><li>[Sistem bütünlüğünü denetleme](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-system-integrity)</li></ul>|

## <a name="event-ids-per-category"></a>Kategori başına olay kimlikleri
 Azure AD etki alanı Hizmetleri güvenlik denetimi, belirli bir eylemi denetlenebilir bir olayı tetiklendiğinde, aşağıdaki olay kimliklerinden kaydeder.

| Olay kategorisi adı | Olay kimlikleri |
|:---|:---|
|Hesap oturum açma güvenliği|4767, 4774, 4775, 4776, 4777|
|Hesap Yönetimi Güvenlik|4720, 4722, 4723, 4724, 4725, 4726, 4727, 4728, 4729, 4730, 4731, 4732, 4733, 4734, 4735, 4737, 4738, 4740, 4741, 4742, 4743, 4754, 4755, 4756, 4757, 4758, 4764, 4765, 4766, 4780, 4781, 4782, 4793, 4798, 4799, 5376, 5377|
|Ayrıntılı izleme güvenlik|None|
|DS erişimi güvenliği|5136, 5137, 5138, 5139, 5141|
|Oturum açma ve kapatma güvenlik|4624, 4625, 4634, 4647, 4648, 4672, 4675, 4964|
|Nesne erişimi güvenliği|None|
|İlke değişikliği güvenlik|4670, 4703, 4704, 4705, 4706, 4707, 4713, 4715, 4716, 4717, 4718, 4719, 4739, 4864, 4865, 4866, 4867, 4904, 4906, 4911, 4912|
|Ayrıcalık kullanım güvenlik|4985|
|Sistem güvenliği|4612, 4621|

## <a name="enable-security-audit-events"></a>Güvenlik denetim olaylarını etkinleştir
Aşağıdaki yönergeler, başarılı bir şekilde Azure AD etki alanı Hizmetleri güvenlik denetim olaylarına abone olmak için yardımcı olur.

> [!IMPORTANT]
> Azure AD etki alanı Hizmetleri Güvenlik denetimleri geriye dönük değildir. Daha önce olayları almak için ya da son olayları yeniden Yürüt mümkün değil. Hizmet etkinleştirildikten sonra gerçekleşen olayları yalnızca gönderebilirsiniz.
>

### <a name="choose-the-target-resource"></a>Hedef kaynağı seçin
Güvenlik denetimleri için bir hedef kaynak olarak Azure depolama, Azure Event Hubs veya Azure Log Analytics çalışma alanları herhangi bir birleşimini kullanabilirsiniz. Aşağıdaki tabloda kullanım durumunuz için en iyi kaynak için göz önünde bulundurun.

> [!IMPORTANT]
> Azure AD Domain Services güvenlik denetimleri etkinleştirmeden önce hedef kaynak oluşturmak için ihtiyacınız.
>

| Hedef kaynak | Senaryo |
|:---|:---|
|Azure Storage|Güvenlik denetim olaylarını arşivleme amacıyla saklamak için birincil ihtiyacınız olduğunda bu hedef kullanmayı düşünün. Diğer hedeflerden arşivleme amacıyla kullanılabilir. Ancak, bu hedefleri arşivleme birincil gereken ötesinde özellikler sunar. Bir Azure depolama hesabı oluşturmak için takip [bir depolama hesabı oluşturun.](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal#create-a-storage-account-1)|
|Azure Event Hubs|Veri analizi yazılım veya güvenlik bilgileri ve Olay yönetimi (SIEM) yazılımı gibi ek yazılım güvenlik denetim olaylarını paylaşmak için birincil ihtiyacınız olduğunda bu hedef kullanmayı düşünün. Bir olay hub'ı oluşturmak için takip [hızlı başlangıç: Azure portalını kullanarak bir olay hub'ı oluşturun.](https://docs.microsoft.com/azure/event-hubs/event-hubs-create)|
|Azure Log Analytics çalışma alanı|Analiz etmek ve doğrudan Azure portalından güvenli denetimleri gözden geçirmek için birincil ihtiyacınız olduğunda bu hedef kullanmayı düşünün.  Bir Log Analytics çalışma alanı oluşturmak için takip [Azure portalında Log Analytics çalışma alanı oluşturma.](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace)|

## <a name="using-the-azure-portal-to-enable-security-audit-events"></a>Güvenlik denetim olaylarını etkinleştirmek için Azure portalını kullanarak 
1. [https://portal.azure.com](https://portal.azure.com ) adresinden Azure portalında oturum açın.  Azure portalında tüm hizmetleri Ekle'ye tıklayın. Kaynak listesinde yazın **etki alanı**. Yazmaya başladığınızda liste, girişinize göre filtrelenir. Tıklayın **Azure AD etki alanı Hizmetleri**.
2. Listeden Azure AD Domain Services örneğine tıklayın.
3. Tıklayın **tanılama ayarları (Önizleme)** soldaki Eylemler listesinden.</p>
![Tanılama ayarı eylemi](./media/security-audit-events/diagnostic-settings-action.png)
4. Tanılama yapılandırma adını yazın (**aadds denetim** örnek olarak).</p>
![Tanılama Ayarları sayfası](./media/security-audit-events/diagnostic-settings-page.png)
5. Güvenlik denetim olaylarını ile kullanacağınız hedeflenen kaynaklar yanındaki uygun onay kutusunu seçin.
    > [!NOTE]
    > Bu sayfadan hedef kaynaklar oluşturulamıyor.
    >
    
    **Azure Depolama:**</p>
    Seçin **bir depolama hesabında arşivle**. **Yapılandır**'a tıklayın. Seçin **abonelik** ve **depolama hesabı** güvenlik olaylarını denetleme arşivlemek için kullanmak istediğiniz. **Tamam**'ı tıklatın.</p>
    
    ![Tanılama depolama ayarları](./media/security-audit-events/diag-settings-storage.png)
    
    **Azure olay hub'ları:**</p>
    Seçin **Stream olay hub'ına**. **Yapılandır**'a tıklayın. İçinde **Select olay hub'ı sayfası**seçin **abonelik** olay hub'ı oluşturmak için kullanılır. Ardından, **olay hub'ı ad alanı**, **olay hub'ı adı**, ve **olay hub'ı ilke adı**. **Tamam**'ı tıklatın.</p>
    ![Tanılama Olay hub'ı ayarları](./media/security-audit-events/diag-settings-eventhub.png)
    
    **Azure günlük analizi çalışma alanları:**</p>
    Seçin **Log Analytics'e gönderme**. Seçin **abonelik** ve **Log Analytics çalışma alanı** güvenlik denetim olaylarını depolamak için kullanılır.</p>
    ![Tanılama çalışma alanı ayarları](./media/security-audit-events/diag-settings-log-analytics.png)

6. Belirli hedef kaynağı için dahil edilmesini istediğiniz günlük kategorileri seçin. Depolama hesapları kullanılıyorsa, bekletme ilkeleri yapılandırabilirsiniz.

    > [!NOTE]
    > Farklı günlük kategorileri içinde tek bir yapılandırma hedeflenen her bir kaynak seçebilirsiniz. Bu, günlükleri arşivlemek istiyorsanız, kategorileri günlükleri ve Log Analytics için tutmak istediğiniz kategorileri seçmenize olanak sağlar.
    >

7. Tıklayın **Kaydet** değişikliklerinizi işleyebilirsiniz. Hedef kaynaklar kısa bir süre sonra yapılandırmanızı kaydettikten sonra Azure AD Domain Services güvenlik denetim olaylarını alacaksınız.

## <a name="using-azure-powershell-to-enable-security-audit-events"></a>Güvenlik denetim olaylarını etkinleştirmek için Azure PowerShell'i kullanma
 
### <a name="prerequisites"></a>Önkoşullar

İçin makaledeki yönergeleri [Azure PowerShell modülünü yüklemek ve Azure aboneliğinize bağlayın](https://docs.microsoft.com/powershell/azure/install-az-ps?toc=%2fazure%2factive-directory-domain-services%2ftoc.json).

### <a name="enable-security-audits"></a>Güvenlik denetimlerini etkinleştir

1. Azure Resource Manager'a uygun Kiracı ve aboneliği kullanarak kimlik doğrulaması **Connect AzAccount** Azure PowerShell cmdlet'i.
2. Güvenlik için hedef kaynak denetim olayları oluşturun.</p>
    **Azure Depolama:**</p>
    İzleyin [depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-powershell) depolama hesabınızı oluşturmak için.</p>
    **Azure olay hub'ları:**</p>
    İzleyin [hızlı başlangıç: Azure PowerShell kullanarak bir olay hub'ı oluşturma](https://docs.microsoft.com/azure/event-hubs/event-hubs-quickstart-powershell) olay hub'ınızı oluşturmak için. Kullanmanız gerekebilir [yeni AzEventHubAuthorizationRule](https://docs.microsoft.com/powershell/module/az.eventhub/new-azeventhubauthorizationrule?view=azps-2.3.2) Active Directory AD etki alanı Hizmetleri izinleri olay hub'ına izin veren bir yetkilendirme kuralı oluşturmak için Azure PowerShell cmdlet'ini **ad alanı**. Yetkilendirme kuralı içermelidir **Yönet**, **dinleme**, ve **Gönder** hakları.
    > [!IMPORTANT]
    > Olay hub'ı ad alanı ve olay hub yetkilendirme kuralı ayarlandığından emin olun.
       
    </p>
    
    **Azure günlük analizi çalışma alanları:**</p>
    İzleyin [Azure PowerShell ile bir Log Analytics çalışma alanı oluşturma](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace-posh) çalışma alanınızı oluşturmak için.
3. Azure AD Domain Services Örneğiniz için kaynak Kimliğini alın. Bir açılan, kimliği doğrulanmış Windows PowerShell konsolunda aşağıdaki komutu yazın. Kullanım **$aadds. ResourceId** değişken gelecekteki cmdlet'leri için Azure AD Domain Services kaynak kimliği için bir parametre olarak.
    ```powershell
    $aadds = Get-AzResource -name aaddsDomainName
    ``` 
4. Kullanma **kümesi AzDiagnosticSetting** cmdlet'i, hedef kaynağı için Azure AD Domain Services güvenlik denetim olaylarını kullanmak için Azure tanılama ayarlarını yapılandırmak için. Aşağıda, ' % s'değişkeni $aadds örnekleri. ResourceId kaynak kimliği, Azure AD Domain Services örneğinizin temsil eder (bkz. 3. adım).</p>
    **Azure Depolama:**
    ```powershell
    Set-AzDiagnosticSetting `
    -ResourceId $aadds.ResourceId` 
    -StorageAccountId storageAccountId `
    -Enabled $true
    ```
    Değiştirin *Storageaccountıd* kimliği ile depolama hesabı</p>
    
    **Azure olay hub'ları:**
    ```powershell
    Set-AzDiagnosticSetting -ResourceId $aadds.ResourceId ` 
    -EventHubName eventHubName `
    -EventHubAuthorizationRuleId eventHubRuleId `
    -Enabled $true
    ```
    Değiştirin *eventHubName* olay hub'ınızın adıyla. Değiştirin *eventHubRuleId* kimliğinizle daha önce oluşturduğunuz yetkilendirme kuralı.</p>
    
    **Azure günlük analizi çalışma alanları:**
    ```powershell
    Set-AzureRmDiagnosticSetting -ResourceId $aadds.ResourceId ` 
    -WorkspaceID workspaceId `
    -Enabled $true
    ```
    Değiştirin *Workspaceıd* önceden oluşturduğunuz Log Analytics çalışma alanı kimliği. 

## <a name="view-security-audit-events-using-azure-monitor"></a>Azure İzleyicisi'ni kullanarak güvenlik denetim olaylarını görüntüle
Günlük analizi çalışma alanlarını görüntülemek ve Azure İzleyici ve Kusto sorgu dilini kullanarak güvenlik denetim olaylarını çözümlemek etkinleştirin. Sorgu dili bir kolayca okunur söz dizimi ile güç analitik yetenekleri sunar salt okunur kullanmak için tasarlanmıştır.
Kusto sorgu dillerle çalışmaya başlamanıza yardımcı olacak bazı kaynaklar aşağıda verilmiştir.
* [Azure İzleyici belgeleri](https://docs.microsoft.com/azure/azure-monitor/)
* [Azure İzleyici'de Log Analytics ile çalışmaya başlama](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal)
* [Azure İzleyici'de günlük sorguları kullanmaya başlama](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-queries)
* [Verilerin Log Analytics panoları oluşturma ve paylaşma](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-logs-dashboards)

## <a name="sample-queries"></a>Örnek sorgular

### <a name="sample-query-1"></a>Örnek sorgu 1
Tüm hesap kilitleme olaylarını son yedi gündür.
```Kusto
AADDomainServicesAccountManagement
| where TimeGenerated >= ago(7d)
| where OperationName has "4740"
```

### <a name="sample-query-2"></a>Örnek Sorgu 2
Hesap kilitleme arasındaki tüm etkinlikleri (4740) 26 Haziran 2019 9: 00 ve tarih ve saate göre artan düzende sıralanmış 1 Temmuz 2019 gece yarısı.
```Kusto
AADDomainServicesAccountManagement
| where TimeGenerated >= datetime(2019-06-26 09:00) and TimeGenerated <= datetime(2019-07-01) 
| where OperationName has "4740"
| sort by TimeGenerated asc
```

### <a name="sample-query-3"></a>Örnek sorgu 3
Günlük olayları önce yedi gün (bugünden itibaren) hesap adlı kullanıcı hesabı için.
```Kusto
AADDomainServicesAccountLogon
| where TimeGenerated >= ago(7d)
| where "user" == tolower(extract("Logon Account:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
```

### <a name="sample-query-4"></a>Örnek sorgu 4
Hesap oturum açma olayları artık hesap için önce yedi gün hatalı bir parola (0xC0000006a) kullanarak oturum açmayı denediğini kullanıcı adı.
```Kusto
AADDomainServicesAccountLogon
| where TimeGenerated >= ago(7d)
| where "user" == tolower(extract("Logon Account:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
| where "0xc000006a" == tolower(extract("Error Code:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
```

### <a name="sample-query-5"></a>Örnek sorgu 5
Hesap oturum açma olayları artık hesap (out 0xC0000234) kilitliyken oturum açmayı denediği hesabı adlı kullanıcı için önce yedi gün.
```Kusto
AADDomainServicesAccountLogon
| where TimeGenerated >= ago(7d)
| where "user" == tolower(extract("Logon Account:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
| where "0xc0000234" == tolower(extract("Error Code:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
```

### <a name="sample-query-6"></a>Örnek sorgu 6
Hesap oturumu açma olaylarını sayısı, tüm kullanıcıların kilitli oluştu girişim yedi gün artık tüm önce gelen oturum açın.
```Kusto
AADDomainServicesAccountLogon
| where TimeGenerated >= ago(7d)
| where "0xc0000234" == tolower(extract("Error Code:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
| summarize count()
```

## <a name="related-content"></a>İlgili içerik
* [Genel Bakış](https://docs.microsoft.com/azure/kusto/query/) Kusto sorgu dili.
* [Kusto öğretici](https://docs.microsoft.com/azure/kusto/query/tutorial) sorgu temel kavramları hakkında bilgi için.
* [Örnek sorgular](https://docs.microsoft.com/azure/kusto/query/samples) verilerinizi görmek için yeni yollar öğrenmenize yardımcı olacak.
* Kusto [en iyi uygulamalar](https://docs.microsoft.com/azure/kusto/query/best-practices) – sorgularınızı başarı için iyileştirin.














 
