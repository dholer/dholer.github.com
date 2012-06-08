---
layout: page 
title: Drupal
---

###drupal db options

	ini_set('display_errors','1'); 
	ini_set('display_startup_errors','1'); 
	error_reporting (E_ALL);
	
	define('DRUPAL_ROOT', "/Users/logic/Sites/JLMWholesale");
	
	require_once '../../includes/bootstrap.inc';
	
	drupal_bootstrap(DRUPAL_BOOTSTRAP_DATABASE);
	
	$result = db_query("SELECT * FROM {taxonomy_term_data}");
	
	foreach ($result as $record) {
		#print_r($record);
	}
	
	$id = db_insert('mytable')
	  ->fields(array(
	    'intvar' => 5,
	    'stringvar' => 'hello world',
	    'floatvar' => 3.14,
	  ))
	  ->execute();
	
	
	db_update('node')
	  ->fields(array('title' => 'hello world', 'status' => 1))
	  ->condition('uid', 5)
	  ->execute();
	  
	db_delete('node')
	  ->condition('uid', 5)
	  ->condition('created', REQUEST_TIME - 3600, '<')
	  ->execute();

