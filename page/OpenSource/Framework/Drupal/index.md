---
layout: page 
title: Drupal
---

###资料
-[Boost模块-用drupal来建大网站的得力助手](http://drupalgarden.cn/content/boostmo-kuai-yong-drupallai-jian-da-wang-zhan-de-de-li-zhu-shou)



###drupal7 transaction

	// The transaction opens here.
	  $txn = db_transaction();
	
	  try {
	    $id = db_insert('example')
	      ->fields(array(
	        'field1' => 'mystring',
	        'field2' => 5,
	      ))
	      ->execute();
	
	    my_other_function($id);
	
	    return $id;
	  }
	  catch (Exception $e) {
	    // Something went wrong somewhere, so roll back now.
	    $txn->rollback();
	    // Log the exception to watchdog.
	    watchdog_exception('type', $e);
	  }


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
	  
	$result = db_select('node', 'n')
	    ->fields('n')
	    ->condition('nid', $node->nid,'=')
	    ->condition('status', 0,'>')
	    ->condition('uid', array(1,5,7),'IN')
	    ->execute()
	    ->fetchAssoc();
	    
	$query = db_select('node', 'n');
    $query->join('users', 'u', 'n.uid = u.uid'); //JOIN node with users
    $query->groupBy('u.uid');//GROUP BY user ID
    $query->fields('n',array('title','created'))//SELECT the fields from node
    ->fields('u',array('name'))//SELECT the fields from user
    ->orderBy('created', 'DESC')//ORDER BY created
    ->range(0,2);//LIMIT to 2 records
    $result = $query->execute();
    while($record = $result->fetchAssoc()) {
        print_r($record);
    }

