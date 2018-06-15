---
title: Azure Active Directory'de gezici Kurumsal durumu etkinleştir | Microsoft Docs
description: Windows cihazları'deki kurumsal durumda Dolaşım ayarları hakkında sık sorulan sorular. Kurumsal durumda dolaşım, kullanıcılar Windows cihazlarını arasında birleştirilmiş bir deneyim sağlar ve yeni bir cihaz yapılandırmak için gereken süreyi azaltır.
services: active-directory
keywords: Kurumsal durumda dolaşımı, windows bulut Kurumsal durumda Dolaşım etkinleştirme
documentationcenter: ''
author: tanning
manager: mtillman
editor: curtand
ms.assetid: f71d66fd-7f9e-45eb-9cfe-5d989870f8a4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/14/2018
ms.author: markvi
ms.openlocfilehash: dba749b6d85898e6438ce1160b9bf6eaff6f4ac9
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
ms.locfileid: "34257980"
---
# <a name="enable-enterprise-state-roaming-in-azure-active-directory"></a>Azure Active Directory'de Kurumsal Durumda Dolaşımı etkinleştirme
Kurumsal durumda Dolaşım herhangi bir Azure AD Premium veya Enterprise Mobility + güvenlik (EMS) lisansı kuruluşla kullanılabilir. Bir Azure AD aboneliği edinme hakkında daha fazla bilgi için bkz: [Azure AD ürün sayfası](https://azure.microsoft.com/services/active-directory).

Kurumsal durumda Dolaşım etkinleştirdiğinizde kuruluşunuz Azure Rights Management koruması ücretsiz, sınırlı kullanım lisansı otomatik olarak Azure bilgi koruma verilir. Bu ücretsiz abonelik, şifreleme ve şifre çözme Kurumsal ayarları ve uygulama verileri Kurumsal durumda Dolaşım tarafından eşitlenen sınırlıdır. Bilmeniz gereken [Ücretli abonelik](https://azure.microsoft.com/pricing/details/information-protection/) Azure Rights Management hizmeti özelliklerini kullanmak için.

## <a name="to-enable-enterprise-state-roaming"></a>Kurumsal durumda Dolaşım etkinleştirmek için

1. Oturum [Azure AD Yönetim Merkezi](https://aad.portal.azure.com/).

2. Seçin **Azure Active Directory** &gt; **aygıtları** &gt; **aygıt ayarları**.

3. Seçin **kullanıcılar eşitleme ayarları ve uygulama verileri cihaz üzerinden**. Daha fazla bilgi için bkz: [cihaz ayarlarının nasıl yapılandırılacağı](https://docs.microsoft.com/azure/active-directory/device-management-azure-portal).
  
  ![Aygıt ayarı kullanıcıların etiketli görüntüsü cihazlarda ayarları ve uygulama verilerini eşitleyebilir](./media/active-directory-windows-enterprise-state-roaming-enable/device-settings.png)
  
Windows 10 cihazına Kurumsal durumda Dolaşım hizmetini kullanmak bir Azure AD kimlik bilgileriniz kullanılarak aygıtın kimliğini doğrulaması gerekir. Azure AD'ye katılan cihazlar için oturum açma kullanıcının birincil kimliğini kendi Azure AD kimlik olduğundan ek yapılandırma gerekli değildir. BT yöneticiniz şirket içi Active Directory kullanan cihazlar için gereken [Windows 10 deneyimleri için etki alanına katılmış cihazlar için Azure AD connect](active-directory-azureadjoin-devices-group-policy.md).

## <a name="data-storage"></a>Veri depolama
Bir veya daha fazla veri Kurumsal durumda Dolaşım barındırılan [Azure bölgeleri](https://azure.microsoft.com/regions/) en iyi ülke/bölge değeri Azure Active Directory örneğine ayarlanmış hizalayın. Kurumsal durumda Dolaşım veri bölümlenmiş üç ana coğrafi bölgelerine bağlı: Kuzey Amerika, EMEA ve APAC. Kurumsal durumda Dolaşım veri Kiracı için coğrafi bölge ile yerel olarak bulunur ve bölgeler arasında çoğaltılmaz.  Örneğin:
Ülke/bölge değeri | verilerini barındırılan
---------------------|-------------------------
"Fransa" veya "Zambiya" gibi bir EMEA ülke | bir ya da Avrupa içindeki Azure bölgeler 
Kuzey Amerika ülke "ABD" veya "Kanada" gibi | bir veya daha fazla ABD içinde Azure bölgeleri
"Avustralya" veya "Yeni Zelanda" gibi bir APAC ülke | bir veya daha fazla Asya içinde Azure bölgeleri
Güney Amerika ve Antarktika Bölgeleri | ABD içinden bir veya daha fazla Azure bölgeleri

Ülke/bölge değeri Azure AD dizin oluşturma işleminin bir parçası olarak ayarlayın ve daha sonra değiştirilemez. Veri depolama konumunuz hakkında daha fazla ayrıntı gerekiyorsa, bir dosya [Azure Destek](https://azure.microsoft.com/support/options/).

## <a name="view-per-user-device-sync-status"></a>Kullanıcı başına cihaz eşitleme durumunu görüntüle
Bir kullanıcı başına cihaz eşitleme durum raporunu görüntülemek için aşağıdaki adımları izleyin.

1. Oturum [Azure AD Yönetim Merkezi](https://aad.portal.azure.com/).

2. Seçin **Azure Active Directory** &gt; **kullanıcılar ve gruplar** &gt; **tüm kullanıcılar**.

3. Kullanıcıyı seçin ve ardından **aygıtları**.

4. Altında **Göster**seçin **ayarları ve uygulama verileri eşitleniyor aygıtları** eşitleme durumunu göstermek için.
  
  ![aygıt Eşitleme veri ayarı görüntüsü](./media/active-directory-windows-enterprise-state-roaming-enable/sync-status.png)
  
5. Bu kullanıcı için eşitleniyor cihazlar varsa, aşağıda gösterildiği gibi cihazlar bakın.
  
  ![aygıt Eşitleme sütunlu veri görüntüsü](./media/active-directory-windows-enterprise-state-roaming-enable/device-status-row.png)

## <a name="data-retention"></a>Veri saklama
Kurumsal durumda Dolaşım kullanarak Azure eşitlenen verileri el ile silinene kadar veya söz konusu veri eski olduğu belirlenir kadar korunur. 

### <a name="explicit-deletion"></a>Açık silme
Açık silinmesine Azure yönetici bir kullanıcı veya bir dizin siler veya aksi halde açıkça veri silinecek olan istekleri durumdur.

* **Kullanıcı silme**: bir kullanıcı Azure AD'de silindiğinde, veri gezici kullanıcı hesabı 90-180 gün sonra silinir. 
* **Dizin silme**: Azure AD içinde tüm dizin silinmesi hemen bir işlem olduğundan. Tüm ayarları veri ile dizin 90-180 gün sonra silinir ilişkilendirilmiş. 
* **İstek silme**: belirli bir kullanıcının veri veya ayar verileri el ile silmek Azure AD yönetim istiyorsa, bir yönetici dosya [Azure Destek](https://azure.microsoft.com/support/). 

### <a name="stale-data-deletion"></a>Eski veri silme
Bir yıl ("Bekletme dönemi") için erişilemeyen veri eski kabul edilir ve Azure'dan silinmiş. Saklama dönemi değiştirilebilir ancak 90 günden daha az olmayacak. Eski veriler Windows/uygulama ayarları veya bir kullanıcı için tüm ayarları belirli bir kümesi olabilir. Örneğin:

* Hiçbir aygıt belirli ayarları koleksiyonu erişirseniz (örneğin, bir uygulama CİHAZDAN kaldırılır veya ayarları grubu "Tema" gibi tüm kullanıcı aygıtları için devre dışı bırakılır), o koleksiyona saklama döneminden sonra eski haline gelir ve silinebilir . 
* Kullanıcı ayarları eşitleme güncelleştirmesini tüm cihazlarda kapalı, ayarları verilerin hiçbiri sonra erişilir ve bu kullanıcı için tüm ayarları veri eski olur ve saklama döneminden sonra silinebilir. 
* Azure AD directory Yöneticisi Kurumsal durumda dolaşım, tüm kullanıcılar gibi tüm dizin için dizin ayarları eşitleniyor durdurur ve tüm kullanıcılar için tüm ayarları verileri eski haline gelir ve saklama döneminden sonra silinebilir kapatırsa. 

### <a name="deleted-data-recovery"></a>Silinen veri kurtarma
Veri bekletme ilkesi yapılandırılabilir değildir. Verileri kalıcı olarak silindiğinde, kurtarılabilir değil. Ancak, yalnızca son kullanıcı aygıttan Azure gelen ayarları veriler silinir. Herhangi bir aygıt daha sonra kurumsal durumda Dolaşım hizmete bağlanırsa, ayarları yeniden eşitlenen ve Azure'da depolanır.

## <a name="related-topics"></a>İlgili konular
* [Kurumsal durumda Dolaşım genel bakış](active-directory-windows-enterprise-state-roaming-overview.md)
* [Ayarlar ve veri dolaşımı SSS](active-directory-windows-enterprise-state-roaming-faqs.md)
* [Grup İlkesi ve MDM ayarları ayarları eşitleme](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
* [Windows 10 Dolaşım ayarları başvurusu](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
* [Sorun giderme](active-directory-windows-enterprise-state-roaming-troubleshooting.md)
