## <a name="microsoft-open-source-code-of-conduct"></a>Microsoft açık kaynak kullanım kuralları

Bu proje [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/) (Microsoft Açık Kaynak Kullanım Kuralları) belgesinde listelenen kurallara uygundur.
Daha fazla bilgi için [kod kuralları hakkında SSS](https://opensource.microsoft.com/codeofconduct/faq/)'e veya [ opencode@microsoft.com ](mailto:opencode@microsoft.com) mail adresine başvurabilir, sorularınızı ya da görüşlerinizi gönderebilirsiniz.

## <a name="contribute-to-azure-technical-documentation"></a>Azure teknik belgelerine katkıda bulunun
Azure Topluluklarımızın (kullanıcılar, müşteriler, iş ortakları, çekirdek Azure Ürün Birimleri haricindeki MSFT çalışanları) ve çekirdek Azure Ürün Birimlerinde çalışanların katkılarını kabul ediyoruz. Kim olduğunuza bağlı olarak nasıl katkıda bulunabileceğiniz değişiklik gösterecektir. 

* **Topluluk - küçük güncelleştirmeler**: kalbinizin güzelliğinden dolayı ufak katkılarda bulunmak istiyorsanız makaleyi bu depoda bulabilir veya makaleyi sitesinde ziyaret edip [https://docs.microsoft.com/azure](https://docs.microsoft.com/azure) **Düzenle** düğmesine tıklayarak GitHub'daki yerine ulaşabilirsiniz. Ardından, katkıda bulunmak için yalnızca GitHub kullanıcı arabirimini kullanabileceğiniz gibi isterseniz depoyu forklayarak değişiklikleri çekme isteği olarak da gönderebilirsiniz. 

* **Topluluk - yeni makaleler**: Eğer yeni bir makale oluşturmak istiyorsanız ve Azure Topluluğumuzun bir üyesiyseniz bir çalışan ile beraber ilerleyerek dışarıya açık ve kapalı depolarda çalışmalar yapmanız gerekecektir. 

* **Çalışanlar**: Bir Azure hizmetinin ürün takımında çalışan teknik yazar, program yöneticisi veya yazılım geliştiriciyseniz ve işiniz bu depoya katkıda bulunmak veya teknik yazı yazmaksa çalışmanızı özel depoda yapmalısınız ([https://github.com/MicrosoftDocs/azure-docs-pr](https://github.com/MicrosoftDocs/azure-docs-pr)). Var olan bir makalede önemli değişiklikler yapacaksanız, resimler ekleyip, değiştirecek, yeni makale katkısında bulunacaksanız bu depoyu forklamalı, Git Bash yüklemeli, bir markdown editörü edinmeli ve git komutları öğrenmelisiniz. Detaylar için [İç Katkıda bulunanlar Kılavuzu](https://review.docs.microsoft.com/en-us/help/contribute/?branch=master)'na göz atabilirsiniz.

## <a name="about-your-contributions-to-azure-content"></a>Azure içeriğine katkılarınız hakkında
### <a name="minor-corrections"></a>Küçük düzeltmeler
Bu depodaki belge veya kod örnekleri için yaptığınız küçük düzeltmeler veya açıklamalar [docs.microsoft.com Kullanım Koşulları](https://docs.microsoft.com/legal/termsofuse) kapsamındadır.

### <a name="larger-submissions"></a>Daha büyük gönderiler
Belge ve kod örneklerinde yeni veya önemli değişiklikler içeren bir çekme isteği gönderirseniz ve Microsoft çalışanı değilseniz GitHub'da yorum göndererek sizden Çevrimiçi Katkı Lisansı Sözleşmesi (CLA) göndermenizi rica edeceğiz. Çekme isteğiniz kabul edilmeden önce çevrimiçi formu doldurmanız gerekir.

## <a name="tools-and-setup"></a>Araçlar ve Kurulum
Topluluğa katkıda bulunanlar GitHub kullanıcı arabirimini kullanabilir veya depoyu forklayarak ilerleyebilirler. Çalışanlar [İç Katkıda Bulunanlar Kılavuzu](https://review.docs.microsoft.com/en-us/help/contribute/?branch=master)'na danışarak daha fazla bilgi edinebilirler.

## <a name="repository-organization"></a>Depo düzeni
azure-docs.tr-tr deposundaki tüm içerik https://docs.microsoft.com/azure adresine belge düzenini takip eder. Bu depoda iki ana klasör bulunur:

### <a name="articles"></a>\articles
*\articles* klasörü *.md* uzantılı markdown dosyaları olarak biçimlendirilmiş belgeleri içerir. Bu belgeler, genellikle Azure hizmetleri çerçevesinde gruplandırılır.

*\Articles* klasörü *\media* adında makale medyaları için bir kök dizin içerir. Bu klasör içerisinde her makale için medyaları içeren ayrı alt klasörler bulunur. Makale resmi klasörleri, *.md* dosya uzantılı makale dosyalarının adları ile aynı ada sahiptir. 

### <a name="includes"></a>\includes
Bir veya daha fazla makaleye dahil edilecek yeniden kullanılabilir içerik bölümlerini bu klasörde oluşturabilirsiniz. 

## <a name="how-to-use-markdown-to-format-your-topic"></a>Konunuzu biçimlendirmek için Markdown kullanma
Bu depodaki tüm makaleler GitHub’a uygun markdown kullanır. Gerekli bilgileri aşağıdaki kaynaklardan edinebilirsiniz. 

* [Markdown temel bilgileri](https://help.github.com/articles/markdown-basics/)
* [Yazdırılabilir markdown ipucu sayfası](./contributor-guide/media/documents/markdown-cheatsheet.pdf?raw=true)

## <a name="labels"></a>Etiketler
Açık azure-docs.tr-tr deposundaki çekme istekleri otomatik etiketler atanarak yönetilir. Bu etiketler aynı anda çekme isteği sahibine de çekme isteğinin mevcut durumu ile ilgili bilgi verme amacını taşır. 

* Katkı Lisansı Sözleşmesi (CLA) konusunda;
  * cla-not-required: değişiklik görece küçük olduğu için CLA gerektirmez.
  * cla-required: değişikliğin kapsamı görece büyük olduğu için bir CLA imzalanması gerekir.
  * cla-signed: Hali hazırda imzalı CLA bulunduğu için çekme isteği değerlendirmeye alınabilir. 
* Change sent to author: Yazar bekleyen çekme isteği hakkında bildirildi.
* ready-to-merge: çekme isteği gözden geçirme ekibimiz tarafından gözden geçirme için hazır.


