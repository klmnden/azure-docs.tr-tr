---
title: Örnek - ISO 27001 ASE/SQL iş yükü şema - dağıtma adımları
description: Adımlar ISO 27001 App Service ortamı/SQL veritabanı iş yükü şema örnek dağıtın.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/14/2019
ms.topic: sample
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 78f608aedd53aa1071eaf88864f5a63f8f9e6072
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59791020"
---
# <a name="deploy-the-iso-27001-app-service-environmentsql-database-workload-blueprint-sample"></a>ISO 27001 App Service ortamı/SQL veritabanı iş yükü şema örneği dağıtma

Azure şemaları ISO 27001 App Service ortamı/SQL veritabanı iş yükü şema örnek dağıtmak için aşağıdaki adım atılmalıdır:

> [!div class="checklist"]
> - Dağıtma [ISO 27001 Paylaşılan Hizmetler](../iso27001-shared/index.md) şema örnek
> - Örnekten yeni blueprint oluşturma
> - Örnek olarak kopyanızın işaretlemek **yayımlandı**
> - Blueprint kopyasını mevcut bir aboneliğe atayın

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturun.

## <a name="deploy-the-iso-27001-shared-services-blueprint-sample"></a>ISO 27001 paylaşılan hizmetler blueprint örneği dağıtma

Bu şema örnek dağıtılabilmesi [ISO 27001 Paylaşılan Hizmetler](../iso27001-shared/index.md) şema örnek hedef aboneliğine dağıtılması gerekir. ISO 27001 paylaşılan hizmetler şema örnek başarılı bir dağıtımı, bu şema örnek altyapı bağımlılıklar eksik olacaktır ve dağıtım sırasında başarısız.

> [!IMPORTANT]
> Bu şema örnek ile aynı abonelikte atanmalıdır [ISO 27001 Paylaşılan Hizmetler](../iso27001-shared/index.md) şema örnek.

## <a name="create-blueprint-from-sample"></a>Örnekten Blueprint oluşturma

İlk olarak, şema örnek bir başlangıç örneğini kullanarak ortamınızda yeni bir şema oluşturarak uygulayın.

1. Seçin **tüm hizmetleri** arayın ve seçin **ilke** sol bölmesinde. Üzerinde **ilke** sayfasında **şemaları**.

1. Gelen **Başlarken** seçin sol taraftaki sayfasında **Oluştur** düğmesini _blueprint oluşturma_.

1. Bulma **ISO 27001: ASE/SQL iş yükü** şema örnek altında _diğer örnekleri_ seçip **Bu örneği kullanmak**.

1. Girin _Temelleri_ şema örnek:

   - **Blueprint adı**: ISO 27001 ASE/SQL iş yükü şema örnek kopyası için bir ad sağlayın.
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

     - **Abonelikler**: Bir veya daha fazla yönetim grubuna olduğunuz abonelikleri için şema örnek kopyanızın kaydedilen seçin. Birden fazla aboneliğiniz seçerseniz, bir atama için her girdiğiniz parametreleri kullanarak oluşturulur.
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
     - **Paylaşılan hizmet abonelik kimliği**: Abonelik kimliği burada [ISO 27001 Paylaşılan Hizmetler](../iso27001-shared/index.md) şema örnek atanır.
     - **Varsayılan alt ağ adres ön eki**: Sanal ağ varsayılan alt ağ için CIDR gösterimi.
       Varsayılan değer _10.1.0.0/16_.
     - **İş yükü konumu**: Yapıtlar dağıtılan hangi konumunu belirler. Tüm hizmetler tüm konumlarda kullanılabilir. Tür hizmetlerin dağıtımı yapıtları dağıtmak için bu yapıt konumu için bir parametre seçeneği sağlar.

   - Yapıt parametreleri

     Bu bölümde tanımlanan parametrelerin altında tanımlandığı yapıtı için geçerlidir. Bu parametreler [dinamik parametreleri](../../concepts/parameters.md#dynamic-parameters) blueprint ataması sırasında tanımlanan olduğundan. Tam bir liste veya yapıt parametrelerin ve Tanımlamaların için bkz. [Yapıt parametreleri tablo](#artifact-parameters-table).

1. Tüm parametreler girildikten sonra seçin **atama** sayfanın alt kısmındaki. Şema atamasını oluşturulur ve yapıt dağıtımı başlar. Dağıtım gereken yaklaşık bir saat. Dağıtım durumunu denetlemek için şema atamasını açın.

> [!WARNING]
> Azure şemaları hizmet ve yerleşik şema örnekleri **ücretsiz olarak sunulmaktadır**. Azure kaynaklarıdır [ürüne göre fiyatlandırılır](https://azure.microsoft.com/pricing/). Kullanım [fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/) çalıştıran bu şema örnek tarafından dağıtılan kaynakların maliyetini tahmin etmek için.

## <a name="artifact-parameters-table"></a>Yapıt parametreleri Tablo

Aşağıdaki tabloda, yapıt parametreleri şema listesi sağlar:

|Yapıt adı|Yapıt türü|Parametre adı|Açıklama|
|-|-|-|-|
|Log Analytics kaynak grubu|Kaynak grubu|Ad|**Kilitli** -sıralar **kuruluş adı** ile `-workload-log-rg` kaynak grubunu benzersiz olacak şekilde.|
|Log Analytics kaynak grubu|Kaynak grubu|Konum|**Kilitli** -şema parametresini kullanır.|
|Log Analytics şablonu|Resource Manager şablonu|Hizmet katmanı|Log Analytics çalışma alanının katman ayarlar. Varsayılan değer _PerNode_.|
|Log Analytics şablonu|Resource Manager şablonu|Gün cinsinden günlük tutma süresi|Gün cinsinden veri tutma. Varsayılan değer _365_.|
|Log Analytics şablonu|Resource Manager şablonu|Konum|Log Analytics çalışma alanı oluşturmak için kullanılan bölge. Varsayılan değer _Batı ABD 2_.|
|Ağ kaynak grubu|Kaynak grubu|Ad|**Kilitli** -sıralar **kuruluş adı** ile `-workload-net-rg` kaynak grubunu benzersiz olacak şekilde.|
|Ağ kaynak grubu|Kaynak grubu|Konum|**Kilitli** -şema parametresini kullanır.|
|Ağ Güvenlik Grubu şablonu|Resource Manager şablonu|Gün cinsinden günlük tutma süresi|Gün cinsinden veri tutma. Varsayılan değer _365_.|
|Sanal Ağ ve Yönlendirme Tablosu şablonu|Resource Manager şablonu|Azure güvenlik duvarı özel IP'si|Özel IP'si ile yapılandırır [Azure Güvenlik Duvarı](../../../../firewall/overview.md). Tanımlanan CIDR gösteriminde bir parçası olması gereken _ISO 27001: Paylaşılan Hizmetler_ yapıt parametre **Azure güvenlik duvarı alt ağ adres ön eki**. Varsayılan değer _10.0.4.4_.|
|Sanal Ağ ve Yönlendirme Tablosu şablonu|Resource Manager şablonu|Paylaşılan hizmetler Abonelik kimliği|Bir iş yükü ve paylaşılan hizmetler arasında eşleme VNET etkinleştirmek için kullanılan değer.|
|Sanal Ağ ve Yönlendirme Tablosu şablonu|Resource Manager şablonu|Sanal Ağ adresi ön eki|Sanal ağ için CIDR gösterimi. Varsayılan değer _10.1.0.0/16_.|
|Sanal Ağ ve Yönlendirme Tablosu şablonu|Resource Manager şablonu|Varsayılan alt ağ adresi ön eki|Sanal ağ varsayılan alt ağ için CIDR gösterimi. Varsayılan değer _10.1.0.0/16_.|
|Sanal Ağ ve Yönlendirme Tablosu şablonu|Resource Manager şablonu|ADDS IP adresi|İlk EKLER VM IP adresi. Bu değer özel VNET DNS kullanılır.|
|Key Vault kaynak grubu|Kaynak grubu|Ad|**Kilitli** -sıralar **kuruluş adı** ile `-workload-kv-rg` kaynak grubunu benzersiz olacak şekilde.|
|Key Vault kaynak grubu|Kaynak grubu|Konum|**Kilitli** -şema parametresini kullanır.|
|Key Vault şablonu|Resource Manager şablonu|AAD nesnesi kimliği|Key Vault örneğine erişim gerektirir. hesap AAD nesne tanımlayıcısı. Varsayılan değer ve boş bırakılamaz. Azure portalından bu değeri bulmak için arama ve altında "Kullanıcılar" seçin _Hizmetleri_. Kullanım _adı_ kutusunu filtrelemek için hesap adını ve bu hesabı seçin. Üzerinde _kullanıcı profili_ sayfasında, "kopyalamak için tıklayın" simgesini seçin _nesne kimliği_.|
|Key Vault şablonu|Resource Manager şablonu|Gün cinsinden günlük tutma süresi|Gün cinsinden veri tutma. Varsayılan değer _365_.|
|Key Vault şablonu|Resource Manager şablonu|Key Vault SKU'su|Oluşturulan anahtar kasası SKU'su belirtir. Varsayılan değer _Premium_.|
|Key Vault şablonu|Resource Manager şablonu|Azure SQL Server yönetici kullanıcı adı|Azure SQL sunucusuna erişmek için kullanılan kullanıcı adı. Aynı özellik değeri ile eşleşmesi gerekir **Azure SQL veritabanı şablon**. Varsayılan değer _sql yönetici kullanıcı_.|
|Azure SQL veritabanı kaynak grubu|Kaynak grubu|Ad|**Kilitli** -sıralar **kuruluş adı** ile `-workload-azsql-rg` kaynak grubunu benzersiz olacak şekilde.|
|Azure SQL veritabanı kaynak grubu|Kaynak grubu|Konum|**Kilitli** -şema parametresini kullanır.|
|Azure SQL Veritabanı şablonu|Resource Manager şablonu|Azure SQL Server yönetici kullanıcı adı|Azure SQL Server için kullanıcı adı. Aynı özellik değeri ile eşleşmesi gerekir **Key Vault şablon**. Varsayılan değer _sql yönetici kullanıcı_.|
|Azure SQL Veritabanı şablonu|Resource Manager şablonu|Azure SQL Server Yönetici parolası (Key Vault kaynak kimliği)|Key Vault kaynak kimliği. Kullanım "/ subscription/{subscriptionId}/resourceGroups/{orgName}-workload-kv/providers/Microsoft.KeyVault/vaults/{orgName}-workload-kv" değiştirin `{subscriptionId}` yerine abonelik Kimliğinizi ve `{orgName}` ile  **Kuruluş adı** blueprint parametresi.|
|Azure SQL Veritabanı şablonu|Resource Manager şablonu|Azure SQL Server Yönetici parolası (Key Vault gizli dizi adı)|Kullanıcı adı bir SQL Sunucusu Yöneticisi Değer eşleşmelidir **Key Vault şablon** özelliği **Azure SQL Server yönetici kullanıcı adı**.|
|Azure SQL Veritabanı şablonu|Resource Manager şablonu|Gün cinsinden günlük tutma süresi|Gün cinsinden veri tutma. Varsayılan değer _365_.|
|Azure SQL Veritabanı şablonu|Resource Manager şablonu|AAD yönetici nesne kimliği|Bir Active Directory yönetici olarak atanan kullanıcının AAD nesne kimliği Varsayılan değer ve boş bırakılamaz. Azure portalından bu değeri bulmak için arama ve altında "Kullanıcılar" seçin _Hizmetleri_. Kullanım _adı_ kutusunu filtrelemek için hesap adını ve bu hesabı seçin. Üzerinde _kullanıcı profili_ sayfasında, "kopyalamak için tıklayın" simgesini seçin _nesne kimliği_.|
|Azure SQL Veritabanı şablonu|Resource Manager şablonu|AAD yönetici oturum açma|Şu anda Microsoft hesapları (örneğin, live.com veya outlook.com) yönetici olarak ayarlanamaz Yalnızca kullanıcı ve kuruluşunuzdaki güvenlik grupları yönetici olarak ayarlanabilir. Varsayılan değer ve boş bırakılamaz. Azure portalından bu değeri bulmak için arama ve altında "Kullanıcılar" seçin _Hizmetleri_. Kullanım _adı_ kutusunu filtrelemek için hesap adını ve bu hesabı seçin. Üzerinde _kullanıcı profili_ sayfasında, kopya _kullanıcı adı_.|
|App Service ortamı kaynak grubu|Kaynak grubu|Ad|**Kilitli** -sıralar **kuruluş adı** ile `-workload-ase-rg` kaynak grubunu benzersiz olacak şekilde.|
|App Service ortamı kaynak grubu|Kaynak grubu|Konum|**Kilitli** -şema parametresini kullanır.|
|App Service Ortamı şablonu|Resource Manager şablonu|Etki alanı adı|Active Directory örnek tarafından oluşturulan adı. Varsayılan değer _contoso.com_.|
|App Service Ortamı şablonu|Resource Manager şablonu|ASE konumu|App Service ortamınızın konumuyla. Varsayılan değer _Batı ABD 2_.|
|App Service Ortamı şablonu|Resource Manager şablonu|Application Gateway günlük tutma (gün)|Gün cinsinden veri tutma. Varsayılan değer _365_.|

## <a name="next-steps"></a>Sonraki adımlar

ISO 27001 App Service ortamı/SQL veritabanı iş yükü şema örnek dağıtmak için adımları gözden geçirdikten sonra denetimi eşleme ve mimari hakkında bilgi edinmek için şu makaleleri ziyaret edin:

> [!div class="nextstepaction"]
> [ISO 27001 App Service ortamı/SQL veritabanı iş yükü şema - genel bakış](./index.md)
> [ISO 27001 App Service ortamı/SQL veritabanı iş yükü şema - denetim eşleme](./control-mapping.md)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Şema yaşam döngüsü](../../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../../how-to/update-existing-assignments.md) öğrenin.