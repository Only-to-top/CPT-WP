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
            'add_new'            => 'Добавить', // для добавления новой записи
            'add_new_item'       => 'Добавление', // заголовка у вновь создаваемой записи в админ-панели.
            'edit_item'          => 'Редактирование', // для редактирования типа записи
            'new_item'           => 'Новая', // текст новой записи
            'view_item'          => 'Смотреть', // для просмотра записи этого типа (ед. число).
            'search_items'       => 'Искать', // для поиска по этим типам записи
            'not_found'          => 'Не найдено', // если в результате поиска ничего не было найдено
            'not_found_in_trash' => 'Не найдено в корзине', // если не было найдено в корзине
            'menu_name'          => 'Услуги', // название меню
        ],
        'description'            => '',
        'public'                 => true,
        // 'capability_type'     => 'page', // default 'post'
        // 'hierarchical'        => true, // будут ли записи этого типа иметь древовидную структуру (как постоянные страницы), default 'false'
        'menu_icon'              => 'dashicons-clipboard',
        'supports'               => [
            'title',
            'editor',
            // 'excerpt',
            // 'trackbacks',
            // 'custom-fields',
            // 'comments',
            // 'revisions',
            // 'thumbnail',
            // 'author',
            // 'page-attributes', # атрибуты страницы
        ],
        'has_archive'           => false,
        'rewrite'               => true,
        'query_var'             => true,
    ) );
}
add_action( 'init', 'services_taxonomies', 0 ); # Создаем новую таксономию для Услуг
function services_taxonomies(){
    $labels = array(
        'name'              => 'Категории',
        'singular_name'     => 'Категория',
        'search_items'      => 'Найти категорию',
        'all_items'         => 'Все категории',
        'parent_item'       => 'Родительская категория',
        'parent_item_colon' => 'Родительская категория',
        'edit_item'         => 'Родительская категория',
        'update_item'       => 'Обновить категорию',
        'add_new_item'      => 'Добавить новую категорию',
        'new_item_name'     => 'Название новой категории',
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
        'public' => true,
        'show_admin_column' => 'true', // авто-создание колонки таксы в таблице
    ));
}
```
