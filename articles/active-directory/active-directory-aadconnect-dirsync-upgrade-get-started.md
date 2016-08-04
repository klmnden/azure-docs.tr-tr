<properties
   pageTitle="Azure AD Connect: DirSync'ten yükseltme | Microsoft Azure"
   description="DirSync'ten Azure AD Connect'e nasıl yükseltme yapılacağı konusunda bilgi edinin. Bu makalede DirSync'ten Azure AD Connect'e yükseltmeye yönelik adımlar açıklanmaktadır."
   services="active-directory"
   documentationCenter=""
   authors="andkjell"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.date="05/31/2016"
   ms.author="andkjell;shoatman;billmath"/>

# Azure AD Connect: DirSync'ten yükseltme
Azure AD Connect, DirSync'in yerini almıştır. Bu konu başlığı altında DirSync'ten yükseltme yöntemlerini bulabilirsiniz. Bu adımlar, Azure AD Connect'in başka bir sürümünden veya Azure AD Eşitleme'den yapılacak yükseltmeler için geçerli değildir.

Azure AD Connect'i yüklemeye başlamadan önce [Azure AD Connect'i indirdiğinizden](http://go.microsoft.com/fwlink/?LinkId=615771) ve [Azure AD Connect: Donanım ve önkoşullar](active-directory-aadconnect-prerequisites.md) bölümündeki önkoşul adımlarını tamamladığınızdan emin olun.

DirSync'ten yükseltmiyorsanız diğer senaryolar için [ilgili belgelere](#related-documentation) göz atın.

## DirSync'ten yükseltme
Geçerli DirSync dağıtımınıza bağlı olarak farklı yükseltme seçenekleri mevcuttur. Tahmini yükseltme süresi 3 saatten azsa yerinde yükseltme yapılması önerilir. Tahmini yükseltme süresi 3 saatten fazlaysa başka bir sunucu üzerinde paralel dağıtım yapılması önerilir. 50.000'den fazla nesneniz varsa yükseltmenin 3 saatten fazla süreceği tahmin edilir.

Senaryo |  
---- | ----
[Yerinde yükseltme](#in-place-upgrade)  | Yükseltmenin 3 saatten az sürmesi beklendiğinde tercih edilen seçenek.
[Paralel dağıtım](#parallel-deployment) | Yükseltmenin 3 saatten fazla sürmesi beklendiğinde tercih edilen seçenek.

>[AZURE.NOTE] DirSync'ten Azure AD Connect'e yükseltmeyi planlıyorsanız yükseltmeden önce DirSync'i kendiniz kaldırmayın. Azure AD Connect, DirSync'ten yapılandırmayı okuyup geçişini yapar ve sunucuyu denetledikten sonra kaldırma işlemini gerçekleştirir.

**Yerinde yükseltme**  
Yükseltmenin tamamlanmasına ilişkin tahmini süre sihirbaz tarafından görüntülenir. Bu tahmin, yükseltme işleminin 50.000 nesne (kullanıcılar, kişiler ve gruplar) içeren bir veritabanı için 3 saat süreceği varsayımına dayanır. Azure AD Connect geçerli DirSync ayarlarınızı analiz eder ve veritabanınızdaki nesne sayısı 50.000'den az ise yerinde yükseltme seçeneğini önerir. Devam etmek isterseniz yükseltme sırasında geçerli ayarlarınız otomatik olarak uygulanır ve sunucunuz etkin eşitlemeyi otomatik olarak sürdürür.

Yapılandırma geçişi ve paralel dağıtım yapmak istiyorsanız yerinde yükseltme önerisini geçersiz kılabilirsiniz. Örneğin, donanım ve işletim sistemini yenileme olanağından faydalanabilirsiniz. Daha fazla bilgi için [paralel dağıtım](#parallel-deployment) bölümüne göz atın.

**Paralel dağıtım**  
50.000'den fazla nesneniz varsa paralel dağıtım seçeneğini kullanmanız önerilir. Bu, kullanıcılarınızın işletimsel gecikme yaşamasını önler. Azure AD Connect yüklemesi, yükseltme için kesinti süresini tahmin etmeye çalışır ancak DirSync'i daha önce yükselttiyseniz kendi deneyiminizin sizin için en iyi kılavuz olacağını söyleyebiliriz.

### Yükseltilecek olan desteklenen DirSync yapılandırmaları
Şu yapılandırma değişiklikleri DirSync ile desteklenmekte olup yükseltilecektir:

- Etki alanı ve OU filtreleme
- Alternatif kimlik (UPN)
- Parola eşitleme ve Exchange karma ayarları
- Ormanınız/etki alanınız ve Azure AD ayarlarınız
- Kullanıcı özniteliği tabanlı filtreleme

Aşağıdaki değişiklik yükseltilemez. Bu değişikliği yaptıysanız yükseltme engellenir:

- Desteklenmeyen DirSync değişiklikleri. Örneğin, kaldırılan öznitelikler ve özel uzantı DLL'si kullananlar

![Yükseltme engellendi](./media/active-directory-aadconnect-dirsync-upgrade-get-started/analysisblocked.png)

Bu gibi durumlarda, [hazırlama modunda](active-directory-aadconnectsync-operations.md#staging-mode) yeni bir Azure AD Connect sunucusunun yüklenmesi ve eski DirSync ile yeni Azure AD Connect yapılandırmasının doğrulanması önerilir. [Azure AD Connect Eşitleme özel yapılandırmasında](active-directory-aadconnectsync-whatis.md) tanımlanan şekilde, özel yapılandırmayı kullanarak yapılan tüm değişiklikleri yeniden uygulayın.

DirSync tarafından hizmet hesapları için kullanılan parolalar alınamaz ve geçirilmez. Bu parolalar yükseltme sırasında sıfırlanır.

### DirSync'ten Azure AD Connect'e yükseltme için üst düzey adımlar

1. Azure AD Connect'e Hoş Geldiniz
2. Geçerli DirSync yapılandırmasının analizi
3. Azure AD genel yönetici parolası toplama
4. Kuruluş yöneticisi hesabı için kimlik bilgileri toplama (yalnızca Azure AD Connect yüklemesi sırasında kullanılır)
5. Azure AD Connect yüklemesi
    * DirSync'i kaldırma
    * Azure AD Connect'i yükleme
    * Eşitlemeyi isteğe bağlı olarak başlatma

Ek adımların gerekli olduğu durumlar:

* Tam SQL Server (yerel veya uzak) kullanıyor olmanız
* Eşitleme kapsamında 50.000'den fazla nesnenizin olması

## Yerinde yükseltme

1. Azure AD Connect yükleyicisini (MSI) başlatın.
2. Lisans koşulları ve gizlilik bildirimini gözden geçirin ve kabul edin.
![Azure AD'ye Hoş Geldiniz](./media/active-directory-aadconnect-dirsync-upgrade-get-started/Welcome.png)
3. Mevcut DirSync yüklemenizin analizine başlamak için İleri'ye tıklayın.
![Mevcut Directory Sync yüklemesini analiz etme](./media/active-directory-aadconnect-dirsync-upgrade-get-started/Analyze.png)
4. Analiz tamamlandığında nasıl devam edeceğinize ilişkin öneriler göreceksiniz.  
    - SQL Server Express kullanıyorsanız ve 50.000'den az nesneniz varsa aşağıdaki ekran gösterilir: ![Analiz tamamlandı, DirSync'ten yükseltme yapmaya hazırsınız](./media/active-directory-aadconnect-dirsync-upgrade-get-started/AnalysisReady.png)
    - DirSync için tam SQL Server kullanıyorsanız o ekran yerine şu sayfayı görürsünüz: ![Analiz tamamlandı, DirSync'ten yükseltme yapmaya hazırsınız](./media/active-directory-aadconnect-dirsync-upgrade-get-started/AnalysisReadyFullSQL.png)  
DirSync tarafından kullanılan mevcut SQL Server veritabanı sunucusuyla ilgili bilgiler görüntülenir. Gerekirse uygun ayarlamaları yapın. Yüklemeye devam etmek için **İleri**'ye tıklayın.
    - 50.000'den fazla nesneniz varsa şu ekranı görürsünüz: ![Analiz tamamlandı, DirSync'ten yükseltme yapmaya hazırsınız](./media/active-directory-aadconnect-dirsync-upgrade-get-started/AnalysisRecommendParallel.png)  
Yerinde yükseltme işlemiyle devam etmek için şu iletinin yanındaki onay kutusuna tıklayın: **Bu bilgisayarda DirSync'i yükseltmeye devam et.**
Bunun yerine [paralel dağıtım](#parallel-deployment) yapmak için, DirSync yapılandırma ayarlarını dışarı aktarın ve yeni sunucuya taşıyın.
5. Şu anda Azure AD'ye bağlanmak için kullandığınız hesabın parolasını belirtin. Bu hesabın, şu anda DirSync tarafından kullanılan hesap olması gerekir.
![Azure AD kimlik bilgilerinizi girin](./media/active-directory-aadconnect-dirsync-upgrade-get-started/ConnectToAzureAD.png)  
Bir hatayla karşılaştıysanız ve bağlantı sorunlarınız varsa lütfen bkz. [Bağlantı sorunlarını giderme](active-directory-aadconnect-troubleshoot-connectivity.md).
6. Active Directory için bir kuruluş yöneticisi hesabı sağlayın.
![ADDS kimlik bilgilerinizi girin](./media/active-directory-aadconnect-dirsync-upgrade-get-started/ConnectToADDS.png)
7. Artık yapılandırma için hazırsınız. **Yükselt**'e tıkladığınızda DirSync kaldırılır, Azure AD Connect yapılandırılır ve eşitleme başlar.
![Yapılandırma için hazır](./media/active-directory-aadconnect-dirsync-upgrade-get-started/ReadyToConfigure.png)
8. Yükleme tamamlandıktan sonra Synchronization Service Manager'ı ve Synchronization Rule Editor'ı kullanmadan veya başka bir yapılandırma değişikliği yapmadan önce Windows oturumunuzu kapatıp tekrar açın.

## Paralel dağıtım

### DirSync yapılandırmasını dışarı aktarma
**50.000'den fazla nesneyle paralel dağıtım**

50.000'den fazla nesneniz varsa Azure AD Connect yüklemesi tarafından paralel dağıtım seçeneğini kullanmanız önerilir.

Şuna benzer bir ekran görüntülenir:

![Analiz tamamlandı](./media/active-directory-aadconnect-dirsync-upgrade-get-started/AnalysisRecommendParallel.png)

Paralel dağıtım ile devam etmek istiyorsanız şu adımları tamamlamanız gerekir:

- **Ayarları dışarı aktar** düğmesine tıklayın. Azure AD Connect'i ayrı bir sunucuya yüklediğinizde bu ayarlar, geçerli tüm DirSync ayarlarınızın yeni Azure AD Connect yüklemenize geçirilmesi için içeri aktarılır.

Ayarlarınız başarıyla dışarı aktarıldıktan sonra DirSync sunucusundaki Azure AD Connect sihirbazından çıkabilirsiniz. [Azure AD Connect'i ayrı bir sunucuya yüklemek](#installation-of-azure-ad-connect-on-separate-server) için bir sonraki adımla devam edin

**50.000'den az nesneyle paralel dağıtım**

50.000'den az nesneniz varsa ancak yine de paralel dağıtım yapmak istiyorsanız şunları yapın:

1. Azure AD Connect yükleyicisini (MSI) çalıştırın.
2. **Azure AD Connect'e Hoş Geldiniz** ekranını gördüğünüzde, pencerenin sağ üst köşesindeki "X" işaretine tıklayarak yükleme sihirbazından çıkın.
3. Bir komut istemi açın.
4. Azure AD Connect yükleme konumundan (Varsayılan: C:\Program Files\Microsoft Azure Active Directory Connect) şu komutu yürütün:  `AzureADConnect.exe /ForceExport`.
5. **Ayarları dışarı aktar** düğmesine tıklayın.  Azure AD Connect'i ayrı bir sunucuya yüklediğinizde bu ayarlar, geçerli tüm DirSync ayarlarınızın yeni Azure AD Connect yüklemenize geçirilmesi için içeri aktarılır.

![Analiz tamamlandı](./media/active-directory-aadconnect-dirsync-upgrade-get-started/forceexport.png)

Ayarlarınız başarıyla dışarı aktarıldıktan sonra DirSync sunucusundaki Azure AD Connect sihirbazından çıkabilirsiniz. [Azure AD Connect'i ayrı bir sunucuya yüklemek](#installation-of-azure-ad-connect-on-separate-server) için bir sonraki adımla devam edin

### Azure AD Connect'i ayrı bir sunucuya yükleme

Azure AD Connect'i yeni bir sunucuya yüklediğinizde, Azure AD Connect temiz bir yükleme gerçekleştirmek istediğinizi varsayar. DirSync yapılandırmasını kullanmak istiyorsanız tamamlamanız gereken bazı ek adımlar vardır:

1. Azure AD Connect yükleyicisini (MSI) çalıştırın.
2. **Azure AD Connect'e Hoş Geldiniz** ekranını gördüğünüzde, pencerenin sağ üst köşesindeki "X" işaretine tıklayarak yükleme sihirbazından çıkın.
3. Bir komut istemi açın.
4. Azure AD Connect yükleme konumundan (Varsayılan: C:\Program Files\Microsoft Azure Active Directory Connect) şu komutu yürütün:  `AzureADConnect.exe /migrate`.
    Azure AD Connect yükleme sihirbazı başlar ve şu ekranla karşılaşırsınız: ![Azure AD kimlik bilgilerinizi girin](./media/active-directory-aadconnect-dirsync-upgrade-get-started/ImportSettings.png)
5. DirSync yüklemesinden dışarı aktarılan ayarlar dosyasını seçin.
6. Şunlar dahil olmak üzere tüm gelişmiş seçenekleri yapılandırın:
    - Azure AD Connect için özel bir yükleme konumu.
    - Mevcut bir SQL Server örneği (Varsayılan: Azure AD Connect, SQL Server 2012 Express'i yükler). DirSync sunucunuzla aynı veritabanını kullanmayın.
    - SQL Server'a bağlanmak için kullanılan hizmet hesabı. (SQL Server veritabanınız uzak ise bu hesabın etki alanı hizmet hesabı olması gerekir.)
Bu ekranda şu seçenekleri görebilirsiniz: ![Azure AD kimlik bilgilerinizi girin](./media/active-directory-aadconnect-dirsync-upgrade-get-started/advancedsettings.png)
7. **İleri**'ye tıklayın.
8. **Yapılandırma için hazır** sayfasında, **Yapılandırma tamamlanınca eşitlemeyi başlat** seçeneğini işaretli olarak bırakın. Sunucu, [hazırlama modunda](active-directory-aadconnectsync-operations.md#staging-mode) olacağından değişiklikler bu sırada Azure AD'ye aktarılmaz.
9. **Yükle**'ye tıklayın.
10. Yükleme tamamlandıktan sonra Synchronization Service Manager'ı ve Synchronization Rule Editor'ı kullanmadan veya başka bir yapılandırma değişikliği yapmadan önce Windows oturumunuzu kapatıp tekrar açın.

>[AZURE.NOTE] Windows Server Active Directory ile Azure Active Directory arasında eşitleme başlar ancak Azure AD'ye hiçbir değişiklik aktarılmaz.  Aynı anda yalnızca bir eşitleme aracı değişiklikleri etkin olarak dışarı aktarabilir. Bu [hazırlama modu](active-directory-aadconnectsync-operations.md#staging-mode) olarak adlandırılır.

### Azure AD Connect'in eşitlemeye başlamak için hazır olduğunu doğrulama

Azure AD Connect'in DirSync'ten devralma işlemi hazır olduğunu doğrulamak üzere başlat menüsünden **Azure AD Connect** grubundaki **Synchronization Service Manager**'ı açmanız gerekir.

Uygulama içinde **İşlemler** sekmesini görüntülemeniz gerekir. Bu sekmede şu işlemlerin tamamlandığını doğrulamanız gerekir:

- AD Bağlayıcısı üzerinde içeri aktarma
- Azure AD Bağlayıcısı üzerinde içeri aktarma
- AD Bağlayıcısı üzerinde tam eşitleme
- Azure AD Bağlayıcısı üzerinde tam eşitleme

![İçeri aktarma ve Eşitleme tamamlandı](./media/active-directory-aadconnect-dirsync-upgrade-get-started/importsynccompleted.png)

Bu işlemlerin sonucunu gözden geçirin ve herhangi bir hata olmadığından emin olun.

Azure AD'ye aktarılacak olan değişiklikleri görmek ve incelemek isterseniz [hazırlama modu](active-directory-aadconnectsync-operations.md#staging-mode) bölümüne giderek yapılandırmanın nasıl doğrulanacağı hakkında bilgi edinin. Beklenmeyen bir durumla karşılaşmadığınız sürece gerekli yapılandırma değişikliklerini yapın.

Bu 4 işlem tamamlandığında, herhangi bir hata yoksa ve dışarı aktarılacak olan değişikliklerden memnunsanız DirSync'i kaldırıp Azure AD Connect eşitlemesini etkinleştirmeye hazırsınız demektir. Geçişi tamamlamak için sonraki iki adımı tamamlayın.

### DirSync'i (eski sunucuyu) kaldırma

- **Programlar ve özellikler**'e giderek **Microsoft Azure Active Directory eşitleme aracını** bulun
- **Microsoft Azure Active Directory eşitleme aracını** kaldırma
- Kaldırma işleminin 15 dakika sürebileceğini unutmayın.

DirSync kaldırıldığında, Azure AD'ye aktarım gerçekleştiren hiç etkin sunucu kalmaz. Şirket içi Active Directory'nizdeki değişikliklerin Azure AD ile eşitlenmeye devam edebilmesi için sonraki adımın tamamlanması gerekir.

### Azure AD Connect'i (yeni sunucu) etkinleştirme
Yükleme sonrasında, Azure AD Connect'i yeniden açtığınızda başka yapılandırma değişiklikleri gerçekleştirebilirsiniz. **Azure AD Connect**'i başlat menüsünden veya masaüstündeki kısayoldan başlatın. MSI yüklemesini tekrar çalıştırmayı denemeyin.

Şunları görmeniz gerekir:

![Ek görevler](./media/active-directory-aadconnect-dirsync-upgrade-get-started/AdditionalTasks.png)

- **Hazırlama modunu yapılandır** seçeneğini belirleyin.
- **Hazırlama modu etkin** onay kutusunun işaretini kaldırarak hazırlamayı devre dışı bırakın.

![Azure AD kimlik bilgilerinizi girin](./media/active-directory-aadconnect-dirsync-upgrade-get-started/configurestaging.png)

- **İleri** düğmesine tıklayın
- Onay sayfasındaki **Yükle** düğmesine tıklayın.

Azure AD Connect artık etkin sunucunuzdur.

## Sonraki adımlar
Azure AD Connect'i yüklediniz, artık [yüklemeyi doğrulayabilir ve lisans atayabilirsiniz](active-directory-aadconnect-whats-next.md).

Yüklemeyle etkinleştirilen şu yeni özellikler hakkında daha fazla bilgi edinin: [Otomatik yükseltme](active-directory-aadconnect-feature-automatic-upgrade.md), [Yanlışlıkla silmeleri engelleme](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) ve [Azure AD Connect Health](active-directory-aadconnect-health-sync.md).

Şu genel konu başlıkları hakkında daha fazla bilgi edinin: [Zamanlayıcı ve eşitleme tetikleme](active-directory-aadconnectsync-feature-scheduler.md).

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md) hakkında daha fazla bilgi edinin.

## İlgili belgeler

Konu başlığı |  
--------- | ---------
Azure AD Connect'e genel bakış | [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)
Önceki Connect sürümünden yükseltme | [Connect'in önceki sürümünden yükseltme](active-directory-aadconnect-upgrade-previous-version.md)
Hızlı ayarları kullanarak yükleme | [Azure AD Connect'i hızlı yükleme](active-directory-aadconnect-get-started-express.md)
Özelleştirilmiş ayarları kullanarak yükleme | [Azure AD Connect özel yüklemesi](active-directory-aadconnect-get-started-custom.md)
Yükleme için kullanılan hesaplar | [Azure AD Connect hesapları ve izinleri hakkında daha fazla bilgi](active-directory-aadconnect-accounts-permissions.md)



<!----HONumber=Jun16_HO2-->


