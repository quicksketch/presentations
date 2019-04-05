- Porting a module to Backdrop!
- It's usually easy (because that's the whole point of Backdrop)

- Simple GMap Module
  - https://drupal.org/project/simple_gmap
  - https://backdropcms.org/project/simple_gmap

- Clone repository from Drupal.org

- Update .info file.
  - Add: backdrop = 1.x
  - Add: type = module
  - Remove: core = 7.x
  - Remove: files[] = whatever.inc

- Heck, give it a whirl!

- Update Drupal-namespaced functions:
  - Replace "drupal_" with "backdrop_" (relevant for PHP)
  - Replace "Drupal" with "Backdrop" (relevant for JS)

- Convert Variables to Configuration
  - Add a hook_config_info()

function mymodule_config_info() {
  $prefixes['mymodule.settings'] = array(
    'label' => t('My module settings'),
    'group' => t('Configuration'),
  );
  return $prefixes;
}

  - Replace variable_get('my_module_setting_name', 'default')
  - With config_get('my_module.settings', 'setting_name')

  - Create a default configuration file at my_module/config/my_module.settings.json

- Create an upgrade path
  - Add hook_update_1000() to your module .install file
  - Pull in variables and replace with configuration:

function my_module_update_1000() {
  // Migrate variables to config.
  $config = config('book.settings');
  $config->set('book_allowed_types', update_variable_get('book_allowed_types', array('book')));
  $config->set('book_child_type', update_variable_get('book_child_type', 'book'));
  $config->set('book_block_mode', update_variable_get('book_block_mode', 'all pages'));
  $config->save();

  // Delete variables.
  update_variable_del('book_allowed_types');
  update_variable_del('book_child_type');
  update_variable_del('book_block_mode');
}

- Updating entities:

$node = new stdClass();
$node->type = 'blog';
$node->title = 'New Title';
...
node_save($node);

$node = new Node(array('type' => 'blog'));
$node->uid = $user->uid;
$node->title = 'New Title';
$node->save();

- system_settings_form() removed

