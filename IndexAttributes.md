# Introduction #

In default, extension send only fulltext results to Solr. However, it is possible to send all the attributes (Warning: it will take longer to send data).

# Details #

**Step 1: Index**

Open **Magentix\_Solr\_Model\_Indexer**(app/app/code/local/Magentix/Solr/Model/Indexer.php)

Line 51 :

```
$products = $this->_getConnection()->fetchAll('SELECT * FROM '.$this->_getTable('catalogsearch/fulltext'));
```

Here we want to retrieve products Attributes. Delete and replace by:

```
$products = Mage::getResourceModel('catalog/product_collection')
            ->addAttributeToFilter('status', 1)
            ->addAttributeToFilter('visibility', array('in' => array(Mage_Catalog_Model_Product_Visibility::VISIBILITY_IN_CATALOG, Mage_Catalog_Model_Product_Visibility::VISIBILITY_BOTH)))
            ->addAttributeToSelect('name')
            ->addAttributeToSelect('store_id')
            ->addAttributeToSelect('sku')
            ->addAttributeToSelect('short_description')
            ->addAttributeToSelect('manufacturer');
```

Next we can add attributes to document (line 55):

```
foreach($products as $product) {
    $document = Mage::getModel('solr/document');
    $document->id = $product->getId();
    $document->store_id = $product->getStoreId();
    $document->name = $product->getName();
    $document->short_description = $product->getShortDescription();
    $document->sku = $product->getSku();
    $document->manufacturer = $product->getResource()->getAttribute('manufacturer')->getFrontend()->getValue($product);
 
    $documents[] = $document;
}
```

And add fields to schema.xml in Solr configuration:

```xml
<field name="id" type="string" indexed="true" stored="true" required="true" />

<field name="store_id" type="string" indexed="true" stored="true" required="true" />

<field name="name" type="text" indexed="true" stored="true" />

<field name="manufacturer" type="text" indexed="true" stored="true" />

<field name="sku" type="text" indexed="true" stored="true" />

<field name="short_description" type="text" indexed="true" stored="true" />
```

**Step 2: Query**

Edit **Magentix\_Solr\_Model\_Search** (app/code/local/Magentix/Solr/Model/Search.php)

We can change query, example:

```
public function loadQuery($query,$storeId=0,$limit=10) {
    if(!$this->_response && $query) {
        $params = array(
            'fl' => 'id,score',
            'fq' => 'store_id:'.$storeId,
        );

        $query = 'name:'.$query.'^10 OR short_description:'.$query.'^5 OR manufacturer:'.$query.'^5 OR sku:'.$query;

        $response = $this->search($query,0,$limit,$params,'POST');
        $this->setResponse($response->response);
    }
        
    return $this;
}
```