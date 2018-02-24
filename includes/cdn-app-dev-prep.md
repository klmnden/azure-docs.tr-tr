## <a name="prerequisites"></a>Önkoşullar
CDN yönetim kod yazmadan önce Azure Kaynak Yöneticisi ile etkileşim kurmak için kodu etkinleştirmek için bazı hazırlıklar yapmanız gerekir. Bu hazırlık yapmak için aktarmanız gerekir:

* Bu öğreticide oluşturduğunuz CDN profili içermesi için bir kaynak grubu oluştur
* Uygulaması için kimlik doğrulaması sağlamak için Azure Active Directory yapılandırma
* Böylece yalnızca yetkili kullanıcıların Azure AD kiracınız CDN profili ile etkileşim kurabilir kaynak grubu için izinleri uygula

### <a name="creating-the-resource-group"></a>Kaynak grubu oluşturma
1. İçine oturum [Azure Portal](https://portal.azure.com).
2. Tıklatın **kaynak oluşturma**.
3. Arama **kaynak grubu** ve kaynak grubu bölmesinde **oluşturma**.

    ![Yeni bir kaynak grubu oluşturma](./media/cdn-app-dev-prep/cdn-new-rg-1-include.png)
3. Kaynak grubunuzu adlandırın *CdnConsoleTutorial*.  Aboneliğinizi seçin ve yakın bir konum seçin.  İsterseniz, tıklayabilirsiniz **panoya Sabitle** portalda kaynak grubu panoya sabitlemek için onay kutusunu.  Sabitleme sonra bulmayı kolaylaştırır.  Seçimlerinizi yaptıktan sonra tıklatın **oluşturma**.

    ![Kaynak grubu adlandırma](./media/cdn-app-dev-prep/cdn-new-rg-2-include.png)
4. Panonuza sabitlemek alamadık, kaynak grubu oluşturduktan sonra bunu tıklayarak bulabilirsiniz **Gözat**, ardından **kaynak grupları**.  Açmak için kaynak grubunu tıklatın.  Not, **abonelik kimliği**. Bunu daha sonra ihtiyacımız var.

    ![Kaynak grubu adlandırma](./media/cdn-app-dev-prep/cdn-subscription-id-include.png)

### <a name="creating-the-azure-ad-application-and-applying-permissions"></a>Azure AD uygulaması oluşturma ve izinleri uygulama
Azure Active Directory ile uygulama kimlik doğrulamasına iki yaklaşım vardır: tek tek kullanıcılara veya bir hizmet sorumlusu. Bir hizmet sorumlusu Windows hizmet hesabında benzer.  İzinler, belirli bir kullanıcının CDN profilleri ile etkileşim kurmasına izin verme yerine, bunun yerine hizmet sorumlusu verilir.  Hizmet asıl adı, genellikle otomatik, etkileşimli olmayan işlemleri için kullanılır.  Bu öğretici bir etkileşimli konsol uygulaması yazma olsa bile, biz hizmet asıl yaklaşıma odak.

Bir hizmet sorumlusu oluşturma Azure Active Directory Uygulama oluşturma dahil olmak üzere birkaç adımdan oluşur.  Oluşturmak için oluşturacağız [Bu öğreticiyi izleyin](../articles/resource-group-create-service-principal-portal.md).

> [!IMPORTANT]
> Tüm adımları izlediğinizden emin olun [bağlantılı Öğreticisi](../articles/resource-group-create-service-principal-portal.md).  Bu *önemli* , tam olarak açıklandığı gibi tamamlayın.  Not emin olun, **kimliği Kiracı**, **Kiracı etki alanı adı** (genellikle bir *. onmicrosoft.com* etki alanı özel bir etki alanı belirtmediğiniz sürece), **istemci kimliği** , ve **istemci kimlik doğrulama anahtarı**daha sonra bu bilgilere ihtiyacımız gibi.  Koruma sağlamak dikkatli olun, **istemci kimliği** ve **istemci kimlik doğrulama anahtarı**gibi bu kimlik bilgileri herkes tarafından hizmet sorumlusu işlemlerini yürütmek için kullanılabilir.
>
> Yapılandırma çok kiracılı uygulama adlı adım aldığınızda, seçin **Hayır**.
>
> Adım ulaştıklarında [uygulama rol atama](../articles/azure-resource-manager/resource-group-create-service-principal-portal.md#assign-application-to-role), daha önce oluşturduğunuz kaynak grubunu kullanmak *CdnConsoleTutorial*, ancak yerine **okuyucu** rolü atayın **CDN profili katkıda bulunan** rol.  Uygulama atadıktan sonra **CDN profili katkıda bulunan** , kaynak grubu, Bu öğretici için dönüş rolü. 
>
>

Hizmet sorumlusu oluşturup atanan sonra **CDN profili katkıda bulunan** rolü **kullanıcılar** dikey penceresinde kaynak grubunuz için aşağıdaki görüntüye benzer görünmelidir.

![Kullanıcılar dikey penceresi](./media/cdn-app-dev-prep/cdn-service-principal-include.png)

### <a name="interactive-user-authentication"></a>Etkileşimli kullanıcı kimlik doğrulaması
Bir hizmet sorumlusu yerine etkileşimli tek tek kullanıcı kimlik doğrulaması yerine olurdu varsa, bir hizmet sorumlusu için benzer bir işlemdir.  Aslında, aynı yordamı izleyin, ancak bazı küçük değişiklikler yapmak gerekir.

> [!IMPORTANT]
> Yalnızca bir hizmet sorumlusu yerine tek tek kullanıcı kimlik doğrulaması kullanmayı tercih sonraki adımları izleyin.
>
>

1. Uygulamanızın, yerine oluştururken **Web uygulaması**, seçin **yerel uygulama**.

    ![Yerel uygulama](./media/cdn-app-dev-prep/cdn-native-application-include.png)
2. Sonraki sayfada, sizden istenir bir **yeniden yönlendirme URI'si**.  URI doğrulanması çalışmaz, ancak ne girdiğiniz unutmayın. Daha sonra ihtiyacınız.
3. Oluşturmak için gerekli bir **istemci kimlik doğrulama anahtarı**.
4. Bir hizmet sorumlusu atama yerine **CDN profili katkıda bulunan** rolü yapmamız bireysel kullanıcıları veya grupları atamak için.  Bu örnekte, ı atadığınız görebilirsiniz *CDN Demo kullanıcı* için **CDN profili katkıda bulunan** rol.  

    ![Tek tek kullanıcı erişimi](./media/cdn-app-dev-prep/cdn-aad-user-include.png)
