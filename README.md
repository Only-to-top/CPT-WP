# new-post_type-WP

<h2>Создание нового типа записи а также таксономии (типо рубрики)</h2>

```php
// New post type - Услуги
add_action( 'init', 'register_post_type_services' );
function register_post_type_services(){
    register_post_type('services', array(
        'label'  => null,
        'labels' => [
            'name'               => 'Услуги', // основное название для типа записи
            'singular_name'      => 'Услуга', // название для одной записи этого типа
            'add_new'            => 'Добавить Услугу', // для добавления новой записи
            'add_new_item'       => 'Добавление Услуги', // заголовка у вновь создаваемой записи в админ-панели.
            'edit_item'          => 'Редактирование Услуги', // для редактирования типа записи
            'new_item'           => 'Новая Услуга', // текст новой записи
            'view_item'          => 'Смотреть Услуги', // для просмотра записи этого типа.
            'search_items'       => 'Искать Услуги', // для поиска по этим типам записи
            'not_found'          => 'Не найдено', // если в результате поиска ничего не было найдено
            'not_found_in_trash' => 'Не найдено в корзине', // если не было найдено в корзине
            'menu_name'          => 'Услуги', // название меню
        ],
        'description'         => 'Услуги',
        'exclude_from_search' => true,
        'public'              => true,
        // 'capability_type'     => 'page',
        'hierarchical' => true,
        'show_in_menu'        => null, // показывать ли в меню адмнки
        'show_in_rest'        => null, // добавить в REST API. C WP 4.7
        'rest_base'           => null, // $post_type. C WP 4.7
        'menu_position'       => null,
        'menu_icon'           => 'dashicons-clipboard',
        'supports'            => [
            'title',
            'editor',
            //'excerpt',
            //'trackbacks',
            //'custom-fields',
            //'comments',
            // 'revisions',
            // 'thumbnail',
            // 'author',
            'page-attributes', # атрибуты страницы
        ],
        'has_archive'         => false,
        'rewrite'             => true,
        'query_var'           => true,
    ) );
}
add_action( 'init', 'services_taxonomies', 0 ); # Создаем новую таксономию для Услуг
function services_taxonomies(){
    $labels = array(
        'name'              => 'Категории Услуг',
        'singular_name'     => 'Категория услуги',
        'search_items'      => 'Найти категорию услуг',
        'all_items'         => 'Все категории услуг',
        'parent_item'       => 'Родительская категория услуги',
        'parent_item_colon' => 'Родительская категория',
        'edit_item'         => 'Родительская категория',
        'update_item'       => 'Обновить категорию',
        'add_new_item'      => 'Добавить новую категорию',
        'new_item_name'     => 'Название новой категории услуги',
        'menu_name'         => 'Категории',
    );
    register_taxonomy('services_categories', 'services', array(
        'hierarchical' => true,
        'labels' => $labels,
        'show_ui' => true,
        'query_var' => true,
        'rewrite' => [
            'slug' => 'services',
            'hierarchical' => true
        ],
        'public' => true
    ));
}
```
