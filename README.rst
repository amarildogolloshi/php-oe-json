===========
php-oe-json
===========

Modest PHP class to manage OpenERP Json query.


Installation
------------

This is a very thin layer above Tivoka_, a JSON-RPC PHP lib.
You will need to download Tivoka_, and make it available in
your PHP dependencies (e.g. by copying the `tivoka` folder
in the `php-oe-json` folder)

Code maturity
-------------

Early working alpha. Comments welcome.


Features
--------

- Simple way to add new json-rpc entry points
- Quite easy to get started (see Usage section)
- Ability to resume an open session (and thus login in openerp) with a
  saved ``session_id`` and HTTP ``cookie_id`` (without ``$login`` and
  ``$password``)


Usage
-----

sample PHP code::

  <?php

  require_once 'openerp.php';

  
  // Instanciate with $url, and $dbname
  $oe = new OpenERP("http://localhost:8069", "test_json");

  // login with $url and $dbname
  $oe->login("admin", "xxxxxx");

  echo "Logged in (session id: " . $oe->session_id . ")";

  // Query with direct object method which are mapped to json-rpc calls
  $partners = $oe->search_read(array(
    'model' => 'res.partner',
    'fields' => array('name', 'city'),
  ));

  echo "<ul>";
  foreach($partners['records'] as $partner) {
     echo "    <li>" . $partner["name"] . " - " . $partner["city"] . "</li>\n";
  }
  echo "</ul>";

  // Instanciate a particular Model proxy
  $partners = new OpenERPModel($oe, 'res.partner');

  // Create a new partner
  $partner_id = $partners->create(array(array(
    'name' => 'John Doe',
    'city' => 'Brussels',
    // etc..
  )));
  echo "<h1>New partner created with ID $partner_id</h1>";


  // Error Handling example - This will FAIL as we're creating a partner without name
  try {
      $partner_id = $partners->create(array(array(
        'name' => null,
      )));
  } catch (OpenERPException $e) {
      echo '<pre>Caught error ' . $e->code . ' ' . $e->message . '</pre>';
  }


  ?>




.. _Tivoka: https://github.com/marcelklehr/tivoka
