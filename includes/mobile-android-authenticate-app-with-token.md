
Önceki örnekte oturum açma, uygulama her başlatıldığında kimlik sağlayıcısı ve arka uç Azure hizmeti başvurun istemciye gerektiren standart gösterdi. Bu yöntem yetersiz olduğunu ve birçok müşteri uygulamanız aynı anda başlatmayı denerseniz kullanımı ile ilgili sorunlar olabilir. Daha iyi bir yaklaşım olduğu Azure hizmet tarafından döndürülen yetkilendirme belirtecini önbelleğe ve bir sağlayıcı tabanlı oturum açma kullanmadan önce bu ilk kullanmayı deneyin.

> [!NOTE]
> Arka uç istemcisi tarafından yönetilen veya hizmet tarafından yönetilen kimlik doğrulaması kullanıp bağımsız olarak Azure hizmeti tarafından verilen belirteç önbelleğe alabilir. Bu öğretici, kimlik doğrulama hizmeti tarafından yönetilen kullanır.
>
>

1. ToDoActivity.java dosyasını açın ve aşağıdaki içeri aktarma deyimlerini ekleyin:

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;
2. Aşağıdaki üye ekleme `ToDoActivity` sınıfı.

        public static final String SHAREDPREFFILE = "temp";    
        public static final String USERIDPREF = "uid";    
        public static final String TOKENPREF = "tkn";    
3. ToDoActivity.java dosyasında aşağıdaki tanımı Ekle `cacheUserToken` yöntemi.

        private void cacheUserToken(MobileServiceUser user)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            Editor editor = prefs.edit();
            editor.putString(USERIDPREF, user.getUserId());
            editor.putString(TOKENPREF, user.getAuthenticationToken());
            editor.commit();
        }    

    Bu yöntem kullanıcı kimliği ve belirteç özel olarak işaretlenmiş bir tercih dosyasında depolar. Böylece cihazdaki diğer uygulamalar erişim belirtecine sahip olmayan bu önbelleğe erişim korumanız gerekir. Tercih uygulama için korumalı. Birisi aygıtına erişim kazanırsa, ancak belirteç önbelleği arasında başka yollarla erişim kazanmak mümkündür.

   > [!NOTE]
   > Ayrıca, belirteç erişim için yüksek oranda gizli olarak kabul edilir ve birisi aygıta erişim belirteç şifreleme ile koruyabilirsiniz. Tamamen güvenli bir çözümdür Bu öğretici kapsamında değildir ancak ve güvenlik gereksinimlerine bağlıdır.
   >
   >
4. ToDoActivity.java dosyasında aşağıdaki tanımı Ekle `loadUserTokenCache` yöntemi.

        private boolean loadUserTokenCache(MobileServiceClient client)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            String userId = prefs.getString(USERIDPREF, null);
            if (userId == null)
                return false;
            String token = prefs.getString(TOKENPREF, null);
            if (token == null)
                return false;

            MobileServiceUser user = new MobileServiceUser(userId);
            user.setAuthenticationToken(token);
            client.setCurrentUser(user);

            return true;
        }
5. İçinde *ToDoActivity.java* dosya, yerine `authenticate` yöntemi ile bir belirteç önbelleği kullanan aşağıdaki yöntemi. Google dışında bir hesap kullanmak istiyorsanız, oturum açma sağlayıcısı değiştirin.

        private void authenticate() {
            // We first try to load a token cache if one exists.
            if (loadUserTokenCache(mClient))
            {
                createTable();
            }
            // If we failed to load a token cache, login and create a token cache
            else
            {
                // Login using the Google provider.    
                ListenableFuture<MobileServiceUser> mLogin = mClient.login(MobileServiceAuthenticationProvider.Google);

                Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                    @Override
                    public void onFailure(Throwable exc) {
                        createAndShowDialog("You must log in. Login Required", "Error");
                    }           
                    @Override
                    public void onSuccess(MobileServiceUser user) {
                        createAndShowDialog(String.format(
                                "You are now logged in - %1$2s",
                                user.getUserId()), "Success");
                        cacheUserToken(mClient.getCurrentUser());
                        createTable();    
                    }
                });
            }
        }
6. Geçerli bir hesap kullanarak uygulama ve test kimlik doğrulaması oluşturun. Çalıştırın en az iki kez. İlk çalıştırmada oturum açın ve belirteç önbelleği oluşturmak için bir istem almanız gerekir. Bundan sonra her çalıştırmayı kimlik doğrulaması için belirteç önbelleği yüklemeyi dener. Oturum açmak için gerekli olmamalıdır.
