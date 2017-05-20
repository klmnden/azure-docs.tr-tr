Noktadan Siteye bağlantı kullanarak bir sanal ağa bağlanan her istemci bilgisayarda bir istemci sertifikası yüklü olmalıdır. İstemci sertifikası kök sertifikadan oluşturulur ve her bir istemci bilgisayara yüklenir. Geçerli bir istemci sertifikası yüklü değilse ve istemci sanal ağa bağlanmaya çalışırsa, kimlik doğrulaması başarısız olur.

Her istemci için benzersiz bir sertifika oluşturabileceğiniz gibi, birden çok istemci için aynı sertifikayı da kullanabilirsiniz. Benzersiz istemci sertifikaları oluşturmanın avantajı, tek bir sertifikayı iptal edebiliyor olmanızdır. Aksi takdirde, birden fazla istemci aynı istemci sertifikasını kullanıyorsa ve sertifikayı iptal etmeniz gerekirse, kimlik doğrulaması için bu sertifikayı kullanan tüm istemciler için yeni sertifikalar oluşturup yüklemeniz gerekir.

Aşağıdaki yöntemleri kullanarak istemci sertifikaları oluşturabilirsiniz:

- **Kurumsal sertifika:**

  - Kurumsal bir sertifika çözümü kullanıyorsanız, 'etkialaniadi\kullaniciadi' biçimini kullanmak yerine, yaygın olarak kullanılan 'name@yourdomain.com' ad değer biçimiyle bir istemci sertifikası oluşturun.
  - İstemci sertifikasının, kullanım listesindeki ilk öğe olarak Akıllı Kart Oturumu, vb. yerine ‘İstemci Kimlik Doğrulaması’na sahip ‘Kullanıcı’ sertifikası şablonunu temel alarak hazırlandığından emin olun. İstemci sertifikasına sağ tıklayıp *Ayrıntılar > Gelişmiş Anahtar Kullanımı*’nı görüntüleyerek sertifikayı denetleyebilirsiniz.

- **Otomatik olarak imzalanan kök sertifika**: [Noktadan Siteye bağlantılar için otomatik olarak imzalanan kök sertifika oluşturma](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md#clientcert) makalesindeki yönergeleri kullanarak bir otomatik olarak imzalanan kök sertifikadan istemci sertifikası oluşturursanız sertifika, sertifikayı oluşturmak için kullandığınız bilgisayara otomatik olarak yüklenir. İstemci sertifikasını başka bir istemci bilgisayara yüklemek istiyorsanız, sertifikayı dışarı aktarmanız gerekir. [Sertifikayı dışarı aktarmak](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md#clientexport) için makaledeki yönergeleri takip edin.