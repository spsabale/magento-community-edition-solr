<strong>Overview :</strong>

Make search on <strong>Magento CE</strong> with <strong>Apache Solr</strong>.

Based on PHP library for Apache Solr <a href='http://code.google.com/p/solr-php-client/'>solr-php-client</a>

<strong>Informations :</strong>

- Extension use fulltext for index. You can search in any attributes (Use in Quick Search).

- If Apache Tomcat server is down, usual search is used. It is invisible for user.

- Search relevance is stored (Magento do not use relevance with MySQL fulltext).

- Extension do not support catalog advanced search. Usual search is used for this feature.

- For Magento <= 1.5, please read <a href='http://code.google.com/p/magento-community-edition-solr/wiki/MagentoOldVersions'>Older Versions</a> first.

<strong>Screenshots :</strong>

<img src='http://www.magentix.fr/assets/images/magento-solr/solr-1.1.0.png' alt='Magento CE Solr configuration' />

<img src='http://www.magentix.fr/assets/images/magento-solr/solr-search.jpg' alt='Magento CE Solr search' />

<strong>Roadmap</strong>

- Connect to Solr Server protected by password

- Search Suggestions

- Search Recommandations

- Test Connection

<strong>Links :</strong>

- <a href='http://lucene.apache.org/solr/'>Solr Website</a>

- <a href='http://www.magentocommerce.com/'>Magento Website</a>

- <a href='https://github.com/magentix/Solr'>Magento CE Solr on GitHub</a>