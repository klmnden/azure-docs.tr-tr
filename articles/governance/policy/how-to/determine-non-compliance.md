---
title: Uyumsuzluk nedenlerini belirleme
description: Uyumlu olmayan bir kaynak olduğunda, çok sayıda olası nedeni vardır. Uyumsuzluk neyin neden olduğunu bulmak öğrenin.
author: DCtheGeek
ms.author: dacoulte
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: 6e3e01ca9bd459aa6c6aca8dfaacb98b1267fada
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65979340"
---
# <a name="determine-causes-of-non-compliance"></a>Uyumsuzluk nedenlerini belirleme

Bir Azure kaynağı için bir ilke kuralı uyumlu olmadığı belirlendiğinde kaynağı ile uyumlu değil kural hangi kısmının anlamak yararlıdır. Hangi değişiklik uyumlu hale getirmek için daha önce uyumlu bir kaynak değiştirilmiş anlamak kullanışlıdır. Bu bilgileri bulmanın iki yolu vardır:

> [!div class="checklist"]
> - [Uyumluluk ayrıntıları](#compliance-details)
> - [Değişiklik geçmişi (Önizleme)](#change-history-preview)

## <a name="compliance-details"></a>Uyumluluk ayrıntıları

Uyumlu olmayan bir kaynak olduğunda, o kaynak için Uyumluluk ayrıntıları kullanılabilir **ilke uyumluluğunu** sayfası. Uyumluluk ayrıntıları bölmesinde aşağıdaki bilgileri içerir:

- Ad, tür, konum ve kaynak kimliği gibi kaynak ayrıntıları
- Uyumluluk durumu ve geçerli ilke ataması için son değerlendirme zaman damgası
- Listesini _nedeniyle_ kaynak uyumsuzluk

> [!IMPORTANT]
> Uyumluluk ayrıntıları olarak bir _uyumlu_ kaynak gösterir Özellikleri'nin geçerli değeri bu kaynak üzerinde kullanıcı olmalıdır **okuma** işlemi **türü** , Kaynak. Örneğin, varsa _uyumlu_ kaynak **Microsoft.Compute/virtualMachines** kullanıcının olmalıdır sonra **Microsoft.Compute/virtualMachines/read** işlem. Kullanıcının gerekli işlemi yoksa, bir erişim hatası görüntülenir.

Uyumluluk ayrıntıları görüntülemek için aşağıdaki adımları izleyin:

1. Azure portalında **Tüm hizmetler**’e tıkladıktan sonra **İlke**'yi arayıp seçerek Azure İlkesi hizmetini başlatın.

1. Üzerinde **genel bakış** veya **Uyumluluk** sayfasında, ilke bir **uyumluluk durumu** diğer bir deyişle _uyumlu_.

1. Altında **kaynak Uyumluluk** sekmesinde **ilke uyumluluğunu** sayfasında sağ tıklayın veya bir kaynağın noktayı bir **uyumluluk durumu** diğer bir deyişle  _Uyumlu olmayan_. Ardından **uyumluluk ayrıntıları görüntüle**.

   ![Görünümü uyumluluğu ayrıntıları seçeneği](../media/determine-non-compliance/view-compliance-details.png)

1. **Uyumluluk ayrıntıları** bölmesi, geçerli bir ilke ataması kaynağın en son değerlendirme bilgileri görüntüler. Bu örnekte, alanın **Microsoft.Sql/servers/version** olduğu tespit edildiğinden _12.0_ while beklenen ilke tanımı _14.0_. Kaynak birden fazla nedenlerle uyumlu ise, her bu bölmesinde listelenir.

   ![Ayrıntılar bölmesinde uyumluluk ve uyumsuzluk nedenleri](../media/determine-non-compliance/compliance-details-pane.png)

   İçin bir **auditIfNotExists** veya **Deployıfnotexists** ilke tanımı ayrıntıları dahil **details.type** özelliği ve diğer isteğe bağlı özellikleri. Bir liste için bkz. [auditIfNotExists özellikleri](../concepts/effects.md#auditifnotexists-properties) ve [Deployıfnotexists özellikleri](../concepts/effects.md#deployifnotexists-properties). **Kaynak son değerlendirme** ilgili bir kaynaktan geliyorsa **ayrıntıları** tanımının bölümü.

   Örnek kısmi **Deployıfnotexists** tanımı:

   ```json
   {
       "if": {
           "field": "type",
           "equals": "[parameters('resourceType')]"
       },
       "then": {
           "effect": "DeployIfNotExists",
           "details": {
               "type": "Microsoft.Insights/metricAlerts",
               "existenceCondition": {
                   "field": "name",
                   "equals": "[concat(parameters('alertNamePrefix'), '-', resourcegroup().name, '-', field('name'))]"
               },
               "existenceScope": "subscription",
               "deployment": {
                   ...
               }
           }
       }
   }
   ```

   ![Uyumluluk ayrıntıları bölmesinde - * ifNotExists](../media/determine-non-compliance/compliance-details-pane-existence.png)

> [!NOTE]
> Bir özellik değeri olduğunda verileri korumak için bir _gizli_ yıldız geçerli değeri görüntüler.

Bu ayrıntıları neden bir kaynak şu anda uyumlu olduğunu açıklayan, ancak değişikliğin uyumlu olmayan bir duruma neden olan bir kaynak için ne zaman yapıldığı gösterme. Bu bilgi için bkz: [değişiklik geçmişini (Önizleme)](#change-history-preview) aşağıda.

### <a name="compliance-reasons"></a>Uyumluluk nedenleriyle

Matris her olası eşler _neden_ sorumlu için [koşul](../concepts/definition-structure.md#conditions) ilke tanımı içinde:

|Reason | Koşul |
|-|-|
|Geçerli değer, anahtar olarak hedef değeri içermelidir. |containsKey veya **değil** notContainsKey |
|Geçerli değer hedef değeri içermelidir. |içerir veya **değil** notContains |
|Geçerli değer hedef değere eşit olmalıdır. |eşittir veya **değil** notEquals |
|Geçerli değer mevcut olmalıdır. |Var. |
|Geçerli değer hedef değerde bulunmalıdır. |içinde veya **değil** notIn |
|Geçerli değer hedef değer gibi olmalıdır. |gibi veya **değil** notLike |
|Geçerli değer, büyük/küçük harfe duyarlı olarak hedef değerle eşleşmelidir. |eşleşen veya **değil** notMatch |
|Geçerli değer, büyük/küçük harfe duyarlı olmayan şekilde hedef değerle eşleşmelidir. |matchInsensitively veya **değil** notMatchInsensitively |
|Geçerli değer, anahtar olarak hedef değeri içermemelidir. |notContainsKey veya **değil** containsKey|
|Geçerli değer hedef değeri içermemelidir. |notContains veya **değil** içerir |
|Geçerli değer hedef değere eşit olmamalıdır. |notEquals veya **değil** eşittir |
|Geçerli değer mevcut olmamalıdır. |**değil** var.  |
|Geçerli değer hedef değerde bulunmamalıdır. |notIn veya **değil** içinde |
|Geçerli değer hedef değer gibi olmamalıdır. |notLike veya **değil** gibi |
|Geçerli değer büyük/küçük harfe duyarlı olarak hedef değerle eşleşmemelidir. |notMatch veya **değil** eşleşmesi |
|Geçerli değer, büyük/küçük harfe duyarlı olmayan bir şekilde hedef değerle eşleşmemelidir. |notMatchInsensitively veya **değil** matchInsensitively |
|İlke tanımındaki etki ayrıntılarıyla eşleşen ilgili kaynak yok. |Bir kaynak türünün tanımlanan **then.details.type** ve ilgili tanımlanan kaynağa **varsa** ilke kuralı bölümü yok. |

## <a name="compliance-details-for-guest-configuration"></a>Konuk yapılandırması için Uyumluluk ayrıntıları

İçin _denetim_ ilkelerinde _Konuk yapılandırma_ kategori, sanal makine içinde değerlendirilen birden çok ayarları olabilir ve her bir ayar ayrıntılarını görüntülemek ihtiyacınız olacak. Örneğin, yüklü uygulamalar ve atama durumu listesi için Denetim ise _uyumlu_, belirli uygulamaları eksik olduğunu bilmeniz gerekir.

Ayrıca VM doğrudan oturum açmak için erişimi olmayabilir ancak VM neden olduğunu bildirmek gereken _uyumlu_. Örneğin, VM'ler doğru etki alanına ve geçerli etki alanı üyeliği raporlama ayrıntıları dahil denetim.

### <a name="azure-portal"></a>Azure portal

1. Azure portalında **Tüm hizmetler**’e tıkladıktan sonra **İlke**'yi arayıp seçerek Azure İlkesi hizmetini başlatın.

1. Üzerinde **genel bakış** veya **Uyumluluk** sayfasında, Konuk yapılandırma ilke tanımını içeren herhangi bir girişim için bir ilke ataması bu _uyumlu_.

1. Seçin bir _denetim_ girişim ilkesinde o _uyumlu_.

   ![Denetim tanımı ayrıntılarını görüntüle](../media/determine-non-compliance/guestconfig-audit-compliance.png)

1. Üzerinde **kaynak Uyumluluk** sekmesinde, aşağıdaki bilgileri sağlanır:

   - **Ad** -Konuk yapılandırma atamalarını adı.
   - **Üst kaynak** -sanal makinede bir _uyumlu_ seçili Konuk yapılandırma atamasını durumunda.
   - **Kaynak türü** - _guestConfigurationAssignments_ tam adı.
   - **Son Değerlendirme** - Konuk Yapılandırma hizmeti, Azure İlkesi hedef sanal makinenin durumu hakkında bildirimde en son ne zaman.

   ![Uyumluluk ayrıntılarını görüntüleyin.](../media/determine-non-compliance/guestconfig-assignment-view.png)

1. Konuk yapılandırma atama adı seçin **adı** açmak için sütun **kaynak Uyumluluk** sayfası.

1. Seçin **görünümü kaynak** açmak için sayfanın üstünde düğme **Konuk atama** sayfası.

**Konuk atama** sayfası, tüm kullanılabilir uyumluluk ayrıntılarını görüntüler. Her satır Görünümü'nde sanal makinenin içinde gerçekleştirilen değerlendirme temsil eder. İçinde **neden** sütun, Konuk atamanın neden olduğunu tanımlayan bir ifade _uyumlu_ gösterilir. Örneğin, Vm'leri bir etki alanına katılması, Denetim **neden** sütun, geçerli etki alanı üyeliği de dahil olmak üzere metin görüntüler.

![Uyumluluk ayrıntılarını görüntüleyin.](../media/determine-non-compliance/guestconfig-compliance-details.png)

### <a name="azure-powershell"></a>Azure PowerShell

Azure powershell'den uyumluluk ayrıntıları da görüntüleyebilirsiniz. İlk olarak, Konuk yapılandırma Modülü yüklü olduğundan emin olun.

```azurepowershell-interactive
Install-Module Az.GuestConfiguration
```

Aşağıdaki komutu kullanarak bir VM için tüm Konuk atamalarını geçerli durumunu görüntüleyebilirsiniz:

```azurepowershell-interactive
Get-AzVMGuestPolicyReport -ResourceGroupName <resourcegroupname> -VMName <vmname>
```

```output
PolicyDisplayName                                                         ComplianceReasons
-----------------                                                         -----------------
Audit that an application is installed inside Windows VMs                 {[InstalledApplication]bwhitelistedapp}
Audit that an application is not installed inside Windows VMs.            {[InstalledApplication]NotInstalledApplica...
```

Yalnızca görüntülemek için _neden_ VM neden olduğunu açıklayan tümcecik _uyumlu olmayan_, yalnızca neden alt özelliği döndürür.

```azurepowershell-interactive
Get-AzVMGuestPolicyReport -ResourceGroupName <resourcegroupname> -VMName <vmname> | % ComplianceReasons | % Reasons | % Reason
```

```output
The following applications are not installed: '<name>'.
```

Ayrıca, sanal makine için kapsamda Konuk atamaları için Uyumluluk geçmişi çıkış sağlayabilir. Bu komutun çıktısı, sanal makine için her rapor ayrıntılarını içerir.

> [!NOTE]
> Çıktı, büyük miktarda veri döndürebilir. Çıktı bir değişkende depolamak için önerilir.

```azurepowershell-interactive
$guestHistory = Get-AzVMGuestPolicyStatusHistory -ResourceGroupName <resourcegroupname> -VMName <vmname>
$guestHistory
```

```output
PolicyDisplayName                                                         ComplianceStatus ComplianceReasons StartTime              EndTime                VMName LatestRepor
                                                                                                                                                                  tId
-----------------                                                         ---------------- ----------------- ---------              -------                ------ -----------
[Preview]: Audit that an application is installed inside Windows VMs      NonCompliant                       02/10/2019 12:00:38 PM 02/10/2019 12:00:41 PM VM01  ../17fg0...
<truncated>
```

Bu görünüm basitleştirmek için **ShowChanged** parametresi. Bu komutun çıktısı, yalnızca uyumluluk durumu değişikliği izleyen raporlar içerir.

```azurepowershell-interactive
$guestHistory = Get-AzVMGuestPolicyStatusHistory -ResourceGroupName <resourcegroupname> -VMName <vmname> -ShowChanged
$guestHistory
```

```output
PolicyDisplayName                                                         ComplianceStatus ComplianceReasons StartTime              EndTime                VMName LatestRepor
                                                                                                                                                                  tId
-----------------                                                         ---------------- ----------------- ---------              -------                ------ -----------
Audit that an application is installed inside Windows VMs                 NonCompliant                       02/10/2019 10:00:38 PM 02/10/2019 10:00:41 PM VM01  ../12ab0...
Audit that an application is installed inside Windows VMs.                Compliant                          02/09/2019 11:00:38 AM 02/09/2019 11:00:39 AM VM01  ../e3665...
Audit that an application is installed inside Windows VMs                 NonCompliant                       02/09/2019 09:00:20 AM 02/09/2019 09:00:23 AM VM01  ../15ze1...
```

## <a name="a-namechange-historychange-history-preview"></a><a name="change-history"/>Değişiklik geçmişi (Önizleme)

Yeni bir parçası olarak **genel Önizleme**, değişiklik geçmişini son 14 günün destekleyen tüm Azure kaynakları için kullanılabilir [tamamlama modu silme](../../../azure-resource-manager/complete-mode-deletion.md). Değişiklik geçmişi sağlar ayrıntılar hakkında bir değişiklik algıladığında ve _visual fark_ her değişiklik için. Resource Manager özelliklerini eklendiğinde, kaldırılmış veya değiştirilmiş bir değişiklik algılama tetiklenir.

1. Azure portalında **Tüm hizmetler**’e tıkladıktan sonra **İlke**'yi arayıp seçerek Azure İlkesi hizmetini başlatın.

1. Üzerinde **genel bakış** veya **Uyumluluk** sayfasında, herhangi bir ilke seçin **uyumluluk durumu**.

1. Altında **kaynak Uyumluluk** sekmesinde **ilke uyumluluğunu** sayfasında, bir kaynak seçin.

1. Seçin **değişiklik geçmişini (Önizleme)** sekmesinde **kaynak Uyumluluk** sayfası. Tüm mevcut görüntüleniyorsa listesini değişiklikleri algıladı.

   ![Kaynak uyumluluk sayfasında Azure İlkesi değişiklik geçmişi sekmesi](../media/determine-non-compliance/change-history-tab.png)

1. Algılanan değişiklikler birini seçin. _Visual fark_ kaynak üzerinde sunulan için **değişiklik geçmişini** sayfası.

   ![Azure İlkesi değişiklik geçmişi görsel fark değişiklik geçmişi sayfasındaki](../media/determine-non-compliance/change-history-visual-diff.png)

_Visual fark_ aides içinde bir kaynak değişikliklerini tanımlama. Algılanan değişiklikler kaynağın geçerli uyumluluk durumu için ilişkili değil.

Değişiklik geçmişi verilerini sağlayan [Azure kaynak Graph](../../resource-graph/overview.md). Azure portalı bu bilgileri sorgulamak için bkz. [alma kaynak değişiklikleri](../../resource-graph/how-to/get-resource-changes.md).

## <a name="next-steps"></a>Sonraki adımlar

- Gözden geçirme örneklere [Azure ilkesi örnekleri](../samples/index.md).
- [Azure İlkesi tanımı yapısını](../concepts/definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](../concepts/effects.md) konusunu gözden geçirin.
- Anlamak için nasıl [programlı olarak ilkeler oluşturma](programmatically-create.md).
- Bilgi edinmek için nasıl [uyumluluk verilerini alma](getting-compliance-data.md).
- Bilgi edinmek için nasıl [uyumlu olmayan kaynakları düzeltme](remediate-resources.md).
- Bir yönetim grubu olan gözden geçirme [kaynaklarınızı Azure yönetim gruplarıyla düzenleme](../../management-groups/overview.md).