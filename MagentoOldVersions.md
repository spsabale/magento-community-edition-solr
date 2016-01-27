# Introduction #

Magento <= 1.5

# Details #

## Fulltext Directory (<= 1.5) ##

On Magento 1.6, the class Fulltext changed. The new class is Mage\_CatalogSearch\_Model\_Resource\_Fulltext. For previous versions it is necessary to change.

**Step 1**

app/code/local/Magentix/Solr/Model/CatalogSearch/**Resource**/Fulltext.php

Rename _"Resource"_ folder for _"Mysql4"_ :

app/code/local/Magentix/Solr/Model/CatalogSearch/**Mysql4**/Fulltext.php


**Step 2**

Edit "Fulltext.php" :

`class Magentix_Solr_Model_CatalogSearch_Resource_Fulltext extends Mage_CatalogSearch_Model_Resource_Fulltext`

Become :

`class Magentix_Solr_Model_CatalogSearch_Mysql4_Fulltext extends Mage_CatalogSearch_Model_Mysql4_Fulltext`


**Step 3**

Edit _app/code/local/Magentix/Solr/etc/config.xml_

```xml
<catalogsearch_resource>

<rewrite>

<fulltext>Magentix_Solr_Model_CatalogSearch_Resource_Fulltext

Unknown end tag for &lt;/fulltext&gt;





Unknown end tag for &lt;/rewrite&gt;





Unknown end tag for </catalogsearch\_resource>



```

Become :

```xml
<catalogsearch_mysql4>

<rewrite>

<fulltext>Magentix_Solr_Model_CatalogSearch_Mysql4_Fulltext

Unknown end tag for &lt;/fulltext&gt;





Unknown end tag for &lt;/rewrite&gt;





Unknown end tag for </catalogsearch\_mysql4>



```


## Adapter insertMultiple (< 1.4) ##

In versions prior to 1.4, the method **insertMultiple** does not exist (Varien\_Db\_Adapter\_Pdo\_Mysql).

Edit :

_app/code/local/Magentix/Solr/Model/CatalogSearch/Mysql4/Fulltext.php_

```
if($search->count()) {
    $products = $search->getProducts();

    $data = array();
    foreach($products as $product) {
        $data[] = array('query_id'   => $query->getId(),
                        'product_id' => $product['id'],
                        'relevance'  => $product['relevance']);
    }

    $adapter->insertMultiple($this->getTable('catalogsearch/result'),$data);
}
```

Become :

```
if($search->count()) {
    $products = $search->getProducts();

    $sql = 'INSERT INTO '.$this->getTable('catalogsearch/result').' (query_id,product_id,relevance) VALUES ';

    foreach($products as $product) {
        $sql .= '("'.$query->getId().'","'.$product['id'].'","'.$product['relevance'].'"),';
    }
    $sql = trim($sql,',');

    $adapter->query($sql, array());
}
```