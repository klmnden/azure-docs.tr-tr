---
title: Microsoft Developer hesabı oluşturma | Azure Market
description: Gereksinimleri ve Microsoft Developer hesabı oluşturma adımları.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: 4fde5d81fb97bec23fdb46ff53b05874c88d9d67
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64935856"
---
<a name="create-a-microsoft-developer-account"></a>Microsoft Developer hesabı oluşturma
====================================

Bu makalede, Azure Market'te yayımlamak için onaylanmış bir Microsoft Developer haline açıklar.

## <a name="create-a-microsoft-account"></a>Microsoft hesabı oluşturun

Yayımlama işlemini başlatmak için tamamlanması gerekir **Microsoft Developer Center** kayıt. Kayıtlı hesap kullanacağınız **[bulut iş ortağı portalı](https://cloudpartner.azure.com/)** yayımlama işlemini başlatmak için.

### <a name="general-account-guidelines"></a>Genel hesabı yönergeleri

Azure Marketi Teklifleriniz için bir Microsoft hesabı yalnızca sahip olmasını öneririz. Bu hesap, hizmetleri veya teklifler özgü olmamalıdır.

Kullanıcı adını oluşturan adres, etki alanınız üzerinde olmalı ve BT ekibiniz tarafından denetlenir. Tüm yayımlama ilgili etkinlikleri bu hesap aracılığıyla yapılmalıdır.

>[!WARNING]
>"Azure" ve "Microsoft" gibi sözcükleri Microsoft hesap kaydı için desteklenmez. Hesap oluşturma ve kayıt işlemini tamamlamak için bu sözcükler kullanmaktan kaçının.

### <a name="company-account-guidelines"></a>Şirket hesabı yönergeleri

Hesap açılan Microsoft hesabıyla oturum açarak hesabınıza erişmek birden fazla kişi gerekiyorsa aşağıdaki yönergeleri izleyin.

>[!IMPORTANT]
>Birden çok kullanıcı Geliştirme Merkezi hesabınıza erişmesine izin vermek için Azure Active Directory rolleri ayrı kullanıcılara atamak için kullanmanızı öneririz. Bireysel oturum açarak hesaba erişebilir Azure AD kimlik. Daha fazla bilgi için [hesabı kullanıcılarını yönetmek](https://docs.microsoft.com/windows/uwp/publish/manage-account-users).

-   Şirketinize ait bir e-posta adresi kullanarak Microsoft hesabınızı oluşturma\'s etki alanı, ancak tek. Örneğin, windowsapps\@fabrikam.com.
-   Geliştiricilerin olası en küçük sayı bu Microsoft hesabına erişimi sınırlayın.
-   Geliştirici hesabına erişmesi için gereken herkes içeren bir şirket e-posta dağıtım listesini ayarlamak ve bu e-posta adresi güvenlik bilgilerinizi ekleyin. Bu tüm çalışanları, listede gerektiğinde güvenlik kodlarını ve Microsoft hesabınızın güvenlik bilgilerini yönetmenize olanak sağlar. Bir dağıtım listesi oluşturarak uygun değilse, tek bir e-posta hesabının sahibi erişim ve güvenlik kodunu (örneğin, yeni güvenlik bilgileri hesabınıza eklendiğinde veya ne zaman yeni bir cihaz üzerinden erişilmelidir.) istendiğinde paylaşmak kullanılabilir olması gerekir
-   Bir uzantı gerektirmez ve anahtar takım üyeleri için erişilebilir olan bir şirket telefon numarası ekleyin.
-   Genel olarak, şirketinizin Geliştirici hesabınıza oturum açmak için güvenilen cihazları kullanan geliştiricileri sahiptir. Tüm anahtar ekip üyeleri, bu güvenilen cihazlara erişimi olmalıdır. Bu hesap erişirken gönderilecek güvenlik kodlarını gereksinimini azaltır.
-   Güvenilir olmayan bir Bilgisayardan hesabına erişime ihtiyacınız varsa, en fazla beş geliştiriciden erişimi sınırlayın. İdeal olarak, bu geliştiricilerin hesabı, aynı coğrafi paylaşın ve ağ konumu makinelerden erişmelidir.
-   Sık gözden geçirin, [şirketin güvenlik bilgisi](https://account.live.com/proofs/Manage) geçerli olduğundan emin olmak için.

>[!IMPORTANT]
>Geliştirici hesabınızın birincil güvenilen bilgisayarlarından erişilmelidir. Hesap, haftası başına oluşturulan kodları sayısına bir sınır olduğundan bu önemlidir. Ayrıca, en kolay oturum açma deneyimi sağlar.
>
>Daha fazla bilgi için [ek Geliştirici hesabı kuralları ve güvenlik](https://msdn.microsoft.com/windows/uwp/publish/opening-a-developer-account#additional-guidelines-for-company-accounts).

### <a name="to-create-a-microsoft-account"></a>Bir Microsoft hesabı oluşturmak için

1.  Yeni bir gizli Chrome veya Internet Explorer gözatma oturumunda InPrivate, mevcut bir hesaba oturum açmadıysanız emin olmak için açın.
2.  Bunu kullanarak bir Microsoft hesabı olarak (önceki yönergeleri kullanarak) e-posta kaydetme [bağlantı](https://signup.live.com/signup.aspx). Aşağıdaki kaydolma yönergeleri izleyin:

    - Hesabınızı bir Microsoft hesabı olarak kaydolurken size kısa mesajla veya otomatik çağrı bir hesap doğrulama kodu göndermek için geçerli bir telefon numarası sistemi vermeniz gerekir.
    - Hesabınızı bir Microsoft hesabı olarak kaydolurken hesap doğrulaması için otomatik bir e-posta almak için geçerli bir e-posta kimliği sağlamanız gerekir.
    - DL'ye gönderilen e-posta adresini doğrulayın.

    Artık Microsoft Developer Center'da yeni bir Microsoft hesabı kullanmaya hazırsınız.

## <a name="register-your-account-in-microsoft-developer-center"></a>Microsoft Developer Center'da hesabınızı kaydedin

Microsoft Developer Center şirket bilgilerinin bir kere kaydetmek için kullanılır. Kayıt yetkilisi geçerli bir şirket temsilcisi olmalıdır ve kullanıcıların kimliğini doğrulamak için bir yol olarak kendi kişisel bilgilerini sağlamanız gerekir. Kaydetme kişi, şirket için paylaşılan bir Microsoft hesabı kullanmanız gerekir **ve bulut iş ortağı Portalı'nda aynı hesabı kullanılmalıdır.** Oluşturmak çalışmadan önce şirketinizin zaten bir Microsoft Developer Center hesabına sahip olmayan yapmak için denetleme yapmalıdır. İşlemi sırasında biz şirketin adres bilgilerini, banka hesabı bilgilerini toplayın ve vergi bilgilerini. Bu bilgiler genellikle finans bölümüyle veya işletmeyle ilgili kişilerden alınabilir.

>[!IMPORTANT]
>Teklif oluşturulmasını ve dağıtımını çeşitli aşamaları ilerleme için aşağıdaki Geliştirici profili bileşenleri tamamlamanız gerekir.

| Geliştirici profili     | Draft'ı başlatmak için    | Staging       | Ücretsiz yayımlama ve çözüm şablonu   | Ticari yayımlama   |
|---------------------- |----------------   |-----------    |-------------------------------------  |---------------------  |
| Kayıt şirketi  | Sahip olmalıdır         | Sahip olmalıdır     | Sahip olmalıdır                             | Sahip olmalıdır             |
| Vergi profili kimliği        | İsteğe bağlı          | İsteğe bağlı      | İsteğe bağlı                              | Sahip olmalıdır             |
| Banka hesabı          | İsteğe bağlı          | İsteğe bağlı      | İsteğe bağlı                              | Sahip olmalıdır             |

>[!NOTE]
>Getir kendi lisansını (KLG) yalnızca sanal makineler için desteklenir ve ücretsiz bir teklif olarak kabul edilir.

### <a name="register-your-company-account"></a>Şirket hesabınızı kaydedin

1. Yeni bir InPrivate Internet Explorer veya Chrome gizli gözatma oturumunda, kişisel bir hesapla oturum açmadıysanız emin olmak için açın.

2. Git [Windows Dev Center](https://dev.windows.com/registration?accountprogram=azure) kendiniz bir satıcı kaydedilecek. Devam etmeden önce lütfen aşağıdaki önemli nota okuyun.

   ![Microsoft hesabı doğrulama](./media/cloud-partner-portal-create-dev-center-registration/seller-dashboard-verify.jpg)

    >[!IMPORTANT]
    >İlk kez bir Microsoft hesabı olarak kayıtlı geliştirme Merkezi'nde kaydetmek için kullanan (bir dağıtım listesi kişilerden gelen bağımlılığı kaldırmak için önerilir) e-posta kimliği veya dağıtım listesinin konumunda olduğundan emin olun. Aksi durumda, bu bağlantıyı kullanarak Lütfen kaydedin. Ayrıca, Microsoft şirket etki alanı altında herhangi bir e-posta kimliği Geliştirme Merkezi kaydı için kullanılamaz.'

   ![Dev center oturum açma](./media/cloud-partner-portal-create-dev-center-registration/seller-dashboard-login.jpg)

3. Bir telefon numarası veya e-posta adresi kullanarak kimliğinizi doğrulamak için "hesabınızı korumamıza yardımcı olun" sihirbazını çalıştırın.

4. Kayıt hesabı bilgilerini seçin, **hesap ülke/bölge** seçin ve açılır listeden **sonraki**.

   ![Ülke/bölge seçin](./media/cloud-partner-portal-create-dev-center-registration/imgRegisterCo_04.png)

    >[!WARNING]
    >"Satış yapan" ülkeler/bölgeler: Hizmetlerinizi Azure Marketi'nde satmak için kayıtlı varlık onaylı "satış yapan" açılan listesinde gösterilen ülkeleri birinden olması gerekir. Bu kısıtlama ödeme ve vergi amaçlıdır. Market katılım ilkeleri daha fazla bilgi için bkz.

5. Seçin **şirket** seçin ve "Hesap türü" olarak **sonraki**.

    >[!IMPORTANT]
    >Daha iyi hesap türlerini anlamanıza ve hangi tür sizin için en iyi olduğuna karar vermek için sayfa hesap türlerini, konumları ve ücretler bir sonraki ekran görüntüsünde gösterilen görüntüleyin.

    ![Hesap türleri satıcılar için](./media/cloud-partner-portal-create-dev-center-registration/imgRegisterCo_05.png)

6. Girin **yayımcı görünen adı**. Bu genelde şirketinizin adıdır.

    >[!NOTE]
    >Teklifinizi listelenen sonra geliştirme Merkezi'nde girilen yayımcı görünen adı Azure Market'te görüntülenmiyor. Ancak bu bilgiler, kayıt işlemini tamamlamak için gereklidir.

7. Girin **kişi bilgisi** Hesap doğrulama.

    >[!IMPORTANT]
    >Bu bizim şirketiniz doğrulama işleminde Geliştirici Merkezi'nde onaylanması için kullanılacağından doğru iletişim bilgileri sağlamanız gerekir.'

8. Kişi bilgilerini girin **şirket onaylayanı**. Şirket onaylayanı, kuruluşunuz adına geliştirme Merkezi'nde hesap oluşturmak için yetkilendirilmiş olduğunuzu doğrulayın kişidir. Bu bilgileri verdikten sonra seçin **sonraki** taşımak için **ödeme bölüm**.

    ![Şirket onaylayanı tanımlayın](./media/cloud-partner-portal-create-dev-center-registration/imgRegisterCo_08.png)

9. Hesabınızın ödeme bilgilerini girin. Kayıt maliyetini kapsar bir promosyon kodu varsa, burada da girebilirsiniz. Aksi takdirde, kredi kartı bilgileri (veya desteklenen pazarda PayPal) sağlayın. Seçin **sonraki** için en son taşımak **gözden geçirme**.

   ![Ödeme kayıt](./media/cloud-partner-portal-create-dev-center-registration/imgRegisterCo_09.png)

10. Hesap bilgilerinizi gözden geçirin ve her şeyin doğru olduğundan emin olun. Okuyup hüküm ve koşulları kabul [Microsoft Azure Marketi yayımcı anlaşması](https://go.microsoft.com/fwlink/?LinkID=699560). Okuma ve bu koşulları kabul belirtmek için kutuyu işaretleyin.

11. Seçin **son** kaydınızı doğrulamak için. Bir onay iletisi e-posta adresinize gönderilir.

12. Yalnızca ücretsiz teklifleri yayımlama planlıyorsanız seçin [bulut iş ortağı Portalı'na gitmek](https://cloudpartner.azure.com/) "Bu makalede bulut iş ortağı portalı hesabınızdaki kaydetmek için" atlayın.

### <a name="commercial-offers"></a>Ticari teklifler

Bir saatlik faturalandırma modeli kullanarak bir sanal makine teklifi gibi ticari teklifleri yayımlama planlıyorsanız, vergi ve bankaya ilişkin bilgileri sağlamanız gerekir. Bu, Geliştirici Merkezi hesabınızda oturum açın ve seçin **hesap bilgilerinizi güncelleştirmeniz**. Sonraki bölümde yönergeleri "Bankacılık ve vergi bilgileri ekleme".

>[!IMPORTANT]
>Ticari bir teklif banka hesabı ve vergi bilgilerini sağlamadan üretime gönderme mümkün olmayacaktır.

Bankanız güncelleştirin ve vergi bilgilerini daha sonra isterseniz, "Bu makalede bulut iş ortağı portalı hesabınızdaki kaydetmek için" atlayabilirsiniz.

>[!NOTE]
>Vergi bilgileri doğrulamak için zaman alacağından banka hesabı ve vergi bilgilerini olabildiğince çabuk sağlama öneririz.

### <a name="add-banking-and-tax-information"></a>Ve vergi bilgilerini eklemek bankacılık

Ticari teklifleri satın alma için yayımlamak için ödeme ve vergi bilgilerini ve eklemek Geliştirici Merkezi'nde doğrulama için gönderme gerekir.

**Banka bilgi sağlamak için**

1.  Oturum [Microsoft Developer Center](https://dev.windows.com/registration?accountprogram=azure) Microsoft hesabınızla.
2.  Seçin **ödeme hesap** soldaki menüde altında **ödeme yöntemi seçin**seçin **banka hesabı** veya **PayPal**.

    >[!NOTE]
    >Müşterilerin Market'te satın alması ticari teklifler varsa, bu bu satın alma işlemleri için ödeme almanıza hesabıdır.
3.  Ödeme bilgilerini girin ve ardından **Kaydet**.

    >[!IMPORTANT]
    >Güncelleştirme veya ödeme hesabınızı değiştirmeniz gerekiyorsa yeni bilgilerle geçerli bilgilerini değiştirmek için önceki adımları izleyin.
    >
    >Ödeme hesabınızı değiştirme, ödemeler tarafından en çok bir ödeme dönemi gecikmeye yol açabilir. Bu gecikme, ödeme hesabınızı ayarlarken yaptığımız gibi hesap değişikliğini doğrulamak ihtiyacımız oluşur. Hesabınızı doğrulandıktan sonra hala tam miktar için ücretli; son ödeme için geçerli ödeme döngüsü sonraki teste eklenir.

4.  **İleri**’yi seçin.

**Vergi bilgileri sağlamak için**

1.  Oturum [Microsoft Developer Center](https://dev.windows.com/registration?accountprogram=azure) Microsoft hesabınıza (gerekirse).
2.  Sol menüden **vergi profili**.
3.  Üzerinde **, vergi formunu'kurmak** sayfası:
    - Ülke veya kalıcı yerleşimi bulunduğu bölgeyi seçin.
    - Ülke veya bölge birincil Vatandaşlık tutun yeri seçin.
    - **İleri**’yi seçin.
4.  Vergi ayrıntılarınızı girin ve ardından **sonraki**.

>[!WARNING]
>Banka hesabı ve vergi bilgilerini Microsoft Developer Center hesabınızdaki sağlamadan ticari tekliflerinizi üretime gönderme mümkün olmayacaktır.

### <a name="developer-center-registration-issues"></a>Geliştirici Merkezi kayıt sorunları

Geliştirici Merkezi kayıt ile ilgili sorunlar varsa, bir destek bileti açmak için aşağıdaki adımları kullanın.

1.  Git [destek bağlantısı](https://developer.microsoft.com/windows/support).
2.  Altında **bize**seçin **bir olay göndermek**.
    ![Bir bilet açın](./media/cloud-partner-portal-create-dev-center-registration/imgAddTax_02.png)
3.  İçin **sorun türü**, "Yardım ile Geliştirme Merkezi" seçin ve **kategori**seçin "Yayımla ve uygulamaları yönetme". Seçin **Başlat e-posta**.

    ![sorun türünü tanımlayın](./media/cloud-partner-portal-create-dev-center-registration/imgAddTax_03.png)

4.  Bir oturum açma sayfasında verilir. Oturum açmak için herhangi bir Microsoft hesabı kullanın. Ardından bir Microsoft hesabınız yoksa [oluşturmak](https://signup.live.com/signup?uaid=0089f09ccae94043a0f07c2aaf928831&lic=1). \

5.  Seçin ve sorun hakkında ayrıntılı bilgi sağlar **Gönder** bilet göndermek için.

    ![bilet gönderin](./media/cloud-partner-portal-create-dev-center-registration/imgAddTax_05.png)

## <a name="register-your-account-in-the-cloud-partner-portal"></a>Bulut iş ortağı Portalı'nda hesabınızı kaydedin

Kullandığınız [bulut iş ortağı portalı](https://cloudpartner.azure.com/) tekliflerinizi yönetmek ve yayımlamak için.

1.  Yeni bir gizli Chrome veya Internet Explorer gözatma oturumunda InPrivate, kişisel bir hesapla oturum açmadıysanız emin olmak için açın.
2.  Git [bulut iş ortağı portalı](https://cloudpartner.azure.com/).
3.  Yeni bir kullanıcı ve oturum açma kullanıyorsanız [bulut iş ortağı portalı](https://cloudpartner.azure.com/) ilk kez, ardından, Geliştirme Merkezi hesabınızla kayıtlı aynı e-posta kimliği kullanarak oturum açmanız gerekir. Bu, Geliştirici Merkezi hesabınızda ve bulut iş ortağı portalı hesabı birbirine bağlanır sağlar.

Daha sonra uygulama üzerinde çalışan diğer üyelerinin şirket ekleyebilirsiniz. Bunları katkıda bulunanlar veya Bulut iş ortağı Portalı'nda sahipleri olarak sonraki bölümde yer alan adımları izleyerek yapabilirsiniz.

Ardından bulut iş ortağı portalı portalı katkıda bulunan/sahibi olarak eklediyseniz, kendi hesabınızla oturum açın.

>[!TIP]
>Katılım ilkeleri Azure Web sitesinde açıklanmıştır.

## <a name="manage-users-as-owners-or-contributors-in-the-cloud-partner-portal"></a>Sahipleri veya Bulut iş ortağı Portalı'nda katkıda bulunanlar olarak kullanıcıları yönetme

[Bulut iş ortağı portalı kullanıcıları yönetme adımları](./cloud-partner-portal-manage-users.md)


## <a name="next-steps"></a>Sonraki adımlar

Hesabınızı oluşturduğunuz ve kayıtlı göre Azure Marketi'nde yayımlama işlemi başlatabilirsiniz.
