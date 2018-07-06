# Spider
# 爬取百度贴吧


import urllib.request
import urllib.parse
import os


def main():
    kw = input("请输入您要爬取的贴吧名称：")
    start_page = int(input("请输入爬取的起始页码："))
    end_page = int(input("请输入爬取的结束页码："))
    url = "http://tieba.baidu.com/f?ie=utf-8&"

    for page in range(start_page, end_page + 1):
        request = handle_request(url, page, kw)
        print("正在下载%s页。。。。。。" % page)
        download(request, kw, page)
        print("结束下载%s页" % page)


def download(request, kw, page):
    response = urllib.request.urlopen(request)
    if not os.path.exists(kw):
        os.mkdir(kw)
    filename = "第%s页.html" % page
    filepath = os.path.join(kw, filename)
    with open(filepath, "wb")as fp:
        fp.write(response.read())


def handle_request(url, page, kw):
    pn = (page - 1) * 50
    data = {
        "kw": kw,
        "pn": pn,
    }
    url += urllib.parse.urlencode(data)
    # print(url)
    headers = {
        'User-Agent': 'Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10_6_8; en-us) AppleWebKit/534.50 (KHTML, like Gecko) Version/5.1 Safari/534.50',
    }
    request = urllib.request.Request(url=url, headers=headers)
    return request


if __name__ == '__main__':
    main()
