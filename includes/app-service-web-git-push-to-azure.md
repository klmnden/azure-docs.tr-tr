## <a name="push-to-azure-from-git"></a>Git üzerinden Azure'a gönderme

Yerel Git deponuza bir Azure uzak deposu ekleyin.

```bash
git remote add azure <URI from previous step>
```

Uygulamanızı dağıtmak için Azure uzak deposuna gönderin. Daha önce dağıtım kullanıcısını oluştururken oluşturduğunuz parola istenir. Azure portalında oturum açarken kullandığınız parolayı değil [Dağıtım kullanıcısı yapılandırma](#configure-a-deployment-user) adımında oluşturduğunuz parolayı girdiğinizden emin olun.

```bash
git push azure master
```

Önceki komut, aşağıdaki örneğe benzer bilgiler görüntüler:
