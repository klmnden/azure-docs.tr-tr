---
title: Etkinleştirme Kurumsal durumda Dolaşım Azure Active Directory'de | Microsoft Docs
description: Windows cihazlarında Kurumsal durumda Dolaşım ayarları hakkında sık sorulan sorular. Kurumsal durumda dolaşım, kullanıcılar Windows cihazlarını arasında birleşik bir deneyim sağlar ve yeni bir cihaz yapılandırmak için gereken süreyi azaltır.
services: active-directory
keywords: Kurumsal durumda dolaşım, Microsoft Bulutu, Kurumsal durumda Dolaşım etkinleştirme
documentationcenter: ''
author: tanning
manager: daveba
editor: curtand
ms.subservice: devices
ms.assetid: f71d66fd-7f9e-45eb-9cfe-5d989870f8a4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2018
ms.author: joflore
ms.collection: M365-identity-device-management
ms.openlocfilehash: e7e665f3124296392c8138f4f2bd3632b56e53de
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65230938"
---
# <a name="enable-enterprise-state-roaming-in-azure-active-directory"></a>Azure Active Directory'de Kurumsal Durumda Dolaşımı etkinleştirme
Kurumsal durumda dolaşım, tüm kuruluşa bir Azure AD Premium veya Enterprise Mobility + Security (EMS) lisansı ile kullanılabilir. Azure AD aboneliğiniz alma hakkında daha fazla bilgi için bkz. [Azure AD'ye ürün sayfası](https://azure.microsoft.com/services/active-directory).

Kurumsal durumda Dolaşım etkinleştirdiğinizde, kuruluşunuz Azure Information Protection'dan Azure Rights Management koruması için ücretsiz, sınırlı kullanım bir lisans otomatik olarak verilir. Bu ücretsiz aboneliği, şifreleme ve kurumsal ayarları ve uygulama verileri Kurumsal durumda Dolaşım tarafından eşitlenen şifresini çözme sınırlıdır. Olmalıdır [Ücretli aboneliğe](https://azure.microsoft.com/pricing/details/information-protection/) tüm özelliklerini Azure Rights Management hizmetini kullanmak için.

## <a name="to-enable-enterprise-state-roaming"></a>Kurumsal durumda Dolaşım etkinleştirmek için

1. Oturum [Azure AD yönetim merkezini](https://aad.portal.azure.com/).

1. Seçin **Azure Active Directory** &gt; **cihazları** &gt; **Kurumsal durumda Dolaşım**.

1. Seçin **kullanıcılar eşitleme ayarları ve uygulama verilerini cihazlarda**. Daha fazla bilgi için [cihaz ayarlarının nasıl yapılandırılacağı](https://docs.microsoft.com/azure/active-directory/device-management-azure-portal).
  
   ![Etiketli Kullanıcılar Cihaz ayarı görüntüsü cihazlarda ayarları ve uygulama verilerini eşitleyebilir](./media/enterprise-state-roaming-enable/device-settings.png)
  
Windows 10 cihaz Kurumsal durumda Dolaşım hizmetini kullanmak üzere bir Azure AD kimlik kullanarak cihaz kimliğini doğrulaması gerekir. Azure AD'ye katılmış cihazlar için oturum açma kullanıcının birincil kimliğini kendi Azure AD kimlik olduğundan ek yapılandırma gerekli değildir. Şirket içi Active Directory kullanan cihazlar için BT yöneticisi gerekir [yapılandırma hibrit Azure Active Directory alanına katılmış cihazlar](https://docs.microsoft.com/azure/active-directory/devices/hybrid-azuread-join-manual-steps). 

## <a name="data-storage"></a>Veri depolama
Bir veya daha fazla veri Kurumsal durumda Dolaşım barındırılan [Azure bölgeleri](https://azure.microsoft.com/regions/) en iyi ülke/bölge değeri Azure Active Directory örneğinde ayarlanmış Hizala. Kurumsal durumda Dolaşım veriler üzerinde üç ana coğrafi bölgeler göre bölümlenir: Kuzey Amerika, EMEA ve APAC. Kiracının verileri Kurumsal durumda Dolaşım coğrafi bölge ile yerel olarak bulunur ve bölgeler arasında çoğaltılmaz.  Örneğin:

Ülke/bölge değeri | kendi veri barındırılan
---------------------|-------------------------
Bir listelenen Fransa veya Zambiya gibi EMEA ülke/bölge | Bir veya daha fazla Avrupa içinde Azure bölgeleri 
ABD veya Kanada gibi Kuzey Amerika bir ülke/bölge | bir veya daha fazla ABD içindeki Azure bölgeler
Bir listelenen Avustralya veya Yeni Zelanda'da gibi APAC ülke/bölge | bir veya daha fazla Azure bölgeleri içinde Asya
Güney Amerika ve Antarktika bölgeler | ABD içindeki bir veya daha fazla Azure bölgeleri

Ülke/bölge değeri, Azure AD dizin oluşturma işleminin bir parçası olarak ayarlanır ve daha sonra değiştirilemez. Bir bileti, veri depolama konumu ile ilgili daha fazla ayrıntı gerekiyorsa, [Azure Destek](https://azure.microsoft.com/support/options/).

## <a name="view-per-user-device-sync-status"></a>Kullanıcı başına cihaz eşitleme durumunu görüntüle
Kullanıcı başına cihaz eşitleme Durum raporunda görüntülemek için aşağıdaki adımları izleyin.

1. Oturum [Azure AD yönetim merkezini](https://aad.portal.azure.com/).

1. Seçin **Azure Active Directory** &gt; **kullanıcılar** &gt; **tüm kullanıcılar**.

1. Kullanıcıyı seçin ve ardından **cihazları**.

1. Altında **Göster**seçin **ayarları ve uygulama verilerini eşitleyen cihazlar** eşitleme durumunu göstermek için.
  
   ![cihaz eşitleme verilerini ayarın görüntüsü](./media/enterprise-state-roaming-enable/sync-status.png)
  
1. Bu kullanıcı için eşitleyen cihazlar varsa, burada gösterildiği gibi cihazlar bakın.
  
   ![cihaz eşitleme sütunlu verilerin görüntüsü](./media/enterprise-state-roaming-enable/device-status-row.png)

## <a name="data-retention"></a>Veri saklama
Kurumsal durumda Dolaşım kullanarak Microsoft Bulut'a eşitlenmiş verileri el ile silinene kadar veya söz konusu veri eski olduğu belirlenir kadar korunur. 

### <a name="explicit-deletion"></a>Açık silme
Azure yönetici bir kullanıcı ya da bir dizin siler veya aksi halde açıkça veri silinecek olan istekleri açık silinmesine olur.

* **Kullanıcı silme**: Veri dolaşımı kullanıcı hesabı, bir kullanıcının Azure AD'de silindiğinde, 90-180 gün sonra silinir. 
* **Dizin silme**: Azure AD'de bir tüm dizin silmeden hemen bir işlemdir. İlişkili tüm ayarları verileri ile dizin 90-180 gün sonra silinir. 
* **Silme isteği**: Azure AD Yöneticisi, belirli bir kullanıcının veri veya ayar verileri el ile silmeniz isterse, yönetici bileti ile dosya [Azure Destek](https://azure.microsoft.com/support/). 

### <a name="stale-data-deletion"></a>Eski veri silme
Bir yıl ("Bekletme dönemi") erişilemeyen veri eski kabul edilir ve Microsoft bulutundan silinebilir. Saklama dönemi değiştirilebilir, ancak 90 günden daha az olur. Eski veri Windows/uygulama ayarları veya bir kullanıcı için tüm ayarları belirli bir kümesi olabilir. Örneğin:

* Cihaz erişimi belirli ayarlar koleksiyonu (örneğin, bir uygulama CİHAZDAN kaldırılır veya bir "Tema" gibi ayarları grubu tüm kullanıcı aygıtları için devre dışıdır), o koleksiyon saklama döneminden sonra eski hale gelir ve silinebilir . 
* Bir kullanıcı, kullanıcının tüm cihazlarında ayarları eşitleme devre dışı bıraktıysa ayarları verilerin hiçbiri ardından erişilir ve söz konusu kullanıcı için tüm ayarları veri eski hale gelir ve saklama döneminden sonra silinebilir. 
* Azure AD dizin Yöneticisi Kurumsal durumda dolaşım, tüm kullanıcılar gibi tüm dizin için dizin ayarları eşitlenirken durdurur ve tüm kullanıcılar için tüm ayarları veri eski hale gelir ve saklama döneminden sonra silinebilir kapatırsa. 

### <a name="deleted-data-recovery"></a>Silinen verileri kurtarma
Veri bekletme ilkesi yapılandırılabilir değildir. Veriler kalıcı olarak silindikten sonra kurtarılamaz değil. Ancak, son kullanıcı CİHAZDAN Microsoft bulutundan yalnızca ayar verileri silinir. Herhangi bir CİHAZDAN Kurumsal durumda Dolaşım hizmete daha sonra bağlanırsa, ayarları yeniden eşitlenir ve Microsoft bulutta depolanır.

## <a name="next-steps"></a>Sonraki adımlar

* [Kurumsal durumda dolaşıma genel bakış](enterprise-state-roaming-overview.md)
* [Ayarlar ve veri dolaşımı hakkında SSS](enterprise-state-roaming-faqs.md)
* [Grup İlkesi ve MDM ayarları için ayarları eşitleme](enterprise-state-roaming-group-policy-settings.md)
* [Windows 10 Dolaşım ayarları başvurusu](enterprise-state-roaming-windows-settings-reference.md)
* [Sorun giderme](enterprise-state-roaming-troubleshooting.md)
