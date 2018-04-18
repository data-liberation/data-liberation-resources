https://github.com/dannguyen/abbyy-finereader-ocr-senate




# 利用 ABBYY FineReader 提取 US 参议员个人财务报告中的表格数据

美国要求国会议员必须要提交常规的报告来公示个人财产。尽管有
[电子化的提交系统](http://www.rollcall.com/moneyline/senate-enters-electronic-age-with-personal-wealth-disclosures/), 一些议员仍然是提交纸质文件，然后这些文件被扫描以后，以pdf或图片形式上传到 ([Senate](https://efdsearch.senate.gov/search/home/) / [House](http://clerk.house.gov/public_disc/financial-search.aspx))的数据库里.

参议院的电子化提交系统已经[上线了一些年头](http://www.rollcall.com/moneyline/senate-enters-electronic-age-with-personal-wealth-disclosures/); [Senator Bernie Sanders](https://www.opensecrets.org/pfds/summary.php?year=2014&cid=N00000528)就是其中一个一开始提交纸质档后来又使用系统的人:

![Sen. Bernie Sanders annual disclosures, 2011 and 2014](images/sen-bernie-sanders-2014-forms.jpg)

要从扫描件的图片里提取数据是最常见最困难的数据整理工作，以至于 OpenSecrets (aka The Center for Responsive Politics) [发起了创建一个高效的解析国会成员个人财务披露数据解决方案的比赛](https://github.com/pdfliberation/pdf-hackathon/blob/master/challenges/house-financial-disclosures.md#2nd-option---handwritten-reports).

本文旨在给大家展示一下使用 [MAC版的ABBYY FineReader](http://www.abbyy.com/finereader/pro-for-mac/)从扫描版的披露文件中得到可用的数据是多么有用。注意我本意不是想说要把不完全准确的OCR结果清理好并导入到数据库中，以及如何将其自动化成批量处理的过程。仅仅是为了半自动化的从一个扫描文件中提取文字本身就是很难的一件事。


要想更多的了解 PDF 和结构化数据，包括不同类型的 PDF，从诸多PDF中提取结构化数据需要面临的挑战和采取的方法，请阅读[Jacob Fenton 和 Jeremy Singer-Vine 在 NICAR16 上做的题为  Parsing Prickly PDFs的演讲](https://github.com/jsfenfen/parsing-prickly-pdfs). 如果你只是关注某个国会议员的财务数据， [OpenSecrets 已经帮你搞定了](https://www.opensecrets.org/pfds/).

另外, [Robert Gebeloff of the New York Times](https://github.com/gebelo) 在NICAR的演讲里也[整理了很多其他商业化软件和应用场景 (.docx)](https://github.com/gebelo/nicar2016/raw/master/pdf_wrangling16.docx)


__第一点:__ FineReader 非常适合此类任务; 后面我会详尽阐述如何应用到处理多种表单的半自动化处理中去 (或者其他任意纸质文件的扫描件 ). 

简要起见，本文主要是针对 [议员财务披露文件](https://efdsearch.senate.gov/search/home/) -与两院的OCR 难题基本上是一样的

## 议员提交的财务披露文件长什么样子

[议员财务披露数据库](https://efdsearch.senate.gov/search/home/):

https://efdsearch.senate.gov/search/home/

要访问上面的链接，需要在浏览器里手动同意使用说明。这样子会另外打开一个tab页来浏览网站。

###  以电子档形式提交的个人财务披露报告 

 [Senator Marco Rubio](https://www.opensecrets.org/pfds/reports.php?year=2014&cid=N00030612)2014年提交的数据:

https://efdsearch.senate.gov/search/view/annual/de85e0d9-7eeb-49b6-83df-67affd2df645/

为了方便查看，我搞了一份[  HTML版本的](http://so.danwin.com/senate-ocr/2014-rubio-report.html) ，不需要去参议院的网站上看。

<a href="http://so.danwin.com/senate-ocr/2014-rubio-report.html"><img src="images/rubio-table.png" alt="rubio-table.png"></a>

如你所见，HTML 要提取成结构化数据很容易。As you can see, the HTML is straightforward to parse as machine-readable data. So let's dispel once and for all with this fiction that Senator Rubio doesn't know what he's doing. He knows __exactly__ what he's doing.

### 以纸质文件形式提交的个人财务披露报告 

还是上面那一份报告，议员[Senator Dianne Feinstein](https://www.opensecrets.org/pfds/reports.php?cid=N00007364)当时提交的是纸质文件:

https://efdsearch.senate.gov/search/view/paper/B06D0983-3786-41CB-92C6-5209F288D517/

[OpenSecrets has a copy of the PDF](http://pfds.opensecrets.org/N00007364_2014.pdf)上可以浏览，扫描件如下所示：

<a href="http://pfds.opensecrets.org/N00007364_2014.pdf">
  <img src="images/feinstein-000001065.gif" alt="">
</a>

#  OCR 难题

尽管电子档易于处理，但要把这些数据导入到数据库里，创建数据结构的难度也是有的。

对纸质文件来说，这个问题同样存在，额外还有一个就是怎么样提取到数据。这就是[__OCR 技术的用武之地__](https://en.wikipedia.org/wiki/Optical_character_recognition), aka __OCR__. 

我们期望达到的效果是:

###### 1. 把扫描件转换成纯文本数据

也就是把图片形式的![](https://raw.githubusercontent.com/dannguyen/abbyy-finereader-ocr-senate/master/images/a.jpg) 转换成文本编辑器可以读取的文本形式: `a`


###### 2.把扫描件中的表格数据转换成 Excel 中的表格



也就是把图片形式的:

![](https://raw.githubusercontent.com/dannguyen/abbyy-finereader-ocr-senate/master/images/pension-table.jpg)

转成 Excel：

|                                |                   |         |         |
|--------------------------------|-------------------|---------|---------|
| City & County of San Francisco | San Francisco, CA | Pension | $56,804 |


把图片变成文本的过程是极其复杂的，除了 [Google以外](https://cloud.google.com/vision/).其他团队想要达到一个非常高的准确性是难以做到的. 另外图片形式的表格数据本身又是一大难题。


## 使用 ABBYY FineReader

有一些开源的OCR工具，比如 [Tesseract 是最广为人知的](https://github.com/tesseract-ocr),但基本上都不能很好的处理表格数据 (注意: [诸如Tabula](http://tabula.technology/)能够处理表格数据，但又搞不定扫描件的图片).  

商业化软件如 [ABBYY's FineReader](http://www.abbyy.com/finereader/) 和 [OmniPage](http://www.nuance.com/for-business/by-product/omnipage/ultimate/index.htm) -- 都号称能够有效的处理OCR类型的表格数据。
  

mac版的[FineReader Pro for Mac](http://www.abbyy.com/finereader/pro-for-mac/)标价$119. Windows 可以使用 [FineReader 12 Professional](http://www.abbyy.com/finereader/professional/) 和 [Corporate](http://www.abbyy.com/finereader/corporate/) .

### ABBYY Cloud?

在[OpenSecrets PDF hackathon](https://github.com/pdfliberation/pdf-hackathon/blob/master/challenges/house-financial-disclosures.md#2nd-option---handwritten-reports)期间, developer Ross Tsiomenko [测试了使用ABBYY云服务](http://ocrsdk.com/) 来批量处理[国会议员的财务报表](https://www.opengovfoundation.org/developers-blog-liberating-congressional-financial-disclosure-data-from-pdfs/):

> 国会议员的财务报表不是纯文本的pdf，而是扫描件的图片，需要使用OCR软件来提取数据。 ABBYY 云 OCR 是目前唯一可以准确提取表格数据的软件。使用shell上传pdf到云API，然后拿到一个文本，其中包含了列数据行数据。然后使用python清洗整理成csv文件。

> ...验证了这条路可行，就需要找到使用付费的ABBYY Cloud OCR服务的替代方案。ABBYY的话处理一年的报表，大概需要一万页请求，折合成900美元。

我没用过云服务，价格看起来还挺合理。本文里我用的桌面程序 ，我理解不论是windows还是mac版都能达到相当的OCR效果。随后主要是给大家讲讲如何使用桌面版来批量处理OCR。





## 简单的表格

我们对比看一下 FineReader OCR 和 Tesseract OCR 在处理扫描件的效果[ later in this writeup](#finereader-vs-tesseract).

如下是财务披露文件中比较简单的一种：

![000602483.gif](https://raw.githubusercontent.com/dannguyen/abbyy-finereader-ocr-senate/master/images/000602483.gif)

注意: 本文中的例子都来自 [Senator Lamar Alexander's](https://www.opensecrets.org/pfds/summary.php?cid=N00009888) 2011 Annual Report (mirrored [here](http://pfds.opensecrets.org/N00009888_2011.pdf) at OpenSecrets). [电子系统才上线了几年而已](http://www.rollcall.com/moneyline/senate-enters-electronic-age-with-personal-wealth-disclosures/) ， Senator Alexander's [上一次提交的是电子档](https://efdsearch.senate.gov/search/view/annual/88f4fc2c-e599-44e9-a704-fbebd05f4207/), 给他点个赞.

###  FineReader 的效果

当把上面图片导入到FineReader，结果如下所示：


![000602483-excel-abbyy-preview.jpg](https://raw.githubusercontent.com/dannguyen/abbyy-finereader-ocr-senate/master/images/000602483-excel-abbyy-preview.jpg)

### 得到的 Excel spreadsheet

FineReader 可以把图片导出成PDF. 但我们想要的是Excel

结果如下所示:

![000602483-excel.jpg](https://raw.githubusercontent.com/dannguyen/abbyy-finereader-ocr-senate/master/images/000602483-excel.jpg)


 [Excel 文件](https://github.com/dannguyen/abbyy-finereader-ocr-senate/blob/master/xls/000602483.xlsx).  [转换成的PDF文件](https://github.com/dannguyen/abbyy-finereader-ocr-senate/blob/master/pdf/000602483.pdf).

## 稍微复杂的表格


![000602474.gif](https://raw.githubusercontent.com/dannguyen/abbyy-finereader-ocr-senate/master/images/000602474.gif)


FineReader's Excel 效果:

![000602474-excel.jpg](https://raw.githubusercontent.com/dannguyen/abbyy-finereader-ocr-senate/master/images/000602474-excel.jpg)

效果没有前面的好，但老实说，比我想象的要好很多。尤其是对竖着的表头的处理效果。但要把这些数据导入到数据库里还有大量的事情要做。


[Excel spreadsheet](https://github.com/dannguyen/abbyy-finereader-ocr-senate/blob/master/xls/000602474.xlsx). And here's the [PDF](https://github.com/dannguyen/abbyy-finereader-ocr-senate/blob/master/pdf/000602474.pdf) 


## 带复选框的例子 


![000602470.gif](https://github.com/dannguyen/abbyy-finereader-ocr-senate/blob/master/images/000602470.gif)


这个就很复杂了，框里面有框。而且有很多手写的

FineReader 的结果：

![](https://github.com/dannguyen/abbyy-finereader-ocr-senate/blob/master/images/000602470-excel.jpg)

看起来基本没法用，主要不是字错的太多，而是完全丢失了表格结构。

[spreadsheet 文件](https://github.com/dannguyen/abbyy-finereader-ocr-senate/blob/master/xls/000602470.xlsx). And the [PDF with embeddable text here](https://github.com/dannguyen/abbyy-finereader-ocr-senate/blob/master/pdf/000602470.pdf).



## 简单的页面 (FineReader vs Tesseract效果对比)

常规文本的效果怎么样，[Tesseract (I'm using 3.04.01, which was released in February)](https://github.com/tesseract-ocr/tesseract), 我们比较一下.

原始的文件如下：

![000602485.gif](https://raw.githubusercontent.com/dannguyen/abbyy-finereader-ocr-senate/master/images/000602485.gif)


### FineReader + pdftotext -layout

 [FineReader's OCR的pdf文件](https://github.com/dannguyen/abbyy-finereader-ocr-senate/blob/master/pdf/000602485.pdf), 能够自动处理orientation。由于没有表格数据，直接用[Poppler's](https://poppler.freedesktop.org/) __pdftotext__ utility 带`-layout` 参数得到下面的结果：

不理想的地方只有
 `17.1226% interest`. 以及把`of` 弄成了 `o f`:

~~~
     LAMAR ALEXANDER
         TENNESSEE




                                              Mttd States Senate
                                                     WASHINGTON, DC 20510




                                                                 May 15,2012


           Dear Senators Boxer and Isakson,

                   My wife, Leslee B. Alexander, owns 17,1226% interest in her family’    s Texas
           corporation, the Starboard Corporation. Since 2003,1have reported her ownership o f this
           interest and relied on the Starboard Corporation to provide my accountants with a list o f
           underlying assets o f the corporation so that I could also list them on my annual financial report.

                   This year, while preparing my 2011 Financial disclosure, my accountants inquired o f the
           Starboard corporation whether the list o f underlying assets was up-to-date. On May 10, 2012,
           the corporation notified my accountants that one such asset had not been included on the list— a
           piece o f commercial real estate in San Antonio purchased by the Starboard Corporation in 2004.
           My accountants say that this property had not been reported to them previously.

                  This omission did not affect the accuracy o f the “
                                                                    amount or o f type o f income”from the
           Starboard Corporation reported on my annual financial disclosures between 2004 and 2010.
           What was inaccurate was failure to report ownership o f this one underlying asset o f the
           corporation, the San Antonio property.

                  In this year’s 2011 Financial Disclosure report, the San Antonio property is included
           along with ten other underlying assets o f the Starboard Corporation (all o f which have been
           previously reported) among our non-publicly traded assets and unearned income sources. It can
           be found on page 9, line 1 o f the 2011 report.

                  Looking ahead, I have talked both with my Nashville accountants and the Texas
           accountant for the Starboard Corporation and emphasized to them the importance o f reporting
           underlying assets and o f observing the new rules concerning reporting transactions within 30
           days. I do not own any publicly traded securities.

                     Should you have additional questions regarding this matter, please contact me at 202 224
            1989.


                                                                 Sim
                                                                 Sincerely,
$0
ST
iN                                                       Lamar Alexander
©
io
o
D
P
P
O

~~~




### Tesseract OCR


这里使用的Tesseract [version 3.04.01](https://github.com/tesseract-ocr/tesseract/releases/tag/3.04.01),  2016年2月发布的版本. Tesseract 不能处理 gif,所以需要通过[ImageMagick](http://www.imagemagick.org/script/index.php)转换好格式. 



 Tesseract 也没法自动实现朝向检测，需要事先手动调整好。

 [ImageMagick](http://www.imagemagick.org/script/index.php)格式转换的命令如下:

~~~sh
$ convert 000602485.gif -rotate 270 000602485.tiff
$ tesseract 000602485.tiff 000602485-tesseract
~~~


结果如下所示：


~~~
LAMAR ALEXANDER
TENNESSEE

ﬂﬂnittd 0%tatts :52an

WASHINGTON, DC 20510
May 15, 2012

Dear Senators Boxer and Isakson,

My wife, Leslee B. Alexander, owns 17.1226% interest in her family’s Texas
corporation, the Starboard Corporation. Since 2003, I have reported her ownership of this
interest and relied on the Starboard Corporation to provide my accountants with a list of
underlying assets of the corporation so that I could also list them on my annual ﬁnancial report.

This year, while preparing my 201 1 Financial disclosure, my accountants inquired of the
Starboard corporation whether the list of underlying assets was up-to-date. On May 10, 2012,
the corporation notiﬁed my accountants that one such asset had not been included on the list—a
piece of commercial real estate in San Antonio purchased by the Starboard Corporation in 2004.
My accountants say that this property had not been reported to them previously.

This omission did not affect the accuracy of the “amount or of type of income” from the
Starboard Corporation reported on my annual ﬁnancial disclosures between 2004 and 2010.
What was inaccurate was failure to report ownership of this one underlying asset of the
corporation, the San Antonio property.

In this year’s 2011 Financial Disclosure report, the San Antonio property is included
along with ten other underlying assets of the Starboard Corporation (all of which have been
previously reported) among our non-publicly traded assets and unearned income sources. It can
be found on page 9, line 1 of the 2011 report.

Looking ahead, I have talked both with my Nashville accountants and the Texas
accountant for the Starboard Corporation and emphasized to them the importance of reporting
underlying assets and of observing the new rules concerning reporting transactions within 30
days. I do not own any publicly traded securities.

Should you have additional questions regarding this matter, please contact me at 202 224
1989.

m Sincerely,
ST
N , Lamar Alexander

Ll?
CD!

(5:)
G)
~~~



# Conclusion

Turning data that was "optimized" for paper -- whether [it be digital PDFs](https://www.propublica.org/nerds/item/heart-of-nerd-darkness-why-dollars-for-docs-was-so-difficult) or [scanned images simply packaged as PDFs](https://www.propublica.org/series/free-the-files) -- will be a significant computational task as long as humans require human-readable information. And it's safe to assume that state-of-the-art OCR will never be 100% accurate.

For now, I'm pretty satisfied with the kind of performance FineReader provides at ~$100 -- and I don't blame the open-source contributors of Tesseract if they aren't able to voluntarily tackle the challenge of OCRing tabular data -- Tesseract (which is also trainable) works pretty damn well on regular text.

In terms of [OpenSecrets's call-to-arms to automate the processing of Congressional paper forms](https://github.com/pdfliberation/pdf-hackathon/blob/master/challenges/house-financial-disclosures.md#2nd-option---handwritten-reports), good OCR is not enough, we need a system to batch collect and process the documents, which will be its own writeup.

It's worth noting, though, that OpenSecrets isn't waiting around for magic OCR to come around: they've processed the financial disclosure forms the old-fashioned way -- human-powered reading and data entry -- and have generously provided their results in browsable and searchable form on the [Personal Finances](https://www.opensecrets.org/pfds/) section of their eponymous political transparency site:

https://www.opensecrets.org/pfds/


