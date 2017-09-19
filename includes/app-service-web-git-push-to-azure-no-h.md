Yerel terminal penceresinde yerel Git deponuza bir Azure uzak deposu ekleyin.

```bash
git remote add azure <URI from previous step>
```

Aşağıdaki komutla uygulamanızı dağıtmak için Azure uzak deposuna gönderin. Parola istendiğinde Azure portalında oturum açarken kullandığınız parolayı değil [Dağıtım kullanıcısı yapılandırma](#configure-a-deployment-user) adımında oluşturduğunuz parolayı girdiğinizden emin olun.

```bash
git push azure master
```

Önceki komut, aşağıdaki örneğe benzer bilgiler görüntüler:
