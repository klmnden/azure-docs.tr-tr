---
title: "Azure AD Connect: Sürüm yayımlama geçmişi | Microsoft Docs"
description: "Bu makalede Azure AD Connect ve Azure AD eşitleme'nin tüm sürümlerinde listeler"
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: ef2797d7-d440-4a9a-a648-db32ad137494
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/14/2017
ms.author: billmath
ms.openlocfilehash: ff43edc9799670fd90beaef1dbe4db48b2e762e5
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
# <a name="azure-ad-connect-version-release-history"></a>Azure AD Connect: Sürüm yayımlama geçmişi
Azure Active Directory (Azure AD) ekibin yeni özellikler ve işlevsellik ile Azure AD Connect düzenli olarak güncelleştirir. Tüm eklemeleri tüm izleyiciler için geçerlidir.
' Bu makalede yayımlanan sürümleri izlemenize yardımcı olacak ve en yeni sürüme veya güncelleştirme gerekip gerekmediğini anlamak için tasarlanmıştır.

İlgili Konular listesidir:



Konu |  Ayrıntılar
--------- | --------- |
Azure AD Connect'ten yükseltme adımları | İçin farklı yöntemler [en son önceki bir sürümünden yükseltme](active-directory-aadconnect-upgrade-previous-version.md) Azure AD Connect sürüm.
Gerekli izinler | Bir güncelleştirmeyi uygulamak için gereken izinler için bkz: [hesapları ve izinleri](./active-directory-aadconnect-accounts-permissions.md#upgrade).

Karşıdan yükleme | [Azure AD Connect'i indirme](http://go.microsoft.com/fwlink/?LinkId=615771).

## <a name="116540"></a>1.1.654.0
Durumu: 12 Aralık 2017

>[!NOTE]
>Bu bir güvenlik olan Azure AD Connect için ilgili düzeltme

### <a name="azure-ad-connect"></a>Azure AD Connect
Bir geliştirme önerilen izni altında açıklanan bölüm değiştirdiğinden emin olmak için Azure AD Connect sürüm 1.1.654.0 (ve sonra) eklendi [AD DS hesabı için erişim kilitleme](#lock) otomatik olarak zaman uygulanan Azure AD Connect AD DS hesabı oluşturur. 

- Azure AD Connect ayarlarken, yükleme yönetici mevcut bir AD DS hesap sağlayın veya Azure AD Connect otomatik olarak hesabı oluşturmak istiyorum. İzin değişiklikleri otomatik olarak Azure AD Connect tarafından kurulumu sırasında oluşturulan AD DS hesabı için uygulanır. Yükleme yönetici tarafından sağlanan mevcut AD DS hesabı için uygulanmaz.
- Azure AD Connect eski bir sürümden izni 1.1.654.0 (veya sonra), yükselten müşterilerin için değişiklikleri firmalarda geriye dönük yükseltmeden önce oluşturulan mevcut AD DS hesaplarına uygulanmaz. Bunlar yalnızca yükseltme işleminden sonra oluşturulan yeni AD DS hesaplara uygulanır. Bu durum, Azure AD ile eşitlenecek yeni AD ormanına eklemekte olduğunuz oluşur.

>[!NOTE]
>Bu sürüm yalnızca hizmet hesabı yükleme işleminin oluşturulduğu Azure AD Connect yeni yüklemeler için güvenlik açığı kaldırır. Var olan yüklemeler için ya da hesap kendiniz verdiğiniz durumlarda bu güvenlik açığı yok emin olmalısınız.

#### <a name="lock"></a>AD DS hesabı için erişim kilitleme
Şirket içi aşağıdaki izin değişiklikleri uygulayarak AD DS hesabı için erişim kilitleme AD:  

*   Belirtilen nesne devralmayı devre dışı bırak
*   Tüm ACE'ler KENDİNE özgü ACE dışında belirli nesne üzerinde kaldırın. Varsayılan izinleri KENDİSİNE geldiğinde korumanız istiyoruz.
*   Bu özel izinleri atayın:

Tür     | Ad                          | Access               | Şunun İçin Geçerli
---------|-------------------------------|----------------------|--------------|
İzin Ver    | SİSTEM                        | Tam Denetim         | Bu nesne  |
İzin Ver    | Enterprise Admins             | Tam Denetim         | Bu nesne  |
İzin Ver    | Etki alanı yöneticileri                 | Tam Denetim         | Bu nesne  |
İzin Ver    | Yöneticiler                | Tam Denetim         | Bu nesne  |
İzin Ver    | Kuruluş etki alanı denetleyicileri | İçeriğini listele        | Bu nesne  |
İzin Ver    | Kuruluş etki alanı denetleyicileri | Tüm özellikleri oku  | Bu nesne  |
İzin Ver    | Kuruluş etki alanı denetleyicileri | Okuma izinleri     | Bu nesne  |
İzin Ver    | Kimliği doğrulanmış kullanıcılar           | İçeriğini listele        | Bu nesne  |
İzin Ver    | Kimliği doğrulanmış kullanıcılar           | Tüm özellikleri oku  | Bu nesne  |
İzin Ver    | Kimliği doğrulanmış kullanıcılar           | Okuma izinleri     | Bu nesne  |

AD DS hesabı için ayarları artırmak için çalıştırabilirsiniz [bu PowerShell Betiği](https://gallery.technet.microsoft.com/Prepare-Active-Directory-ef20d978). PowerShell komut dosyası, AD DS hesabı için yukarıdaki izinleri atar.

#### <a name="powershell-script-to-tighten-a-pre-existing-service-account"></a>Önceden var olan bir hizmet hesabı artırmak için PowerShell Betiği

Önceden var olan bir AD DS hesabı için bu ayarları uygulamak için PowerShell Betiği kullanmak (kuruluşunuz tarafından sağlanan veya Azure AD Connect, önceki bir yüklemesi tarafından oluşturulan ether Lütfen karşıdan yükleme komut dosyası yukarıdaki sağlanan bağlantıdan.

##### <a name="usage"></a>Kullanım:

```powershell
Set-ADSyncRestrictedPermissions -ObjectDN <$ObjectDN> -Credential <$Credential>
```

Burada 

**$ObjectDN** = Active Directory hesap izinlerini sıkılaştırıldığını gerekir.

**$Credential** $ObjectDN hesap izinlerini kısıtlamak için gerekli ayrıcalıklara sahip yönetici kimlik bilgileri =. Bu genellikle kuruluş veya etki alanı yönetici olur. Hesap arama hatalarını önlemek için yönetici hesabı tam etki alanı adını kullanın. Örnek: contoso.com\admin.

>[!NOTE] 
>$credential. Kullanıcı adı FQDN\username biçiminde olmalıdır. Örnek: contoso.com\admin 

##### <a name="example"></a>Örnek:

```powershell
Set-ADSyncRestrictedPermissions -ObjectDN "CN=TestAccount1,CN=Users,DC=bvtadwbackdc,DC=com" -Credential $credential 
```
### <a name="was-this-vulnerability-used-to-gain-unauthorized-access"></a>Bu güvenlik açığından izinsiz erişim için kullanılan?

Bu güvenlik açığından Azure AD aşmaya kullanılan olmadığını görmek için hizmet hesabı tarihi son parola doğrulamalısınız Connect yapılandırması sıfırlayın.  Varsa beklenmeyen timestamp, daha fazla araştırma, olay günlüğü, bu parolayı sıfırlama olayı için üstlendiği.

Daha fazla bilgi için bkz: [Microsoft güvenlik önerisi 4056318](https://technet.microsoft.com/library/security/4056318)

## <a name="116490"></a>1.1.649.0
Durum: 27 Ekim 2017

>[!NOTE]
>Bu yapı müşteriler için Azure AD Connect otomatik yükseltmesi özelliği aracılığıyla kullanılabilir değil.

### <a name="azure-ad-connect"></a>Azure AD Connect
#### <a name="fixed-issue"></a>Sabit sorunu
* Azure AD Connect ve Azure AD Connect Health Aracısı (için eşitleme) arasında bir sürüm uyumluluk sorunu sabit. Bu sorunu Azure AD Connect 1.1.647.0 sürümüne yerinde yükseltme gerçekleştirme müşterileri etkiler, ancak şu anda sistem durumu aracısı sürüm 3.0.127.0 yok. Yükseltmeden sonra sistem durumu aracısı artık Azure AD sistem durumu hizmeti için Azure AD Connect eşitleme hizmeti ilgili sağlık verilerini gönderebilirsiniz. Bu düzeltme, sistem durumu aracısı sürüm 3.0.129.0 Azure AD Connect yerinde yükseltme sırasında yüklenir. Sistem Durumu Aracısı sürüm 3.0.129.0 Azure AD Connect sürüm 1.1.649.0 uyumluluk sorunu yok.


## <a name="116470"></a>1.1.647.0
Durum: 19 Ekim 2017

> [!IMPORTANT]
> Azure AD Connect sürüm 1.1.647.0 ve Azure AD Connect Health Aracısı (için eşitleme) sürüm 3.0.127.0 arasında bir bilinen bir uyumluluk sorunu yok. Bu sorunu sistem durumu aracısı, Azure AD sistem durumu hizmeti için Azure AD Connect eşitleme (Nesne eşitleme hatalarının ve çalıştırma geçmişi veriler dahil) hizmeti ilgili sağlık verilerini göndermesini engeller. El ile Azure AD Connect dağıtımınızı 1.1.647.0 sürümüne yükseltmeden önce lütfen geçerli sürümü Azure AD Connect Health Aracısı, Azure AD Connect sunucunuzda yüklü doğrulayın. Giderek bunu yapabilirsiniz *Denetim Masası → Program Ekle/Kaldır* ve uygulama için Ara *Microsoft Azure AD Connect Health aracısını eşitleme için*. Kendi sürümü 3.0.127.0 ise, yükseltmeden önce kullanılabilir olması sonraki Azure AD Connect sürüm beklemeniz önerilir. Sistem Durumu Aracısı'nın sürümünü 3.0.127.0 yoksa, el ile yerinde yükseltme işlemine devam etmek uygundur. Bu sorunu Esnek yükseltme veya Azure AD Connect yeni yüklemesi gerçekleştirme müşteriler etkilemez unutmayın.
>
>

### <a name="azure-ad-connect"></a>Azure AD Connect
#### <a name="fixed-issues"></a>Giderilen sorunlar
* İle ilgili bir sorunu sabit *değiştirme kullanıcı oturum açma* görev Azure AD Connect Sihirbazı'nda:

  * Parola Eşitleme ile var olan bir Azure AD Connect dağıtımını olduğunda sorun ortaya **etkin**, ve oturum açma kullanıcı yöntemi olarak ayarlamaya çalıştığınız *doğrudan kimlik doğrulama*. Değişikliği uygulamadan önce sihirbaz yanlış gösterir "*parola eşitleme devre dışı*" istemi. Ancak, değişikliği uygulandıktan sonra parola eşitleme etkin halde kalır. Bu düzeltme, sihirbaz artık istemi gösterir.

  * Kullanıcı oturum açma yöntemini kullanarak güncelleştirdiğinizde tasarım gereği, sihirbazın parola eşitleme devre dışı bırakmaz *değiştirme kullanıcı oturum açma* görev. Doğrudan kimlik doğrulama veya bunların birincil kullanıcı oturum açma yöntemi olarak Federasyon etkinleştiriyorsanız olsa bile parola eşitleme, korumak istediğiniz müşterileri kesintisi yaşamamak için budur.
  
  * Oturum açma kullanıcı yöntemi güncelleştirdikten sonra parola eşitleme devre dışı bırakmak istiyorsanız, yürütülmelidir *özelleştirme eşitleme yapılandırması* Sihirbazı'nda görev. Ne zaman gezinmek için *isteğe bağlı özellikler* sayfasında, onay kutusunu temizleyin *parola eşitleme* seçeneği.
  
  * Sorunsuz çoklu oturum açmayı etkinleştir/devre dışı bırak çalışırsanız, aynı sorunu da oluştuğunu unutmayın. Özellikle, parola eşitleme etkin var olan bir Azure AD Connect dağıtıma sahip ve kullanıcı oturum açma yöntemi olarak zaten yapılandırıldı *doğrudan kimlik doğrulama*. Kullanarak *değiştirme kullanıcı oturum açma* görevi denediğinizde onay/işaretini kaldırın *etkinleştirmek sorunsuz çoklu oturum açma* oturum açma kullanıcı yöntemi "Geçişli kimlik doğrulaması" yapılandırılmış kalırken seçeneği. Değişikliği uygulamadan önce sihirbaz yanlış gösterir "*parola eşitleme devre dışı*" istemi. Ancak, değişikliği uygulandıktan sonra parola eşitleme etkin halde kalır. Bu düzeltme, sihirbaz artık istemi gösterir.

* İle ilgili bir sorunu sabit *değiştirme kullanıcı oturum açma* görev Azure AD Connect Sihirbazı'nda:

   * Parola Eşitleme ile var olan bir Azure AD Connect dağıtımını olduğunda sorun ortaya **devre dışı**, ve oturum açma kullanıcı yöntemi olarak ayarlamaya çalıştığınız *doğrudan kimlik doğrulama*. Değişiklik uygulandığında sihirbazın doğrudan kimlik doğrulama ve parola eşitleme sağlar. Bu düzeltmeyle Sihirbazı artık parola eşitleme sağlar.

  * Daha önce parola eşitleme, geçişli kimlik doğrulamasını etkinleştirmek için bir önkoşul oluştu. Oturum açma kullanıcı yöntemi olarak ayarlandığında *doğrudan kimlik doğrulama*, geçişli kimlik doğrulaması ve Parola Eşitleme Sihirbazı'nı etkinleştirir. Yakın zamanda, parola eşitleme, bir önkoşul kaldırıldı. Azure AD Connect sürüm 1.1.557.0 bir parçası olarak, oturum açma kullanıcı yöntemi olarak ayarladığınızda, parola eşitlemeyi etkinleştirmek için Azure AD Connect'e bir değişiklik yapılmıştır *doğrudan kimlik doğrulama*. Ancak, değişikliği yalnızca Azure AD Connect'i yüklemeye uygulandı. Bu düzeltme ile aynı değişiklik de uygulanır *değiştirme kullanıcı oturum açma* görev.
  
  * Sorunsuz çoklu oturum açmayı etkinleştir/devre dışı bırak çalışırsanız, aynı sorunu da oluştuğunu unutmayın. Özellikle, parola eşitleme devre dışı var olan bir Azure AD Connect dağıtıma sahip ve kullanıcı oturum açma yöntemi olarak zaten yapılandırılmış *doğrudan kimlik doğrulama*. Kullanarak *değiştirme kullanıcı oturum açma* görevi denediğinizde onay/işaretini kaldırın *etkinleştirmek sorunsuz çoklu oturum açma* oturum açma kullanıcı yöntemi "Geçişli kimlik doğrulaması" yapılandırılmış kalırken seçeneği. Değişiklik uygulandığında Sihirbazı Parola Eşitleme sağlar. Bu düzeltmeyle Sihirbazı artık parola eşitleme sağlar. 

* Hata ile Azure AD Connect yükseltmesi başarısız olmasına neden olan sorunu sabit "*Eşitleme Hizmeti'ni yükseltme yüklenemiyor*". Ayrıca, eşitleme hizmeti artık olay hatasıyla başlatmak için "*hizmeti, veritabanı sürümü yüklü ikili sürümü daha yeni olduğundan dolayı başlatılamadı*". Yükseltme yapan Yöneticisi Azure AD Connect tarafından kullanılan SQL Server Sistem Yöneticisi ayrıcalığına sahip değil sorun ortaya çıkar. Bu düzeltme ile Azure AD Connect yalnızca yükseltme sırasında ADSync veritabanı için db_owner ayrıcalığına sahip için yönetici gerektirir.

* Etkin müşteriler etkilenen bir Azure AD Connect Yükseltmesi sorunu sabit [sorunsuz çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso). Azure AD Connect yükseltildikten sonra özelliği etkin ve tamamen işlevsel kalır rağmen sorunsuz çoklu oturum açma hatalı Azure AD Connect Sihirbazı'nda, devre dışı olarak görünür. Bu düzeltmeyle özelliği artık doğru Sihirbazı'nda olarak etkinleştirilmiş görüntülenir.

* Azure AD Connect Sihirbazı her zaman göster neden olan sorunu sabit "*kaynak bağlantısı yapılandırma*" üzerinde komut istemi *yapılandırma için hazır* sayfasında bile kaynak bağlantısı için ilgili hiçbir değişiklik yapılmadı.

* Azure AD Connect el ile yerinde yükseltme yaparken, müşteri karşılık gelen Azure AD kiracısı genel yönetici kimlik bilgilerini sağlamak için gereklidir. Daha önce sağlanan genel yönetici kimlik bilgileri ait olsa da farklı bir Azure yükseltme devam AD Kiracı. Yükseltme başarıyla tamamlamak için görüntülenirken, belirli yapılandırmaları doğru yükseltmeye kalıcı değildir. Bu değişiklikle, sihirbazın sağlanan kimlik bilgileri Azure AD kiracısı eşleşmiyorsa, devam etmek için Yükselt izin vermez.

* Azure AD Connect Health hizmeti el ile yükseltme işleminin başında gereksiz yere yeniden yedekli mantığı kaldırıldı.


#### <a name="new-features-and-improvements"></a>Yeni özellikleri ve geliştirmeleri
* Azure AD Connect Microsoft Almanya bulut ile ayarlamak için gerekli olan adımları basitleştirmek için eklenen mantığı. Daha önce bu makalede anlatıldığı gibi belirli kayıt defteri anahtarlarını Microsoft Almanya Bulutla doğru çalışması için Azure AD Connect sunucusu üzerinde güncelleştirmek için gereklidir. Şimdi, Azure AD Connect otomatik olarak Kiracı Microsoft Almanya bulutta Kurulum sırasında sağlanan genel yönetici kimlik bilgileri dayanır, algılayabilir.

### <a name="azure-ad-connect-sync"></a>Azure AD Connect Sync
>[!NOTE]
> Not: Eşitleme hizmeti, kendi özel Zamanlayıcı geliştirmenize olanak sağlayan bir WMI arabirimine sahiptir. Bu arabirim artık kullanım dışıdır ve gelecekteki kaldırılacak Azure AD Connect'in sürümlerinde sevk 30 Haziran 2018 sonra. Eşitleme zamanlaması özelleştirmek istediğiniz müşteriler [yerleşik Zamanlayıcı (https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-feature-scheduler). kullanmanız gerekir

#### <a name="fixed-issues"></a>Giderilen sorunlar
* Azure AD Connect Sihirbazı değişiklikleri şirket içi Active Directory'den eşitlemek için gereken AD Bağlayıcısı hesap oluşturduğunda, o doğru hesap PublicFolder nesneleri okumak için gerekli izni atamaz. Bu sorun, hızlı yükleme ve özel yükleme etkiler. Bu değişiklik sorunu giderir.

* Windows Server 2016'dan çalışan yöneticiler için doğru neden değil işlemek Azure AD Connect Sihirbazı'nı sorun giderme sayfa bir sorun düzeltilmiştir.

#### <a name="new-features-and-improvements"></a>Yeni özellikleri ve geliştirmeleri
* Sorun giderme sayfası Azure AD Connect Sihirbazı'nı kullanarak parola eşitleme sorunlarını giderirken, sorun giderme sayfası artık etki alanına özel durumu döndürür.

* Daha önce parola karma eşitlemesini etkinleştirmek denediyseniz, Azure AD Connect doğrulama olup AD Bağlayıcısı hesabı parola karma tablolarından eşitlemek için gerekli izinlere şirket içi AD. Şimdi, Azure AD Connect Sihirbazı doğrulayın ve AD Bağlayıcısı hesabın yeterli izni yoksa uyar.

### <a name="ad-fs-management"></a>AD FS Yönetimi
#### <a name="fixed-issue"></a>Sabit sorunu
* Kullanımı ile ilgili bir sorun sabit [msDS-ConsistencyGuid kaynak bağlantısı olarak](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-msds-consistencyguid-as-sourceanchor) özelliği. Bu sorun, yapılandırdığınız müşterileri etkiler *AD FS ile Federasyon* oturum açma kullanıcı yöntemi olarak. Çalıştırdığınızda *yapılandırma kaynak bağlantısı* sihirbazında, Azure AD Connect görevde anahtarları kullanarak * ms-DS-ConsistencyGuid kaynak İmmutableıd özniteliği olarak. Bu değişikliğin bir parçası olarak, Azure AD Connect İmmutableıd AD FS'de talep kuralları güncelleştirmek çalışır. Ancak, Azure AD Connect AD FS'yi yapılandırmak için gereken yönetici kimlik bilgilerine sahip olmadığından bu adım başarısız oldu. Bu düzeltme ile Azure AD Connect artık çalıştırdığınızda AD FS için yönetici kimlik bilgilerini girmenizi ister *yapılandırma kaynak bağlantısı* görev.



## <a name="116140"></a>1.1.614.0
Durum: 05 Eylül 2017

### <a name="azure-ad-connect"></a>Azure AD Connect

#### <a name="known-issues"></a>Bilinen sorunlar
* Hata ile Azure AD Connect yükseltmesi başarısız olmasına neden olan bir bilinen sorun "*Eşitleme Hizmeti'ni yükseltme yüklenemiyor*". Ayrıca, eşitleme hizmeti artık olay hatasıyla başlatmak için "*hizmeti, veritabanı sürümü yüklü ikili sürümü daha yeni olduğundan dolayı başlatılamadı*". Yükseltme yapan Yöneticisi Azure AD Connect tarafından kullanılan SQL Server Sistem Yöneticisi ayrıcalığına sahip değil sorun ortaya çıkar. Dbo izinleri yeterli değil.

* Azure AD Connect etkinleştirdiyseniz müşteriler etkileyen yükseltmesi ile ilgili bilinen bir sorun var. [sorunsuz çoklu oturum açma](active-directory-aadconnect-sso.md). Azure AD Connect yükseltildikten sonra özelliğin etkin kalmasını olsa bile özelliği Sihirbazı'nda devre dışı olarak görüntülenir. Bu sorunun gelecekte sağlanacak için bir düzeltme bırakın. Bu görünen sorunla ilgili bir kaygınız müşteriler el ile sihirbazda sorunsuz çoklu oturum açma etkinleştirerek düzeltebilirsiniz.

#### <a name="fixed-issues"></a>Giderilen sorunlar
* Azure AD Connect etkinleştirilirken bir şirket içi ADFS talep kuralları güncelleştirme engelleyen bir sorun sabit [msDS-ConsistencyGuid kaynak bağlantısı olarak](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-msds-consistencyguid-as-sourceanchor) özelliği. ADFS oturum açma yöntemi olarak yapılandırılmış olan mevcut bir Azure AD Connect dağıtım özelliğini etkinleştirmeyi deneyin, sorun oluşur. Sihirbaz ADFS kimlik bilgileri için ADFS talep kuralları güncelleştirmek denemeden önce sormaz sorun oluşur.
* Azure AD Connect, yükleme başarısız olmasına neden olan sorunu sabit şirket içi AD ormanına sahip NTLM devre dışı. Kerberos kimlik doğrulaması için gerekli güvenlik kapsamları oluştururken tam kimlik bilgilerini sağlayan değil Azure AD Connect Sihirbazı nedeniyle işlemidir. Bu, Kerberos kimlik doğrulamasının başarısız olmasına ve Azure AD Connect Sihirbazı NTLM kullanmaya geri döner neden olur.

### <a name="azure-ad-connect-sync"></a>Azure AD Connect Sync
#### <a name="fixed-issues"></a>Giderilen sorunlar
* Bir sorun etiketi öznitelik doldurulmuş değil, yeni eşitleme kuralı burada oluşturulamıyor sabit.
* Bağlanmak Azure AD Connect neden olan sorunu sabit AD NTLM kullanarak parola eşitlemesi için Kerberos kullanılabilir olsa bile şirket. Bu sorun oluşur şirket içi AD topolojisine sahip bir yedeklemeden geri yüklenen bir veya daha fazla etki alanı denetleyicileri.
* Tam eşitleme adımları yükseltmeden sonra gereksiz yere oluşmasına neden bir sorun düzeltilmiştir. Out-of-box eşitleme kuralları için değişiklik varsa genel olarak, tam eşitleme adımları çalıştıran yükseltmeden sonra gereklidir. Eşitleme kuralı ifadesi satırbaşı karakterlerini ile karşılaşıldığında yanlış bir değişiklik algılandı değişiklik algılama mantığı bir hata nedeniyle sorun oluştu. Satırbaşı karakterlerini okunabilirliğini artırmak için eşitleme kuralı ifadesine eklenir.
* Azure AD Connect sunucusu doğru otomatik yükseltme çalışmamasına neden olan sorunu sabit. Bu sorunu Azure AD Connect sunucuları 1.1.443.0 sürümüyle etkiler (veya öncesi). Sorun hakkında daha fazla ayrıntı için makalesine başvurun [Azure AD Connect otomatik yükseltme işleminden sonra düzgün çalışmıyor](https://support.microsoft.com/help/4038479/azure-ad-connect-is-not-working-correctly-after-an-automatic-upgrade).
* Her 5 dakikada bir zaman hatalarla denenmek üzere otomatik yükseltme neden olabilecek bir sorun düzeltilmiştir. Hatalarla karşılaştığında düzeltmeyle otomatik yükseltme üstel geri alma ile yeniden dener.
* Bir sorun nerede parola eşitleme olay 611 yanlış Windows uygulama olay günlükleri gösterilen sabit **bilgilendirme** yerine **hata**. Parola Eşitleme bir sorunla karşılaştığında olay 611 oluşturulur. 
* Grup geri yazma özelliği grup geri yazma için gereken bir OU seçmeden etkin olmasını sağlayan Azure AD Connect Sihirbazı'nda bir sorun düzeltilmiştir.

#### <a name="new-features-and-improvements"></a>Yeni özellikleri ve geliştirmeleri
* Sorun giderme Görev Azure AD Connect Sihirbazı Ek görevleri altında eklendi. Müşteriler için parola eşitlemeyi ilgili sorunları giderin ve genel tanılama toplamak için bu görev yararlanabilirsiniz. Gelecekte, sorun giderme Görev diğer dizin eşitleme ile ilgili sorunlar dahil etmek için genişletilir.
* Yeni bir yükleme modu adlı azure AD Connect şimdi destekler **varolan veritabanını kullan**. Bu yükleme modu müşterilerin var olan bir ADSync veritabanı belirtir Azure AD Connect yüklemesine olanak tanır. Bu özellik hakkında daha fazla bilgi için makalesine başvurun [varolan veritabanını kullan](active-directory-aadconnect-existing-database.md).
* Gelişmiş güvenlik için Azure AD Connect artık TLS1.2 dizin eşitleme için Azure AD bağlanmak için kullanılmasıdır. Daha önce varsayılan TLS1.0 oluştu.
* Azure AD Connect parola eşitleme Aracısı başladığında Azure AD parola eşitleme için iyi bilinen bitiş noktasına bağlanmaya çalışır. Başarılı bağlantı kurulduğunda bölgeye özgü uç noktasına yönlendirilir. Daha önce yeniden başlatılana kadar parola eşitleme Aracısı bölgeye özgü endpoint önbelleğe alır. Şimdi, bölgeye özgü uç noktası ile bağlantı sorunu karşılaşırsa Aracısı önbellek ve yeniden deneme iyi bilinen uç noktası ile temizler. Bu değişiklik, parola eşitleme erişebileceği farklı bir bölgeye özgü uç yük devretme önbelleğe alınmış bölgeye özgü uç noktası artık kullanılabilir olduğunda sağlar.
* Bir şirket içi değişiklikleri eşitlemek için AD ormanı, bir AD DS hesabı gereklidir. Oluşturabilir ya da (i) AD DS kendiniz hesap ve Azure AD Connect'e kendi kimlik bilgilerini girin veya (II) bir kuruluş yöneticisi kimlik bilgilerini ve AD DS hesabı oluşturduğunuzda Azure AD Connect olanak sağlar. Daha önce (i) Azure AD Connect Sihirbazı'nda varsayılan seçenektir. Şimdi, (II) varsayılan seçenek olan.

### <a name="azure-ad-connect-health"></a>Azure AD Connect Health

#### <a name="new-features-and-improvements"></a>Yeni özellikleri ve geliştirmeleri
* Microsoft Azure Bulutu ve Microsoft Cloud Almanya desteği eklendi.

### <a name="ad-fs-management"></a>AD FS Yönetimi
#### <a name="fixed-issues"></a>Giderilen sorunlar
* AD hazırlık powershell modülündeki Initialize-ADSyncNGCKeysWriteBack cmdlet'i yanlış cihaz kayıt kapsayıcısı ACL'ing oldu ve bu nedenle yalnızca varolan devralmak.  Eşitleme hizmeti hesabını doğru izinlere sahip olması güncelleştirildi.

#### <a name="new-features-and-improvements"></a>Yeni özellikleri ve geliştirmeleri
* AAD bağlanma doğrulayın ADFS oturum açma görev, Microsoft Online ve ADFS yalnızca belirteç alımı karşı oturumların doğrular şekilde güncelleştirilmiştir.
* Böylece kullanıcı ADFS ve WAP sunucularının sağlamaları istenir önce şimdi oluşur AAD Connect kullanarak yeni bir ADFS grubunun kurulumu sırasında ADFS kimlik bilgilerinin sorulmasına sayfa taşınmış olabilir.  Bu, belirtilen hesap doğru izinlere sahip olduğunu denetlemek AAD Connect sağlar.
* ADFS güveni AAD güncelleştiremediğinde AAD Connect yükseltme sırasında biz artık yükseltme başarısız olur.  Bu durumda, kullanıcı uygun bir uyarı iletisi gösterilir ve AAD Connect ek görev aracılığıyla güven sıfırlamaya devam etmelisiniz.

### <a name="seamless-single-sign-on"></a>Sorunsuz çoklu oturum açma
#### <a name="fixed-issues"></a>Giderilen sorunlar
* Etkinleştirmek denerseniz hata döndürmek Azure AD Connect Sihirbazı neden olan sorunu sabit [sorunsuz çoklu oturum açma](active-directory-aadconnect-sso.md). Hata iletisi *"Yapılandırması Microsoft Azure AD Connect kimlik doğrulama Aracısı başarısız oldu."* Bu sorunu el ile kimlik doğrulaması aracılar için önizleme sürümünü yükselten varolan müşterileri etkiler [doğrudan kimlik doğrulama](active-directory-aadconnect-sso.md) bu konuda açıklanan adımları temel [makale](active-directory-aadconnect-pass-through-authentication-upgrade-preview-authentication-agents.md).


## <a name="115610"></a>1.1.561.0
Durum: 23 Temmuz 2017

### <a name="azure-ad-connect"></a>Azure AD Connect

#### <a name="fixed-issue"></a>Sabit sorunu

* Out-of-box eşitleme kuralı "AD - kullanıcı İmmutableıd aşımına" neden olan sorunu sabit kaldırılacak:

  * Azure AD Connect yükseltildiğinde ya da sorun ortaya görev seçeneği *eşitleme yapılandırması güncelleştirme* içinde Azure AD Connect Sihirbazı Azure AD Connect eşitleme yapılandırmasını güncelleştirmek için kullanılır.
  
  * Bu eşitleme kuralının etkin müşteriler için geçerlidir [msDS-ConsistencyGuid kaynak bağlantısı özellik olarak](active-directory-aadconnect-design-concepts.md#using-msds-consistencyguid-as-sourceanchor). Bu özellik sürümünde 1.1.524.0 ve sonra sunulmuştur. Azure AD Connect eşitleme kuralı kaldırıldığında, artık şirket içi doldurabilirsiniz AD ms DS ConsistencyGuid özniteliğiyle objectGUID özniteliğinin değeri. Yeni kullanıcıların Azure AD ile sağlanan engellemez.
  
  * Özelliğin etkin olduğu sürece düzeltme eşitleme kuralı artık yükseltme sırasında ya da yapılandırma değişikliği sırasında kaldırılacak olmasını sağlar. Bu sorundan etkilenen mevcut müşteriler için Azure AD Connect bu sürüme yükseltirken, eşitleme kuralı eklediğiniz düzeltme de sağlar.

* 100'den az öncelik değerine sahip olacak şekilde out-of-box eşitleme kuralları neden olan bir sorunu sabit:

  * Genel olarak, öncelik değerler 0 - 99 özel eşitleme kuralları için ayrılmıştır. Yükseltme sırasında out-of-box eşitleme kuralları için öncelik değerleri eşitleme kuralı değişiklikleri uyum sağlayacak şekilde güncelleştirilir. Bu sorun nedeniyle, out-of-box eşitleme kuralları 100'den az öncelik değeri atanabilir.
  
  * Düzeltme sorunu yükseltme sırasında oluşmasını engeller. Ancak, bu öncelik değerleri sorundan etkilenen mevcut müşteriler geri yüklemez. Ayrı bir düzeltme gelecekte geri yükleme ile yardımcı olmak için sağlanır.

* Bir sorun sabit nerede [etki alanı ve OU filtreleme ekran](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) içinde Azure AD Connect Sihirbazı'nı gösteren *tüm etki alanları ve OU'lar eşitleme* seçeneği olarak seçili OU tabanlı filtreleme etkinleştirilmiş olsa bile.

*   Bir sorunu neden sabit [dizin bölümlerini Yapılandır ekran](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) hata ise döndürülecek Eşitleme Hizmeti Yöneticisi, *yenileme* düğmesine tıklandığında. Hata iletisi *"etki alanları yenilenirken bir hata oluştu: 'türü için System.Collections.ArrayList' türündeki nesne yayınlanamıyor ' Microsoft.DirectoryServices.MetadirectoryServices.UI.PropertySheetBase.MaPropertyPages.PartitionObject."* Yeni AD etki alanı olan bir AD ormanına eklendi ve Yenile düğmesini kullanarak Azure AD Connect'i güncelleştirmeye çalıştığınız hata oluşur.

#### <a name="new-features-and-improvements"></a>Yeni özellikleri ve geliştirmeleri

* [Otomatik yükseltme özelliği](active-directory-aadconnect-feature-automatic-upgrade.md) aşağıdaki yapılandırmalara sahip müşteriler desteklemek için genişletilen:
  * Cihaz geri yazma özelliğini etkinleştirdiniz.
  * Grup geri yazma özelliğini etkinleştirdiniz.
  * Yükleme, bir hızlı ayarları veya DirSync yükseltme değil.
  * Meta veri deposunda 100. 000'den fazla nesneler var.
  * Birden çok ormana bağlanırsınız. Hızlı Kurulum yalnızca bir ormana bağlanır.
  * AD Bağlayıcısı hesabı varsayılan MSOL_ hesabı artık değil.
  * Sunucu hazırlama modu olarak ayarlanmalıdır.
  * Kullanıcı geri yazma özelliğini etkinleştirdiniz.
  
  >[!NOTE]
  >Otomatik yükseltme özellik kapsamı genişlemesi sonra Azure AD Connect yapı 1.1.105.0 ile müşterileri etkiler. Otomatik olarak yükseltilmesi için Azure AD Connect sunucu istemiyorsanız, Azure AD Connect sunucunuzda aşağıdaki cmdlet'i çalıştırmanız gerekir: `Set-ADSyncAutoUpgrade -AutoUpgradeState disabled`. Etkinleştirme/otomatik yükseltme devre dışı bırakma hakkında daha fazla bilgi için makalesine başvurun [Azure AD Connect: Otomatik yükseltme](active-directory-aadconnect-feature-automatic-upgrade.md).

## <a name="115580"></a>1.1.558.0
Durum: yayımlanan değil. Bu yapı değişiklikleri sürüm 1.1.561.0 dahil edilir.

### <a name="azure-ad-connect"></a>Azure AD Connect

#### <a name="fixed-issue"></a>Sabit sorunu

* Out-of-box eşitleme kuralı "AD - kullanıcı İmmutableıd aşımına" neden olan sorunu sabit OU tabanlı filtreleme yapılandırma güncelleştirildiğinde kaldırılacak. Bu eşitleme kuralı için gerekli [msDS-ConsistencyGuid kaynak bağlantısı özellik olarak](active-directory-aadconnect-design-concepts.md#using-msds-consistencyguid-as-sourceanchor).

* Bir sorun sabit nerede [etki alanı ve OU filtreleme ekran](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) içinde Azure AD Connect Sihirbazı'nı gösteren *tüm etki alanları ve OU'lar eşitleme* seçeneği olarak seçili OU tabanlı filtreleme etkinleştirilmiş olsa bile.

*   Bir sorunu neden sabit [dizin bölümlerini Yapılandır ekran](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) hata ise döndürülecek Eşitleme Hizmeti Yöneticisi, *yenileme* düğmesine tıklandığında. Hata iletisi *"etki alanları yenilenirken bir hata oluştu: 'türü için System.Collections.ArrayList' türündeki nesne yayınlanamıyor ' Microsoft.DirectoryServices.MetadirectoryServices.UI.PropertySheetBase.MaPropertyPages.PartitionObject."* Yeni AD etki alanı olan bir AD ormanına eklendi ve Yenile düğmesini kullanarak Azure AD Connect'i güncelleştirmeye çalıştığınız hata oluşur.

#### <a name="new-features-and-improvements"></a>Yeni özellikleri ve geliştirmeleri

* [Otomatik yükseltme özelliği](active-directory-aadconnect-feature-automatic-upgrade.md) aşağıdaki yapılandırmalara sahip müşteriler desteklemek için genişletilen:
  * Cihaz geri yazma özelliğini etkinleştirdiniz.
  * Grup geri yazma özelliğini etkinleştirdiniz.
  * Yükleme, bir hızlı ayarları veya DirSync yükseltme değil.
  * Meta veri deposunda 100. 000'den fazla nesneler var.
  * Birden çok ormana bağlanırsınız. Hızlı Kurulum yalnızca bir ormana bağlanır.
  * AD Bağlayıcısı hesabı varsayılan MSOL_ hesabı artık değil.
  * Sunucu hazırlama modu olarak ayarlanmalıdır.
  * Kullanıcı geri yazma özelliğini etkinleştirdiniz.
  
  >[!NOTE]
  >Otomatik yükseltme özellik kapsamı genişlemesi sonra Azure AD Connect yapı 1.1.105.0 ile müşterileri etkiler. Otomatik olarak yükseltilmesi için Azure AD Connect sunucu istemiyorsanız, Azure AD Connect sunucunuzda aşağıdaki cmdlet'i çalıştırmanız gerekir: `Set-ADSyncAutoUpgrade -AutoUpgradeState disabled`. Etkinleştirme/otomatik yükseltme devre dışı bırakma hakkında daha fazla bilgi için makalesine başvurun [Azure AD Connect: Otomatik yükseltme](active-directory-aadconnect-feature-automatic-upgrade.md).

## <a name="115570"></a>1.1.557.0
Durum: Temmuz 2017

>[!NOTE]
>Bu yapı müşteriler için Azure AD Connect otomatik yükseltmesi özelliği aracılığıyla kullanılabilir değil.

### <a name="azure-ad-connect"></a>Azure AD Connect

#### <a name="fixed-issue"></a>Sabit sorunu
* Geçerli bir etki alanı hala olsa bile, değiştirilecek varolan hizmet bağlantı noktası nesnesi üzerinde yapılandırılmış bir doğrulanmış etki alanının neden Initialize-ADSyncDomainJoinedComputerSync cmdlet ile ilgili bir sorun düzeltilmiştir. Hizmet bağlantı noktası yapılandırmak için kullanılan birden fazla doğrulanan etki alanlarının Azure AD kiracınıza sahip olduğunda bu sorun oluşur.

#### <a name="new-features-and-improvements"></a>Yeni özellikleri ve geliştirmeleri
* Parola geri yazma için Microsoft Azure kamu Bulut ve Microsoft Cloud Almanya önizlemesi kullanıma sunulmuştur. Farklı hizmet örnekleri için Azure AD Connect desteği hakkında daha fazla bilgi için makalesine başvurun [Azure AD Connect: özel konular örnekleri](active-directory-aadconnect-instances.md).

* Initialize-ADSyncDomainJoinedComputerSync cmdlet'ine şimdi AzureADDomain adlı yeni bir isteğe bağlı parametre vardır. Bu parametre, hizmet bağlantı noktası yapılandırmak için kullanılacak etki alanını doğruladıysanız belirtmenize olanak sağlar.

### <a name="pass-through-authentication"></a>Doğrudan Kimlik Doğrulama

#### <a name="new-features-and-improvements"></a>Yeni özellikleri ve geliştirmeleri
* Doğrudan kimlik doğrulama değiştirildi için gerekli aracı adını *Microsoft Azure AD uygulama ara sunucusu Bağlayıcısı* için *Microsoft Azure AD Connect kimlik doğrulama Aracısı*.

* Doğrudan kimlik doğrulama artık etkinleştirme parola karması eşitlemesi varsayılan olarak etkinleştirir.


## <a name="115530"></a>1.1.553.0
Durum: Haziran 2017

> [!IMPORTANT]
> Bu yapı içinde sunulan şema ve eşitleme kuralı değişiklikler vardır. Azure AD Connect eşitleme hizmeti yükseltmeden sonra tam içeri aktarma ve tam eşitleme adımları tetikler. Değişiklikleri Ayrıntılar aşağıda açıklanmıştır. Geçici olarak tam içeri aktarma ve tam eşitleme adımları yükseltmeden sonra erteleme, makalesine başvurun [yükselttikten sonra tam eşitleme erteleme nasıl](active-directory-aadconnect-upgrade-previous-version.md#how-to-defer-full-synchronization-after-upgrade).
>
>

### <a name="azure-ad-connect-sync"></a>Azure AD Connect Sync

#### <a name="known-issue"></a>Bilinen bir sorun
* Kullanan müşteriler etkileyen bir sorun [OU tabanlı filtreleme](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) Azure AD Connect eşitleme ile. Ne zaman gezinmek için [etki alanı ve OU filtreleme sayfası](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) Azure AD Connect Sihirbazı'nda aşağıdaki davranış beklenir:
  * OU tabanlı filtreleme etkinleştirilirse **seçilen etki alanları ve OU'lar eşitleme** seçeneği işaretlidir.
  * Aksi takdirde, **tüm etki alanları ve OU'lar eşitleme** seçeneği işaretlidir.

Ortaya sorunu olan **tüm etki alanları ve OU'lar seçeneği eşitleme** Sihirbazı'nı çalıştırdığınızda, her zaman işaretlidir.  OU tabanlı filtreleme önceden yapılandırılmış olsa bile bu oluşur. AAD Connect yapılandırma değişiklikleri kaydetmeden önce emin **seçili etki alanı eşitleme ve OU'lar seçeneği belirlendiğinde** ve eşitlemek için gereken tüm OU'larda yeniden etkinleştirildiğini onaylayın. Aksi takdirde, OU tabanlı filtreleme devre dışı bırakılır.

#### <a name="fixed-issues"></a>Giderilen sorunlar

* Bir sorun sabit Azure AD şirket içi parola sıfırlama için yöneticinin parola geri yazma ile AD ayrıcalıklı kullanıcı hesabı. Azure AD Connect ayrıcalıklı hesabı parola sıfırlama izin verildiğinde sorun oluşur. Bu sorunu ele Yöneticisi hesabının sahibi söz konusu değilse bu sürümünde sağlayarak rasgele şirket içi parola sıfırlama için Azure AD yönetici değil, Azure AD Connect, AD kullanıcı hesabı ayrıcalıklı. Daha fazla bilgi için bkz [güvenlik önerisi 4033453](https://technet.microsoft.com/library/security/4033453).

* İlgili bir sorunu sabit [msDS-ConsistencyGuid kaynak bağlantısı olarak](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-msds-consistencyguid-as-sourceanchor) burada Azure AD Connect mu için geri yazma özelliğini şirket içi AD msDS-ConsistencyGuid özniteliği. Birden çok şirket içi olduğunda sorun ortaya Azure AD Connect'e eklenen AD ormanına ve *kullanıcı kimlikleri arasında birden çok dizin seçeneği mevcut* seçilir. Bu tür yapılandırma kullanıldığında, sonuç eşitleme kuralları meta veri sourceAnchorBinary özniteliğinde doldurmayın. SourceAnchorBinary öznitelik, msDS-ConsistencyGuid özniteliği için kaynak öznitelik olarak kullanılır. Sonuç olarak, ms-DSConsistencyGuid özniteliği için geri yazma gerçekleşmez. Sorunu düzeltmek için aşağıdaki eşitleme kuralları meta veri sourceAnchorBinary özniteliğinde her zaman doldurulur emin olmak için güncelleştirilmiştir:
  * İçinde AD'den - InetOrgPerson AccountEnabled.xml
  * İçinde AD'den - InetOrgPerson Common.xml
  * İçinde AD'den - kullanıcı AccountEnabled.xml
  * İçinde AD'den - kullanıcı Common.xml
  * İçinde AD - kullanıcı katılımı SOAInAAD.xml

* Daha önce olsa bile [msDS-ConsistencyGuid kaynak bağlantısı olarak](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-msds-consistencyguid-as-sourceanchor) özelliği etkin değil, "AD – kullanıcı İmmutableıd aşımına" eşitleme kuralı hala Azure AD Connect'e eklenir. Etkisi çalıştırmak ve geri yazma msDS-ConsistencyGuid özniteliğinin oluşmasına neden olmaz. Karışıklığı önlemek için mantığı özelliği etkinleştirilmişse, eşitleme kuralı yalnızca eklendiğinden emin olmak için eklenmiştir.

* Hata olayı 611 parola karması eşitlemesi başarısız olmasına neden olan sorunu sabit. Daha fazla etki alanı denetleyicileri kaldırılmış olan şirket içi AD veya bir sonra bu sorun oluşur. Eşitleme tanımlama bilgisi her parola eşitleme döngüsü sonunda verilen şirket içi AD çağırma kimliklerle Kaldırılan etki alanı denetleyicilerinin 0 (güncelleştirme sıra numarası) USN değeri içerir. Parola Eşitleme Yöneticisi, eşitleme tanımlama bilgisi içeren USN değeri 0 devam edemiyor ve hata olayı 611 başarısız olur. Sonraki eşitleme döngüsü sırasında Parola Eşitleme Yöneticisi 0 USN değeri içermiyor son kalıcı eşitleme tanımlama bilgisi yeniden kullanır. Bu, eşitlenmesi aynı parola değişikliğini neden olur. Bu düzeltmeyle, doğru eşitleme tanımlama bilgisi Parola Eşitleme Yöneticisi devam ettirir.

* Daha önce Otomatik yükseltme Set-ADSyncAutoUpgrade cmdlet'i kullanılarak devre dışı bırakılmış olsa bile denetlemek için işlemine devam otomatik yükseltme düzenli aralıklarla yükseltme ve disablement vermenizin indirilen yükleyiciyi dayanır. Bu düzeltmeyle otomatik yükseltme işlemi artık yükseltme için düzenli olarak denetler. Bu Azure AD Connect sürümü için yükseltme yükleyici kez yürütüldüğünde düzeltme otomatik olarak uygulanır.

#### <a name="new-features-and-improvements"></a>Yeni özellikleri ve geliştirmeleri

* Daha önce [msDS-ConsistencyGuid kaynak bağlantısı olarak](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-msds-consistencyguid-as-sourceanchor) özelliği yalnızca yeni dağıtımlar için kullanılabilir. Artık, var olan dağıtımlar için kullanılabilir. Daha ayrıntılı belirtmek gerekirse:
  * Özelliğe erişmek için Azure AD Connect Sihirbazı'nı başlatın ve seçin *güncelleştirme kaynak bağlantısı* seçeneği.
  * Bu seçenek yalnızca objectGUID sourceAnchor özniteliği olarak kullanarak var olan dağıtımlar için görünür durumdadır.
  * Seçeneği yapılandırırken, sihirbazın şirket içi Active Directory'de msDS-ConsistencyGuid öznitelik durumunu doğrular. Öznitelik dizinde herhangi bir kullanıcı nesnesindeki yapılandırılmazsa sihirbaz msDS-ConsistencyGuid sourceAnchor özniteliği olarak kullanır. Dizindeki bir veya daha fazla kullanıcı nesnesi öznitelik yapılandırdıysanız, sihirbazın öznitelik diğer uygulamalar tarafından kullanılıyor ve sourceAnchor özniteliği olarak uygun değil ve devam etmek için kaynak bağlantısı değişiklik izin vermez sonlanır. Öznitelik mevcut uygulamalar tarafından kullanılmadığından eminseniz, hatayı bastırmak hakkında bilgi için desteğe başvurun gerekir.

* Özel **userCertificate** cihaz nesneleri, Azure AD Connect artık öznitelikte arar için gerekli sertifikaları değerler için [Windows 10 deneyimi için Azure AD etki alanına katılmış aygıtlar bağlanma](https://docs.microsoft.com/azure/active-directory/active-directory-azureadjoin-devices-group-policy) ve Azure AD ile eşitliyorsanız önce kalan çıkış filtreleri. Bu davranışı etkinleştirmek için "Kullanıma için AAD – aygıt katılma SOAInAD" out-of-box eşitleme kuralı güncelleştirildi.

* Azure AD Connect şimdi destekler geri yazma Exchange Online **cloudPublicDelegates** özniteliğini şirket içi AD **publicDelegates** özniteliği. Bu, burada bir Exchange Online posta kutusu şirket içi Exchange posta kutusu kullanıcılarla SendOnBehalfTo hakkı verilebilir senaryo sağlar. Bu özellik, yeni bir out-of-box eşitleme kuralı "AD – kullanıcı Exchange karma PublicDelegates geri yazma için çıkış" desteklemek için eklendi. Exchange karma özelliği etkinleştirilmişse, bu eşitleme kuralı yalnızca Azure AD Connect'e eklenir.

*   Azure AD Connect şimdi destekler eşitleme **altRecipient** Azure AD'den özniteliği. Bu değişiklik desteklemek için aşağıdaki out-of-box eşitleme kuralları gerekli öznitelik akışı içerecek şekilde güncelleştirilmiştir:
  * İçinde AD'den – kullanıcı Exchange
  * Out AAD – kullanıcı ExchangeOnline
  
* **CloudSOAExchMailbox** meta veri özniteliği gösteren verili bir kullanıcı veya Exchange Online posta kutusu olup. Tanımına gibi donanım ve Konferans odası posta kutularını olarak ek Exchange Online RecipientDisplayTypes içerecek şekilde güncelleştirildi. Bu değişiklik etkinleştirmek için "İçinde gelen AAD – kullanıcı Exchange karma", out-of-box eşitleme kuralı altında bulunan cloudSOAExchMailbox öznitelik tanımı gelen güncelleştirilmiştir:

```
CBool(IIF(IsNullOrEmpty([cloudMSExchRecipientDisplayType]),NULL,BitAnd([cloudMSExchRecipientDisplayType],&amp;HFF) = 0))
```

… şu şekilde:

```
CBool(
  IIF(IsPresent([cloudMSExchRecipientDisplayType]),(
    IIF([cloudMSExchRecipientDisplayType]=0,True,(
      IIF([cloudMSExchRecipientDisplayType]=2,True,(
        IIF([cloudMSExchRecipientDisplayType]=7,True,(
          IIF([cloudMSExchRecipientDisplayType]=8,True,(
            IIF([cloudMSExchRecipientDisplayType]=10,True,(
              IIF([cloudMSExchRecipientDisplayType]=16,True,(
                IIF([cloudMSExchRecipientDisplayType]=17,True,(
                  IIF([cloudMSExchRecipientDisplayType]=18,True,(
                    IIF([cloudMSExchRecipientDisplayType]=1073741824,True,(
                       IF([cloudMSExchRecipientDisplayType]=1073741840,True,False)))))))))))))))))))),False))

```

* X509Certificate2 uyumlu işlevleri eşitleme userCertificate özniteliği sertifika değerleri işlemek için kural ifadeleri oluşturmak için aşağıdaki kümesini eklendi:

    ||||
    | --- | --- | --- |
    |CertSubject|CertIssuer|CertKeyAlgorithm|
    |CertSubjectNameDN|CertIssuerOid|CertNameInfo|
    |CertSubjectNameOid|CertIssuerDN|IsCert|
    |CertFriendlyName|Certthumbprınt|CertExtensionOids|
    |CertFormat|CertNotAfter|CertPublicKeyOid|
    |CertSerialNumber|CertNotBefore|CertPublicKeyParametersOid|
    |CertVersion|CertSignatureAlgorithmOid|Şunu seçin:|
    |CertKeyAlgorithmParams|CertHashString|Burada|
    |||Avantaj ile|

* Aşağıdaki şema değişiklikleri, sAMAccountName, domainNetBios ve Grup nesneleri için domainFQDN yanı sıra, kullanıcı nesnelerinin distinguishedName akış için özel eşitleme kuralları oluşturmak müşteriler izin vermek için sunulmuştur:

  * Aşağıdaki öznitelikler MV şemaya eklenmiştir:
    * Grup: AccountName
    * Grup: domainNetBios
    * Grup: domainFQDN
    * Kişi: distinguishedName

  * Aşağıdaki öznitelikler Azure AD Bağlayıcısı şemaya eklenmiştir:
    * Grup: OnPremisesSamAccountName
    * Grup: NetBiosName
    * Grup: DNSEtkiAlanıAdı
    * Kullanıcı: OnPremisesDistinguishedName

* ADSyncDomainJoinedComputerSync cmdlet betik şimdi AzureEnvironment adlı yeni bir isteğe bağlı parametre vardır. Parametresi, ilgili Azure Active Directory Kiracı içinde barındırılan hangi bölgede belirtmek için kullanılır. Geçerli değerler şunlardır:
  * AzureCloud (varsayılan)
  * AzureChinaCloud
  * AzureGermanyCloud
  * USGovernment
 
* Güncelleştirilmiş eşitleme kuralı kullanılacak Düzenleyicisi bağlantı türünün varsayılan değer olarak (sağlamak yerine) eşitleme kuralı oluşturma sırasında katılın.

### <a name="ad-fs-management"></a>AD FS Yönetimi

#### <a name="issues-fixed"></a>Giderilen sorunlar

* URL'leri kimlik doğrulaması kesinti karşı dayanıklılığı artırmak için Azure AD tarafından sunulan yeni WS-Federasyon uç noktaları ve şirket içi eklenecek AD FS bölümünde, bağlı olan taraf güven yapılandırması:
  * https://ests.Login.microsoftonline.com/Login.srf
  * https://stamp2.Login.microsoftonline.com/Login.srf
  * https://ccs.Login.microsoftonline.com/Login.srf
  * https://ccs-sdf.Login.microsoftonline.com/Login.srf
  
* AD FS için IssuerID yanlış talep değeri üretmek neden bir sorun düzeltilmiştir. Azure AD kiracısında birden çok doğrulanmış etki alanı vardır ve IssuerID talep oluşturmak için kullanılan userPrincipalName özniteliğinin etki alanı soneki en az ise sorun 3 düzeyleri derin (örneğin, johndoe@us.contoso.com). Talep kuralları tarafından kullanılan regex güncelleştirerek sorun çözüldüğünde.

#### <a name="new-features-and-improvements"></a>Yeni özellikleri ve geliştirmeleri
* Daha önce Azure AD Connect tarafından sağlanan ADFS sertifika yönetimi özelliği yalnızca Azure AD Connect aracılığıyla yönetilen ADFS grupları ile kullanılır. Şimdi, Azure AD Connect'i kullanarak yönetilmeyen ADFS grupları ile özelliğini kullanabilirsiniz.

## <a name="115240"></a>1.1.524.0
Yayımlanma tarihi: Mayıs 2017

> [!IMPORTANT]
> Bu yapı içinde sunulan şema ve eşitleme kuralı değişiklikler vardır. Azure AD Connect eşitleme hizmeti yükseltmeden sonra tam içeri aktarma ve tam eşitleme adımları tetikler. Değişiklikleri Ayrıntılar aşağıda açıklanmıştır.
>
>

**Giderilen sorunlar:**

Azure AD Connect Eşitleme

* Müşteri Set-ADSyncAutoUpgrade cmdlet'ini kullanarak özelliği devre dışı bıraktı olsa bile Azure AD Connect sunucusu üzerinde gerçekleşmesi otomatik yükseltme neden olan bir sorunu sabit. Bu düzeltmeyle sunucuda otomatik yükseltme işlemi hala yükseltme için düzenli olarak denetler, ancak otomatik yükseltme yapılandırma indirilen yükleyiciyi geliştirir.
* DirSync yerinde yükseltme sırasında Azure AD Connect Azure AD ile eşitlemek için Azure AD Bağlayıcısı tarafından kullanılacak bir Azure AD hizmet hesabı oluşturur. Hesap oluşturulduktan sonra Azure AD Connect hesabı kullanarak Azure AD ile kimliğini doğrular. Bazı durumlarda, sırayla DirSync yerinde yükseltme hatası ile başarısız olmasına neden olan kimlik doğrulama geçici sorunları nedeniyle başarısız *"yapılandırma AAD eşitleme görevini yürütme bir hata oluştu: AADSTS50034: Bu uygulamaya imzalamak için hesap olmalıdır xxx.onmicrosoft.com dizinine eklendi."* DirSync yükseltme esnekliğini artırmak için Azure AD Connect artık kimlik doğrulama adımı yeniden dener.
* Yapı 443 ile DirSync yerinde yükseltmesinin başarılı olması için neden olan bir sorun oluştu, ancak dizin eşitlenmesi için gereken çalıştırma profillerini oluşturulmaz. Mantığı düzeltme bu Azure AD Connect derlemede dahil edilir. Müşteri için bu yapı yükseltildiğinde, Azure AD Connect çalıştırma profillerini eksik algılar ve bunları oluşturur.
* Parola eşitleme işleminin olay kimliği 6900 ve hata ile başlatmak başarısız olmasına neden olan bir sorunu sabit *"aynı anahtara sahip bir öğe zaten eklenmiş"*. AD yapılandırma bölümü eklemek için OU filtreleme yapılandırması güncelleştirme yoksa bu sorun oluşur. Bu sorunu gidermek için parola eşitleme işlemi yalnızca AD etki alanı bölümlerini parola değişikliklerini eşitler. Yapılandırma bölümü gibi etki alanı dışı bölümleri atlanır.
* Hızlı yükleme sırasında bir şirket içi Azure AD Connect oluşturur ile iletişim kurmak için AD Bağlayıcısı tarafından kullanılacak AD DS hesabı şirket içi AD. Daha önce hesabı kullanıcı hesabı denetimi özniteliği PASSWD_NOTREQD bayrağı oluşturulur ve rasgele bir parola hesabında ayarlanır. Şimdi, parola hesabında ayarlandıktan sonra Azure AD Connect açıkça PASSWD_NOTREQD bayrağını kaldırır.
* Hata ile DirSync yükseltme başarısız olmasına neden olan bir sorunu sabit *"kilitlenme sql Server'da hangi bir uygulama kilidi oluştu"* mailNickname özniteliği bulunduğunda şirket içi AD şema için sınırlı değildir ancak AD kullanıcı nesne sınıfı.
* Cihaz geri yazma özelliği yöneticinin Azure AD Connect Sihirbazı'nı kullanarak Azure AD Connect eşitleme yapılandırması güncelleştirilirken otomatik olarak devre dışı bırakılmasına neden olan bir sorunu sabit. Bu, önkoşul denetimi mevcut cihaz geri yazma yapılandırmasında şirket içi AD ve Denetim başarısız yapan Sihirbazı tarafından kaynaklanır. Cihaz geri yazma zaten daha önce etkinleştirilmişse denetimi atlayacak şekilde açıklanmıştır.
* OU filtreleme yapılandırmak için ya da Azure AD Connect Sihirbazı'nı veya Eşitleme Hizmeti Yöneticisi'ni kullanabilirsiniz. OU filtreleme yapılandırmak için Azure AD Connect Sihirbazı'nı kullanırsanız daha önce oluşturduğunuz yeni OU'lar daha sonra için dizin eşitleme dahil edilir. Eklenecek yeni OU'lar istemiyorsanız, Eşitleme Hizmeti Yöneticisi'ni kullanarak OU filtreleme yapılandırmanız gerekir. Şimdi, Azure AD Connect Sihirbazı'nı kullanarak aynı davranışı elde edebilirsiniz.
* Saklı yordamlar yükleme yönetici şema altında yerine altında dbo schema oluşturulması için Azure AD Connect için gerekli neden olan bir sorunu sabit.
* AAD Connect sunucu olay günlüklerinde atlanacak Azure AD tarafından döndürülen Trackingıd özniteliği neden olan bir sorunu sabit. Azure AD Connect, Azure AD'den bir yeniden yönlendirme iletisi alır ve Azure AD Connect sağlanan uç noktasına bağlanamıyor ise sorun oluşur. Trackingıd sorun giderme sırasında hizmet tarafı günlükleriyle ilişkilendirmek için destek mühendisleri tarafından kullanılır.
* Azure AD Connect Azure AD'den LargeObject hata aldığında, Azure AD Connect EventID 6941 ve ileti sahip bir olay oluşturur *"Sağlanan nesne çok büyük. Bu nesnedeki öznitelik değerlerinin sayısı kırpma."* Aynı anda Azure AD Connect de EventID 6900 ve ileti yanıltıcı bir olay oluşturur. *"Microsoft.Online.Coexistence.ProvisionRetryException: Windows ile iletişim kurulamıyor Azure Active Directory Hizmeti."* Karışıklığı en aza indirmek için Azure AD Connect artık LargeObject hata alındığında, ikinci olay oluşturur.
* Eşitleme Service Manager'ın genel LDAP Bağlayıcısı için yapılandırma güncelleştirmeye çalışırken yanıt veremez duruma neden olan bir sorunu sabit.

**Yeni özellikler/iyileştirmeleri:**

Azure AD Connect Eşitleme
* Uygulanan eşitleme kuralı değişiklikler – aşağıdaki eşitleme kuralı:
  * Güncelleştirilmiş varsayılan eşitleme kural kümesi öznitelikleri değil dışarı aktarmak için **userCertificate** ve **userSMIMECertificate** öznitelikleri 15'ten fazla değerleri varsa.
  * AD öznitelikleri **EmployeeID** ve **msExchBypassModerationLink** şimdi varsayılan eşitleme kuralı kümesine eklenir.
  * AD özniteliği **fotoğraf** varsayılan eşitleme kuralı kümesinden kaldırılmıştır.
  * Eklenen **preferredDataLocation** meta veri deposu şeması ve AAD bağlayıcı şema. Azure AD içinde ya da öznitelikleri güncelleştirmek istediğiniz müşterileri Bunu yapmak için özel eşitleme kurallarını uygulayabilir. Öznitelik hakkında daha fazla bilgi için makale bölümüne bakın. [Azure AD Connect eşitleme: varsayılan yapılandırması - PreferredDataLocation etkinleştir eşitlenmesi değişiklik yapmak nasıl](active-directory-aadconnectsync-change-the-configuration.md#enable-synchronization-of-preferreddatalocation).
  * Eklenen **userType** meta veri deposu şeması ve AAD bağlayıcı şema. Azure AD içinde ya da öznitelikleri güncelleştirmek istediğiniz müşterileri Bunu yapmak için özel eşitleme kurallarını uygulayabilir.

* Azure AD Connect artık şirket içi kaynak bağlantısı özniteliği olarak ConsistencyGuid özniteliği kullanımını otomatik olarak etkinleştirir AD nesnelerini. Boş ise daha, Azure AD Connect ConsistencyGuid özniteliği objectGUID özniteliğinin değeri ile doldurur. Bu özellik yalnızca yeni dağıtımı için geçerlidir. Bu özellik hakkında daha fazla bilgi için makale bölümüne bakın. [Azure AD Connect: tasarım kavramları - sourceAnchor msDS-ConsistencyGuid kullanarak](active-directory-aadconnect-design-concepts.md#using-msds-consistencyguid-as-sourceanchor).
* Yeni cmdlet sorun giderme Invoke-ADSyncDiagnostics ilgili sorunlar parola karması eşitlemesi tanımlanmasına yardımcı olmak için eklendi. Cmdlet kullanma hakkında daha fazla bilgi için makalesine başvurun [Parola Eşitleme ile Azure AD Connect eşitleme sorunlarını giderme](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-troubleshoot-password-synchronization).
* Azure AD Connect eşitleme Mail-Enabled ortak klasör destekler nesnelerin artık Azure ad AD şirket içi. İsteğe bağlı özellikler altında Azure AD Connect Sihirbazı kullanarak özelliğini etkinleştirebilirsiniz. Bu özellik hakkında daha fazla bilgi için makalesine başvurun [şirket içi posta etkinleştirilmiş ortak klasörleri Office 365 dizin dayalı kenar engelleme desteği](https://blogs.technet.microsoft.com/exchange/2017/05/19/office-365-directory-based-edge-blocking-support-for-on-premises-mail-enabled-public-folders).
* Azure AD Connect gerektiren bir AD DS hesabı'ndan eşitlemek için şirket içi AD. Daha önce Azure AD Connect Express modunu kullanarak yüklediyseniz, bir kuruluş yöneticisi hesabı ve Azure AD Connect kimlik bilgileri gerekli AD DS hesabı oluşturacak sağlayabilir. Ancak, özel bir yükleme ve var olan bir dağıtıma ormanlar ekleme için AD DS hesabı yerine sağlayın gerekirdi. Şimdi, ayrıca özel bir yükleme sırasında bir Kurumsal Yönetici hesabının kimlik bilgilerini sağlayın ve Azure AD Connect gereken AD DS hesabı oluşturma izin seçeneğiniz vardır.
* Azure AD Connect artık SQL AOA destekler. Azure AD Connect yüklemeden önce SQL AOA etkinleştirmeniz gerekir. Yükleme sırasında Azure AD Connect veya sağlanan SQL örneği için SQL AOA etkin olup olmadığını algılar. SQL AOA etkinleştirilirse, daha fazla Azure AD Connect SQL AOA zaman uyumlu veya zaman uyumsuz çoğaltma kullanacak şekilde yapılandırılıp yapılandırılmadığını rakamlar. Kullanılabilirlik grubu dinleyicisi Kur ayarlarken RegisterAllProvidersIP özelliği 0 olarak ayarlayın önerilir. SQL Native Client MultiSubNetFailover özelliğinin kullanımını desteklemez ve Azure AD Connect, SQL Native Client SQL bağlanmak için şu anda kullandığı nedeni budur.
* Yerel veritabanı veritabanı olarak Azure AD Connect sunucunuz için kullandığınız ve 10 GB boyut sınırına ulaştı, eşitleme hizmeti artık başlatır. Daha önce eşitleme hizmetinin başlatılması için yeterli DB alan geri kazanmak için yerel veritabanı üzerinde SHRINKDATABASE işlemi gerçekleştirmek için gerekir. Sonrasında, daha fazla DB alan geri kazanmak için çalıştırma geçmişi silmek için Eşitleme Hizmeti Yöneticisi'ni kullanabilirsiniz. Şimdi, başlangıç ADSyncPurgeRunHistory cmdlet'i çalıştırmak temizleme geçmiş verileri LocalDB DB alanı geri kazanmak için kullanabilirsiniz. Ayrıca, bu cmdlet Çevrimdışı mod destekler (belirterek - offline parametresini) kullanılabileceği eşitleme hizmeti çalışmadığı zaman. Not: Çevrimdışı modda yalnızca eşitleme hizmeti çalışmıyor ve kullanılan veritabanı LocalDB ise kullanılabilir.
* Depolama alanı gerekli, Azure AD miktarını azaltmak için Bağlan şimdi eşitleme hata ayrıntıları LocalDB/SQL veritabanlarında depolamadan önce sıkıştırır. Azure AD Connect daha eski bir sürümünden bu sürüme yükseltirken, Azure AD Connect bir kerelik sıkıştırma mevcut eşitleme hata ayrıntıları gerçekleştirir.
* Daha önce OU filtreleme yapılandırması güncelleştirdikten sonra el ile düzgün dahil/dizin eşitlemesini dışlanan varolan nesneleri olduğundan emin olmak için tam içeri aktarma çalıştırmanız gerekir. Şimdi, Azure AD Connect tam içeri aktarma otomatik olarak tetikleyen bir sonraki eşitleme sırasında döngüsü. Daha fazla, tam alma yalnızca olması uygulanan güncelleştirmesinden etkilenen AD bağlayıcılar. Not: Bu geliştirme, yalnızca Azure AD Connect Sihirbazı kullanılarak yapılan güncelleştirmeler filtreleme OU için geçerlidir. Eşitleme Hizmeti Yöneticisi'ni kullanarak yapılan güncelleştirme filtreleme OU için geçerli değildir.
* Daha önce kullanıcıları, grupları, Grup tabanlı filtreleme destekler ve kişi yalnızca nesneleri. Şimdi, Grup tabanlı filtreleme ayrıca bilgisayar nesneleri destekler.
* Daha önce Azure AD Connect Eşitleme Zamanlayıcısı'nı devre dışı bırakmadan bağlayıcı alanı verilerini silebilirsiniz. Şimdi, Zamanlayıcı etkin olduğunu algılarsa, Eşitleme Hizmeti Yöneticisi'ni bağlayıcı alanı verilerin silinmesini engeller. Ayrıca, bir uyarı Bağlayıcısı alanı verilerini silinirse, olası veri kaybına müşterilere bildirmek için döndürülür.
* Daha önce Azure AD Connect Sihirbazı'nın düzgün çalışması PowerShell transcription devre dışı bırakmalısınız. Bu sorunu kısmen çözümlenir. Eşitleme yapılandırması yönetmek için Azure AD Connect Sihirbazı'nı kullanıyorsanız, PowerShell transcription etkinleştirebilirsiniz. ADFS yapılandırmasını yönetmek için Azure AD Connect Sihirbazı'nı kullanıyorsanız PowerShell transcription devre dışı bırakmanız gerekir.



## <a name="114860"></a>1.1.486.0
Yayımlanma tarihi: Nisan 2017

**Giderilen sorunlar:**
* Burada Azure AD Connect'i başarıyla yerelleştirilmiş Windows Server sürümünde yüklenmez sorun düzeltilmiştir.

## <a name="114840"></a>1.1.484.0
Yayımlanma tarihi: Nisan 2017

**Bilinen sorunlar:**

* Aşağıdaki koşulların tümü doğruysa, Azure AD Connect bu sürümü başarıyla yüklenmez:
   1. Ya da DirSync yerinde yükseltme veya yeni Azure AD Connect yüklemesi yapıyorsunuz.
   2. Burada "Yöneticiler" sunucusunda yerleşik yönetici grubunun adını değil Windows Server'ın yerelleştirilmiş bir sürümün kullanıyor.
   3. Varsayılan SQL Server 2012 Express yerine kendi tam SQL sağlayan Azure AD Connect ile yüklenen LocalDB kullanıyor.

**Giderilen sorunlar:**

Azure AD Connect Eşitleme
* Çalıştırma profili bu eşitleme adımı için bir veya daha fazla bağlayıcılar eksikse eşitleme Zamanlayıcı tüm eşitleme adım burada atlar bir sorun düzeltilmiştir. Örneğin, el ile çalıştırma profili için bir Delta alma oluşturmadan Eşitleme Hizmeti Yöneticisi'ni kullanarak bir bağlayıcı eklendi. Eşitleme Zamanlayıcı Delta içeri aktarma için diğer bağlayıcıları çalışmaya devam eder, bu düzeltme sağlar.
* Bir sorun olduğu eşitleme hizmeti hemen çalıştırma profili işlemeyi durdurur sabit olduğu zaman, çalışma adımlardan biri ile ilgili bir sorunu karşılaşır. Bu düzeltme, eşitleme hizmeti adım çalıştırılan atlar ve rest işlemeye devam sağlar. Örneğin, bir Delta alma (her AD etki alanı içi) ile birden çok çalışma adımları çalıştırma profili AD Bağlayıcısı için gerekir. Bunlardan biri ağ bağlantısı sorunları olsa bile eşitleme hizmeti Delta içeri aktarma diğer AD etki alanlarıyla çalıştırın.
* Otomatik yükseltme sırasında atlanacak Azure AD Bağlayıcısı güncelleştirme neden olan bir sorunu sabit.
* Yanlış server kurulumu sırasında bir etki alanı denetleyicisi olup olmadığını belirlemek Azure AD Connect neden olan bir sorunu sabit, hangi içinde Aç DirSync yükseltme başarısız olmasına neden olur.
* Her çalıştırma profili için Azure AD Bağlayıcısı oluşturmamayı DirSync yerinde yükseltme neden olan bir sorunu sabit.
* Bir sorun olduğu Eşitleme Hizmeti Yöneticisi kullanıcı arabirimi genel LDAP Bağlayıcısı yapılandırılmaya çalışılırken yanıt vermeyi sabit.

AD FS Yönetimi
* AD FS birincil düğüm başka bir sunucuya taşınırsa, Azure AD Connect Sihirbazı nerede başarısız bir sorun düzeltilmiştir.

Masaüstü SSO
* Bir sorun, burada oturum açma ekranında, parola eşitleme, oturum açma seçeneği yeni yüklemesi sırasında olarak seçerseniz, Masaüstü SSO özelliği etkinleştirmek izin vermez Azure AD Connect Sihirbazı'nda sabit.

**Yeni özellikler/iyileştirmeleri:**

Azure AD Connect Eşitleme
* Azure AD Connect eşitleme, artık sanal hizmet hesabı, yönetilen hizmet hesabı ve Grup yönetilen hizmet hesabı hizmet hesabı olarak kullanılmasını destekler. Bu, Azure AD Connect yalnızca yeni yükleme için geçerlidir. Azure AD Connect yüklerken:
    * Varsayılan olarak, Azure AD Connect Sihirbazı sanal bir hizmet hesabı oluşturur ve hizmet hesabı olarak kullanır.
    * Bir etki alanı denetleyicisinde yüklüyorsanız, Azure AD Connect burada bir etki alanı kullanıcı hesabı oluşturur ve bunun yerine, hizmet hesabı olarak kullanan önceki davranışı geri döner.
    * Aşağıdakilerden birini sağlayarak varsayılan davranışı geçersiz kılabilirsiniz:
      * Bir grup yönetilen hizmet hesabı
      * Yönetilen hizmet hesabı
      * Bir etki alanı kullanıcı hesabı
      * Yerel bir kullanıcı hesabı
* Daha önce Azure AD Connect içeren yeni bir yapı yükseltirseniz bağlayıcılar güncelleştirin veya eşitleme kuralı değişiklikleri, Azure AD Connect bir tam eşitleme döngüsü tetikler. Şimdi, Azure AD Connect seçmeli olarak tam alma adımı güncelleştirme bağlayıcıları için yalnızca ve yalnızca eşitleme kuralı değişikliklerle bağlayıcıları için tam eşitleme adımı tetikler.
* Daha önce dışarı aktarma silme eşiği yalnızca eşitleme Zamanlayıcı tetiklenen dışarı aktarmaları için geçerlidir. Şimdi, özelliği el ile Eşitleme Hizmeti Yöneticisi'ni kullanarak müşteri tarafından tetiklenen dışarı kapsayacak şekilde genişletilir.
* Azure AD kiracınıza veya parola eşitleme özelliği kiracınız için etkin olup olmadığını gösteren bir hizmet yapılandırması yok. Daha önce hizmet yapılandırması, etkin bir basamak sunucusunda sahip olduğunuzda ve Azure AD Connect tarafından yanlış yapılandırılması kolaydır. Azure AD Connect artık, hizmet yapılandırması, etkin ile tutarlı tutmaya çalışacağı yalnızca Azure AD Connect sunucusu.
* Sihirbaz şimdi algılar ve bir uyarı verir azure AD Connect AD AD geri dönüşüm kutusu etkin olmayan şirket içi.
* Toplu işlem içindeki nesneler birleşik boyutunun belirli eşiği aşarsa, daha önce Azure AD zaman aşımına ve başarısız dışarı aktarın. Şimdi, eşitleme hizmeti sorunla karşılaşılırsa nesneleri ayrı, daha küçük gruplar halinde yeniden göndermeyi deneyecek.
* Eşitleme hizmeti anahtar yönetimi uygulaması Windows Başlat menüsünden kaldırılmıştır. Şifreleme anahtarı yönetimi miiskmu.exe kullanarak komut satırı arabirimi aracılığıyla desteklenmeye devam eder. Şifreleme anahtarını yönetme hakkında daha fazla bilgi için makalesine başvurun [Azure AD Connect eşitleme şifreleme anahtarını bırakıp](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-change-serviceacct-pass#abandoning-the-azure-ad-connect-sync-encryption-key).
* Azure AD Connect eşitleme hizmeti hesabı parolasını değiştirirseniz, şifreleme anahtarını terk ve Azure AD Connect eşitleme hizmeti hesabının parolasını yeniden kadar daha önce eşitleme hizmeti mümkün başlangıç doğru olmaz. Şimdi, bu artık gerekli değildir.

Masaüstü SSO

* Azure AD Connect Sihirbazı artık doğrudan kimlik doğrulama ve Masaüstü SSO yapılandırırken ağ üzerinde açılacak 9090 bağlantı noktası gerektirir. Yalnızca bağlantı noktası 443 gereklidir. 

## <a name="114430"></a>1.1.443.0
Yayımlanma tarihi: Mart 2017

**Giderilen sorunlar:**

Azure AD Connect Eşitleme
* Azure AD Connect Sihirbazı Azure AD Bağlayıcısı görünen adını Azure AD kiracısı atanan ilk onmicrosoft.com etki alanı içermiyorsa başarısız olmasına neden olan bir sorun düzeltilmiştir.
* Azure AD Connect Sihirbazı eşitleme hizmeti hesabının parolasını kesme işareti, virgül ve boşluk gibi özel karakterler içeriyorsa, SQL veritabanı bağlantısı yaparken başarısız olmasına neden olan bir sorun düzeltilmiştir.
* "Dimage görüntüden daha farklı bir bağlantı var" hataya neden olan bir sorunu sabit bir şirket içi geçici olarak dışarıda bıraktığınız sonra hazırlama modu, içinde bir Azure AD Connect sunucusu üzerinde gerçekleşmesi için AD nesne eşitlenmesini ve ardından, yeniden eşitleme için dahil.
* "DN bulunan hayali nesnedir" hataya neden olan bir sorunu sabit bir şirket içi geçici olarak dışarıda bıraktığınız sonra hazırlama modu, içinde bir Azure AD Connect sunucusu üzerinde gerçekleşmesi için AD nesne eşitlenmesini ve ardından, yeniden eşitleme için dahil.

AD FS Yönetimi
* Burada Azure AD Connect Sihirbazı değil AD FS yapılandırmasını güncelleştirin ve alternatif oturum açma Kimliğini yapılandırıldıktan sonra sağ talep bağlı olan taraf güveni üzerinde ayarlanmış bir sorun düzeltilmiştir.
* Azure AD Connect Sihirbazı, hizmet hesapları yerine sAMAccountName biçimi userPrincipalName biçimi kullanılarak yapılandırılmış olan AD FS sunucuları doğru şekilde işleyemedi olduğu bir sorun düzeltilmiştir.

Doğrudan Kimlik Doğrulama
* Azure AD Connect Sihirbazı aracılığıyla kimlik doğrulaması geçirmek seçildiğinde ancak kendi bağlayıcı kaydı başarısız olursa başarısız olmasına neden olan bir sorun düzeltilmiştir.
* Hangi nedenler Masaüstü SSO özelliği etkinleştirilmişse, seçili oturum açma yöntemi üzerinde Doğrulamayı atla için Azure AD Connect Sihirbazı'nı denetler bir sorun düzeltilmiştir.

Parola Sıfırlama
* Azure AAD Connect sunucunun bağlantı bir güvenlik duvarı veya proxy tarafından sonlandırıldı, yeniden bağlanmayı denemek değil neden olabilecek bir sorun düzeltilmiştir.

**Yeni özellikler/iyileştirmeleri:**

Azure AD Connect Eşitleme
* Get-ADSyncScheduler cmdlet şimdi SyncCycleInProgress adlı yeni bir Boolean özelliği döndürür. Döndürülen değeri true ise, bir zamanlanmış eşitleme döngüsü sürmekte olduğu anlamına gelir.
* Azure AD Connect yükleme ve Kurulum günlüklerini depolamak için hedef klasör %localappdata%\AADConnect günlük dosyalarını erişilebilirlik geliştirmek için %programdata%\AADConnect için taşındı.

AD FS Yönetimi
* AD FS sunucu grubu SSL sertifikasını güncelleştirmek için destek eklendi.
* AD FS 2016 yönetmek için destek eklendi.
* Şimdi, AD FS yükleme sırasında mevcut gMSA (Grup yönetilen hizmet hesabı) belirtebilirsiniz.
* Azure AD bağlı olan taraf güveni imza karma algoritma olarak SHA-256 artık yapılandırabilirsiniz.

Parola Sıfırlama
* Ürün işlevi için daha katı güvenlik duvarı kuralları ile ortamlarda izin vermek için sunulan geliştirmeleri.
* Azure Service Bus güvenilirlik geliştirilmiş bağlantı.

## <a name="113800"></a>1.1.380.0
Yayımlanma tarihi: Aralık 2016

**Sabit sorunu:**

* Active Directory Federasyon Hizmetleri (AD FS) için issuerid talep kuralı bu derlemede eksik olduğu sorun düzeltilmiştir.

>[!NOTE]
>Bu yapı müşteriler için Azure AD Connect otomatik yükseltmesi özelliği aracılığıyla kullanılabilir değil.

## <a name="113710"></a>1.1.371.0
Yayımlanma tarihi: Aralık 2016

**Bilinen bir sorun:**

* Bu yapı içinde AD FS için issuerid talep kuralı eksik. Azure Active Directory (Azure AD) ile birden çok etki alanı ad'niz issuerid talep kuralı gereklidir. Şirket içi yönetmek için Azure AD Connect kullanıyorsanız, AD FS dağıtımı, bu yapıya yükseltme, AD FS yapılandırmasından varolan issuerid talep kuralı kaldırır. Sorunun geçici çözümü için yükleme/yükseltme sonrasında issuerid talep kuralı ekleyerek. Kural issuerid ekleme ile ilgili ayrıntılar talep için üzerinde bu makalesine başvurun [Azure AD ile Federasyon için çoklu etki alanı desteği](active-directory-aadconnect-multiple-domains.md).

**Sabit sorunu:**

* Giden bağlantı için bağlantı noktası 9090 açılmadı Azure AD Connect'i yükleme veya yükseltme başarısız olur.

>[!NOTE]
>Bu yapı müşteriler için Azure AD Connect otomatik yükseltmesi özelliği aracılığıyla kullanılabilir değil.

## <a name="113700"></a>1.1.370.0
Yayımlanma tarihi: Aralık 2016

**Bilinen sorunlar:**

* Bu yapı içinde AD FS için issuerid talep kuralı eksik. Azure AD ile birden çok etki alanı ad'niz issuerid talep kuralı gereklidir. Şirket içi yönetmek için Azure AD Connect kullanıyorsanız, AD FS dağıtımı, bu yapıya yükseltme, AD FS yapılandırmasından varolan issuerid talep kuralı kaldırır. Sorunun geçici çözümü için yükleme/yükseltme sonrasında issuerid talep kuralı ekleyerek. Kural issuerid ekleme ile ilgili ayrıntılar talep için üzerinde bu makalesine başvurun [Azure AD ile Federasyon için çoklu etki alanı desteği](active-directory-aadconnect-multiple-domains.md).
* Bağlantı noktası 9090 olması açmanız yüklemeyi tamamlamak üzere giden.

**Yeni Özellikler:**

* Doğrudan kimlik doğrulama (Önizleme).

>[!NOTE]
>Bu yapı müşteriler için Azure AD Connect otomatik yükseltmesi özelliği aracılığıyla kullanılabilir değil.

## <a name="113430"></a>1.1.343.0
Yayımlanma tarihi: Kasım 2016

**Bilinen bir sorun:**

* Bu yapı içinde AD FS için issuerid talep kuralı eksik. Azure AD ile birden çok etki alanı ad'niz issuerid talep kuralı gereklidir. Şirket içi yönetmek için Azure AD Connect kullanıyorsanız, AD FS dağıtımı, bu yapıya yükseltme, AD FS yapılandırmasından varolan issuerid talep kuralı kaldırır. Sorunun geçici çözümü için yükleme/yükseltme sonrasında issuerid talep kuralı ekleyerek. Kural issuerid ekleme ile ilgili ayrıntılar talep için üzerinde bu makalesine başvurun [Azure AD ile Federasyon için çoklu etki alanı desteği](active-directory-aadconnect-multiple-domains.md).

**Giderilen sorunlar:**

* Bazı durumlarda, kuruluşunuzun parola ilkesi tarafından belirtilen karmaşıklık düzeyi parolasını karşılayan bir yerel hizmet hesabı oluşturamadı çünkü Azure AD Connect yükleme başarısız olur.
* Bağlayıcı alanı içindeki bir nesneyi aynı anda kapsam dışı olduğunda burada birleştirme kuralları yeniden değerlendirilerek olmayan bir sorunu sabit bir birleştirme kuralı ve kapsam içinde hale başka için. Bu durum, birleştirme koşulları karşılıklı olarak birbirini dışlar iki veya daha fazla birleşim kurallar varsa meydana gelebilir.
* Bir sorun birleştirme kuralları içeren olandan daha düşük öncelik değerleri varsa birleştirme kuralları içeren değil, gelen eşitleme kurallarından (Azure AD), burada işlenmez sabit.

**İyileştirmeleri:**

* Windows Server 2016 standart veya yüksek Azure AD Connect'i yüklemek için destek eklendi.
* SQL Server 2016 için Azure AD Connect Uzak veritabanı olarak kullanmak için destek eklendi.

## <a name="112810"></a>1.1.281.0
Yayımlanma tarihi: Ağustos 2016

**Giderilen sorunlar:**

* Sonraki eşitleme döngüsü tamamlandıktan sonra aralığı eşitlemek için değişiklikleri kadar gerçekleşmesi değil.
* Azure AD Connect Sihirbazı, kullanıcı adı alt çizgi ile başlayan bir Azure AD hesabının kabul etmedi (\_).
* Azure AD Connect Sihirbazı hesap parolası çok fazla özel karakterler içeriyorsa, Azure AD hesabının kimlik doğrulaması başarısız. Hata iletisi "kimlik bilgilerini doğrulama yüklenemiyor. Beklenmeyen bir hata oluştu." döndürülür.
* Sunucu hazırlama kaldırma Azure AD kiracısında parola eşitleme devre dışı bırakır ve parola eşitleme ile active server başarısız olmasına neden olur.
* Parola Eşitleme, kullanıcıya depolanan hiçbir parola karması olduğunda seyrek durumlarda başarısız olur.
* Azure AD Connect sunucusu için hazırlama modu etkinleştirildiğinde, parola geri yazma geçici olarak devre dışı değil.
* Sunucu hazırlama modunda olduğunda azure AD Connect Sihirbazı gerçek parola eşitleme ve parola geri yazma yapılandırma göstermez. Bu her zaman bunları devre dışı olarak gösterilir.
* Sunucu hazırlama modunda olduğunda yapılandırma değişikliklerini parola eşitleme ve parola geri yazma özelliğini Azure AD Connect Sihirbazı tarafından kalıcı değildir.

**İyileştirmeleri:**

* Başarıyla veya yeni bir eşitleme döngüsü başlatmak mümkün olup olmadığını belirtmek için başlangıç-ADSyncSyncCycle cmdlet'i güncelleştirildi.
* Devam eden eşitleme döngüsü ve işlemini sonlandırmak için Stop-ADSyncSyncCycle cmdlet'i eklendi.
* Devam eden eşitleme döngüsü ve işlemini sonlandırmak için Stop-ADSyncScheduler cmdlet'i güncelleştirildi.
* Yapılandırırken [dizin uzantıları](active-directory-aadconnectsync-feature-directory-extensions.md) Azure AD Connect Sihirbazı'nda Azure AD öznitelik türü "Teletex dizesi" şimdi seçilebilir.

## <a name="111890"></a>1.1.189.0
Yayımlanma tarihi: Haziran 2016

**Giderilen sorunlar ve iyileştirmeleri:**

* Azure AD Connect artık FIPS ile uyumlu bir sunucuya yüklenebilir.
  * Parola eşitlemesi için bkz: [parola eşitleme ve FIPS](active-directory-aadconnectsync-implement-password-synchronization.md#password-synchronization-and-fips).
* Bir sorun olduğu bir NetBIOS adı için Active Directory Bağlayıcısı FQDN çözümlenemedi sabit.

## <a name="111800"></a>1.1.180.0
Yayımlanma tarihi: Mayıs 2016

**Yeni Özellikler:**

* Sizi uyarır ve, Azure AD Connect çalıştırmadan önce bunu siz etki alanları doğrulamanıza yardımcı olur.
* Desteği eklendi [Microsoft Cloud Almanya](active-directory-aadconnect-instances.md#microsoft-cloud-germany).
* En son desteği eklendi [Microsoft Azure kamu bulut](active-directory-aadconnect-instances.md#microsoft-azure-government-cloud) yeni URL gereksinimlerini altyapısıyla.

**Giderilen sorunlar ve iyileştirmeleri:**

* Eşitleme kuralları bulmayı kolaylaştırmak için eşitleme kuralı Düzenleyicisi filtreleme eklendi.
* Bağlayıcı alanı silerken performansı.
* Aynı nesne hem silinmiş ve aynı (çağrılan silme/add) çalıştırmak eklenen olduğunda bir sorun düzeltilmiştir.
* Devre dışı bırakılmış bir eşitleme kuralı artık nesneler dahil yeniden etkinleştirir ve yükseltme veya dizin şeması özniteliklerinde yenileyin.

## <a name="111300"></a>1.1.130.0
Yayımlanma tarihi: Nisan 2016

**Yeni Özellikler:**

* Birden çok değerli öznitelikler için destek eklenmiştir [dizin uzantıları](active-directory-aadconnectsync-feature-directory-extensions.md).
* Daha fazla yapılandırma farklılıkları için desteği eklendi [otomatik yükseltme](active-directory-aadconnect-feature-automatic-upgrade.md) yükseltme için uygun olarak kabul edilmesi için.
* Bazı cmdlet'ler için eklenen [özel Zamanlayıcı](active-directory-aadconnectsync-feature-scheduler.md#custom-scheduler).

## <a name="111190"></a>1.1.119.0
Yayımlanma tarihi: Mart 2016

**Giderilen sorunlar:**

* Yapılan emin parola eşitleme için Windows Server 2008 (R2 öncesi) hızlı yükleme kullanılamaz bu işletim sisteminde desteklenmiyor.
* Özel filtre yapılandırmasıyla dirsync'ten yükseltme, beklendiği gibi çalışmadı.
* Daha yeni bir sürüme yükseltirken yapılandırmada değişiklik yapılmadan, tam alma/eşitleme zamanlanmadı.

## <a name="111100"></a>1.1.110.0
Yayımlanma tarihi: Şubat 2016

**Giderilen sorunlar:**

* Yükleme varsayılan C:\Program Files klasöründe değilse, önceki sürümlerinden yükseltme çalışmaz.
* Yüklediğinizde ve temizleyin **eşitleme işlemini başlatmak** Yükleme Sihirbazı'nı sonunda, ikinci kez Yükleme Sihirbazı'nı çalıştıran Zamanlayıcı izin vermez.
* Zamanlayıcı ABD-İngilizcesi tarih/saat biçimi değil kullanıldığı sunucularda beklendiği gibi çalışmıyor. Ayrıca engeller `Get-ADSyncScheduler` zamanlarını doğru dönün.
* Yükseltme ve oturum açma seçeneği AD FS ile Azure AD Connect'in önceki bir sürümünü yüklediyseniz, Yükleme Sihirbazı'nı yeniden çalıştırılamıyor.

## <a name="111050"></a>1.1.105.0
Yayımlanma tarihi: Şubat 2016

**Yeni Özellikler:**

* [Otomatik yükseltmeyi](active-directory-aadconnect-feature-automatic-upgrade.md) özellik hızlı ayarları müşteriler için.
* Yükleme Sihirbazı'nda Azure çok faktörlü kimlik doğrulaması ve Privileged Identity Management kullanarak genel yönetici desteği.
  * Ayrıca çok faktörlü kimlik doğrulaması kullanırsanız https://secure.aadcdn.microsoftonline-p.com trafiğine izin verecek şekilde proxy izin vermeniz gerekir.
  * Multi-Factor Authentication'ın düzgün çalışması için Güvenilen siteler listesine https://secure.aadcdn.microsoftonline-p.com eklemeniz gerekir.
* İlk yüklemeden sonra kullanıcının oturum açma yöntemini değiştirme izin verin.
* İzin [etki alanı ve OU filtreleme](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) Yükleme Sihirbazı'nda. Bu, aynı zamanda tüm etki alanları kullanılabildiği ormanlara bağlanma sağlar.
* [Zamanlayıcı](active-directory-aadconnectsync-feature-scheduler.md) eşitleme altyapısı yerleşiktir.

**GA için Önizleme'den yükseltilen özellikleri:**

* [Cihaz geri yazma](active-directory-aadconnect-feature-device-writeback.md).
* [Dizin genişletmeleri](active-directory-aadconnectsync-feature-directory-extensions.md).

**Yeni Önizleme özellikleri:**

* Yeni varsayılan eşitleme döngüsü aralığı 30 dakikadır. Tüm önceki sürümler için üç saat olması için kullanılır. Değiştirmek için destek ekler [Zamanlayıcı](active-directory-aadconnectsync-feature-scheduler.md) davranışı.

**Giderilen sorunlar:**

* Doğrulama DNS etki alanları sayfasına her zaman etki alanı tanıması alamadık.
* AD FS yapılandırma sırasında etki alanı yönetici kimlik bilgilerini ister.
* Şirket içi AD hesapları tanınmıyor Yükleme Sihirbazı'nı farklı bir DNS ağaç kök etki alanı fazla olan bir etki alanında bulunan.

## <a name="1091310"></a>1.0.9131.0
Yayımlanma tarihi: Aralık 2015

**Giderilen sorunlar:**

* Bir parola ayarladığınızda, Active Directory etki alanı Hizmetleri (AD DS), ancak works parolalarda değiştirdiğinizde, parola eşitleme çalışmayabilir.
* Bir proxy sunucusu varsa, yükleme sırasında Azure ad kimlik doğrulama başarısız olabilir veya yükseltme yapılandırma sayfasında iptal edilir.
* Bir SQL Server Sistem Yöneticisi (SA) değilse, bir tam SQL Server örneği ile Azure AD Connect'in önceki bir sürümünden güncelleştirme başarısız olur.
* Uzak bir SQL Server ile Azure AD Connect'in önceki bir sürümünden güncelleştirme "Erişilemedi ADSync SQL veritabanı" hata gösterir.

## <a name="1091250"></a>1.0.9125.0
Yayımlanma tarihi: Kasım 2015

**Yeni Özellikler:**

* Azure AD güveni için AD FS yeniden yapılandırabilirsiniz.
* Active Directory şemasını yenileyin ve eşitleme kuralları yeniden kullanabilirsiniz.
* Bir eşitleme kuralının devre dışı bırakabilirsiniz.
* "AuthoritativeNull" bir eşitleme kuralında yeni bir hazır değer olarak tanımlayabilirsiniz.

**Yeni Önizleme özellikleri:**

* [Azure AD Connect Health eşitleme](../connect-health/active-directory-aadconnect-health-sync.md).
* Desteği [Azure AD etki alanı Hizmetleri](../active-directory-passwords-update-your-own-password.md) parola eşitleme.

**Yeni desteklenen senaryo:**

* Birden çok şirket içi Exchange kuruluşu destekler. Daha fazla bilgi için bkz: [birden çok Active Directory ormanına karma dağıtımlarında](https://technet.microsoft.com/library/jj873754.aspx).

**Giderilen sorunlar:**

* Parola eşitleme sorunlarını:
  * Çıkış, kapsamının dışında kapsam içinde taşınan bir nesne eşitlenen parolasını sahip olmaz. Bu, OU ve öznitelik filtreleme içerir.
  * Eşitleme dahil etmek için yeni bir OU seçerek tam parola eşitlemesini gerektirmez.
  * Devre dışı bir kullanıcı etkinleştirilmişse, parola eşitlemez.
  * Parola yeniden deneme kuyruğu sonsuzdur ve 5.000 devreden nesnelere önceki sınırını kaldırılmıştır.
* Active Directory orman işlev düzeyi Windows Server 2016 bağlanmak erişilemiyor.
* İlk yüklemeden sonra Grup filtreleme için kullanılan grubunu değiştirmek erişilemiyor.
* Artık parola geri yazma etkinleştirilmiş bir parola değişikliği yapan her kullanıcı için Azure AD Connect sunucusu üzerinde yeni bir kullanıcı profili oluşturur.
* Kullanmak için eşitleme kuralları kapsamları uzun tamsayı değerleri.
* Erişilemeyen etki alanı denetleyicileri varsa onay kutusu "cihaz geri yazma" devre dışı kalır.

## <a name="1086670"></a>1.0.8667.0
Yayımlanma tarihi: Ağustos 2015

**Yeni Özellikler:**

* Azure AD Connect Yükleme Sihirbazı'nı şimdi tüm Windows Server dillerine yerelleştirilmiş.
* Hesap için destek eklendi Azure AD parola yönetimi kullanırken kilidini açın.

**Giderilen sorunlar:**

* Başka bir kullanıcı ilk yükleme başlatan kişi yerine yükleme devam ederse, azure AD Connect Yükleme Sihirbazı'nı çöküyor.
* Azure AD Connect eşitleme düzgün bir şekilde kaldırmak, Azure AD Connect'in önceki kaldırma başarısız olursa yeniden mümkün değil.
* Azure AD Connect kullanıcı orman kök etki alanında değilse veya Active Directory İngilizce olmayan bir sürümü kullanılırsa, hızlı yükleme kullanarak yükleyemezsiniz.
* Active Directory kullanıcı hesabının FQDN'sini çözümlenemezse, yanıltıcı bir hata iletisi "şema kaydetmek için başarısız" gösterilir.
* Active Directory Bağlayıcısı üzerinde kullanılan hesabı Sihirbazı'nı dışında değiştirdiyseniz, sihirbazın sonraki çalışır başarısız olur.
* Azure AD Connect, bazen bir etki alanı denetleyicisine yüklemek başarısız olur.
* Oluşturulamıyor etkinleştirmek ve uzantı öznitelikleri eklediyseniz "Hazırlama modu" devre dışı bırakın.
* Parola geri yazma özelliğini bir Active Directory Bağlayıcısı hatalı parola nedeniyle bazı yapılandırmalarda başarısız olur.
* Ayırt edici ad (DN) öznitelik filtrelemesi kullanılıyorsa DirSync yükseltilemiyor.
* Parola sıfırlama kullanırken, aşırı CPU kullanımı.

**Kaldırılan Önizleme özellikleri:**

* Önizleme özelliğini [kullanıcı geri yazma](active-directory-aadconnect-feature-preview.md#user-writeback) geçici olarak Önizleme müşterilerimizden geri bildirim dayanan kaldırıldı. Biz sağlanan geri bildirim ele sonra daha sonra yeniden eklenir.

## <a name="1086410"></a>1.0.8641.0
Yayımlanma tarihi: Haziran 2015

**Azure AD Connect ilk sürümünü.**

Azure AD Connect değiştirilen adı Azure AD eşitleme'den.

**Yeni Özellikler:**

* [Hızlı Ayarlar](active-directory-aadconnect-get-started-express.md) yükleme
* Yapabilirsiniz [AD FS yapılandırma](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs)
* Yapabilirsiniz [dirsync'ten yükseltme](active-directory-aadconnect-dirsync-upgrade-get-started.md)
* [Yanlışlıkla silmeleri engelleme](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)
* Sunulan [hazırlama modu](active-directory-aadconnectsync-operations.md#staging-mode)

**Yeni Önizleme özellikleri:**

* [Kullanıcı geri yazma](active-directory-aadconnect-feature-preview.md#user-writeback)
* [Grup geri yazma](active-directory-aadconnect-feature-preview.md#group-writeback)
* [Cihaz geri yazma](active-directory-aadconnect-feature-device-writeback.md)
* [Dizin genişletmeleri](active-directory-aadconnect-feature-preview.md)

## <a name="104940501"></a>1.0.494.0501
Yayımlanma tarihi: Mayıs 2015

**Yeni gereksinimi:**

* Azure AD eşitleme şimdi yüklenmesi .NET Framework 4.5.1 sürümünü gerektirir.

**Giderilen sorunlar:**

* Parola geri yazma özelliğini Azure AD'den Azure Service Bus bağlantı hatasıyla başarısız oluyor.

## <a name="104910413"></a>1.0.491.0413
Yayımlanma tarihi: Nisan 2015

**Giderilen sorunlar ve iyileştirmeleri:**

* Active Directory Bağlayıcısı siler doğru geri dönüşüm kutusu etkinse ve ormanda birden çok etki alanı olan işlemez.
* Performans alma işlemlerinin Azure Active Directory Bağlayıcısı için geliştirilmiştir.
* Ne zaman bir grup aştı üyelik sınırı (varsayılan olarak, 50.000 nesnelere sınırı ayarlanır), Azure Active Directory'de Grup silindi. Yeni davranış Grup silinmez, bir hata oluşturulur ve yeni üyelik değişiklikleri aktarılmaz.
* Aşamalı bir silme aynı DN'si bağlayıcı alanı zaten varsa, yeni bir nesne sağlanamıyor.
* Bir nesne üzerinde hazırlanmış herhangi bir değişiklik olsa bile bazı nesneler delta eşitlemesi sırasında eşitleme için işaretlenir.
* Parola Eşitleme zorlama, tercih edilen DC listesi kaldırır.
* CSExportAnalyzer bazı nesneler durumları sorunları vardır.

**Yeni Özellikler:**

* Bir birleştirme şimdi "ANY" nesne türünün MV bağlanabilir.

## <a name="104850222"></a>1.0.485.0222
Yayımlanma tarihi: Şubat 2015

**İyileştirmeleri:**

* Geliştirilmiş alma performans.

**Giderilen sorunlar:**

* Parola Eşitleme, öznitelik filtrelemesi tarafından kullanılan cloudFiltered özniteliği geliştirir. Filtrelenmiş nesneler artık parola eşitleme kapsamında değildir.
* Burada topoloji birçok etki alanı denetleyicilerine sahip nadir durumlarda, parola eşitleme çalışmıyor.
* Azure AD/Intune'da "Durduruldu-server" Azure AD Bağlayıcısı'ndan sonra Aygıt Yönetimi alırken etkinleştirildi.
* Yabancı güvenlik sorumlusu (FSP) aynı ormandaki birden çok etki alanından birleştirme birleştirme belirsiz hataya neden olur.

## <a name="104751202"></a>1.0.475.1202
Yayımlanma tarihi: Aralık 2014

**Yeni Özellikler:**

* Parola Eşitleme'öznitelik tabanlı filtreleme ile artık desteklenmektedir. Daha fazla bilgi için bkz: [filtreleme ile parola eşitlemesini](active-directory-aadconnectsync-configure-filtering.md).
* MsDS-ExternalDirectoryObjectID özniteliği Active Directory'ye geri yazabilirsiniz. Bu özellik, Office 365 uygulamaları için destek ekler. OAuth2, karma bir Exchange dağıtım çevrimiçi ve şirket içi kutularına erişmek için kullanır.

**Sabit yükseltme sorunlar:**

* Sunucuda oturum açma Yardımcısı'nın daha yeni bir sürümü kullanılabilir.
* Özel bir yükleme yolu, Azure AD eşitleme yüklemek için kullanıldı.
* Geçersiz özel birleştirme ölçüt yükseltme engeller.

**Diğer düzeltmeleri:**

* Şablonları Office Pro Plus düzeltti.
* Bir tire ile başlayan kullanıcı adlarını nedeniyle oluşan sabit yükleme sorunları.
* Yükleme Sihirbazı'nı ikinci kez çalıştırırken sourceAnchor ayarı kaybetme sabit.
* Parola Eşitleme için sabit ETW İzleme.

## <a name="104701023"></a>1.0.470.1023
Yayımlanma tarihi: Ekim 2014

**Yeni Özellikler:**

* Parola Eşitleme birden çok Active Directory Azure AD ile şirket içi.
* Tüm Windows Server dillerine yerelleştirilmiş yükleme kullanıcı Arabirimi.

**Modu'nu 1.0 GA ' yükseltme**

Azure AD eşitleme zaten varsa, out-of-box eşitleme kurallardan herhangi birinin değişmiş durumda yapmanız gereken ek bir adım yoktur. 1.0.470.1023 yükselttikten sonra değiştirilmiş kuralları yineleniyor eşitleme bırakın. Her değiştirilmiş eşitleme kuralı için aşağıdakileri yapın:

1.  Eşitleme kuralı değiştirdiniz ve değişiklikleri bir not alın bulun.
* Eşitleme kuralını silin.
* Azure AD Sync tarafından oluşturulan yeni eşitleme kuralı bulun ve değişiklikleri yeniden uygulayın.

**Active Directory hesabı için izinler**

Active Directory hesabı parola karmalarını Active Directory'den okuyabilir için ek izinler verilmelidir. İzinleri vermek için "Dizin Değişikliklerini Çoğaltma" adlı ve "Tüm çoğaltma dizini değiştirir." Parola karmaları okuyabilir için her iki izinleri gereklidir.

## <a name="104190911"></a>1.0.419.0911
Yayımlanma tarihi: Eylül 2014

**Azure AD eşitleme ilk sürümünü.**

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md) hakkında daha fazla bilgi edinin.
