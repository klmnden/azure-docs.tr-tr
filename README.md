# Azure Teknik Belgeler - Katkıda Bulunanlar Kılavuzu

[http://azure.microsoft.com/documentation](http://azure.microsoft.com/documentation) adresindeki Azure Belge Merkezi’nde yayımlanan teknik belgelere ilişkin kaynakları barındıran GitHub deposuna ulaştınız.

Bu depo, teknik belgelerimize katkıda bulunmanıza yardımcı olan yönergeleri de içerir.  Katkıda bulunanlar kılavuzundaki makalelerin listesi için [dizine](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-index.md) bakın.

## Azure belgelerine katkıda bulunun

Azure belgelerine gösterdiğiniz ilgi için teşekkür ederiz!

* [Katkıda bulunma yolları](#ways-to-contribute)
* [Kullanım kuralları](#code-of-conduct)
* [Azure içeriğine katkılarınız hakkında](#about-your-contributions-to-azure-content)
* [Depo düzeni](#repository-organization)
* [GitHub, Git ve bu depoyu kullanma](#use-github-git-and-this-repository)
* [Konunuzu biçimlendirmek için markdown kullanma](#how-to-use-markdown-to-format-your-topic)
* [Geri bildirim, yorumlar ve destek](./contributor-guide/feedback-and-comments.md)
* [Diğer kaynaklar](#more-resources)
* [atkıda bulunanlar kılavuzundaki tüm makalelerin dizini](./contributor-guide/contributor-guide-index.md) (Yeni bir sayfada açılır)

## Katkıda bulunma yolları 

[Azure belgelerine](http://azure.microsoft.com/documentation/) birkaç farklı şekilde katkıda bulunabilirsiniz:

* Bir [forum tartışmasına](http://social.msdn.microsoft.com/Forums/windowsazure/home) katkıda bulunun.
* Makalelerin altında Disqus yorumları gönderin.
* GitHub kullanıcı arabiriminde teknik makalelere kolayca katkıda bulunabilirsiniz. Makaleyi bu depoda bulun veya [http://azure.microsoft.com/documentation](http://azure.microsoft.com/documentation) adresinde ilgili makaleye giderek makalenin GitHub kaynağına giden bağlantıya tıklayın.
* Mevcut bir makalede önemli değişiklikler yapıyor, resim ekliyor ya da değiştiriyor veya yeni bir makale ekliyorsanız bu depoyu çatallaştırmanız, Git Bash ile Markdown Pad’i yüklemeniz ve bazı git komutlarını öğrenmeniz gerekir.

##Kullanım kuralları

Bu proje [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/) (Microsoft Açık Kaynak Kullanım Kuralları) belgesinde listelenen kurallara uygundur. Daha fazla bilgi için [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) (Kullanım Kuralları Hakkında SSS) konusuna bakın veya sorularınızı ya da görüşlerinizi bildirmek için [opencode@microsoft.com](mailto:opencode@microsoft.com) adresinden bize ulaşın.

##Azure içeriğine katkılarınız hakkında

###Küçük düzeltmeler

Bu depodaki belge veya kod örneklerinde yaptığınız küçük düzeltmeler veya açıklamalar [Azure Web Sitesi Kullanım Koşulları (ToU)](http://azure.microsoft.com/support/legal/website-terms-of-use/) kapsamındadır.


###Daha büyük gönderiler

Belge ve kod örneklerinde yeni veya önemli değişiklikler içeren bir çekme isteği gönderdiğinizde, aşağıdaki gruplardan birindeyseniz GitHub’da size bir yorum göndererek çevrimiçi bir Katkı Lisansı Sözleşmesi (CLA) göndermenizi isteriz:

* Microsoft Open Technologies grubunun üyeleri.
* Microsoft çalışanı olmayan katkıda bulunanlar.

Çekme isteğiniz kabul edilmeden önce çevrimiçi formu doldurmanız gerekir.

İlgili tüm ayrıntıları [http://azure.github.io/guidelines/#cla](http://azure.github.io/guidelines/#cla) adresinde bulabilirsiniz.

## Depo düzeni

Azure-content deposundaki içerik, [Azure.Microsoft.com](http://azure.microsoft.com) üzerindeki belgelerin düzenini izler. Bu depo iki kök klasör içerir:

### \articles

*\articles* klasörü *.md* uzantılı markdown dosyaları olarak biçimlendirilmiş belge makalelerini içerir.

Kök dizindeki makaleler *http://azure.microsoft.com/documentation/articles/{md-olmadan-makale-adı}/* yolunda Azure.Microsoft.com’da yayımlanır.

* **Makale dosya adları:** [Dosya adlandırma kılavuzumuza](./contributor-guide/file-names-and-locations.md) bakın.

Belirli bir hizmet klasöründeki makaleler *http://azure.microsoft.com/documentation/articles/service-folder/{md-olmadan-makale-adı}/* yolunda Azure.Microsoft.com’da yayımlanır.

* **Medya alt klasörleri:** *\articles* klasörü, kök dizindeki makalelere ait medya dosyalarının tutulduğu *\media* klasörünü içerir. Bu klasörün içinde her makaleye ilişkin resimlerin yer aldığı alt klasörler bulunur.  Hizmet klasörleri, her bir hizmet klasörünün içindeki makaleler için ayrı bir medya klasörü içerir. Makale resmi klasörleri, *.md* dosya uzantısı dışında makale dosyasıyla aynı ada sahiptir.

### \includes

Bir veya daha fazla makaleye dahil edilecek yeniden kullanılabilir içerik bölümleri oluşturabilirsiniz. Bkz. [Teknik içeriklerimizde kullanılan özel uzantılar](./contributor-guide/custom-markdown-extensions.md).

### \markdown templates

Bu klasör, makaleler için gereken temel markdown biçimlendirmesine sahip standart markdown şablonumuzu içerir.

### \contributor-guide

Bu klasör, katkıda bulunanlar kılavuzumuzun parçası olan makaleleri içerir.  

## GitHub, Git ve bu depoyu kullanma

Katkıda bulunma, küçük değişiklikler yapmak için GitHub kullanıcı arabirimini kullanma, daha önemli değişiklikler yapmak üzere depoyu çatallaştırma ve kopyalama hakkında bilgi almak için bkz. [Install and set up tools for authoring in GitHub](./contributor-guide/tools-and-setup.md) (GitHub’da yazma araçlarını yükleme ve kurma).

Git Bash’i yükler ve yerel olarak çalışmayı seçerseniz, yeni bir yerel çalışma dalı oluşturmak, değişiklikler yapmak ve değişiklikleri ana dala geri göndermek için kullanılan adımlar [Git commands for creating a new article or updating an existing article](./contributor-guide/git-commands-for-master.md) (Yeni bir makale oluşturmak veya mevcut bir makaleyi güncelleştirmek için kullanılan Git komutları) bölümünde listelenmiştir.

### Dallar

Belirli bir değişiklik kapsamını hedefleyen yerel çalışma dalları oluşturmanızı öneririz. İş akışını kolaylaştırmak ve birleştirme sırasında ortaya çıkabilecek çakışmaları azaltmak için her dalın tek bir kavramla/makaleyle sınırlandırılmasını öneririz.  Aşağıdaki çalışmalar yeni bir dal için uygun kapsama sahiptir:

* Yeni bir makale (ve ilişkili resimler)
* Bir makale üzerinde yazım ve dil bilgisi düzenlemeleri.
* Büyük bir makale kümesine bir biçimlendirme değişikliği uygulama (örneğin yeni bir telif hakkı alt bilgisi).

## Konunuzu biçimlendirmek için markdown kullanma

Bu depodaki tüm makaleler GitHub’a uygun markdown kullanır.  Kaynakların listesi aşağıda verilmiştir.

- [Markdown temelleri](https://help.github.com/articles/markdown-basics/)

- [Yazdırılabilir markdown başvuru sayfası](./contributor-guide/media/documents/markdown-cheatsheet.pdf?raw=true)

- Markdown düzenleyicilerimizin listesi için [araçlar ve kurulum konusuna](./contributor-guide/tools-and-setup.md#install-a-markdown-editor) bakın.

## Makale meta verileri

Makale meta verileri, azure.microsoft.com web sitesinde yazar bilgisi, katkıda bulunan bilgisi, içerik haritaları, makale açıklamaları ve SEO iyileştirmelerinin yanı sıra Microsoft’un içerik performansını değerlendirmek için kullandığı raporlama gibi belirli işlevlerin kullanılmasını sağlar. Bu nedenle, meta veriler önemlidir! [Meta verilerinizin doğru biçimlendirildiğinden emin olmak için kullanabileceğiniz kaynakları burada bulabilirsiniz](./contributor-guide/article-metadata.md).

## Diğer kaynaklar

Kılavuzdaki tüm konular için [katkıda bulunanlar kılavuzu dizinimize](./contributor-guide/contributor-guide-index.md) bakın.



<!--HONumber=Aug16_HO1-->


