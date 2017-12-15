# MediaInfo-Helper

A Laravel helper for parsing MediaInfo dumps.

## Install

Via Composer

``` bash
$ composer require hdvinnie/mediainfo-helper
```

## Usage

**Parsing a MediaInfo string**

Returns an array containing the parsed information.

```php
$parser = new Parser();
$parsed = $parser->parse($mediaInfo);
```

## Testing

``` bash
$ composer test
```
