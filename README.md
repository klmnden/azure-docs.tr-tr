## <a name="microsoft-open-source-code-of-conduct"></a>Microsoft açık kaynak kullanım kuralları

Bu proje [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/) (Microsoft Açık Kaynak Kullanım Kuralları) belgesinde listelenen kurallara uygundur.

Daha fazla bilgi için [Kullanım Kuralları SSS](https://opensource.microsoft.com/codeofconduct/faq/) sayfasına bakın. Başka sorularınız ya da yorumlarınız varsa bunları [opencode@microsoft.com](mailto:opencode@microsoft.com) adresine gönderebilirsiniz.

## <a name="contribute-to-azure-technical-documentation"></a>Azure teknik belgelerine katkıda bulunun
Topluluklarımızdan (kullanıcılar, müşteriler, iş ortakları, çekirdek Azure ürün birimleri dışındaki MSFT çalışanları vb) ve çekirdek Azure ürün birimlerinde çalışanlardan gelen katkıları memnuniyetle karşılıyoruz. Nasıl katkıda bulunabileceğiniz üyesi olduğunuz topluluğa göre değişir:

* **Topluluk - küçük güncelleştirmeler**: kalbinizin güzelliğinden dolayı ufak katkılarda bulunmak istiyorsanız makaleyi bu depoda bulabilir veya makaleyi sitesinde ziyaret edip [https://docs.microsoft.com/azure](https://docs.microsoft.com/azure) **Düzenle** düğmesine tıklayarak GitHub'daki yerine ulaşabilirsiniz. Ardından, katkıda bulunmak için yalnızca GitHub kullanıcı arabirimini kullanabileceğiniz gibi isterseniz depoyu forklayarak değişiklikleri çekme isteği olarak da gönderebilirsiniz. 


* **Topluluk - yeni makaleler**: Eğer yeni bir makale oluşturmak istiyorsanız ve Azure Topluluğumuzun bir üyesiyseniz bir çalışan ile beraber ilerleyerek dışarıya açık ve kapalı depolarda çalışmalar yapmanız gerekecektir. 

* **Çalışanlar**: Bir Azure hizmetinin ürün takımında çalışan teknik yazar, program yöneticisi veya yazılım geliştiriciyseniz ve işiniz bu depoya katkıda bulunmak veya teknik yazı yazmaksa çalışmanızı özel depoda yapmalısınız ([https://github.com/MicrosoftDocs/azure-docs-pr](https://github.com/MicrosoftDocs/azure-docs-pr)). Var olan bir makalede önemli değişiklikler yapacaksanız, resimler ekleyip, değiştirecek, yeni makale katkısında bulunacaksanız bu depoyu forklamalı, Git Bash yüklemeli, bir markdown editörü edinmeli ve git komutları öğrenmelisiniz. Detaylar için [İç Katkıda bulunanlar Kılavuzu](https://review.docs.microsoft.com/en-us/help/contribute/?branch=master)'na göz atabilirsiniz.

## <a name="about-your-contributions-to-azure-content"></a>Azure içeriğine katkılarınız hakkında
### <a name="minor-corrections"></a>Küçük düzeltmeler
Bu depodaki belge veya kod örnekleri için yaptığınız küçük düzeltmeler veya açıklamalar [docs.microsoft.com Kullanım Koşulları](https://docs.microsoft.com/legal/termsofuse) kapsamındadır.

### <a name="larger-submissions"></a>Daha büyük gönderiler

Belge ve kod örneklerinde yeni veya önemli değişiklikler içeren bir çekme isteği gönderirseniz ve Microsoft çalışanı değilseniz GitHub'da size bir yorum göndererek çevrimiçi bir Katılım Lisans Sözleşmesi (CLA) göndermenizi isteyeceğiz. Çekme isteğinizi kabul edebilmemiz için bu çevrimiçi formu doldurmanız gerekir.

## <a name="tools-and-setup"></a>Araçlar ve Kurulum
Topluluk üyeleri GitHub kullanıcı arabirimini kullanarak veya depo için çatal oluşturarak katkıda bulunabilirler.  Çalışanların [Şirket İçinden Katkıda Bulunma Kılavuzuna](https://review.docs.microsoft.com/en-us/help/contribute/?branch=master) giderek teknik belge setine nasıl katkıda bulunabilecekleri konusunda daha fazla bilgi alması gerekir.

## <a name="repository-organization"></a>Depo düzeni
azure-docs deposundaki tüm içerik https://docs.microsoft.com/azure adresindeki belge düzenini takip eder. Bu depoda iki kök klasör bulunur: 

### <a name="articles"></a>\articles
*\articles* klasörü *.md* uzantılı markdown dosyaları olarak biçimlendirilmiş belge makaleleri içerir. Bu makaleler, genellikle Azure hizmetine göre gruplandırılır.

*\articles* klasörü kök dizindeki makalelerin medya dosyaları için *\media* adlı bir klasör içerir. Bu klasörün içinde her makalenin resim dosyalarını içeren alt klasörler bulunur.  Hizmet klasörlerinde her bir hizmet klasöründeki makaleler için ayrı bir medya klasörü vardır.  Makale resim klasörleri, *.md* dosya uzantısı dışında makale dosyaları ile aynı ada sahiptir. 

### <a name="includes"></a>\includes
Bir veya daha fazla makaleye dahil etmek üzere oluşturduğunuz yeniden kullanılabilir içerik bölümleri burada bulunur. 

## <a name="how-to-use-markdown-to-format-your-topic"></a>Konunuzu biçimlendirmek için markdown kullanma
Bu depodaki tüm makaleler GitHub’a uygun markdown kullanır.  Gerekli bilgileri aşağıdaki kaynaklardan edinebilirsiniz.

* [Markdown temel bilgileri](https://help.github.com/articles/markdown-basics/)
* [Yazdırılabilir markdown ipucu sayfası](./contributor-guide/media/documents/markdown-cheatsheet.pdf?raw=true)

## <a name="labels"></a>Etiketler

Genel kullanıma açık azure-docs.tr-tr deposunda, çekme istekleri iş akışını yönetmemize yardımcı olması için çekme isteklerine otomatik etiketler atanır. Bu etiketler aynı zamanda çekme isteğinde bulunanlara isteklerinin durumu ile ilgili bilgi vermeye de olanak sağlar.

* Katılım Lisans Sözleşmesi (CLA) ile ilgili etiketler
  * cla-not-required: değişiklik görece küçük olduğu için CLA gerektirmez.
  * cla-required: değişikliğin kapsamı görece büyük olduğu için bir CLA imzalanması gerekir.
  * cla-signed: Hali hazırda imzalı CLA bulunduğu için çekme isteği değerlendirmeye alınabilir. 
* Change sent to author: Yazar bekleyen çekme isteği hakkında bildirildi.
* ready-to-merge: çekme isteği gözden geçirme ekibimiz tarafından gözden geçirme için hazır.



