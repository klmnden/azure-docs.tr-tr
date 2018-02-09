Yerel terminal penceresinde yerel Git deponuza bir Azure uzak deposu ekleyin. _&lt;Kopyalanmış\_URL‘yi\_buraya\_yapıştırın>_ öğesini, [Bir web uygulaması oluşturun](#create) bölümünde kaydettiğiniz Git uzak URL’si ile değiştirin.

```bash
git remote add azure <deploymentLocalGitUrl-from-create-step>
```

Aşağıdaki komutla uygulamanızı dağıtmak için Azure uzak deposuna gönderin. Parola istendiğinde Azure portalında oturum açarken kullandığınız parolayı değil [Dağıtım kullanıcısı yapılandırma](#configure-a-deployment-user) adımında oluşturduğunuz parolayı girdiğinizden emin olun.

```bash
git push azure master
```

Bu komutun çalıştırılması birkaç dakika sürebilir. Çalıştırıldığında, aşağıdaki örneğe benzer bilgiler görüntüler:
