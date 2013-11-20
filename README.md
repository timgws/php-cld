# PHP Bindings for Chromium Compact Language Detector (CLD)
[![Build Status](https://secure.travis-ci.org/lstrojny/php-cld.png)](http://travis-ci.org/lstrojny/php-cld)

This small extension provides bindings to use the Chromium Compact Language Detector
(http://code.google.com/p/chromium-compact-language-detector/) in PHP.


## Installation

 1. Chcek out cld2 (Compact Language Detector 2) with `svn checkout http://cld2.googlecode.com/svn/trunk/ cld2`
 2. `cd cld2/internal`
 3. run `chmod +x ./compile_libs.sh` followed by `./compile_libs.sh`
 4. `cd ../../`
 5. Checkout Chromium Language Detector with `hg clone
    https://code.google.com/p/chromium-compact-language-detector`
 6. `cd chromium-compact-language-detector`
 7. Edit both setup.py and setup_full.py: edit CLD2_PATH to point to where you checked out the CLD2 sources.
 8. Run `./build.sh`
 9. Checkout this project
 10. Run `phpize && ./configure --with-libcld-dir=... && make && sudo make install`
 11. Add `extension=cld.so` to your `php.ini`

## Usage

### Procedural API
```php
<?php
var_export(CLD\detect("Drüben hinterm Dorfe wohnt ein Leiermann. Und mit starren Fingern spielt er was er kann"));
var_export(CLD\detect("日[の]本([の]国", false, true, null, CLD\Language::JAPANESE, CLD\Encoding::JAPANESE_EUC_JP));
```

### Object-oriented API

```php
<?php
$detector = new CLD\Detector();
var_export($detector->detectLanguage('Drüben hinterm Dorfe wohnt ein Leiermann. Und mit starren Fingern spielt er was er kann'));

$detector->setLanguageHint(CLD\Language::JAPANESE);
$detector->setEncodingHint(CLD\Encoding::JAPANESE_EUC_JP);
$detector->detectLanguage("日[の]本([の]国", false);
```

will return

```text
array (
  0 =>
  array (
    'name' => 'GERMAN',
    'code' => 'de',
    'reliable' => true,
    'bytes' => 90,
  ),
)
array (
  0 =>
  array (
    'name' => 'JAPANESE',
    'code' => 'ja',
    'reliable' => true,
    'bytes' => 22,
  ),
  1 =>
  array (
    'name' => 'CHINESE',
    'code' => 'zh',
    'reliable' => true,
    'bytes' => 22,
  ),
)
```
