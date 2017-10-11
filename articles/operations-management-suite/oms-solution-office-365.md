---
title: "Office 365 çözümü Operations Management Suite (OMS) | Microsoft Docs"
description: "Bu makalede, yapılandırması ve OMS Office 365 çözümde kullanımı hakkında ayrıntılar sağlar.  Günlük analizi oluşturulan Office 365 kayıtları ayrıntılı açıklamasını içerir."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: bwren
ms.openlocfilehash: dcc44986acbb76eafc3cfacb79acf237802de021
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2017
---
# <a name="office-365-solution-in-operations-management-suite-oms"></a>Office 365 çözümü Operations Management Suite (OMS)

![Office 365 logosu](media/oms-solution-office-365/icon.png)

Office 365 çözümü Operations Management Suite (OMS) için günlük analizi, Office 365 ortamınızda izlemenize olanak tanır.  

- Office 365 hesaplarınızın yanı sıra kullanım desenlerini analiz davranış eğilimleri belirlemek için kullanıcı etkinliklerini izler. Örneğin, kuruluşunuzun veya en popüler SharePoint siteleri dışında paylaşılan dosyaları gibi belirli kullanım senaryoları ayıklayabilirsiniz.
- Yapılandırma değişikliklerini veya yüksek ayrıcalıklı işlemleri izlemek için yönetici etkinliklerini izler.
- Algılama ve kuruluş gereksinimlerinize göre özelleştirilmiş istenmeyen kullanıcı davranışı araştırın.
- Denetim ve uyumluluk gösterir. Örneğin, Denetim ve uyumluluk işlemiyle Yardım gizli dosyalar üzerinde dosya erişim işlemleri izleyebilirsiniz.
- OMS arama en üstünde, kuruluşunuzun Office 365 etkinlik verileri kullanarak işlemsel sorun giderme gerçekleştirir.

## <a name="prerequisites"></a>Ön koşullar
Aşağıdaki yüklenmiş ve yapılandırılmış bu çözümü olan önce gereklidir.

- Kuruluş Office 365 aboneliği.
- Genel yönetici olan bir kullanıcı hesabı için kimlik bilgileri.
- Denetim verileri almak için şunları yapmalısınız [denetimini yapılandırmak](https://support.office.com/en-us/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=en-US&rs=en-US&ad=US#PickTab=Before_you_begin) Office 365 aboneliğinizde.  Unutmayın [posta kutusu denetimi](https://technet.microsoft.com/library/dn879651.aspx) ayrı olarak yapılandırılır.  Hala çözümü yüklemek ve denetim yapılandırılmamışsa diğer verileri toplar.
 


## <a name="management-packs"></a>Yönetim paketleri
Bu çözüm, bağlı yönetim gruplarında herhangi bir Yönetim Paketi yüklemez.
  

## <a name="configuration"></a>Yapılandırma
Bir kez, [Office 365 çözümü, aboneliğinize eklediğiniz](../log-analytics/log-analytics-add-solutions.md), Office 365 aboneliğinize bağlanması gerekir.

1. Uyarı yönetimi çözümü açıklanan işlemi kullanarak OMS çalışma alanınıza ekleyin [çözümleri Ekle](../log-analytics/log-analytics-add-solutions.md).
2. Git **ayarları** OMS portalında.
3. Altında **bağlı kaynakları**seçin **Office 365**.
4. Tıklayın **Office 365 bağlanmak**.<br>![Bağlantı Office 365](media/oms-solution-office-365/configure.png)
5. Office 365 aboneliğiniz için bir genel yönetici olan bir hesapla oturum açın. 
6. Abonelik çözümü izleyeceğiniz iş yükleri ile listelenir.<br>![Bağlantı Office 365](media/oms-solution-office-365/connected.png) 


## <a name="data-collection"></a>Veri toplama
### <a name="supported-agents"></a>Desteklenen aracılar
Office 365 çözümü veri birinden almıyorsa [OMS Aracısı](../log-analytics/log-analytics-data-sources.md).  Office 365'ten doğrudan verileri alır.

### <a name="collection-frequency"></a>Toplama sıklığı
Office 365 gönderir bir [Web kancası bildirim](https://msdn.microsoft.com/office-365/office-365-management-activity-api-reference#receiving-notifications) ayrıntılı verilerle günlük analizi için her zaman bir kayıt oluşturulur.

## <a name="using-the-solution"></a>Çözümü kullanma
Office 365 çözümü, OMS çalışma alanına eklediğinizde **Office 365** döşeme OMS panonuz eklenir. Bu kutucukta, ortamınızdaki bilgisayarların sayısına ve güncelleştirme uyumluluğuna ilişkin bir sayı ve grafik gösterimi görüntülenir.<br><br>
![Office 365 Özet kutucuğu](media/oms-solution-office-365/tile.png)  

Tıklayın **Office 365** açmak için kutucuğa **Office 365** Pano.

![Office 365 Panosu](media/oms-solution-office-365/dashboard.png)  

Pano aşağıdaki tabloda gösterilen sütunları içerir. Her bir sütunun üst on uyarıları Belirtilen kapsam ve zaman aralığı için bu sütunun ölçütlerle eşleşen sayısına göre listeler. Bkz: tüm sütun altındaki tıklatarak veya sütun başlığını tıklatarak listenin tamamını sağlayan bir günlük arama çalıştırabilirsiniz.

| Sütun | Açıklama |
|:--|:--|
| İşlemler | İzlenen tüm Office 365 abonelik etkin kullanıcılar hakkında bilgi sağlar. Zaman içinde gerçekleşen etkinliklerin sayısını görmeye olacaktır.
| Exchange | Posta Kutusu Ekle izni veya Set-Mailbox gibi Exchange Server etkinlikleri dökümünü gösterir. |
| SharePoint | Kullanıcılar SharePoint belgelerindeki gerçekleştirmek üst etkinlikleri gösterir. Bu kutucuğunda detaya, arama sayfası hedef belge ve bu etkinliği konumunu gibi bu etkinlikler ayrıntılarını gösterir. Örneğin, dosyaya erişim bir olay için erişiliyor, belge görmeye siz olacaksınız ilişkili hesap adı ve IP adresi. |
| Azure Active Directory | Kullanıcı parola sıfırlama ve oturum açma denemesi gibi en çok kullanan kullanıcı etkinlikleri içerir. Ayrıntıya olduğunda, bu etkinlikler sonuç durumu gibi ayrıntılarını görmeye olacaktır. Bu genellikle, Azure Active Directory kuşkulu etkinlikleri izlemek istiyorsanız yararlıdır. |




## <a name="log-analytics-records"></a>Log Analytics kayıtları

Günlük analizi çalışma alanında Office 365 çözümü tarafından oluşturulan tüm kayıtlarına sahip bir **türü** , **OfficeActivity**.  **OfficeWorkload** özelliği, Exchange, AzureActiveDirectory, SharePoint veya OneDrive için - kayıt başvuruyor hangi Office 365 hizmet belirler.  **RecordType** özelliği işlemi türünü belirtir.  Özellikler her işlem türü için değişir ve aşağıdaki tablolarda gösterilmiştir.

### <a name="common-properties"></a>Ortak Özellikler
Aşağıdaki özellikleri için tüm Office 365 kayıtları yaygındır.

| Özellik | Açıklama |
|:--- |:--- |
| Tür | *OfficeActivity* |
| ClientIP | Etkinlik oturum açıldığında kullanılan aygıtın IP adresi. IP adresi, bir IPv4 veya IPv6 adresi biçiminde görüntülenir. |
| OfficeWorkload | Kayıt başvurduğu office 365 hizmeti.<br><br>AzureActiveDirectory<br>Exchange<br>SharePoint|
| İşlem | Kullanıcı veya yönetici etkinliği adı.  |
| OrganizationId | Kuruluşunuzun Office 365 Kiracı için GUID. Bu değer her zaman içinde oluştuğu Office 365 hizmeti bağımsız olarak, kuruluşunuz için aynı olacaktır. |
| RecordType | Gerçekleştirilen işlem türü. |
| ResultStatus | (İşlemi özelliğinde belirtilen) eylem başarılı olup olmadığını gösterir. Olası değerler, başarılı, PartiallySucceded veya başarısız olur. Exchange yönetici etkinliği için değer her iki true'dur ya da yanlış. |
| Kullanıcı Kimliği | (Kullanıcı asıl adı) günlüğe kaydedilmesini kaydında sonuçlandı eylemi gerçekleştiren kullanıcının UPN'sini; Örneğin, my_name@my_domain_name. Sistem hesapları (örneğin, SHAREPOINT\system veya ntauthority\system adlı) tarafından gerçekleştirilen etkinlik kayıtlarını da dahil olduğunu unutmayın. | 
| UserKey | UserId özelliğinde belirtilen kullanıcı için alternatif bir kimliği.  Örneğin, bu özellik, iş ve Exchange için SharePoint, OneDrive bulunan kullanıcılar tarafından gerçekleştirilen olaylar için passport benzersiz kimliği (PUID) ile doldurulur. Bu özellik, diğer hizmetleri ve sistem hesapları tarafından gerçekleştirilen etkinlikleri içinde gerçekleşen olayların UserId özelliği olarak aynı değeri de belirtebilir|
| UserType | İşlem gerçekleştirilirken kullanıcı türü.<br><br>Yönetici<br>Uygulama<br>DcAdmin<br>Normal<br>Ayrılmış<br>ServicePrincipal<br>Sistem |


### <a name="azure-active-directory-base"></a>Azure Active Directory temel
Aşağıdaki özellikleri için tüm Azure Active Directory kayıtları yaygındır.

| Özellik | Açıklama |
|:--- |:--- |
| OfficeWorkload | AzureActiveDirectory |
| RecordType     | AzureActiveDirectory |
| AzureActiveDirectory_EventType | Azure AD olay türü. |
| extendedProperties | Azure AD olay genişletilmiş özellikler. |


### <a name="azure-active-directory-account-logon"></a>Azure Active Directory hesabı oturum açma
Bir Active Directory kullanıcı oturum açma girişiminde Bu kayıtları oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| OfficeWorkload | AzureActiveDirectory |
| RecordType     | AzureActiveDirectoryAccountLogon |
| Uygulama | Office 15 gibi hesap oturum açma olayı tetikler uygulama. |
| İstemci | İstemci ayrıntılarını cihaz, cihaz işletim sistemine ve için kullanılan aygıt tarayıcı hesap oturum açma olay. |
| loginStatus | Bu özellik, doğrudan OrgIdLogon.LoginStatus olur. Çeşitli ilginç oturum açma hataları eşleme algoritmaları uyarı tarafından yapılabilir. |
| UserDomain | Kiracı kimlik bilgileri (TII). | 


### <a name="azure-active-directory"></a>Azure Active Directory
Azure Active Directory nesneleri değiştirme veya ekleme yapıldığında bu kayıtları oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| OfficeWorkload | AzureActiveDirectory |
| RecordType     | AzureActiveDirectory |
| AADTarget | (İşlemi özelliği tarafından tanımlanan) eylem gerçekleştirilip gerçekleştirilmediğine kullanıcı. |
| Aktör | Kullanıcı veya gerçekleştirilen eylem hizmet sorumlusu. |
| ActorContextId | Aktör ait kuruluş GUID. |
| ActorIpAddress | Aktör'ın IP adresi IPv4 veya IPv6 adresi biçiminde. |
| InterSystemsId | Office 365 hizmet içinde bileşenleri arasında eylemleri izlemek GUID. |
| IntraSystemId |   Azure eylemi izlemek için Active Directory tarafından üretilen GUID. |
| SupportTicketId | Müşteri, "act-üzerinde-adına-of" durumlarda eylemi için bilet kimliği destekler. |
| TargetContextId | Hedeflenen kullanıcının ait olduğu kuruluş GUID. |


### <a name="data-center-security"></a>Veri Merkezi güvenlik
Bu kayıtları veri merkezi güvenlik denetim verilerden oluşturulur.  

| Özellik | Açıklama |
|:--- |:--- |
| EffectiveOrganization | Yükseltme/cmdlet hedeflenmiş Kiracı adı. |
| ElevationApprovedTime | Yükseltme onaylandığı zaman damgası. |
| ElevationApprover | Bir Microsoft yöneticisinin adı. |
| ElevationDuration | Yükseltme etkin olduğu süre. |
| ElevationRequestId |  Yükseltme isteği için benzersiz bir tanımlayıcı. |
| ElevationRole | Yükseltme rol için istendi. |
| ElevationTime | Yükseltme başlangıç saati. |
| Start_Time | Cmdlet yürütmesini başlangıç saati. |


### <a name="exchange-admin"></a>Exchange yönetici
Exchange yapılandırması için değişiklik yapıldığında bu kayıtları oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| OfficeWorkload | Exchange |
| RecordType     | ExchangeAdmin |
| ExternalAccess |  Cmdlet, kuruluşunuz Microsoft datacenter personeli tarafından veya bir veri merkezi hizmet hesabı bir kullanıcı tarafından veya yönetici temsilcisi tarafından çalıştırılıp çalıştırılmadığını belirtir. Cmdlet, kuruluşunuzdaki bir kişi tarafından çalıştırıldığı False değerini gösterir. Cmdlet datacenter personel, bir veri merkezi hizmet hesabı veya yönetici temsilcisi tarafından çalıştırıldığı True değerini gösterir. |
| ModifiedObjectResolvedName |  Bu cmdlet tarafından değiştirilen nesne kullanıcı dostu adıdır. Bu cmdlet nesne değiştiriyorsa günlüğe kaydedilir. |
| Kuruluş adı | Kiracı adı. |
| OriginatingServer | Cmdlet yürütülen sunucunun adıdır. |
| Parametreler | Ad ve işlemleri özelliğinde tanımlanan cmdlet ile kullanılan tüm parametreler için değer. |


### <a name="exchange-mailbox"></a>Exchange posta kutusu
Exchange posta kutularına değişiklikler ve eklemeler yapıldığında bu kayıtları oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| OfficeWorkload | Exchange |
| RecordType     | ExchangeItem |
| ClientInfoString | Bir tarayıcı sürümü, Outlook sürümü ve mobil aygıt bilgileri gibi işlemi gerçekleştirmek için kullanılan e-posta istemcisi hakkında bilgi. |
| Client_IPAddress | İşlemi oturum açıldığında kullanılan aygıtın IP adresi. IP adresi, bir IPv4 veya IPv6 adresi biçiminde görüntülenir. |
| ClientMachineName | Outlook istemcisi barındıran makine adı. |
| ClientProcessName | Posta kutularına erişmek için kullanılan e-posta istemcisi. |
| ClientVersion | E-posta istemcisi sürümü. |
| InternalLogonType | İç kullanım için ayrılmıştır. |
| Logon_Type | Posta kutusu erişilen ve günlüğe işlemi gerçekleştiren kullanıcı türünü belirtir. |
| LogonUserDisplayName |    İşlemi gerçekleştiren kullanıcı kolay adı. |
| LogonUserSid | İşlemi gerçekleştiren kullanıcının SID'si. |
| MailboxGuid | Erişilen posta kutusunun Exchange GUID. |
| MailboxOwnerMasterAccountSid | Posta kutusu sahibi hesabının yönetici hesabı SID'si. |
| MailboxOwnerSid | Posta kutusu sahibi SID'si. |
| MailboxOwnerUPN | Erişilen posta kutusu sahibi olan kişinin e-posta adresi. |


### <a name="exchange-mailbox-audit"></a>Exchange posta kutusu denetimi
Bir posta kutusu denetim girdisi oluşturulduğunda bu kayıtları oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| OfficeWorkload | Exchange |
| RecordType     | ExchangeItem |
| Öğe | Bağlı işlem gerçekleştirildi öğeyi temsil eder | 
| SendAsUserMailboxGuid | E-posta olarak gönderilecek erişilmiş olan posta kutusunun Exchange GUID. |
| SendAsUserSmtp | Kimliğine bürünülen kullanıcı SMTP adresi. |
| SendonBehalfOfUserMailboxGuid | Adına posta göndermek için erişilen posta kutusunun Exchange GUID. |
| SendOnBehalfOfUserSmtp | SMTP adresi kullanıcının adına e-posta gönderilir. |


### <a name="exchange-mailbox-audit-group"></a>Exchange posta kutusu denetim grubu
Değişiklikler ve eklemeler Exchange gruplarına yapıldığında bu kayıtları oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| OfficeWorkload | Exchange |
| OfficeWorkload | ExchangeItemGroup |
| AffectedItems | Gruptaki her öğe hakkında bilgiler. |
| CrossMailboxOperations | İşlem birden fazla posta kutusu dahil olmadığını gösterir. |
| DestMailboxId | Yalnızca CrossMailboxOperations parametresi True ise ayarlayın. Hedef posta kutusu GUID belirtir. |
| DestMailboxOwnerMasterAccountSid | Yalnızca CrossMailboxOperations parametresi True ise ayarlayın. Hedef posta kutusu sahibi SID'si yönetici hesabı için SID belirtir. |
| DestMailboxOwnerSid | Yalnızca CrossMailboxOperations parametresi True ise ayarlayın. Hedef posta kutusu SID'si belirtir. |
| DestMailboxOwnerUPN | Yalnızca CrossMailboxOperations parametresi True ise ayarlayın. Hedef posta kutusu sahibinin UPN belirtir. |
| DestFolder | Taşıma gibi işlemleri için hedef klasör. |
| Klasör | Bir öğe grubunu bulunduğu klasör. |
| Klasörler |     Bir işlemde yer alan kaynak klasörleri hakkında bilgi; Örneğin, klasörleri seçtiyseniz ve ardından silinir. |


### <a name="sharepoint-base"></a>SharePoint tabanı
Bu özellikler, tüm SharePoint kayıtları yaygındır.

| Özellik | Açıklama |
|:--- |:--- |
| OfficeWorkload | SharePoint |
| OfficeWorkload | SharePoint |
| EventSource | Bir olay SharePoint'te oluştuğunu tanımlar. Olası değerler, SharePoint veya ObjectModel olur. |
| itemType | Erişilen veya değiştirilmiş nesnenin türü. Ayrıntılar için ItemType tabloya nesne türlerini bakın. |
| MachineDomainInfo | Aygıt eşitleme işlemleri hakkında bilgi. Yalnızca istek varsa, bu bilgileri bildirilir. |
| ResourceId |   Aygıt eşitleme işlemleri hakkında bilgi. Yalnızca istek varsa, bu bilgileri bildirilir. |
| Site_ | Dosya veya klasör kullanıcı tarafından erişilen bulunduğu site GUID. |
| Source_Name | Denetlenen işlemi tetiklenen varlık. Olası değerler, SharePoint veya ObjectModel olur. |
| UserAgent | Kullanıcının istemci veya tarayıcı ilgili bilgiler. Bu bilgiler istemci veya tarayıcı tarafından sağlanır. |


### <a name="sharepoint-schema"></a>SharePoint şeması
SharePoint için yapılandırma değişiklik yapıldığında bu kayıtları oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| OfficeWorkload | SharePoint |
| OfficeWorkload | SharePoint |
| CustomEvent | Özel olaylar için isteğe bağlı dize. |
| Event_Data |  Özel olaylar için isteğe bağlı yükü. |
| ModifiedProperties | Özelliği bir site veya bir site koleksiyonu yönetici grubunun bir üyesi olarak kullanıcı ekleme gibi yönetim olaylar için dahil edilmiştir. Özelliği (örneğin, Site Yönetici grubu), (bir site yöneticisi olarak eklenmiş gibi kullanıcının) değiştirilmiş özelliğin yeni değeri ve değiştirilmiş nesne önceki değeri değiştirildiği özelliğinin adını içerir. |


### <a name="sharepoint-file-operations"></a>SharePoint dosya işlemleri
Bu kayıtları yanıt SharePoint'teki dosya işlemleri olarak oluşturulur.

| Özellik | Açıklama |
|:--- |:--- |
| OfficeWorkload | SharePoint |
| OfficeWorkload | SharePointFileOperation |
| DestinationFileExtension | Kopyalanan veya taşınan bir dosyanın dosya uzantısını. Bu özellik yalnızca FileCopied ve FileMoved olaylar için görüntülenir. |
| DestinationFileName öğesinin | Kopyalanan veya taşınan dosyasının adı. Bu özellik yalnızca FileCopied ve FileMoved olaylar için görüntülenir. |
| DestinationRelativeUrl | Burada bir dosya kopyalanan taşınmış veya hedef klasör URL'si. SiteURL, DestinationRelativeURL ve destinationFileName öğesinin parametreleri ait değerlerin birleşimi kopyalanmıştır dosyasının tam yolu adı olduğundan objectID özelliğinin değeri ile aynıdır. Bu özellik yalnızca FileCopied ve FileMoved olaylar için görüntülenir. |
| SharingType | Paylaşım kaynağı ile paylaşıldı kullanıcıya atanmış izinleri türü. Bu kullanıcı UserSharedWith parametresi tarafından tanımlanır. |
| Site_Url | Dosya veya klasör kullanıcı tarafından erişilen bulunduğu site URL'si. |
| SourceFileExtension | Kullanıcı tarafından erişilen dosyanın dosya uzantısı. Bu özellik, erişilen nesne bir klasör ise boştur. |
| SourceFileName |  Dosya veya kullanıcı tarafından erişilen klasörün adı. |
| SourceRelativeUrl | Kullanıcı tarafından erişilen dosyanın bulunduğu klasörü URL'si. Birleşimi SiteURL, SourceRelativeURL ve SourceFileName parametreleri için değerleri, kullanıcı tarafından erişilen dosyanın tam yolunu adı objectID özelliğinin değeri ile aynıdır. |
| UserSharedWith |  Bir kaynak ile paylaşıldı kullanıcı. |




## <a name="sample-log-searches"></a>Örnek günlük aramaları
Aşağıdaki tabloda, bu çözüm tarafından toplanan güncelleştirme kayıtlarına ilişkin örnek günlük aramaları sunulmaktadır.

| Sorgu | Açıklama |
| --- | --- |
|Office 365 aboneliğinizin tüm işlemlerinin sayısı |' Türü OfficeActivity = | İşlem tarafından Count() ölçmek ' |
|SharePoint siteleri kullanımı|' Türü OfficeActivity OfficeWorkload = sharepoint = | Ölçü count() SiteUrl bazında sayı olarak | Count asc sıralama '|
|Kullanıcı türüne göre dosya erişim işlemleri|' Türü OfficeActivity OfficeWorkload = sharepoint işlemi = FileAccessed = | UserType tarafından Count() ölçmek '|
|Belirli bir anahtar sözcük ile arama|`Type=OfficeActivity OfficeWorkload=azureactivedirectory "MyTest"`|
|Exchange dış eylemlerini izleme|`Type=OfficeActivity OfficeWorkload=exchange ExternalAccess = true`|



## <a name="troubleshooting"></a>Sorun giderme

Office 365 çözümünüzü veri beklendiği gibi topluyor değil, OMS portalında durumunu denetleyin. **ayarları** -> **bağlı kaynakları** -> **Office 365**. Aşağıdaki tabloda her bir durumu açıklanmaktadır.

| Durum | Açıklama |
|:--|:--|
| Etkin | Office 365 aboneliğinizin etkin olduğunu ve iş yükü, OMS çalışma alanınızla başarıyla bağlandı. |
| Beklemede | Office 365 aboneliğinizin etkin olduğunu, ancak iş yükü henüz, OMS çalışma alanınızla başarıyla bağlı değil. Başarıyla bağlanıldı kadar Office 365 aboneliği bağlanmak ilk kez bu durum tüm iş yükleri olacaktır. Lütfen etkin geçiş yapmak tüm iş yükleri için 24 saat bekleyin. |
| Etkin olmayan | Office 365 abonelik etkin olmayan bir durumda. Ayrıntılar için Office 365 Yönetici sayfanızı denetleyin. Office 365 aboneliğinize etkinleştirdikten sonra OMS çalışma alanından bağlantısını ve veri alma yeniden başlatmak için bağlayabilirsiniz. |



## <a name="next-steps"></a>Sonraki adımlar
* Ayrıntılı güncelleştirme verilerini görüntülemek için [Log Analytics](../log-analytics/log-analytics-log-searches.md)’te Günlük Aramalarını kullanın.
* [Kendi panolar oluşturun](../log-analytics/log-analytics-dashboards.md) , sık kullanılan Office 365 arama sorgularını görüntülemek için.
* [Uyarı oluşturma](../log-analytics/log-analytics-alerts.md) önemli Office 365 etkinliklerini proaktif olarak bildirilmesi.  
