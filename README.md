# Recursive Pagination

[![Latest Version](https://img.shields.io/packagist/v/xparse/recursive-pagination.svg?style=flat-square)](https://packagist.org/packages/xparse/recursive-pagination)
[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](LICENSE.md)
[![Build Status](https://img.shields.io/travis/xparse/RecursivePagination/master.svg?style=flat-square)](https://travis-ci.org/xparse/RecursivePagination)
[![Coverage Status](https://img.shields.io/scrutinizer/coverage/g/xparse/RecursivePagination.svg?style=flat-square)](https://scrutinizer-ci.com/g/xparse/RecursivePagination/code-structure)
[![Quality Score](https://img.shields.io/scrutinizer/g/xparse/RecursivePagination.svg?style=flat-square)](https://scrutinizer-ci.com/g/xparse/RecursivePagination)
[![Total Downloads](https://img.shields.io/packagist/dt/xparse/recursive-pagination.svg?style=flat-square)](https://packagist.org/packages/xparse/recursive-pagination)

Recursive Pagination allows you to parse website pages recursively. 
You need to pass link from where to start and set next page expression (xPath, css, etc). 

## Installation

You can install the package via Composer

``` bash
$ composer require xparse/recursive-pagination
```

## Basic Usage

Try to find all links to the repositories on github. Our query will be `xparse`.
With recursive pagination we can traverse all pagination links and process each resulting page to fetch repositories links.  

```php
  use Xparse\Parser\Parser;
  use Xparse\RecursivePagination\RecursivePagination;
  
  # init Parser
  $parser = new Parser();

  # set expression to pagination links
  $paginator = new RecursivePagination($parser, "//*[@class='pagination']//a/@href");
  # set initial page url
  $paginator->addToQueue('https://github.com/search?q=xparse');

  $allLinks = [];
  while (($page = $paginator->getNextPage())) {
    # set expression to fetch repository links
    $adsList = $page->value("//*[@class='repo-list-name']//a/@href")->getItems();
    # merge and remove duplicates
    $allLinks = array_values(array_unique(array_merge($allLinks, $adsList)));
  }
  print_r($allLinks);
```

## Testing

``` bash
$ vendor/bin/phpunit
```

## Credits

- [Ganchev Anatolii](https://github.com/ganchclub)
- [All Contributors](https://github.com/xparse/RecursivePagination/graphs/contributors)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.