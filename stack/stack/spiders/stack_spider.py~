from scrapy import Spider
from scrapy.selector import Selector
import scrapy
from stack.items import StackItem
from scrapy import Selector
from scrapy.selector import HtmlXPathSelector

class StackSpider(Spider):
    name = "stack"
    allowed_domains = ["http://www.joybynature.com/"]
    start_urls = [
        "http://www.joybynature.com",
    ]

    def parse(self, response):
        urls = Selector(response).xpath('//ul[@class="site-nav"]//li[@class="dropdown mega-menu"]')
        #inners= Selector(response).xpath('//div[@class="inner"]')
        #titles = Selector(response).xpath('//div[@class="product-bottom"]')
        #prices = Selector(response).xpath('//div[@class="price-box"]')
        #reviews = Selector(response).xpath('/span[@class="spr-badge"]')
        #print urls
        for url in urls:          
            item = StackItem()
            item['dropdown_url'] = url.xpath(
                './/a/@href').extract()[0]
            item['dropdown_name'] = url.xpath(
                './/a/span/text()').extract()[0]
            inners = url.xpath(
                './/div[@class="col-1 parent-mega-menu"]').extract()[0]
            sel = Selector(text=inners, type="html")
            for scope in sel.xpath('//div[@class="inner"]'):
                item['page_brand_name']=scope.xpath('//div[@class="inner"]/a/span/text()').extract()
                item['page_brand_url'] = scope.xpath('//div[@class="inner"]/a/@href').extract()
                item['dropdown_li_name']=scope.xpath('//div[@class="inner"]/ul[@class="dropdown"]/li/a/span/text()').extract()
                item['dropdown_li_url']=scope.xpath('//div[@class="inner"]/ul[@class="dropdown"]/li/a/@href').extract()
                for url in item['dropdown_li_url']:
                    yield scrapy.Request(response.urljoin(url), self.parse_titles)

                #item['page_brand_url']=scope.xpath('//a/@href').extract()
            '''for a in inners:
                
                
                sel = Selector(text=a ,type="html")
                #print item['page_brand_name']
                item['page_brand_name']=sel.xpath('.//div[@class="col-1 parent-mega-menu"]//div[@class="inner"]/a/span/text()').extract()[0]'''
            #item['dropdown_li_url']=url.xpath('//div[@class="inner"]/ul[@class="dropdown"]/li/a/@href').extract()
            #item['dropdown_li_name']=url.xpath('//div[@class="inner"]/ul[@class="dropdown"]/li/a/span/text()').extract()
            '''print li_items
               for li in li_items:
                item['dropdown_li_url']=li.xpath(
                '/li/a/@href').extract()[0]'''
            #item['dropdown_li_name']=url.xpath('//div[@class="inner"]/ul[@class="dropdown"]/li//span/text()').extract()[0]
            #item['dropdown_li_url']=url.xpath('//div[@class="inner"]/ul[@class="dropdown"]/li//@href').extract()[0]'''

            #print dropdowns
            '''for dropdown in dropdowns:
                item['dropdown_li_url'] = url.xpath(
                '//li//@href').extract()             
                item['dropdown_li_name'] = url.xpath(
                '//li//span/text()').extract()
            #item['image'] = url.xpath(
                #'//div[@class="product-image"]//img/@src').extract()[0]'''


            yield item
       
    def parse_titles(self, response):     
            item=StackItem()
            item['title']=response.xpath('a[@class="product-title"]/text()').extract()[0]
            item['price']=response.xpath('//div[@class="price-box"]/p/span/text()').extract()[0]
            #item['reviews']=title.xpath('span/span/text()').extract()
            #item['review']=title.xpath('//span[@class="spr-badge"]//span[@class="spr-badge-caption"]//text')[0].strip()
            

            yield item

