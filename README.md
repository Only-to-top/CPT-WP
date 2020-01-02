# new-post_type-WP

<h2>Создание нового типа записи а также таксономии (типо рубрики)</h2>

```php
// New post type - name
add_action( 'init', 'register_post_types_name' );
function register_post_types_name(){
    register_post_type('postTypeName', array(
        'label'  => null,
        'labels' => array(
            'name'               => '______', // основное название для типа записи
            'singular_name'      => '______', // название для одной записи этого типа
            'add_new'            => 'Добавить ______', // для добавления новой записи
            'add_new_item'       => 'Добавление ______', // заголовка у вновь создаваемой записи в админ-панели.
            'edit_item'          => 'Редактирование ______', // для редактирования типа записи
            'new_item'           => 'Новая ______', // текст новой записи
            'view_item'          => 'Смотреть ______', // для просмотра записи этого типа.
            'search_items'       => 'Искать ______', // для поиска по этим типам записи
            'not_found'          => 'Не найдено', // если в результате поиска ничего не было найдено
            'not_found_in_trash' => 'Не найдено в корзине', // если не было найдено в корзине
            'parent_item_colon'  => '', // для родителей (у древовидных типов)
            'menu_name'          => '______', // название меню
        ),
        'description'         => '',
        'public'              => true,
        'show_in_menu'        => null, // показывать ли в меню адмнки
        'show_in_rest'        => null, // добавить в REST API. C WP 4.7
        'rest_base'           => null, // $post_type. C WP 4.7
        'menu_position'       => null,
        'menu_icon'           => 'dashicons-image-filter',
        'hierarchical'        => false,
        'supports'            => [ 'title', 'editor', 'thumbnail' ], // 'title','editor','author','thumbnail','excerpt','trackbacks','custom-fields','comments','revisions','page-attributes','post-formats'
        'taxonomies' => [],
        // 'taxonomies' => [
        //     'post_tag', // метки
        //     'category'  // рубрики
        // ],
        'has_archive'         => false,
        'rewrite'             => true,
        'query_var'           => true,
    ) );
}

// Создаем новую таксономию для ______
add_action( 'init', '_______taxonomies', 0 );
function _______taxonomies(){
    $labels = array(
        'name' => 'Категории ______',
        'singular_name' => 'Категория ______',
        'search_items' =>  'Найти категорию ______',
        'all_items' => 'Все категории ______',
        'parent_item' => 'Родительская категория ______',
        'parent_item_colon' => 'Родительская категория',
        'edit_item' => 'Родительская категория',
        'update_item' => 'Обновить категорию',
        'add_new_item' => 'Добавить новую категорию',
        'new_item_name' => 'Название новой категории ______',
        'menu_name' => 'Категории',
    );
    register_taxonomy('accessories', array('______'), array( // array = post_type name (см. выше) - taxonomy 'accessories'
        'hierarchical' => true,
        'labels' => $labels,
        'show_ui' => true,
        'query_var' => true,
        'rewrite' => array( 'slug' => '______' ),
    ));
}
```
