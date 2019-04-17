<p align="center">
    <a href="https://github.com/yiisoft" target="_blank">
        <img src="https://avatars0.githubusercontent.com/u/993323" height="100px">
    </a>
    <h1 align="center">ActiveRecord Collection Extension for Yii 2</h1>
    <br>
</p>

This extension provides a generic data collection as well as a collection for the ActiveRecord DB layer of Yii 2.

**Development is currently in experimental state. It is not ready for production use and may change significantly.**

For license information check the [LICENSE](LICENSE.md)-file.

Documentation is at [docs/guide/README.md](docs/guide/README.md).

[![License](https://img.shields.io/github/license/laxity7/yii2-collection.svg)](https://github.com/laxity7/yii2-collection/blob/master/LICENSE)
[![Latest Stable Version](https://img.shields.io/packagist/v/laxity7/yii2-collection.svg)](https://packagist.org/packages/laxity7/yii2-collection)
[![Total Downloads](https://img.shields.io/packagist/dt/laxity7/yii2-collection.svg)](https://packagist.org/packages/laxity7/yii2-collection)

Installation
------------

The preferred way to install this extension is through [composer](http://getcomposer.org/download/).

Either run

```
php composer.phar require --prefer-dist laxity7/yii2-collection
```

or add

```json
"laxity7/yii2-collection": "~1.0.0"
```

to the require section of your composer.json.

##Basic usage

Collection is a container for a set of items.

It provides methods for transforming and filtering the items as well as sorting methods, which can be applied
using a chained interface. All these operations will return a new collection containing the modified data
keeping the original collection as it was as long as containing objects state is not changed.

```php
$collection = new Collection([1, 2, 3]);
echo $collection->map(function($i) { // [2, 3, 4]
    return $i + 1;
})->filter(function($i) { // [2, 3]
    return $i < 4;
})->sum(); // 5
```

The collection implements [[ArrayAccess]], [[Iterator]], and [[Countable]], so you can access it in
the same way you use a PHP array. A collection however is read-only, you can not manipulate single items.

```php
$collection = new Collection([1, 2, 3]);
echo $collection[1]; // 2
foreach($collection as $item) {
    echo $item . ' ';
} // will print 1 2 3
```

Configuration for ActiveRecord
-------------

To use this extension, you have to attach the `yii\collection\CollectionBehavior` to the `ActiveQuery` instance of
your `ActiveRecord` classes by overriding the `find()` method:

```php
/**
 * {@inheritdoc}
 * @return \yii\db\ActiveQuery|\yii\collection\CollectionBehavior
 */
public static function find()
{
    $query = parent::find();
    $query->attachBehavior('collection', \yii\collection\CollectionBehavior::class);
    return $query;
}
```