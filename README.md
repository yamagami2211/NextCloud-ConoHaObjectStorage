# なにこれ
NextCloudの外部ストレージ連携で、ConoHaオブジェクトストレージを使えるようにPHPソースを書き換えたものをおいています。

[NextCloudの外部ストレージとしてConoHaのオブジェクトストレージを使う](https://qiita.com/yamagami2211/items/e3bba1b3df0376a4d466)

**他のswift系のストレージを外部ストレージ連携で使う場合は、この書き換えだと問題が出る可能性があります。  **

`lib/private/Files/ObjectStore` の `SwiftFactory.php` を編集します。
```php
//'catalogName' => 'swift',
  'catalogName' => 'Object Storage Service',
```

これで動きますが、一日の一番最初にサイトにアクセスしたときにエラーが出るので、そちらも修正します。  
`3rdparty/php-opencloud/openstack/src` の `OpenStack.php` を編集します。
```php
    /**
     * Creates a new Object Store v1 service.
     *
     * @param array $options options that will be used in configuring the service
     */
    /*public function objectStoreV1(array $options = []): \OpenStack\ObjectStore\v1\Service
    {
        $defaults = ['catalogName' => 'swift', 'catalogType' => 'object-store'];

        return $this->builder->createService('ObjectStore\\v1', array_merge($defaults, $options));
    }*/
    public function objectStoreV1(array $options = []): \OpenStack\ObjectStore\v1\Service
    {
        $defaults = ['catalogName' => 'Object Storage Service', 'catalogType' => 'object-store'];

        return $this->builder->createService('ObjectStore\\v1', array_merge($defaults, $options));
    }
```

多分問題なく動くと思います。書き換えが面倒な場合は、ダウンロードしてその場所に上書き保存してください。

なおNextCloudのアップデート毎に書き換えを行う必要があります。  
NectCloud 20.0.8 (20.0.7) で動作の確認をしています。
