
<properties
    pageTitle="Azure AD Connect Health'i AD FS ile Kullanma| Microsoft Azure"
    description="Bu Azure AD Connect Health sayfasında, şirket içi AD FS altyapınızı nasıl izleyeceğiniz açıklanmıştır."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="stevenpo"
    editor="karavar"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="03/21/2016"
    ms.author="vakarand"/>

# Azure AD Connect Health'i AD FS ile Kullanma
Aşağıdaki belgeler, AD FS altyapınızın Azure AD Connect Health ile izlenmesine ilişkin belgelerdir.

## AD FS uyarıları
Azure AD Connect Health Uyarıları bölümünde etkin uyarıların listesi mevcuttur. Her uyarı için ilgili bilgiler, çözüm adımları ve ilgili belgelere yönelik bağlantılar verilmiştir. Etkin veya çözümlenmiş bir uyarıyı seçtiğinizde, alarmı çözümlemek için uygulayabileceğiniz adımların ve ek belgelere yönelik bağlantıların yanı sıra ek bilgiler içeren yeni bir dikey pencere görürsünüz. Ayrıca, geçmişte çözümlenen uyarılara ilişkin geçmiş verileri de görüntüleyebilirsiniz.

Bir uyarıyı seçtiğinizde, uyarıyı çözümlemek için uygulayabileceğiniz adımlar ve ek belgelere yönelik bağlantıların yanı sıra ek bilgiler alırsınız.

![Azure AD Connect Health Portalı](./media/active-directory-aadconnect-health/alert2.png)



## AD FS için Kullanım Analizi
Azure AD Connect Health Kullanım Analizi, federasyon sunucularınızın kimlik doğrulama trafiğini analiz eder. Kullanım analizi kutusunu seçtiğinizde, ölçümleri ve gruplandırmaları gösteren kullanım analizi dikey penceresi açılır.

>[AZURE.NOTE] Kullanım Analizini AD FS ile kullanabilmek için AD FS denetiminin etkin olduğundan emin olmanız gerekir. Daha fazla bilgi için bkz. [AD FS için Denetimi Etkinleştirme](active-directory-aadconnect-health-agent-install.md#enable-auditing-for-ad-fs).

![Azure AD Connect Health Portalı](./media/active-directory-aadconnect-health/report1.png)

Ek ölçümler seçmek, zaman aralığı belirtmek veya gruplandırmayı değiştirmek için kullanım analizi grafiğine sağ tıklayıp Grafiği Düzenle seçeneğini belirlemeniz yeterlidir. Ardından zaman aralığı belirtebilir, ölçümleri değiştirip seçebilir ve gruplandırmayı değiştirebilirsiniz. Kimlik doğrulama trafiğinin dağıtımını farklı "ölçümlere" göre görüntüleyebilir ve aşağıda açıklanan ilgili "gruplandırma ölçütü" parametrelerini kullanarak her ölçümü gruplandırabilirsiniz.

| Ölçüm | Gruplandırma Ölçütü | Gruplandırma nedir ve ne işe yarar? |
| ------ | -------- | -------------------------------------------- |
| Toplam İstekler: Federasyon hizmeti tarafından yürütülen isteklerin toplam sayısı | Tümü | Bu, gruplandırma olmadan isteklerin toplam sayısını gösterir. |
|  | Uygulama | Bu seçenek, tüm istekleri hedeflenen bağlı olan tarafa göre gruplandırır. Bu gruplandırma, hangi uygulamanın toplam trafiğin yüzde kaçını aldığını anlamak için kullanışlıdır. |
|  | Sunucu | Bu seçenek, tüm istekleri isteği işleyen sunucuya göre gruplandırır. Bu gruplandırma, toplam trafiğin yük dağıtımını anlamak için kullanışlıdır. |
|  | Çalışma Alanına Katılım | Bu seçenek, tüm istekleri çalışma alanına katılmış (bilinen) cihazlardan gelip gelmediğine göre gruplandırır. Bu gruplandırma, kaynaklarınıza kimlik altyapısı tarafından bilinmeyen cihazlar kullanılarak erişilip erişilmediğini anlamak için kullanışlıdır. |
|  | Kimlik Doğrulama Yöntemi | Bu seçenek toplam istekleri kimlik doğrulamak için kullanılan kimlik doğrulama yöntemine göre gruplandırır. Bu gruplandırma, kimlik doğrulaması için kullanılan yaygın kimlik doğrulama yöntemini anlamak için kullanışlıdır. Olası kimlik doğrulama yöntemleri aşağıda verilmiştir <ol> <li>Windows Tümleşik Kimlik Doğrulaması (Windows)</li> <li>Forms Tabanlı Kimlik Doğrulaması (Forms)</li> <li>SSO (Çoklu Oturum Açma)</li> <li>X509 Sertifika Doğrulaması (Sertifika)</li> <br>Federasyon sunucularının bir isteği SSO Tanımlama Bilgisi ile alması halinde, bu isteğin SSO (Çoklu Oturum Açma) sayıldığını unutmayın. Bu gibi durumlarda, tanımlama bilgisi geçerliyse kullanıcıdan kimlik bilgilerini sağlaması istenmez ve kullanıcı uygulamaya sorunsuz bir şekilde erişir. Federasyon sunucuları tarafından korunan birden fazla bağlı olan tarafınız varsa bu yaygın bir durumdur. |
|  | Ağ Konumu | Bu seçenek, tüm istekleri kullanıcının ağ konumuna göre gruplandırır. İntranet veya extranet olabilir. Bu gruplandırma, trafiğin yüzde kaçının intranetten veya extranetten geldiğini anlamak için kullanışlıdır. |
| Toplam Başarısız İstek: Federasyon hizmeti tarafından işlenen başarısız isteklerin toplam sayısı. <br> (Bu ölçüm yalnızca Windows Server 2012 R2 için AD FS'de kullanılabilir)| Hata Türü | Bu, önceden tanımlanmış hata türlerine göre hata sayısını gösterir. Bu gruplandırma, genel hata türlerini anlamak için kullanışlıdır. <ul><li>Hatalı Kullanıcı Adı veya Parola: Hatalı kullanıcı adı veya paroladan kaynaklanan hatalar.</li> <li>"Extranet Kilitleme": Extranet dışında bırakılan bir kullanıcıdan alınan isteklerden kaynaklanan hatalar </li><li> "Süresi Dolan Parola": Süresi dolmuş bir parolayla oturum açmaya çalışan kullanıcılardan kaynaklanan hatalar.</li><li>"Devre Dışı Hesap": Devre dışı bırakılmış bir hesap ile oturum açmaya çalışan kullanıcılardan kaynaklanan hatalar.</li><li>"Cihaz Kimlik Doğrulaması": Cihaz Kimlik Doğrulamasını kullanarak kimlik doğrulaması gerçekleştiremeyen kullanıcılardan kaynaklanan hatalar.</li><li>"Kullanıcı Sertifikası Kimlik Doğrulaması": Geçersiz bir sertifika nedeniyle kimlik doğrulaması gerçekleştiremeyen kullanıcılardan kaynaklanan hatalar.</li><li>"MFA": Multi-Factor Authentication kullanarak kimlik doğrulaması yapmakta başarısız olan kullanıcılardan kaynaklanan hatalar.</li><li>"Diğer Kimlik Bilgisi": "Sertifika Verme Yetkilendirmesi": Başarısız yetkilendirmelerden kaynaklanan hatalar.</li><li>"Sertifika Verme Temsilcisi": Sertifika verme temsilcisi hatalarından kaynaklanan hatalar.</li><li>"Belirteç Onayı": ADFS'nin 3. taraf Kimlik Sağlayıcısından gelen bir belirteci reddetmesinden kaynaklanan hatalar.</li><li>"Protokol": Protokol hatalarından kaynaklanan hatalar.</li><li>"Bilinmeyen": Farklı nedenlerden kaynaklananlar. Tanımlanan kategorilere uymayan diğer tüm hatalar.</li> |
|  | Sunucu | Bu, hataları sunucuya göre gruplandırır. Sunucular genelindeki hata dağıtımını anlamak için kullanışlıdır. Düzensiz dağıtım, hatalı durumdaki bir sunucunun göstergesi olabilir. |
|  | Ağ Konumu | Bu, hataları isteklerin ağ konumuna (intranet veya extranet) göre gruplandırır. Başarısız olan istek türlerini anlamak için kullanışlıdır. |
|  | Uygulama | Bu, hataları hedeflenen uygulamaya (bağlı olan taraf) göre gruplandırır. Hedeflenen hangi uygulamanın en çok hata sayısını gördüğünü anlamak için kullanışlıdır. |
| Kullanıcı Sayısı: Sistemdeki benzersiz etkin kullanıcıların ortalama sayısı | Tümü | Bu, seçilen bir zaman dilimi içinde federasyon hizmeti kullanan kullanıcıların ortalama sayısını sağlar. Kullanıcılar gruplandırılmaz. <br>Ortalama, seçilen zaman dilimine bağlı olarak değişir. |
|  | Uygulama | Bu, ortalama kullanıcı sayısını hedeflenen uygulamaya (bağlı olan taraf) göre gruplandırır. Bu, kaç kullanıcının hangi uygulamayı kullanmakta olduğunu anlamak için kullanışlıdır. |


## AD FS için Performans İzleme
Azure AD Connect Health Performans İzleme, ölçümlere ilişkin izleme bilgileri sağlar. İzleme kutusunu seçtiğinizde, ölçümlere ilişkin ayrıntılı bilgiler bulabileceğiniz bir dikey pencere açılır.


![Azure AD Connect Health Portalı](./media/active-directory-aadconnect-health/perf1.png)


Dikey pencerenin üst kısmındaki Filtre seçeneğini işaretlediğinizde her bir sunucunun ölçümlerini görmek için sunucuya göre filtreleme yapabilirsiniz. Ölçümleri değiştirmek için, yalnızca izleme dikey penceresinin altındaki izleme grafiğine tıklamanız ve Grafiği Düzenle öğesini seçmeniz yeterlidir. Ardından, açılan yeni dikey pencerede bulunan açılır menüden başka ölçümler seçebilir ve performans verilerini görüntülemek istediğiniz zaman aralığını belirtebilirsiniz.

## AD FS raporları
Azure AD Connect Health, AD FS'nin etkinlik ve performansı hakkında raporlar sağlar. Bu raporlar, yöneticilerin AD FS sunucularındaki etkinlikler hakkında öngörü edinmelerine yardımcı olur.

### Hatalı Kullanıcı Adı/Parola kullanarak oturum açamayan İlk 50 Kullanıcı

Bir AD FS sunucusunda gerçekleşen başarısız kimlik doğrulama isteğinin en yaygın nedenlerinden biri geçersiz kimlik bilgileri ile ilgili isteklerdir, yani yanlış kullanıcı adı veya şifre istekleridir. Bu genellikle karmaşık parola kullanımından, parolaların unutulmasından ve yazım yanlışlarından kaynaklanır.

Ancak AD FS sunucularınız tarafından ele alınan bu tür beklenmedik miktarlardaki isteklerin başka nedenleri de vardır. Bir uygulama tarafından önbelleğe alınan kullanıcı kimlik bilgilerinin süresinin dolması veya kötü niyetli bir kullanıcının iyi bilinen bir dizi parola ile bir kullanıcının hesabına girmeyi denemesi bunlara örnek olarak verilebilir.

ADFS için Azure AD Connect Health, geçersiz kullanıcı adı veya paroladan dolayı oturum açma denemeleri başarısız olan İlk 50 Kullanıcıya ilişkin bir rapor sağlar. Bu, gruplardaki tüm AD FS sunucuları tarafından oluşturulan denetim olaylarının işlenmesiyle sağlanır.

![Azure AD Connect Health Portalı](./media/active-directory-aadconnect-health-adfs/report1a.png)

Bu raporda şu bilgilere kolayca erişebilirsiniz:

- Son 30 gün içinde yanlış kullanıcı adı/parola ile başarısız olmuş isteklerin toplam sayısı
- Her gün hatalı kullanıcı adı/parola ile oturum açarak başarısız olan kullanıcıların ortalama sayısı.

Bu bölüme tıkladığınızda, ek ayrıntılı bilgi görüntüleyebileceğiniz ana rapor dikey penceresine yönlendirilirsiniz. Burada yanlış kullanıcı adı ve parola isteklerine zemin oluşturan eğilim bilgilerini görebileceğiniz bir grafiğin yanı sıra, başarısız oturum açma deneme sayılarına göre ilk 50'de yer alan kullanıcıların bir listesini bulabilirsiniz.

Grafikte şu bilgiler yer alır:

- Günlük olarak hatalı kullanıcı adı/parola nedeniyle başarısız olan oturum açma işlemlerinin toplam sayısı.
- Günlük olarak oturum açmada başarısız olan benzersiz kullanıcıların toplam sayısı.

![Azure AD Connect Health Portalı](./media/active-directory-aadconnect-health-adfs/report2a.png)

Raporda şunlar yer alır:

| Rapor Öğesi | Açıklama
| ------ | -------- |
|Kullanıcı Kimliği| Kullanılan kullanıcı kimliğini gösterir. Değerin kullanıcının girdiği değer olduğunu ve bazı durumlarda yanlış kullanıcı kimliğinin kullanıldığını görebileceğinizi unutmayın.|
|Başarısız Denemeler|Belirli bir kullanıcı kimliğine ait başarısız denemelerin toplam sayısını gösterir. Tablo, en fazla deneme sayısından en aza doğru azalan bir düzende sıralanır.|
|Son Hata|Bu, son hatanın oluştuğu andaki zaman damgasını gösterir.



>[AZURE.NOTE] Bu rapor, her 2 saatte bir bu süre içinde toplanan yeni bilgiler ile otomatik olarak güncelleştirilir. Bu nedenle raporda son 2 saat içinde gerçekleşen oturum açma denemeleri bulunmayabilir.



## İlgili bağlantılar

* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Azure AD Connect Health Aracısı Yüklemesi](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect Health İşlemleri](active-directory-aadconnect-health-operations.md)
* [Eşitleme için Azure AD Connect Health'i kullanma](active-directory-aadconnect-health-sync.md)
* [Azure AD Connect Health ile ilgili SSS](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect Health Sürüm Geçmişi](active-directory-aadconnect-health-version-history.md)



<!----HONumber=Jun16_HO2-->


