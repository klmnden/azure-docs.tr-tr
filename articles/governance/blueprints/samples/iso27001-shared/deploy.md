---
title: Örnek - ISO 27001 paylaşılan hizmetler blueprint - adımları dağıtma
description: Adımlar ISO 27001 paylaşılan hizmetler şema örnek dağıtın.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/14/2019
ms.topic: sample
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: d27f2495c70dbe6e10fb3adf5370a31903be3abf
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59267932"
---
# <a name="deploy-the-iso-27001-shared-services-blueprint-sample"></a>ISO 27001 paylaşılan hizmetler blueprint örneği dağıtma

Azure kavramsal tasarımlar ISO 27001 paylaşılan hizmetler şema örnek dağıtmak için aşağıdaki adım atılmalıdır:

> [!div class="checklist"]
> - Örnekten yeni blueprint oluşturma
> - Örnek olarak kopyanızın işaretlemek **yayımlandı**
> - Blueprint kopyasını mevcut bir aboneliğe atayın

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturun.

## <a name="create-blueprint-from-sample"></a>Örnekten Blueprint oluşturma

İlk olarak, şema örnek bir başlangıç örneğini kullanarak ortamınızda yeni bir şema oluşturarak uygulayın.

1. Seçin **tüm hizmetleri** arayın ve seçin **ilke** sol bölmesinde. Üzerinde **ilke** sayfasında **şemaları**.

1. Gelen **Başlarken** seçin sol taraftaki sayfasında **Oluştur** düğmesini _blueprint oluşturma_.

1. Bulma **ISO 27001: Paylaşılan Hizmetler** şema örnek altında _diğer örnekleri_ seçip **Bu örneği kullanmak**.

1. Girin _Temelleri_ şema örnek:

   - **Blueprint adı**: ISO 27001 paylaşılan hizmetler şema örnek kopyası için bir ad sağlayın.
   - **Tanım konumu**: Üç nokta kullanın ve örneğe kopyasını kaydetmek için yönetim grubunu seçin.

1. Seçin _Yapıtları_ sayfanın üst kısmındaki sekme veya **sonraki: Yapıtları** sayfanın alt kısmındaki.

1. Şema örnek değişiklikleri yapıtların listesini gözden geçirin. Yapıtlar birçoğu, daha sonra tanımlarız parametrelere sahip. Seçin **Taslağı Kaydet** bittiğinde şema örnek gözden geçirme.

## <a name="publish-the-sample-copy"></a>Örnek kopyalama yayımlama

Şema örnek kopyanızın artık ortamınızda oluşturuldu. İçinde oluşturulan **taslak** modu ve olmalıdır **yayımlanan** , atanan ve dağıtılan kullanılmadan önce. Değişiklik uzağa ISO 27001 standardının taşıyabilir ancak bu şema kopyasını gereksinimlerine ve ortam için özelleştirilebilir.

1. Seçin **tüm hizmetleri** arayın ve seçin **ilke** sol bölmesinde. Üzerinde **ilke** sayfasında **şemaları**.

1. Seçin **Blueprint tanımları** soldaki sayfası. Şema örnek kopyasını bulun ve seçin için filtreleri kullanın.

1. Seçin **Yayımla şema** sayfanın üstünde. Sağ taraftaki yeni sayfa sağlayan bir **sürüm** şema örnek kopyası için. Bu özellik, daha sonra bir değişikliği yapmak için yararlıdır. Sağlamak **notları değiştirmek** "ilk sürüm ISO 27001 blueprint örnekten yayımlanan gibi." Ardından **Yayımla** sayfanın alt kısmındaki.

## <a name="assign-the-sample-copy"></a>Örnek kopya atama

Blueprint kopyasını başarıyla silindikten sonra **yayımlanan**, yönetim grubu için kaydedildi dahilinde bir aboneliğe atanabilir. Bu adım, burada parametreler şema kopyasını, her dağıtım benzersiz olacak şekilde sağlanır.

1. Seçin **tüm hizmetleri** arayın ve seçin **ilke** sol bölmesinde. Üzerinde **ilke** sayfasında **şemaları**.

1. Seçin **Blueprint tanımları** soldaki sayfası. Şema örnek kopyasını bulun ve seçin için filtreleri kullanın.

1. Seçin **Ata şema** şema tanımı sayfanın üstünde.

1. Blueprint ataması için parametre değerlerini sağlayın:

   - Temel Bilgiler

     - **Abonelikleri**: Bir veya daha fazla yönetim grubuna olduğunuz abonelikleri için şema örnek kopyanızın kaydedilen seçin. Birden fazla aboneliğiniz seçerseniz, bir atama için her girdiğiniz parametreleri kullanarak oluşturulur.
     - **Ödev adı**: Şema adını temel alarak, önceden doldurulmuş adıdır.
       Gerektiği gibi değiştirin ya da olduğu gibi bırakın.
     - **Konum**: Yönetilen kimlikle oluşturulması için bir bölge seçin. Azure Blueprint bu yönetilen kimliği kullanarak tüm yapıtları atanmış şemaya dağıtır. Daha fazla bilgi için bkz. [Azure kaynakları için yönetilen kimlikler](../../../../active-directory/managed-identities-azure-resources/overview.md).
     - **Şema tanımı sürümü**: Çekme bir **yayımlanan** şema örnek kopyanızın sürümü.

   - Atamayı Kilitle

     Ortamınızı ayarlama şema kilidi seçin. Daha fazla bilgi için bkz. [şema kaynağı kilitleme](../../concepts/resource-locking.md).

   - Yönetilen Kimlik

     Varsayılan değeri bırakın _sistem tarafından atanan_ yönetilen kimlik seçeneği.

   - Şema parametreleri

     Bu bölümde tanımlanan parametrelerin, tutarlılık sağlamak için şema tanımı içindeki yapıları birçoğu tarafından kullanılır.

     - **Kuruluş adı**: Kuruluşunuz için bir kısa ad girin. Bu özellik, öncelikle kaynakları adlandırmak için kullanılır.
     - **Paylaşılan Hizmetler alt ağı adres ön eki**: Dağıtılan kaynakları birlikte ağ için CIDR gösterimi değeri sağlayın.
     - **Ortak Hizmetleri konuma**: Yapıtlar dağıtılan hangi konumunu belirler. Tüm hizmetler tüm konumlarda kullanılabilir. Tür hizmetlerin dağıtımı yapıtları dağıtmak için bu yapıt konumu için bir parametre seçeneği sağlar.
     - **İzin verilen konumunu (İlkesi: Blueprint girişim ISO 27001 için)**: Kaynak grubu ve kaynak için izin verilen konumlar belirten değer.
     - **VM aracıları için log Analytics çalışma alanı (İlkesi: Blueprint girişim ISO 27001 için)**: Bir çalışma alanının kaynak Kimliğini belirtir. Bu parametre bir `concat` kaynak kimliği oluşturmak için işlevi

   - Yapıt parametreleri

     Bu bölümde tanımlanan parametrelerin altında tanımlandığı yapıtı için geçerlidir. Bu parametreler [dinamik parametreleri](../../concepts/parameters.md#dynamic-parameters) blueprint ataması sırasında tanımlanan olduğundan. Tam bir liste veya yapıt parametrelerin ve Tanımlamaların için bkz. [Yapıt parametreleri tablo](#artifact-parameters-table).

1. Tüm parametreler girildikten sonra seçin **atama** sayfanın alt kısmındaki. Şema atamasını oluşturulur ve yapıt dağıtımı başlar. Dağıtım gereken yaklaşık bir saat. Dağıtım durumunu denetlemek için şema atamasını açın.

> [!WARNING]
> Azure şemaları hizmet ve yerleşik şema örnekleri **ücretsiz olarak sunulmaktadır**. Azure kaynaklarıdır [ürüne göre fiyatlandırılır](https://azure.microsoft.com/en-us/pricing/). Kullanım [fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/) çalıştıran bu şema örnek tarafından dağıtılan kaynakların maliyetini tahmin etmek için.

## <a name="artifact-parameters-table"></a>Yapıt parametreleri Tablo

Aşağıdaki tabloda, yapıt parametreleri şema listesi sağlar:

|Yapıt adı|Yapıt türü|Parametre adı|Açıklama|
|-|-|-|-|
|[Önizleme]: Linux VM ölçek kümeleri (VMSS) için log Analytics aracısını dağıtmayı|İlke ataması|İsteğe bağlı: Kapsama eklemek için Linux işletim sistemi desteklenen bir VM görüntüsü listesi|(İsteğe bağlı) Varsayılan değer _["none"]_.|
|[Önizleme]: Linux Vm'leri için log Analytics aracısını dağıtmayı|İlke ataması|İsteğe bağlı: Kapsama eklemek için Linux işletim sistemi desteklenen bir VM görüntüsü listesi|(İsteğe bağlı) Varsayılan değer _["none"]_.|
|[Önizleme]: Windows VM ölçek kümeleri (VMSS) için log Analytics aracısını dağıtmayı|İlke ataması|İsteğe bağlı: Windows işletim sistemi kapsamına eklenecek desteklenen bir VM görüntüsü listesi|(İsteğe bağlı) Varsayılan değer _["none"]_.|
|[Önizleme]: Windows Vm'leri için log Analytics aracısını dağıtmayı|İlke ataması|İsteğe bağlı: Windows işletim sistemi kapsamına eklenecek desteklenen bir VM görüntüsü listesi|(İsteğe bağlı) Varsayılan değer _["none"]_.|
|İzin verilen kaynak türleri|İlke ataması|İzin verilen kaynak türleri|Dağıtılacak izin verilen kaynak türleri listesi. Bu liste, paylaşılan hizmetlerinde dağıtılan tüm kaynak türleri, oluşur.|
|İzin verilen depolama hesabı SKU'ları|İlke ataması|İzin verilen depolama SKU'ları|Liste tanılama depolama hesabı SKU'ları izin günlüğe kaydeder. Varsayılan değer _["Standard_LRS"]_.|
|İzin verilen sanal makine SKU'ları|İlke ataması|Dağıtılacak sanal makine SKU'ların listesini izin. Varsayılan değer _["Standard_DS1_v2", "Standard_DS2_v2"]_.|
|ISO 27001 için şema girişim|İlke ataması|Tanılama günlükleri denetlemek için kaynak türleri|Tanılama Günlüğü ayarı etkin değilse denetim için kaynak türleri listesi. Kabul edilebilir değerler bulunabilir [Azure İzleyici tanılama günlükleri şemaları](../../../../azure-monitor/platform/diagnostic-logs-schema.md#supported-log-categories-per-resource-type).|
|Log Analytics kaynak grubu|Kaynak grubu|Ad|**Kilitli** -sıralar **kuruluş adı** ile `-sharedsvsc-log-rg` kaynak grubunu benzersiz olacak şekilde.|
|Log Analytics kaynak grubu|Kaynak grubu|Konum|**Kilitli** -şema parametresini kullanır.|
|Log Analytics şablonu|Resource Manager şablonu|Hizmet katmanı|Log Analytics çalışma alanının katman ayarlar. Varsayılan değer _PerNode_.|
|Log Analytics şablonu|Resource Manager şablonu|Gün cinsinden günlük tutma süresi|Gün cinsinden veri tutma. Varsayılan değer _365_.|
|Log Analytics şablonu|Resource Manager şablonu|Konum|Log Analytics çalışma alanı oluşturmak için kullanılan bölge. Varsayılan değer _Batı ABD 2_.|
|Ağ kaynak grubu|Kaynak grubu|Ad|**Kilitli** -sıralar **kuruluş adı** ile `-sharedsvcs-net-rg` kaynak grubunu benzersiz olacak şekilde.|
|Ağ kaynak grubu|Kaynak grubu|Konum|**Kilitli** -şema parametresini kullanır.|
|Azure Güvenlik Duvarı şablonu|Resource Manager şablonu|Azure güvenlik duvarı özel IP'si|Özel IP'si ile yapılandırır [Azure Güvenlik Duvarı](../../../../firewall/overview.md). Bu değer, paylaşılan hizmetler alt ağdaki varsayılan rota tablosu olarak da kullanılır. Tanımlanan CIDR gösteriminde bir parçası olması gereken **Azure güvenlik duvarı alt ağ adres ön eki**. Varsayılan değer _10.0.4.4_.|
|Azure Güvenlik Duvarı şablonu|Resource Manager şablonu|Gün cinsinden günlük tutma süresi|Gün cinsinden veri tutma. Varsayılan değer _365_.|
|Ağ Güvenlik Grubu şablonu|Resource Manager şablonu|Gün cinsinden günlük tutma süresi|Gün cinsinden veri tutma. Varsayılan değer _365_.|
|Sanal Ağ ve Yönlendirme Tablosu şablonu|Resource Manager şablonu|Sanal Ağ adresi ön eki|Sanal ağ için CIDR gösterimi. Varsayılan değer _10.0.0.0/16_.|
|Sanal Ağ ve Yönlendirme Tablosu şablonu|Resource Manager şablonu|Sanal Ağ DDoS korumasını etkinleştir|DDoS koruması sanal ağ için yapılandırır. Varsayılan değer _true_.|
|Sanal Ağ ve Yönlendirme Tablosu şablonu|Resource Manager şablonu|Paylaşılan Hizmetler alt ağı adres ön eki|Paylaşılan Hizmetler alt ağ için CIDR gösterimi. Varsayılan değer _10.0.0.0/24_.|
|Sanal Ağ ve Yönlendirme Tablosu şablonu|Resource Manager şablonu|DMZ alt ağ adresi ön eki|DMZ alt ağ için CIDR gösterimi. Varsayılan değer _10.0.1.0/24_.|
|Sanal Ağ ve Yönlendirme Tablosu şablonu|Resource Manager şablonu|Uygulama Ağ Geçidi alt ağ adresi ön eki|Uygulama ağ geçidi alt ağının CIDR gösterimi. Varsayılan değer _10.0.2.0/24_.|
|Sanal Ağ ve Yönlendirme Tablosu şablonu|Resource Manager şablonu|Sanal Ağ Geçidi alt ağ adresi ön eki|Sanal ağ ağ geçidi alt ağının CIDR gösterimi. Varsayılan değer _10.0.3.0/24_.|
|Sanal Ağ ve Yönlendirme Tablosu şablonu|Resource Manager şablonu|Azure Güvenlik Duvarı alt ağ adresi ön eki|CIDR gösterimi [Azure Güvenlik Duvarı](../../../../firewall/overview.md) alt ağ. İçermelidir **Azure güvenlik duvarı özel IP** parametresi.|
|Key Vault kaynak grubu|Kaynak grubu|Ad|**Kilitli** -sıralar **kuruluş adı** ile `-sharedsvcs-kv-rg` kaynak grubunu benzersiz olacak şekilde.|
|Key Vault kaynak grubu|Kaynak grubu|Konum|**Kilitli** -şema parametresini kullanır.|
|Key Vault şablonu|Resource Manager şablonu|Jumpbox yöneticisi kullanıcı adı|Sıçrama kutusu için kullanıcı adı. Aynı özellik değeri ile eşleşmesi gerekir **Sıçrama kutusu şablonu**. Varsayılan değer _jb yönetici kullanıcı_.|
|Key Vault şablonu|Resource Manager şablonu|Jumpbox yöneticisinin SSH anahtarı veya parolası|Anahtar veya Sıçrama kutusu hesap için parola. Aynı özellik değeri ile eşleşmesi gerekir **Sıçrama kutusu şablonu**. Varsayılan değer ve boş bırakılamaz.|
|Key Vault şablonu|Resource Manager şablonu|Etki alanı yöneticisi kullanıcı adı|Active Directory VM erişmek ve diğer VM'lerin bir etki alanına eklemek için kullanılan kullanıcı adı. Eşleşmelidir **etki alanı yönetici kullanıcı** özellik değeri **Active Directory Domain Services şablon**. Varsayılan değer _etki alanı yönetici kullanıcı_.|
|Key Vault şablonu|Resource Manager şablonu|Etki alanı yöneticisi parolası|Etki alanı yönetici kullanıcının parolası. Varsayılan değer ve boş bırakılamaz.|
|Key Vault şablonu|Resource Manager şablonu|AAD nesnesi kimliği|Key Vault örneğine erişim gerektirir. hesap AAD nesne tanımlayıcısı. Varsayılan değer ve boş bırakılamaz. Azure portalından bu değeri bulmak için arama ve altında "Kullanıcılar" seçin _Hizmetleri_. Kullanım _adı_ kutusunu filtrelemek için hesap adını ve bu hesabı seçin. Üzerinde _kullanıcı profili_ sayfasında, "kopyalamak için tıklayın" simgesini seçin _nesne kimliği_.  |
|Key Vault şablonu|Resource Manager şablonu|Gün cinsinden günlük tutma süresi|Gün cinsinden veri tutma. Varsayılan değer _365_.|
|Key Vault şablonu|Resource Manager şablonu|Key Vault SKU'su|Oluşturulan anahtar kasası SKU'su belirtir. Varsayılan değer _Premium_.|
|Sıçrama kutusu kaynak grubu|Kaynak grubu|Ad|**Kilitli** -sıralar **kuruluş adı** ile `-sharedsvcs-jb-rg` kaynak grubunu benzersiz olacak şekilde.|
|Sıçrama kutusu kaynak grubu|Kaynak grubu|Konum|**Kilitli** -şema parametresini kullanır.|
|Jumpbox şablonu|Resource Manager şablonu|Jumpbox yöneticisi kullanıcı adı|Vm'lere Sıçrama kutusuna erişmek için kullanılan kullanıcı adı. Aynı özellik değeri ile eşleşmesi gerekir **Key Vault şablon**. Varsayılan değer _jb yönetici kullanıcı_.|
|Jumpbox şablonu|Resource Manager şablonu|Sıçrama kutusu yönetici parolası (Key Vault kaynak kimliği)|Key Vault kaynak kimliği. Kullanım "/ subscriptions/{subscriptionId}/resourceGroups/{orgName}-sharedsvcs-kv-rg/providers/Microsoft.KeyVault/vaults/{orgName}-sharedsvcs-kv" değiştirin `{subscriptionId}` yerine abonelik Kimliğinizi ve `{orgName}` ile  **Kuruluş adı** blueprint parametresi.|
|Jumpbox şablonu|Resource Manager şablonu|Sıçrama kutusu yönetici parolası (Key Vault gizli dizi adı)|Sıçrama kutusu yöneticisinin kullanıcı adı Değer eşleşmelidir **Key Vault şablon** özelliği **Sıçrama kutusu yönetici kullanıcı adı**.|
|Jumpbox şablonu|Resource Manager şablonu|Jumpbox İşletim Sistemi|Sıçrama kutusu VM işletim sistemini belirler. Varsayılan değer _Windows_.|
|Active Directory etki alanı Hizmetleri kaynak grubu|Kaynak grubu|Ad|**Kilitli** -sıralar **kuruluş adı** ile `-sharedsvcs-adds-rg` kaynak grubunu benzersiz olacak şekilde.|
|Active Directory etki alanı Hizmetleri kaynak grubu|Kaynak grubu|Konum|**Kilitli** -şema parametresini kullanır.|
|Active Directory Domain Services şablonu|Resource Manager şablonu|Etki alanı yöneticisi kullanıcı adı|ADDS Sıçrama kutusu için kullanıcı adı. Aynı özellik değeri ile eşleşmesi gerekir **Key Vault şablon**. Varsayılan değer _ekler-admin-user_.|
|Active Directory Domain Services şablonu|Resource Manager şablonu|Etki alanı yönetici parolası (Key Vault kaynak kimliği)|Key Vault kaynak kimliği. Kullanım "/ subscriptions/{subscriptionId}/resourceGroups/{orgName}-sharedsvcs-kv-rg/providers/Microsoft.KeyVault/vaults/{orgName}-sharedsvcs-kv" değiştirin `{subscriptionId}` yerine abonelik Kimliğinizi ve `{orgName}` ile  **Kuruluş adı** blueprint parametresi.|
|Active Directory Domain Services şablonu|Resource Manager şablonu|Etki alanı yönetici parolası (Key Vault gizli dizi adı)|Etki alanı yöneticisinin kullanıcı adı Değer eşleşmelidir **Key Vault şablon** özelliği **etki alanı yönetici kullanıcı adı**.|
|Active Directory Domain Services şablonu|Resource Manager şablonu|Etki alanı adı|Active Directory örnek tarafından oluşturulan adı. Varsayılan değer _contoso.com_.|
|Active Directory Domain Services şablonu|Resource Manager şablonu|Etki alanı yönetici kullanıcı|AD yönetici hesabı ve cihazlar için AD etki alanına katılmak için kullanıcı adı. Eşleşmelidir **AD yönetici kullanıcı adı** özellik değeri **Key Vault şablon**. Varsayılan değer _etki alanı yönetici kullanıcı_.|
|Active Directory Domain Services şablonu|Resource Manager şablonu|Etki alanı yöneticisi parolası|Parolayı depolamak için Key Vault ayrıntılarını ayarlayın. Varsayılan değer ve boş bırakılamaz.|

## <a name="next-steps"></a>Sonraki adımlar

ISO 27001 paylaşılan hizmetler şema örnek dağıtmak için adımları gözden geçirdikten sonra denetimi eşleme ve mimari hakkında bilgi edinmek için şu makaleleri ziyaret edin:

> [!div class="nextstepaction"]
> [ISO 27001 paylaşılan hizmetler şema - genel bakış](./index.md)
> [ISO 27001 paylaşılan hizmetler şema - denetim eşleme](./control-mapping.md)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Şema yaşam döngüsü](../../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../../how-to/update-existing-assignments.md) öğrenin.