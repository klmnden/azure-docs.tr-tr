## <a name="microsoft-open-source-code-of-conduct"></a>Microsoft açık kaynak kullanım kuralları

Bu proje [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/) (Microsoft Açık Kaynak Kullanım Kuralları) belgesinde listelenen kurallara uygundur.
Daha fazla bilgi için [Kullanım Kuralları SSS](https://opensource.microsoft.com/codeofconduct/faq/) sayfasına bakın. Başka sorularınız ya da yorumlarınız varsa bunları [opencode@microsoft.com](mailto:opencode@microsoft.com) adresine gönderebilirsiniz.

## <a name="contribute-to-azure-technical-documentation"></a>Azure teknik belgelerine katkıda bulunun
Topluluklarımızdan (kullanıcılar, müşteriler, iş ortakları, çekirdek Azure ürün birimleri dışındaki MSFT çalışanları vb) ve çekirdek Azure ürün birimlerinde çalışanlardan gelen katkıları memnuniyetle karşılıyoruz. Nasıl katkıda bulunabileceğiniz üyesi olduğunuz topluluğa göre değişir:

* **Topluluk - küçük güncelleştirmeler**: tutarak güzelliklerine dışında küçük güncelleştirmeler katkısı olan, makaleyi bu depoda bulun, veya makaleyi ziyaret [https://docs.microsoft.com/azure](https://docs.microsoft.com/azure) ve'ı tıklatın **Düzenle** makale için GitHub kaynağına giden makalede bağlantı. Güncelleştirmelerinizi GitHub kullanıcı arabirimini kullanarak yapabilirsiniz. İsterseniz depo için çatal oluşturarak güncelleştirmeleri bu çataldan da gönderebilirsiniz.

* **Topluluk - yeni makaleler**: parçası değilseniz Azure topluluk ve yeni bir makale oluşturmak istiyorsanız, bu yeni bir ortak iş bir birleşimiyle içeriği ve özel havuz taşımanıza yardımcı olmak için bir çalışan ile çalışmak için gerekir.

* **Çalışanlar**: bir teknik yazar, program yöneticisi olduğunuz ya da bir Azure hizmeti ve bu ürün ekibinin Geliştirici işinizi katkıda veya teknik makaleleri yazmak için ise, özel deposu (https://github.com/ kullanmanız gerekir MicrosoftDocs/azure-docs-pr). Var olan bir makalede önemli değişiklikler yapacaksanız ekleme veya görüntüleri değiştirme veya yeni bir makale, bu depoyu çatallaştırmanız, Git Bash ile markdown Düzenleyicisi yüklemeniz ve bazı git komutlarını öğrenmeniz gerekir. Bkz: [iç katkıda bulunanlar Kılavuzu](https://review.docs.microsoft.com/en-us/help/contribute/?branch=master) daha fazla bilgi için.


## <a name="about-your-contributions-to-azure-content"></a>Azure içeriğine katkılarınız hakkında
### <a name="minor-corrections"></a>Küçük düzeltmeler
Yaptığınız küçük düzeltmeler veya açıklamalar, belge veya kod örnekleri bu depodaki kapsamındadır [docs.microsoft.com Kullanım Koşulları'nı](https://docs.microsoft.com/legal/termsofuse).

### <a name="larger-submissions"></a>Daha büyük gönderiler
Belge ve kod örneklerinde yeni veya önemli değişiklikler içeren bir çekme isteği gönderirseniz ve Microsoft çalışanı değilseniz GitHub'da size bir yorum göndererek çevrimiçi bir Katılım Lisans Sözleşmesi (CLA) göndermenizi isteyeceğiz. Çekme isteğinizi kabul edebilmemiz için bu çevrimiçi formu doldurmanız gerekir.

## <a name="tools-and-setup"></a>Araçlar ve Kurulum
Topluluk üyeleri GitHub kullanıcı arabirimini kullanarak veya depo için çatal oluşturarak katkıda bulunabilirler.  Çalışanların [Şirket İçinden Katkıda Bulunma Kılavuzuna](https://review.docs.microsoft.com/en-us/help/contribute/?branch=master) giderek teknik belge setine nasıl katkıda bulunabilecekleri konusunda daha fazla bilgi alması gerekir.

## <a name="repository-organization"></a>Depo düzeni
azure-docs deposundaki tüm içerik https://docs.microsoft.com/azure adresindeki belge düzenini takip eder. Bu depoda iki kök klasör bulunur: Bu depo iki kök klasör içerir:

### <a name="articles"></a>\articles
*\articles* klasörü *.md* uzantılı markdown dosyaları olarak biçimlendirilmiş belge makalelerini içerir. Bu makaleler, genellikle Azure hizmetine göre gruplandırılır.

*\articles* klasörü kök dizindeki makalelerin medya dosyaları için *\media* adlı bir klasör içerir. Bu klasörün içinde her makalenin resim dosyalarını içeren alt klasörler bulunur.  Hizmet klasörlerinde her bir hizmet klasöründeki makaleler için ayrı bir medya klasörü vardır.  Makale resim klasörleri, *.md* dosya uzantısı dışında makale dosyaları ile aynı ada sahiptir. 

### <a name="includes"></a>\includes
Bir veya daha fazla makaleye dahil etmek üzere oluşturduğunuz yeniden kullanılabilir içerik bölümleri burada bulunur. 

## <a name="how-to-use-markdown-to-format-your-topic"></a>Konunuzu biçimlendirmek için markdown kullanma
Bu depodaki tüm makaleler GitHub’a uygun markdown kullanır.  Gerekli bilgileri aşağıdaki kaynaklardan edinebilirsiniz.

* [Markdown temel bilgileri](https://help.github.com/articles/markdown-basics/)
* [Yazdırılabilir markdown başvuru sayfası](https://guides.github.com/pdfs/markdown-cheatsheet-online.pdf)


## <a name="labels"></a>Etiketler
Genel kullanıma açık azure-docs deposunda, çekme istekleri iş akışını yönetmemize yardımcı olması için çekme isteklerine otomatik etiketler atanır. Bu etiketler aynı zamanda çekme isteğinde bulunanlara isteklerinin durumu ile ilgili bilgi vermeye de olanak sağlar.

* Katılım Lisans Sözleşmesi (CLA) ile ilgili etiketler
  * cla değil gerekli: değişiklik görece küçük ve bir CLA oturum gerektirmez.
  * cla gerekli: değişikliğin kapsamını görece büyük ve bir CLA oturum gerektirir.
  * cla imzalı: çekme isteği şimdi İleri gözden geçirilmek üzere taşımak üzere katkıda bulunan CLA imzalanmış.
* Değişiklik yazmak için gönderilen: Yazar bekleyen çekme isteği bildirildi.
* hazır birleştirme: çekme isteği gözden geçirme ekibimiz tarafından gözden geçirme için hazır.


