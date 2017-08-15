Uygulamanızda ayrıntılı bir parola sıfırlama özelliği sunmak için bir parola sıfırlama İlkesi oluşturmanız gerekir. Kiracı genelinde parola sıfırlama seçeneği Not [burada](../articles/active-directory-b2c/active-directory-b2c-reference-sspr.md) belirtilir. Bu ilke, tüketicilerin parola sıfırlama sırasında karşılaşacağı deneyimleri ve işlem başarıyla tamamlandığında uygulamanın alacağı belirteçlerin içeriğini açıklar.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

Ayarların ilkeler bölümünde **Parola sıfırlama ilkeleri**’ni seçip **+ Ekle**’ye tıklayın.

![Oturum açma veya kaydolma ilkeleri’ni seçin ve Ekle düğmesine tıklayın.](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-policy.png)

Uygulamanızın başvuracağı ilke **Adını** girin. Örneğin, `SSPR` girin.

**Kimlik sağlayıcıları**’nı seçin ve **E-posta adresi kullanarak parola sıfırla**’yı işaretleyin. **Tamam** düğmesine tıklayın.

![Kimlik sağlayıcısı olarak E-posta adresi kullanarak parola sıfırla’yı seçin ve Tamam düğmesine tıklayın](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-identity-providers.png)

**Uygulama talepleri**’ni seçin. Başarılı bir parola sıfırlama deneyiminden sonra uygulamanıza geri gönderilen yetkilendirme belirteçlerinde döndürülmesini istediğiniz talepleri seçin. Örneğin, **Kullanıcının Nesne Kimliği**’ni seçin.

![Bazı uygulama taleplerini seçin ve Tamam düğmesine tıklayın.](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-application-claims.png)

İlkeyi eklemek için **Oluştur**’a tıklayın. İlke **B2C_1_SSPR** olarak listelenir. Ada **B2C_1_** öneki eklenir.

**B2C_1_SSPR** adını seçerek ilkeyi açın. Tabloda belirtilen ayarlarını doğrulayın, ardından **Şimdi Çalıştır**’a tıklayın.

![İlkeyi seçin ve çalıştırın.](media/active-directory-b2c-create-password-reset-policy/run-b2c-password-reset-policy.png)

| Ayar      | Değer  |
| ------------ | ------ |
| **Uygulamalar** | Contoso B2C uygulaması |
| **Yanıt URL'sini seçin** | `https://localhost:44316/` |

Yeni bir tarayıcı sekmesi açılır. Buradan, uygulamanızın parola sıfırlamaya yönelik tüketici deneyimini doğrulayabilirsiniz.

> [!NOTE]
> İlke oluşturma ve güncelleştirmelerinin etkili olması bir dakika kadar alabilir.
>
