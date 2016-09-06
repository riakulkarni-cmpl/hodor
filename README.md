# Hodor

A simple html scraper with xpath or css.

## Usage

### As python package

***WARNING: This package by default doesn't very ssl connections. Please check options to enable them.***


```
from hodor import Hodor

url = 'http://www.nasdaq.com/markets/stocks/symbol-change-history.aspx'

CONFIG = {
    'old_symbol': {
        'css': '#SymbolChangeList_table tr td:nth-child(1)',
        'many': True
    },
    'new_symbol': {
        'css': '#SymbolChangeList_table tr td:nth-child(2)',
        'many': True
    },
    'effective_date': {
        'css': '#SymbolChangeList_table tr td:nth-child(3)',
        'many': True
    },
    '_groups': {
        'data': ['old_symbol', 'new_symbol', 'effective_date']
    }
    '_paginate_by': {
        'xpath': '//*[@id="two_column_main_content_lb_NextPage"]/@href',
        'many': False
    }
}

h = Hodor(url=url, config=CONFIG, pagination_max_limit=5)

h.data
```

It also takes arguments:

- ```ua``` (User-Agent)
- ```proxies``` (check requesocks)
- ```auth```
- ```crawl_delay``` (crawl delay in seconds across pagination - default 3 seconds)
- ```pagination_max_limit``` (max number of pages to crawl - default 100)
- ```ssl_verify``` (default - False)


Config parameters:
- By default any key in the config is a rule to parse.
- Extra parameters include grouping (_groups) and pagination (_paginate_by) which is also of the rule format.



### As tornado service

#### Server
```
docker-compose up
```

### Client
```
curl -X POST -F "url=https://www.compile.com/" -F "config={\"src\": {\"css\": \"strong\", \"many\":false}, \"width\": {\"xpath\": \"/html/body/div[1]/nav/div/div[1]/a/img/@width\", \"many\":false}}" "http://localhost:8888"
```


## Result
```
{'src': '/img/compile-logo-white.1002b288.svg', 'width': ['100px']}
```
