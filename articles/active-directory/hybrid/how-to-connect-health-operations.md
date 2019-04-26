---
title: Azure Active Directory Connect Health işlemleri
description: Bu makalede, Azure AD Connect Health dağıttıktan sonra gerçekleştirilen ek işlemleri açıklar.
services: active-directory
documentationcenter: ''
author: zhiweiwangmsft
manager: daveba
ms.assetid: 86cc3840-60fb-43f9-8b2a-8598a9df5c94
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/18/2017
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 090a066afb24c4776f9844b8850264ffad842c59
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60350176"
---
# <a name="azure-active-directory-connect-health-operations"></a>Azure Active Directory Connect Health işlemleri
Bu konuda, Azure Active Directory (Azure AD) Connect Health kullanarak gerçekleştirebileceğiniz çeşitli işlemler açıklanmaktadır.

## <a name="enable-email-notifications"></a>E-posta bildirimlerini etkinleştirme
Uyarıları kimlik altyapınızı iyi durumda değil belirttiğinizde, e-posta bildirimleri göndermek için Azure AD Connect Health hizmeti yapılandırabilirsiniz. Bu, bir uyarı oluşturulduğunda ve Çözümlenmiş olduğunda gerçekleşir.

![Ekran görüntüsü, Azure AD Connect Health e-posta bildirimi ayarları](./media/how-to-connect-health-operations/email_noti_discover.png)

> [!NOTE]
> E-posta bildirimleri varsayılan olarak etkindir.
>
>

### <a name="to-enable-azure-ad-connect-health-email-notifications"></a>Azure AD Connect Health e-posta bildirimlerini etkinleştirmek için
1. Açık **uyarılar** e-posta bildirimi almak istediğiniz hizmet için dikey pencere.
2. Eylem çubuğundaki **bildirim ayarları**.
3. E-posta bildirim anahtarında seçin **ON**.
4. E-posta bildirimleri almak için tüm genel yöneticilerin istiyorsanız onay kutusunu işaretleyin.
5. Herhangi bir e-posta adreslerine e-posta bildirimleri almak istemiyorsanız, bunları belirlemek **ek e-posta alıcılarını** kutusu. Bir e-posta adresi bu listeden kaldırmak için girişe sağ tıklayın ve seçin **Sil**.
6. Değişiklikleri sonlandırmak için tıklatın **Kaydet**. Yalnızca kaydettikten sonra değişiklikler geçerli olacaktır.

## <a name="delete-a-server-or-service-instance"></a>Bir sunucu veya hizmet örneği silme

>[!NOTE] 
> Silme adımları için Azure AD premium lisansı gereklidir.

Bazı durumlarda, izlenmekte olan bir sunucuyu kaldırmak isteyebilirsiniz. Azure AD Connect Health hizmetinden bir sunucuyu kaldırmak için bilmeniz gerekenler aşağıda verilmiştir.

Bir sunucu silmekte olduğunuz, aşağıdakilere dikkat edin:

* Bu eylem bu sunucudan daha fazla veri toplamayı durdurur. Bu sunucu izleme hizmetinden kaldırılır. Bu eylemi gerçekleştirdikten sonra yeni uyarıları, izleme veya bu sunucu için kullanım analizi verilerini görüntülemek mümkün değildir.
* Bu eylem Health Aracısı sunucunuzdan kaldırmaz. Sistem Durumu Aracısı bu adımı gerçekleştirmeden önce kaldırmadıysanız sunucuda sistem durumu aracısı ile ilgili hatalar görebilirsiniz.
* Bu eylem bu sunucudan zaten toplanmış olan verileri silmez. Bu verileri Azure veri bekletme ilkesi uygun şekilde silinir.
* Bu eylemi gerçekleştirdikten sonra aynı sunucuyu izlemeye başlamak isterseniz yeniden, kaldırıp sistem durumu aracısı bu sunucuda yeniden yüklemeniz gerekir.

### <a name="delete-a-server-from-the-azure-ad-connect-health-service"></a>Azure AD Connect Health hizmetinden bir sunucu silme

>[!NOTE] 
> Silme adımları için Azure AD premium lisansı gereklidir.

Azure AD Connect Health'i Active Directory Federasyon Hizmetleri (AD FS) için ve Azure AD Connect (eşitleme):

1. Açık **sunucu** dikey penceresinden **sunucu listesi** kaldırılacak sunucu adını seçerek dikey.
2. Üzerinde **sunucu** eylem çubuğunda dikey **Sil**.
![Ekran görüntüsü, Azure AD Connect Health sunucu Sil](./media/how-to-connect-health-operations/DeleteServer2.png)
3. Onay kutusuna sunucu adını yazarak doğrulayın.
4. **Sil**'e tıklayın.

Azure AD Connect Health'i için Azure Active Directory etki alanı Hizmetleri:

1. Açık **etki alanı denetleyicileri** Pano.
2. Etki alanı denetleyicisinin kaldırılmasını seçin.
3. Eylem çubuğundaki **Sil Seçili**.
4. Sunucu silme eylemi onaylayın.
5. **Sil**'e tıklayın.

### <a name="delete-a-service-instance-from-azure-ad-connect-health-service"></a>Azure AD Connect Health hizmetinden bir hizmet örneği silme
Bazı durumlarda, bir hizmet örneği kaldırmak isteyebilirsiniz. Bir hizmet örneği Azure AD Connect Health hizmetten kaldırmak için bilmeniz gerekenler aşağıda verilmiştir.

Bir hizmet örneği silmekte olduğunuz, aşağıdakilere dikkat edin:

* Bu eylem izleme hizmetinden mevcut hizmet örneğinin kaldırır.
* Bu eylemi kaldırın veya bu hizmet örneği bir parçası olarak izlenen sunucuların sistem durumu aracısını kaldırın desteklemez. Sistem Durumu Aracısı bu adımı gerçekleştirmeden önce kaldırmadıysanız sunucularda sistem durumu aracısı ile ilgili hatalar görebilirsiniz.
* Bu hizmete ait tüm verileri Azure veri bekletme ilkesi uygun şekilde silinir.
* Bu eylemi gerçekleştirdikten sonra hizmeti izlemeye başlamak istiyorsanız, kaldırmak ve tüm sunucularda sistem durumu aracısını yeniden yükleyin. Bu eylemi gerçekleştirdikten sonra aynı sunucuyu izlemeye tekrar başlamak istiyorsanız, kaldırma, yeniden yükleyin ve bu sunucuda sistem durumu aracısı kaydedin.

#### <a name="to-delete-a-service-instance-from-the-azure-ad-connect-health-service"></a>Bir hizmet örneği Azure AD Connect Health hizmetinden silmek için
1. Açık **hizmet** dikey penceresinden **hizmet listesi** dikey penceresinde, kaldırmak istediğiniz hizmet tanımlayıcısı (grup adı) seçerek. 
2. Üzerinde **hizmet** eylem çubuğunda dikey **Sil**. 
![Ekran görüntüsü, Azure AD Connect Health hizmeti Sil](./media/how-to-connect-health-operations/DeleteServer.png)
3. Hizmet adını onay kutusuna yazarak onaylayın (örneğin: sts.contoso.com).
4. **Sil**'e tıklayın.
   <br><br>

[//]: # (RBAC bölümünün Başlat)
## <a name="manage-access-with-role-based-access-control"></a>Rol tabanlı erişim denetimi ile erişimi yönetme
[Rol tabanlı erişim denetimi (RBAC)](../../role-based-access-control/role-assignments-portal.md) kullanıcılar ve gruplar dışında genel Yöneticiler, Azure AD Connect Health erişim sağlar. RBAC rolleri için hedeflenen kullanıcılar atar ve gruplar ve genel Yöneticiler dizininizdeki sınırlamak için bir mekanizma sağlar.

### <a name="roles"></a>Roller
Azure AD Connect Health aşağıdaki yerleşik roller destekler:

| Rol | İzinler |
| --- | --- |
| Sahip |Sahipleri *erişimini yönetme* (örneğin, bir rol bir kullanıcı veya grup için atama), *tüm bilgileri görüntüleyebilir* (örneğin, uyarıları görüntüleme) Portal'dan ve *ayarlarını değiştirme* (için Örneğin, e-posta bildirimleri) Azure AD Connect Health içindeki. <br>Varsayılan olarak, Azure AD genel Yöneticiler bu role atanmış ve bu ayar değiştirilemez. |
| Katılımcı |Katkıda Bulunanlar için *tüm bilgileri görüntüleyebilir* (örneğin, uyarıları görüntüleme) Portal'dan ve *ayarlarını değiştirme* (örneğin, e-posta bildirimleri) Azure AD Connect Health içindeki. |
| Okuyucu |Okuyucuların *tüm bilgileri görüntüleyebilir* (örneğin, uyarıları görüntüleme) içinde Azure AD Connect Health portalından. |

Rolleri portal deneyimi içinde kullanılabilir olsa bile, diğer tüm rolleri (örneğin, kullanıcı erişim yöneticileri veya DevTest Labs kullanıcıların) Azure AD Connect Health, erişmek için herhangi bir etkisi sahip.

### <a name="access-scope"></a>Erişim kapsamı
Azure AD Connect Health, iki düzeyde yönetme erişimi destekler:

* **Tüm hizmet örnekleri**: Bu, çoğu durumda önerilen yoludur. Azure AD Connect Health tarafından izlenen tüm rol türlerinde tüm hizmet örnekleri (örneğin, bir AD FS grubu) için erişimi denetler.
* **Hizmet örneği**: Bazı durumlarda, erişim rol türleri veya bir hizmet örneği tarafından göre ayırmak gerekebilir. Bu durumda, hizmet örneği düzeyinde erişimi yönetebilir.  

Son kullanıcı erişimi düzeyinde dizin veya hizmet varsa izin verilir örnek düzeyi.

### <a name="allow-users-or-groups-access-to-azure-ad-connect-health"></a>Kullanıcıları veya grupları Azure AD Connect Health erişmesine izin ver
Aşağıdaki adımlarda, erişime izin verecek şekilde gösterilmektedir.
#### <a name="step-1-select-the-appropriate-access-scope"></a>1. Adım: Uygun erişim kapsamı seçin
Bir kullanıcı erişim izin vermek için *tüm hizmet örnekleri* Azure AD Connect Health düzey, Azure AD Connect Health ana dikey penceresini açın.<br>

#### <a name="step-2-add-users-and-groups-and-assign-roles"></a>2. Adım: Kullanıcılar ve gruplar ekleyin ve Rolleri Ata
1. Gelen **yapılandırma** bölümünde **kullanıcılar**.<br>
   ![Ekran görüntüsü, Azure AD Connect Health kaynak kenar çubuğu](./media/how-to-connect-health-operations/startRBAC.png)
2. **Add (Ekle)** seçeneğini belirleyin.
3. İçinde **bir rol seçin** bölmesinde, bir rol seçin (örneğin, **sahibi**).<br>
   ![Ekran Azure AD Health RBAC kullanıcıların bağlanmasına penceresi](./media/how-to-connect-health-operations/RBAC_add.png)
4. Adına veya tanımlayıcısına hedeflenen kullanıcı veya grubun yazın. Aynı anda bir veya daha fazla kullanıcılar veya gruplar seçebilirsiniz. **Seç**'e tıklayın.
   ![Ekran Azure AD Health RBAC kullanıcıların bağlanmasına penceresi](./media/how-to-connect-health-operations/RBAC_select_users.png)
5. **Tamam**’ı seçin.<br>
6. Rol ataması tamamlandıktan sonra kullanıcıları ve grupları listesinde görünür.<br>
   ![Vurgulanmış yeni kullanıcılarla ekran Azure AD Health RBAC kullanıcıların bağlanmasına penceresi](./media/how-to-connect-health-operations/RBAC_user_list.png)

Listelenen kullanıcılar ve gruplar, atanan rollerinin göre yararlanabiliyor.

> [!NOTE]
> * Genel yöneticiler her zaman tüm işlemleri tam erişime sahiptir, ancak genel yönetici hesapları yukarıdaki listede yok.
> * Azure AD Connect Health kullanıcılar davet özelliği desteklenmiyor.
>
>

#### <a name="step-3-share-the-blade-location-with-users-or-groups"></a>3. Adım: Dikey konumu kullanıcılar veya gruplar ile paylaşma
1. İzinleri atadıktan sonra bir kullanıcının Azure AD Connect Health giderek erişebildiği [burada](https://aka.ms/aadconnecthealth).
2. Dikey penceresinde kullanıcı, dikey ya da farklı bölümlerini, panoya sabitleyebilirsiniz. Tıklamanız yeterlidir **panoya Sabitle** simgesi.<br>
   ![PIN simgesinin vurgulandığı ekran görüntüsü, Azure AD Connect Health RBAC PIN dikey](./media/how-to-connect-health-operations/RBAC_pin_blade.png)

> [!NOTE]
> Okuyucu rolüne atanmış olan bir kullanıcı Azure AD Connect Health uzantısını Azure Market'ten Al mümkün değil. Kullanıcı, "oluşturma işlemi, bunu yapmak için" gerekli gerçekleştirilemiyor. Kullanıcı için yukarıdaki bağlantıya giderek için dikey yine de alabilirsiniz. Kullanıcı, sonraki kullanım için dikey pencereyi panoya sabitleyebilirsiniz.
>
>

### <a name="remove-users-or-groups"></a>Kullanıcılarını veya gruplarını kaldırın
Bir kullanıcı ya da Azure AD Connect Health RBAC için eklenen bir grubu kaldırabilirsiniz. Yalnızca kullanıcı veya grup sağ tıklatın ve seçin **Kaldır**.<br>
![Ekran görüntüsü, Azure AD Health RBAC kullanıcıların bağlanmasına penceresinin vurgulanmış Kaldır](./media/how-to-connect-health-operations/RBAC_remove.png)

[//]: # (RBAC bölüm sonu)

## <a name="next-steps"></a>Sonraki adımlar
* [Azure AD Connect Health](whatis-hybrid-identity-health.md)
* [Azure AD Connect Health Aracısı yüklemesi](how-to-connect-health-agent-install.md)
* [Azure AD Connect Health'i AD FS ile Kullanma](how-to-connect-health-adfs.md)
* [Eşitleme için Azure AD Connect Health'i kullanma](how-to-connect-health-sync.md)
* [Azure AD Connect Health'i AD DS ile Kullanma](how-to-connect-health-adds.md)
* [Azure AD Connect Health ile ilgili SSS](reference-connect-health-faq.md)
* [Azure AD Connect Health sürüm geçmişi](reference-connect-health-version-history.md)
