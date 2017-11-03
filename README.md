## <a name="microsoft-open-source-code-of-conduct"></a>Microsoft açık kaynak kullanım kuralları

Bu proje [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/) (Microsoft Açık Kaynak Kullanım Kuralları) belgesinde listelenen kurallara uygundur.
Daha fazla bilgi için bkz: [kod kuralları hakkında SSS](https://opensource.microsoft.com/codeofconduct/faq/) veya başvurun [ opencode@microsoft.com ](mailto:opencode@microsoft.com) sorularınızı ya da görüşlerinizi.

## <a name="contribute-to-azure-technical-documentation"></a>Azure teknik belgelerine katkıda bulunun
Biz topluluğumuz (kullanıcılar, müşteriler, iş ortakları, çekirdek Azure ürün birimleri, vb. dışında MSFT çalışanlar) yanı sıra çekirdek Azure ürün birimlerinde çalışanların Katkıları Hoş Geldiniz. Nasıl katkıda kimin size bağlıdır:

* **Topluluk - küçük güncelleştirmeler**: tutarak güzelliklerine dışında küçük güncelleştirmeler katkısı olan, makaleyi bu depoda bulun, veya makaleyi ziyaret [https://docs.microsoft.com/azure](https://docs.microsoft.com/azure) ve'ı tıklatın **Düzenle** makale için GitHub kaynağına giden makalede bağlantı. Ardından, güncelleştirmelerinin olması için yalnızca GitHub kullanıcı arabirimini kullanın. Veya çatalı depo Hoş Geldiniz ve güncelleştirmeler, çatalı gönderin.

* **Topluluk - yeni makaleler**: parçası değilseniz Azure topluluk ve yeni bir makale oluşturmak istiyorsanız, bu yeni bir ortak iş bir birleşimiyle içeriği ve özel havuz taşımanıza yardımcı olmak için bir çalışan ile çalışmak için gerekir.

* **Çalışanlar**: bir teknik yazar, program yöneticisi olduğunuz ya da bir Azure hizmeti ve bu ürün ekibinin Geliştirici işinizi katkıda veya teknik makaleleri yazmak için ise, özel deposu (https://github.com/ kullanmanız gerekir MicrosoftDocs/azure-docs-pr). Var olan bir makalede önemli değişiklikler yapacaksanız ekleme veya görüntüleri değiştirme veya yeni bir makale, bu depoyu çatallaştırmanız, Git Bash ile markdown Düzenleyicisi yüklemeniz ve bazı git komutlarını öğrenmeniz gerekir. Bkz: [iç katkıda bulunanlar Kılavuzu](https://review.docs.microsoft.com/en-us/help/contribute/?branch=master) daha fazla bilgi için.


## <a name="about-your-contributions-to-azure-content"></a>Azure içeriğine katkılarınız hakkında
### <a name="minor-corrections"></a>Küçük düzeltmeler
Yaptığınız küçük düzeltmeler veya açıklamalar, belge veya kod örnekleri bu depodaki kapsamındadır [docs.microsoft.com Kullanım Koşulları'nı](https://docs.microsoft.com/legal/termsofuse).

### <a name="larger-submissions"></a>Daha büyük gönderiler
Belge ve kod örneklerinde yeni veya önemli değişiklikler içeren bir çekme isteği gönderirseniz, yorum Github'da çevrimiçi bir katkı lisansı Sözleşmesi (CLA) bir çalışan Microsoft değilse gönderme isteyen göndereceğiz. Çekme isteğiniz kabul edilmeden önce çevrimiçi formu doldurmanız gerekir.

## <a name="tools-and-setup"></a>Araçlar ve Kurulum
Topluluğa katkıda bulunanlar GitHub kullanıcı arabirimini veya katkıda depoyu çatallaştırma kullanabilirsiniz. Çalışanlar ziyaret [iç katkıda bulunanlar Kılavuzu](https://review.docs.microsoft.com/en-us/help/contribute/?branch=master) teknik belgelerine katkıda bulunma hakkında daha fazla bilgi için ayarlayın.

## <a name="repository-organization"></a>Depo düzeni
Azure belgeleri deposundaki içerik üzerinde https://docs.microsoft.com/azure belgeleri izler. Bu depo iki kök klasör içerir:

### <a name="articles"></a>\articles
*\articles* klasörü *.md* uzantılı markdown dosyaları olarak biçimlendirilmiş belge makalelerini içerir. Makaleler, genellikle Azure hizmeti tarafından gruplandırılır.

*\Articles* klasörde *\media* klasöründe için kök dizini makale medyası dosyaları, hangi içinde her makaleye ilişkin resimlerin alır.  Hizmet klasörleri, her bir hizmet klasörünün içindeki makaleler için ayrı bir medya klasörü içerir. Makale resmi klasörleri, *.md* dosya uzantısı dışında makale dosyasıyla aynı ada sahiptir.

### <a name="includes"></a>\includes
Bir veya daha fazla makaleye dahil edilecek yeniden kullanılabilir içerik bölümleri oluşturabilirsiniz. 

## <a name="how-to-use-markdown-to-format-your-topic"></a>Konunuzu biçimlendirmek için markdown kullanma
Bu depodaki tüm makaleler GitHub’a uygun markdown kullanır.  Kaynakların listesi aşağıda verilmiştir.

* [Markdown temel bilgileri](https://help.github.com/articles/markdown-basics/)
* [Yazdırılabilir markdown başvuru sayfası](./contributor-guide/media/documents/markdown-cheatsheet.pdf?raw=true)


## <a name="labels"></a>Etiketler
Ortak azure belgeleri deposunda istekleri çekme isteği iş akışı yönetmenize yardımcı olmak için ve çekme isteğinize neler olduğunu size bildirmek amacıyla çıkarmak için otomatik etiketleri atanır:

* İlgili katkı lisansı Sözleşmesi
  * cla değil gerekli: değişiklik görece küçük ve bir CLA oturum gerektirmez.
  * cla gerekli: değişikliğin kapsamını görece büyük ve bir CLA oturum gerektirir.
  * cla imzalı: çekme isteği şimdi İleri gözden geçirilmek üzere taşımak üzere katkıda bulunan CLA imzalanmış.
* Değişiklik yazmak için gönderilen: Yazar bekleyen çekme isteği bildirildi.
* hazır birleştirme: çekme isteği gözden geçirme ekibimiz tarafından gözden geçirme için hazır.


