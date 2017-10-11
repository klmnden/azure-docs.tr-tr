---
title: "Azure Active Directory Connect Health işlemleri"
description: "Bu makalede, Azure AD Connect Health dağıttıktan sonra gerçekleştirilebilir ek işlemleri açıklanır."
services: active-directory
documentationcenter: 
author: karavar
manager: femila
ms.assetid: 86cc3840-60fb-43f9-8b2a-8598a9df5c94
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 06afc6b4149ea1590a2994d1638d6979a89035e3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-connect-health-operations"></a>Azure Active Directory Connect Health işlemleri
Bu konuda, Azure Active Directory (Azure AD) Connect Health kullanarak gerçekleştirebileceğiniz çeşitli işlemler açıklanmaktadır.

## <a name="enable-email-notifications"></a>E-posta bildirimlerini etkinleştir
Uyarılar kimlik altyapınızı sağlıklı olmadığını belirtir, e-posta bildirimleri göndermek için Azure AD Connect Health hizmetine yapılandırabilirsiniz. Bu, bir uyarı oluşturulduğunda ve giderildikten sonra oluşur.

![Ekran Azure AD Connect Health e-posta bildirim ayarları](./media/active-directory-aadconnect-health/email_noti_discover.png)

> [!NOTE]
> E-posta bildirimleri varsayılan olarak etkinleştirilir.
>
>

### <a name="to-enable-azure-ad-connect-health-email-notifications"></a>Azure AD Connect Health e-posta bildirimlerini etkinleştirmek için
1. Açık **uyarıları** e-posta bildirimi almak istediğiniz hizmet için dikey.
2. Eylem çubuğundaki **bildirim ayarlarını**.
3. E-posta bildirim anahtarında seçin **ON**.
4. E-posta bildirimleri almak için tüm genel Yöneticiler istiyorsanız onay kutusunu seçin.
5. Başka bir e-posta adreslerini e-posta bildirimleri almak istiyorsanız, bunları belirtin **ek e-posta alıcılarını** kutusu. Bir e-posta adresi bu listeden kaldırmak için girişi sağ tıklatın ve seçin **silmek**.
6. Değişiklikleri sonlandırmak için tıklatın **kaydetmek**. Yalnızca kaydettikten sonra değişiklikler etkili olur.

## <a name="delete-a-server-or-service-instance"></a>Bir sunucu veya hizmet örneğini silin

Bazı durumlarda, izlenmekte olan bir sunucuyu kaldırmak isteyebilirsiniz. Azure AD Connect Health hizmetinden bir sunucuyu kaldırmak için bilmeniz gerekenleri burada verilmiştir.

Bir sunucu silmekte olduğunuz, aşağıdakilere dikkat edin:

* Bu eylem bu sunucudan daha fazla veri toplamayı durdurur. Bu sunucu izleme hizmetinden kaldırılır. Bu işlemin ardından, yeni uyarılar, izleme veya bu sunucu için kullanım analizi verilerini görüntülemek mümkün değildir.
* Bu eylem sistem durumu aracısı sunucunuzdan kaldırmaz. Sistem Durumu Aracısı bu adımı uygulamadan önce kaldırmadıysanız sunucuda sistem durumu aracısı ile ilgili hatalar görebilirsiniz.
* Bu eylem bu sunucudan zaten toplanmış verileri silmez. Bu verileri Azure veri bekletme ilkesiyle uygun şekilde silinir.
* Bu eylemi gerçekleştirdikten sonra aynı sunucu izleme başlatmak istiyorsanız yeniden, kaldırıp sistem durumu aracısı bu sunucuya yeniden yüklemeniz gerekir.

### <a name="to-delete-a-server-from-the-azure-ad-connect-health-service"></a>Bir sunucu Azure AD Connect Health hizmetinden silmek için
Azure AD Connect Health için Active Directory Federasyon Hizmetleri (AD FS) ve Azure AD Connect (eşitleme):

1. Açık **Server** dikey penceresinden **sunucu listesi** dikey kaldırılacak sunucu adını seçin.
2. Üzerinde **Server** eylem çubuğunda dikey tıklayın **silmek**.
3. Onay kutusuna sunucu adını yazarak onaylayın.
4. **Sil**'e tıklayın.

Azure AD Connect Health için Azure Active Directory etki alanı Hizmetleri:

1. Açık **etki alanı denetleyicileri** Pano.
2. Kaldırılacak etki alanı denetleyicisini seçin.
3. Eylem çubuğundaki **Sil Seçili**.
4. Sunucu silme eylemi onaylayın.
5. **Sil**'e tıklayın.

### <a name="delete-a-service-instance-from-azure-ad-connect-health-service"></a>Bir hizmet örneği Azure AD Connect Health hizmetinden Sil
Bazı durumlarda, bir hizmet örneği kaldırmak isteyebilirsiniz. Bir hizmet örneği Azure AD Connect Health hizmetinden kaldırmak için bilmeniz gerekenleri burada verilmiştir.

Bir hizmet örneği silmekte olduğunuz, aşağıdakilere dikkat edin:

* Bu eylem izleme hizmetinden mevcut hizmet örneğinin kaldırır.
* Bu eylem bırakmaz kaldırın veya bu hizmet örneği bir parçası olarak izlenmekte sunuculardan herhangi biri sistem durumu aracısını kaldırın. Sistem Durumu Aracısı bu adımı uygulamadan önce kaldırmadıysanız sunucularda sistem durumu aracısı ile ilgili hatalar görebilirsiniz.
* Azure veri bekletme ilkesiyle uygun şekilde bu hizmete ait tüm veriler silinir.
* Bu eylemi gerçekleştirdikten sonra aynı hizmeti izlemeye başlamak istiyorsanız, kaldırın ve tüm sunucularda sistem durumu aracısı yükleyin. Bu eylemi gerçekleştirdikten sonra aynı sunucuyu izlemeye tekrar başlamak istiyorsanız, kaldırma, yeniden yükleyin ve bu sunucuda sistem durumu aracısı kaydedin.

#### <a name="to-delete-a-service-instance-from-the-azure-ad-connect-health-service"></a>Bir hizmet örneği Azure AD Connect Health hizmetinden silmek için
1. Açık **hizmet** dikey penceresinden **hizmet listesi** kaldırmak istediğiniz hizmet tanımlayıcısını (grup adı) seçerek dikey.
2. Üzerinde **Server** eylem çubuğunda dikey tıklayın **silmek**.
3. Onay kutusuna hizmet adını yazarak onaylayın (örneğin: sts.contoso.com).
4. **Sil**'e tıklayın.
   <br><br>

[//]: # (Start of RBAC section)
## <a name="manage-access-with-role-based-access-control"></a>Rol tabanlı erişim denetimi ile erişimi yönetme
[Rol tabanlı erişim denetimi (RBAC)](../role-based-access-control-configure.md) kullanıcılar ve gruplar dışında genel Yöneticiler için Azure AD Connect Health erişim sağlar. RBAC atar rolleri hedeflenen kullanıcılara ve gruplara ve dizininizde genel Yöneticiler sınırlamak için bir mekanizma sağlar.

### <a name="roles"></a>Roller
Azure AD Connect Health aşağıdaki yerleşik roller destekler:

| Rol | İzinler |
| --- | --- |
| Sahip |Sahipleri için *erişimini yönetme* (örneğin, bir rol için bir kullanıcı veya Grup Ata), *tüm bilgileri görüntüleyebilir* (örneğin, uyarıları görüntüleme) Portal'dan ve *ayarlarını değiştirme* (için Örneğin, e-posta bildirimleri) Azure AD Connect Health içinde. <br>Varsayılan olarak, Azure AD genel Yöneticiler bu role atanmış ve bu değiştirilemez. |
| Katılımcı |Katkıda Bulunanlar yapabilirsiniz *tüm bilgileri görüntüleyebilir* (örneğin, uyarıları görüntüleme) Portal'dan ve *ayarlarını değiştirme* (örneğin, e-posta bildirimleri) Azure AD Connect Health içinde. |
| Okuyucu |Okuyucuların *tüm bilgileri görüntüleyebilir* (örneğin, uyarıları görüntüleme) Azure AD Connect Health içinde portalından. |

Rolleri portal deneyimi kullanılabilir olsa bile diğer tüm rolleri (örneğin, kullanıcı erişim yöneticileri veya DevTest Labs kullanıcıların) Azure AD Connect Health içinde erişmek için hiçbir etkisi.

### <a name="access-scope"></a>Erişim kapsamı
Azure AD Connect Health, iki düzeyde erişimi yönetme destekler:

* **Tüm hizmet örnekleri**: Bu çoğu durumda önerilen yoldur. Azure AD Connect Health tarafından izlenen tüm rol türleri arasındaki tüm hizmet örnekleri (örneğin, bir AD FS grubunu) erişimi denetler.
* **Hizmet örneği**: Bazı durumlarda, rol türleri veya bir hizmet örneği tarafından dayalı erişim kurabilmeleri gerekebilir. Bu durumda, hizmet örnek düzeyinde erişimi yönetebilirsiniz.  

Bir son kullanıcı erişimi ya da dizin veya hizmet varsa izin verilir örnek düzeyi.

### <a name="allow-users-or-groups-access-to-azure-ad-connect-health"></a>Kullanıcıları veya grupları erişim Azure AD Connect Health için izin ver
Aşağıdaki adımlar erişime izin verecek şekilde nasıl gösterir.
#### <a name="step-1-select-the-appropriate-access-scope"></a>1. adım: uygun erişim kapsamını seçin
Bir kullanıcının erişim izin vermek için *tüm hizmet örnekleri* düzey Azure AD Connect Health içinde Azure AD Connect Health ana dikey penceresini açın.<br>

#### <a name="step-2-add-users-and-groups-and-assign-roles"></a>2. adım: Kullanıcıları ve grupları ekleyin ve Rolleri Ata
1. Gelen **yapılandırma** 'yi tıklatın **kullanıcılar**.<br>
   ![Ekran Azure AD Connect sistem durumu RBAC ana dikey, vurgulanmış kullanıcılarla](./media/active-directory-aadconnect-health/RBAC_main_blade.png)
2. **Add (Ekle)** seçeneğini belirleyin.
3. İçinde **bir rol seçin** bölmesinde, bir rol seçin (örneğin, **sahibi**).<br>
   ![Azure ekran AD sistem durumu RBAC kullanıcıların bağlanmasına penceresi](./media/active-directory-aadconnect-health/RBAC_add.png)
4. Adı veya tanımlayıcısı hedeflenen kullanıcı veya grup yazın. Aynı anda bir veya daha fazla kullanıcı veya grup seçebilirsiniz. **Seç**'e tıklayın.
   ![Azure ekran AD sistem durumu RBAC kullanıcıların bağlanmasına penceresi](./media/active-directory-aadconnect-health/RBAC_select_users.png)
5. **Tamam**’ı seçin.<br>
6. Rol ataması tamamlandıktan sonra kullanıcılar ve gruplar listesinde görünür.<br>
   ![Vurgulanan yeni kullanıcılar ile Azure ekran AD sistem durumu RBAC kullanıcıların bağlanmasına penceresi](./media/active-directory-aadconnect-health/RBAC_user_list.png)

Şimdi listelenen kullanıcılar ve gruplar, atanan rollerinin göre erişimi.

> [!NOTE]
> * Genel yöneticilerin her zaman tüm işlemlere tam erişimi vardır, ancak genel yönetici hesapları yukarıdaki listede yer yok.
> * Kullanıcıların davet Özelliği Azure AD Connect Health içinde desteklenmez.
>
>

#### <a name="step-3-share-the-blade-location-with-users-or-groups"></a>3. adım: kullanıcıları veya grupları dikey konumu paylaşma
1. İzinleri atadıktan sonra bir kullanıcı Azure AD Connect Health giderek erişebilir [burada](http://aka.ms/aadconnecthealth).
2. Dikey penceresinde kullanıcı dikey veya bunu farklı bölümleri panoya sabitleyebilirsiniz. Tıklamanız yeterlidir **panoya Sabitle** simgesi.<br>
   ![Vurgulanan PIN simgesiyle ekran Azure AD Connect sistem durumu RBAC PIN dikey](./media/active-directory-aadconnect-health/RBAC_pin_blade.png)

> [!NOTE]
> Atanan okuyucu rolüne sahip bir kullanıcı Azure Marketi'nden Azure AD Connect Health uzantıyı almak mümkün değil. Kullanıcı "Bunu yapmak için işlem oluşturma" gerekli gerçekleştiremezsiniz. Kullanıcı, önceki bağlantısını giderek dikey penceresine hala alabilirsiniz. Sonraki kullanım için kullanıcı panoya dikey pencereyi sabitleyebilirsiniz.
>
>

### <a name="remove-users-or-groups"></a>Kullanıcıları veya grupları kaldırın
Bir kullanıcı veya Azure AD Connect sistem durumu RBAC eklenen bir grup kaldırabilirsiniz. Sadece kullanıcı veya grubunu sağ tıklatın ve seçin **kaldırmak**.<br>
![Vurgulanan Kaldır ile Azure ekran AD sistem durumu RBAC kullanıcıların bağlanmasına penceresi](./media/active-directory-aadconnect-health/RBAC_remove.png)

[//]: # (End of RBAC section)

## <a name="next-steps"></a>Sonraki adımlar
* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Azure AD Connect Health Aracısı yüklemesi](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect Health'i AD FS ile Kullanma](active-directory-aadconnect-health-adfs.md)
* [Eşitleme için Azure AD Connect Health'i kullanma](active-directory-aadconnect-health-sync.md)
* [Azure AD Connect Health'i AD DS ile Kullanma](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health ile ilgili SSS](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect Health sürüm geçmişi](active-directory-aadconnect-health-version-history.md)
