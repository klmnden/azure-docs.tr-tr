Yerel terminal penceresinde yerel Git deponuza bir Azure uzak deposu ekleyin. Bu Azure uzaktan uygulamasında oluşturulan [bir web uygulaması oluşturma](#create-a-web-app).

```bash
git remote add azure <deploymentLocalGitUrl-from-create-step>
```

Aşağıdaki komutla uygulamanızı dağıtmak için Azure uzak deposuna gönderin. Parola istendiğinde Azure portalında oturum açarken kullandığınız parolayı değil [Dağıtım kullanıcısı yapılandırma](#configure-a-deployment-user) adımında oluşturduğunuz parolayı girdiğinizden emin olun.

```bash
git push azure master
```

Bu komutu çalıştırmak için birkaç dakika sürebilir. Çalıştırırken, aşağıdaki örneğe benzer bilgileri görüntüler:
